<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vazifalar — To-do App</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@500&display=swap" rel="stylesheet">
<style>
  :root{
    --bg: #10151a;
    --panel: #171e25;
    --panel-soft: #1d2530;
    --border: #263039;
    --text: #eef2f4;
    --text-muted: #8a97a0;
    --text-dim: #57626a;
    --teal: #4fd1c5;
    --teal-soft: rgba(79,209,197,0.12);
    --rose: #e28b9e;
    --rose-soft: rgba(226,139,158,0.12);
    --display: 'Space Grotesk', sans-serif;
    --sans: 'Inter', sans-serif;
    --mono: 'JetBrains Mono', monospace;
  }

  *{ margin:0; padding:0; box-sizing:border-box; }

  body{
    min-height:100vh;
    background:
      radial-gradient(circle at 15% 10%, rgba(79,209,197,0.06), transparent 40%),
      radial-gradient(circle at 85% 90%, rgba(226,139,158,0.05), transparent 40%),
      var(--bg);
    color: var(--text);
    font-family: var(--sans);
    display:flex;
    align-items:center;
    justify-content:center;
    padding: 32px 16px;
  }

  .app{
    width:100%;
    max-width: 440px;
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 18px;
    padding: 28px 26px 22px;
    box-shadow: 0 30px 70px -25px rgba(0,0,0,0.55);
  }

  /* Header with progress ring */
  .head{
    display:flex;
    align-items:center;
    gap: 18px;
    margin-bottom: 22px;
  }

  .ring-wrap{ position:relative; width:64px; height:64px; flex-shrink:0; }
  .ring-wrap svg{ transform: rotate(-90deg); }
  .ring-bg{ fill:none; stroke: var(--panel-soft); stroke-width:6; }
  .ring-fill{
    fill:none;
    stroke: var(--teal);
    stroke-width:6;
    stroke-linecap:round;
    stroke-dasharray: 163.36;
    stroke-dashoffset: 163.36;
    transition: stroke-dashoffset .5s cubic-bezier(.4,0,.2,1);
  }
  .ring-label{
    position:absolute; inset:0;
    display:flex; align-items:center; justify-content:center;
    font-family: var(--mono);
    font-size: 13px;
    color: var(--teal);
  }

  .head-text h1{
    font-family: var(--display);
    font-size: 20px;
    font-weight:600;
    letter-spacing: -0.01em;
  }
  .head-text p{
    font-size: 12.5px;
    color: var(--text-muted);
    margin-top: 2px;
    font-family: var(--mono);
  }

  /* Input row */
  .add-row{
    display:flex;
    gap: 8px;
    margin-bottom: 18px;
  }
  .add-row input{
    flex:1;
    background: var(--panel-soft);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 12px 14px;
    color: var(--text);
    font-family: var(--sans);
    font-size: 14px;
    outline: none;
    transition: border-color .2s ease;
  }
  .add-row input::placeholder{ color: var(--text-dim); }
  .add-row input:focus{ border-color: var(--teal); }

  .add-btn{
    background: var(--teal);
    color: #0a1613;
    border: none;
    border-radius: 10px;
    width: 44px;
    font-size: 20px;
    font-weight:600;
    cursor:pointer;
    transition: transform .15s ease, filter .15s ease;
  }
  .add-btn:hover{ filter: brightness(1.08); }
  .add-btn:active{ transform: scale(0.94); }

  /* Filters */
  .filters{
    display:flex;
    gap: 6px;
    margin-bottom: 14px;
  }
  .filter-btn{
    font-family: var(--mono);
    font-size: 11.5px;
    color: var(--text-muted);
    background: transparent;
    border: 1px solid var(--border);
    padding: 6px 12px;
    border-radius: 20px;
    cursor:pointer;
    transition: all .2s ease;
  }
  .filter-btn:hover{ color: var(--text); }
  .filter-btn.active{
    background: var(--teal-soft);
    border-color: var(--teal);
    color: var(--teal);
  }

  /* Task list */
  .list{
    display:flex;
    flex-direction:column;
    gap: 6px;
    min-height: 60px;
  }

  .task{
    display:flex;
    align-items:flex-start;
    gap: 12px;
    padding: 12px 10px;
    border-radius: 10px;
    transition: background .2s ease;
    animation: slideIn .25s ease;
  }
  .task:hover{ background: var(--panel-soft); }
  .task:hover .del-btn{ opacity:1; }

  @keyframes slideIn{
    from{ opacity:0; transform: translateY(-6px); }
    to{ opacity:1; transform: translateY(0); }
  }

  .checkbox{
    width: 20px; height:20px;
    border-radius: 6px;
    border: 1.5px solid var(--text-dim);
    flex-shrink:0;
    margin-top: 1px;
    cursor:pointer;
    display:flex;
    align-items:center;
    justify-content:center;
    transition: border-color .2s ease, background .2s ease;
  }
  .checkbox svg{
    width: 12px; height:12px;
    stroke: #0a1613;
  }
  .checkbox path{
    fill:none;
    stroke: #0a1613;
    stroke-width: 3;
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-dasharray: 20;
    stroke-dashoffset: 20;
  }
  .task.done .checkbox{ background: var(--teal); border-color: var(--teal); }
  .task.done .checkbox path{ animation: draw .3s ease forwards; }
  @keyframes draw{ to{ stroke-dashoffset:0; } }

  .task-text{
    flex:1;
    font-size: 14px;
    line-height:1.5;
    color: var(--text);
    word-break: break-word;
    cursor:pointer;
  }
  .task.done .task-text{
    color: var(--text-dim);
    text-decoration: line-through;
    text-decoration-color: var(--text-dim);
  }

  .del-btn{
    opacity:0;
    background:none;
    border:none;
    color: var(--rose);
    font-size: 15px;
    cursor:pointer;
    padding: 2px 4px;
    transition: opacity .2s ease, transform .15s ease;
  }
  .del-btn:hover{ transform: scale(1.15); }

  .empty{
    text-align:center;
    padding: 28px 10px;
    color: var(--text-dim);
    font-size: 13px;
  }

  /* Footer */
  .foot{
    display:flex;
    justify-content:space-between;
    align-items:center;
    margin-top: 16px;
    padding-top: 14px;
    border-top: 1px solid var(--border);
  }
  .foot span{
    font-family: var(--mono);
    font-size: 11.5px;
    color: var(--text-dim);
  }
  .clear-btn{
    background:none;
    border:none;
    color: var(--rose);
    font-size: 12px;
    font-family: var(--sans);
    cursor:pointer;
    opacity: 0.85;
  }
  .clear-btn:hover{ opacity:1; text-decoration: underline; }

  @media (prefers-reduced-motion: reduce){
    .task{ animation:none; }
    .ring-fill{ transition:none; }
  }
