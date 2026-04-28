---
layout: default
title: Home
---

# A6 — Bedside Patient Monitor

> **Course:** Real-Time Systems, Spring 2026
> **Theme:** Healthcare
> **Platform:** ESP32 (dual-core Xtensa) on ESP-IDF + FreeRTOS

A simulated bedside vitals monitor for the kind of low-cost telemetry hardware
a Central Florida medical-device startup might ship. A potentiometer stands in
for an analog vital sensor; an ESP32 samples it on a hard-real-time loop, raises
an audible/visual alarm when the reading crosses a configurable threshold, and
publishes patient state to a Wi-Fi nurse dashboard that any phone or laptop on
the same network can reach.

---

## What it does

- **Vital sensing** — ADC1 channel 6 (GPIO34) is sampled every 20 ms. Threshold
  crossings post a counting semaphore to the alarm handler.
- **Nurse-call button** — GPIO18 with a hardware ISR. The ISR is debounced
  (50 ms), toggles alarm mode under a critical section, and posts to a depth-1
  queue so a flurry of presses collapses into a single ack.
- **Alarm pipeline** — A high-priority handler (`alarm_handler_task`, prio 6)
  fires the red LED via a separate `blink_task` so the handler itself never
  blocks on `vTaskDelay` and never misses its 10 ms wake budget.
- **Nurse dashboard** — HTTP server on port 80 with auto-refreshing HTML,
  remote LED toggles, and a one-click "Nurse Acknowledge" link. Pinned to
  core 0 so the Wi-Fi stack and hard-real-time tasks live on different cores.
- **Risk analysis** — A soft task with variable compute (20–99 ms per pass,
  250 ms period) that classifies the latest reading without ever blocking the
  sense loop.
- **Heartbeat / status LED** — Slow blink on the green LED, suppressed when the
  dashboard manually overrides it.

## Real-time design

| Task             | Class | Period / Trigger | Deadline | Core |
|------------------|-------|------------------|----------|------|
| `alarm_handler`  | Hard  | Event (sem + btn)| 10 ms    | 1    |
| `vital_sensor`   | Hard  | 20 ms periodic   | 20 ms    | 1    |
| `nurse_dashboard`| Soft  | Per request      | 500 ms   | 0    |
| `risk_analysis`  | Soft  | 250 ms periodic  | 250 ms   | 1    |
| `heartbeat`      | Soft  | 2000 ms periodic | —        | 1    |
| `blink_task`     | Soft  | Event-driven     | —        | 1    |

Concurrency primitives at a glance:

- **Counting semaphore** (`sem_vital`, depth 20) — vital task → alarm handler.
- **Mutexes** — one for serialized log output, one for cross-core shared state
  (`latest_vital`, dashboard LED overrides).
- **Critical section spinlock** (`mode_mux`) — guards `alarm_mode`, the only
  state mutated from both ISR and task contexts.
- **Queues** — vital readings (depth 10), blink commands (depth 4), and a
  depth-1 overwrite queue for nurse-call events.

## Things that bit me along the way

- Wokwi fires a synthetic edge on the button pin at sim start. The fix was
  to bring Wi-Fi up first, install the ISR after a short delay, then drain
  the queue once before any task can read from it.
- An earlier version of `alarm_handler_task` spun on the semaphore with a
  zero timeout to stay responsive to button presses. That starved IDLE1 and
  tripped the task watchdog. Switching to a 10 ms timeout on the semaphore
  take fixed it without giving up responsiveness.
- ESP-IDF v5 renamed `ADC_ATTEN_DB_11` to `ADC_ATTEN_DB_12` (same hardware
  behaviour, just a more honest name).

---

*Built on ESP-IDF + FreeRTOS. Source available on
[GitHub](https://github.com/peytonlynnbarnes).*
