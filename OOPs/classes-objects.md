# CLASSES AND OBJECTS

- Classes are blueprints or templates for creating objects in Python.
- Objects are instances of classes that contain both data (attributes) and methods (functions).
- Classes define the structure and behavior that their objects will have.
- Python follows the Object-Oriented Programming (OOP) paradigm.
- **Key Concept**: Everything in Python is an object, including integers, strings, lists, and functions.

## `CREATING A CLASS`

Classes are defined using the `class` keyword followed by the class name (conventionally in PascalCase).

```python
# Basic class definition
class Person:
    pass  # Empty class

# Creating an object (instance) of the class
person1 = Person()
print(type(person1))  # Output: <class '__main__.Person'>
```

## `CLASS ATTRIBUTES AND INSTANCE ATTRIBUTES`

### 1. **Instance Attributes**

Instance attributes are unique to each object and are defined inside methods, typically in `__init__()`.

```python
class Person:
    def __init__(self, name, age):
        self.name = name        # Instance attribute
        self.age = age          # Instance attribute
        self.city = "Unknown"   # Default instance attribute

# Creating objects with different attributes
person1 = Person("Alice", 25)
person2 = Person("Bob", 30)

print(person1.name)  # Output: Alice
print(person2.name)  # Output: Bob
print(person1.age)   # Output: 25
print(person2.age)   # Output: 30
```

### 2. **Class Attributes**

Class attributes are shared by all instances of the class and are defined directly in the class body.

```python
class Person:
    species = "Homo sapiens"    # Class attribute
    count = 0                   # Class attribute to count instances

    def __init__(self, name, age):
        self.name = name        # Instance attribute
        self.age = age          # Instance attribute
        Person.count += 1       # Increment class attribute

    def display_info(self):
        return f"{self.name} is {self.age} years old"

# Creating objects
person1 = Person("Alice", 25)
person2 = Person("Bob", 30)

# Accessing class attributes
print(Person.species)       # Output: Homo sapiens
print(person1.species)      # Output: Homo sapiens (accessed through instance)
print(Person.count)         # Output: 2 (total instances created)

# Class attribute is shared
print(person1.species == person2.species)  # Output: True
```

## `THE __init__ METHOD (CONSTRUCTOR)`

The `__init__` method is a special method called when an object is created. It initializes the object's attributes.

```python
class Student:
    def __init__(self, name, student_id, grade=None):
        self.name = name
        self.student_id = student_id
        self.grade = grade if grade is not None else "Not Assigned"
        self.courses = []  # Empty list for each student

        # Validation
        if not isinstance(student_id, int):
            raise TypeError("Student ID must be an integer")

    def add_course(self, course):
        self.courses.append(course)

    def get_info(self):
        return f"Student: {self.name} (ID: {self.student_id}), Grade: {self.grade}"

# Creating student objects
student1 = Student("Alice", 12345, "A")
student2 = Student("Bob", 67890)  # Using default grade

print(student1.get_info())  # Output: Student: Alice (ID: 12345), Grade: A
print(student2.get_info())  # Output: Student: Bob (ID: 67890), Grade: Not Assigned

student1.add_course("Python Programming")
student1.add_course("Data Structures")
print(student1.courses)  # Output: ['Python Programming', 'Data Structures']
```

## `INSTANCE METHODS`

Instance methods are functions defined inside a class that operate on instances of the class. They take `self` as the first parameter.

```python
class BankAccount:
    def __init__(self, account_number, initial_balance=0):
        self.account_number = account_number
        self.balance = initial_balance
        self.transaction_history = []

    def deposit(self, amount):
        """Instance method to deposit money"""
        if amount > 0:
            self.balance += amount
            self.transaction_history.append(f"Deposited: ${amount}")
            return f"Deposited ${amount}. New balance: ${self.balance}"
        else:
            return "Invalid deposit amount"

    def withdraw(self, amount):
        """Instance method to withdraw money"""
        if amount > 0 and amount <= self.balance:
            self.balance -= amount
            self.transaction_history.append(f"Withdrew: ${amount}")
            return f"Withdrew ${amount}. New balance: ${self.balance}"
        else:
            return "Invalid withdrawal amount or insufficient funds"

    def get_balance(self):
        """Instance method to get current balance"""
        return f"Current balance: ${self.balance}"

    def get_transaction_history(self):
        """Instance method to get transaction history"""
        return self.transaction_history

# Using the class
account = BankAccount("ACC123", 1000)
print(account.get_balance())        # Output: Current balance: $1000
print(account.deposit(500))         # Output: Deposited $500. New balance: $1500
print(account.withdraw(200))        # Output: Withdrew $200. New balance: $1300
print(account.get_transaction_history())  # Output: ['Deposited: $500', 'Withdrew: $200']
```

