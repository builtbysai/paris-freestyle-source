# Paris Freestyle source deconstruction

Max Vedel / Spellbound Labs, `parisfreestyle.netlify.app`. Source pulled 2026-11-13.
For understanding only. No code from this source is to be copied into our repo.

## How this was read

| File | Size | Status |
| --- | --- | --- |
| `index.html` | 9,038 B | Readable. Plain HTML, fonts (Unbounded, Inter, Caveat Brush), lil-gui + three.js + mp4-muxer vendors, loads `character.js` then `app.js`. |
| `app.js` | 161,856 B | **Minified, 1 line.** Beautified with `npx js-beautify` to `/tmp/app-beauty.js` (6,763 lines). All `app.js` line numbers below refer to that beautified copy. Regenerate with `npx -y js-beautify -f app.js -o /tmp/app-beauty.js` from this directory if `/tmp` is wiped. Variable names are single letters throughout; string literals, numeric constants, GLSL shader strings, and program structure survive. |
| `character.js` | 2,410,995 B | Data blob, 2 lines. Header: "built by tools/build_character.py from the rigged GLB: do not edit by hand". `window.CHARACTER` object; geometry arrays are base64 strings. Decoded by sampling, not dumped. |

Nothing below is invented. Where a mechanism could not be read (minified names), that is stated.

## 1. Architecture map

`app.js` is a sequence of IIFEs, each registering one module on a global namespace:

| Lines | Module | Role |
| --- | --- | --- |
| 1-49 | BRUSH core | Utils: seeded rng, clamp/lerp/sstep/damp, canvas, 256px paper texture, `viewOf`. `VERSION "1.0.0"`. |
| 50-353 | Stamp atlas | 16 procedural brush shapes baked into a 1024x512 canvas texture. |
| 354-673 | `createEngine` | The 2D paint engine: StrokeSet, the 2D stroke vertex/fragment shaders, palette/layer uniforms, paint front. |
| 674-855 | `e.kit` | StrokeSet builders: `line`, `fill` (golden-ratio sampling), `box`, `slab`, `sketch`. |
| 856-1106 | `createPainter` | Paints StrokeSets into render targets (backdrop renderer). |
| 1107-1417 | `paintObject` / `paintSkinned` | Samples 3D mesh surfaces and generates instanced brush strokes on them. |
| 1418-1531 | `createTrail` | Instanced motion trails (capacity 1536 default). |
| 1532-1891 | LENS | Fisheye projection, skin/hull/ball/blot/cast/post/word materials, street raymarcher. |
| 1892-2358 | AUDIO | WebAudio synthesis: drums, chip, pad, per-body-part touch sounds, per-street melodies, scheduler, offline render. |
| 2359-4729 | PLACES | 4 Paris streets: palettes, building heights, `compose` (kit -> StrokeSet), `lightDir`. |
| 4730-5429 | PLAYER | The character, the routine, the ball. The core. |
| 5430-6763 | Main app | Settings, renderer, camera modes, words, impact, render pipeline, export, lil-gui, boot. |

Boot order: `window.CHARACTER ? PLAYER.loadTexture(CHARACTER.tex, ...) : ...` (texture first), then the main function. The main loop is `requestAnimationFrame`; animation time comes from the audio clock (`ft.time()`), not from `performance.now()`. `Lt(e)` seek = restart + step `Qe(1/30)`, so the whole animation is a pure function of time.

Settings persist to localStorage key `paris-freestyle-v7` (rev 2); paint settings are saved separately.

**One fact that reframes everything below: this is soccer freestyle, not basketball.** The ball shader draws pentagon patches (icosahedron vertices, golden ratio 1.618034). Touches are foot/knee/head/chest/volley. There are no hand touches, no dribble, no floor bounce. Several of the physics differences in section 10 follow from the sport, not from better engineering.

## 2. Renderer / scene setup

- `WebGLRenderer({canvas, antialias: false, preserveDrawingBuffer: true, powerPreference: "high-performance"})`. **Pixel ratio locked to 1** (`setPixelRatio(1)`); preview resolution is handled manually in `dt()`: canvas sized by DPR capped at 2, preview factor auto = min(dpr, 1.5), full = dpr, half = .75. `autoClear false`. **Requires WebGL 2.**
- Camera: `PerspectiveCamera(50, 1, .1, 500)`. The 50-degree FOV is nominal; the fisheye lens patch (section 6) replaces the projection in every material.
- Lighting is not scene lighting. There are no three.js lights doing the work. Everything is toon-shaded in custom shaders with a per-street `surfaceLight` (uLight/uShadow/uDeep/uRim/uLightDir/uInk). Default light direction `(.45, .6, .66)` normalized. Per-street surface light comes from the place def (`surf.light/shadow/deep/rim/ink`, e.g. haussmann light `#ffe0bc`, shadow `#6f9cca`, ink `#27406e`).
- The "scene" is a 2.5D corridor, not a 3D street. The street is a fullscreen-quad raymarcher (`LENS.createStreet`): for each pixel it un-projects through the fisheye, ray-casts a ground plane and two walls with per-block heights (`uHL`/`uHR` arrays, NB=11 blocks, block width BW=12 m, street length TW=132 m), and samples painted textures (far 91x61 m region, near 26x61 m, dome panorama). The painted textures are rendered by the 2D paint engine into render targets, then the raymarcher maps them onto ground/walls/dome. Paint in 2D, project in 3D.
- Render pipeline `lt()`: render targets far, near, dome, scene (MSAA 4 if aa). Quality scales low .55 / medium .78 / high 1 (base 18/40/12 px-per-unit). `at()` renders the painted backdrop into the RTs; halftone pass uses `overrideMaterial = castMat` with camera layers set to 1. Post pass: `T.scene` with uFx (fringe/vignette/grain/zoomBlur + whip + impact), uCol (saturation/contrast), uImp, uBrk (outro shards).
- Adaptive quality: fps < 22 three times in a row degrades in steps: aa off, quality medium, preview half, quality low, fewer strokes (with a toast).
- Background/paper: the paint engine's paper texture (256px, value-noise) modulates every stroke's coverage; the street shader mixes haze toward the dome by depth (uStreet.w = 60 m far distance).

