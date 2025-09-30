# DICTIONARIES

- Dictionaries are **ordered** (Python 3.7+), **mutable** collections of key-value pairs in Python.
- Dictionaries store data in `key: value` format where keys must be **unique** and **immutable** (hashable).
- Dictionaries are defined using curly braces `{}` with key-value pairs separated by commas `,`.
- Keys and values are separated by colons `:`.
- Default value of dictionary is empty dictionary `{}`.
- **Key Interview Point**: Dictionaries provide O(1) average time complexity for lookups, insertions, and deletions.

## `IMPORTANT NOTE: {} CONFUSION`

**Critical Point**: `{}` creates an empty dictionary, NOT an empty set!

```python
# This creates an empty DICTIONARY
empty_dict = {}
print(type(empty_dict))  # Output: <class 'dict'>

# To create an empty SET, you must use set()
empty_set = set()
print(type(empty_set))   # Output: <class 'set'>

# With elements, both use curly braces but behave differently
my_dict = {"key": "value"}      # Dictionary (key-value pairs)
my_set = {"apple", "banana"}    # Set (unique elements only)

print(type(my_dict))  # Output: <class 'dict'>
print(type(my_set))   # Output: <class 'set'>
```

**Why this confusion exists:**

- Both dictionaries and sets use curly braces `{}`
- Empty `{}` defaults to dictionary because dictionaries were implemented first in Python
- This is a very common interview question and source of bugs

**Memory tip:** "Empty curly braces = Empty Dictionary"

## `CREATING A DICTIONARY`

We can create a dictionary in multiple ways:

1. **Using curly braces `{}` (dictionary literal)**

```python
# Creating an empty dictionary
my_dict = {}
print(my_dict)  # Output: {}
print(type(my_dict))  # Output: <class 'dict'>

# Creating a dictionary with key-value pairs
student = {"name": "Alice", "age": 25, "grade": "A"}
print(student)  # Output: {'name': 'Alice', 'age': 25, 'grade': 'A'}

# Mixed data types
mixed_dict = {
    "string_key": "value",
    42: "number_key",
    (1, 2): "tuple_key",
    True: "boolean_key"
}
print(mixed_dict)  # Output: {'string_key': 'value', 42: 'number_key', (1, 2): 'tuple_key', True: 'boolean_key'}
```

2. **Using the `dict()` constructor**

```python
# Creating an empty dictionary
my_dict = dict()
print(my_dict)  # Output: {}

# Creating a dictionary from keyword arguments
student = dict(name="Alice", age=25, grade="A")
print(student)  # Output: {'name': 'Alice', 'age': 25, 'grade': 'A'}

# Creating a dictionary from a list of tuples
pairs = [("name", "Bob"), ("age", 30), ("city", "New York")]
student = dict(pairs)
print(student)  # Output: {'name': 'Bob', 'age': 30, 'city': 'New York'}

# Creating a dictionary from two lists using zip
keys = ["name", "age", "city"]
values = ["Charlie", 35, "London"]
student = dict(zip(keys, values))
print(student)  # Output: {'name': 'Charlie', 'age': 35, 'city': 'London'}
```

3. **Using dictionary comprehension**

```python
# Create dictionary from range
squares = {x: x**2 for x in range(5)}
print(squares)  # Output: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Create dictionary with condition
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
print(even_squares)  # Output: {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}
```

## `ACCESSING ELEMENTS IN A DICTIONARY`

1. **Using square brackets `[]`**

```python
student = {"name": "Alice", "age": 25, "grade": "A"}

print(student["name"])  # Output: Alice
print(student["age"])   # Output: 25

# KeyError if key doesn't exist
# print(student["height"])  # KeyError: 'height'
```

2. **Using `get()` method** (safer approach)

```python
student = {"name": "Alice", "age": 25, "grade": "A"}

print(student.get("name"))      # Output: Alice
print(student.get("height"))    # Output: None
print(student.get("height", 0)) # Output: 0 (default value)

# get() with default value is very useful
height = student.get("height", "Not specified")
print(height)  # Output: Not specified
```

## `MODIFYING DICTIONARIES`

1. **Adding/Updating elements**

```python
student = {"name": "Alice", "age": 25}

# Adding new key-value pair
student["grade"] = "A"
print(student)  # Output: {'name': 'Alice', 'age': 25, 'grade': 'A'}

# Updating existing value
student["age"] = 26
print(student)  # Output: {'name': 'Alice', 'age': 26, 'grade': 'A'}

# Using update() method
student.update({"city": "New York", "grade": "A+"})
print(student)  # Output: {'name': 'Alice', 'age': 26, 'grade': 'A+', 'city': 'New York'}

# Update with keyword arguments
student.update(country="USA", phone="123-456-7890")
print(student)
# Output: {'name': 'Alice', 'age': 26, 'grade': 'A+', 'city': 'New York', 'country': 'USA', 'phone': '123-456-7890'}
```

2. **Removing elements**

