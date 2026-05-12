# 01-Core-Python-Fundamentals

## Variables & Data Types

- Variables store data: `name = "Alice"`
- Types: int, float, str, bool
- Check type: `type(variable)`

## Lists, Tuples, Sets, Dictionaries

- List: mutable, ordered: `my_list = [1, 2, 3]`
- Tuple: immutable, ordered: `my_tuple = (1, 2, 3)`
- Set: unique, unordered: `my_set = {1, 2, 3}`
- Dict: key-value pairs: `my_dict = {"key": "value"}`

## Loops

- For loop: `for item in list:`
- While loop: `while condition:`

## Conditions

- If/elif/else: `if x > 0: elif x == 0: else:`

## Functions

- Define: `def my_func(param): return result`
- Call: `my_func(arg)`

## Modules & Packages

- Import: `import module` or `from module import func`
- Install packages: `pip install package`

## Error Handling

- Try/except: `try: code except Exception as e: handle`

## File Handling

- Open: `with open("file.txt", "r") as f: data = f.read()`
- Write: `with open("file.txt", "w") as f: f.write("text")`

## List Comprehensions

- Create lists: `[x*2 for x in range(5)]`

## Virtual Environments

- Create: `python -m venv env`
- Activate: `env\Scripts\activate` (Windows)

## OOP Basics

- Class: `class MyClass: def __init__(self): pass`
- Object: `obj = MyClass()`

## JSON Handling

- Import json: `import json`
- Parse: `json.loads(string)`
- Serialize: `json.dumps(dict)`

## Working with APIs

- Use requests library for HTTP calls
