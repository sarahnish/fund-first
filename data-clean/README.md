# Clean Data

This folder contains the processed datasets generated from the raw public sources and used during FundFirst modelling and evaluation. The cleaned data is kept separate from the original source files so the transformation pipeline remains reproducible.

## Contents

The files in this folder contain the annualised and merged borough-year data produced by the modelling notebook.

Typical outputs include:

- cleaned annual house-price data
- reshaped borough-level earnings data
- annual household saving-ratio data
- annual time-weighted Bank Rate data
- merged borough-year feature data
- labelled modelling data used for machine-learning experiments

## Final Modelling Dataset

The final modelling dataset contains:

- **72 borough-year observations**
- **6 South-West London boroughs**
- **2014–2025**
- **4 model features**
- **3 feasibility classes**

### Features

| Feature | Description |
|---|---|
| `AveragePrice` | Annual mean borough house price |
| `MedianAnnualPay` | Full-time median annual workplace earnings |
| `SavingRatio` | UK household saving ratio |
| `BaseRate` | Time-weighted annual Bank Rate |

### Target

The target variable is generated using FundFirst's project-defined **Transparent Saving Model (TSM)**.

| Class | Estimated Saving Period |
|---|---|
| **Achievable** | ≤72 months |
| **Stretch** | 73–108 months |
| **Unfeasible** | >108 months |

The labels are project-generated feasibility benchmarks rather than observed mortgage approvals or individual buyer outcomes.

## Processing Notes

The cleaned datasets are generated from files stored in:

[`../data_raw/`](../data_raw/)

Key preparation steps include:

1. filtering source data to the selected boroughs and 2014–2025 period
2. converting monthly house-price observations into annual borough means
3. reshaping workplace earnings into borough-year format
4. interpolating the suppressed Kingston upon Thames 2018 earnings value between 2017 and 2019
5. converting Bank Rate change history into time-weighted annual values
6. joining the four data sources by borough and year
7. checking for missing values and duplicate borough-year records
8. generating the TSM feasibility labels

## Data Lineage

```text
data_raw/
    ↓
Cleaning and annualisation
    ↓
Source-specific cleaned datasets
    ↓
Borough-year merge
    ↓
Transparent Saving Model labels
    ↓
Final modelling dataset
    ↓
Model training and evaluation
```
