# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**OkosTojás** is a gamified math practice web application for Hungarian 2nd-grade students. It is a fully client-side, vanilla HTML/CSS/JavaScript app with no build system, bundler, or external dependencies.

## Running Locally

```bash
python3 -m http.server 8000
# or
npx http-server -p 8000
```

Then open `http://localhost:8000`. Deployment to GitHub Pages is automatic on push to `main`.

## Architecture

### Page Flow
1. **`index.html`** — Main menu: player selection, theme/game-type selection, difficulty config
2. **`game.html`** — Active game: question display, answer input, real-time feedback
3. **`leaderboard.html`** — All-time scores and per-user stats
4. **`tasks.html`** — Per-task statistics broken down by user

### JavaScript Modules (`js/`)
- **`config.js`** — Hero tier titles, encouragement/error messages, game constants
- **`storage.js`** — All `localStorage` read/write: leaderboard, player stats, task history
- **`game.js`** — Game engine: question generation, adaptive selection, scoring, boss battle logic
- **`mascot.js`** — SVG mascot rendering and tier-based character evolution

### Data Persistence (`localStorage`)
All data lives in browser `localStorage`. Key formats:
- Leaderboard: array of score objects
- Player stats: object keyed by normalized player name
- Task history: `{ username: { taskKey: [{ c: 0|1, t: timestamp }, ...] } }`
  - Multiplication task keys: `multiply:base:multiplier`
  - Addition task keys: `add:limit:num1:num2:operator`

### Adaptive Question Selection
Tasks with higher error rates are weighted more heavily in random selection. This logic lives in `js/game.js`.

### Scoring
- 100 points per correct answer
- Speed bonus: up to 100 extra points (full bonus for answers under 20 seconds)
- -20 points per wrong answer (total never goes negative)

### Game Modes
- **Normal:** Fixed number of questions (10/15/20/25/30)
- **Boss Battle:** Defeat boss by getting 10 consecutive correct answers within 60 attempts; visualized with heart HP

### Mascot / Hero System
- 10 tiers based on cumulative points (500-point intervals), defined in `config.js`
- Inactivity regression: tier penalties after 4+ days without practice
- Per-user mascot state tracked in `localStorage`

### Themes
Two themes (girl: pink/purple, boy: blue/green) controlled via CSS classes and stored per user in `localStorage`. All CSS is in `css/style.css`.

## Language
All UI text is in **Hungarian**. Keep new user-facing strings in Hungarian.
