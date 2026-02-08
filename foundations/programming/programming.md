---
title: "Programming"
description: "Learn Python fundamentals — syntax, data structures, functions, file I/O, virtual environments, and package management."
position: 7
icon: "code"
---

# Programming

Shell scripts handle system tasks well, but larger automation, tooling, and infrastructure projects call for a general-purpose programming language. Python is the most widely used language in infrastructure, DevOps, and data engineering.

## What Is Python?

Python is a high-level, interpreted programming language known for its readability and broad ecosystem. It runs on every major operating system and is used for automation scripts, CLI tools, web APIs, infrastructure management (Ansible, Pulumi), and machine learning. Unlike compiled languages such as C or Go, Python executes code line by line through an interpreter, which makes it fast to write and test even if it runs slower than compiled alternatives. For the automation, tooling, and infrastructure work you will do, that tradeoff is almost always worth it.

## Why It Matters

Python appears everywhere in modern infrastructure: writing Lambda functions, building CLI tools, scripting cloud API interactions, processing data, and extending CI/CD pipelines. Understanding Python gives you access to thousands of libraries and frameworks that accelerate your work. When you reach the [CI/CD](/learn/foundations/cicd/) and [Infrastructure as Code](/learn/foundations/iac/) sections later in this path, you will see Python used as the glue that holds workflows together.

## What You'll Learn

- Python syntax and basic data types
- Data structures: lists, dictionaries, tuples, and sets
- Functions and modules
- File I/O and working with JSON/YAML
- Error handling with try/except
- Virtual environments and `pip` package management
- Writing and running Python scripts

---

## Getting Started

### Checking Your Installation

Most Linux distributions and macOS ship with Python 3 pre-installed. Verify it:

```bash
python3 --version
```

```
Python 3.12.3
```

If the command is not found, install Python through your package manager. On Ubuntu:

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv
```

### The REPL

Python includes an interactive interpreter called the **REPL** (Read-Eval-Print Loop). Launch it by typing `python3` with no arguments:

```bash
python3
```

```
Python 3.12.3 (main, Apr  9 2025, 08:52:47) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

The `>>>` prompt means Python is waiting for input. Type an expression and press Enter to see the result immediately:

```python
>>> 2 + 2
4
>>> "hello".upper()
'HELLO'
>>> exit()
```

The REPL is invaluable for testing snippets before putting them in a script. Use it constantly.

### Running Scripts

Create a file called `hello.py`:

```python
print("Hello from Python")
```

Run it:

```bash
python3 hello.py
```

```
Hello from Python
```

The `print()` function sends output to the terminal, the same role `echo` plays in Bash. You will use it for debugging, logging, and user-facing output throughout every script you write.

> **Try It**: Open the Python REPL with `python3`. Type `print("Hello, world!")` and press Enter. Then try a math expression like `100 / 3`. Type `exit()` to leave the REPL. Next, create a file called `greet.py` that prints your name, and run it with `python3 greet.py`.

---

## Variables and Data Types

### Variables

In Python, you create a variable by assigning a value. There is no type declaration keyword:

```python
name = "cloudchase"
age = 30
pi = 3.14159
active = True
```

Variable names must start with a letter or underscore, contain only letters, digits, and underscores, and are case-sensitive (`Name` and `name` are different variables). Python convention is `snake_case` for variable and function names.

### Basic Types

| Type | Example | Description |
|---|---|---|
| `str` | `"hello"` | Text (string) |
| `int` | `42` | Whole number |
| `float` | `3.14` | Decimal number |
| `bool` | `True` / `False` | Boolean (note the capital T and F) |
| `NoneType` | `None` | Represents "no value" or "nothing" |

Use `type()` to inspect a variable's type:

```python
name = "cloudchase"
print(type(name))    # <class 'str'>

count = 10
print(type(count))   # <class 'int'>
```

### Type Conversion

Convert between types with built-in functions:

```python
# String to integer
port = int("8080")
print(port + 1)          # 8081

# Integer to string
version = str(3)
print("Python " + version)  # Python 3

# String to float
ratio = float("0.75")
print(ratio)             # 0.75

# Any value to boolean
print(bool(0))           # False
print(bool(""))          # False
print(bool("hello"))     # True
```

The rules for boolean conversion matter: `0`, `0.0`, `""` (empty string), `None`, and empty collections (`[]`, `{}`, `()`) are all `False`. Everything else is `True`. You will rely on this behavior in conditionals later.

