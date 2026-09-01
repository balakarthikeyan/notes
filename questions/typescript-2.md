# What is TypeScript?

TypeScript is an open-source programming language developed and maintained by Microsoft. It is a typed superset of JavaScript that compiles to plain JavaScript.

1. **Static Typing:** You can declare the types of variables, function parameters, and return types. This allows for better code quality, early error detection, and improved code documentation.

2. **Language Features:** It includes classes (ES6 also has classes but without `public`, `protected`, `private` modifiers), interfaces, enums, and modules. These features enable better code organization, encapsulation, and maintainability.

3. **Tooling and Ecosystem:** TypeScript has its own compiler (`tsc`) that transpiles TypeScript code into JavaScript, providing additional checks and optimizations.

4. **Compatibility:** TypeScript is a superset of JavaScript, so any valid JavaScript code is also valid TypeScript code.

```typescript
interface Person {
  name: string;
  age: number;
}

class Employee implements Person {
  name: string;
  age: number;
  id: number;

  constructor(name: string, age: number, id: number) {
    this.name = name;
    this.age = age;
    this.id = id;
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}, ID: ${this.id}`;
  }
}

const employee = new Employee("John Doe", 30, 12345);
console.log(employee.getDetails()); // Output: Name: John Doe, Age: 30, ID: 12345
```

---

# How do you declare a variable in TypeScript?

You can declare a variable using the `let` or `const` keyword, just like in JavaScript. However, TypeScript also allows you to specify the variable type using type annotations.

```typescript
// Declaring a variable with a type annotation
let myNumber: number = 42;

// Declaring a variable without a type annotation (Type Inference)
let myString = "Hello, world!";

// Declaring a constant with a type annotation
const PI: number = 3.14;

// Declaring a variable with an array type
let myArray: string[] = ["apple", "banana", "orange"];

// Declaring a variable with an object type
let myObject: { name: string; age: number } = { name: "Alice", age: 30 };
```

---

# What is a union type in TypeScript?

A union type allows a variable to have more than one possible type. It is defined using the vertical bar `|` to separate the different types that are allowed. For example: `let myVariable: number | string;`

---

# How do you define an interface in TypeScript?

An `interface` defines the structural contract for an object. It specifies the names, types of properties, and method signatures that an object must adhere to.

```typescript
interface UserProfile {
  id: number;
  username: string;
  email?: string; // Optional property
  readonly createdAt: Date; // Readonly property
}
```

---

# What is a namespace in TypeScript?

A `namespace` is a TypeScript-specific way to group related code together under a single named scope, preventing naming conflicts in the global namespace.

```typescript
namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }

  export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string): boolean {
      return s.length === 5;
    }
  }
}
```

---

# How can you use decorators in TypeScript?

`Decorators` provide a way to add metadata or behavior to classes, methods, properties, and parameters at design time. Decorators are applied using the `@` symbol followed by the decorator function name. A decorator function receives arguments based on where it is applied:

* **Class Decorator:** Target constructor function. takes one argument, which is the constructor function of the class.
* **Method/Property Decorator:** Prototype of the class and the member name. takes three arguments: the target object (the prototype of the class for instance members), the member name, and a property descriptor.
* **Parameter Decorator:** Target prototype, member name, and index of the parameter. takes four arguments: the target object, the member name, the accessor descriptor, and an optional descriptor for the associated property.

Decorators are currently an experimental feature in TypeScript.

| Decorator     | Applied To       | Arguments Received                      |
| ------------- | ---------------- | --------------------------------------- |
| **Class**     | Class            | `(constructor)`                         |
| **Method**    | Method           | `(target, propertyKey, descriptor)`     |
| **Property**  | Property         | `(target, propertyKey)`                 |
| **Parameter** | Method Parameter | `(target, propertyKey, parameterIndex)` |

```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

1. Class Decorator

A Class Decorator is applied to the constructor of a class. It can replace or modify the class.

```typescript
function timestamp<T (...args: any[]): extends new { {} }>(constructor: T) {
  return class extends constructor {
    timestamp = new Date();
  };
}

@timestamp
class MyClass {
  timestamp?: Date;
}

const myInstance = new MyClass();
console.log(myInstance.timestamp);

// Output
// logs the current date 2026-07-21T08:35:12.456Z
```

How it works: `function timestamp<T extends new (...args: any[]) => {}>(constructor: T)` receives: `constructor` which is equivalent to `MyClass` returns a new class extending the original one.

2. Method Decorator

A Method Decorator can inspect, modify, or replace a method. In TypeScript, `PropertyDescriptor` is a built-in interface that mirrors native JavaScript property descriptors to describe the configuration and behavior of an object property.

```typescript
function logExecution(
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
) {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args: any[]) {
        console.log(`Calling ${propertyKey}`);
        console.log("Arguments:", args);

        const result = originalMethod.apply(this, args);

        console.log(`Finished ${propertyKey}`);
        console.log("Returned:", result);

        return result;
    };
}

class Calculator {

    @logExecution
    add(a: number, b: number): number {
        return a + b;
    }

}

const calc = new Calculator();

console.log(calc.add(10, 20));

// Output
// Calling add
// Arguments: [10,20]
// Finished add
// Returned: 30
// 30

```
How it works: Arguments received `target` prototype of the class `Calculator.prototype` propertyKey `Method name "add"` descriptor
`PropertyDescriptor` contains: value, writable, enumerable, configurable

3. Property Decorator

Property decorators cannot modify the property's value directly, but they can record metadata or observe declarations.

```typescript
function readonly(target: any, propertyKey: string) {
    console.log(`${propertyKey} is marked as readonly`);
}

class User {
    @readonly
    id!: number;
    name!: string;
}

const user = new User();

// Output
// id is marked as readonly
```

4. Parameter Decorator

A Parameter Decorator is applied to a method parameter.

It receives:

- Target object
- Method name
- Parameter index

```typescript
function logParameter(
    target: any,
    methodName: string,
    parameterIndex: number
) {
    console.log(
      `Parameter at index ${parameterIndex} in method ${methodName}`
    );
}

class Employee {
    greet(
      @logParameter name: string,
      age: number
    ) {
      console.log(`Hello ${name}, Age: ${age}`);
    }
}

const emp = new Employee();
emp.greet("John", 30);

// Output
// Parameter at index 0 in method greet
// Hello John, Age: 30
```

---

# What are modifiers in TypeScript?

In TypeScript, `public`, `private`, and `protected` modifiers control the visibility and accessibility of class members (properties and methods):

* `public`: Accessible from anywhere (default).
* `private`: Accessible only within the declaring class.
* `protected`: Accessible within the declaring class and its subclasses.

---

# What is a type assertion in TypeScript?

Type assertion is a mechanism to explicitly tell the TypeScript compiler to treat a value as a specific type when you have information that TypeScript cannot infer automatically.

```typescript
let value: unknown = "This is a string";

// Using 'as' syntax
let strLength1: number = (value as string).length;

// Using angle-bracket syntax
let strLength2: number = (<string>value).length;
```

---

# What is a module in TypeScript?

A module is a way to organize code into reusable, self-contained units that execute within their own scope (not in the global scope). Modules can contain classes, functions, interfaces, and other code, and can be either internal to a project or external libraries. Modules export declarations using the `export` keyword and consume other modules using the `import` keyword.

```typescript
// mathUtils.ts
export function add(a: number, b: number): number {
  return a + b;
}

// app.ts
import { add } from './mathUtils';
console.log(add(5, 10));
```

---

## An Introduction to Objects

```typescript
let car = {
  brand: 'Toyota',
  model: 'Corolla',
  year: 2022,
  start: function() {
    return `${this.brand} ${this.model} started!`;
  }
};
```

---

## Accessing Object Properties and Methods

```typescript
console.log(car.brand); // Dot notation to access property
console.log(car['model']); // Bracket notation to access property
console.log(car.start()); // Accessing object method

(car as any).color = 'blue'; // Adding a new property
car.year = 2023; // Modifying an existing property
delete (car as any).model; // Removing a property
```

