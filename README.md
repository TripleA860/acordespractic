<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>GuitarApp</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0a0f;
    --bg2: #111118;
    --bg3: #1a1a24;
    --bg4: #222230;
    --surface: #1e1e2a;
    --surface2: #252535;
    --accent: #7c6bff;
    --accent2: #a594ff;
    --accent3: #4ade80;
    --accent4: #f97316;
    --text: #f0f0fa;
    --text2: #a0a0c0;
    --text3: #5a5a78;
    --border: rgba(124,107,255,0.15);
    --border2: rgba(255,255,255,0.06);
    --font-head: 'Syne', sans-serif;
    --font-body: 'DM Sans', sans-serif;
    --nav-h: 72px;
    --radius: 16px;
    --radius-sm: 10px;
  }

  html, body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--font-body);
    height: 100%;
    overflow: hidden;
  }

  /* ─── Layout ─── */
  #app {
    display: flex;
    flex-direction: column;
    height: 100dvh;
    max-width: 480px;
    margin: 0 auto;
    position: relative;
  }

  #content {
    flex: 1;
    overflow-y: auto;
    overflow-x: hidden;
    scroll-behavior: smooth;
    padding-bottom: calc(var(--nav-h) + 16px);
    -webkit-overflow-scrolling: touch;
  }
  #content::-webkit-scrollbar { display: none; }

  /* ─── Header ─── */
  .header {
    padding: 20px 20px 12px;
    position: sticky;
    top: 0;
    z-index: 10;
    background: linear-gradient(to bottom, var(--bg) 80%, transparent);
  }
  .header-title {
    font-family: var(--font-head);
    font-size: 28px;
    font-weight: 800;
    letter-spacing: -0.5px;
    background: linear-gradient(135deg, var(--text) 30%, var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .header-sub {
    font-size: 13px;
    color: var(--text3);
    margin-top: 2px;
    font-weight: 400;
  }

  /* ─── Bottom Nav ─── */
  #bottom-nav {
    position: fixed;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 100%;
    max-width: 480px;
    height: var(--nav-h);
    background: rgba(10,10,15,0.92);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border-top: 1px solid var(--border2);
    display: flex;
    z-index: 100;
  }
  .nav-btn {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 4px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 8px 0 12px;
    transition: opacity 0.15s;
    -webkit-tap-highlight-color: transparent;
  }
  .nav-btn svg { width: 22px; height: 22px; fill: none; stroke: var(--text3); stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; transition: stroke 0.2s; }
  .nav-btn span { font-size: 10px; color: var(--text3); font-family: var(--font-body); font-weight: 500; letter-spacing: 0.3px; transition: color 0.2s; }
  .nav-btn.active svg { stroke: var(--accent2); }
  .nav-btn.active span { color: var(--accent2); }
  .nav-dot {
    width: 4px; height: 4px; border-radius: 50%;
    background: var(--accent);
    opacity: 0;
    transition: opacity 0.2s;
    margin-top: 2px;
  }
  .nav-btn.active .nav-dot { opacity: 1; }

  /* ─── Sections ─── */
  .section { display: none; }
  .section.active { display: block; }

  /* ─── Acordes Section ─── */
  .chord-group { margin-bottom: 8px; }
  .chord-group-title {
    font-family: var(--font-head);
    font-size: 20px;
    font-weight: 700;
    padding: 16px 20px 10px;
    letter-spacing: -0.3px;
    color: var(--text);
  }
  .chord-group-title span {
    font-size: 14px;
    font-weight: 400;
    color: var(--text3);
    margin-left: 6px;
    font-family: var(--font-body);
  }
  .chord-row {
    display: flex;
    gap: 12px;
    padding: 0 20px 20px;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
  }
  .chord-row::-webkit-scrollbar { display: none; }

  .chord-card {
    flex: 0 0 120px;
    scroll-snap-align: start;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
    cursor: pointer;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
  }
  .chord-card-name {
    font-size: 13px;
    font-weight: 500;
    color: var(--text2);
    font-family: var(--font-body);
  }
  .chord-diagram {
    width: 120px;
    height: 130px;
    border-radius: var(--radius-sm);
    background: var(--surface);
    border: 1px solid var(--border2);
    transition: transform 0.15s, border-color 0.15s, box-shadow 0.15s;
    display: block;
    overflow: hidden;
  }
  .chord-card:active .chord-diagram,
  .chord-card.playing .chord-diagram {
    transform: scale(0.94);
    border-color: var(--accent);
    box-shadow: 0 0 20px rgba(124,107,255,0.3);
  }
  .chord-card.playing .chord-card-name { color: var(--accent2); }

  /* ─── Notas Section ─── */
  .notes-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    padding: 8px 20px 20px;
  }
  .note-btn {
    aspect-ratio: 1;
    border-radius: var(--radius);
    border: 1px solid var(--border2);
    background: var(--surface);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    -webkit-tap-highlight-color: transparent;
    transition: transform 0.12s, box-shadow 0.12s;
    user-select: none;
    position: relative;
    overflow: hidden;
  }
  .note-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--note-color, var(--accent));
    opacity: 0;
    transition: opacity 0.15s;
  }
  .note-btn:active::before, .note-btn.playing::before { opacity: 0.15; }
  .note-btn:active, .note-btn.playing { transform: scale(0.93); }
  .note-name {
    font-family: var(--font-head);
    font-size: 28px;
    font-weight: 800;
    color: var(--text);
    z-index: 1;
    line-height: 1;
  }
  .note-label {
    font-size: 11px;
    color: var(--text3);
    margin-top: 4px;
    z-index: 1;
    font-weight: 500;
  }
  .note-freq {
    font-size: 10px;
    color: var(--text3);
    z-index: 1;
    margin-top: 2px;
  }
  .note-info-panel {
    margin: 0 20px 20px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 16px;
    min-height: 80px;
    transition: all 0.2s;
  }
  .note-info-title {
    font-family: var(--font-head);
    font-size: 22px;
    font-weight: 700;
    color: var(--accent2);
  }
  .note-info-detail {
    font-size: 13px;
    color: var(--text2);
    margin-top: 6px;
    line-height: 1.7;
  }
  .note-info-placeholder {
    font-size: 13px;
    color: var(--text3);
    font-style: italic;
  }

  /* ─── Wheel Section ─── */
  .wheel-wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 12px 20px 20px;
    gap: 20px;
  }
  #wheel-svg {
    width: 100%;
    max-width: 340px;
    touch-action: none;
  }
  .wheel-info {
    width: 100%;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 16px;
  }
  .wheel-info-title {
    font-family: var(--font-head);
    font-size: 20px;
    font-weight: 700;
    color: var(--accent2);
  }
  .wheel-scales {
    margin-top: 10px;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .scale-row {
    display: flex;
    align-items: center;
    gap: 8px;
    flex-wrap: wrap;
  }
  .scale-label {
    font-size: 12px;
    color: var(--text3);
    font-weight: 500;
    min-width: 70px;
  }
  .note-pill {
    background: var(--bg3);
    border: 1px solid var(--border2);
    border-radius: 20px;
    padding: 3px 10px;
    font-size: 12px;
    color: var(--text2);
    font-weight: 500;
  }
  .note-pill.root {
    background: var(--accent);
    border-color: var(--accent);
    color: #fff;
  }
  .note-pill.highlight {
    border-color: var(--accent);
    color: var(--accent2);
  }

  /* ─── Feedback pulse ─── */
  @keyframes ripple {
    0% { transform: scale(0.3); opacity: 0.6; }
    100% { transform: scale(2.5); opacity: 0; }
  }
  .ripple-effect {
    position: absolute;
    border-radius: 50%;
    background: var(--accent);
    pointer-events: none;
    animation: ripple 0.5s ease-out forwards;
    width: 60px; height: 60px;
    margin-left: -30px; margin-top: -30px;
  }

  /* ─── Scrollbar ghost fix ─── */
  * { scrollbar-width: none; }
</style>
</head>
<body>
<div id="app">
  <div id="content">

    <!-- SECTION: ACORDES -->
    <div id="sec-acordes" class="section active">
      <div class="header">
        <div class="header-title">Acordes</div>
        <div class="header-sub">Toca un diagrama para escuchar</div>
      </div>
      <div id="acordes-list"></div>
    </div>

    <!-- SECTION: NOTAS / COMPOSITOR -->
    <div id="sec-notas" class="section">
      <div class="header">
        <div class="header-title">Compositor</div>
        <div class="header-sub">Crea y guarda tus progresiones</div>
      </div>

      <!-- SAVED SONGS -->
      <div id="saved-songs-wrap" style="padding: 0 20px 8px; display:none;">
        <div style="display:flex; align-items:center; justify-content:space-between; margin-bottom:10px;">
          <span style="font-size:12px; color:var(--text3); font-weight:500; letter-spacing:0.5px; text-transform:uppercase;">Mis canciones</span>
          <button id="new-song-btn" style="background:none;border:none;cursor:pointer;color:var(--accent2);font-size:13px;font-family:var(--font-body);font-weight:500;">+ Nueva</button>
        </div>
        <div id="saved-songs-list" style="display:flex;flex-direction:column;gap:8px;"></div>
      </div>

      <!-- COMPOSER PANEL -->
      <div id="composer-panel" style="padding: 0 20px;">

        <!-- Song name input -->
        <div style="margin-bottom:14px;">
          <input id="song-name-input" type="text" placeholder="Nombre de la canción..." style="width:100%;background:var(--surface);border:1px solid var(--border2);border-radius:var(--radius-sm);padding:10px 14px;color:var(--text);font-family:var(--font-body);font-size:15px;outline:none;"/>
        </div>

        <!-- Style selector -->
        <div style="margin-bottom:14px;">
          <div style="font-size:11px;color:var(--text3);font-weight:500;letter-spacing:0.5px;text-transform:uppercase;margin-bottom:8px;">Estilo musical</div>
          <div id="style-pills" style="display:flex;gap:8px;flex-wrap:wrap;"></div>
        </div>

        <!-- Chord input row -->
        <div style="display:flex;gap:8px;margin-bottom:14px;">
          <div style="position:relative;flex:1;">
            <input id="chord-input" type="text" placeholder="Ej: Em, Am, G7..." maxlength="6"
              style="width:100%;background:var(--surface);border:1px solid var(--border2);border-radius:var(--radius-sm);padding:10px 14px;color:var(--text);font-family:var(--font-head);font-size:16px;font-weight:600;outline:none;text-transform:uppercase;"/>
            <div id="chord-autocomplete" style="display:none;position:absolute;top:100%;left:0;right:0;background:var(--surface2);border:1px solid var(--border);border-radius:var(--radius-sm);z-index:50;overflow:hidden;margin-top:4px;"></div>
          </div>
          <button id="add-chord-btn" style="background:var(--accent);border:none;border-radius:var(--radius-sm);padding:10px 18px;color:#fff;font-family:var(--font-head);font-size:15px;font-weight:700;cursor:pointer;white-space:nowrap;-webkit-tap-highlight-color:transparent;">+ Agregar</button>
        </div>

        <!-- Quick chord palette -->
        <div id="quick-palette" style="margin-bottom:16px;"></div>

        <!-- Progression display -->
        <div style="font-size:11px;color:var(--text3);font-weight:500;letter-spacing:0.5px;text-transform:uppercase;margin-bottom:10px;">Tu progresión</div>
        <div id="progression-display" style="min-height:64px;background:var(--surface);border:1px solid var(--border2);border-radius:var(--radius);padding:12px;margin-bottom:14px;display:flex;flex-wrap:wrap;gap:8px;align-items:center;">
          <span id="prog-placeholder" style="font-size:13px;color:var(--text3);font-style:italic;">Agrega acordes para comenzar...</span>
        </div>

        <!-- Action buttons -->
        <div style="display:flex;gap:8px;margin-bottom:20px;" id="action-btns">
          <button id="play-prog-btn" style="flex:1;background:var(--surface);border:1px solid var(--border2);border-radius:var(--radius-sm);padding:11px;color:var(--text2);font-family:var(--font-body);font-size:13px;font-weight:500;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:6px;" disabled>
            <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><polygon points="5,3 19,12 5,21"/></svg>
            Escuchar
          </button>
          <button id="suggest-btn" style="flex:1;background:var(--surface);border:1px solid var(--border2);border-radius:var(--radius-sm);padding:11px;color:var(--text2);font-family:var(--font-body);font-size:13px;font-weight:500;cursor:pointer;" disabled>
            ✦ Sugerir
          </button>
          <button id="save-song-btn" style="flex:1;background:var(--accent);border:none;border-radius:var(--radius-sm);padding:11px;color:#fff;font-family:var(--font-body);font-size:13px;font-weight:600;cursor:pointer;" disabled>
            Guardar
          </button>
        </div>

        <!-- Suggestions area -->
        <div id="suggestions-wrap" style="display:none;">
          <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:10px;">
            <div style="font-size:11px;color:var(--text3);font-weight:500;letter-spacing:0.5px;text-transform:uppercase;">Sugerencias ✦</div>
            <button id="dismiss-suggestions" style="background:none;border:none;color:var(--text3);font-size:12px;cursor:pointer;font-family:var(--font-body);">Cerrar</button>
          </div>
          <div id="suggestions-list" style="display:flex;flex-direction:column;gap:10px;"></div>
        </div>

      </div>
    </div>

    <!-- SECTION: RUEDA -->
    <div id="sec-rueda" class="section">
      <div class="header">
        <div class="header-title">Rueda Musical</div>
        <div class="header-sub">Toca una nota para ver sus relaciones</div>
      </div>
      <div class="wheel-wrap">
        <svg id="wheel-svg" viewBox="0 0 340 340" xmlns="http://www.w3.org/2000/svg"></svg>
        <div class="wheel-info" id="wheel-info">
          <div class="note-info-placeholder">Selecciona una nota en la rueda</div>
        </div>
      </div>
    </div>

  </div>

  <!-- BOTTOM NAV -->
  <nav id="bottom-nav">
    <button class="nav-btn active" data-sec="acordes">
      <svg viewBox="0 0 24 24"><path d="M9 18V5l12-2v13"/><circle cx="6" cy="18" r="3"/><circle cx="18" cy="16" r="3"/></svg>
      <span>Acordes</span>
      <div class="nav-dot"></div>
    </button>
    <button class="nav-btn" data-sec="notas">
      <svg viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2"/><path d="M7 8h10M7 12h6M7 16h4"/></svg>
      <span>Compositor</span>
      <div class="nav-dot"></div>
    </button>
    <button class="nav-btn" data-sec="rueda">
      <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="9"/><path d="M12 12L12 3M12 12L18.7 7.5M12 12L18.7 16.5M12 12L12 21M12 12L5.3 16.5M12 12L5.3 7.5"/></svg>
      <span>Rueda</span>
      <div class="nav-dot"></div>
    </button>
  </nav>
</div>

<script>
// ═══════════════════════════════════════
// AUDIO ENGINE
// ═══════════════════════════════════════
let audioCtx = null;
function getCtx() {
  if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  if (audioCtx.state === 'suspended') audioCtx.resume();
  return audioCtx;
}

function noteFreq(note, octave) {
  const notes = ['C','C#','D','D#','E','F','F#','G','G#','A','A#','B'];
  const n = notes.indexOf(note);
  return 440 * Math.pow(2, (n - 9 + (octave - 4) * 12) / 12);
}

function playNote(freq, duration = 1.2, type = 'triangle', volume = 0.4) {
  const ctx = getCtx();
  const osc = ctx.createOscillator();
  const gain = ctx.createGain();
  const dist = ctx.createWaveShaper();

  function makeDistCurve(amount) {
    const k = amount, n_samples = 256;
    const curve = new Float32Array(n_samples);
    for (let i = 0; i < n_samples; i++) {
      const x = (i * 2 / n_samples) - 1;
      curve[i] = (Math.PI + k) * x / (Math.PI + k * Math.abs(x));
    }
    return curve;
  }
  dist.curve = makeDistCurve(30);

  osc.connect(dist);
  dist.connect(gain);
  gain.connect(ctx.destination);

  osc.type = type;
  osc.frequency.setValueAtTime(freq, ctx.currentTime);
  osc.frequency.exponentialRampToValueAtTime(freq * 0.998, ctx.currentTime + duration);

  gain.gain.setValueAtTime(0, ctx.currentTime);
  gain.gain.linearRampToValueAtTime(volume, ctx.currentTime + 0.01);
  gain.gain.setValueAtTime(volume, ctx.currentTime + 0.1);
  gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + duration);

  osc.start(ctx.currentTime);
  osc.stop(ctx.currentTime + duration);
}

