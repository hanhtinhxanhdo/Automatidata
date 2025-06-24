# NYC Taxi Tip Prediction: A Machine Learning Project

This repository contains the complete data analysis and machine learning project for predicting generous tipping behavior for the New York City Taxi & Limousine Commission (TLC). The project was completed as part of the Automatidata data analytics program.

## Project Overview

The project's objective is to identify the key factors that influence generous taxi tips, defined as a tip of 20% or more. Understanding these drivers is crucial for the NYC TLC to develop policies and support programs aimed at increasing gratuities and improving driver satisfaction.

To achieve this, a Random Forest classification model was developed using the 2017 NYC Yellow Taxi trip dataset. The final model demonstrates reliable performance with **73% accuracy** across key metrics, including precision, recall, and F1-score. The analysis revealed that the most significant predictors of a generous tip are **the taxi company (VendorID), the total fare amount, and the trip duration**.

## Business Problem

The NYC TLC sought a data-driven approach to enhance driver earnings, which are significantly impacted by tips. The initial request was to build a model for an in-car app to alert drivers to riders who are unlikely to tip. However, after a thorough ethical review, the project's objective was revised to focus on generating system-wide insights rather than targeting individual riders, thereby avoiding potential discriminatory service patterns.


## Methodology

The project followed the PACE (Plan, Analyze, Construct, Execute) problem-solving framework.

### 1. Ethical Considerations
A significant part of the planning phase was dedicated to ethical considerations.
* **Primary Risk**: The main ethical concern was the potential for **algorithmic bias**. The model could learn to associate certain neighborhoods with lower tips, leading drivers to avoid those areas and creating inequitable service.
* **Model Errors**:
    * A **False Positive** (predicting a generous tip that doesn't occur) was identified as the more common error, which could lead to driver frustration and erode trust in the model.
    * A **False Negative** (failing to identify a generous tipper) represents a missed opportunity for the driver.
* **Revised Objective**: To mitigate these risks, the objective was shifted from predicting individual tips to identifying the universal factors that drive generosity across the entire system.

### 2. Feature Engineering
To prepare the data for modeling, the following steps were taken:
* **Data Filtering**: The dataset was filtered to include only trips paid by **credit card** (`payment_type == 1`), as cash tips are not recorded.
* **Target Variable Creation**: A new binary column, `generous`, was created. It was assigned a value of `1` if the `tip_percent` was ≥ 20%, and `0` otherwise.
* **New Feature Creation**:
    * Time-based features were engineered from the `tpep_pickup_datetime` column, including `day` of the week, `month`, and four binary columns for time-of-day (`am_rush`, `daytime`, `pm_rush`, `nighttime`).
* **Data Cleaning**: Redundant, irrelevant, and data-leaking columns (e.g., `tip_amount`, `total_amount`, `trip_distance`) were dropped.
* **Variable Encoding**: Categorical features like `day` and `month` were converted to numerical format using one-hot encoding.

### 3. Modeling and Evaluation
* **Model Selection**: A **Random Forest Classifier** was chosen for its robustness and ability to handle non-linear relationships. An XGBoost model was also trained as a benchmark, but the Random Forest was selected as the final model due to its comparable performance and superior ease of explanation.
* **Evaluation Metric**: The **F1-score** was chosen as the primary evaluation metric because the business objective required a balance between precision and recall. The dataset was nearly balanced (49% "generous" vs. 51% "not generous"), making accuracy a reasonable secondary metric.
* **Tuning**: `GridSearchCV` was used to perform a comprehensive search for the optimal hyperparameters for the Random Forest model, ensuring it was well-tuned and not overfit.

## Results and Key Insights

The final Random Forest model is a reliable predictor of generous tipping behavior, achieving a **73% F1-score and accuracy** on the holdout test set. The test scores were nearly identical to the validation scores, indicating a well-generalized model.

The most valuable output of the project was the feature importance analysis, which provided clear, actionable insights:
* **Vendor Identity (`VendorID`)**: This was overwhelmingly the most important predictor, with over **60% importance**. This indicates a significant difference in tipping behavior based on the taxi company.
* **Trip Value (`predicted_fare`, `mean_duration`)**: Higher fares and longer trips were strongly correlated with generous tips and were key predictors.
* **Passenger Count**: The number of passengers had a slight effect on tipping behavior.
* **Minimal Impact Features**: Features related to time, such as rush hours, day of the week, and month, had negligible impact on predicting a generous tip.

## Recommendations

Based on the model's findings, the following strategic recommendations were made to the NYC TLC:
1.  **Utilize for System-Wide Insights, Not Individual Targeting**: The model should be used as a policy guidance tool to understand system-wide trends, not as a real-time app for drivers. This approach maximizes benefits while ensuring equitable service.
2.  **Investigate Vendor Disparities**: A high-priority action should be to investigate the significant performance differences between vendors identified by the `VendorID` feature. This could lead to standardizing best practices for payment interfaces or service quality programs.
3.  **Promote High-Value Trips & Credit Card Payments**: Since higher fares, longer trips, and credit card payments are linked to generous tips, the TLC should consider programs to encourage these types of transactions.
4.  **Implement Ongoing Bias Monitoring**: The TLC should establish continuous fairness audits for this and future models to prevent any form of discrimination.
5.  **Enhance Future Data Collection**: To improve future model performance, the TLC should consider collecting or integrating external data sources, such as historical weather conditions and traffic data.

## Repository Structure
