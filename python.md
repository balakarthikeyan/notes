# 🐍 Python Study Guide & Core Concepts

## 🗺️ Learning Roadmap & Core Milestones
* **Data Types:** Master foundational structures (`int`, `float`, `str`, `list`, `dict`, `set`, `tuple`).
* **Control Flow:** Practice loops (`for`, `while`) and conditional branches (`if`, `elif`, `else`).
* **Functional Programming:** Define reusable blocks of logic (`def`) and understand scope (`local` vs. `global`).
* **Modular Architecture:** Import, leverage, and build modules using both standard libraries (`math`, `random`) and custom files.
* **Object-Oriented Programming (OOP):** Master encapsulation, classes, objects, attributes, and methods.
* **Advanced OOP:** Implement code reusability and dynamic behavior using inheritance and polymorphism.
* **Environment Isolation:** Leverage `venv` to create clean, isolated project environments.
* **Dependency Management:** Install, track, and manage third-party packages securely using `pip` (e.g., `requests`, `numpy`).
* **Project Structure:** Understand and implement enterprise-grade directory layouts for scalable development.

---

## Python Syntax & Core Fundamentals

### Core Summary

A fundamental syntax reference cheat sheet mapping out variable declaration patterns, core data structures, basic arithmetic structures, documentation syntax, terminal stream management, functional blocks, logical paths, and initialization evaluations.

### Syntax Breakdown

#### Variables & Built-In Data Types

```python
# Declaration Pattern
my_var = 5

# Data Type Evolutionary Paradigms (Legacy vs. Modern)
Integer: 5
Float:   5.0
Bool:    True, False
String:  "Hello"
Tuple:   (1, 2, 3, 4, 5)
List:    [1, 2, 3, 4, 5]
Dict:    {"2": 4, "3": 9}
Set:     {2, 4, 5}

# Legacy Historical Note:
# In Python 2, a long integer was explicitly indicated with an 'L' suffix (e.g., 5L). 
# In Python 3, the 'int' and 'long' types are completely unified; the 'L' suffix is a syntax error.

```

#### Core Arithmetic Operations

* **Sum:** `a + b`
* **Difference:** `a - b`
* **Product:** `a * b`
* **Quotient:** `a / b` (Always returns a `float` in Python 3)
* **Mod (Remainder):** `a % b`
* **Power:** `a^b` (Corrected from legacy structural notation)
* **Integer Division:** `a // b` (Floors the resulting quotient)

#### Built-In List Mechanics

* **Indexing:** `list[index]`
* **Length Verification:** `len(list)`
* **Slicing:** `list[start:end]`
* **Appending Elements:** `list.append(obj)`
* **Removing Elements:** `list.remove(obj)`

#### Code Documentation

* **Single-Line Comment:** `# this is comment`
* **Multi-Line Docstring:** `"""multi-line"""`

#### Basic User Stream Input/Output

* **Stream Input Extraction:** `v = input("msg")` (Always returns a `str` object in Python 3)
* **Stream Output Injection:** `print(v)`

#### Functional Block Schemes

* **Standard Definition:**

```python
def func(a, b): 
    return a + b

```

* **Anonymous (Lambda) Definition:**

```python
func = lambda a, b : a + b

```

#### Logical Controls & Flow Blocks

```python
# Boolean Assertions & Comparison Evaluators
# Examples: >, <, >=, <=, ==, !=

# Conditional Evaluation Blocks
if True:
    statement
elif True:
    statement
else:
    statement

```

#### Structural Loops

```python
# While Loop Pattern
while condition:
    statement
    i += 1

# For Range Loop Pattern
for i in range(10):
    print(i)

```

#### Equality Verifications

* **Value (Object) Structural Match:** `val_1 == val_2` (Evaluates truth based on data equality)
* **Referential (Identity) Memory Match:** `[1, 2, 3, 4] is [1, 2, 3, 4]` *(Evaluates to `False` because lists are mutable and instantiated at separate distinct memory references)*

---

## 📚 Detailed Sequence Types