---

## Strings

Strings are the data type you will work with most often in infrastructure scripts, whether you are building file paths, formatting log output, or constructing API requests.

### Creating Strings

Single quotes and double quotes are interchangeable:

```python
message = "Hello, world"
message = 'Hello, world'
```

Use the other quote type when your string contains one kind:

```python
html = '<div class="container">content</div>'
sql = "SELECT * FROM users WHERE name = 'admin'"
```

### f-strings

Formatted string literals (f-strings) let you embed expressions directly in a string. Prefix the string with `f`:

```python
name = "cloudchase"
region = "us-east-1"
print(f"Deploying as {name} to {region}")
```

```
Deploying as cloudchase to us-east-1
```

You can put any expression inside the curly braces:

```python
count = 5
print(f"There are {count * 2} items")  # There are 10 items
```

f-strings are the modern, preferred way to build strings in Python. You will see older approaches (`%` formatting and `.format()`) in existing codebases, but use f-strings in new code.

### Common String Methods

| Method | Description | Example |
|---|---|---|
| `.upper()` | Uppercase | `"hello".upper()` returns `"HELLO"` |
| `.lower()` | Lowercase | `"HELLO".lower()` returns `"hello"` |
| `.strip()` | Remove leading/trailing whitespace | `"  hi  ".strip()` returns `"hi"` |
| `.split()` | Split into a list | `"a,b,c".split(",")` returns `["a", "b", "c"]` |
| `.join()` | Join a list into a string | `",".join(["a", "b", "c"])` returns `"a,b,c"` |
| `.replace()` | Replace substring | `"hello".replace("l", "L")` returns `"heLLo"` |
| `.startswith()` | Check prefix | `"hello".startswith("he")` returns `True` |
| `.endswith()` | Check suffix | `"file.py".endswith(".py")` returns `True` |
| `len()` | Length (built-in function) | `len("hello")` returns `5` |

### String Slicing

Access individual characters or substrings using bracket notation. Indexing starts at 0:

```python
path = "/var/log/syslog"

print(path[0])       # /
print(path[5:8])     # log
print(path[-6:])     # syslog
print(path[:4])      # /var
```

The syntax is `string[start:stop]` where `start` is inclusive and `stop` is exclusive. Negative indices count from the end.

### Multiline Strings

Triple quotes create multiline strings. This is useful for documentation, templates, and embedded configuration:

```python
config_template = """
server:
  host: {host}
  port: {port}
  debug: false
"""
```

---

## Numbers and Math

Python handles integers and floating-point numbers natively. For infrastructure scripts, you will use math for capacity calculations, threshold checks, and data transformations.

### Arithmetic Operators

| Operator | Operation | Example | Result |
|---|---|---|---|
| `+` | Addition | `10 + 3` | `13` |
| `-` | Subtraction | `10 - 3` | `7` |
| `*` | Multiplication | `10 * 3` | `30` |
| `/` | Division | `10 / 3` | `3.333...` |
| `//` | Floor division | `10 // 3` | `3` |
| `%` | Modulo (remainder) | `10 % 3` | `1` |
| `**` | Exponent | `2 ** 10` | `1024` |

Division with `/` always returns a float, even if the result is a whole number (`10 / 2` returns `5.0`). Use `//` when you want an integer result.

Modulo (`%`) is useful for checking divisibility or cycling through values:

```python
for i in range(10):
    if i % 3 == 0:
        print(f"{i} is divisible by 3")
```

### The math Module

For more advanced operations, import the `math` module:

```python
import math

print(math.sqrt(144))     # 12.0
print(math.ceil(4.2))     # 5
print(math.floor(4.8))    # 4
print(math.log2(1024))    # 10.0
```

---

## Data Structures

Data structures let you organize and manipulate collections of values. Python's four built-in collection types cover nearly every use case you will encounter.

### Lists

A list is an ordered, mutable collection. Think of it as an array that can hold any mix of types (though in practice you usually keep one type per list).

```python
servers = ["web-01", "web-02", "db-01"]
ports = [80, 443, 5432]
```

**Indexing and slicing** work the same as strings:

```python
print(servers[0])       # web-01
print(servers[-1])      # db-01
print(servers[1:])      # ["web-02", "db-01"]
```

**Common list operations**:

