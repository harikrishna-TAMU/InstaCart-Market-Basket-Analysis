# Instacart Market Basket Analysis  
**CSCE 676: Data Mining — Spring 2026** **Checkpoint 1 — Dataset Comparison, Selection, and Exploratory Data Analysis (EDA)**

This repository contains the work for **Checkpoint 1** of the course project by Hari Krishna Shivanathri.  
The goal of this checkpoint is to understand the **Instacart Market Basket Analysis** dataset and extract insights that affect **frequent itemset mining**, **association rules**, and **temporal sequential mining** (such as sparsity, long-tail popularity bias, and implicit feedback).

---

## Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Repository Structure](https://github.com/harikrishna-TAMU/InstaCart-Market-Basket-Analysis/edit/main/README.md#repository-structure-)
- [How to Run](https://github.com/harikrishna-TAMU/InstaCart-Market-Basket-Analysis/edit/main/README.md#how-to-run-)
- [EDA Results](https://github.com/harikrishna-TAMU/InstaCart-Market-Basket-Analysis/edit/main/README.md#eda-results-)
- [Key Findings Summary](https://github.com/harikrishna-TAMU/InstaCart-Market-Basket-Analysis/edit/main/README.md#key-findings-summary-)
- [Next Steps](https://github.com/harikrishna-TAMU/InstaCart-Market-Basket-Analysis/edit/main/README.md#next-steps-)
- [Citation](https://github.com/harikrishna-TAMU/InstaCart-Market-Basket-Analysis/edit/main/README.md#citation-)

---

## Project Overview
Market basket analysis is challenging because:
- product purchases follow a **long-tail / popularity bias**,
- the user-item interaction matrix is incredibly **sparse** (**cold start**),
- we only have **implicit feedback** (what was bought and the `reordered` flag, not explicit ratings),
- purchase intervals and restocking behavior matter (**temporal effects**).

In this checkpoint, we run EDA on the Instacart dataset to quantify these issues and prepare for building data mining models (like Apriori and FP-Growth) in later checkpoints.

---

## Dataset
**Dataset:** Instacart Market Basket Analysis DataSet (SIGIR eCom 2022 / Kaggle)

### Expected path used in notebook
Place the dataset `.csv` files in the same directory as the notebook (or adjust paths as needed). The core files required are:

orders.csv
products.csv
order_products__train.csv
aisles.csv
departments.csv

---

## Repository Structure:- 

.
├── 537000542_project_checkpoint1.ipynb
├── README.md
└── assets/
    └── figures/
        ├── item_support_longtail.png
        ├── basket_size_distribution.png
        └── sparsity_map.png

## How to Run:-

#Google Colab

Upload the dataset .csv files to your Colab workspace.

Open 537000542_project_checkpoint1.ipynb in Google Colab.

Run all cells from top to bottom. (Note: The notebook loads a 100k row subset initially to ensure it runs fast in Colab memory).

## EDA Results:- 

1) Item Support Distribution (The Long Tail)

What this shows: The frequency (support) of items ranked by popularity follows a strict "Power Law" distribution.

Why it matters: This long-tail distribution proves the absolute necessity of setting a Minimum Support threshold. Frequent itemset mining algorithms (like Apriori) only work effectively if we can filter out the massive volume of niche items in the tail.

2) Basket Size Distribution

What this shows: The distribution of the number of items purchased per order.

Why it matters: Apriori's computational complexity depends heavily on basket size. Since the average basket size is relatively small and manageable, standard frequent itemset mining techniques remain computationally feasible for this dataset.

3) User-Item Matrix Sparsity

What this shows: A visual representation of the non-zero interactions between users/orders and products.

Why it matters: Our sparsity audit calculated a matrix sparsity of 99.92%. Most users buy only a tiny fraction of the available catalog, creating a massive cold-start problem. This severely limits standard collaborative filtering without significant pre-processing.

## Key Findings Summary:- 

Popularity Bias: Strong long-tail behavior in item support necessitates Minimum Support thresholds.

Algorithmic Feasibility: The average basket size makes Apriori and FP-Growth algorithms viable.

Data Sparsity: The user-item matrix is 99.92% sparse, making standard collaborative filtering difficult.

Implicit Feedback: Without explicit 1-5 star ratings, models must rely on the reordered flag as a proxy for user satisfaction.

## Next Steps:-

Threshold Sensitivity: Analyze how the distribution of Rule Lift changes as the Minimum Support threshold is lowered in a 99% sparse environment.

Sequential vs. Static: Evaluate whether Sequential Pattern Mining approaches can outperform static Frequent Itemset Mining in predicting next-basket contents.

Feature-Based Clustering: Test if clustering products by textual keywords (via product_name) before running Association Rules reveals stronger cross-department patterns than using raw Product IDs.

## Citation:-

If you use this dataset or findings, please cite:

"An Embedding-Based Grocery Search Model at Instacart". SIGIR eCom 2022

Instacart Market Basket Analysis DataSet Documentation / Kaggle Competition (2017).
