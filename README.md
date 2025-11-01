# Color-match-
Color matching game for children's.
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