```python
# Adding items
servers.append("cache-01")           # add to end
servers.extend(["worker-01", "worker-02"])  # add multiple items

# Removing items
servers.pop()                        # remove and return last item
servers.remove("db-01")             # remove by value

# Length
print(len(servers))

# Check membership
if "web-01" in servers:
    print("web-01 is in the list")

# Sorting
ports = [443, 80, 5432, 22]
ports.sort()                         # sorts in place: [22, 80, 443, 5432]
sorted_ports = sorted(ports)         # returns a new sorted list, original unchanged
```

**List comprehensions** let you build lists with a concise syntax. They are one of Python's most useful features:

```python
# Without comprehension
squares = []
for x in range(10):
    squares.append(x ** 2)

# With comprehension
squares = [x ** 2 for x in range(10)]

# Filtering with a condition
even_squares = [x ** 2 for x in range(10) if x % 2 == 0]
# [0, 4, 16, 36, 64]

# Practical: uppercase all server names
upper_servers = [s.upper() for s in servers]
```

### Dictionaries

A dictionary stores key-value pairs. It is the Python equivalent of a hash map or associative array. Dictionaries are everywhere in infrastructure code because they naturally represent configuration, API responses, and structured data.

```python
server = {
    "hostname": "web-01",
    "ip": "10.0.1.10",
    "port": 80,
    "healthy": True,
}
```

**Accessing values**:

```python
print(server["hostname"])         # web-01

# .get() returns None (or a default) instead of raising an error for missing keys
print(server.get("region"))           # None
print(server.get("region", "us-east-1"))  # us-east-1
```

Use `.get()` when the key might not exist. Direct bracket access raises a `KeyError` if the key is missing, which will crash your script.

**Useful methods**:

```python
print(server.keys())     # dict_keys(["hostname", "ip", "port", "healthy"])
print(server.values())   # dict_values(["web-01", "10.0.1.10", 80, True])
print(server.items())    # dict_items([("hostname", "web-01"), ("ip", "10.0.1.10"), ...])
```

**Adding and updating**:

```python
server["region"] = "us-east-1"   # add a new key
server["port"] = 443             # update an existing key
```

**Server inventory example** -- a real-world pattern you will see in infrastructure tools:

```python
inventory = {
    "web-01": {"ip": "10.0.1.10", "role": "frontend", "status": "running"},
    "web-02": {"ip": "10.0.1.11", "role": "frontend", "status": "running"},
    "db-01":  {"ip": "10.0.2.10", "role": "database", "status": "running"},
}

# Access nested values
print(inventory["db-01"]["ip"])   # 10.0.2.10

# Iterate over all servers
for name, info in inventory.items():
    print(f"{name}: {info['role']} at {info['ip']}")
```

**Dictionary comprehensions**:

```python
# Create a quick lookup of server names to IPs
ip_lookup = {name: info["ip"] for name, info in inventory.items()}
# {"web-01": "10.0.1.10", "web-02": "10.0.1.11", "db-01": "10.0.2.10"}
```

### Tuples

A tuple is an ordered, **immutable** collection. Once created, you cannot add, remove, or change its elements.

```python
coordinates = (40.7128, -74.0060)
rgb_green = (0, 255, 0)
```

**Unpacking** lets you assign tuple elements to variables in one line:

```python
lat, lon = coordinates
print(f"Latitude: {lat}, Longitude: {lon}")

# Functions often return tuples for multiple values
def get_server_info():
    return "web-01", "10.0.1.10", 80

hostname, ip, port = get_server_info()
```

**When to use tuples over lists**: Use tuples for fixed collections that should not change, such as coordinates, RGB colors, database connection parameters, or function return values. Tuples are also slightly more memory-efficient than lists.

### Sets

A set is an unordered collection of unique values. Duplicates are automatically removed.

```python
regions = {"us-east-1", "us-west-2", "eu-west-1", "us-east-1"}
print(regions)   # {"us-east-1", "us-west-2", "eu-west-1"} — duplicate removed
```

**Operations**:

```python
regions.add("ap-southeast-1")
regions.remove("eu-west-1")      # raises KeyError if not found
regions.discard("eu-west-1")     # does nothing if not found

# Set math
prod_regions = {"us-east-1", "us-west-2", "eu-west-1"}
dev_regions = {"us-east-1", "eu-central-1"}

print(prod_regions | dev_regions)    # union — all regions
print(prod_regions & dev_regions)    # intersection — regions in both
print(prod_regions - dev_regions)    # difference — in prod but not dev
```

