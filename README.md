# Project in progress...
Stay Tuned...


**# bank-customer-churn-prediction**# 🏦 

## 📌 Problem Statement
Customer churn is a major challenge in the banking sector and losing valuable customers directly impacts revenue and growth.  
The goal of this project is to **predict which customers are likely to churn** (leave the bank) so retention teams can take proactive actions.  
We focus not only on model accuracy, but also on **recall (catching churners)** and **explainability (why customers churn)** to make the results actionable for business stakeholders.

---

## 📊 Dataset Description
- **Source:** [Bank Churn Modelling Dataset](https://www.kaggle.com/datasets/shrutimechlearn/churn-modelling)  
- **Rows:** 10,000 customers  
- **Features:**
  - `CreditScore`, `Age`, `Tenure`, `Balance`, `NumOfProducts`, `HasCrCard`, `IsActiveMember`, `EstimatedSalary`
  - `Geography`, `Gender`
- **Target:** `Exited` (1 = churned, 0 = stayed)  
- **Churn Rate:** ~20% (imbalanced dataset)

---

## 🔍 EDA Highlights
- **Churn distribution:** Only ~20% customers churn - strong class imbalance.  
- **Tenure & Activity:** Customers with short tenure and inactive accounts churn more often.  
- **Geography:** Churn is higher among customers from Germany.  
- **Balance:** Surprisingly, customers with both very low and very high balances show higher churn.  
- **Age:** Older customers (>50) are more likely to churn.

<img width="580" height="433" alt="image" src="https://github.com/user-attachments/assets/1c8911b1-d7e2-40a2-938e-1703134f3f37" />
<img width="567" height="433" alt="image" src="https://github.com/user-attachments/assets/12c980cb-a71c-4f12-848f-16e8c76d9c50" />
<img width="563" height="433" alt="image" src="https://github.com/user-attachments/assets/fa6ca3f9-97ee-4358-9861-27232c9d1303" />
<img width="598" height="433" alt="image" src="https://github.com/user-attachments/assets/6d8c763e-b646-4d16-a2de-76630e03f6fc" />





---

## 🤖 Model Comparison

| Model                | Accuracy | Precision (Churn=1) | Recall (Churn=1) | F1   | ROC-AUC | PR-AUC |
|-----------------------|----------|---------------------|------------------|------|---------|--------|
| Dummy (majority)      | 0.796    | 0.00                | 0.00             | 0.00 | 0.500   | 0.200  |
| Logistic Regression   | 0.808    | 0.59                | 0.19             | 0.28 | 0.582   | 0.284  |
| Random Forest         | 0.859    | 0.76                | 0.45             | 0.57 | 0.710   | 0.452  |
| XGBoost               | **0.864**| **0.76**            | **0.49**         | **0.59** | **0.724** | **0.474** |

➡️ **XGBoost outperformed other models** on all key metrics, especially recall, F1, and PR-AUC.

---

## 🎯 Threshold Tuning (Recall vs Precision)

Default classification threshold = **0.5**:  
- Recall (churn) = 0.49 → *we miss more than half of churners*.  
- Precision (churn) = 0.76 → very precise, but too many churners slip through.  

After tuning threshold = **0.165**:  
- Recall (churn) = 0.80 → *we now catch 80% of churners*.  
- Precision (churn) = 0.45 → more false positives, but far fewer missed churners.  

<img width="567" height="453" alt="image" src="https://github.com/user-attachments/assets/8ab36b34-4a00-4c7e-be80-b6788125afb7" />


---

## 🧠 Explainable AI (SHAP)

**Global Insights (SHAP summary):**
- Short tenure, being inactive, and living in Spain are the strongest churn drivers.
- Customers with credit cards are more likely to churn.
- Customers with multiple products are more likely to churn.
- 
<img width="438" height="680" alt="image" src="https://github.com/user-attachments/assets/159b70e5-5260-4950-9229-5211dc1c1443" />

**Local Explanation (example customer):**
- Predicted churn probability = 0.78  
- Top churn factors:
  - Short tenure (+0.25 risk)  
  - Not an active member (+0.15 risk)  
  - Geography = France (+0.10 risk)  

<img width="841" height="593" alt="image" src="https://github.com/user-attachments/assets/26fa1429-cf10-4c71-a766-dd514c1448b1" />


---

## 💡 Business Takeaway
- At the **default 0.5 threshold**, the model missed more than half of churners.  
- By lowering the threshold to **0.165**, the bank can **catch ~80% of churners**, though only ~45% of flagged customers will actually churn.  
- This trade-off makes sense: the cost of retaining loyal customers with small incentives is far lower than the cost of losing high-value churners.  

➡️ **Deploy XGBoost at threshold 0.165** as the operating point for churn flagging.  

---

## 🛠 Tech Stack
- Python, Pandas, NumPy, Matplotlib  
- scikit-learn, XGBoost, imbalanced-learn  
- SHAP (explainable AI)  

---

## 🚀 Next Steps
- Incorporate **customer lifetime value (CLV)** to prioritise high-value churners.  
- Test retention strategies with A/B experiments.  
- Deploy model as an API / Streamlit dashboard for customer success teams.  

---