### List (Mutable)
```python
fruits = ["apple", "banana", "cherry"]
print("Original:", fruits)

fruits[1] = "mango"   # Modify element in-place
fruits.append("grape") # Append new element dynamically
print("Modified:", fruits)

```

**Output:**

```text
Original: ['apple', 'banana', 'cherry']
Modified: ['apple', 'mango', 'cherry', 'grape']

```

### Tuple (Immutable)

```python
coordinates = (10, 20, 30)
print("Original:", coordinates)

# Attempting to modify an immutable sequence will trigger a runtime error
try:
    coordinates[1] = 50
except TypeError as e:
    print("Error:", e)

```

**Output:**

```text
Original: (10, 20, 30)
Error: 'tuple' object does not support item assignment

```

### Range

```python
numbers = range(5)
print("Range values:", list(numbers))

```

**Output:**

```text
Range values: [0, 1, 2, 3, 4]

```

---

## 🗂️ Mapping Type

### Dictionary (Mutable)

```python
student = {"name": "John", "age": 25}
print("Original:", student)

student["age"] = 26   # Update existing key's value
student["grade"] = "A" # Insert a brand new key-value pair
print("Modified:", student)

```

**Output:**

```text
Original: {'name': 'John', 'age': 25}
Modified: {'name': 'John', 'age': 26, 'grade': 'A'}

```

---

## ✅ Boolean Type

```python
is_active = True
print("Boolean value:", is_active)

if is_active:
    print("The system is active.")

```

**Output:**

```text
Boolean value: True
The system is active.

```

---

## 🔗 Set Types

### Set (Mutable)

```python
colors = {"red", "green", "blue"}
print("Original:", colors)

colors.add("yellow")   # Add a unique element
colors.remove("green") # Remove an existing element
print("Modified:", colors)

```

**Output:** *(Note: Set output ordering may vary slightly at runtime due to hashing architecture)*

```text
Original: {'red', 'green', 'blue'}
Modified: {'red', 'blue', 'yellow'}

```

### Frozenset (Immutable)

```python
frozen_colors = frozenset(["red", "green", "blue"])
print("Frozen set:", frozen_colors)

# Attempting to mutate a frozen set triggers an error as it lacks mutating methods
try:
    frozen_colors.add("yellow")
except AttributeError as e:
    print("Error:", e)

```

**Output:**

```text
Frozen set: frozenset({'red', 'green', 'blue'})
Error: 'frozenset' object has no attribute 'add'

```

---

## 💾 Binary Types

### Bytes (Immutable)

```python
b = b"Hello"
print("Bytes:", b)

# Bytes objects are immutable array-like structures containing integers 0-255
try:
    b[0] = 65
except TypeError as e:
    print("Error:", e)

```

**Output:**

```text
Bytes: b'Hello'
Error: 'bytes' object does not support item assignment

```

### Bytearray (Mutable)

```python
ba = bytearray([65, 66, 67]) # Corresponding to ASCII characters A, B, C
print("Original:", ba)

ba[1] = 90  # Mutate index 1 directly: change ASCII value from B (66) → Z (90)
print("Modified:", ba)

```

**Output:**

```text
Original: bytearray(b'ABC')
Modified: bytearray(b'AZC')

```

### Memoryview

```python
mv = memoryview(b"Python")
print("Memoryview first element:", mv[0])  # Returns the raw ASCII integer underlying 'P'

# Convert back to high-level bytes object representation
print("As bytes:", mv.tobytes())

```

**Output:**

```text
Memoryview first element: 80
As bytes: b'Python'

```

---

## 📝 Special Type

### NoneType

```python
x = None
print("Value:", x)

# Always use identity operators ('is' / 'is not') when checking for None
if x is None:
    print("x has no value assigned.")

```

**Output:**

```text
Value: None
x has no value assigned.

```
---

## 📊 Core Data Types Overview

