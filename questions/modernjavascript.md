# Modern JavaScript & Advanced Core Architecture

# 1. Tooling, Language Foundations & Core Syntax

### Q1: What is Modern JavaScript `(ES6+)?`

**Definition:**
Modern JavaScript refers to language features, syntax, and APIs introduced in ECMAScript 2015 (ES6) and subsequent ECMAScript revisions. The primary goal of modern JavaScript is to provide robust mechanisms for modularity, asynchronous programming, immutability patterns, object-oriented programming, functional design, maintainability, and tooling optimization.

**Workflow/Architecture:**

```text
ES6+ Feature Core
│
├── Syntax & Bindings (let, const, Template Literals, Destructuring, Spread/Rest)
├── Object & Function Enhancements (Arrow Functions, Classes, Computed Properties)
├── Modular Architecture (ES Modules, Dynamic Imports)
└── Async Ecosystem (Promises, async/await, Fetch API)

```

**Example:**

```javascript
// Legacy (ES5)
var name = "John";
var message = "Hello " + name;

// Modern (ES6+)
const user = "John";
const modernMessage = `Hello ${user}`;
console.log(modernMessage);

```

**Output:**

```text
Hello John

```

---

### Q2: What is Babel, and how do Babel Presets and Plugins work?

**Definition:**
Babel is a JavaScript compiler and transpiler used to convert modern ES6+ syntax into backwards-compatible JavaScript code capable of executing in legacy runtime environments.

* **Babel Plugin:** An individual program that performs a specific code transformation (e.g., transforming arrow functions or classes).
* **Babel Preset:** A pre-configured collection of plugins bundled together to target a specific runtime environment or framework.

**Workflow/Architecture:**

```text
Source Code (ES6+)
       │
       ▼
Babel Transpiler
       │
       ├── Plugin A (Arrow Functions)
       ├── Plugin B (Class Properties)
       └── Plugin C (Destructuring)
       │
       ▼
Compatible Output Code (ES5)

```

**Example:**

```javascript
// Source Code (Modern ES6 Input)
const greet = name => `Hello ${name}`;

// Transpiled Babel Output (ES5 Target)
var greet = function (name) {
  return "Hello " + String(name);
};
console.log(greet("Alice"));

```

**Output:**

```text
Hello Alice

```

---

### Q3: What is the difference between Transpiling and Polyfilling?

**Definition:**

* **Transpiling:** Changes syntax (e.g., converting arrow functions, `const`/`let`, or class definitions into standard ES5 syntax). Transpilation does not inject missing runtime APIs.
* **Polyfilling:** Provides an implementation for missing runtime APIs, objects, or methods (e.g., `Promise`, `Map`, `Set`, `Array.prototype.includes`) that do not exist in older engines.

**Workflow/Architecture:**

```text
Modern Code Feature
       │
       ├────── Syntax Error in Legacy Engine? ──► Transpile (Babel Syntax Transform)
       │
       └────── Missing API Object in Engine?  ──► Polyfill (Runtime Shim Injection)

```

**Example:**

```javascript
// Legacy Code Needing Transpilation (Syntax Change)
// Input:
const add = (a, b) => a + b;
// Transpiled Output:
var add = function(a, b) { return a + b; };

// Code Needing Polyfill (Runtime API Injection)
if (!window.Promise) {
    window.Promise = PolyfillPromiseImplementation;
}

```

**Output:**

```text
Transpilation transforms syntax; Polyfilling provides runtime APIs.

```

---

### Q4: What is `@babel/preset-env` and how does it optimize JavaScript bundles?

**Definition:**
`@babel/preset-env` is a smart Babel preset that automatically determines the exact set of syntax transformations and polyfills required based on target browser or runtime specifications (e.g., `browserslist`). This prevents unnecessary transformation overhead when target environments natively support modern features.

**Workflow/Architecture:**

```text
Target Environments (Browsers / Node.js)
                  ↓
          @babel/preset-env
                  ↓
 Required Transformations & Polyfills ONLY
                  ↓
       Optimized Bundle Output

```

**Example:**

```javascript
// Source Input
const values = [1, 2, 3];
const double = values.map(x => x * 2);

// Target: Modern Browsers (ES2022+) -> Code stays intact
// Target: IE11 -> Code transformed to ES5 function expressions

```

**Output:**

```text
Target-driven transformation eliminates bloated ES5 code in modern environments.

```

---

# 2. Variables, Scope, & Memory Rules

### Q5: What is the difference between `var`, `let`, and `const`?

**Definition:**

* `var`: Function-scoped or globally scoped variable binding. It allows re-declaration and re-assignment and is hoisted with an initial value of `undefined`.
* `let`: Block-scoped variable binding. It permits re-assignment but prohibits re-declaration within the same scope.
* `const`: Block-scoped variable binding that cannot be reassigned or re-declared. However, `const` does not make complex objects or arrays immutable.

**Example:**

