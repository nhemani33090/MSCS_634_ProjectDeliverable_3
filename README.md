## Deliverables 1, 2 & 3: Data Cleaning, Exploration, Regression, Classification, Clustering, and Pattern Mining

## 📘 Dataset Summary and Justification
The dataset used for this project is the **Superstore Sales Dataset**, sourced from Kaggle.  
It contains **9,994 rows** and **21 attributes**, including customer details, product information, geographic attributes, and financial metrics such as `Sales`, `Profit`, `Discount`, and `Quantity`.

This dataset is an excellent choice because:
- It includes **both numeric and categorical** features suitable for regression, classification, clustering, and association‑rule mining.
- It meets project requirements (**≥ 8 attributes**, **≥ 500 rows**).
- It reflects real-world retail behavior, making model insights meaningful and applicable.

---

## 📎 Dataset Source
Kaggle Dataset: https://www.kaggle.com/datasets/vivek468/superstore-dataset-final  
File used: `Sample - Superstore.csv`

**Attribution:**  
Vivek. (2019). *Superstore Sales Dataset*. Kaggle.

---

# 🧩 Deliverable 1 — Data Collection, Cleaning, and Exploration

### ✔️ Cleaning Steps Performed
- Loaded the dataset and inspected structure (`.info()`, `.describe()`).
- Summarized missing values **before and after cleaning**.
- Removed duplicates (0 found).
- Parsed date fields (`Order Date`, `Ship Date`) ➝ `datetime`.
- Converted `Postal Code` → string (avoids losing leading zeros).
- Imputed missing values (none present, but logic implemented).
- **Outlier Detection**: Identified extreme values using the IQR method.
- **Outlier Clipping**: Capped `Sales`, `Profit`, `Discount`, and `Quantity` to reduce noise.

### ✔️ Exploratory Data Analysis (EDA)
Visualizations included:
- Histograms  
- Boxplots  
- Correlation heatmap  
- Profit grouped by Category, Sub‑Category, Region, and Segment  

### ✔️ Insights
- Higher discounts sharply reduce profit.
- Technology products generate the highest margins.
- Tables & Bookcases are consistently low-profit categories.
- Profit is most correlated with Sales, followed by Quantity and Discount.

The cleaned dataset was saved as:  
`data/cleaned_superstore.csv`

---

# 📈 Deliverable 2 — Regression Modeling & Performance Evaluation

## ✔️ Objective
Predict **Profit** using regression models and compare model performance.

## ✔️ Models Implemented
- **Linear Regression**
- **Ridge Regression** (L2 regularization)
- **Lasso Regression** (L1 regularization)

All models used:
- One‑hot encoding for categorical features  
- Standardization for numeric features  
- Pipelines to ensure consistent preprocessing  

## ✔️ Evaluation Metrics
- R²  
- MAE  
- MSE  
- RMSE  
- **5‑fold cross‑validation**

## ✔️ Actual Results

| Model | Test R² | Test MAE | Test RMSE | CV R² Mean |
|------|---------|-----------|-----------|------------|
| Linear Regression | **0.5733** | 14.103 | 18.936 | 0.5701 |
| Ridge (α=1.0) | 0.5733 | 14.102 | 18.936 | 0.5701 |
| Lasso (α=0.001) | 0.5733 | 14.103 | 18.937 | 0.5701 |

### ✔️ Interpretation
- All models performed **almost identically**.
- Regularization does NOT dramatically improve performance, meaning:
  - Data is stable  
  - No major multicollinearity  
  - One‑hot encoding helps stabilize the model  

### ✔️ Most Important Features (Coefficient Analysis)
**Strong positive predictors:**  
- Copiers  
- Sales  
- Furnishings  
- Binders  
- Technology category  

**Strong negative predictors:**  
- Discount  
- Tables  
- Storage  
- Bookcases  
- Machines  

---

## 📘 Deliverable 3 — Classification, Clustering, and Pattern Mining

This phase expands on Deliverables 1 and 2 by introducing classification models, clustering techniques, and association rule mining to uncover deeper patterns in the Superstore dataset.

---

## 🔹 Classification Models

### **Target Variable**
To frame a classification problem, `Profit` was converted into a binary label:

