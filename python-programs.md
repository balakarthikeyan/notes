## Topic 1: Python Data Types & Mutability (Lists vs. Tuples)

### Definition

* **Built-in Data Types:** Python includes primitive and collection types: numeric (`int`, `float`, `complex`), sequences (`str`, `list`, `tuple`, `range`), mappings (`dict`), sets (`set`, `frozenset`), binary (`bytes`, `bytearray`, `memoryview`), and null (`NoneType`).
* **Mutability:** The characteristic determining whether an object's state can be changed in place after instantiation.

### Explanation

Lists are mutable (changeable) collections that use square brackets: [1, 2, 3], ordered sequences, where elements can be updated, appended, or removed after creation. 
Tuples are immutable (fixed) collections using parentheses: (1, 2, 3), ordered sequences, where elements cannot be changed or reassigned after instantiation. Attempting to modify a tuple item directly raises a `TypeError`.

### Question and Answer

#### Q: What happens when you attempt to modify an element inside a tuple?

* **Answer:** Python raises a `TypeError` exception because tuples are immutable objects designed to preserve fixed data.

### Program

```python
def demonstrate_mutability():
    # List demonstration (Mutable)
    fruits = ["apple", "banana", "cherry"]  # list
    print(f"Original List: {fruits}")

    fruits[1] = "mango"  # Modify element in place
    fruits.append("grape")  # Add new element
    print(f"Modified List: {fruits}")

    # Tuple demonstration (Immutable)
    coordinates = (10, 20)  # tuple
    print(f"Original Tuple: {coordinates}")
    try:
        coordinates[1] = 50  # Attempt modification
    except TypeError as error:
        print(f"Caught Expected Error: {error}")

# --- Execution Demonstration ---
if __name__ == "__main__":
    demonstrate_mutability()
```

### Output

```text
Original List: ['apple', 'banana', 'cherry']
Modified List: ['apple', 'mango', 'cherry', 'grape']
Original Tuple: (10, 20)
Caught Expected Error: 'tuple' object does not support item assignment
```

#### Q: Alternatives to `f-string`?

* **Answer:** The **`f`** stands for **f-string** (formatted string literal). It allows you to evaluate and embed Python variables or expressions directly inside `{}` curly braces within the string.

### Program

1. **`.format()` method:**
```python
print("Hello, {}".format(name))

```

2. **`%` operator (printf-style):**
```python
print("Hello, %s" % name)
```

3. **Comma-separated arguments in `print()`:**
```python
print("Hello,", name)
```

4. **String Concatenation (`+`):**
```python
print("Hello, " + name)
```

*(Note: `f-string` are the modern, preferred standard because they are cleaner and faster.)*

---

## Topic 2: Function Type Hints & Annotations

### Definition

* **Type Hints:** A formal syntax introduced in Python (PEP 484) that allows developers to annotate expected data types for function arguments and return values.

### Explanation

In basic Python functions, parameters and return values do not enforce specific data types. Type hints add clarity by using the `param: type -> return_type` syntax. While Python remains a dynamically typed language and does not enforce type annotations at runtime, type hints improve code readability, enable IDE autocompletion, and allow static analysis tools (like `mypy`) to catch type-related bugs early.

### Question and Answer

#### Q: Does Python enforce function type hints at runtime?

* **Answer:** No, Python does not enforce type annotations at runtime. Passing an argument of a different type will not automatically raise a `TypeError` during execution; type hints serve primarily as documentation and guidelines for static analysis tools, linters, and IDEs.

### Program

```python
# Standard function signature with explicit type annotations
def add(a: int, b: int) -> int:
    return a + b

# --- Execution Demonstration ---
if __name__ == "__main__":
    result = add(5, 3)
    print(f"Calculated Sum: {result}")
```

### Output

```text
Calculated Sum: 8
```

*(Note: PEP 484 (Python Enhancement Proposal 484) is the official specification that introduced standardized Type Hints to Python.)*

---

## Topic 3: Class Inheritance & Method Overriding

### Definition

* **Inheritance:** An object-oriented mechanism where a child class inherits attributes and methods from a parent class.
* **Method Overriding:** Re-implementing a parent class method inside a child class to provide specific behavior.

### Explanation

Classes define object blueprints and state initialization via `__init__()`. Child classes inherit from parent classes by passing the parent class name in parentheses (e.g., `class Dog(Animal):`). The child class inherits all parent methods but can override specific methods to specialize its behavior.

### Question and Answer

#### Q: How does a child class override a method inherited from a parent class?

* **Answer:** A child class defines a method with the exact same name as the method in its parent class. When called on a child class instance, Python executes the child's implementation instead of the parent's.

### Program

```python
# Base Parent Class
class Animal:
    def __init__(self, name: str):
        self.name = name

    def speak(self) -> None:
        print(f"{self.name} makes a sound")  # Base implementation

# Derived Child Class (Inheritance)
class Dog(Animal):
    # Method Overriding
    def speak(self) -> None:
        print(f"{self.name} barks!")  # Specialized child implementation

# --- Execution Demonstration ---
if __name__ == "__main__":
    generic_animal = Animal("Generic")
    generic_animal.speak()

    dog = Dog("Mini")  # Instantiating child class
    dog.speak()  # Invokes overridden child method
```

### Output

```text
Generic makes a sound
Mini barks!
```

---

## Topic 4: Standard Library Modules

### Definition

* **Module:** A file containing Python definitions, functions, and statements designed for import and reuse across programs.
* **Standard Library:** Built-in modules provided with Python (e.g., `math`, `random`, `datetime`) that offer ready-to-use functionality without external installations.

### Explanation

Python provides built-in utilities through standard library modules:

* `math`: Provides mathematical operations like `math.sqrt()`.
* `random`: Generates pseudo-random values like `random.randint()`.
* `datetime`: Handles date and time formatting via `datetime.date.today()`.

### Question and Answer

#### Q: How do built-in modules differ from custom modules?

* **Answer:** Built-in standard library modules are included with Python. Custom modules are user-defined `.py` files containing code that can be imported using `import filename` or `from filename import function`.

### Program

```python
import datetime
import math
import random

def demonstrate_standard_modules():
    # Math operations
    square_root = math.sqrt(16)  # Output: 4.0
    print(f"Math Square Root (16): {square_root}")

    # Random operations
    random_dice_roll = random.randint(1, 6)  # Random integer between 1 and 6
    print(f"Random Dice Roll (1-6): {random_dice_roll}")

    # Datetime operations
    current_date = datetime.date.today()  # Fetches current date
    print(f"Current Date: {current_date}")

# --- Execution Demonstration ---
if __name__ == "__main__":
    demonstrate_standard_modules()
```

### Output

```text
Math Square Root (16): 4.0
Random Dice Roll (1-6): 4
Current Date: 2026-08-04
```

---