```javascript
// var (Function Scoped)
if (true) {
    var a = 10;
}
console.log(a);

// let / const (Block Scoped)
if (true) {
    let b = 20;
    const c = { name: "John" };
    c.name = "Mike"; // Property mutation allowed
    console.log(c.name);
}
// console.log(b); // ReferenceError: b is not defined

```

**Output:**

```text
10
Mike

```

---

### Q6: What is the Temporal Dead Zone (TDZ)?

**Definition:**
The Temporal Dead Zone (TDZ) is the period between entering a block scope and the execution of the explicit variable initialization for `let` or `const` declarations. While `let` and `const` bindings are hoisted, accessing them during the TDZ triggers a `ReferenceError`.

**Workflow/Architecture:**

```text
Block Scope Entered
       │
       ▼
┌──────────────┐
│     TDZ      │ ◄── Variable accessed here throws ReferenceError
└──────────────┘
       │
       ▼
let / const variable = value  ◄── Variable initialized & accessible

```

**Example:**

```javascript
{
    // TDZ for variable 'user' starts here
    // console.log(user); // Uncaught ReferenceError: Cannot access 'user' before initialization
    
    let user = "Alice"; // TDZ ends here
    console.log(user);
}

```

**Output:**

```text
Alice

```

---

### Q7: How do Template Literals and Multiline Strings work?