Sets are ideal for membership checks (`if item in my_set` is very fast), deduplication, and computing differences between two groups.

> **Try It**: Create a dictionary representing a cloud resource with keys for `name`, `type`, `region`, and `tags` (where tags is a list of strings). Print each key-value pair using a `for` loop with `.items()`. Then use a list comprehension to extract just the tags that start with `"env:"`.

---

## Control Flow

Control flow determines which code runs and how many times. Python uses indentation (not braces) to define blocks, so consistent indentation is mandatory. The standard is four spaces per level.

### Conditionals

```python
status_code = 503

if status_code == 200:
    print("OK")
elif status_code == 404:
    print("Not found")
elif status_code >= 500:
    print("Server error")
else:
    print(f"Unexpected status: {status_code}")
```

**Comparison operators**:

| Operator | Meaning |
|---|---|
| `==` | Equal to |
| `!=` | Not equal to |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal to |
| `>=` | Greater than or equal to |
| `in` | Membership (is value in collection?) |
| `not in` | Not a member |
| `is` | Identity (same object in memory) |
| `and` | Logical AND |
| `or` | Logical OR |
| `not` | Logical NOT |

**Truthy and falsy values**: As noted in the data types section, these values evaluate to `False` in a conditional: `False`, `None`, `0`, `0.0`, `""`, `[]`, `{}`, `()`, `set()`. Everything else is `True`. This lets you write clean checks:

```python
servers = []

if not servers:
    print("No servers found")

name = ""
if name:
    print(f"Hello, {name}")
else:
    print("Name is empty")
```

### for Loops

`for` loops iterate over any sequence (list, string, dictionary, range):

```python
# Iterate over a list
regions = ["us-east-1", "us-west-2", "eu-west-1"]
for region in regions:
    print(f"Checking {region}...")

# range() generates a sequence of numbers
for i in range(5):          # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 11):     # 1 through 10
    print(i)

for i in range(0, 20, 5):  # 0, 5, 10, 15 (step by 5)
    print(i)
```

**`enumerate()`** gives you both the index and the value:

```python
servers = ["web-01", "web-02", "db-01"]
for index, server in enumerate(servers):
    print(f"{index}: {server}")
```

```
0: web-01
1: web-02
2: db-01
```

**Iterating over dictionaries**:

```python
server = {"hostname": "web-01", "ip": "10.0.1.10", "port": 80}

# Keys only (default)
for key in server:
    print(key)

# Keys and values
for key, value in server.items():
    print(f"{key}: {value}")
```

### while Loops

`while` loops run as long as a condition is true:

```python
retries = 0
max_retries = 5

while retries < max_retries:
    print(f"Attempt {retries + 1}...")
    # Imagine an API call here
    retries += 1

print("All retries exhausted")
```

### break and continue

`break` exits the loop immediately. `continue` skips to the next iteration:

```python
# break — stop when you find what you need
for server in servers:
    if server == "db-01":
        print("Found the database server")
        break

# continue — skip items that don't match
for server in servers:
    if not server.startswith("web"):
        continue
    print(f"Web server: {server}")
```

> **Try It**: Write a script that creates a list of numbers from 1 to 20. Use a `for` loop with an `if/elif/else` to print "FizzBuzz" if the number is divisible by both 3 and 5, "Fizz" if only by 3, "Buzz" if only by 5, and the number itself otherwise.

---

## Functions

Functions let you organize code into reusable blocks. As your scripts grow beyond a few dozen lines, functions become essential for readability and maintenance.

### Defining Functions

```python
def greet(name):
    print(f"Hello, {name}!")

greet("cloudchase")   # Hello, cloudchase!
```

### Return Values

Use `return` to send a value back to the caller. A function without `return` (or with a bare `return`) returns `None`:

```python
def calculate_cost(hours, rate):
    return hours * rate

total = calculate_cost(160, 75)
print(f"Total cost: ${total}")   # Total cost: $12000
```

### Default Parameters

Parameters can have default values. This makes them optional when calling the function:

```python
def connect(host, port=443, timeout=30):
    print(f"Connecting to {host}:{port} (timeout: {timeout}s)")

connect("api.example.com")              # uses defaults: port=443, timeout=30
connect("api.example.com", 8080)         # port=8080, timeout=30
connect("api.example.com", timeout=60)   # port=443, timeout=60 (keyword argument)
```

### *args and **kwargs

`*args` collects extra positional arguments into a tuple. `**kwargs` collects extra keyword arguments into a dictionary. You will encounter these in library code and decorators:

```python
def log_event(message, *tags, **metadata):
    print(f"[{', '.join(tags)}] {message}")
    for key, value in metadata.items():
        print(f"  {key}: {value}")

log_event("Deployment started", "deploy", "prod", version="2.1.0", region="us-east-1")
```

```
[deploy, prod] Deployment started
  version: 2.1.0
  region: us-east-1
```

You do not need to use `*args` and `**kwargs` in every function. Know what they mean so you can read code that uses them.

### Scope

Variables defined inside a function are **local** -- they exist only within that function. Variables defined outside all functions are **global**:

```python
environment = "production"      # global

def deploy():
    target = "web-01"          # local — only accessible inside deploy()
    print(f"Deploying to {target} in {environment}")

deploy()
# print(target)                # this would raise NameError
```

Avoid modifying global variables from inside functions. If a function needs data, pass it as a parameter. If it produces a result, return it. This keeps functions predictable and testable.

### Docstrings

A docstring is a string literal that appears as the first statement inside a function (or class or module). It documents what the function does:

```python
def parse_log_line(line):
    """Parse a single log line and return a dictionary.

    Args:
        line: A string in the format 'TIMESTAMP LEVEL MESSAGE'.

    Returns:
        A dictionary with keys 'timestamp', 'level', and 'message'.
    """
    parts = line.split(" ", 2)
    return {
        "timestamp": parts[0],
        "level": parts[1],
        "message": parts[2],
    }
```

Docstrings are accessible at runtime via `help(parse_log_line)` and are used by documentation generators. Write them for any function that is not immediately obvious.

> **Try It**: Write a function called `check_disk` that takes two parameters: `used` (int) and `total` (int). It should calculate the percentage used (`used / total * 100`), then return `"critical"` if above 90, `"warning"` if above 70, or `"ok"` otherwise. Call it with several test values and print the results.

---

## File I/O

Reading and writing files is central to infrastructure work: parsing configuration files, processing logs, generating reports, and transforming data between formats.

### Reading Files

Always open files using the `with` statement. It guarantees the file is properly closed when the block ends, even if an error occurs:

```python
# Read entire file as one string
with open("/etc/hostname", "r") as f:
    content = f.read()
    print(content)

# Read all lines into a list
with open("/var/log/syslog", "r") as f:
    lines = f.readlines()
    print(f"Total lines: {len(lines)}")

# Iterate line by line (memory-efficient for large files)
with open("/var/log/syslog", "r") as f:
    for line in f:
        if "error" in line.lower():
            print(line.strip())
```

The third approach is best for large files because it reads one line at a time instead of loading the entire file into memory.

### Writing Files

```python
# Write (creates file or overwrites existing)
with open("output.txt", "w") as f:
    f.write("Line one\n")
    f.write("Line two\n")

# Append (adds to end of existing file)
with open("output.txt", "a") as f:
    f.write("Line three\n")
```

### Working with JSON

JSON is the default data format for APIs and many configuration files. Python's `json` module handles it natively:

```python
import json

# Read JSON from a file
with open("config.json", "r") as f:
    config = json.load(f)

print(config["database"]["host"])

# Write JSON to a file
data = {
    "servers": ["web-01", "web-02"],
    "region": "us-east-1",
    "replicas": 3,
}

with open("output.json", "w") as f:
    json.dump(data, f, indent=2)

# Convert between JSON strings and Python objects
json_string = json.dumps(data, indent=2)    # Python dict -> JSON string
parsed = json.loads(json_string)             # JSON string -> Python dict
```

| Function | Direction | Source |
|---|---|---|
| `json.load(f)` | JSON file to Python object | File |
| `json.dump(obj, f)` | Python object to JSON file | File |
| `json.loads(s)` | JSON string to Python object | String |
| `json.dumps(obj)` | Python object to JSON string | String |

The `s` at the end of `loads` and `dumps` stands for "string." This is an easy way to remember which function to use.

### Working with YAML

