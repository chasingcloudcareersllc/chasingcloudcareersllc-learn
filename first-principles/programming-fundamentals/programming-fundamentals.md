---
title: "Programming Fundamentals"
description: "Python, OOP, functional programming, testing, error handling, and project structure."
position: 5
icon: "code"
---

# Programming Fundamentals

You have a working Linux environment, you understand how the operating system manages processes and memory, and you can automate text processing with shell scripts and regular expressions. Shell scripting is powerful for system tasks, but it breaks down as complexity grows. Error handling is fragile, data structures are limited to strings and arrays, and there is no type system to catch mistakes before runtime. General-purpose programming languages exist to solve these problems.

This domain teaches Python from the ground up: syntax, data structures, functions, classes, testing, error handling, file I/O, and the execution model underneath. Python is the dominant language in infrastructure automation, DevOps tooling, cloud SDKs, and data processing. Every tool you encounter in later domains -- Ansible, Pulumi, pytest, cloud CLIs -- either is written in Python or has a Python SDK.

## Why It Matters

Shell scripts work until they don't. The moment you need to parse structured data, call an API, handle complex error conditions, or write maintainable code that a team can collaborate on, you need a real programming language. Python is that language for infrastructure work. It appears in Lambda functions, CLI tools, Terraform providers, CI/CD pipelines, monitoring agents, and configuration management. Understanding Python deeply -- not just copying snippets -- means you can read, write, debug, and extend the code that runs your infrastructure.

More fundamentally, learning to program teaches you to think in abstractions. Functions, classes, modules, and interfaces are tools for managing complexity. These concepts transfer to every language and every system you will ever work with.

## What You'll Learn

- Python syntax, dynamic typing, numbers, strings, booleans, and None
- Built-in data structures: lists, tuples, dicts, sets, and when to use each
- Control flow: loops, iterators, generators, and the itertools library
- Functions: parameters, closures, decorators, and the LEGB scope rule
- Object-oriented programming: classes, inheritance, dunder methods, SOLID principles
- Functional programming: pure functions, immutability, map/filter/reduce
- Error handling: exceptions, context managers, EAFP vs. LBYL
- File I/O and data formats: JSON, YAML, CSV, TOML
- Virtual environments and dependency management: venv, pip, uv, pyproject.toml
- Testing with pytest: fixtures, parametrize, mocking, coverage
- The standard library: pathlib, subprocess, argparse, logging, re, datetime, collections
- Python internals: bytecode, the GIL, reference counting, and memory management
- Project structure: modules, packages, entry points, and layout conventions

---

## A. Python Core

### Theory

Python is a dynamically typed, garbage-collected, interpreted language. "Dynamically typed" means variable types are determined at runtime, not at compile time. You do not declare a variable's type -- you assign a value, and the variable takes on that value's type.

```python
x = 42          # x is an int
x = "hello"     # now x is a str -- no error
x = [1, 2, 3]   # now x is a list
```

This flexibility speeds up development but shifts type errors from compile time to runtime. Type hints (covered in section D) mitigate this tradeoff.

**Variables and assignment.** A variable is a name bound to an object. Assignment does not copy data -- it creates a reference to an object in memory.

```python
a = [1, 2, 3]
b = a           # b points to the same list object
b.append(4)
print(a)        # [1, 2, 3, 4] -- a sees the change
```

This behavior is fundamental. Every time you pass a mutable object to a function, you pass a reference. Mutating it inside the function mutates it everywhere.

**Numbers.** Python has two primary numeric types:

| Type | Description | Example |
|------|-------------|---------|
| `int` | Arbitrary-precision integer | `2 ** 1000` works without overflow |
| `float` | IEEE 754 double-precision (64-bit) | `0.1 + 0.2 == 0.30000000000000004` |

Arbitrary-precision integers mean Python never overflows on integer arithmetic. This is unusual among programming languages and eliminates an entire class of bugs. Floats follow IEEE 754, which means they have finite precision. Never compare floats with `==`. Use `math.isclose()` or the `decimal` module for financial calculations.

```python
# Arbitrary precision -- no overflow
print(2 ** 256)
# 115792089237316195423570985008687907853269984665640564039457584007913129639936

# Float precision issues
print(0.1 + 0.2)           # 0.30000000000000004
print(0.1 + 0.2 == 0.3)    # False

import math
print(math.isclose(0.1 + 0.2, 0.3))  # True
```

**Strings.** Strings are immutable sequences of Unicode characters. Once created, a string cannot be modified -- every string operation returns a new string.

```python
name = "Python"
upper = name.upper()    # "PYTHON" -- new string, name unchanged
print(name)             # "Python" -- original intact

# f-strings (formatted string literals) -- the standard way to embed expressions
version = 3.12
print(f"Using {name} {version}")  # "Using Python 3.12"

# Slicing
text = "Hello, World!"
print(text[0:5])    # "Hello"
print(text[-6:])    # "orld!"
print(text[::2])    # "Hlo ol!"   -- every second character
print(text[::-1])   # "!dlroW ,olleH" -- reversed
```

f-strings are the idiomatic way to build strings in modern Python. They are faster and more readable than `%` formatting or `.format()`. Use them everywhere.

**Booleans.** Python booleans are `True` and `False`. Every object has a truth value. The following are falsy -- everything else is truthy:

| Falsy Value | Type |
|-------------|------|
| `False` | bool |
| `0`, `0.0`, `0j` | numeric zeros |
| `""` | empty string |
| `[]`, `()`, `{}`, `set()` | empty containers |
| `None` | NoneType |

**Short-circuit evaluation.** `and` returns the first falsy operand (or the last operand if all are truthy). `or` returns the first truthy operand (or the last operand if all are falsy).

```python
# Short-circuit examples
result = "" or "default"       # "default" -- "" is falsy
result = "value" and "other"   # "other"   -- both truthy, returns last
result = None or [] or "found" # "found"   -- first truthy value

# Common pattern: default values
name = user_input or "Anonymous"
```

**None.** `None` is Python's null value. It is a singleton -- there is exactly one `None` object. Always test for `None` with `is`, never `==`:

```python
x = None

# Correct
if x is None:
    print("x is None")

# Wrong -- works but violates convention and can break with __eq__ overrides
if x == None:
    print("don't do this")
```

**PEP 8, black, and ruff.** PEP 8 is the official Python style guide. It specifies 4-space indentation, snake_case for functions and variables, PascalCase for classes, UPPER_CASE for constants, and a maximum line length of 79 characters (relaxed to 88 by many teams). Rather than memorize every rule, use automated formatters:

- **black** -- an opinionated formatter that enforces consistent style with zero configuration
- **ruff** -- a fast linter and formatter that replaces flake8, isort, and black in a single tool

```bash
# Install and use
pip install ruff
ruff check .        # Lint
ruff format .       # Format
```

### Practice

```python
# Demonstrate core types and their behaviors

# Integer arithmetic -- no overflow
factorial_100 = 1
for i in range(1, 101):
    factorial_100 *= i
print(f"100! has {len(str(factorial_100))} digits")  # 158 digits

# String immutability
original = "immutable"
modified = original.replace("im", "")  # "mutable" -- new string
print(original)                         # "immutable" -- unchanged

# Boolean truth testing
values = [0, 1, "", "text", [], [1], None, 42]
for v in values:
    print(f"{str(v):>10} -> {bool(v)}")

# None identity
sentinel = None
print(sentinel is None)      # True
print(type(sentinel))        # <class 'NoneType'>
```

### Connection

These are not just syntax rules. Understanding that Python integers have arbitrary precision means you will never write overflow-checking code in Python the way you would in C. Understanding that strings are immutable means you know that building a string in a loop with `+=` creates a new string on every iteration -- use `"".join()` instead. Understanding truthiness means you can write idiomatic conditionals without explicit comparisons. Understanding that `=` creates references, not copies, prevents an entire category of bugs involving shared mutable state.

> **Try It**: Open a Python REPL. Compute `2 ** 1000` and verify Python handles it without error. Create a list, assign it to a second variable, modify the list through the second variable, and observe that the first variable sees the change. Test `0.1 + 0.2 == 0.3` and then `math.isclose(0.1 + 0.2, 0.3)`.

---

## B. Built-in Data Structures

### Theory

Python provides four core built-in data structures. Each has different performance characteristics. Choosing the right one is a daily decision.

**Lists** are ordered, mutable sequences. They are implemented as dynamic arrays.

| Operation | Time Complexity | Notes |
|-----------|----------------|-------|
| `append(x)` | O(1) amortized | Adds to end |
| `pop()` | O(1) | Removes from end |
| `insert(i, x)` | O(n) | Shifts elements right |
| `pop(i)` | O(n) | Shifts elements left |
| `x in list` | O(n) | Linear search |
| `list[i]` | O(1) | Direct index access |

```python
# List basics
numbers = [1, 2, 3, 4, 5]
numbers.append(6)           # [1, 2, 3, 4, 5, 6]
numbers.insert(0, 0)        # [0, 1, 2, 3, 4, 5, 6] -- O(n), shifts everything
last = numbers.pop()        # 6, list is now [0, 1, 2, 3, 4, 5]

# List comprehensions -- the Pythonic way to build lists
squares = [x ** 2 for x in range(10)]           # [0, 1, 4, 9, ..., 81]
evens = [x for x in range(20) if x % 2 == 0]   # [0, 2, 4, ..., 18]

# Nested comprehension
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [val for row in matrix for val in row]   # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**Tuples** are ordered, immutable sequences. Once created, they cannot be modified. They are used for fixed collections and as dictionary keys (because they are hashable).

```python
point = (3, 4)
# point[0] = 5  # TypeError: 'tuple' object does not support item assignment

# Named tuples add readability
from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(3, 4)
print(p.x, p.y)    # 3 4
print(p[0])         # 3 -- still indexable

# Tuple unpacking
first, *rest = (1, 2, 3, 4)
print(first)  # 1
print(rest)   # [2, 3, 4]
```

**Dicts** are hash tables mapping keys to values. They provide O(1) average-case lookup, insertion, and deletion.

```python
# Dict basics
config = {"host": "localhost", "port": 8080, "debug": True}
print(config["host"])         # "localhost"
# print(config["missing"])    # KeyError

# Safe access with .get()
timeout = config.get("timeout", 30)   # 30 (default)

# .setdefault() -- get value or set and return default
config.setdefault("timeout", 30)      # Sets config["timeout"] = 30

