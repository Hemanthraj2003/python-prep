# ABSTRACTION

- Abstraction is the process of hiding complex implementation details while showing only essential features of an object.
- It focuses on what an object does rather than how it does it.
- Abstraction is achieved through abstract classes and interfaces that define a common structure.
- **Key Concept**: Abstraction reduces complexity by providing a simplified interface to interact with complex systems.

## `ABSTRACT BASE CLASSES (ABC)`

Python's `abc` module provides infrastructure for defining Abstract Base Classes:

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    """Abstract base class for all vehicles"""

    def __init__(self, brand, model, year):
        self.brand = brand
        self.model = model
        self.year = year
        self.is_running = False

    @abstractmethod
    def start_engine(self):
        """Abstract method - must be implemented by subclasses"""
        pass

    @abstractmethod
    def stop_engine(self):
        """Abstract method - must be implemented by subclasses"""
        pass

    @abstractmethod
    def get_fuel_type(self):
        """Abstract method - must be implemented by subclasses"""
        pass

    def get_info(self):
        """Concrete method - available to all subclasses"""
        return f"{self.year} {self.brand} {self.model}"

    def honk(self):
        """Concrete method - available to all subclasses"""
        return "Beep! Beep!"

class Car(Vehicle):
    """Concrete implementation of Vehicle"""

    def __init__(self, brand, model, year, doors):
        super().__init__(brand, model, year)
        self.doors = doors
        self.fuel_level = 100

    def start_engine(self):
        """Implementation of abstract method"""
        if not self.is_running:
            self.is_running = True
            return f"{self.brand} {self.model} engine started"
        return f"{self.brand} {self.model} is already running"

    def stop_engine(self):
        """Implementation of abstract method"""
        if self.is_running:
            self.is_running = False
            return f"{self.brand} {self.model} engine stopped"
        return f"{self.brand} {self.model} is already stopped"

    def get_fuel_type(self):
        """Implementation of abstract method"""
        return "Gasoline"

    def drive(self, distance):
        """Car-specific method"""
        if self.is_running and self.fuel_level > 0:
            fuel_consumed = distance * 0.1
            self.fuel_level = max(0, self.fuel_level - fuel_consumed)
            return f"Drove {distance} km. Fuel level: {self.fuel_level:.1f}%"
        return "Cannot drive - engine not running or no fuel"

class ElectricCar(Vehicle):
    """Another concrete implementation of Vehicle"""

    def __init__(self, brand, model, year, battery_capacity):
        super().__init__(brand, model, year)
        self.battery_capacity = battery_capacity
        self.battery_level = 100

    def start_engine(self):
        """Implementation of abstract method"""
        if not self.is_running:
            self.is_running = True
            return f"{self.brand} {self.model} electric system started"
        return f"{self.brand} {self.model} is already running"

    def stop_engine(self):
        """Implementation of abstract method"""
        if self.is_running:
            self.is_running = False
            return f"{self.brand} {self.model} electric system stopped"
        return f"{self.brand} {self.model} is already stopped"

    def get_fuel_type(self):
        """Implementation of abstract method"""
        return "Electric"

    def charge(self, hours):
        """Electric car specific method"""
        charge_gained = min(100 - self.battery_level, hours * 20)
        self.battery_level += charge_gained
        return f"Charged for {hours} hours. Battery level: {self.battery_level:.1f}%"

# Cannot instantiate abstract class
# vehicle = Vehicle("Generic", "Model", 2025)  # TypeError: Can't instantiate abstract class

# Can instantiate concrete classes
car = Car("Toyota", "Camry", 2025, 4)
electric_car = ElectricCar("Tesla", "Model 3", 2025, 75)

print(car.get_info())               # Output: 2025 Toyota Camry (inherited method)
print(car.start_engine())           # Output: Toyota Camry engine started
print(car.get_fuel_type())          # Output: Gasoline
print(car.drive(50))                # Output: Drove 50 km. Fuel level: 95.0%

