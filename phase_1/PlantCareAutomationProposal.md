# Project Proposal: Energy-Autonomous Plant-Care Controller

## Mission Statement

For growers who need a living plant cared for without reliable power, the **Plant-Care Controller** is a solar-harvested embedded system that only acts when it can afford to — unlike always-powered auto-waterers, which never have to ask that question.

---

## User Stories and Acceptance Criteria

Each story below is a distinct, independently buildable unit of work. Acceptance criteria are written as an assumption table: what has to hold true for the story to be considered done, how it will be verified, and the pass/fail threshold. Since these are pre-build acceptance criteria rather than a record of a completed system, the **Result** column is marked *pending* throughout — it's the column to fill in as each story is actually built and tested, not a claim that testing has already happened.

### Story 1

**As a** plant owner leaving for an extended period, **I want** the system to automatically water or dose my plant based on its sensed condition, **so that** my plant survives without me being present to check on it.

| Assumption | Test Method | Pass/Fail Threshold | Result |
|---|---|---|---|
| The sensor can reliably detect a low water/nutrient condition | Compare sensor reading against a manual ground-truth measurement across several fill levels | Sensor and manual reading agree within an acceptable margin at least 9/10 trials | Pending |
| The actuator delivers a correct, repeatable dose when triggered | Trigger the actuator N times and measure delivered volume each time | Delivered volume stays within a defined tolerance of the target across all trials | Pending |
| The plant stays within a healthy condition range across an unattended period with no manual intervention | Run the full loop unattended for a defined multi-day period; inspect plant condition at the end | Plant shows no signs of water/nutrient stress at end of period | Pending |

### Story 2

**As a** researcher relying on the system to run unattended, **I want** the system to keep operating on harvested energy alone without draining its reserve to empty, **so that** the run doesn't fail because the controller went dark.

| Assumption | Test Method | Pass/Fail Threshold | Result |
|---|---|---|---|
| The harvester and buffer can sustain the minimum required duty cycle under realistic low-light conditions | Run the system under the dimmest expected lighting condition and log buffer charge over time | Buffer charge never reaches zero over a defined test window | Pending |
| The system detects a low-energy state and enters a safe, reduced-activity mode before full depletion | Deliberately starve the harvester and observe system behavior as charge drops | System reduces its own duty cycle (rather than crashing or hanging) once charge crosses a defined low threshold | Pending |

### Story 3

**As an** engineer evaluating the system's core research question, **I want** to run the same care policy under several fixed energy budgets and log both outcome quality and response time at each, **so that** I can characterize how care quality changes as the energy budget shrinks.

| Assumption | Test Method | Pass/Fail Threshold | Result |
|---|---|---|---|
| The system's duty cycle can be fixed/throttled to a few distinct, repeatable budget levels for controlled comparison | Configure the system at each intended budget level and confirm the resulting duty cycle matches the target | Measured duty cycle at each level falls within a defined tolerance of its target | Pending |
| Logging captures both a care-outcome measure and a response-latency measure at each budget level | Run a full test cycle at one budget level and review the resulting log | Log contains both a usable outcome metric and a usable latency metric with no gaps | Pending |
| The outcome and/or latency measures actually differ in a distinguishable way across budget levels | Compare logged results across all tested budget levels | At least a directional difference is observable between the highest and lowest budget levels tested | Pending |

### Story 4

**As a** remote or off-grid deployment operator, **I want** the system to survive a multi-day stretch of unusually low harvested energy without failing outright, **so that** a run of bad weather or low light doesn't kill the plant or the device.

| Assumption | Test Method | Pass/Fail Threshold | Result |
|---|---|---|---|
| The energy buffer is sized to bridge a defined worst-case low-harvest period | Simulate a defined worst-case low-harvest period (e.g., near-zero input) and observe system behavior | System remains in a safe, recoverable state through the full simulated period | Pending |
| The system degrades gracefully (does something less often) rather than failing outright when energy is scarce | Observe system behavior as it enters a low-energy state | System reduces activity in a controlled, predictable way rather than behaving erratically or hanging | Pending |

### Story 5

**As a** person checking on the system without being physically present, **I want** visibility into its recent decisions and energy state, **so that** I can confirm it's working correctly from a distance.

| Assumption | Test Method | Pass/Fail Threshold | Result |
|---|---|---|---|
| The system records each sense/decide/act event along with the energy state at that moment | Run the system for a defined period and review the resulting log | Every actuation event has a corresponding, timestamped log entry with energy state recorded | Pending |
| The log is retrievable without needing to physically interrupt or reset the running system | Retrieve the log while the system continues operating | Log is retrieved successfully with no interruption to normal operation | Pending |

---

## Core Assumptions and Kill Criteria

The proposal above rests on four load-bearing assumptions. Two of them are hard kill criteria — if either turns out false, the project as scoped has no experiment left to run, and it should be stopped and fundamentally rethought rather than patched. The other two are serious risks that would force a major re-scope, but not necessarily an outright stop.

**1. The plant will show a measurable, distinguishable care-outcome difference across different response speeds within the available observation window.**
**— Kill criterion.** The entire point of the system is to measure how care quality changes as the energy budget shrinks. If the chosen plant either can't be driven into a visibly different outcome by delayed care within the time available, or the outcome can't be measured cheaply and reliably, there is no dependent variable left to test — the project would have nothing left to demonstrate, no matter how well the hardware works.

**2. A realistically small solar harvester and energy buffer can support a meaningfully variable duty-cycle range in the intended environment — not just "always effectively full" or "always effectively starved."**
**— Kill criterion.** The research question requires a real spectrum between generous and scarce power. If the available harvested energy in the intended environment collapses to only two states — comfortably sufficient no matter the setting, or never enough to wake the controller at all — the energy-budget-vs-care-quality tradeoff has nothing in between to characterize, and the project's central question can't be answered as designed.

**3. A low-power actuator can dose water or nutrients in small, controllable increments cheaply enough to fit within the harvested energy budget alongside sensing and computation.**
**— High risk, not an automatic kill.** If the actuator turns out to consume a disproportionate share of the available energy, or can't be controlled finely enough, the project would need to re-scope the actuation approach (e.g., a simpler on/off valve, less frequent full doses, or a different actuation mechanism) rather than stop outright.

**4. The hardware/firmware build and enough multi-condition test cycles can be completed within the available time, despite the plant's own growth and response timescale running on its own multi-day clock that can't be sped up.**
**— High risk, not an automatic kill.** If establishing a stable, healthy plant baseline consumes most of the available time, the project would need to shrink its scope — fewer budget levels tested, a shorter observation window, or a faster-responding proxy measurement — rather than abandon the work entirely.
