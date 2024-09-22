# What is TypeScript ?
TypeScript is an open-source programming language developed and maintained by Microsoft. It is a superset of JavaScript.

1. `Static Typing:` You can declare the types of variables, function parameters, and return types. This allows for better code quality, early error detection, and improved code documentation.

2. `Language Features:` It includes classes (ES6 also have classes but without public, protected,private modifiers), interfaces, enums, and modules. These features enable better code organization, encapsulation, and maintainability.

3. `Tooling and Ecosystem:` TypeScript has its own compiler (`tsc`) that transpiles TypeScript code into JavaScript, providing additional checks and optimizations.

4. `Compatibility:` TypeScript is a superset of JavaScript, any valid JavaScript code is also valid TypeScript code.

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

# How do you declare a variable in TypeScript ?
You can declare a variable using the `let` or `const` keyword, just like in JavaScript. However, TypeScript also allows you to specify the variable type using type annotations.

```typescript
// Declaring a variable with a type annotation
let myNumber: number = 42;

// Declaring a variable without a type annotation
let myString = "Hello, world!";

// Declaring a constant with a type annotation
const PI: number = 3.14;

// Declaring a variable with an array type
let myArray: string[] = ["apple", "banana", "orange"];

// Declaring a variable with an object type
let myObject: { name: string, age: number } = { name: "Alice", age: 30 };
```

# What is a union type in TypeScript ? 
A union type allows a variable to have more than one possible type. It is defined using the vertical bar `|` to separate the different types that are allowed. For example: `let myVariable: number | string;`

# How do you define an interface in TypeScript ?
An `interface` defines the structure of an object. It specifies the names and types of properties that an object.

# What is a namespace in TypeScript ?
A `namespace` is a way to group related code together and prevent naming conflicts. 

# How can you use decorators in TypeScript ?
`Decorators` provide a way to add metadata or behavior to classes, methods, properties, and other declarations at design time. Decorators are applied using the `@` symbol followed by the decorator function name, which can be defined separately. A decorator function takes one, two or three arguments depending on where it's applied:

- If the decorator is applied to a class, the decorator function takes one argument, which is the constructor function of the class.
- If the decorator is applied to a method, property, or parameter, the decorator function takes three arguments: the target object (the prototype of the class for instance members), the member name, and a property descriptor.
- If the decorator is applied to an accessor (getter or setter), the decorator function takes four arguments: the target object, the member name, the accessor descriptor, and an optional descriptor for the associated property.

```typescript
function timestamp(target: Function) {
    target.prototype.timestamp = new Date();
}
@timestamp
class MyClass {
}
const myInstance = new MyClass();
console.log(myInstance.timestamp); // logs the current date
```
# What are modifiers in TypeScript ?
In TypeScript, `public`, `private`, and `protected` modifiers to control the visibility and accessibility of class members (properties and methods). 

# What is a type assertion in TypeScript ?
Type assertion is a way to tell the compiler the type of a value.

# What is a module in TypeScript ?
A module is a way to organize code into reusable, self-contained units of code that can be imported and exported between different parts of an application. Modules can contain classes, functions, interfaces, and other code, and can be either internal to a project or external libraries.

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

## Accessing Object Properties and Methods
```typescript
console.log(car.brand); // Dot notation to access property
console.log(car['model']); // Bracket notation to access property
console.log(car.start()); // Accessing object method

car.color = 'blue'; // Adding a new property
car.year = 2023; // Modifying an existing property
delete car.model; // Removing a property
```

## Object Prototypes and Inheritance

### Inheritance through Prototypes
```typescript
function Vehicle(brand, model, year) {
    this.brand = brand;
    this.model = model;
    this.year = year;
}

Vehicle.prototype.start = function() {
    return `${this.brand} ${this.model} started!`;
};

function Car(brand, model, year, color) {
    Vehicle.call(this, brand, model, year); // Call the Vehicle constructor
    this.color = color;
}

// Set Car's prototype to an instance of Vehicle
Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car; // Reset constructor

Car.prototype.drive = function() {
    return `The ${this.color} ${this.brand} ${this.model} is driving!`;
};

let myCar = new Car('Toyota', 'Corolla', 2022, 'blue');

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
console.log(counter.count); // Output: undefined (count is private)
```

### Abstraction

