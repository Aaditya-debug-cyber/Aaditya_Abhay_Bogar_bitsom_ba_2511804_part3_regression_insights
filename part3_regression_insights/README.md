# Part 3: Regression-Based Business Insights & Model Interpretation

**Student:** Aaditya Abhay Bogar  
**Student ID:** bitsom_ba_2511804  
**Repository:** `aadityaabhaybogar_bitsom_ba_2511804_part3_regression_insights`

---

## Business Problem Summary

A retail chain wants to understand what factors drive monthly sales performance across stores. Leadership is considering actions such as increasing marketing spend, improving inventory, changing discounts, reallocating staff, and prioritizing store types or regions. This regression analysis identifies the strongest predictors of monthly sales and translates statistical findings into business recommendations.

---

## Dataset Description

| Attribute | Value |
|---|---|
| File | `data/business_regression_data.xlsx` |
| Records | 320 (80 stores × 4 months: Jan–Apr 2025) |
| Columns | 14 |
| Missing | competitor_distance_km (6), customer_rating (8) |

**Dependent variable:** `monthly_sales`

**Independent variables:**

| Variable | Type | Role |
|---|---|---|
| marketing_spend | Numerical | Independent — marketing investment |
| footfall | Numerical | Independent — store traffic (strongest predictor) |
| avg_discount_pct | Numerical | Independent — discounting strategy |
| staff_count | Numerical | Independent — human resource allocation |
| inventory_availability_pct | Numerical | Independent — supply chain quality |
| competitor_distance_km | Numerical | Independent — competitive position |
| holiday_flag | Binary | Independent — seasonal control |
| customer_rating | Numerical | Independent — service quality |
| region | Categorical → Dummies | Independent — geographic effect |
| store_type | Categorical → Dummies | Independent — format effect |
| store_id, month | Identifier/date | Not used as predictors |
| monthly_profit | Numerical | Not used (downstream of sales; would cause leakage) |

---

## Regression Approach

Three simple linear regression models and one multiple regression model were built using Ordinary Least Squares (OLS).

| Model | Variables | R² |
|---|---|---|
| Model 1 | footfall | 0.7363 |
| Model 2 | marketing_spend | 0.1672 |
| Model 3 | staff_count | 0.6523 |
| **Model 4 (Final)** | footfall + marketing_spend + staff_count + avg_discount_pct + inventory_availability_pct + holiday_flag + region dummies + store_type dummies | **0.8337** |

---

## Dummy Variable Approach

Two categorical variables were converted to dummy variables:

**region** — Reference category: **East**  
Dummies created: region_North, region_South, region_West

**store_type** — Reference category: **Residential**  
Dummies created: type_HighStreet, type_Airport, type_Mall

The reference category is not included as a dummy to avoid perfect multicollinearity (dummy variable trap). Coefficients for other categories represent the difference vs the reference category.

---

## Model Comparison Summary

| Model | R² | Adj R² | RMSE | Best For |
|---|---|---|---|---|
| M1: footfall | 0.7363 | 0.7355 | ₹54,010 | Understanding footfall impact |
| M2: marketing_spend | 0.1672 | 0.1646 | ₹94,850 | Weak standalone predictor |
| M3: staff_count | 0.6523 | 0.6512 | ₹61,720 | Collinear with footfall |
| **M4: Multiple** | **0.8337** | **0.8272** | **₹43,180** | **Best overall — final model** |

---

## Final Model Selected

**Model 4 — Multiple Regression** was selected as the final model because it has the highest explanatory power (R² = 83.4%), lowest RMSE (₹43,180), and includes actionable predictors including dummy variables for store type and region.

**Key equation:**
```
monthly_sales = 89,737
              + 27.99 × footfall
              + 1.15  × marketing_spend
              + 3,026 × staff_count
              − 49,804 × avg_discount_pct        [NOT significant]
              + 3,033 × inventory_availability_pct
              + 16,167 × holiday_flag
              + 19,692 × region_South
              + 19,832 × region_West
              + 17,888 × type_HighStreet
              + 39,019 × type_Airport
              + 29,745 × type_Mall
```

---

## Business Recommendation

1. **Footfall is the #1 driver** — invest in footfall-driving initiatives (events, local marketing, loyalty programs)
2. **Fix inventory gaps** — every 1% improvement in inventory availability = ~₹3,033 more in monthly sales
3. **Prioritize Airport and Mall formats** for new store openings — ₹39,019 and ₹29,745 monthly premium vs Residential
4. **Plan for holiday months** — stock up and staff up, consistent +₹16,167 seasonal lift
5. **Do not use discounting as a primary lever** — avg_discount_pct is not statistically significant (p=0.17)
6. **Audit STR-1017** (West, High Street) — largest negative residual (-₹175,028), severely underperforming

---

## Assumptions and Limitations

- OLS assumes independent observations — violated here since the same store appears 4 times (panel structure). Fixed effects model would be more rigorous
- Missing values for competitor_distance_km and customer_rating filled with median/mean
- monthly_profit excluded from predictors (downstream outcome, causes data leakage)
- Correlation does not imply causation — all findings should be validated with controlled experiments before large-scale action
- Short time window (4 months) limits seasonal analysis

---

## Screenshots Included

| File | Contents |
|---|---|
| `screenshots/simple_regression_output.png` | Scatter plots + regression lines for Model 1 (footfall) and Model 2 (marketing_spend) |
| `screenshots/multiple_regression_output.png` | Full coefficient table for Model 4 with significance flags |
| `screenshots/residuals_preview.png` | Residuals vs fitted, distribution histogram, top over/under predictions |
| `screenshots/model_comparison_preview.png` | R² and RMSE bar charts comparing all 4 models |

---

## Repository Structure

```
part3_regression_insights/
├── data/
│   └── business_regression_data.xlsx
├── analysis/
│   ├── regression_workbook.xlsx   # 9-sheet workbook: raw data, cleaned data, dummies, 3 simple regressions, multiple regression, predictions/residuals, model comparison
│   ├── model_comparison.md        # Full model comparison with business context
│   └── residual_analysis.md       # Top/bottom residuals with business interpretation
├── outputs/
│   ├── regression_summary.xlsx    # Clean comparison table (3 sheets)
│   ├── final_recommendation.md    # Data-backed business recommendation
│   └── model_equations.md         # All equations with coefficient explanations
├── screenshots/
│   ├── simple_regression_output.png
│   ├── multiple_regression_output.png
│   ├── residuals_preview.png
│   └── model_comparison_preview.png
└── README.md
```
