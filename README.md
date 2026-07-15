# Customer Segmentation & RFM Analytics Dashboard

A Power BI dashboard applying RFM (Recency, Frequency, Monetary) analysis to segment 500 customers based on ~3,350 transactions, identifying which customers to reward, retain, or win back.

![Dashboard overview](dashboard_overview.png)

## Overview

RFM analysis is a customer segmentation technique widely used in marketing analytics to identify high-value customers and at-risk relationships without needing complex predictive modeling. It scores each customer on three dimensions:

- **Recency** — how recently they last purchased
- **Frequency** — how often they purchase
- **Monetary** — how much they've spent in total

Each customer is scored 1-5 on each dimension (5 = best) using quintile-based scoring, then assigned to a behavioral segment based on their combined R/F/M profile.

## Segments

| Segment | Profile | Suggested action |
|---|---|---|
| **Champions** | Recent, frequent, high spend | Reward loyalty, early access to new products |
| **Loyal Customers** | Consistent purchase history | Upsell, cross-sell, referral programs |
| **At Risk** | Historically valuable, going quiet | Win-back campaigns before they churn fully |
| **Lost** | Long inactive, low historical value | Low-cost re-engagement or deprioritize |
| **New Customers** | Very recent, limited history | Onboarding nurture sequences |
| **Occasional** | Irregular, moderate activity | Targeted promotions to build habit |

## Key findings

- **Champions** (114 customers, 22.8%) generate **€364,712** — by far the largest share of total revenue (€529,985) — despite being less than a quarter of the customer base.
- **Lost** customers (91, 18.2%) show an average recency of **325 days** — nearly 11 months without a purchase — with average frequency of just 2.13 orders, confirming genuine churn rather than a slow purchase cycle.
- **At Risk** customers (92, 18.4%) still show meaningful historical value (€705.59 average monetary, 6.36 average frequency) but recency has slipped to 227.99 days — a clear win-back opportunity before they convert to Lost.
- **New Customers** (71, 14.2%) have very low recency (29.65 days) as expected, but also the lowest average order frequency (1.55) — the segment most sensitive to onboarding quality.

## Data model

| Table | Description |
|---|---|
| `Customers` | 500 customers — country, acquisition channel, signup date |
| `Transactions` | ~3,350 individual purchase transactions |
| `CustomerRFM` | Pre-calculated RFM values, R/F/M scores (1-5), and final segment per customer |

Relationships: `Transactions[CustomerID]` → `Customers[CustomerID]` (many-to-one); `CustomerRFM[CustomerID]` → `Customers[CustomerID]` (one-to-one).

## Methodology note

RFM scores were calculated using quintile binning (`pandas.qcut`) on Recency, Frequency, and Monetary values, then mapped to segments using a rule-based classification (e.g. R≥4 & F≥4 & M≥4 → Champions). This pre-calculation was done in Python; the Power BI layer handles aggregation, visualization, and interactive filtering by segment.

## Dashboard layout

- **KPI cards** — Total Revenue, Total Customers, Avg Order Value
- **Donut chart** — customer count by segment
- **Bar chart** — total revenue by segment
- **Table** — average Recency, Monetary, and Frequency per segment

## Tools

- Power BI Desktop (data model, DAX measures, report)
- Python (pandas, numpy) — synthetic dataset generation and RFM scoring

## Files

```
marketing-rfm-dashboard/
├── README.md
├── marketing_rfm_dashboard.pbix
├── dashboard_overview.png
└── data/
    ├── Customers.csv
    ├── Transactions.csv
    └── CustomerRFM.csv
```

## Note on data

All data in this project is synthetically generated for portfolio demonstration purposes and does not represent any real company or customers.
