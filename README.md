# Target Revenue Analysis

Quarterly revenue analysis of Target Corporation (NYSE: TGT), prepared for Chapman Wealth Management by QuantFolio Solutions.

## Overview

This project analyzes Target's historical quarterly revenue to separate two dominant patterns in the data:

1. **A steady upward trend** — revenue drifts higher across the full history.
2. **A recurring holiday spike** — every fiscal Q4 (the Nov–Jan holiday quarter) jumps well above surrounding quarters.

A time-series regression is used to quantify both effects.

## Data

- Source file: `qSales_2024.csv` (quarterly financials for multiple companies)
- Filtered to Target Corporation (`tic == 'TGT'`), 2001–2023, 93 quarterly observations

## Methodology

**Model:** OLS regression with `time` plus two dummy variables:

```
saleq = β0 + β1·time + β2·holiday_dummy + β3·holiday_interaction
```

| Variable | Description |
|---|---|
| `time` | Period counter (1, 2, 3, …) capturing the underlying growth trend |
| `holiday_dummy` | 1 if fiscal Q4 (holiday quarter), 0 otherwise |
| `holiday_interaction` | `time × holiday_dummy`, allowing the holiday premium to grow/shrink over time |

**Train/test split:** first 75% of quarters for training, final 25% held out for evaluation, with 80% prediction intervals on the test set.

## Key Results

| Coefficient | Estimate | Interpretation |
|---|---|---|
| Intercept | ~$9,471M | Baseline non-holiday revenue at series start |
| `time` | ~$135M/quarter | Underlying trend growth |
| `holiday_dummy` | ~$4,007M | Extra revenue earned in a holiday quarter |
| `holiday_interaction` | ~$16M/quarter (not statistically significant, p = 0.338) | Holiday premium widens only slightly over time |

- R² = 0.896 (Adj. R² = 0.891)
- Most of Target's growth comes from the steady underlying trend rather than an expanding holiday effect.

## Repository Structure

```
├── Target_Revenue_Analysis-2.ipynb   # Full analysis notebook
├── qSales_2024.csv                   # Source data
└── README.md
```

## Requirements

```
pandas
numpy
statsmodels
matplotlib
```

## Usage

```bash
pip install pandas numpy statsmodels matplotlib
jupyter notebook Target_Revenue_Analysis-2.ipynb
```

## Author

QuantFolio Solutions — prepared for Chapman Wealth Management
