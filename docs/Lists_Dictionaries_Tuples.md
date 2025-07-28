Python provides several built-in data structures to store collections of items.  
For crop mapping and geospatial tasks, the most commonly used ones are:

- `list`: ordered, changeable collection
- `tuple`: ordered, unchangeable collection
- `dict`: key-value mappings

---

#### Lists

Lists are **ordered** and **mutable** collections that can store any type of data.

```python
# Define a list of crops
crops = ['Maize', 'Beans', 'Sorghum']

# Access items
print(crops[0])  # Output: Maize

# Add new item
crops.append('Cassava')

# Check contents
print(crops)
```

**Common List Methods**

- `append()`: Add item to end
- `remove()`: Remove by value
- `sort()`: Sort items
- `len()`: Get number of items

---

#### Tuples

Tuples are like lists, but **immutable** (they cannot be changed after creation).  
Use them to store **fixed** data like coordinates.

```python
# Define a tuple
coordinates = (1.95, 30.06)

# Access values
print("Latitude:", coordinates[0])
```

---

#### Dictionaries

Dictionaries store data in **key-value** pairs.  
They’re ideal for mapping relationships, such as crop name → yield.

```python
# Create a dictionary of crop yields
crop_yields = {
    'Maize': 2.5,
    'Beans': 1.8
}

# Access value by key
print(crop_yields['Maize'])  # Output: 2.5

# Add new entry
crop_yields['Sorghum'] = 2.0

# Display all keys
print(crop_yields.keys())
```

**Common Dictionary Methods**

- `get(key)`: Get value safely
- `keys()`: Return all keys
- `values()`: Return all values
- `items()`: Return key-value pairs

---

#### Exercises

1. Create a list of five crops and print the third crop.
2. Create a tuple representing the lat/lon of a farm location.
3. Create a dictionary with crop names as keys and yield per hectare as values.
4. Add a new crop to the dictionary and print all items.
5. Write a function that takes a crop name and returns the yield from the dictionary.

---

⬅️ [Previous: Variables and Data Types](Variables.md) | [Next: Loops and Conditionals ➡️](Loops_and_Conditionals.md)
