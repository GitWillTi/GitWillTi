```html
<!-- index.html -->
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>HACKER HUD — Demo</title>
  <link rel="stylesheet" href="assets/css/hacker.css">
</head>
<body>
  <div class="hud">
    <header class="hud-header">
      <h1 class="glitch" data-text="HACKER HUD">HACKER HUD</h1>
      <div class="badges">
        <span class="badge">v1.2.3</span>
        <span class="badge alt">alpha</span>
      </div>
    </header>

    <main class="panel">
      <aside class="sidebar">
        <nav>
          <a href="#">System</a>
          <a href="#">Network</a>
          <a href="#">Logs</a>
        </nav>
      </aside>

      <section class="display">
        <div class="terminal">$ ./deploy --force</div>
      </section>
    </main>
  </div>
</body>
</html>
```

```css
/* assets/css/hacker.css */
:root{
  --bg:#0b0f14; --panel:#071018; --neon:#00e6b8; --accent:#7c3cff;
  --glass: rgba(255,255,255,0.03); --glass-2: rgba(255,255,255,0.02);
  --mono:'Fira Code', 'JetBrains Mono', ui-monospace, SFMono-Regular, monospace;
}

/* reset */
*{box-sizing:border-box}
html,body{height:100%;margin:0;background:radial-gradient(ellipse at 10% 10%, rgba(124,60,255,0.06), transparent 10%), linear-gradient(180deg,var(--bg),#020406 80%);font-family:var(--mono);color:#cfeff1}

.hud{max-width:980px;margin:32px auto;padding:20px;border-radius:14px;background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);box-shadow:0 8px 40px rgba(2,6,12,0.8);overflow:hidden;position:relative;border:1px solid rgba(255,255,255,0.03)}

/* header */
.hud-header{display:flex;justify-content:space-between;align-items:center;padding:18px}
.glitch{font-size:34px;letter-spacing:1px;position:relative}
.glitch::after{content:attr(data-text);position:absolute;left:2px;top:0;color:var(--neon);mix-blend-mode:screen;filter:blur(0.6px);opacity:0.85;transform:translateZ(0)}

@keyframes glitch-anim{
  0%{transform:translateX(0)}
  10%{transform:translateX(-2px) skewX(-1deg)}
  20%{transform:translateX(2px) skewX(1deg)}
  30%{transform:translateX(-1px)}
  100%{transform:none}
}
.glitch{animation:glitch-anim 2.6s infinite steps(2)}

/* holographic visor */
.panel{display:grid;grid-template-columns:220px 1fr;gap:18px;padding:18px}
.sidebar{background:linear-gradient(180deg,var(--glass),transparent);padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.03);backdrop-filter: blur(6px)}
.display{background:linear-gradient(135deg, rgba(124,60,255,0.02), rgba(0,230,184,0.02));padding:18px;border-radius:10px;min-height:220px;position:relative}

/* terminal */
.terminal{font-family:var(--mono);padding:16px;border-radius:8px;background:linear-gradient(180deg, #001217, #001518);color:#9fffe9;box-shadow:inset 0 1px 0 rgba(255,255,255,0.02);border:1px solid rgba(0,255,200,0.06)}
.terminal::before{content:'$';opacity:0.6;margin-right:8px}

/* badges */
.badge{display:inline-block;padding:6px 10px;margin-left:8px;border-radius:999px;background:linear-gradient(90deg,#061622,#042a2f);border:1px solid rgba(255,255,255,0.03);font-size:12px}
.badge.alt{background:linear-gradient(90deg,#12101f,#2b004a);}

/* hologram shimmer */
.display::after{content:'';position:absolute;left:-40%;top:-60%;width:200%;height:200%;background:conic-gradient(from 90deg at 50% 50%, rgba(124,60,255,0.08), rgba(0,230,184,0.04), transparent);transform:rotate(12deg);pointer-events:none;mix-blend-mode:overlay}

/* responsive */
@media (max-width:720px){.panel{grid-template-columns:1fr}.hud{margin:16px}}

/* accessibility */
@media (prefers-reduced-motion:reduce){.glitch, .glitch::after{animation:none}}
```