```python
student = {"name": "Alice", "age": 25, "grade": "A", "city": "New York"}

# Using del statement
del student["city"]
print(student)  # Output: {'name': 'Alice', 'age': 25, 'grade': 'A'}

# Using pop() method - removes and returns value
age = student.pop("age")
print(age)      # Output: 25
print(student)  # Output: {'name': 'Alice', 'grade': 'A'}

# pop() with default value
height = student.pop("height", "Not found")
print(height)   # Output: Not found

# Using popitem() method - removes and returns last inserted key-value pair
item = student.pop("grade")
print(item)     # Output: A
print(student)  # Output: {'name': 'Alice'}

# Using clear() method - removes all elements
student.clear()
print(student)  # Output: {}
```

## `DICTIONARY METHODS`

| Method                     | Description                                                   | Example                                    |
| -------------------------- | ------------------------------------------------------------- | ------------------------------------------ |
| `len()`                    | Returns the number of key-value pairs                         | `length = len(my_dict)`                    |
| `get(key, default)`        | Returns value for key, or default if key doesn't exist        | `value = my_dict.get("key", "default")`    |
| `keys()`                   | Returns a view of dictionary keys                             | `keys = my_dict.keys()`                    |
| `values()`                 | Returns a view of dictionary values                           | `values = my_dict.values()`                |
| `items()`                  | Returns a view of key-value pairs as tuples                   | `items = my_dict.items()`                  |
| `pop(key, default)`        | Removes key and returns its value                             | `value = my_dict.pop("key")`               |
| `popitem()`                | Removes and returns last inserted key-value pair              | `key, value = my_dict.popitem()`           |
| `update(other)`            | Updates dictionary with key-value pairs from other            | `my_dict.update({"new_key": "new_value"})` |
| `clear()`                  | Removes all elements from dictionary                          | `my_dict.clear()`                          |
| `copy()`                   | Returns a shallow copy of dictionary                          | `new_dict = my_dict.copy()`                |
| `setdefault(key, default)` | Returns value of key, inserts key with default if not present | `value = my_dict.setdefault("key", [])`    |
| `fromkeys(keys, value)`    | Creates dictionary from keys with same value                  | `dict.fromkeys(["a", "b"], 0)`             |

```python
student = {"name": "Alice", "age": 25, "grade": "A"}

# keys(), values(), items()
print(student.keys())    # Output: dict_keys(['name', 'age', 'grade'])
print(student.values())  # Output: dict_values(['Alice', 25, 'A'])
print(student.items())   # Output: dict_items([('name', 'Alice'), ('age', 25), ('grade', 'A')])

# Convert to lists if needed
keys_list = list(student.keys())
values_list = list(student.values())
items_list = list(student.items())

# setdefault() - very useful for complex data structures
data = {}
data.setdefault("users", []).append("Alice")
data.setdefault("users", []).append("Bob")
print(data)  # Output: {'users': ['Alice', 'Bob']}

# fromkeys() - class method
default_dict = dict.fromkeys(["a", "b", "c"], 0)
print(default_dict)  # Output: {'a': 0, 'b': 0, 'c': 0}
```

## `ITERATING THROUGH DICTIONARIES`

**Key Point**: Understanding different ways to iterate through dictionaries is crucial for efficient programming.

```python
student = {"name": "Alice", "age": 25, "grade": "A"}

# Iterate through keys (default behavior)
for key in student:
    print(f"{key}: {student[key]}")

# Iterate through keys explicitly
for key in student.keys():
    print(key)

# Iterate through values
for value in student.values():
    print(value)

# Iterate through key-value pairs (most common)
for key, value in student.items():
    print(f"{key}: {value}")

# Using enumerate with items()
for index, (key, value) in enumerate(student.items()):
    print(f"{index}: {key} = {value}")
```

## `NESTED DICTIONARIES`

```python
# Creating nested dictionaries
students = {
    "student1": {"name": "Alice", "age": 25, "grades": [85, 90, 92]},
    "student2": {"name": "Bob", "age": 23, "grades": [78, 85, 88]},
    "student3": {"name": "Charlie", "age": 24, "grades": [92, 95, 97]}
}

# Accessing nested values
print(students["student1"]["name"])        # Output: Alice
print(students["student2"]["grades"][1])   # Output: 85

# Adding to nested dictionary
students["student1"]["city"] = "New York"

# Safely accessing nested values
name = students.get("student4", {}).get("name", "Unknown")
print(name)  # Output: Unknown

# Iterating through nested dictionary
for student_id, info in students.items():
    print(f"{student_id}:")
    for key, value in info.items():
        print(f"  {key}: {value}")
```

## `DICTIONARY COMPREHENSIONS`

**Important**: Dictionary comprehensions are powerful and provide concise ways to create dictionaries.

