# Interactive Spider Lily — web version

A coded port of the TouchDesigner piece: MediaPipe hand tracking grows and blooms
a procedural spider lily plant rendered full-screen, with your webcam as a
window in the bottom-right corner (the flowers overlay it, slightly see-through).

The bloom is a staged **anime.js timeline** (based on an 18-frame botanical
reference): tight bud → petals lift & separate → arch outward & curl back →
stamens extend → full bloom → settle. Your hand (or the debug cycle) *scrubs*
this timeline, so the stages always play in order, forward or backward.
Each flower head is an umbel of several florets, like a real Lycoris.

## Run it

```bash
cd spiderlily-web
python3 -m http.server 8642
# open http://localhost:8642 in Chrome/Safari and allow the camera
```

(Any static server works. Camera + hand tracking require http://localhost or https.)

## Interaction

- **Left hand** — pinch out (spread thumb + index) = **grow** the plant
- **Right hand** — pinch out = **bloom** the flowers
- Grow tracks your pinch linearly and reaches full plant at 75% of a
  full spread (`PARAMS.interaction.growFullAt`).
- Hands are identified by MediaPipe handedness, so the mapping holds even
  if you cross your hands. If your setup reports them backwards, set
  `PARAMS.interaction.swapHands = true`.
- The camera window shows a live tracking overlay: white dots on your
  thumb and index tips joined by a thin white pinch-distance line, with
  the grow/bloom values labeled beside each hand. Tracking runs even in
  debug mode, so you can verify MediaPipe is seeing your hands at any time.
- The bloom plays the staged choreography: petals lift → arch & curl back →
  stamens extend (each filament individually, sagging wide then sweeping
  up at the tip) → settle
- All flowers bloom **in sync**, tracking your hand 1:1. To make them take
  turns instead, raise `PARAMS.flowers.bloomStagger` toward `1`
  (strictly sequential).

## Keys

| Key | Action |
|-----|--------|
| `D` | toggle debug mode (auto bloom cycle) vs live hands |
| `1` / `2` / `3` | pin bloom at bud / arching / full (debug) |
| `0` | resume the auto cycle |
| `H` | hide/show the HUD |
| drag / scroll / right-drag | orbit / zoom / pan the camera (it also slowly auto-rotates) |

## Tweaking

Everything lives in the `PARAMS` object at the top of `index.html` —
plant shape, petal count/recurve/ripple, stamen count/length/emergence,
colors, glow, pinch ranges, camera framing. Each value is commented.

`PARAMS` is also exposed as `window.PARAMS`, so you can tweak live from
the browser console, e.g.:

```js
PARAMS.stamens.count = 12        // takes effect on reload-built things
PARAMS.glow.strength = 1.6       // some values apply immediately
```

(Structural values like counts/seed need a reload; per-frame values like
glow, tilt angles, and emergence thresholds apply live.)

## Structure of index.html

1. `PARAMS` — all tweakables, including `bloomPoses` (the key poses of the
   bloom choreography: bud / lift / arch / full / settle)
2. bloom timeline — anime.js keyframes `bloomState` through the reference's
   18 frames over a virtual 1800ms; hand input scrubs it with `.seek()`
3. plant skeleton — recursive branching rendered as solid tapered stems
   (the lsystem equivalent); pedicels stay fanned, flowers bloom on top
4. flower heads — umbels: 3 florets per head pointing away from each other
5. petal geometry — curved blade rebuilt each frame (recurve + edge ripple;
   negative recurve = curled inward for the bud), painted with layered
   vertex colors: pale pink throat, midrib highlight, crimson edges/tips,
   faint veins
6. stamen geometry — 6 filaments per floret rooted at its throat, each
   sagging wide/down then sweeping up at the tip (declinate, like the real
   flower), golden anther riding the tip tangent
7. hand tracking — MediaPipe HandLandmarker, pinch distance → grow/bloom
8. main loop — time-based smoothing (lag), debug cycle, render with UnrealBloom

To retune the choreography itself, edit `PARAMS.bloomPoses` (the poses) or
the timeline offsets/easings in the `bloomTimeline` block (the timing).