function playChordNotes(freqs, strum = true) {
  const ctx = getCtx();
  freqs.forEach((f, i) => {
    setTimeout(() => playNote(f, 1.8, 'sawtooth', 0.12), strum ? i * 35 : 0);
  });
}

// chord → [intervals from root in semitones]
const CHORD_TYPES = {
  '': [0, 4, 7],
  'm': [0, 3, 7],
  '7': [0, 4, 7, 10],
  'm7': [0, 3, 7, 10],
  'maj7': [0, 4, 7, 11],
  'sus2': [0, 2, 7],
  'sus4': [0, 5, 7],
  'dim': [0, 3, 6],
  'aug': [0, 4, 8],
  '5': [0, 7],
  'add9': [0, 4, 7, 14],
};
const NOTE_ORDER = ['C','C#','D','D#','E','F','F#','G','G#','A','A#','B'];

function chordFreqs(root, type, octave = 3) {
  const rootIdx = NOTE_ORDER.indexOf(root);
  const intervals = CHORD_TYPES[type] || [0,4,7];
  return intervals.map(semi => {
    const noteIdx = (rootIdx + semi) % 12;
    const oct = octave + Math.floor((rootIdx + semi) / 12);
    return noteFreq(NOTE_ORDER[noteIdx], oct);
  });
}

