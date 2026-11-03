# Asset forecasting notebooks

This project contains 25 forecasting notebooks: ARIMA, SVR, XGBoost, LSTM and Transformer for BTC, S&P 500, USD/EUR, WTI and gold (XAU).

## Browse the results

Open a notebook on GitHub to view its code and saved tables and plots. GitHub displays saved results; it does not execute notebook cells.

| Asset | Notebooks | Source data |
| --- | --- | --- |
| BTC | [BTC_TEST](BTC_TEST/) | [Bitcoin Historical Data.csv](CSV/Bitcoin%20Historical%20Data.csv) |
| S&P 500 | [S&P_TEST](S%26P_TEST/) | [S&P 500 Historical Data.csv](CSV/S%26P%20500%20Historical%20Data.csv) |
| USD/EUR | [USD_TEST](USD_TEST/) | [USD_EUR_Historical.csv](CSV/USD_EUR_Historical.csv) |
| WTI | [WTI_TEST](WTI_TEST/) | [WTI_USD Historical Data.csv](CSV/WTI_USD%20Historical%20Data.csv) |
| XAU | [XAU_TEST](XAU_TEST/) | [XAU_USD Historical Data.csv](CSV/XAU_USD%20Historical%20Data.csv) |

All five XGBoost notebooks contain **Step 10.1 - Lag ranking by Train Gain**, with separate Top 10 tables for horizons 1, 3, 5, 7, 14, 21 and 30. Horizons follow each notebook's declared calendar. The rankings describe Train feature importance; they do not establish an optimal lookback length.

## Find Future and Test RMSE/MAPE

All 25 notebooks include **Results navigation** near the top. Use **Task 1** for realized future forecasts and **Task 2** for the original historical Test set. Saved tables can be viewed without rerunning training.

| Model | Task 1: BTC / S&P | Task 1: USD / WTI / XAU | Task 2: BTC / S&P | Task 2: USD / WTI / XAU |
| --- | --- | --- | --- | --- |
| ARIMA | Section 7.5 | Section 7.5 | Step 8 | Step 8 |
| SVR / LSTM / Transformer | Section 9.5 | Section 11.3 | Step 10 | Step 12 |
| XGBoost | Section 8.4.1 | Section 11.2.1 | Step 9 | Step 12 |

**Task 1 - Future forecast RMSE/MAPE:** original forecasts from 9 September 2026 are compared with the supplied prices in [ACTUAL_FOR_PREDICT](ACTUAL_FOR_PREDICT/). The tables show dated forecast/actual pairs, metrics by horizon and metrics across each 7/30-step path. The original target dates and forecasts are preserved; actual prices are used only for evaluation, without fitting models. There is one pair per horizon, so per-horizon RMSE equals absolute error. Whole-path scores combine different horizons from that single origin.

WTI has no supplied actual price for 11 or 12 October 2026: its 30-step path is evaluated on **28/30 matched pairs**, explicitly marked as partial. Other assets have 30/30 matched pairs. Original archived S&P Future forecasts use saved notebook precision. Current SVR/XGBoost reruns use full-precision forecasts; each notebook records its source and precision.

**Task 2 - Test set RMSE/MAPE:** original chronological Test 20% only, with separate scores and sample counts for every horizon. The new future-actual files are excluded. LSTM and Transformer report their original predefined-seed ensemble forecasts; seed metric averages are separate.

Rerunning the reporting cells exports separate tables to `OUTPUT/METRICS_REVIEW/Task1_Future/<notebook-name>/` and `OUTPUT/METRICS_REVIEW/Task2_Test/<notebook-name>/`. Generated CSV exports remain local; saved tables are embedded in each shared notebook.

## SVR and XGBoost: Top 5 before Test

The current SVR and XGBoost notebooks show five distinct parameter combinations per requested horizon (1, 3, 5, 7, 14, 21 and 30), ranked by **Train RMSE in original price units**. Train MAPE and the parameter values are displayed alongside the ranking. Candidate ID identifies a predefined parameter combination; it is not the rank.

- **SVR:** 27 combinations of C, epsilon and gamma multipliers (0.5/1/2 around Train-derived reference values); rank 1 is selected per horizon.
- **XGBoost:** 18 combinations of n_estimators (50/100/200), learning_rate (0.1/0.3) and max_depth (3/6/8). Other parameters remain fixed. The selected row is explicitly labelled **Best on Train**, and a separate best-model table is shown.

| Model and assets | Top 5 section | Top 5 cell | Revised Future cell | Current Test cell |
| --- | --- | --- | --- | --- |
| SVR: all five assets | 7.1 | 20 | 36 | 40 |
| XGBoost: BTC / S&P | 6.1 | 18 | 36 | 42 |
| XGBoost: USD / WTI / XAU | 7.1 | 18 | 40 | 46 |

Cell positions count every cell, including Markdown. Use the results navigation near the top of each notebook for direct links.

The selected rank-1 parameters are frozen before Test evaluation. The split remains chronological Train/Test 80/20, with no validation or cross-validation. Train-fit ranking does not establish the best performance on new data. Test and future-actual dates were inspected in earlier experiments, so these reruns are retrospective evaluations.

Original archived Future scores remain separate from the revised SVR/XGBoost Future scores. Quantile prediction-interval models in XGBoost retain their separate original fixed Train-only specification. Search tables and selected-parameter exports are regenerated locally under `OUTPUT/SVR_TRAIN_SEARCH/` and `OUTPUT/XGBOOST_TRAIN_SEARCH/`; they are also embedded in the saved notebook outputs.

## Run locally

1. Clone or download the repository, keeping the folder structure and CSV files together.
2. Open JupyterLab from the project root and select a Python environment with the notebook's dependencies. Check the imports and displayed library versions in each notebook; some models require different libraries.
3. Review the configuration and data path, then run cells from top to bottom. Running model cells trains models and can take time.

The existing notebooks record local Python kernel names. Other machines may need to select their own installed kernel. Generated exports under `OUTPUT/`, temporary copies and checkpoints are excluded from uploads; notebook outputs remain embedded in the `.ipynb` files.
