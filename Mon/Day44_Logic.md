# Week 08 · Monday (Day 44) – Time Series & Sensor Forecasting Logic

## Overview

The assignment is split by difficulty:
- Easy (Sub-steps 1–2): understand and clean ecommerce sales and sensor datasets.
- Medium (Sub-steps 3–5): build baseline and extended sales models and a sensor failure-risk model.
- Hard (Sub-steps 6–7): compare a simple sensor rule to the model and pick a cost-optimal threshold.

Each level has its own notebook so the workflow stays modular.

---

## Easy notebook – sales and sensor cleaning

### Sales series
- Load `ecommerce_sales_ts.csv-2.csv` and enforce `date` and `sales` columns.
- Sort by date, set `date` as index, and inspect the date range, index frequency, missing values, and duplicate timestamps.
- Plot the full series and a 30-day rolling mean to see trend and seasonality.
- Run an Augmented Dickey–Fuller test to check basic stationarity.
- Reindex to a full daily calendar and interpolate sales; drop duplicate dates.
- Save `ecommerce_sales_ts_clean.csv` for use in Medium sub-steps.

### Sensor data
- Load `sensor-3.csv` with `timestamp` parsed to datetime.
- Profile dtypes, missing values, duplicates, and common time deltas.
- Describe numeric sensor columns and flag extreme z-score outliers.
- Drop duplicate timestamps, reindex to a regular time grid using the modal delta.
- Interpolate missing numeric values; mask `|z|>4` spikes and interpolate again.
- Save `sensor_data_clean.csv` as the base for the sensor models.

---

## Medium notebook – models and evaluation

### Sub-step 3 – Baseline sales model
- Load `ecommerce_sales_ts_clean.csv`.
- Use the last 30 days as a time-ordered hold-out set.
- Fit SARIMA(1,1,1) on the training period.
- Forecast the hold-out and compute MAE and MAPE.
- Plot train, test, and forecast; explain MAPE as average percentage error the inventory team can expect.

### Sub-step 4 – Extended model for extra patterns
- Add weekday indicators (day-of-week dummies) as exogenous features.
- Fit a seasonal SARIMA with weekly seasonality and exogenous regressors.
- Compare MAE and MAPE with the baseline and compute relative MAE improvement.
- Decide whether the improvement justifies added complexity.

### Sub-step 5 – Sensor failure-risk model (next 24 hours)
- Load `sensor_data_clean.csv` with label `failure_within_24h`.
- Use all numeric sensor features; drop timestamp and label.
- Split chronologically (first 80% train, last 20% test).
- Train a RandomForest classifier and evaluate with ROC-AUC and precision–recall curve.
- Print example thresholds with precision and recall so you can choose a recall-heavy operating point for maintenance.

---

## Hard notebook – rule vs model and cost-based threshold

### Sub-step 6 – Rule on a single sensor signal
- Choose a representative numeric sensor (first numeric column) and set a rule: alert when value > 99th percentile.
- Compute its confusion matrix and F1 on labelled data.
- This approximates the colleague's simple-threshold proposal.

### Sub-step 7 – Cost-based threshold selection
- On the same dataset, train a RandomForest model and get failure probabilities.
- For thresholds from 0.1 to 0.9, compute expected daily cost using:
  - high cost for missed failures (C_MISS),
  - lower cost for false alarms (C_FALSE),
  - 100,000 sensors in the fleet.
- Find the cost-minimising threshold and compare it with the F1-maximising threshold.
- This shows that the business-optimal threshold is not always the F1-optimal one.

---

## Suggested Git commit plan (6 commits)

1. `Add Day44 easy sales and sensor profiling notebook`
2. `Add ADF test and daily calendar + sensor cleaning exports`
3. `Add Day44 medium SARIMA baseline and plots`
4. `Add Day44 seasonal SARIMA with weekday exogenous features`
5. `Add Day44 sensor failure-risk RandomForest model`
6. `Add Day44 hard rule-vs-model cost analysis and logic markdown`

This gives six clear, incremental commits aligned to Easy → Medium → Hard work.
