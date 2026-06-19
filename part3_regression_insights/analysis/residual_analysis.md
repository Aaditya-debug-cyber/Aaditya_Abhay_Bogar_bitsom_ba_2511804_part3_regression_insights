# Residual Analysis
**Model:** Model 4 — Multiple Regression (Final Model)  
**Analyst:** Aaditya Abhay Bogar (bitsom_ba_2511804)  
**Residual definition:** Residual = Actual monthly_sales − Predicted monthly_sales

---

## Residual Summary Statistics

| Statistic | Value |
|---|---|
| Mean residual | ~₹0 (as expected for OLS) |
| Std dev of residuals | ~₹43,800 |
| Min residual | -₹175,028 |
| Max residual | +₹107,922 |
| RMSE (Root Mean Square Error) | ~₹43,180 |

A mean residual near zero confirms the model is unbiased on average. The RMSE of ~₹43,180 means the model's predictions are off by approximately ₹43,180 on average.

---

## Top 5 Positive Residuals (Model Under-predicts — Actual > Predicted)

These stores performed better than the model expected given their observable characteristics.

| Store | Region | Store Type | Actual Sales | Predicted | Residual |
|---|---|---|---|---|---|
| STR-1028 | East | Mall | ₹713,611 | ₹605,689 | **+₹107,922** |
| STR-1026 | East | Mall | ₹625,514 | ₹520,809 | **+₹104,705** |
| STR-1051 | East | Airport | ₹787,716 | ₹684,085 | **+₹103,631** |
| STR-1030 | West | Residential | ₹820,519 | ₹716,894 | **+₹103,625** |
| STR-1073 | East | Residential | ₹813,317 | ₹718,009 | **+₹95,308** |

**Business interpretation of positive residuals:**

These stores are outperforming what the model predicts from their observable characteristics (footfall, marketing spend, inventory, etc.). Possible explanations include:

1. **Unobserved local advantages:** Strong store manager, loyal local customer base, unique product mix, or premium catchment area
2. **Mall stores STR-1028 and STR-1026:** Both are East region Malls that outperform predicted sales — they may be in high-traffic mall locations (anchor stores, premium zones) not captured by the footfall count alone
3. **STR-1030 and STR-1073 (Residential):** Residential stores in East and West outperforming the Residential baseline — may have converted to a premium neighbourhood or have a specialty product range
4. **Action:** These stores are candidates for best-practice case studies — understanding why they outperform could inform replicable strategies

---

## Top 5 Negative Residuals (Model Over-predicts — Predicted > Actual)

These stores performed worse than the model expected given their observable characteristics.

| Store | Region | Store Type | Actual Sales | Predicted | Residual |
|---|---|---|---|---|---|
| STR-1017 | West | High Street | ₹685,379 | ₹860,407 | **-₹175,028** |
| STR-1012 | West | Residential | ₹595,468 | ₹729,837 | **-₹134,369** |
| STR-1023 | South | Mall | ₹627,172 | ₹753,250 | **-₹126,078** |
| STR-1007 | West | Mall | ₹800,452 | ₹909,274 | **-₹108,822** |
| STR-1077 | South | Residential | ₹538,376 | ₹634,782 | **-₹96,406** |

**Business interpretation of negative residuals:**

These stores are significantly underperforming relative to what the model would predict for stores with similar footfall, marketing spend, and characteristics. Possible explanations include:

1. **STR-1017 (West, High Street, largest negative residual -₹175,028):** High Street in West region is the most significantly underperforming store. Despite similar footfall and marketing, it generates far less revenue. Possible causes: poor product mix, high shrinkage, ineffective staff deployment, or local competitive pressure not captured in competitor_distance_km
2. **West region underperformers (STR-1012, STR-1007):** Two of the top 5 negative residuals are West region stores. This may indicate that the West region coefficient (+₹19,832) is driven by a subset of high-performing West stores, while a tail of underperformers is pulling down the average
3. **South Mall (STR-1023):** Despite Mall type premium and South region premium, this store significantly underperforms — possible operational issues or location within the mall (lower traffic zone)
4. **Action:** These stores are candidates for operational review and root cause analysis before further investment

---

## Pattern Analysis

### Under-prediction pattern (positive residuals)
- Concentrated in East region (3 of top 5)
- Includes both Mall and Residential types
- These stores have some systematic outperformance the model cannot explain — likely unobserved quality factors

### Over-prediction pattern (negative residuals)
- Concentrated in West region (3 of top 5)
- The model may be overestimating the West region effect for some stores
- High Street type appears vulnerable — STR-1017's -₹175,028 residual is the largest in the dataset

### Does the model systematically over or under-predict certain store types?

Calculating average residuals by store type:
- Residential: residuals are roughly centered (no systematic bias)
- Mall: slight positive skew — model may slightly under-predict Mall stores on average
- Airport: small sample, but residuals reasonably balanced
- High Street: slight negative skew — some High Street stores underperform the model's expectation

### Residual distribution
The residuals appear approximately normally distributed with mean ≈ 0, consistent with OLS assumptions being reasonably met. There is no clear non-linear pattern in the residuals vs fitted values plot, suggesting the linear model is an appropriate functional form for this data.

---

## Limitations of Residual Analysis

- With only 4 time periods per store, residuals may capture time-specific shocks rather than structural store-level issues
- Stores with consistently large residuals (positive or negative) across all 4 months are more diagnostically meaningful than single-month outliers
- Residuals do not tell us *why* stores over/under-perform — only that they do; root cause analysis requires additional investigation
