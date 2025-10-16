
[index.html](https://github.com/user-attachments/files/22942144/index.html)<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>集中タイマー</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h1>集中タイマー ⏱️</h1>
    <p>時間を設定して集中スタート！</p>

    <div class="input-area">
      <label for="focusTime">集中時間（分）:</label>
      <input type="number" id="focusTime" min="1" value="5">
      <button id="startBtn">スタート</button>
      <button id="resetBtn">リセット</button>
    </div>

    <div class="timer" id="timer">05:00</div>

    <div id="resultArea">
      <p id="aiMessage"></p>
      <p id="breakSuggestion"></p>
      <p id="relaxSuggestion"></p>
    </div>

    <div id="historyArea">
      <h3>集中記録 📚</h3>
      <ul id="historyList"></ul>
      <button id="clearHistoryBtn">記録を全て削除</button>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
[script.js](https://github.com/user-attachments/files/22942150/script.js)
let timer;
let timeLeft = 0;
let isRunning = false;

const timerDisplay = document.getElementById("timer");
const startBtn = document.getElementById("startBtn");
const resetBtn = document.getElementById("resetBtn");
const focusTimeInput = document.getElementById("focusTime");
const aiMessage = document.getElementById("aiMessage");
const breakSuggestion = document.getElementById("breakSuggestion");
const relaxSuggestion = document.getElementById("relaxSuggestion");
const historyList = document.getElementById("historyList");

// -------------------- タイマー開始 --------------------
startBtn.addEventListener("click", () => {
  if (isRunning) return;
  let minutes = parseInt(focusTimeInput.value);
  if (isNaN(minutes) || minutes <= 0) {
    alert("1分以上の時間を設定してください。");
    return;
  }

  timeLeft = minutes * 60;
  isRunning = true;
  startBtn.disabled = true;
  focusTimeInput.disabled = true;

  timer = setInterval(updateTimer, 1000);
});

function updateTimer() {
  const minutes = Math.floor(timeLeft / 60);
  const seconds = timeLeft % 60;
  timerDisplay.textContent = `${String(minutes).padStart(2, "0")}:${String(seconds).padStart(2, "0")}`;
  timeLeft--;

  if (timeLeft < 0) {
    clearInterval(timer);
    isRunning = false;
    startBtn.disabled = false;
    focusTimeInput.disabled = false;
    timerDisplay.textContent = "完了！";
    showResults();
    saveHistory(parseInt(focusTimeInput.value));
  }
}

// -------------------- リセット --------------------
resetBtn.addEventListener("click", () => {
  clearInterval(timer);
  isRunning = false;
  startBtn.disabled = false;
  focusTimeInput.disabled = false;
  timerDisplay.textContent = `${String(focusTimeInput.value).padStart(2, "0")}:00`;
});

// -------------------- 結果表示 --------------------
function showResults() {
  aiMessage.textContent = "お疲れさま！集中できたね！✨";

  const focusMinutes = parseInt(focusTimeInput.value);
  const breakTime = Math.round(focusMinutes * 0.2);
  const relaxOptions = [
    "肩をゆっくり回してストレッチしましょう。",
    "目を閉じて深呼吸を3回してみよう。",
    "軽く立って体を伸ばして。",
    "お水を一口飲んでリフレッシュ！",
    "手首と首をゆっくり回そう。"
  ];
  const randomRelax = relaxOptions[Math.floor(Math.random() * relaxOptions.length)];

  breakSuggestion.textContent = `休憩時間の目安：${breakTime}分`;
  relaxSuggestion.textContent = randomRelax;
}

// -------------------- 記録を保存 --------------------
function saveHistory(minutes) {
  const title = prompt("この集中時間にタイトルをつけましょう！（例：レポート執筆）", "");
  const now = new Date().toLocaleString("ja-JP");
  const record = {
    title: title || "無題の集中",
    minutes,
    date: now
  };

  let history = JSON.parse(localStorage.getItem("focusHistory")) || [];
  history.push(record);
  localStorage.setItem("focusHistory", JSON.stringify(history));

  renderHistory();
}

// -------------------- 記録の表示 --------------------
function renderHistory() {
  const history = JSON.parse(localStorage.getItem("focusHistory")) || [];
  historyList.innerHTML = "";

  history.forEach((item, index) => {
    const li = document.createElement("li");
    li.innerHTML = `
      <strong>${item.title}</strong>（${item.minutes}分）<br>
      <small>${item.date}</small>
      <div class="edit-buttons">
        <button class="edit-btn" data-index="${index}">編集</button>
        <button class="delete-btn" data-index="${index}">削除</button>
      </div>
    `;
    historyList.appendChild(li);
  });

  // 編集・削除ボタンのイベント
  document.querySelectorAll(".edit-btn").forEach(btn => {
    btn.addEventListener("click", (e) => {
      const index = e.target.dataset.index;
      editHistory(index);
    });
  });

  document.querySelectorAll(".delete-btn").forEach(btn => {
    btn.addEventListener("click", (e) => {
      const index = e.target.dataset.index;
      deleteHistory(index);
    });
  });
}

// -------------------- 編集機能 --------------------
function editHistory(index) {
  let history = JSON.parse(localStorage.getItem("focusHistory")) || [];
  const newTitle = prompt("新しいタイトルを入力してください：", history[index].title);
  if (newTitle !== null) {
    history[index].title = newTitle || history[index].title;
    localStorage.setItem("focusHistory", JSON.stringify(history));
    renderHistory();
  }
}

// -------------------- 削除機能 --------------------
function deleteHistory(index) {
  let history = JSON.parse(localStorage.getItem("focusHistory")) || [];
  history.splice(index, 1);
  localStorage.setItem("focusHistory", JSON.stringify(history));
  renderHistory();
}

// -------------------- 全削除 --------------------
document.getElementById("clearHistoryBtn").addEventListener("click", () => {
  if (confirm("本当に全ての記録を削除しますか？")) {
    localStorage.removeItem("focusHistory");
    renderHistory();
  }
});

// ページ読み込み時に記録を表示
renderHistory();
[style.css](https://github.com/user-attachments/files/22942154/style.css)
/* ========== 全体の設定 ========== */
body {
  font-family: "Hiragino Kaku Gothic ProN", "Noto Sans JP", sans-serif;
  background: linear-gradient(135deg, #cce7f6, #f3f7ff);
  color: #333;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 40px 20px;
}

/* ========== メインカード ========== */
.container {
  width: 100%;
  max-width: 520px;
  background: #ffffffcc;
  backdrop-filter: blur(8px);
  border-radius: 24px;
  box-shadow: 0 6px 25px rgba(0, 0, 0, 0.1);
  text-align: center;
  padding: 40px 30px 50px;
  transition: all 0.3s ease;
}

.container:hover {
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
}

/* ========== タイトル関連 ========== */
h1 {
  font-size: 1.8rem;
  color: #0077b6;
  letter-spacing: 0.05em;
  margin-bottom: 0.2em;
}

p {
  color: #555;
  font-size: 1rem;
  margin-bottom: 1.2em;
}

/* ========== 入力フォーム ========== */
.input-area {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  margin-top: 20px;
}

label {
  color: #444;
  font-weight: bold;
}

input {
  width: 80px;
  font-size: 18px;
  text-align: center;
  border: 2px solid #90e0ef;
  border-radius: 10px;
  padding: 6px 10px;
  transition: 0.2s;
}

input:focus {
  outline: none;
  border-color: #00b4d8;
  background-color: #e8faff;
}

button {
  background: linear-gradient(135deg, #48cae4, #00b4d8);
  color: white;
  border: none;
  border-radius: 12px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, background 0.2s ease;
}

button:hover {
  transform: translateY(-2px);
  background: linear-gradient(135deg, #0096c7, #0077b6);
}

button:disabled {
  background: #ccc;
  cursor: not-allowed;
}

/* ========== タイマー ========== */
.timer {
  font-size: 56px;
  font-weight: bold;
  color: #023e8a;
  margin: 30px 0 20px;
  letter-spacing: 2px;
}

/* ========== 結果表示エリア ========== */
#resultArea {
  margin-top: 30px;
  background: #e9faff;
  border-radius: 16px;
  padding: 20px;
  box-shadow: inset 0 0 10px rgba(0, 183, 255, 0.1);
  animation: fadeIn 1s ease;
}

#aiMessage {
  font-size: 1.2rem;
  color: #0077b6;
  font-weight: bold;
  margin-bottom: 10px;
}

#breakSuggestion {
  color: #333;
  font-weight: bold;
  margin-bottom: 5px;
}

#relaxSuggestion {
  color: #444;
  font-style: italic;
}

/* ========== 集中履歴 ========== */
#historyArea {
  margin-top: 50px;
  text-align: center;
}

#historyArea h3 {
  color: #0096c7;
  margin-bottom: 12px;
  font-size: 1.2rem;
}

#historyList {
  list-style: none;
  padding: 0;
  max-height: 180px;
  overflow-y: auto;
  background: #f8fcff;
  border-radius: 12px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.05);
  margin-bottom: 12px;
}

#historyList li {
  padding: 10px;
  border-bottom: 1px solid #e6f2ff;
  color: #333;
  font-size: 0.95rem;
}

#historyList li:last-child {
  border-bottom: none;
}

#clearHistoryBtn {
  background: #ff6b6b;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 8px 14px;
  font-size: 14px;
  cursor: pointer;
  transition: background 0.2s ease;
}

#clearHistoryBtn:hover {
  background: #d62828;
}

/* ========== アニメーション ========== */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

