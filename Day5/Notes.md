# Day 5 - Automated EDA & Introduction to Feature Engineering

## What I Learned Today

Today was not about learning machine learning algorithms.

Instead, I learned:

1. How to quickly understand a dataset using automated EDA tools.
2. What Feature Engineering is.
3. Why Feature Engineering is often more important than choosing a fancy machine learning algorithm.
4. A high-level overview of the major categories of Feature Engineering.
5. Some real-world package and dependency debugging while working with Python environments.

---

# Part 1: Automated EDA

## What problem does Automated EDA solve?

When we get a new dataset, we usually ask questions like:

* How many rows are there?
* How many columns are there?
* Which columns contain missing values?
* Which columns are numerical?
* Which columns are categorical?
* Are there any outliers?
* Are some columns highly correlated?

Normally we would write many lines of code to answer these questions.

Automated EDA tools generate a report automatically.

---

## Library Used

The instructor used:

```python
from pandas_profiling import ProfileReport
```

However, this library is outdated.

### Modern Alternative

```python
from ydata_profiling import ProfileReport
```

Installation:

```bash
pip install ydata-profiling
```

---

## Basic Usage

```python
from ydata_profiling import ProfileReport

profile = ProfileReport(df)

profile.to_file("report.html")
```

This generates an HTML report containing lots of information about the dataset.

---

## What information does the report provide?

### Overview

Quick summary of:

* Number of rows
* Number of columns
* Missing values
* Duplicate rows
* Memory usage

---

### Variable Analysis

For each column it shows:

For numerical columns:

* Mean
* Median
* Min
* Max
* Histogram

For categorical columns:

* Unique values
* Frequency counts

---

### Correlation Analysis

Shows which columns are related to each other.

At this stage, I only understand that:

> Correlation measures how strongly two variables move together.

I still need to learn correlation properly.

---

### Missing Value Analysis

Shows:

* Which columns have missing values.
* How many missing values exist.

This helps us decide what cleaning is required before training a model.

---

## My Biggest Takeaway

Automated EDA can save a lot of time.

But it does not replace thinking.

The report can show patterns.

I still need to understand what those patterns mean.

---

# Part 2: Introduction to Feature Engineering

## What is Feature Engineering?

My current understanding:

Feature Engineering is the process of preparing and improving data before giving it to a machine learning model.

Raw data is usually not ready for machine learning.

We often need to:

* Clean it
* Transform it
* Create better columns
* Remove unnecessary columns

before training a model.

---

## Why is Feature Engineering Important?

One idea from the lecture stood out:

A simple algorithm with excellent features can outperform a powerful algorithm with poor features.

This means:

```text
Good Data > Fancy Algorithm
```

This is probably one of the most important lessons so far.

---

# Types of Feature Engineering

Today I only learned the overview.

I have not learned the detailed techniques yet.

The four major categories are:

1. Feature Transformation
2. Feature Construction
3. Feature Selection
4. Feature Extraction

---

# 1. Feature Transformation

### My Current Understanding

We already have a column.

We change its form so it becomes more useful for machine learning.

---

## Example: Missing Values

Dataset:

```text
Age

22
35
NaN
18
```

Machine learning algorithms usually do not like missing values.

So we must handle them.

Today I learned that this process is called:

```text
Imputation
```

Some common approaches are:

* Mean
* Median
* Mode

I have not learned when to use each method yet.

---

## Example: Categorical Data

Dataset:

```text
Dog
Cat
Dog
Sheep
```

Machine learning models work with numbers.

Therefore categorical values must somehow be converted into numbers.

I learned that techniques such as encoding are used for this.

I will learn the details later.

---

## Example: Feature Scaling

Suppose:

```text
Age     = 20 to 60
Salary  = 20,000 to 500,000
```

Salary values are much larger.

Some algorithms can become biased toward large-scale features.

Therefore features are often scaled.

I learned the purpose but not the detailed mathematics yet.

---

## Example: Outliers

Outliers are values that are very different from the rest of the data.

Example:

```text
50
55
60
58
1000
```

1000 is probably an outlier.

I learned that outliers can negatively affect some algorithms.

I still need to learn how to detect and handle them properly.

---

# 2. Feature Construction

### My Current Understanding

Feature Construction means creating new columns using existing columns.

Example from Titanic dataset:

```text
SibSp
Parch
```

Can be combined into:

```text
FamilySize
```

The idea is that the new feature may contain more useful information than the original features.

---

# 3. Feature Selection

### My Current Understanding

Not all columns are useful.

Feature Selection means:

* Keeping useful columns
* Removing unnecessary columns

Benefits:

* Faster training
* Simpler models
* Less noise

I have not learned the actual selection techniques yet.

---

# 4. Feature Extraction

### My Current Understanding

This was the most difficult concept from today's lecture.

My understanding right now:

Feature Extraction creates entirely new features from existing features using mathematical techniques.

The instructor mentioned PCA.

At this point I only know:

* PCA is a Feature Extraction technique.
* It helps reduce dimensions.
* It creates new features from existing features.

I do not yet understand how PCA works internally.

---

# Real-World Debugging Lessons

Today's coding session taught me an important engineering lesson.

I encountered:

* Pydantic errors
* pandas-profiling compatibility issues
* ydata-profiling installation issues
* setuptools issues
* NumPy and Numba version conflicts
* Jupyter kernel caching problems

The biggest lesson:

Do not guess.

Debug systematically.

Process:

```text
Read Error
    ↓
Understand Error
    ↓
Check Versions
    ↓
Test Assumption
    ↓
Apply Fix
    ↓
Verify Fix
```

---

# Things I Need To Learn Next

* Missing Value Imputation in detail
* Encoding categorical variables
* Outlier Detection techniques
* Feature Scaling techniques
* Feature Selection methods
* PCA and Feature Extraction
* When to use each Feature Engineering technique

---

# One-Sentence Summary

Today I learned that successful machine learning is not only about choosing algorithms; a huge part of the work is understanding, cleaning, transforming, and improving the data before the model ever sees it.
