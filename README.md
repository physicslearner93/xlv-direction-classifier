# Healthcare ETF Direction Classifier: Can Machine Learning Beat "Always Up"?

Can a random forest predict whether the SPDR Healthcare Sector ETF (XLV) will be higher in 5 trading days, using price, volume, VIX and "recession" search interest?

## Key findings

- The model did not show a real edge. On the test set (March 2020 to March 2024, 1,014 days), the first model scored 55.4% accuracy, the same as the always-predict-up baseline (55.4%). It predicted "up" on almost every day (recall 0.99 on up days, 3 true negatives).
- After dropping 7 low-importance features, test accuracy was 56.2%, about 8 more correct days than the baseline. The notebook says this gap is within sampling noise and is not evidence of an edge.
- Overfitting showed up clearly in cross-validation. At max depth 10, training accuracy was 0.92 against 0.47 in cross-validation. The best grid-search setting was shallow (depth 2), with cross-validation accuracy of 0.494.
- Test conditions differed from training: higher "recession" search interest (median 15 vs 8) and higher VIX (median 21.2 vs 15.6). The notebook reads this as distribution shift, not learned skill.
- The most important feature was the Google Trends "recession" series, but the notebook flags look-ahead bias. Monthly values were interpolated to daily, so a mid-month day already reflects the month-end value.

## What I did

- Loaded XLV prices and volume (5,094 days, 2004 to 2024), VIX data and monthly Google Trends data.
- Built features: cyclical month, weekday, 1/5/10/20-day returns, log volume, internal bar strength, Bollinger Band and RSI indicators, VIX and the search trend.
- Set the target as a positive 5-day forward return (56.6% of days). Removed collinear features and dropped rows that could not have known future data.
- Split by time (first 80% train and validation, last 20% test), set the baseline, and read learning curves.
- Tuned a random forest with grid search, evaluated precision, recall and confusion matrices, then retrained on the reduced feature set.
- Wrote next steps: walk-forward cross-validation with purging, fixing the Trends look-ahead, a cost-aware backtest against buy-and-hold, and forecasting volatility or drawdown risk instead of direction.

## Tools

Python, pandas, scikit-learn, ta (technical indicators), matplotlib, seaborn, Plotly.

## Files

| File | What it is |
|---|---|
| `project_starter.ipynb` | Completed notebook with outputs, charts and written answers |
| `project_starter.html` | HTML export of the notebook |
| `xlv_data.csv` | XLV daily prices and volume, provided by Udacity |
| `vix_data.csv` | VIX daily data, provided by Udacity |
| `GoogleTrendsData.csv` | Monthly "recession" search interest, provided by Udacity |
| `NOTICE.md` | Terms of use |

## How to view

Open the notebook on GitHub, or in Jupyter. All outputs are saved, so you do not need to rerun it.

## About this project

Completed for the Udacity AI Trading Strategies Nanodegree (passed project review, November 2026). Starter materials and project specifications belong to Udacity. The analysis and write-up are my own. See NOTICE.md. If you are a current Udacity student, do not copy this work; submitting it is a violation of the Udacity Honor Code.
