# What to Angular 21?

Angular 20 was about stabilizing Signals, Angular 21 is about removing the old guard. The "Angular Way" has fundamentally changed: zone.js is optional, Karma is dead, and RxJS is slowly retreating to the edges.

- `Signal Forms`: A replacement of reactive forms as a simpler state management method. 
- `Zoneless Change Detection`: Improves the performance of an app by removing unwanted patches. 
- `Enhanced Developer Tooling & AI-Assisted Diagnostics`: Shortens the time of debugging and testing. 
- `Experimental ARIA Components`: Makes web applications more accessible and inclusive. 

Before you run `ng update`, be aware that your build will likely fail if you rely on these legacy patterns.

## The Karma Extinction Event (Vitest is Default)
Angular 21 has officially swapped Karma for Vitest as the default test runner.

What breaks: 

If you have a custom `karma.conf.js` or rely on specific Karma `plugins/reporters`, your test suite is now legacy code.

The Fix:

- `New Projects:` You get Vitest out of the box. It's faster, cleaner, and uses Vite.
- `Existing Projects:` You aren't forced to switch immediately, but the writing is on the wall. The CLI will nag you.
- `Migration:` Run the schematic `ng generate @angular/core:karma-to-vitest` to attempt an auto-migration. It's remarkably good at converting standard configs, but custom Webpack hacks in your test setup will need manual rewriting for Vite.

## HttpClient
Remember adding `provideHttpClient()` to your `app.config.ts` or importing `HttpClientModule`?

`The Change:` HttpClient is now injected by default in the root injector.

What breaks:

If you have tests that mock `HttpClient` by expecting it not to be there, they might fail.
If you rely on `HttpClientModule` for complex interceptor ordering in a mixed NgModule/Standalone app, you might see subtle behavior changes.

The Fix: 

Remove explicit `provideHttpClient()` calls unless you are passing configuration options (like `withInterceptors` or `withFetch`). It cleans up your config, but check your interceptor execution order.

## zone.js is Gone (For New Apps)
New apps generated with ng new will exclude zone.js by default.

What breaks: 

Nothing for existing apps (yet). Your polyfils.ts will keep importing Zone.

The Warning: 

If you copy-paste code from a new v21 tutorial into your existing v20 app, it might assume Zoneless behavior (using `ChangeDetectorRef` less often, relying on `Signals`). If you mix the two paradigms without understanding them, you'll get "changed after checked" errors or views that don't update.

Once you fix the build, v21 offers some incredible DX improvements.

## Signal Forms
This is the feature we've been waiting for. No more valueChanges.pipe(...) spaghetti.

```typescript
import { form, field } from '@angular/forms/signals';

// Define a reactive form model
const loginForm = form({
  email: field('', [Validators.required, Validators.email]),
  password: field('', [Validators.required])
});

// Access values directly as signals!
console.log(loginForm.value().email);
```

Why use it: It's type-safe by default and doesn't require RxJS mastery.

Status: Experimental. Use it for new features, but maybe don't rewrite your checkout flow just yet.

## Angular Aria (Developer Preview)
A new library of headless primitives for accessibility.

Instead of fighting with aria-expanded and `role="button"`, you use directives that handle the a11y logic while you handle the CSS.

```html
<!-- Handles keyboard nav, focus, and ARIA roles automatically -->
<div ariaMenu>
  <button ariaMenuItem>Option 1</button>
  <button ariaMenuItem>Option 2</button>
</div>
```

## Regex in Templates
Small but mighty. You can finally use regex literals in templates, perfect for @if logic without creating a helper function.

```html

@if (email() | match: /@company\.com$/) {
  <span class="badge">Employee</span>
}
```

## **Step 1:** Upgrade Checklist

Backup: Commit everything. Seriously.

Update the Global CLI: Updating Angular generally involves two parts: the global CLI and the local project dependencies.

```bash

# Optional: Uninstall the old global version first to avoid conflicts
npm uninstall -g @angular/cli

# Verify the npm cache
npm cache verify

# Install the latest global CLI version
npm install -g @angular/cli@latest

# Update Local Project: Now update your local project dependencies:
ng update @angular/cli@21 @angular/core@21

# Run the Diagnostics: Angular 21 includes smarter diagnostics. 
# Pay attention to warnings about ngClass (soft deprecated in favor of [class.my-class]) and standalone migration opportunities.

# Check Your Tests:
ng test.

# Path A: Keep Karma (add @angular/build:karma manually if removed).
# Path B: Migrate to Vitest (Recommended).

# Optional: Go Zoneless: If you're feeling brave, run the experimental migration:
ng generate @angular/core:zoneless-migration
```

## **Step 2:** Switch from Reactive Forms to Signal Forms (Optional but Recommended) 
Angular 21 marks the beginning of the future of forms with Signal Forms.

```typescript
// Old Reactive Form
form = new FormGroup({ 
  name: new FormControl(''), 
}); 

// New Signal Form 
import { signalForm, field } from '@angular/forms'; 
form = signalForm({ 
  name: field.string(''), 
}); 
```

Why migrate?

- Faster & more predictable. 
- Eliminates subscriptions. 
- Simpler component lifecycle. 

## **Step 3:** Prepare for Zoneless Mode 

