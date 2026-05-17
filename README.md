# BigMart Sales — Data Analysis Portfolio

This repository documents my first two data science projects completed as part of my college learning journey. Both projects use the BigMart Sales dataset — a retail dataset containing 8,523 sales records across 10 outlets in India. I chose to keep both projects in one place because they tell a connected story: the first project prepares the data, and the second project investigates it.

---

## Project 1 — Data Cleaning & Visualization

### Objective
Raw data is rarely ready for analysis. Before any meaningful exploration could happen, the dataset needed to be understood, audited, and fixed. This project focused entirely on that preparation work.

### Problems Found in Raw Data

**Inconsistent Fat Content Labels**
The FatContent column contained five unique values when it should have had two. Labels like `LF`, `low fat`, and `Low Fat` all referred to the same category. These were standardised into just `Low Fat` and `Regular` before any analysis began.

**Missing Weight Values — 1,463 rows (17.2%)**
Rather than filling all missing weights with a single global average, each missing value was filled using the median weight of its specific product type. A dairy product and a household cleaning item have very different typical weights — using one number for everything would introduce noise rather than reduce it.

**Missing Outlet Size Values — 2,410 rows (28.3%)**
Outlet size is a text column, so median does not apply. Instead, the most common outlet size within each outlet type was used as the fill value. This preserves the underlying business logic — Grocery Stores are almost always small-sized, so filling their missing values with Small makes far more sense than a random global guess.

**Establishment Year — Feature Engineering**
The raw column stored the year an outlet was founded, which is not directly useful for comparison. It was converted into outlet age (2025 minus establishment year) to make patterns easier to interpret.

### Visualizations Built

Five charts were produced, each answering a specific business question rather than existing purely for demonstration:

- **Sales Distribution** — confirmed right skew; most products are mid-range sellers with a small number of exceptional high performers
- **Total Sales by Product Type** — Fruits & Vegetables and Snack Foods lead all categories
- **Sales by Outlet Type** — Supermarket Type 3 consistently outperforms all other formats
- **MRP vs Sales** — moderate positive relationship; higher priced items tend to sell more but outlet type is a stronger influence
- **Correlation Heatmap** — MRP is the strongest numeric predictor (0.57); product visibility shows a surprising negative correlation (-0.13)

### Key Finding
Product visibility has a slight negative correlation with sales. BigMart appears to give more shelf space to slow-moving items in an attempt to push them, while fast-moving products sell themselves regardless of placement. The current visibility strategy does not appear to be driving sales.

---

## Project 2 — Exploratory Data Analysis

### Objective
With clean data in hand, this project focused on systematic investigation — asking structured business questions and answering each one with the appropriate chart and statistical method. The goal was not just to produce visualizations but to build a coherent understanding of what drives sales performance across BigMart's product and outlet network.

### Business Questions Investigated

```
About Products
Q1.  Which product types generate the most sales?
Q2.  Does a higher MRP always mean higher sales?
Q3.  Do low fat products outsell regular fat products?
Q4.  Which products have highest visibility — do they sell more?

About Outlets
Q5.  Which outlet type performs best in sales?
Q6.  Do bigger outlets sell more than smaller ones?
Q7.  Does outlet location (Tier 1/2/3) affect sales?
Q8.  Do older outlets perform better than newer ones?

About the Data Itself
Q9.  How is sales data distributed — is it skewed?
Q10. Which columns have the strongest relationship with sales?
Q11. Are there any outliers — which columns have them?
Q12. Which factors influence sales the most?
```

### Analysis Performed

**Univariate Analysis**
Each column was studied individually before any comparisons were made. OutletSales and ProductVisibility are both highly right skewed (skewness above 1.0), while Weight and MRP are symmetric and consistent. Supermarket Type 1 dominates the dataset in terms of product count, which means conclusions about other outlet types are based on proportionally less data.

**Bivariate Analysis**
Two columns were studied together to find relationships. MRP shows a moderate positive correlation with sales (0.57), meaning higher priced products tend to generate more revenue but with many exceptions. Product visibility shows a weak negative correlation (-0.13), confirming the finding from Project 1. Outlet type produces the most dramatic differences in average sales — far more than location tier or outlet size.

