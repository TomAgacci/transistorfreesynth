TITLE: Passive Square-Wave Keyboard (Moog-Style, No Transistors, Minimal Parts)

GOAL
- Build a “keyboard” of square-wave notes.
- Map:
  - DISTANCE → pitch/position
  - VOLUME → amplitude
- Emulate Moog modular settings (tuning, mixing, filtering, envelopes) using:
  - Switches
  - Resistors
  - Capacitors
  - Mechanical chopper
- Use VERY few components per key; no transistors.

============================================================
1. CORE ARCHITECTURE (ONE GLOBAL CHOPPER, MANY KEYS)
============================================================

- ONE electromechanical chopper disk → global square-wave clock.
- KEYS are simple switches that:
  - Connect different RC networks to the chopper → different pitches.
  - Route signal to different volume paths.

Conceptual block:

   [Chopper Disk + Brush] ---> [RC Network per Key] ---> [Mixer] ---> [Output Coil/Speaker]

Each key:
- Selects a frequency (pitch) via RC.
- Selects a volume via resistor.

============================================================
2. KEYBOARD WIRING (MINIMAL PER-KEY COMPONENTS)
============================================================

COMPONENTS PER KEY (ideal minimal):
- Kx: Key switch (momentary or latching)
- Rx: Pitch resistor
- Cx: Pitch capacitor
- Vx: Volume resistor (or shared volume ladder)

GLOBAL COMPONENTS:
- DISK + BRUSH: Square-wave chopper
- MIX_BUS: Passive summing node
- L_OUT: Output coil/speaker
- R_OUT: Output resistor (current limit)
- V1: DC supply (optional, for stronger tone)
- F1: Fuse

ASCII WIRING (SIMPLIFIED):

   +V (optional DC) 
      |
     F1
      |
   [Chopper Disk + Brush]  <-- global square-wave source
      |
      +-------------------+-------------------+-------------------+
      |                   |                   |
     K1                  K2                  K3          ... (keys)
      |                   |                   |
     R1                  R2                  R3          (pitch resistors)
      |                   |                   |
     C1                  C2                  C3          (pitch capacitors)
      |                   |                   |
      +--------- MIX_BUS (passive sum of all active keys) --------+
                              |
                             V-LADDER (volume resistors network)
                              |
                             L_OUT (coil/speaker)
                              |
                             R_OUT
                              |
                             GND

FUNCTION:
- Chopper produces a base square wave.
- Each key Kx, when pressed:
  - Connects that square wave through Rx–Cx → shapes frequency/phase.
  - Feeds into MIX_BUS.
- MIX_BUS sums all active keys passively.
- V-LADDER sets overall volume or per-key volume.
- L_OUT converts summed square waves to sound.

============================================================
3. MAPPING DISTANCE AND VOLUME
============================================================

DISTANCE → PITCH / POSITION:
- Define a “distance” parameter D for each key (e.g., physical key position).
- Map D to Rx–Cx values:
  - Closer keys → lower resistance or different capacitance → lower pitch.
  - Farther keys → higher resistance or different capacitance → higher pitch.
- Practically:
  - Arrange keys physically along a strip.
  - Use a resistor ladder (R1 < R2 < R3 < …) so position = pitch.

VOLUME → AMPLITUDE:
- Use a simple volume ladder:
  - V-LADDER = series of resistors to ground or to output.
  - Each key can tap a different point on the ladder:
    - Lower resistance to output → louder.
    - Higher resistance → quieter.
- Or one global volume pot:
  - MIX_BUS → V_POT → L_OUT.

============================================================
4. “MOOG MODULAR” SETTINGS WITH PASSIVE PARTS
============================================================

You can emulate Moog-like controls with very few passive modules:

[1] TUNING (OSC FREQUENCY)
- Controlled by Rx–Cx per key.
- Coarse tuning: change Cx values.
- Fine tuning: small trim resistors in series with Rx.

[2] MIXER
- MIX_BUS is a passive summing node.
- Multiple keys can sound at once → chords of square waves.

[3] FILTER (CRUDE LOW-PASS)
- Add a simple RC low-pass after MIX_BUS:
  - MIX_BUS → R_FILT → C_FILT → GND
  - Output taken across C_FILT.
- This softens the harshness of the square waves (Moog-like “filter”).

[4] ENVELOPE / VOLUME SHAPE (VERY SIMPLE)
- Use a capacitor to ground at the output:
  - Key press charges C_ENV through R_ENV.
  - Release lets C_ENV discharge.
  - This creates a crude attack/decay envelope.

Example:

   MIX_BUS ---- R_ENV ---- L_OUT
                      |
                     C_ENV
                      |
                     GND

[5] MODULATION (DISTANCE-BASED)
- Use a mechanical or resistive element that changes with physical distance:
  - A sliding contact on a resistor strip (like a long fader).
  - Distance of slider → changes resistance → modulates pitch or volume.

============================================================
5. ULTRA-MINIMAL PER-KEY DESIGN
============================================================

If you want *very, very few components* per key:

Per key:
- Kx: key switch
- Rx: pitch resistor
- (Optional) Cx: shared capacitor for all keys instead of per-key

Shared:
- One C_SHARED for all keys (global timing).
- One V_POT for global volume.
- One RC filter for global tone shaping.

Wiring:

   [Chopper] ----+---- K1 ---- R1 ---- MIX_BUS
                 |
                 +---- K2 ---- R2 ---- MIX_BUS
                 |
                 +---- K3 ---- R3 ---- MIX_BUS
                 ...

   MIX_BUS ---- C_SHARED ---- GND
        |
       V_POT
        |
       L_OUT ---- R_OUT ---- GND

This gives:
- Different pitches via R1, R2, R3.
- Shared timing via C_SHARED.
- Shared volume via V_POT.
- Shared output via L_OUT.

============================================================
6. SUMMARY

- One electromechanical chopper = global square-wave source.
- Keys are just:
  - Switch + resistor (and maybe capacitor).
- Distance is encoded in resistor values or physical slider position.
- Volume is controlled by a simple resistor ladder or single pot.
- Moog-like “modules” (tuning, mixing, filtering, envelope) are approximated with:
  - Passive RC networks.
  - Mechanical routing.
- No transistors; only:
  - Switches
  - Resistors
  - Capacitors
  - Coil/speaker
  - Chopper disk
  - Optional low-voltage DC supply.

