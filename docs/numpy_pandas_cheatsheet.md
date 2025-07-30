
## NumPy (Numerical Python)

NumPy is the fundamental package for numerical computing in Python. It provides support for large, multi-dimensional arrays and matrices, along with a collection of mathematical functions to operate on these arrays.

### Importing NumPy
```python
import numpy as np
```

### Creating Arrays
```python
# 1D array
arr1 = np.array([1, 2, 3])
print("np.array([1, 2, 3]) →", arr1)

# 2D array of zeros
arr2 = np.zeros((2, 3))
print("np.zeros((2, 3)) →\n", arr2)

# 1D array of ones
arr3 = np.ones(5)
print("np.ones(5) →", arr3)

# Identity matrix
arr4 = np.eye(3)
print("np.eye(3) →\n", arr4)

# Range of values with step
arr5 = np.arange(0, 10, 2)
print("np.arange(0, 10, 2) →", arr5)

# Evenly spaced values over interval
arr6 = np.linspace(0, 1, 5)
print("np.linspace(0, 1, 5) →", arr6)

```

---

## Array Operations
```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

print("a + b →", a + b)
print("a - b →", a - b)
print("a * b →", a * b)
print("a / b →", a / b)

print("np.dot(a, b) →", np.dot(a, b))  # 1×4 + 2×5 + 3×6 = 32

print("np.sum(a) →", np.sum(a))
print("np.mean(a) →", np.mean(a))
print("np.std(a) →", np.std(a))
print("np.max(a) →", np.max(a))
print("np.min(a) →", np.min(a))
print("np.argmin(a) →", np.argmin(a))  # index of min
```

---

## Indexing and Slicing
```python
# Create a 3x3 2D array
a2d = np.array([[10, 20, 30],
                [40, 50, 60],
                [70, 80, 90]])

# Access the first row
print("a2d[0] →", a2d[0])  # [10, 20, 30]

# Slice rows from index 1 to 2 (1:3 means rows 1 and 2)
print("a2d[1:3] →\n", a2d[1:3])  # [[40, 50, 60], [70, 80, 90]]

# Access the last row using negative index
print("a2d[-1] →", a2d[-1])  # [70, 80, 90]

# Access the second column from all rows
print("a2d[:, 1] →", a2d[:, 1])  # [20, 50, 80]

# Access all columns of the second row
print("a2d[1, :] →", a2d[1, :])  # [40, 50, 60]

# Slice a 2x2 subarray from rows 1-2 and columns 1-2
print("a2d[1:3, 1:3] →\n", a2d[1:3, 1:3])  # [[50, 60], [80, 90]]
```

---

## Random Numbers
NumPy provides utilities to generate random numbers, which are very useful in simulations, testing, and initializing machine learning models.

### `np.random.rand()` – Uniform distribution in [0, 1)

Generates random float numbers between 0 and 1.
```python
rand_array = np.random.rand(3, 2)
print("np.random.rand(3, 2) →\n", rand_array)
```
_Output:_ 3x2 matrix with random float values.

---

### `np.random.randint()` – Random integers in a specified range

Generates random integers from the given range.
```python
rand_int = np.random.randint(0, 10, size=(3, 3))
print("np.random.randint(0, 10, (3, 3)) →\n", rand_int)
```
_Output:_ 3x3 matrix with random integers from 0 to 9.

---

### `np.random.seed()` – Reproducibility

Sets a seed so that random numbers are the same across runs.
```python
np.random.seed(42)
print("Seed set to 42 (for reproducibility)")
```

---

## Useful Functions
```python
import numpy as np
# Create an array with duplicates, NaN, and Inf values
arr = np.array([4, 2, 2, np.nan, np.inf, 4])
print("Original array →", arr)

# Get unique elements from the array
print("np.unique(arr) →", np.unique(arr))

# Sort the array (NaN values usually appear last)
print("np.sort(arr) →", np.sort(arr))

# Concatenate two arrays
concat = np.concatenate(([1, 2], [3, 4]))
print("np.concatenate →", concat)

# Reshape a 2x3 array into 3x2
reshaped = np.array([[1, 2, 3], [4, 5, 6]]).reshape((3, 2))
print("np.reshape →\n", reshaped)

# Flatten the reshaped array into a 1D array
flat = reshaped.flatten()
print("np.flatten →", flat)

# Add an extra dimension (row vector)
exp_dim = np.expand_dims(np.array([1, 2]), axis=0)
print("np.expand_dims →", exp_dim)

# Check for NaN values
print("np.isnan(arr) →", np.isnan(arr))

# Check for infinite values
print("np.isinf(arr) →", np.isinf(arr))

# Find indices where value is 4
print("np.where(arr == 4) →", np.where(arr == 4))
```

## Pandas (Python Data Analysis Library)

Pandas is the **go‑to library** for working with **tabular data** (rows & columns). It builds on NumPy and adds labeled axes, powerful indexing, grouping, joining, time‑series tools, and I/O.

## Importing Pandas
```python
import pandas as pd
```

---

## Creating Data Structures

### Series (1D, labeled)
```python
s = pd.Series([1, 2, 3], name="values")
print(s)
print("Index:", s.index)
print("Values:", s.values)
```

### DataFrame (2D table)
```python
df = pd.DataFrame({'a': [1, 2], 'b': [3, 4]})
print(df)
print("Columns:", df.columns)
print("Shape:", df.shape)
```

---

## Input/Output

### Read
```python
df = pd.read_csv('file.csv')
df = pd.read_excel('file.xlsx')
df = pd.read_json('file.json')
```

### Write
```python
df.to_csv('out.csv', index=False)
```

---

## Inspecting Data
```python
df.head()
df.tail()
df.info()
df.describe()
df.shape
df.columns
df.dtypes
```

---

## Selecting Data

### Column selection
```python
df['col']
df[['col1','col2']]
```

### Row selection
```python
df.loc[0]
df.iloc[0]
```

### Boolean filtering
```python
df[df['col'] > 10]
df.query("col > 10")
```

---

## Cleaning Data
```python
df.dropna()
df.fillna(0)
df.replace('NA', pd.NA)
df.drop(columns=['col1'])
df.rename(columns={'a':'A'})
df['col'] = df['col'].astype(int)
df.duplicated()
df.drop_duplicates()
```

---

## Aggregation & Grouping
```python
df.groupby('group_col')['value'].mean()
df.groupby('group_col').agg({'value':'mean', 'other':'sum'})
pd.pivot_table(df, values='val', index='group', columns='type')
```

---

## Merging & Joining
```python
pd.concat([df1, df2])
pd.merge(df1, df2, on='key')
df1.join(df2.set_index('key'), on='key')
```

---

## Sorting
```python
df.sort_values('col', ascending=False)
df.sort_index()
```

---

## Time Series
```python
df['date'] = pd.to_datetime(df['date'])
df.set_index('date').resample('M').mean()
```

---

## Math & Stats
```python
df['col'].mean()
df['col'].std()
df.corr()
df['col'].cumsum()
```

---

## Apply & Map
```python
df['col'].apply(lambda x: x * 2)
df['label'].map({'A':1, 'B':2})
df.applymap(lambda x: len(str(x)))
```



---

⬅️ [Previous: Lists, Dictionaries, and Tuples](Lists_Dictionaries_Tuples.md) | [Next: Loops and Conditionals ➡️](Loops_and_Conditionals.md)
