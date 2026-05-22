<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>🔒 Secure Worksheet: WOW! Culture (Unit 7) + Google Sheets Lock</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #eef2f5;
            font-family: 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
            padding: 40px 20px;
            color: #1e2a3a;
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
        }

        @media print {
            body { display: none !important; }
        }

        body.locked .worksheet-container,
        body.sheet-locked .worksheet-container {
            filter: blur(5px);
            pointer-events: none;
            user-select: none;
        }

        body.locked .lock-overlay,
        body.sheet-locked .sheet-lock-overlay {
            display: flex;
        }

        .lock-overlay, .sheet-lock-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.85);
            backdrop-filter: blur(8px);
            z-index: 10000;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', system-ui;
        }

        .lock-card, .sheet-lock-card {
            background: white;
            max-width: 460px;
            width: 90%;
            padding: 32px 28px;
            border-radius: 48px;
            text-align: center;
            box-shadow: 0 25px 45px rgba(0,0,0,0.3);
            animation: fadeInUp 0.2s ease;
        }

        .sheet-lock-card h2 {
            color: #d9534f;
        }

        .lock-card h2 {
            color: #c4452c;
        }

        .lock-card p, .sheet-lock-card p {
            margin-bottom: 24px;
            color: #2c3e4e;
        }

        .lock-card input, .sheet-lock-card input {
            width: 100%;
            padding: 14px 18px;
            font-size: 1rem;
            border: 2px solid #d4dee8;
            border-radius: 60px;
            margin-bottom: 18px;
            outline: none;
            text-align: center;
        }

        .lock-card button, .sheet-lock-card button {
            background: #1f5a7a;
            border: none;
            color: white;
            font-weight: bold;
            padding: 12px 24px;
            border-radius: 60px;
            font-size: 1rem;
            cursor: pointer;
            width: 100%;
        }

        .error-msg {
            color: #d9534f;
            margin-top: 12px;
            font-size: 0.85rem;
        }

        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .worksheet-container {
            max-width: 1100px;
            margin: 0 auto;
            background: white;
            border-radius: 28px;
            box-shadow: 0 20px 35px -12px rgba(0, 0, 0, 0.15);
            overflow: hidden;
            padding: 30px 35px 45px;
            transition: all 0.2s;
        }

        .student-auth-area {
            background: #f0f7fc;
            border-radius: 28px;
            padding: 20px 28px;
            margin-bottom: 30px;
            border: 1px solid #cde1ec;
            text-align: center;
        }

        .student-auth-area input {
            padding: 12px 20px;
            font-size: 1rem;
            border-radius: 40px;
            border: 1px solid #cbdde9;
            width: 260px;
            margin: 10px 8px;
        }

        .student-auth-area button {
            background: #1f5a7a;
            color: white;
            border: none;
            padding: 12px 28px;
            border-radius: 40px;
            font-weight: bold;
            cursor: pointer;
        }

        .auth-error {
            color: #c0392b;
            margin-top: 12px;
            font-size: 0.85rem;
        }

        .workspace-content {
            display: none;
        }

        body.authenticated .workspace-content {
            display: block;
        }

        body.authenticated .student-auth-area {
            display: none;
        }

        h1 {
            font-size: 1.9rem;
            font-weight: 600;
            background: linear-gradient(135deg, #1f4870, #2a6f8f);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            border-left: 6px solid #2a6f8f;
            padding-left: 20px;
            margin-bottom: 12px;
        }

        .sub {
            color: #4b6f8c;
            margin-bottom: 32px;
            font-size: 1rem;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 10px;
        }

        .activity-card {
            background: #fefefe;
            border-radius: 24px;
            margin-bottom: 40px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.03);
            border: 1px solid #e9edf2;
        }

        .activity-title {
            font-size: 1.5rem;
            font-weight: 600;
            background: #f8fafc;
            padding: 16px 24px;
            border-radius: 24px 24px 0 0;
            border-bottom: 2px solid #dee4ec;
            color: #0f3b4f;
        }

        .activity-content {
            padding: 20px 28px 28px 28px;
        }

        .sentence-item {
            background: #f9fafb;
            padding: 16px 20px;
            border-radius: 20px;
            border: 1px solid #eef2f6;
            margin-bottom: 14px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .sentence-row-layout {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100%;
            flex-wrap: wrap;
            gap: 15px;
        }

        .sentence-text {
            font-size: 1rem;
            font-weight: 450;
            flex: 1;
            min-width: 280px;
        }

        .option-buttons {
            display: flex;
            gap: 15px;
            align-items: center;
        }

        .option-buttons label {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            background: white;
            padding: 6px 16px;
            border-radius: 60px;
            border: 1px solid #cfdfed;
            font-weight: 600;
        }

        .option-buttons input {
            margin: 0;
            accent-color: #2a6f8f;
            width: 18px;
            height: 18px;
        }

        .field-row {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 12px;
            margin-bottom: 14px;
            background: #f9fafb;
            padding: 12px 18px;
            border-radius: 18px;
            border: 1px solid #e9edf2;
        }

        .field-label {
            font-weight: 600;
            min-width: 40px;
            color: #1f4e6e;
        }

        .explanation-input {
            width: 100%;
            padding: 10px 16px;
            border-radius: 14px;
            border: 1px solid #cddfea;
            font-size: 0.9rem;
            margin-top: 5px;
        }

        .btn-check {
            background: #1f5a7a;
            border: none;
            color: white;
            font-weight: 600;
            padding: 12px 28px;
            border-radius: 60px;
            font-size: 1rem;
            cursor: pointer;
            margin-top: 12px;
            margin-bottom: 18px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .btn-check:hover {
            background: #0f415b;
            transform: scale(0.98);
        }

        .score-area {
            background: #eef3f7;
            border-radius: 28px;
            padding: 16px 25px;
            margin: 28px 0 10px;
            font-weight: 600;
            font-size: 1.2rem;
            text-align: center;
            border: 1px solid #cde1ec;
        }

        .feedback {
            font-size: 0.9rem;
            margin-top: 6px;
            color: #2c6e2c;
            font-weight: 500;
        }

        .example-text {
            color: #3b7c9c;
            background: #eef5f9;
            padding: 10px 16px;
            border-radius: 14px;
            font-size: 0.9rem;
            margin-bottom: 18px;
        }

        .info-grid {
            display: flex;
            flex-direction: column;
            gap: 16px;
            margin: 15px 0;
        }

        .student-card {
            background: #f1f6fa;
            border-radius: 24px;
            padding: 22px;
            border-left: 6px solid #ffbe5e;
        }
        .student-card h3 { margin-bottom: 14px; color: #1f4870; font-size: 1.2rem;}
        .student-card ul { list-style: none; padding-left: 0; }
        .student-card li { margin: 12px 0; display: flex; flex-wrap: wrap; align-items: center; gap: 8px; }
        .label-badge { font-weight: 600; min-width: 140px; color: #334e68; }

        .inline-gap {
            width: 130px;
            padding: 6px 12px;
            border-radius: 20px;
            border: 1px solid #cddfea;
            text-align: center;
            font-weight: 600;
            color: #1f5a7a;
        }

        @media (max-width: 700px) {
            .worksheet-container { padding: 20px; }
            .sentence-row-layout { flex-direction: column; align-items: flex-start; }
            .option-buttons { width: 100%; justify-content: flex-start; }
        }
    </style>
    <script>
        /* ===================== GOOGLE SHEETS INTEGRATION ===================== */
        // 🔁 قم بتغيير هذا الرابط بعد نشر Google Apps Script (doPost)
// 🔁 ضع الرابط الذي نسخته من الخطوة 9 هنا:
const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbw3KEwFFzzZyJP-pETrU2wWN9J5aWtZ4tdKwECKBD8UDGW5wEUInQQkxLyzonRyDOFXQw/exec"; 
        
        let globalStudentId = "";
        let globalStudentName = ""; // متغير جديد للاسم
        let startTime; // متغير لحساب وقت البدء
        
        async function checkStudentInSheet(studentId) {
            try {
                const response = await fetch(SCRIPT_URL, {
                    method: "POST",
                    // استخدام text/plain يمنع أخطاء CORS مع Google Apps Script
                    headers: { "Content-Type": "text/plain;charset=utf-8" }, 
                    body: JSON.stringify({ action: "check", studentId: studentId })
                });
                const data = await response.json();
                return data.exists === true;
            } catch(err) {
                console.warn("Sheet check failed", err);
                return false; 
            }
        }
        
        async function saveResultToSheet(studentId, studentName, score, timeSpent) {
            try {
                await fetch(SCRIPT_URL, {
                    method: "POST",
                    headers: { "Content-Type": "text/plain;charset=utf-8" },
                    body: JSON.stringify({ 
                        action: "save", 
                        studentId: studentId, 
                        studentName: studentName,
                        score: score,
                        timeSpent: timeSpent
                    })
                });
                return true;
            } catch(err) {
                console.error("Failed to save", err);
                return false;
            }
        }
        
        /* --- HIGH-SECURITY DEEP STORAGE LOCKING ENGINE (INDEXEDDB) --- */
        const DB_NAME = "SecuritySubsystem7th";
        const STORE_NAME = "SessionLocks";
        let db;
        
        function initSecurityDB() {
            return new Promise((resolve) => {
                let request = indexedDB.open(DB_NAME, 1);
                request.onupgradeneeded = function(e) {
                    let database = e.target.result;
                    if (!database.objectStoreNames.contains(STORE_NAME)) {
                        database.createObjectStore(STORE_NAME);
                    }
                };
                request.onsuccess = function(e) {
                    db = e.target.result;
                    resolve(true);
                };
                request.onerror = function() { resolve(false); };
            });
        }
        
        function readPermanentLock() {
            return new Promise((resolve) => {
                if (!db) { resolve(false); return; }
                let transaction = db.transaction([STORE_NAME], "readonly");
                let store = transaction.objectStore(STORE_NAME);
                let getRequest = store.get("permanently_submitted");
                getRequest.onsuccess = function() {
                    resolve(getRequest.result === "true");
                };
                getRequest.onerror = function() { resolve(false); };
            });
        }
        
        function writePermanentLock() {
            if (!db) return;
            let transaction = db.transaction([STORE_NAME], "readwrite");
            let store = transaction.objectStore(STORE_NAME);
            store.put("true", "permanently_submitted");
        }
        
        function clearPermanentLock() {
            if (!db) return;
            let transaction = db.transaction([STORE_NAME], "readwrite");
            let store = transaction.objectStore(STORE_NAME);
            store.delete("permanently_submitted");
        }
        
        // التحقق الأساسي عند التحميل (IDB + localStorage + Google Sheets)
        async function runEnforcementCheck() {
            await initSecurityDB();
            const idbLocked = await readPermanentLock();
            const lsLocked = localStorage.getItem('worksheet_permanently_submitted') === 'true';
            
            // إذا كان هناك قفل محلي -> منع فوري
            if (lsLocked || idbLocked) {
                localStorage.setItem('worksheet_permanently_submitted', 'true');
                writePermanentLock();
                showPermanentLockScreen();
                return true; // locked
            }
            
            // لا قفل محلي -> انتظر المصادقة عبر Student ID وفحص Google Sheets
            return false;
        }
        
        function showPermanentLockScreen() {
            document.documentElement.innerHTML = `
            <head><title>Access Denied</title><meta name="viewport" content="width=device-width, initial-scale=1.0"></head>
            <body style="background:#0b0f19;color:#ff6b6b;display:flex;flex-direction:column;justify-content:center;align-items:center;height:100vh;font-family:sans-serif;padding:20px;text-align:center;">
                <h2>🔒 Access Terminated</h2>
                <p style="color:#a0aec0;margin-top:10px;max-width:400px;">This evaluation session has already been completed and recorded. Re-entry is strictly prohibited.</p>
                <div style="margin-top:30px; background:#161b26; padding:20px 30px; border-radius:20px;">
                    <p style="color:#e2e8f0; font-size:0.85rem;">🛠️ TEACHER OVERRIDE:</p>
                    <input type="password" id="overrideInput" placeholder="Password" style="padding:10px; border-radius:30px; background:#1f2738; color:#fff; text-align:center;">
                    <button id="overrideBtn" style="background:#2563eb; color:white; border:none; padding:10px 20px; border-radius:30px; margin-top:12px;">Clear Lock & Reload</button>
                </div>
            </body>`;
            setTimeout(() => {
                document.getElementById('overrideBtn').addEventListener('click', () => {
                    if (document.getElementById('overrideInput').value.trim() === "0007") {
                        localStorage.removeItem('worksheet_permanently_submitted');
                        localStorage.removeItem('worksheet_status_7th');
                        clearPermanentLock();
                        sessionStorage.clear();
                        window.location.reload();
                    } else alert("❌ Incorrect teacher code.");
                });
            }, 50);
        }
        
        // منع back button
        history.pushState(null, null, window.location.href);
        window.addEventListener('popstate', function () {
            history.pushState(null, null, window.location.href);
        });
        
        // حظر النسخ والطباعة
        const killInteractions = (e) => e.preventDefault();
        document.addEventListener('copy', killInteractions);
        document.addEventListener('cut', killInteractions);
        document.addEventListener('contextmenu', killInteractions);
        document.addEventListener('selectstart', killInteractions);
        
        document.addEventListener('keydown', function(e) {
            if (e.ctrlKey && (e.key === 'c' || e.key === 'x' || e.key === 'a' || e.key === 'u' || e.key === 's' || e.key === 'p')) {
                e.preventDefault();
                return false;
            }
            if (e.key === 'F12' || (e.ctrlKey && e.shiftKey && (e.key === 'I' || e.key === 'J' || e.key === 'C'))) {
                e.preventDefault();
                return false;
            }
        });
    </script>
</head>
<body>

<div id="lockOverlay" class="lock-overlay">
    <div class="lock-card">
        <h2>🔒 Activity Locked</h2>
        <p>⚠️ You left the page or the window lost focus.<br>Enter teacher password to continue.</p>
        <input type="password" id="passwordInput" placeholder="Enter password" autocomplete="off">
        <button id="unlockBtn">Unlock Worksheet</button>
        <div id="lockErrorMsg" class="error-msg"></div>
    </div>
</div>

<div id="sheetLockOverlay" class="sheet-lock-overlay">
    <div class="sheet-lock-card">
        <h2>🚫 Duplicate Attempt Blocked</h2>
        <p>This Student ID has already completed the worksheet.<br>Re-entry is not allowed.</p>
        <p style="font-size:0.8rem;">🔐 Record exists in Google Sheets.</p>
        <div style="margin-top: 20px;">
            <input type="password" id="teacherOverrideSheet" placeholder="Teacher password" style="width:100%;">
            <button id="overrideSheetBtn">Override & Clear</button>
        </div>
        <div id="sheetLockError" class="error-msg"></div>
    </div>
</div>

<div class="worksheet-container">
    <h1>📝 WOW! Culture — Lesson 8</h1>
    <div class="sub">Non-verbal Communication & Global Languages | 🔐 Anti-Cheat + Google Sheets Lock</div>

    <!-- Student Authentication Panel -->
    <div class="student-auth-area" id="studentAuthPanel">
        <h3>🔑 Enter your Details to begin</h3>
        <input type="text" id="studentIdInput" placeholder="Student ID (e.g., 2024001)" autocomplete="off">
        <input type="text" id="studentNameInput" placeholder="Full Name in English" autocomplete="off">
        <button id="startExamBtn">Start Evaluation</button>
        <div id="authErrorMsg" class="auth-error"></div>
    </div>

    <div class="workspace-content" id="workspaceContent">
        <!-- بقية محتوى الاختبار نفسه دون تغيير -->
        <div class="activity-card">
            <div class="activity-title">📖 1. After you read: Complete the sentences</div>
            <div class="activity-content">
                <div class="example-text">📌 Write <b>one word</b> in each gap based on your Pupil's Book page 38.</div>
                <div class="field-row"><span class="field-label">1️⃣</span><span class="sentence-text">We can <b>communicate</b> with each other <input type="text" id="act1_q1" class="inline-gap" placeholder="gap 1"> using any words.</span></div>
                <div class="field-row"><span class="field-label">2️⃣</span><span class="sentence-text">Emojis are <input type="text" id="act1_q2" class="inline-gap" placeholder="gap 2"> that are used in social media and text messages.</span></div>
                <div class="field-row"><span class="field-label">3️⃣</span><span class="sentence-text">Hieroglyphics are a <input type="text" id="act1_q3" class="inline-gap" placeholder="gap 3"> language that was used in Egypt.</span></div>
                <div class="field-row"><span class="field-label">4️⃣</span><span class="sentence-text">The <input type="text" id="act1_q4" class="inline-gap" placeholder="gap 4"> Day of Sign Languages is on 23rd September.</span></div>
                <div id="act1Feedback" class="feedback"></div>
            </div>
        </div>

        <div class="activity-card">
            <div class="activity-title">✔️ 2. Read the sentences and circle T (True) or F (False)</div>
            <div class="activity-content">
                <div class="example-text">📌 Select T or F, then write a brief explanation for your answer.</div>
                <div class="sentence-item"><div class="sentence-row-layout"><div class="sentence-text">1️⃣ Some types of language use pictures instead of words.</div><div class="option-buttons"><label><input type="radio" name="act2_q1" value="T"> T</label><label><input type="radio" name="act2_q1" value="F"> F</label></div></div><input type="text" id="act2_exp1" class="explanation-input" placeholder="Explanation: Emojis and hieroglyphics use pictures."></div>
                <div class="sentence-item"><div class="sentence-row-layout"><div class="sentence-text">2️⃣ Emojis aren't popular with 18-25-year-old people.</div><div class="option-buttons"><label><input type="radio" name="act2_q2" value="T"> T</label><label><input type="radio" name="act2_q2" value="F"> F</label></div></div><input type="text" id="act2_exp2" class="explanation-input" placeholder="Explain your answer..."></div>
                <div class="sentence-item"><div class="sentence-row-layout"><div class="sentence-text">3️⃣ Sad emojis aren't used as often as happy emojis.</div><div class="option-buttons"><label><input type="radio" name="act2_q3" value="T"> T</label><label><input type="radio" name="act2_q3" value="F"> F</label></div></div><input type="text" id="act2_exp3" class="explanation-input" placeholder="Explain your answer..."></div>
                <div class="sentence-item"><div class="sentence-row-layout"><div class="sentence-text">4️⃣ We can't understand what hieroglyphics mean.</div><div class="option-buttons"><label><input type="radio" name="act2_q4" value="T"> T</label><label><input type="radio" name="act2_q4" value="F"> F</label></div></div><input type="text" id="act2_exp4" class="explanation-input" placeholder="Explain your answer..."></div>
                <div class="sentence-item"><div class="sentence-row-layout"><div class="sentence-text">5️⃣ There is more than one type of sign language.</div><div class="option-buttons"><label><input type="radio" name="act2_q5" value="T"> T</label><label><input type="radio" name="act2_q5" value="F"> F</label></div></div><input type="text" id="act2_exp5" class="explanation-input" placeholder="Explain your answer..."></div>
                <div id="act2Feedback" class="feedback"></div>
            </div>
        </div>

        <div class="activity-card">
            <div class="activity-title">🎧 3. Listen to a report about Silbo Gomero: Complete the notes</div>
            <div class="activity-content">
                <div class="info-grid">
                    <div class="student-card"><h3>🗣️ Language Profile: Silbo Gomero</h3><ul><li><span class="label-badge">• Type:</span> <span>A very unusual whistling language now used by about <input type="text" id="act3_q2" class="inline-gap" style="width:70px;" placeholder="2"> people.</span></li></ul></div>
                    <div class="student-card" style="border-left-color: #2a6f8f;"><h3>📍 Place Used</h3><ul><li><span class="label-badge">• Location:</span> <span>Used on the <input type="text" id="act3_q3" class="inline-gap" placeholder="3"> of La Gomera, which is part of Spain.</span></li><li><span class="label-badge">• Geography:</span> <span>In the mountains, where people are separated by <input type="text" id="act3_q4" class="inline-gap" placeholder="4">.</span></li><li><span class="label-badge">• Advantage:</span> <span>Easier than <input type="text" id="act3_q5" class="inline-gap" placeholder="5"> long distances to speak with people.</span></li></ul></div>
                    <div class="student-card" style="border-left-color: #2c6e2c;"><h3>⏳ History & Status</h3><ul><li><span class="label-badge">• Origins:</span> <span>Used by the Guanches people for <input type="text" id="act3_q6" class="inline-gap" placeholder="6"> of years.</span></li><li><span class="label-badge">• Evolution:</span> <span>Changed later to communicate the <input type="text" id="act3_q7" class="inline-gap" placeholder="7"> language.</span></li><li><span class="label-badge">• Education:</span> <span>Became an official school subject on La Gomera in <input type="text" id="act3_q8" class="inline-gap" placeholder="8">.</span></li><li><span class="label-badge">• UNESCO:</span> <span>Recognised as a World Heritage language by UNESCO in <input type="text" id="act3_q9" class="inline-gap" placeholder="9">.</span></li><li><span class="label-badge">• Modern Day:</span> <span>Now popular with <input type="text" id="act3_q10" class="inline-gap" placeholder="10"> who come to La Gomera to hear it.</span></li></ul></div>
                </div>
                <div id="act3Feedback" class="feedback"></div>
            </div>
        </div>

        <div style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap;">
            <button class="btn-check" id="checkAllBtn">✅ Auto-Correct & Score</button>
            <button class="btn-check" id="resetBtn" style="background: #5e7c8c;">⟳ Reset all answers</button>
        </div>
        <div id="totalScoreArea" class="score-area">📊 Total score: -- / 18</div>
    </div>
</div>

<script>
    // --------------------- GOOGLE SHEETS AUTH & LOCK ---------------------
    const TEACHER_PASSWORD = "0007";
    let isLocked = false;
    let hasAnswersBeforeLeave = false;
    let currentStudentId = "";
    
    // عناصر DOM
    const lockOverlay = document.getElementById('lockOverlay');
    const sheetLockOverlay = document.getElementById('sheetLockOverlay');
    const passwordInput = document.getElementById('passwordInput');
    const unlockBtn = document.getElementById('unlockBtn');
    const lockErrorMsg = document.getElementById('lockErrorMsg');
    const startBtn = document.getElementById('startExamBtn');
    const studentIdInput = document.getElementById('studentIdInput');
    const authErrorMsg = document.getElementById('authErrorMsg');
    const workspace = document.getElementById('workspaceContent');
    
    // دوال القفل المحلي
    function lockPage(reason = "generic") {
        if (isLocked) return;
        isLocked = true;
        document.body.classList.add('locked');
        lockOverlay.style.display = 'flex';
        lockErrorMsg.innerText = '';
        passwordInput.value = '';
        localStorage.setItem('worksheet_status_7th', 'locked');
    }
    
    function unlockPage() {
        if (!isLocked) return;
        if (passwordInput.value.trim() === TEACHER_PASSWORD) {
            isLocked = false;
            document.body.classList.remove('locked');
            lockOverlay.style.display = 'none';
            lockErrorMsg.innerText = '';
            localStorage.removeItem('worksheet_status_7th');
        } else {
            lockErrorMsg.innerText = '❌ Incorrect password. Access denied.';
            passwordInput.value = '';
        }
    }
    
    unlockBtn.addEventListener('click', unlockPage);
    passwordInput.addEventListener('keypress', (e) => { if (e.key === 'Enter') unlockPage(); });
    
    // مراقبة مغادرة الصفحة
    document.addEventListener("visibilitychange", () => {
        if (document.hidden  && !isLocked && document.body.classList.contains('authenticated')) {
            lockPage('Tab leave caught');
            alert("Activity Locked: You switched tabs or minimized the page.");
        }
    });
    window.addEventListener("blur", function() {
        if (!isLocked  && document.body.classList.contains('authenticated')) {
            lockPage('Window lost focus');
            alert("Activity Locked: Window lost focus.");
        }
    });
    
    function checkAnyAnswer() {
        for (let i = 1; i <= 5; i++) {
            if (document.querySelector(`input[name="act2_q${i}"]:checked`)) return true;
        }
        const inputs = document.querySelectorAll('input[type="text"]');
        for (let inp of inputs) {
            if (inp.value.trim() !== "") return true;
        }
        return false;
    }
    function updateAnswerFlag() { hasAnswersBeforeLeave = checkAnyAnswer(); }
    document.addEventListener('change', updateAnswerFlag);
    document.addEventListener('input', updateAnswerFlag);
    
    // بدء الاختبار بعد التحقق من Google Sheets

startBtn.addEventListener('click', async () => {
        const studentId = studentIdInput.value.trim();
        const studentName = document.getElementById('studentNameInput').value.trim();
        
        if (!studentId || !studentName) {
            authErrorMsg.innerText = "❌ Please enter both Student ID and Name.";
            return;
        }
        // تحقق من أن الاسم باللغة الإنجليزية (اختياري)
        if (!/^[a-zA-Z\s]+$/.test(studentName)) {
            authErrorMsg.innerText = "❌ Please write your name in English only.";
            return;
        }

        authErrorMsg.innerText = "⏳ Checking with Google Sheets...";
        startBtn.disabled = true;
        
        try {
            const exists = await checkStudentInSheet(studentId);
            if (exists) {
                authErrorMsg.innerText = "🚫 This Student ID has already completed the worksheet. Access denied.";
                sheetLockOverlay.style.display = 'flex';
                document.body.classList.add('sheet-locked');
                
                document.getElementById('overrideSheetBtn').onclick = () => {
                    const pwd = document.getElementById('teacherOverrideSheet').value;
                    if (pwd === "0007") {
                        localStorage.clear();
                        sessionStorage.clear();
                        window.location.reload();
                    } else {
                        document.getElementById('sheetLockError').innerText = "Incorrect password.";
                    }
                };
                startBtn.disabled = false;
                return;
            }
        } catch(err) {
            authErrorMsg.innerText = "⚠️ Could not connect to Google Sheets.";
            console.warn(err);
        }
        
        globalStudentId = studentId;
        globalStudentName = studentName;
        currentStudentId = studentId;
        
        // تسجيل وقت البدء
        startTime = new Date();
        
        localStorage.setItem('current_worksheet_student', studentId);
        document.body.classList.add('authenticated');
        workspace.style.display = 'block';
        authErrorMsg.innerText = "";
        startBtn.disabled = false;
    });
    
    // --------------------- GRADING & SUBMIT WITH SHEETS SAVE ---------------------
    const act1Keys = {
        act1_q1: /without/i,
        act1_q2: /(pictures|symbols|emojis)/i,
        act1_q3: /(picture|written|hieroglyphic)/i,
        act1_q4: /(world|international)/i
    };
    const act2Keys = { act2_q1: 'T', act2_q2: 'F', act2_q3: 'T', act2_q4: 'F', act2_q5: 'T' };
    const act3Keys = {
        act3_q2: /(22000|22,000|thousand)/i,
        act3_q3: /(island|islands)/i,
        act3_q4: /(valleys|gorges|mountains)/i,
        act3_q5: /(walking|traveling|travelling)/i,
        act3_q6: /(thousands|hundreds)/i,
        act3_q7: /(spanish|castilian)/i,
        act3_q8: /(1999)/i,
        act3_q9: /(2009)/i,
        act3_q10: /(tourists|visitors)/i
    };
    
    function calculateScore() {
        let score = 0;
        let a1Score = 0;
        for (let id in act1Keys) {
            let val = document.getElementById(id).value.trim();
            if (act1Keys[id].test(val)) a1Score++;
        }
        score += a1Score;
        
        let a2Score = 0;
        for (let i = 1; i <= 5; i++) {
            let rad = document.querySelector(`input[name="act2_q${i}"]:checked`);
            if (rad && rad.value === act2Keys[`act2_q${i}`]) a2Score++;
        }
        score += a2Score;
        
        let a3Score = 0;
        for (let id in act3Keys) {
            let val = document.getElementById(id).value.trim();
            if (act3Keys[id].test(val)) a3Score++;
        }
        score += a3Score;
        return { score, a1Score, a2Score, a3Score };
    }
    
    async function processGrading() {
        if (!document.body.classList.contains('authenticated')) {
            alert("Please enter your details first.");
            return;
        }
        const { score, a1Score, a2Score, a3Score } = calculateScore();
        
        // حساب الوقت المستغرق
        const endTime = new Date();
        const timeDiffSeconds = Math.round((endTime - startTime) / 1000);
        const minutes = Math.floor(timeDiffSeconds / 60);
        const seconds = timeDiffSeconds % 60;
        const timeSpent = `${minutes}m ${seconds}s`; // الصيغة: 5m 30s
        
        // حفظ النتيجة مع الاسم والوقت
        const saved = await saveResultToSheet(globalStudentId, globalStudentName, score, timeSpent);
        if (!saved) {
            if (!confirm("⚠️ Failed to reach Google Sheets. Submit locally anyway?")) return;
        }
        
        localStorage.setItem('worksheet_permanently_submitted', 'true');
        writePermanentLock();
        
        alert(`📊 EVALUATION COMPLETED\nName: ${globalStudentName}\nTime: ${timeSpent}\nTotal Score: ${score} / 18`);
        
        document.documentElement.innerHTML = `
        <head><title>Submitted</title><meta name="viewport" content="width=device-width, initial-scale=1.0"></head>
        <body style="background:#0b0f19;color:#4ade80;display:flex;flex-direction:column;justify-content:center;align-items:center;height:100vh;font-family:sans-serif;text-align:center;">
            <h2>✅ Score Logged Successfully</h2>
            <p style="color:#a0aec0;">Name: ${globalStudentName} | Score: ${score}/18 | Time: ${timeSpent}</p>
            <div style="margin-top:30px; background:#161b26; padding:20px; border-radius:20px;">
                <p style="color:#e2e8f0;">🛠️ TEACHER OVERRIDE:</p>
                <input type="password" id="fallbackOverrideInput" placeholder="Password" style="padding:10px; border-radius:30px;">
                <button id="fallbackOverrideBtn" style="background:#2563eb; color:white; margin-top:12px; border:none; padding:10px 20px; border-radius:30px;">Clear Lock</button>
            </div>
        </body>`;
        setTimeout(() => {
            document.getElementById('fallbackOverrideBtn').addEventListener('click', () => {
                if (document.getElementById('fallbackOverrideInput').value === "0007") {
                    localStorage.clear();
                    sessionStorage.clear();
                    clearPermanentLock();
                    window.location.reload();
                } else alert("Wrong code.");
            });
        }, 50);
    }
    
    document.getElementById('checkAllBtn').addEventListener('click', processGrading);
    document.getElementById('resetBtn').addEventListener('click', () => {
        if (localStorage.getItem('worksheet_permanently_submitted') === 'true') return;
        const inputs = document.querySelectorAll('input[type="text"]');
        inputs.forEach(inp => inp.value = '');
        for(let i=1; i<=5; i++) {
            document.querySelectorAll(`input[name="act2_q${i}"]`).forEach(r => r.checked = false);
        }
        updateAnswerFlag();
    });
    
    // بدء فحص القفل المحلي أولاً
    runEnforcementCheck().then(locked => {
        if (!locked) {
            // عرض واجهة الـ ID
            document.body.classList.remove('authenticated');
            workspace.style.display = 'none';
        } else {
            document.body.classList.add('sheet-locked');
        }
    });
</script>
</body>
</html>
