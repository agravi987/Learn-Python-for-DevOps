# 01-Core-Python-Fundamentals 🐍

Python fundamentals are the base layer for every DevOps automation script. Before writing cloud, Docker, Kubernetes, or CI/CD automation, you need to be comfortable with variables, data structures, control flow, functions, files, errors, and packages.

## 🎯 What You Will Learn

- How Python stores and organizes data.
- How to make decisions with conditions.
- How to repeat work with loops.
- How to reuse logic with functions and modules.
- How to read/write files and handle errors safely.
- How to structure a small automation script like a real DevOps utility.

## 🧱 Variables & Data Types

A variable is a name that points to a value. In automation, variables often store server names, file paths, API URLs, ports, usernames, or environment names.

```python
app_name = "payment-api"
replicas = 3
cpu_limit = 0.5
is_production = True

print(type(app_name))      # str
print(type(replicas))      # int
print(type(cpu_limit))     # float
print(type(is_production)) # bool
```

Common data types:

- `str` stores text, such as `"dev-server-01"`.
- `int` stores whole numbers, such as `8080`.
- `float` stores decimal numbers, such as `75.5`.
- `bool` stores `True` or `False`.
- `None` represents an empty or missing value.

## 📦 Lists, Tuples, Sets, Dictionaries

DevOps scripts usually work with collections: lists of servers, dictionaries of configuration, sets of unique ports, and tuples for fixed values.

```python
servers = ["web-01", "web-02", "db-01"]          # list
region = ("ap-south-1", "mumbai")                # tuple
open_ports = {22, 80, 443, 443}                  # set removes duplicates
config = {"env": "prod", "replicas": 3}          # dictionary

print(servers[0])
print(config["env"])
```

Use them like this:

- Use a `list` when order matters and values can change.
- Use a `tuple` when values should stay fixed.
- Use a `set` when you need unique values.
- Use a `dict` when you need key-value configuration.

## 🔁 Loops

Loops repeat actions. This is perfect for checking many servers, reading many files, or calling many APIs.

```python
servers = ["web-01", "web-02", "db-01"]

for server in servers:
    print(f"Checking {server}")
```

`while` loops continue until a condition becomes false:

```python
attempt = 1

while attempt <= 3:
    print(f"Deployment attempt {attempt}")
    attempt += 1
```

## 🚦 Conditions

Conditions help scripts make decisions.

```python
cpu_usage = 87

if cpu_usage >= 90:
    print("Critical alert")
elif cpu_usage >= 75:
    print("Warning alert")
else:
    print("System healthy")
```

In DevOps, conditions are used for alerts, retries, validations, deployment checks, and rollback decisions.

## 🧩 Functions

A function groups logic so you can reuse it.

```python
def build_alert(service, status):
    return f"{service} is currently {status}"

message = build_alert("payment-api", "down")
print(message)
```

Good function habits:

- Give functions clear names.
- Keep one function focused on one job.
- Return values instead of only printing.
- Add parameters for values that may change.

## 📚 Modules & Packages

A module is a Python file you can import. A package is a collection of modules.

```python
import os
from pathlib import Path

current_dir = os.getcwd()
readme_exists = Path("README.md").exists()
```

Install third-party packages with:

```bash
pip install requests
```

For DevOps, common packages include `requests`, `pyyaml`, `boto3`, `docker`, `kubernetes`, `pytest`, and `psutil`.

## 🛡️ Error Handling

Automation should fail clearly instead of crashing without explanation.

```python
try:
    with open("config.json", "r") as file:
        data = file.read()
except FileNotFoundError:
    print("Config file was not found")
except Exception as error:
    print(f"Unexpected error: {error}")
```

Use `try/except` around file operations, API calls, database work, cloud actions, and shell commands.

## 📄 File Handling

Many automation scripts read config files and write reports.

```python
from pathlib import Path

report_path = Path("health-report.txt")
report_path.write_text("service=api status=healthy\n")

content = report_path.read_text()
print(content)
```

`pathlib` is usually cleaner than older `os.path` code because it gives you readable object-style paths.

## ⚡ List Comprehensions

List comprehensions create lists in a short, readable way.

```python
servers = ["web-01", "web-02", "db-01"]
web_servers = [server for server in servers if server.startswith("web")]

print(web_servers)
```

Use them when the logic is simple. If the logic becomes hard to read, use a normal `for` loop.

## 🧪 Virtual Environments

A virtual environment keeps project dependencies separate.

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Linux/macOS:

```bash
source .venv/bin/activate
```

Then install packages inside the environment:

```bash
pip install requests pyyaml
```

## 🏗️ OOP Basics

Object-oriented programming helps when you want to group data and behavior.

```python
class Service:
    def __init__(self, name, port):
        self.name = name
        self.port = port

    def describe(self):
        return f"{self.name} runs on port {self.port}"

api = Service("payment-api", 8080)
print(api.describe())
```

You do not need OOP for every script, but it becomes useful when your automation grows.

## 🧾 JSON Handling

JSON is common in APIs, cloud services, Docker, Kubernetes, and configuration.

```python
import json

service = {"name": "payment-api", "replicas": 3}

json_text = json.dumps(service, indent=2)
print(json_text)

loaded_service = json.loads(json_text)
print(loaded_service["name"])
```

## 🌐 Working with APIs

Python can call APIs using the `requests` package.

```python
import requests

response = requests.get("https://api.github.com")
print(response.status_code)
print(response.json()["current_user_url"])
```

APIs usually return JSON, so Python dictionaries and lists are very important.

## 🛠️ Mini Project: Service Health Report

Goal: Build a small script that reads service data, checks status values, and writes a health report.

### Step 1: Create Sample Data

Create a file named `services.json`:

```json
[
  {"name": "frontend", "status": "healthy", "port": 80},
  {"name": "payment-api", "status": "warning", "port": 8080},
  {"name": "database", "status": "critical", "port": 5432}
]
```

### Step 2: Read the JSON File

```python
import json
from pathlib import Path

services = json.loads(Path("services.json").read_text())
```

### Step 3: Convert Status into Messages

```python
def status_message(service):
    if service["status"] == "healthy":
        return f"✅ {service['name']} is healthy"
    if service["status"] == "warning":
        return f"⚠️ {service['name']} needs attention"
    return f"🚨 {service['name']} is critical"
```

### Step 4: Generate the Report

```python
lines = []

for service in services:
    lines.append(status_message(service))

report = "\n".join(lines)
Path("health-report.txt").write_text(report)
print(report)
```

### Step 5: Improve It

- Add a count of healthy, warning, and critical services.
- Ask the user for the input filename.
- Add error handling if `services.json` is missing.
- Store the report filename in a variable.

## ✅ Quick Revision

- Variables store values.
- Lists and dictionaries are the most common DevOps data structures.
- Loops repeat work.
- Conditions make decisions.
- Functions make code reusable.
- Error handling makes scripts safer.
- Files and JSON are essential for automation.
