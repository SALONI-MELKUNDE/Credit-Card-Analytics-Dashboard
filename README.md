# 💳 Credit Card Analytics Dashboard (Power BI) 📊📈  

This project provides an **interactive Power BI dashboard** for analyzing **credit card transactions** and **customer behavior**.  
It includes KPIs, quarterly and weekly revenue trends, spending categories, card performance, demographics, and segmentation insights.  

---

## 📌 Table of Contents  

1. [Project Objective](#project-objective)  
2. [Project Structure](#project-structure)  
3. [Features](#features)  
4. [Technologies Used](#technologies-used)  
5. [Prerequisites](#prerequisites)  
6. [Setup & Installation](#setup--installation)  
7. [Dashboard Pages](#dashboard-pages)  
8. [DAX Measures](#dax-measures)  
9. [Insights](#insights)  
10. [Future Enhancements](#future-enhancements)  
11. [License](#license)   


---


## 1. Project Objective 🎯  

The objective of this project is to **develop a comprehensive Credit Card Analytics Dashboard** that delivers real-time insights into:  

- **Key Performance Metrics** → Revenue, Interest, Transaction Amount, Transaction Count  
- **Customer Behavior** → Spending patterns, demographics, and segmentation  
- **Card Performance** → Usage by category (Blue, Silver, Gold, Platinum) and channel (Swipe, Chip, Online)  
- **Geographic & Demographic Trends** → Income groups, age groups, states, marital status, and ownership  

This dashboard enables stakeholders to **monitor, analyze, and improve credit card operations effectively** by tracking weekly and quarterly trends, identifying revenue drivers, and detecting growth opportunities.  

---


## 2. Project Structure 🗂️  

```text
/ (root)
├─ Credit Card Dashboard (Power BI).pdf
├─ Credit Card Financial Dashboard-Transaction.pdf
├─ Credit Card Financial Dashboard-Customer.pdf
├─ credit_card.csv
├─ customer.csv
├─ cc_add.csv
├─ cust_add.csv
└─ README.md
```

---


## 3. Features ⭐

**Transaction Report**

- KPIs: Revenue, Interest Earned, Amount, Count
- Quarterly Revenue & Transactions (combo chart)
- Revenue by Expense Type: Bills, Entertainment, Fuel, Grocery, Food, Travel
- Revenue by Education: Graduate, High School, Post-Graduate, Doctorate, Uneducated, Unknown
- Revenue by Customer Job: Businessmen, White-collar, Govt, Self-employed, Blue-collar, Retirees
- Revenue by Use Chip: Swipe, Chip, Online
- Revenue by Card Category: Blue, Silver, Gold, Platinum

**Customer Report**

- KPIs: Revenue, Interest, Income, CSS
- Revenue by Week (line chart)
- Revenue by Income Group (High, Medium, Low — segmented by gender)
- Revenue by Age Group (20–30, 30–40, 40–50, 50–60, 60+)
- Top 5 States: TX, NY, CA, FL, NJ
- Revenue by Customer Job (segmented by gender)
- Revenue by Education Level (segmented by gender)
- Revenue by Marital Status (Married, Single, Unknown)
- Revenue by Dependents
- Car Owner & House Owner Count

---

## 4. Technologies Used 💻

- **Power BI Desktop**
- **CSV files** (credit_card.csv, customer.csv, cc_add.csv, cust_add.csv)
- **MS Excel** (data preparation & CSV exports)
- **MySQL** (data storage and preprocessing before importing to Power BI)  
- **DAX** (Data Analysis Expressions)  
- **Power Query** for data transformations  

---

## 5. Prerequisites 📋

- **MS Excel** installed for initial dataset review and preprocessing. 
- **MySQL Server** installed (for structured data storage).
- **Power BI Desktop** installed.
- Basic knowledge of **SQL**, **Excel**, and **Power BI**.
- Access to datasets (credit_card.csv, customer.csv, cc_add.csv, cust_add.csv).

---

## 6. Setup & Installation 🚀

### A. Import data into SQL database  
- Prepare CSV files.  
- Create tables in MySQL.  
- Import CSV data into SQL tables.  

### B. Export or connect data  
- Use **Excel** or direct **MySQL → Power BI connection**.  
- Alternatively, export processed tables to CSV.  

### C. Load data into Power BI  
- Import `credit_card.csv`, `customer.csv`, `cc_add.csv`, `cust_add.csv`.  
- Use **Power Query** for cleaning & transformations.  

### D. Build relationships between **fact** and **dimension** tables.  
### E. Add **DAX Measures** (see below).

---

## 7. Dashboard Pages 📑  

### Transaction Dashboard  
- **KPIs**: Revenue, Interest, Amount, Count  
- **Quarterly trends** (Revenue vs Transactions)  
- **Revenue by Expense Type**  
- **Revenue by Education**  
- **Revenue by Customer Job**  
- **Revenue by Use Chip** (Swipe, Chip, Online)  
- **Revenue by Card Category** (Blue, Silver, Gold, Platinum)  

### Customer Dashboard  
- **KPIs**: Revenue, Interest, Income, CSS  
- **Weekly revenue line chart**  
- **Revenue by Income Group**  
- **Revenue by Age Group**  
- **Revenue by Education**  
- **Revenue by Customer Job**  
- **Revenue by Marital Status**  
- **Revenue by Dependents**  
- **Car Owner & House Owner segmentation**  
- **Top 5 States** (TX, NY, CA, FL, NJ)  

---

## 8. DAX Measures 🧮  

### Age Group Segmentation
```DAX
AgeGroup = SWITCH(
    TRUE(),
    'customer'[customer_age] < 30, "20-30",
    'customer'[customer_age] >= 30 && 'customer'[customer_age] < 40, "30-40",
    'customer'[customer_age] >= 40 && 'customer'[customer_age] < 50, "40-50",
    'customer'[customer_age] >= 50 && 'customer'[customer_age] < 60, "50-60",
    'customer'[customer_age] >= 60, "60+",
    "Unknown"
)
```

### Income Group Segmentation
```DAX
IncomeGroup = SWITCH(
    TRUE(),
    'customer'[income] < 35000, "Low",
    'customer'[income] >= 35000 && 'customer'[income] < 70000, "Med",
    'customer'[income] >= 70000, "High",
    "Unknown"
)
```

### Weekly KPIs
```DAX
week_num2 = WEEKNUM('cc_detail'[week_start_date])

Revenue = 'cc_detail'[annual_fees] + 
          'cc_detail'[total_trans_amt] + 
          'cc_detail'[interest_earned]

Current_week_Revenue = CALCULATE(
    SUM('cc_detail'[Revenue]),
    FILTER(
        ALL('cc_detail'),
        'cc_detail'[week_num2] = MAX('cc_detail'[week_num2])
    )
)

Previous_week_Revenue = CALCULATE(
    SUM('cc_detail'[Revenue]),
    FILTER(
        ALL('cc_detail'),
        'cc_detail'[week_num2] = MAX('cc_detail'[week_num2]) - 1
    )
)
```

---

## 9. Insights 🔍  

### Week-on-Week Change (Week 53, 31st Dec)  
- Revenue increased by **28.8%**  
- Total Transaction Amount & Count increased  
- Customer count increased  

### Year-to-Date Overview  
- **Overall Revenue** → 57M  
- **Total Interest** → 8M  
- **Total Transaction Amount** → 46M  
- **Male Customers** → 31M revenue  
- **Female Customers** → 26M revenue  
- **Blue & Silver cards** → 93% of overall transactions  
- **Top States** → TX, NY, CA contribute 68%  
- **Activation Rate** → 57.5%  
- **Delinquency Rate** → 6.06%  

---

## 10. Future Enhancements 🔮  
- Automate **data refresh** using Power BI Service.  
- Add **drill-through analysis** for customers.  
- Enhance **forecasting models** for revenue and transactions.  
- Apply **Row-Level Security (RLS)** by state or region.  

---

## 11. License 📜
*This project is licensed under the MIT License. (Include or reference the actual LICENSE file in my repository.)*


## **Thank you for exploring the Credit Card Financial Dashboard Project! 🎉**


