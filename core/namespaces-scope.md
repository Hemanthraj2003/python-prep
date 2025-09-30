# NAMESPACES AND SCOPE (LEGB RULE)

- **Namespace** is a container that holds a mapping from names to objects.
- **Scope** defines the region of code where a namespace is directly accessible.
- Python follows the **LEGB Rule** for variable lookup: **Local → Enclosing → Global → Built-in**.
- Understanding scope is crucial for avoiding naming conflicts and writing maintainable code.
- Python uses **lexical scoping** (static scoping) - scope is determined by where variables are defined in code.

## `WHAT ARE NAMESPACES?`

### **Definition and Purpose**

```python
# Namespace is like a dictionary mapping names to objects
# Different namespaces can have the same name pointing to different objects

# Global namespace
x = "global x"

def outer_function():
    # Local namespace of outer_function
    x = "outer x"

    def inner_function():
        # Local namespace of inner_function
        x = "inner x"
        print(f"Inner function x: {x}")

    inner_function()
    print(f"Outer function x: {x}")

outer_function()
print(f"Global x: {x}")

# Output:
# Inner function x: inner x
# Outer function x: outer x
# Global x: global x
```

### **Types of Namespaces**

```python
# 1. Built-in namespace - contains built-in functions and exceptions
print(dir(__builtins__)[:10])  # ['ArithmeticError', 'AssertionError', ...]

# 2. Global namespace - module level names
global_var = "I'm global"

# 3. Local namespace - function level names
def my_function():
    local_var = "I'm local"
    print(locals())  # Shows local namespace as dictionary

my_function()
print(globals().keys())  # Shows global namespace keys
```

## `THE LEGB RULE EXPLAINED`

**LEGB** stands for the order in which Python searches for variables:

### **L - Local Scope**

```python
def my_function():
    # Local scope - variables defined inside the function
    local_var = "I'm local"
    x = 10

    print(f"Local variable: {local_var}")
    print(f"Local x: {x}")

# local_var is not accessible outside the function
my_function()
# print(local_var)  # NameError: name 'local_var' is not defined
```

### **E - Enclosing Scope**

```python
def outer_function():
    # Enclosing scope for inner_function
    enclosing_var = "I'm in enclosing scope"
    x = 20

    def inner_function():
        # Can access variables from enclosing scope
        print(f"Accessing enclosing variable: {enclosing_var}")
        print(f"Enclosing x: {x}")

    inner_function()
    return inner_function

# Example of closure - inner function remembers enclosing scope
closure_func = outer_function()
closure_func()  # Still has access to enclosing_var
```

### **G - Global Scope**

```python
# Global scope - module level
global_var = "I'm global"
x = 30

def access_global():
    # Accessing global variable
    print(f"Global variable: {global_var}")
    print(f"Global x: {x}")

def modify_global():
    global x  # Declare x as global to modify it
    x = 100
    print(f"Modified global x: {x}")

access_global()
modify_global()
print(f"Global x after modification: {x}")
```

### **B - Built-in Scope**

```python
# Built-in scope - predefined functions and exceptions
def demonstrate_builtin():
    # These are from built-in scope
    print(len([1, 2, 3]))      # len is built-in
    print(abs(-5))             # abs is built-in
    print(max(1, 2, 3))        # max is built-in

demonstrate_builtin()

# You can shadow built-in names (but shouldn't)
def bad_example():
    len = "I'm not the built-in len"  # Shadows built-in len
    # print(len([1, 2, 3]))  # TypeError: 'str' object is not callable

# bad_example()  # Uncomment to see the error
```

## `COMPLETE LEGB EXAMPLE`

```python
# Built-in: len, print, etc.
# Global scope
x = "global"
y = "global y"

def outer():
    # Enclosing scope
    x = "enclosing"
    z = "enclosing z"

    def inner():
        # Local scope
        x = "local"

        print("=== LEGB Lookup Demonstration ===")
        print(f"x: {x}")           # Local x
        print(f"y: {y}")           # Global y (not found in L, E)
        print(f"z: {z}")           # Enclosing z (not found in L)
        print(f"len([1,2]): {len([1,2])}")  # Built-in len

    inner()
    print(f"In outer - x: {x}")    # Enclosing x

outer()
print(f"In global - x: {x}")       # Global x

# Output:
# === LEGB Lookup Demonstration ===
# x: local
# y: global y
# z: enclosing z
# len([1,2]): 2
# In outer - x: enclosing
# In global - x: global
```

