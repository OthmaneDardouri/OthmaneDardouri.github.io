<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>League Champion Roulette</title>
  <link href="https://api.fontshare.com/v2/css?f[]=clash-display@600,700&f[]=satoshi@400,500,700&display=swap" rel="stylesheet">
  <style>
    :root {
      --text-xs: clamp(0.75rem, 0.72rem + 0.18vw, 0.875rem);
      --text-sm: clamp(0.875rem, 0.83rem + 0.2vw, 1rem);
      --text-base: clamp(1rem, 0.96rem + 0.22vw, 1.1rem);
      --text-lg: clamp(1.15rem, 1rem + 0.8vw, 1.55rem);
      --text-xl: clamp(1.7rem, 1.3rem + 1.6vw, 2.6rem);
      --text-2xl: clamp(2.3rem, 1.5rem + 3vw, 4.4rem);
      --space-2: .5rem; --space-3: .75rem; --space-4: 1rem; --space-5: 1.25rem; --space-6: 1.5rem; --space-8: 2rem; --space-10: 2.5rem; --space-12: 3rem;
      --bg: #13100d; --surface: #1c1712; --surface-2: #241d17; --border: rgba(211,164,83,.18); --text: #f4e6cf; --muted: #bca88c; --primary: #d3a453; --teal: #18716d;
      --radius: 24px;
      --shadow: 0 20px 60px rgba(0,0,0,.35);
      --font-display: 'Clash Display', sans-serif;
      --font-body: 'Satoshi', sans-serif;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      min-height: 100vh;
      color: var(--text);
      font-family: var(--font-body);
      background:
        radial-gradient(circle at top, rgba(24,113,109,.18), transparent 22%),
        radial-gradient(circle at 80% 18%, rgba(211,164,83,.14), transparent 22%),
        linear-gradient(180deg, #100d0a, #18120d 45%, #100d0a);
      display: grid;
      place-items: center;
      padding: var(--space-6);
    }
    .app {
      width: min(1100px, 100%);
      border: 1px solid var(--border);
      border-radius: 32px;
      background: linear-gradient(180deg, rgba(28,23,18,.96), rgba(22,18,14,.96));
      box-shadow: var(--shadow);
      overflow: hidden;
    }
    .hero {
      display: grid;
      grid-template-columns: 1.05fr .95fr;
      gap: var(--space-8);
      padding: clamp(1.5rem, 4vw, 3rem);
      align-items: center;
    }
    .kicker {
      display: inline-flex; align-items: center; gap: .6rem; padding: .5rem .9rem; border-radius: 999px; border: 1px solid var(--border); color: var(--muted); font-size: var(--text-xs); letter-spacing: .08em; text-transform: uppercase;
    }
    .kicker::before { content: ''; width: 10px; height: 10px; border-radius: 50%; background: var(--teal); box-shadow: 0 0 0 6px rgba(24,113,109,.15); }
    h1 { font-family: var(--font-display); font-size: var(--text-2xl); line-height: .95; letter-spacing: -.04em; margin-top: var(--space-5); max-width: 10ch; }
    .lead { margin-top: var(--space-5); color: var(--muted); max-width: 56ch; }
    .controls { display: flex; flex-wrap: wrap; gap: var(--space-4); margin-top: var(--space-6); }
    button, select {
      min-height: 48px; border-radius: 999px; border: 1px solid var(--border); padding: 0 1rem; color: var(--text); background: rgba(255,255,255,.03); font: inherit;
    }
    button.primary { background: var(--primary); color: #140f0b; border-color: transparent; font-weight: 700; }
    button:hover { transform: translateY(-1px); }
    .visual {
      position: relative; min-height: 480px; border-radius: 28px; overflow: hidden; border: 1px solid var(--border); background: #0e0c0a;
    }
    .visual img { width: 100%; height: 100%; object-fit: cover; }
    .overlay {
      position: absolute; inset: auto 1rem 1rem 1rem; padding: 1rem; border-radius: 22px; backdrop-filter: blur(12px); background: rgba(15,12,10,.72); border: 1px solid var(--border);
    }
    .overlay h2 { font-family: var(--font-display); font-size: var(--text-xl); }
    .overlay p { color: var(--muted); margin-top: .4rem; }
    .meta { display: flex; flex-wrap: wrap; gap: .75rem; margin-top: .9rem; }
    .pill { border: 1px solid var(--border); border-radius: 999px; padding: .45rem .8rem; font-size: var(--text-xs); color: var(--muted); }
    .grid {
      display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: var(--space-5); padding: 0 clamp(1.5rem, 4vw, 3rem) clamp(1.5rem, 4vw, 3rem);
    }
    .card { border: 1px solid var(--border); border-radius: 24px; padding: var(--space-5); background: rgba(255,255,255,.02); }
    .card h3 { font-size: var(--text-sm); text-transform: uppercase; letter-spacing: .08em; color: var(--muted); margin-bottom: .65rem; }
    .card p { color: var(--text); }
    .quote { color: var(--muted); }
    @media (max-width: 860px) {
      .hero, .grid { grid-template-columns: 1fr; }
      .visual { min-height: 380px; }
      h1 { max-width: 12ch; }
    }
  </style>
</head>
<body>
  <main class="app">
    <section class="hero">
      <div>
        <span class="kicker">League side quest</span>
        <h1>Champion roulette for the curious and slightly unhinged.</h1>
        <p class="lead">A tiny side project for your GitHub: press the button, get a random League of Legends champion, and use Riot Data Dragon splash art to make it feel real and fun.</p>
        <div class="controls">
          <button class="primary" id="rollBtn">Roll champion</button>
          <select id="roleFilter" aria-label="Filter by role">
            <option value="ALL">All roles</option>
            <option value="Assassin">Assassin</option>
            <option value="Fighter">Fighter</option>
            <option value="Mage">Mage</option>
            <option value="Marksman">Marksman</option>
            <option value="Support">Support</option>
            <option value="Tank">Tank</option>
          </select>
        </div>
      </div>
      <section class="visual" aria-live="polite">
        <img id="champImg" src="https://ddragon.leagueoflegends.com/cdn/img/champion/splash/Jhin_0.jpg" alt="Champion splash art" width="1215" height="717" loading="lazy" />
        <div class="overlay">
          <h2 id="champName">Jhin</h2>
          <p id="champTitle">The Virtuoso</p>
          <div class="meta" id="champTags"></div>
        </div>
      </section>
    </section>
    <section class="grid">
      <article class="card">
        <h3>Why it works</h3>
        <p>It adds personality to your GitHub without looking random. Recruiters remember playful but well-executed side projects.</p>
      </article>
      <article class="card">
        <h3>How it works</h3>
        <p>Champion data loads from Riot's Data Dragon champion dataset and splash images come directly from the official static asset paths.</p>
      </article>
      <article class="card">
        <h3>Small upgrade ideas</h3>
        <p class="quote">Add lane filters, random team generation, or a “compose the most cursed comp” mode for extra fun.</p>
      </article>
    </section>
  </main>
  <script>
    const version = '14.24.1';
    const dataUrl = `https://ddragon.leagueoflegends.com/cdn/${version}/data/en_US/champion.json`;
    const img = document.getElementById('champImg');
    const nameEl = document.getElementById('champName');
    const titleEl = document.getElementById('champTitle');
    const tagsEl = document.getElementById('champTags');
    const filterEl = document.getElementById('roleFilter');
    const rollBtn = document.getElementById('rollBtn');
    let champions = [];

    const renderChampion = (champ) => {
      img.src = `https://ddragon.leagueoflegends.com/cdn/img/champion/splash/${champ.id}_0.jpg`;
      img.alt = `${champ.name} splash art`;
      nameEl.textContent = champ.name;
      titleEl.textContent = champ.title;
      tagsEl.innerHTML = champ.tags.map(tag => `<span class="pill">${tag}</span>`).join('');
    };

    const randomChampion = () => {
      const role = filterEl.value;
      const pool = role === 'ALL' ? champions : champions.filter(champ => champ.tags.includes(role));
      if (!pool.length) return;
      const selected = pool[Math.floor(Math.random() * pool.length)];
      renderChampion(selected);
    };

    fetch(dataUrl)
      .then(res => res.json())
      .then(data => {
        champions = Object.values(data.data);
        renderChampion(champions.find(c => c.id === 'Jhin') || champions[0]);
      })
      .catch(() => {
        titleEl.textContent = 'Could not load champion data right now.';
      });

    rollBtn.addEventListener('click', randomChampion);
    filterEl.addEventListener('change', randomChampion);
  </script>
</body>
</html>
