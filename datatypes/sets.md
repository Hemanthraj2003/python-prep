# SETS

- Sets are **unordered** and **mutable** collections of **unique** elements in Python.
- Sets automatically eliminate duplicate values.
- Sets are defined using curly braces `{}` and elements are separated by commas `,`.
- Sets are **not indexed** - you cannot access elements by position.
- Default value of set is empty set `set()` (not `{}` which creates an empty dictionary).
- **Key Interview Point**: Sets are primarily used for membership testing, eliminating duplicates, and mathematical operations.

## `CREATING A SET`

We can create a set in multiple ways:

1. **Using curly braces `{}` (set literal)**

```python
# Creating a set with elements
fruits = {"apple", "banana", "cherry"}
print(fruits)  # Output: {'apple', 'banana', 'cherry'}
print(type(fruits))  # Output: <class 'set'>

# Duplicates are automatically removed
numbers = {1, 2, 3, 2, 1, 4}
print(numbers)  # Output: {1, 2, 3, 4}

# Mixed data types (must be immutable/hashable)
mixed_set = {1, "hello", 3.14, (1, 2)}
print(mixed_set)  # Output: {1, 3.14, 'hello', (1, 2)}
```

2. **Using the `set()` constructor**

```python
# Creating an empty set
my_set = set()
print(my_set)  # Output: set()
print(type(my_set))  # Output: <class 'set'>

# Creating a set from a list
fruits = set(["apple", "banana", "cherry", "apple"])
print(fruits)  # Output: {'apple', 'banana', 'cherry'}

# Creating a set from a string
char_set = set("hello")
print(char_set)  # Output: {'h', 'e', 'l', 'o'}

# Creating a set from a tuple
tuple_set = set((1, 2, 3, 2, 1))
print(tuple_set)  # Output: {1, 2, 3}
```

> **Important**: `{}` creates an empty dictionary, not an empty set. Use `set()` for empty sets.

```python
empty_dict = {}
empty_set = set()
print(type(empty_dict))  # Output: <class 'dict'>
print(type(empty_set))   # Output: <class 'set'>
```

## `ADDING ELEMENTS TO A SET`

Sets are mutable, so we can add elements after creation:

1. **Using `add()` method** - adds a single element

```python
fruits = {"apple", "banana"}
fruits.add("cherry")
print(fruits)  # Output: {'apple', 'banana', 'cherry'}

# Adding duplicate has no effect
fruits.add("apple")
print(fruits)  # Output: {'apple', 'banana', 'cherry'}
```

2. **Using `update()` method** - adds multiple elements from an iterable

```python
fruits = {"apple", "banana"}
fruits.update(["cherry", "date", "elderberry"])
print(fruits)  # Output: {'apple', 'banana', 'cherry', 'date', 'elderberry'}

# Can update with multiple iterables
fruits.update(["fig"], ("grape", "kiwi"), {"lemon"})
print(fruits)
# Output: {'apple', 'banana', 'cherry', 'date', 'elderberry', 'fig', 'grape', 'kiwi', 'lemon'}

# Update with string (each character becomes an element)
letters = {"a", "b"}
letters.update("hello")
print(letters)  # Output: {'a', 'b', 'h', 'e', 'l', 'o'}
```

## `REMOVING ELEMENTS FROM A SET`

1. **Using `remove()` method** - removes specified element, raises KeyError if not found

```python
fruits = {"apple", "banana", "cherry"}
fruits.remove("banana")
print(fruits)  # Output: {'apple', 'cherry'}

# This will raise KeyError
# fruits.remove("grape")  # KeyError: 'grape'
```

2. **Using `discard()` method** - removes specified element, no error if not found

```python
fruits = {"apple", "banana", "cherry"}
fruits.discard("banana")
print(fruits)  # Output: {'apple', 'cherry'}

# This won't raise an error
fruits.discard("grape")  # No error, set remains unchanged
print(fruits)  # Output: {'apple', 'cherry'}
```

3. **Using `pop()` method** - removes and returns an arbitrary element

```python
fruits = {"apple", "banana", "cherry"}
removed = fruits.pop()
print(f"Removed: {removed}")  # Output: Removed: apple (or any element)
print(fruits)  # Output: {'banana', 'cherry'} (remaining elements)

# pop() on empty set raises KeyError
# empty_set = set()
# empty_set.pop()  # KeyError: 'pop from an empty set'
```