Abstraction simplifies intricate realities by constructing classes or objects
```typescript
class Shape {
    constructor(color) {
        this.color = color;
    }

    // Abstract method
    calculateArea() {
        throw new Error('Method must be implemented');
    }
}

class Circle extends Shape {
    constructor(radius, color) {
        super(color);
        this.radius = radius;
    }

    calculateArea() {
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
    makeSound() {
        return 'Some generic sound';
    }
}

// Creating a subclass
class Dog extends Animal {
    makeSound() {
        return 'Woof!';
    }
}

// Creating instances
let genericAnimal = new Animal();
let dog = new Dog();

console.log(genericAnimal.makeSound()); // Output: Some generic sound
console.log(dog.makeSound()); // Output: Woof!
```

## Generics Functions

A generic function that works with any data type. The actual type is determined when the function is called, whether it’s a number, string, or any other type.
```typescript
function identity<T>(arg: T): T {
    return arg;
}

// Using the generic function with different types
let num = identity<number>(42); // T is number
let str = identity<string>("Hello"); // T is string

//TS automatically infers the type so this also works:
//let num = identity(42); // T is number
//let str = identity("Hello"); // T is string

const fetchData = <Type>(url: string): Promise<Type> => {
    return fetch(url)
}

fetchData<{name: string, age: number}>("/api/books")
.then(res => {
    console.log(res)
    // res: {name: string, age: number}
})
```

## Generic Interfaces

These types are specified when an instance is created and can be any type. The only constraint is that each property maintains its respective type.
```typescript
interface Pair<T, K> {
  first: T;
  second: K;
}

let pair: Pair<string, number> = { first: "one", second: 2 }
let anotherPair: Pair<number, string> = { first: 1, second: "second" }
let otherPair: Pair<number, number> = { first: 1, second: 2 }
```

## Generics Types

Generics can also be applied to custom types. The process is similar to interfaces: you define a shape with placeholder types, which are determined when the type is used.
```typescript
type Person<T, K, V> = {
  name: T,
  age: K,
  isMarried: V
}

const person1: Person<string, number, boolean> = {
  name: 'Bob',
  age: 33,
  isMarried: false
}
```

## Generic Classes

Generic classes allow you to define a blueprint where the data’s type is flexible, only being defined when an instance is created.
```typescript
class Box<T> {
  content: T

  constructor(content: T){
    this.content = content
  }

  getContent(): T {
    return this.content
  }
}

const letterBox = new Box('a')
const numberBox = new Box(1)
```

## Generics with built-in functions
```typescript
const numberSet = new Set<number>();
numberSet.add(1);
numberSet.add(2);
numberSet.add(3);
console.log(numberSet)

const stringToNumberMap = new Map<string, number>();
stringToNumberMap.set("one", 1);
stringToNumberMap.set("two", 2);
stringToNumberMap.set("three", 3);
console.log(stringToNumberMap)
```

## Constraints

Generics also support constraints, allowing you to limit the types that can be used.
```typescript
type LengthType = {length: number}
function getLength<T extends LengthType>(args: T){
  return args.length
}

getLength('abc')
getLength([1, 2])

//this doesn't work:
//getLength(2)
//getLength({"a": "b"})
```

## Utility types

Utility types in TypeScript use generics to create flexible and reusable type transformations. Utility types in Typescript are some predefined generic types that can be used to manipulate or create other new types

Different utility types are Partial,  Required,  Omit,  Pick,  Multiple utility types together,  Readonly,  Mutable,  Exclude,  Extract,  ReturnType,  Parameters,  NonNullable,  Awaited,  Awaited and ReturnType combined

```typescript
interface User {
  name: string;
  age: number;
  location: string;
}

// makes all properties optional 
const partialUser: Partial<User> = {name: 'Joe'}
console.log(partialUser);

// interface User = {
//   name?: string | undefined;
//   age?: number | undefined;
//   location?: string | undefined;
// }

type RequiredPerson = Required<User>;
let reqUser: RequiredPerson = {name: 'Joe', age: 33, location: 'India'}
console.log(reqUser);

// allows to omit properties: here the name property is omitted
const omitUser: Omit<User, 'name'> = {location: 'India', age: 33}
console.log(omitUser);

type Person = {
    name: string
    lastName: string
    age: number
    hobbies: string
}

type SomePerson = Pick<Person, "name" | "age">
const somePerson: SomePerson = {name: 'Joe', age: 33}
console.log(somePerson);

// type SomePerson = {
//  name: string;
//  age: number;
// }

type SomePerson = Omit<Person, "lastName" | "hobbies">
const somePerson: SomePerson = {name: 'Joe', age: 33}
console.log(somePerson);

// type SomePerson = {
//  name: string;
//  age: number;
// }

type ReadOnlyPerson = Readonly<Person>
const person: ReadOnlyPerson = {
    name: "Fizz",
    lastName: 'Alice',
    age: 33,
    hobbies: 'football'
}
console.log(person);

// type ReadOnlyPerson = {
//     readonly name: string;
//     readonly lastName: string;
//     readonly age: number;
//     readonly hobbies: string;
// }


```
Extract is a utitly type in Typescript that allows you to create a new type by extracting a subset of an existing type.

