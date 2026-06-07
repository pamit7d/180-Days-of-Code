# Data Encoding in Machine Learning

## Why Encoding is Needed?

Machine Learning algorithms work with numbers, not text.

Example:

| Gender |
| ------ |
| Male   |
| Female |
| Male   |

A machine cannot directly understand "Male" or "Female".

So we convert categorical values into numerical values. This process is called **Encoding**.

---

# Types of Categorical Data

There are two major types of categorical data:

## 1. Nominal Data

Categories have **no order**.

Examples:

* Gender → Male, Female
* Color → Red, Blue, Green
* Fuel Type → Petrol, Diesel, CNG

❌ No ranking exists.

---

## 2. Ordinal Data

Categories have a **meaningful order**.

Examples:

| Review  |
| ------- |
| Poor    |
| Average |
| Good    |

or

| Education |
| --------- |
| School    |
| UG        |
| PG        |

✅ Ranking exists.

---

# Which Encoder Should Be Used?

| Data Type                 | Encoder         |
| ------------------------- | --------------- |
| Nominal Feature           | One Hot Encoder |
| Ordinal Feature           | Ordinal Encoder |
| Target Categorical Column | Label Encoder   |

---

# Golden Rule of Preprocessing

Before applying any encoding:

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(...)
```

### Important Rule

Always:

```python
encoder.fit(X_train)
encoder.transform(X_train)
encoder.transform(X_test)
```

Never:

```python
encoder.fit(X_test)
```

Why?

Because the model must learn only from training data.

Otherwise, **Data Leakage** occurs.

---

# 1. Ordinal Encoding

Used when categories have an order.

Example:

```python
review = ['Poor', 'Average', 'Good']
education = ['School', 'UG', 'PG']
```

Encoding:

| Original | Encoded |
| -------- | ------- |
| Poor     | 0       |
| Average  | 1       |
| Good     | 2       |

| Original | Encoded |
| -------- | ------- |
| School   | 0       |
| UG       | 1       |
| PG       | 2       |

---

## Applying Ordinal Encoder

```python
from sklearn.preprocessing import OrdinalEncoder

oe = OrdinalEncoder(
    categories=[
        ['Poor','Average','Good'],
        ['School','UG','PG']
    ]
)

X_train = oe.fit_transform(X_train)
X_test = oe.transform(X_test)
```

---

## Checking Learned Categories

```python
oe.categories_
```

Output:

```python
[
 ['Poor','Average','Good'],
 ['School','UG','PG']
]
```

---

## When to Use Ordinal Encoder?

✅ Reviews

* Poor
* Average
* Good

✅ Education

* School
* UG
* PG

✅ Ratings

* Low
* Medium
* High

❌ Gender

❌ City

❌ Color

These have no natural order.

---

# 2. Label Encoding

Used mainly for the **Target Variable (y)**.

Example:

| Purchased |
| --------- |
| Yes       |
| No        |
| Yes       |

After Encoding:

| Purchased |
| --------- |
| 1         |
| 0         |
| 1         |

---

## Applying Label Encoder

```python
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()

le.fit(y_train)

y_train = le.transform(y_train)
y_test = le.transform(y_test)
```

---

## Checking Classes

```python
le.classes_
```

Example:

```python
['No', 'Yes']
```

---

## When to Use Label Encoder?

✅ Target Column

```python
Purchased
Pass/Fail
Spam/Not Spam
```

❌ Feature Columns with Nominal Categories

Reason:

The model may assume:

```python
Male = 0
Female = 1
```

and think Female > Male, which is meaningless.

---

# 3. One Hot Encoding (OHE)

Used for **Nominal Data**.

Example:

Fuel Type:

| Fuel   |
| ------ |
| Petrol |
| Diesel |
| CNG    |

After One-Hot Encoding:

| Petrol | Diesel | CNG |
| ------ | ------ | --- |
| 1      | 0      | 0   |
| 0      | 1      | 0   |
| 0      | 0      | 1   |

Each category gets its own column.

No false ordering is introduced.

---

# One Hot Encoding using Pandas

```python
pd.get_dummies(
    cars_df,
    columns=['fuel', 'owner'],
    dtype=int
)
```

Output:

Creates new binary columns:

```text
fuel_Diesel
fuel_Petrol
fuel_CNG

