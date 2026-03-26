<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>UpperSchool Laundry Room</title>
  <link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Syne:wght@400;600;700&display=swap" rel="stylesheet" />
  <style>
    :root {
      --bg: #0d0f12;
      --surface: #161a20;
      --surface2: #1e232b;
      --border: rgba(255,255,255,0.07);
      --border-hover: rgba(255,255,255,0.14);
      --text: #e8eaf0;
      --muted: #7a8090;
      --accent-blue: #4a9eff;
      --accent-blue-dim: rgba(74,158,255,0.12);
      --accent-green: #3ecf8e;
      --accent-green-dim: rgba(62,207,142,0.12);
      --accent-amber: #f5a623;
      --accent-amber-dim: rgba(245,166,35,0.10);
      --accent-red: #ff5f5f;
      --radius: 14px;
      --radius-sm: 8px;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Syne', sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      overflow-x: hidden;
    }

    body::before {
      content: '';
      position: fixed; inset: 0;
      background-image:
        linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);
      background-size: 40px 40px;
      pointer-events: none;
      z-index: 0;
    }

    .page {
      position: relative; z-index: 1;
      max-width: 1000px;
      margin: 0 auto;
      padding: 2.5rem 1.5rem 4rem;
    }

    header {
      display: flex;
      align-items: flex-start;
      justify-content: space-between;
      margin-bottom: 2.5rem;
      flex-wrap: wrap;
      gap: 1rem;
    }

    .header-left h1 {
      font-size: clamp(24px, 4vw, 36px);
      font-weight: 700;
      letter-spacing: -0.02em;
      line-height: 1.1;
      color: var(--text);
    }

    .header-left h1 span { color: var(--accent-blue); }

    .header-left p {
      font-size: 13px;
      color: var(--muted);
      margin-top: 6px;
      font-family: 'DM Mono', monospace;
    }

    .stats-bar {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      align-items: flex-start;
      justify-content: flex-end;
    }

    .stat-pill {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 100px;
      padding: 6px 14px;
      font-size: 12px;
      font-family: 'DM Mono', monospace;
      color: var(--muted);
      display: flex;
      align-items: center;
      gap: 6px;
      white-space: nowrap;
    }

    .stat-pill .dot {
      width: 7px; height: 7px;
      border-radius: 50%;
    }

    .dot-blue { background: var(--accent-blue); }
    .dot-green { background: var(--accent-green); }
    .dot-free { background: #3a3f4a; }

    .section-header {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 12px;
    }

    .section-header h2 {
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--muted);
      font-family: 'DM Mono', monospace;
      white-space: nowrap;
    }

    .count-badge {
      font-family: 'DM Mono', monospace;
      font-size: 11px;
      padding: 2px 9px;
      border-radius: 100px;
      background: var(--surface2);
      color: var(--muted);
      border: 1px solid var(--border);
      white-space: nowrap;
    }

    .section-header .line {
      flex: 1;
      height: 1px;
      background: var(--border);
    }

    .machines-grid {
      display: grid;
      gap: 10px;
      margin-bottom: 2rem;
    }

    .washers-grid { grid-template-columns: repeat(4, 1fr); }
    .dryers-grid { grid-template-columns: repeat(3, 1fr); }

    @media (max-width: 680px) {
      .washers-grid { grid-template-columns: repeat(2, 1fr); }
      .dryers-grid { grid-template-columns: repeat(2, 1fr); }
    }

    .machine-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 16px;
      cursor: pointer;
      transition: border-color 0.2s, background 0.2s, transform 0.15s;
      display: flex;
      flex-direction: column;
      gap: 8px;
      min-height: 150px;
      position: relative;
      overflow: hidden;
    }

    .machine-card::after {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      border-radius: var(--radius) var(--radius) 0 0;
      background: transparent;
      transition: background 0.2s;
    }

    .machine-card:hover { border-color: var(--border-hover); transform: translateY(-1px); }
    .machine-card.in-use { background: var(--accent-blue-dim); border-color: rgba(74,158,255,0.3); }
    .machine-card.in-use::after { background: var(--accent-blue); }
    .machine-card.done { background: var(--accent-green-dim); border-color: rgba(62,207,142,0.3); }
    .machine-card.done::after { background: var(--accent-green); }

    .machine-top {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .machine-id {
      font-size: 11px;
      font-family: 'DM Mono', monospace;
      color: var(--muted);
      font-weight: 500;
    }

    .machine-card.in-use .machine-id { color: var(--accent-blue); }
    .machine-card.done .machine-id { color: var(--accent-green); }

    .status-badge {
      font-size: 10px;
      font-family: 'DM Mono', monospace;
      padding: 3px 8px;
      border-radius: 100px;
      font-weight: 500;
    }

    .badge-free { background: rgba(255,255,255,0.06); color: var(--muted); }
    .badge-active { background: rgba(74,158,255,0.15); color: var(--accent-blue); }
    .badge-done { background: rgba(62,207,142,0.15); color: var(--accent-green); }

    .machine-user {
      font-size: 14px;
      font-weight: 600;
      color: var(--text);
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .machine-temp {
      font-size: 11px;
      font-family: 'DM Mono', monospace;
      color: var(--accent-amber);
    }

    .timer {
      font-size: 22px;
      font-weight: 700;
      font-family: 'DM Mono', monospace;
      color: var(--accent-blue);
      letter-spacing: -0.02em;
    }

    .timer.done { color: var(--accent-green); }

    .progress-bar-wrap {
      height: 3px;
      background: rgba(255,255,255,0.07);
      border-radius: 3px;
      overflow: hidden;
    }

    .progress-bar-fill {
      height: 100%;
      border-radius: 3px;
      background: var(--accent-blue);
      transition: width 1s linear;
    }

    .machine-card.done .progress-bar-fill { background: var(--accent-green); width: 100% !important; }

    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
      padding: 8px 14px;
      font-family: 'Syne', sans-serif;
      font-size: 13px;
      font-weight: 600;
      border-radius: var(--radius-sm);
      cursor: pointer;
      border: 1px solid var(--border);
      background: transparent;
      color: var(--text);
      transition: all 0.15s;
      width: 100%;
    }

    .btn:hover { background: rgba(255,255,255,0.06); border-color: var(--border-hover); }
    .btn:active { transform: scale(0.97); }

    .btn-primary { background: var(--accent-blue); border-color: var(--accent-blue); color: #fff; }
    .btn-primary:hover { background: #6ab2ff; border-color: #6ab2ff; }
    .btn-danger { background: rgba(255,95,95,0.1); border-color: rgba(255,95,95,0.3); color: var(--accent-red); }
    .btn-danger:hover { background: rgba(255,95,95,0.18); }

    /* Modal */
    .modal-overlay {
      position: fixed; inset: 0;
      background: rgba(0,0,0,0.65);
      backdrop-filter: blur(4px);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 200;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.2s;
    }

    .modal-overlay.open { opacity: 1; pointer-events: all; }

    .modal {
      background: var(--surface);
      border: 1px solid var(--border-hover);
      border-radius: var(--radius);
      padding: 2rem;
      width: 380px;
      max-width: 90vw;
      transform: translateY(12px);
      transition: transform 0.2s;
      max-height: 90vh;
      overflow-y: auto;
    }

    .modal-overlay.open .modal { transform: translateY(0); }

    .modal h2 { font-size: 20px; font-weight: 700; margin-bottom: 4px; }

    .modal .modal-sub {
      font-size: 13px;
      color: var(--muted);
      font-family: 'DM Mono', monospace;
      margin-bottom: 1.5rem;
    }

    .form-group { margin-bottom: 1rem; }

    .form-group label {
      display: block;
      font-size: 12px;
      font-family: 'DM Mono', monospace;
      color: var(--muted);
      margin-bottom: 6px;
      letter-spacing: 0.05em;
    }

    .form-group input, .form-group select {
      width: 100%;
      padding: 10px 14px;
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      color: var(--text);
      font-family: 'Syne', sans-serif;
      font-size: 15px;
      outline: none;
      transition: border-color 0.15s;
      appearance: none;
      -webkit-appearance: none;
    }

    .form-group select {
      background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%237a8090' stroke-width='1.5' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
      background-repeat: no-repeat;
      background-position: right 14px center;
      padding-right: 36px;
      cursor: pointer;
    }

    .form-group select option { background: #1e232b; color: #e8eaf0; }
    .form-group input:focus, .form-group select:focus { border-color: var(--accent-blue); }
    .form-group input.error { border-color: var(--accent-red); }

    .duration-presets {
      display: flex;
      gap: 6px;
      margin-top: 8px;
      flex-wrap: wrap;
    }

    .preset-btn {
      font-family: 'DM Mono', monospace;
      font-size: 12px;
      padding: 4px 10px;
      border-radius: 100px;
      border: 1px solid var(--border);
      background: transparent;
      color: var(--muted);
      cursor: pointer;
      transition: all 0.15s;
    }

    .preset-btn:hover, .preset-btn.active {
      border-color: var(--accent-blue);
      color: var(--accent-blue);
      background: var(--accent-blue-dim);
    }

    .modal-actions {
      display: flex;
      gap: 8px;
      margin-top: 1.5rem;
    }

    .modal-actions .btn { width: auto; flex: 1; }

    /* Log & completed */
    .log-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      overflow: hidden;
    }

    .scrollable {
      max-height: 260px;
      overflow-y: auto;
    }

    .scrollable::-webkit-scrollbar { width: 4px; }
    .scrollable::-webkit-scrollbar-track { background: transparent; }
    .scrollable::-webkit-scrollbar-thumb { background: var(--border-hover); border-radius: 2px; }

    .log-item {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 10px 18px;
      border-bottom: 1px solid var(--border);
      font-size: 13px;
      animation: fadeIn 0.3s ease;
    }

    .log-item:last-child { border-bottom: none; }

    .log-time {
      font-family: 'DM Mono', monospace;
      font-size: 11px;
      color: var(--muted);
      min-width: 50px;
    }

    .log-dot { width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0; }
    .log-msg { color: var(--text); flex: 1; }

    .log-empty {
      padding: 24px 18px;
      font-size: 13px;
      color: var(--muted);
      font-family: 'DM Mono', monospace;
    }

    /* Completed loads */
    .completed-table {
      width: 100%;
      border-collapse: collapse;
      font-size: 13px;
    }

    .completed-table th {
      text-align: left;
      padding: 10px 16px;
      font-size: 10px;
      font-family: 'DM Mono', monospace;
      font-weight: 500;
      color: var(--muted);
      letter-spacing: 0.08em;
      text-transform: uppercase;
      border-bottom: 1px solid var(--border);
      background: var(--surface2);
    }

    .completed-table td {
      padding: 9px 16px;
      border-bottom: 1px solid var(--border);
      color: var(--text);
      vertical-align: middle;
    }

    .completed-table tr:last-child td { border-bottom: none; }
    .completed-table tr { animation: fadeIn 0.3s ease; }

    .temp-tag {
      font-family: 'DM Mono', monospace;
      font-size: 11px;
      padding: 2px 8px;
      border-radius: 100px;
      background: rgba(245,166,35,0.12);
      color: var(--accent-amber);
      border: 1px solid rgba(245,166,35,0.2);
    }

    .type-tag {
      font-family: 'DM Mono', monospace;
      font-size: 11px;
      padding: 2px 8px;
      border-radius: 100px;
    }

    .type-washer { background: var(--accent-blue-dim); color: var(--accent-blue); }
    .type-dryer { background: rgba(62,207,142,0.1); color: var(--accent-green); }

    .clear-btn-sm {
      font-size: 11px;
      font-family: 'DM Mono', monospace;
      padding: 3px 8px;
      border-radius: 100px;
      border: 1px solid rgba(255,95,95,0.2);
      background: transparent;
      color: var(--accent-red);
      cursor: pointer;
      opacity: 0.5;
      transition: opacity 0.15s;
    }

    .clear-btn-sm:hover { opacity: 1; }

    .two-col {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.5rem;
    }

    @media (max-width: 680px) {
      .two-col { grid-template-columns: 1fr; }
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(-4px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .pulse { animation: pulse 2s ease-in-out infinite; }

    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.5; }
    }
  </style>
</head>
<body>

<div class="page">
  <header>
    <div class="header-left">
      <h1>UpperSchool <span>Laundry Room</span></h1>
      <p>4 washers · 6 dryers</p>
    </div>
    <div class="stats-bar" id="stats-bar"></div>
  </header>

  <div class="section-header">
    <h2>Washers</h2>
    <span class="count-badge" id="washer-open-badge">4 open</span>
    <div class="line"></div>
  </div>
  <div class="machines-grid washers-grid" id="washers"></div>

  <div class="section-header">
    <h2>Dryers</h2>
    <span class="count-badge" id="dryer-open-badge">6 open</span>
    <div class="line"></div>
  </div>
  <div class="machines-grid dryers-grid" id="dryers"></div>

  <div class="two-col" style="margin-top:2rem;">
    <div>
      <div class="section-header">
        <h2>Completed loads</h2>
        <div class="line"></div>
      </div>
      <div class="log-card">
        <div class="scrollable" id="completed-list">
          <div class="log-empty">No completed loads yet.</div>
        </div>
      </div>
    </div>

    <div>
      <div class="section-header">
        <h2>Activity log</h2>
        <div class="line"></div>
      </div>
      <div class="log-card">
        <div class="scrollable" id="log-list">
          <div class="log-empty">No activity yet. Start a load above.</div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Modal -->
<div class="modal-overlay" id="modal-overlay">
  <div class="modal">
    <h2 id="modal-title">Start Machine</h2>
    <div class="modal-sub" id="modal-sub"></div>
    <div class="form-group">
      <label>Name / Room #</label>
      <input type="text" id="inp-user" placeholder="e.g. Alex, Room 204" autocomplete="off" />
    </div>
    <div class="form-group">
      <label>Duration (minutes)</label>
      <input type="number" id="inp-dur" min="1" max="120" />
      <div class="duration-presets" id="presets"></div>
    </div>
    <div class="form-group">
      <label id="temp-label">Temperature</label>
      <select id="inp-temp"></select>
    </div>
    <div class="modal-actions">
      <button class="btn" onclick="closeModal()">Cancel</button>
      <button class="btn btn-primary" onclick="confirmStart()">Start load</button>
    </div>
  </div>
</div>

<script>
  const DEFAULTS = { washer: 35, dryer: 45 };
  const WASHER_TEMPS = ['Cold', 'Cool', 'Warm', 'Hot'];
  const DRYER_TEMPS = ['Air Dry / No Heat', 'Low Heat', 'Medium Heat', 'High Heat'];

  let machines = {
    washers: Array.from({length: 4}, (_, i) => ({
      id: `W${i+1}`, type: 'washer', label: `Washer ${i+1}`,
      state: 'free', user: '', endTime: null, duration: null, startTime: null, temp: ''
    })),
    dryers: Array.from({length: 6}, (_, i) => ({
      id: `D${i+1}`, type: 'dryer', label: `Dryer ${i+1}`,
      state: 'free', user: '', endTime: null, duration: null, startTime: null, temp: ''
    }))
  };

  let activityLog = [];
  let completedLoads = [];
  let activeModal = null;

  function timeLeft(m) {
    if (!m.endTime) return null;
    return Math.max(0, Math.ceil((m.endTime - Date.now()) / 1000));
  }

  function fmtSecs(s) {
    if (s <= 0) return 'Done';
    const mins = Math.floor(s / 60);
    const sec = s % 60;
    return `${String(mins).padStart(2,'0')}:${String(sec).padStart(2,'0')}`;
  }

  function progress(m) {
    if (!m.startTime || !m.endTime) return 0;
    return Math.min(1, (Date.now() - m.startTime) / (m.endTime - m.startTime));
  }

  function fmtTime() {
    return new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'});
  }

  function renderStats() {
    const washersOpen = machines.washers.filter(m => m.state === 'free').length;
    const dryersOpen = machines.dryers.filter(m => m.state === 'free').length;
    const running = [...machines.washers, ...machines.dryers].filter(m => m.state === 'in-use' && timeLeft(m) > 0).length;
    const ready = [...machines.washers, ...machines.dryers].filter(m => m.state === 'in-use' && timeLeft(m) === 0).length;

    document.getElementById('washer-open-badge').textContent = `${washersOpen} open`;
    document.getElementById('dryer-open-badge').textContent = `${dryersOpen} open`;

    document.getElementById('stats-bar').innerHTML = `
      <div class="stat-pill"><span class="dot dot-blue pulse"></span>${running} running</div>
      ${ready > 0 ? `<div class="stat-pill"><span class="dot dot-green"></span>${ready} ready to grab</div>` : ''}
      <div class="stat-pill"><span class="dot dot-free"></span>${washersOpen} washers free</div>
      <div class="stat-pill"><span class="dot dot-free"></span>${dryersOpen} dryers free</div>
    `;
  }

  function renderMachines() {
    ['washers','dryers'].forEach(type => {
      const container = document.getElementById(type);
      container.innerHTML = machines[type].map(m => {
        const secs = timeLeft(m);
        const isDone = m.state === 'in-use' && secs === 0;
        const isActive = m.state === 'in-use' && secs > 0;
        const cardClass = isDone ? 'done' : isActive ? 'in-use' : '';
        const badge = isDone
          ? '<span class="status-badge badge-done">done</span>'
          : isActive
          ? '<span class="status-badge badge-active">running</span>'
          : '<span class="status-badge badge-free">free</span>';
        const icon = type === 'washers' ? '⊙' : '◎';
        const pct = m.state === 'in-use' ? Math.round(progress(m) * 100) : 0;

        if (m.state === 'free') {
          return `<div class="machine-card" onclick="openModal('${type}','${m.id}')">
            <div class="machine-top"><span class="machine-id">${m.id}</span>${badge}</div>
            <div style="font-size:28px;color:var(--muted);">${icon}</div>
            <div style="font-size:13px;color:var(--muted);margin-top:auto;">tap to start</div>
          </div>`;
        }

        return `<div class="machine-card ${cardClass}">
          <div class="machine-top"><span class="machine-id">${m.id}</span>${badge}</div>
          <div class="machine-user">${m.user}</div>
          ${m.temp ? `<div class="machine-temp">⬥ ${m.temp}</div>` : ''}
          <div class="timer ${isDone ? 'done' : ''}">${isDone ? '✓ Done' : fmtSecs(secs)}</div>
          <div class="progress-bar-wrap">
            <div class="progress-bar-fill" style="width:${pct}%"></div>
          </div>
          <button class="btn btn-danger" onclick="clearMachine('${type}','${m.id}')">Clear</button>
        </div>`;
      }).join('');
    });
    renderStats();
  }

  function addLog(msg, color) {
    activityLog.unshift({ time: fmtTime(), msg, color: color || 'var(--accent-blue)' });
    if (activityLog.length > 40) activityLog.pop();
    renderLog();
  }

  function renderLog() {
    const el = document.getElementById('log-list');
    if (!activityLog.length) {
      el.innerHTML = '<div class="log-empty">No activity yet. Start a load above.</div>';
      return;
    }
    el.innerHTML = activityLog.map(e => `
      <div class="log-item">
        <span class="log-time">${e.time}</span>
        <span class="log-dot" style="background:${e.color}"></span>
        <span class="log-msg">${e.msg}</span>
      </div>`).join('');
  }

  function renderCompleted() {
    const el = document.getElementById('completed-list');
    if (!completedLoads.length) {
      el.innerHTML = '<div class="log-empty">No completed loads yet.</div>';
      return;
    }
    el.innerHTML = `<table class="completed-table">
      <thead><tr>
        <th>Time</th><th>Person</th><th>Machine</th><th>Temp</th><th></th>
      </tr></thead>
      <tbody>
        ${completedLoads.map((c, i) => `<tr>
          <td style="font-family:'DM Mono',monospace;font-size:11px;color:var(--muted);">${c.time}</td>
          <td>${c.user}</td>
          <td><span class="type-tag ${c.type === 'washer' ? 'type-washer' : 'type-dryer'}">${c.machine}</span></td>
          <td>${c.temp ? `<span class="temp-tag">${c.temp}</span>` : '<span style="color:var(--muted);font-size:12px;">—</span>'}</td>
          <td><button class="clear-btn-sm" onclick="removeCompleted(${i})">✕</button></td>
        </tr>`).join('')}
      </tbody>
    </table>`;
  }

  function removeCompleted(i) {
    completedLoads.splice(i, 1);
    renderCompleted();
  }

  function openModal(type, id) {
    activeModal = { type, id };
    const m = machines[type].find(x => x.id === id);
    const def = DEFAULTS[m.type];

    document.getElementById('modal-title').textContent = `Start ${m.label}`;
    document.getElementById('modal-sub').textContent = `${m.id} · ${m.type}`;
    document.getElementById('inp-user').value = '';
    document.getElementById('inp-user').classList.remove('error');
    document.getElementById('inp-dur').value = def;

    const temps = type === 'washers' ? WASHER_TEMPS : DRYER_TEMPS;
    document.getElementById('temp-label').textContent = type === 'washers' ? 'Wash temperature' : 'Heat setting';
    document.getElementById('inp-temp').innerHTML = temps.map((t, i) =>
      `<option value="${t}" ${i === 2 ? 'selected' : ''}>${t}</option>`
    ).join('');

    const presets = type === 'washers' ? [25, 35, 45, 60] : [30, 45, 60, 75];
    document.getElementById('presets').innerHTML = presets.map(p =>
      `<button class="preset-btn ${p === def ? 'active' : ''}" onclick="setPreset(${p})">${p}m</button>`
    ).join('');

    document.getElementById('modal-overlay').classList.add('open');
    setTimeout(() => document.getElementById('inp-user').focus(), 150);
  }

  function setPreset(val) {
    document.getElementById('inp-dur').value = val;
    document.querySelectorAll('.preset-btn').forEach(b => {
      b.classList.toggle('active', b.textContent === `${val}m`);
    });
  }

  function closeModal() {
    document.getElementById('modal-overlay').classList.remove('open');
    activeModal = null;
  }

  function confirmStart() {
    const user = document.getElementById('inp-user').value.trim();
    const dur = parseInt(document.getElementById('inp-dur').value);
    const temp = document.getElementById('inp-temp').value;
    const inp = document.getElementById('inp-user');
    if (!user) { inp.classList.add('error'); inp.focus(); return; }
    inp.classList.remove('error');
    if (!dur || dur < 1) return;

    const { type, id } = activeModal;
    const m = machines[type].find(x => x.id === id);
    m.state = 'in-use';
    m.user = user;
    m.temp = temp;
    m.startTime = Date.now();
    m.duration = dur;
    m.endTime = Date.now() + dur * 60 * 1000;
    addLog(`${user} started ${m.label} · ${temp} · ${dur}min`, 'var(--accent-blue)');
    closeModal();
    renderMachines();
  }

  function clearMachine(type, id) {
    const m = machines[type].find(x => x.id === id);
    const isDone = timeLeft(m) === 0;

    completedLoads.unshift({
      time: fmtTime(),
      user: m.user,
      machine: m.label,
      type: m.type,
      temp: m.temp,
      duration: m.duration
    });
    if (completedLoads.length > 50) completedLoads.pop();

    addLog(
      isDone ? `${m.label} cleared — ${m.user}'s load is done` : `${m.label} cleared early (${m.user})`,
      isDone ? 'var(--accent-green)' : 'var(--accent-amber)'
    );

    m.state = 'free'; m.user = ''; m.temp = '';
    m.startTime = null; m.duration = null; m.endTime = null;
    renderMachines();
    renderCompleted();
  }

  document.getElementById('modal-overlay').addEventListener('click', e => {
    if (e.target === document.getElementById('modal-overlay')) closeModal();
  });

  document.addEventListener('keydown', e => {
    if (e.key === 'Escape') closeModal();
    if (e.key === 'Enter' && activeModal) confirmStart();
  });

  setInterval(renderMachines, 1000);
  renderMachines();
  renderLog();
  renderCompleted();
</script>
</body>
</html>
