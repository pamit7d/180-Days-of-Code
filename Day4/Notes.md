# Day 4 - Exploratory Data Analysis (EDA) with Titanic Dataset

## Overview

Today I learned the fundamentals of **Exploratory Data Analysis (EDA)** using the famous Titanic dataset.

The goal of EDA is to understand the dataset before building any machine learning model. Through various visualizations and statistical summaries, I explored the structure of the data, identified patterns, and extracted meaningful insights.

---

## Dataset

**Source:** Kaggle Titanic Dataset

The dataset contains information about passengers aboard the Titanic, including:

* Passenger Class
* Age
* Gender
* Fare
* Family Information
* Survival Status

---

## Topics Covered

### 1. Downloading Data from Kaggle

Learned how to:

* Configure Kaggle API credentials
* Download datasets directly from Kaggle
* Extract downloaded files
* Load CSV files into Pandas

Tools used:

* Kaggle API
* Pandas

---

## 2. Understanding the Dataset

Before analyzing the data, I learned how to answer some important questions:

### How big is the dataset?

```python
df.shape
```

### What does the dataset look like?

```python
df.head()
df.sample()
```

### What are the data types?

```python
df.info()
```

### Are there missing values?

```python
df.isnull().sum()
```

### Are there duplicate records?

```python
df.duplicated().sum()
```

### Statistical Summary

```python
df.describe()
```

### Correlation Analysis

```python
df.corr(numeric_only=True)
```

---

# Exploratory Data Analysis (EDA)

EDA can be broadly divided into:

1. Univariate Analysis
2. Bivariate Analysis
3. Multivariate Analysis

---

# Univariate Analysis

Univariate Analysis focuses on understanding a single variable at a time.

---

## Categorical Data Analysis

### Count Plot

Used to count occurrences of each category.

Example:

```python
sns.countplot(data=df, x='Survived')
```

Insight:

* Most passengers did not survive.
* Approximately 38% survived.

---

### Pie Chart

Used to visualize category percentages.

Examples:

```python
df['Survived'].value_counts().plot(kind='pie')
```

```python
df['Pclass'].value_counts().plot(kind='pie')
```

```python
df['Sex'].value_counts().plot(kind='pie')
```

Insights:

* Majority of passengers traveled in 3rd class.
* Male passengers were significantly more than female passengers.

---

## Numerical Data Analysis

### Histogram

Used to understand the distribution of numerical values.

Example:

```python
df['Age'].plot(kind='hist')
```

Insight:

* Most passengers were between 20 and 40 years old.

---

### Distribution Plot

Used to understand:

* Data distribution
* Probability density
* Skewness

Example:

```python
sns.distplot(df['Age'], kde=True)
```

Learned about:

* Normal Distribution
* Skewness
* Density Curves

---

### Box Plot

Used for:

* Detecting outliers
* Understanding spread

Example:

```python
sns.boxplot(df['Fare'])
```

Learned:

* How to identify extreme values
* How outliers affect data analysis

---

# Bivariate and Multivariate Analysis

These analyses help understand relationships between two or more variables.

---

## Scatter Plot

Numerical vs Numerical

Example:

```python
sns.scatterplot(
    data=tips,
    x='total_bill',
    y='tip',
    hue='sex',
    size='size',
    style='smoker'
)
```

Learned:

* Relationship between variables
* Trend analysis
* Multi-dimensional visualization

---

## Line Plot

Numerical vs Numerical over time

Example:

```python
flights.groupby('year')['passengers'].sum().plot(kind='line')
```

Learned:

* Trend visualization
* Time-series patterns

---

## Bar Plot

Numerical vs Categorical

Examples:

```python
sns.barplot(
    data=df,
    x='Pclass',
    y='Fare',
    hue='Survived'
)
```

```python
sns.barplot(
    data=df,
    x='Pclass',
    y='Age',
    hue='Survived'
)
```

Learned:

* Comparing averages across categories
* Understanding grouped data

---

## Boxen Plot

Categorical vs Numerical

Example:

```python
sns.boxenplot(
    data=tips,
    x='day',
    y='total_bill',
    hue='sex'
)
```

Learned:

* Advanced distribution comparison
* Category-wise spread analysis

---

## KDE Plot

Numerical vs Categorical

Example:

```python
sns.kdeplot(
    data=df,
    x='Age',
    hue='Survived',
    fill=True
)
```

Learned:

* Comparing distributions
* Probability density estimation
* Survival patterns based on age

---

## Heatmap

Categorical vs Categorical

Example:

```python
sns.heatmap(
    pd.crosstab(df['Pclass'], df['Survived']),
    annot=True
)
```

Learned:

* Relationship between passenger class and survival
* Cross-tabulation analysis

---

## Cluster Map

Example:

```python
sns.clustermap(
    pd.crosstab(df['Parch'], df['Survived']),
    annot=True
)
```

Learned:

* Automatic grouping of similar patterns
* Cluster-based visualization

---

## Pair Plot

Example:

```python
sns.pairplot(iris, hue='species')
```

Learned:

* Relationship between all numerical variables
* Species separation patterns
* Quick dataset exploration

---

# Key Learnings

Today I learned:

✅ How to download datasets using the Kaggle API

✅ How to inspect and understand a dataset before analysis

✅ How to identify missing values and duplicates

✅ Difference between categorical and numerical data

✅ Univariate Analysis techniques

✅ Bivariate Analysis techniques

✅ Multivariate Analysis techniques

✅ Histogram, Countplot, Pie Chart, KDE Plot, Box Plot

✅ Scatter Plot, Line Plot, Bar Plot

✅ Heatmaps and Cluster Maps

✅ Pair Plots for feature exploration

✅ How visualizations help uncover hidden patterns in data

---

# Tools & Libraries Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Kaggle API

---

# Conclusion

This notebook provided a hands-on introduction to Exploratory Data Analysis (EDA). By analyzing the Titanic dataset, I learned how to summarize data, visualize distributions, study relationships between variables, and extract meaningful insights before moving to machine learning.

EDA is one of the most important steps in the data science workflow because better understanding of data leads to better models and better decisions.
