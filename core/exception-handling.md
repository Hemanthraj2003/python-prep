# EXCEPTION HANDLING

Exception handling in Python is a mechanism that allows you to gracefully handle `errors` and `exceptions` that may occur during the execution of a program. It helps to prevent the program from crashing and provides a way to respond to unexpected situations.

## Key Concepts

1. **`Exceptions`**: An exception is an event that occurs during the execution of a program that disrupts the normal flow of instructions. Common exceptions include `ZeroDivisionError`, `ValueError`, `TypeError`, etc.

2. **`Error`**: An error is a more serious issue that typically cannot be handled by the program. Examples include `SyntaxError` and `IndentationError`.

## EXCEPTION CLASSES

Python has a built-in hierarchy of exception classes.

- `Exception` is the sub-class of the `BaseException` class.
- All other exception classes are derived from the `Exception` class. except for `SystemExit`, `KeyboardInterrupt`, and `GeneratorExit`, which are derived directly from `BaseException`.
- You can create your own custom exception classes by inheriting from the `Exception` class.
  Here are some commonly used exception classes in Python:
- `ArithmeticError`
- `ZeroDivisionError`
- `ValueError`
- `TypeError`
- `IndexError`
- `KeyError`
- `FileNotFoundError`
- `ImportError`
- `AttributeError`
- `RuntimeError`
- `StopIteration`
- `KeyboardInterrupt`
- `SystemExit`

## HANDLING EXCEPTIONS

To handle exceptions in python, you can use the `try`, `except`, `else`, and `finally` blocks.

### `try` and `except`

The `try` block contains the code that may raise an exception, while the `except` block contains the code that handles the exception.

```python
try:
    # Code that may raise an exception
    result = 10 / 0
except ZeroDivisionError:
    # Code to handle the exception
    print("Error: Division by zero is not allowed.")
```

we can access the exception object using the `as` keyword.

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")

# Output: Error: division by zero
```

we can also catch multiple exceptions in a single `except` block by specifying a tuple of exception types or by using multiple `except` blocks.

```python
try:
    value = int(input("Enter a number: "))
    result = 10 / value
#except (ValueError, ZeroDivisionError) as e:
 except ValueError as e:
    print(f"Error: Invalid input. {e}")
 except ZeroDivisionError as e:
    print(f"Error: Division by zero is not allowed. {e}")
```

### `else` Block

The `else` block is executed if no exceptions are raised in the `try` block.
`else` block can also be used with `for` statements.

```python
try:
    value = int(input("Enter a number: "))
    result = 10 / value
except (ValueError, ZeroDivisionError) as e:
    print(f"Error: {e}")
else:
    print(f"Result: {result}")
```

T

### `finally` Block

The `finally` block is executed regardless of whether an exception was raised or not. It is typically used for cleanup actions, such as closing files or releasing resources.

```python
try:
    value = int(input("Enter a number: "))
    result = 10 / value
except (ValueError, ZeroDivisionError) as e:
    print(f"Error: {e}")
else:
    print(f"Result: {result}")
finally:
    print("Execution completed.")

# Output:
# Enter a number: 0
# Error: Division by zero is not allowed.
# Execution completed.
```

### `Raising Exceptions`

You can raise exceptions manually using the `raise` statement.

```python
def divide(a, b):
    if b == 0:
        raise ValueError("Denominator cannot be zero.")
    return a / b

try:
    result = divide(10, 0)
except ValueError as e:
    print(f"Error: {e}")

# Output: Error: Denominator cannot be zero.
```

### `Custom Exceptions`

You can create your own custom exception classes by inheriting from the built-in `Exception` class.

```python
class CustomError(Exception):
    pass

def risky_function():
    raise CustomError("Something went wrong in risky_function.")

try:
    risky_function()
except CustomError as e:
    print(f"Error: {e}")

# Output: Error: Something went wrong in risky_function.
```

These are some of the key concepts and techniques for exception handling in Python. Proper exception handling is crucial for building robust and reliable applications.
