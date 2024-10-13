### Comparing two approaches: 

1. **Normal Isolation Forest** (without any preprocessing like rolling average).
2. **Rolling Average + Isolation Forest** (smoothing the data before applying the Isolation Forest).

### Key Metrics to Compare:

1. **Number of Anomalies Detected**:
   - **Normal Isolation Forest**: 889 anomalies detected.
   - **Rolling Average + Isolation Forest**: 889 anomalies detected.
   - The same number of anomalies were detected in both approaches.

2. **Anomaly Score Distribution**:
   - **Normal Isolation Forest**:
     - Mean: 0.140620
     - Std (standard deviation): 0.065149
     - Minimum: -0.256389
     - Maximum: 0.207683
   - **Rolling Average + Isolation Forest**:
     - Mean: 0.122533
     - Std: 0.061143
     - Minimum: -0.266103
     - Maximum: 0.190819

   **Observations**:
   - The mean anomaly score is slightly lower with the rolling average, indicating that the anomalies may be a bit more pronounced (lower anomaly scores are more likely to be anomalies).
   - The standard deviation is smaller after applying the rolling average, suggesting that the scores are less spread out, i.e., there is less variability in the anomaly scores.
   - The minimum and maximum anomaly scores are also slightly more extreme (in both directions) with the rolling average, which might suggest the smoothing process amplifies certain trends.

### Visualizations:
1. **Consumption Trends for Top Anomalous Customers**:
   - With the **normal Isolation Forest**, you might have more variability and fluctuations, showing sharp peaks or troughs in consumption patterns.
   - With the **Rolling Average + Isolation Forest**, the trends are smoother, showing less fluctuation. This can help in making the underlying consumption trends more apparent.

2. **Anomalies Per Month**:
   - Both visualizations show the same number of anomalies per month, so no significant change is visible in terms of anomaly detection frequency.

### Which Approach is Better?

- **Normal Isolation Forest**:
  - May be better if you're looking for short-term spikes, sudden changes, or extreme consumption anomalies that could be masked by smoothing. 
  - Provides a more raw detection process, where noise and sharp changes are treated as anomalies.

- **Rolling Average + Isolation Forest**:
  - Useful if your goal is to detect more **consistent anomalies** or long-term trends by smoothing out noise.
  - This approach could be more effective if the consumption data is very noisy, as smoothing helps focus on more sustained patterns rather than short-term fluctuations.

### Conclusion:
- If you are primarily interested in **short-term variations**, stick with **normal Isolation Forest**.
- If you're more interested in **long-term consumption trends** or **reducing noise**, the **Rolling Average + Isolation Forest** may be the better option.

Ultimately, the choice depends on the nature of the anomalies you're looking to detect and whether you want to focus on short-term outliers or long-term patterns.



<div style="page-break-before: always; visibility: hidden"> 
\pagebreak 
</div>


The provided file contains data on anomalies detected in electricity consumption. Here's a breakdown of the key elements and what each part of the dataset represents:

### Columns Explanation:

1. **`Customer No`**:
   - This column represents the unique identifier for each customer in the dataset.

2. **Postpaid Consumption Columns (`post_*`)**:
   - These columns represent the electricity consumption (in units) during the postpaid billing period. The columns are named based on the months, such as `post_21_july_unit`, indicating the consumption in July 2021.

3. **Prepaid Consumption Columns (`pre_*`)**:
   - Similar to the postpaid columns, these represent electricity consumption during the prepaid billing period, such as `pre_23_july_unit` for July 2023. These columns allow for direct comparison between the postpaid and prepaid periods.

4. **`cmment`**:
   - This column seems to be empty or used for comments. It’s possible that it was intended to store additional information, but no data is provided here.

5. **`anomaly_label`**:
   - The Isolation Forest algorithm outputs a label for each entry:
     - `-1`: Indicates an anomaly (i.e., the customer's consumption pattern is considered unusual compared to others).
     - `1`: Indicates a normal consumption pattern.

6. **`anomaly_score`**:
   - This score represents the degree of "anomalousness" for each customer. A lower score means that the data point is more likely to be considered an anomaly. This score is derived from the Isolation Forest model’s decision function.

### Example Rows:

1. **Customer 17338245**:
   - This customer is flagged as an anomaly (`anomaly_label = -1`) with a relatively low anomaly score (`-0.139412`).
   - The customer has consumption data across several months for both postpaid and prepaid systems. For instance, in **July 2021**, the consumption is 963.5 units (postpaid), and in **July 2023**, the consumption is 952.075 units (prepaid).

2. **Customer 17449088**:
   - Also flagged as an anomaly (`anomaly_label = -1`) with an anomaly score of `-0.114334`.
   - Their consumption patterns show a decrease from 925 units in August 2021 (postpaid) to 966.845 units in July 2023 (prepaid).

### Key Takeaways:

- **Anomaly Detection**: 
   - The `anomaly_label` column indicates that the data points flagged as anomalies are considered unusual compared to others, based on the consumption patterns.
   - The `anomaly_score` helps quantify how anomalous a customer's consumption is. Lower values are more anomalous.

- **Postpaid vs Prepaid**:
   - The data allows for a comparison of each customer’s consumption during the postpaid and prepaid periods. By comparing values from columns like `post_21_july_unit` and `pre_23_july_unit`, you can examine how a customer’s consumption pattern changed after switching from postpaid to prepaid.

### Conclusion:

This dataset is useful for analyzing anomalies in consumption patterns. The results can help you understand which customers exhibit unusual usage patterns and whether these anomalies are more prevalent in either the postpaid or prepaid systems. You could use this data to further investigate the reasons behind these anomalies, such as billing errors, consumption habits, or meter issues.