**Definition:**
Template literals use backtick characters (```) instead of standard quotes. They support expression interpolation using `${expression}`, clean multiline string declaration without concatenation operators, and tagged template formatting.

**Example:**

```javascript
const price = 100;
const quantity = 3;

// Multiline & Interpolation
const invoice = `
Invoice:
Total: $${price * quantity}
`;

console.log(invoice.trim());

```

**Output:**

```text
Invoice:
Total: $300

```

---

# 3. Functions, Destructuring, & Modern Syntax Enhancements

### Q8: How do Arrow Functions work and how do they handle `this`?

**Definition:**
Arrow functions provide concise function syntax and possess lexical `this` binding. Unlike standard function declarations, arrow functions do not bind their own `this`, `arguments`, `super`, or `new.target` bindings; instead, they capture `this` from their immediate outer scope.

**Example:**

```javascript
const user = {
    name: "John",
    greet() {
        const innerArrow = () => {
            console.log(`Hello ${this.name}`);
        };
        innerArrow();
    }
};

user.greet();

```

**Output:**

```text
Hello John

```

---

### Q9: When should Arrow Functions NOT be used?

**Definition:**
Arrow functions cannot be used as object methods requiring dynamic `this`, event listeners needing element-bound `this`, prototype methods, or constructor functions called with `new` (because arrow functions lack a `prototype` property and internal `[[Construct]]` method).

**Example:**

```javascript
const Person = (name) => {
    this.name = name;
};

try {
    const p = new Person("John");
} catch (err) {
    console.log(err.message);
}

```

**Output:**

```text
Person is not a constructor

```

---

### Q10: How does Array Destructuring work (including skipping values)?

**Definition:**
Array destructuring allows positional extraction of values from arrays into distinct local variables using array syntax positioning.

**Example:**

```javascript
const numbers = [10, 20, 30, 40];

// Basic and skipped value destructuring
const [first, , third] = numbers;

console.log(first);
console.log(third);

```

**Output:**

```text
10
30

```

---

### Q11: How does Object Destructuring work (including renaming, nested, and default values)?

**Definition:**
Object destructuring extracts property values from objects by matching property keys. It supports variable renaming, nested object extraction, and default value assignment if a property evaluates to `undefined`.

**Example:**

```javascript
const user = {
    id: 101,
    profile: {
        firstName: "Alice"
    }
};

// Renaming, nested extraction, and default values
const {
    id: userId,
    profile: { firstName },
    role = "StandardUser"
} = user;

console.log(userId);
console.log(firstName);
console.log(role);

```

**Output:**

```text
101
Alice
StandardUser

```

---

### Q12: How do Default Parameters work?

**Definition:**
Default parameters initialize function arguments with default values if no value or `undefined` is passed during invocation. Default parameters trigger exclusively for `undefined` arguments, not for other falsy values like `null`, `false`, or `0`.

**Example:**

```javascript
function calculateTotal(price, tax = 0.05) {
    return price + (price * tax);
}

console.log(calculateTotal(100));
console.log(calculateTotal(100, 0.10));

```

**Output:**

```text
105
110

```

---

### Q13: What are Computed Property Names, Property Shorthand, and Method Shorthand?

**Definition:**

* **Computed Property Names:** Allows evaluated dynamic expressions wrapped in brackets `[]` to serve as object keys.
* **Property Shorthand:** Omits the key-value pairing when the variable name matches the target property key.
* **Method Shorthand:** Omits the `function` keyword when defining methods inside object literals.

**Example:**

```javascript
const keyName = "role";
const name = "John";

const user = {
    name, // Property Shorthand
    [keyName]: "Admin", // Computed Property
    greet() { // Method Shorthand
        return `User: ${this.name}`;
    }
};

console.log(user);
console.log(user.greet());

```

**Output:**

```text
{ name: 'John', role: 'Admin', greet: [Function: greet] }
User: John

```

---

### Q14: What is the Spread Operator and how does Shallow Copying work?

**Definition:**
The spread operator (`...`) expands iterable objects or array elements into individual elements or shallowly copies properties from one object into another. Spread operations create shallow copies—nested object references remain shared between the original and copied object.

**Workflow/Architecture:**

```text
Original Object ───► [Top-level properties copied] ───► Cloned Object
     │                                                      │
     └──────► Shared Nested Object Reference ◄──────────────┘

```

**Example:**

```javascript
const original = { name: "John", meta: { city: "Chennai" } };
const copy = { ...original, name: "Mike" };

copy.meta.city = "Bangalore"; // Mutates nested object shared by original

console.log(original.name);
console.log(original.meta.city);

```

**Output:**

```text
John
Bangalore

```

---

### Q15: What are Rest Parameters and how do they differ from Spread?

**Definition:**

* **Rest Parameters (`...args`):** Collects multiple discrete arguments passed to a function into a single array structure. Rest must appear as the final parameter in a function definition.
* **Spread Operator (`...iterable`):** Expands an array or object into individual elements or properties.

**Example:**

```javascript
// Rest (Collects values into array)
function sum(...numbers) {
    return numbers.reduce((acc, curr) => acc + curr, 0);
}

const values = [10, 20, 30];
// Spread (Expands array elements)
console.log(sum(...values));

```

**Output:**

```text
60

```

---

# 4. Prototype Chain, Object-Oriented JS, & Classes

### Q16: What is the Prototype and Prototype Chain in JavaScript?

**Definition:**
A prototype is an internal object from which another object inherits properties and methods. The prototype chain is the lookup path JavaScript traverses when accessing a property or method; if a property is not found on the instance, engine lookups climb up the prototype hierarchy until reaching `Object.prototype` or `null`.

**Workflow/Architecture:**

```text
Instance Object (user)
       │ [[Prototype]]
       ▼
User.prototype
       │ [[Prototype]]
       ▼
Object.prototype
       │ [[Prototype]]
       ▼
     null

```

**Example:**

```javascript
const user = { name: "Alice" };

// toString() is inherited via prototype chain from Object.prototype
console.log(user.hasOwnProperty("name"));
console.log(user.toString());

```

**Output:**

```text
true
[object Object]

```

---

### Q17: How do `Object.getPrototypeOf()`, `__proto__`, and `Object.hasOwn()` work?

**Definition:**

* `Object.getPrototypeOf(obj)`: The standard ECMAScript method to inspect an object's prototype reference.
* `__proto__`: Legacy accessor property for accessing or mutating an object's internal `[[Prototype]]`.
* `Object.hasOwn(obj, prop)`: The modern, safe static method to check if a property exists directly on an object (own property) without relying on inherited prototype methods.

**Example:**

```javascript
const proto = { greet: "Hello" };
const obj = Object.create(proto);
obj.id = 1;

console.log(Object.getPrototypeOf(obj) === proto);
console.log(Object.hasOwn(obj, "id"));
console.log(Object.hasOwn(obj, "greet"));

```

**Output:**

```text
true
true
false

```

---

### Q18: How do Constructor Functions and Prototype Sharing work?

**Definition:**
Constructor functions initialize new instances when invoked with the `new` keyword. Attaching shared methods to the constructor function's `.prototype` property ensures all instances share a single memory reference for methods rather than re-creating functions per instance.

**Example:**

```javascript
function User(name) {
    this.name = name;
}

// Shared prototype method
User.prototype.greet = function() {
    return `Hello, ${this.name}`;
};

const user1 = new User("Alice");
const user2 = new User("Bob");

console.log(user1.greet === user2.greet);

```

**Output:**

```text
true

```

---

### Q19: What does the `new` operator do under the hood?

**Definition:**
When a function is executed with the `new` keyword, JavaScript automatically performs the following steps under the hood:

1. Creates a blank plain object `{}`.
2. Sets the new object's internal `[[Prototype]]` link to the constructor function's `.prototype` property.
3. Executes the constructor function, binding `this` to the newly created object.
4. Returns the object created in step 1, unless the constructor explicitly returns a non-primitive object.

**Workflow/Architecture:**

```text
new User("John")
       │
       ├── 1. Create empty object: obj = {}
       ├── 2. Set prototype: Object.setPrototypeOf(obj, User.prototype)
       ├── 3. Execute constructor: User.call(obj, "John")
       └── 4. Return obj (if non-primitive return absent)

```

**Example:**

```javascript
function ManualNew(Constructor, ...args) {
    const instance = Object.create(Constructor.prototype);
    const result = Constructor.apply(instance, args);
    return (typeof result === "object" && result !== null) ? result : instance;
}

function Person(name) {
    this.name = name;
}

const p = ManualNew(Person, "Dave");
console.log(p.name);

```

**Output:**

```text
Dave

```

---

### Q20: How does `Object.create()` differ from `new`?

**Definition:**
`Object.create(proto)` creates a new object and explicitly links its internal `[[Prototype]]` to `proto` without invoking a constructor function. The `new` operator creates an object, links its prototype, and executes the constructor function to initialize state.

**Example:**

```javascript
const animalPrototype = {
    speak() { return `${this.name} makes a noise.`; }
};

// Object.create
const dog = Object.create(animalPrototype);
dog.name = "Rex";

console.log(dog.speak());
console.log(Object.getPrototypeOf(dog) === animalPrototype);

```

**Output:**

```text
Rex makes a noise.
true

```

---

### Q21: What is the difference between `Function.prototype`, `Constructor.prototype`, and `[[Prototype]]`?

**Definition:**

* `Function.prototype`: The prototype object shared by all function objects in JavaScript (where methods like `.call()`, `.apply()`, and `.bind()` reside).
* `Constructor.prototype`: The prototype object associated with a constructor/class that becomes assigned as `[[Prototype]]` to instances created via `new Constructor()`.
* `[[Prototype]]`: The actual internal hidden prototype reference link of an instance object.

**Workflow/Architecture:**

```text
User Function Object ────────► [[Prototype]] ────────► Function.prototype
       │
       └── .prototype property ──► User.prototype ◄── [[Prototype]] ── user Instance

```

**Example:**

```javascript
function User() {}
const user = new User();

console.log(Object.getPrototypeOf(User) === Function.prototype);
console.log(Object.getPrototypeOf(user) === User.prototype);

```

**Output:**

```text
true
true

```

---

### Q22: How does `instanceof` work compared to `typeof`?

**Definition:**

* `typeof`: A basic operator that returns a string indicating the primitive data type or primitive object categorization (`"string"`, `"number"`, `"boolean"`, `"function"`, `"object"`, `"undefined"`, `"symbol"`, `"bigint"`).
* `instanceof`: Tests whether a constructor function's `.prototype` property appears anywhere along the target object's prototype chain.

**Example:**

```javascript
function Admin() {}
const admin = new Admin();

console.log(typeof admin);
console.log(admin instanceof Admin);
console.log(admin instanceof Object);

```

**Output:**

```text
object
true
true

```

---

### Q23: How is inheritance implemented via Constructor Functions vs ES6 Classes?

**Definition:**

* **Legacy Constructor Inheritance:** Uses `ParentConstructor.call(this, args)` inside the child constructor to copy instance properties, combined with `Child.prototype = Object.create(Parent.prototype)` to chain prototype methods.
* **Modern ES6 Class Inheritance:** Uses `class Child extends Parent` and calls `super(args)` in the child constructor to perform internal prototype wiring syntactic sugar.

**Example:**

```javascript
// Legacy Pattern
function AnimalLegacy(name) {
    this.name = name;
}
AnimalLegacy.prototype.eat = function() { return `${this.name} eats.`; };

function DogLegacy(name, breed) {
    AnimalLegacy.call(this, name);
    this.breed = breed;
}
DogLegacy.prototype = Object.create(AnimalLegacy.prototype);
DogLegacy.prototype.constructor = DogLegacy;

// Modern ES6 Class Pattern
class AnimalModern {
    constructor(name) { this.name = name; }
    eat() { return `${this.name} eats.`; }
}

class DogModern extends AnimalModern {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }
}