## Topic 5: Data Classes (`@dataclass`)

### Definition

* **Data Class:** A class decorator (`@dataclass`) provided by the `dataclasses` module that automatically generates special boilerplate methods like `__init__()`, `__repr__()`, and `__eq__()` for data-focused classes.

### Explanation

Writing data-container classes manually requires boilerplate code for initialization and string representation. Applying the `@dataclass` decorator automates method generation using class type annotations.

### Question and Answer

#### Q: What automatic methods does the `@dataclass` decorator generate?

* **Answer:** `@dataclass` automatically generates `__init__()` for attribute assignment, `__repr__()` for readable object string output, and `__eq__()` for object attribute equality comparisons.

### Program

```python
from dataclasses import dataclass

@dataclass
class Book:
    title: str
    author: str
    pages: int  # Decorator automatically generates __init__ and __repr__

def describe(book: Book) -> str:
    return f"'{book.title}' by {book.author}, containing {book.pages} pages."

# --- Execution Demonstration ---
if __name__ == "__main__":
    b = Book("Python Basics", "Alice", 200)  # Instantiation using auto-generated __init__
    print(f"Auto-generated Repr: {b}")  # Printing uses auto-generated __repr__
    print(f"Description: {describe(b)}")
```

### Output

```text
Auto-generated Repr: Book(title='Python Basics', author='Alice', pages=200)
Description: 'Python Basics' by Alice, containing 200 pages.
```

---

## Topic 6: Asynchronous Execution (`asyncio`)

### Definition

* **Asynchronous Programming:** A execution model using an event loop to handle concurrent task execution without multi-threading.
* **`async` / `await`:** Syntactic keywords used to define asynchronous coroutines (`async def`) and pause execution (`await`) until an asynchronous task completes.

### Explanation

Synchronous code executes tasks sequentially, blocking progress during operations like I/O or sleep calls. `asyncio` runs tasks concurrently on an event loop. Calling `await asyncio.sleep(1)` yields control back to the event loop, allowing other concurrent coroutines (e.g., via `asyncio.gather()`) to execute in the interim.

### Question and Answer

#### Q: What is the key difference between `time.sleep()` and `asyncio.sleep()`?

* **Answer:** `time.sleep()` blocks the entire execution thread, stopping all progress. `asyncio.sleep()` pauses only the specific coroutine non-blockingly, yielding control back to the event loop so other concurrent tasks can run.

### Program

```python
import asyncio

async def say_hello(name: str) -> None:
    print(f"Task {name}: Starting...")
    await asyncio.sleep(1)  # Non-blocking pause yielding control back to loop
    print(f"Task {name}: Completed!")

async def main() -> None:
    # Concurrent execution using asyncio.gather
    await asyncio.gather(say_hello("A"), say_hello("B"))

# --- Execution Demonstration ---
if __name__ == "__main__":
    asyncio.run(main())  # Initializes event loop and runs main coroutine
```

### Output

```text
Task A: Starting...
Task B: Starting...
Task A: Completed!
Task B: Completed!
```

---

## Topic 7: Object-Oriented Programming (OOP) & Encapsulation

### Definition

* **Object-Oriented Programming (OOP):** A programming paradigm organized around objects (data instances) rather than actions or logic loops. It models real-world entities using classes.
* **Encapsulation:** The practice of bundling data (attributes) and methods that operate on that data into a single unit (class), while restricting direct external access to some of the object's components.

### Explanation

Classes act as blueprints that define properties (attributes) and behaviors (methods). Objects are specific instances created from these blueprints.

To protect an object's internal state from unintended modification, encapsulation uses naming conventions to establish access boundaries:

* **Public Attributes:** Accessible from anywhere inside or outside the class (e.g., `self.name`).
* **Protected Attributes:** Indicated with a single underscore (e.g., `self._status`), signaling that the variable is intended for internal use and within subclasses.
* **Private Attributes:** Indicated with a double underscore prefix (e.g., `self.__balance`). This triggers a mechanism called **Name Mangling**, where the Python interpreter automatically rewrites the internal name to `_ClassName__variableName` to prevent direct external access or accidental overwriting.

Access to private variables is safely provided through controlled pathways called **Getters** and **Setters**, often declared elegantly using the built-in `@property` decorator.

### Question and Answer

#### Q1: What is the primary difference between a class and an object?

* **Answer:** A class is a structural blueprint or template that outlines the data attributes and functional methods available to a specific type of data structure. An object is a concrete, initialized instance of that class allocated in memory, containing actual values in place of the defined properties.

#### Q2: Is data truly private in Python when using double underscores?

* **Answer:** No, data privacy in Python is not strictly enforced at the hardware level. Double underscores trigger name mangling, which changes the attribute name from `__balance` to `_BankAccount__balance`. Bypassing this safety mechanism is still possible if an external script explicitly targets the mangled name, meaning it acts as a protection layer rather than a total restriction.

### Program: Secure Bank Account Management

```python
class BankAccount:
    def __init__(self, owner: str, initial_balance: float):
        self.owner: str = owner
        if initial_balance < 0:
            raise ValueError("Initial balance cannot be negative.")
        # Encapsulated private attribute to prevent unauthorized direct writes
        self.__balance: float = initial_balance

    # Getter property to safely read the encapsulated balance
    @property
    def balance(self) -> float:
        return self.__balance

    # Public method acting as a controlled gateway to modify state
    def deposit(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("Deposit amount must be greater than zero.")
        self.__balance += amount
        print(f"[SUCCESS] Deposited ${amount:.2f}. New Balance: ${self.__balance:.2f}")

    def withdraw(self, amount: float) -> None:
        if amount <= 0:
            print("[ERROR] Withdrawal amount must be positive.")
            return
        if amount > self.__balance:
            print("[ERROR] Insufficient funds available.")
            return
        self.__balance -= amount
        print(f"[SUCCESS] Withdrew ${amount:.2f}. Remaining Balance: ${self.__balance:.2f}")


# --- Execution Demonstration ---
if __name__ == "__main__":
    account = BankAccount("Alice Vance", 1000.00)
    print(f"Account Holder: {account.owner}")
    print(f"Verified Balance via Property: ${account.balance}")
    
    # Standard updates through the explicit interface
    account.deposit(250.50)
    account.withdraw(500.00)
    
    # Demonstrating Encapsulation Safety
    print("\n--- Attempting illegal direct state modification ---")
    try:
        # This will fail to access the internal variable due to name mangling
        print(account.__balance)
    except AttributeError as e:
        print(f"Caught Expected Exception: {e}")

```

### Output

```text
Account Holder: Alice Vance
Verified Balance via Property: $1000.0
[SUCCESS] Deposited $250.50. New Balance: $1250.50
[SUCCESS] Withdrew $500.00. Remaining Balance: $750.50

--- Attempting illegal direct state modification ---
Caught Expected Exception: 'BankAccount' object has no attribute '__balance'

```

