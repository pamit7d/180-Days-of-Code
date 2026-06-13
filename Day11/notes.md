# Mathematical Transformations & Function Transformer (EDA + Feature Engineering Notes)

---

# The Story

Imagine you are a Data Scientist working at a bank.

Your manager asks:

> "Predict whether a customer will repay a loan."

You receive data like:

| Income |
|----------|
| 20,000 |
| 25,000 |
| 30,000 |
| 35,000 |
| 8,00,000 |

Immediately you notice one thing:

- Most values are small.
- A few values are extremely large.

This creates an **uneven distribution**.

Many Machine Learning algorithms learn better when data follows a **Normal Distribution**.

This is where **Transformations** come into the picture.

---

# What are Transformations?

## Definition

Transformations are mathematical operations applied to feature values to change their distribution.

### Goal

Convert skewed data into a more normal-looking distribution.

---

# Why are Transformations Used?

### 1. Reduce Skewness

Make the distribution more balanced.

### 2. Reduce Impact of Outliers

Large values stop dominating the model.

### 3. Improve Model Performance

Especially for:

- Linear Regression
- Logistic Regression
- Statistical Models

### 4. Satisfy Statistical Assumptions

Many statistical techniques assume data is normally distributed.

---

# Types of Transformations

## 1. Log Transformation

### Formula

```python
y = log(x)
```

### Best For

Right-skewed data.

### Example

```python
import numpy as np

df["income_log"] = np.log(df["income"])
```

### Before

```text
10, 100, 1000, 10000
```

### After

```text
1, 2, 3, 4
```

Large gaps become smaller.

---

## 2. Reciprocal Transformation

### Formula

```python
y = 1/x
```

### Best For

Highly right-skewed data.

### Example

```python
df["new_col"] = 1 / df["col"]
```

---

## 3. Square Transformation

### Formula

```python
y = x²
```

### Best For

Left-skewed data.

### Example

```python
df["new_col"] = df["col"] ** 2
```

---

## 4. Square Root Transformation

### Formula

```python
y = √x
```

### Best For

Moderately skewed data.

### Example

```python
import numpy as np

df["new_col"] = np.sqrt(df["col"])
```

---

# Function Transformer

---

## What is Function Transformer?

A Scikit-Learn transformer that allows us to apply any custom mathematical function to features.

---

## Why is it Used?

Instead of manually transforming data every time, we can include transformations inside an ML Pipeline.

Benefits:

- Cleaner code
- Reusable
- Pipeline friendly
- Prevents data leakage

---

## How Does it Work?

You provide a function.

Function Transformer applies that function to every value.

---

## Example 1: Log Transform

```python
from sklearn.preprocessing import FunctionTransformer
import numpy as np

transformer = FunctionTransformer(np.log1p)

X_transformed = transformer.fit_transform(X)
```

---

## Example 2: Square Transform

```python
from sklearn.preprocessing import FunctionTransformer

transformer = FunctionTransformer(
    lambda x: x**2
)

X_transformed = transformer.fit_transform(X)
```

---

## Example 3: Custom Transform

```python
transformer = FunctionTransformer(
    lambda x: x + 10
)
```

---

# Normal Distribution

---

## What is Normal Distribution?

A symmetric bell-shaped distribution.

```text
          /\
         /  \
        /    \
       /      \
------/--------\------
```

---

## Characteristics

- Mean ≈ Median ≈ Mode
- Symmetric
- Most values near center
- Fewer values at extremes

---

## Why is Normal Distribution Important?

Many statistical algorithms assume:

> "Data follows a normal distribution."

Examples:

- Linear Regression
- Logistic Regression
- Naive Bayes
- Statistical Hypothesis Tests

When data is normal:

- Better learning
- Stable coefficients
- Better statistical inference

---

# Skewness

---

## What is Skewness?

Measure of asymmetry in data.

Tells us whether data is tilted left or right.

---

## Formula Idea

Measures how far distribution is from symmetry.

---

## Types of Skewness

### 1. Zero Skew

```text
      /\
     /  \
    /    \
```