# Dict comprehension
squares = {x: x ** 2 for x in range(6)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

**defaultdict** provides a factory function for missing keys. **Counter** counts hashable objects.

```python
from collections import defaultdict, Counter

# defaultdict -- no KeyError, auto-creates values
word_lists = defaultdict(list)
word_lists["fruits"].append("apple")
word_lists["fruits"].append("banana")
# {"fruits": ["apple", "banana"]} -- no need to check if key exists

# Counter
text = "the quick brown fox jumps over the lazy dog"
word_counts = Counter(text.split())
print(word_counts.most_common(3))
# [('the', 2), ('quick', 1), ('brown', 1)]
```

**Sets** are unordered collections of unique, hashable elements. They provide O(1) membership testing.

```python
# Set basics
seen = {1, 2, 3, 4, 5}
print(3 in seen)    # True -- O(1), not O(n) like lists

# Set operations
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
print(a | b)    # {1, 2, 3, 4, 5, 6}  -- union
print(a & b)    # {3, 4}              -- intersection
print(a - b)    # {1, 2}              -- difference
print(a ^ b)    # {1, 2, 5, 6}        -- symmetric difference
```

**Deque** (double-ended queue) provides O(1) append and pop from both ends. Use it when you need a queue or need to add/remove from the front.

```python
from collections import deque

q = deque([1, 2, 3])
q.appendleft(0)    # O(1) -- [0, 1, 2, 3]
q.popleft()         # O(1) -- returns 0
q.append(4)         # O(1) -- [1, 2, 3, 4]

# Bounded deque -- keeps last N elements
recent = deque(maxlen=3)
for i in range(5):
    recent.append(i)
print(recent)       # deque([2, 3, 4])
```

**heapq** implements a min-heap on top of a list. Use it for priority queues.

```python
import heapq

heap = []
heapq.heappush(heap, 5)
heapq.heappush(heap, 1)
heapq.heappush(heap, 3)
print(heapq.heappop(heap))   # 1 -- smallest first
print(heapq.heappop(heap))   # 3

# Get N smallest/largest
data = [10, 4, 6, 1, 8, 3]
print(heapq.nsmallest(3, data))   # [1, 3, 4]
print(heapq.nlargest(2, data))    # [10, 8]
```

**Choosing the right structure:**

| Need | Use | Why |
|------|-----|-----|
| Ordered, indexed access | `list` | O(1) index, O(1) append |
| Immutable sequence, dict key | `tuple` | Hashable, fixed |
| Key-value lookup | `dict` | O(1) average lookup |
| Unique elements, membership test | `set` | O(1) membership |
| FIFO queue or deque | `deque` | O(1) both ends |
| Priority queue | `heapq` | O(log n) push/pop |
| Count occurrences | `Counter` | Built for counting |
| Missing keys with defaults | `defaultdict` | No KeyError |

### Practice

```python
# Practical example: log analysis with appropriate data structures
from collections import Counter, defaultdict

log_lines = [
    "2024-01-15 10:23:45 ERROR Database connection failed",
    "2024-01-15 10:24:01 INFO  Server started",
    "2024-01-15 10:24:15 ERROR Timeout waiting for response",
    "2024-01-15 10:25:00 WARN  High memory usage",
    "2024-01-15 10:25:30 ERROR Database connection failed",
    "2024-01-15 10:26:00 INFO  Health check passed",
]

# Count log levels
levels = Counter(line.split()[2] for line in log_lines)
print(levels)  # Counter({'ERROR': 3, 'INFO': 2, 'WARN': 1})

# Group messages by level
by_level = defaultdict(list)
for line in log_lines:
    parts = line.split(maxsplit=3)
    by_level[parts[2]].append(parts[3])

# Unique error messages
unique_errors = set(by_level["ERROR"])
print(f"Unique errors: {len(unique_errors)}")

# Most common error
error_counts = Counter(by_level["ERROR"])
print(f"Most common: {error_counts.most_common(1)}")
```

### Connection

In [Data Structures and Algorithms](/learn/first-principles/data-structures-and-algorithms/), you will implement these data structures from scratch -- building your own hash table, linked list, heap, and tree. Here, you learn to *use* them effectively. Knowing that `dict` is a hash table underneath tells you why key lookup is O(1) and why keys must be hashable. Knowing that `list` is a dynamic array tells you why `append` is O(1) but `insert(0, x)` is O(n). This knowledge directly informs the choices you make every time you write Python code.

> **Try It**: Write a script that reads `/var/log/syslog` (or any log file you have), counts the occurrences of each unique word, and prints the 10 most common words. Use `Counter`. Then find all unique IP addresses in the file using a `set`.

---

## C. Control Flow

### Theory

**Conditional statements.** `if/elif/else` branches execute code based on conditions. Python uses indentation, not braces, to define blocks.

```python
status_code = 404

if status_code == 200:
    print("OK")
elif status_code == 404:
    print("Not Found")
elif status_code >= 500:
    print("Server Error")
else:
    print(f"Other: {status_code}")

# Ternary expression
result = "even" if x % 2 == 0 else "odd"
```

**for loops.** Python's `for` iterates over any iterable -- lists, strings, ranges, files, generators, dictionaries.

```python
# range(start, stop, step)
for i in range(5):          # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 3):  # 2, 5, 8
    print(i)

# enumerate -- get index and value
names = ["Alice", "Bob", "Carol"]
for i, name in enumerate(names):
    print(f"{i}: {name}")

# zip -- iterate multiple sequences in parallel
keys = ["host", "port", "debug"]
values = ["localhost", 8080, True]
config = dict(zip(keys, values))
# {"host": "localhost", "port": 8080, "debug": True}
```

**while loops, break, continue.**

```python
# while with break
while True:
    line = input("Enter command (quit to exit): ")
    if line == "quit":
        break
    print(f"You entered: {line}")

# continue -- skip to next iteration
for i in range(10):
    if i % 3 == 0:
        continue
    print(i)  # 1, 2, 4, 5, 7, 8
```

**else clause on loops.** The `else` block executes only if the loop completes without hitting `break`. This is useful for search patterns.

```python
# Search with for/else
target = 7
for item in [1, 3, 5, 7, 9]:
    if item == target:
        print(f"Found {target}")
        break
else:
    print(f"{target} not found")  # Only runs if no break
```

**Match statement (structural pattern matching).** Introduced in Python 3.10, `match/case` provides pattern matching far more powerful than a switch statement.

```python
def handle_command(command):
    match command.split():
        case ["quit"]:
            return "Goodbye"
        case ["go", direction]:
            return f"Moving {direction}"
        case ["get", item] if item != "nothing":
            return f"Picked up {item}"
        case ["get", *items]:
            return f"Picked up {', '.join(items)}"
        case _:
            return f"Unknown command: {command}"

print(handle_command("go north"))       # Moving north
print(handle_command("get sword"))      # Picked up sword
print(handle_command("get a b c"))      # Picked up a, b, c
```

**Iterators and generators.** An iterator is any object that implements `__iter__()` and `__next__()`. A generator is a function that uses `yield` to produce values lazily -- one at a time, on demand, without storing the entire sequence in memory.

```python
# Generator function
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Lazy -- only computes values as needed
fib = fibonacci()
for _ in range(10):
    print(next(fib), end=" ")  # 0 1 1 2 3 5 8 13 21 34

# Generator expression (like list comprehension but lazy)
squares = (x ** 2 for x in range(1_000_000))  # No memory for 1M items
first_10 = [next(squares) for _ in range(10)]
```

The distinction between eager and lazy evaluation matters for infrastructure work. When processing a 10 GB log file, you cannot load it all into memory. Generators let you process it line by line.

```python
def read_large_file(path):
    """Yield lines from a file without loading it all into memory."""
    with open(path, encoding="utf-8") as f:
        for line in f:
            yield line.strip()

# Process a huge file with constant memory usage
error_count = sum(1 for line in read_large_file("/var/log/syslog") if "ERROR" in line)
```

**itertools.** The `itertools` module provides composable building blocks for iterator-based processing.

```python
import itertools

# chain -- concatenate iterables
combined = itertools.chain([1, 2], [3, 4], [5, 6])
print(list(combined))  # [1, 2, 3, 4, 5, 6]

# islice -- slice an iterator (lazy)
fib = fibonacci()
first_20 = list(itertools.islice(fib, 20))

# groupby -- group consecutive elements (must be pre-sorted)
data = [("web", "nginx"), ("web", "apache"), ("db", "postgres"), ("db", "mysql")]
for category, items in itertools.groupby(data, key=lambda x: x[0]):
    print(f"{category}: {[item[1] for item in items]}")
# web: ['nginx', 'apache']
# db: ['postgres', 'mysql']

# product -- Cartesian product
regions = ["us-east-1", "eu-west-1"]
sizes = ["small", "large"]
for region, size in itertools.product(regions, sizes):
    print(f"{region}-{size}")
# us-east-1-small, us-east-1-large, eu-west-1-small, eu-west-1-large

# permutations and combinations
print(list(itertools.permutations([1, 2, 3], 2)))
# [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]
```

### Practice

```python
# Build a log processor using generators and itertools
import itertools

def parse_log_lines(lines):
    """Parse log lines into structured dicts."""
    for line in lines:
        parts = line.strip().split(maxsplit=3)
        if len(parts) >= 4:
            yield {
                "date": parts[0],
                "time": parts[1],
                "level": parts[2],
                "message": parts[3],
            }

def filter_by_level(entries, level):
    """Yield only entries matching the given level."""
    for entry in entries:
        if entry["level"] == level:
            yield entry

# Pipeline: read -> parse -> filter -> group -> count
# Each step is lazy -- nothing executes until consumed
sample_lines = [
    "2024-01-15 10:00:00 ERROR db timeout",
    "2024-01-15 10:00:01 INFO  health ok",
    "2024-01-15 10:00:02 ERROR db timeout",
    "2024-01-15 10:00:03 WARN  memory high",
    "2024-01-15 10:00:04 ERROR auth failed",
]

entries = parse_log_lines(sample_lines)
errors = filter_by_level(entries, "ERROR")
for entry in errors:
    print(f"[{entry['time']}] {entry['message']}")
```

### Connection

Generators are the bridge between shell pipelines and Python programs. A shell pipeline like `grep ERROR /var/log/syslog | sort | uniq -c | sort -rn | head -10` processes data as a stream. Python generators work the same way -- each stage produces values on demand, passing them to the next stage without buffering the entire dataset. When you write infrastructure tools that process logs, metrics, or configuration files, this pattern keeps memory usage constant regardless of input size.

> **Try It**: Write a generator function `countdown(n)` that yields values from `n` down to 1. Use it in a `for` loop. Then write a generator expression that produces the squares of all odd numbers from 1 to 100. Consume it with `sum()` to get the total.

---

## D. Functions

### Theory

Functions are the primary unit of code organization in Python. A function takes inputs (parameters), performs computation, and returns a result.

**Parameters.** Python has five kinds of parameters:

```python
def example(
    positional,             # 1. positional (can also be keyword)
    /,                      # everything before / is positional-only
    normal,                 # 2. normal (positional or keyword)
    *,                      # everything after * is keyword-only
    keyword_only,           # 3. keyword-only
    default_val="hello",    # 4. with default value
):
    pass

def variadic(*args, **kwargs):   # 5. *args and **kwargs
    print(args)     # tuple of positional arguments
    print(kwargs)   # dict of keyword arguments
```

```python
# Practical function with mixed parameter types
def connect(
    host: str,
    port: int = 5432,
    *,
    timeout: int = 30,
    ssl: bool = True,
) -> str:
    """Connect to a database server.

    Args:
        host: Server hostname.
        port: Server port (default 5432).
        timeout: Connection timeout in seconds.
        ssl: Whether to use SSL.

    Returns:
        Connection status string.
    """
    return f"Connected to {host}:{port} (ssl={ssl}, timeout={timeout}s)"

# Valid calls
connect("db.example.com")
connect("db.example.com", 3306)
connect("db.example.com", timeout=60)
connect("db.example.com", 3306, ssl=False, timeout=10)

# Invalid -- keyword-only params cannot be positional
# connect("db.example.com", 3306, 60)  # TypeError
```

**Type hints.** Type hints document expected types and enable static analysis with mypy. They do not enforce types at runtime.

```python
from typing import Optional, Union

def parse_port(value: str) -> int:
    """Parse a port number from a string."""
    return int(value)

def find_user(user_id: int) -> Optional[dict]:
    """Return user dict or None if not found."""
    users = {1: {"name": "Alice"}, 2: {"name": "Bob"}}
    return users.get(user_id)

# Modern syntax (Python 3.10+)
def process(data: list[str] | None = None) -> dict[str, int]:
    if data is None:
        data = []
    return {item: len(item) for item in data}
```

**Docstrings.** The first string literal in a function, class, or module becomes its `__doc__` attribute. Follow Google, NumPy, or Sphinx conventions consistently.

**First-class functions.** Functions are objects. You can assign them to variables, pass them as arguments, and return them from other functions.

```python
def apply_operation(func, x, y):
    """Apply a function to two arguments."""
    return func(x, y)

def add(a, b):
    return a + b

result = apply_operation(add, 3, 4)  # 7

# Lambda -- anonymous function for simple expressions
result = apply_operation(lambda a, b: a * b, 3, 4)  # 12

# Sorting with key function
servers = [("web1", 80), ("db1", 5432), ("cache1", 6379)]
servers.sort(key=lambda s: s[1])  # Sort by port number
```

**Closures.** A closure is a function that captures variables from its enclosing scope. The inner function "remembers" the environment in which it was created.

```python
def make_multiplier(factor):
    """Return a function that multiplies by factor."""
    def multiplier(x):
        return x * factor    # captures 'factor' from enclosing scope
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)
print(double(5))    # 10
print(triple(5))    # 15

# nonlocal -- modify a closure variable
def make_counter():
    count = 0
    def increment():
        nonlocal count
        count += 1
        return count
    return increment

counter = make_counter()
print(counter())  # 1
print(counter())  # 2
print(counter())  # 3
```

**Decorators.** A decorator is a function that takes a function and returns a modified version of it. The `@decorator` syntax is syntactic sugar for `func = decorator(func)`.

```python
import functools
import time

def timer(func):
    """Measure and print execution time."""
    @functools.wraps(func)  # Preserves __name__, __doc__
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

@timer
def slow_function(n):
    """Simulate slow work."""
    time.sleep(n)
    return "done"

slow_function(1)  # "slow_function took 1.0012s"

# Decorator with arguments
def retry(max_attempts=3):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts:
                        raise
                    print(f"Attempt {attempt} failed: {e}. Retrying...")
        return wrapper
    return decorator

@retry(max_attempts=3)
def unreliable_api_call():
    import random
    if random.random() < 0.7:
        raise ConnectionError("Server unavailable")
    return {"status": "ok"}
```

Built-in decorators you will use constantly:

| Decorator | Purpose |
|-----------|---------|
| `@staticmethod` | Method that does not access instance or class |
| `@classmethod` | Method that receives the class, not the instance |
| `@property` | Turn a method into a read-only attribute |
| `@functools.wraps` | Preserve wrapped function's metadata |
| `@functools.lru_cache` | Memoize function results |

**Scope: the LEGB rule.** Python resolves variable names in four scopes, checked in order:

1. **L**ocal -- inside the current function
2. **E**nclosing -- inside enclosing function(s) (closures)
3. **G**lobal -- module-level
4. **B**uilt-in -- Python's built-in names (`print`, `len`, `int`, etc.)

```python
x = "global"

def outer():
    x = "enclosing"

    def inner():
        x = "local"
        print(x)    # "local" -- L

    inner()
    print(x)        # "enclosing" -- E

outer()
print(x)            # "global" -- G
print(len)          # <built-in function len> -- B
```

Use `global` to modify a global variable from within a function (avoid this -- it makes code hard to reason about). Use `nonlocal` to modify a variable in an enclosing scope.

### Practice

```python
import functools
import time

# Build a practical decorator: caching with TTL
def ttl_cache(seconds=60):
    """Cache function results with a time-to-live."""
    def decorator(func):
        cache = {}

        @functools.wraps(func)
        def wrapper(*args):
            now = time.time()
            if args in cache:
                result, timestamp = cache[args]
                if now - timestamp < seconds:
                    return result
            result = func(*args)
            cache[args] = (result, now)
            return result

        wrapper.cache = cache
        wrapper.clear_cache = cache.clear
        return wrapper
    return decorator

@ttl_cache(seconds=10)
def fetch_config(key):
    """Simulate fetching config from a slow source."""
    print(f"Fetching {key}...")
    time.sleep(0.1)
    return f"value_for_{key}"

# First call -- computes
print(fetch_config("db_host"))   # "Fetching db_host..." then "value_for_db_host"
# Second call -- cached
print(fetch_config("db_host"))   # "value_for_db_host" (no "Fetching..." print)
```

### Connection

Decorators are not academic curiosities. In [Software Engineering and Collaboration](/learn/first-principles/software-engineering-and-collaboration/), you will see them in Flask and FastAPI route handlers (`@app.route`), in pytest fixtures (`@pytest.fixture`), and in infrastructure code for retry logic, authentication, and rate limiting. Understanding closures is essential for understanding how these decorators work underneath. Understanding LEGB scope prevents subtle bugs where a function accidentally reads a global variable instead of a local one.

> **Try It**: Write a decorator `@log_calls` that prints the function name and arguments before each call and the return value after. Apply it to a function that calculates the area of a circle. Then write a closure `make_greeting(salutation)` that returns a function taking a name and returning `"{salutation}, {name}!"`.

---

## E. Object-Oriented Programming

### Theory

A class is a blueprint for creating objects. An object is an instance of a class. OOP organizes code around data and the operations that act on that data.

```python
class Server:
    """Represents a server in the infrastructure."""

    # Class attribute -- shared by all instances
    default_port = 22

    def __init__(self, hostname: str, ip: str, port: int | None = None):
        # Instance attributes -- unique to each instance
        self.hostname = hostname
        self.ip = ip
        self.port = port or self.default_port
        self._status = "stopped"   # Convention: _ means "private"

    def start(self):
        self._status = "running"
        print(f"{self.hostname} started on {self.ip}:{self.port}")

    def stop(self):
        self._status = "stopped"
        print(f"{self.hostname} stopped")

    @property
    def status(self) -> str:
        """Read-only access to server status."""
        return self._status

    def __repr__(self) -> str:
        return f"Server({self.hostname!r}, {self.ip!r}, port={self.port})"

    def __str__(self) -> str:
        return f"{self.hostname} ({self.ip}:{self.port}) [{self._status}]"

    def __eq__(self, other) -> bool:
        if not isinstance(other, Server):
            return NotImplemented
        return self.hostname == other.hostname and self.ip == other.ip

    def __hash__(self) -> int:
        return hash((self.hostname, self.ip))


web = Server("web-1", "10.0.1.10", 80)
web.start()
print(web)          # web-1 (10.0.1.10:80) [running]
print(repr(web))    # Server('web-1', '10.0.1.10', port=80)
print(web.status)   # "running" -- property, not method call
```

**Dunder methods** (double-underscore, also called magic methods) let your classes integrate with Python's built-in operations:

| Method | Triggered By | Purpose |
|--------|-------------|---------|
| `__init__` | `MyClass()` | Initialize instance |
| `__repr__` | `repr(obj)` | Unambiguous string for debugging |
| `__str__` | `str(obj)`, `print(obj)` | Human-readable string |
| `__eq__` | `obj == other` | Equality comparison |
| `__hash__` | `hash(obj)`, dict keys, sets | Hashing |
| `__len__` | `len(obj)` | Length |
| `__getitem__` | `obj[key]` | Index/key access |
| `__iter__` | `for x in obj` | Iteration |
| `__enter__`/`__exit__` | `with obj` | Context manager |
| `__lt__`, `__le__`, `__gt__`, `__ge__` | `<`, `<=`, `>`, `>=` | Ordering |

**Properties** let you add validation or computation to attribute access without changing the interface.

```python
class Temperature:
    def __init__(self, celsius: float):
        self.celsius = celsius   # Uses the setter

    @property
    def celsius(self) -> float:
        return self._celsius

    @celsius.setter
    def celsius(self, value: float):
        if value < -273.15:
            raise ValueError("Temperature below absolute zero")
        self._celsius = value

    @property
    def fahrenheit(self) -> float:
        return self._celsius * 9 / 5 + 32

temp = Temperature(100)
print(temp.fahrenheit)    # 212.0
temp.celsius = -300       # ValueError
```

**Inheritance.** A subclass inherits attributes and methods from a parent class and can override or extend them.

```python
class LoadBalancer(Server):
    """A server that distributes traffic to backends."""

    def __init__(self, hostname: str, ip: str, backends: list[Server]):
        super().__init__(hostname, ip, port=80)
        self.backends = backends

    def health_check(self) -> dict[str, str]:
        return {b.hostname: b.status for b in self.backends}

    def __repr__(self) -> str:
        return f"LoadBalancer({self.hostname!r}, backends={len(self.backends)})"
```

**Multiple inheritance and MRO.** Python supports multiple inheritance. The Method Resolution Order (MRO) determines which parent's method is called. Python uses C3 linearization.

```python
class Loggable:
    def log(self, message: str):
        print(f"[{self.__class__.__name__}] {message}")

class Serializable:
    def to_dict(self) -> dict:
        return vars(self)

class ManagedServer(Server, Loggable, Serializable):
    """Server with logging and serialization."""
    pass

ms = ManagedServer("app-1", "10.0.1.20")
ms.log("Starting up")     # From Loggable
print(ms.to_dict())       # From Serializable
print(ManagedServer.__mro__)
# (ManagedServer, Server, Loggable, Serializable, object)
```

**Abstract classes** define interfaces that subclasses must implement.

```python
from abc import ABC, abstractmethod

class HealthCheck(ABC):
    @abstractmethod
    def check(self) -> bool:
        """Return True if the service is healthy."""
        ...

    @abstractmethod
    def name(self) -> str:
        ...

# Cannot instantiate abstract class
# hc = HealthCheck()  # TypeError

class HTTPHealthCheck(HealthCheck):
    def __init__(self, url: str):
        self.url = url

    def check(self) -> bool:
        # In practice, make an HTTP request
        return True

    def name(self) -> str:
        return f"HTTP:{self.url}"
```

**Dataclasses** reduce boilerplate for classes that are primarily data containers.

```python
from dataclasses import dataclass, field

@dataclass
class Deployment:
    name: str
    image: str
    replicas: int = 1
    labels: dict[str, str] = field(default_factory=dict)

    # __init__, __repr__, __eq__ are generated automatically

d = Deployment("web", "nginx:latest", replicas=3, labels={"app": "web"})
print(d)  # Deployment(name='web', image='nginx:latest', replicas=3, labels={'app': 'web'})
```

**Protocols (structural subtyping).** Protocols define interfaces based on what an object *does*, not what it *is*. This is "duck typing" with type-checker support.

```python
from typing import Protocol

class Restartable(Protocol):
    def restart(self) -> None: ...

def restart_all(services: list[Restartable]) -> None:
    for service in services:
        service.restart()

# Any class with a restart() method satisfies the protocol
# No inheritance required
```

**Composition vs. inheritance.** Prefer composition ("has-a") over inheritance ("is-a") when the relationship is about capability, not identity.

```python
# Composition -- more flexible
class Application:
    def __init__(self, name: str, server: Server, db: Server):
        self.name = name
        self.server = server
        self.db = db

    def deploy(self):
        self.server.start()
        self.db.start()
        print(f"{self.name} deployed")
```

**SOLID Principles:**

| Principle | Meaning | Practical Implication |
|-----------|---------|----------------------|
| **S**ingle Responsibility | A class has one reason to change | Split a class that handles both HTTP requests and database queries |
| **O**pen/Closed | Open for extension, closed for modification | Use abstract base classes and dependency injection |
| **L**iskov Substitution | Subtypes must be substitutable for their base type | If `Square` extends `Rectangle`, setting width must not break height expectations |
| **I**nterface Segregation | Clients should not depend on interfaces they do not use | Split a fat interface into smaller, focused ones |
| **D**ependency Inversion | Depend on abstractions, not concretions | Accept a `HealthCheck` protocol, not a concrete `HTTPHealthCheck` |

### Practice

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass, field
from typing import Protocol


class StorageBackend(Protocol):
    """Any object with read/write methods satisfies this."""
    def read(self, key: str) -> str | None: ...
    def write(self, key: str, value: str) -> None: ...


class InMemoryStorage:
    """Simple in-memory key-value store."""
    def __init__(self):
        self._data: dict[str, str] = {}

    def read(self, key: str) -> str | None:
        return self._data.get(key)

    def write(self, key: str, value: str) -> None:
        self._data[key] = value


@dataclass
class ConfigManager:
    """Manages configuration using dependency injection."""
    storage: StorageBackend
    defaults: dict[str, str] = field(default_factory=dict)

    def get(self, key: str) -> str | None:
        value = self.storage.read(key)
        if value is None:
            return self.defaults.get(key)
        return value

    def set(self, key: str, value: str) -> None:
        self.storage.write(key, value)


# Use it
storage = InMemoryStorage()
config = ConfigManager(storage, defaults={"log_level": "INFO"})
config.set("db_host", "10.0.1.5")
print(config.get("db_host"))      # "10.0.1.5"
print(config.get("log_level"))    # "INFO" -- from defaults
print(config.get("missing"))      # None
```

### Connection

OOP is not just a language feature -- it is a design paradigm that shapes how you build systems. Ansible modules, Terraform providers, cloud SDK clients, and web frameworks are all built with classes and inheritance. Understanding SOLID principles tells you *why* good libraries are designed the way they are. Understanding protocols and composition tells you how to write code that is testable and maintainable. In [Software Engineering and Collaboration](/learn/first-principles/software-engineering-and-collaboration/), you will apply these principles to real project architectures.

> **Try It**: Create a `Container` class with attributes `name`, `image`, and `status`. Implement `__repr__`, `__eq__`, and `__hash__`. Add a `start()` and `stop()` method. Then create a `Pod` class that contains a list of `Container` objects and implements `__len__` and `__iter__`. Use a `for` loop to iterate over all containers in a pod.

---

## F. Functional Programming

### Theory

Functional programming (FP) treats computation as the evaluation of mathematical functions. Its core ideas are:

**Pure functions** always return the same output for the same input and have no side effects. They do not modify global state, mutate arguments, write to files, or make network calls.

```python
# Pure -- depends only on inputs, no side effects
def calculate_tax(amount: float, rate: float) -> float:
    return amount * rate

# Impure -- modifies external state
total = 0
def add_to_total(amount: float) -> float:
    global total
    total += amount    # Side effect: modifies global state
    return total
```

Pure functions are easier to test, reason about, and parallelize. When you know a function has no side effects, you can call it with confidence that it will not break anything else.

**Immutability** means data does not change after creation. Instead of modifying data in place, you create new data.

```python
# Mutable approach -- modifies the list
def add_server_mutable(servers: list, server: str) -> list:
    servers.append(server)    # Side effect: mutates input
    return servers

# Immutable approach -- returns a new list
def add_server_immutable(servers: tuple, server: str) -> tuple:
    return servers + (server,)  # New tuple, original unchanged

# Frozen sets and tuples are immutable equivalents
config = frozenset({"feature_a", "feature_b"})
# config.add("feature_c")  # AttributeError
```

**Higher-order functions** take functions as arguments or return functions as results. You have already used these: decorators, `sorted(key=...)`, `map()`, `filter()`.

```python
# map -- apply function to every element
ports = ["80", "443", "8080"]
int_ports = list(map(int, ports))          # [80, 443, 8080]

# filter -- keep elements that satisfy a predicate
high_ports = list(filter(lambda p: p > 1024, int_ports))  # [8080]

# reduce -- accumulate a sequence into a single value
from functools import reduce
total = reduce(lambda acc, x: acc + x, [1, 2, 3, 4, 5])  # 15

# In practice, prefer comprehensions and sum() for simple cases
int_ports = [int(p) for p in ports]             # More readable than map
high_ports = [p for p in int_ports if p > 1024]  # More readable than filter
total = sum([1, 2, 3, 4, 5])                     # More readable than reduce
```

**When FP patterns help:**

- Data transformation pipelines (parse -> validate -> transform -> output)
- Concurrent code (pure functions can safely run in parallel)
- Configuration generation (declarative, no side effects)
- Testing (pure functions need no mocking)

**When FP patterns hurt:**

- Deeply nested `map(filter(map(...)))` chains become unreadable
- Forced immutability in Python is awkward because the language was not designed for it
- Side effects are unavoidable for I/O, logging, and database access
- Lambda chains obscure logic that a simple `for` loop would make clear

### Practice

```python
from functools import reduce
from typing import Callable

# Data pipeline using functional composition
def pipeline(*functions: Callable) -> Callable:
    """Compose functions left to right."""
    def apply(data):
        return reduce(lambda result, func: func(result), functions, data)
    return apply

# Individual transform functions (pure)
def parse_line(line: str) -> dict:
    parts = line.split(",")
    return {"name": parts[0], "cpu": float(parts[1]), "memory": int(parts[2])}

def add_status(server: dict) -> dict:
    status = "overloaded" if server["cpu"] > 80 else "healthy"
    return {**server, "status": status}   # New dict, original unchanged

def format_report(server: dict) -> str:
    return f"{server['name']}: {server['status']} (CPU: {server['cpu']}%)"

# Compose the pipeline
process = pipeline(parse_line, add_status, format_report)

raw_data = [
    "web-1,45.2,2048",
    "web-2,92.1,4096",
    "db-1,78.5,8192",
]

for line in raw_data:
    print(process(line))
# web-1: healthy (CPU: 45.2%)
# web-2: overloaded (CPU: 92.1%)
# db-1: healthy (CPU: 78.5%)
```

### Connection

You will not write purely functional Python in infrastructure work. You will use functional *patterns* where they improve clarity: comprehensions instead of loops for transformations, pure functions for testable business logic, `map()` for batch operations. The key insight is knowing when a functional approach makes code clearer and when it makes code obscure. A list comprehension is almost always better than `map()` with a lambda. A `for` loop with `append` is almost always better than a deeply nested functional chain.

> **Try It**: Write a pure function `classify_ports(ports: list[int]) -> dict[str, list[int]]` that takes a list of port numbers and returns a dict with keys `"well_known"` (0-1023), `"registered"` (1024-49151), and `"dynamic"` (49152-65535). Do not mutate the input list. Then use `map()` and `filter()` to convert a list of string port numbers to integers and keep only ports above 1024.

---

## G. Paradigm Awareness

### Theory

Programming paradigms are not mutually exclusive. Most real programs use multiple paradigms. Understanding them helps you recognize which approach fits which problem.

| Paradigm | Core Idea | Python Support | Best For |
|----------|-----------|---------------|----------|
| **Imperative/Procedural** | Step-by-step instructions | Full | Scripts, automation |
| **Object-Oriented** | Data + behavior in objects | Full | Complex domain models, frameworks |
| **Functional** | Pure functions, immutable data | Partial | Data transformations, pipelines |
| **Declarative** | Describe *what*, not *how* | Via libraries | Configuration, queries (SQL, Terraform) |
| **Event-driven** | Respond to events/callbacks | Via asyncio | Network servers, GUIs |
| **Concurrent** | Multiple tasks simultaneously | threading, multiprocessing, asyncio | I/O-bound and CPU-bound parallelism |

**Concurrent programming preview.** Python provides three concurrency models:

```python
# Threading -- concurrent I/O, limited by the GIL for CPU work
import threading

def fetch_url(url: str) -> None:
    print(f"Fetching {url}")
    # In practice: requests.get(url)

threads = [threading.Thread(target=fetch_url, args=(url,))
           for url in ["http://a.com", "http://b.com", "http://c.com"]]
for t in threads:
    t.start()
for t in threads:
    t.join()

# Multiprocessing -- true parallelism, separate memory spaces
from multiprocessing import Pool

def cpu_heavy(n: int) -> int:
    return sum(i * i for i in range(n))

with Pool(4) as pool:
    results = pool.map(cpu_heavy, [10**6, 10**6, 10**6, 10**6])

# async/await -- cooperative multitasking for I/O
import asyncio

async def fetch_async(url: str) -> str:
    print(f"Fetching {url}")
    await asyncio.sleep(1)   # Simulates I/O wait
    return f"Response from {url}"

async def main():
    results = await asyncio.gather(
        fetch_async("http://a.com"),
        fetch_async("http://b.com"),
        fetch_async("http://c.com"),
    )
    print(results)

# asyncio.run(main())
```

| Model | Best For | GIL Impact | Memory |
|-------|----------|------------|--------|
| `threading` | I/O-bound (network, disk) | Limited by GIL for CPU | Shared |
| `multiprocessing` | CPU-bound (computation) | Bypasses GIL | Separate per process |
| `asyncio` | High-concurrency I/O | Single thread, no GIL issue | Shared |

### Practice

```python
# Same problem, three paradigms

# Imperative: step-by-step
def count_errors_imperative(lines: list[str]) -> int:
    count = 0
    for line in lines:
        if "ERROR" in line:
            count += 1
    return count

# Functional: transformation
def count_errors_functional(lines: list[str]) -> int:
    return sum(1 for line in lines if "ERROR" in line)

# OOP: encapsulated
class LogAnalyzer:
    def __init__(self, lines: list[str]):
        self.lines = lines

    def count_errors(self) -> int:
        return sum(1 for line in self.lines if "ERROR" in line)

    def error_lines(self) -> list[str]:
        return [line for line in self.lines if "ERROR" in line]

# All three produce the same result. Choose based on context:
# - Quick script? Imperative or functional.
# - Part of a larger system? OOP.
# - Data pipeline? Functional.
```

### Connection

No paradigm is universally best. Terraform is declarative. Ansible playbooks are declarative with imperative escape hatches. Python scripts are typically imperative with functional and OOP elements. Kubernetes controllers are event-driven. Understanding multiple paradigms lets you read and write code in any of these systems. The concurrent programming models preview here -- threading, multiprocessing, and asyncio -- will be essential in [Networking](/learn/first-principles/networking/) and [Infrastructure at Scale](/learn/first-principles/infrastructure-at-scale/) where you handle multiple connections and parallel operations.

> **Try It**: Write the same function three ways -- imperative, functional, and OOP -- that takes a list of server hostnames and returns only those containing "prod". Discuss with yourself (or a peer) which version you find most readable and why.

---

## H. Error Handling

### Theory

Errors are not exceptional in infrastructure code -- they are expected. Networks fail, disks fill up, APIs return unexpected responses, configuration files contain typos. Robust error handling is the difference between a script that crashes at 3 AM and one that logs the problem and recovers.

**try/except/else/finally:**

```python
import json

def load_config(path: str) -> dict:
    """Load JSON config with comprehensive error handling."""
    try:
        with open(path, encoding="utf-8") as f:
            config = json.load(f)
    except FileNotFoundError:
        print(f"Config file not found: {path}")
        return {}
    except json.JSONDecodeError as e:
        print(f"Invalid JSON in {path}: {e}")
        return {}
    except PermissionError:
        print(f"Permission denied: {path}")
        return {}
    else:
        # Runs only if no exception was raised
        print(f"Loaded config from {path}")
        return config
    finally:
        # Always runs -- cleanup code goes here
        print("Config loading attempt complete")
```

**Exception hierarchy.** All exceptions inherit from `BaseException`. You almost always catch `Exception` or its subclasses, never `BaseException` (which includes `KeyboardInterrupt` and `SystemExit`).

```
BaseException
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception
    ├── StopIteration
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   └── OverflowError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── OSError
    │   ├── FileNotFoundError
    │   ├── PermissionError
    │   └── ConnectionError
    ├── ValueError
    ├── TypeError
    └── RuntimeError
```

**Custom exceptions.** Define custom exceptions for domain-specific errors. This makes error handling precise and self-documenting.

```python
class InfrastructureError(Exception):
    """Base exception for infrastructure operations."""
    pass

class DeploymentError(InfrastructureError):
    """Raised when a deployment fails."""
    def __init__(self, service: str, reason: str):
        self.service = service
        self.reason = reason
        super().__init__(f"Failed to deploy {service}: {reason}")

class HealthCheckError(InfrastructureError):
    """Raised when a health check fails."""
    def __init__(self, endpoint: str, status_code: int):
        self.endpoint = endpoint
        self.status_code = status_code
        super().__init__(f"Health check failed for {endpoint}: HTTP {status_code}")

# Usage
try:
    raise DeploymentError("web-api", "image pull failed")
except DeploymentError as e:
    print(f"Service: {e.service}, Reason: {e.reason}")
except InfrastructureError:
    print("Generic infrastructure error")
```

**EAFP vs. LBYL.** Python strongly favors "Easier to Ask Forgiveness than Permission" (EAFP) over "Look Before You Leap" (LBYL).

```python
# LBYL -- check before acting (non-Pythonic)
if "key" in config:
    value = config["key"]
else:
    value = "default"

# EAFP -- try and handle failure (Pythonic)
try:
    value = config["key"]
except KeyError:
    value = "default"

# Even better for dicts
value = config.get("key", "default")
```

EAFP is preferred because it avoids race conditions (the state can change between the check and the action) and is often faster when the common case succeeds.

**Context managers.** The `with` statement ensures resources are properly cleaned up, even if exceptions occur. A context manager implements `__enter__` (called on entry) and `__exit__` (called on exit, even after exceptions).

```python
# Built-in context managers
with open("data.txt", encoding="utf-8") as f:
    content = f.read()
# File is automatically closed here, even if an exception occurred

# Custom context manager using a class
class Timer:
    def __enter__(self):
        self.start = __import__("time").perf_counter()
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.elapsed = __import__("time").perf_counter() - self.start
        print(f"Elapsed: {self.elapsed:.4f}s")
        return False  # Do not suppress exceptions

with Timer():
    sum(range(1_000_000))

# Context manager using contextlib (simpler)
from contextlib import contextmanager

@contextmanager
def managed_connection(host: str, port: int):
    """Simulate a managed database connection."""
    print(f"Connecting to {host}:{port}")
    conn = {"host": host, "port": port, "status": "open"}
    try:
        yield conn
    except Exception as e:
        print(f"Error during connection: {e}")
        raise
    finally:
        conn["status"] = "closed"
        print(f"Connection to {host}:{port} closed")

with managed_connection("db.example.com", 5432) as conn:
    print(f"Using connection: {conn}")
```

**Exception chaining.** Use `raise ... from ...` to chain exceptions, preserving the original cause.

```python
def connect_to_database(host: str) -> dict:
    try:
        # Simulate a low-level connection error
        raise ConnectionError(f"Cannot reach {host}")
    except ConnectionError as e:
        raise DeploymentError("database", "connection failed") from e
        # The original ConnectionError is preserved as __cause__
```

### Practice

```python
from contextlib import contextmanager
import json
import os

# Build a robust configuration loader with custom exceptions
class ConfigError(Exception):
    """Base config exception."""
    pass

class ConfigNotFoundError(ConfigError):
    pass

class ConfigParseError(ConfigError):
    pass

class ConfigValidationError(ConfigError):
    pass

@contextmanager
def atomic_write(path: str):
    """Write to a temp file, then atomically rename.

    Ensures the file is never left in a partial state.
    """
    tmp_path = path + ".tmp"
    try:
        with open(tmp_path, "w", encoding="utf-8") as f:
            yield f
        os.rename(tmp_path, path)  # Atomic on POSIX
    except Exception:
        if os.path.exists(tmp_path):
            os.remove(tmp_path)
        raise

def load_and_validate_config(path: str, required_keys: set[str]) -> dict:
    """Load, parse, and validate a JSON config file."""
    try:
        with open(path, encoding="utf-8") as f:
            config = json.load(f)
    except FileNotFoundError:
        raise ConfigNotFoundError(f"Config not found: {path}")
    except json.JSONDecodeError as e:
        raise ConfigParseError(f"Invalid JSON: {e}") from e

    missing = required_keys - set(config.keys())
    if missing:
        raise ConfigValidationError(f"Missing required keys: {missing}")

    return config

# Usage
try:
    config = load_and_validate_config(
        "app.json",
        required_keys={"host", "port", "database"},
    )
except ConfigNotFoundError:
    print("Using default configuration")
    config = {"host": "localhost", "port": 8080, "database": "app_db"}
except ConfigParseError as e:
    print(f"Fix the config file: {e}")
    raise SystemExit(1)
except ConfigValidationError as e:
    print(f"Config incomplete: {e}")
    raise SystemExit(1)
```

### Connection

Every infrastructure tool you write will need error handling. Cloud API calls fail. Configuration files are missing or malformed. Services are unreachable. Context managers ensure connections are closed, locks are released, and temporary files are cleaned up. Custom exceptions make error handling precise -- catching `DeploymentError` instead of bare `Exception` means you handle only what you expect and let unexpected errors propagate. In [APIs and Integration](/learn/first-principles/apis-and-integration/), you will build clients with retry logic, exponential backoff, and circuit breakers -- all built on these error handling fundamentals.

> **Try It**: Write a context manager `@contextmanager` called `suppress_and_log(*exceptions)` that catches specified exception types, logs them, and continues execution. Use it to wrap a block that raises a `ValueError`. Verify that the error is logged but execution continues.

---

## I. File I/O and Data Formats

### Theory

Infrastructure code constantly reads and writes files: configuration files, log files, state files, deployment manifests. Python provides clean abstractions for file operations and parsers for common data formats.

**pathlib** is the modern way to work with file paths. Use it instead of `os.path`.

```python
from pathlib import Path

# Path construction
config_dir = Path("/etc/myapp")
config_file = config_dir / "config.json"    # / operator joins paths
print(config_file)          # /etc/myapp/config.json
print(config_file.name)     # config.json
print(config_file.stem)     # config
print(config_file.suffix)   # .json
print(config_file.parent)   # /etc/myapp

# File operations
if config_file.exists():
    content = config_file.read_text(encoding="utf-8")
    data = config_file.read_bytes()

# Write
output = Path("output.txt")
output.write_text("Hello\n", encoding="utf-8")

# Glob -- find files matching a pattern
for py_file in Path(".").glob("**/*.py"):
    print(py_file)

# Directory operations
Path("logs/2024/01").mkdir(parents=True, exist_ok=True)
```

**Always specify `encoding='utf-8'`.** Python's default encoding depends on the platform. On Windows, it is often a legacy encoding like `cp1252`. Explicit UTF-8 prevents encoding bugs.

**JSON** is the dominant data format for APIs, configuration, and state files.

```python
import json

# Parse JSON string
data = json.loads('{"host": "localhost", "port": 8080}')
print(data["host"])   # "localhost"

# Generate JSON string
output = json.dumps(data, indent=2)
print(output)

# Read/write JSON files
with open("config.json", encoding="utf-8") as f:
    config = json.load(f)

with open("output.json", "w", encoding="utf-8") as f:
    json.dump(config, f, indent=2)
```

**YAML** is widely used in infrastructure: Kubernetes manifests, Ansible playbooks, Docker Compose files, CI/CD configs.

```python
# pip install pyyaml
import yaml

# Parse YAML string
yaml_text = """
apiVersion: v1
kind: Service
metadata:
  name: web
  labels:
    app: web
spec:
  ports:
    - port: 80
      targetPort: 8080
  selector:
    app: web
"""

data = yaml.safe_load(yaml_text)   # ALWAYS use safe_load, never load()
print(data["metadata"]["name"])     # "web"

# Write YAML
with open("service.yaml", "w", encoding="utf-8") as f:
    yaml.dump(data, f, default_flow_style=False)

# safe_load vs load:
# yaml.load() can execute arbitrary Python code -- NEVER use it on untrusted input
# yaml.safe_load() only parses basic YAML types -- ALWAYS use this
```

**CSV** for tabular data.

```python
import csv
from pathlib import Path

# Read CSV
with open("servers.csv", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(f"{row['hostname']}: {row['ip']}")

# Write CSV
servers = [
    {"hostname": "web-1", "ip": "10.0.1.10", "port": "80"},
    {"hostname": "db-1", "ip": "10.0.1.20", "port": "5432"},
]

with open("output.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["hostname", "ip", "port"])
    writer.writeheader()
    writer.writerows(servers)
```

**TOML** is used for Python project configuration (`pyproject.toml`).

```python
# Python 3.11+ includes tomllib in the standard library
import tomllib

with open("pyproject.toml", "rb") as f:   # Note: "rb" -- binary mode
    config = tomllib.load(f)

print(config["project"]["name"])
print(config["project"]["version"])
```

### Practice

```python
from pathlib import Path
import json
import csv

# Build a configuration management utility
class ConfigStore:
    """Read and write configs in multiple formats."""

    def __init__(self, base_dir: str | Path):
        self.base_dir = Path(base_dir)
        self.base_dir.mkdir(parents=True, exist_ok=True)

    def save_json(self, name: str, data: dict) -> Path:
        path = self.base_dir / f"{name}.json"
        path.write_text(
            json.dumps(data, indent=2) + "\n",
            encoding="utf-8",
        )
        return path

    def load_json(self, name: str) -> dict:
        path = self.base_dir / f"{name}.json"
        return json.loads(path.read_text(encoding="utf-8"))

    def save_csv(self, name: str, rows: list[dict]) -> Path:
        if not rows:
            return self.base_dir / f"{name}.csv"
        path = self.base_dir / f"{name}.csv"
        with open(path, "w", newline="", encoding="utf-8") as f:
            writer = csv.DictWriter(f, fieldnames=rows[0].keys())
            writer.writeheader()
            writer.writerows(rows)
        return path

    def list_configs(self, pattern: str = "*.json") -> list[Path]:
        return sorted(self.base_dir.glob(pattern))


# Usage
store = ConfigStore("/tmp/configs")
store.save_json("database", {
    "host": "10.0.1.5",
    "port": 5432,
    "name": "production",
})

store.save_csv("inventory", [
    {"hostname": "web-1", "role": "frontend", "az": "us-east-1a"},
    {"hostname": "db-1", "role": "database", "az": "us-east-1b"},
])

print(store.list_configs("*"))
```

### Connection

Every tool in the infrastructure ecosystem uses these formats. Kubernetes manifests are YAML. Terraform state is JSON. AWS CLI output is JSON. Ansible inventory can be CSV, YAML, or INI. CI/CD configs (GitHub Actions, GitLab CI) are YAML. Python project metadata is TOML. Knowing how to read and write each format programmatically means you can build automation that ties these tools together -- generating Kubernetes manifests from a database, converting Terraform state to a CSV inventory, or parsing CI/CD output for monitoring.

> **Try It**: Create a Python script that reads a JSON file containing a list of servers (`[{"name": "web-1", "ip": "10.0.1.10"}, ...]`), adds a `status` field set to `"unknown"` to each server, and writes the result to both a new JSON file and a CSV file. Use `pathlib` for all file paths.

---

## J. Virtual Environments and Packages

### Theory

Python projects depend on third-party packages. Without isolation, installing a package for one project can break another. Virtual environments solve this by creating an isolated Python environment per project with its own packages.

**Why isolation matters:**

- Project A needs `requests==2.28.0`, Project B needs `requests==2.31.0`
- Installing a package globally may conflict with system Python packages
- Reproducibility requires knowing exact dependency versions
- Deployment environments should match development environments exactly

**venv** is the standard library tool for creating virtual environments.

```bash
# Create a virtual environment
python3 -m venv .venv

# Activate it
source .venv/bin/activate      # Linux/macOS
# .venv\Scripts\activate       # Windows

# Your prompt changes to show the active environment
(.venv) $ which python
/path/to/project/.venv/bin/python

# Install packages (isolated to this environment)
pip install requests flask

# Deactivate when done
deactivate
```

**pip** is the standard package installer.

```bash
# Install a specific version
pip install requests==2.31.0

# Install from requirements file
pip install -r requirements.txt

# Freeze current environment to a file
pip freeze > requirements.txt

# Show installed packages
pip list

# Show package info
pip show requests
```

**requirements.txt** lists exact dependency versions for reproducibility.

```
# requirements.txt
requests==2.31.0
flask==3.0.0
pyyaml==6.0.1
pytest==8.1.1
```

Always pin versions in production. `pip freeze` generates pinned versions from your current environment.

**uv** is a modern, fast Python package manager written in Rust. It is significantly faster than pip and provides additional features.

```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create a virtual environment
uv venv

# Install packages (10-100x faster than pip)
uv pip install requests flask

# Sync from requirements file
uv pip sync requirements.txt

# Compile a lock file from loose requirements
uv pip compile requirements.in -o requirements.txt
```

**pyproject.toml** is the modern standard for Python project configuration, replacing `setup.py` and `setup.cfg`.

```toml
[project]
name = "my-infra-tool"
version = "0.1.0"
description = "Infrastructure automation utilities"
requires-python = ">=3.11"
dependencies = [
    "requests>=2.31.0",
    "pyyaml>=6.0",
    "click>=8.1.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.0",
    "ruff>=0.3.0",
    "mypy>=1.9.0",
]

[project.scripts]
infra-tool = "my_infra_tool.cli:main"

[tool.ruff]
line-length = 88
target-version = "py311"

[tool.pytest.ini_options]
testpaths = ["tests"]
```

**Package manager comparison:**

| Feature | pip | uv | pipenv | poetry |
|---------|-----|-----|--------|--------|
| Speed | Baseline | 10-100x faster | Slower | Slower |
| Lock file | No (manual freeze) | Yes (`uv.lock`) | Yes | Yes |
| Dependency resolution | Basic | Advanced | Advanced | Advanced |
| Built-in venv | No (use `venv`) | Yes | Yes | Yes |
| Standard | Yes | Emerging | Community | Community |
| pyproject.toml | Partial | Full | No | Yes |

### Practice

```bash
# Complete workflow for a new project

# 1. Create project directory
mkdir my-project && cd my-project

# 2. Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# 3. Install development dependencies
pip install pytest ruff pyyaml requests

# 4. Freeze exact versions
pip freeze > requirements.txt

# 5. Create .gitignore
cat > .gitignore << 'EOF'
.venv/
__pycache__/
*.pyc
.pytest_cache/
*.egg-info/
dist/
build/
.mypy_cache/
.ruff_cache/
EOF

# 6. Verify isolation
which python     # Points to .venv/bin/python
pip list         # Shows only your project's packages

# 7. Deactivate
deactivate

# --- Alternative workflow with uv ---
uv venv
source .venv/bin/activate
uv pip install pytest ruff pyyaml requests
uv pip freeze > requirements.txt
```

### Connection

Virtual environments are not optional -- they are required for professional Python development. Every CI/CD pipeline creates a fresh virtual environment, installs dependencies from `requirements.txt` or `pyproject.toml`, runs tests, and discards the environment. In [Software Engineering and Collaboration](/learn/first-principles/software-engineering-and-collaboration/), you will automate this process with GitHub Actions. In [Infrastructure at Scale](/learn/first-principles/infrastructure-at-scale/), you will see Docker images that install Python dependencies in isolated layers. Understanding dependency management now prevents "works on my machine" problems later.

> **Try It**: Create a new directory, set up a virtual environment with `python3 -m venv .venv`, activate it, install `requests` and `pyyaml`, freeze the requirements, deactivate, delete the `.venv` directory, recreate it, and reinstall from your `requirements.txt`. Verify that both packages are present and at the exact same versions.

---

## K. Testing

### Theory

Tests are not optional. They are the mechanism that allows you to change code without fear. Without tests, every change is a gamble. With tests, you know immediately when something breaks.

**pytest** is the dominant testing framework in Python. It discovers tests automatically, provides rich assertion introspection, and has a powerful plugin ecosystem.

```python
# test_math.py
def add(a: int, b: int) -> int:
    return a + b

def test_add_positive():
    assert add(2, 3) == 5

def test_add_negative():
    assert add(-1, -2) == -3

def test_add_zero():
    assert add(0, 0) == 0
```

```bash
# Run tests
pytest                     # Discover and run all tests
pytest test_math.py        # Run specific file
pytest -v                  # Verbose output
pytest -x                  # Stop on first failure
pytest -k "negative"       # Run tests matching pattern
```

pytest discovers tests by finding files named `test_*.py` or `*_test.py`, then finding functions and methods named `test_*`.

**Fixtures** provide reusable setup and teardown logic.

```python
# conftest.py -- shared fixtures, auto-discovered by pytest
import pytest
import json
from pathlib import Path

@pytest.fixture
def sample_config():
    """Provide a sample configuration dict."""
    return {
        "host": "localhost",
        "port": 8080,
        "debug": True,
    }

@pytest.fixture
def config_file(tmp_path, sample_config):
    """Write config to a temporary file."""
    path = tmp_path / "config.json"
    path.write_text(json.dumps(sample_config), encoding="utf-8")
    return path

@pytest.fixture(scope="session")
def database_url():
    """Session-scoped fixture -- created once per test session."""
    return "sqlite:///test.db"
```

```python
# test_config.py
import json

def test_load_config(config_file):
    """config_file fixture provides a temp file with JSON content."""
    content = config_file.read_text(encoding="utf-8")
    config = json.loads(content)
    assert config["host"] == "localhost"
    assert config["port"] == 8080

def test_config_has_debug(sample_config):
    assert "debug" in sample_config
    assert sample_config["debug"] is True
```

Fixture scopes control lifetime:

| Scope | Lifetime | Use For |
|-------|----------|---------|
| `function` (default) | One per test function | Most fixtures |
| `class` | One per test class | Shared class state |
| `module` | One per test module | Expensive module setup |
| `session` | One per test run | Database connections, server startup |

**Parametrize** runs the same test with different inputs.

```python
import pytest

def is_valid_port(port: int) -> bool:
    return 0 <= port <= 65535

@pytest.mark.parametrize("port,expected", [
    (80, True),
    (443, True),
    (0, True),
    (65535, True),
    (-1, False),
    (65536, False),
    (99999, False),
])
def test_is_valid_port(port, expected):
    assert is_valid_port(port) == expected
```

**Mocking** replaces real objects with fake ones during testing. Use it to isolate the code under test from external dependencies (APIs, databases, filesystems).

```python
from unittest.mock import patch, MagicMock
import pytest

# Function that calls an external API
def get_server_status(hostname: str) -> str:
    import requests
    response = requests.get(f"https://api.example.com/status/{hostname}")
    response.raise_for_status()
    return response.json()["status"]

# Test with mocking -- no real HTTP request
@patch("requests.get")
def test_get_server_status(mock_get):
    # Configure the mock
    mock_response = MagicMock()
    mock_response.json.return_value = {"status": "healthy"}
    mock_response.raise_for_status.return_value = None
    mock_get.return_value = mock_response

    # Call the function -- it uses the mock instead of real requests
    result = get_server_status("web-1")

    assert result == "healthy"
    mock_get.assert_called_once_with("https://api.example.com/status/web-1")

# Testing exceptions
@patch("requests.get")
def test_get_server_status_error(mock_get):
    import requests
    mock_get.side_effect = requests.ConnectionError("Connection refused")

    with pytest.raises(requests.ConnectionError):
        get_server_status("web-1")
```

**Coverage** measures which lines of code are executed during tests.

```bash
# Install
pip install pytest-cov

# Run with coverage
pytest --cov=my_package --cov-report=term-missing

# Output shows which lines are NOT covered
# Name                    Stmts   Miss  Cover   Missing
# my_package/config.py       45      3    93%   22-24
# my_package/deploy.py       67     12    82%   34-45
```

**Test organization:**

```
my_project/
├── src/
│   └── my_package/
│       ├── __init__.py
│       ├── config.py
│       └── deploy.py
├── tests/
│   ├── conftest.py          # Shared fixtures
│   ├── test_config.py
│   └── test_deploy.py
├── pyproject.toml
└── requirements.txt
```

### Practice

```python
# test_infrastructure.py
import pytest
from unittest.mock import patch, MagicMock
from dataclasses import dataclass

# Code under test
@dataclass
class Server:
    hostname: str
    ip: str
    port: int = 22

    def health_check_url(self) -> str:
        return f"http://{self.ip}:{self.port}/health"

def deploy_servers(servers: list[Server], dry_run: bool = False) -> dict:
    results = {}
    for server in servers:
        if dry_run:
            results[server.hostname] = "would deploy"
        else:
            results[server.hostname] = "deployed"
    return results

# Tests
@pytest.fixture
def web_servers():
    return [
        Server("web-1", "10.0.1.10", 80),
        Server("web-2", "10.0.1.11", 80),
    ]

def test_server_health_check_url():
    s = Server("web-1", "10.0.1.10", 80)
    assert s.health_check_url() == "http://10.0.1.10:80/health"

def test_server_default_port():
    s = Server("bastion", "10.0.0.1")
    assert s.port == 22

def test_deploy_servers(web_servers):
    results = deploy_servers(web_servers)
    assert results == {"web-1": "deployed", "web-2": "deployed"}

def test_deploy_servers_dry_run(web_servers):
    results = deploy_servers(web_servers, dry_run=True)
    assert all(v == "would deploy" for v in results.values())

@pytest.mark.parametrize("hostname,ip,port,expected_url", [
    ("web", "10.0.1.1", 80, "http://10.0.1.1:80/health"),
    ("db", "10.0.2.1", 5432, "http://10.0.2.1:5432/health"),
    ("ssh", "10.0.3.1", 22, "http://10.0.3.1:22/health"),
])
def test_health_check_url_parametrized(hostname, ip, port, expected_url):
    s = Server(hostname, ip, port)
    assert s.health_check_url() == expected_url

def test_deploy_empty_list():
    results = deploy_servers([])
    assert results == {}
```

### Connection

Testing is not a separate activity from development -- it is part of development. In [Software Engineering and Collaboration](/learn/first-principles/software-engineering-and-collaboration/), you will set up CI/CD pipelines that run your test suite on every commit. A PR cannot merge if tests fail. Coverage reports show which code paths are untested. Mocking lets you test code that calls cloud APIs without making real API calls (which cost money and are slow). Every professional Python project has tests. The sooner you internalize this, the sooner your code becomes trustworthy.

> **Try It**: Write a function `parse_port(value: str) -> int` that raises `ValueError` for non-numeric strings and values outside 0-65535. Write pytest tests for valid ports, invalid strings, and out-of-range numbers. Use `@pytest.mark.parametrize` to cover edge cases. Run with `pytest -v`.

---

## L. Standard Library

### Theory

Python's standard library is massive. You do not need to memorize it, but you need to know what is available so you reach for the right tool. These modules appear most frequently in infrastructure code.

**pathlib** -- modern path handling (covered in section I).

**subprocess** -- run external commands from Python.

```python
import subprocess

# Basic command execution
result = subprocess.run(
    ["ls", "-la", "/tmp"],
    capture_output=True,
    text=True,
    check=True,       # Raise CalledProcessError on non-zero exit
)
print(result.stdout)
print(result.returncode)  # 0

# Capture errors
try:
    subprocess.run(
        ["ls", "/nonexistent"],
        capture_output=True,
        text=True,
        check=True,
    )
except subprocess.CalledProcessError as e:
    print(f"Command failed: {e.stderr}")

# DANGER: shell=True -- allows shell injection
# NEVER use shell=True with user-provided input
subprocess.run("echo hello && echo world", shell=True)

# Safe alternative for pipes
ps = subprocess.run(["ps", "aux"], capture_output=True, text=True)
grep = subprocess.run(
    ["grep", "python"],
    input=ps.stdout,
    capture_output=True,
    text=True,
)
print(grep.stdout)
```

**argparse** -- build command-line interfaces.

```python
import argparse

def main():
    parser = argparse.ArgumentParser(description="Server management tool")

    # Subcommands
    subparsers = parser.add_subparsers(dest="command", required=True)

    # 'deploy' subcommand
    deploy_parser = subparsers.add_parser("deploy", help="Deploy a service")
    deploy_parser.add_argument("service", help="Service name")
    deploy_parser.add_argument("--replicas", type=int, default=1)
    deploy_parser.add_argument("--dry-run", action="store_true")

    # 'status' subcommand
    status_parser = subparsers.add_parser("status", help="Check service status")
    status_parser.add_argument("service", help="Service name")
    status_parser.add_argument("--verbose", "-v", action="store_true")

    args = parser.parse_args()

    if args.command == "deploy":
        print(f"Deploying {args.service} with {args.replicas} replicas")
        if args.dry_run:
            print("(dry run)")
    elif args.command == "status":
        print(f"Status of {args.service}")

if __name__ == "__main__":
    main()
```

```bash
# Usage
python tool.py deploy web-api --replicas 3 --dry-run
python tool.py status web-api -v
python tool.py --help
python tool.py deploy --help
```

**logging** -- structured logging with levels and handlers.

```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(name)s %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)

logger = logging.getLogger(__name__)

# Log levels (from least to most severe)
logger.debug("Detailed debugging info")     # Not shown at INFO level
logger.info("Normal operation")
logger.warning("Something unexpected")
logger.error("Operation failed")
logger.critical("System is down")

# Logging to a file
file_handler = logging.FileHandler("app.log")
file_handler.setLevel(logging.WARNING)
file_handler.setFormatter(logging.Formatter(
    "%(asctime)s %(levelname)s %(message)s"
))
logger.addHandler(file_handler)
```

**re** -- regular expressions (you learned the theory in [Text Processing and Automation](/learn/first-principles/text-processing-and-automation/), now use them in Python).

```python
import re

# Match -- anchored at the start
match = re.match(r"(\d{1,3}\.){3}\d{1,3}", "10.0.1.5")
if match:
    print(match.group())  # "10.0.1.5"

# Search -- find first occurrence anywhere
log = "Connection from 192.168.1.100 to port 443"
ip_match = re.search(r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}", log)
if ip_match:
    print(ip_match.group())  # "192.168.1.100"

# findall -- all occurrences
ips = re.findall(r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}", log)
print(ips)  # ["192.168.1.100"]

# sub -- replace
sanitized = re.sub(r"\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}", "[REDACTED]", log)

# Compile for reuse
IP_PATTERN = re.compile(r"(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})")
match = IP_PATTERN.match("192.168.1.100")
if match:
    octets = match.groups()  # ('192', '168', '1', '100')
```

**datetime** -- timezone-aware date and time handling.

```python
from datetime import datetime, timezone, timedelta

# Always use timezone-aware datetimes
now = datetime.now(timezone.utc)
print(now.isoformat())  # "2024-01-15T10:30:00+00:00"

# Parse ISO format
parsed = datetime.fromisoformat("2024-01-15T10:30:00+00:00")

# Time arithmetic
one_hour_later = now + timedelta(hours=1)
one_week_ago = now - timedelta(weeks=1)

# Format for display
print(now.strftime("%Y-%m-%d %H:%M:%S %Z"))

# NEVER use naive datetimes (without timezone) in production code
# naive = datetime.now()  # No timezone info -- ambiguous
```

**collections** -- specialized container types.

```python
from collections import OrderedDict, ChainMap, defaultdict, Counter, deque

# ChainMap -- search multiple dicts as one
defaults = {"color": "blue", "size": "medium"}
user_prefs = {"color": "red"}
env_vars = {"size": "large", "debug": "true"}

config = ChainMap(env_vars, user_prefs, defaults)
print(config["color"])   # "red" -- found in user_prefs
print(config["size"])    # "large" -- found in env_vars
print(config["debug"])   # "true" -- found in env_vars
```

**functools** -- higher-order function utilities.

```python
from functools import lru_cache, partial, wraps

# lru_cache -- memoize expensive function calls
@lru_cache(maxsize=128)
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(100))   # Instant -- cached
print(fibonacci.cache_info())

# partial -- create new function with some arguments pre-filled
import subprocess

git = partial(subprocess.run, capture_output=True, text=True, check=True)
result = git(["git", "status"])
print(result.stdout)
```

**shutil** -- high-level file operations.

```python
import shutil

# Copy files and directories
shutil.copy2("src.txt", "dst.txt")         # Copy with metadata
shutil.copytree("src_dir", "dst_dir")      # Copy entire directory tree
shutil.rmtree("dir_to_delete")             # Remove directory tree
shutil.move("old_path", "new_path")        # Move/rename
shutil.disk_usage("/")                      # Disk space info
```

### Practice

```python
#!/usr/bin/env python3
"""Server inventory tool using the standard library."""
import argparse
import json
import logging
import subprocess
from pathlib import Path
from datetime import datetime, timezone

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s",
)
logger = logging.getLogger(__name__)

