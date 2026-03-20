<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>Tirupati Assembly Constituency – Online Voting System 2024</title>
<link href="https://fonts.googleapis.com/css2?family=Mukta:wght@400;600;700;800&family=Playfair+Display:wght@700;900&display=swap" rel="stylesheet"/>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.3/dist/chart.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/crypto-js@4.2.0/crypto-js.js"></script>
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.3/dist/confetti.browser.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --sf:#FF9933;--sf2:#e67e00;
  --gr:#138808;--gr2:#0d6b05;
  --nv:#003366;--nv2:#004080;
  --bg:#eef2f7;--card:#fff;
  --txt:#1a1a2e;--muted:#6b7280;
  --bdr:#dde3ec;
  --shd:0 4px 22px rgba(0,51,102,.10);
  --rad:14px;
}
body{font-family:'Mukta',sans-serif;background:var(--bg);color:var(--txt);min-height:100vh;transition:background .3s,color .3s}
body.dark{--bg:#0f172a;--card:#1e293b;--txt:#e2e8f0;--muted:#94a3b8;--bdr:#334155;--shd:0 4px 22px rgba(0,0,0,.4)}
body.dark .form-control{background:#263349;color:#e2e8f0;border-color:#475569}
body.dark .sec-title{color:var(--sf)}
body.dark .stat-val{color:var(--sf)}
body.dark .bar-bg{background:#334155}
body.dark .tbl tr:hover td{background:rgba(255,153,51,.06)}
body.dark .cand-card{background:#1e293b;border-color:#334155}
body.dark .stat-box{background:#1e293b;border-color:#334155}
body.dark .ldr-row{background:#1e293b;border-color:#334155}
body.dark .sym-wrap{background:#263349;border-color:#475569}

.tricolor{height:7px;background:linear-gradient(to right,var(--sf) 33.33%,#fff 33.33% 66.66%,var(--gr) 66.66%)}

.hdr{background:linear-gradient(135deg,#001f4d,var(--nv));padding:11px 22px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:10px}
.hdr-left{display:flex;align-items:center;gap:13px}
.hdr-logo{font-size:2.6rem;filter:drop-shadow(0 2px 4px rgba(0,0,0,.3))}
.hdr-t1{font-family:'Playfair Display',serif;color:var(--sf);font-size:1.05rem;font-weight:900;line-height:1.2}
.hdr-t2{color:#aac4e8;font-size:.73rem;margin-top:2px}
.hdr-t3{color:#f0c040;font-size:.8rem;margin-top:1px}
.hdr-right{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
.btn-sm{border:none;border-radius:20px;padding:5px 13px;font-size:.77rem;font-weight:700;cursor:pointer;font-family:'Mukta',sans-serif;transition:background .2s}
.btn-theme{background:rgba(255,255,255,.14);color:#fff;border:1px solid rgba(255,255,255,.24)}
.btn-theme:hover{background:rgba(255,255,255,.26)}
.btn-lang{background:var(--sf);color:var(--nv)}
.btn-lang:hover{background:var(--sf2)}

.timer-bar{background:linear-gradient(90deg,#001a3a,#002b5c);padding:7px 22px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px}
.timer-left-txt{color:rgba(255,255,255,.78);font-size:.79rem;display:flex;align-items:center;gap:7px}
.timer-clock{font-family:'Playfair Display',serif;font-size:1.1rem;font-weight:900;color:#fff;background:rgba(255,255,255,.13);padding:3px 13px;border-radius:20px;letter-spacing:.04em;min-width:100px;text-align:center}
.timer-clock.red{color:#ff6b6b;animation:pulse 1s infinite}
.closed-badge{background:#dc2626;color:#fff;font-size:.74rem;font-weight:800;padding:4px 13px;border-radius:20px;letter-spacing:.05em;display:none}
.closed-badge.show{display:inline-block;animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.55}}
.timer-prog-wrap{height:4px;background:rgba(255,255,255,.1)}
.timer-prog{height:100%;background:linear-gradient(90deg,var(--sf),var(--gr));transition:width 1s linear}

.nav-bar{background:linear-gradient(90deg,var(--gr2),var(--gr));padding:6px 22px;display:flex;gap:5px;flex-wrap:wrap}
.nav-btn{background:none;border:none;color:rgba(255,255,255,.78);font-size:.81rem;padding:5px 12px;border-radius:12px;cursor:pointer;font-family:'Mukta',sans-serif;font-weight:600;transition:background .2s,color .2s}
.nav-btn:hover,.nav-btn.active{background:rgba(255,255,255,.22);color:#fff}

.page{max-width:1100px;margin:0 auto;padding:26px 16px 60px;display:none}
.page.active{display:block}

.card{background:var(--card);border-radius:var(--rad);box-shadow:var(--shd);border:1px solid var(--bdr);padding:22px;margin-bottom:16px}
.badge-tag{display:inline-block;background:var(--sf);color:var(--nv);font-size:.66rem;font-weight:800;padding:3px 10px;border-radius:20px;letter-spacing:.08em;text-transform:uppercase;margin-bottom:8px}
.sec-title{font-family:'Playfair Display',serif;font-size:1.45rem;font-weight:700;color:var(--nv);margin-bottom:4px}

.alert{border-radius:10px;padding:11px 14px;font-size:.86rem;font-weight:500;margin-bottom:14px;display:flex;align-items:flex-start;gap:10px;line-height:1.6}
.a-info{background:#dbeafe;color:#1e40af;border-left:4px solid #3b82f6}
.a-warn{background:#fef9c3;color:#854d0e;border-left:4px solid var(--sf)}
.a-err {background:#fee2e2;color:#991b1b;border-left:4px solid #dc2626;display:none}
.a-ok  {background:#dcfce7;color:#166534;border-left:4px solid var(--gr);display:none}
.a-red {background:#fff1f2;color:#881337;border-left:4px solid #e11d48}
body.dark .a-info{background:rgba(30,64,175,.22);color:#93c5fd}
body.dark .a-warn{background:rgba(133,77,14,.22);color:#fde047}
body.dark .a-err {background:rgba(153,27,27,.22);color:#fca5a5}
body.dark .a-ok  {background:rgba(22,101,52,.22);color:#86efac}
body.dark .a-red {background:rgba(136,19,55,.22);color:#fda4af}

.fraud-overlay{position:fixed;inset:0;background:rgba(0,0,0,.88);z-index:99999;display:none;align-items:center;justify-content:center;padding:20px}
.fraud-overlay.open{display:flex;animation:fadeIn .3s ease}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
.fraud-box{background:#fff;border-radius:18px;padding:36px 30px;max-width:480px;width:100%;text-align:center;border:4px solid #dc2626;box-shadow:0 0 60px rgba(220,38,38,.5)}
body.dark .fraud-box{background:#1e293b}
.fraud-siren{font-size:3.5rem;animation:shake .4s infinite}
@keyframes shake{0%,100%{transform:rotate(-8deg)}50%{transform:rotate(8deg)}}
.fraud-title{font-family:'Playfair Display',serif;font-size:1.5rem;color:#dc2626;font-weight:900;margin:10px 0 6px}
.fraud-msg{font-size:.9rem;color:var(--txt);line-height:1.6;margin-bottom:16px}
.fraud-id{font-family:monospace;background:#fee2e2;color:#991b1b;padding:6px 16px;border-radius:8px;font-size:1.1rem;font-weight:800;display:inline-block;margin:6px 0 14px}
.fraud-countdown{font-family:'Playfair Display',serif;font-size:2.8rem;font-weight:900;color:#dc2626}
.fraud-count-lbl{font-size:.78rem;color:var(--muted);margin-top:2px}

.form-label{display:block;font-weight:700;font-size:.87rem;margin-bottom:5px}
.form-control{width:100%;border:1.5px solid var(--bdr);border-radius:8px;padding:10px 13px;font-family:'Mukta',sans-serif;font-size:.95rem;outline:none;background:var(--card);color:var(--txt);transition:border-color .2s,box-shadow .2s}
.form-control:focus{border-color:var(--nv);box-shadow:0 0 0 3px rgba(0,51,102,.10)}
.otp-inp{letter-spacing:.45em;font-size:1.5rem;text-align:center;font-weight:800;max-width:170px;margin:0 auto;display:block}

.btn{border:none;border-radius:8px;padding:10px 22px;font-size:.94rem;font-weight:700;cursor:pointer;font-family:'Mukta',sans-serif;transition:opacity .2s,transform .15s;display:inline-flex;align-items:center;gap:7px;line-height:1}
.btn:disabled{opacity:.46;cursor:not-allowed;transform:none!important}
.btn:not(:disabled):hover{opacity:.85;transform:translateY(-1px)}
.btn-prim{background:linear-gradient(135deg,var(--nv),var(--nv2));color:#fff}
.btn-succ{background:linear-gradient(135deg,var(--gr2),var(--gr));color:#fff}
.btn-dang{background:linear-gradient(135deg,#b91c1c,#dc2626);color:#fff}
.btn-outl{background:transparent;border:2px solid var(--nv);color:var(--nv)}
body.dark .btn-outl{border-color:#60a5fa;color:#60a5fa}
.btn-full{width:100%;justify-content:center}

.spinner{width:15px;height:15px;border:2.5px solid rgba(255,255,255,.3);border-top-color:#fff;border-radius:50%;animation:spin .7s linear infinite;display:inline-block;flex-shrink:0}
@keyframes spin{to{transform:rotate(360deg)}}

.cand-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:13px;margin-bottom:22px}
.cand-card{background:var(--card);border:2.5px solid var(--bdr);border-radius:var(--rad);padding:16px 14px 13px;cursor:pointer;transition:border-color .2s,box-shadow .2s,transform .18s;position:relative;user-select:none}
.cand-card:hover{border-color:var(--sf);box-shadow:0 6px 22px rgba(255,153,51,.16);transform:translateY(-2px)}
.cand-card.sel{border-color:var(--gr);background:rgba(19,136,8,.04);box-shadow:0 6px 22px rgba(19,136,8,.14)}
.radio-dot{position:absolute;top:12px;right:12px;width:21px;height:21px;border:2.5px solid #bbb;border-radius:50%;display:flex;align-items:center;justify-content:center}
.cand-card.sel .radio-dot{border-color:var(--gr);background:var(--gr)}
.cand-card.sel .radio-dot::after{content:'';width:8px;height:8px;background:#fff;border-radius:50%;display:block}

.sym-wrap{width:66px;height:66px;border-radius:50%;display:flex;align-items:center;justify-content:center;margin-bottom:8px;border:3px solid var(--sf);background:#f8fafc;overflow:hidden;flex-shrink:0}
body.dark .sym-wrap{background:#263349}
.sym-wrap svg{display:block}
.cand-name{font-weight:800;font-size:.89rem;line-height:1.3;margin-top:2px}
.cand-te{font-size:.72rem;color:var(--muted);margin:2px 0 5px}
.party-tag{display:inline-block;font-size:.65rem;font-weight:700;padding:2px 8px;border-radius:10px;letter-spacing:.03em}

.stats-row{display:grid;grid-template-columns:repeat(auto-fit,minmax(125px,1fr));gap:11px;margin-bottom:18px}
.stat-box{background:var(--card);border:1px solid var(--bdr);border-radius:var(--rad);padding:15px;text-align:center}
.stat-val{font-family:'Playfair Display',serif;font-size:1.85rem;font-weight:900;color:var(--nv);line-height:1}
.stat-lbl{font-size:.7rem;color:var(--muted);margin-top:4px;font-weight:600;text-transform:uppercase;letter-spacing:.04em}

.bar-wrap{margin-bottom:12px}
.bar-label{display:flex;justify-content:space-between;font-size:.82rem;font-weight:600;margin-bottom:4px;flex-wrap:wrap;gap:4px}
.bar-bg{height:21px;background:#e9ecef;border-radius:11px;overflow:hidden}
.bar-fill{height:100%;border-radius:11px;transition:width .9s cubic-bezier(.4,0,.2,1);display:flex;align-items:center;padding-left:8px;color:#fff;font-size:.71rem;font-weight:700}

.ldr-row{display:flex;align-items:center;gap:12px;padding:11px 14px;border-radius:10px;margin-bottom:8px;border:1px solid var(--bdr);background:var(--card)}
.ldr-row.r1{background:linear-gradient(90deg,rgba(255,215,0,.09),transparent);border-color:gold}
.ldr-row.r2{background:linear-gradient(90deg,rgba(192,192,192,.09),transparent);border-color:silver}
.ldr-row.r3{background:linear-gradient(90deg,rgba(205,127,50,.09),transparent);border-color:#cd7f32}
.ldr-medal{font-size:1.4rem;width:28px;text-align:center;flex-shrink:0}
.ldr-sym{width:38px;height:38px;border-radius:50%;display:flex;align-items:center;justify-content:center;background:#f0f4f8;border:2px solid var(--bdr);flex-shrink:0}
body.dark .ldr-sym{background:#334155;border-color:#475569}
.ldr-info{flex:1;min-width:0}
.ldr-name{font-weight:700;font-size:.87rem;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.ldr-party{font-size:.72rem;color:var(--muted)}
.ldr-votes{font-family:'Playfair Display',serif;font-size:1.25rem;font-weight:900;color:var(--nv);white-space:nowrap}
body.dark .ldr-votes{color:var(--sf)}
.ldr-pct{font-size:.72rem;color:var(--muted);text-align:right}

.tbl{width:100%;border-collapse:collapse;font-size:.85rem}
.tbl th{background:var(--nv);color:#fff;padding:9px 11px;text-align:left;font-size:.75rem;font-weight:700;letter-spacing:.04em;text-transform:uppercase}
.tbl td{padding:9px 11px;border-bottom:1px solid var(--bdr);vertical-align:middle}
.tbl tr:last-child td{border-bottom:none}
.tbl tr:hover td{background:rgba(0,51,102,.04)}

.charts-row{display:grid;grid-template-columns:3fr 2fr;gap:14px;margin-bottom:14px}
@media(max-width:660px){.charts-row{grid-template-columns:1fr}}

.modal-bg{position:fixed;inset:0;background:rgba(0,0,0,.55);backdrop-filter:blur(3px);z-index:9000;display:none;align-items:center;justify-content:center;padding:14px}
.modal-bg.open{display:flex}
.modal-box{background:var(--card);border-radius:18px;padding:28px;max-width:460px;width:100%;box-shadow:0 28px 80px rgba(0,0,0,.28);animation:slideUp .3s cubic-bezier(.34,1.56,.64,1)}
@keyframes slideUp{from{opacity:0;transform:translateY(30px)}to{opacity:1;transform:translateY(0)}}

.success-icon{width:70px;height:70px;border-radius:50%;background:linear-gradient(135deg,var(--gr2),var(--gr));display:flex;align-items:center;justify-content:center;font-size:2.1rem;margin:0 auto 13px;animation:pop .5s cubic-bezier(.34,1.56,.64,1)}
@keyframes pop{from{transform:scale(0)}to{transform:scale(1)}}

.ring-wrap{display:flex;flex-direction:column;align-items:center;margin-top:14px;position:relative}
.ring-wrap svg{width:80px;height:80px}
.ring-bg-c{fill:none;stroke:var(--bdr);stroke-width:5}
.ring-prog-c{fill:none;stroke:var(--gr);stroke-width:5;stroke-linecap:round;stroke-dasharray:220;transform-origin:50% 50%;transform:rotate(-90deg);transition:stroke-dashoffset 1s linear}
.ring-num-inner{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center}
.ring-num{font-family:'Playfair Display',serif;font-size:1.8rem;font-weight:900;color:var(--nv);line-height:1}
body.dark .ring-num{color:var(--sf)}
.ring-lbl{font-size:.68rem;color:var(--muted);margin-top:2px}

.final-banner{background:linear-gradient(135deg,#7c0000,#b91c1c);color:#fff;border-radius:var(--rad);padding:18px 22px;margin-bottom:16px;text-align:center}
.final-banner h2{font-family:'Playfair Display',serif;font-size:1.5rem;margin-bottom:3px}
.final-banner p{font-size:.86rem;opacity:.82}
.winner-card{background:linear-gradient(135deg,rgba(255,215,0,.14),rgba(255,153,51,.06));border:2px solid gold;border-radius:var(--rad);padding:20px;text-align:center;margin-bottom:16px}
.winner-sym{margin:0 auto 8px;width:80px;height:80px;border-radius:50%;background:#fff;display:flex;align-items:center;justify-content:center;border:3px solid gold}
body.dark .winner-sym{background:#263349}
.winner-name{font-family:'Playfair Display',serif;font-size:1.45rem;font-weight:900;margin:6px 0 3px}
.winner-party{font-size:.86rem;color:var(--muted)}
.winner-votes{font-family:'Playfair Display',serif;font-size:2.1rem;font-weight:900;color:var(--gr);margin-top:7px}

.mb1{margin-bottom:8px}.mb2{margin-bottom:16px}.mb3{margin-bottom:22px}
.mt1{margin-top:8px}.mt2{margin-top:16px}
.txt-center{text-align:center}
.txt-muted{color:var(--muted);font-size:.81rem}
.fw7{font-weight:700}.fw8{font-weight:800}
.row-bet{display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px}
.row-cen{display:flex;justify-content:center;align-items:center;flex-wrap:wrap;gap:10px}
.code{font-family:monospace;background:rgba(0,0,0,.08);padding:1px 6px;border-radius:4px;font-size:.87em}
body.dark .code{background:rgba(255,255,255,.11)}
@media(max-width:480px){.page{padding:18px 10px 50px}}

footer{background:linear-gradient(135deg,#001a3a,var(--nv));color:rgba(255,255,255,.7);padding:26px 22px;font-size:.78rem}
footer a{color:var(--sf);text-decoration:none}
.ft-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:18px;margin-bottom:14px;max-width:1100px;margin-left:auto;margin-right:auto}
.ft-head{font-weight:700;color:#fff;margin-bottom:5px;font-size:.84rem}
.ft-bot{border-top:1px solid rgba(255,255,255,.10);padding-top:11px;text-align:center;font-size:.72rem;line-height:1.9;max-width:1100px;margin:0 auto}
</style>
</head>
<body>

<div class="fraud-overlay" id="fraud-overlay">
  <div class="fraud-box">
    <div class="fraud-siren">🚨</div>
    <div class="fraud-title">FRAUD ATTEMPT DETECTED!</div>
    <div class="fraud-msg">
      This Voter ID has <strong>already cast a vote</strong> in this election.
      Attempting to vote again is a criminal offence under the
      <strong>Representation of the People Act, 1951 — Section 171G</strong>.
      This incident has been automatically reported to the Election Officer.
    </div>
    <div class="fraud-id" id="fraud-voter-id">--</div>
    <div style="font-size:.82rem;color:var(--muted);margin-bottom:10px">
      📋 Incident logged at <strong id="fraud-time">--</strong><br/>
      🏛️ Reported to: <strong>Returning Officer, Tirupati (SC)</strong>
    </div>
    <div class="fraud-countdown" id="fraud-count">10</div>
    <div class="fraud-count-lbl">Redirecting to Login in <span id="fraud-count2">10</span> seconds</div>
    <div style="margin-top:14px">
      <button class="btn btn-dang" onclick="closeFraud()">Acknowledge &amp; Go Back</button>
    </div>
  </div>
</div>

<div class="tricolor"></div>
<div class="hdr">
  <div class="hdr-left">
    <span class="hdr-logo">🗳️</span>
    <div>
      <div class="hdr-t1" id="hdr-t1">Tirupati Assembly Constituency</div>
      <div class="hdr-t3" id="hdr-t3">తిరుపతి అసెంబ్లీ నియోజకవర్గం</div>
      <div class="hdr-t2" id="hdr-t2">Online Voting System — 2024 General Elections</div>
    </div>
  </div>
  <div class="hdr-right">
    <button class="btn-sm btn-theme" id="theme-btn" onclick="toggleTheme()">🌙 Dark</button>
    <button class="btn-sm btn-lang"  id="lang-btn"  onclick="toggleLang()">తెలుగు</button>
  </div>
</div>

<div class="timer-bar">
  <div class="timer-left-txt">
    🗓️ &nbsp;<span id="timer-status">Voting in Progress — Closes at <strong id="close-time">--:--:--</strong></span>
  </div>
  <div style="display:flex;align-items:center;gap:10px">
    <span style="color:rgba(255,255,255,.55);font-size:.74rem">Time Remaining:</span>
    <span class="timer-clock" id="timer-clock">01:00:00</span>
    <span class="closed-badge" id="closed-badge">⛔ VOTING CLOSED</span>
  </div>
</div>
<div class="timer-prog-wrap">
  <div class="timer-prog" id="timer-prog" style="width:100%"></div>
</div>

<div class="nav-bar">
  <button class="nav-btn active" id="nav-login"   onclick="goTo('login')">🏠 <span id="nl">Voter Login</span></button>
  <button class="nav-btn" id="nav-vote" style="display:none" onclick="goTo('vote')">✅ <span id="nv2">Cast Vote</span></button>
  <button class="nav-btn" id="nav-results" onclick="goTo('results')">📊 <span id="nr">Live Results</span></button>
  <button class="nav-btn" id="nav-admin"   onclick="goTo('admin')">🔐 <span id="na">Admin Panel</span></button>
</div>

<div class="page active" id="page-login">
  <div style="max-width:520px;margin:0 auto">

    <div class="alert a-red" id="closed-notice" style="display:none">
      ⛔ <strong>Voting has closed.</strong> The 1-hour election window has ended.
      <button class="btn btn-dang" style="padding:5px 13px;font-size:.78rem;margin-left:8px" onclick="goTo('results')">View Final Results</button>
    </div>

    <div class="alert a-info">
      ℹ️ Official portal — Tirupati (SC) Assembly Constituency, 2024 General Elections.
      Each Voter ID can vote <strong>once only</strong>. Voting is open for <strong>1 hour</strong>.
    </div>

    <div class="card">
      <span class="badge-tag">🗳️ Voter Authentication</span>
      <div class="sec-title">Voter Login</div>
      <div class="txt-muted mb2">Tirupati (SC) Assembly — 2024 Elections</div>

      <div class="alert a-err" id="login-err"><span id="login-err-txt"></span></div>
      <div class="alert a-ok"  id="login-ok" ><span id="login-ok-txt"></span></div>

      
      <div id="step-voter">
        <label class="form-label">Enter Your Voter ID</label>
        <input class="form-control mb1" id="voter-id-inp"
          placeholder="AP12345" maxlength="7"
          oninput="this.value=this.value.toUpperCase().replace(/[^A-Z0-9]/g,'')"
          onkeydown="if(event.key==='Enter')sendOtp()"/>
        <div class="txt-muted mb2">
          💡 Format: AP + 5 digits &nbsp;|&nbsp; Example: <span class="code">AP12345</span>
        </div>
        <button class="btn btn-prim btn-full" id="otp-btn" onclick="sendOtp()">📱 Send OTP</button>
      </div>

      
      <div id="step-otp" style="display:none">
        <label class="form-label txt-center">Enter OTP</label>
        <input class="form-control otp-inp mb1" id="otp-inp"
          placeholder="Enter your OTP" maxlength="4"
          oninput="this.value=this.value.replace(/\D/g,'')"
          onkeydown="if(event.key==='Enter')verifyOtp()"/>
        <div class="txt-muted mb2 txt-center">OTP sent to your registered mobile &nbsp;|&nbsp; Your OTP: <strong id="otp-hint-txt" style="font-family:monospace;font-size:1.1rem;letter-spacing:.2em;color:var(--gr)">----</strong></div>
        <button class="btn btn-succ btn-full mb1" id="verify-btn" onclick="verifyOtp()">🔓 Verify & Login</button>
        <button class="btn btn-outl btn-full"      onclick="backToVid()">← Change Voter ID</button>
      </div>
    </div>

    <div class="alert a-warn">
      🧪 <strong>Demo:</strong> Any Voter ID like <span class="code">AP12345</span> &nbsp;|&nbsp; OTP: <strong>Any 4-digit number shown on screen after clicking Send OTP</strong>
    </div>
  </div>
</div>

<div class="page" id="page-vote">
  <div class="card row-bet">
    <div>
      <span class="badge-tag">✅ Electronic Ballot</span>
      <div class="sec-title">Select Your Candidate</div>
      <div class="txt-muted" id="vote-vid-lbl"></div>
    </div>
    <div style="text-align:right">
      <div style="font-size:.78rem;color:var(--muted)">Session Timer</div>
      <div class="timer-clock" id="vote-timer" style="font-size:.95rem;margin-top:3px">01:00:00</div>
    </div>
  </div>

  <div class="alert a-err" id="vote-err" style="display:none">
    ⚠️ Please select a candidate before submitting your vote.
  </div>

  <div class="cand-grid" id="cand-grid"></div>

  <div class="txt-center">
    <button class="btn btn-succ" style="padding:13px 44px;font-size:1rem" onclick="castVote()">
      🗳️ Cast Vote
    </button>
    <div class="txt-muted mt1">🔐 Your vote is AES-256 encrypted before being stored.</div>
  </div>
</div>

<div class="page" id="page-results">

  
  <div id="final-banner-wrap" style="display:none">
    <div class="final-banner">
      <h2>🏆 Final Election Results</h2>
      <p>Tirupati (SC) Assembly Constituency — 2024 General Elections | Voting Period Ended</p>
    </div>
    <div class="winner-card" id="winner-card"></div>
  </div>

  <div class="card row-bet" style="margin-bottom:16px">
    <div>
      <span class="badge-tag">📊 Live Results</span>
      <div class="sec-title" id="results-title">Live Election Results</div>
      <div class="txt-muted">Tirupati (SC) Assembly Constituency — 2024</div>
    </div>
    <div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap">
      <label style="display:flex;align-items:center;gap:5px;font-size:.83rem;font-weight:600;cursor:pointer">
        <input type="checkbox" id="auto-chk" onchange="toggleAuto(this.checked)" style="width:14px;height:14px">
        Auto Refresh
      </label>
      <button class="btn btn-prim" style="padding:7px 13px;font-size:.81rem" onclick="loadResults()">🔄 Refresh</button>
    </div>
  </div>

  <div class="stats-row" id="res-stats"></div>

  <div class="charts-row">
    <div class="card"><div class="fw7 mb2">📊 Bar Chart</div><canvas id="bar-chart" style="max-height:260px"></canvas></div>
    <div class="card"><div class="fw7 mb2">🥧 Vote Share</div><canvas id="pie-chart" style="max-height:260px"></canvas></div>
  </div>

  <div class="card"><div class="fw7 mb2">🏆 Leaderboard</div><div id="leaderboard"></div></div>
  <div class="card"><div class="fw7 mb2">📈 Vote % Bars</div><div id="res-bars"></div></div>

  <div class="card">
    <div class="fw7 mb2">📋 Candidate Details</div>
    <div style="overflow-x:auto">
      <table class="tbl">
        <thead><tr>
          <th>#</th><th>Symbol</th><th>Candidate</th><th>Party</th><th>Votes</th><th>%</th><th>Status</th>
        </tr></thead>
        <tbody id="res-table"></tbody>
      </table>
    </div>
  </div>
</div>

<div class="page" id="page-admin">
  <div id="admin-login-wrap" style="max-width:460px;margin:0 auto">
    <div class="card">
      <span class="badge-tag">🔐 Secure Admin Access</span>
      <div class="sec-title mb3">Admin Login</div>
      <div class="alert a-err" id="admin-err" style="display:none"><span id="admin-err-txt"></span></div>
      <div class="mb2">
        <label class="form-label">Email</label>
        <input class="form-control" id="adm-email" type="email" placeholder="admin@tirupati.gov.in"
          onkeydown="if(event.key==='Enter')adminLogin()"/>
      </div>
      <div class="mb3">
        <label class="form-label">Password</label>
        <input class="form-control" id="adm-pass" type="password" placeholder="••••••••"
          onkeydown="if(event.key==='Enter')adminLogin()"/>
      </div>
      <button class="btn btn-prim btn-full" id="adm-btn" onclick="adminLogin()">🔐 Login</button>
      <div class="alert a-warn mt2">
        🧪 <strong>Demo:</strong> <span class="code"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d0b1b4bdb9be90a4b9a2a5a0b1a4b9feb7bfa6feb9be">[email&#160;protected]</a></span> / <span class="code">Admin@123</span>
      </div>
    </div>
  </div>

  <div id="admin-dash" style="display:none">
    <div class="card row-bet mb2">
      <div>
        <span class="badge-tag">🔐 Admin Dashboard</span>
        <div class="sec-title">Admin Panel</div>
      </div>
      <div style="display:flex;gap:8px">
        <button class="btn btn-prim" style="padding:7px 13px;font-size:.81rem" onclick="adminRefresh()">🔄 Refresh</button>
        <button class="btn btn-outl" style="padding:7px 13px;font-size:.81rem" onclick="adminLogout()">🚪 Logout</button>
      </div>
    </div>
    <div class="alert a-ok" id="adm-ok" style="display:none"><span id="adm-ok-txt"></span></div>

    
    <div class="card mb2">
      <div class="fw7 mb2">🚨 Fraud Attempt Log</div>
      <div style="overflow-x:auto">
        <table class="tbl">
          <thead><tr><th>#</th><th>Voter ID</th><th>Attempt Time</th><th>IP (Simulated)</th><th>Status</th></tr></thead>
          <tbody id="fraud-log-table"><tr><td colspan="5" style="text-align:center;color:var(--muted);padding:18px">No fraud attempts recorded.</td></tr></tbody>
        </table>
      </div>
    </div>

    <div class="stats-row" id="adm-stats"></div>
    <div class="card mb2">
      <div class="fw7 mb2">📋 Candidate-wise Breakup</div>
      <div style="overflow-x:auto">
        <table class="tbl">
          <thead><tr><th>#</th><th>Symbol</th><th>Candidate</th><th>Party</th><th>Votes</th><th>%</th><th>Progress</th></tr></thead>
          <tbody id="adm-table"></tbody>
        </table>
      </div>
    </div>
    <div class="card">
      <div class="fw7 mb2">⚙️ Admin Actions</div>
      <div style="display:flex;gap:10px;flex-wrap:wrap">
        <button class="btn btn-prim" onclick="downloadCSV()">📥 Download CSV</button>
        <button class="btn btn-dang" onclick="confirmReset()">🗑️ Reset All Votes</button>
        <button class="btn btn-dang" onclick="resetTimer()" style="background:linear-gradient(135deg,#7c3aed,#9333ea)">⏱️ Reset Timer</button>
      </div>
      <div class="txt-muted mt1">⚠️ Reset clears all votes &amp; voter records. Timer reset restarts the 1-hour election window.</div>
    </div>
  </div>
</div>

<div class="modal-bg" id="modal-confirm">
  <div class="modal-box txt-center">
    <div class="sec-title mb2" style="color:var(--nv)">Confirm Your Vote</div>
    <div id="modal-preview" style="border-radius:12px;padding:17px;margin-bottom:13px;border:2px solid #ccc;background:var(--bg)"></div>
    <div class="txt-muted mb2" style="font-size:.84rem">
      ⚠️ This action is <strong>irreversible</strong>. Your vote will be AES-encrypted and permanently recorded.
    </div>
    <div class="row-cen" style="gap:10px">
      <button class="btn btn-dang" onclick="closeModal('modal-confirm')">✖ Cancel</button>
      <button class="btn btn-succ" id="confirm-btn" onclick="submitVote()">✅ Confirm Vote</button>
    </div>
  </div>
</div>

<div class="modal-bg" id="modal-success">
  <div class="modal-box txt-center">
    <div class="success-icon">✅</div>
    <div class="sec-title" style="color:var(--gr);margin-bottom:6px">Vote Recorded Successfully!</div>
    <div class="txt-muted mb2">
      Your vote has been securely encrypted and stored. Thank you for participating in democracy.
    </div>
    <div class="alert a-ok" style="display:flex">
      🔐 <span id="success-label">Vote encrypted &amp; stored.</span>
    </div>
    
    <div class="ring-wrap mt2">
      <svg viewBox="0 0 80 80">
        <circle class="ring-bg-c" cx="40" cy="40" r="35"/>
        <circle class="ring-prog-c" id="ring-circle" cx="40" cy="40" r="35"/>
      </svg>
      <div class="ring-num-inner">
        <div class="ring-num" id="ring-num">5</div>
        <div class="ring-lbl">seconds</div>
      </div>
    </div>
    <div class="txt-muted mt1" style="font-size:.79rem">
      Showing results then redirecting to <strong>Login</strong> in <span id="redirect-count">5</span> seconds…
    </div>
    <div style="margin-top:12px">
      <button class="btn btn-prim" onclick="doRedirect()">🏠 Go to Login Now</button>
    </div>
  </div>
</div>

<div class="modal-bg" id="modal-reset">
  <div class="modal-box txt-center">
    <div style="font-size:2.8rem;margin-bottom:10px">⚠️</div>
    <div class="sec-title" style="color:#b91c1c;margin-bottom:8px">Confirm Reset</div>
    <div class="txt-muted mb3">This will permanently delete ALL votes and voter records. Cannot be undone.</div>
    <div class="row-cen" style="gap:10px">
      <button class="btn btn-outl" onclick="closeModal('modal-reset')">Cancel</button>
      <button class="btn btn-dang" onclick="doReset()">🗑️ Yes, Reset All</button>
    </div>
  </div>
</div>

<div class="modal-bg" id="modal-restart" style="z-index:99998">
  <div class="modal-box txt-center" style="border:3px solid var(--sf);max-width:420px">
    <div style="font-size:3rem;margin-bottom:6px">🔄</div>
    <div class="sec-title" style="color:var(--sf);margin-bottom:6px">New Voting Round Starting</div>
    <div class="txt-muted mb2">Results have been recorded. A fresh election cycle begins automatically.</div>
    <div style="font-family:'Playfair Display',serif;font-size:3.5rem;font-weight:900;color:var(--nv)" id="restart-count">60</div>
    <div class="txt-muted" style="margin-bottom:16px">seconds until new round</div>
    <div style="height:8px;background:var(--bdr);border-radius:8px;overflow:hidden;margin-bottom:16px">
      <div id="restart-prog" style="height:100%;width:100%;background:linear-gradient(90deg,var(--sf),var(--gr));border-radius:8px;transition:width 1s linear"></div>
    </div>
    <div class="txt-muted" style="font-size:.78rem">All votes will be cleared and a new 1-hour session will begin.</div>
  </div>
</div>

<footer>
  <div style="max-width:1100px;margin:0 auto">
    <div class="ft-grid">
      <div>
        <div class="ft-head">🗳️ Election Commission of India</div>
        Tirupati (SC) Assembly Constituency<br>Andhra Pradesh<br>
        <a href="https://eci.gov.in" target="_blank">eci.gov.in</a>
      </div>
      <div>
        <div class="ft-head">Quick Links</div>
        <a href="#">Voter Helpline: 1950</a><br>
        <a href="#">National Voter Services Portal</a><br>
        <a href="#">Voter ID Card Application</a>
      </div>
      <div>
        <div class="ft-head">Election Info</div>
        Constituency No: 175<br>Category: SC (Reserved)<br>State: Andhra Pradesh
      </div>
    </div>
    <div class="ft-bot">
      ⚠️ Disclaimer: This is a demo portal for educational purposes only. For official info visit eci.gov.in<br>
      © 2024 Election Commission of India | Demo Portal — For Educational Use Only
    </div>
  </div>
</footer>

<script>

var ENC_KEY      = "ECI-TIRUPATI-2024-SECURE-AES-KEY";
var ADMIN_EMAIL  = "admin@tirupati.gov.in";
var ADMIN_PASS   = "Admin@123";
var LS_VOTES     = "eci_tp_votes_v2";
var LS_VOTERS    = "eci_tp_voters_v2";
var LS_ADMIN     = "eci_tp_admin_v2";
var LS_TIMER     = "eci_tp_timer_v2";
var LS_FRAUD     = "eci_tp_fraud_v2";
var ELECTION_DURATION = 60 * 60 * 1000; 

var SYMBOLS = {
  
  TDP: '<svg viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg" width="44" height="44"><circle cx="12" cy="34" r="8" stroke="#1a6b00" stroke-width="2.5" fill="none"/><circle cx="36" cy="34" r="8" stroke="#1a6b00" stroke-width="2.5" fill="none"/><path d="M12 34 L24 16 L36 34" stroke="#1a6b00" stroke-width="2.5" stroke-linejoin="round" fill="none"/><path d="M24 16 L30 34" stroke="#1a6b00" stroke-width="2" fill="none"/><path d="M19 22 L30 22" stroke="#1a6b00" stroke-width="2" stroke-linecap="round"/><circle cx="24" cy="16" r="2" fill="#1a6b00"/><circle cx="12" cy="34" r="2.5" fill="#1a6b00"/><circle cx="36" cy="34" r="2.5" fill="#1a6b00"/></svg>',

  
  YSRCP: '<svg viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg" width="44" height="44"><circle cx="24" cy="24" r="3.5" fill="#1e40af"/><line x1="24" y1="8" x2="24" y2="20" stroke="#1e40af" stroke-width="2"/><ellipse cx="24" cy="14" rx="7" ry="4" fill="#1e40af" opacity=".85" transform="rotate(0 24 14)"/><ellipse cx="34" cy="24" rx="7" ry="4" fill="#1e40af" opacity=".85" transform="rotate(90 34 24)"/><ellipse cx="24" cy="34" rx="7" ry="4" fill="#1e40af" opacity=".85" transform="rotate(180 24 34)"/><ellipse cx="14" cy="24" rx="7" ry="4" fill="#1e40af" opacity=".85" transform="rotate(270 14 24)"/><circle cx="24" cy="24" r="3" fill="#1e40af"/><line x1="24" y1="5" x2="24" y2="9" stroke="#1e40af" stroke-width="2.5" stroke-linecap="round"/></svg>',

  
  JSP: '<svg viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg" width="44" height="44"><path d="M13 11 L15 40 Q15 42 24 42 Q33 42 33 40 L35 11 Z" stroke="#c2400a" stroke-width="2.2" fill="rgba(234,88,12,.12)" stroke-linejoin="round"/><line x1="13" y1="11" x2="35" y2="11" stroke="#c2400a" stroke-width="2.2" stroke-linecap="round"/><path d="M33 18 Q38 18 38 22 Q38 26 33 26" stroke="#c2400a" stroke-width="2" fill="none" stroke-linecap="round"/><line x1="17" y1="17" x2="31" y2="17" stroke="#c2400a" stroke-width="1.5" stroke-linecap="round" opacity=".5"/><line x1="17" y1="22" x2="31" y2="22" stroke="#c2400a" stroke-width="1.5" stroke-linecap="round" opacity=".4"/><path d="M19 8 Q21 5 24 8 Q27 5 29 8" stroke="#c2400a" stroke-width="1.5" fill="none" stroke-linecap="round"/></svg>',

  
  BJP: '<svg viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg" width="44" height="44"><path d="M24 36 Q14 28 14 20 Q14 12 24 14 Q34 12 34 20 Q34 28 24 36Z" fill="rgba(255,153,51,.2)" stroke="#FF9933" stroke-width="1.8"/><path d="M24 36 Q10 24 10 16 Q16 10 24 16 Q32 10 38 16 Q38 24 24 36Z" fill="rgba(255,153,51,.15)" stroke="#FF9933" stroke-width="1.8"/><path d="M24 36 Q8 26 12 12 Q18 8 24 18 Q30 8 36 12 Q40 26 24 36Z" fill="rgba(255,153,51,.1)" stroke="#FF9933" stroke-width="1.5"/><circle cx="24" cy="22" r="4" fill="#FF9933"/><line x1="24" y1="36" x2="24" y2="44" stroke="#FF9933" stroke-width="2" stroke-linecap="round"/></svg>',

  
  INC: '<svg viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg" width="44" height="44"><rect x="18" y="22" width="4" height="16" rx="2" fill="#15803d"/><rect x="14" y="18" width="4" height="20" rx="2" fill="#15803d"/><rect x="22" y="18" width="4" height="20" rx="2" fill="#15803d"/><rect x="26" y="20" width="4" height="18" rx="2" fill="#15803d"/><path d="M30 25 Q34 23 34 27 L34 34 Q34 38 30 38 L14 38 Q12 38 12 36 L12 28 Q12 26 14 25 L14 22" stroke="#15803d" stroke-width="1.5" fill="rgba(21,128,61,.12)" stroke-linejoin="round"/><rect x="10" y="28" width="4" height="12" rx="2" fill="#15803d"/></svg>',

  
  NOTA: '<svg viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg" width="44" height="44"><rect x="6" y="6" width="36" height="36" rx="5" stroke="#64748b" stroke-width="2.5" fill="rgba(100,116,139,.08)"/><line x1="14" y1="14" x2="34" y2="34" stroke="#dc2626" stroke-width="3.5" stroke-linecap="round"/><line x1="34" y1="14" x2="14" y2="34" stroke="#dc2626" stroke-width="3.5" stroke-linecap="round"/><circle cx="24" cy="24" r="7" stroke="#dc2626" stroke-width="2" fill="none"/></svg>'
};

var CANDS = [
  {id:1, name:"Nara Chandra Babu Naidu", te:"నారా చంద్రబాబు నాయుడు", party:"Telugu Desam Party",       short:"TDP",   symKey:"TDP",   col:"#16a34a", bbg:"#f0fdf4", btxt:"#166534"},
  {id:2, name:"Y.S. Jagan Mohan Reddy",  te:"వై.ఎస్. జగన్ మోహన్ రెడ్డి", party:"YSR Congress Party",  short:"YSRCP", symKey:"YSRCP", col:"#1e40af", bbg:"#dbeafe", btxt:"#1e40af"},
  {id:3, name:"Pawan Kalyan",             te:"పవన్ కళ్యాణ్",             party:"Jana Sena Party",        short:"JSP",   symKey:"JSP",   col:"#ea580c", bbg:"#fff7ed", btxt:"#9a3412"},
  {id:4, name:"Narendra Modi",            te:"నరేంద్ర మోదీ",             party:"Bharatiya Janata Party", short:"BJP",   symKey:"BJP",   col:"#FF9933", bbg:"#fff7ed", btxt:"#7c2d12"},
  {id:5, name:"Rahul Gandhi",             te:"రాహుల్ గాంధీ",             party:"Indian National Congress",short:"INC",  symKey:"INC",   col:"#15803d", bbg:"#dcfce7", btxt:"#166534"},
  {id:6, name:"NOTA",                     te:"నోటా (పై వారెవరూ కాదు)",   party:"None Of The Above",      short:"NOTA",  symKey:"NOTA",  col:"#dc2626", bbg:"#fff1f2", btxt:"#991b1b", isNota:true}
];

var currentVoterId  = null;
var selectedCandId  = null;
var lang            = "en";
var theme           = "light";
var autoId          = null;
var barChart        = null;
var pieChart        = null;
var tempVoterId     = "";
var currentOtp      = "";   
var redirectTimer   = null;
var fraudCountTimer = null;
var electionEndTime = 0;
var timerInterval   = null;
var votingClosed    = false;

function enc(data){
  return CryptoJS.AES.encrypt(JSON.stringify(data), ENC_KEY).toString();
}
function dec(str){
  try{
    var b = CryptoJS.AES.decrypt(str, ENC_KEY);
    var s = b.toString(CryptoJS.enc.Utf8);
    return s ? JSON.parse(s) : null;
  }catch(e){ return null; }
}
function getVotes()  { var r=localStorage.getItem(LS_VOTES);  return r?(dec(r)||{}):{};  }
function getVoters() { var r=localStorage.getItem(LS_VOTERS); return r?(dec(r)||{}):{};  }
function getFraudLog(){ var r=localStorage.getItem(LS_FRAUD); return r?(dec(r)||[]):[]; }
function saveVotes(v)  { localStorage.setItem(LS_VOTES,  enc(v)); }
function saveVoters(v) { localStorage.setItem(LS_VOTERS, enc(v)); }
function saveFraudLog(l){ localStorage.setItem(LS_FRAUD, enc(l)); }
function hasVoted(id)  { return !!getVoters()[id.toUpperCase()]; }
function markVoted(id) {
  var v = getVoters();
  v[id.toUpperCase()] = new Date().toISOString();
  saveVoters(v);
}
function isValidId(id){ return /^AP\d{5}$/.test(id); }
function randIP(){ return (100+Math.floor(Math.random()*155))+"."+(0+Math.floor(Math.random()*255))+"."+(0+Math.floor(Math.random()*255))+"."+(1+Math.floor(Math.random()*254)); }

function computeResults(){
  var votes = getVotes();
  var total = CANDS.reduce(function(s,c){ return s+(votes[c.id]||0); },0);
  var res   = CANDS.map(function(c){
    var v = votes[c.id]||0;
    return Object.assign({},c,{votes:v, pct: total>0?((v/total)*100).toFixed(1):"0.0", total:total});
  });
  res.sort(function(a,b){ return b.votes-a.votes; });
  return res;
}

function initTimer(){
  var stored = localStorage.getItem(LS_TIMER);
  if(stored){
    electionEndTime = parseInt(stored);
  } else {
    electionEndTime = Date.now() + ELECTION_DURATION;
    localStorage.setItem(LS_TIMER, electionEndTime);
  }
  tickTimer();
  timerInterval = setInterval(tickTimer, 1000);
}

function tickTimer(){
  var now       = Date.now();
  var remaining = electionEndTime - now;
  var elapsed   = ELECTION_DURATION - remaining;
  var pct       = Math.max(0, (remaining / ELECTION_DURATION) * 100);

  document.getElementById("timer-prog").style.width = pct + "%";

  if(remaining <= 0){
    
    if(!votingClosed){
      votingClosed = true;
      onElectionClose();
    }
    document.getElementById("timer-clock").textContent = "00:00:00";
    document.getElementById("timer-clock").classList.add("red");
    return;
  }

  
  var h = Math.floor(remaining / 3600000);
  var m = Math.floor((remaining % 3600000) / 60000);
  var s = Math.floor((remaining % 60000) / 1000);
  var txt = pad(h)+":"+pad(m)+":"+pad(s);
  document.getElementById("timer-clock").textContent = txt;
  document.getElementById("vote-timer").textContent  = txt;

  
  if(remaining < 300000){
    document.getElementById("timer-clock").classList.add("red");
  }

  
  var closeDate = new Date(electionEndTime);
  document.getElementById("close-time").textContent =
    pad(closeDate.getHours())+":"+pad(closeDate.getMinutes())+":"+pad(closeDate.getSeconds());
}

function pad(n){ return n<10?"0"+n:String(n); }

var restartCountTimer = null;
var AUTO_RESTART_SECS = 60;

function onElectionClose(){
  votingClosed = true;
  document.getElementById("closed-badge").classList.add("show");
  document.getElementById("timer-status").innerHTML = "⛔ <strong>Voting Period Has Ended</strong>";
  document.getElementById("closed-notice").style.display = "flex";
  goTo("results");
  loadResults();
  startAutoRestartCountdown();
}

function startAutoRestartCountdown(){
  var left = AUTO_RESTART_SECS;
  var total = AUTO_RESTART_SECS;
  document.getElementById("restart-count").textContent = left;
  document.getElementById("restart-prog").style.width = "100%";
  document.getElementById("modal-restart").classList.add("open");
  if(restartCountTimer) clearInterval(restartCountTimer);
  restartCountTimer = setInterval(function(){
    left--;
    document.getElementById("restart-count").textContent = left;
    document.getElementById("restart-prog").style.width = ((left / total) * 100) + "%";
    if(left <= 0){
      clearInterval(restartCountTimer);
      document.getElementById("modal-restart").classList.remove("open");
      fullReset();
    }
  }, 1000);
}

function fullReset(){
  localStorage.removeItem(LS_VOTES);
  localStorage.removeItem(LS_VOTERS);
  localStorage.removeItem(LS_FRAUD);
  localStorage.removeItem(LS_TIMER);
  votingClosed = false;
  selectedCandId = null;
  currentVoterId = null;
  currentOtp = "";
  document.getElementById("closed-badge").classList.remove("show");
  document.getElementById("timer-status").innerHTML = "Voting in Progress — Closes at <strong id='close-time'>--:--:--</strong>";
  document.getElementById("closed-notice").style.display = "none";
  document.getElementById("timer-clock").classList.remove("red");
  document.getElementById("final-banner-wrap").style.display = "none";
  document.getElementById("nav-vote").style.display = "none";
  document.getElementById("voter-id-inp").value = "";
  document.getElementById("otp-inp").value = "";
  document.getElementById("otp-hint-txt").textContent = "----";
  document.getElementById("step-otp").style.display = "none";
  document.getElementById("step-voter").style.display = "block";
  hideEl("login-err"); hideEl("login-ok");
  if(timerInterval) clearInterval(timerInterval);
  electionEndTime = Date.now() + ELECTION_DURATION;
  localStorage.setItem(LS_TIMER, electionEndTime);
  timerInterval = setInterval(tickTimer, 1000);
  tickTimer();
  loadResults();
  goTo("login");
}

function resetTimer(){
  if(restartCountTimer) clearInterval(restartCountTimer);
  document.getElementById("modal-restart").classList.remove("open");
  fullReset();
}

function toggleTheme(){
  theme = theme==="light"?"dark":"light";
  document.body.className = theme==="dark"?"dark":"";
  document.getElementById("theme-btn").textContent = theme==="light"?"🌙 Dark":"☀️ Light";
}

var LANG = {
  "hdr-t1":  {en:"Tirupati Assembly Constituency",   te:"తిరుపతి అసెంబ్లీ నియోజకవర్గం"},
  "hdr-t2":  {en:"Online Voting System — 2024 General Elections", te:"ఆన్‌లైన్ ఓటింగ్ వ్యవస్థ — 2024 సాధారణ ఎన్నికలు"},
  "nl":      {en:"Voter Login",   te:"ఓటర్ లాగిన్"},
  "nv2":     {en:"Cast Vote",     te:"ఓటు వేయండి"},
  "nr":      {en:"Live Results",  te:"ఫలితాలు"},
  "na":      {en:"Admin Panel",   te:"అడ్మిన్"}
};
function toggleLang(){
  lang = lang==="en"?"te":"en";
  document.getElementById("lang-btn").textContent = lang==="en"?"తెలుగు":"English";
  Object.keys(LANG).forEach(function(id){
    var el=document.getElementById(id);
    if(el) el.textContent = LANG[id][lang];
  });
}

function goTo(pg){
  if(pg==="vote" && !currentVoterId){ goTo("login"); return; }
  document.querySelectorAll(".page").forEach(function(p){ p.classList.remove("active"); });
  document.querySelectorAll(".nav-btn").forEach(function(b){ b.classList.remove("active"); });
  document.getElementById("page-"+pg).classList.add("active");
  document.getElementById("nav-"+(pg==="vote"?"vote":pg)).classList.add("active");
  if(pg==="results") loadResults();
  if(pg==="admin" && localStorage.getItem(LS_ADMIN)) showDash();
}

function closeModal(id){ document.getElementById(id).classList.remove("open"); }
function hideEl(id){ var el=document.getElementById(id); if(el) el.style.display="none"; }
function showEl(id,disp){ var el=document.getElementById(id); if(el) el.style.display=disp||"flex"; }

function showErr(id, msg){
  var el=document.getElementById(id); if(!el) return;
  el.style.display="flex";
  var t=document.getElementById(id+"-txt"); if(t) t.textContent=msg;
}
function showOk(id, msg){
  var el=document.getElementById(id); if(!el) return;
  el.style.display="flex";
  var t=document.getElementById(id+"-txt"); if(t) t.textContent=msg;
}

function sendOtp(){
  hideEl("login-err"); hideEl("login-ok");

  
  if(votingClosed){
    showErr("login-err","⛔ Voting has closed. You cannot log in after the election period ends.");
    return;
  }

  var id = document.getElementById("voter-id-inp").value.trim().toUpperCase();
  tempVoterId = id;

  if(!isValidId(id)){
    showErr("login-err","Invalid Voter ID. Please enter a valid 7-character ID starting with AP followed by 5 digits (e.g. AP12345).");
    return;
  }

  
  if(hasVoted(id)){
    triggerFraudAlert(id);
    return;
  }

  var btn = document.getElementById("otp-btn");
  btn.disabled = true;
  btn.innerHTML = '<span class="spinner"></span> Sending OTP…';
  setTimeout(function(){
    
    currentOtp = String(Math.floor(1000 + Math.random() * 9000));
    btn.disabled = false;
    btn.innerHTML = "📱 Send OTP";
    document.getElementById("step-voter").style.display = "none";
    document.getElementById("step-otp").style.display   = "block";
    document.getElementById("otp-inp").focus();
    
    document.getElementById("otp-hint-txt").textContent = currentOtp;
    showOk("login-ok", "✅ OTP sent to your registered mobile number. Your OTP is: " + currentOtp);
  }, 1200);
}

function verifyOtp(){
  hideEl("login-err");
  var otp = document.getElementById("otp-inp").value.trim();

  
  if(otp.length !== 4 || !/^\d{4}$/.test(otp)){
    showErr("login-err","Please enter a valid 4-digit OTP.");
    return;
  }
  if(otp !== currentOtp){
    showErr("login-err","Incorrect OTP. Please check the OTP shown on this screen and try again.");
    return;
  }
  var btn = document.getElementById("verify-btn");
  btn.disabled = true;
  btn.innerHTML = '<span class="spinner"></span> Verifying…';
  setTimeout(function(){
    currentVoterId = tempVoterId;
    document.getElementById("vote-vid-lbl").textContent =
      "Voter ID: "+currentVoterId+" — Tirupati (SC) Assembly Constituency";
    document.getElementById("nav-vote").style.display = "inline-block";
    buildGrid();
    goTo("vote");
    btn.disabled = false;
    btn.innerHTML = "🔓 Verify & Login";
  }, 900);
}

function backToVid(){
  document.getElementById("step-otp").style.display   = "none";
  document.getElementById("step-voter").style.display = "block";
  document.getElementById("otp-inp").value = "";
  document.getElementById("otp-hint-txt").textContent = "----";
  currentOtp = "";
  hideEl("login-ok"); hideEl("login-err");
}

function triggerFraudAlert(voterId){
  
  var log   = getFraudLog();
  var entry = {
    voterId:   voterId,
    time:      new Date().toISOString(),
    ip:        randIP(),
    reported:  true
  };
  log.push(entry);
  saveFraudLog(log);

  
  document.getElementById("fraud-voter-id").textContent = voterId;
  document.getElementById("fraud-time").textContent     = new Date().toLocaleTimeString();
  document.getElementById("fraud-overlay").classList.add("open");

  
  var count = 10;
  document.getElementById("fraud-count").textContent  = count;
  document.getElementById("fraud-count2").textContent = count;
  if(fraudCountTimer) clearInterval(fraudCountTimer);
  fraudCountTimer = setInterval(function(){
    count--;
    document.getElementById("fraud-count").textContent  = count;
    document.getElementById("fraud-count2").textContent = count;
    if(count <= 0){
      clearInterval(fraudCountTimer);
      closeFraud();
    }
  }, 1000);
}

function closeFraud(){
  clearInterval(fraudCountTimer);
  document.getElementById("fraud-overlay").classList.remove("open");
  
  document.getElementById("voter-id-inp").value = "";
  document.getElementById("otp-inp").value       = "";
  document.getElementById("otp-hint-txt").textContent = "----";
  currentOtp = "";
  document.getElementById("step-otp").style.display   = "none";
  document.getElementById("step-voter").style.display = "block";
  hideEl("login-err"); hideEl("login-ok");
  goTo("login");
}

function buildGrid(){
  var g = document.getElementById("cand-grid");
  g.innerHTML = "";
  selectedCandId = null;

  CANDS.forEach(function(c){
    var d = document.createElement("div");

    if(c.isNota){
      
      d.style.cssText = "grid-column:1/-1;background:var(--card);border:2.5px dashed #dc2626;border-radius:var(--rad);padding:16px 20px;cursor:pointer;display:flex;align-items:center;gap:16px;transition:border-color .2s,box-shadow .2s;position:relative;user-select:none";
    } else {
      d.className = "cand-card";
    }

    d.id = "card-"+c.id;
    d.setAttribute("onclick","selectCand("+c.id+")");

    if(c.isNota){
      
      d.innerHTML =
        '<div id="rdot-'+c.id+'" style="position:absolute;top:12px;right:12px;width:21px;height:21px;border:2.5px solid #bbb;border-radius:50%;display:flex;align-items:center;justify-content:center;"></div>'+
        '<div style="width:62px;height:62px;border-radius:50%;display:flex;align-items:center;justify-content:center;border:3px solid #dc2626;background:#fff1f2;flex-shrink:0">'+
          SYMBOLS[c.symKey]+
        '</div>'+
        '<div>'+
          '<div style="font-weight:800;font-size:1rem;color:#dc2626">'+c.name+' — None Of The Above</div>'+
          '<div style="font-size:.78rem;color:var(--muted);margin:2px 0 5px">'+c.te+'</div>'+
          '<span class="party-tag" style="background:#fff1f2;color:#991b1b;border:1px solid #fca5a5">'+
            'I do not wish to vote for any of the above candidates'+
          '</span>'+
        '</div>';
    } else {
      d.innerHTML =
        '<div class="radio-dot" id="rdot-'+c.id+'"></div>'+
        '<div class="sym-wrap" style="border-color:'+c.col+'">'+SYMBOLS[c.symKey]+'</div>'+
        '<div class="cand-name">'+c.name+'</div>'+
        '<div class="cand-te">'+c.te+'</div>'+
        '<span class="party-tag" style="background:'+c.bbg+';color:'+c.btxt+'">'+
          c.short+' — '+c.party+'</span>';
    }

    g.appendChild(d);
  });
}

function selectCand(id){
  selectedCandId = id;
  CANDS.forEach(function(c){
    var card = document.getElementById("card-"+c.id);
    var rdot = document.getElementById("rdot-"+c.id);
    if(c.isNota){
      
      if(c.id===id){
        card.style.borderColor="#dc2626";
        card.style.boxShadow="0 6px 22px rgba(220,38,38,.18)";
        card.style.background="rgba(220,38,38,.04)";
        if(rdot){ rdot.style.borderColor="#dc2626"; rdot.style.background="#dc2626"; rdot.innerHTML='<span style="width:8px;height:8px;background:#fff;border-radius:50%;display:block"></span>'; }
      } else {
        card.style.borderColor="#dc2626";
        card.style.boxShadow="none";
        card.style.background="var(--card)";
        if(rdot){ rdot.style.borderColor="#bbb"; rdot.style.background="transparent"; rdot.innerHTML=""; }
      }
    } else {
      card.classList[c.id===id?"add":"remove"]("sel");
    }
  });
  hideEl("vote-err");
}

function castVote(){
  if(votingClosed){
    showEl("vote-err","flex");
    document.getElementById("vote-err").innerHTML =
      "⛔ Voting has closed. The election period has ended.";
    return;
  }
  if(!selectedCandId){
    showEl("vote-err","flex");
    document.getElementById("vote-err").innerHTML =
      "⚠️ Please select a candidate before submitting your vote.";
    return;
  }
  var c = CANDS.find(function(x){ return x.id===selectedCandId; });
  var prev = document.getElementById("modal-preview");
  prev.style.borderColor = c.col;
  prev.innerHTML =
    '<div style="margin:0 auto 8px;width:72px;height:72px;border-radius:50%;background:var(--card);border:3px solid '+c.col+';display:flex;align-items:center;justify-content:center">'+
    SYMBOLS[c.symKey]+'</div>'+
    '<div class="fw8" style="font-size:1.05rem;margin-bottom:3px">'+c.name+'</div>'+
    '<div style="font-size:.77rem;color:var(--muted);margin-bottom:7px">'+c.te+'</div>'+
    '<span class="party-tag" style="background:'+c.bbg+';color:'+c.btxt+'">'+c.party+'</span>';
  document.getElementById("modal-confirm").classList.add("open");
}

function submitVote(){
  var btn = document.getElementById("confirm-btn");
  btn.disabled = true;
  btn.innerHTML = '<span class="spinner"></span> Recording…';

  
  setTimeout(function(){
    
    var votes = getVotes();
    votes[selectedCandId] = (votes[selectedCandId]||0)+1;
    saveVotes(votes);
    markVoted(currentVoterId);

    closeModal("modal-confirm");

    
    document.getElementById("success-label").textContent =
      "Voter ID: "+currentVoterId+" — Vote encrypted & stored securely.";
    document.getElementById("modal-success").classList.add("open");
    shootConfetti();

    btn.disabled = false;
    btn.innerHTML = "✅ Confirm Vote";

    
    loadResults();

    
    startRedirectCountdown(5);
  }, 1500);
}

function startRedirectCountdown(secs){
  var total  = secs;
  var left   = secs;
  var circle = document.getElementById("ring-circle");
  var full   = 220; 

  document.getElementById("ring-num").textContent      = left;
  document.getElementById("redirect-count").textContent = left;
  circle.style.strokeDashoffset = 0;

  if(redirectTimer) clearInterval(redirectTimer);
  redirectTimer = setInterval(function(){
    left--;
    document.getElementById("ring-num").textContent      = left;
    document.getElementById("redirect-count").textContent = left;
    
    var offset = ((total - left) / total) * full;
    circle.style.strokeDashoffset = offset;

    if(left <= 0){
      clearInterval(redirectTimer);
      doRedirect();
    }
  }, 1000);
}

function doRedirect(){
  if(redirectTimer) clearInterval(redirectTimer);
  closeModal("modal-success");
  
  selectedCandId = null;
  currentVoterId = null;
  currentOtp     = "";
  document.getElementById("nav-vote").style.display = "none";
  
  document.getElementById("voter-id-inp").value = "";
  document.getElementById("otp-inp").value       = "";
  document.getElementById("otp-hint-txt").textContent = "----";
  document.getElementById("step-otp").style.display   = "none";
  document.getElementById("step-voter").style.display = "block";
  hideEl("login-err"); hideEl("login-ok");
  goTo("login");
}

function statBox(val,lbl){
  return '<div class="stat-box"><div class="stat-val">'+val+'</div><div class="stat-lbl">'+lbl+'</div></div>';
}

function symIcon(symKey, size){
  size = size||28;
  var svgStr = SYMBOLS[symKey];
  
  return svgStr.replace(/width="44"/g,'width="'+size+'"').replace(/height="44"/g,'height="'+size+'"');
}

function loadResults(){
  var res    = computeResults();
  var total  = res.length>0 ? res[0].total : 0;
  var turnout= total>0 ? ((total/1000)*100).toFixed(1) : "0.0";
  var medals = ["🥇","🥈","🥉","4️⃣","5️⃣"];

  
  if(votingClosed){
    document.getElementById("results-title").textContent = "Final Election Results";
    var fbw = document.getElementById("final-banner-wrap");
    fbw.style.display = "block";
    var winner = res[0];
    if(winner && winner.votes>0){
      document.getElementById("winner-card").innerHTML =
        '<div class="winner-sym">'+SYMBOLS[winner.symKey]+'</div>'+
        '<div style="font-size:.78rem;color:var(--muted);font-weight:700;letter-spacing:.08em;text-transform:uppercase;margin-top:6px">🏆 WINNER</div>'+
        '<div class="winner-name">'+winner.name+'</div>'+
        '<div class="winner-party">'+winner.party+'</div>'+
        '<div class="winner-votes">'+winner.votes+' votes</div>'+
        '<div class="winner-pct">'+winner.pct+'% of total votes cast</div>';
    } else {
      document.getElementById("winner-card").innerHTML =
        '<div style="padding:20px;color:var(--muted)">No votes were cast during the election period.</div>';
    }
  }

  
  document.getElementById("res-stats").innerHTML =
    statBox(total,    "Total Votes Cast") +
    statBox(turnout+"%", "Voter Turnout") +
    statBox(res[0]&&res[0].votes>0?res[0].short:"—", "Leading Party") +
    statBox(Object.keys(getVoters()).length, "Unique Voters");

  
  var lb = "";
  res.forEach(function(r,i){
    lb += '<div class="ldr-row r'+(i+1)+'">'+
      '<span class="ldr-medal">'+medals[i]+'</span>'+
      '<div class="ldr-sym">'+symIcon(r.symKey,22)+'</div>'+
      '<div class="ldr-info">'+
        '<div class="ldr-name">'+r.name+'</div>'+
        '<div class="ldr-party">'+r.party+'</div>'+
      '</div>'+
      '<div style="text-align:right">'+
        '<div class="ldr-votes">'+r.votes+'</div>'+
        '<div class="ldr-pct">'+r.pct+'%</div>'+
      '</div>'+
    '</div>';
  });
  document.getElementById("leaderboard").innerHTML = lb;

  
  var bars = "";
  res.forEach(function(r){
    bars += '<div class="bar-wrap">'+
      '<div class="bar-label">'+
        '<span>'+symIcon(r.symKey,16)+' <strong>'+r.name+'</strong> '+
        '<span class="party-tag" style="background:'+r.bbg+';color:'+r.btxt+'">'+r.short+'</span></span>'+
        '<span class="fw8">'+r.votes+' votes ('+r.pct+'%)</span>'+
      '</div>'+
      '<div class="bar-bg"><div class="bar-fill" style="width:'+r.pct+'%;background:linear-gradient(90deg,'+r.col+'aa,'+r.col+')">'+
        (parseFloat(r.pct)>8?r.pct+"%":"")+
      '</div></div></div>';
  });
  document.getElementById("res-bars").innerHTML = bars;

  
  var rows = "";
  res.forEach(function(r,i){
    var status = i===0&&r.votes>0
      ? '<span style="color:gold;font-weight:800">🥇 Winner</span>'
      : i===1&&r.votes>0
      ? '<span style="color:silver;font-weight:800">🥈 Runner-up</span>'
      : '<span style="color:var(--muted)">#'+(i+1)+'</span>';
    rows += '<tr>'+
      '<td class="fw7">'+(i+1)+'</td>'+
      '<td><div style="width:36px;height:36px;border-radius:50%;background:var(--bg);border:2px solid '+r.col+';display:flex;align-items:center;justify-content:center">'+symIcon(r.symKey,22)+'</div></td>'+
      '<td><div class="fw8">'+r.name+'</div><div style="font-size:.72rem;color:var(--muted)">'+r.te+'</div></td>'+
      '<td><span class="party-tag" style="background:'+r.bbg+';color:'+r.btxt+'">'+r.short+'</span></td>'+
      '<td style="font-family:\'Playfair Display\',serif;font-size:1.1rem;font-weight:900">'+r.votes+'</td>'+
      '<td>'+r.pct+'%</td>'+
      '<td>'+status+'</td>'+
    '</tr>';
  });
  document.getElementById("res-table").innerHTML = rows;

  renderCharts(res);
}

function renderCharts(res){
  if(barChart){ barChart.destroy(); barChart=null; }
  if(pieChart){ pieChart.destroy(); pieChart=null; }
  var labels  = res.map(function(r){ return r.short; });
  var votes   = res.map(function(r){ return r.votes; });
  var colors  = res.map(function(r){ return r.col+"cc"; });
  var borders = res.map(function(r){ return r.col; });
  var hasVotes= votes.some(function(v){ return v>0; });

  barChart = new Chart(document.getElementById("bar-chart").getContext("2d"),{
    type:"bar",
    data:{labels:labels, datasets:[{label:"Votes",data:votes,backgroundColor:colors,borderColor:borders,borderWidth:2,borderRadius:7}]},
    options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true,ticks:{stepSize:1}}}}
  });
  pieChart = new Chart(document.getElementById("pie-chart").getContext("2d"),{
    type:"doughnut",
    data:{labels:labels, datasets:[{data:hasVotes?votes:res.map(function(){return 1;}),backgroundColor:colors,borderColor:borders,borderWidth:2}]},
    options:{responsive:true,plugins:{legend:{position:"bottom",labels:{font:{family:"Mukta"},padding:11}}}}
  });
}

function toggleAuto(on){
  if(autoId){ clearInterval(autoId); autoId=null; }
  if(on){ autoId = setInterval(loadResults, 8000); }
}

function adminLogin(){
  hideEl("admin-err");
  var em = document.getElementById("adm-email").value.trim();
  var pw = document.getElementById("adm-pass").value.trim();
  if(em!==ADMIN_EMAIL || pw!==ADMIN_PASS){
    var el=document.getElementById("admin-err"); el.style.display="flex";
    document.getElementById("admin-err-txt").textContent="Invalid admin credentials.";
    return;
  }
  var btn = document.getElementById("adm-btn");
  btn.disabled = true;
  btn.innerHTML = '<span class="spinner"></span> Verifying…';
  setTimeout(function(){
    localStorage.setItem(LS_ADMIN,"1");
    showDash();
    btn.disabled = false;
    btn.innerHTML = "🔐 Login";
  }, 900);
}

function showDash(){
  document.getElementById("admin-login-wrap").style.display = "none";
  document.getElementById("admin-dash").style.display       = "block";
  adminRefresh();
}

function adminLogout(){
  localStorage.removeItem(LS_ADMIN);
  document.getElementById("admin-dash").style.display       = "none";
  document.getElementById("admin-login-wrap").style.display = "block";
  document.getElementById("adm-email").value = "";
  document.getElementById("adm-pass").value  = "";
}

function adminRefresh(){
  var res     = computeResults();
  var total   = res.length>0 ? res[0].total : 0;
  var voters  = Object.keys(getVoters()).length;
  var turnout = total>0 ? ((total/1000)*100).toFixed(1) : "0.0";
  var remaining = Math.max(0, electionEndTime - Date.now());
  var h=Math.floor(remaining/3600000), m=Math.floor((remaining%3600000)/60000), s=Math.floor((remaining%60000)/1000);

  document.getElementById("adm-stats").innerHTML =
    statBox(total,      "Total Votes") +
    statBox(voters,     "Unique Voters") +
    statBox("1,000",    "Registered (Demo)") +
    statBox(turnout+"%","Voter Turnout") +
    statBox(getFraudLog().length, "Fraud Attempts") +
    statBox(votingClosed?"CLOSED":pad(h)+":"+pad(m)+":"+pad(s), "Election Status");

  var rows = "";
  res.forEach(function(r,i){
    rows += '<tr>'+
      '<td class="fw7">'+(i+1)+'</td>'+
      '<td><div style="width:32px;height:32px;border-radius:50%;border:2px solid '+r.col+';display:flex;align-items:center;justify-content:center;background:var(--bg)">'+symIcon(r.symKey,20)+'</div></td>'+
      '<td class="fw7">'+r.name+'</td>'+
      '<td><span class="party-tag" style="background:'+r.bbg+';color:'+r.btxt+'">'+r.short+'</span></td>'+
      '<td style="font-family:\'Playfair Display\',serif;font-size:1.05rem;font-weight:900">'+r.votes+'</td>'+
      '<td>'+r.pct+'%</td>'+
      '<td><div style="height:10px;background:#e9ecef;border-radius:10px;overflow:hidden;min-width:80px">'+
        '<div style="height:100%;width:'+r.pct+'%;background:linear-gradient(90deg,'+r.col+'99,'+r.col+');border-radius:10px;transition:width .8s"></div>'+
      '</div></td></tr>';
  });
  document.getElementById("adm-table").innerHTML = rows;

  
  var fraudLog = getFraudLog();
  if(fraudLog.length===0){
    document.getElementById("fraud-log-table").innerHTML =
      '<tr><td colspan="5" style="text-align:center;color:var(--muted);padding:18px">✅ No fraud attempts recorded.</td></tr>';
  } else {
    var frows = "";
    fraudLog.forEach(function(f,i){
      frows += '<tr>'+
        '<td class="fw7">'+(i+1)+'</td>'+
        '<td style="font-family:monospace;color:#dc2626;font-weight:800">'+f.voterId+'</td>'+
        '<td>'+new Date(f.time).toLocaleString()+'</td>'+
        '<td style="font-family:monospace">'+f.ip+'</td>'+
        '<td><span style="background:#fee2e2;color:#991b1b;padding:2px 8px;border-radius:10px;font-size:.72rem;font-weight:800">🚨 REPORTED</span></td>'+
      '</tr>';
    });
    document.getElementById("fraud-log-table").innerHTML = frows;
  }
}

function confirmReset(){ document.getElementById("modal-reset").classList.add("open"); }

function doReset(){
  localStorage.removeItem(LS_VOTES);
  localStorage.removeItem(LS_VOTERS);
  localStorage.removeItem(LS_FRAUD);
  closeModal("modal-reset");
  adminRefresh();
  var ok = document.getElementById("adm-ok");
  document.getElementById("adm-ok-txt").textContent = "✅ All votes, voter records and fraud logs have been reset.";
  ok.style.display = "flex";
  setTimeout(function(){ ok.style.display="none"; }, 4000);
}

function downloadCSV(){
  var res = computeResults();
  var csv = "Rank,Candidate,Party,Symbol,Votes,Percentage\n";
  res.forEach(function(r,i){
    csv += (i+1)+',"'+r.name+'","'+r.party+'","'+r.short+'",'+r.votes+','+r.pct+'%\n';
  });
  
  var fl = getFraudLog();
  if(fl.length>0){
    csv += "\n\nFRAUD LOG\nVoterID,Time,IP\n";
    fl.forEach(function(f){
      csv += f.voterId+","+f.time+","+f.ip+"\n";
    });
  }
  var blob = new Blob([csv],{type:"text/csv"});
  var url  = URL.createObjectURL(blob);
  var a    = document.createElement("a");
  a.href=url; a.download="tirupati_results_"+new Date().toISOString().slice(0,10)+".csv";
  document.body.appendChild(a); a.click(); document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

function shootConfetti(){
  var o = {origin:{y:0.6}};
  confetti(Object.assign({},o,{particleCount:55,spread:26,startVelocity:55,colors:["#FF9933","#ffffff","#138808"]}));
  confetti(Object.assign({},o,{particleCount:44,spread:60,colors:["#FF9933","#138808"]}));
  confetti(Object.assign({},o,{particleCount:77,spread:100,decay:.91,scalar:.8,colors:["#ffffff","#003366","#FF9933"]}));
  confetti(Object.assign({},o,{particleCount:22,spread:120,startVelocity:25,decay:.92,scalar:1.2}));
}

window.onload = function(){
  initTimer();
  goTo("login");
};

window.sendOtp       = sendOtp;
window.verifyOtp     = verifyOtp;
window.backToVid     = backToVid;
window.selectCand    = selectCand;
window.castVote      = castVote;
window.submitVote    = submitVote;
window.doRedirect    = doRedirect;
window.goTo          = goTo;
window.toggleTheme   = toggleTheme;
window.toggleLang    = toggleLang;
window.loadResults   = loadResults;
window.toggleAuto    = toggleAuto;
window.adminLogin    = adminLogin;
window.adminLogout   = adminLogout;
window.adminRefresh  = adminRefresh;
window.confirmReset  = confirmReset;
window.doReset       = doReset;
window.downloadCSV   = downloadCSV;
window.closeModal    = closeModal;
window.closeFraud    = closeFraud;
window.resetTimer    = resetTimer;
window.fullReset     = fullReset;
</script>
</body>
</html>