**Multivariate Analysis**
A third variable was introduced to test whether relationships held consistently across groups. The most significant finding here was that the MRP and sales relationship behaves very differently depending on outlet type. In Grocery Stores the relationship is almost flat — price has no meaningful influence on how much sells. In Supermarket Type 3 the relationship is steeper — higher priced products genuinely sell significantly more. The overall 0.57 correlation figure was masking two very different underlying patterns.

**Outlier Analysis**
Outliers were identified using the IQR method across all numeric columns. Weight and MRP showed zero outliers. ProductVisibility had 144 outliers (1.7%) and OutletSales had 186 outliers (2.2%). Both were retained — they represent real shelf allocations and genuine high-performing products respectively, not data entry errors.

**Statistical Summaries**
Skewness, kurtosis, and percentile breakdowns were calculated for all numeric columns. The 90th percentile of OutletSales sits at approximately ₹4,570, meaning only 10% of products generate sales above this level. The groupby summary confirmed that Supermarket Type 3 leads in both mean and median sales, ruling out the possibility that its high average is driven purely by a few outlier products.

### Key Findings

**What drives sales at BigMart:**
Outlet type is the single strongest differentiator of sales performance. Supermarket Type 3 generates nearly three times the average sales of a Grocery Store. Location tier, contrary to expectation, has minimal impact — Tier 1 cities do not significantly outsell Tier 2 or Tier 3. Medium sized outlets outperform Large ones, which suggests that format efficiency matters more than raw size.

**What does not drive sales:**
Product visibility, outlet age, and product weight all show near-zero correlation with sales. Fat content has a negligible impact — customers do not meaningfully prefer Low Fat over Regular in terms of purchase volume.

**The MRP insight:**
Price matters, but only in the right context. The same product priced identically will sell very differently depending on which outlet type it is placed in. Pricing strategy cannot be designed in isolation from outlet format.

---

## Combined Business Recommendations

Based on findings across both projects:

**1. Prioritise Supermarket Type 3 for any new outlet expansion.**
It consistently outperforms all other formats across product categories and location tiers. No other single factor produces a performance gap as large as outlet type.

**2. Focus inventory and stocking efforts on daily necessity categories.**
Fruits, Vegetables, and Snack Foods drive the highest total sales. These categories bring customers in regularly and generate consistent volume even at lower price points.

**3. Redesign the shelf visibility strategy.**
Two separate analyses — correlation in Project 1 and bivariate analysis in Project 2 — both arrived at the same conclusion. More shelf space does not translate to more sales. A structured review of which products receive premium placement, and why, is overdue.

**4. Treat Medium outlet size as the target format.**
Medium outlets outperform Large ones in average sales across most location tiers. Larger does not mean more productive — medium format appears to be the operational sweet spot.

**5. Do not over-invest in location tier selection.**
Tier 1, Tier 2, and Tier 3 cities produce similar average sales. The energy spent on location tier analysis would be better directed toward outlet type and format decisions, which show far greater performance differences.

---

## Project 3 — Predictive Modeling Using Machine Learning

### Objective
Having cleaned the data in Project 1 and understood it in Project 2, the natural next step was to build models that could predict outlet sales for unseen products. This project applies three supervised learning algorithms to the same BigMart dataset, compares their performance, and identifies which features drive the most accurate predictions.

### What is Supervised Learning
Supervised learning means training a model on data where the answer is already known. In this case, the model learns from 6,818 rows where actual sales figures are available, then gets tested on 1,705 rows it has never seen. The goal is to predict OutletSales as accurately as possible using product and outlet attributes as inputs.

### Data Preparation

**Feature Selection**
Nine columns were selected as inputs based on findings from the EDA in Project 2. Columns like ProductID and OutletID were excluded because they are identifiers with no predictive value. EstablishmentYear was dropped in favour of the OutletAge feature created in Project 1.

```
Input features  → MRP, OutletAge, ProductVisibility, Weight,
                  FatContent, OutletSize, OutletType,
                  LocationType, ProductType

Target column   → OutletSales
```

**Encoding Categorical Columns**
Machine learning algorithms cannot process text. All categorical columns were converted to numbers using Label Encoding before training began.

**Train Test Split**
The dataset was split into 80% training (6,818 rows) and 20% testing (1,705 rows). The model learns exclusively from the training set and is evaluated on the test set — rows it has never seen during training. This ensures the performance metrics reflect genuine predictive ability rather than memorisation.

