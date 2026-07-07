- Data types (int, float, str, list, dict).
- Practice loops (for, while) and conditionals (if/else).
- Define functions (def).
- Learn scope (local vs global variables).
- Import and use modules (math, random).
- Object-Oriented Programming (OOP) - Classes, objects, attributes, methods.
- Inheritance and polymorphism.
- Learn venv for isolated environments.
- Install packages with pip (e.g., requests, numpy).
- Understand project structure.

Numeric Types
- int → Whole numbers
- float → Decimal number
- complex → Numbers with real and imaginary parts

Sequence Types
- list → Ordered, mutable collection
- tuple → Ordered, immutable collection
- range → Sequence of numbers

Mapping Type
- dict → Key-value pairs

Boolean Type
- bool → True or False

Set Types
- set → Unordered collection of unique items
- frozenset → Immutable set

Binary Types
- bytes → Immutable sequence of bytes
- bytearray → Mutable sequence of bytes
- memoryview → View of memory buffe

Special Type
- NoneType → Represents "no value"

## 🔄 Mutable vs Immutable
- **Mutable** → Can be changed after creation (e.g., list, dict, set, bytearray).  
- **Immutable** → Cannot be changed after creation (e.g., tuple, str, frozenset, bytes).

---

## 📚 Sequence Types

### List (Mutable)
```python
fruits = ["apple", "banana", "cherry"]
print("Original:", fruits)

fruits[1] = "mango"   # Modify element
fruits.append("grape") # Add new element
print("Modified:", fruits)
```
**Output:**
```
Original: ['apple', 'banana', 'cherry']
Modified: ['apple', 'mango', 'cherry', 'grape']
```

---

### Tuple (Immutable)
```python
coordinates = (10, 20, 30)
print("Original:", coordinates)

# Attempt to modify
try:
    coordinates[1] = 50
except TypeError as e:
    print("Error:", e)
```
**Output:**
```
Original: (10, 20, 30)
Error: 'tuple' object does not support item assignment
```

---

### Range
```python
numbers = range(5)
print("Range values:", list(numbers))
```
**Output:**
```
Range values: [0, 1, 2, 3, 4]
```

---

## 🗂️ Mapping Type

### Dictionary (Mutable)
```python
student = {"name": "John", "age": 25}
print("Original:", student)

student["age"] = 26   # Modify value
student["grade"] = "A" # Add new key-value
print("Modified:", student)
```
**Output:**
```
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
```
Boolean value: True
The system is active.
```

---

## 🔗 Set Types

### Set (Mutable)
```python
colors = {"red", "green", "blue"}
print("Original:", colors)

colors.add("yellow")   # Add element
colors.remove("green") # Remove element
print("Modified:", colors)
```
**Output:**
```
Original: {'red', 'green', 'blue'}
Modified: {'red', 'blue', 'yellow'}
```

---

### Frozenset (Immutable)
```python
frozen_colors = frozenset(["red", "green", "blue"])
print("Frozen set:", frozen_colors)

# Attempt to modify
try:
    frozen_colors.add("yellow")
except AttributeError as e:
    print("Error:", e)
```
**Output:**
```
Frozen set: frozenset({'red', 'green', 'blue'})
Error: 'frozenset' object has no attribute 'add'
```

---

## 💾 Binary Types

### Bytes (Immutable)
```python
b = b"Hello"
print("Bytes:", b)

try:
    b[0] = 65
except TypeError as e:
    print("Error:", e)
```
**Output:**
```
Bytes: b'Hello'
Error: 'bytes' object does not support item assignment
```

---

### Bytearray (Mutable)
```python
ba = bytearray([65, 66, 67]) # A, B, C
print("Original:", ba)

ba[1] = 90  # Change B → Z
print("Modified:", ba)
```
**Output:**
```
Original: bytearray(b'ABC')
Modified: bytearray(b'AZC')
```

---

### Memoryview
```python
mv = memoryview(b"Python")
print("Memoryview:", mv[0])  # ASCII of 'P'

# Convert back to bytes
print("As bytes:", mv.tobytes())
```
**Output:**
```
Memoryview: 80
As bytes: b'Python'
```

---

## 📝 Special Type

### NoneType
```python
x = None
print("Value:", x)

if x is None:
    print("x has no value assigned.")
```
**Output:**
```
Value: None
x has no value assigned.
```

---
