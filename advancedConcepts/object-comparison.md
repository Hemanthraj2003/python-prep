# OBJECT COMPARISON METHODS

Object comparison methods allow you to define how instances of your classes are compared using operators like `==`, `<`, `>`, `<=`, `>=`, and `!=`. These methods enable sorting, equality checking, and rich comparisons.

## `BASIC COMPARISON CONCEPTS`

### **Why Implement Comparison Methods?**

Comparison methods allow you to:

- Enable sorting of your objects with `sorted()` and `list.sort()`
- Use objects in conditional statements and comparisons
- Store objects in ordered collections like `heapq`
- Make your objects behave naturally in mathematical contexts
- Enable use in sets and as dictionary keys (with `__hash__`)

### **The Rich Comparison Methods**

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
        return NotImplemented

    def __le__(self, other):
        """Less than or equal to (<=)"""
        if isinstance(other, Student):
            return self.grade <= other.grade
        return NotImplemented

    def __gt__(self, other):
        """Greater than (>)"""
        if isinstance(other, Student):
            return self.grade > other.grade
        return NotImplemented

    def __ge__(self, other):
        """Greater than or equal to (>=)"""
        if isinstance(other, Student):
            return self.grade >= other.grade
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
print(f"bob > alice: {bob > alice}")                  # True (92 > 85)

# Sorting capability
students = [bob, alice, charlie]
sorted_students = sorted(students)  # Uses __lt__ for comparison
print("Sorted by grade:")
for student in sorted_students:
    print(f"  {student}")

# Set and dictionary usage (requires __hash__ and __eq__)
student_set = {alice, bob, alice_copy}  # alice and alice_copy are equal
print(f"Unique students: {len(student_set)}")  # 2

# Dictionary keys
grades_dict = {alice: "B+", bob: "A-", charlie: "B+"}
print(f"Alice's grade: {grades_dict[alice_copy]}")  # B+ (same as alice)
```

## `COMPARISON PATTERNS AND STRATEGIES`

### **Single Attribute Comparison**

```python
class Employee:
    def __init__(self, name, salary, hire_date):
        self.name = name
        self.salary = salary
        self.hire_date = hire_date  # datetime.date object

    def __eq__(self, other):
        """Equality based on all attributes"""
        if not isinstance(other, Employee):
            return False
        return (self.name == other.name and
                self.salary == other.salary and
                self.hire_date == other.hire_date)

    def __lt__(self, other):
        """Less than based on salary (primary), then hire date"""
        if not isinstance(other, Employee):
            return NotImplemented

        if self.salary != other.salary:
            return self.salary < other.salary
        # If salaries are equal, compare by hire date (earlier = "less")
        return self.hire_date < other.hire_date

    def __le__(self, other):
        return self == other or self < other

    def __gt__(self, other):
        if not isinstance(other, Employee):
            return NotImplemented
        return not (self <= other)

    def __ge__(self, other):
        return self == other or self > other

    def __ne__(self, other):
        return not self == other

    def __hash__(self):
        return hash((self.name, self.salary, self.hire_date))

    def __str__(self):
        return f"{self.name}: ${self.salary:,} (hired {self.hire_date})"

# Usage
from datetime import date

emp1 = Employee("Alice", 75000, date(2020, 1, 15))
emp2 = Employee("Bob", 80000, date(2019, 3, 10))
emp3 = Employee("Charlie", 75000, date(2021, 6, 1))

employees = [emp1, emp2, emp3]
employees.sort()

print("Employees sorted by salary, then hire date:")
for emp in employees:
    print(f"  {emp}")

# Output:
# Alice: $75,000 (hired 2020-01-15)
# Charlie: $75,000 (hired 2021-06-01)
# Bob: $80,000 (hired 2019-03-10)
```

### **Multi-Criteria Comparison with Priority**

```python
from functools import total_ordering

