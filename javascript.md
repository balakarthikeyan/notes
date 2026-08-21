## JavaScript Core Concepts

### 1. Scope

#### Definition

Scope determines the visibility and lifetime of variables and functions at a given point in your code during runtime. JavaScript uses **Lexical Scoping** (also called Static Scoping), meaning scope is determined at compile time based on where code is physically written in the source file, not where it is called.

JavaScript has four primary scope levels:

* **Global Scope:** Accessible anywhere in the environment.
* **Module Scope:** Accessible only within the ES module.
* **Function Scope:** Variables declared with `var` are constrained to the enclosing function.
* **Block Scope:** Variables declared with `let` and `const` are constrained to the nearest `{}` block.

#### Code Example

```javascript
const globalVar = "Global";

function outerFunction() {
  const outerVar = "Outer";

  if (true) {
    var functionScoped = "I am function-scoped (var)";
    let blockScoped = "I am block-scoped (let)";
    console.log(globalVar, outerVar, functionScoped, blockScoped);
  }

  console.log(functionScoped); // Works: `var` ignores the `if` block boundary
  
  try {
    console.log(blockScoped); // Throws error
  } catch (err) {
    console.log("Error caught:", err.message);
  }
}

outerFunction();

```

#### Explanation

1. `globalVar` resides in the global execution context and is accessible in all child scopes.
2. `outerVar` is defined inside `outerFunction` and accessible via closure/lexical lookup inside the `if` block.
3. `functionScoped` is declared using `var`, so its declaration is hoisted to the top of `outerFunction`. It ignores the `if (...)` block.
4. `blockScoped` is declared using `let`, binding it strictly to the `if` block `{}`. Accessing it outside this block throws a `ReferenceError`.

#### Output

```text
Global Outer I am function-scoped (var) I am block-scoped (let)
I am function-scoped (var)
Error caught: blockScoped is not defined

```

---

### 2. `var`, `let`, `const`

#### Definition

Keywords used to declare variables in JavaScript, differing across **Scope**, **Re-declaration**, **Re-assignment**, and **Hoisting/TDZ behavior**:

| Feature | `var` | `let` | `const` |
| --- | --- | --- | --- |
| **Scope** | Function scope | Block scope | Block scope |
| **Re-declaration** | Allowed in same scope | Forbidden | Forbidden |
| **Re-assignment** | Allowed | Allowed | Forbidden (Immutable binding) |
| **Hoisting** | Hoisted (initialized to `undefined`) | Hoisted (Uninitialized - TDZ) | Hoisted (Uninitialized - TDZ) |

#### Code Example

```javascript
// 1. Re-declaration & Shadowing
var a = 1;
var a = 2; // Valid

let b = 10;
// let b = 20; // SyntaxError: Identifier 'b' has already been declared

// 2. Mutation vs Re-assignment for `const`
const config = { theme: "dark" };
config.theme = "light"; // Valid: Mutating a property inside the object
console.log("Config theme:", config.theme);

try {
  config = { theme: "blue" }; // Invalid: Attempting to re-assign the variable binding
} catch (err) {
  console.log("Re-assignment error:", err.message);
}

```

#### Explanation

* `var` allows accidental re-declarations within the same scope, which historically caused silent variable overwrites in legacy codebases.
* `let` enforces strict single-declaration within a block scope, throwing compile-time errors on duplicates.
* `const` creates an **immutable reference binding**, not an immutable value. You can mutate properties on a `const` object or array, but you cannot re-assign the variable to a new memory reference.

#### Output

```text
Config theme: light
Re-assignment error: Assignment to constant variable.

```

---

### 3. Hoisting

#### Definition

Hoisting is the JS engine's behavior during the compilation phase where variable and function declarations are moved into memory before execution begins.

* **Function Declarations:** Fully hoisted (both declaration and implementation body).
* **`var` Declarations:** Declaration is hoisted and initialized immediately with `undefined`.
* **`let` / `const` Declarations:** Declaration is hoisted into memory, but left **uninitialized** in the Temporal Dead Zone.
* **Function Expressions:** Hoisted based on their declaring keyword (`var`, `let`, or `const`).

#### Code Example

```javascript
console.log("Hoisted var:", hoistedVar);

var hoistedVar = "Now assigned!";

// Function Declaration
hoistedFunction();

function hoistedFunction() {
  console.log("Function declaration called before definition line!");
}

// Function Expression
try {
  funcExpression();
} catch (err) {
  console.log("Function expression error:", err.message);
}

var funcExpression = function () {
  console.log("Inside expression");
};

```

#### Explanation

1. During compilation, `var hoistedVar` is hoisted and assigned `undefined`. The assignment `"Now assigned!"` happens later during the execution phase.
2. `hoistedFunction` is stored in memory with its full body implementation, making it callable anywhere in the scope.
3. `funcExpression` is declared with `var`, so the variable `funcExpression` is hoisted as `undefined`. Trying to invoke `undefined()` throws a `TypeError: funcExpression is not a function`.

