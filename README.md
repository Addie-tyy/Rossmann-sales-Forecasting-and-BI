# Rossmann-sales-Forecasting-and-BI
End-to-end retail sales forecasting and executive business intelligence pipeline. Features predictive ML modeling in Python and an interactive Dark Obsidian Power BI dashboard analyzing €2.3B+ revenue, promo uplift, and footfall dynamics.
# Rossmann Retail Intelligence: Sales Forecasting & BI Pipeline

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Power BI](https://img.shields.io/badge/Power_BI-Executive_Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Machine Learning](https://img.shields.io/badge/Scikit--Learn-Random_Forest-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![Theme](https://img.shields.io/badge/UI_Design-Dark_Obsidian-10B981?style=for-the-badge)](https://github.com/)

> **An end-to-end retail intelligence pipeline integrating machine learning sales forecasting with an executive-grade Power BI dashboard to evaluate multi-year revenue drivers, promo elasticity, and footfall patterns across 1,115 Rossmann store locations.**

---

## Project Overview

This project delivers a full-stack retail analytics pipeline using historical sales, footfall, and operational data across **1,115 Rossmann store locations** (2013–2015). 

The workflow bridges two core functions:
1. **Predictive Analytics Pipeline (`.ipynb`)**: End-to-end data cleaning, calendar and lag feature engineering, promotional uplift evaluation, and regression modeling to forecast store revenues.
2. **Executive Business Intelligence Dashboard (`.pbix`)**: A custom-designed, multi-page executive interface styled in a bespoke **Dark Obsidian** aesthetic (`#142920` canvas, Emerald `#10B981`, and Lime `#A3E635` accents) built to track high-level revenue health and store behavior.

---

## Executive Power BI Dashboard Showcase

![Rossmann Executive Dashboard Overview](assets/dashboard_preview.png)
*(Replace with `assets/dashboard_demo.gif` for interactive button showcase)*

### Layout & Core Architecture
* **Top KPI Banner**: Real-time evaluation of **Total Revenue (€ bn)**, **Total Footfall (M)**, **Average Basket Spend (€)**, and **Promo Uplift %**.
* **Interactive Year Navigator**: Custom-styled pill buttons (`2013`, `2014`, `2015`) allowing instant global state recalculation without blocking canvas visual flow.
* **Monthly Revenue vs. Promo Lift %**: Dual-axis combination chart tracking seasonal volume alongside campaign elasticity.
* **Store Type Distribution**: High-contrast donut visual breaking down macro revenue contribution by store classification (`a`, `b`, `c`, `d`).
* **Weekly Revenue Rhythm**: Horizontal bar breakdown displaying consumer traffic cycles chronologically from Monday through Sunday.
* **Assortment & Promo Uplift Matrix**: Direct comparative matrix isolating regular vs. promotional performance across product assortment tiers (`Basic`, `Extra`, `Extended`).

---

## Key Business Insights & Findings

1. **Monday Volume Peak vs. Sunday Closures**:
   * Revenue exhibits a pronounced intra-week surge on **Mondays (€0.45bn)** driven by pent-up consumer demand following regular Sunday statutory closures (**€0.01bn**). 
   * Mid-week trade remains steady (averaging **€0.36bn–€0.39bn**), recovering into Friday before weekend drop-offs.
2. **Store Type `a` Dominance**:
   * Store Type `a` serves as the primary revenue backbone, driving **over 53% to 54% of total enterprise sales** across all tracked periods.
   * Store Type `b` records the lowest aggregate volume but maintains distinctive basket spend dynamics, indicating distinct target demographics.
3. **Promotional Uplift & Basket Spend Dynamics**:
   * Active promotional campaigns consistently maintain a **~37% to 38% revenue uplift**.
   * Average basket spend increases systematically under active promotions (e.g., Assortment `a` spends elevate from **€8.48** in regular periods to **€9.73** during campaigns; Assortment `c` elevates from **€9.39** to **€10.70**).
4. **Assortment Tier Performance**:
   * Assortments `a` (Basic) and `c` (Extended) drive the bulk of operational revenue, each exceeding **€300M+** in seasonal runs, while Assortment `b` (Extra) operates at lower overall volume but supports strategic localized inventory.

---

## Data Modeling & Core DAX Measures

The Power BI data model connects normalized tables (`dim_store`, `dim_date`, `fct_daily_sales`) via strict 1-to-many star-schema relationships.

### 1. Total Revenue
```dax
Total Revenue = SUM(fct_daily_sales[Sales])
