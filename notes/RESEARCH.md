# Freestyle Basketball - Deep Research

**Date:** 2026-11-12
**Purpose:** Research for a freestyle basketball piece modeled on Max Vedel's Paris Freestyle, after Hans rejected the first attempt (a 2D canvas New York version) as lazy and missing the spirit. His order: study how the creator actually built his pieces, the process, the models, the 3D, the artwork, the inspiration - no shortcuts - then build slowly and correctly, better than the reference.
**Status:** RESEARCH ONLY. Nothing in this file is a build. The staged build plan is Section 6.

## How to read this file

- Every factual claim carries a source URL. Claims observed first-hand from the reference site or its breakdown video cite the study file and the X post.
- **[DERIVED]** marks a claim computed or synthesized from cited sources (arithmetic, or a design inference drawn from the sources). It is not a sourced fact; it is labeled so the builder can tell the difference.
- **[UNCONFIRMED]** marks a claim seen once but not re-verifiable at write time. It is a lead, not a fact.
- Section 5 lists everything that could NOT be found. Read it before building: it names the gaps the build must work around, and the closest public equivalent for each.

---

## 0. The reference at a glance: Paris Freestyle

Paris Freestyle by Max Vedel (@mxvdl) is a brush-stroke juggling lab at
https://parisfreestyle.netlify.app/ - a stylised juggler juggling a football
through four painted Paris streets, rendered live with a lightweight Three.js
brush-stroke shader. It is not a game: no score, no fail state, no stage
interaction (click/drag/hover do nothing). The Settings panel is the real
interaction: you are the art director.

Source: live-browser study 2026-11-12 11:28-11:36 EST, recorded in
`~/workspace/freestyle-basketball/research/paris-freestyle-study.md`, and
Vedel's X post https://x.com/mxvdl/status/2120809570344050705 (posted 4:22 AM
Nov 12, 2026; 191 likes, 8,144 views at time of reading). Saved page PDFs:
`~/workspace/.hatch-browser/outputs/browser-task:403febc9-cd7d-5c81-8e90-03089a0dd5fb/`.

**Facts from the 1:40 breakdown video in that post:**

- 65-joint rig, "mapped to the routine"; IK-solved legs.
- "Every touch plays a note."
- One long background painting per street, "21 769 brush strokes" in the Paris
  one; "The brush runs just ahead of the camera."
- Halftone drop shadow "cast from his real silhouette."
- Trails left by feet and ball; hit frame (occasional slowed stylised freeze,
  figure in brush fragments).
- "Everything is a setting" (stroke size 1.25, density 0.45 shown on screen).
- Four streets: Boulevard Montmartre (golden hour), Quais de Seine
  (afternoon), Pigalle (night), fourth unnamed.
- Look: flat poster colours, ink outline, halftone shadow, white/blue speed
  lines, comic onomatopoeia near the ball ("pok", "poc", "toc", "pow").

**Facts from the live site:**

- 144 BPM beat clock; street changes every 16 beats (~6.7 s).
- Director's-cut camera (24 shots).
- Format switcher: 16:9 / 9:16 / 1:1 / 4:5.
- Palette swatches; real Export MP4 (30 s, 1920x1080, 1600 frames).
- Sound off by default; "Tap for sound" enables a 144 BPM kick/bass loop with
  a note per ball touch.

**Vedel's post text, verbatim (typos as posted):**

> "For the people interested by the @threejs ligthweight brush stroke shader
> and overal animation I posted yesterday, here's a complete breakdown. Still
> fully made with Fable 5.2 @claudeai (Yes you can also create these easily)"

**Self-reply in the same thread:** BRUSH RUSH, a snowboard prototype using the
same shader: "Will fine tune a bit and post a playable version."

**Unresolved detail:** 1600 frames over 30 s is 53.3 fps, not a standard frame
rate. Either the export captures at a non-standard rate, the frame count
covers something else, or one of the two numbers was misread. Flagged here;
see Section 5.

**The driver, in one sentence:** a 144 BPM beat clock drives everything -
streets change every 16 beats, every ball touch plays a note, the background
paints in on the beat, and a Director's-cut camera reframes the same infinite
juggle so one move never feels like one shot.

---

## 1. The creator and his process

### 1.1 Who Max Vedel is

Max Vedel (@mxvdl, X bio "Creator of Spellbound Labs", about 1K followers at
time of reading) is co-founder and Creative Director at **Swipe Back**, a
metaverse creative agency that has worked with Gucci, Nike and Swarovski.

- https://www.windowswear.com/did-metaverse-fashion-week-show-us-the-way-for-virtual-brand-experiences-well-sort-of/
- https://cmgcrypto.com/2022/10/06/fashion-week-hits-the-metaverse/
- https://aws2.campaignasia.com/article/did-metaverse-fashion-week-show-us-the-way-for-virtual-brand-experiences-well-s/476950

He runs Swipe Back with co-founder **Nikhil Roy**. After several years doing
AR, 3D and gaming activations for clients, the pair pivoted fully to the
metaverse following Meta's rebrand; the studio helps brands like Nike, Gucci
and Lego on metaverse strategy.

- https://www.creativereview.co.uk/metaverse-design-architecture-gaming/

His credited campaign work includes **#GlobalPrideCrossing**, an Animal
Crossing: New Horizons campaign crediting Swipe Back with Max Vedel and Nikhil
Roy as Creative Directors.

- https://www.bestadsontv.com/ad/116651/Animal-Crossing-New-Horizons-GlobalPrideCrossing

**His design philosophy, in his own words** (Vogue Business, on Decentraland's
Metaverse Fashion Week, March 2022; original at
https://www.voguebusiness.com/technology/metaverse-fashion-week-the-hits-and-misses,
read via syndication):

> "You don't just want to recreate real life in the metaverse; you want to be
> pushing boundaries and going really crazy with your designs, show spectacles,
> and the actual experience."

> "Someone always has to be first, and by going first you don't always get it
> right. That shouldn't be seen as a negative."

> "Newcomers who are attending on desktops with limited hardware could feel
> slightly deflated by the look of the world."

That first quote is the clearest statement of his creative stance anywhere in
public: spectacle over realism, push the medium. It explains Paris Freestyle
directly - a painted world that could never exist as a photograph, built as a
spectacle piece.

- https://cmgcrypto.com/2022/10/06/fashion-week-hits-the-metaverse/
- https://beautymatter.com/articles/metaverse-beauty-week-2023

### 1.2 "Spellbound Labs": a label, not a verifiable entity

Vedel's X bio says "Creator of Spellbound Labs". No independent source
connects him to any company or studio of that name:

- **spellboundlabs.io** is a real independent game studio ("An independent
  game studio with a dream to make people happy through our work"), building a
  co-op plague-doctor survival horror called Malpractice for PS5 and PC. It
  opened in June 2026 and pitched to PlayStation in July 2026. The site names
  no founders and nothing on it connects it to Max Vedel.
  https://www.spellboundlabs.io/
- **spellboundlabs.ai** is a different company entirely (personalized gift
  books composed from memories). https://www.spellboundlabs.ai/
- Other "Spellbound Labs" results are unrelated: a South African lab-services
  company (formerly EC Lab Services, Port Elizabeth), an MIT Media Lab
  physical game project, a UCSB human-AI lab project, a pre-workout
  supplement, an email-interactivity startup founded by Akshaya Dinesh.

**Conclusion: "Spellbound Labs" is, as far as any public source shows, Vedel's
own label for his solo creative-coding work.** There is no verifiable studio,
team, or company page behind it. Our piece must not use the name or imply any
association.

### 1.3 His pieces (public record)

| Piece | What is known | Source |
|---|---|---|
| Paris Freestyle | Three.js brush-stroke juggling lab, four Paris streets, 65-joint rig, 144 BPM beat clock, live at parisfreestyle.netlify.app | https://x.com/mxvdl/status/2120809570344050705 |
| BRUSH RUSH | Snowboard prototype reusing the same brush-stroke shader; self-reply to the Paris Freestyle post; "Will fine tune a bit and post a playable version" | Same X thread |
| #Vibelog **[UNCONFIRMED]** | Reported as "Eliott & Lily, Space Adventure, an AI game made with my kids" - a Vampire Survivors-like top-down shooter built with Cursor AI, key art via Magnific AI, about 5-6 hours in. The mirror page carrying this could not be re-opened for citation. Treat as a lead, not a fact. | No citable URL recovered |

The pattern across the confirmed pieces: one strong shader/technique, reused
across subjects (football juggling, then snowboarding). The shader is the
product; the sport is the skin. That is exactly the model our basketball piece
should follow - and exceed by grounding the sport properly (Section 3), which
Vedel's juggling piece does only loosely.

### 1.4 What Fable 5.2 is

Fable 5.2 is Anthropic's Claude model, released **9 November 2026** - three
days before Vedel's post. It is the successor to Fable 5.1.

- Official page: https://www.anthropic.com/claude/fable
- Launch-day guide: https://createmotion.io/guides/claude-fable-5-2-animation/
- Uses roundup: https://www.zeniteq.com/5-cool-things-people-use-claude-fable-5-2-for-bb8k7e
- Early-tester report: https://news.sulat.com/2026/11/10/claude-fable-5-2-mythos-devin-early-access-builds/

Key facts:

- API model ID `claude-fable-5-2`. Price $8 per million input tokens, $40 per
  million output tokens (down from $10/$50 for Fable 5.1).
- Anthropic's description: "Our most capable model for technical work." For
  3D and motion: it "reads detailed images and PDFs, builds interactive 3D
  worlds, designs hardware components, creates simulations, animations, and
  educational videos with sound in code."
- **It only outputs text.** The animation is an HTML page, a Three.js scene,
  or a Remotion/HyperFrames project that plays in a browser and renders to
  MP4. It does not generate video frames the way a video model does.
  (createmotion.io)
- Mythos 5.2 is the same model for verified organizations only.
- By launch evening, 30 videos credited to Fable 5.2 by 30 different creators
  had been archived from X (createmotion.io, reported not confirmed).
- The best public example from the previous generation: a developer used
  Fable 5 to build LAAS, a 4x4 km open world running in the browser on WebGPU,
  with the model writing roughly 99% of it with minimal human steering - though
  the human still had to judge things a screenshot cannot show, "like whether
  the camera bob felt right while walking." (zeniteq.com)
- Not to be confused with: the Fable motion-design platform (fable.app,
  founded 2019, Brooklyn) or Fable Studio's AI storytelling tool. Different
  products, same word.

### 1.5 What his pipeline appears to be [DERIVED synthesis]

No public source describes Vedel's pipeline step by step (see Section 5).
What the evidence supports:

1. **He directs; the model writes code.** His post says the piece is "fully
   made with Fable 5.2". Given what Fable 5.2 outputs (code, not video
   frames), the piece is a Three.js program the model wrote under his
   direction, which he then iterated on. He is a creative director using an
   AI coding model the way he would use a technical team.
