# EC601 - Product Design in Electrical & Computer Engineering

Topic: Hardware, Architecture & Embedded Systems
Personal Interests: Sound, Plants, Biophilic Design
Using Claude Pro Sonnet 5

# Phase 0: Research & Professors at BU
## Ajay Joshi 
- Statement & Links:
  - The Boston University Integrated Circuits, Architectures & Systems (ICAS) Group focuses on developing novel architectures and circuits to design low-power, high-performance, and secure systems.
  - https://www.bu.edu/eng/profile/ajay-joshi/
  - https://www.bu.edu/icas/
- Recent Projects: 
  - Electro-Photonic Computing (EPiC) for On-Premise Applications (funded by IARPA)
  - Privacy-Preserving Computing using Fully Homomorphic Encryption (funded by RedHat, NSF)
  - Network and Memory Architectures for Manycore/GPU Systems (funded by NSF, DARPA)
  - Taming Memory Corruption with Security Monitors (funded by NSF, Google)
- Summarized Topics:
  - Computer architecture
  - Hardware security
  - Digital VLSI
  - Reconfigurable computing
  - Neuromorphic computing
  - Fully-homomorphic-encryption for ML security

## Rabia Yazicigil
- Statement & Links:
  - Wireless Integrated Systems and Extreme Circuits (WISE) is focused on innovating energy-efficient application-specific integrated circuits (ICs) and system solutions in diverse fields, including biosensing, information theory, signal processing, and secure wireless communications.
  - https://www.bu.edu/eng/profile/rabia-yazicigil-ph-d/
  - https://sites.bu.edu/wisecircuits/
- Recent Projects:
  - Cyber-Secure Biological Systems
  - All-in-One Data Decoders
  - Secure Wireless Communications
- Summarized Topics:
  - Energy-constrained wireless systems
  - RF/physical-layer security
  - Spectrum sensing
 
## Roscoe Giles - Professor Emeritus
- Statement & Links:
  - https://www.bu.edu/eng/profile/roscoe-giles/
- Summarized Topics:
  - Advanced computer architectures
  - Distributed and parallel computing
  - Advanced Scientific Computing
    
## Douglas Densmore
- Statement & Links:
  - Cross-disciplinary Integration of Design Automation Research (CIDAR) group at Boston University develop computational and experimental tools for synthetic biology. 
  - https://www.bu.edu/eng/profile/douglas-densmore/
  - https://www.cidarlab.org/doug-densmore
- Recent Projects:
  - Bio-design automation: software + biology + robots
  - Genetic circuit design automation
  - Fluigi: Microfluidic Device Synthesis for Synthetic Biology
  - Improving engineered biological systems with electronics and microfluidics
- Summarized Topics:
  - Synthetic biology
  - Microfluidics
  - Cyber-Physical systems

# Phase 1 Assignment: Build a Tutorial with an LLM

(Below generated with Claude Sonnet 5 and edited)
## Project type
**Ultra-low-power embedded/IoT systems project** — this sits squarely in one of the four buckets the assignment names outright ("ultra-low-power embedded/IoT devices"), not in FPGA/architecture or hardware-security. More precisely: it's a **firmware-and-hardware-integration project in energy-aware embedded control**, combining three things that are each individually well-scoped for 12 weeks: (1) a power subsystem (harvester + supercap + low-power MCU) that determines how much energy is available at any moment; (2) a sensing/actuation loop (moisture/light/level sensing driving a water or nutrient dosing actuator) that would normally run on whatever schedule you choose; and (3) a decision policy that has to reconcile the two — deciding whether a given sense/decide/act cycle is worth spending energy on right now. There's no reconfigurable-logic (FPGA) component and no custom silicon; the "architecture" contribution is in the energy-aware scheduling and control-loop design, not in digital hardware design. This also matches your own conclusion — dropping the earlier optional FPGA extension is the right call for a 12-week single-person build with a live-organism dependency, which is already a real timeline risk on its own.

## Keywords
**Domain / application:** closed-loop plant-care automation, precision agriculture, automated plant phenotyping, self-driving lab, biological design automation, cyber-physical biological systems, hybrid electronic–biological systems
**Core technical:** energy harvesting, ultra-low-power embedded systems, adaptive duty cycling, energy-aware scheduling, closed-loop control, threshold (bang-bang) control, supercapacitor energy buffering, sensor fusion, actuation control
**Intermittent computing** — this is the actual name of the CS/embedded-systems subfield studying computation that has to checkpoint, pause, and resume as harvested power comes and goes, which is close to the exact technical core of your project even though your "computation" is a lightweight sense-decide-act loop rather than general-purpose code. There's a real survey literature here (e.g., ["A survey of techniques for intermittent computing"](https://www.sciencedirect.com/science/article/abs/pii/S1383762120301430)) that's worth at least skimming — it's the closest existing body of work to your project's central tension, even though none of it is plant- or biology-related. Worth adding to your keyword list for future searches even if you don't cite it heavily in Phase 1.

