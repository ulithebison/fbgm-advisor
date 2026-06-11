# ⚡ FBGM Advisor

Standalone roster analysis tool for [Football GM](https://play.football-gm.com/) — powered by real game formulas extracted from the [ZenGM source code](https://github.com/zengm-games/zengm).

Every calculation (OVR, aging curves, team OVR weights, draft AI) matches the actual game engine. No guessing.

## Features

- **Roster & Grades** — Sortable depth chart by position, grades vs. league average, starter/backup hierarchy
- **Draft Center** — Mock draft simulator (all 7 rounds) using the exact AI algorithm, draft pick tracker, hit rate grading, lottery-aware pick order
- **Moves & Contracts** — Cap space overview (real salary cap), bad contracts, fill the gap (FA suggestions by need), extension watch, sell high, trade candidates, value over replacement (VOR)
- **Aging Projections** — 3-season OVR forecasts per player and position group, biggest risers & drops
- **Compare** — Team vs. team (position-by-position with weighted OVR) and player vs. player (all 21 ratings side by side, key ratings highlighted)
- **Player Detail** — Click any player for all 21 raw ratings, OVR at every position, position switch recommendations, individual aging timeline

## How to Use

1. In Football GM → **Tools** → **Export League** → select **Players**, **All team data**, and **Draft picks** → Download
2. Open the Advisor → drag & drop your JSON file
3. Select your team → explore

Alternatively, use the **Quick Export Bookmarklet** (`bookmarklet.html`) for one-click exports directly from IndexedDB.

## Source Code Analysis

Formulas extracted from `zengm-games/zengm`:

| File | What it does |
|------|-------------|
| `player/ovr.football.ts` | Position-specific OVR from 21 raw ratings |
| `team/ovr.football.ts` | Team OVR regression weights (OL ~37%, DL ~33%, QB ~13%) |
| `player/developSeason.football.ts` | Aging curves per position (speed positions decline after 27, IQ positions hold until 33+) |
| `draft/runPicks.ts` | Draft AI: `(teamOvrDiff + 0.05 × valueFuzz) ^ 40` |
| `player/fuzzRating.ts` | Scouting fuzz: `round(bound(rating + fuzz, 0, 100))` |

## Tech Stack

Single HTML file. React 18 + Babel (in-browser JSX). No build step, no dependencies, no server. Just open the file.

## License

MIT
