## What are Angular features ?
- Interpolation
- Template Statements
- Event Binding
- Built-in Directives
- Pipes
- Property Binding
- Attribute, Class and Style Bindings

> One Way Binding(Interpolation):

Interpolation is way to bind dynamic values directly into the text content of HTML elements.
`<h1>{{ pageTitle }}</h1>`

> Two way binding:

Two way binding is a feature in Angular that synchronizes the Model(data) and the View(UI). This means the changes in the View automatically updates the Model, and the changes in the View are reflected back to the Model.

The `[(ngModel)]` directive is a shorthand for binding the value property of the `<input>` element to the field property of the component and listening for input events to update the field property.
```html
<input [(ngModel)]="username">
<input [value]="username" (input)="username = $event.target.value">
```

> Property binding:

Helps you set values for properties of HTML elements and directives
```html
<img [src]="imageUrl" alt="Image">
<td [colSpan]="1 + 1" style="background-color: yellow;">2 cols</td>
```

> Attribute, Class and Style Bindings:
```html
<td [attr.colspan]="1 + 1" style="background-color: yellow;">2 cols</td>
<p [style.color]="green ? 'green' : 'red'">hello world</p>
<p [class.green]="green" [class.red]="!green">hello world</p>
```

> Event binding:

Allows you to respond to user events (such as clicks, keypresses, etc.) by triggering methods in the component.
```html
<button (click)="onClick()">Click me</button>
<input (input)="onInputChange($event)">
```
This data binding type is when information flows from the view to the component when an event is triggered. The view sends the data from an event like the click of a button to be used to update the component.

## What is a Pipe ?
Pipes are simple functions to use in template expressions to accept an input value and return a transformed value. 

By default, pipes are "pure," the pipe will be called when the input values they depend on change. 
pipes are "impure" by setting the pure property to false. the pipe will be called in every change detection cycle, regardless of whether the input values have changed or not. 

### What is Template expression ?
Template expression typically produces a value within the double curly braces that Angular executes and binds to the property of a target being an HTML element, component or directive.

### What are Angular built-in pipes ?
Angular provides built-in pipes for typical data transformations, including transformations for internationalization (i18n), which use locale information to format data. 

- `DatePipeFormats` a date value according to locale rules.

- `UpperCasePipeTransforms` text to all upper case.

- `LowerCasePipeTransforms` text to all lower case.

- `CurrencyPipeTransforms` a number to a currency string, formatted according to locale rules.

- `DecimalPipeTransforms` a number into a string with a decimal point, formatted according to locale rules.

- `PercentPipeTransforms` a number to a percentage string, formatted according to locale rules.

## What is a Dependency Injection
This reduces the coupling between classes and their dependencies, making the code more maintainable, testable, and reusable.
```typescript
export class AppComponent {
  service = new RootService();
}

export class AppComponent {
  service = inject(RootService);
  constructor(private service: RootService) {}
}
```
It delegates this task to an external source, which is responsible for either returning an existing instance or creating a new instance of the requested service.
```typescript
export const inject = (searchClass: Class) => {
  const dependance = find(searchClass)
  if(dependance) {
    return dependance;
  } else {
    return new searchClass();
  }
}
```
The Injector stores information about all injectable classes, which includes anything with a decorator such as @Injectable, @Component, @Pipe, and @Directive

Angular has two categories of Injectors:

- `EnvironmentInjector:` This category includes all global injectable classes provided through the router, modules, or using the `providedIn: 'root'` keyword.
- `NodeInjector:` This category contains all local injectable classes found in each component or template.

## What Are Angular Signals?
Angular Signals represent a new way to build reactive apps, built on top of reactive primitives that emit updates when their underlying values change.

A signal is like a container that holds a value (like a number or a string) and tells other parts of your app when this value changes.

The Signals API is a small and easy-to-use API, with three main reactive primitives,

- `Writable signals:` These are signals that you can change. For example, if you have a signal for a number, you can increase or decrease this number.
- `Computed signals:` Their value depends on other signals. If the signal they depend on changes, they change too.
Effects: These are special functions that respond when signal values change.

