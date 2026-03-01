# March-Machine-Learning-Mania-2026
### Kaggle Competition — NCAA Basketball Tournament Prediction

---

## Competition Overview

- **Goal:** Predict the outcome of every possible matchup in the 2026 NCAA Men's and Women's Basketball Tournaments
- **Metric:** Brier Score (lower is better)
- **Submissions:** 519,144 rows covering all possible team matchups for seasons 2022–2026
- **Prize:** $10,000 first place
- **Deadline:** March 19, 2026

---

## Current Standing

| Metric | Value |
|---|---|
| Best Public Score | 0.10570 |
| Current Rank | 513 |
| Best Submission | Ensemble XGB + LGB + LR |

---

## What We Built

### Data Used
- Men's regular season detailed results (122,775 games, 34 stats per game)
- Women's regular season detailed results (85,505 games)
- NCAA Tournament results — Men's (2,585 games) and Women's (1,717 games)
- Tournament seeds — Men's and Women's
- Massey Ordinals — 5.7 million rows of rankings from dozens of rating systems
- Sample Submission Stage 1 — 519,144 matchups to predict

---

### Features Engineered

**Elo Ratings (Margin of Victory variant)**
- Season-by-season Elo computed from regular season games
- K-factor adjusted by margin of victory using log scale
- 50% carry-over of prior season rating to next season
- Captures team strength trajectory over time

**Season Statistics (from Detailed Box Scores)**
- Win rate, average points scored and allowed, point differential
- Field goal %, 3-point %, free throw %, true shooting %
- Rebounds, assists, turnovers, steals, blocks per game
- Computed separately for Men's and Women's

**Seeds**
- Tournament seed number parsed from seed string
- Seed difference between matchup teams

**Massey Ordinals Consensus Ranking**
- Mean rank and minimum rank across all rating systems
- Filtered to rankings within 133 days of tournament
- Consensus view across dozens of expert ranking systems

**Recent Form (Last 30 Days)**
- Win rate and point differential in final 30 days of regular season
- Captures hot and cold streaks heading into tournament

**Tempo and Offensive/Defensive Efficiency**
- Possessions per game (tempo)
- Offensive efficiency: points scored per 100 possessions
- Defensive efficiency: points allowed per 100 possessions
- Net efficiency: offensive minus defensive
- True shooting %, assist-to-turnover ratio, offensive rebound rate

**Strength of Schedule**
- Average opponent win rate faced during regular season
- Average opponent point differential faced

**Difference Features**
- All key features computed as Team1 minus Team2 differential
- EloDiff, SeedDiff, MeanRankDiff, NetEffDiff, WinRateDiff, SOSDiff, etc.

**Final Feature Count: 47 features**

---

### Models Used

**XGBoost Classifier**
- 600 estimators, learning rate 0.02, max depth 4
- Subsample 0.8, column sample 0.8
- Early stopping on validation log loss

**LightGBM Classifier**
- 600 estimators, learning rate 0.02, max depth 4
- Early stopping with 50 rounds patience

**Logistic Regression**
- C = 0.1, StandardScaler normalization
- Serves as a linear baseline in the ensemble

**Ensemble Method**
- Equal weighted average of all three models (1/3 each)
- 5-fold Stratified Cross Validation for OOF evaluation
- Predictions clipped to range [0.025, 0.975]

---

## Key Learnings

1. **More features did not help** — going from 43 to 67 features barely moved OOF score. With only 2,585 tournament game samples, overfitting is the main risk.

2. **Isotonic calibration overfit** — improved OOF Brier but hurt public score. Platt scaling showed our model is already well-calibrated internally.

3. **Regular season data hurt tournament prediction** — the two distributions are fundamentally different. Regular season has home court advantage and scheduling noise. Tournament is neutral site, high stakes, different dynamics entirely.

4. **OOF Brier and public leaderboard are not perfectly correlated** — the leaderboard scores only on actual tournament game matchups (~3.5% of all submissions). Trust the leaderboard, not OOF alone.

5. **SeedDiff and EloDiff are the dominant features** — they account for the majority of feature importance. Every other feature adds marginal signal on top of these two.

---

## Tech Stack

- Python 3
- Pandas, NumPy
- XGBoost, LightGBM, Scikit-learn
- Kaggle Notebooks (CPU)

---

## Repository Structure

```
march-mania-2026/
│
├── march-mania-2026-winner.ipynb   # Main notebook with all cells
├── submission.csv                   # Current best submission
└── README.md                        # This file
```
