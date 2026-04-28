# Instacart Market Basket Analysis — Mining Patterns in 3 Million Grocery Orders

**Author:** Hari Krishna Shivanathri  
**Course:** CSCE 676 — Data Mining and Analysis (Spring 2026)  
**Student ID:** 537000542

---

## 📋 Project Summary

This project applies association rule mining, text-based product clustering, and sequential pattern mining to the **Instacart Market Basket Analysis** dataset (~3.4 million orders from 200,000+ users) to uncover actionable purchasing patterns for grocery recommendation systems. We go beyond simple popularity rankings to find co-purchase relationships, latent product groupings hidden in product names, and temporal purchasing sequences.

---

## 🎥 Project Video

> **Video Link:**(https://www.youtube.com/watch?v=KyAHsA2rg-8)

---

## 📓 Main Notebook

👉 **[`main_notebook.ipynb`](main_notebook.ipynb)** — The complete, narrative-driven analysis containing all EDA, research questions, results, and interpretation.

Previous checkpoint notebooks are available in the [`checkpoints/`](checkpoints/) folder for reference.

---

## 🔬 Research Questions

| RQ | Question | Algorithm(s) | Type |
|:---:|---|---|---|
| **RQ1** | What product co-purchases have the highest lift, beyond baseline popularity? | Apriori + Association Rules | Course Technique ✓ |
| **RQ2** | Can TF-IDF keyword extraction from product names reveal latent product groups that the existing aisle/department taxonomy misses? | TF-IDF + K-Means Clustering | Course Technique ✓ |
| **RQ3** | Do temporal purchase sequences (A → B → C) reveal directional patterns that unordered co-occurrence (Apriori) cannot detect? | PrefixSpan Sequential Mining | External Technique ✓ |

---

## 📊 Data Section

### Source
- **Dataset:** [Instacart Market Basket Analysis (Kaggle)](https://www.kaggle.com/datasets/psparks/instacart-market-basket-analysis)
- **Size:** ~3.4 million orders from 200,000+ users across 49,688 products

### Data Files
| File | Description | Rows |
|---|---|---|
| `orders.csv` | Order metadata (user, time, day of week) | ~3.4M |
| `order_products__prior.csv` | Products in prior orders | ~32M |
| `order_products__train.csv` | Products in training orders | ~1.38M |
| `products.csv` | Product names, aisle, department | ~50K |
| `aisles.csv` | Aisle names | 134 |
| `departments.csv` | Department names | 21 |

### Preprocessing
- Merged all tables into a unified DataFrame on shared keys (`order_id`, `product_id`, `aisle_id`, `department_id`)
- Applied **top-200 product filter** for Apriori (RQ1) to manage sparsity (99.92% sparse)
- Used **department-level abstraction** (21 categories) for PrefixSpan (RQ3) to reduce vocabulary
- **Sampled 5,000 users** for sequential mining to ensure Colab feasibility

> **Note:** The dataset CSV files are not included in this repository due to size. Download them from the [Kaggle competition page](https://www.kaggle.com/datasets/psparks/instacart-market-basket-analysis).

---

## 🚀 How to Reproduce

This project was developed in **Google Colab**.

### Option 1: Google Colab (Recommended)
1. Open `main_notebook.ipynb` in Google Colab
2. Upload the Instacart dataset CSV files to the Colab runtime (or mount Google Drive)
3. Run all cells from top to bottom

### Option 2: Local Environment
```bash
# Clone the repository
git clone https://github.com/harikrishna-TAMU/InstaCart-Market-Basket-Analysis.git
cd InstaCart-Market-Basket-Analysis

# Install dependencies
pip install -r requirements.txt

# Download the Instacart dataset from Kaggle and place CSV files in the project root

# Open the notebook
jupyter notebook main_notebook.ipynb
```

---

## 📦 Key Dependencies

| Package | Version | Purpose |
|---|---|---|
| `pandas` | ≥ 2.0.0 | Data manipulation and analysis |
| `numpy` | ≥ 1.24.0 | Numerical computing |
| `matplotlib` | ≥ 3.7.0 | Visualization |
| `seaborn` | ≥ 0.12.0 | Statistical visualization |
| `scikit-learn` | ≥ 1.3.0 | TF-IDF vectorization, K-Means clustering, silhouette analysis |
| `mlxtend` | ≥ 0.22.0 | Apriori algorithm and association rules |
| `prefixspan` | ≥ 0.5.0 | PrefixSpan sequential pattern mining (external technique) |

See [`requirements.txt`](requirements.txt) for the full list.

---

## 🗂️ Repository Structure

```
InstaCart-Market-Basket-Analysis/
├── main_notebook.ipynb              # Main analysis notebook (polished, narrative-driven)
├── README.md                        # This file
├── requirements.txt                 # Python dependencies
├── .gitignore                       # Git ignore rules
└── checkpoints/
    ├── checkpoint_1.ipynb           # Checkpoint 1: EDA & dataset selection
    └── checkpoint_2.ipynb           # Checkpoint 2: RQ formulation & POC runs
```

---

## 📈 Results Summary

### RQ1 — Association Rules (Apriori)
- **178 frequent itemsets** found at min_support = 0.01
- **52 association rules** with lift > 1.5 extracted
- **Top rule:** Large Lemon ↔ Limes (lift = 3.34)
- **Key Insight:** The highest-lift rules involve niche/specialty products, not universally popular items like bananas — confirming that **lift is the right metric** for surfacing non-obvious co-purchase insights

### RQ2 — Product Clustering (TF-IDF + K-Means)
- TF-IDF vectorized **49,688 product names** into 500-feature space (unigrams + bigrams)
- Silhouette analysis identified **k = 14** as optimal cluster count (silhouette score: 0.0584)
- Each cluster has a clear, interpretable theme (e.g., "cheese", "organic/baby", "ice cream", "sauce/pasta", "yogurt", "chocolate", "gluten-free")
- **Key Insight:** TF-IDF clusters reveal **latent product attributes** (organic, flavor, dietary) that the existing aisle/department taxonomy does not capture

### RQ3 — Sequential Pattern Mining (PrefixSpan)
- Constructed temporal purchase sequences for **5,000 sampled users** at department level
- PrefixSpan discovered **directional patterns** like `produce → dairy eggs → snacks` that Apriori cannot detect
- **Key Insight:** Temporal ordering adds value — the **direction of purchase** (A → B, not just A ∧ B) matters for recommendation systems and understanding consumer behavior

### Cross-RQ Comparison
- Apriori finds **what** co-occurs; PrefixSpan reveals **when** (in what order)
- Both approaches are complementary — combining them gives retailers both basket-level and journey-level insights

---

## 📚 References

1. **Instacart Market Basket Analysis.** Kaggle Competition, 2017. [Link](https://www.kaggle.com/c/instacart-market-basket-analysis)
2. Agrawal, R. and Srikant, R. *Fast Algorithms for Mining Association Rules in Large Databases.* VLDB 1994.
3. Pei, J. et al. *PrefixSpan: Mining Sequential Patterns Efficiently by Prefix-Projected Pattern Growth.* ICDE 2001.
4. Raschka, S. *MLxtend: Providing machine learning and data science utilities.* JOSS, 2018.
5. Xie, Y. et al. *An Embedding-Based Grocery Search Model at Instacart.* SIGIR eCom 2022.