## Research articles ranked by relevance to *this* project
Ranked for the embedded, no-FPGA, energy-constrained plant-care version specifically — not a general "how good is this paper" ranking.

1. **[Densmore & Yazicigil et al., "Improving engineered biological systems with electronics and microfluidics," Nature Biotechnology 2025](https://www.nature.com/articles/s41587-025-02709-6).** Still the anchor. Defines your system category, co-authored by both target professors, and its own text hands you the gap statement (closed-loop bio-electronic systems, energy constraints unaddressed).
2. **["Towards Mass-Scale IoT with Energy-Autonomous LoRaWAN Sensor Nodes," PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC11244049/).** Not faculty-authored, but the closest *technical* precedent to what you're actually building — an energy-harvested node making its own operating decisions under a real power budget. This moved up because your project is now defined as embedded-only, and this is the paper closest to your literal architecture.
3. **[Liu, Mendoza, Yasar, Caygara, Kassem, Densmore & Yazicigil, "Integrated Real-Time CMOS Luminescence Sensing and Impedance Spectroscopy in Droplet Microfluidics," IEEE TBioCAS 2024](https://pmc.ncbi.nlm.nih.gov/articles/PMC11875993/).** Joint paper, proves this professor pair builds real sensing hardware together — strong credibility citation, but the application (droplet biosensor screening) doesn't hand you engineering detail you'll actually reuse.
4. **["The Duckbot: A system for automated imaging and manipulation of duckweed," PLOS ONE](https://pmc.ncbi.nlm.nih.gov/articles/PMC10805289/).** Directly relevant if you go with duckweed — defines the "what exists, why isn't it enough" story for the plant side (imaging only, no actuation, not energy-constrained).
5. **["Plant Microbial Fuel Cells–Based Energy Harvester System for Self-Powered IoT Applications," PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC6470559/).** Relevant specifically if you consider a PMFC harvester; concrete precedent for that power-subsystem choice.
6. **[Way, Collins, Keasling & Silver, "Integrating Biological Redesign," Cell 2014](https://www.cell.com/cell/fulltext/S0092-8674(14)00280-3).** High value for your motivation/framing section (specification, abstraction, standardization as the field's founding move), low value as engineering precedent — it doesn't touch hardware or energy at all.
7. **["SIPEREA: A Scalable Imaging Platform for Measuring Two-Dimensional Growth of Duckweed"](https://doi.org/10.3390/app16010066).** Largely redundant with Duckbot for your purposes; keep as a secondary citation if you want more depth on growth-quantification method specifically.
8. **["Autonomous closed-loop mechanistic investigation of molecular electrochemistry via automation," Nature Communications 2024](https://www.nature.com/articles/s41467-024-47210-x).** CIDAR-adjacent, good for the general "closed-loop decide-next-step" methodology, but it's chemistry-automation software, not embedded hardware — lower priority now that FPGA/architecture ambitions are off the table and the project is scoped tightly to embedded control.
9. **[Lashkaripour et al., "Design automation of microfluidic single and double emulsion droplets with machine learning," Nature Communications 2024](https://www.cidarlab.org/publications).** Same category as #8, one step further from your actual build (ML-driven physical-device design, not real-time control).
10. **["Batteryless Soil EIS Sensor Powered by Microbial Fuel Cell"](https://link.springer.com/chapter/10.1007/978-3-031-26066-7_43).** Narrow — relevant only if you commit to both soil-based sensing and MFC power, which is less likely now that duckweed (no soil) is the leading plant candidate.

*Not ranked as an academic source:* the ["Energy-Harvesting IoT reaching practical scale in 2026" trade article](https://iotbusinessnews.com/2025/11/26/energy-harvesting-iot-practical-applications-finally-reaching-scale-in-2026/) is trade press, not peer-reviewed — fine as one line of "why now" market context, not something to cite as a research reference in the formal literature review.

## Candidate variables to test (pick one as your semester focus)
### Recommended single focus: energy-budget vs. care-policy quality
This is the project's actual novel contribution, so it should be the thing you vary and measure, not a side detail. **Independent variable:** average available power / energy-bank state (run the same care policy under a few fixed regimes — e.g., unconstrained/wall-power baseline, a moderate harvested budget, and a tight harvested budget, ideally plus one run on live harvested power once the fixed-regime results validate the approach). **Dependent variables:** (a) a plant-relevant outcome — for duckweed, frond coverage/growth rate over time, or simpler, "time spent outside a healthy water/nutrient range" as a proxy that doesn't require imaging; and (b) decision responsiveness — how long the system goes between an actual need (e.g., reservoir level drops) and the system acting on it, at each budget level. The single figure this produces — outcome quality vs. available power — is exactly the result that makes your combined 6+7 idea legible as one project rather than two bolted-together halves, and it's the cleanest "asymmetric cost" story for your write-up: acting too eagerly strands the reserve, conserving too hard lets a real need go unaddressed.

### Other candidate variables — hold these fixed this semester, note as future work
- **Sense/decide/act interval.** How coarse can the duty cycle be (check every few minutes vs. every few hours) before care quality degrades. Closely related to the recommended focus, but a distinct axis if you wanted it instead — pick one or the other, don't vary both at once in a 12-week build.
- **Actuation policy type.** Simple threshold/bang-bang dosing vs. a slightly smarter proportional or hysteresis-based controller. Worth implementing as a competent baseline, but treating the *choice* of controller as your research variable is a smaller, less novel project than the energy-budget question above.
- **Energy buffer size.** Supercap capacitance as a design parameter — bigger buffer smooths over gaps but adds cost/size/charge time. Good for a design-space discussion in your report; not worth a full parameter sweep as the main result.
- **Sensing modality.** For duckweed: a water-level float switch vs. a conductivity/EC probe vs. a cheap photodiode/turbidity-based coverage proxy. Pick one for the build (start with the simplest — level or conductivity — and treat imaging as a stretch goal, not a dependency), rather than comparing all three.
- **Harvester source.** Solar vs. plant microbial fuel cell. Solar is the lower-risk choice for a reliable, characterizable power source on a 12-week clock; PMFC is the thematically tighter but electrically messier option (lower, less predictable current). Pick one to build with — don't try to compare them this semester.
- **Growth/health quantification method.** Image-based coverage estimate vs. a simpler proxy sensor signal. This is a measurement-method decision, not really an experimental variable — pick the simplest one that gives you a believable outcome metric (a proxy sensor is far more 12-week-friendly than building an imaging pipeline on top of everything else).

The pattern across all of these: each one *could* be a project on its own, which is exactly why they should stay fixed, simple design choices this semester rather than additional axes you're testing — the energy-budget-vs-outcome tradeoff is the one variable ambitious enough to be a real contribution and narrow enough to actually finish.

## Current Idea (Generated with Claude and edited):
Energy-constrained closed-loop plant-care automation
Draws on research from Professor Rabia and Profesor Densmore
- Professor Densmore's research page lists platform-based embedded-systems design as a core area distinct from synthetic biology
- Professor Rabia's research page has a project explicitly framed around energy-constrained systems

Problem: Closed-loop experimentation systems normally assume you can sense and compute whenever you want — the self-driving-lab literature this draws from runs on wall power. Energy-harvested sensor nodes, on the other hand, are almost always built for passive sensing, not for making decisions and driving actuators. Running the decision loop itself — not just the sensors — inside a tight, unpredictable harvested-energy budget is where those two don't automatically compose: every sense/decide/actuate cycle costs energy you might not have yet, and every cycle you skip to conserve energy is a cycle where the experiment doesn't advance.

Why it matters. This is a genuine gap: automated plant-phenotyping rigs (e.g., the precision-greenhouse and robotic-phenotyping systems in the literature) assume continuous power and often continuous human oversight; energy-harvested ag/soil sensor nodes assume the compute side stays essentially idle. A node that has to budget its own experimentation is a different, harder problem than either.

12-week build. Scope this to one decision variable, not several, or it will sprawl — misting/watering schedule for your moss (or a second plant chamber run in parallel as a control) is the natural choice given your existing rig. Hardware: a solar (simplest, most predictable) or PMFC (thematically tighter, riskier) harvester feeding a supercap through a BQ25570-class harvesting IC, powering a low-power MCU that reads humidity/soil-moisture and drives the misting actuator you already have. Firmware, in two layers: (1) an energy-aware scheduler that only allows a sense/decide/actuate cycle when the energy bank is above a threshold, otherwise sleeps; (2) inside each allowed cycle, a simple closed-loop policy (start with basic threshold or bang-bang control before anything fancier — e.g., a small multi-armed-bandit-style comparison across two watering schedules run in parallel chambers is a good stretch goal, not the baseline) that decides whether to mist and logs the outcome. Evaluate on two axes together, which is the actual contribution: how much the closed-loop policy improves outcomes over a fixed schedule (Densmore-style automation result), and how much decision quality/responsiveness degrades as the energy budget tightens, compared against the same policy run on wall power (Yazicigil-style energy-constrained result). A plot of "policy performance vs. average harvested power available" is your single best result — it's the one figure that makes the combination legible as one idea rather than two.

User / decision / cost framing. The user is you (or whoever's relying on the system to find a good care policy unattended). The embedded controller's decision, every cycle, is "do I have enough banked energy to act on this reading now, or do I conserve and wait?" Acting too readily drains the energy reserve and can strand the node in a low-power state right when a decision matters most; conserving too aggressively means a real need (the moss drying out) goes unaddressed during the wait. That's a genuine asymmetric cost you can characterize experimentally rather than assert — run the same care policy at a few different energy-availability levels and show where outcomes start to degrade.
