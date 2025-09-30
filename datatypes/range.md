# RANGE

- Range is an **immutable** sequence type in Python that represents a sequence of numbers.
- Range objects are **memory efficient** - they don't store all values in memory, they generate them on-the-fly.
- Range is commonly used in `for` loops and when you need a sequence of numbers.
- Range objects support indexing, slicing, and iteration like other sequences.
- **Key Concept**: Range uses lazy evaluation - values are generated only when needed.

## `CREATING A RANGE`

Range can be created using the `range()` function with different parameters:

### 1. **Single parameter (stop)**

```python
# range(stop) - from 0 to stop-1
numbers = range(5)
print(list(numbers))  # Output: [0, 1, 2, 3, 4]
print(type(numbers))  # Output: <class 'range'>
```

### 2. **Two parameters (start, stop)**

```python
# range(start, stop) - from start to stop-1
numbers = range(2, 8)
print(list(numbers))  # Output: [2, 3, 4, 5, 6, 7]
```

### 3. **Three parameters (start, stop, step)**

```python
# range(start, stop, step)
even_numbers = range(0, 10, 2)
print(list(even_numbers))  # Output: [0, 2, 4, 6, 8]

# Negative step for reverse order
reverse_numbers = range(10, 0, -1)
print(list(reverse_numbers))  # Output: [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

## `RANGE PROPERTIES`

### **Immutable**

```python
numbers = range(5)
# numbers[0] = 10  # TypeError: 'range' object does not support item assignment
```

### **Memory Efficient**

```python
# These both use the same amount of memory
small_range = range(10)
large_range = range(1000000)

# Range doesn't store all values, it calculates them when needed
print(small_range.__sizeof__())  # Small memory footprint
print(large_range.__sizeof__())  # Same small memory footprint
```

## `RANGE OPERATIONS`

### **Indexing**

```python
numbers = range(10, 20)
print(numbers[0])   # Output: 10
print(numbers[5])   # Output: 15
print(numbers[-1])  # Output: 19
```

### **Slicing**

```python
numbers = range(0, 10)
print(list(numbers[2:7]))    # Output: [2, 3, 4, 5, 6]
print(list(numbers[::2]))    # Output: [0, 2, 4, 6, 8]
print(list(numbers[::-1]))   # Output: [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

### **Length and Membership**

```python
numbers = range(5, 15)
print(len(numbers))      # Output: 10
print(8 in numbers)      # Output: True
print(20 in numbers)     # Output: False
```

## `COMMON USE CASES`

### **For Loops**

```python
# Most common use case
for i in range(5):
    print(f"Iteration {i}")

# With start and step
for i in range(1, 10, 2):
    print(f"Odd number: {i}")

# Reverse iteration
for i in range(10, 0, -1):
    print(f"Countdown: {i}")
```

### **List Generation**

```python
# Create lists with specific patterns
squares = [x**2 for x in range(1, 6)]
print(squares)  # Output: [1, 4, 9, 16, 25]

# Create list of even numbers
evens = list(range(0, 20, 2))
print(evens)  # Output: [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

### **Array Indexing**

```python
data = ['a', 'b', 'c', 'd', 'e']

# Access elements by index
for i in range(len(data)):
    print(f"Index {i}: {data[i]}")

# Access with both index and value
for i in range(len(data)):
    print(f"{i} -> {data[i]}")
```

## `RANGE VS LIST`

### **Memory Comparison**

```python
import sys

# Range object
r = range(1000000)
print(f"Range memory: {sys.getsizeof(r)} bytes")

# List object
l = list(range(1000000))
print(f"List memory: {sys.getsizeof(l)} bytes")

# Range uses constant memory regardless of size
# List memory grows with number of elements
```

### **Performance Comparison**

```python
import time

# Range iteration (faster)
start = time.time()
for i in range(1000000):
    pass
range_time = time.time() - start

# List iteration (slower for large sequences)
start = time.time()
numbers = list(range(1000000))
for i in numbers:
    pass
list_time = time.time() - start

print(f"Range iteration: {range_time:.4f} seconds")
print(f"List iteration: {list_time:.4f} seconds")
```

## `PRACTICAL EXAMPLES`

### **Example 1: Multiplication Table**

```python
def multiplication_table(n):
    """Generate multiplication table for number n"""
    for i in range(1, 11):
        print(f"{n} x {i} = {n * i}")

multiplication_table(7)
```

### **Example 2: Password Generator Positions**

```python
import random
import string

