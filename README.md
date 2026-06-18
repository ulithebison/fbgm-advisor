# ⚡ FBGM Advisor

Standalone roster analysis tool for [Football GM](https://play.football-gm.com/) — powered by real game formulas extracted from the [ZenGM source code](https://github.com/zengm-games/zengm).

Every calculation (OVR, aging curves, team OVR weights, draft AI, trade AI) matches the actual game engine. No guessing.

If there are any questions, please DM me here or in this Discord thread: [ZenGM Discord](https://discord.com/channels/290013534023057409/1514707917490622614)

## Features

- **Power Rankings** — Team selection screen with ranked teams (#1-#32), tier grades (A-F), Team OVR (using the game's scale formula), W-L record, average age, and position ranks per position, all color-coded by tier
- **Roster & Grades** — Sortable depth chart by position, grades vs. league average, starter/backup hierarchy, priority need ranking
- **Roster Health** — Complete roster analysis per position: starter quality vs league, depth evaluation, 3-year outlook, rising players count. Identifies NEED, DEPTH, AGING, and SURPLUS positions. Expandable player lists with Trade buttons. Trade Assets section shows surplus players to deal, Priority Targets shows where to add talent
- **Draft Center** — Mock draft simulator (all 7 rounds) using the exact AI algorithm, draft pick tracker, hit rate grading, OVR/POT fuzz reveal (+/- columns showing true vs scouted values), Sleeper Alert for undervalued prospects, and Draft Class Comparison (bar chart comparing position strength across up to 6 draft years using median POT of top 5 prospects)
- **Moves & Contracts** — Cap space overview (real salary cap), bad contracts, fill the gap (FA suggestions by need), extension watch, re-sign watch, sell high, trade candidates, value over replacement (VOR) grouped by position. Direct "Trade" button on every player to jump to Shop Player
- **Trades** — Three sub-tabs:
  - *Trade Checker* — Build trades manually with salary cap enforcement and confidence levels (Sure/Likely/Maybe/Risky). Full ValueChangeCalculator formula
  - *Shop Player* — Select one or more players/picks to shop. Finds 1-for-1 up to 5-for-1 offers from all 31 teams with position and team filters, cap warnings, and sortable results
  - *Find Player* — Search any player in the league, see what packages from your roster the AI would accept. Sorted by cheapest deal first
- **Aging Projections** — 3-season OVR forecasts per player and position group, biggest risers & drops. Supports ProgCode 3.5 custom progression curves
- **Compare** — Team vs. team (position-by-position with weighted OVR) and player vs. player (all 21 ratings side by side, key ratings highlighted)
- **Player Detail** — Click any player name anywhere for all 21 raw ratings, OVR at every position, position switch recommendations, individual aging timeline

## How to Use

1. In Football GM → **Tools** → **Export League** → select **Players**, **All team data**, **Draft picks**, and **League settings** (all four required!) → Download
2. Open the Advisor → drag & drop your JSON or gzip file
3. Select your team from the Power Rankings → explore

Alternatively, use the **Quick Export Bookmarklet** ([bookmarklet.html](https://ulithebison.github.io/fbgm-advisor/bookmarklet.html)) for one-click exports directly from IndexedDB. Supports both direct transfer (opens Advisor automatically) and JSON download. Retired players are automatically filtered out for faster loading.

## Trade Engine

Full port of the game's `ValueChangeCalculator` (~830 lines), including:

| Factor | Status |
|--------|--------|
| Z-Score normalization (true OVR mean/std) | ✅ |
| True player value (`p.value`, not fuzz) | ✅ |
| EXPONENT = 3 (football) | ✅ |
| Contract value (salary vs. expected, capped at 0.1) | ✅ |
| Strategy multipliers (rebuilding/contending, all age brackets) | ✅ |
| fudgeFactor (1.0 for user trades, 1.05 for AI-vs-AI) | ✅ |
| Difficulty-based fudge scaling | ✅ |
| Injury discount (user team exempt) | ✅ |
| justDrafted floor | ✅ |
| Pick value floor (Math.max(0.1, zScore)) | ✅ |
| Rookie salary contract value | ✅ |
| First round pick protection (SPORT_FACTOR = 2.5) | ✅ |
| Estimated pick positions (Team OVR → Win%) | ✅ |
| Dynamic pick revaluation (getModifiedPickRank) | ✅ |
| Pick regression + user pick penalty | ✅ |
| Salary cap enforcement (hard cap block, both teams) | ✅ |
| Phase-aware contract and regression handling | ✅ |
| Correct order of operations (strategy → injury → negative → contract → justDrafted → exponent) | ✅ |

## Draft Intelligence

- **Fuzz Reveal** — OVR/POT +/- columns show the gap between true ratings and scouted (fuzzed) ratings. Green = undervalued, Red = overvalued
- **Sleeper Alert** — Current year prospects ranked by how much the AI undervalues them
- **Draft Class Comparison** — Bar chart comparing position strength across up to 6 years, using median POT of top 5 prospects per position. Identifies the best year to draft each position

## Roster Health

Per-position analysis across 4 dimensions:

| Dimension | What it measures |
|-----------|-----------------|
| Starter Quality | Your starters vs league average |
| Depth | Roster count vs ideal depth |
| Future | 3-year OVR projection trend |
| Rising | Players with 10+ points of POT upside |

Actions: **NEED** (below average), **DEPTH** (thin backup), **AGING** (declining starters), **SURPLUS** (trade assets available), **OK** (healthy)

## Source Code Analysis

Formulas extracted from `zengm-games/zengm`:

| File | What it does |
|------|-------------|
| `player/ovr.football.ts` | Position-specific OVR from 21 raw ratings |
| `team/ovr.football.ts` | Team OVR regression weights (OL ~37%, DL ~33%, QB ~13%) + scale formula |
| `team/ValueChangeCalculator.ts` | Full trade AI with value normalization, strategy, and pick valuation |
| `trade/getPickValues.ts` | Draft pick value estimation from prospect pool |
| `draft/getRookieSalaries.ts` | Rookie contract salary scale |
| `player/developSeason.football.ts` | Aging curves per position |
| `draft/runPicks.ts` | Draft AI: `(teamOvrDiff + 0.05 × valueFuzz) ^ 40` |
| `player/fuzzRating.ts` | Scouting fuzz: `round(bound(rating + fuzz, 0, 100))` |
| `views/powerRankings.ts` | Power rankings score formula |
| `trade/makeItWork.ts` | Trade negotiation AI (forward selection algorithm) |

## Tech Stack

Single HTML file. React 18 + Babel (in-browser JSX). No build step, no dependencies, no server. Supports JSON and gzip compressed exports.

## License

MIT
