# Advanced JavaScript & ES6+ Guide

---

# Part 1 — Advanced JavaScript: Inheritance & Prototype Chain

## 1. Prototype — The Foundation

### Definition

A prototype is an object from which other objects inherit properties and methods directly.

### Explanation

JavaScript relies on prototype-based inheritance rather than class-based inheritance found in languages like Java or C++. Every object in JavaScript maintains an internal reference link to another object called its prototype.

### Example

**Legacy Approach (ES5 Prototypes)**

```javascript
var user = {
    name: "John"
};

console.log(user.toString());

```

**Modern Approach (ES6+ Object APIs)**

```javascript
const user = {
    name: "John"
};

console.log(user.toString());

```

### Output

```text
[object Object]

```

### Workflow

1. JavaScript attempts to access `user.toString()`.
2. It checks if `toString` exists directly on `user`.
3. Not finding it on `user`, it follows the internal prototype link to `Object.prototype`.
4. It finds `toString` on `Object.prototype` and executes it.

### Architecture

```text
user
 │
 ▼
Object.prototype
 │
 ├── toString()
 ├── hasOwnProperty()
 ├── isPrototypeOf()
 └── ...

```

### Points

* JavaScript objects inherit directly from other objects dynamically at runtime.
* Property lookups travel up the prototype chain until the property is found or `null` is reached.

---

## 2. Prototype Chain

### Definition

The prototype chain is the series of linked objects that JavaScript traverses to resolve property or method lookups.

### Explanation

When accessing a property, JavaScript first searches the object's own properties. If absent, it queries the prototype, then the prototype's prototype, continuing until reaching `Object.prototype`, whose prototype is `null`.

### Example

```javascript
const user = {
    name: "John"
};

console.log(user.name);
console.log(user.toString());

```

### Output

```text
John
[object Object]

```

### Workflow

```text
user.property
     │
     ▼
Is property on user?
     │
   No ──► (If Yes: return property)
     │
     ▼
Search user.[[Prototype]]
     │
     ▼
Object.prototype
     │
     ▼
Found?
     │
   No ──► (If Yes: return property)
     │
     ▼
null
     │
     ▼
undefined

```

### Architecture

```text
                  user
                   │
                   │ [[Prototype]]
                   ▼
             Object.prototype
                   │
                   │ [[Prototype]]
                   ▼
                  null

```

### Points

* Traversing a long prototype chain can impact performance for missing properties.
* Calling an `undefined` property as a function triggers a `TypeError`.

---

## 3. Internal Prototype `[[Prototype]]` & Inspection

### Definition

`[[Prototype]]` is the internal hidden property of an object pointing to its prototype instance.

### Explanation

JavaScript exposes APIs to inspect and manipulate this hidden internal reference safely. Modern code uses explicit methods rather than legacy property accessors.

### Example

**Legacy Code (`__proto__`)**

```javascript
var user = { name: "John" };
console.log(user.__proto__ === Object.prototype);

```

**Modern Code (`Object.getPrototypeOf`)**

```javascript
const user = { name: "John" };
console.log(Object.getPrototypeOf(user) === Object.prototype);

```

### Output

```text
true

```

### Workflow

1. Pass object to `Object.getPrototypeOf(obj)`.
2. Engine extracts internal `[[Prototype]]` reference.
3. Returns prototype object or `null`.

### Points

* `__proto__` is a legacy accessor property on `Object.prototype`; avoid using it in modern production code.
* Use `Object.getPrototypeOf()` for reading and `Object.setPrototypeOf()` for writing (though mutating prototypes causes performance degradation in V8 engine optimization paths).

---

## 4. Constructor Functions & Prototype Sharing

### Definition

Constructor functions serve as blueprints for instantiating multiple objects sharing properties and prototype methods.

### Explanation

Attaching methods directly inside a constructor function duplicates methods in memory for every instance. Placing methods on the constructor's `prototype` ensures a single shared memory reference.

### Example

**Legacy / Inefficient (Method Duplication)**

```javascript
function User(name) {
    this.name = name;
    this.greet = function() {
        console.log("Hello " + this.name);
    };
}

```

**Modern / Optimized (Prototype Method Sharing)**

```javascript
function User(name, age) {
    this.name = name;
    this.age = age;
}

User.prototype.greet = function() {
    console.log("Hello " + this.name);
};

const user1 = new User("John", 30);
const user2 = new User("Mary", 25);

user1.greet();

```

### Output

```text
Hello John

```

### Architecture

```text
user1 ──┐
user2 ──┤
user3 ──┼──► User.prototype.greet
...   ──┤
userN ──┘

```

### Points

* Adding methods to `.prototype` saves substantial memory when creating large quantities of instances.
* Arrow functions should not be used as constructor functions as they lack a `prototype` property and cannot bind `this`.

---

## 5. The `new` Keyword Execution Engine

### Definition