// ═══════════════════════════════════════
// SVG CHORD DIAGRAMS
// ═══════════════════════════════════════

// Chord fingering data: [string6..string1], value = fret (0=open, -1=mute, N=fret)
const CHORD_DATA = {
  'C':     { frets: [-1,3,2,0,1,0], barre: null },
  'Cm':    { frets: [-1,3,5,5,4,3], barre: {fret:3,from:1,to:5} },
  'C7':    { frets: [-1,3,2,3,1,0], barre: null },
  'Cm7':   { frets: [-1,3,5,3,4,3], barre: {fret:3,from:1,to:5} },
  'Csus2': { frets: [-1,3,5,5,3,3], barre: {fret:3,from:0,to:5} },
  'Cadd9': { frets: [-1,3,2,0,3,0], barre: null },

  'D':     { frets: [-1,-1,0,2,3,2], barre: null },
  'Dm':    { frets: [-1,-1,0,2,3,1], barre: null },
  'D7':    { frets: [-1,-1,0,2,1,2], barre: null },
  'Dm7':   { frets: [-1,-1,0,2,1,1], barre: null },
  'Dsus2': { frets: [-1,-1,0,2,3,0], barre: null },
  'Dsus4': { frets: [-1,-1,0,2,3,3], barre: null },

  'E':     { frets: [0,2,2,1,0,0], barre: null },
  'Em':    { frets: [0,2,2,0,0,0], barre: null },
  'E7':    { frets: [0,2,0,1,0,0], barre: null },
  'Em7':   { frets: [0,2,0,0,0,0], barre: null },
  'Esus4': { frets: [0,2,2,2,0,0], barre: null },
  'Eaug':  { frets: [0,3,2,1,1,0], barre: null },

  'F':     { frets: [1,3,3,2,1,1], barre: {fret:1,from:0,to:5} },
  'Fm':    { frets: [1,3,3,1,1,1], barre: {fret:1,from:0,to:5} },
  'F7':    { frets: [1,3,1,2,1,1], barre: {fret:1,from:0,to:5} },
  'Fmaj7': { frets: [1,3,3,2,1,0], barre: null },
  'Fsus2': { frets: [1,3,3,0,1,1], barre: {fret:1,from:0,to:5} },

  'G':     { frets: [3,2,0,0,0,3], barre: null },
  'Gm':    { frets: [3,5,5,3,3,3], barre: {fret:3,from:0,to:5} },
  'G7':    { frets: [3,2,0,0,0,1], barre: null },
  'Gmaj7': { frets: [3,2,0,0,0,2], barre: null },
  'Gsus2': { frets: [3,0,0,0,3,3], barre: null },
  'Gadd9': { frets: [3,2,0,2,0,3], barre: null },

  'A':     { frets: [-1,0,2,2,2,0], barre: null },
  'Am':    { frets: [-1,0,2,2,1,0], barre: null },
  'A7':    { frets: [-1,0,2,0,2,0], barre: null },
  'Am7':   { frets: [-1,0,2,0,1,0], barre: null },
  'Asus4': { frets: [-1,0,2,2,3,0], barre: null },
  'Amaj7': { frets: [-1,0,2,1,2,0], barre: null },

  'B':     { frets: [-1,2,4,4,4,2], barre: {fret:2,from:1,to:5} },
  'Bm':    { frets: [-1,2,4,4,3,2], barre: {fret:2,from:1,to:5} },
  'B7':    { frets: [-1,2,1,2,0,2], barre: null },
  'Bm7':   { frets: [-1,2,0,2,0,2], barre: null },
  'Bsus4': { frets: [-1,2,4,4,5,2], barre: {fret:2,from:1,to:5} },
};

