# Project & Agent Instructions

## Toolchain & Verification
- **Project Type & Stack:** Single-file browser pixel-art editor (`index.html`, inline HTML/CSS/JavaScript; Bootstrap 5 and optional Google fonts); outputs TRS-80 CoCo PMODE 1/3 6809-compatible graphics and BASIC/ML loaders.
- **Build & Test Command:** `test -s index.html && grep -q '</html>' index.html` (file integrity only; no build system or automated test command exists). Open `index.html` in a modern browser for actual functional verification.
- **Emulator Verification:** For CoCo binaries, test changes in `xroar-dev` (`~/.xroar`) before marking a task complete.
- **CoCo Hardware Corpus:** For any 6809/6309 assembly, GIME, VDG, SAM, PIA, BASIC, or disk work, read from `$HOME/CoCo-Skills` (pre-installed by container setup; do NOT re-clone if present). Only if missing, run `gh repo clone CarlosCamacho/CoCo-Skills "$HOME/CoCo-Skills" -- --depth 1`. Start with `coco-router/SKILL.md` and load only task-relevant child skills.

## Standing Rules
- **CoCoSDC Required:** Every CoCo program must support CoCoSDC (and DriveWire via DSKCON) and must never hang without a disk controller, unless explicitly excluded. Image exports and standalone BASIC loaders that do not access disks should remain disk-independent.
- **Hardware Discipline:** Preserve Direct Page (`DP`) across interrupts/subroutines and strictly respect 8KB MMU bank boundaries when developing CoCo 3/native code; PMODE 1/3 targets use CoCo 1/2 VDG constraints.
- **Context Hygiene:** Keep edits strictly focused; do not refactor unrelated files.
- **Format Discipline:** PMODE 1 is 128x96/3072 bytes and PMODE 3 is 128x192/6144 bytes; both use 2bpp, four pixels per byte, MSB first. Validate on real/emulated CoCo.
- **Single-File Discipline:** Source of truth is `index.html`; there is no generated bundle. Keep `README.md` and `backlog.md` accurate after functional changes.

## Mandatory Session Handoff Rule
- Before ending a session, closing out, or opening a PR, you MUST overwrite root `HANDOFF.md` (strictly under 35 lines) with:
  1. Current branch/commit status and what builds cleanly
  2. Immediate next backlog task
  3. Dead ends / failed approaches tried this session that must not be repeated
- Always include updated `HANDOFF.md` in your commit/PR.
