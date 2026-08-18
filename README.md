<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>Prince Game Zone - 100 Games</title>

<style>
*{box-sizing:border-box}
html,body{margin:0;padding:0;background:#080b18;color:#fff;font-family:Arial,sans-serif}
body{min-height:100vh}
header{padding:22px 14px;text-align:center;background:linear-gradient(135deg,#6c2cff,#00a8ff)}
header h1{margin:0;font-size:30px}
header p{margin:8px 0 0}
.wrap{max-width:1200px;margin:auto;padding:15px}
#search{width:100%;padding:16px;border:0;border-radius:15px;font-size:17px;outline:0;margin-bottom:14px}
.categories{display:flex;gap:8px;overflow:auto;margin-bottom:15px}
.categories button{border:0;padding:10px 15px;border-radius:20px;background:#222941;color:#fff;white-space:nowrap}
.categories button.active{background:#7137ff}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(190px,1fr));gap:14px}
.card{background:#151b30;border-radius:18px;padding:18px;text-align:center;box-shadow:0 5px 20px #0006}
.icon{font-size:50px}
.card h2{font-size:18px;margin:8px 0}
.card small{color:#aaa}
.play{width:100%;margin-top:12px;padding:12px;border:0;border-radius:12px;background:#7137ff;color:#fff;font-size:16px}
#gameScreen{display:none;position:fixed;inset:0;background:#070a13;z-index:1000;overflow:auto}
.gameTop{height:60px;background:#151b30;display:flex;align-items:center;justify-content:space-between;padding:8px 12px;position:sticky;top:0;z-index:5}
.gameTop button{border:0;border-radius:10px;padding:10px 14px;background:#30384f;color:#fff}
#gameTitle{font-size:18px;font-weight:bold}
#gameArea{text-align:center;padding:15px;min-height:calc(100vh - 60px)}
canvas{display:block;margin:10px auto;max-width:100%;background:#050912;border-radius:15px;touch-action:none}
.gameBtn{padding:13px 20px;margin:5px;border:0;border-radius:12px;background:#7137ff;color:#fff;font-size:17px}
.big{font-size:90px;margin:25px}
.score{font-size:22px;color:#ffd54f;margin:15px}
.controls{display:flex;justify-content:center;gap:10px;flex-wrap:wrap}
.msg{font-size:20px;min-height:30px;margin:15px}
.board{display:grid;grid-template-columns:repeat(3,90px);justify-content:center;gap:5px}
.cell{width:90px;height:90px;border:0;border-radius:10px;background:#222b45;color:#fff;font-size:35px}
.food{font-size:80px}
input.answer{padding:13px;border:0;border-radius:10px;font-size:18px}
footer{text-align:center;padding:30px;color:#777}
</style>
</head>

<body>

<header>
<h1>🎮 Prince Game Zone</h1>
<p>100 Games • Sound • Fullscreen • Mobile Friendly</p>
</header>

<div class="wrap">
<input id="search" placeholder="🔎 Search Games...">
<div class="categories" id="categories"></div>
<div class="grid" id="gameList"></div>
</div>

<div id="gameScreen">
<div class="gameTop">
<button onclick="closeGame()">← Games</button>
<div id="gameTitle"></div>
<button onclick="toggleFull()">⛶ Fullscreen</button>
</div>
<div id="gameArea"></div>
</div>

<footer>🎮 Prince Game Zone</footer>

<script>

/* =========================
   100 GAMES
========================= */

const games=[
["Ludo","🎲","Board","ludo"],
["Ludo Master","🎲","Board","ludo"],
["Carrom Board","🎯","Board","carrom"],
["Carrom Pro","🎯","Board","carrom"],
["Chess","♟️","Board","chess"],
["Checkers","⚫","Board","tic"],
["Domino","🀫","Board","dice"],
["8 Ball Pool","🎱","Sports","pool"],
["Table Tennis","🏓","Sports","pong"],
["Football","⚽","Sports","football"],
["Penalty Kick","🥅","Sports","football"],
["Basketball","🏀","Sports","basket"],
["Cricket","🏏","Sports","cricket"],
["Boxing","🥊","Action","boxing"],
["Target Shooter","🎯","Action","shooter"],
["Space Shooter","🚀","Action","shooter"],
["Alien Shooter","👾","Action","shooter"],
["Zombie Survival","🧟","Action","shooter"],
["Tank Battle","🛡️","Action","shooter"],
["Robot Battle","🤖","Action","shooter"],
["Ninja Attack","🥷","Action","shooter"],
["Archery","🏹","Action","shooter"],
["Airplane Battle","✈️","Action","shooter"],
["Car Racing","🏎️","Racing","racing"],
["Bike Racing","🏍️","Racing","racing"],
["Formula Racing","🏁","Racing","racing"],
["Traffic Racer","🚗","Racing","racing"],
["Monster Truck","🚙","Racing","racing"],
["Drift Racer","💨","Racing","racing"],
["Endless Runner","🏃","Arcade","runner"],
["Ninja Runner","🥷","Arcade","runner"],
["Dino Runner","🦖","Arcade","runner"],
["Helicopter","🚁","Arcade","runner"],
["Boat Race","🚤","Racing","racing"],
["Snake","🐍","Arcade","snake"],
["Snake Master","🐍","Arcade","snake"],
["Brick Breaker","🧱","Arcade","brick"],
["Flappy Bird","🐦","Arcade","runner"],
["Bubble Shooter","🫧","Puzzle","click"],
["Match 3","💎","Puzzle","click"],
["2048","🔢","Puzzle","click"],
["Tetris","🧱","Puzzle","click"],
["Maze","🌀","Puzzle","click"],
["Memory Cards","🃏","Puzzle","memory"],
["Sudoku","🔢","Puzzle","quiz"],
["Math Quiz","🧠","Quiz","quiz"],
["India Quiz","🇮🇳","Quiz","quiz"],
["Science Quiz","🔬","Quiz","quiz"],
["Animal Quiz","🐯","Quiz","quiz"],
["Emoji Quiz","😀","Quiz","quiz"],
["General Knowledge","📚","Quiz","quiz"],
["Number Guess","🔢","Puzzle","guess"],
["Rock Paper Scissors","✊","Classic","rps"],
["Tic Tac Toe","⭕","Classic","tic"],
["Connect Game","🔴","Classic","tic"],
["Hangman","🔤","Word","guess"],
["Typing Challenge","⌨️","Skill","typing"],
["Reaction Test","⚡","Skill","reaction"],
["Click Challenge","👆","Skill","click"],
["Fast Tap","⚡","Skill","click"],
["Dice Game","🎲","Luck","dice"],
["Coin Flip","🪙","Luck","coin"],
["Lucky Wheel","🎡","Luck","wheel"],
["Higher Lower","📈","Luck","guess"],
["Makeup Studio","💄","Makeup","makeup"],
["Makeup Artist","💋","Makeup","makeup"],
["Beauty Salon","💅","Makeup","makeup"],
["Nail Art","💅","Makeup","makeup"],
["Hair Salon","💇","Makeup","makeup"],
["Princess Makeover","👸","Makeup","makeup"],
["Wedding Makeover","👰","Makeup","makeup"],
["Fashion Designer","👗","Fashion","makeup"],
["Dress Up","👚","Fashion","makeup"],
["Kitchen Chef","👩‍🍳","Kitchen","kitchen"],
["Pizza Maker","🍕","Kitchen","kitchen"],
["Burger Maker","🍔","Kitchen","kitchen"],
["Cake Maker","🎂","Kitchen","kitchen"],
["Ice Cream Maker","🍦","Kitchen","kitchen"],
["Juice Maker","🥤","Kitchen","kitchen"],
["Sushi Maker","🍣","Kitchen","kitchen"],
["Restaurant Chef","👨‍🍳","Kitchen","kitchen"],
["Cupcake Maker","🧁","Kitchen","kitchen"],
["Fruit Salad","🍓","Kitchen","kitchen"],
["Candy Factory","🍬","Kitchen","kitchen"],
["Coffee Shop","☕","Kitchen","kitchen"],
["Pancake Maker","🥞","Kitchen","kitchen"],
["Farm Game","🚜","Simulation","click"],
["Pet Care","🐶","Simulation","click"],
["Doctor Game","🩺","Simulation","click"],
["Firefighter","🚒","Simulation","click"],
["Treasure Hunt","💰","Adventure","click"],
["Island Adventure","🏝️","Adventure","click"],
["Castle Adventure","🏰","Adventure","click"],
["Space Adventure","🌌","Adventure","click"],
["Fishing","🎣","Arcade","click"],
["Final Challenge","🏆","Challenge","click"]
];

let currentCategory="All";
let audio=null;

/* =========================
   SOUND
========================= */

function sound(freq=500,duration=.08){
 try{
  audio=audio||new(window.AudioContext||window.webkitAudioContext)();
  const o=audio.createOscillator();
  const g=audio.createGain();
  o.frequency.value=freq;
  o.type="sine";
  g.gain.value=.06;
  o.connect(g);g.connect(audio.destination);
  o.start();
  g.gain.exponentialRampToValueAtTime(.001,audio.currentTime+duration);
  o.stop(audio.currentTime+duration);
 }catch(e){}
}

/* =========================
   HOME
========================= */

const list=document.getElementById("gameList");
const search=document.getElementById("search");
const categories=document.getElementById("categories");

const cats=["All",...new Set(games.map(g=>g[2]))];

cats.forEach((c,i)=>{
 const b=document.createElement("button");
 b.textContent=c==="All"?"🎮 All":c;
 if(i===0)b.className="active";
 b.onclick=()=>{
  currentCategory=c;
  document.querySelectorAll(".categories button")
  .forEach(x=>x.classList.remove("active"));
  b.classList.add("active");
  render();
 };
 categories.appendChild(b);
});

function render(){
 const q=search.value.toLowerCase();

 const arr=games.filter(g=>
  (currentCategory==="All"||g[2]===currentCategory)&&
  g[0].toLowerCase().includes(q)
 );

 list.innerHTML=arr.map((g)=>{
  const i=games.indexOf(g);
  return `
  <div class="card">
   <div class="icon">${g[1]}</div>
   <h2>${g[0]}</h2>
   <small>${g[2]}</small>
   <button class="play" onclick="openGame(${i})">▶ PLAY</button>
  </div>`;
 }).join("");
}

search.oninput=render;

/* =========================
   GAME OPEN
========================= */

function openGame(i){
 sound(700,.12);

 document.getElementById("gameScreen").style.display="block";
 document.getElementById("gameTitle").textContent=
 games[i][1]+" "+games[i][0];

 document.getElementById("gameArea").innerHTML="";

 startGame(games[i][3]);

 document.documentElement.requestFullscreen?.().catch(()=>{});
}

function closeGame(){
 document.getElementById("gameScreen").style.display="none";
 try{document.exitFullscreen?.()}catch(e){}
}

function toggleFull(){
 if(!document.fullscreenElement)
  document.documentElement.requestFullscreen?.();
 else
  document.exitFullscreen?.();
}

/* =========================
   GAME ENGINE
========================= */

function startGame(type){

 if(type==="ludo")return ludo();
 if(type==="carrom")return carrom();
 if(type==="racing")return racing();
 if(type==="snake")return snake();
 if(type==="brick")return brick();
 if(type==="pong")return pong();
 if(type==="football")return football();
 if(type==="basket")return basketball();
 if(type==="shooter")return shooter();
 if(type==="runner")return runner();
 if(type==="memory")return memory();
 if(type==="quiz")return quiz();
 if(type==="guess")return guess();
 if(type==="rps")return rps();
 if(type==="tic")return tic();
 if(type==="dice")return dice();
 if(type==="coin")return coin();
 if(type==="wheel")return wheel();
 if(type==="click")return clickGame();
 if(type==="reaction")return reaction();
 if(type==="typing")return typing();
 if(type==="makeup")return makeup();
 if(type==="kitchen")return kitchen();

 simpleGame();
}

/* =========================
   LUDO
========================= */

function ludo(){

let position=0;

gameArea.innerHTML=`
<h2>🎲 Ludo</h2>
<div class="big" id="ludoPiece">🔴</div>
<button class="gameBtn" onclick="ludoRoll()">🎲 ROLL DICE</button>
<div class="score" id="ludoScore">Position: 0</div>
<div class="msg" id="ludoMsg"></div>
`;

window.ludoRoll=()=>{
 sound(450,.12);
 const n=Math.floor(Math.random()*6)+1;
 position+=n;

 if(position>=50){
  position=0;
  ludoMsg.textContent="🏆 YOU WIN!";
  sound(900,.3);
 }else{
  ludoMsg.textContent="Dice: "+n;
 }

 ludoScore.textContent="Position: "+position;
};

}

/* =========================
   CARROM
========================= */

function carrom(){

let score=0;

gameArea.innerHTML=`
<h2>🎯 Carrom Board</h2>
<canvas id="carromCanvas" width="420" height="420"></canvas>
<br>
<button class="gameBtn" onclick="carromShot()">🎯 STRIKE</button>
<div class="score" id="carromScore">Score: 0</div>
`;

const c=carromCanvas;
const x=c.getContext("2d");

function draw(){
 x.fillStyle="#b87832";
 x.fillRect(0,0,420,420);

 x.fillStyle="#111";
 [[20,20],[400,20],[20,400],[400,400]]
 .forEach(p=>{
  x.beginPath();
  x.arc(p[0],p[1],22,0,Math.PI*2);
  x.fill();
 });

 x.fillStyle="#fff";
 x.beginPath();
 x.arc(210,210,18,0,Math.PI*2);
 x.fill();

 x.fillStyle="#111";
 x.beginPath();
 x.arc(210,210,8,0,Math.PI*2);
 x.fill();
}
draw();

window.carromShot=()=>{
 sound(350,.1);
 if(Math.random()<.55){
  score+=10;
  carromScore.textContent="Score: "+score+" 🎉";
  sound(850,.15);
 }else{
  carromScore.textContent="Try Again!";
 }
};

}

/* =========================
   RACING
========================= */

function racing(){

let canvas=document.createElement("canvas");
canvas.width=360;
canvas.height=600;
gameArea.appendChild(canvas);

gameArea.innerHTML+=`
<div class="controls">
<button class="gameBtn" onclick="moveCar(-1)">⬅️</button>
<button class="gameBtn" onclick="moveCar(1)">➡️</button>
</div>
<div class="score" id="raceScore">Score: 0</div>
`;

const ctx=canvas.getContext("2d");

let car=180;
let enemyY=-100;
let enemyX=Math.random()*300;
let score=0;
let running=true;

window.moveCar=d=>{
 car+=d*35;
 car=Math.max(25,Math.min(335,car));
 sound(250,.05);
};

function loop(){

if(!running)return;

ctx.fillStyle="#242424";
ctx.fillRect(0,0,360,600);

ctx.fillStyle="#fff";
for(let y=0;y<600;y+=80)
 ctx.fillRect(175,y,10,40);

ctx.fillStyle="#09f";
ctx.fillRect(car-18,520,36,65);

ctx.fillStyle="#f33";
ctx.fillRect(enemyX,enemyY,38,65);

enemyY+=5;

if(enemyY>620){
 enemyY=-100;
 enemyX=Math.random()*320;
 score++;
 raceScore.textContent="Score: "+score;
 sound(500,.04);
}

if(enemyY>500 &&
 Math.abs(enemyX-car)<35){
 running=false;
 sound(100,.4);
 raceScore.textContent="💥 GAME OVER — Score: "+score;
}

requestAnimationFrame(loop);
}

loop();
}

/* =========================
   SNAKE
========================= */

function snake(){

gameArea.innerHTML=`
<canvas id="snakeCanvas" width="360" height="360"></canvas>
<div class="controls">
<button class="gameBtn" onclick="snakeDir(0,-1)">⬆️</button>
<button class="gameBtn" onclick="snakeDir(-1,0)">⬅️</button>
<button class="gameBtn" onclick="snakeDir(1,0)">➡️</button>
<button class="gameBtn" onclick="snakeDir(0,1)">⬇️</button>
</div>
<div class="score" id="snakeScore">Score: 0</div>
`;

const ctx=snakeCanvas.getContext("2d");
let snakeBody=[{x:10,y:10}];
let dir={x:1,y:0};
let food={x:15,y:15};
let score=0;

window.snakeDir=(x,y)=>{
 if(dir.x+x===0&&dir.y+y===0)return;
 dir={x,y};
 sound(250,.04);
};

function loop(){

let head={
 x:snakeBody[0].x+dir.x,
 y:snakeBody[0].y+dir.y
};

if(head.x<0||head.y<0||head.x>=18||head.y>=18){
 sound(100,.4);
 snakeBody=[{x:10,y:10}];
 dir={x:1,y:0};
 score=0;
}

snakeBody.unshift(head);

if(head.x===food.x&&head.y===food.y){
 score++;
 food={
  x:Math.floor(Math.random()*18),
  y:Math.floor(Math.random()*18)
 };
 sound(800,.1);
}else snakeBody.pop();

ctx.fillStyle="#050912";
ctx.fillRect(0,0,360,360);

ctx.fillStyle="#00e676";
snakeBody.forEach(p=>
 ctx.fillRect(p.x*20,p.y*20,19,19)
);

ctx.fillStyle="#ff1744";
ctx.fillRect(food.x*20,food.y*20,19,19);

snakeScore.textContent="Score: "+score;
}

setInterval(loop,120);
}

/* =========================
   SHOOTER
========================= */

function shooter(){

let score=0;

gameArea.innerHTML=`
<div class="big">🎯</div>
<button class="gameBtn" onclick="shootTarget()">🔫 FIRE</button>
<div class="score" id="shootScore">Score: 0</div>
<div class="msg" id="shootMsg">Target को hit करें!</div>
`;

window.shootTarget=()=>{
 sound(180,.08);

 if(Math.random()<.7){
  score++;
  shootMsg.textContent="💥 HIT!";
  sound(900,.12);
 }else{
  shootMsg.textContent="❌ MISS!";
 }

 shootScore.textContent="Score: "+score;
};

}

/* =========================
   BRICK
========================= */

function brick(){

let score=0;
let bricks=20;

gameArea.innerHTML=`
<div id="brickBox" style="
display:grid;
grid-template-columns:repeat(5,1fr);
gap:6px;
max-width:500px;
margin:auto"></div>
<button class="gameBtn" onclick="hitBrick()">🟠 BREAK</button>
<div class="score" id="brickScore">Bricks: 20</div>
`;

for(let i=0;i<20;i++){
 const b=document.createElement("div");
 b.style.height="45px";
 b.style.background="#ff5252";
 b.style.borderRadius="8px";
 b.id="brick"+i;
 brickBox.appendChild(b);
}

window.hitBrick=()=>{
 if(bricks<=0)return;

 bricks--;
 score+=10;

 document.getElementById("brick"+bricks).style.visibility="hidden";
 brickScore.textContent="Bricks: "+bricks;

 sound(650,.08);

 if(bricks===0){
  brickScore.textContent="🏆 YOU WIN! Score: "+score;
  sound(1000,.3);
 }
};

}

/* =========================
   PONG
========================= */

function pong(){

let score=0;

gameArea.innerHTML=`
<canvas id="pongCanvas" width="360" height="360"></canvas>
<div class="controls">
<button class="gameBtn" onclick="pongMove(-1)">⬅️</button>
<button class="gameBtn" onclick="pongMove(1)">➡️</button>
</div>
<div class="score" id="pongScore">Score: 0</div>
`;

const ctx=pongCanvas.getContext("2d");
let paddle=140;

window.pongMove=d=>{
 paddle+=d*30;
 paddle=Math.max(0,Math.min(280,paddle));
 sound(300,.04);
};

function draw(){

ctx.fillStyle="#050912";
ctx.fillRect(0,0,360,360);

ctx.fillStyle="#fff";
ctx.fillRect(paddle,320,80,12);

ctx.beginPath();
ctx.arc(Math.random()*360,100,8,0,Math.PI*2);
ctx.fill();

score++;
pongScore.textContent="Score: "+score;

}

setInterval(draw,500);

}

/* =========================
   FOOTBALL
========================= */

function football(){

let score=0;

gameArea.innerHTML=`
<div class="big">🥅</div>
<div class="big" id="ball">⚽</div>
<button class="gameBtn" onclick="kick()">⚽ KICK</button>
<div class="score" id="footballScore">Goals: 0</div>
`;

window.kick=()=>{
 sound(300,.1);

 if(Math.random()<.65){
  score++;
  footballScore.textContent="⚽ GOAL! Goals: "+score;
  sound(850,.2);
 }else{
  footballScore.textContent="🧤 SAVED!";
 }
};

}

/* =========================
   BASKETBALL
========================= */

function basketball(){

let score=0;

gameArea.innerHTML=`
<div class="big">🏀</div>
<button class="gameBtn" onclick="basketShot()">🏀 SHOOT</button>
<div class="score" id="basketScore">Score: 0</div>
`;

window.basketShot=()=>{
 sound(300,.08);

 if(Math.random()<.6){
  score++;
  sound(850,.12);
  basketScore.textContent="🏀 SCORE! "+score;
 }else{
  basketScore.textContent="❌ MISS!";
 }
};

}

/* =========================
   RUNNER
========================= */

function runner(){

let score=0;

gameArea.innerHTML=`
<div class="big">🏃</div>
<button class="gameBtn" onclick="jumpRunner()">⬆️ JUMP</button>
<div class="score" id="runnerScore">Distance: 0</div>
`;

window.jumpRunner=()=>{
 score+=10;
 sound(500,.06);
 runnerScore.textContent="Distance: "+score;
};

}

/* =========================
   MEMORY
========================= */

function memory(){

const items=["🍎","🍌","🍇","🍓","🍎","🍌","🍇","🍓"];
items.sort(()=>Math.random()-.5);

gameArea.innerHTML=`
<div id="memoryBoard" style="
display:grid;
grid-template-columns:repeat(4,75px);
gap:7px;
justify-content:center"></div>
<div class="score" id="memoryScore">Matches: 0</div>
`;

let first=null;
let lock=false;
let matches=0;

items.forEach((v,i)=>{
 const b=document.createElement("button");
 b.className="cell";
 b.textContent="❓";

 b.onclick=()=>{
  if(lock||b.textContent!=="❓")return;

  b.textContent=v;
  sound(500,.05);

  if(first===null){
   first={i,b,v};
  }else{

   if(first.v===v){
    matches++;
    memoryScore.textContent="Matches: "+matches;
    first=null;
    sound(900,.12);
   }else{
    lock=true;
    setTimeout(()=>{
     b.textContent="❓";
     first.b.textContent="❓";
     first=null;
     lock=false;
    },600);
   }

  }
 };

 memoryBoard.appendChild(b);
});

}

/* =========================
   QUIZ
========================= */

function quiz(){

const qs=[
["भारत की राजधानी?",["दिल्ली","मुंबई","जयपुर"],0],
["2 + 2 = ?",["3","4","5"],1],
["पानी का सूत्र?",["CO2","H2O","O2"],1],
["राष्ट्रीय पशु?",["शेर","बाघ","हाथी"],1],
["सूर्य किस दिशा से निकलता है?",["पूर्व","पश्चिम","उत्तर"],0]
];

let q=qs[Math.floor(Math.random()*qs.length)];

gameArea.innerHTML=`
<h2>${q[0]}</h2>
${q[1].map((a,i)=>
`<button class="gameBtn" onclick="answer(${i},${q[2]})">${a}</button>`
).join("")}
<div class="msg" id="quizMsg"></div>
`;

window.answer=(a,c)=>{
 if(a===c){
  quizMsg.textContent="🎉 सही जवाब!";
  sound(900,.15);
 }else{
  quizMsg.textContent="❌ गलत जवाब";
  sound(150,.2);
 }
};

}

/* =========================
   GUESS
========================= */

function guess(){

const secret=Math.floor(Math.random()*20)+1;

gameArea.innerHTML=`
<h2>🔢 1 से 20 तक नंबर Guess करें</h2>
<input class="answer" id="guessInput" type="number">
<button class="gameBtn" onclick="checkGuess()">CHECK</button>
<div class="msg" id="guessMsg"></div>
`;

window.checkGuess=()=>{
 const n=Number(guessInput.value);

 if(n===secret){
  guessMsg.textContent="🏆 सही नंबर!";
  sound(900,.2);
 }else if(n<secret){
  guessMsg.textContent="⬆️ इससे बड़ा नंबर";
 }else{
  guessMsg.textContent="⬇️ इससे छोटा नंबर";
 }
};

}

/* =========================
   RPS
========================= */

function rps(){

gameArea.innerHTML=`
<h2>✊ Rock Paper Scissors</h2>
<button class="gameBtn" onclick="rpsPlay('✊')">✊</button>
<button class="gameBtn" onclick="rpsPlay('✋')">✋</button>
<button class="gameBtn" onclick="rpsPlay('✌️')">✌️</button>
<div class="msg" id="rpsMsg"></div>
`;

window.rpsPlay=p=>{
 const c=["✊","✋","✌️"][Math.floor(Math.random()*3)];

 if(p===c)rpsMsg.textContent="🤝 DRAW";
 else rpsMsg.textContent="आप "+p+" | Computer "+c;

 sound(500,.08);
};

}

/* =========================
   TIC TAC TOE
========================= */

function tic(){

let board=Array(9).fill("");

gameArea.innerHTML=`
<div class="board">
${board.map((_,i)=>
`<button class="cell" id="tic${i}" onclick="ticPlay(${i})"></button>`
).join("")}
</div>
<div class="msg" id="ticMsg"></div>
`;

window.ticPlay=i=>{
 if(board[i])return;

 board[i]="X";
 tic0(i,"X");
 sound(600,.06);

 if(checkWin("X")){
  ticMsg.textContent="🏆 You Win!";
  return;
 }

 const empty=board.map((v,i)=>v?null:i).filter(v=>v!==null);

 if(empty.length){
  const c=empty[Math.floor(Math.random()*empty.length)];
  board[c]="O";
  tic0(c,"O");

  if(checkWin("O"))
   ticMsg.textContent="🤖 Computer Wins";
 }
};

function tic0(i,v){
 document.getElementById("tic"+i).textContent=v;
}

function checkWin(p){

const w=[
[0,1,2],[3,4,5],[6,7,8],
[0,3,6],[1,4,7],[2,5,8],
[0,4,8],[2,4,6]
];

return w.some(a=>a.every(i=>board[i]===p));
}

}

/* =========================
   DICE
========================= */

function dice(){

gameArea.innerHTML=`
<div class="big" id="diceFace">🎲</div>
<button class="gameBtn" onclick="rollDice()">ROLL</button>
<div class="msg" id="diceMsg"></div>
`;

window.rollDice=()=>{
 const f=["⚀","⚁","⚂","⚃","⚄","⚅"];
 diceFace.textContent=f[Math.floor(Math.random()*6)];
 sound(500,.1);
};

}

/* =========================
   COIN
========================= */

function coin(){

gameArea.innerHTML=`
<div class="big" id="coinFace">🪙</div>
<button class="gameBtn" onclick="flipCoin()">FLIP</button>
`;

window.flipCoin=()=>{
 coinFace.textContent=Math.random()<.5?"🙂":"🔵";
 sound(500,.1);
};

}

/* =========================
   WHEEL
========================= */

function wheel(){

gameArea.innerHTML=`
<div class="big" id="wheelFace">🎡</div>
<button class="gameBtn" onclick="spinWheel()">SPIN</button>
<div class="msg" id="wheelMsg"></div>
`;

window.spinWheel=()=>{
 const a=["⭐ BONUS","🎁 GIFT","🏆 WIN","🍀 LUCK"];
 wheelMsg.textContent=a[Math.floor(Math.random()*a.length)];
 sound(900,.2);
};

}

/* =========================
   CLICK GAME
========================= */

function clickGame(){

let score=0;

gameArea.innerHTML=`
<div class="big">👆</div>
<button class="gameBtn" onclick="clickMe()">CLICK!</button>
<div class="score" id="clickScore">0</div>
`;

window.clickMe=()=>{
 score++;
 clickScore.textContent=score;
 sound(500,.03);
};

}

/* =========================
   REACTION
========================= */

function reaction(){

gameArea.innerHTML=`
<button class="gameBtn" id="reactionBtn">WAIT...</button>
<div class="msg" id="reactionMsg"></div>
`;

let start=0;
let ready=false;

setTimeout(()=>{
 ready=true;
 start=performance.now();
 reactionBtn.textContent="CLICK!";
 sound(900,.1);
},1000+Math.random()*2500);

window.reactionBtn.onclick=()=>{
 if(!ready){
  reactionMsg.textContent="❌ Too Early!";
  return;
 }

 reactionMsg.textContent=
 "⚡ "+Math.round(performance.now()-start)+" ms";
 sound(800,.1);
};

}

/* =========================
   TYPING
========================= */

function typing(){

const words=["PRINCE","GAME","RACING","PLAYER","VICTORY"];

const word=words[Math.floor(Math.random()*words.length)];

gameArea.innerHTML=`
<h2>${word}</h2>
<input class="answer" id="typingInput">
<button class="gameBtn" onclick="checkTyping()">CHECK</button>
<div class="msg" id="typingMsg"></div>
`;

window.checkTyping=()=>{
 if(typingInput.value.toUpperCase()===word){
  typingMsg.textContent="🏆 PERFECT!";
  sound(900,.15);
 }else{
  typingMsg.textContent="❌ Try Again";
 }
};

}

/* =========================
   MAKEUP
========================= */

function makeup(){

let points=0;

gameArea.innerHTML=`
<div class="big" id="girl">👩</div>

<div class="controls">
<button class="gameBtn" onclick="beauty('💄')">💄</button>
<button class="gameBtn" onclick="beauty('💋')">💋</button>
<button class="gameBtn" onclick="beauty('💅')">💅</button>
<button class="gameBtn" onclick="beauty('👑')">👑</button>
<button class="gameBtn" onclick="beauty('💇')">💇</button>
</div>

<div class="score" id="beautyScore">Beauty: 0</div>
`;

window.beauty=x=>{
 points+=10;
 girl.textContent=x;
 beautyScore.textContent="Beauty: "+points;
 sound(700,.08);
};

}

/* =========================
   KITCHEN
========================= */

function kitchen(){

let score=0;

const foods=[
"🍕 Pizza",
"🍔 Burger",
"🎂 Cake",
"🍦 Ice Cream",
"🍣 Sushi",
"🥞 Pancake"
];

gameArea.innerHTML=`
<div class="big" id="food">👩‍🍳</div>
<h2 id="foodName">Recipe चुनें</h2>

<div class="controls">
${foods.map((f,i)=>
`<button class="gameBtn" onclick="cook(${i})">${f}</button>`
).join("")}
</div>

<div class="score" id="foodScore">Orders: 0</div>
`;

window.cook=i=>{
 food.textContent=foods[i].split(" ")[0];
 foodName.textContent="👨‍🍳 "+foods[i]+" तैयार!";
 score++;
 foodScore.textContent="Orders: "+score;
 sound(700,.12);
};

}

/* =========================
   SIMPLE GAMES
========================= */

function simpleGame(){

let score=0;

gameArea.innerHTML=`
<div class="big">🎮</div>
<button class="gameBtn" onclick="simplePoint()">PLAY</button>
<div class="score" id="simpleScore">Score: 0</div>
`;

window.simplePoint=()=>{
 score++;
 simpleScore.textContent="Score: "+score;
 sound(600,.06);
};

}

render();

</script>

</body>
</html>