2. **The character and rig are code-authored.** There is no evidence of
   sculpted models, Blender files, mocap, or downloaded assets anywhere in
   his public record. The 65-joint rig is most plausibly a joint hierarchy
   defined in code, posed by code, with IK solving the legs. Whether the
   65-joint count matches a standard auto-rigged skeleton is an open
   question (Section 5).
3. **He publishes static sites on Netlify** (parisfreestyle.netlify.app) and
   documents with breakdown videos on X. The breakdown video is his process
   documentation - it names the mechanisms (rig, IK, stroke count, halftone
   shadow, settings) but shows no code.
4. **He iterates fast and in public.** Paris Freestyle was posted one day,
   the breakdown the next, and a second prototype (BRUSH RUSH) was already
   in the same thread. Speed is part of the practice - but speed is what
   produced the weaknesses our build must avoid (see 1.6).

### 1.6 What this means for our build

- **The bar is the output, not the tool.** Vedel's advantage is iteration
  speed with a frontier model. Our advantage must be craft: a real move
  vocabulary, real dribble physics, and a look that holds up frame by frame.
  The rejected 2D attempt failed because it had neither his speed nor real
  craft - it was a flat imitation of the surface.
- **Do not try to copy his pipeline.** We do not have his model access, and
  copying would produce the same shallow result. We hand-build in Three.js,
  one stage at a time, verified before continuing (Section 6).
- **Do not copy his words, title, character design, streets, or branding.**
  Model-and-improve: same driving system (beat clock, painted world,
  settings-as-direction), our own city, art, code, and words. This was the
  rule in the original study and it stands.
- **His stated philosophy is our license to be spectacular.** "Pushing
  boundaries... show spectacles" - the piece should be bolder than the
  reference, not a careful copy. Section 6 names the specific ways we exceed
  it.

---
## 2. The brush-stroke shader and the artwork

### 2.1 The academic foundations

Painterly rendering of 3D scenes is a 30-year-old research field, not a
recent trick. Two papers found the whole area:

- **Barbara J. Meier, "Painterly Rendering for Animation", SIGGRAPH 1996,
  pp. 477-484.** The key move: attach brush strokes to 3D surfaces as
  particles, so strokes stick to the objects they paint and stay coherent
  from frame to frame as the camera and objects move. Without this, strokes
  swim across surfaces and the painting falls apart in motion.
  https://doi.org/10.1145/237170.237288
- **Aaron Hertzmann, "Painterly Rendering with Curved Brush Strokes of
  Multiple Sizes", SIGGRAPH 1998, pp. 453-460.** Curved strokes of multiple
  sizes, painted coarse-to-fine: large strokes lay in the big colour masses,
  smaller strokes add detail only where the image needs it. This is why a
  painting reads at a glance and rewards a closer look.
  https://doi.org/10.1145/280814.280951

A later WebGL implementation of Meier's approach (Hanson and Todd, RPI)
represents brush strokes as particles in particle systems, builds "stroke
meshes" out of three.js geometries, and renders in two passes - first the
original scene, then strokes with layer, colour, lighting and depth
information. Their repository is public:

- Paper: https://www.cs.rpi.edu/~cutler/classes/advancedgraphics/S14/final_projects/scott_andy.pdf
- Code: https://github.com/ScottTodd/PainterlyRendering

**The single most important lesson from the literature: strokes must live on
surfaces, not on the screen.** Every approach below is judged by that test.

### 2.2 Four live approaches in Three.js

There are four genuinely different ways to get a brush-stroke look in
Three.js. They are not interchangeable; they trade off against each other on
coherence, control, and cost.

#### Approach A: Kuwahara post-processing (screen space)

The Kuwahara filter is an edge-preserving smoothing filter, originally for
removing noise from medical images. For each pixel: centre a box on it, divide
the box into 4 sectors, compute the average colour and variance of each
sector, and set the pixel to the average colour of the lowest-variance
sector. The smoothing erases texture detail; the sector choice preserves (and
sometimes sharpens) edges.

The definitive write-up is Maxime Heckel's "On Crafting Painterly Shaders"
(Oct 2024), which implements it as a custom post-processing pass over a
React Three Fiber scene and covers the improvements from the literature:
Papari's extension (circular kernel, 8 sectors) and the anisotropic variant,
which aligns the sampling area with the local edge direction.

- https://blog.maximeheckel.com/posts/on-crafting-painterly-shaders/

Downstream work:

- **Susurrus** (Codrops, Apr 2026): a cozy watercolor world in Three.js where
  the Kuwahara shader is the ONLY post-processing pass, and "everything is
  built around it."
  https://tympanus.net/codrops/2026/04/24/susurrus-crafting-a-cozy-watercolor-world-with-three-js-and-shaders/
- **React Postprocessing** ships an anisotropic generalized Kuwahara effect:
  "A painterly filter that smooths flat areas into brush-like strokes while
  keeping edges sharp." Cost warning: about `radius * sectorCount * 3`
  texture samples per pixel, so large radii get expensive fast.
  https://react-postprocessing.docs.pmnd.rs/effects/kuwahara

**Limitation, stated bluntly by Paintify** (Shiva V., Sep 2026): "Kuwahara
flattens color but has no texture, so it never looks like strokes." Paintify
started from Hertzmann's 1997/1998 stroke algorithms instead, for exactly
that reason.

- https://shivav.com/writing/paintify-making-3d-feel-painted.pdf

**Verdict for our build:** Kuwahara is the cheapest way to make a 3D scene
read as painted, and it is a legitimate foundation - but on its own it
produces flat colour regions with no stroke texture. It needs a stroke layer
on top (Approach B, C, or D) to read as brushwork rather than a blur filter.

#### Approach B: PainterlyPass (depth-rebuild post-processing)

PainterlyPass (minchenlee/three-painterly, created Nov 5 2026, MIT, three.js
r160+, npm `painterly-pass`) is a post-processing pass that repaints any
three.js scene in impressionist brush strokes. It replaces RenderPass: it
renders the scene itself, rebuilds world positions from the depth buffer, and
lays strokes on each surface's own plane, "so strokes stay on the building,
tree or ground when the camera moves." No mesh changes. It does its own tone
mapping and colour-space output, so no OutputPass after it.

- https://github.com/minchenlee/three-painterly
- https://discourse.threejs.org/t/painterlypass-bakeddebris-painterly-post-processing-and-rewindable-explosion-debris/96674

Its stated prior art is Meier 1996 and Sekuta82's per-mesh template (Approach
C). The author notes it was built with Claude Code ("I steered and tested; it
wrote most of the code") and the repo ships an llms.txt.

**Verdict for our build:** this is the closest public equivalent to what
Vedel describes as a "lightweight brush stroke shader" - a single pass, no
mesh changes, strokes that stay on surfaces. It is the leading candidate for
our stroke layer, to be evaluated in Stage 4 against a hand-rolled alternative.

#### Approach C: per-mesh instanced strokes (object space)

The fullest public breakdown of this approach is Gabriel de Laubier's
"Stylized paint shader breakdown" (2023, UE5, no post-processing - a
deliberate choice so several styles could mix in one scene).

- https://cyn-prod.com/stylized-paint-shader-breakdown

Its mechanisms, each directly portable to Three.js:

- **One packed texture** (made in Substance Designer): a normal map on R+G,
  two brush masks on B+A. The diffuse stroke is a blurred square mixed with a
  grunge map, scattered with random grey values. The normal map scatters the
  same shape but with the normal vector "randomized quite strongly, like each
  paint stroke was pointing in a different direction completely."
- **Diffuse:** the B channel interpolates two input colours; BumpOffset
  (cheap parallax mapping) gives the strokes depth. BumpOffset "can break
  down at shallow angles or high height values, but this actually works to
  our advantage in this case."
- **Toon lighting:** stepped gradient with values 0, 0.25 and 1 from the dot
  product of world normal and sun direction - three hard bands.
- **Reflections:** dot product of light vector and reflection vector, lerped
  between colours, with swizzled UVs so the brush pattern differs from the
  diffuse. "You can see the colors dancing when the object moves and it feels
  quite liquid and oily."
- **Occlusion:** mesh signed-distance-field ambient occlusion, eroded by the
  alpha-channel brush mask (the most expensive part; behind a static switch).
- **The fresnel problem:** cheap fresnel (dot of world normal and view
  direction) gives false edges on flat faces at shallow angles. His fix: bake
  the mesh's smooth normals into vertex colours in Blender (smooth shading,
  bake normals to vertex colours, revert to auto-smooth), then read flat or
  smooth normals at will in the shader with no extra sampler.
