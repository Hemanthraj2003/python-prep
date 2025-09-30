# INHERITANCE

- Inheritance is a fundamental concept in OOP that allows a class to inherit attributes and methods from another class.
- The class that inherits is called the **child class** or **derived class**.
- The class being inherited from is called the **parent class** or **base class**.
- Inheritance promotes code reusability and establishes a hierarchical relationship between classes.
- **Key Concept**: Inheritance implements the "is-a" relationship (e.g., Dog is-a Animal).

## `BASIC INHERITANCE`

```python
# Parent class (Base class)
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species
        self.is_alive = True

    def eat(self):
        return f"{self.name} is eating"

    def sleep(self):
        return f"{self.name} is sleeping"

    def make_sound(self):
        return f"{self.name} makes a sound"

# Child class (Derived class)
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, "Canine")  # Call parent constructor
        self.breed = breed

    def make_sound(self):  # Method overriding
        return f"{self.name} barks: Woof!"

    def fetch(self):  # New method specific to Dog
        return f"{self.name} is fetching the ball"

# Another child class
class Cat(Animal):
    def __init__(self, name, color):
        super().__init__(name, "Feline")
        self.color = color

    def make_sound(self):  # Method overriding
        return f"{self.name} meows: Meow!"

    def climb(self):  # New method specific to Cat
        return f"{self.name} is climbing a tree"

# Using inheritance
dog = Dog("Buddy", "Golden Retriever")
cat = Cat("Whiskers", "Orange")

print(dog.name)         # Output: Buddy (inherited attribute)
print(dog.species)      # Output: Canine (set by parent constructor)
print(dog.breed)        # Output: Golden Retriever (child-specific attribute)

print(dog.eat())        # Output: Buddy is eating (inherited method)
print(dog.make_sound()) # Output: Buddy barks: Woof! (overridden method)
print(dog.fetch())      # Output: Buddy is fetching the ball (child-specific method)

print(cat.make_sound()) # Output: Whiskers meows: Meow! (overridden method)
print(cat.climb())      # Output: Whiskers is climbing a tree (child-specific method)
```

## `THE super() FUNCTION`

The `super()` function allows you to access methods and attributes from the parent class:

```python
class Vehicle:
    def __init__(self, brand, model, year):
        self.brand = brand
        self.model = model
        self.year = year
        self.mileage = 0

    def start_engine(self):
        return f"{self.brand} {self.model} engine started"

    def drive(self, miles):
        self.mileage += miles
        return f"Drove {miles} miles. Total mileage: {self.mileage}"

    def get_info(self):
        return f"{self.year} {self.brand} {self.model}"

class Car(Vehicle):
    def __init__(self, brand, model, year, doors):
        super().__init__(brand, model, year)  # Call parent constructor
        self.doors = doors
        self.fuel_level = 100

    def start_engine(self):
        # Call parent method and extend it
        parent_message = super().start_engine()
        return f"{parent_message} - Car is ready to drive"

    def drive(self, miles):
        if self.fuel_level > 0:
            self.fuel_level -= miles * 0.1  # Consume fuel
            return super().drive(miles) + f" - Fuel level: {self.fuel_level:.1f}%"
        else:
            return "Cannot drive - out of fuel!"

    def get_info(self):
        # Call parent method and extend it
        parent_info = super().get_info()
        return f"{parent_info} - {self.doors} doors"

class Motorcycle(Vehicle):
    def __init__(self, brand, model, year, engine_size):
        super().__init__(brand, model, year)
        self.engine_size = engine_size
        self.has_sidecar = False

    def wheelie(self):
        return f"{self.brand} {self.model} is doing a wheelie!"

    def get_info(self):
        parent_info = super().get_info()
        return f"{parent_info} - {self.engine_size}cc motorcycle"

# Using super() function
car = Car("Toyota", "Camry", 2022, 4)
bike = Motorcycle("Harley-Davidson", "Street 750", 2021, 750)

print(car.start_engine())   # Output: Toyota Camry engine started - Car is ready to drive
print(car.drive(50))        # Output: Drove 50 miles. Total mileage: 50 - Fuel level: 95.0%
print(car.get_info())       # Output: 2022 Toyota Camry - 4 doors

print(bike.get_info())      # Output: 2021 Harley-Davidson Street 750 - 750cc motorcycle
print(bike.wheelie())       # Output: Harley-Davidson Street 750 is doing a wheelie!
```

## `MULTIPLE INHERITANCE`

Python supports multiple inheritance, where a class can inherit from multiple parent classes:

