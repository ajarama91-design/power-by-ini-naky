
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>POWER BY INI NAKY</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#100e16;color:white;font-family:Arial;padding:20px}
header{text-align:center;padding:25px;color:#e8c36d}
h1{font-size:25px}
.card{max-width:430px;margin:20px auto;background:#211c2a;padding:22px;border-radius:16px;border:1px solid #98723b}
input{width:100%;padding:13px;margin:8px 0;border-radius:8px;border:1px solid #766080;background:#15121c;color:white}
button{padding:13px;border:0;border-radius:8px;background:#c49a4c;color:#17121b;font-weight:bold;width:100%;margin-top:10px}
.secondary{background:#514064;color:white}
p{color:#c5b9ce;font-size:14px}
.hidden{display:none}
#status{color:#e8c36d}
</style>
</head>
<body>
<header>
<h1>♛ POWER BY INI NAKY</h1>
<p>VOICE • COMMUNITY • FRIENDSHIP</p>
</header>

<section class="card" id="auth">
<h2>Daftar / Login</h2>
<input id="name" placeholder="Nama pengguna (untuk daftar)">
<input id="email" type="email" placeholder="Alamat email">
<input id="password" type="password" placeholder="Password minimal 6 karakter">
<button id="register">DAFTAR AKUN</button>
<button id="login" class="secondary">LOGIN</button>
<p id="status">Silakan masuk atau buat akun.</p>
</section>

<section class="card hidden" id="profile">
<h2>Selamat datang!</h2>
<p id="userEmail"></p>
<p id="userName"></p>
<button id="logout">LOGOUT</button>
</section>

<script type="module">
import {initializeApp}
from "https://www.gstatic.com/firebasejs/12.5.0/firebase-app.js";

import {
 getAuth,createUserWithEmailAndPassword,
 signInWithEmailAndPassword,onAuthStateChanged,
 signOut,updateProfile
} from "https://www.gstatic.com/firebasejs/12.5.0/firebase-auth.js";

// GANTI NILAI DI BAWAH DENGAN KONFIGURASI FIREBASE MILIKMU
const firebaseConfig = {
 apiKey: "ISI_API_KEY",
 authDomain: "power-by-ini-naky.firebaseapp.com",
 projectId: "power-by-ini-naky",
 storageBucket: "ISI_STORAGE_BUCKET",
 messagingSenderId: "40499073709",
 appId: "ISI_APP_ID"
};

const app=initializeApp(firebaseConfig);
const auth=getAuth(app);

const $=id=>document.getElementById(id);
const status=$("status");

$("register").onclick=async()=>{
 try{
  const name=$("name").value.trim();
  const email=$("email").value.trim();
  const password=$("password").value;
  if(!name)throw Error("Masukkan nama pengguna.");
  const result=await createUserWithEmailAndPassword(auth,email,password);
  await updateProfile(result.user,{displayName:name});
  status.textContent="Pendaftaran berhasil!";
 }catch(e){status.textContent=e.message;}
};

$("login").onclick=async()=>{
 try{
  await signInWithEmailAndPassword(
   auth,$("email").value.trim(),$("password").value
  );
  status.textContent="Login berhasil!";
 }catch(e){status.textContent=e.message;}
};

$("logout").onclick=()=>signOut(auth);

onAuthStateChanged(auth,user=>{
 if(user){
  $("auth").classList.add("hidden");
  $("profile").classList.remove("hidden");
  $("userEmail").textContent="Email: "+user.email;
  $("userName").textContent="Nama: "+(user.displayName||"Pengguna");
 }else{
  $("auth").classList.remove("hidden");
  $("profile").classList.add("hidden");
 }
});
</script>
</body>
</html>
