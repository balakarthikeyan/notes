## What are Angular Features?

* Interpolation
* Template Statements
* Event Binding
* Built-in Directives
* Pipes
* Property Binding
* Attribute, Class, and Style Bindings

---

## Data Binding

Binding refers to the process of communication between a component and its corresponding view. It is utilized for transferring data to and from the Angular framework. Data can be passed through various means, such as through events, interpolation, properties, or through the two-way binding mechanism. Moreover, data can also be shared between related components (parent-child relation) and between two unrelated components using the Service feature.

Data binding flow classification:

* **Data source to view target**: Includes interpolation, properties, attributes, classes, and styles. Applied using `[]` or `{{}}` in templates.
* **View target to data source**: Includes events. Applied using `()` in templates.
* **Two-Way**: Applied using `[()]` in templates.

Target member classifications:

* **Property**: Element property, component property, directive property. Example: `<img [alt]="hero.name" [src]="heroImageUrl" />`
* **Event**: Element event, component event, directive event. Example: `<button type="button" (click)="onSave()">Save</button>`
* **Two-way**: Event and property. Example: `<input [(ngModel)]="name" />`
* **Attribute**: Attribute property. Example: `<button type="button" [attr.aria-label]="help">Help</button>`
* **Class**: Class property. Example: `<div [class.special]="isSpecial">Special</div>`
* **Style**: Style property. Example: `<button type="button" [style.color]="isSpecial ? 'red' : 'green'">Color</button>`
### One-Way Binding (Interpolation)

Interpolation is a way to bind dynamic values directly into the text content of HTML elements.

```html
<h1>{{ pageTitle }}</h1>
```

### Two-Way Binding

Two-way binding synchronizes the Model (data) and the View (UI). Changes in the View automatically update the Model, and changes in the Model are reflected back to the View.

*Legacy Syntax (`ngModel` / Property + Event):*

```html
<!-- Shorthand using ngModel -->
<input [(ngModel)]="username">

<!-- Expanded syntax equivalent -->
<input [value]="username" (input)="username = $event.target.value">
```

*Modern Syntax (Angular 17+ Signal Model Inputs):*

```typescript
// Component definition
username = model('');
```

```html
<!-- Component template -->
<input [(ngModel)]="username">
```

### Property Binding

Helps set values for properties of HTML elements and directives.

```html
<img [src]="imageUrl" alt="Image">
<td [colSpan]="1 + 1" style="background-color: yellow;">2 cols</td>
```

### Attribute, Class, and Style Bindings

```html
<td [attr.colspan]="1 + 1" style="background-color: yellow;">2 cols</td>
<p [style.color]="green ? 'green' : 'red'">hello world</p>
<p [class.green]="green" [class.red]="!green">hello world</p>
```

### Event Binding

Allows you to respond to user events (such as clicks, keypresses, etc.) by triggering methods in the component.

```html
<button (click)="onClick()">Click me</button>
<input (input)="onInputChange($event)">
```

---

## What is a Pipe?

Pipes are simple functions to use in template expressions to accept an input value and return a transformed value.

* **Pure Pipes**: By default, pipes are "pure." The pipe will be called when the input values they depend on change.
* **Impure Pipes**: Set `pure: false` in metadata. The pipe will be called in every change detection cycle, regardless of whether the input values have changed or not.

### What is a Template Expression?

A template expression produces a value within double curly braces that Angular executes and binds to the property of a target, such as an HTML element, component, or directive.

### What are Angular Built-in Pipes?

Angular provides built-in pipes for typical data transformations, including internationalization (i18n) formatting:

* `DatePipe`: Formats a date value according to locale rules.
* `UpperCasePipe`: Transforms text to all upper case.
* `LowerCasePipe`: Transforms text to all lower case.
* `CurrencyPipe`: Transforms a number to a currency string, formatted according to locale rules.
* `DecimalPipe`: Transforms a number into a string with a decimal point, formatted according to locale rules.
* `PercentPipe`: Transforms a number to a percentage string, formatted according to locale rules.