const dog = new DogModern("Buddy", "Golden");
console.log(dog.eat());

```

**Output:**

```text
Buddy eats.

```

---

### Q24: How do Public Class Fields, Private Class Fields (`#`), and Static Methods work?

**Definition:**

* **Public Class Fields:** Instance properties declared directly inside the class body.
* **Private Class Fields (`#`):** Properties scoped strictly to the class body, inaccessible outside the class using property accessors.
* **Static Methods:** Methods attached directly to the class constructor itself, rather than to class instances.

**Example:**

```javascript
class BankAccount {
    publicField = "Public Access";
    #balance = 0; // Private Class Field

    constructor(initial) {
        this.#balance = initial;
    }

    deposit(amount) {
        this.#balance += amount;
        return this.#balance;
    }

    static getBankInfo() { // Static Method
        return "National Bank System";
    }
}

const acc = new BankAccount(500);
console.log(acc.deposit(200));
console.log(BankAccount.getBankInfo());
// console.log(acc.#balance); // SyntaxError

```

**Output:**

```text
700
National Bank System

```

---

### Q25: What is Composition vs Inheritance, and why does React prefer Composition?

**Definition:**

* **Inheritance ("Is-A"):** Establishes tight coupling through vertical inheritance chains (`Dog extends Animal`).
* **Composition ("Has-A / Uses-A"):** Combines independent, modular functions or objects to assemble complex features. React favors composition (e.g., passing components via `props.children`) because it prevents brittle, deep class hierarchies and offers flexible reuse.

