# What is Angular 20?

Angular 20 is the newest version of Google’s web framework that helps you build websites and apps. It was officially released on May 29, 2025, and introduces key architectural improvements to enhance performance and developer experience:

* Control flow syntax (`@if`, `@for`, `@switch`) is stable.
* Standalone components are now the default.
* `zone.js` is optional, offering better developer ergonomics.
* Signals receive framework-level support.
* Server-Side Rendering (SSR) is faster, lighter, and streamlined.
* Forms support enhanced type inference.
* Angular CLI and dev server offer improved performance.
* Replaces `TestBed.get()` with `TestBed.inject()`.
* Updates `*ngIf`, `*ngFor`, and `*ngSwitch` to modern built-in control flow syntax.

---

## Why did Angular drop Karma?

The default build package changes from `@angular-devkit/build-angular` to `@angular/build`. This package excludes the Karma plugin used by legacy test setups. The ecosystem has shifted to faster, modern Node.js-based test runners like Vitest and Jest that leverage Vite and esbuild.

---

## What the New World Looks Like (Vitest/Jest)

Angular's test runner powered by Vitest provides a fast, modern testing environment. If you need to legacy-support Karma during migration, reinstall the legacy build package:

```bash
npm install @angular-devkit/build-angular --save-dev
```

---

## Prerequisites

* **Node.js v20+**: Angular 20 drops support for Node 18. Check your version with `node -v`.
* **TypeScript 5.8+**: Upgrade TypeScript via npm:

```bash
npm install typescript@5.8 --save-dev
```

---

## Step-by-Step Upgrade Guide

### Step 1. Update Angular CLI

```bash
npm uninstall -g @angular/cli
npm install -g @angular/cli@20
```

### Step 2. Upgrade Project Dependencies

```bash
npm run ng update @angular/cli@20 @angular/core@20

# Upgrade to the latest prerelease/next version if required
ng update @angular/cli @angular/core --next

# Update specific package dependencies
npm run ng update @angular/material 
npm run ng update @angular/forms @angular/router
npm run ng update @angular/common@20 @angular/material@20 @angular/animations@20 @angular/platform-browser@20
npm install @ng-bootstrap/ng-bootstrap@17.0.0 --legacy-peer-deps
```

### Step 3. Migrate Control Flow Syntax

Migrate template directives (`*ngIf`, `*ngFor`, `*ngSwitch`) to the built-in control flow blocks.

**Conditionals (`*ngIf` vs `@if`):**

```html
<!-- Legacy syntax -->
<div *ngIf="user">{{ user.name }}</div>

<!-- Modern syntax -->
@if (user) {
  <div>{{ user.name }}</div>
}
```

**Loops (`*ngFor` vs `@for`):**

```html
<!-- Legacy syntax -->
<div *ngFor="let item of items; trackBy: trackItemById">{{ item.name }}</div>

<!-- Modern syntax -->
@for (item of items; track item.id) {
  <div>{{ item.name }}</div>
} @empty {
  <div>No items to display.</div>
}
```

* `track` is mandatory in modern syntax to enforce rendering performance.
* `@empty` handles empty collection states natively without nested checks.

**Switch Statement (`*ngSwitch` vs `@switch`):**

```html
<!-- Legacy syntax -->
<div [ngSwitch]="role">
  <div *ngSwitchCase="'admin'">Admin Panel</div>
  <div *ngSwitchDefault>User Panel</div>
</div>

<!-- Modern syntax -->
@switch (role) {
  @case ('admin') {
    <div>Admin Panel</div>
  }
  @default {
    <div>User Panel</div>
  }
}
```

**Enhanced String Templates:**

Mix strings, evaluation operations, and template expressions seamlessly inside templates:

```html
<!-- Legacy evaluation -->
<p>Hello {{ name }} from {{ city }}</p>

<!-- Modern template strings -->
<p>{{ `Hello ${name} from ${city}` }}</p>
```

### Step 4. Replace `TestBed.get()` with `TestBed.inject()`

`TestBed.get()` is fully removed in favor of the type-safe `TestBed.inject()` API.

```typescript
// Legacy
const service = TestBed.get(UserService);

// Modern
const service = TestBed.inject(UserService);
```

### Step 5. Update Forms API

Utilize modern Form group helper methods such as `markAllAsDirty()`:

```typescript
this.userForm.markAllAsDirty();
```

### Step 6. Enable Zoneless Change Detection

Zoneless mode is in Developer Preview in Angular 20. It optimizes performance by reducing overhead:

* Faster app load times
* Reduced memory usage
* Smaller bundle size
* Improved runtime stability

