# Justification for Using Prophet and Isolation Forest

In this project, I used a combination of **Prophet** and **Isolation Forest** for time series forecasting and anomaly detection. This method was specifically chosen because of the unique strengths that each algorithm offers, especially in handling customer consumption data, which often displays seasonal trends and occasional anomalies.

### 1. Why **Prophet** Was Used

**Prophet** is an open-source forecasting tool developed by Facebook, designed to handle time series data that may exhibit complex patterns like seasonality and trends. Here's why Prophet was a suitable choice for this project:

- **Seasonality Handling**: Consumption data often displays seasonal patterns (e.g., higher usage during summer or winter months). Prophet automatically detects such seasonality and accounts for it in the forecasts. This is critical when forecasting monthly customer consumption trends.
  
- **Handling Missing Data and Outliers**: Customer consumption data can often have missing values or unusual spikes. Prophet is designed to handle such issues smoothly without losing accuracy in forecasting.

- **Multiple Seasonalities**: Although we focused mainly on monthly seasonality in this case, Prophet can handle more complex seasonal patterns like daily or yearly seasonality when needed. This flexibility is an important feature for future scalability.

- **Simplicity and Efficiency**: Prophet allows us to fit accurate models to the data with minimal manual adjustments. It’s highly efficient for forecasting time series data over large datasets, such as customer consumption records.

### 2. Why **Isolation Forest** Was Used

**Isolation Forest (IF)** is an anomaly detection algorithm that excels at identifying unusual data points (outliers). Here’s why Isolation Forest was chosen for this task:

- **Unsupervised Learning**: In this project, we didn't have predefined labels for anomalous data points, making Isolation Forest a perfect choice. It automatically detects anomalies by "isolating" points that differ from the general pattern of the data.

- **High-Dimensional Data Handling**: After Prophet forecasted the consumption values, we calculated the residuals (i.e., the difference between actual and forecasted values). The residuals represent how much the actual data deviated from what was expected. Isolation Forest efficiently processes these residuals to detect anomalous patterns.

- **Efficiency with Large Datasets**: Isolation Forest is highly scalable and can handle large datasets quickly. Given the number of customers and months of consumption data, this was critical to ensure the model ran efficiently without compromising accuracy.

### 3. Why Combine **Prophet** and **Isolation Forest**?

The combination of Prophet and Isolation Forest works very well because both algorithms complement each other in their strengths:

- **Prophet for Forecasting**: Prophet captures the normal patterns in the data, such as seasonal trends and expected consumption behaviors. This allows us to establish a baseline for what constitutes "normal" consumption.

- **Isolation Forest for Anomaly Detection**: Once we have the forecasted values from Prophet, we can focus on the residuals (i.e., how much the actual consumption deviated from the forecast). Isolation Forest detects significant outliers in these residuals, highlighting the customers whose consumption patterns deviated from the forecast in unexpected ways.

- **Residual Analysis**: By using the residuals (actual - forecasted values) as input for Isolation Forest, we are able to focus specifically on the deviations that cannot be explained by the normal patterns. This makes the anomaly detection process more precise and reliable.

### 4. The Workflow in the Code

1. **Prophet Forecasting**: For each customer, the historical consumption data (both postpaid and prepaid) is modeled using Prophet to predict future consumption values based on past trends.
   
2. **Residual Calculation**: The difference between the actual consumption and the forecasted consumption (residuals) is calculated. Large residuals indicate a significant deviation from expected behavior.

3. **Isolation Forest Anomaly Detection**: The residuals are then passed through an Isolation Forest model, which identifies anomalous data points where the residuals are unusually large, flagging these as anomalies.

4. **Parallel Processing**: The process is applied across multiple customers using parallel processing to speed up the computations.

5. **Results**: Customers with detected anomalies are flagged, and the results are saved to a CSV file for further analysis. Visualizations are also generated to highlight the trends and anomalies detected.

### 5. Key Advantages of This Approach

- **Robust Anomaly Detection**: Combining Prophet's forecasting capabilities with Isolation Forest's anomaly detection ensures that anomalies are detected based on deviations from expected patterns, rather than just raw consumption values.
  
- **Scalability**: Both Prophet and Isolation Forest are computationally efficient, allowing this approach to scale to large datasets with many customers over long periods of time.

- **Interpretability**: By focusing on residuals, this approach makes it easier to explain why a particular customer was flagged as anomalous (i.e., because their consumption significantly deviated from the forecasted trend).

In conclusion, the combination of **Prophet** and **Isolation Forest** provided a powerful, scalable, and interpretable method for detecting anomalous consumption patterns across a large number of customers. This approach is particularly well-suited for time series data, where understanding the trends is crucial to identifying unusual behavior.






# Overview of Provided Files

This project involves analyzing customer consumption patterns using anomaly detection methods such as **Prophet** combined with **Isolation Forest (IF)**, as well as **Regression Analysis (RA)** combined with **Isolation Forest**. Below is an explanation of each file you have provided, detailing its purpose and how it fits into the project.

---

## 1. `dataset.csv`

