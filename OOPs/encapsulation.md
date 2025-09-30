# ENCAPSULATION

- Encapsulation is the principle of bundling data (attributes) and methods that operate on that data within a single unit (class).
- It restricts direct access to some of an object's components and prevents accidental modification of data.
- Encapsulation is achieved through access modifiers and provides data hiding and abstraction.
- **Key Concept**: Encapsulation promotes data integrity and reduces system complexity by hiding internal implementation details.

## `ACCESS MODIFIERS IN PYTHON`

Python uses naming conventions to indicate the intended access level of attributes and methods:

### 1. **Public Members**

Public members are accessible from anywhere and have no special naming convention.

```python
class Student:
    def __init__(self, name, student_id):
        self.name = name              # Public attribute
        self.student_id = student_id  # Public attribute
        self.courses = []             # Public attribute

    def add_course(self, course):     # Public method
        """Add a course to the student's course list"""
        self.courses.append(course)
        return f"Added {course} to {self.name}'s courses"

    def get_info(self):               # Public method
        """Get student information"""
        return f"Student: {self.name} (ID: {self.student_id})"

# Public members are accessible from outside the class
student = Student("Alice", "S12345")
print(student.name)           # Output: Alice
print(student.student_id)     # Output: S12345
print(student.add_course("Python Programming"))  # Output: Added Python Programming to Alice's courses
print(student.courses)        # Output: ['Python Programming']
```

### 2. **Protected Members**

Protected members are intended for internal use within the class and its subclasses. They are prefixed with a single underscore `_`.

```python
class BankAccount:
    def __init__(self, account_number, initial_balance):
        self.account_number = account_number    # Public
        self._balance = initial_balance         # Protected attribute
        self._transaction_history = []         # Protected attribute

    def _validate_amount(self, amount):         # Protected method
        """Internal method to validate transaction amounts"""
        return isinstance(amount, (int, float)) and amount > 0

    def _add_transaction(self, transaction):    # Protected method
        """Internal method to add transaction to history"""
        self._transaction_history.append(transaction)

    def deposit(self, amount):                  # Public method
        """Public method to deposit money"""
        if self._validate_amount(amount):
            self._balance += amount
            self._add_transaction(f"Deposited: ${amount}")
            return f"Deposited ${amount}. New balance: ${self._balance}"
        return "Invalid deposit amount"

    def withdraw(self, amount):                 # Public method
        """Public method to withdraw money"""
        if self._validate_amount(amount) and amount <= self._balance:
            self._balance -= amount
            self._add_transaction(f"Withdrew: ${amount}")
            return f"Withdrew ${amount}. New balance: ${self._balance}"
        return "Invalid withdrawal or insufficient funds"

    def get_balance(self):                      # Public method
        """Public method to get current balance"""
        return self._balance

class SavingsAccount(BankAccount):
    def __init__(self, account_number, initial_balance, interest_rate):
        super().__init__(account_number, initial_balance)
        self._interest_rate = interest_rate     # Protected attribute

    def add_interest(self):
        """Add interest to the account"""
        if self._validate_amount(self._balance):  # Accessing protected method from parent
            interest = self._balance * self._interest_rate / 100
            self._balance += interest
            self._add_transaction(f"Interest added: ${interest:.2f}")
            return f"Interest added: ${interest:.2f}. New balance: ${self._balance:.2f}"

# Using protected members
account = BankAccount("ACC123", 1000)
savings = SavingsAccount("SAV456", 2000, 2.5)

print(account.deposit(500))         # Public method
print(account.get_balance())        # Output: 1500

# Protected members are accessible but shouldn't be used directly
print(account._balance)             # Output: 1500 (works but not recommended)
print(account._transaction_history) # Works but not recommended

# Protected methods can be used in subclasses
print(savings.add_interest())       # Uses protected methods from parent class
```

### 3. **Private Members**

Private members are intended to be completely internal to the class. They are prefixed with double underscores `__` and use name mangling.

