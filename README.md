
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
## Analytics (Power BI Measures)
Used **DAX formulas** to derive key metrics for churn analysis:

#### **%Device Protection**
```DAX
%Device Protection =
DIVIDE(
CALCULATE(COUNT('01 Churn-Dataset'[DeviceProtection]),'01 Churn-Dataset'[DeviceProtection] ="Yes",'01 Churn-Dataset'[Churn] = "C-Yes"),
CALCULATE(COUNT('01 Churn-Dataset'[DeviceProtection]),'01 Churn-Dataset'[Churn] = "C-Yes"),
0)
```
#### **%Having Dependents**
```DAX
%Having Dependents =
DIVIDE(
CALCULATE(COUNT('01 Churn-Dataset'[Dependents]),'01 Churn-Dataset'[Dependents] = "Yes",'01 Churn-Dataset'[Churn] = "C-Yes"),
CALCULATE(COUNT('01 Churn-Dataset'[Dependents]),'01 Churn-Dataset'[Churn] ="C-Yes"),
0)
```

Similar DAX formulas were used for:
- **% Having Dependents**
- **% Having Partner**
- **% Using Internet Service**
- **% Using Multiple Lines**
- **% Using Online Backup**
- **% Using Online Security**
- **% Using Paperless Billing**
- **% Using Phone Service**
- **% Using Streaming Movies**
- **% Using Streaming TV**
- **% Using Tech Support**

#### **Churn Rate Calculation**
This measure calculates the churn percentage dynamically. This formula was initially generated with the aid of ChatGPT (through AI prompting), but contained errors that were subsequently identified and corrected by the author prior to implementation.
```DAX
%Churn = 
VAR TotalCustomers = CALCULATE(COUNT('01 Churn-Dataset'[customerID]), ALL('01 Churn-Dataset'[Churn]))
VAR ChurnedCustomers = CALCULATE(COUNT('01 Churn-Dataset'[customerID]), '01 Churn-Dataset'[Churn] = "C-Yes")
VAR SelectedChurn = SELECTEDVALUE('01 Churn-Dataset'[Churn], "All")  // Default to "All" if nothing is selected

RETURN 
SWITCH(
    SelectedChurn,
    "C-Yes", DIVIDE(ChurnedCustomers, TotalCustomers) * 100,  // % of churned customers
    "C-No", DIVIDE(TotalCustomers - ChurnedCustomers, TotalCustomers) * 100,  // % of non-churned customers
    100  // When "All" is selected, show 100%
)
```

## Dashboard & Visualization

---
Visualizations, primarily column charts, were used extensively to analyze these KPIs and identify patterns in the data.

![Main Dashboard](https://github.com/Sayan-J-Pandit/Customer-Churn-Dashboard/blob/main/Customer%20Churn%20Dashboard%20self-images-0.jpg)
![Analysis Acorss Service](https://github.com/Sayan-J-Pandit/Customer-Churn-Dashboard/blob/main/Customer%20Churn%20Dashboard%20self-images-1.jpg)
![Analysis Acorss Segments](https://github.com/Sayan-J-Pandit/Customer-Churn-Dashboard/blob/main/Customer%20Churn%20Dashboard%20self-images-2.jpg)



### **Charts & Visuals Used:**
- **Gauge Chart** 
- **Line Chart** 
- **Stacked Bar Chart** 
- **Doughnut & Pie Charts** 
- **Clustered Column Chart**
- **Cards & Multi-Row Cards**
### **Insights & Recommendations**

![keyInsights & Conclusions](https://github.com/Sayan-J-Pandit/Customer-Churn-Dashboard/blob/main/Customer%20Churn%20Dashboard%20self-images-3.jpg)
![Suggestion](https://github.com/Sayan-J-Pandit/Customer-Churn-Dashboard/blob/main/Customer%20Churn%20Dashboard%20self-images-4.jpg)
Additional:
![Additional:Suggestion in Details](https://github.com/Sayan-J-Pandit/Customer-Churn-Dashboard/blob/main/Customer%20Churn%20Dashboard%20self-images-5.jpg)

## 📌 Conclusion

This project provides a **data-driven understanding of customer churn** and identifies key factors influencing retention. The insights gained from this analysis can help businesses **optimize customer engagement strategies and reduce churn rates**.


## 🚀 How to Use This Repository
1. Download the dataset from the provided sources.
2. Open the **Power BI file** to explore the visualizations.
3. Review the transformations in **Power Query & DAX measures**.
4. Use the insights and recommendations to improve customer retention strategies.
**For further details or inquiries, feel free to reach out!** 🎯
