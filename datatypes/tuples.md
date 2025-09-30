# TUPLES

- Tuples are sequential and **immutable** data structures in Python.
- Tuples can hold heterogeneous data types and allow duplicate values.
- Tuples are defined using parentheses `()` and elements are separated by commas `,`.
- Tuples are `zero` indexed.
- Default value of tuple is empty tuple `()`.
- **Key Interview Point**: Tuples are immutable, meaning you cannot change their elements after creation.

## `CREATING A TUPLE`

We can create a tuple in multiple ways:

1. **Using parentheses `()` (tuple literal)**

```python
# Creating an empty tuple
my_tuple = ()
print(my_tuple)  # Output: ()
print(type(my_tuple))  # Output: <class 'tuple'>

# Creating a tuple with elements
fruits = ("apple", "banana", "cherry")
print(fruits)  # Output: ('apple', 'banana', 'cherry')

# Single element tuple (comma is required!)
single_tuple = ("apple",)  # Note the comma
print(single_tuple)  # Output: ('apple',)
print(type(single_tuple))  # Output: <class 'tuple'>

# Without comma, it's just a string in parentheses
not_tuple = ("apple")
print(type(not_tuple))  # Output: <class 'str'>
```

2. **Using the `tuple()` constructor**

```python
# Creating an empty tuple
my_tuple = tuple()
print(my_tuple)  # Output: ()

# Creating a tuple from a list
fruits = tuple(["apple", "banana", "cherry"])
print(fruits)  # Output: ('apple', 'banana', 'cherry')

# Creating a tuple from a string
char_tuple = tuple("hello")
print(char_tuple)  # Output: ('h', 'e', 'l', 'l', 'o')
```

3. **Tuple packing (without parentheses)**

```python
# Tuple packing - Python automatically creates a tuple
coordinates = 10, 20, 30
print(coordinates)  # Output: (10, 20, 30)
print(type(coordinates))  # Output: <class 'tuple'>
```

## `ACCESSING ELEMENTS IN A TUPLE`

We can access elements in a tuple using `indexing` and `slicing` (same as lists).

- **Indexing**
  - Access elements using their index.
  - Indexing starts from `0`.
  - Supports negative indexing, where `-1` refers to the last element.

```python
fruits = ("apple", "banana", "cherry", "date")
print(fruits[0])   # Output: apple (first element)
print(fruits[2])   # Output: cherry (third element)
print(fruits[-1])  # Output: date (last element)
print(fruits[-2])  # Output: cherry (second last element)
```

- **Slicing**
  - Access a range of elements in a tuple.
  - Slicing syntax: `tuple[start:end:step]`

```python
fruits = ("apple", "banana", "cherry", "date", "elderberry")

print(fruits[1:4])
# Output: ('banana', 'cherry', 'date')

print(fruits[:3])
# Output: ('apple', 'banana', 'cherry')

print(fruits[2:])
# Output: ('cherry', 'date', 'elderberry')

print(fruits[::2])
# Output: ('apple', 'cherry', 'elderberry')

print(fruits[::-1])
# Output: ('elderberry', 'date', 'cherry', 'banana', 'apple')
```

## `TUPLE UNPACKING`

**Key Concept**: Tuple unpacking is a powerful feature that allows you to assign tuple values to multiple variables.

```python
# Basic unpacking
coordinates = (10, 20, 30)
x, y, z = coordinates
print(f"x={x}, y={y}, z={z}")  # Output: x=10, y=20, z=30

# Swapping variables using tuple unpacking
a, b = 5, 10
a, b = b, a  # Swap values
print(f"a={a}, b={b}")  # Output: a=10, b=5

# Using asterisk (*) for extended unpacking
numbers = (1, 2, 3, 4, 5)
first, *middle, last = numbers
print(f"first={first}")    # Output: first=1
print(f"middle={middle}")  # Output: middle=[2, 3, 4]
print(f"last={last}")      # Output: last=5

# Function returning multiple values (actually returns a tuple)
def get_name_age():
    return "Alice", 25

name, age = get_name_age()
print(f"Name: {name}, Age: {age}")  # Output: Name: Alice, Age: 25
```

## `TUPLE METHODS`

Since tuples are immutable, they have very limited methods compared to lists. Tuples only have **2 specific methods**:

| Method     | Description                                          | Example                  |
| ---------- | ---------------------------------------------------- | ------------------------ |
| `count(x)` | Returns the number of times `x` appears in the tuple | `fruits.count("apple")`  |
| `index(x)` | Returns the index of the first occurrence of `x`     | `fruits.index("cherry")` |

**Important Note**: Built-in functions like `len()`, `max()`, `min()`, `sum()` work with tuples but are **not tuple methods** - they are general built-in functions that work with any iterable:

| Built-in Function | Description                                   | Example                  |
| ----------------- | --------------------------------------------- | ------------------------ |
| `len()`           | Returns the number of items in the tuple      | `length = len(fruits)`   |
| `max()`           | Returns the largest item in the tuple         | `maximum = max(numbers)` |
| `min()`           | Returns the smallest item in the tuple        | `minimum = min(numbers)` |
| `sum()`           | Returns the sum of numeric items in the tuple | `total = sum(numbers)`   |

