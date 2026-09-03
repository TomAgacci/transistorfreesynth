TITLE: MASTER WIRING SCHEMATIC – TRANSISTOR‑FREE CRYSTAL SQUARE SYNTH
       (Moog‑Modular‑Style Features, Passive Equivalents)

GOAL
- One synth box, transistor‑free, powered by a MIDI keyboard’s DC/USB supply.
- Features (all in passive / mechanical equivalents):
  - Square‑wave oscillator (VCO)
  - Mixer
  - Ladder‑style low‑pass filter
  - Envelope (attack/decay)
  - VCA‑like volume shaping
  - Mechanical arpeggiator / sequencer
  - MIDI‑layout keyboard (distance + volume ratios)
  - Crystal timbre via 1 cm trick position

============================================================
0. GLOBAL POWER (FROM MIDI KEYBOARD)
============================================================

[MIDI KEYBOARD POWER SUPPLY]
- DC_OUT+ : +V (e.g., 5–12 V)
- DC_OUT- : GND

WIRING:

   DC_OUT+  ------------------------->  SYNTH +V
   DC_OUT-  ------------------------->  SYNTH GND

Inside synth, everything is passive / mechanical; power is just borrowed.

============================================================
1. CORE OSCILLATOR (SQUARE-WAVE CHOPPER = VCO)
============================================================

COMPONENTS:
- CHOPPER_DISK: rotor with conductive/insulating segments
- CHOPPER_BRUSH: spring contact
- F1: fuse
- SW_MAIN: main power switch

WIRING:

   +V (from MIDI keyboard)
      |
     F1
      |
   SW_MAIN
      |
   CHOPPER_DISK + CHOPPER_BRUSH
      |
   SQR_OUT  (global square-wave signal)
      |
     (to mixer, keyboard, arp, etc.)

FUNCTION:
- Mechanical rotation sets frequency (VCO equivalent).
- Disk pattern sets duty cycle (square wave).

============================================================
2. KEYBOARD MODULE (MIDI-LAYOUT, DISTANCE + VOLUME RATIOS)
============================================================

