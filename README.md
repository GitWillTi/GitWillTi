<h1 align="center">👋 Olá, eu sou Will!</h1>

<p align="center">
  <img src="https://readme-typing-svg.herokuapp.com/?lines=Desenvolvedor+Fullstack;Apaixonado+por+tecnologia;Sempre+aprendendo+coisas+novas&center=true&width=500&height=50" />
</p>

---

🎯 **Sobre mim**

- 🌱 Atualmente estou aprendendo: `IA`, `Next.js`, `DevOps`
- 💻 Trabalho com: `React`, `Node.js`, `Docker`, `PostgreSQL`
- 💬 Pergunte-me sobre: `Desenvolvimento Web`, `APIs`, `UI/UX`
- 📫 Como me encontrar: alveswillian198@gmail.com 
---

📊 **Estatísticas do GitHub**

<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Estatísticas do GitHub — Visualizador</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--muted:#9aa4b2;--accent:#1f8ef1}
    *{box-sizing:border-box;font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial}
    body{margin:0;background:linear-gradient(180deg,#071226 0%, #071229 100%);color:#e6eef6;min-height:100vh;display:flex;align-items:center;justify-content:center;padding:32px}
    .app{width:980px;max-width:100%;}
    header{display:flex;gap:16px;align-items:center;margin-bottom:18px}
    h1{margin:0;font-size:20px}
    .controls{display:flex;gap:8px;margin-left:auto}
    input,button,select{padding:10px 12px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);background:rgba(255,255,255,0.02);color:inherit}
    button{cursor:pointer}
    .grid{display:grid;grid-template-columns:320px 1fr;gap:16px}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:16px;border-radius:12px;box-shadow:0 6px 18px rgba(2,6,23,0.6)}
    .profile{display:flex;flex-direction:column;gap:12px;align-items:center;text-align:center}
    .avatar{width:120px;height:120px;border-radius:14px;overflow:hidden;border:3px solid rgba(255,255,255,0.04)}
    img{max-width:100%;display:block}
    .stats{display:flex;gap:8px;flex-wrap:wrap;justify-content:center}
    .stat{background:rgba(255,255,255,0.02);padding:8px 12px;border-radius:8px}
    .repos-list{max-height:360px;overflow:auto;margin-top:8px}
    .repo{display:flex;justify-content:space-between;padding:8px 6px;border-bottom:1px solid rgba(255,255,255,0.02)}
    .repo a{color:var(--accent);text-decoration:none}
    .main-stats{display:flex;gap:12px;flex-wrap:wrap}
    .big{flex:1;min-width:200px}
    .small{flex:1;min-width:160px}
    .badges{display:flex;gap:8px;flex-wrap:wrap}
    footer{margin-top:12px;color:var(--muted);font-size:13px}
    .loading{opacity:0.8}
    .error{color:#ff8a8a;background:rgba(255,138,138,0.06);padding:8px;border-radius:8px}
    .top-row{display:flex;gap:8px;align-items:center}
  </style>
</head>
<body>
  <div class="app">
    <header>
      <div>
        <h1>Estatísticas do GitHub</h1>
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

---

⚙️ **Tecnologias e Ferramentas**

<p align="center">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" width="40" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/postgresql/postgresql-original.svg" width="40" />
</p>

---

🧠 **Projetos em destaque**

- 💡 [Nome do Projeto 1](https://github.com/SEU_USUARIO/projeto1): descrição rápida.
- 🔧 [Nome do Projeto 2](https://github.com/SEU_USUARIO/projeto2): o que ele faz.
- 🚀 [Nome do Projeto 3](https://github.com/SEU_USUARIO/projeto3): por que é legal.

---

📌 **Frase que me inspira**
> “Escolha um trabalho que você ama, e você nunca terá que trabalhar um dia sequer na vida.” – Confúcio

---

<div align="center">
  <img src="https://media.giphy.com/media/qgQUggAC3Pfv687qPC/giphy.gif" width="300" />
</div>

