# TYPE CASTING

- We can convert one data type to another data type using type casting.
- Python provides several built-in types for type casting.
- Common type casting include:
  - `int()`: Converts a value to an integer.
  - `float()`: Converts a value to a float.
  - `str()`: Converts a value to a string.
  - `list()`: Converts a value to a list.
  - `tuple()`: Converts a value to a tuple.
  - `set()`: Converts a value to a set.
  - `dict()`: Converts a value to a dictionary.

built-in types are basically a class and the values are passed to the constructor and new object of its type is returned.

**Example**

```python
a = 5
b = float(a) # we pass a to float classes constructor
```

## `int()`

- Converts a value to an integer.

| Input Value | Expression   | Output | Description                  |
| ----------- | ------------ | ------ | ---------------------------- |
| `"10"`      | `int("10")`  | `10`   | String to integer            |
| `3.7`       | `int(3.7)`   | `3`    | Float to integer (truncates) |
| `True`      | `int(True)`  | `1`    | Boolean to integer           |
| `False`     | `int(False)` | `0`    | Boolean to integer           |

**Note:** When converting a float to an integer, the decimal part is truncated (not rounded).