print(electric_car.get_info())      # Output: 2025 Tesla Model 3 (inherited method)
print(electric_car.start_engine())  # Output: Tesla Model 3 electric system started
print(electric_car.get_fuel_type()) # Output: Electric
print(electric_car.charge(3))       # Output: Charged for 3 hours. Battery level: 100.0%
```

## `MULTIPLE ABSTRACT METHODS`

```python
from abc import ABC, abstractmethod
import math

class Shape(ABC):
    """Abstract base class for geometric shapes"""

    def __init__(self, name):
        self.name = name

    @abstractmethod
    def area(self):
        """Calculate the area of the shape"""
        pass

    @abstractmethod
    def perimeter(self):
        """Calculate the perimeter of the shape"""
        pass

    @abstractmethod
    def draw(self):
        """Draw the shape"""
        pass

    def description(self):
        """Concrete method providing shape description"""
        return f"{self.name} with area {self.area():.2f} and perimeter {self.perimeter():.2f}"

    def compare_area(self, other_shape):
        """Concrete method to compare areas"""
        if not isinstance(other_shape, Shape):
            return "Can only compare with other shapes"

        if self.area() > other_shape.area():
            return f"{self.name} is larger than {other_shape.name}"
        elif self.area() < other_shape.area():
            return f"{self.name} is smaller than {other_shape.name}"
        else:
            return f"{self.name} and {other_shape.name} have equal areas"

class Rectangle(Shape):
    """Concrete implementation of Shape"""

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

    def draw(self):
        """Implementation of abstract method"""
        return f"Drawing a rectangle {self.width}x{self.height}"

class Circle(Shape):
    """Concrete implementation of Shape"""

    def __init__(self, radius):
        super().__init__("Circle")
        self.radius = radius

    def area(self):
        """Implementation of abstract method"""
        return math.pi * self.radius ** 2

    def perimeter(self):
        """Implementation of abstract method"""
        return 2 * math.pi * self.radius

    def draw(self):
        """Implementation of abstract method"""
        return f"Drawing a circle with radius {self.radius}"

class Triangle(Shape):
    """Concrete implementation of Shape"""

    def __init__(self, side_a, side_b, side_c):
        super().__init__("Triangle")
        self.side_a = side_a
        self.side_b = side_b
        self.side_c = side_c

        # Validate triangle inequality
        if not (side_a + side_b > side_c and side_a + side_c > side_b and side_b + side_c > side_a):
            raise ValueError("Invalid triangle: sides don't satisfy triangle inequality")

    def area(self):
        """Implementation of abstract method using Heron's formula"""
        s = self.perimeter() / 2
        return math.sqrt(s * (s - self.side_a) * (s - self.side_b) * (s - self.side_c))

    def perimeter(self):
        """Implementation of abstract method"""
        return self.side_a + self.side_b + self.side_c

    def draw(self):
        """Implementation of abstract method"""
        return f"Drawing a triangle with sides {self.side_a}, {self.side_b}, {self.side_c}"

# Using abstract classes and polymorphism
shapes = [
    Rectangle(5, 3),
    Circle(4),
    Triangle(3, 4, 5)
]

print("Shape Information:")
for shape in shapes:
    print(f"- {shape.description()}")
    print(f"  {shape.draw()}")

print("\nArea Comparisons:")
print(shapes[0].compare_area(shapes[1]))  # Rectangle vs Circle
print(shapes[1].compare_area(shapes[2]))  # Circle vs Triangle
```

## `ABSTRACT PROPERTIES`

Abstract classes can also define abstract properties that must be implemented by subclasses:

```python
from abc import ABC, abstractmethod

