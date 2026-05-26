# Color Spritx — Product Backlog

This document tracks the project's delivered work, known limitations, and a prioritized list of future ideas. It's organized so anyone picking up the project can quickly see what exists, what's rough, and what to build next.

Legend: ✅ done · 🟡 partial / has caveats · ⬜ not started

---

## 1. Delivered

### Core editor
- ✅ Single self-contained HTML file (HTML + CSS + JS inlined)
- ✅ PMODE 1 (128×96) and PMODE 3 (128×192) support
- ✅ Authentic MC6847 4-color palettes (Set 0 and Set 1)
- ✅ Index-based pixel storage with instant palette re-tinting
- ✅ Square-dot vs. Fit-to-TV (4:3) views; 4:3 default on
- ✅ Full-resolution nearest-neighbor rendering (uniform pixel columns)
- ✅ Zoom from 1/25× to 500× with multiplicative stepping
- ✅ Deluxe-Paint-style gray-bevel UI; collapsible/floating panels
- ✅ Live preview window mirroring the canvas view
- ✅ Undo / redo

### Tools (24)
- ✅ Marquee Selection, Magic Wand, Crop (resizable box), Selection Move
- ✅ Pan, Type
- ✅ Pencil, Brush, Line, Shape (polyline)
- ✅ Spray (airbrush, density + speed), Stamp
- ✅ Box Empty/Filled, Circle Empty/Full (border thickness for outlines)
- ✅ Color Picker, Replace Color, Eraser (paints background), Bucket Fill
- ✅ Dithering: Atkinson, Noise, Ordered (Bayer), Floyd–Steinberg
- ✅ Per-tool option panel showing only relevant settings

### Color, patterns, dithering
- ✅ Overlapping FG/BG color slots; X to swap
- ✅ 16 fill patterns rendered in live FG/BG
- ✅ "Use pattern" toggle on Brush, Bucket, Box Filled, Circle Full, Spray
- ✅ Two-color directional dither gradients (8 directions)
- ✅ User-settable tile-grid line color

### Structure
- ✅ Layers: new, duplicate, delete, merge, reorder, visibility, rename, clear, fill
- ✅ Frames + animation timeline with onion-skinning
- ✅ Spritesheet mode: grid setup, sprite list, rename, clear, rotate, flip, mirror, copy-to-sprite, flashing selection

### Type tool
- ✅ Six Google pixel fonts, selectable size
- ✅ Hard-thresholded (no anti-alias) rendering
- ✅ Click-to-start, drag-to-move, click-to-commit onto a new auto-named layer

### Import / Export
- ✅ Project save/load (JSON, includes layers/frames/spritesheet)
- ✅ PNG import at native size with size validation
- ✅ PNG export (respects current view)
- ✅ CoCo raw binary + LOADM `.BIN`
- ✅ 6809 FCB assembly source (with optional sprite-name comments)
- ✅ BASIC loader — simple DATA version
- ✅ BASIC loader — fast RLE + machine-language version (verified expansion, protected memory)
- ✅ Animation strip export
- ✅ Export guards that block invalid (non-PMODE) sizes

---

## 2. Known Limitations & Caveats

- 🟡 **Fast BASIC loader requires 32K+ RAM.** The ML routine and data sit at `$6000`. A 16K-friendly variant (lower load address, smaller `CLEAR`) is not yet offered.
- 🟡 **Type tool fonts need internet.** The Google fonts load from Google's CDN; offline use falls back to a default font. Bundling the fonts (or a built-in bitmap font) would make it fully offline.
- 🟡 **No persistence between sessions.** Browser storage is intentionally unused; work must be saved via Project (JSON) export. An optional autosave/restore would help.
- 🟡 **Spritesheet rotate** is only meaningful for square tiles; non-square tiles are left unchanged on rotate.
- 🟡 **Single-image PNG import** replaces the document; there's no "import onto current layer" or batch import.
- 🟡 **No PMODE 0/2/4 support.** Only the two 4-color modes (PMODE 1 and 3) are modeled.

---

## 3. Backlog — Prioritized

### High priority
- ⬜ **16K-compatible fast loader** — pick a lower load address and matching `CLEAR`, or auto-select based on a target-RAM dropdown in the export dialog.
- ⬜ **Offline Type fonts** — bundle a small built-in pixel font (or embed the Google fonts as base64) so the Type tool works with no connection.
- ⬜ **Autosave / session restore** — keep an in-memory or downloadable recovery snapshot so an accidental tab close doesn't lose work.
- ⬜ **Export dialog** — a single dialog consolidating format, target RAM, load address, and sprite-name options, instead of separate menu items.

### Medium priority
- ⬜ **PMODE 4 (256×192, 2-color)** support, including its different pixel packing.
- ⬜ **Resize / canvas-size dialog** for non-destructive growing/shrinking, padding, and re-anchoring.
- ⬜ **Custom palette remap on PNG import** — let the user choose which CoCo colors map to which source colors, rather than nearest-RGB only.
- ⬜ **Per-frame duration editing** in the animation timeline (currently a single global FPS).
- ⬜ **Sprite extraction tools** in spritesheet mode — export individual sprites as separate binaries/FCB blocks with per-sprite load offsets.
- ⬜ **Selection transforms beyond 90°** — arbitrary-angle rotate, free scale with preview.
- ⬜ **Configurable ML loader memory location** for embedding the fast loader inside larger BASIC programs.

### Lower priority / nice-to-have
- ⬜ **Tile/map editor** — lay sprites from a spritesheet onto a larger scrollable map and export the map data.
- ⬜ **Additional dithering algorithms** (Sierra, Burkes, Stucki) and a 2-color **threshold/halftone** brush.
- ⬜ **Gradient tool** that isn't dither-based (pattern banding presets).
- ⬜ **Symmetry / kaleidoscope drawing modes** (the old X/Y mirror, reworked as an opt-in drawing aid).
- ⬜ **Reference-image overlay** — load a photo as a dimmed background to trace over.
- ⬜ **Onion-skin opacity control** and color-coding options.
- ⬜ **Keyboard shortcut for every tool**, plus a remappable shortcuts panel.
- ⬜ **Color-cycling preview** to simulate CoCo palette animation tricks.
- ⬜ **Direct emulator handoff** — "open in MAME/XRoar" style export helpers or `.dsk` packaging.

### Code health / non-feature
- ⬜ Split the single file into source modules with a documented inline-build step (the build is currently a manual one-liner).
- ⬜ Expand automated tests for the spritesheet transforms and the animation-strip export.
- ⬜ Add a visual regression harness so UI changes are screenshot-diffed.
- ⬜ Document the JSON project schema and bump a version field for forward compatibility.

---

## 4. Open Questions

- Should the fast loader's compression switch automatically to raw bytes when RLE doesn't help (e.g., very noisy images where pairs exceed raw size)?
- Is there demand for **CoCo 3** modes (more colors, higher resolution), which would be a substantial new code path?
- For spritesheets, what's the most useful default export — one combined sheet binary, or separate per-sprite files?

---

_Maintained alongside Color Spritx. © 2026 Carlos A. Camacho._