**Example:**

```javascript
// Composition Pattern
const canEat = { eat: () => "Eating" };
const canBark = { bark: () => "Barking" };

const createDog = (name) => {
    return {
        name,
        ...canEat,
        ...canBark
    };
};

const dog = createDog("Buddy");
console.log(dog.eat(), dog.bark());

```

**Output:**

```text
Eating Barking

```

---

### Q26: What is an Agnostic Constructor pattern?

**Definition:**
An Agnostic Constructor (or Scope-Safe Constructor) is a defensive programming pattern for constructor functions that ensures an instance is correctly instantiated regardless of whether the caller invokes the function with or without the `new` keyword.

**Example:**

```javascript
function User(name) {
    if (!(this instanceof User)) {
        return new User(name);
    }
    this.name = name;
}

const u1 = new User("Alice");
const u2 = User("Bob"); // Safe invocation without 'new'

console.log(u1.name);
console.log(u2.name);

```

**Output:**

```text
Alice
Bob

```

---

# 5. Advanced Functional Patterns (`this`, Closures, Currying, Debounce, Throttle)

### Q27: How is the `this` keyword determined in regular vs arrow functions?

**Definition:**

* **Regular Functions:** The value of `this` is dynamically bound based on how the function is invoked at runtime (Method call `obj.fn()`, Plain call `fn()`, Explicit `call/apply/bind`, or Constructor `new fn()`). In strict mode, plain invocation defaults `this` to `undefined`.
* **Arrow Functions:** Bound lexically at creation time based on the enclosing execution context.

**Example:**

```javascript
"use strict";

const obj = {
    name: "ScopeObject",
    regular() {
        console.log("Regular:", this ? this.name : undefined);
    },
    arrow: () => {
        console.log("Arrow:", typeof this);
    }
};

obj.regular();

const detached = obj.regular;
detached(); // Strict mode default invocation

obj.arrow();

```

**Output:**

```text
Regular: ScopeObject
Regular: undefined
Arrow: undefined

```

---

### Q28: What is the difference between `call()`, `apply()`, and `bind()`?

**Definition:**

* `call(thisArg, arg1, arg2, ...)`: Invokes the function immediately with explicit `this` and individual arguments.
* `apply(thisArg, [argArray])`: Invokes the function immediately with explicit `this` and arguments provided as an array.
* `bind(thisArg, arg1, ...)`: Returns a new function with `this` permanently bound, delaying execution.

**Example:**

```javascript
function introduce(city, country) {
    return `${this.name} from ${city}, ${country}`;
}

const person = { name: "Alice" };

console.log(introduce.call(person, "Paris", "France"));
console.log(introduce.apply(person, ["Tokyo", "Japan"]));

const boundFn = introduce.bind(person, "London");
console.log(boundFn("UK"));

```

**Output:**

```text
Alice from Paris, France
Alice from Tokyo, Japan
Alice from London, UK

```

---

### Q29: What is a Closure and how does it enable data encapsulation?

**Definition:**
A closure is a function bundled together with references to its surrounding lexical environment. This allows an inner function to retain access to outer scope variables even after the outer enclosing function has completed execution, providing private encapsulation.

**Workflow/Architecture:**

```text
Outer Function Execution
       │
       ├── Variable: let privateCounter = 0
       │
       └── Return Inner Function
                 │
                 ▼
Inner Function retains Lexical Environment Link ──► [ privateCounter Access ]

```

**Example:**

```javascript
function createCounter() {
    let count = 0; // Private state via closure
    return {
        increment() { count++; return count; },
        getCount() { return count; }
    };
}

const counter = createCounter();
console.log(counter.increment());
console.log(counter.increment());
console.log(counter.count); // Undefined: Encapsulated

```

**Output:**

```text
1
2
undefined

```

---

### Q30: How does `let` fix the classic `var` inside `setTimeout` loop trap?

**Definition:**
When using `var` inside a loop, `var` creates a single function-scoped variable shared by all timer callbacks; by the time callbacks fire, the loop variable has completed iteration. `let` creates a distinct block-scoped binding for each loop iteration.

**Example:**

```javascript
// Legacy Var Trap Solution / Comparison
function runLoop() {
    for (let i = 0; i < 3; i++) {
        setTimeout(() => console.log(`let i: ${i}`), 10);
    }
}
runLoop();

```

**Output:**

```text
let i: 0
let i: 1
let i: 2

```

---

### Q31: What is Currying and how is it implemented?

**Definition:**
Currying transforms a multi-argument function into a series of nested unary functions that each accept a single argument.

**Workflow/Architecture:**

```text
f(a, b, c) ──► Currying ──► f(a)(b)(c)

```

**Example:**

```javascript
// Standard Function
const multiply = (a, b) => a * b;

// Curried Version
const curriedMultiply = a => b => a * b;

const double = curriedMultiply(2);
console.log(double(5));
console.log(curriedMultiply(3)(4));

```