INVENTORY_FILE = Path("inventory.json")


def load_inventory() -> list[dict]:
    if INVENTORY_FILE.exists():
        return json.loads(INVENTORY_FILE.read_text(encoding="utf-8"))
    return []


def save_inventory(servers: list[dict]) -> None:
    INVENTORY_FILE.write_text(
        json.dumps(servers, indent=2) + "\n",
        encoding="utf-8",
    )
    logger.info("Saved %d servers to %s", len(servers), INVENTORY_FILE)


def ping_server(hostname: str) -> bool:
    try:
        subprocess.run(
            ["ping", "-c", "1", "-W", "2", hostname],
            capture_output=True,
            check=True,
        )
        return True
    except subprocess.CalledProcessError:
        return False


def cmd_add(args):
    servers = load_inventory()
    server = {
        "hostname": args.hostname,
        "ip": args.ip,
        "role": args.role,
        "added": datetime.now(timezone.utc).isoformat(),
    }
    servers.append(server)
    save_inventory(servers)
    logger.info("Added server: %s", args.hostname)


def cmd_list(args):
    servers = load_inventory()
    for s in servers:
        print(f"{s['hostname']:20s} {s['ip']:15s} {s['role']}")


def cmd_check(args):
    servers = load_inventory()
    for s in servers:
        reachable = ping_server(s["ip"])
        status = "UP" if reachable else "DOWN"
        print(f"{s['hostname']:20s} {status}")


