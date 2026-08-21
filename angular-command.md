## 1. CLI Installation & Cache Management

### Global Installation

* **Global Installation (Specific Versions):**
```bash
npm install -g @angular/cli@16.1.6
npm install -g @angular/cli@17.3.8
```

* **Modern Version Execution (Latest):**
```bash
npx -p @angular/cli@latest ng new my-angular-22
```

### Version Verification

```bash
ng version  # Modern command
ng v        # Legacy alias
```

### Uninstallation & Cache Reset

* **Global Cleanup:**
```bash
npm uninstall -g angular-cli
npm uninstall -g @angular/cli
npm cache clean --force
npm cache verify --force
```

* **Local Project Cleanup:**
```bash
npm uninstall @angular/cli@16.1.6 --save-dev
```

---

## 2. Project Creation & CLI Generators

### Basic Project Execution

```bash
# Create base project
ng new project_name

# Serve with custom configuration and auto-open browser
ng serve --open --configuration=<env/lang> # e.g., development/production

# Run local development server on a custom port
npm start -- --port 4100
```

### Component & Artifact Generation Options

```bash
# General schematic generators
ng generate interface interface_name
ng generate component component_name
ng generate service service_name
ng generate module views/dashboard --route dashboard --module app.module

# Generate AppRoutingModule for NgModule-based setups
ng generate module app-routing --flat --module=app
```

* **`--flat`:** Places the generated file directly inside `src/app` instead of creating its own folder.
* **`--module=app`:** Registers the routing module inside the `imports` array of `AppModule`.

### Skip Test Specification Files (.spec.ts)

* **Global CLI Configuration:**
```bash
# Set schematics globally to skip generating unit test (.spec.ts) files
ng config schematics.@schematics/angular:component.skipTests true
```

* **Project Initialization Flag:**
```bash
# Initialize project with specific configuration flags
ng new breakpointobserver-example-angular --routing --prefix corp --style scss --skip-tests
```

* **`--routing`:** Generates a dedicated routing module.
* **`--prefix corp`:** Sets a custom component selector prefixes (e.g., `<corp-root>` instead of `<app-root>`). (defaults to `app`)
* **`--style scss`:** Configures SCSS as the primary styling preprocessor.
* **`--skip-tests`:** Omits `.spec.ts` testing file generation.

### 3. Angular Code Patterns: Legacy (NgModules / Pre-v17) vs. Modern (Standalone / v17+)

**Component Generation:**
* **Legacy (NgModule-based):** `ng generate component component_name`
* **Modern (v17+ Standalone):** `ng generate component component_name --standalone`

**Services & Interfaces:**
* **Legacy & Modern:** `ng generate service service_name` and `ng generate interface interface_name`

**Lazy-Loaded Route Generation:**
* **Legacy:** `ng generate module views/dashboard --route dashboard --module app.module`
* **Modern:** Defined using `loadComponent: () => import(...)` directly in `app.routes.ts`.

**Routing Setup:**
* **Legacy:** `ng generate module app-routing --flat --module=app`
* **Modern:** `provideRouter(routes)` configured in `app.config.ts`.

**HTTP Client Setup:**
* **Legacy:** `HttpClientModule` imported inside `AppModule`.
* **Modern:** `provideHttpClient()` function passed in `app.config.ts`.

---

## 4. UI Frameworks, Libraries & Utilities Setup

### Angular Material & CDK

```bash
ng add @angular/material
npm install -s @angular/cdk @angular/flex-layout

# Generate Material Component Schematics
ng generate @angular/material:navigation navigation
ng generate @angular/material:table table
```

### State Management (NgRx) & Mocking

```bash
npm install @ngrx/store@16.2 @ngrx/effects@16.2
ng add @ngrx/store-devtools@16.2
npm install faker --save
```

### UI Libraries & Styling

* **Ngx-Spinner:**
```bash
npm install ngx-spinner --save
ng add ngx-spinner
ng update ngx-spinner@latest --allow-dirty --force
```

* **Bootstrap:**
```bash
npm install bootstrap@5.3
npm install bootstrap bootstrap-icons
ng add @ng-bootstrap/ng-bootstrap
ng update @ng-bootstrap/ng-bootstrap@latest --allow-dirty --force
```

* **Tailwind CSS:**
```bash
npm install tailwindcss
npx tailwindcss init
npx tailwindcss -i ./src/style.css -o ./src/stylesheet.css --watch
```

### Code Formatting (Prettier)

```bash
npm install --save-dev --save-exact prettier
node --eval "fs.writeFileSync('.prettierrc','{}\n')"
npx prettier . --write
```

### Localization (i18n) & Translations