## 3. The rig

### The character data (`character.js`)

`window.CHARACTER = {name:"kid", verts:47087, tris:51515, lo:[-0.404, 0, -0.239], hi:[0.404, 0.9995, 0.239], pos, nrm, uv, idx, si, sw, reg, bones, regions, regLuma, regCol, measures, tex}`.

- A real skinned mesh, built from a rigged GLB by `tools/build_character.py`. 47,087 vertices, 51,515 triangles. Height 0.9995 m (a 1 m tall figure, scaled up at runtime by `charHeight/figH`, default 1.64).
- Encoding (verified by decoding samples): `pos` = Int16 quantized against lo/hi; `nrm` = Int8/127; `uv` = Uint16/65535; `idx` = Uint16 triangles; `si` = 4 bone indices per vertex (uint8); `sw` = 4 weights per vertex (uint8, sum 255, e.g. [225,22,8,0]); `reg` = 1 byte per vertex = region id.
- Regions: `['Cap','Hair','Skin','Jacket','Shorts','Shoes']`, with per-region `regLuma` ([0.542, 0.132, 0.646, 0.347, 0.356, 0.521]) and `regCol` RGB arrays. Region drives stroke flow and recolouring.
- Measures: `footTop [0.0699, 0.075]`, `kneeTop [0.0925, 0.02]`, `chest [0, 0.54, 0.1008]`, `headTop 0.3299`, `sole 0.0839`, `height 0.9995`. **The head is ~33% of the 1 m height** (headTop 0.3299): a chibi proportion, baked into the source mesh.
- `tex`: an embedded `data:image/webp;base64` colour map.

### The skeleton: 65 bones, Mixamo-style

Root `Hips` at (0.002, 0.424, 0.025). Chains: Hips -> Left/RightUpLeg -> Leg -> Foot -> ToeBase -> Toe_End; Hips -> Spine -> Spine1 -> Spine2 -> (Left/RightShoulder -> Arm -> ForeArm -> Hand -> 5 fingers x 4 segments each = 40 finger bones) and Spine2 -> Neck -> Head -> HeadTop_End.

