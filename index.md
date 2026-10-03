<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="theme-color" content="#f4f3ed">
  <meta name="description" content="Practice EMT patient assessment with interactive medical and trauma scenarios.">
  <title>Fieldwork | EMT Scenario Trainer</title>
  <style>
    :root {
      color-scheme: light;
      --paper: #f4f3ed;
      --white: #fffefa;
      --ink: #202722;
      --muted: #747b73;
      --line: #e3e3d9;
      --green: #245b49;
      --green-soft: #e7f0e8;
      --red: #c84f3d;
      --red-soft: #f8eae5;
      --yellow: #e6c44e;
      --shadow: 0 12px 36px rgb(35 43 36 / 6%);
      font-family: "Avenir Next", Avenir, "Segoe UI", sans-serif;
    }

    * { box-sizing: border-box; }

    body {
      min-width: 320px;
      min-height: 100vh;
      margin: 0;
      color: var(--ink);
      background-color: var(--paper);
      background-image: radial-gradient(#d9d9ce 0.7px, transparent 0.7px);
      background-size: 17px 17px;
    }

    button, input { font: inherit; }
    button { color: inherit; }

    .topbar {
      height: 68px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 5vw;
      border-bottom: 1px solid var(--line);
      background: rgb(255 254 250 / 94%);
    }

    .brand { display: flex; align-items: center; gap: 11px; font-weight: 760; letter-spacing: 0; }
    .brand-mark {
      width: 32px;
      height: 32px;
      display: grid;
      place-items: center;
      border-radius: 9px;
      color: white;
      background: var(--green);
      font-size: 19px;
      line-height: 1;
    }
    .brand-name { font-size: 17px; }
    .brand-name span { color: var(--muted); font-weight: 500; }
    .top-meta { display: flex; align-items: center; gap: 20px; color: var(--muted); font-size: 12px; }
    .practice-tag { display: flex; align-items: center; gap: 7px; color: var(--green); font-weight: 700; }
    .practice-dot { width: 7px; height: 7px; border-radius: 50%; background: #59a477; }

    .page { width: min(1380px, 92vw); margin: 0 auto; padding: 34px 0 30px; }
    .heading-row { display: flex; align-items: end; justify-content: space-between; gap: 16px; margin-bottom: 24px; }
    .eyebrow { margin: 0 0 7px; color: var(--red); font-size: 10px; font-weight: 800; letter-spacing: 1.3px; text-transform: uppercase; }
    h1 { margin: 0; font-family: Georgia, "Times New Roman", serif; font-size: clamp(24px, 3vw, 34px); font-weight: 500; line-height: 1.1; letter-spacing: 0; }
    .heading-note { max-width: 330px; margin: 0; color: var(--muted); font-size: 13px; line-height: 1.55; }

    .workspace { display: grid; grid-template-columns: 218px minmax(360px, 1fr) 246px; gap: 15px; align-items: stretch; min-height: 650px; }
    .panel { border: 1px solid var(--line); border-radius: 8px; background: var(--white); box-shadow: var(--shadow); }
    .scenario-panel, .details-panel { padding: 19px 16px; }
    .panel-label { margin: 0 0 14px; color: var(--muted); font-size: 10px; font-weight: 800; letter-spacing: 1px; text-transform: uppercase; }
    .scenario-group + .scenario-group { margin-top: 21px; }
    .group-title { margin: 0 0 8px; color: #9a9c93; font-size: 10px; font-weight: 750; letter-spacing: .7px; text-transform: uppercase; }
    .scenario-button {
      width: 100%;
      display: flex;
      align-items: flex-start;
      gap: 10px;
      padding: 11px 9px;
      border: 1px solid transparent;
      border-radius: 6px;
      text-align: left;
      background: transparent;
      cursor: pointer;
      transition: background .16s ease, border-color .16s ease;
    }
    .scenario-button:hover { background: #f7f7f1; }
    .scenario-button[aria-pressed="true"] { border-color: #c9d9ca; background: var(--green-soft); }
    .scenario-glyph { width: 26px; height: 26px; flex: 0 0 26px; display: grid; place-items: center; border-radius: 6px; color: var(--green); background: white; font-size: 12px; font-weight: 800; }
    .scenario-button[data-type="trauma"] .scenario-glyph { color: var(--red); }
    .scenario-name { display: block; margin: 1px 0 4px; font-size: 12px; font-weight: 720; }
    .scenario-description { display: block; color: var(--muted); font-size: 10px; line-height: 1.35; }
    .scenario-count { margin-top: 25px; padding-top: 14px; border-top: 1px solid var(--line); color: var(--muted); font-size: 11px; }

    .chat-panel { min-width: 0; display: flex; flex-direction: column; overflow: hidden; }
    .chat-header { display: flex; align-items: center; justify-content: space-between; gap: 10px; padding: 16px 18px; border-bottom: 1px solid var(--line); }
    .case-heading { min-width: 0; }
    .case-heading h2 { overflow: hidden; margin: 0 0 4px; font-size: 14px; text-overflow: ellipsis; white-space: nowrap; }
    .case-subtitle { color: var(--muted); font-size: 10px; }
    .status-pill { flex: 0 0 auto; padding: 6px 8px; border-radius: 4px; color: #84691c; background: #f8f1d9; font-size: 9px; font-weight: 800; letter-spacing: .45px; text-transform: uppercase; }
    .status-pill.ready { color: var(--green); background: var(--green-soft); }
    .conversation { min-height: 360px; flex: 1; overflow-y: auto; padding: 20px 18px 12px; scroll-behavior: smooth; }
    .time-stamp { margin: 0 0 17px; color: #9a9c93; font-size: 10px; text-align: center; }
    .message { display: flex; align-items: flex-start; gap: 9px; margin: 0 0 15px; animation: arrive .2s ease-out both; }
    .message.user { justify-content: flex-end; }
    .avatar { width: 25px; height: 25px; flex: 0 0 25px; display: grid; place-items: center; border-radius: 50%; color: var(--green); background: var(--green-soft); font-size: 10px; font-weight: 800; }
    .bubble { max-width: min(82%, 470px); padding: 11px 13px; border: 1px solid var(--line); border-radius: 2px 9px 9px 9px; background: white; font-size: 12px; line-height: 1.55; white-space: pre-line; }
    .message.user .bubble { border-color: var(--green); border-radius: 9px 2px 9px 9px; color: white; background: var(--green); }
    .message-time { display: block; margin-top: 5px; color: #a0a39b; font-size: 9px; }
    .message.user .message-time { color: #d5e3da; }
    .quick-prompts { display: flex; flex-wrap: wrap; gap: 6px; padding: 0 18px 12px; }
    .quick-prompt { padding: 7px 9px; border: 1px solid #dce5db; border-radius: 4px; color: var(--green); background: #f7faf5; font-size: 10px; font-weight: 650; cursor: pointer; transition: background .15s ease, transform .15s ease; }
    .quick-prompt:hover { transform: translateY(-1px); background: var(--green-soft); }
    .composer { display: flex; align-items: end; gap: 8px; padding: 12px 14px; border-top: 1px solid var(--line); background: #fcfcf8; }
    .composer input { min-width: 0; height: 40px; flex: 1; padding: 0 12px; border: 1px solid var(--line); border-radius: 5px; outline: none; color: var(--ink); background: white; font-size: 12px; }
    .composer input:focus { border-color: #8aa892; box-shadow: 0 0 0 3px rgb(36 91 73 / 9%); }
    .composer input::placeholder { color: #a2a59c; }
    .send-button { height: 40px; padding: 0 15px; border: 0; border-radius: 5px; color: white; background: var(--red); font-size: 11px; font-weight: 750; cursor: pointer; transition: background .15s ease; }
    .send-button:hover { background: #ad4032; }

    .patient-identity { padding: 3px 0 16px; border-bottom: 1px solid var(--line); }
    .patient-avatar { width: 46px; height: 46px; display: grid; place-items: center; margin-bottom: 12px; border-radius: 50%; color: var(--green); background: #e9eee5; font-size: 17px; font-weight: 760; }
    .patient-name { margin: 0 0 5px; font-size: 15px; }
    .patient-meta { margin: 0; color: var(--muted); font-size: 11px; }
    .vitals-title { display: flex; align-items: center; justify-content: space-between; margin: 16px 0 8px; }
    .vitals-title h3 { margin: 0; font-size: 11px; }
    .lock-label { color: #92968e; font-size: 9px; }
    .vital-row { display: flex; align-items: center; justify-content: space-between; padding: 9px 0; border-bottom: 1px solid #efefe9; font-size: 10px; }
    .vital-row span:first-child { color: var(--muted); }
    .vital-value { color: #a0a39b; font-size: 10px; font-weight: 700; }
    .vital-value.revealed { color: var(--ink); }
    .finding { margin-top: 17px; padding: 12px; border-left: 2px solid var(--yellow); background: #fbf8eb; }
    .finding strong { display: block; margin-bottom: 5px; font-size: 10px; }
    .finding p { margin: 0; color: #737365; font-size: 10px; line-height: 1.5; }
    .reset-button { width: 100%; margin-top: 17px; padding: 9px; border: 1px solid var(--line); border-radius: 5px; color: var(--muted); background: transparent; font-size: 10px; font-weight: 650; cursor: pointer; }
    .reset-button:hover { border-color: #b8c8b8; color: var(--green); }
    .disclaimer { margin: 15px 2px 0; color: #85877f; font-size: 10px; line-height: 1.5; }

    @keyframes arrive { from { opacity: 0; transform: translateY(4px); } to { opacity: 1; transform: translateY(0); } }
    @media (prefers-reduced-motion: reduce) { *, *::before, *::after { scroll-behavior: auto !important; animation-duration: .01ms !important; transition-duration: .01ms !important; } }
    @media (max-width: 1000px) {
      .workspace { grid-template-columns: 190px minmax(330px, 1fr); }
      .details-panel { grid-column: 1 / -1; display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 18px; }
      .patient-identity { border: 0; }
      .vitals-block { grid-column: 2; }
      .finding-block { grid-column: 3; }
      .vitals-title { margin-top: 3px; }
      .finding { margin-top: 0; }
      .reset-button { margin-top: 10px; }
    }
    @media (max-width: 680px) {
      .topbar { height: 58px; padding: 0 4vw; }
      .top-meta > span:first-child { display: none; }
      .page { width: 92vw; padding-top: 25px; }
      .heading-row { display: block; margin-bottom: 17px; }
      .heading-note { margin-top: 9px; }
      .workspace { grid-template-columns: 1fr; gap: 11px; }
      .scenario-panel { padding: 13px; }
      .scenario-panel > .panel-label { margin-bottom: 9px; }
      .scenario-list { display: flex; gap: 7px; overflow-x: auto; padding-bottom: 2px; }
      .scenario-group { display: contents; }
      .group-title, .scenario-count { display: none; }
      .scenario-button { width: auto; min-width: 150px; flex: 0 0 auto; padding: 9px 7px; }
      .chat-panel { min-height: 560px; }
      .conversation { min-height: 300px; }
      .details-panel { grid-column: auto; display: block; padding: 15px; }
      .patient-identity { display: flex; align-items: center; gap: 12px; padding: 0 0 12px; border-bottom: 1px solid var(--line); }
      .patient-avatar { width: 38px; height: 38px; flex: 0 0 38px; margin: 0; font-size: 14px; }
      .vitals-title { margin-top: 12px; }
      .finding { margin-top: 12px; }
      .reset-button { margin-top: 12px; }
    }
  </style>
</head>
<body>
  <header class="topbar">
    <div class="brand"><span class="brand-mark" aria-hidden="true">+</span><span class="brand-name">fieldwork <span>/ EMT practice</span></span></div>
    <div class="top-meta"><span>Independent practice</span><span class="practice-tag"><span class="practice-dot"></span>Training mode</span></div>
  </header>

  <main class="page">
    <div class="heading-row">
      <div><p class="eyebrow">Patient assessment lab</p><h1>Run the call.</h1></div>
      <p class="heading-note">Talk through the assessment. The patient only gives you what you’ve earned.</p>
    </div>

    <section class="workspace" aria-label="EMT scenario practice">
      <aside class="panel scenario-panel" aria-label="Choose a scenario">
        <p class="panel-label">Scenario board</p>
        <div class="scenario-list">
          <div class="scenario-group">
            <p class="group-title">Medical</p>
            <button class="scenario-button" type="button" data-scenario="glucose" data-type="medical" aria-pressed="true">
              <span class="scenario-glyph" aria-hidden="true">M</span><span><span class="scenario-name">Sudden confusion</span><span class="scenario-description">Medical · altered mental status</span></span>
            </button>
          </div>
          <div class="scenario-group">
            <p class="group-title">Trauma</p>
            <button class="scenario-button" type="button" data-scenario="fall" data-type="trauma" aria-pressed="false">
              <span class="scenario-glyph" aria-hidden="true">T</span><span><span class="scenario-name">Fall from a ladder</span><span class="scenario-description">Trauma · lower leg injury</span></span>
            </button>
          </div>
        </div>
        <div class="scenario-count">2 practice calls · more on the way</div>
      </aside>

      <section class="panel chat-panel" aria-label="Scenario conversation">
        <header class="chat-header">
          <div class="case-heading"><h2 id="case-title">Sudden confusion at home</h2><span class="case-subtitle" id="case-subtitle">Medical call · You are the lead EMT</span></div>
          <span class="status-pill" id="case-status">Scene not checked</span>
        </header>
        <div class="conversation" id="conversation" role="log" aria-live="polite" aria-relevant="additions">
          <p class="time-stamp">CALL RECEIVED · 14:32</p>
        </div>
        <div class="quick-prompts" id="quick-prompts" aria-label="Suggested actions"></div>
        <form class="composer" id="composer">
          <input id="message-input" type="text" autocomplete="off" placeholder="Ask a question or describe your assessment..." aria-label="Your assessment or question">
          <button class="send-button" type="submit">Send</button>
        </form>
      </section>

      <aside class="panel details-panel" aria-label="Patient information">
        <div class="patient-identity">
          <div class="patient-avatar" id="patient-avatar" aria-hidden="true">JL</div>
          <div><h2 class="patient-name" id="patient-name">Jordan Lee</h2><p class="patient-meta" id="patient-meta">58 years · Female</p></div>
        </div>
        <div class="vitals-block">
          <div class="vitals-title"><h3>Patient snapshot</h3><span class="lock-label" id="vitals-lock">Locked</span></div>
          <div class="vital-row"><span>Blood pressure</span><span class="vital-value" data-vital="bp">— / —</span></div>
          <div class="vital-row"><span>Pulse</span><span class="vital-value" data-vital="pulse">— bpm</span></div>
          <div class="vital-row"><span>Respirations</span><span class="vital-value" data-vital="resp">— /min</span></div>
          <div class="vital-row"><span>SpO₂</span><span class="vital-value" data-vital="spo2">— %</span></div>
          <div class="vital-row"><span>Blood glucose</span><span class="vital-value" data-vital="glucose">— mg/dL</span></div>
        </div>
        <div class="finding-block">
          <div class="finding"><strong>Dispatch notes</strong><p id="dispatch-note">Family reports the patient is acting confused and sweating.</p></div>
          <button class="reset-button" id="reset-button" type="button">↺ &nbsp;Restart this call</button>
          <p class="disclaimer">Practice scenarios only. Follow your local protocols and instructor guidance in real patient care.</p>
        </div>
      </aside>
    </section>
  </main>

  <script>
    const scenarios = {
      glucose: {
        title: "Sudden confusion at home",
        subtitle: "Medical call · You are the lead EMT",
        patient: "Jordan Lee",
        initials: "JL",
        demographics: "58 years · Female",
        dispatch: "Family reports the patient is acting confused and sweating.",
        scene: "You arrive at a private home. The patient's daughter meets you at the door. No immediate hazards are apparent; use standard precautions.",
        presentation: "The patient is sitting at the kitchen table, pale and diaphoretic. She looks toward you when you speak but answers slowly.",
        vitals: { bp: "104 / 68", pulse: "108", resp: "20", spo2: "97", glucose: "52" },
        history: "Her daughter says Jordan has diabetes and takes insulin. She skipped lunch today. No known allergies. The daughter is unsure when Jordan last took insulin.",
        complaint: "When asked what is wrong, she says, \"I feel shaky and a little confused.\"",
        treatment: "Jordan is awake and can follow a simple instruction. Before any oral glucose, confirm she can safely swallow and follow your local protocol.",
        trauma: false
      },
      fall: {
        title: "Fall from a ladder",
        subtitle: "Trauma call · You are the lead EMT",
        patient: "Morgan Reyes",
        initials: "MR",
        demographics: "64 years · Male",
        dispatch: "Caller reports a fall from a ladder with leg pain.",
        scene: "You arrive at a private driveway. The ladder is down and no electrical or traffic hazards are present. Use standard precautions.",
        presentation: "The patient is lying on the ground beside the ladder, alert and anxious. His left lower leg is painful and visibly swollen.",
        vitals: { bp: "138 / 84", pulse: "112", resp: "22", spo2: "96", glucose: "108" },
        history: "The patient says he fell about six feet and landed on his left side. He denies losing consciousness. He takes a blood thinner; no known allergies. Last meal was this morning.",
        complaint: "He says, \"My leg hurts badly. I didn't hit my head.\"",
        treatment: "The patient is alert. Avoid unnecessary movement, reassess distal circulation, sensation, and movement, and follow your local spinal motion and splinting protocols.",
        trauma: true
      }
    };

    const state = { scenario: "glucose", sceneSafe: false, primaryAssessed: false, vitalsRevealed: false, historyRevealed: false, messages: [] };
    const conversation = document.querySelector("#conversation");
    const input = document.querySelector("#message-input");
    const prompts = document.querySelector("#quick-prompts");
    const status = document.querySelector("#case-status");

    function addMessage(text, sender = "patient") {
      state.messages.push({ text, sender });
      const message = document.createElement("div");
      message.className = `message ${sender === "user" ? "user" : "patient"}`;
      if (sender !== "user") {
        const avatar = document.createElement("span");
        avatar.className = "avatar";
        avatar.textContent = "PT";
        message.append(avatar);
      }
      const bubble = document.createElement("div");
      bubble.className = "bubble";
      bubble.append(document.createTextNode(text));
      const time = document.createElement("span");
      time.className = "message-time";
      time.textContent = new Intl.DateTimeFormat(undefined, { hour: "numeric", minute: "2-digit" }).format(new Date());
      bubble.append(time);
      message.append(bubble);
      conversation.append(message);
      conversation.scrollTop = conversation.scrollHeight;
    }

    function updateSnapshot() {
      const scenario = scenarios[state.scenario];
      const vitalValues = state.vitalsRevealed ? scenario.vitals : { bp: "— / —", pulse: "—", resp: "—", spo2: "—", glucose: "—" };
      for (const [key, value] of Object.entries(vitalValues)) {
        const suffix = { pulse: " bpm", resp: " /min", spo2: " %", glucose: " mg/dL" }[key] || "";
        const element = document.querySelector(`[data-vital="${key}"]`);
        element.textContent = `${value}${suffix}`;
        element.classList.toggle("revealed", state.vitalsRevealed);
      }
      document.querySelector("#vitals-lock").textContent = state.vitalsRevealed ? "Obtained" : "Locked";
      status.textContent = state.sceneSafe ? (state.primaryAssessed ? "Assessment underway" : "Scene safe") : "Scene not checked";
      status.classList.toggle("ready", state.sceneSafe);
    }

    function setPrompts() {
      const suggestions = state.sceneSafe
        ? ["Perform primary assessment", "Check blood pressure", "Ask about history"]
        : ["Check scene safety", "What is the blood pressure?", "Assess the patient"];
      prompts.replaceChildren();
      for (const suggestion of suggestions) {
        const button = document.createElement("button");
        button.type = "button";
        button.className = "quick-prompt";
        button.textContent = suggestion;
        button.addEventListener("click", () => handleInput(suggestion));
        prompts.append(button);
      }
    }

    function startScenario(id) {
      state.scenario = id;
      state.sceneSafe = false;
      state.primaryAssessed = false;
      state.vitalsRevealed = false;
      state.historyRevealed = false;
      state.messages = [];
      const scenario = scenarios[id];
      document.querySelector("#case-title").textContent = scenario.title;
      document.querySelector("#case-subtitle").textContent = scenario.subtitle;
      document.querySelector("#patient-name").textContent = scenario.patient;
      document.querySelector("#patient-avatar").textContent = scenario.initials;
      document.querySelector("#patient-meta").textContent = scenario.demographics;
      document.querySelector("#dispatch-note").textContent = scenario.dispatch;
      conversation.replaceChildren();
      addMessage(`Dispatch: ${scenario.dispatch} You are on scene. What do you do first?`);
      updateSnapshot();
      setPrompts();
    }

    function handleInput(rawText) {
      const text = rawText.trim();
      if (!text) return;
      addMessage(text, "user");
      input.value = "";
      const query = text.toLowerCase();
      const scenario = scenarios[state.scenario];
      const asksSafety = /scene|hazard|safe|safety|ppe|precaution/.test(query);

      if (!state.sceneSafe && asksSafety) {
        state.sceneSafe = true;
        addMessage(`Scene assessment: ${scenario.scene} Scene is safe to approach.`);
        updateSnapshot();
        setPrompts();
        return;
      }
      if (!state.sceneSafe) {
        addMessage("Before approaching or assessing the patient, confirm scene safety and use appropriate PPE. Start with the scene.");
        return;
      }

      const asksVitals = /blood pressure|\bbp\b|vital|pulse|heart rate|respirat|breathing rate|spo2|oxygen saturation|glucose|blood sugar/.test(query);
      const asksPrimary = /primary|assess|assessment|responsiv|alert|airway|breath|circulation|general impression|look at the patient/.test(query);
      const asksHistory = /history|sample|opqrst|medication|allerg|medical condition|what happened|what.s wrong|complaint|pain/.test(query);
      const asksTreatment = /treat|oxygen|glucose gel|oral glucose|sugar|splint|move|immobili/.test(query);

      if (asksPrimary) {
        state.primaryAssessed = true;
        addMessage(`Primary assessment: ${scenario.presentation} Assess responsiveness, airway, breathing, and circulation; address immediate life threats and continue reassessment.`);
      } else if (asksVitals) {
        if (!state.primaryAssessed) {
          addMessage("Before focused vitals, perform your primary assessment: form a general impression and assess responsiveness, airway, breathing, and circulation.");
        } else {
          state.vitalsRevealed = true;
          const v = scenario.vitals;
          addMessage(`Vitals obtained: BP ${v.bp} mmHg; pulse ${v.pulse}/min; respirations ${v.resp}/min; SpO₂ ${v.spo2}%; blood glucose ${v.glucose} mg/dL.`);
          updateSnapshot();
        }
      } else if (asksHistory) {
        state.historyRevealed = true;
        addMessage(`Patient interview: ${scenario.complaint} ${scenario.history}`);
      } else if (asksTreatment) {
        if (!state.primaryAssessed) {
          addMessage("Assess responsiveness, airway, and ability to follow instructions before choosing a treatment. Continue with your local protocol.");
        } else {
          addMessage(`Treatment considerations: ${scenario.treatment}`);
        }
      } else {
        addMessage("I don't have a specific response for that yet. Try an assessment action, ask about symptoms or history, or request a vital sign. Keep following your assessment sequence.");
      }
      updateSnapshot();
      setPrompts();
    }

    document.querySelector("#composer").addEventListener("submit", event => {
      event.preventDefault();
      handleInput(input.value);
      input.focus();
    });
    document.querySelectorAll("[data-scenario]").forEach(button => {
      button.addEventListener("click", () => {
        document.querySelectorAll("[data-scenario]").forEach(item => item.setAttribute("aria-pressed", String(item === button)));
        startScenario(button.dataset.scenario);
      });
    });
    document.querySelector("#reset-button").addEventListener("click", () => startScenario(state.scenario));

    startScenario("glucose");
  </script>
</body>
</html># emt-scenario-trainer
