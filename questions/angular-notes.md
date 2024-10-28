# What is Angular ?
Angular is a powerful front-end framework that is widely used for building dynamic web applications. It is open-source and maintained by Google. One of the key features of Angular is its use of TypeScript, a typed superset of JavaScript that makes code easier to read, maintain, and debug.

my-workspace/
├── ... #workspace-wide configuration files
├── src
│   ├── app
│   │   ├── app.module.ts #defines the root module, that tells Angular how to assemble the application
│   │   ├── app.component.ts #defines the logic for the application's root component
│   │   ├── app.component.html #defines the HTML template associated with the root component
│   │   ├── app.component.css #defines the base CSS stylesheet for the root component
│   │   ├── app.component.spec.ts #defines a unit test for the root component
│   │   └── app-routing.module.ts #provides routing capability for the application
│   ├── ...
│   ├── index.html #main HTML page, where the component will be rendered
├── angular.json #provides workspace-wide and project-specific configuration defaults
└── tsconfig.json #provides the base TypeScript configuration for projects in the workspace

# Angular 15:
- `Standalone Components:` Angular 15 introduced fully standalone components, allowing developers to create components without needing NgModules, simplifying the structure and dependencies of applications.
- `Directive Composition API:` This feature enables the reuse of directives by composing them together.
- `ESBuild Integration:` Improved build performance using ESBuild, making the build process faster and more efficient.
- `Router Improvements:` New routerLinkActiveOptions provides more control over link activation.
- `Forms Enhancements:` Improved handling of form controls, including new APIs for adding and removing form controls dynamically.

# Angular 16:
- `Non-Destructive Hydration:` Enhances server-side rendering by retaining server-rendered content and making it interactive without re-rendering it, improving performance and reducing flickering during page load​.
- `Signals:` Signals offer a new reactivity model for managing state changes, inspired by Solid.js. This helps in reducing the overhead of change detection and makes Zone.js optional​.
- `Language Service:` Enhanced Angular Language Service. 
- `Improved Developer Tools:` Angular DevTools for better debugging and code navigation.
- `Component Input Binding:` Allows route data to be directly bound to component inputs without needing to inject the ActivatedRoute service​
- `DestroyRef:` A new provider to register destroy callbacks for cleanup logic, improving memory management​
- `Caching:` Default caching with the ability to control the cache location via the ng cache command​
- `Angular Material Updates:` Introduction of a new date range picker and other enhancements to improve accessibility and performance.

# Angular 17:
- `ESLint Integration:` Angular 17 replaces TSLint with ESLint as the default linter, aligning with the broader TypeScript ecosystem.
- `Optional NgModules:` Angular 17 further simplifies app structures by making NgModules completely optional for more parts of the application.
- `Signals and Effects:` Angular 17 introduces a stable API for signals and effects, streamlining state management.
- `Router Enhancements:` Improved lazy loading and enhanced error handling in routing configurations.
- `Performance Improvements:` Ongoing performance enhancements, especially in build times and application startup, leveraging Webpack 5 and other optimizations.

## Changes in Angular 17
- Deferrable Views
- Improved Change Detection
- Built-in Control Flow Loops
- Custom Element Bindings and Providers
- Modernization and Tooling Updates

# Angular 18:
- `Enhanced Performance:` Faster load times and better runtime performance.
- `New Features:` Introduction of new components and APIs.
- `Improved Developer Experience:` Enhanced debugging tools and error messages.
- `Security Updates:` Patches for known vulnerabilities and improved security practices.
- `Better Compatibility:` Improved support for modern web standards and third-party libraries.

## What is shadow DOM ?
Shadow DOM is a web standard that allows for encapsulation of DOM elements within a host element. It provides a way to create a scoped subtree of DOM elements with its own styling and behavior. The encapsulated elements are isolated from the rest of the document, preventing styles and structure from leaking out or being affected by the surrounding page.

## What is view encapsulation ?
View encapsulation in Angular refers to how styles and templates are confined to a particular component's view.
View encapsulation is a concept ensures that the styles and structure of a component are encapsulated and scoped to that component.
It ensures that the styles and templates defined within a component do not affect other components in the application.

There are three types of view encapsulation in Angular:

`Emulated:` In an emulated view, the styles defined within a component are encapsulated within that component's view and do not affect other components.

`Native:` Native view encapsulation in Angular uses the browser's built-in Shadow DOM technology to isolate component styles and templates. This ensures they do not affect or get affected by other components.

`None:` With the none view encapsulation, styles defined in a component can affect the entire application.

## What are the design patterns in Angular?
These design patterns help developers create maintainable, scalable, and modular applications.

1. `Singleton Pattern:` Angular services are often implemented using the Singleton pattern. A service is instantiated once and shared across multiple components, allowing them to communicate and share data. To create a single service in angular
- `providedIn` property, create service add `providedIn: 'root'` property in the `@Injectable` decorator 
- `NgModule` providers arrays, by passing the service as a value to the `providers` array and if the `NgModule` is root app module

