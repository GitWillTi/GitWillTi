<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Estatísticas do GitHub — Visualizador</title>
  <style>
  body {
  background: #000;
  color: #0aff0a;
  font-family: 'Courier New', monospace;
  overflow: hidden;
}

  .container {
    text-align: center;
    margin-top: 120px;
    transform-style: preserve-3d;
    animation: float 6s ease-in-out infinite;
  }

  @keyframes float {
    0% { transform: translateY(0) rotateX(0deg) rotateY(0deg); }
    50% { transform: translateY(-20px) rotateX(8deg) rotateY(8deg); }
    100% { transform: translateY(0) rotateX(0deg) rotateY(0deg); }
  }

  h1 {
    font-size: 4rem;
    text-shadow: 0 0 20px #00f7ff, 0 0 40px #00eaff;
    animation: neonPulse 2s infinite alternate;
  }

  @keyframes neonPulse {
    0% { text-shadow: 0 0 10px #00eaff, 0 0 20px #00f7ff; }
    100% { text-shadow: 0 0 30px #00f7ff, 0 0 60px #00faff; }
  }

  .card {
    width: 380px;
    margin: 40px auto;
    padding: 25px;
    border-radius: 20px;
    background: rgba(0, 10, 25, 0.35);
    backdrop-filter: blur(12px);
    border: 2px solid rgba(0, 255, 255, 0.3);
    box-shadow: 0 0 20px #00faff80, inset 0 0 20px #00eaff40;
    transform-style: preserve-3d;
    transform: rotateY(-10deg);
    animation: holoSpin 8s linear infinite;
  }

  @keyframes holoSpin {
    0% { transform: rotateY(-10deg); }
    50% { transform: rotateY(10deg); }
    100% { transform: rotateY(-10deg); }
  }

  .glow {
    font-size: 1.3rem;
    text-shadow: 0 0 10px #00faff;
  }

  a {
    color: #00faff;
    text-decoration: none;
    font-weight: bold;
  }
  a:hover {
    text-shadow: 0 0 10px #00faff;
  }
  /* Holographic distortion effect */
  @keyframes holoDistort {
    0% { clip-path: polygon(0% 0%, 100% 2%, 100% 98%, 0% 100%); filter: hue-rotate(0deg) blur(0px); }
    25% { clip-path: polygon(0% 2%, 100% 0%, 100% 96%, 0% 98%); filter: hue-rotate(30deg) blur(1px); }
    50% { clip-path: polygon(0% 1%, 100% 3%, 100% 99%, 0% 97%); filter: hue-rotate(60deg) blur(0px); }
    75% { clip-path: polygon(0% 3%, 100% 1%, 100% 97%, 0% 99%); filter: hue-rotate(120deg) blur(1px); }
    100% { clip-path: polygon(0% 0%, 100% 2%, 100% 98%, 0% 100%); filter: hue-rotate(0deg) blur(0px); }
  }

  .holo-text {
    animation: holoDistort 3s infinite ease-in-out;
    display: inline-block;
  }
  /* Matrix rain background */
  canvas#matrixRain {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    background: black;
  }

  /* Matrix holographic text */
  .matrix-holo {
    text-shadow: 0 0 10px #00ff41, 0 0 20px #00ff41;
    animation: matrixGlitch 2.5s infinite;
  }

  @keyframes matrixGlitch {
    0% { filter: hue-rotate(0deg); }
    25% { filter: hue-rotate(30deg); }
    50% { filter: hue-rotate(60deg); }
    75% { filter: hue-rotate(120deg); }
    100% { filter: hue-rotate(0deg); }
  }
</style>

<canvas id="matrixRain"></canvas>
<script>
  // MATRIX RAIN EFFECT
  const canvas = document.getElementById('matrixRain');
  const ctx = canvas.getContext('2d');

  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789".split("");
  const fontSize = 16;
  const columns = canvas.width / fontSize;
  const drops = [];

  for (let i = 0; i < columns; i++) drops[i] = 1;

  function draw() {
    ctx.fillStyle = "rgba(0, 0, 0, 0.05)";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    ctx.fillStyle = "#00ff41";
    ctx.font = fontSize + "px monospace";

    for (let i = 0; i < drops.length; i++) {
      const text = letters[Math.floor(Math.random() * letters.length)];
      ctx.fillText(text, i * fontSize, drops[i] * fontSize);

      if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) drops[i] = 0;

      drops[i]++;
    }
  }

  setInterval(draw, 33);
