# 🏀 March Machine Learning Mania 2026 — NCAA Tournament Bracket Prediction

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python&logoColor=white)
![Kaggle](https://img.shields.io/badge/Kaggle-Competition-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-Ensemble-red?style=for-the-badge)
![LightGBM](https://img.shields.io/badge/LightGBM-Ensemble-brightgreen?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

## 📌 Project Overview

This repository contains a complete machine learning solution for **March Machine Learning Mania 2026** — Kaggle's annual NCAA Men's Basketball Tournament prediction competition. The goal is to predict the win probability for every possible matchup in the NCAA March Madness bracket.

The solution combines **Elo ratings, seed differentials, Massey ordinal rankings, seasonal statistics, and recent form** into a powerful ensemble of XGBoost, LightGBM, and Logistic Regression models.

---

## 🏆 Competition Context

> Each year, 68 college basketball teams compete in the NCAA Tournament. Predicting upsets and Cinderella stories requires capturing both season-long performance and late-season momentum. Models are evaluated using **Brier Score** (lower is better) — a proper scoring rule for probability predictions.

- **Task:** Predict win probability for all 2278 possible Men's tournament matchups
- **Evaluation Metric:** Brier Score Loss (lower = better)
- **Platform:** Kaggle
- **Season:** 2025–2026 NCAA Men's Basketball

---

## 🎯 Feature Engineering

A rich set of features was engineered from historical game results:

### Team Statistics (Season-Long)
| Feature | Description |
|---------|-------------|
| `WinRate` | Season win percentage |
| `AvgPtsFor` | Average points scored |
| `AvgPtsAgn` | Average points allowed |
| `AvgPtsDiff` | Average point differential |
| `FGPct / FG3Pct / FTPct` | Field goal & free throw percentages |
| `AvgReb / AvgAst / AvgTO / AvgStl / AvgBlk` | Advanced box score stats |

### Strength & Ranking Features
| Feature | Description |
|---------|-------------|
| `EloDiff` | Elo rating differential between teams |
| `SeedDiff` | Tournament seed differential |
| `MeanRankDiff` | Massey ordinal ranking differential |

### Momentum Features
| Feature | Description |
|---------|-------------|
| `RecentWinRate` | Win rate over last 30 days |
| `RecentPtsDiff` | Point differential over last 30 days |

---

## 🤖 Model Architecture

A **5-fold cross-validated ensemble** of three model types:

```
XGBoost (5 folds)
    +
LightGBM (5 folds)
    +
Logistic Regression (5 folds)
    ↓
Weighted Average → Win Probability
```

### Evaluation (Cross-Validation)
- **Brier Score Loss** — measures calibration quality
- **Log Loss** — measures probabilistic accuracy
- All models validated on held-out fold before ensembling

---

## 📊 Technical Highlights

### Elo Rating System
```python
def compute_elo(results_df, k=20, base=1500):
    # Per-season Elo with 50% carry-over from prior season
    # Updates after each game: winner gains, loser loses based on expected probability
```

### Massey Ordinal Rankings
- Used rankings from the final week before tournament (cutoff day 133)
- Averaged across all rating systems to create a composite rank

### Recent Form Window
- Last 30 days of games used to capture late-season momentum
- Separate `RecentWinRate` and `RecentPtsDiff` features per team

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3.10+ | Core language |
| Pandas / NumPy | Data processing |
| XGBoost | Gradient boosting |
| LightGBM | Gradient boosting |
| Scikit-learn | Logistic Regression, cross-validation, metrics |
| Kaggle Environment | Competition data access |

---

## 📁 Project Structure

```
March-Machine-Learning-Mania-2026/
│
├── march-mania-2026.ipynb    # Complete solution notebook
├── README.md
└── LICENSE
```

---

## 🚀 How to Run

This notebook is designed to run on **Kaggle** with the competition dataset attached.

1. Open on Kaggle and attach the `march-machine-learning-mania-2026` dataset
2. Run all cells sequentially
3. Download the generated `submission.csv`

---

## 🔗 Competition Link

[![Kaggle](https://img.shields.io/badge/View%20on%20Kaggle-March%20Mania%202026-20BEFF?style=for-the-badge&logo=kaggle)](https://www.kaggle.com/competitions/march-machine-learning-mania-2026)

---

## 👨‍💻 Author

**Homeswar Rao Nelakurthi**
[![GitHub](https://img.shields.io/badge/GitHub-homeshwarnelakurthi-181717?style=flat&logo=github)](https://github.com/homeshwarnelakurthi)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
