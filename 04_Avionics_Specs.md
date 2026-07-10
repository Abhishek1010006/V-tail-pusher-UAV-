# Technical Specifications — Avionics
### V-Tail Pusher UAV | Aerospace Society, BIT Mesra
*Compiled from: V-Tail Pusher UAV Design Proposal — Design Report (28 January 2026)*

---

## 1. Flight Controller

| Parameter | Specification |
|---|---|
| Board | Pixhawk 2.4.8 |
| Recommended flight stack | ArduPlane (see Automation README for firmware details) |

## 2. Motor

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

**Selection rationale:** balance of static thrust and low acoustic signature (lower KV vs. e.g. 920 KV alternatives) for surveillance-type missions; propeller range matches the chosen 14x4.7 propeller; light mass and small shaft/adapter footprint suit the pusher mount.

## 3. Electronic Speed Controller (ESC)

| Parameter | Specification |
|---|---|
| Rating | 40 A |
| Thermal margin at nominal load | ~65-70% headroom above the 12-14 A nominal draw |
| Function | Converts DC battery power to 3-phase AC; interprets PWM/Dshot signals |

## 4. Propeller

| Parameter | Specification |
|---|---|
| Diameter | 14 inch |
| Pitch | 4.7 inch |
| Material | Orange HD glass-fibre nylon |
| Blades | 2 |
| Rotation | CW and CCW variants available (1 CW + 1 CCW stocked) |
| Pitch-to-diameter ratio | ~0.336 (optimal range 0.3-0.4 for electric UAVs / low-speed flight) |
| Shaft interface | 7.6 mm (3, 4.5, 6 mm plastic reducers available) |
| Assembly length | 354 mm |
| Assembly mass | 60 g |

**Bench thrust data (3S battery only — 4S/6S retest recommended):**

| Throttle | Thrust | Current draw | Battery |
|---|---|---|---|
| 15% | 220 g | 1.21 A | 3S |
| 50% | 680 g | 7.25 A | 3S |
| 75% | 1228 g | 17.62 A | 3S |
| 100% | 1495 g | 25 A | 3S |

## 5. Battery Eliminator Circuit (BEC)

| Parameter | Specification |
|---|---|
| Type | UBEC (external, switched-mode) |
| Output | 5V / 5A |
| Efficiency | 85-90% |
| Purpose | Powers ruddervator servos + receiver independently of the ESC's internal BEC (~3 A rated, insufficient at ~3-4 A peak servo draw) |

## 6. Servo Actuators

| Parameter | Specification |
|---|---|
| Model | MG90S |
| Angular rotation range | 180 degrees |
| Torque | 1.8 kg-cm @ 4.8V — 2.2 kg-cm @ 6.6V |
| Operating voltage | 4.8-6.6 VDC |
| Mass | 13 g each |
| Peak current draw | 0.9-1.0 A each |
| Function | Ruddervator actuation (combined pitch + yaw) |

## 7. Known Open Items (Avionics-Relevant, Carried from Future Work)

- Motor/ESC/BEC margins bench-tested on **3S only**; system is designed for 4S (motor rated to 6S) — retest before relying on these numbers operationally.
- No self-stabilization / flight controller integration had been completed as of the Design Report (addressed in the Automation README).
- No payload (camera/sensor/telemetry) has been integrated; power budget for a payload is not yet accounted for in the BEC/battery sizing above.
- Noise output not measured despite being a stated design driver for motor/prop choice.
