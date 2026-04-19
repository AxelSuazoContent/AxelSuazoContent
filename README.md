<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
  @import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;500;600&family=Space+Grotesk:wght@300;400;500;600;700&display=swap');

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: #0d1117;
    color: #c9d1d9;
    font-family: 'Space Grotesk', sans-serif;
    overflow-x: hidden;
    min-height: 100vh;
  }

  /* ===== CANVAS PARTICLES ===== */
  #particle-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    pointer-events: none;
    z-index: 0;
  }

  .container {
    position: relative;
    z-index: 1;
    max-width: 860px;
    margin: 0 auto;
    padding: 40px 24px 60px;
  }

  /* ===== HERO ===== */
  .hero {
    text-align: center;
    padding: 40px 0 30px;
    animation: fadeInDown 0.8s ease both;
  }

  .avatar-wrap {
    position: relative;
    display: inline-block;
    margin-bottom: 20px;
  }

  .avatar {
    width: 110px;
    height: 110px;
    border-radius: 50%;
    background: linear-gradient(135deg, #1f6feb, #58a6ff, #79c0ff, #1f6feb);
    background-size: 300% 300%;
    animation: gradientSpin 4s linear infinite;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 42px;
    font-weight: 700;
    color: #fff;
    font-family: 'Fira Code', monospace;
    position: relative;
    z-index: 1;
    text-shadow: 0 0 20px rgba(255,255,255,0.4);
  }

  .avatar-ring {
    position: absolute;
    top: -8px; left: -8px;
    width: 126px; height: 126px;
    border-radius: 50%;
    border: 2px solid transparent;
    border-top-color: #58a6ff;
    border-right-color: #3fb950;
    border-bottom-color: #f78166;
    border-left-color: #d2a8ff;
    animation: spin 3s linear infinite;
  }

  .avatar-ring2 {
    position: absolute;
    top: -16px; left: -16px;
    width: 142px; height: 142px;
    border-radius: 50%;
    border: 1px solid rgba(88,166,255,0.2);
    animation: spin 6s linear infinite reverse;
  }

  @keyframes gradientSpin {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  @keyframes spin {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }

  .hero-name {
    font-size: 42px;
    font-weight: 700;
    background: linear-gradient(90deg, #58a6ff, #79c0ff, #a5f3fc, #58a6ff);
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: shimmer 3s linear infinite;
    letter-spacing: -1px;
    margin-bottom: 6px;
  }

  @keyframes shimmer {
    0% { background-position: 0% center; }
    100% { background-position: 200% center; }
  }

  .hero-title {
    font-family: 'Fira Code', monospace;
    font-size: 15px;
    color: #3fb950;
    margin-bottom: 14px;
    animation: blink 1.2s step-end infinite;
  }

  .hero-title::after {
    content: '|';
    animation: blink 1s step-end infinite;
  }

  @keyframes blink {
    50% { opacity: 0; }
  }

  .hero-desc {
    font-size: 14px;
    color: #8b949e;
    max-width: 520px;
    margin: 0 auto;
    line-height: 1.7;
  }

  /* ===== STATUS BADGES ===== */
  .badges {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
    margin: 20px 0;
    animation: fadeIn 1s ease 0.3s both;
  }

  .badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: rgba(88,166,255,0.08);
    border: 1px solid rgba(88,166,255,0.25);
    border-radius: 20px;
    padding: 5px 14px;
    font-size: 12px;
    color: #58a6ff;
    font-family: 'Fira Code', monospace;
    transition: all 0.3s;
    cursor: default;
    position: relative;
    overflow: hidden;
  }

  .badge::before {
    content: '';
    position: absolute;
    top: 0; left: -100%;
    width: 100%; height: 100%;
    background: linear-gradient(90deg, transparent, rgba(88,166,255,0.15), transparent);
    transition: left 0.4s;
  }

  .badge:hover::before { left: 100%; }
  .badge:hover {
    background: rgba(88,166,255,0.15);
    border-color: rgba(88,166,255,0.6);
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(88,166,255,0.2);
  }

  .badge.green { color: #3fb950; border-color: rgba(63,185,80,0.25); background: rgba(63,185,80,0.08); }
  .badge.green:hover { background: rgba(63,185,80,0.15); border-color: rgba(63,185,80,0.6); box-shadow: 0 4px 12px rgba(63,185,80,0.2); }
  .badge.purple { color: #d2a8ff; border-color: rgba(210,168,255,0.25); background: rgba(210,168,255,0.08); }
  .badge.purple:hover { background: rgba(210,168,255,0.15); border-color: rgba(210,168,255,0.6); box-shadow: 0 4px 12px rgba(210,168,255,0.2); }
  .badge.orange { color: #ffa657; border-color: rgba(255,166,87,0.25); background: rgba(255,166,87,0.08); }
  .badge.orange:hover { background: rgba(255,166,87,0.15); border-color: rgba(255,166,87,0.6); box-shadow: 0 4px 12px rgba(255,166,87,0.2); }

  /* ===== DIVIDER ===== */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(88,166,255,0.4), rgba(63,185,80,0.4), transparent);
    margin: 28px 0;
    animation: dividerFlow 4s ease infinite;
  }

  @keyframes dividerFlow {
    0%, 100% { opacity: 0.5; }
    50% { opacity: 1; }
  }

  /* ===== SECTION TITLES ===== */
  .section-title {
    font-size: 13px;
    font-family: 'Fira Code', monospace;
    color: #58a6ff;
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 18px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, rgba(88,166,255,0.3), transparent);
  }

  /* ===== ABOUT GRID ===== */
  .about-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 12px;
    animation: fadeInUp 0.8s ease 0.2s both;
  }

  .about-card {
    background: #161b22;
    border: 1px solid #30363d;
    border-radius: 12px;
    padding: 16px 18px;
    display: flex;
    align-items: flex-start;
    gap: 12px;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }

  .about-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0;
    width: 3px; height: 100%;
    background: var(--accent, #58a6ff);
    border-radius: 3px 0 0 3px;
  }

  .about-card:hover {
    border-color: var(--accent, #58a6ff);
    transform: translateX(4px);
    box-shadow: -4px 0 16px rgba(88,166,255,0.1);
  }

  .about-icon {
    font-size: 20px;
    flex-shrink: 0;
    margin-top: 1px;
  }

  .about-text {
    font-size: 13px;
    color: #8b949e;
    line-height: 1.6;
  }

  .about-text strong { color: #c9d1d9; font-weight: 500; }

  /* ===== TECH STACK ===== */
  .tech-section {
    animation: fadeInUp 0.8s ease 0.3s both;
  }

  .tech-category {
    margin-bottom: 20px;
  }

  .tech-cat-label {
    font-family: 'Fira Code', monospace;
    font-size: 11px;
    color: #6e7681;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    margin-bottom: 10px;
    padding-left: 4px;
    border-left: 2px solid #3fb950;
    padding-left: 8px;
  }

  .tech-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }

  .tech-pill {
    display: inline-flex;
    align-items: center;
    gap: 7px;
    background: #161b22;
    border: 1px solid #30363d;
    border-radius: 8px;
    padding: 7px 13px;
    font-size: 12px;
    font-weight: 500;
    color: #c9d1d9;
    cursor: default;
    transition: all 0.25s;
    position: relative;
    overflow: hidden;
  }

  .tech-pill:hover {
    border-color: var(--tc, #58a6ff);
    color: var(--tc, #58a6ff);
    background: rgba(88,166,255,0.06);
    transform: translateY(-3px) scale(1.04);
    box-shadow: 0 6px 16px rgba(0,0,0,0.3);
  }

  .tech-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--tc, #58a6ff);
    flex-shrink: 0;
    animation: pulse 2s ease infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.6; transform: scale(0.8); }
  }

  /* ===== STATS GRID ===== */
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 12px;
    animation: fadeInUp 0.8s ease 0.4s both;
  }

  .stat-card {
    background: #161b22;
    border: 1px solid #30363d;
    border-radius: 12px;
    padding: 20px 16px;
    text-align: center;
    transition: all 0.35s;
    position: relative;
    overflow: hidden;
  }

  .stat-card::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0;
    width: 100%; height: 2px;
    background: linear-gradient(90deg, var(--sc, #58a6ff), transparent);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s;
  }

  .stat-card:hover::after { transform: scaleX(1); }
  .stat-card:hover {
    border-color: var(--sc, #58a6ff);
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.4);
  }

  .stat-num {
    font-size: 30px;
    font-weight: 700;
    font-family: 'Fira Code', monospace;
    color: var(--sc, #58a6ff);
    display: block;
    margin-bottom: 4px;
  }

  .stat-label {
    font-size: 11px;
    color: #6e7681;
    letter-spacing: 0.5px;
    text-transform: uppercase;
  }

  /* ===== TERMINAL ===== */
  .terminal {
    background: #0d1117;
    border: 1px solid #30363d;
    border-radius: 12px;
    overflow: hidden;
    animation: fadeInUp 0.8s ease 0.5s both;
    font-family: 'Fira Code', monospace;
  }

  .terminal-bar {
    background: #161b22;
    padding: 10px 16px;
    display: flex;
    align-items: center;
    gap: 8px;
    border-bottom: 1px solid #30363d;
  }

  .dot { width: 12px; height: 12px; border-radius: 50%; }
  .dot.red { background: #f78166; }
  .dot.yellow { background: #ffa657; }
  .dot.green { background: #3fb950; }

  .terminal-title { font-size: 12px; color: #6e7681; margin-left: 8px; }

  .terminal-body { padding: 18px 20px; line-height: 2.1; font-size: 13px; }
  .prompt { color: #3fb950; }
  .cmd { color: #58a6ff; }
  .output { color: #8b949e; padding-left: 16px; }
  .highlight { color: #d2a8ff; }
  .success { color: #3fb950; }
  .error { color: #f78166; }
  .warn { color: #ffa657; }

  .cursor-line { display: flex; align-items: center; gap: 6px; }
  .cursor { display: inline-block; width: 8px; height: 16px; background: #58a6ff; animation: blink 1s step-end infinite; vertical-align: middle; }

  /* ===== CONTACT ===== */
  .contact-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 10px;
    animation: fadeInUp 0.8s ease 0.6s both;
  }

  .contact-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 9px;
    background: #161b22;
    border: 1px solid #30363d;
    border-radius: 10px;
    padding: 13px 18px;
    font-size: 13px;
    font-weight: 500;
    color: #c9d1d9;
    cursor: pointer;
    transition: all 0.3s;
    text-decoration: none;
    position: relative;
    overflow: hidden;
  }

  .contact-btn::before {
    content: '';
    position: absolute;
    top: 50%; left: 50%;
    width: 0; height: 0;
    background: radial-gradient(circle, var(--cc, rgba(88,166,255,0.15)), transparent 70%);
    transform: translate(-50%, -50%);
    transition: width 0.5s, height 0.5s;
    border-radius: 50%;
  }

  .contact-btn:hover::before { width: 200px; height: 200px; }
  .contact-btn:hover {
    border-color: var(--cc, rgba(88,166,255,0.5));
    color: var(--cc-text, #58a6ff);
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(0,0,0,0.4);
  }

  .contact-icon {
    width: 18px; height: 18px;
    border-radius: 4px;
    display: flex; align-items: center; justify-content: center;
    font-size: 12px;
    background: var(--cc, rgba(88,166,255,0.15));
  }

  /* ===== FOOTER ===== */
  .footer {
    text-align: center;
    padding: 20px 0 0;
    font-family: 'Fira Code', monospace;
    font-size: 11px;
    color: #6e7681;
    animation: fadeIn 1s ease 0.8s both;
  }

  .visitors {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: rgba(63,185,80,0.08);
    border: 1px solid rgba(63,185,80,0.2);
    border-radius: 20px;
    padding: 6px 14px;
    font-size: 12px;
    color: #3fb950;
  }

  .live-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: #3fb950;
    animation: pulse 1.5s ease infinite;
  }

  /* ===== ANIMATIONS ===== */
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  @keyframes fadeInDown {
    from { opacity: 0; transform: translateY(-30px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* Stagger cards */
  .about-card:nth-child(1) { animation: fadeInUp 0.5s ease 0.1s both; }
  .about-card:nth-child(2) { animation: fadeInUp 0.5s ease 0.2s both; }
  .about-card:nth-child(3) { animation: fadeInUp 0.5s ease 0.3s both; }
  .about-card:nth-child(4) { animation: fadeInUp 0.5s ease 0.4s both; }
  .about-card:nth-child(5) { animation: fadeInUp 0.5s ease 0.5s both; }
  .about-card:nth-child(6) { animation: fadeInUp 0.5s ease 0.6s both; }

  .tech-pill:nth-child(odd) { animation-delay: 0.05s; }

  .stat-card:nth-child(1) { animation: fadeInUp 0.5s ease 0.1s both; }
  .stat-card:nth-child(2) { animation: fadeInUp 0.5s ease 0.2s both; }
  .stat-card:nth-child(3) { animation: fadeInUp 0.5s ease 0.3s both; }
  .stat-card:nth-child(4) { animation: fadeInUp 0.5s ease 0.4s both; }
</style>
</head>
<body>

<canvas id="particle-canvas"></canvas>

<div class="container">

  <!-- HERO -->
  <div class="hero">
    <div class="avatar-wrap">
      <div class="avatar-ring2"></div>
      <div class="avatar-ring"></div>
      <div class="avatar">AS</div>
    </div>
    <h1 class="hero-name">Axel Suazo</h1>
    <div class="hero-title">$ Full-Stack Developer & Designer</div>
    <p class="hero-desc">Construyendo experiencias digitales únicas con código limpio, diseño creativo y café ☕</p>

    <div class="badges" style="margin-top:18px;">
      <span class="badge green">🟢 Open to work</span>
      <span class="badge">⚡ React Dev</span>
      <span class="badge purple">🔮 Full-Stack</span>
      <span class="badge orange">🌸 Flower Shop Project</span>
      <span class="badge green">🎓 Learning always</span>
    </div>
  </div>

  <div class="divider"></div>

  <!-- ABOUT -->
  <div class="section-title">💫 about.me</div>
  <div class="about-grid">
    <div class="about-card" style="--accent:#58a6ff">
      <span class="about-icon">🔭</span>
      <div class="about-text">Trabajando en una <strong>web para flower shop</strong> 🌸 con React + Laravel</div>
    </div>
    <div class="about-card" style="--accent:#3fb950">
      <span class="about-icon">🌱</span>
      <div class="about-text">Aprendiendo <strong>JS, Python, PHP, Laravel & Databases</strong></div>
    </div>
    <div class="about-card" style="--accent:#d2a8ff">
      <span class="about-icon">👯</span>
      <div class="about-text">Buscando <strong>colaborar</strong> en proyectos de programación y diseño</div>
    </div>
    <div class="about-card" style="--accent:#ffa657">
      <span class="about-icon">💾</span>
      <div class="about-text">Desarrollando <strong>apps ejecutables offline</strong> con Electron & instaladores</div>
    </div>
    <div class="about-card" style="--accent:#f78166">
      <span class="about-icon">🎧</span>
      <div class="about-text">Fun fact: <strong>debug mejor con música y café</strong> — powered by black magic</div>
    </div>
    <div class="about-card" style="--accent:#79c0ff">
      <span class="about-icon">📫</span>
      <div class="about-text">Email: <strong>axelgeronimo0@gmail.com</strong></div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- TECH STACK -->
  <div class="section-title">💻 tech.stack</div>
  <div class="tech-section">

    <div class="tech-category">
      <div class="tech-cat-label">Frontend</div>
      <div class="tech-grid">
        <div class="tech-pill" style="--tc:#e34f26"><div class="tech-dot" style="background:#e34f26"></div>HTML5</div>
        <div class="tech-pill" style="--tc:#1572b6"><div class="tech-dot" style="background:#1572b6"></div>CSS3</div>
        <div class="tech-pill" style="--tc:#f7df1e"><div class="tech-dot" style="background:#f7df1e"></div>JavaScript</div>
        <div class="tech-pill" style="--tc:#61dafb"><div class="tech-dot" style="background:#61dafb"></div>React</div>
        <div class="tech-pill" style="--tc:#38bdf8"><div class="tech-dot" style="background:#38bdf8"></div>TailwindCSS</div>
        <div class="tech-pill" style="--tc:#000"><div class="tech-dot" style="background:#aaa"></div>Next.js</div>
      </div>
    </div>

    <div class="tech-category">
      <div class="tech-cat-label">Backend & Databases</div>
      <div class="tech-grid">
        <div class="tech-pill" style="--tc:#8993be"><div class="tech-dot" style="background:#8993be"></div>PHP</div>
        <div class="tech-pill" style="--tc:#ff2d20"><div class="tech-dot" style="background:#ff2d20"></div>Laravel</div>
        <div class="tech-pill" style="--tc:#6da55f"><div class="tech-dot" style="background:#6da55f"></div>Node.js</div>
        <div class="tech-pill" style="--tc:#007acc"><div class="tech-dot" style="background:#007acc"></div>SQL / MySQL</div>
        <div class="tech-pill" style="--tc:#4ea94b"><div class="tech-dot" style="background:#4ea94b"></div>MongoDB</div>
        <div class="tech-pill" style="--tc:#336791"><div class="tech-dot" style="background:#336791"></div>PostgreSQL</div>
        <div class="tech-pill" style="--tc:#3776ab"><div class="tech-dot" style="background:#3776ab"></div>Python</div>
        <div class="tech-pill" style="--tc:#ed8b00"><div class="tech-dot" style="background:#ed8b00"></div>Java</div>
        <div class="tech-pill" style="--tc:#00599c"><div class="tech-dot" style="background:#00599c"></div>C++</div>
      </div>
    </div>

    <div class="tech-category">
      <div class="tech-cat-label">Hosting & Deploy</div>
      <div class="tech-grid">
        <div class="tech-pill" style="--tc:#ff6c2c"><div class="tech-dot" style="background:#ff6c2c"></div>Hostinger</div>
        <div class="tech-pill" style="--tc:#000"><div class="tech-dot" style="background:#aaa"></div>Vercel</div>
        <div class="tech-pill" style="--tc:#00c7b7"><div class="tech-dot" style="background:#00c7b7"></div>Netlify</div>
        <div class="tech-pill" style="--tc:#430098"><div class="tech-dot" style="background:#430098"></div>Heroku</div>
        <div class="tech-pill" style="--tc:#0db7ed"><div class="tech-dot" style="background:#0db7ed"></div>Docker</div>
        <div class="tech-pill" style="--tc:#f05032"><div class="tech-dot" style="background:#f05032"></div>Git</div>
      </div>
    </div>

    <div class="tech-category">
      <div class="tech-cat-label">Desktop & Offline Apps</div>
      <div class="tech-grid">
        <div class="tech-pill" style="--tc:#47848f"><div class="tech-dot" style="background:#47848f"></div>Electron.js</div>
        <div class="tech-pill" style="--tc:#ffa500"><div class="tech-dot" style="background:#ffa500"></div>Inno Setup / NSIS</div>
        <div class="tech-pill" style="--tc:#0078d7"><div class="tech-dot" style="background:#0078d7"></div>Windows Installer</div>
        <div class="tech-pill" style="--tc:#3776ab"><div class="tech-dot" style="background:#3776ab"></div>PyInstaller</div>
      </div>
    </div>

    <div class="tech-category">
      <div class="tech-cat-label">Design & Creativity</div>
      <div class="tech-grid">
        <div class="tech-pill" style="--tc:#f24e1e"><div class="tech-dot" style="background:#f24e1e"></div>Figma</div>
        <div class="tech-pill" style="--tc:#31a8ff"><div class="tech-dot" style="background:#31a8ff"></div>Photoshop</div>
        <div class="tech-pill" style="--tc:#ff9a00"><div class="tech-dot" style="background:#ff9a00"></div>Illustrator</div>
        <div class="tech-pill" style="--tc:#00c4cc"><div class="tech-dot" style="background:#00c4cc"></div>Canva</div>
        <div class="tech-pill" style="--tc:#e60000"><div class="tech-dot" style="background:#e60000"></div>AutoCAD</div>
      </div>
    </div>

  </div>

  <div class="divider"></div>

  <!-- TERMINAL -->
  <div class="section-title">🖥️ terminal.axel</div>
  <div class="terminal">
    <div class="terminal-bar">
      <div class="dot red"></div>
      <div class="dot yellow"></div>
      <div class="dot green"></div>
      <span class="terminal-title">axel@dev: ~/portfolio</span>
    </div>
    <div class="terminal-body">
      <div><span class="prompt">axel@dev</span>:<span class="cmd">~$</span> cat about.json</div>
      <div class="output">{</div>
      <div class="output">&nbsp;&nbsp;<span class="highlight">"name"</span>: <span class="success">"Axel Suazo"</span>,</div>
      <div class="output">&nbsp;&nbsp;<span class="highlight">"role"</span>: <span class="success">"Full-Stack Developer"</span>,</div>
      <div class="output">&nbsp;&nbsp;<span class="highlight">"location"</span>: <span class="success">"Honduras 🇭🇳"</span>,</div>
      <div class="output">&nbsp;&nbsp;<span class="highlight">"stack"</span>: [<span class="success">"React"</span>, <span class="success">"Laravel"</span>, <span class="success">"PHP"</span>, <span class="success">"MySQL"</span>],</div>
      <div class="output">&nbsp;&nbsp;<span class="highlight">"hosting"</span>: <span class="success">"Hostinger"</span>,</div>
      <div class="output">&nbsp;&nbsp;<span class="highlight">"offline_apps"</span>: <span class="success">"Electron + Installers"</span>,</div>
      <div class="output">&nbsp;&nbsp;<span class="highlight">"coffee_required"</span>: <span class="warn">true</span></div>
      <div class="output">}</div>
      <br>
      <div><span class="prompt">axel@dev</span>:<span class="cmd">~$</span> php artisan serve</div>
      <div class="output"><span class="success">✓</span> Laravel development server started: http://localhost:8000</div>
      <br>
      <div><span class="prompt">axel@dev</span>:<span class="cmd">~$</span> npm run dev</div>
      <div class="output"><span class="success">✓</span> Ready in 432ms — React + Vite running...</div>
      <br>
      <div class="cursor-line">
        <span class="prompt">axel@dev</span><span>:</span><span class="cmd">~$</span>
        <span class="cursor"></span>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- STATS -->
  <div class="section-title">📊 github.stats</div>
  <div class="stats-grid">
    <div class="stat-card" style="--sc:#58a6ff">
      <span class="stat-num" id="commits">0</span>
      <span class="stat-label">commits</span>
    </div>
    <div class="stat-card" style="--sc:#3fb950">
      <span class="stat-num" id="repos">0</span>
      <span class="stat-label">repos</span>
    </div>
    <div class="stat-card" style="--sc:#d2a8ff">
      <span class="stat-num" id="prs">0</span>
      <span class="stat-label">pull requests</span>
    </div>
    <div class="stat-card" style="--sc:#ffa657">
      <span class="stat-num" id="streak">0</span>
      <span class="stat-label">day streak 🔥</span>
    </div>
  </div>

  <div class="divider"></div>

  <!-- CONTACT -->
  <div class="section-title">🌐 connect.socials</div>
  <div class="contact-grid">
    <a class="contact-btn" style="--cc:rgba(225,48,108,0.6);--cc-text:#e1306c" href="https://www.instagram.com/axelblassehn/">
      <span class="contact-icon" style="--cc:rgba(225,48,108,0.2)">📸</span>Instagram
    </a>
    <a class="contact-btn" style="--cc:rgba(0,119,181,0.6);--cc-text:#0077b5" href="https://www.linkedin.com/in/axel-suazo-833898244/">
      <span class="contact-icon" style="--cc:rgba(0,119,181,0.2)">💼</span>LinkedIn
    </a>
    <a class="contact-btn" style="--cc:rgba(209,52,32,0.6);--cc-text:#ff4444" href="mailto:axelgeronimo0@gmail.com">
      <span class="contact-icon" style="--cc:rgba(209,52,32,0.2)">✉️</span>Email
    </a>
    <a class="contact-btn" style="--cc:rgba(255,0,0,0.6);--cc-text:#ff4444" href="https://www.youtube.com/@axelgeronimo5945">
      <span class="contact-icon" style="--cc:rgba(255,0,0,0.2)">▶️</span>YouTube
    </a>
  </div>

  <div class="divider"></div>

  <!-- FOOTER -->
  <div class="footer">
    <div class="visitors">
      <div class="live-dot"></div>
      <span>Profile visitors: <strong>1,337</strong></span>
    </div>
    <br>
    <span style="font-size:10px;opacity:0.5;">⚡ Powered by black magic · Built with React, PHP, Laravel & ☕</span>
  </div>

</div>

<script>
// PARTICLES
const canvas = document.getElementById('particle-canvas');
const ctx = canvas.getContext('2d');
let particles = [];
let W, H;

function resize() {
  W = canvas.width = window.innerWidth;
  H = canvas.height = window.innerHeight;
}
resize();
window.addEventListener('resize', resize);

const COLORS = ['#58a6ff','#3fb950','#d2a8ff','#ffa657','#f78166','#79c0ff'];

class Particle {
  constructor() { this.reset(); }
  reset() {
    this.x = Math.random() * W;
    this.y = Math.random() * H;
    this.vx = (Math.random() - 0.5) * 0.4;
    this.vy = (Math.random() - 0.5) * 0.4;
    this.r = Math.random() * 1.8 + 0.5;
    this.color = COLORS[Math.floor(Math.random() * COLORS.length)];
    this.alpha = Math.random() * 0.4 + 0.1;
    this.life = 1;
    this.decay = Math.random() * 0.003 + 0.001;
  }
  update() {
    this.x += this.vx;
    this.y += this.vy;
    this.life -= this.decay;
    if (this.life <= 0 || this.x < 0 || this.x > W || this.y < 0 || this.y > H) this.reset();
  }
  draw() {
    ctx.save();
    ctx.globalAlpha = this.alpha * this.life;
    ctx.fillStyle = this.color;
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
  }
}

for (let i = 0; i < 120; i++) particles.push(new Particle());

function drawConnections() {
  for (let i = 0; i < particles.length; i++) {
    for (let j = i + 1; j < particles.length; j++) {
      const dx = particles[i].x - particles[j].x;
      const dy = particles[i].y - particles[j].y;
      const d = Math.sqrt(dx*dx + dy*dy);
      if (d < 80) {
        ctx.save();
        ctx.globalAlpha = (1 - d/80) * 0.06;
        ctx.strokeStyle = particles[i].color;
        ctx.lineWidth = 0.5;
        ctx.beginPath();
        ctx.moveTo(particles[i].x, particles[i].y);
        ctx.lineTo(particles[j].x, particles[j].y);
        ctx.stroke();
        ctx.restore();
      }
    }
  }
}

function animate() {
  ctx.clearRect(0, 0, W, H);
  drawConnections();
  particles.forEach(p => { p.update(); p.draw(); });
  requestAnimationFrame(animate);
}
animate();

// COUNTER ANIMATION
function animateCounter(id, target, duration) {
  const el = document.getElementById(id);
  let start = 0;
  const step = target / (duration / 16);
  const timer = setInterval(() => {
    start += step;
    if (start >= target) { start = target; clearInterval(timer); }
    el.textContent = Math.floor(start).toLocaleString();
  }, 16);
}

setTimeout(() => {
  animateCounter('commits', 342, 1800);
  animateCounter('repos', 28, 1200);
  animateCounter('prs', 57, 1400);
  animateCounter('streak', 21, 1000);
}, 600);

// TYPING EFFECT for terminal last line
const cmds = ['php artisan make:model Flower', 'git push origin main', 'npm run build', 'laravel new proyecto', 'python manage.py runserver'];
let ci = 0, chi = 0;
const cursorEl = document.querySelector('.cursor-line');

function typeCmd() {
  const cmd = cmds[ci % cmds.length];
  if (chi <= cmd.length) {
    cursorEl.innerHTML = `<span style="color:#3fb950">axel@dev</span>:<span style="color:#58a6ff">~$</span>&nbsp;${cmd.substring(0,chi)}<span class="cursor"></span>`;
    chi++;
    setTimeout(typeCmd, 60 + Math.random() * 40);
  } else {
    setTimeout(() => {
      chi = 0; ci++;
      typeCmd();
    }, 2500);
  }
}
setTimeout(typeCmd, 1000);
</script>
</body>
</html>
