# Big Data Video Games Reviews

Apache Spark and MongoDB course project for analyzing large-scale **Amazon Reviews 2023** data in the **Video Games** category.

## Overview

This project builds an end-to-end **Big Data analytics pipeline** to study customer review behavior, product trends, and query performance using:

- **Apache Spark (PySpark)** for large-scale data ingestion, transformation, and analysis
- **Spark SQL** for analytical querying
- **MongoDB** for NoSQL storage and indexed document queries
- **Matplotlib / Pandas** for visual outputs and summary tables

The project focuses on the **Amazon Reviews 2023 – Video_Games** dataset from the McAuley Lab collection and demonstrates how Spark and MongoDB can be combined in a practical analytics workflow.

## Problem Statement

Customer reviews contain valuable signals about user satisfaction, purchasing behavior, product popularity, and review usefulness. However, extracting these insights at scale requires a pipeline that can handle millions of records efficiently.

The goal of this project is to analyze review patterns in the Amazon Video Games category and support **data-driven decision-making** in e-commerce by identifying:

- rating behavior and sentiment trends
- differences between verified and non-verified purchases
- helpful review patterns
- product, category, and platform-level trends
- performance differences between **Spark SQL** and **MongoDB** queries

## Dataset

**Source:** Amazon Reviews 2023 dataset by McAuley Lab  
**Category used:** `Video_Games`

The selected dataset contains large-scale product review and metadata information, including fields such as:

- review text
- rating
- timestamp
- helpful votes
- verified purchase flag
- product title
- categories
- price and store information

This makes it well suited for a Big Data project involving distributed processing, NoSQL modeling, performance optimization, and analytics.

## Project Objectives

- Download and validate the raw Video Games review and metadata files
- Clean and standardize review and product data with **PySpark**
- Build a reusable analytical base dataset in **Parquet** format
- Analyze the data with the **PySpark DataFrame API** and **Spark SQL**
- Design a practical **MongoDB schema** for products and reviews
- Create indexes for query optimization in MongoDB
- Compare representative query performance between **MongoDB** and **Spark**
- Present insights through summary tables and visualizations

## Pipeline Architecture

The project follows this workflow:

1. **Data ingestion**  
   Download the Amazon Reviews 2023 `Video_Games` raw review and metadata files.

2. **Initial profiling**  
   Inspect schema, row counts, missing values, distributions, and join-key quality.

3. **Cleaning and transformation**  
   Standardize review and metadata fields, clean selected columns, and join both datasets using `parent_asin`.

4. **Analytical base dataset creation**  
   Save the cleaned dataset in **Parquet** for efficient reuse in later stages.

5. **Spark analysis**  
   Perform large-scale transformations, aggregations, Spark SQL queries, joins, and trend analysis.

6. **MongoDB integration**  
   Load product and review documents into MongoDB using a referenced schema.

7. **Indexing and query comparison**  
   Create indexes and compare MongoDB query timings with equivalent Spark queries.

## MongoDB Schema Design

The MongoDB part uses a **referenced schema** with two collections:

### `products`
One document per product, including fields such as:

- `_id` = `parent_asin`
- `product_title`
- `main_category`
- `categories_clean`
- `category_count`
- `store_clean`
- `price_value`

### `reviews`
One document per review, including fields such as:

- `_id` = stable generated hash
- `parent_asin`
- `user_id`
- `rating`
- `verified_purchase`
- `helpful_vote`
- `review_text_length`
- `review_ts`
- `review_date`
- `review_year`
- `review_month`
- `review_year_month`

This design avoids embedding all reviews under a single product document, which would not scale well for highly reviewed products.

## Indexing Strategy

Indexes are created on the fields used most often in the MongoDB queries:

- `product_title`
- `parent_asin`
- `rating`
- `verified_purchase`
- `review_year_month`
- compound index on `(parent_asin, helpful_vote)`

These indexes support faster filtering, aggregation, and targeted lookup queries.

## Main Analytical Questions

The project investigates questions such as:

- How are ratings distributed across the dataset?
- How do **verified** and **non-verified** purchase reviews differ?
- Which products receive the highest review volume?
- Which reviews receive the most helpful votes?
- How do review patterns change over time?
- Which categories and platform groups appear most often in the Video Games ecosystem?

