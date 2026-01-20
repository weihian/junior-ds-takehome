### Question 1: Negative Package Weights

**Answer:**

No, I would not drop 25% of the dataset due to negative package weights. Removing a quarter of your data would significantly reduce the model's ability to learn patterns and could introduce bias if the erroneous values are not randomly distributed.

Instead, I would:

1. Investigate the Root Cause: First, understand why negative values exist. This could be:
   - Data entry errors (e.g., missing minus sign vs. incorrectly placed minus sign)
   - System bugs in the data collection process
   - Placeholder values indicating missing data

2. Apply Appropriate Imputation: Depending on the investigation:
   - Take absolute values if negatives appear to be sign errors (e.g., -5.0 should be 5.0)
   - Impute with median/mean of the package_weight column if they represent missing data
   - Use model-based imputation (e.g., predict package weight from other features like delivery time, distance)

3. Create an Indicator Feature: Add a binary feature `had_negative_weight` to capture whether this data quality issue might correlate with delivery time patterns. This preserves information about data quality that could be predictive.

4. Fix the Data Pipeline: Work with the data engineering team to prevent future occurrences of this issue.

In my implementation, I handled this within the scikit-learn pipeline using a `SimpleImputer` with median strategy, which ensures:
- No data leakage (imputation uses only training data statistics)
- Consistent preprocessing across train and test sets
- Preservation of all data points

---

### Question 2: Feature Value Assessment

**Answer:**

To determine if `traffic_level` is worth the API cost, I would perform a cost-benefit analysis combining model performance metrics with business value:

**1. Measure Feature Importance**

- Permutation Importance: Randomly shuffle the `traffic_level` feature in the test set and measure the drop in model performance. A large drop indicates high importance.
- Ablation Study: Train two models—one with `traffic_level` and one without—and compare their performance (MAE, RMSE, R²).
- Feature Importance Scores: Use the Random Forest's built-in feature importance to quantify contribution.

From my analysis, `traffic_level` appears in the top 2 features, suggesting it may be valuable for predictions.

**2. Continuous Monitoring**

Set up A/B testing:
- Run 10% of predictions without `traffic_level`
- Monitor both model performance and business KPIs (customer satisfaction, delivery accuracy)
- Adjust usage based on data

**3. Calculate Business Impact**

It is worth if the business value it brings is more than the API costs


