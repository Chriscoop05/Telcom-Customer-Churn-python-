## README.md Content Suggestion

```markdown
# Telco Customer Churn Prediction and Analysis

This repository contains an end-to-end machine learning project focused on predicting customer churn in a telecommunications dataset. The goal is to identify factors contributing to churn and build predictive models to assist in retention strategies.

## Project Overview

Customer churn is a critical problem for many businesses, including telecom companies. Predicting which customers are likely to churn allows businesses to proactively intervene and implement retention programs. This project walks through the entire data science pipeline:

1.  **Data Loading & Initial Inspection**: Understanding the raw data.
2.  **Data Preprocessing & Cleaning**: Preparing the data for machine learning models.
3.  **Exploratory Data Analysis (EDA)**: Discovering patterns, relationships, and insights within the data.
4.  **Model Building & Evaluation**: Training and assessing various classification models.
5.  **Model Refinement**: Improving model performance through feature selection.
6.  **Threshold Optimization**: Calibrating model predictions to meet specific business objectives (e.g., balancing precision and recall).

## Dataset

The project utilizes the `WA_Fn-UseC_-Telco-Customer-Churn.csv` dataset, which contains customer information including demographics, services subscribed, account information, and churn status. This dataset provides a rich set of features to build a robust churn prediction model.

## Methodology

### 1. Data Loading and Initial Inspection

*   The dataset was loaded using pandas. Initial checks confirmed `7043` rows and `21` columns. No duplicate customer IDs were found.
*   Data types were inspected, identifying numerous `object` columns requiring conversion.

### 2. Data Preprocessing and Cleaning

*   `TotalCharges` was converted to a `float` type, handling non-numeric values by filling them with `0`.
*   Several binary `object` columns (e.g., `gender`, `PaperlessBilling`, `Churn`) were converted to `boolean` types.
*   `SeniorCitizen` was converted to `boolean`.
*   Categorical features (`PaymentMethod`, `Contract`, `InternetService`) were one-hot encoded.

### 3. Exploratory Data Analysis (EDA)

*   **Boolean Feature Distributions**: Visualizations provided insights into customer characteristics, such as high rates of paperless billing and lower engagement in streaming services.
*   **Correlation Analysis**: A heatmap revealed strong correlations between `tenure` and `TotalCharges`, and `MonthlyCharges` with `InternetService_Fiber optic`.
*   **Churn Correlations**: Key features highly correlated with churn were identified, including `tenure` (negative), `InternetService_Fiber optic` (positive), `Contract_Two year` (negative), and `PaymentMethod_Electronic check` (positive).

### 4. Model Building and Evaluation

*   The data was split into training and testing sets (80/20) with stratification for `Churn`.
*   A `ColumnTransformer` was used to `StandardScale` numerical features and `passthrough` boolean/one-hot encoded features.
*   The following models were trained and evaluated using 5-fold cross-validation (scoring on ROC AUC):
    *   **Logistic Regression**: Test ROC AUC: `0.84`, Churn Recall: `0.78`, Churn Precision: `0.50`.
    *   **Random Forest Classifier (Original)**: Test ROC AUC: `0.84`, Churn Recall: `0.82`, Churn Precision: `0.51`.
    *   **Gradient Boosting Classifier**: Test ROC AUC: `0.85`, Churn Recall: `0.49`, Churn Precision: `0.65`.
*   Feature importances were analyzed for each model.

### 5. Model Refinement (Feature Selection)

*   A **Refined Random Forest Model** was developed by selecting features with an importance score of `0.02` or higher from the original Random Forest model (reducing features from 23 to 10).
*   The refined model maintained similar performance: Test ROC AUC: `0.84`, Churn Recall: `0.80`, Churn Precision: `0.51`.

### 6. Threshold Optimization

*   Given the relatively low precision scores, **threshold optimization** was applied to improve the precision of 'Churn' predictions, targeting `precision >= 0.60` and `recall > 0.60`.
*   For the **Refined Random Forest Model**: An optimal threshold of `0.63` yielded Churn Precision: `0.60`, Churn Recall: `0.65`.
*   For the **Original Random Forest Model**: An optimal threshold of `0.62` yielded Churn Precision: `0.60`, Churn Recall: `0.65`.

## Conclusion

The threshold optimization significantly enhanced the precision of our churn prediction models, making them more effective for business applications where accurate identification of at-risk customers is crucial. While a slight decrease in recall was observed, the trade-off resulted in a more balanced and actionable model for targeted retention efforts, moving past the 50-50 guessing performance without optimization.

This project provides a robust framework for Telco customer churn prediction, offering valuable insights and a deployable model for proactive customer management.