#### Output

```text
Hoisted var: undefined
Function declaration called before definition line!
Function expression error: funcExpression is not a function

```

---

### 4. Temporal Dead Zone (TDZ)

#### Definition

The Temporal Dead Zone (TDZ) is the period between the entering of a scope where a `let` or `const` variable is declared, and the actual line where its initialization is executed. Attempting to evaluate or access a variable while it is in the TDZ results in an uncatchable compile-time/runtime `ReferenceError`.

#### Code Example

```javascript
{
  // --- TDZ for `value` starts here ---
  const dummy = "I am ready"; 
  
  // console.log(value); // Uncaught ReferenceError: Cannot access 'value' before initialization

  function test() {
    console.log("Value inside test function:", value);
  }

  let value = 42; // --- TDZ for `value` ends here ---

  test(); // Called AFTER initialization line
}

```

#### Explanation

* Unlike `var` (which auto-initializes to `undefined`), `let` and `const` variables remain in an "uninitialized" state in memory from block start until execution reaches `let value = 42`.
* The TDZ is temporal (time-based), not spatial (location-based). Notice `test()` is declared above `let value = 42`, but because `test()` is executed *after* `value` is initialized, it works fine.

#### Output

```text
Value inside test function: 42

```

---

### 5. Closures

#### Definition

A closure is a function bundled together with references to its surrounding state (lexical environment). In JavaScript, every inner function retains access to the scope in which it was created, even after the outer function has returned and its execution context has been popped off the call stack.

Key use cases:

* Data privacy and encapsulation (emulating private variables).
* Function carrying and partial application.
* Maintaining state in asynchronous callbacks and event handlers.

#### Code Example

```javascript
function createBankSavingsAccount(initialBalance) {
  let balance = initialBalance; // Private variable trapped in closure

  return {
    deposit(amount) {
      balance += amount;
      return balance;
    },
    withdraw(amount) {
      if (amount > balance) return "Insufficient funds";
      balance -= amount;
      return balance;
    },
    getBalance() {
      return balance;
    }
  };
}

const myAccount = createBankSavingsAccount(100);

console.log("Deposit 50:", myAccount.deposit(50));
console.log("Withdraw 30:", myAccount.withdraw(30));
console.log("Current Balance:", myAccount.getBalance());

// Direct access attempt
console.log("Direct property access:", myAccount.balance);

```

#### Explanation

1. `createBankSavingsAccount(100)` executes, creates local variable `balance`, and returns an object containing three methods.
2. Even after `createBankSavingsAccount` finishes executing, the returned object methods retain a reference to the Lexical Environment where `balance` lives.
3. `balance` cannot be directly inspected or mutated from outside code (`myAccount.balance` returns `undefined`), ensuring complete encapsulation.

#### Output

```text
Deposit 50: 150
Withdraw 30: 120
Current Balance: 120
Direct property access: undefined

```

---

### 6. `this`

#### Definition

In JavaScript, `this` is a keyword whose value is determined dynamically at runtime based on **how a function is invoked** (execution context), rather than where it is declared.

`this` binding follows five precedence rules (from highest to lowest priority):

1. **`new` Binding:** Inside a function called with `new`, `this` refers to the newly created object.
2. **Explicit Binding:** Set manually using `.call()`, `.apply()`, or `.bind()`.
3. **Implicit Binding:** Inside a method invoked as `obj.method()`, `this` refers to `obj`.
4. **Default Binding:** In non-strict mode, standalone calls point to `globalThis` (`window` in browsers, `global` in Node.js). In strict mode (`"use strict";`), default binding is `undefined`.
5. **Lexical Binding:** Arrow functions ignore all above rules and inherit `this` from their enclosing lexical scope.

#### Code Example

```javascript
"use strict";

const user = {
  name: "Alice",
  greet() {
    console.log(`Implicit binding: Hello, ${this.name}`);
  },
  delayedGreet() {
    // Loss of implicit context in callbacks when using standard function
    setTimeout(function () {
      try {
        console.log(`Callback this: ${this.name}`);
      } catch (err) {
        console.log("Strict mode error in callback:", err.message);
      }
    }, 10);
  }
};

user.greet();

// Explicit Binding with call/bind
const standaloneGreet = user.greet;
const boundGreet = standaloneGreet.bind({ name: "Bob" });
boundGreet();

user.delayedGreet();

```

#### Explanation

1. `user.greet()` uses implicit binding, so `this` inside `greet` references `user`.
2. `standaloneGreet.bind({ name: "Bob" })` creates a new function where `this` is explicitly locked to `{ name: "Bob" }`.
3. In `delayedGreet()`, `setTimeout` executes the traditional callback as a standalone function. Because strict mode is enabled, `this` is `undefined`, causing a `TypeError` when reading `this.name`.

#### Output

```text
Implicit binding: Hello, Alice
Implicit binding: Hello, Bob
Strict mode error in callback: Cannot read properties of undefined (reading 'name')

```

---

