# Olist E-commerce Analysis

Exploring customer geography, geolocation data quality, order payments, and delivery experience with **Python, PySpark, Databricks, pandas, NumPy, SciPy, and Matplotlib**.

Adapted by **Farida Belal** from CSCI 443 Homework 6 at the University of Mississippi. The questions originated in coursework. This edition reorganizes the submitted analysis and revises selected code and explanations for a portfolio audience.

## What this project does

- Profiles customer identifiers, missing values, states, and regions.
- Examines more than one million geolocation records using Spark aggregations and approximate ZIP-prefix medians.
- Screens geographic outliers and measures their distance from ZIP-prefix median coordinates.
- Joins orders, customers, and payments to compare summed order payments in São Paulo and Rio de Janeiro states.
- Explores the association between delivery duration and review score using Pearson correlation and permutation testing.

## Findings from the original saved run

These are historical outputs from the submitted homework, **not results from a fresh run of the revised notebook**.

| Measure | Original result |
|---|---:|
| Distinct customer IDs | 99,441 |
| Distinct customer-unique IDs | 96,096 |
| Geolocation rows | 1,000,163 |
| Distinct geolocation ZIP prefixes | 19,015 |
| Rows outside expanded geographic rectangle | 40 |
| ZIP prefixes with medians outside unexpanded rectangle | 7 |
| Retained rows more than 200 km from their ZIP-prefix median | 576 / 1,000,134 (0.0576%) |
| Mean summed payment per order, SP | 143.69 |
| Mean summed payment per order, RJ | 166.85 |
| Mean difference, SP − RJ | −23.16 |
| Welch t-test | t ≈ −9.83; p ≈ 9.81 × 10⁻²³ |
| Delivery duration / review score correlation | r ≈ −0.334 |

The original sample showed lower mean order payments in SP and lower review scores associated with longer delivery times. These are observational findings, not causal conclusions.

## Files

- `olist_ecommerce_analysis.ipynb` — the analysis and documented assumptions.
- `data/README.md` — required CSV files and source.
- `CHANGELOG.md` — changes from the submitted homework and verification status.
- `.gitignore` — excludes raw datasets and notebook checkpoints.

## Run in Databricks

1. Download the [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Review the source terms before redistributing data.
2. Extract and upload these files into a Databricks volume you can read:
   - `olist_customers_dataset.csv`
   - `olist_geolocation_dataset.csv`
   - `olist_orders_dataset.csv`
   - `olist_order_payments_dataset.csv`
   - `olist_order_reviews_dataset.csv`
3. Import `olist_ecommerce_analysis.ipynb` into Databricks and attach Python/Spark compute.
4. Ensure NumPy, pandas, SciPy, Matplotlib, and psutil are available in the notebook environment. Package versions are not pinned because the original runtime versions were not supplied.
5. Set `DATA_DIR` in the setup cell to your volume directory and run all cells in order.
6. Review the results, update historical findings if appropriate, and export the executed notebook with outputs before publishing it on GitHub.

Raw datasets are not included. This notebook uses Databricks `dbutils` and `display`; it is not configured as a standalone local Jupyter application. Both permutation sections use 5,000 draws and may take several minutes. The seed makes draws reproducible for a fixed input ordering; Spark row ordering may vary across runs.

## Method notes and limitations

- State/region charts count customer records, not distinct people.
- The geographic rectangle is an approximate screen, not a verified national boundary. Approximate median coordinates can mask local variation; original coordinates are retained.
- Geographic cleaning is separate from the payment/review analyses and does not filter their records.
- Summed order payments are neither net revenue nor profit. All order statuses are retained in the payment comparison.
- Multiple reviews and repeat purchases can violate independence assumptions. Review scores are ordinal and missing delivery dates may introduce selection bias.
- Label permutation requires exchangeability, a stronger assumption than equality of means alone. No causal effect is estimated.
- Monte Carlo p-values use `(extreme + 1) / (B + 1)` rather than reporting zero. The two-sided payment test counts absolute mean differences; two-sided conventions can differ. See [SciPy documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.permutation_test.html).
- Plotting uses a small geographic sample; aggregate counts use the full Spark DataFrame. Selected payment/review arrays are collected to the driver for SciPy/NumPy calculations.

## Verification status

The revised file passed structural checks and Python syntax checks. The Olist CSV files and a connected Databricks runtime were not supplied, so the revised analysis has **not been executed end to end**. All code outputs are intentionally cleared. Multiline CSV handling was corrected; this can change review parsing and downstream statistics. Rerun before presenting the historical figures as current results.
