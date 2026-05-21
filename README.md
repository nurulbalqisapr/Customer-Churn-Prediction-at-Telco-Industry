# Customer Churn Prediction at Telco Industry

### Created By: Nurul Balqis Apriany

---

## Project Overview

This project aims to predict customer churn in the telco industry using Machine Learning.  
Customer churn happens when customers stop using the company's service.

In a subscription-based business like internet service, churn can reduce recurring revenue. Therefore, the company needs a prediction system to identify high-risk churn customers earlier and support more targeted retention strategies.

This project uses a public telco customer churn dataset as a business case representation, not actual company data.

---

## Business Problem

The company has difficulty identifying customers who are likely to stop using the service.

Without a prediction system, customers may leave before the company can take preventive action. However, retention strategies such as discounts, promotions, or loyalty offers require additional cost.

Therefore, the main business question is:

> How can the company identify high-risk churn customers and make retention efforts more targeted and cost-efficient?

---

## Objectives

The objectives of this project are:

- Predict customers who are likely to churn
- Identify key factors that influence churn
- Compare several classification models
- Select the best model based on churn detection performance
- Estimate the business impact of using Machine Learning
- Provide data-driven business recommendations

---

## Dataset Information

The dataset contains customer profile, subscription, service usage, billing, and churn information.

### Target

| Target | Meaning |
|---|---|
| 0 | Customer does not churn |
| 1 | Customer churns |

### Dataset Summary

| Description | Value |
|---|---:|
| Initial records | 4,930 |
| Duplicate rows removed | 77 |
| Final records analyzed | 4,853 |
| Churn customers | 1,288 |
| Non-churn customers | 3,565 |
| Churn proportion | 26.5% |

The dataset is moderately imbalanced because non-churn customers are more dominant than churn customers.

---

## Key EDA Insights

### 1. Month-to-Month Contract is the Main Churn Driver

Customers with month-to-month contracts have the highest churn risk.

This shows that customers with short-term contracts have lower commitment and are easier to lose.

### 2. Higher Monthly Charges Increase Churn Risk

Customers who churn tend to have higher monthly charges.

This indicates that pricing pressure can influence churn, especially when customers feel the service value is not equal to the cost paid.

### 3. Fiber Optic Customers Show Higher Churn Risk

Fiber Optic customers show higher churn count compared to other internet service types.

This may indicate higher customer expectations regarding price, service quality, or network stability.

### 4. Long Tenure and Add-on Services Help Reduce Churn

Customers with longer tenure, long-term contracts, and add-on services such as Online Security, Device Protection, and Tech Support tend to be more loyal.

---

## Data Preprocessing

The preprocessing steps include:

- Removing duplicate rows
- Checking missing values
- Encoding the target variable
- Splitting data into train and test set
- Scaling numerical features
- Encoding categorical features
- Applying preprocessing using ColumnTransformer and Pipeline

### Preprocessing Pipeline

| Feature Type | Columns | Method |
|---|---|---|
| Numerical | tenure, MonthlyCharges | StandardScaler |
| Categorical | Dependents, Contract, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, PaperlessBilling | OneHotEncoder |
| Target | Churn | LabelEncoder |

Pipeline was used to make sure preprocessing is applied consistently during training and testing.

---

## Modeling

This project uses supervised learning with a classification approach because the target is binary: churn or not churn.

Several models were compared:

- Logistic Regression
- Decision Tree
- XGBoost
- Random Forest

The main evaluation metric is **Recall** because the business wants to detect as many churn customers as possible.

In this case, False Negative is more costly because it means the model fails to detect customers who actually churn.

---

## 5-Fold Cross Validation Result

| Rank | Model | Mean Recall CV Score |
|---:|---|---:|
| 1 | Logistic Regression | 0.795 |
| 2 | Decision Tree | 0.749 |
| 3 | XGBoost | 0.650 |
| 4 | Random Forest | 0.457 |

## Best Model

**Logistic Regression** was selected as the final model because it achieved the highest and most stable Recall score during cross validation.

Logistic Regression was also chosen because it is interpretable, meaning the model can explain which features increase or decrease churn risk.

---

## Hyperparameter Tuning

After selecting Logistic Regression, hyperparameter tuning was performed using GridSearchCV.

