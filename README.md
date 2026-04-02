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
