# Spiderlily

An interactive spider lily that grows and blooms in response to your hands, using
MediaPipe hand tracking. It comes in two forms:

- **Web version** (`spiderlily-web/`) — a self-contained Three.js + MediaPipe
  visualiser that runs in the browser. This is the 3D visualiser.
- **TouchDesigner version** (`InteractiveFlower.43.toe`) — the original piece
  built in TouchDesigner.

Both react to the same gesture idea: one hand grows the plant, the other blooms
the flowers.

---

## Web version (3D visualiser)

A single self-contained `spiderlily-web/index.html`. It pulls Three.js and
MediaPipe from a CDN, so there's no build step — but the webcam and hand tracking
require the page to be served over `http://localhost` or `https`, not opened as a
`file://` path.

### Open it

```bash
cd spiderlily-web
python3 -m http.server 8642
```

Then open **http://localhost:8642** in Chrome or Safari and **allow the camera**
when prompted. (Any static server works — `python3 -m http.server` is just the
simplest.)

### Use it

- **Left hand** — pinch out (spread thumb + index) to **grow** the plant
- **Right hand** — pinch out to **bloom** the flowers
- The webcam shows in the bottom-right corner with a live tracking overlay; the
  flowers render over the full screen.
- Press **`D`** to toggle a debug auto-bloom cycle (no hands needed), **`H`** to
  hide the HUD, and drag / scroll to orbit and zoom the camera.

Full interaction details, keys, and tweakable parameters are in
[`spiderlily-web/README.md`](spiderlily-web/README.md).

---

## TouchDesigner version

`InteractiveFlower.43.toe` is the TouchDesigner project file.

### Open it

1. Install **TouchDesigner** (free non-commercial license) from
   [derivative.ca](https://derivative.ca/download).
2. Double-click **`InteractiveFlower.43.toe`**, or launch TouchDesigner and use
   **File → Open** to select it.
3. Press the play/perform controls to run it; a connected webcam drives the hand
   tracking.

> **Note:** the TouchDesigner project uses MediaPipe hand-tracking components. If
> the network shows missing-operator errors on open, install/enable
> TouchDesigner's MediaPipe components (or the matching `.tox` tracking modules)
> so the tracking chain resolves.

---

## Which one should I run?

- Want to try it instantly in a browser with just a webcam? → **Web version.**
- Want the full TouchDesigner piece to edit or perform? → **TouchDesigner version.**