---

## What is Dependency Injection?

Dependency Injection (DI) reduces coupling between classes and their dependencies, making code more maintainable, testable, and reusable.

*Legacy Syntax (Constructor Injection):*

```typescript
import { Component } from '@angular/core';
import { RootService } from './root.service';

@Component({ ... })
export class AppComponent {
  constructor(private service: RootService) {}
}
```

*Modern Syntax (Angular 14+ `inject()` Function):*

```typescript
import { Component, inject } from '@angular/core';
import { RootService } from './root.service';

@Component({ ... })
export class AppComponent {
  service = inject(RootService);
}
```

*Internal DI Mechanism (Conceptual Overview):*

```typescript
export const inject = (searchClass: any) => {
  const dependence = find(searchClass);
  if (dependence) {
    return dependence;
  } else {
    return new searchClass();
  }
};
```

The Injector stores information about all injectable classes, which includes anything marked with a decorator such as `@Injectable`, `@Component`, `@Pipe`, and `@Directive`.

Angular has two categories of Injectors:

* **EnvironmentInjector**: Global injectable classes provided through the router, modules, or using `providedIn: 'root'`.
* **NodeInjector**: Local injectable classes found in each component or directive template.

---

## What Are Angular Signals?

Angular Signals represent a modern way to build reactive applications, built on top of reactive primitives that emit updates when their underlying values change. A signal is a container that holds a value and notifies subscribers when that value changes.

The Signals API is a small and easy-to-use API, Reactive primitives:

* **Writable signals**: Signals whose value can be directly updated.
* **Computed signals**: Signals whose value depends on other signals.
* **Effects**: Special functions that execute side-effects in response to signal changes.

*Modern Signals Example (Angular 16+):*

```typescript
import { signal, computed, effect } from '@angular/core';

// Writable Signal
const count = signal(0);
count.set(5);
count.update(v => v + 1);

// Computed Signal
const doubleCount = computed(() => count() * 2);

// Effect
effect(() => {
  console.log(`Current value: ${count()}`);
});
```

---

## What are RxJS Operators?

* `toSignal`: Converts/transforms Observables (e.g., HTTP responses) into Signals.
* `Subject`: A special type of Observable that shares a single execution path among observers (multicast).
* `interval`: Returns an Observable that emits numbers in sequence based on a specified time interval.
* `takeUntil`: A filtering operator that emits values until a provided Observable emits.
* `take`: Emits a specified number of values before completing.
* `takeWhile`: Emits values until a specified predicate condition evaluates to false.

### What is ReplaySubject in Angular?

`ReplaySubject` is a variant of `Subject` that replays a specified number of past emissions to new subscribers.

Key Features:

* **Buffering**: Maintains a buffer of emitted values based on a specified size parameter.
* **Subscription**: New subscribers immediately receive buffered values upon subscribing.
* **Timeframe**: Optional window time limit for buffering emitted values.

### How to make multiple HTTP calls in parallel in Angular?

Use `forkJoin`, which accepts an array or object of Observables, waits for all to complete, and emits their final values.

```typescript
import { forkJoin } from 'rxjs';

forkJoin({
  users: this.http.get('/api/users'),
  posts: this.http.get('/api/posts')
}).subscribe(({ users, posts }) => {
  console.log(users, posts);
});
```

### How do you handle errors in RxJS observables?

RxJS provides several operators for handling errors in Observables.

* `catchError`: Catches errors on an Observable stream and handles them gracefully by returning a new Observable or throwing a transformed error.
* `retry`: Resubscribes to a source Observable when an error occurs, accepting an optional max retry attempt count.

### How do you implement backpressure in RxJS?

`Backpressure` manages scenarios where an Observable emits data faster than a consumer can process it.
RxJS provides several operators for implementing backpressure, including `buffer`, `throttle`, `debounce`, `sample`, and `switchMap`.

Operators for backpressure:

* `buffer`: Collects emissions into an array and emits the array when it reaches a set size.
* `throttle`: Discards emissions within a specified time window.
* `debounce`: Delays emissions until a specified timeframe elapses without new emissions.
* `sample`: Emits the most recent value within periodic time intervals.
* `switchMap`: Cancels pending inner emissions to limit concurrent processing.

### What is the purpose of using schedulers in RxJS?

Schedulers control the timing and execution context of Observable emissions. The `observeOn()` operator specifies the target scheduler. Common schedulers: `async`, `queue`, `animationFrame`, `asap`.

### What is a pipeable operator in RxJS?

Pipeable operators are standalone functions passed into the `.pipe()` method. They take an Observable input and return a new transformed Observable.

### What is an async pipe?

The `async` pipe automatically subscribes to an Observable or Promise in Angular templates, returns latest values, marks components for change detection, and unsubscribes when destroyed.

### What is a patch operator in RxJS?

*(Legacy RxJS 4/5)* Patch operators were attached directly to the `Observable.prototype` rather than composed via pure function pipelines.

### What is the reduce operator in RxJS?

Applies an accumulator function over source emissions and emits a single final accumulated result upon stream completion.

### What is a BehaviorSubject?

A variant of `Subject` that holds an initial/current value and immediately emits its current value to new subscribers.

---

## What is HttpClient?

`HttpClient` handles HTTP requests to communicate with remote servers.

*Modern Standalone Setup (Angular 15+):*

```typescript
import { provideHttpClient, withFetch, withInterceptors } from '@angular/common/http';

export const appConfig = {
  providers: [
    provideHttpClient(
      withFetch(),
      withInterceptors([authInterceptor])
    )
  ]
};
```

*Legacy Module Setup:*

```typescript
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [HttpClientModule]
})
export class AppModule {}
```

Configuring `HttpClient` Features:

* `withFetch`: Switches request transport from `XMLHttpRequest` to the native `fetch` API.
* `withInterceptors`: Configures functional interceptor functions.
* `withInterceptorsFromDi`: Includes legacy class-based interceptors from DI.
* `withRequestsMadeViaParent`: Routes requests up to a parent injector `HttpClient` instance.
* `withJsonpSupport`: Enables JSONP cross-domain data fetching.
* `withXsrfConfiguration`: Configures custom XSRF anti-forgery protection.
* `withNoXsrfProtection`: Disables built-in XSRF protection.

### Explain the HttpClientModule

`HttpClientModule` is a built-in NgModule from `@angular/common/http` that provides the `HttpClient` service for handling asynchronous HTTP communication.

---

## What Is TestBed?

`TestBed` is the primary Angular testing utility to configure and initialize unit test environments, mocking components, directives, services, and pipes.

---

## What is the purpose of decorators in Angular?

Decorators add metadata and configuration instructions to classes and members.

Decorator Types:

* **Method decorator**: Modifies or enhances class method behavior.
* **Class decorator**: Configures top-level class behavior (e.g., `@Component`, `@Injectable`).
* **Parameter decorator**: Applied to method or constructor parameters.
* **Property decorator**: Modifies property bindings or accessors.

### What is @Input decorator?

Marks a property as an input target for receiving data from a parent component.

*Legacy Decorator:*

```typescript
@Input() label: string = '';
```

*Modern Signal Input (Angular 17+):*

```typescript
label = input<string>('');
```

### What is @Output decorator?

Marks a property as an event output to send data to parent components via an `EventEmitter`.

*Legacy Decorator:*

```typescript
@Output() save = new EventEmitter<string>();
```

*Modern Signal Output (Angular 17+):*

```typescript
save = output<string>();
```

Using `HostAttributeToken` provides safe attribute reading for SSR and non-standard element rendering (`ng-container`, `ng-template`).

### What is the purpose of @ViewChild decorator?

Grants direct access to child components, directives, or DOM elements within the component template. Evaluated before `ngAfterViewInit`.