**Output:**

```text
10
12

```

---

### Q32: What is Debouncing and how is it implemented?

**Definition:**
Debouncing is a rate-limiting technique that delays a function execution until a specified duration of inactivity has elapsed since its last invocation. Rapid repeated triggers reset the timer.

**Workflow/Architecture:**

```text
Events Triggered:  ██ ██ ██ ──────────── (Inactivity Pause) ──────► Execution Fires
Timer Action:      Reset Reset Reset ──────────────────────────────► Execute Callback

```

**Example:**

```javascript
function debounce(fn, delay) {
    let timer;
    return function (...args) {
        clearTimeout(timer);
        timer = setTimeout(() => fn.apply(this, args), delay);
    };
}

const logSearch = debounce(q => console.log(`Search: ${q}`), 100);
logSearch("Rea");
logSearch("React"); // Resets previous timer

```

**Output:**

```text
(After 100ms pause)
Search: React

```

---

### Q33: What is Throttling and how does it compare to Debouncing?

**Definition:**

* **Throttling:** Ensures a function executes at most once in a specified time window, regardless of trigger frequency.
* **Debouncing:** Executes only after events stop for a given duration.

**Example:**

```javascript
function throttle(fn, limit) {
    let inThrottle = false;
    return function (...args) {
        if (!inThrottle) {
            fn.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

const processScroll = throttle(() => console.log("Scroll event logged"), 100);
processScroll();
processScroll(); // Ignored during throttle period

```

**Output:**

```text
Scroll event logged

```

---

# 6. Array Methods & Immutability Patterns

### Q34: How do transformation and filtering methods (`map()`, `filter()`, `forEach()`) work?

**Definition:**

* `map()`: Transforms every element of an array, returning a new transformed array of identical length.
* `filter()`: Evaluates elements against a boolean predicate, returning a new array containing matching elements.
* `forEach()`: Iterates through elements to perform side effects, returning `undefined`.

**Example:**

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);

console.log("Doubled:", doubled);
console.log("Evens:", evens);

```

**Output:**

```text
Doubled: [ 2, 4, 6, 8, 10 ]
Evens: [ 2, 4 ]

```

---

### Q35: How does `reduce()` work for accumulation and data grouping?

**Definition:**
`reduce()` processes an array elements sequentially against an accumulator callback, reducing the values into a single consolidated result (such as a number, object, or array). Always pass an initial value to avoid runtime errors on empty arrays.

**Example:**

```javascript
const items = [
    { type: "fruit", name: "apple" },
    { type: "veg", name: "carrot" },
    { type: "fruit", name: "banana" }
];

// Data Grouping
const grouped = items.reduce((acc, item) => {
    acc[item.type] = acc[item.type] || [];
    acc[item.type].push(item.name);
    return acc;
}, {});

console.log(grouped);

```

**Output:**

```text
{ fruit: [ 'apple', 'banana' ], veg: [ 'carrot' ] }

```

---

### Q36: How do search methods (`find()`, `findIndex()`, `some()`, `every()`, `includes()`) work?

**Definition:**

* `find()`: Returns the first element satisfying the predicate, or `undefined`.
* `findIndex()`: Returns the index of the first matching element, or `-1`.
* `some()`: Returns `true` if at least one element satisfies the predicate.
* `every()`: Returns `true` if all elements satisfy the predicate.
* `includes()`: Checks for value presence using equality.

**Example:**

```javascript
const users = [{ id: 1, admin: false }, { id: 2, admin: true }];

console.log(users.find(u => u.admin));
console.log(users.some(u => u.admin));
console.log(users.every(u => u.admin));

```

**Output:**

```text
{ id: 2, admin: true }
true
false

```

---

### Q37: What is the difference between `sort()`, `toSorted()`, `slice()`, and `splice()`?

**Definition:**

* `sort()`: In-place array sorting (mutates original array).
* `toSorted()`: Modern non-mutating sort returning a new array.
* `slice(start, end)`: Non-mutating shallow extraction of elements.
* `splice(start, deleteCount, ...items)`: In-place array mutation (removes/inserts elements).

**Example:**

```javascript
const numbers = [3, 1, 2];

// Mutating vs Non-Mutating Sort
const immutableSorted = numbers.toSorted((a, b) => a - b);
console.log("Original:", numbers);
console.log("Immutable Sorted:", immutableSorted);

// Slice (Copy) vs Splice (Mutate)
const sliced = numbers.slice(0, 2);
console.log("Sliced:", sliced);

```

**Output:**

```text
Original: [ 3, 1, 2 ]
Immutable Sorted: [ 1, 2, 3 ]
Sliced: [ 3, 1 ]