### 7. Arrow Functions

#### Definition

Introduced in ES6, arrow functions provide a concise syntax for writing functions. Crucially, they differ from standard function expressions in four ways:

* **No own `this`:** They bind `this` lexically from their enclosing scope at declaration time.
* **No `arguments` object:** Use rest parameters (`...args`) instead.
* **Cannot be constructors:** Calling an arrow function with `new` throws a `TypeError`.
* **No `prototype` property:** They do not create a `.prototype` link on definition.

#### Code Example

```javascript
const calculator = {
  factor: 2,
  
  // Standard method using regular function
  multiplyRegular(numbers) {
    return numbers.map(function (n) {
      // 'this' is lost here without arrow functions or .bind(this)
      return n * (this ? this.factor : 0);
    });
  },

  // Method using arrow function
  multiplyArrow(numbers) {
    return numbers.map((n) => n * this.factor); // Inherits 'this' from multiplyArrow context
  }
};

console.log("Regular callback output:", calculator.multiplyRegular([1, 2, 3]));
console.log("Arrow callback output:", calculator.multiplyArrow([1, 2, 3]));

const ArrowConstruct = () => {};
try {
  new ArrowConstruct();
} catch (err) {
  console.log("Constructor error:", err.message);
}

```

#### Explanation

1. In `multiplyRegular`, the callback passed to `.map()` gets executed with `this` bound to `undefined` (in strict mode), losing access to `calculator.factor`.
2. In `multiplyArrow`, the arrow function `(n) => n * this.factor` retains `this` from `multiplyArrow`, correctly accessing `this.factor` (which is `2`).
3. Attempting to instantiate `new ArrowConstruct()` fails because arrow functions lack the internal `[[Construct]]` method and `.prototype` property.

#### Output

```text
Regular callback output: [ 0, 0, 0 ]
Arrow callback output: [ 2, 4, 6 ]
Constructor error: ArrowConstruct is not a constructor

```

---

### 8. Prototypes & Prototype Chain

#### Definition

JavaScript is a prototype-based object-oriented language. Every object in JS has an internal, hidden link to another object called its **`[[Prototype]]`** (accessible via `Object.getPrototypeOf(obj)` or `__proto__`).

When accessing a property on an object:

1. JS looks at the object's own properties.
2. If not found, it traverses up the **Prototype Chain** to the object's `[[Prototype]]`.
3. It continues searching up the chain until the property is found or `null` is reached (`Object.prototype.__proto__ === null`).

#### Code Example

```javascript
function Person(name) {
  this.name = name;
}

// Adding method to prototype shared across all instances
Person.prototype.sayHello = function () {
  return `Hi, I'm ${this.name}`;
};

const developer = new Person("Sarah");

console.log("Direct property:", developer.hasOwnProperty("name"));
console.log("Prototype property:", developer.hasOwnProperty("sayHello"));
console.log("Inherited call:", developer.sayHello());

// Inspecting the Prototype Chain
console.log(
  "Is developer.[[Prototype]] === Person.prototype?",
  Object.getPrototypeOf(developer) === Person.prototype
);
console.log(
  "Is Person.prototype.[[Prototype]] === Object.prototype?",
  Object.getPrototypeOf(Person.prototype) === Object.prototype
);

```

#### Explanation

1. Properties defined inside constructor functions (`this.name`) are created directly on every instance (own properties).
2. Methods added to `Person.prototype` (`sayHello`) exist once in memory and are shared across all instances through the prototype link.
3. When calling `developer.sayHello()`, JS checks `developer` first. Finding nothing, it looks up `developer.[[Prototype]]` (which points to `Person.prototype`) and finds `sayHello`.

#### Output

```text
Direct property: true
Prototype property: false
Inherited call: Hi, I'm Sarah
Is developer.[[Prototype]] === Person.prototype? true
Is Person.prototype.[[Prototype]] === Object.prototype? true

```

---

### 9. Classes

#### Definition

Introduced in ES6, `class` syntax is **syntactic sugar** built over JavaScript's existing prototype-based inheritance model. Classes make object creation and inheritance cleaner without changing how JS works under the hood.

Modern class features (ES2022+) include:

* **Public and Private Class Fields:** Private fields are prefixed with `#` and cannot be accessed outside the class body.
* **Static Methods and Fields:** Properties bound directly to the class constructor rather than instance objects.
* **Inheritance:** Handled via `extends` and `super()`.

#### Code Example

```javascript
class Vehicle {
  #vin; // Private property

  constructor(make, vin) {
    this.make = make;
    this.#vin = vin;
  }

  getVin() {
    return this.#vin;
  }

  static isVehicle(obj) {
    return obj instanceof Vehicle;
  }
}

class Car extends Vehicle {
  constructor(make, vin, model) {
    super(make, vin); // Calls base constructor
    this.model = model;
  }

  getDetails() {
    return `${this.make} ${this.model} (VIN: ${this.getVin()})`;
  }
}

const myCar = new Car("Tesla", "5YJ3E1EA", "Model 3");
console.log(myCar.getDetails());
console.log("Static check:", Vehicle.isVehicle(myCar));

try {
  console.log(myCar.#vin); // Private field illegal access
} catch (err) {
  console.log("Private field error:", err.message);
}

```

