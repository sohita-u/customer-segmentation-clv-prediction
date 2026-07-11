
# Customer Segmentation & 3-Month CLV Prediction

RFM-based customer segmentation and a machine-learning model that predicts each customer's revenue over the next 3 months — built on the UCI **Online Retail** dataset (397,884 transactions, 4,338 customers, Dec 2010–Dec 2011).

The goal: give a retail business a data-driven way to prioritize marketing spend, flag at-risk customers before they churn, and forecast near-term revenue by segment.

---


## Results at a glance

| Metric | Value |
|---|---|
| Customers analyzed | 4,338 |
| Transactions processed | 397,884 |
| Historical revenue captured | £8,911,407.90 |
| CLV model R² | **0.667** |
| CLV model RMSE | £3,493.58 |
| Revenue concentration | Top 26% of customers → ~80% of revenue |
| Highest-value segment (avg. CLV) | VIP — £1,197.02 |
| Segment driving the most **total** revenue | Potential (1,489 customers) |

**Key insight:** average CLV alone is misleading. VIP customers are worth the most individually, but the mid-tier **Potential** segment is 3x larger, so it generates more *total* forecast revenue (~£1.32M vs. VIP's ~£0.64M) — meaning upsell campaigns aimed at Potential customers likely beat VIP-only retention spend on aggregate ROI.

---

## Pipeline

```
Raw transactions (541,909 rows)
        │
        ▼
   Excel cleaning  →  397,884 clean rows, RFM table, pivot analysis
        │
        ▼
  Python: RFM feature engineering
        │
        ▼
  K-Means clustering  →  5 business segments (VIP / Loyal / Potential / At Risk / Lost)
        │
        ▼
  Linear Regression  →  Predicted 3-month CLV per customer
        │
        ▼
  Power BI  →  3-page executive dashboard
```

**Tools:** Microsoft Excel · Python (pandas, scikit-learn, matplotlib, seaborn) · Power BI

---

## Methodology

**1. Data cleaning (Excel)**
Removed missing `CustomerID` rows, cancelled/returned orders, and non-positive quantity/price rows. Engineered a `Revenue` field (`Quantity × UnitPrice`). Built pivot tables for a Pareto (80/20) revenue-concentration check.

**2. RFM feature engineering**
For every customer: **Recency** (days since last purchase), **Frequency** (number of orders), **Monetary** (total spend).

**3. Segmentation (K-Means)**
RFM features scaled with `StandardScaler`, then clustered with `KMeans`. Optimal `k` selected via the elbow method:

<img width="600" src="https://github.com/user-attachments/assets/9bb59d8b-8005-42b0-b5e7-a5c8bc51fb70" />


Clusters were mapped to five business-facing segments using RFM quintile scoring:

<img width="600" src="https://github.com/user-attachments/assets/996ae903-73c5-4652-8b35-ff9fc8b98d69" />


<img width="750" alt="image" src="https://github.com/user-attachments/assets/d57dc8f3-5ac6-473d-8497-cf3c3362652a" />


**4. CLV prediction (Linear Regression)**
A rolling 3-month cutoff splits the data into a feature window and a target window (`FutureRevenue`). Features: Recency, Frequency, Monetary, Average Order Value, Purchase Interval. Model: `LinearRegression`, 80/20 train-test split.

**Result: R² = 0.667, RMSE = £3,493.58** — roughly 67% of variance in next-quarter revenue explained.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/6d2bfd6c-1b21-42da-854e-1ad6cbd53d7a" />

<img width="600" alt="image" src="https://github.com/user-attachments/assets/0f4f92e8-69b1-4e50-aad0-d789781ab86a" />

**5. Revenue concentration**
<img width="600" alt="image" src="https://github.com/user-attachments/assets/9ed04972-dd77-4dd7-88a5-3f6b1611957d" />

**6. Power BI dashboard**
Three pages: Executive Overview, Segmentation Analytics, Strategic Recommendations.

---

## Segment strategy

| Segment | Customers | Avg. Predicted CLV | Suggested Action |
|---|---|---|---|
| VIP | 533 | £1,197.02 | Premium loyalty rewards & exclusive access |
| Loyal | 1,157 | £923.77 | Upsell & cross-sell campaigns |
| Potential | 1,489 | £888.14 | Engagement & personalized offers |
| Lost | 152 | £409.78 | Automated win-back |
| At Risk | 1,007 | £408.95 | Retention & discount campaigns |

---

## Repository structure

```
├── data/               Cleaned dataset + RFM table (Excel)
├── notebooks/           Python notebook: feature engineering, clustering, regression
├── dashboard/            Power BI (.pbix) executive dashboard
├── report/               Full written report (Word)
├── images/                Charts used in this README
└── requirements.txt
```

## Running the notebook

```bash
pip install -r requirements.txt
jupyter notebook notebooks/Customer_Clustering.ipynb
```

## Limitations

- Predicts only a 3-month revenue horizon, not full lifetime value
- No churn-probability model included
- No seasonality adjustment (dataset is Q4-heavy)
- Linear model — doesn't capture non-linear feature interactions

**Next steps:** extend the prediction window to 6–12 months, add a churn classifier, try XGBoost, add SHAP for explainability.

---

## Data source

[Online Retail Dataset — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/352/online+retail)

## Author

**So** — Electronics & Communication Engineering