---

## Object Prototypes and Inheritance

### Inheritance through Prototypes

```typescript
function Vehicle(this: any, brand: string, model: string, year: number) {
  this.brand = brand;
  this.model = model;
  this.year = year;
}

Vehicle.prototype.start = function() {
  return `${this.brand} ${this.model} started!`;
};

function Car(this: any, brand: string, model: string, year: number, color: string) {
  Vehicle.call(this, brand, model, year); // Call the Vehicle constructor
  this.color = color;
}

// Set Car's prototype to an instance of Vehicle
Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car; // Reset constructor

Car.prototype.drive = function() {
  return `The ${this.color} ${this.brand} ${this.model} is driving!`;
};

let myCar = new (Car as any)('Toyota', 'Corolla', 2022, 'blue');

console.log(myCar.start()); // Output: Toyota Corolla started!
console.log(myCar.drive()); // Output: The blue Toyota Corolla is driving!
```

### Using Closures for Encapsulation

Encapsulation encompasses the consolidation of data (properties) and methods (functions) within a singular unit, known as an object.

```typescript
function createCounter() {
  let count = 0; // Private variable
  return {
    increment: function() {
      count++;
    },
    getCount: function() {
      return count;
    }
  };
}

let counter = createCounter();
counter.increment();
console.log(counter.getCount()); // Output: 1
console.log((counter as any).count); // Output: undefined (count is private)
```

### Abstraction

Abstraction simplifies intricate realities by constructing classes or objects that hide internal implementation details.

```typescript
abstract class Shape {
  constructor(public color: string) {}

  // Abstract method
  abstract calculateArea(): number;
}

class Circle extends Shape {
  constructor(public radius: number, color: string) {
    super(color);
  }

  calculateArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

let myCircle = new Circle(5, 'red');
console.log(myCircle.calculateArea()); // Output: ~78.54
```

### Polymorphism

Polymorphism, a fundamental principle within Object-Oriented Programming (OOP), enables objects of various classes to be treated as instances of a shared superclass.

```typescript
// Creating a superclass
class Animal {
  makeSound(): string {
    return 'Some generic sound';
  }
}

// Creating a subclass
class Dog extends Animal {
  makeSound(): string {
    return 'Woof!';
  }
}

// Creating instances
let genericAnimal: Animal = new Animal();
let dog: Animal = new Dog();

console.log(genericAnimal.makeSound()); // Output: Some generic sound
console.log(dog.makeSound()); // Output: Woof!
```

---

## Generics Functions

A generic function works with any data type. The actual type is captured when the function is called, whether it is a number, string, or custom object type.

```typescript
function identity<T>(arg: T): T {
  return arg;
}

// Using the generic function with explicit type argument
let num = identity<number>(42); // T is number
let str = identity<string>("Hello"); // T is string

// TypeScript automatically infers the type as well:
// let num = identity(42); 
// let str = identity("Hello"); 

const fetchData = async <Type>(url: string): Promise<Type> => {
  const response = await fetch(url);
  const data: Type = await response.json();
  return data;
};

fetchData<{ name: string; age: number }>("/api/books")
  .then(res => {
    console.log(res);
    // res is typed as { name: string; age: number }
  });
```

---

## Generic Interfaces

Generic types are specified when an instance is created and can represent any type. The constraint is that every property maintains its declared type configuration.

```typescript
interface Pair<T, K> {
  first: T;
  second: K;
}

let pair: Pair<string, number> = { first: "one", second: 2 };
let anotherPair: Pair<number, string> = { first: 1, second: "second" };
let otherPair: Pair<number, number> = { first: 1, second: 2 };
```

---

## Generic Types

Generics can also be applied to custom type aliases. You define a shape with placeholder types, which are bound when the type is referenced.

```typescript
type Person<T, K, V> = {
  name: T;
  age: K;
  isMarried: V;
};

const person1: Person<string, number, boolean> = {
  name: 'Bob',
  age: 33,
  isMarried: false
};
```

---

## Generic Classes

Generic classes allow you to define a blueprint where data types are flexible, being specified during instantiation.

```typescript
class Box<T> {
  content: T;

  constructor(content: T) {
    this.content = content;
  }

  getContent(): T {
    return this.content;
  }
}

const letterBox = new Box<string>('a');
const numberBox = new Box<number>(1);
```

---

## Generics with built-in functions

```typescript
const numberSet = new Set<number>();
numberSet.add(1);
numberSet.add(2);
numberSet.add(3);
console.log(numberSet);

const stringToNumberMap = new Map<string, number>();
stringToNumberMap.set("one", 1);
stringToNumberMap.set("two", 2);
stringToNumberMap.set("three", 3);
console.log(stringToNumberMap);
```

---

## Constraints

Generics support constraints using the `extends` keyword, allowing you to limit the types passed to generic parameter placeholders.

```typescript
type LengthType = { length: number };

function getLength<T LengthType extends>(args: T): number {
  return args.length;
}

getLength('abc'); // Output: 3
getLength([1, 2]); // Output: 2

// The following will throw compilation errors:
// getLength(2); // Error: Type 'number' does not have a 'length' property
// getLength({ "a": "b" }); // Error: Object literal does not have a 'length' property
```

---

## Utility types

Utility types in TypeScript use generics to perform common type transformations. Built-in utility types include `Partial`, `Required`, `Omit`, `Pick`, `Readonly`, `Exclude`, `Extract`, `ReturnType`, `Parameters`, `NonNullable`, `Awaited`, and `Record`.

```typescript
interface User {
  name: string;
  age: number;
  location: string;
}

// Makes all properties optional
const partialUser: Partial<User> = { name: 'Joe' };
console.log(partialUser);

// interface User = {
//   name?: string | undefined;
//   age?: number | undefined;
//   location?: string | undefined;
// }

// Makes all properties required
type RequiredPerson = Required<User>;
let reqUser: RequiredPerson = { name: 'Joe', age: 33, location: 'India' };
console.log(reqUser);

// Omits specified properties
const omitUser: Omit<User, 'name'> = { location: 'India', age: 33 };
console.log(omitUser);

type PersonDetail = {
  name: string;
  lastName: string;
  age: number;
  hobbies: string;
};

// Selects specific properties
type SomePersonPick = Pick<PersonDetail, "age" "name" |>;
const somePersonPick: SomePersonPick = { name: 'Joe', age: 33 };
console.log(somePersonPick);

// type somePersonPick = {
//  name: string;
//  age: number;
// }

// Omits specific properties
type SomePersonOmit = Omit<PersonDetail, "hobbies" "lastName" |>;
const somePersonOmit: SomePersonOmit = { name: 'Joe', age: 33 };
console.log(somePersonOmit);

// type somePersonOmit = {
//  name: string;
//  age: number;
// }

// Makes all properties readonly
type ReadOnlyPerson = Readonly<PersonDetail>;
const personReadOnly: ReadOnlyPerson = {
  name: "Fizz",
  lastName: 'Alice',
  age: 33,
  hobbies: 'football'
};
console.log(personReadOnly);

// type ReadOnlyPerson = {
//     readonly name: string;
//     readonly lastName: string;
//     readonly age: number;
//     readonly hobbies: string;
// }
```

`Extract` extracts types from a union that are assignable to a specific type:

```typescript
// Extract a member from a union
type Animal = "cat" | "dog" | "bird" | "fish";
type OnlyCat = Extract<Animal, "cat">; // 'cat'

// Extract strings containing a specific pattern from a union
type Keys = 'userId' | 'tweetId' | 'userName' | 'tweetName';
type UserKey = Extract<Keys, `user${string}`>; // "userId" | "userName"

// Extract specific primitives from a union
type AllTypes = 'admin' | 'user' | 5 | 6 | 7 | true;
type OnlyNumbers = Extract<AllTypes, number>; // 5 | 6 | 7

// Extract members of a discriminated union
type ShapeType =
  | { type: 'square'; size: number }
  | { type: 'circle'; radius: number }
  | { type: 'triangle'; side: number };

type SquareAndCircleShape = Extract<ShapeType, 'circle' 'square' type: { | }>; 
// type SqaureAndCircleShape = Extract<ShapeType, { type: 'square' | 'circle' }> 
// {type: 'square'; size: number;} | { type: 'circle' }
```

