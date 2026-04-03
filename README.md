<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Syntax OS v1.0</title>
  <link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@300;400;500;600;700&family=Share+Tech+Mono&family=Exo+2:wght@200;300;400;500&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    :root {
      --bg: #050a0f;
      --bg2: #0a1520;
      --bg3: #0d1e2e;
      --accent: #00d4ff;
      --accent2: #0099bb;
      --accent3: #00ff88;
      --text: #c8e8f0;
      --text2: #6a9ab0;
      --text3: #3a6478;
      --border: #0d3a52;
      --border2: #1a5570;
      --glow: rgba(0, 212, 255, 0.15);
      --red: #ff3d5a;
      --amber: #ffaa00;
      --green: #00ff88;
    }
    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Exo 2', sans-serif;
      font-weight: 300;
      min-height: 100vh;
      overflow-x: hidden;
    }
    
    /* BACKGROUND EFFECTS */
    .scanline {
      position: fixed; top: 0; left: 0; right: 0; bottom: 0;
      background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px);
      pointer-events: none; z-index: 9999;
    }
    .grid-bg {
      position: fixed; top: 0; left: 0; right: 0; bottom: 0;
      background-image: linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px), linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px);
      background-size: 40px 40px;
      pointer-events: none; z-index: 0;
    }
    
    .app { position: relative; z-index: 1; min-height: 100vh; display: flex; flex-direction: column; }

    /* TOP BAR */
    .topbar {
      display: flex; align-items: center; justify-content: space-between;
      padding: 12px 24px; border-bottom: 1px solid var(--border);
      background: rgba(5, 10, 15, 0.95); backdrop-filter: blur(10px);
      position: sticky; top: 0; z-index: 100;
    }
    .topbar-left { display: flex; align-items: center; gap: 20px; }
    .logo {
      font-family: 'Rajdhani', sans-serif; font-size: 22px; font-weight: 700;
      color: var(--accent); letter-spacing: 4px; text-transform: uppercase;
    }
    .logo span { color: var(--text2); font-weight: 300; }
    .status-dot {
      width: 6px; height: 6px; border-radius: 50%; background: var(--green);
      box-shadow: 0 0 8px var(--green); animation: pulse 2s infinite;
    }
    @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: 0.4; } }
    .topbar-center { font-family: 'Share Tech Mono', monospace; font-size: 13px; color: var(--text2); display: flex; gap: 24px; }
    .topbar-right { display: flex; align-items: center; gap: 12px; }
    .time-display { font-family: 'Share Tech Mono', monospace; font-size: 18px; color: var(--accent); letter-spacing: 2px; }
    .date-display { font-size: 11px; color: var(--text3); letter-spacing: 1px; text-align: right; }

    /* NAV */
    .nav {
      display: flex; gap: 2px; padding: 0 24px; background: var(--bg);
      border-bottom: 1px solid var(--border);
    }
    .nav-btn {
      padding: 10px 20px; font-family: 'Rajdhani', sans-serif; font-size: 14px;
      letter-spacing: 2px; text-transform: uppercase; color: var(--text3);
      background: none; border: none; cursor: pointer; border-bottom: 2px solid transparent;
      transition: all 0.2s;
    }
    .nav-btn:hover { color: var(--text); border-bottom-color: var(--border2); }
    .nav-btn.active { color: var(--accent); border-bottom-color: var(--accent); }

    /* MAIN */
    .main { display: flex; flex: 1; gap: 0; }
    .sidebar { width: 280px; min-width: 280px; border-right: 1px solid var(--border); display: flex; flex-direction: column; background: var(--bg); }
    .content { flex: 1; overflow: hidden; position: relative; }

    /* PANELS */
    .panel { display: none; animation: fadeIn 0.3s ease-out; }
    .panel.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

    /* SIDEBAR SECTIONS */
    .sidebar-section { border-bottom: 1px solid var(--border); padding: 16px; }
    .section-label {
      font-family: 'Rajdhani', sans-serif; font-size: 11px; letter-spacing: 3px;
      text-transform: uppercase; color: var(--text3); margin-bottom: 12px;
      display: flex; align-items: center; gap: 8px;
    }
    .section-label::after { content: ''; flex: 1; height: 1px; background: var(--border); }

    /* AI INPUT */
    .ai-input-wrap { padding: 16px; border-bottom: 1px solid var(--border); }
    .ai-input {
      width: 100%; background: var(--bg2); border: 1px solid var(--border); border-radius: 2px;
      padding: 10px 12px; color: var(--text); font-family: 'Exo 2', sans-serif; font-size: 13px;
      outline: none; resize: none; transition: border-color 0.2s;
    }
    .ai-input:focus { border-color: var(--accent); }
    .ai-input::placeholder { color: var(--text3); }
    .ai-actions { display: flex; gap: 8px; margin-top: 8px; }
    
    .btn {
      padding: 7px 14px; font-family: 'Rajdhani', sans-serif; font-size: 12px;
      letter-spacing: 2px; text-transform: uppercase; border-radius: 2px;
      cursor: pointer; transition: all 0.2s;
    }
    .btn-primary { background: var(--accent); color: var(--bg); border: 1px solid var(--accent); font-weight: 600; flex: 1; }
    .btn-primary:hover { background: transparent; color: var(--accent); }
    .btn-ghost { background: transparent; color: var(--text2); border: 1px solid var(--border); }
    .btn-ghost:hover { border-color: var(--accent); color: var(--accent); }
    .btn-voice { background: transparent; color: var(--text3); border: 1px solid var(--border); padding: 7px 10px; font-size: 16px; }
    .btn-voice.listening { color: var(--red); border-color: var(--red); animation: pulse 1s infinite; }

    /* MINI CALENDAR */
    .mini-cal { padding: 16px; border-bottom: 1px solid var(--border); }
    .mini-cal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
    .mini-cal-title { font-family: 'Rajdhani', sans-serif; font-size: 14px; letter-spacing: 2px; color: var(--text); }
    .mini-cal-nav { background: none; border: none; color: var(--text3); cursor: pointer; font-size: 14px; padding: 2px 6px; transition: color 0.2s; }
    .mini-cal-nav:hover { color: var(--accent); }
    .mini-cal-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 2px; }
    .mini-day-label { text-align: center; font-size: 9px; letter-spacing: 1px; color: var(--text3); padding: 2px 0; font-family: 'Rajdhani', sans-serif; text-transform: uppercase; }
    .mini-day { text-align: center; font-size: 11px; padding: 4px 2px; cursor: pointer; border-radius: 2px; transition: all 0.15s; color: var(--text2); }
    .mini-day:hover { background: var(--bg3); color: var(--text); }
    .mini-day.today { background: var(--accent); color: var(--bg); font-weight: 600; }
    .mini-day.selected { border: 1px solid var(--accent); color: var(--accent); }
    .mini-day.other-month { color: var(--text3); opacity: 0.4; }
    .mini-day.has-event::after { content: ''; display: block; width: 3px; height: 3px; border-radius: 50%; background: var(--accent3); margin: 1px auto 0; }

    /* GOALS */
    .goal-item {
      display: flex; align-items: flex-start; gap: 10px; padding: 10px;
      background: var(--bg2); border: 1px solid var(--border); border-radius: 2px;
      margin-bottom: 8px; cursor: pointer; transition: border-color 0.2s;
    }
    .goal-item:hover { border-color: var(--border2); }
    .goal-bar-wrap { flex: 1; }
    .goal-name { font-size: 12px; color: var(--text); margin-bottom: 4px; }
    .goal-meta { font-size: 10px; color: var(--text3); margin-bottom: 6px; font-family: 'Share Tech Mono', monospace; }
    .goal-bar { height: 2px; background: var(--bg3); border-radius: 1px; overflow: hidden; }
    .goal-bar-fill { height: 100%; background: var(--accent); border-radius: 1px; transition: width 0.5s; max-width: 100%; }

    /* MAIN CALENDAR VIEW */
    .cal-header {
      display: flex; justify-content: space-between; align-items: center;
      padding: 20px 24px; border-bottom: 1px solid var(--border); background: var(--bg);
    }
    .cal-title { font-family: 'Rajdhani', sans-serif; font-size: 24px; letter-spacing: 3px; color: var(--text); }
    .cal-controls { display: flex; gap: 8px; align-items: center; }
    .view-btns { display: flex; border: 1px solid var(--border); border-radius: 2px; overflow: hidden; }
    .view-btn {
      padding: 6px 14px; background: none; border: none; font-family: 'Rajdhani', sans-serif;
      font-size: 12px; letter-spacing: 1px; color: var(--text3); cursor: pointer; transition: all 0.2s;
    }
    .view-btn.active { background: var(--accent); color: var(--bg); }
    .view-btn:hover:not(.active) { background: var(--bg2); color: var(--text); }

    /* WEEK VIEW */
    .week-grid {
      display: grid; grid-template-columns: 60px repeat(7, 1fr);
      height: calc(100vh - 180px); overflow-y: auto;
    }
    .week-grid::-webkit-scrollbar { width: 4px; }
    .week-grid::-webkit-scrollbar-track { background: var(--bg); }
    .week-grid::-webkit-scrollbar-thumb { background: var(--border2); }
    .time-col { border-right: 1px solid var(--border); }
    .time-slot { height: 60px; border-bottom: 1px solid var(--border); display: flex; align-items: flex-start; padding: 4px 8px 0; font-family: 'Share Tech Mono', monospace; font-size: 10px; color: var(--text3); }
    .day-col { border-right: 1px solid var(--border); position: relative; }
    .day-col-header { padding: 10px 8px; border-bottom: 1px solid var(--border); text-align: center; position: sticky; top: 0; background: var(--bg); z-index: 10; }
    .day-col-name { font-family: 'Rajdhani', sans-serif; font-size: 11px; letter-spacing: 2px; color: var(--text3); }
    .day-col-date { font-size: 20px; font-weight: 200; color: var(--text); }
    .day-col-date.today {
      background: var(--accent); color: var(--bg); width: 32px; height: 32px;
      border-radius: 50%; display: flex; align-items: center; justify-content: center;
      margin: 0 auto; font-size: 14px; font-weight: 600;
    }
    .day-slot { height: 60px; border-bottom: 1px solid rgba(13, 58, 82, 0.5); position: relative; }
    
    .event {
      position: absolute; left: 2px; right: 2px; border-radius: 2px; padding: 3px 6px;
      font-size: 11px; cursor: pointer; border-left: 2px solid; transition: opacity 0.2s; overflow: hidden;
    }
    .event:hover { opacity: 0.8; z-index: 5; }
    .event.study { background: rgba(0, 212, 255, 0.1); border-color: var(--accent); color: var(--accent); }
    .event.personal { background: rgba(0, 255, 136, 0.1); border-color: var(--green); color: var(--green); }
    .event.goal { background: rgba(255, 170, 0, 0.1); border-color: var(--amber); color: var(--amber); }
    .event.break { background: rgba(255, 61, 90, 0.08); border-color: var(--red); color: var(--red); }
    .event-name { font-weight: 500; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .event-time { font-family: 'Share Tech Mono', monospace; font-size: 9px; opacity: 0.7; }

    /* FOCUS MODE */
    .focus-overlay {
      position: fixed; top: 0; left: 0; right: 0; bottom: 0;
      background: #000; z-index: 1000; display: none; flex-direction: column;
      align-items: center; justify-content: center;
    }
    .focus-overlay.active { display: flex; }
    .focus-task { font-family: 'Rajdhani', sans-serif; font-size: 48px; letter-spacing: 4px; color: var(--accent); text-transform: uppercase; margin-bottom: 16px; text-align: center; }
    .focus-time { font-family: 'Share Tech Mono', monospace; font-size: 72px; color: var(--text); margin-bottom: 40px; letter-spacing: 4px; }
    .focus-actions { display: flex; gap: 16px; }
    .focus-status { position: absolute; top: 24px; right: 24px; font-family: 'Share Tech Mono', monospace; font-size: 12px; color: var(--text3); }
    .focus-corner { position: absolute; top: 0; left: 0; width: 80px; height: 80px; border-top: 2px solid var(--accent); border-left: 2px solid var(--accent); }
    .focus-corner.br { top: auto; left: auto; bottom: 0; right: 0; border: none; border-bottom: 2px solid var(--accent); border-right: 2px solid var(--accent); }

    /* SKIP MODAL */
    .modal-overlay {
      position: fixed; top: 0; left: 0; right: 0; bottom: 0;
      background: rgba(0, 0, 0, 0.8); z-index: 500; display: none;
      align-items: center; justify-content: center; backdrop-filter: blur(5px);
    }
    .modal-overlay.active { display: flex; }
    .modal {
      background: var(--bg2); border: 1px solid var(--border2); border-radius: 2px;
      padding: 24px; width: 400px; box-shadow: 0 0 40px rgba(0, 212, 255, 0.1);
    }
    .modal-title { font-family: 'Rajdhani', sans-serif; font-size: 18px; letter-spacing: 2px; color: var(--accent); margin-bottom: 8px; }
    .modal-sub { font-size: 13px; color: var(--text2); margin-bottom: 20px; }
    .skip-reasons { display: flex; flex-direction: column; gap: 8px; margin-bottom: 20px; }
    .skip-reason { padding: 10px 14px; background: var(--bg3); border: 1px solid var(--border); border-radius: 2px; font-size: 13px; color: var(--text2); cursor: pointer; transition: all 0.2s; }
    .skip-reason:hover { border-color: var(--accent); color: var(--accent); }

    /* MORNING BRIEF */
    .brief-overlay {
      position: fixed; top: 0; left: 0; right: 0; bottom: 0;
      background: rgba(0, 0, 0, 0.95); z-index: 600; display: flex;
      align-items: center; justify-content: center; flex-direction: column;
    }
    .brief-content { max-width: 600px; width: 90%; text-align: center; }
    .brief-greeting { font-family: 'Rajdhani', sans-serif; font-size: 36px; letter-spacing: 6px; color: var(--accent); margin-bottom: 8px; text-transform: uppercase; }
    .brief-time { font-family: 'Share Tech Mono', monospace; font-size: 56px; color: var(--text); margin-bottom: 32px; letter-spacing: 4px; }
    .brief-fact {
      background: var(--bg2); border: 1px solid var(--border2); border-radius: 2px;
      padding: 20px 24px; margin-bottom: 24px; text-align: left; border-left: 3px solid var(--accent);
    }
    .brief-fact-label { font-family: 'Rajdhani', sans-serif; font-size: 10px; letter-spacing: 3px; color: var(--text3); margin-bottom: 8px; }
    .brief-fact-text { font-size: 14px; color: var(--text); line-height: 1.6; }
    .brief-stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-bottom: 24px; }
    .brief-stat { background: var(--bg2); border: 1px solid var(--border); border-radius: 2px; padding: 14px; }
    .brief-stat-val { font-family: 'Share Tech Mono', monospace; font-size: 20px; color: var(--accent); margin-bottom: 4px; }
    .brief-stat-label { font-size: 10px; letter-spacing: 2px; color: var(--text3); text-transform: uppercase; }
    .brief-corner { position: absolute; top: 24px; left: 24px; font-family: 'Rajdhani', sans-serif; font-size: 12px; letter-spacing: 3px; color: var(--text3); }

    /* GOALS PANEL */
    .goals-panel { padding: 24px; }
    .goals-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 16px; margin-top: 20px; }
    .goal-card { background: var(--bg2); border: 1px solid var(--border); border-radius: 2px; padding: 20px; transition: border-color 0.2s; }
    .goal-card:hover { border-color: var(--border2); }
    .goal-card-name { font-family: 'Rajdhani', sans-serif; font-size: 18px; letter-spacing: 2px; color: var(--text); margin-bottom: 4px; }
    .goal-card-deadline { font-family: 'Share Tech Mono', monospace; font-size: 11px; color: var(--text3); margin-bottom: 16px; }
    .goal-progress { margin-bottom: 12px; }
    .goal-progress-bar { height: 3px; background: var(--bg3); border-radius: 1px; margin-top: 6px; overflow: hidden; }
    .goal-progress-fill { height: 100%; background: linear-gradient(90deg, var(--accent2), var(--accent)); border-radius: 1px; }
    .goal-schedule { font-size: 12px; color: var(--text2); line-height: 1.8; }
    .goal-schedule span { color: var(--accent); font-family: 'Share Tech Mono', monospace; }
    .pattern-alert { margin-top: 12px; padding: 10px; background: rgba(255, 170, 0, 0.05); border: 1px solid rgba(255, 170, 0, 0.2); border-radius: 2px; font-size: 11px; color: var(--amber); }

    /* NIGHT REVIEW */
    .review-panel { padding: 24px; max-width: 600px; }
    .review-q { margin-bottom: 20px; }
    .review-q-label { font-family: 'Rajdhani', sans-serif; font-size: 13px; letter-spacing: 2px; color: var(--text2); margin-bottom: 8px; text-transform: uppercase; }
    .review-input {
      width: 100%; background: var(--bg2); border: 1px solid var(--border);
      border-radius: 2px; padding: 12px; color: var(--text); font-family: 'Exo 2', sans-serif;
      font-size: 13px; outline: none; resize: vertical; min-height: 80px; transition: border-color 0.2s;
    }
    .review-input:focus { border-color: var(--accent); }

    /* HABIT LOG */
    .habit-row { display: flex; align-items: center; justify-content: space-between; padding: 12px 0; border-bottom: 1px solid var(--border); }
    .habit-name { font-size: 13px; color: var(--text2); }
    .habit-dots { display: flex; gap: 6px; }
    .habit-dot {
      width: 24px; height: 24px; border-radius: 50%; background: var(--bg3); border: 1px solid var(--border);
      cursor: pointer; transition: all 0.2s; display: flex; align-items: center; justify-content: center;
      font-size: 10px; color: var(--text3);
    }
    .habit-dot.done { background: var(--accent); border-color: var(--accent); color: var(--bg); }
    .habit-dot.skip { background: rgba(255, 61, 90, 0.2); border-color: var(--red); color: var(--red); }
    .streak { font-family: 'Share Tech Mono', monospace; font-size: 11px; color: var(--green); }

    /* OVERWHELMED BTN */
    .overwhelmed-btn {
      width: 100%; padding: 10px; background: rgba(255, 61, 90, 0.05); border: 1px solid rgba(255, 61, 90, 0.3);
      border-radius: 2px; color: var(--red); font-family: 'Rajdhani', sans-serif; font-size: 11px;
      letter-spacing: 2px; cursor: pointer; transition: all 0.2s; text-transform: uppercase;
    }
    .overwhelmed-btn:hover { background: rgba(255, 61, 90, 0.1); }

    /* TAGS */
    .tag { display: inline-block; padding: 2px 8px; font-family: 'Rajdhani', sans-serif; font-size: 10px; letter-spacing: 1px; border-radius: 1px; text-transform: uppercase; }
    .tag-study { background: rgba(0, 212, 255, 0.1); color: var(--accent); border: 1px solid rgba(0, 212, 255, 0.2); }
    .tag-goal { background: rgba(255, 170, 0, 0.1); color: var(--amber); border: 1px solid rgba(255, 170, 0, 0.2); }
    .tag-ride { background: rgba(0, 255, 136, 0.1); color: var(--green); border: 1px solid rgba(0, 255, 136, 0.2); }

    /* AI RESPONSE */
    .ai-response {
      margin-top: 8px; padding: 10px 12px; background: rgba(0, 212, 255, 0.04);
      border-left: 2px solid var(--accent); font-size: 12px; color: var(--text2);
      line-height: 1.6; border-radius: 0 2px 2px 0; display: none;
    }
    .ai-response.visible { display: block; animation: fadeIn 0.3s; }
    .ai-typing { display: flex; gap: 4px; align-items: center; padding: 4px 0; }
    .ai-dot { width: 4px; height: 4px; border-radius: 50%; background: var(--accent); animation: typing 1.2s infinite; }
    .ai-dot:nth-child(2) { animation-delay: 0.2s; }
    .ai-dot:nth-child(3) { animation-delay: 0.4s; }
    @keyframes typing { 0%, 60%, 100% { transform: translateY(0); } 30% { transform: translateY(-4px); } }

    /* MOMENTUM */
    .momentum-row { display: flex; gap: 4px; margin-top: 8px; }
    .m-block { flex: 1; height: 20px; border-radius: 1px; background: var(--bg3); border: 1px solid var(--border); transition: all 0.3s; }
    .m-block.done { background: var(--accent); border-color: var(--accent); }
    .m-block.skip { background: var(--red); border-color: var(--red); opacity: 0.5; }

    /* SCROLLBAR */
    ::-webkit-scrollbar { width: 4px; height: 4px; }
    ::-webkit-scrollbar-track { background: var(--bg); }
    ::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 2px; }

    /* RESPONSIVE */
    @media(max-width: 768px) {
      .sidebar { width: 100%; min-width: unset; border-right: none; border-bottom: 1px solid var(--border); }
      .main { flex-direction: column; }
      .week-grid { grid-template-columns: 40px repeat(3, 1fr); }
      .cal-header { flex-direction: column; gap: 12px; align-items: flex-start; }
    }
  </style>