---

## Topic 8: Inheritance & Polymorphism

### Definition

* **Inheritance:** A mechanism that allows a new class (subclass/child) to inherit attributes and methods from an existing class (superclass/parent), promoting code reusability.
* **Polymorphism:** The ability of different object classes to share the same method name but implement distinctly different behaviors under the hood.

### Explanation

Inheritance organizes classes hierarchically, allowing child classes to extend or override parental logic without completely duplicating the foundational code.

Polymorphism allows you to treat different objects uniformly. By defining an **Abstract Base Class (ABC)** with abstract methods, you establish a formal contract. Any child class inheriting from this ABC must implement these abstract methods. When you pass these different objects into the same function, Python dynamically runs each object's specific version of the method at runtime, avoiding the need for complex `if-elif-else` type-checking logic.

### Question and Answer

#### Q1: What is the benefit of using the `abc` module and the `@abstractmethod` decorator?

* **Answer:** It prevents developers from accidentally instantiating an incomplete base class directly. It also strictly enforces a common interface, triggering an explicit runtime error during initialization if a subclass fails to implement any of the designated abstract methods.

#### Q2: What is "Duck Typing" in Python polymorphism?

* **Answer:** Duck typing is Python’s approach to dynamic polymorphism, derived from the phrase *"If it walks like a duck and quacks like a duck, it's a duck."* It means Python checks for the presence of a specific method or attribute at runtime rather than verifying the object's strict class inheritance lineage.

### Program: Decoupled Notification Engine

```python
from abc import ABC, abstractmethod

# Abstract Parent Class enforcing code blueprint expectations
class NotificationChannel(ABC):
    @abstractmethod
    def send_message(self, recipient: str, body: str) -> bool:
        """Enforces uniform interface compliance across all child inheritance lines."""
        pass


# Child Subclass 1 (Inheritance)
class EmailChannel(NotificationChannel):
    def __init__(self, smtp_server: str):
        self.smtp_server = smtp_server

    # Polymorphic Method Overriding
    def send_message(self, recipient: str, body: str) -> bool:
        print(f"Connecting to SMTP Server [{self.smtp_server}]...")
        print(f"Dispatched Email to mailto:{recipient} -> Content: {body}")
        return True


# Child Subclass 2 (Inheritance)
class SMSChannel(NotificationChannel):
    def __init__(self, gateway_api_key: str):
        self.gateway_api_key = gateway_api_key

    # Polymorphic Method Overriding
    def send_message(self, recipient: str, body: str) -> bool:
        print(f"Authenticating SMS Gateway API via Key [{self.gateway_api_key[:4]}****]...")
        print(f"Dispatched SMS Alert to {recipient} -> Text: {body}")
        return True


# Polymorphic Client Function
def broadcast_security_alert(channel: NotificationChannel, user: str, alert_text: str):
    print(f"\nInitializing Security Pipeline for user: {user}")
    # Executes the correct method behavior depending on the object passed
    success = channel.send_message(recipient=user, body=alert_text)
    if success:
        print("Pipeline Tracking: Notification logged successfully.")


# --- Execution Demonstration ---
if __name__ == "__main__":
    email_service = EmailChannel(smtp_server="smtp.provider.com")
    sms_service = SMSChannel(gateway_api_key="ABC123XYZSECRETKEY")
    
    # Executing identical operations polymorphically across distinct instances
    broadcast_security_alert(email_service, "dev@company.com", "Unauthorized Login Detected!")
    broadcast_security_alert(sms_service, "+1-555-0199", "Your OTP verification code is: 4892")

```

### Output

```text
Initializing Security Pipeline for user: dev@company.com
Connecting to SMTP Server [smtp.provider.com]...
Dispatched Email to mailto:dev@company.com -> Content: Unauthorized Login Detected!
Pipeline Tracking: Notification logged successfully.

Initializing Security Pipeline for user: +1-555-0199
Authenticating SMS Gateway API via Key [ABC1****]...
Dispatched SMS Alert to +1-555-0199 -> Text: Your OTP verification code is: 4892
Pipeline Tracking: Notification logged successfully.

```

---

## Topic 9: Virtual Environments (`venv`)

### Definition

* **Virtual Environment (`venv`):** An isolated, self-contained directory tree that contains a localized Python interpreter installation along with its own independent set of third-party packages, preventing dependency version conflicts.

### Explanation

By default, installing third-party libraries globally puts them into a single directory used by your operating system. If Project A needs `requests==2.20` and Project B needs `requests==2.31`, a global environment cannot support both simultaneously.

The `venv` tool resolves this by generating an isolated directory layout inside your project directory. When activated, it adjusts your terminal’s path environment variables so that commands like `python` and `pip` run from the local environment folder instead of the global system paths.

### Question and Answer

#### Q1: What happens behind the scenes when a virtual environment is activated?

* **Answer:** The activation script updates your current terminal shell's environment variables by prepending the virtual environment's `bin/` (macOS/Linux) or `Scripts/` (Windows) directory to the front of the `$PATH` lookup order. This ensures that typing `python` calls the environment's isolated interpreter instance.

#### Q2: Should the virtual environment folder be committed to Git repositories?

* **Answer:** No, it should never be committed to source control. The folder contains compiled binaries specific to the architecture of the local host machine. Instead, add the virtual environment folder to your `.gitignore` file and track your dependencies using a `requirements.txt` or `pyproject.toml` configuration file.

### CLI Workflow Demonstration

```bash
# 1. Move into your project workspace directory
cd /workspace/my_project

# 2. Initialize a fresh, isolated local virtual environment directory named 'env'
python -m venv env

# 3. Activate the environment context based on your host operating system
# For macOS / Linux systems:
source env/bin/activate
# For Windows (PowerShell Core):
.\env\Scripts\Activate.ps1

# 4. Verify the active binary environment lookup address
which python

```

### Output

```text
# Output from a Linux/macOS path trace environment:
/workspace/my_project/env/bin/python

```

---

## Topic 10: Dependency Management (`pip`)

### Definition

* **`pip`:** The standard package management utility used to install, update, track, and uninstall external third-party software packages hosted on the Python Package Index (PyPI).

### Explanation

Managing applications in production requires consistent environments across different machines. Relying on unpinned package installations can introduce breaking changes when an external library updates.

Using explicit version pinning (e.g., `package==version`) preserves environment stability. The `pip freeze` command exports a snapshot of all active dependencies to a text file (typically named `requirements.txt`), allowing other developers or deployment pipelines to recreate the exact environment using `pip install -r requirements.txt`.

### Question and Answer

#### Q1: What is the difference between the outputs of `pip freeze` and `pip list`?

