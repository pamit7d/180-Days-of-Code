# Min-Max Scaling (Practical Understanding from Notebook)

> This notebook is focused on understanding what Min-Max Scaling actually does to data visually and mathematically.

---

# Dataset Used

Wine Quality Dataset

Selected Columns:

```python
df = df[['quality', 'alcohol', 'citric acid']]
```

Features:

```text
alcohol
citric acid
```

Target:

```text
quality
```

---

# Step 1: Visualize Original Data

Scatter Plot:

```python
sns.scatterplot(
    data=df,
    x='alcohol',
    y='citric acid',
    hue='quality'
)
```

Purpose:

* Understand relationship between features.
* Observe distribution before scaling.
* Check how different quality classes are distributed.

---

# Step 2: Check Feature Distributions

KDE Plots:

```python
sns.kdeplot(df['alcohol'])
sns.kdeplot(df['citric acid'])
```

Purpose:

* Understand spread of features.
* Check scale differences.
* Observe distributions before scaling.

---

# Step 3: Train-Test Split

```python
x_train, x_test, y_train, y_test = train_test_split(
    df.drop('quality', axis=1),
    df['quality'],
    test_size=0.3,
    random_state=0
)
```

Important Rule:

Always split data before scaling.

Correct:

```text
Split → Fit Scaler on Train → Transform Train/Test
```

Wrong:

```text
Scale Entire Dataset → Split
```

Why?

Because it causes:

```text
Data Leakage
```

Information from the test set would leak into training.

---

# Step 4: Apply Min-Max Scaling

```python
scaler = MinMaxScaler()

scaler.fit(x_train)

scaled_x_train = scaler.transform(x_train)
scaled_x_test = scaler.transform(x_test)
```

---

# Important Concept

Notice:

```python
scaler.fit(x_train)
```

NOT

```python
scaler.fit(x_test)
```

and NOT

```python
scaler.fit(df)
```

---

# What Does fit() Learn?

MinMaxScaler stores:

```text
Training Minimum
Training Maximum
```

for every feature.

Example:

Suppose training data contains:

Alcohol:

```text
min = 8
max = 15
```

Scaler remembers:

```text
8
15
```

and uses them forever.

---

# What Does transform() Do?

Formula:

```text
x' = (x - xmin) / (xmax - xmin)
```

using the training min and max.

---

# Important Interview Question

## Why is Training Data Between [0,1] But Test Data May Not Be?

Many students think:

```text
MinMaxScaler always produces values between 0 and 1
```

This is NOT always true.

---

Suppose:

Training Alcohol Range:

```text
8 to 15
```

Scaler learns:

```text
xmin = 8
xmax = 15
```

Now suppose test set contains:

```text
16
```

Then:

```text
(16 - 8)/(15 - 8)
=
8/7
=
1.14
```

Output:

```text
1.14
```

which is greater than 1.

---

Similarly:

If test value is:

```text
7
```

Output becomes:

```text
-0.14
```

which is less than 0.

---

# Conclusion

Training data:

```text
Usually within [0,1]
```

Test data:

```text
Can be less than 0
Can be greater than 1
```

This is expected behavior.

---

# Why Don't We Fit on Test Data Too?

Because:

```text
That would leak information
from the future into training.
```

This is called:

```text
Data Leakage
```

and must be avoided.

---

# Step 5: Compare Statistics

Before Scaling:

```python
df.describe()
```

After Scaling:

```python
scaled_x_train.describe()
```

Observation:

* Values are compressed.
* Relative ordering remains same.
* Units disappear.
* Features become comparable.

---

# Step 6: Scatter Plot Before vs After Scaling

```python
sns.scatterplot(...)
```

Original Data

vs

Scaled Data

---

# Important Observation

The overall shape remains almost identical.

Why?

Because Min-Max Scaling performs a linear transformation.

It stretches or compresses the axes.

It does NOT change:

```text
Relative structure
Pattern
Correlation
Ordering
```

---

# What Actually Changes?

Changes:

```text
Scale
Magnitude
Distances
```

Does NOT Change:

```text
Relationships
Patterns
Rankings
```

---

# Step 7: Correlation Check

Notebook verifies:

```python
print(x_train.corr())
print(scaled_x_train.corr())
```

---

# Key Observation

Correlation before scaling:

```text
Same
```

Correlation after scaling:

```text
Same
```

---

# Why?

Correlation measures:

```text
Relationship
```

not magnitude.

Min-Max Scaling is a linear transformation.

Linear transformations preserve correlation.

---

# Interview Question

Does Min-Max Scaling change correlation?

Answer:

```text
No
```

because it is a linear transformation.

---

# Step 8: Distance Check

Notebook computes:

```python
from scipy.spatial.distance import pdist
```

Purpose:

```text
Check pairwise distances
```

---

# Observation

Distances change after scaling.

---

# Why?

Because coordinates themselves changed.

Example:

Original:

```text
(100, 10)
```

Scaled:

```text
(1.0, 0.2)
```

Distance calculations are now completely different.

---

# Important Insight

Min-Max Scaling preserves:

```text
Shape
Structure
Correlation
```

but changes:

```text
Distances
Magnitudes
Scale
```

This is exactly why scaling helps algorithms such as:

* KNN
* K-Means
* SVM

because they depend heavily on distances.

---

# Step 9: KDE Plot Comparison

Notebook compares:

```python
Original KDE
Scaled KDE
```

---

# Observation

Shape remains almost identical.

Only axis values change.

---

# Why?

Scaling changes location and spread.

It does not fundamentally change the distribution shape.

---

# Important KDE Confusion

Sometimes KDE curves extend beyond:

```text
[0,1]
```

even after Min-Max Scaling.

---

Students often think:

```text
Scaling failed
```

but that's incorrect.

---

# Why Does This Happen?

KDE is:

```text
Kernel Density Estimation
```

It is a smooth approximation of the distribution.

The smoothing process extends slightly beyond actual observations.

---

Actual scaled data:

```text
0 to 1
```

KDE curve:

```text
May appear slightly outside
0 to 1
```

This is normal.

---

# Final Mental Model

Think of Min-Max Scaling as:

```text
Taking a rubber sheet
containing your data
and stretching/compressing it
to fit inside a fixed box.
```

The shape stays similar.

The positions become scaled.

---

# What Min-Max Scaling Changes

✓ Scale

✓ Magnitude

✓ Distances

✓ Numerical Range

---

# What Min-Max Scaling Does NOT Change

✓ Correlation

✓ Relative Ordering

✓ Relationships

✓ Overall Structure

✓ Distribution Shape (mostly)

---

# Most Important Takeaways

1. Fit scaler only on training data.

2. Transform both training and test data using the same scaler.

3. Test values can be outside [0,1].

4. Correlation remains unchanged.

5. Distances change.

6. Scatter plot structure remains similar.

7. KDE shape remains similar.

8. Min-Max Scaling is a linear transformation.

9. Scaling helps distance-based algorithms.

10. Never scale the entire dataset before train-test split.


# -------------------------------------------------------------------------------------------------------------------------------------------------------------


# Feature Scaling Notes

# Table of Contents

1. Why Feature Scaling?
2. Standardization (Z-Score Scaling)
3. Normalization (Min-Max Scaling)
4. Mean Normalization
5. MaxAbs Scaling
6. Robust Scaling
7. Standardization vs Normalization
8. Which Scaling Technique Should I Use?
9. Important Interview Notes
10. Summary Table

---

# 1. Why Feature Scaling?

Machine Learning algorithms often work with numerical features that may have very different ranges.

Example:

| Feature | Range              |
| ------- | ------------------ |
| Age     | 18 - 60            |
| Salary  | 20,000 - 2,000,000 |

If scaling is not applied:

* Large-scale features dominate small-scale features.
* Distance-based algorithms become biased.
* Optimization algorithms converge slowly.

Feature scaling brings all features to a comparable scale.

---

# 2. Standardization (Z-Score Scaling)

## Formula

```text
z = (x - μ) / σ
```

Where:

* x = current value
* μ = mean of feature
* σ = standard deviation

---

## Intuition

### Step 1

Subtract Mean

```text
x - μ
```

Moves the entire distribution so that its center becomes 0.

---

### Step 2

Divide by Standard Deviation

```text
(x - μ) / σ
```

Scales the spread of the data.

---

## Result

After standardization:

* Mean = 0
* Standard Deviation = 1

Values are no longer bounded.

Example:

```text
-3.5
-1.2
0
1.8
4.1
```

are all possible.

---

## When to Use?

Most commonly used scaling technique.

Works very well for:

* Linear Regression
* Logistic Regression
* SVM
* PCA
* Neural Networks
* KNN (usually)

---

## Scikit-Learn

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)
```

---

# 3. Normalization (Min-Max Scaling)

## Formula

```text
x' = (x - xmin) / (xmax - xmin)
```

Where:

* xmin = minimum value in the feature
* xmax = maximum value in the feature

---

## Intuition

Take every value and compress it between 0 and 1.

### Example

Original:

```text
10
20
30
40
50
```

After Min-Max Scaling:

```text
0
0.25
0.5
0.75
1
```

---

## Result

Range becomes:

```text
[0,1]
```

Sometimes:

```text
[-1,1]
```

depending on implementation.

---

## When to Use?

When minimum and maximum values are known and meaningful.

Common in:

* Image Processing
* Deep Learning
* CNNs

---

### Image Example

Pixel values:

```text
0 - 255
```

Normalize:

```text
pixel / 255
```

Result:

```text
0 - 1
```

This is one of the most common forms of normalization.

---

## Scikit-Learn

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

X_scaled = scaler.fit_transform(X)
```

---

# 4. Mean Normalization

A less commonly used normalization technique.

## Formula

```text
x' = (x - μ) / (xmax - xmin)
```

Where:

* μ = mean of the feature
* xmin = minimum value
* xmax = maximum value

---

## Intuition

### Step 1

Subtract mean

```text
x - μ
```

Centers data around zero.

---

### Step 2

Divide by range

```text
xmax - xmin
```

Controls overall scale.

---

## Result

Typically values lie around:

```text
[-1,1]
```

(not guaranteed)

---

## Interpretation

If:

```text
x < mean
```

Result is negative.

If:

```text
x > mean
```

Result is positive.

---

## Why Rarely Used?

Standardization usually performs better and is already available in Scikit-Learn.

Therefore most practitioners directly use:

```python
StandardScaler()
```

instead.

---

## Additional Note

This scaler is rarely used in real-world ML projects.

There is no dedicated Scikit-Learn transformer specifically for Mean Normalization.

If needed, it is usually implemented manually.

---

# 5. MaxAbs Scaling

Useful for sparse datasets.

## Formula

```text
x' = x / |xmax|
```

Where:

```text
|xmax|
```

is the largest absolute value in the feature.

---

## Example

Original:

```text
-50
0
25
100
```

Maximum absolute value:

```text
100
```

Scaled:

```text
-0.5
0
0.25
1
```

---

## Intuition

Divide everything by the largest magnitude.

Largest value becomes:

```text
1
```

Smallest becomes:

```text
-1
```

---

## Result

Range:

```text
[-1,1]
```

---

## Major Advantage

Preserves zeros.

If value is:

```text
0
```

it remains:

```text
0
```

---

## When to Use?

Useful for sparse data.

Examples:

* Text data
* Bag-of-Words
* TF-IDF matrices
* Sparse matrices

because such datasets contain many zeros.

---

## Scikit-Learn

```python
from sklearn.preprocessing import MaxAbsScaler

scaler = MaxAbsScaler()

X_scaled = scaler.fit_transform(X)
```

---

# 6. Robust Scaling

Designed for datasets containing outliers.

---

## Formula

```text
x' = (x - Median) / IQR
```

Where:

```text
IQR = Q3 - Q1
```

and

```text
Q3 = 75th percentile
Q1 = 25th percentile
```

---

## Intuition

Instead of using:

* Mean
* Standard Deviation

which are sensitive to outliers,

