<!DOCTYPE html>
<html lang="hu">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ombre- Rablás Minigames</title>
  <style>
    :root {
      --bg1: #07101d;
      --bg2: #03060c;
      --text: #eef6ff;
      --muted: #97a9bf;
      --danger: #ff4a4a;
      --success: #35e06a;
      --purple: #b14dff;
      --purple-dark: #6f2bd6;
    }

    * { box-sizing: border-box; }
    html, body { margin: 0; min-height: 100%; }

    body {
      min-height: 100vh;
      display: grid;
      place-items: center;
      background:
        radial-gradient(circle at top, #10213f 0%, var(--bg1) 34%, var(--bg2) 100%);
      color: var(--text);
      font-family: Arial, Helvetica, sans-serif;
      overflow: hidden;
    }

    .wrap {
      width: min(1120px, calc(100vw - 28px));
      min-height: 680px;
      position: relative;
      border-radius: 24px;
      padding: 26px;
      background:
        linear-gradient(180deg, rgba(10, 16, 28, 0.84), rgba(7, 12, 22, 0.78)),
        radial-gradient(circle at top, rgba(56,189,248,0.08), transparent 28%);
      border: 1px solid rgba(255,255,255,0.07);
      box-shadow:
        inset 0 1px 0 rgba(255,255,255,0.05),
        inset 0 -20px 60px rgba(0,0,0,0.18),
        0 24px 100px rgba(0,0,0,0.42);
      overflow: hidden;
    }

    .wrap::before {
      content: '';
      position: absolute;
      inset: 0;
      pointer-events: none;
      background:
        radial-gradient(circle at 20% 0%, rgba(56,189,248,0.06), transparent 24%),
        radial-gradient(circle at 80% 100%, rgba(34,197,94,0.05), transparent 24%);
    }

    .hud-top {
      position: relative;
      z-index: 2;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 14px;
      flex-wrap: wrap;
      margin-bottom: 10px;
    }

    .title { font-size: 24px; font-weight: 900; letter-spacing: 0.2px; }
    .sub { color: var(--muted); font-size: 14px; margin-top: 4px; }

    .controls {
      display: flex;
      align-items: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    button {
      border: 1px solid rgba(255,255,255,0.08);
      border-radius: 14px;
      padding: 12px 18px;
      color: white;
      font-size: 15px;
      font-weight: 800;
      cursor: pointer;
      transition: transform 0.18s ease, filter 0.18s ease, box-shadow 0.18s ease, border-color 0.18s ease;
      backdrop-filter: blur(8px);
    }

    .mode-btn {
      background: linear-gradient(180deg, rgba(43,58,79,0.96), rgba(20,28,40,0.96));
      box-shadow: 0 10px 24px rgba(0,0,0,0.18), inset 0 1px 0 rgba(255,255,255,0.08);
      color: #dbeafe;
    }

    .mode-btn.active {
      background: linear-gradient(180deg, rgba(56,189,248,0.98), rgba(2,132,199,0.98));
      box-shadow: 0 12px 28px rgba(56,189,248,0.24), inset 0 1px 0 rgba(255,255,255,0.2);
      border-color: rgba(186,230,253,0.28);
      color: white;
    }

    .primary-btn {
      background: linear-gradient(180deg, rgba(34,197,94,0.98), rgba(22,155,73,0.98));
      box-shadow: 0 10px 24px rgba(34,197,94,0.22), inset 0 1px 0 rgba(255,255,255,0.16);
    }

    button:hover {
      filter: brightness(1.06);
      transform: translateY(-1px);
    }

    button:active { transform: translateY(1px) scale(0.99); }
    button:disabled { opacity: 0.7; cursor: not-allowed; }

    .status {
      padding: 10px 14px;
      border-radius: 12px;
      background: rgba(255,255,255,0.04);
      border: 1px solid rgba(255,255,255,0.06);
      font-size: 14px;
      font-weight: 800;
      color: #dbe4ee;
      min-width: 170px;
      text-align: center;
    }

    .fps-box {
      display:flex;
      align-items:center;
      gap:6px;
      font-size:13px;
      color:#cbd5e1;
      padding: 8px 10px;
      border-radius: 10px;
      background: rgba(255,255,255,0.04);
      border:1px solid rgba(255,255,255,0.06);
    }

    .fps-box input {
      width:60px;
      padding:6px 6px;
      border-radius:6px;
      border:1px solid #334155;
      background:#0b1220;
      color:white;
      font-weight:600;
    }

    .hero-logo-wrap {
      display: flex;
      justify-content: center;
      margin: 10px 0 18px;
      pointer-events: none;
      position: relative;
      z-index: 1;
    }

    .hero-logo {
      width: min(240px, 46vw);
      max-height: 150px;
      object-fit: contain;
      display: block;
      filter: drop-shadow(0 14px 30px rgba(0,0,0,0.42));
      user-select: none;
      -webkit-user-drag: none;
    }

    .game-stage {
      position: relative;
      height: 510px;
      margin-top: 6px;
      border-radius: 26px;
      background:
        radial-gradient(circle at 50% 12%, rgba(53, 224, 106, 0.08), transparent 22%),
        radial-gradient(circle at 50% 78%, rgba(255, 74, 74, 0.05), transparent 28%),
        linear-gradient(180deg, rgba(255,255,255,0.035), rgba(255,255,255,0.01));
      box-shadow:
        inset 0 1px 0 rgba(255,255,255,0.05),
        inset 0 -24px 60px rgba(0,0,0,0.18),
        0 24px 80px rgba(0,0,0,0.35);
      overflow: hidden;
      border: 1px solid rgba(255,255,255,0.05);
    }

    .game-view {
      position: absolute;
      inset: 0;
      opacity: 1;
      transform: translateY(0) scale(1);
      transition: opacity 0.22s ease, transform 0.22s ease;
    }

    .game-view.hidden {
      opacity: 0;
      transform: translateY(10px) scale(0.985);
      pointer-events: none;
      visibility: hidden;
    }

    .countdown {
      position: absolute;
      left: 50%;
      top: 108px;
      transform: translateX(-50%);
      font-size: 72px;
      font-weight: 900;
      color: white;
      text-shadow: 0 0 18px rgba(255,255,255,0.25), 0 6px 20px rgba(0,0,0,0.55);
      user-select: none;
      z-index: 5;
    }

    .countdown.start { font-size: 60px; }

    .emoji-row {
      position: absolute;
      left: 50%;
      bottom: 114px;
      transform: translateX(-50%);
      width: min(820px, 92%);
      pointer-events: none;
    }

    .track-wrap {
      position: absolute;
      left: 50%;
      bottom: 68px;
      transform: translateX(-50%);
      width: min(820px, 92%);
      height: 20px;
      pointer-events: none;
    }

    .track {
      position: absolute;
      inset: 0;
      background: linear-gradient(180deg, rgba(10,10,10,0.98), rgba(20,20,20,0.98));
      border: 2px solid rgba(75,75,75,0.9);
      box-shadow: inset 0 2px 0 rgba(255,255,255,0.04), inset 0 -2px 0 rgba(0,0,0,0.3), 0 10px 30px rgba(0,0,0,0.35);
      overflow: hidden;
      border-radius: 6px;
    }

    .danger-zone, .safe-zone { position: absolute; top: 0; bottom: 0; opacity: 0.16; }
    .danger-zone.left { left: 0; width: 34%; background: var(--danger); }
    .safe-zone { left: 44%; width: 12%; background: var(--success); }
    .danger-zone.right { right: 0; width: 34%; background: var(--danger); }

    .marker {
      position: absolute;
      top: -2px;
      width: 6px;
      height: calc(100% + 4px);
      transform: translateX(-50%);
      background: linear-gradient(180deg, #ff8b8b, #ff4a4a);
      box-shadow: 0 0 8px rgba(255,98,98,0.5), 0 0 18px rgba(255,98,98,0.22);
      z-index: 2;
      border-radius: 4px;
    }

    .faces { position: relative; width: 100%; height: 82px; }
    .face {
      position: absolute;
      width: 52px;
      height: 52px;
      display: grid;
      place-items: center;
      user-select: none;
      filter: drop-shadow(0 4px 10px rgba(0,0,0,0.45));
    }

    .face.left  { left: -6px; bottom: 0; }
    .face.mid   { left: 50%; transform: translateX(-50%); bottom: 0; }
    .face.right { right: -6px; bottom: 0; }

    .angry-base, .smile-base {
      position: absolute;
      inset: 0;
      display: grid;
      place-items: center;
      opacity: 0.95;
    }

    .angry-fill, .smile-fill {
      position: absolute;
      inset: 0;
      overflow: hidden;
      display: grid;
      place-items: center;
    }

    .angry-fill.left-fill, .angry-fill.right-fill { clip-path: inset(100% 0 0 0); }
    .smile-fill { clip-path: inset(100% 0 0 0); }

    .smile-image, .smile-image-base {
      width: 52px;
      height: 52px;
      object-fit: contain;
      image-rendering: auto;
      user-select: none;
      -webkit-user-drag: none;
      display: block;
    }

    .smile-image-base {
      filter: grayscale(1) brightness(0.7);
      opacity: 0.95;
    }

    .smile-image { opacity: 1; filter: none; }

    .cash-stage {
      position: absolute;
      inset: 0;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .cash-wrap {
      position: relative;
      width: min(760px, 78vw);
      aspect-ratio: 1.8 / 1;
    }

    .cash-box {
      position: absolute;
      inset: 12% 10%;
      border-radius: 12px;
      overflow: visible;
    }

    .cash-box-base {
      position: absolute;
      inset: 16% 12%;
      border-radius: 12px;
      background: linear-gradient(180deg, rgba(108,78,168,0.95), rgba(67,49,119,0.95));
      border: 3px solid rgba(86,57,146,0.92);
      box-shadow: inset 0 2px 0 rgba(255,255,255,0.08), inset 0 -10px 20px rgba(0,0,0,0.18), 0 20px 40px rgba(0,0,0,0.22);
    }

    .cash-box-lid {
      position: absolute;
      inset: 0;
      border-radius: 12px;
      background: linear-gradient(180deg, rgba(214,189,255,0.98), rgba(160,112,217,0.94)), linear-gradient(135deg, rgba(255,255,255,0.25), rgba(0,0,0,0.08));
      border: 3px solid rgba(142,92,226,0.84);
      box-shadow: inset 0 0 0 2px rgba(255,255,255,0.14), inset 0 10px 30px rgba(255,255,255,0.12), inset 0 -18px 40px rgba(0,0,0,0.14), 0 24px 50px rgba(0,0,0,0.22);
      transition: transform 0.18s ease, box-shadow 0.18s ease;
      will-change: transform;
    }

    .cash-box-lid::before {
      content: '';
      position: absolute;
      inset: 10% 7%;
      border: 2px solid rgba(120,78,199,0.78);
      border-radius: 8px;
      box-shadow: inset 0 0 18px rgba(0,0,0,0.08);
    }

    .cash-box-lid::after {
      content: '';
      position: absolute;
      right: 14%;
      top: 35%;
      width: 11%;
      height: 18%;
      border-radius: 6px;
      background: rgba(243,232,255,0.85);
      border: 2px solid rgba(255,255,255,0.45);
      box-shadow: 0 0 14px rgba(255,255,255,0.18);
    }

    .cash-lip {
      position:absolute; left:8%; right:8%; height: 7%; border-radius: 6px;
      background: linear-gradient(180deg, rgba(226,206,255,0.96), rgba(151,109,217,0.95));
      border: 2px solid rgba(130,84,205,0.78);
      box-shadow: inset 0 2px 0 rgba(255,255,255,0.2), 0 8px 18px rgba(0,0,0,0.14);
      transition: opacity 0.18s ease;
    }

    .cash-lip.top { top: 7%; }
    .cash-lip.bottom { bottom: 7%; }

    .cash-corner {
      position: absolute;
      width: 96px;
      height: 96px;
      transform: translate(-50%, -50%);
      pointer-events: none;
    }

    .cash-corner.tl { left: 11%; top: 14%; }
    .cash-corner.tr { left: 89%; top: 14%; }
    .cash-corner.bl { left: 11%; top: 86%; }
    .cash-corner.br { left: 89%; top: 86%; }

    .ring-outer {
      position: absolute;
      inset: 0;
      opacity: 0;
      transition: opacity 0.12s ease;
    }

    .ring-timer {
      position: absolute;
      inset: 0;
      border-radius: 50%;
      transform: scale(1);
      transform-origin: center;
      background: radial-gradient(circle at 30% 30%, #39f0ff, #14c7f0 68%, #0a8dc2 100%);
      box-shadow: 0 0 22px rgba(34,229,255,0.34), inset 0 3px 0 rgba(255,255,255,0.24);
      transition: background 0.12s ease, box-shadow 0.12s ease;
      z-index: 1;
    }

    .ring-key {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      width: 34px;
      height: 34px;
      border-radius: 50%;
      background: white;
      color: #1f2937;
      font-weight: 900;
      font-size: 18px;
      display: grid;
      place-items: center;
      box-shadow: 0 2px 8px rgba(0,0,0,0.22);
      z-index: 2;
      user-select: none;
    }

    .cash-progress {
      position:absolute;
      left:50%;
      transform: translateX(-50%);
      bottom: 12px;
      color:#d7f4ff;
      font-weight:800;
      font-size:18px;
      text-shadow: 0 4px 10px rgba(0,0,0,0.35);
      letter-spacing: 0.5px;
    }

    .explosion {
      position:absolute;
      inset:0;
      display:grid;
      place-items:center;
      pointer-events:none;
      background: radial-gradient(circle, rgba(255,92,92,0.2), transparent 45%);
      opacity:0;
      transition: opacity 0.15s ease, transform 0.18s ease, filter 0.18s ease;
      font-size: 72px;
      font-weight: 900;
      color: #ffd1d1;
      text-shadow: 0 10px 30px rgba(255,0,0,0.25);
      transform: scale(0.88);
      filter: blur(2px);
    }

    .explosion.show {
      opacity:1;
      transform: scale(1.08);
      filter: blur(0px);
      animation: boomPulse 0.42s ease-out;
    }

    @keyframes boomPulse {
      0% { transform: scale(0.75); filter: blur(6px); opacity: 0; }
      35% { transform: scale(1.16); filter: blur(0px); opacity: 1; }
      100% { transform: scale(1.02); filter: blur(0px); opacity: 1; }
    }

    .paint-splatter {
      position: fixed;
      inset: 0;
      pointer-events: none;
      opacity: 0;
      z-index: 9999;
      overflow: hidden;
      background: radial-gradient(circle at center, rgba(72,22,120,0.10), rgba(10,4,18,0.0) 55%);
      transition: opacity 0.18s ease;
    }

    .paint-splatter.show {
      opacity: 1;
      animation: screenFlash 0.28s ease-out;
    }

    @keyframes screenFlash {
      0% { background-color: rgba(255,255,255,0.0); }
      25% { background-color: rgba(214,174,255,0.12); }
      100% { background-color: rgba(255,255,255,0.0); }
    }

    .paint-cloud {
      position: absolute;
      border-radius: 50%;
      background: radial-gradient(circle at 35% 35%, rgba(236,213,255,0.95), rgba(177,77,255,0.88) 42%, rgba(111,43,214,0.72) 70%, rgba(111,43,214,0.02) 100%);
      filter: blur(10px);
      transform: scale(0.2);
      opacity: 0;
      animation: smokeBurst 1.35s ease-out forwards;
      box-shadow: 0 0 60px rgba(177,77,255,0.18);
    }

    .paint-cloud.c1 { left: -8%; top: 8%; width: 40vw; height: 36vw; animation-delay: 0.02s; }
    .paint-cloud.c2 { left: 6%; top: -12%; width: 30vw; height: 28vw; animation-delay: 0.08s; }
    .paint-cloud.c3 { right: -10%; top: 12%; width: 38vw; height: 34vw; animation-delay: 0.04s; }
    .paint-cloud.c4 { right: 8%; top: -8%; width: 26vw; height: 24vw; animation-delay: 0.12s; }
    .paint-cloud.c5 { left: 14%; bottom: -18%; width: 34vw; height: 30vw; animation-delay: 0.16s; }
    .paint-cloud.c6 { right: 14%; bottom: -20%; width: 36vw; height: 32vw; animation-delay: 0.1s; }
    .paint-cloud.c7 { left: 34%; top: 20%; width: 20vw; height: 20vw; animation-delay: 0.22s; }
    .paint-cloud.c8 { right: 26%; top: 28%; width: 22vw; height: 22vw; animation-delay: 0.18s; }
    .paint-cloud.c9 { left: 42%; bottom: -12%; width: 24vw; height: 22vw; animation-delay: 0.24s; }

    .paint-fog {
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at center, rgba(177,77,255,0.06), rgba(111,43,214,0.14) 45%, rgba(18,8,28,0.52) 100%);
      opacity: 0;
      animation: fogFade 1.55s ease-out forwards;
    }

    @keyframes smokeBurst {
      0% {
        transform: scale(0.18);
        opacity: 0;
      }
      16% {
        opacity: 0.96;
      }
      55% {
        transform: scale(1.08);
        opacity: 0.74;
      }
      100% {
        transform: scale(1.46);
        opacity: 0.22;
      }
    }

    @keyframes fogFade {
      0% { opacity: 0; }
      18% { opacity: 1; }
      100% { opacity: 0.4; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <audio id="successSound" src="./success.wav" preload="auto"></audio>
    <audio id="failSound" src="./fail.wav" preload="auto"></audio>
    <audio id="explosionSound" src="./explosion.mp3" preload="auto"></audio>
    <audio id="move1Sound" src="./move1.mp3" preload="auto"></audio>
    <audio id="move2Sound" src="./move2.mp3" preload="auto"></audio>
    <audio id="move3Sound" src="./move3.mp3" preload="auto"></audio>
    <audio id="move4Sound" src="./move4.mp3" preload="auto"></audio>

    <div class="hud-top">
      <div>
        <div class="title">Fenyegetés / Kazetta Minigame</div>
        <div class="sub">by liladam</div>
      </div>

      <div class="controls">
        <button id="switchFearBtn" class="mode-btn active">Fenyegetés</button>
        <button id="switchCashBtn" class="mode-btn">Kazetta</button>
        <label class="fps-box">
          FPS
          <input id="fpsInput" type="number" min="1" max="75" value="75">
        </label>
        <button id="startBtn" class="primary-btn">Indítás</button>
        <div class="status" id="statusText">Állapot: Fenyegetés mód</div>
      </div>
    </div>

    <div class="hero-logo-wrap">
      <img src="./v4.png" alt="v4 icon" class="hero-logo">
    </div>

    <div class="game-stage">
      <div class="paint-splatter" id="paintSplatter">
        <div class="paint-fog"></div>
        <div class="paint-cloud c1"></div>
        <div class="paint-cloud c2"></div>
        <div class="paint-cloud c3"></div>
        <div class="paint-cloud c4"></div>
        <div class="paint-cloud c5"></div>
        <div class="paint-cloud c6"></div>
        <div class="paint-cloud c7"></div>
        <div class="paint-cloud c8"></div>
        <div class="paint-cloud c9"></div>
      </div>
      <div id="fearView" class="game-view">
        <div class="countdown" id="timerText">5</div>

        <div class="emoji-row">
          <div class="faces">
            <div class="face left">
              <div class="angry-base"><img src="./smile.png" class="smile-image-base" alt="left base"></div>
              <div class="angry-fill left-fill" id="leftAngryFill"><img src="./smile.png" class="smile-image" alt="left fill"></div>
            </div>

            <div class="face mid">
              <div class="smile-base"><img src="./smile.png" alt="smile" class="smile-image-base"></div>
              <div class="smile-fill" id="smileFill"><img src="./smile.png" alt="smile" class="smile-image"></div>
            </div>

            <div class="face right">
              <div class="angry-base"><img src="./smile.png" class="smile-image-base" alt="right base"></div>
              <div class="angry-fill right-fill" id="rightAngryFill"><img src="./smile.png" class="smile-image" alt="right fill"></div>
            </div>
          </div>
        </div>

        <div class="track-wrap">
          <div class="track">
            <div class="danger-zone left"></div>
            <div class="safe-zone"></div>
            <div class="danger-zone right"></div>
            <div class="marker" id="marker"></div>
          </div>
        </div>
      </div>

      <div id="cashView" class="game-view hidden">
        <div class="countdown" id="cashTimerText">5</div>
        <div class="cash-stage">
          <div class="cash-wrap">
            <div class="cash-box">
              <div class="cash-box-base"></div>
              <div class="cash-box-lid" id="cashBoxLid">
                <div class="cash-lip top" id="cashLipTop"></div>
                <div class="cash-lip bottom"></div>
              </div>
            </div>

            <div class="cash-corner tl" id="corner-tl"><div class="ring-outer"><div class="ring-timer"></div><div class="ring-key">Q</div></div></div>
            <div class="cash-corner tr" id="corner-tr"><div class="ring-outer"><div class="ring-timer"></div><div class="ring-key">A</div></div></div>
            <div class="cash-corner bl" id="corner-bl"><div class="ring-outer"><div class="ring-timer"></div><div class="ring-key">Z</div></div></div>
            <div class="cash-corner br" id="corner-br"><div class="ring-outer"><div class="ring-timer"></div><div class="ring-key">X</div></div></div>

            <div class="cash-progress" id="cashProgressText">0 / 11</div>
            <div class="explosion" id="explosionText">IDIOTA</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    const startBtn = document.getElementById('startBtn');
    const switchFearBtn = document.getElementById('switchFearBtn');
    const switchCashBtn = document.getElementById('switchCashBtn');
    const statusText = document.getElementById('statusText');
    const timerText = document.getElementById('timerText');
    const cashTimerText = document.getElementById('cashTimerText');
    const marker = document.getElementById('marker');
    const successSound = document.getElementById('successSound');
    const failSound = document.getElementById('failSound');
    const explosionSound = document.getElementById('explosionSound');
    const move1Sound = document.getElementById('move1Sound');
    const move2Sound = document.getElementById('move2Sound');
    const move3Sound = document.getElementById('move3Sound');
    const move4Sound = document.getElementById('move4Sound');
    const smileFill = document.getElementById('smileFill');
    const leftAngryFill = document.getElementById('leftAngryFill');
    const rightAngryFill = document.getElementById('rightAngryFill');
    const fearView = document.getElementById('fearView');
    const cashView = document.getElementById('cashView');
    const cashProgressText = document.getElementById('cashProgressText');
    const explosionText = document.getElementById('explosionText');
    const fpsInput = document.getElementById('fpsInput');
    const cashLipTop = document.getElementById('cashLipTop');
    const cashBoxLid = document.getElementById('cashBoxLid');
    const paintSplatter = document.getElementById('paintSplatter');

    const allowedKeys = [
      { key: 'Q', code: 'KeyQ' },
      { key: 'W', code: 'KeyW' },
      { key: 'E', code: 'KeyE' },
      { key: 'A', code: 'KeyA' },
      { key: 'S', code: 'KeyS' },
      { key: 'D', code: 'KeyD' },
      { key: 'F', code: 'KeyF' },
      { key: 'X', code: 'KeyX' },
      { key: 'C', code: 'KeyC' },
    ];

    const cashCorners = [
      { id: 'corner-tl', el: document.querySelector('#corner-tl .ring-outer'), timer: document.querySelector('#corner-tl .ring-timer'), keyEl: document.querySelector('#corner-tl .ring-key') },
      { id: 'corner-tr', el: document.querySelector('#corner-tr .ring-outer'), timer: document.querySelector('#corner-tr .ring-timer'), keyEl: document.querySelector('#corner-tr .ring-key') },
      { id: 'corner-bl', el: document.querySelector('#corner-bl .ring-outer'), timer: document.querySelector('#corner-bl .ring-timer'), keyEl: document.querySelector('#corner-bl .ring-key') },
      { id: 'corner-br', el: document.querySelector('#corner-br .ring-outer'), timer: document.querySelector('#corner-br .ring-timer'), keyEl: document.querySelector('#corner-br .ring-key') }
    ];

    let FPS_SIMULATION = 75;
    let FRAME_TIME = 1000 / 35;
    let currentMode = 'fear';

    function clamp(value, min, max) { return Math.max(min, Math.min(max, value)); }
    function rand(min, max) { return Math.random() * (max - min) + min; }

    function updateFPSSimulation(val) {
      const num = Math.max(1, Math.min(75, Number(val) || 75));
      FPS_SIMULATION = num;
      FRAME_TIME = 1000 / FPS_SIMULATION;
      fpsInput.value = String(FPS_SIMULATION);
    }

    function playSound(win) {
      if (currentMode !== 'fear') return;
      const sound = win ? successSound : failSound;
      if (!sound) return;
      try {
        sound.currentTime = 0;
        sound.play().catch(() => {});
      } catch (e) {}
    }

    function playExplosionSound() {
      if (!explosionSound) return;
      try {
        explosionSound.currentTime = 0;
        explosionSound.play().catch(() => {});
      } catch (e) {}
    }

    function playRandomMoveSound() {
      const sounds = [move1Sound, move2Sound, move3Sound, move4Sound].filter(Boolean);
      if (!sounds.length) return;
      const sound = sounds[Math.floor(Math.random() * sounds.length)];
      try {
        sound.currentTime = 0;
        sound.play().catch(() => {});
      } catch (e) {}
    }

    const fearState = {
      running: false,
      countdownActive: false,
      pressingSpace: false,
      cursor: 0.5,
      progress: 0,
      lastTime: 0,
      frameAccumulator: 0,
      upSpeed: 0.7,
      nextUpSpeed: 1.05,
      downSpeed: 0.52,
      nextDownSpeed: 0.9,
      nextUpStart: 0,
      nextDownStart: 0,
      nextUpTime: 180,
      nextDownTime: 220,
      spikeUntil: 0,
      spikeStrength: 0,
      spikeCooldownUntil: 0,
    };

    const cashState = {
      running: false,
      countdownActive: false,
      total: 11,
      sequenceDone: 0,
      currentCorner: null,
      currentWindow: 1950,
      elapsed: 0,
      frameAccumulator: 0,
      exploded: false,
      lastCornerId: null,
      lidAnimUntil: 0,
    };

    function stopAllGames() {
      fearState.running = false;
      fearState.countdownActive = false;
      fearState.pressingSpace = false;
      cashState.running = false;
      cashState.countdownActive = false;
      startBtn.disabled = false;
      explosionText.classList.remove('show');
    }

    function switchMode(mode) {
      stopAllGames();
      paintSplatter.classList.remove('show');
      currentMode = mode;
      fearView.classList.toggle('hidden', mode !== 'fear');
      cashView.classList.toggle('hidden', mode !== 'cash');
      switchFearBtn.classList.toggle('active', mode === 'fear');
      switchCashBtn.classList.toggle('active', mode === 'cash');

      if (mode === 'fear') {
        resetFearGameState();
        timerText.textContent = '5';
        statusText.textContent = 'Állapot: Fenyegetés mód';
      } else {
        resetCashGame();
        cashTimerText.textContent = '5';
        statusText.textContent = 'Állapot: Kazetta mód';
      }
    }

    function resetFearGameState() {
      fearState.running = false;
      fearState.countdownActive = false;
      fearState.pressingSpace = false;
      fearState.cursor = 0.5;
      fearState.progress = 0;
      fearState.lastTime = 0;
      fearState.frameAccumulator = 0;
      fearState.upSpeed = 0.7;
      fearState.nextUpSpeed = 1.05;
      fearState.downSpeed = 0.52;
      fearState.nextDownSpeed = 0.9;
      fearState.nextUpStart = performance.now();
      fearState.nextDownStart = performance.now();
      fearState.nextUpTime = 180;
      fearState.nextDownTime = 220;
      fearState.spikeUntil = 0;
      fearState.spikeStrength = 0;
      fearState.spikeCooldownUntil = 0;
      updateFearUI();
    }

    function updateFearUI() {
      marker.style.left = `${fearState.cursor * 100}%`;
      const success = Math.max(0, fearState.progress);
      const fail = Math.max(0, -fearState.progress);
      smileFill.style.clipPath = `inset(${100 - success * 100}% 0 0 0)`;

      if (fearState.cursor < 0.5) {
        leftAngryFill.style.clipPath = `inset(${100 - fail * 100}% 0 0 0)`;
        rightAngryFill.style.clipPath = 'inset(100% 0 0 0)';
      } else if (fearState.cursor > 0.5) {
        rightAngryFill.style.clipPath = `inset(${100 - fail * 100}% 0 0 0)`;
        leftAngryFill.style.clipPath = 'inset(100% 0 0 0)';
      } else {
        leftAngryFill.style.clipPath = 'inset(100% 0 0 0)';
        rightAngryFill.style.clipPath = 'inset(100% 0 0 0)';
      }
    }

    function finishFearGame(win) {
      fearState.running = false;
      fearState.countdownActive = false;
      startBtn.disabled = false;
      statusText.textContent = win ? 'Állapot: Siker!' : 'Állapot: Elrontottad';
      timerText.classList.remove('start');
      timerText.textContent = win ? 'SIKER' : 'GATYA';
      playSound(win);
    }

    function updateFearSpeeds(now) {
      if (now - fearState.nextUpStart >= fearState.nextUpTime) {
        fearState.nextUpStart = now;
        fearState.nextUpTime = rand(50, 500);
        fearState.upSpeed = fearState.nextUpSpeed;
        fearState.nextUpSpeed = rand(0.45, 1.45);
      }
      if (now - fearState.nextDownStart >= fearState.nextDownTime) {
        fearState.nextDownStart = now;
        fearState.nextDownTime = rand(50, 500);
        fearState.downSpeed = fearState.nextDownSpeed;
        fearState.nextDownSpeed = rand(0.3, 1.05);
      }
    }

    function updateFearSpike(now) {
      if (!fearState.running) return;
      if (now >= fearState.spikeCooldownUntil && now >= fearState.spikeUntil) {
        if (Math.random() < 0.012) {
          fearState.spikeUntil = now + rand(180, 420);
          fearState.spikeStrength = rand(0.2, 0.55);
          fearState.spikeCooldownUntil = fearState.spikeUntil + rand(350, 900);
        }
      }
      if (now >= fearState.spikeUntil) fearState.spikeStrength = 0;
    }

    function stepFearGame(simulatedDelta, now) {
      if (!fearState.running) return;
      updateFearSpeeds(now);
      updateFearSpike(now);

      let speed = 0;
      if (fearState.pressingSpace) {
        const t = Math.min(1, (now - fearState.nextUpStart) / fearState.nextUpTime);
        speed = fearState.upSpeed + (fearState.nextUpSpeed - fearState.upSpeed) * t;
        const wave = Math.sin(now * 0.004) * 0.35 + Math.sin(now * 0.009) * 0.25;
        speed *= 1 + wave;
        speed *= 1 + fearState.spikeStrength;
        if (fearState.cursor > 0.55) speed *= 1.22;
      } else {
        const t = Math.min(1, (now - fearState.nextDownStart) / fearState.nextDownTime);
        speed = -(fearState.downSpeed + (fearState.nextDownSpeed - fearState.downSpeed) * t);
        const wave = Math.sin(now * 0.003) * 0.25;
        speed *= 1 + wave;
        if (fearState.cursor < 0.45) speed *= 1.1;
      }

      fearState.cursor = clamp(fearState.cursor + speed * (simulatedDelta / 1000), 0, 1);

      if (fearState.cursor <= 0.33) {
        fearState.progress -= ((0.33 - fearState.cursor) / 0.33) * 3.0 * (simulatedDelta / 1000);
      } else if (fearState.cursor >= 0.44 && fearState.cursor <= 0.56) {
        fearState.progress += (1 - Math.abs(fearState.cursor - 0.5) / 0.06) * 0.32 * (simulatedDelta / 1000);
      } else if (fearState.cursor >= 0.67) {
        fearState.progress -= ((fearState.cursor - 0.67) / 0.33) * 3.0 * (simulatedDelta / 1000);
      }

      fearState.progress = clamp(fearState.progress, -1, 1);
      updateFearUI();

      if (fearState.progress >= 1) finishFearGame(true);
      else if (fearState.progress <= -1) finishFearGame(false);
    }

    async function startFearCountdown() {
      resetFearGameState();
      startBtn.disabled = true;
      fearState.countdownActive = true;
      statusText.textContent = 'Állapot: Készülj...';

      for (let i = 5; i >= 1; i--) {
        timerText.classList.remove('start');
        timerText.textContent = String(i);
        await new Promise(resolve => setTimeout(resolve, 1000));
      }

      timerText.classList.add('start');
      timerText.textContent = 'START!';
      statusText.textContent = 'Állapot: Folyamatban';
      fearState.countdownActive = false;
      fearState.running = true;
      fearState.lastTime = 0;
      fearState.frameAccumulator = 0;

      setTimeout(() => {
        if (fearState.running) {
          timerText.classList.remove('start');
          timerText.textContent = '';
        }
      }, 700);
    }

    function resetCashGame() {
      cashState.running = false;
      cashState.countdownActive = false;
      cashState.sequenceDone = 0;
      cashState.currentCorner = null;
      cashState.currentWindow = 1950;
      cashState.elapsed = 0;
      cashState.frameAccumulator = 0;
      cashState.exploded = false;
      cashState.lastCornerId = null;
      cashState.lidAnimUntil = 0;
      cashProgressText.textContent = `0 / ${cashState.total}`;
      explosionText.classList.remove('show');
      paintSplatter.classList.remove('show');
      if (cashLipTop) cashLipTop.style.opacity = '1';
      if (cashBoxLid) {
        cashBoxLid.style.transform = 'translate(0px, 0px)';
        cashBoxLid.style.boxShadow = 'inset 0 0 0 2px rgba(255,255,255,0.14), inset 0 10px 30px rgba(255,255,255,0.15), inset 0 -18px 40px rgba(0,0,0,0.12), 0 24px 50px rgba(0,0,0,0.22)';
      }
      cashCorners.forEach(c => {
        c.el.style.opacity = '0';
        c.timer.style.transform = 'scale(1)';
        c.timer.style.background = 'radial-gradient(circle at 30% 30%, #39f0ff, #14c7f0 68%, #0a8dc2 100%)';
        c.timer.style.boxShadow = '0 0 22px rgba(34,229,255,0.34), inset 0 3px 0 rgba(255,255,255,0.24)';
      });
      cashTimerText.classList.remove('start');
    }

    function pickNextCorner() {
      let next = cashCorners[Math.floor(Math.random() * cashCorners.length)];
      if (cashCorners.length > 1 && cashState.lastCornerId) {
        let guard = 0;
        while (next.id === cashState.lastCornerId && guard < 20) {
          next = cashCorners[Math.floor(Math.random() * cashCorners.length)];
          guard += 1;
        }
      }

      const randomKey = allowedKeys[Math.floor(Math.random() * allowedKeys.length)];
      cashState.currentCorner = next;
      cashState.lastCornerId = next.id;
      cashState.currentCorner.code = randomKey.code;
      if (next.keyEl) next.keyEl.textContent = randomKey.key;
      cashState.elapsed = 0;
      cashState.currentWindow = Math.max(780, 1950 - cashState.sequenceDone * 85);

      cashCorners.forEach(c => {
        c.el.style.opacity = c === next ? '1' : '0';
        c.timer.style.transform = 'scale(1)';
      });

      next.timer.style.background = 'radial-gradient(circle at 30% 30%, #39f0ff, #14c7f0 68%, #0a8dc2 100%)';
      next.timer.style.boxShadow = '0 0 22px rgba(34,229,255,0.34), inset 0 3px 0 rgba(255,255,255,0.24)';
      cashProgressText.textContent = `${cashState.sequenceDone} / ${cashState.total}`;
    }

    function animateCashLid() {
      if (!cashBoxLid || !cashState.currentCorner) return;
      const dirMap = {
        'corner-tl': { x: -12, y: -8 },
        'corner-tr': { x: 12, y: -8 },
        'corner-bl': { x: -10, y: 8 },
        'corner-br': { x: 10, y: 8 },
      };
      const base = dirMap[cashState.currentCorner.id] || { x: 0, y: 0 };
      const jitterX = (Math.random() * 4) - 2;
      const jitterY = (Math.random() * 4) - 2;
      cashState.lidAnimUntil = performance.now() + 150;
      cashBoxLid.style.transform = `translate(${base.x + jitterX}px, ${base.y + jitterY}px)`;
      cashBoxLid.style.boxShadow = 'inset 0 0 0 2px rgba(255,255,255,0.14), inset 0 10px 30px rgba(255,255,255,0.15), inset 0 -18px 40px rgba(0,0,0,0.12), 0 30px 56px rgba(0,0,0,0.26)';
    }

    function explodeCash() {
      cashState.running = false;
      cashState.exploded = true;
      startBtn.disabled = false;
      explosionText.classList.remove('show');
      void explosionText.offsetWidth;
      explosionText.classList.add('show');
      paintSplatter.classList.remove('show');
      void paintSplatter.offsetWidth;
      paintSplatter.classList.add('show');
      playExplosionSound();
      cashCorners.forEach(c => c.el.style.opacity = '0');
      statusText.textContent = 'Állapot: A kazetta felrobbant';
      cashTimerText.textContent = 'ELBASZTAD!';
      playSound(false);
    }

    function finishCashGame() {
      cashState.running = false;
      startBtn.disabled = false;
      cashCorners.forEach(c => c.el.style.opacity = '0');
      cashTimerText.textContent = 'SIKER';
      statusText.textContent = 'Állapot: Kazetta kinyitva';
      cashProgressText.textContent = `${cashState.total} / ${cashState.total}`;
    }

    function onCashKey(code) {
      if (!cashState.running || !cashState.currentCorner) return;
      if (code !== cashState.currentCorner.code) {
        explodeCash();
        return;
      }
      animateCashLid();
      playRandomMoveSound();
      cashState.sequenceDone += 1;
      if (cashState.sequenceDone >= cashState.total) finishCashGame();
      else pickNextCorner();
    }

    function stepCashGame(simulatedDelta) {
      if (!cashState.running || !cashState.currentCorner) return;
      cashState.elapsed += simulatedDelta;
      const progress = clamp(cashState.elapsed / cashState.currentWindow, 0, 1);
      const scale = Math.max(0.18, 1 - progress * 0.82);
      cashState.currentCorner.timer.style.transform = `scale(${scale})`;

      if (progress > 0.72) {
        cashState.currentCorner.timer.style.background = 'radial-gradient(circle at 30% 30%, #ff8e8e, #ff4a4a 68%, #b91c1c 100%)';
        cashState.currentCorner.timer.style.boxShadow = '0 0 22px rgba(255,64,64,0.42), inset 0 3px 0 rgba(255,255,255,0.16)';
      } else {
        cashState.currentCorner.timer.style.background = 'radial-gradient(circle at 30% 30%, #39f0ff, #14c7f0 68%, #0a8dc2 100%)';
        cashState.currentCorner.timer.style.boxShadow = '0 0 22px rgba(34,229,255,0.34), inset 0 3px 0 rgba(255,255,255,0.24)';
      }

      if (cashBoxLid) {
        if (cashState.lidAnimUntil > 0 && performance.now() >= cashState.lidAnimUntil) {
          cashBoxLid.style.transform = 'translate(0px, 0px)';
          cashBoxLid.style.boxShadow = 'inset 0 0 0 2px rgba(255,255,255,0.14), inset 0 10px 30px rgba(255,255,255,0.15), inset 0 -18px 40px rgba(0,0,0,0.12), 0 24px 50px rgba(0,0,0,0.22)';
          cashState.lidAnimUntil = 0;
        }
      }

      if (progress >= 1) explodeCash();
    }

    async function startCashCountdown() {
      resetCashGame();
      startBtn.disabled = true;
      cashState.countdownActive = true;
      cashState.running = false;
      statusText.textContent = 'Állapot: Készülj...';

      for (let i = 5; i >= 1; i--) {
        cashTimerText.classList.remove('start');
        cashTimerText.textContent = String(i);
        await new Promise(resolve => setTimeout(resolve, 1000));
      }

      cashTimerText.classList.add('start');
      cashTimerText.textContent = 'START!';
      cashState.countdownActive = false;
      cashState.running = true;
      cashState.frameAccumulator = 0;
      pickNextCorner();
      statusText.textContent = 'Állapot: Kazetta folyamatban';

      setTimeout(() => {
        if (cashState.running) {
          cashTimerText.classList.remove('start');
          cashTimerText.textContent = '';
        }
      }, 700);
    }

    function gameLoop(now) {
      if (!fearState.lastTime) fearState.lastTime = now;
      const realDelta = now - fearState.lastTime;
      fearState.lastTime = now;

      if (currentMode === 'fear') {
        fearState.frameAccumulator += realDelta;
        fearState.frameAccumulator = Math.min(fearState.frameAccumulator, FRAME_TIME * 4);
        while (fearState.frameAccumulator >= FRAME_TIME) {
          stepFearGame(FRAME_TIME, now);
          fearState.frameAccumulator -= FRAME_TIME;
        }
      } else {
        cashState.frameAccumulator += realDelta;
        cashState.frameAccumulator = Math.min(cashState.frameAccumulator, FRAME_TIME * 4);
        while (cashState.frameAccumulator >= FRAME_TIME) {
          stepCashGame(FRAME_TIME);
          cashState.frameAccumulator -= FRAME_TIME;
        }
      }

      requestAnimationFrame(gameLoop);
    }

    switchFearBtn.addEventListener('click', () => switchMode('fear'));
    switchCashBtn.addEventListener('click', () => switchMode('cash'));

    startBtn.addEventListener('click', async () => {
      if (currentMode === 'fear') {
        if (fearState.running || fearState.countdownActive) return;
        await startFearCountdown();
      } else {
        if (cashState.running || cashState.countdownActive) return;
        await startCashCountdown();
      }
    });

    window.addEventListener('keydown', (e) => {
      if (currentMode === 'fear' && e.code === 'Space') {
        e.preventDefault();
        fearState.pressingSpace = true;
      }

      if (currentMode === 'cash') {
        const valid = allowedKeys.find(k => k.code === e.code);
        if (valid) {
          e.preventDefault();
          onCashKey(e.code);
        }
      }
    });

    window.addEventListener('keyup', (e) => {
      if (e.code === 'Space') {
        e.preventDefault();
        fearState.pressingSpace = false;
      }
    });

    fpsInput.addEventListener('change', () => updateFPSSimulation(fpsInput.value));
    fpsInput.addEventListener('keyup', (e) => {
      if (e.key === 'Enter') updateFPSSimulation(fpsInput.value);
    });

    updateFPSSimulation(75);
    resetFearGameState();
    resetCashGame();
    switchMode('fear');
    requestAnimationFrame(gameLoop);
  </script>
</body>
</html>
