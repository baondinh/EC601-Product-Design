# Energy-Constrained Closed-Loop Plant-Care Automation: A Tutorial and Comprehension Guide

## Abstract

Many systems that must sense and care for plants over long periods such as a distributed agricultural sensor network, a remote environmental monitoring station, or an unattended long-duration biological experiment, cannot rely on a wall outlet or a single person to periodically swap batteries. At the same time, a growing body of research has explored automating the *decisions* involved in caring for or experimenting on living systems, largely under the assumption of continuous, uninterrupted power. This tutorial introduces the ideas behind a small embedded system that closes that gap: a self-powered controller that senses conditions around a fast-growing plant, decides whether action (such as watering or dosing nutrients) is needed, and acts while running on harvested energy rather than a plug or a disposable battery. The central question explored is how the quality of that automated care changes as the available energy budget shrinks. This tutorial surveys the research areas that inform the system: engineering approaches to living systems, hybrid electronic-biological devices, real-time biological sensing hardware, automated experimentation, automated plant monitoring, energy harvesting, and computing under interrupted power.

## Background and Need

**Who this is for?** The underlying problem shows up anywhere a living system needs ongoing care or monitoring in a location without reliable power or constant human attention: researchers running biological experiments that need to continue unattended for days or weeks; agricultural operators managing crops spread across large or remote areas where wiring every sensor isn't practical; environmental monitoring programs tracking conditions in hard-to-reach locations; and, at a much smaller scale, anyone trying to keep a garden, terrarium, or hydroponic setup alive while away from it.

**The current gap.** Two separate lines of research have matured in recent years without fully meeting in the middle. One line focuses on *decision-making automation* for the care or experimention of living systems (letting a system observe defined markers and decide its next action within the system without interference in the loop). This research generally assumes the system has continuous access to power. The second line focuses on *energy-harvested sensing* (devices that run indefinitely off small amounts of ambient energy such sunlight, or even the electrochemical activity of soil and plant roots) instead of a wall outlet or battery that needs replacing. This research has generally focused on passive sensing and reporting, not on making decisions or taking physical action. A system that has to make ongoing care decisions *and* operate within a real, fluctuating energy budget sits in the gap between these two bodies of work.

## What This Project Aims to Solve

This project develops and studies a small, self-powered embedded system that automates the care of a fast-growing plant (duckweed or moss are strong candidates, given how quickly they grow and how simply they can be watered). The system pairs an energy harvester (such as a small solar cell) and a modest energy buffer with a low-power controller, a small set of sensors (for example, water or nutrient level), and an actuator capable of dosing water or nutrients. Unlike a device that is always powered, this system has to repeatedly ask itself a harder question: not just "does the plant need something," but "can I currently afford to check, decide, and act on that right now, or should I wait?"

The specific question this work sets out to characterize is the **energy-budget versus care-quality tradeoff**: as the amount of available harvested energy shrinks, how does the quality of automated care (can be measured by how well plant needs are met and how quickly the system responds to a real need) change? Acting too freely can drain the energy reserve and leave the system unable to respond exactly when it matters most; being too conservative can let a real need go unaddressed for too long. Characterizing where that balance breaks down, under a real and limited energy budget, is the core contribution this work is built around.

## Topics Presented by the Relevant Research

### A. Engineering principles for redesigning living systems

For much of the history of biology, understanding a living system meant studying something that already existed. A shift over the last two decades has been to design living systems deliberately, the way an engineer designs a bridge or a circuit. Four ideas made this possible: designing to a specification (deciding exactly what a system should do before building it, like a blueprint); separating design from building (testing a plan before physically constructing it); using standardized, interchangeable parts instead of custom-building everything from scratch; and abstraction — being able to use a building block without needing to understand every detail of how it works internally. These principles, articulated by Way, Collins, Keasling, and Silver (2014), underlie the broader idea that a living system's care and behavior can be treated as something to be engineered and automated, rather than left to manual observation and intervention.