```python
# Basic dictionary comprehension
squares = {x: x**2 for x in range(5)}
print(squares)  # Output: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# With condition
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
print(even_squares)  # Output: {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# From existing dictionary
original = {"a": 1, "b": 2, "c": 3}
doubled = {k: v*2 for k, v in original.items()}
print(doubled)  # Output: {'a': 2, 'b': 4, 'c': 6}

# Swapping keys and values
swapped = {v: k for k, v in original.items()}
print(swapped)  # Output: {1: 'a', 2: 'b', 3: 'c'}

# From two lists
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
name_age = {name: age for name, age in zip(names, ages)}
print(name_age)  # Output: {'Alice': 25, 'Bob': 30, 'Charlie': 35}

# Conditional logic
grades = {"Alice": 85, "Bob": 92, "Charlie": 78, "David": 95}
pass_fail = {name: "Pass" if grade >= 80 else "Fail" for name, grade in grades.items()}
print(pass_fail)  # Output: {'Alice': 'Pass', 'Bob': 'Pass', 'Charlie': 'Fail', 'David': 'Pass'}
```

## `DICTIONARY AS COUNTERS`

```python
# Counting characters in a string
text = "hello world"
char_count = {}
for char in text:
    char_count[char] = char_count.get(char, 0) + 1
print(char_count)  # Output: {'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'w': 1, 'r': 1, 'd': 1}

# Using setdefault for counting
char_count = {}
for char in text:
    char_count.setdefault(char, 0)
    char_count[char] += 1

# Using Counter from collections (better approach)
from collections import Counter
char_count = Counter(text)
print(char_count)  # Output: Counter({'l': 3, 'o': 2, 'h': 1, 'e': 1, ' ': 1, 'w': 1, 'r': 1, 'd': 1})
```

## `DICTIONARY OPERATIONS`

```python
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
dict3 = {"b": 5, "e": 6}

# Merging dictionaries (Python 3.9+)
merged = dict1 | dict2
print(merged)  # Output: {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# Merging with update (in-place)
dict1.update(dict2)
print(dict1)  # Output: {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# Merging with ** operator (Python 3.5+)
merged = {**dict1, **dict3}
print(merged)  # Output: {'a': 1, 'b': 5, 'c': 3, 'd': 4, 'e': 6}

# Membership testing (checks keys only)
print("a" in dict1)      # Output: True
print("z" not in dict1)  # Output: True

# Checking if value exists (slower)
print(1 in dict1.values())  # Output: True
```

## `COMMON INTERVIEW QUESTIONS`

1. **Q: What's the time complexity of dictionary operations?**

   - **A**:
     - Access/Insert/Delete: O(1) average case, O(n) worst case
     - Iteration: O(n)

2. **Q: What can be used as dictionary keys?**
   - **A**: Only immutable (hashable) objects: strings, numbers, tuples (with immutable elements), frozensets.

```python
# Valid keys
valid_dict = {
    "string": 1,
    42: 2,
    (1, 2): 3,
    frozenset([1, 2]): 4
}

# Invalid keys (will raise TypeError)
# invalid_dict = {[1, 2]: "list"}     # TypeError: unhashable type: 'list'
# invalid_dict = {{1, 2}: "set"}      # TypeError: unhashable type: 'set'
# invalid_dict = {{"a": 1}: "dict"}   # TypeError: unhashable type: 'dict'
```

3. **Q: How do you handle missing keys safely?**
   - **A**: Use `get()` method, `setdefault()`, or `defaultdict` from collections.

```python
from collections import defaultdict

# Using get()
safe_value = my_dict.get("key", "default")

# Using setdefault()
my_dict.setdefault("key", []).append("value")

# Using defaultdict
dd = defaultdict(list)
dd["key"].append("value")  # No KeyError
```

4. **Q: What's the difference between `pop()` and `del`?**

   - **A**: `pop()` returns the value and can have a default, `del` just removes and returns nothing.

5. **Q: How are dictionaries ordered?**
   - **A**: Since Python 3.7, dictionaries maintain insertion order. Before 3.7, they were unordered.

## `ADVANCED DICTIONARY TECHNIQUES`

1. **Default dictionaries**

```python
from collections import defaultdict

# Regular dictionary approach
regular_dict = {}
for item in ["apple", "banana", "apple", "cherry", "banana"]:
    if item in regular_dict:
        regular_dict[item] += 1
    else:
        regular_dict[item] = 1

# defaultdict approach (cleaner)
dd = defaultdict(int)
for item in ["apple", "banana", "apple", "cherry", "banana"]:
    dd[item] += 1
print(dict(dd))  # Output: {'apple': 2, 'banana': 2, 'cherry': 1}
```

2. **Ordered dictionaries** (mostly obsolete since Python 3.7)

```python
from collections import OrderedDict

# Before Python 3.7, regular dicts didn't maintain order
ordered_dict = OrderedDict()
ordered_dict["first"] = 1
ordered_dict["second"] = 2
ordered_dict["third"] = 3

# Move to end
ordered_dict.move_to_end("first")
print(ordered_dict)  # Output: OrderedDict([('second', 2), ('third', 3), ('first', 1)])
```

## `PERFORMANCE TIPS`

- Use dictionaries for O(1) lookups instead of lists when you need to search frequently
- Use `get()` instead of checking `if key in dict` then accessing
- Use dictionary comprehensions for creating dictionaries from other iterables
- Consider `defaultdict` for complex nested structures
- Use `collections.Counter` for counting operations