```python
class SecureBankAccount:
    def __init__(self, account_number, pin, initial_balance):
        self.account_number = account_number    # Public
        self.__pin = pin                        # Private attribute
        self.__balance = initial_balance        # Private attribute
        self.__failed_attempts = 0             # Private attribute

    def __validate_pin(self, entered_pin):      # Private method
        """Private method to validate PIN"""
        if entered_pin == self.__pin:
            self.__failed_attempts = 0
            return True
        else:
            self.__failed_attempts += 1
            return False

    def __is_account_locked(self):              # Private method
        """Private method to check if account is locked"""
        return self.__failed_attempts >= 3

    def __reset_failed_attempts(self):          # Private method
        """Private method to reset failed attempts"""
        self.__failed_attempts = 0

    def get_balance(self, pin):                 # Public method
        """Get balance with PIN verification"""
        if self.__is_account_locked():
            return "Account is locked due to multiple failed attempts"

        if self.__validate_pin(pin):
            return f"Current balance: ${self.__balance}"
        return f"Invalid PIN. Attempts remaining: {3 - self.__failed_attempts}"

    def deposit(self, amount, pin):             # Public method
        """Deposit money with PIN verification"""
        if self.__is_account_locked():
            return "Account is locked"

        if self.__validate_pin(pin):
            if amount > 0:
                self.__balance += amount
                return f"Deposited ${amount}. New balance: ${self.__balance}"
            return "Invalid amount"
        return f"Invalid PIN. Attempts remaining: {3 - self.__failed_attempts}"

    def change_pin(self, old_pin, new_pin):     # Public method
        """Change PIN with old PIN verification"""
        if self.__is_account_locked():
            return "Account is locked"

        if self.__validate_pin(old_pin):
            self.__pin = new_pin
            return "PIN changed successfully"
        return f"Invalid PIN. Attempts remaining: {3 - self.__failed_attempts}"

# Using private members
secure_account = SecureBankAccount("SEC789", 1234, 5000)

print(secure_account.get_balance(1234))     # Output: Current balance: $5000
print(secure_account.get_balance(1111))     # Output: Invalid PIN. Attempts remaining: 2
print(secure_account.deposit(500, 1234))    # Output: Deposited $500. New balance: $5500

# Private members are not directly accessible
# print(secure_account.__pin)                # AttributeError: no attribute '__pin'
# print(secure_account.__balance)            # AttributeError: no attribute '__balance'

# Name mangling - Python internally changes __pin to _SecureBankAccount__pin
print(secure_account._SecureBankAccount__pin)     # Output: 1234 (not recommended!)
print(secure_account._SecureBankAccount__balance) # Output: 5500 (not recommended!)
```

## `PROPERTY DECORATORS FOR ENCAPSULATION`

Properties provide a Pythonic way to implement getters and setters while maintaining attribute-like access:

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = None
        self.celsius = celsius  # Use the setter for validation

    @property
    def celsius(self):
        """Getter for celsius temperature"""
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        """Setter for celsius temperature with validation"""
        if not isinstance(value, (int, float)):
            raise TypeError("Temperature must be a number")
        if value < -273.15:
            raise ValueError("Temperature cannot be below absolute zero (-273.15°C)")
        self._celsius = value

    @property
    def fahrenheit(self):
        """Read-only property for fahrenheit temperature"""
        return (self._celsius * 9/5) + 32

    @property
    def kelvin(self):
        """Read-only property for kelvin temperature"""
        return self._celsius + 273.15

    @property
    def description(self):
        """Read-only property for temperature description"""
        if self._celsius < 0:
            return "Freezing"
        elif self._celsius < 20:
            return "Cold"
        elif self._celsius < 30:
            return "Comfortable"
        else:
            return "Hot"

