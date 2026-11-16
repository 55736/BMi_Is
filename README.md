<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BentoBalance</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+Thai:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
  :root {
    --sky: #38bdf8;
    --violet: #8b5cf6;
    --mint: #10b981;
    --rose: #f43f5e;
    --ink: #1e293b;
    --muted: #64748b;
    --surface: #ffffff;
    --canvas: #f8fafc;
    --line: #e2e8f0;
  }

  * {
    box-sizing: border-box;
    font-family: 'IBM Plex Sans Thai', sans-serif;
  }

  body {
    margin: 0;
    padding: 24px 16px;
    background: linear-gradient(145deg, #f0f9ff 0%, #f5f3ff 50%, #fff1f2 100%);
    color: var(--ink);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: flex-start;
  }

  .app {
    width: 100%;
    max-width: 680px;
    background: var(--surface);
    border-radius: 20px;
    padding: 28px;
    box-shadow: 0 12px 32px rgba(139, 92, 246, 0.08);
    border: 1px solid rgba(255, 255, 255, 0.8);
  }

  .topbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 16px;
    margin-bottom: 24px;
    border-bottom: 1px solid var(--line);
  }

  .brand {
    font-size: 20px;
    font-weight: 700;
    background: linear-gradient(90deg, var(--sky), var(--violet), var(--rose));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    letter-spacing: -0.5px;
  }

  .user-pill {
    display: flex;
    align-items: center;
    gap: 10px;
    background: #f5f3ff;
    padding: 6px 12px;
    border-radius: 99px;
    font-size: 13px;
    font-weight: 600;
    color: var(--violet);
  }

  .btn-out {
    background: var(--rose);
    color: #fff;
    border: none;
    padding: 4px 10px;
    border-radius: 99px;
    font-size: 12px;
    cursor: pointer;
    width: auto;
    margin: 0;
  }

  h2 {
    margin: 0 0 8px 0;
    font-size: 22px;
    font-weight: 700;
    color: var(--ink);
  }

  .sub {
    margin: 0 0 20px 0;
    font-size: 14px;
    color: var(--muted);
  }

  .field {
    margin-bottom: 16px;
  }

  .row-2 {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
  }

  label {
    display: block;
    font-size: 13px;
    font-weight: 600;
    margin-bottom: 6px;
    color: var(--ink);
  }

  input[type="text"],
  input[type="password"],
  input[type="number"],
  select {
    width: 100%;
    padding: 11px 14px;
    border: 1.5px solid var(--line);
    border-radius: 10px;
    font-size: 14px;
    background: var(--canvas);
    color: var(--ink);
    transition: 0.2s;
  }

  input:focus, select:focus {
    outline: none;
    border-color: var(--violet);
    background: #fff;
  }

  .radio-group {
    display: flex;
    gap: 16px;
    background: var(--canvas);
    padding: 10px 14px;
    border-radius: 10px;
    border: 1.5px solid var(--line);
  }

  .radio-group label {
    margin: 0;
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 6px;
    cursor: pointer;
  }

  .checks {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    background: var(--canvas);
    padding: 12px;
    border-radius: 10px;
    border: 1.5px solid var(--line);
  }

  .checks label {
    margin: 0;
    font-weight: 400;
    display: flex;
    align-items: center;
    gap: 6px;
    cursor: pointer;
  }

  .seg-tabs {
    display: flex;
    background: #f1f5f9;
    padding: 4px;
    border-radius: 12px;
    margin-bottom: 20px;
    gap: 4px;
  }

  .seg-btn {
    flex: 1;
    padding: 9px;
    border: none;
    background: transparent;
    color: var(--muted);
    font-size: 13px;
    font-weight: 600;
    border-radius: 8px;
    cursor: pointer;
    margin: 0;
    transition: 0.2s;
  }

  .seg-btn.active {
    background: #fff;
    color: var(--violet);
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
  }

  .btn {
    width: 100%;
    padding: 13px;
    border: none;
    border-radius: 12px;
    font-size: 15px;
    font-weight: 600;
    color: #fff;
    cursor: pointer;
    margin-top: 12px;
    transition: 0.2s;
  }

  .btn:hover { opacity: 0.92; transform: translateY(-1px); }
  .btn-violet { background: linear-gradient(90deg, var(--violet), #6366f1); }
  .btn-sky { background: linear-gradient(90deg, var(--sky), #0284c7); }
  .btn-mint { background: linear-gradient(90deg, var(--mint), #059669); }
  .btn-rose { background: linear-gradient(90deg, var(--rose), #e11d48); }
  .btn-ghost { background: #f1f5f9; color: var(--ink); }

  .view { display: none; }
  .view.active { display: block; }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
    margin: 14px 0;
    border-radius: 12px;
    overflow: hidden;
    border: 1px solid var(--line);
  }

  th {
    background: #f8fafc;
    color: var(--muted);
    font-weight: 600;
    padding: 10px 8px;
    text-align: center;
    border-bottom: 1px solid var(--line);
  }

  td {
    padding: 10px 8px;
    text-align: center;
    border-bottom: 1px solid var(--line);
  }

  tr:last-child td { border-bottom: none; }

  .stat-banner {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 20px;
  }

  .stat-box {
    padding: 16px;
    border-radius: 14px;
    color: #fff;
  }

  .stat-box small { font-size: 12px; opacity: 0.9; display: block; margin-bottom: 4px; }
  .stat-box strong { font-size: 22px; font-weight: 700; display: block; }
  .stat-box span { font-size: 12px; }

  .bento-frame {
    background: #fff1f2;
    border: 2px solid #fecdd3;
    padding: 14px;
    border-radius: 16px;
    margin: 16px 0;
  }

  .bento-top {
    display: grid;
    grid-template-columns: 1.5fr 1fr;
    gap: 10px;
  }

  .bento-right {
    display: grid;
    grid-template-rows: 1fr 1fr;
    gap: 10px;
  }

  .bento-slot {
    background: #fff;
    padding: 12px;
    border-radius: 12px;
    position: relative;
    border: 1.5px solid var(--line);
  }

  .slot-tag {
    display: inline-block;
    font-size: 11px;
    font-weight: 600;
    padding: 2px 8px;
    border-radius: 6px;
    margin-bottom: 6px;
  }

  .tag-rose { background: #ffe4e6; color: var(--rose); }
  .tag-violet { background: #ede9fe; color: var(--violet); }
  .tag-mint { background: #d1fae5; color: var(--mint); }
  .tag-sky { background: #e0f2fe; color: #0284c7; }

  .slot-title { font-size: 14px; font-weight: 600; margin: 0 0 4px 0; padding-right: 28px; }
  .slot-meta { font-size: 12px; color: var(--muted); margin: 0; }

  .btn-spin {
    position: absolute;
    bottom: 10px;
    right: 10px;
    width: 28px;
    height: 28px;
    border-radius: 8px;
    border: 1px solid var(--line);
    background: var(--canvas);
    cursor: pointer;
    font-size: 13px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0;
    padding: 0;
  }

  .chart-wrap {
    background: #fff;
    border: 1.5px solid var(--line);
    border-radius: 14px;
    padding: 16px;
    margin-bottom: 18px;
    height: 290px;
    position: relative;
  }

  .monthly-summary {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-bottom: 16px;
  }

  .m-card {
    background: var(--canvas);
    border: 1px solid var(--line);
    border-radius: 12px;
    padding: 12px;
    text-align: center;
  }

  .m-card small { font-size: 11px; color: var(--muted); display: block; }
  .m-card strong { font-size: 18px; color: var(--violet); }

  .me-row { background: #f5f3ff; font-weight: 600; }
  .tip-list { margin: 8px 0 0 0; padding-left: 18px; font-size: 13px; color: var(--muted); line-height: 1.6; }
</style>
</head>
<body>

<div class="app">
  <div class="topbar">
    <div class="brand">BentoBalance</div>
    <div id="userPill" class="user-pill" style="display:none;">
      <span id="navUser">User</span>
      <button class="btn-out" onclick="doLogout()">ออก</button>
    </div>
  </div>

  <!-- Login / Register -->
  <div id="vAuth" class="view active">
    <h2>เข้าสู่ระบบ</h2>
    <p class="sub">บันทึกค่า BMI รายวัน ดูกราฟสถิติ และจัดเซ็ตอาหารที่เหมาะกับร่างกาย</p>

    <div class="seg-tabs">
      <button type="button" class="seg-btn active" id="tLogin" onclick="setAuthMode('login')">เข้าสู่ระบบ</button>
      <button type="button" class="seg-btn" id="tReg" onclick="setAuthMode('register')">สร้างบัญชีใหม่</button>
    </div>

    <div class="field">
      <label>ชื่อผู้ใช้งาน</label>
      <input type="text" id="inUser" placeholder="ระบุชื่อผู้ใช้">
    </div>
    <div class="field">
      <label>รหัสผ่าน</label>
      <input type="password" id="inPass" placeholder="ระบุรหัสผ่าน">
    </div>

    <button class="btn btn-violet" id="btnAuth" onclick="submitAuth()">เข้าสู่ระบบ</button>
  </div>

  <!-- Home / Overview -->
  <div id="vHome" class="view">
    <h2>เกณฑ์มาตรฐาน BMI</h2>
    <p class="sub">ตารางอ้างอิงสัดส่วนร่างกายแยกตามสรีระเพศชายและหญิง</p>

    <table>
      <thead>
        <tr>
          <th>เกณฑ์</th>
          <th>ชาย</th>
          <th>หญิง</th>
          <th>คำแนะนำ</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td style="color:#eab308; font-weight:600;">น้ำหนักน้อย</td>
          <td>&lt; 20.0</td>
          <td>&lt; 18.5</td>
          <td>เพิ่มโปรตีนและพลังงาน</td>
        </tr>
        <tr>
          <td style="color:var(--mint); font-weight:600;">สมส่วน</td>
          <td>20.0 - 24.9</td>
          <td>18.5 - 23.9</td>
          <td>รักษาสมดุลต่อเนื่อง</td>
        </tr>
        <tr>
          <td style="color:#f97316; font-weight:600;">น้ำหนักเกิน</td>
          <td>25.0 - 29.9</td>
          <td>24.0 - 28.9</td>
          <td>คุมคาร์บและน้ำตาล</td>
        </tr>
        <tr>
          <td style="color:var(--rose); font-weight:600;">โรคอ้วน</td>
          <td>&ge; 30.0</td>
          <td>&ge; 29.0</td>
          <td>ปรับโภชนาการเคร่งครัด</td>
        </tr>
      </tbody>
    </table>

    <button class="btn btn-violet" onclick="go('vForm')">คำนวณ BMI และจัดกล่องข้าว Bento</button>
    <button class="btn btn-sky" onclick="openStats()">ดูกราฟสถิติ BMI (รายวัน / สัปดาห์ / เดือน)</button>
    <button class="btn btn-mint" onclick="openRank()">อันดับสุขภาพ (Leaderboard)</button>
  </div>

  <!-- Form -->
  <div id="vForm" class="view">
    <h2>ข้อมูลร่างกาย</h2>
    <p class="sub">กรอกข้อมูลปัจจุบันเพื่อบันทึกสถิติและคำนวณปริมาณอาหารแต่ละมื้อ</p>

    <div class="field">
      <label>เพศ</label>
      <div class="radio-group">
        <label><input type="radio" name="gender" value="ชาย" checked> ชาย</label>
        <label><input type="radio" name="gender" value="หญิง"> หญิง</label>
      </div>
    </div>

    <div class="row-2">
      <div class="field">
        <label>น้ำหนัก (กก.)</label>
        <input type="number" id="inWeight" value="60" step="0.1">
      </div>
      <div class="field">
        <label>ส่วนสูง (ซม.)</label>
        <input type="number" id="inHeight" value="165" step="0.1">
      </div>
    </div>

    <div class="field">
      <label>ข้อจำกัดด้านการแพ้อาหาร</label>
      <div class="checks">
        <label><input type="checkbox" class="alg" value="seafood"> อาหารทะเล</label>
        <label><input type="checkbox" class="alg" value="nut"> ถั่วทุกชนิด</label>
        <label><input type="checkbox" class="alg" value="egg"> ไข่</label>
        <label><input type="checkbox" class="alg" value="milk"> นมวัว / แลคโตส</label>
        <label><input type="checkbox" class="alg" value="wheat"> แป้งสาลี / กลูเตน</label>
      </div>
    </div>

    <button class="btn btn-violet" onclick="runCalc()">บันทึกและวิเคราะห์ผล</button>
    <button class="btn btn-ghost" onclick="go('vHome')">ย้อนกลับ</button>
  </div>

  <!-- Result -->
  <div id="vResult" class="view">
    <h2>ผลวิเคราะห์และจัดเซ็ตอาหาร</h2>
    <p class="sub">ระบบบันทึกค่า BMI ล่าสุดลงในประวัติเรียบร้อยแล้ว</p>

    <div class="stat-banner">
      <div class="stat-box" id="boxBmi">
        <small>ดัชนีมวลกาย (BMI)</small>
        <strong id="valBmi">0.00</strong>
        <span id="txtBmi">-</span>
      </div>
      <div class="stat-box" style="background: linear-gradient(135deg, var(--sky), var(--violet));">
        <small>พลังงานที่ควรได้รับ/วัน</small>
        <strong id="valCal">0 kcal</strong>
        <span>แบ่งสัดส่วน 3 มื้อหลัก</span>
      </div>
    </div>

    <label>ปริมาณอาหารที่แนะนำในแต่ละมื้อ</label>
    <table>
      <thead>
        <tr>
          <th>มื้ออาหาร</th>
          <th>พลังงาน</th>
          <th>คาร์บ</th>
          <th>โปรตีน</th>
          <th>ผัก</th>
        </tr>
      </thead>
      <tbody id="tbMeals"></tbody>
    </table>

    <label style="margin-top:18px;">ตัวอย่างการจัดกล่องข้าว Bento (1 มื้อหลัก)</label>
    <div class="bento-frame">
      <div class="bento-top">
        <div class="bento-slot">
          <span class="slot-tag tag-rose">จานหลัก</span>
          <p class="slot-title" id="fMain"></p>
          <p class="slot-meta" id="wMain"></p>
          <button class="btn-spin" onclick="spinSlot('main')">↻</button>
        </div>
        <div class="bento-right">
          <div class="bento-slot">
            <span class="slot-tag tag-violet">เครื่องเคียง</span>
            <p class="slot-title" id="fSide"></p>
            <p class="slot-meta" id="wSide"></p>
            <button class="btn-spin" onclick="spinSlot('side')">↻</button>
          </div>
          <div class="bento-slot">
            <span class="slot-tag tag-mint">ผักและไฟเบอร์</span>
            <p class="slot-title" id="fVeg"></p>
            <p class="slot-meta" id="wVeg"></p>
            <button class="btn-spin" onclick="spinSlot('veg')">↻</button>
          </div>
        </div>
      </div>
      <div class="bento-slot" style="margin-top:10px;">
        <span class="slot-tag tag-sky">ผลไม้ / ของว่าง</span>
        <p class="slot-title" id="fSnack"></p>
        <p class="slot-meta" id="wSnack"></p>
        <button class="btn-spin" onclick="spinSlot('snack')">↻</button>
      </div>
    </div>

    <div style="background:var(--canvas); padding:14px; border-radius:12px; border:1px solid var(--line); margin-bottom:14px;">
      <label style="margin:0;">แนวทางดูแลตัวเอง</label>
      <ul class="tip-list" id="ulTips"></ul>
    </div>

    <div class="row-2">
      <button class="btn btn-sky" style="margin-top:0;" onclick="spinAll()">สุ่มเมนูใหม่ทั้งหมด</button>
      <button class="btn btn-mint" style="margin-top:0;" onclick="findMap()">ค้นหาร้านใกล้ฉัน</button>
    </div>
    <button class="btn btn-violet" onclick="openStats()">ดูกราฟสถิติ BMI</button>
    <button class="btn btn-ghost" onclick="go('vHome')">กลับหน้าหลัก</button>
  </div>

  <!-- Stats & Charts -->
  <div id="vStats" class="view">
    <h2>สถิติแนวโน้ม BMI</h2>
    <p class="sub">ติดตามความเปลี่ยนแปลงแบบรายวัน รายสัปดาห์ และค่าเฉลี่ยรายเดือน</p>

    <div class="seg-tabs">
      <button type="button" class="seg-btn active" id="tabDay" onclick="switchChart('daily')">กราฟเส้นรายวัน</button>
      <button type="button" class="seg-btn" id="tabWeek" onclick="switchChart('weekly')">กราฟแท่งรายสัปดาห์</button>
      <button type="button" class="seg-btn" id="tabMonth" onclick="switchChart('monthly')">ค่าเฉลี่ยรายเดือน</button>
    </div>

    <div id="monthSummaryBox" class="monthly-summary" style="display:none;">
      <div class="m-card">
        <small>ค่าเฉลี่ยเดือนนี้</small>
        <strong id="mAvg">-</strong>
      </div>
      <div class="m-card">
        <small>ค่าต่ำสุด</small>
        <strong id="mMin" style="color:var(--mint);">-</strong>
      </div>
      <div class="m-card">
        <small>ค่าสูงสุด</small>
        <strong id="mMax" style="color:var(--rose);">-</strong>
      </div>
    </div>

    <div class="chart-wrap">
      <canvas id="bmiChart"></canvas>
    </div>

    <label>ประวัติการบันทึกล่าสุด</label>
    <table>
      <thead>
        <tr>
          <th>วันที่</th>
          <th>น้ำหนัก</th>
          <th>ส่วนสูง</th>
          <th>BMI</th>
        </tr>
      </thead>
      <tbody id="tbHist"></tbody>
    </table>

    <button class="btn btn-rose" onclick="wipeHistory()">ลบประวัติทั้งหมด</button>
    <button class="btn btn-ghost" onclick="go('vHome')">กลับหน้าหลัก</button>
  </div>

  <!-- Leaderboard -->
  <div id="vRank" class="view">
    <h2>อันดับความสมส่วน</h2>
    <p class="sub">จัดอันดับจากความใกล้เคียงเกณฑ์มาตรฐานสุขภาพของผู้ใช้งานในระบบ</p>

    <table>
      <thead>
        <tr>
          <th>อันดับ</th>
          <th>ผู้ใช้งาน</th>
          <th>BMI ล่าสุด</th>
          <th>สถานะ</th>
        </tr>
      </thead>
      <tbody id="tbRank"></tbody>
    </table>

    <button class="btn btn-ghost" onclick="go('vHome')">กลับหน้าหลัก</button>
  </div>
</div>

<script>
let authMode = 'login';
let activeUser = null;
let chartObj = null;
let currentMainMenu = '';
let weightRules = {};
let userAllergies = [];

const menuPool = {
  main: [
    { name: "ข้าวกล้อง + อกไก่ย่างพริกไทยดำ", tags: [] },
    { name: "ข้าวไรซ์เบอร์รี่ + ปลากะพงนึ่งมะนาว", tags: ["seafood"] },
    { name: "เส้นหมี่ข้าวกล้อง + ปลาลวกจิ้มแจ่ว", tags: ["seafood", "wheat"] },
    { name: "ข้าวกล้อง + ลาบอกไก่คั่วแห้ง", tags: [] },
    { name: "ข้าวไรซ์เบอร์รี่ + ไก่ผัดเม็ดมะม่วง", tags: ["nut"] },
    { name: "ข้าวกล้อง + แซลมอนย่างเกลือ", tags: ["seafood"] },
    { name: "ข้าวไรซ์เบอร์รี่ + สันในหมูอบน้ำผึ้ง", tags: [] }
  ],
  side: [
    { name: "ไข่ต้มยางมะตูม 1 ฟอง", tags: ["egg"] },
    { name: "เต้าหู้ขาวนึ่งซีอิ๊วญี่ปุ่น", tags: [] },
    { name: "ไข่ตุ๋นทรงเครื่องอกไก่สับ", tags: ["egg"] },
    { name: "เต้าหู้เหลืองผัดถั่วงอก", tags: [] }
  ],
  veg: [
    { name: "บล็อคโคลี่และแครอทลวก", tags: [] },
    { name: "หน่อไม้ฝรั่งผัดเห็ดออรินจิ", tags: [] },
    { name: "กะหล่ำปลีตุ๋นเห็ดหอม", tags: [] }
  ],
  snack: [
    { name: "แอปเปิ้ลเขียว 1 ลูก", tags: [] },
    { name: "ฝรั่งกิมจูหั่นชิ้น", tags: [] },
    { name: "แก้วมังกรเนื้อขาว", tags: [] },
    { name: "โยเกิร์ตธรรมชาติ 0%", tags: ["milk"] }
  ]
};

window.addEventListener('DOMContentLoaded', () => {
  const saved = localStorage.getItem('bb_session');
  if (saved) {
    activeUser = saved;
    document.getElementById('navUser').innerText = activeUser;
    document.getElementById('userPill').style.display = 'flex';
    go('vHome');
  }
});

function go(id) {
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0, 0);
}

function setAuthMode(mode) {
  authMode = mode;
  document.getElementById('tLogin').classList.toggle('active', mode === 'login');
  document.getElementById('tReg').classList.toggle('active', mode === 'register');
  document.getElementById('btnAuth').innerText = mode === 'login' ? 'เข้าสู่ระบบ' : 'สร้างบัญชีใหม่';
}

function submitAuth() {
  const u = document.getElementById('inUser').value.trim();
  const p = document.getElementById('inPass').value.trim();
  if (!u || !p) { alert('กรุณากรอกข้อมูลให้ครบ'); return; }

  const db = JSON.parse(localStorage.getItem('bb_users') || '{}');
  if (authMode === 'register') {
    if (db[u]) { alert('ชื่อผู้ใช้นี้ถูกใช้ไปแล้ว'); return; }
    db[u] = { pass: p, logs: [] };
    localStorage.setItem('bb_users', JSON.stringify(db));
    startSession(u);
  } else {
    if (!db[u] || db[u].pass !== p) { alert('ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง'); return; }
    startSession(u);
  }
}

function startSession(u) {
  activeUser = u;
  localStorage.setItem('bb_session', u);
  document.getElementById('navUser').innerText = u;
  document.getElementById('userPill').style.display = 'flex';
  document.getElementById('inUser').value = '';
  document.getElementById('inPass').value = '';
  go('vHome');
}

function doLogout() {
  activeUser = null;
  localStorage.removeItem('bb_session');
  document.getElementById('userPill').style.display = 'none';
  go('vAuth');
}

function runCalc() {
  const gender = document.querySelector('input[name="gender"]:checked').value;
  const w = parseFloat(document.getElementById('inWeight').value);
  const h = parseFloat(document.getElementById('inHeight').value);
  if (!w || !h) { alert('กรุณากรอกน้ำหนักและส่วนสูง'); return; }

  const bmi = w / ((h / 100) ** 2);
  saveLog(w, h, bmi, gender);

  let status = '', bg = '', tips = [], tdee = 0;

  if (gender === 'ชาย') {
    tdee = w * 24 * 1.2;
    if (bmi < 20.0) {
      status = 'น้ำหนักน้อยกว่าเกณฑ์'; bg = 'linear-gradient(135deg, #f59e0b, #eab308)';
      weightRules = { main: 'คาร์บ 180g + เนื้อ 150g', side: 'โปรตีน 90g', veg: 'ผัก 120g', snack: '90g' };
      tips = ['เพิ่มปริมาณโปรตีนในทุกมื้ออาหาร', 'ฝึกเวทเทรนนิ่งเพื่อสร้างมวลกล้ามเนื้อ'];
    } else if (bmi < 25.0) {
      status = 'สมส่วน สุขภาพดี'; bg = 'linear-gradient(135deg, #10b981, #059669)';
      weightRules = { main: 'คาร์บ 150g + เนื้อ 130g', side: 'โปรตีน 70g', veg: 'ผัก 120g', snack: '80g' };
      tips = ['รักษาสัดส่วนสารอาหารให้คงที่', 'ออกกำลังกายแบบคาร์ดิโอ 150 นาทีต่อสัปดาห์'];
    } else if (bmi < 30.0) {
      status = 'น้ำหนักเกินเกณฑ์'; bg = 'linear-gradient(135deg, #f97316, #ea580c)';
      weightRules = { main: 'คาร์บ 100g + เนื้อ 130g', side: 'โปรตีน 60g', veg: 'ผัก 160g', snack: '50g' };
      tips = ['ลดอาหารทอดและเครื่องดื่มที่มีน้ำตาล', 'เพิ่มสัดส่วนผักใบเขียวในมื้อเย็น'];
    } else {
      status = 'ภาวะโรคอ้วน'; bg = 'linear-gradient(135deg, #f43f5e, #e11d48)';
      weightRules = { main: 'คาร์บ 70g + เนื้อ 140g', side: 'โปรตีน 60g', veg: 'ผัก 200g', snack: '50g' };
      tips = ['หลีกเลี่ยงแป้งขัดขาวและน้ำหวานเด็ดขาด', 'เริ่มออกกำลังกายด้วยการเดินเร็วหรือว่ายน้ำ'];
    }
  } else {
    tdee = w * 22 * 1.2;
    if (bmi < 18.5) {
      status = 'น้ำหนักน้อยกว่าเกณฑ์'; bg = 'linear-gradient(135deg, #f59e0b, #eab308)';
      weightRules = { main: 'คาร์บ 140g + เนื้อ 110g', side: 'โปรตีน 70g', veg: 'ผัก 100g', snack: '70g' };
      tips = ['เน้นอาหารที่ให้พลังงานและธาตุเหล็กสูง', 'เสริมไขมันดีจากถั่วและอะโวคาโด'];
    } else if (bmi < 24.0) {
      status = 'สมส่วน สุขภาพดี'; bg = 'linear-gradient(135deg, #10b981, #059669)';
      weightRules = { main: 'คาร์บ 120g + เนื้อ 100g', side: 'โปรตีน 50g', veg: 'ผัก 100g', snack: '60g' };
      tips = ['ควบคุมปริมาณน้ำตาลและโซเดียม', 'ออกกำลังกายแบบเวทบอดี้เวทเพื่อความกระชับ'];
    } else if (bmi < 29.0) {
      status = 'น้ำหนักเกินเกณฑ์'; bg = 'linear-gradient(135deg, #f97316, #ea580c)';
      weightRules = { main: 'คาร์บ 80g + เนื้อ 100g', side: 'โปรตีน 40g', veg: 'ผัก 140g', snack: '40g' };
      tips = ['งดเครื่องดื่มชานมและเบเกอรี่', 'เน้นการออกกำลังกายแบบแอโรบิกต่อเนื่อง'];
    } else {
      status = 'ภาวะโรคอ้วน'; bg = 'linear-gradient(135deg, #f43f5e, #e11d48)';
      weightRules = { main: 'คาร์บ 50g + เนื้อ 110g', side: 'โปรตีน 40g', veg: 'ผัก 180g', snack: '30g' };
      tips = ['ปรับลดปริมาณคาร์โบไฮเดรตลงครึ่งหนึ่ง', 'เดินออกกำลังกายสม่ำเสมอทุกวัน'];
    }
  }

  document.getElementById('boxBmi').style.background = bg;
  document.getElementById('valBmi').innerText = bmi.toFixed(2);
  document.getElementById('txtBmi').innerText = `${status} (${gender})`;

  const totalKcal = Math.round(tdee);
  document.getElementById('valCal').innerText = `${totalKcal} kcal`;

  const splits = [
    { label: 'มื้อเช้า (30%)', r: 0.30 },
    { label: 'มื้อกลางวัน (40%)', r: 0.40 },
    { label: 'มื้อเย็น (30%)', r: 0.30 }
  ];

  document.getElementById('tbMeals').innerHTML = splits.map(s => {
    const kcal = Math.round(totalKcal * s.r);
    const c = Math.round((kcal * 0.45) / 4);
    const p = Math.round((kcal * 0.30) / 4);
    const v = Math.round(s.r * (gender === 'ชาย' ? 400 : 320));
    return `<tr>
      <td style="text-align:left; font-weight:500;">${s.label}</td>
      <td style="color:var(--violet); font-weight:600;">${kcal} kcal</td>
      <td>${c}g</td>
      <td>${p}g</td>
      <td style="color:var(--mint);">${v}g</td>
    </tr>`;
  }).join('');

  userAllergies = Array.from(document.querySelectorAll('.alg:checked')).map(el => el.value);
  document.getElementById('ulTips').innerHTML = tips.map(t => `<li>${t}</li>`).join('');

  spinAll();
  go('vResult');
}

function spinSlot(type) {
  const list = menuPool[type].filter(item => !item.tags.some(t => userAllergies.includes(t)));
  const safeList = list.length ? list : [{ name: 'ข้าวกล้อง + ผักนึ่งรวมมิตร', tags: [] }];
  const picked = safeList[Math.floor(Math.random() * safeList.length)].name;

  if (type === 'main') {
    currentMainMenu = picked;
    document.getElementById('fMain').innerText = picked;
    document.getElementById('wMain').innerText = weightRules.main;
  } else if (type === 'side') {
    document.getElementById('fSide').innerText = picked;
    document.getElementById('wSide').innerText = weightRules.side;
  } else if (type === 'veg') {
    document.getElementById('fVeg').innerText = picked;
    document.getElementById('wVeg').innerText = weightRules.veg;
  } else if (type === 'snack') {
    document.getElementById('fSnack').innerText = picked;
    document.getElementById('wSnack').innerText = weightRules.snack;
  }
}

function spinAll() {
  ['main', 'side', 'veg', 'snack'].forEach(spinSlot);
}

function findMap() {
  const q = currentMainMenu ? (currentMainMenu.split('+')[1] || currentMainMenu) : 'อาหารคลีน';
  window.open(`https://www.google.com/maps/search/${encodeURIComponent(q.trim())}+ใกล้ฉัน`, '_blank');
}

function saveLog(w, h, bmi, gender) {
  if (!activeUser) return;
  const db = JSON.parse(localStorage.getItem('bb_users') || '{}');
  if (!db[activeUser]) return;

  const now = new Date();
  const isoDate = now.toISOString().split('T')[0];
  const displayDate = `${now.getDate()}/${now.getMonth() + 1}/${now.getFullYear()}`;

  db[activeUser].logs.push({
    ts: now.getTime(),
    iso: isoDate,
    date: displayDate,
    w: w,
    h: h,
    bmi: parseFloat(bmi.toFixed(2)),
    gender: gender
  });
  localStorage.setItem('bb_users', JSON.stringify(db));
}

function openStats() {
  renderHistoryTable();
  switchChart('daily');
  go('vStats');
}

function renderHistoryTable() {
  const db = JSON.parse(localStorage.getItem('bb_users') || '{}');
  const logs = (db[activeUser] && db[activeUser].logs) ? db[activeUser].logs : [];
  const tb = document.getElementById('tbHist');

  if (!logs.length) {
    tb.innerHTML = '<tr><td colspan="4" style="color:var(--muted);">ยังไม่มีประวัติการบันทึก</td></tr>';
    return;
  }

  tb.innerHTML = logs.slice().reverse().map(l => `
    <tr>
      <td>${l.date}</td>
      <td>${l.w} กก.</td>
      <td>${l.h} ซม.</td>
      <td style="font-weight:600; color:var(--violet);">${l.bmi}</td>
    </tr>
  `).join('');
}

function switchChart(mode) {
  document.getElementById('tabDay').classList.toggle('active', mode === 'daily');
  document.getElementById('tabWeek').classList.toggle('active', mode === 'weekly');
  document.getElementById('tabMonth').classList.toggle('active', mode === 'monthly');
  document.getElementById('monthSummaryBox').style.display = mode === 'monthly' ? 'grid' : 'none';

  const db = JSON.parse(localStorage.getItem('bb_users') || '{}');
  const logs = (db[activeUser] && db[activeUser].logs) ? db[activeUser].logs : [];

  const ctx = document.getElementById('bmiChart').getContext('2d');
  if (chartObj) chartObj.destroy();

  let labels = [];
  let values = [];
  let chartType = 'line';
  let datasetLabel = 'BMI รายวัน';
  let colorMain = '#8b5cf6';
  let bgFill = 'rgba(139, 92, 246, 0.12)';

  if (mode === 'daily') {
    const recent = logs.slice(-14);
    labels = recent.map(l => l.date);
    values = recent.map(l => l.bmi);
    chartType = 'line';
    datasetLabel = 'BMI รายวัน';
  } else if (mode === 'weekly') {
    const weekMap = {};
    logs.forEach(l => {
      const d = new Date(l.ts);
      const firstDay = new Date(d.setDate(d.getDate() - d.getDay()));
      const key = `สัปดาห์ ${firstDay.getDate()}/${firstDay.getMonth() + 1}`;
      if (!weekMap[key]) weekMap[key] = [];
      weekMap[key].push(l.bmi);
    });
    labels = Object.keys(weekMap).slice(-8);
    values = labels.map(k => {
      const arr = weekMap[k];
      return parseFloat((arr.reduce((a, b) => a + b, 0) / arr.length).toFixed(2));
    });
    chartType = 'bar';
    datasetLabel = 'ค่าเฉลี่ย BMI รายสัปดาห์';
    colorMain = '#38bdf8';
    bgFill = 'rgba(56, 189, 248, 0.75)';
  } else if (mode === 'monthly') {
    const monthNames = ['ม.ค.', 'ก.พ.', 'มี.ค.', 'เม.ย.', 'พ.ค.', 'มิ.ย.', 'ก.ค.', 'ส.ค.', 'ก.ย.', 'ต.ค.', 'พ.ย.', 'ธ.ค.'];
    const monthMap = {};
    logs.forEach(l => {
      const d = new Date(l.ts);
      const key = `${monthNames[d.getMonth()]} ${d.getFullYear() + 543}`;
      if (!monthMap[key]) monthMap[key] = [];
      monthMap[key].push(l.bmi);
    });
    labels = Object.keys(monthMap);
    values = labels.map(k => {
      const arr = monthMap[k];
      return parseFloat((arr.reduce((a, b) => a + b, 0) / arr.length).toFixed(2));
    });
    chartType = 'line';
    datasetLabel = 'ค่าเฉลี่ย BMI รายเดือน';
    colorMain = '#f43f5e';
    bgFill = 'rgba(244, 63, 94, 0.12)';

    if (values.length > 0) {
      const currentMonthKey = labels[labels.length - 1];
      const currentArr = monthMap[currentMonthKey];
      const avg = (currentArr.reduce((a, b) => a + b, 0) / currentArr.length).toFixed(2);
      document.getElementById('mAvg').innerText = avg;
      document.getElementById('mMin').innerText = Math.min(...currentArr).toFixed(2);
      document.getElementById('mMax').innerText = Math.max(...currentArr).toFixed(2);
    } else {
      document.getElementById('mAvg').innerText = '-';
      document.getElementById('mMin').innerText = '-';
      document.getElementById('mMax').innerText = '-';
    }
  }

  const minVal = values.length ? Math.min(...values) : 18;
  const maxVal = values.length ? Math.max(...values) : 25;
  let yMin = Math.floor((minVal - 0.6) * 10) / 10;
  let yMax = Math.ceil((maxVal + 0.6) * 10) / 10;
  if (yMin === yMax) { yMin -= 1; yMax += 1; }

  chartObj = new Chart(ctx, {
    type: chartType,
    data: {
      labels: labels,
      datasets: [{
        label: datasetLabel,
        data: values,
        borderColor: colorMain,
        backgroundColor: bgFill,
        borderWidth: 2.5,
        borderRadius: chartType === 'bar' ? 8 : 0,
        fill: chartType === 'line',
        tension: 0.25,
        pointRadius: 5,
        pointBackgroundColor: '#fff',
        pointBorderColor: colorMain,
        pointBorderWidth: 2
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { labels: { font: { family: 'IBM Plex Sans Thai', size: 12 } } }
      },
      scales: {
        y: {
          min: yMin,
          max: yMax,
          ticks: { stepSize: 0.2, font: { family: 'IBM Plex Sans Thai', size: 11 } },
          grid: { color: '#f1f5f9' }
        },
        x: {
          ticks: { font: { family: 'IBM Plex Sans Thai', size: 11 } },
          grid: { display: false }
        }
      }
    }
  });
}

function wipeHistory() {
  if (!activeUser) return;
  if (confirm('ยืนยันการลบประวัติ BMI ทั้งหมดของคุณ?')) {
    const db = JSON.parse(localStorage.getItem('bb_users') || '{}');
    if (db[activeUser]) {
      db[activeUser].logs = [];
      localStorage.setItem('bb_users', JSON.stringify(db));
      openStats();
    }
  }
}

function openRank() {
  const db = JSON.parse(localStorage.getItem('bb_users') || '{}');
  const list = [];

  Object.keys(db).forEach(u => {
    const logs = db[u].logs || [];
    if (logs.length > 0) {
      const last = logs[logs.length - 1];
      const ideal = last.gender === 'ชาย' ? 22.5 : 21.2;
      const score = Math.max(0, 100 - Math.abs(last.bmi - ideal) * 8);

      let st = 'สมส่วน', col = 'var(--mint)';
      if (last.gender === 'ชาย') {
        if (last.bmi < 20) { st = 'น้ำหนักน้อย'; col = '#eab308'; }
        else if (last.bmi >= 25 && last.bmi < 30) { st = 'น้ำหนักเกิน'; col = '#f97316'; }
        else if (last.bmi >= 30) { st = 'โรคอ้วน'; col = 'var(--rose)'; }
      } else {
        if (last.bmi < 18.5) { st = 'น้ำหนักน้อย'; col = '#eab308'; }
        else if (last.bmi >= 24 && last.bmi < 29) { st = 'น้ำหนักเกิน'; col = '#f97316'; }
        else if (last.bmi >= 29) { st = 'โรคอ้วน'; col = 'var(--rose)'; }
      }

      list.push({ user: u, bmi: last.bmi, score: score, st: st, col: col });
    }
  });

  list.sort((a, b) => b.score - a.score);
  const tb = document.getElementById('tbRank');

  if (!list.length) {
    tb.innerHTML = '<tr><td colspan="4" style="color:var(--muted);">ยังไม่มีข้อมูลผู้ใช้งานในระบบ</td></tr>';
  } else {
    tb.innerHTML = list.map((item, i) => `
      <tr class="${item.user === activeUser ? 'me-row' : ''}">
        <td>${i + 1}</td>
        <td>${item.user} ${item.user === activeUser ? '(คุณ)' : ''}</td>
        <td>${item.bmi}</td>
        <td style="color:${item.col}; font-weight:600;">${item.st}</td>
      </tr>
    `).join('');
  }

  go('vRank');
}
</script>
</body>
</html>