owner_First
owner_Second
owner_Third
```

---

## Advantages of pd.get_dummies()

* Very easy
* Fast
* Good for data exploration

---

## Limitation

Cannot easily handle train-test separation.

Therefore, for Machine Learning pipelines, sklearn is preferred.

---

# K-1 One Hot Encoding

Also called:

**Drop First Encoding**

Instead of creating K columns, create K-1 columns.

Example:

Fuel Categories:

```text
Petrol
Diesel
CNG
```

Normally:

```text
Fuel_Petrol
Fuel_Diesel
Fuel_CNG
```

3 columns

After K-1 Encoding:

```text
Fuel_Diesel
Fuel_Petrol
```

Only 2 columns.

The missing category can be inferred.

---

## Why Use K-1 Encoding?

Prevents:

### Dummy Variable Trap

Perfect multicollinearity occurs because:

```text
Petrol + Diesel + CNG = 1
```

One column becomes predictable from others.

This can hurt Linear Regression models.

---

## Pandas Example

```python
pd.get_dummies(
    cars_df,
    columns=['fuel','owner'],
    drop_first=True,
    dtype=int
)
```

---

# One Hot Encoding using Sklearn

Preferred for Machine Learning projects.

---

## Train-Test Split

```python
x_train, x_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

---

## Apply Encoder

```python
from sklearn.preprocessing import OneHotEncoder

ohe = OneHotEncoder(
    drop='first',
    sparse_output=False
)

x_train_new = ohe.fit_transform(
    x_train[['fuel','owner']]
)

x_test_new = ohe.transform(
    x_test[['fuel','owner']]
)
```

---

## Combine Encoded and Numerical Features

```python
import numpy as np

final_train = np.hstack(
    (
        x_train[['brand','km_driven']].values,
        x_train_new
    )
)
```

`np.hstack()` combines arrays horizontally.

---

# Handling High Cardinality

## Problem

Some columns contain too many categories.

Example:

```python
brand
```

May have:

```text
Maruti
Hyundai
Toyota
Honda
BMW
Audi
Mercedes
...
```

Hundreds of brands.

One-Hot Encoding creates hundreds of columns.

This causes:

* More memory usage
* Slower training
* Sparse dataset

---

# Top Category Encoding

Keep common categories.

Group rare categories into:

```text
uncommon
```

---

## Step 1: Count Categories

```python
cars_df['brand'].value_counts()
```

---

## Step 2: Set Threshold

```python
threshold = 100
```

---

## Step 3: Find Rare Categories

```python
count = cars_df['brand'].value_counts()

repl = count[count <= threshold].index
```

---

## Step 4: Replace Rare Categories

```python
cars_df['brand'].replace(
    repl,
    'uncommon'
)
```

---

## Step 5: Apply One Hot Encoding

```python
pd.get_dummies(
    cars_df['brand'].replace(
        repl,
        'uncommon'
    ),
    dtype=int
)
```


# Better Approach: ColumnTransformer

So far we manually did:

```python
ohe = OneHotEncoder(drop='first', sparse_output=False)

x_train_new = ohe.fit_transform(
    x_train[['fuel','owner']]
)

x_test_new = ohe.transform(
    x_test[['fuel','owner']]
)

final_train = np.hstack(
    (
        x_train[['brand','km_driven']].values,
        x_train_new
    )
)
```

This works, but becomes difficult when:

* Many columns need different preprocessing
* Some columns need encoding
* Some columns need scaling
* Some columns need no transformation

Managing everything manually becomes messy.

---

# ColumnTransformer to the Rescue

`ColumnTransformer` allows us to:

* Apply different preprocessing to different columns
* Keep all transformations in one place
* Automatically combine transformed columns
* Avoid using `np.hstack()`
* Build clean ML pipelines

---

# Problem Without ColumnTransformer

Suppose we have:

| Column    | Type      |
| --------- | --------- |
| fuel      | Nominal   |
| owner     | Nominal   |
| education | Ordinal   |
| km_driven | Numerical |
| age       | Numerical |

Without ColumnTransformer we would need:

1. Apply OneHotEncoder on fuel and owner
2. Apply OrdinalEncoder on education
3. Keep numerical columns unchanged
4. Combine everything using `np.hstack()`

This quickly becomes difficult to maintain.

---

