# Technical Specifications — Avionics

### V-Tail Pusher UAV | Aerospace Society, BIT Mesra
*Compiled from: V-Tail Pusher UAV Design Proposal — Design Report (28 January 2026)*

---

## 1. Flight Controller

| Parameter | Specification |
|---|---|
| Board | Pixhawk 2.4.8 |
| Recommended flight stack | ArduPlane (see Automation README for firmware details) |

---

## 2. Motor

<p align="center">
  <img src="assets/photos/motor placement.jpeg" width="500">
</p>

<p align="center">
  <em>Figure 2.1 – Rear-mounted pusher motor configuration.</em>
</p>

| Parameter | Specification |
|---|---|
| Type | 5010 Brushless DC (BLDC) |
| KV rating | 750 rpm/V |
| Compatible battery | 2S–6S LiPo |
| Compatible ESC | 20–40 A |
| Max thrust capability | 1500 g (with 3S LiPo) |
| Shaft diameter | 4 mm |
| Compatible propeller diameter | 14–16 inch |
| Motor mass | 112 g |
| Typical current draw at full throttle (4S, 14.8V) | 12–14 A |

**Selection rationale:** Balance of static thrust and low acoustic signature (lower KV than 920 KV alternatives) for surveillance-type missions. The motor is optimized for 14×4.7 inch propellers while maintaining a lightweight rear-mounted pusher configuration.

---

## 3. Electronic Speed Controller (ESC)

| Parameter | Specification |
|---|---|
| Rating | 40 A |
| Thermal margin at nominal load | ~65–70% headroom above the 12–14 A nominal draw |
| Function | Converts DC battery power to 3-phase AC; interprets PWM/DShot signals |

---

## 4. Propeller

| Parameter | Specification |
|---|---|
| Diameter | 14 inch |
| Pitch | 4.7 inch |
| Material | Orange HD glass-fibre nylon |
| Blades | 2 |
| Rotation | CW and CCW variants available (1 CW + 1 CCW stocked) |
| Pitch-to-diameter ratio | ~0.336 (optimal range 0.3–0.4 for electric UAVs / low-speed flight) |
| Shaft interface | 7.6 mm (3, 4.5, 6 mm plastic reducers available) |
| Assembly length | 354 mm |
| Assembly mass | 60 g |

### Bench Thrust Data (3S Battery)

| Throttle | Thrust | Current Draw | Battery |
|---|---:|---:|---|
| 15% | 220 g | 1.21 A | 3S |
| 50% | 680 g | 7.25 A | 3S |
| 75% | 1228 g | 17.62 A | 3S |
| 100% | 1495 g | 25 A | 3S |

---

## 5. Battery Eliminator Circuit (BEC)

| Parameter | Specification |
|---|---|
| Type | UBEC (external, switched-mode) |
| Output | 5 V / 5 A |
| Efficiency | 85–90% |
| Purpose | Powers ruddervator servos and receiver independently of the ESC's internal BEC (~3 A rated, insufficient at ~3–4 A peak servo draw) |

---

## 6. Servo Actuators

| Parameter | Specification |
|---|---|
| Model | MG90S |
| Angular rotation range | 180° |
| Torque | 1.8 kg·cm @ 4.8 V — 2.2 kg·cm @ 6.6 V |
| Operating voltage | 4.8–6.6 VDC |
| Mass | 13 g each |
| Peak current draw | 0.9–1.0 A each |
| Function | Ruddervator actuation (combined pitch and yaw control) |

---

## 7. Known Open Items (Avionics-Relevant)

- Motor, ESC, and BEC margins were bench-tested only on **3S**. The propulsion system is designed for **4S operation** (motor rated up to **6S**), so additional validation is required before operational deployment.
- Flight controller integration and self-stabilization were not completed in the original Design Report (implemented separately in the Automation documentation).
- Payload systems (camera, telemetry, sensors) have not yet been integrated, and their power requirements are not included in the current electrical budget.
- Acoustic noise measurements have not yet been performed despite being one of the primary motor and propeller selection criteria.
