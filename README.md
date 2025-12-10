<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Productivity + CloudVault</title>

<!-- ================= CSS ================= -->
<style>

/* ROOT VARIABLES */
:root{
  --bg:#0b0b0c;
  --panel:#0f1112;
  --muted:#9aa0a6;
  --accent:#ffb86b;
  --glass: rgba(255,255,255,0.03);
  --shadow: 0 6px 18px rgba(0,0,0,0.6);
  --radius:12px;
  --maxwidth:1100px;
}

/* RESET */
*{box-sizing:border-box}
body{
  margin:0;
  background:linear-gradient(180deg,#050505,#0b0b0c);
  color:#e6eef6;
  font-family:system-ui,Arial;
}

/* COMMON */
.hidden{display:none}
.container{max-width:var(--maxwidth);margin:auto;padding:20px}
.panel{background:var(--panel);padding:20px;border-radius:var(--radius);box-shadow:var(--shadow);margin-bottom:20px}
button{cursor:pointer;padding:10px 14px;border-radius:10px;border:0;background:var(--accent);font-weight:700}
button.ghost{background:transparent;border:1px solid #333;color:var(--muted)}
button.small{padding:6px 8px;font-size:12px}
input,textarea{padding:10px;border-radius:10px;border:1px solid #333;background:transparent;color:inherit}

/* NAV */
.nav{display:flex;justify-content:space-between;align-items:center;padding:14px}
.brand{font-weight:800}

/* HERO */
.hero{display:flex;gap:20px;flex-wrap:wrap}
.hero-left{flex:1}
.hero-right{flex:1}
.hero h1{margin:0 0 10px}

/* TOOLS */
.tools-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px}
.tool-card{border:1px solid #333;padding:16px;border-radius:12px}

/* LIST */
.list-item{border:1px solid #333;padding:10px;border-radius:10px;margin-bottom:8px;display:flex;justify-content:space-between}

/* FOOTER */
.footer{text-align:center;color:var(--muted);padding:10px}

</style>
</head>

<body>

<!-- ================= LOGIN ================= -->
<div id="auth" class="panel container">
  <h2>Login</h2>
  <input id="username" placeholder="Username">
  <input id="password" type="password" placeholder="Password">
  <br><br>
  <button onclick="login()">Login</button>
</div>

<!-- ================= MAIN APP ================= -->
<div id="app" class="hidden">

<nav class="nav">
  <div class="brand">AI Productivity + CloudVault</div>
  <button class="small ghost" onclick="logout()">Logout</button>
</nav>

<div class="container">

<!-- HERO -->
<div class="hero panel">
  <div class="hero-left">
    <h1>Boost Productivity + Secure Storage</h1>
    <p>AI-inspired productivity tools integrated with cloud storage.</p>
  </div>
  <div class="hero-right panel">
    <p>✅ Pomodoro</p>
    <p>✅ Notes</p>
    <p>✅ Cloud Storage</p>
  </div>
</div>

<!-- TOOLS -->
<div class="panel">
  <h2>Productivity Tools</h2>
  <div class="tools-grid">
    <div class="tool-card">
      <h3>Pomodoro</h3>
      <button onclick="startPomodoro()">Start</button>
      <p id="timer"></p>
    </div>
    <div class="tool-card">
      <h3>Notes</h3>
      <textarea id="note"></textarea><br><br>
      <button onclick="saveNote()">Save</button>
      <p id="savedNote"></p>
    </div>
  </div>
</div>

<!-- CLOUD STORAGE -->
<div class="panel">
  <h2>CloudVault Storage</h2>
  <input type="file" id="fileInput">
  <button onclick="uploadFile()">Upload</button>
  <p id="uploadStatus"></p>
  <div id="fileList"></div>
  <button class="small ghost" onclick="clearFiles()">Clear All</button>
</div>

</div>

<footer class="footer">© 2025 AI Productivity + CloudVault</footer>
</div>

<!-- ================= JAVASCRIPT ================= -->
<script>

/* AUTH */
function login(){
  localStorage.setItem("user",username.value);
  auth.classList.add("hidden");
  app.classList.remove("hidden");
}
function logout(){
  localStorage.removeItem("user");
  location.reload();
}
if(localStorage.getItem("user")){
  auth.classList.add("hidden");
  app.classList.remove("hidden");
}

/* POMODORO */
let time=1500,interval;
function startPomodoro(){
  clearInterval(interval);
  interval=setInterval(()=>{
    let m=Math.floor(time/60),s=time%60;
    timer.innerText=${m}:${s<10?'0':''}${s};
    if(--time<0){clearInterval(interval);time=1500;}
  },1000);
}

/* NOTES */
function saveNote(){
  localStorage.setItem("note",note.value);
  savedNote.innerText="Saved ✅";
}
savedNote.innerText=localStorage.getItem("note")||"";

/* CLOUD STORAGE */
function uploadFile(){
  let f=fileInput.files[0];
  if(!f)return;
  let files=JSON.parse(localStorage.getItem("files"))||[];
  files.push({name:f.name,size:f.size});
  localStorage.setItem("files",JSON.stringify(files));
  loadFiles();
}
function loadFiles(){
  fileList.innerHTML="";
  let files=JSON.parse(localStorage.getItem("files"))||[];
  files.forEach((f,i)=>{
    let d=document.createElement("div");
    d.className="list-item";
    d.innerHTML=${f.name} <button onclick="del(${i})">X</button>;
    fileList.appendChild(d);
  });
}
function del(i){
  let files=JSON.parse(localStorage.getItem("files"));
  files.splice(i,1);
  localStorage.setItem("files",JSON.stringify(files));
  loadFiles();
}
function clearFiles(){
  localStorage.removeItem("files");
  loadFiles();
}
loadFiles();

</script>
</body>
</html>
