# Rossmann Retail Intelligence: Sales Forecasting & BI Pipeline
Using 1M+ daily records from 1,115 Rossmann stores, this project ingested transactional data into a serverless PostgreSQL database, built Python machine learning models to forecast sales, and engineered an executive Power BI dashboard to quantify promotional uplift (+37.5%), footfall dynamics, and assortment profitability for data-driven decisions.

---
## 1. Why This Project?

Retail store networks operate on narrow margins where mismatched inventory, inaccurate staffing schedules, and misdirected promotional budgets translate into millions of euros in lost revenue. 

Typical enterprise challenges addressed in this project:
* **The Promotion Dilemma**: Quantifying whether promotional price cuts generate true incremental margin or simply cannibalize full-price baseline spend.
* **Operational Friction**: Managing weekly demand swings—such as the operational surge following statutory Sunday closures.
* **The Analytics Disconnect**: Data science predictive models often remain trapped in isolated Jupyter notebooks, disconnected from store operations. This project bridges raw transactional data directly to an executive decision cockpit.

---

## 2. Dataset Overview

[Rossmann](https://www.kaggle.com/competitions/rossmann-store-sales/data) — one of Europe’s largest drugstore chains store Sales, sourced from Kaggle. Historical Dataset (spanning January 1, 2013 to July 31, 2015) contains 1M+ daily sales records across 1,115 Rossmann stores locations across Germany.

---

## 3. Core Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Cloud Database** | **Neon Serverless PostgreSQL** |
| **DB Management** | **pgAdmin 4** | 
| **ETL & Data Pipeline** | **Python (SQLAlchemy, psycopg2-binary, pandas)** | 
| **Predictive Modeling** | **scikit-learn, numpy** | 
| **Business Intelligence** | **Power BI Desktop** | 
| **Environment** | **Python 3.10+, Google Colab** |
---

## 4. Power BI Dashboard

<p align="center">
  <img src="assets/Dashboard.gif" alt="Rossmann Executive Dashboard Interactive Demo" width="100%" />
</p>


### Executive Architecture & Visual Components
1. **High-Level KPI Banner**: Instant metrics tracking **Total Revenue (€1.39bn)**, **Total Footfall (148M)**, **Average Basket Spend (€9.42)**, and **Promo Uplift % (37.5%)**.
2. **Interactive Year Slicer**: Custom pill buttons (`2013`, `2014`, `2015`) allowing instant global state recalculation.
3. **Weekly Revenue Rhythm**: Horizontal bar breakdown displaying consumer demand patterns chronologically from Monday through Sunday.
4. **Assortment & Promo Uplift Matrix**: Direct comparative table evaluating customer basket spend and overall revenue across product assortments (`Basic`, `Extra`, `Extended`) during `Promo` vs. `Regular` days.
5. **Monthly Revenue vs. Promo Lift %**: Dual-axis combination chart mapping gross monthly revenue alongside campaign efficiency trends.
6. **Store Type Contribution**: Donut distribution isolating revenue splits across store formats (`a`, `b`, `c`, `d`).

---

## 5. End-to-End Methodology
```
1.0M+ Historical Records (1,115 Stores)
               │
               ▼
┌───────────────────────────────┐
│ 1. ETL & Cloud Ingestion      │ ──► Parse dates, filter closures (Open=0)
│    (Python / SQLAlchemy)      │ ──► Impute median CompetitionDistance
└──────────────┬────────────────┘
               │
               ▼
┌───────────────────────────────┐
│ 2. Relational Star-Schema     │ ──► PostgreSQL hosted on Neon Cloud
│    (PostgreSQL)               │ ──► dim_store, dim_date, fct_daily_sales
└──────────────┬────────────────┘
               │
        ┌──────┴──────────┐
        ▼                 ▼
┌──────────────┐    ┌───────────────┐
│ 3A. Feature  │    │ 3B. Semantic  │
│ Engineering  │    │ DAX Modeling  │
│ • 7D/14D lag │    │ • Basket Spend│
│ • Roll means │    │ • Promo Lift  │
│ • Hol. flags │    │ • Day sort    │ 
└──────┬───────┘    └──────┬────────┘
       │                   │
       ▼                   ▼
┌──────────────┐    ┌──────────────┐
│ 4A. Random   │    │ 4B. Power BI │
│ Forest Model │    │ Dashboard    │
│              │    │ • Year pills │
│ • RMSPE eval │    │ • 4-quadrant │
│              │    │ • Cross-filter
└──────┬───────┘    └──────┬───────┘
       │                   │
       └───────┬───────────┘
               │
               ▼
┌───────────────────────────────┐
│ 5. Operational Strategy       │ ──► Capitalize on Monday surge (€0.45bn)
│    & Revenue Optimization     │ ──► Prioritize Store Type 'a' (53% share)
└───────────────────────────────┘
```