```

---

### Q38: How do Shallow Copy traps affect state immutability, and how does `structuredClone()` help?

**Definition:**
Spread or `Object.assign()` perform shallow copies, leaving nested references pointing to original memory locations. In React state management, direct object mutations cause failed re-renders. `structuredClone()` provides a native, deep-copy mechanism for complex serializable object trees.

**Example:**

```javascript
const userState = { id: 1, settings: { theme: "dark" } };

// Native Deep Clone
const deepCopy = structuredClone(userState);
deepCopy.settings.theme = "light";

console.log("Original Theme:", userState.settings.theme);
console.log("Deep Copy Theme:", deepCopy.settings.theme);

```

**Output:**

```text
Original Theme: dark
Deep Copy Theme: light

```

---

# 7. Modules & Bundling Architecture

### Q39: What are ES Modules (ESM) and how do Named vs Default Exports work?

**Definition:**
ES Modules (ESM) represent the official standard module system in ECMAScript (`import`/`export`).

* **Named Exports:** Permits multiple exports per file; must be imported using exact corresponding names wrapped in braces `{}`.
* **Default Export:** Single primary export per file; imported without braces using any chosen identifier.

**Example:**

```javascript
// Module Exports
export const API_URL = "https://api.com"; // Named Export
export default function fetchData() { return "Data"; } // Default Export

// Module Consumer Imports
// import fetchData, { API_URL } from "./module.js";

```

**Output:**

```text
ESM provides strict static module encapsulation.

```

---

### Q40: What is the difference between ES Modules and CommonJS?

**Definition:**

* **CommonJS (`require`/`module.exports`):** Node.js legacy system; synchronous, dynamic evaluation at runtime.
* **ES Modules (`import`/`export`):** Browser & modern Node standard; asynchronous, statically analyzed before code execution.

**Example:**

```javascript
// CommonJS (CJS) Syntax
const path = require("path");
module.exports = { path };

// ES Module (ESM) Syntax
import path from "path";
export default path;

```

**Output:**

```text
CJS is dynamic/synchronous; ESM is static/asynchronous.

```

---

### Q41: What is Tree Shaking and how does static module resolution enable it?

**Definition:**
Tree shaking is a build tool optimization process (e.g., via Rollup, Webpack) that eliminates dead/unused exports from the final JavaScript bundle. Tree shaking relies on the static structure of ES Modules to analyze `import` and `export` statements before runtime.

**Workflow/Architecture:**

```text
Module Source Code ──► Bundler Static Analysis ──► Remove Unused Imports ──► Shaken Minified Bundle

```

**Example:**

```javascript
// utils.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b; // Unused export

// app.js
import { add } from "./utils.js";
console.log(add(2, 3));
// Result: Bundler shakes 'subtract' out of output bundle

```

**Output:**

```text
Dead code eliminated automatically from bundle output.

```

---

### Q42: What is Dynamic Import (`import()`) and Code Splitting?

**Definition:**
Dynamic `import(modulePath)` loads JavaScript modules asynchronously on demand, returning a Promise. This enables code splitting, allowing applications to defer downloading non-critical modules until required.

**Example:**

```javascript
// Dynamic Import
async function loadAnalytics() {
    const analytics = await import("./analytics.js");
    analytics.init();
}

```

**Output:**

```text
Modules downloaded lazily on demand.

```

---

# 8. Asynchronous JavaScript & Event Loop Execution Model

### Q43: What is a Native Promise and what are its execution states?

**Definition:**
A Promise is an object representing the ultimate completion or failure of an asynchronous operation. Promises exist in one of three mutually exclusive states:

1. **Pending:** Initial default state.
2. **Fulfilled:** Operation succeeded (`resolve()` invoked).
3. **Rejected:** Operation failed (`reject()` invoked).

**Workflow/Architecture:**

```text
                 ┌───► Fulfilled (.then)
Pending Promise ─┤
                 └───► Rejected (.catch)

```

**Example:**

```javascript
const asyncOp = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Operation Successful"), 50);
});

asyncOp.then(data => console.log(data));

```

**Output:**

```text
Operation Successful

```

---

### Q44: How do `Promise.all()`, `Promise.allSettled()`, `Promise.race()`, and `Promise.any()` compare?

**Definition:**

* `Promise.all()`: Fulfills if all promises fulfill; short-circuits and rejects immediately if any promise rejects.
* `Promise.allSettled()`: Waits for all promises to settle regardless of outcome, returning array of status objects.
* `Promise.race()`: Settles as soon as the first promise settles (fulfills or rejects).
* `Promise.any()`: Fulfills as soon as the first promise fulfills; ignores rejections unless all reject.

**Example:**

```javascript
const p1 = Promise.resolve("Fast");
const p2 = Promise.reject("Error");

Promise.allSettled([p1, p2]).then(results => console.log(results));

```

**Output:**

```text
[
  { status: 'fulfilled', value: 'Fast' },
  { status: 'rejected', reason: 'Error' }
]

