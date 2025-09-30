# COMPREHENSIVE DUNDER METHODS (MAGIC METHODS)

Dunder methods (double underscore methods) are special methods that define how objects behave with built-in operations and functions. They enable operator overloading and integration with Python's built-in functions.

## `OVERVIEW OF DUNDER METHODS`

Dunder methods allow you to define how your objects interact with:

- Arithmetic operators (`+`, `-`, `*`, `/`, etc.)
- Comparison operators (`==`, `<`, `>`, etc.)
- Container operations (`len()`, `[]`, `in`, etc.)
- String representation (`str()`, `repr()`)
- Boolean context (`bool()`, `if obj:`)
- And much more!

## `ARITHMETIC OPERATORS`

### **Basic Arithmetic Operations**

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        """Addition: vector1 + vector2"""
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        elif isinstance(other, (int, float)):
            return Vector(self.x + other, self.y + other)
        return NotImplemented

    def __radd__(self, other):
        """Reverse addition: 5 + vector"""
        return self.__add__(other)

    def __sub__(self, other):
        """Subtraction: vector1 - vector2"""
        if isinstance(other, Vector):
            return Vector(self.x - other.x, self.y - other.y)
        elif isinstance(other, (int, float)):
            return Vector(self.x - other, self.y - other)
        return NotImplemented

    def __rsub__(self, other):
        """Reverse subtraction: 5 - vector"""
        if isinstance(other, (int, float)):
            return Vector(other - self.x, other - self.y)
        return NotImplemented

    def __mul__(self, other):
        """Multiplication: vector * scalar or vector * vector (dot product)"""
        if isinstance(other, (int, float)):
            return Vector(self.x * other, self.y * other)
        elif isinstance(other, Vector):
            # Dot product
            return self.x * other.x + self.y * other.y
        return NotImplemented

    def __rmul__(self, other):
        """Reverse multiplication: scalar * vector"""
        return self.__mul__(other)

    def __truediv__(self, other):
        """Division: vector / scalar"""
        if isinstance(other, (int, float)):
            if other == 0:
                raise ZeroDivisionError("Cannot divide by zero")
            return Vector(self.x / other, self.y / other)
        return NotImplemented

    def __floordiv__(self, other):
        """Floor division: vector // scalar"""
        if isinstance(other, (int, float)):
            if other == 0:
                raise ZeroDivisionError("Cannot divide by zero")
            return Vector(self.x // other, self.y // other)
        return NotImplemented

    def __mod__(self, other):
        """Modulo: vector % scalar"""
        if isinstance(other, (int, float)):
            return Vector(self.x % other, self.y % other)
        return NotImplemented

    def __pow__(self, other):
        """Power: vector ** power"""
        if isinstance(other, (int, float)):
            return Vector(self.x ** other, self.y ** other)
        return NotImplemented

    def __neg__(self):
        """Unary minus: -vector"""
        return Vector(-self.x, -self.y)

    def __pos__(self):
        """Unary plus: +vector"""
        return Vector(self.x, self.y)

    def __abs__(self):
        """Absolute value: abs(vector) - returns magnitude"""
        return (self.x**2 + self.y**2)**0.5

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

# Usage examples
v1 = Vector(3, 4)
v2 = Vector(1, 2)

print(f"v1: {v1}")                    # Vector(3, 4)
print(f"v2: {v2}")                    # Vector(1, 2)
print(f"v1 + v2: {v1 + v2}")          # Vector(4, 6)
print(f"v1 - v2: {v1 - v2}")          # Vector(2, 2)
print(f"v1 * 2: {v1 * 2}")            # Vector(6, 8)
print(f"2 * v1: {2 * v1}")            # Vector(6, 8)
print(f"v1 * v2 (dot): {v1 * v2}")    # 11 (dot product)
print(f"v1 / 2: {v1 / 2}")            # Vector(1.5, 2.0)
print(f"v1 // 2: {v1 // 2}")          # Vector(1.0, 2.0)
print(f"v1 % 3: {v1 % 3}")            # Vector(0, 1)
print(f"v1 ** 2: {v1 ** 2}")          # Vector(9, 16)
print(f"-v1: {-v1}")                  # Vector(-3, -4)
print(f"+v1: {+v1}")                  # Vector(3, 4)
print(f"abs(v1): {abs(v1)}")          # 5.0
```

### **In-Place Arithmetic Operations**

```python
class MutableVector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __iadd__(self, other):
        """In-place addition: vector += other"""
        if isinstance(other, MutableVector):
            self.x += other.x
            self.y += other.y
        elif isinstance(other, (int, float)):
            self.x += other
            self.y += other
        else:
            return NotImplemented
        return self

    def __isub__(self, other):
        """In-place subtraction: vector -= other"""
        if isinstance(other, MutableVector):
            self.x -= other.x
            self.y -= other.y
        elif isinstance(other, (int, float)):
            self.x -= other
            self.y -= other
        else:
            return NotImplemented
        return self

    def __imul__(self, other):
        """In-place multiplication: vector *= scalar"""
        if isinstance(other, (int, float)):
            self.x *= other
            self.y *= other
        else:
            return NotImplemented
        return self

    def __itruediv__(self, other):
        """In-place division: vector /= scalar"""
        if isinstance(other, (int, float)):
            if other == 0:
                raise ZeroDivisionError("Cannot divide by zero")
            self.x /= other
            self.y /= other
        else:
            return NotImplemented
        return self

    def __str__(self):
        return f"MutableVector({self.x}, {self.y})"

# Usage examples
mv = MutableVector(10, 20)
print(f"Original: {mv}")      # MutableVector(10, 20)

mv += MutableVector(5, 3)
print(f"After += vector: {mv}") # MutableVector(15, 23)

mv *= 2
print(f"After *= 2: {mv}")      # MutableVector(30, 46)

mv /= 5
print(f"After /= 5: {mv}")      # MutableVector(6.0, 9.2)
```

## `COMPARISON OPERATORS (RICH COMPARISON)`

```python
class Student:
    def __init__(self, name, grade, student_id):
        self.name = name
        self.grade = grade
        self.student_id = student_id

    def __eq__(self, other):
        """Equal to (==) - based on student ID"""
        if isinstance(other, Student):
            return self.student_id == other.student_id
        return False

    def __ne__(self, other):
        """Not equal to (!=) - usually derived from __eq__"""
        return not self.__eq__(other)

    def __lt__(self, other):
        """Less than (<) - based on grade"""
        if isinstance(other, Student):
            return self.grade < other.grade
        elif isinstance(other, (int, float)):
            return self.grade < other
        return NotImplemented

    def __le__(self, other):
        """Less than or equal to (<=)"""
        if isinstance(other, Student):
            return self.grade <= other.grade
        elif isinstance(other, (int, float)):
            return self.grade <= other
        return NotImplemented

    def __gt__(self, other):
        """Greater than (>)"""
        if isinstance(other, Student):
            return self.grade > other.grade
        elif isinstance(other, (int, float)):
            return self.grade > other
        return NotImplemented

    def __ge__(self, other):
        """Greater than or equal to (>=)"""
        if isinstance(other, Student):
            return self.grade >= other.grade
        elif isinstance(other, (int, float)):
            return self.grade >= other
        return NotImplemented

    def __hash__(self):
        """Make object hashable (for use in sets/dict keys)"""
        return hash(self.student_id)

    def __str__(self):
        return f"{self.name} (Grade: {self.grade}, ID: {self.student_id})"

# Usage examples
alice = Student("Alice", 85, "S001")
bob = Student("Bob", 92, "S002")
charlie = Student("Charlie", 85, "S003")
alice_copy = Student("Alice", 90, "S001")  # Same ID, different grade

print(f"alice == alice_copy: {alice == alice_copy}")  # True (same ID)
print(f"alice == charlie: {alice == charlie}")        # False (different ID)
print(f"alice < bob: {alice < bob}")                  # True (85 < 92)
print(f"alice >= charlie: {alice >= charlie}")        # True (85 >= 85)
print(f"alice > 80: {alice > 80}")                    # True (85 > 80)

# Can now sort students
students = [bob, alice, charlie]
sorted_students = sorted(students)  # Uses __lt__ for comparison
print("Sorted by grade:")
for student in sorted_students:
    print(f"  {student}")

# Can use in sets and as dict keys (requires __hash__ and __eq__)
student_set = {alice, bob, alice_copy}  # alice and alice_copy are equal
print(f"Unique students: {len(student_set)}")  # 2 (alice and alice_copy count as one)

# Use as dictionary keys
grades_dict = {alice: "B+", bob: "A-", charlie: "B+"}
print(f"Alice's grade: {grades_dict[alice_copy]}")  # B+ (same as alice)
```

## `CONTAINER EMULATION METHODS`

```python
class Matrix:
    def __init__(self, rows, cols, default_value=0):
        self.rows = rows
        self.cols = cols
        self._data = [[default_value for _ in range(cols)] for _ in range(rows)]

    def __len__(self):
        """Return number of elements (rows * cols)"""
        return self.rows * self.cols

    def __bool__(self):
        """False if all elements are falsy"""
        for row in self._data:
            for element in row:
                if element:
                    return True
        return False

    def __getitem__(self, key):
        """Support for matrix[row, col] or matrix[row]"""
        if isinstance(key, tuple):
            row, col = key
            self._validate_indices(row, col)
            return self._data[row][col]
        else:
            self._validate_row_index(key)
            return self._data[key]  # Return entire row

    def __setitem__(self, key, value):
        """Support for matrix[row, col] = value or matrix[row] = [values]"""
        if isinstance(key, tuple):
            row, col = key
            self._validate_indices(row, col)
            self._data[row][col] = value
        else:
            self._validate_row_index(key)
            if not isinstance(value, (list, tuple)) or len(value) != self.cols:
                raise ValueError(f"Row must be a list/tuple of length {self.cols}")
            self._data[key] = list(value)

    def __delitem__(self, key):
        """Support for del matrix[row] (removes row)"""
        if isinstance(key, tuple):
            raise TypeError("Cannot delete individual elements, only rows")
        self._validate_row_index(key)
        del self._data[key]
        self.rows -= 1

    def __contains__(self, item):
        """Support for 'item in matrix'"""
        for row in self._data:
            if item in row:
                return True
        return False

    def __iter__(self):
        """Support for iteration over rows"""
        return iter(self._data)

    def __reversed__(self):
        """Support for reversed(matrix)"""
        return reversed(self._data)

    def _validate_indices(self, row, col):
        """Validate row and column indices"""
        if not (0 <= row < self.rows):
            raise IndexError(f"Row index {row} out of range [0, {self.rows})")
        if not (0 <= col < self.cols):
            raise IndexError(f"Column index {col} out of range [0, {self.cols})")

    def _validate_row_index(self, row):
        """Validate row index"""
        if not (0 <= row < self.rows):
            raise IndexError(f"Row index {row} out of range [0, {self.rows})")

    def __str__(self):
        lines = []
        for row in self._data:
            line = " ".join(f"{elem:6.2f}" for elem in row)
            lines.append(f"[{line}]")
        return "\n".join(lines)

    def __repr__(self):
        return f"Matrix({self.rows}, {self.cols})"

# Usage examples
matrix = Matrix(3, 3)

# Setting values
matrix[0, 0] = 1
matrix[0, 1] = 2
matrix[0, 2] = 3
matrix[1] = [4, 5, 6]  # Set entire row
matrix[2] = [7, 8, 9]

print("Matrix:")
print(matrix)
print(f"Length: {len(matrix)}")           # 9 (3x3)
print(f"Boolean: {bool(matrix)}")         # True (has non-zero elements)
print(f"Element [1,1]: {matrix[1, 1]}")   # 5
print(f"Row 0: {matrix[0]}")              # [1, 2, 3]
print(f"Contains 5: {5 in matrix}")       # True
print(f"Contains 10: {10 in matrix}")     # False

# Iteration
print("Rows:")
for i, row in enumerate(matrix):
    print(f"  Row {i}: {row}")

# Reverse iteration
print("Reversed rows:")
for i, row in enumerate(reversed(matrix)):
    print(f"  Row {i}: {row}")

# Boolean context
empty_matrix = Matrix(2, 2, 0)
print(f"Empty matrix boolean: {bool(empty_matrix)}")  # False

if matrix:
    print("Matrix has data")
```

## `CALLABLE OBJECTS`

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, value):
        """Make the object callable like a function"""
        return value * self.factor

    def __repr__(self):
        return f"Multiplier({self.factor})"

# Usage examples
double = Multiplier(2)
triple = Multiplier(3)

print(f"double: {double}")           # Multiplier(2)
print(f"double(5): {double(5)}")     # 10
print(f"triple(4): {triple(4)}")     # 12

# Can be used as a function
numbers = [1, 2, 3, 4, 5]
doubled = list(map(double, numbers))
print(f"Doubled: {doubled}")         # [2, 4, 6, 8, 10]

# More complex callable example
class Counter:
    def __init__(self, start=0, step=1):
        self.current = start
        self.step = step

    def __call__(self):
        """Return current value and increment"""
        result = self.current
        self.current += self.step
        return result

    def reset(self, start=0):
        self.current = start

    def __repr__(self):
        return f"Counter(current={self.current}, step={self.step})"

# Usage
counter = Counter(0, 2)
print(f"Counter: {counter}")      # Counter(current=0, step=2)
print(f"Next: {counter()}")       # 0
print(f"Next: {counter()}")       # 2
print(f"Next: {counter()}")       # 4
print(f"Counter: {counter}")      # Counter(current=6, step=2)
```

## `CONTEXT MANAGER METHODS`

```python
class FileManager:
    def __init__(self, filename, mode='r'):
        self.filename = filename
        self.mode = mode
        self.file = None

    def __enter__(self):
        """Enter the context manager"""
        print(f"Opening file: {self.filename}")
        self.file = open(self.filename, self.mode)
        return self.file

    def __exit__(self, exc_type, exc_value, traceback):
        """Exit the context manager"""
        if self.file:
            print(f"Closing file: {self.filename}")
            self.file.close()

        # Handle exceptions
        if exc_type is not None:
            print(f"Exception occurred: {exc_type.__name__}: {exc_value}")
            return False  # Don't suppress the exception
        return True

# Usage with 'with' statement
try:
    with FileManager("test.txt", "w") as f:
        f.write("Hello, World!")
        f.write("\nThis is a test file.")
    # File is automatically closed here
except FileNotFoundError as e:
    print(f"File error: {e}")

# More complex context manager
class DatabaseConnection:
    def __init__(self, db_name):
        self.db_name = db_name
        self.connection = None
        self.transaction_active = False

    def __enter__(self):
        print(f"Connecting to database: {self.db_name}")
        # Simulate database connection
        self.connection = f"Connection to {self.db_name}"
        self.transaction_active = True
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        if exc_type is not None:
            print(f"Error occurred, rolling back transaction: {exc_value}")
            self.rollback()
        else:
            print("Committing transaction")
            self.commit()

        print(f"Closing connection to {self.db_name}")
        self.connection = None
        return False  # Don't suppress exceptions

    def execute(self, query):
        if not self.connection:
            raise RuntimeError("No database connection")
        print(f"Executing: {query}")
        return f"Result of: {query}"

    def commit(self):
        if self.transaction_active:
            print("Transaction committed")
            self.transaction_active = False

    def rollback(self):
        if self.transaction_active:
            print("Transaction rolled back")
            self.transaction_active = False

# Usage
try:
    with DatabaseConnection("mydb") as db:
        result1 = db.execute("SELECT * FROM users")
        result2 = db.execute("INSERT INTO logs VALUES ('action')")
        # If we reach here, transaction will be committed
except Exception as e:
    print(f"Database operation failed: {e}")
```

## `COPY AND DEEPCOPY METHODS`

```python
import copy

class Node:
    def __init__(self, value, children=None):
        self.value = value
        self.children = children or []

    def __copy__(self):
        """Shallow copy implementation"""
        print(f"Shallow copying node with value: {self.value}")
        # Create new node but share children list
        return Node(self.value, self.children)

    def __deepcopy__(self, memo):
        """Deep copy implementation"""
        print(f"Deep copying node with value: {self.value}")
        # Create new node with deep copied children
        new_children = copy.deepcopy(self.children, memo)
        return Node(self.value, new_children)

    def add_child(self, child):
        self.children.append(child)

    def __str__(self):
        return f"Node({self.value}, children={len(self.children)})"

    def __repr__(self):
        return f"Node({self.value}, {self.children})"

# Create a tree structure
root = Node("root")
child1 = Node("child1")
child2 = Node("child2")
grandchild = Node("grandchild")

child1.add_child(grandchild)
root.add_child(child1)
root.add_child(child2)

print("Original tree:")
print(f"Root: {root}")
print(f"Child1: {child1}")

# Shallow copy
print("\nShallow copy:")
root_shallow = copy.copy(root)
print(f"Shallow copy: {root_shallow}")
print(f"Same children list: {root.children is root_shallow.children}")  # True

# Deep copy
print("\nDeep copy:")
root_deep = copy.deepcopy(root)
print(f"Deep copy: {root_deep}")
print(f"Same children list: {root.children is root_deep.children}")     # False
```

## `ATTRIBUTE ACCESS METHODS`

```python
class DynamicAttributes:
    def __init__(self):
        self._data = {}

    def __getattr__(self, name):
        """Called when attribute is not found through normal lookup"""
        print(f"Getting attribute: {name}")
        if name in self._data:
            return self._data[name]
        raise AttributeError(f"'{type(self).__name__}' has no attribute '{name}'")

    def __setattr__(self, name, value):
        """Called for all attribute assignments"""
        print(f"Setting attribute: {name} = {value}")
        if name.startswith('_'):
            # Private attributes go to __dict__
            super().__setattr__(name, value)
        else:
            # Public attributes go to _data
            if not hasattr(self, '_data'):
                super().__setattr__('_data', {})
            self._data[name] = value

    def __delattr__(self, name):
        """Called when attribute is deleted"""
        print(f"Deleting attribute: {name}")
        if name in self._data:
            del self._data[name]
        else:
            super().__delattr__(name)

    def __getattribute__(self, name):
        """Called for ALL attribute access (be careful!)"""
        # Only use this for special cases - can cause infinite recursion
        if name == 'special_attr':
            print("Accessing special attribute")
        return super().__getattribute__(name)

# Usage examples
obj = DynamicAttributes()

# Setting attributes
obj.name = "Test"           # Setting attribute: name = Test
obj.value = 42             # Setting attribute: value = 42

# Getting attributes
print(f"Name: {obj.name}")  # Getting attribute: name, then: Name: Test
print(f"Value: {obj.value}")# Getting attribute: value, then: Value: 42

# Deleting attributes
del obj.name               # Deleting attribute: name

# Trying to access deleted attribute
try:
    print(obj.name)        # Getting attribute: name, then AttributeError
except AttributeError as e:
    print(f"Error: {e}")
```

## `NUMERIC CONVERSION METHODS`

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    def __int__(self):
        """Convert to integer"""
        return int(self.celsius)

    def __float__(self):
        """Convert to float"""
        return float(self.celsius)

    def __complex__(self):
        """Convert to complex number"""
        return complex(self.celsius)

    def __round__(self, ndigits=None):
        """Support for round() function"""
        if ndigits is None:
            return Temperature(round(self.celsius))
        return Temperature(round(self.celsius, ndigits))

    def __floor__(self):
        """Support for math.floor()"""
        import math
        return Temperature(math.floor(self.celsius))

    def __ceil__(self):
        """Support for math.ceil()"""
        import math
        return Temperature(math.ceil(self.celsius))

    def __trunc__(self):
        """Support for math.trunc()"""
        import math
        return Temperature(math.trunc(self.celsius))

    def __str__(self):
        return f"{self.celsius}°C"

# Usage examples
temp = Temperature(25.7)

print(f"Original: {temp}")           # 25.7°C
print(f"int(): {int(temp)}")         # 25
print(f"float(): {float(temp)}")     # 25.7
print(f"complex(): {complex(temp)}") # (25.7+0j)
print(f"round(): {round(temp)}")     # 26°C
print(f"round(1): {round(temp, 1)}") # 25.7°C

import math
print(f"floor(): {math.floor(temp)}")   # 25°C
print(f"ceil(): {math.ceil(temp)}")     # 26°C
print(f"trunc(): {math.trunc(temp)}")   # 25°C
```

## `INTERVIEW INSIGHTS`

### **Key Concept: Method Resolution and NotImplemented**

```python
class InterviewExample:
    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        if isinstance(other, InterviewExample):
            return InterviewExample(self.value + other.value)
        elif isinstance(other, (int, float)):
            return InterviewExample(self.value + other)
        # Return NotImplemented, not raise NotImplementedError
        return NotImplemented

    def __radd__(self, other):
        # This handles cases like: 5 + obj
        return self.__add__(other)

    def __str__(self):
        return f"Example({self.value})"

# When Python sees: obj1 + obj2
# 1. Tries obj1.__add__(obj2)
# 2. If that returns NotImplemented, tries obj2.__radd__(obj1)
# 3. If both return NotImplemented, raises TypeError

obj = InterviewExample(10)
print(f"obj + 5: {obj + 5}")        # Works: Example(15)
print(f"5 + obj: {5 + obj}")        # Works: Example(15) (uses __radd__)

# With unsupported type
try:
    result = obj + "string"          # Both __add__ and __radd__ return NotImplemented
except TypeError as e:
    print(f"Error: {e}")
```

### **Important Note: Performance Considerations**

```python
class PerformanceAware:
    def __init__(self, data):
        self.data = data

    def __len__(self):
        """Keep __len__ fast - it's called frequently"""
        return len(self.data)

    def __bool__(self):
        """Faster than using __len__ for boolean context"""
        # Don't implement as: return len(self.data) > 0
        # That would call __len__ which might be expensive
        try:
            next(iter(self.data))
            return True
        except StopIteration:
            return False

    def __contains__(self, item):
        """Implement efficiently for your data structure"""
        # For lists/tuples: O(n) linear search is default
        # For sets/dicts: O(1) lookup is automatic
        # For custom structures: implement the most efficient approach
        return item in self.data

# The default behavior if not implemented:
# __bool__: calls __len__, returns False if 0, True otherwise
# __len__: raises TypeError if not implemented
# __contains__: iterates through __iter__ if available
```

### **Memory Management with `__slots__`**

```python
class WithoutSlots:
    def __init__(self, x, y):
        self.x = x
        self.y = y

class WithSlots:
    __slots__ = ['x', 'y']  # Restrict attributes, save memory

    def __init__(self, x, y):
        self.x = x
        self.y = y

# Compare memory usage
import sys

obj1 = WithoutSlots(1, 2)
obj2 = WithSlots(1, 2)

print(f"Without slots: {sys.getsizeof(obj1.__dict__)} bytes")
print(f"With slots: {sys.getsizeof(obj2)} bytes")

# Note: With __slots__, you lose some flexibility:
# - No __dict__ (can't add attributes dynamically)
# - Weak references need __weakref__ in slots
# - Inheritance is more complex
```

Dunder methods are the foundation of Python's object model, enabling seamless integration with built-in operations and functions. They make your custom objects behave like native Python types.
