# OPERATORS

- Operators are special symbols in Python that perform operations on variables and values.

## Types of operators

1. Arithmetic Operators / Mathematical Operators

2. Comparison Operators / Relational Operators

3. Logical Operators / Boolean Operators

4. Assignment Operators

5. Bitwise Operators

6. Membership Operators

7. Identity Operators

## 1. Arithmetic Operators / Mathematical Operators

- these operators are used to perform mathematical operations like addition, subtraction, multiplication, etc.

| Operator | Name           | Example  | Description                                     |
| -------- | -------------- | -------- | ----------------------------------------------- |
| `+`      | Addition       | a + b    | Adds two operands or concatenates strings       |
| `-`      | Subtraction    | a - b    | Subtracts second operand from the first         |
| `*`      | Multiplication | a \* b   | Multiplies two operands                         |
| `/`      | Division       | a / b    | Divides first operand by the second             |
| `%`      | Modulus        | a % b    | Returns the remainder of division               |
| `**`     | Exponentiation | a \*\* b | Raises first operand to the power of the second |
| `//`     | Floor Division | a // b   | Divides and rounds down                         |

## 2. Comparison Operators / Relational Operators

- these operators are used to compare two values. they return either `True or False` based on the condition.

| Operator | Name                     | Example | Description                                              |
| -------- | ------------------------ | ------- | -------------------------------------------------------- |
| `==`     | Equal / Equality         | a== b   | Returns `True` if both operands are equal                |
| `!=`     | Not Equal / Inequality   | a != b  | Return `True` if operands are not equal                  |
| `>`      | Greater Than             | a > b   | Returns `True` if left is greater than right             |
| `<`      | Less Than                | a < b   | Returns `True` if left is less than right                |
| `>=`     | Greater Than or Equal To | a >= b  | Returns `True` if left is greater than or equal to right |
| `<=`     | Less Than or Equal To    | a <= b  | Returns `True` if left is less than or equal to right    |

**note:** Comparison operators can be used with all data types.\
for example, you can compare

- `strings` :

  - `"apple" == "apple"` returns `True` because both strings are identical.
  - `"apple" != "banana"` returns `True` because the strings are different.
  - `"apple" > "banana"` returns `False` because "apple" comes before "banana" in alphabetical order.

- `lists` :

  - `[1, 2, 3] == [1, 2, 3]` returns `True` because both lists have the same elements in the same order.
  - `[1, 2, 3] != [1, 2, 4]` returns `True` because the lists are different.
  - `[1, 2, 3] < [1, 2, 4]` returns `True` because the first list is considered less than the second list when compared element by element.

- `sets` :

  - `{1, 2, 3} == {3, 2, 1}` returns `True` because sets are unordered collections and contain the same elements.
  - `{1, 2, 3} != {1, 2, 4}` returns `True` because the sets are different.
  - `{1, 2} < {1, 2, 3}` returns `True` because the first set is a proper subset of the second set.

- `dictionaries` :
  - `{'a': 1, 'b': 2} == {'b': 2, 'a': 1}` returns `True` because dictionaries are unordered collections and contain the same key-value pairs.
  - `{'a': 1, 'b': 2} != {'a': 1, 'b': 3}` returns `True` because the dictionaries are different.
  - `{'a': 1} < {'a': 1, 'b': 2}` returns `True` because the first dictionary is a proper subset of the second dictionary.
  - `{'a': 1, 'b': 2} > {'a': 1, 'b': 2, 'c': 3}` returns `False` because the first dictionary is not greater than the second dictionary.

## 3. Logical Operators / Boolean Operators

- these operators are used to combine conditional statements. they return either `True or False` based on the condition.

| Operator | Name        | Example | Description                                               |
| -------- | ----------- | ------- | --------------------------------------------------------- |
| `and`    | Logical AND | a and b | Returns `True` if both statements are true                |
| `or`     | Logical OR  | a or b  | Returns `True` if one of the statements is true           |
| `not`    | Logical NOT | not a   | Reverse the result, returns `False` if the result is true |