class DatabaseConnection(ABC):
    """Abstract base class for database connections"""

    def __init__(self, host, port):
        self._host = host
        self._port = port
        self._is_connected = False

    @property
    @abstractmethod
    def connection_string(self):
        """Abstract property - must be implemented by subclasses"""
        pass

    @property
    @abstractmethod
    def default_port(self):
        """Abstract property - must be implemented by subclasses"""
        pass

    @abstractmethod
    def connect(self):
        """Abstract method to establish connection"""
        pass

    @abstractmethod
    def disconnect(self):
        """Abstract method to close connection"""
        pass

    @abstractmethod
    def execute_query(self, query):
        """Abstract method to execute query"""
        pass

    def is_connected(self):
        """Concrete method"""
        return self._is_connected

    @property
    def host(self):
        """Concrete property"""
        return self._host

    @property
    def port(self):
        """Concrete property"""
        return self._port

class MySQLConnection(DatabaseConnection):
    """MySQL implementation of DatabaseConnection"""

    def __init__(self, host, port, database, username, password):
        super().__init__(host, port)
        self.database = database
        self.username = username
        self.password = password

    @property
    def connection_string(self):
        """Implementation of abstract property"""
        return f"mysql://{self.username}:****@{self._host}:{self._port}/{self.database}"

    @property
    def default_port(self):
        """Implementation of abstract property"""
        return 3306

    def connect(self):
        """Implementation of abstract method"""
        # Simulate MySQL connection
        self._is_connected = True
        return f"Connected to MySQL database '{self.database}' at {self._host}:{self._port}"

    def disconnect(self):
        """Implementation of abstract method"""
        self._is_connected = False
        return f"Disconnected from MySQL database '{self.database}'"

    def execute_query(self, query):
        """Implementation of abstract method"""
        if not self._is_connected:
            return "Error: Not connected to database"
        return f"Executing MySQL query: {query}"

class PostgreSQLConnection(DatabaseConnection):
    """PostgreSQL implementation of DatabaseConnection"""

    def __init__(self, host, port, database, username, password):
        super().__init__(host, port)
        self.database = database
        self.username = username
        self.password = password

    @property
    def connection_string(self):
        """Implementation of abstract property"""
        return f"postgresql://{self.username}:****@{self._host}:{self._port}/{self.database}"

    @property
    def default_port(self):
        """Implementation of abstract property"""
        return 5432

    def connect(self):
        """Implementation of abstract method"""
        # Simulate PostgreSQL connection
        self._is_connected = True
        return f"Connected to PostgreSQL database '{self.database}' at {self._host}:{self._port}"

    def disconnect(self):
        """Implementation of abstract method"""
        self._is_connected = False
        return f"Disconnected from PostgreSQL database '{self.database}'"

    def execute_query(self, query):
        """Implementation of abstract method"""
        if not self._is_connected:
            return "Error: Not connected to database"
        return f"Executing PostgreSQL query: {query}"

# Using abstract properties
mysql_conn = MySQLConnection("localhost", 3306, "myapp", "user", "password")
postgres_conn = PostgreSQLConnection("localhost", 5432, "myapp", "user", "password")

print("MySQL Connection:")
print(f"Connection String: {mysql_conn.connection_string}")
print(f"Default Port: {mysql_conn.default_port}")
print(mysql_conn.connect())
print(mysql_conn.execute_query("SELECT * FROM users"))
print(mysql_conn.disconnect())

print("\nPostgreSQL Connection:")
print(f"Connection String: {postgres_conn.connection_string}")
print(f"Default Port: {postgres_conn.default_port}")
print(postgres_conn.connect())
print(postgres_conn.execute_query("SELECT * FROM users"))
print(postgres_conn.disconnect())
```

## `INTERFACES USING PROTOCOLS`

Python 3.8+ introduced Protocol for structural subtyping (duck typing with type hints):

```python
from typing import Protocol

class Drawable(Protocol):
    """Protocol defining the interface for drawable objects"""

    def draw(self) -> str:
        """Draw the object"""
        ...

    def get_bounds(self) -> tuple[int, int, int, int]:
        """Get the bounding box (x, y, width, height)"""
        ...

class Movable(Protocol):
    """Protocol defining the interface for movable objects"""

    def move(self, dx: int, dy: int) -> None:
        """Move the object by dx, dy"""
        ...

    def get_position(self) -> tuple[int, int]:
        """Get current position (x, y)"""
        ...

