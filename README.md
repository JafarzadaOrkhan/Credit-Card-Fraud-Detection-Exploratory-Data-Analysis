# 💳 Credit Card Fraud Detection — Exploratory Data Analysis

> Detecting fraud patterns in 10,000 bank transactions using Python

![Dashboard](fraud_analiz.png)

---

## 📌 Project Overview

In this project, I performed **Exploratory Data Analysis (EDA)** on a credit card transaction dataset simulating a real-world banking scenario. The goal was to identify key patterns and risk factors that distinguish fraudulent transactions from legitimate ones.

---

## 🔍 Key Findings

| Metric | Value |
|--------|-------|
| Total transactions | 10,000 |
| Fraud cases | 151 |
| Fraud rate | 1.5% |
| Avg. fraud amount | $216 |
| Avg. normal amount | $175 |

### 🔴 Critical Insights

- **Late night hours (00:00–03:59)** — **81% of all fraud** occurs during these hours
- **Foreign transactions** — fraud risk is **11x higher** than domestic transactions (8.4% vs 0.76%)
- **Grocery category** — leads all merchant categories with 39 fraud cases
- **Amount gap** — fraudulent transactions average **23% higher** than legitimate ones

---

## 📊 Dashboard

The dashboard includes the following charts:

- KPI summary cards
- Fraud count by hour (bar chart)
- Fraud count by merchant category (horizontal bar chart)
- Foreign vs domestic transaction risk (bar chart)
- Average transaction amount comparison (bar chart)
- Fraud vs normal ratio (pie chart)

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-3.x-orange)
![Excel](https://img.shields.io/badge/Export-Excel-217346?logo=microsoft-excel&logoColor=white)

```
pandas       — data manipulation and analysis
matplotlib   — visualization and dashboard creation
openpyxl     — Excel export
```

---

## 🚀 Getting Started

### Requirements
```bash
pip install pandas matplotlib openpyxl
```

### Usage
```bash
# Clone the repository
git clone https://github.com/username/credit-card-fraud-analysis.git
cd credit-card-fraud-analysis

# Run the analysis
python fraud_analiz.py
```

The dashboard will be saved as `fraud_analiz.png`.

---

## 📁 Project Structure

```
credit-card-fraud-analysis/
│
├── fraud_analiz.py            # Main analysis and visualization script
├── fraud_analiz.xlsx          # Excel report (5 sheets)
├── fraud_analiz.png           # Dashboard image
├── credit_card_fraud_10k.csv  # Dataset
└── README.md
```

---

## 📈 Excel Report

The `.xlsx` file contains **5 separate sheets**:

| Sheet | Content |
|-------|---------|
| Summary | KPI cards + key findings |
| By Hour | Hourly fraud statistics + chart |
| By Category | Merchant category breakdown |
| Foreign vs Domestic | Risk comparison |
| Raw Data | Full dataset of 10,000 transactions |

---

## 💡 Methodology

```
1. Data loading and initial inspection
2. Missing value analysis
3. Fraud vs normal transaction comparison
4. Temporal analysis (hourly distribution)
5. Categorical analysis (merchant type)
6. Risk factor analysis (foreign transactions, amount)
7. Visualization and reporting
```

---

## 👤 Author

Actively seeking opportunities in data analytics.
Feel free to connect: [LinkedIn](https://linkedin.com/in/ojafarzada)

---

*This project was built on an open dataset for portfolio purposes.*
