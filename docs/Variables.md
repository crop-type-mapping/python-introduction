In Python, **variables** are used to store data that your program can access and manipulate.  
Each variable has a **name** and holds a **value**, which can be of different **data types** such as:

- Integers (e.g., `5`)
- Floating-point numbers (e.g., `3.14`)
- Strings (e.g., `'Maize'`)
- Booleans (e.g., `True`, `False`)
- Collections (e.g., lists, dictionaries, tuples)

These data types are crucial in crop mapping tasks, where you handle numeric values (yield, area), names of crops, and logic checks.

---

#### Declaring Variables

Variables are created by **assigning a value** using the `=` operator.

```python
# Assigning values to variables
crop = 'Maize'           # String
area = 5                 # Integer
yield_per_hectare = 2.8  # Float
is_irrigated = True      # Boolean
```

---

#### Data Types in Practice

Python supports several built-in data types. Here are the most common ones you'll encounter in geospatial or agricultural analysis:

| Type    | Description                    | Example               |
| ------- | ------------------------------ | --------------------- |
| `int`   | Integer numbers                | `5`, `-10`, `2024`    |
| `float` | Decimal numbers                | `2.5`, `0.01`         |
| `str`   | Text or characters             | `'Maize'`, `'Plot A'` |
| `bool`  | Logical true/false             | `True`, `False`       |
| `list`  | Ordered, changeable collection | `['Maize', 'Beans']`  |
| `dict`  | Key-value pairs                | `{'Maize': 2.5}`      |

---

#### Basic Operations

You can perform arithmetic or logical operations with variables.

```python
# Arithmetic
total_yield = area * yield_per_hectare
print("Total Yield (tons):", total_yield)

# String concatenation
crop_info = "Crop: " + crop
print(crop_info)

# Boolean logic
if is_irrigated:
    print("Field is irrigated")
else:
    print("Field is rainfed")
```

---

#### Type Conversion

Sometimes, you need to **convert values** from one type to another using functions like `int()`, `float()`, `str()`, or `bool()`.

```python
value = "25"
value_int = int(value)  # Convert string to integer
print(value_int + 5)
```

---

#### Exercises

Try these on your own or in a Jupyter Notebook:

1. Declare variables for a crop name, area of land (in hectares), and yield per hectare.
2. Compute and print the total expected yield.
3. Convert a number given as a string (e.g., `'10'`) into an integer and multiply it by 2.
4. Write a condition that prints "High Yield" if yield > 3, "Moderate" if between 2 and 3, else "Low".