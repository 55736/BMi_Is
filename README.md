<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BentoBalance - ระบบคำนวณโภชนาการและสุขภาพ</title>
    <!-- นำเข้าฟอนต์ Google Fonts (Kanit & Prompt) เพื่อความสวยงามไม่ซ้ำใคร -->
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600&family=Prompt:wght@300;400;500&display=swap" rel="stylesheet">
    <!-- นำเข้า Chart.js สำหรับวาดกราฟ -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            /* โทนสีใหม่: ฟ้า, ม่วง, เขียว, ชมพู แบบพาสเทลพรีเมียม */
            --primary-blue: #4a90e2;
            --accent-purple: #9b59b6;
            --accent-pink: #ff6b81;
            --accent-green: #2ed573;
            --gradient-main: linear-gradient(135deg, #6c5ce7 0%, #a29bfe 50%, #fd79a8 100%);
            --gradient-card: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            --bg-light: #f7f9fc;
            --text-main: #2f3542;
            --card-bg: #ffffff;
        }

        body {
            font-family: 'Kanit', 'Prompt', sans-serif;
            background-color: var(--bg-light);
            color: var(--text-main);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            background-color: var(--card-bg);
            padding: 35px;
            border-radius: 24px;
            box-shadow: 0 10px 30px rgba(108, 92, 231, 0.1);
            max-width: 650px;
            width: 100%;
            position: relative;
            box-sizing: border-box;
            border: 1px solid rgba(162, 155, 254, 0.2);
        }

        h1, h2, h3, h4 {
            text-align: center;
            color: #2d3436;
            font-weight: 600;
            margin-bottom: 20px;
        }

        h1 {
            background: var(--gradient-main);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-size: 28px;
            padding-bottom: 15px;
            border-bottom: 2px solid #f1f2f6;
        }

        .user-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: linear-gradient(90deg, rgba(74, 144, 226, 0.1), rgba(155, 89, 182, 0.1));
            padding: 12px 18px;
            border-radius: 12px;
            margin-bottom: 20px;
            font-size: 14px;
            font-weight: 500;
            color: var(--accent-purple);
            border: 1px solid rgba(155, 89, 182, 0.2);
        }

        .btn-logout {
            background-color: #ff4757;
            color: white;
            border: none;
            padding: 6px 14px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 12px;
            font-family: 'Kanit', sans-serif;
            width: auto !important;
            margin-top: 0 !important;
            transition: all 0.2s;
        }
        .btn-logout:hover { background-color: #ff6b81; transform: translateY(-1px); }

        .form-group { margin-bottom: 18px; }
        .grid-2 { display: grid; grid-template-columns: 1fr 1fr; grid-gap: 15px; }
        
        label {
            display: block;
            font-weight: 500;
            margin-bottom: 8px;
            color: #57606f;
            font-size: 14px;
        }

        input[type="number"], input[type="text"], input[type="password"], select {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #dfe4ea;
            border-radius: 12px;
            box-sizing: border-box;
            font-size: 14px;
            font-family: 'Kanit', sans-serif;
            transition: all 0.3s;
            background-color: #f8f9fa;
        }

        input:focus, select:focus {
            border-color: var(--primary-blue);
            background-color: #fff;
            outline: none;
            box-shadow: 0 0 0 4px rgba(74, 144, 226, 0.15);
        }
        
        .gender-box {
            display: flex;
            gap: 20px;
            background: #f8f9fa;
            padding: 12px 18px;
            border-radius: 12px;
            border: 2px solid #dfe4ea;
        }
        .gender-box label { margin-bottom: 0; cursor: pointer; font-weight: 400; color: #2f3542; }

        .method-tabs {
            display: flex;
            margin-bottom: 20px;
            background: #f1f2f6;
            border-radius: 12px;
            padding: 4px;
        }
        .tab-btn {
            flex: 1;
            background: none;
            color: #747d8c;
            padding: 10px;
            font-size: 14px;
            font-family: 'Kanit', sans-serif;
            margin: 0;
            border-radius: 10px;
            border: none;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.3s;
        }
        .tab-btn.active {
            background: white;
            color: var(--accent-purple);
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }

        button {
            width: 100%;
            background: var(--gradient-main);
            color: white;
            border: none;
            padding: 14px;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 500;
            font-family: 'Kanit', sans-serif;
            cursor: pointer;
            margin-top: 15px;
            transition: all 0.3s;
            box-shadow: 0 4px 15px rgba(108, 92, 231, 0.3);
        }
        button:hover { opacity: 0.9; transform: translateY(-2px); box-shadow: 0 6px 20px rgba(108, 92, 231, 0.4); }

        .btn-reset-all { background: linear-gradient(135deg, #ffa502 0%, #ff7f50 100%) !important; box-shadow: 0 4px 15px rgba(255, 165, 2, 0.3); }
        .btn-maps { background: linear-gradient(135deg, var(--accent-green) 0%, #10ac84 100%) !important; box-shadow: 0 4px 15px rgba(46, 213, 115, 0.3); }
        .btn-back { background: linear-gradient(135deg, #747d8c 0%, #2f3542 100%) !important; box-shadow: 0 4px 15px rgba(116, 125, 140, 0.3); }
        .btn-leaderboard { background: linear-gradient(135deg, #f39c12 0%, #e67e22 100%) !important; box-shadow: 0 4px 15px rgba(243, 156, 18, 0.3); }

        .info-table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 0;
            margin-top: 15px;
            font-size: 14px;
            border-radius: 12px;
            overflow: hidden;
            border: 1px solid #dfe4ea;
        }
        .info-table th, .info-table td {
            padding: 12px;
            text-align: center;
        }
        .info-table th { background: linear-gradient(135deg, var(--primary-blue), var(--accent-purple)); color: white; font-weight: 500; }
        .info-table td { border-bottom: 1px solid #f1f2f6; border-right: 1px solid #f1f2f6; background-color: #fff; }
        .info-table tr:last-child td { border-bottom: none; }

        .choice-section {
            margin-top: 25px;
            padding-top: 20px;
            border-top: 2px dashed #dfe4ea;
        }

        .screen { display: none !important; }
        .screen.active { display: block !important; }

        .bmi-badge { padding: 18px; border-radius: 14px; color: white; text-align: center; font-size: 18px; font-weight: 500; margin-bottom: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        .calorie-badge { background: linear-gradient(135deg, var(--primary-blue), var(--accent-purple)); color: white; padding: 15px; border-radius: 14px; text-align: center; font-weight: 500; margin-bottom: 20px; line-height: 1.6; box-shadow: 0 4px 15px rgba(74, 144, 226, 0.2); }

        .chart-card {
            background: #fff;
            border: 1px solid #dfe4ea;
            padding: 20px;
            border-radius: 16px;
            margin-bottom: 20px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.03);
        }

        .meal-plan-card {
            background: #f8f9fa;
            border: 1px solid #dfe4ea;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 20px;
        }

        .bento-box { background: linear-gradient(135deg, #ff6b81, #ff4757); padding: 20px; border-radius: 20px; box-shadow: 0 8px 25px rgba(255, 107, 129, 0.3); color: #333; }
        .bento-grid { display: grid; grid-template-columns: 1.8fr 1.2fr; grid-gap: 12px; }
        .bento-cell { background-color: #ffffff; border-radius: 12px; padding: 15px; box-sizing: border-box; position: relative; box-shadow: 0 4px 10px rgba(0,0,0,0.05); }
        .bento-main { border: 3px solid #ff4757; min-height: 150px; }
        .bento-side-container { display: grid; grid-template-rows: 1fr 1fr; grid-gap: 12px; }
        .bento-side1 { border: 3px solid #ffa502; }
        .bento-side2 { border: 3px solid var(--accent-green); }
        .bento-snack { border: 3px solid var(--primary-blue); margin-top: 12px; min-height: 90px; }
        
        .btn-inline-refresh {
            position: absolute; bottom: 10px; right: 10px; background-color: #ffa502; color: white;
            border: none; width: 35px; height: 35px; border-radius: 10px; font-size: 16px; cursor: pointer;
            display: flex; align-items: center; justify-content: center; transition: 0.2s;
        }
        .btn-inline-refresh:hover { transform: scale(1.1); }
        
        .food-text { font-weight: 500; font-size: 14px; margin: 8px 0; padding-right: 35px; color: #2d3436; }
        .weight-text { font-size: 12px; color: #ff4757; margin: 0 0 4px 0; font-style: italic; font-weight: 500; }
        .badge { display: inline-block; color: white; padding: 4px 10px; border-radius: 6px; font-size: 11px; font-weight: 500; margin-bottom: 6px; }
        
        .todo-section { margin-top: 20px; background: #f8f9fa; padding: 18px; border-radius: 14px; border: 1px solid #dfe4ea; }
        .todo-section ul { padding-left: 20px; margin: 8px 0 0 0; color: #57606f; }
        .todo-section li { margin-bottom: 6px; font-size: 14px; }

        .rank-1 { background-color: rgba(255, 234, 167, 0.4) !important; font-weight: 600; }
        .rank-2 { background-color: rgba(223, 230, 233, 0.5) !important; font-weight: 600; }
        .rank-3 { background-color: rgba(250, 177, 160, 0.3) !important; font-weight: 600; }
        .my-rank { border: 2px solid var(--accent-pink) !important; }
    </style>
</head>
<body>

<div class="container">
    
    <!-- แถบแสดงชื่อผู้ใช้งาน -->
    <div id="userBar" class="user-bar" style="display:none;">
        <span>👤 ผู้ใช้งาน: <span id="displayUsername" style="color:var(--accent-purple);">Guest</span></span>
        <button class="btn-logout" onclick="logoutUser()">ออกจากระบบ</button>
    </div>

    <!-- หน้าล็อกอิน / สมัครสมาชิก -->
    <div id="authScreen" class="screen active">
        <h1>🔐 ระบบบัญชีสุขภาพ</h1>
        <p style="text-align:center; font-size:14px; color:#747d8c; margin-bottom:25px;">เข้าสู่ระบบเพื่อบันทึกประวัติ ดูสถิติ และแข่งอันดับ BMI</p>
        
        <div class="method-tabs">
            <button type="button" class="tab-btn active" id="tabLogin" onclick="switchAuthMode('login')">เข้าสู่ระบบ</button>
            <button type="button" class="tab-btn" id="tabRegister" onclick="switchAuthMode('register')">สมัครสมาชิกใหม่</button>
        </div>

        <div class="form-group">
            <label for="authUsername">ชื่อผู้ใช้งาน (Username):</label>
            <input type="text" id="authUsername" placeholder="กรอกชื่อผู้ใช้">
        </div>
        <div class="form-group">
            <label for="authPassword">รหัสผ่าน (Password):</label>
            <input type="password" id="authPassword" placeholder="กรอกรหัสผ่าน">
        </div>

        <button id="btnAuthSubmit" class="btn-next" onclick="handleAuthSubmit()">เข้าสู่ระบบ 🚀</button>
    </div>

    <!-- หน้าแสดงข้อมูลความรู้ และตาราง BMI -->
    <div id="infoScreen" class="screen">
        <h1>📊 ดัชนีมวลกาย (BMI)</h1>
        <p style="line-height: 1.6; text-align: justify; color: #57606f; font-size: 14px;">
            เครื่องมือประเมินความสมดุลของร่างกาย ออกแบบแยกตามสรีระเพศชายและหญิง เพื่อความแม่นยำสูงสุดในการคำนวณโภชนาการประจำวัน
        </p>

        <table class="info-table">
            <thead>
                <tr>
                    <th>เกณฑ์ประเมิน</th>
                    <th>ชาย 👨</th>
                    <th>หญิง 👩</th>
                    <th>ความเสี่ยง</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td style="color:#f1c40f; font-weight:500;">ผอมเกินไป</td>
                    <td>< 20.0</td>
                    <td>< 18.5</td>
                    <td>เสี่ยงขาดสารอาหาร</td>
                </tr>
                <tr>
                    <td style="color:var(--accent-green); font-weight:500;">สมส่วน / ปกติ</td>
                    <td>20.0 - 24.9</td>
                    <td>18.5 - 23.9</td>
                    <td>สุขภาพดีที่สุด</td>
                </tr>
                <tr>
                    <td style="color:#e67e22; font-weight:500;">น้ำหนักเกิน</td>
                    <td>25.0 - 29.9</td>
                    <td>24.0 - 28.9</td>
                    <td>เริ่มมีความเสี่ยง</td>
                </tr>
                <tr>
                    <td style="color:var(--accent-pink); font-weight:500;">โรคอ้วน</td>
                    <td>≥ 30.0</td>
                    <td>≥ 29.0</td>
                    <td>เสี่ยงโรคเรื้อรัง</td>
                </tr>
            </tbody>
        </table>

        <div class="choice-section">
            <h3 style="color: #2f3542; font-size:16px;">✨ เลือกเมนูการใช้งาน</h3>
            <button class="btn-next" onclick="changeScreen('infoScreen', 'bentoFormScreen')">🍱 1. คำนวณ BMI & จัดกล่องข้าว Bento</button>
            <button type="button" class="btn-back" style="background: linear-gradient(135deg, #4a90e2, #9b59b6) !important; margin-top:10px;" onclick="openHistoryGraphScreen()">📈 2. กราฟแนวโน้ม BMI ของฉัน</button>
            <button type="button" class="btn-leaderboard" onclick="openLeaderboardScreen()">🏆 3. ตารางจัดอันดับสุขภาพ (Leaderboard)</button>
        </div>
    </div>

    <!-- หน้าแบบฟอร์มจัดกล่องข้าว Bento & คำนวณ BMI -->
    <div id="bentoFormScreen" class="screen">
        <h2>🍱 ข้อมูลสรีระและโภชนาการ</h2>
        <div class="form-group">
            <label>เพศของคุณ:</label>
            <div class="gender-box">
                <label><input type="radio" name="gender" value="ชาย" checked> 👨 ผู้ชาย</label>
                <label><input type="radio" name="gender" value="หญิง"> 👩 ผู้หญิง</label>
            </div>
        </div>
        <div class="grid-2">
            <div class="form-group">
                <label for="weight">น้ำหนัก (กิโลกรัม):</label>
                <input type="number" id="weight" value="65" step="0.1">
            </div>
            <div class="form-group">
                <label for="height">ส่วนสูง (เซนติเมตร):</label>
                <input type="number" id="height" value="170" step="0.1">
            </div>
        </div>
        <div class="form-group">
            <label for="diseaseInput">โรคประจำตัว (ถ้ามี):</label>
            <input type="text" id="diseaseInput" placeholder="ตัวอย่าง: เบาหวาน, ความดันโลหิตสูง">
        </div>
        
        <div class="form-group" style="background: #f8f9fa; border: 1px dashed #dfe4ea; padding: 15px; border-radius: 12px;">
            <label style="color: var(--accent-purple); margin-bottom: 10px;">🚫 ประวัติการแพ้อาหาร:</label>
            <div style="display: grid; grid-template-columns: 1fr 1fr; grid-gap: 10px;">
                <label><input type="checkbox" class="allergy-check" value="seafood"> อาหารทะเล</label>
                <label><input type="checkbox" class="allergy-check" value="nut"> ถั่วทุกชนิด</label>
                <label><input type="checkbox" class="allergy-check" value="egg"> ไข่</label>
                <label><input type="checkbox" class="allergy-check" value="milk"> นมวัว / แลคโตส</label>
                <label><input type="checkbox" class="allergy-check" value="wheat"> แป้งสาลี / กลูเตน</label>
            </div>
        </div>

        <button class="btn-next" onclick="calculateBentoBMI()">ประมวลผลและจัดกล่องข้าว Bento ✨</button>
        <button class="btn-back" onclick="changeScreen('bentoFormScreen', 'infoScreen')">⬅️ กลับไปหน้าหลัก</button>
    </div>

    <!-- หน้าแสดงผลลัพธ์การจัดกล่องข้าว Bento -->
    <div id="resultScreen" class="screen">
        <h2>📊 ผลการวิเคราะห์และเมนู Bento</h2>
        <div id="bmiBadge" class="bmi-badge"></div>
        <div id="calorieBadge" class="calorie-badge"></div>

        <div class="meal-plan-card">
            <h3 style="margin-top:0; font-size:15px; color:var(--accent-purple);">🍽️ ปริมาณพลังงานและสารอาหารแนะนำแยกตามมื้อ:</h3>
            <table class="info-table" style="margin-top:8px;">
                <thead>
                    <tr>
                        <th>มื้ออาหาร</th>
                        <th>พลังงาน</th>
                        <th>คาร์บ</th>
                        <th>โปรตีน</th>
                        <th>ผัก/ใยอาหาร</th>
                    </tr>
                </thead>
                <tbody id="mealPlanTableBody">
                </tbody>
            </table>
        </div>
        
        <h3>🍱 เมนูอาหารแนะนำสำหรับกล่อง Bento</h3>
        <div class="bento-box">
            <div class="bento-grid">
                <div class="bento-cell bento-main">
                    <span class="badge" style="background-color: #ff4757;">อาหารจานหลัก</span>
                    <p class="food-text" id="menuMain"></p>
                    <p class="weight-text" id="weightMain"></p>
                    <button class="btn-inline-refresh" onclick="refreshSingleCell('main')">🔄</button>
                </div>
                <div class="bento-side-container">
                    <div class="bento-cell bento-side1">
                        <span class="badge" style="background-color: #ffa502;">เครื่องเคียง</span>
                        <p class="food-text" id="menuSide1"></p>
                        <p class="weight-text" id="weightSide1"></p>
                        <button class="btn-inline-refresh" onclick="refreshSingleCell('side')">🔄</button>
                    </div>
                    <div class="bento-cell bento-side2">
                        <span class="badge" style="background-color: var(--accent-green);">ผัก/วิตามิน</span>
                        <p class="food-text" id="menuSide2"></p>
                        <p class="weight-text" id="weightSide2"></p>
                        <button class="btn-inline-refresh" onclick="refreshSingleCell('veggie')">🔄</button>
                    </div>
                </div>
            </div>
            <div class="bento-cell bento-snack">
                <span class="badge" style="background-color: var(--primary-blue);">ของว่าง / ผลไม้</span>
                <p class="food-text" id="menuSnack"></p>
                <p class="weight-text" id="weightSnack"></p>
                <button class="btn-inline-refresh" onclick="refreshSingleCell('snack')">🔄</button>
            </div>
        </div>
        <button class="btn-reset-all" onclick="rerollAllFood()">🔄 สุ่มเปลี่ยนเซ็ตเมนูใหม่ทั้งหมด</button>
        <button class="btn-maps" onclick="findBentoMapsLocation()">📍 ค้นหาร้านอาหารเมนูนี้ใกล้คุณ (Google Maps)</button>
        
        <div class="todo-section">
            <h4 style="margin:0 0 8px 0; color:var(--accent-purple); font-size:15px;">🏃‍♂️ คำแนะนำการปฏิบัติเพื่อสุขภาพ (Action Plan)</h4>
            <ul id="todoList"></ul>
        </div>
        
        <button style="background: linear-gradient(135deg, #4a90e2, #9b59b6); margin-top:15px;" onclick="openHistoryGraphScreen()">📈 ดูกราฟประวัติ BMI ของคุณ</button>
        <button class="btn-leaderboard" onclick="openLeaderboardScreen()">🏆 ตารางจัดอันดับ Leaderboard</button>
        <button class="btn-back" onclick="changeScreen('resultScreen', 'bentoFormScreen')">⬅️ กลับไปแก้ไขข้อมูล</button>
    </div>

    <!-- หน้าแสดงประวัติและกราฟแนวโน้ม BMI -->
    <div id="historyGraphScreen" class="screen">
        <h2>📈 กราฟแนวโน้มพัฒนาการ BMI</h2>
        
        <div class="chart-card">
            <canvas id="bmiChartCanvas" style="max-height: 300px; width:100%;"></canvas>
        </div>

        <h3 style="font-size:16px; color:#2d3436;">📜 ประวัติการบันทึกย้อนหลัง</h3>
        <table class="info-table" style="margin-bottom:15px;">
            <thead>
                <tr>
                    <th>วัน / เวลา</th>
                    <th>น้ำหนัก (กก.)</th>
                    <th>ส่วนสูง (ซม.)</th>
                    <th>ค่า BMI</th>
                </tr>
            </thead>
            <tbody id="historyTableBody">
            </tbody>
        </table>

        <button style="background: linear-gradient(135deg, #ff4757, #ff6b81);" onclick="clearBmiHistory()">🗑️ ล้างประวัติทั้งหมด</button>
        <button class="btn-back" onclick="changeScreen('historyGraphScreen', 'infoScreen')">⬅️ กลับไปหน้าหลัก</button>
    </div>

    <!-- หน้าตารางจัดอันดับ Leaderboard -->
    <div id="leaderboardScreen" class="screen">
        <h2>🏆 ตารางจัดอันดับสุขภาพ (Leaderboard)</h2>
        <p style="text-align:center; font-size:13.5px; color:#747d8c; margin-bottom:20px;">
            จัดอันดับตามคะแนนความสมส่วนสุขภาพ (Health Score) เทียบกับผู้ใช้งานทุกคนในระบบ
        </p>

        <table class="info-table" style="margin-bottom:20px;">
            <thead>
                <tr>
                    <th>อันดับ</th>
                    <th>ชื่อผู้ใช้งาน</th>
                    <th>BMI ล่าสุด</th>
                    <th>สถานะ</th>
                </tr>
            </thead>
            <tbody id="leaderboardTableBody">
            </tbody>
        </table>

        <button class="btn-back" onclick="changeScreen('leaderboardScreen', 'infoScreen')">⬅️ กลับไปหน้าหลัก</button>
    </div>

</div>

<script>
let currentMainFoodName = ""; 
let currentAuthMode = "login";
let currentUser = null;
let bmiChartInstance = null;

// ระบบจดจำการ Login ไม่ให้หลุดตอนรีเฟรชหน้าจอ
window.addEventListener('DOMContentLoaded', () => {
    const savedUser = localStorage.getItem('bentoCurrentLoggedInUser');
    if (savedUser) {
        currentUser = savedUser;
        document.getElementById('displayUsername').innerText = currentUser;
        document.getElementById('userBar').style.display = 'flex';
        changeScreen('authScreen', 'infoScreen');
    }
});

function switchAuthMode(mode) {
    currentAuthMode = mode;
    if (mode === 'login') {
        document.getElementById('tabLogin').classList.add('active');
        document.getElementById('tabRegister').classList.remove('active');
        document.getElementById('btnAuthSubmit').innerText = "เข้าสู่ระบบ 🚀";
    } else {
        document.getElementById('tabLogin').classList.remove('active');
        document.getElementById('tabRegister').classList.add('active');
        document.getElementById('btnAuthSubmit').innerText = "สมัครสมาชิกใหม่ 📝";
    }
}

function handleAuthSubmit() {
    const username = document.getElementById('authUsername').value.trim();
    const password = document.getElementById('authPassword').value.trim();

    if (!username || !password) {
        alert("กรุณากรอกชื่อผู้ใช้และรหัสผ่านให้ครบถ้วนครับ"); return;
    }

    let users = JSON.parse(localStorage.getItem('bentoUsers') || '{}');

    if (currentAuthMode === 'register') {
        if (users[username]) {
            alert("ชื่อผู้ใช้นี้มีในระบบแล้ว กรุณาใช้ชื่ออื่นครับ"); return;
        }
        users[username] = { password: password, history: [] };
        localStorage.setItem('bentoUsers', JSON.stringify(users));
        alert(`ลงทะเบียนบัญชี "${username}" สำเร็จ! เข้าสู่ระบบอัตโนมัติ`);
        loginSuccess(username);
    } else {
        if (!users[username] || users[username].password !== password) {
            alert("ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้องครับ"); return;
        }
        loginSuccess(username);
    }
}

function loginSuccess(username) {
    currentUser = username;
    localStorage.setItem('bentoCurrentLoggedInUser', username);
    document.getElementById('displayUsername').innerText = username;
    document.getElementById('userBar').style.display = 'flex';
    document.getElementById('authUsername').value = '';
    document.getElementById('authPassword').value = '';
    changeScreen('authScreen', 'infoScreen');
}

function logoutUser() {
    currentUser = null;
    localStorage.removeItem('bentoCurrentLoggedInUser');
    document.getElementById('userBar').style.display = 'none';
    changeScreen(getCurrentActiveScreen(), 'authScreen');
}

function getCurrentActiveScreen() {
    const activeScreen = document.querySelector('.screen.active');
    return activeScreen ? activeScreen.id : 'infoScreen';
}

// ระบบจัดอันดับ Leaderboard
function calculateHealthScore(bmi, gender) {
    const idealBmi = gender === 'ชาย' ? 22.5 : 21.2;
    const diff = Math.abs(bmi - idealBmi);
    let score = 100 - (diff * 8);
    return Math.max(0, parseFloat(score.toFixed(1)));
}

function openLeaderboardScreen() {
    renderLeaderboard();
    changeScreen(getCurrentActiveScreen(), 'leaderboardScreen');
}

function renderLeaderboard() {
    let users = JSON.parse(localStorage.getItem('bentoUsers') || '{}');
    let leaderboardData = [];

    Object.keys(users).forEach(uname => {
        const history = users[uname].history || [];
        if (history.length > 0) {
            const lastRecord = history[history.length - 1];
            const gender = lastRecord.gender || 'ชาย';
            const score = calculateHealthScore(lastRecord.bmi, gender);
            
            let statusText = "สมส่วน";
            let statusColor = "#2ed573";
            
            if (gender === 'ชาย') {
                if (lastRecord.bmi < 20.0) { statusText = "ผอม"; statusColor = "#f1c40f"; }
                else if (lastRecord.bmi >= 25.0 && lastRecord.bmi < 30.0) { statusText = "น้ำหนักเกิน"; statusColor = "#e67e22"; }
                else if (lastRecord.bmi >= 30.0) { statusText = "โรคอ้วน"; statusColor = "#ff4757"; }
            } else {
                if (lastRecord.bmi < 18.5) { statusText = "ผอม"; statusColor = "#f1c40f"; }
                else if (lastRecord.bmi >= 24.0 && lastRecord.bmi < 29.0) { statusText = "น้ำหนักเกิน"; statusColor = "#e67e22"; }
                else if (lastRecord.bmi >= 29.0) { statusText = "โรคอ้วน"; statusColor = "#ff4757"; }
            }

            leaderboardData.push({
                username: uname,
                bmi: lastRecord.bmi,
                score: score,
                statusText: statusText,
                statusColor: statusColor
            });
        }
    });

    leaderboardData.sort((a, b) => b.score - a.score);

    const tbody = document.getElementById('leaderboardTableBody');
    tbody.innerHTML = '';

    if (leaderboardData.length === 0) {
        tbody.innerHTML = '<tr><td colspan="4" style="color:#747d8c;">ยังไม่มีข้อมูลการคำนวณ BMI ในระบบ</td></tr>';
        return;
    }

    leaderboardData.forEach((item, index) => {
        const rank = index + 1;
        let rankBadge = `${rank}`;
        let rowClass = "";

        if (rank === 1) { rankBadge = "🥇 1"; rowClass = "rank-1"; }
        else if (rank === 2) { rankBadge = "🥈 2"; rowClass = "rank-2"; }
        else if (rank === 3) { rankBadge = "🥉 3"; rowClass = "rank-3"; }

        if (item.username === currentUser) {
            rowClass += " my-rank";
        }

        let row = `<tr class="${rowClass}">
            <td>${rankBadge}</td>
            <td>${item.username} ${item.username === currentUser ? '<b style="color:var(--accent-pink);">(คุณ)</b>' : ''}</td>
            <td style="font-weight:600;">${item.bmi}</td>
            <td><span style="color:${item.statusColor}; font-weight:500;">${item.statusText}</span></td>
        </tr>`;
        tbody.innerHTML += row;
    });
}

// ระบบบันทึกประวัติ BMI
function saveBmiRecord(weight, height, bmi, gender) {
    if (!currentUser) return;

    let users = JSON.parse(localStorage.getItem('bentoUsers') || '{}');
    if (!users[currentUser]) return;

    const now = new Date();
    const dateStr = `${now.getDate()}/${now.getMonth()+1}/${now.getFullYear()} ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;

    const record = {
        date: dateStr,
        weight: weight,
        height: height,
        bmi: parseFloat(bmi.toFixed(2)),
        gender: gender
    };

    if (!users[currentUser].history) users[currentUser].history = [];
    users[currentUser].history.push(record);
    localStorage.setItem('bentoUsers', JSON.stringify(users));
}

function openHistoryGraphScreen() {
    renderHistoryTableAndChart();
    changeScreen(getCurrentActiveScreen(), 'historyGraphScreen');
}

function renderHistoryTableAndChart() {
    if (!currentUser) return;

    let users = JSON.parse(localStorage.getItem('bentoUsers') || '{}');
    const history = (users[currentUser] && users[currentUser].history) ? users[currentUser].history : [];

    const tbody = document.getElementById('historyTableBody');
    tbody.innerHTML = '';

    if (history.length === 0) {
        tbody.innerHTML = '<tr><td colspan="4" style="color:#747d8c;">ยังไม่มีข้อมูลประวัติการบันทึก</td></tr>';
    } else {
        history.slice().reverse().forEach(item => {
            let row = `<tr>
                <td>${item.date}</td>
                <td>${item.weight}</td>
                <td>${item.height}</td>
                <td style="font-weight:600; color:var(--accent-purple);">${item.bmi}</td>
            </tr>`;
            tbody.innerHTML += row;
        });
    }

    const labels = history.map(item => item.date);
    const dataBmi = history.map(item => item.bmi);

    let minBmi = dataBmi.length > 0 ? Math.min(...dataBmi) : 18;
    let maxBmi = dataBmi.length > 0 ? Math.max(...dataBmi) : 25;
    
    let yMin = Math.floor((minBmi - 0.5) * 10) / 10;
    let yMax = Math.ceil((maxBmi + 0.5) * 10) / 10;
    if (yMin === yMax) { yMin -= 1; yMax += 1; }

    const ctx = document.getElementById('bmiChartCanvas').getContext('2d');

    if (bmiChartInstance) {
        bmiChartInstance.destroy();
    }

    bmiChartInstance = new Chart(ctx, {
        type: 'line',
        data: {
            labels: labels,
            datasets: [{
                label: 'ค่า BMI ของฉัน',
                data: dataBmi,
                borderColor: '#6c5ce7',
                backgroundColor: 'rgba(108, 92, 231, 0.1)',
                borderWidth: 3,
                fill: true,
                tension: 0.3,
                pointRadius: 6,
                pointHoverRadius: 8,
                pointBackgroundColor: '#ff6b81',
                pointBorderColor: '#ffffff',
                pointBorderWidth: 2
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                tooltip: {
                    callbacks: {
                        label: function(context) { return ` BMI: ${context.parsed.y.toFixed(2)}`; }
                    }
                }
            },
            scales: {
                y: {
                    title: { display: true, text: 'ค่าดัชนีมวลกาย (BMI)', font: { family: 'Kanit', weight: '500' } },
                    min: yMin,
                    max: yMax,
                    ticks: { stepSize: 0.2 },
                    grid: { color: 'rgba(0, 0, 0, 0.05)' }
                },
                x: {
                    title: { display: true, text: 'วัน/เวลา ที่บันทึก', font: { family: 'Kanit', weight: '500' } },
                    grid: { color: 'rgba(0, 0, 0, 0.03)' }
                }
            }
        }
    });
}

function clearBmiHistory() {
    if (!currentUser) return;
    if (confirm("คุณต้องการลบประวัติการบันทึก BMI ทั้งหมดใช่หรือไม่?")) {
        let users = JSON.parse(localStorage.getItem('bentoUsers') || '{}');
        if (users[currentUser]) {
            users[currentUser].history = [];
            localStorage.setItem('bentoUsers', JSON.stringify(users));
            renderHistoryTableAndChart();
        }
    }
}

function changeScreen(hideId, showId) {
    const allScreens = document.querySelectorAll('.screen');
    allScreens.forEach(scr => { scr.classList.remove('active'); });
    document.getElementById(showId).classList.add('active');
    window.scrollTo(0, 0); 
}

const bentoFoodPool = {
    main: [
        { name: "ข้าวกล้อง + อกไก่ย่างเกลือสุพรีม", tags: [] },
        { name: "ข้าวไรซ์เบอร์รี่ + ปลากะพงนึ่งมะนาวสด", tags: ["seafood"] },
        { name: "เส้นหมี่ข้าวกล้อง + เนื้อปลาเก๋าลวกจิ้มแจ่ว", tags: ["seafood", "wheat"] },
        { name: "ข้าวกล้อง + ลาบไก่สับลีนแซ่บ", tags: [] },
        { name: "ข้าวไรซ์เบอร์รี่ + อกไก่ผัดเม็ดมะม่วงหิมพานต์", tags: ["nut"] },
        { name: "ข้าวกล้อง + ปลาแซลมอนย่างเกลือชมพู", tags: ["seafood"] },
        { name: "เส้นบุกบะหมี่ + ผัดฉ่าเนื้อปลากะพงรสจัด", tags: ["seafood"] },
        { name: "ข้าวไรซ์เบอร์รี่ + สันในหมูอบน้ำผึ้งโฮมเมด", tags: [] }
    ],
    side: [
        { name: "ไข่ต้มยางมะตูมเบอร์ใหญ่", tags: ["egg"] },
        { name: "เต้าหู้ขาวนึ่งโรยต้นหอมซีอิ๊วโซเดียมต่ำ", tags: [] },
        { name: "ไข่ตุ๋นสมุนไพรเนื้อเนียนใส่ไก่สับ", tags: ["egg"] },
        { name: "เต้าหู้เหลืองผัดถั่วงอกลีน", tags: [] }
    ],
    veggie: [
        { name: "บล็อคโคลี่และแครอทต้มหวานธรรมชาติ", tags: [] },
        { name: "หน่อไม้ฝรั่งและเห็ดออรินจิผัดน้ำมันมะกอก", tags: [] },
        { name: "กะหล่ำปลีตุ๋นเห็ดหอมรสกลมกล่อม", tags: [] }
    ],
    snack: [
        { name: "แอปเปิ้ลเขียวสดคัดพิเศษ 1 ลูก", tags: [] },
        { name: "ฝรั่งกิมจูสดกรอบวิตามินซีสูง", tags: [] },
        { name: "แก้วมังกรเนื้อขาวหั่นชิ้นพอดีคำ", tags: [] },
        { name: "โยเกิร์ตรสธรรมชาติไขมัน 0% 1 ถ้วย", tags: ["milk"] }
    ]
};

let currentWeights = {};
let activeAllergies = [];

function calculateBentoBMI() {
    const gender = document.querySelector('input[name="gender"]:checked').value;
    const weight = parseFloat(document.getElementById('weight').value);
    const heightCm = parseFloat(document.getElementById('height').value);
    if (!weight || !heightCm) { alert("กรุณากรอกน้ำหนักและส่วนสูงก่อนครับ"); return; }
    
    const heightM = heightCm / 100; 
    const bmi = weight / (heightM * heightM);

    saveBmiRecord(weight, heightCm, bmi, gender);

    let status = "", color = "", todo = [], dailyCalorie = 0;

    if (gender === 'ชาย') {
        dailyCalorie = weight * 24 * 1.2;
        if (bmi < 20.0) { 
            status = "ผอมเกินเกณฑ์ (ชาย)"; color = "#f1c40f"; 
            currentWeights = { main: "คาร์บ 180g + เนื้อ 150g", side: "โปรตีน 90g", veggie: "ผัก 120g", snack: "90g" }; 
            todo = ["เน้นสร้างมวลกล้ามเนื้อโดยเพิ่มเวทเทรนนิ่ง", "เพิ่มโปรตีนและแบ่งมื้ออาหารเป็น 4-5 มื้อย่อย"]; 
        } else if (bmi >= 20.0 && bmi < 25.0) { 
            status = "สมส่วน สุขภาพดี (ชาย)"; color = "#2ed573"; 
            currentWeights = { main: "คาร์บ 150g + เนื้อ 130g", side: "โปรตีน 70g", veggie: "ผัก 120g", snack: "80g" }; 
            todo = ["รักษาสมดุลสารอาหาร 5 หมู่", "ออกกำลังกายสม่ำเสมอสัปดาห์ละ 150 นาที"]; 
        } else if (bmi >= 25.0 && bmi < 30.0) { 
            status = "น้ำหนักเกิน / ท้วม (ชาย)"; color = "#e67e22"; 
            currentWeights = { main: "คาร์บ 100g + เนื้อ 130g", side: "โปรตีน 60g", veggie: "ผัก 160g", snack: "50g" }; 
            todo = ["ลดของทอด และเครื่องดื่มชูกำลัง/น้ำหวาน", "เน้นออกกำลังกายเวทเทรนนิ่งควบคู่คาร์ดิโอ"]; 
        } else { 
            status = "ภาวะโรคอ้วน (ชาย)"; color = "#ff4757"; 
            currentWeights = { main: "คาร์บ 70g + เนื้อ 140g", side: "โปรตีน 60g", veggie: "ผัก 200g", snack: "50g" }; 
            todo = ["งดของหวานและน้ำหวานเด็ดขาด", "เน้นเดินเร็วหรือปั่นจักรยานเพื่อเซฟข้อเข่า"]; 
        }
    } else {
        dailyCalorie = weight * 22 * 1.2;
        if (bmi < 18.5) { 
            status = "ผอมเกินเกณฑ์ (หญิง)"; color = "#f1c40f"; 
            currentWeights = { main: "คาร์บ 140g + เนื้อ 110g", side: "โปรตีน 70g", veggie: "ผัก 100g", snack: "70g" }; 
            todo = ["ทานอาหารที่มีธาตุเหล็กและโปรตีนสูง", "เน้นรับประทานไขมันดี เช่น ถั่วและอะโวคาโด"]; 
        } else if (bmi >= 18.5 && bmi < 24.0) { 
            status = "สมส่วน สุขภาพดี (หญิง)"; color = "#2ed573"; 
            currentWeights = { main: "คาร์บ 120g + เนื้อ 100g", side: "โปรตีน 50g", veggie: "ผัก 100g", snack: "60g" }; 
            todo = ["รักษาสมดุลสารอาหารคุมน้ำตาล", "เล่นโยคะหรือพิลาทิสเพื่อสร้างความกระชับ"]; 
        } else if (bmi >= 24.0 && bmi < 29.0) { 
            status = "น้ำหนักเกิน / ท้วม (หญิง)"; color = "#e67e22"; 
            currentWeights = { main: "คาร์บ 80g + เนื้อ 100g", side: "โปรตีน 40g", veggie: "ผัก 140g", snack: "40g" }; 
            todo = ["คุมสัดส่วนขนมหวาน ชานมไข่มุก และของผัดน้ำมัน", "เน้นการออกกำลังกายแบบแอโรบิกต่อเนื่อง"]; 
        } else { 
            status = "ภาวะโรคอ้วน (หญิง)"; color = "#ff4757"; 
            currentWeights = { main: "คาร์บ 50g + เนื้อ 110g", side: "โปรตีน 40g", veggie: "ผัก 180g", snack: "30g" }; 
            todo = ["หลีกเลี่ยงการอดอาหาร แต่เน้นการปรับสัดส่วนอาหาร", "เดินคาร์ดิโอเบาๆ สม่ำเสมอทุกวัน"]; 
        }
    }

    const totalCal = Math.round(dailyCalorie);
    const meals = [
        { name: "🌅 มื้อเช้า (30%)", ratio: 0.30 },
        { name: "☀️ มื้อกลางวัน (40%)", ratio: 0.40 },
        { name: "🌙 มื้อเย็น (30%)", ratio: 0.30 }
    ];

    let tableHTML = "";
    meals.forEach(m => {
        const cal = Math.round(totalCal * m.ratio);
        const carbG = Math.round((cal * 0.45) / 4);
        const proteinG = Math.round((cal * 0.30) / 4);
        const veggieG = Math.round(m.ratio * (gender === 'ชาย' ? 400 : 320));

        tableHTML += `
            <tr>
                <td style="font-weight:500; text-align:left;">${m.name}</td>
                <td style="color:var(--primary-blue); font-weight:600;">${cal} kcal</td>
                <td style="color:#e67e22;">${carbG}g</td>
                <td style="color:#ff4757;">${proteinG}g</td>
                <td style="color:var(--accent-green);">${veggieG}g</td>
            </tr>
        `;
    });

    document.getElementById('mealPlanTableBody').innerHTML = tableHTML;

    activeAllergies = []; 
    document.querySelectorAll('.allergy-check:checked').forEach(chk => { activeAllergies.push(chk.value); });
    
    document.getElementById('bmiBadge').style.backgroundColor = color;
    document.getElementById('bmiBadge').innerHTML = `เพศ ${gender} | ค่า BMI: ${bmi.toFixed(2)}<br><small>(${status})</small>`;
    document.getElementById('calorieBadge').innerHTML = `🎯 พลังงานที่แนะนำรวมต่อวัน (${gender}): <b>${totalCal} kcal</b>`;
    
    const todoList = document.getElementById('todoList'); todoList.innerHTML = "";
    todo.forEach(item => { let li = document.createElement('li'); li.innerText = item; todoList.appendChild(li); });
    
    rerollAllFood(); 
    changeScreen('bentoFormScreen', 'resultScreen');
}

function refreshSingleCell(category) {
    const filtered = bentoFoodPool[category].filter(item => !item.tags.some(tag => activeAllergies.includes(tag)));
    const finalPool = filtered.length > 0 ? filtered : [{name: "ข้าวกล้องเปล่า + ผักนึ่งลีน (เมนูปลอดภัยพิเศษ)", tags: []}];
    const newFood = finalPool[Math.floor(Math.random() * finalPool.length)].name;

    if (category === 'main') { 
        currentMainFoodName = newFood; 
        document.getElementById('menuMain').innerText = newFood; 
        document.getElementById('weightMain').innerText = `⚖️ แนะนำ: ${currentWeights.main}`; 
    } 
    else if (category === 'side') { document.getElementById('menuSide1').innerText = newFood; document.getElementById('weightSide1').innerText = `⚖️ แนะนำ: ${currentWeights.side}`; } 
    else if (category === 'veggie') { document.getElementById('menuSide2').innerText = newFood; document.getElementById('weightSide2').innerText = `⚖️ แนะนำ: ${currentWeights.veggie}`; } 
    else if (category === 'snack') { document.getElementById('menuSnack').innerText = newFood; document.getElementById('weightSnack').innerText = `⚖️ แนะนำ: ${currentWeights.snack}`; }
}

function rerollAllFood() { refreshSingleCell('main'); refreshSingleCell('side'); refreshSingleCell('veggie'); refreshSingleCell('snack'); }

function findBentoMapsLocation() {
    let query = currentMainFoodName ? (currentMainFoodName.split("+")[1] || currentMainFoodName) : "อาหารสุขภาพ";
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
            function(p) { window.open(`https://www.google.com/maps/search/${encodeURIComponent(query)}/@${p.coords.latitude},${p.coords.longitude},14z`, '_blank'); },
            function() { window.open(`https://www.google.com/maps/search/${encodeURIComponent(query)}+ใกล้ฉัน`, '_blank'); }
        );
    } else { window.open(`https://www.google.com/maps/search/${encodeURIComponent(query)}+ใกล้ฉัน`, '_blank'); }
}
</script>

</body>
</html>