class Person:
    def __init__(self, name, age, email):
        self._name = None
        self._age = None
        self._email = None
        self.name = name      # Use setters for validation
        self.age = age
        self.email = email

    @property
    def name(self):
        """Getter for name"""
        return self._name

    @name.setter
    def name(self, value):
        """Setter for name with validation"""
        if not isinstance(value, str):
            raise TypeError("Name must be a string")
        if len(value.strip()) < 2:
            raise ValueError("Name must be at least 2 characters long")
        self._name = value.strip().title()

    @property
    def age(self):
        """Getter for age"""
        return self._age

    @age.setter
    def age(self, value):
        """Setter for age with validation"""
        if not isinstance(value, int):
            raise TypeError("Age must be an integer")
        if value < 0 or value > 150:
            raise ValueError("Age must be between 0 and 150")
        self._age = value

    @property
    def email(self):
        """Getter for email"""
        return self._email

    @email.setter
    def email(self, value):
        """Setter for email with validation"""
        if not isinstance(value, str):
            raise TypeError("Email must be a string")
        if "@" not in value or "." not in value:
            raise ValueError("Invalid email format")
        self._email = value.lower()

    @property
    def is_adult(self):
        """Read-only property to check if person is adult"""
        return self._age >= 18

# Using properties for encapsulation
temp = Temperature(25)
print(f"Temperature: {temp.celsius}°C")         # Output: Temperature: 25°C
print(f"Fahrenheit: {temp.fahrenheit}°F")       # Output: Fahrenheit: 77.0°F
print(f"Kelvin: {temp.kelvin}K")                # Output: Kelvin: 298.15K
print(f"Description: {temp.description}")       # Output: Description: Comfortable

# Validation in action
# temp.celsius = -300  # ValueError: Temperature cannot be below absolute zero

person = Person("john doe", 25, "JOHN@EXAMPLE.COM")
print(f"Name: {person.name}")        # Output: Name: John Doe (automatically formatted)
print(f"Age: {person.age}")          # Output: Age: 25
print(f"Email: {person.email}")      # Output: Email: john@example.com (automatically lowercased)
print(f"Is adult: {person.is_adult}")# Output: Is adult: True

# Validation works on assignment
person.age = 30
print(f"New age: {person.age}")      # Output: New age: 30

