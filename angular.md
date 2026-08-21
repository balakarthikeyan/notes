#  Angular Architecture & Study Guide

## 1. Introduction & Workspace Architecture

### What is Angular?
Angular is a robust, open-source front-end framework developed and maintained by Google. It is engineered for building scalable, high-performance, single-page web applications (SPAs). Built natively with **TypeScript**, Angular enforces strong typing, object-oriented principles, and modular design to improve application maintainability, readability, and testability.

### Workspace Directory Structure
Below is the standard directory structure of an Angular application workspace:

```bash
my-workspace/
├── src/
│   ├── app/
│   │   ├── app.module.ts          # Root module declaring application structure (Legacy)
│   │   ├── app.component.ts       # Application root component logic
│   │   ├── app.component.html     # HTML template for root component
│   │   ├── app.component.css      # CSS stylesheet for root component
│   │   ├── app.component.spec.ts  # Unit tests for root component
│   │   └── app-routing.module.ts  # Application-wide routing definitions
│   ├── index.html                 # Main HTML entry file host for root component
│   └── main.ts                    # Application bootstrap entry point
├── angular.json                   # CLI configuration for builds, environments, and assets
└── tsconfig.json                  # Workspace TypeScript compiler configuration

```

---

## 2. Framework Version Evolution (v15 – v18)

### Angular 15:

* **Standalone Components:** Promoted Standalone Components to stable status, reducing the reliance on `NgModule` for streamlined development.
* **Directive Composition API:** Allowed developers to apply directives to host elements inside component definitions to reuse logic cleanly.
* **ESBuild Integration:** Introduced experimental ESBuild support for faster build times.
* **Router Improvements:** Added `routerLinkActiveOptions` for enhanced control over link activation states.
* **Forms Enhancements:** Introduced new utility methods for dynamic creation and manipulation of form controls.

### Angular 16:

* **Non-Destructive Hydration:** Improved Server-Side Rendering (SSR) by attaching event listeners to existing DOM nodes without DOM re-rendering.
* **Angular Signals:** Introduced Signals as a reactive primitive, laying the groundwork for zone-less applications. Inspired by `Solid.js`. This helps in reducing the overhead of change detection and makes `Zone.js` optional​.
* **Language Service:** Enhanced Angular Language Service. 
* **Improved Developer Tools:** Angular DevTools for better debugging and code navigation.
* **Component Input Binding:** Enables binding route parameters, query parameters, and data directly to `@Input()` properties.
* **`DestroyRef` Provider:** Introduced `DestroyRef` to register cleanup logic dynamically outside traditional lifecycle hooks.
* **Angular DevTools:** Extended debugging capabilities for change detection and dependency injection.
* **Caching:** Default caching with the ability to control the cache location via the `ng cache` command​
* **Angular Material Updates:** Introduction of a new date range picker and other enhancements to improve accessibility and performance.

### Angular 17:

* **Built-in Control Flow:** Replaced structural directives (`*ngIf`, `*ngFor`, `*ngSwitch`) with explicit template syntax (`@if`, `@for`, `@switch`).
* **Deferrable Views (`@defer`):** Allowed declarative lazy loading of template components, directives, and styles based on triggers (e.g., `viewport`, `hover`, `interaction`).
* **ESBuild & Vite Default:** Switched to an ESBuild + Vite build architecture by default for faster build performance.
* **Signals API Stablization:** Signals reached stable status for state management.
* **ESLint Migration:** Official transition away from TSLint to ESLint across tooling defaults.

### Angular 18:

* **Zone-less Change Detection:** Optional complete removal of `zone.js`, powered by Signals-driven reactivity.
* **Event Replaying:** Automatic capturing and replaying of user events before hydration completes on SSR.
* **Enhanced Micro-Syntax & Debugging:** Improved error handling, diagnostic warnings, and CLI tools.
* **Default Material 3:** Full alignment of Angular Material components with Material Design 3 standards.

### Legacy Control Flow (Structural Directives)

