# STRING REPRESENTATION METHODS

String representation methods are special methods that define how objects are converted to strings for display purposes.

## `BASIC STRING REPRESENTATION`

### **`__str__()` Method**

The `__str__()` method defines the "informal" string representation of an object, intended for end-users.

```python
class Book:
    def __init__(self, title, author, pages):
        self.title = title
        self.author = author
        self.pages = pages

    def __str__(self):
        """String representation for end users"""
        return f"{self.title} by {self.author}"

book = Book("Python Programming", "John Doe", 350)
print(str(book))        # Output: Python Programming by John Doe
print(book)             # Output: Python Programming by John Doe (calls __str__)
```

### **`__repr__()` Method**

The `__repr__()` method defines the "official" string representation of an object, intended for developers and debugging.

```python
class Book:
    def __init__(self, title, author, pages):
        self.title = title
        self.author = author
        self.pages = pages

    def __str__(self):
        """String representation for end users"""
        return f"{self.title} by {self.author}"

    def __repr__(self):
        """String representation for developers (should be unambiguous)"""
        return f"Book('{self.title}', '{self.author}', {self.pages})"

book = Book("Python Programming", "John Doe", 350)

print(str(book))        # Output: Python Programming by John Doe
print(repr(book))       # Output: Book('Python Programming', 'John Doe', 350)

# In collections, __repr__ is used
books = [book]
print(books)            # Output: [Book('Python Programming', 'John Doe', 350)]

# In interactive mode, __repr__ is used
book                    # Output: Book('Python Programming', 'John Doe', 350)
```

## `ADVANCED STRING REPRESENTATION`

### **`__format__()` Method**

The `__format__()` method allows custom formatting with format specifiers.

```python
class Product:
    def __init__(self, name, price, category):
        self.name = name
        self.price = price
        self.category = category

    def __str__(self):
        """User-friendly string representation"""
        return f"{self.name} (${self.price})"

    def __repr__(self):
        """Developer-friendly, unambiguous representation"""
        return f"Product('{self.name}', {self.price}, '{self.category}')"

    def __format__(self, format_spec):
        """Custom formatting support"""
        if format_spec == 'full':
            return f"{self.name} - ${self.price} [{self.category}]"
        elif format_spec == 'name':
            return self.name
        elif format_spec == 'price':
            return f"${self.price}"
        elif format_spec == 'category':
            return self.category
        elif format_spec == 'json':
            return f'{{"name": "{self.name}", "price": {self.price}, "category": "{self.category}"}}'
        return str(self)

# Usage examples
laptop = Product("MacBook Pro", 1999, "Electronics")

print(str(laptop))      # Output: MacBook Pro ($1999)
print(repr(laptop))     # Output: Product('MacBook Pro', 1999, 'Electronics')
print(f"{laptop}")      # Uses __str__ - Output: MacBook Pro ($1999)
print(f"{laptop:full}") # Output: MacBook Pro - $1999 [Electronics]
print(f"{laptop:name}") # Output: MacBook Pro
print(f"{laptop:price}")# Output: $1999
print(f"{laptop:category}") # Output: Electronics
print(f"{laptop:json}") # Output: {"name": "MacBook Pro", "price": 1999, "category": "Electronics"}

# In collections, __repr__ is used
products = [laptop, Product("iPhone", 999, "Electronics")]
print(products)  # Uses __repr__ for each item
```

## `COMPREHENSIVE EXAMPLES`

### **Example 1: Person Class with Multiple Representations**

