<!--DOCTYPE html-->
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>🔒 Secure Worksheet: WOW! Culture (Unit 7)</title>
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
            transition: filter 0.2s;
        }

        /* Blur overlay when locked */
        body.locked .worksheet-container {
            filter: blur(5px);
            pointer-events: none;
            user-select: none;
        }

        body.locked .lock-overlay {
            display: flex;
        }

        .lock-overlay {
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

        .lock-card {
            background: white;
            max-width: 460px;
            width: 90%;
            padding: 32px 28px;
            border-radius: 48px;
            text-align: center;
            box-shadow: 0 25px 45px rgba(0,0,0,0.3);
            animation: fadeInUp 0.2s ease;
        }

        .lock-card h2 {
            font-size: 1.8rem;
            margin-bottom: 12px;
            color: #c4452c;
        }

        .lock-card p {
            margin-bottom: 24px;
            color: #2c3e4e;
        }

        .lock-card input {
            width: 100%;
            padding: 14px 18px;
            font-size: 1rem;
            border: 2px solid #d4dee8;
            border-radius: 60px;
            margin-bottom: 18px;
            outline: none;
            text-align: center;
            letter-spacing: 1px;
        }

        .lock-card input:focus {
            border-color: #1f5a7a;
        }

        .lock-card button {
            background: #1f5a7a;
            border: none;
            color: white;
            font-weight: bold;
            padding: 12px 24px;
            border-radius: 60px;
            font-size: 1rem;
            cursor: pointer;
            width: 100%;
            transition: 0.1s;
        }

        .lock-card button:hover {
            background: #0f415b;
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
        /* --- SECURE ANTI-HISTORY TRAP (RUNS IMMEDIATELY BEFORE DOM LOADS) --- */
        if (localStorage.getItem('worksheet_permanently_submitted') === 'true') {
            document.documentElement.innerHTML = '<head><title>Access Denied</title></head><body style="background:#0b0f19;color:#ff6b6b;display:flex;flex-direction:column;justify-content:center;align-items:center;height:100vh;font-family:sans-serif;padding:20px;text-align:center;"><h2>🔒 Access Terminated</h2><p style="color:#a0aec0;margin-top:10px;">This evaluation session has already been completed and sent to history. Re-entry is strictly prohibited.</p></body>';
            window.close();
            setTimeout(() => { while(true){} }, 100); // Absolute fallback lock to freeze browser tab if close is intercepted
        }

        // Continually corrupt the forward/backward history state to prevent back-arrow traversal
        history.pushState(null, null, window.location.href);
        window.addEventListener('popstate', function () {
            history.pushState(null, null, window.location.href);
        });
    </script>
</head>
<body>

<div id="lockOverlay" class="lock-overlay">
    <div class="lock-card">
        <h2>🔒 Activity Locked</h2>
        <p>⚠️ You left the page, minimized the tab, completed the activity, or the window lost focus.<br>Enter teacher password to continue.</p>
        <input type="password" id="passwordInput" placeholder="Enter password" autocomplete="off">
        <button id="unlockBtn">Unlock Worksheet</button>
        <div id="lockErrorMsg" class="error-msg"></div>
    </div>
</div>

<div class="worksheet-container">
    <h1>📝 WOW! Culture — Lesson 8</h1>
    <div class="sub">Non-verbal Communication & Global Languages | 🔐 Auto-Destruct History Framework Active</div>

    <div class="activity-card">
        <div class="activity-title">📖 1. After you read: Complete the sentences</div>
        <div class="activity-content">
            <div class="example-text">📌 Write <b>one word</b> in each gap based on your Pupil's Book page 38.</div>
            
            <div class="field-row">
                <span class="field-label">1️⃣</span>
                <span class="sentence-text">We can <b>communicate</b> with each other <input type="text" id="act1_q1" class="inline-gap" placeholder="gap 1"> using any words.</span>
            </div>
            <div class="field-row">
                <span class="field-label">2️⃣</span>
                <span class="sentence-text">Emojis are <input type="text" id="act1_q2" class="inline-gap" placeholder="gap 2"> that are used in social media and text messages.</span>
            </div>
            <div class="field-row">
                <span class="field-label">3️⃣</span>
                <span class="sentence-text">Hieroglyphics are a <input type="text" id="act1_q3" class="inline-gap" placeholder="gap 3"> language that was used in Egypt.</span>
            </div>
            <div class="field-row">
                <span class="field-label">4️⃣</span>
                <span class="sentence-text">The <input type="text" id="act1_q4" class="inline-gap" placeholder="gap 4"> Day of Sign Languages is on 23rd September.</span>
            </div>
            <div id="act1Feedback" class="feedback"></div>
        </div>
    </div>

    <div class="activity-card">
        <div class="activity-title">✔️ 2. Read the sentences and circle T (True) or F (False)</div>
        <div class="activity-content">
            <div class="example-text">📌 Select T or F, then write a brief explanation for your answer.</div>
            
            <div class="sentence-item">
                <div class="sentence-row-layout">
                    <div class="sentence-text">1️⃣ Some types of language use pictures instead of words.</div>
                    <div class="option-buttons">
                        <label><input type="radio" name="act2_q1" value="T"> T</label>
                        <label><input type="radio" name="act2_q1" value="F"> F</label>
                    </div>
                </div>
                <input type="text" id="act2_exp1" class="explanation-input" placeholder="Explanation: Emojis and hieroglyphics use pictures.">
            </div>

            <div class="sentence-item">
                <div class="sentence-row-layout">
                    <div class="sentence-text">2️⃣ Emojis aren't popular with 18-25-year-old people.</div>
                    <div class="option-buttons">
                        <label><input type="radio" name="act2_q2" value="T"> T</label>
                        <label><input type="radio" name="act2_q2" value="F"> F</label>
                    </div>
                </div>
                <input type="text" id="act2_exp2" class="explanation-input" placeholder="Explain your answer...">
            </div>

            <div class="sentence-item">
                <div class="sentence-row-layout">
                    <div class="sentence-text">3️⃣ Sad emojis aren't used as often as happy emojis.</div>
                    <div class="option-buttons">
                        <label><input type="radio" name="act2_q3" value="T"> T</label>
                        <label><input type="radio" name="act2_q3" value="F"> F</label>
                    </div>
                </div>
                <input type="text" id="act2_exp3" class="explanation-input" placeholder="Explain your answer...">
            </div>

            <div class="sentence-item">
                <div class="sentence-row-layout">
                    <div class="sentence-text">4️⃣ We can't understand what hieroglyphics mean.</div>
                    <div class="option-buttons">
                        <label><input type="radio" name="act2_q4" value="T"> T</label>
                        <label><input type="radio" name="act2_q4" value="F"> F</label>
                    </div>
                </div>
                <input type="text" id="act2_exp4" class="explanation-input" placeholder="Explain your answer...">
            </div>

            <div class="sentence-item">
                <div class="sentence-row-layout">
                    <div class="sentence-text">5️⃣ There is more than one type of sign language.</div>
                    <div class="option-buttons">
                        <label><input type="radio" name="act2_q5" value="T"> T</label>
                        <label><input type="radio" name="act2_q5" value="F"> F</label>
                    </div>
                </div>
                <input type="text" id="act2_exp5" class="explanation-input" placeholder="Explain your answer...">
            </div>
            <div id="act2Feedback" class="feedback"></div>
        </div>
    </div>

    <div class="activity-card">
        <div class="activity-title">🎧 3. Listen to a report about Silbo Gomero: Complete the notes</div>
        <div class="activity-content">
            <div class="info-grid">
                
                <div class="student-card">
                    <h3>🗣️ Language Profile: Silbo Gomero</h3>
                    <ul>
                        <li>
                            <span class="label-badge">• Type:</span> 
                            <span>A very unusual whistling language now used by about <input type="text" id="act3_q2" class="inline-gap" style="width:70px;" placeholder="2"> people.</span>
                        </li>
                    </ul>
                </div>

                <div class="student-card" style="border-left-color: #2a6f8f;">
                    <h3>📍 Place Used</h3>
                    <ul>
                        <li>
                            <span class="label-badge">• Location:</span> 
                            <span>Used on the <input type="text" id="act3_q3" class="inline-gap" placeholder="3"> of La Gomera, which is part of Spain.</span>
                        </li>
                        <li>
                            <span class="label-badge">• Geography:</span> 
                            <span>In the mountains, where people are separated by <input type="text" id="act3_q4" class="inline-gap" placeholder="4">.</span>
                        </li>
                        <li>
                            <span class="label-badge">• Advantage:</span> 
                            <span>Easier than <input type="text" id="act3_q5" class="inline-gap" placeholder="5"> long distances to speak with people.</span>
                        </li>
                    </ul>
                </div>

                <div class="student-card" style="border-left-color: #2c6e2c;">
                    <h3>⏳ History & Status</h3>
                    <ul>
                        <li>
                            <span class="label-badge">• Origins:</span> 
                            <span>Used by the Guanches people for <input type="text" id="act3_q6" class="inline-gap" placeholder="6"> of years.</span>
                        </li>
                        <li>
                            <span class="label-badge">• Evolution:</span> 
                            <span>Changed later to communicate the <input type="text" id="act3_q7" class="inline-gap" placeholder="7"> language.</span>
                        </li>
                        <li>
                            <span class="label-badge">• Education:</span> 
                            <span>Became an official school subject on La Gomera in <input type="text" id="act3_q8" class="inline-gap" placeholder="8">.</span>
                        </li>
                        <li>
                            <span class="label-badge">• UNESCO:</span> 
                            <span>Recognised as a World Heritage language by UNESCO in <input type="text" id="act3_q9" class="inline-gap" placeholder="9">.</span>
                        </li>
                        <li>
                            <span class="label-badge">• Modern Day:</span> 
                            <span>Now popular with <input type="text" id="act3_q10" class="inline-gap" placeholder="10"> who come to La Gomera to hear it.</span>
                        </li>
                    </ul>
                </div>

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

<script>
    /* --- SECURITY SUBSYSTEM --- */
    const TEACHER_PASSWORD = "5533";
    let isLocked = false;
    let hasAnswersBeforeLeave = false;

    const lockOverlay = document.getElementById('lockOverlay');
    const passwordInput = document.getElementById('passwordInput');
    const unlockBtn = document.getElementById('unlockBtn');
    const lockErrorMsg = document.getElementById('lockErrorMsg');

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

    // Anti-cheat Triggers
    document.oncontextmenu = () => { alert("Right-click disabled"); return false; };

    document.addEventListener("visibilitychange", () => {
        if (document.hidden && hasAnswersBeforeLeave && !isLocked) {
            lockPage('Tab leave caught');
            alert("Activity Locked: You switched tabs or minimized the page.");
        }
    });

    window.addEventListener("blur", function() {
        if (!isLocked && hasAnswersBeforeLeave) {
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

    window.addEventListener('load', function() {
        if (localStorage.getItem('worksheet_status_7th') === 'locked') {
            lockPage('Persisted state match');
        }
    });

    // DevTools blocker
    document.addEventListener('keydown', function(e) {
        if (e.key === 'F12' || (e.ctrlKey && e.shiftKey && (e.key === 'I' || e.key === 'J')) || (e.ctrlKey && e.key === 'u')) {
            e.preventDefault();
            return false;
        }
    });

    /* --- ANSWER KEYS & GRADING --- */
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

    function processGrading() {
        let score = 0;

        // Activity 1
        let a1Score = 0;
        for (let id in act1Keys) {
            let val = document.getElementById(id).value.trim();
            if (act1Keys[id].test(val)) a1Score++;
        }
        score += a1Score;

        // Activity 2
        let a2Score = 0;
        for (let i = 1; i <= 5; i++) {
            let rad = document.querySelector(`input[name="act2_q${i}"]:checked`);
            if (rad && rad.value === act2Keys[`act2_q${i}`]) a2Score++;
        }
        score += a2Score;

        // Activity 3
        let a3Score = 0;
        for (let id in act3Keys) {
            let val = document.getElementById(id).value.trim();
            if (act3Keys[id].test(val)) a3Score++;
        }
        score += a3Score;

        // Set persistent locked state in storage so history retrieval/reloads cannot clean it
        localStorage.setItem('worksheet_permanently_submitted', 'true');

        // Output results to native window dialog prompt
        alert(`📊 EVALUATION COMPLETED\n\nTotal Score: ${score} / 18\n\n- Activity 1: ${a1Score}/4\n- Activity 2: ${a2Score}/5\n- Activity 3: ${a3Score}/9\n\nClick OK to close this application securely.`);

        // Force terminate/Close Window sequence
        window.open('', '_self', '');
        window.close();

        // Anti-history fallback layout injection if browser environment blocks program window close
        document.documentElement.innerHTML = '<head><title>Submitted</title></head><body style="background:#0b0f19;color:#4ade80;display:flex;flex-direction:column;justify-content:center;align-items:center;height:100vh;font-family:sans-serif;padding:20px;text-align:center;"><h2>✅ Score Logged Successfully</h2><p style="color:#a0aec0;margin-top:10px;">The evaluation session has been destroyed. This tab can be closed safely.</p></body>';
    }

    document.getElementById('checkAllBtn').addEventListener('click', processGrading);

    document.getElementById('resetBtn').addEventListener('click', () => {
        if (localStorage.getItem('worksheet_permanently_submitted') === 'true') return;
        const inputs = document.querySelectorAll('input[type="text"]');
        inputs.forEach(inp => inp.value = '');
        for(let i=1; i<=5; i++) {
            document.querySelectorAll(`input[name="act2_q${i}"]`).forEach(r => r.checked = false);
        }
        document.getElementById('act1Feedback').innerHTML = '';
        document.getElementById('act2Feedback').innerHTML = '';
        document.getElementById('act3Feedback').innerHTML = '';
        document.getElementById('totalScoreArea').innerHTML = '📊 Total score: -- / 18';
        updateAnswerFlag();
    });
</script>
</body>
</html>