```html
<!-- Legacy Conditionals -->
<div *ngIf="isLoggedIn; else loginBlock">
  <p>Welcome back, {{ user.name }}!</p>
</div>
<ng-template #loginBlock>
  <p>Please log in.</p>
</ng-template>

<!-- Legacy Loops -->
<ul>
  <li *ngFor="let item of items; trackBy: trackById">
    {{ item.name }}
  </li>
</ul>

```

### Modern Control Flow (Angular 17+)

```html
<!-- Modern Built-in Conditionals -->
@if (isLoggedIn) {
  <p>Welcome back, {{ user.name }}!</p>
} @else {
  <p>Please log in.</p>
}

<!-- Modern Built-in Loops -->
<ul>
  @for (item of items; track item.id) {
    <li>{{ item.name }}</li>
  } @empty {
    <li>No items available.</li>
  }
</ul>

```

---

## 3. Core Features & Data Binding Mechanisms

### Primary Angular Features

* Interpolation
* Template Expressions & Statements
* Event Binding
* Property Binding
* Attribute, Class, and Style Bindings
* Built-in & Custom Directives
* Pipe Infrastructure

### Data Binding Summary Table

| Binding Type | Syntax Pattern | Target | Flow Direction |
| --- | --- | --- | --- |
| **Interpolation** | `{{ value }}` | Element Text Content | Source to View |
| **Property Binding** | `[property]="value"` | Element/Component Property | Source to View |
| **Attribute Binding** | `[attr.aria-label]="val"` | DOM Attribute | Source to View |
| **Class Binding** | `[class.active]="flag"` | Element CSS Class List | Source to View |
| **Style Binding** | `[style.color]="val"` | Element Inline Style | Source to View |
| **Event Binding** | `(event)="handler()"` | Element/Component Event | View to Source |
| **Two-Way Binding** | `[(ngModel)]="property"` | Property + Event | Bidirectional |

### Data Binding Implementations

#### 1. One-Way Interpolation & Property Binding

```html
<h1>{{ pageTitle }}</h1>
<img [src]="imageUrl" alt="Product Image">
<td [colSpan]="2" style="background-color: yellow;">2 Columns</td>

```

#### 2. Attribute, Class, and Style Binding

```html
<button type="button" [attr.aria-label]="helpText">Help</button>
<p [style.color]="isGreen ? 'green' : 'red'">Status Indicator</p>
<p [class.special]="isSpecial" [class.danger]="!isSpecial">Dynamic Class</p>

```

#### 3. Event Binding

```html
<button type="button" (click)="onSave()">Save Changes</button>
<input (input)="onInputChange($event)" />

```

#### 4. Two-Way Binding Evolution

##### Legacy Two-Way Data Binding (`ngModel`)

```html
<!-- Standard ngModel binding -->
<input [(ngModel)]="username" />

<!-- Expanded equivalent binding -->
<input [value]="username" (input)="username = $event.target.value" />

```

##### Modern Signal Two-Way Binding (`model()`)

```typescript
// Component Definition
import { Component, model } from '@angular/core';

@Component({
  selector: 'app-custom-input',
  standalone: true,
  template: `<input [value]="value()" (input)="value.set($event.target.value)">`
})
export class CustomInputComponent {
  value = model<string>(''); // Two-way signal model
}

```

### Directives Taxonomy

1. **Component Directives:** Directives with an attached HTML template.
2. **Attribute Directives:** Modify visual appearance or behavior of standard DOM elements (e.g., `ngClass`, `ngStyle`).
3. **Structural Directives:** Change DOM layout structure by adding, removing, or manipulating DOM elements (e.g., `*ngIf`, `*ngFor`).

```typescript
// Custom Attribute Directive Example
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true
})
export class HighlightDirective {
  @Input() appHighlight = '';

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.appHighlight || 'yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight('');
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}

```

---

## 4. Angular Pipe Infrastructure

### Overview & Pure vs. Impure Execution

Pipes take input values and transform them in template expressions without mutating underlying data.

* **Pure Pipes (Default):** Executed only when primitive inputs change or when reference checks (`===`) detect a new object reference.
* **Impure Pipes:** Executed during every change detection cycle regardless of reference changes, which can impact performance if unoptimized.

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'exponential',
  standalone: true,
  pure: true // Default setting
})
export class ExponentialPipe implements PipeTransform {
  transform(value: number, exponent?: number): number {
    return Math.pow(value, isNaN(exponent) ? 1 : exponent);
  }
}

