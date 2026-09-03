# Modelling Notebook

The main notebook contains the complete FundFirst machine-learning workflow.

## Notebook

[`fundfirst-analysis.ipynb`](fundfirst-analysis.ipynb)

The notebook covers:

- loading and cleaning the four public datasets
- constructing borough-year observations
- generating Transparent Saving Model (TSM) feasibility labels
- chronological train, validation and test splitting
- Logistic Regression and Random Forest training
- held-out model evaluation
- local and global SHAP explanations
- income-stratified performance auditing
- model persistence and reproducibility checks
- a manual inference example

## Evaluation Design

The project uses a chronological split rather than a random train/test split:

- **2014–2022** — initial training
- **2023** — validation
- **2014–2023** — final model refit
- **2024–2025** — held-out evaluation

This design tests whether patterns learned from earlier market conditions generalise to later unseen years.

## Running the Notebook

Install the project dependencies:

```bash
python -m pip install -r requirements.txt