## Key Findings

Based on the analysis notebooks, the dataset shows several clear patterns:

- Review sentiment is strongly skewed toward high ratings
- Verified purchases make up the majority of reviews
- Helpful-vote activity is highly skewed, with many reviews receiving zero helpful votes
- Lower-rated reviews often attract stronger engagement
- The dataset covers not only games, but also accessories, hardware, and platform-related products
- Spark is especially useful for large-scale transformations and analytics
- MongoDB performs well for indexed document-style lookups and operational queries

## Performance Comparison

Representative analytical queries were implemented in both **Spark** and **MongoDB**, including:

- rating distribution
- verified vs non-verified purchase behavior
- top products by review count
- top helpful reviews for a selected product

The comparison shows that Spark and MongoDB serve different strengths:

- **Spark** is better suited for large-scale data processing and analytical workloads
- **MongoDB** is better suited for indexed lookup patterns and document-oriented query access

Together, they form a complementary Big Data pipeline.

## Repository Structure

```text
big-data-video-games-reviews/
├── data/
│   ├── raw/
│   │   └── amazon_reviews_2023/video_games/
│   └── processed/
│       └── video_games_analytical_base/
├── docs/
│   ├── ProjectProposal_Group14.pdf
│   └── FinalReport_Group14.pdf
├── notebooks/
│   ├── Notebook_1_Download_Amazon_Video_Games.ipynb
│   ├── Notebook_2_Inspect_Profile_Amazon_Video_Games.ipynb
│   ├── Notebook_3_Clean_Transform.ipynb
│   ├── Notebook_4_Spark_Analysis_SQL_Visuals.ipynb
│   └── Notebook_5_MongoDB_Indexing_Query_Comparison.ipynb
├── results/
│   ├── notebook4_rating_distribution/
│   ├── notebook4_verified_behavior/
│   ├── notebook4_top_categories/
│   ├── notebook5_timing_summary.csv
│   ├── notebook5_top_products_mongo.csv
│   └── notebook5_top_products_spark.csv
├── src/
├── README.md
└── project_structure.txt
```

## How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/escoo1/big-data-video-games-reviews.git
cd big-data-video-games-reviews
```

### 2. Create and activate a Python environment

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

### 3. Install dependencies

Install the packages used in the notebooks:

```bash
pip install pyspark pandas matplotlib datasets pyarrow pymongo
```

### 4. Ensure MongoDB is running

Run MongoDB locally or update the connection string in the MongoDB notebook if you are using **MongoDB Atlas**.

Default connection used in the project:

```python
MONGO_URI = "mongodb://localhost:27017"
MONGO_DB_NAME = "amazon_video_games_group14"
```

### 5. Run the notebooks in order

Execute the notebooks sequentially:

1. **Notebook 1** – download raw data
2. **Notebook 2** – inspect and profile the dataset
3. **Notebook 3** – clean, transform, and build the analytical base dataset
4. **Notebook 4** – Spark analysis, Spark SQL, and visual insights
5. **Notebook 5** – MongoDB loading, indexing, and query comparison

## Tools and Technologies

- **Python**
- **Apache Spark / PySpark**
- **Spark SQL**
- **MongoDB**
- **Pandas**
- **Matplotlib**
- **Jupyter Notebook / Google Colab / VS Code**
- **GitHub**

## Team Members

- Bach Leno
- Escobar Rüsch Oliver
- Vilkin Zakhar

## Course Context

This repository was created as part of a **Big Data Analytics** group project focused on building a full Big Data pipeline using **Apache Spark** and **MongoDB**.

The repository is intended to demonstrate:

- dataset selection and preprocessing
- Spark implementation and SQL analysis
- MongoDB schema design and indexing
- results and insights with visual support
- clear documentation and reproducible project structure

## Future Improvements

Possible next steps for extending the project:

- add **Spark Streaming** for batch vs stream comparison
- compare storage formats such as **CSV**, **Parquet**, and **Avro**
- build an interactive dashboard using **Streamlit** or **Gradio**
- package repeated logic from notebooks into reusable scripts under `src/`
- add screenshots of results and architecture diagrams to the `docs/` folder

## License

This repository is for educational use as part of a university course project.