```

### Built-in Angular Pipes

* `DatePipe`: Formats date objects/strings according to locale rules.
* `UpperCasePipe`: Transforms text to uppercase.
* `LowerCasePipe`: Transforms text to lowercase.
* `CurrencyPipe`: Formats numbers as currency strings based on locale.
* `DecimalPipe`: Formats floating-point numbers into string representations.
* `PercentPipe`: Converts values into formatted percentage strings.

---

## 5. Dependency Injection (DI) & Service Architecture

### Overview

Dependency Injection is a design pattern in which a class receives its dependencies from external sources rather than instantiating them directly. This design promotes low coupling, modularity, and simplified mocking for unit tests.

### Injector Hierarchy

* **`EnvironmentInjector`:** Global injector hierarchy configurable via root injectors, routes, or `providedIn: 'root'`.
* **`NodeInjector`:** Local `component/directive-level` injector created for every HTML template element node containing components or directives.

### Comparative Study: Constructor Injection vs. Modern `inject()` API

#### Legacy Paradigm (Constructor Injection)

```typescript
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class DataService {
  getData() { return ['Data 1', 'Data 2']; }
}

@Component({ selector: 'app-legacy-di' })
export class LegacyDiComponent {
  constructor(private dataService: DataService) {}
}

```

#### Modern Paradigm (Functional `inject()` API)

```typescript
import { Component, inject } from '@angular/core';

@Component({
  selector: 'app-modern-di',
  standalone: true
})
export class ModernDiComponent {
  private dataService = inject(DataService);
}

```

### Pseudo-Implementation of Dependency Injection

```typescript
// Architectural representation of Angular's DI resolution logic
type Type<T> = new (...args: any[]) => T;

const registry = new Map<Type<any>, any>();

export const inject = <T>(targetClass: Type<T>): T => {
  if (registry.has(targetClass)) {
    return registry.get(targetClass);
  }
  const instance = new targetClass();
  registry.set(targetClass, instance);
  return instance;
};

```

---

## 6. Reactive State Management with Angular Signals

### Signals Overview

Angular Signals provide a coarse-to-fine reactivity model using reactive primitives that track dependency graphs automatically. Signals notify the framework precisely where changes occur, minimizing the overhead of full-tree DOM change detection runs.

### Core Signal Primitives

```typescript
import { Component, signal, computed, effect } from '@angular/core';

@Component({
  selector: 'app-signal-demo',
  standalone: true,
  template: `<p>Count: {{ count() }} | Double: {{ doubleCount() }}</p>`
})
export class SignalDemoComponent {
  // 1. Writable Signal
  count = signal<number>(0);

  // 2. Computed Signal (Read-Only dependent state)
  doubleCount = computed(() => this.count() * 2);

  constructor() {
    // 3. Effect (Side-effect handler triggered on signal dependency changes)
    effect(() => {
      console.log(`Current Count: ${this.count()}`);
    });
  }

  increment() {
    this.count.update(val => val + 1);
  }

  reset() {
    this.count.set(0);
  }
}

```

---

## 7. RxJS Integration & Advanced Reactive Patterns

### RxJS Core Concepts

RxJS handles asynchronous operations using data stream pipelines.

* **Observables:** Unicast data streams; execution begins only when a subscriber subscribes.
* **Subjects:** Multicast observables that broadcast messages to multiple registered observers simultaneously.
* **Cold Observables:** Data producers created inside the observable. Subscribers receive independent streams from the beginning.
* **Hot Observables:** Data producers created outside the observable. Subscribers share the same stream and receive values emitted after subscription.

### Essential RxJS Operators

#### 1. Data Stream & Conversion

* `toSignal`: Converts an RxJS Observable into an Angular Signal primitive.
* `interval`: Emits sequential numbers based on a specified time period.

#### 2. Filtering Operators

* `take(n)`: Emits the first $n$ values emitted by the source observable, then completes.
* `takeUntil(notifier)`: Emits source values until the provided `notifier` observable emits a value.
* `takeWhile(predicate)`: Emits values as long as the specified boolean expression evaluates to true.

```typescript
import { interval, Subject } from 'rxjs';
import { take, takeUntil, takeWhile } from 'rxjs/operators';

