# Online Retail II: End-to-End Data Analytics Project

> Turning more than one million retail transactions into a clean analytical dataset, clear business insights, predictive models, and an interactive Power BI dashboard.

## Overview

This project analyzes the [UCI Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii) dataset: two years of transactions from a UK-based, non-store online gift retailer. The original data covers **01 December 2009 to 09 December 2011** and contains over one million product-line records.

The work moves through the complete analytics lifecycle:

- loading and merging two Excel sheets;
- cleaning and validating noisy, mixed-format retail data;
- engineering invoice-, customer-, product-, and time-based features;
- exploring revenue, seasonality, geography, customers, and cancellations;
- comparing classification and regression models;
- generating plain-language findings and recommendations;
- preparing fact tables and measures for a Power BI dashboard.

The final deliverables are designed to be useful to both technical and non-technical audiences: notebooks and exports explain *how* the analysis was done, while the Power BI report provides an interactive view of *what happened*.

## Key Results

| Metric | Result |
|---|---:|
| Clean revenue | £19.41M |
| Invoices | 39,521 |
| Registered customers | 5,853 |
| Average invoice | £491.22 |
| Median invoice | £301.34 |
| Cleaned product lines retained | 94.0% |

- **Growth:** Revenue was £8.605M in the comparable months of 2010 versus £8.787M in 2011, a 2.1% increase.
- **Seasonality:** November is the strongest complete month, with £1.453M in revenue during 2011.
- **Geography:** The UK contributes approximately 85% of revenue. EIRE, the Netherlands, and Germany are the leading export markets.
- **Customer concentration:** The top 10% of registered customers generate 63% of registered-customer revenue.
- **Timing:** Thursday is the strongest trading day and 12:00 is the strongest hour.
- **Cancellations:** 1.8% of product lines are cancelled, representing approximately 2.4% of net revenue.
- **Top products:** *Regency Cakestand 3 Tier*, *White Hanging Heart T-Light Holder*, and *Party Bunting* are the leading products by revenue.

## Data and Cleaning

The source workbook has two sheets with identical columns. They were merged into one continuous time series and cached as `online_retail_combined.csv` to reduce repeated Excel loading time.

The cleaning process handled the following problems:

| Problem | What was found | Fix applied |
|---|---|---|
| Sheet overlap | The 2009-2010 and 2010-2011 sheets overlap in December 2010 | Exact duplicates were removed |
| Repeated records | 34,335 duplicate lines appeared | Duplicate rows were deleted |
| Cancellations | Credit and cancelled invoices can distort sales totals | They were moved to a separate cancellation table |
| Adjustments | Non-sale rows had invalid quantity or price values | Quantity- and price-invalid lines were removed |
| Non-product codes | Entries such as postage, fees, samples, adjustments, and bank charges appeared in the product column | They were excluded from sales and product analysis |
| Missing customer IDs | Guest checkouts would otherwise be lost | Rows were retained and guest checkouts were explicitly flagged |
| Fully reversed giant orders | Some very large orders had matching full cancellations | The sale and matching cancellation were removed as likely data-entry errors |
| Legitimate wholesale demand | Large quantities are common in this customer base | Outliers were flagged and capped, rather than deleting entire rows |

The exact cleaning stages are recorded in `outputs/cleaning_log.csv`. Final clean sales retain **1,003,687 product lines**.

## Feature Engineering

The line-level dataset was converted into an invoice-level analytical table containing:

- invoice value and quantity;
- number of product lines;
- unique products;
- invoice date, month, weekday, and hour;
- country;
- registered-customer versus guest status;
- high-value invoice label;
- cancellation and revenue measures.

This structure supports monthly trends, customer concentration, product performance, geographic analysis, and predictive modeling.

## Exploratory Analysis

The notebook explores:

- monthly revenue, average order value, and units sold;
- complete-year and partial-month effects;
- country-level and product-level revenue;
- invoice and quantity distributions;
- quantity, price, line count, and customer value relationships;
- weekday and hourly purchasing patterns;
- cancellation frequency and cancellation value.

High-resolution charts are exported to `outputs/figures/`. The analysis deliberately notes the partial December 2011 period so that the apparent decline at the end of the series is not mistaken for a real year-over-year fall.

## Predictive Modeling

Two prediction tasks were created:

### High-value invoice classification

An invoice is classified as high value when it is above the median invoice value of **£301.34**.

The project compared:

- Logistic Regression;
- Decision Tree;
- Random Forest;
- Support Vector Machine (RBF).

The best model was a **Decision Tree**:

| Metric | Test result |
|---|---:|
| Accuracy | 83.87% |
| Precision | 82.85% |
| Recall | 85.43% |
| F1 | 84.12% |
| ROC-AUC | 92.08% |

The model uses order size and context, not unit price, and is compared against a 50% majority baseline. The most influential inputs were total quantity, number of product lines, and hour of purchase.