### Models Built

**Linear Regression**
The simplest model — fits a weighted formula across all input features to predict sales. It assumes a straight line relationship between inputs and output, which makes it fast but limited when real patterns are more complex.

**Decision Tree**
Instead of a formula, a Decision Tree asks a series of yes/no questions about the features. For example — is MRP above 150? Is OutletType equal to Type 3? Each path through the tree leads to a predicted sales value. A maximum depth of 6 was set to prevent the tree from becoming too complex and overfitting to the training data.

**Random Forest**
Builds 100 Decision Trees simultaneously, each trained on a random subset of rows and features. The final prediction is the average across all 100 trees. This approach corrects for the individual errors each tree might make — the collective vote is more reliable than any single tree alone.

### Model Performance

| Model | MAE | RMSE | R2 Score |
|---|---|---|---|
| Linear Regression | 856 | 1143 | 0.52 |
| Decision Tree | 727 | 1046 | 0.60 |
| Random Forest | 726 | 1037 | 0.60 |

**MAE** — average prediction error in rupees. Lower is better.
**RMSE** — similar to MAE but penalises large errors more heavily. Lower is better.
**R2 Score** — proportion of sales variation the model explains. Higher is better.

Random Forest is the best performing model across all three metrics, though the margin over Decision Tree is small. This pattern is common in retail datasets where a single well-tuned tree already captures most of the underlying structure.

### A Note on Evaluation Visuals
The project brief mentions confusion matrices and ROC curves. These apply exclusively to classification problems — tasks where the output is a category such as spam or not spam, churn or no churn. Since this project predicts a continuous number (OutletSales), the appropriate evaluation approach is MAE, RMSE, and R2, supplemented by the following two visualizations.

**Actual vs Predicted Plot**
Each dot represents one product from the test set. Its horizontal position shows the actual sales value and its vertical position shows what the model predicted. The diagonal reference line marks perfect prediction. Dots clustered close to this line indicate reliable predictions.

**Residual Plot**
A residual is the difference between actual and predicted sales. Plotting residuals against predicted values reveals whether errors are random or systematic. A mean residual of -₹28 against an average sales value of ₹2,181 confirms the model has virtually no directional bias — it does not consistently over or under predict.

### Feature Importance

Random Forest tracks which features it relied on most across all 100 trees to produce accurate predictions.

```
Most important  → MRP
                  confirms the 0.57 correlation found in EDA
                  price is the single strongest signal

Second          → OutletType
                  consistent with EDA finding that outlet
                  format drives more variation than any
                  other categorical feature

Least important → OutletSize
                  consistent with EDA — medium vs large
                  distinction adds minimal predictive value
```

The feature importance ranking matches the EDA findings from Project 2 almost exactly, which validates the analytical work done before any modelling began.

### Key Takeaways

The models collectively explain 60% of sales variation. The remaining 40% is likely driven by factors not present in this dataset — promotional activity, seasonal demand, competitor proximity, and customer footfall data would all improve prediction accuracy if available.

More importantly, the progression across three projects demonstrates a complete data science workflow. Raw messy data was cleaned, systematically explored to understand what matters, and finally used to build models that quantify those relationships. Each project informed the next — the cleaning made EDA possible, and the EDA made modelling more deliberate.




## Tools Used

- **Python 3** — core programming language
- **Pandas** — data loading, cleaning, groupby analysis, feature engineering
- **NumPy** — numerical operations, IQR outlier detection
- **Matplotlib** — base charting library for histograms, bar charts, scatter plots
- **Seaborn** — statistical visualizations including box plots, heatmaps, pairplots, and FacetGrid

---

## How to Run

1. Clone or download this repository
2. Place `bigmart.csv` in the `data/` folder
3. Open either notebook in Jupyter
4. Run all cells from top to bottom

No additional configuration is needed. All libraries are standard and available in any Jupyter environment. If running for the first time, install dependencies with:

```
pip install pandas numpy matplotlib seaborn
```

---

## About

These two projects represent the foundation of a data science workflow — cleaning raw data and then systematically exploring it. Both were completed during college as part of an internship project series. Feedback is welcome.