```python
class Person:
    def __init__(self, first_name, last_name, age, occupation):
        self.first_name = first_name
        self.last_name = last_name
        self.age = age
        self.occupation = occupation

    def __str__(self):
        """Friendly representation for general use"""
        return f"{self.first_name} {self.last_name}"

    def __repr__(self):
        """Detailed representation for debugging"""
        return f"Person('{self.first_name}', '{self.last_name}', {self.age}, '{self.occupation}')"

    def __format__(self, format_spec):
        """Custom formatting options"""
        if format_spec == 'full':
            return f"{self.first_name} {self.last_name}, {self.age} years old ({self.occupation})"
        elif format_spec == 'formal':
            return f"{self.last_name}, {self.first_name}"
        elif format_spec == 'initials':
            return f"{self.first_name[0]}.{self.last_name[0]}."
        elif format_spec == 'age':
            return str(self.age)
        elif format_spec == 'occupation':
            return self.occupation
        return str(self)

# Usage examples
person = Person("John", "Doe", 30, "Software Engineer")

print(f"String: {str(person)}")          # String: John Doe
print(f"Repr: {repr(person)}")            # Repr: Person('John', 'Doe', 30, 'Software Engineer')
print(f"Default: {person}")               # Default: John Doe
print(f"Full: {person:full}")             # Full: John Doe, 30 years old (Software Engineer)
print(f"Formal: {person:formal}")         # Formal: Doe, John
print(f"Initials: {person:initials}")     # Initials: J.D.
print(f"Age: {person:age}")               # Age: 30
print(f"Job: {person:occupation}")        # Job: Software Engineer

# In different contexts
people = [person, Person("Jane", "Smith", 25, "Data Scientist")]
print("People list:")
for p in people:
    print(f"  {p:full}")

# Output:
# People list:
#   John Doe, 30 years old (Software Engineer)
#   Jane Smith, 25 years old (Data Scientist)
```

### **Example 2: Temperature Class with Unit Formatting**

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    @property
    def fahrenheit(self):
        return (self.celsius * 9/5) + 32

    @property
    def kelvin(self):
        return self.celsius + 273.15

    def __str__(self):
        """Default string representation in Celsius"""
        return f"{self.celsius}°C"

    def __repr__(self):
        """Unambiguous representation"""
        return f"Temperature({self.celsius})"

    def __format__(self, format_spec):
        """Support multiple temperature formats"""
        if format_spec == 'C' or format_spec == 'celsius':
            return f"{self.celsius}°C"
        elif format_spec == 'F' or format_spec == 'fahrenheit':
            return f"{self.fahrenheit:.1f}°F"
        elif format_spec == 'K' or format_spec == 'kelvin':
            return f"{self.kelvin:.1f}K"
        elif format_spec == 'all':
            return f"{self.celsius}°C ({self.fahrenheit:.1f}°F, {self.kelvin:.1f}K)"
        elif format_spec.startswith('.') and format_spec.endswith('f'):
            # Handle decimal precision (e.g., '.2f')
            precision = int(format_spec[1:-1])
            return f"{self.celsius:.{precision}f}°C"
        return str(self)

# Usage examples
temp = Temperature(25.5)

print(f"Default: {temp}")           # Default: 25.5°C
print(f"Repr: {repr(temp)}")        # Repr: Temperature(25.5)
print(f"Celsius: {temp:C}")         # Celsius: 25.5°C
print(f"Fahrenheit: {temp:F}")      # Fahrenheit: 77.9°F
print(f"Kelvin: {temp:K}")          # Kelvin: 298.7K
print(f"All units: {temp:all}")     # All units: 25.5°C (77.9°F, 298.7K)
print(f"Precision: {temp:.1f}")     # Precision: 25.5°C

# Temperature readings
readings = [Temperature(0), Temperature(25), Temperature(100)]
print("Temperature readings:")
for reading in readings:
    print(f"  {reading:all}")

