# 🕵️‍♂️ Fraud Detection & Risk Management Dashboard 🚨

*"Spotting the fraud, protecting the flow."*

An interactive Power BI dashboard analyzing 100,000 credit card transactions to detect fraud patterns, quantify financial losses, and profile the customers, merchants, and regions most exposed to risk. Built for fraud analysts and risk teams who need to act on data-driven signals, not gut feeling.

---

## 🎯 Objectives

- ✅ **Fraud Analysis** — analyze suspicious transactions and measure the financial losses they cause
- ✅ **Demography Analysis** — evaluate how fraud is distributed across geographic regions and customer segments
- ✅ **Key Insights** — surface actionable recommendations and the report's headline analytical findings

## 📈 Business Questions Addressed

**🔍 Fraud & Transaction Analysis**
- Which spending categories generate the most fraudulent transactions?
- How does fraud volume trend over time (daily/date-level)?
- What share of total transaction value is lost to fraud?

**🗺️ Demographic & Geographic Analysis**
- Which U.S. states have the highest concentration of fraud?
- How does fraud break down by customer gender, age group, and job?
- Which merchants and merchant locations are most associated with fraud?

**⚙️ Risk & Customer Impact**
- What percentage of customers have ever been victims of fraud?
- What percentage of merchants have processed at least one fraudulent transaction?
- What is the overall fraud rate across all transactions?

## 🧠 Data Model — Star Schema

| Table | Description |
|---|---|
| **fact_transactions** | Central fact table — transaction amount, timestamp, fraud flag, foreign keys |
| **dim_customer** | Customer profile — name, gender, job, date of birth, age group, location |
| **dim_merchant** | Merchant name and location (lat/long) |
| **dim_category** | Spending category (e.g. Grocery, Shopping, Travel) |
| **dim_date** | Transaction date hierarchy, weekend flag |

All four dimension tables connect to the fact table on a single-direction M:1 relationship (`category_key`, `customer_key`, `merchant_key`, `date_key`).

## 💡 Key KPIs

| KPI | Value |
|---|---|
| 🔢 Total Transactions | 100,000 |
| 🚨 Fraudulent Transactions | 402 |
| 📉 Overall Fraud Rate | 0.40% |
| 💰 Total Transaction Volume | $6.93M |
| 💸 Total Fraud Amount | $203.2K |
| 📊 Fraud Share of Total Amount | 2.93% |
| 👥 Customers | 911 |
| ⚠️ Customers with ≥1 Fraud Case | 44 |
| 🏪 Merchants | 693 |
| ⚠️ Merchants with ≥1 Fraud Case | 252 |
| 🛍️ Top Fraud Category | Shopping Net |
| 📍 Top Fraud State | North Carolina |
| 📅 Data Period | June 21 – July 26, 2020 |

## 💻 Key DAX Measures

```DAX
-- Total Fraudulent Transactions
total fruad = SUM(fact_transactions_1_[fruad_num])

-- Total Transaction Amount
Total Amount = SUM('fact_transactions_1_'[amount])

-- Fraud Amount
Fraud Amount =
CALCULATE(
    SUM('fact_transactions_1_'[amount]),
    'fact_transactions_1_'[is_fraud] = "fruad"
)

-- Fraud Rate (share of transactions that are fraudulent)
Fraud Rate =
DIVIDE(
    SUM('fact_transactions_1_'[fruad_num]),
    COUNT('fact_transactions_1_'[transaction_key]),
    0
)

-- Fraud Amount as % of Total Amount
Amount fraud percentage =
fact_transactions_1_[Fraud Amount] / fact_transactions_1_[Total Amount]

-- Distinct Customers Affected by Fraud
customer fruad num =
CALCULATE(
    DISTINCTCOUNT(fact_transactions_1_[customer name]),
    fact_transactions_1_[fruad_num] = 1
)

-- % of Customers Affected by Fraud
Fraud Cusromer Percentage =
[customer fruad num] / dim_customer_1_[customer num]

-- Distinct Merchants Affected by Fraud
merchant fruad num =
CALCULATE(
    DISTINCTCOUNT(fact_transactions_1_[merchant name]),
    fact_transactions_1_[fruad_num] = 1
)

-- % of Merchants Affected by Fraud
merchant fruad % = [merchant fruad num] / [merchant num]
```

## 🎨 Dashboard Design

Dark, high-contrast theme built for a "risk & alert" feel:

| Purpose | Color |
|---|---|
| Background | `#1A1512` |
| Panels / Secondary | `#2A2320` |
| Primary Accent | `#7A2020` |
| Highlight / Alert | `#E85D5D` |
| Soft Highlight | `#F5A3A3` |
| Text | `#B5B0AC` |

## 📊 Report Structure

| Page | Description |
|---|---|
| **Introduction** | Title page with report objectives and navigation |
| **Fraud analysis** | Fraud KPIs, fraud count by category, fraud trend over time, U.S. state fraud map |
| **Demography_Analysis** | Fraud by job, gender, age group, top merchants, merchant location map |
| **Key Insights** | Summary KPI cards, top fraud categories table, fraud-by-state pivot, closing takeaways |


## 🧰 Tech Stack

- Power BI Desktop
- Power Query (data cleaning & modeling)
- DAX (measures)
- Star schema data modeling

---

*Built to help risk and fraud teams turn transaction data into action.*
