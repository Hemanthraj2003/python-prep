# STRINGS

Strings are sequence of characters enclosed within single quotes ( ' ' ), double quotes ( " " ), or triple quotes ( ''' ''' or """ """ ) in Python. They are used to represent text data.

```python
# Creating strings
single_quote_str = 'Hello, World!'
double_quote_str = "Hello, World!"
triple_quote_str = '''Hello,
World!'''
print(single_quote_str)  # Output: Hello, World!
print(double_quote_str)  # Output: Hello, World!
print(triple_quote_str)  # Output: Hello,
World!
```

## [STRING OPERATIONS](../basics/operators.md)

Strings support various operations such as concatenation, repetition, and slicing.

```python
# Concatenation
str1 = "Hello"
str2 = "World"
result = str1 + " " + str2
print(result)  # Output: Hello World

# Repetition
repeat_str = "Ha" * 3
print(repeat_str)  # Output: HaHaHa

# Slicing
sample_str = "Hello, World!"
print(sample_str[0:5])  # Output: Hello
print(sample_str[-6:])  # Output: World!
```

## STRING METHODS

Python provides a variety of built-in methods to manipulate strings. Here are some commonly used string methods:

| Method               | Description                                                                            | Example                                       |
| -------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------- |
| `lower()`            | Converts all characters in the string to lowercase.                                    | `"Hello".lower()` → `hello`                   |
| `upper()`            | Converts all characters in the string to uppercase.                                    | `"Hello".upper()` → `HELLO`                   |
| `strip()`            | Removes leading and trailing whitespace from the string.                               | `"  Hello  ".strip()` → `Hello`               |
| `replace(old, new)`  | Replaces occurrences of a substring with another substring.                            | `"Hello".replace("H", "J")` → `Jello`         |
| `split(separator)`   | Splits the string into a list of substrings based on the specified separator.          | `"a,b,c".split(",")` → `['a', 'b', 'c']`      |
| `join(iterable)`     | Joins elements of an iterable into a single string, using the string as the separator. | `"-".join(["2024","06","15"])` → `2024-06-15` |
| `find(substring)`    | Returns the lowest index of the substring if found, otherwise returns -1.              | `"Hello".find("e")` → `1`                     |
| `format()`           | Formats the string using placeholders.                                                 | `"Hi {}".format("Alice")` → `Hi Alice`        |
| `capitalize()`       | Capitalizes the first character of the string.                                         | `"hello".capitalize()` → `Hello`              |
| `title()`            | Capitalizes the first character of each word in the string.                            | `"hello world".title()` → `Hello World`       |
| `count(substring)`   | Returns the number of occurrences of a substring in the string.                        | `"Hello".count("l")` → `2`                    |
| `startswith(prefix)` | Returns `True` if the string starts with the specified prefix, otherwise `False`.      | `"Hello".startswith("He")` → `True`           |
| `endswith(suffix)`   | Returns `True` if the string ends with the specified suffix, otherwise `False`.        | `"Hello".endswith("lo")` → `True`             |
| `isalpha()`          | Returns `True` if all characters in the string are alphabetic, otherwise `False`.      | `"Hello".isalpha()` → `True`                  |
| `isdigit()`          | Returns `True` if all characters in the string are digits, otherwise `False`.          | `"123".isdigit()` → `True`                    |
| `isspace()`          | Returns `True` if all characters in the string are whitespace, otherwise `False`.      | `"   ".isspace()` → `True`                    |
| `len()`              | Returns the length of the string.                                                      | `len("Hello")` → `5`                          |
| `ord(char)`          | Returns the Unicode code point of a single character.                                  | `ord('a')` → `97`                             |
| `chr(code)`          | Returns the character corresponding to a Unicode code point.                           | `chr(97)` → `a`                               |

For more string methods, see the [reference](https://www.w3schools.com/python/python_ref_string.asp).

## STRING FORMATTING

Python provides several ways to format strings, including f-strings (formatted string literals), the `format()` method, and the `%` operator.

```python
name = "Alice"
age = 30
# Using f-strings (Python 3.6+)
formatted_str = f"My name is {name} and I am {age} years old."
print(formatted_str)
# Output: My name is Alice and I am 30 years old.

# Using the format() method
formatted_str = "My name is {} and I am {} years old.".format(name, age)
print(formatted_str)
# Output: My name is Alice and I am 30 years old.

# Using the % operator
formatted_str = "My name is %s and I am %d years old." % (name, age)
print(formatted_str)
# Output: My name is Alice and I am 30 years old.
```

## MULTI-LINE STRINGS

Multi-line strings can be created using triple quotes (''' or """).

```python
multi_line_str = '''This is a multi-line string.
It can span multiple lines.
You can include line breaks and indentation.'''
print(multi_line_str)
# Output:
# This is a multi-line string.
# It can span multiple lines.
# You can include line breaks and indentation.
```

## ESCAPE CHARACTERS

Escape character are used to include special charaters in a string. Some common escape characters include:
| Escape Character | Description | Example |
| ---------------- | ------------------------------- | ---------------------- |
| `\n` | New line | `"Hello\nWorld"` |
| `\t` | Tab | `"Hello\tWorld"` |
| `\\` | Backslash | `"C:\\Users\\Name"` |
| `\'` | Single quote | `'It\'s a test'` |
| `\"` | Double quote | `"He said, \"Hello\""` |
| `\r` | Carriage return | `"Hello\rWorld"` |

## RAW STRINGS

Raw strings treat backslashes (`\`) as literal characters and do not interpret them as escape characters. They are created by prefixing the string with an `r` or `R`.

```python
raw_str = r"C:\Users\Name\Documents"
print(raw_str)  # Output: C:\Users\Name\Documents
```
