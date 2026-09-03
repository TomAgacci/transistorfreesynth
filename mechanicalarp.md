TITLE: Passive Mechanical Arpeggiator for Square-Wave Keyboard (No Transistors)

GOAL
- Add an ARPEGGIATOR to the passive square-wave keyboard.
- It should:
  - Step through keys in a pattern (up, down, up-down, random-ish).
  - Use only mechanical switching + resistors/capacitors.
  - Reuse the global square-wave chopper as the clock.
- No transistors, no ICs. Very few extra components.

============================================================
1. CORE IDEA: ROTATING ARP WAFER THAT “PLAYS” KEYS
============================================================

- Use a **rotating commutator / wafer** with multiple contacts.
- Each contact corresponds to a KEY LINE.
- As the wafer rotates, it sequentially connects:
  - KEY 1, then KEY 2, then KEY 3, etc.
- This creates an **arpeggio**: a pattern of notes played one after another.

Think of it as:
- A mechanical step sequencer.
- Driven by the same shaft as the square-wave chopper (or a related shaft).

============================================================
2. EXISTING KEYBOARD (SIMPLIFIED RECAP)
============================================================

From the previous design:

   [Chopper Disk + Brush] ----+---- K1 ---- R1 ---- MIX_BUS
                              |
                              +---- K2 ---- R2 ---- MIX_BUS
                              |
                              +---- K3 ---- R3 ---- MIX_BUS
                              ...

   MIX_BUS ---- V_POT ---- L_OUT ---- R_OUT ---- GND

Now we add an ARP MODULE that can “press” keys electronically by routing the chopper output.

============================================================
3. ARPEGGIATOR WIRING (MECHANICAL STEPPER)
============================================================

NEW COMPONENTS:
- ARP_WAFER: Rotating contact disk with multiple output pads.
- ARP_BRUSH: Spring contact that rides on ARP_WAFER.
- R_TEMPO: Resistor for tempo control (if using RC timing for motor/drive).
- C_TEMPO: Capacitor for tempo smoothing (optional).
- MODE_SWITCH: Selects arp mode (UP, DOWN, UP-DOWN, HOLD).

BASIC ARP ROUTING:

   [Chopper Output] ---- ARP_BRUSH ---- ARP_WAFER PADS ----> KEY INPUTS

ASCII:

   CHOPPER_OUT
       |
      ARP_BRUSH (moving over pads)
       |
   +---+---+---+---+---+
   |   |   |   |   |   |
  P1  P2  P3  P4  P5  ... (pads on ARP_WAFER)
   |   |   |   |   |
   |   |   |   |   +--> K5 input (instead of direct chopper)
   |   |   |   +------> K4 input
   |   |   +----------> K3 input
   |   +--------------> K2 input
   +------------------> K1 input

Each pad Pi is wired to the input side of a key’s resistor (R1, R2, R3…).

So:
- When ARP_BRUSH is on P1 → KEY 1 gets the square wave.
- When ARP_BRUSH moves to P2 → KEY 2 gets the square wave.
- Etc.

============================================================
4. ARP MODES (MECHANICAL)
============================================================

[UP MODE]
- ARP_WAFER pads arranged in ascending order:
  - P1 → K1
  - P2 → K2
  - P3 → K3
  - ...
- Wafer rotates continuously in one direction.
- Result: arpeggio goes up the keyboard.

[DOWN MODE]
- Reverse wiring:
  - P1 → highest key
  - P2 → next lower
  - ...
- Or use a second wafer with reversed mapping, selected by MODE_SWITCH.

[UP-DOWN MODE]
- Use two wafers or a cam that reverses direction mechanically.
- Simpler: one wafer with repeated pattern:
  - K1, K2, K3, K2, K1, K2, K3, ...

[HOLD / MANUAL]
- MODE_SWITCH bypasses ARP_WAFER:
  - CHOPPER_OUT → keys directly (manual play).
  - ARP module disconnected.

============================================================
5. TEMPO CONTROL (PASSIVE / MECHANICAL)
============================================================

You can control arpeggiator speed by controlling the **rotation speed** of ARP_WAFER:

[MECHANICAL TEMPO]
- Use a friction brake or adjustable tension spring on the shaft.
- More friction → slower rotation → slower arp.
- Less friction → faster rotation → faster arp.

[ELECTROMECHANICAL TEMPO (STILL PASSIVE)]
- If the shaft is driven by a small DC motor (allowed if you accept a motor):
  - Use R_TEMPO and C_TEMPO to control motor voltage (simple RC).
- If you want **no motor**, use:
  - Clockwork (spring) drive with escapement.
  - Adjustable escapement → tempo control.

============================================================
6. INTEGRATION WITH VOLUME AND DISTANCE
============================================================

DISTANCE:
- Physical position of each key along the keyboard = “distance”.
- ARP_WAFER pads are arranged to visit keys in a pattern based on that distance.
- You can define:
  - Short distance steps → close notes (small pitch jumps).
  - Long distance steps → wide jumps (big intervals).

VOLUME:
- Each key still has its own R1, R2, R3 (pitch) and possibly Vx (volume).
- Arpeggiator simply decides **which key is active at each moment**.
- Global volume via V_POT or per-key volume via resistors.

So:
- Arpeggiator = time-based selection of keys.
- Distance = spatial mapping of keys.
- Volume = amplitude via resistors.

============================================================
7. MINIMAL COMPONENT ARP MODULE SUMMARY
============================================================

Per arpeggiator:
- 1 × ARP_WAFER (rotating contact disk with multiple pads)
- 1 × ARP_BRUSH (spring contact)
- 1 × MODE_SWITCH (to bypass or select mapping)
- Optional:
  - 1 × R_TEMPO
  - 1 × C_TEMPO
  - Mechanical brake / spring for tempo

No transistors, no ICs:
- Only mechanical switching + resistors + capacitors.
- Arpeggiator is literally a rotating selector that “plays” the keys in sequence.

============================================================
8. HIGH-LEVEL PATCH SUMMARY

- CHOPPER_OUT → ARP_MODULE → KEY_INPUTS → MIX_BUS → VOLUME → L_OUT.
- ARP_MODULE:
  - Rotating wafer selects which key gets the square wave at each moment.
  - Mode switch chooses pattern (UP, DOWN, etc.).
  - Tempo controlled by shaft speed.

This gives you:
- A **square-wave keyboard**.
- A **mechanical arpeggiator**.
- Moog-like behavior (sequencing, pattern play) with:
  - Very few components.
  - No transistors.
  - Mostly mechanical + passive parts.