#### Explanation

1. `class Car extends Vehicle` sets up prototype inheritance between `Car.prototype` and `Vehicle.prototype`.
2. `super(make, vin)` initializes the parent class properties before `Car` executes its own constructor block.
3. Properties declared with `#vin` use hard runtime privacy enforced by the JS engine; attempting to read `myCar.#vin` directly produces a `SyntaxError`.

#### Output

```text
Tesla Model 3 (VIN: 5YJ3E1EA)
Static check: true
Private field error: Private field '#vin' must be declared in an enclosing class

```

---

### 10. Destructuring

#### Definition

Destructuring is an ES6 expression syntax that allows unpacking values from arrays or properties from objects into distinct, individual variables.

Key Capabilities:

* **Renaming:** Assigning extracted properties to new variable names.
* **Default Values:** Fallbacks used if the extracted property evaluates to `undefined`.
* **Rest Pattern (`...`):** Capturing remaining un-extracted values into a single array or object.
* **Nested Destructuring:** Extracting values deep within nested object/array trees.

#### Code Example

```javascript
const userProfile = {
  id: 101,
  username: "alex99",
  details: {
    email: "alex@example.com",
    city: "Seattle"
  },
  roles: ["admin", "editor"]
};

// Object Destructuring with Renaming, Defaults, and Nested Extraction
const {
  username: handle,
  details: { email },
  status = "active" // Default value fallback
} = userProfile;

console.log(`Handle: ${handle}, Email: ${email}, Status: ${status}`);

// Array Destructuring with Rest Operator
const [primaryRole, ...otherRoles] = userProfile.roles;
console.log(`Primary Role: ${primaryRole}, Other Roles:`, otherRoles);

// Function Parameter Destructuring
function renderHeader({ id, username }) {
  return `User ID ${id}: ${username}`;
}

console.log(renderHeader(userProfile));

```

#### Explanation

1. `username: handle` extracts property `username` and binds it to a local variable named `handle`.
2. `status = "active"` sets `status` to `"active"` because `status` does not exist on `userProfile` (evaluates to `undefined`).
3. Array destructuring `[primaryRole, ...otherRoles]` assigns `"admin"` to `primaryRole` and uses the rest syntax to collect remaining array elements (`["editor"]`) into `otherRoles`.

#### Output

```text
Handle: alex99, Email: alex@example.com, Status: active
Primary Role: admin, Other Roles: [ 'editor' ]
User ID 101: alex99

```

---

Here is **Part 3: JavaScript (Topics 11–15)**.

---

### 11. Spread / Rest Syntax (`...`)

#### Definition

The three-dot syntax (`...`) serves two distinct purposes based on where and how it is used in code:

* **Spread Operator (`...`):** Expands an iterable (like an Array, Map, Set, or String) or an Object into individual elements/properties. Used in function calls, array literals, and object literals.
* **Rest Parameter (`...`):** Condenses multiple individual elements into a single array structure. Used in function signature parameters and destructuring assignments to collect "the rest" of the arguments or properties.

#### Code Example

```javascript
// 1. Rest Parameter in Functions & Destructuring
function calculateSum(multiplier, ...numbers) {
  // `numbers` gathers all remaining arguments into a real Array
  return numbers.reduce((sum, n) => sum + n, 0) * multiplier;
}

const user = { id: 101, name: "Maria", role: "Admin", country: "US" };
const { id, ...profileData } = user; // Rest in Object destructuring

// 2. Spread Operator in Objects & Arrays
const baseConfig = { theme: "light", debug: false };
const userConfig = { theme: "dark", lang: "en" };

// Merging objects (Rightmost property wins on conflict)
const mergedConfig = { ...baseConfig, ...userConfig, debug: true };

console.log("Rest Sum:", calculateSum(2, 10, 20, 30));
console.log("Rest Destructuring:", profileData);
console.log("Spread Merged Object:", mergedConfig);

```

#### Explanation

1. `...numbers` in `calculateSum` acts as a **Rest parameter**, collecting `10, 20, 30` into array `[10, 20, 30]`.
2. `...profileData` captures all remaining object properties excluding `id`.
3. In `mergedConfig`, the **Spread operator** expands properties. Because `userConfig.theme` comes after `baseConfig.theme`, `"dark"` overrides `"light"`.

#### Output

```text
Rest Sum: 120
Rest Destructuring: { name: 'Maria', role: 'Admin', country: 'US' }
Spread Merged Object: { theme: 'dark', debug: true, lang: 'en' }

```

---

### 12. Array Methods (Mutating vs. Non-Mutating)

#### Definition

Modern JavaScript array operations are split into two paradigms:

