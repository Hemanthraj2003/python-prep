# FILE HANDLING IN PYTHON

## What is a file ?

- File is a named location on disk to store information or data

- Files stores data permanently.

## Types of files.

- `Text files` - stores data in the form of character data.

- `Binary files` - stores data in the form of bytes / binary data.

## What is file handling ?

- it is a process which involes

  - `writing` data to a file

  - `reading` a data from a file

  - `updating` the data in the file

  - `deleting` the data from the file

## Accessing the file

To do any operation on a file, we need to open it first.

Syntax to open a file

```python
file_object = open("file_path", "mode")
```

- `file_object` - is a variable which holds the reference to the file.

- `file_path` - is the path of the file to be opened.

- `mode` - are classified into two types.

  | Basic modes       | advance modes               |
  | ----------------- | --------------------------- |
  | `r` - read mode   | `r+` - read and write mode  |
  | `w` - write mode  | `w+` - write and read mode  |
  | `a` - append mode | `a+` - append and read mode |

\
\
**`WRITE MODE (w)`**

- used to write data to a file.

- if the file is not present, it creates a new file. If the file is present, it overrides the existing data.
- we have 2 methods to write data to a file.

  1. `write()` - writes a string to the file.

  2. `writelines()` - writes a list of strings to the file.

```python
# Example of write mode
file = open("example.txt", "w")
file.write("Hello, World!\n")
file.writelines(["This is line 1.\n", "This is line 2.\n"])
file.close()
```

```text
# example.txt
Hello, World!
This is line 1.
This is line 2.
```

\
\
**`READ MODE (r)`**

- used to read data from a file.

- if the file is not present, it raises an error.

- we have 4 methods to read data from a file.

  1. `read()` - returns the entire content of the file.

  2. `read(n)` - return n characters from the file.

  3. `readline()` - returns a single line from the file from where the cursor is currently positioned.

  4. `readlines()` - returns a list of lines from the file.

- `cursor` - is a pointer which points to the current position in the file.

- cursor methods:

  1. `tell()` - returns the current position of the cursor.

  2. `seek(offset, whence)` - moves the cursor to a new position.

  - `offset` - number of bytes to move the cursor.

  - `whence` - reference point from where to move the cursor.

    | whence | Description                     |
    | ------ | ------------------------------- |
    | 0      | beginning of the file (default) |
    | 1      | current position of the cursor  |
    | 2      | end of the file                 |
