# Final Business Recommendation
**To:** Retail Chain Leadership  
**From:** Aaditya Abhay Bogar, Business Analyst (bitsom_ba_2511804)  
**Date:** 2026-06-19  
**Subject:** Regression-Based Insights into Monthly Sales Drivers

---

## 1. Which Factors Are Most Strongly Associated with Monthly Sales?

Based on the multiple regression model (R² = 83.4%, 320 observations, 80 stores across 4 months), the following factors show the strongest and most statistically reliable associations with monthly sales:

| Factor | Coefficient | Significance | Strength |
|---|---|---|---|
| Footfall (monthly visitors) | +₹27.99 per visitor | p < 0.001 *** | Very Strong |
| Inventory Availability (%) | +₹3,033 per 1% improvement | p < 0.001 *** | Strong |
| Marketing Spend | +₹1.15 per ₹1 spent | p < 0.001 *** | Moderate |
| Airport store type (vs Residential) | +₹39,019/month | p < 0.001 *** | Strong |
| Mall store type (vs Residential) | +₹29,745/month | p < 0.001 *** | Strong |
| High Street type (vs Residential) | +₹17,888/month | p < 0.01 ** | Moderate |
| West region (vs East) | +₹19,832/month | p < 0.01 ** | Moderate |
| South region (vs East) | +₹19,692/month | p < 0.01 ** | Moderate |
| Holiday months | +₹16,167/month | p < 0.05 * | Moderate |
| Staff count | +₹3,026 per person | p < 0.05 * | Weak (collinear) |

---

## 2. Which Variables Should Leadership Focus On?

### Priority 1 — Footfall (Most Critical)

Footfall is by far the dominant driver of monthly sales. It alone explains 73.6% of sales variation, and retains a coefficient of +₹27.99 even after controlling for all other factors. For every 1,000 additional store visitors per month, sales rise by approximately ₹27,990.

**Leadership action:** Drive footfall through location selection, local marketing events, loyalty programs, and promotional calendars. Prioritize footfall-generating investments over all others.

### Priority 2 — Inventory Availability

Every 1-percentage-point improvement in inventory availability is associated with ₹3,033 more in monthly sales (p < 0.001). Across the dataset, inventory availability ranges from 71% to 99% — an improvement from 71% to 90% would be associated with ~₹57,600 more in monthly sales per store. This is one of the most operationally controllable levers.

**Leadership action:** Audit and improve supply chain reliability for stores with inventory availability below 85%. Prioritize high-footfall stores first.

### Priority 3 — Store Type Strategy

Airport stores generate ₹39,019 more per month than Residential stores with otherwise identical characteristics. Mall stores generate ₹29,745 more. These are structural advantages tied to location.

**Leadership action:** Prioritize Airport and Mall site selection for new store openings. When existing Residential stores are up for lease renewal, evaluate whether relocation to High Street or Mall formats is feasible.

### Priority 4 — Marketing Spend

After controlling for footfall and other factors, each ₹1 in marketing spend is associated with ₹1.15 in monthly sales (marginal ROI). While this is a positive signal, it is lower than the ₹2.13 estimate from the simple model — the difference is absorbed by footfall (marketing drives footfall which drives sales).

**Leadership action:** Marketing ROI should be evaluated at the footfall level, not just the sales level. Track whether marketing spend translates to footfall increases in each store.

### Priority 5 — Seasonal Planning (Holiday Flag)

Holiday months add ~₹16,167 in monthly sales on average. This is a predictable pattern that should inform inventory pre-stocking and staffing plans.

**Leadership action:** Increase inventory levels and staff coverage for stores entering holiday months. Use this estimate for budgeting and demand forecasting.

---

## 3. Which Variables Should NOT Be Over-interpreted?

### avg_discount_pct — NOT Significant (p = 0.17)

The discount percentage coefficient is not statistically significant. This does not mean discounting has no effect — it means the regression model cannot reliably distinguish the effect of discounting from random noise in this dataset. Discount strategies vary widely across stores and may interact with product category in ways this model does not capture.

