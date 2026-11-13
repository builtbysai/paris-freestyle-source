# Paris Freestyle — source study + build prompt for Argon

Private study repo. Max Vedel's Paris Freestyle
(https://parisfreestyle.netlify.app/), deconstructed from the inside.

## Contents

- `source/` — Vedel's served source, pulled from the live site on
  2026-11-13: `index.html`, `app.js` (162 KB, minified), `character.js`
  (2.4 MB, rigged character data). **Study only.** Not open source, not
  ours to republish or copy. Do not lift code from it.
- `notes/DECONSTRUCTION.md` — piece-by-piece read of the source (295
  lines): architecture, rig (65 bones), animation (touch schedule, no
  keyframes), ball (contact by construction), brush shader (strokes as
  geometry), beat clock, camera, export, and a full comparison against
  our basketball build.
- `notes/RESEARCH.md` — 71 sources on the techniques behind the piece.
- `notes/paris-freestyle-study.md` — short study note on the piece.
- `PROMPT.md` — the build prompt for Argon: the same kind of piece in a
  different sport, from understanding, not copying.

## Key findings (from the deconstruction)

- It is **soccer freestyle, not basketball**: foot/knee/head/chest
  touches, the ball never touches the floor.
- **No keyframes.** A 16-beat touch schedule; every pose is a closed-form
  function of beat time.
- **Contact by construction.** The body is posed at each touch beat, the
  real contact point is measured, the ball arc is fitted between measured
  points. The clips an audit finds cannot happen by design.
- **Feet are a world-space IK state machine**; root y is always 0, the
  pelvis jumps; the 360-degree spin never drags the feet.
- **Brush look is geometry**: 6,400 instanced stroke quads skinned to the
  skeleton, plus a 2D paint engine raymarched onto a street corridor,
  plus a fisheye lens patch on every material.
- **Chibi head at 33% of height**, baked into the source mesh.
- **Audio clock drives animation**, with drift resync. Export is offline
  (WebCodecs + Mp4Muxer).

## Provenance

- Source pulled 2026-11-13 from https://parisfreestyle.netlify.app/
  (publicly served files, read for study).
- Vedel's process post: https://x.com/mxvdl/status/2120809570344050705
- Our basketball build (separate repo): builtbysai/freestyle-basketball