```typescript
// Get a member of an union
type Animal = "cat" | "dog" | "bird" | "fish";
type OnlyCat = Extract<Animal, "cat">; // 'cat'

// Get strings containing a substring from an union
type keys = 'userId' | 'tweetId' | 'userName' | 'tweetName'
type UserKey = Extract<keys, `${'user'}${string}`> // "userId" | "userName"

// Get members by a type
type allTypes = 'admin' | 'user' | 5 | 6 | 7 | true
type onlyNumbers = Extract<allTypes, number> // 5 | 6 | 7

// Get a member of a discriminated union
type Shape =
  {
    type: 'square'
  }
| {
    type: 'circle'
  }
| {
    type: 'triangle'
  }

type SqaureAndCircleShape = Extract<Shape, { type: 'square' | 'circle' }> // {type: 'square'; size: number;} | { type: 'circle' }
```

# What are solid principles in javascript?

SOLID principles are a set of guidelines that promote good software design and modular programming.The SOLID principles help in achieving code that is easier to maintain, test, and extend. 

1. `Single Responsibility Principle (SRP):` A class or module should have a single responsibility or reason to change. It states that a class should have only one job or responsibility, and it should encapsulate that responsibility. This principle promotes smaller, focused classes that are easier to understand and maintain.

```typescript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

class EmailSender {
  sendEmail(user, subject, message) {
    // Code for sending email
  }
}

class Database {
  saveUser(user) {
    // Code for saving user to the database
  }
}
```

2. `Open-Closed Principle (OCP):` Software entities (classes, modules, functions) should be open for extension but closed for modification. It means that you should be able to add new functionality to a module without modifying its existing code. By using techniques such as inheritance, interfaces, and dependency injection, you can achieve this principle.

```typescript
// Good example
class Shape {
  calculateArea() {
    throw new Error('calculateArea() method should be implemented in derived classes.');
  }
}

class Circle extends Shape {
  calculateArea() {
    // Code for calculating circle area
  }
}

class Rectangle extends Shape {
  calculateArea() {
    // Code for calculating rectangle area
  }
}
```

3. `Liskov Substitution Principle (LSP):` Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. This principle emphasizes that subclasses should be able to be used interchangeably with their base classes, without causing errors or unexpected behavior.

```typescript
class Shape {
  calculateArea() {
    throw new Error('calculateArea() method should be implemented in derived classes.');
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  calculateArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(side) {
    super();
    this.side = side;
  }

  calculateArea() {
    return this.side * this.side;
  }
}
```

4. Interface Segregation Principle (ISP): Clients should not be forced to depend on interfaces they do not use. This principle suggests that classes should not be forced to depend on interfaces they don't need. Instead, it's better to create smaller and more specific interfaces that are tailored to the requirements of the client.

```typescript
class Printer {
  print(document) {
    // Code for printing the document
  }
}

class Scanner {
  scan(document) {
    // Code for scanning the document
  }
}

class FaxMachine {
  fax(document) {
    // Code for faxing the document
  }
}
```

5. `Dependency Inversion Principle (DIP):` High-level modules should not depend on low-level modules. Both should depend on abstractions. This principle encourages the use of abstractions (interfaces or base classes) to define dependencies between modules. It helps in decoupling modules, making them more flexible, and allowing easier substitution of implementations.

```typescript
class UserService {
  constructor(database) {
    this.database = database;
  }

  getUser(userId) {
    return this.database.getUser(userId);
  }
}

class MongoDBDatabase {
  getUser(userId) {
    // Code for retrieving user from MongoDB database
  }
}

class MySQLDatabase {
  getUser(userId) {
    // Code for retrieving user from MySQL database
  }
}
```


Development front-controllers (app_dev.php etc.) removed during the deployment process and denied by the web server
Application is working correctly through https
E-mail addresses and credentials changed to production values
Cron scripts installed (if applicable)
Correctly configured shared files directories (we use Capistrano)
Confidential data like JWT Tokens, Security Tokens, API keys changed and not stored in repository
All assets minimized and compressed
RWD tests on Browserstack passed
OWASP top 10 tests passed
php.ini settings updated: time zone, max upload size etc.
Unnecessary Symfony bundles disabled
Custom 404 and 500 error pages
Custom favicon
Initial database migration added
Sentry/error tracking configured