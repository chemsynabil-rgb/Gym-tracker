<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#0f0f1a">
<title>Pro Coach Gym Tracker</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background: #0f0f1a;
  color: #fff;
  min-height: 100vh;
  overflow-x: hidden;
  -webkit-tap-highlight-color: transparent;
}
.app-container { max-width: 480px; margin: 0 auto; padding-bottom: 90px; }
.screen { display: none; padding: 16px; animation: fadeIn 0.3s ease; }
.screen.active { display: block; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
.header { text-align: center; padding: 20px 0 12px; }
.header .emoji { font-size: 48px; margin-bottom: 8px; }
.header h1 { font-size: 24px; font-weight: 800; letter-spacing: -0.5px; }
.header p { color: #888; font-size: 13px; margin-top: 4px; }
.card {
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 16px;
  padding: 16px;
  margin-bottom: 12px;
  transition: all 0.2s;
}
.card:active { transform: scale(0.98); }
.stats-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 16px; }
.stat-box { text-align: center; padding: 14px 8px; }
.stat-value { font-size: 24px; font-weight: 800; }
.stat-label { font-size: 11px; color: #888; margin-top: 2px; }
.day-card {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px;
  border-radius: 14px;
  background: rgba(255,255,255,0.04);
  margin-bottom: 10px;
  cursor: pointer;
  transition: all 0.2s;
}
.day-card:active { transform: scale(0.97); }
.day-indicator { width: 4px; height: 40px; border-radius: 4px; flex-shrink: 0; }
.day-info { flex: 1; }
.day-tag { font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.5px; }
.day-name { font-size: 16px; font-weight: 700; margin-top: 2px; }
.day-muscles { font-size: 12px; color: #888; margin-top: 2px; }
.day-status { font-size: 12px; font-weight: 600; white-space: nowrap; }
.exercise-item {
  background: rgba(255,255,255,0.04);
  border-radius: 14px;
  padding: 14px;
  margin-bottom: 10px;
}
.exercise-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 6px; }
.exercise-name { font-size: 15px; font-weight: 700; }
.exercise-meta { font-size: 12px; color: #888; margin-bottom: 6px; }
.exercise-note { font-size: 11px; color: #666; font-style: italic; margin-bottom: 10px; }
.set-row { display: flex; align-items: center; gap: 8px; margin-bottom: 8px; }
.set-label { font-size: 12px; color: #888; width: 42px; flex-shrink: 0; }
.set-input {
  background: rgba(255,255,255,0.08);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 10px;
  padding: 8px 10px;
  color: #fff;
  font-size: 14px;
  width: 70px;
  text-align: center;
  -webkit-appearance: none;
}
.set-input:focus { outline: none; border-color: #00d4aa; }
.set-sep { color: #555; font-size: 13px; }
.set-check {
  width: 36px;
  height: 36px;
  border-radius: 10px;
  background: rgba(255,255,255,0.08);
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  cursor: pointer;
  flex-shrink: 0;
}
.set-check.done { background: rgba(0,212,170,0.15); }
.progress-container { background: rgba(255,255,255,0.08); height: 6px; border-radius: 6px; overflow: hidden; margin: 12px 0; }
.progress-fill { height: 100%; border-radius: 6px; background: #00d4aa; transition: width 0.4s ease; }
.btn {
  width: 100%;
  padding: 16px;
  border-radius: 14px;
  border: none;
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s;
  -webkit-appearance: none;
}
.btn:active { transform: scale(0.97); }
.btn-primary { background: #00d4aa; color: #0f0f1a; }
.btn-secondary { background: rgba(255,255,255,0.08); color: #fff; }
.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(15,15,26,0.95);
  backdrop-filter: blur(20px);
  border-top: 1px solid rgba(255,255,255,0.06);
  z-index: 100;
  padding-bottom: env(safe-area-inset-bottom, 0);
}
.nav-inner { max-width: 480px; margin: 0 auto; display: flex; justify-content: space-around; padding: 8px 0; }
.nav-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3px;
  padding: 4px 12px;
  background: none;
  border: none;
  color: #666;
  font-size: 10px;
  font-weight: 600;
  cursor: pointer;
  transition: color 0.2s;
}
.nav-item.active { color: #00d4aa; }
.nav-icon { font-size: 22px; line-height: 1; }
.select-box {
  width: 100%;
  padding: 14px;
  border-radius: 14px;
  background: rgba(255,255,255,0.08);
  border: 1px solid rgba(255,255,255,0.1);
  color: #fff;
  font-size: 14px;
  -webkit-appearance: none;
  margin-bottom: 12px;
}
.select-box:focus { outline: none; border-color: #00d4aa; }
.history-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 12px;
  background: rgba(255,255,255,0.04);
  border-radius: 10px;
  margin-bottom: 6px;
}
.history-weight { font-weight: 700; font-size: 14px; }
.history-date { font-size: 11px; color: #888; }
.pr-badge { background: rgba(0,212,170,0.15); color: #00d4aa; font-size: 10px; font-weight: 700; padding: 3px 8px; border-radius: 20px; }
.toast {
  position: fixed;
  bottom: 100px;
  left: 50%;
  transform: translateX(-50%);
  background: #00d4aa;
  color: #0f0f1a;
  padding: 12px 24px;
  border-radius: 12px;
  font-weight: 700;
  font-size: 14px;
  z-index: 200;
  animation: toastUp 0.3s ease;
  white-space: nowrap;
}
@keyframes toastUp { from { transform: translateX(-50%) translateY(20px); opacity: 0; } to { transform: translateX(-50%) translateY(0); opacity: 1; } }
.completed { opacity: 0.5; }
.empty-state { text-align: center; padding: 40px 20px; color: #555; }
.empty-state .emoji { font-size: 40px; margin-bottom: 12px; }
.settings-item {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 16px;
  background: rgba(255,255,255,0.04);
  border-radius: 14px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: all 0.2s;
}
.settings-item:active { transform: scale(0.98); }
.settings-icon { font-size: 24px; }
.settings-text { flex: 1; }
.settings-title { font-size: 14px; font-weight: 700; }
.settings-desc { font-size: 12px; color: #888; margin-top: 2px; }
.tracker-table { width: 100%; border-collapse: collapse; font-size: 11px; }
.tracker-table th { background: rgba(255,255,255,0.08); padding: 8px 4px; text-align: center; font-weight: 700; font-size: 10px; }
.tracker-table td { padding: 8px 4px; text-align: center; border-bottom: 1px solid rgba(255,255,255,0.04); }
.tracker-table tr:nth-child(even) { background: rgba(255,255,255,0.02); }
#file-input { display: none; }
@media (max-width: 400px) {
  .set-input { width: 58px; padding: 8px 4px; font-size: 13px; }
  .stats-grid { gap: 8px; }
  .stat-value { font-size: 20px; }
  .tracker-table { font-size: 10px; }
  .tracker-table th, .tracker-table td { padding: 6px 2px; }
}
</style>
</head>
<body>
<div class="app-container">

<div id="screen-home" class="screen active">
  <div class="header">
    <div class="emoji">💪</div>
    <h1>Pro Coach's Split</h1>
    <p>4-Day Full Body Program</p>
  </div>
  <div class="stats-grid">
    <div class="card stat-box">
      <div class="stat-value" style="color:#00d4aa" id="stat-workouts">0</div>
      <div class="stat-label">Workouts</div>
    </div>
    <div class="card stat-box">
      <div class="stat-value" style="color:#ffd93d" id="stat-volume">0</div>
      <div class="stat-label">Total kg</div>
    </div>
    <div class="card stat-box">
      <div class="stat-value" style="color:#ff6b6b" id="stat-streak">0</div>
      <div class="stat-label">Day Streak</div>
    </div>
  </div>
  <h2 style="font-size:16px;margin-bottom:12px;padding-left:4px;">This Week</h2>
  <div id="week-overview"></div>
  <div class="card" style="background: rgba(0,212,170,0.08); border-color: rgba(0,212,170,0.2); margin-top: 16px;">
    <h3 style="font-size:15px;margin-bottom:6px;">Ready to lift?</h3>
    <p style="font-size:13px;color:#888;margin-bottom:12px;">Pick up where you left off or start fresh.</p>
    <button class="btn btn-primary" onclick="showScreen('workout'); selectDay(getNextDay())">Start Next Workout →</button>
  </div>
  <div id="last-workout" style="display:none;margin-top:16px;">
    <h2 style="font-size:16px;margin-bottom:12px;padding-left:4px;">Last Workout</h2>
    <div class="card">
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <span style="font-weight:700;" id="last-day-name">--</span>
        <span style="font-size:12px;color:#888;" id="last-date">--</span>
      </div>
      <div style="font-size:13px;color:#888;margin-top:4px;" id="last-exercises">--</div>
    </div>
  </div>
</div>

<div id="screen-workout" class="screen">
  <div style="padding: 8px 0 16px;">
    <h1 style="font-size:22px;font-weight:800;">Workout</h1>
    <p style="font-size:13px;color:#888;">Select your training day</p>
  </div>
  <div id="day-selector">
    <div class="day-card" onclick="selectDay(0)">
      <div class="day-indicator" style="background:#00d4aa;"></div>
      <div class="day-info">
        <div class="day-tag" style="color:#00d4aa;">DAY 1</div>
        <div class="day-name">PULL</div>
        <div class="day-muscles">Back · Biceps · Rear Delts</div>
      </div>
      <div class="day-status" id="status-0">⏳</div>
    </div>
    <div class="day-card" onclick="selectDay(1)">
      <div class="day-indicator" style="background:#ff6b6b;"></div>
      <div class="day-info">
        <div class="day-tag" style="color:#ff6b6b;">DAY 2</div>
        <div class="day-name">PUSH</div>
        <div class="day-muscles">Chest · Shoulders · Triceps</div>
      </div>
      <div class="day-status" id="status-1">⏳</div>
    </div>
    <div class="day-card" onclick="selectDay(2)">
      <div class="day-indicator" style="background:#ffd93d;"></div>
      <div class="day-info">
        <div class="day-tag" style="color:#ffd93d;">DAY 3</div>
        <div class="day-name">FULL A</div>
        <div class="day-muscles">Compound Strength</div>
      </div>
      <div class="day-status" id="status-2">⏳</div>
    </div>
    <div class="day-card" onclick="selectDay(3)">
      <div class="day-indicator" style="background:#6bcb77;"></div>
      <div class="day-info">
        <div class="day-tag" style="color:#6bcb77;">DAY 4</div>
        <div class="day-name">FULL B</div>
        <div class="day-muscles">Hypertrophy Focus</div>
      </div>
      <div class="day-status" id="status-3">⏳</div>
    </div>
  </div>
  <div id="active-workout" style="display:none;">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
      <div>
        <h2 style="font-size:18px;font-weight:800;" id="active-title">Day 1</h2>
        <p style="font-size:12px;color:#888;" id="active-subtitle">--</p>
      </div>
      <div style="text-align:right;">
        <div style="font-size:11px;color:#888;">Progress</div>
        <div style="font-size:18px;font-weight:800;color:#00d4aa;"><span id="done-count">0</span>/<span id="total-count">0</span></div>
      </div>
    </div>
    <div class="progress-container"><div class="progress-fill" id="workout-progress" style="width:0%"></div></div>
    <div id="exercise-list"></div>
    <button class="btn btn-primary" id="finish-btn" style="display:none;margin-top:16px;" onclick="finishWorkout()">✅ Finish Workout</button>
  </div>
</div>

<div id="screen-progress" class="screen">
  <div style="padding: 8px 0 16px;">
    <h1 style="font-size:22px;font-weight:800;">Progress</h1>
    <p style="font-size:13px;color:#888;">Track your strength gains</p>
  </div>
  <select class="select-box" id="progress-select" onchange="updateProgress()"><option value="">Select an exercise...</option></select>
  <div id="pr-card" style="display:none;">
    <div class="card" style="display:flex;justify-content:space-between;align-items:center;">
      <div>
        <div style="font-size:12px;color:#888;">Personal Best</div>
        <div style="font-size:24px;font-weight:800;color:#00d4aa;" id="pr-value">--</div>
      </div>
      <div style="text-align:right;">
        <div style="font-size:12px;color:#888;">Last Session</div>
        <div style="font-size:16px;font-weight:700;" id="last-session">--</div>
      </div>
    </div>
  </div>
  <div id="history-section" style="display:none;margin-top:12px;">
    <h3 style="font-size:14px;font-weight:700;margin-bottom:10px;">History</h3>
    <div id="history-list"></div>
  </div>
  <div class="empty-state" id="progress-empty">
    <div class="emoji">📊</div>
    <p>Select an exercise to view your progress</p>
  </div>
</div>

<div id="screen-tracker" class="screen">
  <div style="padding: 8px 0 16px;">
    <h1 style="font-size:22px;font-weight:800;">Tracker</h1>
    <p style="font-size:13px;color:#888;">Log your weights for each week</p>
  </div>
  <div style="overflow-x:auto;">
    <table class="tracker-table" id="tracker-table">
      <thead><tr><th>Exercise</th><th>W1</th><th>W2</th><th>W3</th><th>W4</th></tr></thead>
      <tbody id="tracker-body"></tbody>
    </table>
  </div>
</div>

<div id="screen-settings" class="screen">
  <div style="padding: 8px 0 16px;">
    <h1 style="font-size:22px;font-weight:800;">Settings</h1>
    <p style="font-size:13px;color:#888;">Manage your data</p>
  </div>
  <div class="settings-item" onclick="exportData()">
    <div class="settings-icon">📤</div>
    <div class="settings-text">
      <div class="settings-title">Export Data</div>
      <div class="settings-desc">Save your progress as JSON</div>
    </div>
  </div>
  <div class="settings-item" onclick="document.getElementById('file-input').click()">
    <div class="settings-icon">📥</div>
    <div class="settings-text">
      <div class="settings-title">Import Data</div>
      <div class="settings-desc">Restore from backup file</div>
    </div>
  </div>
  <div class="settings-item" onclick="resetData()">
    <div class="settings-icon">🗑️</div>
    <div class="settings-text">
      <div class="settings-title" style="color:#ff6b6b;">Reset All Data</div>
      <div class="settings-desc">Clear all workouts and progress</div>
    </div>
  </div>
  <div class="card" style="margin-top:16px;">
    <h3 style="font-size:15px;margin-bottom:8px;">About</h3>
    <p style="font-size:13px;color:#888;">Pro Coach's 4-Day Full Body Split</p>
    <p style="font-size:12px;color:#666;margin-top:4px;">Pull · Push · Full A · Full B</p>
    <p style="font-size:12px;color:#666;margin-top:8px;">Data stored locally in your browser.</p>
  </div>
</div>

</div>

<input type="file" id="file-input" accept=".json" onchange="handleImport(event)">

<div class="bottom-nav">
  <div class="nav-inner">
    <button class="nav-item active" onclick="showScreen('home')" id="nav-home">
      <span class="nav-icon">🏠</span><span>Home</span>
    </button>
    <button class="nav-item" onclick="showScreen('workout')" id="nav-workout">
      <span class="nav-icon">💪</span><span>Workout</span>
    </button>
    <button class="nav-item" onclick="showScreen('progress')" id="nav-progress">
      <span class="nav-icon">📊</span><span>Progress</span>
    </button>
    <button class="nav-item" onclick="showScreen('tracker')" id="nav-tracker">
      <span class="nav-icon">📝</span><span>Tracker</span>
    </button>
    <button class="nav-item" onclick="showScreen('settings')" id="nav-settings">
      <span class="nav-icon">⚙️</span><span>Settings</span>
    </button>
  </div>
</div>

<script>
const program = {
  days: [
    {
      name: "Day 1 — PULL", subtitle: "Strength Focus · Heavy 5-8 reps", color: "#00d4aa",
      exercises: [
        { name: "Barbell Deadlift", sets: 3, reps: "5", rest: "3 min", note: "Brace hard. If form breaks, stop the set." },
        { name: "Weighted Pull-Up", sets: 4, reps: "6-8", rest: "2.5 min", note: "Full stretch at bottom, drive elbows down." },
        { name: "Barbell Bent-Over Row", sets: 3, reps: "6-8", rest: "2.5 min", note: "Torso ~45°, pull to lower abs." },
        { name: "Face Pulls", sets: 3, reps: "12-15", rest: "90 sec", note: "External rotation — rear delts, not traps." },
        { name: "Barbell Curl", sets: 3, reps: "8-10", rest: "90 sec", note: "No swinging. If you swing, the weight owns you." },
        { name: "Hanging Leg Raise", sets: 3, reps: "10-12", rest: "60 sec", note: "Control the negative." }
      ]
    },
    {
      name: "Day 2 — PUSH", subtitle: "Strength Focus · Heavy 5-8 reps", color: "#ff6b6b",
      exercises: [
        { name: "Barbell Bench Press", sets: 4, reps: "5", rest: "3 min", note: "Tuck elbows ~75°, drive through feet." },
        { name: "Overhead Press", sets: 3, reps: "6-8", rest: "2.5 min", note: "Glutes tight, no excessive lean." },
        { name: "Incline Dumbbell Press", sets: 3, reps: "8-10", rest: "2 min", note: "30° incline — upper chest, not front delts." },
        { name: "Weighted Dips", sets: 3, reps: "6-8", rest: "2 min", note: "Lean forward for chest, upright for triceps." },
        { name: "Lateral Raise", sets: 4, reps: "12-15", rest: "90 sec", note: "Lead with elbows, pinkies up slightly." },
        { name: "Cable Tricep Pushdown", sets: 3, reps: "10-12", rest: "90 sec", note: "Lock out fully, squeeze for 1 sec." }
      ]
    },
    {
      name: "Day 3 — FULL BODY A", subtitle: "Compound Strength Bias · 6-10 reps", color: "#ffd93d",
      exercises: [
        { name: "Front Squat", sets: 3, reps: "6", rest: "3 min", note: "Torso upright — builds quads and core together." },
        { name: "Weighted Chin-Up", sets: 3, reps: "6-8", rest: "2.5 min", note: "Supinated grip, squeeze biceps at top." },
        { name: "Floor Press", sets: 3, reps: "8", rest: "2 min", note: "Reduces shoulder stress, hammers lockout." },
        { name: "Romanian Deadlift", sets: 3, reps: "8-10", rest: "2 min", note: "Hamstring stretch is the rep. Don't bounce." },
        { name: "Farmer's Walk", sets: 3, reps: "40m", rest: "2 min", note: "Heavy. Grip, core, traps — one movement." }
      ]
    },
    {
      name: "Day 4 — FULL BODY B", subtitle: "Hypertrophy & Weak Points · 8-15 reps", color: "#6bcb77",
      exercises: [
        { name: "Hack Squat / Leg Press", sets: 3, reps: "10-12", rest: "2 min", note: "Feet low on platform — more quads." },
        { name: "Chest-Supported Row", sets: 3, reps: "10-12", rest: "90 sec", note: "No lower back fatigue — pure back volume." },
        { name: "Dumbbell Shoulder Press", sets: 3, reps: "10-12", rest: "90 sec", note: "Full ROM, control the negative." },
        { name: "Leg Curl", sets: 3, reps: "12-15", rest: "90 sec", note: "Isolate hamstrings — they need love too." },
        { name: "Incline Dumbbell Curl", sets: 3, reps: "12-15", rest: "60 sec", note: "Stretch at bottom, no momentum." },
        { name: "Cable Fly (High-to-Low)", sets: 3, reps: "12-15", rest: "60 sec", note: "Squeeze pecs — this is your finisher." }
      ]
    }
  ]
};

function loadData() {
  try { const s = localStorage.getItem('gymTrackerData_v2'); if (s) return JSON.parse(s); } catch(e) {}
  return { workouts: [] };
}
function saveData(d) { localStorage.setItem('gymTrackerData_v2', JSON.stringify(d)); }
let appData = loadData();

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById('screen-' + id).classList.add('active');
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById('nav-' + id).classList.add('active');
  if (id === 'home') renderHome();
  if (id === 'progress') initProgress();
  if (id === 'tracker') renderTracker();
}

function renderHome() {
  const w = appData.workouts || [];
  document.getElementById('stat-workouts').textContent = w.length;
  const vol = w.reduce((s, x) => s + (x.volume || 0), 0);
  document.getElementById('stat-volume').textContent = vol > 1000 ? (vol/1000).toFixed(1) + 'k' : vol;
  document.getElementById('stat-streak').textContent = calcStreak();
  program.days.forEach((d, i) => {
    document.getElementById('status-' + i).textContent = w.some(x => x.dayIndex === i) ? '✅' : '⏳';
  });
  document.getElementById('week-overview').innerHTML = program.days.map((d, i) => {
    const done = w.some(x => x.dayIndex === i);
    return `<div class="day-card" onclick="showScreen('workout'); selectDay(${i})">
      <div class="day-indicator" style="background:${d.color}"></div>
      <div class="day-info">
        <div class="day-tag" style="color:${d.color}">DAY ${i+1}</div>
        <div class="day-name">${d.name.split('—')[1].trim()}</div>
        <div class="day-muscles">${d.exercises.length} exercises</div>
      </div>
      <div class="day-status" style="color:${done ? '#00d4aa' : '#888'}">${done ? '✅ Done' : '⏳ Pending'}</div>
    </div>`;
  }).join('');
  if (w.length > 0) {
    const last = w[w.length - 1];
    document.getElementById('last-workout').style.display = 'block';
    document.getElementById('last-day-name').textContent = program.days[last.dayIndex].name;
    document.getElementById('last-date').textContent = new Date(last.date).toLocaleDateString();
    document.getElementById('last-exercises').textContent = (last.exercises?.length || 0) + ' exercises logged';
  }
}

function calcStreak() {
  const dates = [...new Set((appData.workouts || []).map(w => new Date(w.date).toDateString()))].sort();
  if (dates.length === 0) return 0;
  let streak = 1;
  for (let i = dates.length - 1; i > 0; i--) {
    const d1 = new Date(dates[i]), d2 = new Date(dates[i-1]);
    if ((d1 - d2) / (1000*60*60*24) <= 1) streak++; else break;
  }
  return streak;
}

function getNextDay() {
  const w = appData.workouts || [];
  return w.length === 0 ? 0 : (w[w.length - 1].dayIndex + 1) % 4;
}

let currentDay = -1;
function selectDay(idx) {
  currentDay = idx;
  const day = program.days[idx];
  document.getElementById('day-selector').style.display = 'none';
  document.getElementById('active-workout').style.display = 'block';
  document.getElementById('active-title').textContent = day.name;
  document.getElementById('active-title').style.color = day.color;
  document.getElementById('active-subtitle').textContent = day.subtitle;
  const totalSets = day.exercises.reduce((s, e) => s + e.sets, 0);
  document.getElementById('total-count').textContent = totalSets;
  document.getElementById('done-count').textContent = '0';
  document.getElementById('workout-progress').style.width = '0%';
  const prev = (appData.workouts || []).filter(w => w.dayIndex === idx).pop();
  const prevMap = {};
  if (prev) prev.exercises.forEach(e => { prevMap[e.name] = { weight: e.weight, reps: e.reps }; });
  document.getElementById('exercise-list').innerHTML = day.exercises.map((ex, ei) => {
    const pd = prevMap[ex.name];
    let setsHTML = '';
    for (let s = 0; s < ex.sets; s++) {
      const wVal = pd && s === 0 ? pd.weight : '';
      const rVal = pd && s === 0 ? pd.reps : '';
      setsHTML += `<div class="set-row">
        <span class="set-label">Set ${s+1}</span>
        <input type="number" class="set-input" placeholder="kg" id="w-${ei}-${s}" value="${wVal}" onchange="checkDone()">
        <span class="set-sep">×</span>
        <input type="number" class="set-input" placeholder="reps" id="r-${ei}-${s}" value="${rVal}" onchange="checkDone()">
        <button class="set-check" id="c-${ei}-${s}" onclick="toggleCheck(${ei},${s})">⭕</button>
      </div>`;
    }
    return `<div class="exercise-item" id="ex-${ei}">
      <div class="exercise-header">
        <div><div class="exercise-name">${ex.name}</div><div class="exercise-meta">${ex.sets} sets × ${ex.reps} reps · ${ex.rest}</div></div>
        <span id="ex-done-${ei}"></span>
      </div>
      <div class="exercise-note">${ex.note}</div>
      ${setsHTML}
    </div>`;
  }).join('');
  checkDone();
}

function toggleCheck(ei, si) {
  const btn = document.getElementById('c-' + ei + '-' + si);
  const done = btn.textContent === '✅';
  btn.textContent = done ? '⭕' : '✅';
  btn.classList.toggle('done', !done);
  checkDone();
}

function checkDone() {
  const day = program.days[currentDay];
  let doneSets = 0, totalSets = 0;
  day.exercises.forEach((ex, ei) => {
    let exDone = true;
    for (let s = 0; s < ex.sets; s++) {
      totalSets++;
      const c = document.getElementById('c-' + ei + '-' + s).textContent === '✅';
      const w = document.getElementById('w-' + ei + '-' + s).value;
      const r = document.getElementById('r-' + ei + '-' + s).value;
      if (c && w && r) doneSets++; else exDone = false;
    }
    document.getElementById('ex-done-' + ei).textContent = exDone ? '✅' : '';
    document.getElementById('ex-' + ei).classList.toggle('completed', exDone);
  });
  document.getElementById('done-count').textContent = doneSets;
  document.getElementById('workout-progress').style.width = totalSets > 0 ? (doneSets/totalSets*100) + '%' : '0%';
  document.getElementById('finish-btn').style.display = doneSets === totalSets && totalSets > 0 ? 'block' : 'none';
}

function finishWorkout() {
  const day = program.days[currentDay];
  const exercises = [];
  let volume = 0;
  day.exercises.forEach((ex, ei) => {
    for (let s = 0; s < ex.sets; s++) {
      const w = parseFloat(document.getElementById('w-' + ei + '-' + s).value) || 0;
      const r = parseInt(document.getElementById('r-' + ei + '-' + s).value) || 0;
      if (w > 0 && r > 0) {
        volume += w * r;
        if (!exercises.find(e => e.name === ex.name)) exercises.push({ name: ex.name, weight: w, reps: r });
      }
    }
  });
  appData.workouts = appData.workouts || [];
  appData.workouts.push({ dayIndex: currentDay, date: new Date().toISOString(), exercises: exercises, volume: volume, completed: true });
  saveData(appData);
  showToast('Workout saved! 💪');
  showScreen('home');
  document.getElementById('day-selector').style.display = 'block';
  document.getElementById('active-workout').style.display = 'none';
}

function initProgress() {
  const sel = document.getElementById('progress-select');
  sel.innerHTML = '<option value="">Select an exercise...</option>';
  const all = new Set();
  program.days.forEach(d => d.exercises.forEach(e => all.add(e.name)));
  [...all].sort().forEach(n => { const o = document.createElement('option'); o.value = n; o.textContent = n; sel.appendChild(o); });
}

function updateProgress() {
  const name = document.getElementById('progress-select').value;
  if (!name) {
    document.getElementById('progress-empty').style.display = 'block';
    document.getElementById('pr-card').style.display = 'none';
    document.getElementById('history-section').style.display = 'none';
    return;
  }
  document.getElementById('progress-empty').style.display = 'none';
  document.getElementById('pr-card').style.display = 'block';
  document.getElementById('history-section').style.display = 'block';
  const history = (appData.workouts || []).map(w => {
    const ex = w.exercises.find(e => e.name === name);
    return ex ? { date: w.date, weight: ex.weight, reps: ex.reps } : null;
  }).filter(Boolean);
  if (history.length === 0) {
    document.getElementById('pr-value').textContent = '--';
    document.getElementById('last-session').textContent = '--';
    document.getElementById('history-list').innerHTML = '<div style="color:#666;font-size:13px;">No data yet. Log a workout!</div>';
    return;
  }
  const maxW = Math.max(...history.map(h => h.weight));
  const last = history[history.length - 1];
  document.getElementById('pr-value').textContent = maxW + ' kg';
  document.getElementById('last-session').textContent = last.weight + ' kg × ' + last.reps;
  document.getElementById('history-list').innerHTML = history.slice().reverse().map(h => `
    <div class="history-item">
      <div><div class="history-weight">${h.weight} kg × ${h.reps}</div><div class="history-date">${new Date(h.date).toLocaleDateString()}</div></div>
      ${h.weight === maxW ? '<span class="pr-badge">PR</span>' : ''}
    </div>`).join('');
}

function renderTracker() {
  const tbody = document.getElementById('tracker-body');
  const allEx = [];
  program.days.forEach(d => d.exercises.forEach(e => { if (!allEx.find(x => x.name === e.name)) allEx.push(e); }));
  tbody.innerHTML = allEx.map(e => {
    const weeks = [1,2,3,4].map(() => {
      const entry = (appData.workouts || []).find(x => x.exercises.find(ex => ex.name === e.name));
      const ex = entry?.exercises.find(ex => ex.name === e.name);
      return ex ? `<td>${ex.weight}x${ex.reps}</td>` : '<td style="color:#444">—</td>';
    }).join('');
    return `<tr><td style="text-align:left;font-weight:600;font-size:10px;">${e.name}</td>${weeks}</tr>`;
  }).join('');
}

function exportData() {
  const blob = new Blob([JSON.stringify(appData, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'gym-tracker-' + new Date().toISOString().split('T')[0] + '.json';
  a.click();
  URL.revokeObjectURL(url);
  showToast('Data exported! 📤');
}

function handleImport(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = function(ev) {
    try { appData = JSON.parse(ev.target.result); saveData(appData); showToast('Data imported! 📥'); renderHome(); }
    catch(err) { showToast('Invalid file ❌'); }
  };
  reader.readAsText(file);
  e.target.value = '';
}

function resetData() {
  if (confirm('Delete ALL data? This cannot be undone.')) {
    appData = { workouts: [] };
    saveData(appData);
    showToast('Data cleared 🗑️');
    renderHome();
  }
}

function showToast(msg) {
  const t = document.createElement('div');
  t.className = 'toast';
  t.textContent = msg;
  document.body.appendChild(t);
  setTimeout(() => t.remove(), 2500);
}

renderHome();
</script>
</body>
</html>
