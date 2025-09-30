# POLYMORPHISM

- Polymorphism means "many forms" and allows objects of different classes to be treated as objects of a common base class.
- It enables a single interface to represent different underlying forms (data types).
- Polymorphism allows the same method name to behave differently for different objects.
- **Key Concept**: Polymorphism enables writing more flexible and reusable code.

## `METHOD OVERRIDING`

Method overriding occurs when a child class provides a specific implementation of a method that is already defined in its parent class.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def make_sound(self):
        return f"{self.name} makes a generic animal sound"

    def move(self):
        return f"{self.name} moves"

    def sleep(self):
        return f"{self.name} is sleeping"

class Dog(Animal):
    def make_sound(self):  # Override parent method
        return f"{self.name} barks: Woof! Woof!"

    def move(self):  # Override parent method
        return f"{self.name} runs on four legs"

class Cat(Animal):
    def make_sound(self):  # Override parent method
        return f"{self.name} meows: Meow!"

    def move(self):  # Override parent method
        return f"{self.name} walks silently"

class Bird(Animal):
    def make_sound(self):  # Override parent method
        return f"{self.name} chirps: Tweet! Tweet!"

    def move(self):  # Override parent method
        return f"{self.name} flies in the sky"

# Demonstrating polymorphism
animals = [
    Dog("Buddy"),
    Cat("Whiskers"),
    Bird("Tweety"),
    Animal("Generic Animal")
]

# Same method name, different behaviors
for animal in animals:
    print(animal.make_sound())
    print(animal.move())
    print(animal.sleep())  # This method is not overridden, so uses parent implementation
    print("-" * 30)

# Output:
# Buddy barks: Woof! Woof!
# Buddy runs on four legs
# Buddy is sleeping
# ------------------------------
# Whiskers meows: Meow!
# Whiskers walks silently
# Whiskers is sleeping
# ------------------------------
# Tweety chirps: Tweet! Tweet!
# Tweety flies in the sky
# Tweety is sleeping
# ------------------------------
# Generic Animal makes a generic animal sound
# Generic Animal moves
# Generic Animal is sleeping
```

## `DUCK TYPING`

Duck typing is a concept where "if it walks like a duck and quacks like a duck, then it's a duck." Python uses duck typing to achieve polymorphism.

```python
class Duck:
    def speak(self):
        return "Quack!"

    def fly(self):
        return "Duck is flying"

class Airplane:
    def speak(self):
        return "Airplane doesn't speak, but makes noise"

    def fly(self):
        return "Airplane is flying at 30,000 feet"

class Bird:
    def speak(self):
        return "Chirp!"

    def fly(self):
        return "Bird is flying gracefully"

def make_it_fly(obj):
    """Function that works with any object that has a fly() method"""
    print(obj.fly())

def make_it_speak_and_fly(obj):
    """Function that works with any object that has both speak() and fly() methods"""
    print(obj.speak())
    print(obj.fly())

# Duck typing in action - all these objects can fly
duck = Duck()
plane = Airplane()
bird = Bird()

print("Making different objects fly:")
make_it_fly(duck)    # Output: Duck is flying
make_it_fly(plane)   # Output: Airplane is flying at 30,000 feet
make_it_fly(bird)    # Output: Bird is flying gracefully

print("\nMaking objects speak and fly:")
flying_objects = [duck, plane, bird]
for obj in flying_objects:
    make_it_speak_and_fly(obj)
    print("-" * 25)
