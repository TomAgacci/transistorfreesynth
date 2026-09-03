TITLE: Mic-Driven Electromechanical Square Wave (No Transistors)

GOAL
- Use a microphone signal to drive / modulate a square-wave tone.
- No transistors: only passive parts (mic, transformer, resistors, coil, chopper).
- The mic does NOT need to be amplified electronically; we use coupling and mechanics.

============================================================
1. CONCEPTUAL APPROACH
============================================================

- Use a **dynamic microphone** (coil mic) as an AC source.
- Couple the mic through a **transformer** to:
  - Isolate the mic.
  - Step up/down voltage as needed.
- Feed the transformer output into the **chopper contact** and **coil/speaker**.
- The **chopper disk** still creates the ON/OFF square pattern.
- The **mic signal** rides on top of that pattern (amplitude modulation).

So:
- Chopper = ON/OFF gate → square envelope.
- Mic = audio content → modulates the amplitude inside that envelope.

============================================================
2. COMPONENTS
============================================================

[INPUT]
- MIC: Dynamic microphone (coil type)
- T1: Audio transformer (mic coupling transformer)

[CHOPPER + LOAD]
- DISK: Rotating contact disk (conductive segments + gaps)
- BRUSH: Spring contact on disk
- L1: Coil or speaker (tone emitter)
- R_LOAD: Optional series resistor

[POWER (OPTIONAL)]
- V1: Low-voltage DC supply (if you want a DC bias)
- F1: Fuse
- SW_MAIN: Main switch

============================================================
3. WIRING DIAGRAM (MIC INTO SQUARE WAVE PATH)
============================================================

A. MIC + TRANSFORMER (AC SOURCE)

   MIC (dynamic)
      |
      |----[T1 primary]----
      |
     GND_MIC

   T1 secondary:
      S1 ----+----------------------+
             |                      |
             |                      |
            BRUSH (chopper contact) |
             |                      |
             |                      |
            L1 (coil/speaker)       |
             |                      |
             |                      |
           R_LOAD (optional)        |
             |                      |
             |                      |
           GND_AUDIO----------------+

B. CHOPPER CONTACT + DISK

Side view (simplified):

   T1 secondary S1
        |
      BRUSH (spring contact)
        ^
        |
   +----+----+
   |         |   <-- DISK (rotor)
   |  ###    |   ### = conductive segment (ON)
   |  ---    |   --- = insulating gap (OFF)
   |  ###    |
   |  ---    |
   +---------+

- When BRUSH is on "###" (conductive):
  - MIC → T1 → BRUSH → L1 → R_LOAD → GND_AUDIO
  - Mic AC drives L1: audio present.
- When BRUSH is on "---" (insulating):
  - Circuit open at BRUSH/DISK.
  - Mic AC cannot reach L1: audio OFF.
- Result: square-wave gating of mic audio.

============================================================
4. OPTIONAL DC BIAS (IF YOU WANT A STRONGER SQUARE TONE)
============================================================

You can add a DC supply in series with the mic path:

   +V (Battery +)
      |
     F1
      |
   SW_MAIN
      |
      +----[T1 secondary S1]---- BRUSH ---- L1 ---- R_LOAD ---- GND

Mic path:

   MIC ---- T1 primary ---- GND_MIC

Effect:
- DC provides a base current when BRUSH is ON → strong square-wave buzz.
- Mic AC rides on top of that DC → modulates the square-wave amplitude.
- Still no transistors; only passive parts and possibly a battery.

============================================================
5. FUNCTIONAL SUMMARY
============================================================

- MIC generates small AC voltage from sound.
- T1 couples and scales that AC to the chopper/load circuit.
- DISK + BRUSH act as a **mechanical gate**:
  - ON segments: mic signal (and optional DC) reach L1.
  - OFF gaps: L1 is disconnected → silence.
- L1 outputs:
  - A square-wave-like envelope (from chopper).
  - Inside that envelope, the mic’s audio content.

So you get:
- **Square wave structure** from the electromechanical chopper.
- **Mic-driven modulation** of that square wave.
- All done with **no transistors**—only mic, transformer, mechanical contact, coil, and optional battery.