@total_ordering
class Product:
    """
    @total_ordering decorator automatically generates all comparison methods
    from __eq__ and one ordering method (__lt__ in this case)
    """
    def __init__(self, name, price, rating, category):
        self.name = name
        self.price = price
        self.rating = rating  # 1-5 stars
        self.category = category

    def __eq__(self, other):
        if not isinstance(other, Product):
            return False
        return (self.name == other.name and
                self.price == other.price and
                self.rating == other.rating)

    def __lt__(self, other):
        """
        Comparison priority:
        1. Higher rating is "greater" (better)
        2. Lower price is "greater" (better value)
        3. Alphabetical name order
        """
        if not isinstance(other, Product):
            return NotImplemented

        # Higher rating wins (reverse comparison)
        if self.rating != other.rating:
            return self.rating < other.rating  # Lower rating = "less than"

        # If ratings equal, lower price wins (normal comparison)
        if self.price != other.price:
            return self.price > other.price  # Higher price = "less than"

        # If rating and price equal, alphabetical order
        return self.name > other.name

    def __hash__(self):
        return hash((self.name, self.price, self.rating))

    def __str__(self):
        stars = "★" * self.rating + "☆" * (5 - self.rating)
        return f"{self.name} - ${self.price} {stars}"

# Usage
products = [
    Product("Laptop A", 1200, 4, "Electronics"),
    Product("Laptop B", 1000, 4, "Electronics"),  # Same rating, lower price
    Product("Laptop C", 1200, 5, "Electronics"),  # Same price, higher rating
    Product("Phone A", 800, 3, "Electronics"),
    Product("Tablet A", 600, 4, "Electronics"),
]

# Sort products (best to worst based on our criteria)
products.sort(reverse=True)  # reverse=True to get best products first

print("Products ranked by rating, then price, then name:")
for i, product in enumerate(products, 1):
    print(f"{i}. {product}")

# Output will show products sorted by rating (high to low),
# then price (low to high), then name (alphabetical)
```

### **Comparison with Type Flexibility**

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    def __eq__(self, other):
        """Compare with other Temperature objects or numeric values"""
        if isinstance(other, Temperature):
            return self.celsius == other.celsius
        elif isinstance(other, (int, float)):
            return self.celsius == other
        return False

    def __lt__(self, other):
        """Less than comparison with type flexibility"""
        if isinstance(other, Temperature):
            return self.celsius < other.celsius
        elif isinstance(other, (int, float)):
            return self.celsius < other
        return NotImplemented

    def __le__(self, other):
        return self == other or self < other

    def __gt__(self, other):
        if isinstance(other, Temperature):
            return self.celsius > other.celsius
        elif isinstance(other, (int, float)):
            return self.celsius > other
        return NotImplemented

    def __ge__(self, other):
        return self == other or self > other

    def __ne__(self, other):
        return not self == other

    def __hash__(self):
        return hash(self.celsius)

    @property
    def fahrenheit(self):
        return self.celsius * 9/5 + 32

    def __str__(self):
        return f"{self.celsius}°C ({self.fahrenheit:.1f}°F)"

# Usage with mixed types
temp1 = Temperature(25)
temp2 = Temperature(30)

print(f"temp1 == 25: {temp1 == 25}")           # True
print(f"temp1 < temp2: {temp1 < temp2}")       # True
print(f"temp1 < 30: {temp1 < 30}")             # True
print(f"temp2 > 25: {temp2 > 25}")             # True

# Sorting mixed types
temperatures = [Temperature(0), 15, Temperature(25), 10, Temperature(35)]

# This works because Temperature can compare with numbers
mixed_sorted = sorted(temperatures)
print("Mixed temperatures sorted:")
for temp in mixed_sorted:
    if isinstance(temp, Temperature):
        print(f"  {temp}")
    else:
        print(f"  {temp}°C")
```

## `ADVANCED COMPARISON PATTERNS`

### **Custom Comparison Key**

