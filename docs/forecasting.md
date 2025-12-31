# Forecasting Pipeline

The forecasting notebook (`03_data_forecasting.ipynb`) trains a suite of nine models for each city, selects the best by cross-validated MAE, and generates January 2026 forecasts with confidence intervals.

## Feature engineering
For each city, the notebook builds:
- **Calendar features**: year, month, day, day-of-week, day-of-year, quarter, week-of-year
- **Cyclical encodings**: sine/cosine for month and weekday
- **Lag features**: 1, 2, 3, 7, 14, 30-day lags
- **Rolling stats**: mean/std/min/max over 7, 14, 30 days
- **EWMA**: 7-day and 30-day exponential averages

## Model families
**Statistical**
- ARIMA
- SARIMA
- Holt-Winters Exponential Smoothing

**Machine learning**
- Random Forest
- Gradient Boosting
- SVR (with standard scaling)

**Deep learning** (optional, if TensorFlow is available)
- LSTM
- GRU
- CNN-LSTM

## Evaluation strategy
- **Time-series cross validation**: 5 folds, 30-day test windows.
- **Metrics**: MAE, RMSE, MAPE.
- Best model per city is selected by lowest MAE.

## Forecast output
Forecasts are generated for **2026-01-01 → 2026-01-31** and include:
- Prediction
- 80% & 95% confidence intervals (based on residual std)

Outputs:
- `predictions/pm25_forecast_2026_01.csv`
- `predictions/pm25_forecast_2026_01.parquet`
- `outputs/model_comparison.json` (best model + scores per city)

## Customization tips
- Change `FORECAST_START` / `FORECAST_END` for new horizons.
- Add or remove models in the definitions block of the notebook.
- Adjust feature windows or add exogenous features if available.