* **Answer:** `pip freeze` outputs installed packages in the exact `name==version` format required by configuration manifests, omitting pre-installed utility tools like `pip` and `setuptools`. `pip list` displays a human-readable table of all installed packages, including base system administration tooling.

#### Q2: What is a modern alternative to using `requirements.txt` files for enterprise software?

* **Answer:** Modern systems utilize a `pyproject.toml` file alongside dependency management tools like **Poetry** or **uv**. These managers generate cryptographic lock-files that verify file hashes, ensuring secure and deterministic environments across different operating systems.

### Practical Lifecycle Workflow

```bash
# Upgrade foundational package tooling inside the active virtual environment
pip install --upgrade pip

# Install specific versions of third-party dependencies securely
pip install requests==2.31.0

# Export the active dependency snapshot to a manifest configuration file
pip freeze > requirements.txt

```

#### Generated `requirements.txt` Content:

```text
certifi==2024.2.2
charset-normalizer==3.3.2
idna==3.6
requests==2.31.0
urllib3==2.2.1

```

### Program: Using Installed Dependencies

```python
import requests

def check_api_integrity(url: str) -> int | None:
    try:
        # Accessing the third-party requests library installed via pip
        response = requests.get(url, timeout=5)
        return response.status_code
    except requests.exceptions.RequestException as err:
        print(f"Network transport error: {err}")
        return None

if __name__ == "__main__":
    status = check_api_integrity("https://api.github.com")
    print(f"Target Server Integrity Status: {status}")

```

### Output

```text
Target Server Integrity Status: 200

```

---

## Topic 11: How are arguments passed in Python?

### Definition

* **Call-by-Object-Reference:** Python passes arguments by reference to the object, not by copying the value.

### Explanation

When passing a variable to a function, you pass a pointer to the existing object in memory. Modifying a mutable object (like a list) inside the function alters the original object. Reassigning the parameter inside the function only rebinds the local name and does not affect the caller's variable.

### Program

```python
def append_one(lst):
    lst.append(1)  # Mutates original object

def reassign(lst):
    lst = [99, 100]  # Rebinds local parameter reference

nums = [0]
append_one(nums)
print("After append_one:", nums)

nums = [0]
reassign(nums)
print("After reassign:", nums)
```

### Output

```text
After append_one: [0, 1]
After reassign: [0]
```

---

## Topic 12: Explain what Python decorators do.

### Definition

* **Decorator:** A function that takes another function as an argument, adds functionality, and returns a modified function without altering the original source code.

### Explanation

Decorators use the `@decorator_name` syntax placed above a function definition. They are commonly used for cross-cutting concerns like logging, timing, authentication, and caching.

### Program

```python
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"Function {func.__name__} executed in {time.time() - start:.4f}s")
        return result
    return wrapper

@timer
def compute():
    time.sleep(1)
    return "Done"

print("Result:", compute())
```

### Output

```text
Function compute executed in 0.1005s
Result: Done
```

---

## Topic 13: What does PEP 8 say about indentation?

### Definition

* **PEP 8:** The official Python Style Guide that defines conventions for writing readable Python code.

### Explanation

PEP 8 specifies using 4 spaces per indentation level. Spaces are strongly preferred over tabs, and mixing tabs and spaces in the same file is disallowed. Lines should ideally stay under 79 characters.

### Program

```python
def check_indentation(condition):
    # Level 1: 4 spaces
    if condition:
        # Level 2: 8 spaces
        print("Indented using 4 spaces per level.")

check_indentation(True)
```

### Output

```text
Indented using 4 spaces per level.
```

---

## Topic 14: Explain the difference between repr and str.

### Definition

* **`__str__`:** Returns a user-friendly, readable string representation of an object.
* **`__repr__`:** Returns an unambiguous, developer-focused string representation (ideally valid Python code).

### Explanation

`str()` is used by `print()` for presentation. `repr()` is used in debuggers and interactive shells. If `__str__` is not defined, Python falls back to `__repr__`.

### Program

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"{self.name}, {self.age} years old"

    def __repr__(self):
        return f"Person('{self.name}', {self.age})"

p = Person("Alice", 30)
print("str():", str(p))
print("repr():", repr(p))
```

### Output

```text
str(): Alice, 30 years old
repr(): Person('Alice', 30)
```

---

## Topic 15: Why is Python called an interpreted language?

### Definition

* **Interpreted Language:** A language whose source code is executed line-by-line at runtime rather than pre-compiled directly into machine target code.

### Explanation

Python source code `.py` is automatically compiled into intermediate bytecode `.pyc`. The Python Virtual Machine (PVM) then interprets this bytecode line-by-line.

### Program

```python
import dis

def add(a, b):
    return a + b

# Inspecting Python bytecode generated for the function
dis.dis(add)
```

### Output

```text
4           0 LOAD_FAST                0 (a)
            2 LOAD_FAST                1 (b)
            4 BINARY_ADD
            6 RETURN_VALUE
```

---

## Topic 16: What does the with statement do?

### Definition

* **Context Manager Statement:** A structure that encapsulates setup and cleanup logic around a block of code.

### Explanation

The `with` statement calls an object's `__enter__` method before running code and guarantees its `__exit__` method is called afterward, ensuring resources like files or locks are released even if exceptions occur.

### Program

```python
# Safe resource handling with context manager
with open("temp.txt", "w") as f:
    f.write("Hello, World!")

with open("temp.txt", "r") as f:
    print(f.read())
```

### Output

```text
Hello, World!
```

---

## Topic 17: Describe list and dictionary comprehensions.

### Definition

* **Comprehension:** A compact, inline expression for creating new collections from existing iterables.

### Explanation

Comprehensions replace verbose `for` loops with concise syntax. List comprehensions return lists `[expr for item in iterable]`. Dictionary comprehensions return key-value pairs `{k: v for item in iterable}`.

### Program

```python
# List comprehension
squares = [x**2 for x in range(5) if x % 2 == 0]

# Dictionary comprehension
names = ["Alice", "Bob"]
lengths = {name: len(name) for name in names}

print("Squares:", squares)
print("Lengths:", lengths)
```

### Output

```text
Squares: [0, 4, 16]
Lengths: {'Alice': 5, 'Bob': 3}
```

---

## Topic 18: What is the purpose of self in class methods?

### Definition

* **`self`:** An explicit parameter representing the specific instance of the class calling the method.

### Explanation

When an instance calls a method `obj.method()`, Python automatically passes `obj` as the first argument (`Class.method(obj)`). It provides access to instance attributes and other methods.

### Program

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        return f"{self.name} says woof!"

dog = Dog("Rex")
print(dog.bark())
print(Dog.bark(dog))  # Equivalent explicit call
```

### Output

```text
Rex says woof!
Rex says woof!
```

---

## Topic 19: What’s the difference between .py and .pyc files?

### Definition

