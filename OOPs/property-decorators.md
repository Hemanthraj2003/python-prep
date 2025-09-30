# COMPREHENSIVE PROPERTY DECORATORS (GETTER AND SETTER)

Properties provide a Pythonic way to implement getters, setters, and deleters while maintaining attribute-like access syntax. They allow you to add validation, computation, and control to attribute access.

## `BASIC PROPERTY CONCEPTS`

### **Why Use Properties?**

Properties allow you to:

- Add validation when setting values
- Make computed attributes appear like regular attributes
- Control access (read-only, write-only, read-write)
- Add logging or side effects to attribute access
- Maintain backward compatibility when changing internal implementation

### **Basic Property Syntax**

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        """Getter method"""
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        """Setter method with validation"""
        if not isinstance(value, (int, float)):
            raise TypeError("Temperature must be a number")
        if value < -273.15:
            raise ValueError("Temperature cannot be below absolute zero")
        self._celsius = value

    @celsius.deleter
    def celsius(self, value):
        """Deleter method"""
        print("Temperature value deleted")
        self._celsius = 0

# Usage
temp = Temperature(25)
print(temp.celsius)        # Calls getter: 25
temp.celsius = 30          # Calls setter: validates and sets
del temp.celsius           # Calls deleter: prints message and resets
```

## `ADVANCED PROPERTY PATTERNS`

### **Read-Only Properties (Computed Properties)**

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value <= 0:
            raise ValueError("Radius must be positive")
        self._radius = value

    @property
    def diameter(self):
        """Read-only computed property"""
        return self._radius * 2

    @property
    def area(self):
        """Read-only computed property"""
        import math
        return math.pi * self._radius ** 2

    @property
    def circumference(self):
        """Read-only computed property"""
        import math
        return 2 * math.pi * self._radius

# Usage
circle = Circle(5)
print(f"Radius: {circle.radius}")           # 5
print(f"Diameter: {circle.diameter}")       # 10
print(f"Area: {circle.area:.2f}")           # 78.54
print(f"Circumference: {circle.circumference:.2f}")  # 31.42

# Changing radius updates all computed properties
circle.radius = 10
print(f"New diameter: {circle.diameter}")   # 20
print(f"New area: {circle.area:.2f}")       # 314.16

# Can't set read-only properties
try:
    circle.area = 100  # AttributeError: can't set attribute
except AttributeError as e:
    print(f"Error: {e}")
```

### **Property with Complex Validation**

