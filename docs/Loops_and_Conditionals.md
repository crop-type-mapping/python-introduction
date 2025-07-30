Loops and conditionals allow you to control the **flow of logic** in your program.  
They are essential when dealing with datasets, performing repetitive tasks, or applying filters in crop analysis.

---

#### Conditional Statements (`if`, `elif`, `else`)

Use conditionals to execute different blocks of code based on logic.

```python
yield_per_hectare = 2.5

if yield_per_hectare > 3:
    print("High yield")
elif yield_per_hectare > 2:
    print("Moderate yield")
else:
    print("Low yield")
```

**Tips**:

- Use `==` for equality, `!=` for inequality
- Use `and`, `or`, `not` for combining conditions

---

#### `for` Loops

Use a `for` loop to iterate over items in a list or a range of numbers.

```python
crops = ['Maize', 'Beans', 'Sorghum']

for crop in crops:
    print("Processing crop:", crop)

# Loop with range
for i in range(3):
    print("Iteration:", i)
```

---

#### `while` Loops

`while` loops run as long as a condition remains true.

```python
counter = 0

while counter < 3:
    print("Count:", counter)
    counter += 1
```

Be careful with infinite loops — always ensure your loop has an exit condition.

---

#### Loop Control: `break` and `continue`

- `break`: exits the loop entirely
- `continue`: skips to the next iteration

```python
for crop in crops:
    if crop == 'Beans':
        continue  # Skip Beans
    print(crop)

# Breaking the loop
for crop in crops:
    if crop == 'Sorghum':
        break
    print(crop)
```

---

#### Exercises

1. Write a condition to classify yield as `"Low"`, `"Medium"`, or `"High"`.
2. Loop through a list of crops and print each in uppercase.
3. Use a `while` loop to print numbers from 1 to 5.
4. Break the loop when the crop `"Sorghum"` is found.
5. Combine conditionals with a loop to only print crops with yield > 2.0 tons/ha.


---

⬅️ [Previous: Numpy, Pandas](numpy_pandas_cheatsheet.md) | [Next: Functions and File I/O ➡️](Functions_and_File_IO.md)
