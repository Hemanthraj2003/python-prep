# SQL-LITE Database

SQLite is a lightweight, disk-based database that doesn’t require a separate server process and allows access to the database using a nonstandard variant of the SQL query language.

## How to use SQLite in Python

Python has a built-in module called `sqlite3` that allows you to work with SQLite databases. Here’s a simple example of how to use it:

```python
import sqlite3

# Connect to a database (or create one if it doesn't exist)
conn = sqlite3.connect('example.db')

# Create a cursor object
cursor = conn.cursor()

# Create a table
cursor.execute('''CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, name TEXT, age INTEGER)''')

# Insert some data
cursor.execute('''INSERT INTO users (name, age) VALUES (?, ?)''', ('Alice', 30))
cursor.execute('''INSERT INTO users (name, age) VALUES (?, ?)''', ('Bob', 25))

# Commit the changes
conn.commit()

# Query the data
cursor.execute('''SELECT * FROM users''')
rows = cursor.fetchall()
for row in rows:
    print(row)

# Close the connection
conn.close()
```