| Category | Type | Mutability | Description |
| :--- | :--- | :--- | :--- |
| **Numeric Types** | `int` | Immutable | Whole numbers of arbitrary precision |
| | `float` | Immutable | Double-precision floating-point numbers |
| | `complex` | Immutable | Numbers containing real and imaginary parts (`z = a + bj`) |
| **Sequence Types** | `list` | **Mutable** | Ordered, dynamically-sized mutable sequences |
| | `tuple` | Immutable | Ordered, fixed-length immutable sequences |
| | `range` | Immutable | Efficient sequence of numbers generated on demand |
| **Mapping Type** | `dict` | **Mutable** | Unordered (insertion-ordered since 3.7) key-value pairs |
| **Boolean Type** | `bool` | Immutable | Logical values representing `True` or `False` |
| **Set Types** | `set` | **Mutable** | Unordered collections of unique, hashable items |
| | `frozenset` | Immutable | Immutable variant of a standard set |
| **Binary Types** | `bytes` | Immutable | Immutable fixed-length sequences of single bytes |
| | `bytearray` | **Mutable** | Mutable arrays of single bytes |
| | `memoryview` | **Mutable** | Direct C-level memory access buffer wrapper without copying |
| **Special Type** | `NoneType` | Immutable | Represents the absence of a value (`None`) |

### 🔄 Mutability vs. Immutability Quick-Reference
* **Mutable:** Can be directly altered after creation without changing their memory address allocation.
    * *Examples:* `list`, `dict`, `set`, `bytearray`.
* **Immutable:** Cannot be altered after creation. Any modification operation generates a completely new object in memory.
    * *Examples:* `int`, `float`, `complex`, `tuple`, `str`, `frozenset`, `bytes`.

---

# Comprehensive Python

## Module 1: Built-In Functions Reference

### Core Summary

An exhaustive look-up index documenting 50 common core primary functions globally accessible within the standard Python interpreter namespace, displaying exact operational declarations alongside deterministic executions.

### Comprehensive Functions Directory

| Index | Syntax / Application Code Example | Expected Behavior / Evaluation Return |
| --- | --- | --- |
| **1** | `len([1, 2, 3])` | `3` |
| **2** | `type(3.14)` | `<class 'float'>` |
| **3** | `isinstance(3.14, float)` | `True` |
| **4** | `str(123)` | `'123'` |
| **5** | `int('123')` | `123` |
| **6** | `float('3.14')` | `3.14` |
| **7** | `list("Hello")` | `['H', 'e', 'l', 'l', 'o']` |
| **8** | `dict(a=1, b=2)` | `{'a': 1, 'b': 2}` |
| **9** | `set([1, 2, 3, 3, 2, 1])` | `{1, 2, 3}` |
| **10** | `tuple([1, 2, 3])` | `(1, 2, 3)` |
| **11** | `range(5)` | `range(0, 5)` |
| **12** | `sum([1, 2, 3])` | `6` |
| **13** | `max([1, 2, 3])` | `3` |
| **14** | `min([1, 2, 3])` | `1` |
| **15** | `sorted([3, 1, 2])` | `[1, 2, 3]` |
| **16** | `reversed([1, 2, 3])` | `<list_reverseiterator>` *(Yields `3`, `2`, `1` during iteration)* |
| **17** | `abs(-7)` | `7` |
| **18** | `round(3.14159, 2)` | `3.14` |
| **19** | `pow(2, 3)` | `8` |
| **20** | `divmod(9, 2)` | `(4, 1)` |
| **21** | `bin(10)` | `'0b1010'` |
| **22** | `hex(255)` | `'0xff'` |
| **23** | `oct(8)` | `'0o10'` |
| **24** | `ord('a')` | `97` |
| **25** | `chr(97)` | `'a'` |
| **26** | `all([True, True, False])` | `False` |
| **27** | `any([False, False, True])` | `True` |
| **28** | `zip([1, 2, 3], ['a', 'b', 'c'])` | `<zip object>` *(Evaluates to `[(1, 'a'), (2, 'b'), (3, 'c')]` when wrapped in `list()`)* |
| **29** | `enumerate(['a', 'b', 'c'])` | `<enumerate object>` *(Evaluates to `[(0, 'a'), (1, 'b'), (2, 'c')]` when wrapped in `list()`)* |
| **30** | `map(str.upper, ['a', 'b', 'c'])` | `<map object>` *(Evaluates to `['A', 'B', 'C']` when wrapped in `list()`)* |
| **31** | `filter(lambda x: x % 2 == 0, [1, 2, 3, 4])` | `<filter object>` *(Evaluates to `[2, 4]` when wrapped in `list()`)* |
| **32** | `iter([1, 2, 3])` | `<list_iterator object>` |
| **33** | `next(iter([1, 2, 3]))` | `1` |
| **34** | `open('file.txt', 'r')` | `<_io.TextIOWrapper ...>` |
| **35** | `len("Hello")` | `5` |
| **36** | `format(1234, ',')` | `'1,234'` |
| **37** | `id(1234)` | Internal unique object memory address integer (e.g., `139931812065552`) |
| **38** | `help(str)` | Interactive documentation help interface invocation |
| **39** | `dir(str)` | List of all valid properties and structural magic/inner methods |
| **40** | `print("Hello, World!")` | Outputs text to the standard stream; returns `None` |
| **41** | `input("Enter your name: ")` | Blocks execution waiting for user terminal string stream submission |
| **42** | `eval("3 + 4")` | Evaluates single expression string dynamically; returns `7` |
| **43** | `exec("x = 5")` | Compiles and executes multi-line code blocks dynamically; returns `None` |
| **44** | `globals()` | Dictionary mapping containing the current global scope namespace variables |
| **45** | `locals()` | Dictionary mapping containing the current local scope execution variables |
| **46** | `hasattr(obj, 'attr')` | Returns `True` if runtime object contains the matching string label attribute |
| **47** | `setattr(obj, 'attr', value)` | Dynamically writes or overwrites properties on target object runtime definitions |
| **48** | `getattr(obj, 'attr', default)` | Safely fetches property values dynamically, utilizing fallback defaults |
| **49** | `delattr(obj, 'attr')` | Permanently destroys/unbinds specific attribute definitions off runtime objects |
| **50** | `super().method()` | Proxy object allowing safe method dispatch to parent classes via Method Resolution Order (MRO) |