The `new` operator instantiates an object from a constructor function or class by setting up binding and prototype linkages.

### Explanation

Under the hood, `new` automates four fundamental object creation steps.

### Example

**Using `new` Operator**

```javascript
function User(name) {
    this.name = name;
}
const user = new User("John");

```

**Conceptual Polyfill / Equivalent Code**

```javascript
function createInstance(Constructor, ...args) {
    // 1. Create fresh object linked to constructor's prototype
    const instance = Object.create(Constructor.prototype);
    
    // 2. Execute constructor with `this` set to instance
    const result = Constructor.apply(instance, args);
    
    // 3. Return explicit object if returned, otherwise return created instance
    return (typeof result === "object" && result !== null) ? result : instance;
}

const user2 = createInstance(User, "John");

```

### Output

Both produce identical object structures containing `{ name: "John" }`.

### Workflow

```text
new User("John")
       │
       ▼
1. Create a fresh empty object {}
       │
       ▼
2. Link object's [[Prototype]] to Constructor.prototype
       │
       ▼
3. Call Constructor with `this` bound to new object
       │
       ▼
4. If constructor returns an object, return that object; 
   otherwise return the newly created instance.

```

### Points

* Returning a primitive (like a string or number) from a constructor function is ignored; `new` still returns the instance.
* Returning an explicit object/function from a constructor overrides the `new` mechanics and returns that object instead.

---

## 6. `Object.create()`

### Definition

`Object.create()` creates a new object using an existing object directly as its prototype without invoking a constructor function.

### Explanation

It provides clean prototypical inheritance without execution overhead or constructor initializations.

### Example

```javascript
const userPrototype = {
    greet() {
        console.log("Hello " + this.name);
    }
};

const user = Object.create(userPrototype);
user.name = "John";
user.greet();

```

### Output

```text
Hello John

```

### Comparison: `new` vs `Object.create()`

* **`new` Operator**: Creates object -> Links prototype -> Invokes constructor function -> Returns instance.
* **`Object.create()`**: Creates object -> Links prototype -> Returns instance (No constructor execution).

### Architecture

```text
user
 │
 ▼
userPrototype
 │
 └── greet()

```

### Points

* `Object.create(null)` creates a dictionary object with no prototype chain, making it completely clean of default `Object.prototype` methods (like `toString` or `hasOwnProperty`).

---

## 7. `Function.prototype` vs `User.prototype` vs `[[Prototype]]`

### Definition

* `Function.prototype`: The prototype inherited by all function objects in JavaScript.
* `User.prototype`: The object assigned as `[[Prototype]]` to instances created via `new User()`.
* `[[Prototype]]`: The actual internal prototype pointer of any given object.

### Explanation

Functions in JavaScript are first-class objects. Therefore, function objects have their own internal `[[Prototype]]` pointing to `Function.prototype`, while simultaneously carrying a `.prototype` property for instances they construct.

### Example

```javascript
function User() {}
const user = new User();

console.log(Object.getPrototypeOf(User) === Function.prototype);
console.log(Object.getPrototypeOf(user) === User.prototype);

```

### Output

```text
true
true

```

### Architecture

```text
                 User (Function Object)
                  │
       ┌──────────┴──────────┐
       │                     │
       ▼                     ▼
Function.prototype       User.prototype
       ▲                     ▲
       │                     │
  (User Object)        (user Instance)
                            │
                            ▼
                       user = new User()

```

### Points

* `Object.getPrototypeOf(User)` points to `Function.prototype`.
* `Object.getPrototypeOf(user)` points to `User.prototype`.

---

## 8. Property Checking: `hasOwnProperty()` vs `Object.hasOwn()`

### Definition

`hasOwnProperty()` and `Object.hasOwn()` determine if a specified property exists directly on an object rather than through prototype inheritance.

### Explanation

Direct property access via `.hasOwnProperty()` can fail if an object overrides the method or is created with `Object.create(null)`. `Object.hasOwn()` is the safer, modern ES2022 static replacement.

### Example

**Legacy/Unsafe Check**

```javascript
const obj = { hasOwnProperty: () => false, name: "John" };
console.log(obj.hasOwnProperty("name")); // Fails due to shadowing

```

**Modern/Safe Check**

```javascript
const obj = { hasOwnProperty: () => false, name: "John" };
console.log(Object.hasOwn(obj, "name")); // Modern standard

```

### Output

```text
false
true

```

### Points

* `Object.hasOwn(obj, prop)` is resilient against objects with overridden `hasOwnProperty` methods or prototype-less objects created via `Object.create(null)`.

---

## 9. Type Checking: `instanceof` vs `typeof`

### Definition

* `typeof`: Unary operator returning a string indicating the primitive data type or "object"/"function".
* `instanceof`: Operator testing whether a constructor's `.prototype` property appears anywhere along an object's prototype chain.

### Explanation

