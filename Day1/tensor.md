# Tensor Notes (NumPy / Deep Learning)

## 1. What is a Tensor?

A tensor is simply a structured collection of numbers.

Examples:

### Scalar

```python
5
```

### Vector

```python
[1, 2, 3]
```

### Matrix

```python
[
 [1, 2, 3],
 [4, 5, 6]
]
```

All of the above are tensors.

The difference is only in **how the data is organized**.

---

# 2. Biggest Confusion: Two Meanings of "Dimension"

## A. Geometric Dimension

Example:

```python
[1, 2, 3]
```

Mathematically this can represent:

```text
(x, y, z)
```

which is a vector in 3D space.

Here:

- 3D means 3 coordinates.

---

## B. Tensor Dimension (NumPy / PyTorch)

Example:

```python
x = np.array([1, 2, 3])
```

This is a:

```text
1D Tensor
```

Why?

Because only one index is needed:

```python
x[0]
```

Tensor dimension means:

- Number of axes
- Number of indices needed to access one element

NOT physical space dimensions.

---

# 3. What is an Axis?

Axis = One organizational level of data.

Easy rule:

> Every new bracket level introduces a new axis.

Example:

```python
[
 [
   [1, 2, 3]
 ]
]
```

Axes:

```text
Axis 0 → outer bracket
Axis 1 → middle bracket
Axis 2 → inner bracket
```

Total:

```text
3 Axes
⇒ 3D Tensor
```

---

# 4. 0D Tensor (Scalar)

```python
x = np.array(5)
```

Visual:

```text
5
```

Access:

```python
x
```

No index needed.

Therefore:

```text
0 axes
⇒ 0D tensor
```

Shape:

```python
()
```

---

# 5. 1D Tensor (Vector)

```python
x = np.array([10, 20, 30])
```

Visual:

```text
[10 20 30]
```

Access:

```python
x[0]
```

One index needed.

Therefore:

```text
1 axis
⇒ 1D tensor
```

Shape:

```python
(3,)
```

Meaning:

```text
Axis 0 contains 3 elements
```

---

## Important Note

```python
[10,20,30]
```

can represent a vector in 3D space mathematically.

But as a tensor:

```text
1D Tensor
```

because only one axis exists.

---

# 6. 2D Tensor (Matrix)

```python
x = np.array([
 [1, 2, 3],
 [4, 5, 6]
])
```

Visual:

```text
[
 [1 2 3]
 [4 5 6]
]
```

Access:

```python
x[1][2]
```

Need:

- Row index
- Column index

Therefore:

```text
2 axes
⇒ 2D tensor
```

Axes:

```text
Axis 0 → Rows
Axis 1 → Columns
```

Shape:

```python
(2,3)
```

Meaning:

```text
2 rows
3 columns
```

---

# 7. 3D Tensor

```python
x = np.array([
 [
   [1, 2, 3],
   [4, 5, 6]
 ],
 [
   [7, 8, 9],
   [10,11,12]
 ]
])
```

Visual:

```text
[
 Matrix1,
 Matrix2
]
```

Access:

```python
x[1][0][2]
```

Need:

1. Which matrix
2. Which row
3. Which column

Therefore:

```text
3 axes
⇒ 3D tensor
```

Shape:

```python
(2,2,3)
```

Meaning:

```text
2 matrices
2 rows
3 columns
```

---

# 8. Shape vs Dimension

This is the most important distinction.

## Shape

Shape tells:

> How many elements exist along each axis.

Example:

```python
x.shape = (32, 224, 224, 3)
```

Means:

```text
Axis 0 → 32
Axis 1 → 224
Axis 2 → 224
Axis 3 → 3
```

---

## Dimension (Rank / ndim)

Dimension tells:

> Total number of axes.

Example:

```python
(32,224,224,3)
```

has:

```text
4 axes
⇒ 4D tensor
```

---

# 9. Image Tensor Example

```python
images.shape = (32, 224, 224, 3)
```

Meaning:

```text
32 images
224 pixel height
224 pixel width
3 color channels (RGB)
```

Axes:

```text
Axis 0 → Batch / Images
Axis 1 → Height
Axis 2 → Width
Axis 3 → Channel
```

Access:

```python
images[0][100][50][1]
```

Meaning:

```text
Image 0
Row 100
Column 50
Green Channel
```

---

## Common Mistake

Wrong:

```text
224 groups of matrices of size 224×3
```

Correct:

```text
224 rows
224 columns
3 color values (RGB) per pixel
```

---

# 10. Higher Dimensions Are Usually Logical

Many beginners think:

```text
2D = Sheet
3D = Cube
4D = Impossible
```

But in ML:

Axes usually mean:

```text
Batch
Time
Customer
Feature
Product
Token
Channel
Embedding
```

They are organizational levels.

Not physical dimensions.

---

# 11. 6D Tensor Story Example

Suppose:

```python
sales.shape = (3, 4, 30, 100, 50, 2)
```

Meaning:

| Axis | Meaning             |
| ---- | ------------------- |
| 0    | Cities              |
| 1    | Shops               |
| 2    | Days                |
| 3    | Customers           |
| 4    | Products            |
| 5    | Product Information |

Where:

```text
Index 0 → Quantity
Index 1 → Price
```

---

## Story

The company stores:

```text
For every city
    For every shop
        For every day
            For every customer
                For every product
                    Store quantity and price
```

---

# 12. 6D Access Example

```python
sales[2][1][10][25][5][0]
```

Meaning:

```text
City 2
Shop 1
Day 10
Customer 25
Product 5 (Pen)
Quantity
```

Suppose output:

```python
5
```

Meaning:

> Customer 25 bought 5 pens from shop 1 in city 2 on day 10.

---

Another example:

```python
sales[2][1][10][25][5][1]
```

Output:

```python
20
```

Meaning:

> The price of those pens was ₹20.

---

# 13. Ultimate Mental Model

Whenever you see a tensor, ask two questions.

## Question 1

How many indices are needed to access one value?

Answer:

```text
Dimension / Number of Axes
```

---

## Question 2

How many elements exist along each axis?

Answer:

```text
Shape
```

---

# 14. Final Summary

## Tensor

A structured collection of numbers.

---

## Axis

One organizational level of data.

---

## Dimension (Rank)

Total number of axes.

---

## Shape

Size along each axis.

---

# One-Line Intuition

More brackets:

```text
⇒ More axes
⇒ Higher dimensions
```

Numbers inside each bracket:

```text
⇒ Shape sizes
```

Example:

```text
Value
⇒ 0D

[List]
⇒ 1D

[List of Lists]
⇒ 2D

[List of List of Lists]
⇒ 3D
```

Tensor dimensions are usually:

```text
Organizational levels of data
NOT physical space dimensions.
```