---

## Module 2: Collection Methods Reference (Lists, Sets, Dicts)

### Core Summary

A structural index map organizing data-mutation methods across Python's built-in collection types: `set`, `list`, and `dictionary`.

### Built-In Method Maps

```text
  ┌───────────────────────┐      ┌───────────────────────┐      ┌───────────────────────┐
  │         SET           │      │         LIST          │      │      DICTIONARY       │
  ├───────────────────────┤      ├───────────────────────┤      ├───────────────────────┤
  │ • add()               │      │ • append()            │      │ • clear()             │
  │ • clear()             │      │ • clear()             │      │ • copy()              │
  │ • copy()              │      │ • copy()              │      │ • fromkeys()          │
  │ • difference()        │      │ • count()             │      │ • get()               │
  │ • discard()           │      │ • extend()            │      │ • items()             │
  │ • intersection()      │      │ • index()             │      │ • keys()              │
  │ • isdisjoint()        │      │ • insert()            │      │ • pop()               │
  │ • issubset()          │      │ • pop()               │      │ • popitem()           │
  │ • issuperset()        │      │ • remove()            │      │ • setdefault()        │
  │ • pop()               │      │ • reverse()           │      │ • update()            │
  │ • remove()            │      │ • sort()              │      └───────────────────────┘
  │ • union()             │      └───────────────────────┘
  │ • update()            │
  └───────────────────────┘

```

###  Why these methods matter:
* Helps write cleaner and more efficient code.
* Avoids runtime data mutation exceptions and saves engineering time.
* Boosts production pipeline processing throughput.
* Solves tracking and unique entity membership checks.

---

## Module 3: Algorithmic Python One-Liners

### Core Summary

A curated compilation of 18 functional "one-liner" shortcuts optimizing standard computing requirements via inline collection processing statements, dictionary comprehension structures, and functional mapping keys.

