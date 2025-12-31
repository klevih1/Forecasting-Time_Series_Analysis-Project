# Reproducibility Guide

## Environment setup
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

If TensorFlow is difficult to install, you can comment it out in `requirements.txt`. The forecasting notebook will skip deep learning models gracefully.

## Notebook execution order
1. **01_data_acquisition.ipynb**
   - Reads CSVs from `data/raw/`
   - Writes `data/processed/filtered_data.parquet`

2. **02_eda_and_insights.ipynb**
   - Reads the filtered parquet file
   - Produces EDA charts in `outputs/`
   - Writes `data/processed/forecast_ready.parquet`

3. **03_data_forecasting.ipynb**
   - Reads the forecast-ready parquet file
   - Trains models and writes forecasts to `predictions/`
   - Writes `outputs/model_comparison.json`

4. **04_data_evaluation_forecast.ipynb**
   - Reads model comparisons + forecasts
   - Produces multi-city visual analysis in `outputs/`

5. **05_data_report.ipynb**
   - Generates `outputs/forecast_report.txt`
   - Writes supporting CSV tables

## Paths and assumptions
- Notebooks assume they are executed from inside `notebooks/` (they call `os.path.dirname(os.getcwd())`).
- Default target cities vary slightly across notebooks:
  - Acquisition: London, Paris, Beijing, Tokyo, Berlin
  - Forecasting & reporting: Boston, Paris, Beijing, Tokyo, Berlin, London

If you need to align city lists, edit each notebook's `TARGET_CITIES`.
