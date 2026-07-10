# 08 — Future Work
### V-Tail Pusher UAV | Aerospace Society, BIT Mesra

## Why this section exists

This section is for whoever picks up this project next. Building the V-tail pusher taught us a lot, and some of what we learned only became clear once the aircraft was fully built, not while we were still designing it on paper. We are writing it down here so the next team does not have to rediscover the same things the hard way, and can spend their time and budget improving on our work instead of repeating it.

The report before this section covers what we built and why. This part covers what we would do differently, and what we simply did not get to. We have not flight-tested the aircraft yet, so some of the points below are things we noticed during ground assembly and bench testing.

## What we would improve

- **Motor and propeller testing:** We chose the 750 KV motor and 14x4.7 propeller for their balance of static thrust and quiet running, and this time we bench-tested it, but only on a 3S battery. The ESC and battery eliminator circuit were sized assuming 4S operation, so a future team should run the same throttle-vs-thrust-vs-current test on 4S (and ideally 6S, since the motor is rated for it) to confirm the margins hold at higher voltage too.

- **Servo sizing:** A smaller, lighter airframe would also reduce the load on the MG90S servos. It would be worth checking whether a smaller servo could do the job just as well once the aircraft is scaled down, saving a little more weight and current draw.

- **Aircraft size:** The aircraft turned out bigger than it needed to be for the mission. This added extra Depron, extra weight, and a bigger battery than the mission really called for. A smaller build would be easier to transport, launch, and land, and we would recommend setting a firm size target early next time.

- **Self-stabilization:** We did not add any self-stabilization, mainly due to lack of time and budget. Even a basic flight controller with an auto-level mode would make the aircraft far more forgiving in gusty conditions. Entry-level FC boards are cheap and well documented, so this is one of the more achievable upgrades for a future team.

- **Damage-prone area mitigation:** Our own damage-assessment table lists nine areas prone to failure, from the motor and propeller to the V-tail ruddervators and battery mount, but we only documented the risk rather than designing around it. A future team could pick the two or three highest-consequence items, such as the propeller strike zone and the LiPo mount, and add specific protection: a sacrificial nose skid, a firewall between battery and fuselage, or reinforced ruddervator hinges.

- **Earlier testing:** Because of time constraints, most of our testing happened close to the final build rather than at the design stage. Running more ground tests and simple simulations earlier, before parts are cut and glued, would catch sizing and balance issues while they are still cheap to fix.

- **Reynolds number check:** We now have real numbers for cruise speed (7 m/s) and wing chord (0.2 m) from the design calculations in this report, so a future team could work out the actual operating Reynolds number from these instead of just assuming the Clark Y airfoil's usual low-to-medium range applies. This is worth doing before the first flight test, not after.

- **CG reconciliation:** This report actually gives two different centre of gravity figures calculated by two different methods: 47 cm from the aft reference point using fuselage section masses, and 43 cm from the nose using component masses. We never reconciled the two or checked either one against the finished aircraft. A simple balance test on the built airframe would confirm where the real CG sits.

- **Noise measurement:** One reason we picked the 750 KV motor and 14-inch propeller was reduced noise for surveillance-type missions, but we never actually measured sound output. A simple decibel reading at a fixed distance during ground running would confirm whether this choice delivers the quiet operation it was meant for.

- **Payload integration:** This platform was designed with surveillance and environmental monitoring missions in mind, but no camera, sensor, or telemetry payload was ever integrated. Since the airframe was sized before any payload was chosen, a future team should decide on the payload first and size the aircraft and CG around it.
