# Fine vs Coarse Dominance from EPA PM2.5/PM10 (2022–2024)

This repository provides a fully reproducible workflow to (i) download and
prepare daily EPA AirData/AQS particulate datasets and (ii) analyze fine vs
coarse particle dominance across U.S. monitoring sites using a sample-level
**Fine Dominance Index (FDI)** and a cell-level composite index **IFS**.

- **FDI (sample)** = (PM2.5 / PM10) × (1 − r0), with r0 = (PM10 − PM2.5)/(PM10 + PM2.5)
- **IFS (cell)** = standardized composite of mean FDI and r0 shape descriptors
  (mean, skewness, kurtosis) aggregated by Land Use × Location Setting × Season.

The analysis stratifies by land use (Commercial, Residential, Industrial,
Agricultural, Desert, Military reservation, Forest), by location setting
(Rural, Suburban, Urban and center city), and by season (DJF, MAM, JJA, SON).
Uncertainty is quantified with bootstrap CIs; group contrasts use
nonparametric tests (Kruskal–Wallis, Mann–Whitney with Holm correction) and
Cliff’s delta effect sizes.

---


> Paths are created on the fly. You can change them via script constants/args.

---

## Data source

- **EPA AirData / AQS** “daily summary” files (public, no API key):
  https://aqs.epa.gov/aqsweb/airdata/download_files.html  
  Parameters used:
  - PM2.5 FRM/FEM (`daily_88101_YYYY.zip`)
  - PM2.5 non-FRM/FEM (`daily_88502_YYYY.zip`) — used for sensitivity checks
  - PM10 (`daily_81102_YYYY.zip`)
  - PMc (`daily_86101_YYYY.zip`) — auxiliary consistency checks

Daily files contain one daily mean per site/parameter plus metadata (lat/lon,
land use, location setting). Scripts retain the minimal fields required.

---

## Environment

- Python **3.10+** recommended


## Reproducibility notes
- Bootstrap uses --random-state for deterministic resampling.
- Completeness screen at station–parameter level: retain stations with
≥1000 valid daily values over 2022–2024; ratio days require same-day PM2.5
and PM10.
- Invalid ratios (zero/negative denominators, infinities) are dropped.
Thin cells: flagged when n < 300 or wide CI; excluded from rankings but
shown muted in heatmaps.