2. `Dependency Injection (DI) Pattern:` Angular utilizes the DI pattern to manage the dependencies between components and services. With DI, the required dependencies are provided to a component or service through constructor injection or property injection, promoting loose coupling and testability.

3. `Observer Pattern:` Angular leverages the Observer pattern through the EventEmitter class and the RxJS library. Components can emit events using EventEmitters, and other components can subscribe to these events

4. `Strategy Pattern:` The Strategy pattern enables you to dynamically select and switch between different strategies at runtime based on specific conditions or requirements. By encapsulating these behaviors in separate classes, components can switch between strategies based on specific conditions.

5. `Decorator Pattern:` Angular decorators, such as @Component and @Injectable, are based on the Decorator pattern. Decorators provide a way to enhance or modify the behavior of classes or class members without directly modifying the underlying code.

6. `Facade Pattern:` The Facade pattern is a structural design pattern that provides a simplified interface to a complex subsystem. In Angular, you can apply the Facade pattern to create a simplified API or service that encapsulates the complexity of interacting with multiple components, services, or modules.

7. `Composite Pattern:` The Composite Design Pattern is a structual design pattern that is used to compose objects into a tree-like structure. Components can be composed of other components, forming a tree-like structure. This pattern enables the creation of reusable and hierarchical UI components.

8. `Factory Pattern:` The Factory pattern is a creational design pattern that provides an interface for creating objects without specifying the exact class of the object that will be created. In Angular, you can apply the Factory pattern to encapsulate object creation logic and provide a centralized place for creating instances of different classes.

## What Is Linting?
Linting is basically an automated static code analysis of a code block for errors usually done by a tool known as a linter.

## What Is Static Analysis?
Static analysis is the practice of analyzing source code before it is running. Static analysis in JavaScript can drastically improve your code quality.

In compiled programming languages, static analysis might be built into the compiler, but in dynamically interpreted languages like JavaScript, static analysis tools must be configured to run on the code sometime before it is deployed.

### How Static Analysis Helps Improve Your Code ?
1. Formatting and Styling Code
2. Detecting Bugs and Errors
3. Enforcing Best Practices
4. Measuring Complexity
5. Analyzing Security Risks
6. Auditing Third-Party Dependencies
7. Checking Types

## What is Garbage Collector Angular ?
High-level and low-level languages allow us to have better memory management with the Garbage Collector (GC), which, in essence, monitors memory allocations and determines whether a certain memory block is or isn't necessary for us.

## Vendor.js :
The vendor.js file is generated during the build process of an Angular application. It contains all the external libraries and dependencies that your application relies on. These can include frameworks like Angular itself, as well as third-party libraries like RxJS, Angular Material, or Bootstrap. The purpose of bundling these dependencies into a single file is to optimize the application's loading time by reducing the number of network requests needed to fetch multiple JavaScript files.

By bundling the vendor dependencies together, the Angular build process creates a separate chunk that can be cached by the browser, resulting in faster subsequent loads of your application.

## Polyfill.js :
Polyfills play a vital role in ensuring that your Angular application runs smoothly across different browsers and versions. They provide modern JavaScript features to older browsers that do not natively support them. The polyfill.js file includes a collection of JavaScript code that "polyfills" or replicates missing features in older browsers.

Angular applications leverage features from the ECMAScript standards, and not all browsers support the latest specifications. Polyfills bridge this gap by enabling the use of modern JavaScript syntax, APIs, and functionalities in older browsers. The polyfill.js file is automatically generated during the build process, and its contents are based on the browser support configuration specified in the Angular project.

## Main.js :
The main.js file is the entry point of your Angular application. It contains the bootstrap logic that initializes and launches your Angular application. When the browser loads the main.js file, it triggers the Angular framework to start, which in turn bootstraps the root module of your application.

The main.js file is generated during the Angular build process. It includes the necessary code to set up the application environment, load the required modules, and configure the application for rendering in the browser. It also handles other essential tasks such as registering service workers for progressive web apps (PWAs) and enabling production optimizations like AOT (Ahead-of-Time) compilation.

## Runtime.js :
The runtime.js file is another crucial component of an Angular application. It provides the runtime environment necessary for the execution of your application. The file contains the core Angular runtime code, which enables Angular-specific functionalities such as change detection, dependency injection, and routing.

## What is zone js in angular?
Zone.js is a JavaScript library used in Angular to provide execution context and hooks into asynchronous operations. It allows Angular to track and manage the execution of asynchronous tasks, such as event handling, timers, promises, and XHR requests. Zone.js enables Angular to perform change detection and update the UI when asynchronous operations complete. Zone.js provides Angular with a way to seamlessly integrate asynchronous operations into the change detection mechanism, enabling efficient updating of the UI when asynchronous tasks finish

## What is the difference between AOT and JIT?

`AOT (Ahead-of-Time)` and `JIT (Just-in-Time)` are two compilation methods used in Angular. 
`JIT compilation` is used during development, provides a better debugging experience, but can impact the initial load time. 
`AOT compilation` is primarily used in production deployments, improves performance and security, and reduces bundle sizes, but has limitations on dynamic behaviors.