- **Rim erosion:** a mask that chips away at edges with a brush texture
  without disturbing the silhouette. Critically, he does NOT project the
  texture in screen space ("for an interactive camera, the effect really
  falls apart as the user can quite clearly feel how the texture is
  projected when pivoting and panning") - he uses the camera vector's X and Y
  components for a pseudo-view mapping that stays static while pivoting.
- **Curvature filter:** feed normals into dFdx/dFdy (screen-space
  derivatives); flat faces return 0, curved areas return the normal
  difference. This stops the erosion mask from eating whole flat faces.
- **Outlines:** the classic inverted-hull technique (duplicate the mesh,
  invert normals, inflate along the normal), done without extra draw calls by
  instancing the mesh 4 times with a per-instance index (0 base, 1 rim, 2 for
  the two outlines). The smooth vertex-colour normals are reused for
  inflation, which fixes the sharp-geometry breakdown. Each outline instance
  gets a random offset "to get the effect of approximate strokes."
- **Shadows:** override opacity during the shadow pass with the same eroded
  rim effect, more pronounced - stylised shadows for free.

A Three.js port/extension exists: SeloSlav/threejs-stylized-paint-shader
(Three.js 0.185.1, MIT) - instanced brush strokes permanently attached to
meshes (seeded surface position, normal, size, direction), deterministic, no
paint post-process, ACES tone mapping.

- https://github.com/SeloSlav/threejs-stylized-paint-shader

And the per-mesh prior art PainterlyPass cites is Sekuta82's template:

- https://github.com/Sekuta82/oil-on-threejs-template

**Verdict for our build:** Approach C gives the most control and the most
genuinely painted result (real stroke direction, real erosion, real
outlines), at the cost of the most work: every mesh needs the treatment. It
is the right approach for the hero character and the ball. Approaches B and C
can combine: C on the hero, B on the world.

#### Approach D: strokes as geometry

Strokes can also be real 3D objects - ribbon meshes with a custom ink shader,
or particle-system stroke meshes as in the RPI paper (2.1). This is the most
expensive approach and the most literal: the painting is geometry. It suits
trails and accents (brush streaks behind feet and ball, paint smears) better
than whole-scene coverage.

### 2.3 Toon shading, outlines, halftone

The painted look needs three more systems, all standard in three.js:

**Toon/cel shading.** Quantise the lighting term into a few hard bands
instead of a smooth ramp. Three.js ships `MeshToonMaterial` with an optional
`gradientMap` (NearestFilter, no mipmaps) controlling the steps.

- https://threejs.org/docs/pages/MeshToonMaterial.html
- https://threejs.org/examples/webgl_materials_toon.html
- https://jargon-buster.sivaram.dev/3d-webgl/toon-shading-cel-shading/

Polyfork's shader survey adds a caveat nobody mentions: toon banding "does
very little on flat-shaded geometry" - every face is already one flat tone,
so banding mostly removes the small tonal differences that were making the
form readable. On flat-shaded low-poly, drop the lighting model entirely
(unlit, pure albedo) and let the outline do the work.

- https://polyfork.dev/blog/six-shader-looks-and-which-ones-you-can-keep
- https://polyfork.dev/threejs-shaders

**Inverted-hull outlines.** The standard trick: duplicate the mesh, push
every vertex out along its normal, draw only the back faces. The copy shows
where it sticks out past the silhouette and is hidden everywhere else.
Three.js ships this as the `OutlineEffect` addon.

- https://threejs.org/docs/pages/OutlineEffect.html

Three refinements from Polyfork: use smoothed normals for the extrusion
(hard-edged models tear the hull open at corners, three faces pushing three
directions); put the width in pixels, offset in view space scaled by depth,
so the outline holds at any distance; and remember the outline survives
skinning (a hull bound to the same skeleton as the model).

**Variable line weight and halftone shadows.** The Descent project brief
(a cel-shaded Three.js BMX game) names the two details that separate "toon
shader" from "drawn": modulate outline thickness by surface curvature and
distance so lines taper and thicken like a hand-drawn stroke ("This one
detail is the difference between 'toon shader' and 'drawn'"), and in shadow
bands blend in a screen-aligned hatch or halftone dot pattern at low opacity
"the way a cel background painter fills a shadow... it should read as
texture, not as a filter." It also pairs the inverted hull with a second
line system - a screen-space Sobel edge detect over a normal+depth prepass
for interior lines the hull cannot produce.

- https://www.sameprompt.io/prompts/descent/

Halftone dots as a shadow treatment (mapping shadows to a halftone dot
texture instead of flat colours) is also documented as a manga-style
technique:

- https://github.com/priyanshuf1/rabto/blob/HEAD/skills/anime-cel-shading/SKILL.md

This is directly how Paris Freestyle's "halftone drop shadow cast from his
real silhouette" works: the shadow is the character's silhouette shape,
filled with a halftone dot screen instead of flat darkness.

### 2.4 What makes a frame read as a painting, not a filter [DERIVED]

Synthesized from the sources above. A frame reads as a painting when:

1. **Strokes live on surfaces.** They stay on the building/tree/ground when
   the camera moves (Meier; PainterlyPass). Screen-space texture that swims
   on camera moves is the single most reliable tell of a filter (de Laubier).
2. **Colour comes in flat regions with hard edges.** Kuwahara's sector
   averaging, toon banding, and Hertzmann's coarse-to-fine strokes all do the
   same thing: big masses of flat colour, detail only where it earns its
   place.
3. **Strokes have texture, direction, and size variation.** Flat colour with
   no stroke texture "never looks like strokes" (Paintify). Strokes need a
   visible dab shape, a direction (ideally following the surface or the form),
   and multiple sizes.
4. **Silhouettes are broken, not perfect.** Rim erosion chips the edges with
   brush texture (de Laubier). A mathematically perfect silhouette reads as
   3D; a slightly eaten one reads as paint.
5. **Lines vary in weight.** Uniform outlines read as a shader; lines that
   taper and thicken with curvature and distance read as drawn (Descent).
6. **Colour moves.** Reflections that make "the colors dancing when the
   object moves... liquid and oily" (de Laubier) sell the paint more than any
   static texture.

A frame reads as a filter when any of these fail: swimming screen-space
texture, flat colour with no stroke grain, perfect silhouettes, uniform line
weight, dead colour. The v1 New York Freestyle backgrounds failed on exactly
this axis - a uniform horizontal dash texture flattened everything into
streak noise. The v2 fix ("structure first, strokes second") is the same
lesson from the other direction: solid washes carry the structure, strokes
are sparse texture only. (See paris-freestyle-study.md, v2 rework section.)

### 2.5 The unrolled painting: how "one long painting" works technically

The background technique has a direct public analogue. Aduok's infinite-loop
gallery (Jul 2026) renders a chain of images on a single ribbon of geometry
with no infinite array of meshes and no teleporting camera:

- Every source image is packed edge-to-edge into **one wide canvas texture**,
  so the GPU only ever samples a single image. The canvas becomes one
  `CanvasTexture` with `RepeatWrapping` on the horizontal axis.
- The gallery is **one PlaneGeometry**, subdivided into hundreds of segments
  along its length; vertices are reprojected by pure math (in that piece,
  into a helix).
- A fragment shader reads a **scrolling offset and wraps it with `fract()`**,
  which turns a finite strip into something that feels endless. "There is no
  infinite array of meshes... the sense of endless scroll comes entirely
  from a shader that wraps one UV coordinate back on itself."

- https://www.aduok.in/blog/how-to-build-an-infinite-loop-gallery

A companion piece (Aug 2026) bakes a tall sequence of photos into one canvas
band and lets a fragment shader decide, per pixel, which part of the baked
canvas to sample from a scroll offset. The key separation: "the canvas band
is built once... and never redrawn per frame - it is pure image composition.
The shader never knows how many photos exist or what order they are in; it
only knows a scroll offset and a texture to sample."

- https://www.aduok.in/blog/how-to-build-image-wave-scroll

**[DERIVED] Mapping to Paris Freestyle:** "one long painting, unrolled" is
this technique with a painting instead of photos. One wide canvas texture
(the street, painted once in code), stretched across a long plane (or a
gently curved ribbon), sampled with a scrolling UV offset driven by the beat
clock. "The brush runs just ahead of the camera" is a reveal: a shader
uniform wipes the painting in by world position, with a leading brush edge a
fixed distance ahead of the visible window - new paint appears just before
the camera reaches it. Street changes are a texture swap (or a jump to a new
ribbon segment) on a 16-beat boundary, with the reveal restarting.

### 2.6 Painting and illustration traditions the style draws on

- **Impressionist brushwork.** PainterlyPass describes itself as repainting
  scenes "in impressionist brush strokes" - visible dabs, broken colour,
  light over line. The stroke layer of our piece belongs to this tradition.
- **Poster / flat-colour illustration.** Flat poster colours, ink outline,
  hard colour masses - the toon + outline layer. The reference's look is
  closer to a poster than to an oil painting; the brush strokes sit on top of
  poster-flat structure.
- **Print halftone.** The dot screen of analog print, used for shadows. The
  reference's halftone shadow is a print reference, not a painting one -
  it says "comic book" as much as "canvas".
- **Comics.** Onomatopoeia ("pok", "poc", "toc", "pow"), speed lines, hit
  frames with the figure in fragments. These are comics devices, and they do
  real work: they mark the beat, they mark the touch, they give the eye a
  place to land on the accent moments. Our piece needs its own set (not his
  words).

---
## 3. Freestyle basketball movement

### 3.1 Two traditions: streetball handles and freestyle performance

Basketball freestyle has two parent traditions, and the piece needs both.

**Streetball** is the playground game where style and flair take precedence
over rules. It traces its roots to Harlem in the 1940s, when Holcombe Rucker,
a teacher and playground director, started organizing tournaments to engage
local youth. In 1950 he founded the New York City Pro-Am Tournament; by 1954
the competitions had a permanent home at PS 156, a small park between 155th
Street and Frederick Douglass Boulevard. In 1974 the city renamed it Rucker
Park. In 1982 Greg Marius launched the Entertainers Basketball Classic (EBC)
there, blending streetball with hip-hop and bringing NBA stars and streetball
legends onto the same court.

- https://triplethreatstreetball.com.au/what-is-streetball-culture/

The 1990s took streetball global. AND1, founded in 1993 by Wharton graduate
student Seth Berger, found a tape of Rucker Park streetball in its office,
mixed it with unreleased music, and distributed "AND1 MixTape Vol. 1" for
free around Rucker Park. In 1998 they gave away 50,000 "Skip Tapes" to anyone
who tried on shoes at Footaction. The AND1 Mixtape Tour followed - exhibition
games touring New York, New Jersey, Pennsylvania, Washington and Georgia,
signing players like Skip 2 My Lou, Main Event, 1/2 Man 1/2 Amazing, and Hot
Sauce. In 2003 ESPN built a talent-hunt show around the tour; its first find
was Grayson "The Professor" Boucher.

- https://dmtalkies.com/untold-the-rise-and-fall-of-and1-ending-explained-2022-documentary/
- https://faintflex.substack.com/p/faintflex-vol-18-and1s-reign

The legends to know: **Rafer "Skip 2 My Lou" Alston** (went from mixtape fame
to 11 NBA seasons, including a 2009 NBA Finals appearance with the Orlando
Magic), **Philip "Hot Sauce" Champion**, and **Grayson "The Professor"
Boucher**. The through-line for our piece: streetball is handles as
performance - the dribble is the show, the crowd is the judge, and the moves
are named, repeated, and owned by the people who made them famous.

The Paris connection matters too: **Quai 54**, founded in Paris in 2003 by
Hammadoun Sidibe, is France's own streetball institution - the French
equivalent of Rucker Park. Vedel's Paris piece sits in that lineage whether
he names it or not.

- https://nobackboardbasketball.substack.com/p/streetball-culture-quai-54-and-growth

**Freestyle performance** is the second tradition: ball control as a solo
discipline - finger spins, ball juggling, body wraps - judged on control,
variety, and flow rather than on beating a defender. The two traditions share
a vocabulary (Section 3.2) but differ in purpose: streetball handles are
weapons against a defender; freestyle tricks are display for their own sake.
Our piece is freestyle performance built from streetball handles - the
routine is a display, but the moves must be real moves, not invented
flourishes.

### 3.2 The movement vocabulary

Every move below is a real, named move with documented mechanics. The routine
(Section 6, Stage 2) must be built from this list, not from invented motion.

| Move | Mechanics | Source |
|---|---|---|
| Crossover | Ball driven low across the body from one hand to the other, lower than other dribbles, close to the body | http://www.basketball4all.net/dribbling.html |
| Between-the-legs | Ball passed through the legs to the other hand, on the bounce or on the pass | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |
| Behind-the-back | Ball guided behind the back to the opposite hand with wrist and fingertips, "like passing the ball to yourself" | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |
| Around-the-world (behind-the-back) | Dribble the ball around the body, switching hands behind the back each time | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |
| In-and-out | Fake one direction with the hand over the ball, pull it back the same side | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |
| Push cross | Low push dribble across the body | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |
| Spin move | Dribble, then spin the body while the ball is on the fingertip; can be stationary, or dribble-spin-continue-dribbling | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |
| Retrieval dribble | Ball dribbled up and away from the body, then quickly brought back to the hand with a spin or flick of the wrist | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |
| Figure-8 | Ball dribbled as fast as possible in a figure 8 through and around the legs | http://www.basketball4all.net/dribbling.html |
| Finger spin | Ball spinning on one fingertip (see 3.4) | See 3.4 |
| Ball juggling | Tossing the ball up and catching it in different ways, spinning on fingers, balancing on the forearm; toss hand to hand in front, then behind the back | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |
| Control dribble | Athletic stance, knee-to-waist-high dribble, arm bar to protect the ball, head up | http://www.basketball4all.net/dribbling.html |
| Speed dribble | Hand behind the ball, push it in front, then catch up; waist-to-chest-high | http://www.basketball4all.net/dribbling.html |
| Dribble walk | Dribbling while walking, keeping the rhythm alive through footwork | https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929 |

Two coaching notes that matter for the animation. First, the dribble is felt
with the fingertips, not the palm, and low dribbles sit "just a few inches
off the ground." Second, the point of ball handling is not to perform as many
moves as possible - it is to maintain possession while positioning ball and
body for the next action. The routine must read as control, not as flailing.

- https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929
- https://www.houseofjustus.com/the-dribble-cycle/

### 3.3 The dribble cycle and timing

The most useful model for the build is the Ball Handling Cycle (House of
Justus, Sep 2026), which treats every dribble as two alternating phases:

- **Control Phase:** the hand is in contact with the ball and can actively
  influence its position, direction, speed, and trajectory. Within it, three
  actions: **Pull** (bring the ball to the control position), **Organize**,
  **Push** (direct the ball away toward its next path). The objective is not
  to hold or carry the ball.
- **Free Phase:** begins when the ball leaves the hand and ends when contact
  is re-established. The ball is no longer under direct physical influence
  from the hand; it travels on its own trajectory (including the floor
  bounce) while the player reorganizes for the next interaction.

The full cycle: Re-Contact -> Pull -> Organize -> Push -> Release -> Free
Phase -> Re-Contact. "The transition between phases is continuous rather than
separate."

- https://www.houseofjustus.com/the-dribble-cycle/

Timing has three distinct concepts:

- **Dribble Rhythm:** the recurring temporal pattern across consecutive
  dribble cycles.
- **Dribble Tempo:** the rate at which complete dribble cycles repeat.
- **Control Tempo:** the relative timing of the Pull and Push actions within
  the Control Phase, expressible as a Pull:Push ratio (1:1 at foundation
  level). Skilled players manipulate rhythm - changing pace, delaying,
  reaccelerating - while keeping the cycle intact.

- https://www.houseofjustus.com/dribble-rhythm-and-control-tempo/

The cited research base is Katsuhara et al. 2010, "Spatiotemporal
characteristics of rhythmic, stationary basketball bouncing in skilled and
unskilled players" (Perceptual and Motor Skills 110(2):469-78,
doi:10.2466/PMS.110.2.469-478), which measured how skilled and unskilled
players differ in rhythmic stationary bouncing.

**[DERIVED] Bounce timing at 144 BPM.** A basketball in free fall obeys
t = sqrt(2h/g). With g = 9.81 m/s^2 and a restitution of about 0.8 on the
bounce:

- Waist-height dribble (h ~ 0.9 m): ~0.43 s down, ~0.34 s back up, full
  cycle ~0.77 s.
- Low dribble (h ~ 0.45 m): ~0.30 s down, ~0.24 s up, cycle ~0.55 s.
- At 144 BPM one beat is 417 ms. So a waist-height dribble is ~1.85 beats
  and a low dribble is ~1.3 beats.

Consequence for the routine: **dribbles do not land one-per-beat at 144 BPM.**
A routine that puts one dribble on every beat is physically wrong - the ball
cannot fall and return that fast from waist height. The honest mapping is:
touches land ON beats, but a dribble cycle spans roughly two beats (one beat
down, one beat up, with the hand contact at the beat), and fast low dribbles
can run closer to one per beat. Crossovers and hand changes happen at the
beat, during the Control Phase. This is a place our piece can be visibly more
correct than the reference, whose juggling touches are not bound by any
equivalent physics.

### 3.4 The finger spin

The finger spin is the signature freestyle moment - the one the crowd waits
for - and it has precise, documented mechanics:

- **Initiation:** hold the ball in both hands with the seams horizontal,
  dominant hand under the ball, fingers spread, thumb on one side; the
  non-dominant hand on top provides stability. Snap the dominant wrist
  forward while the non-dominant hand rolls the ball backward. The goal is a
  fast rotation around the ball's vertical axis.
- **Speed:** aim for at least **two to three full rotations per second**.
  The faster the ball rotates, the more stable it becomes, due to the
  gyroscopic effect.
- **The finger is a platform, not a motor.** The spinning motion comes from
  the wrist and the other hand, not from the supporting finger itself. The
  finger is a stationary pivot point.
- **Maintenance:** tap the ball periodically to replenish spin as friction
  slows it.
- **Variations:** the "snatch" (toss the spinning ball into the air and
  catch it back on the same finger); the behind-the-back transfer (sweep the
  arm behind the back while the ball spins); switching fingers (index to
  middle).

- https://gitlab.hesconet.com/IT/slideshare/=ImRwdlV6Z1aidkSYRVb1olVzIkRUZlUhJVbGlXVsRWaOdkUHZ1asdlVrVDWWtmWs5EbaRXVtR3dWxGbYNmRWh1VEZFdVFjQrJmRKpXVsZVVWpnVyZ1V0NVYxYlWiVkWYFmesJnVup0bitWMXV2RxgFVqZkcahlTvZlVZpXTWJVU

The competitive benchmark: **Wassim "Wass" Benslimane** (Algeria) holds the
Guinness record for most globe ball tricks in one minute - **34**, set in
Paris on 29 September 2009. A globe trick is spinning the ball on one finger
and twisting the arm so the ball passes under that arm and back to its
starting position. Benslimane is a football and basketball freestyler; he
used a football for the record.

- https://www.guinnessworldrecords.com/world-records/most-globe-ball-tricks-in-one-minute

Contemporary freestyle continues on social platforms - e.g. KalaniBallFree
(@kalaniballfree, 490K followers), spinning two balls simultaneously on two
index fingers, stacking one ball on another balanced on a finger.

- https://www.instagram.com/reel/DdH5pyjvVWj/

**Animation consequences:** the finger spin is a held pose, not a fast move -
the ball rotates at 2-3 rev/s on a stationary fingertip while the body is
free to move (walk, turn, transfer). It is the natural "signature moment" of
a routine: everything slows, the ball spins, the camera closes in. The
behind-the-back transfer and the snatch are the two best escalation moves
from it.

### 3.5 What a routine looks like

The sources support a clear routine shape:

- **Foundation first.** "Drill these until they feel as natural as
  breathing. Integrate them into your warm-ups, your cool-downs, and
  especially into your freestyle practice sessions." A routine opens on
  established rhythm - control dribbles, crossovers - before anything fancy.
  https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929
- **The 30-second freestyle drill** is the practice form of a routine:
  "Dribble for 30 seconds in a confined area, using as many moves and
  combination of moves as possible... Mix up your dribble moves (crossover,
  inside out, between the legs, behind the back, etc). Change the speed and
  the 'rhythm' of the dribble, change the combination of the dribble moves."
  http://www.basketball4all.net/dribbling.html
- **Rhythm manipulation is the skill.** Change of pace - acceleration,
  deceleration, delay, reacceleration - coordinated with dribble rhythm is
  what separates a routine from a drill. The House of Justus curriculum
  treats changing pace as its own topic, coordinated with the dribble cycle.
  https://www.houseofjustus.com/
- **Two-ball work** is the advanced layer: simultaneous same-height dribbles,
  alternating piston dribbles (one ball up while the other is down),
  high-low splits. A second ball is a possible escalation for a later stage,
  not for v1.
  https://www.sixsevenball.com/drills/two-ball-rhythm

**[DERIVED] Routine shape for the build** (to be detailed in Stage 2):
establish rhythm (4 beats of control dribble) -> escalate through hand
changes (crossover, between-the-legs, behind-the-back, 8 beats) -> signature
moment (finger spin with a transfer or snatch, 8 beats) -> resolve (return
to rhythm, 4 beats) -> loop. 24 beats at 144 BPM is 10 seconds per routine
cycle; streets change every 16 beats, so routine and street cycles phase
against each other instead of lining up - the same move lands in different
streets, which is exactly the variety the Director's-cut camera exploits.

### 3.6 Notable freestylers (quick reference)

- **Wassim "Wass" Benslimane** - Guinness globe-trick record, Paris 2009.
  https://www.guinnessworldrecords.com/world-records/most-globe-ball-tricks-in-one-minute
- **Rafer "Skip 2 My Lou" Alston, Philip "Hot Sauce" Champion, Grayson "The
  Professor" Boucher** - AND1 streetball legends (Section 3.1).
- **Harlem Globetrotters** - popularizers of the finger spin as a crowd
  trick, alongside NBA stars, per the spin guide (Section 3.4).
- **KalaniBallFree** - contemporary two-ball freestyler, 490K followers.
  https://www.instagram.com/reel/DdH5pyjvVWj/

---
## 4. The 3D build approach

### 4.1 The character: procedural vs sculpted

Two routes exist for a stylized rigged player in Three.js. Both are real;
the choice is a Stage 1 decision, and the research supports either.

**Route 1: procedural geometry + code rig.** Build the player from primitives
in code (capsules, spheres, boxes, custom lathe/extrude shapes), define the
joint hierarchy in code, pose it in code. Advantages: zero external assets,
total control over the stylization, the rig and the routine are the same
system (the routine IS the pose program), and it matches how Vedel's piece
was most plausibly built (Section 1.5). Disadvantages: hand-authoring 50+
joints and their skinning weights in code is slow, and organic deformation
(elbows, shoulders, hips) is the hard part.

**Route 2: sculpted model + auto-rig + GLB.** Model (or generate) a character
in Blender, auto-rig it, export GLB with skin and clips, play it in Three.js
via AnimationMixer. Auto-rigging options, current as of Oct 2026:

- **Mixamo** (Adobe): bipedal humanoids only, FBX output, free with an Adobe
  ID; characters and animations royalty-free, video games included.
- **Meshy**: textured humanoids up to 300,000 faces, FBX and GLB output, with
  walk and run clips.
- **Tripo**: biped/quadruped/hexapod/octopod/avian/serpentine/aquatic, GLB or
  FBX, Mixamo bone names on request - which matters because three.js finds a
  clip's bones by name, so a clip made for one Mixamo-rigged character plays
  on the next one too.

- https://gamesbyai.win/guides/ai-3d-animation-for-games/

A browser-local auto-rigger also exists (nff747/auto-rig-web: ONNX vision
model in the browser, 19-bone skeleton, CCDIK solver), for the case where no
external service is wanted:

- https://github.com/nff747/auto-rig-web

**Recommendation [DERIVED]:** Route 1 for the hero, because the routine is
the piece. A Mixamo skeleton is built for locomotion clips, not for a
beat-mapped freestyle routine with finger spins and hand changes; retargeting
generic clips onto it would produce generic motion, which is precisely what
Hans rejected. Route 1 keeps the rig, the routine, and the ball in one
coherent program. Route 2 is the fallback if hand-authoring the rig proves
too slow - in which case the model must still be re-rigged/posed by our own
routine code, not by stock clips.

### 4.2 Rigging, skinning, skeletal animation: the mechanics

The three stages, precisely defined:

- **Rigging** builds the skeleton: a hierarchy of bones (an armature in
  Blender) plus the handles an animator grabs, like IK targets on hands and
  feet.
- **Skinning** binds the mesh to that skeleton by giving every vertex weights
  across nearby bones, usually up to four - the part artists do by weight
  painting.
- **Skeletal animation** is keyframing bone transforms over time; the vertex
  shader blends each vertex by its weights every frame.

glTF carries all of this natively as skins plus animation clips, so a Mixamo
or Blender export plays in three.js through AnimationMixer with crossfades
between clips. For faces and squishy shape changes, morph targets (blend
shapes) are the better tool; real productions use both together.

Two gotchas that will bite this build:

- **Candy wrapper collapse:** linear blend skinning pinches a wrist or
  forearm to nothing when it twists past 90 degrees. The fix is a twist bone
  that takes half the rotation, not more weight painting. (Directly relevant:
  the finger spin and behind-the-back transfer both twist the wrist hard.)
- **Retargeting only works if bone names and proportions roughly match.**

- https://jargon-buster.sivaram.dev/3d-webgl/rigging-skinning-skeletal-animation/

**Playback in three.js** (the standard pattern, from three.js's own blending
example):

- One `AnimationMixer` per character; each clip becomes an `AnimationAction`
  you can start, stop and crossfade. Crossfade over ~0.3 s with
  `fadeIn`/`fadeOut`. Drive `mixer.update(delta)` from a `THREE.Clock` in
  the render loop.
- **Procedural bone control runs AFTER `mixer.update()`**, or a clip that
  animates the same bone overwrites it. This is the key ordering rule for
  our routine: the beat-mapped routine poses bones procedurally each frame,
  after any clip playback.
- **Attach props by parenting to bones:** `handBone.add(ball)` makes the
  ball follow the hand's bone transform. (See 4.3 for why we use this only
  during the Control Phase.)
