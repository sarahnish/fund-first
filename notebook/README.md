# Modelling Notebook

The main notebook contains the complete FundFirst machine-learning workflow, from public-data preparation and feasibility-label generation through model evaluation, SHAP explainability and assurance checks.

## Notebook

[Open `fundfirst-analysis.ipynb` →](fundfirst-analysis.ipynb)

The notebook covers:

- loading and cleaning four official public datasets
- constructing borough-year observations
- generating Transparent Saving Model (TSM) feasibility labels
- chronological train, validation and held-out test splitting
- Logistic Regression and Random Forest training
- held-out model evaluation
- local and global SHAP explanations
- income-stratified performance auditing
- model persistence and reproducibility checks
- a manual inference example

---

## Evaluation Design

FundFirst uses a **chronological split** rather than a random train/test split.

| Stage | Period | Purpose |
|---|---|---|
| **Training** | 2014–2022 | Initial model fitting |
| **Validation** | 2023 | Development and model comparison |
| **Final Refit** | 2014–2023 | Refit before final evaluation |
| **Held-out Test** | 2024–2025 | Evaluation on later unseen years |

This design tests whether patterns learned from earlier market conditions generalise to later unseen years.

The final held-out test contains **12 borough-year observations**.

---

# Data & Sources

FundFirst combines four official public datasets covering housing prices, earnings, household saving behaviour and interest rates.

The final modelling dataset contains **72 borough-year observations** across six South-West London boroughs from **2014 to 2025**.

## Six South-West London Boroughs

The project covers:

- Croydon
- Kingston upon Thames
- Merton
- Richmond upon Thames
- Sutton
- Wandsworth

## Data Sources

| Source | Project Feature | Processing |
|---|---|---|
| [UK House Price Index](https://www.ons.gov.uk/economy/inflationandpriceindices/datasets/ukhousepriceindexmonthlypricestatistics) | `AveragePrice` | Monthly borough prices converted to annual means |
| [Earnings by Workplace, Borough](https://data.london.gov.uk/dataset/earnings-by-workplace-borough-vq846) | `MedianAnnualPay` | Full-time median workplace earnings reshaped to borough-year format |
| [ONS Household Saving Ratio](https://www.ons.gov.uk/economy/grossdomesticproductgdp/timeseries/dgd8/ukea) | `SavingRatio` | Annual UK household saving-ratio context |
| [Bank of England Bank Rate History](https://www.bankofengland.co.uk/boeapps/database/Bank-Rate.asp) | `BaseRate` | Rate-change history converted to time-weighted annual means |

---

## Data Preparation

The notebook:

1. filters the house-price data to the selected boroughs and years
2. aggregates monthly prices into annual borough averages
3. reshapes earnings data into borough-year observations
4. interpolates the suppressed Kingston upon Thames 2018 earnings value between 2017 and 2019
5. converts Bank Rate changes into time-weighted annual values
6. joins all four sources into a single borough-year dataset
7. checks the merged dataset for missing values and duplicates

The resulting modelling dataset contains **72 complete borough-year records**.

---

## Model Features

The machine-learning models use four features:

| Feature | Description |
|---|---|
| `AveragePrice` | Annual mean borough house price |
| `MedianAnnualPay` | Full-time median annual workplace earnings |
| `SavingRatio` | UK household saving ratio |
| `BaseRate` | Time-weighted annual Bank Rate |

---

## Transparent Saving Model

FundFirst uses a project-defined **Transparent Saving Model (TSM)** to generate the target classes.

The TSM assumes:

- a **10% house-price deposit target**
- monthly saving equal to **20% of gross monthly income**

The estimated saving period is then mapped to three feasibility tiers:

| Tier | Saving Period |
|---|---|
| **Achievable** | ≤72 months |
| **Stretch** | 73–108 months |
| **Unfeasible** | >108 months |

The 72-month Achievable threshold was selected through development-period sensitivity analysis to retain a usable three-class structure.

> These labels are project-generated benchmark labels rather than observed buyer outcomes or universal affordability definitions.

---

## Models

Two supervised-learning models are compared.

### Logistic Regression

Used as an interpretable linear benchmark.

The model is trained within a pipeline that standardises the input features before classification.

### Random Forest

A **300-tree Random Forest** is used to test whether additional non-linear model complexity improves performance.

---

## Held-Out Results

| Model | Accuracy | Macro-F1 |
|---|---:|---:|
| **Logistic Regression** | **0.833** | **0.778** |
| Random Forest | 0.583 | 0.444 |

Logistic Regression provided the strongest performance on the **2024–2025 held-out period** while retaining a simpler and more interpretable model structure.

The selected model correctly classified **10 of the 12 held-out observations**.

Class-level recall showed:

- **Achievable: 1.00**
- **Stretch: 1.00**
- **Unfeasible: 0.33**

The Unfeasible class therefore remains the main performance limitation despite the stronger overall accuracy.

[View the full evaluation results →](../results/README.md)

---

## Explainability

SHAP is used to examine both global model behaviour and individual predictions.

The notebook includes:

- global mean absolute SHAP feature contributions
- local SHAP explanations for all **12 held-out predictions**
- feature-level inspection of individual borough-year classifications

Global feature contribution ranked:

1. **Average Price**
2. **Median Annual Pay**
3. **Base Rate**
4. **Saving Ratio**

SHAP is used to explain the fitted model's behaviour and should **not** be interpreted as evidence of causal relationships.

---

## Fairness & Assurance

The notebook also includes an exploratory income-stratified performance audit.

Higher- and lower-income borough groups are compared using:

- overall accuracy
- per-class precision
- predefined investigation thresholds

The audit flagged:

- **33.3 percentage-point accuracy gap**
- **66.7 percentage-point Stretch-precision gap**

These results are treated as **signals for further investigation rather than evidence of discrimination**, given the small held-out sample, aggregate borough-level data and use of income only as a socioeconomic proxy.

The project considers four assurance dimensions:

| Area | Outcome |
|---|---|
| **Explainability** | Supported |
| **Reproducibility** | Supported |
| **Performance** | Partial |
| **Fairness** | Flag raised |

---

## Reproducibility

The notebook includes several measures to support reproducibility:

- project-relative file paths
- fixed model random states
- chronological evaluation
- saved model metadata
- serialisation of the selected Logistic Regression pipeline
- a reload check confirming that the saved pipeline reproduces the original prediction
- generation of evaluation tables and figures from the executed workflow

No GPU is required.

---

## Running the Notebook

From the repository root, install the project dependencies:

```bash
python -m pip install -r requirements.txt
```

Then open:

```text
notebooks/fundfirst-analysis.ipynb
```

Run the cells in order from top to bottom.

---

## Important Caveat

FundFirst evaluates agreement with **project-generated feasibility labels**.

It does not predict:

- mortgage approval
- observed home purchases
- individual creditworthiness
- individual affordability
- future housing-market outcomes

The project uses aggregate public statistics only and contains **no personal records**.

FundFirst is an educational research prototype and is not financial, mortgage, lending or investment advice.

---

## Related Resources

- [Project Overview](../README.md)
- [Full Case Study](../docs/case-study.md)
- [Evaluation Results](../results/README.md)
- [Data & Sources](../data/README.md)

