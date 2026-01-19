<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sabrina Story • Passe Premium R$5</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif}
body{background:radial-gradient(circle at top,#0a0f2c,#050510);color:#fff}

/* HEADER */
header{padding:16px 20px;background:linear-gradient(90deg,#0b0033,#1b0066)}
.logo{display:flex;gap:10px;align-items:center}
.logo-icon{
  width:42px;height:42px;border-radius:12px;
  background:linear-gradient(135deg,#ff8a00,#ff00ff);
  display:flex;align-items:center;justify-content:center;
  font-weight:800
}

/* CARD */
.hero{min-height:90vh;display:flex;align-items:center;justify-content:center;padding:20px}
.card{
  max-width:360px;width:100%;
  background:rgba(255,255,255,.08);
  backdrop-filter:blur(18px);
  border-radius:22px;
  padding:26px;
  box-shadow:0 0 40px rgba(0,0,0,.6)
}

.badge{background:#ff8a00;padding:6px 14px;border-radius:20px;font-size:12px;font-weight:600}
.banner{
  margin:14px 0;height:150px;border-radius:18px;
  background:linear-gradient(135deg,#ff00ff,#7f00ff);
  display:flex;align-items:center;justify-content:center;
  font-size:32px;font-weight:800
}

.stats{display:flex;justify-content:space-between;font-size:12px;margin:10px 0}
.stats small{display:block;font-size:11px;opacity:.8}

/* LIST */
ul{list-style:none;margin:12px 0}
ul li::before{content:"✔ ";color:#ff8a00}

/* INPUT */
input{
  width:100%;padding:14px;border-radius:14px;
  border:none;margin:10px 0;font-size:14px
}

/* PRICE */
.old{text-align:center;text-decoration:line-through;opacity:.4}
.price{
  text-align:center;font-size:34px;font-weight:700;margin:6px 0 14px;
  background:linear-gradient(90deg,#00ffcc,#ff8a00);
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent
}

button{
  width:100%;padding:16px;border:none;border-radius:16px;
  background:linear-gradient(90deg,#ff8a00,#ffb347);
  font-size:16px;font-weight:700;cursor:pointer
}
button:disabled{opacity:.4}

/* NOTIFY */
.notify{
  position:fixed;bottom:20px;left:20px;
  background:rgba(0,0,0,.85);
  padding:12px 16px;border-radius:14px;
  font-size:13px;animation:slide .5s
}
@keyframes slide{from{transform:translateX(-120%)}to{transform:translateX(0)}}

/* VIEWERS */
.viewers{
  position:fixed;top:90px;right:20px;
  background:rgba(0,0,0,.75);
  padding:10px 14px;border-radius:14px;font-size:12px
}

/* PULSE */
@keyframes pulse{
  0%{transform:scale(1)}
  50%{transform:scale(1.05);text-shadow:0 0 12px #ff8a00}
  100%{transform:scale(1)}
}
.pulse{animation:pulse .6s}

/* CONFIRM */
#confirm{
  display:none;position:fixed;inset:0;
  background:rgba(0,0,0,.9);
  align-items:center;justify-content:center
}
#confirm .box{
  background:#111;padding:24px;border-radius:18px;
  max-width:320px;width:100%;text-align:center
}
</style>
</head>

<body>

<header>
  <div class="logo">
    <div class="logo-icon">SS</div>
    <div>
      <b>SABRINA STORY</b><br>
      <small>Premium</small>
    </div>
  </div>
</header>

<div class="hero">
  <div class="card">
    <span class="badge">MAIS VENDIDO</span>
    <div class="banner">PASSE</div>

    <h3 style="text-align:center">Passe Premium</h3>
    <p style="text-align:center;font-size:14px;opacity:.8">Entrega rápida no ID</p>

    <div class="stats">
      <span>
        🔥 <b id="sales">8.742</b> vendidos
        <small id="today">+12 vendidos hoje</small>
      </span>
      <span>⚡ Instantâneo</span>
    </div>

    <ul>
      <li>Skins exclusivas</li>
      <li>Recompensas premium</li>
      <li>100% seguro</li>
    </ul>

    <input id="playerId" type="number" placeholder="Digite o ID do jogador" oninput="checkID()">

    <div class="old">R$ 9,99</div>
    <div class="price">R$ 5,00</div>

    <button id="buyBtn" disabled onclick="pay()">Comprar agora</button>
  </div>
</div>

<div class="viewers" id="viewers">👀 12 pessoas vendo agora</div>

<div id="confirm">
  <div class="box" id="confirmBox"></div>
</div>

<script>
// ===== CONFIG =====
const MP_LINK = "https://mpago.la/1k4G4gZ"; // <<< TROQUE SE QUISER
let sales = 8742;
let today = 12;
const maxSales = 9200;
const names = ["Fulano","Ana","Lucas","João","Marcos","Julia","Pedro","Rafael"];

// ===== UI =====
const salesEl = document.getElementById("sales");
const todayEl = document.getElementById("today");

function render(){
  salesEl.textContent = sales.toLocaleString("pt-BR");
  todayEl.textContent = "+"+today+" vendidos hoje";
  salesEl.classList.add("pulse");
  setTimeout(()=>salesEl.classList.remove("pulse"),600);
}

function inc(q=1){
  if(sales < maxSales){
    sales += q;
    today += q;
    render();
  }
}

function checkID(){
  buyBtn.disabled = playerId.value.length < 6;
}

function pay(){
  localStorage.setItem("pid",playerId.value);
  confirm.style.display="flex";
  confirmBox.innerHTML=`
    <h3>💳 Confirmação</h3>
    <p style="margin:10px 0;font-size:14px">
      ID do jogador:<br><b>${playerId.value}</b>
    </p>
    <button onclick="done()">Já paguei</button>
  `;
  window.open(MP_LINK,"_blank");
}

function done(){
  confirmBox.innerHTML=`
    <h3>✅ Pagamento enviado!</h3>
    <p style="font-size:14px;margin-top:10px">
      Aguarde a entrega no ID informado 💎
    </p>
  `;
}

// PROVA SOCIAL
setInterval(()=>{
  const n = names[Math.floor(Math.random()*names.length)];
  const d=document.createElement("div");
  d.className="notify";
  d.innerHTML=`💎 <b>${n}</b> acabou de comprar<br><small>agora mesmo</small>`;
  document.body.appendChild(d);
  setTimeout(()=>d.remove(),4000);
  inc(Math.floor(Math.random()*3)+1);
},8000);

// VIEWERS
setInterval(()=>{
  viewers.innerHTML="👀 "+(Math.floor(Math.random()*10)+8)+" pessoas vendo agora";
},5000);

// CRESCIMENTO
setInterval(()=>inc(1),Math.random()*5000+4000);
render();
</script>

</body>
</html>
