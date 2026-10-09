# Solar Forecast Accuracy & DSM Penalty Risk: 42-Site Study

**Day-ahead forecasts reached a median error (MAPE) of 9% and median R² of 0.97 across 42 grid-connected solar sites (71 MWp); 40 of 42 sites stayed inside the penalty-safe band of India's Deviation Settlement Mechanism (DSM).**

Under DSM rules, a solar plant pays a penalty when actual generation deviates too far from its day-ahead schedule. This study measures how accurate forecasts are, where they fail, and what the error costs.

> Source code and site-level raw data are private (client data). This repo shows the question, method and results.

![Error by site](charts/error_by_site.png)

## Data

- 15-minute generation, irradiance, weather and rainfall data, 45 sites in Gujarat
- Up to 5 years per site (e.g. Sep 2019 – Dec 2024)
- Site IDs are anonymised

**Cleaning done:** mixed date formats across sites (`dd-mm-yyyy`, `dd/mm/yy`, quoted timestamps), missing 15-minute intervals, night-time values, and unit mismatches between sources.

## Results by weather type

Sites were grouped by how stable their weather is:

| Weather group | Sites | Median MAPE | Median R² | Penalty as % of revenue |
|---|---|---|---|---|
| Stable | 13 | 6.6% | 0.987 | 2.4% |
| Mixed | 23 | 9.6% | 0.972 | 4.8% |
| Volatile | 6 | 16.3% | 0.855 | 17.0% |

The 6 volatile-weather sites carry most of the penalty risk: their penalty share is about 7× that of stable sites. That is where better forecasting or bid revision pays off most.

## Forecast ranges

Forecasts give a low / expected / high range (P10 / P50 / P90). A well-calibrated range should have about 10%, 50% and 90% of actual values below each line.

| Range line | Expected | Actual |
|---|---|---|
| P10 | 10% | 5.6% |
| P50 | 50% | 55.0% |
| P90 | 90% | 93.7% |

Calibrating forecasts against actual generation cut median error by 14% (MAE 101 kW → 87 kW) at pilot sites.

![Calibration](charts/calibration.png)

## Metrics used

- **MAE**: average absolute gap between forecast and actual, in kW
- **MAPE**: that gap as % of actual
- **R²**: how much of the variation in generation the forecast explains (1 = perfect)
- **Penalty**: DSM deviation charges estimated from the error in each 15-minute block

## Files

- `site_results.csv`: one row per site (anonymised): capacity, weather group, R², MAPE, penalty and revenue (Rs lakh)
- `by_weather_group.csv`: the summary table above

**Tools:** Python, pandas, statistics, Excel. Forecast models built with AI-assisted development.
