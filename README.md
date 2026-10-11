# Bakery Sales, Waste and Profitability Analysis

*Identifying what drives loss-making sales in a multi-store bakery* — a Python data analytics and machine learning project prepared as part of an IBM internship assignment.

---

## 1. Project Overview

Bakery products spoil quickly, so profit depends on more than sales: discounts and unsold (wasted) stock can erase earnings. This project analyses **16,569 bakery sales records from three stores (2023–2024)** to find out where profit is made, where it is lost, and which factors are linked to loss-making sales. It ends with a simple machine learning model that flags likely loss-making rows.

## 2. Problem Statement

The dataset records about **3.44 million currency units of revenue**, but the `Profit` column adds up to a **net loss of about 230,339**. Management needs to know which products, categories, promotions and shelf-life groups are responsible, so production and discount decisions can be improved.

## 3. Objectives

1. Inspect the dataset and check data quality (missing values, duplicates, inconsistent values).
2. Clean the data and engineer useful features (month, hour, profit margin, loss flag, shelf-life groups, etc.).
3. Measure revenue, waste cost and profit by product, category, store, season and time.
4. Study how discounts and promotions relate to profit.
5. Study how shelf life and unsold stock relate to waste and profit.
6. Build and correctly evaluate a classification model that predicts whether a row will be loss-making.
7. Provide data-backed recommendations.

## 4. Dataset Description

| Item | Details |
|---|---|
| File | `bakery_synthetic_dataset_csv__1_.xlsx` (sheet `bakery_synthetic_dataset`) |
| Size | 16,569 rows × 41 columns (one row = one product line in a customer transaction) |
| Period | 1 Jan 2023 – 31 Dec 2024 |
| Stores / products | 3 stores (ST01–ST03), 35 products in 10 categories |
| Customers | 2,173 unique customer IDs |
| Main column groups | Transaction and customer details, product and price, discounts and promotions, weather and season, stock and shelf life (`Units_Produced`, `Units_Sold`, `Unsold_Units`, `Expiry_Risk`), `Profit`, `Waste_Cost` |
| Target used | No target column is supplied. `Is_Loss` (1 if `Profit < 0`) is created from `Profit`. |
| Data quality | No duplicate rows. Only `Festival` and `Promotion_Type` have empty cells, and these mean "not applicable". Stock, date and billing columns agree with each other. |

**Dataset source / link:** The dataset was **provided privately** for this assignment and has no public URL. Its name indicates that it is *synthetic*. The currency is not stated in the data.

**How to obtain / place the dataset:** get the Excel file from the project supervisor or the original submission, create a folder named `data/` next to the notebook, and put the file there as `data/bakery_synthetic_dataset_csv__1_.xlsx`. If you use another location, change the `DATA_PATH` variable in the notebook (Section 6).

## 5. Technologies and Libraries Used

* **Python 3.10+** (tested on 3.12)
* **pandas, NumPy** – data handling
* **Matplotlib, Seaborn** – visualisation
* **SciPy** – statistical tests (Mann-Whitney U, chi-square)
* **scikit-learn** – preprocessing, Logistic Regression, Random Forest, evaluation
* **openpyxl** – reads the `.xlsx` file
* **Jupyter** – running the notebook

## 6. Project Folder Structure

```
bakery-profitability-analysis/
├── SnehaYadav_ProjectName.ipynb      # Main Jupyter Notebook (analysis + model)
├── SnehaYadav_ProjectReport.docx     # Formal project report
├── README.md                         # This file
├── requirements.txt                  # Python dependencies
├── data/
│   └── bakery_synthetic_dataset_csv__1_.xlsx   # Dataset (you must add this file)
└── figures/                          # Charts (also re-created when the notebook runs)
    ├── 01_profit_bill_distribution.png
    ├── ...
    └── 11_feature_importance.png
```

## 7. Installation Instructions

```bash
# 1. Go to the project folder
cd bakery-profitability-analysis

# 2. (Recommended) create and activate a virtual environment
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# 3. Install the dependencies
pip install -r requirements.txt
```

## 8. Setup and Execution Steps

1. Complete the installation above.
2. Place the dataset in `data/` as described in Section 4.
3. Start Jupyter from the project folder:
   ```bash
   jupyter notebook
   ```
4. Open `SnehaYadav_ProjectName.ipynb`.

## 9. How to Run the Jupyter Notebook

* In the notebook choose **Kernel → Restart & Run All** (or **Run → Run All Cells** in JupyterLab).
* The notebook runs from top to bottom without manual changes, provided the dataset is at `data/bakery_synthetic_dataset_csv__1_.xlsx`.
* All charts appear in the notebook and are saved to the `figures/` folder.
* The model section includes a small grid search and can take a short while; the random seed is fixed (`42`) so results are reproducible on the same library versions.

