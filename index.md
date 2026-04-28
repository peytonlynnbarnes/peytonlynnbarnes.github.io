---
layout: default
title: A6 - Bedside Patient Monitor
---

<style>
  .doc h1 {
    font-family: 'Inter', sans-serif;
    font-size: 1.9em;
    font-weight: 700;
    color: #1f3a3f;
    margin: 0 0 6px;
    letter-spacing: -0.01em;
  }
  .doc .lede {
    color: #5a6770;
    font-size: 1.02em;
    margin: 0 0 4px;
  }
  .doc .meta-strip {
    display: flex; flex-wrap: wrap; gap: 8px;
    margin: 18px 0 24px;
  }
  .doc .meta-pill {
    background: #ffffff;
    border: 1px solid #d6e4dc;
    border-radius: 999px;
    padding: 4px 12px;
    font-size: 0.85em;
    font-weight: 500;
    color: #4a6770;
  }
  .doc .meta-pill .label {
    color: #4a8a7a;
    font-weight: 600;
    margin-right: 4px;
  }
  .doc .header-icon {
    display: inline-block;
    width: 32px; height: 32px;
    vertical-align: -8px;
    margin-right: 8px;
  }

  .doc .card {
    background: #ffffff;
    border: 1px solid #e3ebe7;
    border-left: 3px solid #4a8a7a;
    border-radius: 10px;
    padding: 20px 24px;
    margin: 18px 0;
    box-shadow: 0 1px 2px rgba(60, 90, 80, 0.04);
  }
  .doc .card.alt   { border-left-color: #7aa6b8; }
  .doc .card.warm  { border-left-color: #c89066; }

  .doc h2 {
    font-family: 'Inter', sans-serif;
    font-size: 1.2em;
    font-weight: 600;
    color: #1f3a3f;
    margin: 0 0 12px;
    letter-spacing: -0.005em;
  }
  .doc h2 .num {
    color: #4a8a7a;
    font-weight: 700;
    margin-right: 8px;
    font-variant-numeric: tabular-nums;
  }

  .doc p { margin: 0 0 12px; }
  .doc p:last-child { margin-bottom: 0; }

  .doc ul {
    margin: 0; padding-left: 20px;
  }
  .doc ul li {
    margin: 6px 0;
  }
  .doc ul li::marker { color: #4a8a7a; }
  .doc strong { color: #1f3a3f; }

  .doc table {
    width: 100%;
    border-collapse: collapse;
    margin: 8px 0 4px;
    font-size: 0.94em;
  }
  .doc th, .doc td {
    text-align: left;
    padding: 8px 12px;
    border-bottom: 1px solid #e3ebe7;
  }
  .doc th {
    background: #eef5f1;
    color: #1f3a3f;
    font-weight: 600;
    font-size: 0.86em;
    text-transform: uppercase;
    letter-spacing: 0.04em;
  }
  .doc tr:last-child td { border-bottom: none; }

  .doc code {
    font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
    background: #eef5f1;
    color: #1f3a3f;
    padding: 1px 6px;
    border-radius: 4px;
    font-size: 0.9em;
  }

  .doc .subhead {
    margin-top: 16px;
    font-weight: 600;
    color: #1f3a3f;
  }
</style>

<div class="doc" markdown="0">

  <h1>
    <img class="header-icon" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1fa7a.svg" alt="">
    A6 — Bedside Patient Monitor
  </h1>
  <p class="lede">A simulated vitals monitor and Wi-Fi nurse dashboard for the ESP32, written for the Real-Time Systems coursework in Spring 2026.</p>

  <div class="meta-strip">
    <span class="meta-pill"><span class="label">Course</span> Real-Time Systems · Sp 2026</span>
    <span class="meta-pill"><span class="label">Theme</span> Healthcare</span>
    <span class="meta-pill"><span class="label">Framework</span> ESP-IDF + FreeRTOS</span>
    <span class="meta-pill"><span class="label">Target</span> ESP32 (dual-core)</span>
  </div>

  <div class="card">
    <h2><span class="num">01</span>Overview</h2>
    <p>A bedside vitals monitor for the kind of low-cost telemetry hardware a small medical-device shop might ship. A potentiometer stands in for an analog sensor. The ESP32 samples it on a hard-real-time loop, raises an alarm when the reading crosses a configurable threshold, and publishes patient state to a Wi-Fi nurse dashboard reachable from any phone or laptop on the same network.</p>
  </div>

  <div class="card">
    <h2><span class="num">02</span>What it does</h2>
    <ul>
      <li><strong>Vital sensing.</strong> ADC1 channel 6 (GPIO34) is sampled every 20 ms. Threshold crossings post a counting semaphore to the alarm handler.</li>
      <li><strong>Nurse-call button.</strong> GPIO18 with a hardware ISR. Debounced at 50 ms, toggles alarm mode under a critical section, and posts to a depth-1 queue so a flurry of presses collapses into a single ack.</li>
      <li><strong>Alarm pipeline.</strong> A high-priority handler (<code>alarm_handler_task</code>, priority 6) fires the red LED via a separate <code>blink_task</code> so the handler never blocks on <code>vTaskDelay</code> and never misses its 10 ms wake budget.</li>
      <li><strong>Nurse dashboard.</strong> HTTP server on port 80 with auto-refreshing HTML, remote LED toggles, and a one-click acknowledge link. Pinned to core 0 so the Wi-Fi stack and the hard-real-time tasks live on different cores.</li>
      <li><strong>Risk analysis.</strong> A soft task with variable compute (20–99 ms per pass, 250 ms period) classifies the latest reading without ever blocking the sense loop.</li>
      <li><strong>Heartbeat.</strong> Slow blink on the green LED, suppressed when the dashboard manually overrides it.</li>
    </ul>
  </div>

  <div class="card alt">
    <h2><span class="num">03</span>Real-time design</h2>
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

    <p class="subhead">Concurrency primitives</p>
    <ul>
      <li><strong>Counting semaphore</strong> (<code>sem_vital</code>, depth 20) — vital task to alarm handler.</li>
      <li><strong>Mutexes</strong> — one for serialized log output, one for cross-core shared state (<code>latest_vital</code>, dashboard LED overrides).</li>
      <li><strong>Critical-section spinlock</strong> (<code>mode_mux</code>) — guards <code>alarm_mode</code>, the only state mutated from both ISR and task contexts.</li>
      <li><strong>Queues</strong> — vital readings (depth 10), blink commands (depth 4), and a depth-1 overwrite queue for nurse-call events.</li>
    </ul>
  </div>

  <div class="card warm">
    <h2><span class="num">04</span>Notes from development</h2>
    <ul>
      <li>Wokwi fires a synthetic edge on the button pin at sim start. The fix was to bring Wi-Fi up first, install the ISR after a short delay, then drain the queue once before any task can read from it.</li>
      <li>An earlier <code>alarm_handler_task</code> spun on the semaphore with a zero timeout to stay responsive to button presses. That starved IDLE1 and tripped the task watchdog. Switching to a 10 ms timeout on the semaphore take fixed it without giving up responsiveness.</li>
      <li>ESP-IDF v5 renamed <code>ADC_ATTEN_DB_11</code> to <code>ADC_ATTEN_DB_12</code> — same hardware behavior, more honest name.</li>
    </ul>
  </div>

</div>