---

# What are SOLID principles in JavaScript / TypeScript?

SOLID principles are a set of software design guidelines that promote high cohesion, low coupling, and scalable software architecture.

### 1. Single Responsibility Principle (SRP)

A class or module should have one, and only one, reason to change. 

It should fulfill a single responsibility. This principle promotes smaller, focused classes that are easier to understand and maintain.

```typescript
class User {
  constructor(public name: string, public email: string) {}
}

class EmailSender {
  sendEmail(user: User, subject: string, message: string): void {
    // Code for sending email
  }
}

class UserRepository {
  saveUser(user: User): void {
    // Code for saving user to database
  }
}
```

### 2. Open-Closed Principle (OCP)

Software entities should be open for extension, but closed for modification. 

It means that you should be able to add new functionality to a module without modifying its existing code. By using techniques such as inheritance, interfaces, and dependency injection

```typescript
abstract class Shape {
  abstract calculateArea(): number;
}

class Circle extends Shape {
  constructor(public radius: number) {
    super();
  }

  calculateArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(public width: number, public height: number) {
    super();
  }

  calculateArea(): number {
    return this.width * this.height;
  }
}
```

### 3. Liskov Substitution Principle (LSP)

Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.

This principle emphasizes that subclasses should be able to be used interchangeably with their base classes, without causing errors or unexpected behavior.

```typescript
abstract class Shape {
  abstract calculateArea(): number;
}

class Rectangle extends Shape {
  constructor(public width: number, public height: number) {
    super();
  }

  calculateArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(public side: number) {
    super();
  }

  calculateArea(): number {
    return this.side * this.side;
  }
}
```

### 4. Interface Segregation Principle (ISP)

Clients should not be forced to depend on interfaces they do not use. Prefer small, focused interfaces.

Its better to create smaller and more specific interfaces that are tailored to the requirements of the client.

```typescript
interface Printer {
  print(document: string): void;
}

interface Scanner {
  scan(document: string): void;
}

interface FaxMachine {
  fax(document: string): void;
}

class MultiFunctionPrinter implements Printer, Scanner, FaxMachine {
  print(document: string): void { /* print */ }
  scan(document: string): void { /* scan */ }
  fax(document: string): void { /* fax */ }
}

class SimplePrinter implements Printer {
  print(document: string): void { /* print */ }
}
```

### 5. Dependency Inversion Principle (DIP)

High-level modules should not depend on low-level modules. Both should depend on abstractions.

It helps in decoupling modules, making them more flexible, and allowing easier substitution of implementations.

```typescript
interface Database {
  getUser(userId: string): any;
}

class UserService {
  constructor(private database: Database) {}

  getUser(userId: string) {
    return this.database.getUser(userId);
  }
}

class MongoDBDatabase implements Database {
  getUser(userId: string) {
    // Code for retrieving user from MongoDB
  }
}

class MySQLDatabase implements Database {
  getUser(userId: string) {
    // Code for retrieving user from MySQL
  }
}

```

---

## Question 1 - What is TypeScript?

TypeScript is a typed superset of JavaScript that adds static type definitions to the language. JavaScript is a dynamically typed language, which can lead to runtime errors in production. With TypeScript, developers can catch type errors early during compilation.

---

## Question 2 - What is explicit and implicit type assignment?

* **Explicit:** Explicitly declaring the type using annotations.
```typescript
let firstName: string = "Joe";
```

* **Implicit:** TypeScript infers the type based on the assigned value.
```typescript
let age = 41; // TypeScript infers 'age' as number
```

---

## Question 3 - Difference between any, unknown and never in TypeScript?

* `any` disables type checking and allows any operation.
```typescript
let x: any = 10;
x = 'hello'; // Allowed
console.log(x.toUpperCase()); // Allowed at compile-time
```

* `unknown` is a type-safe counterpart of `any`. It requires type checking or type assertion before performing operations.
```typescript
let y: unknown = 10;
if (typeof y === 'number') {
  console.log(y.toFixed(2)); // Safe
}
```

* `never` represents values that never occur (e.g., functions that always throw an exception or never return).
```typescript
function throwError(message: string): never {
  throw new Error(message);
}
```
---

## Question 4 - How do you give the type of Arrays?

Array types are declared using `type[]` or `Array<type>` syntax:

```typescript
const names: string[] = ["Joe", "Ram", "Rahul"];
names.push("Bala"); // Allowed

const readonlyNames: readonly string[] = ["Joe", "Ram", "Rahul"];
// readonlyNames.push("Jack"); // Error: Property 'push' does not exist on type 'readonly string[]'
```

---

## Question 5 - What is Type Inference in array?

If an array is initialized without explicit annotations, TypeScript infers its element types based on initialized values.

```typescript
const numbers = [1, 2, 3]; // Inferred as number[]
numbers.push(4); // Allowed
```

---

## Question 6 - What are tuples?

A tuple is an array with a fixed number of elements whose types are known at specific indexes.

```typescript
let ourTuple: [number, boolean, string];
ourTuple = [5, false, 'Coding Hero was here']; // Correct
```

---

## Question 7 - What are readonly tuples?

Standard tuples do not prevent array mutator methods like `.push()`. Marking a tuple `readonly` enforces immutable tuple structures.

```typescript
let standardTuple: [number, boolean, string] = [5, false, 'Coding Hero was here'];
// standardTuple.push('Unsafe push'); // Compiles in standard tuples

let safeTuple: readonly [number, boolean, string] = [5, false, 'Coding Hero was here'];
// safeTuple.push('Error'); // Error: Property 'push' does not exist on type 'readonly [number, boolean, string]'
```

---

## Question 8 - How to give the types for Objects?

Object types are defined inline using structure syntax:

```typescript
const car: { brand: string; model: string; year: number } = {
  brand: "Tata",
  model: "Tiago",
  year: 2016
};
```

---

## Question 9 - How to have optional properties in Objects?

Optional properties are specified by appending a `?` after the property key name.

```typescript
const car: { brand: string; model: string; year?: number } = {
  brand: "Tata",
  model: "Punch"
};
```

---

## Question 10 - Explain enum in TypeScript?

An `enum` is a group of unchangeable named constants. By default, enums assign auto-incremented numeric values starting at 0.

```typescript
enum AllDirections { North, East, South, West }

let currentDirection = AllDirections.North;
console.log(currentDirection); // Logs 0
```

---

## Question 11 - What is String enum?

String enums require explicit string initialization for each member, yielding readable output during execution.

```typescript
enum AllDirections {
  North = "North",
  East = "East",
  South = "South",
  West = "West"
}

let currentDirection = AllDirections.North;
console.log(currentDirection); // Logs "North"
```

---

## Question 12 - What are Type Aliases?

Type Aliases assign custom labels to primitive types, unions, intersections, arrays, or object structural contracts using the `type` keyword.

```typescript
type Year = number;
type Type = string;
type Model = string;

type Car = {
  year: Year;
  type: Type;
  model: Model;
};

const carYear: Year = 2005;
const carType: Type = "Tata";
const carModel: Model = "Tiago";

const car: Car = {
  year: carYear,
  type: carType,
  model: carModel
};
```

---

## Question 13 - What are interfaces?

Interfaces define structural contracts primarily for object types.

```typescript
interface Square {
  length: number;
}

const square: Square = {
  length: 20
};
```

---

## Question 14 - How to extend interfaces?

Interfaces inherit members from other interfaces using the `extends` keyword.

```typescript
interface Square {
  length: number;
}

interface ColorSquare extends Square {
  color: string;
}

const square: ColorSquare = {
  length: 20,
  color: "blue"
};
```

