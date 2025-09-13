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
