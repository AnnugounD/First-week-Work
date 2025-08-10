<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Personal Portfolio — Annu Gound</title>
  <meta name="description" content="Personal portfolio: About, Skills, Projects. Responsive with dark mode and small JS game." />

  <style>
    /* ===== Basic reset ===== */
    *, *::before, *::after { box-sizing: border-box; }
    html,body { margin:0; padding:0; font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; line-height:1.4; }
    img { max-width:100%; display:block; }

    /* ===== Theme variables (light + dark toggled with data-theme) ===== */
    :root{
      --bg: #ffffff;
      --text: #111827;
      --muted: #6b7280;
      --accent: #2563eb;
      --card: #f8fafc;
      --radius: 12px;
      --container-pad: 1.25rem;
    }
    [data-theme="dark"]{
      --bg: #0b1220;
      --text: #e6eef8;
      --muted: #94a3b8;
      --accent: #60a5fa;
      --card: #071025;
    }

    body {
      background: linear-gradient(180deg, var(--bg), var(--card));
      color: var(--text);
      min-height:100vh;
      padding: 1.25rem;
      transition: background .25s, color .25s;
    }

    /* ===== Layout container ===== */
    .container {
      max-width: 1000px;
      margin: 0 auto;
      padding: var(--container-pad);
      display: grid;
      grid-template-columns: 300px 1fr;
      gap: 1.25rem;
    }

    /* ===== Header / Nav ===== */
    header {
      grid-column: 1 / -1;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap: 1rem;
      margin-bottom: .5rem;
    }
    .brand { display:flex; gap:.75rem; align-items:center; }
    .avatar {
      width:56px; height:56px; border-radius:12px; background:var(--card); display:flex; align-items:center; justify-content:center;
      font-weight:700; color:var(--accent);
      box-shadow: 0 2px 8px rgba(0,0,0,.06);
    }
    nav a { text-decoration:none; color:var(--muted); padding:.35rem .5rem; border-radius:8px; }
    nav a:hover { color:var(--text); background:rgba(100,116,139,.06); }

    /* ===== Side (About + Contact) ===== */
    aside {
      background: linear-gradient(180deg, rgba(255,255,255,.02), transparent);
      border-radius: var(--radius);
      padding: 1rem;
      min-height: 220px;
      box-shadow: 0 6px 18px rgba(2,6,23,.05);
    }
    .about h2 { margin:.25rem 0 .5rem; font-size:1.1rem; }
    .about p { margin:0 0 .8rem; color:var(--muted); font-size:.95rem; }

    .contact-list { list-style:none; padding:0; margin:0; display:grid; gap:.4rem; }
    .contact-list a { color:var(--accent); text-decoration:none; font-weight:600; }

    /* ===== Main content cards ===== */
    main { display:flex; flex-direction:column; gap:1rem; }
    .card {
      background: linear-gradient(180deg, rgba(255,255,255,.02), transparent);
      padding:1rem;
      border-radius:var(--radius);
      box-shadow: 0 6px 18px rgba(2,6,23,.03);
    }
    .skills-grid { display:flex; gap:.5rem; flex-wrap:wrap; }
    .skill { padding:.35rem .6rem; border-radius:999px; background:rgba(99,102,241,.08); font-weight:600; color:var(--accent); }

    .projects { display:grid; grid-template-columns: repeat(2, 1fr); gap: .8rem; }
    .project { padding:.75rem; border-radius:10px; background:var(--card); }
    .project h3 { margin:.2rem 0 .6rem; }
    .project p { margin:0; color:var(--muted); font-size:.95rem; }

    /* ===== Footer ===== */
    footer { grid-column:1 / -1; text-align:center; color:var(--muted); padding-top:.5rem; font-size:.9rem; }

    /* ===== Controls row ===== */
    .controls { display:flex; gap:.5rem; align-items:center; }
    button { cursor:pointer; border:0; background:var(--accent); color:white; padding:.5rem .7rem; border-radius:8px; font-weight:600; }
    .ghost { background:transparent; border:1px solid rgba(255,255,255,.06); color:var(--text); }

    /* ===== Number guessing modal (simple) ===== */
    .modal {
      position:fixed; inset:0; display:none; align-items:center; justify-content:center;
      background: rgba(2,6,23,.45); z-index:40;
    }
    .modal.show { display:flex; }
    .modal-card { background:var(--bg); padding:1rem; border-radius:12px; width:320px; max-width:90%; box-shadow:0 12px 30px rgba(2,6,23,.45); color:var(--text); }
    .modal-card input { width:100%; padding:.6rem; border-radius:8px; border:1px solid #ddd; margin-bottom:.6rem; }

    /* ===== Responsive ===== */
    @media (max-width: 900px) {
      .container { grid-template-columns: 1fr; }
      .projects { grid-template-columns: 1fr; }
      header { flex-direction:column; align-items:flex-start; gap:.5rem; }
    }
  </style>
</head>
<body data-theme="light">
  <div class="container" role="document">

    <header>
      <div class="brand">
        <div class="avatar" aria-hidden="true">AG</div>
        <div>
          <div style="font-weight:800">Annu Gound</div>
          <div style="font-size:.9rem; color:var(--muted)">Frontend Developer (Fresher)</div>
        </div>
      </div>

      <div style="display:flex; gap:.5rem; align-items:center;">
        <nav aria-label="Main navigation">
          <a href="#about">About</a>
          <a href="#skills">Skills</a>
          <a href="#projects">Projects</a>
          <a href="#contact">Contact</a>
        </nav>

        <div class="controls" style="margin-left: .6rem;">
          <button id="themeToggle" aria-pressed="false" title="Toggle dark/light">Dark Mode</button>
          <button id="playGame" class="ghost" title="Open a small JS game">Number Game</button>
        </div>
      </div>
    </header>

    <!-- Aside: About / Contact -->
    <aside aria-labelledby="about">
      <section id="about" class="about" aria-labelledby="about-heading">
        <h2 id="about-heading">About Me</h2>
        <p>Hi, I’m Annu Gound
          <br>
I’m a passionate Software Developer and Data Analyst with skills in HTML, CSS, JavaScript, Python, and data visualization tools. I enjoy building responsive, user-friendly web applications and analyzing datasets to extract meaningful insights. My expertise includes frontend development, data cleaning, visualization, and reporting. I continuously work on upgrading my skills and applying them to solve real-world problems.</p>
        <p style="color:var(--muted); font-size:.9rem;">Location: Mumbai • Available for internships</p>
      </section>

      <hr style="margin: .6rem 0; border: none; border-top:1px dashed rgba(0,0,0,.06)" />

      <section id="contact" aria-labelledby="contact-heading">
        <h3 id="contact-heading" style="margin:.25rem 0">Contact</h3>
        <ul class="contact-list">
          <li>Email: <a href="mailto:annugound135@gmail.com">goundanu11@gmail.com</a></li>
          <li>Phone: <a href="tel:+919167762695">+91 9167762695</a></li>
          <li>GitHub: <a href="https://github.com/AnnugounD" target="_blank" rel="noopener">https://github.com/AnnugounD</a></li>
        </ul>
      </section>
    </aside>

    <!-- Main content -->
    <main>
      <!-- Skills -->
      <article id="skills" class="card" aria-labelledby="skills-heading">
        <h2 id="skills-heading">Skills</h2>

        <div class="skills-grid" aria-hidden="false">
          <span class="skill">HTML</span>
          <span class="skill">CSS</span>
          <span class="skill">Flexbox</span>
          <span class="skill">Grid</span>
          <span class="skill">Responsive Design</span>
          <span class="skill">JavaScript</span>
          <span class="skill">Git</span>
        </div>

        <h3 style="margin-top:.8rem">Notes</h3>
        <p style="color:var(--muted); margin-top:0;">This site uses semantic tags, accessible attributes (aria), and a mobile-first responsive approach.</p>
      </article>

      <!-- Projects -->
      <article id="projects" class="card" aria-labelledby="projects-heading">
        <h2 id="projects-heading">Projects</h2>
        <div class="projects" role="list">
          <div class="project" role="listitem">
            <h3>Personal Portfolio</h3>
            <p>Single-file responsive portfolio with dark mode toggle (this page!). HTML, CSS, JS.</p>
          </div>

          <div class="project" role="listitem">
            <h3>Landing Page Clone</h3>
            <p>Built a pixel-perfect clone practice using Flexbox and Grid. Focus: semantic layout and responsive breakpoints.</p>
          </div>
        </div>

        <p style="margin-top:.75rem; color:var(--muted)">More projects can be added here — link to GitHub repo when ready.</p>
      </article>
    </main>

    <footer>
      <small>Built with ❤️ • Responsive • Accessible</small>
    </footer>
  </div>

  <!-- Modal: Simple Number Guessing Game -->
  <div id="modal" class="modal" role="dialog" aria-modal="true" aria-hidden="true">
    <div class="modal-card" role="document">
      <h3>Number Guessing Game</h3>
      <p style="color:var(--muted); margin-top:0;">Guess a number between 1 and 20. I'll tell you higher/lower.</p>
      <input id="guessInput" type="number" min="1" max="20" placeholder="Enter your guess (1-20)" />
      <div style="display:flex; gap:.5rem;">
        <button id="guessBtn">Check</button>
        <button id="restartBtn" class="ghost">Restart</button>
        <button id="closeModal" class="ghost">Close</button>
      </div>
      <p id="gameMsg" style="margin-top:.6rem; color:var(--muted)"></p>
    </div>
  </div>

  <script>
    // ===== Theme toggle =====
    const themeToggle = document.getElementById('themeToggle');
    const root = document.documentElement;
    // initialize from localStorage if present
    const savedTheme = localStorage.getItem('theme') || 'light';
    document.body.setAttribute('data-theme', savedTheme);
    themeToggle.textContent = savedTheme === 'dark' ? 'Light Mode' : 'Dark Mode';
    themeToggle.setAttribute('aria-pressed', String(savedTheme === 'dark'));

    themeToggle.addEventListener('click', () => {
      const current = document.body.getAttribute('data-theme') === 'dark' ? 'dark' : 'light';
      const next = current === 'dark' ? 'light' : 'dark';
      document.body.setAttribute('data-theme', next);
      themeToggle.textContent = next === 'dark' ? 'Light Mode' : 'Dark Mode';
      themeToggle.setAttribute('aria-pressed', String(next === 'dark'));
      localStorage.setItem('theme', next);
    });

    // ===== Small Number Guessing Game =====
    const modal = document.getElementById('modal');
    const playBtn = document.getElementById('playGame');
    const closeModal = document.getElementById('closeModal');
    const guessInput = document.getElementById('guessInput');
    const guessBtn = document.getElementById('guessBtn');
    const restartBtn = document.getElementById('restartBtn');
    const gameMsg = document.getElementById('gameMsg');

    let secret = Math.floor(Math.random()*20) + 1;
    let attempts = 0;

    function openGame() {
      modal.classList.add('show');
      modal.setAttribute('aria-hidden','false');
      guessInput.value = '';
      gameMsg.textContent = '';
      attempts = 0;
      secret = Math.floor(Math.random()*20) + 1;
      guessInput.focus();
    }
    function closeGame() {
      modal.classList.remove('show');
      modal.setAttribute('aria-hidden','true');
    }

    playBtn.addEventListener('click', openGame);
    closeModal.addEventListener('click', closeGame);

    guessBtn.addEventListener('click', () => {
      const val = Number(guessInput.value);
      if (!val || val < 1 || val > 20) {
        gameMsg.textContent = 'Please enter a number between 1 and 20.';
        return;
      }
      attempts++;
      if (val === secret) {
        gameMsg.textContent = `🎉 Correct! You guessed in ${attempts} attempt${attempts>1?'s':''}. Secret: ${secret}`;
      } else if (val < secret) {
        gameMsg.textContent = 'Try higher ↑';
      } else {
        gameMsg.textContent = 'Try lower ↓';
      }
    });

    restartBtn.addEventListener('click', () => {
      secret = Math.floor(Math.random()*20) + 1;
      attempts = 0;
      guessInput.value = '';
      gameMsg.textContent = 'Game restarted — new number generated.';
      guessInput.focus();
    });

    // close modal on Esc
    window.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') closeGame();
    });

    // Small accessibility: trap focus very simply when modal open (basic)
    modal.addEventListener('keydown', (e) => {
      if (e.key === 'Tab') {
        // ensure focus stays in modal: move focus to input if tabbing out (simple approach)
        if (document.activeElement === closeModal && !e.shiftKey) {
          e.preventDefault();
          guessInput.focus();
        }
      }
    });

    // Smooth scrolling for nav anchors
    document.querySelectorAll('nav a').forEach(a => {
      a.addEventListener('click', (e) => {
        e.preventDefault();
        const id = a.getAttribute('href').slice(1);
        document.getElementById(id).scrollIntoView({ behavior: 'smooth', block: 'start' });
      });
    });
  </script>
</body>
</html>
# First-week-Work
