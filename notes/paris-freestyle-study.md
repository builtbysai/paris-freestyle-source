# New York Freestyle — study + design note

## The reference (studied 2026-11-12, live browser)

**Paris Freestyle** by Max Vedel (@mxvdl, Spellbound Labs), https://parisfreestyle.netlify.app/.
Breakdown video in his X post https://x.com/mxvdl/status/2120809570344050705 (1:40).
Reference records: ~/workspace/.hatch-browser/outputs/browser-task:403febc9-cd7d-5c81-8e90-03089a0dd5fb/ (page PDFs f1/f2).

What it is: a brush-stroke juggling lab. A stylised juggler (65-joint rig, IK legs)
juggles a football through four painted Paris streets, rendered live with a
lightweight Three.js brush-stroke shader. Not a game: no score, no fail state,
no stage interaction (click/drag/hover do nothing). The Settings panel is the
real interaction: you are the art director.

**The driver, in one sentence:** a 144 BPM beat clock drives everything —
streets change every 16 beats (~6.7s), every ball touch plays a note, the
background paints in on the beat, and a Director's-cut camera reframes the
same infinite juggle so one move never feels like one shot.

Key mechanisms (from the breakdown):
- One long background painting per street (21,769 brush strokes in the Paris
  one), unrolled past the lens; the brush runs just ahead of the camera.
- Halftone drop shadow cast from the juggler's real silhouette.
- Trails: brush streaks and paint smears left by feet and ball.
- Hit frame: occasional slowed stylised freeze, figure in brush fragments.
- Everything is a setting: stroke size 1.25, density 0.45, kit colours,
  tempo, camera mode, lighting, moves. Changes re-skin live.
- Format switcher: 16:9 / 9:16 / 1:1 / 4:5 reframes the stage.
- Export MP4 is a real render (30s, 1920x1080, 1600 frames).
- Sound off by default; "Tap for sound" enables a 144 BPM kick/bass loop
  with a note per ball touch.
- Look: flat poster colours, ink outline, halftone shadow, white/blue speed
  lines, comic onomatopoeia near the ball ("pok", "poc", "toc", "pow").

## Our build: NEW YORK FREESTYLE

Model-and-improve, never clone. Same driving system, our own city, art, code,
and words. Do NOT copy his title, copy, tape captions, character design, or
Spellbound Labs branding. Do NOT copy the Paris streets.

**Setting:** New York. Four streets, each with its own palette and light:
1. Washington Square Park — golden hour
2. DUMBO / Brooklyn Bridge — afternoon
3. SoHo cast-iron — dusk
4. Coney Island boardwalk — night
(If a location proves hard to read as paint, swap it for another unmistakable
NYC scene and note the swap.)

**Tech:** 2D canvas, procedural brush-stroke renderer (deliberate choice over
Three.js: the stage reads as flat poster art, 2D gives precise control of the
painted look, and it is phone-safe). Single-file index.html, no dependencies,
no external assets. All art drawn in code.

**The juggler:** our own character (different kit and look from Max's pink-top
figure), procedural 2D rig with IK legs. Routine: foot, knee, chest, head
touches landing on the beat; ball on a parabolic arc with a dotted trajectory.
Halftone dot shadow from the figure's silhouette. Brush trails behind feet and
ball. Occasional hit-frame freeze. Comic onomatopoeia on touches (our own set).

**Beat clock:** 144 BPM default (slider 10-200). Street changes every 16 beats.
Background paints in over the first beats of each street. Loop is seamless.

**Camera:** Director's cut — shots cycle per street (wide, low angle,
close-up, rear view with sunburst). Poses blend smoothly. Camera mode setting:
Player level / Director / Tracking.

**Controls (every one must work for real; no fake buttons):**
- Pause / Play (freezes the exact frame), Tap for sound (off by default)
- Palette swatches (quick recolour), street jump (arrow keys + click the
  street name), format switcher (16:9 / 9:16 / 1:1 / 4:5)
- Settings panel: tempo, stroke size, density, disorder, kit + ball colours,
  camera mode, lighting, move selection
- Export MP4: real capture of the live canvas via MediaRecorder, downloadable
- Sound: WebAudio 144 BPM kick/bass loop + a note per ball touch. AudioContext
  suspends on tab hide, window blur, and page close; resumes on return unless
  muted (standing rule, applies to everything).

**Layout:** single-column app: header (handwritten-style title, system
handwriting font stack, no webfont dependency) -> stage -> transport/palette
bar -> collapsible Settings. Footer line with city + BPM. Navy page, cream
paper stage, palette shared between UI and artwork.

**Acceptance criteria (numbers before code):**
- Loop runs seamlessly; street changes every 16 beats; all 4 streets cycle.
- Ball touches land on beats; each touch plays a note (sound on), leaves a
  trail, and shows onomatopoeia.
- Settings re-skin live with no reload: stroke size/density/disorder, kit
  colours, tempo, camera mode, lighting.
- Format switch reframes the stage without breaking layout at 390px and
  1280px widths.
- Export MP4 produces a real downloadable file.
- Zero console errors. No control is decorative: everything on screen works.
- Headless screenshots at desktop + phone widths confirm the look before
  handoff. The main agent does the real-phone check before any link is sent.

## v2 background rework (2026-11-12)

v1 verdict: backgrounds did not read as real places on 3 of 4 streets — the
uniform horizontal dash texture flattened everything into streak noise.
Rework rule: **structure first, strokes second.** All landmark structure is
solid washes (crisp, jitter-free, and immune to the Density setting); strokes
are sparse texture only. New density-proof helpers: `washLine` / `washCurve` /
`washRim` (stamp small wash rects along a path) for cables, spokes, rims,
seams and diagonals; `winGrid` (crisp wash-rect windows); `treeSolid` /
`lampSolid` (solid shapes + sparse texture); `groundPlain` (converging seams
+ sparse dashes, no more full `hfill` ground). Skies are calm gradient wash
bands + a few cloud discs + ~10 long soft strokes.

Per street: WSP — full Arch silhouette (cream stone, arched opening, cornice,
statue, two tiny figures walking through), solid-canopy trees, fountain;
DUMBO — two solid towers with arched cutouts, clean catenary cables + regular
suspenders, deck, water band, cobble plaza (designed for the 1.30 low-angle
crop: visible y 0.235+); SoHo — full rebuild, five solid cast-iron facades
with dark gaps, value steps, window grids, storefronts + awnings, a real
fire-escape zigzag (designed for the 1.55 close-up crop: visible x 0.18-0.82);
Coney — complete Wonder Wheel (full rim, spokes, 8 gondolas, rim lights,
A-frame legs), neon signs as solid glowing shapes, boardwalk plank seams.
Juggler: added a yellow cap + slightly waisted torso (rig untouched).

Verified v2: fresh headless shots at desktop for all four streets + one phone
9:16 shot (~/workspace/shots/nyf-*-v2-*.png), all four pass the "name the
place" test; mid-wipe shot confirms the paint-in still works; a density-0.15
DUMBO shot confirms the structure survives the Density setting. Zero console
errors. v1 backup: index.html.bak-v1. Shot script: shot-v2.py.