- **Description**: This is the raw dataset containing customer consumption data, including both postpaid and prepaid consumption records.
- **Usage**: The data from this file is used to model the consumption trends using Prophet for forecasting and Isolation Forest for anomaly detection. The dataset is cleaned and preprocessed to remove missing values, and columns are split based on whether the data corresponds to postpaid (`post_`) or prepaid (`pre_`) consumption.

---

## 2. `Prophet+IF_anomalies_detected.csv`

- **Description**: This file contains the results of anomaly detection using the Prophet model for forecasting and Isolation Forest for detecting deviations. It includes:
  - **Customer IDs**: Unique identifiers for each customer.
  - **Residuals**: The difference between actual consumption and the forecasted consumption.
  - **Anomaly Labels**: Indicates whether the data is normal (1) or anomalous (-1).
  - **Anomaly Scores**: The severity of the anomaly, as calculated by the Isolation Forest algorithm.
- **Usage**: This file is used to analyze which customers exhibited anomalous consumption behavior and can be used for further investigation or reporting.

---

## 3. `Prophet+IF_full_data_with_anomalies.csv`

- **Description**: This file contains the entire dataset of customer consumption along with labels indicating where anomalies were detected. It includes:
  - **Residuals**: The difference between actual and forecasted values.
  - **Anomaly Labels**: Flags for whether an anomaly was detected for each data point.
- **Usage**: This dataset provides a complete picture of both normal and anomalous consumption patterns. It can be used to visualize how consumption varies over time and where anomalies occur.

---

## 4. `Prophet+IF_normal_customers.csv`

- **Description**: This dataset contains a list of customers who did not exhibit any anomalous behavior. These customers have consumption patterns that closely follow the forecasted values with no significant deviations.
- **Usage**: This file can be used as a reference for customers with normal behavior, helping to establish baseline consumption trends. It can also be useful in evaluating how many customers had no detected anomalies during the analysis.

---

## 5. `RA+IF_anomalies_detected.csv`

- **Description**: Similar to `Prophet+IF_anomalies_detected.csv`, this file contains the results of anomaly detection but using **Regression Analysis (RA)** instead of Prophet for forecasting, combined with Isolation Forest for anomaly detection.
- **Usage**: This file allows for comparison between the RA + IF approach and the Prophet + IF approach, helping to evaluate the effectiveness of each method in detecting anomalies. It provides an alternative perspective on which customers showed unusual consumption patterns.

---

## 6. `RA+IF_Consumption Trends for Top Anomalous Customers (Postpaid vs Prepaid).png`

- **Description**: This image provides a visualization of the consumption trends for the top anomalous customers detected using the **RA + IF** method. It includes both postpaid and prepaid customers.
- **Usage**: The plot helps visualize how consumption patterns of the most anomalous customers deviate from the norm. By comparing postpaid and prepaid customers, it becomes easier to understand which type of customer exhibits more unusual behavior.

---

## 7. `RA+IF_full_data_with_anomalies.csv`

- **Description**: Similar to `Prophet+IF_full_data_with_anomalies.csv`, this file contains the full customer consumption data with labels indicating where anomalies were detected, but it uses **RA + IF** instead of Prophet.
- **Usage**: This file can be used for comparison with the Prophet + IF results. By analyzing both datasets, we can determine how many anomalies were consistently detected across different methods and which method performed better in identifying unusual consumption patterns.

---

## 8. `RA+IF_normal_customers.csv`

- **Description**: This file contains the list of customers who were not flagged as anomalous by the **RA + IF** method. These customers had consumption patterns that followed the regression-based forecast closely, with no significant deviations.
- **Usage**: It serves the same purpose as `Prophet+IF_normal_customers.csv`, allowing for a comparison between the RA + IF and Prophet + IF methods in terms of how many and which customers were classified as "normal."

---

## 9. `RA+IF_Number of Anomalies Detected Per Month (Postpaid vs Prepaid).png`

- **Description**: This image shows the number of anomalies detected per month for postpaid vs prepaid customers using the **RA + IF** method. It likely includes bar charts or line graphs to visually represent how many anomalies were detected each month for both customer types.
- **Usage**: This chart helps in identifying temporal trends, showing which months had the most anomalies and whether postpaid or prepaid customers had more irregular consumption during specific periods.

---

## 10. `Prophet+IF_Top_Anomalous_Customers.png`

- **Description**: This image visualizes the residuals (actual - forecasted values) for the top anomalous customers detected using **Prophet + IF**. It highlights the points where the consumption data significantly deviated from the expected values.
- **Usage**: This plot is useful for understanding which customers exhibited the most unusual behavior and for visualizing how their consumption patterns deviated from the forecasted trends. It makes it easier to identify and explain the anomalies detected.

---

# Conclusion

These files together form a comprehensive dataset for analyzing customer consumption patterns and detecting anomalies using two different methods: **Prophet + Isolation Forest** and **Regression Analysis + Isolation Forest**. The visualizations and results allow for:
- Detailed comparisons between the methods.
- Insights into postpaid vs prepaid customer behavior.
- Identification of the most anomalous customers and understanding their consumption trends.

Each dataset and visualization serves a specific purpose in supporting this analysis and can be used to further investigate customer behavior, detect fraud, or optimize consumption forecasts.
