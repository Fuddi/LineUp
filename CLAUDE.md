# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**LineUp** is a team formation utility for sports (originally built for Lyngby HK U11 handball). It splits a roster of players into two teams, with drag-and-drop rebalancing.

## Running the App

No build step or dependencies required. Open `index.html` directly in a browser, or serve it locally:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080
```

There are no tests, no linter, no bundler, and no package.json.

## Architecture

The entire application lives in a single file: `index.html` (HTML + inline CSS + inline JS).

**Tech stack**: Vanilla JS (ES6+), Tailwind CSS via CDN, `localStorage` for persistence.

**State** (four module-level variables):
- `players` — full roster array, persisted to `localStorage` under key `lu_players`
- `selected` — `Set` of player names present for today's game
- `teams` — `{ a: [], b: [] }` of generated team assignments
- `teamsGenerated` — boolean flag controlling which buttons/views are active

**Three views** (tabs), switched with `goTo(view)`:
1. **Roster** (`view-roster`) — add/delete players, persisted to localStorage
2. **Today** (`view-select`) — mark which players are present; enables "Start Game" when ≥2 selected
3. **Teams** (`view-teams`) — rendered team split; supports drag-and-drop (desktop HTML5 drag events + mobile touch events with ghost element)

**Data flow**: roster → today selection → `doStartGame()` shuffles via Fisher-Yates and splits `Math.ceil(n/2)` into Team A, rest into Team B → `renderTeams()` → user can drag to rebalance via `movePlayer(name, fromTeam, toTeam)` or reshuffle with `doReshuffle()`.

**Security note**: All user-supplied player names are HTML-escaped via `esc(str)` before insertion into the DOM.

## Deployment

Static file — can be hosted on any CDN or static host (e.g. GitHub Pages). Remote: `git@github.com:Fuddi/LineUp.git`.