*Legacy Decorator:*

```typescript
@ViewChild('myInput') inputRef!: ElementRef;
```

*Modern Signal Query (Angular 17.2+):*

```typescript
inputRef = viewChild<ElementRef>('myInput');
```

### What is the purpose of @ContentChild decorator?

Accesses projected content (content inserted via `<ng-content>`). Evaluated before `ngAfterContentInit`.

*Legacy Decorator:*

```typescript
@ContentChild(HeaderComponent) header!: HeaderComponent;
```

*Modern Signal Query (Angular 17.2+):*

```typescript
header = contentChild(HeaderComponent);
```

### What are the properties of @Component decorator?

@Component() decorator which takes the following metadata:

* `selector`: Element selector string for referencing the component in templates.
* `templateUrl` / `template`: Inline HTML string or path to an external template file.
* `styleUrls` / `styles`: Inline CSS styles or paths to external stylesheets.
* `standalone`: Specifies if the component manages its own dependencies without an `NgModule`.

### What are the properties inside @NgModule decorator?

The @NgModule decorator is used to define every module in Angular.

* `providers`: Injectable services available to the module's injector.
* `declarations`: Components, directives, and pipes belonging to the module.
* `imports`: Other NgModules whose exported components are needed.
* `exports`: Declarables exported for use in importing modules.
* `entryComponents`: Dynamically compiled component references.
* `bootstrap`: Root components bootstrapped during app startup.
* `schemas`: Allows custom HTML/custom element schemas.
* `id`: Unique identifier for module registration.
* `jit`: Ignores AOT compilation to use JIT compilation.

---

## What is ngZone in Angular?

`NgZone` manages execution contexts for tracking asynchronous events and triggering change detection.

Methods:

* `run()`: Executes code inside Angular's change detection zone.
* `runOutsideAngular()`: Executes code outside change detection, avoiding unnecessary render cycles.

---

## Explain OnPush strategy?

`ChangeDetectionStrategy.OnPush` disables automatic change detection passes, re-evaluating components only when `@Input` reference changes, component events trigger, or explicit Observables emit.

---

## Explain ngAfterContentInit hook?

Lifecycle hook triggered once after Angular finishes projecting external content into the component's view (`<ng-content>`).

---

## Explain ngAfterViewInit hook?

Lifecycle hook called after component views and child views are fully initialized.

---

## Explain ngOnInit hook?

Lifecycle hook executed once after data-bound inputs are initialized, suitable for component setup and API invocation.

---

## What are custom directives?

Custom directives extend HTML capabilities by applying custom behaviors, attributes, or structural modifications to DOM elements.

---

## What are Angular guards?

Guards control router navigation paths based on conditional criteria (authentication, authorization, dirty checks).

### What is canActivateChild route guard?

Determines whether a user can navigate into child routes of a protected parent route.

---

## What is a module in Angular?

An `NgModule` is a logical container that bundles related components, directives, pipes, and services into functional application units.

---

## What are templates in Angular?

Templates define component user interfaces by combining standard HTML markup with Angular-specific binding syntax and directives.

---

## What is NgRx in Angular?

NgRx is a Redux-inspired state management framework for managing global/complex application state through unidirectional data flows, actions, reducers, and effects.

---

## What is RxJS in Angular?

RxJS is a reactive library providing `Observable` constructs for asynchronous data streams, event handling, and pipeline transformations across Angular apps.

---

## What are Angular interceptors?

Interceptors process outgoing HTTP requests and incoming HTTP responses globally.

*Legacy Class Interceptor:*

```typescript
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req);
  }
}
```

*Modern Functional Interceptor (Angular 15+):*

```typescript
export const authInterceptor: HttpInterceptorFn = (req, next) => {
  return next(req);
};
```

---

## Explain lazy loading in Angular?

Lazy loading dynamically imports feature modules or routes on demand when requested by routing, reducing initial application bundle size.

---

## What are directives in Angular?

Directives add custom logic to template DOM elements.

