# DATA TYPES

## What is a Data Type ?

A **_data type_** is a classification that specifies which type of data a variable can hold.

- It determines the operations that can be performed on the data, way the data is stored, and the space it occupies in the memory.

- Python automatically assigns a data type based on the value assigned, but we can also explicitly specify the data using `constructor functions` (also known as [**_type casting_**](./type-casting.md) ).

## Data Types in Python

In python data-types can be classified into the following categories:

1. _Numeric Types_: `int` , `float`, `complex`.

2. _Sequence Types_: `list`, `tuple`, `range`.

3. _Text Types_: `str`.

4. _Set Types_: `set`, `frozenset`.

5. _Mapping Type_: `dict`.

6. _Boolean Type_: `bool`.

7. _Binary Types_: `bytes`, `bytearray`, `memoryview`.

8. _None Type_: `NoneType`.

---

### **`Numeric Types`**

Numeric types are used to represent numbers in Python.

1. **Integer** (`int`): Represents whole numbers, both positive and negative, without decimal points.\
   default value is `0`.

2. **Floating-point** (`float`): Represents real numbers with decimal points.\
   default value is `0.0`.

3. **Complex** (`complex`): Represents Complex numbers, which have a real and an imaginary part.\
   default value is `0 + 0j`.

```python
#Integer
a= 5
print(type(a))  # Output: <class 'int'>

#Float
b= 5.5
print(type(b))  # Output: <class 'float'>

#Complex
c= 3 + 4j
print(type(c))  # Output: <class 'complex'>
```

### **`Boolean Type`**

The Boolean type represents one of two values: `True` or `False`. It is often used in conditional statements and logical operations.\
default value is `False`.

```python
#Boolean
is_active = True
print(type(is_active))  # Output: <class 'bool'>
```

### **`None Type`**

The `NoneType` represents the absence of a value or a null value. It is commonly used to indicate that a variable has no value assigned to it.\
default value is `None`.

```python
#NoneType
result = None
print(type(result))  # Output: <class 'NoneType'>
```

## Checking Data Type of a Variable

We can check the data type of a variable using the built-in `type()` function.

```python
# Checking data type of an integer
num = 10
print(type(num))  # Output: <class 'int'>
```

## KEY DATATYPES IN DETAILS

[STRINGS](../datatypes/string.md)\
\
[LISTS](../datatypes/lists.md)
\
\
[TUPLES](../datatypes/tuples.md)
\
\
[SETS](../datatypes/sets.md)
\
\
[DICTIONARIES](../datatypes/dicts.md)

---

[HOME: Index](../README.md)

[NEXT - TYPE CASTING](type-casting.md)
