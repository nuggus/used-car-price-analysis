# What Drives the Price of a Used Car?

**Practical Application II — Used Car Price Analysis**

---

## Summary of Findings

This project analyzes over 370,000 used car listings to identify the key factors that drive pricing in the used car market. The analysis follows the CRISP-DM framework and delivers actionable recommendations for used car dealers looking to optimize their inventory and pricing strategy.

### Key Findings

**What makes a used car worth more:**

1. **Model Year** — Newer vehicles command a 5–7% price premium per additional year. Year is one of the strongest predictors of price across all models.
2. **Low Mileage** — Odometer reading is a direct and powerful negative predictor of price. Low-mileage vehicles sell for significantly more.
3. **Drivetrain** — 4WD and AWD vehicles command a consistent premium over FWD equivalents, particularly for trucks and SUVs.
4. **Vehicle Type** — Pickups and trucks outperform sedans and hatchbacks in median price across all conditions.
5. **Condition** — Excellent and like-new condition vehicles earn meaningful premiums; dealers who recondition before listing gain pricing power.
6. **Manufacturer** — RAM, GMC, and Ford consistently appear in higher-value listings; brands like Honda and Nissan trend lower.

**What has little effect on price:**
- Paint color
- Geographic state (after controlling for other features)
- Minor trim details not captured in this dataset

### Recommendations for Dealers

- Prioritize acquiring **newer, lower-mileage vehicles** — they have the strongest resale value.
- Stock **AWD/4WD trucks and SUVs** in regions with weather or terrain demand.
- Invest in **vehicle reconditioning** to move listings from "good" to "excellent" condition ratings.
- Use **year and mileage as the primary inputs** when setting asking prices; apply percentage-based adjustments rather than flat dollar amounts.
- Be transparent about **title status** — salvage and rebuilt titles materially reduce buyer willingness to pay.

---

## Models Used

Three regression models were trained and evaluated:

| Model | MAE (dollar scale) | R² | Notes |
|---|---|---|---|
| Linear Regression (baseline) | $5,362 | 0.639 | Strong baseline |
| Ridge Regression (L2, cross-validated) | $5,362 | 0.639 | Matches Linear — optimal alpha is near zero on this well-conditioned feature set |
| **Lasso Regression (L1, cross-validated)** | **$5,451** | **0.628** | **Selected — automatic feature selection aids interpretability** |

**Final model: Lasso Regression** — Linear and Ridge perform identically after proper preprocessing (StandardScaler + excluding the high-cardinality `model` column). Lasso is marginally less accurate ($89 higher MAE) but performs implicit feature selection via L1 regularization, zeroing out low-signal features. This makes the coefficient interpretation more trustworthy and directly useful for the dealership audience.

**Evaluation metric: MAE (Mean Absolute Error)** — on average, the model's price predictions are off by ~$5,400. This is directly interpretable by a dealership. MAE is preferred over RMSE because it is robust to extreme prices (collector cars, luxury vehicles) that would otherwise dominate a squared-error metric.

---

## Notebook

👉 [Open the analysis notebook](./used-car-price-analysis.ipynb)

---

## Project Structure

```
practical_application_II_starter/
├── README.md                  ← This file
├── prompt_II.ipynb            ← Main analysis notebook
├── data/
│   └── vehicles.csv           ← Used car listings dataset (426K rows)
└── images/
    ├── crisp.png
    └── kurt.jpeg
```

---

## Next Steps

- Test non-linear models (XGBoost, Random Forest) to capture more complex pricing patterns
- Incorporate geographic/regional pricing adjustments
- Enrich the dataset with trim level, safety ratings, and optional features
- Build an internal pricing calculator tool for dealer use
- Refresh the model quarterly to account for seasonal price shifts
