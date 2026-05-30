<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AtharvaOS</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.19.0/dist/tabler-icons.min.css">
<style>
*{box-sizing:border-box;margin:0;padding:0;font-family:'Segoe UI',system-ui,sans-serif}
body{background:#0d1117;overflow:hidden;width:100vw;height:100vh}
#boot-screen{position:fixed;inset:0;background:#0d1117;display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:9999;transition:opacity 0.6s}
.boot-logo{width:80px;height:80px;border-radius:20px;background:rgba(48,211,120,0.15);border:2px solid rgba(48,211,120,0.4);display:flex;align-items:center;justify-content:center;font-size:40px;color:#30d378;margin-bottom:24px;animation:pulse 1.5s ease infinite}
@keyframes pulse{0%,100%{box-shadow:0 0 0 0 rgba(48,211,120,0.3)}50%{box-shadow:0 0 0 16px rgba(48,211,120,0)}}
.boot-title{color:#c9d1d9;font-size:22px;font-weight:500;margin-bottom:6px}
.boot-sub{color:#6e7681;font-size:13px;margin-bottom:32px}
.boot-bar-track{width:240px;height:3px;background:rgba(255,255,255,0.08);border-radius:4px;overflow:hidden}
.boot-bar-fill{height:100%;background:#30d378;border-radius:4px;animation:bootLoad 2.2s ease forwards}
@keyframes bootLoad{0%{width:0%}100%{width:100%}}
#desktop{position:fixed;inset:0;background:#0d1117;overflow:hidden}
#wallpaper{position:absolute;inset:0;background:linear-gradient(135deg,#0d1117 0%,#161b22 50%,#0d1117 100%);background-image:repeating-linear-gradient(0deg,transparent,transparent 39px,rgba(0,255,100,0.03) 40px),repeating-linear-gradient(90deg,transparent,transparent 39px,rgba(0,255,100,0.03) 40px)}
#taskbar{position:absolute;bottom:0;left:0;right:0;height:46px;background:rgba(13,17,23,0.97);border-top:1px solid #21262d;display:flex;align-items:center;padding:0 10px;gap:4px;z-index:1000;backdrop-filter:blur(12px)}
#start-btn{height:32px;padding:0 12px;border-radius:8px;border:1px solid rgba(48,211,120,0.3);background:rgba(48,211,120,0.08);color:#30d378;font-size:13px;font-weight:500;cursor:pointer;display:flex;align-items:center;gap:6px;transition:all 0.15s;white-space:nowrap}
#start-btn:hover{background:rgba(48,211,120,0.15);border-color:rgba(48,211,120,0.5)}
.tbtn{height:30px;padding:0 10px;border-radius:6px;border:1px solid transparent;background:transparent;color:#8b949e;font-size:12px;cursor:pointer;display:flex;align-items:center;gap:5px;transition:all 0.12s;white-space:nowrap;max-width:130px;overflow:hidden;text-overflow:ellipsis}
.tbtn:hover{background:rgba(255,255,255,0.07);color:#c9d1d9}
.tbtn.active{background:rgba(48,211,120,0.1);border-color:rgba(48,211,120,0.25);color:#30d378}
#clock-area{margin-left:auto;text-align:right;flex-shrink:0}
#clock-time{color:#c9d1d9;font-size:13px;font-weight:500}
#clock-date{color:#6e7681;font-size:10px}
#start-menu{position:absolute;bottom:50px;left:10px;width:230px;background:#161b22;border:1px solid #30363d;border-radius:12px;padding:10px;z-index:2000;display:none;box-shadow:0 8px 32px rgba(0,0,0,0.6)}
.sm-header{padding:6px 8px 8px;font-size:10px;color:#6e7681;text-transform:uppercase;letter-spacing:0.8px}
.sm-item{display:flex;align-items:center;gap:10px;padding:8px 10px;border-radius:8px;color:#c9d1d9;cursor:pointer;font-size:13px;transition:background 0.1s}
.sm-item:hover{background:rgba(255,255,255,0.07)}
.sm-icon{width:30px;height:30px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:15px;flex-shrink:0}
.sm-divider{border:none;border-top:1px solid #21262d;margin:6px 0}
.di{position:absolute;display:flex;flex-direction:column;align-items:center;gap:5px;cursor:pointer;padding:8px 6px;border-radius:10px;transition:background 0.15s;width:76px}
.di:hover{background:rgba(255,255,255,0.07)}
.di-icon{width:46px;height:46px;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:24px}
.di span{color:#c9d1d9;font-size:10px;text-align:center;line-height:1.3}
.window{position:absolute;background:#161b22;border:1px solid #30363d;border-radius:12px;box-shadow:0 8px 40px rgba(0,0,0,0.7);min-width:300px;overflow:hidden;display:flex;flex-direction:column}
.window.focused{border-color:#484f58;box-shadow:0 12px 48px rgba(0,0,0,0.8)}
.win-tb{background:#21262d;padding:9px 12px;display:flex;align-items:center;gap:8px;cursor:move;border-bottom:1px solid #30363d;flex-shrink:0;user-select:none}
.win-dots{display:flex;gap:7px}
.wd{width:13px;height:13px;border-radius:50%;cursor:pointer;transition:filter 0.1s}
.wd:hover{filter:brightness(1.3)}
.wd-r{background:#f85149}
.wd-y{background:#d2992a}
.wd-g{background:#30d378}
.win-t{color:#8b949e;font-size:12px;margin-left:2px;flex:1}
.win-body{padding:16px;overflow-y:auto;flex:1}
.notif{position:absolute;top:14px;right:14px;background:#21262d;border:1px solid #30363d;border-radius:10px;padding:12px 15px;max-width:240px;z-index:3000;animation:notifIn 0.3s ease}
@keyframes notifIn{from{opacity:0;transform:translateX(20px)}to{opacity:1;transform:translateX(0)}}
.notif-t{color:#c9d1d9;font-size:13px;font-weight:500}
.notif-b{color:#6e7681;font-size:12px;margin-top:3px}
.profile-stat{display:flex;flex-direction:column;align-items:center;padding:10px 8px;background:rgba(255,255,255,0.04);border-radius:8px}
.stat-n{font-size:20px;font-weight:500;color:#30d378}
.stat-l{font-size:11px;color:#6e7681;margin-top:2px}
.fi{display:flex;align-items:center;gap:8px;padding:7px 10px;border-radius:7px;cursor:pointer;transition:background 0.1s}
.fi:hover{background:rgba(255,255,255,0.05)}
.repo-card{background:rgba(255,255,255,0.04);border:1px solid #30363d;border-radius:9px;padding:13px;margin-bottom:8px;cursor:pointer;transition:border-color 0.15s}
.repo-card:hover{border-color:#484f58}
.skill-track{background:rgba(255,255,255,0.06);border-radius:4px;height:6px;overflow:hidden;margin-top:4px}
.skill-fill{height:100%;border-radius:4px}
#term-out{font-family:'Cascadia Code','Fira Code',monospace;font-size:12px;line-height:1.75;min-height:220px}
.term-row{display:flex;align-items:flex-start}
.t-prompt{color:#30d378;margin-right:6px;flex-shrink:0;font-family:inherit}
.t-cmd{color:#c9d1d9;font-family:inherit}
.t-out{font-family:inherit}
.t-green{color:#30d378}.t-blue{color:#388bfd}.t-red{color:#f85149}.t-yellow{color:#d2992a}.t-muted{color:#6e7681}
#term-input-wrap{display:flex;align-items:center;margin-top:8px;border-top:1px solid #21262d;padding-top:8px}
#term-input{background:transparent;border:none;outline:none;color:#c9d1d9;font-family:'Cascadia Code','Fira Code',monospace;font-size:12px;flex:1;caret-color:#30d378}
.calc-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:7px}
.cb{padding:14px 8px;border-radius:8px;border:none;font-size:16px;cursor:pointer;font-family:'Cascadia Code','Fira Code',monospace;transition:filter 0.1s}
.cb:hover{filter:brightness(1.2)}
.cb:active{transform:scale(0.95)}
.cb-num{background:rgba(255,255,255,0.07);color:#c9d1d9}
.cb-op{background:rgba(48,211,120,0.15);color:#30d378}
.cb-clr{background:rgba(248,81,73,0.12);color:#f85149}
.cb-wide{grid-column:span 2}
</style>
</head>
<body>

<div id="boot-screen">
  <div class="boot-logo"><i class="ti ti-shield-lock"></i></div>
  <div class="boot-title">AtharvaOS</div>
  <div class="boot-sub">by Atharva Mani Tripathi · AtharvaSecurity</div>
  <div class="boot-bar-track"><div class="boot-bar-fill"></div></div>
</div>

<div id="desktop" style="display:none">
  <div id="wallpaper"></div>

  <!-- Desktop icons -->
  <div class="di" style="top:20px;left:20px" ondblclick="openWin('profile')">
    <div class="di-icon" style="background:rgba(48,211,120,0.12);color:#30d378"><i class="ti ti-user-circle"></i></div>
    <span>Profile</span>
  </div>
  <div class="di" style="top:20px;left:106px" ondblclick="openWin('files')">
    <div class="di-icon" style="background:rgba(210,153,34,0.12);color:#d2992a"><i class="ti ti-folder-filled"></i></div>
    <span>Projects</span>
  </div>
  <div class="di" style="top:20px;left:192px" ondblclick="openWin('terminal')">
    <div class="di-icon" style="background:rgba(48,211,120,0.1);color:#30d378"><i class="ti ti-terminal-2"></i></div>
    <span>Terminal</span>
  </div>
  <div class="di" style="top:20px;left:278px" ondblclick="openWin('browser')">
    <div class="di-icon" style="background:rgba(56,139,253,0.12);color:#388bfd"><i class="ti ti-world"></i></div>
    <span>GitHub</span>
  </div>
  <div class="di" style="top:20px;left:364px" ondblclick="openWin('skills')">
    <div class="di-icon" style="background:rgba(163,113,247,0.12);color:#a371f7"><i class="ti ti-code"></i></div>
    <span>Skills</span>
  </div>
  <div class="di" style="top:20px;left:450px" ondblclick="openWin('calc')">
    <div class="di-icon" style="background:rgba(56,189,248,0.12);color:#38bdf8"><i class="ti ti-calculator"></i></div>
    <span>Calculator</span>
  </div>
  <div class="di" style="top:20px;left:536px" ondblclick="openWin('about')">
    <div class="di-icon" style="background:rgba(248,81,73,0.12);color:#f85149"><i class="ti ti-info-circle"></i></div>
    <span>About OS</span>
  </div>

  <!-- Start menu -->
  <div id="start-menu">
    <div class="sm-header">Applications</div>
    <div class="sm-item" onclick="openWin('profile');hideStart()"><div class="sm-icon" style="background:rgba(48,211,120,0.12);color:#30d378"><i class="ti ti-user-circle"></i></div>Profile</div>
    <div class="sm-item" onclick="openWin('files');hideStart()"><div class="sm-icon" style="background:rgba(210,153,34,0.12);color:#d2992a"><i class="ti ti-folder"></i></div>Projects</div>
    <div class="sm-item" onclick="openWin('terminal');hideStart()"><div class="sm-icon" style="background:rgba(48,211,120,0.1);color:#30d378"><i class="ti ti-terminal-2"></i></div>Terminal</div>
    <div class="sm-item" onclick="openWin('browser');hideStart()"><div class="sm-icon" style="background:rgba(56,139,253,0.12);color:#388bfd"><i class="ti ti-brand-github"></i></div>GitHub Browser</div>
    <div class="sm-item" onclick="openWin('skills');hideStart()"><div class="sm-icon" style="background:rgba(163,113,247,0.12);color:#a371f7"><i class="ti ti-code"></i></div>Skills</div>
    <div class="sm-item" onclick="openWin('calc');hideStart()"><div class="sm-icon" style="background:rgba(56,189,248,0.12);color:#38bdf8"><i class="ti ti-calculator"></i></div>Calculator</div>
    <div class="sm-item" onclick="openWin('about');hideStart()"><div class="sm-icon" style="background:rgba(248,81,73,0.12);color:#f85149"><i class="ti ti-info-circle"></i></div>About AtharvaOS</div>
    <hr class="sm-divider">
    <div class="sm-item" style="color:#f85149" onclick="shutdown()"><div class="sm-icon" style="background:rgba(248,81,73,0.1);color:#f85149"><i class="ti ti-power"></i></div>Shut Down</div>
  </div>

  <!-- Taskbar -->
  <div id="taskbar">
    <button id="start-btn" onclick="toggleStart()">
      <i class="ti ti-terminal" style="font-size:15px"></i> AtharvaOS
    </button>
    <div id="open-apps" style="display:flex;gap:4px;overflow:hidden"></div>
    <div id="clock-area">
      <div id="clock-time"></div>
      <div id="clock-date"></div>
    </div>
  </div>
</div>

<script>
// Boot
setTimeout(()=>{
  document.getElementById('boot-screen').style.opacity='0';
  setTimeout(()=>{
    document.getElementById('boot-screen').style.display='none';
    document.getElementById('desktop').style.display='block';
    setTimeout(()=>showNotif('Welcome to AtharvaOS!','Double-click any icon to open an app.'),600);
  },600);
},2600);

// Clock
function updateClock(){
  const n=new Date();
  document.getElementById('clock-time').textContent=n.toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'});
  document.getElementById('clock-date').textContent=n.toLocaleDateString([],{weekday:'short',month:'short',day:'numeric'});
}
updateClock(); setInterval(updateClock,1000);

// Start menu
function toggleStart(){const m=document.getElementById('start-menu');m.style.display=m.style.display==='block'?'none':'block';}
function hideStart(){document.getElementById('start-menu').style.display='none';}
document.addEventListener('click',e=>{if(!e.target.closest('#start-menu')&&!e.target.closest('#start-btn'))hideStart();});

function shutdown(){
  hideStart();
  showNotif('Shutting down...','Goodbye, Atharva! Keep hacking. 🔐');
  setTimeout(()=>{document.body.innerHTML='<div style="background:#0d1117;width:100vw;height:100vh;display:flex;align-items:center;justify-content:center;color:#30d378;font-size:18px;font-family:monospace">AtharvaOS has shut down. Refresh to restart.</div>';},2000);
}

// Notifications
function showNotif(title,body){
  const old=document.getElementById('notif-box');if(old)old.remove();
  const n=document.createElement('div');n.id='notif-box';n.className='notif';
  n.innerHTML=`<div class="notif-t">${title}</div><div class="notif-b">${body}</div>`;
  document.getElementById('desktop').appendChild(n);
  setTimeout(()=>{if(n.parentNode)n.remove();},3500);
}

// Window management
const wins={};
let zTop=100;
const winOffsets={x:70,y:55,count:0};

function openWin(id){
  hideStart();
  if(wins[id]){
    const w=wins[id];
    w.el.style.display='flex';
    focusWin(id);
    return;
  }
  const cfg=getWinCfg(id);
  const el=document.createElement('div');
  el.className='window focused';
  el.id='w-'+id;
  const lft=winOffsets.x+winOffsets.count*28;
  const tp=winOffsets.y+winOffsets.count*22;
  winOffsets.count++;
  el.style.cssText=`left:${lft}px;top:${tp}px;width:${cfg.w}px;max-height:${cfg.h}px;z-index:${++zTop}`;
  el.innerHTML=`<div class="win-tb">
    <div class="win-dots">
      <div class="wd wd-r" onclick="closeWin('${id}')"></div>
      <div class="wd wd-y" onclick="minimizeWin('${id}')"></div>
      <div class="wd wd-g"></div>
    </div>
    <div class="win-t">${cfg.title}</div>
  </div>
  <div class="win-body" id="wb-${id}">${cfg.html}</div>`;
  document.getElementById('desktop').appendChild(el);
  wins[id]={el,title:cfg.title,minimized:false};
  makeDraggable(el);
  addTBtn(id,cfg.title);
  el.addEventListener('mousedown',()=>focusWin(id));
  if(cfg.onOpen)cfg.onOpen();
}

function focusWin(id){
  Object.values(wins).forEach(w=>{w.el.classList.remove('focused');});
  if(wins[id]){wins[id].el.classList.add('focused');wins[id].el.style.zIndex=++zTop;}
  document.querySelectorAll('.tbtn').forEach(b=>b.classList.remove('active'));
  const tb=document.getElementById('tb-'+id);if(tb)tb.classList.add('active');
}
function closeWin(id){
  if(wins[id]){wins[id].el.remove();delete wins[id];}
  const tb=document.getElementById('tb-'+id);if(tb)tb.remove();
}
function minimizeWin(id){
  if(!wins[id])return;
  const w=wins[id];
  w.minimized=!w.minimized;
  w.el.style.display=w.minimized?'none':'flex';
  const tb=document.getElementById('tb-'+id);
  if(tb){tb.style.opacity=w.minimized?'0.5':'1';}
}
function addTBtn(id,title){
  const bar=document.getElementById('open-apps');
  const btn=document.createElement('button');
  btn.className='tbtn active';btn.id='tb-'+id;
  btn.innerHTML=`<i class="ti ti-app-window" style="font-size:12px"></i>${title.split('—')[0].trim().slice(0,14)}`;
  btn.onclick=()=>{if(wins[id]){if(wins[id].minimized){wins[id].el.style.display='flex';wins[id].minimized=false;}focusWin(id);}};
  bar.appendChild(btn);
}
function makeDraggable(el){
  const tb=el.querySelector('.win-tb');
  let dragging=false,ox=0,oy=0,sx=0,sy=0;
  tb.addEventListener('mousedown',e=>{
    dragging=true;sx=e.clientX;sy=e.clientY;
    const r=el.getBoundingClientRect();ox=r.left;oy=r.top;e.preventDefault();
  });
  document.addEventListener('mousemove',e=>{
    if(!dragging)return;
    el.style.left=(ox+e.clientX-sx)+'px';el.style.top=(oy+e.clientY-sy)+'px';
  });
  document.addEventListener('mouseup',()=>dragging=false);
}

// Window contents
function getWinCfg(id){
  const configs={
    profile:{title:'Profile — Atharva Mani Tripathi',w:420,h:520,html:`
      <div style="text-align:center;margin-bottom:18px">
        <div style="width:72px;height:72px;border-radius:50%;background:rgba(48,211,120,0.12);border:2px solid rgba(48,211,120,0.35);display:flex;align-items:center;justify-content:center;margin:0 auto 12px;font-size:32px;color:#30d378"><i class="ti ti-shield-lock"></i></div>
        <div style="color:#c9d1d9;font-size:18px;font-weight:500">Atharva Mani Tripathi</div>
        <div style="color:#30d378;font-size:13px;margin-top:3px">@AtharvaSecurity</div>
        <div style="color:#6e7681;font-size:12px;margin-top:8px;line-height:1.6">Class 11 Student · Python Developer<br>Cybersecurity Learner · Building Projects Daily</div>
      </div>
      <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:18px">
        <div class="profile-stat"><div class="stat-n">3</div><div class="stat-l">Repos</div></div>
        <div class="profile-stat"><div class="stat-n">3</div><div class="stat-l">Followers</div></div>
        <div class="profile-stat"><div class="stat-n">7</div><div class="stat-l">Following</div></div>
      </div>
      <div style="background:rgba(255,255,255,0.04);border-radius:9px;padding:13px;margin-bottom:14px">
        <div style="color:#8b949e;font-size:11px;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:10px">Current Goals 2026</div>
        ${['Improve Python programming skills','Learn Linux fundamentals','Learn networking basics','Build useful open-source projects','Strengthen cybersecurity knowledge','Grow GitHub portfolio'].map(g=>`<div style="display:flex;align-items:center;gap:7px;padding:4px 0;color:#c9d1d9;font-size:12px"><i class="ti ti-check" style="color:#30d378;font-size:12px;flex-shrink:0"></i>${g}</div>`).join('')}
      </div>
      <div style="display:flex;gap:8px">
        <a href="https://github.com/AtharvaSecurity" style="flex:1;text-align:center;padding:8px;background:rgba(255,255,255,0.05);border:1px solid #30363d;border-radius:7px;color:#c9d1d9;font-size:12px;text-decoration:none;display:flex;align-items:center;justify-content:center;gap:6px"><i class="ti ti-brand-github" style="font-size:14px"></i>GitHub</a>
        <a href="https://www.youtube.com/@AtharvaSecurity" style="flex:1;text-align:center;padding:8px;background:rgba(255,255,255,0.05);border:1px solid #30363d;border-radius:7px;color:#c9d1d9;font-size:12px;text-decoration:none;display:flex;align-items:center;justify-content:center;gap:6px"><i class="ti ti-brand-youtube" style="color:#f85149;font-size:14px"></i>YouTube</a>
      </div>`},

    files:{title:'Projects — File Manager',w:400,h:420,html:`
      <div style="display:flex;align-items:center;gap:6px;margin-bottom:12px;background:rgba(255,255,255,0.04);border-radius:7px;padding:7px 11px">
        <i class="ti ti-home" style="color:#6e7681;font-size:13px"></i>
        <span style="color:#6e7681;font-size:12px">/home/atharva/</span>
      </div>
      <div style="color:#6e7681;font-size:10px;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:6px;padding:0 10px">Repositories</div>
      <div class="fi" ondblclick="openWin('repo_deauth')">
        <i class="ti ti-folder-filled" style="color:#d2992a;font-size:18px;width:22px"></i>
        <div style="flex:1"><div style="color:#c9d1d9;font-size:13px">DeauthDevil</div><div style="color:#6e7681;font-size:11px">WiFi security tool · Python</div></div>
        <i class="ti ti-chevron-right" style="color:#6e7681;font-size:12px"></i>
      </div>
      <div class="fi" ondblclick="openWin('repo_calc')">
        <i class="ti ti-folder-filled" style="color:#d2992a;font-size:18px;width:22px"></i>
        <div style="flex:1"><div style="color:#c9d1d9;font-size:13px">StudentPro-Calculator</div><div style="color:#6e7681;font-size:11px">Python calculator · CustomTkinter</div></div>
        <i class="ti ti-chevron-right" style="color:#6e7681;font-size:12px"></i>
      </div>
      <div class="fi" ondblclick="openWin('repo_readme')">
        <i class="ti ti-folder-filled" style="color:#d2992a;font-size:18px;width:22px"></i>
        <div style="flex:1"><div style="color:#c9d1d9;font-size:13px">AtharvaSecurity (Profile)</div><div style="color:#6e7681;font-size:11px">README · Markdown</div></div>
        <i class="ti ti-chevron-right" style="color:#6e7681;font-size:12px"></i>
      </div>
      <div style="border-top:1px solid #21262d;margin:10px 0"></div>
      <div style="color:#6e7681;font-size:10px;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:6px;padding:0 10px">Files</div>
      <div class="fi"><i class="ti ti-file-text" style="color:#388bfd;font-size:16px;width:22px"></i><div style="flex:1"><div style="color:#c9d1d9;font-size:13px">goals-2026.md</div></div><div style="color:#6e7681;font-size:11px">1.2 KB</div></div>
      <div class="fi"><i class="ti ti-file-code" style="color:#30d378;font-size:16px;width:22px"></i><div style="flex:1"><div style="color:#c9d1d9;font-size:13px">hello_world.py</div></div><div style="color:#6e7681;font-size:11px">0.1 KB</div></div>
      <div class="fi"><i class="ti ti-lock" style="color:#a371f7;font-size:16px;width:22px"></i><div style="flex:1"><div style="color:#c9d1d9;font-size:13px">cybersec-notes.txt</div></div><div style="color:#6e7681;font-size:11px">4.8 KB</div></div>
      <div style="margin-top:12px;color:#6e7681;font-size:11px;padding:0 4px">6 items · Double-click folders to open</div>`},

    repo_deauth:{title:'DeauthDevil — Repository',w:420,h:440,html:`
      <div style="display:flex;align-items:center;gap:12px;margin-bottom:16px">
        <div style="width:44px;height:44px;border-radius:10px;background:rgba(248,81,73,0.12);display:flex;align-items:center;justify-content:center;font-size:22px;color:#f85149"><i class="ti ti-wifi-off"></i></div>
        <div><div style="color:#c9d1d9;font-size:16px;font-weight:500">DeauthDevil</div><div style="color:#6e7681;font-size:11px;margin-top:2px">AtharvaSecurity/DeauthDevil · Python 100%</div></div>
      </div>
      <div style="color:#8b949e;font-size:13px;line-height:1.65;margin-bottom:14px">WiFi Network Security Testing Tool. Educational WiFi scanner and deauthentication utility for authorized security testing. Cross-platform: Windows, Linux, macOS.</div>
      <div style="background:rgba(248,81,73,0.07);border:1px solid rgba(248,81,73,0.2);border-radius:8px;padding:10px 12px;margin-bottom:14px;font-size:12px;color:#f85149"><i class="ti ti-alert-triangle" style="font-size:13px"></i> For educational and authorized testing only</div>
      <div style="display:flex;flex-wrap:wrap;gap:6px;margin-bottom:14px">
        ${['WiFi Security','Python','Scapy','Educational','Cross-platform'].map(t=>{const c=['#f85149','#30d378','#38bdf8','#a371f7','#d2992a'][['WiFi Security','Python','Scapy','Educational','Cross-platform'].indexOf(t)];return `<span style="background:${c}1a;color:${c};border-radius:20px;padding:3px 11px;font-size:11px">${t}</span>`}).join('')}
      </div>
      <div style="background:#0d1117;border-radius:8px;padding:12px;font-family:monospace;font-size:12px;color:#8b949e;margin-bottom:14px;border:1px solid #21262d">
        <div style="color:#30d378">$ git clone https://github.com/AtharvaSecurity/DeauthDevil</div>
        <div style="color:#30d378;margin-top:4px">$ cd DeauthDevil</div>
        <div style="color:#30d378;margin-top:4px">$ python deauthdevil.py</div>
      </div>
      <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:8px">
        <div class="profile-stat"><div class="stat-n">0</div><div class="stat-l">Stars</div></div>
        <div class="profile-stat"><div class="stat-n">0</div><div class="stat-l">Forks</div></div>
        <div class="profile-stat"><div class="stat-n">MIT</div><div class="stat-l">License</div></div>
      </div>
      <div style="margin-top:12px"><a href="https://github.com/AtharvaSecurity/DeauthDevil" style="color:#388bfd;font-size:12px;text-decoration:none">View on GitHub →</a></div>`},

    repo_calc:{title:'StudentPro Calculator — Repository',w:420,h:400,html:`
      <div style="display:flex;align-items:center;gap:12px;margin-bottom:16px">
        <div style="width:44px;height:44px;border-radius:10px;background:rgba(56,189,248,0.12);display:flex;align-items:center;justify-content:center;font-size:22px;color:#38bdf8"><i class="ti ti-calculator"></i></div>
        <div><div style="color:#c9d1d9;font-size:16px;font-weight:500">StudentPro Calculator</div><div style="color:#6e7681;font-size:11px;margin-top:2px">AtharvaSecurity/StudentPro-Calculator · Python 100%</div></div>
      </div>
      <div style="color:#8b949e;font-size:13px;line-height:1.65;margin-bottom:14px">A modern Python calculator built with CustomTkinter featuring scientific calculations, commerce tools, history tracking, and a responsive dark UI.</div>
      <div style="background:rgba(255,255,255,0.04);border-radius:9px;padding:12px;margin-bottom:14px">
        <div style="color:#8b949e;font-size:11px;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:8px">Features</div>
        ${['Scientific calculations','Commerce tools (GST, interest, etc.)','History tracking','Clean dark interface','Built with CustomTkinter'].map(f=>`<div style="display:flex;align-items:center;gap:7px;padding:3px 0;color:#c9d1d9;font-size:12px"><i class="ti ti-check" style="color:#30d378;font-size:12px"></i>${f}</div>`).join('')}
      </div>
      <div style="display:flex;flex-wrap:wrap;gap:6px;margin-bottom:12px">
        ${['Python','CustomTkinter','Commerce Tools','Dark UI'].map(t=>`<span style="background:rgba(56,189,248,0.1);color:#38bdf8;border-radius:20px;padding:3px 11px;font-size:11px">${t}</span>`).join('')}
      </div>
      <a href="https://github.com/AtharvaSecurity/StudentPro-Calculator" style="color:#388bfd;font-size:12px;text-decoration:none">View on GitHub →</a>`},

    repo_readme:{title:'README — AtharvaSecurity',w:400,h:400,html:`
      <div style="font-family:monospace;font-size:12px;line-height:1.9;color:#c9d1d9">
        <div style="color:#30d378;font-size:18px;font-weight:500;margin-bottom:12px"># Atharva Mani Tripathi</div>
        <div style="color:#388bfd;margin-bottom:4px">## About Me</div>
        <div style="color:#8b949e;margin-bottom:14px;line-height:1.6">Class 11 Commerce Student learning Python,<br>Cybersecurity, Web Development, and AI.</div>
        <div style="color:#388bfd;margin-bottom:6px">## Tech Stack</div>
        <div style="display:flex;flex-wrap:wrap;gap:5px;margin-bottom:14px">
          ${['Python','Git','GitHub','VS Code','Linux','HTML','CSS'].map(t=>`<span style="background:rgba(56,139,253,0.1);color:#388bfd;border-radius:4px;padding:2px 9px;font-size:11px">${t}</span>`).join('')}
        </div>
        <div style="color:#388bfd;margin-bottom:4px">## Current Goals (2026)</div>
        <div style="color:#8b949e;margin-bottom:14px">
          - Improve Python programming skills<br>
          - Learn Linux fundamentals<br>
          - Learn networking basics<br>
          - Build useful open-source projects<br>
          - Strengthen cybersecurity knowledge
        </div>
        <div style="color:#388bfd;margin-bottom:4px">## Connect</div>
        <div style="color:#8b949e">
          GitHub: <span style="color:#388bfd">github.com/AtharvaSecurity</span><br>
          YouTube: <span style="color:#f85149">youtube.com/@AtharvaSecurity</span>
        </div>
        <div style="margin-top:14px;color:#6e7681">⚡ Learning something new every day ⚡</div>
      </div>`},

    terminal:{title:'Terminal — bash',w:460,h:420,html:`
      <div id="term-out"></div>
      <div id="term-input-wrap">
        <span class="t-prompt" style="font-family:monospace;font-size:12px;white-space:nowrap">atharva@AtharvaOS:~$&nbsp;</span>
        <input id="term-input" type="text" placeholder="type a command..." autocomplete="off" spellcheck="false">
      </div>`,
      onOpen:()=>{
        setTimeout(()=>{
          termPrint(false,'AtharvaOS v1.0 — Powered by Atharva','t-out t-green');
          termPrint(false,'Welcome! Type "help" to see commands.','t-out t-muted');
          const inp=document.getElementById('term-input');
          if(inp){inp.focus();inp.addEventListener('keydown',termKey);}
        },80);
      }},

    browser:{title:'Browser — github.com/AtharvaSecurity',w:440,h:480,html:`
      <div style="display:flex;align-items:center;gap:8px;margin-bottom:14px;background:rgba(255,255,255,0.04);border-radius:7px;padding:7px 12px;border:1px solid #21262d">
        <i class="ti ti-lock" style="color:#30d378;font-size:12px"></i>
        <span style="color:#8b949e;font-size:12px;flex:1">github.com/AtharvaSecurity</span>
        <i class="ti ti-refresh" style="color:#6e7681;font-size:13px;cursor:pointer"></i>
      </div>
      <div style="display:flex;align-items:center;gap:13px;margin-bottom:18px">
        <div style="width:52px;height:52px;border-radius:50%;background:#21262d;border:2px solid #30363d;display:flex;align-items:center;justify-content:center;font-size:24px;color:#30d378"><i class="ti ti-shield-lock"></i></div>
        <div>
          <div style="color:#c9d1d9;font-size:16px;font-weight:500">AtharvaSecurity</div>
          <div style="color:#8b949e;font-size:12px;margin-top:1px">Atharva Mani Tripathi</div>
          <div style="color:#6e7681;font-size:11px;margin-top:4px">Class 11 · Python · Cybersecurity · Web Dev · AI</div>
        </div>
      </div>
      <div style="display:flex;gap:16px;margin-bottom:16px;font-size:12px;color:#8b949e">
        <span><strong style="color:#c9d1d9">3</strong> repositories</span>
        <span><strong style="color:#c9d1d9">3</strong> followers</span>
        <span><strong style="color:#c9d1d9">7</strong> following</span>
      </div>
      <div style="color:#6e7681;font-size:11px;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:8px">Pinned</div>
      <div class="repo-card" ondblclick="openWin('repo_deauth')">
        <div style="color:#388bfd;font-size:14px;font-weight:500"><i class="ti ti-book" style="font-size:12px"></i> DeauthDevil</div>
        <div style="color:#8b949e;font-size:12px;margin-top:4px;line-height:1.5">WiFi Network Security Testing Tool — educational deauth utility for authorized testing</div>
        <div style="display:flex;align-items:center;gap:5px;margin-top:8px;font-size:11px;color:#6e7681"><div style="width:10px;height:10px;border-radius:50%;background:#3572A5"></div>Python</div>
      </div>
      <div class="repo-card" ondblclick="openWin('repo_calc')">
        <div style="color:#388bfd;font-size:14px;font-weight:500"><i class="ti ti-book" style="font-size:12px"></i> StudentPro-Calculator</div>
        <div style="color:#8b949e;font-size:12px;margin-top:4px;line-height:1.5">Modern Python calculator with dark UI, scientific & commerce tools</div>
        <div style="display:flex;align-items:center;gap:5px;margin-top:8px;font-size:11px;color:#6e7681"><div style="width:10px;height:10px;border-radius:50%;background:#3572A5"></div>Python</div>
      </div>
      <div style="text-align:center;margin-top:6px"><a href="https://github.com/AtharvaSecurity" style="color:#388bfd;font-size:12px;text-decoration:none">Open in GitHub →</a></div>`},

    skills:{title:'Skills & Tech Stack',w:380,h:440,html:`
      <div style="margin-bottom:18px">
        <div style="color:#8b949e;font-size:11px;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:12px">Proficiency</div>
        ${[
          {n:'Python',p:72,c:'#30d378'},
          {n:'Cybersecurity',p:55,c:'#f85149'},
          {n:'HTML / CSS',p:60,c:'#388bfd'},
          {n:'Git & GitHub',p:65,c:'#a371f7'},
          {n:'Linux',p:48,c:'#d2992a'},
          {n:'Networking',p:40,c:'#38bdf8'},
        ].map(s=>`<div style="margin-bottom:10px">
          <div style="display:flex;justify-content:space-between;font-size:12px;color:#c9d1d9;margin-bottom:5px"><span>${s.n}</span><span style="color:#6e7681">${s.p}%</span></div>
          <div class="skill-track"><div class="skill-fill" style="width:${s.p}%;background:${s.c}"></div></div>
        </div>`).join('')}
      </div>
      <div style="border-top:1px solid #21262d;padding-top:14px">
        <div style="color:#8b949e;font-size:11px;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:10px">Tools & Environment</div>
        <div style="display:flex;flex-wrap:wrap;gap:6px;margin-bottom:14px">
          ${['VS Code','Kali Linux','Wireshark','Git','CustomTkinter','Scapy','Python 3','Nmap'].map(t=>`<span style="background:rgba(255,255,255,0.06);border:1px solid #30363d;border-radius:6px;padding:4px 11px;font-size:11px;color:#c9d1d9">${t}</span>`).join('')}
        </div>
        <div style="color:#8b949e;font-size:11px;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:8px">Interests</div>
        <div style="display:flex;flex-wrap:wrap;gap:6px">
          ${['Python Development','Cybersecurity','Web Development','Artificial Intelligence','Open Source'].map(t=>`<span style="background:rgba(48,211,120,0.08);border:1px solid rgba(48,211,120,0.2);border-radius:6px;padding:4px 11px;font-size:11px;color:#30d378">${t}</span>`).join('')}
        </div>
      </div>`},

    calc:{title:'Calculator',w:290,h:420,html:`
      <div style="background:rgba(255,255,255,0.04);border-radius:9px;padding:13px 16px;margin-bottom:12px;text-align:right">
        <div id="c-expr" style="color:#6e7681;font-size:12px;min-height:18px;font-family:monospace"></div>
        <div id="c-disp" style="color:#c9d1d9;font-size:30px;font-weight:500;margin-top:4px;font-family:monospace">0</div>
      </div>
      <div class="calc-grid">
        <button class="cb cb-clr" onclick="cBtn('C')">C</button>
        <button class="cb cb-num" onclick="cBtn('±')">±</button>
        <button class="cb cb-num" onclick="cBtn('%')">%</button>
        <button class="cb cb-op" onclick="cBtn('÷')">÷</button>
        <button class="cb cb-num" onclick="cBtn('7')">7</button>
        <button class="cb cb-num" onclick="cBtn('8')">8</button>
        <button class="cb cb-num" onclick="cBtn('9')">9</button>
        <button class="cb cb-op" onclick="cBtn('×')">×</button>
        <button class="cb cb-num" onclick="cBtn('4')">4</button>
        <button class="cb cb-num" onclick="cBtn('5')">5</button>
        <button class="cb cb-num" onclick="cBtn('6')">6</button>
        <button class="cb cb-op" onclick="cBtn('−')">−</button>
        <button class="cb cb-num" onclick="cBtn('1')">1</button>
        <button class="cb cb-num" onclick="cBtn('2')">2</button>
        <button class="cb cb-num" onclick="cBtn('3')">3</button>
        <button class="cb cb-op" onclick="cBtn('+')">+</button>
        <button class="cb cb-num cb-wide" onclick="cBtn('0')">0</button>
        <button class="cb cb-num" onclick="cBtn('.')">.</button>
        <button class="cb cb-op" onclick="cBtn('=')">=</button>
      </div>`},

    about:{title:'About AtharvaOS',w:380,h:380,html:`
      <div style="text-align:center;margin-bottom:20px">
        <div style="width:60px;height:60px;border-radius:16px;background:rgba(48,211,120,0.12);border:2px solid rgba(48,211,120,0.3);display:flex;align-items:center;justify-content:center;margin:0 auto 12px;font-size:28px;color:#30d378"><i class="ti ti-terminal"></i></div>
        <div style="color:#c9d1d9;font-size:18px;font-weight:500">AtharvaOS</div>
        <div style="color:#6e7681;font-size:12px;margin-top:4px">Version 1.0.0 · Linux Kernel 6.1</div>
      </div>
      <div style="background:rgba(255,255,255,0.04);border-radius:9px;padding:13px;margin-bottom:14px">
        ${[
          ['Built by','Atharva Mani Tripathi'],
          ['GitHub','@AtharvaSecurity'],
          ['Environment','Web Browser (HTML5)'],
          ['Language','JavaScript + CSS'],
          ['Theme','GitHub Dark'],
          ['Based on','AtharvaSecurity GitHub Profile'],
        ].map(([k,v])=>`<div style="display:flex;justify-content:space-between;padding:5px 0;border-bottom:1px solid #21262d;font-size:12px"><span style="color:#6e7681">${k}</span><span style="color:#c9d1d9">${v}</span></div>`).join('')}
      </div>
      <div style="color:#6e7681;font-size:12px;text-align:center;line-height:1.6">This OS simulation was created from<br>Atharva's GitHub profile. It showcases<br>his projects, skills, and tech interests.</div>
      <div style="text-align:center;margin-top:12px"><a href="https://github.com/AtharvaSecurity" style="color:#388bfd;font-size:12px;text-decoration:none">Visit Real GitHub →</a></div>`}
  };
  return configs[id]||{title:id,w:300,h:200,html:'<p style="color:#6e7681">App not found.</p>'};
}

// Terminal
const termHist=[]; let tHistIdx=-1;
function termPrint(isPrompt,text,cls){
  const o=document.getElementById('term-out');if(!o)return;
  const row=document.createElement('div');row.className='term-row';
  if(isPrompt){
    row.innerHTML=`<span class="t-prompt" style="font-family:monospace;font-size:12px">atharva@AtharvaOS:~$&nbsp;</span><span class="t-cmd" style="font-family:monospace;font-size:12px">${text}</span>`;
  } else {
    row.innerHTML=`<span class="${cls}" style="font-family:monospace;font-size:12px">${text}</span>`;
  }
  o.appendChild(row); o.scrollTop=o.scrollHeight;
}
function termKey(e){
  const inp=document.getElementById('term-input');
  if(e.key==='Enter'){
    const cmd=inp.value.trim(); inp.value='';
    if(!cmd)return;
    termHist.unshift(cmd); tHistIdx=-1;
    termPrint(true,cmd,'');
    runCmd(cmd);
  } else if(e.key==='ArrowUp'){
    tHistIdx=Math.min(tHistIdx+1,termHist.length-1);
    if(termHist[tHistIdx])inp.value=termHist[tHistIdx];
    e.preventDefault();
  } else if(e.key==='ArrowDown'){
    tHistIdx=Math.max(tHistIdx-1,-1);
    inp.value=tHistIdx>=0?termHist[tHistIdx]:'';
  }
}
function runCmd(cmd){
  const p=cmd.trim().split(' '),c=p[0].toLowerCase();
  const cmds={
    help:()=>['help      — show this help','ls        — list files and repos','cat [file]— read a file','whoami    — current user','uname     — system info','python    — python version','clear     — clear terminal','github    — open github app','repos     — list repositories','neofetch  — system info art','exit      — close terminal'].forEach(l=>termPrint(false,l,'t-out t-muted')),
    ls:()=>termPrint(false,'DeauthDevil/   StudentPro-Calculator/   AtharvaSecurity/   goals-2026.md   hello_world.py   cybersec-notes.txt','t-out t-blue'),
    whoami:()=>termPrint(false,'atharva','t-out t-green'),
    pwd:()=>termPrint(false,'/home/atharva','t-out t-muted'),
    uname:()=>termPrint(false,'AtharvaOS Linux 6.1.0-atharva x86_64 GNU/Linux','t-out t-muted'),
    date:()=>termPrint(false,new Date().toString(),'t-out t-muted'),
    python:()=>{termPrint(false,'Python 3.12.0 (AtharvaSecurity build)','t-out t-green');termPrint(false,'Packages: scapy, customtkinter, requests, flask','t-out t-muted');},
    python3:()=>runCmd('python'),
    clear:()=>{const o=document.getElementById('term-out');if(o)o.innerHTML='';},
    github:()=>{openWin('browser');termPrint(false,'Opening GitHub browser...','t-out t-green');},
    repos:()=>{termPrint(false,'1. DeauthDevil         — WiFi security tool (Python)','t-out t-blue');termPrint(false,'2. StudentPro-Calculator — Python calculator (Python)','t-out t-blue');termPrint(false,'3. AtharvaSecurity       — Profile README','t-out t-blue');},
    exit:()=>{const w=Object.entries(wins).find(([,v])=>v.el.querySelector('#term-input'));if(w)closeWin(w[0]);},
    neofetch:()=>{
      const lines=[
        '        ##       atharva@AtharvaOS',
        '       #  #      -----------------',
        '      # // #     OS: AtharvaOS 1.0',
        '     #  ##  #    Host: GitHub Profile',
        '    # #    # #   Kernel: 6.1.0-atharva',
        '   #          #  Shell: bash 5.2',
        '  ##############  Repos: 3',
        ' #              # Skills: Python, Security',
        '  ##############  Goals: Learn everything!',
      ];
      lines.forEach(l=>termPrint(false,l,'t-out t-green'));
    },
  };
  if(c==='cat'){
    const f=p[1]||'';
    if(f==='goals-2026.md'){
      termPrint(false,'# Goals 2026','t-out t-green');
      ['- Improve Python skills','- Learn Linux fundamentals','- Learn networking basics','- Build open-source projects','- Strengthen cybersecurity knowledge'].forEach(l=>termPrint(false,l,'t-out t-muted'));
    } else if(f==='hello_world.py'){
      termPrint(false,'print("Hello, World!")','t-out t-green');
      termPrint(false,"# Atharva's first script",'t-out t-muted');
    } else if(f==='cybersec-notes.txt'){
      termPrint(false,'[Cybersecurity Notes]','t-out t-yellow');
      ['- Always get authorization before testing','- Use Wireshark for packet analysis','- Learn OSI model thoroughly','- Practice on CTF challenges'].forEach(l=>termPrint(false,l,'t-out t-muted'));
    } else {
      termPrint(false,`cat: ${f}: No such file or directory`,'t-out t-red');
    }
    return;
  }
  if(cmds[c]) cmds[c]();
  else termPrint(false,`bash: ${c}: command not found. Type 'help' for commands.`,'t-out t-red');
}

// Calculator
let cVal='',cOp='',cPrev='';
function cBtn(b){
  const d=document.getElementById('c-disp'),ex=document.getElementById('c-expr');
  if(!d)return;
  if(b==='C'){cVal='';cOp='';cPrev='';d.textContent='0';ex.textContent='';return;}
  if(b==='='){
    if(!cOp||cPrev==='')return;
    const a=parseFloat(cPrev),bv=parseFloat(cVal||'0');
    let r;
    if(cOp==='÷')r=a/bv;
    else if(cOp==='×')r=a*bv;
    else if(cOp==='−')r=a-bv;
    else if(cOp==='+')r=a+bv;
    ex.textContent=`${cPrev} ${cOp} ${cVal} =`;
    cPrev='';cOp='';
    cVal=String(parseFloat(r.toFixed(10)));
    d.textContent=cVal;
    return;
  }
  if(['÷','×','−','+'].includes(b)){cPrev=cVal||d.textContent;cOp=b;cVal='';ex.textContent=`${cPrev} ${b}`;return;}
  if(b==='.'&&cVal.includes('.'))return;
  if(b==='±'){cVal=String(-parseFloat(cVal||d.textContent));d.textContent=cVal;return;}
  if(b==='%'){cVal=String(parseFloat(cVal||d.textContent)/100);d.textContent=cVal;return;}
  cVal=(cVal===''||cVal==='0')?b:cVal+b;
  d.textContent=cVal;
}
</script>
</body>
</html>
