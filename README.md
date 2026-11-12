# electric-tools
เว็บไซต์เครื่องมือคำนวณไฟฟ้า

<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Electrical Calculator</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #0b1220;
  color: white;
  font-family: Arial, sans-serif;
}

header {
  background: linear-gradient(120deg,#075985,#172554);
  padding: 25px 15px;
  text-align: center;
}

header h1 {
  margin: 0;
  color: #38bdf8;
  font-size: 25px;
}

header p {
  color: #cbd5e1;
}

nav {
  display: flex;
  gap: 8px;
  padding: 12px;
  overflow-x: auto;
  background: #111c30;
}

nav button {
  flex-shrink: 0;
  border: 1px solid #334155;
  background: #1e293b;
  color: white;
  padding: 12px;
  border-radius: 10px;
}

nav button.active {
  background: #0284c7;
}

.container {
  padding: 15px;
  max-width: 600px;
  margin: auto;
}

.panel {
  display: none;
}

.panel.active {
  display: block;
}

.card {
  background: #172338;
  padding: 18px;
  border-radius: 15px;
  margin-bottom: 15px;
  border: 1px solid #263b57;
}

h2 {
  color: #38bdf8;
  font-size: 20px;
}

label {
  display: block;
  margin-top: 13px;
  color: #cbd5e1;
}

input, select {
  width: 100%;
  padding: 13px;
  margin-top: 6px;
  background: #0b1220;
  color: white;
  border: 1px solid #475569;
  border-radius: 9px;
  font-size: 16px;
}

.action {
  width: 100%;
  padding: 14px;
  background: #0284c7;
  color: white;
  border: none;
  border-radius: 10px;
  margin-top: 15px;
  font-size: 16px;
}

.result {
  background: #063b36;
  border: 1px solid #0d9488;
  border-radius: 10px;
  padding: 15px;
  margin-top: 15px;
  line-height: 1.8;
  overflow-wrap: anywhere;
}

.small {
  color: #94a3b8;
  font-size: 13px;
  line-height: 1.6;
}

footer {
  text-align: center;
  color: #64748b;
  padding: 25px;
}
</style>
</head>

<body>

<header>
  <h1>⚡ ELECTRICAL CALC</h1>
  <p>เครื่องมือคำนวณสำหรับช่างไฟฟ้า</p>
</header>

<nav>
  <button class="active" onclick="showPage('ohm',this)">กฎโอห์ม</button>
  <button onclick="showPage('resistor',this)">แถบสี R</button>
  <button onclick="showPage('power',this)">กำลังไฟ</button>
  <button onclick="showPage('energy',this)">ค่าไฟ</button>
</nav>

<div class="container">

<!-- OHM LAW -->

<section id="ohm" class="panel active">
<div class="card">
<h2>⚡ เครื่องคำนวณกฎของโอห์ม</h2>
<p class="small">
กรอกค่าที่ทราบ 2 ค่า แล้วกดคำนวณ
</p>

<label>แรงดันไฟฟ้า V (โวลต์)</label>
<input id="V" type="number" placeholder="เช่น 12">

<label>กระแสไฟฟ้า I (แอมป์)</label>
<input id="I" type="number" placeholder="เช่น 2">

<label>ความต้านทาน R (โอห์ม)</label>
<input id="R" type="number" placeholder="เช่น 6">

<label>กำลังไฟฟ้า P (วัตต์)</label>
<input id="P" type="number" placeholder="เช่น 24">

<button class="action" onclick="ohm()">คำนวณ</button>
<button class="action" onclick="clearOhm()" style="background:#475569">
ล้างข้อมูล
</button>

<div id="ohmResult" class="result" style="display:none"></div>
</div>

<div class="card">
<h2>สูตรกฎของโอห์ม</h2>
<p>V = I × R</p>
<p>I = V ÷ R</p>
<p>R = V ÷ I</p>
<p>P = V × I</p>
<p class="small">ใช้กับวงจรไฟฟ้ากระแสตรงและโหลดที่เหมาะสมกับการคำนวณ</p>
</div>
</section>

<!-- RESISTOR -->

<section id="resistor" class="panel">
<div class="card">
<h2>🎨 เครื่องอ่านแถบสีตัวต้านทาน</h2>

<label>แถบสีที่ 1</label>
<select id="c1"></select>

<label>แถบสีที่ 2</label>
<select id="c2"></select>

<label>ตัวคูณ (แถบที่ 3)</label>
<select id="c3"></select>

<label>ค่าความคลาดเคลื่อน (แถบที่ 4)</label>
<select id="c4"></select>

<button class="action" onclick="resistor()">อ่านค่าตัวต้านทาน</button>

<div id="resResult" class="result" style="display:none"></div>
</div>
</section>

<!-- POWER -->

<section id="power" class="panel">
<div class="card">
<h2>🔌 คำนวณกำลังไฟฟ้า</h2>

<label>แรงดันไฟฟ้า (V)</label>
<input id="pv" type="number" placeholder="เช่น 220">

<label>กระแสไฟฟ้า (A)</label>
<input id="pi" type="number" placeholder="เช่น 2">

<button class="action" onclick="powerCalc()">คำนวณกำลังไฟฟ้า</button>

<div id="powerResult" class="result" style="display:none"></div>
</div>
</section>

<!-- ENERGY -->

<section id="energy" class="panel">
<div class="card">
<h2>💡 คำนวณพลังงานและค่าไฟ</h2>

<label>กำลังไฟฟ้า (วัตต์)</label>
<input id="watt" type="number" placeholder="เช่น 1000">

<label>จำนวนชั่วโมงใช้งานต่อวัน</label>
<input id="hours" type="number" placeholder="เช่น 5">

<label>จำนวนวันที่ใช้งาน</label>
<input id="days" type="number" value="30">

<label>ค่าไฟต่อหน่วย (บาท/kWh)</label>
<input id="rate" type="number" value="4.5">

<button class="action" onclick="energyCalc()">คำนวณค่าไฟ</button>

<div id="energyResult" class="result" style="display:none"></div>
</div>
</section>

</div>

<footer>
ELECTRICAL CALCULATOR<br>
เครื่องมือเรียนรู้พื้นฐานไฟฟ้า
</footer>

<script>

// เปลี่ยนหน้า
function showPage(id,btn) {
  document.querySelectorAll('.panel').forEach(p=>{
    p.classList.remove('active');
  });

  document.getElementById(id).classList.add('active');

  document.querySelectorAll('nav button').forEach(b=>{
    b.classList.remove('active');
  });

  btn.classList.add('active');
}

// กฎของโอห์ม
function ohm() {
  let v = document.getElementById('V').value;
  let i = document.getElementById('I').value;
  let r = document.getElementById('R').value;
  let p = document.getElementById('P').value;

  let values = [v,i,r,p];
  let count = values.filter(x=>x !== "").length;

  let out = document.getElementById('ohmResult');
  out.style.display = "block";

  if(count !== 2) {
    out.innerHTML = "กรุณากรอกข้อมูลให้ครบ 2 ค่า";
    return;
  }

  v = v === "" ? null : Number(v);
  i = i === "" ? null : Number(i);
  r = r === "" ? null : Number(r);
  p = p === "" ? null : Number(p);

  if(values.some(x=>x!=="" && (!Number.isFinite(Number(x)) || Number(x)<=0))) {
    out.innerHTML = "กรุณากรอกตัวเลขที่มากกว่า 0";
    return;
  }

  let ok = false;

  if(v!==null && i!==null) {
    r=v/i; p=v*i; ok=true;
  }
  else if(v!==null && r!==null) {
    i=v/r; p=v*i; ok=true;
  }
  else if(v!==null && p!==null) {
    i=p/v; r=v/i; ok=true;
  }
  else if(i!==null && r!==null) {
    v=i*r; p=v*i; ok=true;
  }
  else if(i!==null && p!==null) {
    v=p/i; r=v/i; ok=true;
  }
  else if(r!==null && p!==null) {
    i=Math.sqrt(p/r); v=i*r; ok=true;
  }

  if(!ok) {
    out.innerHTML = "ไม่สามารถคำนวณได้";
    return;
  }

  document.getElementById('V').value=v.toFixed(3);
  document.getElementById('I').value=i.toFixed(3);
  document.getElementById('R').value=r.toFixed(3);
  document.getElementById('P').value=p.toFixed(3);

  out.innerHTML =
    "<b>ผลการคำนวณ</b><br>"+
    "แรงดัน V = "+v.toFixed(3)+" V<br>"+
    "กระแส I = "+i.toFixed(3)+" A<br>"+
    "ความต้านทาน R = "+r.toFixed(3)+" Ω<br>"+
    "กำลังไฟ P = "+p.toFixed(3)+" W";
}

function clearOhm() {
  ['V','I','R','P'].forEach(id=>{
    document.getElementById(id).value="";
  });

  document.getElementById('ohmResult').style.display="none";
}

// แถบสีตัวต้านทาน
const colors = [
  {name:"ดำ",value:0,mul:1},
  {name:"น้ำตาล",value:1,mul:10},
  {name:"แดง",value:2,mul:100},
  {name:"ส้ม",value:3,mul:1000},
  {name:"เหลือง",value:4,mul:10000},
  {name:"เขียว",value:5,mul:100000},
  {name:"น้ำเงิน",value:6,mul:1000000},
  {name:"ม่วง",value:7,mul:10000000},
  {name:"เทา",value:8,mul:100000000},
  {name:"ขาว",value:9,mul:1000000000}
];

const multipliers = [
  ...colors,
  {name:"ทอง",mul:0.1},
  {name:"เงิน",mul:0.01}
];

const tolerances = [
  {name:"น้ำตาล ±1%",value:1},
  {name:"แดง ±2%",value:2},
  {name:"เขียว ±0.5%",value:0.5},
  {name:"น้ำเงิน ±0.25%",value:0.25},
  {name:"ม่วง ±0.1%",value:0.1},
  {name:"เทา ±0.05%",value:0.05},
  {name:"ทอง ±5%",value:5},
  {name:"เงิน ±10%",value:10}
];

function fillSelect(id,data) {
  let el=document.getElementById(id);
  data.forEach((item,index)=>{
    let option=document.createElement('option');
    option.value=index;
    option.textContent=item.name;
    el.appendChild(option);
  });
}

fillSelect('c1',colors);
fillSelect('c2',colors);
fillSelect('c3',multipliers);
fillSelect('c4',tolerances);

function resistor() {
  let a=colors[Number(document.getElementById('c1').value)].value;
  let b=colors[Number(document.getElementById('c2').value)].value;
  let m=multipliers[Number(document.getElementById('c3').value)].mul;
  let t=tolerances[Number(document.getElementById('c4').value)].value;

  let result=(a*10+b)*m;

  let formatted=result.toLocaleString('en-US',{
    maximumFractionDigits:4
  });

  document.getElementById('resResult').style.display="block";
  document.getElementById('resResult').innerHTML=
    "<b>ค่าความต้านทาน</b><br>"+
    formatted+" Ω<br>"+
    "ค่าความคลาดเคลื่อน ±"+t+"%";
}

// กำลังไฟฟ้า
function powerCalc() {
  let v=Number(document.getElementById('pv').value);
  let i=Number(document.getElementById('pi').value);

  let out=document.getElementById('powerResult');
  out.style.display="block";

  if(document.getElementById('pv').value==="" ||
     document.getElementById('pi').value==="" ||
     !Number.isFinite(v) || !Number.isFinite(i) ||
     v<=0 || i<=0) {
    out.innerHTML="กรุณากรอกแรงดันและกระแสให้ถูกต้อง";
    return;
  }

  let p=v*i;

  out.innerHTML=
    "<b>ผลการคำนวณ</b><br>"+
    "P = V × I<br>"+
    "P = "+v+" × "+i+"<br>"+
    "กำลังไฟฟ้า = "+p.toFixed(2)+" W";
}

// คำนวณค่าไฟ
function energyCalc() {
  let w=Number(document.getElementById('watt').value);
  let h=Number(document.getElementById('hours').value);
  let d=Number(document.getElementById('days').value);
  let rate=Number(document.getElementById('rate').value);

  let out=document.getElementById('energyResult');
  out.style.display="block";

  let inputs=['watt','hours','days','rate'];
  if(inputs.some(id=>document.getElementById(id).value==="") ||
     ![w,h,d,rate].every(Number.isFinite) ||
     w<=0 || h<0 || h>24 || d<=0 || rate<0) {
    out.innerHTML="กรุณาตรวจสอบตัวเลขอีกครั้ง";
    return;
  }

  let kwh=(w*h*d)/1000;
  let cost=kwh*rate;

  out.innerHTML=
    "<b>ผลการคำนวณ</b><br>"+
    "พลังงานไฟฟ้า = "+kwh.toFixed(2)+" kWh<br>"+
    "ค่าไฟประมาณ = "+cost.toFixed(2)+" บาท";
}

</script>
</body>
</html>
