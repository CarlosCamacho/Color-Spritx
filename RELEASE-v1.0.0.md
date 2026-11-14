# Color Spritx v1.0.0 — Initial Release

**Release date:** November 14, 2026
**Commit:** `505273a`
**File:** `index.html` (single-file, ~135 KB)

Color Spritx is a browser-based pixel-art editor built for the **TRS-80 Color Computer**. It targets the two 4-color graphics modes — **PMODE 1** (128×96) and **PMODE 3** (128×192) — with authentic MC6847 palettes, true 4:3 screen proportions, and exact byte-level packing. Everything the CoCo cares about is modeled correctly, so what you draw is what the hardware shows.

This 1.0 release ships the complete feature set we spent the design cycle building: twenty-four drawing tools, four dithering algorithms, sixteen fill patterns, a spritesheet mode, layers and animation frames, and a fast machine-language BASIC loader.

---

## Download & install

Download `index.html` and open it in any modern browser. No install, no build step, no dependencies to manage. Everything (HTML, CSS, JavaScript) is inlined into a single file.

- Works offline except for the Type tool's Google fonts (they fall back to a system font when offline).
- Project files are plain JSON — portable between browsers and machines.

---

## What's in 1.0

### Faithful CoCo hardware model
- **PMODE 1 (G3C, 3,072 bytes)** and **PMODE 3 (G6C, 6,144 bytes)** with 2 bits per pixel, 4 pixels per byte, MSB-first — exactly the on-wire format.
- Both authentic MC6847 color sets: Green / Yellow / Blue / Red and Buff / Cyan / Magenta / Orange.
- **Square-dot** vs **Fit-to-TV (4:3)** views — the preview mirrors whichever view the canvas is in.
- Full-resolution nearest-neighbor rendering so pixel columns are perfectly uniform at any zoom.
- Zoom from 1/25× (for very large images) to 500× (for single-pixel work).

### 24 tools, two per row
Marquee Selection · Magic Wand · Crop (with drag handles, apply by clicking outside) · Selection Move · Pan · Type · Pencil · Brush · Line · Shape (polyline, double-click to finish) · Spray (density + speed) · Stamp · Box Empty · Box Filled · Circle Empty · Circle Full · Color Picker · Replace Color · Eraser (paints background) · Bucket Fill · Dithering Atkinson · Dithering Noise · Dithering Ordered (Bayer) · Dithering Floyd–Steinberg.

Tool Options shows only the controls that apply to the currently selected tool.

### Dithering and patterns
- Four dither algorithms blend FG → BG across a drag, in 8 directions (45° steps), using only those two colors.
- **16 fill patterns** rendered live in your current FG/BG: Checkerboard, Sparse Checker, Very Sparse, Large Checkers, Horizontal Stripes, Vertical Stripes, Diagonal Lines, Cross-Hatch, Zig-Zag, Brick, Diamonds, Dots Grid, Weave, Plus Signs, Triangles, Scales.
- "Use pattern" toggle on Brush, Bucket Fill, Box Filled, Circle Full, and Spray.

### Type tool
Six Google pixel fonts (Pixelify Sans, Doto, Tiny5, Micro 5, Jacquarda Bastarda 9, Bytesized) with per-tool size. Click to start, drag to position, click again to drop onto a brand-new layer named after the first 12 characters of the text. Hard alpha threshold — no anti-alias fringing.

### Structure
- **Layers** — new, duplicate, delete, merge, reorder, visibility, rename, clear, fill. Index 0 is transparent for compositing.
- **Frames + animation** timeline with play/stop, FPS, and onion-skinning.
- **Spritesheet mode** — grid setup (capped so the sheet never exceeds the PMODE screen), sprite list with thumbnails and editable names, flashing selection border, per-sprite clear/rotate/flip/mirror/copy-to-sprite.

### Import / Export
- Project save/load (JSON) — preserves layers, frames, spritesheet.
- PNG import at native size with PMODE-size validation and a clear warning if the image can't fit a PMODE buffer.
- PNG export (respects the current view).
- Raw CoCo binary and LOADM-wrapped `.BIN`.
- **6809 FCB assembly source** with optional per-sprite name comments.
- **BASIC loader (simple DATA)** — the classic `READ`/`POKE` loop, maximum compatibility.
- **BASIC loader (fast RLE + ML)** — RLE-compresses the image and embeds a 25-byte 6809 machine-language expand routine that POKEs the picture into the screen at machine speed. The ML opcodes, branch offsets, and expansion were verified by simulation. **Requires a 32K or 64K CoCo** (see Known limitations).
- Animation strip export.
- Export guards block invalid (non-PMODE) sizes so you can't accidentally ship a file of the wrong length.

### Interface
- Classic Deluxe-Paint-style gray-bevel chrome.
- Right-column panels for Palette, Patterns, Preview, Tool Options, Layers, Frames, and Sprites — each one collapsible and floatable.
- Overlapping FG/BG color slots in the toolbox; press `X` to swap.
- Status bar with mode, size, byte count, cursor position, active color, and frame/layer info.

---

## Keyboard shortcuts

`B` Pencil · `L` Line · `F` Bucket · `R` Box · `O` Circle · `W` Wand · `S` Select · `K` Picker · `E` Eraser · `T` Type · `P` Pan · `X` Swap FG/BG · `G` Toggle pixel grid · `+/−` Zoom · `,/.` Prev/next frame · `Space` Play/stop · `Ctrl+Z/Y/C/X/V/A/N/S` standard clipboard & file · arrows nudge selection · `Esc` deselect/cancel.

---

## Known limitations

- **Fast BASIC loader requires 32K+ RAM.** The ML routine and data sit at `$6000`. A 16K-safe variant is the top backlog item (Issue #1).
- **Type tool fonts need internet.** The six Google pixel fonts load from Google's CDN; offline use falls back to a system font. Bundled fonts are planned (Issue #2).
- **No session persistence.** Browser storage is intentionally unused; save your work via Project (JSON) export. Autosave is planned (Issue #3).
- **Spritesheet rotation** only applies to square tiles; non-square tiles are left unchanged (Issue #29).
- **PNG import** replaces the current document — no "import onto current layer" yet (Issue #30).
- **PMODE 0/2/4 not supported** in 1.0 (PMODE 4 tracked as Issue #5; CoCo 3 modes as Issue #31).

---

## What's next

The post-1.0 backlog is tracked as open GitHub Issues #1–#20 and #24–#31. The next feature up is **Issue #1 — 16K-compatible fast BASIC loader**, followed by **Issue #2 — offline Type fonts** and **Issue #3 — autosave / session recovery**.

See `backlog.md` for the full prioritized roadmap.

---

## Credits

Created by **Carlos A. Camacho**. © 2026.

Color Spritx is an independent tool for CoCo enthusiasts and is not affiliated with Tandy or the original Color Computer hardware. The MC6847 VDG color sets, PMODE buffer layouts, and all other CoCo-specific details are reproduced from public hardware documentation.
