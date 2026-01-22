<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>深藍任務：平板專用版 (V3.0)</title>
    <style>
        :root {
            --primary-color: #00d2ff;
            --bg-color: #0a192f;
            --panel-bg: #112240;
            --accent-color: #64ffda;
            --text-color: #e6f1ff;
            --danger-color: #ff6b6b;
            --success-color: #51cf66;
            --grid-line: #1d3f5e;
            --input-bg: rgba(0,0,0,0.3);
        }

        /* 基礎設定 */
        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px; /* 平板邊緣留白 */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            /* 深海漸層背景 */
            background-image: radial-gradient(circle at 50% 50%, #1a3c6e 0%, #0a192f 100%);
            /* 防止 iOS 彈性捲動效果露出背景 */
            overscroll-behavior: none; 
        }

        .game-container {
            background-color: var(--panel-bg);
            border: 2px solid var(--primary-color);
            border-radius: 20px;
            width: 100%;
            max-width: 900px; /* 針對平板加寬 */
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
            padding: 30px;
            position: relative;
            display: flex;
            flex-direction: column;
            /* 確保內容不會溢出 */
            max-height: 95vh;
            overflow-y: auto; 
        }

        /* Header */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        h2 { margin: 0 0 10px 0; font-size: 1.8rem; }

        .mission-badge {
            background: linear-gradient(90deg, var(--primary-color), #0077be);
            color: #fff;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 1rem;
            display: inline-block;
        }

        .progress-container {
            text-align: right;
            width: 240px;
        }

        .progress-bar {
            width: 100%;
            height: 12px;
            background: rgba(255,255,255,0.1);
            border-radius: 6px;
            overflow: hidden;
            margin-top: 8px;
        }

        .progress-fill {
            height: 100%;
            background: var(--success-color);
            width: 0%;
            transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        }

        /* Content Areas */
        .question-area {
            flex: 1; /* 讓內容區填滿剩餘空間 */
        }

        .story-text {
            font-size: 1.3rem; /* 平板字體加大 */
            line-height: 1.6;
            background: rgba(0, 210, 255, 0.08);
            padding: 20px;
            border-left: 6px solid var(--primary-color);
            border-radius: 0 12px 12px 0;
            margin-bottom: 25px;
        }

        .visual-display {
            background: rgba(0,0,0,0.25);
            border-radius: 16px;
            padding: 30px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 220px; /* 確保圖形夠大 */
            margin-bottom: 25px;
            border: 2px solid rgba(255,255,255,0.05);
        }

        /* Data Tables */
        .data-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 1.1rem;
        }
        .data-table th, .data-table td {
            border: 1px solid var(--grid-line);
            padding: 15px; /* 加大觸控間距 */
            text-align: center;
        }
        .data-table th { background: rgba(0, 210, 255, 0.15); color: var(--primary-color); }

        /* Ocean Grid (Tablet Optimized) */
        .ocean-grid {
            display: grid;
            /* 平板上格子加大至 55px */
            grid-template-columns: repeat(6, 55px);
            grid-gap: 2px;
            background: var(--grid-line);
            padding: 5px;
            border: 3px solid #5a7d9a;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }
        
        .cell {
            width: 55px;
            height: 55px;
            background: #1b4965;
            position: relative;
        }
        
        .cell.oil-full { background: #111; }
        .cell.oil-full::after {
            content: ''; position: absolute; top: 3px; left: 3px; right: 3px; bottom: 3px;
            border: 1px dashed #444;
        }

        /* Triangles with clip-path (High DPI friendly) */
        .cell.oil-tri-tl { background: #111; clip-path: polygon(0 0, 100% 0, 0 100%); }
        .cell.oil-tri-tr { background: #111; clip-path: polygon(0 0, 100% 0, 100% 100%); }
        .cell.oil-tri-bl { background: #111; clip-path: polygon(0 0, 0 100%, 100% 100%); }
        .cell.oil-tri-br { background: #111; clip-path: polygon(100% 0, 100% 100%, 0 100%); }

        /* Clock Visual */
        .clock-face {
            width: 180px; /* 加大 */
            height: 180px;
            border: 8px solid var(--accent-color);
            border-radius: 50%;
            position: relative;
            background: radial-gradient(circle, #222 0%, #111 100%);
            box-shadow: 0 0 25px rgba(100, 255, 218, 0.2);
        }
        .clock-mark {
            position: absolute; width: 4px; height: 15px; background: #666;
            left: 50%; transform: translateX(-50%);
        }
        .hand {
            position: absolute; bottom: 50%; left: 50%;
            transform-origin: bottom center; background: #fff;
            border-radius: 4px; box-shadow: 0 2px 5px rgba(0,0,0,0.5);
        }
        .hand.hour { height: 50px; width: 8px; background: var(--primary-color); }
        .hand.minute { height: 70px; width: 6px; }
        .center-dot {
            position: absolute; top: 50%; left: 50%;
            width: 16px; height: 16px; background: var(--accent-color);
            border-radius: 50%; transform: translate(-50%, -50%);
        }

        /* Inputs & Buttons (Tablet Friendly) */
        .input-group {
            display: flex;
            gap: 15px;
            margin-top: 25px;
        }

        input[type="number"] {
            flex: 1;
            padding: 15px 20px;
            border-radius: 12px;
            border: 2px solid var(--primary-color);
            background: var(--input-bg);
            color: #fff;
            font-size: 1.5rem; /* 大字體防止 iOS 自動縮放 */
            outline: none;
            -webkit-appearance: none; /* 移除 iOS 預設陰影 */
        }
        input[type="number"]:focus {
            background: rgba(0, 210, 255, 0.1);
            box-shadow: 0 0 0 4px rgba(0, 210, 255, 0.2);
        }

        button {
            padding: 15px 35px;
            border: none;
            border-radius: 12px;
            font-weight: bold;
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.2s;
            /* 解決觸控延遲 */
            touch-action: manipulation; 
        }

        .btn-submit {
            background: var(--primary-color);
            color: #000;
            box-shadow: 0 4px 15px rgba(0, 210, 255, 0.3);
        }
        .btn-submit:active { transform: scale(0.96); }

        .btn-option {
            background: transparent;
            color: var(--primary-color);
            border: 2px solid var(--primary-color);
            flex: 1;
            min-height: 60px; /* 增加點擊高度 */
            font-size: 1.1rem;
        }
        .btn-option.selected {
            background: var(--primary-color);
            color: #000;
            font-weight: 800;
        }

        /* Feedback Area */
        .feedback {
            margin-top: 25px;
            padding: 20px;
            border-radius: 12px;
            font-size: 1.2rem;
            display: none;
            animation: fadeIn 0.4s ease-out;
        }
        .feedback.correct { 
            background: rgba(81, 207, 102, 0.15); 
            border: 1px solid var(--success-color); 
            color: #a5ffd6;
        }
        .feedback.wrong { 
            background: rgba(255, 107, 107, 0.15); 
            border: 1px solid var(--danger-color); 
            color: #ffc9c9;
        }

        .hidden { display: none !important; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* 📱 Mobile Specific (手機直式) */
        @media (max-width: 600px) {
            body { padding: 10px; }
            .game-container { padding: 15px; max-height: 100vh; border-radius: 0; border: none; }
            .ocean-grid { grid-template-columns: repeat(6, 40px); }
            .cell { width: 40px; height: 40px; }
            h2 { font-size: 1.5rem; }
            .story-text { font-size: 1.1rem; }
            .input-group { flex-direction: column; }
            button { width: 100%; }
        }
    </style>
</head>
<body>

<div class="game-container">
    <div class="header">
        <div>
            <h2>🚢 海神號：深藍任務</h2>
            <div style="margin-top:5px;" class="mission-badge" id="level-badge">準備中...</div>
        </div>
        <div class="progress-container">
            <div style="display:flex; justify-content:space-between; align-items:flex-end;">
                <small>電力狀況</small>
                <div id="score-display" style="font-size: 1.1rem; color: var(--accent-color); font-weight:bold;">積分: 0</div>
            </div>
            <div class="progress-bar">
                <div class="progress-fill" id="progress-fill"></div>
            </div>
        </div>
    </div>

    <div id="intro-screen">
        <div class="story-text">
            <strong>艦長您好！歡迎回到「海神號」。</strong><br><br>
            雷達偵測到太平洋出現大量海洋垃圾與油汙。我們需要您的數學能力來指揮機器手臂、導航船隻並計算汙染面積。<br><br>
            ⚠️ <strong>平板操作說明：</strong><br>
            1. 點擊輸入框會彈出鍵盤，輸入後按「確認指令」。<br>
            2. 圖形可以放大查看（如果需要）。<br>
            3. 請準備好紙筆進行計算。
        </div>
        <div style="text-align: center; margin-top: 40px;">
            <button class="btn-submit" onclick="startGame()" style="font-size: 1.4rem; padding: 20px 60px;">啟動引擎 🚀</button>
        </div>
    </div>

    <div id="game-screen" class="hidden">
        <div class="question-area">
            <div id="q-text" class="story-text">題目載入中...</div>
            
            <div id="q-visual" class="visual-display">
                </div>

            <div id="input-number" class="input-group hidden">
                <input type="number" id="user-answer" placeholder="點此輸入數字答案" inputmode="decimal">
                <button id="btn-num-submit" class="btn-submit" onclick="checkAnswer()">確認指令</button>
            </div>

            <div id="input-options" class="input-group hidden">
                </div>
        </div>

        <div id="feedback-box" class="feedback"></div>
        
        <div style="display: flex; justify-content: space-between; margin-top: 30px; gap: 20px;">
            <button class="btn-option" style="flex: 1; border: 1px dashed #666; color: #aaa;" onclick="showHint()">💡 呼叫大副提示</button>
            <button id="btn-next" class="btn-submit hidden" style="flex: 2;" onclick="nextQuestion()">下一題 ➡️</button>
        </div>
    </div>

    <div id="end-screen" class="hidden" style="text-align: center; padding: 20px;">
        <h1 style="color: var(--accent-color); font-size: 3rem; margin-bottom: 20px;">任務完成！</h1>
        <div class="story-text" style="text-align: center;">
            恭喜艦長！<br>
            您成功淨化了海域，所有的計算都精準無誤。<br>
            海神號已安全返航。
        </div>
        <div style="background: rgba(0,0,0,0.3); padding: 30px; border-radius: 20px; margin: 40px 0; border: 2px solid var(--primary-color);">
            <div style="font-size: 1.2rem; color: #888;">最終積分</div>
            <div id="final-score" style="font-size: 4rem; font-weight: bold; color: var(--primary-color); margin: 10px 0;">0</div>
            <div id="final-rank" style="font-size: 1.5rem; color: var(--accent-color);"></div>
        </div>
        <button class="btn-submit" style="width: 100%;" onclick="location.reload()">重新挑戰</button>
    </div>
</div>

<script>
    // 題目資料庫 (保持原樣，僅優化顯示HTML)
    const questions = [
        {
            type: "number",
            text: "【第一關：物資運算】<br>船員小明今天回收了 <strong>35 磚</strong> 寶特瓶。<br>每磚 8 分。特殊規則：『每收集滿 5 磚，額外加送 10 分』。<br>請問總共獲得多少積分？",
            visual: `
                <table class="data-table">
                    <tr><th>物品</th><th>基礎分</th><th>特殊規則</th></tr>
                    <tr><td>寶特瓶磚</td><td>8 分/磚</td><td>滿 5 磚送 10 分</td></tr>
                </table>`,
            answer: 350,
            hint: "步驟 1：先算基礎分 (35 x 8)。<br>步驟 2：算獎勵。35 裡面有幾個 5？就有幾次 10 分。",
            explanation: "基礎分：35 x 8 = 280 分。<br>獎勵次數：35 ÷ 5 = 7 次。<br>獎勵分：7 x 10 = 70 分。<br>總分：280 + 70 = 350 分。"
        },
        {
            type: "number",
            text: "【第一關：物資運算】<br>撈起 <strong>6 張</strong> 幽靈漁網 (每張 50 分)。<br>但每張網子消耗 5 單位電力，每單位電力成本視為 2 分。<br>請問扣除成本後，淨賺多少分？",
            visual: `<div style="font-size: 4rem; display:flex; gap:20px; align-items:center; justify-content:center;">🕸️ x 6 <span style="font-size:0.4em; color:#888; border:1px solid #555; padding:5px; border-radius:5px;">⚠️ 耗電警告</span></div>`,
            answer: 240,
            hint: "收入 = 6 x 50。<br>消耗電力 = 6 x 5 = 30 單位。<br>電力成本 = 30 x 2。<br>最後相減。",
            explanation: "總收入：6 x 50 = 300 分。<br>總耗電：6 x 5 = 30 單位。<br>總成本：30 x 2 = 60 分。<br>淨賺：300 - 60 = 240 分。"
        },
        {
            type: "option",
            options: ["鈍角", "直角", "銳角"],
            text: "【第二關：雷達導航】<br>暴風雨中，船身先向右轉了一個「直角」，再繼續向右轉了一個「銳角」。<br>請問現在船身轉過的角度總和，是什麼角？",
            visual: `<div style="text-align:center;"><span style="font-size:5rem;">🚢</span><br><span style="color:var(--accent-color); font-size: 1.5rem;">90° + ?</span></div>`,
            answer: "鈍角",
            hint: "直角是 90 度。<br>銳角是大於 0 度。<br>加起來一定大於 90 度，且小於 180 度。",
            explanation: "直角(90度) + 銳角(例如30度) = 120度。<br>大於90且小於180的角稱為鈍角。"
        },
        {
            type: "option",
            options: ["大於直角", "等於直角", "小於直角"],
            text: "【第二關：雷達導航】<br>觀察雷達時鐘。<br>3:00 是直角，6:00 是平角。<br>請問 <strong>4:00</strong> 時，指針張開的角度是？",
            visual: `
                <div class="clock-face">
                    <div class="clock-mark" style="transform: rotate(0deg) translateY(5px);"></div>
                    <div class="clock-mark" style="transform: rotate(90deg) translateY(5px);"></div>
                    <div class="clock-mark" style="transform: rotate(180deg) translateY(5px);"></div>
                    <div class="clock-mark" style="transform: rotate(270deg) translateY(5px);"></div>
                    <div class="hand hour" style="transform: rotate(120deg);"></div>
                    <div class="hand minute" style="transform: rotate(0deg);"></div>
                    <div class="center-dot"></div>
                </div>`,
            answer: "大於直角",
            hint: "3點是90度。4點的指針張得比3點更開，所以角度更大。",
            explanation: "每個小時鐘面走 30 度。<br>3點是 90 度 (直角)。<br>4點是 120 度 (鈍角)，所以比直角大。"
        },
        {
            type: "option",
            options: ["大副 (兩銳角)", "二副 (一銳一直)", "都不對"],
            text: "【第二關：圖形推理】<br>海圖上有一個三角形，其中有一個角是「鈍角」。<br>大副說：『那另外兩個角一定都是銳角！』<br>二副說：『不一定，可能還有一個是直角。』<br>誰說得對？",
            visual: `<svg width="200" height="100" viewBox="0 0 200 100"><polygon points="30,80 170,80 70,20" style="fill:none;stroke:#00d2ff;stroke-width:4" /><text x="70" y="70" fill="white" font-size="16">鈍角?</text></svg>`,
            answer: "大副 (兩銳角)",
            hint: "三角形內角和是 180 度。<br>鈍角已經超過 90 度了，剩下兩個角加起來不到 90 度。",
            explanation: "因為鈍角 > 90度，若還有一個直角(90度)，加起來就超過 180 度了，無法形成三角形。所以剩下兩個一定很小(銳角)。"
        },
        {
            type: "number",
            text: "【第三關：汙染測量】<br>請觀察下方的油汙網格圖。<br>黑色(⬛)是完整 1 平方公里，三角形是半格。<br>請問這片油汙的總面積是多少平方公里？",
            visual: `
                <div class="ocean-grid">
                    <div class="cell"></div><div class="cell"></div><div class="cell"></div><div class="cell"></div><div class="cell"></div><div class="cell"></div>
                    <div class="cell"></div><div class="cell oil-tri-tr"></div><div class="cell oil-full"></div><div class="cell oil-full"></div><div class="cell oil-tri-tl"></div><div class="cell"></div>
                    <div class="cell"></div><div class="cell oil-full"></div><div class="cell oil-full"></div><div class="cell oil-full"></div><div class="cell oil-full"></div><div class="cell"></div>
                    <div class="cell"></div><div class="cell oil-tri-br"></div><div class="cell oil-full"></div><div class="cell oil-full"></div><div class="cell oil-tri-bl"></div><div class="cell"></div>
                    <div class="cell"></div><div class="cell"></div><div class="cell"></div><div class="cell"></div><div class="cell"></div><div class="cell"></div>
                </div>
                <div style="margin-top:15px; font-size:1rem; color: #aaa;">💡 提示：請觀察三角形的方向，兩個三角形 = 一個正方形</div>
            `,
            answer: 10,
            hint: "先數完整的黑色正方形有幾個。<br>再數三角形有幾個 (4個)，把它們兩兩拼湊成正方形。",
            explanation: "完整格子：中間一排4個 + 上下排中間各2個 = 8 個。<br>三角形：4 個，拼成 2 個完整格。<br>總計：8 + 2 = 10 平方公里。"
        },
        {
            type: "option",
            options: ["甲比較長", "乙比較長", "一樣長"],
            text: "【第三關：圍籬迷思】<br>比較兩個面積相同的形狀：<br>甲：面積 4 的正方形 (邊長2)。<br>乙：面積 4 的長條形 (長4寬1)。<br>哪一個需要的攔油索(周長)比較長？",
            visual: `<div style="display:flex; gap:40px; justify-content:center; align-items:flex-end;">
                        <div style="text-align:center;">
                            <div style="border:3px solid #fff; width:60px; height:60px; margin:0 auto; background:rgba(255,255,255,0.1);"></div>
                            <span style="font-size:1.2rem">甲</span>
                        </div>
                        <div style="text-align:center;">
                            <div style="border:3px solid #fff; width:120px; height:30px; margin:0 auto; background:rgba(255,255,255,0.1);"></div>
                            <span style="font-size:1.2rem">乙</span>
                        </div>
                     </div>`,
            answer: "乙比較長",
            hint: "算算看周長。<br>甲邊長是 2 (2x2=4)。周長是 2+2+2+2。<br>乙長4寬1 (4x1=4)。周長是 4+1+4+1。",
            explanation: "甲周長 = 2 x 4 = 8。<br>乙周長 = (4+1) x 2 = 10。<br>乙比較長。形狀越扁長，周長通常越長。"
        },
        {
            type: "number",
            text: "【第三關：圖形切割】<br>甲板長 8 公尺、寬 5 公尺。<br>中間破了一個邊長 2 公尺的「正方形」破洞。<br>請問剩下的面積是多少？",
            visual: `<div style="width:200px; height:125px; background:#444; position:relative; margin:0 auto; border:3px solid #888;">
                        <div style="width:50px; height:50px; background:#000; position:absolute; top:35px; left:75px; border:2px dashed #ff6b6b; display:flex; justify-content:center; align-items:center; color:#ff6b6b; font-size:0.9rem;">破洞</div>
                     </div>`,
            answer: 36,
            hint: "大長方形面積 - 小正方形面積。",
            explanation: "大面積：8 x 5 = 40。<br>破洞面積：2 x 2 = 4。<br>剩下：40 - 4 = 36 平方公尺。"
        }
    ];

    let currentQIndex = 0;
    let score = 0;
    let isRetry = false;

    // Enter Key Support
    document.getElementById('user-answer').addEventListener("keypress", function(event) {
        if (event.key === "Enter") {
            event.preventDefault();
            checkAnswer();
            // 在平板上，按下 Enter 後建議讓輸入框失去焦點，以收起鍵盤，讓學生看到 Feedback
            document.getElementById('user-answer').blur();
        }
    });

    function startGame() {
        document.getElementById('intro-screen').classList.add('hidden');
        document.getElementById('game-screen').classList.remove('hidden');
        loadQuestion();
    }

    function loadQuestion() {
        isRetry = false;
        const q = questions[currentQIndex];
        
        document.getElementById('btn-next').classList.add('hidden');
        document.getElementById('feedback-box').style.display = 'none';
        document.getElementById('feedback-box').className = 'feedback';
        document.getElementById('user-answer').disabled = false;
        document.getElementById('btn-num-submit').disabled = false;
        document.getElementById('user-answer').value = ''; // Clear input

        // Progress
        const progressPct = ((currentQIndex) / questions.length) * 100;
        document.getElementById('progress-fill').style.width = `${progressPct}%`;
        
        // Level Badge
        const badge = document.getElementById('level-badge');
        if(currentQIndex < 2) badge.innerText = "第一關：運算中心";
        else if(currentQIndex < 5) badge.innerText = "第二關：角度導航";
        else badge.innerText = "第三關：面積測量";

        // Render Text & Visual
        document.getElementById('q-text').innerHTML = q.text;
        document.getElementById('q-visual').innerHTML = q.visual;
        
        // Input Logic
        const numInput = document.getElementById('input-number');
        const optInput = document.getElementById('input-options');

        if (q.type === 'number') {
            numInput.classList.remove('hidden');
            optInput.classList.add('hidden');
            // 平板上不要自動 focus，避免鍵盤直接彈出遮住題目
        } else {
            numInput.classList.add('hidden');
            optInput.classList.remove('hidden');
            optInput.innerHTML = '';
            q.options.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = 'btn-option';
                btn.innerText = opt;
                btn.onclick = () => checkOption(opt, btn);
                optInput.appendChild(btn);
            });
        }
    }

    function showHint() {
        const q = questions[currentQIndex];
        alert("💡 大副提示：\n" + q.hint.replace(/<br>/g, "\n"));
    }

    function checkAnswer() {
        const userField = document.getElementById('user-answer');
        if(!userField.value) {
            alert("請先輸入數字答案喔！");
            return;
        }
        const userVal = parseFloat(userField.value);
        const q = questions[currentQIndex];
        handleResult(userVal === q.answer);
    }

    function checkOption(selectedVal, btnElement) {
        document.querySelectorAll('.btn-option').forEach(b => b.classList.remove('selected'));
        btnElement.classList.add('selected');
        const q = questions[currentQIndex];
        handleResult(selectedVal === q.answer);
    }

    function handleResult(isCorrect) {
        const fbBox = document.getElementById('feedback-box');
        const q = questions[currentQIndex];
        
        fbBox.style.display = 'block';
        
        if (isCorrect) {
            const points = isRetry ? 5 : 10;
            score += points;
            document.getElementById('score-display').innerText = `積分: ${score}`;

            fbBox.innerHTML = `<strong>✅ 正確！(+${points}分)</strong><br>${q.explanation}`;
            fbBox.classList.add('correct');
            fbBox.classList.remove('wrong');
            
            // Lock UI
            document.getElementById('user-answer').disabled = true;
            document.getElementById('btn-num-submit').disabled = true;
            document.querySelectorAll('.btn-option').forEach(b => b.disabled = true);
            document.getElementById('btn-next').classList.remove('hidden');
            
        } else {
            isRetry = true;
            fbBox.innerHTML = `<strong>⚠️ 運算誤差</strong><br>再試一次，或者點選「呼叫大副提示」。`;
            fbBox.classList.add('wrong');
            fbBox.classList.remove('correct');
        }
    }

    function nextQuestion() {
        currentQIndex++;
        if (currentQIndex < questions.length) {
            loadQuestion();
        } else {
            endGame();
        }
    }

    function endGame() {
        document.getElementById('game-screen').classList.add('hidden');
        document.getElementById('end-screen').classList.remove('hidden');
        document.getElementById('progress-fill').style.width = '100%';
        
        const maxScore = questions.length * 10;
        let rank = "見習水手 ⚓";
        let color = "#888";

        if(score >= maxScore * 0.9) { rank = "傳奇艦長 🏆"; color = "#ffd700"; }
        else if(score >= maxScore * 0.7) { rank = "資深大副 🥇"; color = "#c0c0c0"; }
        else if(score >= maxScore * 0.5) { rank = "正式船員 🥈"; color = "#cd7f32"; }

        document.getElementById('final-score').innerText = score;
        document.getElementById('final-rank').innerHTML = `獲得稱號：<span style="color:${color}; font-weight:bold;">${rank}</span>`;
    }
</script>

</body>
</html>
