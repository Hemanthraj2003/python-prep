# FUNCTIONS

Function is a block of code that that contains sets of instructions to perform a specific task.

## Defining a Function

we use `def` keyword to define a function in python, followed by `functionName` and parentheses `()`.\
Any input parameters / arguments should be placed within these parentheses.

### Syntax

```python
def function_name(parameters):
    statement 01
    statement 02
    statement 03
```

### Example

```python
def greet(name):
    print("Hello, " +name+ "! Welcome to functions")

greet("ALice")
# Output: Hello, ALice! Welcome to functions
```

## Return Statement

`return` statement is used to exit a function and go back to the place where it was called.\

`return ` statements can contain an expression or value that is returned to the caller.\
If there is no expression or value to return, the return statement without any arguments returns `None`.

```python
def add(a,b):
    return a+b
```
