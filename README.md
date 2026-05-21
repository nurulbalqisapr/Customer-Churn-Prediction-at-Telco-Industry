# Customer Churn Prediction at Telco Industry

### Created By: Nurul Balqis Apriany

---

## Business Problem Understanding

### Context

PT Telkom Indonesia is one of the largest telecommunication companies in Indonesia that provides internet and subscription-based digital services through IndiHome.

In a subscription-based business, customer retention is very important because the company generates recurring revenue from monthly subscription payments and additional service packages. When a customer churns, the company does not only lose one customer, but also loses potential recurring revenue in the future.

Customer churn refers to the condition when customers stop using the company's services. Therefore, identifying customers who are likely to churn is important so that the company can take preventive actions earlier.

This project uses a public telco customer churn dataset as a business case representation, not actual company data.

**Target:**

- `0` : Customer does not churn / retained
- `1` : Customer churns / left

---

## Problem Statement

The company has difficulty identifying customers who are likely to stop using the service.

Without a prediction system, customers are often lost without any preventive action. On the other hand, retention strategies such as discounts, promotions, loyalty offers, and customer support improvements require additional cost and resources.

Therefore, the company needs a way to identify high-risk churn customers so that retention strategies become more targeted and cost-efficient.

**Main business question:**

> How can the company identify high-risk churn customers and make retention efforts more targeted and cost-efficient?

---

## Objectives

The objectives of this project are:

1. Predict customers who are likely to churn based on customer profile, service usage, and billing information.
2. Identify important churn drivers through Exploratory Data Analysis.
3. Build and compare several machine learning classification models.
4. Select the best model based on churn detection performance.
5. Provide business recommendations to support more effective customer retention strategies.
6. Estimate the potential business impact of using machine learning for churn prevention.

---

## Analytic Approach

This project uses **supervised machine learning** with a **classification approach** because the target variable is already labeled as churn or not churn.

The overall approach consists of:

1. **Exploratory Data Analysis (EDA)**  
   Used to identify churn patterns and understand which customer characteristics are associated with churn behavior.

2. **Data Preprocessing**  
   Used to clean the data and transform numerical and categorical features into a model-ready format.

3. **Modeling and Model Comparison**  
   Several classification models are trained and evaluated using the same preprocessing pipeline.

4. **Hyperparameter Tuning**  
   The best-performing model is optimized using GridSearchCV.

5. **Model Evaluation and Business Interpretation**  
   The final model is evaluated using test data and translated into business insights.

---

## Evaluation Metric

In this churn prediction case, the company wants to reduce customer loss while maintaining retention cost efficiency.

### Type 1 Error: False Positive

**Condition:**  
The model predicts that a customer will churn, but the customer actually stays.

**Business consequence:**  
The company may spend unnecessary retention costs, such as wasted promotions, discounts, or loyalty offers.

### Type 2 Error: False Negative

**Condition:**  
The model predicts that a customer will stay, but the customer actually churns.

**Business consequence:**  
The company loses customers, loses recurring revenue, and misses the opportunity to retain high-risk customers.

Because missing churn customers is more costly for the business, this project prioritizes:

- **Recall**: to detect as many churn customers as possible
- **F1-Score**: to balance recall and precision
- **ROC-AUC**: to measure overall classification performance

The main focus is **Recall**, because the business objective is to minimize missed churners.

---

## Data Understanding

The dataset contains customer profile, subscription details, service usage, billing information, and churn status.

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

### Attribute Information

| Attribute | Data Type | Description |
|---|---|---|
| Dependents | Categorical | Whether the customer has dependents or not |
| tenure | Numerical | Number of months the customer has stayed with the company |
| OnlineSecurity | Categorical | Whether the customer has online security service or not |
| OnlineBackup | Categorical | Whether the customer has online backup service or not |
| InternetService | Categorical | Customer's internet service type |
| DeviceProtection | Categorical | Whether the customer has device protection or not |
| TechSupport | Categorical | Whether the customer has tech support or not |
| Contract | Categorical | Customer's contract type |
| PaperlessBilling | Categorical | Whether the customer uses paperless billing or not |
| MonthlyCharges | Numerical | Monthly amount charged to the customer |
| Churn | Target | Whether the customer churned or not |

---

## Key EDA Insights

### 1. Month-to-Month Contract is the Strongest Churn Driver

Customers with **month-to-month contracts** show the highest churn risk.

Month-to-month customers make up **89.3% of all churn customers**, while customers with one-year and two-year contracts show significantly lower churn.

**Business meaning:**  
Customers with short-term contracts have lower commitment and are easier to lose.

**Business action:**  
Encourage month-to-month customers to switch to longer contracts through incentives, loyalty rewards, or bundled offers.

---

### 2. Fiber Optic Customers Show Higher Churn Risk

Customers using **Fiber Optic internet service** show a higher churn count compared to other internet service types.

| Internet Service | Churn Count |
|---|---:|
| Fiber Optic | 902 |
| DSL | 311 |
| No internet service | 75 |