```

---

### Q45: How does `async/await` work with `try/catch` error handling?

**Definition:**
`async/await` is syntactic sugar built over Promises. An `async` function always wraps returned values in a resolved Promise. The `await` expression pauses execution within the async function context until the Promise settles. Synchronous `try/catch` blocks handle errors.

**Example:**

```javascript
async function fetchUser() {
    try {
        const res = await Promise.reject(new Error("Network Failure"));
        return res;
    } catch (err) {
        console.log("Caught:", err.message);
    }
}
fetchUser();

```

**Output:**

```text
Caught: Network Failure

```

---

### Q46: What is the performance difference between Sequential and Parallel `await`?

**Definition:**

* **Sequential Await:** Awaits independent promises consecutively, compounding total wait time ($T_{total} = t_1 + t_2$).
* **Parallel Await:** Triggers operations concurrently and awaits them collectively via `Promise.all()`, reducing wait time to the slowest operation ($T_{total} = \max(t_1, t_2)$).

**Example:**

```javascript
const delay = ms => new Promise(res => setTimeout(res, ms));

async function runParallel() {
    const start = Date.now();
    await Promise.all([delay(50), delay(50)]);
    console.log(`Parallel Duration: ~${Date.now() - start}ms`);
}
runParallel();

```

**Output:**

```text
Parallel Duration: ~50ms

```

---

### Q47: Why does `fetch()` NOT reject on HTTP 404 or 500 status codes?

**Definition:**
The browser `fetch()` API only rejects its returned Promise on network errors or when the request fails to execute. HTTP status codes like `404 Not Found` or `500 Server Error` are valid HTTP responses. Developers must manually verify the `response.ok` boolean property.

**Example:**

```javascript
async function checkResponse(responseMock) {
    if (!responseMock.ok) {
        throw new Error(`HTTP Error: ${responseMock.status}`);
    }
    return responseMock.json();
}

const mock404 = { ok: false, status: 404 };
checkResponse(mock404).catch(err => console.log(err.message));

```

**Output:**

```text
HTTP Error: 404

```

---

### Q48: How does the Event Loop handle Call Stack, Web APIs, Microtasks, and Macrotasks?

**Definition:**
The Event Loop constantly manages execution order:

1. Synchronous code runs on the Call Stack.
2. Asynchronous APIs delegate tasks to runtime Web APIs.
3. Microtasks (`Promise.then`, `queueMicrotask`) populate the Microtask Queue.
4. Macrotasks (`setTimeout`, `setInterval`) populate the Task Queue.
5. Microtasks are completely drained before processing the next Macrotask.

**Workflow/Architecture:**

```text
Call Stack (Sync) ──► Microtask Queue (Promises) ──► Task Queue (Timers/DOM)

```

**Example:**

```javascript
console.log("1: Sync Start");

setTimeout(() => console.log("4: Macrotask Timer"), 0);

Promise.resolve().then(() => console.log("3: Microtask Promise"));

console.log("2: Sync End");

```

**Output:**

```text
1: Sync Start
2: Sync End
3: Microtask Promise
4: Macrotask Timer

```

---

# 9. Senior Interview Cheatsheet & Key Takeaways

### Core Language Rules & Memory Mechanics

* `var` is function-scoped and hoisted with `undefined`. `let` and `const` are block-scoped and hoisted into the Temporal Dead Zone (TDZ).
* `const` enforces binding immutability, not structural object freeze.
* Arrow functions capture `this` lexically from their outer scope; they lack `prototype`, `arguments`, and `super` bindings and cannot serve as constructors.
* `Object.hasOwn(obj, prop)` replaces `obj.hasOwnProperty()` for robust, safe checks on own properties.

### Prototype Hierarchy & Classes

* Objects inherit via an internal `[[Prototype]]` link. Prototype lookup walks up the chain until reaching `Object.prototype` then `null`.
* `Function.prototype` is the prototype of functions; `Constructor.prototype` is the prototype assigned to instances created via `new`.
* Class syntax is syntactic sugar over standard prototype inheritance; ES6 private class fields start with `#`.
* React favors Composition over Inheritance ("Has-A" over "Is-A") to avoid tight coupling and brittle hierarchies.

### Execution & Async Order

* Microtasks (`Promise.then`, `queueMicrotask`) take priority over Macrotasks (`setTimeout`, `setInterval`). Microtask queues are fully drained before picking up the next macrotask.
* `fetch()` does not reject on 404/500 HTTP status codes; explicitly inspect `response.ok`.
* Use `Promise.all()` for independent concurrent operations; use `Promise.allSettled()` when operations can fail independently without breaking execution.

### Immutability & Array Optimization

* Mutating Array Methods: `sort()`, `splice()`, `push()`, `pop()`, `shift()`, `unshift()`, `reverse()`.
* Immutability-Safe Counterparts: `toSorted()`, `toReversed()`, `toSpliced()`, `slice()`, `map()`, `filter()`.
* Deep Cloning: Use native `structuredClone()` to deep-copy nested state safely and prevent shallow reference bugs in React state engines.