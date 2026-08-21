# Angular Architecture & Performance Concepts

## 1. Angular SSR — Server-Side Rendering

Angular Server-Side Rendering (SSR) is a technique where Angular renders the initial HTML of a page **on the server** instead of rendering everything in the browser. This improves initial page rendering performance and Search Engine Optimization (SEO). Angular hydration then attaches client-side Angular behavior to the server-rendered HTML.

### Installation & Setup

#### Modern (Angular 17+)

```bash
ng add @angular/ssr
```

#### Legacy (Angular Universal)

```bash
ng add @nguniversal/express-engine
```

### Conceptual Flow

```text
Browser
   ↓
Request Angular page
   ↓
Angular server renders HTML
   ↓
HTML sent to browser
   ↓
Browser displays content immediately
   ↓
Angular hydrates the page
   ↓
Application becomes interactive
```

### Why use Angular SSR?

* Better initial page load performance
* Better SEO (Search Engine Optimization)
* Faster first content display (First Contentful Paint)
* Better social-media previews and Open Graph cards
* Useful for content-heavy applications
* Search engines can receive meaningful HTML without waiting for client-side JavaScript execution

---

## 2. Angular CSR (Client-Side Rendering)

In Client-Side Rendering (CSR), the browser receives a bare minimum HTML page, downloads the client-side JavaScript bundle, bootstraps the Angular application, fetches data from APIs, and renders the DOM elements directly in the browser.

### Conceptual Flow

```text
Browser
   ↓
Download HTML
   ↓
Download JavaScript
   ↓
Angular bootstraps
   ↓
API calls
   ↓
Render UI
```

### Important Distinction Across Rendering Strategies

* **CSR (Client-Side Rendering)**: HTML is generated in the **Browser**
* **SSR (Server-Side Rendering)**: HTML is generated on the **Server**
* **SSG (Static Site Generation)**: HTML is generated at **Build time**
* **Hydration**: Connects client-side Angular logic to existing server-rendered HTML

---

## 3. What is Angular Hydration?

Angular hydration is the process where the client-side framework reuses existing server-rendered HTML to make the page interactive instead of destroying and rebuilding the DOM nodes. When a Server-Side Rendered (SSR) Angular application loads in the browser, the static HTML displays immediately. Angular then attaches event listeners, binds component states, and enables change detection directly onto that structure.

### Conceptual Flow

```text
Server
  ↓
Render HTML
  ↓
Browser receives HTML
  ↓
User sees content
  ↓
Angular hydration
  ↓
Events + bindings become active
```

### Configuration Examples

#### Modern Standalone Setup (Angular 17+)

```typescript
import { bootstrapApplication } from '@angular/platform-browser';
import { provideClientHydration } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';

bootstrapApplication(AppComponent, {
  providers: [
    provideClientHydration()
  ]
});
```

#### Legacy Module-Based Setup (Angular Universal)

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

@NgModule({
  imports: [
    BrowserModule.withServerTransition({ appId: 'serverApp' })
  ]
})
export class AppModule {}
```

### Component Example

```html
<button (click)="addToCart()">
  Add to Cart
</button>
```

SSR generates the button HTML structure on the server, while hydration allows Angular to attach the client-side `(click)` event behavior in the browser.

---

## 4. Angular CDK

**Angular CDK (Component Dev Kit)** is a collection of reusable Angular utilities for building UI components without forcing you to use Angular Material's visual design.

Angular CDK provides low-level, reusable building blocks such as drag-and-drop, overlays, portals, accessibility utilities, and virtual scrolling. Angular Material internally relies on CDK functionality.

> **Key Takeaway:** CDK provides behavior and functionality; Angular Material provides ready-made, styled UI components.

### CDK Feature Architecture

```text
Angular CDK
 ├── Drag & Drop
 ├── Overlay
 ├── Portal
 ├── Accessibility
 ├── Virtual Scrolling
 ├── Clipboard
 ├── Layout
 ├── Dialog infrastructure
 └── Table utilities
```

### Installation

```bash
npm install @angular/cdk

```

### CDK Drag and Drop Examples

#### Modern Standalone Angular Component

```typescript
import { Component } from '@angular/core';
import { DragDropModule } from '@angular/cdk/drag-drop';

