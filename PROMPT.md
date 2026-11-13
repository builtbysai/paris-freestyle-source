# Build prompt for Argon: a Paris Freestyle in a different sport

## What this is

Max Vedel's **Paris Freestyle** (live at https://parisfreestyle.netlify.app/)
is a Three.js piece: a stylised juggler freestyling a soccer ball through the
painted streets of Paris, in rhythm, with a brush-stroke shader. He built it
with Fable 5.2 (Claude) and Three.js. Every frame is painted live in the
browser, the sound is written in code, and everything is a slider.

Your job: build the same KIND of piece, but in a **different sport**.
Not soccer, not basketball (basketball is taken). Pick one and own it:
skateboarding, BMX, tennis, breakdancing, volleyball, juggling, martial arts,
whatever you can study properly. The sport is your choice; the system below
is the assignment.

## Study first (no shortcuts)

This repo holds everything you need to understand how his piece works:

- `source/` — Vedel's actual served source, pulled from the live site:
  `index.html`, `app.js` (his app logic, minified), `character.js` (the
  rigged character data). **Study only. Do not copy his code, do not
  republish it, do not lift shaders or routines. Understand the mechanisms,
  then write your own.**
- `notes/DECONSTRUCTION.md` — the piece-by-piece read of his source:
  architecture, rig, animation, ball, brush shader, beat clock, camera,
  export, and a full comparison against our basketball build. Read it all.
- `notes/RESEARCH.md` — 71 sources on the techniques behind the piece.
- `notes/paris-freestyle-study.md` — the short study note on the piece.

His process, in his own words (X, Nov 11-12 2026): "fully made with Fable
5.2", "every frame is painted live, in a browser", "sound written in code",
"The only image in this build is the texture for the player." He gives no
prompt text and no iteration counts. The system in the source is the real
record of his process. Read the source, not just the posts.

## The system to build (mechanisms, not looks)

Do not copy the look. Build the driving system, in your sport:

1. **A touch schedule, not keyframes.** His 16-beat routine is a schedule
   of touches (foot/knee/head/chest/volley + a side array). Every pose is a
   closed-form function of beat time. Your sport has its own touch/impact
   vocabulary: define it first (e.g. for skateboarding: ollie, kickflip,
   grind, land), schedule it on the beat, derive every pose from it.
2. **Contact by construction.** He poses the body at each touch beat, reads
   the ACTUAL world position of the contact part, and fits the ball arc
   between those measured points. Contact is exact by construction, never by
   audit. Your moving object (ball, board, body) must meet the body at
   measured points. If a guided path can cross the body, the design is wrong.
3. **Feet (or ground contact) as a state machine.** His feet are world-space
   IK targets driven by a run-based state machine: a planted foot cannot
   slide, lift is a sine bump, the spin never drags the feet. Root y is
   always 0; the jump lives in the pelvis. Whatever touches the ground in
   your sport, it obeys the same rule: planted means planted.
4. **A proxy rig.** He animates a small proxy (root/pelvis/spine/neck/head +
   world-space legs) and retargets onto the 65-bone skeleton. Animation and
   mesh stay decoupled, so proportions can change without re-authoring
   poses.
5. **Brush look as geometry.** His strokes are thousands of instanced quads
   skinned to the skeleton, plus a 2D paint engine for the streets
   (5 passes: Sketch/Wash/Masses/Touches/Details) raymarched onto a
   corridor, with a fisheye lens patch on every material. The look is
   geometry and shaders, not a texture.
6. **The audio clock drives the animation.** Animation time comes from the
   audio context clock, with drift resync. Sound is synthesized in code:
   every touch plays a note.
7. **Everything a slider, then export.** Every parameter tunable in the app,
   export to video in any format.

## Rules

- **Study the sport from real reference.** Watch real footage of the moves
  you schedule. No imagined moves. If you cannot find reference for a move,
  do not build it.
- **Original code, your own art direction.** The system is the lesson; the
  code, the look, and the sport are yours.
- **A live system, not a canned timeline.** State, targets, constraints,
  step rules. The piece must be watchable as a system: it loops, it
  responds, it can be tuned live.
- **Verify by playing.** Play it yourself, desktop and phone, before
  calling anything done. A push, a green suite, or a headless screenshot is
  never proof. If it looks wrong on a real device, it is wrong.
- **One exceptional direction.** Research exceptional references first,
  design before implementing, build small and validate, then expand. No
  generic AI aesthetics, no filler, no fake completeness.

## Deliverable

A working piece, live, in your sport, with a short note on what you studied
and what you changed from Vedel's system and why. The note matters as much
as the piece: it proves you understood it instead of copying it.
