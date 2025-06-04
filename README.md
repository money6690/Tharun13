<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Ultimate Personality & Fun Quiz (No Mic, White Snowflakes)</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Baloo+2&display=swap');
    body {
      background: #001f3f;
      color: white;
      font-family: 'Baloo 2', cursive, sans-serif;
      margin: 0;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      position: relative;
      overflow-x: hidden;
    }
    #snowCanvas {
      position: fixed;
      top: 0; left: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      z-index: 0;
    }
    nav {
      background: #003366;
      padding: 12px 20px;
      display: flex;
      justify-content: flex-end;
      align-items: center;
      box-shadow: 0 2px 8px #0074D9;
      position: relative;
      z-index: 10;
    }
    nav button {
      /* rainbow background inherited from button below */
    }
    #optionsContainer label {
      cursor: pointer;
      display: flex;
      align-items: center;
      margin-bottom: 12px;
      font-weight: normal;
    }
    #optionsContainer input[type="radio"] {
      margin-right: 10px;
    }
    #optionsContainer input[type="radio"]:checked + span {
      background: #00aaff;
      color: #001f3f;
      padding: 6px 10px;
      border-radius: 6px;
      transition: background 0.3s, color 0.3s;
    }
    #optionsContainer label span {
      margin-left: 5px;
    }
    /* Rainbow-animated buttons */
    button {
      background: linear-gradient(270deg, red, orange, yellow, green, blue, indigo, violet);
      background-size: 1400% 1400%;
      animation: rainbow 5s ease infinite;
      color: white;
      border: none;
      padding: 10px 18px;
      font-size: 1rem;
      border-radius: 8px;
      cursor: pointer;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }
    button:disabled {
      background: #004466;
      cursor: not-allowed;
      animation: none;
    }
    button:hover:not(:disabled) {
      transform: scale(1.05);
    }
    button:active:not(:disabled) {
      transform: scale(0.95);
      box-shadow: 0 0 12px white;
    }
    @keyframes rainbow {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    .container {
      flex: 1;
      max-width: 720px;
      margin: 30px auto;
      background: #003366;
      border-radius: 15px;
      padding: 30px 40px;
      box-shadow: 0 0 25px #0074D9;
      display: flex;
      flex-direction: column;
      align-items: center;
      position: relative;
      z-index: 5;
    }
    #progressBar {
      width: 100%;
      background: #002244;
      border-radius: 8px;
      height: 14px;
      margin: 15px 0 30px;
      overflow: hidden;
    }
    #progressFill {
      height: 100%;
      background: #00aaff;
      width: 0%;
      transition: width 0.3s ease;
    }
    input[type="text"], textarea, select {
      width: 100%;
      padding: 10px 12px;
      border-radius: 8px;
      border: none;
      font-size: 1rem;
      margin-bottom: 20px;
      box-sizing: border-box;
    }
    textarea {
      resize: vertical;
      min-height: 60px;
    }
    #summary {
      text-align: center;
      width: 100%;
      white-space: pre-wrap;
    }
    #summary h2 {
      animation: glowText 2s ease-in-out infinite alternate;
      color: #00ffff;
      text-shadow: 0 0 5px #00ffff, 0 0 10px #00ffff, 0 0 20px #00ffff;
    }
    @keyframes glowText {
      from {
        text-shadow: 0 0 5px #00ffff, 0 0 10px #00ffff, 0 0 20px #00ffff;
      }
      to {
        text-shadow: 0 0 10px #00ffff, 0 0 20px #00ffff, 0 0 30px #00ffff;
      }
    }
    #adminModal {
      display: none;
      position: fixed;
      z-index: 20;
      left: 0; top: 0;
      width: 100vw; height: 100vh;
      background: rgba(0,0,0,0.7);
      display: flex;
      justify-content: center;
      align-items: center;
    }
    #adminContent {
      background: #003366;
      padding: 30px;
      border-radius: 15px;
      width: 320px;
      color: white;
    }
    #adminContent input[type="password"] {
      width: 100%;
      margin-bottom: 15px;
    }
    #adminError {
      color: #ff5555;
      margin-bottom: 15px;
    }
    #adminDataPre {
      background: #002244;
      padding: 15px;
      border-radius: 10px;
      max-height: 200px;
      overflow: auto;
      white-space: pre-wrap;
    }
  </style>