# This would raise an error:
# person.age = -5  # ValueError: Age must be between 0 and 150
```

## `DATA VALIDATION AND INTEGRITY`

```python
class CreditCard:
    def __init__(self, card_number, cardholder_name, expiry_month, expiry_year, cvv):
        self._card_number = None
        self._cardholder_name = None
        self._expiry_month = None
        self._expiry_year = None
        self._cvv = None
        self._is_active = True

        # Use setters for validation
        self.card_number = card_number
        self.cardholder_name = cardholder_name
        self.expiry_month = expiry_month
        self.expiry_year = expiry_year
        self.cvv = cvv

    @property
    def card_number(self):
        """Getter for card number (masked for security)"""
        if self._card_number:
            return "**** **** **** " + self._card_number[-4:]
        return None

    @card_number.setter
    def card_number(self, value):
        """Setter for card number with validation"""
        if not isinstance(value, str):
            raise TypeError("Card number must be a string")

        # Remove spaces and hyphens
        clean_number = value.replace(" ", "").replace("-", "")

        if not clean_number.isdigit():
            raise ValueError("Card number must contain only digits")

        if len(clean_number) != 16:
            raise ValueError("Card number must be 16 digits long")

        if not self._luhn_algorithm(clean_number):
            raise ValueError("Invalid card number (failed Luhn algorithm)")

        self._card_number = clean_number

    @property
    def cardholder_name(self):
        """Getter for cardholder name"""
        return self._cardholder_name

    @cardholder_name.setter
    def cardholder_name(self, value):
        """Setter for cardholder name with validation"""
        if not isinstance(value, str):
            raise TypeError("Cardholder name must be a string")

        clean_name = value.strip()
        if len(clean_name) < 2:
            raise ValueError("Cardholder name must be at least 2 characters")

        if not all(char.isalpha() or char.isspace() for char in clean_name):
            raise ValueError("Cardholder name must contain only letters and spaces")

        self._cardholder_name = clean_name.upper()

    @property
    def expiry_month(self):
        """Getter for expiry month"""
        return self._expiry_month

    @expiry_month.setter
    def expiry_month(self, value):
        """Setter for expiry month with validation"""
        if not isinstance(value, int):
            raise TypeError("Expiry month must be an integer")

        if value < 1 or value > 12:
            raise ValueError("Expiry month must be between 1 and 12")

        self._expiry_month = value

    @property
    def expiry_year(self):
        """Getter for expiry year"""
        return self._expiry_year

    @expiry_year.setter
    def expiry_year(self, value):
        """Setter for expiry year with validation"""
        if not isinstance(value, int):
            raise TypeError("Expiry year must be an integer")

        current_year = 2025  # In a real application, use datetime.datetime.now().year
        if value < current_year or value > current_year + 10:
            raise ValueError(f"Expiry year must be between {current_year} and {current_year + 10}")

        self._expiry_year = value

    @property
    def cvv(self):
        """Getter for CVV (always returns masked value)"""
        return "***" if self._cvv else None

    @cvv.setter
    def cvv(self, value):
        """Setter for CVV with validation"""
        if not isinstance(value, str):
            raise TypeError("CVV must be a string")

        if not value.isdigit():
            raise ValueError("CVV must contain only digits")

        if len(value) not in [3, 4]:
            raise ValueError("CVV must be 3 or 4 digits long")

        self._cvv = value

    @property
    def is_expired(self):
        """Check if card is expired"""
        current_year = 2025
        current_month = 10  # October

        if self._expiry_year < current_year:
            return True
        elif self._expiry_year == current_year and self._expiry_month < current_month:
            return True
        return False

    @property
    def is_valid(self):
        """Check if card is valid and active"""
        return self._is_active and not self.is_expired

    def deactivate(self):
        """Deactivate the card"""
        self._is_active = False
        return "Card has been deactivated"

    def activate(self):
        """Activate the card"""
        if not self.is_expired:
            self._is_active = True
            return "Card has been activated"
        return "Cannot activate expired card"

    @staticmethod
    def _luhn_algorithm(card_number):
        """Validate card number using Luhn algorithm"""
        def digits_of(n):
            return [int(d) for d in str(n)]

        digits = digits_of(card_number)
        odd_digits = digits[-1::-2]
        even_digits = digits[-2::-2]
        checksum = sum(odd_digits)

        for d in even_digits:
            checksum += sum(digits_of(d*2))

        return checksum % 10 == 0

    def __str__(self):
        return f"Credit Card ending in {self._card_number[-4:]} - {self._cardholder_name}"

# Using encapsulated credit card class
try:
    card = CreditCard(
        card_number="4532 1488 0343 6467",  # Valid test card number
        cardholder_name="john smith",
        expiry_month=12,
        expiry_year=2027,
        cvv="123"
    )

    print(f"Card: {card}")                          # Output: Credit Card ending in 6467 - JOHN SMITH
    print(f"Card Number: {card.card_number}")       # Output: Card Number: **** **** **** 6467
    print(f"Cardholder: {card.cardholder_name}")    # Output: Cardholder: JOHN SMITH
    print(f"Expiry: {card.expiry_month}/{card.expiry_year}")  # Output: Expiry: 12/2027
    print(f"CVV: {card.cvv}")                       # Output: CVV: ***
    print(f"Is Valid: {card.is_valid}")             # Output: Is Valid: True
    print(f"Is Expired: {card.is_expired}")         # Output: Is Expired: False

    # Trying to set invalid data
    # card.card_number = "1234 5678 9012 3456"  # Would raise ValueError (invalid Luhn)
    # card.expiry_month = 13                     # Would raise ValueError (invalid month)

except (ValueError, TypeError) as e:
    print(f"Error: {e}")
```

## `PRACTICAL EXAMPLE: USER ACCOUNT SYSTEM`

```python
import hashlib
import re
from datetime import datetime, timedelta