## What are RxJS Operators ?
- `toSignal:` As httpClient returns an observable, a good approach is to use toSignal() to convert or transform the observables into a signal.

- `Subject:` A Subject is a special type of Observable which shares a single execution path among observers and allows values to be multicast to many Observers. Their message (the subject) is being delivered to many (multicast) people (the observers) at once. This is the basis of multicasting.

- `interval:` An operator that returns an observable which emits numbers in sequence based on provided timeframe.

- `takeUntil:` A filtering operator that emits values until provided observable emits.

- `take:` It emits provided number of values before completing. You can use it when you are interested in only the first emission, you want to use take. 

- `takeWhile:` It emits values until provided expression is false. You can use it when the optional inclusive parameter is set to true it will also emit the first item that didn't pass the predicate.

### What is ReplaySubject in angular ?
It is a variant of the Subject class and allows you to multicast values to multiple subscribers.

A `ReplaySubject` remembers and replays a specific number of values to any subscriber that subscribes to it. When a new subscriber subscribes to a ReplaySubject, it will immediately receive the buffered values, up to a specified buffer size or timeframe.

The key features of a `ReplaySubject` are:

1. Buffering: A ReplaySubject keeps a buffer of values that it has emitted. You can specify the maximum number of values to buffer using the buffer size parameter when creating the ReplaySubject.

2. Subscription: When a new subscriber subscribes to a ReplaySubject, it immediately receives the buffered values. If the buffer size is reached, older values are dropped from the buffer to accommodate new values.

3. Timeframe: In addition to the buffer size, you can also specify a timeframe for the ReplaySubject. With a timeframe, the ReplaySubject will only buffer values emitted within a specific time window.

### How to make multiple http calls in parallel in Angular ?
Using `forkJoin`, this operator takes an array of observables and waits for all the source observables to complete. Once they all complete, it emits an array of the last emitted values from each observable. 

### How do you handle errors in RxJS observables?
RxJS provides several operators for handling errors in Observables. The two main operators for error handling are `catchError` and `retry`.

1. `catchError:` The `catchError` operator is used to catch errors that may occur in an Observable and handle them in a graceful way. It takes a function as an argument that returns another Observable or throws an error.

2. `retry:` The `retry` operator is used to automatically retry an Observable when it encounters an error. It takes an optional argument that specifies the maximum number of retries. The `retry()` operator is used in RxJS to resubscribe to an observable if an error occurs. 

### How do you implement backpressure in RxJS?
`Backpressure` is a mechanism used in reactive programming to handle situations where an Observable is emitting data at a faster rate than it can be consumed. This can lead to issues such as high memory usage, slow processing, and even crashes. 
RxJS provides several operators for implementing backpressure, including `buffer`, `throttle`, `debounce`, `sample`, and `switchMap`.

1. buffer: The `buffer` operator collects emitted values from the source Observable into an array and emits the array when it reaches a specified size. It can be used to temporarily store emitted values until they can be processed.

2. throttle: The `throttle` operator throttles the emissions of the source Observable by discarding emissions that occur within a specified time window. It can be used to limit the rate of emissions from the source Observable.

3. debounce: The `debounce` operator delays emissions from the source Observable until a specified time has elapsed since the last emission. It can be used to filter out rapid emissions and emit only the last value.

4. sample: The `sample` operator emits the most recent value from the source Observable at a specified time interval. It can be used to emit the most recent value at a regular interval, regardless of how many values are emitted.

5. switchMap: The `switchMap` operator can be used to limit the number of concurrent emissions from the source Observable. 

### What is the purpose of using schedulers in RxJS ?
In RxJS, a scheduler is an object that provides a way to control the timing of when events are emitted by observables. Schedulers can be used to schedule tasks to be executed at a specific time, delay the execution of tasks, or specify on which thread the tasks should be executed.