Robust Scaling uses:

* Median
* IQR

which are resistant to outliers.

---

## Example

Dataset:

```text
10
12
15
17
20
5000
```

Here:

```text
5000
```

is an outlier.

---

### Problem

Mean gets distorted.

StandardScaler may become less effective.

---

### Solution

Median remains stable.

RobustScaler works much better.

---

## When to Use?

Whenever dataset contains:

* Extreme values
* Outliers
* Heavy-tailed distributions

---

## Scikit-Learn

```python
from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()

X_scaled = scaler.fit_transform(X)
```

---

# 7. Standardization vs Normalization

This is one of the most common interview questions.

---

## Standardization

Formula:

```text
z = (x - μ) / σ
```

Properties:

* Mean becomes 0
* Standard deviation becomes 1
* No fixed range
* Works well for many ML algorithms
* Most commonly used

---

## Normalization

Formula:

```text
x' = (x - xmin) / (xmax - xmin)
```

Properties:

* Fixed range
* Usually [0,1]
* Sensitive to outliers
* Useful when min and max are meaningful

---

## Visual Difference

### Standardization

Before:

```text
10 20 30 40 50
```

After:

```text
-1.41
-0.71
0
0.71
1.41
```

---

### Normalization

Before:

```text
10 20 30 40 50
```

After:

```text
0
0.25
0.50
0.75
1
```

---

# 8. Which Scaling Technique Should I Use?

## Rule 1

First ask:

**Does this algorithm even require scaling?**

Examples:

### Usually Need Scaling

* KNN
* K-Means
* SVM
* PCA
* Neural Networks
* Gradient Descent based methods

### Usually Don't Need Scaling

* Decision Trees
* Random Forest
* XGBoost
* LightGBM
* CatBoost

---

## Rule 2

If unsure, start with:

```python
StandardScaler()
```

It works well in most practical situations.

---

## Rule 3

Use Min-Max Scaling when:

* Data has known bounds
* Deep Learning
* Image Processing

Example:

```text
0 - 255 pixels
```

---

## Rule 4

Use RobustScaler when:

* Outliers are present

---

## Rule 5

Use MaxAbsScaler when:

* Sparse matrices
* Lots of zeros

---

# 9. Important Interview Notes

### Q1. Why is StandardScaler more popular?

Because:

* Works well in most datasets.
* Centers data around zero.
* Many ML algorithms assume zero-centered data.

---

### Q2. Which scaler is best for outliers?

Answer:

```text
RobustScaler
```

because it uses Median and IQR.

---

### Q3. Which scaler preserves zeros?

Answer:

```text
MaxAbsScaler
```

---

### Q4. Which scaler is commonly used in image processing?

Answer:

```text
MinMaxScaler / Pixel Normalization
```

---

### Q5. Which algorithms generally do not require scaling?

Answer:

```text
Decision Trees
Random Forest
XGBoost
LightGBM
CatBoost
```

---

# 10. Summary Table

| Scaler             | Formula Basis      | Outlier Resistant? | Output Range  | Best Use Case      |
| ------------------ | ------------------ | ------------------ | ------------- | ------------------ |
| StandardScaler     | Mean, Std Dev      | No                 | Unbounded     | General ML         |
| MinMaxScaler       | Min, Max           | No                 | [0,1]         | Images, DL         |
| Mean Normalization | Mean, Range        | No                 | Around [-1,1] | Rarely used        |
| MaxAbsScaler       | Max Absolute Value | No                 | [-1,1]        | Sparse Data        |
| RobustScaler       | Median, IQR        | Yes                | Unbounded     | Outlier-heavy Data |

---

# Practical Takeaway

If you don't know which scaler to use:

1. Try StandardScaler first.
2. If outliers exist → Try RobustScaler.
3. If data has fixed bounds (e.g. images) → Use MinMaxScaler.
4. If data is sparse with many zeros → Use MaxAbsScaler.
5. Compare model performance using cross-validation and choose the best performer.

Feature scaling is not about following rules blindly; it is about experimenting and observing which transformation helps the model learn better.