### B. Hybrid electronic-biological systems

Once living systems can be engineered deliberately, a natural next step is connecting them to electronics that sense what's happening and respond automatically, in real time, without waiting for a person to notice. Bali, Caygara, Densmore, and Yazicigil (2025) survey this category of hybrid system — devices that sense, act on, and report on biological environments for uses ranging from health monitoring to agriculture. Building these systems is harder than it sounds: living systems are wet, messy, and slow-changing, while electronics are precise and fast, and the two don't naturally get along (electrical noise, fouling, and general interference are real engineering challenges). Because these systems are increasingly connected and remotely monitored, keeping them secure from tampering is also a live concern. Notably, this survey identifies real-time, closed-loop hybrid systems as an important and growing category, but does not address what happens when such a system also has to operate under a constrained, harvested energy supply — which is precisely the open question this work addresses.

### C. Real-time sensing hardware for biological monitoring

A closely related idea is building custom sensing hardware that can watch a biological or chemical process directly and report back instantly, rather than requiring a full laboratory setup and a technician. Liu, Arguijo Mendoza, Yasar, Caygara, Kassem, Densmore, and Yazicigil (2024) describe a custom sensor chip that reads both the electrical and optical properties of tiny liquid droplets in real time, allowing fast, automated characterization of what's happening inside each droplet. This is a concrete example of purpose-built sensing hardware supporting a biological system in real time — the same general capability a plant-care system needs, applied here to a different measurement problem.

### D. Automated, closed-loop decision-making in experimentation

Traditionally, a person runs an experiment or check, looks at the result, and decides what to do next. That loop can be automated: a system that observes an outcome and immediately decides its next action, without a person choosing each step, can run through far more iterations than a person could manage and often converge on a good outcome largely on its own. Sheng and colleagues (2024) describe such a system for investigating chemical reaction mechanisms — deciding its own next experiment based on each prior result. A related idea uses machine learning to let software design the physical device itself rather than a person hand-designing every version — Lashkaripour and colleagues (2024) demonstrate this for designing small fluid-handling devices. Together, these represent the "decide, don't just observe" logic that a self-caring plant system also needs: not just sensing a condition, but deciding what to do about it and adjusting future behavior based on what's worked.

### E. Automated monitoring of fast-growing plants

Some of the fastest-growing plants used in research are small, floating species called duckweed, which can double in size every couple of days — useful for observing growth changes quickly. Subbaraman, de Lange, Ferguson, and Peek (2024) built a camera-based system, nicknamed "the Duckbot," that automatically photographs and measures duckweed growth over time rather than requiring manual measurement. This kind of system automates *observation* of a fast-growing plant, but does not sense broader environmental conditions or take physical action (such as watering) in response — it watches, but it doesn't care for the plant, which is the piece a fully autonomous system still needs to add.

### F. Powering devices without batteries

Most electronics need either a wall outlet or a battery that eventually has to be replaced. For a device that has to run for a long time in a location that's inconvenient or impossible to visit regularly — spread across a large area, for instance — neither option scales well. Energy harvesting offers an alternative: pulling small amounts of usable electricity from something already present, such as sunlight through a small solar cell. La Rosa, Boulebnane, Pagano, Giuliano, and Croce (2024) describe how energy-harvested sensor nodes can be deployed at large scale specifically to avoid the ongoing labor cost of visiting and replacing batteries across many devices. A more unusual harvesting source comes from living systems themselves: Osorio de la Rosa and colleagues (2019) describe harvesting small amounts of electricity from the natural electrochemical activity of microbes in soil and around plant roots — meaning a plant-adjacent system could, in principle, draw a small amount of power from the very environment it's caring for.

### G. Computing under interrupted power