* **`.py`:** Source code file containing human-readable Python code.
* **`.pyc`:** Compiled bytecode file generated by Python to speed up module import times.

### Explanation

When a `.py` file is imported, Python compiles it into bytecode `.pyc` and stores it in the `__pycache__` folder. On future runs, Python loads the `.pyc` directly if the source `.py` file has not changed.

### Program

```python
import py_compile
import os

# Compile this script logic into bytecode
compiled_path = py_compile.compile("temp.txt", cfile="temp.pyc")
print("Bytecode created:", os.path.exists(compiled_path))

# Clean up temporary file
if os.path.exists("temp.pyc"):
    os.remove("temp.pyc")
if os.path.exists("temp.txt"):
    os.remove("temp.txt")
```

### Output

```text
Bytecode created: True
```

---

## Topic 20: How would you safely handle missing keys in a dictionary?

### Definition

* **Safe Key Access:** Fetching dictionary values without raising a `KeyError` when a key does not exist.

### Explanation

Missing keys can be handled using `.get(key, default)`, `collections.defaultdict`, membership checks (`if key in dict`), or `try/except KeyError` blocks.

### Program

```python
from collections import defaultdict

data = {"a": 1}

# Method 1: .get()
val1 = data.get("b", 0)

# Method 2: defaultdict
d_dict = defaultdict(int, data)
val2 = d_dict["b"]

print("get():", val1)
print("defaultdict:", val2)
```

### Output

```text
get(): 0
defaultdict: 0
```

---

## Topic 21: What is the Global Interpreter Lock (GIL) and why does it matter?

### Definition

* **GIL:** A mutex lock in CPython that prevents multiple native threads from executing Python bytecode concurrently.

### Explanation

The GIL simplifies CPython memory management but prevents CPU-bound Python threads from running across multiple CPU cores in parallel. CPU-bound tasks require the `multiprocessing` module instead of `threading` to achieve true parallelism.

### Program

```python
import threading
import time

def cpu_task():
    count = 0
    for _ in range(5_000_000):
        count += 1

start = time.time()
t1 = threading.Thread(target=cpu_task)
t2 = threading.Thread(target=cpu_task)
t1.start(); t2.start()
t1.join(); t2.join()

print(f"Threaded time under GIL: {time.time() - start:.2f}s")
```

### Output

```text
Threaded time under GIL: 0.28s
```

---

## Topic 22: Explain shallow copy versus deep copy.

### Definition

* **Shallow Copy:** Creates a new outer collection, but inserts references to the nested objects of the original.
* **Deep Copy:** Recursively creates new copies of both outer containers and all nested objects.

### Explanation

Modifying nested mutable elements in a shallow copy alters the original object. A deep copy is completely independent.

### Program

```python
import copy

original = [1, [2, 3]]
shallow = copy.copy(original)
deep = copy.deepcopy(original)

original[1][0] = 'X'

print("Original:", original)
print("Shallow:", shallow)
print("Deep:", deep)
```

### Output

```text
Original: [1, ['X', 3]]
Shallow: [1, ['X', 3]]
Deep: [1, [2, 3]]
```

---

## Topic 23: Describe how range works in Python 3.

### Definition

* **`range` Object:** A memory-efficient sequence type that generates numbers on demand lazily.

### Explanation

Unlike Python 2's `range()` which created a full list, Python 3's `range` stores only start, stop, and step values. It calculates elements dynamically without holding all items in memory.

### Program

```python
r = range(1_000_000)

print("Type:", type(r))
print("Size in memory remains tiny:", r)
print("Access index 500:", r[500])
```

### Output

```text
Type: <class 'range'>
Size in memory remains tiny: range(0, 1000000)
Access index 500: 500
```

---

## Topic 24: Differentiate pass, continue, and break.

### Definition

* **`pass`:** A null statement placeholder that does nothing.
* **`continue`:** Skips the rest of the current loop iteration and moves to the next.
* **`break`:** Immediately exits the loop entirely.

### Explanation

`pass` fulfills syntax requirements. `continue` filters execution per item. `break` terminates loop execution upon reaching a condition.

### Program

```python
print("Loop execution:")
for i in range(5):
    if i == 1:
        pass  # Does nothing
    if i == 3:
        continue  # Skips 3
    if i == 4:
        break  # Exits loop
    print(i, end=" ")
```

### Output

```text
Loop execution:
0 1 2
```

---

## Topic 25: How do you use try/except/else/finally?

### Definition

* **Exception Block:** Structures error handling lifecycle into distinct execution phases.

### Explanation

* `try`: Code that might throw an error.
* `except`: Catches and handles specific exceptions.
* `else`: Executes only if **no** exceptions were raised.
* `finally`: **Always** executes regardless of exceptions (used for cleanup).

### Program

```python
def parse_int(val):
    try:
        res = int(val)
    except ValueError:
        print("Error: Invalid integer")
    else:
        print("Success: Value is", res)
    finally:
        print("Execution complete\n")

parse_int("10")
parse_int("abc")
```

### Output

```text
Success: Value is 10
Execution complete

Error: Invalid integer
Execution complete
```

---

## Topic 26: What are generators and how do they differ from normal functions?

### Definition

* **Generator:** A function returning an iterator that produces values lazily using the `yield` keyword.

### Explanation

Normal functions compute all results and return them at once via `return`. Generators pause execution state at `yield` and resume on the next call, saving memory.

### Program

```python
def count_down(n):
    while n > 0:
        yield n
        n -= 1

gen = count_down(3)
print(next(gen))
print(next(gen))
print(next(gen))
```

### Output

```text
3
2
1
```

---

## Topic 27: Explain lambda functions and closures.

### Definition

* **Lambda:** An anonymous single-expression function defined with `lambda`.
* **Closure:** An inner function that retains variables from its enclosing outer scope even after the outer scope closes.

### Explanation

Lambdas are used for short inline operations. Closures maintain persistent state without using global variables or explicit classes.

### Program

```python
# Lambda
square = lambda x: x ** 2

# Closure
def multiplier(factor):
    def multiply(number):
        return number * factor
    return multiply

double = multiplier(2)

print("Lambda Square:", square(4))
print("Closure Multiply:", double(5))
```

### Output

```text
Lambda Square: 16
Closure Multiply: 10
```

---

## Topic 28: How would you process an 8 GB text file to find the first non-repeating character?

### Definition

* **Chunk Streaming:** Processing large files line-by-line or in fixed chunks to prevent running out of memory.

### Explanation

Pass 1: Stream the file line-by-line and count character occurrences using `collections.Counter`.
Pass 2: Re-stream the file from the start and return the first character with a count of 1.

### Program

```python
from collections import Counter
import io

# Simulated 8 GB file contents in memory stream
file_data = "apple\napple pie\n"

# Pass 1: Count
counts = Counter()
for line in io.StringIO(file_data):
    counts.update(line)

# Pass 2: Find First Unique
first_unique = None
for line in io.StringIO(file_data):
    for char in line:
        if counts[char] == 1:
            first_unique = char
            break
    if first_unique:
        break

print("First Non-Repeating Character:", repr(first_unique))
```

