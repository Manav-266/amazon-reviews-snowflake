# Data

The raw dataset (~300 MB across 3 CSV parts) exceeds GitHub file limits and is not stored here.

**Source:** [Amazon Fine Food Reviews — Kaggle](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews)
568,454 food reviews from Amazon, October 1999 – October 2012.

**Columns:** Id, ProductId, UserId, ProfileName, HelpfulnessNumerator, HelpfulnessDenominator, Score, Time (Unix), Summary, Text

Download from Kaggle, split into parts if needed, and load into Snowflake using `sql/01_ingestion.sql`.