@Component({
  standalone: true,
  selector: 'app-drag-drop',
  imports: [DragDropModule],
  template: `
    <div cdkDrag class="drag-box">
      Drag me around!
    </div>
  `
})
export class DragDropComponent {}
```

#### Legacy Module-Based Angular Setup

```typescript
import { NgModule } from '@angular/core';
import { DragDropModule } from '@angular/cdk/drag-drop';

@NgModule({
  imports: [
    DragDropModule
  ]
})
export class AppModule {}
```

### CDK Virtual Scrolling

CDK Virtual Scrolling is particularly useful when displaying thousands of records, as it only renders items currently visible in the DOM viewport.

#### Modern Standalone Component Example

```typescript
import { Component } from '@angular/core';
import { ScrollingModule } from '@angular/cdk/scrolling';

@Component({
  standalone: true,
  selector: 'app-virtual-scroll',
  imports: [ScrollingModule],
  template: `
    <cdk-virtual-scroll-viewport itemSize="50" style="height: 300px">
      <div *cdkVirtualFor="let user of users" class="user-item">
        {{ user.name }}
      </div>
    </cdk-virtual-scroll-viewport>
  `
})
export class VirtualScrollComponent {
  users = Array.from({ length: 10000 }, (_, index) => ({
    id: index + 1,
    name: `User ${index + 1}`
  }));
}
```

#### Legacy Module Import

```typescript
import { NgModule } from '@angular/core';
import { ScrollingModule } from '@angular/cdk/scrolling';

@NgModule({
  imports: [
    ScrollingModule
  ]
})
export class AppModule {}
```

---

## 5. OnPush Change Detection

`OnPush` is a change detection strategy that allows Angular to skip unnecessary checks for a component subtree. It works particularly well with immutable data and observable-based or signal-based architectures to improve runtime performance in large Angular applications.

Angular's default change detection strategy is `Default`, checks components on every change cycle:

```typescript
ChangeDetectionStrategy.Default
```

With `OnPush`, Angular uses an optimized strategy and checks the component only when specific conditions occur:

```typescript
ChangeDetectionStrategy.OnPush
```

### Component Configuration Examples

#### Modern Standalone with OnPush Strategy

```typescript
import { Component, ChangeDetectionStrategy, Input } from '@angular/core';