---

## Question 15 - What are Union types?

Union types specify that a value can be one of multiple types using the pipe `|` operator.

```typescript
function printSuccessCode(code: string | number) {
  console.log(`My success code is ${code}.`);
}

printSuccessCode(200);
printSuccessCode('200');
```

---

## Question 16 - How to give the return type in function?

Function return types are specified after parameter declarations using a colon `:`.

```typescript
function getSum(): number {
  return 24;
}

function printMessage(): void {
  console.log("Good Morning");
}
```

---

## Question 17 - How to give type of parameters in function?

Each parameter type annotation follows parameter identifiers with a colon `:`.

```typescript
function sum(a: number, b: number): number {
  return a + b;
}
```

---

## Question 18 - How to give optional, default and rest parameters in function?

```typescript
// Optional parameter 'c'
function subtract(a: number, b: number, c?: number): number {
  return a - b - (c || 0);
}

// Default parameter 'b = 10'
function multiply(a: number, b: number = 10): number {
  return a * b;
}

// Rest parameter '...rest'
function add(a: number, b: number, ...rest: number[]): number {
  return a + b + rest.reduce((acc, curr) => acc + curr, 0);
}
```

---

## Question 19 - What is casting in TypeScript?

Casting explicitly converts or asserts a variable type using `as` or `<Type>` syntax.

```typescript
let y: unknown = 'Welcome';
console.log((y as string).length);

// Angle-bracket alternative
console.log((<string>y).length);
```

---

## Question 20 - How to give type of a variable in Class?

Class properties are declared inside class definitions with explicit type annotations:

```typescript
class Developer {
  name: string = "";
}

const dev = new Developer();
dev.name = "Joe";
```

---

## Question 21 - What is public, private and protected in TypeScript classes?

* `public`: Accessible anywhere (default).
* `private`: Accessible only within the declaring class.
* `protected`: Accessible within the declaring class and its subclasses.

```typescript
// Public Example
class DeveloperPublic {
  public name: string = "";
}

const devPublic = new DeveloperPublic();
devPublic.name = "Joe";

// Private Example with Getter
class DeveloperPrivate {
  private name: string;

  public constructor(name: string) {
    this.name = name;
  }

  public getName(): string {
    return this.name;
  }
}

const devPrivate = new DeveloperPrivate("Joe");
console.log(devPrivate.getName());

// Shorthand Parameter Properties
class DeveloperShorthand {
  public constructor(private name: string) {}

  public getName(): string {
    return this.name;
  }
}

const devShorthand = new DeveloperShorthand("Joe");
console.log(devShorthand.getName());

// Protected Example with Inheritance & Interface Implementation
interface Shape {
  getArea: () => number;
}

class Rectangle implements Shape {
  public constructor(protected width: number, protected height: number) {}

  public getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  public constructor(width: number) {
    super(width, width);
  }
}

const mySq = new Square(20);
console.log(mySq.getArea()); // Output: 400
```

---

## Question 22 - What is the readonly keyword in reference to classes in TypeScript?

The `readonly` keyword prevents member variables from being mutated after class initialization.

```typescript
class Developer {
  public constructor(private readonly name: string) {}

  public getName(): string {
    return this.name;
  }
}

const dev = new Developer("Joe");
console.log(dev.getName());
// dev.name = "New Name"; // Error: Cannot assign to 'name' because it is a read-only property.
```

---

## Question 23 - How to implement overriding in classes of TypeScript?

Overriding allows a subclass to provide a specific implementation of a method already defined in its base class using the `override` keyword.

```typescript
interface Shape {
  getArea: () => number;
}

class Rectangle implements Shape {
  public constructor(protected readonly width: number, protected readonly height: number) {}

  public getArea(): number {
    return this.width * this.height;
  }

  public toString(): string {
    return `Rectangle width is ${this.width} and height is ${this.height}`;
  }
}

class Square extends Rectangle {
  public constructor(width: number) {
    super(width, width);
  }

  public override toString(): string {
    return `Square width is ${this.width}`;
  }
}

const square = new Square(20);
console.log(square.toString()); // Output: Square width is 20
```

---

## Question 24 - What are Abstract classes?

Abstract classes are base classes that cannot be directly instantiated with `new`. They serve as blueprints containing abstract methods (which subclasses must implement) and concrete methods.

```typescript
abstract class Polygon {
  public abstract getArea(): number;

  public toString(): string {
    return `Polygon area is ${this.getArea()}`;
  }
}

class Rectangle extends Polygon {
  public constructor(protected width: number, protected height: number) {
    super();
  }

  public getArea(): number {
    return this.width * this.height;
  }
}

const rect = new Rectangle(10, 20);
console.log(rect.getArea()); // Output: 200
console.log(rect.toString()); // Output: Polygon area is 200
```

---

## Question 25 - What are Singleton classes?

The Singleton pattern ensures a class has only one instance while providing a global access point to that instance.

```typescript
class Coder {
  private static instance: Coder;

  private constructor() {}

  static getInstance(): Coder {
    if (!this.instance) {
      this.instance = new Coder();
    }
    return this.instance;
  }
}

const c1 = Coder.getInstance();
const c2 = Coder.getInstance();

console.log(c1 === c2); // Output: true
```

---

## Question 26 - What are Generics in TypeScript? Give examples in functions, classes and type aliases.

Generics introduce type parameter placeholders, enabling component reuse with strict type preservation across multiple data types.

```typescript
// Generic Function
function createArray<S, T>(arg1: S, arg2: T): [S, T] {
  return [arg1, arg2];
}
console.log(createArray<string, number>('JS', 42)); // Output: ['JS', 42]

// Generic Class
class GenericClass<T> {
  private _value: T | undefined;

  constructor(private name: string) {}

  public setValue(value: T): void {
    this._value = value;
  }

  public getValue(): T | undefined {
    return this._value;
  }

  public toString(): string {
    return `${this.name}: ${this._value}`;
  }
}

const genericVal = new GenericClass<number>('theNum');
genericVal.setValue(10);
console.log(genericVal.toString()); // Output: theNum: 10

// Generic Type Alias
type Numeric<T> = { value: T };
const ageValue: Numeric<number> = { value: 40 };
```

---

## Question 27 - What is Partial utility type?

`Partial<T>` turns all properties of type `T` into optional fields.

```typescript
interface Rectangle {
  width: number;
  height: number;
}

let rectPart: Partial<Rectangle> = {};
rectPart.width = 100;

console.log(rectPart); // Output: { width: 100 }
```

---

## Question 28 - What is Required utility type?

`Required<T>` constructs a type with all properties of `T` marked as required.

```typescript
interface Car {
  make: string;
  model: string;
  mileage?: number;
}

let myCar: Required<Car> = {
  make: 'Tata',
  model: 'Tiago',
  mileage: 20 // Mandatory because of Required<Car>
};

console.log(myCar); // Output: { make: 'Tata', model: 'Tiago', mileage: 20 }
```

---

## Question 29 - What is Record utility type?

`Record<K, T>` constructs an object type whose key keys are `K` and whose property values are `T`.

```typescript
const nameAgeObj: Record<string, number> = {
  'Joe': 42,
  'Ram': 41
};

console.log(nameAgeObj); // Output: { Joe: 42, Ram: 41 }
```

---

## Question 30 - What is Omit utility type?

`Omit<T, K>` constructs a type by picking all properties from `T` and removing keys specified in `K`.

```typescript
interface Coder {
  name: string;
  age: number;
  location?: string;
}

const nabs: Omit<Coder, 'age' 'location' |> = {
  name: 'Joe'
};

console.log(nabs); // Output: { name: 'Joe' }
```

---

## Question 31 - What is Pick utility type?

`Pick<T, K>` constructs a type by picking only the set of properties `K` from type `T`.

```typescript
interface Coder {
  name: string;
  age: number;
  location?: string;
}

const nabs: Pick<Coder, 'name'> = {
  name: 'Joe'
};

console.log(nabs); // Output: { name: 'Joe' }
```

