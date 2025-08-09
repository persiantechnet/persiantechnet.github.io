<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <title>HTML PRO Living v1.0.0</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      background: black;
      font-family: monospace;
      color: white;
    }

    #loader {
      position: absolute;
      width: 100%;
      height: 100%;
      background: black;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      font-size: 2em;
      color: white;
      z-index: 10;
    }

    #progressBar {
      width: 80%;
      height: 10px;
      background: #333;
      border-radius: 5px;
      overflow: hidden;
      margin-top: 20px;
    }

    #progressFill {
      width: 0%;
      height: 100%;
      background: linear-gradient(90deg, red, orange, yellow, green, blue, indigo, violet);
      transition: width 0.1s ease;
    }

    #introText {
      position: absolute;
      width: 100%;
      height: 100%;
      background: black;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 3em;
      color: white;
      z-index: 9;
      opacity: 0;
      transition: opacity 1s ease;
    }

    #versionText {
      position: absolute;
      top: 60%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 1.5em;
      color: white;
      text-shadow: 0 0 10px #0ff;
      z-index: 3;
      display: none;
    }

    #footer {
      position: absolute;
      bottom: 10px;
      left: 50%;
      transform: translateX(-50%);
      font-size: 1em;
      color: #00ffcc;
      text-shadow: 0 0 5px #0ff;
      z-index: 3;
      display: none;
    }

    #clock {
      position: absolute;
      top: 45%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 2em;
      z-index: 3;
      animation: rainbow 5s infinite linear;
      text-shadow: 0 0 10px red;
      display: none;
    }

    #dateDisplay {
      position: absolute;
      top: 35%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 1em;
      color: #00ffcc;
      text-shadow: 0 0 5px #0ff;
      z-index: 3;
      display: none;
    }

    @keyframes rainbow {
      0% { color: red; }
      20% { color: orange; }
      40% { color: yellow; }
      60% { color: green; }
      80% { color: blue; }
      100% { color: violet; }
    }

    canvas {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 1;
    }
  </style>
</head>
<body>

<div id="loader">
  در حال بارگذاری... <span id="percent">0%</span>
  <div id="progressBar"><div id="progressFill"></div></div>
</div>

<div id="introText">HTML PRO LIVING</div>
<div id="dateDisplay"></div>
<div id="clock"></div>
<div id="versionText">Html Pro Living v1.0.0</div>
<div id="footer">Powered By 📡 PersianTechNet | ©2013 - 2025</div>
<canvas id="matrixCanvas"></canvas>

<script>
  // لودر درصدی با نوار
  let percent = 0;
  const loader = document.getElementById('loader');
  const percentSpan = document.getElementById('percent');
  const progressFill = document.getElementById('progressFill');
  const introText = document.getElementById('introText');
  const versionText = document.getElementById('versionText');
  const footer = document.getElementById('footer');
  const clock = document.getElementById('clock');
  const dateDisplay = document.getElementById('dateDisplay');

  const loadInterval = setInterval(() => {
    percent += 2;
    percentSpan.textContent = percent + '%';
    progressFill.style.width = percent + '%';
    if (percent >= 100) {
      clearInterval(loadInterval);
      loader.style.display = 'none';
      introText.style.opacity = 1;
      setTimeout(() => {
        introText.style.opacity = 0;
        setTimeout(() => {
          introText.style.display = 'none';
          versionText.style.display = 'block';
          footer.style.display = 'block';
          clock.style.display = 'block';
          dateDisplay.style.display = 'block';
          startMatrix();
          updateClock();
          updateDate();
        }, 1000);
      }, 3000);
    }
  }, 100);

  // ماتریکس بارش
  function startMatrix() {
    const canvas = document.getElementById('matrixCanvas');
    const ctx = canvas.getContext('2d');

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    ctx.font = '16px monospace';

    const letters = ['0', '1'];
    const fontSize = 16;
    const columns = canvas.width / fontSize;
    const drops = Array(Math.floor(columns)).fill(1);

    function rainbowColor(i) {
      const hue = (i * 10) % 360;
      return `hsl(${hue}, 100%, 50%)`;
    }

    function drawMatrix() {
      ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      for (let i = 0; i < drops.length; i++) {
        const text = letters[Math.floor(Math.random() * letters.length)];
        ctx.fillStyle = rainbowColor(i);
        ctx.fillText(text, i * fontSize, drops[i] * fontSize);

        drops[i]++;
        if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
          drops[i] = 0;
        }
      }
    }

    setInterval(drawMatrix, 50);
  }

  // ساعت دیجیتال
  function updateClock() {
    const now = new Date();
    const time = now.toLocaleTimeString();
    clock.textContent = time;
    setTimeout(updateClock, 1000);
  }

  // تاریخ شمسی و میلادی
  function updateDate() {
    const now = new Date();
    const miladi = now.toLocaleDateString('en-GB');
    const shamsi = new Intl.DateTimeFormat('fa-IR').format(now);
    dateDisplay.textContent = `${shamsi} / ${miladi}`;
  }

  // افکت انفجار با لمس
  document.addEventListener('click', function(e) {
    const canvas = document.getElementById('matrixCanvas');
    const ctx = canvas.getContext('2d');
    const particles = [];

    for (let i = 0; i < 100; i++) {
      particles.push({
        x: e.clientX,
        y: e.clientY,
        dx: (Math.random() - 0.5) * 4,
        dy: (Math.random() - 0.5) * 4,
        life: 100,
        color: `hsl(${Math.random() * 360}, 100%, 50%)`,
        char: Math.random() > 0.5 ? '0' : '1'
      });
    }

    const explosion = setInterval(() => {
      ctx.globalAlpha = 1;
      ctx.fillStyle = 'rgba(0,0,0,0.1)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      for (let p of particles) {
        ctx.fillStyle = p.color;
        ctx.fillText(p.char, p.x, p.y);
        p.x += p.dx;
        p.y += p.dy;
        p.life -= 2;
      }

      ctx.globalAlpha = 1;

      if (particles.every(p => p.life <= 0)) clearInterval(explosion);
    }, 30);
  });
</script>

</body>
</html>
