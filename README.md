
 # Customer Churn Analysis

 
## Author: Sayan Pandit


 This project analyzes customer churn for a telecommunications company, identifying key factors contributing to churn and providing data-driven recommendations to reduce it. The analysis is designed to be easily understood by a non-technical audience.


 ## Objectives 

* **Identify Reasons for Churn:** Uncover the primary drivers behind customer attrition. 

* **Provide Recommendations:** Suggest actionable strategies to minimize churn based on data insights. 


## Data Source 

The dataset used for this analysis is publicly available and can be found at the following locations: 

* [IBM Telco Customer Churn on ICP4D](https://github.com/IBM/telco-customer-churn-on-icp4d/tree/master) 

* [Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)


 The data was prepared using a combination of tools to ensure clarity and consistency. 

**Tools Used:** 

* Excel 
* Power Query (within Power BI) 
* Power BI 
* ChatGPT (for assistance with One DAX measures) 


**Excel Data Cleaning:** 
Several columns were simplified in Excel for easier interpretation: 

* **Senior Citizen:** "Yes" was replaced with "Senior Citizen", and "No" was replaced with "Adult". 

* **Partner:** "Yes" was replaced with "Partnered", and "No" was replaced with "Single". 

These replacements were done using Excel's **Find & Replace (Ctrl + F → Replace All)** functionality. 


**Power BI Data Transformation (Power Query):** 

Further data cleaning and feature engineering were performed in Power BI using Power Query: 

* **Churn:** "Yes" was replaced with "C-Yes", and "No" was replaced with "C-No". This was done using the "Replace Values" option in Power Query.

**New Columns Created:** 

The following new columns were created using custom queries in Power Query: 


***1. Internet Y/N (`Internet Service Indicator`):*** A binary indicator for internet service (Yes/No). Fiber optic and DSL connections were classified as "Yes".
```powerquery 
if [InternetService] = "Fiber optic" then "Yes" 
else if [InternetService] = "DSL" then "Yes" 
else "No" 
```

***2. Number of Services Used:*** A count of the number of services subscribed to by each customer (Streaming Movies, Streaming TV, Tech Support, Online Backup, Online Security, Paperless Billing).
```powerquery
(if [StreamingMovies] = "Yes" then 1 else 0) +
(if [StreamingTV] = "Yes" then 1 else 0) +
(if [TechSupport] = "Yes" then 1 else 0) +
(if [OnlineBackup] = "Yes" then 1 else 0) +
(if [OnlineSecurity] = "Yes" then 1 else 0) +
(if [PaperlessBilling] = "Yes" then 1 else 0)
```
***3. Tenure (in Years) (`Tenure Buckets`):*** Categorized customer tenure into yearly buckets (e.g., "<1 Year", "<2 Years", ..., "6+ Years").
```powerquery
if [Tenure] < 12 then "<1 Year"
else if [Tenure] < 24 then "<2 Years"
else if [Tenure] < 36 then "<3 Years"
else if [Tenure] < 48 then "<4 Years"
else if [Tenure] < 60 then "<5 Years"
else if [Tenure] < 72 then "<6 Years"
else "6+ Years"
```