---

## Question 32 - What is Exclude utility type?

`Exclude<UnionType, ExcludedMembers>` excludes specific types from a union.

```typescript
type Marks = string | number | boolean;

const value: Exclude<Marks, boolean> = '56';

console.log(typeof value); // Output: string
```

---

## Question 33 - What is Readonly utility type?

`Readonly<T>` constructs a type with all properties of `T` set to `readonly`.

```typescript
interface Person {
  name: string;
  age: number;
}

const person: Readonly<Person> = {
  name: "Joe",
  age: 42
};

// person.name = 'Parag'; // Error: Cannot assign to 'name' because it is a read-only property.
```

---

## Question 34 - What is Nullish Coalescing?

The nullish coalescing operator (`??`) returns its right-hand side operand when its left-hand side operand is `null` or `undefined`, preserving valid falsy values like `0`, `""`, or `false`.

```typescript
function showMileage(mileage: number | null | undefined) {
  console.log(`Mileage: ${mileage ?? 'NA'}`);
}

showMileage(null);      // Output: 'Mileage: NA'
showMileage(0);         // Output: 'Mileage: 0'
showMileage(undefined); // Output: 'Mileage: NA'
```

---

## Question 35 - How do Conditional Types and the `infer` keyword work in TypeScript?

Conditional types select one of two possible types based on a condition expressed as a type relationship test: `T extends U ? X : Y`. The `infer` keyword introduces a type variable within the `extends` clause to extract or infer subtype structures.

```typescript
// Extracting return type of a function type
type MyReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

function getUser() {
  return { id: 101, name: "Alice" };
}

type UserResult = MyReturnType<typeof getUser>; 
// UserResult is inferred as { id: number; name: string }

// Extracting element type from Array or Promise
type Unpack<T> = T extends Array<infer U> ? U : T extends Promise<infer U> ? U : T;

type ArrayItem = Unpack<string[]>; // string
type PromiseValue = Unpack<Promise<number>>; // number
```

---

## Question 36 - What are Template Literal Types and Mapped Types with Key Remapping in TypeScript?

Template literal types build upon string literal types to produce new string union types. Combined with mapped types and the `as` clause, key remapping allows modifying property key names.

```typescript
type Event = 'click' | 'hover';
type CallbackName = `on${Capitalize<Event>}`; // 'onClick' | 'onHover'

interface State {
  name: string;
  age: number;
}

// Generate Getters automatically from Interface keys
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

type StateGetters = Getters<State>;
// { getName: () => string; getAge: () => number; }
```

---

## Question 37 - Explain `tsconfig.json` strictness flags and their production importance.

Modern production-ready TypeScript projects enable `"strict": true`, which turns on the following critical type-safety rules:

1. `strictNullChecks`: Disallows implicit `null` and `undefined` assignments to non-nullable types.
2. `noImplicitAny`: Raises errors on expressions and declarations with an implied `any` type.
3. `strictPropertyInitialization`: Ensures class properties are initialized in the constructor or declared with undefined types.
4. `noImplicitThis`: Raises an error when `this` expressions evaluate to an implicit `any`.
5. `strictFunctionTypes`: Enforces strict type checking for function parameters (contravariant parameter checking).
6. `exactOptionalPropertyTypes`: Ensures optional properties (`prop?: string`) do not accept explicit `undefined` unless explicitly declared (`string | undefined`).

---

## Question 38 - What are the key performance and language updates introduced in Modern PHP (8.x)?

Modern PHP introduces high-performance, strictly typed paradigms:

1. **JIT (Just-In-Time) Compiler:** Compiles Opcodes into machine code at runtime for CPU-bound computations.
2. **Constructor Property Promotion:** Simplifies class property declarations.
3. **Match Expressions:** Expressions that evaluate values with strict comparison (`===`) and return values directly.
4. **Attributes (PHP Annotations):** Native, structured metadata for classes, functions, and properties.
5. **Fibers & Asynchronous Execution:** Low-level coroutines enabling non-blocking concurrency frameworks (e.g., Swoole, FrankenPHP, ReactPHP).

```php
<?php
declare(strict_types=1);

namespace App\Services;

enum UserStatus: string {
    case Active = 'active';
    case Suspended = 'suspended';
}

#[Attribute(Attribute::TARGET_CLASS)]
class ServiceRoute {
    public function __construct(public string $path) {}
}

#[ServiceRoute('/api/v1/users')]
class UserService {
    // Constructor Property Promotion
    public function __construct(
        private readonly UserRepository $repository,
        private readonly LoggerInterface $logger
    ) {}

    public function handleStatus(UserStatus $status): string {
        // Match expression
        return match($status) {
            UserStatus::Active => 'User is fully active.',
            UserStatus::Suspended => 'User access has been restricted.',
        };
    }
}
```

---

## Question 39 - What is PSR (PHP Standard Recommendation) compliance, and which PSRs are mandatory for enterprise PHP engineering?

PSR standards defined by the PHP-FIG (Framework Interoperability Group) guarantee architectural consistency across libraries:

* **PSR-1 & PSR-12 / PER-CS:** Coding Standards & Style Guidelines.
* **PSR-4:** Autoloading standard linking namespaces to physical directory paths.
* **PSR-7 & PSR-15:** HTTP Message Interface and HTTP Middleware standards.
* **PSR-11:** Container Interface standard for Dependency Injection containers.
* **PSR-6 & PSR-16:** Caching interfaces (PSR-6 Item Pool & PSR-16 Simple Cache).

---

## Question 40 - How do Async PHP engines like Swoole and FrankenPHP differ from traditional PHP-FPM execution models?

* **PHP-FPM (Shared-Nothing Architecture):** Boots up the framework lifecycle (composer autoloading, configuration parsing, container booting) on *every single request* and tears down everything when execution completes.
* **Async PHP (Swoole / FrankenPHP / RoadRunner):** Boots the application framework into RAM *once*. Requests execute within non-blocking event loops or persistent worker threads without rebooting the application runtime, delivering lower latencies and higher throughput.

---

## Question 41 - How do you construct Custom Gutenberg Blocks using React, TypeScript, and `@wordpress/scripts`?

Modern WordPress development relies on React components registered via `block.json` and bundled using `@wordpress/scripts`.

```json
{
  "$schema": "https://schemas.wp.org/trunk/block.json",
  "apiVersion": 3,
  "name": "custom-plugin/hero-banner",
  "title": "Hero Banner",
  "category": "layout",
  "icon": "superhero",
  "attributes": {
    "title": { "type": "string", "default": "" }
  },
  "editorScript": "file:./index.js",
  "style": "file:./style-index.css"
}

```

```typescript
// src/index.tsx
import { registerBlockType } from '@wordpress/blocks';
import { RichText, useBlockProps } from '@wordpress/block-editor';

registerBlockType('custom-plugin/hero-banner', {
  edit: ({ attributes, setAttributes }: { attributes: { title: string }; setAttributes: (attr: object) => void }) => {
    const blockProps = useBlockProps();
    return (
      <div {...blockProps}>
        <RichText onChange="{(title)" placeholder="Enter Banner Title..." tagName="h2" value="{attributes.title}"> setAttributes({ title })}
        />
      </div>
    );
  },
  save: ({ attributes }: { attributes: { title: string } }) => {
    const blockProps = useBlockProps.save();
    return (
      <div {...blockProps}>
        <RichText.Content tagName="h2" value="{attributes.title}"/>
      </div>
    );
  }
});
```

---

## Question 42 - How do you build Headless WordPress architectures using GraphQL (WPGraphQL) or REST API with Next.js/React frontend?

Headless WordPress isolates the WP admin interface as a Content Management System while serving content dynamically via APIs to decoupled frontends:

1. **WPGraphQL / REST API:** WordPress exposes endpoint schemas for posts, custom post types, and ACF (Advanced Custom Fields).
2. **Next.js Frontend:** Fetches data statically (`getStaticProps` / Incremental Static Regeneration) or via Server Components.
3. **Preview & Webhooks:** WordPress triggers webhooks on post save events to revalidate frontend cache paths automatically.

---

## Question 43 - How do you optimize high-traffic WordPress databases using `$wpdb` prepared statements and indexing strategies?

To prevent SQL Injection vulnerabilities and slow queries on high-traffic sites:

```php
<?php
// Secure prepared database execution using $wpdb
global $wpdb;

$table_name =$wpdb->prefix . 'custom_analytics';
$user_status = 'active';$limit = 10;

// Prepared SQL prevents SQL injection
$results = $wpdb->get_results($wpdb->prepare(
        "SELECT id, user_id, event_type, created_at 
         FROM {$table_name} 
         WHERE status = %s 
         ORDER BY created_at DESC 
         LIMIT %d",
        $user_status,$limit
    )
);
```

Database Optimization Strategies:

1. Avoid `meta_query` with unindexed text searches or `LIKE '%val%'` pattern matching on `wp_postmeta`.
2. Add composite indexes on custom database tables for queried columns (`status`, `created_at`).
3. Leverage Object Caching (`wp_cache_get`, `wp_cache_set`) backed by Redis or Memcached to store heavy query results.

---

## Question 44 - Compare Monolithic Architecture, Microservices, and Modular Monoliths. How do you choose between them?

| Architectural Pattern | Advantages | Trade-offs | Ideal Use Case |
| --- | --- | --- | --- |
| **Monolithic Architecture** | Simple deployment, easier debugging, transactional consistency. | Tight coupling, scaling entire application required. | Early-stage startups, rapid MVP development. |
| **Modular Monolith** | Clear domain boundaries, shared infrastructure, easy transition path to microservices. | Requires strict discipline to enforce domain boundaries. | Growing medium-to-large engineering teams. |
| **Microservices** | Independent deployment, tech stack flexibility, horizontal scaling per domain. | High operational complexity, distributed tracing, network latency. | Enterprise platforms with isolated teams and massive scale requirements. |

---

## Question 45 - What are distributed caching strategies (e.g., Cache-Aside, Write-Through, Read-Through), and how do you prevent Cache Stampedes?

* **Cache-Aside (Lazy Loading):** Application reads from cache; on cache miss, reads from DB and populates cache.
* **Write-Through:** Application writes to cache, and cache synchronously writes to DB.
* **Read-Through:** Application queries cache service directly; cache loads missing data from DB automatically.
* **Preventing Cache Stampedes (Thundering Herd):**
1. **Mutex Locking (Distributed Locks):** Ensure only one process queries the DB on cache miss while others wait.
2. **Probabilistic Early Expiration (XFetch Algorithm):** Recomputes and updates cache keys prior to absolute TTL expiration based on traffic volume.

---

## Question 46 - How do Project References (`composite`, `tsBuildInfo`) optimize compilation times in large TypeScript monorepos?

Project References allow a monorepo codebase to be split into smaller, independent projects. When configured, TypeScript uses `.tsbuildinfo` files to perform incremental builds, recompiling only modified modules and their immediate dependencies rather than parsing the entire project root.

```json
// packages/core/tsconfig.json
{
  "compilerOptions": {
    "composite": true,
    "declaration": true,
    "declarationMap": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"]
}
```

```json
// packages/api/tsconfig.json
{
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "references": [
    { "path": "../core" }
  ]
}
```

---

## Question 47 - How do you integrate modern PHP 8.x DTOs with TypeScript types to guarantee end-to-end type safety?

To keep full-stack PHP (Laravel/Symfony) and TypeScript applications synchronized, developers use automated generator tools like Spatie PHP to TypeScript or OpenAPI generator tools. These tools convert PHP Data Transfer Objects (DTOs) into TypeScript interface declarations during CI/CD pipelines.

```php
<?php
// PHP 8.2 Backend DTO
namespace App\Data;

use Spatie\LaravelData\Data;

class UserData extends Data
{
    public function __construct(
        public int $id,
        public string $name,
        public string $email,
        public ?string $profilePictureUrl
    ) {}
}

```

```typescript
// Automatically Generated TypeScript Interface
export interface UserData {
  id: number;
  name: string;
  email: string;
  profilePictureUrl: string | null;
}
```

---

## Question 48 - How do you optimize high-concurrency WordPress applications using Redis Object Caching and custom database tables?

1. **Custom Database Tables:** Bypasses `wp_postmeta` EAV query bottlenecks by defining structured custom MySQL tables with indexed lookup keys.
2. **Persistent Object Caching:** Caches high-frequency read queries in Redis or Memcached using `wp_cache_get` and `wp_cache_set` hooks to prevent direct database lookups.
3. **Database Prepared Operations:** Wraps queries in `$wpdb->prepare` to guarantee SQL-injection protection and uniform execution plan reuse.

```php
<?php
// High-performance query with Redis caching layer
function get_user_analytics(int $userId): array {
  $cacheKey = "user_analytics_{$userId}";
  $cachedData = wp_cache_get($cacheKey, 'analytics_group');

  if ($cachedData !== false) {
      return $cachedData;
  }

  global $wpdb;
  $table =$wpdb->prefix . 'user_analytics';
  
  $query =$wpdb->prepare(
      "SELECT metric_key, metric_value FROM {$table} WHERE user_id = %d",
      $userId
  );
  
  $results = $wpdb->get_results($query, ARRAY_A);
  
  // Store in cache for 1 hour (3600 seconds)
  wp_cache_set($cacheKey,$results, 'analytics_group', 3600);
  
  return $results;
}
```

---

## Question 49 - What are the key considerations when migrating a legacy PHP/WordPress monolith into a decoupled Headless Architecture using Next.js?

1. **Authentication & Session Persistence:** Transition from PHP native sessions (`PHPSESSID`) or cookie authentication to JWTs or OAuth2 tokens issued through WP-GraphQL / REST endpoints.
2. **Revalidation & Build Performance:** Utilize Next.js Incremental Static Regeneration (ISR) and WordPress post-save webhooks (`save_post`) to revalidate individual static pages without trigger total rebuilds.
3. **SEO & Metadata Parity:** Map RankMath or Yoast SEO metadata endpoints from WordPress directly into Next.js dynamic metadata objects (`generateMetadata`).

---

# 1. Best Practices

## JavaScript: Dynamic Typing
JavaScript is known for its flexibility due to its dynamic typing system.

### What is Dynamic Typing?
In JavaScript, variable types do not need to be declared explicitly when writing code, and the data type of a variable can change dynamically at runtime.

```javascript
let data = 42; // Initially a number
console.log(`The type of data is: ${typeof data}`); // Output: "number"

data = "Hello, World!"; // Now a string
console.log(`The type of data is: ${typeof data}`); // Output: "string"

data = { name: "Alice" }; // Now an object
console.log(`The type of data is: ${typeof data}`); // Output: "object"
```

## TypeScript: Static Typing

TypeScript introduces static typing to the JavaScript ecosystem.

### What is Static Typing?

A statically typed language requires variable types to be declared or inferred at compile time. Once a variable is assigned a type, it cannot change, allowing type mismatches to be caught during compilation rather than at runtime.

```typescript
let data: number = 42; // Declared as a number
console.log(`The type of data is: ${typeof data}`); // Output: "number"

// @ts-expect-error Type 'string' is not assignable to type 'number'
data = "Hello, World!"; // Compile-time error!
```

---

## Consistent Naming Conventions

Maintain strict naming standards across the codebase to ensure consistency and maintainability:

* **Interfaces:** `PascalCase` (e.g., `Address`, `UserProfile`)
* **Variables & Functions:** `camelCase` (e.g., `sendEmail`, `currentUser`)
* **Types:** `PascalCase` (e.g., `ApiResponse`, `UserRole`)

---

## Prefer `unknown`, Avoid `any`

### Bad Paradigm (`any`)

