# StandardScaler - Quick Revision Notes

## Dataset
- Social Network Ads dataset
- Features used:
  - `Age`
  - `EstimatedSalary`
- Target:
  - `Purchased`

---

## Train-Test Split
```python
train_test_split(X, y, test_size=0.3, random_state=0)
```
- 70% Training Data
- 30% Testing Data

---

## Why Scaling?
- `Age` range ≈ 18–60
- `EstimatedSalary` range ≈ 15,000–150,000
- Large-scale features can dominate distance calculations.
- Important for distance-based algorithms.

---

## StandardScaler
### Formula
\[
z = \frac{x - \mu}{\sigma}
\]

Where:
- μ = Mean
- σ = Standard Deviation

### Steps
```python
scaler = StandardScaler()

scaler.fit(X_train)
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Important Rule
- Fit only on training data.
- Transform both training and test data using the same scaler.

---

## After Scaling
- Mean ≈ 0
- Standard Deviation ≈ 1
- Data shape/distribution remains unchanged.
- Only scale changes.

---

## Visual Observations
### Scatter Plot
- Shape remains same.
- Scale changes.

### KDE Plot
- Distribution shape remains same.
- Features become comparable on same scale.

---

## Effect on Algorithms

### Logistic Regression
- Accuracy may remain similar.
- Scaling improves optimization/convergence speed.
- Example:
  - Without scaling: ~65 iterations
  - With scaling: ~7 iterations

### K-Nearest Neighbors (KNN)
- Strongly affected by feature scales.
- Usually performs better after scaling.
- Uses Euclidean distance.

### Decision Tree
- Usually unaffected.
- Does not depend on distance calculations.

---

## Outliers
### Does StandardScaler remove outliers?
❌ No

- Outliers still exist after scaling.
- StandardScaler scales all values, including outliers.
- Outliers must be handled separately.

---

## Exam / Interview Points

### Scale before:
- KNN
- K-Means
- SVM
- PCA
- Logistic Regression (recommended)
- Neural Networks

### Scaling usually not required:
- Decision Trees
- Random Forest
- XGBoost
- LightGBM

---

## Key Takeaways
1. StandardScaler makes features comparable.
2. Mean becomes 0 and std becomes 1.
3. Distribution shape does not change.
4. Essential for distance-based algorithms.
5. Helps gradient-based models converge faster.
6. Does NOT handle outliers.
