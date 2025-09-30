# LISTS

- Lists are sequencial and mutable data structure in python.
- Lists can hold heterogeneous data types and allows duplicate values.
- Lists are defined using square brackets `[]` and elements are separated by commas `,`.
- Lists are `zero` indexed.
- Default values of list is empty list `[]`.

## `CREATING A LIST`

We can create a list in two ways:

1. **Using square brackets `[]` ( array literal )**

```python
# Creating an empty list
my_list = []
print(my_list)  # Output: []
print(type(my_list))  # Output: <class 'list'>

# Creating a list with elements
fruits = ["apple", "banana", "cherry"]
print(fruits)  # Output: ['apple', 'banana', 'cherry']
```

2. **Using the `list()` constructor**

```python
# Creating an empty list
my_list = list()
print(my_list)  # Output: []
print(type(my_list))  # Output: <class 'list'>

# Creating a list with elements
fruits = list(("apple", "banana", "cherry"))
print(fruits)  # Output: ['apple', 'banana', 'cherry']
```

List constructor can take other datatypes and convert them to a list. [more on list() constructor](../basics/type-casting.md)

## `ACCESSING ELEMENTS IN A LIST`

We can access elements in a list using `indexing` and `slicing`.

- **Indexing**
  - lets us to access elements using their index.
  - Indexing starts from `0`.
  - supports negative indexing, where `-1` refers to the last element, `-2` to the second last element, and so on.

```python
fruits = ["apple", "banana", "cherry", "date"]
print(fruits[0])  # Output: apple (first element)
print(fruits[2])  # Output: cherry (third element)
print(fruits[-1])  # Output: date (last element)
print(fruits[-3])  # Output: banana (third last element)
```

- **Slicing**
  - lets us to access a range of elements in a list.
  - Slicing syntax: `list[start:end:step]`
    - `start`: starting index (inclusive), default is `0`
    - `end`: ending index (exclusive), default is `len(list)`
    - `step`: step size (default is `1`)
  - we can also use negative indices in slicing.

```python
fruits = ["apple", "banana", "cherry", "date", "elderberry"]

print(fruits[1:4])
# Output: ['banana', 'cherry', 'date'] (from index 1 to 3)

print(fruits[:3])
# Output: ['apple', 'banana', 'cherry'] (from start to index 2)

print(fruits[2:])
# Output: ['cherry', 'date', 'elderberry'] (from index 2 to end)

print(fruits[::2])
# Output: ['apple', 'cherry', 'elderberry'] (every second element)

print(fruits[-4:-1])
# Output: ['banana', 'cherry', 'date'] (from index -4 to -2)
print(fruits[::-1])
# Output: ['elderberry', 'date', 'cherry', 'banana', 'apple'] (reversed list)
```

## `CHANGING ELEMENTS IN A LIST`

We can change elements in a list by assigning new values to specific indices or slices.

```python
fruits = ["apple", "banana", "cherry"]
print(fruits)  # Output: ['apple', 'banana', 'cherry']

# Changing a single element
fruits[1] = "blueberry"
print(fruits)  # Output: ['apple', 'blueberry', 'cherry']

# Changing a slice of elements
fruits[0:2] = ["avocado", "blackberry"]
print(fruits)  # Output: ['avocado', 'blackberry', 'cherry']

# Changing a slice with different number of elements
fruits[1:3] = ["date", "elderberry", "fig"]
print(fruits)
# Output: ['avocado', 'date', 'elderberry', 'fig']
```

> `Note:` we cannot add new elements by assigning values to an index that is out of range (`IndexError`), but we can change the size of the list by assigning a new list of different size to a slice.

## `ADDING ELEMENTS TO A LIST`

We can add elements to a list using the following methods:

1. **Using `append()` method**
   adds a single element to the end of the list.

   ```python
   fruits = ["apple", "banana"]
   fruits.append("cherry")
   print(fruits)  # Output: ['apple', 'banana', 'cherry']
   ```

2. **Using `insert()` method**\
   adds a single element at a specific index.

   ```python
   fruits = ["apple", "banana"]
   fruits.insert(1, "cherry")
   print(fruits)  # Output: ['apple', 'cherry', 'banana']
   ```