## `CLASS METHODS`

Class methods are bound to the class rather than instances. They use the `@classmethod` decorator and take `cls` as the first parameter.

```python
class Employee:
    company_name = "TechCorp"
    employee_count = 0

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.employee_count += 1

    @classmethod
    def get_company_name(cls):
        """Class method to get company name"""
        return cls.company_name

    @classmethod
    def change_company_name(cls, new_name):
        """Class method to change company name"""
        cls.company_name = new_name

    @classmethod
    def get_employee_count(cls):
        """Class method to get total employee count"""
        return cls.employee_count

    @classmethod
    def create_intern(cls, name):
        """Class method as alternative constructor"""
        return cls(name, 25000)  # Default salary for interns

# Using class methods
emp1 = Employee("Alice", 60000)
emp2 = Employee("Bob", 55000)

print(Employee.get_company_name())      # Output: TechCorp
print(Employee.get_employee_count())    # Output: 2

# Creating intern using class method
intern = Employee.create_intern("Charlie")
print(intern.name, intern.salary)      # Output: Charlie 25000

# Changing company name affects all instances
Employee.change_company_name("NewTech")
print(emp1.company_name)                # Output: NewTech
print(Employee.get_company_name())      # Output: NewTech
```

## `STATIC METHODS`

Static methods don't take `self` or `cls` as parameters. They use the `@staticmethod` decorator and are utility functions related to the class.

```python
class MathUtils:
    @staticmethod
    def add(x, y):
        """Static method to add two numbers"""
        return x + y

    @staticmethod
    def is_even(number):
        """Static method to check if number is even"""
        return number % 2 == 0

    @staticmethod
    def factorial(n):
        """Static method to calculate factorial"""
        if n < 0:
            return None
        elif n == 0 or n == 1:
            return 1
        else:
            result = 1
            for i in range(2, n + 1):
                result *= i
            return result

# Using static methods (can be called on class or instance)
print(MathUtils.add(5, 3))          # Output: 8
print(MathUtils.is_even(10))        # Output: True
print(MathUtils.factorial(5))       # Output: 120

# Can also call on instance
math_obj = MathUtils()
print(math_obj.add(10, 20))         # Output: 30
```

## `PRIVATE AND PROTECTED ATTRIBUTES`

Python uses naming conventions to indicate privacy levels:

```python
class Person:
    def __init__(self, name, age, ssn):
        self.name = name            # Public attribute
        self._age = age             # Protected attribute (convention)
        self.__ssn = ssn            # Private attribute (name mangling)

    def get_age(self):
        """Public method to access protected attribute"""
        return self._age

    def get_ssn(self):
        """Public method to access private attribute"""
        return self.__ssn

    def _internal_method(self):
        """Protected method (convention)"""
        return "This is a protected method"

    def __private_method(self):
        """Private method (name mangling)"""
        return "This is a private method"

    def call_private_method(self):
        """Public method that calls private method"""
        return self.__private_method()

person = Person("Alice", 25, "123-45-6789")

# Public access
print(person.name)                  # Output: Alice

# Protected access (works but not recommended)
print(person._age)                  # Output: 25

# Private access (doesn't work directly)
# print(person.__ssn)               # AttributeError

# Access private through public method
print(person.get_ssn())             # Output: 123-45-6789

# Name mangling - Python internally changes __ssn to _Person__ssn
print(person._Person__ssn)          # Output: 123-45-6789 (not recommended!)

# Method calls
print(person._internal_method())    # Output: This is a protected method
print(person.call_private_method()) # Output: This is a private method
```

## `STRING REPRESENTATION METHODS`