function buildChordSVG(chordName) {
  const data = CHORD_DATA[chordName];
  if (!data) return `<svg viewBox="0 0 120 130" xmlns="http://www.w3.org/2000/svg"><text x="60" y="70" text-anchor="middle" fill="#5a5a78" font-size="12">?</text></svg>`;

  const frets = data.frets; // 6 strings: index 0 = low E (string 6)
  const barre = data.barre;

  const W = 120, H = 130;
  const ml = 18, mt = 28, mr = 8;
  const strW = (W - ml - mr) / 5;
  const fretH = 18;
  const numFrets = 5;

  // Determine starting fret
  const allFrets = frets.filter(f => f > 0);
  let startFret = 1;
  if (allFrets.length > 0) {
    const minF = Math.min(...allFrets);
    const maxF = Math.max(...allFrets);
    if (maxF > 5) startFret = minF;
  }

  let svg = `<svg viewBox="0 0 ${W} ${H}" xmlns="http://www.w3.org/2000/svg">`;

  // Nut or start fret indicator
  if (startFret === 1) {
    svg += `<line x1="${ml}" y1="${mt}" x2="${ml + strW*5}" y2="${mt}" stroke="#a0a0c0" stroke-width="3" stroke-linecap="round"/>`;
  } else {
    svg += `<text x="${ml - 6}" y="${mt + fretH*0.5 + 4}" text-anchor="end" fill="#7c6bff" font-size="10" font-family="DM Sans,sans-serif" font-weight="600">${startFret}fr</text>`;
    svg += `<line x1="${ml}" y1="${mt}" x2="${ml + strW*5}" y2="${mt}" stroke="#5a5a78" stroke-width="1"/>`;
  }

  // Fret lines
  for (let f = 1; f <= numFrets; f++) {
    const y = mt + f * fretH;
    svg += `<line x1="${ml}" y1="${y}" x2="${ml + strW*5}" y2="${y}" stroke="#2a2a3a" stroke-width="1"/>`;
  }

  // String lines
  for (let s = 0; s < 6; s++) {
    const x = ml + s * strW;
    svg += `<line x1="${x}" y1="${mt}" x2="${x}" y2="${mt + numFrets * fretH}" stroke="#3a3a50" stroke-width="1"/>`;
  }

  // Barre
  if (barre) {
    const barFret = barre.fret - startFret + 1;
    const x1 = ml + barre.from * strW;
    const x2 = ml + barre.to * strW;
    const y = mt + (barFret - 0.5) * fretH;
    svg += `<line x1="${x1}" y1="${y}" x2="${x2}" y2="${y}" stroke="#7c6bff" stroke-width="10" stroke-linecap="round" opacity="0.85"/>`;
  }

  // Dots
  frets.forEach((f, i) => {
    const x = ml + i * strW;
    if (f === -1) {
      // Mute
      svg += `<line x1="${x-4}" y1="${mt-12}" x2="${x+4}" y2="${mt-4}" stroke="#e24b4a" stroke-width="1.5" stroke-linecap="round"/>`;
      svg += `<line x1="${x+4}" y1="${mt-12}" x2="${x-4}" y2="${mt-4}" stroke="#e24b4a" stroke-width="1.5" stroke-linecap="round"/>`;
    } else if (f === 0) {
      // Open
      svg += `<circle cx="${x}" cy="${mt-8}" r="4" fill="none" stroke="#a0a0c0" stroke-width="1.5"/>`;
    } else {
      const relFret = f - startFret + 1;
      const y = mt + (relFret - 0.5) * fretH;
      // Skip if barre covers this
      if (barre && f === barre.fret && i >= barre.from && i <= barre.to) return;
      svg += `<circle cx="${x}" cy="${y}" r="7" fill="#7c6bff"/>`;
    }
  });

  svg += `</svg>`;
  return svg;
}

// ═══════════════════════════════════════
// CHORD GROUPS DATA
// ═══════════════════════════════════════

const CHORD_GROUPS = [
  { root: 'C', label: 'C (Do)',   chords: ['C','Cm','C7','Cm7','Csus2','Cadd9'] },
  { root: 'D', label: 'D (Re)',   chords: ['D','Dm','D7','Dm7','Dsus2','Dsus4'] },
  { root: 'E', label: 'E (Mi)',   chords: ['E','Em','E7','Em7','Esus4','Eaug'] },
  { root: 'F', label: 'F (Fa)',   chords: ['F','Fm','F7','Fmaj7','Fsus2'] },
  { root: 'G', label: 'G (Sol)',  chords: ['G','Gm','G7','Gmaj7','Gsus2','Gadd9'] },
  { root: 'A', label: 'A (La)',   chords: ['A','Am','A7','Am7','Asus4','Amaj7'] },
  { root: 'B', label: 'B (Si)',   chords: ['B','Bm','B7','Bm7','Bsus4'] },
];

// Root note for each chord
function getRootAndType(chordName) {
  const rootMap = {
    'C#':'C#','D#':'D#','F#':'F#','G#':'G#','A#':'A#',
    'Cmaj7':'C','Dm7':'D','Em7':'E','Fmaj7':'F','Gmaj7':'G','Am7':'A','Bm7':'B',
    'Csus2':'C','Dsus2':'D','Esus4':'E','Fsus2':'F','Gsus2':'G','Asus4':'A','Bsus4':'B',
    'Cadd9':'C','Gadd9':'G','Dadd9':'D',
    'Eaug':'E',
    'C7':'C','D7':'D','E7':'E','F7':'F','G7':'G','A7':'A','B7':'B',
  };
  for (const root of NOTE_ORDER) {
    if (chordName.startsWith(root)) {
      const type = chordName.slice(root.length);
      return { root, type: type in CHORD_TYPES ? type : '' };
    }
  }
  return { root: 'C', type: '' };
}

// ═══════════════════════════════════════
// BUILD ACORDES SECTION
// ═══════════════════════════════════════
function buildAcordes() {
  const container = document.getElementById('acordes-list');
  CHORD_GROUPS.forEach(group => {
    const div = document.createElement('div');
    div.className = 'chord-group';
    div.innerHTML = `<div class="chord-group-title">${group.label.split(' ')[0]} <span>${group.label.split(' ').slice(1).join(' ')}</span></div>`;
    const row = document.createElement('div');
    row.className = 'chord-row';
    group.chords.forEach(chordName => {
      const card = document.createElement('div');
      card.className = 'chord-card';
      const diagramDiv = document.createElement('div');
      diagramDiv.className = 'chord-diagram';
      diagramDiv.innerHTML = buildChordSVG(chordName);
      const nameDiv = document.createElement('div');
      nameDiv.className = 'chord-card-name';
      nameDiv.textContent = chordName;
      card.appendChild(nameDiv);
      card.appendChild(diagramDiv);
      card.addEventListener('click', (e) => {
        playChordSound(chordName, card, e);
      });
      row.appendChild(card);
    });
    div.appendChild(row);
    container.appendChild(div);
  });
}

function playChordSound(chordName, card, e) {
  const { root, type } = getRootAndType(chordName);
  const freqs = chordFreqs(root, type, 3);
  playChordNotes(freqs);
  card.classList.add('playing');
  setTimeout(() => card.classList.remove('playing'), 700);
  addRipple(card.querySelector('.chord-diagram'), e);
}