Harvested power is not steady — sunlight fades with clouds, and other harvested sources can fluctuate or drop out unpredictably. This creates a real computing problem: if a device loses power mid-task, does it lose all of its progress and have to start over every time? Umesh and Mittal (2021) survey techniques for what is called intermittent computing — ways for a device to save its state just before losing power and pick back up smoothly afterward, similar to how a computer resumes from sleep, except here the interruption can happen often and unpredictably, and the device has to handle it gracefully every time. This is close to the core operating condition a harvested-power, decision-making system has to be designed around, even though the research itself is not about plants or biology at all.

---

## Comprehension Quiz

### Part 1 — Background, Need, and Aim

**1.** Who would find the underlying problem addressed here most relevant?
A) Someone with unlimited access to wall power and constant free time to check on a living system
B) Someone who needs a living system cared for or monitored in a place without reliable power or constant supervision
C) Someone who only cares about indoor, always-plugged-in devices
D) Someone with no interest in automation

**2.** What is the gap between the two existing lines of research described in the Background section?
A) One is about plants and the other is about animals
B) One focuses on automated decision-making assuming continuous power; the other focuses on energy-harvested passive sensing, not decision-making or action
C) Both already fully solve the same problem, so there is no gap
D) One uses electricity and the other does not use any power source at all

**3.** What is the central question this work sets out to characterize?
A) Which plant species tastes best
B) How the quality of automated care changes as the available energy budget shrinks
C) How to make a device as expensive as possible
D) Whether electronics can ever be waterproof

**4.** What happens if the system described acts too freely, checking and responding constantly?
A) Nothing negative — more action is always better
B) It can drain its energy reserve and be left unable to respond exactly when a real need arises
C) It becomes more accurate over time
D) It automatically switches to battery power

**5.** What happens if the system is too conservative, checking rarely to save energy?
A) It becomes more responsive
B) A real need can go unaddressed for too long before the system notices and acts
C) It uses more energy than if it checked constantly
D) It stops sensing entirely

### Part 2 — Research Topics

**6.** According to the engineering-principles idea (Topic A), what does "separation of design from fabrication" mean?
A) Designers and builders must be different people
B) A plan can be tested before it is physically built
C) Design and construction must happen in the same room
D) It refers to separating two different chemical compounds

**7.** Why is it difficult to build hybrid systems that combine living biology with electronics (Topic B)?
A) Biology and electronics use incompatible units of measurement
B) Living systems are wet, messy, and slow-changing while electronics are precise and fast, and connected devices raise new tampering concerns
C) Electronics cannot operate near water under any circumstances
D) It is not actually difficult

**8.** What does the custom sensor chip described in Topic C measure on a tiny liquid droplet?
A) Only its temperature
B) Its electrical and optical (light) properties, in real time
C) Its weight alone
D) Nothing — it only stores droplets

**9.** In the closed-loop, automated decision-making idea (Topic D), what makes a process "closed-loop"?
A) The system repeats the same fixed action regardless of results
B) The system observes an outcome and decides its own next step, without a person choosing each one
C) The loop refers to a physical circular device shape
D) It means the system never produces any output

**10.** What is the key limitation of the camera-based duckweed monitoring system described in Topic E?
A) It cannot take any pictures at all
B) It automates observation and measurement of growth, but does not sense broader conditions or take action like watering
C) It requires a full-time human operator to run
D) It only works on plants other than duckweed

**11.** What is "energy harvesting," as described in Topic F?
A) Manually collecting used batteries for recycling
B) Pulling small amounts of usable electricity from ambient sources already present, such as sunlight or microbial activity, instead of a plug or battery
C) A method for storing energy in large centralized power plants
D) Generating electricity exclusively through nuclear reactions

**12.** What real-world problem does deploying energy-harvested sensor nodes at scale solve, per Topic F?
A) It makes sensors more expensive to manufacture
B) It avoids the ongoing labor cost of visiting and replacing batteries across many devices spread over a large area
C) It eliminates the need for any sensing at all
D) It increases Wi-Fi signal range

**13.** What problem does "intermittent computing" (Topic G) address?
A) How to make a device run faster when it always has stable power
B) How a device can save its progress before an unpredictable power loss and resume smoothly afterward, instead of restarting from scratch
C) How to eliminate the need for a power source entirely
D) How to connect multiple unrelated devices into a network