const destroy$ = new Subject<void>();

interval(1000).pipe(
  take(10), // Stop after 10 emissions
  takeWhile(val => val < 8), // Stop if condition fails
  takeUntil(destroy$) // Unsubscribe when component destroys
).subscribe(console.log);

```

#### 3. Advanced Subjects Overview

```typescript
import { BehaviorSubject, ReplaySubject } from 'rxjs';

// BehaviorSubject requires an initial value and retains the latest value
const behaviorSubject = new BehaviorSubject<string>('Initial Value');
behaviorSubject.subscribe(val => console.log(`BehaviorSubject Subscriber: ${val}`));

// ReplaySubject replays a specified number of historical emissions (Buffer size)
const replaySubject = new ReplaySubject<number>(2); // Buffers last 2 items
replaySubject.next(1);
replaySubject.next(2);
replaySubject.next(3);
replaySubject.subscribe(val => console.log(`ReplaySubject Subscriber: ${val}`)); // Output: 2, 3

```

#### 4. Parallel HTTP Operations (`forkJoin`)

`forkJoin` waits for all provided inner observables to complete before emitting an array or object containing the final emission from each.

```typescript
import { forkJoin } from 'rxjs';
import { inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';

export class ParallelHttpService {
  private http = inject(HttpClient);

  getDashboardData() {
    return forkJoin({
      users: this.http.get('/api/users'),
      stats: this.http.get('/api/stats')
    });
  }
}

```

#### 5. Error Handling Operators

* `catchError`: Intercepts errors on an observable sequence to handle them gracefully by returning a fallback observable.
* `retry`: Re-subscribes to the source observable $n$ times upon encountering an error.

```typescript
import { of } from 'rxjs';
import { catchError, retry } from 'rxjs/operators';

this.http.get('/api/data').pipe(
  retry(3), // Attempt request up to 3 times
  catchError(error => {
    console.error('Request failed after retries:', error);
    return of({ fallback: 'Default Data' }); // Fallback recovery
  })
);

```

#### 6. Backpressure Strategies

* `buffer`: Collects emitted values into array buffers until triggered by a signal.
* `throttle`: Emits the initial item, then silences subsequent emissions for a designated time window.
* `debounce`: Delays emission until a specified time passes without any new values being emitted.
* `sample`: Emits the most recent value within regular time intervals.
* `switchMap`: Cancels pending inner observables to process only the latest emitted observable.

#### 7. RxJS Schedulers

Schedulers control timing execution windows for RxJS streams:

* `asyncScheduler`: Executes tasks asynchronously using standard `setInterval`/`setTimeout`.
* `queueScheduler`: Executes tasks synchronously in a first-in, first-out (FIFO) stack block.
* `animationFrameScheduler`: Schedules work to align with the browser's repainting loop (`requestAnimationFrame`).
* `asapScheduler`: Uses microtask queues (`Promise`) to execute tasks as quickly as possible.

#### 8. Pipeable Operators vs. Patch Operators

* **Pipeable Operators (Modern):** Pure functions imported independently and passed into `.pipe()` (e.g., `map()`, `filter()`). Tree-shakable.
* **Patch Operators (Legacy):** Modified `Observable.prototype` directly (e.g., `Observable.prototype.map`). Not tree-shakable.

#### 9. Mathematical Aggregation (`reduce`)

Applies an accumulator function over an observable stream, emitting the final calculated value only when the source stream completes.

```typescript
import { of } from 'rxjs';
import { reduce } from 'rxjs/operators';

of(10, 20, 30).pipe(
  reduce((acc, curr) => acc + curr, 0)
).subscribe(total => console.log(`Total Aggregate: ${total}`)); // Output: 60

```

---

## 8. HTTP Client Infrastructure & API Interceptors

### Overview

`HttpClient` facilitates HTTP communication with external backend web services via standard protocols.

### Comparative Study: Module Setup vs. Modern Functional Setup

#### Legacy Paradigm (`HttpClientModule` & Class Interceptors)

```typescript
// Legacy AppModule
import { NgModule } from '@angular/core';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';
import { AuthInterceptor } from './auth.interceptor';

@NgModule({
  imports: [HttpClientModule],
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }
  ]
})
export class AppModule {}