* **Mutating Methods (In-Place Modifications):** Alter the original array reference in memory (`push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`).
* **Non-Mutating / Immutable Methods:** Return a brand-new array or scalar value without modifying the target array (`map`, `filter`, `reduce`, `slice`, `concat`, `flatMap`).

ES2023 introduced explicit immutable array utilities (`toSorted`, `toReversed`, `toSpliced`, `with`) to support functional and state-driven programming patterns (such as React state).

#### Code Example

```javascript
const numbers = [3, 1, 4, 1, 5];

// Modern Non-Mutating Sorting (ES2023)
const sortedImmutable = numbers.toSorted((a, b) => a - b);

console.log("Original array unchanged:", numbers);
console.log("Sorted array copy:", sortedImmutable);

// Aggregation with reduce
const tally = numbers.reduce((acc, num) => {
  acc[num] = (acc[num] || 0) + 1;
  return acc;
}, {});

console.log("Tally count (reduce):", tally);

// Modern Immutable replacement (ES2023 .with)
const updatedArray = numbers.with(2, 99); // Replaces index 2 with 99
console.log("Replaced index 2:", updatedArray);

```

#### Explanation

1. `numbers.toSorted()` returns a new array, leaving `numbers` intact. In contrast, standard `.sort()` would mutate `numbers` directly.
2. `.reduce()` accumulates elements into a single structure (here, a frequency-counter map object).
3. `.with(index, value)` allows copy-on-write modifications to a single element in an array without using `slice` or spread hacks.

#### Output

```text
Original array unchanged: [ 3, 1, 4, 1, 5 ]
Sorted array copy: [ 1, 1, 3, 4, 5 ]
Tally count (reduce): { '1': 2, '3': 1, '4': 1, '5': 1 }
Replaced index 2: [ 3, 1, 99, 1, 5 ]

```

---

### 13. Shallow vs. Deep Copy

#### Definition

When copying complex data structures (Objects and Arrays):

* **Shallow Copy:** Creates a new top-level object, but **shares references** to nested child objects/arrays. Modifying a nested property on the copy mutates the original object.
* Methods: `{ ...obj }`, `Object.assign({}, obj)`, `Array.prototype.slice()`.


* **Deep Copy:** Recursively duplicates all levels, creating entirely independent memory references for every nested structure.
* Methods: `structuredClone(obj)` (Native Web API/Node.js standard), `JSON.parse(JSON.stringify(obj))` (Legacy hack with edge-case flaws), or `lodash.cloneDeep`.



#### Comparison Matrix (`structuredClone` vs `JSON.parse/stringify`)

| Feature / Data Type | `structuredClone()` | `JSON.parse(JSON.stringify())` |
| --- | --- | --- |
| **Nested Objects & Arrays** | Handled correctly | Handled correctly |
| **Dates** | Preserved as `Date` objects | Converted to ISO strings |
| **`undefined`, Symbols, Functions** | Preserved / Errors appropriately | Silently stripped/ignored |
| **Circular References** | Handled seamlessly | Throws `TypeError: Converting circular structure to JSON` |
| **Maps, Sets, ArrayBuffers** | Preserved | Converted to `{}` or lost |

#### Code Example

```javascript
const original = {
  name: "Tech Corp",
  tags: ["software", "ai"],
  createdAt: new Date(),
  unsupportedField: undefined
};

// 1. Shallow Copy
const shallowCopy = { ...original };
shallowCopy.tags.push("cloud"); // Modifies original's nested array!

// 2. Deep Copy using structuredClone
const deepCopy = structuredClone(original);
deepCopy.tags.push("mobile"); // Completely isolated

// 3. Deep Copy using JSON hack (Shows edge cases)
const jsonCopy = JSON.parse(JSON.stringify(original));

console.log("Original tags:", original.tags);
console.log("Deep copy tags:", deepCopy.tags);
console.log("JSON copy createdAt type:", typeof jsonCopy.createdAt);
console.log("JSON copy has undefined field?", "unsupportedField" in jsonCopy);

```

#### Explanation

1. Modifying `shallowCopy.tags` mutates `original.tags` because both point to the same memory reference for the `tags` array.
2. `structuredClone(original)` creates a detached memory clone and keeps `createdAt` as a valid `Date` instance.
3. `JSON.parse(JSON.stringify())` converts `createdAt` to a string and silently drops `unsupportedField` because JSON does not support `undefined`.

#### Output

```text
Original tags: [ 'software', 'ai', 'cloud' ]
Deep copy tags: [ 'software', 'ai', 'cloud', 'mobile' ]
JSON copy createdAt type: string
JSON copy has undefined field? false

```

---

### 14. Event Loop

#### Definition

JavaScript is a single-threaded execution language, meaning it has one **Call Stack** and can process only one line of code at a time. The **Event Loop** is the engine mechanism that enables non-blocking asynchronous concurrency.

The Event Loop continuously runs a process:

1. Executes synchronous code on the **Call Stack** until empty.
2. Hands off asynchronous tasks (timers, fetch requests, DOM events) to **Web APIs** (or C++ APIs in Node).
3. Once Web APIs finish, callbacks are pushed to either the **Microtask Queue** or **Macrotask Queue**.
4. When Call Stack is clear, flushes the **Microtask Queue** completely before executing the next **Macrotask**.

#### Code Example

```javascript
console.log("1: Synchronous Start");

setTimeout(() => {
  console.log("2: Timeout Callback (Web API Task)");
}, 0);

console.log("3: Synchronous End");

```

#### Explanation

1. `console.log("1...")` enters Call Stack and executes immediately.
2. `setTimeout` registers its timer callback with the Web API environment and pops off the stack.
3. `console.log("3...")` enters Call Stack and executes immediately.
4. Call stack is now empty. The Event Loop picks the timer callback waiting in the Macrotask queue and pushes it onto the call stack.

#### Output

```text
1: Synchronous Start
3: Synchronous End
2: Timeout Callback (Web API Task)

```

---

### 15. Microtask vs. Macrotask (Task Queue)

#### Definition

Asynchronous callbacks inside the Event Loop are segregated into two queues with strict execution priorities:

* **Microtask Queue (Higher Priority):**
* Examples: `Promise` callbacks (`.then`, `.catch`, `.finally`), `queueMicrotask()`, `MutationObserver`, `process.nextTick` (Node.js).
* Priority rule: **The Microtask Queue is cleared completely** until empty before the browser yields to layout/paint renders or runs the next Macrotask.


* **Macrotask Queue / Task Queue (Lower Priority):**
* Examples: `setTimeout`, `setInterval`, `setImmediate`, I/O operations, UI event listeners.
* Priority rule: **Only ONE Macrotask** is processed per iteration of the Event Loop, followed by a complete drain of any newly scheduled microtasks.



#### Code Example

```javascript
console.log("1: Sync Main Thread");

setTimeout(() => {
  console.log("2: Macrotask (setTimeout)");
}, 0);

Promise.resolve().then(() => {
  console.log("3: Microtask 1 (Promise)");
});

queueMicrotask(() => {
  console.log("4: Microtask 2 (queueMicrotask)");
});

console.log("5: Sync Main Thread End");

```

#### Execution Order Mechanics

```text
[Call Stack Execution]
├── 1: Sync Main Thread
└── 5: Sync Main Thread End
        │
        ▼ (Call stack empties -> Flush Microtask Queue)
[Microtask Queue]
├── 3: Microtask 1 (Promise)
└── 4: Microtask 2 (queueMicrotask)
        │
        ▼ (Microtasks empty -> Pick 1 Macrotask)
[Macrotask Queue]
└── 2: Macrotask (setTimeout)

```

#### Explanation

1. Lines 1 and 5 execute synchronously on the main thread.
2. `setTimeout` schedules callback `2` into the Macrotask Queue.
3. `Promise.resolve().then()` and `queueMicrotask()` schedule callbacks `3` and `4` into the Microtask Queue.
4. When the call stack finishes, the Event Loop drains **all** tasks in the Microtask Queue (`3` then `4`) before touching the Macrotask Queue (`2`).

#### Output

```text
1: Sync Main Thread
5: Sync Main Thread End
3: Microtask 1 (Promise)
4: Microtask 2 (queueMicrotask)
2: Macrotask (setTimeout)

```

---

Here is **Part 4: JavaScript (Topics 16–20)**, completing the JavaScript module.

---

### 16. Promises

#### Definition

A `Promise` is an object representing the ultimate completion or failure of an asynchronous operation and its resulting value. It acts as a proxy for a value that is not necessarily known when the promise is created.

A Promise exists in one of three mutually exclusive states:

* **`pending`**: Initial state; neither fulfilled nor rejected.
* **`fulfilled`**: The operation completed successfully (triggers `.then()`).
* **`rejected`**: The operation failed (triggers `.catch()`).

States:

```text
Pending
   ↓
Fulfilled
   OR
Rejected
```

Example:

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Data received");
  }, 1000);
});

promise.then(data => {
  console.log(data);
});
```

Output after 1 second:

```text
Data received
```

Modern JS provides four static combinators to manage concurrent promises:

| Method | Behavior | Fails When |
| --- | --- | --- |
| `Promise.all()` | Resolves when **all** promises resolve. Returns array of values. | Rejects immediately if **any** promise rejects (short-circuits). |
| `Promise.allSettled()` | Resolves when **all** promises settle (resolve OR reject). | Never rejects. Returns array of outcome objects. |
| `Promise.race()` | Settles as soon as the **first** promise settles (resolve OR reject). | Follows the state of the first settled promise. |
| `Promise.any()` | Resolves as soon as the **first** promise **fulfills**. | Rejects only if **all** promises reject (`AggregateError`). |

#### Code Example

```javascript
const fetchUserData = () => Promise.resolve({ id: 1, name: "Taylor" });
const fetchUserPosts = () => Promise.reject(new Error("Database timeout"));

