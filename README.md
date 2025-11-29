<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Hacker Profile</title>
<style>
/* ======== GLOBAL ======== */
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');
body {
  margin: 0;
  padding: 0;
  background: #000;
  color: #00ff9d;
  font-family: 'Share Tech Mono', monospace;
  overflow-x: hidden;
}

/* ======== MATRIX BACKGROUND ======== */
canvas#matrix {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: -1;
  background: #000;
}

/* ======== GLITCH TITLE ======== */
.glitch {
  font-size: 3rem;
  font-weight: bold;
  position: relative;
  text-align: center;
  margin-top: 60px;
  letter-spacing: 2px;
}
.glitch:before, .glitch:after {
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.glitch:before {
  left: 2px;
  text-shadow: -2px 0 red;
  animation: glitch-1 2s infinite linear alternate-reverse;
}
.glitch:after {
  left: -2px;
  text-shadow: -2px 0 blue;
  animation: glitch-2 2s infinite linear alternate-reverse;
}

@keyframes glitch-1 {
  0% { clip-path: inset(0 0 85% 0); }
  20% { clip-path: inset(40% 0 20% 0); }
  40% { clip-path: inset(80% 0 10% 0); }
  60% { clip-path: inset(10% 0 60% 0); }
  80% { clip-path: inset(30% 0 50% 0); }
  100% { clip-path: inset(0 0 85% 0); }
}
@keyframes glitch-2 {
  0% { clip-path: inset(10% 0 70% 0); }
  20% { clip-path: inset(60% 0 10% 0); }
  40% { clip-path: inset(20% 0 40% 0); }
  60% { clip-path: inset(70% 0 20% 0); }
  80% { clip-path: inset(40% 0 30% 0); }
  100% { clip-path: inset(10% 0 70% 0); }
}

/* ======== CARD ======== */
.card {
  width: 80%;
  max-width: 700px;
  margin: 40px auto;
  padding: 20px;
  background: rgba(0, 0, 0, 0.6);
  border: 1px solid #00ff9d;
  border-radius: 10px;
  box-shadow: 0 0 15px #00ff9d;
  backdrop-filter: blur(5px);
}
.card h2 {
  text-align: center;
  margin-bottom: 15px;
}
.card p {
  line-height: 1.6;
}

/* ======== BADGES ======== */
.badges {
  text-align: center;
  margin-top: 20px;
}
.badge {
  display: inline-block;
  margin: 8px;
  padding: 10px 18px;
  border: 1px solid #00ff9d;
  border-radius: 5px;
  text-transform: uppercase;
  font-size: 0.9rem;
  box-shadow: 0 0 10px #00ff9d;
}
</style>
</head>
<body>
<canvas id="matrix"></canvas>

<div class="glitch" data-text="HACKER PROFILE">HACKER PROFILE</div>

<div class="card">
  <h2>Sobre Mim</h2>
  <p>Desenvolvedor focado em alta performance, segurança e estilo hacker. Criando interfaces futuristas, animações complexas e projetos que parecem saídos de um terminal sci-fi.</p>

  <div class="badges">
    <div class="badge">Developer</div>
    <div class="badge">Cyberpunk UI</div>
    <div class="badge">Dark Neon</div>
  </div>
</div>

<script>
// === MATRIX EFFECT ===
const canvas = document.getElementById('matrix');
const ctx = canvas.getContext('2d');
canvas.height = window.innerHeight;
canvas.width = window.innerWidth;

const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
const arr = Array(Math.floor(canvas.width / 20)).fill(0);

setInterval(() => {
  ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  ctx.fillStyle = '#00ff9d';
  ctx.font = '15px monospace';

  arr.forEach((y, index) => {
    const text = letters.charAt(Math.floor(Math.random() * letters.length));
    const x = index * 20;
    ctx.fillText(text, x, y);

    arr[index] = y > canvas.height + Math.random() * 100 ? 0 : y + 20;
  });
}, 50);
</script>

</body>
</html>
