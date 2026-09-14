# Tutorial: Energy-Constrained Closed-Loop Plant-Care Automation

A walkthrough of the project for classmates who haven't seen it before — what it is, what kind of project it is, what the semester is actually testing, and why any of this matters outside a dorm-room growth chamber.

---

## 1. What is this project?

In plain terms: it's a small, self-powered embedded controller that takes care of a fast-growing plant (duckweed is the leading candidate — it doubles its biomass every 1–4 days) without a wall outlet and without a person checking on it. The controller senses conditions in the plant's enclosure (things like water level or nutrient concentration), decides whether the plant needs something right now, and if so, acts — running a small pump or valve to add water or nutrients. That sense → decide → act cycle repeats on its own for the life of the project.

What makes it more than "an automatic plant waterer" is where the power comes from. The controller isn't plugged in — it runs off energy that's harvested on-site (solar is the leading choice) and stored in a small buffer (a supercapacitor) rather than a battery you'd have to swap. That means every single sense/decide/act cycle costs real, limited energy, and the controller has to decide not just *what* the plant needs, but *whether it can currently afford* to check and respond to that need.

## 2. Project type

This is an **ultra-low-power embedded/IoT systems project** — one of the standard categories for hardware/embedded coursework, alongside things like FPGA prototyping or RISC-V experiments. It is *not* an FPGA or computer-architecture project (that direction was explored and deliberately dropped — not realistic for one person to build and validate against a living organism in 12 weeks), and it's not primarily a circuits/analog-design project either, even though it involves a small power-management front end.

Concretely, it's three well-understood embedded-systems pieces combined:

- **A power subsystem** — a harvester, an energy buffer, and a low-power microcontroller, which together determine how much energy is available at any given moment.
- **A sensing/actuation loop** — sensors that read the plant's environment, and an actuator (pump/valve) that can change it.
- **A decision policy** — the logic that decides, every cycle, whether to act, and that has to account for energy availability, not just what the sensors say.

## 3. The semester focus: energy-budget vs. care-quality tradeoff

Here's the tension the whole project is built around: a closed-loop system that always has power can check on the plant as often as it wants and act the instant something needs attention. A system that only has a little harvested energy trickling in has to be more careful — every time it wakes up to sense, decide, or actuate, it spends down its energy reserve, and if it spends too freely, it can end up with *no* energy left exactly when the plant needs something.

So the actual experiment isn't just "build a plant-watering robot." It's: **how does the quality of care the system provides change as you shrink its energy budget?**

To test that, the plan is to run the *same* care logic under a few different fixed power conditions — for example: unlimited power (a baseline, as if it were plugged in), a moderate harvested budget, and a tight harvested budget — and measure two things at each level:

- **How well the plant is actually cared for** (did the water/nutrient level stay in a healthy range, or did the plant go without for too long?)
- **How responsive the system was** (how much time passed between "the plant actually needed something" and "the system did something about it")

The expectation is that as the energy budget tightens, responsiveness gets worse, and at some point care quality starts to suffer too — and the goal of the project is to find and characterize where that breakdown happens, not just to observe that it does.

### Why this is a real engineering tradeoff, not just a fun constraint

Acting too eagerly (checking and responding constantly) drains the energy reserve fast — the system can end up "broke" right when the plant needs it most. Being too conservative (checking rarely to save energy) means a real problem — the water running low — can go unnoticed and unaddressed for a long stretch. Neither extreme is right; the interesting engineering work is in the policy that balances the two, and in measuring exactly how that balance shifts as the available energy shrinks.

## 4. What this looks like in the real world: farming

This exact tradeoff already shows up at real scale in agriculture, which is why it's worth more than a class demo.

