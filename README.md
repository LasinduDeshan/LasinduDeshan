<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Skills Visualization — Lasindú Deshan</title>
  <link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet"/>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0a0e1a;
      --surface: #111827;
      --surface2: #1a2235;
      --border: rgba(255,255,255,0.07);
      --text-primary: #f0f4ff;
      --text-secondary: #7a8aaa;
      --text-muted: #3a4a6a;
      --cyan: #22d3ee;
      --indigo: #818cf8;
      --lime: #d2ff4c;
      --pink: #f472b6;
      --orange: #fb923c;
      --cyan-dim: rgba(34,211,238,0.10);
      --indigo-dim: rgba(129,140,248,0.10);
      --lime-dim: rgba(210,255,76,0.10);
      --pink-dim: rgba(244,114,182,0.10);
      --orange-dim: rgba(251,146,60,0.10);
    }

    body {
      background: var(--bg);
      color: var(--text-primary);
      font-family: 'Syne', sans-serif;
      min-height: 100vh;
      padding: 3rem 1.5rem;
      overflow-x: hidden;
    }

    /* Subtle grid background */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image:
        linear-gradient(rgba(34,211,238,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(34,211,238,0.03) 1px, transparent 1px);
      background-size: 48px 48px;
      pointer-events: none;
      z-index: 0;
    }

    .wrapper {
      max-width: 860px;
      margin: 0 auto;
      position: relative;
      z-index: 1;
    }

    /* Header */
    .header {
      margin-bottom: 3.5rem;
    }

    .header-label {
      font-family: 'Space Mono', monospace;
      font-size: 11px;
      letter-spacing: 0.2em;
      color: var(--cyan);
      text-transform: uppercase;
      margin-bottom: 0.75rem;
    }

    .header h1 {
      font-size: clamp(2rem, 5vw, 3.2rem);
      font-weight: 800;
      line-height: 1.1;
      letter-spacing: -0.02em;
      background: linear-gradient(120deg, var(--text-primary) 40%, var(--cyan) 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .header-sub {
      margin-top: 0.75rem;
      font-family: 'Space Mono', monospace;
      font-size: 13px;
      color: var(--text-secondary);
    }

    /* Section */
    .section {
      margin-bottom: 3rem;
    }

    .section-header {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-bottom: 1.75rem;
    }

    .section-header::after {
      content: '';
      flex: 1;
      height: 1px;
      background: var(--border);
    }

    .section-title {
      font-family: 'Space Mono', monospace;
      font-size: 11px;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      color: var(--text-secondary);
      white-space: nowrap;
    }

    .section-dot {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      flex-shrink: 0;
    }

    /* Grid */
    .charts-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
      gap: 1rem;
    }

    /* Card */
    .chart-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 1.25rem 1rem 1rem;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0.875rem;
      transition: border-color 0.2s, transform 0.2s;
      cursor: default;
    }

    .chart-card:hover {
      transform: translateY(-3px);
    }

    /* Donut wrapper */
    .donut-wrap {
      position: relative;
      width: 110px;
      height: 110px;
      flex-shrink: 0;
    }

    .donut-wrap canvas {
      width: 100% !important;
      height: 100% !important;
    }

    .donut-center {
      position: absolute;
      inset: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      pointer-events: none;
    }

    .donut-value {
      font-family: 'Space Mono', monospace;
      font-size: 19px;
      font-weight: 700;
      line-height: 1;
    }

    .donut-pct {
      font-family: 'Space Mono', monospace;
      font-size: 11px;
      color: var(--text-secondary);
      margin-top: 2px;
    }

    .chart-label {
      font-size: 13px;
      font-weight: 600;
      color: var(--text-primary);
      text-align: center;
      letter-spacing: 0.01em;
    }

    /* Hover border tint via data attribute */
    .chart-card[data-color="cyan"]   { --card-accent: var(--cyan);   --card-dim: var(--cyan-dim); }
    .chart-card[data-color="indigo"] { --card-accent: var(--indigo); --card-dim: var(--indigo-dim); }
    .chart-card[data-color="lime"]   { --card-accent: var(--lime);   --card-dim: var(--lime-dim); }
    .chart-card[data-color="pink"]   { --card-accent: var(--pink);   --card-dim: var(--pink-dim); }
    .chart-card[data-color="orange"] { --card-accent: var(--orange); --card-dim: var(--orange-dim); }

    .chart-card:hover {
      border-color: var(--card-accent, var(--cyan));
      background: var(--card-dim, var(--cyan-dim));
    }
  </style>
