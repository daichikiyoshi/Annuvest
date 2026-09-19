
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Annuvest Trade — Practice Trading</title>
<style>
 *{box-sizing:border-box}
  body{margin:0;background:#0d1218;color:#f1eb05;font-family:Inter,system-ui,Arial,sans-serif}
  button,select{font:inherit}
  .app{min-height:100vh;display:grid;grid-template-columns:220px 1fr 300px;grid-template-rows:64px 1fr}
  header{grid-column:1/4;border-bottom:1px solid #0e136f;display:flex;align-items:center;justify-content:space-between;padding:0 20px;background:#0e136f}
  .logo{font-weight:800;letter-spacing:.3px}.logo span{color:rgb(249, 249, 249)}
  .paper{font-size:12px;color:#7f8b99;border:1px solid #27313d;padding:6px 9px;border-radius:7px}
  aside{border-right:1px solid #202833;padding:18px 12px;background:#0d1218}
  .nav-title{font-size:11px;text-transform:uppercase;color:#657180;margin:8px 10px}
  .nav button{width:100%;border:0;background:transparent;color:#aeb8c4;text-align:left;padding:11px 12px;border-radius:7px;cursor:pointer}
  .nav button.active,.nav button:hover{background:#18212b;color:#fff}
  main{padding:18px;overflow:auto}
  .top{display:flex;justify-content:space-between;align-items:center;margin-bottom:14px}
  .symbol{font-size:25px;font-weight:750}.price{font-size:15px;color:#91a0ae;margin-left:10px}
  .positive{color:#39d98a}.negative{color:#ff6374}
  .controls{display:flex;gap:7px;flex-wrap:wrap}
  .controls button{background:#121a23;border:1px solid #26313d;color:#9eabb9;padding:7px 10px;border-radius:6px;cursor:pointer}
  .controls button.active{background:#22303d;color:#fff}
  .chart-card{background:#0d1218;border:1px solid #202833;border-radius:10px;padding:12px}
  canvas{width:100%;height:430px;display:block}
  .stats{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-top:12px}
  .stat{background:#0d1218;border:1px solid #202833;border-radius:8px;padding:12px}
  .stat small{color:#687585}.stat strong{display:block;margin-top:5px}
  .right{border-left:1px solid #202833;background:#0d1218;padding:16px}
  .balance{background:#121922;border:1px solid #26313d;border-radius:9px;padding:13px;margin-bottom:14px}
  .balance small{color:#738091}.balance strong{display:block;font-size:22px;margin-top:3px}
  label{display:block;font-size:12px;color:#7d8997;margin:12px 0 5px}
  input,select{width:100%;background:#101720;border:1px solid #2a3542;color:#e8edf3;border-radius:6px;padding:10px;outline:none}
  .trade{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:14px}
  .trade button{border:0;border-radius:7px;padding:12px;color:#fff;font-weight:750;cursor:pointer}
  .buy{background:#1d9d67}.sell{background:#d94c60}
  .position{border-top:1px solid #202833;margin-top:18px;padding-top:14px}
  .pos-row{display:flex;justify-content:space-between;font-size:13px;margin:7px 0}
  .history{margin-top:18px}.history h3,.position h3{font-size:13px;margin:0 0 10px}
  table{width:100%;border-collapse:collapse;font-size:12px}
  th,td{text-align:left;padding:9px 7px;border-bottom:1px solid #1c252f;color:#aab4bf}
  th{color:#687585;font-weight:600}
  .toast{position:fixed;right:18px;bottom:18px;background:#17212b;border:1px solid #2a3642;padding:11px 14px;border-radius:8px;display:none}
  @media(max-width:900px){.app{grid-template-columns:1fr;grid-template-rows:64px auto auto}.app header{grid-column:1}.app aside{display:none}.right{border-left:0;border-top:1px solid #202833}.stats{grid-template-columns:1fr 1fr}}
</style>
</head>
<body>
<div class="app">
<header>
  <div class="logo">Annuvest<span>Trade</span></div>
  <div class="paper">PRACTICE MODE • VIRTUAL MONEY </div>
</header>

<aside>
  <div class="nav-title">Marketplace</div>
  <div class="nav">
    <button class="active" onclick="pick('VAL')">VALORANT <span> VAL</span></button>
    <button onclick="pick('RBLX')">Roblox <span> RBLX</span></button>
    <button onclick="pick('MC')">Minecraft<span> MC</span></button>
    <button onclick="pick('COD')">Call of Duty<span> COD</span></button>
    <button onclick="pick('ML')">Mobile Legends <span> ML</span></button>
  </div>
</aside>

<main>
  <div class="top">
    <div><span class="symbol" id="symbol">VAL</span><span class="price" id="price">$185.20</span><span id="change" class="positive"> +0.00%</span></div>
    <div class="controls">
      <button class="active" onclick="setTF('1m',this)">1m</button>
      <button onclick="setTF('5m',this)">5m</button>
      <button onclick="setTF('15m',this)">15m</button>
      <button onclick="setTF('1h',this)">1h</button>
    </div>
  </div>

  <div class="chart-card"><canvas id="chart"></canvas></div>

  <div class="stats">
    <div class="stat"><small>Open</small><strong id="open">$185.20</strong></div>
    <div class="stat"><small>High</small><strong id="high">$185.20</strong></div>
    <div class="stat"><small>Low</small><strong id="low">$185.20</strong></div>
    <div class="stat"><small>Volume</small><strong id="volume">0</strong></div>
  </div>

  <div class="history">
    <h3>Trade history</h3>
    <table><thead><tr><th>Time</th><th>Asset</th><th>Side</th><th>Qty</th><th>Price</th><th>Profit/Loss</th></tr></thead>
    <tbody id="history"><tr><td colspan="6" style="color:#687585">No trades yet</td></tr></tbody></table>
  </div>
</main>

<section class="right">
  <div class="balance"><small>Remaining balance</small><strong id="balance">$10,000.00</strong></div>

  <label>Asset</label>
  <select id="asset" onchange="pick(this.value)">
    <option>VAL</option><option>RBLX</option><option>MC</option><option>COD</option><option>ML</option>
  </select>

  <label>Quantity</label>
  <input id="qty" type="number" min="1" value="1">

  <label>Order type</label>
  <select id="orderType"><option>Marketplace</option><option>Limit (simulation)</option></select>

  <div class="trade">
    <button class="buy" onclick="trade('BUY')">BUY</button>
    <button class="sell" onclick="trade('SELL')">SELL</button>
  </div>

  <div class="position">
    <h3>Current position</h3>
    <div class="pos-row"><span>Shares</span><b id="shares">0</b></div>
    <div class="pos-row"><span>Average Price</span><b id="avg">$0.00</b></div>
    <div class="pos-row"><span>Market Value</span><b id="value">$0.00</b></div>
    <div class="pos-row"><span>Unrealized Profit/Loss</span><b id="pnl">$0.00</b></div>
  </div>
</section>
</div>
<div class="toast" id="toast"></div>

<script>
const prices={VAL:185.20,RBLX:245.10,MC:142.30,COD:498.20,ML:231.40};
let symbol='VAL', price=prices[symbol], balance=10000, shares=0, avg=0, history=[], tf='1m';
let candles=[];

function seed(){
  candles=[]; let p=price;
  for(let i=0;i<80;i++){let o=p+(Math.random()-.5)*2;let c=o+(Math.random()-.48)*3;let h=Math.max(o,c)+Math.random()*1.5;let l=Math.min(o,c)-Math.random()*1.5;candles.push({o,c,h,l});p=c}
}
seed();

function pick(s){
  symbol=s; price=prices[s]; document.getElementById('symbol').textContent=s;
  document.getElementById('asset').value=s; shares=0; avg=0; seed(); update(); draw();
}
function setTF(x,b){tf=x;document.querySelectorAll('.controls button').forEach(v=>v.classList.remove('active'));b.classList.add('active')}
function money(x){return '$'+x.toLocaleString(undefined,{minimumFractionDigits:2,maximumFractionDigits:2})}
function toast(t){let e=document.getElementById('toast');e.textContent=t;e.style.display='block';setTimeout(()=>e.style.display='none',1800)}

function trade(side){
  const q=Math.max(1,parseInt(document.getElementById('qty').value)||1);
  const cost=q*price;
  if(side==='BUY'){
    if(cost>balance){toast('Not enough virtual balance');return}
    avg=((avg*shares)+(price*q))/(shares+q); shares+=q; balance-=cost;
  }else{
    if(q>shares){toast('Not enough simulated shares');return}
    shares-=q; balance+=cost; if(shares===0)avg=0;
  }
  history.unshift({time:new Date().toLocaleTimeString(),symbol,side,q,price,pnl:(price-avg)*q});
  renderHistory(); update(); toast(side+' order filled — simulation');
}
function update(){
  document.getElementById('price').textContent=money(price);
  document.getElementById('balance').textContent=money(balance);
  document.getElementById('shares').textContent=shares;
  document.getElementById('avg').textContent=money(avg);
  document.getElementById('value').textContent=money(shares*price);
  const p=(price-avg)*shares;
  const pe=document.getElementById('pnl');pe.textContent=money(p);pe.className=p>=0?'positive':'negative';
  const cs=candles.map(x=>[x.h,x.l]).flat();
  document.getElementById('open').textContent=money(candles[0]?.o||price);
  document.getElementById('high').textContent=money(Math.max(...cs,price));
  document.getElementById('low').textContent=money(Math.min(...cs,price));
  document.getElementById('volume').textContent=Math.floor(120000+Math.random()*900000).toLocaleString();
}
function renderHistory(){
  document.getElementById('history').innerHTML=history.length?history.map(x=>`<tr><td>${x.time}</td><td>${x.symbol}</td><td class="${x.side==='BUY'?'positive':'negative'}">${x.side}</td><td>${x.q}</td><td>${money(x.price)}</td><td>${money(x.pnl)}</td></tr>`).join(''):'<tr><td colspan="6">No trades yet</td></tr>';
}
function draw(){
  const c=document.getElementById('chart'),ctx=c.getContext('2d'),d=devicePixelRatio||1,w=c.clientWidth,h=c.clientHeight;
  c.width=w*d;c.height=h*d;ctx.scale(d,d);ctx.clearRect(0,0,w,h);
  const pad={l:12,r:65,t:15,b:25}, cw=w-pad.l-pad.r,ch=h-pad.t-pad.b;
  const highs=candles.map(x=>x.h), lows=candles.map(x=>x.l), max=Math.max(...highs), min=Math.min(...lows);
  const y=v=>pad.t+(max-v)/(max-min||1)*ch, x=i=>pad.l+i*(cw/(candles.length-1));
  ctx.strokeStyle='#1d2731';ctx.lineWidth=1;
  for(let i=0;i<6;i++){let yy=pad.t+i*ch/5;ctx.beginPath();ctx.moveTo(pad.l,yy);ctx.lineTo(w-pad.r,yy);ctx.stroke();ctx.fillStyle='#657180';ctx.font='11px system-ui';ctx.fillText(money(max-(max-min)*i/5),w-pad.r+8,yy+4)}
  candles.forEach((v,i)=>{let xx=x(i),yo=y(v.o),yc=y(v.c),yh=y(v.h),yl=y(v.l);ctx.beginPath();ctx.moveTo(xx,yh);ctx.lineTo(xx,yl);ctx.stroke();let top=Math.min(yo,yc),bh=Math.max(2,Math.abs(yc-yo));ctx.fillStyle=v.c>=v.o?'#39d98a':'#ff6374';ctx.fillRect(xx-2.5,top,5,bh)});
}
function tick(){
  const drift=(Math.random()-.48)*0.9; const o=price; price=Math.max(.01,price+drift);
  candles.push({o,c:price,h:Math.max(o,price)+Math.random()*.4,l:Math.min(o,price)-Math.random()*.4});candles.shift();
  prices[symbol]=price;update();draw();
}
window.addEventListener('resize',draw); setInterval(tick,1200); update(); draw();
</script>
</body>
</html>
