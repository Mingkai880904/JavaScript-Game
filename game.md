```
<html lang="zh-TW"><head>
  <meta charset="UTF-8">
  <title>葉問打光頭</title>
  <link rel="stylesheet" href="style.css">
</head>

 <form>
<select name="數量">
　<option value="three">3</option>
　<option value="four">4</option>
　<option value="five">5</option>
　<option value="six">6</option>
</select>     
</form>
    
<body class="bod">
  <h1>葉問打光頭 得分:
    <span class="score">0</span>
  </h1>
    
    
 <head>
<script type="text/javascript">
function timedMsg()
{
var t=setTimeout("alert('時間到!!')",20000)
}
</script>
</head>

<body>
<form>
<div class="startBtn">
<input type="button" value="遊戲開始" onclick="timedMsg();startGame()">
</div>
</form>
<p>請點選上面的按钮 遊戲20秒倒數計時</p>
</body>  
  

  <div class="game">
    <div class="hole hole1">
      <div class="mole"></div>
    </div>
    <div class="hole hole2">
      <div class="mole"></div>
    </div>
    <div class="hole hole3">
      <div class="mole"></div>
    </div>
    <div class="hole hole4">
      <div class="mole"></div>
    </div>
    <div class="hole hole5">
      <div class="mole"></div>
    </div>
    <div class="hole hole6">
      <div class="mole"></div>
    </div>
      <div class="hole hole7">
      <div class="mole"></div>
    </div>
      <div class="hole hole8">
      <div class="mole"></div>
    </div>
    <div class="hole hole9">
      <div class="mole"></div>
    </div>
  </div>

  <script>
    /**  取得頁面元素 */
    const holes = document.querySelectorAll('.hole');
    const scoreBoard = document.querySelector('.score');
    const moles = document.querySelectorAll('.mole');
    
    /** 預設變數設定 */
    let lastHole; // 最後一次出現的地鼠洞
    let timeUP = false; // 遊戲時間是否結束戳記
    let score = 0; // 分數

    /** 地鼠出現後存在時間，傳入最小&最大值，回傳一個區間亂數 */
    function randomTime(min, max) {
      return Math.round(Math.random() * (max - min) + min);
    }

    /** 地鼠出現的洞 */
    function randomHole(holes) {
      // 取得地鼠洞數量區間內隨機一個洞
      const idx = Math.floor(Math.random() * holes.length);
      const hole = holes[idx];
      // 避免骰到相同的
      if (hole === lastHole) {
        return randomHole(holes);
      }
      // 紀錄最後一個出現的地鼠洞
      lastHole = hole;
      return hole;
    }

    /**  地鼠出現 */
    function peep() {
      // 取得存在時間
      const time = randomTime(100, 1000);
      // 取得出現的洞
      const hole = randomHole(holes);
      // 移除已槌標記
      hole.querySelector('.mole').classList.remove('bonked');
      // 增加出現的動畫class
      hole.classList.add('up');
      // 設定存在時間到的時候移除出現動畫，且若遊戲時間未結束就繼續跑下一run
      setTimeout(() => {
        hole.classList.remove('up');
        if (!timeUP) peep();
      }, time)
    }

    /** 打地鼠 */
    function bonk(e) {
      // isTrusted防止腳本操作，class有bonked代表已被搥過，若符合上述兩者則不進行
      if(!e.isTrusted || this.classList.contains('bonked')) return;
      // 替被打到的地鼠加上bonked的樣式避免連續點擊得分
      this.classList.add('bonked'); 
      // 打到就移除出現的動畫
      this.classList.remove('up');
      // 加分
      score++;
      // 更新顯示分數
      scoreBoard.textContent = score;
    }

    /** 開始遊戲 */
    function startGame() {
      // 時間重置
      timeUP = false;
      // 分數歸零
      scoreBoard.textContent = 0;
      score = 0;
      // 執行地鼠出現函式
      peep();
      // 設定十秒後把時間押為結束
      setTimeout(() => timeUP = true, 20000);
    }

    // 替每個地鼠加上click事件綁定bonk（打地鼠）
    moles.forEach(mole => mole.addEventListener('click', bonk));
  </script>


</body></html>
```