</head>
<body>
<div class="wrapper">

  <header class="header">
    <p class="header-label">// skills.visualization</p>
    <h1>Technical Proficiency</h1>
    <p class="header-sub">Lasindú Deshan &nbsp;·&nbsp; Full Stack Developer</p>
  </header>

  <!-- Programming Languages -->
  <section class="section">
    <div class="section-header">
      <span class="section-dot" style="background: var(--cyan);"></span>
      <span class="section-title">Programming Languages</span>
    </div>
    <div class="charts-grid" id="grid-langs"></div>
  </section>

  <!-- Frameworks -->
  <section class="section">
    <div class="section-header">
      <span class="section-dot" style="background: var(--indigo);"></span>
      <span class="section-title">Frameworks &amp; Technologies</span>
    </div>
    <div class="charts-grid" id="grid-frameworks"></div>
  </section>

  <!-- DevOps -->
  <section class="section">
    <div class="section-header">
      <span class="section-dot" style="background: var(--lime);"></span>
      <span class="section-title">Infrastructure &amp; DevOps</span>
    </div>
    <div class="charts-grid" id="grid-devops"></div>
  </section>

</div>

<script>
  const COLORS = {
    cyan:   { fill: '#22d3ee', bg: 'rgba(34,211,238,0.12)'  },
    indigo: { fill: '#818cf8', bg: 'rgba(129,140,248,0.12)' },
    lime:   { fill: '#d2ff4c', bg: 'rgba(210,255,76,0.12)'  },
    pink:   { fill: '#f472b6', bg: 'rgba(244,114,182,0.12)' },
    orange: { fill: '#fb923c', bg: 'rgba(251,146,60,0.12)'  },
  };

  const GROUPS = [
    {
      gridId: 'grid-langs',
      items: [
        { label: 'Python',     value: 90, colorKey: 'cyan'   },
        { label: 'JavaScript', value: 85, colorKey: 'indigo' },
        { label: 'TypeScript', value: 80, colorKey: 'lime'   },
        { label: 'Java',       value: 70, colorKey: 'pink'   },
        { label: 'PHP',        value: 65, colorKey: 'orange' },
      ]
    },
    {
      gridId: 'grid-frameworks',
      items: [
        { label: 'React',   value: 90, colorKey: 'cyan'   },
        { label: 'Node.js', value: 85, colorKey: 'indigo' },
        { label: 'Next.js', value: 75, colorKey: 'lime'   },
        { label: 'Django',  value: 70, colorKey: 'pink'   },
        { label: 'Vue.js',  value: 70, colorKey: 'orange' },
      ]
    },
    {
      gridId: 'grid-devops',
      items: [
        { label: 'Docker',     value: 75, colorKey: 'cyan'   },
        { label: 'Kubernetes', value: 70, colorKey: 'indigo' },
        { label: 'AWS',        value: 70, colorKey: 'lime'   },
        { label: 'CI/CD',      value: 75, colorKey: 'pink'   },
      ]
    }
  ];

  let chartIndex = 0;

  function buildCard(container, item) {
    const color = COLORS[item.colorKey];
    const id = 'donut-' + chartIndex++;

    const card = document.createElement('div');
    card.className = 'chart-card';
    card.setAttribute('data-color', item.colorKey);

    card.innerHTML = `
      <div class="donut-wrap">
        <canvas id="${id}"></canvas>
        <div class="donut-center">
          <span class="donut-value" style="color:${color.fill}">${item.value}</span>
          <span class="donut-pct">%</span>
        </div>
      </div>
      <span class="chart-label">${item.label}</span>
    `;

    container.appendChild(card);

    new Chart(document.getElementById(id), {
      type: 'doughnut',
      data: {
        datasets: [{
          data: [item.value, 100 - item.value],
          backgroundColor: [color.fill, color.bg],
          borderWidth: 0,
          borderRadius: [8, 0],
          hoverOffset: 0,
        }]
      },
      options: {
        responsive: false,
        cutout: '74%',
        animation: {
          animateRotate: true,
          duration: 1000,
          easing: 'easeOutQuart',
          delay: chartIndex * 60,
        },
        plugins: {
          legend: { display: false },
          tooltip: { enabled: false },
        },
        events: [],
      }
    });
  }

  GROUPS.forEach(group => {
    const container = document.getElementById(group.gridId);
    group.items.forEach(item => buildCard(container, item));
  });
</script>
</body>
</html>