### Output

```text
First Non-Repeating Character: 'l'
```

---

## Topic 29: Why should API keys be stored in environment variables? How do you access them?

### Definition

* **Environment Variables:** Dynamic OS-level variables stored outside the application source code.

### Explanation

Hardcoding keys risks exposing sensitive secrets in version control systems. Environment variables keep credentials isolated per environment and are accessed in Python via `os.getenv()`.

### Program

```python
import os

# Setting key in OS env for demo
os.environ["API_KEY"] = "secret_token_123"

# Accessing key
api_key = os.getenv("API_KEY", "default_fallback")
print("Retrieved Key:", api_key)
```

### Output

```text
Retrieved Key: secret_token_123
```

---

## Topic 30: How do you sort a dictionary by its values?

### Definition

* **Value Sorting:** Ordering dictionary key-value items based on values rather than keys using `sorted()`.

### Explanation

Pass `dict.items()` to `sorted()` with a custom `key` lambda function selecting the second element (`item[1]`). Convert the output back to a `dict`.

### Program

```python
scores = {'Alice': 90, 'Bob': 75, 'Charlie': 95}

# Sort ascending by value
sorted_dict = dict(sorted(scores.items(), key=lambda item: item[1]))

print("Sorted Dict:", sorted_dict)
```

### Output

```text
Sorted Dict: {'Bob': 75, 'Alice': 90, 'Charlie': 95}
```

---

## Topic 31: Explain the difference between is and ==.

### Definition

* **`==`:** Equality operator checking if values of two objects are equivalent.
* **`is`:** Identity operator checking if two variables refer to the exact same memory address (`id(a) == id(b)`).

### Explanation

Two distinct objects can hold identical values (`==` returns `True`), but occupy different memory locations (`is` returns `False`).

### Program

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print("a == b:", a == b)
print("a is b:", a is b)
print("a is c:", a is c)
```

### Output

```text
a == b: True
a is b: False
a is c: True
```

---

## Topic 32: How do you check if two strings are anagrams?

### Definition

* **Anagram Check:** Determining if two strings contain identical character counts.

### Explanation

Method 1: Sort characters and compare equality ($O(n \log n)$).
Method 2: Use `collections.Counter` to compare character frequencies ($O(n)$).

### Program

```python
from collections import Counter

def is_anagram(s1, s2):
    c1 = s1.replace(" ", "").lower()
    c2 = s2.replace(" ", "").lower()
    return Counter(c1) == Counter(c2)

print("listen & silent:", is_anagram("listen", "silent"))
print("hello & world:", is_anagram("hello", "world"))
```

### Output

```text
listen & silent: True
hello & world: False
```

---

## Topic 33: What is Method Resolution Order (MRO) in multiple inheritance?

### Definition

* **MRO:** The linear path Python traverses to look up methods and attributes across parent classes.

### Explanation

Python uses the C3 Linearization algorithm to determine method resolution in complex multiple inheritance structures. 
Inspect MRO using `Class.mro()` or `Class.__mro__`.

### Program

```python
class A: pass
class B(A): pass
class C(A): pass
class D(B, C): pass

print("D MRO:", [cls.__name__ for cls in D.mro()])
```

### Output

```text
D MRO: ['D', 'B', 'C', 'A', 'object']
```

---

## Topic 34: What are magic methods? Give examples.

### Definition

* **Magic Methods:** Built-in methods surrounded by double underscores (`__method__`) that define custom class behavior for built-in operations.

### Explanation

They allow custom objects to integrate with Python operators (`+`, `==`), built-in functions (`len()`, `str()`), and iteration mechanics (`for` loops).

### Program

```python
class CustomList:
    def __init__(self, items):
        self.items = items

    def __len__(self):
        return len(self.items)

    def __getitem__(self, idx):
        return self.items[idx]

c = CustomList([10, 20, 30])
print("len():", len(c))
print("Indexing c[1]:", c[1])
```

### Output

```text
len(): 3
Indexing c[1]: 20
```

---

## Topic 35: How do you write your own context manager?

### Definition

* **Custom Context Manager:** An object that handles resource allocation and cleanup using `__enter__` and `__exit__` methods, or via the `@contextmanager` generator decorator.

### Explanation

`__enter__` sets up the resource. `__exit__` cleans up resources even if an unhandled exception occurs.

### Program

```python
from contextlib import contextmanager

# Generator approach
@contextmanager
def managed_resource():
    print("Resource Setup")
    try:
        yield "Active Resource"
    finally:
        print("Resource Cleanup")

with managed_resource() as res:
    print("Using:", res)
```

### Output

```text
Resource Setup
Using: Active Resource
Resource Cleanup
```

---

## Topic 36: Describe asynchronous programming in Python using async/await.

### Definition

* **Asynchronous Programming:** Concurrent execution strategy using an event loop to handle non-blocking I/O operations.

### Explanation

`async def` declares a coroutine. `await` pauses coroutine execution, freeing the event loop to execute other tasks while waiting for I/O.

### Program

```python
import asyncio

async def fetch(id):
    await asyncio.sleep(0.01)  # Simulate non-blocking I/O
    return f"Data {id}"

async def main():
    results = await asyncio.gather(fetch(1), fetch(2))
    print("Results:", results)

asyncio.run(main())
```

### Output

```text
Results: ['Data 1', 'Data 2']
```

---

## Topic 37: What is pickling? How do you customize object serialization?

### Definition

* **Pickling:** The process of serializing Python objects into byte streams for storage or transmission.

### Explanation

Serialization is handled using `pickle.dumps()` and `pickle.loads()`. Custom serialization logic is defined by overriding the `__getstate__()` and `__setstate__()` magic methods.

### Program

```python
import pickle

class User:
    def __init__(self, name, secret):
        self.name = name
        self.secret = secret

    def __getstate__(self):
        state = self.__dict__.copy()
        del state['secret']  # Exclude secret from pickle
        return state

    def __setstate__(self, state):
        self.__dict__.update(state)
        self.secret = "default_reset"

u = User("Alice", "pass123")
serialized = pickle.dumps(u)
restored = pickle.loads(serialized)

print("Restored Name:", restored.name)
print("Restored Secret:", restored.secret)
```

### Output

```text
Restored Name: Alice
Restored Secret: default_reset
```

## Programs

## Topic 1: String Palindrome Validation

### Definition

* **Palindrome:** A sequence of characters (word, phrase, or number) that reads the same backward as forward.
* **Extended Slicing:** A Python syntax mechanism `[start:stop:step]` that enables sequence extraction and reversal using negative step values.

### Program

```python
def is_palindrome(text: str) -> bool:
    # Normalize input by lowering case to handle mixed-case palindromes reliably
    cleaned_text = text.lower()
    return cleaned_text == cleaned_text[::-1]