```

#### Modern Paradigm (`provideHttpClient` & Functional Interceptors)

```typescript
// Modern Application Config
import { ApplicationConfig } from '@angular/core';
import { provideHttpClient, withFetch, withInterceptors } from '@angular/common/http';
import { authInterceptor } from './auth.interceptor';

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(
      withFetch(), // Enables modern Fetch API underlying engine
      withInterceptors([authInterceptor]) // Registers functional interceptors
    )
  ]
};

```

### Functional Interceptor Implementation

```typescript
import { HttpInterceptorFn } from '@angular/common/http';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authToken = 'Bearer SAMPLE_JWT_TOKEN';
  const authReq = req.clone({
    setHeaders: { Authorization: authToken }
  });
  return next(authReq);
};

```

### Key Configuration Flags

* `withFetch`: Uses the browser `fetch` API instead of `XMLHttpRequest`.
* `withInterceptors`: Configures functional interceptor chains.
* `withInterceptorsFromDi`: Supports legacy class-based interceptors injected via DI.
* `withRequestsMadeViaParent`: Forwards requests to parent injectors for execution.
* `withJsonpSupport`: Enables JSONP cross-domain data fetching.
* `withXsrfConfiguration`: Customizes anti-XSRF token and header settings.
* `withNoXsrfProtection`: Disables built-in XSRF protection.

---

## 9. Component Decorators & Annotations

### Decorator Classifications

1. **Class Decorators:** `@Component`, `@Directive`, `@NgModule`, `@Pipe`, `@Injectable`.
2. **Property Decorators:** `@Input`, `@Output`, `@ViewChild`, `@ContentChild`, `@HostBinding`.
3. **Method Decorators:** `@HostListener`.
4. **Parameter Decorators:** `@Inject`, `@Optional`, `@Self`, `@SkipSelf`.

### Comparative Study: Data I/O Decorators vs. Signals

#### Legacy I/O Decorators (`@Input`, `@Output`)

```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-legacy-io',
  template: `<button (click)="notifyParent()">Send</button>`
})
export class LegacyIoComponent {
  @Input() label = 'Submit';
  @Output() action = new EventEmitter<void>();

  notifyParent() {
    this.action.emit();
  }
}

```

#### Modern Signal I/O (`input()`, `output()`)

```typescript
import { Component, input, output } from '@angular/core';

@Component({
  selector: 'app-modern-io',
  standalone: true,
  template: `<button (click)="notifyParent()">{{ label() }}</button>`
})
export class ModernIoComponent {
  label = input<string>('Submit'); // Read-only Input Signal
  action = output<void>();          // Output emitter

  notifyParent() {
    this.action.emit();
  }
}

```

### Component & Module Metadata Flags

```typescript
// Component Decorator Properties
@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css'], // Historical array syntax
  styleUrl: './example.component.css',   // Modern single-string option
  standalone: true,                      // Indicates non-NgModule dependency
  imports: []                            // Direct dependencies array
})
export class ExampleComponent {}

// Module Decorator Properties (Historical Context)
@NgModule({
  declarations: [/* Component, Directive, Pipe definitions */],
  imports: [/* External imported NgModules */],
  exports: [/* Re-exported members */],
  providers: [/* DI Services */],
  bootstrap: [/* Root Component */],
  entryComponents: [/* Dynamically compiled components - Removed in Angular 13+ */]
})
export class LegacyModule {}

