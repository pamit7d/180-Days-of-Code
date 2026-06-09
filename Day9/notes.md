# ColumnTransformer & Preprocessing Notes

## Dataset
| age | gender | fever | cough  | city      |
| --- | ------ | ----- | ------ | --------- |
| 22  | Male   | 101.0 | Mild   | Delhi     |
| 35  | Female | 100.0 | Strong | Mumbai    |
| 28  | Male   | NaN   | Mild   | Kolkata   |
| 42  | Female | 103.0 | Strong | Bangalore |
| 31  | Male   | 99.0  | Mild   | Delhi     |
| 26  | Female | NaN   | Strong | Mumbai    |
| 38  | Male   | 102.0 | Mild   | Chennai   |
| 24  | Female | 100.0 | Strong | Kolkata   |
| 45  | Male   | 104.0 | Mild   | Bangalore |
| 29  | Female | 101.0 | Strong | Chennai   |

## Feature types
| Column | Type                | Description      | Preprocessing        |
| ------ | ------------------- | ---------------- | -------------------- |
| age    | Numerical           | Person's age     | Pass through / Scale |
| gender | Nominal Categorical | Male/Female      | OneHotEncoder        |
| fever  | Numerical           | Body temperature | SimpleImputer        |
| cough  | Ordinal Categorical | Mild < Strong    | OrdinalEncoder       |
| city   | Nominal Categorical | City name        | OneHotEncoder        |

## Dataset Workflow
1. Load dataset
2. Check structure: `df.info()`
3. Check missing values: `df.isnull().sum()`
4. Check numerical summary: `df.describe()`
5. Split data before preprocessing:
   ```python
   X_train, X_test, y_train, y_test = train_test_split(...)
   ```

---

# Pandas: Series vs DataFrame

| Syntax | Returns |
|----------|----------|
| `df['col']` | Series (1D) |
| `df[['col']]` | DataFrame (2D) |
| `df[['c1','c2']]` | DataFrame (2D) |

### Why it matters
Most sklearn transformers expect **2D input**.

✅ Correct:
```python
si.fit_transform(df[['fever']])
```

❌ Wrong:
```python
si.fit_transform(df['fever'])
```

---

# value_counts()

### Series
```python
df['brand'].value_counts()
```
Counts values in a single column.

### DataFrame
```python
df[['brand','model']].value_counts()
```
Counts unique row combinations.

---

# SimpleImputer

Used to fill missing values.

```python
si = SimpleImputer()
X_train_fever = si.fit_transform(X_train[['fever']])
X_test_fever = si.transform(X_test[['fever']])
```

### Rule
- `fit_transform()` → training data
- `transform()` → test data

---

# OrdinalEncoder

Used when categories have an order.

Example:

```python
Mild < Strong
```

```python
OrdinalEncoder(
    categories=[['Mild','Strong']]
)
```

Output:

```text
Mild   -> 0
Strong -> 1
```

### Common Mistake

❌ Wrong
```python
OrdinalEncoder(categories=['Mild','Strong'])
```

✅ Correct
```python
OrdinalEncoder(categories=[['Mild','Strong']])
```

Reason: sklearn expects one category list per feature.

---

# OneHotEncoder

Used for nominal (unordered) categories.

```python
OneHotEncoder(
    drop='first',
    sparse_output=False
)
```

### Parameters

| Parameter | Meaning |
|----------|----------|
| `drop='first'` | Avoid dummy variable trap |
| `sparse_output=False` | Return NumPy array instead of sparse matrix |

### Important
Fit only on training data:

```python
ohe.fit(X_train)
ohe.transform(X_test)
```

---

# Extracting Numerical Columns

```python
X_train_age = X_train[['age']].values
```

or

```python
X_train.drop(columns=[...]).values
```

---

# NumPy: Combining Features

## concatenate

```python
np.concatenate((a,b,c), axis=1)
```

- General-purpose join
- Must pass arrays inside a tuple/list
- `axis=1` → join columns
- `axis=0` → join rows

### Common Error

❌ Wrong
```python
np.concatenate(a,b,c)
```

✅ Correct
```python
np.concatenate((a,b,c), axis=1)
```

---

# Quick NumPy Stacking Reference

| Function | Purpose | New Axis? |
|----------|----------|----------|
| `np.concatenate()` | General join | ❌ |
| `np.hstack()` | Join columns | ❌ |
| `np.vstack()` | Join rows | ❌ |
| `np.column_stack()` | Make columns from 1D arrays | ❌ |
| `np.stack()` | Create new dimension | ✅ |

### ML Usage

```python
np.hstack([
    X_train_age,
    X_train_genCity,
    X_train_fever,
    X_train_cough
])
```

Equivalent to:

```python
np.concatenate([...], axis=1)
```

---

# ColumnTransformer

Purpose: Apply multiple preprocessing steps in one place.

```python
ColumnTransformer(
    transformers=[
        ('ord', OrdinalEncoder(...), ['cough']),
        ('imp', SimpleImputer(), ['fever']),
        ('ohe', OneHotEncoder(...), ['gender','city'])
    ],
    remainder='passthrough'
)
```

### Benefits
- No manual column extraction
- No manual concatenation
- Cleaner pipeline
- Less error-prone
- Production-friendly

---

# Core Interview Concepts

### Numerical Features
- Age
- Fever

### Categorical Features
- Gender
- City
- Cough

### Encoding Choice

| Feature | Type | Technique |
|----------|----------|----------|
| Age | Numerical | Pass/Scale |
| Fever | Numerical + Missing | SimpleImputer |
| Cough | Ordinal | OrdinalEncoder |
| Gender | Nominal | OneHotEncoder |
| City | Nominal | OneHotEncoder |

---

# Golden Rules

1. Split train/test before preprocessing.
2. Fit only on training data.
3. Transform test data using the fitted transformer.
4. Use DataFrames (`[['col']]`) with sklearn transformers.
5. Use OrdinalEncoder only when order exists.
6. Use OneHotEncoder for nominal categories.
7. `np.concatenate()` needs a tuple/list of arrays.
8. Prefer `ColumnTransformer` over manual preprocessing.
