# CLOSURES

## Definition

A closure is a function that captures and retains access to variables from its outer (enclosing) scope even after the outer function has finished executing. It's a combination of a function and the lexical environment within which that function was defined.

## Key Concepts

### 1. Basic Closure Structure

```python
def outer_function(x):
    # This is the enclosing scope
    def inner_function(y):
        # Inner function has access to 'x' from outer scope
        return x + y

    return inner_function  # Return the inner function

# Create a closure
add_five = outer_function(5)
print(add_five(10))  # Output: 15
```

### 2. How Closures Work

- The inner function maintains a reference to variables in the outer scope
- These variables are kept alive even after the outer function returns
- The closure "closes over" the free variables from the enclosing scope

```python
def make_multiplier(factor):
    def multiplier(number):
        return number * factor
    return multiplier

# Creating closures with different factors
double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5))   # 10
print(triple(5))   # 15

# Each closure maintains its own copy of 'factor'
print(double.__closure__[0].cell_contents)  # 2
print(triple.__closure__[0].cell_contents)  # 3
```

### 3. Practical Examples

#### Counter with Closure

```python
def make_counter():
    count = 0

    def counter():
        nonlocal count
        count += 1
        return count

    return counter

# Each counter maintains its own state
counter1 = make_counter()
counter2 = make_counter()

print(counter1())  # 1
print(counter1())  # 2
print(counter2())  # 1
print(counter1())  # 3
```

#### Configuration Handler

```python
def create_config_handler(config_name):
    settings = {}

    def set_setting(key, value):
        settings[key] = value
        print(f"[{config_name}] Set {key} = {value}")

    def get_setting(key):
        return settings.get(key, f"Setting '{key}' not found")

    def get_all_settings():
        return settings.copy()

    # Return multiple functions that share the same closure
    return set_setting, get_setting, get_all_settings

# Usage
db_set, db_get, db_all = create_config_handler("Database")
web_set, web_get, web_all = create_config_handler("WebServer")

db_set("host", "localhost")
db_set("port", 5432)
web_set("port", 8080)

print(db_get("host"))     # localhost
print(web_get("port"))    # 8080
print(db_all())           # {'host': 'localhost', 'port': 5432}
```

#### Event Handler with State

```python
def create_event_handler(event_type):
    call_count = 0
    handlers = []

    def add_handler(func):
        handlers.append(func)
        return func

    def trigger_event(*args, **kwargs):
        nonlocal call_count
        call_count += 1
        results = []

        print(f"Triggering {event_type} event (call #{call_count})")
        for handler in handlers:
            result = handler(*args, **kwargs)
            results.append(result)

        return results

    def get_stats():
        return {
            'event_type': event_type,
            'call_count': call_count,
            'handler_count': len(handlers)
        }

    return add_handler, trigger_event, get_stats

# Usage
add_click_handler, trigger_click, click_stats = create_event_handler("click")

@add_click_handler
def log_click(x, y):
    print(f"Click logged at ({x}, {y})")
    return f"logged_{x}_{y}"

@add_click_handler
def validate_click(x, y):
    is_valid = 0 <= x <= 100 and 0 <= y <= 100
    print(f"Click validation: {'valid' if is_valid else 'invalid'}")
    return is_valid

# Trigger events
trigger_click(50, 75)
trigger_click(150, 200)
print(click_stats())
```

### 4. Advanced Closure Patterns

#### Function Factory with Parameters

```python
def create_validator(min_val, max_val, data_type=int):
    def validate(value):
        try:
            converted_value = data_type(value)
            if min_val <= converted_value <= max_val:
                return True, converted_value
            else:
                return False, f"Value must be between {min_val} and {max_val}"
        except (ValueError, TypeError):
            return False, f"Value must be of type {data_type.__name__}"

    # Add metadata to the closure
    validate.min_val = min_val
    validate.max_val = max_val
    validate.data_type = data_type

    return validate

# Create different validators
age_validator = create_validator(0, 120)
percentage_validator = create_validator(0.0, 100.0, float)

print(age_validator(25))      # (True, 25)
print(age_validator(150))     # (False, 'Value must be between 0 and 120')
print(percentage_validator("85.5"))  # (True, 85.5)
```

#### Caching with Closures (Simple Memoization)

```python
def memoize():
    cache = {}

    def decorator(func):
        def wrapper(*args):
            if args in cache:
                print(f"Cache hit for {func.__name__}{args}")
                return cache[args]

            print(f"Computing {func.__name__}{args}")
            result = func(*args)
            cache[args] = result
            return result

        def clear_cache():
            cache.clear()
            print(f"Cache cleared for {func.__name__}")

        def cache_info():
            return {
                'size': len(cache),
                'keys': list(cache.keys())
            }

        wrapper.clear_cache = clear_cache
        wrapper.cache_info = cache_info
        return wrapper

    return decorator

@memoize()
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Usage
print(fibonacci(10))  # Computes and caches intermediate results
print(fibonacci(8))   # Uses cached results
print(fibonacci.cache_info())
```