## `SCOPE MODIFICATION KEYWORDS`

### **The `global` Keyword**

```python
counter = 0  # Global variable

def increment_counter():
    global counter  # Declare we want to modify global counter
    counter += 1
    print(f"Counter: {counter}")

def reset_counter():
    global counter
    counter = 0
    print("Counter reset")

# Without global keyword
def increment_local():
    counter = 1  # Creates new local variable
    counter += 1
    print(f"Local counter: {counter}")

increment_counter()    # Counter: 1
increment_counter()    # Counter: 2
increment_local()      # Local counter: 2
print(f"Global counter: {counter}")  # Global counter: 2
reset_counter()        # Counter reset
```

### **The `nonlocal` Keyword**

```python
def outer_function():
    x = "original enclosing"

    def inner_function():
        nonlocal x  # Refer to enclosing scope variable
        x = "modified by inner"
        print(f"Inner modified x: {x}")

    def inner_without_nonlocal():
        x = "local to this function"  # Creates new local variable
        print(f"Inner local x: {x}")

    print(f"Before inner: {x}")
    inner_function()
    print(f"After inner: {x}")

    inner_without_nonlocal()
    print(f"After inner_without_nonlocal: {x}")

outer_function()

# Output:
# Before inner: original enclosing
# Inner modified x: modified by inner
# After inner: modified by inner
# Inner local x: local to this function
# After inner_without_nonlocal: modified by inner
```

## `PRACTICAL EXAMPLES`

### **Example 1: Counter with Closure**

```python
def create_counter():
    """Create a counter function using closure"""
    count = 0

    def counter():
        nonlocal count
        count += 1
        return count

    def reset():
        nonlocal count
        count = 0

    def get_count():
        return count

    # Return multiple functions that share the same enclosing scope
    counter.reset = reset
    counter.get_count = get_count
    return counter

# Create two independent counters
counter1 = create_counter()
counter2 = create_counter()

print(counter1())        # 1
print(counter1())        # 2
print(counter2())        # 1
print(counter1.get_count())  # 2
counter1.reset()
print(counter1())        # 1
```

### **Example 2: Configuration Manager**

```python
# Global configuration
config = {
    "debug": False,
    "max_connections": 100
}

def setup_application():
    """Setup application with global config"""
    global config

    def enable_debug():
        config["debug"] = True
        print("Debug mode enabled")

    def set_max_connections(max_conn):
        config["max_connections"] = max_conn
        print(f"Max connections set to {max_conn}")

    def get_config():
        return config.copy()

    return enable_debug, set_max_connections, get_config

# Setup application
enable_debug, set_connections, get_config = setup_application()

print("Initial config:", get_config())
enable_debug()
set_connections(200)
print("Updated config:", get_config())
```

### **Example 3: Decorator with State**

```python
def call_counter(func):
    """Decorator that counts function calls"""
    count = 0  # Enclosing scope variable

    def wrapper(*args, **kwargs):
        nonlocal count
        count += 1
        print(f"{func.__name__} called {count} times")
        return func(*args, **kwargs)

    def get_count():
        return count

    def reset_count():
        nonlocal count
        count = 0

    wrapper.get_count = get_count
    wrapper.reset_count = reset_count
    return wrapper

@call_counter
def greet(name):
    return f"Hello, {name}!"

@call_counter
def add(a, b):
    return a + b

# Usage
print(greet("Alice"))        # greet called 1 times, Hello, Alice!
print(greet("Bob"))          # greet called 2 times, Hello, Bob!
print(add(3, 4))             # add called 1 times, 7
print(f"Greet count: {greet.get_count()}")  # Greet count: 2
print(f"Add count: {add.get_count()}")      # Add count: 1
```

## `SCOPE AND FUNCTIONS`

### **Function Parameter Scope**

```python
x = "global"

def function_params(x, y="default"):
    """Function parameters create local variables"""
    print(f"Parameter x: {x}")
    print(f"Parameter y: {y}")

    # Parameters are local to the function
    x = "modified local"
    print(f"Modified x: {x}")

function_params("argument")
print(f"Global x unchanged: {x}")  # Global x unchanged: global
```

### **Scope and Default Arguments**