- **HighProfit = 1** if Profit > median  
- **HighProfit = 0** otherwise  

This produced a perfectly balanced dataset:

- 50% high-profit  
- 50% low-profit  

### **Models Developed**
Two classification models were implemented:

1. **Decision Tree Classifier**
2. **k-Nearest Neighbors (k-NN)**

### **Performance Metrics**
Evaluated using **Accuracy**, **F1-score**, **Confusion Matrix**, and **ROC Curve**.

#### **Baseline Decision Tree Results**
- **Accuracy:** 0.9095  
- **F1-score:** 0.9086  

The model performed similarly well across both classes.

### **Hyperparameter Tuning**
GridSearchCV was applied to tune:

- `max_depth`
- `min_samples_split`

**Best parameters found:**
- `max_depth = 10`
- `min_samples_split = 10`

**Tuned Decision Tree Performance:**
- **Accuracy:** 0.9185  
- **F1-score:** 0.9195  

This tuning improved both prediction accuracy and balance between classes.

---

## 🔹 Clustering Analysis (K-Means)

K-Means clustering was applied using numeric features:

- `Sales`
- `Quantity`
- `Discount`
- `Profit`

### **Number of Clusters: 3**  
Clusters corresponded to natural transaction types:

| Cluster | Description |
|--------|-------------|
| **0** | High-discount, negative-profit transactions |
| **1** | High-profit, moderate discount, high sales |
| **2** | Low-sales, low-profit, low-discount orders |

### **Cluster Centers (interpreted):**

- **Cluster 1** → profitable customer segment  
- **Cluster 0** → discount-heavy, unprofitable segment  
- **Cluster 2** → low-value but stable small purchases  

A Sales vs Profit scatter plot was used for cluster visualization.

---

## 🔹 Association Rule Mining (Apriori)

Association rules were generated from combinations of:

- `Category`
- `Sub-Category`

### **Apriori Results**
Using `min_support = 0.02`, Apriori returned **10 strong rules**.

**Examples of strong relationships (lift > 1):**

- **Office Supplies → Binders & Paper**  
- **Technology_Phones → Furniture_Furnishings**  
- **Office Supplies_Art ↔ Technology_Phones**

These rules suggest cross-department product affinities that could help with:

- Bundling recommendations  
- Store layout decisions  
- Promotional targeting  

---

## 💡 Key Insights and Real-World Applications

### **Classification**
- Tuned Decision Trees achieved **~92% accuracy**, indicating profit category is strongly predictable using transactional features.
- Businesses can use this to **flag low-profit orders** and adjust pricing or discounting behavior.

### **Clustering**
- K-Means revealed **three clear customer/order segments**, enabling:
  - Targeted marketing strategies  
  - Identifying loss-making discount patterns  
  - Prioritizing valuable customer groups  

### **Association Rules**
- Meaningful co-purchase patterns were found within Office Supplies and Technology.
- Useful for:
  - Inventory planning  
  - Product bundling  
  - Cross-selling strategies  

---

## ⚠️ Challenges Encountered & How They Were Addressed

### **1. Apriori Support Matrix Errors**
- mlxtend requires a **bool or 0/1 matrix**.
- Fix: convert item counts using  
  `basket = (basket > 0)`

### **2. ROC and Confusion Matrix Display Scaling**
- Large class balance sometimes caused scaling issues.
- Fixed by standardizing through scikit-learn utilities.

### **3. Cluster Interpretability**
- Raw K-Means cluster centers were ambiguous.
- Solved by mapping the scaled centers back to original values for clear interpretation.

### **4. Hyperparameter Tuning Overfitting**
- Large parameter grids initially overfit.
- Fixed by restricting search space (`max_depth` 3–12, `min_samples_split` 2–20).

---

# 📦 Repository Structure

```
MSCS_634_ProjectDeliverable_3/
│
├── deliverable_3.ipynb
├── README.md
├── requirements.txt
├── src/
│   └── utils.py
└── data/
    ├── superstore.csv
    └── cleaned_superstore.csv
```

---

# ▶️ How to Run

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts ctivate
pip install -r requirements.txt
jupyter lab
```

---