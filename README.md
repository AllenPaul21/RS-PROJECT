# Replication Study: "Design and Implementation of a Product Recommendation System with Association and Clustering Algorithms"

##  Paper
**Title:** Design and Implementation of a Product Recommendation System 
with Association and Clustering Algorithms  
**Authors:** Chibuzor Udokwu, Robert Zimmermann, Farzaneh Darbanian, 
Tobechi Obinwanne, Patrick Brandtner  
**Institution:** University of Applied Science Upper Austria  
**Published in:** Procedia Computer Science 219 (2023) 512–520  
**DOI:** https://doi.org/10.1016/j.procs.2023.01.319

---

##  Overview
This project replicates the key findings of the paper, which proposes a 
**hybrid machine learning recommendation system** that combines:
- **Apriori/FP-Growth Algorithm** — to discover product associations
- **K-Means Clustering** — to identify customer demographic profiles 
linked to those associations

The system answers:
> *What product associations exist, and what types of customers 
are connected to each association?*

---

##  Dataset
**Online Retail Dataset** (`OnlineRetail.xlsx`)  
- Source: UCI Machine Learning Repository  
- Link: https://archive.ics.uci.edu/dataset/352/online+retail  
- Contains transactional data of a UK-based online retail store  
- Used as a substitute for the original Austrian hygiene retailer dataset 
(which is not publicly available)

---

##  Methods Used

### Step 1 — Data Preprocessing
- Cleaned raw Online Retail data
- Removed missing values and duplicates
- Prepared basket format for association rule mining
- Extracted customer demographics for clustering

### Step 2 — Finding Optimal K for Clustering
- Used **Elbow Method** and **Silhouette Score**
- Both methods confirmed **optimal k = 4**
- Silhouette Score was highest at k=4 (~0.39)

### Step 3 — Association Rule Mining
- Applied **FP-Growth Algorithm**
- Minimum support: 5%
- Minimum confidence: 55%
- Generated 11 product association rules

### Step 4 — Customer Segmentation (K-Means)
- Applied K-Means with k=4
- 4 customer segments identified:
  - Segment 0: 72.4% of customers (dominant)
  - Segment 1: 7.1% of customers
  - Segment 2: 9.2% of customers
  - Segment 3: 11.3% of customers

### Step 5 — Profile-Rule Mapping
- Mapped unique customer profiles to association rules
- Strong (red) and Weak (gray) connections identified
- Profile-G connected to most rules (7 rules)
- Profile-A and Profile-F each connected to 6 rules

---

##  Results

### Optimal K Analysis
Elbow Method and Silhouette Score both confirm **k=4** as optimal.

![Optimal K Analysis](results/optimal_k_analysis.png)

---

### Customer Segments (k=4)
4 customer segments were identified using K-Means clustering.

![Customer Segments](results/customer_segments.png)

| Segment | % of Customers |
|---------|---------------|
| 0 | 72.4% (dominant segment) |
| 1 | 7.1% |
| 2 | 9.2% |
| 3 | 11.3% |

---

### Table 1 — Product Association Rules
11 association rules generated using FP-Growth algorithm.

See full table: [results/rules_fpgrowth.csv](results/rules_fpgrowth.csv)

---

### Table 2 — Customer Profile Summary
Final customer profiles mapped to association rules.

See full table: [results/final_summary_profiles.csv](results/final_summary_profiles.csv)

---

### Figure 3 — Customer Profile Association Rule Mapping
#### Version 1: Replication Results
Shows 4 unique customer profiles (B, D, F, I) connected to 11 rules.
- **Red lines** = Strong connections
- **Pink lines** = Weak connections

![Figure 3 Replication](results/figure3_replication_results.png)

#### Version 2: Enhanced Network Visualization
Shows 3 customer profiles (A, B, C) with clearer strong/weak connections.
- **Red lines** = Strong connections
- **Gray dashed lines** = Weak connections

![Figure 3 Network](results/figure3_network_visualization.png)

---

### Enhanced Profile Connectivity
Shows how many rules each customer profile is connected to.
- **Profile-G** — connected to 7 rules (most connected)
- **Profile-A, Profile-F** — connected to 6 rules each
- **Profile-E** — connected to 5 rules
- **Profile-M, Profile-H** — strong connections (red)

![Enhanced Profiles](results/enhanced_profiles_connectivity.png)

---

##  Key Findings

| Finding | Description | Result |
|---------|-------------|--------|
| Optimal K | Best number of clusters confirmed |  k=4 (Elbow + Silhouette) |
| Customer Segments | 4 segments identified |  Confirmed |
| Dominant Segment | Segment 0 contains most customers |  72.4% |
| Product Associations | 11 association rules generated |  Confirmed |
| Customer Profiles | Multiple unique profiles identified |  Confirmed |
| Profile-Rule Mapping | Network graph successfully replicated |  Confirmed |

---

##  Differences from Original Paper

| Aspect | Original Paper | Our Replication |
|--------|---------------|-----------------|
| Dataset | Austrian hygiene retailer (private) | Online Retail (UCI, public) |
| Algorithm | Apriori | FP-Growth (more efficient) |
| Demographic variables | Designation, Customer Groups, State, Order Type | Available columns in dataset |
| Business context | B2B hygiene retailer | General online retail |
| Unique profiles | 4 profiles (A, B, C, D) | Multiple profiles (A–O) |

The original dataset used in the paper is **not publicly available** 
(proprietary Austrian retailer data). We used the **Online Retail dataset** 
from UCI as a substitute to replicate the methodology.

---

##  Repository Structure
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

##  How to Run

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