```python
class Person:
    def __init__(self, first_name, last_name, age, email):
        # Use setters for validation during initialization
        self.first_name = first_name
        self.last_name = last_name
        self.age = age
        self.email = email

    @property
    def first_name(self):
        return self._first_name

    @first_name.setter
    def first_name(self, value):
        if not isinstance(value, str):
            raise TypeError("First name must be a string")
        if not value.strip():
            raise ValueError("First name cannot be empty")
        if not value.replace(' ', '').replace('-', '').isalpha():
            raise ValueError("First name can only contain letters, spaces, and hyphens")
        if len(value.strip()) < 2:
            raise ValueError("First name must be at least 2 characters")
        self._first_name = value.strip().title()

    @property
    def last_name(self):
        return self._last_name

    @last_name.setter
    def last_name(self, value):
        if not isinstance(value, str):
            raise TypeError("Last name must be a string")
        if not value.strip():
            raise ValueError("Last name cannot be empty")
        if not value.replace(' ', '').replace('-', '').isalpha():
            raise ValueError("Last name can only contain letters, spaces, and hyphens")
        if len(value.strip()) < 2:
            raise ValueError("Last name must be at least 2 characters")
        self._last_name = value.strip().title()

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if not isinstance(value, int):
            raise TypeError("Age must be an integer")
        if value < 0:
            raise ValueError("Age cannot be negative")
        if value > 150:
            raise ValueError("Age cannot be more than 150")
        self._age = value

    @property
    def email(self):
        return self._email

    @email.setter
    def email(self, value):
        if not isinstance(value, str):
            raise TypeError("Email must be a string")

        # Basic email validation
        email = value.strip().lower()
        if not email:
            raise ValueError("Email cannot be empty")
        if email.count('@') != 1:
            raise ValueError("Email must contain exactly one @ symbol")

        local, domain = email.split('@')
        if not local or not domain:
            raise ValueError("Email must have both local and domain parts")
        if '.' not in domain:
            raise ValueError("Email domain must contain at least one dot")
        if domain.startswith('.') or domain.endswith('.'):
            raise ValueError("Email domain cannot start or end with a dot")

        self._email = email

    @property
    def full_name(self):
        """Read-only computed property"""
        return f"{self._first_name} {self._last_name}"

    @property
    def initials(self):
        """Read-only computed property"""
        return f"{self._first_name[0]}.{self._last_name[0]}."

    @property
    def age_category(self):
        """Read-only computed property based on age"""
        if self._age < 13:
            return "Child"
        elif self._age < 20:
            return "Teenager"
        elif self._age < 65:
            return "Adult"
        else:
            return "Senior"

    @property
    def email_domain(self):
        """Read-only computed property"""
        return self._email.split('@')[1]

# Usage examples
try:
    person = Person("john-paul", "van der berg", 25, "John.Paul@Example.COM")

    print(f"Full name: {person.full_name}")        # John-Paul Van Der Berg
    print(f"Initials: {person.initials}")          # J.V.
    print(f"Email: {person.email}")                # john.paul@example.com
    print(f"Domain: {person.email_domain}")        # example.com
    print(f"Age category: {person.age_category}")  # Adult

    # Property validation in action
    person.first_name = "marie-claire"  # Automatically formatted to "Marie-Claire"
    print(f"Updated name: {person.full_name}")

    # Test validation
    person.age = 30  # Valid
    print(f"Updated age: {person.age}")

except (ValueError, TypeError) as e:
    print(f"Validation error: {e}")

# Test invalid inputs
test_cases = [
    ("age", -5, "Age cannot be negative"),
    ("age", "thirty", "Age must be an integer"),
    ("email", "invalid.email", "Email must contain exactly one @ symbol"),
    ("first_name", "", "First name cannot be empty"),
    ("first_name", "X", "First name must be at least 2 characters"),
]

for attr, value, expected_error in test_cases:
    try:
        setattr(person, attr, value)
        print(f"ERROR: {attr} = {value} should have failed!")
    except (ValueError, TypeError) as e:
        print(f"✓ {attr} validation: {e}")
```

### **Property Caching and Lazy Loading**

```python
import time
from functools import wraps

def cached_property(func):
    """Decorator for cached properties"""
    attr_name = f'_cached_{func.__name__}'

    @property
    @wraps(func)
    def wrapper(self):
        if not hasattr(self, attr_name):
            print(f"Computing {func.__name__}...")
            result = func(self)
            setattr(self, attr_name, result)
        else:
            print(f"Using cached {func.__name__}")
        return getattr(self, attr_name)

    return wrapper

class DataProcessor:
    def __init__(self, data_source):
        self.data_source = data_source
        self._raw_data = None
        self._processed_data = None

    @property
    def raw_data(self):
        """Lazy loading of raw data"""
        if self._raw_data is None:
            print(f"Loading data from {self.data_source}...")
            time.sleep(0.5)  # Simulate loading time
            # Simulate loading data
            self._raw_data = list(range(1000))
            print("Raw data loaded!")
        return self._raw_data

    @property
    def processed_data(self):
        """Computed property with caching"""
        if self._processed_data is None:
            print("Processing raw data...")
            time.sleep(0.3)  # Simulate processing time
            # Process the raw data
            self._processed_data = [x * 2 for x in self.raw_data]
            print("Data processing completed!")
        return self._processed_data

    @cached_property
    def statistics(self):
        """Cached computed statistics using decorator"""
        time.sleep(0.2)  # Simulate computation time
        data = self.processed_data
        return {
            'count': len(data),
            'sum': sum(data),
            'mean': sum(data) / len(data),
            'min': min(data),
            'max': max(data),
            'median': sorted(data)[len(data) // 2]
        }

    @cached_property
    def summary_report(self):
        """Another cached property that depends on statistics"""
        stats = self.statistics
        return f"""
Data Summary Report:
- Total items: {stats['count']:,}
- Sum: {stats['sum']:,}
- Average: {stats['mean']:.2f}
- Range: {stats['min']} to {stats['max']}
- Median: {stats['median']}
"""

    def refresh_cache(self):
        """Clear all cached data to force recomputation"""
        self._raw_data = None
        self._processed_data = None
        # Clear cached properties
        cached_attrs = [attr for attr in dir(self) if attr.startswith('_cached_')]
        for attr in cached_attrs:
            delattr(self, attr)
        print("All caches cleared!")

# Usage examples
processor = DataProcessor("large_dataset.csv")

print("=== First access (everything loads/computes) ===")
stats = processor.statistics
print(f"Mean: {stats['mean']}")

print("\n=== Second access (uses cached data) ===")
stats2 = processor.statistics  # Uses cached data
report = processor.summary_report  # Computes and caches

print("\n=== Third access (all cached) ===")
stats3 = processor.statistics  # Uses cached data
report2 = processor.summary_report  # Uses cached data

print("\n=== After cache refresh ===")
processor.refresh_cache()
stats4 = processor.statistics  # Recomputes everything
```

