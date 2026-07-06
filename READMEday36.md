# Day36
Explore Your Thinking Patterns
[cognitive-pattern-explorer.html](https://github.com/user-attachments/files/29705601/cognitive-pattern-explorer.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cognitive Pattern Explorer</title>
<style>
  :root{
    --bg1:#0f1420;
    --bg2:#161d2e;
    --card:#1c2436;
    --card-hi:#242e45;
    --accent:#7fd8c9;
    --accent2:#a78bfa;
    --accent3:#f6b26b;
    --text:#e8ecf4;
    --text-dim:#9aa5b8;
    --border:#2c3650;
    --shadow: 0 10px 40px rgba(0,0,0,0.4);
    --radius: 18px;
    --dur: 0.5s;
  }
  [data-mode="stress"]{
    --bg1:#161022;
    --bg2:#1e1730;
    --accent:#ffb997;
    --accent2:#ff8fa3;
  }
  *{box-sizing:border-box;}
  html,body{height:100%;}
  body{
    margin:0;
    font-family: 'Segoe UI', system-ui, -apple-system, Roboto, sans-serif;
    background: radial-gradient(1200px 800px at 20% 10%, var(--bg2), var(--bg1));
    color:var(--text);
    min-height:100vh;
    overflow-x:hidden;
    transition: background var(--dur) ease;
  }
  .app{
    max-width: 900px;
    margin: 0 auto;
    padding: 24px 16px 80px;
    position:relative;
    z-index:1;
  }
  /* Ambient floating shapes */
  .ambient{
    position:fixed; inset:0; overflow:hidden; z-index:0; pointer-events:none;
  }
  .blob{
    position:absolute; border-radius:50%;
    filter: blur(60px);
    opacity:0.25;
    animation: float 18s ease-in-out infinite;
  }
  .blob.b1{ width:340px;height:340px; background:var(--accent); top:-100px; left:-80px; animation-duration:22s;}
  .blob.b2{ width:280px;height:280px; background:var(--accent2); bottom:-80px; right:-60px; animation-duration:26s; animation-delay:2s;}
  .blob.b3{ width:220px;height:220px; background:var(--accent3); top:40%; right:10%; animation-duration:20s; animation-delay:4s;}
  @keyframes float{
    0%,100%{ transform: translate(0,0) scale(1); }
    33%{ transform: translate(30px,-40px) scale(1.08); }
    66%{ transform: translate(-25px,25px) scale(0.95); }
  }
  @media (prefers-reduced-motion: reduce){
    .blob{ animation: none !important; }
    *{ animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
  }

  /* Progress bar */
  .progress-wrap{
    display:flex; align-items:center; gap:10px; margin-bottom:28px;
  }
  .progress-track{
    flex:1; height:8px; border-radius:20px; background:var(--card);
    overflow:hidden; border:1px solid var(--border);
  }
  .progress-fill{
    height:100%; width:0%;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    border-radius:20px;
    transition: width 0.6s cubic-bezier(.4,0,.2,1);
  }
  .progress-label{ font-size:0.8rem; color:var(--text-dim); white-space:nowrap;}

  .screen{
    background: var(--card);
    border:1px solid var(--border);
    border-radius: var(--radius);
    padding: 32px;
    box-shadow: var(--shadow);
    animation: fadeSlide var(--dur) ease;
  }
  @keyframes fadeSlide{
    from{ opacity:0; transform: translateY(14px); }
    to{ opacity:1; transform: translateY(0); }
  }
  h1{ font-size:1.8rem; margin:0 0 8px; letter-spacing:-0.02em; }
  h2{ font-size:1.4rem; margin:0 0 6px; }
  p.sub{ color:var(--text-dim); margin:0 0 24px; line-height:1.5; }
  .badge{
    display:inline-block; padding:4px 12px; border-radius:20px;
    background:var(--card-hi); font-size:0.75rem; color:var(--accent);
    border:1px solid var(--border); margin-bottom:14px; letter-spacing:0.03em;
  }

  /* Buttons */
  button{
    font-family:inherit; cursor:pointer; border:none; outline:none;
  }
  .btn{
    padding:14px 26px; border-radius: 14px; font-size:1rem; font-weight:600;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    color:#0c1119;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    box-shadow: 0 6px 20px rgba(127,216,201,0.25);
  }
  .btn:hover{ transform: translateY(-2px); box-shadow: 0 10px 26px rgba(127,216,201,0.35); }
  .btn:active{ transform: translateY(0); }
  .btn:focus-visible{ outline:3px solid var(--accent2); outline-offset:3px; }
  .btn.secondary{
    background:var(--card-hi); color:var(--text); box-shadow:none;
    border:1px solid var(--border);
  }
  .btn:disabled{ opacity:0.4; cursor:not-allowed; transform:none; }

  .mode-toggle{
    display:flex; gap:10px; margin: 18px 0 30px;
  }
  .mode-btn{
    flex:1; padding:16px; border-radius:14px; background:var(--card-hi);
    border:2px solid var(--border); color:var(--text); text-align:left;
    transition: border-color 0.25s ease, transform 0.2s ease;
  }
  .mode-btn strong{ display:block; font-size:1rem; margin-bottom:4px; }
  .mode-btn span{ font-size:0.8rem; color:var(--text-dim); }
  .mode-btn.active{ border-color: var(--accent); }
  .mode-btn:hover{ transform: translateY(-2px); }
  .mode-btn:focus-visible{ outline:3px solid var(--accent2); outline-offset:2px; }

  .actions{ display:flex; justify-content:space-between; gap:12px; margin-top:28px; flex-wrap:wrap; }

  /* Scenario (Chapter 1) */
  .scenario-scene{
    background:var(--card-hi); border:1px solid var(--border); border-radius:14px;
    padding:20px; margin-bottom:20px; line-height:1.6;
  }
  .options{ display:flex; flex-direction:column; gap:12px; }
  .option-btn{
    text-align:left; padding:16px 18px; border-radius:14px;
    background:var(--card-hi); border:1px solid var(--border); color:var(--text);
    font-size:0.95rem; line-height:1.4;
    transition: border-color 0.2s ease, transform 0.15s ease, background 0.2s ease;
  }
  .option-btn:hover{ border-color:var(--accent); transform: translateX(4px); }
  .option-btn.selected{ border-color:var(--accent); background: rgba(127,216,201,0.12); }
  .option-btn:focus-visible{ outline:3px solid var(--accent2); outline-offset:2px; }

  /* Draggable cards (Chapter 2) */
  .drag-instructions{ font-size:0.85rem; color:var(--text-dim); margin-bottom:16px; }
  .rank-board{ display:flex; gap:20px; flex-wrap:wrap; }
  .pool, .ranked{
    flex:1; min-width:240px;
  }
  .zone-title{ font-size:0.8rem; text-transform:uppercase; letter-spacing:0.05em; color:var(--text-dim); margin-bottom:10px;}
  .drop-zone{
    min-height:260px; background:var(--card-hi); border:2px dashed var(--border);
    border-radius:14px; padding:10px; display:flex; flex-direction:column; gap:10px;
    transition: border-color 0.2s ease, background 0.2s ease;
  }
  .drop-zone.dragover{ border-color:var(--accent); background: rgba(127,216,201,0.08); }
  .card-item{
    background:var(--card); border:1px solid var(--border); border-radius:12px;
    padding:14px 16px; cursor:grab; user-select:none; touch-action:none;
    display:flex; align-items:center; justify-content:space-between; gap:10px;
    transition: transform 0.15s ease, box-shadow 0.15s ease, opacity 0.15s ease;
  }
  .card-item:active{ cursor:grabbing; }
  .card-item.dragging{ opacity:0.5; transform:scale(0.98); }
  .card-item .rank-num{
    display:inline-flex; align-items:center; justify-content:center;
    width:24px; height:24px; border-radius:50%; background:var(--accent); color:#0c1119;
    font-size:0.75rem; font-weight:700; flex-shrink:0;
  }
  .card-item .grip{ color:var(--text-dim); font-size:1rem; }
  .card-item:focus-visible{ outline:3px solid var(--accent2); outline-offset:2px; }
  .move-buttons{ display:flex; gap:4px; }
  .move-buttons button{
    width:26px; height:26px; border-radius:6px; background:var(--card-hi);
    border:1px solid var(--border); color:var(--text); font-size:0.8rem;
  }
  .move-buttons button:hover{ border-color:var(--accent); }

  /* Chapter 3 timeline */
  .timeline-track{
    position:relative; background:var(--card-hi); border:1px solid var(--border);
    border-radius:14px; padding:20px; min-height:120px;
  }
  .timeline-line{
    position:absolute; left:20px; right:20px; top:50%; height:2px; background:var(--border);
    transform: translateY(-50%);
  }
  .timeline-slots{ position:relative; display:flex; justify-content:space-between; z-index:1; }
  .timeline-slot{
    width:90px; height:90px; border-radius:50%; border:2px dashed var(--border);
    display:flex; align-items:center; justify-content:center; text-align:center;
    font-size:0.7rem; color:var(--text-dim); background:var(--bg2);
    transition: border-color 0.2s ease;
    padding:4px;
  }
  .timeline-slot.dragover{ border-color:var(--accent); }
  .timeline-slot .card-item{ width:100%; height:100%; border-radius:50%; padding:6px; font-size:0.7rem; text-align:center; justify-content:center; }
  .timeline-pool{ display:flex; gap:12px; flex-wrap:wrap; margin-top:20px; }
  .timeline-pool .card-item{ min-width:130px; }

  /* Journal */
  .journal-section{ margin-bottom:26px; }
  .bar-row{ display:flex; align-items:center; gap:12px; margin-bottom:14px; }
  .bar-label{ width:170px; font-size:0.85rem; flex-shrink:0; }
  .bar-track{ flex:1; height:14px; background:var(--card-hi); border-radius:20px; overflow:hidden; border:1px solid var(--border); }
  .bar-fill{ height:100%; border-radius:20px; background:linear-gradient(90deg, var(--accent), var(--accent2)); width:0%; transition:width 1s ease; }
  .bar-pct{ width:44px; text-align:right; font-size:0.85rem; color:var(--text-dim); }
  .insight-card{
    background:var(--card-hi); border:1px solid var(--border); border-radius:14px;
    padding:20px; margin-bottom:14px; line-height:1.6;
  }
  .insight-card h3{ margin:0 0 8px; font-size:1.05rem; color:var(--accent); }
  .disclaimer{
    font-size:0.78rem; color:var(--text-dim); border-top:1px solid var(--border);
    margin-top:24px; padding-top:16px; line-height:1.5;
  }

  .sr-only{
    position:absolute; width:1px;height:1px; padding:0; margin:-1px; overflow:hidden;
    clip:rect(0,0,0,0); white-space:nowrap; border:0;
  }

  @media (max-width:600px){
    .screen{ padding:22px 18px; }
    h1{ font-size:1.5rem; }
    .rank-board{ flex-direction:column; }
    .timeline-slots{ flex-wrap:wrap; gap:14px; justify-content:center; }
    .timeline-line{ display:none; }
  }
</style>
</head>
<body data-mode="calm">
<div class="ambient" aria-hidden="true">
  <div class="blob b1"></div>
  <div class="blob b2"></div>
  <div class="blob b3"></div>
</div>

<div class="app">
  <div class="progress-wrap" id="progressWrap" style="display:none;">
    <div class="progress-track"><div class="progress-fill" id="progressFill"></div></div>
    <div class="progress-label" id="progressLabel">Step 1 of 4</div>
  </div>

  <div id="screenHost" role="main" aria-live="polite"></div>
</div>

<script>
(function(){
  "use strict";

  /* ---------------------------------------------
     DATA: Tendencies, scenarios, priorities, timelines
  --------------------------------------------- */
  const TENDENCIES = {
    analytical: {
      name: "Analytical Thinker",
      blurb: "You tend to break things down into logic and evidence before deciding. Structure and clarity help you feel grounded."
    },
    emotional: {
      name: "Emotional Intuitive",
      blurb: "You often lead with gut feeling and empathy. You sense the emotional undertone of a situation quickly."
    },
    overthinking: {
      name: "Overthinking Loop Style",
      blurb: "You often revisit decisions from many angles, which can bring thoroughness but sometimes delays closure."
    },
    action: {
      name: "Action-First Decision Maker",
      blurb: "You tend to move quickly once a direction feels right, preferring momentum over prolonged deliberation."
    },
    balanced: {
      name: "Balanced Reflective Thinker",
      blurb: "You often blend logic, feeling, and action fairly evenly, adapting your approach to the situation at hand."
    }
  };

  // Chapter 1 scenarios - each option maps to one or more tendency weights
  const SCENARIOS = [
    {
      scene: "You receive some unexpected feedback from a colleague about a project you led. It's mixed — some praise, some criticism.",
      options: [
        { text: "You reread it a few times, weighing each point carefully against what you know.", weights: {analytical:2, overthinking:1} },
        { text: "You notice how it makes you feel first, and that shapes how you respond.", weights: {emotional:2} },
        { text: "You reply quickly with a plan to address the criticism.", weights: {action:2} },
        { text: "You sit with it for a while, replaying different interpretations in your head.", weights: {overthinking:2} }
      ]
    },
    {
      scene: "A friend asks for advice about a big life decision — should they take a new job in another city?",
      options: [
        { text: "You ask about pros, cons, and long-term numbers before saying anything.", weights: {analytical:2} },
        { text: "You ask how the opportunity makes them feel deep down.", weights: {emotional:2} },
        { text: "You help them list options, then encourage picking one and moving on.", weights: {action:1, balanced:1} },
        { text: "You explore every possible outcome with them, including worst-case scenarios.", weights: {overthinking:2} }
      ]
    },
    {
      scene: "You're in a meeting and a sudden change of plans is announced with no clear explanation.",
      options: [
        { text: "You start mentally mapping the logical implications of the change.", weights: {analytical:2} },
        { text: "You tune into the room's mood — is everyone else uneasy too?", weights: {emotional:2} },
        { text: "You raise your hand and ask what the very next step should be.", weights: {action:2} },
        { text: "You take a breath, consider a few angles, then ask a clarifying question.", weights: {balanced:2} }
      ]
    },
    {
      scene: "You made a small mistake at work that no one else has noticed yet.",
      options: [
        { text: "You calmly assess the impact and outline a fix.", weights: {analytical:2} },
        { text: "You feel a wave of guilt and think about how it affects others.", weights: {emotional:2} },
        { text: "You fix it immediately without dwelling on it.", weights: {action:2} },
        { text: "You keep replaying the moment, wondering if you should have done it differently.", weights: {overthinking:2} }
      ]
    },
    {
      scene: "You're choosing between two similar apartments to rent.",
      options: [
        { text: "You make a spreadsheet comparing price, size, and commute time.", weights: {analytical:2} },
        { text: "You picture yourself living in each and go with what feels right.", weights: {emotional:2} },
        { text: "You pick the first one that meets your basic needs and sign quickly.", weights: {action:2} },
        { text: "You weigh both a little of everything — data, gut feel, and a quick decision window.", weights: {balanced:2} }
      ]
    },
    {
      scene: "A plan you were excited about suddenly falls through.",
      options: [
        { text: "You immediately start listing alternative plans.", weights: {action:2, analytical:1} },
        { text: "You feel the disappointment fully before thinking about next steps.", weights: {emotional:2} },
        { text: "You keep wondering what you could have done differently to prevent it.", weights: {overthinking:2} },
        { text: "You accept it, feel it briefly, then pivot to a new option.", weights: {balanced:2} }
      ]
    }
  ];

  // Chapter 2 - priority cards to rank (drag and drop)
  const PRIORITY_CARDS = [
    { id:"logic", label:"Clear logic and evidence", weights:{analytical:2} },
    { id:"feel", label:"How it feels emotionally", weights:{emotional:2} },
    { id:"speed", label:"Speed of resolution", weights:{action:2} },
    { id:"thorough", label:"Thoroughness, even if slow", weights:{overthinking:2} },
    { id:"harmony", label:"Group harmony & consensus", weights:{emotional:1, balanced:1} },
    { id:"options", label:"Keeping options open", weights:{overthinking:1, balanced:1} }
  ];

  // Chapter 3 - timeline: map stages of a decision to a style
  const TIMELINE_STAGES = ["First reaction", "Midpoint", "Final decision"];
  const TIMELINE_CARDS = [
    { id:"gut", label:"Trust gut feeling", weights:{emotional:2} },
    { id:"data", label:"Gather more data", weights:{analytical:2} },
    { id:"pause", label:"Pause & reconsider repeatedly", weights:{overthinking:2} },
    { id:"commit", label:"Commit and act", weights:{action:2} },
    { id:"balance", label:"Balance feeling & facts", weights:{balanced:2} }
  ];

  /* ---------------------------------------------
     STATE
  --------------------------------------------- */
  const state = {
    mode: "calm",
    step: 0, // 0=start,1=ch1,2=ch2,3=ch3,4=journal
    ch1Answers: [], // index of chosen option per scenario
    ch1Index: 0,
    ch2Rank: [], // ordered array of card ids (top = highest priority)
    ch3Map: {}, // stageIndex -> card id
    scores: { analytical:0, emotional:0, overthinking:0, action:0, balanced:0 }
  };

  const STEP_LABELS = ["Start","Discover Your Thinking Style","Choose Your Priorities","Map Your Thinking","Reflection Journal"];

  const host = document.getElementById("screenHost");
  const progressWrap = document.getElementById("progressWrap");
  const progressFill = document.getElementById("progressFill");
  const progressLabel = document.getElementById("progressLabel");

  function updateProgress(){
    if(state.step === 0){ progressWrap.style.display = "none"; return; }
    progressWrap.style.display = "flex";
    const pct = Math.round((state.step / 4) * 100);
    progressFill.style.width = pct + "%";
    progressLabel.textContent = "Step " + state.step + " of 4 · " + STEP_LABELS[state.step];
  }

  function addWeights(weights){
    Object.keys(weights).forEach(k => {
      state.scores[k] = (state.scores[k] || 0) + weights[k];
    });
  }

  /* ---------------------------------------------
     RENDER: START SCREEN
  --------------------------------------------- */
  function renderStart(){
    state.step = 0;
    updateProgress();
    host.innerHTML = `
      <div class="screen">
        <span class="badge">Self-reflection · Educational</span>
        <h1>Cognitive Pattern Explorer</h1>
        <p class="sub">A calm, exploratory journey through how you tend to think and decide. This isn't a test — there are no right answers, just gentle noticing. Purely educational, not a clinical assessment.</p>

        <h2 style="font-size:1rem; margin-bottom:10px;">Choose your atmosphere</h2>
        <div class="mode-toggle" role="group" aria-label="Choose atmosphere mode">
          <button class="mode-btn ${state.mode==='calm'?'active':''}" data-mode-btn="calm" aria-pressed="${state.mode==='calm'}">
            <strong>🌊 Calm Mode</strong>
            <span>Soft teal tones, gentle pacing</span>
          </button>
          <button class="mode-btn ${state.mode==='stress'?'active':''}" data-mode-btn="stress" aria-pressed="${state.mode==='stress'}">
            <strong>🔥 Stress Mode</strong>
            <span>Warmer tones, quicker pacing</span>
          </button>
        </div>

        <div class="actions">
          <span></span>
          <button class="btn" id="beginBtn">Begin the exploration →</button>
        </div>
      </div>
    `;
    document.body.setAttribute("data-mode", state.mode);
    host.querySelectorAll("[data-mode-btn]").forEach(btn=>{
      btn.addEventListener("click", ()=>{
        state.mode = btn.getAttribute("data-mode-btn");
        renderStart();
      });
    });
    document.getElementById("beginBtn").addEventListener("click", ()=>{
      state.step = 1;
      state.ch1Index = 0;
      renderChapter1();
    });
  }

  /* ---------------------------------------------
     RENDER: CHAPTER 1 - scenario quiz
  --------------------------------------------- */
  function renderChapter1(){
    updateProgress();
    const idx = state.ch1Index;
    const scenario = SCENARIOS[idx];
    const chosen = state.ch1Answers[idx];

    host.innerHTML = `
      <div class="screen">
        <span class="badge">Chapter 1 · Discover Your Thinking Style</span>
        <h2>Scenario ${idx+1} of ${SCENARIOS.length}</h2>
        <div class="scenario-scene">${scenario.scene}</div>
        <div class="options" role="group" aria-label="Choose how you'd respond">
          ${scenario.options.map((opt,i)=>`
            <button class="option-btn ${chosen===i?'selected':''}" data-opt="${i}" aria-pressed="${chosen===i}">
              ${opt.text}
            </button>
          `).join("")}
        </div>
        <div class="actions">
          <button class="btn secondary" id="backBtn" ${idx===0?'disabled':''}>← Back</button>
          <button class="btn" id="nextBtn" ${chosen===undefined?'disabled':''}>${idx===SCENARIOS.length-1?'Continue to Chapter 2 →':'Next →'}</button>
        </div>
      </div>
    `;

    host.querySelectorAll("[data-opt]").forEach(btn=>{
      btn.addEventListener("click", ()=>{
        const optIndex = parseInt(btn.getAttribute("data-opt"),10);
        state.ch1Answers[idx] = optIndex;
        renderChapter1();
      });
    });

    document.getElementById("nextBtn").addEventListener("click", ()=>{
      if(idx < SCENARIOS.length - 1){
        state.ch1Index++;
        renderChapter1();
      } else {
        // tally ch1 scores fresh (in case of re-answering)
        SCENARIOS.forEach((s, i)=>{
          const chosenIdx = state.ch1Answers[i];
          if(chosenIdx !== undefined) addWeights(s.options[chosenIdx].weights);
        });
        state.step = 2;
        state.ch2Rank = PRIORITY_CARDS.map(c=>c.id).slice(); // default order = pool, none ranked yet
        state.ch2Pool = PRIORITY_CARDS.map(c=>c.id).slice();
        state.ch2Ranked = [];
        renderChapter2();
      }
    });

    if(idx > 0){
      document.getElementById("backBtn").addEventListener("click", ()=>{
        state.ch1Index--;
        renderChapter1();
      });
    }
  }

  /* ---------------------------------------------
     RENDER: CHAPTER 2 - drag priority ranking
  --------------------------------------------- */
  function cardMarkup(id, list){
    const card = PRIORITY_CARDS.find(c=>c.id===id) || TIMELINE_CARDS.find(c=>c.id===id);
    const rankNum = list ? list.indexOf(id)+1 : null;
    return `
      <div class="card-item" draggable="true" tabindex="0" data-card-id="${id}" role="listitem" aria-grabbed="false">
        ${rankNum ? `<span class="rank-num">${rankNum}</span>` : `<span class="grip">⠿</span>`}
        <span style="flex:1;">${card.label}</span>
        <div class="move-buttons">
          <button type="button" data-move="up" aria-label="Move up">↑</button>
          <button type="button" data-move="down" aria-label="Move down">↓</button>
        </div>
      </div>
    `;
  }

  function renderChapter2(){
    updateProgress();
    host.innerHTML = `
      <div class="screen">
        <span class="badge">Chapter 2 · Choose Your Priorities</span>
        <h2>Drag cards into your priority order</h2>
        <p class="drag-instructions">Drag from the left pool into the ranked list on the right — top means "matters most to you" when deciding. You can also use the ↑/↓ buttons, or press Enter on a focused card to move it, for full keyboard access.</p>
        <div class="rank-board">
          <div class="pool">
            <div class="zone-title">Available</div>
            <div class="drop-zone" id="poolZone" data-zone="pool" role="list" aria-label="Available priority cards">
              ${state.ch2Pool.map(id=>cardMarkup(id,null)).join("")}
            </div>
          </div>
          <div class="ranked">
            <div class="zone-title">Your Ranking (top = most important)</div>
            <div class="drop-zone" id="rankedZone" data-zone="ranked" role="list" aria-label="Your ranked priorities">
              ${state.ch2Ranked.map(id=>cardMarkup(id,state.ch2Ranked)).join("")}
            </div>
          </div>
        </div>
        <div class="actions">
          <button class="btn secondary" id="backBtn">← Back</button>
          <button class="btn" id="nextBtn" ${state.ch2Ranked.length===0?'disabled':''}>Continue to Chapter 3 →</button>
        </div>
      </div>
    `;
    setupDragDrop("poolZone","rankedZone", state, "ch2Pool","ch2Ranked", renderChapter2);

    document.getElementById("backBtn").addEventListener("click", ()=>{
      state.step = 1;
      renderChapter1();
    });
    document.getElementById("nextBtn").addEventListener("click", ()=>{
      // tally ch2 weights based on rank position (higher rank = more weight)
      const n = state.ch2Ranked.length;
      state.ch2Ranked.forEach((id, i)=>{
        const card = PRIORITY_CARDS.find(c=>c.id===id);
        if(!card) return;
        const positionWeight = (n - i) / n; // top gets ~1, bottom gets small
        const scaled = {};
        Object.keys(card.weights).forEach(k=> scaled[k] = card.weights[k]*positionWeight);
        addWeights(scaled);
      });
      state.step = 3;
      state.ch3Pool = TIMELINE_CARDS.map(c=>c.id).slice();
      state.ch3Map = {};
      renderChapter3();
    });
  }

  /* ---------------------------------------------
     RENDER: CHAPTER 3 - timeline mapping
  --------------------------------------------- */
  function renderChapter3(){
    updateProgress();
    const filledCount = Object.keys(state.ch3Map).length;
    host.innerHTML = `
      <div class="screen">
        <span class="badge">Chapter 3 · Map Your Thinking</span>
        <h2>Place a thinking approach at each stage of a decision</h2>
        <p class="drag-instructions">Drag a card from below onto the stage of a decision where it feels most true for you. Keyboard users: focus a card and press Enter to cycle it into the next open stage.</p>
        <div class="timeline-track">
          <div class="timeline-line" aria-hidden="true"></div>
          <div class="timeline-slots">
            ${TIMELINE_STAGES.map((stage,i)=>`
              <div class="timeline-slot" data-slot="${i}" role="listitem" aria-label="${stage} stage">
                ${state.ch3Map[i] ? cardMarkup(state.ch3Map[i], null) : stage}
              </div>
            `).join("")}
          </div>
        </div>
        <div class="timeline-pool" id="timelinePool" data-zone="tlpool" role="list" aria-label="Available thinking approach cards">
          ${state.ch3Pool.map(id=>cardMarkup(id,null)).join("")}
        </div>
        <div class="actions">
          <button class="btn secondary" id="backBtn">← Back</button>
          <button class="btn" id="nextBtn" ${filledCount<TIMELINE_STAGES.length?'disabled':''}>See My Reflection Journal →</button>
        </div>
      </div>
    `;
    setupTimelineDragDrop();

    document.getElementById("backBtn").addEventListener("click", ()=>{
      state.step = 2;
      renderChapter2();
    });
    document.getElementById("nextBtn").addEventListener("click", ()=>{
      Object.values(state.ch3Map).forEach(id=>{
        const card = TIMELINE_CARDS.find(c=>c.id===id);
        if(card) addWeights(card.weights);
      });
      state.step = 4;
      renderJournal();
    });
  }

  function setupTimelineDragDrop(){
    const poolEl = document.getElementById("timelinePool");
    let draggedId = null;

    function attachCardEvents(container){
      container.querySelectorAll(".card-item").forEach(card=>{
        card.addEventListener("dragstart", e=>{
          draggedId = card.getAttribute("data-card-id");
          card.classList.add("dragging");
          e.dataTransfer.setData("text/plain", draggedId);
          e.dataTransfer.effectAllowed = "move";
        });
        card.addEventListener("dragend", ()=> card.classList.remove("dragging"));

        // touch fallback
        let touchStartY=0, touchStartX=0;
        card.addEventListener("touchstart", e=>{
          draggedId = card.getAttribute("data-card-id");
          const t = e.touches[0];
          touchStartX=t.clientX; touchStartY=t.clientY;
        }, {passive:true});
        card.addEventListener("touchend", e=>{
          const t = e.changedTouches[0];
          const el = document.elementFromPoint(t.clientX, t.clientY);
          const slot = el && el.closest(".timeline-slot");
          if(slot){
            placeInSlot(draggedId, parseInt(slot.getAttribute("data-slot"),10));
          }
        });

        // keyboard: Enter places into next open slot
        card.addEventListener("keydown", e=>{
          if(e.key === "Enter" || e.key === " "){
            e.preventDefault();
            const id = card.getAttribute("data-card-id");
            const openSlot = TIMELINE_STAGES.findIndex((_,i)=> !state.ch3Map[i]);
            if(openSlot !== -1) placeInSlot(id, openSlot);
          }
        });
      });
    }

    function placeInSlot(id, slotIndex){
      if(!id) return;
      // remove id from pool
      state.ch3Pool = state.ch3Pool.filter(c=>c!==id);
      // if slot already occupied, return that card to pool
      if(state.ch3Map[slotIndex]){
        state.ch3Pool.push(state.ch3Map[slotIndex]);
      }
      state.ch3Map[slotIndex] = id;
      renderChapter3();
    }

    attachCardEvents(poolEl);

    document.querySelectorAll(".timeline-slot").forEach(slot=>{
      slot.addEventListener("dragover", e=>{
        e.preventDefault();
        slot.classList.add("dragover");
      });
      slot.addEventListener("dragleave", ()=> slot.classList.remove("dragover"));
      slot.addEventListener("drop", e=>{
        e.preventDefault();
        slot.classList.remove("dragover");
        const id = e.dataTransfer.getData("text/plain") || draggedId;
        placeInSlot(id, parseInt(slot.getAttribute("data-slot"),10));
      });
      // if slot has a card already, attach events so it can be dragged back to pool
      const cardInSlot = slot.querySelector(".card-item");
      if(cardInSlot) attachCardEvents(slot);
    });

    // allow dropping back into pool
    poolEl.addEventListener("dragover", e=>{ e.preventDefault(); poolEl.classList.add("dragover"); });
    poolEl.addEventListener("dragleave", ()=> poolEl.classList.remove("dragover"));
    poolEl.addEventListener("drop", e=>{
      e.preventDefault();
      poolEl.classList.remove("dragover");
      const id = e.dataTransfer.getData("text/plain") || draggedId;
      if(!id) return;
      // remove from map if it was placed
      Object.keys(state.ch3Map).forEach(k=>{
        if(state.ch3Map[k] === id) delete state.ch3Map[k];
      });
      if(!state.ch3Pool.includes(id)) state.ch3Pool.push(id);
      renderChapter3();
    });
  }

  /* ---------------------------------------------
     Reusable drag-and-drop for two-zone lists (Chapter 2)
  --------------------------------------------- */
  function setupDragDrop(poolId, rankedId, stateObj, poolKey, rankedKey, rerender){
    const poolEl = document.getElementById(poolId);
    const rankedEl = document.getElementById(rankedId);
    let draggedId = null;
    let draggedFrom = null;

    function moveCard(id, fromKey, toKey, toIndex){
      stateObj[fromKey] = stateObj[fromKey].filter(c=>c!==id);
      if(toIndex === undefined || toIndex < 0 || toIndex > stateObj[toKey].length){
        stateObj[toKey].push(id);
      } else {
        stateObj[toKey].splice(toIndex, 0, id);
      }
    }

    function attach(container, key){
      container.querySelectorAll(".card-item").forEach((card,cardIdx)=>{
        card.addEventListener("dragstart", e=>{
          draggedId = card.getAttribute("data-card-id");
          draggedFrom = key;
          card.classList.add("dragging");
          e.dataTransfer.setData("text/plain", draggedId);
          e.dataTransfer.effectAllowed = "move";
        });
        card.addEventListener("dragend", ()=> card.classList.remove("dragging"));

        // touch fallback
        card.addEventListener("touchstart", ()=>{
          draggedId = card.getAttribute("data-card-id");
          draggedFrom = key;
        }, {passive:true});
        card.addEventListener("touchend", e=>{
          const t = e.changedTouches[0];
          const el = document.elementFromPoint(t.clientX, t.clientY);
          const zone = el && el.closest(".drop-zone");
          if(zone){
            const toKey = zone.id === poolId ? poolKey : rankedKey;
            moveCard(draggedId, draggedFrom, toKey);
            rerender();
          }
        });

        // keyboard controls: move up/down within ranked, Enter moves pool->ranked
        const upBtn = card.querySelector('[data-move="up"]');
        const downBtn = card.querySelector('[data-move="down"]');
        upBtn.addEventListener("click", (e)=>{
          e.stopPropagation();
          const arr = stateObj[key];
          const i = arr.indexOf(card.getAttribute("data-card-id"));
          if(key === poolKey){
            // move pool item into ranked at top
            moveCard(arr[i], poolKey, rankedKey, 0);
          } else if(i>0){
            const tmp = arr[i-1]; arr[i-1]=arr[i]; arr[i]=tmp;
          }
          rerender();
        });
        downBtn.addEventListener("click", (e)=>{
          e.stopPropagation();
          const arr = stateObj[key];
          const i = arr.indexOf(card.getAttribute("data-card-id"));
          if(key === poolKey){
            moveCard(arr[i], poolKey, rankedKey);
          } else if(i < arr.length-1){
            const tmp = arr[i+1]; arr[i+1]=arr[i]; arr[i]=tmp;
          }
          rerender();
        });

        card.addEventListener("keydown", e=>{
          if(e.key === "Enter"){
            e.preventDefault();
            if(key === poolKey){
              moveCard(card.getAttribute("data-card-id"), poolKey, rankedKey, 0);
              rerender();
            }
          }
        });
      });
    }

    attach(poolEl, poolKey);
    attach(rankedEl, rankedKey);

    [ {el:poolEl, key:poolKey}, {el:rankedEl, key:rankedKey} ].forEach(({el,key})=>{
      el.addEventListener("dragover", e=>{ e.preventDefault(); el.classList.add("dragover"); });
      el.addEventListener("dragleave", ()=> el.classList.remove("dragover"));
      el.addEventListener("drop", e=>{
        e.preventDefault();
        el.classList.remove("dragover");
        const id = e.dataTransfer.getData("text/plain") || draggedId;
        if(!id) return;
        const fromKey = draggedFrom || (stateObj[poolKey].includes(id) ? poolKey : rankedKey);
        moveCard(id, fromKey, key);
        rerender();
      });
    });
  }

  /* ---------------------------------------------
     RENDER: FINAL JOURNAL
  --------------------------------------------- */
  function computePercentages(){
    const total = Object.values(state.scores).reduce((a,b)=>a+b,0) || 1;
    const pct = {};
    Object.keys(state.scores).forEach(k=>{
      pct[k] = Math.round((state.scores[k]/total)*100);
    });
    return pct;
  }

  function renderJournal(){
    updateProgress();
    const pct = computePercentages();
    const sorted = Object.entries(pct).sort((a,b)=>b[1]-a[1]);
    const top = sorted[0][0];
    const second = sorted[1][0];

    host.innerHTML = `
      <div class="screen">
        <span class="badge">Final Reflection Journal</span>
        <h1>Your Thinking Pattern Snapshot</h1>
        <p class="sub">This is a reflective mirror, not a verdict. Thinking styles shift with mood, context, and practice — treat this as one gentle snapshot in time.</p>

        <div class="journal-section">
          <h2 style="font-size:1.1rem;">Breakdown</h2>
          ${sorted.map(([key,val])=>`
            <div class="bar-row">
              <div class="bar-label">${TENDENCIES[key].name}</div>
              <div class="bar-track"><div class="bar-fill" style="width:${val}%"></div></div>
              <div class="bar-pct">${val}%</div>
            </div>
          `).join("")}
        </div>

        <div class="journal-section">
          <div class="insight-card">
            <h3>Your leading pattern: ${TENDENCIES[top].name}</h3>
            <p>${TENDENCIES[top].blurb}</p>
          </div>
          <div class="insight-card">
            <h3>A supporting thread: ${TENDENCIES[second].name}</h3>
            <p>${TENDENCIES[second].blurb} This suggests your thinking often draws on more than one style depending on the moment.</p>
          </div>
        </div>

        <div class="disclaimer">
          This experience is designed for personal reflection and educational curiosity only. It does not diagnose, assess, or represent any clinical or psychological evaluation. If you have concerns about your mental health or wellbeing, please consider speaking with a licensed professional.
        </div>

        <div class="actions">
          <button class="btn secondary" id="restartBtn">↺ Start Over</button>
          <span></span>
        </div>
      </div>
    `;

    // animate bars in after paint
    requestAnimationFrame(()=>{
      requestAnimationFrame(()=>{
        host.querySelectorAll(".bar-fill").forEach((el,i)=>{
          el.style.width = sorted[i][1] + "%";
        });
      });
    });

    document.getElementById("restartBtn").addEventListener("click", ()=>{
      state.step = 0;
      state.ch1Answers = [];
      state.ch1Index = 0;
      state.ch2Pool = [];
      state.ch2Ranked = [];
      state.ch3Pool = [];
      state.ch3Map = {};
      state.scores = { analytical:0, emotional:0, overthinking:0, action:0, balanced:0 };
      renderStart();
    });
  }

  /* ---------------------------------------------
     INIT
  --------------------------------------------- */
  renderStart();
})();
</script>
</body>
</html>
