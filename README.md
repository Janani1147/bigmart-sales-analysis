# BigMart Sales — Data Cleaning & Visualization

This project is part of my data science learning journey as a college student. I chose the BigMart Sales dataset because it closely resembles real-world retail data — messy, incomplete, and full of patterns that are not immediately obvious. The goal was not just to clean the data and produce charts, but to actually understand what the data is saying and translate that into meaningful business insights.

---

## Why This Dataset

BigMart is a fictional retail chain with outlets across multiple cities in India. The dataset contains sales records for 1,559 products across 10 stores. What makes it interesting is that it is intentionally imperfect — missing values, inconsistent labels, and a mix of numerical and categorical data. This makes it a realistic starting point for anyone learning data preprocessing.

---

## What This Project Covers

The project is structured in three phases:

**Phase 1 — Exploration**
Before touching anything, I explored the raw data to understand its structure, spot problems, and form questions worth answering. This step is often skipped by beginners but it is arguably the most important one.

**Phase 2 — Cleaning**
Raw data is rarely ready for analysis. I identified and fixed four specific problems in this dataset — inconsistent category labels, missing numerical values, missing categorical values, and a column that needed to be transformed into something more meaningful.

**Phase 3 — Visualization & Insights**
I built five different chart types, each chosen deliberately to answer a specific business question rather than just to demonstrate variety.

---

## Data Cleaning — What Was Fixed and Why

**Problem 1 — Inconsistent Fat Content Labels**
The FatContent column had five unique values when it should have had two. Labels like `LF`, `low fat`, and `Low Fat` all meant the same thing. These were standardised into just `Low Fat` and `Regular`.

**Problem 2 — Missing Weight Values (1,463 rows)**
Rather than filling all missing weights with a single global median, I filled them using the median weight of each product type separately. A biscuit packet and a bottle of shampoo have very different typical weights — using one number for all of them would introduce noise, not reduce it.

**Problem 3 — Missing Outlet Size Values (2,410 rows)**
Outlet size is a categorical column, so median does not apply. Instead, I used the most common outlet size within each outlet type. This preserves the business logic — for example, Grocery Stores are almost always small-sized, so filling missing values with "Small" for that group makes far more sense than a random global guess.

**Problem 4 — Establishment Year**
The raw column stored the year an outlet was founded, which is not directly useful for analysis. I converted it into outlet age (2025 minus establishment year) to make comparisons more intuitive.

---

## Visualizations & Key Findings

**Chart 1 — Sales Distribution (Histogram)**
Sales data is heavily right-skewed. The majority of products generate between ₹500 and ₹2,500 in sales, while a small number of products significantly outperform the rest. This is a textbook example of the 80-20 principle in retail — a minority of products drive the majority of revenue.

**Chart 2 — Total Sales by Product Type (Bar Chart)**
Fruits & Vegetables and Snack Foods lead all categories in total sales. These are not high-margin items, but they are purchased frequently and consistently. Seafood sits at the bottom, which raises a question worth investigating — is this a demand problem or a supply problem?

**Chart 3 — Sales by Outlet Type (Box Plot)**
Supermarket Type 3 generates the highest and most consistent sales across products. Grocery Stores, on the other hand, produce low and stable sales — they serve a convenience purpose rather than a volume one. Supermarket Type 1 shows the most outliers, meaning a handful of products perform exceptionally well there, even though average performance is moderate.

**Chart 4 — MRP vs Sales (Scatter Plot)**
There is a moderate positive relationship between price and sales (correlation of 0.57). However, the scatter is wide enough to make it clear that price alone does not determine sales. The same product priced identically generates vastly different sales depending on which type of outlet it is placed in — outlet type appears to be a stronger influence than price.

**Chart 5 — Correlation Heatmap**
MRP is the strongest numeric predictor of outlet sales. The more surprising finding is that product visibility carries a slight negative correlation with sales (-0.13). Products given more shelf space do not necessarily sell more — in fact, BigMart appears to be giving more visibility to slow-moving items in an attempt to push them, while fast-moving items sell themselves regardless of placement.

---

## Business Recommendations

Based on the analysis, three actionable recommendations stand out:

1. **Prioritise Supermarket Type 3 for expansion.** It consistently outperforms other formats across product categories and locations.

2. **Double down on daily necessity categories.** Fruits, Vegetables, and Snack Foods drive volume. Ensuring these are always well-stocked is more impactful than optimising premium product placement.

3. **Revisit the shelf visibility strategy.** The data does not support the assumption that more visible products sell better. A structured A/B test on product placement could reveal whether the current approach is actively hurting sales of certain items.

---

## Project Structure

```
bigmart-sales-analysis/
│
├── bigmart.csv                  # Raw dataset
├── bigmart_analysis.ipynb       # Full notebook with code and output
├── bigmart_clean.csv            # Cleaned dataset after preprocessing
└── README.md                   # This file
```

---

## Tools Used

- **Python 3** — core programming language
- **Pandas** — data loading, cleaning, and transformation
- **NumPy** — numerical operations
- **Matplotlib** — base charting library
- **Seaborn** — statistical visualizations built on top of Matplotlib

---

## How to Run

1. Clone or download this repository
2. Open `bigmart_analysis.ipynb` in Jupyter Notebook or Google Colab
3. Upload `bigmart.csv` to the same working directory
4. Run all cells from top to bottom

No additional configuration is needed. All required libraries are standard and available in both Jupyter and Colab environments.

---

## About

This project was completed as part of my data science coursework during college. It is my first end-to-end data analysis project — from raw messy data to cleaned output and visual storytelling. Feedback and suggestions are welcome.
