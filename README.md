<!DOCTYPE html>
<html>
<meta charset="utf-8">
<body>
<div id="life-circle">
<button id="soundBtn">🔊声音</button>

<div id="start" class="page active">
<h4>15 MINUTES CITY</h4>
<h1>你的15分钟<br><span>够用吗？</span></h1>
<p>买菜、取快递、买药……</p>
<button onclick="startGame()">开始挑战</button>
</div>

<div id="game" class="page">
<h2>剩余时间 <span id="time">15:00</span></h2>
<div class="bar"><div id="bar"></div></div>
<h2>你的下一站？</h2>
<div class="cards">
<button class="card" data-t="6" data-task="买菜">🥬<br>菜市场<br>6分钟</button>
<button class="card" data-t="4" data-task="取快递">📦<br>快递驿站<br>4分钟</button>
<button class="card" data-t="5" data-task="买药">💊<br>社区药店<br>5分钟</button>
<button class="card" data-t="7" data-task="吃饭">🍜<br>社区食堂<br>7分钟</button>
<button class="card" data-t="8" data-task="休闲">🌳<br>社区公园<br>8分钟</button>
<button class="card" data-t="15" data-task="购物">🛍️<br>大型商场<br>15分钟</button>
</div>
<p id="route">🏠家</p>
<button onclick="finish()">查看生活圈</button>
</div>

<div id="result" class="page">
<h1 id="score">0</h1>
<h2 id="title">你的生活圈</h2>
<p id="txt"></p>
<div class="data">6255个一刻钟便民生活圈<br>1.29亿居民受益</div>
<button onclick="restart()">再次体验</button>
</div>
</div>

<style>
#life-circle{
width:100%;
height:720px;
overflow:hidden;
background:linear-gradient(135deg,#f7f4ec,#eef7f1);
font-family:"Microsoft YaHei";
color:#27332d;
text-align:center;
position:relative
}
.page{
display:none;
height:720px;
padding:70px;
}
.page.active{
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
}
h1{font-size:70px}
span{color:#f39b55}
button{
border:0;
padding:15px 35px;
border-radius:30px;
background:#27332d;
color:white;
cursor:pointer;
margin:10px
}
.cards{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:15px;
width:90%
}
.card{
background:white;
color:#27332d;
height:120px
}
.card.used{opacity:.3}
.bar{
width:80%;
height:8px;
background:#ddd
}
#bar{
height:100%;
width:100%;
background:#8bbf9f
}
.data{
background:#27332d;
color:white;
padding:30px;
border-radius:20px
}
#soundBtn{
position:absolute;
right:20px;
top:20px;
z-index:5
}
</style>

<script>
let time=15;
let tasks=[];
let ctx;

function tone(f){
ctx=ctx||new AudioContext();
let o=ctx.createOscillator();
let g=ctx.createGain();
o.connect(g);g.connect(ctx.destination);
o.frequency.value=f;
g.gain.value=.1;
o.start();
g.gain.exponentialRampToValueAtTime(.001,ctx.currentTime+.25);
o.stop(ctx.currentTime+.25);
}

function show(id){
document.querySelectorAll(".page").forEach(x=>x.classList.remove("active"));
document.getElementById(id).classList.add("active");
}

function startGame(){
show("game");tone(700);
}

document.querySelectorAll(".card").forEach(c=>{
c.onclick=function(){
let t=Number(this.dataset.t);
if(t>time)return;
time-=t;
tasks.push(this.dataset.task);
this.classList.add("used");
document.getElementById("time").innerHTML=time+":00";
document.getElementById("bar").style.width=(time/15*100)+"%";
document.getElementById("route").innerHTML+=" → "+this.innerText;
tone(600);
}
});

function finish(){
show("result");
document.getElementById("score").innerHTML=tasks.length;
document.getElementById("txt").innerHTML=
tasks.length>=3?"15分钟内完成全部生活需求":"生活圈仍有提升空间";
tone(900);
}

function restart(){
time=15;
tasks=[];
document.querySelectorAll(".card").forEach(c=>c.classList.remove("used"));
document.getElementById("time").innerHTML="15:00";
document.getElementById("bar").style.width="100%";
document.getElementById("route").innerHTML="🏠家";
show("start");
}

document.getElementById("soundBtn").onclick=()=>tone(800);
</script>
</body>
</html>
