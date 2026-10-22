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
- Flow: `requestAnimationFrame(loop)` accumulates time and steps the piece every `dropInterval` → `lockPiece()` (`merge` → `clearLines` → `spawn`). `spawn()` calls `endGame()` if the new piece collides. Pause cancels the RAF and `togglePause` restarts `loop`.
- Scoring/speed are computed in `clearLines` (`LINE_SCORES × level`, level = lines/10 + 1, `dropInterval = max(100, 1000 − (level−1)·90)`); soft/hard drop add points directly.
- Canvas size is hardcoded in `index.html` (`300×600` board, `120×120` next preview); changing `COLS`/`ROWS`/`BLOCK` in `game.js` requires updating the canvas `width`/`height` to match.
- Records: `localStorage["records"]` = `{top:[{name,score,lines}] (max 5), bestCombo, maxLines}`, cargado/validado en `loadRecords()`. Al cargar solo se muestra el overlay de inicio (`gameOver = true`); `restart-btn` ("Jugar"/"Reiniciar") llama a `init()`. `combo` cuenta piezas consecutivas que limpian líneas (en `clearLines`). `endGame()` muestra el form de nombre si entra en el top 5; el keydown global ignora eventos de `name-input`.