3. **Using `extend()` method**\
   adds multiple elements from an iterable (like another list, tuple, or string) to the end of the list.

   ```python
   fruits = ["apple", "banana"]
   fruits.extend(["cherry", "date"])
   print(fruits)  # Output: ['apple', 'banana', 'cherry', 'date']

   fruits.extend("elderberry")
   print(fruits)
   # Output: ['apple', 'banana', 'cherry', 'date', 'e', 'l', 'd', 'e', 'r', 'b', 'e', 'r', 'r', 'y']
   ```

4. **Using `+` operator**\
   concatenates two lists and returns a new list.

   ```python
   fruits = ["apple", "banana"]
   new_fruits = fruits + ["cherry", "date"]
   print(new_fruits)
   # Output: ['apple', 'banana', 'cherry', 'date']
   ```

5. **Using `*` operator**\
   repeats the list a specified number of times and returns a new list.

   ```python
   fruits = ["apple", "banana"]
   new_fruits = fruits * 2
   print(new_fruits)
   # Output: ['apple', 'banana', 'apple', 'banana']
   ```

## `REMOVING ELEMENTS FROM A LIST`

We can remove elements from a list using the following methods:

1. **Using `remove()` method**\
   removes the first occurance of a specific value.

   ```python
    fruits = ["apple", "banana", "cherry", "banana"]
    fruits.remove("banana")
    print(fruits)  # Output: ['apple', 'cherry', 'banana']
   ```

2. **Using `pop()` method**\
   removes and returns an element at a specific index (default is the last element).

   ```python
   fruits = ["apple", "banana", "cherry"]
   removed_fruit = fruits.pop()
   print(fruits)        # Output: ['apple', 'banana']
   print(removed_fruit) # Output: 'cherry'

   removed_fruit = fruits.pop(1)
   print(fruits)        # Output: ['apple', 'cherry']
   print(removed_fruit) # Output: 'banana'
   ```

3. **Using `del` statement**\
   removes an element at a specific index or a slice of elements.

   ```python
   fruits = ["apple", "banana", "cherry"]
   del fruits[1]
   print(fruits)  # Output: ['apple', 'cherry']

   del fruits[0:2]
   print(fruits)  # Output: []
   ```

   > `Note:` `del` can also be used to delete the entire list.

4. **Using `clear()` method**\
   removes all elements from the list.

   ```python
   fruits = ["apple", "banana", "cherry"]
   fruits.clear()
   print(fruits)  # Output: []
   ```

## `LIST METHODS`

Here are some commonly used list methods in Python:

| Method                                                                                    | Description                                                             | Example                                        |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ---------------------------------------------- |
| `len()`                                                                                   | Returns the number of items in the list                                 | `length = len(fruits)`                         |
| `append(x)`                                                                               | Adds an item `x` to the end of the list                                 | `fruits.append("date")`                        |
| `extend(iterable)`                                                                        | Extends the list by appending elements from the iterable                | `fruits.extend(["date", "elderberry"])`        |
| `insert(i, x)`                                                                            | Inserts an item `x` at a given position `i`                             | `fruits.insert(1, "cherry")`                   |
| `remove(x)`                                                                               | Removes the first item from the list whose value is `x`                 | `fruits.remove("banana")`                      |
| `pop([i])`                                                                                | Removes and returns the item at position `i` (default is the last item) | `fruits.pop()` or `fruits.pop(1)`              |
| `clear()`                                                                                 | Removes all items from the list                                         | `fruits.clear()`                               |
| `index(x, start, end)`                                                                    | Returns the index of the first item whose value is `x`                  | `fruits.index("cherry")`                       |
| `count(x)`                                                                                | Returns the number of times `x` appears in the list                     | `fruits.count("banana")`                       |
| [`sort(key=None, reverse=False)`](https://www.w3schools.com/python/python_lists_sort.asp) | Sorts the items of the list in place                                    | `fruits.sort()` or `fruits.sort(reverse=True)` |
| `reverse()`                                                                               | Reverses the elements of the list in place                              | `fruits.reverse()`                             |
| `copy()`                                                                                  | Returns a shallow copy of the list                                      | `new_fruits = fruits.copy()`                   |

## `LIST COMPREHENSIONS`

[CLICK HERE](../advancedConcepts/comprehensions.md) to learn about list comprehensions.