# Solution Using ColumnTransformer

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder, OrdinalEncoder

transformer = ColumnTransformer(
    transformers=[
        (
            'ohe',
            OneHotEncoder(drop='first'),
            ['fuel', 'owner']
        ),
        (
            'ordinal',
            OrdinalEncoder(
                categories=[
                    ['School', 'UG', 'PG']
                ]
            ),
            ['education']
        )
    ],
    remainder='passthrough'
)
```

### Explanation

`remainder='passthrough'`

means:

"Keep all remaining columns unchanged."

Therefore:

```text
fuel        -> OneHotEncoder
owner       -> OneHotEncoder
education   -> OrdinalEncoder
age         -> unchanged
km_driven   -> unchanged
```

---

# Fit and Transform

```python
x_train_transformed = transformer.fit_transform(x_train)

x_test_transformed = transformer.transform(x_test)
```

That's it.

No:

```python
np.hstack()
```

No:

```python
manual column combining
```

No:

```python
multiple transformation steps
```

---

# Complete Example

```python
from sklearn.model_selection import train_test_split
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.linear_model import LinearRegression

X = df.drop('selling_price', axis=1)
y = df['selling_price']

x_train, x_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)

preprocessor = ColumnTransformer(
    transformers=[
        (
            'fuel_encoder',
            OneHotEncoder(drop='first'),
            ['fuel']
        ),
        (
            'owner_encoder',
            OneHotEncoder(drop='first'),
            ['owner']
        )
    ],
    remainder='passthrough'
)

x_train_processed = preprocessor.fit_transform(x_train)
x_test_processed = preprocessor.transform(x_test)

model = LinearRegression()

model.fit(x_train_processed, y_train)

print(model.score(x_test_processed, y_test))
```

---

# Even Better: Using Pipeline

In real-world projects we often combine:

```text
ColumnTransformer
        +
Machine Learning Model
```

inside a Pipeline.

```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.linear_model import LinearRegression

preprocessor = ColumnTransformer(
    transformers=[
        (
            'fuel',
            OneHotEncoder(drop='first'),
            ['fuel']
        ),
        (
            'owner',
            OneHotEncoder(drop='first'),
            ['owner']
        )
    ],
    remainder='passthrough'
)

pipe = Pipeline([
    ('preprocessing', preprocessor),
    ('model', LinearRegression())
])

pipe.fit(x_train, y_train)

predictions = pipe.predict(x_test)
```

---

# Why Pipeline is Preferred

Advantages:

✅ Cleaner code

✅ No manual transformations

✅ No np.hstack()

✅ Prevents preprocessing mistakes

✅ Easier deployment

✅ Industry standard

---

# Interview Tip

For learning:

```text
OrdinalEncoder
LabelEncoder
OneHotEncoder
```

should be practiced individually.

For real projects:

```text
ColumnTransformer + Pipeline
```

is usually the preferred approach.



---

# Summary

| Encoder               | Use Case                     |
| --------------------- | ---------------------------- |
| OrdinalEncoder        | Ordered categorical features |
| LabelEncoder          | Target categorical column    |
| OneHotEncoder         | Nominal features             |
| K-1 Encoding          | Avoid multicollinearity      |
| Top Category Encoding | High-cardinality features    |

---

# Interview Questions

### Q1. Difference between Nominal and Ordinal Data?

**Nominal:** No order

Example:

```text
Red, Blue, Green
```

**Ordinal:** Has order

Example:

```text
Poor < Average < Good
```

---

### Q2. Why not use LabelEncoder on Nominal Features?

Because it creates artificial ordering.

Example:

```text
Red = 0
Blue = 1
Green = 2
```

Model may think:

```text
Green > Blue > Red
```

which is incorrect.

---

### Q3. Why use Drop First?

To avoid:

```text
Dummy Variable Trap
```

and multicollinearity.

---

### Q4. Why fit only on training data?

To prevent:

```text
Data Leakage
```

and obtain realistic model performance.

---

# Final Flow

```text
Raw Data
    │
    ▼
Train-Test Split
    │
    ▼
Ordinal Features
    → OrdinalEncoder
    │
    ▼
Nominal Features
    → OneHotEncoder
    │
    ▼
Target Column
    → LabelEncoder
    │
    ▼
Combine Features
    │
    ▼
Train Model
```
