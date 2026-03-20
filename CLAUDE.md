# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-file browser game (`tictactoe.html`) — no build step, no dependencies, no package manager. HTML, CSS, and JavaScript are all co-located in one file.

To run: open `tictactoe.html` directly in a browser.
```
open tictactoe.html
```

## Git workflow

After every change: commit with a descriptive message and push to GitHub.
```
git add <file>
git commit -m "Short imperative summary"
git push
```

The GitHub remote is `https://github.com/kodax-ai/tic-tac-toe` (branch: `main`).

## Architecture

Everything lives in `tictactoe.html` in three sections:

- **`<style>`** — dark-themed CSS using CSS Grid for the board. Color palette: purple `#a78bfa` for X, blue `#60a5fa` for O.
- **`<body>`** — static HTML structure; cells are plain `<div>`s with `onclick="move(i)"` where `i` is the 0–8 board index.
- **`<script>`** — all game logic as module-level variables and functions (no classes).

### Game state (script globals)
| Variable | Purpose |
|----------|---------|
| `board` | `Array(9)` — `null`, `'X'`, or `'O'` |
| `current` | Active player (`'X'` or `'O'`) |
| `gameOver` | Boolean, blocks further moves |
| `mode` | `'pvp'` or `'ai'` |
| `scores` | `{ X, O, draw }` persisted across resets |

### Key functions
- `move(i)` — entry point for a human click; delegates to `place()`, then triggers AI if needed
- `place(i, player)` — mutates `board`, updates the DOM, checks for win/draw
- `checkWin(player)` — returns the winning index-triple or `null`; uses `WINS` constant
- `reset()` — clears `board` and DOM, resets `current`/`gameOver`, keeps `scores`
- `aiMove()` → `bestMove()` → `minimax()` — unbeatable minimax AI; O always maximizes, X minimizes; scores are `10 - depth` (win) / `depth - 10` (loss) / `0` (draw)

### CSS class conventions on `.cell`
- `.taken` — added after any move; disables hover and pointer
- `.x` / `.o` — sets text color
- `.win` — highlights winning cells with glow + pulse animation
