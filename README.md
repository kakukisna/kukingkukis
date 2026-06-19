Berikut adalah aplikasi web "Photo Booth Frame Studio" yang bisa langsung kamu gunakan. Cukup simpan sebagai file `.html` dan buka di browser.
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, shrink-to-fit=yes, viewport-fit=cover">
  <title>📸 Photo Booth Frame Studio</title>
  <!-- Font & Icons -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Quicksand', sans-serif;
      background: linear-gradient(145deg, #fff8f0 0%, #ffebb3 100%);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 16px;
      margin: 0;
    }

    .app-container {
      max-width: 480px;
      width: 100%;
      background: rgba(255, 248, 240, 0.8);
      backdrop-filter: blur(20px);
      border-radius: 48px;
      padding: 20px 16px 28px;
      box-shadow: 0 30px 50px rgba(255, 182, 193, 0.25), 0 10px 25px rgba(184, 224, 210, 0.4);
      border: 2px solid rgba(255, 255, 255, 0.7);
      display: flex;
      flex-direction: column;
      gap: 18px;
    }

    h1 {
      font-size: 2rem;
      font-weight: 700;
      color: #d4858b;
      text-align: center;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      letter-spacing: -0.5px;
      text-shadow: 2px 2px 0 #ffd93d;
      margin-bottom: 4px;
    }

    .canvas-wrapper {
      background: #ffe9ec;
      border-radius: 36px;
      padding: 12px;
      box-shadow: inset 0 0 15px #ffb6c1, 0 15px 20px rgba(230, 230, 250, 0.7);
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .photobooth-stage {
      position: relative;
      width: 100%;
      aspect-ratio: 3 / 4;
      background: #fff8f0;
      border-radius: 28px;
      overflow: hidden;
      box-shadow: 0 0 0 5px #ffd93d, 0 0 0 10px #ffb6c1;
      background-image: radial-gradient(circle at 20% 30%, #fff0f5 5%, #ffe4e1 90%);
    }

    #photoCanvas {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: block;
      object-fit: cover;
    }

    .sticker-layer {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 10;
    }

    .sticker-item {
      position: absolute;
      pointer-events: auto;
      cursor: grab;
      user-select: none;
      transform-origin: center;
      transition: filter 0.2s;
    }

    .sticker-item:active {
      cursor: grabbing;
    }

    .sticker-item img {
      width: 100%;
      height: 100%;
      object-fit: contain;
      filter: drop-shadow(0 4px 6px rgba(0,0,0,0.2));
    }

    .delete-sticker {
      position: absolute;
      top: -10px;
      right: -10px;
      background: #ffb6c1;
      color: white;
      border-radius: 50%;
      width: 24px;
      height: 24px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 14px;
      font-weight: bold;
      cursor: pointer;
      box-shadow: 0 2px 6px rgba(0,0,0,0.2);
      border: 2px solid white;
      z-index: 20;
    }

    .controls {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      justify-content: center;
    }

    .btn {
      background: #ffffffd6;
      backdrop-filter: blur(10px);
      border: none;
      padding: 12px 16px;
      border-radius: 40px;
      font-weight: 700;
      font-family: 'Quicksand', sans-serif;
      font-size: 0.9rem;
      color: #5e4b4b;
      display: flex;
      align-items: center;
      gap: 6px;
      box-shadow: 0 8px 15px rgba(255, 182, 193, 0.4);
      border: 2px solid #ffd93d;
      background: #fffbf0;
      transition: all 0.25s;
      cursor: pointer;
    }

    .btn:active {
      transform: scale(0.94);
      background: #ffd93d;
    }

    .btn.primary {
      background: #ffd93d;
      border-color: #ffb6c1;
      color: #4a3a3a;
      font-weight: 700;
    }

    .frame-gallery {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      justify-content: center;
      background: rgba(255,255,240,0.7);
      padding: 12px;
      border-radius: 32px;
    }

    .frame-thumb {
      width: 48px;
      height: 48px;
      border-radius: 16px;
      background: #f9e6ff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 22px;
      border: 3px solid #ffffff;
      box-shadow: 0 4px 8px rgba(184,224,210,0.6);
      cursor: pointer;
      transition: 0.2s;
      background-size: cover;
    }

    .frame-thumb.selected {
      border: 3px solid #ffd93d;
      transform: scale(1.15);
      box-shadow: 0 0 15px #ffb6c1;
    }

    .slider-group {
      display: flex;
      flex-direction: column;
      gap: 6px;
      background: #ffffffdb;
      border-radius: 30px;
      padding: 12px 16px;
    }

    input[type="range"] {
      width: 100%;
      accent-color: #ffb6c1;
    }

    .sticker-panel {
      display: flex;
      gap: 10px;
      overflow-x: auto;
      padding: 8px 5px;
    }

    .sticker-option {
      width: 55px;
      height: 55px;
      border-radius: 20px;
      background: #fff0f5;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 32px;
      border: 2px solid #ffd93d;
      flex-shrink: 0;
      cursor: pointer;
    }

    @media (max-width: 400px) {
      .btn {
        padding: 10px 12px;
        font-size: 0.8rem;
      }
    }
  </style>
</head>
<body>
<div class="app-container">
  <h1>
    <span>🌸</span> Photo Booth Frame <span>🎀</span>
  </h1>

  <!-- Stage -->
  <div class="canvas-wrapper">
    <div class="photobooth-stage" id="stageContainer">
      <canvas id="photoCanvas"></canvas>
      <div class="sticker-layer" id="stickerLayer"></div>
    </div>
  </div>

  <!-- Main Buttons -->
  <div class="controls">
    <button class="btn" id="captureBtn"><i class="fas fa-camera"></i> Ambil Foto</button>
    <button class="btn" id="uploadBtn"><i class="fas fa-upload"></i> Upload</button>
    <button class="btn primary" id="saveBtn"><i class="fas fa-download"></i> Simpan HD</button>
  </div>

  <!-- Hidden camera elements -->
  <input type="file" id="fileInput" accept="image/*" style="display: none;">
  <video id="videoElement" autoplay playsinline style="display: none;"></video>
  <canvas id="hiddenCanvas" style="display: none;"></canvas>

  <!-- Frame selection -->
  <div class="frame-gallery" id="frameGallery">
    <!-- 8 default frames + custom upload button -->
  </div>

  <!-- Filter sliders -->
  <div class="slider-group">
    <label>🌫️ Blur <span id="blurVal">0</span>%</label>
    <input type="range" id="blurSlider" min="0" max="10" value="0" step="0.1">
    <label>🌅 Fade <span id="fadeVal">0</span>%</label>
    <input type="range" id="fadeSlider" min="0" max="100" value="0">
    <label>☀️ Brightness <span id="brightVal">100</span>%</label>
    <input type="range" id="brightSlider" min="50" max="200" value="100">
  </div>

  <!-- Sticker panel -->
  <div class="sticker-panel" id="stickerPanel"></div>
  <button class="btn" id="addCustomFrameBtn" style="align-self: center;"><i class="fas fa-plus-circle"></i> Tambah Frame Sendiri</button>
  <input type="file" id="customFrameInput" accept="image/png" style="display: none;">
</div>

<script>
  (function() {
    // ---------- DOM Elements ----------
    const canvas = document.getElementById('photoCanvas');
    const ctx = canvas.getContext('2d');
    const stage = document.getElementById('stageContainer');
    const stickerLayer = document.getElementById('stickerLayer');
    const video = document.getElementById('videoElement');
    const hiddenCanvas = document.getElementById('hiddenCanvas');
    const hiddenCtx = hiddenCanvas.getContext('2d');
    const fileInput = document.getElementById('fileInput');
    const captureBtn = document.getElementById('captureBtn');
    const uploadBtn = document.getElementById('uploadBtn');
    const saveBtn = document.getElementById('saveBtn');
    const frameGallery = document.getElementById('frameGallery');
    const blurSlider = document.getElementById('blurSlider');
    const fadeSlider = document.getElementById('fadeSlider');
    const brightSlider = document.getElementById('brightSlider');
    const blurVal = document.getElementById('blurVal');
    const fadeVal = document.getElementById('fadeVal');
    const brightVal = document.getElementById('brightVal');
    const stickerPanel = document.getElementById('stickerPanel');
    const addCustomFrameBtn = document.getElementById('addCustomFrameBtn');
    const customFrameInput = document.getElementById('customFrameInput');

    // ---------- State ----------
    let currentImage = null; // HTMLImageElement currently displayed
    let currentFrameType = 'default'; // 'default' | custom image url | border style
    let customFrameImage = null; // For uploaded PNG frame
    let frameStyle = { borderColor: '#FFB6C1', type: 'heart' }; // default heart pastel
    let stickers = []; // { id, src, x, y, width, height, rotation }
    let selectedFrameIndex = 0;
    let stream = null;

    // Frame definitions (8 built-in)
    const builtInFrames = [
      { name: 'Hati', icon: '❤️', color: '#FFB6C1', type: 'heart' },
      { name: 'Bintang', icon: '⭐', color: '#FFD93D', type: 'star' },
      { name: 'Bunga Matahari', icon: '🌻', color: '#FFC107', type: 'sunflower' },
      { name: 'Tulip Pink', icon: '🌷', color: '#FFB6C1', type: 'tulip' },
      { name: 'Lily Putih', icon: '🌸', color: '#FFF8F0', type: 'lily' },
      { name: 'Freesia Lavender', icon: '💜', color: '#E6E6FA', type: 'freesia' },
      { name: 'Grid Aesthetic', icon: '🔲', color: '#B8E0D2', type: 'grid' },
      { name: 'Pastel Border', icon: '🍬', color: '#FFD93D', type: 'border' }
    ];

    // Sticker library
    const stickerLibrary = [
      { name: 'Hello Kitty', emoji: '🐱', src: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Ccircle cx="50" cy="50" r="45" fill="%23FFB6C1"/%3E%3Ccircle cx="35" cy="40" r="4" fill="black"/%3E%3Ccircle cx="65" cy="40" r="4" fill="black"/%3E%3Cellipse cx="50" cy="60" rx="8" ry="5" fill="%23FF69B4"/%3E%3Ctext x="50" y="80" text-anchor="middle" font-size="22" fill="%23fff"›🐱%3C/text%3E%3C/svg%3E' },
      { name: 'Kuromi', emoji: '😈', src: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Ccircle cx="50" cy="50" r="42" fill="%23d8b4fe"/%3E%3Ctext x="50" y="65" text-anchor="middle" font-size="40" fill="%23333"%3E😈%3C/text%3E%3C/svg%3E' },
      { name: 'My Melody', emoji: '🐰', src: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Ccircle cx="50" cy="50" r="42" fill="%23FFB6C1"/%3E%3Ctext x="50" y="65" text-anchor="middle" font-size="42"%3E🐰%3C/text%3E%3C/svg%3E' },
      { name: 'Snoopy', emoji: '🐶', src: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Cellipse cx="50" cy="50" rx="40" ry="38" fill="white" stroke="black" stroke-width="3"/%3E%3Ctext x="50" y="68" text-anchor="middle" font-size="44"%3E🐶%3C/text%3E%3C/svg%3E' },
      { name: 'Kitty2', emoji: '🎀', src: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Ccircle cx="50" cy="50" r="45" fill="%23ffd1dc"/%3E%3Ctext x="50" y="70" text-anchor="middle" font-size="40"%3E🎀%3C/text%3E%3C/svg%3E' },
      { name: 'Kuromi2', emoji: '💀', src: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Ccircle cx="50" cy="50" r="42" fill="%23b388eb"/%3E%3Ctext x="50" y="68" text-anchor="middle" font-size="40"%3E💀%3C/text%3E%3C/svg%3E' },
      { name: 'Melody2', emoji: '🌸', src: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Ccircle cx="50" cy="50" r="42" fill="%23ffd1dc"/%3E%3Ctext x="50" y="68" text-anchor="middle" font-size="40"%3E🌸%3C/text%3E%3C/svg%3E' },
      { name: 'Snoopy2', emoji: '🦴', src: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Ccircle cx="50" cy="50" r="40" fill="%23f0f0f0" stroke="%23333" stroke-width="2"/%3E%3Ctext x="50" y="68" text-anchor="middle" font-size="40"%3E🦴%3C/text%3E%3C/svg%3E' }
    ];

    // ---------- Utilities ----------
    function resizeCanvasToStage() {
      const rect = stage.getBoundingClientRect();
      if (rect.width === 0 || rect.height === 0) return;
      canvas.width = rect.width;
      canvas.height = rect.height;
    }

    // Draw everything: photo + frame + stickers
    function drawComposite() {
      resizeCanvasToStage();
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      
      // Draw base image with filters
      if (currentImage) {
        ctx.filter = `blur(${blurSlider.value}px) brightness(${brightSlider.value}%)`;
        ctx.globalAlpha = 1 - (fadeSlider.value / 100);
        ctx.drawImage(currentImage, 0, 0, canvas.width, canvas.height);
        ctx.filter = 'none';
        ctx.globalAlpha = 1.0;
      } else {
        // placeholder
        ctx.fillStyle = '#FFF8F0';
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        ctx.font = '20px Quicksand';
        ctx.fillStyle = '#d4858b';
        ctx.textAlign = 'center';
        ctx.fillText('📷 Ambil atau upload foto', canvas.width/2, canvas.height/2);
      }

      // Draw frame based on type
      drawFrame();

      // Stickers are rendered via DOM, but we keep them on layer
    }

    function drawFrame() {
      if (!currentImage) return;
      const w = canvas.width;
      const h = canvas.height;
      
      if (customFrameImage && currentFrameType === 'custom') {
        ctx.drawImage(customFrameImage, 0, 0, w, h);
        return;
      }

      const type = frameStyle.type;
      ctx.save();
      ctx.lineWidth = 12;
      ctx.strokeStyle = frameStyle.borderColor || '#FFB6C1';
      
      if (type === 'heart') {
        drawHeartBorder(ctx, w, h);
      } else if (type === 'star') {
        drawStarBorder(ctx, w, h);
      } else if (type === 'sunflower') {
        drawSunflowerBorder(ctx, w, h);
      } else if (type === 'tulip') {
        drawTulipBorder(ctx, w, h);
      } else if (type === 'lily') {
        drawLilyBorder(ctx, w, h);
      } else if (type === 'freesia') {
        drawFreesiaBorder(ctx, w, h);
      } else if (type === 'grid') {
        drawGridBorder(ctx, w, h);
      } else {
        // border default
        ctx.strokeStyle = frameStyle.borderColor;
        ctx.lineWidth = 14;
        ctx.strokeRect(8, 8, w-16, h-16);
      }
      ctx.restore();
    }

    function drawHeartBorder(ctx, w, h) {
      for (let i=0; i<12; i++) {
        let x = (i%4)*w/4 + 30;
        let y = Math.floor(i/4)*h/3 + 30;
        ctx.fillStyle = '#FFB6C1';
        ctx.font = '28px sans-serif';
        ctx.fillText('❤️', x, y);
      }
    }
    function drawStarBorder(ctx, w, h) {
      for (let i=0;i<10;i++) {
        ctx.fillStyle = '#FFD93D';
        ctx.font = '30px sans-serif';
        ctx.fillText('⭐', 20+ i*35, 35);
        ctx.fillText('⭐', 20+ i*35, h-20);
      }
    }
    function drawSunflowerBorder(ctx,w,h){ for(let i=0;i<8;i++){ ctx.fillStyle='#FFC107'; ctx.font='32px sans-serif'; ctx.fillText('🌻',25+i*50,40); ctx.fillText('🌻',25+i*50,h-25); } }
    function drawTulipBorder(ctx,w,h){ for(let i=0;i<6;i++){ ctx.fillText('🌷',30+i*70,45); ctx.fillText('🌷',30+i*70,h-30); } }
    function drawLilyBorder(ctx,w,h){ for(let i=0;i<7;i++){ ctx.fillText('🌸',20+i*60,35); } }
    function drawFreesiaBorder(ctx,w,h){ for(let i=0;i<8;i++){ ctx.fillStyle='#E6E6FA'; ctx.font='30px sans-serif'; ctx.fillText('💜',20+i*55,40); } }
    function drawGridBorder(ctx,w,h){ ctx.strokeStyle='#B8E0D2'; ctx.lineWidth=10; for(let i=0;i<4;i++){ ctx.strokeRect(15+i*70,15,50,50); } }

    // Update display
    function redrawAll() {
      drawComposite();
      renderStickersToLayer();
    }

    // Sticker management
    function renderStickersToLayer() {
      stickerLayer.innerHTML = '';
      stickers.forEach((sticker, index) => {
        const div = document.createElement('div');
        div.className = 'sticker-item';
        div.style.left = sticker.x + 'px';
        div.style.top = sticker.y + 'px';
        div.style.width = sticker.width + 'px';
        div.style.height = sticker.height + 'px';
        div.style.transform = `rotate(${sticker.rotation}deg)`;
        div.dataset.index = index;
        
        const img = document.createElement('img');
        img.src = sticker.src;
        img.draggable = false;
        div.appendChild(img);
        
        const delBtn = document.createElement('span');
        delBtn.className = 'delete-sticker';
        delBtn.innerHTML = '×';
        delBtn.addEventListener('click', (e) => {
          e.stopPropagation();
          stickers.splice(index, 1);
          redrawAll();
        });
        div.appendChild(delBtn);
        
        makeStickerDraggable(div, index);
        stickerLayer.appendChild(div);
      });
    }

    function makeStickerDraggable(element, index) {
      let startX, startY, initialX, initialY;
      element.addEventListener('pointerdown', (e) => {
        e.preventDefault();
        startX = e.clientX;
        startY = e.clientY;
        initialX = stickers[index].x;
        initialY = stickers[index].y;
        
        function onMove(e) {
          const dx = e.clientX - startX;
          const dy = e.clientY - startY;
          stickers[index].x = Math.max(0, Math.min(stage.clientWidth - stickers[index].width, initialX + dx));
          stickers[index].y = Math.max(0, Math.min(stage.clientHeight - stickers[index].height, initialY + dy));
          renderStickersToLayer();
        }
        function onUp() {
          window.removeEventListener('pointermove', onMove);
          window.removeEventListener('pointerup', onUp);
        }
        window.addEventListener('pointermove', onMove);
        window.addEventListener('pointerup', onUp);
      });
    }

    function addSticker(src) {
      const size = 70;
      stickers.push({
        src: src,
        x: 40 + Math.random()*80,
        y: 100 + Math.random()*150,
        width: size,
        height: size,
        rotation: 0
      });
      redrawAll();
    }

    // Load image from file or stream
    function loadImageFromSource(imgElement) {
      currentImage = imgElement;
      resizeCanvasToStage();
      redrawAll();
    }

    // Camera
    async function startCamera() {
      if (stream) {
        stream.getTracks().forEach(track => track.stop());
      }
      try {
        stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'user' } });
        video.srcObject = stream;
        video.play();
        // Show video temporarily? better capture direct
        captureBtn.textContent = '📸 Jepret!';
        captureBtn.removeEventListener('click', startCamera);
        captureBtn.addEventListener('click', capturePhoto, { once: false });
      } catch (err) {
        alert('Kamera tidak dapat diakses.');
      }
    }

    function capturePhoto() {
      if (!stream) return;
      hiddenCanvas.width = video.videoWidth || 640;
      hiddenCanvas.height = video.videoHeight || 480;
      hiddenCtx.drawImage(video, 0, 0, hiddenCanvas.width, hiddenCanvas.height);
      const img = new Image();
      img.onload = () => {
        loadImageFromSource(img);
        stopCamera();
        capt
