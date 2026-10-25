<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DailyPulse | Daily Report</title>
  <link rel="stylesheet" href="style.css">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <aside class="sidebar">
    <h2>✦ DailyPulse</h2>
    <p class="muted">WORKSPACE</p>
    <a class="active" href="#overview">▦ Overview</a>
    <a href="#report">✎ Daily Report</a>
    <a href="#analytics">◷ Analytics</a>
    <div class="side-bottom">YOUR DAY. YOUR DATA.</div>
  </aside>

  <main id="overview">
    <header>
      <div>
        <p class="muted">PERSONAL WORKSPACE / 2026</p>
        <h1>Daily Overview<span class="dot">.</span></h1>
        <p class="muted">Track your progress. Build your future.</p>
      </div>
      <div class="datebox" id="today"></div>
    </header>

    <section class="stats">
      <article class="stat">
        <p class="muted">TASKS COMPLETED</p>
        <h2 id="completed">0</h2>
        <span>Keep moving forward ↗</span>
      </article>
      <article class="stat">
        <p class="muted">TOTAL TASKS</p>
        <h2 id="total">0</h2>
        <span>Your daily workload</span>
      </article>
      <article class="stat">
        <p class="muted">PRODUCTIVITY</p>
        <h2 id="score">0%</h2>
        <span>Completion rate</span>
      </article>
    </section>

    <section class="layout">
      <article class="panel" id="report">
        <p class="eyebrow">DAILY LOG / 001</p>
        <h2>Create Daily Report</h2>

        <form id="reportForm">
          <label for="task">Task or activity</label>
          <input id="task" placeholder="e.g. Study, coding, project..." required>

          <label for="category">Category</label>
          <select id="category">
            <option>Study</option>
            <option>Work</option>
            <option>Personal</option>
            <option>Fitness</option>
            <option>Project</option>
          </select>

          <label for="hours">Hours spent</label>
          <input id="hours" type="number" min="0" max="24"
            step="0.5" value="1" required>

          <label for="notes">Daily notes</label>
          <textarea id="notes" placeholder="What did you achieve today?"></textarea>

          <button class="primary" type="submit">＋ Add to Report</button>
        </form>
      </article>

      <article class="panel" id="analytics">
        <p class="eyebrow">LIVE ANALYTICS</p>
        <h2>Productivity</h2>
        <canvas id="chart"></canvas>
        <div class="progress-head">
          <span>Daily completion</span>
          <strong id="progressText">0%</strong>
        </div>
        <div class="progress"><div id="progressBar"></div></div>
        <p class="muted small">Based on today's recorded tasks.</p>
      </article>
    </section>

    <section class="panel history">
      <div class="history-head">
        <div>
          <p class="eyebrow">ACTIVITY STREAM</p>
          <h2>Today's Report</h2>
        </div>
        <button class="secondary" id="export">↓ Export CSV</button>
      </div>
      <div id="reports" class="reports">
        <p class="muted">No reports yet. Add your first activity above.</p>
      </div>
    </section>

    <footer>DAILYPULSE © 2026 — SMALL STEPS. BIG RESULTS.</footer>
  </main>
  <script src="script.js"></script>
</body>
</html>