---

### Answer Key

1. **B** — Someone needing a living system cared for without reliable power or constant supervision.
2. **B** — Automated decision-making (assumes continuous power) vs. energy-harvested passive sensing (no decision-making).
3. **B** — How care quality changes as the available energy budget shrinks.
4. **B** — Draining the reserve can leave the system powerless when it matters most.
5. **B** — A real need can go unaddressed for too long.
6. **B** — A plan can be tested before physical construction.
7. **B** — Mismatched physical properties, plus tampering/security concerns from connectivity.
8. **B** — Electrical and optical properties, in real time.
9. **B** — The system decides its own next step from an observed outcome.
10. **B** — It observes and measures only; it doesn't sense conditions or act on them.
11. **B** — Pulling usable electricity from ambient sources already present.
12. **B** — Avoiding the ongoing labor cost of battery replacement at scale.
13. **B** — Saving progress before power loss and resuming smoothly afterward.

---

## References

Bali, A., Caygara, D., Densmore, D., & Yazicigil, R. T. (2025). Improving engineered biological systems with electronics and microfluidics. *Nature Biotechnology*, *43*(7). https://www.nature.com/articles/s41587-025-02709-6

La Rosa, R., Boulebnane, L., Pagano, A., Giuliano, F., & Croce, D. (2024). Towards mass-scale IoT with energy-autonomous LoRaWAN sensor nodes. *Sensors*, *24*(13), 4279. https://doi.org/10.3390/s24134279

Lashkaripour, A., McIntyre, D. P., Calhoun, S. G. K., Krauth, K., Densmore, D. M., & Fordyce, P. M. (2024). Design automation of microfluidic single and double emulsion droplets with machine learning. *Nature Communications*, *15*, 83. https://www.nature.com/articles/s41467-023-44068-3

Liu, Q., Arguijo Mendoza, D., Yasar, A., Caygara, D., Kassem, A., Densmore, D., & Yazicigil, R. T. (2024). Integrated real-time CMOS luminescence sensing and impedance spectroscopy in droplet microfluidics. *IEEE Transactions on Biomedical Circuits and Systems*. https://pmc.ncbi.nlm.nih.gov/articles/PMC11875993/

Osorio de la Rosa, E., Vázquez Castillo, J., Carmona Campos, M., Barbosa Pool, G. R., Becerra Nuñez, G., Castillo Atoche, A., & Ortegón Aguilar, J. (2019). Plant microbial fuel cells–based energy harvester system for self-powered IoT applications. *Sensors*, *19*(6), 1378. https://doi.org/10.3390/s19061378

Sheng, H., Sun, J., Rodríguez, O., Hoar, B. B., Zhang, W., Xiang, D., Tang, T., Hazra, A., Min, D. S., Doyle, A. G., Sigman, M. S., Costentin, C., Gu, Q., Rodríguez-López, J., & Liu, C. (2024). Autonomous closed-loop mechanistic investigation of molecular electrochemistry via automation. *Nature Communications*, *15*, 2781. https://www.nature.com/articles/s41467-024-47210-x

Subbaraman, B., de Lange, O., Ferguson, S., & Peek, N. (2024). The Duckbot: A system for automated imaging and manipulation of duckweed. *PLOS ONE*, *19*(1), e0296717. https://doi.org/10.1371/journal.pone.0296717

Umesh, S., & Mittal, S. (2021). A survey of techniques for intermittent computing. *Journal of Systems Architecture*, *112*, 101859. https://doi.org/10.1016/j.sysarc.2020.101859

Way, J. C., Collins, J. J., Keasling, J. D., & Silver, P. A. (2014). Integrating biological redesign: Where synthetic biology came from and where it needs to go. *Cell*, *157*(1), 151–161. https://www.cell.com/cell/fulltext/S0092-8674(14)00280-3
