[index (3).html](https://github.com/user-attachments/files/29987652/index.3.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ToolCraft — Free Browser Tools</title>
<style>
  :root{
    --bg:#181a20; --panel:#20232b; --panel2:#262a34; --line:#333844;
    --text:#eef0f4; --muted:#8d94a3;
    --accent-cyan:#4fd3e8; --accent-gold:#e8b64f; --accent-purple:#b98af0; --accent-green:#7ed67e;
    --pixel:2px;
  }
  [data-theme="day"]{
    --bg:#eef2f7; --panel:#ffffff; --panel2:#f4f7fb; --line:#d7dee8;
    --text:#1c222b; --muted:#5b6472;
  }
  *{box-sizing:border-box;}
  body{ margin:0; background:var(--bg); color:var(--text); font-family:'Segoe UI', system-ui, sans-serif; transition:background .2s, color .2s; }
  h1,h2,h3{ font-family:'Courier New', monospace; letter-spacing:.02em; }

  .block{
    background:var(--panel); border:var(--pixel) solid var(--line);
    box-shadow: inset 0 2px 0 rgba(255,255,255,.05), inset 0 -2px 0 rgba(0,0,0,.25);
    border-radius:2px;
  }

  header{
    display:flex; align-items:center; justify-content:space-between; padding:14px 28px;
    border-bottom:var(--pixel) solid var(--line); background:var(--panel);
    position:sticky; top:0; z-index:10; flex-wrap:wrap; gap:8px;
  }
  .logo{ font-size:18px; font-weight:700; cursor:pointer; }
  .logo span{ color:var(--accent-gold); }
  nav.topnav{ display:flex; gap:4px; align-items:center; flex-wrap:wrap; }
  .navgroup{ position:relative; }
  .navbtn{
    background:none; border:none; color:var(--muted); font-size:13.5px; padding:9px 12px;
    cursor:pointer; font-family:inherit; transition:color .15s, background .15s; border-radius:2px;
  }
  .navbtn:hover{ color:var(--text); background:var(--panel2); }
  .dropdown{ display:none; position:absolute; top:100%; left:0; min-width:190px; padding:6px; margin-top:6px; z-index:20; }
  .navgroup:hover .dropdown{ display:block; }
  .dropdown a{ display:block; padding:8px 10px; color:var(--text); text-decoration:none; font-size:13px; border-radius:2px; transition:background .15s, transform .1s; cursor:pointer; }
  .dropdown a:hover{ background:var(--panel2); transform:translateX(2px); }
  .toggle{ background:var(--panel2); border:1px solid var(--line); color:var(--text); padding:7px 12px; border-radius:2px; cursor:pointer; font-size:13px; transition:background .15s; }
  .toggle:hover{ background:var(--line); }

  main{ max-width:1080px; margin:0 auto; padding:38px 24px 60px; }
  .page{ display:none; }
  .page.active{ display:block; animation:fade .2s ease; }
  @keyframes fade{ from{opacity:0; transform:translateY(6px);} to{opacity:1; transform:translateY(0);} }

  .hero h1{ font-size:28px; margin:0 0 8px; }
  .hero p{ color:var(--muted); font-size:14px; max-width:560px; line-height:1.6; margin:0 0 30px; }
  .sectionlabel{ font-size:12px; text-transform:uppercase; letter-spacing:.12em; color:var(--muted); margin:26px 0 12px; }

  .grid{ display:grid; grid-template-columns:repeat(auto-fill, minmax(190px,1fr)); gap:12px; }
  .card{ padding:16px; cursor:pointer; transition:transform .15s, box-shadow .15s; border-left:var(--pixel) solid var(--cat, var(--accent-cyan)); }
  .card:hover{ transform:translateY(-3px) translateX(1px); }
  .card .icon{ font-size:20px; }
  .card h3{ font-size:14px; margin:8px 0 4px; }
  .card p{ font-size:12px; color:var(--muted); margin:0; line-height:1.4; }
  .card .status{ font-size:10px; color:var(--muted); margin-top:8px; font-family:'Courier New',monospace; }

  .tool-header{ margin-bottom:20px; }
  .tool-header h1{ font-size:22px; margin:0 0 6px; }
  .tool-header p{ color:var(--muted); font-size:13.5px; margin:0; }

  .toolbar{ display:flex; gap:8px; flex-wrap:wrap; margin-bottom:14px; align-items:center; }
  .toolbtn{
    background:var(--panel2); border:1px solid var(--line); color:var(--text); padding:8px 12px;
    border-radius:2px; cursor:pointer; font-size:13px; transition:background .15s, transform .1s;
  }
  .toolbtn:hover{ background:var(--line); }
  .toolbtn.active{ outline:2px solid var(--accent-gold); }
  select.toolbtn{ appearance:auto; }

  .editor-layout{ display:grid; grid-template-columns:1fr 220px; gap:18px; }
  #editorWrap{ padding:14px; text-align:center; overflow:auto; }
  #editCanvas{ image-rendering:pixelated; cursor:crosshair; border:1px solid var(--line); }

  .palette{ padding:14px; align-self:start; }
  .palette-grid{ display:grid; grid-template-columns:repeat(6,1fr); gap:5px; margin-bottom:12px; }
  .swatch-btn{ width:100%; aspect-ratio:1; border:1px solid var(--line); border-radius:2px; cursor:pointer; transition:transform .1s; }
  .swatch-btn:hover{ transform:scale(1.1); }
  .colorinput{ width:100%; height:34px; border:1px solid var(--line); border-radius:2px; background:none; cursor:pointer; margin-bottom:10px; }
  .actions{ display:flex; flex-direction:column; gap:6px; }

  .dropzone{ padding:30px; text-align:center; cursor:pointer; color:var(--muted); font-size:13.5px; border-style:dashed !important; transition:border-color .15s, color .15s; margin-bottom:16px; }
  .dropzone:hover{ border-color:var(--accent-cyan) !important; color:var(--text); }
  input[type=file]{ display:none; }

  .avatar-layout{ display:grid; grid-template-columns:1fr 1fr; gap:20px; }
  .avatar-panel{ padding:16px; text-align:center; }
  #avatarPreview{ image-rendering:pixelated; max-width:100%; border:1px solid var(--line); background:repeating-conic-gradient(#3a3f4a 0% 25%, #2b2f38 0% 50%) 50%/16px 16px; }
  .slider-row{ display:flex; align-items:center; gap:10px; margin:14px 0; font-size:13px; color:var(--muted); }
  .slider-row input[type=range]{ flex:1; }

  .workbench{ display:none; margin-top:20px; gap:20px; grid-template-columns:1fr 220px; }
  .workbench.show{ display:grid; }
  #canvasWrap{ padding:10px; overflow:auto; text-align:center; }
  #picCanvas{ max-width:100%; cursor:crosshair; image-rendering:pixelated; }
  .result{ padding:16px; align-self:start; }
  .swatch{ width:100%; height:64px; border:var(--pixel) solid var(--line); margin-bottom:12px; border-radius:2px; }
  .hexrow{ display:flex; gap:6px; margin-bottom:8px; }
  .hexrow input{ flex:1; background:var(--panel2); border:1px solid var(--line); color:var(--text); padding:8px 10px; border-radius:2px; font-family:'Courier New',monospace; font-size:13px; }
  .copybtn{ background:var(--accent-gold); border:none; color:#22190a; font-weight:700; padding:8px 12px; border-radius:2px; cursor:pointer; font-size:12px; transition:background .15s, transform .1s; }
  .copybtn:hover{ filter:brightness(1.08); }
  .copybtn:active{ transform:scale(.96); }
  .zoomhint{ font-size:11px; color:var(--muted); margin-top:14px; line-height:1.5; }

  .related{ margin-top:44px; padding-top:16px; border-top:1px solid var(--line); }
  .related h3{ font-size:12px; text-transform:uppercase; letter-spacing:.1em; color:var(--muted); margin-bottom:10px; }
  .related-links{ display:flex; gap:10px; flex-wrap:wrap; }
  .related-links a{ font-size:12.5px; color:var(--text); text-decoration:none; padding:7px 12px; background:var(--panel2); border:1px solid var(--line); border-radius:2px; transition:background .15s; cursor:pointer; }
  .related-links a:hover{ background:var(--line); }

  footer{ text-align:center; color:var(--muted); font-size:11.5px; padding:26px; border-top:1px solid var(--line); }

  @media (max-width:720px){
    .editor-layout, .avatar-layout, .workbench.show{ grid-template-columns:1fr; }
  }
</style>
</head>
<body data-theme="night">

<header>
  <div class="logo" onclick="go('home')">◼ Tool<span>Craft</span></div>
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
      <button class="navbtn">Color ▾</button>
      <div class="dropdown block">
        <a onclick="go('picker')">🎨 Color Picker</a>
        <a onclick="go('formatcodes')">🌈 Format Code Maker</a>
        <a onclick="alert('Coming soon.')">🌈 Palette Generator</a>
      </div>
    </div>
    <div class="navgroup">
      <button class="navbtn">Tools ▾</button>
      <div class="dropdown block">
        <a onclick="go('coords')">📍 Coordinate Calculator</a>
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
      <h1>ToolCraft</h1>
      <p>Free, browser-based creator tools. Everything runs locally on your device — nothing is ever uploaded to a server.</p>
    </div>

    <div class="sectionlabel">Character</div>
    <div class="grid">
      <div class="card block" style="--cat:var(--accent-purple)" onclick="openEditor('skin')">
        <div class="icon">🧍</div><h3>Skin Editor</h3>
        <p>Pixel-paint a blocky character skin, pencil/fill/eyedropper, download PNG.</p>
        <div class="status">● live</div>
      </div>
      <div class="card block" style="--cat:var(--accent-purple)" onclick="openEditor('cape')">
        <div class="icon">🧥</div><h3>Cape Editor</h3>
        <p>Same pixel tools, sized for a cape/cloak texture.</p>
        <div class="status">● live</div>
      </div>
      <div class="card block" style="--cat:var(--accent-purple)" onclick="go('avatar')">
        <div class="icon">👤</div><h3>Avatar Maker</h3>
        <p>Upload any photo, turn it into a blocky pixel avatar.</p>
        <div class="status">● live</div>
      </div>
    </div>

    <div class="sectionlabel">Color</div>
    <div class="grid">
      <div class="card block" style="--cat:var(--accent-cyan)" onclick="go('picker')">
        <div class="icon">🎨</div><h3>Color Picker</h3>
        <p>Upload an image, click a pixel, get its hex code instantly.</p>
        <div class="status">● live</div>
      </div>
      <div class="card block" style="--cat:var(--accent-cyan)" onclick="go('formatcodes')">
        <div class="icon">🌈</div><h3>Format Code Maker</h3>
        <p>Write styled text with color/bold/italic codes, live preview, copy-ready output.</p>
        <div class="status">● live</div>
      </div>
      <div class="card block" style="--cat:var(--accent-cyan)" onclick="alert('Coming soon.')">
        <div class="icon">🌈</div><h3>Palette Generator</h3>
        <p>Pull a 5-color palette out of any photo.</p>
        <div class="status">○ coming soon</div>
      </div>
    </div>

    <div class="sectionlabel">Tools</div>
    <div class="grid">
      <div class="card block" style="--cat:var(--accent-green)" onclick="go('coords')">
        <div class="icon">📍</div><h3>Coordinate Calculator</h3>
        <p>Convert coordinates between two linked grids at any ratio.</p>
        <div class="status">● live</div>
      </div>
    </div>
  </section>

  <!-- PIXEL EDITOR (skin + cape share this) -->
  <section class="page" id="editor">
    <div class="tool-header">
      <h1 id="editorTitle">🧍 Skin Editor</h1>
      <p>Pencil, eraser, fill, and eyedropper. Draw, then download your texture as a PNG — nothing leaves your device.</p>
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
      <button class="toolbtn active" id="guideToggle" onclick="toggleGuide()">👁 Guide</button>
      <select class="toolbtn" id="sizeSelect" onchange="changeSize()">
        <option value="32x32">32×32</option>
        <option value="64x64">64×64</option>
        <option value="64x32">64×32 (cape)</option>
      </select>
    </div>

    <div class="editor-layout">
      <div id="editorWrap" class="block">
        <div id="canvasStack" style="position:relative; display:inline-block;">
          <canvas id="guideCanvas" style="position:absolute; top:0; left:0; pointer-events:none;"></canvas>
          <canvas id="editCanvas" style="position:relative;"></canvas>
        </div>
      </div>
      <div class="palette block">
        <input type="color" class="colorinput" id="colorPicker" value="#e8b64f">
        <div class="palette-grid" id="paletteGrid"></div>
        <div class="actions">
          <button class="toolbtn" onclick="downloadCanvas()">⬇ Download PNG</button>
        </div>
      </div>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links">
        <a onclick="go('avatar')">Avatar Maker</a>
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

    <textarea id="fcInput" class="block" style="width:100%; min-height:70px; background:var(--panel2); color:var(--text); border:1px solid var(--line); padding:12px; font-family:'Courier New',monospace; font-size:14px; border-radius:2px;" placeholder="Type your text here...">&6Hello &l&bWorld&r!</textarea>

    <div class="sectionlabel">Live Preview</div>
    <div id="fcPreview" class="block" style="padding:16px; font-family:'Courier New',monospace; font-size:16px; min-height:30px; background:#12141a; color:#eee;"></div>

    <div class="sectionlabel">Copy-ready output</div>
    <div class="hexrow">
      <input type="text" id="fcOutput" class="block" style="flex:1; padding:10px 12px; font-family:'Courier New',monospace; color:var(--text); background:var(--panel2); border:1px solid var(--line);" readonly>
      <button class="copybtn" onclick="navigator.clipboard.writeText(document.getElementById('fcOutput').value)">Copy</button>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links"><a onclick="go('picker')">Color Picker</a></div>
    </div>
  </section>

  <!-- COORDINATE CALCULATOR -->
  <section class="page" id="coords">
    <div class="tool-header">
      <h1>📍 Coordinate Calculator</h1>
      <p>Convert a coordinate between two linked grids at any scale ratio — handy any time one space maps onto another at a fixed scale.</p>
    </div>

    <div class="avatar-layout">
      <div class="avatar-panel block" style="text-align:left">
        <h3 style="margin-top:0">Grid A</h3>
        <div class="hexrow"><input type="text" class="block" id="ax" placeholder="X" style="padding:8px; color:var(--text); background:var(--panel2); border:1px solid var(--line);"></div>
        <div class="hexrow"><input type="text" class="block" id="ay" placeholder="Y" style="padding:8px; color:var(--text); background:var(--panel2); border:1px solid var(--line);"></div>
        <div class="hexrow"><input type="text" class="block" id="az" placeholder="Z" style="padding:8px; color:var(--text); background:var(--panel2); border:1px solid var(--line);"></div>
      </div>
      <div class="avatar-panel block" style="text-align:left">
        <h3 style="margin-top:0">Grid B <span style="font-size:11px; color:var(--muted); font-family:'Segoe UI'">(scale ÷<span id="ratioLabel">8</span>)</span></h3>
        <div class="hexrow"><input type="text" class="block" id="bx" readonly style="padding:8px; color:var(--text); background:var(--panel2); border:1px solid var(--line);"></div>
        <div class="hexrow"><input type="text" class="block" id="by" readonly style="padding:8px; color:var(--text); background:var(--panel2); border:1px solid var(--line);"></div>
        <div class="hexrow"><input type="text" class="block" id="bz" readonly style="padding:8px; color:var(--text); background:var(--panel2); border:1px solid var(--line);"></div>
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
      <p>Upload an image below, then click anywhere on it to grab that pixel's hex code. Nothing leaves your device.</p>
    </div>

    <div class="dropzone block" onclick="document.getElementById('imgInput').click()">
      Click to choose an image (PNG, JPG, etc.)
    </div>
    <input type="file" id="imgInput" accept="image/*">

    <div class="workbench" id="workbench">
      <div id="canvasWrap" class="block">
        <canvas id="picCanvas"></canvas>
      </div>
      <div class="result block">
        <div class="swatch" id="swatch" style="background:#333"></div>
        <div class="hexrow">
          <input type="text" id="hexOut" value="#------" readonly>
          <button class="copybtn" onclick="copyHex()">Copy</button>
        </div>
        <div class="hexrow"><input type="text" id="rgbOut" value="rgb(-,-,-)" readonly></div>
        <div class="zoomhint">Click anywhere on the image to sample that pixel.</div>
      </div>
    </div>

    <div class="related">
      <h3>Related Tools</h3>
      <div class="related-links">
        <a onclick="openEditor('skin')">Skin Editor</a>
        <a onclick="go('avatar')">Avatar Maker</a>
      </div>
    </div>
  </section>

  <!-- ABOUT -->
  <section class="page" id="about">
    <div class="tool-header">
      <h1>About ToolCraft</h1>
      <p>A small collection of browser-only creator tools. No accounts, no server uploads — everything runs on your device using standard browser APIs (Canvas, FileReader, Blob).</p>
    </div>
  </section>

</main>

<footer>ToolCraft · built for GitHub Pages · runs entirely in your browser</footer>

<script>
/* ---------- NAV ---------- */
function go(page){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(page).classList.add('active');
}
function toggleTheme(){
  const body = document.body;
  const btn = document.getElementById('themeBtn');
  if(body.dataset.theme === 'night'){ body.dataset.theme = 'day'; btn.textContent = '☀ Day'; }
  else { body.dataset.theme = 'night'; btn.textContent = '🌙 Night'; }
}

/* ---------- PIXEL EDITOR (skin + cape) ---------- */
const editCanvas = document.getElementById('editCanvas');
const editCtx = editCanvas.getContext('2d', { willReadFrequently:true });
const guideCanvas = document.getElementById('guideCanvas');
const guideCtx = guideCanvas.getContext('2d');
let gridW=32, gridH=32, cellSize=14, currentTool='pencil', drawing=false;
let history=[], historyIdx=-1;
let currentEditorKind='skin', guideOn=true;

function setupGrid(w,h){
  gridW=w; gridH=h;
  cellSize = Math.min(14, Math.floor(440/Math.max(w,h)));
  editCanvas.width = w*cellSize;
  editCanvas.height = h*cellSize;
  guideCanvas.width = w*cellSize;
  guideCanvas.height = h*cellSize;
  editCtx.imageSmoothingEnabled=false;
  editCtx.fillStyle = '#ffffff00';
  editCtx.clearRect(0,0,editCanvas.width,editCanvas.height);
  drawGridLines();
  drawGuide();
  history=[editCtx.getImageData(0,0,editCanvas.width,editCanvas.height)];
  historyIdx=0;
}

function toggleGuide(){
  guideOn = !guideOn;
  document.getElementById('guideToggle').classList.toggle('active', guideOn);
  drawGuide();
}

function drawGuide(){
  guideCtx.clearRect(0,0,guideCanvas.width,guideCanvas.height);
  if(!guideOn) return;
  guideCtx.save();
  guideCtx.strokeStyle='rgba(232,182,79,0.55)';
  guideCtx.fillStyle='rgba(232,182,79,0.07)';
  guideCtx.lineWidth=2;
  const W=guideCanvas.width, H=guideCanvas.height;

  if(currentEditorKind==='cape'){
    // cape silhouette: collar at top-center, tapering cloak body below
    const collarW=W*0.22, collarX=(W-collarW)/2, collarY=H*0.04, collarH=H*0.08;
    const bodyTop=collarY+collarH, bodyBottom=H*0.96;
    const topW=W*0.42, botW=W*0.5, cx=W/2;
    guideCtx.beginPath();
    guideCtx.rect(collarX,collarY,collarW,collarH);
    guideCtx.moveTo(cx-topW/2, bodyTop);
    guideCtx.lineTo(cx+topW/2, bodyTop);
    guideCtx.lineTo(cx+botW/2, bodyBottom);
    guideCtx.lineTo(cx-botW/2, bodyBottom);
    guideCtx.closePath();
    guideCtx.fill(); guideCtx.stroke();
  } else {
    // simple front-facing humanoid guide: head, body, two arms, two legs
    const headS=W*0.28, headX=(W-headS)/2, headY=H*0.04;
    const bodyW=W*0.32, bodyX=(W-bodyW)/2, bodyY=headY+headS+H*0.02, bodyH=H*0.32;
    const armW=W*0.13, armH=bodyH, armY=bodyY;
    const legW=W*0.14, legH=H*0.30, legY=bodyY+bodyH;
    guideCtx.strokeRect(headX,headY,headS,headS);
    guideCtx.fillRect(headX,headY,headS,headS);
    guideCtx.strokeRect(bodyX,bodyY,bodyW,bodyH);
    guideCtx.fillRect(bodyX,bodyY,bodyW,bodyH);
    guideCtx.strokeRect(bodyX-armW,armY,armW,armH);
    guideCtx.fillRect(bodyX-armW,armY,armW,armH);
    guideCtx.strokeRect(bodyX+bodyW,armY,armW,armH);
    guideCtx.fillRect(bodyX+bodyW,armY,armW,armH);
    guideCtx.strokeRect(bodyX+bodyW*0.02,legY,legW,legH);
    guideCtx.fillRect(bodyX+bodyW*0.02,legY,legW,legH);
    guideCtx.strokeRect(bodyX+bodyW-legW*1.02,legY,legW,legH);
    guideCtx.fillRect(bodyX+bodyW-legW*1.02,legY,legW,legH);
  }
  guideCtx.restore();
}
function drawGridLines(){
  editCtx.save();
  editCtx.strokeStyle='rgba(128,128,128,0.25)';
  editCtx.lineWidth=1;
  for(let x=0;x<=gridW;x++){ editCtx.beginPath(); editCtx.moveTo(x*cellSize,0); editCtx.lineTo(x*cellSize,gridH*cellSize); editCtx.stroke(); }
  for(let y=0;y<=gridH;y++){ editCtx.beginPath(); editCtx.moveTo(0,y*cellSize); editCtx.lineTo(gridW*cellSize,y*cellSize); editCtx.stroke(); }
  editCtx.restore();
}

function openEditor(kind){
  go('editor');
  currentEditorKind = kind;
  document.getElementById('editorTitle').textContent = kind==='cape' ? '🧥 Cape Editor' : '🧍 Skin Editor';
  document.getElementById('sizeSelect').value = kind==='cape' ? '64x32' : '32x32';
  changeSize();
}
function changeSize(){
  const [w,h] = document.getElementById('sizeSelect').value.split('x').map(Number);
  setupGrid(w,h);
}
setupGrid(32,32);

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
  editCtx.strokeStyle='rgba(128,128,128,0.25)';
  editCtx.strokeRect(x*cellSize,y*cellSize,cellSize,cellSize);
}
function getCellColor(x,y){
  const d = editCtx.getImageData(x*cellSize+1,y*cellSize+1,1,1).data;
  if(d[3]===0) return null;
  return '#'+[d[0],d[1],d[2]].map(v=>v.toString(16).padStart(2,'0')).join('');
}
function floodFill(x,y,target,fillColor){
  if(x<0||y<0||x>=gridW||y>=gridH) return;
  const stack=[[x,y]];
  const visited = new Set();
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
  else if(currentTool==='eyedrop'){ const c=getCellColor(x,y); if(c) document.getElementById('colorPicker').value=c; setTool('pencil'); }
  else if(currentTool==='fill'){ floodFill(x,y,getCellColor(x,y),color); }
}
editCanvas.addEventListener('mousedown', e=>{ drawing=true; handlePaint(e); });
editCanvas.addEventListener('mousemove', e=>{ if(drawing && (currentTool==='pencil'||currentTool==='eraser')) handlePaint(e); });
window.addEventListener('mouseup', ()=>{ if(drawing){ drawing=false; pushHistory(); }});

function pushHistory(){
  history = history.slice(0, historyIdx+1);
  history.push(editCtx.getImageData(0,0,editCanvas.width,editCanvas.height));
  if(history.length>40) history.shift();
  historyIdx = history.length-1;
}
function undo(){ if(historyIdx>0){ historyIdx--; editCtx.putImageData(history[historyIdx],0,0); } }
function redo(){ if(historyIdx<history.length-1){ historyIdx++; editCtx.putImageData(history[historyIdx],0,0); } }
function clearCanvas(){ editCtx.clearRect(0,0,editCanvas.width,editCanvas.height); drawGridLines(); pushHistory(); }

document.getElementById('editUpload').addEventListener('change', e=>{
  const file = e.target.files[0]; if(!file) return;
  const img = new Image();
  img.onload = ()=>{
    editCtx.clearRect(0,0,editCanvas.width,editCanvas.height);
    editCtx.drawImage(img,0,0,gridW*cellSize,gridH*cellSize);
    drawGridLines();
    pushHistory();
  };
  img.src = URL.createObjectURL(file);
});

function downloadCanvas(){
  // export without grid lines: redraw clean to offscreen canvas
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

// palette swatches
const palette = ['#000000','#ffffff','#8d94a3','#e8b64f','#4fd3e8','#b98af0','#7ed67e','#e05d5d','#c97b3f','#3f6cc9','#ffe1b3','#5a3d2b'];
const paletteGrid = document.getElementById('paletteGrid');
palette.forEach(c=>{
  const b = document.createElement('button');
  b.className='swatch-btn'; b.style.background=c;
  b.onclick=()=>document.getElementById('colorPicker').value=c;
  paletteGrid.appendChild(b);
});

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
  // cover-crop the image into a square
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
imgInput.addEventListener('change', e=>{
  const file = e.target.files[0]; if(!file) return;
  const img = new Image();
  img.onload = ()=>{
    const maxW=700; const scale=Math.min(1,maxW/img.width);
    picCanvas.width=img.width*scale; picCanvas.height=img.height*scale;
    picCtx.drawImage(img,0,0,picCanvas.width,picCanvas.height);
    workbench.classList.add('show');
  };
  img.src = URL.createObjectURL(file);
});
picCanvas.addEventListener('click', e=>{
  const rect = picCanvas.getBoundingClientRect();
  const x = Math.floor((e.clientX-rect.left)*(picCanvas.width/rect.width));
  const y = Math.floor((e.clientY-rect.top)*(picCanvas.height/rect.height));
  const [r,g,b] = picCtx.getImageData(x,y,1,1).data;
  const hex = '#'+[r,g,b].map(v=>v.toString(16).padStart(2,'0')).join('').toUpperCase();
  document.getElementById('swatch').style.background=hex;
  document.getElementById('hexOut').value=hex;
  document.getElementById('rgbOut').value=`rgb(${r}, ${g}, ${b})`;
});
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
</script>
</body>
</html>