4. **Using `clear()` method** - removes all elements

```python
fruits = {"apple", "banana", "cherry"}
fruits.clear()
print(fruits)  # Output: set()
```

## `SET OPERATIONS`

**Key Concept**: Sets support mathematical set operations which are essential for data manipulation.

```python
set1 = {1, 2, 3, 4, 5}
set2 = {4, 5, 6, 7, 8}

# Union (|) - all unique elements from both sets
union_result = set1 | set2
print(union_result)  # Output: {1, 2, 3, 4, 5, 6, 7, 8}
# or use union() method
union_result = set1.union(set2)

# Intersection (&) - common elements
intersection_result = set1 & set2
print(intersection_result)  # Output: {4, 5}
# or use intersection() method
intersection_result = set1.intersection(set2)

# Difference (-) - elements in set1 but not in set2
difference_result = set1 - set2
print(difference_result)  # Output: {1, 2, 3}
# or use difference() method
difference_result = set1.difference(set2)

# Symmetric Difference (^) - elements in either set but not in both
sym_diff_result = set1 ^ set2
print(sym_diff_result)  # Output: {1, 2, 3, 6, 7, 8}
# or use symmetric_difference() method
sym_diff_result = set1.symmetric_difference(set2)

```

## `SET COMPARISON OPERATIONS`

```python
set1 = {1, 2, 3}
set2 = {1, 2, 3, 4, 5}
set3 = {1, 2, 3}
set4 = {4, 5, 6}

# Subset check
print(set1.issubset(set2))    # Output: True (set1 ⊆ set2)
print(set1 <= set2)           # Output: True (alternative syntax)

# Proper subset (strict subset)
print(set1 < set2)            # Output: True

# Superset check
print(set2.issuperset(set1))  # Output: True (set2 ⊇ set1)
print(set2 >= set1)           # Output: True (alternative syntax)

# Proper superset (strict superset)
print(set2 > set1)            # Output: True

# Disjoint sets (no common elements)
print(set1.isdisjoint(set4))  # Output: True
print(set1.isdisjoint(set2))  # Output: False

# Equality
print(set1 == set3)           # Output: True
```

## `SET METHODS`

| Method                   | Description                                        | Example                                    |
| ------------------------ | -------------------------------------------------- | ------------------------------------------ |
| `len()`                  | Returns the number of elements in the set          | `length = len(my_set)`                     |
| `add(x)`                 | Adds element `x` to the set                        | `my_set.add("new_item")`                   |
| `update(iterable)`       | Adds multiple elements from an iterable            | `my_set.update([1, 2, 3])`                 |
| `remove(x)`              | Removes element `x`, raises KeyError if not found  | `my_set.remove("item")`                    |
| `discard(x)`             | Removes element `x`, no error if not found         | `my_set.discard("item")`                   |
| `pop()`                  | Removes and returns an arbitrary element           | `item = my_set.pop()`                      |
| `clear()`                | Removes all elements from the set                  | `my_set.clear()`                           |
| `copy()`                 | Returns a shallow copy of the set                  | `new_set = my_set.copy()`                  |
| `union(*others)`         | Returns union of sets                              | `result = set1.union(set2, set3)`          |
| `intersection(*others)`  | Returns intersection of sets                       | `result = set1.intersection(set2)`         |
| `difference(*others)`    | Returns difference of sets                         | `result = set1.difference(set2)`           |
| `symmetric_difference()` | Returns symmetric difference                       | `result = set1.symmetric_difference(set2)` |
| `issubset(other)`        | Tests whether every element is in other            | `set1.issubset(set2)`                      |
| `issuperset(other)`      | Tests whether every element in other is in the set | `set1.issuperset(set2)`                    |
| `isdisjoint(other)`      | Tests whether sets have no elements in common      | `set1.isdisjoint(set2)`                    |

## `MEMBERSHIP TESTING`

**Performance Note**: Sets provide O(1) average time complexity for membership testing, making them very efficient for checking if an element exists.

```python
fruits = {"apple", "banana", "cherry", "date"}

# Membership testing
print("apple" in fruits)      # Output: True
print("grape" not in fruits)  # Output: True

# Comparing with lists (sets are much faster for large datasets)
import time

# Large dataset
large_list = list(range(100000))
large_set = set(range(100000))

# Testing membership in list (O(n) - slower)
start = time.time()
99999 in large_list
list_time = time.time() - start

# Testing membership in set (O(1) - faster)
start = time.time()
99999 in large_set
set_time = time.time() - start

print(f"List time: {list_time:.6f}s")
print(f"Set time: {set_time:.6f}s")
```

