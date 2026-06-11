# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

No build step required — open `index.html` directly in a browser or serve it locally:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Architecture

Three files, no dependencies, no bundler:

- **`index.html`** — DOM structure: two `<canvas>` elements (`#board` 300×600px, `#next-canvas` 120×120px), sidebar HUD (`#score`, `#lines`, `#level`), and a shared overlay (`#overlay`) used for both pause and game over states.
- **`style.css`** — Dark/retro arcade theme with flexbox layout and `backdrop-filter` on the overlay.
- **`game.js`** — All game logic (~300 lines, `'use strict'`, no classes).

### Key data model (`game.js`)

- `board`: `ROWS × COLS` (20×10) 2D array; `0` = empty, `1–7` = color/piece index.
- `current` / `next`: piece objects `{ type, shape, x, y }` where `shape` is a 2D matrix of color indices.
- `PIECES[1–7]`: piece definitions as square matrices; `rotateCW` uses transpose + row-reverse.
- `collide(shape, ox, oy)`: checks out-of-bounds and overlap against `board`.
- `tryRotate()`: attempts rotation with ±0, ±1, ±2 column wall-kick offsets.

### Game loop

`init()` → `spawn()` → `requestAnimationFrame(loop)`. The `loop` accumulates delta time against `dropInterval`; when exceeded it calls `lockPiece()` which runs `merge()` → `clearLines()` → `spawn()`. If `spawn()` immediately collides, `endGame()` fires.

Speed formula: `dropInterval = Math.max(100, 1000 − (level − 1) × 90)` ms. Level increases every 10 lines.

### Tunable constants (top of `game.js`)

| Constant | Default | Note |
|---|---|---|
| `COLS` / `ROWS` | 10 / 20 | Must also update `<canvas>` dimensions in `index.html` |
| `BLOCK` | 30px | Canvas pixel size per cell |
| `COLORS` | 7 colors | Index matches piece type |
| `LINE_SCORES` | `[0,100,300,500,800]` | Multiplied by current level |
