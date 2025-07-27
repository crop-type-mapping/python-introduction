Functions and file input/output (I/O) are essential tools in Python that allow you to:

- Reuse blocks of code
- Organize logic into manageable units
- Read and write files for automation and reporting

---

#### Defining and Using Functions

Functions are declared using the `def` keyword.  
They can accept **parameters** and return **results**.

```python
# Define a function to calculate yield
def calculate_yield(area, rate):
    return area * rate

# Use the function
result = calculate_yield(10, 2.5)
print("Total Yield:", result)
```

**Tips**:

- Functions improve modularity and reusability.
- You can return multiple values using tuples.

---

#### Built-in Python Functions

Python provides a rich set of built-in functions:

```python
crops = ['Maize', 'Beans']

print(len(crops))       # Count items
print(type(crops))      # Type of variable
print(sorted(crops))    # Sorted list
```

---

#### Reading from Files

Use the built-in `open()` function to read content from files (e.g., text or CSV).

```python
# Reading a file line by line
with open('crop_names.txt', 'r') as file:
    for line in file:
        print("Crop:", line.strip())
```

**Common modes**:

- `'r'`: read
- `'w'`: write (overwrites)
- `'a'`: append
- `'rb'`: read binary

---

#### Writing to Files

To save data or reports to a file:

```python
# Writing to a file
with open('output.txt', 'w') as file:
    file.write("Crop Yield Report\n")
    file.write("Maize: 2.5 tons/ha\n")
```

Always use `with open(...)` — it automatically closes the file safely.

---

#### Exercises

1. Write a function that calculates the cost of production given area and cost per hectare.
2. Create a function that returns the longer of two crop names.
3. Read a file with crop names and print each with `"Crop:"` prefix.
4. Write a function that takes a list of yields and writes them to a file.
5. Extend the above function to also return the average yield.