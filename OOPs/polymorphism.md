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

## `OPERATOR OVERLOADING`

Python allows you to define how operators work with your custom objects by implementing special methods.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        """Override + operator"""
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        return NotImplemented

    def __sub__(self, other):
        """Override - operator"""
        if isinstance(other, Vector):
            return Vector(self.x - other.x, self.y - other.y)
        return NotImplemented

    def __mul__(self, scalar):
        """Override * operator for scalar multiplication"""
        if isinstance(scalar, (int, float)):
            return Vector(self.x * scalar, self.y * scalar)
        return NotImplemented

    def __eq__(self, other):
        """Override == operator"""
        if isinstance(other, Vector):
            return self.x == other.x and self.y == other.y
        return False

    def __lt__(self, other):
        """Override < operator (comparing magnitudes)"""
        if isinstance(other, Vector):
            return self.magnitude() < other.magnitude()
        return NotImplemented

    def __str__(self):
        """String representation for users"""
        return f"Vector({self.x}, {self.y})"

    def __repr__(self):
        """String representation for developers"""
        return f"Vector({self.x}, {self.y})"

    def magnitude(self):
        """Calculate vector magnitude"""
        return (self.x**2 + self.y**2)**0.5

# Using operator overloading
v1 = Vector(3, 4)
v2 = Vector(1, 2)
v3 = Vector(3, 4)

print(f"v1 = {v1}")          # Output: v1 = Vector(3, 4)
print(f"v2 = {v2}")          # Output: v2 = Vector(1, 2)

# Addition
v_sum = v1 + v2
print(f"v1 + v2 = {v_sum}")  # Output: v1 + v2 = Vector(4, 6)

# Subtraction
v_diff = v1 - v2
print(f"v1 - v2 = {v_diff}") # Output: v1 - v2 = Vector(2, 2)

# Scalar multiplication
v_scaled = v1 * 2
print(f"v1 * 2 = {v_scaled}")# Output: v1 * 2 = Vector(6, 8)

# Equality comparison
print(f"v1 == v2: {v1 == v2}")  # Output: v1 == v2: False
print(f"v1 == v3: {v1 == v3}")  # Output: v1 == v3: True

# Less than comparison
print(f"v2 < v1: {v2 < v1}")    # Output: v2 < v1: True (smaller magnitude)
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
