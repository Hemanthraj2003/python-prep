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

## Higher-Order Functions

A higher-order function is a function that either takes one or more functions as arguments or returns a function as its result.

```python
def greet(name):
    return f'Hello, {name}!'
def call_function(func, name):
    return func(name)
result = call_function(greet, 'Alice')
print(result)  # Output: Hello, Alice!
```

In this example, `call_function` is a higher-order function because it takes another function `greet` as an argument and calls it.

## Creating a Decorator

A decorator is a function that takes another function as an argument, adds some functionality to it, and returns a new function. So we use inner functions and higher-order functions to create decorators.

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

def say_hello():
    print("Hello!")

decorated_function = my_decorator(say_hello)

decorated_function()

# Output:
# Something is happening before the function is called.
# Hello!
# Something is happening after the function is called.
```

In this example, `my_decorator` is a decorator that adds behavior before and after the `say_hello` function is called.

## Using the `@` Syntax

Python provides a special syntax using the `@` symbol to apply decorators in a more readable way.

```python
@my_decorator
def say_hello():
    print("Hello!")

say_hello()

# Output:
# Something is happening before the function is called.
# Hello!
# Something is happening after the function is called.
```

### `Decorators with Arguments`

If the decorated function takes arguments, the wrapper function inside the decorator must also accept those arguments.

```python
def my_decorator(func):

    def wrapper(name):
        # most of the time we use *args
        # and **kwargs to handle any number of arguments
        print("Before the function call.")
        func(name)
        print("After the function call.")


    return wrapper

@my_decorator
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")

# Output:
# Before the function call.
# Hello, Alice!
# After the function call.
```

### `Decorators with Parameters`

If you want to pass parameters to a decorator itself, you need to create a `decorator factory` that returns a `decorator`.

```python
# syntax
def decorator_factory(param):
    def my_decorator(func):
        def wrapper(*args, **kwargs):
            func(*args, **kwargs)
        return wrapper
    return my_decorator

@decorator_factory("parameter")
def functions(parameters):
    // code logic
```

#### Example

```python
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(n):
                func(*args, **kwargs)

            return wrapper

    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")

# Output:
# Hello, Alice!
# Hello, Alice!
# Hello, Alice!
```

### Decorator Chaining

You can apply multiple decorators to a single function by stacking them on top of each other.

```python
def decorator_one(func):
    def wrapper():
        print("Decorator One")
        func()
    return wrapper

def decorator_two(func):
    def wrapper():
        print("Decorator Two")
        func()
    return wrapper

@decorator_one
@decorator_two
def say_hello():
    print("Hello!")

say_hello()

# Output:
# Decorator One
# Decorator Two
# Hello!
```

> we learn `class decorators` in the OOPS Concepts