Configure `app.config.ts`:

```typescript
import { ApplicationConfig, provideZonelessChangeDetection } from '@angular/core';

export const appConfig: ApplicationConfig = {
  providers: [
    provideZonelessChangeDetection()
  ]
};
```

*Note: Manual change detection triggers may be required for legacy third-party libraries.*

### Step 7. Integrate Signals

Signals provide granular, reactive state management across your application:

```typescript
import { signal, computed } from '@angular/core';

const name = signal('');
const isValid = computed(() => name().length > 2);
```

### Step 8. Update Browserslist Configuration

Angular 20 officially drops support for Opera. Angular 20 targets modern browsers released within the last 30 months. Update `.browserslistrc`:

```text
Chrome >= 107
Firefox >= 104
Safari >= 16
```

### Step 9. Automated Migrations and Schematics

Execute built-in CLI schematics to automate modern pattern conversions across your codebase:

```bash
# Convert template directives to modern control flow (@if, @for, @switch)
ng generate @angular/core:control-flow

# Additional framework schematics
ng generate @angular/core:standalone
ng generate @angular/core:inject
ng generate @angular/core:route-lazy-loading
ng generate @angular/core:signal-input-migration
ng generate @angular/core:signal-queries-migration
ng generate @angular/core:output-migration
```

### Step 10. Direct State Updates in Zoneless Mode

In a zoneless application, UI state changes occur via direct Signal updates without relying on zone-based digest cycles:

```typescript
mySignal.set(newValue);
```

This targets DOM updates specifically where the signal is read, enabling high-performance rendering.

### Step 11. Component Event Error Checking

Angular 20 introduces strict compile-time checking for component events and decorators like `@HostBinding` and `@HostListener`, identifying invalid signatures prior to execution.

### Step 12. Test and Optimize

* **Run Tests:** Angular 20 deprecates Karma. Migrate test suites from Karma to Vitest or Web Test Runner.
* **Check Bundle Size:** Generate stats with `ng build --stats-json` and evaluate output using `webpack-bundle-analyzer`.
* **Verify SSR:** Validate server-side rendering functionality using `ng serve --ssr`.
* **TypeScript Settings:** Enable strict type checking in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

### Step 13. Troubleshooting Common Issues

* **Node.js / TypeScript Compatibility:** Verify Node.js is v20+ and TypeScript is version 5.8 or higher.
* **Deprecated Syntax:** While legacy directives (`*ngIf`, `*ngFor`) may function temporarily in transitional builds, migrate to `@if` and `@for`.
* **"Cannot find module" Errors:** Clean workspace artifacts and reinstall dependencies:

```bash
rm -rf node_modules package-lock.json
npm install
```

* **Zone.js Warnings:** If using zoneless mode, pass `ngZone: 'noop'` to `provideZonelessChangeDetection()` configuration where applicable.
* **Legacy Browser Requirements:** Extending target options in `.browserslistrc` to legacy browsers will increase output bundle sizes.
* **Deprecated APIs:** Update usages of `InjectFlags` to the functional `inject()` options object API.

---

## Architecture & Naming Conventions

### Standalone Components by Default

Modules are optional. Components explicitly declare dependencies using the `imports` array:

* Clearer module boundaries and architecture
* Improved tree-shaking capabilities
* Reduced final application bundle sizes

### Modern File Naming Conventions

Angular 20 updates official file naming standards by dropping structural suffixes from filenames.

**Legacy Naming Pattern:**

* `user-profile.component.ts`
* `auth.service.ts`
* `highlight.directive.ts`

**Modern Naming Pattern:**

* `user-api.ts` (API/HTTP requests)
* `user-profile.ts` (UI Component)
* `auth-store.ts` (State management)
* `highlight.ts` (Directive)

**Feature-Based Directory Structure:**

```text
src/
├── core/
│   └── auth/
│       ├── auth-store.ts
│       ├── login.ts
│       └── register.ts
├── features/
│   └── users/
│       ├── user-profile.ts
│       ├── user-api.ts
│       └── user-settings.ts
```

---

## Post-Upgrade Verification Checklist

Verify the following functional areas after completion of the upgrade:

* Ensure third-party libraries (e.g., NgRx, RxJS) are compatible. 
* All application routes and pages load without errors.
* Form state management and validation logic operate correctly.
* User authentication state (login/logout) functions as expected.
* Data persistence operations run cleanly.
* Responsive viewports and mobile features render properly.
* Performance baseline meets targets.

Run automated execution suites:

```bash
ng test
ng e2e
```