```

---

## 10. Lifecycle Hooks Architecture

### Lifecycle Call Sequence

1. `constructor()`: Class instantiation. DI dependencies injected.
2. `ngOnChanges()`: Called when input bound properties change.
3. `ngOnInit()`: Called once after data-bound inputs are initialized.
4. `ngDoCheck()`: Custom change detection run.
5. `ngAfterContentInit()`: Executed after projected content (`<ng-content>`) is initialized.
6. `ngAfterContentChecked()`: Runs after projected content is checked.
7. `ngAfterViewInit()`: Executed after component templates and child component views are initialized.
8. `ngAfterViewChecked()`: Executed after component and child views are checked.
9. `ngOnDestroy()`: Executed right before component instance destruction for resource cleanup.

```typescript
import { Component, OnInit, AfterContentInit, AfterViewInit, ViewChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-lifecycle',
  standalone: true,
  template: `<input #titleInput type="text" value="Angular">`
})
export class LifecycleComponent implements OnInit, AfterContentInit, AfterViewInit {
  @ViewChild('titleInput') inputRef!: ElementRef<HTMLInputElement>;

  ngOnInit(): void {
    // Inputs are set, view DOM not accessible yet
  }

  ngAfterContentInit(): void {
    // External Projected Content (<ng-content>) initialized
  }

  ngAfterViewInit(): void {
    // View DOM and ViewChildren accessible here
    this.inputRef.nativeElement.focus();
  }
}

```

---

## 11. View Encapsulation & Shadow DOM

### View Encapsulation Modes

* `ViewEncapsulation.Emulated` **(Default):** Styles are scoped locally by generating unique HTML attribute markers (e.g., `_ngcontent-c12`). Global styles leak in, local styles don't leak out.
* `ViewEncapsulation.ShadowDom`: Uses native Shadow DOM encapsulation. Isolates styles inside a shadow host.
* `ViewEncapsulation.None`: Styles applied globally across the entire document.

```typescript
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-encapsulated',
  standalone: true,
  template: `<h2>Encapsulated Content</h2>`,
  styles: [`h2 { color: blue; }`],
  encapsulation: ViewEncapsulation.Emulated
})
export class EncapsulatedComponent {}

```

---

## 12. Routing, Navigation Guards, and Performance

### Route Guard Architecture

Guards secure routes based on conditions (e.g., authentication status).

* `canActivate`: Controls if a route can be rendered.
* `canActivateChild`: Determines if child routes under a parent can be rendered.
* `canDeactivate`: Checks if a user can navigate away from a route.
* `canMatch`: Guards against matching a route definition.

```typescript
// Functional Route Guard Example
import { inject } from '@angular/core';
import { CanActivateFn, Router } from '@angular/router';
import { AuthService } from './auth.service';

export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isAuthenticated()) {
    return true;
  }
  return router.parseUrl('/login');
};

```

### Lazy Loading Route Setup

```typescript
import { Routes } from '@angular/router';

export const APP_ROUTES: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.routes').then(m => m.ADMIN_ROUTES)
  }
];

```

---

## 13. Compilation Modes, Runtime Engines & Performance Tuning

### AOT vs. JIT Compilation

| Property | JIT (Just-In-Time) | AOT (Ahead-Of-Time) |
| --- | --- | --- |
| **Compilation Phase** | Runtime inside client browser | Build time on server/CI runner |
| **Payload Size** | Larger (Includes Angular Compiler) | Smaller (Compiler excluded) |
| **Startup Performance** | Slower (Compiles upon page request) | Faster (Pre-compiled JavaScript) |
| **Security Risk** | Vulnerable to template injection | Highly secure; early HTML error detection |

```bash
# Compilation Build Commands
ng build --aot
ng serve --aot

