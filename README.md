[index.html](https://github.com/user-attachments/files/22940678/index.html)
<!DOCTYPE html>
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