```python
class Flyable:
    def __init__(self):
        self.can_fly = True
        self.altitude = 0

    def fly(self):
        self.altitude = 1000
        return f"Flying at {self.altitude} feet"

    def land(self):
        self.altitude = 0
        return "Landed successfully"

class Swimmable:
    def __init__(self):
        self.can_swim = True
        self.depth = 0

    def swim(self):
        self.depth = 10
        return f"Swimming at {self.depth} feet depth"

    def surface(self):
        self.depth = 0
        return "Surfaced"

class Bird(Animal):
    def __init__(self, name, wingspan):
        super().__init__(name, "Avian")
        self.wingspan = wingspan

class Duck(Bird, Flyable, Swimmable):
    def __init__(self, name, wingspan):
        Bird.__init__(self, name, wingspan)
        Flyable.__init__(self)
        Swimmable.__init__(self)

    def make_sound(self):
        return f"{self.name} quacks: Quack!"

    def migrate(self):
        return f"{self.name} is migrating south"

# Using multiple inheritance
duck = Duck("Donald", 24)

print(duck.name)            # Output: Donald (from Animal via Bird)
print(duck.species)         # Output: Avian (from Animal via Bird)
print(duck.wingspan)        # Output: 24 (from Bird)
print(duck.can_fly)         # Output: True (from Flyable)
print(duck.can_swim)        # Output: True (from Swimmable)

print(duck.make_sound())    # Output: Donald quacks: Quack! (Duck's method)
print(duck.eat())           # Output: Donald is eating (from Animal)
print(duck.fly())           # Output: Flying at 1000 feet (from Flyable)
print(duck.swim())          # Output: Swimming at 10 feet depth (from Swimmable)
print(duck.migrate())       # Output: Donald is migrating south (Duck's method)
```

## `METHOD RESOLUTION ORDER (MRO)`

Python uses the Method Resolution Order to determine which method to call in case of multiple inheritance:

```python
class A:
    def method(self):
        return "Method from A"

class B(A):
    def method(self):
        return "Method from B"

class C(A):
    def method(self):
        return "Method from C"

class D(B, C):
    pass

# Check the Method Resolution Order
print(D.__mro__)
# Output: (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)

# Also available as:
print(D.mro())

d = D()
print(d.method())  # Output: Method from B (B comes before C in MRO)

# Diamond Problem Example
class Animal:
    def __init__(self, name):
        self.name = name
        print(f"Animal.__init__({name})")

    def speak(self):
        return f"{self.name} makes a sound"

class Mammal(Animal):
    def __init__(self, name):
        super().__init__(name)
        print(f"Mammal.__init__({name})")

    def feed_milk(self):
        return f"{self.name} feeds milk to babies"

class Carnivore(Animal):
    def __init__(self, name):
        super().__init__(name)
        print(f"Carnivore.__init__({name})")

    def hunt(self):
        return f"{self.name} is hunting"

class Lion(Mammal, Carnivore):
    def __init__(self, name):
        super().__init__(name)
        print(f"Lion.__init__({name})")

    def roar(self):
        return f"{self.name} roars loudly"

# Creating a Lion object
lion = Lion("Simba")
# Output:
# Animal.__init__(Simba)
# Carnivore.__init__(Simba)
# Mammal.__init__(Simba)
# Lion.__init__(Simba)

print(Lion.__mro__)
# Shows the method resolution order
```

## `TYPES OF INHERITANCE`

### 1. **Single Inheritance**

One child class inherits from one parent class.

```python
class Parent:
    def parent_method(self):
        return "This is from parent"

class Child(Parent):
    def child_method(self):
        return "This is from child"

child = Child()
print(child.parent_method())  # Inherited method
print(child.child_method())   # Own method
```

### 2. **Multiple Inheritance**

One child class inherits from multiple parent classes.

```python
class Father:
    def father_trait(self):
        return "Father's trait"

class Mother:
    def mother_trait(self):
        return "Mother's trait"

class Child(Father, Mother):
    def child_trait(self):
        return "Child's trait"

child = Child()
print(child.father_trait())  # From Father
print(child.mother_trait())  # From Mother
print(child.child_trait())   # Own trait
```

### 3. **Multilevel Inheritance**

Classes form a hierarchy where each class inherits from the previous one.

```python
class GrandParent:
    def grandparent_method(self):
        return "GrandParent method"

class Parent(GrandParent):
    def parent_method(self):
        return "Parent method"

class Child(Parent):
    def child_method(self):
        return "Child method"

child = Child()
print(child.grandparent_method())  # From GrandParent
print(child.parent_method())       # From Parent
print(child.child_method())        # Own method
```

### 4. **Hierarchical Inheritance**