`typeof` is best suited for primitives. `instanceof` evaluates prototype relationships for complex objects.

### Example

```javascript
function User() {}
const user = new User();

console.log(typeof user);
console.log(user instanceof User);

```

### Output

```text
object
true

```

### Workflow

`instanceof` evaluation for `user instanceof User`:

1. Get prototype of `user` via `Object.getPrototypeOf(user)`.
2. Compare with `User.prototype`.
3. If matched, return `true`.
4. If not matched, move up to next prototype until reaching `null` (returns `false`).

### Points

* `instanceof` fails across multi-iframe/window browser environments because each execution context has distinct constructor references and prototypes.

---

## 10. Inheritance: ES5 Prototype Chain vs ES6 Classes

### Definition

Mechanisms for sharing behavior and attributes between parent and child abstractions.

### Explanation

ES6 classes are syntactical sugar built over JavaScript's underlying prototype chain.

### Example

**Legacy ES5 Inheritance**

```javascript
function Animal(name) {
    this.name = name;
}
Animal.prototype.speak = function() {
    console.log(this.name + " makes a sound");
};

function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function() {
    console.log(this.name + " barks");
};

var dog = new Dog("Max", "Labrador");
dog.speak();
dog.bark();

```

**Modern ES6 Class Inheritance**

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() {
        console.log(`${this.name} makes a sound`);
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }
    bark() {
        console.log(`${this.name} barks`);
    }
}

const dog = new Dog("Max", "Labrador");
dog.speak();
dog.bark();

```

### Output

```text
Max makes a sound
Max barks

```

### Architecture

```text
dog instance
 │
 ▼
Dog.prototype
 │
 ▼
Animal.prototype
 │
 ▼
Object.prototype
 │
 ▼
null

```

### Points

* In ES6 `extends`, `super()` must be invoked inside the derived class constructor before accessing `this`.
* Standard ES6 class methods are non-enumerable by default, unlike standard properties attached to ES5 prototypes.

---

## 11. Public vs Private Class Fields

### Definition

Public class fields are exposed on every instance, whereas Private class fields (prefixed with `#`) are hard-encapsulated within the class body.

### Explanation

Private fields cannot be accessed, modified, or read outside the enclosing class declaration, enforcing strict encapsulation natively.

### Example

```javascript
class BankAccount {
    publicLabel = "Savings Account"; // Public field
    #balance = 0;                    // Private field

    deposit(amount) {
        this.#balance += amount;
    }

    getBalance() {
        return this.#balance;
    }
}

const account = new BankAccount();
account.deposit(500);

console.log(account.publicLabel);
console.log(account.getBalance());
// console.log(account.#balance); // SyntaxError

```

### Output

```text
Savings Account
500

```

### Feature Comparison

* **Syntax**: Public fields use standard property names (`name`); Private fields prefix names with `#` (`#balance`).
* **External Accessibility**: Public fields are accessible everywhere; Private fields throw a SyntaxError outside the class body.
* **Encapsulation Level**: Public fields offer low encapsulation; Private fields enforce language-level hard encapsulation.

### Points

* Private fields do not use standard property lookup mechanisms; they are stored in internal private state slots accessible strictly by class methods.

---

## 12. Static Methods vs Instance Methods

### Definition

Static methods exist directly on the class constructor itself, while Instance methods are stored on `.prototype` and inherited by instance objects.

### Explanation

Use static methods for utility functions or factory methods that do not require an instance state.

### Example

```javascript
class User {
    constructor(name) {
        this.name = name;
    }

    // Instance method
    greet() {
        console.log(`Hello ${this.name}`);
    }

    // Static Factory Method
    static createAnonymous() {
        return new User("Anonymous");
    }
}

const user = User.createAnonymous();
user.greet();
// user.createAnonymous(); // TypeError: user.createAnonymous is not a function

```

### Output

```text
Hello Anonymous

```

### Points

* Static methods are inherited by child classes via prototype linkage between constructor functions (`Child.__proto__ === Parent`).

---

## 13. Composition vs Inheritance

### Definition

Inheritance establishes an **"Is-A"** hierarchy, while Composition models a **"Has-A"** or **"Uses-A"** capability by combining discrete behavioral modules.

### Explanation

Deep inheritance hierarchies create tight coupling, making refactoring brittle. Composition constructs instances by combining isolated functional features.

### Example

**Inheritance Approach**

```javascript
class Animal {}
class Dog extends Animal {}

```

**Composition Approach**

```javascript
const canEat = {
    eat() { console.log("Eating"); }
};
const canBark = {
    bark() { console.log("Barking"); }
};

function createDog(name) {
    const dog = { name };
    return Object.assign(dog, canEat, canBark);
}

const dog = createDog("Max");
dog.eat();
dog.bark();

```

### Output

```text
Eating
Barking

```

### Feature Comparison