Using `any` disables all type-checking capabilities, allowing potential runtime errors to pass through unnoticed.

```typescript
// Bad: Disables all type safety
function processData(data: any) {
  return data.name; // Unsafe: 'data' could be null, undefined, or missing 'name'
}
```

### Good Paradigm (`Data` Interface & Narrowing)

Define structured types or use `unknown` to force runtime type validation before operating on untyped data.

```typescript
// Good: Strictly defined shape
interface Data {
  name: string;
  value?: number;
}

function processData(data: Data): string {
  return data.name;
}
```

* `any` disables all type checking.
* `unknown` forces type checking before performing operations on the value, making it type-safe and preventing runtime exceptions.

Use `unknown` for untyped inputs (such as raw API responses) and narrow the type using type guards:

```typescript
function isData(obj: unknown): obj is Data {
  return (
    typeof obj === "object" && obj !== null && "name" in obj &&
    typeof (obj as Data).name === "string"
  );
}

function handleRawInput(input: unknown) {
  if (isData(input)) {
    console.log(input.name); // Safe: TypeScript knows 'input' is 'Data'
  }
}
```

---

## Use `readonly` for Immutable Properties

Mark properties as `readonly` to prevent post-initialization modifications and enforce data integrity.

```typescript
interface Settings {
  readonly theme: string;
  readonly language: string;
}

const userSettings: Settings = {
  theme: "dark",
  language: "en"
};

// Error: Cannot assign to 'theme' because it is a read-only property.
// userSettings.theme = "light";
```

---

## Power of Type Guards

Type guards are expressions that perform runtime checks to narrow down types within a conditional block.

```typescript
// 1. typeof Type Guard (Used for primitive types: string, number, boolean, symbol, bigint, undefined, object, function)
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log("String ID:", id.toUpperCase());
  } else {
    console.log("Numeric ID:", id.toFixed(2));
  }
}

// 2. instanceof Type Guard (Used for class instances)
class Cat {
  meow() {
    console.log("Meow!");
  }
}

class Dog {
  bark() {
    console.log("Woof!");
  }
}

function speak(animal: Cat | Dog) {
  if (animal instanceof Cat) {
    animal.meow();
  } else {
    animal.bark();
  }
}

// 3. Custom Type Guards (Used for interfaces or custom object unions)
interface Feline {
  meow: () => void;
}

interface Canine {
  bark: () => void;
}

function isFeline(animal: Feline | Canine): animal is Feline {
  return (animal as Feline).meow !== undefined;
}

function makeSound(animal: Feline | Canine) {
  if (isFeline(animal)) {
    animal.meow(); // Safe: Narrowed to Feline
  } else {
    animal.bark(); // Safe: Narrowed to Canine
  }
}
```

---

## Avoid Non-Null Assertions

Avoid using the non-null assertion operator (`!`) to silence compiler warnings. Instead, use explicit narrowing or optional chaining (`?.`).

```typescript
declare function getUsername(): string | null;

// Bad: Silences the compiler, risks runtime crashes if null
const usernameBad: string | null = getUsername();
console.log(usernameBad!.toUpperCase());

// Good: Type-safe runtime verification
const usernameGood: string | null = getUsername();
if (usernameGood !== null) {
  console.log(usernameGood.toUpperCase());
}
```

---

## Tuple Types

A tuple is a fixed-length array where each index corresponds to a specific type.

```typescript
function transform(input: string): [string, string] {
  const x = input.slice(0, Math.floor(input.length / 2));
  const y = input.slice(Math.floor(input.length / 2));
  return [x, y];
}

const [leftHalf, rightHalf] = transform('first string');
console.log(leftHalf, rightHalf);

```

---

## Primitive Types vs. Wrapper Types & Object Constraints

* **Primitive types (`string`, `boolean`, `number`):** Represent raw, immutable values.
* **Wrapper types (`String`, `Boolean`, `Number`):** Represent object instances created via constructors (`new String("...")`). Always use lower-case primitive annotations.

### Object Type Differences:

* `Object`: Extremely broad type. Represents any value that inherits from `Object.prototype`, including primitives (excluding `null` and `undefined`).
* `{}`: Represents any non-nullable value (everything except `null` and `undefined`).
* `object`: Represents any non-primitive type (excludes `string`, `number`, `boolean`, `symbol`, `bigint`, `null`, `undefined`).

```typescript
let primitive: string = "hello";
let wrapper: String = new String("hello");

console.log(typeof primitive); // "string"
console.log(typeof wrapper);   // "object"

function logValue(val: {}): void {
  console.log(val);
}

logValue(42);        // Allowed
logValue("hello");   // Allowed
// logValue(null);      // Error: Argument of type 'null' is not assignable to parameter of type '{}'.
// logValue(undefined); // Error: Argument of type 'undefined' is not assignable to parameter of type '{}'.

function logObject(val: object): void {
  console.log(val);
}

logObject({});       // Allowed
logObject([]);       // Allowed
// logObject("hello");  // Error: Argument of type 'string' is not assignable to parameter of type 'object'.
```

---

## Prefer Interfaces for Object Shapes

Interfaces are ideal for defining object shapes and supporting declaration merging, whereas type aliases are best suited for unions, intersections, and primitive aliases.

```typescript
interface User {
  id: number;
  name: string;
}

const user: User = { id: 1, name: "Alice" };
```

---

## Prefer `type` for Union and Intersection Types

Use type aliases when defining complex structural compositions like unions and intersections.

```typescript
type SuccessResponse = { success: true; data: string };
type ErrorResponse = { success: false; error: string };
type APIResponse = SuccessResponse | ErrorResponse;
```

---

## Leverage Utility Types for Reusability

TypeScript provides several built-in utility types to transform existing types efficiently:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

// Partial<T>: Makes all properties optional
const draft: Partial<User> = { name: "John" };

// Pick<T, K>: Selects specific keys
const profile: Pick<User, "email" "name" |> = { name: "John", email: "j@example.com" };

// Omit<T, K>: Removes specific keys
const userWithoutEmail: Omit<User, "email"> = { 
  id: 1, 
  name: "John" 
};

// Record<K, T>: Creates an object type with keys K and value types T
type Role = "admin" | "editor" | "viewer";
const permissions: Record<Role, boolean> = {
  admin: true,
  editor: true,
  viewer: false,
};

// Exclude<T, U>: Removes types from a union
type Status = "pending" | "approved" | "rejected";
type FinalStatus = Exclude<Status, "pending">; // "approved" | "rejected"

// ReturnType<T>: Extracts the return type of a function
function createUser() {
  return { id: 1, name: "Alice" };
}
type CreatedUser = ReturnType<typeof createUser>; // { id: number; name: string }
```

---

## Prefer `type` + Union Over `enum`

While enums are useful, string unions are often cleaner, lightweight, tree-shakable, and eliminate runtime overhead.

```typescript
type Status = "pending" | "approved" | "rejected";

function updateStatus(status: Status) {
  console.log("Status:", status);
}

updateStatus("approved"); // Valid
// updateStatus("done");     // Error: Argument of type '"done"' is not assignable to parameter of type 'Status'.
```

---

## Use Enums or Const Assertions for Constants

For fixed sets of immutable constant values, use standard `enum` definitions or array/object `as const` assertions.

```typescript
// 1. Using enum
enum UserRole {
  Admin = "admin",
  Editor = "editor",
  Viewer = "viewer",
}
const user1: { role: UserRole } = { role: UserRole.Admin };

// 2. Using const assertion (Zero runtime code overhead)
const UserRoles = ["admin", "editor", "viewer"] as const;
type UserRoleType = typeof UserRoles[number]; // "admin" | "editor" | "viewer"
const user2: { role: UserRoleType } = { role: "admin" };

// Enum declaration
enum StatusEnum {
  Success = "success",
  Failure = "failure",
}

