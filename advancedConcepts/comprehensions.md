# COMPREHENSIONS

Comprehensions provide a concise way to create lists, sets, or dictionaries in Python.\
They consist of brackets containing an expression followed by a `for` clause, and can include optional `if` clauses to filter items.

## List Comprehensions

- List comprehensions are used to create lists.

- **Syntax**:

  ```python
  resultList = [expression for item in iterable if condition]
  ```

- **Example**:

  ```python
  # Create a list of squares of even numbers from 0 to 9
  squares = [x**2 for x in range(10) if x % 2 == 0]
  print(squares)
  # Output: [0, 4, 16, 36, 64]
  ```

- **Example**:

  ```python
  # take users inputs
  user_inputs = [int(input("Enter a number: ")) for _ in range(5)]
  ```

- **Example**:

  ```python
  # nested list comprehension
  ```

## Set Comprehensions

- Set comprehensions are used to create sets.

- **Syntax**:

  ```python
  resultSet = {expression for item in iterable if condition}
  ```

## Dictionary Comprehensions

- Dictionary comprehensions are used to create dictionaries.

- **Syntax**:

  ```python
  resultDict = {key_expression: value_expression for item in iterable if condition}
  ```

- **Example**:

  ```python
   with open('file.txt', 'r') as file:
        lines = file.readlines()
        lines = [line for line in lines]
        line_lengths = {line: len(line) for line in lines.split()}
  ```
