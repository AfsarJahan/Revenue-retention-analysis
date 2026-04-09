# 📊 Revenue Intelligence & Retention Diagnostics System

![Python](https://img.shields.io/badge/Python-3.10-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-yellow)
![PowerBI](https://img.shields.io/badge/PowerBI-Dashboard-orange)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)

A product-focused data analytics project designed to diagnose revenue drivers, uncover customer retention issues, and provide actionable strategies for sustainable business growth in an e-commerce environment.

---

## 🚀 Executive Summary

This project analyzes an e-commerce dataset to evaluate whether revenue growth is driven by sustainable customer retention or continuous acquisition.

### 💣 Key Conclusion:
> The business is **heavily acquisition-driven**, with extremely low customer retention (<1%), making long-term growth unsustainable.

---

## 🎯 Business Problem

Despite increasing revenue, the business lacks visibility into:

- Customer retention behavior  
- Repeat purchase patterns  
- Long-term revenue sustainability  

### Core Question:
> **Is growth coming from loyal customers or constant acquisition?**

---

## 🧩 Data Engineering & Integration

- Merged multiple relational datasets (orders, customers, payments, items)
- Built a unified dataset for analysis

```python
df = orders.merge(payments, on='order_id', how='left')
df = df.merge(customers, on='customer_id', how='left')
df = df.merge(order_items, on='order_id', how='left')
```

---

## 📈 1️⃣ Revenue Trend Analysis

- Aggregated revenue at monthly level  
- Analyzed growth patterns  

```python
df['month'] = df['order_purchase_timestamp'].dt.to_period('M')
monthly_revenue = df.groupby('month')['payment_value'].sum()
```

### 📌 Insight:
- Strong upward trend with fluctuations  
- Seasonal spikes indicate demand cycles  

---

## 👤 2️⃣ Customer Segmentation

Customers segmented into:

- **New Users** → Single purchase  
- **Repeat Users** → Multiple purchases  

```python
customer_orders = df.groupby('customer_unique_id')['order_id'].nunique().reset_index()
customer_orders['segment'] = customer_orders['total_orders'].apply(
    lambda x: "New" if x == 1 else "Repeat"
)
```

### 📌 Insight:
- ~97% of customers are one-time buyers  
- Extremely low repeat engagement  

---

## 💰 3️⃣ Revenue Contribution Analysis

```python
revenue_segment = df.groupby('segment')['payment_value'].sum()
```

### 📌 Insight:
- Revenue dominated by new customers  
- Repeat users contribute smaller but meaningful share  

---

## 🔁 4️⃣ Cohort Analysis (Retention Tracking)

Tracked customer retention over time based on first purchase month.

```python
df['order_month'] = df['order_purchase_timestamp'].dt.to_period('M')

cohort = df.groupby('customer_unique_id')['order_month'].min().reset_index()
cohort.columns = ['customer_unique_id', 'cohort_month']

df = df.merge(cohort, on='customer_unique_id')

cohort_data = df.groupby(['cohort_month', 'order_month'])['customer_unique_id'].nunique().reset_index()
cohort_pivot = cohort_data.pivot(index='cohort_month', columns='order_month', values='customer_unique_id')
```

---

## 📉 5️⃣ Retention Modeling (Advanced)

```python
def normalize_row(row):
    first = row[row.first_valid_index()]
    return row / first

retention = cohort_pivot.apply(normalize_row, axis=1) * 100
```

### 📌 Insight:
- Retention drops below **1% after first purchase**  
- Severe churn across all cohorts  

---

## 🔍 Key Findings

- 📈 Revenue is growing but unstable  
- 👤 ~97% customers are one-time buyers  
- 🔁 Repeat customers are extremely low  
- 📉 Retention < 1% after first purchase  
- ⚠️ Business relies heavily on acquisition  

---

## 💣 Core Business Insight

The business is scaling revenue through continuous customer acquisition rather than building long-term customer relationships.

> This leads to:
- High churn  
- Low customer lifetime value (CLV)  
- Unsustainable growth model  

---

## 💡 Strategic Recommendations

- Improve post-purchase engagement  
- Introduce loyalty & retention programs  
- Focus on repeat purchase behavior  
- Optimize Customer Lifetime Value (CLV)  
- Reduce dependency on acquisition  

---

## 📊 Dashboard (Power BI)

This project includes an interactive dashboard covering:

- Revenue trends over time  
- Customer segmentation  
- Revenue contribution  
- Cohort retention analysis  

📌 Designed for **decision-making, not just visualization**

---

## 🧠 Skills Demonstrated

- Data Cleaning & Feature Engineering  
- Exploratory Data Analysis (EDA)  
- Customer Segmentation  
- Cohort & Retention Analysis  
- Business Insight Generation  
- Data Storytelling  

---

## ⚙️ Tech Stack

- Python (Pandas, NumPy)  
- Matplotlib  
- Power BI  
- Jupyter Notebook  

---

## 📌 Business Impact

Even a small improvement in retention can:

- Increase revenue significantly  
- Reduce acquisition costs  
- Improve long-term sustainability  

---

## 🔮 Future Work

- Churn prediction models  
- Customer lifetime value modeling  
- A/B testing for retention strategies  
- Demand forecasting  

---

## 👤 Author

**Afsar Jahan Shaik**  
M.Sc. Statistics & Data Analytics  

---

⭐ If you found this project useful, feel free to connect or give feedback!
