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

<img width="580" height="433" alt="image" src="https://github.com/user-attachments/assets/42d84d59-9980-434c-8411-6cfd3d157229" />
<img width="563" height="433" alt="image" src="https://github.com/user-attachments/assets/08fd0aa4-3d95-4a3d-8072-cd409ca53c65" />
<img width="567" height="433" alt="image" src="https://github.com/user-attachments/assets/fde5a0fb-dc86-44bb-9e06-c768de8f4e6b" />
<img width="598" height="433" alt="image" src="https://github.com/user-attachments/assets/c91202e3-3df3-40fe-bc22-0a741250ecdf" />
<img width="563" height="433" alt="image" src="https://github.com/user-attachments/assets/cfd2cf3f-6de8-48dc-88a4-1ec5df3dddf1" />

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

<img width="567" height="453" alt="image" src="https://github.com/user-attachments/assets/8c02a41b-c414-4eae-87fa-d6f4729ede88" />

---

## 🧠 Explainable AI (SHAP)

**Global Insights (SHAP summary):**
- The plot shows that high values for Age (red dots) generally have a positive SHAP value, indicating that older customers are more likely to churn.
- A high number of products (red dots) has a strong positive impact, suggesting that having many products increases a customer's likelihood of churning.
- Higher balances being a strong indicator of a higher churn risk.

<img width="770" height="659" alt="image" src="https://github.com/user-attachments/assets/f333d9dd-805e-4599-a836-08f1ba887b07" />

**Local Explanation (What factors are contributing to a particular customer to churn):**
The following SHAP waterfall plot is displaying (idx = 1) the factors affecting to churn - for 1st custoomer (idx = 1)
<img width="841" height="593" alt="image" src="https://github.com/user-attachments/assets/1f3cba92-4cb5-41cf-827e-901fca557c9f" />

---

## 💡 Business Takeaway
- At the **default 0.5 threshold**, the model missed more than half of churners.  
- By lowering the threshold to **0.165**, the bank can **catch ~80% of churners**, though only ~45% of flagged customers will actually churn.  
- This trade-off makes sense: the cost of retaining loyal customers with small incentives is far lower than the cost of losing high-value churners.  

➡️ **Deploy XGBoost at threshold 0.165** as the operating point for churn flagging.  

---

## 🛠 Tech Stack
- Python, Pandas, NumPy, Matplotlib  
- scikit-learn, XGBoost
- SHAP (explainable AI)  

---

## 🚀 Next Steps
- Incorporate **customer lifetime value (CLV)** to prioritise high-value churners.  
- Test retention strategies with A/B experiments.  
- Deploy model as an API / Streamlit dashboard for customer success teams.  

---
