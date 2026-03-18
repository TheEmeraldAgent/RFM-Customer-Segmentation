# Customer Segmentation with RFM Analysis & K-Means Clustering

> Segmenting 4,000+ e-commerce customers into actionable groups using RFM scoring and K-Means clustering — with interactive Plotly visualisations and a CRM action plan.

---

## Overview

Most businesses treat all customers the same — same email, same discount, same message. This project builds a data-driven segmentation system that splits customers into 4 distinct groups based on their purchase behaviour, so a CRM team knows exactly how to treat each one.

The segmentation is based on three questions for every customer:
- **Recency** — how recently did they buy?
- **Frequency** — how often do they buy?
- **Monetary** — how much do they spend?

---

## Key Findings

| Finding | Detail |
|---|---|
| Top 18% of customers | Drive 61% of total revenue |
| Champions segment | Avg. spend 8× higher than Hibernating |
| At Risk customers | High past spend — haven't ordered in 90+ days |
| Hibernating segment | Largest group, lowest revenue contribution |

---

## The 4 Segments

| Segment | Customers | Revenue Share | Avg. Spend | Action |
|---|---|---|---|---|
| 🟢 Champions | ~18% | ~61% | High | Reward, loyalty program |
| 🔵 Loyal customers | ~22% | ~24% | Medium-High | Upsell, early access |
| 🟠 At risk | ~25% | ~12% | Medium | Win-back campaign, 15% offer |
| ⚫ Hibernating | ~35% | ~3% | Low | Low-cost reactivation email |

---

## Project Structure

```
rfm-segmentation/
├── data/
│   ├── raw/                        ← original dataset (UCI Online Retail II)
│   └── processed/                  ← cleaned parquet files
├── notebooks/
│   ├── 01_eda.ipynb                ← data loading, validation, cleaning
│   ├── 02_rfm_scores.ipynb         ← RFM calculation, quintile scoring, rule-based segments
│   ├── 03_segmentation.ipynb       ← K-Means clustering, elbow method, silhouette analysis
│   └── 04_visualisation.ipynb      ← segment profiling, Plotly charts, CRM action plan
├── reports/
│   └── figures/                    ← saved plots and interactive HTML charts
│       ├── 04_rfm_3d.html          ← interactive 3D scatter (open in browser)
│       └── 04_treemap.html         ← interactive revenue treemap
├── requirements.txt
└── README.md
```

---

## Notebook Flow

Run notebooks in order — each saves a parquet file that the next one loads.

```
01_eda → data/processed/online_retail_clean.parquet
02_rfm_scores → data/processed/rfm_scored.parquet
03_segmentation → data/processed/rfm_clustered.parquet
04_visualisation → data/processed/rfm_final.parquet + reports/crm_action_plan.csv
```

---

## Methodology

### Data Cleaning
- Removed transactions with no Customer ID (~25% of raw rows)
- Removed cancellations (Invoice starting with 'C') to avoid double-counting
- Removed zero/negative Quantity and Price values
- Result: ~400,000 clean transactions across 4,000+ customers

### RFM Scoring
- Quintile scoring (1–5) per dimension
- Recency scored in reverse — lower days = higher score
- `rank(method='first')` used on Frequency to handle ties in `pd.qcut()`
- Composite score string (e.g. `'543'`) for rule-based segment lookup

### K-Means Clustering
- Log-transform applied to Monetary before clustering to reduce extreme right skew
- Features standardised with `StandardScaler` — ensures equal weight across dimensions
- Optimal k selected via elbow method + silhouette score sweep (k=2 to 10)
- `n_init=10` used to guard against poor random initialisation
- Rule-based segments (notebook 02) compared against K-Means output as validation

### Why SMOTE is not used here
Unlike classification models, clustering does not require class balancing — there is no target variable. Segment imbalance is a finding, not a problem to correct.

---

## Visualisations

| Chart | Description | File |
|---|---|---|
| Revenue donut | Revenue share by segment | `04_revenue_donut.png` |
| Bubble chart | Customer share vs revenue contribution | `04_bubble_chart.png` |
| RFM heatmap | Average R/F/M scores per segment | `04_rfm_heatmap.png` |
| 3D scatter | All customers in RFM space (interactive) | `04_rfm_3d.html` |
| Treemap | Revenue share — proportional rectangles (interactive) | `04_treemap.html` |
| Boxplots | R/F/M distributions per segment | `04_boxplots.png` |

> Open `.html` files directly in your browser for interactive charts — no Python required.

---

## Dataset

**UCI Online Retail II** — real transactional data from a UK-based online retailer.

- 1,067,371 transactions (raw)
- 2 years: Dec 2009 – Dec 2011
- 40 countries
- Source: [UCI ML Repository](https://archive.ics.uci.edu/ml/datasets/Online+Retail+II)

---

## Installation

```bash
git clone https://github.com/TheEmeraldAgent/rfm-segmentation.git
cd rfm-segmentation
pip install -r requirements.txt
```

Then run notebooks in order from `notebooks/`.

---

## Requirements

```
pandas==2.2.2
numpy==1.26.4
scikit-learn==1.4.2
plotly==5.22.0
matplotlib==3.8.4
seaborn==0.13.2
openpyxl==3.1.2
jupyter==1.0.0
```

---

## Skills Demonstrated

`Python` `Pandas` `Scikit-learn` `K-Means Clustering` `RFM Analysis` `Plotly` `Data Cleaning` `Feature Engineering` `Unsupervised Learning` `Customer Analytics` `Data Visualisation`

---

## Author

**Bora Çakır** — Data Analyst  
[GitHub](https://github.com/TheEmeraldAgent) · [LinkedIn](https://linkedin.com/in/TheEmeraldAgent)
