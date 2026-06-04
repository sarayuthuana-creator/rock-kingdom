[game1.html.html](https://github.com/user-attachments/files/28608406/game1.html.html)
# rock-kingdom<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>ด่าน 1 : หุบเขาหินอัคนี</title>
<style>
body{margin:0;font-family:Arial,sans-serif;background:linear-gradient(135deg,#ffd89b,#19547b);min-height:100vh}
.wrap{max-width:1000px;margin:auto;padding:20px}
.card{background:white;border-radius:20px;padding:20px;box-shadow:0 4px 20px rgba(0,0,0,.2)}
h1{text-align:center}
.progress{height:20px;background:#eee;border-radius:20px;overflow:hidden}
.bar{height:100%;width:0%;background:#4caf50}
.top{display:flex;justify-content:space-between;gap:10px;flex-wrap:wrap}
.zone{flex:1;min-height:140px;border:3px dashed #777;border-radius:15px;padding:10px;margin:10px 0;background:#fafafa}
.items{display:flex;flex-wrap:wrap;gap:10px;justify-content:center}
.item{padding:12px 16px;background:#ffe0b2;border-radius:12px;cursor:move}
#reward{display:none;text-align:center;font-size:28px;margin-top:15px}
button{padding:12px 18px;font-size:18px;border:none;border-radius:12px;cursor:pointer}
</style>
</head>
<body>
<div class="wrap">
<div class="card">
<h1>🔴 หุบเขาหินอัคนี</h1>
<div id="player"></div>
<p>ลากหินไปยังประเภทที่ถูกต้อง (สุ่ม 5 ข้อ ข้อละ 5 คะแนน)</p>

<div class="progress"><div class="bar" id="bar"></div></div>
<h3>คะแนน: <span id="score">0</span>/25</h3>

<div class="top">
<div class="zone" id="extrusive" ondrop="drop(event)" ondragover="allowDrop(event)">🌋 หินอัคนีพุ</div>
<div class="zone" id="intrusive" ondrop="drop(event)" ondragover="allowDrop(event)">🏔️ หินอัคนีแทรกซอน</div>
</div>

<div class="items" id="items"></div>

<div style="text-align:center;margin-top:20px">
<button onclick="finishGame()">จบด่าน</button>
</div>

<div id="reward">💎 ได้รับผลึกสีแดง!</div>
<div id="msg"></div>
</div>
</div>

<script>
const WEBAPP="https://script.google.com/macros/s/AKfycbwvFtdoQhD6QBjf5P7kZQybUfg_FynntsI4Ond_0PcCFNDq_7vJSeqCXu_i5lbm0njbjQ/exec";

const rocks=[
{name:"หินบะซอลต์",type:"extrusive"},
{name:"หินออบซิเดียน",type:"extrusive"},
{name:"หินพัมมิซ",type:"extrusive"},
{name:"หินไรโอไลต์",type:"extrusive"},
{name:"หินแกรนิต",type:"intrusive"},
{name:"หินไดออไรต์",type:"intrusive"},
{name:"หินแกบโบร",type:"intrusive"},
{name:"หินเพอริโดไทต์",type:"intrusive"}
];

document.getElementById('player').innerHTML=
"👦 "+(localStorage.getItem('playerName')||'-')+
" | 🏫 "+(localStorage.getItem('playerClass')||'-')+
" | เลขที่ "+(localStorage.getItem('playerNo')||'-');

const selected=rocks.sort(()=>Math.random()-0.5).slice(0,5);
let score=0,done=0;

selected.forEach((r,i)=>{
 const d=document.createElement('div');
 d.className='item';
 d.draggable=true;
 d.id='r'+i;
 d.dataset.type=r.type;
 d.innerText=r.name;
 d.ondragstart=e=>e.dataTransfer.setData('text',d.id);
 items.appendChild(d);
});

function allowDrop(e){e.preventDefault();}

function drop(e){
 e.preventDefault();
 const id=e.dataTransfer.getData('text');
 const el=document.getElementById(id);
 if(!el || el.dataset.done) return;

 if(el.dataset.type===e.currentTarget.id){
   score+=5;
   el.style.background='#c8e6c9';
 } else {
   el.style.background='#ffcdd2';
 }
 el.dataset.done=1;
 done++;
 document.getElementById('score').innerText=score;
 document.getElementById('bar').style.width=(done/5*100)+'%';
}

function finishGame(){
 if(done<5){alert('กรุณาตอบให้ครบ 5 ข้อ');return;}

 localStorage.setItem('score1',score);
 localStorage.setItem('crystal1','true');
 document.getElementById('reward').style.display='block';

 const payload={
   name:localStorage.getItem('playerName')||'',
   classroom:localStorage.getItem('playerClass')||'',
   number:localStorage.getItem('playerNo')||'',
   stage:'ด่าน1-หินอัคนี',
   score:score,
   crystal:'แดง',
   total:score,
   playtime:'-',
   device:navigator.userAgent
 };

 fetch(WEBAPP,{
   method:'POST',
   body:JSON.stringify(payload)
 }).catch(()=>{});

 document.getElementById('msg').innerHTML=
 '<h3>บันทึกคะแนนแล้ว</h3><p>นำลิงก์ด่านถัดไปไปใส่ใน Canva ได้เลย</p>';
}
</script>
</body>
</html>