</style>
</head>
<body>

<div class="app">

  <div class="head">
    <div class="ring-wrap">
      <svg width="64" height="64" viewBox="0 0 64 64">
        <circle class="ring-bg" cx="32" cy="32" r="26"></circle>
        <circle class="ring-fill" id="ring" cx="32" cy="32" r="26"></circle>
      </svg>
      <div class="ring-label" id="ringLabel">0%</div>
    </div>
    <div class="head-text">
      <h1>Bugungi vazifalar</h1>
      <p id="counter">0 / 0 bajarildi</p>
    </div>
  </div>

  <div class="add-row">
    <input type="text" id="taskInput" placeholder="Yangi vazifa qo'shish..." maxlength="120">
    <button class="add-btn" id="addBtn" aria-label="Qo'shish">+</button>
  </div>

  <div class="filters">
    <button class="filter-btn active" data-filter="all">Hammasi</button>
    <button class="filter-btn" data-filter="active">Faol</button>
    <button class="filter-btn" data-filter="done">Bajarilgan</button>
  </div>

  <div class="list" id="list"></div>

  <div class="foot">
    <span id="leftCount">0 ta vazifa qoldi</span>
    <button class="clear-btn" id="clearBtn">Bajarilganlarni tozalash</button>
  </div>

</div>

