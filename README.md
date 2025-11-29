<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>HACKER HUD — Demo</title>

  <style>
    :root{
      --bg:#0b0f14; 
      --panel:#071018; 
      --neon:#00e6b8; 
      --accent:#7c3cff;
      --glass:rgba(255,255,255,0.03); 
      --mono:'Fira Code','JetBrains Mono',monospace;
    }

    *{ box-sizing:border-box; margin:0; padding:0; }

    html,body{
      background:radial-gradient(ellipse at 10% 10%, rgba(124,60,255,0.07), transparent),
                linear-gradient(180deg, var(--bg), #020406);
      font-family:var(--mono);
      color:#d5ffee;
      height:100%;
    }

    .hud{
      max-width:900px;
      margin:40px auto;
      background:rgba(255,255,255,0.03);
      border:1px solid rgba(255,255,255,0.05);
      padding:20px;
      border-radius:12px;
      backdrop-filter:blur(6px);
      box-shadow:0 0 40px rgba(0,0,0,0.7);
    }

    .hud-header{
      display:flex;
      justify-content:space-between;
      align-items:center;
      margin-bottom:20px;
    }

    .glitch{
      position:relative;
      font-size:34px;
    }
    .glitch::after{
      content:attr(data-text);
      position:absolute;
      left:2px;
      top:0;
      color:var(--neon);
      opacity:.8;
      mix-blend-mode:screen;
    }

    @keyframes glitch-anim {
      0%{ transform:translateX(0); }
      10%{ transform:translateX(-2px); }
      20%{ transform:translateX(2px); }
      30%{ transform:translateX(-1px); }
      100%{ transform:none; }
    }
    .glitch{
      animation:glitch-anim 2.4s infinite steps(2);
    }

    .panel{
      display:grid;
      grid-template-columns:230px 1fr;
      gap:18px;
    }

    .sidebar{
      background:var(--glass);
      padding:14px;
      border-radius:10px;
      border:1px solid rgba(255,255,255,0.05);
    }
    .sidebar nav a{
      display:block;
      padding:8px 0;
      color:#9fffe9;
      text-decoration:none;
      opacity:.8;
    }
    .sidebar nav a:hover{
      opacity:1;
    }

    .display{
      background:rgba(255,255,255,0.02);
      border-radius:10px;
      padding:20px;
      position:relative;
    }

    .terminal{
      padding:16px;
      background:#001216;
      border:1px solid rgba(0,255,200,0.1);
      border-radius:8px;
      color:#8fffe2;
    }
    .terminal::before{
      content:'$ ';
      opacity:.7;
    }

    .badge{
      background:#052027;
      padding:6px 12px;
      border-radius:20px;
      font-size:12px;
      margin-left:8px;
    }
    .badge.alt{
      background:#160028;
    }

    @media(max-width:720px){
      .panel{ grid-template-columns:1fr; }
    }
  </style>
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
        <div class="terminal">./deploy --force</div>
      </section>
    </main>
  </div>

</body>
</html>
