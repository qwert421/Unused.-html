<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Ошибка</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body {
    width: 100%; height: 100%;
    background: #000; color: #fff;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif;
    overflow: hidden;
    display: flex; justify-content: center; align-items: center;
    user-select: none;
    -webkit-tap-highlight-color: transparent;
    touch-action: none;
  }
  #countdown {
    font-size: 25vw; font-weight: bold;
    color: #fff; text-shadow: 0 0 20px #f00;
    z-index: 10;
  }
  #error-screen {
    display: none;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 20px;
    width: 100%; height: 100%;
    background: #1a1a1a;
    z-index: 20;
  }
  .error-icon { font-size: 80px; margin-bottom: 20px; color: #ff5555; }
  .error-title { font-size: 22px; font-weight: bold; margin-bottom: 10px; }
  .error-text { font-size: 16px; color: #aaa; margin-bottom: 40px; }
  .fix-button {
    background: #3ddc84; color: #000; border: none; border-radius: 4px;
    padding: 14px 40px; font-size: 18px; font-weight: bold;
    text-transform: uppercase; cursor: pointer;
    box-shadow: 0 4px 10px rgba(0,0,0,0.5);
  }
  .fix-button:active { transform: scale(0.95); }

  #screamer {
    display: none;
    position: fixed; top: 0; left: 0;
    width: 100vw; height: 100vh;
    background: #000;
    z-index: 100;
    justify-content: center; align-items: center;
    overflow: hidden;
  }
  #screamer.active {
    display: flex;
    animation: violentShake 0.05s infinite;
  }
  #faceWrap {
    position: absolute; inset: 0;
    width: 100%; height: 100%;
    animation: facePulse 0.08s infinite alternate;
  }
  #faceWrap img { width: 100%; height: 100%; object-fit: cover; display: block; }

  .flash {
    position: absolute; inset: 0;
    background: #ff0000; opacity: 0; pointer-events: none;
    animation: flashAnim 0.2s infinite;
  }
  @keyframes flashAnim {
    0%   { opacity: 0; }
    15%  { opacity: 0.9; }
    30%  { opacity: 0; }
    55%  { opacity: 0.8; background:#ffffff; }
    70%  { opacity: 0; }
    85%  { opacity: 0.9; background:#ff0000; }
    100% { opacity: 0; }
  }
  @keyframes violentShake {
    0%   { transform: translate(0,0) rotate(0deg); }
    25%  { transform: translate(-10px,5px) rotate(-1.5deg); }
    50%  { transform: translate(10px,-8px) rotate(1.5deg); }
    75%  { transform: translate(-8px,-6px) rotate(-1deg); }
    100% { transform: translate(0,0) rotate(0deg); }
  }
  @keyframes facePulse {
    from { transform: scale(1); filter: contrast(1.3) saturate(1.5); }
    to   { transform: scale(1.1); filter: contrast(1.8) saturate(2) hue-rotate(-15deg); }
  }
</style>
</head>
<body>
  <div id="countdown">10</div>

  <div id="error-screen">
    <div class="error-icon">⚠️</div>
    <div class="error-title">Произошла ошибка в приложении</div>
    <div class="error-text">Приложение не отвечает. Попробуйте исправить.</div>
    <button class="fix-button" id="fixBtn">Починить</button>
  </div>

  <div id="screamer">
    <div id="faceWrap">
      <img src="scary.jpg" alt="Screamer">
    </div>
    <div class="flash"></div>
    <div class="flash" style="animation-delay:0.1s; background:#ffffff;"></div>
  </div>

<script>
  // ============ ЗАЩИТА ОТ ЗАКРЫТИЯ ============
  // 1) Не даём закрыть/перезагрузить вкладку
  window.addEventListener('beforeunload', (e) => {
    e.preventDefault();
    e.returnValue = '';
    return '';
  });

  // 2) Блокируем кнопку "назад" (свайп назад на телефоне)
  history.pushState(null, null, location.href);
  window.addEventListener('popstate', () => {
    history.pushState(null, null, location.href);
  });

  // 3) Блокируем правый клик, выделение, перетаскивание
  document.addEventListener('contextmenu', (e) => e.preventDefault());
  document.addEventListener('selectstart', (e) => e.preventDefault());
  document.addEventListener('dragstart', (e) => e.preventDefault());

  // 4) Блокируем жесты "назад" свайпом
  document.addEventListener('touchstart', (e) => {
    if (e.touches.length > 1) e.preventDefault();
  }, { passive: false });

  let lastTouch = 0;
  document.addEventListener('touchend', (e) => {
    const now = Date.now();
    if (now - lastTouch < 300) e.preventDefault();
    lastTouch = now;
  }, { passive: false });

  // 5) Не даём выйти по Escape, F5, Ctrl+W и т.д.
  document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape' || e.key === 'F5' ||
        (e.ctrlKey && ['w','W','r','R','n','N','t','T'].includes(e.key))) {
      e.preventDefault();
      e.stopPropagation();
      if (!document.fullscreenElement && document.documentElement.requestFullscreen) {
        document.documentElement.requestFullscreen().catch(()=>{});
      }
      return false;
    }
  });

  // 6) Автоматически возвращаем фуллскрин, если вышли
  document.addEventListener('fullscreenchange', () => {
    if (!document.fullscreenElement && document.documentElement.requestFullscreen) {
      setTimeout(() => {
        document.documentElement.requestFullscreen().catch(()=>{});
      }, 100);
    }
  });

  // ============ ОТСЧЁТ ============
  let count = 10;
  const countdownEl = document.getElementById('countdown');
  const errorScreen = document.getElementById('error-screen');
  const screamer = document.getElementById('screamer');
  const fixBtn = document.getElementById('fixBtn');

  const interval = setInterval(() => {
    count--;
    if (count > 0) {
      countdownEl.textContent = count;
    } else {
      countdownEl.textContent = '0';
      clearInterval(interval);
      setTimeout(() => {
        countdownEl.style.display = 'none';
        errorScreen.style.display = 'flex';
      }, 500);
    }
  }, 1000);

  // ============ ЗВУК ============
  function playScream() {
    const AC = window.AudioContext || window.webkitAudioContext;
    const ctx = new AC();
    if (ctx.state === 'suspended') ctx.resume();

    const master = ctx.createGain();
    master.gain.setValueAtTime(1.0, ctx.currentTime);
    master.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 4);
    master.connect(ctx.destination);

    // 1) Резкий белый шум (шипение/хрип)
    const bufferSize = ctx.sampleRate * 4;
    const buffer = ctx.createBuffer(1, bufferSize, ctx.sampleRate);
    const data = buffer.getChannelData(0);
    for (let i = 0; i < bufferSize; i++) data[i] = Math.random() * 2 - 1;
    const noise = ctx.createBufferSource();
    noise.buffer = buffer;
    const nf = ctx.createBiquadFilter();
    nf.type = 'bandpass';
    nf.frequency.setValueAtTime(2000, ctx.currentTime);
    nf.frequency.exponentialRampToValueAtTime(500, ctx.currentTime + 3);
    nf.Q.value = 0.5;
    const ng = ctx.createGain();
    ng.gain.setValueAtTime(1.0, ctx.currentTime);
    ng.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 3.5);
    noise.connect(nf); nf.connect(ng); ng.connect(master);
    noise.start();

    // 2) Основной вой (sawtooth)
    const osc1 = ctx.createOscillator();
    osc1.type = 'sawtooth';
    osc1.frequency.setValueAtTime(1800, ctx.currentTime);
    osc1.frequency.exponentialRampToValueAtTime(50, ctx.currentTime + 3);
    const og1 = ctx.createGain();
    og1.gain.setValueAtTime(1.0, ctx.currentTime);
    og1.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 3.5);
    osc1.connect(og1); og1.connect(master);
    osc1.start(); osc1.stop(ctx.currentTime + 4);

    // 3) Визг (square)
    const osc2 = ctx.createOscillator();
    osc2.type = 'square';
    osc2.frequency.setValueAtTime(3000, ctx.currentTime);
    osc2.frequency.exponentialRampToValueAtTime(300, ctx.currentTime + 2);
    const og2 = ctx.createGain();
    og2.gain.setValueAtTime(0.8, ctx.currentTime);
    og2.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 2.5);
    osc2.connect(og2); og2.connect(master);
    osc2.start(); osc2.stop(ctx.currentTime + 3);

    // 4) Низкий гул
    const osc3 = ctx.createOscillator();
    osc3.type = 'sine';
    osc3.frequency.setValueAtTime(80, ctx.currentTime);
    osc3.frequency.linearRampToValueAtTime(30, ctx.currentTime + 3);
    const og3 = ctx.createGain();
    og3.gain.setValueAtTime(0.9, ctx.currentTime);
    og3.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 4);
    osc3.connect(og3); og3.connect(master);
    osc3.start(); osc3.stop(ctx.currentTime + 4);

    // Вибрация (агрессивная, долгая)
    if (navigator.vibrate) {
      navigator.vibrate([500,100,500,100,500,100,1000,200,1000]);
    }
  }

  // ============ ЗАПУСК СКРИМЕРА ============
  fixBtn.addEventListener('click', () => {
    errorScreen.style.display = 'none';
    screamer.style.display = 'flex';
    screamer.classList.add('active');
    playScream();

    // Включаем полный экран при клике
    const el = document.documentElement;
    if (el.requestFullscreen) el.requestFullscreen().catch(()=>{});
    else if (el.webkitRequestFullscreen) el.webkitRequestFullscreen();
  });
</script>
</body>
</html>
