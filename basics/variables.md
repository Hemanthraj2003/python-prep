# VARIABLES

## What is a Variable?

A variable is a name that is a reference or pointer to a value / object stored in the memory. Once a value or object is assigned to a variable, we can refer to that object by using the variable name.

```python
x = 5
y = "Hello, World!"
```

In the above example, `x` and `y` are variables that stores values.

## Dynamic Typing

Python is a dynamically typed language, meaning we do not need to declare the type of a variable when we create one. we can see in the above example that we assigned a number to `x` and a string to `y` without any type declaration.

## Variable Naming Rules

There are some rules to follow when naming a variable in Python:

1. Variable names must start with a letter or underscore(\_).
2. Variable names cannot start with a number.
3. Variable names can only contain alpha-numeric characters and underscores.
4. Variable names are case-sensitive.
5. Variable names cannot be a reserved keyword in Python.
6. Variable names should not contain spaces.
7. We cannot use same name for different variable in the same scope.

## Assigning Values to Variables

we can assign values to variables using the assignment operator `=`.

Python supports multiple assignments in a single line.

```python
x = 5                        # Single assignment
y, z = 10, 15                # Multiple assignment
a,b,c = "A", "B", 123        # Multiple assignment with different data types
```

## Scope of Variables

The scope of a variable determines where that variable can be accessed or modified. There are two types of variable scopes in Python:

1. `Local Scope`: Variables defined inside a function are in the local scope and can only be accessed within that function.

   ```python
   def my_function():
       x = 10  # local variable
       print(x)
   my_function()  # Output: 10
   # print(x)  # Error: x is not defined outside the function
   ```

2. `Global Scope`: Variables defined outside of any function are in the global scope and can be accessed from anywhere in the code.

   ```python
   x = 20  # global variable
   def my_function():
       print(x)  # accessing global variable
   my_function()  # Output: 20
   print(x)  # Output: 20
   ```

---

[HOME: Index](../README.md)

[NEXT: DATA TYPES](data-types.md)
