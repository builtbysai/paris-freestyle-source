Build Max Vedel's Paris Freestyle, the original. A Three.js piece: a chibi juggler freestyling a soccer ball through the painted streets of Paris, in rhythm, with a brush-stroke shader. Live reference: https://parisfreestyle.netlify.app/ — open it, watch it, study it before you write code. Process post: https://x.com/mxvdl/status/2120809570344050705

THE SYSTEM (build this, not the look):
1. Touch schedule, not keyframes. 16-beat loop at 144 BPM (6.67 s). 14 touches at beats 0-14, each {beat, body part, side}: parts are foot/knee/head/chest/volley, side is +/-1. A lob at beat 4, around-the-worlds at beats 9 and 11, a 360 spin under the lob, a jump at 13.55-14.5. A 16-entry SWING array gives the active side per beat. Every pose is a closed-form function of beat time — no keyframe tracks.
2. Contact by construction. For each touch, pose the body at the touch beat, read the ACTUAL world position of the contact part, and fit the ball's parabola between those measured points (g/2 = 4.905). Contact is exact by design, never by audit. The ball never touches the floor. Ball: visual radius 0.11, physics radius 0.135; 13 revolutions per loop, backspin, visual only. The around-the-world is inverted: the ball is the anchor, the FOOT orbits it.
3. Feet are a world-space IK state machine. Legs are NOT parented to the pelvis. A planted foot cannot slide; lift is a sine bump; the spin never drags the feet. Root y is always 0; the jump lives in the pelvis. Two-bone IK with a knee pole.
4. Proxy rig. Animate a small proxy (root/pelvis/spine/neck/head + world-space legs) and retarget onto a 65-bone skinned skeleton (Mixamo-style, incl. finger bones) via bind-offset quaternions. Chibi proportions: head 33% of height.
5. Brush look is geometry. Thousands of instanced stroke quads skinned to the skeleton (his: 6,400), plus a 2D paint engine for the streets: 5 passes (Sketch/Wash/Masses/Touches/Details) with a paint front that grows ahead of the camera, raymarched onto a 2.5D street corridor. Fisheye lens on every material (his default FOV 175). The only image in the build is the player texture.
6. It travels. 4 streets (Haussmann, Montmartre, Quais, Pigalle), 16 beats per street; the character advances one stride per beat. The world and camera move; the routine does not stay in place.
7. Audio clock drives animation, with drift resync. All sound synthesized in code (no samples): kick/snare/hat, chip melody, pad, and a touch sound pitched by body part (head 330, chest 120, knee 200, foot 170 Hz). Every touch plays a note.
8. Everything a slider, then export. Every parameter tunable in the app (GUI). Export to MP4 offline (WebCodecs VideoEncoder + Mp4Muxer, offline audio render), any aspect/format.
9. Head tracks the ball (pitch -0.6 to +0.5, yaw -0.7 to +0.7), suppressed during head/chest touches.

RULES:
- Original code, written by you. Do not copy Vedel's source. Study the live piece and the system above, then write your own.
- Study the real thing first: watch the live site, study real soccer freestyle footage for the moves. No imagined moves.
- A live system, not a canned timeline: state, targets, constraints, step rules. It loops, it responds, it can be tuned live.
- Verify by playing: play it yourself, desktop and phone, before calling anything done. A push, a green suite, or a headless screenshot is never proof.
- One exceptional direction: design before implementing, build small and validate, then expand. No generic AI aesthetics, no filler, no fake completeness.

DELIVERABLE: the working piece, live, with a short note on what you studied and what you changed from Vedel's system and why.
