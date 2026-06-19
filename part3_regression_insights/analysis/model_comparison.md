# Model Comparison
**Analyst:** Aaditya Abhay Bogar (bitsom_ba_2511804)  
**Date:** 2026-06-19  
**Dataset:** business_regression_data.xlsx | N=320 (80 stores × 4 months)

---

## Models Built

### Model 1 — Simple Regression: footfall

| Item | Value |
|---|---|
| Dependent variable | monthly_sales |
| Independent variable | footfall |
| R-squared | 0.7363 |
| Adjusted R-squared | 0.7355 |
| RMSE | ~54,010 |
| Intercept | 446,411 (p < 0.001) |
| footfall coefficient | 35.68 (p < 0.001 ***) |

**Business usefulness:** Strong. Footfall alone explains 73.6% of monthly sales variation. This is the single most powerful predictor in the dataset. Each additional visitor is associated with ~₹35.68 more in sales. Very useful for capacity planning and store traffic initiatives.

**Limitations:** Does not account for store type, region, marketing spend, or seasonal effects. Cannot separate footfall driven by the campaign from footfall driven by location.

---

### Model 2 — Simple Regression: marketing_spend

| Item | Value |
|---|---|
| Dependent variable | monthly_sales |
| Independent variable | marketing_spend |
| R-squared | 0.1672 |
| Adjusted R-squared | 0.1646 |
| RMSE | ~94,850 |
| Intercept | 560,777 (p < 0.001) |
| marketing_spend coefficient | 2.13 (p < 0.001 ***) |

**Business usefulness:** Moderate. Significant relationship but explains only 16.7% of sales variation — far less than footfall. The estimated marketing multiplier of ₹2.13 per ₹1 spent appears promising but is likely confounded (stores that spend more may also have higher footfall by nature).

**Limitations:** Confounding is a serious concern — marketing spend and footfall are correlated. Isolated ROI from marketing alone cannot be reliably estimated from this model.

---

### Model 3 — Simple Regression: staff_count

| Item | Value |
|---|---|
| Dependent variable | monthly_sales |
| Independent variable | staff_count |
| R-squared | 0.6523 |
| Adjusted R-squared | 0.6512 |
| RMSE | ~61,720 |
| Intercept | 400,551 (p < 0.001) |
| staff_count coefficient | 16,984 (p < 0.001 ***) |

**Business usefulness:** Strong in isolation but likely collinear with footfall (larger stores need more staff and attract more footfall). Coefficient of ₹16,984 per staff member drops to ₹3,026 in the multiple model — confirming this variable shares variance with footfall.

**Limitations:** Staff count is likely an endogenous variable — stores assign staff based on expected footfall. Cannot use this model alone for staffing decisions.

---

### Model 4 — Multiple Regression (Final Model)

| Item | Value |
|---|---|
| Dependent variable | monthly_sales |
| Independent variables | footfall, marketing_spend, staff_count, avg_discount_pct, inventory_availability_pct, holiday_flag, region dummies (North, South, West vs East), store_type dummies (High Street, Airport, Mall vs Residential) |
| R-squared | 0.8337 |
| Adjusted R-squared | 0.8272 |
| RMSE | ~43,180 |
| No. of predictors | 12 |

**Significant variables (p < 0.05):** footfall (***), marketing_spend (***), inventory_availability_pct (***), type_Airport (***), type_Mall (***), region_South (**), region_West (**), type_HighStreet (**), holiday_flag (*), staff_count (*), intercept (*)

**Non-significant variables (p > 0.05):** avg_discount_pct (p=0.17), region_North (p=0.27)

**Business usefulness:** Highest explanatory power — explains 83.4% of sales variance. Accounts for store location, store format, marketing, footfall, inventory, and seasonality simultaneously. Best model for understanding what drives sales.

**Limitations:** Cannot establish causation. Panel data structure (repeated measures on same stores) may violate OLS independence assumptions. Does not include store-level fixed effects.

---

## Model Comparison Table

| Model | Variables | R² | Adj R² | RMSE | Key Sig Vars | Recommended? |
|---|---|---|---|---|---|---|
| M1: footfall | footfall | 0.7363 | 0.7355 | ₹54,010 | footfall *** | No (simple only) |
| M2: marketing | marketing_spend | 0.1672 | 0.1646 | ₹94,850 | marketing_spend *** | No (weak explanatory power) |
| M3: staff | staff_count | 0.6523 | 0.6512 | ₹61,720 | staff_count *** | No (collinear with footfall) |
| **M4: Multiple** | **12 predictors** | **0.8337** | **0.8272** | **₹43,180** | **7+ variables** | **YES** |

---

## Why Model 4 is the Final Model

1. **Highest R²:** Explains 83.4% of variation — 10+ percentage points above the next best (Model 1)
2. **Lowest RMSE:** Average prediction error of ₹43,180 vs ₹54,010 for Model 1
3. **Includes dummies:** Properly controls for store type and region effects
4. **Most actionable:** Multiple levers identified — leadership can act on footfall, marketing, inventory, and store type strategy simultaneously
5. **Adjusted R² penalty:** Adj R² = 0.8272 confirms the improvement is not just from adding variables — the additional predictors genuinely improve model quality

---

## Key Limitations Across All Models

- **Correlation ≠ causation:** No model can prove that increasing marketing spend *causes* higher sales — only that they are associated
- **Panel data issue:** The same 80 stores appear 4 times each. Standard OLS assumes independent observations — this is technically violated. A fixed-effects panel model would be more appropriate
- **Multicollinearity:** footfall and staff_count are correlated, which affects coefficient interpretation in the multiple model
- **Omitted variables:** Store age, competitor promotions, economic conditions, and local demographics are not in the data but likely influence sales
- **avg_discount_pct:** Not statistically significant — discounting may not be a useful lever, or its effect is too noisy to detect across stores
