# Day 40 – Time Series & Sensor Data Logic

## Overall idea

The assignment has two questions at the Easy level:
1. Understand and clean daily ecommerce sales time series before modelling.
2. Detect and fix sensor data quality issues that would break sequence models.

Both notebooks keep the code simple and rely on standard libraries only.

---

## Q1 – Ecommerce sales time series

### Goals
- Load `ecommerce_sales_ts.csv` safely.
- Characterise the series: date range, frequency, seasonality, stationarity.
- Identify data quality issues: missing days, missing values, duplicates, negatives, and outliers.
- Produce a clean daily series ready for forecasting.

### Key steps
- Parse the `date` column to datetime, sort, and set as index.
- Inspect index differences to infer if the series is truly daily or has gaps.
- Plot the full series and a 30‑day rolling mean to see trend and seasonality.
- Run the Augmented Dickey–Fuller (ADF) test to check basic stationarity.
- Count missing values, duplicate timestamps, negatives, zeros, and extreme tails.
- Reindex to a full daily calendar between min and max date, then interpolate sales.
- Drop duplicate timestamps and save `ecommerce_sales_ts_clean.csv` for later models.

This clean file is what all subsequent sales‑forecasting sub‑steps (Medium/Hard) should use.

---

## Q2 – Sensor data quality

### Goals
- Load `sensor_data.csv` and profile the timestamped measurements.
- Identify structural problems that break sequence models: unsorted timestamps, duplicates, irregular frequency, missing values, and extreme spikes.
- Produce a cleaned, regularly spaced multivariate time series.

### Key steps
- Parse the `timestamp` column, sort by time, and print dtypes and missing‑value counts.
- Count duplicate timestamps and inspect the most common time differences to infer the intended sampling interval.
- Describe numeric columns, flagging extreme outliers via z‑scores.
- Drop duplicate timestamps, set a time index, and reindex to the dominant sampling frequency.
- Interpolate missing values across all numeric sensors.
- Replace values with z‑score greater than 4 by NaN, then interpolate again to smooth sensor spikes that likely come from logging glitches.
- Save `sensor_data_clean.csv` as the canonical cleaned dataset for any RNN/sequence model.

---

## Suggested commit plan (to reach 6 commits overall)

You can align your Git commits with the phases below:

1. `Add Day40 sales time series loading and profiling (Q1)`
2. `Add ADF stationarity test and daily calendar reindexing (Q1)`
3. `Add sensor data profiling and duplicate checks (Q2)`
4. `Add sensor cleaning pipeline and export cleaned CSV (Q2)`
5. `Add Day40 logic markdown documentation`
6. `Tidy notebooks and update week07 README for Day40`

This gives you six meaningful commits that match the structure of the work.
