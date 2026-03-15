<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Vamos Esports</title>

<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700&family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

<style>
/* RESET */
* {margin:0; padding:0; box-sizing:border-box; font-family:Poppins,sans-serif;}

/* BODY */
body{
  background: linear-gradient(145deg,#001a0f,#003a1a);
  color:white;
}

/* HEADER */
header{
  text-align:center;
  padding:50px 20px;
  background:rgba(0,255,120,0.08);
  backdrop-filter: blur(12px);
  border-bottom:1px solid rgba(0,255,120,0.3);
}
header img{
  width:130px;
  margin-bottom:10px;
}
header h1{
  font-family:Orbitron, sans-serif;
  font-size:45px;
  color:#00ff8c;
  letter-spacing:5px;
  text-shadow: 0 0 10px #00ff8c,0 0 20px #00ff8c;
}

/* NAVBAR */
nav{
  display:flex;
  justify-content:center;
  flex-wrap:wrap;
  gap:15px;
  padding:20px;
}
nav button{
  padding:12px 25px;
  border-radius:30px;
  border:none;
  background:rgba(0,255,140,0.1);
  color:#00ff8c;
  font-weight:600;
  cursor:pointer;
  transition:0.3s;
}
nav button:hover{
  background:#00ff8c;
  color:black;
  transform:scale(1.1);
}

/* SECTIONS */
section{
  display:none;
  max-width:1000px;
  margin:auto;
  padding:30px;
}

/* CARD */
.card{
  background: rgba(0,255,140,0.05);
  border-radius:15px;
  padding:20px;
  margin:15px 0;
  border:1px solid rgba(0,255,140,0.2);
  backdrop-filter: blur(12px);
}

/* GRID */
.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
  gap:20px;
}

/* INPUT */
input,textarea{
  width:100%;
  padding:12px;
  margin:8px 0;
  border-radius:10px;
  border:none;
  background:rgba(255,255,255,0.08);
  color:white;
}

/* BUTTON */
.action{
  background:#00ff8c;
  border:none;
  padding:10px 18px;
  border-radius:10px;
  font-weight:bold;
  cursor:pointer;
  transition:0.3s;
}
.action:hover{
  transform:scale(1.05);
}

/* ADMIN PANEL */
.admin{
  border:1px solid #00ff8c;
  padding:15px;
  margin-top:20px;
  border-radius:10px;
}

/* TOAST */
#toast{
  position: fixed;
  top:20px;
  right:20px;
  background: rgba(0,255,140,0.1);
  border-left:5px solid #00ff8c;
  padding:15px 20px;
  border-radius:10px;
  color:#00ff8c;
  font-weight:700;
  font-family:'Orbitron',sans-serif;
  backdrop-filter: blur(10px);
  box-shadow:0 0 15px #00ff8c;
  opacity:0;
  transform: translateX(100%);
  transition:0.5s ease;
  z-index:9999;
}
#toast.show{
  opacity:1;
  transform: translateX(0);
}