```python
class Student:
    def __init__(self, name, math_score, english_score, science_score):
        self.name = name
        self.math_score = math_score
        self.english_score = english_score
        self.science_score = science_score

    @property
    def total_score(self):
        return self.math_score + self.english_score + self.science_score

    @property
    def average_score(self):
        return self.total_score / 3

    def comparison_key(self, criteria="total"):
        """Return comparison key based on criteria"""
        if criteria == "total":
            return self.total_score
        elif criteria == "average":
            return self.average_score
        elif criteria == "math":
            return self.math_score
        elif criteria == "english":
            return self.english_score
        elif criteria == "science":
            return self.science_score
        elif criteria == "name":
            return self.name
        else:
            raise ValueError(f"Unknown criteria: {criteria}")

    def __eq__(self, other):
        if not isinstance(other, Student):
            return False
        return self.name == other.name

    def __lt__(self, other):
        """Default comparison by total score"""
        if not isinstance(other, Student):
            return NotImplemented
        return self.total_score < other.total_score

    def __hash__(self):
        return hash(self.name)

    def __str__(self):
        return f"{self.name}: Math={self.math_score}, English={self.english_score}, Science={self.science_score} (Total: {self.total_score})"

# Advanced sorting with different criteria
students = [
    Student("Alice", 90, 85, 88),
    Student("Bob", 88, 92, 86),
    Student("Charlie", 85, 88, 92),
    Student("Diana", 92, 86, 84),
]

print("Original order:")
for student in students:
    print(f"  {student}")

# Sort by different criteria using key function
print("\nSorted by total score:")
by_total = sorted(students, key=lambda s: s.comparison_key("total"), reverse=True)
for student in by_total:
    print(f"  {student}")

print("\nSorted by math score:")
by_math = sorted(students, key=lambda s: s.comparison_key("math"), reverse=True)
for student in by_math:
    print(f"  {student}")

print("\nSorted by name:")
by_name = sorted(students, key=lambda s: s.comparison_key("name"))
for student in by_name:
    print(f"  {student}")
```

### **Comparison with Tolerance (Floating Point)**

```python
class FloatValue:
    def __init__(self, value, tolerance=1e-9):
        self.value = float(value)
        self.tolerance = tolerance

    def __eq__(self, other):
        """Equality with tolerance for floating point comparison"""
        if isinstance(other, FloatValue):
            return abs(self.value - other.value) <= max(self.tolerance, other.tolerance)
        elif isinstance(other, (int, float)):
            return abs(self.value - other) <= self.tolerance
        return False

    def __lt__(self, other):
        """Less than with tolerance"""
        if isinstance(other, FloatValue):
            tolerance = max(self.tolerance, other.tolerance)
            # Not equal and self.value is smaller
            return not abs(self.value - other.value) <= tolerance and self.value < other.value
        elif isinstance(other, (int, float)):
            return not abs(self.value - other) <= self.tolerance and self.value < other
        return NotImplemented

    def __le__(self, other):
        """Less than or equal with tolerance"""
        return self == other or self < other

    def __gt__(self, other):
        """Greater than with tolerance"""
        return not (self <= other)

    def __ge__(self, other):
        """Greater than or equal with tolerance"""
        return self == other or self > other

    def __ne__(self, other):
        return not self == other

    def __hash__(self):
        # Hash based on rounded value to maintain consistency with equality
        return hash(round(self.value / self.tolerance))

    def __str__(self):
        return f"{self.value} (±{self.tolerance})"

# Usage with floating point precision issues
a = FloatValue(0.1 + 0.2)  # This is actually 0.30000000000000004
b = FloatValue(0.3)
c = FloatValue(0.3000001, tolerance=1e-6)

print(f"a = {a}")
print(f"b = {b}")
print(f"c = {c}")

print(f"a == b: {a == b}")  # True (within tolerance)
print(f"a == 0.3: {a == 0.3}")  # True (within tolerance)
print(f"b == c: {b == c}")  # True (within c's larger tolerance)

# Sorting with tolerance
values = [
    FloatValue(1.0000001),
    FloatValue(1.0),
    FloatValue(0.9999999),
    FloatValue(1.5),
    FloatValue(0.5)
]

sorted_values = sorted(values)
print("\nSorted floating point values:")
for val in sorted_values:
    print(f"  {val}")
```

