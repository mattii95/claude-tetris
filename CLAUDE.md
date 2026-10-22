# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Tetris in vanilla JS + HTML5 Canvas. No dependencies, no `package.json`, no build, no linter, no tests. README and UI text are in Spanish.

## Running

Open `index.html` directly, or serve statically (`python -m http.server 8000`) and visit `http://localhost:8000`.

## Architecture

- `game.js` is a single script (global scope, `'use strict'`) holding all logic; `index.html` supplies the DOM ids it looks up at load (`board`, `next-canvas`, `score`, `lines`, `level`, `overlay`, `overlay-title`, `overlay-score`, `restart-btn`). Renaming an id in the HTML requires updating the lookups at the top of `game.js`.
- Game state lives in module-level `let` variables (`board`, `current`, `next`, `score`, …) reset by `init()`, which is also the restart handler.
- Piece/board cells store a color index 1–7 into `COLORS`/`PIECES`; `0` is empty. Pieces are matrices rotated by `rotateCW` with simple horizontal kicks (`tryRotate`).
- Flow: `requestAnimationFrame(loop)` accumulates time and steps the piece every `dropInterval` → `lockPiece()` (`merge` → `clearLines` → `spawn`). `spawn()` calls `endGame()` if the new piece collides. Pause (`P`/`Escape`) shows the shared overlay as a menu (resume/restart/controls/`#start-level` select, persisted in `localStorage` as `startLevel`; `init()` starts at `startLevel`, level = `startLevel + lines/10`); keydown only accepts P/Escape while paused. Pause cancels the RAF and `togglePause` restarts `loop`.
- Scoring/speed are computed in `clearLines` (`LINE_SCORES × level`, level = startLevel + lines/10, `dropInterval = max(100, 1000 − (level−1)·90)`); soft/hard drop add points directly.
- Canvas size is hardcoded in `index.html` (`300×600` board, `120×120` next preview); changing `COLS`/`ROWS`/`BLOCK` in `game.js` requires updating the canvas `width`/`height` to match.