@Component({
  standalone: true,
  selector: 'app-user',
  template: `
    <h2>{{ user.name }}</h2>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserComponent {
  @Input({ required: true }) user!: { name: string };
}
```

#### Legacy Module-Declared Component with Default Strategy

```typescript
import { Component, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-user',
  template: `
    <h2>{{ user.name }}</h2>
  `,
  changeDetection: ChangeDetectionStrategy.Default
})
export class UserComponent {
  user = {
    name: 'John'
  };
}
```

### Important OnPush Triggers

An `OnPush` component will trigger change detection when:

#### 1. Input Reference Changes

Passing a new object reference triggers updates:

```typescript
// Mutating object directly won't trigger OnPush:
// this.user.name = 'David'; 

// Creating a new reference triggers OnPush:
this.user = {
  ...this.user,
  name: 'David'
};
```

#### 2. Event Occurs Inside the Component

Events originating within the component or its children trigger checks:

```html
<button (click)="save()">
  Save
</button>

```

#### 3. An Observable Used with Async Pipe Emits (or Signals Update)

Using RxJS `async` pipe or modern Angular Signals automatically marks the view for check:

```html
<div>
  {{ user$ | async }}
</div>
```

#### 4. Explicit Change Detection is Requested

Using `ChangeDetectorRef` programmatically:

```typescript
import { Component, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-user-manual',
  template: `<div>{{ name }}</div>`
})
export class UserManualComponent {
  name = 'Initial';

  constructor(private cdr: ChangeDetectorRef) {}

  updateName() {
    this.name = 'Updated';
    this.cdr.markForCheck();
  }
}
```

---

## 6. trackBy & Modern Control Flow

Tracking item identities prevents Angular from recreating DOM elements unnecessarily when array references change.

### Legacy (`*ngFor` with `trackBy`)

```typescript
import { Component } from '@angular/core';

interface User {
  id: number;
  name: string;
}

@Component({
  selector: 'app-legacy-trackby',
  template: `
    <div *ngFor="let user of users; trackBy: trackByUserId">
      {{ user.name }}
    </div>
  `
})
export class LegacyTrackByComponent {
  users: User[] = [];

  trackByUserId(index: number, user: User): number {
    return user.id;
  }
}
```

### Modern Built-in Control Flow (`@for` with `track`)

```typescript
import { Component } from '@angular/core';

interface User {
  id: number;
  name: string;
}

@Component({
  standalone: true,
  selector: 'app-modern-track',
  template: `
    @for (user of users; track user.id) {
      <div>
        {{ user.name }}
      </div>
    } @empty {
      <p>No users found.</p>
    }
  `
})
export class ModernTrackComponent {
  users: User[] = [];
}
```

When changes occur, Angular reuses existing DOM nodes matching the item keys.

### High-Performance Rendering Architecture

```text
@for / track (or trackBy)
       +
OnPush Change Detection
       +
CDK Virtual Scrolling
       +
Pagination
       +
Server-Side Filtering
```

---

## 7. Lazy-Loaded Modules and Routes

**Lazy loading** means loading application feature chunks on demand when the user navigates to a specific route rather than at application launch.

### Legacy Lazy-Loaded Module Architecture

```text
src/app/
│
├── app.module.ts
├── app-routing.module.ts
│
└── admin/
    ├── admin.module.ts
    ├── admin-routing.module.ts
    └── admin.component.ts

```

#### Legacy Admin Module & Routing

```typescript
// admin-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AdminComponent } from './admin.component';

const routes: Routes = [
  {
    path: '',
    component: AdminComponent
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class AdminRoutingModule {}

// admin.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { AdminComponent } from './admin.component';
import { AdminRoutingModule } from './admin-routing.module';

@NgModule({
  declarations: [AdminComponent],
  imports: [
    CommonModule,
    AdminRoutingModule
  ]
})
export class AdminModule {}

// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () =>
      import('./admin/admin.module').then(m => m.AdminModule)
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```

### Modern Standalone Lazy Loading Architecture

Modern Angular applications commonly use standalone components, eliminating `NgModule` declarations entirely.

#### Single Standalone Component Lazy Loading

```typescript
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
  {
    path: 'admin',
    loadComponent: () =>
      import('./admin/admin.component').then(m => m.AdminComponent)
  }
];
```

#### Group of Lazy-Loaded Routes

```typescript
// admin.routes.ts
import { Routes } from '@angular/router';
import { AdminComponent } from './admin.component';
import { AdminDashboardComponent } from './admin-dashboard.component';

export const ADMIN_ROUTES: Routes = [
  {
    path: '',
    component: AdminComponent,
    children: [
      { path: 'dashboard', component: AdminDashboardComponent }
    ]
  }
];

// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () =>
      import('./admin/admin.routes').then(m => m.ADMIN_ROUTES)
  }
];
```

### Lazy Loading Execution Sequence

```text
Browser
   ↓
Navigate /admin
   ↓
Router
   ↓
loadChildren() / loadComponent()
   ↓
Download admin chunk
   ↓
Initialize Module / Component
   ↓
Display AdminComponent

```

### Optimizing Feature Processing for Large Datasets

```text
Users Feature
     │
     ├── Lazy Route
     │
     ├── OnPush Change Detection
     │
     ├── @for + track user.id
     │
     ├── CDK Virtual Scroll
     │
     └── Server-Side Pagination
```

---

## 8. Tree Shaking

**Tree shaking** is a build-time optimization process executed by bundlers (such as esbuild or Webpack) that analyzes static ES module `import` and `export` statements to remove unused code from the final production bundle.

---

## 9. Dead-Code Elimination

**Dead-code elimination (DCE)** is a compiler optimization technique that removes code that can never be executed (such as unreachable code following a return statement or code inside `if (false)` conditions) or code whose results are never used elsewhere in the application.

---

## 10. Vendor Chunks

**Vendor chunks** traditionally aggregate third-party dependencies (e.g., RxJS, Angular core libraries, third-party utilities) separately from application source code to optimize HTTP caching and browser reuse. Modern Angular build toolchains utilizing esbuild create optimized shared code chunks instead of a single monolithic `vendor.js` file.