# Data Pipeline

## Raw data expectations
The acquisition notebook expects WAQI-style CSV files in `data/raw/` with:
- A 4-line header comment (handled by `skiprows=4`).
- Columns that include **Date**, **City**, and **Specie**.

If your raw files do not match this format, adjust `skiprows` or the required column list in `notebooks/01_data_acquisition.ipynb`.

## Filtering & normalization
`01_data_acquisition.ipynb` performs the following:
- Normalizes city names to title case.
- Filters to `TARGET_CITIES` (defaults: London, Paris, Beijing, Tokyo, Berlin).
- Concatenates all city slices into a single dataframe.
- Sorts by `City`, `Date`, and `Specie`.

Output:
- `data/processed/filtered_data.parquet`

## Forecast-ready resampling
`02_eda_and_insights.ipynb` prepares a daily series:
- Filters to **pm25** (`Specie == 'pm25'`).
- Resamples to daily frequency per city (mean aggregation).

Output:
- `data/processed/forecast_ready.parquet`

## Notes for adapting new datasets
- Update `TARGET_CITIES` in notebooks if you change regions.
- Ensure `Date` parses cleanly into a datetime column.
- If you need additional pollutants, adjust the `target_specie` filter.