```python
fruits = ("apple", "banana", "cherry", "banana", "date")
numbers = (1, 5, 3, 9, 2, 7)

# Tuple-specific methods
print(fruits.count("banana"))  # Output: 2
print(fruits.index("cherry"))  # Output: 2
print(fruits.index("banana", 2))  # Output: 3 (searches from index 2)

# Built-in functions (work with any iterable)
print(len(fruits))    # Output: 5
print(max(numbers))   # Output: 9
print(min(numbers))   # Output: 1
print(sum(numbers))   # Output: 27

# These DON'T work with tuples (they're list methods):
# fruits.append("grape")    # AttributeError
# fruits.remove("apple")    # AttributeError
# fruits.sort()             # AttributeError
```

**Note**: Unlike lists, tuples don't inherit or share methods from lists. The immutable nature of tuples means they cannot have methods that modify their content.

## `TUPLE OPERATIONS`

```python
# Concatenation with +
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)
combined = tuple1 + tuple2
print(combined)  # Output: (1, 2, 3, 4, 5, 6)

# Repetition with *
repeated = tuple1 * 3
print(repeated)  # Output: (1, 2, 3, 1, 2, 3, 1, 2, 3)

# Membership testing
print(2 in tuple1)      # Output: True
print(7 not in tuple1)  # Output: True

# Comparison
tuple_a = (1, 2, 3)
tuple_b = (1, 2, 4)
print(tuple_a < tuple_b)  # Output: True (lexicographic comparison)
```

## `NESTED TUPLES`

```python
# Creating nested tuples
nested_tuple = ((1, 2), (3, 4), (5, 6))
print(nested_tuple)  # Output: ((1, 2), (3, 4), (5, 6))

# Accessing nested elements
print(nested_tuple[0])     # Output: (1, 2)
print(nested_tuple[1][0])  # Output: 3

# Mixed data types
mixed_tuple = ("Alice", 25, (180, 70), ["Python", "Java"])
print(mixed_tuple[2][0])   # Output: 180 (height)
print(mixed_tuple[3][1])   # Output: Java
```

## `CONVERTING BETWEEN TUPLES AND LISTS`

```python
# Tuple to List
my_tuple = (1, 2, 3, 4)
my_list = list(my_tuple)
print(my_list)  # Output: [1, 2, 3, 4]

# List to Tuple
my_list = [1, 2, 3, 4]
my_tuple = tuple(my_list)
print(my_tuple)  # Output: (1, 2, 3, 4)

# Modifying tuple indirectly (creating a new tuple)
original = (1, 2, 3)
modified = original + (4, 5)
print(modified)  # Output: (1, 2, 3, 4, 5)
```

## `WHEN TO USE TUPLES`

**Key Use Cases**:

1. **Immutable data**: When you need data that shouldn't change (coordinates, RGB values, database records).
2. **Dictionary keys**: Tuples can be used as dictionary keys (if they contain only immutable elements).
3. **Function returns**: Return multiple values from functions.
4. **Performance**: Tuples are slightly faster than lists for iteration.
5. **Hashable**: Tuples are hashable (can be used in sets and as dict keys).

```python
# Example: Using tuples as dictionary keys
locations = {
    (0, 0): "Origin",
    (1, 2): "Point A",
    (3, 4): "Point B"
}
print(locations[(1, 2)])  # Output: Point A

# Example: Tuple in a set
coordinate_set = {(0, 0), (1, 1), (2, 2)}
print(coordinate_set)  # Output: {(0, 0), (1, 1), (2, 2)}
```

## `COMMON INTERVIEW QUESTIONS`

1. **Q: What's the difference between lists and tuples?**

   - **A**: Lists are mutable, tuples are immutable. Tuples are hashable and can be used as dict keys.

2. **Q: How do you create a single-element tuple?**

   - **A**: Use a trailing comma: `(element,)` or `element,`

3. **Q: Can you modify a tuple?**
   - **A**: No, tuples are immutable. But if a tuple contains mutable objects (like lists), those objects can be modified.

```python
# Tuple with mutable elements
mixed_tuple = (1, 2, [3, 4])
mixed_tuple[2].append(5)  # This works!
print(mixed_tuple)  # Output: (1, 2, [3, 4, 5])

# But you can't reassign the tuple element itself
# mixed_tuple[2] = [6, 7]  # This would raise TypeError
```

4. **Q: Are tuples faster than lists?**
   - **A**: Yes, for iteration and access operations, because they're immutable and have less overhead.

## `TUPLE COMPREHENSIONS`

**Note**: There's no such thing as tuple comprehensions in Python. Using `(x for x in iterable)` creates a generator, not a tuple.

```python
# This creates a generator, not a tuple
gen = (x**2 for x in range(5))
print(type(gen))  # Output: <class 'generator'>

# To create a tuple from comprehension, use tuple()
squares_tuple = tuple(x**2 for x in range(5))
print(squares_tuple)  # Output: (0, 1, 4, 9, 16)
print(type(squares_tuple))  # Output: <class 'tuple'>
```