<script>
  // Vazifalar shu massivda saqlanadi. Har bir vazifa - bitta object.
  // localStorage orqali sahifani yopib-ochsangiz ham ma'lumot yo'qolmaydi.
  let tasks = JSON.parse(localStorage.getItem('todo-tasks') || '[]');
  let currentFilter = 'all';

  const list = document.getElementById('list');
  const input = document.getElementById('taskInput');
  const addBtn = document.getElementById('addBtn');
  const ring = document.getElementById('ring');
  const ringLabel = document.getElementById('ringLabel');
  const counter = document.getElementById('counter');
  const leftCount = document.getElementById('leftCount');
  const clearBtn = document.getElementById('clearBtn');
  const filterBtns = document.querySelectorAll('.filter-btn');

  const RING_CIRCUMFERENCE = 163.36; // 2 * PI * 26 (radius)

  function save(){
    localStorage.setItem('todo-tasks', JSON.stringify(tasks));
  }

  function addTask(text){
    tasks.push({ id: Date.now(), text: text, done: false });
    save();
    render();
  }

  function toggleTask(id){
    const t = tasks.find(t => t.id === id);
    if (t) t.done = !t.done;
    save();
    render();
  }

  function deleteTask(id){
    tasks = tasks.filter(t => t.id !== id);
    save();
    render();
  }

  function clearDone(){
    tasks = tasks.filter(t => !t.done);
    save();
    render();
  }

  function getFiltered(){
    if (currentFilter === 'active') return tasks.filter(t => !t.done);
    if (currentFilter === 'done') return tasks.filter(t => t.done);
    return tasks;
  }

  function render(){
    const filtered = getFiltered();
    list.innerHTML = '';

    if (filtered.length === 0){
      const emptyMsg = tasks.length === 0
        ? "Hozircha vazifa yo'q. Birinchisini qo'shing!"
        : "Bu bo'limda vazifa yo'q.";
      list.innerHTML = `<div class="empty">${emptyMsg}</div>`;
    }

    filtered.forEach(t => {
      const row = document.createElement('div');
      row.className = 'task' + (t.done ? ' done' : '');
      row.innerHTML = `
        <div class="checkbox" data-id="${t.id}">
          <svg viewBox="0 0 20 20"><path d="M4 10 L8 14 L16 5"></path></svg>
        </div>
        <div class="task-text" data-id="${t.id}">${escapeHtml(t.text)}</div>
        <button class="del-btn" data-id="${t.id}">✕</button>
      `;
      list.appendChild(row);
    });

    // Progress ring va hisoblagichlarni yangilash
    const total = tasks.length;
    const done = tasks.filter(t => t.done).length;
    const percent = total === 0 ? 0 : Math.round((done / total) * 100);

    ring.style.strokeDashoffset = RING_CIRCUMFERENCE - (RING_CIRCUMFERENCE * percent / 100);
    ringLabel.textContent = percent + '%';
    counter.textContent = `${done} / ${total} bajarildi`;
    leftCount.textContent = `${total - done} ta vazifa qoldi`;
  }

  function escapeHtml(str){
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
  }

  // Vazifa qo'shish
  function handleAdd(){
    const value = input.value.trim();
    if (value === '') return;
    addTask(value);
    input.value = '';
    input.focus();
  }

  addBtn.addEventListener('click', handleAdd);
  input.addEventListener('keydown', e => { if (e.key === 'Enter') handleAdd(); });

  // Checkbox / matn bosilganda vazifani bajarilgan deb belgilash
  list.addEventListener('click', e => {
    const checkbox = e.target.closest('.checkbox');
    const text = e.target.closest('.task-text');
    const del = e.target.closest('.del-btn');

    if (checkbox) toggleTask(Number(checkbox.dataset.id));
    else if (text) toggleTask(Number(text.dataset.id));
    else if (del) deleteTask(Number(del.dataset.id));
  });

  clearBtn.addEventListener('click', clearDone);

  filterBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      filterBtns.forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      currentFilter = btn.dataset.filter;
      render();
    });
  });

  render();
</script>

</body>
</html>
