<p align="center">
  <img src="logo/fundfirst-logo.png" alt="FundFirst logo" width="220"/>
</p>

<h1 align="center">FundFirst — Explainable Deposit Feasibility Classification</h1>

<p align="center">
  <b>An explainable machine-learning prototype that classifies first-time buyer deposit-saving scenarios across six South-West London boroughs as Achievable, Stretch or Unfeasible.</b>
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

## At a Glance

| Dataset | Geography | Period | Models | Best Test Accuracy |
|---|---|---|---|---:|
| **72 borough-year observations** | **6 South-West London boroughs** | **2014–2025** | **Logistic Regression · Random Forest** | **83.3%** |

### How It Works

**Public housing + economic data → transparent feasibility labels → chronological ML evaluation → SHAP explanations → fairness checks**

FundFirst classifies deposit-saving scenarios into three feasibility tiers:

- **Achievable** — up to 72 months
- **Stretch** — 73–108 months
- **Unfeasible** — over 108 months

The project uses four features:

| Feature | Meaning |
|---|---|
| **Average Price** | Borough-level average house price |
| **Median Annual Pay** | Full-time median workplace earnings |
| **Saving Ratio** | UK household saving ratio |
| **Bank Rate** | Annualised Bank of England Bank Rate |

### Key Results

**83.3% accuracy** · **77.8% macro-F1** · **10/12 held-out cases correctly classified** · **12/12 predictions explained with SHAP**

---

## Tech Stack

| Layer | Technologies |
|---|---|
| **Language** | Python 3.12 |
| **Machine Learning** | scikit-learn |
| **Data Processing** | pandas, NumPy |
| **Explainability** | SHAP |
| **Evaluation** | scikit-learn metrics |
| **Visualisation** | Matplotlib |
| **Model Persistence** | joblib |
| **Development** | Jupyter, Git, GitHub |

---

## The Problem

For many prospective first-time buyers, housing affordability information is available as individual numbers — house prices, earnings, deposit targets and interest rates — but these figures do not necessarily provide a clear indication of whether a saving goal is realistically achievable.

FundFirst reframes deposit planning as a **three-class feasibility problem**.

> **Core Research Question:**  
> Can machine learning reproduce transparent deposit-feasibility tiers on later unseen market years while still providing interpretable and auditable evidence?

---

## Overview

FundFirst combines four public economic and housing datasets to create a borough-year dataset covering **2014–2025** across:

- Croydon
- Kingston upon Thames
- Merton
- Richmond upon Thames
- Sutton
- Wandsworth

A Transparent Saving Model (TSM) generates project-specific feasibility labels based on a **10% deposit benchmark** and an assumed **20% monthly gross-income saving rate**. 

The resulting labels are then used to compare:

1. **Logistic Regression**
2. **Random Forest**

using a chronological evaluation rather than a random train/test split.

---

## Approach

FundFirst follows a five-stage pipeline:

1. **Collect** — house prices, earnings, saving ratio and Bank Rate
2. **Clean & align** — convert each source into borough-year observations
3. **Create labels** — generate Achievable, Stretch and Unfeasible classes using the TSM
4. **Evaluate models** — train on earlier years and test on later unseen years
5. **Explain & audit** — SHAP explanations, class-level metrics and fairness checks

The project uses **2014–2022 for training**, **2023 for validation**, and **2024–2025 as the held-out test period**.

---

## Key Findings

<p align="center">
  <img src="results/held_out_model_performance.png" alt="Held-out model performance" width="800"/>
</p>

- **Logistic Regression performed best** on the 2024–2025 held-out test set, achieving **83.3% accuracy** and **77.8% macro-F1**.
- **Random Forest underperformed**, reaching **58.3% accuracy** and **44.4% macro-F1**.
- The selected model performed strongly on **Achievable** and **Stretch** cases, but **Unfeasible recall was only 0.33**.
- Global SHAP analysis showed that **Average Price** and **Median Annual Pay** contributed most strongly to model predictions.

[View full evaluation results →](results/README.md)

## Final Design

```text
Public Data Sources
        ↓
Cleaning + Borough-Year Alignment
        ↓
Transparent Saving Model
        ↓
Achievable / Stretch / Unfeasible Labels
        ↓
Chronological Model Evaluation
        ↓
Logistic Regression
        ↓
SHAP Explanation
        ↓
Fairness + Assurance Checks
        ↓
Human Interpretation
```

## Explore Further

- [Notebook](notebooks/fundfirst-analysis.ipynb)
- [Case Study](docs/case-study.md)
- [Results](results/)
- [Data Notes](data/README.md)

## Disclaimer

FundFirst is an educational research prototype and does not provide financial, mortgage, lending or investment advice.
