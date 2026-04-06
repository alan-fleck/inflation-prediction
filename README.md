# What Drives Inflation?
A machine learning analysis of U.S. CPI inflation using macroeconomic data from FRED.

## Overview
This project explores the primary drivers of U.S. CPI inflation and benchmarks five regression models — LASSO, Ridge, Bayesian Ridge, Random Forest, and Gradient Boosting — on a 253-month dataset (Dec 2004 – Jan 2026).

## Results

| Model | RMSE | MAE | R² |
|---|---|---|---|
| Bayesian Ridge | ~0.42 | ~0.33 | ~0.96 |
| LASSO | ~0.65 | ~0.50 | ~0.90 |
| Ridge | ~0.90 | ~0.70 | ~0.82 |
| Gradient Boosting | ~1.05 | ~0.80 | ~0.76 |
| Random Forest | ~1.10 | ~0.85 | ~0.74 |

## Setup

```bash
pip install -r requirements.txt
jupyter notebook notebook.ipynb
```

## Data
Data is fetched automatically from [FRED](https://fred.stlouisfed.org/) on first run and cached locally. No manual download needed.

## Project Structure
```
├── notebook.ipynb      # Full analysis
├── requirements.txt    # Dependencies
├── .gitignore
└── README.md
```
