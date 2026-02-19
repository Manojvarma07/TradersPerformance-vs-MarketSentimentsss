# 📊 Crypto Trader Behavior Analysis
### Analyzing the impact of Fear & Greed sentiment on crypto trading performance

---

## 📁 Repository Structure

```
crypto-trader-analysis/
│
├── 📂 programs/
│   ├── part_a_fixed.py          # Data preparation & cleaning
│   ├── part_b_fixed.py          # Analysis & clustering
│   └── part_c_fixed_v2.py       # Strategy simulation & ML model
│
├── 📂 datasets/
│   ├── fear_greed_index.csv     # Daily Fear & Greed sentiment data
│   └── historical_data.csv      # Historical trades data
│
├── 📂 outputs/
│   ├── 📊 Charts (.png)         # All visualizations
│   └── 📋 Tables (.csv)         # All result tables
│
└── README.md
```

---

## ⚙️ Setup

Install required libraries:

```bash
pip install pandas numpy matplotlib scikit-learn scipy
```

---

## ▶️ How to Run

> ⚠️ **Important:** Always run in order — Part A → Part B → Part C.
> Each part depends on the output of the previous one.

### In Jupyter Notebook

| Step | Action |
|------|--------|
| 1 | Open Jupyter Notebook |
| 2 | Create a new notebook |
| 3 | Copy code from `part_a_fixed.py` → paste into Cell 1 → press **Shift + Enter** |
| 4 | Copy code from `part_b_fixed.py` → paste into Cell 2 → press **Shift + Enter** |
| 5 | Copy code from `part_c_fixed_v2.py` → paste into Cell 3 → press **Shift + Enter** |
| 6 | Open `outputs/` folder to view all results |

### In Terminal

```bash
python programs/part_a_fixed.py
python programs/part_b_fixed.py
python programs/part_c_fixed_v2.py
```

---

## 🔬 Methodology

### Part A — Data Preparation
- Loaded and cleaned trades and sentiment datasets
- Converted timestamps and aligned both datasets by date
- Computed daily metrics per account:
  - Daily PnL, Win Rate, Leverage, Trade Frequency, Long/Short Ratio

### Part B — Analysis
- Compared trader performance on **Fear vs Greed** days
- Used **Mann-Whitney statistical test** to confirm significance
- Applied **K-Means Clustering (k=3)** to segment traders by behavior

### Part C — Strategy & Model
- Simulated 2 strategy rules on historical data
- Measured impact on total PnL and daily volatility
- Trained a **Random Forest classifier** to predict next-day profitability

---

## 💡 Key Insights

#### Insight 1 — Fear days hurt trader performance
PnL and win rate drop significantly on Fear days. The Mann-Whitney
test confirmed this is statistically significant and not random.
> 📊 See: `boxplot_daily_pnl_by_sentiment.png`

#### Insight 2 — High leverage traders drive platform risk
Cluster 1 (avg leverage ~28,664) causes the most volatility.
Capping their leverage on Fear days reduced daily volatility
from **2,581,231 → 2,311,222** (reduction of ~270,000).
> 📊 See: `simulation_cumpnl_partc_final.png`

#### Insight 3 — Consistent winners thrive on Greed days
Cluster 0 traders (win rate 0.58) consistently outperform on
Greed days. Giving them 20% more size on Greed days generated
positive returns with minimal added risk.
> 📊 See: `cluster_scatter.png`

---

## 🎯 Strategy Recommendations

### Rule 1 — Reduce leverage on Fear days
```
IF sentiment == Fear
AND trader in Cluster 1 (high leverage)
THEN cap leverage at 3x
```
**Result:** Platform volatility reduced by ~270,000

---

### Rule 2 — Increase size on Greed days
```
IF sentiment == Greed
AND trader in Cluster 0 (consistent winners)
AND historical win rate > 0.5
THEN increase position size by 20%
```
**Result:** Positive PnL delta for top performing accounts

---

## 🤖 Predictive Model Results

| Metric    | Score       |
|-----------|-------------|
| Accuracy  | **0.76**    |
| F1 Score  | **0.81**    |
| AUC       | **0.83**    |
| CV AUC    | 0.72 ± 0.11 |

**Top Predictive Features:**

| Feature | Importance |
|---------|------------|
| 7-day rolling leverage | 0.39 |
| 7-day rolling trade frequency | 0.22 |
| 7-day rolling position size | 0.18 |
| Market sentiment (Fear/Greed) | 0.11 |
| 7-day rolling win rate | 0.10 |

> Leverage is the strongest predictor of next-day profitability —
> confirming that **managing leverage is the most impactful intervention**.

---

## 📂 Output Files Reference

| File | Description |
|------|-------------|
| `boxplot_daily_pnl_by_sentiment.png` | PnL comparison: Fear vs Greed |
| `boxplot_win_rate_by_sentiment.png` | Win rate comparison: Fear vs Greed |
| `cluster_scatter.png` | 3 trader segments visualized |
| `simulation_cumpnl_partc_final.png` | Original vs adjusted strategy PnL |
| `model_feature_importances_partc_final.png` | Model feature importances |
| `sentiment_metric_tests.csv` | Full statistical test results |
| `agg_behav_by_sentiment.csv` | Behavior changes by sentiment |
| `account_segments.csv` | Each account's cluster label |
| `strategy_simulation_summary_partc_final.csv` | Simulation summary |
| `model_report_partc_final.txt` | Full model evaluation report |