def main():
    parser = argparse.ArgumentParser(description="Server inventory tool")
    sub = parser.add_subparsers(dest="command", required=True)

    add_p = sub.add_parser("add", help="Add a server")
    add_p.add_argument("hostname")
    add_p.add_argument("ip")
    add_p.add_argument("--role", default="general")
    add_p.set_defaults(func=cmd_add)

    list_p = sub.add_parser("list", help="List all servers")
    list_p.set_defaults(func=cmd_list)

    check_p = sub.add_parser("check", help="Check server reachability")
    check_p.set_defaults(func=cmd_check)

    args = parser.parse_args()
    args.func(args)


if __name__ == "__main__":
    main()
```

### Connection

The standard library is your first line of defense before reaching for third-party packages. Need to call a shell command? `subprocess`, not `os.system()`. Need to parse CLI arguments? `argparse`, not manual `sys.argv` parsing. Need to work with paths? `pathlib`, not string concatenation. Every module here will appear repeatedly in later domains: `subprocess` for running Git and Docker commands, `logging` for observability, `argparse` for CLI tools, `re` for parsing structured text, `datetime` for timestamps in monitoring and alerting.

> **Try It**: Build a CLI tool with `argparse` that has two subcommands: `convert` (takes a JSON file and outputs YAML) and `validate` (takes a YAML file and checks if it parses without errors). Use `pathlib` for file paths, `logging` for status messages, and proper error handling.

---

## M. Python Internals

### Theory

Understanding how Python executes code explains its performance characteristics and informs your decisions about concurrency, memory, and optimization.

**Source to bytecode to execution.** When you run a Python script, CPython (the standard Python implementation) does not interpret the source code directly. It compiles it to bytecode, then executes the bytecode on the CPython virtual machine.

```
source.py  →  compile  →  bytecode (.pyc)  →  CPython VM  →  result
```

```python
# Inspect bytecode
import dis

