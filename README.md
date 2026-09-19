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


### Executive Architecture
1. **KPI Banner**: Instant metrics tracking **Total Revenue (€1.39bn)**, **Total Footfall (148M)**, **Average Basket Spend (€9.42)**, and **Promo Uplift % (37.5%)**.
2. **Year Slicer**: Buttons (`2013`, `2014`, `2015`) allowing instant global state recalculation.
3. **Weekly Revenue Rhythm**: Displaying consumer demand patterns chronologically from Monday through Sunday.
4. **Assortment & Promo Uplift Matrix**: Direct comparative table evaluating customer basket spend and overall revenue across product assortments (`Basic`, `Extra`, `Extended`) during `Promo` vs. `Regular` days.
5. **Monthly Revenue vs. Promo Lift %**: Dual-axis combination chart mapping gross monthly revenue parallely with campaign efficiency trends.
6. **Store Type Contribution**: Donut chart showing revenue splits across store formats (`a`, `b`, `c`, `d`).

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
## 6. Methods & Analytical Techniques

### Exploratory Data Analysis & Statistical Profiling (Python)
* **Distribution Skewness & Log Transformation**: Identified strong right-skewness in raw store daily sales distributions; evaluated target normalization strategies to stabilize variance across high-volume vs. low-volume store clusters.
* **Store Closure & Zero-Sales Segmentation**: Segmented trading versus non-trading periods (`Open == 0` vs. `Open == 1`); isolated mandatory Sunday statutory closures to eliminate zero-inflated distortion without losing structural calendar signals.
* **Competitor Proximity Analysis**: Evaluated the non-linear relationship between `CompetitionDistance` and revenue density; discovered that stores with nearby competitors (<1,000m) often exhibited higher baseline revenue due to prime, high-density urban footfall locations.
* **Holiday Impact & Demand Spikes**: Analyzed calendar anomalies across `StateHoliday` types (National, Easter, Christmas) and `SchoolHoliday` windows, quantifying pre-holiday pantry-loading versus post-holiday demand lulls.
* **Multi-Collinearity & Correlation Screening**: Screened store metadata features using correlation heatmaps and Variance Inflation Factor (VIF) checks to remove redundant promotional interval markers.

### Feature Engineering & Data Preparation
* **Target Leakage Prevention (Customer Footfall)**: Intentionally excluded the `Customers` column from sales forecasting features, as real-time customer counts are unavailable at the time of future inference, preventing unrealistic predictive leakage.
* **Autoregressive Lags & Rolling Statistics**: Engineered historical sales lags ($t-7$, $t-14$, $t-21$, $t-28$) to capture cyclical day-of-week seasonality, alongside 7-day, 14-day, and 30-day rolling averages and rolling standard deviations to capture localized demand momentum and volatility.
* **Categorical Encoding & Pipeline Transformations**: Implemented target-safe encoding for high-cardinality categorical variables (`StoreType`, `Assortment`, `StateHoliday`) and imputed missing `CompetitionDistance` values using grouped medians conditioned on `StoreType`.
* **Promotional Duration & Temporal Flags**: Deconstructed promotional cycles into continuous time features—calculating months since competitor opening and weeks active in rolling `Promo2` campaigns.

### Predictive Modeling & Performance Validation
* **Temporal Cross-Validation Strategy**: Avoided randomized train-test splits (which induce look-ahead bias in time series); implemented strict out-of-time chronological validation (training on historical data and validating on the final forward weeks).
* **Ensemble Learning Architecture**: Trained an ensemble **Random Forest Regressor** to model high-dimensional non-linear interactions between promotion status, store density, seasonality, and rolling historical sales.
* **Dual Evaluation Metrics (RMSPE & RMSE)**:
  * **Root Mean Squared Error (RMSE)**: Monitored to penalize large absolute currency deviations on peak volume trading days.
  * **Root Mean Squared Percentage Error (RMSPE)**: Optimized as the primary retail evaluation benchmark to treat percentage forecast error symmetrically across small rural outlets and high-turnover flagship locations:
    $$\text{RMSPE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} \left(\frac{y_i - \hat{y}_i}{y_i}\right)^2}$$

### Semantic Modeling & DAX Formulation (Power BI)
* **Average Basket Spend**:
  ```dax
  Avg Basket Spend = DIVIDE([Total Revenue], SUM(fct_daily_sales[Customers]), 0)
