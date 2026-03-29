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
- **Apriori Algorithm** — to discover product associations
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

### Step 1 — Association Rule Mining (Apriori)
- Minimum support: 5%
- Minimum confidence: 55%
- Generates product association rules with Support, Confidence, 
Rule Support, and Lift

### Step 2 — Customer Profiling (K-Means Clustering)
- Cluster size: 3 per rule
- Only clusters with size > 30% are accepted
- Demographic variables: Designation, Customer Groups, State, Order Type

### Step 3 — Profile-Rule Mapping
- Maps unique customer profiles to association rules
- Strong (s) and Weak (w) connections identified

---

##  Repository Structure
```
product-recommendation-replication/
│
├── README.md
│
├── research_paper/
│   └── Paper1_main.pdf                  ← Original paper
│
├── dataset/
│   └── OnlineRetail.xlsx                ← Dataset used
│
├── code/
│   └── preprocessing.ipynb              ← Main code notebook
│
└── results/
    ├── figure3_replication_results.png  ← Network graph
    ├── table1_association_rules.csv     ← Association rules
    └── table2_final_profiles.csv        ← Customer profiles
```

---

##  How to Run

### Prerequisites
- Python 3
- Jupyter Notebook or Google Colab
- Libraries: `pandas`, `mlxtend`, `scikit-learn`, `matplotlib`, 
`networkx`, `openpyxl`

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

##  Results

### Table 1 — Product Association Rules
11 association rules were generated with minimum confidence of 55% 
and minimum support of 5%.

See full table: [results/table1_association_rules.csv](results/table1_association_rules.csv)

| Rule ID | Antecedent 1 | Antecedent 2 | Consequent | Confidence % | Lift |
|---------|-------------|-------------|------------|-------------|------|
| 8 | 947 | 993 | 13 | 79.251 | 2.34 |
| 9 | 947 | 660 | 13 | 79.014 | 2.33 |
| 1 | 947 | - | 13 | 77.087 | 2.276 |
| 3 | 562 | 993 | 660 | 65.589 | 2.149 |
| ... | ... | ... | ... | ... | ... |

### Table 2 — Customer Profile Clusters
4 unique customer profiles (A, B, C, D) were identified across 
16 valid profile-rule mappings.

See full table: [results/table2_final_profiles.csv](results/table2_final_profiles.csv)

### Figure 3 — Customer Profile Association Rule Mapping
The network graph below shows which customer profiles are strongly 
(red lines) or weakly connected to each association rule.

![Figure 3](results/figure3_replication_results.png)

---

##  Key Findings

| Finding | Description | Result |
|---------|-------------|--------|
| Product Associations | 11 association rules generated | ✅ Confirmed |
| Top Rule | Rule 8: Product 947 + 993 → 13 (Confidence: 79.25%) | ✅ Confirmed |
| Unique Customer Profiles | 4 unique profiles (A, B, C, D) identified | ✅ Confirmed |
| Dominant Profile | Profile-A strongly connected to most rules | ✅ Confirmed |
| Profile-Rule Mapping | Network graph matches paper's Fig. 3 | ✅ Confirmed |

---

##  Differences from Original Paper

| Aspect | Original Paper | Our Replication |
|--------|---------------|-----------------|
| Dataset | Austrian hygiene retailer (private) | Online Retail (UCI, public) |
| Demographic variables | Designation, Customer Groups, State, Order Type | Available columns in dataset |
| Business context | B2B hygiene retailer | General online retail |

The original dataset used in the paper is **not publicly available** 
(proprietary Austrian retailer data). We used the **Online Retail dataset** 
from UCI as a substitute to replicate the methodology.

---

## 📚 Dependencies
- `pandas` — data processing
- `mlxtend` — Apriori algorithm
- `scikit-learn` — K-Means clustering
- `matplotlib` — plotting
- `networkx` — network graph (Figure 3)
- `openpyxl` — reading Excel files
