# FundFirst — Case Study

## Project Summary

FundFirst is an explainable machine-learning proof-of-concept exploring deposit-saving feasibility for prospective first-time buyers across six South-West London boroughs.

Instead of presenting affordability as a single number, the project reframes deposit planning as a three-class decision:

- **Achievable**
- **Stretch**
- **Unfeasible**

The objective was not to predict mortgage approval or real buyer behaviour. Instead, FundFirst asks whether machine learning can reproduce a transparent project-defined feasibility benchmark on later unseen market years while providing evidence about explainability, performance and fairness.

---

## Why This Project?

Housing affordability information is often presented through individual statistics such as house prices, earnings, interest rates and required deposit amounts.

For prospective buyers, however, the practical question is often simpler:

> **How realistic is this saving goal?**

FundFirst explores whether those inputs can be transformed into an interpretable feasibility classification rather than a standalone affordability number.

The project also investigates whether a more complex model meaningfully improves performance and whether model outputs can be accompanied by useful assurance evidence.

---

## Research Questions

The project addresses three questions.

### RQ1 — Predictive Validity

Can machine learning reproduce the project-defined feasibility tiers when evaluated on later unseen years?

### RQ2 — Model Selection

Does additional model complexity improve performance compared with an interpretable linear model?

### RQ3 — Assurance

Can predictions be accompanied by useful evidence about explainability, reproducibility, performance and fairness?

---

## Data

FundFirst combines four official public data sources:

- UK House Price Index
- borough-level workplace earnings
- ONS Household Saving Ratio
- Bank of England Bank Rate

After cleaning and alignment, the final dataset contains:

- **72 borough-year observations**
- **6 South-West London boroughs**
- **2014–2025**

The four model features are:

- Average Price
- Median Annual Pay
- Saving Ratio
- Base Rate

No personal records are used.

---

## Transparent Saving Model

The project does not contain observed labels describing whether a real buyer successfully saved a deposit, FundFirst defines a transparent benchmark called the **Transparent Saving Model (TSM)**.

The TSM assumes:

- deposit target = **10% of average house price**
- monthly saving = **20% of gross monthly income**

The resulting estimated saving period is classified as:

| Tier | Months |
|---|---:|
| Achievable | ≤72 |
| Stretch | 73–108 |
| Unfeasible | >108 |

These thresholds are part of the experimental design and should not be interpreted as universal affordability standards.

---

## Evaluation Design

FundFirst uses a chronological evaluation instead of randomly mixing earlier and later years.

```text
2014–2022
Training
    ↓
2023
Validation
    ↓
2014–2023
Final refit
    ↓
2024–2025
Held-out evaluation
```