// Promise.allSettled avoids short-circuit failure
Promise.allSettled([fetchUserData(), fetchUserPosts()])
  .then((results) => {
    results.forEach((result, idx) => {
      if (result.status === "fulfilled") {
        console.log(`Task ${idx + 1} Success:`, result.value);
      } else {
        console.log(`Task ${idx + 1} Failed:`, result.reason.message);
      }
    });
  });

```

#### Explanation

1. `fetchUserData()` returns a resolved promise immediately, while `fetchUserPosts()` returns a rejected promise.
2. If we used `Promise.all()`, the entire call would reject immediately, losing access to `fetchUserData()`'s result.
3. `Promise.allSettled()` waits for both tasks to finish and returns an array of status objects containing `{ status: "fulfilled", value }` or `{ status: "rejected", reason }`.

#### Output

```text
Task 1 Success: { id: 1, name: 'Taylor' }
Task 2 Failed: Database timeout

```

---

### 17. `async/await`

#### Definition

Introduced in ES2017, `async/await` is syntactic sugar built on top of **Promises** and **Generators**. It allows asynchronous, non-blocking code to be written and structured like synchronous code.

Key Mechanics:

* Marking a function `async` guarantees it returns a Promise (wrapping plain return values automatically).
* The `await` keyword pauses execution inside the `async` function until the awaited Promise settles.
* It frees the main thread while paused, allowing other Event Loop tasks to run.

#### Code Example

```javascript
const fakeApi = (value, delay) =>
  new Promise((resolve) => setTimeout(() => resolve(value), delay));

// 1. Sequential execution (Waterfall - slow)
async function runSequential() {
  const start = Date.now();
  const a = await fakeApi("Data A", 50);
  const b = await fakeApi("Data B", 50);
  console.log(`Sequential took: ~${Date.now() - start}ms ->`, [a, b]);
}

// 2. Parallel execution (Fast)
async function runParallel() {
  const start = Date.now();
  // Initiate both promises concurrently before awaiting
  const promiseA = fakeApi("Data A", 50);
  const promiseB = fakeApi("Data B", 50);
  
  const results = await Promise.all([promiseA, promiseB]);
  console.log(`Parallel took: ~${Date.now() - start}ms ->`, results);
}

async function main() {
  await runSequential();
  await runParallel();
}

main();

```

#### Explanation

1. In `runSequential()`, `await fakeApi(...)` pauses execution twice sequentially, taking a total of ~100ms.
2. In `runParallel()`, both `fakeApi` invocations start immediately without awaiting first. `Promise.all` awaits both promises concurrently, completing in ~50ms total.

#### Output

```text
Sequential took: ~100ms -> [ 'Data A', 'Data B' ]
Parallel took: ~50ms -> [ 'Data A', 'Data B' ]

```

Modern way to consume promises.

```js
async function getUsers() {
  const response = await fetch("/api/users");

  const users = await response.json();

  console.log(users);
}
```

Error handling:

```js
async function getUsers() {
  try {
    const response = await fetch("/api/users");

    if (!response.ok) {
      throw new Error("Request failed");
    }

    return await response.json();

  } catch (error) {
    console.error(error);
  }
}
```

Important:

> `async/await` does not make JavaScript synchronous. It provides cleaner syntax for working with Promises.

---

### 18. Error Handling

#### Definition

Error handling in JavaScript manages runtime exceptions gracefully using `try...catch...finally` blocks, custom error classes, and asynchronous rejection handlers.

Key Principles:

* **`try`**: Wraps dangerous code that might throw an exception.
* **`catch`**: Catches errors thrown synchronously or awaited asynchronously inside `try`.
* **`finally`**: Executes unconditionally regardless of whether an error was thrown or caught (ideal for cleanup tasks like closing database connections or clearing spinners).
* **Custom Errors**: Subclassing `Error` to preserve stack traces and add metadata (e.g., HTTP status codes).

#### Code Example

```javascript
class NetworkError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.name = "NetworkError";
    this.statusCode = statusCode;
  }
}

async function processOrder(orderId) {
  try {
    if (!orderId) {
      throw new NetworkError("Missing Order ID", 400);
    }
    console.log(`Processing order #${orderId}`);
  } catch (err) {
    if (err instanceof NetworkError) {
      console.log(`Caught ${err.name} (${err.statusCode}): ${err.message}`);
    } else {
      console.log("Unhandled Exception:", err);
    }
  } finally {
    console.log("Cleanup: Closing payment gateway connection.");
  }
}

processOrder(null);

```

#### Explanation

1. `NetworkError` extends `Error`, assigning a custom `statusCode` and setting `this.name`.
2. When `processOrder(null)` runs, `throw new NetworkError(...)` halts the `try` block and jumps into `catch`.
3. `instanceof NetworkError` isolates custom API errors from unhandled engine errors.
4. The `finally` block runs right after `catch` finishes.

#### Output

```text
Caught NetworkError (400): Missing Order ID
Cleanup: Closing payment gateway connection.