## 10. Methodology in Brief

1. Load and inspect the data → 2. data-quality checks → 3. cleaning and feature engineering → 4. EDA and charts → 5. statistical tests → 6. machine learning: predict `Is_Loss` with Logistic Regression and Random Forest, compared with a majority-class baseline → 7. findings and recommendations.

**Correct model evaluation:** the models are **trained on 2023 and tested on 2024**, with time-ordered cross-validation inside 2023 for tuning. Columns that would leak the answer or are only known after the sale (`Profit`, `Waste_Cost`, `Unsold_Units`, `Stock_Available`, `Units_Sold`, `Expiry_Risk`, etc.) are **not** used as inputs.

## 11. Key Analysis Results and Findings

All numbers below come from running the notebook on the provided dataset.

| Measure | Result |
|---|---|
| Total revenue | 3,444,112.95 |
| Total recorded waste cost | 1,388,812.80 (40.3% of revenue) |
| Total profit | −230,339.25 |
| Loss-making rows | 50.4% |
| Products with a net loss | 23 of 35 |
| Months with negative total profit | 24 of 24 |

* **Largest losses by product:** Black Forest Cake (−54,908.52), Chocolate Cake (−40,012.97), Honey (−39,437.53), Eggs (−30,753.30).
* **Most profitable products:** Coffee (+35,301.44), Cold Coffee (+22,204.34), Tea (+14,168.81), Juice (+10,216.41).
* **Cakes** bring the most revenue (1,288,747) but also the highest waste cost (515,611) and the largest category loss (−107,657). Dairy (−105,527) and Spreads (−74,018) also lose heavily; Beverages are the strongest category (+74,824).
* **Promotions:** average profit per row is −69.13 with a promotion vs +0.15 without; the loss rate is 84.6% vs 41.7%. The difference is statistically significant (p < 0.001), but average quantity bought is almost the same (2.56 vs 2.60, p = 0.103, not significant).
* **Discount depth:** the loss rate is 39.4% at 0% discount and 94.3% at 20–30% discount.
* **Quantity per row:** loss rate falls from 70.1% (1 unit) to 30.4% (6 units).
* **Shelf life:** 1-day items lose money on 27.0% of rows; items with 4–30 days shelf life on about 66–74%.
* **Loss is not tied to one store, season or month** – every store, season and month shows negative profit, which points to product, pricing and discount decisions.
* **Model (trained 2023, tested on unseen 2024 data):**

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Baseline (majority class) | 0.491 | 0.000 | 0.000 | 0.000 | 0.500 |
| Logistic Regression | 0.748 | 0.759 | 0.740 | 0.749 | 0.833 |
| Random Forest | 0.790 | 0.804 | 0.777 | 0.791 | 0.871 |

Most important features (permutation importance): `Quantity`, `Units_Produced`, `Discount_Percentage`, `Category`, `Promotion_Applied`.

## 12. Visualizations

The notebook produces 11 chart files in `figures/`, for example:

| Chart | File |
|---|---|
| Profit and bill distributions | `figures/01_profit_bill_distribution.png` |
| Profit by product | `figures/02_profit_by_product.png` |
| Revenue, waste and profit by category | `figures/03_category_revenue_waste_profit.png` |
| Shelf life and expiry risk | `figures/04_shelf_life_expiry_risk.png` |
| Promotions | `figures/05_promotions.png` |
| Monthly trend | `figures/06_monthly_trend.png` |
| Correlation heatmap | `figures/09_correlation_heatmap.png` |
| Confusion matrices | `figures/10_confusion_matrices.png` |
| Feature importance | `figures/11_feature_importance.png` |



## 13. Limitations

* The dataset is **synthetic**, so conclusions describe this dataset and may not hold for a real bakery.
* Production, sales and waste quantities are the same for rows sharing a date, store and product, but `Waste_Cost` and `Profit` are given per row. If batch waste was charged on every row, losses may be overstated. The data does not explain how `Profit` is calculated, so it was used as provided.
* No cost price column is given and the currency is not stated.
* 2023 and 2024 look very similar, so the 2024 test score may be optimistic for a real change in conditions.
* Results show association, not proof of cause. Product, price and category overlap, which affects how feature importance should be read.

## 14. Future Improvements

* Obtain batch-level waste and cost-price data to calculate profit accurately.
* Forecast daily demand per product to guide production quantities.
* Test discount strategies with a controlled experiment (A/B test).
* Add a regression model to predict profit amount, and try gradient boosting.
* Build a small dashboard for store managers.

## 15. Author

**Sneha Yadav**
IBM Internship Project