### 5. Closure vs Class Comparison

```python
# Using Closure
def make_account_closure(initial_balance):
    balance = initial_balance

    def deposit(amount):
        nonlocal balance
        balance += amount
        return balance

    def withdraw(amount):
        nonlocal balance
        if amount <= balance:
            balance -= amount
            return balance
        else:
            raise ValueError("Insufficient funds")

    def get_balance():
        return balance

    return deposit, withdraw, get_balance

# Using Class
class Account:
    def __init__(self, initial_balance):
        self.balance = initial_balance

    def deposit(self, amount):
        self.balance += amount
        return self.balance

    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount
            return self.balance
        else:
            raise ValueError("Insufficient funds")

    def get_balance(self):
        return self.balance

# Closure usage
deposit, withdraw, get_balance = make_account_closure(100)
print(deposit(50))    # 150
print(withdraw(30))   # 120

# Class usage
account = Account(100)
print(account.deposit(50))   # 150
print(account.withdraw(30))  # 120
```

### 6. Common Pitfalls and Solutions

#### Late Binding Problem

```python
# Problem: All closures reference the same variable
functions = []
for i in range(3):
    functions.append(lambda: i)

for func in functions:
    print(func())  # Prints 2, 2, 2 (not 0, 1, 2)

# Solution 1: Use default argument
functions_fixed1 = []
for i in range(3):
    functions_fixed1.append(lambda x=i: x)

for func in functions_fixed1:
    print(func())  # Prints 0, 1, 2

# Solution 2: Create a closure factory
def make_closure(value):
    return lambda: value

functions_fixed2 = []
for i in range(3):
    functions_fixed2.append(make_closure(i))

for func in functions_fixed2:
    print(func())  # Prints 0, 1, 2
```

#### Memory Considerations

```python
def potential_memory_leak():
    large_data = list(range(1000000))  # Large list

    def small_function():
        return "Hello"  # Doesn't use large_data

    return small_function

# The closure keeps reference to large_data even though it's not used
func = potential_memory_leak()

# Better approach: Only capture what you need
def memory_efficient():
    large_data = list(range(1000000))
    needed_value = large_data[0]  # Extract only what's needed

    def small_function():
        return f"First value: {needed_value}"

    return small_function
```

## Interview Insights

### Common Interview Questions

1. **"What is a closure and how does it work?"**

   - A function that retains access to variables from its enclosing scope
   - Variables are captured by reference, not by value
   - Enables data encapsulation and state preservation

2. **"What's the difference between closures and classes?"**

   - Closures: Functional approach, implicit state, simpler for single responsibility
   - Classes: Object-oriented approach, explicit state, better for complex objects

3. **"When would you use closures?"**
   - Factory functions that create specialized functions
   - Event handlers that need to maintain state
   - Decorators and higher-order functions
   - Simple data encapsulation without full classes

### Performance Considerations

```python
import time
from functools import wraps

def timing_closure():
    def decorator(func):
        call_count = 0
        total_time = 0

        @wraps(func)
        def wrapper(*args, **kwargs):
            nonlocal call_count, total_time
            start = time.time()
            result = func(*args, **kwargs)
            execution_time = time.time() - start

            call_count += 1
            total_time += execution_time

            return result

        def get_stats():
            avg_time = total_time / call_count if call_count > 0 else 0
            return {
                'calls': call_count,
                'total_time': total_time,
                'avg_time': avg_time
            }

        wrapper.get_stats = get_stats
        return wrapper

    return decorator

@timing_closure()
def slow_function():
    time.sleep(0.1)
    return "Done"

# Usage
slow_function()
slow_function()
print(slow_function.get_stats())
```

### Key Interview Points

1. **Lexical Scoping**: Closures capture variables from the lexical scope
2. **State Persistence**: Variables remain accessible after outer function returns
3. **Memory Implications**: Closures keep references to outer scope variables
4. **Late Binding**: Variables are bound at runtime, not definition time
5. **Use Cases**: Factory functions, decorators, event handlers, callbacks

### Best Practices

1. Use closures for simple state encapsulation
2. Be mindful of memory usage with large outer scope variables
3. Use `nonlocal` keyword when modifying outer scope variables in Python 3
4. Consider classes for complex state management
5. Document closure behavior clearly
6. Test for late binding issues in loops

## Related Concepts

- Lexical Scoping and Variable Resolution
- Decorators and Higher-Order Functions
- Function Objects and First-Class Functions
- Memory Management and Garbage Collection
- Functional Programming Patterns