**note:** Logical operators are typically used with boolean values (`True` or `False`). however, they can also be used with non-boolean values. in such cases, the values are evaluated based on their "truthiness" or "falsiness".

## 4. Assignment Operators

- these operators are used to assign values to variables.

| Operator | Example   | Description                                          |
| -------- | --------- | ---------------------------------------------------- |
| `=`      | a = 5     | Assigns value of right to left                       |
| `+=`     | a += 5    | Adds right to left and assigns                       |
| `-=`     | a -= 5    | Subtracts right from left and assigns                |
| `*=`     | a \*= 5   | Multiplies left with right and assigns               |
| `/=`     | a /= 5    | Divides left by right and assigns                    |
| `%=`     | a %= 5    | Takes modulus using two operands and assigns         |
| `**=`    | a \*\*= 5 | Performs exponential (power) calculation and assigns |
| `//=`    | a //= 5   | Performs floor division and assigns                  |

**note:** Assignment operators can be used with all data types.

## 5. Bitwise Operators

- these operators are used to perform bit-level operations on binary numbers.
  | Operator | Name | Example | Description |
  | -------- | ------------ | ------- | --------------------------------------------------- |
  | `&` | Bitwise AND | a & b | Sets each bit to 1 if both bits are 1 |
  | `\|` | Bitwise OR | a \| b | Sets each bit to 1 if one of two bits is 1 |
  | `^` | Bitwise XOR | a ^ b | Sets each bit to 1 if only one of two bits is 1 |
  | `~` | Bitwise NOT | ~a | Inverts all the bits |
  | `<<` | Bitwise Left Shift | a << 2 | Shifts bits of a to the left by 2 places |
  | `>>` | Bitwise Right Shift | a >> 2 | Shifts bits of a to the right by 2 places |

**note:** Bitwise operators are typically used with integers. they operate on the binary representations of the numbers.

```python
# Example:
a = 60          # 60 = 0011 1100
b = 13          # 13 = 0000 1101

print(a & b)   # Output: 12 = 0000 1100

print(a | b)   # Output: 61 = 0011 1101

print(a ^ b)   # Output: 49 = 0011 0001

print(~a)      # Output: -61 = 1100 0011 (in two's complement form)

print(a << 2)  # Output: 240 = 1111 0000

print(a >> 2)  # Output: 15 = 0000 1111
```

## 6. Membership Operators

- these operators are used to test if a sequence (like a string, list, tuple, set or dictionary) contains a certain value.

| Operator | Example    | Description                                        |
| -------- | ---------- | -------------------------------------------------- |
| `in`     | x in y     | Returns `True` if x is found in the sequence y     |
| `not in` | x not in y | Returns `True` if x is not found in the sequence y |

**note:** Membership operators can be used with all sequence types like strings, lists, tuples, sets, and dictionaries.

```python
# Using 'in' operator
print('a' in 'apple')          # Output: True
print(2 in [1, 2, 3])          # Output: True
print(3 in (1, 2, 3))          # Output: True
print(1 in {1, 2, 3})          # Output: True
print('key1' in {'key1': 1, 'key2': 2})
# Output: True
```

## 7. Identity Operators

- these operators are used to compare the memory locations of two objects.

| Operator | Example    | Description                                                           |
| -------- | ---------- | --------------------------------------------------------------------- |
| `is`     | x is y     | returns `True` if both variables point to the same object in memory   |
| `is not` | x is not y | returns `True` if both variables point to different objects in memory |

**note:** Identity operators are typically used with objects to check if they are the same instance.

```python
a = [1, 2, 3]
b = a
c = [1, 2, 3]

print(a is b)
# Output: True

print(a is c)
# Output: False

print(a == c)
# Output: True
```

---

[HOME: Index](../README.md)

[NEXT: CONTROL STATEMENTS](control-statements.md)
