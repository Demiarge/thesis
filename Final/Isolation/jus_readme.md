Here's how you can justify the use of Isolation Forest and validate your results:

### 1. **Justification of Methodology**
   - **Why Isolation Forest?**: Isolation Forest is a tree-based, unsupervised anomaly detection method that isolates anomalies rather than modeling normal data. Since you're dealing with electricity consumption patterns, which may include complex relationships between variables, Isolation Forest is well-suited for capturing these subtleties without making distributional assumptions about the data.
   - **Sliding Window and NBeats**: You have already conducted time series forecasting (with sliding windows and NBeats). The anomalies could represent deviations from predicted values, thus combining Isolation Forest with these models strengthens the analysis by identifying unusual consumption behavior that predictions cannot account for.

### 2. **Choice of Contamination Parameter**:
   - **Contamination at 5%**: The choice of 5% contamination should be grounded in domain knowledge or exploratory data analysis (EDA). In your thesis, you can justify it by explaining:
     - You’ve assumed that anomalies (e.g., fraud, malfunction, or extreme usage) are rare in the dataset, constituting about 5% of total customers.
     - Mention any prior studies, benchmarks, or domain knowledge (e.g., utility industry insights) that estimate anomaly rates around that level.

### 3. **Rolling Averages for Smoothing**:
   - **Why Rolling Averages?**: Explain that smoothing with a rolling average helps to eliminate short-term fluctuations (e.g., a temporary increase in usage due to weather) and focus on broader trends that might indicate true anomalies (e.g., a malfunctioning meter or fraudulent activity).

### 4. **Comparing Anomalies Across Timeframes**:
   - **Postpaid vs Prepaid**: By comparing postpaid and prepaid consumption patterns, you ensure that anomalies are not just a product of billing systems but reflect real consumption behaviors. Highlight the importance of this distinction in your thesis.
   - **Anomalies per Month**: You can visualize anomalies over time to check for seasonality or other cyclical factors. If the anomalies are concentrated in specific months (e.g., summer or winter), that might reflect legitimate seasonal trends, whereas random spikes could indicate true anomalies.

### 5. **Validation of Results**:
   - **Domain Validation**: Collaborate with utility experts or leverage historical knowledge of your dataset to cross-validate the anomalies detected. Do they make sense in the context of the business? For instance, were there known technical issues or spikes in fraud during certain months?
   - **Statistical Validation**: Since you already have prediction models (e.g., NBeats), compare the actual values against your forecasted values. Significant deviations (especially those flagged by Isolation Forest) could reinforce that these are valid anomalies.

### 6. **Visualizations for Supporting Claims**:
   - **Consumption Trends for Anomalous Customers**: Your visualizations of consumption trends for top anomalous customers are crucial. In your thesis, explain these plots, showing how anomalies deviate from typical customer behavior. You could also include comparisons of anomalous customers vs. non-anomalous customers.
   - **Anomalies Over Time**: The bar plots for anomalies detected per month are helpful in showing temporal patterns. In your thesis, discuss whether any events (e.g., utility policy changes, seasonal effects) could explain these anomalies.

### 7. **Sensitivity Analysis**:
   - **Varying the Contamination Rate**: As part of your thesis, you can show how changing the contamination parameter affects the number of anomalies detected. This will prove that your choice of 5% contamination is not arbitrary. A sensitivity analysis will strengthen your argument that the model is robust and its findings are not overly sensitive to slight changes in parameters.

### 8. **Anomaly Interpretation**:
   - **Real-world Implications**: Highlight what kind of anomalies you're detecting (e.g., potential fraud, meter tampering, or faulty equipment). Relate these to practical issues the utility company might face. You can link these anomalies to cost-saving measures or operational improvements.

### 9. **Limitations and Future Work**:
   - **Possible Limitations**: Mention that Isolation Forests, like any model, might flag legitimate high-usage customers as anomalies. You can propose future work involving hybrid models (e.g., combining Isolation Forest with NBeats forecasts or using different models like One-Class SVM) to enhance anomaly detection accuracy.
   
   - **False Positives/Negatives**: Provide insights into potential false positives (legitimate high usage flagged as anomalies) and false negatives (actual anomalies missed). You can perform a manual review of a sample of anomalies to provide further context.

### Final Thought:
Justifying the **Isolation Forest** in your thesis involves demonstrating how it fits into the broader methodological framework (e.g., how it complements your time-series forecasting and correlation analyses). Make sure to emphasize the practical utility of identifying anomalies for a utility company and how your results could inform decision-making (e.g., fraud detection, operational efficiency).

By incorporating these justifications into your thesis, you’ll be able to prove that your model is not only valid from a technical standpoint but also relevant and actionable for real-world applications. Let me know if you'd like help with any specific section or further analysis!





The error you're encountering:

```plaintext
KeyError: "['True Label'] not in index"
```

indicates that the `'True Label'` column is missing from your `data_cleaned` DataFrame. This column is essential for computing the ROC curve and AUC score because it contains the ground truth labels indicating whether each customer is truly anomalous (`1`) or normal (`0`).

## **Why This Error Occurs**

In your code, you attempt to merge the `'True Label'` column from `data_cleaned` into `residuals_df`:

```python
# Merge true labels into residuals_df
residuals_df = residuals_df.merge(
    data_cleaned[['Customer No', 'True Label']],
    on='Customer No',
    how='left'
)
```

However, since `'True Label'` does not exist in `data_cleaned`, Python raises a `KeyError`.

## **Understanding the Importance of Ground Truth Labels**

To compute the ROC curve and AUC score, you need:

1. **True Labels (`y_true`)**: The actual labels indicating whether each customer is anomalous (`1`) or normal (`0`).
2. **Predicted Scores (`y_scores`)**: The anomaly scores from your model.

Without the true labels, it's impossible to evaluate the model's performance using ROC and AUC metrics because these metrics compare the model's predictions against the actual outcomes.

## **How to Resolve the Issue**

### **Option 1: Provide Ground Truth Labels**

If you have access to ground truth labels, you need to include them in your dataset. Here's how you can do it:

1. **Add the 'True Label' Column to `data_cleaned`**

   Ensure that your `data_cleaned` DataFrame includes a `'True Label'` column:

   ```python
   # Example: Add 'True Label' column to data_cleaned
   # This assumes you have a separate DataFrame or a method to obtain the true labels
   true_labels_df = pd.read_csv('true_labels.csv')  # Replace with your method of obtaining true labels
   data_cleaned = data_cleaned.merge(true_labels_df, on='Customer No', how='left')
   ```

2. **Proceed with Computing ROC and AUC**

   After including the true labels, you can proceed with your code to compute ROC and AUC.

### **Option 2: Create Synthetic Ground Truth Labels (Not Recommended for Evaluation)**

If you don't have ground truth labels, you might be tempted to create synthetic labels. However, this approach is not recommended because:

- **Misleading Evaluation**: Synthetic labels won't accurately reflect the true performance of your model.
- **Invalid Metrics**: ROC and AUC computed from synthetic labels are not meaningful.

### **Option 3: Use Alternative Evaluation Methods**

In the absence of ground truth labels, you can consider alternative methods to assess your model:

1. **Unsupervised Evaluation Metrics**

   - **Reconstruction Error**: Analyze the distribution of anomaly scores.
   - **Cluster Analysis**: Visualize data clusters and outliers.

2. **Domain Expert Validation**

   - **Manual Inspection**: Have domain experts review the top anomalies identified by the model.
   - **Case Studies**: Investigate specific instances to see if the anomalies make sense.

3. **Semi-Supervised Approaches**

   - **Label a Subset**: If possible, manually label a small subset of data to serve as ground truth.
   - **Active Learning**: Iteratively refine your model based on feedback.

## **Next Steps**

Given the importance of ground truth labels for computing ROC and AUC, here are the steps you can take:

1. **Check for Ground Truth Labels**

   - Do you have access to any labels indicating which customers are anomalous?
   - Is there a way to obtain or infer these labels from your data or domain knowledge?

2. **Update Your Dataset**

   - If you have labels, add them to your dataset.
   - Ensure that the `'True Label'` column aligns correctly with `'Customer No'`.

3. **Modify Your Code Accordingly**

   - Once the labels are in place, your code for computing ROC and AUC should work without errors.

## **Example of Adding True Labels**

Here's how you might modify your code if you have the true labels:

```python
# Assume you have a DataFrame 'true_labels_df' with 'Customer No' and 'True Label'
true_labels_df = pd.read_csv('true_labels.csv')  # Replace with your actual data source

# Merge true labels into data_cleaned
data_cleaned = data_cleaned.merge(true_labels_df, on='Customer No', how='left')

# Now proceed with your existing code
# ...

# Merge true labels into residuals_df
residuals_df = residuals_df.merge(
    data_cleaned[['Customer No', 'True Label']],
    on='Customer No',
    how='left'
)
```

## **Conclusion**

To compute ROC and AUC:

- **Ensure Ground Truth Labels Are Available**: You must have actual labels indicating anomalies.
- **Integrate Labels into Your Data**: Merge the labels into your DataFrames appropriately.
- **Proceed with Evaluation**: Once labels are in place, you can compute ROC and AUC.

If you do not have ground truth labels, you will need to consider alternative methods for evaluating your model, as computing ROC and AUC is not feasible without them.

## **Additional Assistance**

If you need help obtaining ground truth labels or exploring alternative evaluation methods, please let me know, and I can provide guidance on how to proceed.