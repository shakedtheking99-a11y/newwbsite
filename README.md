<!doctype html>
<html lang="he">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>ברוך הבא — הקלד את שמך</title>
  <style>
    :root{--bg:#0f172a;--card:#111827;--accent:#06b6d4;--text:#e6eef6}
    html,body{height:100%;margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,'Helvetica Neue',Arial}
    body{display:flex;align-items:center;justify-content:center;background:linear-gradient(180deg,var(--bg),#081028);color:var(--text)}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:28px;border-radius:16px;box-shadow:0 10px 30px rgba(2,6,23,0.7);width:100%;max-width:520px}
    h1{margin:0 0 10px;font-size:20px}
    label{display:block;margin-bottom:8px;font-weight:600}
    .row{display:flex;gap:8px}
    input[type=text]{flex:1;padding:10px 12px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:var(--text);outline:none;font-size:16px}
    button{padding:10px 14px;border-radius:10px;border:none;background:var(--accent);color:#042028;font-weight:700;cursor:pointer}
    .msg{margin-top:18px;padding:14px;border-radius:10px;background:rgba(255,255,255,0.02);min-height:48px;display:flex;align-items:center}
    .hint{margin-top:8px;color:rgba(230,238,246,0.6);font-size:13px}
    .error{color:#ffb4b4}
  </style>
</head>
<body>
  <main class="card" role="main">
    <h1>ברוך הבא — תנסה להקליד את שמך</h1>
    <label for="name">הקלד את שמך:</label>
    <div class="row">
      <input id="name" type="text" autocomplete="name" placeholder="לדוגמה: שרית" />
      <button id="go">שלח</button>
    </div>

    <div id="result" class="msg" aria-live="polite"></div>
    <div class="hint">לחץ Enter בשביל לשלוח. השם מטופל בבטחה (לא רץ כ־HTML) כדי למנוע קוד זדוני.</div>
  </main>

  <script>
    const input = document.getElementById('name');
    const btn = document.getElementById('go');
    const result = document.getElementById('result');

    function showWelcome(){
      const raw = input.value.trim();
      if(!raw){
        result.textContent = '';
        result.classList.add('error');
        result.textContent = 'בבקשה הזן שם כדי לראות את הברכה.';
        return;
      }
      // השתמש ב־textContent כדי למנוע XSS — לא מוסיפים HTML ישיר
      result.classList.remove('error');
      result.textContent = `ברוך הבא, ${raw}!`;
    }

    btn.addEventListener('click', showWelcome);
    input.addEventListener('keydown', (e)=>{
      if(e.key === 'Enter') showWelcome();
    });

    // אופציונלי: שמירת השם בזיכרון מקומי
    input.addEventListener('change', ()=>{
      try{ localStorage.setItem('guestName', input.value.trim()); }catch(e){}
    });
    // טעינת שם אם יש
    window.addEventListener('load', ()=>{
      try{ const n = localStorage.getItem('guestName'); if(n) input.value = n; }catch(e){}
    });
  </script>
</body>
</html>
