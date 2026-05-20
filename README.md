# Customer Churn Prediction at Telco Industry
### Created By : Nurul Balqis Apriany

---

## Business Problem Understanding

### **Context**
PT Telkom Indonesia is one of the largest telecommunication companies in Indonesia that provides internet and subscription services to customers. One of the major challenges faced by the company is customer churn, which occurs when customers stop using the company’s services.

Customer churn can negatively impact company revenue and increase the cost of acquiring new customers. Therefore, the company wants to identify customers who are likely to churn so preventive actions can be taken earlier.

The available data contains customer demographic information, subscription details, service usage, and billing information which can be used to analyze customer behavior related to churn.

**Target:**
* `0` : Customer does not churn (Retained)
* `1` : Customer churns (Left)

### **Problem Statement**
The company faces difficulties in identifying customers who are likely to stop using the service. Without a prediction system, the company could lose many customers without taking prior preventive actions. 

Moreover, customer retention strategies such as discounts, promotions, and enhanced customer support require additional costs and resources. Therefore, the company needs a way to focus retention efforts on customers who have a high probability of churning.

### **Analytic Approach**
The approach used in this project is supervised learning with classification methods.

1. First, Exploratory Data Analysis (EDA) will be conducted to identify patterns and relationships between customer characteristics and churn behavior.
2. Then, several classification algorithms will be used to build predictive models, such as:
   * Logistic Regression
   * Decision Tree
   * XGBoost
   * Random Forest
3. The performance of each model will then be compared to determine the best model for predicting customer churn.

### **Metric Evaluation**
In this churn prediction case:

* **Type 1 Error : False Positive**
  * *Condition:* The model predicts that a customer will churn, but the customer actually stays.
  * *Consequence:* Unnecessary retention costs, wasted promotions or discounts, and inefficient allocation of company resources.

* **Type 2 Error : False Negative**
  * *Condition:* The model predicts that a customer will stay, but the customer actually churns.
  * *Consequence:* Loss of customers, loss of company revenue, and lost opportunities to retain customers.

Based on these consequences, the company wants to minimize customer loss while maintaining the cost efficiency of retention. Therefore, the main evaluation metrics used are:
* **Recall**
* **F1-Score**
* **ROC-AUC**

Since detecting as many potential churners as possible is crucial for the business, the best model will be selected based on:
* Highest Recall
* Highest F1-Score
* Highest ROC-AUC Score

---

## Data Understanding

This dataset represents the profile of customers who have left a telecommunications company. Churn in telco and other subscription-based services refers to the situation when a customer leaves the service provider.

**Notes:**
- The dataset is used to predict customer churn in a telecommunication company.
- The dataset consists of categorical and numerical features.
- Most features are categorical (Nominal/Binary).
- Each row in the dataset represents a single customer.
- The dataset has a binary classification target:
  - `Yes` = Customer churns
  - `No` = Customer does not churn
- The dataset is moderately imbalanced, where customers who do not churn are more dominant than those who do.

### **Attribute Information**

| Attribute | Data Type | Description |
|---|---|---|
| **Dependents** | Text | Whether the customer has dependents or not |
| **tenure** | Integer | Number of months the customer has stayed with the company |
| **OnlineSecurity** | Text | Whether the customer has online security or not |
| **OnlineBackup** | Text | Whether the customer has online backup or not |
| **InternetService** | Text | Customer’s internet service provider |
| **DeviceProtection** | Text | Whether the customer has device protection or not |
| **TechSupport** | Text | Whether the customer has tech support or not |
| **Contract** | Text | The contract term of the customer |
| **PaperlessBilling** | Text | Whether the customer has paperless billing or not |
| **MonthlyCharges** | Float | The amount charged to the customer monthly |
| **Churn** | Binary/Target | Whether the customer churned or not |

---

## Final Takeaway

The current model is **already effective for deployment**, but its performance can still be improved through better features, deeper error analysis, and model optimization. The system provides a strong foundation for a **data-driven customer retention strategy**.
