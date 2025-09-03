# 💰 FinanceDemo – 5-Minute Low-Code Finance Demos with Microsoft Fabric  

This repo contains a **tiny Excel dataset (`FinanceDemo.xlsx`)** and **5 simple demos** you can run in under 5 minutes.  
It’s designed for **finance professionals** and **Fabric beginners** — no advanced coding needed.  

---

## 📂 Dataset  

**Transactions (Sheet 1)**  

| Date       | Category | Amount |
|------------|----------|--------|
| 2025-01-31 | Revenue  | 50,000 |
| 2025-01-31 | COGS     | 30,000 |
| 2025-01-31 | Expense  | 13,000 |
| 2025-02-28 | Revenue  | 60,000 |
| 2025-02-28 | COGS     | 35,000 |
| 2025-02-28 | Expense  | 13,200 |

**Budget (Sheet 2)**  

| Month    | RevenueBudget | ExpenseBudget |
|----------|---------------|---------------|
| Jan 2025 | 52,000        | 12,000        |
| Feb 2025 | 62,000        | 13,000        |
| Mar 2025 | 65,000        | 13,500        |

---

## 🛠️ Demos  

### 1️⃣ End-of-Month Automation (No Code)  
- Upload Excel → OneDrive → OneLake  
- Create pipeline → schedule refresh on 1st → send Teams alert  
- ✅ 100% clicks, no coding  

---

### 2️⃣ 📊 Budget vs Actual (DAX)  

```dax
Month = FORMAT(Transactions[Date], "MMM YYYY")
TotalRevenue = SUM(Transactions[Amount])
RevenueVariance = [TotalRevenue] - SUM(Budget[RevenueBudget])



3️⃣ 📈 Month-over-Month Growth (DAX)
RevenueMoM =
DIVIDE(
    SUM(Transactions[Amount]) 
        - CALCULATE(SUM(Transactions[Amount]), PREVIOUSMONTH(Transactions[Date])),
    CALCULATE(SUM(Transactions[Amount]), PREVIOUSMONTH(Transactions[Date]))
)

4️⃣ 🗄️ SQL – Monthly Aggregation
SELECT 
    FORMAT(Date,'yyyy-MM') AS Month,
    Category,
    SUM(Amount) AS TotalAmount
FROM Transactions
GROUP BY FORMAT(Date,'yyyy-MM'), Category
ORDER BY Month, Category;

5️⃣ 📓 Notebook – Budget vs Actual (Python)
import pandas as pd
import matplotlib.pyplot as plt

transactions = pd.read_excel("FinanceDemo.xlsx", sheet_name="Transactions")
budget = pd.read_excel("FinanceDemo.xlsx", sheet_name="Budget")

transactions['Month'] = transactions['Date'].dt.strftime('%b %Y')
actuals = transactions.groupby('Month')['Amount'].sum().reset_index()

budget['Month'] = pd.to_datetime(budget['Month']).dt.strftime('%b %Y')
df = pd.merge(actuals, budget, on='Month', how='left')

plt.bar(df['Month'], df['RevenueBudget'], alpha=0.5, label='Budget')
plt.bar(df['Month'], df['Amount'], alpha=0.5, label='Actual')
plt.title('Revenue: Budget vs Actual')
plt.legend()
plt.show()

📊 Final Visual Outputs
Revenue vs Budget → images/revenue_vs_budget.png
Expense vs Budget → images/expense_vs_budget.png
MoM Growth % → images/mom_growth.png
SQL Aggregation → images/sql_output.png
Notebook Output → images/notebook_budget_vs_actual.png

👨‍💻 Created by: Olufemi Olamoyegun, FMVA®

