# Banking Transaction Anomaly Detection & Analysis

## Overview

A Python-based analysis pipeline for banking transaction data that detects anomalies, identifies spending patterns, and surfaces risk signals. Built on real audit experience analyzing SBI branch-level transaction data.

This project demonstrates:
- Exploratory data analysis (EDA) on financial time-series data
- Statistical anomaly detection (Z-score + IQR methods)
- Pattern analysis: merchant trends, category spend, time-based behavior
- Visualisation of findings using Matplotlib and Seaborn

## Business Context

Banks and financial institutions generate millions of transactions daily. Manual review of every transaction is impossible. This pipeline flags:
- Unusual transaction amounts (statistical outliers)
- Velocity anomalies (too many transactions in a short period)
- Dormant account spikes (sudden activity after inactivity)
- Category concentration risk (over-exposure to one merchant/category)

## Project Structure

```
project3_banking_transactions/
│
├── README.md
├── transaction_analysis.py       ← Main analysis + anomaly detection
├── generate_transactions.py      ← Realistic mock transaction generator
├── requirements.txt
├── data/
│   └── transactions.csv
└── output/
    ├── anomaly_flagged.csv        ← Transactions flagged for review
    ├── monthly_summary.csv        ← Monthly spend by category
    ├── analysis_report.txt        ← Narrative findings
    └── charts/
        ├── monthly_spend.png
        ├── category_distribution.png
        ├── anomaly_scatter.png
        └── top_merchants.png
```

## Analyses Performed

| Analysis | Method | Output |
|---|---|---|
| Outlier detection | Z-score > 3 AND IQR × 3 | anomaly_flagged.csv |
| Monthly trend | groupby + resample | monthly_summary.csv + chart |
| Category split | pie/bar chart | category_distribution.png |
| Top merchants | ranked bar | top_merchants.png |
| Velocity check | rolling 24h window | flagged in anomaly file |
| Dormant spikes | 30-day gap + sudden activity | flagged in anomaly file |

## Key Skills Demonstrated
- EDA on financial time-series data
- Dual-method anomaly detection (Z-score + IQR)
- Matplotlib + Seaborn visualisation
- Pandas groupby, resample, rolling window operations
- Documented, reproducible analysis with findings narrative

## How to Run

```bash
pip install -r requirements.txt
python generate_transactions.py
python transaction_analysis.py
```

## Assumptions & Design Decisions

1. Z-score threshold of 3 used for outliers — industry standard for financial fraud detection
2. IQR method applied as a second filter to reduce false positives
3. Velocity check window: 1 hour (configurable). High-frequency micro-transactions in 1 hour flagged
4. Dormant threshold: no transactions for 30+ days, then 3+ in one day = spike
5. Failed/reversed transactions kept in dataset for pattern analysis but excluded from spend totals

## Sample Findings (from mock data)
- 3.2% of transactions flagged as statistical outliers
- Top 3 categories account for 68% of total spend
- December and March show spend spikes (seasonal pattern)
- 2 accounts show dormant-then-spike behavior requiring review
