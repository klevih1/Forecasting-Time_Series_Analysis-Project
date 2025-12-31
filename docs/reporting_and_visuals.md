# Reporting & Visuals

Two notebooks generate visual and narrative outputs for stakeholders.

## Exploratory analysis (`02_eda_and_insights.ipynb`)
Key artifacts:
- **Gap analysis heatmap**: visualizes missing data per city.
- **Historical trend plot**: compares PM2.5 trajectories.
- **Seasonality heatmap**: average monthly PM2.5 by city.
- **Violin plot**: distribution and volatility by city.

Outputs saved to `outputs/`:
- `overall_trends.png`
- `seasonal_heatmap.png`
- `city_volatility.png`

## Forecast evaluation (`04_data_evaluation_forecast.ipynb`)
Key artifacts:
- **Multi-city overview** with confidence intervals.
- **Detailed per-city plots** with training/forecast split.
- **Zoom view** for recent history + forecast.
- **Performance summary bar chart** showing best model MAE.
- **Console diagnostics** to explain low-MAPE but “flat” forecasts.

Outputs saved to `outputs/`:
- `all_forecasts_with_training.png`
- `detailed_forecast_<city>.png`
- `zoom_forecast_view.png`
- `performance_summary_clean.png`

## Executive report (`05_data_report.ipynb`)
Generates a plain-text report and supporting CSV tables:
- **Forecast summary** (mean/std/min/max per city).
- **Historical January comparison** vs prior years.
- **Uncertainty analysis** (relative CI width).
- **Health risk assessment** (Good → Very Unhealthy).
- **Actionable recommendations** per city.

Outputs saved to `outputs/`:
- `forecast_report.txt`
- `forecast_summary.csv`
- `historical_comparison.csv`
- `uncertainty_analysis.csv`
- `health_risk_assessment.csv`