Bind quaternions are non-trivial: Hips q=[0,-0.707,0,0.707] (-90 deg about Y); LeftUpLeg q approx [-0.996, 0.035, 0.035, 0.075] (about 180 deg about X, flipping the bone's +Y down). Bone translations are in bone-local space (leg bones have +Y translations like 0.158, 0.167).

### How it is built at runtime (PLAYER `r.create`, app.js ~L4760-4900)

- Decodes CHARACTER into a `THREE.BufferGeometry` (position/normal/uv/skinIndex/skinWeight/`aReg` attributes, Uint16 index), builds 65 `THREE.Bone`s + `Skeleton`.
- Creates **two SkinnedMeshes sharing geometry and skeleton**: skin (`LENS.skinMat(..., false)`, DoubleSide, `layers.enable(1)`) and hull (`LENS.skinMat(..., true)`, BackSide, `renderOrder -1`). The hull is the outline (see section 6).
- Captures bind-pose world quaternions `E`, world positions `B`, local quaternions `T` per bone.
- Measured params `V`: figH = measures.height, hipY = .985 * B.Hips.y, ankle = B.LeftFoot.y, footX = .82 * |B.LeftFoot.x|, upper = |Arm -> ForeArm|, hand = 1.5 * |ForeArm -> Hand|, scale 1.6.
- `U` = per-bone bind-offset quaternions (`U[name] = frame^-1 x bindWorldQuat`): the retarget layer between the proxy rig and the real skeleton.
- Finger curl is baked into the bind: axis (0,1,0) for thumb else (0,0,1), angle ("Thumb" ? .25 : -1) * side * [0, .38, .5, .34][segment].
- Fallback figure (no CHARACTER data): primitives (spheres/capsules/cylinders) parented to the same proxy rig, with `paintSkinned` strokes per mesh (count from surface area, 120-760) and `paintObject` on shells.

### The proxy rig `F`: the rig that is actually animated

The 65-bone skeleton is never posed directly. It is driven by a small proxy rig, and `apply()` retargets proxy world quaternions x `U`, converting world to local down the bone chain, scaling hands/feet/head by settings.

- root -> pelvis (at hipY, rotation order YXZ) -> spine -> neck -> head.
- Arms parented to spine (arm -> fore).
- **Legs (leg/shin/foot) are separate groups parented to the world group, NOT to the pelvis. They are IK-driven in world space.** This is the single most important structural fact in the rig: the feet live in world space, so the body can spin, jump, and lean without dragging the feet.
- The ball is a SphereGeometry r=.11 mesh + `LENS.ballMat` + `paintObject` strokes (rim 60). Physics radius `I.ballR = .135` (bigger than the visual .11: contact reads generous). Shadow blobs = `LENS.blot` planes, renderOrder -5.
- Settings `I`: stride 1, energy 1, sway 1, jump .42, spin 1, ballR .135, scale, head/hands/feet 1.

## 4. Animation

### The routine is a touch schedule, not keyframes

`LOOP = 16` beats. The routine is defined as:

- `TOUCH`: 14 entries at beats 0-14, each `{b, part, s}` where part is foot/knee/head/chest/volley, s = +/-1 side. `lob:1` at b=4, `atw:1` (around the world) at b=9 and b=11.
- `SWING`: 16-entry +/-1 array (per-beat active side).

There are no pose keyframes anywhere. Every pose is computed from closed-form functions of beat time. Helpers: `f` = beat wrap mod 16; `p` = wrapped beat difference; `h` = cosine bump; `m` = smoothstep window; `v` = smoothstep.

### Root and body (`de(e,t,a,r,s)`, the pose function)

- **Root y is always 0.** Root position is (sway, 0, depth). The jump lives in the pelvis, not the root.
- `te(e)` yaw = .14*sin(2*pi*t/16 + 1) + spin * 2*pi * smoothstep((t - 4.3)/1.45): a full 360-degree spin over beats 4.3-5.75.
- `ae(e)` sway = 1.15 * L * (.75*sin(2*pi*e/16) + .22*sin(2*pi*e/8 + 1)).
- `ne(e)` jump = jump * L * 4t(1-t), t = clamp((f(e) - 13.55)/.95): a parabolic hop over beats 13.55-14.5.
- Touch windows: H (foot/volley), O (knee), N (head), ee (chest); `ue` = spin window at beat 5; `de` = jump window 13.55-14.5; `fe` = beat-4 window; `pe` = pelvis accents (bumps at 13.42, 14.62, 5.72, 6.72, 6.02, 7.02).
- Pelvis y = hipY + (bob + pe*A)*L + jump. Pelvis yaw = C + .16*ve (ve = foot spread, re[1].z - re[-1].z). Spine pitch leans on chest (.42*ee) and jump (.36*de); spine yaw counter-rotates with foot spread and twists toward the active touch side. Arms abduct on touches/jump (1.75*de)/spin/head/chest. Neck counter-pitches to keep the head level; **head tracks the ball** (pitch clamped -.6 to +.5, yaw -.7 to +.7), suppressed during head/chest touches.

### Feet: a step state machine (`oe(e,t,a)`)

For side e at beat t: if side e is active at rounded beat o, find the run of consecutive active beats [a..r] (max 4 each way); foot z = lerp(-.5, .5, smoothstep((t - (a - .5))/(r - a + 1))), lift = .13*sin(pi*s), air = 1. If inactive, the foot is planted but blends near boundaries (window .17) with lift .09. Returns {z, lift, air, u}.

Foot target = U(side*footX, ankle + lift*L + P*(air ? .55 : .9), z*R). During the jump both feet rise, but planted feet rise .9P and airborne feet .55P, so the feet tuck differentially relative to the pelvis.

- Pitch: airborne .25*sin(pi*clamp(z+.5)); planted heel-toe roll .5*d(.2,.5,u) - .28*(1 - d(-.5,-.32,u)).
- Jump special: right foot kicks to (1.2*footX, V+P+.3*L*de, -.22*L*de), pitch -> .7.
- Touch override: the striking foot moves to the `ie[part]` position with approach slope (.55*r*L, lob x1.5), volley adds ne(f.b); foot lerps by the window; pitch -> knee .55 / foot .62.
- **Around-the-world: the FOOT orbits the BALL in a vertical circle** (angle -pi/2 + 2*pi*smoothstep(e), radius lerp(ce, max(1.45*ce, .3L), sin(pi*e))), pitch .62. The ball is the anchor; the foot moves. Contact params `ie`: foot [.15,.36,.4], knee [.13,.5,.36], volley [.24,.86,.5].

### Two-bone IK

Hip G (pelvis.localToWorld(hipOff)), target _; |W| clamped to .996*(L1+L2) max / .25*(L1+L2) min; H=(L1^2-L2^2+T^2)/2T, O=sqrt(L1^2-H^2); knee pole Y forward (+z in character frame) with .12*t sideways tilt; knee j = G + W*H + q*O. `$[t]` stores the knee position (used for knee touches). Thigh/shin quaternions from `k()` basis construction; foot from Euler(pitch, C+.14*t, 0, YXZ).

### The body leads, the ball follows (`fe()`, the measure step)

For each touch, the code poses the body at the touch beat (`de(a.b, 0, null)`) and reads the actual world position of the contact part:

- foot/volley = foot pos + footTop offsets (the ball sits ON the foot);
- knee = `$[s]` IK knee pos + kneeTop;
- head = Head pos + headTop + .92*ballR;
- chest = Spine2 local chestLocal + .95*ballR outward.

Intro ball `ue` = left foot pos + forward offset, y = ballR. `shotPoint(e,t,a)` poses with s=2 and returns the left foot pos + offset (camera look-at for the outro).

This is the mechanism that makes ball/body contact exact: the contact points are measured from the posed body, and the ball is fitted between them. Contact is exact by construction, not by audit.

## 5. The ball

`ballAt(e,t,a,n)`: **exact projectile motion, NO floor bounce.**

- `p.lerpVectors(c.p, u.p, p); p.y += 4.905*m^2*p*(1-p)` where 4.905 = g/2, m = segment duration in seconds, p = segment progress. `vy = (y1-y0)/m + 4.905*m*(1-2p)`.
- `squash = max(h(o,c.b,.1), h(o,u.b,.1))`: squash peaks at the touch endpoints.
- The ball arcs touch-to-touch and stays airborne. Freestyle juggling, not dribbling. The ball never touches the floor in the loop.
- Per-frame: ball world z = p.z + k (k = distance travelled, see section 7). Squash/stretch scale (1+.2D-.5O, 1-.28D+O, 1+.2D-.5O) where D = squash near touches, O = .16*clamp(|vy|/7) speed stretch.
- Ball rotation: **-13 * 2*pi/16 * e: 13 revolutions per 16-beat loop, backspin**, plus wobble. Spin is purely visual; it never affects the path.
- Ball shadow is a blot plane that shrinks with height (1 - min(y/3.2, .7)). Character shadow shrinks with jump (1 - .6q).

The ball shader (`LENS.ballMat`) is a soccer ball: 12 icosahedron vertices (golden ratio 1.618034), pentagon pattern from the two largest dot products (pent = smoothstep(.560,.574,g), seam), three-tone lighting (deep/shadow/light), bounce light from below (uBounce), rim, specular, halftone dots on the shadow side (uHtBall).

## 6. The brush shader

There are three brush systems, not one: (a) instanced stroke quads on 3D meshes, (b) the 2D paint engine for streets, (c) the fisheye lens patch that every material gets.

### (a) Strokes on meshes: `paintSkinned` / `paintObject` (L1107-1417)

The look is NOT a texture and NOT a post-process. It is thousands of instanced brush-stroke quads sampled onto mesh surfaces.

**Sampling.** `paintSkinned` (character): area-weighted triangle sampling (cumulative area table, binary search `find`), barycentric point-in-triangle, up to `count` samples with 30x rejection retries. The character gets **6400 strokes (seed 91)**. Per sample it records position, normal, UV, dominant vertex, and colour from the texture map (256px sample). The stroke instance copies the **skin indices/weights of its dominant vertex**, and the stroke mesh is a `SkinnedMesh` bound to the same skeleton, so strokes deform with the body.

**Flow rule.** A `rule(P, T, color, rng, vertexIndex)` returns {flow, len, wid, st, bend, tint, face}. The kid's rule: face region (y .61-.86, z > .085, |x| < .13) is skipped entirely (returns null: no strokes on the face). Per-region flow: hair [2.2x, -1, 1.2z-.1], cap [3*-z, .25, 3x], shoes [.2,-.35,1], jacket sides [+/-1,-.25,0], default [.25*tx, -1, .25*tz]. 35% long strokes (len .05, wid .017), else short (.028/.0095). Shapes from [0,1,2,3,4,12,13]. Tint = region/8. Face cull threshold .05.

**`paintObject`** (props, ball, fallback parts): two passes. Surface strokes: tangent from a flow field (downward on vertical surfaces, horizontal on flat ones), len .13/.26, wid .045/.085, shapes [0,1,3,16] / [1,2,5,4,13]. Rim strokes: tangent horizontal, shapes [10,11,10,4]. Edges: `EdgesGeometry(28 deg)` -> black strokes along hard edges (len = edge length, wid .028). Counts scale with area: surface = clamp(95*E, 70, 380), rim = clamp(70*E, 60, 300), E = total area / h^2.

**Stroke vertex shader (skinned variant).** Skinning matrix `sm = bindMatrixInverse * (sum weight*boneMat) * bindMatrix` applied to rest position/tangent/normal. Strokes are billboarded in view space: d = view-space tangent direction; `fore = clamp(dl/sc, .35, 1)` foreshortens strokes whose tangent points at the camera. Facing cull: discarded if `facing < aQ.w` or a random draw exceeds `uCh.x` (density cull, uCh.x = 1/CH_DENS, CH_DENS = 1.6). Length/width get per-stroke random variance (uCtl.z), angle variance (uCh.w), bend as a circular arc (R = len/bend). `mv.z += 0.22*sc` pushes strokes toward the camera to avoid z-fighting. Colour: base = mix(uColor, uColor*texture(map, aUv2), uHasMap), tint mix; tone = 3-band (deep/shadow/light by N dot L + random .9 jitter); rim mix by facing; vC2 = darker variant for the stamp's second colour.

**Stroke fragment shader (shared, `FS`, L~520).** Samples the stamp atlas (4x4 grid, cell from vSt). Paper tooth modulates coverage: `cov = s.r + (tooth-.5)*vO.w*uFx.w`. Grow reveal along x (vSt.z). **Impasto relief from screen-space derivatives**: `l = dot(dFdx/dFdy of (s.g + tooth*.1)*a, (.6,-.8)) * uFx.y`, boosted for fresh strokes. Output is premultiplied alpha; additive flag in vO.y.

**Stamp atlas (L256-353).** 1024x512 canvas, 16 shapes in a 4x4 grid of 256x128 cells, generated procedurally: each shape has nb bristles (16-26) with grain/raggedness/load/ridge/frequency/tone/dep; pixel loop computes coverage from bristle profiles with value noise and wobble. Shape 4 is a radial dab; shapes 0-3 are horizontal strokes with different tip profiles (ellipse, sine, flat, noise).

### (b) The 2D paint engine: `createEngine` (L459-673), `createPainter` (L876-1106)

Streets are painted as StrokeSets: items of 24 floats [x, y, len, wid, ang, bend, layer, rnd, palA, palB, lumMix, haze, lum, op, shape, kind, pass, delay, free, only/keep, c2, pig, add, sfree]. Sorted by order key (pass + layer depth).

- **Layers** (PLACES.LAYERS): dome, moon, sun, cloud, far, ground, wall, etc., each with pf (paint-front factor), tw (tile width), bias, late, drift. Palette: 41 named colour slots (`top, mid, hor, fog, win, moonC, sun, ink, white, canvas, cloud, ... neonA, neonB, dark`); the engine throws if `top/mid/hor/fog/win/moonC/sun/ink/white` are missing.
- **Paint front**: `prog = uFrontU.x + LA.w + uLead[ip] - xs - (pass + step(.5,pass)*2.6)*uFrontU.z - delay - hash*.9*uFrontU.y`; `grow = clamp(prog/(uFrontU.y*.7 + len*.55))`. Strokes grow in left-to-right as the front passes; `fresh` = 1 - clamp((prog - uFrontU.y - len)/(uFrontU.y*3)) makes freshly painted strokes slightly darker/wetter. `uOut` fades strokes out on street transitions. 5 passes (uPass[5], uLead[5]): Sketch/Wash/Masses/Touches/Details, toggled in the GUI.
- **Tiling**: xs wraps by tile width around the view centre, so the street repeats infinitely (TW=132 m). Strokes outside the view are culled in the vertex shader.
- **Kinds**: 1 sky (skyCol gradient hor -> mid -> top + moon/sun glow), 2 stars (twinkle), 3 moon, 4 sun, 5/6 windows (lit by uEnv.x threshold), 7 falling (y wraps with time: leaves/petals/rain), 8 snowfall, 9 neon (flicker), 10 moon disc.
- **Kit** (`e.kit`, L678-855): `line` (strokes along a segment, segmented every .72*seg, jittered), `fill` (area fill with golden-ratio low-discrepancy sampling .7548776662 / .569840291, count = cover*area/(len*wid*.62)*DENS), `box`, `slab`, `sketch`.
- **PLACES.compose** (L2853+): builds a street from the kit. Styles: haus (groundH 3.9, floorH 3, roofH 2.4, winN 5, balcony 1, awn .75), village (groundH 3.2, floorH 2.8, roofH 1.1, winN 4, shutters 1), neon (like haus + neon 1). `E()` tests whether a point is in building shadow given the sun direction. Each place def carries W (street half-width, 7 for haussmann), sidewalk 2.4, style, ground, landmark (eiffel), sun/moon positions, light flags, clouds count, falling config, props (tree every 12 m at 6, lamp every 24 m at 0), surf light colours, paperMix .06, and a full palette.
- The painter renders StrokeSets into the far/near/dome render targets; `renderBack`/`renderFront` split background vs foreground strokes. Defaults: density .45, size 1.85, colVar .75, chDens .25, chSize 2.1, chOp 1, trPersist 3, trAmount .75.

### (c) LENS: the fisheye patch and the outline (L1532-1891)

- **`lensProject` GLSL**: rho = length(mv.xy), th = atan(rho, -mv.z), tk = th*uLens.y; r = tk<1.45 ? tan(tk) : 8.238 + (tk-1.45)*70 (linear extension past the tan singularity); r *= uLens.x/uLens.y; depth from distance mapped to near/far. `a.set(fov, fisheye, aspect)`: o = 1 - .92*fisheye; r = min(fov*pi/360, 1.35/o); uLens = (hypot(aspect,1)*o/tan(r*o), o, aspect, 0). At the default fov 175, fisheye 1: o = .08.
- **`a.patch`**: string-replaces `gl_Position = projectionMatrix*mv;` with `gl_Position = lensProject(mv.xyz);` in ANY material's vertex shader, prepending the GLSL. Every material gets fisheye by textual patching. `patchObject` also rewrites the stroke shader's depth offset and tint line (to `charGrade`).
- **`charGrade`**: per-region recolour preserving luminance relative to `regLuma` (mix toward uRegA[r].rgb * clamp(l/uRegL[r], 0, 1.7) by the tint amount), then saturation (uGrade.x) and brightness (uGrade.y).
- **`skinMat`**: ShaderMaterial with three.js skinning chunks; `aReg` attribute, flat `vReg` varying. **Hull variant inflates along normals by `uHull*(0.62 + 0.38*sin(position.y*83 + position.x*61)*sin(position.z*77 + position.y*29))`: a wobbly hand-drawn outline**, not a constant-width shell. Fragment: hull = solid ink; skin = charGrade(texture) x two-tone toon `smoothstep(-.05,.2, N dot L)` between uShadow/uLight, scaled by uGrade.z. Hull = BackSide, skin = DoubleSide.
- **`flat`**: toon `smoothstep(-.05,.15, N dot L)`, optional glow. **`blot`**: radial gradient `pow(clamp(1-d),1.4)*uOp`, premultiplied alpha, custom blending (soft shadows, lamp glows).
- **`castMat` (halftone drop shadow)**: vertex flattens the skinned mesh onto the ground with shear by height (`w.xz += uCast.xy*h; w.y = uCast.z`); fragment = rotated halftone grid, dot radius thins with height (`r = .74*uCastK.y*(1 - uCastK.z*clamp(vH))`); `gl_FragDepth = 0.9999`.
- **`createPost`**: chromatic fringe (radial R/B offset), zoom blur (8 taps), `burst()` impact star-burst (46/23 radial sectors, hashed per-sector, stroke samples from the stamp atlas; three impact modes: threshold/posterize, dot/halftone, line burst), `shards()` outro lens-break (7 sectors x 2 rings = 14 shards sampling the 8-view snapshot atlas, with gap/turn/fall physics), saturation/contrast, vignette, grain.
- **`wordMat`**: word quads; red = letter fill, blue = halo, green = wobble; reveal sweep uRev.
- **Trails** (`createTrail`, L1418-1531): instanced, capacity 1024 (main app) / 1536 default, drift 1, vertex shader patched with `lensProject`. Marked per frame: ball (wid .16), ball2 (+.05y, .08), feet (.1*B), hands (.05*B).

## 7. Beat clock and audio

- **Tempo 144 BPM** (GUI 80-160). `xe()` = 60/bpm = 0.4167 s per beat. `ye()` = 16 beats per street. The character advances one stride per beat along the street (`k = strideWorld() * y`): **the routine travels; the world and camera move, the routine does not stay in place.** Slow-mo warp around beats Be=[4,10,14].
- **Animation time is driven by the audio clock.** In the rAF loop, `a = max(0, ft.time() - ie)`; resync if drift > 3 s or < -.25 s. dt clamped to .05 s. `visibilitychange` stops/starts audio.
- Bus graph: master -> compressor (threshold -20, ratio 4, attack .004, release .18) -> limiter (threshold -7, ratio 20) -> .86 gain -> destination. Inst bus -> WaveShaper with an 11-step quantizer curve (bit-crush) -> beat bus -> lowpass 18 kHz -> master. SFX and amb connect directly to master.
- Instruments (all synthesized, no samples): kick (sine 170 -> 46 Hz pitch drop + noise click), snare (bandpass 2100 noise + triangle 240 -> 150), hat (highpass 8200 noise), crash, sub (sine through tanh waveshaper, pitch 1.6x -> 1x), chip (periodic-wave pulse, vibrato LFO 6.2 Hz), blip (octave pop), tri, pad (detuned sawtooths +/-8 cents through sweeping lowpass), **thok (touch sound: pitch by body part: head 330, chest 120, knee 200, foot 170 Hz; sine 1.7x -> .7x + bandpass noise)**, step (lowpass noise + 80 Hz sine), whoosh (bandpass sweep 2600 -> 300 or 400 -> 3200), glass (outro shatter: highpass noise + 10 sine partials 2100-9300 Hz), boom, rise, scratch, bell (inharmonic partials 1/2.76/5.4), chirp, crackle, air (per-street ambience: haussmann lowpass 420 + bandpass 1800; montmartre + 17 Hz square tremolo + 4300 sine; quais 0.31 Hz LFO on bandpass frequency = water lapping; pigalle highpass 2600 + 100 Hz saw hum).
- Per-street music tables (haussmann, montmartre, quais, pigalle): `duty` (chip pulse width), `ch` chord voicings (4 bars), `roots`, `mel` melody as [beat, MIDI, duration] (11-22 notes over 16 beats). Drum patterns (32 steps): `u` kick, `d` snare, `f` hat (2 = open hat).
- Scheduler `e.emit`: steps 16th notes (h = spb/4); swing `b = y + (v%2 ? .17*h : 0)`. At each beat start, finds the touch (`touches.find(b === S)`) -> blip (pitch from chord + octave by part: head -> top chord note +24, knee -> chord[1]+12, chest -> chord[0]+12, foot -> chord[S%len]+12 or +24 if lob/volley) + thok + boom/crash/kick if lob/volley; whooshes at beats 4 (spin, 1.5p), 13 (jump), 9/11 (atw); steps at v===2; per-street instruments (haussmann chip arpeggio, montmartre chord stabs, quais triangle arpeggio, pigalle chip + pad); melody with swing and octave alternation per loop; crackle/chirp/bell ambience. **Slow-mo: master tone lowpass sweeps down to 18k*(1-.97k)^1.6+260 and back over 1.9 beats.** Outro: rise/kick/crash/thok/whoosh, snare build, glass+boom at brk (lens shatter), bell arpeggio, glass+whoosh at fall.
- `e.create`: lookahead scheduler (setInterval 70 ms, 0.4 s ahead); `time()` = ctx.currentTime - n. `e.render`: OfflineAudioContext 48 kHz, emits the whole duration, fades the last .06 s (used for export).
- Levels: master, beat, inst, sfx, amb; defaults vBeat 1.06, vInst 1.13, vSfx 1.27, vAmb 1.11, volume .8.

## 8. Camera / director

Three modes, default "follow". Settings: fov 175, fisheye 1, camDist 2.35, camHeight .25, orbit 1.05, roll 1.05, punch 1.1, shake 1.15, follow .45.

- **"director"**: `Te` table of 9 shots, each [b0,b1,az0,az1,dist0,dist1,h0,h1,fov0,fov1,lookBall,roll,fisheye], smoothstep-interpolated within its beat window.
- **"ball"**: orbit .75*sin(Y)+.3*sin(2Y+1.3), distance punches on the volley, height follows ball y.
- **"follow"**: orbit .5*sin(Y)+.18*sin(2Y+1.3), distance with punch terms, lookAt lerps toward the ball by .42*follow, **fov = fov + punch*(9J - 6K)** (FOV punches on the volley).
- Shake `ce` from touch bumps; noise `Ge(frame#)`*.09*ce on position; roll += .03*noise*ce; whip `exp(-(ie-pe)/.07)` on shot change. `LENS.set(fov clamped 40-176, fisheye, aspect)`. `Xe()` projects world to screen through the fisheye for word placement.
- Intro (if intro > 0) and outro (ball -> shotPoint -> camera) have dedicated camera blends.
- **Words**: pop on touch beats; big word if lob/volley; positioned at ball + camera-right/up offsets at a seeded random angle; scale pop 1+.35*exp(-t/.045); life .62 s (small) / 1 s (big). Default small words "pac, poc, tac, pok, toc, bop, pouf, tok", big "POW, BAM".
- **Impact**: `_e.imp` 0/1/2/3 + `_e.k` by time since last touch; fed to the post shader's burst.

## 9. Export and UI

- **Export `Dt()`**: VideoEncoder (WebCodecs) + Mp4Muxer. Codec preference avc1.640033 -> avc1.4d0033 -> avc1.42003e -> avc1.640028 -> vp9 -> av1; audio AAC/opus. Size from aspect + res (even-rounded); duration = seconds setting or outro ? full : intro + (total-intro)*loops. Keyframe every 2 s; `encodeQueueSize` backpressure; offline audio via `AUDIO.render`; cancel supported; quality forced high + aa during export. Save via `window.claude.use("downloads")` if present (Claude artifact hook) else anchor download. Filename: `paris-freestyle-<aspect>-<short side>p-<fps>fps.mp4`.
- **Formats**: 9:16 / 4:5 / 1:1 / 16:9, res 1080, fps 30, loops 2, bitrate 16, quality "high", preview "auto", aa true, adapt true. Export modal shows progress bar + video preview + Save.
- **GUI (lil-gui)**: folders Scene (bpm, beatsPerPlace [8,16,24,32,48,64], intro "walk to the ball", outro "End scene: he shoots the lens", outroFall "the shards fall away", outroSeed, street toggles, stride "Walk speed (m per beat)", props, falling), Paint front (frontAnchor camera/player, proxLine/proxBrush/proxDetail, paintSpeed), Character (model/builtin, charHeight 1-2.2, charHead/Hands/Feet, outline, charShade/Sat/Bright, Recolour per region, Brush on him), Camera (camMode, slowmo, impact, words, fov 60-175 "Field of view (diagonal deg)", fisheye, camDist, camHeight, orbit, roll, punch, shake, follow), Sound (volume, vBeat/vInst/vSfx/vAmb), Moves and ball (energy, sway, jump, spin "360 under the lob", ballSize, trails), Brush strokes (density/size/angVar/sizeVar/colVar/relief, Passes: Sketch/Wash/Masses/Touches/Details, on-player, motion strokes), Look (saturation/contrast/haze/zoomBlur/fringe/vignette/grain, safe zones), Halftone shadows (htBall/BallOff/Floor/Len/Turn/Fade/Size/Dot/Angle/Auto/Color, shadowSoft), Ball and built-in figure colours, Preview, Export MP4, Presets (copy/load settings JSON, reset).
- **Header/transport** (index.html): brand, street chips, fps, format select, Settings button; Play/Pause, Restart, Sound, clickable timeline bands, time, Export MP4; JSON settings modal; toast. Keyboard: Space play/pause, R restart.
- `window.__lab` exposes players, sim, draw, seek, exportMP4 for debugging.

## 10. Comparison with our build (Stages 1-12)

Our build: `~/workspace/freestyle-basketball/`, repo builtbysai/freestyle-basketball, live viewer `stage8/viewer.html`. Our sport is basketball (hands, dribble, finger spin); his is soccer freestyle (feet, no hands, no bounce). Differences marked [sport] follow from that choice, not from better engineering. The rest are structural.

### Summary table

| System | His | Ours | Reference |
| --- | --- | --- | --- |
| Routine length | 16 beats, 6.67 s at 144 BPM | 24 beats, 10.0 s at 144 BPM | app.js L4730; `stage2/routine.js:19-22` |
| Routine definition | Touch schedule: 14 touches + SWING array | 33 pose keyframes `K(beat, {...})` | app.js L4730; `stage2/routine.js` (33 `K(`) |
| Interpolation | Closed-form functions of beat time (cos bumps, smoothstep windows) | Non-uniform Catmull-Rom (Barry-Goldman) through keyframes | app.js L4730-5429; `stage2/routine.js:53` |
| Character mesh | Real skinned mesh, 47,087 verts, 51,515 tris, embedded webp texture | Procedural primitives (spheres/capsules/cylinders) on a 60-joint rig | character.js; `stage1/player-model.js` |
| Skeleton | 65 bones, Mixamo-style, 40 finger bones | ~60 joints incl. fingers (4 x 3 segments + thumb per hand) | character.js; `stage1/player-model.js:31-77` |
| Head proportion | 33% of height (headTop 0.3299 of 0.9995) | ~19% (1.35x scale about neck base) | character.js measures; `stage1/player-model.js:321` |
| Character height | 1 m source, scaled to 1.64 m | Root at 1.02, head at 1.68 (~1.9 m total) | app.js settings charHeight 1.64; `stage1/player-model.js:70-77` |
| Legs | World-space groups, IK-driven, not parented to pelvis | IK chains (thigh/shin/foot) parented in the skeleton; feet are keyframed world targets since Stage 11 | app.js PLAYER; `stage2/pose.js:65-66` |
| Root motion | Root y always 0; jump in pelvis | Root y keyframed (e.g. `root: [0, 0.91, 0]`) | app.js `de()`; `stage2/routine.js:98` |
| Ball | Pure projectile between measured contact points, no bounce [sport] | 6 segment kinds: control/bounce/direct/path/spin/snatch; bounce solver with tB; arc-length reparam on paths | app.js `ballAt`; `stage3/ball-drive.js:30,59-67,129-144` |
| Ball radius | Physics .135, visual .11 | Single BALL_R .12 | app.js PLAYER `I`; `stage3/ball-drive.js:23` |
| Ball spin | 13 rev per 16-beat loop, backspin, visual only | 2.5 rev/s on fingertip; spin is a routine segment | app.js `Qe`; `stage3/ball-drive.js:9,25` |
| Brush look | 6,400 instanced stroke quads skinned to the skeleton + 2D painted streets raymarched + fisheye | Procedural canvas brush texture in UV space + inverted-hull outlines (constant px) + baked canvas street paintings on cylinder + ground plane | app.js L1107-1417, L1532-1891; `stage4/look.js:11-13`, `stage5/scene.js:3-17` |
| Outline | Hull inflated by wobbly sine product along normals | Inverted hull, constant px width, N.V weight variation | app.js `skinMat`; `stage4/look.js:107-110` |
| Streets | 4 Paris streets, kit-composed StrokeSets, raymarched corridor, paint front grows ahead of camera | 4 NY streets, baked canvas paintings, backdrop cylinder + ground plane, wipe-arc reveal | app.js PLACES; `stage5/paint-streets.js:7`, `stage5/scene.js:10-14` |
| Camera | 3 modes (director 9-shot table / ball / follow); FOV 175 fisheye default; FOV punch, shake, whip | 17-shot director table; FOV 40-44; no fisheye (deferred) | app.js main; `stage7/director.js:45-54` |
| Audio | Full synth kit (kick/snare/hat/chip/pad + per-part thok + per-street melodies + swing + slow-mo lowpass); animation driven by audio clock | Kick/bass loop + note per touch; touches derived from segment boundaries | app.js AUDIO; `stage6/audio.js:1-13,42-64` |
| Beat clock | Audio clock drives animation; resync if drift > 3 s | Beat clock + audio (separate module `beat-clock.js`) | app.js rAF loop; `stage6/beat-clock.js` |
| Export | WebCodecs VideoEncoder + Mp4Muxer, offline audio render, 4 aspect ratios, quality forced high | MediaRecorder-style (mime avc1 -> vp9 -> vp8), fixed 1920x1080, pixel ratio forced to 1 | app.js `Dt()`; `stage8/export.js:22-25,56-78` |
| Pixel ratio | Locked to 1; preview DPR handled manually (auto = min(dpr,1.5)) | min(DPR,2) live; forced to 1 during export | app.js renderer; `stage8/viewer.html:49` |
| Settings | ~100 settings in lil-gui, localStorage `paris-freestyle-v7`, JSON copy/load presets | settings.js + panel.js + presets.js | app.js GUI; `stage8/settings.js`, `stage8/presets.js` |
| Travelling | Character advances 1 stride per beat; world/camera move | Character stays in place; streets change every 16 beats | app.js `k = strideWorld() * y`; `stage5/scene.js:22` |

### Physics: the differences that matter (Hans flagged weird physical things in ours)

**1. Contact by construction vs contact by audit.** His `fe()` poses the body at each touch beat and reads the actual world position of the contact part (foot pos + footTop, IK knee pos + kneeTop, head pos + headTop + .92*ballR, chest + .95*ballR), then fits the ball arc between those measured points. Ball/body contact is exact by construction; there is nothing to audit. Ours authors ball segments from `handHold(t)` positions (also read from the posed body), but the guided kinds (`path` with hand-placed vias, `control` attached to the hand) can still cross the body mid-segment, which is exactly what the Stage 12 audit found: around-the-world ball through torso/thigh, between-legs ball through thigh, behind-back path speed swinging 5x. His arcs are pure parabolas between surface points with no mid-path authoring, so the only way the ball clips the body is if the parabola itself crosses it. Lesson: where a segment can be a pure parabola between measured points, it should be; guided paths are where clips are born.

**2. Around-the-world is inverted.** In his ATW the ball is the anchor (near the apex of its arc, nearly stationary) and the FOOT orbits the ball in a vertical circle. In ours the ball is guided around the body on a `path` segment with hand-placed side keyframes. His version cannot clip by construction (the foot path is a circle around the ball, radius >= max(1.45*ce, .3L)); ours needed the Stage 12 side-keyframe fix and still has thin clearance by design.

**3. Feet are a state machine, not keyframes.** His `oe()` computes foot state from the SWING schedule: runs of consecutive active beats, z lerped across the run, lift as a sine bump, air flag, planted feet blending near run boundaries. A planted foot cannot slide because its z is held by the run. Ours keyframes foot targets in the routine; foot correctness depends on keyframe placement, which is why Stage 11 had to fix spin leg-crossing with world targets + pre-rotated keyframes + lift 0.30 -> 0.17. His feet are world-space IK targets from the start (legs parented to the world group, not the pelvis), so the 360-degree spin (beats 4.3-5.75, a yaw term on the proxy rig) never drags the feet.

**4. Root never leaves the ground; the pelvis jumps.** His root y is always 0. Jump is pelvis y (parabolic, beats 13.55-14.5) plus differential foot tuck (planted feet rise .9P, airborne .55P). Ours keyframes root y (root [0, 0.91, 0] in stance keyframes) and the jump comes from root motion + keyframed foot lift. His separation (root = travel + sway, pelvis = bob + jump + accents) means the jump never fights the foot planting: the feet are IK targets in world space and the pelvis moves under them.

**5. The ball is simpler because the sport is simpler [sport].** His ball needs one mechanism: exact projectile between measured points, g = 9.81 (the 4.905 = g/2 term), squash at the endpoints, velocity-based stretch (O = .16*clamp(|vy|/7)). No bounce, no hand attachment, no spin segment. Ours needs six segment kinds because basketball has hand control, floor bounces, and a fingertip spin. Our bounce solver (`solveBounce`, `stage3/ball-drive.js:67`) and arc-length reparam (`buildArcTable`/`arcToU`, lines 129-144) are machinery his design never needs. The comparison is not "his ball is better"; it is "his ball has one job and one formula".

**6. Physics radius vs visual radius.** His physics radius (.135) is larger than the visual ball (.11): contact reads generous, so the ball visually overlaps the foot slightly at touches instead of floating. Ours uses a single BALL_R (.12) for both. At hand-offs, a slightly generous physics radius hides small gaps; a single radius exposes them. (Stage 12's worst remaining clips are brief grazes <= 8 cm, <= 0.35 beats at hand-offs.)

**7. Head tracking.** His head tracks the ball (pitch -.6 to +.5, yaw -.7 to +.7), suppressed during head/chest touches. Ours has no ball-tracking head. A head that follows the ball sells the physics of the ball more than any arc math; a head that stares forward while the ball flies reads as disconnected.

### What his build does structurally better (sport-independent)

- **The proxy rig + retarget layer.** Animating a small proxy (root/pelvis/spine/neck/head + world-space legs) and retargeting onto the 65-bone skeleton via bind-offset quaternions `U` decouples the animation from the mesh. Ours animates the skeleton directly; changing proportions means re-authoring keyframes. His `charHead/Hands/Feet` settings rescale parts at retarget time with no animation change.
- **The measure step as an interface.** `fe()` (pose the body, read contact points, fit the ball) is a clean interface between animation and ball. Ours has the same idea in `handHold(t)` for segment endpoints but not for mid-segment contact (the `path` vias are hand-placed, not measured).
- **Strokes as geometry, not texture.** His 6,400 skinned stroke quads move with the body, occlude correctly, and get per-stroke facing/density culling in the vertex shader. Ours is a procedural canvas texture in UV space: strokes stay on surfaces but cannot occlude, vary with facing, or thin on silhouette edges. His stroke fragment shader's impasto relief (screen-space derivatives of the stamp's green channel) is also something a UV texture cannot do.
- **The paint front.** His streets paint themselves ahead of the camera (grow/fresh in the 2D stroke shader, front anchored to camera z or player z). Ours reveals with a wipe arc. His is a property of the stroke system; ours is a property of the backdrop.
- **Audio clock drives animation.** His animation time comes from `ft.time()` (the audio context clock) with drift resync. Ours has a beat clock and audio as separate modules; drift between them is possible in principle.
- **Export is offline and deterministic.** His export renders audio offline (48 kHz OfflineAudioContext) and encodes with WebCodecs + Mp4Muxer with backpressure, quality forced high. Ours records the live canvas in real time.

### What ours does that his does not (for the record)

- Real floor bounces with a solved bounce time (his ball never touches the floor).
- Hand control segments and a fingertip spin (his has no hands in the routine).
- A 24-beat routine with more distinct moves (his 16-beat loop is one juggling pattern with a spin, a lob, two ATWs, and a jump).
- Frame-by-frame verification harness (Stage 11: 48 front + 24 side frames; Stage 12: 481-sample dense audit). His repo has no visible verification harness; the code is the spec.
- Non-uniform Catmull-Rom through authored keyframes gives per-move timing control his closed-form functions do not (his timing is baked into the window constants).

### Unreadable / not verified

- `app.js` is minified: local variable names are single letters. All mechanism descriptions above come from structure, constants, string literals, and GLSL, which survive minification. Where a name is a guess from context (e.g. `fe` = "measure"), the description is of what the code does, not what it is called.
- The PLACES `compose` function (L2853-4729, ~1,900 lines) was read in structure (kit helpers, styles, layer/kind usage) but not line by line; per-street composition details (exact window/awning/shop counts) are not catalogued here.
- `createPainter` internals (L876-1106) and `createTrail` (L1418-1531) were skimmed, not read in full.
- The character texture (`tex`, embedded webp) was not decoded to an image; region colours come from `regCol`/`regLuma` in the data.
- Runtime behaviour (actual frame times, export output, audio sound) was not executed; this is a static read.