## `COMPARISON WITH INHERITANCE`

```python
class Shape:
    def __init__(self, name):
        self.name = name

    def area(self):
        raise NotImplementedError("Subclasses must implement area()")

    def __eq__(self, other):
        """Base equality: same type and same area"""
        return (type(self) == type(other) and
                abs(self.area() - other.area()) < 1e-9)

    def __lt__(self, other):
        """Compare by area"""
        if isinstance(other, Shape):
            return self.area() < other.area()
        return NotImplemented

    def __hash__(self):
        return hash((type(self).__name__, round(self.area(), 9)))

    def __str__(self):
        return f"{self.name} (area: {self.area():.2f})"

class Rectangle(Shape):
    def __init__(self, width, height):
        super().__init__("Rectangle")
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def __eq__(self, other):
        """Rectangle-specific equality: same dimensions"""
        if isinstance(other, Rectangle):
            return (self.width == other.width and self.height == other.height) or \
                   (self.width == other.height and self.height == other.width)
        # Fall back to base class comparison (by area)
        return super().__eq__(other)

class Circle(Shape):
    def __init__(self, radius):
        super().__init__("Circle")
        self.radius = radius

    def area(self):
        import math
        return math.pi * self.radius ** 2

    def __eq__(self, other):
        """Circle-specific equality: same radius"""
        if isinstance(other, Circle):
            return abs(self.radius - other.radius) < 1e-9
        # Fall back to base class comparison (by area)
        return super().__eq__(other)

# Usage with inheritance
shapes = [
    Rectangle(3, 4),      # area = 12
    Circle(2),            # area ≈ 12.57
    Rectangle(2, 6),      # area = 12
    Circle(1.5),          # area ≈ 7.07
    Rectangle(4, 3),      # area = 12 (same as first, but rotated)
]

print("Original shapes:")
for shape in shapes:
    print(f"  {shape}")

# Sort by area
shapes.sort()
print("\nSorted by area:")
for shape in shapes:
    print(f"  {shape}")

# Test equality
rect1 = Rectangle(3, 4)
rect2 = Rectangle(4, 3)  # Rotated version
rect3 = Rectangle(2, 6)  # Different rectangle, same area

print(f"\nrect1 == rect2 (rotated): {rect1 == rect2}")  # True
print(f"rect1 == rect3 (same area): {rect1 == rect3}")  # False (different dimensions)

# Cross-type comparison by area
circle = Circle(1.954)  # area ≈ 12
print(f"rect1 area: {rect1.area()}")
print(f"circle area: {circle.area():.2f}")
print(f"rect1 == circle (by area): {rect1 == circle}")  # True (same area)
```

## `BEST PRACTICES AND COMMON PITFALLS`

### **1. Consistent Comparison Logic**

```python
class BadExample:
    """DON'T DO THIS - Inconsistent comparison logic"""
    def __init__(self, value):
        self.value = value

    def __eq__(self, other):
        return self.value == other.value  # Compare by value

    def __lt__(self, other):
        return len(str(self.value)) < len(str(other.value))  # Compare by string length!

    # This creates inconsistent behavior where a == b but a < b might be True

class GoodExample:
    """DO THIS - Consistent comparison logic"""
    def __init__(self, value):
        self.value = value

    def __eq__(self, other):
        if not isinstance(other, GoodExample):
            return False
        return self.value == other.value

    def __lt__(self, other):
        if not isinstance(other, GoodExample):
            return NotImplemented
        return self.value < other.value  # Same criteria as equality

    def __hash__(self):
        return hash(self.value)  # Hash based on same criteria as equality
```

### **2. Handling Type Compatibility**

