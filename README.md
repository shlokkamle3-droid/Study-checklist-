<!DOCTYPE html>
<!--
STUDY CHECKLIST WEBSITE – READY FOR GITHUB PAGES 🚀
--------------------------------------------------
HOW TO RUN ON GITHUB:
1. Create a GitHub repository
2. Upload this file and name it: index.html
3. Go to Settings → Pages → Enable GitHub Pages
4. Open the generated link (HTTPS required for notifications)

Made with love ❤️ by Shlok Kamle
-->
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Study Checklist – Shlok Kamle</title>
  <style>
    :root{
      --bg:#f4f6fb;
      --card:#ffffff;
      --text:#111827;
      --muted:#6b7280;
      --accent:#4f46e5;
      --shadow:0 10px 30px rgba(0,0,0,0.08);
    }
    body.dark{
      --bg:#0f172a;
      --card:#111827;
      --text:#f9fafb;
      --muted:#9ca3af;
      --accent:#818cf8;
      --shadow:0 10px 30px rgba(0,0,0,0.4);
    }
    *{box-sizing:border-box;font-family:Inter,system-ui,Arial;}
    body{margin:0;background:var(--bg);color:var(--text);}
    header{text-align:center;padding:24px;position:relative;}
    h1{font-size:3rem;margin:0;font-weight:800;}
    .subtitle{color:var(--muted);margin-top:6px;}
    .container{max-width:1000px;margin:auto;padding:20px;}
    .card{background:var(--card);border-radius:16px;padding:20px;margin-bottom:20px;box-shadow:var(--shadow);}    
    .row{display:flex;gap:16px;flex-wrap:wrap;}
    label{font-weight:600;}
    input,textarea,button{width:100%;padding:10px;border-radius:10px;border:1px solid #e5e7eb;font-size:1rem;}
    body.dark input,body.dark textarea{background:#020617;color:white;border-color:#1e293b;}
    button{background:var(--accent);color:white;border:none;font-weight:700;cursor:pointer;}
    .task{display:flex;align-items:center;justify-content:space-between;padding:10px;border-bottom:1px solid #e5e7eb;}
    body.dark .task{border-color:#1e293b;}
    .progress{height:10px;background:#e5e7eb;border-radius:999px;overflow:hidden;margin-top:10px;}
    .progress div{height:100%;background:var(--accent);width:0%;}
    footer{text-align:center;padding:20px;color:var(--muted);}    
    .toggle{position:absolute;top:-12px;right:12px;}
  </style>
</head>
<body>

  <header>
    <button class="toggle" onclick="toggleMode()">🌙 / ☀️</button>
    <h1 id="countdown">Board Exam Countdown</h1>
    <div class="subtitle">Edit exam date below</div>
  </header>

  <div class="container">
    <div class="card">
      <label>Board Exam Date</label>
      <input type="date" id="examDate" />
    </div>

    <div class="card">
      <h2>📋 Study Checklist</h2>
      <div class="row">
        <input id="taskName" placeholder="Task name" />
        <input type="date" id="taskDate" />
        <button onclick="addTask()">Add Task</button>
      </div>
      <div id="taskList"></div>
    </div>

    <div class="card">
      <h2>⏱ Daily Study Time (hours)</h2>
      <input type="number" id="studyTime" placeholder="e.g. 3" />
      <button onclick="saveStudyTime()">Save</button>
      <div class="progress"><div id="timeProgress"></div></div>
    </div>

    <div class="card">
      <h2>🤖 AI Generated Study Plan (Editable)</h2>
      <textarea rows="6" id="aiPlan">Revise weak chapters first, practice numericals daily, revise notes every night.</textarea>
    </div>

    <div class="card">
      <h2>📆 Weekly Timetable (Editable)</h2>
      <table style="width:100%;border-collapse:collapse;text-align:center;">
        <tr><th>Day</th><th>Plan</th></tr>
        <tr><td>Monday</td><td contenteditable>Maths + Science</td></tr>
        <tr><td>Tuesday</td><td contenteditable>Social Science</td></tr>
        <tr><td>Wednesday</td><td contenteditable>English</td></tr>
        <tr><td>Thursday</td><td contenteditable>Sanskrit</td></tr>
        <tr><td>Friday</td><td contenteditable>Revision</td></tr>
        <tr><td>Saturday</td><td contenteditable>Mock Test</td></tr>
        <tr><td>Sunday</td><td contenteditable>Light Study + Rest</td></tr>
      </table>
    </div>

    <div class="card">
      <h2>💬 Daily Quote</h2>
      <p id="quote"></p>
    </div>
  </div>

  <footer>
    Made with love ❤️ <br/>By Shlok Kamle
  </footer>

  <script>
    const quotes=["Consistency beats motivation.","Small steps daily lead to big success.","Focus today, relax tomorrow.","Discipline creates confidence."];
    document.getElementById('quote').innerText=quotes[Math.floor(Math.random()*quotes.length)];

    function toggleMode(){document.body.classList.toggle('dark');}

    const examInput=document.getElementById('examDate');
    examInput.onchange=updateCountdown;

    function updateCountdown(){
      if(!examInput.value) return;
      const d=new Date(examInput.value);
      const now=new Date();
      const diff=Math.ceil((d-now)/(1000*60*60*24));
      document.getElementById('countdown').innerText=`${diff} Days Left for Board Exam`;
    }

    function addTask(){
      const name=taskName.value;
      const date=taskDate.value;
      if(!name||!date) return;
      const days=Math.ceil((new Date(date)-new Date())/(1000*60*60*24));
      const div=document.createElement('div');
      div.className='task';
      div.innerHTML=`<span>✔️ ${name} (${days} days left)</span><input type='checkbox'>`;
      taskList.appendChild(div);
      taskName.value='';taskDate.value='';
    }

    function saveStudyTime(){
      const val=document.getElementById('studyTime').value;
      const percent=Math.min((val/6)*100,100);
      document.getElementById('timeProgress').style.width=percent+'%';
    }

    if('Notification' in window){
      Notification.requestPermission();
      setTimeout(()=>{
        if(Notification.permission==='granted'){
          new Notification('📢 Study Reminder',{body:'Check your pending tasks & revise today!'});
        }
      },5000);
    }
  </script>
</body>
</html>