class Button:
    """Button class implementing Drawable and Movable protocols"""

    def __init__(self, text: str, x: int, y: int, width: int, height: int):
        self.text = text
        self.x = x
        self.y = y
        self.width = width
        self.height = height

    def draw(self) -> str:
        """Implementation of Drawable protocol"""
        return f"Drawing button '{self.text}' at ({self.x}, {self.y})"

    def get_bounds(self) -> tuple[int, int, int, int]:
        """Implementation of Drawable protocol"""
        return (self.x, self.y, self.width, self.height)

    def move(self, dx: int, dy: int) -> None:
        """Implementation of Movable protocol"""
        self.x += dx
        self.y += dy

    def get_position(self) -> tuple[int, int]:
        """Implementation of Movable protocol"""
        return (self.x, self.y)

    def click(self) -> str:
        """Button-specific method"""
        return f"Button '{self.text}' clicked"

class Image:
    """Image class implementing Drawable and Movable protocols"""

    def __init__(self, filename: str, x: int, y: int):
        self.filename = filename
        self.x = x
        self.y = y
        self.width = 100  # Default width
        self.height = 100  # Default height

    def draw(self) -> str:
        """Implementation of Drawable protocol"""
        return f"Drawing image '{self.filename}' at ({self.x}, {self.y})"

    def get_bounds(self) -> tuple[int, int, int, int]:
        """Implementation of Drawable protocol"""
        return (self.x, self.y, self.width, self.height)

    def move(self, dx: int, dy: int) -> None:
        """Implementation of Movable protocol"""
        self.x += dx
        self.y += dy

    def get_position(self) -> tuple[int, int]:
        """Implementation of Movable protocol"""
        return (self.x, self.y)

def draw_all(objects: list[Drawable]) -> None:
    """Function that works with any object implementing Drawable protocol"""
    for obj in objects:
        print(obj.draw())
        bounds = obj.get_bounds()
        print(f"  Bounds: x={bounds[0]}, y={bounds[1]}, w={bounds[2]}, h={bounds[3]}")

def move_all(objects: list[Movable], dx: int, dy: int) -> None:
    """Function that works with any object implementing Movable protocol"""
    for obj in objects:
        old_pos = obj.get_position()
        obj.move(dx, dy)
        new_pos = obj.get_position()
        print(f"Moved object from {old_pos} to {new_pos}")

# Using protocols for abstraction
ui_elements = [
    Button("Submit", 10, 20, 80, 30),
    Button("Cancel", 100, 20, 80, 30),
    Image("logo.png", 50, 100)
]

print("Drawing all UI elements:")
draw_all(ui_elements)

print("\nMoving all elements by (5, 10):")
move_all(ui_elements, 5, 10)

print("\nDrawing after move:")
draw_all(ui_elements)
```

## `PRACTICAL EXAMPLE: FILE PROCESSING SYSTEM`

```python
from abc import ABC, abstractmethod
import json
import csv
from typing import Any, Dict, List

class FileProcessor(ABC):
    """Abstract base class for file processors"""

    def __init__(self, filename: str):
        self.filename = filename
        self._data = None

    @abstractmethod
    def read_file(self) -> Any:
        """Abstract method to read file"""
        pass

    @abstractmethod
    def write_file(self, data: Any) -> bool:
        """Abstract method to write file"""
        pass

    @abstractmethod
    def validate_data(self, data: Any) -> bool:
        """Abstract method to validate data"""
        pass

    def process_file(self) -> Dict[str, Any]:
        """Template method using abstract methods"""
        try:
            # Read the file
            self._data = self.read_file()

            # Validate the data
            if not self.validate_data(self._data):
                return {"success": False, "error": "Data validation failed"}

            # Process the data (can be overridden by subclasses)
            processed_data = self.transform_data(self._data)

            return {
                "success": True,
                "data": processed_data,
                "record_count": len(processed_data) if isinstance(processed_data, list) else 1
            }

        except Exception as e:
            return {"success": False, "error": str(e)}

    def transform_data(self, data: Any) -> Any:
        """Default transformation - can be overridden by subclasses"""
        return data

    def backup_file(self) -> str:
        """Concrete method available to all subclasses"""
        backup_filename = f"{self.filename}.backup"
        # In a real implementation, you would copy the file
        return f"Backup created: {backup_filename}"