```python
# Default arguments are evaluated once when function is defined
def append_to_list(item, target_list=None):
    """Correct way to handle mutable default arguments"""
    if target_list is None:
        target_list = []  # Create new list each time
    target_list.append(item)
    return target_list

# Wrong way (common pitfall)
def wrong_append(item, target_list=[]):
    """Dangerous: mutable default argument"""
    target_list.append(item)
    return target_list

# Demonstrate the difference
print(append_to_list(1))      # [1]
print(append_to_list(2))      # [2] (new list each time)

print(wrong_append(1))        # [1]
print(wrong_append(2))        # [1, 2] (same list reused!)
```

## `SCOPE AND CLASSES`

### **Class Scope**

```python
class MyClass:
    class_var = "I'm a class variable"  # Class scope

    def __init__(self, name):
        self.name = name  # Instance scope

    def instance_method(self):
        local_var = "I'm local to method"  # Method local scope
        print(f"Instance: {self.name}")
        print(f"Class: {self.class_var}")
        print(f"Local: {local_var}")

    @classmethod
    def class_method(cls):
        print(f"Class variable: {cls.class_var}")
        # print(self.name)  # Error: no access to instance variables

    @staticmethod
    def static_method():
        print("Static method - no access to class or instance variables")

# Usage
obj = MyClass("Object1")
obj.instance_method()
MyClass.class_method()
MyClass.static_method()
```

### **Class Variable vs Instance Variable Scope**

```python
class ScopeDemo:
    shared_var = "shared"  # Class variable

    def __init__(self, value):
        self.instance_var = value  # Instance variable

    def modify_shared(self, new_value):
        # This creates an instance variable, doesn't modify class variable
        self.shared_var = new_value

    def modify_class_shared(self, new_value):
        # This modifies the class variable
        ScopeDemo.shared_var = new_value

obj1 = ScopeDemo("obj1")
obj2 = ScopeDemo("obj2")

print(f"Initial shared: {ScopeDemo.shared_var}")  # shared

obj1.modify_shared("modified by obj1")
print(f"obj1.shared_var: {obj1.shared_var}")      # modified by obj1
print(f"obj2.shared_var: {obj2.shared_var}")      # shared (unchanged)
print(f"Class shared_var: {ScopeDemo.shared_var}")  # shared (unchanged)

obj1.modify_class_shared("modified at class level")
print(f"After class modification: {ScopeDemo.shared_var}")  # modified at class level
```

## `ADVANCED SCOPE CONCEPTS`

### **List Comprehension Scope**

```python
# List comprehension has its own scope (Python 3)
x = "global"
numbers = [x for x in range(5)]  # x here doesn't affect global x
print(f"Global x: {x}")          # global (unchanged in Python 3)
print(f"Numbers: {numbers}")     # [0, 1, 2, 3, 4]

# Nested comprehensions
matrix = [[i + j for j in range(3)] for i in range(3)]
print(matrix)  # [[0, 1, 2], [1, 2, 3], [2, 3, 4]]

# Variables in comprehensions can access enclosing scope
def create_multipliers():
    multipliers = []
    for i in range(3):
        # Closure captures the variable from enclosing scope
        multipliers.append(lambda x, m=i: x * m)  # Use default parameter
    return multipliers

funcs = create_multipliers()
print([f(10) for f in funcs])  # [0, 10, 20]
```

### **Generator Scope**

```python
def generator_scope_demo():
    """Generator functions and scope"""
    enclosing_var = "enclosing"

    def number_generator():
        local_var = "generator local"
        for i in range(3):
            # Generator can access enclosing scope
            yield f"{i}: {enclosing_var} - {local_var}"

    return number_generator()

gen = generator_scope_demo()
for value in gen:
    print(value)

# Output:
# 0: enclosing - generator local
# 1: enclosing - generator local
# 2: enclosing - generator local
```

## `COMMON SCOPE PITFALLS`

### **1. Late Binding Closures**

```python
# Common mistake - all functions refer to the same variable
functions = []
for i in range(3):
    functions.append(lambda x: x * i)  # i is captured by reference

# All functions use the final value of i (2)
print([f(10) for f in functions])  # [20, 20, 20] - not [0, 10, 20]

# Solution 1: Use default parameter
functions_fixed1 = []
for i in range(3):
    functions_fixed1.append(lambda x, multiplier=i: x * multiplier)

print([f(10) for f in functions_fixed1])  # [0, 10, 20]

# Solution 2: Use closure factory
def create_multiplier(multiplier):
    return lambda x: x * multiplier

functions_fixed2 = [create_multiplier(i) for i in range(3)]
print([f(10) for f in functions_fixed2])  # [0, 10, 20]
```

### **2. Unintended Global Modification**