The `observeOn()` operator is used to specify the scheduler on which an observable should emit its values. Some common schedulers in RxJS include `async`, `queue`, `animationFrame` and `immediate`.

### What is pipeable operator in RxJS ? 
Pipeable operators are imported as standalone functions and chaining them together with the `pipe()` function. Pipeable operators are pure functions that take an observable as their input and return a new observable as their output, allowing multiple operators to be composed together to form a pipeline.

### What is async pipe ? 
The `async` pipe subscribes to an Observable or Promise and returns the latest value it has emitted. When a new value is emitted, the `async` pipe marks the component to be checked for changes. When the component gets destroyed, the `async` pipe unsubscribes automatically to avoid potential memory leaks.

### What is patch operator in RxJS ?
Patch operators are imported as methods on the `Observable` class and are then used by calling them directly on an observable. Patch operators modify the behavior of the observable instance they are called on, and cannot be composed together in the same way as pipeable operators.

### What is reduce operator in RxJS ?
The `reduce` operator is used to apply an accumulation function to the values emitted by an observable sequence and emit a single accumulated result. It is similar to the Array.prototype.reduce() function in JavaScript. The reduce operator is useful when you want to obtain a single accumulated result from a sequence of values.

### What is a BehaviorSubject?
 BehaviorSubject is a type of Observable provided by the RxJS library. Unlike traditional Observables that emit values only upon specific events, BehaviorSubject maintains the latest value it has emitted and immediately dispatches it to new subscribers upon subscription.

## What is HttpClient:
`HttpClient` is a tool provided by Angular that helps us communicate with servers over the internet. We use it to fetch data from servers or send data to servers.

Configuring features of HttpClient

- `withFetch`:
This feature switches HttpClient to use the fetch API instead of the default XMLHttpRequest API.

- `withInterceptors`:
Configures a set of interceptor functions to process requests made through HttpClient.

- `withInterceptorsFromDi`:
Includes the older style of class-based interceptors in the HttpClient configuration.

- `withRequestsMadeViaParent`:
Passes requests up to the HttpClient instance in the parent injector after passing through interceptors at the current level.

- `withJsonpSupport`:
Enables the .jsonp() method on HttpClient for cross-domain loading of data using JSONP convention.

- `withXsrfConfiguration`:
Allows customization of HttpClient’s built-in XSRF security functionality.

- `withNoXsrfProtection`:
Disables HttpClient’s built-in XSRF security functionality.

### Explain the HTTPClientModule ?
In Angular, the `HttpClientModule` is a built-in module that helps in making HTTP requests to remote servers or APIs. It is part of the `@angular/common/http` package and is used for handling asynchronous HTTP requests and responses. The HttpClientModule provides the `HttpClient` service, which is used to make HTTP requests. It supports various HTTP methods such as GET, POST, PUT, DELETE, etc.

## What Is TestBed ?
TestBed is an Angular testing class that makes it easy to configure and initialize the environment for unit tests in Angular. It acts as a sandbox where you can configure and set up the components, directives, services and pipes that are going to be tested.

TestBed makes it easy to create and work with Angular components and their dependencies in a testing scenario. By using TestBed, we can configure testing modules similar to how you would set up an Angular app with specified declarations, imports, providers and more. This helps in mocking or faking the runtime environment of an Angular application.

## What is the purpose of decorators in Angular?
Decorators are used in Angular to add metadata and configuration to Angular elements like components, services, directives, pipes, etc., that state how these elements will be processed and constructed.

There are four different types of decorators in Angular:

`Method decorator:` This decorator is applied to a method within a class and allows you to modify or enhance the behavior of that method. It can be used to add functionality around the method execution.

`Class decorator:` This decorator is used to enhance the behavior of a class.

`Parameter decorator:` This decorator is added to the parameters of a method.

`Property decorator:` This decorator is applied to a particular property and can modify accessors, define bindings, or perform validation.

### What is @Input decorator ?
`@Input()` is an Angular decorator that marks a class property as an input property of the component.