* **Angular Native Localize:**
```bash
npm install @angular/localize --save
ng add @angular/localize
ng extract-i18n --output-path src/locale --i18n-format xlf
ng build --localize
```

* **Ngx-Translate:**
```bash
npm install @ngx-translate/core @ngx-translate/http-loader
```

---

## 5. Upgrade Strategy, Migrations & Refactoring

### Reasons to Update

* **Enhanced Performance:** Faster load times and improved runtime efficiency.
* **New Features:** Access to modern components, APIs, and Signal-based architectures.
* **Improved Developer Experience:** Enhanced error diagnostics and debugging capabilities.
* **Security Updates:** Vulnerability patches and updated dependency security practices.
* **Better Compatibility:** Improved alignment with modern web standards and third-party packages.

### Installing & Auditing Existing/Old Projects

For legacy projects spanning multiple major versions, perform sequential major upgrades:

```bash
# Step 1: Switch Node version to 18 and step through Angular v14 to v17
nvm use 18
ng update @angular/core@14 @angular/cli@14 --force
ng update @angular/core@15 @angular/cli@15 --force
ng update @angular/core@16 @angular/cli@16 --force
ng update @angular/core@17 @angular/cli@17 --force

# Step 2: Switch Node version to 20+ for latest Angular releases
nvm use 20
ng update @angular/core@latest @angular/cli@latest --force
```

### Direct Upgrades & Dependency Management

```bash
# Direct Core & Material Update
npm install -g @angular/cli@latest
ng update @angular/cli@latest @angular/core@latest --force
ng update @angular/material@latest
npm install @angular-devkit/build-angular@latest --force

# ESLint Configuration
ng lint
ng add @angular-eslint/schematics@17.5.2
ng update @angular-eslint/schematics@latest --allow-dirty --force

# TypeScript Version Updates
npm install typescript@<required_version>
npm install typescript@">=3.4.0 and <3.5.0" --save-dev

# Install & Audit Legacy Projects
npm install
npm audit fix --force

# Checking Third-Party Dependencies / Outdated Dependency Auditing
npm outdated
npm update
```

### 6. Syntax Refactoring & Common Breaking Changes

**Angular Material Sass Mixin Updates: (M2 Namespace Changes)**
* Replace `mat.define-palette` with `mat.m2-define-palette`
* Replace `mat.define-typography-config` with `mat.m2-define-typography-config`
* Replace `mat.define-light-theme` with `mat.m2-define-light-theme`
* Replace `mat.define-dark-theme` with `mat.m2-define-dark-theme`
* Replace `mat.red-palette` with `mat.$m2-red-palette`

**Syntax Refactoring Checklist:**
* **Two-Way Binding Syntax:** Updates invalid two-way bindings to use explicit longform expressions.
    * (e.g., Converting `[(value)]="expr"` with invalid target expressions into explicit `[value]` and `(valueChange)` event bindings)
* **HTTP Providers:** Replaces deprecated `HttpClientModule` setups with standalone provider functions like `provideHttpClient()`.
* **Lifecycle Hooks:** Refactors `afterRender` lifecycle calls by providing explicit execution phases per updated rendering APIs.

---

## 5. Directory Utilities, Docker & Deployment

### Directory Inspection Utility (Windows CMD)

Find all `node_modules` folders recursively:

```cmd
FOR /d /r . %d in (node_modules) DO @IF EXIST "%d" echo %d
```

### Local Build Testing

```bash
npx http-server dist/project_name
```

### Docker Containerization

```bash
# Generate project without SSR for docker container
ng new angular-docker --ssr false --routing true --style scss
npm start

# Build and execute Docker container
docker build -t my_angular_app:latest .
docker run -d -p 4201:4200 my_angular_app:latest
docker run -d -it -p 4200:4200/tcp --name my_angular_app my_angular_app:latest
```

### Static Deployment via Surge & GitLab CI/CD

Surge is an npm package used to deploy static websites for free.

```bash
# Global installation and build
npm install --global surge
npm run build
surge
surge token
```

**GitLab CI/CD Environment Variable Setup:**

Navigate to **Repository > Settings > CI/CD > Variables** and register:

* `SURGE_LOGIN`: Email account registered on Surge.
* `SURGE_TOKEN`: Access token obtained from running `surge token`.
```bash
# Git Commit and Deployment Push
git add .
git commit -m "added gitlab.yml"
git push origin main

# Surge Deployment Details Example:
# Running as balakarthikeyan07@gmail.com (Student)
# project: project_name
# domain: insidious-stream.surge.sh
# token: 7a76ce0d677762cb2bd449c05fdb6a65
```