# Current Baton Pass
- **Current State:** `main` at `1cd5aff` before handoff setup; Color Spritx is a single-file browser editor for CoCo PMODE 1/3 with palette tools, layers, frames, spritesheets, dithering and CoCo export. Last five commits concern screenshot upload/rename and README references, not runtime changes; no automated tests or build system.
- **Immediate Next Step:**
  - Prioritize the highest-priority open GitHub Issue after backlog migration, initially the 16K-compatible fast BASIC loader from `backlog.md`.
  - Add verification with xroar-dev for BASIC/ML loader output and preserve CoCo binary packing.
- **Dead Ends / Do Not Repeat:** None recorded yet; `claude.md` and `CLAUDE.md` were absent on the sole `main` branch. The current fast RLE+ML loader targets 32K+ machines, so do not claim 16K support.
- **Verification:** Run `test -s color-spritx.html && grep -q '</html>' color-spritx.html` (integrity only); open `color-spritx.html` in a browser and validate UI. Test exported CoCo BASIC/ML programs in `xroar-dev`. No build step exists.
