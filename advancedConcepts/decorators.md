# DECORATORS IN PYTHON

A decorator in Python is a special type of function that modifies the behavior of another function or method. Decorators allow you to wrap another function to extend its behavior without modifying its code directly. They are often used for logging, access control, instrumentation, and caching.

**_Before going into the details of decorators, let's understand the concept of higher-order functions and nested functions._**

## Nested Functions / Inner Functions

A nested function is a function defined inside another function. The inner function can access variables and parameters of the outer function.

```python
def outer_function(text):
    def inner_function():
        print(text)
    inner_function()

outer_function('Hello, World!')
# Output: Hello, World!
```

we can even return the inner function from the outer function.

```python
def outer_function(text):
    def inner_function():
        print(text)
    return inner_function

my_function = outer_function('Hello, World!')
my_function()
# Output: Hello, World!
```

In this example, `inner_function` is defined inside `outer_function` and can access the `text` parameter of `outer_function`.
