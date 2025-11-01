Color-match-
Color matching game for children's.

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Color Match — Kids Game</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <style>
    body { font-family: system-ui, -apple-system, Roboto, sans-serif; margin:0; display:flex; flex-direction:column; align-items:center; justify-content:flex-start; min-height:100vh; background:#f8f9fb; }
    header { width:100%; padding:18px; text-align:center; background:linear-gradient(90deg,#ffefba,#ffffff); box-shadow:0 2px 6px rgba(0,0,0,0.06); }
    h1 { margin:0; font-size:20px; color:#333; }
    .score { margin-top:10px; font-weight:700; color:#555; }
    main { width:100%; max-width:540px; padding:20px; box-sizing:border-box; }
    .card { background:#fff; border-radius:14px; padding:18px; box-shadow:0 6px 18px rgba(2,6,23,0.06); text-align:center; }
    .prompt { font-size:28px; margin-bottom:12px; color:#222; }
    .sub { color:#666; margin-bottom:18px; }
    .targets { display:grid; grid-template-columns:repeat(2,1fr); gap:12px; margin-bottom:14px; }
    .color-btn { height:110px; border-radius:12px; border:4px solid rgba(255,255,255,0.4); cursor:pointer; transition:transform .12s ease, box-shadow .12s; display:flex; align-items:center; justify-content:center; font-weight:700; color:rgba(0,0,0,0.45); text-shadow:0 1px 0 rgba(255,255,255,0.2); box-shadow:0 6px 12px rgba(10,10,30,0.06); user-select:none; }
    .color-btn:active { transform:scale(.98); }
    .controls { display:flex; gap:8px; justify-content:center; margin-top:10px; }
    .btn { padding:8px 12px; border-radius:10px; border:none; background:#2b8cff; color:white; font-weight:700; cursor:pointer; }
    .small { background:#6c757d; }
    .message { margin-top:12px; font-size:16px; min-height:24px; }
    .correct { color:#0a7a2f; font-weight:800; }
    .wrong { color:#c92a2a; font-weight:800; }
  </style>
</head>
<body>
  <header>
    <h1>Color Match — Learn Colors with Joy</h1>
    <div class="score">Stars: <span id="stars">0</span></div>
  </header>
 <main>
    <div class="card" role="main" aria-live="polite">
      <div class="prompt" id="prompt">Tap the color:</div>
      <div class="sub" id="colorName" style="font-size:22px; font-weight:800">Red</div>
  <div class="targets" id="options"></div>

  <div class="controls">
    <button class="btn" id="nextBtn">Next</button>
    <button class="btn small" id="voiceToggle">Voice: On</button>
  </div>

  <div class="message" id="messageArea"></div>
</div>  <script>
    const COLORS = [
      {name: 'Red', hex: '#e53935'},
      {name: 'Blue', hex: '#1e88e5'},
      {name: 'Green', hex: '#43a047'},
      {name: 'Yellow', hex: '#fdd835'},
      {name: 'Orange', hex: '#fb8c00'},
      {name: 'Purple', hex: '#8e44ad'},
      {name: 'Pink', hex: '#ff6b9a'},
      {name: 'Brown', hex: '#8d6e63'},
      {name: 'Black', hex: '#222222'},
      {name: 'White', hex: '#ffffff'},
      {name: 'Gray', hex: '#9e9e9e'},
      {name: 'Cyan', hex: '#00bcd4'}
    ];

    const optionsEl = document.getElementById('options');
    const colorNameEl = document.getElementById('colorName');
    const messageArea = document.getElementById('messageArea');
    const starsEl = document.getElementById('stars');
    const nextBtn = document.getElementById('nextBtn');
    const voiceToggle = document.getElementById('voiceToggle');

    let target = null;
    let stars = 0;
    let voiceOn = true;

    function speak(text) {
      if (!voiceOn) return;
      if ('speechSynthesis' in window) {
        const u = new SpeechSynthesisUtterance(text);
        u.rate = 0.95;
        window.speechSynthesis.cancel();
        window.speechSynthesis.speak(u);
      }
    }

    function pickTarget() {
      const idx = Math.floor(Math.random() * COLORS.length);
      return COLORS[idx];
    }

    function shuffle(arr) {
      for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
      return arr;
    }

    function startRound() {
      messageArea.textContent = '';
      optionsEl.innerHTML = '';
      target = pickTarget();
      colorNameEl.textContent = target.name;
      speak(target.name);
      const others = COLORS.filter(c => c.name !== target.name);
      shuffle(others);
      const choices = [target, ...others.slice(0, 3)];
      shuffle(choices);
      choices.forEach(c => {
        const btn = document.createElement('button');
        btn.className = 'color-btn';
        btn.style.background = c.hex;
        btn.setAttribute('aria-label', c.name + ' color');
        const luminance = getLuminance(c.hex);
        btn.style.color = luminance > 0.7 ? '#111' : '#fff';
        btn.textContent = '';
        btn.addEventListener('click', () => handlePick(c));
        optionsEl.appendChild(btn);
      });
    }

    function getLuminance(hex) {
      const c = hex.replace('#','');
      const r = parseInt(c.substr(0,2),16)/255;
      const g = parseInt(c.substr(2,2),16)/255;
      const b = parseInt(c.substr(4,2),16)/255;
      const a = [r,g,b].map(v => v <= 0.03928 ? v/12.92 : Math.pow((v+0.055)/1.055,2.4));
      return 0.2126*a[0] + 0.7152*a[1] + 0.0722*a[2];
    }

    function handlePick(choice) {
      if (choice.name === target.name) {
        stars++;
        starsEl.textContent = stars;
        messageArea.textContent = 'Great job! ⭐';
        messageArea.className = 'message correct';
        speak('Yes! ' + target.name);
        flashBorder('#2ecc71');
      } else {
        messageArea.textContent = 'Oops! Try again.';
        messageArea.className = 'message wrong';
        speak('No, that is ' + choice.name + '. Try ' + target.name);
        flashBorder('#e74c3c');
      }
    }

    function flashBorder(color) {
      const card = document.querySelector('.card');
      const old = card.style.boxShadow;
      card.style.boxShadow = `0 0 0 6px ${hexToRGBA(color,0.14)}, 0 6px 18px rgba(2,6,23,0.06)`;
      setTimeout(() => {
        card.style.boxShadow = old || '0 6px 18px rgba(2,6,23,0.06)';
      }, 450);
    }

    function hexToRGBA(hex, a=1) {
      const c = hex.replace('#','');
      const r = parseInt(c.substr(0,2),16);
      const g = parseInt(c.substr(2,2),16);
      const b = parseInt(c.substr(4,2),16);
      return `rgba(${r},${g},${b},${a})`;
    }

    nextBtn.addEventListener('click', startRound);
    voiceToggle.addEventListener('click', () => {
      voiceOn = !voiceOn;
      voiceToggle.textContent = 'Voice: ' + (voiceOn ? 'On' : 'Off');
    });

    startRound();
  </script> 