**Leadership action:** Do not use this regression to justify increasing or decreasing discounts. A dedicated pricing experiment (A/B test on discount levels) would be needed to assess the causal effect of discounting on sales.

### staff_count — Use With Caution

Staff count is statistically significant (p = 0.015) but its coefficient drops from ₹16,984 (simple model) to ₹3,026 (multiple model) — an 82% reduction. This is a sign of collinearity with footfall (larger stores are both more staffed and more visited). The coefficient does not cleanly represent the return to hiring one additional staff member.

**Leadership action:** Do not use this regression to set staffing levels. Staffing should be driven by footfall forecasts and store-specific operational standards, not this coefficient.

### region_North — Not Different from East

The North region coefficient (+₹7,711) is not statistically significant (p = 0.27). North and East stores perform similarly on average after controlling for other factors.

---

## 4. Recommended Business Actions (Priority Order)

1. **Footfall strategy first:** Invest in footfall-driving initiatives (events, local marketing, loyalty programs) for underperforming stores. The regression shows every 1,000 additional visitors = ~₹27,990 in sales.
2. **Fix inventory gaps:** Stores below 85% inventory availability should be prioritized for supply chain improvement. Estimated payoff: ~₹3,033 per 1% improvement.
3. **Optimize store portfolio:** Plan new openings in Airport and Mall formats where feasible. These formats command a structural ₹29,000–₹39,000/month premium.
4. **Holiday preparation:** Pre-stock and increase staffing for holiday months — the model shows a reliable +₹16,167 seasonal lift.
5. **Investigate underperforming stores:** Stores with large negative residuals (STR-1017 West High Street, STR-1012 West Residential, STR-1023 South Mall) are significantly underperforming their predicted sales — conduct operational audits.
6. **Learn from outperformers:** Stores with large positive residuals (STR-1028, STR-1030, STR-1073) outperform predictions consistently — document their practices and assess replicability.

---

## 5. Risks and Limitations Leadership Should Keep in Mind

### Regression shows association, not causation

This is the most important limitation. The regression identifies *correlations* between predictors and monthly sales — it does not prove that changing one variable will cause sales to change by the modeled amount. For example:

- Stores with high footfall also tend to be in prime locations, have more experienced managers, and carry better product ranges — the model cannot separate the individual contributions of these unmeasured factors
- Increasing marketing spend in a store with low footfall may not generate the same ₹1.15-per-₹1 return seen in the aggregate data, because the underlying reason for low footfall (poor location, poor store format) is not addressed

### Panel data limitation

The 320 observations come from 80 stores observed 4 times each. Standard OLS regression assumes all observations are independent. Because the same store appears in the data 4 times, there is store-level clustering that OLS does not account for. This may cause the model to overstate statistical significance (p-values may be slightly optimistic). A store-level fixed effects model or clustered standard errors would be more statistically rigorous.

### Short time window

Four months of data (January–April 2025) is a very short window. Seasonal patterns, long-term trends, and the effect of one-off events cannot be reliably estimated. The holiday_flag coefficient is based on 55 holiday observations — a reasonable sample but not definitive.

### Missing data imputation

6 observations for competitor_distance_km and 8 for customer_rating were missing and filled with median/mean values. This introduces a small amount of noise into the model but is unlikely to materially change conclusions.

### Model fit is good but not perfect

R² = 83.4% means ~16.6% of sales variation is unexplained by the model. This unexplained variation may include store-specific management quality, product mix, local competition, or external economic shocks — factors not in the dataset.

---

## 6. Summary

The regression analysis provides strong evidence that **footfall, inventory availability, and store type are the most actionable and statistically reliable drivers of monthly sales**. Marketing spend is a positive but lower-priority lever. Discounting is not a reliable lever based on this model. Leadership should prioritize footfall-driving investments and inventory reliability, and use store type and regional insights to guide portfolio decisions. All actions should be validated with controlled experiments before large-scale resource reallocation.
