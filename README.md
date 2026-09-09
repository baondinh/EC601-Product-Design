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

## Papers
- Improving engineered biological systems with electronics and microfluidics
  - https://www.nature.com/articles/s41587-025-02709-6
- Integrating biological redesign: where synthetic biology came from and where it needs to go
  - https://www.cell.com/cell/fulltext/S0092-8674(14)00280-3?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS0092867414002803%3Fshowall%3Dtrue
## Current Idea (Generated with Claude and edited):
Energy-constrained closed-loop plant-care automation
Draws on research from Professor Rabia and Profesor Densmore
- Professor Densmore's research page lists platform-based embedded-systems design as a core area distinct from synthetic biology
- Professor Rabia's research page has a project explicitly framed around energy-constrained systems

Problem: Closed-loop experimentation systems normally assume you can sense and compute whenever you want — the self-driving-lab literature this draws from runs on wall power. Energy-harvested sensor nodes, on the other hand, are almost always built for passive sensing, not for making decisions and driving actuators. Running the decision loop itself — not just the sensors — inside a tight, unpredictable harvested-energy budget is where those two don't automatically compose: every sense/decide/actuate cycle costs energy you might not have yet, and every cycle you skip to conserve energy is a cycle where the experiment doesn't advance.

Why it matters. This is a genuine gap: automated plant-phenotyping rigs (e.g., the precision-greenhouse and robotic-phenotyping systems in the literature) assume continuous power and often continuous human oversight; energy-harvested ag/soil sensor nodes assume the compute side stays essentially idle. A node that has to budget its own experimentation is a different, harder problem than either.

12-week build. Scope this to one decision variable, not several, or it will sprawl — misting/watering schedule for your moss (or a second plant chamber run in parallel as a control) is the natural choice given your existing rig. Hardware: a solar (simplest, most predictable) or PMFC (thematically tighter, riskier) harvester feeding a supercap through a BQ25570-class harvesting IC, powering a low-power MCU that reads humidity/soil-moisture and drives the misting actuator you already have. Firmware, in two layers: (1) an energy-aware scheduler that only allows a sense/decide/actuate cycle when the energy bank is above a threshold, otherwise sleeps; (2) inside each allowed cycle, a simple closed-loop policy (start with basic threshold or bang-bang control before anything fancier — e.g., a small multi-armed-bandit-style comparison across two watering schedules run in parallel chambers is a good stretch goal, not the baseline) that decides whether to mist and logs the outcome. Evaluate on two axes together, which is the actual contribution: how much the closed-loop policy improves outcomes over a fixed schedule (Densmore-style automation result), and how much decision quality/responsiveness degrades as the energy budget tightens, compared against the same policy run on wall power (Yazicigil-style energy-constrained result). A plot of "policy performance vs. average harvested power available" is your single best result — it's the one figure that makes the combination legible as one idea rather than two.

User / decision / cost framing. The user is you (or whoever's relying on the system to find a good care policy unattended). The embedded controller's decision, every cycle, is "do I have enough banked energy to act on this reading now, or do I conserve and wait?" Acting too readily drains the energy reserve and can strand the node in a low-power state right when a decision matters most; conserving too aggressively means a real need (the moss drying out) goes unaddressed during the wait. That's a genuine asymmetric cost you can characterize experimentally rather than assert — run the same care policy at a few different energy-availability levels and show where outcomes start to degrade.