# --- Execution Demonstration ---
if __name__ == "__main__":
    word_1 = "Madam"
    word_2 = "Python"

    print(f"Is '{word_1}' a palindrome? {is_palindrome(word_1)}")
    print(f"Is '{word_2}' a palindrome? {is_palindrome(word_2)}")
```

### Output

```text
Is 'Madam' a palindrome? True
Is 'Python' a palindrome? False
```

---

## Topic 2: Factorial Calculation via Recursion

### Definition

* **Factorial ($n!$):** The product of all positive integers less than or equal to a non-negative integer $n$. By mathematical convention, $0! = 1$.
* **Recursion:** A programming pattern where a function calls itself to break down a problem into smaller sub-problems until reaching a base termination case.

### Program

```python
def factorial(n: int) -> int:
    if n < 0:
        raise ValueError("Factorial is not defined for negative integers.")
    if n == 0:
        return 1  # Base Case
    return n * factorial(n - 1)  # Recursive Step

# --- Execution Demonstration ---
if __name__ == "__main__":
    number = 5
    result = factorial(number)
    print(f"The factorial of {number} is: {result}")
```

### Output

```text
The factorial of 5 is: 120
```

---

## Topic 3: Maximum Element Traversal

### Definition

* **Linear Scan:** An algorithm that iterates through a collection sequentially from start to end, maintaining a running state of interest (such as the highest value).

### Program

```python
def find_largest(numbers: list[int | float]) -> int | float:
    if not numbers:
        raise ValueError("Cannot find the largest element in an empty list.")

    largest = numbers[0]
    for num in numbers:
        if num > largest:
            largest = num
    return largest

# --- Execution Demonstration ---
if __name__ == "__main__":
    nums = [10, 5, 8, 20, 3]
    largest_num = find_largest(nums)
    print(f"The largest number in the list is: {largest_num}")
```

### Output

```text
The largest number in the list is: 20
```

---

## Topic 4: String Reversal Mechanics

### Definition

* **Immutability:** A property of data types (such as `str`, `tuple`, `int`) preventing them from being modified in place after instantiation.

### Program

```python
def reverse_string(text: str) -> str:
    return text[::-1]

# --- Execution Demonstration ---
if __name__ == "__main__":
    original_text = "Hello, World!"
    reversed_text = reverse_string(original_text)
    reversed_string = join(reversed(original_text))
    print(f"Original: {original_text}")
    print(f"Reversed: {reversed_text}")
    print(f"Reversed function: {reversed_string}")
```

### Output

```text
Original: Hello, World!
Reversed: !dlroW ,olleH
```

---

## Topic 5: Dictionary Frequency Counting

### Definition

* **Hash Map / Dictionary:** A mutable data structure that maps unique keys to values, offering efficient $O(1)$ average time complexity for lookups, insertions, and updates.

*Note* Python provides `collections.Counter`, a dict subclass designed for counting hashable objects. Passing a list to `Counter(numbers)` generates the frequency dictionary automatically.

### Program

```python
def count_frequency(items: list) -> dict:
    frequency = {}
    for item in items:
        if item in frequency:
            frequency[item] += 1
        else:
            frequency[item] = 1
    return frequency


# --- Execution Demonstration ---
if __name__ == "__main__":
    nums = [1, 2, 3, 2, 1, 3, 2, 4, 5, 4]
    frequency_count = count_frequency(nums)
    print(f"Element Frequencies: {frequency_count}")
```

### Output

```text
Element Frequencies: {1: 2, 2: 3, 3: 2, 4: 2, 5: 1}
```

---

## Topic 6: Prime Number Primality Test

### Definition

* **Prime Number:** A natural number greater than 1 that has no positive divisors other than 1 and itself.
* **Trial Division Optimization:** An algorithmic optimization that tests divisibility only up to $\sqrt{n}$.

### Program

```python
def is_prime(number: int) -> bool:
    if number < 2:
        return False
    # Check divisibility up to square root of number
    for i in range(2, int(number**0.5) + 1):
        if number % i == 0:
            return False  # Factor found; not prime
    return True

# --- Execution Demonstration ---
if __name__ == "__main__":
    test_number = 17
    if is_prime(test_number):
        print(f"{test_number} is a prime number.")
    else:
        print(f"{test_number} is NOT a prime number.")
```

### Output

```text
17 is a prime number.
```

---

## Topic 7: Set-Based Collection Intersection

### Definition

* **Set Intersection:** A set theory operation that returns a new collection containing only elements shared by two or more sets.

### Program

```python
def find_common_elements(list1: list, list2: list) -> list:
    # Converting lists to sets provides O(1) average lookup performance
    set_a = set(list1)
    set_b = set(list2)
    # The '&' operator computes set intersection
    return list(set_a & set_b)

# --- Execution Demonstration ---
if __name__ == "__main__":
    list_a = [1, 2, 3, 4, 5]
    list_b = [4, 5, 6, 7, 8]
    common = find_common_elements(list_a, list_b)
    print(f"Common Elements: {common}")
```

### Output

```text
Common Elements: [4, 5]
```

---

## Topic 8: In-Place Bubble Sort Algorithm

### Definition

* **Bubble Sort:** A simple comparison-based sorting algorithm that repeatedly steps through a list, compares adjacent elements, and swaps them if they are in the wrong order.

### Program

```python
def bubble_sort(elements: list[int | float]) -> None:
    n = len(elements)
    for i in range(n - 1):
        swapped = False
        for j in range(n - i - 1):
            if elements[j] > elements[j + 1]:
                # In-place Pythonic variable swap
                elements[j], elements[j + 1] = elements[j + 1], elements[j]
                swapped = True
        # If no swaps occurred during this pass, the array is already sorted
        if not swapped:
            break

# --- Execution Demonstration ---
if __name__ == "__main__":
    nums = [5, 2, 8, 1, 9]
    print(f"Before Sorting: {nums}")
    bubble_sort(nums)
    print(f"After Sorting:  {nums}")
```

### Output

```text
Before Sorting: [5, 2, 8, 1, 9]
After Sorting:  [1, 2, 5, 8, 9]
```

---

## Topic 9: Single-Pass Second Maximum Search

### Definition

* **Single-Pass Tracking:** An algorithmic technique that finds statistical metrics (like the largest and second largest elements) in a single $O(n)$ iteration through a dataset.

### Program

```python
def find_second_largest(numbers: list[int | float]) -> int | float | None:
    if len(numbers) < 2:
        return None

    largest = float('-inf')
    second_largest = float('-inf')

    for num in numbers:
        if num > largest:
            second_largest = largest
            largest = num
        elif num > second_largest and num != largest:
            second_largest = num

    # Return None if no distinct second largest element exists (e.g., [10, 10, 10])
    return second_largest if second_largest != float('-inf') else None


