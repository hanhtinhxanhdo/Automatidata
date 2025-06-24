# NYC Taxi Tip Prediction: A Machine Learning Project

This repository contains the complete data analysis and machine learning project for predicting generous tipping behavior for the New York City Taxi & Limousine Commission (TLC). The project was completed as part of the Automatidata data analytics program.

## Project Overview

[cite_start]The objective of this project is to identify the key factors that influence whether a New York City taxi rider will leave a generous tip (defined as ≥20% of the fare)[cite: 1, 24]. [cite_start]Understanding these drivers is crucial for the NYC TLC to develop policies and driver support programs aimed at increasing gratuities and improving driver satisfaction[cite: 1, 2].

[cite_start]A Random Forest classification model was developed using the 2017 NYC Yellow Taxi trip dataset[cite: 3]. [cite_start]The final model demonstrates reliable performance with **73% accuracy** across key metrics, including precision, recall, and F1-score[cite: 5, 12]. [cite_start]The analysis revealed that the most significant predictors of a generous tip are **the taxi company (VendorID), the total fare amount, and the trip duration**[cite: 12, 101, 102].

## Business Problem

[cite_start]The NYC TLC sought a data-driven approach to enhance driver earnings, which are significantly impacted by tips[cite: 1]. [cite_start]The initial proposal was to build a model for an in-car app to alert drivers to riders who are unlikely to tip[cite: 146, 149]. [cite_start]However, after a thorough ethical review, the project's objective was revised to focus on generating system-wide insights rather than targeting individual riders, thereby avoiding potential discriminatory service patterns[cite: 11, 123, 149].

## Data

[cite_start]This project utilizes the **2017 Yellow Taxi Trip Data** dataset[cite: 38]. [cite_start]This file contains detailed trip-level records for New York City yellow taxis, including trip times, distance, fare details, and payment methods[cite: 38]. [cite_start]The dataset's reliability is considered strong as it originates from the NYC TLC, though it required cleaning and validation[cite: 31, 32].

## Methodology

[cite_start]The project followed the PACE (Plan, Analyze, Construct, Execute) problem-solving framework[cite: 15, 22].

### 1. Ethical Considerations
[cite_start]A significant part of the planning phase was dedicated to ethical considerations[cite: 27].
* [cite_start]**Primary Risk**: The main ethical concern was the potential for **algorithmic bias**[cite: 29, 146]. [cite_start]The model could learn to associate certain neighborhoods with lower tips, leading drivers to avoid those areas and creating inequitable service[cite: 146, 147].
* **Model Errors**:
    * [cite_start]A **False Positive** (predicting a generous tip that doesn't occur) was identified as the more common error, which could lead to driver frustration and erode trust in the model[cite: 152, 153].
    * [cite_start]A **False Negative** (failing to identify a generous tipper) represents a missed opportunity for the driver[cite: 154, 155].
* [cite_start]**Revised Objective**: To mitigate these risks, the objective was shifted from predicting individual tips to identifying the universal factors that drive generosity across the entire system[cite: 124, 149].

### 2. Feature Engineering
To prepare the data for modeling, the following steps were taken:
* [cite_start]**Data Filtering**: The dataset was filtered to include only trips paid by **credit card** (`payment_type == 1`), as cash tips are not recorded[cite: 63].
* [cite_start]**Target Variable Creation**: A new binary column, `generous`, was created[cite: 65]. [cite_start]It was assigned a value of `1` if the `tip_percent` was ≥ 20%, and `0` otherwise[cite: 24, 65].
* **New Feature Creation**:
    * [cite_start]Time-based features were engineered from the `tpep_pickup_datetime` column, including `day` of the week, `month`, and four binary columns for time-of-day (`am_rush`, `daytime`, `pm_rush`, `nighttime`)[cite: 65, 66].
* [cite_start]**Data Cleaning**: Redundant, irrelevant, and data-leaking columns (e.g., `tip_amount`, `total_amount`, `trip_distance`) were dropped[cite: 83].
* [cite_start]**Variable Encoding**: Categorical features like `day` and `month` were converted to numerical format using one-hot encoding[cite: 67].

### 3. Modeling and Evaluation
* [cite_start]**Model Selection**: A **Random Forest Classifier** was chosen for its robustness and ability to handle non-linear relationships[cite: 51, 110, 114]. An XGBoost model was also trained as a benchmark, but the Random Forest was selected as the final model due to its comparable performance and superior ease of explanation.
* [cite_start]**Evaluation Metric**: The **F1-score** was chosen as the primary evaluation metric because the business objective required a balance between precision and recall[cite: 64, 86]. [cite_start]The dataset was nearly balanced (49% "generous" vs. 51% "not generous"), making accuracy a reasonable secondary metric[cite: 64].
* **Tuning**: `GridSearchCV` was used to perform a comprehensive search for the optimal hyperparameters for the Random Forest model, ensuring it was well-tuned and not overfit.

## Results and Key Insights

[cite_start]The final Random Forest model is a reliable predictor of generous tipping behavior, achieving a **73% F1-score and accuracy** on the holdout test set[cite: 5, 88, 112]. [cite_start]The test scores were nearly identical to the validation scores, indicating a well-generalized model[cite: 87, 88].

The most valuable output of the project was the feature importance analysis, which provided clear, actionable insights:
* [cite_start]**Vendor Identity (`VendorID`)**: This was overwhelmingly the most important predictor, with over **60% importance**[cite: 12, 13, 100]. This indicates a significant difference in tipping behavior based on the taxi company.
* [cite_start]**Trip Value (`predicted_fare`, `mean_duration`)**: Higher fares and longer trips were strongly correlated with generous tips and were key predictors[cite: 12, 13, 101].
* [cite_start]**Passenger Count**: The number of passengers had a slight effect on tipping behavior[cite: 12, 13, 103].
* [cite_start]**Minimal Impact Features**: Features related to time, such as rush hours, day of the week, and month, had negligible impact on predicting a generous tip[cite: 12, 13, 102].

## Recommendations

Based on the model's findings, the following strategic recommendations were made to the NYC TLC:
1.  [cite_start]**Utilize for System-Wide Insights, Not Individual Targeting**: The model should be used as a policy guidance tool to understand system-wide trends, not as a real-time app for drivers[cite: 11, 123, 124]. This approach maximizes benefits while ensuring equitable service.
2.  [cite_start]**Investigate Vendor Disparities**: A high-priority action should be to investigate the significant performance differences between vendors identified by the `VendorID` feature[cite: 9, 133]. This could lead to standardizing best practices for payment interfaces or service quality programs.
3.  [cite_start]**Promote High-Value Trips & Credit Card Payments**: Since higher fares, longer trips, and credit card payments are linked to generous tips, the TLC should consider programs to encourage these types of transactions[cite: 7, 8, 125].
4.  [cite_start]**Implement Ongoing Bias Monitoring**: The TLC should establish continuous fairness audits for this and future models to prevent any form of discrimination[cite: 10, 126, 131].

## Repository Structure
