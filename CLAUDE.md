# CLAUDE.md — CIS 5450 Final Project

## Project Overview

**Title:** Predicting NBA Game Outcomes Using Historical Team Performance Data  
**Course:** CIS 5450 (Big Data Analytics), University of Pennsylvania  
**Team:** CIS 5450 project team (4 members)

**Goal:** Binary classification — predict whether a team wins (1) or loses (0) a given NBA game using pre-game historical statistics. No in-game or post-game data may be used as features (no leakage).

**Target variable:** `WL` (Win/Loss)

---

## Repository Structure

```
cis5450-project/
├── CLAUDE.md                    # This file — session context for Claude Code
├── README.md                    # Human-facing project overview
├── requirements.txt             # Python dependencies
├── .env.example                 # Template for environment variables (no keys needed currently)
├── .gitignore                   # Ignores .env, __pycache__, .ipynb_checkpoints, etc.
├── data/
│   └── raw/                         # Raw downloaded data (committed to repo — all data is public/legal)
│       ├── league_game_finder.csv   # All game-level results (leaguegamefinder endpoint)
│       ├── game_logs/               # Per-season team game logs (teamgamelogs endpoint)
│       │   └── game_logs_YYYY-YY.csv
│       └── team_stats/              # Season-level advanced stats (leaguedashteamstats endpoint)
│           └── team_stats_YYYY-YY.csv
├── 01_data_acquisition.ipynb    # Step 1: Download & save raw data
├── 02_eda.ipynb                 # Step 2: EDA & hypothesis testing
├── 03_cleaning_features.ipynb   # Step 3: Cleaning & feature engineering
└── 04_modeling.ipynb            # Step 4: Modeling & evaluation
```

---

## Data Sources

### Primary: `nba_api` Python Package
- **Package:** `nba_api` (pip install nba_api)
- **Source repo:** https://github.com/swar/nba_api
- **No API key required** — this package makes HTTP requests to stats.nba.com (public, free, legal)
- **Rate limiting:** NBA's servers throttle fast requests. The notebook uses `time.sleep(1)` between calls. Do not remove these delays or you will get 429 errors / IP blocks.

#### Endpoints Used

| Endpoint | Class | What it gives us | Saved as |
|---|---|---|---|
| `leaguegamefinder` | `LeagueGameFinder` | Game-level results (all teams, all seasons) | `data/raw/league_game_finder.csv` |
| `teamgamelogs` | `TeamGameLogs` | Per-game box scores per team per season | `data/raw/game_logs_YYYY-YY.csv` |
| `leaguedashteamstats` | `LeagueDashTeamStats` | Season-level & advanced team metrics | `data/raw/team_stats_YYYY-YY.csv` |

#### Seasons Covered
Seasons 2000-01 through 2024-25 (25 seasons), yielding ~65,000+ team-game rows before enrichment.

Season ID format used by NBA API: `"2000-01"`, `"2001-02"`, ..., `"2024-25"`

### Supplementary (Optional / Future)
- Historical Vegas closing spreads & over/under lines from covers.com or similar public archives
- No acquisition code written yet for this source

---

## Current Status

| Step | Status |
|---|---|
| Data acquisition | ✅ Notebook created (`01_data_acquisition.ipynb`) |
| EDA & hypothesis testing | ⬜ Not started |
| Cleaning & feature engineering | ⬜ Not started |
| Modeling & evaluation | ⬜ Not started |

---

## Data Storage Decision

**Raw data IS committed to the repo** (not gitignored). Rationale:
- All data is sourced from a free, public, legal API (stats.nba.com via nba_api)
- Total raw data size is ~30–80 MB (CSV files), well within GitHub's 1 GB soft limit
- Committing data lets all collaborators work immediately without re-running the slow API download
- If the repo ever hits size limits, migrate to Git LFS or store data externally (e.g., Google Drive) and update this note

`.env` files are gitignored (no keys needed right now, but placeholder in place for future credentials).

---

## Key Modeling Decisions (Recorded Here to Avoid Future Debate)

1. **No data leakage:** All features must be computable before tip-off. Box scores from the game being predicted are forbidden as features.
2. **Time-aware CV:** Train on earlier seasons, test on later ones. No standard k-fold (would leak future into past).
3. **Row definition:** One row = one team's performance in one game. Both teams in a game each get a row.
4. **Evaluation metrics:** Accuracy, AUC-ROC, precision, recall, F1. Baseline = home-team-always-wins (~60%).

---

## How to Resume in a New Claude Session

1. Read `CLAUDE.md` (this file) first
2. Check which notebooks exist and their completion status above
3. Confirm `data/raw/` contains CSV files (if not, run `01_data_acquisition.ipynb`)
4. The next step is `02_eda.ipynb` — hypothesis tests to implement are listed in the notebook

---

## Environment Setup

```bash
pip install -r requirements.txt
```

No `.env` keys needed for data acquisition. Copy `.env.example` to `.env` if adding future credentials.
