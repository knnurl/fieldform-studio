# Fieldform Studio

**Fieldform turns mathematical fields into 3D CNC-ready relief carvings.** Pick a field
mode, sculpt it with a handful of sliders, check that a ballnose can actually cut it,
and export a watertight STL (in millimetres) or a grayscale heightmap — all in the
browser, with no build step and nothing to install.

It's a single self-contained HTML file. **Double-click it to run.** Everything —
the maths, the 3D preview, the machinability analysis and the exporters — lives in
that one file.

```
fieldform_studio.html   ← the app
```

Open it in a modern browser (Chrome or Firefox recommended — see
[Browser support](#browser-support)).

---

## Table of contents

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Field modes](#field-modes)
- [The four tabs](#the-four-tabs)
  - [Pattern](#pattern-tab)
  - [Stock](#stock-tab)
  - [Machining](#machining-tab)
  - [Project](#project-tab)
- [Symmetry fold (kaleidoscope)](#symmetry-fold)
- [Presets, randomize & seeds](#presets-randomize--seeds)
- [Config library](#config-library)
- [Exporting](#exporting)
- [Reading the machinability report](#reading-the-machinability-report)
- [Themes & appearance](#themes--appearance)
- [Reproducibility](#reproducibility)
- [Browser support](#browser-support)
- [License](#license)

---

## What it does

Fieldform samples a 2-D scalar field over your stock, normalises it to a 0–1 height
map, and lifts it into a **single-valued relief** — a surface where every point has
one height, which is exactly what a 3-axis mill needs. It then:

- builds a **watertight solid** (top surface + flat floor + four walls) so the STL is
  manifold and slicer/CAM-safe;
- runs a **machinability check** at export resolution — slope, concave radius vs.
  tool radius, flute reach and scallop height — and tells you where a given ballnose
  would gouge or can't reach;
- keeps the preview, the on-screen report and the exported file in lockstep, so
  what you see is what you cut.

Typical use: decorative panels, mould masters, foam/MDF/hardwood signage and
sculptural relief.

---

## Quick start

1. Open **`fieldform_studio.html`**. A short guided tour runs the first time; reopen
   it anytime with the **?** button.
2. On the **Pattern** tab, pick a **Field mode** and drag sliders, or click a
   **preset**, or hit **🎲 Randomize** until you like the shape.
3. On the **Stock** tab, set your block size in millimetres (`X × Y × Z`) and the
   **retained floor**.
4. On the **Machining** tab, choose a **material preset** (or set your tool by hand)
   and watch the status light: **green = machinable**.
5. On the **Project** tab, click **↓ Watertight STL** to export.

That's the whole loop. Everything else below is detail.

---

## Field modes

The **Field mode** dropdown chooses the underlying maths. The five shape sliders
below it (Symmetry, Harmonics, Ring density, Balance, Spiral) **change meaning per
mode** — their labels update when you switch, and the table shows what each controls.

| Mode | Character | "Symmetry" | "Balance" | "Spiral" |
|---|---|---|---|---|
| **Coaxial** | Counter-rotating harmonograph rings | angular symmetry | counter-rotation mix | spiral twist |
| **Chladni** | Cymatic standing-wave plate | mode order | symmetric ↔ antisymmetric | (inactive) |
| **Flux** | Magnetic field lines (complex potential) | pole count | dipole ↔ monopole | field swirl |
| **Resonance** | φ-quasiperiodic plane-wave stack | rotational fold | phase disorder | golden twist |
| **Superformula** | Deformed concentric rings | superformula *m* | deform strength | twist per radius |
| **Phyllotaxis** | Golden-angle bump field (sunflower) | spiral family | bump width | (inactive) |
| **Moiré** | Two detuned counter-rotated gratings | grating fold | detune angle | scale detune |

Every mode is seeded, so the **seed** and **🎲** explore variations while the sliders
set the overall character.

---

## The four tabs

### Pattern tab

The shape itself.

- **Field mode** + the five shape sliders (see the table above).
- **Symmetry fold** — clip the pattern to a wedge and repeat it around the centre;
  see [Symmetry fold](#symmetry-fold).
- **Composition** — layer big form over fine detail:
  - **Macro carrier** / **Carrier scale** — a low-frequency swell that gives the
    panel an overall form the pattern rides on.
  - **Quiet zones** / **Quiet scale** — softly mute part of the area so the pattern
    breathes instead of filling every millimetre.
  - **Origin X/Y** + **Pattern zoom** — slide and scale the pattern window over the
    block.
  - **Cross-section** shaper — remap the height profile: *Plateau* (flat crests),
    *Terrace* (stepped levels), *Cove* (flat valleys) — all still single-valued and
    machinable — with a **Shaper strength**.
- **Detail** — harmonic falloff, radial decay, domain **warp** amount/scale, and
  **Mesh resolution** for the live preview.
- **Presets**, **🎲 Randomize / ↶ Undo**, and **seed** stepping.

### Stock tab

The physical block and how the field maps into it.

- **X · Y · Z** — stock size in millimetres.
- **Retained floor** — solid material kept under the relief.
- **Depth scale** / **Contrast (γ)** — how the 0–1 field stretches into millimetres.
- **Flat edge margin** — ramp the relief down to the floor at the border.
- **Invert relief** and **Machinability smoothing** (+ radius) — pre-blur detail a
  ballnose could never resolve.
- **Appearance** — **Block colour** picker plus quick swatches, for taste and
  contrast while you work.

### Machining tab

Will it actually cut?

- **Material preset** — *Foam / MDF / Hardwood* set sensible ballnose, stepover and
  feed starting points (editing any tool field flips it to *Custom*).
- **Tool** — ballnose Ø, flute length, feed.
- **Stepover** — finishing pass spacing (drives scallop height).
- **Show machinability overlay** — paint problem zones onto the 3D model.
- A live **report** — see [Reading the machinability report](#reading-the-machinability-report).

### Project tab

Export, reuse and reset.

- **Export resolution** and **↓ Watertight STL · mm** / **↓ Heightmap (8-bit)**.
- **Save / Import preset** (`.json`).
- **Config library** — link a folder of configs for one-click switching; see
  [Config library](#config-library).
- **Reset to defaults**.

---

## Symmetry fold

**Symmetry fold** (Pattern tab) turns any field into a rosette. It clips the pattern
to one angular wedge and stamps that wedge evenly around the centre:

- **Pieces around** — how many wedges (2–16).
- **Mirror alternate (kaleidoscope)** — reflect every other copy so seams meet as
  true mirror lines.
- **Fold rotation** — spin the whole rosette.

The fold is applied before the domain warp, so every copy is identical down to the
fine detail — the result is exactly *n*-fold symmetric.

---

## Presets, randomize & seeds

- **Presets** are exact addresses: clicking one sets the mode, seed and every form
  parameter at once (your stock, floor, resolution and tool settings are never
  touched).
- **🎲 Randomize** rolls every unlocked shape slider and a fresh seed. **↶ Undo**
  steps back through the history.
- The **🔒 lock** beside any slider pins it, so Randomize skips it.
  **Quiet zones**, **Macro carrier** and **Pattern zoom** start locked.
- **Seed** — the same seed + parameters always reproduces the same surface. Use the
  **← →** arrows to walk seeds and **↻** for a random one.

---

## Config library

*(Chrome only.)* On the **Project** tab, **🔗 Link configs folder** lets
you pick a local folder of `.json` setups. They appear in a dropdown you can switch
between with one click, and **↓ Save into configs folder** writes the current setup
back. The linked folder is remembered between sessions.

An example config lives in [`configs/`](configs/) — import it from **Project →
Import preset**, or drop it in your linked folder.

Under the hood this uses the browser's File System Access API. If your browser
doesn't support it, use **Save / Import preset** instead — same `.json` format.

---

## Exporting

- **Watertight STL (mm)** — a manifold solid at your chosen **Export resolution**,
  named with the stock size and seed. A matching preset `.json` sidecar downloads
  alongside it so the file is always reproducible. The STL header embeds the mode,
  seed and key parameters.
- **Heightmap PNG** — an 8-bit grayscale height field at export resolution, for
  displacement workflows or documentation.

> **Tip:** keep **Export resolution ≥ preview resolution**. If it's lower, the app
> warns you that the STL will be coarser than what you see.

---

## Reading the machinability report

The report is computed **at export resolution** (a finer grid resolves tighter
curvature), so the numbers describe the file you'll actually ship, not just the
preview. The colours in the overlay:

| Colour | Meaning |
|---|---|
| 🟢 green | fine — within slope and reach limits |
| 🟠 orange | steep (> 70°) — cuttable but slow / poor finish |
| 🟣 purple | flute too short to reach the valley floor |
| 🔴 red | tool too large — the ballnose would gouge a concave feature |

Key figures: **tightest concave radius vs. tool radius** (must be ≥ tool radius to
avoid gouging), **flute reach** vs. relief depth, and **scallop height** at your
stepover. The status **LED goes green** when the flagged area is negligible.

---

## Themes & appearance

- **◑ / ☀** toggles light/dark; your choice is remembered.
- **Block colour** (Stock → Appearance) recolours the previewed material — pick
  whatever reads best for the shape you're judging.

---

## Reproducibility

A relief is fully determined by its parameters and seed. That means:

- the same preset always regenerates the same surface;
- the STL sidecar `.json` captures everything needed to rebuild the exact part;
- the STL's 80-byte header records the mode, seed and key settings in plain text.

---

## Browser support

- **Chrome** — full support, including the config library (File System Access API).
- **Firefox** — everything works *except* the config-folder linking (that API is
  Chromium-only); use **Save / Import preset** for `.json` instead.
- Other Chromium browsers (Edge, Brave) also support the config library; Safari
  works apart from folder linking.
- Requires WebGL for the 3D preview.

No network access is needed to run — the only external requests are for the web
fonts and the Three.js library from a CDN.

---

## License

MIT — see [`LICENSE`](LICENSE).