Zoneless mode is not enabled by default during migration. Start by removing `zone.js` dependency when ready. 

Enable Zoneless Mode:

```typescript
import { provideExperimentalZonelessChangeDetection } from '@angular/core'; 
bootstrapApplication(AppComponent, { 
providers: [provideExperimentalZonelessChangeDetection()] 
});
```
Check your code for:

- Manual event Listeners.
- Third-party libraries requiring zone.js. 
- Auto-triggered change detection assumptions. 

## **Step 4:** Update Tests from Karma/Jasmine to Vitest 

Angular 21 officially encourages Vite for new & migrated projects. 

```bash
# Install Vitest
ng add @angular/vitest 
```

```typescript
// Before (Jasmine): 

it('should add numbers', () => { 
  expect(add(2, 3)).toEqual(5); 
}); 

// After (Vitest): 

import { it, expect } from 'vitest'; 
it('adds numbers', () => { 
  expect(add(2, 3)).toBe(5); 
}); 

import { describe, it, expect } from 'vitest'; 
import { add } from './math'; 
describe('Math Utils', () => { 
  it('should add numbers correctly', () => { 
    expect(add(2, 3)).toBe(5); 
  }); 
}); 
```

## **Step 6:** Run Post-Migration Validation 

```bash
# Check for deprecated APIs  
ng lint 
ng build 

# Run full test suite 
ng test 

# Rebuild production version 
ng build --configuration production 
```

## 🔧 General Angular Build Optimization Strategies

### 1. Enable Production Mode
- Always build with:
  ```bash
  ng build --configuration production
  ```
- This ensures Angular runs optimizations like Ahead-of-Time (AOT) compilation, minification, and tree-shaking.

### 2. Use Standalone Components & Lazy Loading
- Standalone components reduce NgModule overhead.
- Lazy load large feature modules (e.g., `DashboardModule`) so they don’t inflate the initial bundle.

### 3. Optimize CSS & Styles
- Use **SCSS** or **Tailwind** with purge settings to remove unused styles.
- Enable CSS minification and consider splitting global styles into smaller chunks.

### 4. Remove Unused Polyfills & Scripts
- Check `polyfills.ts` and `angular.json` for legacy scripts you don’t need.
- Example: If you target modern browsers, you may not need ES5 polyfills.

### 5. Bundle & Asset Optimization
- Compress assets with gzip or Brotli at the server level.
- Use Angular CLI options:
  ```json
  "optimization": true,
  "outputHashing": "all",
  "extractLicenses": true,
  "sourceMap": false
  ```

### 6. Code Splitting & Preloading
- Use Angular’s `PreloadAllModules` strategy for critical lazy modules.
- Break large components into smaller chunks if possible.

### 7. Third-Party Library Audit
- Replace heavy libraries with lighter alternatives.
- Example: Use `date-fns` instead of `moment.js`.

---

## 📊 Applying to Your Build Output

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
   - Use ESBuild for faster and smaller builds.

3. **Check `scripts.js` (107 kB)**  
   - Likely external scripts added in `angular.json`.  
   - Remove or lazy load them if not critical.

4. **Lazy Loading is Good**  
   - Your `dashboard.module` is only 4.7 kB, which is excellent.  
   - Keep feature modules lazy loaded.

---

## 🌐 Method 1: Angular-Specific Build Optimizations

These are settings and practices you can apply directly in Angular projects:

### Angular.json Production Settings
```json
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
```

- **optimization** → enables minification and tree-shaking.  
- **buildOptimizer** → removes Angular decorators and unused code.  
- **vendorChunk: false** → merges vendor libraries into main bundle to reduce requests.  
- **outputHashing: all** → cache-busting for assets.  
- **sourceMap: false** → don’t ship source maps in production.  

### Angular Practices
- **Standalone components** → reduce NgModule overhead.  
- **Lazy loading** → keep feature modules out of the initial bundle.  
- **Preload strategy** → preload critical lazy modules.  
- **Remove unused polyfills** → modern browsers don’t need ES5 support.  
- **Audit scripts** → check `angular.json` for unnecessary global scripts.  

---

## 🧹 Method 2: CSS Optimization (Styles.css is your biggest chunk)

Your `styles.css` is ~494 kB, which is unusually large. Here’s how to trim it:

### PurgeCSS (works with Angular CLI)
- Install:
  ```bash
  npm install @fullhuman/postcss-purgecss --save-dev
  ```
- Add to `postcss.config.js`:
  ```js
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
- This strips unused CSS selectors from your final build.

### Angular-Specific CSS Practices
- Scope styles to components instead of global `styles.css`.  
- Use utility-first frameworks like Tailwind with purge enabled.  
- Avoid importing entire CSS libraries (e.g., Bootstrap full bundle). Import only what you need.  
- Minify CSS automatically with Angular’s production build.

---

## 📊 Applying to Your Build Output

- **styles.css (494 kB)** → Apply PurgeCSS or Tailwind purge to strip unused styles.  
- **main.js (304 kB)** → Enable `buildOptimizer` and audit libraries.  
- **scripts.js (107 kB)** → Remove or lazy load non-critical scripts.  
- **polyfills.js (237 B)** → Already minimal.  
- **Lazy chunks (~5 kB)** → Excellent, keep lazy loading pattern.  