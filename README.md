<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>競艇オッズ予想ゲーム</title>

<style>
body{
  margin:0;
  font-family:sans-serif;
  background:#eef3ff;
}

.screen{
  display:none;
  padding:20px;
}

.screen.active{
  display:block;
}

h1{margin:0 0 10px}

button{
  padding:12px 20px;
  font-size:16px;
  border-radius:10px;
  border:none;
  background:#111;
  color:#fff;
  cursor:pointer;
}

.bigCanvas{
  width:100%;
  height:50vh; /* ← 画面半分 */
  background:#bfe9ff;
  border-radius:12px;
  border:2px solid #fff;
}

.center{
  text-align:center;
}

.card{
  background:#fff;
  padding:15px;
  border-radius:12px;
  margin-bottom:15px;
}
</style>
</head>

<body>

<!-- ① 投票画面 -->
<div id="screenBet" class="screen active">
  <h1>🚤 展示情報＆買い目</h1>

  <div class="card">
    <p>ここに展示情報（仮）</p>
  </div>

  <div class="card">
    <label>金額：
      <input id="money" type="number" value="100">
    </label>
  </div>

  <button onclick="startRace()">購入してレース開始</button>
</div>



<!-- ② レース画面 -->
<div id="screenRace" class="screen center">
  <h1>レース中…</h1>

  <canvas id="raceCanvas" class="bigCanvas"></canvas>
</div>



<!-- ③ 結果画面 -->
<div id="screenResult" class="screen center">
  <h1>🏁 結果</h1>

  <div id="resultText" class="card"></div>

  <button onclick="backToBet()">もう一度遊ぶ</button>
</div>



<script>
const betScreen   = document.getElementById("screenBet");
const raceScreen  = document.getElementById("screenRace");
const resultScreen= document.getElementById("screenResult");
const canvas      = document.getElementById("raceCanvas");
const ctx         = canvas.getContext("2d");

function show(screen){
  betScreen.classList.remove("active");
  raceScreen.classList.remove("active");
  resultScreen.classList.remove("active");
  screen.classList.add("active");
}

/* ===== レース開始 ===== */
function startRace(){
  show(raceScreen);

  resizeCanvas();
  animateRace();
}

/* ===== キャンバスサイズ調整 ===== */
function resizeCanvas(){
  canvas.width  = canvas.clientWidth;
  canvas.height = canvas.clientHeight;
}

/* ===== 簡易レースアニメ ===== */
function animateRace(){
  let x = 0;
  const speed = 2;

  function loop(){
    ctx.clearRect(0,0,canvas.width,canvas.height);

    // 水面
    ctx.fillStyle = "#7dd3fc";
    ctx.fillRect(0,0,canvas.width,canvas.height);

    // ボート（丸）
    ctx.fillStyle = "#111";
    ctx.beginPath();
    ctx.arc(50 + x, canvas.height/2, 10, 0, Math.PI*2);
    ctx.fill();

    x += speed;

    if(x < canvas.width - 100){
      requestAnimationFrame(loop);
    }else{
      finishRace();
    }
  }

  loop();
}

/* ===== ゴール後 ===== */
function finishRace(){
  setTimeout(()=>{
    document.getElementById("resultText").innerText =
      "1着：1号艇（仮）\nおめでとう！";

    show(resultScreen);
  },800);
}

/* ===== 戻る ===== */
function backToBet(){
  show(betScreen);
}
</script>

</body>
</html>