The `@Input` decorator is used to pass data from a parent component to a child component.

### What is @Output decorator ?
`@Output` decorator allows a child component to emit/trigger events and send data back to the parent component.

The `@Output` decorator that marks a component property as an output of the component, it is associated with an EventEmitter.

When an EventEmitter emits an event, any parent component listening to that event through an event binding can respond to it accordingly.

@Output({
    alias?: string;
}) propertyName = new EventEmitter<type>();

By using `HostAttributeToken`, you make your code compatible with elements like ng-container and ng-template, ensuring safer usage in server-side rendering (SSR) scenarios.

### What is the purpose of decorators @ViewChild?
The purpose of the @ViewChild decorator in Angular allows users to access the properties, methods, and events of the particular selected child component or element within a parent component. 

- `ngAfterViewInit`: Use `@ViewChild` decorator to access view elements.

### What is the purpose of @ContentChild decorator?
The @ContentChild decorator in Angular is used to access the first occurrence of a directive or component within the content of a component. It allows users to get a reference of a specific element or component that is projected into the component's view. 

- `ngAfterContentInit`: Use `@ContentChild` decorator to access projected content.

### What are the properties of @Component decorator ?
@Component() decorator which takes the following metadata:
- `selector` that allows us to give the component a tag name that can be used to reference the component from other templates just like standard HTML tags.
- `templateUrl` that points to the HTML template that renders the view of the component. You can also use an inline template with the template property instead.
- `styleUrls` that allows us to associate one or multiple stylesheets to our component.

### What are the properties inside @NgModule decorator ?
The @NgModule decorator is used to define every module in Angular.
- `providers?` The set of injectable objects that are available in the injector of this module.

- `declarations?` | The set of components, directives, and pipes (declarables) that belong to this module.

- `imports?` | The set of NgModules whose exported declarables are available to templates in this module.

- `exports?` | The set of components, directives, and pipes declared in this NgModule that can be used in the template of any component that is part of an NgModule that imports this NgModule. Exported declarations are the module's public API.

- `entryComponents?` | The set of components to compile when this NgModule is defined, so that they can be dynamically loaded into the view.

- `bootstrap?` | The set of components that are bootstrapped when this module is bootstrapped. The components listed here are automatically added to entryComponents.

- `schemas?` | The set of schemas that declare elements to be allowed in the NgModule. Elements and properties that are neither Angular components nor directives must be declared in a schema.

- `id?` | A name or path that uniquely identifies this NgModule in getModuleFactory If left undefined, the NgModule is not registered with getModuleFactory.

- `jit?` | When present, this module is ignored by the AOT compiler. JIT compiler attempts to compile it at run time, in the browser. To ensure the correct behavior, the app must import @angular/compiler.

## What is ngZone in Angular?
In Angular, `NgZone` helps manage and control the execution of asynchronous tasks and change detection. It is responsible for triggering change detection and updating the view when changes occur.

When code executes within this zone, Angular's change detection mechanism is triggered automatically, and the view is updated accordingly. 
When code runs outside of the Angular zone, Angular may not be aware of the changes, leading to potential issues with the application state and view synchronization.

`NgZone` provides a way to explicitly run code inside or outside of the Angular zone. It offers two methods for executing code: `run()` and `runOutsideAngular()`.

## Explain OnPush strategy ?
The `OnPush` strategy only triggers change detection in a component when one of its input properties changes or when an event emitted by the component itself or its child components is received.

## Explain ngAfterContentInit hooks ?
The `ngAfterContentInit` hook is a lifecycle hook in Angular that is called after Angular initializes the content projected into a component. This hook is useful when you want to perform some initialization or setup logic after the content has been projected into the component.

## Explain ngAfterViewInit hook ?
The `ngAfterViewInit` hook is a lifecycle hook in Angular that is called after Angular initializes the component’s view and its child views. This hook is useful when you need to perform some logic or operations that require access to the component’s view or its child views.

