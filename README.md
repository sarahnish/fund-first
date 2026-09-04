<p align="center">
  <img src="logo/logo.png" alt="FundFirst logo" width="220"/>
</p> 

<h1 align="center">FundFirst — Explainable Deposit Feasibility Classification</h1>

<p align="center">
  <b>An explainable machine-learning prototype that classifies first-time buyer deposit-saving scenarios across six South-West London boroughs as Achievable, Stretch or Unfeasible.</b>
</p>

## Quick Links

<p align="center">
  <a href="docs/case-study.md">Full Case Study</a> •
  <a href="notebooks/fundfirst-analysis.ipynb">Modelling Notebook</a> •
  <a href="results/README.md">Evaluation Results</a> •
  <a href="data/README.md">Data & Sources</a> •
  <a href="https://github.com/sarahnish/portfolio">Project Portfolio</a>
</p>

---

## Overview

FundFirst is an academic proof-of-concept that explores whether deposit-saving scenarios for prospective first-time buyers in six South-West London boroughs can be classified as **Achievable**, **Stretch** or **Unfeasible**.

The project combines public housing, earnings, household-saving and interest-rate data; defines a transparent Time–Savings–Market (TSM) labelling rule; compares Logistic Regression with Random Forest; and uses SHAP plus an income-stratified performance audit to examine the selected model.

> FundFirst measures agreement with project-generated feasibility labels. It does not predict mortgage approval, observed home purchases or individual affordability, and it is not financial advice.

---

## At a Glance

| Dataset | Geography | Period | Models | Best Test Accuracy |
|---|---|---|---|---:|
| **72 borough-year observations** | **6 South-West London boroughs** | **2014–2025** | **Logistic Regression · Random Forest** | **83.3%** |

## Live Application

FundFirst is deployed as an end-to-end machine-learning application with a separate frontend and Python inference API.

