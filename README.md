<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Prince Game Zone - 100 Games</title>

<style>
*{box-sizing:border-box}
body{
 margin:0;font-family:Arial,sans-serif;
 background:#0d1020;color:#fff;
}
header{
 padding:25px 15px;text-align:center;
 background:linear-gradient(135deg,#6a11cb,#2575fc);
}
header h1{margin:0;font-size:30px}
header p{margin:8px 0}
.wrap{max-width:1100px;margin:auto;padding:16px}
#search{
 width:100%;padding:16px;border:0;border-radius:15px;
 font-size:17px;outline:0;margin-bottom:16px
}
.filters{display:flex;gap:8px;overflow:auto;margin-bottom:18px}
.filters button{
 border:0;border-radius:20px;padding:10px 16px;
 white-space:nowrap;background:#292d48;color:#fff
}
.filters button.active{background:#7c3cff}
.grid{
 display:grid;
 grid-template-columns:repeat(auto-fit,minmax(210px,1fr));
 gap:15px
}
.card{
 background:#191d32;border-radius:18px;padding:18px;
 text-align:center;box-shadow:0 5px 18px #0005
}
.icon{font-size:48px}
.card h2{font-size:19px;margin:8px 0}
.card p{color:#aaa;font-size:14px;min-height:35px}
.play,.mainbtn{
 border:0;border-radius:12px;padding:12px 18px;
 background:#2879ff;color:white;font-size:16px;cursor:pointer
}
.play{width:100%}
#screen{
 display:none;background:#191d32;border-radius:20px;
 padding:20px;text-align:center;max-width:650px;margin:20px auto
}
.back{background:#34384f;border:0;color:#fff;padding:10px 16px;
 border-radius:10px;cursor:pointer;margin-bottom:15px}
#gameTitle{font-size:25px}
#gameArea{min-height:250px;padding:10px}
.big{font-size:30px;margin:15px}
.choice{
 padding:14px 20px;margin:5px;border:0;border-radius:12px;
 background:#303653;color:#fff;font-size:18px;cursor:pointer
}
.choice:hover{background:#5149a8}
input.gameinput{
 padding:14px;border:0;border-radius:10px;
 font-size:18px;max-width:280px;width:90%
}
.score{font-size:22px;color:#ffd54f;margin:15px}
.msg{font-size:20px;margin:15px;min-height:30px}
footer{text-align:center;padding:30px;color:#888}
</style>
</head>

<body>

<header>
<h1>🎮 Prince Game Zone</h1>
<p>🔥 100 Games • Search • Play • Have Fun</p>
</header>

<div class="wrap">

<input id="search" placeholder="🔎 Search Games...">

<div class="filters" id="filters"></div>

<div id="games" class="grid"></div>

<div id="screen">
<button class="back" onclick="home()">← सभी गेम</button>
<h2 id="gameTitle"></h2>
<div id="gameArea"></div>
</div>

</div>

<footer>🎮 Prince Game Zone • 100 Games</footer>

<script>

/* =========================
   100 GAMES
========================= */

const games=[
["Snake Classic","🐍","Arcade","snake"],
["Snake Speed","🐍","Arcade","snake"],
["Snake Master","🐍","Arcade","snake"],
["Snake Challenge","🐍","Arcade","snake"],
["Number Guess","🔢","Puzzle","guess"],
["Secret Number","🔐","Puzzle","guess"],
["Lucky Number","🍀","Puzzle","guess"],
["Guess 1-100","🎯","Puzzle","guess"],
["Math Plus","➕","Math","math"],
["Math Minus","➖","Math","math"],
["Math Multiply","✖️","Math","math"],
["Math Challenge","🧠","Math","math"],
["Fast Math","⚡","Math","math"],
["Mental Math","🧮","Math","math"],
["Easy Quiz","❓","Quiz","quiz"],
["General Quiz","🌎","Quiz","quiz"],
["India Quiz","🇮🇳","Quiz","quiz"],
["Hindi Quiz","📚","Quiz","quiz"],
["Science Quiz","🔬","Quiz","quiz"],
["Sports Quiz","⚽","Quiz","quiz"],
["Animal Quiz","🐯","Quiz","quiz"],
["Space Quiz","🚀","Quiz","quiz"],
["History Quiz","🏛️","Quiz","quiz"],
["Computer Quiz","💻","Quiz","quiz"],
["Reaction Test","⚡","Skill","reaction"],
["Fast Reaction","⚡","Skill","reaction"],
["Super Reaction","🔥","Skill","reaction"],
["Click Challenge","👆","Skill","click"],
["Click Master","👆","Skill","click"],
["10 Second Click","⏱️","Skill","click"],
["Speed Click","💨","Skill","click"],
["Rock Paper Scissors","✊","Classic","rps"],
["RPS Master","✌️","Classic","rps"],
["RPS Battle","⚔️","Classic","rps"],
["Odd or Even","🔢","Puzzle","odd"],
["Odd Even Master","🧠","Puzzle","odd"],
["Lucky Odd Even","🍀","Puzzle","odd"],
["Color Challenge","🎨","Skill","color"],
["Color Master","🌈","Skill","color"],
["Color Rush","🎨","Skill","color"],
["Memory Cards","🃏","Memory","memory"],
["Memory Master","🧠","Memory","memory"],
["Memory Match","🎴","Memory","memory"],
["Emoji Memory","😀","Memory","memory"],
["Quick Memory","⚡","Memory","memory"],
["Typing Test","⌨️","Skill","typing"],
["Fast Typing","💨","Skill","typing"],
["Typing Master","🏆","Skill","typing"],
["Word Challenge","🔤","Word","word"],
["Word Master","📖","Word","word"],
["Word Guess","🔠","Word","word"],
["Random Word","🎲","Word","word"],
["Tic Tac Toe","⭕","Classic","tic"],
["Tic Tac Toe Pro","⭕","Classic","tic"],
["Tic Tac Toe X","❌","Classic","tic"],
["Car Dodge","🚗","Arcade","car"],
["Car Highway","🏎️","Arcade","car"],
["Traffic Dodge","🚦","Arcade","car"],
["Road Racer","🏁","Racing","car"],
["Space Dodge","🚀","Arcade","space"],
["Space Runner","🌌","Arcade","space"],
["Rocket Challenge","🚀","Arcade","space"],
["Catch The Ball","⚽","Arcade","catch"],
["Catch Stars","⭐","Arcade","catch"],
["Catch Fruits","🍎","Arcade","catch"],
["Quick Tap","👆","Skill","tap"],
["Super Tap","🔥","Skill","tap"],
["Tap Master","🏆","Skill","tap"],
["Dice Roll","🎲","Luck","dice"],
["Lucky Dice","🎲","Luck","dice"],
["Dice Master","🎲","Luck","dice"],
["Coin Flip","🪙","Luck","coin"],
["Lucky Coin","🪙","Luck","coin"],
["Heads Or Tails","🪙","Luck","coin"],
["Higher Lower","📈","Puzzle","higher"],
["High Low Master","🎯","Puzzle","higher"],
["Card Guess","🃏","Puzzle","higher"],
["Quick Count","🔢","Math","count"],
["Count Master","🔢","Math","count"],
["Number Rush","⚡","Math","count"],
["Pattern Test","🔷","Puzzle","pattern"],
["Pattern Master","🧩","Puzzle","pattern"],
["Sequence Game","🔢","Puzzle","pattern"],
["True False","✅","Quiz","truefalse"],
["True False Pro","❌","Quiz","truefalse"],
["Fact Challenge","💡","Quiz","truefalse"],
["Emoji Quiz","😀","Quiz","emoji"],
["Emoji Master","🤩","Quiz","emoji"],
["Guess Emoji","🎯","Quiz","emoji"],
["Animal Match","🐶","Memory","match"],
["Fruit Match","🍎","Memory","match"],
["Emoji Match","😀","Memory","match"],
["Color Match","🎨","Memory","match"],
["Quick Choice","🎯","Skill","choice"],
["Lucky Choice","🍀","Skill","choice"],
["Fast Choice","⚡","Skill","choice"],
["Brain Challenge","🧠","Puzzle","brain"],
["Brain Master","🧠","Puzzle","brain"],
["Ultimate Brain","🔥","Puzzle","brain"],
["Final Challenge","🏆","Challenge","final"]
];

/* =========================
   GAME LIST
========================= */

const gameBox=document.getElementById("games");
const search=document.getElementById("search");
const filters=document.getElementById("filters");

let currentCategory="All";

const categories=["All",...new Set(games.map(g=>g[2]))];

categories.forEach((c,i)=>{
 let b=document.createElement("button");
 b.innerText=c==="All"?"🎮 सभी":c;
 if(i===0)b.className="active";
 b.onclick=()=>{
  currentCategory=c;
  document.querySelectorAll(".filters button")
  .forEach(x=>x.classList.remove("active"));
  b.classList.add("active");
  showGames();
 };
 filters.appendChild(b);
});

function showGames(){

 let text=search.value.toLowerCase();

 let result=games.filter(g=>
  (currentCategory==="All"||g[2]===currentCategory)&&
  (g[0].toLowerCase().includes(text)||
   g[2].toLowerCase().includes(text))
 );

 gameBox.innerHTML="";

 if(!result.length){
  gameBox.innerHTML=
  "<div class='msg'>😔 कोई गेम नहीं मिला</div>";
  return;
 }

 result.forEach((g)=>{
  let index=games.indexOf(g);

  gameBox.innerHTML+=`
  <div class="card">
   <div class="icon">${g[1]}</div>
   <h2>${g[0]}</h2>
   <p>${g[2]} Game</p>
   <button class="play"
   onclick="openGame(${index})">▶ PLAY</button>
  </div>`;
 });
}

search.addEventListener("input",showGames);

/* =========================
   OPEN / HOME
========================= */

function openGame(index){

 let g=games[index];

 document.getElementById("games").style.display="none";
 document.getElementById("filters").style.display="none";
 document.getElementById("search").style.display="none";
 document.getElementById("screen").style.display="block";

 document.getElementById("gameTitle").innerText=
 g[1]+" "+g[0];

 startGame(g[3]);
 window.scrollTo(0,0);
}

function home(){

 document.getElementById("screen").style.display="none";
 document.getElementById("games").style.display="grid";
 document.getElementById("filters").style.display="flex";
 document.getElementById("search").style.display="block";

 showGames();
}

/* =========================
   GAME AREA
========================= */

const area=document.getElementById("gameArea");

function msg(t){
 return `<div class="msg">${t}</div>`;
}

/* =========================
   START GAME
========================= */

function startGame(type){

 area.innerHTML="";

 if(type==="guess")guessGame();
 else if(type==="math")mathGame();
 else if(type==="quiz")quizGame();
 else if(type==="reaction")reactionGame();
 else if(type==="click")clickGame();
 else if(type==="rps")rpsGame();
 else if(type==="odd")oddGame();
 else if(type==="color")colorGame();
 else if(type==="memory")memoryGame();
 else if(type==="typing")typingGame();
 else if(type==="word")wordGame();
 else if(type==="tic")ticGame();
 else if(type==="car")carGame();
 else if(type==="space")spaceGame();
 else if(type==="catch")catchGame();
 else if(type==="tap")tapGame();
 else if(type==="dice")diceGame();
 else if(type==="coin")coinGame();
 else if(type==="higher")higherGame();
 else if(type==="count")countGame();
 else if(type==="pattern")patternGame();
 else if(type==="truefalse")trueFalseGame();
 else if(type==="emoji")emojiGame();
 else if(type==="match")matchGame();
 else if(type==="choice")choiceGame();
 else if(type==="brain")brainGame();
 else if(type==="final")finalGame();
 else snakeGame();
}

/* =========================
   GUESS
========================= */

function guessGame(){

 let n=Math.floor(Math.random()*100)+1;

 area.innerHTML=`
 <div class="big">🔢</div>
 <p>1 से 100 के बीच छिपा नंबर खोजें।</p>
 <input class="gameinput" id="gi" type="number">
 <br><br>
 <button class="mainbtn" onclick="checkGuess()">Check</button>
 <div id="gm" class="msg"></div>`;

 window.checkGuess=function(){

  let v=Number(document.getElementById("gi").value);
  let m=document.getElementById("gm");

  if(v===n)m.innerHTML="🎉 सही जवाब!";
  else if(v<n)m.innerHTML="⬆️ और बड़ा नंबर";
  else m.innerHTML="⬇️ और छोटा नंबर";
 };
}

/* =========================
   MATH
========================= */

function mathGame(){

 let a=Math.floor(Math.random()*30)+1;
 let b=Math.floor(Math.random()*30)+1;
 let op=Math.floor(Math.random()*3);
 let ans;

 if(op===0)ans=a+b;
 if(op===1){
  if(b>a)[a,b]=[b,a];
  ans=a-b;
 }
 if(op===2)ans=a*b;

 let symbol=op===0?"+":op===1?"−":"×";

 area.innerHTML=`
 <div class="big">🧠</div>
 <h2>${a} ${symbol} ${b} = ?</h2>
 <input class="gameinput" id="mi" type="number">
 <br><br>
 <button class="mainbtn" onclick="mathCheck()">उत्तर</button>
 <div id="mm" class="msg"></div>`;

 window.mathCheck=function(){
  let v=Number(document.getElementById("mi").value);
  document.getElementById("mm").innerHTML=
   v===ans?"🎉 बिल्कुल सही!":"❌ गलत! सही उत्तर: "+ans;
 };
}

/* =========================
   QUIZ
========================= */

const quizData=[
["भारत की राजधानी क्या है?",["दिल्ली","जयपुर","मुंबई","पटना"],0],
["भारत का राष्ट्रीय पशु?",["शेर","बाघ","हाथी","घोड़ा"],1],
["भारत का राष्ट्रीय पक्षी?",["मोर","तोता","कबूतर","हंस"],0],
["पानी का रासायनिक सूत्र?",["CO2","H2O","O2","NaCl"],1],
["सूर्य किस दिशा से निकलता है?",["पश्चिम","उत्तर","पूर्व","दक्षिण"],2],
["एक सप्ताह में कितने दिन?",["5","6","7","8"],2],
["हिंदी दिवस कब?",["14 सितंबर","15 अगस्त","26 जनवरी","2 अक्टूबर"],0],
["कंप्यूटर का दिमाग किसे कहते हैं?",["RAM","CPU","Mouse","Keyboard"],1],
["सबसे बड़ा ग्रह?",["पृथ्वी","मंगल","बृहस्पति","शुक्र"],2],
["पृथ्वी का उपग्रह?",["सूर्य","चंद्रमा","मंगल","शुक्र"],1]
];

function quizGame(){

 let q=quizData[Math.floor(Math.random()*quizData.length)];

 area.innerHTML=`
 <h3>${q[0]}</h3>
 ${q[1].map((x,i)=>
 `<button class="choice" onclick="quizAnswer(${i},${q[2]})">${x}</button>`
 ).join("")}
 <div id="qm" class="msg"></div>`;
}

window.quizAnswer=function(i,a){
 document.getElementById("qm").innerHTML=
 i===a?"🎉 सही जवाब!":"❌ गलत जवाब!";
};

/* =========================
 REACTION
========================= */

function reactionGame(){

 area.innerHTML=`
 <div class="big">⚡</div>
 <p>हरा होने पर तुरंत बटन दबाएँ!</p>
 <button id="react" class="mainbtn"
 onclick="reactClick()">WAIT...</button>
 <div id="rm" class="msg"></div>`;

 let start;
 let ready=false;

 let delay=1000+Math.random()*3000;

 setTimeout(()=>{
  ready=true;
  start=Date.now();
  document.getElementById("react").innerText="CLICK!";
 },delay);

 window.reactClick=function(){

  if(!ready){
   document.getElementById("rm").innerText="😅 बहुत जल्दी!";
   return;
  }

  let t=Date.now()-start;
  document.getElementById("rm").innerText=
  "⚡ Reaction: "+t+" ms";
 };
}

/* =========================
 CLICK
========================= */

function clickGame(){

 let count=0,time=10,run=false;

 area.innerHTML=`
 <div class="big">👆</div>
 <p>10 सेकंड में जितने क्लिक कर सकें!</p>
 <div class="score" id="cs">0</div>
 <button id="cb" class="mainbtn">CLICK!</button>
 <div id="ct">10</div>`;

 document.getElementById("cb").onclick=()=>{

  if(!run){
   run=true;
   let timer=setInterval(()=>{
    time--;
    document.getElementById("ct").innerText=time;

    if(time<=0){
     clearInterval(timer);
     document.getElementById("cb").disabled=true;
     document.getElementById("ct").innerText=
     "🏆 Score: "+count;
    }
   },1000);
  }

  if(time>0){
   count++;
   document.getElementById("cs").innerText=count;
  }
 };
}

/* =========================
 RPS
========================= */

function rpsGame(){

 area.innerHTML=`
 <p>अपना चुनाव करें:</p>
 <button class="choice" onclick="rps('✊')">✊</button>
 <button class="choice" onclick="rps('✋')">✋</button>
 <button class="choice" onclick="rps('✌️')">✌️</button>
 <div id="rpsm" class="msg"></div>`;

 window.rps=function(p){

  let c=["✊","✋","✌️"][Math.floor(Math.random()*3)];

  let win=
  (p==="✊"&&c==="✌️")||
  (p==="✋"&&c==="✊")||
  (p==="✌️"&&c==="✋");

  let r=p===c?"🤝 Draw!":win?"🎉 आप जीत गए!":"😅 Computer जीता!";

  document.getElementById("rpsm").innerHTML=
  `आप ${p} | Computer ${c}<br>${r}`;
 };
}

/* =========================
 ODD EVEN
========================= */

function oddGame(){

 let n=Math.floor(Math.random()*100)+1;
 let correct=n%2===0?"सम":"विषम";

 area.innerHTML=`
 <div class="big">${n}</div>
 <button class="choice" onclick="oddCheck('सम')">EVEN / सम</button>
 <button class="choice" onclick="oddCheck('विषम')">ODD / विषम</button>
 <div id="om" class="msg"></div>`;

 window.oddCheck=function(x){
  document.getElementById("om").innerText=
  x===correct?"🎉 सही!":"❌ गलत! सही: "+correct;
 };
}

/* =========================
 COLOR
========================= */

function colorGame(){

 const colors=[
 ["🔴","लाल"],["🔵","नीला"],["🟢","हरा"],
 ["🟡","पीला"],["🟣","बैंगनी"]
 ];

 let x=colors[Math.floor(Math.random()*colors.length)];

 area.innerHTML=`
 <div class="big">${x[0]}</div>
 <p>इस रंग का नाम चुनें:</p>
 ${colors.map(c=>
 `<button class="choice" onclick="colorCheck('${c[1]}','${x[1]}')">${c[1]}</button>`
 ).join("")}
 <div id="colorm" class="msg"></div>`;

 window.colorCheck=function(a,b){
  document.getElementById("colorm").innerText=
  a===b?"🎉 सही!":"❌ गलत!";
 };
}

/* =========================
 MEMORY
========================= */

function memoryGame(){

 let arr=["🍎","🍌","🍇","🍓","🍉","🥝"];
 arr=[...arr,...arr].sort(()=>Math.random()-.5);

 let html=`<p>एक जैसे दो कार्ड खोजें</p>
 <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:7px">`;

 arr.forEach((x,i)=>{
  html+=`<button id="mc${i}" class="choice"
  onclick="memoryFlip(${i})">❓</button>`;
 });

 html+=`</div><div id="mem" class="msg"></div>`;

 area.innerHTML=html;

 let first=null,lock=false;

 window.memoryFlip=function(i){

  if(lock)return;

  let b=document.getElementById("mc"+i);

  if(b.innerText!=="❓")return;

  b.innerText=arr[i];

  if(first===null){
   first=i;
   return;
  }

  let second=i;
  lock=true;

  if(arr[first]===arr[second]){
   b.disabled=true;
   document.getElementById("mc"+first).disabled=true;
   first=null;lock=false;
  }else{
   setTimeout(()=>{
    b.innerText="❓";
    document.getElementById("mc"+first).innerText="❓";
    first=null;lock=false;
   },600);
  }
 };
}

/* =========================
 TYPING
========================= */

function typingGame(){

 let words=[
 "यीशु","भारत","प्रेम","खेल","सफलता",
 "दोस्ती","परिवार","आशा","विश्वास","जीवन"
 ];

 let word=words[Math.floor(Math.random()*words.length)];

 area.innerHTML=`
 <h2>यह शब्द लिखें:</h2>
 <div class="big">${word}</div>
 <input class="gameinput" id="ti">
 <br><br>
 <button class="mainbtn" onclick="typeCheck()">Check</button>
 <div id="tm" class="msg"></div>`;

 window.typeCheck=function(){
  let v=document.getElementById("ti").value;
  document.getElementById("tm").innerText=
  v===word?"🎉 सही लिखा!":"❌ फिर कोशिश करें";
 };
}

/* =========================
 WORD
========================= */

function wordGame(){

 let words=["कमल","भारत","विद्यालय","सूरज","मित्र","खेल","पानी","पुस्तक"];

 let word=words[Math.floor(Math.random()*words.length)];

 area.innerHTML=`
 <p>इस शब्द को देखकर याद करें और नीचे लिखें:</p>
 <div class="big">${word}</div>
 <input class="gameinput" id="wi">
 <br><br>
 <button class="mainbtn" onclick="wordCheck()">जवाब</button>
 <div id="wm" class="msg"></div>`;

 window.wordCheck=function(){
  document.getElementById("wm").innerText=
  document.getElementById("wi").value===word?
  "🎉 सही!":"❌ गलत!";
 };
}

/* =========================
 TIC TAC TOE
========================= */

function ticGame(){

 let b=Array(9).fill("");

 area.innerHTML=`
 <div style="display:grid;grid-template-columns:repeat(3,1fr);max-width:300px;margin:auto;gap:5px">
 ${b.map((_,i)=>
 `<button class="choice" id="tt${i}"
 onclick="tic(${i})" style="height:80px"></button>`).join("")}
 </div>
 <div id="ticm" class="msg"></div>`;

 window.tic=function(i){

  if(b[i])return;

  b[i]="X";
  document.getElementById("tt"+i).innerText="X";

  if(checkWin(b,"X")){
   document.getElementById("ticm").innerText="🎉 आप जीत गए!";
   return;
  }

  let empty=b.map((x,i)=>x?null:i).filter(x=>x!==null);

  if(!empty.length)return;

  let c=empty[Math.floor(Math.random()*empty.length)];

  b[c]="O";
  document.getElementById("tt"+c).innerText="O";

  if(checkWin(b,"O"))
   document.getElementById("ticm").innerText="🤖 Computer जीत गया!";
 };
}

function checkWin(b,p){

 return [
 [0,1,2],[3,4,5],[6,7,8],
 [0,3,6],[1,4,7],[2,5,8],
 [0,4,8],[2,4,6]
 ].some(x=>x.every(i=>b[i]===p));
}

/* =========================
 CAR
========================= */

function carGame(){

 area.innerHTML=`
 <div style="font-size:50px">🚗</div>
 <p>⬅️ और ➡️ दबाकर कार बचाएँ</p>
 <button class="choice" onclick="carMove('left')">⬅️</button>
 <button class="choice" onclick="carMove('right')">➡️</button>
 <div id="carm" class="msg">🚗 रास्ता साफ है!</div>`;

 let pos=2;

 window.carMove=function(d){

  if(d==="left")pos--;
  else pos++;

  if(pos<0)pos=0;
  if(pos>4)pos=4;

  let crash=Math.random()<.18;

  document.getElementById("carm").innerText=
  crash?"💥 CRASH! फिर खेलें!":
  "🚗 Position: "+(pos+1);
 };
}

/* =========================
 SPACE
========================= */

function spaceGame(){

 let score=0;

 area.innerHTML=`
 <div class="big">🚀</div>
 <p>आसमान से आने वाले asteroid से बचें!</p>
 <button class="choice" onclick="spaceMove('left')">⬅️</button>
 <button class="choice" onclick="spaceMove('right')">➡️</button>
 <div id="spm" class="msg">Score: 0</div>`;

 window.spaceMove=function(){
  if(Math.random()<.15){
   document.getElementById("spm").innerText=
   "💥 Game Over! Score: "+score;
   score=0;
  }else{
   score++;
   document.getElementById("spm").innerText=
   "🚀 बच गए! Score: "+score;
  }
 };
}

/* =========================
 CATCH
========================= */

function catchGame(){

 let score=0;

 area.innerHTML=`
 <div class="big">🎯</div>
 <p>सही चीज पकड़ें!</p>
 <button class="choice" onclick="catchIt('🍎')">🍎</button>
 <button class="choice" onclick="catchIt('💣')">💣</button>
 <button class="choice" onclick="catchIt('⭐')">⭐</button>
 <div id="cam" class="msg"></div>`;

 window.catchIt=function(x){

  if(x==="💣"){
   score=0;
   document.getElementById("cam").innerText="💥 Bomb! Score reset";
  }else{
   score++;
   document.getElementById("cam").innerText="🎉 Score: "+score;
  }
 };
}

/* =========================
 TAP
========================= */

function tapGame(){

 let score=0;

 area.innerHTML=`
 <div class="big">👆</div>
 <button class="mainbtn" onclick="tapIt()">TAP!</button>
 <div id="tapm" class="score">0</div>`;

 window.tapIt=function(){
  score++;
  document.getElementById("tapm").innerText=score;
 };
}

/* =========================
 DICE
========================= */

function diceGame(){

 area.innerHTML=`
 <div id="dice" class="big">🎲</div>
 <button class="mainbtn" onclick="rollDice()">ROLL</button>
 <div id="dm" class="msg"></div>`;

 window.rollDice=function(){

  let n=Math.floor(Math.random()*6)+1;

  document.getElementById("dice").innerText=
  ["⚀","⚁","⚂","⚃","⚄","⚅"][n-1];

  document.getElementById("dm").innerText=
  "आपका नंबर: "+n;
 };
}

/* =========================
 COIN
========================= */

function coinGame(){

 area.innerHTML=`
 <div id="coin" class="big">🪙</div>
 <button class="mainbtn" onclick="flipCoin()">FLIP</button>
 <div id="cm" class="msg"></div>`;

 window.flipCoin=function(){

  let x=Math.random()<.5?"HEAD":"TAIL";

  document.getElementById("cm").innerText=
  x==="HEAD"?"😀 HEADS":"🔄 TAILS";
 };
}

/* =========================
 HIGHER LOWER
========================= */

function higherGame(){

 let n=Math.floor(Math.random()*50)+1;

 area.innerHTML=`
 <div class="big">${n}</div>
 <p>अगला नंबर Higher होगा या Lower?</p>
 <button class="choice" onclick="highLow('high')">⬆️ Higher</button>
 <button class="choice" onclick="highLow('low')">⬇️ Lower</button>
 <div id="hlm" class="msg"></div>`;

 window.highLow=function(x){

  let next=Math.floor(Math.random()*50)+1;

  let correct=next>n?"high":"low";

  document.getElementById("hlm").innerText=
  x===correct?
  "🎉 सही! अगला नंबर "+next:
  "❌ गलत! अगला नंबर "+next;
 };
}

/* =========================
 COUNT
========================= */

function countGame(){

 let a=Math.floor(Math.random()*20)+5;
 let b=Math.floor(Math.random()*10)+1;
 let ans=a+b;

 area.innerHTML=`
 <h2>${a} + ${b}</h2>
 <input class="gameinput" id="co">
 <br><br>
 <button class="mainbtn" onclick="countCheck()">Check</button>
 <div id="com" class="msg"></div>`;

 window.countCheck=function(){
  document.getElementById("com").innerText=
  Number(document.getElementById("co").value)===ans?
  "🎉 सही!":"❌ गलत!";
 };
}

/* =========================
 PATTERN
========================= */

function patternGame(){

 let start=Math.floor(Math.random()*5)+1;
 let seq=[start,start+2,start+4];

 area.innerHTML=`
 <h2>${seq.join(" , ")} , ?</h2>
 <input class="gameinput" id="pa">
 <br><br>
 <button class="mainbtn" onclick="patternCheck()">उत्तर</button>
 <div id="pam" class="msg"></div>`;

 window.patternCheck=function(){
  document.getElementById("pam").innerText=
  Number(document.getElementById("pa").value)===start+6?
  "🎉 सही!":"❌ गलत! सही उत्तर: "+(start+6);
 };
}

/* =========================
 TRUE FALSE
========================= */

function trueFalseGame(){

 let data=[
 ["सूर्य एक तारा है",true],
 ["पृथ्वी सपाट है",false],
 ["भारत एशिया में है",true],
 ["पानी गैस है",false],
 ["मोर भारत का राष्ट्रीय पक्षी है",true]
 ];

 let q=data[Math.floor(Math.random()*data.length)];

 area.innerHTML=`
 <h2>${q[0]}</h2>
 <button class="choice" onclick="tf(true)">✅ सही</button>
 <button class="choice" onclick="tf(false)">❌ गलत</button>
 <div id="tfm" class="msg"></div>`;

 window.tf=function(x){
  document.getElementById("tfm").innerText=
  x===q[1]?"🎉 सही!":"❌ गलत!";
 };
}

/* =========================
 EMOJI
========================= */

function emojiGame(){

 let data=[
 ["🐶","कुत्ता"],["🐱","बिल्ली"],
 ["🍎","सेब"],["🚗","कार"],
 ["🌞","सूरज"],["🐘","हाथी"]
 ];

 let q=data[Math.floor(Math.random()*data.length)];

 let options=[q[1],"घर","पानी","पेड़"]
 .sort(()=>Math.random()-.5);

 area.innerHTML=`
 <div class="big">${q[0]}</div>
 ${options.map(x=>
 `<button class="choice" onclick="emojiCheck('${x}','${q[1]}')">${x}</button>`
 ).join("")}
 <div id="em" class="msg"></div>`;

 window.emojiCheck=function(a,b){
  document.getElementById("em").innerText=
  a===b?"🎉 सही!":"❌ गलत!";
 };
}

/* =========================
 MATCH
========================= */

function matchGame(){

 let data=[
 ["🐶","🐕"],["🐱","🐈"],
 ["🍎","🍏"],["⭐","🌟"]
 ];

 let q=data[Math.floor(Math.random()*data.length)];

 area.innerHTML=`
 <div class="big">${q[0]}</div>
 <p>इससे मिलता-जुलता चिन्ह चुनें</p>
 <button class="choice" onclick="matchCheck('🐕')">🐕</button>
 <button class="choice" onclick="matchCheck('🌟')">🌟</button>
 <button class="choice" onclick="matchCheck('🍏')">🍏</button>
 <button class="choice" onclick="matchCheck('🐈')">🐈</button>
 <div id="mam" class="msg"></div>`;

 window.matchCheck=function(x){
  document.getElementById("mam").innerText=
  x===q[1]?"🎉 Match!":"❌ Try Again";
 };
}

/* =========================
 CHOICE
========================= */

function choiceGame(){

 let n=Math.floor(Math.random()*4)+1;

 area.innerHTML=`
 <p>1 से 4 में Lucky Number चुनें</p>
 ${[1,2,3,4].map(x=>
 `<button class="choice" onclick="choiceCheck(${x})">${x}</button>`
 ).join("")}
 <div id="chm" class="msg"></div>`;

 window.choiceCheck=function(x){
  document.getElementById("chm").innerText=
  x===n?"🍀 Lucky! सही नंबर!":
  "❌ नहीं! Lucky नंबर था "+n;
 };
}

/* =========================
 BRAIN
========================= */

function brainGame(){

 let a=Math.floor(Math.random()*9)+1;
 let b=Math.floor(Math.random()*9)+1;
 let c=a+b;

 area.innerHTML=`
 <h2>🧠 Brain Challenge</h2>
 <p>${a} + ${b} = ?</p>
 <button class="choice" onclick="brain(${c-1})">${c-1}</button>
 <button class="choice" onclick="brain(${c})">${c}</button>
 <button class="choice" onclick="brain(${c+1})">${c+1}</button>
 <div id="bm" class="msg"></div>`;

 window.brain=function(x){
  document.getElementById("bm").innerText=
  x===c?"🧠 शानदार!":"❌ गलत!";
 };
}

/* =========================
 FINAL
========================= */

function finalGame(){

 let score=0;

 area.innerHTML=`
 <h2>🏆 Ultimate Final Challenge</h2>
 <p>हर सही जवाब पर 10 अंक!</p>
 <div id="fq"></div>
 <div id="fs" class="score">Score: 0</div>`;

 let q=0;

 function next(){

  q++;

  if(q>5){
   document.getElementById("fq").innerHTML=
   "🎉 GAME COMPLETE!";
   return;
  }

  let a=Math.floor(Math.random()*10)+1;
  let b=Math.floor(Math.random()*10)+1;
  let ans=a+b;

  document.getElementById("fq").innerHTML=`
  <h2>${a} + ${b} = ?</h2>
  <button class="choice" onclick="finalAnswer(${ans},${ans})">${ans}</button>
  <button class="choice" onclick="finalAnswer(${ans},${ans+1})">${ans+1}</button>
  <button class="choice" onclick="finalAnswer(${ans},${ans-1})">${ans-1}</button>`;
 }

 window.finalAnswer=function(correct,x){

  if(correct===x){
   score+=10;
   document.getElementById("fs").innerText=
   "Score: "+score;
  }

  next();
 };

 next();
}

/* =========================
 SNAKE
========================= */

function snakeGame(){

 area.innerHTML=`
 <div class="big">🐍 Snake</div>
 <p>⬅️ ➡️ buttons से snake चलाएँ</p>
 <button class="choice" onclick="snakeMove('left')">⬅️</button>
 <button class="choice" onclick="snakeMove('right')">➡️</button>
 <button class="choice" onclick="snakeMove('up')">⬆️</button>
 <button class="choice" onclick="snakeMove('down')">⬇️</button>
 <div id="snm" class="msg">Score: 0</div>`;

 let score=0;

 window.snakeMove=function(){
  score++;
  document.getElementById("snm").innerText=
  "🐍 Score: "+score;
 };
}

/* START */

showGames();

</script>

</body>
</html>
<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Prince Game Zone - 100 Games</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#0b1020;color:#fff}
header{padding:25px 15px;text-align:center;background:linear-gradient(135deg,#7b2ff7,#00a8ff)}
header h1{margin:0;font-size:30px}
.wrap{max-width:1200px;margin:auto;padding:15px}
#search{width:100%;padding:16px;border:0;border-radius:15px;font-size:17px;outline:0;margin-bottom:15px}
.filters{display:flex;gap:8px;overflow:auto;margin-bottom:15px}
.filters button{border:0;border-radius:20px;padding:10px 16px;background:#242a43;color:white;white-space:nowrap}
.filters button.active{background:#7b2ff7}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(210px,1fr));gap:15px}
.card{background:#171d34;border-radius:18px;padding:18px;text-align:center;box-shadow:0 5px 18px #0006}
.icon{font-size:52px}.card h2{font-size:19px;margin:8px 0}
.card p{color:#aab0c0;height:35px}
.play,.btn{border:0;border-radius:12px;padding:12px 18px;background:#2678ff;color:white;font-size:16px;cursor:pointer}
.play{width:100%}
#screen{display:none;background:#171d34;border-radius:20px;padding:20px;text-align:center;max-width:800px;margin:20px auto}
.back{background:#363c55;color:white;border:0;padding:11px 18px;border-radius:10px;cursor:pointer}
canvas{background:#07101d;border-radius:15px;max-width:100%;touch-action:none}
.choice{padding:13px 18px;margin:5px;border:0;border-radius:12px;background:#303754;color:#fff;font-size:18px;cursor:pointer}
.input{padding:13px;border:0;border-radius:10px;font-size:17px;max-width:280px;width:90%}
.msg{font-size:20px;margin:15px;min-height:30px}
.score{font-size:22px;color:#ffd54f;margin:12px}
footer{text-align:center;padding:30px;color:#888}
.ludo{display:grid;grid-template-columns:repeat(3,1fr);max-width:420px;margin:auto}
.ludo div{height:70px;border:1px solid #555;display:flex;align-items:center;justify-content:center;font-size:24px}
.carrom{position:relative;width:min(90vw,480px);height:min(90vw,480px);margin:auto;background:#c9904b;border:15px solid #5b3017;border-radius:20px}
.hole{position:absolute;width:35px;height:35px;background:#111;border-radius:50%}
.h1{top:-5px;left:-5px}.h2{top:-5px;right:-5px}.h3{bottom:-5px;left:-5px}.h4{bottom:-5px;right:-5px}
.disk{position:absolute;width:35px;height:35px;border-radius:50%;background:#222;left:50%;top:50%;transform:translate(-50%,-50%)}
</style>
</head>

<body>

<header>
<h1>🎮 Prince Game Zone</h1>
<p>100 Games • Racing • Ludo • Carrom • Makeup • Kitchen • Action</p>
</header>

<div class="wrap">
<input id="search" placeholder="🔎 Search Games...">
<div class="filters" id="filters"></div>
<div id="games" class="grid"></div>

<div id="screen">
<button class="back" onclick="home()">← सभी गेम</button>
<h2 id="title"></h2>
<div id="area"></div>
</div>
</div>

<footer>🎮 Prince Game Zone</footer>

<script>

const games=[
["Ludo","🎲","Board","ludo"],
["Ludo Challenge","🎲","Board","ludo"],
["Carrom Board","🎯","Board","carrom"],
["Carrom Master","🎯","Board","carrom"],
["Chess","♟️","Board","chess"],
["Checkers","⚫","Board","checkers"],
["Domino","🀫","Board","domino"],
["Sudoku","🔢","Puzzle","sudoku"],
["8 Ball Pool","🎱","Sports","pool"],
["Table Tennis","🏓","Sports","pong"],
["Football","⚽","Sports","football"],
["Penalty Shoot","🥅","Sports","penalty"],
["Basketball","🏀","Sports","basket"],
["Cricket","🏏","Sports","cricket"],
["Boxing","🥊","Action","boxing"],
["Sword Battle","⚔️","Action","sword"],
["Target Shooter","🎯","Action","shooter"],
["Space Shooter","🚀","Action","space"],
["Alien Blaster","👾","Action","alien"],
["Zombie Survival","🧟","Action","zombie"],
["Tank Battle","🛡️","Action","tank"],
["Robot Battle","🤖","Action","robot"],
["Ninja Game","🥷","Action","ninja"],
["Archery","🏹","Action","archery"],
["Airplane Shooter","✈️","Action","plane"],
["Car Racing","🏎️","Racing","racing"],
["Bike Racing","🏍️","Racing","bike"],
["Formula Racer","🏁","Racing","racing"],
["Traffic Racer","🚗","Racing","traffic"],
["Monster Truck","🚙","Racing","truck"],
["Drift Challenge","💨","Racing","drift"],
["Motorcycle Rush","🏍️","Racing","bike"],
["Endless Runner","🏃","Arcade","runner"],
["Jump Runner","🦘","Arcade","runner"],
["Ninja Runner","🥷","Arcade","runner"],
["Snake","🐍","Arcade","snake"],
["Snake Master","🐍","Arcade","snake"],
["Brick Breaker","🧱","Arcade","brick"],
["Space Runner","🌌","Arcade","space"],
["Flappy Bird Style","🐦","Arcade","flappy"],
["Dino Runner","🦖","Arcade","runner"],
["Helicopter","🚁","Arcade","helicopter"],
["Boat Race","🚤","Racing","boat"],
["Fishing","🎣","Arcade","fishing"],
["Bubble Shooter","🫧","Puzzle","bubble"],
["Match 3","💎","Puzzle","match3"],
["2048","🔢","Puzzle","2048"],
["Tetris","🧱","Puzzle","tetris"],
["Maze","🌀","Puzzle","maze"],
["Memory Cards","🃏","Puzzle","memory"],
["Word Game","🔤","Puzzle","word"],
["Math Quiz","🧠","Quiz","math"],
["India Quiz","🇮🇳","Quiz","quiz"],
["Science Quiz","🔬","Quiz","quiz"],
["Animal Quiz","🐯","Quiz","quiz"],
["Emoji Quiz","😀","Quiz","emoji"],
["General Knowledge","📚","Quiz","quiz"],
["Color Match","🎨","Puzzle","color"],
["Number Guess","🔢","Puzzle","guess"],
["Rock Paper Scissors","✊","Classic","rps"],
["Tic Tac Toe","⭕","Classic","tic"],
["Connect Four","🔴","Classic","connect"],
["Hangman","🔤","Word","hangman"],
["Typing Test","⌨️","Skill","typing"],
["Reaction Test","⚡","Skill","reaction"],
["Click Challenge","👆","Skill","click"],
["Fast Tap","👆","Skill","tap"],
["Dice Game","🎲","Luck","dice"],
["Coin Flip","🪙","Luck","coin"],
["Higher Lower","📈","Luck","higher"],
["Lucky Wheel","🎡","Luck","wheel"],
["Makeup Studio","💄","Makeup","makeup"],
["Makeup Artist","💋","Makeup","makeup"],
["Beauty Salon","💅","Makeup","salon"],
["Nail Art","💅","Makeup","nails"],
["Hair Salon","💇","Makeup","hair"],
["Dress Up","👗","Fashion","dress"],
["Fashion Designer","👚","Fashion","fashion"],
["Princess Dress Up","👸","Fashion","dress"],
["Wedding Makeover","👰","Makeup","makeup"],
["Cooking Kitchen","👩‍🍳","Kitchen","kitchen"],
["Pizza Maker","🍕","Kitchen","pizza"],
["Burger Maker","🍔","Kitchen","burger"],
["Cake Maker","🎂","Kitchen","cake"],
["Ice Cream Shop","🍦","Kitchen","icecream"],
["Juice Maker","🥤","Kitchen","juice"],
["Restaurant Chef","👨‍🍳","Kitchen","restaurant"],
["Sushi Maker","🍣","Kitchen","sushi"],
["Pizza Restaurant","🍕","Kitchen","pizza"],
["Cupcake Maker","🧁","Kitchen","cake"],
["Fruit Salad","🍓","Kitchen","salad"],
["Candy Factory","🍬","Kitchen","candy"],
["Coffee Shop","☕","Kitchen","coffee"],
["Pancake Maker","🥞","Kitchen","pancake"],
["Burger Restaurant","🍔","Kitchen","burger"],
["Kitchen Rush","🍳","Kitchen","kitchen"],
["Farm Game","🚜","Simulation","farm"],
["Pet Care","🐶","Simulation","pet"],
["Doctor Game","🩺","Simulation","doctor"],
["Hospital Game","🏥","Simulation","hospital"],
["Firefighter","🚒","Simulation","fire"],
["Police Chase","🚓","Action","police"],
["Treasure Hunt","💰","Adventure","treasure"],
["Island Adventure","🏝️","Adventure","island"],
["Castle Adventure","🏰","Adventure","castle"],
["Space Adventure","🌌","Adventure","space"],
["Final Challenge","🏆","Challenge","final"]
];

const box=document.getElementById("games");
const search=document.getElementById("search");
const filters=document.getElementById("filters");
let category="All";

const cats=["All",...new Set(games.map(x=>x[2]))];

cats.forEach((c,i)=>{
 const b=document.createElement("button");
 b.innerText=c==="All"?"🎮 सभी":c;
 if(i===0)b.className="active";
 b.onclick=()=>{
  category=c;
  document.querySelectorAll(".filters button").forEach(x=>x.classList.remove("active"));
  b.classList.add("active");
  show();
 };
 filters.appendChild(b);
});

function show(){
 const q=search.value.toLowerCase();
 const list=games.filter(g=>
  (category==="All"||g[2]===category)&&
  (g[0].toLowerCase().includes(q)||g[2].toLowerCase().includes(q))
 );
 box.innerHTML=list.length?list.map(g=>{
  const i=games.indexOf(g);
  return `<div class="card">
  <div class="icon">${g[1]}</div>
  <h2>${g[0]}</h2>
  <p>${g[2]}</p>
  <button class="play" onclick="openGame(${i})">▶ PLAY</button>
  </div>`;
 }).join(""):"<div class='msg'>😔 गेम नहीं मिला</div>";
}
search.oninput=show;

function openGame(i){
 document.getElementById("games").style.display="none";
 document.getElementById("filters").style.display="none";
 search.style.display="none";
 document.getElementById("screen").style.display="block";
 document.getElementById("title").innerText=games[i][1]+" "+games[i][0];
 start(games[i][3]);
 window.scrollTo(0,0);
}

function home(){
 document.getElementById("screen").style.display="none";
 document.getElementById("games").style.display="grid";
 document.getElementById("filters").style.display="flex";
 search.style.display="block";
 show();
}

const area=document.getElementById("area");

function start(t){
 area.innerHTML="";
 if(t==="ludo")ludo();
 else if(t==="carrom")carrom();
 else if(t==="racing"||t==="bike"||t==="traffic"||t==="truck"||t==="drift")racing();
 else if(t==="makeup"||t==="salon"||t==="nails"||t==="hair")makeup();
 else if(t==="kitchen"||t==="pizza"||t==="burger"||t==="cake"||t==="icecream"||t==="juice"||t==="restaurant"||t==="sushi"||t==="salad"||t==="candy"||t==="coffee"||t==="pancake")kitchen(t);
 else if(t==="shooter"||t==="space"||t==="alien"||t==="zombie"||t==="tank"||t==="robot"||t==="ninja"||t==="archery"||t==="plane"||t==="police")shooter();
 else if(t==="snake")snake();
 else if(t==="brick")brick();
 else if(t==="pong")pong();
 else if(t==="football"||t==="penalty")football();
 else if(t==="basket")basket();
 else if(t==="dice")dice();
 else if(t==="coin")coin();
 else if(t==="rps")rps();
 else if(t==="tic")tic();
 else if(t==="quiz"||t==="math"||t==="emoji")quiz();
 else if(t==="guess")guess();
 else if(t==="click"||t==="tap")click();
 else if(t==="reaction")reaction();
 else if(t==="memory")memory();
 else if(t==="color")color();
 else if(t==="typing")typing();
 else if(t==="runner")runner();
 else if(t==="fishing")fishing();
 else if(t==="bubble")bubble();
 else if(t==="match3")match3();
 else if(t==="2048")game2048();
 else if(t==="wheel")wheel();
 else if(t==="pet")pet();
 else if(t==="farm")farm();
 else if(t==="doctor"||t==="hospital")doctor();
 else if(t==="fire")fire();
 else if(t==="treasure"||t==="island"||t==="castle")adventure();
 else simple(t);
}

/* LUDO */
function ludo(){
 let pos=[0,0,0,0],turn=0;
 area.innerHTML=`
 <h2>🎲 Ludo</h2>
 <div class="ludo">
 ${Array(9).fill(0).map((_,i)=>`<div id="l${i}">⬜</div>`).join("")}
 </div>
 <button class="btn" onclick="rollLudo()">🎲 Dice</button>
 <div id="lm" class="msg"></div>`;
 window.rollLudo=()=>{
  let n=Math.floor(Math.random()*6)+1;
  turn=(turn+n)%9;
  document.querySelectorAll(".ludo div").forEach(x=>x.innerText="⬜");
  document.getElementById("l"+turn).innerText="🔴";
  document.getElementById("lm").innerText="Dice: "+n;
 };
}

/* CARROM */
function carrom(){
 area.innerHTML=`
 <h2>🎯 Carrom Board</h2>
 <div class="carrom">
 <div class="hole h1"></div><div class="hole h2"></div>
 <div class="hole h3"></div><div class="hole h4"></div>
 <div class="disk"></div>
 </div>
 <p>🎯 स्ट्राइकर को टैप करके शॉट लगाएँ</p>
 <button class="btn" onclick="carromShot()">SHOT</button>
 <div id="cm" class="msg"></div>`;
 window.carromShot=()=>{
  let n=Math.floor(Math.random()*3);
  document.getElementById("cm").innerText=n===0?"🎉 Coin Pocket!":"🏃 फिर कोशिश करें!";
 };
}

/* RACING */
function racing(){
 let score=0,pos=2;
 area.innerHTML=`
 <div class="big">🏎️</div>
 <p>कार को बाएँ-दाएँ चलाएँ और obstacles से बचें।</p>
 <button class="choice" onclick="race('l')">⬅️</button>
 <button class="choice" onclick="race('r')">➡️</button>
 <div id="race" class="score">Score: 0</div>`;
 window.race=d=>{
  pos+=d==="l"?-1:1;
  pos=Math.max(0,Math.min(4,pos));
  score++;
  document.getElementById("race").innerText=
   Math.random()<.12?"💥 Crash! Score: "+score:"🏎️ Score: "+score;
 };
}

/* MAKEUP */
function makeup(){
 area.innerHTML=`
 <h2>💄 Makeup Studio</h2>
 <div style="font-size:100px">👩</div>
 <p>अपना makeup चुनें</p>
 <button class="choice" onclick="make('💄')">💄 Lipstick</button>
 <button class="choice" onclick="make('👁️')">👁️ Eye</button>
 <button class="choice" onclick="make('💇')">💇 Hair</button>
 <button class="choice" onclick="make('💅')">💅 Nails</button>
 <div id="make" class="msg"></div>`;
 window.make=x=>document.getElementById("make").innerText="✨ "+x+" लगाया गया!";
}

/* KITCHEN */
function kitchen(t){
 const items={
 pizza:"🍕 Pizza",burger:"🍔 Burger",cake:"🎂 Cake",
 icecream:"🍦 Ice Cream",juice:"🥤 Juice",sushi:"🍣 Sushi",
 salad:"🥗 Salad",candy:"🍬 Candy",coffee:"☕ Coffee",
 pancake:"🥞 Pancake",kitchen:"🍳 Dish",restaurant:"👨‍🍳 Meal"
 };
 let item=items[t]||"🍳 Food";
 area.innerHTML=`
 <h2>👩‍🍳 Kitchen Game</h2>
 <div class="big">${item}</div>
 <p>सही ingredients चुनें:</p>
 <button class="choice" onclick="cook('🥕')">🥕</button>
 <button class="choice" onclick="cook('🧀')">🧀</button>
 <button class="choice" onclick="cook('🍅')">🍅</button>
 <button class="choice" onclick="cook('🌶️')">🌶️</button>
 <div id="cook" class="msg"></div>`;
 window.cook=x=>document.getElementById("cook").innerText="👨‍🍳 "+x+" added!";
}

/* SHOOTING */
function shooter(){
 let score=0;
 area.innerHTML=`
 <h2>🎯 Target Shooter</h2>
 <div style="font-size:90px" id="target">🎯</div>
 <button class="btn" onclick="shoot()">🔫 SHOOT</button>
 <div id="shot" class="score">Score: 0</div>`;
 window.shoot=()=>{
  score++;
  document.getElementById("target").innerText=
   ["🎯","💥","⭐","🎯"][Math.floor(Math.random()*4)];
  document.getElementById("shot").innerText="Score: "+score;
 };
}

/* SNAKE */
function snake(){
 let score=0;
 area.innerHTML=`
 <div class="big">🐍</div>
 <p>Arrow buttons से snake चलाएँ</p>
 <button class="choice" onclick="snakeMove()">⬅️</button>
 <button class="choice" onclick="snakeMove()">➡️</button>
 <button class="choice" onclick="snakeMove()">⬆️</button>
 <button class="choice" onclick="snakeMove()">⬇️</button>
 <div id="sn" class="score">Score: 0</div>`;
 window.snakeMove=()=>{
  score++;
  document.getElementById("sn").innerText="🐍 Score: "+score;
 };
}

/* BRICK */
function brick(){
 let score=0;
 area.innerHTML=`
 <h2>🧱 Brick Breaker</h2>
 <div id="bricks" style="display:grid;grid-template-columns:repeat(5,1fr);gap:5px"></div>
 <button class="btn" onclick="breakBrick()">BALL 🟠</button>
 <div id="bm" class="score">Bricks: 25</div>`;
 let n=25;
 for(let i=0;i<25;i++)
  document.getElementById("bricks").innerHTML+=`<div id="b${i}" style="height:35px;background:#ff5252;border-radius:5px"></div>`;
 window.breakBrick=()=>{
  if(n>0){
   n--;
   document.getElementById("b"+n).style.visibility="hidden";
   document.getElementById("bm").innerText="Bricks: "+n;
  }
 };
}

/* PONG */
function pong(){
 let score=0;
 area.innerHTML=`
 <h2>🏓 Ping Pong</h2>
 <button class="btn" onclick="pongHit()">🏓 HIT</button>
 <div id="pong" class="score">Score: 0</div>`;
 window.pongHit=()=>{
  score++;
  document.getElementById("pong").innerText="Score: "+score;
 };
}

/* FOOTBALL */
function football(){
 area.innerHTML=`
 <h2>⚽ Penalty Kick</h2>
 <div class="big">🥅</div>
 <button class="choice" onclick="kick('left')">⬅️</button>
 <button class="choice" onclick="kick('center')">⬆️</button>
 <button class="choice" onclick="kick('right')">➡️</button>
 <div id="fk" class="msg"></div>`;
 window.kick=x=>{
  document.getElementById("fk").innerText=
   Math.random()<.6?"⚽ GOAL! 🎉":"🧤 Saved!";
 };
}

/* BASKET */
function basket(){
 let s=0;
 area.innerHTML=`
 <h2>🏀 Basketball</h2>
 <button class="btn" onclick="basketShot()">🏀 SHOOT</button>
 <div id="bs" class="score">Score: 0</div>`;
 window.basketShot=()=>{
  if(Math.random()<.6)s++;
  document.getElementById("bs").innerText=
   Math.random()<.6?"🏀 Score: "+s:"❌ Miss! Score: "+s;
 };
}

/* DICE */
function dice(){
 area.innerHTML=`
 <div class="big" id="die">🎲</div>
 <button class="btn" onclick="roll()">ROLL</button>`;
 window.roll=()=>{
  let n=Math.floor(Math.random()*6);
  document.getElementById("die").innerText=["⚀","⚁","⚂","⚃","⚄","⚅"][n];
 };
}

/* COIN */
function coin(){
 area.innerHTML=`
 <div class="big" id="coin">🪙</div>
 <button class="btn" onclick="flip()">FLIP</button>
 <div id="coinm" class="msg"></div>`;
 window.flip=()=>{
  document.getElementById("coinm").innerText=
   Math.random()<.5?"😀 HEADS":"🔄 TAILS";
 };
}

/* RPS */
function rps(){
 area.innerHTML=`
 <button class="choice" onclick="playR('✊')">✊</button>
 <button class="choice" onclick="playR('✋')">✋</button>
 <button class="choice" onclick="playR('✌️')">✌️</button>
 <div id="rps" class="msg"></div>`;
 window.playR=p=>{
  let c=["✊","✋","✌️"][Math.floor(Math.random()*3)];
  document.getElementById("rps").innerText=
   p===c?"🤝 Draw!":"आप: "+p+" | Computer: "+c;
 };
}

/* TIC */
function tic(){
 let b=Array(9).fill("");
 area.innerHTML=`
 <div style="display:grid;grid-template-columns:repeat(3,1fr);max-width:330px;margin:auto">
 ${b.map((_,i)=>`<button class="choice" id="t${i}" style="height:80px" onclick="tt(${i})"></button>`).join("")}
 </div><div id="ticm" class="msg"></div>`;
 window.tt=i=>{
  if(b[i])return;
  b[i]="X";document.getElementById("t"+i).innerText="X";
  let e=b.map((x,i)=>x?null:i).filter(x=>x!==null);
  if(e.length){
   let c=e[Math.floor(Math.random()*e.length)];
   b[c]="O";document.getElementById("t"+c).innerText="O";
  }
 };
}

/* QUIZ */
function quiz(){
 let q=[
 ["भारत की राजधानी?",["दिल्ली","मुंबई","जयपुर"],0],
 ["राष्ट्रीय पक्षी?",["मोर","तोता","कबूतर"],0],
 ["2+2?",["3","4","5"],1],
 ["पानी का सूत्र?",["H2O","CO2","O2"],0]
 ][Math.floor(Math.random()*4)];
 area.innerHTML=`<h2>${q[0]}</h2>
 ${q[1].map((x,i)=>`<button class="choice" onclick="qa(${i},${q[2]})">${x}</button>`).join("")}
 <div id="qm" class="msg"></div>`;
 window.qa=(a,b)=>document.getElementById("qm").innerText=a===b?"🎉 सही!":"❌ गलत!";
}

/* GUESS */
function guess(){
 let n=Math.floor(Math.random()*50)+1;
 area.innerHTML=`
 <h2>🔢 Number Guess</h2>
 <input class="input" id="guess">
 <button class="btn" onclick="gcheck()">CHECK</button>
 <div id="gm" class="msg"></div>`;
 window.gcheck=()=>{
  let v=+document.getElementById("guess").value;
  document.getElementById("gm").innerText=v===n?"🎉 सही!":v<n?"⬆️ बड़ा नंबर":"⬇️ छोटा नंबर";
 };
}

/* CLICK */
function click(){
 let s=0;
 area.innerHTML=`<button class="btn" onclick="clk()">👆 CLICK</button><div id="clk" class="score">0</div>`;
 window.clk=()=>{
  s++;
  document.getElementById("clk").innerText=s;
 };
}

/* REACTION */
function reaction(){
 area.innerHTML=`
 <button class="btn" id="react" onclick="react()">WAIT</button>
 <div id="rm" class="msg"></div>`;
 let ready=false,start;
 setTimeout(()=>{
  ready=true;start=Date.now();
  document.getElementById("react").innerText="CLICK!";
 },1000+Math.random()*2000);
 window.react=()=>{
  document.getElementById("rm").innerText=
   ready?"⚡ "+(Date.now()-start)+" ms":"😅 Too Early!";
 };
}

/* MEMORY */
function memory(){
 let a=["🍎","🍌","🍇","🍓"].concat(["🍎","🍌","🍇","🍓"]).sort(()=>Math.random()-.5);
 area.innerHTML=`<div style="display:grid;grid-template-columns:repeat(4,1fr);gap:7px">
 ${a.map((x,i)=>`<button class="choice" id="m${i}" onclick="mf(${i})">❓</button>`).join("")}</div>`;
 let first=null;
 window.mf=i=>{
  let b=document.getElementById("m"+i);
  b.innerText=a[i];
  if(first===null)first=i;
  else{
   if(a[first]!==a[i])setTimeout(()=>{
    document.getElementById("m"+first).innerText="❓";
    b.innerText="❓";
   },500);
   first=null;
  }
 };
}

/* COLOR */
function color(){
 let x=["🔴","🔵","🟢","🟡"][Math.floor(Math.random()*4)];
 area.innerHTML=`
 <div class="big">${x}</div>
 <button class="choice" onclick="colorAns('🔴')">🔴</button>
 <butt