```

### Change Detection Strategies & `NgZone`

* **Default Strategy:** Checks every component tree node whenever asynchronous browser events occur (e.g., DOM events, timers, HTTP calls).
* **`OnPush` Strategy:** Runs change detection only when input references change, explicit signals/events fire inside the component, or manual mark calls (`markForCheck()`) execute.

```typescript
import { Component, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-on-push',
  standalone: true,
  template: `<p>OnPush Performance Optimized</p>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OnPushComponent {}

```

#### Bypassing Zone.js Execution

```typescript
import { Component, inject, NgZone } from '@angular/core';

@Component({ selector: 'app-zone-demo', standalone: true, template: '' })
export class ZoneDemoComponent {
  private ngZone = inject(NgZone);

  runOutsideAngularZone() {
    // Execution will not trigger full Angular Change Detection cycles
    this.ngZone.runOutsideAngular(() => {
      setInterval(() => {
        // High frequency DOM operation
      }, 100);
    });
  }
}

```

### Bundle Artifact Breakdown

* `main.js`: Application entry point code, components, and logic.
* `vendor.js`: External libraries, framework core, third-party packages.
* `polyfills.js`: Compatibility scripts for older browser runtimes.
* `runtime.js`: Webpack/ESBuild chunk loading runtime environment.

---

## 14. Enterprise Security Model & Sanitization

### Security Context Scope

Angular automatically sanitizes inputs in these contexts:

* `SecurityContext.HTML`
* `SecurityContext.STYLE`
* `SecurityContext.URL`
* `SecurityContext.RESOURCE_URL`
* `SecurityContext.SCRIPT`
* `SecurityContext.NONE`

### Sanitization & Bypassing Security Trust

```typescript
import { Component, inject } from '@angular/core';
import { DomSanitizer, SafeHtml } from '@angular/platform-browser';

@Component({
  selector: 'app-security',
  standalone: true,
  template: `<div [innerHTML]="trustedHtml"></div>`
})
export class SecurityComponent {
  private sanitizer = inject(DomSanitizer);
  trustedHtml!: SafeHtml;

  updateContent(rawHtml: string) {
    // Bypasses Angular default sanitization check
    this.trustedHtml = this.sanitizer.bypassSecurityTrustHtml(rawHtml);
  }
}

```

---

## 15. CLI Commands & Auxiliary Configuration Options

### Package Installation: `ng add` vs. `npm install`

* `npm install`: Standard Node module dependency installer.
* `ng add`: Installs npm package dependencies and executes framework schematic hooks to configure Angular projects automatically.

---

## 16. Architectural Design Patterns Matrix

```
┌───────────────────────────────────────────────────────────┐
│              ANGULAR DESIGN PATTERNS MATRIX               │
├──────────────────┬────────────────────────────────────────┤
│ Pattern          │ Application Example                    │
├──────────────────┼────────────────────────────────────────┤
│ Singleton        │ Services providedIn: 'root'            │
│ Dependency Inj.  │ Constructor / inject() resolution      │
│ Observer         │ RxJS Observables / EventEmitters       │
│ Strategy         │ Dynamic Component / Control Value Acc. │
│ Decorator        │ @Component, @Injectable metadata       │
│ Facade           │ Complex state abstraction services     │
│ Composite        │ Angular Nested Tree View Components    │
│ Factory          │ Custom Injection Tokens / APP_INITIALIZER│
└──────────────────┴────────────────────────────────────────┘

```

---

## 17. How do you manage technical debt and execute zero-downtime upgrades for legacy Angular applications (e.g., v12 to v18)?

1. **Automated Schematic Upgrades:** Run step-by-step update paths (`ng update @angular/core@13 @angular/cli@13`, progressing version by version) rather than skipping major releases.
2. **Modular Migration to Standalone Components:** Use automated Angular CLI migration schematics (`ng g @angular/core:standalone`) to convert `NgModule` architectures to standalone components incrementally.
3. **Deprecation Strategy:** Mark legacy APIs with `@deprecated` comments, track technical debt metrics using SonarQube, and allocate dedicated refactoring time during sprint planning.
4. **Automated Regression Pipelines:** Use E2E Cypress/Playwright suites and visual regression tests across staging environments to validate builds prior to production deployments.

## 18. How do you define and enforce performance budgets, linting rules, and code standards across a large team?

* **Performance Budget Enforcers:** Configure bundle size thresholds directly inside `angular.json` to fail builds if bundle boundaries are exceeded:

```json
"budgets": [
  {
    "type": "initial",
    "maximumWarning": "500kB",
    "maximumError": "1MB"
  },
  {
    "type": "anyComponentStyle",
    "maximumWarning": "4kB",
    "maximumError": "8kB"
  }
]
```