</head>
<body>
  <div class="scanline"></div>
  <div class="grid-bg"></div>

  <div class="brief-overlay" id="briefOverlay">
    <div class="brief-corner" id="briefCorner">SYNTAX OS v1.0</div>
    <div class="brief-content">
      <div class="brief-greeting" id="briefGreeting">Loading...</div>
      <div class="brief-time" id="briefClock">--:--</div>
      <div class="brief-stats">
        <div class="brief-stat">
          <div class="brief-stat-val" id="briefTasks">4</div>
          <div class="brief-stat-label">Tasks Today</div>
        </div>
        <div class="brief-stat">
          <div class="brief-stat-val" id="briefGoals">2</div>
          <div class="brief-stat-label">Goals Active</div>
        </div>
        <div class="brief-stat">
          <div class="brief-stat-val">29°C</div>
          <div class="brief-stat-label">Kalyan</div>
        </div>
      </div>
      <div class="brief-fact">
        <div class="brief-fact-label">// power fact</div>
        <div class="brief-fact-text" id="briefFact">Loading intelligence feed...</div>
      </div>
      <button class="btn btn-primary" style="width:100%;font-size:14px;padding:14px;" onclick="closeBrief()">ENTER COMMAND CENTER</button>
    </div>
  </div>

  <div class="focus-overlay" id="focusOverlay">
    <div class="focus-corner"></div>
    <div class="focus-corner br"></div>
    <div class="focus-status" id="focusStatus">FOCUS MODE ACTIVE // DND ON</div>
    <div class="focus-task" id="focusTaskName">Deep Work Session</div>
    <div class="focus-time" id="focusTimer">25:00</div>
    <div style="font-size:12px;color:var(--text3);margin-bottom:32px;letter-spacing:2px;font-family:'Share Tech Mono',monospace">DISTRACTION SHIELD ACTIVE</div>
    <div class="focus-actions">
      <button class="btn btn-ghost" onclick="openSpotify()">♪ SPOTIFY</button>
      <button class="btn btn-primary" onclick="pauseFocus()" id="focusPauseBtn">PAUSE</button>
      <button class="btn btn-ghost" onclick="endFocus()">EXIT FOCUS</button>
    </div>
  </div>

  <div class="modal-overlay" id="skipModal">
    <div class="modal">
      <div class="modal-title">// TASK SKIPPED</div>
      <div class="modal-sub">I noticed you pushed "<span id="skippedTask">Maths Study</span>". What happened?</div>
      <div class="skip-reasons" id="skipReasons">
        <div class="skip-reason" onclick="logSkip('low energy')">Low energy / tired</div>
        <div class="skip-reason" onclick="logSkip('anxious')">Feeling anxious / overwhelmed</div>
        <div class="skip-reason" onclick="logSkip('distracted')">Got distracted</div>
        <div class="skip-reason" onclick="logSkip('not ready')">Not mentally ready</div>
        <div class="skip-reason" onclick="logSkip('other')">Something else came up</div>
      </div>
      <button class="btn btn-ghost" style="width:100%;" onclick="closeSkipModal()">CLOSE</button>
    </div>
  </div>

  <div class="app">
    <div class="topbar">
      <div class="topbar-left">
        <div class="logo">SYNTAX <span>OS</span></div>
        <div class="status-dot"></div>
      </div>
      <div class="topbar-center">
        <span id="topWeather">☁ 29°C · KALYAN</span>
        <span id="topGoalSnippet">▸ MATHS — DAY 12 OF 180</span>
      </div>
      <div class="topbar-right">
        <div>
          <div class="time-display" id="topClock">--:--</div>
          <div class="date-display" id="topDate">LOADING...</div>
        </div>
      </div>
    </div>

    <div class="nav">
      <button class="nav-btn active" onclick="switchPanel('calendar',this)">Calendar</button>
      <button class="nav-btn" onclick="switchPanel('goals',this)">Goals</button>
      <button class="nav-btn" onclick="switchPanel('review',this)">Night Review</button>
      <button class="nav-btn" onclick="switchPanel('habits',this)">Habits</button>
    </div>

    <div class="main">
      <div class="sidebar">
        <div class="ai-input-wrap">
          <div class="section-label">// ai command</div>
          <textarea class="ai-input" id="aiInput" rows="2" placeholder="at 3pm I need to study maths..."></textarea>
          <div class="ai-actions">
            <button class="btn btn-voice" id="voiceBtn" onclick="toggleVoice()" title="Voice Input">🎤</button>
            <button class="btn btn-primary" onclick="processAI()">EXECUTE</button>
          </div>
          <div class="ai-response" id="aiResponse"></div>
        </div>

        <div class="mini-cal" id="miniCalSection">
          <div class="mini-cal-header">
            <button class="mini-cal-nav" onclick="miniCalNav(-1)">‹</button>
            <div class="mini-cal-title" id="miniCalTitle">APR 2026</div>
            <button class="mini-cal-nav" onclick="miniCalNav(1)">›</button>
          </div>
          <div class="mini-cal-grid" id="miniCalGrid"></div>
        </div>

        <div class="sidebar-section">
          <button class="overwhelmed-btn" onclick="overwhelmedMode()">⚡ I'M OVERWHELMED</button>
        </div>

        <div class="sidebar-section" style="flex:1;overflow-y:auto;">
          <div class="section-label">// active goals</div>
          <div id="sidebarGoals"></div>
        </div>

        <div class="sidebar-section">
          <button class="btn btn-primary" style="width:100%;" onclick="startFocus()">⬛ ENTER FOCUS MODE</button>
        </div>
      </div>

      <div class="content">
        <div class="panel active" id="panel-calendar">
          <div class="cal-header">
            <div class="cal-title" id="calTitle">APRIL 2026</div>
            <div class="cal-controls">
              <button class="btn btn-ghost" onclick="calNav(-1)">‹ PREV</button>
              <button class="btn btn-ghost" onclick="calNav(0)">TODAY</button>
              <button class="btn btn-ghost" onclick="calNav(1)">NEXT ›</button>
              <div class="view-btns">
                <button class="view-btn active" onclick="setView('week',this)">WEEK</button>
                <button class="view-btn" onclick="setView('day',this)">DAY</button>
              </div>
            </div>
          </div>
          <div id="calBody"></div>
        </div>

        <div class="panel" id="panel-goals">
          <div class="goals-panel">
            <div style="display:flex;justify-content:space-between;align-items:center">
              <div>
                <div style="font-family:'Rajdhani',sans-serif;font-size:22px;letter-spacing:3px;color:var(--text)">LONG-TERM GOALS</div>
                <div style="font-size:12px;color:var(--text3);margin-top:2px">Auto-scheduled into your daily blocks</div>
              </div>
              <button class="btn btn-primary" onclick="addGoal()">+ ADD GOAL</button>
            </div>
            <div class="goals-grid" id="goalsGrid"></div>
          </div>
        </div>

        <div class="panel" id="panel-review">
          <div class="review-panel">
            <div style="font-family:'Rajdhani',sans-serif;font-size:22px;letter-spacing:3px;color:var(--text);margin-bottom:4px">NIGHT REVIEW</div>
            <div style="font-size:12px;color:var(--text3);margin-bottom:24px">End of day debrief — <span style="font-family:'Share Tech Mono',monospace;color:var(--accent)" id="reviewDate"></span></div>
            <div class="review-q">
              <div class="review-q-label">// 01 · What worked today?</div>
              <textarea class="review-input" id="r1" placeholder="What actually happened..."></textarea>
            </div>
            <div class="review-q">
              <div class="review-q-label">// 02 · What didn't work?</div>
              <textarea class="review-input" id="r2" placeholder="Honest assessment..."></textarea>
            </div>
            <div class="review-q">
              <div class="review-q-label">// 03 · Tomorrow's #1 priority</div>
              <textarea class="review-input" id="r3" placeholder="One thing that matters most..." rows="2"></textarea>
            </div>
            <button class="btn btn-primary" style="width:100%;padding:12px;" onclick="saveReview()">LOG REVIEW</button>
            <div style="margin-top:24px;border-top:1px solid var(--border);padding-top:20px;">
              <div class="section-label">// skip pattern analysis</div>
              <div id="skipAnalysis" style="font-size:13px;color:var(--text2);line-height:1.8"></div>
            </div>
          </div>
        </div>

        <div class="panel" id="panel-habits">
          <div style="padding:24px">
            <div style="font-family:'Rajdhani',sans-serif;font-size:22px;letter-spacing:3px;color:var(--text);margin-bottom:4px">HABIT LOG</div>
            <div style="font-size:12px;color:var(--text3);margin-bottom:24px">Track. Analyze. Improve.</div>
            <div id="habitLog"></div>
            <div style="margin-top:24px">
              <div class="section-label">// 7-day momentum</div>
              <div id="momentumGrid"></div>
            </div>
            <div style="margin-top:20px;padding:16px;background:rgba(0,212,255,0.04);border:1px solid var(--border);border-radius:2px">
              <div style="font-size:11px;letter-spacing:2px;color:var(--text3);margin-bottom:8px;font-family:'Rajdhani',sans-serif">// GYM COMEBACK PLANNER</div>
              <div style="font-size:12px;color:var(--text2)">Dormant — activate when ready</div>
              <button class="btn btn-ghost" style="margin-top:10px;font-size:11px;" onclick="activateGym()">ACTIVATE ROUTINE</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    const FACTS = [
      "The compound effect: 1% better every day = 37x better in a year. Not motivation — math.",
      "Warren Buffett reads 500 pages daily. Information compounds the same way money does.",
      "The Stoics built empires on one rule: control what you can, release what you can't.",
      "Marcus Aurelius ran the most powerful empire on earth and journaled every night. Process matters.",
      "Chess grandmasters see 40,000+ patterns. Domain mastery is just pattern volume.",
      "Naval Ravikant: Specific knowledge is what you'd do even if you weren't paid for it.",
      "The Nifty 50 has never given negative returns over any 10-year period. Time is the edge.",
      "Charlie Munger read 600-900 pages a week. He called it 'going to bed smarter than you woke up'.",
      "India will be the 3rd largest economy by 2027. You're early. That's the advantage.",
      "Seneca: 'It is not that we have a short time to live, but that we waste much of it.'"
    ];

    let state = {
      events: [
        {id:1, name:"Maths — Chapter 4", type:"study", day:0, hour:10, duration:2, goalId:1},
        {id:2, name:"Syntax Writing", type:"personal", day:1, hour:20, duration:1.5},
        {id:3, name:"Maths — Chapter 5", type:"study", day:2, hour:11, duration:2, goalId:1},
        {id:4, name:"Solo Ride / Reset", type:"personal", day:3, hour:17, duration:1},
        {id:5, name:"Finance Reading", type:"goal", day:4, hour:9, duration:1, goalId:2},
        {id:6, name:"Maths — Chapter 6", type:"study", day:5, hour:10, duration:2, goalId:1},
      ],
      goals: [
        {id:1, name:"Master Mathematics", deadline:"6 months", progress:6, daily:"45 mins/day", weekly:"5 days/week", skips:[{date:"Apr 1",reason:"low energy"},{date:"Mar 28",reason:"anxious"}]},
        {id:2, name:"Finance & Markets", deadline:"3 months", progress:22, daily:"30 mins/day", weekly:"6 days/week", skips:[]},
      ],
      habits: [
        {name:"Maths Study", streak:5, week:[1,1,0,1,1,0,1]},
        {name:"Syntax Writing", streak:3, week:[0,1,1,0,1,0,1]},
        {name:"Finance Reading", streak:8, week:[1,1,1,1,1,0,1]},
        {name:"Solo Ride", streak:2, week:[0,0,1,0,0,1,0]},
      ],
      currentWeekOffset: 0,
      currentView: 'week',
      miniCalOffset: 0,
      focusActive: false,
      focusInterval: null,
      focusSeconds: 1500,
      skipLog: [],
      reviews: [],
      voiceActive: false,
    };

    function initClock() {
      function tick() {
        const now = new Date();
        const h = String(now.getHours()).padStart(2,'0');
        const m = String(now.getMinutes()).padStart(2,'0');
        document.getElementById('topClock').textContent = `${h}:${m}`;
        document.getElementById('briefClock').textContent = `${h}:${m}`;
        
        const days = ['SUN','MON','TUE','WED','THU','FRI','SAT'];
        const months = ['JAN','FEB','MAR','APR','MAY','JUN','JUL','AUG','SEP','OCT','NOV','DEC'];
        document.getElementById('topDate').textContent = `${days[now.getDay()]} ${now.getDate()} ${months[now.getMonth()]} ${now.getFullYear()}`;
        document.getElementById('reviewDate').textContent = `${now.getDate()} ${months[now.getMonth()]} ${now.getFullYear()}`;
      }
      tick();
      setInterval(tick, 1000);
    }

    function initBrief() {
      const now = new Date();
      const h = now.getHours();
      let greeting = '';
      
      // Fixed malformed ternary string from original code
      if (h < 12) greeting = 'Good Morning, Sir';
      else if (h < 17) greeting = 'Good Afternoon, Sir';
      else greeting = 'Good Evening, Sir';
      
      document.getElementById('briefGreeting').textContent = greeting;
      document.getElementById('briefFact').textContent = FACTS[Math.floor(Math.random() * FACTS.length)];
    }

    function closeBrief() {
      document.getElementById('briefOverlay').style.display = 'none';
      
      // Trigger voice AFTER user interaction to bypass browser autoplay blocks
      const synth = window.speechSynthesis;
      if (synth) {
        const greeting = document.getElementById('briefGreeting').textContent;
        const fact = document.getElementById('briefFact').textContent;
        const u = new SpeechSynthesisUtterance(`${greeting}. System online. You have ${state.events.length} tasks scheduled today. Here is your power fact: ${fact}`);
        u.rate = 0.9;
        u.pitch = 0.8;
        synth.speak(u);
      }
    }

    function getWeekDates() {
      const now = new Date();
      const day = now.getDay();
      const monday = new Date(now);
      monday.setDate(now.getDate() - day + 1 + (state.currentWeekOffset * 7));
      const dates = [];
      for(let i=0; i<7; i++){
        const d = new Date(monday);
        d.setDate(monday.getDate() + i);
        dates.push(d);
      }
      return dates;
    }

    function renderCalendar() {
      const dates = getWeekDates();
      const days = ['MON','TUE','WED','THU','FRI','SAT','SUN'];
      const today = new Date();
      const months = ['January','February','March','April','May','June','July','August','September','October','November','December'];
      
      document.getElementById('calTitle').textContent = `${months[dates[0].getMonth()].toUpperCase()} ${dates[0].getFullYear()}`;
      
      let html = `<div class="week-grid">`;
      html += `<div class="time-col"><div style="height:61px;border-bottom:1px solid var(--border)"></div>`;
      for(let h=6; h<23; h++){
        html += `<div class="time-slot">${String(h).padStart(2,'0')}:00</div>`;
      }
      html += `</div>`;
      
      for(let d=0; d<7; d++){
        const date = dates[d];
        const isToday = date.toDateString() === today.toDateString();
        html += `<div class="day-col">`;
        html += `<div class="day-col-header">
          <div class="day-col-name">${days[d]}</div>
          <div class="day-col-date ${isToday?'today':''}">${date.getDate()}</div>
        </div>`;
        for(let h=6; h<23; h++){
          html += `<div class="day-slot" data-day="${d}" data-hour="${h}" onclick="addEventClick(${d},${h})">`;
          const evs = state.events.filter(e => e.day === d && e.hour === h);
          evs.forEach(ev => {
            const pct = ev.duration * 60; // 60px per hour
            html += `<div class="event ${ev.type}" style="top:0;height:${pct}px;" onclick="event.stopPropagation();skipEvent(${ev.id})">
              <div class="event-name">${ev.name}</div>
              <div class="event-time">${String(ev.hour).padStart(2,'0')}:00</div>
            </div>`;
          });
          html += `</div>`;
        }
        html += `</div>`;
      }
      html += `</div>`;
      document.getElementById('calBody').innerHTML = html;
    }

    function renderMiniCal() {
      const now = new Date();
      const y = now.getFullYear();
      const m = now.getMonth() + state.miniCalOffset;
      const d = new Date(y, m, 1);
      const months = ['JAN','FEB','MAR','APR','MAY','JUN','JUL','AUG','SEP','OCT','NOV','DEC'];
      document.getElementById('miniCalTitle').textContent = `${months[d.getMonth()]} ${d.getFullYear()}`;
      
      const firstDay = (d.getDay() + 6) % 7;
      const daysInMonth = new Date(d.getFullYear(), d.getMonth() + 1, 0).getDate();
      const daysInPrev = new Date(d.getFullYear(), d.getMonth(), 0).getDate();
      const days = ['M','T','W','T','F','S','S'];
      
      let html = days.map(x => `<div class="mini-day-label">${x}</div>`).join('');
      for(let i=0; i<firstDay; i++){
        html += `<div class="mini-day other-month">${daysInPrev - firstDay + 1 + i}</div>`;
      }
      for(let i=1; i<=daysInMonth; i++){
        const isToday = i === now.getDate() && d.getMonth() === now.getMonth() && d.getFullYear() === now.getFullYear();
        html += `<div class="mini-day ${isToday ? 'today' : ''}">${i}</div>`;
      }
      document.getElementById('miniCalGrid').innerHTML = html;
    }

    function renderSidebarGoals() {
      let html = '';
      state.goals.forEach(g => {
        html += `<div class="goal-item">
          <div class="goal-bar-wrap">
            <div class="goal-name">${g.name}</div>
            <div class="goal-meta">${g.deadline} · ${g.daily}</div>
            <div class="goal-bar"><div class="goal-bar-fill" style="width:${Math.min(g.progress, 100)}%"></div></div>
          </div>
          <div style="font-family:'Share Tech Mono',monospace;font-size:11px;color:var(--accent);min-width:32px;text-align:right">${g.progress}%</div>
        </div>`;
      });
      document.getElementById('sidebarGoals').innerHTML = html;
    }

    function renderGoalsPanel() {
      let html = '';
      state.goals.forEach(g => {
        const skipInfo = g.skips.length > 0 ? `<div class="pattern-alert">⚠ ${g.skips.length} skips detected — most common: "${g.skips[0]?.reason||'unknown'}"</div>` : '';
        html += `<div class="goal-card">
          <div class="goal-card-name">${g.name}</div>
          <div class="goal-card-deadline">// target: ${g.deadline}</div>
          <div class="goal-progress">
            <div style="display:flex;justify-content:space-between;font-size:11px;color:var(--text3)">
              <span>PROGRESS</span><span style="color:var(--accent);font-family:'Share Tech Mono',monospace">${g.progress}%</span>
            </div>
            <div class="goal-progress-bar"><div class="goal-progress-fill" style="width:${Math.min(g.progress, 100)}%"></div></div>
          </div>
          <div class="goal-schedule">
            Daily block: <span>${g.daily}</span><br>
            Frequency: <span>${g.weekly}</span>
          </div>
          ${skipInfo}
        </div>`;
      });
      document.getElementById('goalsGrid').innerHTML = html;
    }

    function renderHabits() {
      const dayLabels = ['M','T','W','T','F','S','S'];
      let html = '';
      state.habits.forEach((h, i) => {
        const dots = h.week.map((d, j) => `<div class="habit-dot ${d===1?'done':d===-1?'skip':''}" onclick="toggleHabit(${i},${j})">${dayLabels[j]}</div>`).join('');
        html += `<div class="habit-row">
          <div>
            <div class="habit-name">${h.name}</div>
            <div class="streak">▲ ${h.streak} day streak</div>
          </div>
          <div class="habit-dots">${dots}</div>
        </div>`;
      });
      document.getElementById('habitLog').innerHTML = html;

      let mhtml = '';
      state.habits.forEach(h => {
        mhtml += `<div style="margin-bottom:8px">
          <div style="font-size:11px;color:var(--text3);margin-bottom:3px">${h.name}</div>
          <div class="momentum-row">
            ${h.week.map(d => `<div class="m-block ${d===1?'done':d===-1?'skip':''}"></div>`).join('')}
          </div>
        </div>`;
      });
      document.getElementById('momentumGrid').innerHTML = mhtml;
    }

    function renderSkipAnalysis() {
      const all = state.skipLog;
      if (all.length === 0) {
        document.getElementById('skipAnalysis').textContent = 'No skips logged yet. Keep the streak alive.';
        return;
      }
      const counts = {};
      all.forEach(s => counts[s.reason] = (counts[s.reason] || 0) + 1);
      const top = Object.entries(counts).sort((a,b) => b[1] - a[1]);
      
      let html = `<div style="margin-bottom:8px">Total skips logged: <span style="color:var(--accent);font-family:'Share Tech Mono',monospace">${all.length}</span></div>`;
      html += `<div style="margin-bottom:8px">Most common reason: <span style="color:var(--amber)">${top[0]?.[0] || '—'}</span></div>`;
      
      if (top[0]?.[0] === 'anxious' || top[0]?.[0] === 'overwhelmed') {
        html += `<div style="color:var(--red);font-size:12px;margin-top:8px">Pattern detected: Anxiety is your main blocker. Consider breaking tasks into 15-min micro-sessions.</div>`;
      } else if (top[0]?.[0] === 'low energy') {
        html += `<div style="color:var(--amber);font-size:12px;margin-top:8px">Pattern detected: Energy management. Try shifting heavy study earlier in the day.</div>`;
      }
      document.getElementById('skipAnalysis').innerHTML = html;
    }

    function switchPanel(name, btn) {
      document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
      document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
      document.getElementById('panel-' + name).classList.add('active');
      btn.classList.add('active');
      
      if(name === 'goals') renderGoalsPanel();
      if(name === 'habits') renderHabits();
      if(name === 'review') renderSkipAnalysis();
    }

    function calNav(dir) {
      if(dir === 0) state.currentWeekOffset = 0;
      else state.currentWeekOffset += dir;
      renderCalendar();
    }

    function miniCalNav(dir) {
      state.miniCalOffset += dir;
      renderMiniCal();
    }
    
    function setView(v, btn) {
      document.querySelectorAll('.view-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      state.currentView = v;
      renderCalendar();
    }

    function processAI() {
      const input = document.getElementById('aiInput').value.trim();
      if(!input) return;
      
      const resp = document.getElementById('aiResponse');
      resp.innerHTML = `<div class="ai-typing"><div class="ai-dot"></div><div class="ai-dot"></div><div class="ai-dot"></div></div>`;
      resp.classList.add('visible');
      
      setTimeout(() => {
        const lower = input.toLowerCase();
        let msg = '';
        let added = false;
        
        const timeMatch = input.match(/(\d{1,2})(?::(\d{2}))?\s*(am|pm)?/i);
        const hour = timeMatch ? parseInt(timeMatch[1]) + (timeMatch[3]?.toLowerCase() === 'pm' && parseInt(timeMatch[1]) < 12 ? 12 : 0) : null;
        
        if (lower.includes('maths') || lower.includes('math') || lower.includes('study')) {
          if (hour) {
            state.events.push({id:Date.now(), name:'Maths Study', type:'study', day:new Date().getDay()-1, hour:hour, duration:1.5, goalId:1});
            msg = `Maths session blocked at ${hour}:00. Goal tracker updated. Streak continues — don't skip this one.`;
            added = true; renderCalendar();
          } else { msg = 'Got it. What time? Give me a slot and I\'ll lock it in.'; }
        } else if (lower.includes('syntax') || lower.includes('writing')) {
          if (hour) {
            state.events.push({id:Date.now(), name:'Syntax — Writing', type:'personal', day:new Date().getDay()-1, hour:hour, duration:1.5});
            msg = `Syntax writing blocked at ${hour}:00. Last arc: Arc 3. Pick it up from there.`;
            added = true; renderCalendar();
          } else { msg = 'When do you want to write? Tell me the time.'; }
        } else if (lower.includes('ride') || lower.includes('bike')) {
          if (hour) {
            state.events.push({id:Date.now(), name:'Solo Ride — Reset', type:'personal', day:new Date().getDay()-1, hour:hour, duration:1});
            msg = `Ride logged at ${hour}:00. Recharge block protected. M 1000 RR time.`;
            added = true; renderCalendar();
          } else { msg = 'What time is the ride? I\'ll block it and protect it.'; }
        } else if (lower.includes('overwhelm') || lower.includes('too much')) {
          overwhelmedMode(); msg = 'Overwhelmed mode activated. Stripped to essentials.';
        } else if (lower.includes('goal') && (lower.includes('add') || lower.includes('new'))) {
          msg = 'Tell me the goal and the deadline. Format: "learn X in Y months" and I\'ll build the schedule.';
        } else if (hour && !added) {
          state.events.push({id:Date.now(), name:input.replace(/at \d+.*?(am|pm)?/i, '').trim() || 'Event', type:'study', day:new Date().getDay()-1, hour:hour, duration:1});
          msg = `Locked in at ${hour}:00. Calendar updated.`;
          renderCalendar();
        } else {
          msg = 'Tell me what to schedule, and when. I\'ll take care of the rest.';
        }
        
        resp.innerHTML = `<span style="color:var(--accent);font-family:'Share Tech Mono',monospace;font-size:10px">// JARVIS</span><br>${msg}`;
        
        if (window.speechSynthesis) {
          const u = new SpeechSynthesisUtterance(msg);
          u.rate = 0.95; u.pitch = 0.85; window.speechSynthesis.speak(u);
        }
      }, 1200);
      
      document.getElementById('aiInput').value = '';
    }

    function skipEvent(id) {
      const ev = state.events.find(e => e.id === id);
      if(!ev) return;
      document.getElementById('skippedTask').textContent = ev.name;
      document.getElementById('skipModal').classList.add('active');
    }

    function logSkip(reason) {
      state.skipLog.push({reason, date:new Date().toDateString()});
      const goal = state.goals.find(g => g.id === state.events[0]?.goalId);
      if(goal) goal.skips.push({date:new Date().toDateString(), reason});
      
      closeSkipModal();
      const resp = document.getElementById('aiResponse');
      resp.innerHTML = `<span style="color:var(--accent);font-family:'Share Tech Mono',monospace;font-size:10px">// logged</span><br>Reason noted: "${reason}". I'll factor this into your schedule. Pattern analysis updated.`;
      resp.classList.add('visible');
    }

    function closeSkipModal() {
      document.getElementById('skipModal').classList.remove('active');
    }

    function startFocus() {
      const tasks = state.events.filter(e => e.type === 'study');
      const task = tasks[0];
      if(task) document.getElementById('focusTaskName').textContent = task.name;
      
      state.focusSeconds = 1500;
      document.getElementById('focusTimer').textContent = '25:00';
      document.getElementById('focusOverlay').classList.add('active');
      state.focusActive = true;
      
      state.focusInterval = setInterval(() => {
        if (state.focusSeconds <= 0) { clearInterval(state.focusInterval); endFocus(); return; }
        state.focusSeconds--;
        const m = Math.floor(state.focusSeconds / 60);
        const s = state.focusSeconds % 60;
        document.getElementById('focusTimer').textContent = `${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
      }, 1000);
      
      if(window.speechSynthesis) {
        const u = new SpeechSynthesisUtterance('Focus mode activated. Distraction shield is up. Do the work.');
        u.rate = 0.9; u.pitch = 0.8; window.speechSynthesis.speak(u);
      }
    }

    function pauseFocus() {
      if (state.focusActive) {
        clearInterval(state.focusInterval);
        state.focusActive = false;
        document.getElementById('focusPauseBtn').textContent = 'RESUME';
      } else {
        state.focusActive = true;
        document.getElementById('focusPauseBtn').textContent = 'PAUSE';
        state.focusInterval = setInterval(() => {
          if (state.focusSeconds <= 0) { clearInterval(state.focusInterval); endFocus(); return; }
          state.focusSeconds--;
          const m = Math.floor(state.focusSeconds / 60);
          const s = state.focusSeconds % 60;
          document.getElementById('focusTimer').textContent = `${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
        }, 1000);
      }
    }

    function endFocus() {
      clearInterval(state.focusInterval);
      document.getElementById('focusOverlay').classList.remove('active');
      document.getElementById('focusPauseBtn').textContent = 'PAUSE';
      if(window.speechSynthesis) {
        const u = new SpeechSynthesisUtterance('Focus session complete. Good work.');
        u.rate = 0.9; window.speechSynthesis.speak(u);
      }
    }

    // Fixed broken Spotify link placeholder
    function openSpotify() {
      window.open('https://open.spotify.com', '_blank');
    }

    function overwhelmedMode() {
      const priority = state.events.filter(e => e.type === 'study').slice(0,2);
      state.events = priority;
      renderCalendar();
      
      const resp = document.getElementById('aiResponse');
      resp.innerHTML = `<span style="color:var(--red);font-family:'Share Tech Mono',monospace;font-size:10px">// overwhelmed mode</span><br>Day stripped to ${priority.length} essentials. Everything else cleared. Breathe. One thing at a time.`;
      resp.classList.add('visible');
      
      if(window.speechSynthesis) {
        const u = new SpeechSynthesisUtterance('Day simplified. Focus on one thing at a time.');
        u.rate = 0.9; window.speechSynthesis.speak(u);
      }
    }

    function toggleHabit(i, j) {
      const h = state.habits[i];
      h.week[j] = h.week[j] === 1 ? -1 : h.week[j] === -1 ? 0 : 1;
      renderHabits();
    }

    function addGoal() {
      const name = prompt('Goal name:');
      const deadline = prompt('Deadline (e.g. 6 months):');
      if (name && deadline) {
        state.goals.push({id:Date.now(), name, deadline, progress:0, daily:'30 mins/day', weekly:'5 days/week', skips:[]});
        renderGoalsPanel(); renderSidebarGoals();
      }
    }

    function addEventClick(day, hour) {
      const name = prompt(`Add event at ${String(hour).padStart(2,'0')}:00:`);
      if (name) {
        state.events.push({id:Date.now(), name, type:'study', day, hour, duration:1});
        renderCalendar();
      }
    }

    function saveReview() {
      const r1 = document.getElementById('r1').value;
      const r2 = document.getElementById('r2').value;
      const r3 = document.getElementById('r3').value;
      state.reviews.push({date:new Date().toDateString(), r1, r2, r3});
      
      document.getElementById('r1').value = '';
      document.getElementById('r2').value = '';
      document.getElementById('r3').value = '';
      renderSkipAnalysis();
      alert('Night review logged. See you tomorrow, Sir.');
    }

    function activateGym() {
      state.habits.push({name:'Gym', streak:0, week:[0,0,0,0,0,0,0]});
      renderHabits();
      alert('Gym routine activated. Starting with 3x/week. Build the habit first.');
    }

    function toggleVoice() {
      if (!('webkitSpeechRecognition' in window || 'SpeechRecognition' in window)) {
        alert('Voice not supported in this browser. Try Chrome.');
        return;
      }
      const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
      const rec = new SR();
      rec.lang = 'en-IN'; 
      rec.interimResults = false;
      
      const btn = document.getElementById('voiceBtn');
      btn.classList.add('listening');
      rec.start();
      
      rec.onresult = e => {
        document.getElementById('aiInput').value = e.results[0][0].transcript;
        btn.classList.remove('listening');
        processAI();
      };
      rec.onerror = () => btn.classList.remove('listening');
      rec.onend = () => btn.classList.remove('listening');
    }

    document.getElementById('aiInput').addEventListener('keydown', e => {
      if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); processAI(); }
    });

    initClock();
    initBrief();
    renderCalendar();
    renderMiniCal();
    renderSidebarGoals();
  </script>
</body>
</html>