def add(a, b):
    return a + b

dis.dis(add)
#   0 LOAD_FAST    0 (a)
#   2 LOAD_FAST    1 (b)
#   4 BINARY_ADD
#   6 RETURN_VALUE
```

`.pyc` files in `__pycache__/` are cached bytecode. Python regenerates them when the source file changes. They speed up import time (not execution time) by skipping the compilation step.

**The Global Interpreter Lock (GIL).** The GIL is a mutex that allows only one thread to execute Python bytecode at a time. It exists because CPython's memory management (reference counting) is not thread-safe.

Implications:

| Scenario | Impact |
|----------|--------|
| CPU-bound threading | GIL prevents true parallelism -- threads take turns |
| I/O-bound threading | GIL is released during I/O waits -- threads run concurrently |
| Multiprocessing | Each process has its own GIL -- true parallelism |
| C extensions | GIL can be released by C code -- NumPy does this |

```python
import threading
import time

# CPU-bound: threading does NOT help (GIL)
def cpu_work(n):
    total = 0
    for i in range(n):
        total += i * i
    return total

# This is NOT faster than sequential -- the GIL serializes it
start = time.perf_counter()
threads = [threading.Thread(target=cpu_work, args=(10**7,)) for _ in range(4)]
for t in threads:
    t.start()