### Invoice value regression

Log-linear regression was used to predict the log of invoice value:

- **R2:** 0.802
- **MAE:** £181.65
- **MAE (log scale):** 0.385
- **RMSE (log scale):** 0.529

The model explains approximately 80% of the variation in log invoice value.

All model metrics are available in `outputs/model_results.csv` and `outputs/linear_regression_metrics.csv`.

## AI-Assisted Insights

The notebook generates a plain-language summary and recommendations. It highlights:

1. Revenue growth and the need to invest in effective product and customer segments.
2. Autumn demand planning for September through November.
3. Diversification opportunities outside the UK.
4. Retention strategies for high-value customers.
5. Staffing and promotion scheduling around Thursday and noon.
6. Review of repeated cancellations and the products or customers involved.

The generated report is stored at `outputs/ai_insights.md`.

## Power BI Dashboard

The project includes both a PBIX report and the decomposed report/model project files.

Power BI exports include:

- `fact_sales_lines.csv`
- `fact_invoices.csv`
- `fact_cancellations.csv`
- `summary_monthly.csv`
- `kpis.csv`

The report package is organized under:

- `online retail(power bi).pbix`
- `online retail(power bi).Report/`
- `online retail(power bi).SemanticModel/`

It contains cleaned sales facts, monthly summaries, cancellation facts, model outputs, and dashboard-ready KPI values.

## Project Files

```text
.
|-- AI_Assisted_Data_Analytics_Final_Project.ipynb
|-- online retail(power bi).pbix
|-- online retail(power bi).Report/
|-- online retail(power bi).SemanticModel/
|-- online_retail_II.xlsx
|-- online_retail_combined.csv
|-- outputs/
|   |-- ai_insights.md
|   |-- cleaning_log.csv
|   |-- linear_regression_metrics.csv
|   |-- model_results.csv
|   |-- online_retail_clean_final.csv
|   |-- figures/
|   `-- powerbi/
`-- staging/
    `-- recovery and backup versions of project assets
```

The `staging/` directory contains recovery and backup copies created while the Power BI report was being developed. The main project files above are the primary sources for reproduction.

## How to Run

### 1. Python and Jupyter

From a Python environment, install the common analysis packages:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl jupyter
```

Place `online_retail_II.xlsx` beside the notebook, then open and run:

```bash
jupyter notebook "AI_Assisted_Data_Analytics_Final_Project.ipynb"
```

Use **Run All** to:

1. merge the two workbook sheets;
2. clean and cache the data;
3. generate invoice features;
4. create charts;
5. train and evaluate models;
6. export dashboard data and insights.

The first workbook read can take several minutes for roughly one million rows. The combined CSV cache makes later runs substantially faster.

### 2. Power BI

Open `online retail(power bi).pbix` in Power BI Desktop to explore the report. The decomposed `.Report` and `.SemanticModel` directories provide the report definition and semantic model source files for tools that support PBIP/PBIR projects.

## Main Technologies

- Python
- Pandas and NumPy
- Matplotlib and Seaborn
- Scikit-learn
- Jupyter Notebook
- Microsoft Power BI
- CSV and Excel data exports

## Interpretation and Limitations

- The dataset covers a historical business period and should not be used as evidence of current retail performance.
- December 2011 contains transactions only until 09 December, so the late-month drop is incomplete rather than necessarily a decline.
- Revenue is highly concentrated by country and customer, but the data does not include marketing spend, inventory, product margin, or customer acquisition cost.
- Missing customer IDs are preserved as guest checkouts. They cannot be used for repeat-purchase analysis at the individual-customer level.
- The high-value classification target is based on a median threshold; it is a decision-support label, not a forecast of future invoices.
- The regression result is on a log-transformed target and is intended for relative value estimation.
- Cancellations are shown separately to prevent negative and reversed transactions from overstating sales.

## What Was Accomplished

This project started with raw workbook sheets and produced a defensible analytical workflow end to end:

1. combined overlapping source periods safely;
2. separated cancellations and invalid lines instead of silently dropping useful information;
3. preserved guest-customer and wholesale behavior;
4. removed genuine data errors without deleting legitimate high-demand orders;
5. engineered reusable line-, invoice-, customer-, and time-level features;
6. identified revenue, customer, product, geographic, seasonal, and timing patterns;
7. tested four classifiers and a regression model;
8. documented model trade-offs and business implications;
9. exported fact tables and KPI measures for Power BI;
10. generated a reproducible narrative of findings and recommendations.

## Dataset Attribution

The project uses the UCI **Online Retail II** dataset.

- Dataset: Daqing Chen
- Source: UCI Machine Learning Repository
- License: Creative Commons Attribution 4.0 International (CC BY 4.0)
- DOI: `10.24432/C5CG6D`

Please attribute the dataset when redistributing data or derivative publications.