Multiple child classes inherit from the same parent class.

```python
class Animal:
    def eat(self):
        return "Animal is eating"

class Dog(Animal):
    def bark(self):
        return "Dog is barking"

class Cat(Animal):
    def meow(self):
        return "Cat is meowing"

class Bird(Animal):
    def fly(self):
        return "Bird is flying"

dog = Dog()
cat = Cat()
bird = Bird()

print(dog.eat())   # All inherit eat() from Animal
print(cat.eat())
print(bird.eat())
```

## `ABSTRACT BASE CLASSES`

Abstract base classes define a common interface that subclasses must implement:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    def __init__(self, name):
        self.name = name

    @abstractmethod
    def area(self):
        """Abstract method - must be implemented by subclasses"""
        pass

    @abstractmethod
    def perimeter(self):
        """Abstract method - must be implemented by subclasses"""
        pass

    def description(self):
        """Concrete method - can be used by subclasses"""
        return f"This is a {self.name}"

class Rectangle(Shape):
    def __init__(self, width, height):
        super().__init__("Rectangle")
        self.width = width
        self.height = height

    def area(self):
        """Implementation of abstract method"""
        return self.width * self.height

    def perimeter(self):
        """Implementation of abstract method"""
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        super().__init__("Circle")
        self.radius = radius

    def area(self):
        """Implementation of abstract method"""
        import math
        return math.pi * self.radius ** 2

    def perimeter(self):
        """Implementation of abstract method"""
        import math
        return 2 * math.pi * self.radius

# Cannot instantiate abstract class
# shape = Shape("Generic")  # TypeError: Can't instantiate abstract class

# Can instantiate concrete subclasses
rectangle = Rectangle(5, 3)
circle = Circle(4)

print(rectangle.description())  # Output: This is a Rectangle
print(f"Rectangle area: {rectangle.area()}")        # Output: Rectangle area: 15
print(f"Rectangle perimeter: {rectangle.perimeter()}")  # Output: Rectangle perimeter: 16

print(circle.description())     # Output: This is a Circle
print(f"Circle area: {circle.area():.2f}")         # Output: Circle area: 50.27
print(f"Circle perimeter: {circle.perimeter():.2f}")   # Output: Circle perimeter: 25.13
```

## `PRACTICAL EXAMPLE: EMPLOYEE MANAGEMENT SYSTEM`

```python
from datetime import datetime
from abc import ABC, abstractmethod

class Person:
    """Base class for all persons in the system"""
    def __init__(self, name, email, phone):
        self.name = name
        self.email = email
        self.phone = phone
        self.created_at = datetime.now()

    def get_contact_info(self):
        return f"Email: {self.email}, Phone: {self.phone}"

    def __str__(self):
        return f"{self.name} ({self.email})"

class Employee(Person):
    """Base class for all employees"""
    employee_count = 0

    def __init__(self, name, email, phone, employee_id, department):
        super().__init__(name, email, phone)
        self.employee_id = employee_id
        self.department = department
        self.is_active = True
        Employee.employee_count += 1

    @abstractmethod
    def calculate_salary(self):
        """Abstract method - each employee type calculates salary differently"""
        pass

    @abstractmethod
    def get_role(self):
        """Abstract method - each employee type has a different role"""
        pass

    def deactivate(self):
        """Deactivate employee"""
        self.is_active = False
        Employee.employee_count -= 1
        return f"{self.name} has been deactivated"

    @classmethod
    def get_employee_count(cls):
        return cls.employee_count

    def __str__(self):
        return f"{self.get_role()}: {self.name} (ID: {self.employee_id})"

class FullTimeEmployee(Employee):
    """Full-time employee with fixed annual salary"""
    def __init__(self, name, email, phone, employee_id, department, annual_salary):
        super().__init__(name, email, phone, employee_id, department)
        self.annual_salary = annual_salary

    def calculate_salary(self):
        """Monthly salary for full-time employee"""
        return self.annual_salary / 12

    def get_role(self):
        return "Full-Time Employee"

    def get_annual_bonus(self):
        """Full-time employees get 10% annual bonus"""
        return self.annual_salary * 0.10

class PartTimeEmployee(Employee):
    """Part-time employee with hourly wage"""
    def __init__(self, name, email, phone, employee_id, department, hourly_rate, hours_per_week):
        super().__init__(name, email, phone, employee_id, department)
        self.hourly_rate = hourly_rate
        self.hours_per_week = hours_per_week

    def calculate_salary(self):
        """Monthly salary for part-time employee (assuming 4.33 weeks per month)"""
        return self.hourly_rate * self.hours_per_week * 4.33

    def get_role(self):
        return "Part-Time Employee"