```

## `COMPREHENSIVE OPERATOR OVERLOADING`

Operator overloading allows you to define how operators work with your custom objects by implementing special methods (dunder methods).

### **Arithmetic Operators**

```python
class Money:
    def __init__(self, amount, currency="USD"):
        self.amount = amount
        self.currency = currency

    def __add__(self, other):
        """Addition: money1 + money2"""
        if isinstance(other, Money):
            if self.currency != other.currency:
                raise ValueError(f"Cannot add {self.currency} and {other.currency}")
            return Money(self.amount + other.amount, self.currency)
        elif isinstance(other, (int, float)):
            return Money(self.amount + other, self.currency)
        return NotImplemented

    def __radd__(self, other):
        """Reverse addition: 100 + money"""
        return self.__add__(other)

    def __sub__(self, other):
        """Subtraction: money1 - money2"""
        if isinstance(other, Money):
            if self.currency != other.currency:
                raise ValueError(f"Cannot subtract {other.currency} from {self.currency}")
            return Money(self.amount - other.amount, self.currency)
        elif isinstance(other, (int, float)):
            return Money(self.amount - other, self.currency)
        return NotImplemented

    def __rsub__(self, other):
        """Reverse subtraction: 100 - money"""
        if isinstance(other, (int, float)):
            return Money(other - self.amount, self.currency)
        return NotImplemented

    def __mul__(self, other):
        """Multiplication: money * scalar"""
        if isinstance(other, (int, float)):
            return Money(self.amount * other, self.currency)
        return NotImplemented

    def __rmul__(self, other):
        """Reverse multiplication: scalar * money"""
        return self.__mul__(other)

    def __truediv__(self, other):
        """Division: money / scalar"""
        if isinstance(other, (int, float)):
            if other == 0:
                raise ZeroDivisionError("Cannot divide by zero")
            return Money(self.amount / other, self.currency)
        elif isinstance(other, Money):
            if self.currency != other.currency:
                raise ValueError(f"Cannot divide {self.currency} by {other.currency}")
            return self.amount / other.amount  # Returns ratio, not Money
        return NotImplemented

    def __floordiv__(self, other):
        """Floor division: money // scalar"""
        if isinstance(other, (int, float)):
            if other == 0:
                raise ZeroDivisionError("Cannot divide by zero")
            return Money(self.amount // other, self.currency)
        return NotImplemented

    def __mod__(self, other):
        """Modulo: money % scalar"""
        if isinstance(other, (int, float)):
            return Money(self.amount % other, self.currency)
        return NotImplemented

    def __pow__(self, other):
        """Power: money ** power (compound interest)"""
        if isinstance(other, (int, float)):
            return Money(self.amount ** other, self.currency)
        return NotImplemented

    def __neg__(self):
        """Unary minus: -money"""
        return Money(-self.amount, self.currency)

    def __pos__(self):
        """Unary plus: +money"""
        return Money(self.amount, self.currency)

    def __abs__(self):
        """Absolute value: abs(money)"""
        return Money(abs(self.amount), self.currency)

    def __str__(self):
        return f"{self.currency} {self.amount:.2f}"

    def __repr__(self):
        return f"Money({self.amount}, '{self.currency}')"

# Usage examples - Arithmetic Operations
usd_100 = Money(100, "USD")
usd_50 = Money(50, "USD")

print(f"Original: {usd_100}")                    # USD 100.00
print(f"Addition: {usd_100 + usd_50}")           # USD 150.00
print(f"Subtraction: {usd_100 - usd_50}")        # USD 50.00
print(f"Multiplication: {usd_100 * 1.5}")        # USD 150.00
print(f"Reverse multiplication: {2 * usd_50}")   # USD 100.00
print(f"Division: {usd_100 / 2}")                # USD 50.00
print(f"Floor division: {usd_100 // 3}")         # USD 33.00
print(f"Modulo: {usd_100 % 30}")                 # USD 10.00
print(f"Power (compound): {Money(100) ** 1.05}") # USD 105.00
print(f"Negative: {-usd_100}")                   # USD -100.00
print(f"Absolute: {abs(Money(-50))}")            # USD 50.00

# Ratio calculation
ratio = usd_100 / usd_50
print(f"Ratio: {ratio}")                         # 2.0
```

### **Comparison Operators**

```python
class Grade:
    def __init__(self, score, max_score=100):
        self.score = score
        self.max_score = max_score

    @property
    def percentage(self):
        return (self.score / self.max_score) * 100

    def __eq__(self, other):
        """Equal to (==)"""
        if isinstance(other, Grade):
            return self.percentage == other.percentage
        elif isinstance(other, (int, float)):
            return self.score == other
        return False

    def __ne__(self, other):
        """Not equal to (!=)"""
        return not self.__eq__(other)

    def __lt__(self, other):
        """Less than (<)"""
        if isinstance(other, Grade):
            return self.percentage < other.percentage
        elif isinstance(other, (int, float)):
            return self.score < other
        return NotImplemented

    def __le__(self, other):
        """Less than or equal to (<=)"""
        if isinstance(other, Grade):
            return self.percentage <= other.percentage
        elif isinstance(other, (int, float)):
            return self.score <= other
        return NotImplemented

    def __gt__(self, other):
        """Greater than (>)"""
        if isinstance(other, Grade):
            return self.percentage > other.percentage
        elif isinstance(other, (int, float)):
            return self.score > other
        return NotImplemented

    def __ge__(self, other):
        """Greater than or equal to (>=)"""
        if isinstance(other, Grade):
            return self.percentage >= other.percentage
        elif isinstance(other, (int, float)):
            return self.score >= other
        return NotImplemented

    def __hash__(self):
        """Make hashable for use in sets/dicts"""
        return hash((self.score, self.max_score))

    def letter_grade(self):
        """Convert to letter grade"""
        percentage = self.percentage
        if percentage >= 90:
            return 'A'
        elif percentage >= 80:
            return 'B'
        elif percentage >= 70:
            return 'C'
        elif percentage >= 60:
            return 'D'
        else:
            return 'F'

    def __str__(self):
        return f"{self.score}/{self.max_score} ({self.percentage:.1f}%) - {self.letter_grade()}"

# Usage examples - Comparison Operations
grade1 = Grade(85, 100)  # 85%
grade2 = Grade(42, 50)   # 84%
grade3 = Grade(85, 100)  # 85%

print(f"Grade 1: {grade1}")  # 85/100 (85.0%) - B
print(f"Grade 2: {grade2}")  # 42/50 (84.0%) - B
print(f"Grade 3: {grade3}")  # 85/100 (85.0%) - B

print(f"grade1 == grade3: {grade1 == grade3}")  # True (same percentage)
print(f"grade1 == grade2: {grade1 == grade2}")  # False (different percentage)
print(f"grade1 > grade2: {grade1 > grade2}")    # True (85% > 84%)
print(f"grade1 >= 85: {grade1 >= 85}")          # True (score >= 85)

# Can be used in sorting
grades = [grade2, grade1, grade3, Grade(95), Grade(78)]
sorted_grades = sorted(grades, reverse=True)
print("\nSorted grades (highest to lowest):")
for grade in sorted_grades:
    print(f"  {grade}")

# Can be used in sets (requires __hash__ and __eq__)
unique_grades = {grade1, grade2, grade3}  # grade1 and grade3 considered equal
print(f"\nUnique grades: {len(unique_grades)}")  # 2 (grade1 and grade3 are equal)
```

### **Container and Iteration Operators**

```python
class Matrix:
    def __init__(self, rows, cols, default_value=0):
        self.rows = rows
        self.cols = cols
        self._data = [[default_value for _ in range(cols)] for _ in range(rows)]

    def __getitem__(self, key):
        """Support for matrix[row, col] or matrix[row]"""
        if isinstance(key, tuple):
            row, col = key
            return self._data[row][col]
        else:
            return self._data[key]  # Return entire row

    def __setitem__(self, key, value):
        """Support for matrix[row, col] = value or matrix[row] = [values]"""
        if isinstance(key, tuple):
            row, col = key
            self._data[row][col] = value
        else:
            if len(value) != self.cols:
                raise ValueError(f"Row must have exactly {self.cols} elements")
            self._data[key] = list(value)

    def __len__(self):
        """Return number of rows"""
        return self.rows

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

    def __bool__(self):
        """False if all elements are 0 or falsy"""
        for row in self._data:
            for element in row:
                if element:
                    return True
        return False

    def __add__(self, other):
        """Matrix addition"""
        if isinstance(other, Matrix):
            if self.rows != other.rows or self.cols != other.cols:
                raise ValueError("Matrices must have same dimensions")
            result = Matrix(self.rows, self.cols)
            for i in range(self.rows):
                for j in range(self.cols):
                    result[i, j] = self[i, j] + other[i, j]
            return result
        elif isinstance(other, (int, float)):
            # Scalar addition
            result = Matrix(self.rows, self.cols)
            for i in range(self.rows):
                for j in range(self.cols):
                    result[i, j] = self[i, j] + other
            return result
        return NotImplemented

    def __mul__(self, other):
        """Matrix multiplication or scalar multiplication"""
        if isinstance(other, (int, float)):
            # Scalar multiplication
            result = Matrix(self.rows, self.cols)
            for i in range(self.rows):
                for j in range(self.cols):
                    result[i, j] = self[i, j] * other
            return result
        elif isinstance(other, Matrix):
            # Matrix multiplication
            if self.cols != other.rows:
                raise ValueError("Cannot multiply: columns of first matrix must equal rows of second")
            result = Matrix(self.rows, other.cols)
            for i in range(self.rows):
                for j in range(other.cols):
                    sum_product = 0
                    for k in range(self.cols):
                        sum_product += self[i, k] * other[k, j]
                    result[i, j] = sum_product
            return result
        return NotImplemented

    def __str__(self):
        lines = []
        for row in self._data:
            line = " ".join(f"{elem:6.2f}" for elem in row)
            lines.append(f"[{line}]")
        return "\n".join(lines)

    def __repr__(self):
        return f"Matrix({self.rows}, {self.cols})"

# Usage examples - Container Operations
matrix = Matrix(3, 3)

# Setting values
matrix[0, 0] = 1
matrix[0, 1] = 2
matrix[0, 2] = 3
matrix[1] = [4, 5, 6]  # Set entire row
matrix[2] = [7, 8, 9]

print("Original Matrix:")
print(matrix)
print(f"Matrix dimensions: {len(matrix)} rows")
print(f"Element at [1,1]: {matrix[1, 1]}")    # 5
print(f"Row 0: {matrix[0]}")                  # [1, 2, 3]
print(f"Contains 5: {5 in matrix}")           # True
print(f"Contains 10: {10 in matrix}")         # False

# Matrix operations
matrix2 = Matrix(3, 3, 1)  # All elements are 1
matrix2[0] = [1, 0, 1]
matrix2[1] = [0, 1, 0]
matrix2[2] = [1, 0, 1]

print("\nSecond Matrix:")
print(matrix2)

# Addition
print("\nMatrix Addition:")
result = matrix + matrix2
print(result)

# Scalar multiplication
print("\nScalar Multiplication (* 2):")
result = matrix * 2
print(result)

# Iteration over rows
print("\nIterating over rows:")
for i, row in enumerate(matrix):
    print(f"Row {i}: {row}")
```

### **Bitwise and Logical Operators**

```python
class BitSet:
    """A simple bit set implementation demonstrating bitwise operators"""

    def __init__(self, value=0):
        self._value = value

    def __and__(self, other):
        """Bitwise AND (&)"""
        if isinstance(other, BitSet):
            return BitSet(self._value & other._value)
        elif isinstance(other, int):
            return BitSet(self._value & other)
        return NotImplemented

    def __or__(self, other):
        """Bitwise OR (|)"""
        if isinstance(other, BitSet):
            return BitSet(self._value | other._value)
        elif isinstance(other, int):
            return BitSet(self._value | other)
        return NotImplemented

    def __xor__(self, other):
        """Bitwise XOR (^)"""
        if isinstance(other, BitSet):
            return BitSet(self._value ^ other._value)
        elif isinstance(other, int):
            return BitSet(self._value ^ other)
        return NotImplemented

    def __lshift__(self, other):
        """Left shift (<<)"""
        if isinstance(other, int):
            return BitSet(self._value << other)
        return NotImplemented

    def __rshift__(self, other):
        """Right shift (>>)"""
        if isinstance(other, int):
            return BitSet(self._value >> other)
        return NotImplemented

    def __invert__(self):
        """Bitwise NOT (~)"""
        # Limit to 32 bits for demonstration
        return BitSet(~self._value & 0xFFFFFFFF)

    def __iand__(self, other):
        """In-place AND (&=)"""
        if isinstance(other, BitSet):
            self._value &= other._value
        elif isinstance(other, int):
            self._value &= other
        else:
            return NotImplemented
        return self

    def __ior__(self, other):
        """In-place OR (|=)"""
        if isinstance(other, BitSet):
            self._value |= other._value
        elif isinstance(other, int):
            self._value |= other
        else:
            return NotImplemented
        return self

    def __str__(self):
        return f"BitSet({bin(self._value)})"

    def __repr__(self):
        return f"BitSet({self._value})"

# Usage examples - Bitwise Operations
bits1 = BitSet(0b1010)  # 10 in decimal
bits2 = BitSet(0b1100)  # 12 in decimal

print(f"bits1: {bits1}")              # BitSet(0b1010)
print(f"bits2: {bits2}")              # BitSet(0b1100)
print(f"AND: {bits1 & bits2}")        # BitSet(0b1000) - 8
print(f"OR: {bits1 | bits2}")         # BitSet(0b1110) - 14
print(f"XOR: {bits1 ^ bits2}")        # BitSet(0b110) - 6
print(f"NOT bits1: {~bits1}")         # BitSet with inverted bits
print(f"Left shift: {bits1 << 2}")    # BitSet(0b101000) - 40
print(f"Right shift: {bits1 >> 1}")   # BitSet(0b101) - 5

# In-place operations
bits3 = BitSet(0b1111)
print(f"Original bits3: {bits3}")     # BitSet(0b1111)
bits3 &= 0b1010
print(f"After &= 0b1010: {bits3}")    # BitSet(0b1010)
```

## `POLYMORPHISM WITH INHERITANCE`

```python
class Shape:
    def __init__(self, name):
        self.name = name

    def area(self):
        raise NotImplementedError("Subclass must implement area method")

    def perimeter(self):
        raise NotImplementedError("Subclass must implement perimeter method")

    def display_info(self):
        return f"{self.name} - Area: {self.area():.2f}, Perimeter: {self.perimeter():.2f}"

class Rectangle(Shape):
    def __init__(self, width, height):
        super().__init__("Rectangle")
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        super().__init__("Circle")
        self.radius = radius

    def area(self):
        import math
        return math.pi * self.radius ** 2

    def perimeter(self):
        import math
        return 2 * math.pi * self.radius

class Triangle(Shape):
    def __init__(self, a, b, c):
        super().__init__("Triangle")
        self.a = a
        self.b = b
        self.c = c

    def area(self):
        # Using Heron's formula
        s = (self.a + self.b + self.c) / 2
        return (s * (s - self.a) * (s - self.b) * (s - self.c)) ** 0.5

    def perimeter(self):
        return self.a + self.b + self.c

def calculate_total_area(shapes):
    """Function that works with any Shape object"""
    total_area = 0
    for shape in shapes:
        total_area += shape.area()  # Polymorphic method call
    return total_area

def print_shape_details(shapes):
    """Function that displays details of any Shape object"""
    for shape in shapes:
        print(shape.display_info())  # Polymorphic method call

# Using polymorphism with inheritance
shapes = [
    Rectangle(5, 3),
    Circle(4),
    Triangle(3, 4, 5),
    Rectangle(2, 8)
]

print("Shape Details:")
print_shape_details(shapes)
print(f"\nTotal area of all shapes: {calculate_total_area(shapes):.2f}")

# Output:
# Shape Details:
# Rectangle - Area: 15.00, Perimeter: 16.00
# Circle - Area: 50.27, Perimeter: 25.13
# Triangle - Area: 6.00, Perimeter: 12.00
# Rectangle - Area: 16.00, Perimeter: 20.00
#
# Total area of all shapes: 87.27
```

## `POLYMORPHISM WITH INTERFACES (PROTOCOLS)`

Using typing.Protocol to define interfaces (Python 3.8+):

```python
from typing import Protocol

class Drawable(Protocol):
    """Protocol defining what it means to be drawable"""
    def draw(self) -> str:
        ...

    def get_area(self) -> float:
        ...

class Square:
    def __init__(self, side):
        self.side = side

    def draw(self) -> str:
        return f"Drawing a square with side {self.side}"

    def get_area(self) -> float:
        return self.side ** 2

class Circle:
    def __init__(self, radius):
        self.radius = radius

    def draw(self) -> str:
        return f"Drawing a circle with radius {self.radius}"

    def get_area(self) -> float:
        import math
        return math.pi * self.radius ** 2

class Text:
    def __init__(self, content):
        self.content = content

    def draw(self) -> str:
        return f"Drawing text: '{self.content}'"

    def get_area(self) -> float:
        return len(self.content) * 10  # Arbitrary area calculation for text

def render_drawable(drawable: Drawable) -> None:
    """Function that works with any object implementing Drawable protocol"""
    print(drawable.draw())
    print(f"Area: {drawable.get_area():.2f}")

def render_multiple_drawables(drawables: list[Drawable]) -> None:
    """Function that renders multiple drawable objects"""
    total_area = 0
    for drawable in drawables:
        render_drawable(drawable)
        total_area += drawable.get_area()
        print("-" * 30)
    print(f"Total area: {total_area:.2f}")

# Using protocol-based polymorphism
drawables = [
    Square(5),
    Circle(3),
    Text("Hello World")
]

render_multiple_drawables(drawables)
```

## `PRACTICAL EXAMPLE: PAYMENT SYSTEM`

```python
from abc import ABC, abstractmethod
from typing import Protocol
import datetime

class PaymentProcessor(ABC):
    """Abstract base class for payment processors"""

    def __init__(self, fee_percentage):
        self.fee_percentage = fee_percentage

    @abstractmethod
    def process_payment(self, amount: float) -> dict:
        """Process payment and return transaction details"""
        pass

    @abstractmethod
    def refund_payment(self, transaction_id: str, amount: float) -> dict:
        """Process refund and return refund details"""
        pass

    def calculate_fee(self, amount: float) -> float:
        """Calculate processing fee"""
        return amount * (self.fee_percentage / 100)

    def validate_amount(self, amount: float) -> bool:
        """Validate payment amount"""
        return amount > 0 and amount <= 10000  # Max $10,000 per transaction

class CreditCardProcessor(PaymentProcessor):
    def __init__(self):
        super().__init__(2.5)  # 2.5% fee

    def process_payment(self, amount: float) -> dict:
        if not self.validate_amount(amount):
            return {"status": "failed", "error": "Invalid amount"}

        fee = self.calculate_fee(amount)
        net_amount = amount - fee

        # Simulate credit card processing
        transaction_id = f"CC_{datetime.datetime.now().strftime('%Y%m%d_%H%M%S')}"

        return {
            "status": "success",
            "transaction_id": transaction_id,
            "amount": amount,
            "fee": fee,
            "net_amount": net_amount,
            "processor": "Credit Card"
        }

    def refund_payment(self, transaction_id: str, amount: float) -> dict:
        return {
            "status": "success",
            "refund_id": f"REF_{transaction_id}",
            "amount": amount,
            "processor": "Credit Card"
        }

class PayPalProcessor(PaymentProcessor):
    def __init__(self):
        super().__init__(3.0)  # 3.0% fee

    def process_payment(self, amount: float) -> dict:
        if not self.validate_amount(amount):
            return {"status": "failed", "error": "Invalid amount"}

        fee = self.calculate_fee(amount)
        net_amount = amount - fee

        # Simulate PayPal processing
        transaction_id = f"PP_{datetime.datetime.now().strftime('%Y%m%d_%H%M%S')}"

        return {
            "status": "success",
            "transaction_id": transaction_id,
            "amount": amount,
            "fee": fee,
            "net_amount": net_amount,
            "processor": "PayPal"
        }

    def refund_payment(self, transaction_id: str, amount: float) -> dict:
        return {
            "status": "success",
            "refund_id": f"REF_{transaction_id}",
            "amount": amount,
            "processor": "PayPal"
        }

class BankTransferProcessor(PaymentProcessor):
    def __init__(self):
        super().__init__(1.0)  # 1.0% fee (lowest fee)

    def process_payment(self, amount: float) -> dict:
        if not self.validate_amount(amount):
            return {"status": "failed", "error": "Invalid amount"}

        fee = self.calculate_fee(amount)
        net_amount = amount - fee

        # Simulate bank transfer processing
        transaction_id = f"BT_{datetime.datetime.now().strftime('%Y%m%d_%H%M%S')}"

        return {
            "status": "success",
            "transaction_id": transaction_id,
            "amount": amount,
            "fee": fee,
            "net_amount": net_amount,
            "processor": "Bank Transfer"
        }

    def refund_payment(self, transaction_id: str, amount: float) -> dict:
        # Bank transfers take longer to refund
        return {
            "status": "pending",
            "refund_id": f"REF_{transaction_id}",
            "amount": amount,
            "processor": "Bank Transfer",
            "note": "Bank transfer refunds take 3-5 business days"
        }

class PaymentGateway:
    """Payment gateway that uses different processors"""

    def __init__(self):
        self.processors = {
            "credit_card": CreditCardProcessor(),
            "paypal": PayPalProcessor(),
            "bank_transfer": BankTransferProcessor()
        }

    def process_payment(self, processor_type: str, amount: float) -> dict:
        """Process payment using specified processor"""
        if processor_type not in self.processors:
            return {"status": "failed", "error": "Unknown processor type"}

        processor = self.processors[processor_type]
        return processor.process_payment(amount)

    def refund_payment(self, processor_type: str, transaction_id: str, amount: float) -> dict:
        """Process refund using specified processor"""
        if processor_type not in self.processors:
            return {"status": "failed", "error": "Unknown processor type"}

        processor = self.processors[processor_type]
        return processor.refund_payment(transaction_id, amount)

    def get_best_processor(self, amount: float) -> str:
        """Find processor with lowest fee for given amount"""
        best_processor = None
        lowest_fee = float('inf')

        for name, processor in self.processors.items():
            fee = processor.calculate_fee(amount)
            if fee < lowest_fee:
                lowest_fee = fee
                best_processor = name

        return best_processor

def process_multiple_payments(gateway: PaymentGateway, payments: list) -> None:
    """Process multiple payments using polymorphism"""
    total_processed = 0
    total_fees = 0

    for payment in payments:
        processor_type = payment["processor"]
        amount = payment["amount"]

        print(f"Processing ${amount} via {processor_type}...")
        result = gateway.process_payment(processor_type, amount)

        if result["status"] == "success":
            total_processed += result["net_amount"]
            total_fees += result["fee"]
            print(f"✓ Success: Transaction ID {result['transaction_id']}")
            print(f"  Fee: ${result['fee']:.2f}, Net: ${result['net_amount']:.2f}")
        else:
            print(f"✗ Failed: {result['error']}")

        print("-" * 50)

    print(f"Total processed: ${total_processed:.2f}")
    print(f"Total fees: ${total_fees:.2f}")

# Using the polymorphic payment system
gateway = PaymentGateway()

# Process payments using different processors
payments = [
    {"processor": "credit_card", "amount": 100.00},
    {"processor": "paypal", "amount": 250.00},
    {"processor": "bank_transfer", "amount": 500.00},
    {"processor": "credit_card", "amount": 75.00}
]

process_multiple_payments(gateway, payments)

# Find best processor for a specific amount
amount = 1000.00
best_processor = gateway.get_best_processor(amount)
print(f"\nBest processor for ${amount}: {best_processor}")

# Process payment with best processor
result = gateway.process_payment(best_processor, amount)
print(f"Result: {result}")
```

## `BENEFITS OF POLYMORPHISM`

1. **Code Reusability**: Write functions that work with multiple types
2. **Flexibility**: Easy to add new types without changing existing code
3. **Maintainability**: Changes to implementation don't affect client code
4. **Extensibility**: New classes can be added that work with existing functions

## `COMMON POLYMORPHISM QUESTIONS`

1. **Q: What's the difference between method overriding and method overloading?**

   - **A**: Python doesn't support method overloading (same method name with different parameters). It only supports method overriding (redefining parent class methods).

2. **Q: What is duck typing?**

   - **A**: Duck typing allows objects to be used based on their behavior (methods/attributes) rather than their type.

3. **Q: How does Python achieve polymorphism?**

   - **A**: Through method overriding, duck typing, and operator overloading using special methods.

4. **Q: What are magic methods (dunder methods)?**

   - **A**: Special methods with double underscores that allow operator overloading and define object behavior (`__init__`, `__str__`, `__add__`, etc.).

5. **Q: When should you use polymorphism?**
   - **A**: When you want to write code that works with multiple types, implement common interfaces, or create flexible and extensible systems.