#### 1. Stream File Line Aggregator

```python
print(sum(1 for line in open('filename.txt')))

```

#### 2. Sequence Palindrome Assertion

```python
print("Palindrome" if input_str == input_str[::-1] else "Not Palindrome")

```

#### 3. Maximum Collection Value Extractor

```python
print(max(lst))

```

#### 4. Inline Structural Character Reversion

```python
print(input_str[::-1])

```

#### 5. Absolute Prime Verification Assertion

```python
print("Prime" if all(num % i != 0 for i in range(2, int(num**0.5) + 1)) and num > 1 else "Not Prime")

```

#### 6. Recursive Lambda Fibonacci Generator

```python
fib = lambda n: n if n <= 1 else fib(n-1) + fib(n-2)

```

#### 7. Character Distribution Occurrences Mapping

```python
print({char: string.count(char) for char in string})

```

#### 8. Dimensional Matrix Transposition Engine

```python
print([[matrix[j][i] for j in range(len(matrix))] for i in range(len(matrix[0]))])

```

#### 9. Array Element Arithmetic Totalizer

```python
print(sum(lst))

```

#### 10. Distinct Elements Membership Asserter

```python
print("Unique" if len(lst) == len(set(lst)) else "Not Unique")

```

#### 11. Array List Deduplication Step

```python
print(list(set(lst)))

```

#### 12. Mathematical Factorial Calculation via Inline Lambda

```python
fact = lambda n: 1 if n == 0 else n * fact(n - 1)

```

#### 13. Complex List of Dictionaries Ordering via Lambda Key

```python
sorted_lst = sorted(lst, key=lambda x: x['key_name'])

```

#### 14. Second Smallest Distinct Array Element Search

```python
print(sorted(set(lst))[1])

```

#### 15. Single Expression Key-Value Dictionary Merge

```python
merged_dict = {**dict1, **dict2}

```

#### 16. Targeted Element Purging Comprehension

```python
lst = [x for x in lst if x != value_to_remove]

```

#### 17. Structural Anagram String Evaluation Method

```python
print("Anagram" if sorted(str1) == sorted(str2) else "Not Anagram")

```

#### 18. Multi-List Zip Dictionary Composition

```python
dictionary = dict(zip(keys_list, values_list))

```

---

## Module 4: Python Production Concepts

### Modern Type Hinting & Static Safety (Python 3.10+)

Modern production environments rely heavily on static type checkers like `mypy`. Avoid legacy syntax or reliance on basic annotations alone. Use the built-in pipe `|` syntax for Union types instead of importing `Union` from the `typing` module.

```python
from typing import Final, Literal

# Final prevents variables from being reassigned
DATABASE_URL: Final[str] = "localhost:5432"

# Literal restricts values to specific predefined options
Environment = Literal["dev", "staging", "prod"]

# Using the modern Pipe (|) operator for Union types
def process_user_id(user_id: int | str) -> str:
    if isinstance(user_id, int):
        return f"USR_{user_id:04d}"
    return user_id.strip().upper()

```

### High-Performance Structural Data Models: Data Classes

Instead of boilerplates involving writing custom `__init__`, `__repr__`, and equality checks manually within classes, use the modern native `@dataclass` decorator.

```python
from dataclasses import dataclass, field

@dataclass(frozen=True) # frozen=True enforces complete immutability
class Product:
    id: int
    name: str
    price: float
    tags: list[str] = field(default_factory=list) # Safe initialization for mutable defaults

# Automatic generation of descriptive __repr__ and robust struct comparisons
gadget = Product(id=101, name="Mechanical Keyboard", price=120.00)
print(gadget) # Product(id=101, name='Mechanical Keyboard', price=120.0, tags=[])

```

### Structural Pattern Matching (`match` / `case`)

Introduced in Python 3.10, this replaces long `if-elif-else` chains with declarative structural destructuring matching capabilities.