- [Launch FundFirst](https://fundfirst-dream-planner.lovable.app)
- [View the FastAPI backend](https://github.com/sarahnish/FundFirst_backend)

### Deployment Architecture

```text
User enters a scenario
        ↓
Lovable frontend
        ↓
HTTPS POST /predict
        ↓
FastAPI backend
        ↓
Saved scikit-learn pipeline
StandardScaler + Logistic Regression
        ↓
Prediction + class probabilities
        ↓
JSON response
        ↓
Lovable displays the result
```

### How It Works

**Public housing + economic data → transparent feasibility labels → chronological ML evaluation → SHAP explanations → fairness checks**

FundFirst classifies deposit-saving scenarios into three feasibility tiers:

- **Achievable** — up to 72 months
- **Stretch** — 73–108 months
- **Unfeasible** — over 108 months

---

## Key Results

The final dataset contains **72 borough-year observations** for Croydon, Kingston upon Thames, Merton, Richmond upon Thames, Sutton and Wandsworth from **2014–2025**.

Models were evaluated chronologically: 2014–2022 for initial training, 2023 as a validation checkpoint, and 2024–2025 as the held-out test period after refitting on 2014–2023.

<p align="center">
  <img src="results/held_out_model_performance.png" alt="Held-out model performance" width="800"/>
</p>

| Model | Held-out Accuracy | Held-out Macro-F1 |
|---|---:|---:|
| **Logistic Regression** | **0.833** | **0.778** |
| Random Forest | 0.583 | 0.444 |

Logistic Regression was selected because it performed better on the held-out period while retaining a simpler, more interpretable model structure.

On the 12-row test set, it correctly classified all Achievable and Stretch observations, but recall for Unfeasible cases was **0.33**.

### Assurance Evidence

FundFirst evaluated four assurance dimensions:

| Area | Outcome |
|---|---|
| **Explainability** | Supported |
| **Reproducibility** | Supported |
| **Performance** | Partial |
| **Fairness** | Flag raised |

The income-stratified audit flagged a **33.3 percentage-point accuracy gap** and a **66.7 percentage-point Stretch-precision gap** between borough groups.

These results are treated as **investigation flags rather than evidence of unfair treatment**, given the small 12-row test set, aggregate borough-level data and use of income as a socioeconomic proxy.

The strongest assurance evidence came from transparency and reproducibility, while fairness and robustness remained open risks.

[View full evaluation results →](results/README.md)

---

## Method

1. Clean and combine four official public datasets at borough-year level.
2. Calculate a 10% deposit target and assume monthly saving equal to 20% of gross income.
3. Assign TSM labels:
   - **Achievable** at ≤72 months
   - **Stretch** at >72 and ≤108 months
   - **Unfeasible** at >108 months
4. Compare standardised Logistic Regression with a 300-tree Random Forest using chronological splits.
5. Explain held-out predictions with local and global SHAP values.
6. Audit accuracy and class precision across income-stratified borough groups.

The 72-month Achievable threshold was calibrated using the 2014–2022 development period to retain a usable three-class structure. It is specific to this dataset and is not a universal affordability definition.

---

## Data

| Source | Project Feature |
|---|---|
| [UK House Price Index](https://www.ons.gov.uk/economy/inflationandpriceindices/datasets/ukhousepriceindexmonthlypricestatistics) | Annual mean borough house price |
| [Earnings by Workplace, Borough](https://data.london.gov.uk/dataset/earnings-by-workplace-borough-vq846) | Full-time median annual pay |
| [ONS Household Saving Ratio](https://www.ons.gov.uk/economy/grossdomesticproductgdp/timeseries/dgd8/ukea) | National annual saving-ratio context |
| [Bank of England Bank Rate history](https://www.bankofengland.co.uk/boeapps/database/Bank-Rate.asp) | Time-weighted annual Bank Rate |

The suppressed Kingston upon Thames earnings value for 2018 is linearly interpolated between 2017 and 2019.

The resulting dataset contains aggregate statistics only and no personal records.

[View full data notes →](data/README.md)

---

## Limitations

- Labels are generated by a deterministic rule rather than observed buyer outcomes.
- The dataset is small and covers only six South-West London boroughs.
- National saving-ratio and Bank Rate features repeat across boroughs within each year.
- The held-out test contains only 12 observations, so performance and audit gaps are prototype-level evidence.
- SHAP describes model contributions, not causal effects.
- Income is used only as a socioeconomic proxy in the fairness analysis.
- The project is an educational research prototype, not a production decision system.

---

## Tech Stack

| Layer | Technologies |
|---|---|
| **Language** | Python 3.12 |
| **Machine Learning** | scikit-learn |
| **Data Processing** | pandas, NumPy |
| **Explainability** | SHAP |
| **API** | FastAPI, Pydantic, Uvicorn |
| **Model Persistence** | joblib |
| **Frontend** | Lovable |
| **Deployment** | Render |
| **Development** | Jupyter, Git, GitHub |
---

## Repository Guide

- `notebooks/fundfirst-analysis.ipynb` — executed end-to-end analysis, model evaluation, SHAP explanations and audit
- `data_raw/` — original public source files used by the notebook
- `data_clean/` — generated annual and labelled datasets
- `results/` — model-performance and explainability figures
- `outputs/` — generated evaluation tables
- `models/` — selected fitted pipeline and model metadata
- `model_card.md` — model documentation
- `requirements.txt` — Python dependencies

To reproduce the analysis, create a Python 3.12 environment, install `requirements.txt`, open the notebook from the repository root and run all cells in order. No GPU is required.

---

## Explore Further

| Resource | What You'll Find |
|---|---|
| **[Full Case Study](docs/case-study.md)** | Problem framing, methodology, evaluation, assurance and limitations |
| **[Modelling Notebook](notebooks/fundfirst-analysis.ipynb)** | Complete data preparation, modelling, SHAP and audit workflow |
| **[Evaluation Results](results/README.md)** | Confusion matrices, SHAP results and supporting figures |
| **[Data Notes](data/README.md)** | Data sources, transformations and feature definitions |

---

## Disclaimer

FundFirst is an educational research prototype and does not provide financial, mortgage, lending or investment advice.
