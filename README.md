<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Ultimate Personality Quiz</title>
  <style>
    body { background: #001f3f; color: white; font-family: sans-serif; margin: 0; padding: 0; }
    .container { max-width: 600px; margin: 0 auto; padding: 20px; }
    input, textarea, button { width: 100%; padding: 10px; margin: 10px 0; border-radius: 8px; border: none; }
    button { background-color: #0074D9; color: white; font-size: 1rem; cursor: pointer; }
    button:hover { background-color: #005fa3; }
    #adminSection { display: none; background: #003366; padding: 20px; border-radius: 10px; margin-top: 20px; }
    #adminOutput { white-space: pre-wrap; max-height: 300px; overflow-y: auto; background: #002244; padding: 15px; border-radius: 10px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>Fun Personality Quiz</h1>
    <label>Enter your Instagram Name:</label>
    <input id="username" placeholder="@yourname" />

    <div id="quiz"></div>

    <button id="submitBtn">Submit</button>

    <hr />

    <button id="showAdmin">Admin</button>
    <div id="adminSection">
      <input type="password" id="adminPass" placeholder="Enter Admin Password" />
      <button id="unlockBtn">Unlock</button>
      <div id="adminOutput"></div>
    </div>
  </div>

  <script>
    const questions = [
      "What do you like about me?",
      "Why do you like that about me?",
      "What don't you like about me?",
      "What bothers you about this?",
      "What is something you like in my character?",
      "What do you expect from me?",
      "Would you give me time or attention?",
      "Funniest memory with me?",
      "Tell that moment in your words.",
      "What made you angry with me?",
      "Share an angry moment with me.",
      "How should I improve my attitude?",
      "Emotional moment between us?",
      "Change one thing about me?",
      "What do you admire most in me?",
      "Happy moment with me?",
      "Favorite joke or funny moment?",
      "Any suggestion to improve me?"
    ];

    const quizDiv = document.getElementById('quiz');
    questions.forEach((q, i) => {
      const textarea = document.createElement('textarea');
      textarea.placeholder = q;
      textarea.id = 'q' + i;
      quizDiv.appendChild(textarea);
    });

    document.getElementById('submitBtn').onclick = () => {
      const username = document.getElementById('username').value.trim();
      if (!username) return alert("Please enter Instagram name");

      const answers = {};
      questions.forEach((q, i) => {
        answers[q] = document.getElementById('q' + i).value.trim();
      });

      const payload = { username, answers };
      fetch('https://tharun-quiz-data-endpoint.vercel.app/api/store', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      }).then(() => {
        alert("Responses submitted! Thank you ❤️");
        location.reload();
      }).catch(err => {
        alert("Failed to save. Try again later.");
        console.error(err);
      });
    };

    // Admin access
    document.getElementById('showAdmin').onclick = () => {
      document.getElementById('adminSection').style.display = 'block';
    };

    document.getElementById('unlockBtn').onclick = () => {
      const pass = document.getElementById('adminPass').value;
      if (pass !== "2008") {
        alert("Incorrect password");
        return;
      }

      fetch('https://tharun-quiz-data-endpoint.vercel.app/api/all')
        .then(res => res.json())
        .then(data => {
          let out = "";
          data.forEach(entry => {
            out += `Name: ${entry.username}\n`;
            Object.entries(entry.answers).forEach(([q, a], i) => {
              out += `${i + 1}. ${q}\n→ ${a}\n`;
            });
            out += `\n---\n\n`;
          });
          document.getElementById('adminOutput').textContent = out;
        });
    };
  </script>
</body>
</html>