for t in threads:
    t.join()
print(f"Threaded: {time.perf_counter() - start:.2f}s")

# Use multiprocessing for CPU-bound work
from concurrent.futures import ProcessPoolExecutor

start = time.perf_counter()
with ProcessPoolExecutor(max_workers=4) as executor:
    results = list(executor.map(cpu_work, [10**7] * 4))
print(f"Multiprocessing: {time.perf_counter() - start:.2f}s")  # Actually faster
```

**Memory management.** CPython uses two mechanisms:

1. **Reference counting.** Every object has a reference count. When it drops to zero, the object is immediately deallocated.

```python
import sys

a = [1, 2, 3]
print(sys.getrefcount(a))   # 2 (a + the getrefcount arg)

b = a
print(sys.getrefcount(a))   # 3 (a + b + the getrefcount arg)

del b
print(sys.getrefcount(a))   # 2 again
```

2. **Generational garbage collection.** Reference counting cannot handle circular references (A references B, B references A). The generational GC detects and collects these cycles. It uses three generations -- objects that survive GC runs are promoted to older generations, which are collected less frequently.

```python
import gc

# Circular reference -- reference counting alone cannot collect this
class Node:
    def __init__(self):
        self.parent = None
        self.children = []

a = Node()
b = Node()
a.children.append(b)
b.parent = a