```python
def parse_api_response(response: dict | tuple) -> str:
    match response:
        case {"status": "success", "data": payload}:
            return f"Data retrieved successfully: {payload}"
        case {"status": "error", "error_code": int(code)}:
            return f"Critical error code encountered: {code}"
        case (int(http_code), str(message)):
            return f"Legacy Tuple Protocol: {http_code} - {message}"
        case _:
            return "Unknown response format received."

```

## Module 5: Enterprise Project Organization Best Practices

Modern applications use a standardized `src/` layout, layout combined with a consolidated `pyproject.toml` file to manage build definitions, dependencies, and linting rules.

### Structural Blueprint Layout

```text
enterprise_project/
├── .gitignore
├── README.md
├── pyproject.toml              # Unified/Universal configuration of metadata and dependency definitions, replacing setup.py
├── requirements.txt 
├── src/                        # Encapsulates codebase source folder protecting imports from path pollution
│   └── my_app/
│       ├── __init__.py         # Defines the directory as an importable module package
│       ├── core.py             # Core internal business processing rules
│       └── utils.py            # Supplementary helper utilities
└── tests/                      # Testing suites completely isolated unit and integration suites from the source tree
    ├── __init__.py
    └── test_core.py            # Automated testing verification scripts

```

#### Modern Dependency Management Configuration (`pyproject.toml`)

```toml
[project]
name = "my_app"
version = "1.0.0"
description = "A production-grade modern application structure"
requires-python = ">=3.10"
dependencies = [
    "requests>=2.31.0",
    "numpy>=1.26.0",
]

[tool.uv]
# Configured for modern blazing-fast rust-based 'uv' packaging ecosystems
dev-dependencies = [
    "pytest>=8.0.0",
    "mypy>=1.8.0",
]

```

---

### Definition

* **`src/` Layout Pattern:** An enterprise-grade architecture directory convention that puts all application source code inside a dedicated subfolder named `src/`, separating production code from configuration files and testing suites.

### Explanation

Using a flat directory structure where code sits in the root folder alongside tests can introduce subtle bugs. When running automated tests from the root, Python automatically appends the current directory to `sys.path`. This allows tests to import local files directly, masking issues like missing files or incomplete packaging configurations that only appear after deployment.

The `src/` layout resolves this by forcing you to install the package (often in editable mode via `pip install -e .`) to run tests. This structure ensures your tests run against the packaged version of the application, mirroring how the code behaves in production.

### Question and Answer

#### Q1: What specific issue does putting source code inside a `src/` directory solve?

* **Answer:** It prevents automated testing tools from accidentally importing local code files directly from the root directory instead of the installed package. This structure ensures that testing accurately verifies the packaged version of the application, catching missing configuration modules or packaging errors before deployment.

#### Q2: What role does a `pyproject.toml` file fill in modern Python structures?

* **Answer:** Defined in PEP 518 and PEP 621, `pyproject.toml` serves as a unified configuration file for Python projects. It replaces legacy setup files like `setup.py` and `requirements.txt` by consolidating build requirements, project metadata, dependency declarations, and tool configurations into a single file.

### Implementation Source Code Components

#### File: `src/my_app/core.py`

```python
def calculate_taxed_total(subtotal: float, tax_rate: float) -> float:
    if subtotal < 0 or tax_rate < 0:
        raise ValueError("Inputs cannot be negative values.")
    return round(subtotal * (1 + tax_rate), 2)

```

#### File: `tests/test_core.py`

```python
import pytest
# Imports from the installed module package layout environment securely
from my_app.core import calculate_taxed_total

def test_accurate_taxation_computation():
    assert calculate_taxed_total(100.00, 0.15) == 115.00
    assert calculate_taxed_total(50.00, 0.00) == 50.00

def test_negative_value_handling_exceptions():
    with pytest.raises(ValueError):
        calculate_taxed_total(-10.00, 0.05)

```

### Execution Trace (Running Tests)

```bash
# Executing automated tests across the directory layout via pytest
pytest tests/

```

### Output

```text
============================= test session starts =============================
collected 2 items

tests/test_core.py ..                                                    [100%]

============================== 2 passed in 0.03s ==============================

```