# Automation — Pixhawk 2.4.8 Integration
### V-Tail Pusher UAV | Aerospace Society, BIT Mesra

This document covers everything needed to automate the V-Tail Pusher UAV using a **Pixhawk 2.4.8** flight controller: hardware notes specific to this board, required software, firmware selection, wiring, and the full configuration sequence for a V-tail fixed-wing airframe.

---

## 1. Important Hardware Note on the Pixhawk 2.4.8

The board sold as "Pixhawk 2.4.8" is a third-party clone of the original **Pixhawk 1** hardware design (STM32F427 MCU, 1MB flash / 192KB–256KB RAM depending on batch, MPU6000 + ICM-20608 IMUs, MS5611 barometer, no internal compass). Keep this in mind, since it directly affects firmware choice:

- Because of the limited 1MB flash, only **older, size-trimmed firmware builds** fit on the board.
- Some clone batches substitute sensors (e.g. MS5607 instead of MS5611), which can throw pre-arm warnings. This is a known issue, not necessarily a fault.
- There is **no CAN bus / UAVCAN support** on this board.
- No built-in compass — an external GPS+compass module (e.g. a Ublox M8N with integrated compass) is required.

## 2. Recommended Firmware

| Flight stack | Recommended version for this board | Notes |
|---|---|---|
| **ArduPlane** (recommended) | Latest stable **4.3.x / 4.4.x "fmuv3" build** | Fixed-wing V-tail mixing is natively supported and well-tested on this hardware class |
| PX4 | v1.13.4 (legacy) | PX4's newer releases have dropped support for this flash-limited hardware |

For this airframe, use **ArduPlane**, since it has built-in V-tail (ruddervator) mixing and this project is a fixed-wing pusher, not a multirotor.

## 3. Required Software

| Software | Purpose | Platform |
|---|---|---|
| **Mission Planner** | Primary Ground Control Station (GCS): firmware flashing, calibration, parameter tuning, mission planning | Windows (best supported) |
| **QGroundControl (QGC)** | Alternative GCS, cross-platform | Windows / macOS / Linux |
| **ArduPilot firmware (ArduPlane)** | Flight stack running on the Pixhawk | Flashed via Mission Planner/QGC |
| **Mavlink Inspector / MAVProxy** (optional) | Advanced telemetry inspection and scripting | Windows / Linux |
| **SiK Radio firmware** (if using 3DR/HolyBro telemetry radios) | Ground-to-air telemetry link | Configured via Mission Planner |

> Mission Planner is Windows-native. If working on macOS/Linux, use QGroundControl for setup; steps below map closely between the two, with minor UI differences.

## 4. Wiring Overview

| Component | Connects to |
|---|---|
| 750 KV BLDC motor (via 40A ESC) | MAIN OUT 3 (Throttle channel, typically) |
| Ruddervator servo 1 (MG90S) | MAIN OUT 1 |
| Ruddervator servo 2 (MG90S) | MAIN OUT 2 |
| Receiver (PPM/SBUS) | RC IN port |
| UBEC 5V/5A | Servo rail (powers servos + receiver independently of ESC BEC) |
| GPS + Compass module | GPS & I2C ports |
| Telemetry radio | TELEM1 |
| LiPo battery (via power module) | POWER port |

**Critical:** Since this build already uses a dedicated UBEC (see Technical Specs README, Section 4.5) to avoid overloading the ESC's internal BEC, make sure the ESC's BEC output is **not** simultaneously connected to the servo rail — only one power source should feed the servo rail at a time.

## 5. Firmware Flashing Steps

1. Connect the Pixhawk to your PC via USB **without** the battery connected.
2. Open Mission Planner → **Setup → Install Firmware**.
3. Select **Plane** (not Copter/Rover).
4. Choose the recommended stable ArduPlane version compatible with fmuv3/Pixhawk 1-class hardware.
5. Wait for "erase → program → verify → Upload Done."
6. Disconnect and reconnect (allow the bootloader to exit), then click **Connect** in Mission Planner at 115200 baud.