## Explain ngOnInit hook ?
The `ngOnInit` is a lifecycle hook part of the Angular component lifecycle. The hook is invoked when a component is being initialized and is ready to perform any necessary setup tasks before being rendered. 

## What are custom directives ?
Custom directives are a feature in Angular that allow developers to extend the functionality of HTML by creating their own custom HTML elements or attributes. With custom directives, developers can define their own behavior, such as adding event listeners, modifying the DOM, or manipulating data.

## What are Angular guards?
Guards are a feature in Angular that allows users to manage and control the application's routing. Angular Guards can protect routes, control access to certain routes based on user authentication, and perform pre-navigation checks or modifications.

### What is canActivateChild route guard ?
The `canActivateChild` route guard in Angular allows you to check if a user is allowed to activate child routes. It is used to protect child routes of a particular route from being activated if certain conditions are not met.

## What is a module in Angular?
In Angular, a module is like a package that is used to organize and bundle related components, directives, services, and other features of an application.

## What are templates in Angular?
Angular templates allow you to define the UI of your applications. Angular-specific templates that are written in HTML contain Angular-specific elements and attributes.

## What is NgRx in Angular?
NgRx is a state management library in Angular that is based on Redux. NgRx helps in managing the state by separating it from the components, thus implementing a unidirectional data flow. It combines all events and defines a common state in the angular application.

## What is RxJs in Angular?
RxJs, or Reactive extensions for JavaScript, is a fundamental library in Angular that supports reactive programming with the help of observables. It handles asynchronous operations, callbacks, event handling, and data flow management.

## What is Angular interceptors ?
Angular interceptors provide a way to intercept and modify HTTP requests and responses globally in an Angular application. Interceptors are implemented as classes that implement the HttpInterceptor interface. When an HTTP request is made, the interceptors in Angular are executed in a chain before the request reaches the server. 

## Explain lazy loading in Angular?
Lazy loading is a feature in Angular that allows developers to load modules and components as per their demand on the action of a specific route rather than loading everything in advance. The feature helps improve the loading time, thereby increasing the application's performance.

## What are directives in Angular?
Directives are attributes that allow the user to write new HTML syntax specific to the applications. These directives execute whenever the Angular compiler finds them in the DOM. Angular supports three types of built-in directives:

`Component Directives:` These are the directives that come with a template and are the most common type of directives. 
 
`Attribute Directives:` These are the directives that can change the appearance of a component, page or even other directives.

`Structural Directives:` These directives are responsible for changing the DOM layout either by adding or by removing the DOM elements. Every structural directive has a ‘*’ sign before them.

### What is ngClass directive in Angular ?
The `ngClass` directive in Angular is used to apply CSS classes to an HTML element based on certain expressions or conditions.

### What is ngStyle in Angular?
The `ngStyle` directive in Angular is used to apply inline CSS styles to HTML elements dynamically based on specific conditions.

### What is BreakpointObserver service ?
the BreakpointObserver service in Angular comes with predefined breakpoints and can be used to adjust the layout and style of the application based on different screen sizes without actually writing the CSS media queries. 

## What is AOT? 
AOT stands for Ahead of time. It is a type of compilation that compiles your application at build time. For AOT compilation, we need to include the –aot option with the ng build or ng serve command. 
```
ng build –aot
ng serve –aot
```

## What is Server-side rendering:
Server-side rendering (SSR) is a process that involves rendering pages on the server, resulting in initial HTML content which contains initial page state. Once the HTML content is delivered to a browser, Angular initializes the application and utilizes the data contained within the HTML.

## What is spinner options ?
`bdcolor` - to set the background color
`size` - to set the size of the spinner
`color` - to set the color of the spinner
`type` - to set the type of the spinner
`fullscreen` - to enable/disable full screen mode
`name` - to set the name of the spinner if there are multiple instances of the spinners

## Explain ngFor directive ?

`<div *ngFor="let photo of photos; trackById"></div>`
simplification is:
`<ng-template ngFor let-photo [ngForOf]="photos" ngForTrackById"></ng-template>`