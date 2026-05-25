# Power Distribution & Protection Simulation

**MATLAB/Simulink · Power Systems · Protection Engineering**

A single-phase power distribution and protection simulation that models fault detection and automatic isolation using overcurrent relay logic and a circuit breaker.

---

## About

**Orlando Miranda**
B.S. Electrical and Computer Engineering, Microelectronics Concentration
New Mexico State University · Graduating May 2027

---

## System Overview

The simulation models a single-phase distribution system with two parallel loads, a step-down transformer, a triggered short-circuit fault, and an overcurrent protection relay that automatically isolates the fault.

| Component | Specification |
|-----------|--------------|
| AC Voltage Source | 220V RMS, 60 Hz |
| Step-Down Transformer | 220V / 110V, 10 kVA |
| Load 1 | 10Ω resistive |
| Load 2 | 10Ω resistive |
| Fault Resistance | 0.001Ω (near short circuit) |
| Fault Trigger Time | t = 0.05s |
| Relay Trip Threshold | 50A |
| Breaker Response | < 1 cycle (< 16.7ms) |

**Normal operating conditions:**
- Secondary voltage: 110V RMS / 155V peak
- Feeder current: 22A RMS / 31A peak
- Power factor: 1.0 (resistive load)

**Under fault:**
- Fault current: ~1500A peak (limited by transformer leakage impedance)

---

## Protection Logic

```
Feeder Current → |Abs| → Relay (trips at 50A) → Breaker Control Input
```

The relay monitors the absolute value of feeder current. When it exceeds 50A, it sends a trip signal to open the breaker. The breaker then latches open and remains isolated for the rest of the simulation.

**Design note:** The relay's switch-off point is set to −1. Since the Abs block guarantees a non-negative input signal, the relay can never reset once tripped — implementing a latch without any additional logic blocks. This mirrors the behavior of a real overcurrent relay that requires manual reset after a trip event.

---

## Simulation Results

### 1 — Normal Operation (0 to 0.05s)

Clean sinusoidal voltage and current at rated conditions before the fault is applied.

![normal operation](/docs/normal_operation.png)

---

### 2 — Fault Event (t = 0.05s)

Short-circuit fault applied across Load 2. Feeder current spikes to ~1500A peak and secondary voltage collapses.

![fault event](/docs/fault_event.pdf)

---

### 3 — Relay Trip and Isolation

Overcurrent relay detects the fault within one cycle, trips the breaker, and latches open. Feeder current drops to zero and remains there for the rest of the simulation.

[View: relay_trip.pdf](docs/relay_trip.pdf)

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
│     └── relay_trip.pdf                 ← Relay trip and breaker isolation
└── /hardware
      └── (schematics — future)
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
3. Confirm Stop Time is set to `0.1` seconds
4. Click Run
5. Open the Scope blocks to view waveforms

---

## Skills Demonstrated

- Power system modeling in MATLAB/Simulink
- Transformer and load configuration
- Short-circuit fault simulation and analysis
- Overcurrent relay logic and protection coordination
- Latching relay design
- Waveform analysis and result documentation

---

## Other Projects

| Repository | Description |
|------------|-------------|
| [smart-power-monitor](https://github.com/omiranda-ee/smart-power-monitor) | Real-time voltage, current, and power factor monitoring on ESP32 with web dashboard |
| [multisensor-security-system](https://github.com/omiranda-ee/multisensor-security-system) | Multi-sensor embedded security system with PIR, radar, ultrasonic detection, and wireless alerts |
| [embedded-diagnostics-platform](https://github.com/omiranda-ee/embedded-diagnostics-platform) | Hardware/software platform for automated sensor validation and diagnostics |

---

*Orlando Miranda · New Mexico State University · ECE · May 2027*