function addRipple(el, e) {
  const r = document.createElement('div');
  r.className = 'ripple-effect';
  const rect = el.getBoundingClientRect();
  const x = (e.clientX || rect.left + rect.width/2) - rect.left;
  const y = (e.clientY || rect.top + rect.height/2) - rect.top;
  r.style.left = x + 'px';
  r.style.top = y + 'px';
  el.style.position = 'relative';
  el.appendChild(r);
  setTimeout(() => r.remove(), 600);
}

// ═══════════════════════════════════════
// COMPOSITOR DE PROGRESIONES
// ═══════════════════════════════════════

const STYLE_DATA = {
  worship:  { label: '🙏 Worship',  color: '#a594ff', progressions: [
    ['G','Em','C','D'],['D','A','Bm','G'],['E','B','C#m','A'],
    ['C','G','Am','F'],['G','D','Em','C'],['A','E','F#m','D'],
  ]},
  pop: { label: '🎵 Pop', color: '#38bdf8', progressions: [
    ['C','G','Am','F'],['G','D','Em','C'],['Am','F','C','G'],
    ['D','A','Bm','G'],['C','Am','F','G'],['F','G','Am','C'],
  ]},
  rock: { label: '🎸 Rock', color: '#f97316', progressions: [
    ['E','A','D','A'],['A','D','E','D'],['G','C','D','C'],
    ['Em','C','G','D'],['A','G','D','A'],['E','B','A','E'],
  ]},
  blues: { label: '🎷 Blues', color: '#4ade80', progressions: [
    ['A7','D7','A7','E7'],['E7','A7','B7','A7'],['G7','C7','G7','D7'],
    ['A','D','A','E'],['C7','F7','C7','G7'],
  ]},
  latin: { label: '💃 Latin', color: '#f43f5e', progressions: [
    ['Am','Dm','E7','Am'],['Dm','Gm','A7','Dm'],['Am','F','C','E7'],
    ['Cm','Fm','G7','Cm'],['Am','E7','Am','Dm'],
  ]},
  jazz: { label: '🎺 Jazz', color: '#facc15', progressions: [
    ['Cmaj7','Am7','Dm7','G7'],['Fmaj7','Bm7','E7','Amaj7'],
    ['Am7','D7','Gmaj7','Cmaj7'],['Dm7','G7','Cmaj7','Am7'],
  ]},
};

let currentProgression = [];
let currentStyle = 'worship';
let savedSongs = JSON.parse(localStorage.getItem('guitar_songs') || '[]');
let editingIndex = -1;

const ALL_CHORDS = [
  'C','Cm','C7','Cm7','Cmaj7','Csus2','Cadd9',
  'D','Dm','D7','Dm7','Dsus2','Dsus4',
  'E','Em','E7','Em7','Esus4',
  'F','Fm','F7','Fmaj7','Fsus2',
  'G','Gm','G7','Gmaj7','Gsus2','Gadd9',
  'A','Am','A7','Am7','Asus4','Amaj7',
  'B','Bm','B7','Bm7','Bsus4',
  'F#m','C#m','G#m','Bb','Bbm','Eb','Ebm','Ab','Abm','Db',
];

function initCompositor() {
  buildStylePills();
  buildQuickPalette();
  renderSavedSongs();
  bindCompositorEvents();
}

function buildStylePills() {
  const wrap = document.getElementById('style-pills');
  wrap.innerHTML = '';
  Object.entries(STYLE_DATA).forEach(([key, s]) => {
    const btn = document.createElement('button');
    btn.textContent = s.label;
    btn.dataset.style = key;
    btn.style.cssText = `background:${key===currentStyle ? s.color+'33' : 'var(--surface)'};border:1px solid ${key===currentStyle ? s.color : 'var(--border2)'};border-radius:20px;padding:6px 14px;color:${key===currentStyle ? s.color : 'var(--text3)'};font-family:var(--font-body);font-size:13px;font-weight:500;cursor:pointer;-webkit-tap-highlight-color:transparent;transition:all 0.15s;`;
    btn.addEventListener('click', () => { currentStyle = key; buildStylePills(); });
    wrap.appendChild(btn);
  });
}

const QUICK_ROOTS = ['C','D','E','F','G','A','B'];
function buildQuickPalette() {
  const wrap = document.getElementById('quick-palette');
  const inner = document.createElement('div');
  inner.style.cssText = 'display:flex;gap:6px;flex-wrap:wrap;';
  wrap.innerHTML = '<div style="font-size:11px;color:var(--text3);font-weight:500;letter-spacing:0.5px;text-transform:uppercase;margin-bottom:8px;">Acceso rápido</div>';
  const types = ['','m','7','m7'];
  QUICK_ROOTS.forEach(r => {
    types.forEach(t => {
      const ch = r + t;
      const btn = document.createElement('button');
      btn.textContent = ch;
      btn.style.cssText = `background:var(--surface);border:1px solid var(--border2);border-radius:8px;padding:6px 10px;color:var(--text2);font-family:var(--font-head);font-size:13px;font-weight:600;cursor:pointer;-webkit-tap-highlight-color:transparent;`;
      btn.addEventListener('click', () => addChordToProgression(ch));
      inner.appendChild(btn);
    });
  });
  wrap.appendChild(inner);
}

function bindCompositorEvents() {
  const input = document.getElementById('chord-input');
  const addBtn = document.getElementById('add-chord-btn');
  input.addEventListener('input', () => showAutocomplete(input.value.trim()));
  input.addEventListener('keydown', (e) => { if (e.key === 'Enter') tryAddFromInput(); });
  addBtn.addEventListener('click', tryAddFromInput);
  document.getElementById('suggest-btn').addEventListener('click', showSuggestions);
  document.getElementById('play-prog-btn').addEventListener('click', playProgression);
  document.getElementById('save-song-btn').addEventListener('click', saveSong);
  document.getElementById('dismiss-suggestions').addEventListener('click', () => {
    document.getElementById('suggestions-wrap').style.display = 'none';
  });
  document.getElementById('new-song-btn').addEventListener('click', () => {
    editingIndex = -1; currentProgression = [];
    document.getElementById('song-name-input').value = '';
    renderProgression(); updateActionBtns();
    document.getElementById('suggestions-wrap').style.display = 'none';
  });
  document.addEventListener('click', (e) => {
    if (!e.target.closest('#chord-input') && !e.target.closest('#chord-autocomplete'))
      document.getElementById('chord-autocomplete').style.display = 'none';
  });
}

function showAutocomplete(val) {
  const ac = document.getElementById('chord-autocomplete');
  if (!val) { ac.style.display = 'none'; return; }
  const matches = ALL_CHORDS.filter(c => c.toLowerCase().startsWith(val.toLowerCase())).slice(0,6);
  if (!matches.length) { ac.style.display = 'none'; return; }
  ac.style.display = 'block';
  ac.innerHTML = matches.map(c => `<div data-chord="${c}" style="padding:10px 14px;cursor:pointer;font-family:var(--font-head);font-weight:600;font-size:15px;color:var(--text);border-bottom:1px solid var(--border2);">${c}</div>`).join('');
  ac.querySelectorAll('[data-chord]').forEach(d => {
    d.addEventListener('click', () => {
      addChordToProgression(d.dataset.chord);
      document.getElementById('chord-input').value = '';
      ac.style.display = 'none';
    });
  });
}