## 6. V-Tail (Ruddervator) Mixing Configuration

ArduPlane has native V-tail mixing — you do not need external Y-cables or a separate mixer.

1. Go to **Config/Tuning → Full Parameter List** (or Full Parameter Tree).
2. Set the servo output functions:
   - `SERVOx_FUNCTION` = **75 (VTail Left)** for the left ruddervator servo output.
   - `SERVOy_FUNCTION` = **76 (VTail Right)** for the right ruddervator servo output.
   - `SERVOz_FUNCTION` = **70 (Throttle)** for the ESC/motor output.
3. Confirm `MIXING_GAIN` is left at default (1.0) unless throw needs scaling.
4. Use **Setup → Mandatory Hardware → Servo Output** to verify correct throw direction on both ruddervators — differential = yaw, symmetric = pitch, matching Section 3 of the Design Report.
5. Set throw limits (`SERVOx_MIN/MAX/TRIM`) so the ruddervators do not bind mechanically at full deflection — cross-check against the MG90S's 180° rotation range and the hinge geometry.

## 7. Mandatory Calibration Sequence

Perform these in Mission Planner under **Setup → Mandatory Hardware**, in this order:

1. **Frame/Airframe type** — select "Plane" and the V-tail airframe class if offered by your firmware version (some builds infer this purely from `SERVO_FUNCTION` mapping).
2. **Accelerometer calibration** — 6-position calibration (level, nose up, nose down, left, right, inverted). Set `AHRS_ORIENTATION` correctly if the Pixhawk is mounted rotated relative to the nose.
3. **Compass calibration** — rotate the aircraft through all axes with the external GPS/compass connected.
4. **Radio calibration** — verify full stick throw is captured for all channels, including throttle cut and any mode-switch channel.
5. **ESC calibration** — follow the ArduPilot ESC calibration procedure so throttle-zero and throttle-full match the transmitter's range (important given the bench-tested 3S throttle-vs-thrust curve in the Design Report; recheck once operating on 4S).
6. **Flight mode setup** — assign at minimum: **Manual**, **Stabilize** (or **FBWA**), and **RTL** to your mode switch positions.
7. **Failsafe configuration**:
   - RC failsafe → set to **RTL** or **Circle**.
   - Battery failsafe voltage/capacity thresholds — set conservatively given the airframe has no self-stabilization history yet.
   - GCS failsafe (if using telemetry-based automation) → set to **RTL**.

## 8. Pre-Autonomy Checklist (Ground Testing Before First Autonomous Flight)

- [ ] Motor/prop thrust re-verified on 4S (per Future Work recommendation — do this before trusting throttle-to-thrust mapping in autopilot tuning).
- [ ] Ruddervator direction and mixing verified on the bench with radio in Manual mode.
- [ ] CG reconciled and balance-tested on the physical airframe (see Design Report Section 3.2 vs Section 6.2 — two CG values, 47 cm and 43 cm, were never reconciled).
- [ ] Compass calibrated away from motor/ESC wiring interference.
- [ ] RTL altitude and home position behavior tested in a safe, open area.
- [ ] Range test of RC link and telemetry link completed.
- [ ] Battery failsafe thresholds tested with a partially depleted pack, not just configured on paper.

## 9. Optional: Adding a Camera/Telemetry Payload

Per the Design Report's Future Work notes, no payload was integrated in this build. When one is added:

- Decide on the payload **before** finalizing CG placement — mount it and re-balance rather than assuming the existing 43–47 cm CG range still holds.
- Route payload power independently of the servo UBEC rail if the payload draws significant current, to avoid disturbing servo voltage stability.
- If adding an FPV/telemetry camera, a separate video transmitter and antenna should be kept clear of the GPS/compass module to avoid RF interference with heading estimation.

---

*This document assumes ArduPlane as the flight stack. If the team later decides to move to a more modern autopilot (e.g. Pixhawk 4/Cube Orange) for RTK GPS or CAN-bus peripherals, most of the mixing and calibration logic above carries over — only the flashing step changes.*
