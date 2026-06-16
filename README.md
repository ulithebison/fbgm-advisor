# ⚡ FBGM Advisor

Standalone roster analysis tool for [Football GM](https://play.football-gm.com/) — powered by real game formulas extracted from the [ZenGM source code](https://github.com/zengm-games/zengm).

Every calculation (OVR, aging curves, team OVR weights, draft AI, trade AI) matches the actual game engine. No guessing.

If there are any questions, please DM me here or in this Discord thread: [ZenGM Discord](https://discord.com/channels/290013534023057409/1514707917490622614)

## Features

- **Power Rankings** — Team selection screen with ranked teams, Team OVR (using the game's scale formula), W-L record, average age, and position ranks (#1-#32 per position)
- **Roster & Grades** — Sortable depth chart by position, grades vs. league average, starter/backup hierarchy
- **Draft Center** — Mock draft simulator (all 7 rounds) using the exact AI algorithm, draft pick tracker, hit rate grading, lottery-aware pick order
- **Moves & Contracts** — Cap space overview (real salary cap), bad contracts, fill the gap (FA suggestions by need), extension watch, re-sign watch, sell high, trade candidates, value over replacement (VOR) grouped by position
- **Trades** — Trade Checker with salary cap enforcement using the full ValueChangeCalculator formula (fudgeFactor, EXPONENT, injury discount, pick regression, dynamic revaluation). Shop Player finds offers from all 31 teams for your players and/or picks (1-for-1 up to 3-for-1 combos), with position filters
- **Aging Projections** — 3-season OVR forecasts per player and position group, biggest risers & drops. Supports ProgCode 3.5 custom progression curves
- **Compare** — Team vs. team (position-by-position with weighted OVR) and player vs. player (all 21 ratings side by side, key ratings highlighted)
- **Player Detail** — Click any player name anywhere for all 21 raw ratings, OVR at every position, position switch recommendations, individual aging timeline

## How to Use

1. In Football GM → **Tools** → **Export League** → select **Players**, **All team data**, **Draft picks**, and **League settings** (all four required!) → Download
2. Open the Advisor → drag & drop your JSON or gzip file
3. Select your team from the Power Rankings → explore

Alternatively, use the **Quick Export Bookmarklet** ([bookmarklet.html](https://ulithebison.github.io/fbgm-advisor/bookmarklet.html)) for one-click exports directly from IndexedDB. Supports both direct transfer (opens Advisor automatically) and JSON download.

## Trade Engine

Full port of the game's `ValueChangeCalculator` (~830 lines), including:

| Factor | Status |
|--------|--------|
| Z-Score normalization (OVR mean/std) | ✅ |
| True player value (`p.value`, not fuzz) | ✅ |
| EXPONENT = 3 (football) | ✅ |
| Contract value (salary vs. expected) | ✅ |
| Strategy multipliers (rebuilding/contending) | ✅ |
| fudgeFactor 1.05 × difficulty | ✅ |
| Injury discount | ✅ |
| justDrafted floor | ✅ |
| First round pick protection | ✅ |
| Estimated pick positions (Team OVR → Win%) | ✅ |
| Dynamic pick revaluation (getModifiedPickRank) | ✅ |
| Pick regression + user pick penalty | ✅ |
| Salary cap enforcement (hard cap block) | ✅ |

## Source Code Analysis

Formulas extracted from `zengm-games/zengm`:

| File | What it does |
|------|-------------|
| `player/ovr.football.ts` | Position-specific OVR from 21 raw ratings |
| `team/ovr.football.ts` | Team OVR regression weights (OL ~37%, DL ~33%, QB ~13%) + scale formula |
| `team/ValueChangeCalculator.ts` | Full trade AI with value normalization, strategy, and pick valuation |
| `player/developSeason.football.ts` | Aging curves per position |
| `draft/runPicks.ts` | Draft AI: `(teamOvrDiff + 0.05 × valueFuzz) ^ 40` |
| `player/fuzzRating.ts` | Scouting fuzz: `round(bound(rating + fuzz, 0, 100))` |
| `views/powerRankings.ts` | Power rankings score formula |

## Tech Stack

Single HTML file. React 18 + Babel (in-browser JSX). No build step, no dependencies, no server. Supports JSON and gzip compressed exports.

## License

MIT