function tryAddFromInput() {
  const input = document.getElementById('chord-input');
  const val = input.value.trim();
  if (!val) return;
  const match = ALL_CHORDS.find(c => c.toLowerCase() === val.toLowerCase()) || val;
  addChordToProgression(match);
  input.value = '';
  document.getElementById('chord-autocomplete').style.display = 'none';
}

function addChordToProgression(chord) {
  if (currentProgression.length >= 16) return;
  currentProgression.push(chord);
  renderProgression();
  updateActionBtns();
}

function removeChordFromProgression(idx) {
  currentProgression.splice(idx, 1);
  renderProgression();
  updateActionBtns();
}

const CHORD_COLORS_C = ['#7c6bff','#a594ff','#38bdf8','#4ade80','#f97316','#f43f5e','#facc15','#e879f9'];
function chordColor(chord) {
  const roots = ['C','D','E','F','G','A','B','F#','C#','G#','D#','A#','Bb','Eb','Ab','Db'];
  for (let i = 0; i < roots.length; i++) {
    if (chord.startsWith(roots[i])) return CHORD_COLORS_C[i % CHORD_COLORS_C.length];
  }
  return CHORD_COLORS_C[0];
}

function renderProgression() {
  const disp = document.getElementById('progression-display');
  const placeholder = document.getElementById('prog-placeholder');
  disp.querySelectorAll('.prog-chip').forEach(c => c.remove());
  if (!currentProgression.length) { placeholder.style.display = ''; return; }
  placeholder.style.display = 'none';
  currentProgression.forEach((chord, i) => {
    const color = chordColor(chord);
    const chip = document.createElement('div');
    chip.className = 'prog-chip';
    chip.style.cssText = `display:inline-flex;align-items:center;gap:6px;background:${color}22;border:1px solid ${color}55;border-radius:10px;padding:7px 10px 7px 12px;cursor:pointer;transition:all 0.12s;-webkit-tap-highlight-color:transparent;`;
    chip.innerHTML = `<span style="font-family:var(--font-head);font-size:16px;font-weight:700;color:${color};">${chord}</span><button data-i="${i}" style="background:${color}33;border:none;border-radius:50%;width:18px;height:18px;display:flex;align-items:center;justify-content:center;cursor:pointer;color:${color};font-size:11px;line-height:1;padding:0;">✕</button>`;
    chip.querySelector('button').addEventListener('click', (e) => { e.stopPropagation(); removeChordFromProgression(i); });
    chip.addEventListener('click', (e) => { if (!e.target.closest('button')) playChordFromProg(chord, chip); });
    disp.appendChild(chip);
  });
}

function playChordFromProg(chordName, chip) {
  const { root, type } = getRootAndType(chordName);
  playChordNotes(chordFreqs(root, type, 3));
  chip.style.transform = 'scale(0.93)';
  setTimeout(() => chip.style.transform = '', 200);
}

let playInterval = null;
function playProgression() {
  if (!currentProgression.length) return;
  if (playInterval) { clearInterval(playInterval); playInterval = null; document.querySelectorAll('.prog-chip').forEach(c => c.style.boxShadow = ''); return; }
  let i = 0;
  function playNext() {
    const chips = document.querySelectorAll('.prog-chip');
    chips.forEach(c => c.style.boxShadow = '');
    if (i >= currentProgression.length) { clearInterval(playInterval); playInterval = null; return; }
    const { root, type } = getRootAndType(currentProgression[i]);
    playChordNotes(chordFreqs(root, type, 3));
    if (chips[i]) chips[i].style.boxShadow = '0 0 0 2px var(--accent)';
    i++;
  }
  playNext();
  playInterval = setInterval(playNext, 1800);
}

function updateActionBtns() {
  const has = currentProgression.length > 0;
  ['play-prog-btn','suggest-btn','save-song-btn'].forEach(id => {
    const btn = document.getElementById(id);
    btn.disabled = !has;
    btn.style.opacity = has ? '1' : '0.4';
  });
}

function showSuggestions() {
  const wrap = document.getElementById('suggestions-wrap');
  const list = document.getElementById('suggestions-list');
  wrap.style.display = 'block';
  list.innerHTML = '';
  const s = STYLE_DATA[currentStyle];
  const suggestions = [];
  const lastChord = currentProgression[currentProgression.length - 1];
  const nextChords = getNextChordSuggestions(lastChord);
  if (nextChords.length) suggestions.push({ type: 'next', label: `Siguientes a ${lastChord}`, chords: nextChords, color: '#4ade80' });
  s.progressions.forEach(prog => suggestions.push({ type: 'progression', label: s.label, prog, color: s.color }));
  suggestions.slice(0, 7).forEach(sg => {
    const card = document.createElement('div');
    card.style.cssText = `background:var(--surface);border:1px solid ${sg.color}33;border-radius:var(--radius);padding:12px 14px;margin-bottom:0;`;
    if (sg.type === 'next') {
      card.innerHTML = `<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:8px;"><span style="font-size:11px;color:${sg.color};font-weight:600;letter-spacing:0.5px;text-transform:uppercase;">${sg.label}</span><button class="dismiss-card" style="background:none;border:none;color:var(--text3);font-size:14px;cursor:pointer;padding:0 2px;">✕</button></div><div style="display:flex;gap:6px;flex-wrap:wrap;">${sg.chords.map(c=>`<button class="sug-add-btn" data-chord="${c}" style="background:${chordColor(c)}22;border:1px solid ${chordColor(c)}55;border-radius:8px;padding:6px 12px;color:${chordColor(c)};font-family:var(--font-head);font-size:14px;font-weight:700;cursor:pointer;-webkit-tap-highlight-color:transparent;">${c}</button>`).join('')}</div>`;
    } else {
      const progHtml = sg.prog.map(c => `<span style="background:${chordColor(c)}22;border:1px solid ${chordColor(c)}44;border-radius:8px;padding:4px 10px;color:${chordColor(c)};font-family:var(--font-head);font-size:14px;font-weight:700;">${c}</span>`).join(`<span style="color:var(--text3);font-size:12px;">›</span>`);
      card.innerHTML = `<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:8px;"><span style="font-size:11px;color:${sg.color};font-weight:600;letter-spacing:0.5px;text-transform:uppercase;">${sg.label}</span><div style="display:flex;gap:6px;"><button class="use-prog-btn" style="background:${sg.color}22;border:1px solid ${sg.color}55;border-radius:6px;padding:4px 10px;color:${sg.color};font-size:12px;cursor:pointer;font-weight:500;">Usar</button><button class="dismiss-card" style="background:none;border:none;color:var(--text3);font-size:14px;cursor:pointer;padding:0 2px;">✕</button></div></div><div style="display:flex;gap:6px;flex-wrap:wrap;align-items:center;">${progHtml}</div>`;
      card.querySelector('.use-prog-btn').addEventListener('click', () => {
        currentProgression = [...sg.prog]; renderProgression(); updateActionBtns(); wrap.style.display = 'none';
      });
    }
    card.querySelector('.dismiss-card').addEventListener('click', () => card.remove());
    card.querySelectorAll('.sug-add-btn').forEach(btn => btn.addEventListener('click', () => addChordToProgression(btn.dataset.chord)));
    list.appendChild(card);
  });
}

