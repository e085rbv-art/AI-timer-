<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AIタイマー</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>集中タイマー ⏱️</h1>

  <p>時間を設定して集中スタート！</p>

  <label for="focusTime">集中時間（分）：</label>
  <input type="number" id="focusTime" min="1" value="5">
  <button id="startBtn">スタート</button>
  <button id="resetBtn">リセット</button>

  <p id="timer">05:00</p>

  <h2>集中記録 🧠</h2>
  <button id="clearHistory">記録を全て削除</button>
  <ul id="historyList"></ul>

  <script src="script.js"></script>
</body>
</html>