* **Relationship**: Inheritance uses IS-A; Composition uses HAS-A / USES-A.
* **Implementation**: Inheritance relies on `extends`; Composition combines objects/behavior.
* **Coupling**: Inheritance creates tight coupling; Composition promotes loose coupling.
* **Flexibility**: Inheritance creates deep hierarchies; Composition enables flexible capability assembly.

### Points

* Favor composition over inheritance to avoid brittle base-class problems and tight coupling.

---

## 14. "Agnostic Constructor" Patterns

### Definition

A pattern ensuring a constructor function behaves consistently whether invoked with or without the `new` keyword.

### Explanation

In legacy JavaScript, invoking a constructor without `new` accidentally assigned properties to the global object (`window` or `undefined` in strict mode). Agnostic constructors enforce proper instantiation internally.

### Example

**Legacy Agnostic Pattern**

```javascript
function User(name) {
    if (!(this instanceof User)) {
        return new User(name);
    }
    this.name = name;
}

```

**Modern `new.target` Pattern**

```javascript
function User(name) {
    if (!new.target) {
        return new User(name);
    }
    this.name = name;
}

const u1 = User("John");
const u2 = new User("John");

```

### Points

* ES6 classes resolve this automatically by throwing a `TypeError: Class constructor cannot be invoked without 'new'` if invoked without `new`.

---

# Part 2 — Advanced JavaScript: Array Methods

## 1. Array Method Categorization

JavaScript provides built-in higher-order array methods grouped by functional behavior:

* **Transformation**: `map()`
* **Filtering**: `filter()`
* **Aggregation**: `reduce()`
* **Iteration**: `forEach()`
* **Searching**: `find()`, `findIndex()`, `some()`, `every()`, `includes()`
* **Ordering**: `sort()`, `toSorted()`
* **Extraction/Modification**: `slice()`, `splice()`

---

## 2. `map()`

### Definition

`map()` constructs a **new array** populated with the results of calling a provided function on every element in the calling array.

### Example

```javascript
const numbers = [1, 2, 3, 4];
const doubled = numbers.map(num => num * 2);

console.log(doubled);
console.log(numbers);

```

### Output

```text
[2, 4, 6, 8]
[1, 2, 3, 4]

```

### Workflow

```text
[1, 2, 3, 4] ──► (num * 2) ──► [2, 4, 6, 8]

```

---

## 3. `filter()`

### Definition

`filter()` constructs a **new array** containing all elements from the original array that pass a truth test predicate.

### Example

```javascript
const numbers = [1, 2, 3, 4, 5, 6];
const evens = numbers.filter(num => num % 2 === 0);

console.log(evens);

```

### Output

```text
[2, 4, 6]

```

---

## 4. `reduce()`

### Definition

`reduce()` executes a user-supplied "reducer" callback function on each element of the array, passing in the return value from the calculation on the preceding element, resulting in a **single accumulated value**.

### Example

**Basic Accumulation**

```javascript
const numbers = [1, 2, 3, 4];
const total = numbers.reduce((acc, current) => acc + current, 0);

console.log(total);

```

**Complex Aggregation (Grouping Objects)**

```javascript
const users = [
    { name: "John", role: "admin" },
    { name: "Mary", role: "user" },
    { name: "David", role: "admin" }
];

const grouped = users.reduce((acc, user) => {
    (acc[user.role] = acc[user.role] || []).push(user);
    return acc;
}, {});

console.log(grouped);

```

### Output

```text
10
{
  admin: [ { name: 'John', role: 'admin' }, { name: 'David', role: 'admin' } ],
  user: [ { name: 'Mary', role: 'user' } ]
}

```

### Points

* Always supply an initial value to `reduce()`. Executing `reduce()` on an empty array without an initial value throws a `TypeError`.

---

## 5. `forEach()` vs `map()`

### Definition

* `forEach()` executes a provided callback once for each array element without returning a value (`undefined`).
* `map()` constructs and returns a new transformed array.

### Example

```javascript
const nums = [1, 2, 3];

const mapResult = nums.map(x => x * 2);
const forEachResult = nums.forEach(x => x * 2);

console.log(mapResult);
console.log(forEachResult);

```

### Output

```text
[2, 4, 6]
undefined

```

### Points

* Use `map()` when building functional data pipelines; use `forEach()` exclusively for executing side-effects (e.g., triggering loggers or DOM updates).

---

## 6. Search Operations: `find()`, `filter()`, `some()`, `every()`, `includes()`

### Definition

* `find()`: Returns the **first element** satisfying the predicate; otherwise `undefined`.
* `findIndex()`: Returns the **index of the first element** matching the predicate; otherwise `-1`.
* `some()`: Returns `true` if **at least one** element matches.
* `every()`: Returns `true` only if **all** elements match.
* `includes()`: Returns `true` if an primitive value exists in the array.

### Example