Each key K_n (C, C#, D, …) has:
- R_VOL_n : volume ratio (velocity equivalent)
- R_DIST_n: distance / octave ratio (pitch family)

WIRING (PER KEY):

   SQR_OUT
      |
     K_n (key switch)
      |
     R_VOL_n   (volume ladder tap)
      |
     R_DIST_n  (distance / octave ladder tap)
      |
     KEY_BUS_n
      |
     (to MIX_BUS)

ALL KEYS TO MIX BUS:

   KEY_BUS_C   \
   KEY_BUS_C#   \
   KEY_BUS_D     \
   ...            +----> MIX_BUS
   KEY_BUS_B     /
   KEY_BUS_HIGH /

- Keys laid out like a MIDI keyboard.
- Ratios in R_VOL_n and R_DIST_n chosen to match MIDI note behavior.

============================================================
3. MIXER MODULE (PASSIVE SUM, MOOG-LIKE MIXER)
============================================================

COMPONENTS:
- MIX_BUS: passive summing node
- R_MIX_OUT: output resistor

WIRING:

   (from all KEY_BUS_n)
      |
    MIX_BUS
      |
    R_MIX_OUT
      |
    MIX_OUT
      |
    (to filter and envelope)

FUNCTION:
- Passive sum of all active keys.
- R_MIX_OUT sets overall level.

============================================================
4. LADDER-STYLE LOW-PASS FILTER (PASSIVE MOOG-LIKE)
============================================================

COMPONENTS:
- R_F1, R_F2, R_F3: filter resistors
- C_F1, C_F2, C_F3: filter capacitors

WIRING (SERIES LADDER):

   MIX_OUT
      |
     R_F1
      |
     R_F2
      |
     R_F3
      |
    FILT_OUT
      |
     (to envelope / VCA-like stage)

SHUNT CAPACITORS TO GND:

   Node between MIX_OUT and R_F1:  C_F1 → GND
   Node between R_F1 and R_F2:    C_F2 → GND
   Node between R_F2 and R_F3:    C_F3 → GND

ASCII:

   MIX_OUT ─ R_F1 ─ R_F2 ─ R_F3 ─ FILT_OUT
              |       |       |
             C_F1    C_F2    C_F3
              |       |       |
             GND     GND     GND

FUNCTION:
- Multi‑pole low‑pass filter.
- Passive approximation of Moog ladder filter (no resonance, but Moog‑like rolloff).

============================================================
5. ENVELOPE + VCA-LIKE STAGE (PASSIVE ATTACK/DECAY VOLUME SHAPING)
============================================================

COMPONENTS:
- R_ENV_CHARGE: attack resistor
- R_ENV_DISCH: decay resistor
- C_ENV: envelope capacitor
- R_ENV_VOL: envelope‑controlled volume resistor
- MECH_LINK: mechanical linkage (optional) to move a wiper

ENVELOPE GENERATION (RC):

   KEY_GATE (logical OR of all key presses)
      |
   R_ENV_CHARGE
      |
     C_ENV
      |
     GND

   C_ENV also discharges through R_ENV_DISCH to GND.

FUNCTION:
- When any key is pressed, KEY_GATE goes high:
  - C_ENV charges through R_ENV_CHARGE → attack.
- When keys are released:
  - C_ENV discharges through R_ENV_DISCH → decay.

VCA-LIKE VOLUME CONTROL (MECHANICAL / PASSIVE):

Option A: Electrical shaping (simple):

   FILT_OUT
      |
    R_ENV_VOL (set by envelope level or fixed)
      |
    AMP_IN
      |
    (to output driver)

Option B: Mechanical VCA:

- C_ENV voltage drives a small actuator (spring, lever).
- Lever moves a wiper on a resistor or a physical damper on the coil.
- Volume changes over time → VCA‑like behavior.

============================================================
6. MECHANICAL ARPEGGIATOR / SEQUENCER MODULE
============================================================

COMPONENTS:
- ARP_WAFER: rotating contact disk with pads P1, P2, … Pn
- ARP_BRUSH: spring contact riding on ARP_WAFER
- MODE_SWITCH: selects ARP or direct keyboard

WIRING:

   SQR_OUT
      |
   ARP_BRUSH
      |
   ARP_WAFER PADS:
      P1 → K_C input
      P2 → K_E input
      P3 → K_G input
      ...
      Pn → other key inputs

MODE SWITCH:

   SQR_OUT ---- MODE_SWITCH ----> (either)
      - DIRECT: SQR_OUT → keyboard K_n directly
      - ARP:    SQR_OUT → ARP_BRUSH → ARP_WAFER → keys

FUNCTION:
- As ARP_WAFER rotates, ARP_BRUSH sequentially connects SQR_OUT to different keys.
- Creates arpeggios / sequences mechanically.
- Tempo set by shaft speed.

============================================================
7. OUTPUT DRIVER (COIL AT 1 CM TRICK POSITION, CRYSTAL TIMBRE)
============================================================

COMPONENTS:
- L_OUT: coil or small speaker
- R_OUT: safety / shaping resistor
- C_SHINE: small capacitor for brightness (optional)
- MOUNT: mechanical mount at 1 cm trick position

WIRING:

   AMP_IN (from envelope/VCA-like stage)
      |
     L_OUT  (mounted 1 cm from rigid surface)
      |
     R_OUT
      |
     GND

Optional brightness:

   L_OUT
      |
     C_SHINE
      |
     GND

FUNCTION:
- L_OUT at 1 cm from a plate/wall gives a single “crystal” tone.
- R_OUT limits current.
- C_SHINE adds subtle high‑frequency shaping.

============================================================
8. MODULE INTERCONNECTION SUMMARY (BLOCK FLOW)
============================================================

POWER:
   MIDI_KEYBOARD_DC → F1 → SW_MAIN → +V → all modules
   MIDI_KEYBOARD_GND → GND → all modules

SIGNAL FLOW:

   +V → CHOPPER → SQR_OUT
      |
      +--> KEYBOARD (K_n → R_VOL_n → R_DIST_n → KEY_BUS_n → MIX_BUS)
      |
      +--> ARPEGGIATOR (optional path to keys)

   MIX_BUS → R_MIX_OUT → MIX_OUT
   MIX_OUT → LADDER FILTER (R_F1/C_F1, R_F2/C_F2, R_F3/C_F3) → FILT_OUT
   FILT_OUT → ENVELOPE/VCA-LIKE (R_ENV, C_ENV, mechanical volume) → AMP_IN
   AMP_IN → L_OUT (1 cm trick position) → R_OUT → GND
           + optional C_SHINE → GND

CONTROL:

- KEYBOARD:
  - Layout matches MIDI notes.
  - R_VOL_n = velocity / volume ratios.
  - R_DIST_n = octave / pitch ratios.

- ARPEGGIATOR:
  - Rotating wafer selects keys in patterns (UP, DOWN, etc.).

- ENVELOPE:
  - KEY_GATE from keys triggers RC envelope.

============================================================
9. WHAT THIS MASTER SCHEMATIC GIVES YOU (MOOG-LIKE FEATURES)
============================================================

- VCO: CHOPPER (square-wave oscillator).
- MIXER: MIX_BUS + R_MIX_OUT.
- FILTER: passive ladder low‑pass (R_Fn + C_Fn).
- ENVELOPE: RC attack/decay (R_ENV_CHARGE, R_ENV_DISCH, C_ENV).
- VCA-LIKE: envelope‑driven volume via R_ENV_VOL / mechanical linkage.
- SEQUENCER/ARP: ARP_WAFER + ARP_BRUSH + MODE_SWITCH.
- KEYBOARD: MIDI‑layout, distance + volume ratios.
- OUTPUT: crystal square synth sound via 1 cm trick position.

All of it:
- Transistor‑free inside the synth.
- Powered by the MIDI keyboard’s DC/USB supply.
- Built from:
  - Resistors
  - Capacitors
  - Coil/speaker
  - Mechanical disks, brushes, levers
  - Simple switches and wiring

END OF MASTER SCHEMATIC