def generate_password(length):
    """Generate random password of given length"""
    chars = string.ascii_letters + string.digits + "!@#$%"
    password = ""

    for i in range(length):
        password += random.choice(chars)

    return password

print(generate_password(12))
```

### **Example 3: Matrix Operations**

```python
def create_matrix(rows, cols, default_value=0):
    """Create a matrix using range"""
    matrix = []
    for i in range(rows):
        row = []
        for j in range(cols):
            row.append(default_value)
        matrix.append(row)
    return matrix

matrix = create_matrix(3, 4, 0)
print(matrix)
# Output: [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

## `ADVANCED RANGE TECHNIQUES`

### **Range with Enumerate**

```python
colors = ['red', 'green', 'blue', 'yellow']

# Using range
for i in range(len(colors)):
    print(f"{i}: {colors[i]}")

# Better approach with enumerate
for i, color in enumerate(colors):
    print(f"{i}: {color}")
```

### **Range in List Comprehensions**

```python
# Generate specific patterns
fibonacci_like = [i + (i-1) if i > 0 else 0 for i in range(10)]
print(fibonacci_like)

# Create coordinate pairs
coordinates = [(x, y) for x in range(3) for y in range(3)]
print(coordinates)
```

### **Range with Zip**

```python
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 35]

# Using range to create numbered list
for i in range(len(names)):
    print(f"{i+1}. {names[i]} is {ages[i]} years old")

# Better approach with enumerate
for i, (name, age) in enumerate(zip(names, ages), 1):
    print(f"{i}. {name} is {age} years old")
```

## `IMPORTANT CONCEPTS`

### **Empty Range**

```python
# These create empty ranges
empty1 = range(0)
empty2 = range(5, 5)
empty3 = range(10, 0)  # No step, so step=1, 10 to 0 impossible

print(list(empty1))  # Output: []
print(list(empty2))  # Output: []
print(list(empty3))  # Output: []
```

### **Range Equality**

```python
r1 = range(5)
r2 = range(0, 5)
r3 = range(0, 5, 1)

print(r1 == r2 == r3)  # Output: True
print(list(r1))        # Output: [0, 1, 2, 3, 4]
```

### **Range Conversion**

```python
# Convert range to other data types
r = range(1, 6)

# To list
print(list(r))      # Output: [1, 2, 3, 4, 5]

# To tuple
print(tuple(r))     # Output: (1, 2, 3, 4, 5)

# To set
print(set(r))       # Output: {1, 2, 3, 4, 5}
```

## `KEY DIFFERENCES FROM OTHER SEQUENCES`

| Feature        | Range         | List                       | Tuple                      |
| -------------- | ------------- | -------------------------- | -------------------------- |
| Mutability     | Immutable     | Mutable                    | Immutable                  |
| Memory Usage   | Constant      | Grows with size            | Grows with size            |
| Creation Speed | Fast          | Slower for large sequences | Slower for large sequences |
| Indexing       | Supported     | Supported                  | Supported                  |
| Slicing        | Supported     | Supported                  | Supported                  |
| Storage        | Formula-based | Value-based                | Value-based                |

## `INTERVIEW INSIGHTS`

### **Key Concept: Why Range is Memory Efficient**

```python
# Range stores only start, stop, step values
# It calculates values on-demand using the formula:
# value = start + (index * step)

r = range(0, 1000000, 2)
# This doesn't store 500,000 numbers in memory
# It stores: start=0, stop=1000000, step=2
# When you access r[100], it calculates: 0 + (100 * 2) = 200
```

### **Important Note: Range vs Xrange (Python 2 vs 3)**

```python
# Python 2 had both range() and xrange()
# Python 3 range() = Python 2 xrange() (memory efficient)
# Python 3 doesn't have xrange()

# In Python 3, range() is always memory efficient
# No need to worry about memory issues with large ranges
```

### **Performance Tip: When to Convert Range to List**

```python
# Keep as range if you're just iterating
for i in range(1000000):
    # Process i
    pass

# Convert to list only if you need:
# 1. Multiple iterations
# 2. Random access frequently
# 3. List-specific methods (append, remove, etc.)
numbers = list(range(100))  # Only when necessary
```

Range is a fundamental Python data type that provides an efficient way to work with sequences of numbers, especially in loops and iterations. Its memory efficiency and lazy evaluation make it perfect for working with large sequences of numbers without consuming excessive memory.
