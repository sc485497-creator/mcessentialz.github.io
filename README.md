[index (3).html](https://github.com/user-attachments/files/29989777/index.3.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MCessentials — Free Browser Tools</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<style>
  :root{
    --bg:#0b0d14; --panel:rgba(255,255,255,0.045); --panel-solid:#171a24; --line:rgba(255,255,255,0.09);
    --text:#f1f3f8; --muted:#9099ab;
    --accent:#ff4d5e; --accent-rgb:255,77,94;
    --radius:16px;
  }
  [data-theme="day"]{
    --bg:#1b2030; --panel:rgba(255,255,255,0.07); --panel-solid:#232a3f; --line:rgba(255,255,255,0.13);
    --text:#f1f3f8; --muted:#9aa2b4;
  }
  *{box-sizing:border-box;}
  body{
    margin:0; background:var(--bg); color:var(--text);
    font-family:'Segoe UI', system-ui, sans-serif; overflow-x:hidden; min-height:100vh;
    transition:background .3s;
  }
  h1,h2,h3{ font-weight:800; letter-spacing:-.01em; }

  /* ---------- STARFIELD BG ---------- */
  #stars{ position:fixed; inset:0; z-index:-2; overflow:hidden; }
  #stars .dot{ position:absolute; width:2px; height:2px; background:#fff; border-radius:50%; opacity:.5; animation:twinkle 4s ease-in-out infinite; }
  @keyframes twinkle{ 0%,100%{opacity:.15;} 50%{opacity:.85;} }
  #nebula{
    position:fixed; inset:0; z-index:-1; pointer-events:none;
    background:
      radial-gradient(600px 400px at 15% 20%, rgba(var(--accent-rgb),0.16), transparent 60%),
      radial-gradient(500px 380px at 85% 75%, rgba(var(--accent-rgb),0.10), transparent 60%);
    transition: background 0.4s ease;
  }

  /* ---------- ACCENT PICKER ---------- */
  .accent-picker{
    position:fixed; top:16px; left:16px; z-index:100;
    width:46px; height:46px; border-radius:50%;
    background:conic-gradient(from 0deg, red, yellow, lime, cyan, blue, magenta, red);
    padding:3px; box-shadow:0 4px 14px rgba(0,0,0,.4);
    transition:transform .2s;
  }
  .accent-picker:hover{ transform:scale(1.08) rotate(8deg); }
  .accent-picker input[type=color]{
    width:100%; height:100%; border:3px solid var(--bg); border-radius:50%; cursor:pointer;
    padding:0; overflow:hidden; background:none;
  }
  .accent-picker input[type=color]::-webkit-color-swatch-wrapper{ padding:0; }
  .accent-picker input[type=color]::-webkit-color-swatch{ border:none; border-radius:50%; }

  header{
    display:flex; align-items:center; justify-content:space-between; padding:16px 30px 16px 76px;
    position:sticky; top:0; z-index:10; flex-wrap:wrap; gap:10px;
    background:rgba(11,13,20,0.55); backdrop-filter:blur(14px); border-bottom:1px solid var(--line);
  }
  .logo{ font-size:19px; font-weight:800; cursor:pointer; letter-spacing:-.02em; }
  .logo span{ color:var(--accent); transition:color .3s; }
  nav.topnav{ display:flex; gap:6px; align-items:center; flex-wrap:wrap; }
  .navgroup{ position:relative; }
  .navbtn{
    background:none; border:none; color:var(--muted); font-size:13.5px; padding:9px 15px;
    cursor:pointer; font-family:inherit; font-weight:600; transition:.18s; border-radius:999px;
  }
  .navbtn:hover{ color:var(--text); background:var(--panel); }
  .dropdown{
    display:none; position:absolute; top:110%; left:0; min-width:200px;
    padding:8px; margin-top:2px; z-index:20; border-radius:14px;
    background:var(--panel-solid); border:1px solid var(--line); box-shadow:0 12px 30px rgba(0,0,0,.35);
  }
  .navgroup:hover .dropdown{ display:block; animation:dropIn .15s ease; }
  @keyframes dropIn{ from{opacity:0; transform:translateY(-4px);} to{opacity:1; transform:translateY(0);} }
  .dropdown a{
    display:block; padding:9px 12px; color:var(--text); text-decoration:none;
    font-size:13.5px; border-radius:10px; transition:.15s; cursor:pointer; font-weight:500;
  }
  .dropdown a:hover{ background:rgba(var(--accent-rgb),0.15); transform:translateX(3px); color:var(--accent); }
  .toggle{
    background:var(--panel); border:1px solid var(--line); color:var(--text); padding:8px 14px;
    border-radius:999px; cursor:pointer; font-size:13px; font-weight:600; transition:.18s;
  }
  .toggle:hover{ background:rgba(var(--accent-rgb),0.18); border-color:rgba(var(--accent-rgb),0.4); }

  main{ max-width:1080px; margin:0 auto; padding:40px 24px 70px; position:relative; z-index:1; }

  /* ---------- quick-access bubble dock ---------- */
  #quickDock{
    position:fixed; left:16px; top:50%; transform:translateY(-50%); z-index:15;
    display:flex; flex-direction:column; align-items:center; gap:10px;
  }
  .quick-bubble{
    width:46px; height:46px; border-radius:50%; flex-shrink:0;
    background:#262a35; border:1px solid rgba(255,255,255,0.07);
    display:flex; align-items:center; justify-content:center;
    font-size:18px; cursor:pointer; position:relative; color:#eef0f4;
    box-shadow:0 4px 14px rgba(0,0,0,0.35);
    transition:transform .18s, background .18s;
  }
  [data-theme="day"] .quick-bubble{ background:#cdd2db; color:#20232b; box-shadow:0 4px 14px rgba(0,0,0,0.14); border-color:rgba(0,0,0,0.06); }
  .quick-bubble:hover{ transform:scale(1.08); }
  .quick-bubble.plusbubble{ font-size:20px; font-weight:300; }
  .bubble-remove{
    position:absolute; top:-4px; right:-4px; width:16px; height:16px; border-radius:50%;
    background:#e05d5d; color:#fff; font-size:10px; display:none; align-items:center; justify-content:center; cursor:pointer;
  }
  .quick-bubble:hover .bubble-remove{ display:flex; }
  @keyframes bubblePop{ 0%{ transform:scale(0); opacity:0; } 70%{ transform:scale(1.15); opacity:1; } 100%{ transform:scale(1); } }
  .bubble-pop{ animation:bubblePop .4s cubic-bezier(.34,1.56,.64,1); }
  @keyframes iconFly{ 0%{ transform:scale(0.15) rotate(-20deg); opacity:0; } 60%{ transform:scale(1.25) rotate(6deg); opacity:1; } 100%{ transform:scale(1) rotate(0); } }
  .icon-fly{ display:inline-block; animation:iconFly .45s cubic-bezier(.34,1.56,.64,1); }

  @keyframes bubbleGlowPulse{
    0%,100%{ box-shadow:0 0 8px 2px rgba(147,51,234,0.55), 0 4px 14px rgba(0,0,0,0.35); }
    50%{ box-shadow:0 0 22px 9px rgba(147,51,234,0.9), 0 4px 14px rgba(0,0,0,0.35); }
  }
  [data-theme="day"] .quick-bubble.glowing{ animation-name: bubbleGlowPulseDay; }
  @keyframes bubbleGlowPulseDay{
    0%,100%{ box-shadow:0 0 8px 2px rgba(234,179,8,0.6), 0 4px 14px rgba(0,0,0,0.14); }
    50%{ box-shadow:0 0 22px 9px rgba(234,179,8,0.9), 0 4px 14px rgba(0,0,0,0.14); }
  }
  .quick-bubble.glowing{ animation:bubbleGlowPulse 1.3s ease-in-out infinite; }
  .quick-bubble.glowing-intense{ animation:bubbleGlowIntense .5s ease-in-out 2 !important; }
  @keyframes bubbleGlowIntense{
    0%{ box-shadow:0 0 12px 4px rgba(147,51,234,0.65); transform:scale(1); }
    50%{ box-shadow:0 0 36px 15px rgba(147,51,234,1); transform:scale(1.18); }
    100%{ box-shadow:0 0 12px 4px rgba(147,51,234,0.65); transform:scale(1); }
  }
  [data-theme="day"] .quick-bubble.glowing-intense{ animation-name: bubbleGlowIntenseDay !important; }
  @keyframes bubbleGlowIntenseDay{
    0%{ box-shadow:0 0 12px 4px rgba(234,179,8,0.65); transform:scale(1); }
    50%{ box-shadow:0 0 36px 15px rgba(234,179,8,1); transform:scale(1.18); }
    100%{ box-shadow:0 0 12px 4px rgba(234,179,8,0.65); transform:scale(1); }
  }
  @keyframes bubblePopOut{
    0%{ transform:scale(1); opacity:1; }
    55%{ transform:scale(1.55); opacity:0.55; }
    100%{ transform:scale(0.15); opacity:0; }
  }
  .bubble-pop-out{ animation:bubblePopOut .38s ease-in forwards; }

  #dockPicker{
    display:none; position:fixed; left:70px; z-index:60; padding:6px;
    max-height:70vh; overflow-y:auto; min-width:200px;
  }
  .dock-pick-item{
    padding:9px 10px; border-radius:8px; cursor:pointer; font-size:13px;
    display:flex; align-items:center; gap:8px; transition:.15s;
  }
  .dock-pick-item:hover{ background:var(--panel2); }
  .dock-pick-item .pinned-tag{ margin-left:auto; color:var(--muted); font-size:10.5px; }
  .page{ display:none; }
  .page.active{ display:block; animation:fadeUp .3s cubic-bezier(.2,.8,.2,1); }
  @keyframes fadeUp{ from{opacity:0; transform:translateY(10px);} to{opacity:1; transform:translateY(0);} }

  .hero h1{ font-size:32px; margin:0 0 8px; }
  .hero p{ color:var(--muted); font-size:14.5px; max-width:560px; line-height:1.6; margin:0 0 34px; }
  .sectionlabel{ font-size:12px; text-transform:uppercase; letter-spacing:.14em; color:var(--muted); margin:30px 0 14px; font-weight:700; }

  .block{
    background:var(--panel); border:1px solid var(--line); border-radius:var(--radius);
    backdrop-filter:blur(10px);
  }

  .grid{ display:grid; grid-template-columns:repeat(auto-fill, minmax(200px,1fr)); gap:14px; }
  .card{
    padding:20px; cursor:pointer; position:relative; overflow:hidden;
    transition:transform .25s cubic-bezier(.2,.9,.3,1.3), box-shadow .25s, border-color .25s;
  }
  .card:hover{ transform:translateY(-6px) scale(1.02); box-shadow:0 16px 30px rgba(var(--accent-rgb),0.18); border-color:rgba(var(--accent-rgb),0.45); }
  .card .icon-badge{
    width:44px; height:44px; border-radius:13px; display:flex; align-items:center; justify-content:center;
    font-size:21px; background:rgba(var(--accent-rgb),0.16); margin-bottom:12px; transition:.25s;
  }
  .card:hover .icon-badge{ transform:scale(1.12) rotate(-6deg); background:rgba(var(--accent-rgb),0.28); }
  .card h3{ font-size:15px; margin:0 0 5px; }
  .card p{ font-size:12.5px; color:var(--muted); margin:0; line-height:1.45; }

  .tool-header{ margin-bottom:22px; }
  .tool-header h1{ font-size:24px; margin:0 0 6px; }
  .tool-header p{ color:var(--muted); font-size:13.5px; margin:0; }

  .toolbar{ display:flex; gap:8px; flex-wrap:wrap; margin-bottom:16px; align-items:center; }
  .toolbtn{
    background:var(--panel); border:1px solid var(--line); color:var(--text); padding:9px 14px;
    border-radius:12px; cursor:pointer; font-size:13px; font-weight:600; transition:.18s;
  }
  .toolbtn:hover{ background:rgba(var(--accent-rgb),0.18); transform:translateY(-1px); border-color:rgba(var(--accent-rgb),0.4); }
  .toolbtn.active{ background:var(--accent); color:#fff; border-color:var(--accent); }
  select.toolbtn{ appearance:auto; }

  .editor-layout{ display:grid; grid-template-columns:1fr 230px; gap:20px; }
  #editorWrap{ padding:16px; text-align:center; overflow:auto; }
  #editCanvas{ image-rendering:pixelated; cursor:crosshair; border-radius:8px; }

  .palette{ padding:16px; align-self:start; }
  .palette-grid{ display:grid; grid-template-columns:repeat(6,1fr); gap:6px; margin-bottom:14px; }
  .swatch-btn{ width:100%; aspect-ratio:1; border:1px solid var(--line); border-radius:8px; cursor:pointer; transition:.15s; }
  .swatch-btn:hover{ transform:scale(1.15); }
  .colorinput{ width:100%; height:38px; border:1px solid var(--line); border-radius:10px; background:none; cursor:pointer; margin-bottom:12px; }
  .actions{ display:flex; flex-direction:column; gap:8px; }

  .dropzone{
    padding:34px; text-align:center; cursor:pointer; color:var(--muted); font-size:13.5px;
    border:2px dashed var(--line) !important; transition:.2s; margin-bottom:18px; border-radius:var(--radius);
  }
  .dropzone:hover{ border-color:var(--accent) !important; color:var(--text); background:rgba(var(--accent-rgb),0.06); }
  input[type=file]{ display:none; }

  .avatar-layout{ display:grid; grid-template-columns:1fr 1fr; gap:20px; }
  .avatar-panel{ padding:18px; text-align:center; }
  #avatarPreview{ image-rendering:pixelated; max-width:100%; border-radius:12px; background:repeating-conic-gradient(#2a2e3a 0% 25%, #1c1f28 0% 50%) 50%/16px 16px; }
  .slider-row{ display:flex; align-items:center; gap:10px; margin:14px 0; font-size:13px; color:var(--muted); }
  .slider-row input[type=range]{ flex:1; accent-color:var(--accent); }

  .workbench{ display:none; margin-top:20px; gap:20px; grid-template-columns:1fr 230px; }
  .workbench.show{ display:grid; }
  #canvasWrap{ padding:12px; overflow:auto; text-align:center; }
  #picCanvas{ cursor:crosshair; image-rendering:pixelated; border-radius:8px; }
  .result{ padding:18px; align-self:start; }
  .swatch{ width:100%; height:64px; border-radius:12px; margin-bottom:14px; border:1px solid var(--line); }
  .hexrow{ display:flex; gap:6px; margin-bottom:8px; }
  .hexrow input{ flex:1; background:var(--panel); border:1px solid var(--line); color:var(--text); padding:9px 12px; border-radius:10px; font-family:'Courier New',monospace; font-size:13px; }
  .copybtn{ background:var(--accent); border:none; color:#fff; font-weight:700; padding:9px 14px; border-radius:10px; cursor:pointer; font-size:12px; transition:.18s; }
  .copybtn:hover{ filter:brightness(1.12); transform:translateY(-1px); }
  .copybtn:active{ transform:scale(.95); }
  .zoomhint{ font-size:11.5px; color:var(--muted); margin-top:14px; line-height:1.5; }

  .palette-result{ display:grid; grid-template-columns:repeat(5,1fr); gap:10px; margin-top:16px; }
  .palette-swatch{ border-radius:12px; padding:10px; text-align:center; cursor:pointer; transition:.2s; border:1px solid var(--line); }
  .palette-swatch:hover{ transform:translateY(-4px); }
  .palette-swatch .sw{ height:60px; border-radius:8px; margin-bottom:8px; }
  .palette-swatch .hx{ font-family:'Courier New',monospace; font-size:11.5px; color:var(--text); }

  .related{ margin-top:44px; padding-top:18px; border-top:1px solid var(--line); }
  .related h3{ font-size:12px; text-transform:uppercase; letter-spacing:.1em; color:var(--muted); margin-bottom:10px; }
  .related-links{ display:flex; gap:10px; flex-wrap:wrap; }
  .related-links a{
    font-size:12.5px; color:var(--text); text-decoration:none; padding:8px 14px;
    background:var(--panel); border:1px solid var(--line); border-radius:999px; transition:.18s; cursor:pointer; font-weight:600;
  }
  .related-links a:hover{ background:rgba(var(--accent-rgb),0.18); border-color:rgba(var(--accent-rgb),0.4); }

  footer{ text-align:center; color:var(--muted); font-size:11.5px; padding:30px; position:relative; z-index:1; }

  @media (max-width:720px){
    .editor-layout, .avatar-layout, .workbench.show{ grid-template-columns:1fr; }
    header{ padding-left:70px; }
  }

  /* ---------- 3D/2D editor layout ---------- */
  .stack3d2d{ display:grid; grid-template-columns:1fr 190px; gap:14px; align-items:start; }
  .stackcol{ display:flex; flex-direction:column; gap:14px; min-width:0; }
  .togglerail{ padding:16px; display:flex; flex-direction:column; gap:14px; }
  .togglerail .tr-title{ font-size:11px; text-transform:uppercase; letter-spacing:.1em; color:var(--muted); font-weight:700; margin-bottom:2px; }
  .tr-row{ display:flex; align-items:center; justify-content:space-between; gap:8px; }
  .tr-row span{ font-size:12.5px; font-weight:600; }
  .pillswitch{ position:relative; width:38px; height:22px; flex-shrink:0; }
  .pillswitch input{ opacity:0; width:0; height:0; }
  .pillswitch .track{
    position:absolute; inset:0; background:rgba(255,255,255,0.12); border-radius:999px; cursor:pointer; transition:.2s;
    border:1px solid var(--line);
  }
  .pillswitch .track::before{
    content:''; position:absolute; width:16px; height:16px; left:2px; top:2px; background:#fff; border-radius:50%; transition:.2s;
  }
  .pillswitch input:checked + .track{ background:var(--accent); }
  .pillswitch input:checked + .track::before{ transform:translateX(16px); }

  #templateCanvas, #gridCanvas2{ position:absolute; top:0; left:0; pointer-events:none; }
  .partpicker rect{ fill:#4a4f5c; stroke:rgba(255,255,255,0.25); stroke-width:1; cursor:pointer; transition:fill .15s; }
  .partpicker rect:hover{ stroke:rgba(var(--accent-rgb),0.8); }
  .partpicker rect.on{ fill:#f1f3f8; }

  /* ---------- color panel ---------- */
  .colorpanel{ display:grid; grid-template-columns:170px 1fr; gap:22px; padding:20px; }
  #wheelCanvas{ cursor:crosshair; border-radius:50%; }
  .wheel-wrap{ position:relative; width:170px; height:170px; }
  .wheel-marker{ position:absolute; width:12px; height:12px; border:2px solid #fff; border-radius:50%; box-shadow:0 0 0 1px rgba(0,0,0,.4); pointer-events:none; transform:translate(-50%,-50%); }
  .lightness-slider{ width:100%; height:16px; border-radius:8px; margin-top:12px; -webkit-appearance:none; appearance:none; cursor:pointer; }
  .lightness-slider::-webkit-slider-thumb{ -webkit-appearance:none; width:18px; height:18px; border-radius:50%; background:#fff; border:2px solid var(--line); box-shadow:0 2px 6px rgba(0,0,0,.4); }
  .colorpanel-right{ display:flex; flex-direction:column; gap:10px; }
  .swatch-preview{ width:100%; height:44px; border-radius:10px; border:1px solid var(--line); }
  .lightendarken{ display:flex; gap:8px; }
  .lightendarken button{ flex:1; }
</style>
</head>
<body data-theme="night">

<div id="stars"></div>
<div id="nebula"></div>

<div class="accent-picker" title="Change accent color">
  <input type="color" id="accentInput" value="#ff4d5e">
</div>

<div id="quickDock"></div>
<div id="dockPicker" class="block"></div>

<header>
  <div class="logo" onclick="go('home')">MC<span>essentials</span></div>
  <nav class="topnav">
    <div class="navgroup">
      <button class="navbtn">Character ▾</button>
      <div class="dropdown block">
        <a onclick="openEditor('skin')">🧍 Skin Editor</a>
        <a onclick="openEditor('cape')">🧥 Cape Editor</a>
        <a onclick="go('avatar')">👤 Avatar Maker</a>
      </div>
    </div>
    <div class="navgroup">
      <button class="navbtn">Create ▾</button>
      <div class="dropdown block">
        <a onclick="openIcon('painting')">🖼 Custom Paintings</a>
        <a onclick="openIcon('disc')">💿 Music Disc Maker</a>
        <a onclick="openIcon('totem')">🗿 Totem Generator</a>
        <a onclick="openIcon('head')">🧊 Player Heads</a>
        <a onclick="openIcon('emoji')">😀 Custom Emojis</a>
      </div>
    </div>
    <div class="navgroup">
      <button class="navbtn">Color ▾</button>
      <div class="dropdown block">
        <a onclick="go('picker')">🎨 Color Picker</a>
        <a onclick="go('formatcodes')">🌈 Format Code Maker</a>
        <a onclick="go('palette')">🧬 Palette Generator</a>
      </div>
    </div>
    <div class="navgroup">
      <button class="navbtn">Tools ▾</button>
      <div class="dropdown block">
        <a onclick="go('coords')">📍 Coordinate Calculator</a>
        <a onclick="go('skinpack')">📦 Skin Pack Maker</a>
        <a onclick="go('merger')">🗂 Texture Pack Merger</a>
        <a onclick="go('hud')">❤️ HUD Customizer</a>
      </div>
    </div>
    <button class="navbtn" onclick="go('about')">About</button>
    <button class="toggle" onclick="toggleTheme()" id="themeBtn">🌙 Night</button>
  </nav>
</header>

<main>

  <!-- HOME -->
  <section class="page active" id="home">
    <div class="hero">
      <h1>MCessentials</h1>
      <p>Free, browser-based creator tools. Everything runs locally on your device — nothing is ever uploaded to a server.</p>
    </div>

    <div class="sectionlabel">Character</div>
    <div class="grid">
      <div class="card block" onclick="homeSelect('skin')">
        <div class="icon-badge">🧍</div><h3>Skin Editor</h3>
        <p>Pixel-paint a blocky character skin with pencil, fill, and eyedropper tools.</p>
      </div>
      <div class="card block" onclick="homeSelect('cape')">
        <div class="icon-badge">🧥</div><h3>Cape Editor</h3>
        <p>Same pixel tools, sized for a cape — with a shape guide overlay.</p>
      </div>
      <div class="card block" onclick="homeSelect('avatar')">
        <div class="icon-badge">👤</div><h3>Avatar Maker</h3>
        <p>Upload any photo, turn it into a blocky pixel avatar.</p>
      </div>
    </div>

    <div class="sectionlabel">Color</div>
    <div class="grid">
      <div class="card block" onclick="homeSelect('picker')">
        <div class="icon-badge">🎨</div><h3>Color Picker</h3>
        <p>Upload an image, click a pixel, get its hex code instantly.</p>
      </div>
      <div class="card block" onclick="homeSelect('formatcodes')">
        <div class="icon-badge">🌈</div><h3>Format Code Maker</h3>
        <p>Write styled text with color/bold/italic codes and live preview.</p>
      </div>
      <div class="card block" onclick="homeSelect('palette')">
        <div class="icon-badge">🧬</div><h3>Palette Generator</h3>
        <p>Pull the 5 dominant colors out of any photo.</p>
      </div>
    </div>

    <div class="sectionlabel">Create</div>
    <div class="grid">
      <div class="card block" onclick="homeSelect('icon_painting')">
        <div class="icon-badge">🖼</div><h3>Custom Paintings</h3>
        <p>Frame any image as a wall painting texture.</p>
      </div>
      <div class="card block" onclick="homeSelect('icon_disc')">
        <div class="icon-badge">💿</div><h3>Music Disc Maker</h3>
        <p>Turn a photo into a labeled disc icon.</p>
      </div>
      <div class="card block" onclick="homeSelect('icon_totem')">
        <div class="icon-badge">🗿</div><h3>Totem Generator</h3>
        <p>Build a small glowing talisman icon.</p>
      </div>
      <div class="card block" onclick="homeSelect('icon_head')">
        <div class="icon-badge">🧊</div><h3>Player Heads</h3>
        <p>Crop a photo into a mini blocky head icon.</p>
      </div>
      <div class="card block" onclick="homeSelect('icon_emoji')">
        <div class="icon-badge">😀</div><h3>Custom Emojis</h3>
        <p>Crop and round any image into an emoji-sized icon.</p>
      </div>
    </div>

    <div class="sectionlabel">Tools</div>
    <div class="grid">
      <div class="card block" onclick="homeSelect('coords')">
        <div class="icon-badge">📍</div><h3>Coordinate Calculator</h3>
        <p>Convert coordinates between two linked grids at any ratio.</p>
      </div>
      <div class="card block" onclick="homeSelect('skinpack')">
        <div class="icon-badge">📦</div><h3>Skin Pack Maker</h3>
        <p>Bundle a skin and cape into one downloadable .zip pack.</p>
      </div>
      <div class="card block" onclick="homeSelect('merger')">
        <div class="icon-badge">🗂</div><h3>Texture Pack Merger</h3>
        <p>Combine several images into a single .zip download.</p>
      </div>
      <div class="card block" onclick="homeSelect('hud')">
        <div class="icon-badge">❤️</div><h3>HUD Customizer</h3>
        <p>Pick colors for a health/hunger/XP bar mockup and export the preset.</p>
      </div>
    </div>
  </section>

  <!-- PIXEL EDITOR (skin + cape share this) -->
  <section class="page" id="editor">
    <div class="tool-header">
      <h1 id="editorTitle">🧍 Skin Editor</h1>
      <p id="editorSub">Rotate and paint the 3D model directly, or use the 2D template below for pixel-precise edits.</p>
    </div>

    <div class="dropzone block" onclick="document.getElementById('editUpload').click()">
      Click to load an existing PNG to keep editing it
    </div>
    <input type="file" id="editUpload" accept="image/*">

    <div class="toolbar">
      <button class="toolbtn active" data-tool="pencil" onclick="setTool('pencil')">✏ Pencil</button>
      <button class="toolbtn" data-tool="eraser" onclick="setTool('eraser')">🧹 Eraser</button>
      <button class="toolbtn" data-tool="fill" onclick="setTool('fill')">🪣 Fill</button>
      <button class="toolbtn" data-tool="eyedrop" onclick="setTool('eyedrop')">💧 Eyedropper</button>
      <button class="toolbtn" onclick="undo()">↶ Undo</button>
      <button class="toolbtn" onclick="redo()">↷ Redo</button>
      <button class="toolbtn" onclick="clearCanvas()">🗑 Clear</button>
      <button class="toolbtn" id="template2DToggle" onclick="toggle2DPanel()">🧩 2D Template</button>
      <select class="toolbtn" id="sizeSelect" onchange="changeSize()">
        <option value="32x32">32×32</option>
        <option value="64x64">64×64</option>
        <option value="64x32">64×32 (cape)</option>
      </select>
    </div>

    <div class="stack3d2d">
      <div class="stackcol">
        <!-- 3D panel: skin only, on top by default -->
        <div id="threeDPanel" class="block" style="padding:16px;">
          <div style="display:flex; gap:8px; margin-bottom:10px;">
            <button class="toolbtn active" id="poseStanding" onclick="setPose('standing')">🧍 Standing</button>
            <button class="toolbtn" id="poseTpose" onclick="setPose('tpose')">🤸 T-Pose</button>
          </div>
          <div style="font-size:12px; color:var(--muted); margin-bottom:10px;">Drag empty space to rotate • Scroll to zoom • Click + drag on the model to paint</div>
          <div id="threeHost" style="width:100%; max-width:100%; height:380px; border-radius:12px; overflow:hidden; background:radial-gradient(circle at 50% 30%, rgba(var(--accent-rgb),0.12), transparent 70%);"></div>
        </div>

        <!-- 2D template panel: below 3D, off by default -->
        <div id="editorWrap" class="block" style="padding:16px; display:none;">
          <div id="template2DLabel" style="font-size:12px; color:var(--muted); margin-bottom:10px;">🧩 2D Template — colored regions show which part of the model each pixel belongs to</div>
          <div id="canvasStack" style="position:relative; display:inline-block;">
            <canvas id="editCanvas" style="position:relative;"></canvas>
            <canvas id="templateCanvas"></canvas>
            <canvas id="gridCanvas2"></canvas>
          </div>
        </div>
      </div>

      <div class="togglerail block">
        <div class="tr-title">View Options</div>
        <div class="tr-row"><span>Grid</span><label class="pillswitch"><input type="checkbox" id="toggleGridChk" checked onchange="onToggleGrid()"><span class="track"></span></label></div>
        <div class="tr-row"><span>Template</span><label class="pillswitch"><input type="checkbox" id="toggleTemplateChk" checked onchange="onToggleTemplate()"><span class="track"></span></label></div>
        <div class="tr-row"><span>Layer names</span><label class="pillswitch"><input type="checkbox" id="toggleLayerNamesChk" checked onchange="onToggleLayerNames()"><span class="track"></span></label></div>

        <div id="overlayPickerWrap" style="margin-top:6px;">
          <div class="tr-title">Outer layer — click a part</div>
          <svg id="overlayPicker" class="partpicker" viewBox="0 0 60 86" style="width:100%; max-width:120px; display:block; margin:0 auto;">
            <rect data-part="head" x="20" y="2" width="20" height="18" rx="2"></rect>
            <rect data-part="armR" x="2" y="22" width="12" height="30" rx="2"></rect>
            <rect data-part="body" x="16" y="22" width="28" height="30" rx="2"></rect>
            <rect data-part="armL" x="46" y="22" width="12" height="30" rx="2"></rect>
            <rect data-part="legR" x="16" y="54" width="13" height="30" rx="2"></rect>
            <rect data-part="legL" x="31" y="54" width="13" height="30" rx="2"></rect>
          </svg>
          <div style="font-size:10.5px; color:var(--muted); text-align:center; margin-top:6px;">white = overlay visible</div>
        </div>

        <div id="visibilityPickerWrap" style="margin-top:16px;">
          <div class="tr-title">Body parts — click to hide</div>
          <svg id="visibilityPicker" class="partpicker" viewBox="0 0 60 86" style="width:100%; max-width:120px; display:block; margin:0 auto;">
            <rect data-part="head" x="20" y="2" width="20" height="18" rx="2" class="on"></rect>
            <rect data-part="armR" x="2" y="22" width="12" height="30" rx="2" class="on"></rect>
            <rect data-part="body" x="16" y="22" width="28" height="30" rx="2" class="on"></rect>
            <rect data-part="armL" x="46" y="22" width="12" height="30" rx="2" class="on"></rect>
            <rect data-part="legR" x="16" y="54" width="13" height="30" rx="2" class="on"></rect>
            <rect data-part="legL" x="31" y="54" width="13" height="30" rx="2" class="on"></rect>
          </svg>
          <div style="font-size:10.5px; color:var(--muted); text-align:center; margin-top:6px;">gray = hidden — click again to bring back</div>
        </div>
      </div>
    </div>

    <!-- color panel -->
    <div class="block colorpanel" style="margin-top:16px;">
      <div>
        <div class="wheel-wrap">
          <canvas id="wheelCanvas" width="170" height="170"></canvas>
          <div class="wheel-marker" id="wheelMarker"></div>
        </div>
        <input type="range" class="lightness-slider" id="lightnessSlider" min="0" max="100" value="55">
      </div>
      <div class="colorpanel-right">
        <div class="swatch-preview" id="swatchPreview" style="background:#e8b64f;"></div>
        <div class="hexrow">
          <input type="text" id="hexInput" value="#E8B64F" style="text-transform:uppercase;">
          <button class="copybtn" onclick="setColor(document.getElementById('hexInput').value)">Set</button>
        </div>
        <div class="lightendarken">
          <button class="toolbtn" onclick="lightenDarken(12)">☀ Lighten</button>
          <button class="toolbtn" onclick="lightenDarken(-12)">🌑 Darken</button>
        </div>
        <div class="palette-grid" id="paletteGrid"></div>
        <button class="toolbtn" onclick="downloadCanvas()">⬇ Download PNG</button>
        <input type="color" id="colorPicker" value="#e8b64f" style="display:none;">
      </div>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links">
        <a onclick="go('picker')">Color Picker</a>
      </div>
    </div>
  </section>

  <!-- AVATAR MAKER -->
  <section class="page" id="avatar">
    <div class="tool-header">
      <h1>👤 Avatar Maker</h1>
      <p>Upload any photo and turn it into a blocky pixel avatar. Adjust the resolution to taste, then download.</p>
    </div>

    <div class="dropzone block" onclick="document.getElementById('avatarUpload').click()">
      Click to choose a photo
    </div>
    <input type="file" id="avatarUpload" accept="image/*">

    <div class="avatar-layout">
      <div class="avatar-panel block">
        <canvas id="avatarPreview" width="256" height="256"></canvas>
      </div>
      <div class="avatar-panel block" style="text-align:left">
        <div class="slider-row">
          <span>Blockiness</span>
          <input type="range" id="pixelSize" min="4" max="64" value="24">
          <span id="pixelSizeLabel">24px</span>
        </div>
        <button class="toolbtn" style="width:100%" onclick="downloadAvatar()">⬇ Download PNG</button>
      </div>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links">
        <a onclick="openEditor('skin')">Skin Editor</a>
        <a onclick="go('picker')">Color Picker</a>
      </div>
    </div>
  </section>

  <!-- FORMAT CODE MAKER -->
  <section class="page" id="formatcodes">
    <div class="tool-header">
      <h1>🌈 Format Code Maker</h1>
      <p>Type your text, click a color or style to insert its code at the cursor, then copy the ready-to-use output.</p>
    </div>

    <div class="toolbar" id="fcPalette"></div>
    <div class="toolbar">
      <button class="toolbtn" onclick="fcInsert('&l')">Bold</button>
      <button class="toolbtn" onclick="fcInsert('&o')">Italic</button>
      <button class="toolbtn" onclick="fcInsert('&n')">Underline</button>
      <button class="toolbtn" onclick="fcInsert('&r')">Reset</button>
      <button class="toolbtn" onclick="document.getElementById('fcInput').value=''; fcRender();">🗑 Clear</button>
    </div>

    <textarea id="fcInput" class="block" style="width:100%; min-height:70px; background:transparent; color:var(--text); padding:14px; font-family:'Courier New',monospace; font-size:14px;" placeholder="Type your text here...">&6Hello &l&bWorld&r!</textarea>

    <div class="sectionlabel">Live Preview</div>
    <div id="fcPreview" class="block" style="padding:18px; font-family:'Courier New',monospace; font-size:16px; min-height:30px; background:#12141a;"></div>

    <div class="sectionlabel">Copy-ready output</div>
    <div class="hexrow">
      <input type="text" id="fcOutput" readonly>
      <button class="copybtn" onclick="navigator.clipboard.writeText(document.getElementById('fcOutput').value)">Copy</button>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links"><a onclick="go('picker')">Color Picker</a></div>
    </div>
  </section>

  <!-- PALETTE GENERATOR -->
  <section class="page" id="palette">
    <div class="tool-header">
      <h1>🧬 Palette Generator</h1>
      <p>Upload a photo to pull out its 5 most dominant colors. Click any swatch to copy its hex code.</p>
    </div>

    <div class="dropzone block" onclick="document.getElementById('paletteUpload').click()">
      Click to choose an image
    </div>
    <input type="file" id="paletteUpload" accept="image/*">

    <div id="paletteResult" class="palette-result"></div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links"><a onclick="go('picker')">Color Picker</a></div>
    </div>
  </section>

  <!-- ICON MAKER (paintings / discs / totems / heads / emojis) -->
  <section class="page" id="iconmaker">
    <div class="tool-header">
      <h1 id="iconTitle">🖼 Custom Paintings</h1>
      <p id="iconSub">Upload an image, pick an accent color, and download the framed result.</p>
    </div>

    <div class="dropzone block" onclick="document.getElementById('iconUpload').click()">
      Click to choose an image
    </div>
    <input type="file" id="iconUpload" accept="image/*">

    <div class="avatar-layout">
      <div class="avatar-panel block">
        <canvas id="iconCanvas" width="300" height="300"></canvas>
      </div>
      <div class="avatar-panel block" style="text-align:left">
        <div class="slider-row"><span>Frame color</span><input type="color" class="colorinput" id="iconColor" value="#e8b64f" style="flex:1"></div>
        <button class="toolbtn" style="width:100%" onclick="downloadIcon()">⬇ Download PNG</button>
      </div>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links">
        <a onclick="go('avatar')">Avatar Maker</a>
        <a onclick="go('palette')">Palette Generator</a>
      </div>
    </div>
  </section>

  <!-- SKIN PACK MAKER -->
  <section class="page" id="skinpack">
    <div class="tool-header">
      <h1>📦 Skin Pack Maker</h1>
      <p>Upload a skin and (optionally) a cape, name your pack, and download a ready-to-share .zip.</p>
    </div>

    <div class="avatar-layout">
      <div class="avatar-panel block">
        <h3 style="margin-top:0; font-size:14px;">Skin PNG</h3>
        <div class="dropzone block" onclick="document.getElementById('spSkinUpload').click()" style="margin-bottom:0;">Click to choose skin.png</div>
        <input type="file" id="spSkinUpload" accept="image/*">
        <div id="spSkinName" style="font-size:12px; color:var(--muted); margin-top:8px;"></div>
      </div>
      <div class="avatar-panel block">
        <h3 style="margin-top:0; font-size:14px;">Cape PNG (optional)</h3>
        <div class="dropzone block" onclick="document.getElementById('spCapeUpload').click()" style="margin-bottom:0;">Click to choose cape.png</div>
        <input type="file" id="spCapeUpload" accept="image/*">
        <div id="spCapeName" style="font-size:12px; color:var(--muted); margin-top:8px;"></div>
      </div>
    </div>

    <div class="hexrow" style="margin-top:16px;">
      <input type="text" id="spPackName" placeholder="Pack name (e.g. MyCharacter)" value="MyPack">
    </div>
    <button class="toolbtn" onclick="buildSkinPack()">📦 Build & Download Pack</button>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links">
        <a onclick="openEditor('skin')">Skin Editor</a>
        <a onclick="openEditor('cape')">Cape Editor</a>
      </div>
    </div>
  </section>

  <!-- TEXTURE PACK MERGER -->
  <section class="page" id="merger">
    <div class="tool-header">
      <h1>🗂 Texture Pack Merger</h1>
      <p>Upload several images and combine them into a single downloadable .zip, keeping their original filenames.</p>
    </div>

    <div class="dropzone block" onclick="document.getElementById('mergerUpload').click()">
      Click to choose multiple images
    </div>
    <input type="file" id="mergerUpload" accept="image/*" multiple>

    <div id="mergerList" class="grid" style="margin-top:10px;"></div>

    <div class="hexrow" style="margin-top:16px;">
      <input type="text" id="mergerZipName" placeholder="Zip file name" value="texture-pack">
    </div>
    <button class="toolbtn" onclick="buildMergedZip()">🗂 Merge & Download .zip</button>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links"><a onclick="go('skinpack')">Skin Pack Maker</a></div>
    </div>
  </section>

  <!-- HUD CUSTOMIZER -->
  <section class="page" id="hud">
    <div class="tool-header">
      <h1>❤️ HUD Customizer</h1>
      <p>Pick colors for a health / hunger / experience bar mockup, then export your color preset as JSON.</p>
    </div>

    <div class="avatar-layout">
      <div class="avatar-panel block" style="text-align:left">
        <div class="slider-row"><span style="width:110px;">Health bar</span><input type="color" id="hudHealth" value="#ff5555" style="flex:1; height:34px; border-radius:8px; border:1px solid var(--line); background:none;"></div>
        <div class="slider-row"><span style="width:110px;">Hunger bar</span><input type="color" id="hudHunger" value="#c97b3f" style="flex:1; height:34px; border-radius:8px; border:1px solid var(--line); background:none;"></div>
        <div class="slider-row"><span style="width:110px;">XP bar</span><input type="color" id="hudXp" value="#7ed67e" style="flex:1; height:34px; border-radius:8px; border:1px solid var(--line); background:none;"></div>
        <button class="toolbtn" style="width:100%; margin-top:10px;" onclick="downloadHudPreset()">⬇ Download preset (.json)</button>
      </div>
      <div class="avatar-panel block">
        <div style="width:100%; max-width:260px; margin:0 auto;">
          <div style="font-size:11px; color:var(--muted); margin-bottom:4px;">Health</div>
          <div style="height:14px; border-radius:7px; background:rgba(255,255,255,0.08); overflow:hidden; margin-bottom:12px;"><div id="hudHealthBar" style="height:100%; width:80%; background:#ff5555; transition:.2s;"></div></div>
          <div style="font-size:11px; color:var(--muted); margin-bottom:4px;">Hunger</div>
          <div style="height:14px; border-radius:7px; background:rgba(255,255,255,0.08); overflow:hidden; margin-bottom:12px;"><div id="hudHungerBar" style="height:100%; width:60%; background:#c97b3f; transition:.2s;"></div></div>
          <div style="font-size:11px; color:var(--muted); margin-bottom:4px;">Experience</div>
          <div style="height:14px; border-radius:7px; background:rgba(255,255,255,0.08); overflow:hidden;"><div id="hudXpBar" style="height:100%; width:35%; background:#7ed67e; transition:.2s;"></div></div>
        </div>
      </div>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links"><a onclick="go('formatcodes')">Format Code Maker</a></div>
    </div>
  </section>

  <!-- COORDINATE CALCULATOR -->
  <section class="page" id="coords">
    <div class="tool-header">
      <h1>📍 Coordinate Calculator</h1>
      <p>Convert a coordinate between two linked grids at any scale ratio.</p>
    </div>

    <div class="avatar-layout">
      <div class="avatar-panel block" style="text-align:left">
        <h3 style="margin-top:0">Grid A</h3>
        <div class="hexrow"><input type="text" id="ax" placeholder="X"></div>
        <div class="hexrow"><input type="text" id="ay" placeholder="Y"></div>
        <div class="hexrow"><input type="text" id="az" placeholder="Z"></div>
      </div>
      <div class="avatar-panel block" style="text-align:left">
        <h3 style="margin-top:0">Grid B <span style="font-size:11px; color:var(--muted); font-weight:400">(scale ÷<span id="ratioLabel">8</span>)</span></h3>
        <div class="hexrow"><input type="text" id="bx" readonly></div>
        <div class="hexrow"><input type="text" id="by" readonly></div>
        <div class="hexrow"><input type="text" id="bz" readonly></div>
      </div>
    </div>
    <div class="slider-row">
      <span>Scale ratio (1 in B = this many in A)</span>
      <input type="range" id="ratioSlider" min="2" max="16" value="8">
      <span id="ratioLabel2">8</span>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links"><a onclick="go('formatcodes')">Format Code Maker</a></div>
    </div>
  </section>

  <!-- COLOR PICKER -->
  <section class="page" id="picker">
    <div class="tool-header">
      <h1>🎨 Color Picker</h1>
      <p>Upload an image below. Hover to magnify, click anywhere to grab that pixel's hex code. Nothing leaves your device.</p>
    </div>

    <div class="dropzone block" onclick="document.getElementById('imgInput').click()">
      Click to choose an image (PNG, JPG, etc.)
    </div>
    <input type="file" id="imgInput" accept="image/*">

    <div class="workbench" id="workbench">
      <div id="canvasWrap" class="block">
        <div class="toolbar" style="margin-bottom:12px;">
          <button class="toolbtn" onclick="picZoom(0.8)">➖ Zoom out</button>
          <button class="toolbtn" onclick="picZoom(1.25)">➕ Zoom in</button>
          <button class="toolbtn" onclick="picZoomReset()">⤢ Fit</button>
        </div>
        <div id="picCanvasScroll" style="overflow:auto; max-height:520px; text-align:center;">
          <canvas id="picCanvas"></canvas>
        </div>
      </div>
      <div class="result block">
        <div class="swatch" id="swatch" style="background:#333"></div>
        <div class="hexrow">
          <input type="text" id="hexOut" value="#------" readonly>
          <button class="copybtn" onclick="copyHex()">Copy</button>
        </div>
        <div class="hexrow"><input type="text" id="rgbOut" value="rgb(-,-,-)" readonly></div>
        <div class="zoomhint">Hover to magnify, click anywhere on the image to sample that pixel.</div>
      </div>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links">
        <a onclick="openEditor('skin')">Skin Editor</a>
      </div>
    </div>
  </section>

  <!-- floating magnifier loupe, positioned via JS -->
  <div id="magnifier" style="display:none; position:fixed; z-index:200; background:#ffffff; border-radius:10px; box-shadow:0 10px 30px rgba(0,0,0,0.45); padding:10px; pointer-events:none;">
    <canvas id="magnifierCanvas" width="180" height="180" style="display:block; border-radius:4px; overflow:hidden;"></canvas>
    <div id="magnifierHex" style="text-align:center; font-family:'Courier New',monospace; font-size:12px; color:#111; margin-top:6px; font-weight:700;">#------</div>
  </div>

  <!-- ABOUT -->
  <section class="page" id="about">
    <div class="tool-header">
      <h1>About MCessentials</h1>
      <p>A small collection of browser-only creator tools. No accounts, no server uploads — everything runs on your device using standard browser APIs (Canvas, FileReader, Blob).</p>
    </div>
  </section>

</main>

<footer>MCessentials · built for GitHub Pages · runs entirely in your browser</footer>

<script>
/* ---------- STARFIELD ---------- */
const starsEl = document.getElementById('stars');
for(let i=0;i<140;i++){
  const d = document.createElement('div');
  d.className='dot';
  d.style.left = Math.random()*100+'%';
  d.style.top = Math.random()*100+'%';
  d.style.animationDelay = (Math.random()*4)+'s';
  d.style.width = d.style.height = (Math.random()*1.6+1)+'px';
  starsEl.appendChild(d);
}

/* ---------- ACCENT COLOR ---------- */
function hexToRgb(hex){
  const m = hex.replace('#','');
  const bigint = parseInt(m,16);
  return [(bigint>>16)&255,(bigint>>8)&255,bigint&255].join(',');
}
function setAccent(hex){
  document.documentElement.style.setProperty('--accent', hex);
  document.documentElement.style.setProperty('--accent-rgb', hexToRgb(hex));
  localStorage.setItem('mce_accent', hex);
}
const accentInput = document.getElementById('accentInput');
const savedAccent = localStorage.getItem('mce_accent');
if(savedAccent){ accentInput.value = savedAccent; setAccent(savedAccent); }
accentInput.addEventListener('input', e=>setAccent(e.target.value));

/* ---------- NAV ---------- */
function go(page){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(page).classList.add('active');
  window.scrollTo({top:0, behavior:'smooth'});
}
function toggleTheme(){
  const body = document.body;
  const btn = document.getElementById('themeBtn');
  if(body.dataset.theme === 'night'){ body.dataset.theme = 'day'; btn.textContent = '☀ Day'; }
  else { body.dataset.theme = 'night'; btn.textContent = '🌙 Night'; }
}

/* ---------- SHARED UV ATLAS ----------
   Verified against the Minecraft skin technical spec (minotar/skin-spec):
   64x64 base layer + 1.8+ overlay layer, with genuinely separate left/right
   arm and leg regions (not mirrored) — this matches how the game actually
   reads the file. Coordinates are [x, y, width, height] in texture pixels. */
const ATLAS = {
  skin: {
    base: {
      head: {front:[8,8,8,8], back:[24,8,8,8], right:[0,8,8,8], left:[16,8,8,8], top:[8,0,8,8], bottom:[16,0,8,8]},
      body: {front:[20,20,8,12], back:[32,20,8,12], right:[16,20,4,12], left:[28,20,4,12], top:[20,16,8,4], bottom:[28,16,8,4]},
      armR: {front:[44,20,4,12], back:[52,20,4,12], right:[40,20,4,12], left:[48,20,4,12], top:[44,16,4,4], bottom:[48,16,4,4]},
      armL: {front:[36,52,4,12], back:[44,52,4,12], right:[32,52,4,12], left:[40,52,4,12], top:[36,48,4,4], bottom:[40,48,4,4]},
      legR: {front:[4,20,4,12], back:[12,20,4,12], right:[0,20,4,12], left:[8,20,4,12], top:[4,16,4,4], bottom:[8,16,4,4]},
      legL: {front:[20,52,4,12], back:[28,52,4,12], right:[16,52,4,12], left:[24,52,4,12], top:[20,48,4,4], bottom:[24,48,4,4]}
    },
    overlay: {
      head: {front:[40,8,8,8], back:[56,8,8,8], right:[32,8,8,8], left:[48,8,8,8], top:[40,0,8,8], bottom:[48,0,8,8]},
      body: {front:[20,36,8,12], back:[32,36,8,12], right:[16,36,4,12], left:[28,36,4,12], top:[20,32,8,4], bottom:[28,32,8,4]},
      armR: {front:[44,36,4,12], back:[52,36,4,12], right:[40,36,4,12], left:[48,36,4,12], top:[44,32,4,4], bottom:[48,32,4,4]},
      armL: {front:[52,52,4,12], back:[60,52,4,12], right:[48,52,4,12], left:[56,52,4,12], top:[52,48,4,4], bottom:[56,48,4,4]},
      legR: {front:[4,36,4,12], back:[12,36,4,12], right:[0,36,4,12], left:[8,36,4,12], top:[4,32,4,4], bottom:[8,32,4,4]},
      legL: {front:[4,52,4,12], back:[12,52,4,12], right:[0,52,4,12], left:[8,52,4,12], top:[4,48,4,4], bottom:[8,48,4,4]}
    }
  },
  cape: {
    base: { cape: {front:[1,1,10,16], back:[12,1,10,16]} }
  }
};
function facesToArray(f){ return [f.right,f.left,f.top,f.bottom,f.front,f.back]; }
function partColor(part){
  return {head:'232,182,79', body:'185,138,240', armR:'79,211,232', armL:'79,211,232', legR:'126,214,126', legL:'126,214,126', cape:'185,138,240'}[part] || '255,255,255';
}
let overlayParts = {head:false, body:false, armR:false, armL:false, legR:false, legL:false};

/* ---------- PIXEL EDITOR (skin + cape) ---------- */
const editCanvas = document.getElementById('editCanvas');
const editCtx = editCanvas.getContext('2d', { willReadFrequently:true });
const templateCanvas = document.getElementById('templateCanvas');
const templateCtx = templateCanvas.getContext('2d');
const gridCanvas2 = document.getElementById('gridCanvas2');
const gridCtx2 = gridCanvas2.getContext('2d');
let gridW=64, gridH=64, cellSize=9, currentTool='pencil', drawing=false;
let history=[], historyIdx=-1;
let currentEditorKind='skin';
let gridOn=true, templateOn=true, layerNamesOn=true, panel2DOn=false;
let defaultFilled=false;

function setupGrid(w,h){
  gridW=w; gridH=h;
  cellSize = Math.min(14, Math.floor(620/Math.max(w,h)));
  [editCanvas, templateCanvas, gridCanvas2].forEach(c=>{ c.width=w*cellSize; c.height=h*cellSize; });
  editCtx.imageSmoothingEnabled=false;
  editCtx.clearRect(0,0,editCanvas.width,editCanvas.height);
  drawGridLines2();
  drawTemplate();
  history=[editCtx.getImageData(0,0,editCanvas.width,editCanvas.height)];
  historyIdx=0;
}
function drawGridLines2(){
  gridCtx2.clearRect(0,0,gridCanvas2.width,gridCanvas2.height);
  if(!gridOn) return;
  gridCtx2.save();
  gridCtx2.strokeStyle='rgba(255,255,255,0.15)';
  gridCtx2.lineWidth=1;
  for(let x=0;x<=gridW;x++){ gridCtx2.beginPath(); gridCtx2.moveTo(x*cellSize+0.5,0); gridCtx2.lineTo(x*cellSize+0.5,gridH*cellSize); gridCtx2.stroke(); }
  for(let y=0;y<=gridH;y++){ gridCtx2.beginPath(); gridCtx2.moveTo(0,y*cellSize+0.5); gridCtx2.lineTo(gridW*cellSize,y*cellSize+0.5); gridCtx2.stroke(); }
  gridCtx2.restore();
}
function drawRegionSet(layerObj, layerName){
  Object.entries(layerObj).forEach(([partName, faces])=>{
    const col = partColor(partName);
    Object.entries(faces).forEach(([faceName, rect])=>{
      const [px,py,pw,ph] = rect;
      const x=px*cellSize, y=py*cellSize, w=pw*cellSize, h=ph*cellSize;
      templateCtx.fillStyle = `rgba(${col},${layerName==='overlay'?0.14:0.16})`;
      templateCtx.strokeStyle = `rgba(${col},${layerName==='overlay'?0.5:0.55})`;
      templateCtx.lineWidth=1;
      templateCtx.setLineDash(layerName==='overlay' ? [3,2] : []);
      templateCtx.fillRect(x,y,w,h);
      templateCtx.strokeRect(x+0.5,y+0.5,w-1,h-1);
      templateCtx.setLineDash([]);
      if(layerNamesOn && w>=14 && h>=14){
        templateCtx.save();
        templateCtx.fillStyle='rgba(255,255,255,0.9)';
        templateCtx.font = Math.max(7,Math.min(11, Math.floor(Math.min(w,h)/2.6)))+'px sans-serif';
        templateCtx.textAlign='center'; templateCtx.textBaseline='middle';
        const label = faceName.slice(0,1).toUpperCase()+faceName.slice(1,3);
        if(h>w*1.5){
          templateCtx.translate(x+w/2, y+h/2);
          templateCtx.rotate(-Math.PI/2);
          templateCtx.fillText(label,0,0);
        } else {
          templateCtx.fillText(label, x+w/2, y+h/2);
        }
        templateCtx.restore();
      }
    });
  });
}
function drawTemplate(){
  templateCtx.clearRect(0,0,templateCanvas.width,templateCanvas.height);
  if(!templateOn) return;
  const atlas = currentEditorKind==='cape' ? ATLAS.cape : ATLAS.skin;
  drawRegionSet(atlas.base, 'base');
  if(atlas.overlay){
    Object.entries(atlas.overlay).forEach(([partName,faces])=>{
      if(overlayParts[partName]) drawRegionSet({[partName]:faces}, 'overlay');
    });
  }
}
function onToggleGrid(){ gridOn=document.getElementById('toggleGridChk').checked; drawGridLines2(); syncThreeWireframe(); }
function onToggleTemplate(){ templateOn=document.getElementById('toggleTemplateChk').checked; drawTemplate(); }
function onToggleLayerNames(){ layerNamesOn=document.getElementById('toggleLayerNamesChk').checked; drawTemplate(); }
function toggle2DPanel(){
  panel2DOn = !panel2DOn;
  document.getElementById('template2DToggle').classList.toggle('active', panel2DOn);
  document.getElementById('editorWrap').style.display = panel2DOn ? 'block' : 'none';
}

/* body-part overlay picker (replaces the old single global toggle) */
document.querySelectorAll('#overlayPicker rect').forEach(rect=>{
  rect.addEventListener('click', ()=>{
    const part = rect.dataset.part;
    overlayParts[part] = !overlayParts[part];
    rect.classList.toggle('on', overlayParts[part]);
    drawTemplate();
    syncThreeOverlayPart(part);
  });
});

/* body-part visibility picker (hide/show whole parts to isolate one thing) */
document.querySelectorAll('#visibilityPicker rect').forEach(rect=>{
  rect.addEventListener('click', ()=>{
    const part = rect.dataset.part;
    const nowVisible = !partVisible[part];
    rect.classList.toggle('on', nowVisible);
    setPartVisible(part, nowVisible);
  });
});

function openEditor(kind){
  go('editor');
  currentEditorKind = kind;
  document.getElementById('editorTitle').textContent = kind==='cape' ? '🧥 Cape Editor' : '🧍 Skin Editor';
  document.getElementById('editorSub').textContent = kind==='cape'
    ? 'Use the 2D template below for pixel-precise edits.'
    : 'Rotate and paint the 3D model directly, or use the 2D template below for pixel-precise edits.';
  document.getElementById('sizeSelect').value = kind==='cape' ? '64x32' : '64x64';
  document.getElementById('threeDPanel').style.display = kind==='cape' ? 'none' : 'block';
  document.getElementById('overlayPickerWrap').style.display = kind==='cape' ? 'none' : 'block';
  document.getElementById('visibilityPickerWrap').style.display = kind==='cape' ? 'none' : 'block';
  changeSize();
}
function changeSize(){
  const [w,h] = document.getElementById('sizeSelect').value.split('x').map(Number);
  setupGrid(w,h);
  if(currentEditorKind==='skin'){
    if(w===64 && h===64){
      if(!threeInited) initThree();
      if(!defaultFilled){ fillDefaultSkin(); defaultFilled=true; pushHistory(); }
      hideThreeNotice();
      updateSkinTexture();
    } else if(threeInited){
      showThreeNotice();
    }
  }
}
setupGrid(64,64);
function setTool(tool){
  currentTool=tool;
  document.querySelectorAll('.toolbar .toolbtn[data-tool]').forEach(b=>b.classList.remove('active'));
  document.querySelector(`.toolbtn[data-tool="${tool}"]`).classList.add('active');
}
function cellFromEvent(e){
  const rect = editCanvas.getBoundingClientRect();
  const scaleX = editCanvas.width/rect.width, scaleY = editCanvas.height/rect.height;
  const x = Math.floor((e.clientX-rect.left)*scaleX/cellSize);
  const y = Math.floor((e.clientY-rect.top)*scaleY/cellSize);
  return {x,y};
}
function paintCell(x,y,color){
  if(x<0||y<0||x>=gridW||y>=gridH) return;
  if(color===null){ editCtx.clearRect(x*cellSize,y*cellSize,cellSize,cellSize); }
  else{ editCtx.fillStyle=color; editCtx.fillRect(x*cellSize,y*cellSize,cellSize,cellSize); }
}
function getCellColor(x,y){
  if(x<0||y<0||x>=gridW||y>=gridH) return null;
  const cx = Math.min(editCanvas.width-1, x*cellSize+Math.floor(cellSize/2));
  const cy = Math.min(editCanvas.height-1, y*cellSize+Math.floor(cellSize/2));
  const d = editCtx.getImageData(cx,cy,1,1).data;
  if(d[3]===0) return null;
  return '#'+[d[0],d[1],d[2]].map(v=>v.toString(16).padStart(2,'0')).join('');
}
function floodFill(x,y,target,fillColor){
  if(x<0||y<0||x>=gridW||y>=gridH) return;
  const stack=[[x,y]]; const visited = new Set();
  while(stack.length){
    const [cx,cy] = stack.pop();
    const key=cx+','+cy;
    if(visited.has(key)) continue;
    if(cx<0||cy<0||cx>=gridW||cy>=gridH) continue;
    const c = getCellColor(cx,cy);
    if(c!==target) continue;
    visited.add(key);
    paintCell(cx,cy,fillColor);
    stack.push([cx+1,cy],[cx-1,cy],[cx,cy+1],[cx,cy-1]);
  }
}
function handlePaint(e){
  const {x,y} = cellFromEvent(e);
  const color = document.getElementById('colorPicker').value;
  if(currentTool==='pencil') paintCell(x,y,color);
  else if(currentTool==='eraser') paintCell(x,y,null);
  else if(currentTool==='eyedrop'){ const c=getCellColor(x,y); if(c) setColor(c); setTool('pencil'); }
  else if(currentTool==='fill'){ floodFill(x,y,getCellColor(x,y),color); }
  updateSkinTexture();
}
editCanvas.addEventListener('mousedown', e=>{ drawing=true; handlePaint(e); });
editCanvas.addEventListener('mousemove', e=>{ if(drawing && (currentTool==='pencil'||currentTool==='eraser')) handlePaint(e); });
window.addEventListener('mouseup', ()=>{ if(drawing){ drawing=false; pushHistory(); }});
function pushHistory(){
  history = history.slice(0, historyIdx+1);
  history.push(editCtx.getImageData(0,0,editCanvas.width,editCanvas.height));
  if(history.length>40) history.shift();
  historyIdx = history.length-1;
  updateSkinTexture();
}
function undo(){ if(historyIdx>0){ historyIdx--; editCtx.putImageData(history[historyIdx],0,0); updateSkinTexture(); } }
function redo(){ if(historyIdx<history.length-1){ historyIdx++; editCtx.putImageData(history[historyIdx],0,0); updateSkinTexture(); } }
function clearCanvas(){ editCtx.clearRect(0,0,editCanvas.width,editCanvas.height); pushHistory(); }
document.getElementById('editUpload').addEventListener('change', e=>{
  const file = e.target.files[0]; if(!file) return;
  const img = new Image();
  img.onload = ()=>{
    editCtx.clearRect(0,0,editCanvas.width,editCanvas.height);
    editCtx.drawImage(img,0,0,gridW*cellSize,gridH*cellSize);
    defaultFilled = true;
    pushHistory();
  };
  img.src = URL.createObjectURL(file);
});
function downloadCanvas(){
  const off = document.createElement('canvas');
  off.width=gridW; off.height=gridH;
  const offCtx = off.getContext('2d');
  offCtx.imageSmoothingEnabled=false;
  offCtx.drawImage(editCanvas,0,0,gridW,gridH);
  const a = document.createElement('a');
  a.download = 'texture.png';
  a.href = off.toDataURL('image/png');
  a.click();
}
function fillDefaultSkin(){
  const c1='#8b91a3', c2='#767c8f';
  Object.values(ATLAS.skin.base).forEach(faces=>{
    Object.values(faces).forEach(([px,py,pw,ph])=>{
      for(let x=px;x<px+pw;x++){
        for(let y=py;y<py+ph;y++){
          paintCell(x,y,(x+y)%2===0 ? c1 : c2);
        }
      }
    });
  });
}
const palette = ['#000000','#ffffff','#8d94a3','#e8b64f','#4fd3e8','#b98af0','#7ed67e','#e05d5d','#c97b3f','#3f6cc9','#ffe1b3','#5a3d2b'];
const paletteGrid = document.getElementById('paletteGrid');
palette.forEach(c=>{
  const b = document.createElement('button');
  b.className='swatch-btn'; b.style.background=c;
  b.onclick=()=>setColor(c);
  paletteGrid.appendChild(b);
});

/* ---------- COLOR WHEEL / HEX / LIGHTEN-DARKEN ---------- */
function hexToRgbArr(hex){
  hex = hex.replace('#','');
  if(hex.length===3) hex = hex.split('').map(c=>c+c).join('');
  const num = parseInt(hex,16);
  return [(num>>16)&255,(num>>8)&255,num&255];
}
function rgbToHex(r,g,b){
  return '#'+[r,g,b].map(v=>Math.max(0,Math.min(255,Math.round(v))).toString(16).padStart(2,'0')).join('').toUpperCase();
}
function rgbToHsl(r,g,b){
  r/=255; g/=255; b/=255;
  const max=Math.max(r,g,b), min=Math.min(r,g,b);
  let h,s,l=(max+min)/2;
  if(max===min){ h=s=0; }
  else{
    const d=max-min;
    s = l>0.5 ? d/(2-max-min) : d/(max+min);
    switch(max){
      case r: h=(g-b)/d+(g<b?6:0); break;
      case g: h=(b-r)/d+2; break;
      default: h=(r-g)/d+4;
    }
    h/=6;
  }
  return [h*360, s*100, l*100];
}
function hslToRgb(h,s,l){
  h=((h%360)+360)%360; h/=360; s/=100; l/=100;
  let r,g,b;
  if(s===0){ r=g=b=l; }
  else{
    const hue2rgb=(p,q,t)=>{
      if(t<0) t+=1; if(t>1) t-=1;
      if(t<1/6) return p+(q-p)*6*t;
      if(t<1/2) return q;
      if(t<2/3) return p+(q-p)*(2/3-t)*6;
      return p;
    };
    const q = l<0.5 ? l*(1+s) : l+s-l*s;
    const p = 2*l-q;
    r=hue2rgb(p,q,h+1/3); g=hue2rgb(p,q,h); b=hue2rgb(p,q,h-1/3);
  }
  return [r*255,g*255,b*255];
}
let curHue=42, curSat=75, curLight=55;
function setColor(hex, fromWheel){
  if(!hex) return;
  if(hex[0]!=='#') hex='#'+hex;
  if(!/^#[0-9a-fA-F]{6}$/.test(hex)) return;
  document.getElementById('colorPicker').value=hex;
  document.getElementById('hexInput').value=hex.toUpperCase();
  document.getElementById('swatchPreview').style.background=hex;
  if(!fromWheel){
    const [r,g,b]=hexToRgbArr(hex);
    const [h,s,l]=rgbToHsl(r,g,b);
    curHue=h; curSat=s; curLight=l;
    document.getElementById('lightnessSlider').value=Math.round(l);
    positionWheelMarker();
  }
}
function positionWheelMarker(){
  const center=85, radius=75;
  const rad = curHue*Math.PI/180;
  const r = (curSat/100)*radius;
  const marker = document.getElementById('wheelMarker');
  marker.style.left = (center + Math.cos(rad)*r)+'px';
  marker.style.top = (center + Math.sin(rad)*r)+'px';
}
function drawWheel(){
  const canvas = document.getElementById('wheelCanvas');
  const ctx = canvas.getContext('2d');
  const size=170, center=85, radius=82;
  const img = ctx.createImageData(size,size);
  for(let y=0;y<size;y++){
    for(let x=0;x<size;x++){
      const dx=x-center, dy=y-center;
      const dist=Math.sqrt(dx*dx+dy*dy);
      const idx=(y*size+x)*4;
      if(dist<=radius){
        let angle=Math.atan2(dy,dx)*180/Math.PI; if(angle<0) angle+=360;
        const sat=Math.min(100,(dist/radius)*100);
        const [r,g,b]=hslToRgb(angle,sat,50);
        img.data[idx]=r; img.data[idx+1]=g; img.data[idx+2]=b; img.data[idx+3]=255;
      }
    }
  }
  ctx.putImageData(img,0,0);
}
let wheelDragging=false;
const wheelCanvasEl = document.getElementById('wheelCanvas');
function wheelPick(e){
  const rect=wheelCanvasEl.getBoundingClientRect();
  const x=(e.clientX-rect.left) * (170/rect.width);
  const y=(e.clientY-rect.top) * (170/rect.height);
  const dx=x-85, dy=y-85;
  let dist=Math.min(Math.sqrt(dx*dx+dy*dy),82);
  let angle=Math.atan2(dy,dx)*180/Math.PI; if(angle<0) angle+=360;
  curHue=angle; curSat=(dist/82)*100;
  applyCurrentHSL();
}
wheelCanvasEl.addEventListener('mousedown', e=>{ wheelDragging=true; wheelPick(e); });
window.addEventListener('mousemove', e=>{ if(wheelDragging) wheelPick(e); });
window.addEventListener('mouseup', ()=>wheelDragging=false);
document.getElementById('lightnessSlider').addEventListener('input', e=>{ curLight=parseFloat(e.target.value); applyCurrentHSL(); });
function applyCurrentHSL(){
  const [r,g,b]=hslToRgb(curHue,curSat,curLight);
  setColor(rgbToHex(r,g,b), true);
  positionWheelMarker();
}
document.getElementById('hexInput').addEventListener('change', e=>setColor(e.target.value));
function lightenDarken(amount){
  curLight = Math.max(0, Math.min(100, curLight+amount));
  document.getElementById('lightnessSlider').value=Math.round(curLight);
  applyCurrentHSL();
}
drawWheel();
setColor('#e8b64f');

/* ---------- 3D SKIN PREVIEW ---------- */
let threeInited=false;
let scene3, camera3, renderer3, raycaster3, charGroup, skinCanvasClean, skinTexture;
let rotY=0.5, rotX=0.15, zoomDist=3.4;
let dragging3D=false, painting3D=false, lastPX=0, lastPY=0;
let wireframeMeshes=[], baseMeshByPart={}, overlayMeshByPart={}, limbPivots={}, edgesByPart={}, overlayEdgesByPart={};
let partVisible = {head:true, body:true, armR:true, armL:true, legR:true, legL:true};
const S = 0.045;

function ensureThreeNotice(host){
  if(document.getElementById('threeDNotice')) return;
  const notice=document.createElement('div');
  notice.id='threeDNotice';
  notice.style.cssText='position:absolute; inset:0; display:none; align-items:center; justify-content:center; text-align:center; color:var(--muted); font-size:13px; padding:20px;';
  notice.textContent='Switch size to 64×64 above to see the 3D model.';
  host.style.position='relative';
  host.appendChild(notice);
}
function showThreeNotice(){ const n=document.getElementById('threeDNotice'); if(n) n.style.display='flex'; }
function hideThreeNotice(){ const n=document.getElementById('threeDNotice'); if(n) n.style.display='none'; }

function uvRect(px,py,pw,ph,texW,texH){
  const u0=px/texW, u1=(px+pw)/texW;
  const vTop=1-py/texH, vBottom=1-(py+ph)/texH;
  return [u0,vTop, u1,vTop, u0,vBottom, u1,vBottom];
}
function setBoxUV(geo, faces, texW, texH){
  const uvAttr = geo.attributes.uv;
  let offset=0;
  faces.forEach(f=>{
    const quad = uvRect(f[0],f[1],f[2],f[3],texW,texH);
    for(let i=0;i<4;i++){ uvAttr.array[offset*8 + i*2] = quad[i*2]; uvAttr.array[offset*8 + i*2+1] = quad[i*2+1]; }
    offset++;
  });
  uvAttr.needsUpdate = true;
}
function makePart(w,h,d,faces){
  const geo = new THREE.BoxGeometry(w*S,h*S,d*S);
  setBoxUV(geo, faces, 64, 64);
  const mat = new THREE.MeshBasicMaterial({ map: skinTexture, transparent:true, alphaTest:0.1, side:THREE.DoubleSide });
  return new THREE.Mesh(geo, mat);
}
function edgesFor(mesh){
  const e = new THREE.LineSegments(new THREE.EdgesGeometry(mesh.geometry), new THREE.LineBasicMaterial({color:0xffffff, transparent:true, opacity:0.35}));
  e.visible = gridOn;
  return e;
}
function overlayEdgesFor(mesh){
  const e = new THREE.LineSegments(new THREE.EdgesGeometry(mesh.geometry), new THREE.LineBasicMaterial({color:0xffffff, transparent:true, opacity:0.85}));
  e.visible = false;
  return e;
}
function buildStatic(partKey, dims, pos, overlayDims){
  const mesh = makePart(dims[0],dims[1],dims[2], facesToArray(ATLAS.skin.base[partKey]));
  mesh.position.copy(pos);
  charGroup.add(mesh);
  baseMeshByPart[partKey]=mesh;
  const edges = edgesFor(mesh); edges.position.copy(pos); charGroup.add(edges); wireframeMeshes.push(edges); edgesByPart[partKey]=edges;
  if(overlayDims && ATLAS.skin.overlay[partKey]){
    const om = makePart(overlayDims[0],overlayDims[1],overlayDims[2], facesToArray(ATLAS.skin.overlay[partKey]));
    om.position.copy(pos); om.visible=false;
    charGroup.add(om);
    overlayMeshByPart[partKey]=om;
    const oe = overlayEdgesFor(om); oe.position.copy(pos); charGroup.add(oe); overlayEdgesByPart[partKey]=oe;
  }
}
function buildLimb(partKey, dims, jointPos, overlayDims){
  const pivot = new THREE.Group();
  pivot.position.copy(jointPos);
  charGroup.add(pivot);
  const halfH = dims[1]*S/2;

  const mesh = makePart(dims[0],dims[1],dims[2], facesToArray(ATLAS.skin.base[partKey]));
  mesh.position.set(0,-halfH,0);
  pivot.add(mesh);
  baseMeshByPart[partKey]=mesh;

  const edges = edgesFor(mesh); edges.position.set(0,-halfH,0); pivot.add(edges); wireframeMeshes.push(edges); edgesByPart[partKey]=edges;

  if(overlayDims && ATLAS.skin.overlay[partKey]){
    const om = makePart(overlayDims[0],overlayDims[1],overlayDims[2], facesToArray(ATLAS.skin.overlay[partKey]));
    om.position.set(0,-halfH,0); om.visible=false;
    pivot.add(om);
    overlayMeshByPart[partKey]=om;
    const oe = overlayEdgesFor(om); oe.position.set(0,-halfH,0); pivot.add(oe); overlayEdgesByPart[partKey]=oe;
  }
  limbPivots[partKey]=pivot;
}
function initThree(){
  threeInited = true;
  const host = document.getElementById('threeHost');
  ensureThreeNotice(host);
  const W = host.clientWidth, H = host.clientHeight;
  scene3 = new THREE.Scene();
  camera3 = new THREE.PerspectiveCamera(45, W/H, 0.1, 100);
  renderer3 = new THREE.WebGLRenderer({ antialias:true, alpha:true });
  renderer3.setSize(W,H);
  renderer3.setPixelRatio(window.devicePixelRatio || 1);
  const holder = document.createElement('div');
  holder.style.cssText='width:100%; height:100%;';
  holder.appendChild(renderer3.domElement);
  host.insertBefore(holder, host.firstChild);

  skinCanvasClean = document.createElement('canvas');
  skinCanvasClean.width=64; skinCanvasClean.height=64;
  skinTexture = new THREE.CanvasTexture(skinCanvasClean);
  skinTexture.magFilter = THREE.NearestFilter;
  skinTexture.minFilter = THREE.NearestFilter;

  charGroup = new THREE.Group();
  buildStatic('head', [8,8,8], new THREE.Vector3(0,10*S,0), [9,9,9]);
  buildStatic('body', [8,12,4], new THREE.Vector3(0,0,0), [9,13,5]);
  buildLimb('armR', [4,12,4], new THREE.Vector3(-6*S,6*S,0), [5,13,5]);
  buildLimb('armL', [4,12,4], new THREE.Vector3(6*S,6*S,0), [5,13,5]);
  buildLimb('legR', [4,12,4], new THREE.Vector3(-2*S,-6*S,0), [5,13,5]);
  buildLimb('legL', [4,12,4], new THREE.Vector3(2*S,-6*S,0), [5,13,5]);
  charGroup.position.y = -0.05;
  scene3.add(charGroup);

  raycaster3 = new THREE.Raycaster();
  updateCameraPos();
  animate3();

  const dom = renderer3.domElement;
  dom.style.cursor='grab';
  dom.addEventListener('pointerdown', e=>{
    const hit = pickPixel(e);
    if(hit){ painting3D=true; applyPaint3D(hit); }
    else { dragging3D=true; dom.style.cursor='grabbing'; }
    lastPX=e.clientX; lastPY=e.clientY;
  });
  window.addEventListener('pointermove', e=>{
    if(painting3D){ const hit=pickPixel(e); if(hit) applyPaint3D(hit); return; }
    if(!dragging3D) return;
    rotY += (e.clientX-lastPX)*0.01;
    rotX = Math.max(-1.2, Math.min(1.2, rotX + (e.clientY-lastPY)*0.01));
    lastPX=e.clientX; lastPY=e.clientY;
    updateCameraPos();
  });
  window.addEventListener('pointerup', ()=>{
    if(painting3D){ painting3D=false; pushHistory(); }
    dragging3D=false; dom.style.cursor='grab';
  });
  dom.addEventListener('wheel', e=>{
    e.preventDefault();
    zoomDist = Math.max(1.6, Math.min(6, zoomDist + e.deltaY*0.003));
    updateCameraPos();
  }, {passive:false});
}
function syncThreeWireframe(){ wireframeMeshes.forEach(e=>e.visible=gridOn); }
function syncThreeOverlayPart(part){
  const on = overlayParts[part] && partVisible[part];
  if(overlayMeshByPart[part]) overlayMeshByPart[part].visible = on;
  if(overlayEdgesByPart[part]) overlayEdgesByPart[part].visible = on;
}
function setPartVisible(part, visible){
  partVisible[part] = visible;
  if(limbPivots[part]){
    limbPivots[part].visible = visible;
  } else {
    if(baseMeshByPart[part]) baseMeshByPart[part].visible = visible;
    if(edgesByPart[part]) edgesByPart[part].visible = visible && gridOn;
  }
  syncThreeOverlayPart(part);
}
function getPaintTargets(){
  const targets = [];
  Object.entries(baseMeshByPart).forEach(([part,mesh])=>{ if(partVisible[part]) targets.push(mesh); });
  Object.entries(overlayMeshByPart).forEach(([part,mesh])=>{ if(partVisible[part] && overlayParts[part]) targets.push(mesh); });
  return targets;
}
function updateCameraPos(){
  camera3.position.set(
    zoomDist*Math.sin(rotY)*Math.cos(rotX),
    zoomDist*Math.sin(rotX)+0.3,
    zoomDist*Math.cos(rotY)*Math.cos(rotX)
  );
  camera3.lookAt(0,0,0);
}
function animate3(){
  if(!threeInited) return;
  requestAnimationFrame(animate3);
  renderer3.render(scene3, camera3);
}
function pickPixel(e){
  const dom = renderer3.domElement;
  const rect = dom.getBoundingClientRect();
  const mouse = new THREE.Vector2(
    ((e.clientX-rect.left)/rect.width)*2-1,
    -((e.clientY-rect.top)/rect.height)*2+1
  );
  raycaster3.setFromCamera(mouse, camera3);
  const hits = raycaster3.intersectObjects(getPaintTargets());
  if(!hits.length || !hits[0].uv) return null;
  const uv = hits[0].uv;
  return { x: Math.floor(uv.x*64), y: Math.floor((1-uv.y)*64) };
}
function applyPaint3D(hit){
  const color = document.getElementById('colorPicker').value;
  if(currentTool==='eyedrop'){ const c=getCellColor(hit.x,hit.y); if(c) setColor(c); setTool('pencil'); }
  else if(currentTool==='fill'){ floodFill(hit.x,hit.y,getCellColor(hit.x,hit.y),color); }
  else if(currentTool==='eraser'){ paintCell(hit.x,hit.y,null); }
  else { paintCell(hit.x,hit.y,color); }
  updateSkinTexture();
}
function updateSkinTexture(){
  if(!threeInited) return;
  if(gridW!==64 || gridH!==64) return;
  const ctx = skinCanvasClean.getContext('2d');
  ctx.imageSmoothingEnabled=false;
  ctx.clearRect(0,0,64,64);
  ctx.drawImage(editCanvas,0,0,64,64);
  skinTexture.needsUpdate = true;
}
function setPose(pose){
  document.getElementById('poseStanding').classList.toggle('active', pose==='standing');
  document.getElementById('poseTpose').classList.toggle('active', pose==='tpose');
  if(!threeInited) return;
  if(pose==='tpose'){
    limbPivots.armR.rotation.z = -Math.PI/2;
    limbPivots.armL.rotation.z = Math.PI/2;
    limbPivots.legR.rotation.set(0,0,0);
    limbPivots.legL.rotation.set(0,0,0);
  } else {
    limbPivots.armR.rotation.set(0,0,0);
    limbPivots.armL.rotation.set(0,0,0);
    limbPivots.legR.rotation.set(0,0,0);
    limbPivots.legL.rotation.set(0,0,0);
  }
}

/* ---------- AVATAR MAKER ---------- */
const avatarPreview = document.getElementById('avatarPreview');
const avatarCtx = avatarPreview.getContext('2d');
let avatarImg = null;
document.getElementById('avatarUpload').addEventListener('change', e=>{
  const file = e.target.files[0]; if(!file) return;
  const img = new Image();
  img.onload = ()=>{ avatarImg = img; renderAvatar(); };
  img.src = URL.createObjectURL(file);
});
document.getElementById('pixelSize').addEventListener('input', e=>{
  document.getElementById('pixelSizeLabel').textContent = e.target.value+'px';
  renderAvatar();
});
function renderAvatar(){
  if(!avatarImg) return;
  const blockiness = parseInt(document.getElementById('pixelSize').value);
  const small = Math.max(4, Math.floor(256/blockiness));
  const off = document.createElement('canvas');
  off.width=small; off.height=small;
  const offCtx = off.getContext('2d');
  const s = Math.min(avatarImg.width, avatarImg.height);
  const sx = (avatarImg.width-s)/2, sy=(avatarImg.height-s)/2;
  offCtx.drawImage(avatarImg, sx, sy, s, s, 0, 0, small, small);
  avatarCtx.imageSmoothingEnabled=false;
  avatarCtx.clearRect(0,0,256,256);
  avatarCtx.drawImage(off,0,0,small,small,0,0,256,256);
}
function downloadAvatar(){
  if(!avatarImg){ alert('Upload a photo first.'); return; }
  const a = document.createElement('a');
  a.download='avatar.png';
  a.href=avatarPreview.toDataURL('image/png');
  a.click();
}

/* ---------- COLOR PICKER ---------- */
const imgInput = document.getElementById('imgInput');
const picCanvas = document.getElementById('picCanvas');
const picCtx = picCanvas.getContext('2d', { willReadFrequently:true });
const workbench = document.getElementById('workbench');
let picImageData = null, picZoomLevel = 1, picFitZoom = 1;

imgInput.addEventListener('change', e=>{
  const file = e.target.files[0]; if(!file) return;
  const img = new Image();
  img.onload = ()=>{
    picCanvas.width = img.width;
    picCanvas.height = img.height;
    picCtx.imageSmoothingEnabled = false;
    picCtx.drawImage(img,0,0);
    picImageData = picCtx.getImageData(0,0,picCanvas.width,picCanvas.height);
    const scrollBox = document.getElementById('picCanvasScroll');
    const containerW = Math.max(280, scrollBox.clientWidth - 20);
    // fit big images down, and scale small images up so nothing looks tiny or huge
    picFitZoom = img.width > containerW ? containerW / img.width : Math.min(3, Math.max(1, 380 / img.width));
    picZoomLevel = picFitZoom;
    applyPicZoom();
    workbench.classList.add('show');
  };
  img.src = URL.createObjectURL(file);
});
function applyPicZoom(){
  picCanvas.style.width = (picCanvas.width*picZoomLevel)+'px';
  picCanvas.style.height = (picCanvas.height*picZoomLevel)+'px';
}
function picZoom(factor){
  if(!picImageData) return;
  picZoomLevel = Math.max(0.1, Math.min(8, picZoomLevel*factor));
  applyPicZoom();
}
function picZoomReset(){
  if(!picImageData) return;
  picZoomLevel = picFitZoom;
  applyPicZoom();
}
function picPixelAt(clientX, clientY){
  const rect = picCanvas.getBoundingClientRect();
  const x = Math.floor((clientX-rect.left)*(picCanvas.width/rect.width));
  const y = Math.floor((clientY-rect.top)*(picCanvas.height/rect.height));
  return {x,y};
}
function picColorAt(x,y){
  if(x<0||y<0||x>=picCanvas.width||y>=picCanvas.height) return null;
  const i=(y*picCanvas.width+x)*4;
  const d=picImageData.data;
  return [d[i],d[i+1],d[i+2]];
}
function setPickedColor(r,g,b){
  const hex = '#'+[r,g,b].map(v=>v.toString(16).padStart(2,'0')).join('').toUpperCase();
  document.getElementById('swatch').style.background=hex;
  document.getElementById('hexOut').value=hex;
  document.getElementById('rgbOut').value=`rgb(${r}, ${g}, ${b})`;
  return hex;
}
picCanvas.addEventListener('click', e=>{
  const {x,y} = picPixelAt(e.clientX,e.clientY);
  const c = picColorAt(x,y);
  if(c) setPickedColor(c[0],c[1],c[2]);
});

/* magnifier loupe */
const magnifier = document.getElementById('magnifier');
const magCanvas = document.getElementById('magnifierCanvas');
const magCtx = magCanvas.getContext('2d');
const magHexLabel = document.getElementById('magnifierHex');
const MAG_CELLS = 9;
const MAG_CELL_SIZE = 180/MAG_CELLS;

picCanvas.addEventListener('mousemove', e=>{
  if(!picImageData) return;
  const {x,y} = picPixelAt(e.clientX,e.clientY);
  magCtx.clearRect(0,0,180,180);
  const half = Math.floor(MAG_CELLS/2);
  for(let row=-half; row<=half; row++){
    for(let col=-half; col<=half; col++){
      const sx=x+col, sy=y+row;
      const c = picColorAt(sx,sy);
      const cx=(col+half)*MAG_CELL_SIZE, cy=(row+half)*MAG_CELL_SIZE;
      magCtx.fillStyle = c ? `rgb(${c[0]},${c[1]},${c[2]})` : '#eeeeee';
      magCtx.fillRect(cx,cy,MAG_CELL_SIZE,MAG_CELL_SIZE);
      magCtx.strokeStyle='rgba(0,0,0,0.12)';
      magCtx.strokeRect(cx,cy,MAG_CELL_SIZE,MAG_CELL_SIZE);
    }
  }
  // highlight the exact center pixel that would be picked
  magCtx.strokeStyle='#e05d5d';
  magCtx.lineWidth=2;
  magCtx.strokeRect(half*MAG_CELL_SIZE+1, half*MAG_CELL_SIZE+1, MAG_CELL_SIZE-2, MAG_CELL_SIZE-2);

  const centerColor = picColorAt(x,y);
  magHexLabel.textContent = centerColor ? setPickedColorPreviewOnly(centerColor) : '#------';

  magnifier.style.display='block';
  magnifier.style.left = (e.clientX+18)+'px';
  magnifier.style.top = (e.clientY+18)+'px';
});
picCanvas.addEventListener('mouseleave', ()=>{ magnifier.style.display='none'; });
function setPickedColorPreviewOnly(c){
  return '#'+[c[0],c[1],c[2]].map(v=>v.toString(16).padStart(2,'0')).join('').toUpperCase();
}
function copyHex(){ navigator.clipboard.writeText(document.getElementById('hexOut').value); }

/* ---------- FORMAT CODE MAKER ---------- */
const fcColors = [
  ['&0','#000000','Black'],['&1','#0000AA','Dark Blue'],['&2','#00AA00','Dark Green'],
  ['&3','#00AAAA','Dark Aqua'],['&4','#AA0000','Dark Red'],['&5','#AA00AA','Purple'],
  ['&6','#FFAA00','Gold'],['&7','#AAAAAA','Gray'],['&8','#555555','Dark Gray'],
  ['&9','#5555FF','Blue'],['&a','#55FF55','Green'],['&b','#55FFFF','Aqua'],
  ['&c','#FF5555','Red'],['&d','#FF55FF','Pink'],['&e','#FFFF55','Yellow'],['&f','#FFFFFF','White']
];
const fcPaletteEl = document.getElementById('fcPalette');
fcColors.forEach(([code,hex,name])=>{
  const b = document.createElement('button');
  b.className='toolbtn'; b.style.background=hex;
  b.style.color = (hex==='#000000'||hex==='#0000AA'||hex==='#00AA00'||hex==='#AA0000'||hex==='#555555') ? '#fff' : '#111';
  b.title=name; b.textContent=name;
  b.onclick=()=>fcInsert(code);
  fcPaletteEl.appendChild(b);
});
const fcInput = document.getElementById('fcInput');
function fcInsert(code){
  const start = fcInput.selectionStart ?? fcInput.value.length;
  const end = fcInput.selectionEnd ?? fcInput.value.length;
  fcInput.value = fcInput.value.slice(0,start) + code + fcInput.value.slice(end);
  fcInput.focus();
  fcInput.selectionStart = fcInput.selectionEnd = start + code.length;
  fcRender();
}
function fcRender(){
  const raw = fcInput.value;
  document.getElementById('fcOutput').value = raw;
  const preview = document.getElementById('fcPreview');
  preview.innerHTML='';
  let span = document.createElement('span');
  let styleColor='#eee', bold=false, italic=false, underline=false;
  function applyStyle(){
    span.style.color=styleColor;
    span.style.fontWeight = bold?'700':'400';
    span.style.fontStyle = italic?'italic':'normal';
    span.style.textDecoration = underline?'underline':'none';
  }
  let i=0;
  while(i<raw.length){
    if(raw[i]==='&' && i+1<raw.length){
      const code = '&'+raw[i+1].toLowerCase();
      const colorMatch = fcColors.find(c=>c[0]===code);
      if(colorMatch){ preview.appendChild(span); span=document.createElement('span'); styleColor=colorMatch[1]; applyStyle(); i+=2; continue; }
      if(code==='&l'){ preview.appendChild(span); span=document.createElement('span'); bold=true; applyStyle(); i+=2; continue; }
      if(code==='&o'){ preview.appendChild(span); span=document.createElement('span'); italic=true; applyStyle(); i+=2; continue; }
      if(code==='&n'){ preview.appendChild(span); span=document.createElement('span'); underline=true; applyStyle(); i+=2; continue; }
      if(code==='&r'){ preview.appendChild(span); span=document.createElement('span'); styleColor='#eee'; bold=italic=underline=false; applyStyle(); i+=2; continue; }
    }
    span.textContent += raw[i]; i++;
  }
  preview.appendChild(span);
}
fcInput.addEventListener('input', fcRender);
fcRender();

/* ---------- PALETTE GENERATOR ---------- */
document.getElementById('paletteUpload').addEventListener('change', e=>{
  const file = e.target.files[0]; if(!file) return;
  const img = new Image();
  img.onload = ()=>{
    const off = document.createElement('canvas');
    const s = 80;
    off.width=s; off.height=s;
    const offCtx = off.getContext('2d');
    offCtx.drawImage(img,0,0,s,s);
    const data = offCtx.getImageData(0,0,s,s).data;
    const counts = {};
    for(let i=0;i<data.length;i+=4){
      const r = Math.round(data[i]/32)*32, g=Math.round(data[i+1]/32)*32, b=Math.round(data[i+2]/32)*32;
      const key = r+','+g+','+b;
      counts[key] = (counts[key]||0)+1;
    }
    const sorted = Object.entries(counts).sort((a,b)=>b[1]-a[1]).slice(0,5);
    const resultEl = document.getElementById('paletteResult');
    resultEl.innerHTML='';
    sorted.forEach(([rgbKey])=>{
      const [r,g,b] = rgbKey.split(',').map(Number);
      const hex = '#'+[r,g,b].map(v=>Math.min(255,v).toString(16).padStart(2,'0')).join('').toUpperCase();
      const div = document.createElement('div');
      div.className='palette-swatch block';
      div.innerHTML = `<div class="sw" style="background:${hex}"></div><div class="hx">${hex}</div>`;
      div.onclick = ()=>navigator.clipboard.writeText(hex);
      div.title = 'Click to copy';
      resultEl.appendChild(div);
    });
  };
  img.src = URL.createObjectURL(file);
});

/* ---------- ICON MAKER ---------- */
const iconTitles = {
  painting:['🖼 Custom Paintings','Upload an image, pick a frame color, and download it as a framed painting texture.'],
  disc:['💿 Music Disc Maker','Upload album art or any image and turn it into a labeled disc icon.'],
  totem:['🗿 Totem Generator','Upload an image to build a small glowing talisman-style icon.'],
  head:['🧊 Player Heads','Upload a photo and crop it into a mini blocky head icon.'],
  emoji:['😀 Custom Emojis','Upload any image and round it into an emoji-sized icon.']
};
let iconStyle='painting', iconImg=null;
const iconCanvas = document.getElementById('iconCanvas');
const iconCtx = iconCanvas.getContext('2d');
function openIcon(style){
  go('iconmaker');
  iconStyle = style;
  document.getElementById('iconTitle').textContent = iconTitles[style][0];
  document.getElementById('iconSub').textContent = iconTitles[style][1];
  renderIcon();
}
document.getElementById('iconUpload').addEventListener('change', e=>{
  const file = e.target.files[0]; if(!file) return;
  const img = new Image();
  img.onload = ()=>{ iconImg = img; renderIcon(); };
  img.src = URL.createObjectURL(file);
});
document.getElementById('iconColor').addEventListener('input', renderIcon);
function renderIcon(){
  const W=300,H=300;
  iconCtx.clearRect(0,0,W,H);
  const color = document.getElementById('iconColor').value;
  const drawCover = (x,y,w,h)=>{
    if(!iconImg) return;
    const s = Math.min(iconImg.width, iconImg.height);
    const sx=(iconImg.width-s)/2, sy=(iconImg.height-s)/2;
    iconCtx.drawImage(iconImg, sx, sy, s, s, x, y, w, h);
  };
  if(iconStyle==='painting'){
    iconCtx.fillStyle=color; iconCtx.fillRect(30,30,240,240);
    drawCover(46,46,208,208);
    iconCtx.strokeStyle='rgba(0,0,0,0.3)'; iconCtx.lineWidth=4; iconCtx.strokeRect(46,46,208,208);
  } else if(iconStyle==='disc'){
    iconCtx.fillStyle='#111'; iconCtx.beginPath(); iconCtx.arc(150,150,130,0,7); iconCtx.fill();
    iconCtx.save(); iconCtx.beginPath(); iconCtx.arc(150,150,90,0,7); iconCtx.clip();
    drawCover(60,60,180,180); iconCtx.restore();
    iconCtx.fillStyle=color; iconCtx.beginPath(); iconCtx.arc(150,150,22,0,7); iconCtx.fill();
    iconCtx.fillStyle='#111'; iconCtx.beginPath(); iconCtx.arc(150,150,7,0,7); iconCtx.fill();
  } else if(iconStyle==='totem'){
    iconCtx.save();
    iconCtx.shadowColor=color; iconCtx.shadowBlur=30;
    iconCtx.fillStyle=color; iconCtx.beginPath();
    iconCtx.moveTo(150,30); iconCtx.lineTo(230,110); iconCtx.lineTo(200,270); iconCtx.lineTo(100,270); iconCtx.lineTo(70,110);
    iconCtx.closePath(); iconCtx.fill(); iconCtx.restore();
    iconCtx.save(); iconCtx.beginPath();
    iconCtx.moveTo(150,55); iconCtx.lineTo(205,115); iconCtx.lineTo(182,250); iconCtx.lineTo(118,250); iconCtx.lineTo(95,115);
    iconCtx.closePath(); iconCtx.clip();
    drawCover(60,60,180,180); iconCtx.restore();
  } else if(iconStyle==='head'){
    iconCtx.fillStyle=color; iconCtx.fillRect(60,60,180,180);
    drawCover(78,78,144,144);
    iconCtx.strokeStyle='rgba(0,0,0,0.35)'; iconCtx.lineWidth=6; iconCtx.strokeRect(60,60,180,180);
  } else if(iconStyle==='emoji'){
    iconCtx.save(); iconCtx.beginPath(); iconCtx.arc(150,150,120,0,7); iconCtx.clip();
    drawCover(30,30,240,240); iconCtx.restore();
    iconCtx.strokeStyle=color; iconCtx.lineWidth=8; iconCtx.beginPath(); iconCtx.arc(150,150,120,0,7); iconCtx.stroke();
  }
}
function downloadIcon(){
  if(!iconImg){ alert('Upload an image first.'); return; }
  const a = document.createElement('a');
  a.download = iconStyle+'.png';
  a.href = iconCanvas.toDataURL('image/png');
  a.click();
}

/* ---------- SKIN PACK MAKER ---------- */
let spSkinBlob=null, spCapeBlob=null;
document.getElementById('spSkinUpload').addEventListener('change', e=>{
  const f = e.target.files[0]; if(!f) return;
  spSkinBlob=f; document.getElementById('spSkinName').textContent = '✓ '+f.name;
});
document.getElementById('spCapeUpload').addEventListener('change', e=>{
  const f = e.target.files[0]; if(!f) return;
  spCapeBlob=f; document.getElementById('spCapeName').textContent = '✓ '+f.name;
});
async function buildSkinPack(){
  if(!spSkinBlob){ alert('Upload a skin PNG first.'); return; }
  const zip = new JSZip();
  const packName = document.getElementById('spPackName').value || 'MyPack';
  zip.file('skin.png', spSkinBlob);
  if(spCapeBlob) zip.file('cape.png', spCapeBlob);
  zip.file('pack.json', JSON.stringify({name:packName, created:new Date().toISOString()}, null, 2));
  const blob = await zip.generateAsync({type:'blob'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = packName+'.zip';
  a.click();
}

/* ---------- TEXTURE PACK MERGER ---------- */
let mergerFiles = [];
document.getElementById('mergerUpload').addEventListener('change', e=>{
  mergerFiles = Array.from(e.target.files);
  const list = document.getElementById('mergerList');
  list.innerHTML='';
  mergerFiles.forEach(f=>{
    const div = document.createElement('div');
    div.className='card block';
    div.style.cursor='default';
    div.innerHTML = `<div class="icon-badge">🖼</div><h3 style="font-size:13px;">${f.name}</h3><p>${(f.size/1024).toFixed(1)} KB</p>`;
    list.appendChild(div);
  });
});
async function buildMergedZip(){
  if(!mergerFiles.length){ alert('Choose at least one image first.'); return; }
  const zip = new JSZip();
  mergerFiles.forEach(f=> zip.file(f.name, f));
  const blob = await zip.generateAsync({type:'blob'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = (document.getElementById('mergerZipName').value || 'texture-pack')+'.zip';
  a.click();
}

/* ---------- HUD CUSTOMIZER ---------- */
function updateHud(){
  document.getElementById('hudHealthBar').style.background = document.getElementById('hudHealth').value;
  document.getElementById('hudHungerBar').style.background = document.getElementById('hudHunger').value;
  document.getElementById('hudXpBar').style.background = document.getElementById('hudXp').value;
}
['hudHealth','hudHunger','hudXp'].forEach(id=>document.getElementById(id).addEventListener('input', updateHud));
function downloadHudPreset(){
  const preset = {
    health: document.getElementById('hudHealth').value,
    hunger: document.getElementById('hudHunger').value,
    xp: document.getElementById('hudXp').value
  };
  const blob = new Blob([JSON.stringify(preset,null,2)], {type:'application/json'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'hud-preset.json';
  a.click();
}

/* ---------- COORDINATE CALCULATOR ---------- */
const coordIds = ['ax','ay','az'];
const ratioSlider = document.getElementById('ratioSlider');
function updateCoords(){
  const ratio = parseInt(ratioSlider.value);
  document.getElementById('ratioLabel').textContent = ratio;
  document.getElementById('ratioLabel2').textContent = ratio;
  ['x','y','z'].forEach(axis=>{
    const v = parseFloat(document.getElementById('a'+axis).value);
    document.getElementById('b'+axis).value = isNaN(v) ? '' : (v/ratio).toFixed(2);
  });
}
coordIds.forEach(id=>document.getElementById(id).addEventListener('input', updateCoords));
ratioSlider.addEventListener('input', updateCoords);

/* ---------- QUICK-ACCESS BUBBLE DOCK ---------- */
const ALL_TOOLS = [
  {id:'skin', label:'Skin Editor', icon:'🧍', action:()=>openEditor('skin')},
  {id:'cape', label:'Cape Editor', icon:'🧥', action:()=>openEditor('cape')},
  {id:'avatar', label:'Avatar Maker', icon:'👤', action:()=>go('avatar')},
  {id:'picker', label:'Color Picker', icon:'🎨', action:()=>go('picker')},
  {id:'formatcodes', label:'Format Code Maker', icon:'🌈', action:()=>go('formatcodes')},
  {id:'palette', label:'Palette Generator', icon:'🧬', action:()=>go('palette')},
  {id:'coords', label:'Coordinate Calculator', icon:'📍', action:()=>go('coords')},
  {id:'skinpack', label:'Skin Pack Maker', icon:'📦', action:()=>go('skinpack')},
  {id:'merger', label:'Texture Pack Merger', icon:'🗂', action:()=>go('merger')},
  {id:'hud', label:'HUD Customizer', icon:'❤️', action:()=>go('hud')},
  {id:'icon_painting', label:'Custom Paintings', icon:'🖼', action:()=>openIcon('painting')},
  {id:'icon_disc', label:'Music Disc Maker', icon:'💿', action:()=>openIcon('disc')},
  {id:'icon_totem', label:'Totem Generator', icon:'🗿', action:()=>openIcon('totem')},
  {id:'icon_head', label:'Player Heads', icon:'🧊', action:()=>openIcon('head')},
  {id:'icon_emoji', label:'Custom Emojis', icon:'😀', action:()=>openIcon('emoji')}
];
let pinnedTools;
try{ pinnedTools = JSON.parse(localStorage.getItem('mce_pinned')) || []; }
catch(e){ pinnedTools = []; }
let dockInitialized = pinnedTools.length > 0;
let awaitingSelection = false;
function savePinned(){ try{ localStorage.setItem('mce_pinned', JSON.stringify(pinnedTools)); }catch(e){} }

/* tiny synthesized sound effects — no audio files needed */
let audioCtx;
function playTone(kind){
  try{
    if(!audioCtx) audioCtx = new (window.AudioContext||window.webkitAudioContext)();
    const now = audioCtx.currentTime;
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.connect(gain); gain.connect(audioCtx.destination);
    let f0=500, f1=500, dur=0.15, type='sine', vol=0.08;
    if(kind==='open'){ f0=440; f1=680; dur=0.18; type='sine'; }
    else if(kind==='charge'){ f0=520; f1=900; dur=0.32; type='triangle'; vol=0.1; }
    else if(kind==='pop'){ f0=760; f1=160; dur=0.16; type='square'; vol=0.08; }
    else if(kind==='appear'){ f0=600; f1=1040; dur=0.22; type='sine'; vol=0.09; }
    osc.type=type;
    osc.frequency.setValueAtTime(f0, now);
    osc.frequency.exponentialRampToValueAtTime(Math.max(60,f1), now+dur);
    gain.gain.setValueAtTime(vol, now);
    gain.gain.exponentialRampToValueAtTime(0.001, now+dur);
    osc.start(now);
    osc.stop(now+dur+0.03);
  }catch(e){ /* audio unavailable, ignore */ }
}

function toolPageId(id){
  if(id==='skin'||id==='cape') return 'editor';
  if(id.indexOf('icon_')===0) return 'iconmaker';
  return id;
}
function isToolActive(id){
  const activePage = document.querySelector('.page.active');
  if(!activePage) return false;
  if(id==='skin' || id==='cape') return activePage.id==='editor' && currentEditorKind===id;
  if(id.indexOf('icon_')===0) return activePage.id==='iconmaker' && iconStyle===id.slice(5);
  return activePage.id===id;
}

function makeEmptyBubble(){
  const b = document.createElement('div');
  b.className = 'quick-bubble';
  b.title = 'Pick a tool on the home page to start your dock';
  document.getElementById('quickDock').appendChild(b);
  return b;
}
function makePlusBubble(animate){
  const b = document.createElement('div');
  b.className = 'quick-bubble plusbubble' + (animate ? ' bubble-pop' : '');
  b.id = 'dockPlusBubble';
  b.title = 'Add a tool';
  b.textContent = '+';
  b.addEventListener('click', activatePlusBubble);
  document.getElementById('quickDock').appendChild(b);
}
function activatePlusBubble(){
  const plusBubble = document.getElementById('dockPlusBubble');
  if(!plusBubble) return;
  plusBubble.textContent = '?';
  plusBubble.classList.add('glowing');
  plusBubble.title = 'Pick a tool on the home page';
  awaitingSelection = true;
  playTone('open');
  go('home');
}
function makeToolBubble(id, animate){
  const tool = ALL_TOOLS.find(t=>t.id===id);
  if(!tool) return null;
  const b = document.createElement('div');
  b.className = 'quick-bubble' + (animate ? ' bubble-pop' : '');
  b.title = tool.label;
  b.dataset.tool = id;
  b.innerHTML = `<span${animate?' class="icon-fly"':''}>${tool.icon}</span><div class="bubble-remove">×</div>`;
  b.addEventListener('click', (e)=>{
    if(e.target.classList.contains('bubble-remove')){
      const wasActive = isToolActive(id);
      pinnedTools = pinnedTools.filter(p=>p!==id);
      savePinned();
      b.remove();
      if(wasActive) go('home');
      return;
    }
    tool.action();
  });
  return b;
}
function renderDockInitial(){
  const dock = document.getElementById('quickDock');
  dock.innerHTML='';
  if(!dockInitialized){
    makeEmptyBubble();
    return;
  }
  pinnedTools.forEach(id=>{
    const b = makeToolBubble(id, false);
    if(b) dock.appendChild(b);
  });
  makePlusBubble(false);
}
function homeSelect(id){
  if(awaitingSelection){
    resolveAwaitingSelection(id);
  } else {
    activateDock(id);
  }
  const tool = ALL_TOOLS.find(t=>t.id===id);
  if(tool) tool.action();
}
function activateDock(id){
  if(!dockInitialized){
    dockInitialized = true;
    const dock = document.getElementById('quickDock');
    dock.innerHTML='';
    const toolBubble = makeToolBubble(id, true);
    dock.appendChild(toolBubble);
    pinnedTools = [id];
    savePinned();
    setTimeout(()=>makePlusBubble(true), 300);
  } else if(!pinnedTools.includes(id)){
    pinnedTools.push(id);
    savePinned();
    const dock = document.getElementById('quickDock');
    const plusBubble = document.getElementById('dockPlusBubble');
    const b = makeToolBubble(id, true);
    dock.insertBefore(b, plusBubble);
  }
}
function resolveAwaitingSelection(id){
  awaitingSelection = false;
  const plusBubble = document.getElementById('dockPlusBubble');
  if(!plusBubble) return;
  plusBubble.classList.remove('glowing');
  plusBubble.classList.add('glowing-intense');
  playTone('charge');
  setTimeout(()=>{
    plusBubble.classList.add('bubble-pop-out');
    playTone('pop');
    setTimeout(()=>{
      plusBubble.remove();
      if(!pinnedTools.includes(id)){ pinnedTools.push(id); savePinned(); }
      const dock = document.getElementById('quickDock');
      const toolBubble = makeToolBubble(id, true);
      dock.appendChild(toolBubble);
      playTone('appear');
      setTimeout(()=>makePlusBubble(true), 300);
    }, 380);
  }, 550);
}
renderDockInitial();
</script>
</body>
</html>