### Tuning Setup

| Component | Description |
|---|---|
| Method | GridSearchCV |
| Cross Validation | 5-fold |
| Scoring | Recall |
| Class Weight | balanced |

### Best Parameters

| Parameter | Best Value |
|---|---|
| C | 0.001 |
| Solver | liblinear |
| class_weight | balanced |

The tuning process focused on maximizing Recall so the model can detect more potential churn customers.

---

## Final Model Performance

The final Logistic Regression model was evaluated on 971 unseen test customers.

| Metric | Score |
|---|---:|
| Accuracy | 74.4% |
| Precision | 51.1% |
| Recall | 80.2% |
| F1-Score | 62.4% |
| ROC-AUC | 76.2% |

### Interpretation

The model successfully captures around 80% of actual churn customers.

Precision is lower because the model intentionally flags more customers as potential churners. This may create extra retention cost, but it helps reduce the risk of missing real churn customers.

---

## Feature Importance Interpretation

The Logistic Regression model shows two main patterns:

### Churn Drivers

Features that increase churn risk:

- Month-to-month contract
- Higher Monthly Charges
- Fiber Optic internet service

These features indicate low commitment, price pressure, and higher service expectation.

### Retention Factors

Features that reduce churn risk:

- Longer tenure
- One-year or two-year contract
- Online Security
- Device Protection
- Tech Support

These features increase customer commitment, service value, and customer dependency.

---

## Model Limitations

Although the model performs well, it still has limitations.

The model captures about 80% of churn customers, but still misses around 20% of actual churners.

This happens because:

- Customer behavior data is limited
- Complaint history is not fully available
- Payment delay information is not included
- Network quality data is not included
- External factors such as competitor offers are unknown
- There is a trade-off between Recall and Precision

The model is most reliable for customers with similar patterns to the training data, especially based on contract type, tenure, monthly charges, internet service type, and add-on services.

This model should be used as a decision support system, not as a perfect prediction tool.

---

## Business Impact

Machine Learning helps change retention strategy from general assumption into targeted action.

### Confusion Matrix Result

| Result | Count |
|---|---:|
| True Negative | 515 |
| False Positive | 198 |
| False Negative | 51 |
| True Positive | 207 |

### Business Impact Summary

| Metric | Value |
|---|---:|
| Revenue Loss Without Model | $19.28K |
| Revenue Loss With Model | $13.23K |
| Revenue Saved | $6.05K |
| Efficiency Gain | 31.37% |
| Churners Detected | 207 |
| Missed Churners | 51 |

The model helps reduce estimated revenue loss by $6.05K or 31.37% efficiency gain by identifying high-risk churn customers earlier.

---

## Business Recommendations

### 1. Targeted Retention

Focus retention campaigns on customers predicted as high-risk churners, especially customers with:

- Month-to-month contracts
- High monthly charges
- Fiber Optic service

### 2. Contract Migration Campaign

Encourage month-to-month customers to move to yearly contracts through:

- Loyalty rewards
- Limited-time discounts
- Bundled offers

### 3. Value-Added Service Bundling

Offer add-on service bundles such as:

- Online Security
- Device Protection
- Tech Support

This can increase perceived value and reduce churn risk.

### 4. Pricing Strategy Review

Review pricing strategy for high-cost customers, especially Fiber Optic users.

The company can provide personalized offers or loyalty discounts to reduce price sensitivity.

### 5. Model Improvement

To reduce missed churners, future improvement should include additional data such as:

- Customer complaint history
- Payment delays
- Customer service tickets
- Network quality
- Usage behavior

---

## Conclusion

Customer churn is mainly influenced by customer commitment, pricing pressure, service type, and value-added services.

The final Logistic Regression model is stable, interpretable, and effective for churn prediction. It achieves 80.2% Recall, meaning it can detect most churn customers.

From the business perspective, the model helps reduce estimated revenue loss by $6.05K and improves retention efficiency by 31.37%.

Overall, this project provides a strong foundation for a data-driven customer retention strategy in the telco industry.

---

## Repository Structure

```text
Customer-Churn-Prediction-at-Telco-Industry/
│
├── CapstoneProject_Module3_Nurul Balqis Apriany.ipynb
├── data_telco_customer_churn.csv
└── README.md
