# Login-dashboard
Tmgm login dashboard
<!DOCTYPE html><html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>TMGM PropFarm</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body{margin:0;font-family:'Poppins',sans-serif;background:#f1f5f9;color:#0f172a}
.hidden{display:none}
.login{display:flex;justify-content:center;align-items:center;height:100vh}
.login-box{background:white;padding:30px;border-radius:12px;box-shadow:0 10px 30px rgba(0,0,0,0.1)}
input,select{padding:10px;margin:8px 0;width:100%;border-radius:6px;border:1px solid #ccc}
button{padding:10px;border:none;border-radius:6px;background:#3b82f6;color:white;cursor:pointer}
.menu-btn{position:fixed;top:10px;left:10px;background:#3b82f6;color:white;padding:10px;border-radius:8px;cursor:pointer;z-index:1000}
.sidebar{position:fixed;left:-260px;top:0;width:240px;height:100vh;background:#1e293b;color:white;padding:20px;transition:.3s;z-index:999}
.sidebar.active{left:0}
.sidebar li{margin:15px 0;cursor:pointer}
.overlay{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.4);display:none}
.overlay.active{display:block}
.main{padding:20px;margin-top:50px}
.card{background:white;padding:15px;border-radius:10px;margin-bottom:15px;box-shadow:0 5px 15px rgba(0,0,0,0.05)}
.bar{height:10px;background:#e2e8f0;border-radius:8px}
.fill{height:100%;border-radius:8px}
.equity{background:#22c55e;width:75%}
.drawdown{background:#ef4444;width:25%}
.table{width:100%;border-collapse:collapse}
.table td,.table th{padding:8px;border-bottom:1px solid #ddd}
.notify{position:fixed;top:10px;right:10px;background:#22c55e;color:white;padding:10px;border-radius:6px;display:none}
.small{font-size:12px;opacity:.7}
.rule li{margin:6px 0}
.admin-box{background:#eef2ff;padding:10px;border-radius:8px;margin-top:10px}
</style>
</head>
<body><div id="loginPage" class="login">
  <div class="login-box">
    <h2>TMGM PropFarm Login</h2>
    <input type="text" placeholder="Username">
    <input type="password" placeholder="Password">
    <button onclick="login()">Login</button>
  </div>
</div><div id="dashboard" class="hidden"><div class="menu-btn" onclick="toggleMenu()">☰</div>
<div class="overlay" id="overlay" onclick="closeMenu()"></div><div class="sidebar" id="sidebar">
  <ul>
    <li onclick="nav('dash')">Dashboard</li>
    <li onclick="nav('mt5')">MT5 Terminal</li>
    <li onclick="nav('admin')">Admin Panel</li>
    <li onclick="nav('withdraw')">Withdraw</li>
  </ul>
</div><div class="main"><!-- DASHBOARD --><div id="dash" class="page">
  <div class="card">
    <h3>TMGM PropFarm Account</h3>
    <p>Status: Phase 1 Completed | Phase 2 Ongoing</p>
  </div>  <div class="card">
    <h4>Equity</h4>
    <div class="bar"><div class="fill equity"></div></div>
    <h4>Drawdown</h4>
    <div class="bar"><div class="fill drawdown"></div></div>
  </div>  <div class="card">
    <h3>Live Market (Candlestick Style)</h3>
    <canvas id="chart"></canvas>
    <p class="small">Simulated live price movement</p>
  </div>  <div class="card">
    <h3>Trading Rules</h3>
    <ul class="rule">
      <li>No News Trading</li>
      <li>No Night Trading</li>
      <li>No Multiple Positions</li>
      <li>No Weekend Trading</li>
      <li>Profit Target 50%</li>
      <li>Daily Loss 15%</li>
      <li>Max Loss 30%</li>
    </ul>
  </div>
</div><!-- MT5 --><div id="mt5" class="page hidden">
  <div class="card">
    <h3>MT5 Terminal</h3>
    <table class="table">
      <tr><th>Pair</th><th>Lot</th><th>Profit</th></tr>
      <tr><td>EURUSD</td><td>0.5</td><td>+120</td></tr>
      <tr><td>XAUUSD</td><td>0.2</td><td>-50</td></tr>
    </table>
  </div>
</div><!-- ADMIN --><div id="admin" class="page hidden">
  <div class="card">
    <h3>Admin Panel</h3>
    <div class="admin-box">User001 - Active</div>
    <div class="admin-box">User002 - Failed</div>
    <div class="admin-box">User003 - Phase 2</div>
  </div>
</div><!-- WITHDRAW --><div id="withdraw" class="page hidden">
  <div class="card">
    <h3>Withdraw Funds</h3>
    <select>
      <option>Bank (NGN)</option>
      <option>USDT</option>
      <option>PayPal</option>
    </select>
    <input type="number" placeholder="Enter Amount">
    <button onclick="withdraw()">Submit</button>
    <p id="msg"></p>
  </div>
</div></div>
</div><div class="notify" id="notify">Price Updated</div><script>
function login(){
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('dashboard').classList.remove('hidden');
}
function toggleMenu(){
  document.getElementById('sidebar').classList.add('active');
  document.getElementById('overlay').classList.add('active');
}
function closeMenu(){
  document.getElementById('sidebar').classList.remove('active');
  document.getElementById('overlay').classList.remove('active');
}
function nav(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
  closeMenu();
}
function withdraw(){
  document.getElementById('msg').innerText='Challenge not completed';
}

// Fake live price movement
let data=[5000,5050,4980,5100,5200];
setInterval(()=>{
  let last=data[data.length-1];
  let next=last+(Math.random()*100-50);
  data.push(next);
  data.shift();
  chart.data.datasets[0].data=data;
  chart.update();
},2000);

// Notification
setInterval(()=>{
  let n=document.getElementById('notify');
  n.style.display='block';
  setTimeout(()=>n.style.display='none',1500);
},4000);

let chart=new Chart(document.getElementById('chart'),{
  type:'line',
  data:{labels:['1','2','3','4','5'],datasets:[{data:data}]}
});
</script></body>
</html>