class JSONProcessor(FileProcessor):
    """JSON file processor"""

    def read_file(self) -> Dict[str, Any]:
        """Implementation of abstract method"""
        try:
            with open(self.filename, 'r') as file:
                return json.load(file)
        except FileNotFoundError:
            # For demo purposes, return sample data
            return {"users": [{"name": "John", "age": 30}, {"name": "Jane", "age": 25}]}

    def write_file(self, data: Dict[str, Any]) -> bool:
        """Implementation of abstract method"""
        try:
            with open(self.filename, 'w') as file:
                json.dump(data, file, indent=2)
            return True
        except Exception:
            return False

    def validate_data(self, data: Dict[str, Any]) -> bool:
        """Implementation of abstract method"""
        return isinstance(data, dict) and len(data) > 0

    def transform_data(self, data: Dict[str, Any]) -> List[Dict[str, Any]]:
        """Override transformation for JSON"""
        # Flatten nested structures if needed
        if "users" in data:
            return data["users"]
        return [data]

class CSVProcessor(FileProcessor):
    """CSV file processor"""

    def __init__(self, filename: str, delimiter: str = ','):
        super().__init__(filename)
        self.delimiter = delimiter

    def read_file(self) -> List[Dict[str, str]]:
        """Implementation of abstract method"""
        try:
            with open(self.filename, 'r', newline='') as file:
                reader = csv.DictReader(file, delimiter=self.delimiter)
                return list(reader)
        except FileNotFoundError:
            # For demo purposes, return sample data
            return [
                {"name": "Alice", "age": "28", "city": "New York"},
                {"name": "Bob", "age": "32", "city": "Los Angeles"}
            ]

    def write_file(self, data: List[Dict[str, str]]) -> bool:
        """Implementation of abstract method"""
        try:
            if not data:
                return False

            with open(self.filename, 'w', newline='') as file:
                fieldnames = data[0].keys()
                writer = csv.DictWriter(file, fieldnames=fieldnames, delimiter=self.delimiter)
                writer.writeheader()
                writer.writerows(data)
            return True
        except Exception:
            return False

    def validate_data(self, data: List[Dict[str, str]]) -> bool:
        """Implementation of abstract method"""
        return isinstance(data, list) and len(data) > 0 and all(isinstance(row, dict) for row in data)

    def transform_data(self, data: List[Dict[str, str]]) -> List[Dict[str, Any]]:
        """Override transformation for CSV"""
        # Convert string numbers to integers where possible
        transformed = []
        for row in data:
            new_row = {}
            for key, value in row.items():
                try:
                    # Try to convert to int, then float, otherwise keep as string
                    if value.isdigit():
                        new_row[key] = int(value)
                    elif '.' in value and value.replace('.', '').isdigit():
                        new_row[key] = float(value)
                    else:
                        new_row[key] = value
                except (ValueError, AttributeError):
                    new_row[key] = value
            transformed.append(new_row)
        return transformed

class XMLProcessor(FileProcessor):
    """XML file processor (simplified implementation)"""

    def read_file(self) -> Dict[str, Any]:
        """Implementation of abstract method"""
        # For demo purposes, return sample XML-like data
        return {
            "root": {
                "employees": [
                    {"name": "Charlie", "department": "Engineering", "salary": "75000"},
                    {"name": "Diana", "department": "Marketing", "salary": "65000"}
                ]
            }
        }

    def write_file(self, data: Dict[str, Any]) -> bool:
        """Implementation of abstract method"""
        # Simplified XML writing (in real implementation, use xml.etree.ElementTree)
        return True

    def validate_data(self, data: Dict[str, Any]) -> bool:
        """Implementation of abstract method"""
        return isinstance(data, dict) and "root" in data

    def transform_data(self, data: Dict[str, Any]) -> List[Dict[str, Any]]:
        """Override transformation for XML"""
        if "root" in data and "employees" in data["root"]:
            return data["root"]["employees"]
        return [data]