```javascript
const users = [
    { id: 1, age: 16 },
    { id: 2, age: 22 },
    { id: 3, age: 30 }
];

console.log(users.find(u => u.age > 18));
console.log(users.some(u => u.age < 18));
console.log(users.every(u => u.age > 18));

```

### Output

```text
{ id: 2, age: 22 }
true
false

```

### Points

* `find()`, `some()`, and `every()` short-circuit on match detection, giving them an average case execution faster than $O(n)$ when matches occur early.
* `includes()` correctly handles `NaN` comparisons (`[NaN].includes(NaN)` is `true`), whereas `indexOf()` uses strict equality (`[NaN].indexOf(NaN)` is `-1`).

---

## 7. Ordering: `sort()` vs `toSorted()`

### Definition

* `sort()`: Sorts elements **in-place**, mutating the original array reference.
* `toSorted()`: Modern ES2023 immutable sort returning a **new sorted array**.

### Example

**Legacy / Mutating Sort**

```javascript
const numbers = [10, 2, 30, 4];
numbers.sort((a, b) => a - b);

console.log(numbers);

```

**Modern Immutable Sort (ES2023)**

```javascript
const numbers = [10, 2, 30, 4];
const sorted = numbers.toSorted((a, b) => a - b);

console.log(sorted);
console.log(numbers);

```

### Output

```text
[2, 4, 10, 30]
[2, 4, 10, 30]
[10, 2, 30, 4]

```

### Points

* Default `.sort()` converts items to strings, ordering lexicographically (`[10, 2]` yields `[10, 2]`). Always pass a explicit numeric comparison callback `(a, b) => a - b`.

---

## 8. Extraction & Mutation: `slice()` vs `splice()`

### Definition

* `slice(start, end)`: Non-mutating operation returning a shallow copy of a portion of an array.
* `splice(start, deleteCount, ...items)`: Mutating operation that removes, replaces, or adds elements directly inside the original array.

### Example

```javascript
const arr1 = [1, 2, 3, 4, 5];
const sliced = arr1.slice(1, 4);

const arr2 = [1, 2, 3, 4, 5];
const spliced = arr2.splice(1, 2);

console.log("Sliced:", sliced, "Original:", arr1);
console.log("Spliced:", spliced, "Original:", arr2);

```

### Output

```text
Sliced: [2, 3, 4] Original: [1, 2, 3, 4, 5]
Spliced: [2, 3] Original: [1, 4, 5]

```

### Feature Comparison

* **`slice()`**: Non-mutating; returns an extracted copy; used for non-destructive data extraction.
* **`splice()`**: Mutating; modifies the source array in place; used to add, remove, or replace elements.

---

## 9. Immutability & React State Updates

### Definition

In modern React and functional paradigms, direct state mutations bypass reference checks, breaking component re-render triggers.

### Example

**Legacy/Incorrect State Mutation**

```javascript
const users = state.users;
users.sort((a, b) => a.name.localeCompare(b.name)); // Mutates state directly

```

**Modern Immutable Pattern**

```javascript
setUsers(prev => prev.toSorted((a, b) => a.name.localeCompare(b.name)));
// Or fallback for older environments:
setUsers(prev => [...prev].sort((a, b) => a.name.localeCompare(b.name)));

```

### Points

* Array spread (`[...arr]`) performs a **shallow copy**. Nested objects inside the array still share underlying references. Use `structuredClone()` for deep object cloning.

---

# Part 3 — Modules, `this`, Functions & Async Patterns

## 1. JavaScript Modules: ES Modules (ESM) vs CommonJS (CJS)

### Definition

Modules organize discrete units of functionality into reusable isolated files.

### Example

**Legacy CommonJS (Node.js CJS)**

```javascript
// math.js
function add(a, b) { return a + b; }
module.exports = { add };

// app.js
const { add } = require("./math");

```

**Modern ES Modules (ESM Standards)**

```javascript
// math.js
export function add(a, b) { return a + b; }

// app.js
import { add } from "./math.js";

```

### Feature Comparison

* **Syntax**: CommonJS uses `require()` / `module.exports`; ES Modules use `import` / `export`.
* **Loading Mechanism**: CommonJS loads synchronously at runtime; ES Modules load asynchronously and parse statically.
* **Tree Shaking**: CommonJS support is limited and dynamic; ES Modules offer native support via static structure analysis.
* **Target Environment**: CommonJS is legacy Node.js default; ES Modules are standard across modern browsers and modern Node.js environments.

---

## 2. Understanding `this` & Invocation Contexts

### Definition

The `this` keyword evaluates to an object reference determined strictly by **how a function is invoked** at runtime (call-site).

### Example

**Default vs Method Context**

```javascript
const user = {
    name: "John",
    greet() {
        console.log(this.name);
    }
};

user.greet(); // Method call -> this refers to user

const detachedGreet = user.greet;
detachedGreet(); // Plain function call -> this is undefined in strict mode

```

**Arrow Function Context**