```python
class VersionNumber:
    def __init__(self, version_string):
        # Parse version like "1.2.3" into tuple (1, 2, 3)
        self.parts = tuple(int(x) for x in version_string.split('.'))
        self.version_string = version_string

    def __eq__(self, other):
        if isinstance(other, VersionNumber):
            return self.parts == other.parts
        elif isinstance(other, str):
            try:
                other_version = VersionNumber(other)
                return self.parts == other_version.parts
            except ValueError:
                return False
        return False

    def __lt__(self, other):
        if isinstance(other, VersionNumber):
            return self.parts < other.parts
        elif isinstance(other, str):
            try:
                other_version = VersionNumber(other)
                return self.parts < other_version.parts
            except ValueError:
                return NotImplemented
        return NotImplemented

    def __str__(self):
        return self.version_string

# Usage
v1 = VersionNumber("1.2.3")
v2 = VersionNumber("1.2.10")
v3 = VersionNumber("2.0.0")

print(f"v1 < v2: {v1 < v2}")          # True
print(f"v2 < v3: {v2 < v3}")          # True
print(f"v1 == '1.2.3': {v1 == '1.2.3'}")  # True
print(f"v1 < '1.3.0': {v1 < '1.3.0'}")    # True

versions = [v3, v1, v2, VersionNumber("1.0.0")]
versions.sort()
print("Sorted versions:", [str(v) for v in versions])
```

## `INTERVIEW INSIGHTS`

### **Key Concept: Comparison Method Relationships**

```python
# Python automatically provides some relationships:
# If you implement __eq__ and __lt__, you get:
# - __ne__ (opposite of __eq__)
# - __gt__ (opposite of __lt__ with swapped arguments)
# - __le__ (__eq__ or __lt__)
# - __ge__ (__eq__ or __gt__)

# But it's better to be explicit or use @total_ordering

from functools import total_ordering

@total_ordering
class InterviewExample:
    def __init__(self, value):
        self.value = value

    def __eq__(self, other):
        return isinstance(other, InterviewExample) and self.value == other.value

    def __lt__(self, other):
        if isinstance(other, InterviewExample):
            return self.value < other.value
        return NotImplemented

    # @total_ordering automatically generates __le__, __gt__, __ge__, __ne__
```

### **Important Note: Hash Consistency**

```python
class HashableExample:
    def __init__(self, value):
        self.value = value

    def __eq__(self, other):
        return isinstance(other, HashableExample) and self.value == other.value

    def __hash__(self):
        # CRITICAL: If a == b, then hash(a) must equal hash(b)
        return hash(self.value)

# This allows use in sets and as dictionary keys
obj1 = HashableExample(5)
obj2 = HashableExample(5)

print(f"obj1 == obj2: {obj1 == obj2}")        # True
print(f"hash(obj1) == hash(obj2): {hash(obj1) == hash(obj2)}")  # True

# Can be used in sets
s = {obj1, obj2}  # Only one object in set because they're equal
print(f"Set size: {len(s)}")  # 1
```

### **Performance Tip: Comparison Shortcuts**

```python
class OptimizedComparison:
    def __init__(self, data):
        self.data = data
        self._hash = None

    def __eq__(self, other):
        if self is other:  # Same object reference
            return True
        if not isinstance(other, OptimizedComparison):
            return False

        # Quick check: if hashes are different, objects can't be equal
        if hash(self) != hash(other):
            return False

        # Expensive comparison only if necessary
        return self.data == other.data

    def __hash__(self):
        if self._hash is None:
            self._hash = hash(tuple(self.data) if isinstance(self.data, list) else self.data)
        return self._hash

    def __lt__(self, other):
        if not isinstance(other, OptimizedComparison):
            return NotImplemented
        return self.data < other.data
```

Object comparison methods are essential for creating objects that integrate naturally with Python's built-in functions and operations. They enable sorting, equality testing, and use in ordered collections while providing intuitive behavior for users of your classes.
