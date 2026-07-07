# What is Angular 20? 

Angular 20 is the newest version of Google’s web framework that helps you build websites and apps. It’s officially released on 29th May, 2025 and has some big changes that make your apps run better and faster. 

- Control flow syntax (@if, @for) is stable
- Standalone components are now the default (finally!)
- zone.js is now optional, with better dev ergonomics
- Signals are getting real framework-level support
- SSR is faster, lighter, and less confusing
- Forms now support better type inference
- Angular CLI + dev server got faster and smarter
- Replaces TestBed.get() with TestBed.inject().
- Updates ngIf, ngFor, and ngSwitch to a new control flow syntax.

## Why did Angular drop Karma?
The default build package changes from `@angular-devkit/build-angular` to the new `@angular/build`. This new package no longer includes the Karma plugin used by legacy test setups. The web ecosystem has moved on to faster test runners like Vitest and Jest that use modern tools like Vite and esbuild.

## What the new world looks like (Vitest/Jest)
Angular's experimental test runner, now powered by Vitest, is the future. Migrating means your unit tests will run in a fast, modern Node.js-based environment. To reinstall the old compiler with Karma support:

```bash
npm install @angular-devkit/build-angular --save-dev
```

This command forces the CLI to fall back to the old compiler that still supports Karma.

## Prerequisites
- Node.js v20: Angular 20 no longer supports Node 18. Verify with `node -v`.
- TypeScript 5.8: Update by running `npm install typescript@5.8`.

- `Step 1.` Update Angular CLI

```bash
npm uninstall -g @angular/cli
npm install -g @angular/cli@20
```

- `Step 2.` Upgrade Project Dependencies

```bash
npm run ng update @angular/cli@20 @angular/core@20
# For the latest version
ng update @angular/cli @angular/core --next
# Update other packages
npm run ng update @angular/material 
npm run ng update @angular/forms @angular/router
npm run ng update @angular/common@20 @angular/material@20 @angular/animations@20 @angular/platform-browser@20
npm install @ng-bootstrap/ng-bootstrap@17.0.0 --legacy-peer-deps
```

- `Step 3.` Control Flow:

Migrating ngIf, ngFor, and ngSwitch to the new control flow syntax.

Example Migration: 
```html
<!-- Before -->   
<div *ngIf="user">{{ user.name }}</div>   
 
<!-- After -->   
@if (user) {   
  <div>{{ user.name }}</div>   
}
```

@for replaces *ngFor and is a major improvement.

```html
<!-- Old syntax -->  
<div *ngFor="let item of items; trackBy: trackItemById">{{ item.name }}</div>

<!-- New syntax -->  
@for (item of items; track item.id) {
  <div>{{ item.name }}</div>
} @empty {
  <div>No items to display.</div>
}
```
- `track` is mandatory and encourages best practices.
- `@empty` improves DX by removing the need for separate @if.

Better Templates 

String templates you can mix text with, power math symbols, checking if things exist, and empty operations. 

```html
<!-- Old way --> 
<p>Hello {{ name }} from {{ city }}</p> 
 
<!-- New way with template strings --> 
<p>{{ `Hello ${name} from ${city}` }}</p> 
```

- `Step 3.` TestBed.get() Removal 

Replacing TestBed.get() with TestBed.inject()

```ts
// Before
const service = TestBed.get(UserService);   

// After
const service = TestBed.inject(UserService);  
```

- `Step 4.` Forms API Updates 

New methods like markAllAsDirty() are available: 

```ts
this.userForm.markAllAsDirty();   
```

- `Step 5.` Enable Experimental Features (Optional) 

Zoneless is no longer experimental, but not yet stable. It is now in developer preview. Zoneless Change Detection, Add to app.config.ts: 

  - Faster to load 
  - Use less memory 
  - Have smaller file sizes 
  - Work better overall

```ts
import { provideZonelessChangeDetection } from '@angular/core';   
 
export const appConfig: ApplicationConfig = {   
  providers: [provideZonelessChangeDetection()]   
};  
```