```javascript
const userArrow = {
    name: "John",
    greet: () => {
        console.log(this.name); // Lexical binding -> inherits outer scope 'this'
    }
};

userArrow.greet();

```

### Output

```text
John
undefined
undefined

```

### Points

* Regular functions define `this` dynamically at call-site execution.
* Arrow functions do not bind `this`; they capture `this` lexically from their enclosing execution scope.

---

## 3. Explicit Binding: `call()`, `apply()`, `bind()`

### Definition

Methods available on `Function.prototype` used to explicitly force a specific `this` context during execution.

### Explanation

* `call()`: Executes the function immediately, taking arguments individually.
* `apply()`: Executes the function immediately, taking arguments as an array.
* `bind()`: Returns a new function with `this` permanently bound, without immediate execution.

### Example

```javascript
function introduce(city, country) {
    console.log(`${this.name} lives in ${city}, ${country}`);
}

const user = { name: "John" };

// Immediate Execution
introduce.call(user, "Chennai", "India");
introduce.apply(user, ["Chennai", "India"]);

// Delayed Execution
const boundFunc = introduce.bind(user, "Chennai");
boundFunc("India");

```

### Output

```text
John lives in Chennai, India
John lives in Chennai, India
John lives in Chennai, India

```

### Feature Comparison

* **`call()`**: Executes immediately; accepts individual arguments.
* **`apply()`**: Executes immediately; accepts arguments array.
* **`bind()`**: Delayed execution; returns a new bound function.

---

## 4. Closures

### Definition

A closure is a function bundled together with references to its surrounding lexical environment, allowing access to outer variables even after the parent function has finished executing.

### Example

**Encapsulation via Closures**

```javascript
function createCounter() {
    let count = 0; // Encapsulated variable

    return {
        increment() { return ++count; },
        decrement() { return --count; },
        getCount() { return count; }
    };
}

const counter = createCounter();
console.log(counter.increment());
console.log(counter.increment());
console.log(counter.count); // Private, inaccessible directly

```

### Output

```text
1
2
undefined

```

### Classic Loop Closure Trap & Fix

**Broken (Legacy `var`)**

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Prints: 3, 3, 3

```

**Fixed (Modern `let`)**

```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Prints: 0, 1, 2

```

### Points

* `let` establishes a fresh lexical block scope for every iteration, preserving variable states inside closures created in loops.

---

## 5. Currying

### Definition

Currying transforms a multi-argument function into a sequence of nested functions, each taking a single argument.

### Example

```javascript
// Uncurried
const add = (a, b, c) => a + b + c;

// Curried
const curriedAdd = a => b => c => a + b + c;

console.log(curriedAdd(1)(2)(3));

```

### Output

```text
6

```

---

## 6. Rate Limiting: Debounce vs Throttle

### Definition

* **Debounce**: Delays function execution until a specified period of inactivity has passed.
* **Throttle**: Limits execution rate to at most once per defined time window.

### Example

**Debounce Implementation**

```javascript
function debounce(fn, delay) {
    let timer;
    return function (...args) {
        clearTimeout(timer);
        timer = setTimeout(() => fn.apply(this, args), delay);
    };
}

```

**Throttle Implementation**

```javascript
function throttle(fn, delay) {
    let lastTime = 0;
    return function (...args) {
        const now = Date.now();
        if (now - lastTime >= delay) {
            lastTime = now;
            fn.apply(this, args);
        }
    };
}

```

### Feature Comparison

* **Debounce**: Waits for inactivity; fires after events pause; used for search bar inputs and window resize ends.
* **Throttle**: Controls execution rate; fires periodically during events; used for scroll listeners and drag-and-drop operations.

---

## 7. Async Patterns: Promises & `async/await`

### Definition

Mechanisms for managing asynchronous workflows without falling into nested "callback hell".

### Example

```javascript
// Promise Producer
const fetchData = () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve("Data loaded"), 1000);
    });
};