# Output:
# Temperature readings:
#   0°C (32.0°F, 273.2K)
#   25°C (77.0°F, 298.2K)
#   100°C (212.0°F, 373.2K)
```

### **Example 3: Money Class with Currency Formatting**

```python
class Money:
    def __init__(self, amount, currency="USD"):
        self.amount = amount
        self.currency = currency

    def __str__(self):
        """Default string representation"""
        return f"{self.currency} {self.amount:.2f}"

    def __repr__(self):
        """Unambiguous representation"""
        return f"Money({self.amount}, '{self.currency}')"

    def __format__(self, format_spec):
        """Advanced currency formatting"""
        if format_spec == 'symbol':
            symbols = {'USD': '$', 'EUR': '€', 'GBP': '£', 'JPY': '¥'}
            symbol = symbols.get(self.currency, self.currency)
            return f"{symbol}{self.amount:.2f}"
        elif format_spec == 'full':
            return f"{self.amount:.2f} {self.currency}"
        elif format_spec == 'accounting':
            if self.amount < 0:
                return f"({self.currency} {abs(self.amount):.2f})"
            return f"{self.currency} {self.amount:.2f}"
        elif format_spec.startswith('.') and format_spec.endswith('f'):
            # Handle decimal precision
            precision = int(format_spec[1:-1])
            return f"{self.currency} {self.amount:.{precision}f}"
        elif format_spec == 'words':
            # Simple number to words (basic implementation)
            return f"{self.amount:.2f} {self.currency} ({self._amount_to_words()})"
        return str(self)

    def _amount_to_words(self):
        """Convert amount to words (simplified)"""
        # This is a simplified version - real implementation would be more complex
        amount_int = int(self.amount)
        if amount_int == 0:
            return "zero"
        elif amount_int < 20:
            words = ["", "one", "two", "three", "four", "five", "six", "seven",
                    "eight", "nine", "ten", "eleven", "twelve", "thirteen",
                    "fourteen", "fifteen", "sixteen", "seventeen", "eighteen", "nineteen"]
            return words[amount_int]
        elif amount_int < 100:
            tens = ["", "", "twenty", "thirty", "forty", "fifty", "sixty", "seventy", "eighty", "ninety"]
            return tens[amount_int // 10] + ("" if amount_int % 10 == 0 else " " + self._amount_to_words_helper(amount_int % 10))
        else:
            return "many"

    def _amount_to_words_helper(self, n):
        words = ["", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"]
        return words[n]

# Usage examples
usd_money = Money(1234.56, "USD")
eur_money = Money(-500.75, "EUR")

print(f"Default: {usd_money}")           # Default: USD 1234.56
print(f"Repr: {repr(usd_money)}")        # Repr: Money(1234.56, 'USD')
print(f"Symbol: {usd_money:symbol}")     # Symbol: $1234.56
print(f"Full: {usd_money:full}")         # Full: 1234.56 USD
print(f"Accounting: {usd_money:accounting}")  # Accounting: USD 1234.56
print(f"Precision: {usd_money:.0f}")     # Precision: USD 1235

print(f"Negative accounting: {eur_money:accounting}")  # Negative accounting: (EUR 500.75)

# Portfolio display
portfolio = [
    Money(1000, "USD"),
    Money(750, "EUR"),
    Money(50000, "JPY")
]

print("\nPortfolio:")
for money in portfolio:
    print(f"  {money:symbol} ({money:full})")

# Output:
# Portfolio:
#   $1000.00 (1000.00 USD)
#   €750.00 (750.00 EUR)
#   ¥50000.00 (50000.00 JPY)
```

## `BEST PRACTICES FOR STRING REPRESENTATION`

### **1. `__str__` vs `__repr__` Guidelines**

```python
class BestPracticeExample:
    def __init__(self, name, value):
        self.name = name
        self.value = value

    def __str__(self):
        """
        Should be readable and concise for end users
        Focus on the most important information
        """
        return f"{self.name}: {self.value}"

    def __repr__(self):
        """
        Should be unambiguous and ideally eval(repr(obj)) == obj
        Include all necessary information to recreate the object
        """
        return f"BestPracticeExample('{self.name}', {self.value})"

# Test the best practice
obj = BestPracticeExample("example", 42)
print(f"str(): {str(obj)}")     # str(): example: 42
print(f"repr(): {repr(obj)}")   # repr(): BestPracticeExample('example', 42)

# repr should ideally allow object recreation
recreated = eval(repr(obj))
print(f"Recreated: {recreated}")  # Recreated: example: 42
```

### **2. Handling Edge Cases**

```python
class RobustStringExample:
    def __init__(self, data):
        self.data = data

    def __str__(self):
        """Handle various data types gracefully"""
        if self.data is None:
            return "Empty Data"
        elif isinstance(self.data, str):
            return f"Text: {self.data}"
        elif isinstance(self.data, (list, tuple)):
            if len(self.data) == 0:
                return "Empty Collection"
            elif len(self.data) <= 3:
                return f"Collection: {', '.join(map(str, self.data))}"
            else:
                return f"Collection: {', '.join(map(str, self.data[:3]))}... (+{len(self.data)-3} more)"
        else:
            return f"Data: {self.data}"

    def __repr__(self):
        """Unambiguous representation"""
        return f"RobustStringExample({repr(self.data)})"

# Test with different data types
examples = [
    RobustStringExample(None),
    RobustStringExample("Hello World"),
    RobustStringExample([1, 2, 3]),
    RobustStringExample([1, 2, 3, 4, 5, 6, 7]),
    RobustStringExample([]),
    RobustStringExample(42)
]

for example in examples:
    print(f"str: {str(example)}")
    print(f"repr: {repr(example)}")
    print()

# Output:
# str: Empty Data
# repr: RobustStringExample(None)
#
# str: Text: Hello World
# repr: RobustStringExample('Hello World')
#
# str: Collection: 1, 2, 3
# repr: RobustStringExample([1, 2, 3])
#
# str: Collection: 1, 2, 3... (+4 more)
# repr: RobustStringExample([1, 2, 3, 4, 5, 6, 7])
#
# str: Empty Collection
# repr: RobustStringExample([])
#
# str: Data: 42
# repr: RobustStringExample(42)
```

## `INTERVIEW INSIGHTS`

### **Key Concept: When Each Method is Called**

```python
class InterviewExample:
    def __init__(self, value):
        self.value = value

    def __str__(self):
        return f"STR: {self.value}"

    def __repr__(self):
        return f"REPR: {self.value}"

obj = InterviewExample("test")

# Different contexts call different methods
print("1. str() function:", str(obj))           # Calls __str__
print("2. repr() function:", repr(obj))         # Calls __repr__
print("3. print() function:", obj)              # Calls __str__
print("4. f-string:", f"{obj}")                 # Calls __str__
print("5. Interactive mode:")
obj                                             # Would call __repr__ in interactive mode
print("6. In container:", [obj])                # Calls __repr__
print("7. String conversion:", str([obj]))      # Container uses __repr__

# Output:
# 1. str() function: STR: test
# 2. repr() function: REPR: test
# 3. print() function: STR: test
# 4. f-string: STR: test
# 5. Interactive mode:
# 6. In container: [REPR: test]
# 7. String conversion: [REPR: test]
```

### **Important Note: Fallback Behavior**

```python
class OnlyRepr:
    def __init__(self, value):
        self.value = value

    def __repr__(self):
        return f"OnlyRepr({self.value})"

    # No __str__ method defined

class OnlyStr:
    def __init__(self, value):
        self.value = value

    def __str__(self):
        return f"OnlyStr: {self.value}"

    # No __repr__ method defined

# Test fallback behavior
only_repr = OnlyRepr("test1")
only_str = OnlyStr("test2")

print("Only __repr__ defined:")
print(f"  str(): {str(only_repr)}")        # Falls back to __repr__
print(f"  repr(): {repr(only_repr)}")      # Uses __repr__

print("\nOnly __str__ defined:")
print(f"  str(): {str(only_str)}")         # Uses __str__
print(f"  repr(): {repr(only_str)}")       # Falls back to default representation

# In containers
print(f"\nIn container with only __repr__: {[only_repr]}")  # Uses __repr__
print(f"In container with only __str__: {[only_str]}")      # Uses default, not __str__
```

### **Performance Consideration**

```python
class PerformanceExample:
    def __init__(self, data):
        self.data = data

    def __str__(self):
        """Efficient string representation"""
        # Keep __str__ fast and simple
        return f"Data({len(self.data)} items)"

    def __repr__(self):
        """Detailed but potentially expensive representation"""
        # __repr__ can be more detailed but should still be reasonable
        if len(self.data) <= 10:
            return f"PerformanceExample({self.data})"
        else:
            return f"PerformanceExample([{self.data[0]}, ..., {self.data[-1]}] ({len(self.data)} items))"

# Test with large data
large_data = list(range(1000))
example = PerformanceExample(large_data)

print(f"Fast str(): {str(example)}")        # Fast: Data(1000 items)
print(f"Detailed repr(): {repr(example)}")  # Detailed but reasonable: PerformanceExample([0, ..., 999] (1000 items))
```

String representation methods are crucial for debugging, logging, and user interaction. Implementing them thoughtfully makes your objects much more usable and professional.