class FileProcessingManager:
    """Manager class that uses abstraction to handle different file types"""

    def __init__(self):
        self.processors = {}

    def register_processor(self, file_extension: str, processor_class: type):
        """Register a processor for a specific file extension"""
        self.processors[file_extension] = processor_class

    def get_processor(self, filename: str) -> FileProcessor:
        """Factory method to get appropriate processor based on file extension"""
        extension = filename.split('.')[-1].lower()
        if extension not in self.processors:
            raise ValueError(f"No processor registered for .{extension} files")

        processor_class = self.processors[extension]
        return processor_class(filename)

    def process_file(self, filename: str) -> Dict[str, Any]:
        """Process any supported file type"""
        try:
            processor = self.get_processor(filename)
            result = processor.process_file()

            if result["success"]:
                result["processor_type"] = type(processor).__name__
                result["backup_info"] = processor.backup_file()

            return result

        except Exception as e:
            return {"success": False, "error": f"Processing failed: {str(e)}"}

    def process_multiple_files(self, filenames: List[str]) -> Dict[str, Any]:
        """Process multiple files of different types"""
        results = {}
        total_records = 0
        successful_files = 0

        for filename in filenames:
            result = self.process_file(filename)
            results[filename] = result

            if result["success"]:
                successful_files += 1
                total_records += result.get("record_count", 0)

        return {
            "individual_results": results,
            "summary": {
                "total_files": len(filenames),
                "successful_files": successful_files,
                "failed_files": len(filenames) - successful_files,
                "total_records_processed": total_records
            }
        }

# Using the abstract file processing system
manager = FileProcessingManager()

# Register processors for different file types
manager.register_processor("json", JSONProcessor)
manager.register_processor("csv", CSVProcessor)
manager.register_processor("xml", XMLProcessor)

# Process different file types using the same interface
files_to_process = [
    "users.json",
    "employees.csv",
    "config.xml"
]

print("Processing multiple files:")
results = manager.process_multiple_files(files_to_process)

for filename, result in results["individual_results"].items():
    print(f"\n{filename}:")
    if result["success"]:
        print(f"  ✓ Success - {result['record_count']} records processed")
        print(f"  Processor: {result['processor_type']}")
        print(f"  {result['backup_info']}")
        print(f"  Sample data: {result['data'][:1] if result['data'] else 'No data'}")
    else:
        print(f"  ✗ Failed - {result['error']}")

print(f"\nSummary: {results['summary']}")
```

## `BENEFITS OF ABSTRACTION`

1. **Simplified Interface**: Hide complex implementation details
2. **Code Reusability**: Common interface for different implementations
3. **Maintainability**: Changes to implementation don't affect client code
4. **Polymorphism**: Different objects can be used interchangeably
5. **Design Clarity**: Clear separation between interface and implementation

## `COMMON ABSTRACTION QUESTIONS`

1. **Q: What's the difference between abstraction and encapsulation?**

   - **A**: Abstraction hides complexity by showing only essential features. Encapsulation hides data and methods for data protection.

2. **Q: When should you use abstract base classes vs protocols?**

   - **A**: Use ABC when you want to enforce implementation and provide common functionality. Use protocols for duck typing with type hints.

3. **Q: Can abstract classes have concrete methods?**

   - **A**: Yes, abstract classes can have both abstract methods (must be implemented) and concrete methods (inherited as-is).

4. **Q: What happens if you don't implement all abstract methods?**

   - **A**: Python will raise a TypeError when you try to instantiate the class.

5. **Q: How does abstraction relate to the other OOP principles?**
   - **A**: Abstraction works with inheritance (defining common interfaces), polymorphism (using different implementations interchangeably), and encapsulation (hiding implementation details).
