# Raw Data

This folder contains the original public source files used to build the FundFirst modelling dataset. The raw data is kept separate from cleaned and generated files so the data-preparation process remains traceable and reproducible.

## Sources

The project uses four public datasets:

| Source | Used For |
|---|---|
| UK House Price Index | Borough-level property prices |
| Earnings by Workplace, Borough | Median annual workplace earnings |
| ONS Household Saving Ratio | Household saving behaviour |
| Bank of England Bank Rate History | Interest-rate context |

## Processing

These source files are not used directly for model training.

The modelling notebook:

1. loads the raw source files
2. filters the required years and boroughs
3. converts monthly observations to annual values where required
4. reshapes and cleans the earnings data
5. converts Bank Rate changes into annual time-weighted values
6. joins the four sources into borough-year observations
7. generates the final labelled modelling dataset

Processed datasets are stored separately in:

[`../data_clean/`](../data_clean/)

## Note

The project uses aggregate public statistics only and contains no personal records.
