# CONTROL STATEMENTS

- these statements are used to control the flow of execution in a program based on certain conditions.

## Types of Control Statements

1. `Conditional Statements`

   - used to perform different actions based on different conditions.
   - examples: if statement, switch statement.

2. `Looping Statements`

   - used to execute a block of code repeatedly.
   - examples: for loop, while loop.

3. `Jump Statements`
   - used to alter the flow of control in a program.
   - examples: break statement, continue statement.

## Conditional Statements

- used to perform different actions based on different conditions.

- their are `three` types of conditional statements in python:

  1. `if Statement`

     - used to test a specific condition.
     - example:

       ```python
       if condition:
           # code to execute if condition is true
       ```

  2. `if-else Statement`

     - used to test a specific condition and provide an alternative action if the condition is false.
     - example:

       ```python
       if condition:
           # code to execute if condition is true
       else:
           # code to execute if condition is false
       ```

  3. `if-elif-else Statement`

     - used to test multiple conditions and provide alternative actions for each condition.
     - example:

       ```python
       if condition1:
           # code to execute if condition1 is true
       elif condition2:
           # code to execute if condition2 is true
       else:
           # code to execute if both conditions are false
       ```

  ### Example for conditional statements

  ```python
  if age < 18:
      print("You are a minor.")
  elif age < 65:
      print("You are an adult.")
  else:
      print("You are a senior citizen.")
  ```

## Looping Statements

- used to execute a block of code repeatedly.

- their are two types of looping statements in python:

  1. **`for loop`**:
     used to iterate over a sequence (collection of items) such as a list, tuple, string, range etc..

  - **Syntax**:

    ```python
    for items in sequence:
        # code to execute for each item
    ```

  2. **`while loop`**:
     used to execute a block of code as long as a specified condition is `true`.

  - **Syntax**:

    ```python
    while condition:
        # code to execute while condition is TRUE
    ```

### Example for looping statements

```python
# Example for for loop
for i in range(5):
    print(i)

# Example for while loop
count = 0
while count < 5:
    print(count)
    count += 1
```

## Jump Statements

- these statements are used to alter the flow of control in a program.

- their are two types of jump statements in python:

1. **`break statement`**:
   used to exit a loop or switch statement before it has completed all its iterations.

   - **Syntax**:

     ```python
     break
     ```

2. **`continue statement`**:
   used to skip the current iteration of a loop and move to the next iteration.

   - **Syntax**:

     ```python
     continue
     ```

### Example for jump statements

```python
# Example for break statement
for i in range(10):
    if i == 5:
        break
    print(i)

# Example for continue statement
for i in range(10):
    if i == 5:
        continue
    print(i)
```

### `note :`

- Python does not have a switch statement like some other programming languages. Instead, you can use `if-elif-else statements` or **`match-case`** statements (introduced in Python 3.10) to achieve similar functionality.

- **`match-case statement`**:
  used to match a value against a series of patterns and execute the corresponding block of code.

  - **Syntax**:

    ```python
    match value:
        case pattern1:
            # code to execute if pattern1 matches
        case pattern2:
            # code to execute if pattern2 matches
        case _:
            # code to execute if no patterns match
    ```

### Example for match-case statement

```python
def check_day(day):
    match day:
        case "Monday":
            print("It's the start of the week.")
        case "Friday":
            print("It's almost the weekend!")
        # we can use the OR operator to match multiple cases
        case "Saturday" | "Sunday":
            print("It's the weekend!")
        case _:
            print("It's a regular weekday.")
```

---

[HOME: Index](../README.md)

[NEXT: CONTROL STATEMENTS](control-statements.md)
