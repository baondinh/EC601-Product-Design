# Energy-Constrained Closed-Loop Plant-Care Automation
**EC602 Product Design — Project Plan and Testing Phase**

---

## 1. Project Summary

This project delivers an energy-constrained, solar-powered closed-loop plant-care system that autonomously monitors and responds to plant health whenever stored charge allows, giving hobbyists, farmers, and researchers continuous, low-maintenance, low-energy plant care in resource-limited settings.

| Question | Answer |
|---|---|
| **Who** | Hobbyists, farmers, and researchers who need reliable plant care without constant manual attention |
| **What** | A solar-harvested, closed-loop plant-care automation system |
| **When** | Whenever enough stored energy is available (charge-triggered wake) |
| **Where** | Resource- or power-limited growing environments |
| **Why** | Maximize plant growth outcomes while minimizing energy use and hands-on intervention |

---

## 2. System Architecture

The design uses a tiered wake-up so each stage only powers on when needed.

1. **Solar panel** trickle-charges a **supercapacitor** (no LiPo battery).
2. A **comparator-based wake gate** (UVLO with hysteresis) watches the supercapacitor voltage and connects power to the controller only when enough energy is banked.
3. A **microcontroller** (ESP32-CAM) wakes, reads sensors, captures an image, and makes the care decision.
4. An optional **low-power FPGA accelerator** (Lattice iCE40 UltraPlus) is powered briefly to process image data (e.g., pigment threshold and pixel counting), returns a small result, and powers down.
5. The MCU drives the **actuator** (mini diaphragm pump for misting or water top-off) through a MOSFET, then everything sleeps.

A fixed RTC ceiling (e.g., check at least once every 12 hours if energy permits) can be added as a hybrid safeguard so the system never goes dark for too long.

### Hardware roles

| Component | Role |
|---|---|
| Solar panel (3W, 6V, ~500mA) | Energy source |
| Supercapacitor (1F, 5.5V) | Sole energy storage |
| LM393 comparator + resistor divider + PNP transistor | Wake gate / load switch |
| ESP32-CAM (Freenove kit) | Control, sensing, camera |
| UPduino v3.1 (iCE40 UltraPlus) | Burst image-processing accelerator (stretch/scoped goal) |
| IRLZ44N logic-level MOSFET | Pump driver |
| R370 diaphragm pump (3.7V, ≤500mA) | Misting / water actuation |
| DHT22 | Humidity/temperature sensing |

### Key design notes

- Pump runtime is capped in firmware per wake cycle; pump energy per burst must be checked against supercapacitor stored energy (½CV²).
- The LM393 draws more quiescent current than a nanopower comparator; measure it as part of the power budget.
- FPGA is justified only for burst-parallel work (image processing); MCU handles control and sleep.

---

## 3. Biological Tracks

| Track | Species | Purpose | Notes |
|---|---|---|---|
| **Fast track** | *Lemna minor* (duckweed) | Short-window growth and sensing validation | Doubling roughly every 2–4 days; floats, so it needs a dedicated shallow, wide container and water-based actuation (top-off / nutrient dosing) rather than misting |
| **Slow track** | *Hypnum* (sheet moss) | Long-term moss-wall reuse | Growth measured in weeks; misting-based care |

Water: use distilled water (or a dechlorinator) for both tracks.

---

## 4. Overall Project Plan (12 Weeks)

| Weeks | Focus | Deliverables |
|---|---|---|
| Pre-phase (about 2 weeks) | Validation testing (Section 5) | Pass/pivot decision on each critical assumption |
| 1–3 | **Hardware** — energy harvesting and wake gate | Working panel → supercap → comparator → load; measured sleep/active current; power budget |
| 4–6 | **Embedded systems** — MCU wake/sleep loop | Sensor integration, camera capture, pump control with capped runtime |
| 7–9 | **FPGA** — one narrow pipeline | Pigment threshold + pixel count block, bench-tested standalone, power/time compared to MCU software |
| 10–11 | **Integration** | FPGA accelerator in the wake loop; architecture diagram finalized |
| 12 | **Testing and demo** | End-to-end power measurements, results, demo |

**Fallback:** if the FPGA is behind by week 9, present the MCU-only closed loop as the core result and document the FPGA as future work. Hardware, architecture, and embedded systems deliverables remain complete.

**Difficulty estimate:** about 7/10 for the full scope. The main risks are the optical sensing metric (still undefined) and FPGA learning curve.

---

## 5. Testing Phase

Run all tests in parallel where possible. Live-plant tests set the pace, so start them first and leave extra time to pivot.

### Timeline and dependencies

Day 0 is the day parts and plants are in hand.

