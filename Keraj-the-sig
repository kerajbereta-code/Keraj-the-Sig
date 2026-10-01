<!DOCTYPE html?
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neon Velocity 3D - Cyberpunk Multi-Mode Arena</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      overflow: hidden;
      background-color: #03030c;
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      user-select: none;
      color: #fff;
    }
    #game-canvas {
      width: 100vw;
      height: 100vh;
      display: block;
    }
    
    /* HUD Overlays */
    .hud-card {
      position: absolute;
      background: rgba(8, 12, 28, 0.75);
      border: 1px solid rgba(0, 243, 255, 0.3);
      box-shadow: 0 0 15px rgba(0, 243, 255, 0.15);
      backdrop-filter: blur(8px);
      border-radius: 8px;
      padding: 12px 18px;
      pointer-events: none;
    }
    
    #pos-hud {
      top: 20px;
      left: 20px;
      display: flex;
      align-items: baseline;
      gap: 6px;
    }
    #pos-hud .label { font-size: 11px; color: #00f3ff; letter-spacing: 2px; text-transform: uppercase; }
    #pos-hud .value { font-size: 32px; font-weight: 900; color: #fff; text-shadow: 0 0 10px #00f3ff; }

    #speed-hud {
      top: 20px;
      left: 50%;
      transform: translateX(-50%);
      text-align: center;
      min-width: 180px;
    }
    #speed-hud .value { font-size: 36px; font-weight: 900; color: #fff; font-style: italic; }
    #speed-hud .unit { font-size: 12px; color: #ff007f; letter-spacing: 1px; }
    #boost-bar-container {
      margin-top: 6px;
      width: 100%;
      height: 6px;
      background: rgba(255,255,255,0.1);
      border-radius: 3px;
      overflow: hidden;
    }
    #boost-bar {
      width: 100%;
      height: 100%;
      background: linear-gradient(90deg, #ff007f, #00f3ff);
      transition: width 0.1s linear;
    }

    #info-hud {
      top: 20px;
      right: 20px;
      text-align: right;
    }
    #info-hud .mode-title { font-size: 12px; color: #00f3ff; letter-spacing: 2px; text-transform: uppercase; }
    #info-hud .value { font-size: 24px; font-weight: 800; color: #ff007f; text-shadow: 0 0 10px #ff007f; }
    #info-hud .sub-value { font-size: 14px; font-family: monospace; color: #fff; margin-top: 2px; }

    #emp-status {
      position: absolute;
      bottom: 25px;
      left: 50%;
      transform: translateX(-50%);
      font-size: 12px;
      font-weight: 800;
      letter-spacing: 2px;
      color: #00f3ff;
      border: 1px solid #00f3ff;
      padding: 6px 16px;
      border-radius: 20px;
      background: rgba(0, 243, 255, 0.1);
      text-shadow: 0 0 8px #00f3ff;
    }

    /* Leaderboard / Stats */
    #leaderboard {
      position: absolute;
      bottom: 20px;
      left: 20px;
      width: 230px;
      max-height: 200px;
      font-size: 11px;
    }
    #leaderboard h4 {
      color: #00f3ff;
      margin-bottom: 8px;
      letter-spacing: 1.5px;
      border-bottom: 1px solid rgba(0, 243, 255, 0.2);
      padding-bottom: 4px;
    }
    .lb-row {
      display: flex;
      justify-content: space-between;
      margin-bottom: 4px;
      opacity: 0.8;
    }
    .lb-row.player { opacity: 1; color: #ff007f; font-weight: 700; }

    /* Radar Map */
    #radar {
      position: absolute;
      bottom: 20px;
      right: 20px;
      width: 120px;
      height: 120px;
      border-radius: 50%;
      background: rgba(8, 12, 28, 0.85);
      border: 2px solid #00f3ff;
      box-shadow: 0 0 15px rgba(0, 243, 255, 0.2);
    }

    /* Menus & Overlays */
    #start-overlay, #options-modal, #gameover-modal {
      position: absolute;
      inset: 0;
      background: rgba(3, 3, 12, 0.85);
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      z-index: 100;
      backdrop-filter: blur(10px);
    }

    #options-modal, #gameover-modal {
      display: none;
      z-index: 101;
    }

    .menu-card {
      background: rgba(8, 12, 28, 0.9);
      border: 1px solid #00f3ff;
      box-shadow: 0 0 25px rgba(0, 243, 255, 0.2);
      padding: 30px 40px;
      border-radius: 12px;
      width: 400px;
      text-align: center;
    }

    h1, h2 {
      font-weight: 900;
      letter-spacing: 3px;
      background: linear-gradient(45deg, #00f3ff, #ff007f);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      margin-bottom: 15px;
      text-transform: uppercase;
    }

    .option-group {
      margin: 14px 0;
      text-align: left;
    }

    .option-group label {
      display: block;
      font-size: 11px;
      color: #00f3ff;
      letter-spacing: 1px;
      margin-bottom: 5px;
      text-transform: uppercase;
    }

    .option-group select, .option-group input[type="color"] {
      width: 100%;
      padding: 8px 12px;
      background: #03030c;
      border: 1px solid rgba(0, 243, 255, 0.4);
      color: #fff;
      border-radius: 6px;
      outline: none;
      font-size: 13px;
    }

    .btn-container {
      display: flex;
      gap: 10px;
      justify-content: center;
      margin-top: 20px;
    }

    .btn {
      padding: 12px 24px;
      font-size: 13px;
      font-weight: 800;
      letter-spacing: 1px;
      color: #03030c;
      background: #00f3ff;
      border: none;
      border-radius: 30px;
      cursor: pointer;
      box-shadow: 0 0 15px #00f3ff;
      transition: all 0.2s ease;
    }

    .btn:hover {
      transform: scale(1.05);
      background: #ff007f;
      color: #fff;
      box-shadow: 0 0 20px #ff007f;
    }

    .btn-secondary {
      background: transparent;
      color: #00f3ff;
      border: 1px solid #00f3ff;
      box-shadow: none;
    }

    .btn-secondary:hover {
      background: rgba(0, 243, 255, 0.1);
      color: #00f3ff;
      box-shadow: 0 0 10px #00f3ff;
    }

    #options-trigger-btn {
      position: absolute;
      top: 20px;
      right: 180px;
      z-index: 10;
      pointer-events: auto;
      padding: 8px 16px;
      font-size: 11px;
    }

    .controls-hint {
      margin-top: 15px;
      font-size: 11px;
      color: rgba(255, 255, 255, 0.6);
      text-align: center;
      line-height: 1.5;
    }
  </style>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>

  <canvas id="game-canvas"></canvas>

  <button id="options-trigger-btn" class="btn btn-secondary">⚙ MODES & OPTIONS</button>

  <div id="pos-hud" class="hud-card">
    <span id="pos-label" class="label">RANK</span>
    <span id="pos-val" class="value">1</span>
  </div>

  <div id="speed-hud" class="hud-card">
    <div id="speed-val" class="value">0</div>
    <div class="unit">KM/H</div>
    <div id="boost-bar-container">
      <div id="boost-bar"></div>
    </div>
  </div>

  <div id="info-hud" class="hud-card">
    <div id="mode-title-val" class="mode-title">CIRCUIT RACE</div>
    <div id="main-stat-val" class="value">LAP 1/3</div>
    <div id="sub-stat-val" class="sub-value">00:00.00</div>
  </div>

  <div id="emp-status">⚡ EMP SHOCKWAVE [E KEY]</div>

  <div id="leaderboard" class="hud-card">
    <h4 id="lb-title">STANDINGS</h4>
    <div id="lb-rows"></div>
  </div>

  <canvas id="radar"></canvas>

  <!-- Start Screen -->
  <div id="start-overlay">
    <div class="menu-card">
      <h1>Neon Velocity 3D</h1>
      <p style="color: #00f3ff; letter-spacing: 2px; font-size: 12px; margin-bottom: 10px;">CYBERPUNK HOVER ARENA</p>
      
      <div class="controls-hint">
        <b>[W / UP]</b> Thrust &nbsp;|&nbsp; <b>[S / DOWN]</b> Brake / Reverse<br>
        <b>[A / D]</b> or <b>[LEFT / RIGHT]</b> Steering<br>
        <b>[SHIFT / SPACE]</b> Nitro &nbsp;|&nbsp; <b>[E]</b> EMP Shockwave<br>
        <b>[C]</b> Switch Camera Angle
      </div>

      <div class="btn-container">
        <button id="open-options-btn" class="btn btn-secondary">SELECT MODE</button>
        <button id="start-btn" class="btn">START GAME</button>
      </div>
    </div>
  </div>

  <!-- Options & Game Mode Modal -->
  <div id="options-modal">
    <div class="menu-card">
      <h2>Game Settings</h2>

      <div class="option-group">
        <label for="opt-mode">Game Mode</label>
        <select id="opt-mode">
          <option value="race" selected>🏁 Circuit Race (3 Laps Speed Race)</option>
          <option value="elimination">☠ Elimination Derby (Knockout AI with EMP)</option>
          <option value="freeroam">🌌 Open World Free Roam & Stunts</option>
        </select>
      </div>

      <div class="option-group">
        <label for="opt-difficulty">AI Difficulty</label>
        <select id="opt-difficulty">
          <option value="easy">Easy</option>
          <option value="medium" selected>Medium</option>
          <option value="hard">Hard</option>
        </select>
      </div>

      <div class="option-group">
        <label for="opt-opponents">Number of AI Hovercrafts</label>
        <select id="opt-opponents">
          <option value="3">3 Opponents</option>
          <option value="5">5 Opponents</option>
          <option value="8" selected>8 Opponents</option>
        </select>
      </div>

      <div class="option-group">
        <label for="opt-color">Hovercraft Neon Paint</label>
        <input type="color" id="opt-color" value="#ff007f">
      </div>

      <div class="btn-container">
        <button id="save-options-btn" class="btn">APPLY & PLAY</button>
      </div>
    </div>
  </div>

  <!-- Game Over Screen -->
  <div id="gameover-modal">
    <div class="menu-card">
      <h2 id="go-title">MATCH FINISHED</h2>
      <p id="go-desc" style="color: #00f3ff; margin-bottom: 20px; font-size: 14px;"></p>
      <button id="restart-btn" class="btn">PLAY AGAIN</button>
    </div>
  </div>

  <script>
    // Audio Synthesizer
    class SoundEngine {
      constructor() {
        this.ctx = null;
        this.engineOsc = null;
        this.engineGain = null;
      }
      init() {
        if (this.ctx) return;
        this.ctx = new (window.AudioContext || window.webkitAudioContext)();
        this.engineOsc = this.ctx.createOscillator();
        this.engineGain = this.ctx.createGain();
        this.engineOsc.type = 'sawtooth';
        this.engineOsc.frequency.setValueAtTime(60, this.ctx.currentTime);
        this.engineGain.gain.setValueAtTime(0.04, this.ctx.currentTime);
        this.engineOsc.connect(this.engineGain);
        this.engineGain.connect(this.ctx.destination);
        this.engineOsc.start();
      }
      updateEngine(speedRatio) {
        if (!this.ctx) return;
        this.engineOsc.frequency.setTargetAtTime(60 + speedRatio * 180, this.ctx.currentTime, 0.05);
      }
      playEMP() {
        if (!this.ctx) return;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = 'square';
        osc.frequency.setValueAtTime(160, this.ctx.currentTime);
        osc.frequency.exponentialRampToValueAtTime(25, this.ctx.currentTime + 0.45);
        gain.gain.setValueAtTime(0.3, this.ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.45);
        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start();
        osc.stop(this.ctx.currentTime + 0.45);
      }
    }

    const sound = new SoundEngine();

    // Configuration
    const gameConfig = {
      mode: 'race', // 'race', 'elimination', 'freeroam'
      difficulty: 'medium',
      numOpponents: 8,
      craftColor: 0xff007f,
      cameraMode: 0
    };

    // Three.js Scene Setup
    const canvas = document.getElementById('game-canvas');
    const scene = new THREE.Scene();
    scene.fog = new THREE.FogExp2(0x03030c, 0.0035);

    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    const renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));

    scene.add(new THREE.AmbientLight(0xffffff, 0.6));
    const dirLight = new THREE.DirectionalLight(0x00f3ff, 1.2);
    dirLight.position.set(50, 100, 50);
    scene.add(dirLight);

    const gridHelper = new THREE.GridHelper(2000, 100, 0xff007f, 0x00f3ff);
    gridHelper.position.y = -1;
    scene.add(gridHelper);

    // Track Setup
    const trackPoints = [];
    const numPoints = 12, radiusX = 180, radiusZ = 320;
    for (let i = 0; i < numPoints; i++) {
      const angle = (i / numPoints) * Math.PI * 2;
      const x = Math.sin(angle) * radiusX + Math.sin(angle * 3) * 30;
      const z = Math.cos(angle) * radiusZ;
      trackPoints.push(new THREE.Vector3(x, 0, z));
    }
    const trackCurve = new THREE.CatmullRomCurve3(trackPoints, true);

    const trackRings = [];
    const ringGeom = new THREE.TorusGeometry(12, 0.4, 8, 24);
    const ringMat = new THREE.MeshBasicMaterial({ color: 0x00f3ff, wireframe: true });
    for (let i = 0; i < 40; i++) {
      const ring = new THREE.Mesh(ringGeom, ringMat);
      ring.position.copy(trackCurve.getPoint(i / 40));
      ring.lookAt(trackCurve.getPoint((i + 1) % 40 / 40));
      scene.add(ring);
      trackRings.push(ring);
    }

    // Builder for Hovercraft
    function createHovercraft(colorHex) {
      const group = new THREE.Group();
      const bodyGeom = new THREE.ConeGeometry(2, 6, 5);
      bodyGeom.rotateX(Math.PI / 2);
      const body = new THREE.Mesh(bodyGeom, new THREE.MeshStandardMaterial({ color: colorHex, roughness: 0.2, metalness: 0.8 }));
      group.add(body);

      const wingGeom = new THREE.BoxGeometry(7, 0.2, 2);
      const wings = new THREE.Mesh(wingGeom, new THREE.MeshStandardMaterial({ color: 0x111122 }));
      wings.position.set(0, 0, 0.5);
      group.add(wings);

      const engineGeom = new THREE.CylinderGeometry(0.8, 0.8, 2, 8);
      engineGeom.rotateX(Math.PI / 2);
      const engineMat = new THREE.MeshBasicMaterial({ color: 0x00f3ff });
      const engineL = new THREE.Mesh(engineGeom, engineMat);
      engineL.position.set(-1.8, 0.2, 2);
      const engineR = engineL.clone();
      engineR.position.x = 1.8;
      group.add(engineL, engineR);

      return group;
    }

    let playerCraft = createHovercraft(gameConfig.craftColor);
    scene.add(playerCraft);

    const player = {
      mesh: playerCraft,
      pos: new THREE.Vector3(0, 0, radiusZ),
      speed: 0,
      heading: 0,
      maxSpeed: 1.8,
      boost: 100,
      empReady: true,
      lap: 1,
      progress: 0,
      hp: 100,
      kills: 0,
      startTime: Date.now()
    };

    const botNames = ['NEON_PHANTOM', 'CYBER_BLADE', 'SYNTH_WAVE', 'GRID_RUNNER', 'ZERO_COOL', 'BYTE_HAWK', 'KINETIC_X', 'VOID_RACER'];
    const botColors = [0x00f3ff, 0x39ff14, 0xff0055, 0xffff00, 0xa100ff, 0xff8c00, 0x00ffff, 0xff00aa];
    let bots = [];

    function initBots() {
      bots.forEach(b => scene.remove(b.mesh));
      bots = [];

      let speedMult = 1.0;
      if (gameConfig.difficulty === 'easy') speedMult = 0.75;
      if (gameConfig.difficulty === 'hard') speedMult = 1.25;

      for (let i = 0; i < gameConfig.numOpponents; i++) {
        const mesh = createHovercraft(botColors[i % botColors.length]);
        scene.add(mesh);
        bots.push({
          id: i,
          name: botNames[i % botNames.length],
          mesh,
          t: (i + 1) * 0.02,
          speed: (0.0006 + Math.random() * 0.00015) * speedMult,
          lap: 1,
          progress: 0,
          hp: 100,
          alive: true
        });
      }
    }

    // Input Controller
    const keys = {};
    window.addEventListener('keydown', e => {
      keys[e.key.toLowerCase()] = true;
      if (e.key.toLowerCase() === 'e') triggerEMP();
      if (e.key.toLowerCase() === 'c') gameConfig.cameraMode = (gameConfig.cameraMode + 1) % 3;
    });
    window.addEventListener('keyup', e => keys[e.key.toLowerCase()] = false);

    // EMP Shockwave
    const empMesh = new THREE.Mesh(
      new THREE.SphereGeometry(1, 16, 16),
      new THREE.MeshBasicMaterial({ color: 0x00f3ff, wireframe: true, transparent: true, opacity: 0.8 })
    );
    scene.add(empMesh);
    empMesh.visible = false;

    function triggerEMP() {
      if (!player.empReady) return;
      player.empReady = false;
      sound.playEMP();
      document.getElementById('emp-status').innerText = '⚡ EMP RECHARGING...';
      document.getElementById('emp-status').style.opacity = '0.4';

      empMesh.position.copy(player.pos);
      empMesh.scale.set(1, 1, 1);
      empMesh.visible = true;

      bots.forEach(bot => {
        if (!bot.alive) return;
        if (bot.mesh.position.distanceTo(player.pos) < 55) {
          if (gameConfig.mode === 'elimination') {
            bot.hp -= 50;
            if (bot.hp <= 0) {
              bot.alive = false;
              bot.mesh.visible = false;
              player.kills++;
            }
          } else {
            const origSpeed = bot.speed;
            bot.speed = 0.0001;
            setTimeout(() => bot.speed = origSpeed, 3000);
          }
        }
      });

      setTimeout(() => {
        player.empReady = true;
        document.getElementById('emp-status').innerText = '⚡ EMP SHOCKWAVE [E KEY]';
        document.getElementById('emp-status').style.opacity = '1';
      }, 6000);
    }

    // Radar Map
    const radarCanvas = document.getElementById('radar');
    const radarCtx = radarCanvas.getContext('2d');
    radarCanvas.width = 120;
    radarCanvas.height = 120;

    function drawRadar() {
      radarCtx.clearRect(0, 0, 120, 120);
      const cx = 60, cy = 60, scale = 0.12;

      radarCtx.fillStyle = '#ff007f';
      radarCtx.beginPath();
      radarCtx.arc(cx, cy, 4, 0, Math.PI * 2);
      radarCtx.fill();

      radarCtx.fillStyle = '#00f3ff';
      bots.forEach(bot => {
        if (!bot.alive) return;
        const dx = (bot.mesh.position.x - player.pos.x) * scale;
        const dz = (bot.mesh.position.z - player.pos.z) * scale;
        if (Math.abs(dx) < 50 && Math.abs(dz) < 50) {
          radarCtx.beginPath();
          radarCtx.arc(cx + dx, cy + dz, 3, 0, Math.PI * 2);
          radarCtx.fill();
        }
      });
    }

    // Menu Controls
    const optionsModal = document.getElementById('options-modal');
    const gameoverModal = document.getElementById('gameover-modal');

    document.getElementById('open-options-btn').addEventListener('click', () => optionsModal.style.display = 'flex');
    document.getElementById('options-trigger-btn').addEventListener('click', () => optionsModal.style.display = 'flex');

    document.getElementById('save-options-btn').addEventListener('click', () => {
      gameConfig.mode = document.getElementById('opt-mode').value;
      gameConfig.difficulty = document.getElementById('opt-difficulty').value;
      gameConfig.numOpponents = parseInt(document.getElementById('opt-opponents').value);
      gameConfig.craftColor = parseInt(document.getElementById('opt-color').value.replace('#', '0x'));

      scene.remove(playerCraft);
      playerCraft = createHovercraft(gameConfig.craftColor);
      scene.add(playerCraft);
      player.mesh = playerCraft;

      initBots();
      applyModeUI();
      optionsModal.style.display = 'none';
    });

    function applyModeUI() {
      const modeTitle = document.getElementById('mode-title-val');
      const posLabel = document.getElementById('pos-label');
      
      if (gameConfig.mode === 'race') {
        modeTitle.innerText = 'CIRCUIT RACE';
        posLabel.innerText = 'RANK';
        trackRings.forEach(r => r.visible = true);
      } else if (gameConfig.mode === 'elimination') {
        modeTitle.innerText = 'ELIMINATION DERBY';
        posLabel.innerText = 'KILLS';
        trackRings.forEach(r => r.visible = false);
      } else if (gameConfig.mode === 'freeroam') {
        modeTitle.innerText = 'FREE ROAM';
        posLabel.innerText = 'MODE';
        document.getElementById('pos-val').innerText = 'FREE';
        trackRings.forEach(r => r.visible = true);
      }
    }

    let gameActive = false;

    function startGame() {
      document.getElementById('start-overlay').style.display = 'none';
      gameoverModal.style.display = 'none';
      sound.init();
      initBots();
      player.pos.set(0, 0, radiusZ);
      player.speed = 0;
      player.kills = 0;
      player.lap = 1;
      player.startTime = Date.now();
      applyModeUI();
      gameActive = true;
    }

    document.getElementById('start-btn').addEventListener('click', startGame);
    document.getElementById('restart-btn').addEventListener('click', startGame);

    function triggerGameOver(win, message) {
      gameActive = false;
      document.getElementById('go-title').innerText = win ? 'VICTORY!' : 'DERBY ENDED';
      document.getElementById('go-desc').innerText = message;
      gameoverModal.style.display = 'flex';
    }

    // Main Game Loop
    function animate() {
      requestAnimationFrame(animate);

      if (!gameActive) {
        renderer.render(scene, camera);
        return;
      }

      // Driving Dynamics
      if (keys['a'] || keys['arrowleft']) player.heading += 0.04;
      if (keys['d'] || keys['arrowright']) player.heading -= 0.04;

      let accel = 0;
      if (keys['w'] || keys['arrowup']) accel = 0.03;
      if (keys['s'] || keys['arrowdown']) accel = -0.02;

      let currentMaxSpeed = player.maxSpeed;
      if ((keys['shift'] || keys[' ']) && player.boost > 0) {
        accel *= 2.2;
        currentMaxSpeed *= 1.6;
        player.boost = Math.max(0, player.boost - 0.8);
      } else if (player.boost < 100) {
        player.boost = Math.min(100, player.boost + 0.2);
      }

      player.speed = THREE.MathUtils.clamp(player.speed + accel - player.speed * 0.02, -0.5, currentMaxSpeed);

      player.pos.x += Math.sin(player.heading) * player.speed;
      player.pos.z += Math.cos(player.heading) * player.speed;
      playerCraft.position.copy(player.pos);
      playerCraft.rotation.y = player.heading;
      playerCraft.position.y = Math.sin(Date.now() * 0.005) * 0.3;

      sound.updateEngine(Math.abs(player.speed) / player.maxSpeed);

      // EMP Wave Scale Expansion
      if (empMesh.visible) {
        empMesh.scale.addScalar(1.5);
        empMesh.material.opacity -= 0.025;
        if (empMesh.material.opacity <= 0) {
          empMesh.visible = false;
          empMesh.material.opacity = 0.8;
        }
      }

      // AI Bots Pathing
      bots.forEach(bot => {
        if (!bot.alive) return;

        if (gameConfig.mode === 'elimination') {
          // In Derby Mode, AI wanders towards the player
          const dirToPlayer = new THREE.Vector3().subVectors(player.pos, bot.mesh.position).normalize();
          bot.mesh.position.addScaledVector(dirToPlayer, bot.speed * 80);
          bot.mesh.lookAt(player.pos);
        } else {
          // Standard Track Following
          bot.t = (bot.t + bot.speed) % 1;
          const pt = trackCurve.getPoint(bot.t);
          bot.mesh.position.copy(pt);
          bot.mesh.lookAt(trackCurve.getPoint((bot.t + 0.01) % 1));
        }
        bot.mesh.position.y = Math.sin(Date.now() * 0.005 + bot.t * 10) * 0.3;
      });

      // Calculate Leaderboards / Mode Progress
      let closestU = 0, minDst = Infinity;
      for (let u = 0; u < 1; u += 0.01) {
        const d = trackCurve.getPoint(u).distanceTo(player.pos);
        if (d < minDst) { minDst = d; closestU = u; }
      }
      player.progress = player.lap + closestU;

      const activeBots = bots.filter(b => b.alive);

      if (gameConfig.mode === 'race') {
        const standings = [
          { name: 'YOU (PLAYER)', progress: player.progress, isPlayer: true },
          ...activeBots.map(b => ({ name: b.name, progress: b.lap + b.t, isPlayer: false }))
        ].sort((a, b) => b.progress - a.progress);

        const rank = standings.findIndex(s => s.isPlayer) + 1;
        document.getElementById('pos-val').innerText = `${rank}/${activeBots.length + 1}`;
        document.getElementById('main-stat-val').innerText = `LAP ${Math.floor(player.progress)}/3`;

        document.getElementById('lb-rows').innerHTML = standings.slice(0, 5).map((s, idx) => `
          <div class="lb-row ${s.isPlayer ? 'player' : ''}">
            <span>${idx + 1}. ${s.name}</span>
            <span>LAP ${Math.floor(s.progress)}</span>
          </div>
        `).join('');

        if (player.progress >= 4) {
          triggerGameOver(true, `Race Finished! Final Position: ${rank}`);
        }

      } else if (gameConfig.mode === 'elimination') {
        document.getElementById('pos-val').innerText = `${player.kills}/${gameConfig.numOpponents}`;
        document.getElementById('main-stat-val').innerText = `ALIVE: ${activeBots.length}`;

        document.getElementById('lb-rows').innerHTML = bots.map(b => `
          <div class="lb-row ${b.alive ? '' : 'player'}">
            <span>${b.name}</span>
            <span>${b.alive ? `HP ${b.hp}` : 'DESTROYED'}</span>
          </div>
        `).join('');

        if (activeBots.length === 0) {
          triggerGameOver(true, `All opponents eliminated! Total kills: ${player.kills}`);
        }

      } else if (gameConfig.mode === 'freeroam') {
        document.getElementById('main-stat-val').innerText = `OPEN WORLD`;
        document.getElementById('lb-rows').innerHTML = `<p style="opacity: 0.6;">Explore the neon horizon, test nitro boosts, and perform free stunts.</p>`;
      }

      document.getElementById('speed-val').innerText = Math.round(Math.abs(player.speed) * 120);
      document.getElementById('boost-bar').style.width = player.boost + '%';

      const elapsed = (Date.now() - player.startTime) / 1000;
      document.getElementById('sub-stat-val').innerText = `${Math.floor(elapsed / 60).toString().padStart(2, '0')}:${(elapsed % 60).toFixed(2).padStart(5, '0')}`;

      // Camera Offset
      if (gameConfig.cameraMode === 0) {
        camera.position.x = player.pos.x - Math.sin(player.heading) * 18;
        camera.position.z = player.pos.z - Math.cos(player.heading) * 18;
        camera.position.y = player.pos.y + 7;
        camera.lookAt(player.pos.x, player.pos.y + 2, player.pos.z);
      } else if (gameConfig.cameraMode === 1) {
        camera.position.set(player.pos.x, player.pos.y + 1, player.pos.z);
        camera.lookAt(player.pos.x + Math.sin(player.heading) * 20, player.pos.y + 1, player.pos.z + Math.cos(player.heading) * 20);
      } else if (gameConfig.cameraMode === 2) {
        camera.position.set(player.pos.x, player.pos.y + 60, player.pos.z + 0.1);
        camera.lookAt(player.pos);
      }

      drawRadar();
      renderer.render(scene, camera);
    }

    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });

    animate();
  </script>
</body>
</html>