# Delete references
del a, b
# Reference counts are still > 0 (circular reference)
# The generational GC will eventually collect them

# Force garbage collection
gc.collect()
print(gc.get_stats())  # Shows collection statistics per generation
```

**Threading vs. multiprocessing decision matrix:**

| Factor | threading | multiprocessing |
|--------|-----------|-----------------|
| I/O-bound work (network, disk) | Good | Overkill |
| CPU-bound work (computation) | Useless (GIL) | Good |
| Memory sharing | Shared (easy, but needs locks) | Separate (safe, but needs IPC) |
| Startup cost | Low | High (fork/spawn) |
| Debugging | Hard (race conditions) | Harder (separate processes) |

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
import time

def io_task(url: str) -> str:
    """Simulate I/O-bound work."""
    time.sleep(1)  # Simulate network delay
    return f"Fetched {url}"

def cpu_task(n: int) -> int:
    """CPU-bound work."""
    return sum(i * i for i in range(n))

# I/O-bound: use ThreadPoolExecutor
urls = [f"http://example.com/{i}" for i in range(10)]
start = time.perf_counter()
with ThreadPoolExecutor(max_workers=10) as executor:
    results = list(executor.map(io_task, urls))
print(f"Threaded I/O: {time.perf_counter() - start:.2f}s")   # ~1 second

# CPU-bound: use ProcessPoolExecutor
start = time.perf_counter()
with ProcessPoolExecutor(max_workers=4) as executor:
    results = list(executor.map(cpu_task, [10**7] * 4))
print(f"Parallel CPU: {time.perf_counter() - start:.2f}s")
```

### Practice

