<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Agri Earn Money 🌿</title>
<style>
/* আগের গ্রাফিক্স ডিজাইন 그대로 */
body { margin:0; font-family: Arial, sans-serif; background:#121212; color:#fff; }
header { background:#1c1c1c; padding:12px; text-align:center; font-size:20px; font-weight:bold; }
.view { display:none; padding:15px; }
.view.active { display:block; }
.card { background:#222; border-radius:8px; padding:15px; margin:10px 0; cursor:pointer; box-shadow: 0 2px 5px rgba(0,0,0,0.5); transition: 0.2s; }
.card:hover { transform: translateY(-2px); background:#333; }
.footer-nav { position:fixed; bottom:0; left:0; width:100%; display:flex; background:#1c1c1c; border-top:1px solid #333; }
.footer-nav button { flex:1; padding:10px; background:none; border:none; color:#aaa; font-size:14px; }
.footer-nav button.active { color:#00ff66; font-weight:bold; }
table { width:100%; border-collapse:collapse; margin-top:10px; }
th, td { border:1px solid #333; padding:8px; text-align:center; }
ul { list-style:none; padding-left:0; }
ul li { background:#222; margin:5px 0; padding:10px; border-radius:6px; cursor:pointer; transition:0.2s; }
ul li:hover { background:#333; }
/* Modal */
.modal { display:none; position:fixed; z-index:1000; left:0; top:0; width:100%; height:100%; background: rgba(0,0,0,0.7); }
.modal-content { background:#1c1c1c; margin:15% auto; padding:20px; border-radius:8px; width:80%; max-width:400px; text-align:center; }
.modal button { margin:10px; padding:10px 20px; border:none; border-radius:5px; cursor:pointer; }
.modal .confirm { background:#00ff66; color:#000; }
.modal .cancel { background:#ff4d4d; color:#fff; }
</style>
<script src='//libtl.com/sdk.js' data-zone='9760722' data-sdk='show_9760722'></script>
</head>
<body>

<header>Agri Earn Money 🌿</header>

<div id="home" class="view active">
  <h2>হোম</h2>
  <p>স্বাগতম! এখানে কাজ করে পয়েন্ট অর্জন করুন।</p>
  <p>পয়েন্ট: <span id="points-value">0</span></p>
  <p>ব্যালেন্স: <span id="balance-value">$0.00</span></p>
</div>

<div id="tasks" class="view">
  <h2>আয়ের কাজ</h2>
  <div class="card" onclick="earnPoints()">
    <b>📺 বিজ্ঞাপন দেখুন</b>
    <p>পয়েন্ট অর্জনের জন্য বিজ্ঞাপন দেখুন।</p>
  </div>
</div>

<div id="leader" class="view">
  <h2>লিডারবোর্ড</h2>
  <p>শীঘ্রই আসছে...</p>
</div>

<div id="history" class="view">
  <h2>ইতিহাস</h2>
  <table>
    <tr><th>কাজ</th><th>রিওয়ার্ড</th></tr>
  </table>
</div>

<div id="payments" class="view">
  <h2>পেমেন্ট মেথড</h2>
  <ul id="payment-list"></ul>
</div>

<!-- Modal -->
<div id="withdraw-modal" class="modal">
  <div class="modal-content">
    <p id="modal-text"></p>
    <button class="confirm" onclick="confirmWithdraw()">Confirm</button>
    <button class="cancel" onclick="closeModal()">Cancel</button>
  </div>
</div>

<div class="footer-nav">
  <button class="nav-button active" onclick="showView('home')">🏠 হোম</button>
  <button class="nav-button" onclick="showView('tasks')">✅ টাস্ক</button>
  <button class="nav-button" onclick="showView('leader')">🏆 লিডার</button>
  <button class="nav-button" onclick="showView('history')">🔄 ইতিহাস</button>
  <button class="nav-button" onclick="showView('payments')">💳 পেমেন্ট</button>
</div>

<script>
let points = 0;
let balance = 0;
const ratePerPoint = 0.01;
const minWithdraw = 5;
const paymentMethods = ["TearC","BinB","Ethereum","Tron Coin","Ades","Ton Coin","bKash","Nagad"];
let pendingMethod = '';

function showView(viewId){
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  document.getElementById(viewId).classList.add('active');
  document.querySelectorAll('.nav-button').forEach(btn => btn.classList.remove('active'));
  const activeBtn=document.querySelector(`.footer-nav button[onclick="showView('${viewId}')"]`);
  if(activeBtn) activeBtn.classList.add('active');
}

function earnPoints(){
  show_9760722().then(()=>{
    points += 1;
    balance = points * ratePerPoint;
    document.getElementById("points-value").innerText=points;
    document.getElementById("balance-value").innerText="$"+balance.toFixed(2);
    addHistory("বিজ্ঞাপন দেখা হয়েছে", "+"+ratePerPoint.toFixed(2));
  }).catch(err=>{
    console.log("Ad error:",err);
    alert("বিজ্ঞাপন লোড হয়নি, পরে আবার চেষ্টা করুন।");
  });
}

function addHistory(task,reward){
  const table=document.querySelector("#history table");
  const row=document.createElement("tr");
  row.innerHTML=`<td>${task}</td><td>${reward}</td>`;
  table.appendChild(row);
}

function loadPaymentMethods(){
  const ul=document.getElementById("payment-list");
  ul.innerHTML="";
  paymentMethods.forEach(method=>{
    const li=document.createElement("li");
    li.textContent=method;
    li.onclick=()=>openModal(method);
    ul.appendChild(li);
  });
}

function openModal(method){
  if(balance < minWithdraw){
    alert(`❌ মিনিমাম $${minWithdraw} (প্রায় ৫০০ টাকা) থাকতে হবে withdraw এর জন্য।`);
    return;
  }
  pendingMethod = method;
  document.getElementById('modal-text').innerText = `আপনি কি ${method} এ $${balance.toFixed(2)} withdraw করতে চান?`;
  document.getElementById('withdraw-modal').style.display = 'block';
}

function closeModal(){
  document.getElementById('withdraw-modal').style.display = 'none';
}

function confirmWithdraw(){
  addHistory(`Withdraw to ${pendingMethod}`, `-$${balance.toFixed(2)}`);
  sendWithdrawRequest("RaselUser", pendingMethod, balance.toFixed(2));
  points = 0;
  balance = 0;
  document.getElementById("points-value").innerText = points;
  document.getElementById("balance-value").innerText = "$"+balance.toFixed(2);
  alert(`✅ সফলভাবে ${pendingMethod} এ withdraw হয়েছে।`);
  closeModal();
}

function sendWithdrawRequest(user, method, amount){
  const token = "7518504224:AAHb54L02FW3o3u0sdVpDbIQIDaNmUDROMI";
  const chatId = "6427972534";
  const message = `📢 নতুন Withdraw রিকোয়েস্ট!\n\n👤 User: ${user}\n💳 Method: ${method}\n💵 Amount: $${amount}`;
  fetch(`https://api.telegram.org/bot${token}/sendMessage`,{
    method:"POST",
    headers:{"Content-Type":"application/json"},
    body:JSON.stringify({chat_id:chatId,text:message})
  });
}

window.onload=function(){ loadPaymentMethods(); };
</script>

</body>
</html>
