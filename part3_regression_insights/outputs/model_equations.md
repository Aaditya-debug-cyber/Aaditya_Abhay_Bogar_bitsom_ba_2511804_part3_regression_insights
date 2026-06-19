# Model Equations
**Analyst:** Aaditya Abhay Bogar (bitsom_ba_2511804)  
**Date:** 2026-06-19

---

## Dummy Variable Approach

Before running regressions, categorical variables were converted to binary dummy variables using the **drop-one category (reference category)** approach. This avoids perfect multicollinearity (the "dummy variable trap").

### Region Dummies (Reference Category = East)

| Dummy Variable | = 1 when | = 0 when |
|---|---|---|
| region_North | store is in North region | store is not in North |
| region_South | store is in South region | store is not in South |
| region_West  | store is in West region | store is not in West |
| **East (reference)** | — | All three dummies = 0 |

**Reference category interpretation:** The intercept and region-related coefficients are interpreted relative to the East region. A region_South coefficient of +19,692 means South stores generate ₹19,692 more per month on average compared to East stores with identical other characteristics.

### Store Type Dummies (Reference Category = Residential)

| Dummy Variable | = 1 when | = 0 when |
|---|---|---|
| type_HighStreet | store_type = High Street | store_type ≠ High Street |
| type_Airport    | store_type = Airport     | store_type ≠ Airport |
| type_Mall       | store_type = Mall        | store_type ≠ Mall |
| **Residential (reference)** | — | All three dummies = 0 |

**Reference category interpretation:** A type_Airport coefficient of +39,019 means Airport stores generate ₹39,019 more per month than a comparable Residential store.

---

## Model 1 — Simple Regression: monthly_sales ~ footfall

**Equation:**

```
monthly_sales = 446,411 + 35.68 × footfall
```

| Term | Value | Business Meaning |
|---|---|---|
| Intercept (446,411) | Base sales when footfall = 0 | Theoretical minimum (fixed costs, base revenue) |
| footfall coeff (35.68) | Each extra visitor adds ₹35.68 | For every 1,000 more visitors, sales rise by ~₹35,680 |

**Model fit:** R² = 0.7363 — footfall explains 73.6% of monthly sales variation  
**Significance:** p < 0.001 (***) for both intercept and footfall

---

## Model 2 — Simple Regression: monthly_sales ~ marketing_spend

**Equation:**

```
monthly_sales = 560,777 + 2.13 × marketing_spend
```

| Term | Value | Business Meaning |
|---|---|---|
| Intercept (560,777) | Base sales without marketing | Sales floor before marketing effect |
| marketing_spend coeff (2.13) | Each ₹1 in marketing → ₹2.13 in sales | Marketing multiplier of 2.13× (unadjusted for confounders) |

**Model fit:** R² = 0.1672 — marketing alone explains only 16.7% of variation  
**Significance:** p < 0.001 (***)

---

## Model 3 — Simple Regression: monthly_sales ~ staff_count

**Equation:**

```
monthly_sales = 400,551 + 16,984 × staff_count
```

| Term | Value | Business Meaning |
|---|---|---|
| Intercept (400,551) | Base sales with zero staff (theoretical) | Sales floor |
| staff_count coeff (16,984) | Each additional staff member → ₹16,984 more in sales | Drops dramatically in multiple model due to collinearity |

**Model fit:** R² = 0.6523  
**Significance:** p < 0.001 (***)

---

## Model 4 — Multiple Regression (Final Model)

**Equation:**

```
monthly_sales = 89,737
              + 27.99 × footfall
              + 1.15  × marketing_spend
              + 3,026 × staff_count
              − 49,804 × avg_discount_pct
              + 3,033 × inventory_availability_pct
              + 16,167 × holiday_flag
              + 7,711 × region_North
              + 19,692 × region_South
              + 19,832 × region_West
              + 17,888 × type_HighStreet
              + 39,019 × type_Airport
              + 29,745 × type_Mall
```

### Coefficient Interpretation (Business Language)

| Variable | Coefficient | p-value | Business Meaning |
|---|---|---|---|
| Intercept | 89,737 | 0.038 * | Base monthly sales for an East-region Residential store with all continuous vars = 0 (theoretical baseline) |
| footfall | +27.99 | <0.001 *** | Every 1,000 additional store visitors generates ~₹27,990 more in monthly sales (holding all else constant) |
| marketing_spend | +1.15 | <0.001 *** | Each additional ₹1 in marketing spend is associated with ₹1.15 more in monthly sales (marginal ROI after controlling for footfall) |
| staff_count | +3,026 | 0.015 * | Each additional staff member adds ~₹3,026 in monthly sales after controlling for footfall (much lower than simple regression due to collinearity) |
| avg_discount_pct | -49,804 | 0.171 ns | Not statistically significant — discount level does not reliably predict sales in this model |
| inventory_availability_pct | +3,033 | <0.001 *** | Each 1-percentage-point improvement in inventory availability adds ~₹3,033 in monthly sales |
| holiday_flag | +16,167 | 0.014 * | Holiday months generate ~₹16,167 more than non-holiday months on average |
| region_North | +7,711 | 0.269 ns | North stores sell slightly more than East, but not statistically significant |
| region_South | +19,692 | 0.006 ** | South stores generate ~₹19,692 more per month than comparable East stores |
| region_West | +19,832 | 0.002 ** | West stores generate ~₹19,832 more per month than comparable East stores |
| type_HighStreet | +17,888 | 0.003 ** | High Street stores outperform Residential by ~₹17,888/month |
| type_Airport | +39,019 | <0.001 *** | Airport stores generate the highest store-type premium: ~₹39,019/month above Residential |
| type_Mall | +29,745 | <0.001 *** | Mall stores generate ~₹29,745/month above Residential stores |

**Model fit:** R² = 0.8337 | Adjusted R² = 0.8272 | RMSE ≈ ₹43,180

---

## Final Model Selected: Model 4 (Multiple Regression)

**Reason for selection:**
1. **Highest explanatory power:** R² = 83.4%, Adj R² = 82.7% — 10 points better than the next best model (Model 1)
2. **Lowest prediction error:** RMSE ≈ ₹43,180 vs ₹54,010 for Model 1
3. **Accounts for structural factors:** Store type and regional effects are properly controlled via dummy variables
4. **Multiple actionable levers:** Footfall, marketing, inventory, holiday timing, and store type all provide direction for business decisions
5. **Statistically sound:** 9 of 12 predictors are significant at p < 0.05; Adjusted R² confirms parsimony

**Variables not recommended for action:** avg_discount_pct (p=0.17, not significant), region_North (p=0.27, not different from East)
