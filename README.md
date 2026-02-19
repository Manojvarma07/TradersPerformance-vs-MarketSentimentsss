
📁 programs/
    ├── part_a_      ← Data preparation
    ├── part_b_      ← Analysis
    └── part_c_      ← Strategy + Model  
📁 outputs/
    ├── charts (.png)
    └── tables (.csv)
📁 datasets/
    ├── fear_greed_index.csv
    └── historical_data.csv
└── README.md

ANALYSIS RESULTS

Setup & Requirements
bashpip install pandas numpy matplotlib scikit-learn scipy

How to Run
Option 1 — Jupyter Notebook

Open Jupyter Notebook
Create a new notebook
Copy code from each program file into separate cells
Run in order: Part A → Part B → Part C
Check outputs/ folder for all results

Option 2 — Terminal
bashpython programs/part_a_fixed.py
python programs/part_b_fixed.py
python programs/part_c_fixed_v2.py

⚠️ Always run in order. Each part depends on the previous one.
⚠️ Data files must be inside a folder called data science/


Methodology
Part A — Data Preparation
Loaded and cleaned two datasets (trades + sentiment index). Converted
timestamps and aligned both by date. Created daily metrics per account
including PnL, win rate, leverage distribution, trade frequency and
long/short ratio.
Part B — Analysis
Compared trader performance and behavior across Fear vs Greed days
using Mann-Whitney statistical tests. Applied K-Means clustering
(k=3) on leverage, win rate and trade frequency to identify 3 distinct
trader segments.
Part C — Strategy Simulation + Predictive Model
Simulated 2 strategy rules on historical data and measured impact on
PnL and volatility. Built a Random Forest classifier to predict
next-day profitability using rolling features and sentiment.

Insights
Insight 1 — Fear days hurt performance
PnL and win rate drop significantly on Fear days compared to Greed
days. Platform volatility peaks during Fear periods confirming
sentiment directly impacts trading outcomes.
Insight 2 — High leverage traders drive platform risk
Cluster 1 traders (avg leverage ~28,664, ~12,000 trades/day) cause
most of the platform volatility. Capping their leverage on Fear days
reduced daily volatility from 2,581,231 to 2,311,222 — a meaningful
risk reduction of ~270,000.
Insight 3 — Consistent winners are reliable on Greed days
Cluster 0 traders (win rate 0.58) consistently outperform on Greed
days. Increasing their position size by 20% on Greed days generated
positive returns with minimal added risk.

Strategy Recommendations
Rule 1 — Cap leverage on Fear days

During Fear days, cap leverage at 3x for high-leverage traders
(Cluster 1)

Reduces platform volatility and protects traders from large drawdowns.
Rule 2 — Increase size on Greed days

During Greed days, increase position size by 20% for consistent
winners (Cluster 0) with win rate > 0.5

Rewards disciplined traders and improves platform-level returns.

Predictive Model Results
MetricScoreAccuracy0.76F1 Score0.81AUC0.83CV AUC0.72 ± 0.11
Top feature: 7-day rolling leverage (importance: 0.39) —
confirming leverage management is the most critical factor.

Output Files
FileDescriptionboxplot_daily_pnl_by_sentiment.pngPnL: Fear vs Greedboxplot_win_rate_by_sentiment.pngWin rate: Fear vs Greedcluster_scatter.png3 trader segmentssimulation_cumpnl_partc_final.pngStrategy impact on PnLmodel_feature_importances_partc_final.pngModel featuressentiment_metric_tests.csvStatistical test resultsaccount_segments.csvTrader cluster labelsstrategy_simulation_summary_partc_final.csvSimulation summaryShare
