---
layout: default
title: A6 - Bedside Patient Monitor
---

<link href="https://fonts.googleapis.com/css2?family=Pacifico&family=Quicksand:wght@400;500;600;700&display=swap" rel="stylesheet">

<style>
.cutesy {
  font-family: 'Quicksand', sans-serif;
  color: #5a3e5b;
  background:
    radial-gradient(circle at 10% 0%, #ffe9f3 0%, transparent 40%),
    radial-gradient(circle at 90% 10%, #e8f4ff 0%, transparent 40%),
    radial-gradient(circle at 50% 100%, #fff4e0 0%, transparent 40%),
    #fff8fc;
  padding: 28px 24px;
  border-radius: 24px;
  box-shadow: 0 8px 24px rgba(255, 182, 213, 0.25);
  border: 2px dashed #ffc4dd;
  position: relative;
}
.cutesy a { color: #d96a9b; text-decoration: none; border-bottom: 1px dotted #f3a8c8; }
.cutesy a:hover { color: #b04779; border-bottom-style: solid; }

.cutesy .hero {
  text-align: center;
  padding: 18px 12px 24px;
  background: linear-gradient(135deg, #ffd6e8 0%, #e0d4ff 50%, #d6f0ff 100%);
  border-radius: 20px;
  margin-bottom: 24px;
  box-shadow: inset 0 0 24px rgba(255, 255, 255, 0.6);
  position: relative;
  overflow: hidden;
}
.cutesy .hero h1 {
  font-family: 'Pacifico', cursive;
  color: #b04779;
  font-size: 2.1em;
  margin: 8px 0 6px;
  text-shadow: 1px 1px 0 #fff, 2px 2px 4px rgba(255, 138, 184, 0.4);
}
.cutesy .hero .subtitle {
  font-style: italic;
  color: #7a5a8a;
  margin: 0;
  font-weight: 500;
}
.cutesy .hero .twemoji-lg {
  width: 56px; height: 56px;
  vertical-align: middle;
  margin: 0 6px;
  filter: drop-shadow(0 3px 4px rgba(199, 109, 161, 0.35));
}
.cutesy .twemoji {
  width: 1.2em; height: 1.2em;
  vertical-align: -0.25em;
  margin: 0 2px;
}

.cutesy .meta-strip {
  display: flex; flex-wrap: wrap; justify-content: center; gap: 10px;
  margin: -8px 0 24px;
}
.cutesy .meta-pill {
  background: #fff;
  border: 2px solid #ffd0e3;
  border-radius: 999px;
  padding: 6px 14px;
  font-size: 0.92em;
  font-weight: 600;
  color: #8a4a72;
  box-shadow: 0 2px 6px rgba(255, 182, 213, 0.3);
}

.cutesy .card {
  background: #fff;
  border-radius: 18px;
  padding: 18px 22px;
  margin: 18px 0;
  box-shadow: 0 4px 12px rgba(218, 168, 197, 0.2);
  border-left: 6px solid #ffb6d5;
}
.cutesy .card.lavender { border-left-color: #c8b6ff; }
.cutesy .card.mint    { border-left-color: #b6ead0; }
.cutesy .card.peach   { border-left-color: #ffd5b6; }

.cutesy h2 {
  font-family: 'Pacifico', cursive;
  color: #c1568d;
  font-size: 1.6em;
  margin-top: 0;
  display: flex; align-items: center; gap: 10px;
}
.cutesy h2 .twemoji { width: 1.3em; height: 1.3em; }

.cutesy ul { list-style: none; padding-left: 4px; }
.cutesy ul li {
  padding: 6px 0 6px 28px;
  position: relative;
}
.cutesy ul li::before {
  content: "✿";
  position: absolute;
  left: 4px;
  color: #f0a4c4;
  font-size: 1.1em;
}
.cutesy ul li b, .cutesy ul li strong { color: #b04779; }

.cutesy table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  background: #fff;
  border-radius: 14px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(218, 168, 197, 0.18);
}
.cutesy th {
  background: linear-gradient(135deg, #ffd6e8, #e0d4ff);
  color: #5a3e5b;
  padding: 10px 12px;
  text-align: left;
  font-weight: 700;
}
.cutesy td {
  padding: 10px 12px;
  border-top: 1px dashed #ffe0ee;
}
.cutesy tr:nth-child(even) td { background: #fff8fc; }

.cutesy code {
  background: #fff0f7;
  color: #b04779;
  padding: 2px 6px;
  border-radius: 6px;
  font-size: 0.9em;
  border: 1px solid #ffd6e8;
}

.cutesy .footer-note {
  text-align: center;
  margin-top: 24px;
  color: #a07a98;
  font-style: italic;
  font-size: 0.95em;
}
.cutesy .sparkle {
  display: inline-block;
  animation: twinkle 2s ease-in-out infinite;
}
@keyframes twinkle {
  0%, 100% { opacity: 1; transform: scale(1); }
  50%      { opacity: 0.6; transform: scale(1.15); }
}
</style>

<div class="cutesy" markdown="0">

  <div class="hero">
    <img class="twemoji-lg" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1fa7a.svg" alt="stethoscope">
    <img class="twemoji-lg" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1f496.svg" alt="sparkling heart">
    <img class="twemoji-lg" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1f3e5.svg" alt="hospital">
    <h1>A6 ~ Bedside Patient Monitor</h1>
    <p class="subtitle">a teeny tiny RTOS project for keeping patients safe and sound <span class="sparkle">✨</span></p>
  </div>

  <div class="meta-strip">
    <span class="meta-pill">📚 Real-Time Systems · Spring 2026</span>
    <span class="meta-pill">🏥 Healthcare theme</span>
    <span class="meta-pill">🤖 ESP-IDF + FreeRTOS</span>
    <span class="meta-pill">💻 ESP32 dual-core</span>
  </div>

  <div class="card peach">
    <p>A simulated bedside vitals monitor for the kind of low-cost telemetry hardware a Central Florida medical-device startup might ship. <img class="twemoji" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1f338.svg" alt=""> A potentiometer stands in for an analog vital sensor; an ESP32 samples it on a hard-real-time loop, raises an audible/visual alarm when the reading crosses a configurable threshold, and publishes patient state to a Wi-Fi nurse dashboard that any phone or laptop on the same network can reach. <img class="twemoji" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1f496.svg" alt=""></p>
  </div>

  <div class="card">
    <h2>
      <img class="twemoji" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1f338.svg" alt="">
      What it does
    </h2>
    <ul>
      <li><b>Vital sensing</b> — ADC1 channel 6 (GPIO34) is sampled every 20 ms. Threshold crossings post a counting semaphore to the alarm handler.</li>
      <li><b>Nurse-call button</b> — GPIO18 with a hardware ISR. The ISR is debounced (50 ms), toggles alarm mode under a critical section, and posts to a depth-1 queue so a flurry of presses collapses into a single ack.</li>
      <li><b>Alarm pipeline</b> — A high-priority handler (<code>alarm_handler_task</code>, prio 6) fires the red LED via a separate <code>blink_task</code> so the handler itself never blocks on <code>vTaskDelay</code> and never misses its 10 ms wake budget.</li>
      <li><b>Nurse dashboard</b> — HTTP server on port 80 with auto-refreshing HTML, remote LED toggles, and a one-click "Nurse Acknowledge" link. Pinned to core 0 so the Wi-Fi stack and hard-real-time tasks live on different cores.</li>
      <li><b>Risk analysis</b> — A soft task with variable compute (20–99 ms per pass, 250 ms period) that classifies the latest reading without ever blocking the sense loop.</li>
      <li><b>Heartbeat / status LED</b> — Slow blink on the green LED, suppressed when the dashboard manually overrides it.</li>
    </ul>
  </div>

  <div class="card lavender">
    <h2>
      <img class="twemoji" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/23f0.svg" alt="">
      Real-time design
    </h2>
    <table>
      <thead>
        <tr><th>Task</th><th>Class</th><th>Period / Trigger</th><th>Deadline</th><th>Core</th></tr>
      </thead>
      <tbody>
        <tr><td><code>alarm_handler</code></td><td>Hard</td><td>Event (sem + btn)</td><td>10 ms</td><td>1</td></tr>
        <tr><td><code>vital_sensor</code></td><td>Hard</td><td>20 ms periodic</td><td>20 ms</td><td>1</td></tr>
        <tr><td><code>nurse_dashboard</code></td><td>Soft</td><td>Per request</td><td>500 ms</td><td>0</td></tr>
        <tr><td><code>risk_analysis</code></td><td>Soft</td><td>250 ms periodic</td><td>250 ms</td><td>1</td></tr>
        <tr><td><code>heartbeat</code></td><td>Soft</td><td>2000 ms periodic</td><td>—</td><td>1</td></tr>
        <tr><td><code>blink_task</code></td><td>Soft</td><td>Event-driven</td><td>—</td><td>1</td></tr>
      </tbody>
    </table>
    <p style="margin-top:14px;"><b>Concurrency primitives at a glance:</b></p>
    <ul>
      <li><b>Counting semaphore</b> (<code>sem_vital</code>, depth 20) — vital task → alarm handler.</li>
      <li><b>Mutexes</b> — one for serialized log output, one for cross-core shared state (<code>latest_vital</code>, dashboard LED overrides).</li>
      <li><b>Critical section spinlock</b> (<code>mode_mux</code>) — guards <code>alarm_mode</code>, the only state mutated from both ISR and task contexts.</li>
      <li><b>Queues</b> — vital readings (depth 10), blink commands (depth 4), and a depth-1 overwrite queue for nurse-call events.</li>
    </ul>
  </div>

  <div class="card mint">
    <h2>
      <img class="twemoji" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1fa79.svg" alt="">
      Things that bit me along the way
    </h2>
    <ul>
      <li>Wokwi fires a synthetic edge on the button pin at sim start. The fix was to bring Wi-Fi up first, install the ISR after a short delay, then drain the queue once before any task can read from it.</li>
      <li>An earlier version of <code>alarm_handler_task</code> spun on the semaphore with a zero timeout to stay responsive to button presses. That starved IDLE1 and tripped the task watchdog. Switching to a 10 ms timeout on the semaphore take fixed it without giving up responsiveness.</li>
      <li>ESP-IDF v5 renamed <code>ADC_ATTEN_DB_11</code> to <code>ADC_ATTEN_DB_12</code> (same hardware behaviour, just a more honest name).</li>
    </ul>
  </div>

  <p class="footer-note">
    <span class="sparkle">✨</span>
    built with love on ESP-IDF + FreeRTOS &nbsp;·&nbsp;
    <a href="https://github.com/peytonlynnbarnes">github.com/peytonlynnbarnes</a>
    <span class="sparkle">✨</span>
  </p>

</div>
