<div align="center">

# 🛍️ Customer Shopping Behavior Analysis
### End-to-End Consumer Intelligence, Behavioral Segmentation & BI Analytics

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![MySQL](https://img.shields.io/badge/MySQL-8.0%2B-4479A1?style=flat&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![Power BI](https://img.shields.io/badge/Power_BI-Interactive%20Dashboard-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=flat)](LICENSE)

<p align="center">
  <b>A complete data analytics pipeline transforming 3,900 raw retail transactions into actionable customer segmentation, product performance insights, and revenue growth strategies.</b>
</p>

</div>

---

## 📖 Overview

In modern retail, data-driven decisions are vital for improving customer retention and optimizing marketing spend. This project delivers an end-to-end data analytics workflow on a customer shopping dataset spanning demographics, purchase histories, product categories, discounts, subscriptions, and shipping preferences.

The project follows a structured three-phase pipeline:
1. **Python:** Exploratory data analysis, categorical median imputation, and feature engineering.
2. **MySQL:** Structured relational analysis, customer segmentation CTEs, and window rankings.
3. **Power BI:** Interactive executive dashboard with dynamic filtering across demographic and transactional dimensions.

---

## 🔄 Analytics Workflow

```
┌─────────────────────────┐       ┌─────────────────────────┐       ┌─────────────────────────┐
│     1. PREPARATION      │       │       2. ANALYSIS       │       │    3. VISUALIZATION     │
│       (Python)          │  ───► │         (MySQL)         │  ───► │       (Power BI)        │
│ • Median Imputation     │       │ • CTE Customer Segments │       │ • Interactive KPI Cards │
│ • Age Cohort Binning    │       │ • Window Function Ranks │       │ • Category Heatmaps     │
│ • SQLAlchemy Ingestion  │       │ • Promo Elasticity      │       │ • Demographic Slicers   │
└─────────────────────────┘       └─────────────────────────┘       └─────────────────────────┘
```

---

## ✨ Key Findings & Insights

| Focus Area | Key Data Finding | Business Implication |
| :--- | :--- | :--- |
| **Demographics** | Male shoppers generate **67.7% of total revenue ($157.9k)** vs Female ($75.2k), while average order values remain identical (~$59.80). | Revenue disparity is volume-driven, highlighting a major growth opportunity in female customer acquisition. |
| **Subscriptions** | **73% of repeat customers (>5 orders)** are currently non-subscribers. | Untapped opportunity to convert established loyal shoppers into recurring subscription members. |
| **Promotions** | High-performing hero items (Boots, Gloves) show >40% discount utilization. | Blanket discounts erode product margins; promotions should pivot to order minimums ($75+). |
| **Fulfillment** | Express ($60.10) and Standard ($59.62) shipping maintain virtually identical basket sizes. | Fast shipping options do not cannibalize order volume and can be maintained as premium tiers. |

---

## 🛠️ Tech Stack & Tools

- **Programming & EDA:** Python 3.10+, Pandas, NumPy, Jupyter Notebook
- **Database & ETL:** MySQL 8.0, SQLAlchemy, PyMySQL
- **Business Intelligence:** Microsoft Power BI Desktop (DAX, Interactive Reporting)

---

## 📂 Project Structure

```text
customer-shopping-behavior-analysis/
├── data/
│   └── customer_shopping_behavior.csv         # Raw dataset (3,900 rows × 18 columns)
├── python/
│   └── customer_shopping_behavior_analysis.ipynb # Data cleaning, imputation & MySQL upload
├── sql/
│   └── customer_behavior_analysis.sql         # 10 production SQL business intelligence queries
├── powerbi/
│   └── customer_behavior_dashboard.pbix       # Interactive Power BI dashboard report
├── customer_shopping_behavior_analysis_report.docx # Executive summary & findings report
├── README.md
└── LICENSE
```

---

## 🔬 Implementation Highlights

### 1. Robust Imputation & Feature Engineering (Python)
Missing product review ratings are imputed using category-specific medians to prevent global distortion, and continuous ages are segmented into targeted marketing cohorts:

```python
# Impute missing ratings by category median
df['Review Rating'] = df.groupby('Category')['Review Rating'].transform(
    lambda x: x.fillna(x.median())
)

# Segment customers into standard marketing cohorts
labels = ['Young Adult', 'Adult', 'Middle-aged', 'Senior']
df['age_group'] = pd.cut(df['age'], bins=[17, 30, 45, 60, 100], labels=labels)
```

### 2. Customer Lifecycle Segmentation (SQL CTEs)
Customers are dynamically categorized into **New**, **Returning**, or **Loyal** segments based on historical purchases:

```sql
WITH customer_type AS (
    SELECT 
        customer_id,
        previous_purchases,
        CASE
            WHEN previous_purchases <= 1 THEN 'New'
            WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
            ELSE 'Loyal'
        END AS customer_segment
    FROM customer
)
SELECT 
    customer_segment,
    COUNT(*) AS total_customers,
    ROUND(100.0 * COUNT(*) / (SELECT COUNT(*) FROM customer), 2) AS segment_share_pct
FROM customer_type
GROUP BY customer_segment
ORDER BY total_customers DESC;
```

---

## 📊 Power BI Dashboard Highlights

The interactive dashboard ([`customer_behavior_dashboard.pbix`](powerbi/customer_behavior_dashboard.pbix)) provides an executive overview of core retail KPIs:

- **Executive KPI Cards:** Total Revenue ($233.1k), Customer Count (3,900), Average Spend ($59.76), Average Rating (3.75 / 5.0).
- **Revenue by Age Cohort:** Visual spend contribution across Young Adult, Adult, Middle-aged, and Senior segments.
- **Category & Item Performance:** Category breakdowns across Clothing ($104.3k), Accessories ($74.2k), Footwear ($36.1k), and Outerwear ($18.5k).
- **Interactive Slicers:** Instant multi-attribute filtering by Gender, Category, Subscription Status, and Shipping Method.

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10+
- MySQL Server 8.0+
- Power BI Desktop (Windows)

### Quickstart
```bash
# 1. Clone repository
git clone https://github.com/lavansh4725/customer-shopping-behavior-analysis.git
cd customer-shopping-behavior-analysis

# 2. Set up environment and install dependencies
python -m venv venv
source venv/bin/activate       # On Windows: .\venv\Scripts\activate
pip install pandas numpy sqlalchemy pymysql jupyter

# 3. Run data cleaning & database upload
jupyter notebook python/customer_shopping_behavior_analysis.ipynb

# 4. Execute analytical queries
mysql -u root -p < sql/customer_behavior_analysis.sql
```

---

<div align="center">

### 👨‍💻 Connect with the Author

**Lavansh Jindal**  
*Data Analyst | Software Developer | Full-Stack Developer | Business Intelligence Analyst*

[![GitHub](https://img.shields.io/badge/GitHub-lavansh4725-181717?style=flat&logo=github&logoColor=white)](https://github.com/lavansh4725)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Lavansh%20Jindal-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/lavansh-jindal/)
[![LeetCode](https://img.shields.io/badge/LeetCode-lavansh__0395-FFA116?style=flat&logo=leetcode&logoColor=white)](https://leetcode.com/u/lavansh_0395/)
[![Email](https://img.shields.io/badge/Email-lavansh4725jindal%40gmail.com-D14836?style=flat&logo=gmail&logoColor=white)](mailto:lavansh4725jindal@gmail.com)

<sub>*"Bridging full-stack software development and data analytics — architecting resilient dimensional data warehouses, intelligent GenAI platforms, and high-impact business intelligence."*</sub>

</div>