**Business meaning:**  
Fiber Optic customers may have higher expectations toward service quality, price, or stability. If the perceived value is not strong enough, they may be more likely to churn.

---

### 3. Higher Monthly Charges Increase Churn Risk

Churned customers tend to have higher monthly charges than retained customers.

| Customer Group | Median Monthly Charges |
|---|---:|
| Churn | ± 80 |
| Non-churn | ± 65 |

**Business meaning:**  
Higher monthly cost may increase churn risk, especially when customers do not feel that the service value matches the price paid.

---

### 4. Long Tenure and Add-on Services Reduce Churn Risk

Customers with longer tenure and long-term contracts tend to be more loyal.

Additional services such as:

- Online Security
- Device Protection
- Tech Support

can act as retention factors because they increase perceived value and customer dependency on the service.

---

## Data Preprocessing Pipeline

The preprocessing steps were designed to transform raw customer data into a model-ready format.

### Process Flow

| Step | Description |
|---|---|
| Raw Dataset | 4,930 initial records |
| Remove Duplicates | 77 duplicate rows removed |
| Missing Value Check | 0 missing values found |
| Target Encoding | Churn label converted into binary format: Yes = 1, No = 0 |
| Feature Split | X = 10 features, y = Churn |
| Train-Test Split | 80:20 split with stratify=y |
| ColumnTransformer | Numerical scaling and categorical encoding inside pipeline |

### Feature Transformation

| Feature Type | Columns | Method |
|---|---|---|
| Numerical | tenure, MonthlyCharges | StandardScaler |
| Categorical | Dependents, Contract, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, PaperlessBilling | OneHotEncoder |
| Target | Churn | LabelEncoder |

### Why These Methods Were Used

- **Duplicate removal** prevents repeated customer records from biasing the model.
- **Label encoding** converts the target variable into a binary classification format.
- **StandardScaler** ensures numerical features have consistent scale.
- **OneHotEncoder** transforms categorical variables without creating false order.
- **ColumnTransformer and pipeline** ensure preprocessing is applied consistently during training and testing.

---

## Modeling

Several classification models were compared using the same preprocessing pipeline:

- Logistic Regression
- Decision Tree
- XGBoost
- Random Forest

The models were evaluated using **5-fold cross validation** with **Recall** as the primary metric.

Recall was prioritized because the main business goal is to detect as many churn customers as possible and reduce missed churners.

---

## 5-Fold Cross Validation Comparison

| Rank | Model | Mean Recall CV Score |
|---:|---|---:|
| 1 | Logistic Regression | 0.795 |
| 2 | Decision Tree | 0.749 |
| 3 | XGBoost | 0.650 |
| 4 | Random Forest | 0.457 |

### Best Model Selection

**Logistic Regression** was selected as the final model because it produced the highest and most stable Recall score during cross validation.

Besides its strong recall performance, Logistic Regression is also interpretable, meaning it can explain which features increase or reduce churn risk.

This is important for business use because the company needs not only predictions, but also clear reasons behind the churn risk.

---

## Hyperparameter Tuning

After selecting Logistic Regression as the best model, hyperparameter tuning was performed using **GridSearchCV**.

### Tuning Setup

| Component | Description |
|---|---|
| Search Method | GridSearchCV |
| Cross Validation | 5-fold |
| Scoring Metric | Recall |
| Total Candidate Combinations | 18 |
| Class Weight | balanced |

### Parameter Grid

| Parameter | Values |
|---|---|
| C | 0.001, 0.01, 0.1, 1, 10, 100 |
| Solver | liblinear, lbfgs, newton-cg |
| class_weight | balanced |

### Best Hyperparameters

| Parameter | Best Value |
|---|---|
| C | 0.001 |
| Solver | liblinear |
| class_weight | balanced |

### Why Hyperparameter Tuning Was Used

GridSearchCV was used to find the most suitable Logistic Regression configuration for the business objective.

The tuning process was focused on maximizing Recall so the model could detect more potential churn customers.

---

## Final Model Performance

The final Logistic Regression model was evaluated on **971 unseen test customers**.

### Test Set Results

| Metric | Score |
|---|---:|
| Accuracy | 74.4% |
| Precision | 51.1% |
| Recall | 80.2% |
| F1-Score | 62.4% |
| ROC-AUC | 76.2% |

### Generalization Check

| Metric | Train | Test |
|---|---:|---:|
| Accuracy | 0.738 | 0.744 |
| Precision | 0.503 | 0.511 |
| Recall | 0.800 | 0.802 |
| F1-Score | 0.618 | 0.624 |
| ROC-AUC | 0.757 | 0.762 |

### Model Interpretation

The model captures about **80% of actual churn customers**, which aligns with the business objective to reduce missed churners.

The precision is lower because the model intentionally flags more customers as potential churners. This may create extra retention costs, but it reduces the risk of losing high-risk customers without action.

The train and test scores are close, indicating that the model is stable and does not show significant overfitting.

---

## Feature Importance Interpretation

Logistic Regression does not only predict churn, but also helps explain why customers are at risk.

### Churn Drivers

Features that increase churn probability:

- Month-to-month contract
- Higher monthly charges
- Fiber Optic internet service

These factors indicate that churn risk is higher among customers with low commitment, higher cost pressure, and specific service expectations.

### Retention Factors

Features that reduce churn probability:

- Longer tenure
- One-year or two-year contract
- Online Security
- Device Protection
- Tech Support

These features act as retention signals because they increase customer commitment, perceived value, and dependency on the service.

### Key Insight

Churn is mainly influenced by customer commitment, pricing pressure, service value, and additional support services.

---

## Model Limitations

Although the final model performs well, it still has limitations.

### The 20% Gap

The model catches about **8 out of 10 churn customers**, but still misses some real churn customers.

This means there is still around a **20% False Negative Rate**, where some churn customers are predicted as non-churn.

### Why This Happens

The model has limitations because:

1. **Limited behavior data**  
   Customer usage activity, complaints, service history, and support interactions are not fully captured.

2. **Unknown external factors**  
   Competitor offers, personal customer decisions, and external market conditions are outside the dataset.

3. **Recall-precision trade-off**  
   Increasing churn detection may also increase false alarms.

### Accurate When

The model is most reliable when customers have similar patterns to the training data, especially based on:

- Contract type
- Tenure
- Monthly charges
- Internet service type
- Additional services

### Improvement Roadmap

To improve the model, future work should include:

1. **Add more behavioral signals**
   - Customer complaints
   - Payment delays
   - Usage trends
   - Customer service tickets
   - Network quality

2. **Try alternative methods**
   - LightGBM
   - XGBoost tuning
   - SMOTENC
   - Threshold tuning

3. **Analyze missed churners**
   - Study False Negative cases
   - Identify hidden churn patterns
   - Improve feature engineering strategy

### Final Limitation Message

This model should be used as a recall-focused decision support system, not as a perfect churn prediction oracle.

---

## Business Impact

Machine Learning changes retention from broad assumptions into targeted churn action.

### Before Machine Learning

Without the model, churn risk is not prioritized. The company may lose churn customers without taking preventive action.

### After Machine Learning

With the model, the company can identify high-risk customers earlier and prioritize them for retention treatment.

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

### Business Meaning

The model helps reduce estimated revenue loss by **$6.05K** or **31.37% efficiency gain** by identifying high-risk churn customers earlier.

Machine Learning does not eliminate churn completely, but it helps the company reduce business loss through more targeted retention actions.

---

## Executive Conclusion

Based on the diagnostic analysis and predictive modeling results, customer churn is not random.

Churn is concentrated among customers with:

- Low commitment contracts
- Higher monthly charges
- Fiber Optic internet service
- Limited support or add-on services

On the other hand, customers with longer tenure, long-term contracts, and additional services are more likely to stay.

The final Logistic Regression model is stable, interpretable, and optimized for Recall. It successfully detects most high-risk churn customers early enough for retention action.

### Most Impactful Conclusion

Machine Learning shifts customer retention from reactive spending to targeted churn prevention.

The model reduces estimated revenue loss by **$6.05K**, achieves **31.37% efficiency gain**, and keeps the decision process explainable for business stakeholders.

---

## Strategic Recommendations

The recommendations are directly based on the strongest churn patterns found in the analysis.

### 1. Targeted Retention

Prioritize customers flagged as high-risk by the model.

Focus especially on customers with:

- Month-to-month contract
- High monthly charges
- Fiber Optic internet service

**Business goal:**  
Use retention budget only where churn probability is high.

---

### 2. Contract Migration Campaign

Encourage month-to-month customers to switch to yearly contracts.

Possible actions:

- Limited-time discounts
- Loyalty rewards
- Bundled offers
- Free or discounted add-on services

**Business goal:**  
Increase customer lock-in and reduce switching tendency.

---

### 3. Value-Added Service Bundling

Promote service bundles such as:

- Online Security
- Device Protection
- Tech Support

Target customers with high monthly charges but limited add-on services.

**Business goal:**  
Increase perceived value and customer dependency on the service.

---

### 4. Pricing Strategy Review

Review the pricing perception of high-cost customers, especially Fiber Optic users.

Possible actions:

- Personalized loyalty offers
- Discount for customers after a certain tenure
- Bundling high-cost plans with additional services
- Improve service value communication

**Business goal:**  
Reduce churn caused by price sensitivity.

---

### 5. Continuous Model Improvement

To reduce the 20% False Negative gap, the company should add richer customer behavior data such as:

- Customer service tickets
- Complaint history
- Payment delays
- Network quality
- Usage behavior

**Business goal:**  
Improve model accuracy and discover hidden churn drivers.

---

## Final Takeaway

The current model is already effective as a business decision support system for churn prediction.

It helps the company:

- Detect high-risk churn customers earlier
- Prioritize retention actions
- Reduce estimated revenue loss
- Improve retention cost efficiency
- Understand the main factors driving churn

However, the model should continue to be improved with richer behavioral data, deeper error analysis, and regular monitoring.

Overall, this project provides a strong foundation for building a data-driven customer retention strategy in the telco industry.
