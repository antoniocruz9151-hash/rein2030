<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>CyberType: Pro Roadmap</title>
  <style>
    :root {
      --bg: #090d16;
      --card-bg: #111827;
      --accent: #38bdf8;
      --accent-glow: rgba(56, 189, 248, 0.4);
      --success: #10b981;
      --error: #ef4444;
      --text: #f1f5f9;
      --muted: #64748b;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'JetBrains Mono', 'Fira Code', monospace, sans-serif; }
    body { background: var(--bg); color: var(--text); min-height: 100vh; display: flex; justify-content: center; align-items: center; padding: 16px; }
    
    .arena-container {
      background: var(--card-bg);
      border: 1px solid #1e293b;
      border-radius: 16px;
      width: 100%;
      max-width: 750px;
      padding: 24px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.8), 0 0 20px var(--accent-glow);
    }
    
    .top-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; border-bottom: 1px solid #1e293b; padding-bottom: 15px; }
    .level-badge { background: #1e293b; color: var(--accent); border: 1px solid var(--accent); padding: 6px 14px; border-radius: 20px; font-weight: bold; font-size: 0.85rem; letter-spacing: 1px; }
    
    .progress-track { width: 100%; height: 6px; background: #1e293b; border-radius: 4px; margin-bottom: 20px; overflow: hidden; }
    .progress-fill { height: 100%; background: linear-gradient(90deg, var(--accent), var(--success)); width: 0%; transition: width 0.3s ease; }

    .stats-hud { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-bottom: 20px; }
    .stat-card { background: #0b0f19; padding: 10px; border-radius: 8px; border: 1px solid #1e293b; text-align: center; }
    .stat-title { font-size: 0.7rem; color: var(--muted); text-transform: uppercase; }
    .stat-value { font-size: 1.3rem; font-weight: bold; color: var(--accent); margin-top: 2px; }

    .lesson-meta { font-size: 0.85rem; color: #94a3b8; margin-bottom: 10px; display: flex; justify-content: space-between; }
    
    .text-display {
      background: #060911;
      padding: 20px;
      border-radius: 10px;
      font-size: 1.25rem;
      line-height: 1.7;
      min-height: 120px;
      margin-bottom: 16px;
      border: 1px solid #1e293b;
      word-break: break-word;
      user-select: none;
    }
    .c { color: #475569; }
    .c.ok { color: var(--success); text-shadow: 0 0 8px rgba(16,185,129,0.3); }
    .c.err { color: var(--error); background: rgba(239,68,68,0.25); border-radius: 2px; }
    .c.current { border-bottom: 3px solid var(--accent); color: var(--text); }

    textarea {
      width: 100%;
      height: 80px;
      background: #060911;
      border: 1.5px solid #334155;
      border-radius: 8px;
      color: var(--text);
      font-size: 1.15rem;
      padding: 12px;
      outline: none;
      resize: none;
      transition: border-color 0.2s;
    }
    textarea:focus { border-color: var(--accent); box-shadow: 0 0 10px var(--accent-glow); }

    .controls { display: flex; gap: 10px; margin-top: 15px; }
    .btn { flex: 1; padding: 12px; border: none; border-radius: 8px; font-weight: bold; font-size: 0.95rem; cursor: pointer; transition: 0.2s; }
    .btn-primary { background: #0284c7; color: white; }
    .btn-primary:active { background: #0369a1; }
    .btn-skip { background: #1e293b; color: #94a3b8; border: 1px solid #334155; }
  </style>
</head>
<body>

<div class="arena-container">
  <div class="top-bar">
    <h3 style="color: var(--accent);">⚡ CYBERTYPE ROADMAP</h3>
    <div class="level-badge" id="tierDisplay">TIER 1: FOUNDATION</div>
  </div>

  <div class="progress-track">
    <div class="progress-fill" id="progressBar"></div>
  </div>

  <div class="stats-hud">
    <div class="stat-card"><div class="stat-title">Stage</div><div class="stat-value" id="stageDisplay">1 / 16</div></div>
    <div class="stat-card"><div class="stat-title">Speed</div><div class="stat-value"><span id="wpmDisplay">0</span> <small style="font-size:0.7rem">WPM</small></div></div>
    <div class="stat-card"><div class="stat-title">Accuracy</div><div class="stat-value" id="accDisplay">100%</div></div>
    <div class="stat-card"><div class="stat-title">Target</div><div class="stat-value" id="targetWpmDisplay">30+</div></div>
  </div>

  <div class="lesson-meta">
    <span id="lessonObjective">Focus: Speed rhythm & common words</span>
    <span id="queueCount">Remaining: 15</span>
  </div>

  <div class="text-display" id="textDisplay"></div>

  <textarea id="typingInput" placeholder="Start typing with your OTG keyboard to begin..." autofocus></textarea>

  <div class="controls">
    <button class="btn btn-skip" onclick="retryStage()">Retry Stage</button>
    <button class="btn btn-primary" id="actionBtn" onclick="nextStageManual()">Next Stage ➔</button>
  </div>
</div>

<script>
  // Complete 4-Tier Syllabus from Intermediate to Advanced
  const syllabus = [
    // TIER 1: SPEED FOUNDATION (WPM Target: 30-40)
    { tier: "TIER 1: FOUNDATION", targetWPM: 30, desc: "Rhythm & Word Flow", text: "the quick brown fox jumps over the lazy dog every single morning without hesitation" },
    { tier: "TIER 1: FOUNDATION", targetWPM: 32, desc: "Burst Speed & Common Words", text: "they were able to find the right solution before the team ran out of available time" },
    { tier: "TIER 1: FOUNDATION", targetWPM: 35, desc: "Sustained Rhythm", text: "consistency is the ultimate foundation for developing natural touch typing muscle memory" },
    { tier: "TIER 1: FOUNDATION", targetWPM: 38, desc: "Endurance Test", text: "keep your eyes locked on the display screen and let your fingers find every position smoothly" },

    // TIER 2: FINGER INDEPENDENCE (WPM Target: 42-50)
    { tier: "TIER 2: INDEPENDENCE", targetWPM: 42, desc: "Tricky Letter Combinations", text: "extraordinary achievements require extraordinary discipline and rhythmic finger dexterity" },
    { tier: "TIER 2: INDEPENDENCE", targetWPM: 45, desc: "Uncommon Word Lengths", text: "knowledgeable typists navigate complex vocabulary without breaking their continuous momentum" },
    { tier: "TIER 2: INDEPENDENCE", targetWPM: 48, desc: "Pinky & Ring Finger Reach", text: "puzzling quizzes quickly amaze playful players jumping around quiet and cozy plaza zones" },
    { tier: "TIER 2: INDEPENDENCE", targetWPM: 50, desc: "Tier 2 Mastery Gate", text: "precision eliminates wasted physical motion and unlocks reliable speed across the keyboard" },

    // TIER 3: PUNCTUATION & CAPITALIZATION (WPM Target: 52-60)
    { tier: "TIER 3: PRECISION", targetWPM: 52, desc: "Shift Keys & Capitalization", text: "London, Tokyo, and New York host massive global financial networks every single day." },
    { tier: "TIER 3: PRECISION", targetWPM: 55, desc: "Punctuation Harmony", text: "If you want to succeed, remember this rule: work hard, stay humble, and never quit!" },
    { tier: "TIER 3: PRECISION", targetWPM: 58, desc: "Numbers & Dates", text: "In 2026, over 85% of modern developers reported typing at speeds above 65 words per minute." },
    { tier: "TIER 3: PRECISION", targetWPM: 60, desc: "Tier 3 Mastery Gate", text: "Mastery isn't about rushing; it's about eliminating all micro-pauses between sentences!" },

    // TIER 4: ADVANCED SYNTAX & CODE (WPM Target: 65-75+)
    { tier: "TIER 4: ADVANCED SYNTAX", targetWPM: 65, desc: "Brackets & Semicolons", text: "const sanitizeInput = (str) => { return str.trim().toLowerCase().replace(/\\s+/g, '-'); };" },
    { tier: "TIER 4: ADVANCED SYNTAX", targetWPM: 68, desc: "Complex Array Operations", text: "const activeUsers = dataset.filter(user => user.age >= 18 && user.isActive === true);" },
    { tier: "TIER 4: ADVANCED SYNTAX", targetWPM: 72, desc: "Async/Await Logic", text: "async function fetchData(endpoint) { const res = await fetch(endpoint); return await res.json(); }" },
    { tier: "TIER 4: ADVANCED SYNTAX", targetWPM: 75, desc: "Grandmaster Final Test", text: "export default class ArenaEngine { constructor(config) { this.state = Object.freeze({...config}); } }" }
  ];

  let currentStage = parseInt(localStorage.getItem("cybertype_stage")) || 0;
  let startTime = null;
  let timerInterval = null;
  let charIdx = 0;
  let errors = 0;

  const textDisplay = document.getElementById("textDisplay");
  const typingInput = document.getElementById("typingInput");
  const tierDisplay = document.getElementById("tierDisplay");
  const stageDisplay = document.getElementById("stageDisplay");
  const wpmDisplay = document.getElementById("wpmDisplay");
  const accDisplay = document.getElementById("accDisplay");
  const targetWpmDisplay = document.getElementById("targetWpmDisplay");
  const lessonObjective = document.getElementById("lessonObjective");
  const queueCount = document.getElementById("queueCount");
  const progressBar = document.getElementById("progressBar");

  function loadStage() {
    if (currentStage >= syllabus.length) {
      textDisplay.innerHTML = "<span style='color:var(--success); font-size:1.4rem;'>🎉 CONGRATULATIONS! You have completed the entire Advanced Typing Syllabus!</span>";
      typingInput.disabled = true;
      progressBar.style.width = "100%";
      return;
    }

    const currentData = syllabus[currentStage];
    tierDisplay.textContent = currentData.tier;
    stageDisplay.textContent = `${currentStage + 1} / ${syllabus.length}`;
    targetWpmDisplay.textContent = `${currentData.targetWPM}+`;
    lessonObjective.textContent = `Focus: ${currentData.desc}`;
    queueCount.textContent = `Remaining: ${syllabus.length - currentStage - 1}`;
    progressBar.style.width = `${(currentStage / syllabus.length) * 100}%`;

    // Render characters
    textDisplay.innerHTML = "";
    currentData.text.split("").forEach((c, idx) => {
      const span = document.createElement("span");
      span.className = idx === 0 ? "c current" : "c";
      span.textContent = c;
      textDisplay.appendChild(span);
    });

    typingInput.value = "";
    typingInput.disabled = false;
    charIdx = 0;
    errors = 0;
    startTime = null;
    clearInterval(timerInterval);
    wpmDisplay.textContent = "0";
    accDisplay.textContent = "100%";
    typingInput.focus();
  }

  typingInput.addEventListener("input", () => {
    const rawTarget = syllabus[currentStage].text;
    const typed = typingInput.value;
    const spans = textDisplay.querySelectorAll(".c");

    if (!startTime && typed.length > 0) {
      startTime = new Date();
      timerInterval = setInterval(updateLiveStats, 500);
    }

    let correctSoFar = 0;
    errors = 0;

    for (let i = 0; i < rawTarget.length; i++) {
      if (i < typed.length) {
        if (typed[i] === rawTarget[i]) {
          spans[i].className = "c ok";
          correctSoFar++;
        } else {
          spans[i].className = "c err";
          errors++;
        }
      } else if (i === typed.length) {
        spans[i].className = "c current";
      } else {
        spans[i].className = "c";
      }
    }

    updateLiveStats();

    // Completed Sentence Check
    if (typed.length >= rawTarget.length) {
      clearInterval(timerInterval);
      evaluateCompletion();
    }
  });

  function updateLiveStats() {
    if (!startTime) return;
    const elapsedMinutes = (new Date() - startTime) / 60000;
    const typedLength = typingInput.value.length;
    const wpm = Math.round(((typedLength - errors) / 5) / (elapsedMinutes || 0.001));
    const acc = Math.round(((typedLength - errors) / (typedLength || 1)) * 100);

    wpmDisplay.textContent = wpm > 0 ? wpm : 0;
    accDisplay.textContent = `${acc > 0 ? acc : 0}%`;
  }

  function evaluateCompletion() {
    const finalWPM = parseInt(wpmDisplay.textContent);
    const finalAcc = parseInt(accDisplay.textContent);
    const targetWPM = syllabus[currentStage].targetWPM;

    if (finalWPM >= targetWPM && finalAcc >= 90) {
      currentStage++;
      localStorage.setItem("cybertype_stage", currentStage);
      setTimeout(() => {
        alert(`🎯 STAGE CLEARED!\nSpeed: ${finalWPM} WPM\nAccuracy: ${finalAcc}%\nAdvancing to next stage...`);
        loadStage();
      }, 200);
    } else {
      setTimeout(() => {
        alert(`❌ STAGE FAILED\nTarget: ${targetWPM} WPM, 90% Acc\nYour Score: ${finalWPM} WPM, ${finalAcc}%\nTry again to unlock the next level!`);
        retryStage();
      }, 200);
    }
  }

  function retryStage() {
    loadStage();
  }

  function nextStageManual() {
    if (confirm("Skip this stage? (Progress will not count toward mastery)")) {
      currentStage++;
      localStorage.setItem("cybertype_stage", currentStage);
      loadStage();
    }
  }

  loadStage();
</script>
</body>
</html>