</head>
<body>
  <canvas id="snowCanvas"></canvas>
  <nav>
    <button id="adminBtn">Admin</button>
  </nav>
  <div class="container">
    <div id="instaNameSection">
      <h1>Welcome to the Ultimate Personality & Fun Quiz</h1>
      <label for="instaNameInput">Please enter your Instagram name to start:</label>
      <input type="text" id="instaNameInput" placeholder="@yourusername" />
      <button id="startQuizBtn" disabled>Start Quiz</button>
    </div>
    <div id="quizSection" style="display:none;">
      <div id="progressBar">
        <div id="progressFill"></div>
      </div>
      <div id="questionSection">
        <div id="questionNumber">Question 1 of 20</div>
        <div id="questionText"></div>
        <div id="optionsContainer"></div>
        <textarea id="answerInput" placeholder="Type your answer here"></textarea>
        <div class="buttons">
          <button id="nextBtn">Next</button>
        </div>
      </div>
      <div id="summary" style="display:none;"></div>
    </div>
  </div>
  <div id="adminModal">
    <div id="adminContent">
      <h2>Admin Panel – Enter Password</h2>
      <input type="password" id="adminPasswordInput" placeholder="Enter Admin Password" />
      <div id="adminError"></div>
      <button id="adminSubmitBtn">Unlock</button>
      <button id="adminCloseBtn">Close</button>
      <pre id="adminDataPre" style="display:none;"></pre>
    </div>
  </div>
  <script>
    const canvas = document.getElementById('snowCanvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    let flakes = [];
    function createFlake() {
      flakes.push({ x: Math.random()*canvas.width, y: 0, r: Math.random()*5 + 2, d: Math.random() + 1 });
    }
    function drawFlakes() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = "white";
      ctx.beginPath();
      for (let i = 0; i < flakes.length; i++) {
        const f = flakes[i];
        ctx.moveTo(f.x, f.y);
        ctx.arc(f.x, f.y, f.r, 0, Math.PI*2, true);
      }
      ctx.fill();
      moveFlakes();
    }
    function moveFlakes() {
      for (let i = 0; i < flakes.length; i++) {
        const f = flakes[i];
        f.y += f.d;
        if (f.y > canvas.height) {
          flakes[i] = { x: Math.random()*canvas.width, y: 0, r: f.r, d: f.d };
        }
      }
    }
    setInterval(() => {
      createFlake();
      drawFlakes();
    }, 25);

    const questions = [
      { text: "What do you like about me?", type: "options", options: ["Smile", "Humor", "Confidence", "Honesty"] },
      { text: "Why do you like that about me?", type: "text" },
      { text: "What don't you like about me?", type: "options", options: ["Anger", "Attitude", "Stubbornness", "Impatience"] },
      { text: "What bothers you about this?", type: "text" },
      { text: "What is something you like in my character?", type: "text" },
      { text: "What do you expect from me?", type: "options", options: ["More Time Together", "Better Communication", "More Honesty", "Greater Support", "Understanding", "Loyalty"] },
      { text: "If you had to choose, would you rather give me your time or your attention?", type: "options", options: ["Time", "Attention"] },
      { text: "What is the funniest comedy incident you remember involving me or us?", type: "text" },
      { text: "Tell a comedy incident in your own words.", type: "text" },
      { text: "What is the angriest thing about me?", type: "text" },
      { text: "Share an angry incident or moment you experienced with me.", type: "text" },
      { text: "What do you think I should improve about my attitude?", type: "text" },
      { text: "What’s a memorable emotional moment between us?", type: "text" },
      { text: "If you could change one thing about me, what would it be?", type: "text" },
      { text: "What do you admire most about my personality?", type: "text" },
      { text: "Describe a moment you felt very happy with me.", type: "text" },
      { text: "What’s your favorite joke or funny moment we shared?", type: "text" },
      { text: "What is the suggestion do you give to me and I will add some features to my character", type: "text" }
    ];

    const instaNameInput = document.getElementById("instaNameInput");
    const startQuizBtn   = document.getElementById("startQuizBtn");
    const quizSection    = document.getElementById("quizSection");
    const questionNumber = document.getElementById("questionNumber");
    const questionText   = document.getElementById("questionText");
    const optionsContainer = document.getElementById("optionsContainer");
    const answerInput    = document.getElementById("answerInput");
    const nextBtn        = document.getElementById("nextBtn");
    const progressFill   = document.getElementById("progressFill");
    const summary        = document.getElementById("summary");
    let currentQuestion  = 0;
    let currentUsername  = "";
    let userAnswers      = {};

    // Load saved data
    const savedData = localStorage.getItem('userAnswers');
    if (savedData) {
      try { userAnswers = JSON.parse(savedData); }
      catch { userAnswers = {}; }
    }
    const savedUser = localStorage.getItem('currentUsername');
    if (savedUser) currentUsername = savedUser;
    const savedQuestion = localStorage.getItem('currentQuestion');
    if (savedQuestion) currentQuestion = parseInt(savedQuestion, 10);

    instaNameInput.addEventListener("input", () => {
      startQuizBtn.disabled = instaNameInput.value.trim() === "";
    });

    startQuizBtn.addEventListener("click", () => {
      currentUsername = instaNameInput.value.trim();
      if (!userAnswers[currentUsername]) {
        userAnswers[currentUsername] = [];
        currentQuestion = 0;
      } else {
        currentQuestion = userAnswers[currentUsername].length || 0;
      }
      localStorage.setItem('currentUsername', currentUsername);
      localStorage.setItem('currentQuestion', currentQuestion);
      document.getElementById("instaNameSection").style.display = "none";
      quizSection.style.display = "block";
      loadQuestion();
    });

    function loadQuestion() {
      const q = questions[currentQuestion];
      questionNumber.textContent = `Question ${currentQuestion + 1} of ${questions.length}`;
      questionText.textContent = q.text;
      optionsContainer.innerHTML = "";
      answerInput.value = "";
      nextBtn.disabled = true;

      if (q.type === "options") {
        q.options.forEach(opt => {
          const label = document.createElement("label");
          const radio = document.createElement("input");
          radio.type = "radio";
          radio.name = "option";
          radio.value = opt;
          radio.addEventListener("change", () => nextBtn.disabled = false);
          label.appendChild(radio);
          const span = document.createElement("span");
          span.textContent = opt;
          label.appendChild(span);
          optionsContainer.appendChild(label);
        });
        answerInput.style.display = "none";

        const saved = userAnswers[currentUsername]?.[currentQuestion];
        if (saved && saved.answer) {
          document.querySelectorAll("input[name='option']").forEach(r => {
            if (r.value === saved.answer) {
              r.checked = true;
              nextBtn.disabled = false;
            }
          });
        }
      } else {
        answerInput.style.display = "block";
        const saved = userAnswers[currentUsername]?.[currentQuestion];
        if (saved && saved.answer) {
          answerInput.value = saved.answer;
          nextBtn.disabled = false;
        }
        answerInput.addEventListener("input", () => {
          nextBtn.disabled = answerInput.value.trim() === "";
        });
      }
      progressFill.style.width = `${(currentQuestion / questions.length) * 100}%`;
    }

    nextBtn.addEventListener("click", () => {
      const q = questions[currentQuestion];
      const answer = q.type === "options"
        ? document.querySelector("input[name='option']:checked")?.value
        : answerInput.value.trim();

      if (!userAnswers[currentUsername]) userAnswers[currentUsername] = [];
      userAnswers[currentUsername][currentQuestion] = { question: q.text, answer };

      localStorage.setItem('userAnswers', JSON.stringify(userAnswers));
      localStorage.setItem('currentQuestion', currentQuestion + 1);

      currentQuestion++;
      if (currentQuestion < questions.length) {
        loadQuestion();
      } else {
        showSummary();
      }
    });

    function showSummary() {
      document.getElementById("questionSection").style.display = "none";
      summary.style.display = "block";
      summary.innerHTML = `
        <h2>Thank you for your feedback, ${currentUsername}!</h2>
        <button onclick="location.reload()">Logout</button>
      `;
    }

    // Admin Panel Logic
    const adminBtn = document.getElementById("adminBtn");
    const adminModal = document.getElementById("adminModal");
    const adminCloseBtn = document.getElementById("adminCloseBtn");
    const adminSubmitBtn = document.getElementById("adminSubmitBtn");
    const adminPasswordInput = document.getElementById("adminPasswordInput");
    const adminError = document.getElementById("adminError");
    const adminDataPre = document.getElementById("adminDataPre");

    adminBtn.addEventListener("click", () => adminModal.style.display = "flex");
    adminCloseBtn.addEventListener("click", () => adminModal.style.display = "none");

    adminSubmitBtn.addEventListener("click", () => {
      const pwd = adminPasswordInput.value;
      if (pwd === "2008") {
        adminError.textContent = "";
        let out = "";
        for (let name in userAnswers) {
          out += `Name: ${name}\n`;
          userAnswers[name].forEach((qa, i) => {
            out += `${i+1}. ${qa.question}\n- ${qa.answer}\n`;
          });
          out += "\n";
        }
        adminDataPre.style.display = "block";
        adminDataPre.textContent = out;
      } else {
        adminError.textContent = "Incorrect password. It must be exactly 16 characters.";
        adminDataPre.style.display = "none";
      }
    });
  </script>
</body>
</html>
