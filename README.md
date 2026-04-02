**Case Scenario: ConnectTel Retention Crisis**

**Company Background**: ConnectTel is a mid-sized telecommunications provider offering phone, internet, and streaming services. Over the past quarter, customer churn has risen from 18% to 27%, costing an estimated $2.1M in lost recurring revenue.

**You are a Business Analytics Consultant hired to:**
* Diagnose why customers are leaving
* Build a model to predict high-risk customers
* Recommend data-driven retention strategies

**5 W**
- *who* - the customers
- *what* - churn rate rose from 18% to 27%, costing an estimated of 2.1M doller
- *when* - past quater
- *where*  - 
- *why* - 
- *how* - identify high risk customers

# 📊 Telco Customer Churn – Data Encoding Documentation

## 🔧 Overview

This document explains the preprocessing and encoding steps applied to categorical features in the dataset. The goal is to convert categorical variables into numerical form suitable for machine learning models.

---

## 🧹 1. Data Cleaning

### ✔ Steps Performed

* Converted `TotalCharges` to numeric:

  ```python
  df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce')
  ```
* Removed rows with invalid/missing values:

  ```python
  df = df.dropna()
  ```
* Dropped irrelevant identifier column:

  ```python
  df = df.drop(columns=['customerID'])
  ```

---

## 🎯 2. Target Variable Encoding

### Variable: `Churn`

* Type: Binary categorical (Yes / No)
* Encoding Method: **Label Encoding**

| Original | Encoded |
| -------- | ------- |
| No       | 0       |
| Yes      | 1       |

---

## 📊 3. Ordinal Encoding

### 🔹 What is Ordinal Data?

Ordinal variables have a **meaningful order**.

### Variable Encoded:

* `Contract`

### Categories Order:

| Category       | Meaning     |
| -------------- | ----------- |
| Month-to-month | Short-term  |
| One year       | Medium-term |
| Two year       | Long-term   |

### Encoding Mapping:

| Contract Type  | Encoded Value |
| -------------- | ------------- |
| Month-to-month | 0             |
| One year       | 1             |
| Two year       | 2             |

✔ Reason: Longer contracts imply stronger customer commitment → natural order exists.

---

## 🔄 4. Nominal Encoding (One-Hot Encoding)

### 🔹 What is Nominal Data?

Nominal variables have **no inherent order**.

### Encoding Method:

* **OneHotEncoder (drop='first')**
* Avoids multicollinearity (Dummy Variable Trap)

---

## 🧩 Nominal Features Encoded

### 👤 Customer Demographics

* `gender`
* `Partner`
* `Dependents`

### 📞 Services

* `PhoneService`
* `MultipleLines`
* `InternetService`

### 🔐 Add-on Services

* `OnlineSecurity`
* `OnlineBackup`
* `DeviceProtection`
* `TechSupport`

### 📺 Streaming Services

* `StreamingTV`
* `StreamingMovies`

### 💳 Billing Information

* `PaperlessBilling`
* `PaymentMethod`

---

## 🔍 Example Transformations

### Example: `InternetService`

| Original Value | Encoded Column                 |
| -------------- | ------------------------------ |
| DSL            | (Dropped as baseline)          |
| Fiber optic    | InternetService_Fiberoptic = 1 |
| No             | InternetService_No = 1         |

---

### Example: `PaymentMethod`

| Original Value   | Encoded Column                    |
| ---------------- | --------------------------------- |
| Bank transfer    | (Dropped baseline)                |
| Credit card      | PaymentMethod_Creditcard = 1      |
| Electronic check | PaymentMethod_Electroniccheck = 1 |
| Mailed check     | PaymentMethod_Mailedcheck = 1     |

---

### Example: Binary Features

| Feature | Original | Encoded         |
| ------- | -------- | --------------- |
| Partner | Yes      | Partner_Yes = 1 |
| Partner | No       | 0               |

---

## ⚠️ Special Cases Handled

### "No Internet Service"

Some features contain:

* `"No internet service"`

✔ Handled as separate category:

```
OnlineSecurity_Nointernetservice
StreamingTV_Nointernetservice
```