// Const assertion for literal derivation
const StatusesList = ["success", "failure"] as const;
type StatusLiteral = typeof StatusesList[number];
```

---

## Use Optional Chaining (`?.`) and Nullish Coalescing (`??`)

* **Optional Chaining (`?.`):** Accesses deeply nested properties safely without manual `null` or `undefined` checks.
* **Nullish Coalescing (`??`):** Provides fallback values specifically when the expression evaluates to `null` or `undefined` (unlike `||`, which triggers on falsy values like `0` or `""`).

```typescript
const user = { profile: { name: "Alice" }, settings: undefined };

console.log(user?.profile?.name); // "Alice"
console.log(user?.settings?.theme); // undefined (no error thrown)
console.log(user?.profile?.name ?? "Guest"); // "Alice"

const user2: { profile?: { name?: string } } = {};
console.log(user2?.profile?.name ?? "Guest"); // "Guest"
```

---

## Harness `keyof` and `typeof` for DRY Code

Combine `keyof` and `typeof` with `as const` assertions to dynamically derive strict TypeScript types directly from single-source-of-truth object configurations.

```typescript
const SUPPORTED_LANGUAGES = {
  en: "English",
  es: "Spanish",
  fr: "French",
  de: "German"
} as const;

// Derive types dynamically
type LanguageCode = keyof typeof SUPPORTED_LANGUAGES; // 'en' | 'es' | 'fr' | 'de'
type LanguageName = typeof SUPPORTED_LANGUAGES[LanguageCode]; // "English" | "Spanish" | "French" | "German"

function getLanguageName(code: LanguageCode): LanguageName {
  return SUPPORTED_LANGUAGES[code];
}

function isLanguageSupported(code: string): code is LanguageCode {
  return code in SUPPORTED_LANGUAGES;
}
```

---

## Centralize Types in Dedicated Files

Organize shared interfaces and type definitions inside designated `types/` directories and export them cleanly using barrel files (`index.ts`).

```bash
src/
├── types/
│   ├── index.ts
│   ├── user.ts
│   └── product.ts
├── components/
│   └── ProductList.tsx

```

```typescript
// src/types/index.ts
export * from "./user";
export * from "./product";
```

---

## Lint Your Types

Integrate TypeScript with ESLint to enforce consistent code style and identify common pitfalls automatically.

```bash
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

```json
{
  "parser": "@typescript-eslint/parser",
  "plugins": [
    "@typescript-eslint"
  ],
  "extends": [
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "@typescript-eslint/explicit-function-return-type": "warn",
    "@typescript-eslint/no-explicit-any": "error"
  }
}
```

---

## Proper Error Handling with Custom Types

Using discriminated unions for API error handling eliminates ambiguous runtime conditions and avoids relies on arbitrary `try/catch` logic or throwing unhandled errors.

```typescript
type ApiResult<T> = 
  | { success: true; data: T } 
  | { success: false; error: string };

interface User {
  id: number;
  name: string;
}

function fetchUser(id: number): ApiResult<User> {
  if (id === 1) {
    return { success: true, data: { id: 1, name: "John" } };
  }
  return { success: false, error: "User not found" };
}

// Usage
const result = fetchUser(2);

if (result.success) {
  console.log("User:", result.data.name);
} else {
  console.error("Error:", result.error);
}

type ValidationResult = 
  | { valid: true; value: string }
  | { valid: false; reason: string };

function validateEmail(email: string): ValidationResult {
  return email.includes("@")
    ? { valid: true, value: email }
    : { valid: false, reason: "Invalid email format" };
}
```

---

## Write Small, Reusable Types and Interfaces

Modular, composed interfaces prevent monolithic type structures and improve readability across components.

```typescript
interface Address {
  street: string;
  city: string;
  zipCode: string;
}

interface User {
  name: string;
  address: Address;
}
```

---

## Enable Strict Mode for Maximum Safety

Enabling `"strict": true` in `tsconfig.json` turns on all strict type-checking flags, catching subtle bugs during compilation.

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "es2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src"
  }
}
```

## Master Production `tsconfig.json` Configuration

Below is an enterprise-ready `tsconfig.json` template incorporating strict safety flags:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "lib": ["ES2022", "DOM"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitOverride": true,
    "noPropertyAccessFromIndexSignature": true,
    "noFallthroughCasesInSwitch": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

## `noUncheckedIndexedAccess`

By default, TypeScript assumes that indexed array or dictionary accesses always return the declared type. Enabling `noUncheckedIndexedAccess` automatically appends `| undefined` to indexed lookup types.

### Without the flag:

```typescript
const names: string[] = ["Alice", "Bob"];
const thirdName = names[2]; // TypeScript infers type as 'string'
// console.log(thirdName.toUpperCase()); // Throws runtime TypeError: Cannot read properties of undefined!

```

### With `noUncheckedIndexedAccess` enabled:

```typescript
const names: string[] = ["Alice", "Bob"];
const thirdName = names[2]; // TypeScript infers type as 'string | undefined'

if (thirdName !== undefined) {
  console.log(thirdName.toUpperCase()); // Safe execution
}
```

---

## `exactOptionalPropertyTypes`

Without this flag, optional properties (`bio?: string`) implicitly allow explicit `undefined` assignments (`bio: undefined`). Enabling this flag forces properties to either match their defined type or be omitted entirely.

### Without the flag:

```typescript
interface UserProfile {
  username: string;
  bio?: string;
}

const userBad: UserProfile = {
  username: "johndoe",
  bio: undefined // Allowed by default
};
```

### With `exactOptionalPropertyTypes` enabled:

```typescript
interface UserProfile {
  username: string;
  bio?: string;
}

// Error: Type 'undefined' is not assignable to type 'string'.
// const userGood: UserProfile = { username: "johndoe", bio: undefined };

const userCorrect: UserProfile = {
  username: "johndoe" // Correct: Property completely omitted
};
```

---

## `noImplicitOverride`

Enforcing `noImplicitOverride` requires the `override` keyword when extending and overriding base class methods, preventing silent breakages if the superclass implementation changes.

### Without the flag:

```typescript
class BaseAnimal {
  makeSound() {}
}

class DogSubclass extends BaseAnimal {
  makeSound() {} // Overrides method silently
}
```

### With `noImplicitOverride` enabled:

```typescript
class BaseAnimal {
  makeSound() {}
}

class DogSubclass extends BaseAnimal {
  override makeSound() {} // Correct: Explicitly declares method override
}
```

---

## `noPropertyAccessFromIndexSignature`

This flag prevents property lookups using dot notation (`obj.key`) on dynamic index signatures, requiring bracket notation (`obj['key']`) instead. This reserve dot notation exclusively for statically declared properties.

### Without the flag:

```typescript
interface AppSettings {
  theme: string;
  [key: string]: string; // Dynamic index signature
}

const settings: AppSettings = { theme: "dark" };
console.log(settings.tehme); // Typo allowed! Evaluates to 'undefined' at runtime
```

### With `noPropertyAccessFromIndexSignature` enabled:

```typescript
interface AppSettings {
  theme: string;
  [key: string]: string;
}

const settings: AppSettings = { theme: "dark" };

console.log(settings.theme); // Allowed: Statically declared property
// console.log(settings.tehme); // Error: Property 'tehme' comes from an index signature and must be accessed with ['tehme'].
console.log(settings["tehme"]); // Allowed: Bracket notation required for dynamic key lookups
```

---

## `noFallthroughCasesInSwitch`

Ensures switch cases include explicit `break`, `return`, or `throw` statements, preventing unintended execution fallthrough bugs.

### Without the flag:

```typescript
let executionState = "loading";

switch (executionState) {
  case "loading":
    console.log("Loading...");
    // Forgotten 'break' statement causes fallthrough!
  case "success":
    console.log("Success!");
    break;
}
// Outputs: "Loading..." AND "Success!"
```

### With `noFallthroughCasesInSwitch` enabled:

```typescript
let executionState = "loading";

switch (executionState) {
  case "loading":
    console.log("Loading...");
    break; // Correct: Prevents fallthrough error
  case "success":
    console.log("Success!");
    break;
}

```