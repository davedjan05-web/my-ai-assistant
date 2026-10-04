# my-ai-assistant
مستودع ai
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>مساعدي الذكي</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #111827;
      color: white;
      text-align: center;
      padding: 30px;
    }

    .box {
      max-width: 500px;
      margin: auto;
      background: #1f2937;
      padding: 25px;
      border-radius: 20px;
    }

    input {
      width: 90%;
      padding: 15px;
      border-radius: 10px;
      border: none;
      margin: 10px;
      font-size: 16px;
      box-sizing: border-box;
    }

    button {
      padding: 12px 25px;
      border: none;
      border-radius: 10px;
      background: #2563eb;
      color: white;
      font-size: 16px;
      margin: 5px;
    }

    #voiceButton {
      background: #16a34a;
      font-size: 20px;
    }

    #answer {
      margin-top: 20px;
      font-size: 18px;
      white-space: pre-line;
    }

    #status {
      margin-top: 15px;
      color: #9ca3af;
    }
  </style>
</head>

<body>

<div class="box">

  <h1>🤖 مساعدي الذكي</h1>

  <p>اكتب أو تحدث معي</p>

  <input
    id="question"
    placeholder="اكتب سؤالك هنا"
  >

  <br>

  <button onclick="answer()">اسألني</button>

  <button id="voiceButton" onclick="startListening()">🎙️ تحدث</button>

  <div id="status"></div>

  <div id="answer"></div>

</div>

<script>

function answer() {

  const question =
    document.getElementById("question").value;

  const result =
    document.getElementById("answer");

  if (question.trim() === "") {

    result.innerText =
      "اكتب شيئاً أولاً 🙂";

    return;
  }

  const response =
    "أهلاً! فهمت سؤالك: " +
    question +
    "\n\nأنا مساعدك الذكي 🤖";

  result.innerText = response;

  speak(response);
}


function speak(text) {

  if ("speechSynthesis" in window) {

    const speech =
      new SpeechSynthesisUtterance(text);

    speech.lang = "ar-SA";

    speech.rate = 1;

    window.speechSynthesis.speak(speech);
  }
}


function startListening() {

  const SpeechRecognition =
    window.SpeechRecognition ||
    window.webkitSpeechRecognition;

  const status =
    document.getElementById("status");

  if (!SpeechRecognition) {

    status.innerText =
      "المتصفح لا يدعم التحدث الصوتي حالياً.";

    return;
  }

  const recognition =
    new SpeechRecognition();

  recognition.lang = "ar-SA";

  recognition.interimResults = false;

  recognition.continuous = false;


  status.innerText =
    "🎙️ اسمعك... احكي الآن";

  recognition.start();


  recognition.onresult = function(event) {

    const text =
      event.results[0][0].transcript;

    document.getElementById("question").value = text;

    status.innerText =
      "تم سماع كلامك ✅";

    answer();
  };


  recognition.onerror = function() {

    status.innerText =
      "ما قدرت أسمعك، جرّب مرة ثانية.";
  };


  recognition.onend = function() {

    if (status.innerText === "🎙️ اسمعك... احكي الآن") {

      status.innerText = "";
    }
  };
}

</script>

</body>
</html>
