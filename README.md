# PM2.5 Forecasting Time Series Analysis Project

A full forecasting workflow for particulate matter (PM2.5) across multiple cities. The project ingests raw WAQI-style CSV files, explores data quality, engineers time-series features, trains a diverse model suite (statistical, ML, and deep learning), and produces forecasts, visual evaluations, and an executive-ready report for January 2026.

## Highlights
- **End-to-end pipeline**: raw data → cleaned parquet → forecast-ready daily series → model training → forecasts + reports.
- **Model diversity**: ARIMA/SARIMA/Holt-Winters, Random Forest/Gradient Boosting/SVR, and LSTM/GRU/CNN-LSTM.
- **Actionable outputs**: CSV + Parquet forecasts, visualizations, model comparisons, and an executive report.

## Repository Structure
```
.
├── data/
│   └── processed/             # Filtered parquet + forecast-ready parquet
├── notebooks/
│   ├── 01_data_acquisition.ipynb
│   ├── 02_eda_and_insights.ipynb
│   ├── 03_data_forecasting.ipynb
│   ├── 04_data_evaluation_forecast.ipynb
│   └── 05_data_report.ipynb
├── predictions/               # Output forecasts (CSV + parquet)
├── docs/                      # Extended documentation
├── requirements.txt
└── README.md
```

## Quickstart
1. **Install dependencies**
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```

2. **Add raw data**
   - Place WAQI-style CSV files in `data/raw/`.
   - Each file should include at least `Date`, `City`, and `Specie` columns (the notebook uses `skiprows=4`).

3. **Run notebooks in order**
   1. `01_data_acquisition.ipynb` → filters and consolidates raw CSV files into `data/processed/filtered_data.parquet`.
   2. `02_eda_and_insights.ipynb` → generates plots and resamples data into `data/processed/forecast_ready.parquet`.
   3. `03_data_forecasting.ipynb` → trains 9 models per city and writes forecasts to `predictions/`.
   4. `04_data_evaluation_forecast.ipynb` → builds comparative visualizations and diagnostics.
   5. `05_data_report.ipynb` → generates a text report + summary CSVs.

## Key Outputs
- **Processed data**: `data/processed/filtered_data.parquet`, `data/processed/forecast_ready.parquet`
- **Forecasts**: `predictions/pm25_forecast_2026_01.csv` + `.parquet`
- **Visuals**: saved to `outputs/` by notebooks 2 and 4
- **Report**: `outputs/forecast_report.txt` + CSV tables

## Documentation
See the `docs/` folder for detailed pipeline, modeling, and reporting notes.

## Requirements
- Python 3.8+
- Dependencies listed in `requirements.txt` (TensorFlow is optional)

## Notes
- Forecast horizon in the default notebooks is **January 1–31, 2026**.
- `TARGET_CITIES` are defined in each notebook; adjust if you want a different geography.
