<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>MedGuide</title>

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">

<style>
:root {
  --blue: #6baed6;
  --mint: #7edad2;
  --lavender: #c6b7e2;
  --glass: rgba(255,255,255,0.6);
}

* { box-sizing: border-box; font-family: 'Inter', sans-serif; }
body {
  margin: 0; min-height: 100vh;
  background: linear-gradient(135deg, var(--blue), var(--mint), var(--lavender));
  display: flex; justify-content: center; padding: 20px; color: #334155;
}

.app {
  width: 100%; max-width: 450px;
  background: var(--glass); backdrop-filter: blur(14px);
  border-radius: 24px; box-shadow: 0 20px 40px rgba(0,0,0,0.15);
  padding: 20px; display: flex; flex-direction: column;
}

header { text-align: center; margin-bottom: 12px; }
header h1 { margin: 0; font-weight: 600; }
header p { font-size: 14px; opacity: 0.8; }

nav { display: flex; justify-content: space-around; margin-bottom: 10px; }
nav button {
  background: var(--glass); border: none; padding: 10px 14px;
  border-radius: 12px; cursor: pointer; font-weight: 500;
  transition: 0.2s;
}
nav button.active { background: rgba(255,255,255,0.9); }

.tab { display: none; flex-direction: column; flex: 1; }
.tab.active { display: flex; }

.chat { flex: 1; overflow-y: auto; padding: 10px 0; }
.message {
  max-width: 80%; padding: 12px 14px; border-radius: 16px;
  margin-bottom: 10px; animation: fadeIn 0.3s ease;
}
.ai { background: rgba(255,255,255,0.9); align-self: flex-start; }
.user { background: linear-gradient(135deg, var(--blue), var(--mint)); color: white; align-self: flex-end; }

.confidence { font-size: 12px; margin: 8px 0; opacity: 0.8; }
input { width: 100%; border: none; padding: 14px; border-radius: 14px; outline: none; margin-top: 10px; }

.profile-card, .doctor-card {
  background: rgba(255,255,255,0.9); padding: 14px; border-radius: 16px;
  margin-bottom: 10px; box-shadow: 0 6px 20px rgba(0,0,0,0.08);
}

.disclaimer { font-size: 11px; text-align: center; margin-top: 10px; opacity: 0.7; }

@keyframes fadeIn { from {opacity:0; transform:translateY(5px);} to{opacity:1; transform:translateY(0);} }

</style>
</head>
<body>

<div class="app">
  <header>
    <h1>MedGuide</h1>
    <p>I feel safe, understood, and not judged.</p>
  </header>

  <nav>
    <button class="active" onclick="showTab('chatTab', this)">Чат</button>
    <button onclick="showTab('profileTab', this)">Мой профиль</button>
    <button onclick="showTab('doctorTab', this)">Мой врач</button>
  </nav>

  <!-- ЧАТ -->
  <div class="tab active" id="chatTab">
    <div class="chat" id="chat">
      <div class="message ai">
        I’m here with you 💙  
        Tell me what symptoms you’re experiencing.
      </div>
    </div>
    <div class="confidence" id="confidence">Confidence level: —</div>
    <input id="input" placeholder="Describe how you feel..." onkeydown="handleKey(event)" />
  </div>

  <!-- МОЙ ПРОФИЛЬ -->
  <div class="tab" id="profileTab">
    <div class="profile-card">
      <h3>Имя: Мирана К.</h3>
      <p>Возраст: 17 лет</p>
    </div>
    <div class="profile-card">
      <h4>История симптомов:</h4>
      <ul>
        <li>Головная боль – 10.01.2026</li>
        <li>Лёгкая усталость – 08.01.2026</li>
      </ul>
    </div>
  </div>

  <!-- МОЙ ВРАЧ -->
  <div class="tab" id="doctorTab">
    <div class="doctor-card">
      <h3>Имя врача: Dr. Smith</h3>
      <p>Больница: City Hospital</p>
    </div>
    <div class="doctor-card">
      <h4>Сообщения от врача:</h4>
      <ul>
        <li>“Пей больше воды и отдыхай” – 09.01.2026</li>
        <li>“Если головная боль не проходит, приходи на осмотр” – 10.01.2026</li>
      </ul>
    </div>
  </div>

  <div class="disclaimer">
    ⚠️ This is not a medical diagnosis. Always consult a professional.
  </div>
</div>

<script>
const chat = document.getElementById('chat');
const input = document.getElementById('input');
const confidence = document.getElementById('confidence');

function addMessage(text, role){
  const div = document.createElement('div');
  div.className = 'message ' + role;
  div.textContent = text;
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
}

function handleKey(e){
  if(e.key==='Enter' && input.value.trim()!==''){
    const userText = input.value;
    addMessage(userText,'user');
    input.value='';
    setTimeout(()=>aiReply(userText),700);
  }
}

function aiReply(text){
  addMessage("Thanks for sharing. When did these symptoms start?",'ai');
  setTimeout(()=>{
    addMessage("This could be related to stress, mild infection, or dehydration. I can’t diagnose, but I recommend rest and hydration. If symptoms worsen, consider seeing a doctor.",'ai');
    confidence.textContent="Confidence level: Medium";
  },1200);
}

// Вкладки
function showTab(tabId, button){
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  document.getElementById(tabId).classList.add('active');
  document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));
  button.classList.add('active');
}
</script>

</body>
</html>