### **Property with Lazy Validation**

```python
class LazyValidatedData:
    def __init__(self):
        self._data = {}
        self._validation_cache = {}

    def _validate_value(self, key, value):
        """Expensive validation that we want to cache"""
        print(f"Validating {key}...")
        time.sleep(0.1)  # Simulate expensive validation

        if key == 'email':
            return '@' in str(value) and '.' in str(value)
        elif key == 'age':
            return isinstance(value, int) and 0 <= value <= 150
        elif key == 'phone':
            phone_str = str(value).replace('-', '').replace(' ', '').replace('(', '').replace(')', '')
            return phone_str.isdigit() and 10 <= len(phone_str) <= 15
        return True

    def __setattr__(self, name, value):
        if name.startswith('_'):
            super().__setattr__(name, value)
        else:
            # Store value immediately, validate later
            if not hasattr(self, '_data'):
                super().__setattr__('_data', {})
            if not hasattr(self, '_validation_cache'):
                super().__setattr__('_validation_cache', {})

            self._data[name] = value
            # Clear validation cache for this key
            if name in self._validation_cache:
                del self._validation_cache[name]

    def __getattribute__(self, name):
        if name.startswith('_') or name in ['_validate_value']:
            return super().__getattribute__(name)

        _data = super().__getattribute__('_data')
        _validation_cache = super().__getattribute__('_validation_cache')

        if name in _data:
            # Lazy validation on access
            if name not in _validation_cache:
                is_valid = self._validate_value(name, _data[name])
                _validation_cache[name] = is_valid
                if not is_valid:
                    raise ValueError(f"Invalid value for {name}: {_data[name]}")
            elif not _validation_cache[name]:
                raise ValueError(f"Invalid value for {name}: {_data[name]}")

            return _data[name]

        return super().__getattribute__(name)

# Usage
lazy_data = LazyValidatedData()

# Setting values (no validation yet)
lazy_data.email = "user@example.com"
lazy_data.age = 25
lazy_data.phone = "555-123-4567"

print("Values set, now accessing...")

# Accessing triggers validation
print(f"Email: {lazy_data.email}")    # Validates email
print(f"Age: {lazy_data.age}")        # Validates age
print(f"Phone: {lazy_data.phone}")    # Validates phone

print("\nSecond access (uses cached validation):")
print(f"Email: {lazy_data.email}")    # Uses cached validation result

# Invalid value
lazy_data.invalid_email = "not-an-email"
try:
    print(lazy_data.invalid_email)    # Triggers validation and fails
except ValueError as e:
    print(f"Validation error: {e}")
```

## `PROPERTY FACTORIES AND DESCRIPTORS`

### **Property Factory for Repeated Patterns**

```python
def validated_property(name, validator, doc=None):
    """Factory function to create validated properties"""
    private_name = f'_{name}'

    def getter(self):
        return getattr(self, private_name)

    def setter(self, value):
        if not validator(value):
            raise ValueError(f"Invalid value for {name}: {value}")
        setattr(self, private_name, value)

    return property(getter, setter, doc=doc)

def positive_number_validator(value):
    return isinstance(value, (int, float)) and value > 0

def non_empty_string_validator(value):
    return isinstance(value, str) and len(value.strip()) > 0

def email_validator(value):
    return isinstance(value, str) and '@' in value and '.' in value

class Product:
    # Using property factory
    name = validated_property('name', non_empty_string_validator, "Product name")
    price = validated_property('price', positive_number_validator, "Product price")
    email = validated_property('email', email_validator, "Contact email")

    def __init__(self, name, price, email):
        self.name = name
        self.price = price
        self.email = email

# Usage
try:
    product = Product("Laptop", 999.99, "sales@company.com")
    print(f"Product: {product.name}, Price: ${product.price}")

    # This will fail validation
    product.price = -100  # ValueError: Invalid value for price: -100
except ValueError as e:
    print(f"Validation error: {e}")
```