✔ This preserves:

* Business meaning
* Dependency between features

---

## 🧠 Final Feature Structure

### Total Columns After Encoding: **30**

### Feature Types:

| Type            | Features                             |
| --------------- | ------------------------------------ |
| Ordinal         | Contract                             |
| One-Hot Encoded | ~25 columns                          |
| Numerical       | tenure, MonthlyCharges, TotalCharges |
| Target          | Churn                                |

---

## 📈 Why This Approach?

### ✔ Advantages

* Handles categorical data correctly
* Avoids ordinal misinterpretation
* Prevents multicollinearity
* Keeps features interpretable

### ❗ Important Design Choice

Instead of removing feature names:

```
Yes ❌
```

We used:

```
Feature_Category ✅
```

✔ Ensures:

* Unique columns
* Better debugging
* Model interpretability

---

## 🚀 Summary

| Encoding Type    | Use Case                      |
| ---------------- | ----------------------------- |
| Label Encoding   | Target variable               |
| Ordinal Encoding | Ordered categories (Contract) |
| One-Hot Encoding | Unordered categories          |

---

## 🧩 Next Steps (Recommended)

* Feature scaling (StandardScaler)
* Train ML models (Logistic Regression, Random Forest)
* Feature importance analysis
* Model evaluation (ROC-AUC, Precision, Recall)

---

**End of Document**



# ConnectTel Churn Reduction Strategy: Recommendations

## PROBLEM
- **Current churn rate**: 27% (↑9 percentage points QoQ)  
- **Quarterly revenue impact**: $2.1M  
- High churn is primarily driven by dissatisfaction with internet service quality, billing friction, and price sensitivity.

## KEY INSIGHTS
1. **Fiber Optic customers churn significantly more** (strongest correlation: 0.308)  
   → Despite being a premium service, Fiber users are leaving at a much higher rate, likely due to higher MonthlyCharges combined with perceived service or reliability issues.

2. **Electronic Check is the riskiest payment method** (correlation: 0.302)  
   → Customers using Electronic Check have substantially higher churn. This points to payment friction, lack of auto-renewal convenience, and higher likelihood of service cancellation.

3. **Higher MonthlyCharges and PaperlessBilling strongly predict churn** (correlations 0.193 and 0.192)  
   → Price-sensitive and digitally engaged customers are more likely to switch providers when bills feel too high or when switching is frictionless.

## RECOMMENDATIONS

✅ **High-Impact / Low-Effort**:  
**Launch targeted "Switch to Auto-Pay" campaign for Electronic Check + Fiber Optic users**  
→ Expected impact: Reduce churn by 4–6% in targeted segment within one quarter (based on payment method coefficient strength).

✅ **High-Impact / High-Effort**:  
**Review and optimize Fiber Optic pricing + service bundles** (e.g., introduce loyalty discounts for 1- or 2-year contracts)  
→ Timeline: 8–10 weeks  
→ Expected impact: Address the root cause of the highest correlation driver (InternetService_Fiber optic).

✅ **Quick Win**:  
**Offer retention incentives (1–2 months discount or free add-on) to customers flagged by the model as high-risk (predicted churn probability > 60%)**  
→ Owner: Retention & Marketing Team  
→ Focus on SeniorCitizen segment with extra care to avoid bias.

## NEXT STEPS
- **Pilot test** Recommendation #1 (Auto-Pay campaign) with 5,000 high-risk customers (Fiber + Electronic Check) by **April 20, 2026**.
- **Monitor weekly**: Churn rate in pilot group vs control group; success threshold: **≥ 15% relative reduction** in churn.
- Run fairness audit on SeniorCitizen predictions before full rollout.
- Schedule model retraining and A/B testing of retention offers in Q3.

**Prepared using Logistic Regression baseline model**  
**Precision: 58.07%** | **Overall Accuracy: 76.72%**  
Focus on **Precision** ensures retention budget is spent on customers who are truly at risk of leaving.

---
**Note**: SeniorCitizen shows moderate correlation (0.151). All retention actions targeting this group must be monitored for fairness to avoid unintended discrimination.