class Manager(FullTimeEmployee):
    """Manager inherits from FullTimeEmployee and has additional responsibilities"""
    def __init__(self, name, email, phone, employee_id, department, annual_salary, team_size):
        super().__init__(name, email, phone, employee_id, department, annual_salary)
        self.team_size = team_size
        self.team_members = []

    def get_role(self):
        return "Manager"

    def calculate_salary(self):
        """Managers get base salary plus management bonus"""
        base_salary = super().calculate_salary()
        management_bonus = self.team_size * 500  # $500 per team member
        return base_salary + management_bonus

    def add_team_member(self, employee):
        """Add employee to team"""
        if isinstance(employee, Employee):
            self.team_members.append(employee)
            return f"{employee.name} added to {self.name}'s team"
        else:
            return "Can only add Employee objects to team"

    def get_team_info(self):
        """Get information about team members"""
        if not self.team_members:
            return f"{self.name} has no team members"

        team_info = f"{self.name}'s Team:\n"
        for member in self.team_members:
            team_info += f"- {member}\n"
        return team_info.strip()

class Contractor(Person):
    """Contractor is not an employee but inherits from Person"""
    def __init__(self, name, email, phone, contract_id, project, hourly_rate):
        super().__init__(name, email, phone)
        self.contract_id = contract_id
        self.project = project
        self.hourly_rate = hourly_rate
        self.hours_worked = 0

    def log_hours(self, hours):
        """Log hours worked"""
        self.hours_worked += hours
        return f"Logged {hours} hours for {self.name}. Total: {self.hours_worked}"

    def calculate_payment(self):
        """Calculate total payment for contractor"""
        return self.hours_worked * self.hourly_rate

    def __str__(self):
        return f"Contractor: {self.name} (Contract ID: {self.contract_id})"

# Using the employee management system
# Create employees
john = FullTimeEmployee("John Doe", "john@company.com", "123-456-7890", "EMP001", "Engineering", 80000)
jane = PartTimeEmployee("Jane Smith", "jane@company.com", "098-765-4321", "EMP002", "Marketing", 25, 20)
bob = Manager("Bob Johnson", "bob@company.com", "555-123-4567", "MGR001", "Engineering", 120000, 0)
alice = Contractor("Alice Brown", "alice@freelance.com", "777-888-9999", "CON001", "Website Redesign", 75)

# Manager adds team members
print(bob.add_team_member(john))    # Output: John Doe added to Bob Johnson's team
print(bob.add_team_member(jane))    # Output: Jane Smith added to Bob Johnson's team
bob.team_size = 2  # Update team size for salary calculation

# Calculate salaries
print(f"{john.name} monthly salary: ${john.calculate_salary():.2f}")  # Output: John Doe monthly salary: $6666.67
print(f"{jane.name} monthly salary: ${jane.calculate_salary():.2f}")  # Output: Jane Smith monthly salary: $2165.00
print(f"{bob.name} monthly salary: ${bob.calculate_salary():.2f}")    # Output: Bob Johnson monthly salary: $11000.00

# Contractor logs hours and gets paid
alice.log_hours(40)
alice.log_hours(35)
print(f"{alice.name} total payment: ${alice.calculate_payment()}")    # Output: Alice Brown total payment: $5625

# Get team information
print(bob.get_team_info())
# Output:
# Bob Johnson's Team:
# - Full-Time Employee: John Doe (ID: EMP001)
# - Part-Time Employee: Jane Smith (ID: EMP002)

# Check employee count
print(f"Total employees: {Employee.get_employee_count()}")  # Output: Total employees: 3

# Annual bonus for full-time employees
print(f"{john.name} annual bonus: ${john.get_annual_bonus()}")  # Output: John Doe annual bonus: $8000.0
```

## `COMMON INHERITANCE QUESTIONS`

1. **Q: What is the difference between `super()` and calling parent method directly?**

   - **A**: `super()` respects the MRO and is safer for multiple inheritance. Direct calls can cause issues in complex hierarchies.

2. **Q: Can you override class attributes in child classes?**

   - **A**: Yes, child classes can override class attributes from parent classes.

3. **Q: What happens when you don't call `super().__init__()`?**

   - **A**: The parent class constructor won't be called, so parent attributes won't be initialized.

4. **Q: How does Python handle the diamond problem?**

   - **A**: Python uses Method Resolution Order (MRO) with C3 linearization to determine which method to call.

5. **Q: When should you use abstract base classes?**
   - **A**: When you want to define a common interface that all subclasses must implement, ensuring consistency across related classes.
