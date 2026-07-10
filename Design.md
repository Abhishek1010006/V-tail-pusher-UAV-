# V-Tail Pusher UAV — Design


## 02 — Design

> Aeronautics was neither an industry nor a science; it was a miracle.
> *- Igor Sikorsky*

### 2.1 Airfoil Selection and Aerodynamic Characteristics

**Selected airfoil:** Clark Y (cambered)

**Geometric properties:**

- Maximum thickness: 11.7% of chord at ~28% from leading edge
- Flat lower surface from ~30% chord to trailing edge
- Curved, cambered upper surface
- Maximum camber: ~3.54% at ~34.52% chord
- Rounded leading edge for smooth aerodynamic entry
- Tapered trailing edge to reduce separation and drag

**Aerodynamic performance:**

- Maximum lift coefficient ($C_l$): approximately 1.3–1.68 for a plain wing configuration
- Stall onset: approximately 15°–18° angle of attack
- Moment coefficient ($C_m$) remains generally stable near the 25% chord reference line, a feature suited to basic aircraft applications
- When flaps are deployed, pitching moment becomes more negative and the center of pressure shifts toward the trailing edge (typical of cambered airfoils)
- The airfoil performs well in the low-to-medium Reynolds number regime typical of small UAVs; at very high Reynolds numbers, modern airfoil designs show greater relative performance gains compared to Clark Y

**Design rationale:** Clark Y was chosen for its well-documented, forgiving low-speed behavior, making it suitable for a small pusher UAV operating at modest airspeeds.

---

### 2.2 V-Tail Pusher Architecture

**Design characteristics:**

1. **Unconventional tail configuration**: two surfaces oriented upward in a "V" pattern, inclined at 120° when viewed from aft, replacing the standard T-tail/cruciform arrangement.
2. **Integrated control surfaces (ruddervators)**: each slanted tail surface has a hinged control section combining rudder (yaw) and elevator (pitch) function. Surfaces move differentially for yaw and symmetrically for pitch.
3. **Aerodynamic advantages**: reduced parasitic drag, reduced structural weight, and improved stealth characteristics from eliminating flow-disrupting/radar-reflecting surfaces.

**Additional advantages noted for the pusher configuration:**

- Reduced drag from fewer tail surfaces vs. conventional tail
- Lower structural weight from combined yaw/pitch control surfaces
- Improved aerodynamic efficiency in cruise
- Rear-mounted pusher prop keeps airflow clean over the wings
- Enhanced propeller safety during takeoff/landing
- Better nose-mounted payload and sensor visibility
- Compact, aesthetically clean configuration
- Suitable for VTOL, hover, and transition-capable UAV variants

**Trade-off:** The V-tail requires more complex control-surface mixing algorithms in the flight controller compared to a conventional tail.

---

### 2.3 Centre of Gravity Calculations

| Section         | Length | Mass   | Position from Reference |
| --------------- | ------ | ------ | ----------------------- |
| Forward section | 180 cm | 3205 g | 90 cm                   |
| Aft section     | 720 cm | 1765 g | 1760 cm from nose       |

Total fuselage length: 900 cm

Formula:

$x_{CG}$ = $\frac {Σ(m × d)} { Σm}$

Calculation:

$x_{CG}$ = $\frac{330 × 72} {506}$ = 46.9 cm ≈ **47 cm** from the ruddervators

---

### 2.4 Structural Material Selection

**Selected material:** Depron foam (lightweight, closed-cell extruded polystyrene/XPS sheet), used for fuselage, wing, and control surface construction.

**Material selection justification:**

1. Lightweight with a high strength-to-weight ratio; easy to fabricate, assemble, and field-repair, supporting rapid prototyping and iteration.
2. Better availability and cost-effectiveness compared to alternatives (foam core board, balsa wood, composite laminates).
3. Naturally smooth surface finish reduces skin friction drag and improves boundary layer behavior over aerodynamic surfaces while also easing fabrication and joining.

---

### 2.5 CAD Model, Dimensions, and Performance Calculations

**Key dimensions:**

| Parameter         | Value    |
| ----------------- | -------- |
| Wing Span         | 1.6 m    |
| Wing Chord        | 0.2 m    |
| Wing Area         | 0.32 m²  |
| Aspect Ratio      | 8        |
| Angle of V-Tail   | 120°     |
| V-Tail Root Chord | 0.152 m  |
| V-Tail Tip Chord  | 0.114 m  |
| V-Tail Length     | 0.6 m    |
| Fuselage Length   | 0.9 m    |
| Fuselage Width    | 0.0762 m |
| Dihedral Angle    | 30°      |

**Performance calculations:**

- **Stall speed:** $V_{stall}$ = $\sqrt{\frac{2L}{ρ·S·C_{L_{max}}}}$ ; ρ = 1.225 kg/m³, $C_{L_{max}}$ = 1.4 → **$V_{stall}$ = 7.0 m/s**

- **Cruise speed and thrust:** $V_{cruise}$ = $\sqrt{\frac{2W}{ρ·S·C_{L_{cruise}}}}$, cruise at 0° angle of attack, weight = lift, ρ = 1.225 kg/m³, $C_L$ (at 0°) = 1.4 → **$V_{cruise}$ = 7.0 m/s**. Estimated drag to overcome: **400 g** → **Required thrust: 400 g**

- **Takeoff speed:** At 10°–12° angle of attack, taken as 1.2× stall speed → **$V_{takeoff}$ = 8.4 m/s**

- **Wing Area** = Wing span × Wing chord = 1.6 × 0.2 = **0.32 m²**

- **Aspect Ratio** = $\frac{(Wing span)^2}{Wing\ area}$ = $\frac{2.56}{0.32}$ = **8**

- **Thrust-to-Weight Ratio** = $\frac{T_{max}}{Weight}$ = $\frac{14.71}{13.73}$ = **1.1**