* **Component Directives**: Directives with templates.
* **Attribute Directives**: Directives that alter host element appearance or behavior.
* **Structural Directives**: Directives that alter DOM layouts by adding/removing elements (`*`).

### What is ngClass directive in Angular?

Dynamically applies or removes CSS class sets based on expression evaluations.

### What is ngStyle in Angular?

Dynamically updates inline element styling based on expression maps.

### What is BreakpointObserver service?

An Angular CDK service for matching media queries and building responsive layouts.

---

## What is AOT?

Ahead-of-Time (AOT) compilation compiles Angular templates and TypeScript code into JavaScript during build time prior to deployment.

```bash
ng build --aot
ng serve --aot

```

---

## What is Server-side rendering?

Server-side rendering (SSR) generates dynamic HTML pages directly on a Node server to deliver complete initial markup for faster rendering and improved SEO.

---

## What is spinner options?

Configuration parameters for loading spinner displays:

* `bdcolor`: Background overlay color
* `size`: Spinner element dimensions
* `color`: Spinner theme color
* `type`: Animation visual type
* `fullscreen`: Enables or disables full-page backdrop
* `name`: Identifier for multiple concurrent spinners

---

## Explain ngFor directive?

Renders a template block for each item in a collection.

*Legacy Structural Syntax:*

```html
<div *ngFor="let photo of photos; trackBy: trackById"></div>

<!-- Desugared structural element equivalent -->
<ng-template ngFor let-photo [ngForOf]="photos" [ngForTrackBy]="trackById"></ng-template>
```

*Modern Control Flow Syntax (Angular 17+):*

```html
@for (photo of photos; track photo.id) {
  <div></div>
}
```

---

## What is Angular security model?

Angular automatically sanitizes untrusted values across standard `SecurityContext` points:

- `None`;
- `HTML` is used, when interpreting value as HTML;
- `STYLE` is used, when binding CSS into the style property;
- `URL` is used for URL properties, such as `<a href>`;
- `SCRIPT` is used for JavaScript code;
- `RESOURCE_URL` as a URL that is loaded and executed as code, for example, in `<script src>`.

---

## What are Bypass Security Trust Methods?

`DomSanitizer` methods to disable automatic sanitization for explicitly trusted inputs:

* `bypassSecurityTrustUrl`
* `bypassSecurityTrustResourceUrl`
* `bypassSecurityTrustHtml`
* `bypassSecurityTrustScript`
* `bypassSecurityTrustStyle`

### Injection Scenarios:

* **HTML Injection**: Occurs when binding raw user input to properties like `innerHTML`. Angular sanitizes string content into `SecurityContext.HTML` to allow markup while suppressing active script execution.
* **Template Injection (CSR)**: Occurs when user string input contains evaluation brackets (`{{}}`), allowing dynamic evaluation during Client-Side Rendering.
* **Server-Side Rendering (SSR)**: Universal applies sanitization mechanisms during server compilation to secure generated HTML.

---

## What is Shadow DOM?

Shadow DOM provides DOM and style scoping encapsulation natively inside web platform elements.

---

## What is view encapsulation?

Determines whether style definitions inside component metadata leak into global scope:

* **Emulated**: Default behavior. Emulates Shadow DOM using generated scope attributes.
* **ShadowDom**: Uses native browser Shadow DOM encapsulation.
* **None**: Component CSS leaks globally into the document.

---

## What are the design patterns in Angular?

1. **Singleton Pattern**: Injectable services configured with `providedIn: 'root'` or root modules share a single instance application-wide.
2. **Dependency Injection Pattern**: Managed lookup mechanism for binding parameters and class dependencies dynamically.
3. **Observer Pattern**: Event emission and subscription handling through `EventEmitter` and `RxJS` streams.
4. **Strategy Pattern**: Dynamic interchange of algorithms or handlers through common interface patterns.
5. **Decorator Pattern**: Meta-programming design enabling annotations like `@Component` to enrich class structures.
6. **Facade Pattern**: Service implementations exposing simple interfaces while concealing complex underlying subsystem logic.
7. **Composite Pattern**: Component tree hierarchies building UI components out of child components.
8. **Factory Pattern**: Centralized dynamic instantiation logic.