function getNextChordSuggestions(chord) {
  const map = {
    C:['G','Am','F','Em','Dm'],D:['G','A','Bm','Em','F#m'],E:['A','B','C#m','F#m','Am'],
    F:['C','G','Am','Dm','Bb'],G:['C','D','Em','Am','Bm'],A:['D','E','F#m','Bm','C#m'],
    B:['E','F#m','G#m','A','C#m'],Em:['C','G','D','Am','Bm'],Am:['F','G','C','Dm','E7'],
    Dm:['Gm','A7','F','C','Bb'],
  };
  for (const key in map) { if (chord && chord.toLowerCase()===key.toLowerCase()) return map[key]; }
  return [...new Set(STYLE_DATA[currentStyle].progressions.flat())].slice(0,5);
}

function saveSong() {
  const name = document.getElementById('song-name-input').value.trim() || `Progresión ${savedSongs.length + 1}`;
  const song = { name, style: currentStyle, chords: [...currentProgression], date: Date.now() };
  if (editingIndex >= 0) savedSongs[editingIndex] = song;
  else { savedSongs.unshift(song); if (savedSongs.length > 20) savedSongs.pop(); }
  localStorage.setItem('guitar_songs', JSON.stringify(savedSongs));
  renderSavedSongs();
  showToast(`"${name}" guardada ✓`);
}

function renderSavedSongs() {
  const wrap = document.getElementById('saved-songs-wrap');
  const list = document.getElementById('saved-songs-list');
  if (!savedSongs.length) { wrap.style.display = 'none'; return; }
  wrap.style.display = 'block';
  list.innerHTML = '';
  savedSongs.slice(0,8).forEach((song, i) => {
    const color = STYLE_DATA[song.style]?.color || '#7c6bff';
    const card = document.createElement('div');
    card.style.cssText = `background:var(--surface);border:1px solid var(--border2);border-radius:var(--radius-sm);padding:12px 14px;display:flex;align-items:center;gap:10px;`;
    const preview = song.chords.slice(0,5).join(' › ') + (song.chords.length > 5 ? '…' : '');
    card.innerHTML = `<div style="flex:1;min-width:0;"><div style="font-family:var(--font-head);font-size:14px;font-weight:700;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis;">${song.name}</div><div style="font-size:12px;color:var(--text3);margin-top:2px;">${preview}</div><div style="font-size:11px;color:${color};margin-top:3px;font-weight:500;">${STYLE_DATA[song.style]?.label || song.style}</div></div><div style="display:flex;gap:6px;"><button data-load="${i}" style="background:var(--bg3);border:none;border-radius:6px;padding:6px 10px;color:var(--text2);font-size:12px;cursor:pointer;">Cargar</button><button data-del="${i}" style="background:none;border:none;color:var(--text3);font-size:16px;cursor:pointer;padding:0 4px;">✕</button></div>`;
    card.querySelector(`[data-load]`).addEventListener('click', () => loadSong(i));
    card.querySelector(`[data-del]`).addEventListener('click', () => deleteSong(i));
    list.appendChild(card);
  });
}

function loadSong(i) {
  const song = savedSongs[i];
  editingIndex = i; currentProgression = [...song.chords]; currentStyle = song.style;
  document.getElementById('song-name-input').value = song.name;
  renderProgression(); updateActionBtns(); buildStylePills();
  document.getElementById('content').scrollTop = 0;
}

function deleteSong(i) {
  savedSongs.splice(i, 1);
  localStorage.setItem('guitar_songs', JSON.stringify(savedSongs));
  renderSavedSongs();
}

function showToast(msg) {
  const t = document.createElement('div');
  t.textContent = msg;
  t.style.cssText = `position:fixed;bottom:90px;left:50%;transform:translateX(-50%);background:var(--accent);color:#fff;padding:10px 20px;border-radius:20px;font-size:13px;font-weight:600;font-family:var(--font-body);z-index:999;pointer-events:none;transition:opacity 0.4s;`;
  document.body.appendChild(t);
  setTimeout(() => { t.style.opacity = '0'; setTimeout(() => t.remove(), 400); }, 1800);
}

function getMajorScale(root) {
  const steps = [0,2,4,5,7,9,11,12];
  const idx = NOTE_ORDER.indexOf(root);
  return steps.map(s => NOTE_ORDER[(idx + s) % 12]);
}
function getMinorScale(root) {
  const steps = [0,2,3,5,7,8,10,12];
  const idx = NOTE_ORDER.indexOf(root);
  return steps.map(s => NOTE_ORDER[(idx + s) % 12]);
}

initCompositor();

// ═══════════════════════════════════════
// WHEEL / RUEDA MUSICAL
// ═══════════════════════════════════════
const CHROMATIC = ['C','C#','D','D#','E','F','F#','G','G#','A','A#','B'];
const NOTE_NAMES_ES = { C:'Do',D:'Re',E:'Mi',F:'Fa',G:'Sol',A:'La',B:'Si', 'C#':'Do#','D#':'Re#','F#':'Fa#','G#':'Sol#','A#':'La#' };
const COLORS_WHEEL = ['#7c6bff','#8b7aff','#9b8aff','#a594ff','#38bdf8','#34d399','#4ade80','#a3e635','#facc15','#f97316','#f43f5e','#e879f9'];

let selectedWheelNote = null;