```python
import dis
import sys
import gc

# Explore Python internals hands-on

# 1. Examine bytecode for different operations
def example():
    x = 10
    y = 20
    return x + y

print("=== Bytecode ===")
dis.dis(example)

# 2. Observe reference counting
print("\n=== Reference Counting ===")
data = {"key": "value"}
print(f"Initial refcount: {sys.getrefcount(data)}")
ref = data
print(f"After assignment: {sys.getrefcount(data)}")
container = [data]
print(f"After list append: {sys.getrefcount(data)}")
del ref
print(f"After del ref: {sys.getrefcount(data)}")

# 3. GC statistics
print("\n=== GC Stats ===")
stats = gc.get_stats()
for i, gen in enumerate(stats):
    print(f"Generation {i}: {gen}")

# 4. Object sizes
print("\n=== Object Sizes ===")
objects = [
    42,
    3.14,
    "hello",
    [1, 2, 3],
    {"a": 1, "b": 2},
    {1, 2, 3},
    (1, 2, 3),
]
for obj in objects:
    print(f"{type(obj).__name__:>10}: {sys.getsizeof(obj):>6} bytes  {obj!r}")
```

### Connection

These internals directly affect infrastructure decisions. The GIL means CPU-bound Python code does not benefit from threading -- use multiprocessing or switch to Go/Rust for compute-heavy tasks. Reference counting means objects are deallocated promptly (unlike Java's GC, which has unpredictable pauses), but circular references require the generational GC. Understanding bytecode explains why Python is slower than compiled languages but fast enough for infrastructure automation where I/O (network, disk) dominates execution time. In [Data Structures and Algorithms](/learn/first-principles/data-structures-and-algorithms/), you will analyze algorithmic complexity. The constant factors (bytecode interpretation overhead, GIL contention) affect real-world performance on top of big-O analysis.

> **Try It**: Use `dis.dis()` to examine the bytecode of a function that uses a list comprehension versus an equivalent `for` loop with `append`. Compare the number of bytecode instructions. Then use `sys.getsizeof()` to compare the memory usage of a `list`, `tuple`, `dict`, and `set` each containing the same 1000 integers.

---

## N. Project Structure

### Theory

As projects grow beyond a single script, structure determines maintainability. Python uses modules and packages to organize code.

**Modules** are Python files. Every `.py` file is a module. Import code from modules with `import`.

```python
# utils.py
def sanitize(text: str) -> str:
    return text.strip().lower()

# main.py
from utils import sanitize
print(sanitize("  Hello  "))  # "hello"
```

**Packages** are directories containing an `__init__.py` file (which can be empty). They organize modules into namespaces.

```
my_package/
├── __init__.py
├── config.py
├── deploy.py
└── utils/
    ├── __init__.py
    ├── network.py
    └── file_ops.py
```

```python
# Import from packages
from my_package.config import load_config
from my_package.utils.network import ping
from my_package import deploy
```

**Entry points.** The `if __name__ == "__main__"` guard runs code only when the file is executed directly, not when imported.

```python
# cli.py
def main():
    print("Running as CLI tool")

if __name__ == "__main__":
    main()
```

For packages, `__main__.py` lets you run the package with `python -m my_package`.

```
my_package/
├── __init__.py
├── __main__.py     # python -m my_package runs this
└── core.py
```

**src layout vs. flat layout:**

```
# src layout (recommended for libraries)
my-project/
├── src/
│   └── my_package/
│       ├── __init__.py
│       └── core.py
├── tests/
│   └── test_core.py
├── pyproject.toml
└── README.md

# flat layout (simpler, common for applications)
my-project/
├── my_package/
│   ├── __init__.py
│   └── core.py
├── tests/
│   └── test_core.py
├── pyproject.toml
└── README.md
```

The `src` layout prevents accidentally importing the local package instead of the installed one during testing. It is the recommended layout for distributable packages.

**.gitignore for Python:**

```gitignore
# Byte-compiled files
__pycache__/
*.py[cod]
*$py.class

# Virtual environments
.venv/
venv/
env/

# Distribution
dist/
build/
*.egg-info/

# Testing
.pytest_cache/
.coverage
htmlcov/

# IDE
.vscode/
.idea/

# Type checking
.mypy_cache/

# Linting
.ruff_cache/
```

### Practice

```bash
# Create a well-structured Python project from scratch

mkdir -p my-infra-tool/src/infra_tool
mkdir -p my-infra-tool/tests

# Create package files
cat > my-infra-tool/src/infra_tool/__init__.py << 'EOF'
"""Infrastructure management toolkit."""
__version__ = "0.1.0"
EOF

cat > my-infra-tool/src/infra_tool/core.py << 'EOF'
"""Core infrastructure operations."""
from dataclasses import dataclass

@dataclass
class Server:
    hostname: str
    ip: str
    port: int = 22

    def __str__(self) -> str:
        return f"{self.hostname} ({self.ip}:{self.port})"
EOF

cat > my-infra-tool/src/infra_tool/__main__.py << 'EOF'
"""Entry point for python -m infra_tool."""
from infra_tool.core import Server

def main():
    s = Server("web-1", "10.0.1.10", 80)
    print(f"Managing: {s}")

if __name__ == "__main__":
    main()
EOF

cat > my-infra-tool/tests/test_core.py << 'EOF'
from infra_tool.core import Server

def test_server_str():
    s = Server("web-1", "10.0.1.10", 80)
    assert str(s) == "web-1 (10.0.1.10:80)"

def test_server_default_port():
    s = Server("bastion", "10.0.0.1")
    assert s.port == 22
EOF

cat > my-infra-tool/pyproject.toml << 'EOF'
[project]
name = "infra-tool"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = []

[project.optional-dependencies]
dev = ["pytest>=8.0", "ruff>=0.3.0"]

[project.scripts]
infra-tool = "infra_tool.__main__:main"

[tool.pytest.ini_options]
testpaths = ["tests"]
EOF
```

```bash
# Set up and test
cd my-infra-tool
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"    # Install in editable mode with dev deps
pytest -v                   # Run tests
python -m infra_tool        # Run as module
```

### Connection

Project structure is not cosmetic. It determines whether your code is importable, testable, and distributable. In [Software Engineering and Collaboration](/learn/first-principles/software-engineering-and-collaboration/), you will publish Python packages, build Docker images from `pyproject.toml`, and set up CI/CD pipelines that depend on standard project layout. Every professional Python project follows these conventions. Knowing them means you can navigate any open-source Python project and contribute immediately.

> **Try It**: Create a Python project with the `src` layout. Include a package with two modules, a `__main__.py` entry point, a `tests/` directory with at least two test files and a `conftest.py`, and a `pyproject.toml`. Set up a virtual environment, install the package in editable mode, and run the tests.

---

## Exercises

These exercises integrate concepts across multiple sections of this domain.

1. **Configuration manager.** Build a CLI tool that reads server configurations from a YAML file, validates required fields (hostname, ip, role), and outputs the configuration in JSON. Handle missing files, invalid YAML, and missing fields with custom exceptions. Include `argparse` subcommands for `validate`, `convert`, and `list`. Write pytest tests with at least 80% coverage.

2. **Log analyzer.** Write a program that processes a log file (real or generated) and produces a summary: total lines, lines per log level, top 10 most common messages, and errors per hour. Use generators to handle arbitrarily large files. Use `Counter`, `defaultdict`, and `datetime` for aggregation. Use `re` to parse log lines. Output the summary as both a formatted table (to stdout) and a JSON file.

3. **Server inventory system.** Design a class hierarchy: `Server` (base), `WebServer`, `DatabaseServer`, `LoadBalancer`. Use dataclasses, properties, and dunder methods (`__repr__`, `__eq__`, `__hash__`, `__iter__` for `LoadBalancer`). Implement a `StorageBackend` protocol with `InMemoryStorage` and `FileStorage` (JSON) implementations. Write a CLI with `argparse`. Test with pytest fixtures and mocking (mock the file system for `FileStorage` tests).

4. **Build tool wrapper.** Create a Python wrapper around `git` using `subprocess`. Support `status`, `log`, `diff`, and `commit` subcommands. Parse the output into structured Python objects. Handle errors (not a git repo, merge conflicts, uncommitted changes). Use `pathlib` for path handling and `logging` for debug output. Write tests that mock `subprocess.run`.

5. **Concurrent health checker.** Write a tool that reads a list of URLs from a YAML file and checks each one concurrently using `ThreadPoolExecutor`. Report the HTTP status code and response time for each URL. Use `dataclasses` for results, `logging` for progress, and output a CSV report. Compare single-threaded vs. multi-threaded performance. Add a `--timeout` flag with `argparse`.

---

## Assessment Dimensions

### Explain

You can explain why Python uses dynamic typing and what tradeoffs it creates. You can describe the difference between a list and a dict in terms of their underlying data structures and time complexity. You can explain the GIL, why it exists, and when it matters. You can articulate SOLID principles with concrete examples. You can explain EAFP vs. LBYL and why Python favors EAFP. You can describe how Python's memory management works (reference counting plus generational GC) and why circular references require the GC.

### Build

You can write a Python CLI tool with multiple subcommands, file I/O in multiple formats, custom exceptions, proper logging, and type hints. You can create a class hierarchy with dataclasses, protocols, and composition. You can set up a project with virtual environment, `pyproject.toml`, `src` layout, and pytest test suite with fixtures, parametrize, and mocking. You can write generators that process data lazily with constant memory.

### Debug

Given a Python program that produces incorrect results due to mutable default arguments, you can identify and fix the bug. Given a multi-threaded program that is slower than single-threaded, you can explain that the GIL prevents CPU-bound parallelism and suggest `multiprocessing` instead. Given a test that passes in isolation but fails when run with the full suite, you can identify shared mutable state between tests and fix it with proper fixture scoping. Given a `UnicodeDecodeError` when reading a file, you can diagnose the encoding mismatch and fix it with explicit `encoding='utf-8'`.

---

## Key Takeaways

- Python's dynamic typing speeds up development but shifts errors to runtime -- use type hints and mypy to catch mistakes early
- Lists are dynamic arrays (O(1) append, O(n) insert), dicts are hash tables (O(1) lookup), sets provide O(1) membership testing -- choose based on access patterns
- Generators enable lazy evaluation, processing large datasets with constant memory
- Decorators are functions that wrap functions -- they are how frameworks (Flask, pytest, click) extend behavior
- LEGB scope rule determines variable resolution: Local, Enclosing, Global, Built-in
- Prefer composition over inheritance, protocols over abstract classes, dataclasses over manual `__init__`
- EAFP (try/except) over LBYL (check first) -- it is idiomatic Python and avoids race conditions
- Context managers (with statement) guarantee cleanup -- use them for files, connections, locks, and temporary state
- Virtual environments are not optional -- every project needs isolated dependencies
- Tests with pytest are the mechanism for changing code without fear -- fixtures, parametrize, and mocking make them maintainable
- The GIL means threading helps for I/O but not for CPU -- use multiprocessing or async for true parallelism
- The standard library (pathlib, subprocess, argparse, logging, re, datetime, collections) handles most tasks before you need third-party packages
- Project structure (src layout, pyproject.toml, tests/) determines whether code is importable, testable, and distributable

## Resources & Further Reading

- [Python Documentation](https://docs.python.org/3/)
- [PEP 8 -- Style Guide for Python Code](https://peps.python.org/pep-0008/)
- [PEP 484 -- Type Hints](https://peps.python.org/pep-0484/)
- [Real Python](https://realpython.com/)
- [pytest Documentation](https://docs.pytest.org/)
- [Python Module of the Week](https://pymotw.com/3/)
- [Fluent Python by Luciano Ramalho](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/)
- [Effective Python by Brett Slatkin](https://effectivepython.com/)
- [ruff -- Python Linter and Formatter](https://docs.astral.sh/ruff/)
- [uv -- Python Package Manager](https://docs.astral.sh/uv/)
- [mypy -- Static Type Checker](https://mypy-lang.org/)
- [Python Bytecode Documentation](https://docs.python.org/3/library/dis.html)
- [concurrent.futures Documentation](https://docs.python.org/3/library/concurrent.futures.html)
