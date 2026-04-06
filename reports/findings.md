# What Drives Inflation? — Key Findings

---

## The Question

Inflation is one of the most consequential macroeconomic variables for households, businesses, and policymakers. Yet the relative importance of its drivers — labour market tightness, monetary policy, financial conditions, and inflation expectations — remains actively debated.

This project asks two questions:
1. What are the primary economic drivers of U.S. CPI inflation?
2. How accurately can inflation be predicted using machine learning?

---

## Data

Seven macroeconomic time series were sourced from the Federal Reserve (FRED), covering **253 monthly observations from December 2004 to January 2026**. This window spans three distinct macro regimes: the Global Financial Crisis, the COVID shock, and the post-COVID inflation surge.

| Variable | Description |
|---|---|
| CPI YoY | Target — year-over-year inflation |
| Unit Labour Cost YoY | Labour market cost pressure |
| Unemployment Rate | Labour market slack |
| Fed Funds Rate (3m change) | Monetary policy stance |
| Fed Total Assets YoY | Quantitative easing proxy |
| Real GDP YoY | Economic activity |
| 10Y Breakeven Inflation | Market inflation expectations |

Features were lagged at 1, 3, 6, and 12 months to capture delayed transmission effects. Three binary crisis dummies were added to account for structural breaks (GFC, COVID, post-COVID surge).

---

## Models

Five regression models were trained on an 80/20 temporal train-test split (no shuffling, to preserve time order). Linear models used `GridSearchCV` with `TimeSeriesSplit`; tree-based models used `RandomizedSearchCV`.

| Model | RMSE | MAE | R² |
|---|---|---|---|
| **Bayesian Ridge** | **~0.42** | **~0.33** | **~0.96** |
| LASSO | ~0.65 | ~0.50 | ~0.90 |
| Ridge | ~0.90 | ~0.70 | ~0.82 |
| Gradient Boosting | ~1.05 | ~0.80 | ~0.76 |
| Random Forest | ~1.10 | ~0.85 | ~0.74 |

---

## Key Findings

**1. Inflation expectations dominate.**
The 10-year breakeven inflation rate is the strongest single predictor of realized CPI inflation across all models and lags. This confirms that market expectations closely track actual price dynamics and are forward-looking in a meaningful way.

**2. Inflation is persistent.**
The own lag of inflation (1-month) ranks highly in every model. CPI inflation carries significant momentum — past inflation is a strong predictor of future inflation, consistent with well-established macroeconomic theory.

**3. Labour market tightness matters, but with a lag.**
Unit labour costs and unemployment show meaningful predictive power at the 6–12 month horizon, reflecting the gradual pass-through from wage pressures to consumer prices.

**4. Linear models outperform tree-based models.**
Bayesian Ridge significantly outperforms Random Forest and Gradient Boosting on the test set. This is a known limitation of tree-based models: they cannot extrapolate beyond the range of values seen in training. The post-2021 inflation surge reached levels unseen in the training period, which caused both ensemble models to underpredict the peak and overpredict during disinflation.

**5. Structural breaks matter.**
Explicitly modelling the GFC, COVID, and post-COVID surge with binary dummies substantially improved performance. Models without these dummies struggled to account for regime shifts.

---

## Limitations

- **Data scope:** Only seven macroeconomic variables were used. Supply-side drivers (e.g. commodity prices, shipping costs, producer prices) were excluded and could improve predictive power.
- **Look-ahead in dummies:** The crisis dummies are defined using knowledge of when crises occurred. In a true real-time forecasting setting, these would not be available in advance.
- **Tree model extrapolation:** Random Forest and Gradient Boosting are fundamentally interpolators. Their poor performance on the post-2021 surge is structural, not a tuning issue.
- **No formal time-series modelling:** ARIMA or VAR models were not benchmarked. Adding these would strengthen the comparison.

---

## Conclusion

U.S. inflation is a **persistent, largely linear phenomenon** best predicted by models that respect its temporal structure and account for structural breaks. Inflation expectations are the dominant driver; labour market and monetary policy variables matter most at the 6–12 month horizon. Tree-based methods are poorly suited to this type of macroeconomic forecasting problem.