### **Descriptor Class for Advanced Properties**

```python
class ValidatedAttribute:
    """Descriptor class for validated attributes"""

    def __init__(self, validator, doc=None):
        self.validator = validator
        self.doc = doc

    def __set_name__(self, owner, name):
        """Called when descriptor is assigned to a class attribute"""
        self.name = name
        self.private_name = f'_{name}'

    def __get__(self, obj, objtype=None):
        """Get the attribute value"""
        if obj is None:
            return self
        return getattr(obj, self.private_name, None)

    def __set__(self, obj, value):
        """Set the attribute value with validation"""
        if not self.validator(value):
            raise ValueError(f"Invalid value for {self.name}: {value}")
        setattr(obj, self.private_name, value)

    def __delete__(self, obj):
        """Delete the attribute"""
        delattr(obj, self.private_name)

# Validator functions
def range_validator(min_val, max_val):
    return lambda x: isinstance(x, (int, float)) and min_val <= x <= max_val

def length_validator(min_len, max_len):
    return lambda x: isinstance(x, str) and min_len <= len(x.strip()) <= max_len

def choice_validator(*choices):
    return lambda x: x in choices

class Employee:
    # Using descriptor class
    name = ValidatedAttribute(length_validator(2, 50), "Employee name")
    age = ValidatedAttribute(range_validator(18, 100), "Employee age")
    department = ValidatedAttribute(
        choice_validator("Engineering", "Sales", "Marketing", "HR"),
        "Employee department"
    )
    salary = ValidatedAttribute(range_validator(0, 1000000), "Employee salary")

    def __init__(self, name, age, department, salary):
        self.name = name
        self.age = age
        self.department = department
        self.salary = salary

    def __str__(self):
        return f"{self.name} ({self.age}) - {self.department}: ${self.salary}"

# Usage
try:
    emp = Employee("Alice Johnson", 30, "Engineering", 85000)
    print(emp)  # Alice Johnson (30) - Engineering: $85000

    # Valid changes
    emp.salary = 90000
    emp.department = "Sales"
    print(emp)  # Alice Johnson (30) - Sales: $90000

    # Invalid changes
    emp.age = 200  # ValueError: Invalid value for age: 200
except ValueError as e:
    print(f"Validation error: {e}")

# Test other validations
test_cases = [
    ("name", "", "Too short"),
    ("name", "A" * 60, "Too long"),
    ("department", "Finance", "Invalid choice"),
    ("salary", -1000, "Negative salary"),
]

for attr, value, description in test_cases:
    try:
        setattr(emp, attr, value)
        print(f"ERROR: {description} should have failed!")
    except ValueError as e:
        print(f"✓ {description}: {e}")
```

## `PROPERTY BEST PRACTICES`

### **1. Property vs Method Decision**

```python
class BestPracticesExample:
    def __init__(self, data):
        self._data = data

    @property
    def size(self):
        """Use property for simple attribute-like access"""
        return len(self._data)

    @property
    def is_empty(self):
        """Use property for simple boolean checks"""
        return len(self._data) == 0

    @property
    def summary(self):
        """Use property for computed values that feel like attributes"""
        return f"Data with {len(self._data)} items"

    def calculate_statistics(self):
        """Use method for operations that clearly do work"""
        # This clearly performs computation
        return {
            'mean': sum(self._data) / len(self._data) if self._data else 0,
            'max': max(self._data) if self._data else None,
            'min': min(self._data) if self._data else None
        }

    def save_to_file(self, filename):
        """Use method for actions that have side effects"""
        with open(filename, 'w') as f:
            for item in self._data:
                f.write(f"{item}\n")

# Guidelines:
# - Use properties for attribute-like access (size, is_empty, summary)
# - Use methods for operations that clearly do work or have side effects
# - Properties should be fast and not have surprising side effects
```

### **2. Error Handling in Properties**