class UserAccount:
    """Encapsulated user account class with validation and security"""

    # Class attributes for validation
    MIN_PASSWORD_LENGTH = 8
    MAX_LOGIN_ATTEMPTS = 3
    LOCKOUT_DURATION = 30  # minutes

    def __init__(self, username, email, password):
        # Private attributes
        self.__username = None
        self.__email = None
        self.__password_hash = None
        self.__is_active = True
        self.__failed_login_attempts = 0
        self.__last_failed_login = None
        self.__created_at = datetime.now()
        self.__last_login = None

        # Use setters for validation
        self.username = username
        self.email = email
        self._set_password(password)

    @property
    def username(self):
        """Getter for username"""
        return self.__username

    @username.setter
    def username(self, value):
        """Setter for username with validation"""
        if not isinstance(value, str):
            raise TypeError("Username must be a string")

        clean_username = value.strip().lower()

        if len(clean_username) < 3:
            raise ValueError("Username must be at least 3 characters long")

        if len(clean_username) > 20:
            raise ValueError("Username must be no more than 20 characters long")

        if not re.match("^[a-z0-9_]+$", clean_username):
            raise ValueError("Username can only contain lowercase letters, numbers, and underscores")

        self.__username = clean_username

    @property
    def email(self):
        """Getter for email"""
        return self.__email

    @email.setter
    def email(self, value):
        """Setter for email with validation"""
        if not isinstance(value, str):
            raise TypeError("Email must be a string")

        email_pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        if not re.match(email_pattern, value):
            raise ValueError("Invalid email format")

        self.__email = value.lower()

    @property
    def is_active(self):
        """Getter for account active status"""
        return self.__is_active

    @property
    def is_locked(self):
        """Check if account is locked due to failed login attempts"""
        if self.__failed_login_attempts >= self.MAX_LOGIN_ATTEMPTS:
            if self.__last_failed_login:
                lockout_end = self.__last_failed_login + timedelta(minutes=self.LOCKOUT_DURATION)
                return datetime.now() < lockout_end
        return False

    @property
    def created_at(self):
        """Getter for account creation time"""
        return self.__created_at

    @property
    def last_login(self):
        """Getter for last login time"""
        return self.__last_login

    def _set_password(self, password):
        """Private method to set password with validation"""
        if not isinstance(password, str):
            raise TypeError("Password must be a string")

        if len(password) < self.MIN_PASSWORD_LENGTH:
            raise ValueError(f"Password must be at least {self.MIN_PASSWORD_LENGTH} characters long")

        # Check for password strength
        if not re.search(r'[A-Z]', password):
            raise ValueError("Password must contain at least one uppercase letter")

        if not re.search(r'[a-z]', password):
            raise ValueError("Password must contain at least one lowercase letter")

        if not re.search(r'\d', password):
            raise ValueError("Password must contain at least one digit")

        if not re.search(r'[!@#$%^&*(),.?":{}|<>]', password):
            raise ValueError("Password must contain at least one special character")

        # Hash the password
        self.__password_hash = self._hash_password(password)

    def _hash_password(self, password):
        """Private method to hash password"""
        salt = "user_account_salt_2025"  # In production, use random salt per user
        return hashlib.sha256((password + salt).encode()).hexdigest()

    def _verify_password(self, password):
        """Private method to verify password"""
        return self._hash_password(password) == self.__password_hash

    def login(self, password):
        """Attempt to login with password"""
        if not self.__is_active:
            return {"success": False, "message": "Account is deactivated"}

        if self.is_locked:
            return {"success": False, "message": "Account is temporarily locked due to failed login attempts"}

        if self._verify_password(password):
            # Successful login
            self.__last_login = datetime.now()
            self.__failed_login_attempts = 0
            self.__last_failed_login = None
            return {"success": True, "message": "Login successful"}
        else:
            # Failed login
            self.__failed_login_attempts += 1
            self.__last_failed_login = datetime.now()

            remaining_attempts = self.MAX_LOGIN_ATTEMPTS - self.__failed_login_attempts
            if remaining_attempts > 0:
                return {
                    "success": False,
                    "message": f"Invalid password. {remaining_attempts} attempts remaining"
                }
            else:
                return {
                    "success": False,
                    "message": f"Account locked for {self.LOCKOUT_DURATION} minutes due to multiple failed attempts"
                }

    def change_password(self, old_password, new_password):
        """Change password with old password verification"""
        if not self._verify_password(old_password):
            return {"success": False, "message": "Current password is incorrect"}

        try:
            self._set_password(new_password)
            return {"success": True, "message": "Password changed successfully"}
        except ValueError as e:
            return {"success": False, "message": str(e)}

    def deactivate_account(self):
        """Deactivate the account"""
        self.__is_active = False
        return "Account has been deactivated"

    def activate_account(self):
        """Activate the account"""
        self.__is_active = True
        self.__failed_login_attempts = 0
        self.__last_failed_login = None
        return "Account has been activated"

    def get_account_info(self):
        """Get safe account information"""
        return {
            "username": self.__username,
            "email": self.__email,
            "is_active": self.__is_active,
            "is_locked": self.is_locked,
            "created_at": self.__created_at,
            "last_login": self.__last_login,
            "failed_attempts": self.__failed_login_attempts
        }

    def __str__(self):
        return f"UserAccount: {self.__username} ({self.__email})"

    def __repr__(self):
        return f"UserAccount(username='{self.__username}', email='{self.__email}')"

