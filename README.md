# Amazon Reviews Analytics Pipeline on Snowflake

**Tech stack:** Snowflake (Stages, COPY INTO, Warehouses) · SQL · Snowpark for Python · Pandas · Seaborn/Matplotlib

An end-to-end analytics pipeline processing **568,000+ Amazon Fine Food reviews** entirely inside Snowflake — from raw CSV ingestion through layered SQL transformation to sentiment feature engineering and dashboard-ready visuals.

## Architecture

```
Raw CSVs --> Snowflake Internal Stage --> RAW schema --> ANALYTICS schema --> Visuals
             (PUT / COPY INTO)            REVIEWS_RAW     REVIEWS_CLEAN
                                                          PRODUCT_METRICS
                                                          DAILY_RATINGS
                                                          FEATURE_ENGINEERED_REVIEWS
```

The pipeline follows a layered warehouse design: an immutable **RAW** landing zone, and an **ANALYTICS** schema containing cleaned, validated, and aggregated tables that downstream analysis reads from.

## Pipeline Stages

| # | Notebook | What it does |
|---|---|---|
| 00 | `00_data_ingestion.ipynb` | Creates internal stage + file format, defines the raw schema, bulk-loads CSVs with `COPY INTO` and error tolerance |
| 01 | `01_data_cleaning_transform.ipynb` | SQL transformations: null handling, helpfulness-ratio calculation, Unix-to-date conversion, data-quality filters; builds `REVIEWS_CLEAN`, `PRODUCT_METRICS`, `DAILY_RATINGS` |
| 02 | `02_eda_business_metrics.ipynb` | Snowpark to Pandas EDA: rating distributions, review length patterns, helpfulness trends, time-based behavior |
| 03 | `03_sentiment_features.ipynb` | Text feature engineering: word counts, caps ratio, exclamation counts, lexicon-based sentiment scoring; writes engineered table back to Snowflake |
| 04 | `04_dashboard_visuals.ipynb` | Consolidated visual layer: ratings, sentiment labels, product performance, trends |

Standalone SQL scripts are also available in [`sql/`](sql/) for quick review of the warehouse work.

## Data Quality Rules Applied

- Ratings constrained to valid 1-5 range
- Helpfulness votes validated (numerator <= denominator, non-negative)
- Null review text excluded
- Partial-load tolerance on ingest (`ON_ERROR = 'CONTINUE'`) with post-load row-count verification

## Dataset

[Amazon Fine Food Reviews (Kaggle)](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews) — 568,454 reviews, 1999-2012. The raw CSVs (~300 MB) are not stored in this repo; download from Kaggle and load via `sql/01_ingestion.sql`.

## Skills Demonstrated

- Snowflake warehouse fundamentals: stages, file formats, `COPY INTO`, schema design
- Production-minded SQL: CTAS transformations, data-quality gates, aggregate metric tables
- Snowpark for Python: querying warehouse tables into Pandas for analysis at scale
- Feature engineering on text data and writing results back to the warehouse

## Run It Yourself

1. Create a Snowflake trial account and a database `AMAZON_REVIEWS_DB`
2. Download the dataset from Kaggle and split/upload to the stage (`sql/01_ingestion.sql`)
3. Run notebooks 00-04 in order inside Snowflake Notebooks (Snowpark session is provided by the environment)

---
*Developed as a graduate group project (M.Sc. Data Analytics, 2025) — team of 4; my focus areas included SQL transformation and feature engineering. Restructured for portfolio presentation.*