For comprehensive coverage of string representation methods including `__str__`, `__repr__`, `__format__`, and advanced formatting techniques, see:

**📚 [String Representation Methods](./string-representation.md)**

This covers:

- `__str__` vs `__repr__` differences and best practices
- Custom formatting with `__format__`
- Integration with f-strings and format() function
- Real-world examples with Product, Temperature, and Money classes

## `COMPREHENSIVE DUNDER METHODS (MAGIC METHODS)`

For comprehensive coverage of all dunder methods (magic methods) including arithmetic operations, container emulation, context managers, and more, see:

**📚 [Comprehensive Dunder Methods](./dunder-methods.md)**

This covers:

- Arithmetic operations (`__add__`, `__sub__`, `__mul__`, etc.)
- Comparison methods (`__eq__`, `__lt__`, `__gt__`, etc.)
- Container emulation (`__len__`, `__getitem__`, `__setitem__`, etc.)
- Context managers (`__enter__`, `__exit__`)
- Callable objects (`__call__`)
- Advanced examples with Vector, Matrix, and custom containers

## `COMPREHENSIVE PROPERTY DECORATORS (GETTER AND SETTER)`

For comprehensive coverage of property decorators including validation, computed properties, caching, and lazy loading, see:

**📚 [Property Decorators Guide](./property-decorators.md)**

This covers:

- Basic property implementation with getter, setter, deleter
- Complex validation patterns with type checking and business rules
- Computed properties and read-only attributes
- Property caching and lazy loading techniques
- Real-world examples with BankAccount, Person, and DataProcessor classes

## `OBJECT COMPARISON METHODS`

For comprehensive coverage of object comparison methods including rich comparisons, sorting, equality testing, and hash consistency, see:

**📚 [Object Comparison Methods](./object-comparison.md)**

This covers:

- Rich comparison methods (`__eq__`, `__lt__`, `__gt__`, `__le__`, `__ge__`, `__ne__`)
- Multi-criteria comparison strategies and sorting patterns
- Hash consistency and use in sets/dictionaries
- Comparison with type flexibility and tolerance
- Advanced patterns with inheritance and performance optimization

## `PRACTICAL EXAMPLES`

### Example 1: Shopping Cart System

```python
class Product:
    def __init__(self, name, price, category):
        self.name = name
        self.price = price
        self.category = category

    def __str__(self):
        return f"{self.name} - ${self.price}"

    def __repr__(self):
        return f"Product('{self.name}', {self.price}, '{self.category}')"

class ShoppingCart:
    def __init__(self):
        self.items = []
        self.discount = 0

    def add_item(self, product, quantity=1):
        """Add product to cart"""
        for _ in range(quantity):
            self.items.append(product)
        return f"Added {quantity} x {product.name} to cart"

    def remove_item(self, product_name):
        """Remove first occurrence of product from cart"""
        for item in self.items:
            if item.name == product_name:
                self.items.remove(item)
                return f"Removed {product_name} from cart"
        return f"{product_name} not found in cart"

    def get_total(self):
        """Calculate total price"""
        subtotal = sum(item.price for item in self.items)
        return subtotal * (1 - self.discount / 100)

    def apply_discount(self, percentage):
        """Apply discount to cart"""
        self.discount = percentage
        return f"Applied {percentage}% discount"

    def get_cart_summary(self):
        """Get summary of cart contents"""
        if not self.items:
            return "Cart is empty"

        item_counts = {}
        for item in self.items:
            if item.name in item_counts:
                item_counts[item.name] += 1
            else:
                item_counts[item.name] = 1

        summary = "Cart Contents:\n"
        for name, count in item_counts.items():
            summary += f"- {name} x{count}\n"
        summary += f"Total: ${self.get_total():.2f}"
        return summary

# Using the shopping cart system
laptop = Product("Gaming Laptop", 1200, "Electronics")
mouse = Product("Wireless Mouse", 25, "Electronics")
book = Product("Python Guide", 40, "Books")

cart = ShoppingCart()
print(cart.add_item(laptop))        # Output: Added 1 x Gaming Laptop to cart
print(cart.add_item(mouse, 2))      # Output: Added 2 x Wireless Mouse to cart
print(cart.add_item(book))          # Output: Added 1 x Python Guide to cart

print(cart.get_cart_summary())
# Output:
# Cart Contents:
# - Gaming Laptop x1
# - Wireless Mouse x2
# - Python Guide x1
# Total: $1290.00

print(cart.apply_discount(10))      # Output: Applied 10% discount
print(f"New total: ${cart.get_total():.2f}")  # Output: New total: $1161.00
```

