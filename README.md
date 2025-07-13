# Python Dictionary - English

A complete reference of Python commands translated to English in a direct and straightforward way.

## Table of Contents

- [Basic Commands](#basic-commands)
- [Control Structures](#control-structures)
- [Functions and Classes](#functions-and-classes)
- [Error Handling](#error-handling)
- [Logical Operators](#logical-operators)
- [Data Structures](#data-structures)
- [List Methods](#list-methods)
- [String Methods](#string-methods)
- [Import and Modules](#import-and-modules)
- [Graphical Interface (GUI)](#graphical-interface-gui)
- [Useful Built-in Functions](#useful-built-in-functions)
- [Symbols and Operators](#symbols-and-operators)
- [Special Values](#special-values)

---

## Basic Commands

Essential commands you'll use all the time:

| Command | Meaning |
|---------|---------|
| `print` | print/display a message on screen |
| `input` | receive user input |
| `len` | get the length/size of something |
| `type` | check the type of a variable |
| `str` | convert to text (string) |
| `int` | convert to integer number |
| `float` | convert to decimal number |
| `bool` | convert to true/false |

**Example:**
```python
name = input("Enter your name: ")  # receive user input
print(f"Hello, {name}!")           # print message on screen
length = len(name)                 # get length
print(f"Your name has {length} letters")
```

---

## Control Structures

To control your program's flow:

| Command | Meaning |
|---------|---------|
| `if` | if (condition) |
| `elif` | else if (condition) |
| `else` | else |
| `for` | for each item in... |
| `while` | while (condition is true) |
| `break` | stop/exit the loop |
| `continue` | skip to next loop iteration |
| `pass` | do nothing (placeholder) |

**Example:**
```python
for i in range(5):      # for each number from 0 to 4
    if i == 2:          # if i equals 2
        continue        # skip to next iteration
    elif i == 4:        # else if i equals 4
        break           # stop the loop
    else:               # else
        print(i)        # print the number
```

---

## Functions and Classes

To organize and reuse code:

| Command | Meaning |
|---------|---------|
| `def` | define a function |
| `return` | return a value from function |
| `class` | create a class |
| `self` | reference to the object itself |
| `super` | access parent class |
| `__init__` | class constructor method |

**Example:**
```python
def greeting(name):             # define function
    return f"Hello, {name}!"    # return value

class Person:                   # create class
    def __init__(self, name):   # constructor method
        self.name = name        # self = reference to object
    
    def speak(self):
        return f"{self.name} is speaking"
```

---

## Error Handling

To deal with problems in code:

| Command | Meaning |
|---------|---------|
| `try` | try to execute code |
| `except` | catch specific error |
| `finally` | always execute, with or without error |
| `raise` | raise an error |

**Example:**
```python
try:                            # try to execute
    number = int(input("Enter a number: "))
    result = 10 / number
except ValueError:              # catch specific error
    print("That's not a number!")
except ZeroDivisionError:       # catch another error
    print("Cannot divide by zero!")
finally:                        # always execute
    print("Operation finished")
```

---

## Logical Operators

For comparisons and logic:

| Command | Meaning |
|---------|---------|
| `and` | and (both conditions true) |
| `or` | or (at least one condition true) |
| `not` | not (invert logical value) |
| `in` | is contained in |
| `is` | is the same object as |

**Example:**
```python
age = 25
if age >= 18 and age <= 65:    # and (both conditions)
    print("Can work")
    
if "a" in "house":             # is contained in
    print("Has the letter 'a'")
    
if not False:                  # not (invert)
    print("This is true")
```

---

## Data Structures

To store and organize information:

| Command | Meaning |
|---------|---------|
| `list` | list (mutable array) |
| `tuple` | tuple (immutable array) |
| `dict` | dictionary (key-value) |
| `set` | set (no duplicates) |
| `range` | generate sequence of numbers |

**Example:**
```python
fruits = ["apple", "banana", "grape"]       # list
coordinates = (10, 20)                      # tuple
person = {"name": "John", "age": 30}        # dictionary
numbers = {1, 2, 3, 3, 4}                  # set (no duplicates)
sequence = range(1, 6)                     # generate numbers 1 to 5
```

---

## List Methods

To work with lists:

| Command | Meaning |
|---------|---------|
| `append` | add item to end of list |
| `insert` | insert item at specific position |
| `remove` | remove first item with specific value |
| `pop` | remove and return item by index |
| `sort` | sort list |
| `reverse` | reverse list order |

**Example:**
```python
list_items = [3, 1, 4]
list_items.append(2)         # add to end: [3, 1, 4, 2]
list_items.insert(0, 0)      # insert at position 0: [0, 3, 1, 4, 2]
list_items.remove(3)         # remove first 3: [0, 1, 4, 2]
last = list_items.pop()      # remove last: [0, 1, 4], last = 2
list_items.sort()            # sort: [0, 1, 4]
list_items.reverse()         # reverse: [4, 1, 0]
```

---

## String Methods

To manipulate text:

| Command | Meaning |
|---------|---------|
| `split` | split string into list |
| `join` | join list into string |
| `replace` | replace part of string |
| `upper` | convert to uppercase |
| `lower` | convert to lowercase |
| `strip` | remove spaces from edges |

**Example:**
```python
text = "  Python is cool  "
words = text.split()            # split into list: ["Python", "is", "cool"]
joined = "-".join(words)        # join with hyphen: "Python-is-cool"
new = text.replace("cool", "awesome") # replace: "  Python is awesome  "
uppercase = text.upper()        # uppercase: "  PYTHON IS COOL  "
lowercase = text.lower()        # lowercase: "  python is cool  "
clean = text.strip()            # no spaces: "Python is cool"
```

---

## Import and Modules

To use code from other libraries:

| Command | Meaning |
|---------|---------|
| `import` | import complete module |
| `from` | import something specific from module |
| `as` | give alias to import |

**Example:**
```python
import math                    # import complete module
from datetime import datetime  # import something specific
import pandas as pd           # give alias

# Usage:
print(math.pi)                # use complete module
now = datetime.now()          # use specific import
df = pd.DataFrame()           # use alias
```

---

## Graphical Interface (GUI)

To create windows and interfaces with tkinter:

### Basic Widgets

| Command | Meaning |
|---------|---------|
| `tkinter` | standard library for creating windows and interfaces |
| `Tk` | create main window |
| `Label` | static text on screen |
| `Button` | clickable button |
| `Entry` | text field for user input |
| `Text` | multiline text area |
| `Frame` | container to organize widgets |

### Layout and Organization

| Command | Meaning |
|---------|---------|
| `pack` | organize widgets automatically |
| `grid` | organize widgets in grid |
| `place` | position widget manually |

### Window Control

| Command | Meaning |
|---------|---------|
| `mainloop` | keep window open (main loop) |
| `geometry` | set window size and position |
| `title` | set window title |
| `destroy` | close window |

**Example:**
```python
import tkinter as tk

window = tk.Tk()                    # create main window
window.title("My Application")      # set title
window.geometry("300x200")          # set size

label = tk.Label(window, text="Hello!") # create text
button = tk.Button(window, text="Click") # create button

label.pack()                        # organize automatically
button.pack()                       # organize automatically

window.mainloop()                   # keep window open
```

### Other GUI Libraries

| Command | Meaning |
|---------|---------|
| `PyQt5` | advanced library for interfaces (tkinter alternative) |
| `Kivy` | library for mobile and desktop apps |
| `Pygame` | library for games and graphics |

---

## Useful Built-in Functions

Ready-made functions that make your life easier:

| Command | Meaning |
|---------|---------|
| `enumerate` | number items in a list |
| `zip` | combine multiple lists |
| `map` | apply function to each item |
| `filter` | filter items by condition |
| `sum` | sum numeric items |
| `max` | find largest value |
| `min` | find smallest value |
| `sorted` | return sorted list |
| `reversed` | return reverse iterator |
| `all` | check if all are true |
| `any` | check if any is true |

**Example:**
```python
numbers = [1, 2, 3, 4, 5]
names = ["Ana", "Bruno", "Carlos"]

# enumerate: number items
for i, name in enumerate(names):
    print(f"{i}: {name}")  # 0: Ana, 1: Bruno, 2: Carlos

# zip: combine lists
for num, name in zip(numbers, names):
    print(f"{num} - {name}")  # 1 - Ana, 2 - Bruno, 3 - Carlos

# map: apply function to each item
doubled = list(map(lambda x: x * 2, numbers))  # [2, 4, 6, 8, 10]

# filter: filter by condition
evens = list(filter(lambda x: x % 2 == 0, numbers))  # [2, 4]

# Aggregation functions
print(sum(numbers))     # 15 (sum)
print(max(numbers))     # 5 (largest)
print(min(numbers))     # 1 (smallest)
```

---

## Symbols and Operators

Special symbols you'll use:

| Symbol | Meaning |
|--------|---------|
| `==` | equal to (comparison) |
| `!=` | not equal to |
| `<=` | less than or equal |
| `>=` | greater than or equal |
| `+=` | add and assign |
| `-=` | subtract and assign |
| `*=` | multiply and assign |
| `/=` | divide and assign |
| `//` | integer division |
| `%` | remainder of division |
| `**` | power (raised to) |
| `[]` | access item by index |
| `{}` | dictionary or set |
| `()` | tuple or function call |
| `:` | slice or block definition |
| `#` | comment (ignored line) |
| `"""` | multiline string or docstring |

**Example:**
```python
# Comparisons
a = 10
b = 20
print(a == b)    # False (equal to)
print(a != b)    # True (not equal to)
print(a <= b)    # True (less than or equal)

# Assignment operators
a += 5          # a = a + 5 (now a = 15)
a *= 2          # a = a * 2 (now a = 30)

# Mathematical operators
print(7 // 2)   # 3 (integer division)
print(7 % 2)    # 1 (remainder)
print(2 ** 3)   # 8 (2 raised to 3)

# Access elements
list_items = [1, 2, 3]
print(list_items[0])     # 1 (first item)
print(list_items[1:3])   # [2, 3] (slice)
```

---

## Special Values

Special Python values:

| Value | Meaning |
|-------|---------|
| `None` | null/empty value |
| `True` | true |
| `False` | false |
| `global` | global variable |
| `nonlocal` | variable from outer scope |
| `lambda` | anonymous function (one line) |
| `with` | context manager |
| `yield` | generate value in generator function |
| `del` | delete variable or item |

**Example:**
```python
# None: null value
result = None
if result is None:
    print("No result")

# True/False: booleans
active = True
if active:
    print("It's active")

# lambda: anonymous function
double = lambda x: x * 2
print(double(5))  # 10

# with: context manager
with open("file.txt", "r") as file:
    content = file.read()
# File is automatically closed
```

---

## How to Use This Dictionary

### Quick Search

Use Ctrl+F (or Cmd+F on Mac) to quickly find any command.

### Contributing

Found a command that's missing? Feel free to contribute to this dictionary!

### Study Tips

1. **Start with basics**: Focus first on basic commands and control structures
2. **Practice with examples**: Each section has practical examples for you to test
3. **Use as reference**: You don't need to memorize everything, use this dictionary for reference
4. **Experiment**: The best way to learn is by practicing!

---

## License

This dictionary is open source and can be used freely in your projects and studies.

---

**Last updated:** July 2025  
**Version:** 1.0

*"Python is a language that lets you express ideas clearly and concisely. This dictionary helps you do that in English!"*