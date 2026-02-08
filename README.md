<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Launcher - Free Fire Max</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
*{box-sizing:border-box;font-family:Arial,Helvetica,sans-serif}
body{
  margin:0;
  height:100vh;
  background:#0b0b0f;
  overflow:hidden;
  color:#e5e5e5;
}
canvas{position:fixed;inset:0;z-index:0}
.container{
  position:relative;
  z-index:2;
  height:100vh;
  display:flex;
  align-items:center;
  justify-content:center;
}
.panel{
  width:360px;
  max-width:94%;
  background:rgba(20,20,25,0.95);
  border-radius:14px;
  box-shadow:0 0 40px rgba(0,0,0,0.8);
  padding:16px;
}
.title{
  text-align:center;
  font-weight:bold;
  letter-spacing:1px;
  margin-bottom:12px;
  opacity:.95;
}
.section{
  border-top:1px solid #333;
  padding-top:10px;
  margin-top:10px;
}
.row{
  display:flex;
  align-items:center;
  justify-content:space-between;
  margin:8px 0;
  font-size:14px;
}
input[type="checkbox"]{transform:scale(1.2)}
input[type="range"]{width:100%}
.select{
  width:100%;
  background:#1a1a1f;
  color:#eee;
  border:1px solid #333;
  padding:8px;
  border-radius:8px;
}
.inject{
  margin-top:14px;
  width:100%;
  padding:14px;
  border:none;
  border-radius:10px;
  background:#3a3a3f;
  color:#fff;
  font-weight:bold;
  letter-spacing:1px;
  font-size:15px;
}
.inject:active{transform:scale(0.98)}
.status{
  margin-top:12px;
  text-align:center;
  font-size:14px;
  min-height:20px;
  opacity:.95;
}
.loading{
  width:100%;
  height:8px;
  background:#1a1a1f;
  border-radius:8px;
  overflow:hidden;
  margin-top:10px;
  display:none;
}
.bar{
  height:100%;
  width:0%;
  background:linear-gradient(90deg,#aaa,#fff,#aaa);
  animation: glow 1s linear infinite;
}
@keyframes glow{
  0%{filter:brightness(0.8)}
  50%{filter:brightness(1.3)}
  100%{filter:brightness(0.8)}
}
</style>
</head>
<body>

<canvas id="bg"></canvas>

<div class="container">
  <div class="panel">
    <div class="title">FREE FIRE MAX - LAUNCHER</div>

    <div class="section">
      <div class="row">
        <span>Aimbot (visual)</span>
        <input type="checkbox">
      </div>
      <div class="row">
        <span>Aimlock (visual)</span>
        <input type="checkbox">
      </div>

      <div class="row" style="flex-direction:column;align-items:flex-start;">
        <span>FOV: <b id="fovVal">50</b></span>
        <input type="range" min="0" max="100" value="50" oninput="fovVal.textContent=this.value">
      </div>

      <div class="row" style="flex-direction:column;align-items:flex-start;">
        <span>Precisão: <b id="precVal">5</b></span>
        <input type="range" min="0" max="10" value="5" oninput="precVal.textContent=this.value">
      </div>

      <div class="row">
        <span>Otimização</span>
        <input type="checkbox">
      </div>

      <select class="select">
        <option>Alvo: Cabeça</option>
        <option>Alvo: Pescoço</option>
      </select>
    </div>

    <button class="inject" onclick="inject()">INJETAR E ABRIR JOGO</button>

    <div class="loading" id="loading">
      <div class="bar" id="bar"></div>
    </div>

    <div class="status" id="status"></div>
  </div>
</div>

<script>
// Fundo animado
const canvas = document.getElementById("bg");
const ctx = canvas.getContext("2d");
let w,h;
function resize(){w=canvas.width=window.innerWidth;h=canvas.height=window.innerHeight}
window.addEventListener("resize",resize);resize();

let points=[];
for(let i=0;i<70;i++){
  points.push({
    x:Math.random()*w,
    y:Math.random()*h,
    vx:(Math.random()-0.5)*0.7,
    vy:(Math.random()-0.5)*0.7
  });
}
function draw(){
  ctx.clearRect(0,0,w,h);
  ctx.fillStyle="#0b0b0f";
  ctx.fillRect(0,0,w,h);
  for(let i=0;i<points.length;i++){
    let p=points[i];
    p.x+=p.vx; p.y+=p.vy;
    if(p.x<0||p.x>w) p.vx*=-1;
    if(p.y<0||p.y>h) p.vy*=-1;
    for(let j=i+1;j<points.length;j++){
      let q=points[j];
      let dx=p.x-q.x, dy=p.y-q.y;
      let d=Math.sqrt(dx*dx+dy*dy);
      if(d<140){
        ctx.strokeStyle="rgba(200,200,200,"+(1-d/140)+")";
        ctx.beginPath();
        ctx.moveTo(p.x,p.y);
        ctx.lineTo(q.x,q.y);
        ctx.stroke();
      }
    }
  }
  requestAnimationFrame(draw);
}
draw();

// Animação + abrir jogo
function inject(){
  const status=document.getElementById("status");
  const loading=document.getElementById("loading");
  const bar=document.getElementById("bar");

  loading.style.display="block";
  bar.style.width="0%";
  status.textContent="Carregando configurações...";

  let p=0;
  const interval=setInterval(()=>{
    p+=3;
    bar.style.width=p+"%";
    if(p>=100){
      clearInterval(interval);
      status.textContent="Concluído! Abrindo Free Fire Max...";

      setTimeout(()=>{
        // Tenta abrir o app
        window.location.href = "freefiremax://";

        // Se não abrir, manda pra Play Store
        setTimeout(()=>{
          window.location.href = "https://play.google.com/store/apps/details?id=com.dts.freefiremax";
        },2000);
      },1000);
    }
  },60);
}
</script>

</body>
</html>
