# "Design and Implementation of a Product Recommendation System with Association and Clustering Algorithms"

[![Python 3.7+](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Jupyter](https://img.shields.io/badge/jupyter-notebook-orange.svg)](https://jupyter.org/)
[![ML](https://img.shields.io/badge/machine-learning-red.svg)](https://scikit-learn.org/)


---

## Paper Information

| Detail | Information |
|--------|-------------|
| **Title** | Design and Implementation of a Product Recommendation System with Association and Clustering Algorithms |
| **Authors** | Chibuzor Udokwu, Robert Zimmermann, Farzaneh Darbanian, Tobechi Obinwanne, Patrick Brandtner |
| **Institution** | University of Applied Science Upper Austria |
| **Published in** | Procedia Computer Science 219 (2023) 512–520 |
| **DOI** | [10.1016/j.procs.2023.01.319](https://doi.org/10.1016/j.procs.2023.01.319) |

---

## Overview

This project replicates the key findings of the paper, which proposes a **hybrid machine learning recommendation system** that combines:

- **Apriori Algorithm** — to discover product associations
- **K-Means Clustering** — to identify customer demographic profiles linked to those associations

The system answers:
> *What product associations exist, and what types of customers are connected to each association?*

---

## Dataset

**Online Retail Dataset** (`OnlineRetail.xlsx`)

| Property | Details |
|----------|---------|
| **Source** | UCI Machine Learning Repository |
| **Link** | [Online Retail Dataset](https://archive.ics.uci.edu/dataset/352/online+retail) |
| **Description** | Transactional data from a UK-based online retail store |
| **Records** | 541,909 transactions |
| **Products** | 3,665 unique items |
| **Customers** | 4,338 unique customers |
| **Time Period** | Dec 2010 - Dec 2011 |

> **Note:** The original paper used a private Austrian hygiene retailer dataset. We use the publicly available Online Retail dataset to replicate the methodology.

---

## Methods Used

### Step 1 — Data Preprocessing
- Remove missing CustomerID values
- Remove cancelled orders (invoices starting with 'C')
- Remove negative quantities and unit prices
- Prepare basket format for association rule mining
- Extract customer demographics for clustering

### Step 2 — Finding Optimal K for Clustering
- **Elbow Method** — Plotting inertia for k=2 to 8 to identify the bend point where additional clusters provide diminishing returns
- **Silhouette Score** — Measuring how similar points are to their own cluster versus other clusters (higher score indicates better-defined clusters)
- **Result:** Both methods confirmed optimal k = 4 (silhouette score: 0.390), indicating that 4 customer segments provide the most meaningful grouping without over-segmenting the data

### Step 3 — Association Rule Mining
- **Algorithm:** Apriori (following paper methodology)
- **Minimum Support:** 2% (products must appear together in at least 2% of transactions)
- **Minimum Confidence:** 55% (when customers buy antecedent products, 55% confidence they will also buy consequent products)
- **Rules Generated:** 26 association rules
- **Lift Range:** 6.31 - 24.03 (lift > 1 indicates positive correlation; values > 10 indicate very strong associations)

### Step 4 — Customer Segmentation (K-Means)
- **Clusters:** k=4 (optimal from Step 2)
- **Features:** 7 demographic and behavioral variables (designation, customer group, region, order type, frequency, spending, diversity)
- **Segments Identified:** 4 distinct customer groups with different purchasing behaviors

### Step 5 — Profile-Rule Mapping
- **Mapping:** Connect customer profiles to association rules based on purchasing patterns
- **Connections:** Strong (dominant cluster) and Weak (secondary cluster) connections
- **Visualization:** Network graph (Figure 3) showing relationships between rules and customer profiles

---

## Results

### Optimal K Analysis
The Elbow Method shows diminishing returns after k=4, while the Silhouette Score peaks at k=4 with a score of 0.390, confirming that 4 customer segments is the optimal number for this dataset.

![Optimal K Analysis](results/optimal_k_analysis.png)

### Customer Segments (k=4)
K-Means clustering identified 4 distinct customer segments based on purchasing behavior and demographics. Segment 0 represents the largest group (72.4% of customers), characterized by small volume purchases and low spending.

| Segment | % of Customers | Designation | Region | Order Type | Frequency | Spending |
|---------|---------------|-------------|--------|------------|-----------|----------|
| **Segment 0** | 72.4% | Small Volume | UK_North | Sales Rep | Low | Low |
| **Segment 1** | 7.1% | High Volume | UK_West | Automated | High | High |
| **Segment 2** | 9.2% | Medium Volume | Other | Sales Rep | Low | High |
| **Segment 3** | 11.3% | Medium Volume | UK_West | Web | High | High |

**What this shows:** The dominant customer base (72.4%) makes small, infrequent purchases through sales representatives, while a smaller group of high-volume, high-spending customers use automated ordering systems.

![Customer Segments](results/customer_segments.png)

---

### Table 1: Product Association Rules

The Apriori algorithm generated 26 association rules showing which products are frequently purchased together. Rules with high confidence and lift indicate strong product relationships that can be used for recommendations.

**26 association rules generated** with min_support=2%, min_confidence=55%.

#### Top 10 Rules by Confidence:

| Rule ID | Antecedent 1 | Antecedent 2 | Consequent | Support % | Confidence % | Lift |
|---------|--------------|--------------|-----------|-----------|--------------|------|
| 1 | 22698 | 22699 | 22697 | 2.104 | 89.450 | 23.991 |
| 2 | 22697 | 22698 | 22699 | 2.104 | 84.783 | 20.067 |
| 3 | 22698 | — | 22697 | 2.482 | 82.734 | 22.190 |
| 4 | 22698 | — | 22699 | 2.353 | 78.417 | 18.561 |
| 5 | 22697 | — | 22699 | 2.919 | 78.292 | 18.531 |
| 6 | 23300 | — | 23301 | 2.498 | 72.913 | 17.874 |
| 7 | 22697 | 22699 | 22698 | 2.104 | 72.089 | 24.029 |
| 8 | 22698 | — | 22697 | 2.104 | 70.144 | 24.029 |
| 9 | 22699 | — | 22697 | 2.919 | 69.093 | 18.531 |
| 10 | 22630 | — | 22629 | 2.288 | 68.831 | 18.120 |

**What these rules show:** Products with codes 22697, 22698, and 22699 (likely a set of related items) are strongly associated, with confidence values ranging from 70-89% and very high lift values (20-24), indicating these products are almost always purchased together. This suggests they are complementary products or part of a set.

**Key Statistics:**
- **Rules with confidence > 80%:** 3 rules
- **Rules with confidence > 70%:** 8 rules
- **Rules with lift > 2.0:** 26 rules (100%)
- **Confidence range:** 55.4% - 89.4%
- **Lift range:** 6.31 - 24.03

**Full table:** [results/table1_association_rules.csv](results/table1_association_rules.csv)

---

### Table 2: Customer Profile Summary

After mapping association rules to customer segments, 4 unique customer profiles emerged, each with distinct purchasing characteristics.

| Profile | Rules Connected | Designation | Order Type | Frequency | Spending |
|---------|----------------|-------------|------------|-----------|----------|
| **Profile-B** | 6 | Medium | Sales Rep | High | Low |
| **Profile-D** | 3 | High | Sales Rep | High | High |
| **Profile-F** | 1 | High | Sales Rep | High | High |
| **Profile-I** | 1 | High | Sales Rep | High | Low |

**What this shows:** Profile-B (Medium Volume, Low Spending) is connected to the most rules (6), indicating these customers are most likely to benefit from product recommendations. Profiles D, F, and I represent high-volume customers with varying spending levels, each connected to fewer but still important product associations.

**Full table:** [results/final_summary_profiles.csv](results/final_summary_profiles.csv)

---

### Figure 3: Customer Profile Association Rule Mapping

This network visualization shows the relationships between association rules (blue squares) and customer profiles (green circles). Red lines indicate strong connections where a customer segment is the dominant group for that rule.

- **11 rules** connected to **4 unique profiles**
- **Red lines** = Strong connections (dominant clusters) - these profiles are most associated with these rules
- **Gray dashed lines** = Weak connections (secondary clusters)
- Each rule has exactly **1 strong connection**, demonstrating clear profile-rule relationships

![Figure 3 Network](results/figure3_network_visualization.png)

---

## Key Findings

| Finding | Description | Result |
|---------|-------------|--------|
| **Optimal K** | Best number of clusters confirmed | k=4 (Elbow + Silhouette) |
| **Customer Segments** | 4 segments identified | Confirmed |
| **Dominant Segment** | Segment 0 contains most customers | 72.4% |
| **Product Associations** | 26 association rules generated | Exceeded paper's 11 rules |
| **Association Quality** | Lift values | 6.31 - 24.03 (very strong) |
| **Customer Profiles** | 4 unique profiles identified | Confirmed |
| **Profile-Rule Mapping** | Network graph successfully replicated | Confirmed |

---

## Comparison with Original Paper

| Aspect | Original Paper | Our Replication | Improvement |
|--------|---------------|-----------------|-------------|
| **Dataset** | Austrian hygiene retailer (private) | Online Retail (UCI, public) | Publicly available |
| **Algorithm** | Apriori | Apriori | Replicated |
| **Rules Generated** | 11 rules | 26 rules | +136% |
| **Confidence Range** | 55% - 79% | 55% - 89% | +10% higher max |
| **Lift Range** | 1.75 - 2.34 | 6.31 - 24.03 | 10x stronger |
| **Customer Profiles** | 4 profiles | 4 profiles | Same (more detailed) |
| **Profile-Rule Mapping** | 1 strong per rule | 1 strong per rule | Same |
| **Dataset Size (Products)** | >500 | 3,665 products | 7x larger |
| **Dataset Size (Customers)** | B2B (size not specified) | 4,338 customers | Larger sample |

---

## Repository Structure


## Repository Structure
```
product-recommendation-replication/
│
├── README.md
│
├── research_paper/
│   └── Paper1_main.pdf                      ← Original paper
│
├── dataset/
│   └── OnlineRetail.xlsx                    ← Dataset used
│
├── code/
│   └── preprocessing.ipynb                  ← Main code notebook
│
└── results/
    ├── figure3_replication_results.png      ← Fig 3: Replication network
    ├── figure3_network_visualization.png    ← Fig 3: Enhanced network
    ├── enhanced_profiles_connectivity.png   ← Profile-rule connectivity
    ├── customer_segments.png                ← K-Means segments (k=4)
    ├── optimal_k_analysis.png               ← Elbow + Silhouette plot
    ├── rules_fpgrowth.csv                   ← Association rules table
    └── final_summary_profiles.csv           ← Customer profiles table
```

---

## How to Run

### Prerequisites
- Python 3
- Jupyter Notebook or Google Colab
- Libraries: `pandas`, `mlxtend`, `scikit-learn`, 
`matplotlib`, `networkx`, `openpyxl`

### Install dependencies
```bash
pip install pandas mlxtend scikit-learn matplotlib networkx openpyxl
```

### Steps
1. Download or clone this repository
2. Open `code/preprocessing.ipynb` in Jupyter Notebook or Google Colab
3. Make sure `dataset/OnlineRetail.xlsx` is accessible
4. Run all cells in order

---

##  Dependencies
- `pandas` — data processing
- `mlxtend` — FP-Growth / Apriori algorithm
- `scikit-learn` — K-Means clustering
- `matplotlib` — plotting
- `networkx` — network graph (Figure 3)
- `openpyxl` — reading Excel files