---

## What Is Linting?

Static inspection process executing rules over target source code to flag potential style, syntax, and structural errors.

---

## What Is Static Analysis?

Analysis of application source code executed without actually running the program.

### Code Improvements Provided by Static Analysis:

1. Formatting and Styling Code
2. Detecting Bugs and Errors
3. Enforcing Best Practices
4. Measuring Complexity
5. Analyzing Security Risks
6. Auditing Third-Party Dependencies
7. Checking Types

---

## What is Garbage Collector Angular?

Automatic engine runtime memory management clearing unreachable heap references.

---

## Vendor.js

Bundled script containing third-party dependencies and framework libraries (`@angular`, `rxjs`).

---

## Polyfill.js

JavaScript compatibility shims providing modern API features to legacy environments.

---

## Main.js

Entry point script containing application startup logic and root component initialization.

---

## Runtime.js

Webpack/esbuild loader logic managing module execution and chunkloading runtime operations.

---

## What is zone js in angular?

`Zone.js` intercepts asynchronous operations (timer events, XHR requests, DOM actions) to auto-trigger change detection passes.

---

## What is the difference between AOT and JIT?

### JIT (Just-in-Time):

* **Compilation**: Happens at runtime in browser environments during startup.
* **Development Mode**: Fast incremental build output suited for development debugging.
* **Performance**: Slows initial rendering due to runtime compilation steps.
* **Debugging**: Direct sourcemaps matching compiled output with original source code.

### AOT (Ahead-of-Time):

* **Compilation**: Converts templates and code to JavaScript during local build operations.
* **Production Mode**: Optimized bundle output designed for live releases.
* **Performance**: Faster startup and smaller footprint.
* **Security**: Mitigates template injection vulnerabilities by removing template parsers from outputs.
* **Smaller Bundle Size**: Allows tree-shaking optimizations.
* **Dynamic Limitations**: Limits runtime dynamic template compilation.

---

## What are annotations in Angular?

Annotations (Decorators) attach structural metadata to class definitions. Common annotations: `@NgModule`, `@Component`, `@Directive`, `@Pipe`, `@Injectable`, `@Input`, `@Output`, `@ViewChild`, `@HostListener`, `@HostBinding`.

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