### Example 2: Library Management System

```python
from datetime import datetime, timedelta

class LibraryItem:
    total_items = 0

    def __init__(self, title, item_id):
        self.title = title
        self.item_id = item_id
        self.is_available = True
        self.borrowed_by = None
        self.due_date = None
        LibraryItem.total_items += 1

    def borrow(self, borrower_name, days=14):
        """Borrow the item"""
        if self.is_available:
            self.is_available = False
            self.borrowed_by = borrower_name
            self.due_date = datetime.now() + timedelta(days=days)
            return f"{self.title} borrowed by {borrower_name}. Due: {self.due_date.strftime('%Y-%m-%d')}"
        else:
            return f"{self.title} is not available"

    def return_item(self):
        """Return the item"""
        if not self.is_available:
            borrower = self.borrowed_by
            self.is_available = True
            self.borrowed_by = None
            self.due_date = None
            return f"{self.title} returned by {borrower}"
        else:
            return f"{self.title} was not borrowed"

    def is_overdue(self):
        """Check if item is overdue"""
        if self.due_date and datetime.now() > self.due_date:
            return True
        return False

    @classmethod
    def get_total_items(cls):
        """Get total number of library items"""
        return cls.total_items

class Book(LibraryItem):
    def __init__(self, title, item_id, author, pages):
        super().__init__(title, item_id)
        self.author = author
        self.pages = pages
        self.item_type = "Book"

    def __str__(self):
        return f"Book: {self.title} by {self.author}"

class DVD(LibraryItem):
    def __init__(self, title, item_id, director, duration):
        super().__init__(title, item_id)
        self.director = director
        self.duration = duration  # in minutes
        self.item_type = "DVD"

    def borrow(self, borrower_name, days=7):  # DVDs have shorter borrowing period
        """Override borrow method for DVDs"""
        return super().borrow(borrower_name, days)

    def __str__(self):
        return f"DVD: {self.title} directed by {self.director}"

# Using the library system
book1 = Book("Python Programming", "B001", "John Doe", 400)
book2 = Book("Data Structures", "B002", "Jane Smith", 350)
dvd1 = DVD("The Matrix", "D001", "Wachowski Sisters", 136)

print(book1)  # Output: Book: Python Programming by John Doe
print(dvd1)   # Output: DVD: The Matrix directed by Wachowski Sisters

print(book1.borrow("Alice"))        # Output: Python Programming borrowed by Alice. Due: 2025-10-14
print(dvd1.borrow("Bob"))           # Output: The Matrix borrowed by Bob. Due: 2025-10-07
print(book1.borrow("Charlie"))      # Output: Python Programming is not available

print(f"Total library items: {LibraryItem.get_total_items()}")  # Output: Total library items: 3

print(book1.return_item())          # Output: Python Programming returned by Alice
print(book1.borrow("Charlie"))      # Output: Python Programming borrowed by Charlie. Due: 2025-10-14
```

## `COMMON QUESTIONS AND CONCEPTS`

1. **Q: What's the difference between class and instance attributes?**

   - **A**: Class attributes are shared by all instances, instance attributes are unique to each object.

2. **Q: When should you use `@classmethod` vs `@staticmethod`?**

   - **A**: Use `@classmethod` when you need access to the class itself (alternative constructors). Use `@staticmethod` for utility functions related to the class.

3. **Q: What is the purpose of `self`?**

   - **A**: `self` refers to the current instance of the class and is used to access instance attributes and methods.

4. **Q: How do you create private attributes in Python?**

   - **A**: Use double underscores `__attribute` for name mangling, or single underscore `_attribute` by convention.

5. **Q: What's the difference between `__str__` and `__repr__`?**
   - **A**: `__str__` is for end-user readable output, `__repr__` is for developers and should be unambiguous.