# --- Execution Demonstration ---
if __name__ == "__main__":
    nums = [10, 5, 8, 20, 3]
    second_largest_num = find_second_largest(nums)
    print(f"The second largest number is: {second_largest_num}")
```

### Output

```text
The second largest number is: 10
```

---

## Topic 10: Sequence Deduplication

### Definition

* **Deduplication:** The process of filtering out duplicate entries from a data structure, leaving only unique values.

### Program

```python
def remove_duplicates_preserve_order(numbers: list) -> list:
    # Dict keys are unique and preserve insertion order in modern Python
    return list(dict.fromkeys(numbers))

# --- Execution Demonstration ---
if __name__ == "__main__":
    nums = [1, 2, 3, 2, 1, 3, 2, 4, 5, 4]
    unique_nums = remove_duplicates_preserve_order(nums)
    print(f"Original List:  {nums}")
    print(f"Deduplicated:   {unique_nums}")
```

### Output

```text
Original List:  [1, 2, 3, 2, 1, 3, 2, 4, 5, 4]
Deduplicated:   [1, 2, 3, 4, 5]
```

## Topic 11: Show ways to get every third item from a list.

### Definition

* **Step Slicing:** Extracting elements from a sequence at fixed intervals using `[start:stop:step]` notation.

### Explanation

The slice `[::3]` starts at index 0, runs to the end of the list, and steps by 3. Alternative methods include list comprehensions with `enumerate()` and standard `for` loops using `range()`.

### Program

```python
items = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Method 1: Slicing
res_slice = items[::3]

# Method 2: List Comprehension
res_comp = [item for idx, item in enumerate(items) if idx % 3 == 0]

# Method 3: For Loop
res_loop = []
for i in range(0, len(items), 3):
    res_loop.append(items[i])

print("Slice:", res_slice)
print("Comprehension:", res_comp)
print("Loop:", res_loop)
```

### Output

```text
Slice: [0, 3, 6, 9]
Comprehension: [0, 3, 6, 9]
Loop: [0, 3, 6, 9]
```

## Topic 12: Sorting & Array Modification Algorithms

### Core Summary

A walkthrough analyzing fundamental array mechanics, comparing explicit multi-paradigm solutions to reverse linear lists along with historical manual sorting tracing.

### Five Methods to Reverse an Array List

#### #1 In-Place Array List Mutation Method (`.reverse()`)

```python
my_list = [1, 2, 3, 4, 5]
my_list.reverse()
print(my_list) # Output: [5, 4, 3, 2, 1]

```

#### #2 Step Slicing Syntax (`[::-1]`)

```python
my_list = [1, 2, 3, 4, 5]
reversed_list = my_list[::-1]
print(reversed_list) # Output: [5, 4, 3, 2, 1]

```

#### #3 Functional Generation Engine Wrappers (`reversed()`)

```python
my_list = [1, 2, 3, 4, 5]
reversed_list = list(reversed(my_list))
print(reversed_list) # Output: [5, 4, 3, 2, 1]

```

#### #4 Iterative Insertion Traversal Loops

```python
my_l = [1, 2, 3, 4, 5]
r_list = []
for i in my_l:
    r_list.insert(0, i)
print(r_list) # Output: [5, 4, 3, 2, 1]

```

#### #5 Decrementing Range Index Pointer Tracking

```python
my_list = [1, 2, 3, 4, 5]
reversed_list = []
for i in range(len(my_list) - 1, -1, -1):
    reversed_list.append(my_list[i])
print(reversed_list) # Output: [5, 4, 3, 2, 1]

```

---

#### #6 In-Place Exchange Sort Manual Implementation

```python
len_my_list = int(input("Enter length of list: "))
my_list = []

for k in range(len_my_list):
    element = int(input(f"Enter {k+1} number: "))
    my_list.append(element)

print(f"Before sorting: {my_list}")

# In-place reference index array value manipulation loop
for i in range(0, len(my_list)):
    for j in range(0, len(my_list)):
        if my_list[i] < my_list[j]:
            # Pythonic Multi-Assignment swap paradigm comparison:
            # Modern production style: 
            # my_list[i], my_list[j] = my_list[j], my_list[i]
            # Historical learning structural swap logic:
            tem = my_list[i]
            my_list[i] = my_list[j]
            my_list[j] = tem

print(f"After sorting: {my_list}")

```

#### Example Output Execution Trace:

```text
Enter length of list: 3
Enter 1 number: 69
Enter 2 number: 96
Enter 3 number: 86
Before sorting: [69, 96, 86]
After sorting: [69, 86, 96]

```

---

## Topic 13: Console Pattern Printing Exercises

### Core Summary

A collection of structural logic exercises designed to master console output layouts, coordinate constraints, step increment loops, and multi-layered string index slicing.

### Alphanumeric String Slicing Mirror Pyramid

#### Source Code:

```python
my_str = "Python"
x = 0

# Ascending Slicing Iteration Step
for i in my_str:
    x += 1
    print(my_str[0:x])

# Descending Slicing Iteration Step
for i in my_str:
    x -= 1
    print(my_str[0:x])

```

#### Generated Console Layout:

```text
P
Py
Pyt
Pyth
Pytho
Python
Pytho
Pyth
Pyt
Py
P

```

---

### Top 4 Structural Coordinate Star Glyphs

#### Pattern 1: Mathematical Console Heart Layout

```python
for i in range(6):
    for j in range(7):
        if (i == 0 and j % 3 != 0) or \
           (i == 1 and j % 3 == 0) or \
           (i - j == 2) or (i + j == 8):
            print("*", end=" ")
        else:
            print(" ", end=" ")
    print()

```

```text
  * *   * *  
*     *     *
*           *
  *       *  
    *   *    
      *
```

#### Pattern 2: Empty Grid Square Frame Boundary Block
```python
n = 5
for i in range(n):
    for j in range(n):
        if i == 0 or i == n-1 or j == 0 or j == n-1:
            print("*", end=" ")
        else:
            print(" ", end=" ")
    print()

```

```text
* * * * *
*       *
*       *
*       *
* * * * *
```

#### Pattern 3: Asymmetric Centered Triangle Pyramid
```python
n = 5
for i in range(n):
    for j in range(n - i - 1):
        print(" ", end=" ")
    for k in range(2 * i + 1):
        print("*", end=" ")
    print()

```

```text
        *
      * * *
    * * * * *
  * * * * * * *
* * * * * * * * *
```

#### Pattern 4: Linear Step String Multiplier Right Triangle
```python
n = 5
for i in range(1, n + 1):
    print("* " * i)

```

```text
* 
* * 
* * * 
* * * * 
* * * * *
```

---