YAML is the configuration format for Kubernetes, Ansible, Docker Compose, GitHub Actions, and many other infrastructure tools. Python does not include a YAML parser in the standard library, so you need to install one:

```bash
pip install pyyaml
```

```python
import yaml

# Read YAML
with open("config.yaml", "r") as f:
    config = yaml.safe_load(f)

print(config["server"]["port"])

# Write YAML
data = {
    "server": {
        "host": "0.0.0.0",
        "port": 8080,
        "workers": 4,
    },
    "database": {
        "host": "db-01",
        "port": 5432,
    },
}

with open("output.yaml", "w") as f:
    yaml.safe_dump(data, f, default_flow_style=False)
```

Always use `safe_load()` and `safe_dump()` instead of `load()` and `dump()`. The unsafe versions can execute arbitrary Python code embedded in YAML files, which is a security risk.

> **Try It**: Create a JSON file called `servers.json` containing a list of three server dictionaries (each with `name`, `ip`, and `role` keys). Write a Python script that reads the file, loops through the servers, and prints a formatted summary of each one. Then modify the script to add a new server to the list and write the updated data back to the file.

---

## Error Handling

When your script reads a file that does not exist, calls an API that is down, or receives unexpected input, Python raises an **exception**. Without error handling, the exception crashes your script. The `try/except` block lets you catch exceptions and respond gracefully.

### Basic try/except

```python
try:
    with open("config.json", "r") as f:
        config = json.load(f)
except FileNotFoundError:
    print("Config file not found, using defaults")
    config = {"port": 8080, "debug": False}
except json.JSONDecodeError:
    print("Config file contains invalid JSON")
    config = {"port": 8080, "debug": False}
```

Always catch **specific** exceptions. A bare `except:` catches everything, including keyboard interrupts and system exits, which makes debugging much harder.

### else and finally

```python
try:
    with open("data.json", "r") as f:
        data = json.load(f)
except FileNotFoundError:
    print("File not found")
    data = None
else:
    # Runs only if no exception was raised
    print(f"Loaded {len(data)} records")
finally:
    # Runs no matter what — exception or not
    print("Operation complete")
```

`finally` is useful for cleanup tasks like closing network connections or releasing resources, though the `with` statement handles file closing for you.

### Raising Exceptions

You can raise your own exceptions to signal errors in your code:

```python
def set_replicas(count):
    if not isinstance(count, int):
        raise TypeError("Replica count must be an integer")
    if count < 1:
        raise ValueError("Replica count must be at least 1")
    print(f"Setting replicas to {count}")
```

### Practical Example: Safe File Reader

Here is a pattern you will use repeatedly in infrastructure scripts:

```python
import json

def load_config(path):
    """Load a JSON config file with error handling.

    Args:
        path: Path to the JSON configuration file.

    Returns:
        A dictionary with the configuration, or None on failure.
    """
    try:
        with open(path, "r") as f:
            config = json.load(f)
    except FileNotFoundError:
        print(f"Error: {path} not found")
        return None
    except PermissionError:
        print(f"Error: no permission to read {path}")
        return None
    except json.JSONDecodeError as e:
        print(f"Error: invalid JSON in {path}: {e}")
        return None
    else:
        print(f"Loaded config from {path}")
        return config

config = load_config("app_config.json")
if config is None:
    print("Falling back to defaults")
    config = {"port": 8080}
```

The `as e` syntax captures the exception object so you can include its message in your error output. This is invaluable for debugging.

> **Try It**: Write a function called `safe_divide` that takes two numbers and returns their quotient. Use `try/except` to handle `ZeroDivisionError` (return `None` and print a warning) and `TypeError` (for non-numeric input). Test it with `safe_divide(10, 3)`, `safe_divide(10, 0)`, and `safe_divide(10, "two")`.

---

## Virtual Environments

When you install a Python package with `pip`, it goes into a system-wide location by default. This creates problems when different projects need different versions of the same library. Virtual environments solve this by giving each project its own isolated set of packages.

### Why Virtual Environments Matter

Imagine you have two projects. Project A requires `requests==2.28.0` and Project B requires `requests==2.31.0`. Without isolation, installing one version overwrites the other. Virtual environments keep each project's dependencies separate, just like containers isolate applications.

### Creating and Using a Virtual Environment

```bash
# Create a virtual environment in a .venv directory
python3 -m venv .venv

# Activate it (Linux/macOS)
source .venv/bin/activate

# Your prompt changes to show the active environment
(.venv) cloudchase@ubuntu:~/project$
```