/* RESPONSIVE TEXT */
h2{font-family:Orbitron; color:#00ff8c; text-shadow: 0 0 8px #00ff8c;}
p{font-size:15px;}
</style>
</head>

<body>

<!-- HEADER -->
<header>
<img src="logo.png" alt="Vamos Logo">
<h1>VAMOS ESPORTS</h1>
</header>

<!-- NAVBAR -->
<nav>
<button onclick="show('login')">Login</button>
<button onclick="show('signup')">Create Account</button>
<button onclick="show('home')">Home</button>
<button onclick="show('history')">History</button>
<button onclick="show('players')">Players</button>
<button onclick="show('achievements')">Achievements</button>
<button onclick="show('motto')">Motto</button>
</nav>

<!-- TOAST -->
<div id="toast"></div>

<!-- LOGIN -->
<section id="login">
<div class="card">
<h2>Login</h2>
<input id="loginUser" placeholder="Username">
<input id="loginPass" type="password" placeholder="Password">
<button class="action" onclick="login()">Login</button>
</div>
</section>

<!-- SIGNUP -->
<section id="signup">
<div class="card">
<h2>Create Account</h2>
<input id="newUser" placeholder="Username">
<input id="newPass" placeholder="Password">
<button class="action" onclick="signup()">Create Account</button>
</div>
</section>

<!-- HOME -->
<section id="home">
<div class="card">
<h2>Welcome to Vamos Esports</h2>
<p>Elite Competitive Gaming Organization.</p>
</div>
</section>

<!-- HISTORY -->
<section id="history">
<h2>Team History</h2>
<div id="historyText" class="card"></div>
<div id="historyAdmin" class="admin" style="display:none">
<textarea id="historyInput" placeholder="Write team history"></textarea>
<button class="action" onclick="saveHistory()">Save</button>
</div>
</section>

<!-- PLAYERS -->
<section id="players">
<h2>Team Players</h2>
<div id="playerList" class="grid"></div>
<div id="playerAdmin" class="admin" style="display:none">
<h3>Add / Edit Player Info</h3>
<input id="pname" placeholder="Real Name">
<input id="pign" placeholder="IGN">
<input id="pid" placeholder="Game ID">
<input id="plevel" placeholder="ID Level">
<input id="pdevice" placeholder="Device">
<input id="pexp" placeholder="Experience">
<button class="action" onclick="addPlayer()">Save Player</button>
</div>
</section>

<!-- ACHIEVEMENTS -->
<section id="achievements">
<h2>Achievements</h2>
<div id="achList"></div>
<div id="achAdmin" class="admin" style="display:none">
<input id="achText" placeholder="Achievement">
<button class="action" onclick="addAch()">Add</button>
</div>
</section>

<!-- MOTTO -->
<section id="motto">
<h2>Team Motto</h2>
<div id="mottoText" class="card"></div>
<div id="mottoAdmin" class="admin" style="display:none">
<input id="mottoInput" placeholder="Edit Motto">
<button class="action" onclick="saveMotto()">Save</button>
</div>
</section>

<script>
// GLOBAL VARIABLES
let currentUser="";
let users=JSON.parse(localStorage.getItem("users"))||[];
let players=JSON.parse(localStorage.getItem("players"))||[];
let achievements=JSON.parse(localStorage.getItem("ach"))||[];
let history=localStorage.getItem("history")||"No history added yet.";
let motto=localStorage.getItem("motto")||"Victory Through Unity.";

// SHOW SECTION
function show(id){
document.querySelectorAll("section").forEach(s=>s.style.display="none");
document.getElementById(id).style.display="block";
render();
}

// TOAST FUNCTION
function showToast(msg){
const toast=document.getElementById("toast");
toast.innerText=msg;
toast.classList.add("show");
setTimeout(()=>{toast.classList.remove("show");},3000);
}

// SIGNUP
function signup(){
users.push({u:newUser.value,p:newPass.value});
localStorage.setItem("users",JSON.stringify(users));
showToast("Account created successfully! 🟢");
}

// LOGIN
function login(){
let u=loginUser.value;
let p=loginPass.value;
if(u=="admin" && p=="admin123"){currentUser="admin";showToast("Admin Login Success! ⚡");}
else{
let found=users.find(x=>x.u==u && x.p==p);
if(found){currentUser=u;showToast("Login Success! ✅");}
else{showToast("Wrong login credentials! ❌"); return;}
}
show("home");
}

// RENDER FUNCTION
function render(){
historyText.innerText=history;
mottoText.innerText=motto;

playerList.innerHTML="";
players.forEach((p,i)=>{
playerList.innerHTML+=`
<div class="card">
${p.user?`<p><b>Player Account:</b> ${p.user}</p>`:"<p><b>Added by Admin</b></p>"}
<h3>${p.name}</h3>
<p><b>IGN:</b> ${p.ign}</p>
<p><b>ID:</b> ${p.id}</p>
<p><b>Level:</b> ${p.level}</p>
<p><b>Device:</b> ${p.device}</p>
<p><b>Experience:</b> ${p.exp}</p>
${currentUser=="admin"?`<button class="action" onclick="removePlayer(${i})">Remove</button>`:""}
</div>`;
});

achList.innerHTML="";
achievements.forEach((a,i)=>{
achList.innerHTML+=`
<div class="card">
${a}
${currentUser=="admin"?`<button class="action" onclick="removeAch(${i})">Remove</button>`:""}
</div>`;
});

// SHOW ADMIN PANEL
if(currentUser=="admin"){
historyAdmin.style.display="block";
playerAdmin.style.display="block";
achAdmin.style.display="block";
mottoAdmin.style.display="block";
}
}

// SAVE HISTORY
function saveHistory(){
history=historyInput.value;
localStorage.setItem("history",history);
showToast("Team history updated! ⚡");
render();
}

// ADD / EDIT PLAYER
function addPlayer(){
if(currentUser=="admin"){
players.push({name:pname.value,ign:pign.value,id:pid.value,level:plevel.value,device:pdevice.value,exp:pexp.value});
showToast("Admin added a new player! ✅");
} else {
let existing=players.find(p=>p.user==currentUser);
if(existing){
existing.name=pname.value;
existing.ign=pign.value;
existing.id=pid.value;
existing.level=plevel.value;
existing.device=pdevice.value;
existing.exp=pexp.value;
showToast("Your information has been updated! ⚡");
} else {
players.push({user:currentUser,name:pname.value,ign:pign.value,id:pid.value,level:plevel.value,device:pdevice.value,exp:pexp.value});
showToast("Player information added successfully! 🟢");
}
}
localStorage.setItem("players",JSON.stringify(players));
render();
}

// REMOVE PLAYER
function removePlayer(i){
players.splice(i,1);
localStorage.setItem("players",JSON.stringify(players));
showToast("Player removed! ❌");
render();
}

// ACHIEVEMENTS
function addAch(){
achievements.push(achText.value);
localStorage.setItem("ach",JSON.stringify(achievements));
showToast("Achievement added! 🏆");
render();
}
function removeAch(i){
achievements.splice(i,1);
localStorage.setItem("ach",JSON.stringify(achievements));
showToast("Achievement removed! ❌");
render();
}

// MOTTO
function saveMotto(){
motto=mottoInput.value;
localStorage.setItem("motto",motto);
showToast("Team Motto updated! ⚡");
render();
}

// INITIAL SHOW LOGIN
show("login");
</script>
</body>
</html>