### What is `JIT (Just-in-Time):` ?

1. Compilation: JIT compilation happens at runtime in the user's browser. The Angular compiler runs in the browser and compiles the application's templates and components into JavaScript during the application's bootstrap process.
2. Development mode: JIT is primarily used during development as it allows for rapid iterations and immediate feedback. It supports features like hot module replacement, which speeds up the development process.
3. Performance: JIT compilation can impact the initial load time of the application because the compilation process happens at runtime. The browser needs to download the Angular compiler and perform the compilation process, which can lead to a slower startup time.
4. Debugging: JIT allows for better debugging experience as the browser can map the compiled code to the original TypeScript source files, enabling developers to debug directly in the browser's developer tools.

### What is `AOT (Ahead-of-Time):` ?

1. Compilation: AOT compilation occurs before the application is deployed. The Angular compiler runs on the developer's machine during the build process and generates pre-compiled JavaScript code. The compiled code includes templates and components converted into efficient JavaScript code.
2. Production mode: AOT is primarily used in production deployments to optimize the performance and load time of the application. It eliminates the need for the Angular compiler in the browser, resulting in faster startup times and smaller bundle sizes.
3. Performance: AOT significantly improves the initial load time of the application. The browser downloads pre-compiled JavaScript code, reducing the amount of work needed to be done at runtime.
4. Security: AOT provides a level of security by pre-compiling the templates and removing the Angular compiler from the client-side code. This mitigates the risk of template injection attacks.
5. Smaller bundle size: AOT allows for tree shaking, a process that eliminates unused code during the compilation phase. This leads to smaller bundle sizes and reduces the overall download size for users.
6. Limited dynamic behavior: AOT introduces some limitations on dynamic behaviors, such as dynamic template generation or dynamic component loading, as the templates and components are pre-compiled during the build process.

## What are annotations in Angular? 
Annotations in Angular are essential for defining components, services, directives, and other building blocks of an Angular application. They help Angular understand the structure and behavior of your application's elements. They are the metadata set on the class used to reflect the metadata library. 

Some of the most commonly used annotations (decorators) in Angular include:

- `@NgModule:` Annotates a class to specify that it is an Angular module and provides metadata about its dependencies, components, directives, pipes, and services.
- `@Component:` Annotates a class to define an Angular component, providing metadata such as its selector, template, and style.
- `@Directive:` Annotates a class to define an Angular directive, which allows you to add behavior to elements in the DOM.
- `@Pipe:` Annotates a class to define an Angular pipe, which transforms input data to a desired output format for display.
- `@Injectable:` Annotates a class to define an injectable service that can be injected into other components or services.
- `@Input:` Annotates a class property to allow data to be passed into a component from its parent component.
- `@Output:` Annotates a class property to allow a component to emit custom events to its parent component.
- `@ViewChild` and `@ViewChildren`: Annotates a class property to query and access child components or elements in the component's template.
- `@HostListener`: Annotates a class method to listen for events on the host element of a directive or component.
- `@HostBinding`: Annotates a class property to bind to a host element property or attribute in a directive or component.

```javascript
/* JavaScript imports */
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';

/* the AppModule class with the @NgModule decorator */
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## What is Angular change detection ?
Angular's change detection is a mechanism that detects and propagates changes in the application's data model to update the corresponding views.

## Observable in Angular
- In Angular, Observables are part of the Reactive Extensions for JavaScript (RxJS) library.
- Observables provide support for passing messages between parts of your application.
- Observables are a powerful feature used extensively in reactive programming to handle asynchronous operations and data streams.
- Observables provide a way to subscribe to and receive notifications when new data or events are emitted, enabling you to react to changes in real-time.

## Difference between Observables & Subjects ?
It is a part of the Reactive Extensions for JavaScript (RxJS) library, which is included by default in Angular projects.
Plain Observables are unicast (each subscribed Observer owns an independent execution of the Observable)
Subjects are multicast.

## Difference between cold observables and hot observables ?

Cold Observables:

A cold observable starts emitting values when a subscription is made. Each subscription to a cold observable triggers a separate execution of the observable's logic. The data stream is independent for each subscriber, and each subscriber receives the full sequence of emitted values from the beginning. Example Observable.

Hot Observables:

A hot observable emits values regardless of subscriptions. It starts emitting values immediately upon creation, and subscribers receive only values emitted after they subscribe. The data stream is shared among subscribers, and late subscribers may miss previously emitted values. Example Subject.

## Difference between ng add and  npm install ?

`ng add` is a convenient way to add Angular-specific packages or schematics to your project while automating the necessary setup steps.
`npm install` is a general-purpose command used to install any package from the npm registry into your project, regardless of the specific framework or library being used.

## What is DomSanitizer ?
DomSanitizer, a service of Angular helps to prevent attackers from injecting malicious client-side scripts into web pages, which is often referred to as Cross-site Scripting or XSS.