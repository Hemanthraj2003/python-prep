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

## Arguments and Parameters

Their are two types of arguements

- **Formal Arguments** : these are the parameters defined in the function definition.

- **Actual Arguments** : these are the values that passed to the function when it is called.

```python
def multiply(x, y):  # x and y are formal arguments
    return x * y

result = multiply(5, 10)  # 5 and 10 are actual arguments
```

## Types of Actual Arguments

Their are four types of actual arguments:

1. **Positional Actual Arguments**

2. **Keyword Actual Arguments**

3. **Default Actual Arguments**

4. **Variable-length Actual Arguments**

### 1. `Positional Actual Arguments`

The `actual arguments` are passed in the same order as the `formal arguments`.

```python
def subtract(a, b):
    return a - b

result = subtract(10, 5)  # positional arguments
print(result)  # Output: 5
```

### 2. `Keyword Actual Arguments`

The `actual arguments` are passed by explicitly specifying the name of the `formal arguments`.

- order of the actual arguments does not matter.

```python
def divide(a, b):
    return a / b
result = divide(b=2, a=10)  # keyword arguments
print(result)  # Output: 5.0
```

- `Note:` we can use both positional and keyword arguments in the same function call but positional arguments must be placed before keyword arguments.

```python
def demonstrate(a, b, c):
    return a + b * c

result = demonstrate(2, c=3, b=4)  # valid
print(result)  # Output: 14
# result = demonstrate(a=2, 3, b=4)  # invalid
```

### 3. `Default Actual Arguments`

We can assign default values to the `formal arguments` in the function definition. and the `actual arguments` for those can be omitted / optional when calling the function.

```python
def power(base, exponent=2):  # exponent has a default value of 2
    return base ** exponent

print(power(3))      # Output: 9 (3^2)
print(power(3, 3))   # Output: 27 (3^3)
```

- `note:` default arguments must be placed after non-default arguments in the function definition.

```python
def example(a, b=2, c=3):  # valid
    return a + b + c

def example_invalid(a=1, b, c=3):  # invalid
    return a + b + c
```

### 4. `Variable-length Actual Arguments`

These types of arguments allow `n-number` of arguments to be passed to a function. we use `*` and `**` symbols to denote variable-length arguments.\
There are two types of variable-length arguments:

- **Non-keyword Variable-length Arguments** : denoted by `*` symbol, allows passing a variable number of non-keyword arguments to a function, usually called as `*args`. and are represented as a tuple inside the function.

  ```python
  def sum_all(*args):
      total = 0
      for num in args:
          total += num
      return total
  print(sum_all(1, 2, 3))          # Output: 6
  ```

- **Keyword Variable-length Arguments** : denoted by `**` symbol, allows passing a variable number of keyword arguments to a function, usually called as `**kwargs`. and are represented as a dictionary inside the function.

  ```python
  def display_info(**kwargs):
      for key, value in kwargs.items():
          print(f"{key}: {value}")
  display_info(name="Alice", age=30, city="New York")
  # Output:
  # name: Alice
  # age: 30
  # city: New York
  ```

## Recursive Functions

A recursive function is a function that calls itself in order to solve a problem.\
A recursive function must have a base case to stop the recursion, otherwise it will lead to infinite recursion and eventually a stack overflow error.

### Example

```python
def factorial(n):
    # Base case
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)  # Recursive case
print(factorial(5))  # Output: 120
```

## Anonymous Functions (Lambda Functions)

Anonymous functions are functions that are defined without a name, using the `lambda` keyword.\

- Syntax: `lambda arguments: expression`

- contains a single expression, whose result is returned.

- and the expression is evaluated and returned implicitly.

### Example

```python
# Regular function
def add(x, y):
    return x + y

# Lambda function
add_lambda = lambda x, y: x + y
print(add(2, 3))          # Output: 5
print(add_lambda(2, 3))   # Output: 5
```

### Use Cases of Lambda Functions

- used in higher-order functions like `map()`, `filter()`, and `reduce()`.

```python
# Using lambda with map()
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))
print(squared)  # Output: [1, 4, 9, 16, 25]

# Using lambda with filter()
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # Output: [2, 4]
```

## Inner Functions

An inner function is a function defined inside another function.\
Inner functions can access variables and parameters of the outer function.

### Example

```python
def outer_function(text):
    def inner_function():
        print(text)
    inner_function()
outer_function("Hello from inner function!")
# Output: Hello from inner function!
```

### `NOTE:`

1. we can return anything from a function including other functions.

   ```python
   def outer_function():
       def inner_function():
           return "Hello from inner function!"
       return inner_function  # returning the inner function
   my_func = outer_function()  # my_func now holds the inner_function
   print(my_func())  # Output: Hello from inner function!
   ```

2. we can also pass functions as arguments to other functions.

   ```python
   def greet(name):
       return "Hello, " + name + "!"
   def call_function(func, arg):
       return func(arg)
   result = call_function(greet, "Alice")
   print(result)  # Output: Hello, Alice!
   ```