- **Large or remote farms can't wire every sensor to power.** A field spanning many acres, or a remote grazing area, doesn't have outlets scattered through it. Soil-moisture and irrigation-control sensors deployed across a large area are increasingly solar-powered and communicate over long-range, low-power radio (this is the same category of system as the energy-autonomous LoRaWAN sensor nodes referenced in the project's literature review) specifically *because* running power and data cable to every point isn't practical.
- **Battery swaps don't scale.** A farm with hundreds of sensor nodes can't send someone out every few weeks to change batteries — that's a real, ongoing labor cost. Energy-harvested nodes that never need a battery change are the reason this category of device exists at all.
- **The cost of getting the decision wrong is asymmetric, just like in this project.** Under-watering a crop at the wrong moment can mean real yield loss; over-watering wastes water (a real cost, and in drought-prone regions, a resource-scarcity problem) and can promote root disease. A farm irrigation controller making decisions on a constrained power/communication budget faces exactly the same question this project's controller does: is this reading worth acting on right now, or can it wait?
- **Precision agriculture is explicitly moving this direction.** Industry coverage of the space frames energy-harvested, autonomous agricultural sensing as reaching practical commercial scale within the next couple of years — this project is a small, hands-on version of a real and current engineering trend, not a purely academic exercise.

The plant-scale version in this project is a controlled, fast-iterating way to study the same question a much larger irrigation network has to answer: how much can you shrink the energy budget of an autonomous care system before the quality of care it provides breaks down — and what's the smartest way to spend a limited energy budget when you can't do everything?

---

## Quiz

Answer each question, then check the key at the end.

**1. What broad category of project is this (per the course's own project-area categories)?**
A) FPGA prototype
B) RISC-V architecture experiment
C) Ultra-low-power embedded/IoT device
D) Hardware security demo

**2. Why was an FPGA-based extension of this project dropped from the plan?**
A) FPGAs can't run control loops
B) It wasn't realistic to build and validate against a living organism in a single semester on top of everything else
C) FPGAs don't support sensors
D) The professors advised against FPGA work generally

**3. What are the three main subsystems this project combines?**
A) A camera, a speaker, and a battery
B) A power subsystem, a sensing/actuation loop, and a decision policy
C) A neural network, a radio, and a display
D) A solar panel, a Wi-Fi module, and a mobile app

**4. What is the single variable this semester's experiment is built around?**
A) Which plant species grows fastest
B) How care-policy quality changes as the available energy budget shrinks
C) Comparing solar power to wall power on cost alone
D) How many sensors can fit in one enclosure

**5. In the experiment design, what are the two things measured at each energy-budget level?**
A) Temperature and humidity only
B) Cost and weight of the hardware
C) Care quality (was the plant kept in a healthy range) and responsiveness (how long between a real need and the system acting on it)
D) Battery voltage and Wi-Fi signal strength

**6. Why is "act too eagerly" a real risk, not just a minor inefficiency?**
A) It makes the controller too loud
B) It can drain the energy reserve and leave the system with nothing left when the plant actually needs help
C) It uses too much memory on the microcontroller
D) It voids the warranty on the supercapacitor

**7. In the farming analogy, why do many real agricultural sensor deployments use energy harvesting instead of batteries?**
A) Batteries are illegal in most farming regions
B) Solar panels are required by agricultural regulation
C) Sending someone to swap batteries across hundreds of sensors at large/remote farm scale is a real, ongoing labor cost that energy harvesting avoids
D) Batteries don't work outdoors

**8. What is the "asymmetric cost" idea, applied to farm irrigation, that mirrors this project's core tradeoff?**
A) Under-watering and over-watering cost exactly the same, so timing doesn't matter
B) Under-watering can cause real yield loss, while over-watering wastes water and can promote disease — getting the decision wrong in either direction has a real, different cost
C) Irrigation controllers never make mistakes once installed
D) Cost only matters for the initial hardware purchase, not ongoing operation

---

### Answer key

1. **C** — Ultra-low-power embedded/IoT device, one of the course's own named project categories.
2. **B** — Not realistic to build and validate against a living organism in 12 weeks on top of the rest of the build.
3. **B** — Power subsystem, sensing/actuation loop, decision policy.
4. **B** — How care-policy quality changes as the available energy budget shrinks.
5. **C** — Care quality and responsiveness, measured at each budget level.
6. **B** — Draining the reserve can leave the system powerless exactly when it's needed most.
7. **C** — Battery-swap labor doesn't scale across large/remote sensor deployments.
8. **B** — Both directions of error have a real, distinct cost — which is exactly the asymmetric-cost framing the project measures for its own energy-budget decision.