// Async/Await Consumer
async function process() {
    try {
        const data = await fetchData();
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}
process();

```

### Output

```text
Data loaded

```

---

## 8. Parallel Async Handling: Promise Methods

### Definition

Methods for coordinating multiple asynchronous operations concurrently.

### Explanation

* `Promise.all()`: Fails fast if any promise rejects.
* `Promise.allSettled()`: Waits for all promises to settle regardless of outcome.
* `Promise.race()`: Resolves/rejects as soon as the first promise settles.
* `Promise.any()`: Resolves as soon as the first promise fulfills (ignores rejections unless all reject).

### Example

```javascript
const p1 = Promise.resolve("A");
const p2 = Promise.reject("B");
const p3 = Promise.resolve("C");

async function execute() {
    const settled = await Promise.allSettled([p1, p2, p3]);
    console.log(settled);
}
execute();

```

### Output

```text
[
  { status: 'fulfilled', value: 'A' },
  { status: 'rejected', reason: 'B' },
  { status: 'fulfilled', value: 'C' }
]

```

---

## 9. The JavaScript Event Loop & Task Queues

### Definition

The event loop manages execution contexts, coordinating the Call Stack, Web APIs, Microtask Queue, and Macrotask Queue.

### Example

```javascript
console.log("1");

setTimeout(() => console.log("2"), 0); // Macrotask

Promise.resolve().then(() => console.log("3")); // Microtask

console.log("4");

```

### Output

```text
1
4
3
2

```

### Workflow

1. Execute synchronous code on the Call Stack (`1`, `4`).
2. Move Macrotasks (`setTimeout`) to the Task Queue.
3. Move Microtasks (`Promise.then`) to the Microtask Queue.
4. When Stack empties, flush **ALL** Microtasks (`3`).
5. Pick the next Macrotask from the Task Queue (`2`).

### Architecture

```text
┌────────────────────────────────────────┐
│               Call Stack               │
└───────────────────┬────────────────────┘
                    │
                    ▼
┌────────────────────────────────────────┐
│            Microtask Queue             │ ◄── Executes entirely first
│     (Promises, process.nextTick)       │
└───────────────────┬────────────────────┘
                    │
                    ▼
┌────────────────────────────────────────┐
│            Macrotask Queue             │ ◄── Executes one per loop turn
│    (setTimeout, setInterval, I/O)      │
└────────────────────────────────────────┘

```

---

# Part 4 — Modern JavaScript (ES6+ Standards & Tooling)

## 1. JavaScript Transpilation: Babel, Presets & Polyfills

### Definition

* **Transpiler (Babel)**: Converts modern ES6+ syntax into backwards-compatible ES5 syntax.
* **Polyfill**: Provides polyfilled implementations of missing global runtime APIs or methods.

### Explanation

Babel transforms *syntax* (e.g., arrow functions to normal functions), but does not invent missing *runtime APIs* (e.g., `Promise`, `Symbol`, `Array.from`). Polyfills patch runtime targets with missing structures.

### Example

**Syntax Transpilation (Babel)**

```javascript
// Input
const add = (a, b) => a + b;

// Transpiled Output
var add = function(a, b) { return a + b; };

```

### Feature Comparison

* **Babel Plugins**: Individual transformation units for specific syntax features.
* **Babel Presets**: Pre-configured groupings of plugins (e.g., `@babel/preset-env`).
* **Polyfills**: Runtime patches providing missing global methods (`Promise`, `fetch`).

---

## 2. Block Scoped Declarations: `let`, `const`, `var` & Temporal Dead Zone

### Definition

Modern variable declarations enforce block scoping and prevent access before initialization.

### Explanation

Variables declared with `let` and `const` are hoisted, but reside inside the **Temporal Dead Zone (TDZ)** from the start of the block until execution reaches the declaration line.

### Example

```javascript
console.log(a); // undefined (var hoists with value)
var a = 1;

// console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 2;

```

### Feature Comparison

* **`var`**: Function-scoped; hoists with `undefined`; allows re-declaration.
* **`let`**: Block-scoped; hoists into TDZ; re-declaration throws error.
* **`const`**: Block-scoped; hoists into TDZ; re-declaration/re-assignment throws error.

---

## 3. Destructuring, Rest & Spread Patterns

### Definition

Syntax improvements for extracting, passing, and merging data structures easily.

### Example

**Object/Array Destructuring with Defaults**

```javascript
const user = { name: "John" };
const { name, age = 18 } = user;

const colors = ["red", "green"];
const [primary, secondary, tertiary = "blue"] = colors;

```

**Rest vs Spread Parameters**

```javascript
// Rest: Collects remaining inputs into an array
function collect(...args) {
    return args;
}

// Spread: Expands array into individual elements
const numbers = [1, 2, 3];
const combined = [...numbers, 4, 5];

```

### Output

`combined` yields `[1, 2, 3, 4, 5]`.

---

## 4. Native Fetch API & Error Handling

### Definition

The standard native browser interface for making asynchronous network HTTP requests.

### Explanation

Unlike external libraries like Axios, `fetch()` only rejects a promise on network failures. HTTP error responses (such as 404 or 500) fulfill normally with `response.ok` set to `false`.

### Example

```javascript
async function loadData() {
    try {
        const response = await fetch("https://api.example.com/data");
        
        // Manual HTTP status validation required
        if (!response.ok) {
            throw new Error(`HTTP Error Status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        console.error("Network or Parsing Failure:", error);
    }
}

```

---

# Consolidated Q&A

### Q1: What is the prototype chain?

**Answer**: The prototype chain is the linked delegation chain JavaScript traverses when resolving properties. If an object lacks a requested property, JavaScript checks its internal `[[Prototype]]`, moving up parent prototypes until finding the property or encountering `null`.

---

### Q2: What is the difference between `prototype` and `[[Prototype]]`?

**Answer**: `prototype` is an explicit object property attached to constructor functions/classes, used to build `[[Prototype]]` links on newly created instances. `[[Prototype]]` is the actual internal, hidden pointer on an instantiated object linking it to its parent prototype.

---

### Q3: What steps are executed when the `new` operator is called?

**Answer**:

1. Creates a fresh, empty JavaScript object (`{}`).
2. Links the object's internal `[[Prototype]]` to the constructor's `.prototype`.
3. Binds `this` to the new instance and executes the constructor function with supplied arguments.
4. Returns the newly created instance unless the constructor explicitly returns a non-primitive object.

---

### Q4: What is the difference between `Object.create()` and constructor invocation via `new`?

**Answer**: `new` creates an object and immediately executes the constructor function to initialize state. `Object.create(proto)` directly creates a new object using the supplied object as its `[[Prototype]]` without executing a constructor function.

---

### Q5: Are ES6 classes true classes or syntax sugar?

**Answer**: ES6 classes are syntactical sugar built over JavaScript's existing prototype mechanism. Underneath class declarations, methods reside on `Constructor.prototype`, and inheritance utilizes prototypical chaining.

---

### Q6: How does `instanceof` work internally?

**Answer**: `instanceof` tests whether `Constructor.prototype` appears anywhere along the target object's prototype chain by repeatedly calling `Object.getPrototypeOf(obj)`.

---

### Q7: Why should methods be attached to `.prototype` instead of defined inside constructor functions?

**Answer**: Defining methods inside constructor functions creates duplicate function objects in memory for every instance created. Attaching methods to `.prototype` ensures a single function instance is shared across all instances via prototype delegation.

---

### Q8: What is the difference between `map()` and `forEach()`?

**Answer**: `map()` generates and returns a new transformed array containing values returned by the callback. `forEach()` iterates over elements to perform side effects and always returns `undefined`.

---

### Q9: What is the difference between `find()` and `filter()`?

**Answer**: `find()` returns the first single element matching a predicate condition and short-circuits execution. `filter()` iterates through the entire array and returns a new array containing all matching elements.

---

### Q10: Why is direct state mutation problematic in React?

**Answer**: React performs shallow reference checks (`Object.is`) on state variables to determine if a component requires re-rendering. Mutating an existing array or object in place keeps the same object memory reference, causing React to miss the change and skip required UI updates.

---

### Q11: What is a closure and what are its practical use cases?

**Answer**: A closure is a function that retains access to variables declared in its lexical scope even after its outer execution context has been popped off the Call Stack. Use cases include data encapsulation/private state, currying, function memoization, and custom event handlers.

---

### Q12: Explain the event loop order when mixing `setTimeout` and Promises.

**Answer**: Synchronous code executes first on the Call Stack. When cleared, the event loop drains the entire Microtask Queue (Promise callbacks, `queueMicrotask`). Only after all microtasks finish does the loop process the next Macrotask (such as `setTimeout`).

---

### Q13: What is the difference between Transpiling and Polyfilling?

**Answer**: Transpiling transforms modern language *syntax* (e.g., arrow functions, classes, optional chaining) into older syntax versions. Polyfilling adds missing global *APIs or runtime methods* (e.g., `Promise`, `Array.prototype.includes`, `fetch`) by attaching custom implementations to the runtime environment.

---

### Q14: Why doesn't `fetch()` reject its promise on 404 or 500 HTTP responses?

**Answer**: The `fetch()` promise rejects only on network failures or when a request fails to complete. HTTP 404 and 500 statuses represent valid completed HTTP responses from a server, so `fetch()` resolves normally, setting `response.ok` to `false`.

---

### Q15: Compare `Promise.all()` vs `Promise.allSettled()`.

**Answer**: `Promise.all()` executes promises concurrently and rejects immediately if *any* promise rejects (fail-fast behavior). `Promise.allSettled()` waits for *all* promises to finish, returning an array of status objects detailing whether each individual promise was fulfilled or rejected.

---

### Q16: What is the Temporal Dead Zone (TDZ)?

**Answer**: TDZ is the period between entering a block scope and reaching the explicit declaration line of a `let` or `const` variable. Accessing the variable while inside its TDZ triggers a `ReferenceError`.

---

### Q17: What is the difference between `call()`, `apply()`, and `bind()`?

**Answer**: `call()` invokes a function immediately with a specified `this` context and individual arguments. `apply()` invokes the function immediately with arguments passed as an array. `bind()` returns a new function with `this` permanently bound for delayed execution.

---

### Q18: What is Tree Shaking and how do ES Modules enable it?

**Answer**: Tree shaking is a build-tool optimization process that removes dead (unused) code from production bundles. It relies on the static structure of ES Modules (`import`/`export` keywords), which can be analyzed ahead of time during compilation, unlike the dynamic `require()` calls of CommonJS.