## `FROZEN SETS`

**Important Concept**: Frozen sets are immutable versions of sets.

```python
# Creating a frozen set
frozen_fruits = frozenset(["apple", "banana", "cherry"])
print(frozen_fruits)  # Output: frozenset({'apple', 'banana', 'cherry'})
print(type(frozen_fruits))  # Output: <class 'frozenset'>

# Frozen sets are hashable and can be used as dictionary keys or set elements
dict_with_frozenset_key = {
    frozenset([1, 2, 3]): "first set",
    frozenset([4, 5, 6]): "second set"
}

# Set of frozen sets
set_of_frozensets = {
    frozenset([1, 2]),
    frozenset([3, 4]),
    frozenset([1, 2])  # Duplicate, will be ignored
}
print(set_of_frozensets)  # Output: {frozenset({1, 2}), frozenset({3, 4})}

# Frozen sets support all read-only operations
fs1 = frozenset([1, 2, 3])
fs2 = frozenset([2, 3, 4])
print(fs1 | fs2)  # Output: frozenset({1, 2, 3, 4})
print(fs1 & fs2)  # Output: frozenset({2, 3})

# But no modification methods
# fs1.add(4)  # AttributeError: 'frozenset' object has no attribute 'add'
```

## `PRACTICAL APPLICATIONS`

1. **Removing Duplicates**

```python
# Remove duplicates from a list while preserving uniqueness
numbers = [1, 2, 3, 2, 1, 4, 5, 4]
unique_numbers = list(set(numbers))
print(unique_numbers)  # Output: [1, 2, 3, 4, 5] (order may vary)

# To preserve order, use dict.fromkeys() (Python 3.7+)
unique_ordered = list(dict.fromkeys(numbers))
print(unique_ordered)  # Output: [1, 2, 3, 4, 5]
```

2. **Finding Common Elements**

```python
list1 = [1, 2, 3, 4, 5]
list2 = [4, 5, 6, 7, 8]
common = list(set(list1) & set(list2))
print(common)  # Output: [4, 5]
```

3. **Finding Differences**

```python
all_students = {"Alice", "Bob", "Charlie", "David"}
present_students = {"Alice", "Charlie"}
absent_students = all_students - present_students
print(absent_students)  # Output: {'Bob', 'David'}
```

## `COMMON INTERVIEW QUESTIONS`

1. **Q: What's the time complexity of set operations?**

   - **A**:
     - Add/Remove/Membership: O(1) average case
     - Union/Intersection: O(len(s) + len(t))
     - Difference: O(len(s))

2. **Q: Can sets contain mutable objects?**
   - **A**: No, sets can only contain hashable (immutable) objects. Lists, dictionaries, and sets themselves cannot be elements of a set.

```python
# This works - immutable objects
valid_set = {1, "hello", (1, 2), frozenset([3, 4])}

# This doesn't work - mutable objects
# invalid_set = {[1, 2]}  # TypeError: unhashable type: 'list'
# invalid_set = {{1, 2}}  # TypeError: unhashable type: 'set'
```

3. **Q: How do you convert a set to a list?**

   - **A**: Use `list(my_set)`, but note that order is not guaranteed in older Python versions.

4. **Q: What's the difference between remove() and discard()?**
   - **A**: `remove()` raises KeyError if element doesn't exist, `discard()` doesn't raise an error.

## `SET COMPREHENSIONS`

```python
# Basic set comprehension
squares = {x**2 for x in range(10)}
print(squares)  # Output: {0, 1, 4, 9, 16, 25, 36, 49, 64, 81}

# Set comprehension with condition
even_squares = {x**2 for x in range(10) if x % 2 == 0}
print(even_squares)  # Output: {0, 4, 16, 36, 64}

# From string
vowels = {char.lower() for char in "Hello World" if char.lower() in "aeiou"}
print(vowels)  # Output: {'e', 'o'}
```

## `PERFORMANCE TIPS`

- Use sets for membership testing when you need to check if items exist frequently
- Use sets to eliminate duplicates efficiently
- Use set operations instead of loops for mathematical set operations
- Convert to set for operations, then back to list if needed for ordering