- Clone skinned models with `SkeletonUtils.clone`, never plain `clone()`,
  or the copy stays bound to the original's bones. Use `SkeletonHelper` to
  visualize the rig while debugging. Retarget clips with
  `SkeletonUtils.retargetClip` when bone names/axes differ.

- https://gamesbyai.win/guides/ai-3d-animation-for-games/
- https://www.ui-skills.com/skills/cloudai-x/threejs-animation

**IK.** The reference solves its legs with IK, and ours must too - feet must
plant on the ground while the body moves through the routine. The standard
technique is **two-bone IK** (analytic solution for a two-segment limb given
a target position), listed alongside per-bone additive layers, aim damping
and foot placement in current three.js starter kits:

- https://github.com/vibegameengine/web-starter-kit

Plan: IK targets on both feet (planted during stance, lifted during steps
and spins) and IK targets on both hands during ball contact (Section 4.3).
The spine, head, and free arm are posed by the routine program.

**Joint budget [DERIVED]:** the reference has 65 joints. Our rig should
match or exceed that where it matters: fingers need joints (at least 2 per
finger on the spinning hand, to shape the fingertip platform and the wrist
snap), the spine needs 3-4 joints (the routine's body rotation lives there),
and each wrist needs a twist bone (candy-wrapper fix). Target: 55-70 joints
total. Fewer than 50 and the hands will read as mittens; more than 70 and
the routine program gets unwieldy.

### 4.3 Driving the ball: physics arcs vs keyframed paths

Neither pure physics nor pure keyframing is right. The correct drive is a
hybrid mapped directly onto the Ball Handling Cycle (Section 3.3):

- **Control Phase (hand on ball):** the ball is driven by the hand. Parent
  it to the hand bone (`handBone.add(ball)`) or lerp its position to the hand
  each frame. The hand's Pull/Organize/Push actions ARE the ball's motion.
  Hand changes (crossover, behind-the-back, between-the-legs) happen here, at
  beat boundaries, by re-parenting or re-targeting the ball to the other hand
  during the contact window.
- **Free Phase (ball in the air/on the bounce):** the ball is ballistic. A
  parabola under gravity (g = 9.81), with a floor bounce at restitution
  ~0.8, computed from the release position and velocity the Push action set.
  The release velocity is chosen so the ball arrives at the next hand contact
  on the next beat - which is solvable exactly (projectile motion with a
  fixed flight time), not faked.
- **Finger spin:** the ball sits on the fingertip (parented to the fingertip
  bone, offset by the ball radius along the finger axis) and rotates at 2-3
  rev/s around its vertical axis. The body is free to move underneath it.
  The snatch is a short Free Phase (toss up, catch back on the same finger).

**[DERIVED] Why the hybrid beats either pure approach:** pure physics cannot
hit beats reliably (a physics sim drifts off the clock); pure keyframing
cannot look physical (hand-tuned arcs never match gravity, and the eye
catches it - a ball that falls wrong is the fastest way to break the piece).
The hybrid gives exact beat timing (the Free Phase flight time is set by the
beat grid) with exact physics (the arc is a real parabola). The acceptance
criteria in Stage 3 test both.

Ball spin matters too: a dribbling ball rotates (backspin off the fingertips
on release, topspin after the bounce). The ball mesh needs visible seams so
rotation reads; a featureless orange sphere spinning at 2-3 rev/s looks
static.

### 4.4 The painted city background in 3D

From Section 2.5: one wide canvas texture per street, painted once in code
on an offscreen 2D canvas (structure first as solid washes, strokes as sparse
texture - the v2 lesson), stretched across a long plane or gently curved
ribbon, sampled with a scrolling UV offset driven by the beat clock, with a
`fract()` wrap for the loop. The reveal wipe ("brush runs just ahead of the
camera") is a shader uniform keyed to world position.

Structure per street, following the v2 "name the place" rule: each street
must be identifiable from its silhouette and landmark structure alone, before
any stroke texture is applied. Candidate NYC streets (from the original study;
revisit in Stage 5): Washington Square Park (golden hour), DUMBO / Brooklyn
Bridge (afternoon), SoHo cast-iron (dusk), Coney Island boardwalk (night).
If a location proves hard to read as paint, swap it for another unmistakable
NYC scene and note the swap.

The ground plane is part of the painting: converging seams, sparse dashes,
no full-texture noise (the v2 `groundPlain` rule). The halftone shadow is a
separate system (Section 2.3): the character's silhouette projected onto the
ground, filled with a halftone dot screen.

Depth layering: the reference is essentially a stage - character on a ground
plane, painting behind. Keep that. Do not build a 3D city; build a 2.5D
stage. The painting is flat; the character is 3D; the camera moves in 3D
around the character while the painting unrolls behind. That contrast - flat
world, round player - is the look.

### 4.5 The camera: a director, not a track

The reference's Director's-cut camera (24 shots) has direct public analogues
to build from:

**PolyFish's cinematic camera** (three-layer architecture):

- **Layer 1 - DocumentaryDirector (editorial brain):** decides what shot to
  take and when. A narrative state machine with five phases (ESTABLISHING,
  INTRODUCE, DEVELOP, CLIMAX, RESOLVE), each with a weighted pool of shot
  types; duration variance of +/-20% per shot (seeded, so deterministic but
  organic).
- **Layer 2 - Cinematographer (camera operator):** given a target and a shot
  type, computes the exact camera position, look-at point, and field of view.
  Applies smoothing, velocity damping to prevent jitter, and direction
  continuity to avoid 180-degree flips. Eleven shot types.
- **Layer 3 - transitions:** velocity damping smooths between shots over
  0.5-1.0 s. The key insight: store the camera's velocity, not just its
  position, and blend the velocity toward the new direction with a spring
  damping coefficient (0.1-0.2) to prevent bouncing and overshooting.
- Depth of field via BokehPass, with focus distance animated smoothly so
  focus tracks the subject.

- https://polyfish.unclemattmakes.com/making-of/camera

**threejs-previz** (cinematic previz in code): a camera-move catalog where
each move is a function `move(u) -> {position, target, fov, roll}` and the
timeline maps frame -> shot -> u -> camera. Critically, every shot is
**measured, not eyeballed**: eight audits per shot, including whether the
hero is in frame, whether the hero is visible, whether the camera clears the
geometry, and whether the path jumps.

- https://github.com/DevMartinese/threejs-previz

**Transition grammar** (video-illustrator references):

- **Hard cut is the default.** Most seams are cuts on a beat or a narration
  onset. 0 frames: the first frame of the new shot is `Math.round(beat x 30)`.
  The outgoing shot holds its last pose for at least 2 frames; the incoming
  shot opens already in motion.
- **One seam, one form.** Never stack two transitions. Pick one or two
  signature forms for the film and repeat them; within 30 seconds use at most
  four non-cut transitions.
- During a transition, both shots are drawn from the timeline, never from
  the previous frame, so any single frame re-renders identically.
- Not for: two shots with the same subject and nearly the same framing (reads
  as a glitch, not a cut).

- https://github.com/axtonliu/video-illustrator/blob/HEAD/references/transitions.md

**Camera motion craft:** cameras must never move in rigid straight lines -
use `THREE.CatmullRomCurve3` or similar splines for drone-like paths; damp
with `maath/easing` (`easing.damp3`) or drei's `CameraControls`; interpolate
the lookAt target simultaneously with the position to avoid mechanical
panning. GSAP timelines are the standard tool for choreographing camera cuts
and scene transitions.

- https://github.com/utorrentgaming/inject-ig/blob/HEAD/.agents/skills/cinematic-motion-graphics-skill/SKILL.md
- https://github.com/rk2725q-star/atcli/blob/HEAD/.agents/skills/cinematic-scene-director/SKILL.md

**[DERIVED] Our director:** a shot list (16+ shots: wide, low angle,
close-up on hands, close-up on ball, rear view, top-down for the finger spin,
tracking dolly, etc.), each a function of routine time; cuts on beat
boundaries (hard cut default, one signature transition form for street
changes); a measurement audit per shot in the style of threejs-previz (hero
in frame, no geometry clipping, no path jumps). Shot changes follow the
routine's phrasing - the finger spin gets the close-up, the crossovers get
the low angle, the resolve gets the wide.

### 4.6 Settings as interaction

The reference's model: "Everything is a setting." The Settings panel is the
real interaction; the stage itself is watch-only. What it exposes: stroke
size, density, kit colours, ball colours, tempo, camera mode, lighting,
moves, palette swatches, format switcher.

The standard three.js settings-UI library is **lil-gui** - the modern
dat.GUI replacement (official three.js examples switched in r135), ~10 KB,
ES modules, drop-in:

- `gui.add(target, property, min, max, step)` - numbers become sliders,
  booleans checkboxes, strings dropdowns, colours colour pickers. Changes
  apply live, no save, no reload.
- Folders group controls past 5 sliders (camera, lights, materials,
  post-processing). `.listen()` reflects external mutations. Presets
  save/load as JSON via localStorage. Multiple panels, hideable with a key.

- https://masterallarts.com/learn/threejs-from-zero/s14-02-debug-ui-mastery/
- https://svilenkovic.com/3d/lil-gui-three-js
- https://copyprogramming.com/howto/represent-a-three-js-gui-button-with-an-icon

Alternatives: **Tweakpane** (compact panes, graph monitors),
**Leva** (React Three Fiber). A Vite plugin exists that auto-generates
Tweakpane controls for shader uniforms.

- https://threejsresources.com/tools

**[DERIVED] Recommendation:** lil-gui for development tuning (it is built for
exactly this - finding the right shader uniforms, light intensities, and
camera angles live), but the shipped piece needs a custom-designed settings
panel in the piece's own visual language, not a stock lil-gui panel. The
reference uses a custom panel; a stock debug panel would read as unfinished.
What ours should expose (superset of the reference): stroke size, density,
disorder, tempo (60-180 BPM), kit + ball colours, palette, camera mode
(director / tracking / fixed), lighting, move selection (which moves are in
the routine), routine selection, format (16:9 / 9:16 / 1:1 / 4:5), sound
on/off, export. Every control must work for real; no decorative controls.

### 4.7 Export: recording the live canvas

The reference's Export MP4 is a real render. The browser-native route is
`canvas.captureStream()` + `MediaRecorder`:

- `const stream = canvas.captureStream(30)` exposes the canvas as a media
  stream at 30 fps; `new MediaRecorder(stream, { mimeType,
  videoBitsPerSecond })` encodes it; chunks are collected into a Blob and
  downloaded. The recorder captures the exact canvas used for preview, so
  the export is what the viewer sees.
- **Probe before offering:** check `MediaRecorder.isTypeSupported()` for each
  candidate. WebM candidates try VP9, then VP8, then generic WebM. MP4 is
  enabled only when the browser exposes a compatible MP4 recorder.
- **Browser reality:** Chrome/Edge record WebM (VP9); Firefox records WebM
  (VP8) and lacks native MP4 recording; Safari 16+ records MP4 (H.264).
  Safari wrote nothing but MP4 before version 18.4. MP4 is the format a
  phone's photo library and every social platform accept without conversion.
- A robust probe order (from the GESTURE-FX spec): `video/mp4;codecs=avc1.42E01E`,
  `video/mp4`, `video/webm;codecs=vp9,opus`, `video/webm;codecs=vp8,opus`,
  `video/webm;codecs=vp9`, `video/webm;codecs=vp8`, `video/webm`.
- `preserveDrawingBuffer` must be enabled on the WebGL context for reliable
  capture. `captureStream` support: Chrome 51, Firefox 43, Safari 11.
  MediaRecorder: Chrome 47, Firefox 25, Safari 14.1.
- **Fallback for MP4 everywhere:** record WebM, convert with ffmpeg.wasm in
  the browser - but at high resolutions this is slow and memory-hungry; use
  it for short clips and local FFmpeg for larger recordings. WebCodecs offers
  hardware-accelerated encoding (up to 10x faster than MediaRecorder +
  FFmpeg) and is available in Chromium-based browsers as of 2026.

- https://dev.to/sean_2de44664048773bf102a/how-i-built-a-wobble-gif-maker-with-canvas-api-and-nextjs-55g3
- https://github.com/amey-thakur/gesture-fx/blob/HEAD/docs/SPECIFICATION.md
- https://www.igfont.top/video-text.php
- https://www.explainroo.com/guides/render-canvas-animation-to-video/
- https://www.xjavascript.com/blog/save-canvas-data-as-mp4-javascript/

**[DERIVED] Export spec for our piece:** 30 s at 1920x1080 (matching the
reference), MP4 where the browser supports it, WebM otherwise, with the
format labeled honestly in the UI (never promise MP4 and deliver WebM). The
export must capture the live canvas - including the current settings, street,
and routine - not a canned render.

### 4.8 Sound

From the reference: sound off by default; "Tap for sound" enables a 144 BPM
kick/bass loop (WebAudio synthesized) with a note per ball touch. Every touch
plays a note - the ball is an instrument. Our piece keeps this exactly, with
one addition: the note should vary by move type (a crossover sounds different
from a finger-spin tap), so the ear can follow the routine. Standing rule:
the AudioContext suspends on tab hide, window blur, and page close, resuming
on return unless muted.

### 4.9 Performance notes [DERIVED]

- Kuwahara cost scales as `radius x sectorCount x 3` samples per pixel -
  keep radius small on phones, or make radius a quality setting.
- PainterlyPass needs three.js r160+ and WebGL 2. Verify the target devices
  support WebGL 2 (all modern phones do; old ones do not - the piece should
  degrade gracefully, not blank-screen).
- Stroke counts are a budget, not a brag: the reference's 21,769 strokes is
  a painting count (baked into the background texture), not a per-frame cost.
  Per-frame stroke work must stay inside the frame budget: target 60 fps on
  desktop, 30 fps on a real phone, measured, not assumed.
- The background painting is baked once per street (offscreen canvas at load
  or at street change), never redrawn per frame.

---
## 5. What could NOT be found

Stated plainly, as ordered. Each item names the closest public equivalent
where one exists.

1. **Vedel's shader code, model files, or any repository.** A GitHub search
   on 2026-11-12 returned nothing relevant under his name or the piece's
   name. No public repo, no gist, no code in the breakdown video. The
   site's JavaScript bundle is publicly served (it has to be, to run in a
   browser) but it is not published as source, and **we will not lift code
   from it** - the build is ours, from the research in this file.
   Closest public equivalents: PainterlyPass (post-process stroke layer,
   https://github.com/minchenlee/three-painterly), the SeloSlav
   three.js port of de Laubier (per-mesh strokes,
   https://github.com/SeloSlav/threejs-stylized-paint-shader), and Sekuta82's
   template (https://github.com/Sekuta82/oil-on-threejs-template).
2. **Any interview, write-up, or process breakdown beyond the breakdown
   video and X posts.** Vedel has not published a making-of article, a
   conference talk, or a tutorial for Paris Freestyle. Everything in Section
   1.5 is inference from the breakdown video and the Fable 5.2
   documentation, labeled as such.
3. **Any verifiable "Spellbound Labs" entity.** See Section 1.2. The .io
   studio names no founders; nothing links it to Vedel.
4. **Whether the 65-joint rig matches a standard auto-rigged skeleton.**
   Open question. No source found that would confirm or deny it. It does
   not matter for the build: our rig is specified from the routine's needs
   (Section 4.2), not from matching his joint count.
5. **The Moët Hennessy "Divine, AI Sommelier" / Lovie Awards 2024 credit.**
   Seen in an earlier research pass; no URL could be recovered at write
   time. Treated as an unconfirmed lead, not cited as fact.
6. **#Vibelog details.** The mirror page carrying the description could not
   be re-opened (unresolvable). See Section 1.3.
7. **The 1600-frames / 30-second export arithmetic.** 1600 / 30 = 53.3 fps,
   not a standard rate. Either the export captures at a non-standard rate,
   the frame count covers something else, or a number was misread in the
   study. Unresolved; our export spec (Section 4.7) uses standard rates and
   does not depend on resolving it.
8. **Several pre-compaction sources could not be re-recovered with URLs**
   (AO Hoops move library, Active.com crossover techniques, smartballers.com,
   the Konstanz 2011 dribbling biomechanics paper, the Field Notes 01
   rigging write-up, the Sorceress 3D Studio auto-rigger, the OVU ribbon
   -stroke forum thread). Claims that depended solely on those sources were
   either dropped or re-grounded in the sources cited in this file. The move
   vocabulary, dribble timing, rigging mechanics, and stroke techniques in
   Sections 2-4 rest only on the URLs listed in Section 7.
9. **Mixamo's current backend status.** One source (not re-verified) reported
   its backend broke in June 2025. A current guide (Oct 1, 2026) still lists
   Mixamo as free and available
   (https://gamesbyai.win/guides/ai-3d-animation-for-games/). If Route 2 is
   chosen in Stage 1, verify Mixamo works before depending on it.
10. **No source found for the fourth Paris Freestyle street's name.** The
    breakdown video names three streets (Boulevard Montmartre, Quais de
    Seine, Pigalle); the fourth is unnamed in every source consulted.

---

## 6. Staged build plan

Build slowly, one stage at a time, verified before continuing. No stage
starts until the previous stage's acceptance criteria pass. Each stage ends
with a verification step; "done" means the numbers pass, not that the code
exists. The build is Three.js (WebGL 2), single-page, no backend. The
rejected 2D version is not reused.

**Where we beat the reference** (the bar Hans set - "better than the
reference I sent"):

1. **Real moves, named and sourced.** The reference's juggling is generic
   motion; our routine is built from the documented vocabulary in Section 3.2,
   with each move named in the UI.
2. **Real dribble physics.** Control/Free phases, gravity-correct arcs, beat
   timing derived from actual bounce times (Section 3.3) - not one-touch-per
   -beat faking.
3. **A signature moment the reference lacks.** The finger spin (2-3 rev/s,
   held pose, camera close-up) is a showpiece the juggling piece has no
   equivalent of.
4. **Measured camera.** Every shot audited (hero in frame, no clipping, no
   path jumps) in the threejs-previz style, not eyeballed.
5. **Honest export.** Format labeled truthfully per browser (Section 4.7).

### Stage 0 - Research (this file)

- **Done when:** this file is complete, every claim carries a source URL,
  Section 5 is honest about gaps, and the plan below is agreed.
- **Status: DONE 2026-11-12.**

### Stage 1 - Character model

- **Work:** Build the stylized player. Route 1 (procedural geometry + code
  rig) is the recommendation; Route 2 (Blender + auto-rig + GLB) is the
  fallback. Decide in writing before starting, with reasons.
- **Acceptance criteria:**
  - One stylized player, original design (different kit and look from
    Vedel's pink-top figure; our own character).
  - Readable silhouette at 3 distances (full body at 5 m, 10 m, 20 m) in
    headless screenshots - a stranger can tell it is a basketball player.
  - 55-70 joints, including at least 2 joints per finger on each hand,
    3-4 spine joints, and a twist bone per forearm.
  - Joint count verified in code (log the skeleton; count must match the
    spec, not be eyeballed).
  - No candy-wrapper pinch: wrist twisted 180 degrees in a test pose must
    keep at least 60% of its rest cross-section (measure the mesh, do not
    eyeball).
  - Triangle count at or under 20,000.
  - Skeleton renders correctly under SkeletonHelper; every bone named.
- **Done means:** turntable screenshots at 3 angles + the joint-count log +
  the twist test, all passing. No routine yet, no shader yet.

### Stage 2 - Rig + routine

- **Work:** IK legs (two-bone), IK hand targets, the beat-mapped routine
  program. Routine built only from Section 3.2 moves.
- **Acceptance criteria:**
  - Routine contains at least 8 distinct named moves: control dribble,
    crossover, between-the-legs, behind-the-back, spin move,
    around-the-world, retrieval dribble, finger spin (with a transfer or
    snatch).
  - Routine is 24 beats at 144 BPM (10.0 s), loops seamlessly: first and
    last pose match within 1 degree per joint.
  - Every move occupies a whole number of beats; move boundaries fall on
    beat boundaries.
  - Feet planted: during stance phases, planted-foot skate is under 2 cm
    per step (measure world position of the foot target across the stance).
  - Hand-ball contact: during Control Phase windows, the ball centre is
    within 5 cm of the active hand bone.
  - Procedural posing runs after mixer.update() (or no mixer is used at
    all, if the routine is fully procedural).
- **Done means:** the routine plays on a grey stage (no shader, no
  background) and a reviewer who knows basketball can name at least 6 of
  the 8 moves from the motion alone. If the moves do not read without paint,
  the routine is wrong - fix the motion, not the look.

### Stage 3 - Ball physics

- **Work:** Hybrid ball drive (Section 4.3): hand-parented in Control
  Phase, ballistic in Free Phase, fingertip spin for the signature moment.
- **Acceptance criteria:**
  - Free Phase arcs are parabolas under g = 9.81 (fit the logged ball
    positions; residuals under 1 cm).
  - Bounce restitution 0.75-0.85, measured from logged apex heights.
  - Waist-height dribble cycle 0.75-0.80 s; low dribble cycle 0.53-0.57 s
    (Section 3.3 derivation).
  - Zero frames with the ball below the floor plane; zero frames with the
    ball more than 10 cm from the hand during Control Phase.
  - Every ball touch (floor bounce, hand contact) lands within 30 ms of a
    beat boundary.
  - Finger spin: ball rotates 2.0-3.0 rev/s on the fingertip, fingertip
    contact maintained (ball centre within 2 cm of the fingertip bone +
    ball radius) for the full spin window.
  - Ball seams visible and rotation readable at 1080p.
- **Done means:** a logged run of 10 routine cycles (100 s) passes every
  number above. No visual judgement substitutes for the log.

### Stage 4 - Shader + look

- **Work:** The painted look. Evaluate PainterlyPass against a hand-rolled
  alternative (Kuwahara base + per-mesh strokes on the hero, de Laubier
  style). Pick on evidence, in writing. Add toon banding, inverted-hull
  outlines, halftone shadow, rim erosion.
- **Acceptance criteria:**
  - Strokes stay on surfaces: in a 360-degree camera orbit test, strokes
    do not swim across surfaces (frame-diff the orbit: stroke features
    track their surfaces, not the screen).
  - Toon lighting: 3 hard bands (0, 0.25, 1 or tuned equivalent), verified
    by sampling pixel values across a lit surface.
  - Inverted-hull outline: 2 px at 1080p, constant across camera distance
    (measure outline width in screenshots at 5 m and 20 m; both 2 px
    +/- 0.5 px), no hull tears at hard corners.
  - Halftone shadow: the shadow is the character's real silhouette shape,
    filled with a dot screen; dot size varies with light (denser in
    darker areas).
  - Rim erosion visible on the silhouette at 1080p (compare silhouette
    edge against the un-eroded render; at least 10% of the edge pixels
    differ).
  - Line weight varies with curvature/distance (measure outline width on
    a curved vs flat region; they must differ by at least 1 px).
  - Frame rate: 60 fps on desktop, 30 fps on a real phone, measured over
    a 60 s run (not a single frame sample).
- **Done means:** a side-by-side still against the "painting, not filter"
  test in Section 2.4 passes all six points, and the frame-rate numbers
  hold. If the look fails, change the technique (Approach A/B/C/D), not the
  acceptance bar.

### Stage 5 - Scene (four streets)

- **Work:** Four NYC streets as unrolled paintings (Section 2.5 + 4.4):
  Washington Square Park (golden hour), DUMBO / Brooklyn Bridge (afternoon),
  SoHo cast-iron (dusk), Coney Island boardwalk (night). Structure first
  (solid washes), strokes as sparse texture.
- **Acceptance criteria:**
  - "Name the place" test: each street is identifiable from its silhouette
    and landmark structure alone, with stroke texture disabled. 4/4 must
    pass.
  - Painting reveals ahead of the camera: the reveal edge leads the visible
    window by at least 10% of the screen width at all times.
  - Street changes every 16 beats (~6.7 s at 144 BPM); all 4 streets cycle;
    the loop is seamless (no visible pop at the wrap point).
  - Structure survives the Density setting: at density 0.15, all landmarks
    remain identifiable (the v2 lesson).
  - Background is baked once per street, never redrawn per frame (verify by
    profiling: no per-frame canvas 2D draw calls for the background).
- **Done means:** headless screenshots of all 4 streets at desktop + phone
  widths pass the name test, and the 16-beat cycle is logged, not assumed.

### Stage 6 - Beat clock + sound

- **Work:** 144 BPM beat clock driving routine, street changes, camera cuts,
  and paint reveal. WebAudio kick/bass loop + a note per ball touch, note
  varying by move type. Sound off by default.
- **Acceptance criteria:**
  - Tempo slider 60-180 BPM; default 144. Changing tempo re-times the
    routine, street changes, and camera cuts with no reload and no drift
    (measure beat alignment over 60 s at 60, 144, and 180 BPM; drift under
    50 ms).
  - Every ball touch plays a note within 30 ms of the touch (sound on).
  - At least 4 distinct note timbres/pitches, one per move family
    (dribble, crossover/hand-change, spin, finger-spin tap).
  - AudioContext suspends on tab hide, window blur, and page close;
    resumes on return unless muted (standing rule; test all three).
  - Sound is off by default; "Tap for sound" enables it.
- **Done means:** a 60 s run at each of 3 tempos passes the drift and
  note-timing numbers, and the lifecycle test passes on all three triggers.

### Stage 7 - Camera director

- **Work:** Shot list + director (Section 4.5). 16+ shots, cuts on beat
  boundaries, hard cut default, one signature transition form for street
  changes.
- **Acceptance criteria:**
  - At least 16 distinct shots, each defined as position/target/fov/roll
    over routine time.
  - Cuts land on beat boundaries (within 1 frame at 60 fps).
  - No shot holds longer than 8 beats; no two consecutive shots share the
    same subject and near-identical framing.
  - Measured audit per shot (threejs-previz style): hero in frame in 100%
    of sampled frames (sample every 6th frame over a full routine cycle);
    zero frames where the camera clips geometry; zero path jumps (camera
    position delta per frame under a sane bound except at cuts).
  - No 180-degree flips (direction continuity enforced).
  - Transitions between shots complete within 1.0 s; velocity damping, no
    overshoot (camera position overshoot under 5% of the move distance).
  - The finger spin gets a close-up; crossovers get a low angle; the
    resolve gets the wide (shot-to-move mapping verified in the shot list).
- **Done means:** the audit log passes for a full routine cycle at 144 BPM.
  Eyeballing the camera is not verification.

### Stage 8 - Settings + export

- **Work:** Custom settings panel (not a stock lil-gui panel in the shipped
  piece; lil-gui may be used during development), format switcher, palette
  swatches, export.
- **Acceptance criteria:**
  - Every control works for real, verified one by one: stroke size,
    density, disorder, tempo, kit colours, ball colours, palette, camera
    mode (director / tracking / fixed), lighting, move selection, routine
    selection, format (16:9 / 9:16 / 1:1 / 4:5), sound on/off, export.
    Zero decorative controls.
  - Settings re-skin live with no reload (change stroke size mid-routine;
    the look changes within 1 frame, the routine does not restart).
  - Format switch reframes the stage without breaking layout at 390 px
    and 1280 px widths (screenshot both, all four formats).
  - Presets save/load as JSON (export + import round-trips identically).
  - Export produces a real downloadable file: 30 s, 1920x1080, in Chrome
    and Safari. Format labeled honestly in the UI (MP4 where supported,
    WebM otherwise). The export captures the live canvas with current
    settings - verify by exporting with a distinctive palette and checking
    the file's frames carry it.
  - Zero console errors across all of the above.
- **Done means:** a control-by-control checklist, all passing, plus a real
  export file inspected frame by frame.

### Stage 9 - Verification (the gate)

- **Work:** Real-device check on Hans's phone, performance pass, final
  review against the brief.
- **Acceptance criteria:**
  - Loads and runs correctly on his real phone (portrait + landscape):
    whole stage visible, controls reachable, routine readable, no
    blank screens, no swallowed taps. This is the gate Hans's standing
    rule requires - headless screenshots and green suites are proxies,
    never the done signal.
  - 30 fps sustained on the phone over a 60 s run.
  - Zero console errors on desktop and phone.
  - The piece passes the Section 2.4 painting test and the Section 3.2
    move-naming test on the real device.
  - Link sent only after the real-device check passes.
- **Done means:** Hans sees it on his phone and it works. Nothing before
  that counts as done.

---

## 7. Source list

### The creator and his process

1. Vedel's Paris Freestyle X post + breakdown video -
   https://x.com/mxvdl/status/2120809570344050705
2. Paris Freestyle live site - https://parisfreestyle.netlify.app/
3. Vedel / Swipe Back, Vogue Business piece (syndicated) -
   https://www.windowswear.com/did-metaverse-fashion-week-show-us-the-way-for-virtual-brand-experiences-well-sort-of/
4. Same piece, second syndication (incl. the "pushing boundaries" quote) -
   https://cmgcrypto.com/2022/10/06/fashion-week-hits-the-metaverse/
5. Vogue Business original (cited in references; read via syndication) -
   https://www.voguebusiness.com/technology/metaverse-fashion-week-the-hits-and-misses
6. Vedel / Swipe Back, Campaign Asia -
   https://aws2.campaignasia.com/article/did-metaverse-fashion-week-show-us-the-way-for-virtual-brand-experiences-well-s/476950
7. Vedel / Swipe Back / Nikhil Roy, Creative Review -
   https://www.creativereview.co.uk/metaverse-design-architecture-gaming/
8. #GlobalPrideCrossing credits -
   https://www.bestadsontv.com/ad/116651/Animal-Crossing-New-Horizons-GlobalPrideCrossing
9. Vedel quote, BeautyMatter -
   https://beautymatter.com/articles/metaverse-beauty-week-2023
10. Spellbound Labs (.io studio, no Vedel link) - https://www.spellboundlabs.io/
11. Spellbound Labs (.ai, different company) - https://www.spellboundlabs.ai/
12. Claude Fable 5.2, official - https://www.anthropic.com/claude/fable
13. Fable 5.2 launch-day guide -
    https://createmotion.io/guides/claude-fable-5-2-animation/
14. Fable 5.2 uses roundup -
    https://www.zeniteq.com/5-cool-things-people-use-claude-fable-5-2-for-bb8k7e
15. Fable 5.2 early testers -
    https://news.sulat.com/2026/11/10/claude-fable-5-2-mythos-devin-early-access-builds/

### The brush-stroke shader and the artwork

16. Meier 1996, Painterly Rendering for Animation -
    https://doi.org/10.1145/237170.237288
17. Hertzmann 1998, Painterly Rendering with Curved Brush Strokes -
    https://doi.org/10.1145/280814.280951
18. Hanson & Todd, Object-Space Painterly Rendering for WebGL (paper) -
    https://www.cs.rpi.edu/~cutler/classes/advancedgraphics/S14/final_projects/scott_andy.pdf
19. Same, code - https://github.com/ScottTodd/PainterlyRendering
20. Heckel, On Crafting Painterly Shaders (Kuwahara) -
    https://blog.maximeheckel.com/posts/on-crafting-painterly-shaders/
21. React Postprocessing Kuwahara -
    https://react-postprocessing.docs.pmnd.rs/effects/kuwahara
22. Susurrus, Codrops (Kuwahara as the only post pass) -
    https://tympanus.net/codrops/2026/04/24/susurrus-crafting-a-cozy-watercolor-world-with-three-js-and-shaders/
23. Paintify (Kuwahara critique; Hertzmann approach) -
    https://shivav.com/writing/paintify-making-3d-feel-painted.pdf
24. PainterlyPass repo - https://github.com/minchenlee/three-painterly
25. PainterlyPass forum announcement -
    https://discourse.threejs.org/t/painterlypass-bakeddebris-painterly-post-processing-and-rewindable-explosion-debris/96674
26. de Laubier, Stylized paint shader breakdown -
    https://cyn-prod.com/stylized-paint-shader-breakdown
27. SeloSlav three.js port of de Laubier -
    https://github.com/SeloSlav/threejs-stylized-paint-shader
28. Sekuta82 per-mesh oil-paint template -
    https://github.com/Sekuta82/oil-on-threejs-template
29. Polyfork, six shader looks (toon, inverted hull, pixel outlines) -
    https://polyfork.dev/blog/six-shader-looks-and-which-ones-you-can-keep
30. Polyfork, three.js shaders live - https://polyfork.dev/threejs-shaders
31. MeshToonMaterial docs - https://threejs.org/docs/pages/MeshToonMaterial.html
32. OutlineEffect docs - https://threejs.org/docs/pages/OutlineEffect.html
33. Toon example - https://threejs.org/examples/webgl_materials_toon.html
34. Toon/cel shading explainer -
    https://jargon-buster.sivaram.dev/3d-webgl/toon-shading-cel-shading/
35. Descent cel-shaded brief (variable line weight, halftone shadows) -
    https://www.sameprompt.io/prompts/descent/
36. Anime cel-shading skill (halftone shadows, inverted hull) -
    https://github.com/priyanshuf1/rabto/blob/HEAD/skills/anime-cel-shading/SKILL.md
37. Infinite-loop gallery (CanvasTexture atlas, fract UV wrap) -
    https://www.aduok.in/blog/how-to-build-an-infinite-loop-gallery
38. Image wave scroll (baked canvas band, scroll-offset sampling) -
    https://www.aduok.in/blog/how-to-build-image-wave-scroll

### Freestyle basketball movement

39. Streetball culture and history (Rucker, EBC, AND1, legends) -
    https://triplethreatstreetball.com.au/what-is-streetball-culture/
40. AND1 Untold explainer (Mixtape Vol. 1, tour, ESPN, Professor) -
    https://dmtalkies.com/untold-the-rise-and-fall-of-and1-ending-explained-2022-documentary/
41. AND1's reign (1993 founding, 50,000 Skip Tapes) -
    https://faintflex.substack.com/p/faintflex-vol-18-and1s-reign
42. Streetball culture, Quai 54 Paris -
    https://nobackboardbasketball.substack.com/p/streetball-culture-quai-54-and-growth
43. Freestyle moves and routine building -
    https://smtp2.itp.net/now-chronicle/master-awesome-basketball-freestyle-moves-1767646929
44. Dribbling tips, drills, 30-second freestyle -
    http://www.basketball4all.net/dribbling.html
45. The Dribble Cycle (Control/Free Phase) -
    https://www.houseofjustus.com/the-dribble-cycle/
46. Dribble Rhythm and Control Tempo -
    https://www.houseofjustus.com/dribble-rhythm-and-control-tempo/
47. House of Justus curriculum index - https://www.houseofjustus.com/
48. Katsuhara et al. 2010, rhythmic basketball bouncing -
    https://doi.org/10.2466/PMS.110.2.469-478
49. Finger-spin guide (2-3 rev/s, gyroscopic, snatch, transfer) -
    https://gitlab.hesconet.com/IT/slideshare/=ImRwdlV6Z1aidkSYRVb1olVzIkRUZlUhJVbGlXVsRWaOdkUHZ1asdlVrVDWWtmWs5EbaRXVtR3dWxGbYNmRWh1VEZFdVFjQrJmRKpXVsZVVWpnVyZ1V0NVYxYlWiVkWYFmesJnVup0bitWMXV2RxgFVqZkcahlTvZlVZpXTWJVU
50. Guinness globe ball tricks record (Wass Benslimane, Paris 2009) -
    https://www.guinnessworldrecords.com/world-records/most-globe-ball-tricks-in-one-minute
51. KalaniBallFree two-ball freestyle -
    https://www.instagram.com/reel/DdH5pyjvVWj/
52. Two-ball rhythm drill - https://www.sixsevenball.com/drills/two-ball-rhythm

### The 3D build approach

53. Rigging, skinning, skeletal animation -
    https://jargon-buster.sivaram.dev/3d-webgl/rigging-skinning-skeletal-animation/
54. AI 3D animation for games (Mixamo, Meshy, Tripo, retargeting) -
    https://gamesbyai.win/guides/ai-3d-animation-for-games/
55. Three.js animation (mixer, procedural bones after update, attach) -
    https://www.ui-skills.com/skills/cloudai-x/threejs-animation
56. Browser-local auto-rigger - https://github.com/nff747/auto-rig-web
57. three.js starter kit (two-bone IK, foot placement) -
    https://github.com/vibegameengine/web-starter-kit
58. PolyFish cinematic camera (director layers, damping, BokehPass) -
    https://polyfish.unclemattmakes.com/making-of/camera
59. threejs-previz (measured shots, move catalog) -
    https://github.com/DevMartinese/threejs-previz
60. Transition grammar (hard cut on the beat) -
    https://github.com/axtonliu/video-illustrator/blob/HEAD/references/transitions.md
61. Cinematic camera motion (splines, damping) -
    https://github.com/utorrentgaming/inject-ig/blob/HEAD/.agents/skills/cinematic-motion-graphics-skill/SKILL.md
62. Cinematic scene director (GSAP camera, AnimationMixer) -
    https://github.com/rk2725q-star/atcli/blob/HEAD/.agents/skills/cinematic-scene-director/SKILL.md
63. lil-gui debug UI mastery -
    https://masterallarts.com/learn/threejs-from-zero/s14-02-debug-ui-mastery/
64. lil-gui for three.js - https://svilenkovic.com/3d/lil-gui-three-js
65. Three.js GUI libraries 2026 (lil-gui vs Tweakpane vs Leva) -
    https://copyprogramming.com/howto/represent-a-three-js-gui-button-with-an-icon
66. Three.js tools directory (Tweakpane) - https://threejsresources.com/tools
67. Canvas to video, MediaRecorder (Purupuru Maker) -
    https://dev.to/sean_2de44664048773bf102a/how-i-built-a-wobble-gif-maker-with-canvas-api-and-nextjs-55g3
68. GESTURE-FX spec (container probe order, capture support) -
    https://github.com/amey-thakur/gesture-fx/blob/HEAD/docs/SPECIFICATION.md
69. Canvas video compatibility table - https://www.igfont.top/video-text.php
70. Render canvas animation to MP4 (ffmpeg.wasm limits) -
    https://www.explainroo.com/guides/render-canvas-animation-to-video/
71. Save canvas as MP4 (WebM workaround, WebCodecs) -
    https://www.xjavascript.com/blog/save-canvas-data-as-mp4-javascript/

### Local reference records

72. Reference study (live-browser, 2026-11-12) -
    `~/workspace/freestyle-basketball/research/paris-freestyle-study.md`
73. Saved page PDFs -
    `~/workspace/.hatch-browser/outputs/browser-task:403febc9-cd7d-5c81-8e90-03089a0dd5fb/`

---

*Research completed 2026-11-12. 71 external sources + 2 local records. No
build work done; no code written. Next step is the parent agent's: Stage 1,
starting with the Route 1 vs Route 2 decision in writing.*