While activated, `python` and `pip` point to the virtual environment's copies, not the system ones:

```bash
which python
# /home/cloudchase/project/.venv/bin/python
```

### Installing Packages

```bash
# Install a single package
pip install requests

# Install a specific version
pip install requests==2.31.0

# Install multiple packages
pip install flask boto3 pyyaml
```

### Freezing and Reproducing Dependencies

The `requirements.txt` file is the standard way to record project dependencies. This file is critical for reproducibility and will be checked into [Version Control](/learn/foundations/version-control/) alongside your code.

```bash
# Save current packages and versions to a file
pip freeze > requirements.txt
```

The output looks like:

```
boto3==1.34.50
flask==3.0.2
pyyaml==6.0.1
requests==2.31.0
```

On a different machine (or in a CI/CD pipeline), recreate the exact same environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Deactivating

```bash
deactivate
```

This returns you to the system Python. The virtual environment still exists in `.venv/` -- you can reactivate it any time.

### Best Practices

- Create a `.venv` directory in every project root.
- Add `.venv/` to your `.gitignore` file. You commit `requirements.txt`, not the virtual environment itself.
- Always activate the virtual environment before running your project or installing packages.
- Run `pip freeze > requirements.txt` after adding or updating dependencies.

> **Try It**: Create a new directory called `~/pyproject`. Inside it, create a virtual environment with `python3 -m venv .venv` and activate it. Install the `requests` library with `pip install requests`. Run `pip freeze` to see the installed packages. Save them with `pip freeze > requirements.txt`. Deactivate with `deactivate`, then verify `pip freeze` shows different output (the system packages). Reactivate and confirm `requests` is still available.

---

## Practical Script: Config Reader and System Reporter

This section ties together everything you have learned into a complete script. The script reads a YAML configuration file and reports system metrics using Python standard library modules. This pattern -- load config, gather data, format output -- is the skeleton of most infrastructure automation scripts.

### The Configuration File

Create a file called `report_config.yaml`:

```yaml
report:
  title: "System Health Report"
  include:
    - platform
    - disk
    - python
  thresholds:
    disk_warning: 70
    disk_critical: 90
```

### The Script

Create a file called `system_report.py`:

```python
"""System reporter that reads a YAML config and prints system metrics."""

import json
import os
import platform
import shutil
import sys

try:
    import yaml
except ImportError:
    print("Error: pyyaml is not installed. Run: pip install pyyaml")
    sys.exit(1)


def load_config(path):
    """Load a YAML configuration file.

    Args:
        path: Path to the YAML config file.

    Returns:
        A dictionary with the parsed configuration.
    """
    try:
        with open(path, "r") as f:
            return yaml.safe_load(f)
    except FileNotFoundError:
        print(f"Error: config file '{path}' not found")
        sys.exit(1)
    except yaml.YAMLError as e:
        print(f"Error: invalid YAML in '{path}': {e}")
        sys.exit(1)


def get_platform_info():
    """Return a dictionary of platform details."""
    return {
        "system": platform.system(),
        "node": platform.node(),
        "release": platform.release(),
        "architecture": platform.machine(),
        "python_version": platform.python_version(),
    }


def get_disk_info(path="/"):
    """Return disk usage for the given path.

    Args:
        path: Filesystem path to check. Defaults to root.

    Returns:
        A dictionary with total, used, free (in GB), and percent used.
    """
    usage = shutil.disk_usage(path)
    total_gb = usage.total / (1024 ** 3)
    used_gb = usage.used / (1024 ** 3)
    free_gb = usage.free / (1024 ** 3)
    percent = (usage.used / usage.total) * 100
    return {
        "total_gb": round(total_gb, 2),
        "used_gb": round(used_gb, 2),
        "free_gb": round(free_gb, 2),
        "percent_used": round(percent, 1),
    }


def get_python_info():
    """Return details about the Python environment."""
    return {
        "executable": sys.executable,
        "version": sys.version.split()[0],
        "virtual_env": os.environ.get("VIRTUAL_ENV", "None"),
        "path_entries": len(sys.path),
    }


def check_disk_status(percent, thresholds):
    """Return a status string based on disk usage thresholds.

    Args:
        percent: Current disk usage percentage.
        thresholds: Dictionary with 'disk_warning' and 'disk_critical' keys.

    Returns:
        A string: 'CRITICAL', 'WARNING', or 'OK'.
    """
    if percent >= thresholds.get("disk_critical", 90):
        return "CRITICAL"
    elif percent >= thresholds.get("disk_warning", 70):
        return "WARNING"
    return "OK"


def print_section(title, data):
    """Print a formatted section of the report.

    Args:
        title: The section heading.
        data: A dictionary of key-value pairs to display.
    """
    print(f"\n--- {title} ---")
    for key, value in data.items():
        label = key.replace("_", " ").title()
        print(f"  {label}: {value}")


def main():
    """Main entry point for the system reporter."""
    config = load_config("report_config.yaml")
    report = config.get("report", {})
    sections = report.get("include", [])
    thresholds = report.get("thresholds", {})

    print(f"=== {report.get('title', 'System Report')} ===")

    if "platform" in sections:
        info = get_platform_info()
        print_section("Platform", info)

    if "disk" in sections:
        info = get_disk_info()
        status = check_disk_status(info["percent_used"], thresholds)
        info["status"] = status
        print_section("Disk Usage", info)

    if "python" in sections:
        info = get_python_info()
        print_section("Python Environment", info)

    print("\n=== Report Complete ===")


if __name__ == "__main__":
    main()
```