```python
class RobustProperty:
    def __init__(self, initial_value=None):
        self._value = initial_value
        self._error_count = 0

    @property
    def value(self):
        return self._value

    @value.setter
    def value(self, new_value):
        try:
            # Attempt validation/conversion
            if isinstance(new_value, str):
                new_value = float(new_value)  # Try to convert string to float
            if not isinstance(new_value, (int, float)):
                raise TypeError("Value must be a number")
            if new_value < 0:
                raise ValueError("Value must be non-negative")

            self._value = new_value
            self._error_count = 0  # Reset error count on success

        except (ValueError, TypeError) as e:
            self._error_count += 1

            # Provide helpful error messages
            if isinstance(e, TypeError):
                raise TypeError(f"Invalid type for value: expected number, got {type(new_value).__name__}")
            else:
                raise ValueError(f"Invalid value: {e}")

    @property
    def has_errors(self):
        """Read-only property to check if there have been errors"""
        return self._error_count > 0

    @property
    def error_count(self):
        """Read-only property to get error count"""
        return self._error_count

# Usage with error handling
robust = RobustProperty(10)

test_values = [20, "30", "invalid", -5, 40.5]

for test_value in test_values:
    try:
        robust.value = test_value
        print(f"✓ Set value to: {robust.value}")
    except (ValueError, TypeError) as e:
        print(f"✗ Error setting '{test_value}': {e}")

print(f"Final value: {robust.value}")
print(f"Error count: {robust.error_count}")
```

## `INTERVIEW INSIGHTS`

### **Key Concept: Property vs Attribute**

```python
class InterviewExample:
    def __init__(self, name):
        self._name = name

    # This is a property (has getter/setter)
    @property
    def name(self):
        print("Getting name")
        return self._name

    @name.setter
    def name(self, value):
        print(f"Setting name to {value}")
        self._name = value

    # This is a regular attribute (direct access)
    def __init__(self, name):
        self.direct_name = name  # Regular attribute

obj = InterviewExample("test")

# Property access (calls getter/setter)
print(obj.name)      # Prints: "Getting name", then "test"
obj.name = "new"     # Prints: "Setting name to new"

# Direct attribute access (no method calls)
print(obj.direct_name)  # Just returns "test"
obj.direct_name = "new"  # Just sets the value
```

### **Important Note: Property Inheritance**

```python
class Parent:
    def __init__(self, value):
        self._value = value

    @property
    def value(self):
        return self._value

    @value.setter
    def value(self, new_value):
        print("Parent setter")
        self._value = new_value

class Child(Parent):
    @property
    def value(self):
        print("Child getter")
        return super().value  # Call parent getter

    @value.setter
    def value(self, new_value):
        print("Child setter")
        if new_value < 0:
            raise ValueError("Child doesn't allow negative values")
        super(Child, self.__class__).value.__set__(self, new_value)

# Usage
child = Child(10)
print(child.value)    # Prints: "Child getter", then "10"
child.value = 20      # Prints: "Child setter", then "Parent setter"

try:
    child.value = -5  # Prints: "Child setter", then raises ValueError
except ValueError as e:
    print(f"Error: {e}")
```

### **Performance Tip: Property Caching**

```python
class PerformanceOptimized:
    def __init__(self, data):
        self._data = data
        self._expensive_result = None
        self._data_hash = None

    @property
    def expensive_computation(self):
        """Cache expensive computations"""
        current_hash = hash(tuple(self._data))

        if self._data_hash != current_hash or self._expensive_result is None:
            print("Computing expensive result...")
            # Simulate expensive computation
            self._expensive_result = sum(x**2 for x in self._data)
            self._data_hash = current_hash
        else:
            print("Using cached result")

        return self._expensive_result

    def modify_data(self, new_data):
        """Modifying data invalidates cache"""
        self._data = new_data
        # Cache will be invalidated automatically due to hash change

# Usage
obj = PerformanceOptimized([1, 2, 3, 4, 5])
print(obj.expensive_computation)  # Computes: 55
print(obj.expensive_computation)  # Uses cache: 55

obj.modify_data([1, 2, 3])
print(obj.expensive_computation)  # Recomputes: 14
```

Properties are a powerful feature that makes Python classes more intuitive and maintainable by providing controlled access to attributes while maintaining a clean, attribute-like interface.
