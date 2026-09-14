# Rossmann Retail Intelligence: Sales Forecasting & BI Pipeline

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Neon_Serverless-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://neon.tech/)
[![Power BI](https://img.shields.io/badge/Power_BI-Executive_Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Random_Forest-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![UI Theme](https://img.shields.io/badge/UI_Design-Dark_Obsidian-10B981?style=for-the-badge)](https://github.com/)

> **An end-to-end retail intelligence pipeline integrating machine learning sales forecasting with an executive-grade Power BI dashboard to evaluate multi-year revenue drivers, promo elasticity, and footfall patterns across 1,115 Rossmann store locations.**

---

## 1. Why This Project?

Retail store networks operate on narrow margins where mismatched inventory, inaccurate staffing schedules, and misdirected promotional budgets translate into millions of euros in lost revenue. 

Typical enterprise challenges addressed in this project:
* **The Promotion Dilemma**: Quantifying whether promotional price cuts generate true incremental margin or simply cannibalize full-price baseline spend.
* **Operational Friction**: Managing weekly demand swings—such as the operational surge following statutory Sunday closures.
* **The Analytics Disconnect**: Data science predictive models often remain trapped in isolated Jupyter notebooks, disconnected from store operations. This project bridges raw transactional data directly to an executive decision cockpit.

---

## 2. Dataset Overview

* **Source**: Rossmann Store Sales Historical Dataset (spanning January 1, 2013 to July 31, 2015).
* **Scope**: Over 1,000,000 daily sales records across 1,115 pharmacy/drugstore locations across Germany.
* **Core Tables & Schema Design**:
  * `fct_daily_sales`: Daily store transactional records containing `Sales`, `Customers` (footfall), `Open` flags, `Promo` status, and `StateHoliday` indicators.
  * `dim_store`: Store metadata including `StoreType` (`a`, `b`, `c`, `d`), `Assortment` tier (`Basic`, `Extra`, `Extended`), `CompetitionDistance`, and promotion cycle markers (`Promo2`).
  * `dim_date`: Extracted calendar lookup table indexing `DayOfWeek`, `Month`, `Quarter`, and `Year`.

---

## 3. Core Tech Stack

| Layer | Technology | Function |
| :--- | :--- | :--- |
| **Cloud Database** | **Neon Serverless PostgreSQL** | Cloud-native relational storage hosting star-schema tables. |
| **DB Management** | **pgAdmin 4** | Schema enforcement, table indexing, and relational integrity. |
| **ETL & Data Pipeline** | **Python (SQLAlchemy, psycopg2-binary, pandas)** | Ingestion, data cleaning, datetime extraction, and automated loading. |
| **Predictive Modeling** | **scikit-learn, numpy** | Feature engineering, time-lag creation, and Random Forest regression. |
| **Business Intelligence** | **Power BI Desktop (Dark Obsidian Theme)** | DAX modeling, custom UX design, dynamic slicing, and interactive analysis. |
| **Version Control** | **Git & GitHub** | Source code management, documentation, and asset distribution. |

---

## 4. Executive Power BI Dashboard

<p align="center">
  <img src="assets/dashboard_demo.gif" alt="Rossmann Executive Dashboard Interactive Demo" width="100%" />
</p>

### Custom Design Language: "Dark Obsidian"
The report avoids default dashboard templates in favor of a bespoke visual identity built for low eye strain and high visual hierarchy:
* **Canvas Background**: Deep Obsidian Forest (`#142920`)
* **KPI Containers**: Bordered cards (`#1C3B2E`) with a `12px` rounded radius
* **Data Accents**: Emerald Green (`#10B981`) for standard operational volumes; Neon Lime (`#A3E635`) for active promotion benchmarks
* **Typography**: Crisp White (`#FFFFFF`) for primary callouts; Soft Slate (`#94A3B8`) for secondary labels and axes

### Executive Architecture & Visual Components
1. **High-Level KPI Banner**: Instant metrics tracking **Total Revenue (€1.39bn)**, **Total Footfall (148M)**, **Average Basket Spend (€9.42)**, and **Promo Uplift % (37.5%)**.
2. **Interactive Year Slicer**: Custom pill buttons (`2013`, `2014`, `2015`) allowing instant global state recalculation.
3. **Weekly Revenue Rhythm**: Horizontal bar breakdown displaying consumer demand patterns chronologically from Monday through Sunday.
4. **Assortment & Promo Uplift Matrix**: Direct comparative table evaluating customer basket spend and overall revenue across product assortments (`Basic`, `Extra`, `Extended`) during `Promo` vs. `Regular` days.
5. **Monthly Revenue vs. Promo Lift %**: Dual-axis combination chart mapping gross monthly revenue alongside campaign efficiency trends.
6. **Store Type Contribution**: Donut distribution isolating revenue splits across store formats (`a`, `b`, `c`, `d`).

---

## 5. End-to-End Methodology