Note: Manual change detection may be required for third-party libraries.

- `Step 6.` Signals:

Signals help your app know when things change and update the right parts automatically.

```ts
const name = signal('');   
const isValid = computed(() => name().length > 2);   
```

- `Step 7.` Update Browserslist Configuration 

Angular 20 targets browsers released in the last 30 months. Update `.browserslistrc:` 

Chrome >= 107   
Firefox >= 104   
Safari >= 16   

- `Step 8.` Test and Optimize 

  - `Run Tests:` Angular 20 deprecates Karma. Migrate to Web Test Runner or Vitest.
  - `Check Bundle Size:` Use `ng build --stats-json` to analyze with `webpack-bundle-analyzer`. 
  - `Verify SSR:` Test server-side rendering with `ng serve --ssr`. 

Post-Upgrade Checklist 
Update Angular Material/Material UI if used. 
Ensure third-party libraries (e.g., NgRx, RxJS) are compatible. 

Enable stricter TypeScript checks in tsconfig.json: 

```json
{   
  "compilerOptions": {   
    "strict": true   
  }   
}   
```

- `Step 9.` Automated migration and performance:

Use the CLI to automatically refactor templates to the new control flow syntax:

```bash
ng generate @angular/core:control-flow

# General Best Practices and Further Migrations

ng generate @angular/core:standalone
ng generate @angular/core:inject
ng generate @angular/core:route-lazy-loading
ng generate @angular/core:signal-input-migration
ng generate @angular/core:signal-queries-migration
ng generate @angular/core:output-migration
```

These commands allow for a comprehensive update of an Angular app to leverage the latest patterns.

- `Step 10.` Zoneless: Escaping the "Magic" of Change Detection

In a zone-less world, the UI only updates when you explicitly Signals.

```ts
mySignal.set(newValue);
```

This directly tells Angular to update only the DOM parts that use that signal. It's a surgical, predictable, and high-performance approach.

- `Step 11.` Error Checking for Component Events 

Angular 20 now checks your component event code for mistakes. When you write `@HostBinding` or `@HostListener` code, Angular will tell you if something looks wrong. This catches errors before your app runs, which saves you time debugging. 

- `Step 12.` Important Detail: browserslist and Browser Support

Angular 20 no longer supports Opera officially. If you list Opera in your browserslist, you may need to remove it.

- `Step 13.` Troubleshooting Common Issues 

  - `Node 18` and `TypeScript` versions below 5.8 has been dropped.
  - The old `*ngIf` and `*ngFor` ways still work but are deprecated. Start using the new `@if` and `@for` syntax instead. 
  - "Cannot find module" Errors: Delete `node_modules` and` package-lock.json`, then run `npm install`. 
  - `Zone.js` Warnings: Add `ngZone: 'noop'` to `provideZonelessChangeDetection()` if using zoneless mode.
  - Legacy Browser Support: Adjust `.browserslistrc` if targeting older browsers (may increase bundle size). 
  - `InjectFlags` way of doing things that was marked as outdated.

### Standalone by Default: A Fundamental Architectural Shift

By explicitly listing dependencies using the imports array at the component level, each component becomes self-contained. 
- Clarifies your architecture
- Improves tree-shaking
- Results in smaller bundles

### New Naming Convention:

Angular 20 introduces a new official naming convention that drops traditional suffixes.

Old naming

```bash
user-profile.component.ts;
auth.service.ts;
highlight.directive.ts;
```

New naming

```bash
user-profile.ts; // UI component
auth-store.ts; // state
highlight.ts; // directive
```

Focus on intent instead of type

```bash
user-api.ts; // HTTP requests
auth-store.ts; // reactive state
movie-card.ts; // UI component
movie-details.ts; // UI component
```

Feature-based folder structure

```bash
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

### After upgrading, test these things: 

- All pages load correctly 
- Forms work properly 
- User login/logout works 
- Data saves and loads 
- Mobile version works 
- Performance is good 

Run your automated tests too: 

```bash
ng test 
ng e2e
```