</script>
</head>
<body>
  <div class="app">
    <header>
      <div>
        <h1 class="matrix-holo" class="holo-text">Estatísticas do GitHub</h1>
        <div style="color:var(--muted);font-size:13px">Insira um usuário GitHub para exibir estatísticas públicas</div>
      </div>
      <div class="controls">
        <input id="username" placeholder="ex: torvalds" />
        <input id="token" placeholder="(opcional) token pessoal — aumenta limite" />
        <button id="btnFetch">Buscar</button>
      </div>
    </header>

    <div id="result" class="grid">
      <div class="card" id="leftCard">
        <div id="profileArea" class="profile">
          <div class="avatar"><img id="avatarImg" src="" alt="avatar"/></div>
          <div id="nameLogin"></div>
          <div id="bio" style="color:var(--muted);font-size:13px"></div>
          <div class="stats" id="topStats"></div>
          <div class="badges" id="badges"></div>
        </div>
      </div>

      <div class="card" id="rightCard">
        <div class="top-row">
          <div style="flex:1">
            <div style="font-weight:600">Resumo</div>
            <div id="summary" style="color:var(--muted);font-size:13px">Nenhuma busca ainda</div>
          </div>
          <div>
            <button id="btnExport">Exportar JSON</button>
          </div>
        </div>

        <hr style="margin:12px 0;border:none;border-top:1px solid rgba(255,255,255,0.03)" />

        <div class="main-stats">
          <div class="big card" style="padding:12px">
            <div style="font-weight:600">Repositórios (top 10)</div>
            <div id="repos" class="repos-list"></div>
          </div>

          <div class="small card" style="padding:12px">
            <div style="font-weight:600">Estatísticas agregadas</div>
            <div style="margin-top:8px;color:var(--muted);font-size:13px" id="aggStats"></div>
            <div style="margin-top:12px;font-weight:600">Linguagens</div>
            <div id="languages" style="margin-top:8px;color:var(--muted);font-size:13px"></div>
          </div>
        </div>

        <footer>
          Nota: a maioria das estatísticas são públicas. Use um token GitHub (scopes mínimos: public_repo) se atingir limites de requisição.
        </footer>
      </div>
    </div>
  </div>

  <script>
    // Helper: cria elemento com texto
    const $ = (sel) => document.querySelector(sel);
    const btn = $('#btnFetch');
    const usernameInput = $('#username');
    const tokenInput = $('#token');

    btn.addEventListener('click', () => run());
    usernameInput.addEventListener('keydown', (e)=>{ if(e.key==='Enter') run(); });

    async function run(){
      const user = usernameInput.value.trim();
      const token = tokenInput.value.trim();
      if(!user){ alert('Informe um usuário GitHub.'); return; }
      setLoading(true);
      clearUI();
      try{
        const headers = token ? { Authorization: 'token '+token } : {};
        // 1) busca infos do usuário
        const userRes = await fetch(`https://api.github.com/users/${encodeURIComponent(user)}`, {headers});
        if(userRes.status===404) throw new Error('Usuário não encontrado');
        if(userRes.status===401) throw new Error('Token inválido');
        if(!userRes.ok) throw new Error('Erro ao buscar usuário: '+userRes.status);
        const userData = await userRes.json();

        // 2) busca todos os repositórios (paginação)
        let page = 1; const per_page = 100; let repos = [];
        while(true){
          const url = `https://api.github.com/users/${encodeURIComponent(user)}/repos?per_page=${per_page}&page=${page}&sort=updated`;
          const r = await fetch(url, {headers});
          if(!r.ok) throw new Error('Erro ao buscar repositórios: '+r.status);
          const chunk = await r.json();
          repos = repos.concat(chunk);
          if(chunk.length < per_page) break;
          page++;
          // segurança: previne loop infinito
          if(page>20) break;
        }

        // Agrega dados
        let totalStars = 0, totalForks = 0, totalOpenIssues = 0;
        const langMap = {};
        for(const repo of repos){
          totalStars += repo.stargazers_count || 0;
          totalForks += repo.forks_count || 0;
          totalOpenIssues += repo.open_issues_count || 0;
          if(repo.language){ langMap[repo.language] = (langMap[repo.language]||0) + 1; }
        }

        // Ordena repositórios por estrelas
        const topRepos = repos.slice().sort((a,b)=> (b.stargazers_count||0) - (a.stargazers_count||0)).slice(0,10);

        // Exibe
        $('#avatarImg').src = userData.avatar_url || '';
        $('#nameLogin').innerHTML = `<div style="font-weight:700">${escapeHtml(userData.name || '')}</div><div style="color:var(--muted)">@${escapeHtml(userData.login)}</div>`;
        $('#bio').textContent = userData.bio || '';
        $('#topStats').innerHTML = '';
        addStat(`${userData.public_repos} repos`);
        addStat(`${totalStars} ★`);
        addStat(`${userData.followers} seguidores`);
        addStat(`${userData.following} seguindo`);

        $('#badges').innerHTML = '';
        if(userData.company) addBadge(userData.company);
        if(userData.location) addBadge(userData.location);
        if(userData.blog) addBadge(`<a href="${escapeHtml(userData.blog)}" target="_blank">site</a>`);

        $('#summary').textContent = `${userData.login} — ${userData.public_repos} repositórios públicos, ${totalStars} estrelas total, ${totalForks} forks.`;

        // repos list
        const reposDiv = $('#repos');
        reposDiv.innerHTML = '';
        for(const r of topRepos){
          const el = document.createElement('div'); el.className='repo';
          el.innerHTML = `<div style="max-width:70%"><a href="${r.html_url}" target="_blank">${escapeHtml(r.name)}</a> <div style="color:var(--muted);font-size:12px">${escapeHtml(r.description||'')}</div></div><div style="text-align:right;color:var(--muted);font-size:13px">★ ${r.stargazers_count || 0} · Forks ${r.forks_count || 0}</div>`;
          reposDiv.appendChild(el);
        }

        // agregados
        $('#aggStats').innerHTML = `Estrelas totais: <strong>${totalStars}</strong><br/>Forks totais: <strong>${totalForks}</strong><br/>Issues abertas (soma): <strong>${totalOpenIssues}</strong><br/>Repositórios públicos: <strong>${userData.public_repos}</strong>`;

        // linguagens
        const languages = Object.entries(langMap).sort((a,b)=>b[1]-a[1]).slice(0,10);
        $('#languages').innerHTML = languages.map(l=>`${escapeHtml(l[0])}: ${l[1]} repos`).join('<br/>') || '<span style="color:var(--muted)">Nenhuma linguagem detectada</span>';

        // export
        $('#btnExport').onclick = ()=>{
          const payload = { user: userData, repos, totals: {stars:totalStars,forks:totalForks,issues:totalOpenIssues}, languages: langMap };
          const blob = new Blob([JSON.stringify(payload,null,2)],{type:'application/json'});
          const url = URL.createObjectURL(blob);
          const a = document.createElement('a'); a.href=url; a.download = `${userData.login||user}_github_stats.json`; a.click(); URL.revokeObjectURL(url);
        };

      }catch(err){
        console.error(err);
        const right = $('#rightCard');
        right.querySelector('#summary').textContent = 'Erro: '+err.message;
        if(!$('#rightCard').querySelector('.error')){
          const e = document.createElement('div'); e.className='error'; e.textContent = err.message; $('#rightCard').prepend(e);
        }
      } finally { setLoading(false); }
    }

    function addStat(text){ const d=document.createElement('div'); d.className='stat'; d.innerHTML=text; $('#topStats').appendChild(d); }
    function addBadge(text){ const d=document.createElement('div'); d.className='stat'; d.innerHTML=text; $('#badges').appendChild(d); }
    function clearUI(){ $('#avatarImg').src=''; $('#nameLogin').innerHTML=''; $('#bio').textContent=''; $('#topStats').innerHTML=''; $('#badges').innerHTML=''; $('#repos').innerHTML=''; $('#aggStats').innerHTML=''; $('#languages').innerHTML=''; const err = document.querySelector('.error'); if(err) err.remove(); }
    function setLoading(v){ document.body.classList.toggle('loading', v); btn.disabled = v; btn.textContent = v? 'Buscando...' : 'Buscar'; }
    function escapeHtml(s){ if(!s) return ''; return s.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;'); }
  </script>
</body>
</html>