function buildWheel() {
  const svg = document.getElementById('wheel-svg');
  const cx = 170, cy = 170;
  const outerR = 150, innerR = 85, midR = 118;

  svg.innerHTML = '';

  // Outer ring (chromatic notes)
  CHROMATIC.forEach((note, i) => {
    const angle = (i * 30 - 90) * Math.PI / 180;
    const startAngle = ((i * 30 - 15) - 90) * Math.PI / 180;
    const endAngle = ((i * 30 + 15) - 90) * Math.PI / 180;

    const x1o = cx + outerR * Math.cos(startAngle);
    const y1o = cy + outerR * Math.sin(startAngle);
    const x2o = cx + outerR * Math.cos(endAngle);
    const y2o = cy + outerR * Math.sin(endAngle);
    const x1i = cx + (midR+2) * Math.cos(startAngle);
    const y1i = cy + (midR+2) * Math.sin(startAngle);
    const x2i = cx + (midR+2) * Math.cos(endAngle);
    const y2i = cy + (midR+2) * Math.sin(endAngle);

    const path = `M${x1i},${y1i} A${midR+2},${midR+2} 0 0,1 ${x2i},${y2i} L${x2o},${y2o} A${outerR},${outerR} 0 0,0 ${x1o},${y1o} Z`;
    const seg = document.createElementNS('http://www.w3.org/2000/svg','path');
    seg.setAttribute('d', path);
    seg.setAttribute('fill', COLORS_WHEEL[i] + '22');
    seg.setAttribute('stroke', COLORS_WHEEL[i] + '55');
    seg.setAttribute('stroke-width', '0.5');
    seg.setAttribute('data-note', note);
    seg.style.cursor = 'pointer';
    seg.style.transition = 'fill 0.2s';
    svg.appendChild(seg);

    // Note label
    const tx = cx + (midR + 20) * Math.cos(angle);
    const ty = cy + (midR + 20) * Math.sin(angle);
    const txt = document.createElementNS('http://www.w3.org/2000/svg','text');
    txt.setAttribute('x', tx);
    txt.setAttribute('y', ty);
    txt.setAttribute('text-anchor', 'middle');
    txt.setAttribute('dominant-baseline', 'middle');
    txt.setAttribute('fill', COLORS_WHEEL[i]);
    txt.setAttribute('font-size', note.includes('#') ? '11' : '13');
    txt.setAttribute('font-family', 'Syne, sans-serif');
    txt.setAttribute('font-weight', '700');
    txt.setAttribute('data-note', note);
    txt.style.pointerEvents = 'none';
    txt.textContent = note;
    svg.appendChild(txt);
  });

  // Inner circle (clickable notes — diatonic)
  const DIATONIC = ['C','D','E','F','G','A','B'];
  DIATONIC.forEach((note, i) => {
    const angle = (i * (360/7) - 90) * Math.PI / 180;
    const r = innerR - 22;
    const x = cx + r * Math.cos(angle);
    const y = cy + r * Math.sin(angle);
    const idx = CHROMATIC.indexOf(note);
    const color = COLORS_WHEEL[idx];

    const circle = document.createElementNS('http://www.w3.org/2000/svg','circle');
    circle.setAttribute('cx', x);
    circle.setAttribute('cy', y);
    circle.setAttribute('r', '22');
    circle.setAttribute('fill', color + '20');
    circle.setAttribute('stroke', color + '80');
    circle.setAttribute('stroke-width', '1.5');
    circle.setAttribute('data-note', note);
    circle.style.cursor = 'pointer';
    svg.appendChild(circle);

    const txt = document.createElementNS('http://www.w3.org/2000/svg','text');
    txt.setAttribute('x', x);
    txt.setAttribute('y', y - 2);
    txt.setAttribute('text-anchor', 'middle');
    txt.setAttribute('dominant-baseline', 'middle');
    txt.setAttribute('fill', color);
    txt.setAttribute('font-size', '13');
    txt.setAttribute('font-family', 'Syne, sans-serif');
    txt.setAttribute('font-weight', '800');
    txt.setAttribute('data-note', note);
    txt.style.pointerEvents = 'none';
    txt.textContent = NOTE_NAMES_ES[note] || note;
    svg.appendChild(txt);

    const sub = document.createElementNS('http://www.w3.org/2000/svg','text');
    sub.setAttribute('x', x);
    sub.setAttribute('y', y + 10);
    sub.setAttribute('text-anchor', 'middle');
    sub.setAttribute('dominant-baseline', 'middle');
    sub.setAttribute('fill', color + 'aa');
    sub.setAttribute('font-size', '9');
    sub.setAttribute('font-family', 'DM Sans, sans-serif');
    sub.setAttribute('data-note', note);
    sub.style.pointerEvents = 'none';
    sub.textContent = note;
    svg.appendChild(sub);
  });

  // Center
  const centerC = document.createElementNS('http://www.w3.org/2000/svg','circle');
  centerC.setAttribute('cx', cx); centerC.setAttribute('cy', cy);
  centerC.setAttribute('r', '28');
  centerC.setAttribute('fill', '#1a1a24');
  centerC.setAttribute('stroke', '#2a2a3a');
  centerC.setAttribute('stroke-width', '1');
  svg.appendChild(centerC);

  const centerT = document.createElementNS('http://www.w3.org/2000/svg','text');
  centerT.setAttribute('x', cx); centerT.setAttribute('y', cy);
  centerT.setAttribute('text-anchor', 'middle');
  centerT.setAttribute('dominant-baseline', 'middle');
  centerT.setAttribute('fill', '#5a5a78');
  centerT.setAttribute('font-size', '10');
  centerT.setAttribute('font-family', 'DM Sans, sans-serif');
  centerT.textContent = 'RUEDA';
  svg.appendChild(centerT);

  // Events
  svg.addEventListener('click', (e) => {
    const note = e.target.getAttribute('data-note');
    if (note) selectWheelNote(note);
  });
}

function selectWheelNote(note) {
  selectedWheelNote = note;
  const idx = CHROMATIC.indexOf(note);
  const color = COLORS_WHEEL[idx];
  const freq = noteFreq(note, 4);
  playNote(freq, 1.0, 'triangle', 0.4);
  updateWheelInfo(note, color);
  highlightWheelNote(note, color);
}

function highlightWheelNote(note, color) {
  const svg = document.getElementById('wheel-svg');
  svg.querySelectorAll('path[data-note], circle[data-note]').forEach(el => {
    el.setAttribute('fill', el.tagName === 'path'
      ? COLORS_WHEEL[CHROMATIC.indexOf(el.getAttribute('data-note'))] + '22'
      : COLORS_WHEEL[CHROMATIC.indexOf(el.getAttribute('data-note'))] + '20');
    el.setAttribute('stroke-width', el.tagName === 'circle' ? '1.5' : '0.5');
  });
  // Highlight selected
  svg.querySelectorAll(`[data-note="${note}"]`).forEach(el => {
    if (el.tagName === 'path') {
      el.setAttribute('fill', color + '55');
      el.setAttribute('stroke', color);
    } else if (el.tagName === 'circle') {
      el.setAttribute('fill', color + '55');
      el.setAttribute('stroke', color);
      el.setAttribute('stroke-width', '2.5');
    }
  });
}

function updateWheelInfo(note, color) {
  const major = getMajorScale(note);
  const minor = getMinorScale(note);
  const nameEs = NOTE_NAMES_ES[note] || note;
  const dom5 = CHROMATIC[(CHROMATIC.indexOf(note) + 7) % 12];
  const sub4 = CHROMATIC[(CHROMATIC.indexOf(note) + 5) % 12];
  const rel = CHROMATIC[(CHROMATIC.indexOf(note) + 9) % 12];

  const pills = (arr, root) => arr.map((n,i) => {
    const isRoot = i === 0 || i === 7;
    return `<span class="note-pill ${isRoot?'root':''}">${n}</span>`;
  }).join('');

  document.getElementById('wheel-info').innerHTML = `
    <div class="wheel-info-title" style="color:${color}">${nameEs} (${note})</div>
    <div class="wheel-scales">
      <div class="scale-row">
        <span class="scale-label">Mayor:</span>
        ${pills(major, note)}
      </div>
      <div class="scale-row">
        <span class="scale-label">Menor:</span>
        ${pills(minor, note)}
      </div>
      <div class="scale-row">
        <span class="scale-label">Dominante:</span>
        <span class="note-pill highlight">${dom5}</span>
        <span style="font-size:11px;color:var(--text3)">5ª</span>
        <span class="note-pill highlight">${sub4}</span>
        <span style="font-size:11px;color:var(--text3)">4ª</span>
      </div>
      <div class="scale-row">
        <span class="scale-label">Relativa:</span>
        <span class="note-pill">${rel}m</span>
        <span style="font-size:11px;color:var(--text3)">menor relativa</span>
      </div>
    </div>
  `;
}

// ═══════════════════════════════════════
// NAVIGATION
// ═══════════════════════════════════════
document.querySelectorAll('.nav-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    const sec = btn.dataset.sec;
    document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
    document.getElementById('sec-' + sec).classList.add('active');
    document.getElementById('content').scrollTop = 0;
  });
});

// ═══════════════════════════════════════
// INIT
// ═══════════════════════════════════════
buildAcordes();
buildWheel();
</script>
</body>
</html>