### Running the Script

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install pyyaml
python3 system_report.py
```

```
=== System Health Report ===

--- Platform ---
  System: Linux
  Node: ubuntu-server
  Release: 6.8.0-45-generic
  Architecture: x86_64
  Python Version: 3.12.3

--- Disk Usage ---
  Total Gb: 49.51
  Used Gb: 12.34
  Free Gb: 37.17
  Percent Used: 24.9
  Status: OK

--- Python Environment ---
  Executable: /home/cloudchase/project/.venv/bin/python
  Version: 3.12.3
  Virtual Env: /home/cloudchase/project/.venv
  Path Entries: 8

=== Report Complete ===
```

### What This Script Demonstrates

- **Imports**: `json`, `os`, `platform`, `shutil`, `sys`, and third-party `yaml`
- **Error handling**: Graceful failures for missing config files and missing packages
- **Functions**: Each task is isolated in its own function with a docstring
- **Dictionaries**: Used throughout for structured data
- **f-strings**: Used for all formatted output
- **File I/O**: YAML config file reading with `safe_load()`
- **Conditionals**: Threshold checking for disk status
- **Virtual environments**: The script detects whether it is running inside one
- **The `if __name__ == "__main__"` pattern**: Ensures `main()` runs only when the file is executed directly, not when imported as a module

This `if __name__` pattern is a Python convention you will see in every well-structured script. When Python runs a file directly, it sets `__name__` to `"__main__"`. When the file is imported by another module, `__name__` is set to the module name instead, so `main()` does not run automatically.

> **Try It**: Modify the script to add a fourth section called `"environment"` that reads and prints selected environment variables (`HOME`, `USER`, `SHELL`, `PATH`). Use `os.environ.get()` with sensible defaults for each variable. Add `"environment"` to the `include` list in your YAML config file and run the script again.

---

## Key Takeaways

- Python is the dominant language for infrastructure automation. Learn it and you unlock access to thousands of tools and libraries.
- The REPL (`python3`) is your testing playground. Use it to validate snippets before committing them to scripts.
- Variables need no type declaration. Know the basic types (`str`, `int`, `float`, `bool`, `None`) and how to convert between them.
- f-strings (`f"text {variable}"`) are the standard way to build formatted strings.
- Lists, dictionaries, tuples, and sets are the four core collection types. Dictionaries are the most important for infrastructure work because they represent structured data naturally.
- List and dictionary comprehensions provide concise, readable syntax for transforming data.
- Functions organize code into reusable blocks. Use parameters for input, `return` for output, and docstrings for documentation.
- Always open files with the `with` statement. Use `json.load()`/`json.dump()` for JSON and `yaml.safe_load()`/`yaml.safe_dump()` for YAML.
- Catch specific exceptions with `try/except`. Never use bare `except:` in production code.
- Virtual environments (`python3 -m venv .venv`) isolate project dependencies. Commit `requirements.txt` to version control, not the `.venv/` directory.
- The `if __name__ == "__main__"` pattern separates reusable modules from executable scripts.
