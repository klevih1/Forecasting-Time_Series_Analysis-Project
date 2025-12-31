# PM2.5 Forecast Report (January 2026)

Generated from `data/processed/forecast_ready.parquet` and `predictions/pm25_forecast_2026_01.parquet`.

## Executive summary
- Cities covered: Beijing, Berlin, Boston, London, Paris, Tokyo
- Highest average forecast PM2.5: **Paris** (101.7 µg/m³)
- Lowest average forecast PM2.5: **Boston** (31.6 µg/m³)
- Largest increase vs historical January: **Paris** (89.7%)
- Largest decrease vs historical January: **Beijing** (-23.0%)
- Highest forecast uncertainty: **Beijing** (avg 95% CI width 107.27 µg/m³)
- Lowest forecast uncertainty: **Paris** (avg 95% CI width 22.95 µg/m³)

## Visual analysis (rendered via notebook cells)
Binary assets are not included in this repo. Use the following notebook code snippets to render the visuals locally. Narrative commentary is embedded in `notebooks/04_data_evaluation_forecast.ipynb` and `notebooks/05_data_report.ipynb`.

### Daily forecast trajectories
```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

pred = pd.read_parquet('predictions/pm25_forecast_2026_01.parquet')
pred['Date'] = pd.to_datetime(pred['Date'])

plt.figure(figsize=(12, 6))
sns.lineplot(data=pred, x='Date', y='Prediction', hue='City', linewidth=2)
plt.title('January 2026 PM2.5 Forecasts by City')
plt.ylabel('PM2.5 (µg/m³)')
plt.xlabel('Date')
plt.legend(title='City', bbox_to_anchor=(1.02, 1), loc='upper left')
plt.tight_layout()
plt.show()
```

### Distribution of daily forecasts
```python
plt.figure(figsize=(10, 6))
sns.boxplot(data=pred, x='City', y='Prediction', hue='City', palette='Set3', legend=False)
plt.title('Distribution of January 2026 Forecasts')
plt.ylabel('PM2.5 (µg/m³)')
plt.xlabel('City')
plt.xticks(rotation=30)
plt.tight_layout()
plt.show()
```

### Average forecast with 95% CI width
```python
summary = pred.groupby('City').agg(
    avg=('Prediction', 'mean'),
    lower=('Lower_95', 'mean'),
    upper=('Upper_95', 'mean'),
)
summary['ci_width'] = summary['upper'] - summary['lower']
summary = summary.sort_values('avg')

plt.figure(figsize=(10, 6))
plt.barh(summary.index, summary['avg'], xerr=summary['ci_width'] / 2, color='#5dade2', alpha=0.85)
plt.title('Average January Forecast with 95% CI Width')
plt.xlabel('PM2.5 (µg/m³)')
plt.tight_layout()
plt.show()
```

### Peak day severity by city
```python
peak = pred.loc[pred.groupby('City')['Prediction'].idxmax()].sort_values('Prediction')

plt.figure(figsize=(10, 6))
plt.barh(peak['City'], peak['Prediction'], color='#ec7063')
plt.title('Peak Forecast Day by City (Jan 2026)')
plt.xlabel('PM2.5 (µg/m³)')
plt.tight_layout()
plt.show()
```

## January 2026 forecast summary
```text
           mean    std    min     max
City                                 
Beijing   86.11  13.60  51.96  121.00
Berlin    45.12   4.78  41.23   64.17
Boston    31.59   4.13  23.03   38.02
London    39.87   1.30  37.43   43.40
Paris    101.72  19.50  74.75  134.99
Tokyo     40.66   4.03  30.92   49.20
```

## Historical January comparison
```text
         Historical_Jan_Mean  Forecast_Jan_2026  Difference  Pct_Change
City                                                                   
Beijing               111.86              86.11      -25.75       -23.0
Berlin                 44.20              45.12        0.92         2.1
Boston                 28.65              31.59        2.94        10.3
London                 38.03              39.87        1.84         4.8
Paris                  53.62             101.72       48.10        89.7
Tokyo                  39.63              40.66        1.03         2.6
```

## Forecast uncertainty (95% CI width)
```text
         Prediction  Lower_95  Upper_95  CI_Width  Relative_Uncertainty
City                                                                   
Beijing       86.11     32.48    139.75    107.27                 124.6
Berlin        45.12     33.68     56.55     22.87                  50.7
Boston        31.59     22.49     40.69     18.20                  57.6
London        39.87     31.40     48.34     16.94                  42.5
Paris        101.72     90.25    113.20     22.95                  22.6
Tokyo         40.66     29.27     52.04     22.77                  56.0
```

## Health risk assessment (based on forecast averages)
```text
         Avg_Level  Max_Level  Days_Good  Days_Moderate  Days_Unhealthy_Sensitive  Days_Unhealthy  Days_Very_Unhealthy             Overall_Risk
City                                                                                                                                           
Beijing       86.1      121.0          0              0                         1              30                    0                Unhealthy
Berlin        45.1       64.2          0              0                        30               1                    0  Unhealthy for Sensitive
Boston        31.6       38.0          0             25                         6               0                    0                 Moderate
London        39.9       43.4          0              0                        31               0                    0  Unhealthy for Sensitive
Paris        101.7      135.0          0              0                         0              31                    0                Unhealthy
Tokyo         40.7       49.2          0              2                        29               0                    0  Unhealthy for Sensitive
```

## Notable insights (linked to the notebook visuals)
- The **trajectory plot** shows Paris consistently above the other cities, with a widening gap mid-month, matching the highest average and peak values.
- The **distribution plot** highlights Paris’s broader spread and higher median, while London and Berlin are tightly clustered with lower variance.
- The **average + CI chart** emphasizes Beijing’s uncertainty band relative to its mean, signaling less confidence in day-to-day values despite a lower January mean than historical.
- The **peak day chart** indicates a Paris peak near 135 µg/m³, supporting the “Unhealthy” classification for all 31 days.

## January forecast highlights
- **Beijing**: peak 121.0 µg/m³ on 2026-01-22 | low 52.0 µg/m³ on 2026-01-06 | avg 86.1 µg/m³
- **Berlin**: peak 64.2 µg/m³ on 2026-01-01 | low 41.2 µg/m³ on 2026-01-17 | avg 45.1 µg/m³
- **Boston**: peak 38.0 µg/m³ on 2026-01-25 | low 23.0 µg/m³ on 2026-01-01 | avg 31.6 µg/m³
- **London**: peak 43.4 µg/m³ on 2026-01-07 | low 37.4 µg/m³ on 2026-01-05 | avg 39.9 µg/m³
- **Paris**: peak 135.0 µg/m³ on 2026-01-20 | low 74.8 µg/m³ on 2026-01-04 | avg 101.7 µg/m³
- **Tokyo**: peak 49.2 µg/m³ on 2026-01-16 | low 30.9 µg/m³ on 2026-01-03 | avg 40.7 µg/m³

## Recommendations
- **Paris**: sustain public advisories throughout January, focus on high-risk populations, and validate model drivers behind the large historical jump.
- **Beijing**: prioritize real-time monitoring given wide uncertainty; consider adaptive thresholds for alerts.
- **Boston/London/Berlin/Tokyo**: maintain routine monitoring, with attention to early-month peaks (Berlin) and mid-month upticks (Tokyo).