| Test | Starts | Duration | Depends on |
|---|---|---|---|
| 1. Solar light sufficiency | Day 0 | 1–2 days | — |
| 2. Plant growth | Day 0 | Continuous through the window | — |
| 3. Sensing sensitivity | ~Day 3–4 | Through end of window | Daily photos from Test 2 |
| 4. Actuation response | Day 0 | 3–5 days | — (parallel with Test 2) |
| 5. Wake circuit | Day 0 | 1–2 days | — |
| 6. FPGA feasibility | Day 0 | 3–5 days | — |

Only Test 3 depends on another test, and only on enough accumulated photos, not on Test 2 finishing.

### Test 1 — Solar light sufficiency
**Assumption:** indoor/windowsill light yields usable harvested energy.
1. Place the panel at the real intended location.
2. Log open-circuit voltage at several times of day.
3. Log short-circuit current at the same times.
4. Record time of day and light conditions in a spreadsheet.
5. Compare against the power budget.

**Pivot signal:** readings in the µW–mW range or near zero on cloudy periods → add supplemental light or use a larger panel.

### Test 2 — Plant growth
**Assumption:** supplied plants show visible growth within a short window.
1. Set up identical duckweed (and moss) samples under identical light and moisture.
2. Photograph daily from day 0.
3. Continue through the full window.

**Pivot signal:** no visible change by day 4 → change species or supplier, or rely on duckweed as the critical-path track.

### Test 3 — Sensing sensitivity
**Assumption:** the chosen marker (pigment saturation, coverage, frond count) is detectable above noise.
1. Reuse the Test 2 photos.
2. Compute color and coverage statistics per image (spreadsheet or short script).
3. Compare day 0 against later days relative to camera noise.

**Pivot signal:** day 0 vs. day 4 differences fall within the noise floor → choose a more sensitive marker or extend the observation window.

### Test 4 — Actuation response
**Assumption:** watering/misting produces a measurable response.
1. Prepare two identical samples: one on a regular schedule, one kept drier.
2. Compare using the same image analysis as Test 3.

**Pivot signal:** no difference between samples → the window is too short or the marker is insensitive to water stress.

### Test 5 — Wake circuit
**Assumption:** the supercapacitor/comparator circuit triggers at a sensible interval with real solar input.
1. Breadboard panel → comparator → supercapacitor → LED load.
2. Observe LED behavior over a full day.
3. Adjust divider resistors or capacitor size as needed.

**Pivot signal:** LED never blinks (undercharged) or blinks constantly (threshold too low) → resize the capacitor or adjust thresholds.

### Test 6 — FPGA feasibility
**Assumption:** a simple HDL pipeline is buildable in the available time.
1. Implement a threshold comparison on a few test pixel values.
2. Synthesize and simulate on the UPduino toolchain.

**Pivot signal:** still stuck on basic synthesis/simulation after 3–5 days → demote the FPGA to a stretch goal and process images in MCU software.

---

## 6. Parts List

| Item | Subsystem | Source |
|---|---|---|
| Freenove ESP32 Basic Starter Kit (ESP32-CAM) | MCU + camera | Amazon |
| 3W 6V polycrystalline solar panel | Energy harvesting | Amazon |
| Supercapacitor 1F 5.5V (KEMET FGR0H105ZF) | Energy storage | Digi-Key |
| LM393P comparator (DIP-8) | Wake gate | Digi-Key |
| 2N3906 PNP transistor | Load switch | Digi-Key |
| Carbon film resistor kit (0–1MΩ) | Dividers / pull-up | Amazon |
| IRLZ44NPBF logic-level MOSFET | Pump driver | Digi-Key |
| R370 diaphragm pump, 3.7V | Actuation | Amazon |
| Silicone tubing, 4mm ID | Actuation | Amazon |
| DHT22 sensor | Humidity/temperature | Amazon |
| UPduino v3.1 (iCE40 UltraPlus) | FPGA accelerator | Tindie |
| *Lemna minor* duckweed | Biological | Etsy |
| *Hypnum* moss | Biological | Etsy / The Bio Dude |
| Shallow clear propagation tray | Container | Amazon |
| Multimeter with µA range | Power measurement | Any |

---

## 7. Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Insufficient indoor solar energy | Test 1 early; supplemental light or larger panel |
| Growth too slow to observe | Prioritize duckweed; start plants day 0 |
| Optical marker too subtle | Test 3 on early photos; consider multispectral or frame-differencing markers |
| Pump overdraws supercapacitor | Budget energy per burst; cap pump runtime in firmware |
| FPGA learning curve | One narrow pipeline; MCU-only fallback |
| Water chemistry/evaporation drift (duckweed) | Distilled water; monitor level; top-off actuation |

---

## 8. Progress Log

| Date | Entry |
|---|---|
| | |