```typescript
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

---

## What is Angular change detection?

Mechanism monitoring model changes to keep the visual DOM synced with underlying state.

---

## Observable in Angular

* RxJS streaming model constructs emitting data across application instances.
* Stream processing unit managing asynchronous values over time.

---

## Difference between Observables & Subjects?

* **Observables**: Unicast model (each subscriber receives independent execution context flows).
* **Subjects**: Multicast model (shares single execution paths across multiple subscriber listeners).

---

## Difference between cold observables and hot observables?

* **Cold Observables**: Produces emissions only when active subscriptions are established.
* **Hot Observables**: Emits values regardless of subscriber presence.

---

## Difference between ng add and npm install?

* `ng add`: Installs NPM packages and runs schematics to configure project setups.
* `npm install`: Standard package manager command that fetches modules without running Angular schematics.

---

## What is DomSanitizer?

Built-in security service sanitizing unsanitized HTML/CSS/URL references against XSS attacks.

---

## General Angular Build Optimization Strategies

### 1. Enable Production Mode

```bash
ng build --configuration production
```

- Runs optimizations like Ahead-of-Time (AOT) compilation, minification, and tree-shaking.

### 2. Use Standalone Components & Lazy Loading

- Standalone components reduce NgModule overhead.
- Lazy load large feature modules (e.g., `DashboardModule`) so they don’t inflate the initial bundle.

### 3. Optimize CSS & Styles

- Use **SCSS** or **Tailwind** with purge settings to remove unused styles.
- Enable CSS minification and consider splitting global styles into smaller chunks.

### 4. Remove Unused Polyfills & Scripts

- Check `polyfills.ts` and `angular.json` for legacy scripts, Strip legacy browser polyfills if targeting modern web runtimes.

### 5. Bundle & Asset Optimization

- Compress assets with gzip or Brotli at the server level.
```json
{
  "optimization": true,
  "outputHashing": "all",
  "extractLicenses": true,
  "sourceMap": false
}
```

### 6. Code Splitting & Preloading

- Apply explicit preloading strategies (`PreloadAllModules`).

### 7. Third-Party Library Audit

- Swap oversized dependencies with lighter utilities (e.g., replace `moment` with `date-fns`).

---

## Applying to Your Build Output

### Current Build Sizes
| File              | Size     | Notes |
|-------------------|----------|-------|
| styles.css        | 494.61 kB | Very large – likely unused CSS included |
| main.js           | 304.42 kB | Acceptable, but can be reduced |
| scripts.js        | 107.73 kB | Check if all scripts are necessary |
| polyfills.js      | 237 B     | Fine |
| Lazy chunks       | ~5 kB     | Good – lazy loading is working |

**Total initial size: ~911 kB** → This is on the heavier side for initial load.

### Recommendations for Your Case

1. **Reduce `styles.css` (494 kB)**  
   - Audit global styles. Use Angular’s `::ng-deep` carefully.  
   - Consider CSS scoping per component.  
   - Use PurgeCSS/Tailwind purge to strip unused styles.

2. **Shrink `main.js` (304 kB)**  
   - Enable `"buildOptimizer": true` in `angular.json`.  
   - Audit imports—remove unused RxJS operators or large libraries.  
   - Use ESBuild (Angular 17+ defaults to it) for faster and smaller builds.

3. **Check `scripts.js` (107 kB)**  
   - Likely external scripts added in `angular.json`.  
   - Remove or lazy load them if not critical.

4. **Lazy Loading is Good**  
   - Your `dashboard.module` is only 4.7 kB, which is excellent.  
   - Keep feature modules lazy loaded.
---

## Method 1: Angular-Specific Build Optimizations

### Production Build `angular.json` Configuration:

```json
{
  "configurations": {
    "production": {
      "optimization": true,
      "outputHashing": "all",
      "sourceMap": false,
      "extractLicenses": true,
      "namedChunks": false,
      "buildOptimizer": true,
      "vendorChunk": false
    }
  }
}
```

* **optimization**: Enables minification and tree-shaking.
* **buildOptimizer**: Removes Angular decorators and unused code.
* **vendorChunk**: Setting to `false` merges vendor libraries into main bundle to reduce network roundtrips.
* **outputHashing**: Handles browser cache invalidation.
* **sourceMap**: Prevents non-essential debugging maps from loading in production.

---

## Method 2: CSS Optimization

### PurgeCSS Setup:

```bash
npm install @fullhuman/postcss-purgecss --save-dev
```

`postcss.config.js` configuration:

```javascript
const purgecss = require('@fullhuman/postcss-purgecss')({
  content: [
    './src/**/*.html',
    './src/**/*.ts'
  ],
  defaultExtractor: content => content.match(/[\w-/:]+(?<!:)/g) || []
});

module.exports = {
  plugins: [
    require('tailwindcss'),
    require('autoprefixer'),
    ...(process.env.NODE_ENV === 'production' ? [purgecss] : [])
  ]
};
```

---

## Summary of Optimization Actions

* **styles.css (494 kB)**: Apply PurgeCSS or Tailwind purge to strip unused styles.
* **main.js (304 kB)**: Enable `buildOptimizer` and audit third-party libraries.
* **scripts.js (107 kB)**: Remove or lazy load non-critical scripts.
* **polyfills.js (237 B)**: Already minimal.
* **Lazy chunks (~5 kB)**: Excellent; keep lazy loading pattern active.