Perfectly normal.

```python
skew = 0
```

---

### 2. Right Skew (Positive)

```text
███████████
█████
██
█
```

Long tail on right side.

```python
skew > 0
```

Examples:

- Salary
- House Prices
- Income

---

### 3. Left Skew (Negative)

```text
        █
       ██
    █████
██████████
```

Long tail on left side.

```python
skew < 0
```

---

## How to Calculate?

```python
df["income"].skew()
```

---

## Why is Skewness Used?

To decide:

- Whether transformation is needed.
- Which transformation to apply.

---

# Kurtosis

---

## What is Kurtosis?

Measures the heaviness of tails.

Tells us whether extreme values (outliers) exist.

---

## Why is it Important?

Two datasets may have same mean and variance.

But one may contain many outliers.

Kurtosis helps detect that.

---

## Types

### 1. Mesokurtic

Normal distribution.

```python
kurtosis ≈ 3
```

---

### 2. Leptokurtic

Heavy tails.

More outliers.

```python
kurtosis > 3
```

---

### 3. Platykurtic

Light tails.

Fewer outliers.

```python
kurtosis < 3
```

---

## How to Calculate?

```python
df["income"].kurt()
```

---

## Where is Kurtosis Used?

- Outlier detection
- Risk analysis
- Financial data
- EDA

---

# How to Detect Normality?

---

## Method 1: Histogram

```python
import seaborn as sns

sns.histplot(df["income"])
```

Quick visual inspection.

---

## Method 2: Skewness

```python
df["income"].skew()
```

Near zero → More normal.

---

## Method 3: QQ Plot (Most Reliable)

### What?

Compares actual data with ideal normal distribution.

---

### Code

```python
import scipy.stats as stats
import matplotlib.pyplot as plt

stats.probplot(
    df["income"],
    dist="norm",
    plot=plt
)

plt.show()
```

---

### Interpretation

If points follow a straight line:

```text
•
  •
    •
      •
```

Data is approximately normal.

---

## Method 4: Statistical Tests

### Shapiro-Wilk Test

```python
from scipy.stats import shapiro

stat, p = shapiro(df["income"])
```

### Interpretation

```python
p > 0.05
```

Normal

```python
p < 0.05
```

Not Normal

---

# EDA Workflow for Numerical Features

Whenever you see a numerical column:

### Step 1

Check missing values.

```python
df.isnull().sum()
```

---

### Step 2

Plot histogram.

```python
sns.histplot(df["col"])
```

---

### Step 3

Check skewness.

```python
df["col"].skew()
```

---

### Step 4

Check kurtosis.

```python
df["col"].kurt()
```

---

### Step 5

Create QQ Plot.

```python
stats.probplot(df["col"], dist="norm", plot=plt)
```

---

### Step 6

Apply transformation if required.

- Right Skew → Log
- Heavy Right Skew → Reciprocal
- Left Skew → Square
- Mild Skew → Square Root

---

### Step 7

Recheck distribution.

Repeat Histogram + QQ Plot.

---

# Interview Questions

## Why do we use transformations?

To make data more normal and improve statistical model performance.

---

## Which transformation is best for right-skewed data?

Log Transformation.

---

## What is skewness?

Measure of asymmetry in a distribution.

---

## What is kurtosis?

Measure of tail heaviness and outliers.

---

## Why check normality before modeling?

Many statistical algorithms assume normal data.

---

## Which plot is best for checking normality?

QQ Plot.

---

# Quick Revision

### Transformation

Change data distribution using mathematical functions.

### Function Transformer

Scikit-Learn tool to apply custom mathematical transformations.

### Normal Distribution

Bell-shaped symmetric distribution.

### Skewness

Measures asymmetry.

### Kurtosis

Measures tail heaviness and outliers.

### QQ Plot

Checks how close data is to a normal distribution.

### Log Transform

Best for right-skewed data.

### Square Transform

Best for left-skewed data.

### Square Root Transform

Best for mild skewness.

### Reciprocal Transform

Best for highly skewed data.