```python
total = 0

def add_to_total(value):
    # Forgot to declare global - creates local variable instead
    total = total + value  # UnboundLocalError!
    return total

# This will raise an error
# add_to_total(5)

# Correct version
def add_to_total_correct(value):
    global total
    total = total + value
    return total

print(add_to_total_correct(5))  # 5
```

### **3. Mutable Default Arguments**

```python
# Dangerous pattern
def append_item(item, container=[]):
    container.append(item)
    return container

# The same list is reused across calls
list1 = append_item(1)  # [1]
list2 = append_item(2)  # [1, 2] - unexpected!

# Safe pattern
def append_item_safe(item, container=None):
    if container is None:
        container = []
    container.append(item)
    return container

list3 = append_item_safe(1)  # [1]
list4 = append_item_safe(2)  # [2] - as expected
```

## `DEBUGGING SCOPE ISSUES`

### **Useful Functions for Scope Debugging**

```python
def debug_scope():
    local_var = "local"

    def inner():
        inner_local = "inner local"

        print("=== Scope Debug Info ===")
        print("locals():", locals())
        print("globals() keys:", list(globals().keys())[-5:])  # Last 5 keys
        print("dir():", dir())  # Names in current scope

        # Access the call stack
        import inspect
        frame = inspect.currentframe()
        print("Current function:", frame.f_code.co_name)
        print("Local variables:", list(frame.f_locals.keys()))

        outer_frame = frame.f_back
        if outer_frame:
            print("Outer function:", outer_frame.f_code.co_name)
            print("Outer locals:", list(outer_frame.f_locals.keys()))

    inner()

debug_scope()
```

### **Using `vars()` to Inspect Namespaces**

```python
class NamespaceExample:
    class_var = "class level"

    def __init__(self):
        self.instance_var = "instance level"

    def show_namespaces(self):
        local_var = "local level"

        print("Instance namespace:", vars(self))
        print("Class namespace:", vars(self.__class__))
        print("Local namespace:", locals())

obj = NamespaceExample()
obj.show_namespaces()
```

## `BEST PRACTICES`

### **1. Minimize Global Variables**

```python
# Instead of global variables
# counter = 0  # Global

# Use class or function-based approach
class Counter:
    def __init__(self):
        self._count = 0

    def increment(self):
        self._count += 1
        return self._count

    @property
    def count(self):
        return self._count

counter = Counter()
```

### **2. Use Explicit Scope Declaration**

```python
def clear_scope_example():
    global global_var  # Explicit global usage
    nonlocal_var = "enclosing"

    def inner():
        nonlocal nonlocal_var  # Explicit nonlocal usage
        local_var = "local"    # Obviously local

        # Clear intent
        nonlocal_var = "modified"
        global global_var
        global_var = "modified global"

global_var = "original global"
clear_scope_example()
```

### **3. Avoid Shadowing Built-ins**

```python
# Bad - shadows built-in
def bad_example():
    len = "not the built-in len"
    max = 100
    # Now len() and max() are not available

# Good - use descriptive names
def good_example():
    length_limit = 100
    maximum_value = 1000
    # Built-ins remain accessible
```

## `INTERVIEW INSIGHTS`

### **Key Concept: LEGB Rule Application**

```python
# Interview question: What does this print?
x = 1
def outer():
    x = 2
    def inner():
        print(x)  # What value of x?
    return inner

func = outer()
func()  # Prints 2 (from enclosing scope)

# More complex example
x = 1
def outer():
    def inner():
        print(x)  # What value of x?
    x = 2  # This line matters!
    return inner

func = outer()
func()  # Prints 2 (enclosing scope x)
```

### **Important Note: Function Definition vs Function Call**

```python
# Scope is determined at function DEFINITION time, not call time
def create_func():
    x = "enclosing"
    def inner():
        print(x)
    return inner

x = "global"
func = create_func()
x = "changed global"
func()  # Still prints "enclosing" - scope captured at definition time
```

### **Performance Tip: Local Variable Access is Fastest**

```python
import time

def global_access():
    for i in range(1000000):
        len([1, 2, 3])  # Built-in access

def local_access():
    local_len = len  # Store built-in in local variable
    for i in range(1000000):
        local_len([1, 2, 3])  # Local access

# local_access() is faster because local variable lookup is optimized
```

Understanding namespaces and scope is essential for writing maintainable Python code, avoiding common pitfalls, and understanding how Python resolves variable names. The LEGB rule provides a clear framework for understanding Python's scoping behavior.