```

---

### 19. Event Bubbling & Capturing

#### Definition

When an event occurs on a DOM element, it does not happen in isolation. It propagates through the DOM tree in three distinct phases:

1. **Capturing Phase (Trickling):** The event starts at `window` and travels down the DOM tree toward the target element.
2. **Target Phase:** The event arrives at the target element where it was triggered.
3. **Bubbling Phase:** The event bubbles back up from the target element through parent nodes all the way to `window`.

EventListener Syntax: `element.addEventListener(type, listener, useCapture)`

* `useCapture = false` (Default): Trigger handler during **Bubbling**.
* `useCapture = true`: Trigger handler during **Capturing**.

Control Methods:

* `event.stopPropagation()`: Prevents further propagation along the capture/bubble chain.
* `event.stopImmediatePropagation()`: Prevents propagation AND stops other listeners attached to the *same* element from executing.
* `event.preventDefault()`: Cancels browser default behavior (e.g., form submit refresh, link navigation).

#### Code Example

```javascript
// Mocking DOM Event Propagation structure in Node/JS concept
class MockDOMElement {
  constructor(name, parent = null) {
    this.name = name;
    this.parent = parent;
    this.listeners = [];
  }

  addEventListener(phase, callback) {
    this.listeners.push({ phase, callback });
  }

  dispatch(eventName) {
    const chain = [];
    let curr = this;
    while (curr) {
      chain.unshift(curr); // Creates top-down chain [Parent, Child]
      curr = curr.parent;
    }

    // 1. Capture Phase (Window -> Target)
    chain.forEach((node) => {
      node.listeners
        .filter((l) => l.phase === "capture")
        .forEach((l) => l.callback(node.name));
    });

    // 2. Bubble Phase (Target -> Window)
    chain.reverse().forEach((node) => {
      node.listeners
        .filter((l) => l.phase === "bubble")
        .forEach((l) => l.callback(node.name));
    });
  }
}

const parent = new MockDOMElement("DIV (Parent)");
const child = new MockDOMElement("BUTTON (Child)", parent);

parent.addEventListener("capture", (name) => console.log(`[CAPTURE] Listener on ${name}`));
parent.addEventListener("bubble", (name) => console.log(`[BUBBLE] Listener on ${name}`));
child.addEventListener("bubble", (name) => console.log(`[BUBBLE] Listener on ${name}`));

child.dispatch("click");

```

#### Explanation

1. During event dispatch on `BUTTON (Child)`, propagation first runs the **Capture phase** from `DIV (Parent)` downward.
2. Next, during the **Bubbling phase**, the event flows from `BUTTON (Child)` up through `DIV (Parent)`.
3. Notice `[CAPTURE] DIV` logs *before* `[BUBBLE] BUTTON`.

#### Output

```text
[CAPTURE] Listener on DIV (Parent)
[BUBBLE] Listener on BUTTON (Child)
[BUBBLE] Listener on DIV (Parent)

```

---

### 20. Debouncing & Throttling

#### Definition

Debouncing and Throttling are performance optimization techniques used to rate-limit execution frequency for high-rate events (like `scroll`, `resize`, `mousemove`, or live search `keyup`).

* **Debounce:** Delays function execution until a specified delay period has passed **since the last time the event was triggered**. Resetting the timer on every event ensures the function fires only once after activity stops.
* *Use Case:* Auto-saving forms, live search query input fields.


* **Throttle:** Guarantees function execution occurs at most **once per specified time interval**, regardless of how many times the event fires.
* *Use Case:* Scroll position tracking, window resizing handlers, infinite scrolling triggers.


| Strategy | Behavior | Guarantee |
| --- | --- | --- |
| **Debounce** | Postpones execution until quiet period. | Fires 1 time *after* user stops interacting. |
| **Throttle** | Paces execution at regular intervals. | Fires continuously at capped frequency (e.g. max once per 100ms). |

#### Code Example

```javascript
// 1. Debounce Implementation
function debounce(fn, delay) {
  let timerId;
  return function (...args) {
    clearTimeout(timerId); // Reset timer if invoked again within delay window
    timerId = setTimeout(() => fn.apply(this, args), delay);
  };
}

// 2. Throttle Implementation
function throttle(fn, limit) {
  let inThrottle = false;
  return function (...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

// Testing Debounce
let searchCount = 0;
const performSearch = debounce((query) => {
  searchCount++;
  console.log(`[Debounce API Call ${searchCount}]: Searching for "${query}"`);
}, 50);

// Fast rapid keypresses
performSearch("r");
performSearch("rea");
performSearch("react"); // Only this final call fires after 50ms pause

```

#### Explanation

1. `debounce()` holds a `timerId` inside its closure. When `performSearch` is called rapidly three times, `clearTimeout(timerId)` cancels the previous two pending timeouts.
2. Only the third invocation ("react") survives to execute after the 50ms quiet interval elapses.

#### Output

```text
[Debounce API Call 1]: Searching for "react"

```

---