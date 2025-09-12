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

| Input Value | Expression   | Output       | Description                            |
| ----------- | ------------ | ------------ | -------------------------------------- |
| `"10"`      | `int("10")`  | `10`         | String to integer                      |
| `3.7`       | `int(3.7)`   | `3`          | Float to integer (truncates)           |
| `True`      | `int(True)`  | `1`          | Boolean to integer                     |
| `False`     | `int(False)` | `0`          | Boolean to integer                     |
| `""`        | `int("")`    | `ValueError` | Cannot convert empty string to integer |

**Note:** When converting a float to an integer, the decimal part is truncated (not rounded).

## `float()`

- Converts a value to a float.

| Input Value | Expression      | Output       | Description                 |
| ----------- | --------------- | ------------ | --------------------------- |
| `"10"`      | `float("10")`   | `10.0`       | String to float             |
| `3`         | `float(3)`      | `3.0`        | Integer to float            |
| `True`      | `float(True)`   | `1.0`        | Boolean to float            |
| `False`     | `float(False)`  | `0.0`        | Boolean to float            |
| `""`        | `float("")`     | `ValueError` | Cannot convert empty string |
| `"3.14"`    | `float("3.14")` | `3.14`       | String to float             |
| `"abc"`     | `float("abc")`  | `ValueError` | Cannot convert non-numeric  |

**Note:** Strings must represent valid numbers to be converted to float.

## `str()`

- Converts a value to a string.

| Input Value | Expression       | Output        | Description       |
| ----------- | ---------------- | ------------- | ----------------- |
| `10`        | `str(10)`        | `"10"`        | Integer to string |
| `3.14`      | `str(3.14)`      | `"3.14"`      | Float to string   |
| `True`      | `str(True)`      | `"True"`      | Boolean to string |
| `False`     | `str(False)`     | `"False"`     | Boolean to string |
| `[1, 2, 3]` | `str([1, 2, 3])` | `"[1, 2, 3]"` | List to string    |

**Note:** Every value can be converted to a string.
