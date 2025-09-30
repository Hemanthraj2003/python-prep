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