# Using the encapsulated user account system
try:
    # Create user account with validation
    user = UserAccount("john_doe", "john.doe@example.com", "SecurePass123!")

    print(f"User created: {user}")
    print(f"Account info: {user.get_account_info()}")

    # Test login functionality
    print("\n--- Login Tests ---")
    result = user.login("WrongPassword")
    print(f"Wrong password: {result}")

    result = user.login("SecurePass123!")
    print(f"Correct password: {result}")

    # Test password change
    print("\n--- Password Change ---")
    result = user.change_password("SecurePass123!", "NewSecurePass456@")
    print(f"Password change: {result}")

    # Test account properties
    print(f"\nIs active: {user.is_active}")
    print(f"Is locked: {user.is_locked}")
    print(f"Username: {user.username}")
    print(f"Email: {user.email}")

    # Cannot access private attributes directly
    # print(user.__password_hash)  # AttributeError

except (ValueError, TypeError) as e:
    print(f"Error creating user: {e}")

# Test failed login attempts
print("\n--- Failed Login Test ---")
test_user = UserAccount("test_user", "test@example.com", "TestPass123!")

for i in range(4):  # Try 4 failed logins (more than MAX_LOGIN_ATTEMPTS)
    result = test_user.login("wrong_password")
    print(f"Attempt {i+1}: {result}")
```

## `BENEFITS OF ENCAPSULATION`

1. **Data Security**: Prevents unauthorized access to sensitive data
2. **Data Integrity**: Ensures data remains in a valid state through validation
3. **Modularity**: Internal implementation can change without affecting external code
4. **Debugging**: Easier to track data changes when access is controlled
5. **Code Maintenance**: Reduces coupling between different parts of the system

## `COMMON ENCAPSULATION QUESTIONS`

1. **Q: What's the difference between private and protected in Python?**

   - **A**: Protected (`_attribute`) is a convention suggesting internal use. Private (`__attribute`) uses name mangling for stronger privacy.

2. **Q: How do properties differ from regular attributes?**

   - **A**: Properties allow controlled access through getter/setter methods while maintaining attribute-like syntax.

3. **Q: Can you access private attributes from outside the class?**

   - **A**: Python's private attributes use name mangling, so they're accessible as `_ClassName__attribute` but this isn't recommended.

4. **Q: When should you use properties vs direct attributes?**

   - **A**: Use properties when you need validation, computed values, or want to control access to attributes.

5. **Q: What is name mangling in Python?**
   - **A**: Name mangling automatically changes `__attribute` to `_ClassName__attribute` to make it harder to access accidentally.
