# Angular 21 Documentation Guide

## What is Angular 21?

Angular 20 was about stabilizing Signals; Angular 21 is about removing the old guard. The "Angular Way" has fundamentally changed: `zone.js` is optional, Karma is dead, and RxJS is slowly retreating to the edges.

* **Signal Forms**: A replacement of reactive forms as a simpler state management method.
* **Zoneless Change Detection**: Improves the performance of an app by removing unwanted patches.
* **Enhanced Developer Tooling & AI-Assisted Diagnostics**: Shortens the time of debugging and testing.
* **Experimental ARIA Components**: Makes web applications more accessible and inclusive.

Before running `ng update`, be aware that your build will likely fail if you rely on legacy patterns.

## The Karma Extinction Event (Vitest is Default)

Angular 21 has officially swapped Karma for Vitest as the default test runner.

*What Breaks:*
If you have a custom `karma.conf.js` or rely on specific Karma `plugins/reporters`, your test suite is now legacy code.

*The Fix:*

* **New Projects:** You get Vitest out of the box. It's faster, cleaner, and uses Vite.
* **Existing Projects:** You aren't forced to switch immediately, but the CLI will alert you.
* **Migration:** Run the schematic `ng generate @angular/core:karma-to-vitest` to attempt an auto-migration. It is effective at converting standard configs, but custom Webpack setups in tests will need manual rewriting for Vite.

## HttpClient Updates

In previous versions, you imported `HttpClientModule` or called `provideHttpClient()` in `app.config.ts`.

*The Change:* `HttpClient` is now injected by default in the root injector.

*Legacy Configuration:*

```typescript
// Legacy setup in app.config.ts
import { provideHttpClient } from '@angular/common/http';

export const appConfig: ApplicationConfig = {
  providers: [provideHttpClient()]
};
```

*Modern Angular 21 Setup:*
`HttpClient` is injected into the root injector automatically without requiring manual provider setup.

*What Breaks:*

* If you have tests that mock `HttpClient` by expecting it not to be present, they might fail.
* If you rely on `HttpClientModule` for complex interceptor ordering in a mixed NgModule/Standalone app, you might see subtle behavior changes.

*The Fix:*
Remove explicit `provideHttpClient()` calls unless passing configuration options (like `withInterceptors` or `withFetch`).

## `zone.js` is Gone (For New Apps)

New apps generated with `ng new` exclude `zone.js` by default.

*What Breaks:*
Nothing for existing apps yet. Your `polyfills.ts` will keep importing Zone.

*The Warning:*
If you copy-paste code from a new v21 tutorial into an existing v20 app, it might assume Zoneless behavior (using `ChangeDetectorRef` less often, relying on `Signals`). Mixing paradigms without understanding them leads to "changed after checked" errors or un-updated views.

## Signal Forms

Signal Forms eliminate `valueChanges.pipe(...)` complexity.

```typescript
// Modern Signal Forms Syntax
import { form, field } from '@angular/forms/signals';
import { Validators } from '@angular/forms';

// Define a reactive form model
const loginForm = form({
  email: field('', [Validators.required, Validators.email]),
  password: field('', [Validators.required])
});

// Access values directly as signals!
console.log(loginForm.value().email);
```

* **Why use it:** Type-safe by default without requiring RxJS mastery.
* **Status:** Experimental. Use it for new features, but avoid rewriting complex checkout flows immediately.

## Angular ARIA (Developer Preview)

A new library of headless primitives for accessibility. Instead of managing `aria-expanded` and `role="button"` manually, these directives handle accessibility logic while you style with CSS.

```html
<!-- Handles keyboard navigation, focus, and ARIA roles automatically -->
<div ariaMenu>
  <button ariaMenuItem>Option 1</button>
  <button ariaMenuItem>Option 2</button>
</div>
```

## Regex in Templates

Regex literals are supported in template expressions, allowing pattern matching directly in `@if` logic.

```html
@if (email() | match: /@company\.com$/) {
  <span class="badge">Employee</span>
}
```

## Step 1: Upgrade Checklist

* **Backup:** Commit all project files prior to starting.
* **Update Global CLI:** Updating Angular involves two parts: global CLI and local dependencies.

```bash
# Optional: Uninstall old global version first
npm uninstall -g @angular/cli

# Verify npm cache
npm cache verify

# Install latest global CLI
npm install -g @angular/cli@latest

# Update local project dependencies
ng update @angular/cli@21 @angular/core@21
```

* **Run Diagnostics:** Angular 21 includes diagnostics. Review warnings about `ngClass` (soft deprecated in favor of `[class.my-class]`) and standalone migrations.
* **Check Your Tests:** Run `ng test`.
* *Path A:* Keep Karma (add `@angular/build:karma` manually if removed).
* *Path B:* Migrate to Vitest (Recommended).

* **Optional: Enable Zoneless Mode:** Execute the experimental migration schematic:

```bash
ng generate @angular/core:zoneless-migration
```

## Step 2: Switch from Reactive Forms to Signal Forms

Angular 21 marks the beginning of Signal Forms.

*Legacy Reactive Forms vs Modern Signal Forms:*

```typescript
// Legacy Reactive Form
import { FormGroup, FormControl } from '@angular/forms';

form = new FormGroup({ 
  name: new FormControl(''), 
}); 

// Modern Signal Form 
import { signalForm, field } from '@angular/forms'; 

form = signalForm({ 
  name: field.string(''), 
}); 
```

*Why Migrate:*

* Faster and more predictable.
* Eliminates subscriptions.
* Simplifies component lifecycle.

## Step 3: Prepare for Zoneless Mode

Zoneless mode is not enabled by default during migration. Remove `zone.js` dependencies when ready.

*Enable Zoneless Mode:*

```typescript
import { bootstrapApplication } from '@angular/platform-browser';
import { provideExperimentalZonelessChangeDetection } from '@angular/core'; 
import { AppComponent } from './app.component';

bootstrapApplication(AppComponent, { 
  providers: [
    provideExperimentalZonelessChangeDetection()
  ] 
});

```

*Review checklist:*

* Manual event listeners.
* Third-party libraries requiring `zone.js`.
* Auto-triggered change detection assumptions.

## Step 4: Update Tests from Karma/Jasmine to Vitest

Angular 21 officially encourages Vitest for new and migrated projects.

```bash
# Install Vitest
ng add @angular/vitest 
```

*Legacy Jasmine Test vs Modern Vitest Test:*

```typescript
// Legacy (Jasmine)
it('should add numbers', () => { 
  expect(add(2, 3)).toEqual(5); 
}); 

// Modern (Vitest)
import { describe, it, expect } from 'vitest'; 
import { add } from './math'; 

describe('Math Utils', () => { 
  it('should add numbers correctly', () => { 
    expect(add(2, 3)).toBe(5); 
  }); 
}); 
```

## Step 5: Run Post-Migration Validation

```bash
# Check for deprecated APIs  
ng lint 
ng build 

# Run full test suite 
ng test 

# Rebuild production version 
ng build --configuration production 
```