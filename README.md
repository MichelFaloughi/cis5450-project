# Predicting NBA Game Outcomes

**CIS 5450 Final Project — University of Pennsylvania**

## Goal

Binary classification: predict whether a team **wins (1) or loses (0)** a given NBA game using only pre-game historical statistics. No in-game or post-game data is used as features.

## Data

- **Source:** [nba_api](https://github.com/swar/nba_api) — free, public access to the official NBA statistics database. No API key required.
- **Coverage:** 25 NBA seasons, 2000-01 through 2024-25
- **Raw data** is committed to this repo under `data/raw/`. All data is publicly available and legal to redistribute.

### Datasets

| Dataset | Location | Granularity | Description |
|---|---|---|---|
| Game logs | `data/raw/game_logs/` | One row per team per game | Per-game box scores: points, rebounds, assists, turnovers, shooting splits, plus/minus, and the win/loss outcome (`WL`) — the target variable |
| Team stats | `data/raw/team_stats/` | One row per team per season | Season-level advanced metrics: offensive rating, defensive rating, net rating, pace, and true shooting % |
| Game index | `data/raw/league_game_finder.csv` | One row per team per game | Flat index of all game results: game ID, date, matchup, and outcome — used for joining and cross-referencing |

## Setup

```bash
pip install -r requirements.txt
```

## Notebooks

| Notebook | Description |
|---|---|
| `01_data_acquisition.ipynb` | Download raw data from NBA API and save to `data/raw/` |
| `02_eda.ipynb` | Exploratory data analysis & hypothesis testing |
| `03_cleaning_features.ipynb` | Data cleaning, wrangling & feature engineering |
| `04_modeling.ipynb` | Modeling, evaluation & hyperparameter tuning |

## Data Acquisition

Open and run `01_data_acquisition.ipynb` top-to-bottom. The notebook downloads all raw data and saves it to `data/raw/` as CSV files.

> If `data/raw/` already contains CSV files (pulled from git), you can skip straight to notebook 02.

**Note:** The first run takes 10–25 minutes due to rate-limit delays between API requests. Do not remove the sleep calls.
