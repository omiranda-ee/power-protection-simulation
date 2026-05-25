# Power Distribution & Protection Simulation

**MATLAB/Simulink | Power Systems | Protection Engineering**

A single-phase power distribution and protection simulation built as part of a summer 2026 engineering portfolio. The system models real-world utility protection behavior — detecting a short-circuit fault and isolating it automatically using relay logic and a circuit breaker.

---

## About

Built by **Orlando Miranda** — graduating May 2027 with a B.S. in Electrical and Computer Engineering (Microelectronics concentration) from [University Name]. This project targets utility/power engineering and demonstrates hands-on proficiency with power system simulation, fault analysis, and protection coordination.

---

## System Overview

The simulation models a single-phase distribution system consisting of:

- **AC Voltage Source** — 220V RMS, 60 Hz
- **Step-Down Transformer** — 220V / 110V, 10 kVA
- **Two Parallel Resistive Loads** — 10Ω each (5Ω equivalent)
- **Short-Circuit Fault** — 0.001Ω fault resistance triggered at t = 0.05s
- **Overcurrent Relay** — trips when feeder current exceeds 50A
- **Circuit Breaker** — externally controlled, opens on relay trip and latches open

Normal feeder current: **~22A RMS (31A peak)**
Fault current: **~1500A peak** (limited by transformer leakage impedance)
Relay response: **< 1 cycle (< 16.7ms)**

---

## Protection Logic

```
Feeder Current → |Abs| → Relay (trips at 50A) → Breaker Control Input
```

The relay uses a latching design: once the overcurrent threshold is exceeded, the breaker opens and remains open for the duration of the simulation — mirroring the behavior of a real utility protection relay that requires manual reset after a trip event.

**Key design decision:** The relay's switch-off point is set to −1. Because the Abs block guarantees a non-negative input, the relay can never reset once tripped — implementing a software latch without additional logic blocks.

---

## Simulation Results

### Normal Operation (0 – 0.05s)
Clean sinusoidal waveforms at rated voltage and current.

| Parameter | Value |
|-----------|-------|
| Secondary voltage | 110V RMS / 155V peak |
| Feeder current | 22A RMS / 31A peak |
| Frequency | 60 Hz |
| Power factor | 1.0 (resistive load) |

![Normal Operation](docs/normal_operation.pdf)

---

### Fault Event (t = 0.05s)
Short-circuit fault applied across Load 2. Current spikes to ~1500A peak and secondary voltage collapses.

![Fault Event](docs/fault_event.pdf)

---

### Relay Trip and Isolation
Overcurrent relay detects the fault within one cycle, trips the breaker, and latches open. Feeder current drops to zero and remains there.

![Relay Trip](docs/relay_trip.pdf)

---

## Repository Structure

```
power-protection-simulation/
│
├── README.md
├── /simulation
│     └── power_protection_sim.slx       ← Simulink model
├── /docs
│     ├── normal_operation.pdf           ← Scope output, pre-fault
│     ├── fault_event.pdf                ← Scope output, fault at t=0.05s
│     └── relay_trip.pdf                 ← Relay latch and voltage isolation
└── /hardware
      └── (schematics and wiring diagrams — future)
```

---

## How to Run

**Requirements:**
- MATLAB R2022a or later
- Simulink
- Simscape Electrical (Specialized Power Systems toolbox)

**Steps:**
1. Clone or download this repository
2. Open `simulation/power_protection_sim.slx` in MATLAB
3. Set simulation Stop Time to `0.1` seconds
4. Click Run
5. Open the Scope blocks to view waveforms

---

## Skills Demonstrated

- Power system modeling in MATLAB/Simulink
- Transformer and load configuration
- Short-circuit fault simulation
- Overcurrent relay logic and protection coordination
- Latching relay design without additional logic blocks
- Scope-based waveform analysis and result documentation

---

## Portfolio Context

This is **Project 2 of 4** in a summer 2026 engineering portfolio targeting both **Utility/Power Engineering** and **Defense/Embedded Systems** career paths ahead of graduation in May 2027.

| Project | Focus |
|---------|-------|
| 1 — Smart Power Monitor | Embedded instrumentation, ESP32, real-time measurement |
| **2 — Power Protection Simulation** | **Power systems, fault analysis, relay coordination** |
| 3 — Multi-Sensor Security System | Defense, embedded firmware, wireless comms |
| 4 — Embedded Test & Diagnostics Platform | Validation engineering, test automation |

---

*Orlando Miranda · Electrical and Computer Engineering · May 2027*
