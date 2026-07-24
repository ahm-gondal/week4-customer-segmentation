# Week 4 — Unsupervised Learning & Customer Segmentation

**Data Analysis & Business Understanding — Internship Task 4**

## Project Overview

Unlike previous weeks, there is **no target column**. The goal is unsupervised: discover natural segments hidden in the behaviour of ~9,000 credit-card customers, so a business can target marketing, manage risk, and personalize services — one of the most common uses of ML in banking. The work has two parts: **K-Means** clustering (Part 1) and **Hierarchical** clustering (Part 2), then a comparison of the two.

## Dataset

- **Name:** Credit Card Dataset for Clustering ([Kaggle: arjunbhasin2013/ccdata](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata))
- **Size:** 8,950 customers × 18 columns (behaviour over 6 months)
- **Features:** balance, purchases, one-off vs installment purchases, cash-advance amount/frequency, purchase frequency, credit limit, payments, minimum payments, % full payment, tenure, etc.

## Environment Setup

```bash
git clone https://github.com/ahm-gondal/week4-customer-segmentation.git
cd week4-customer-segmentation
pip install -r requirements.txt
jupyter notebook notebooks/week4_clustering.ipynb
```

The notebook auto-loads the dataset from `data/` (or the working directory) and runs top to bottom.

## Approach

1. **Preprocessing** — dropped `CUST_ID` (identifier, not behaviour); imputed the only two columns with missing values (`MINIMUM_PAYMENTS` 313 rows, `CREDIT_LIMIT` 1 row) with the **median** (robust to the heavy right-skew of financial data); scaled all 17 features with **StandardScaler** (mandatory — clustering is distance-based, and unscaled features like credit limit would otherwise dominate).
2. **K-Means** — ran k = 2…10, recording inertia and silhouette; plotted the **elbow curve** and **silhouette curve**; chose **k = 4** (at the elbow and business-interpretable); profiled clusters with a mean-per-feature **heatmap**.
3. **Hierarchical** — Ward-linkage **dendrogram** on a 300-row sample with a cut line; `AgglomerativeClustering` with the same k = 4; **cross-tabulation** vs K-Means; comparison report.

## Key Results

| Item | Value |
|---|---|
| Customers × features (after dropping CUST_ID) | 8,950 × 17 |
| Missing values imputed (median) | 314 cells (MINIMUM_PAYMENTS, CREDIT_LIMIT) |
| Silhouette peak | k = 3 (0.251); k = 4 chosen for interpretability |
| Chosen clusters (K-Means, k=4) | 3,977 / 409 / 1,197 / 3,367 |
| Chosen clusters (Hierarchical, k=4) | 4,875 / 300 / 1,194 / 2,581 |

### The four customer segments (from the profiling heatmap)

- **Dormant / low-activity** — low balance, few purchases; keep a card but rarely transact. *Re-activation offers.*
- **Prime high spenders** — high purchases (~£7.7k avg), high credit limit (~£9.7k), pay their balances, low cash advance. Most valuable, lowest risk. *Rewards, upsell, cross-sell.*
- **Cash-advance revolvers** — high cash advance (~£4.5k avg) and balance, few purchases, low full-payment. Borrow rather than shop; higher risk. *Monitor risk, structured loans.*
- **Mainstream moderate users** — around-average behaviour; the broad middle. *Standard engagement, nurture toward prime.*

### K-Means vs Hierarchical
Both algorithms independently rediscovered the same core segments (agreement shown by the cross-tabulation), which is strong evidence the structure is real. **Recommendation: K-Means for production** — it scales to millions of customers, can instantly assign new customers with `predict()`, and gave cleaner, more balanced clusters here. Hierarchical clustering is a great supporting tool: its dendrogram intuitively shows how many segments exist.

## Repository Structure

```
├── notebooks/
│   └── week4_clustering.ipynb   # Executed, all outputs visible
├── data/
│   ├── DataSet(W4).csv.xls      # Dataset
│   └── customers_with_clusters.csv  # Output: customers + cluster labels
├── charts/                      # Exported figures (elbow, silhouette, heatmap, dendrogram, crosstab)
├── Week4_Documentation.docx     # Full written documentation
├── README.md
└── requirements.txt
```

## Tools

Python 3 · pandas · NumPy · scikit-learn (KMeans, AgglomerativeClustering, StandardScaler, silhouette) · SciPy (dendrogram) · Matplotlib · Seaborn
