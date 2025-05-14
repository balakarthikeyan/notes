## Install Global Angular For Project
```bash
npm install -g @angular/cli@16.1.6 / npm install -g @angular/cli@17.3.8
ng v / ng version
ng serve --open --configuration=<env/lang> // development/production

ng new project_name
ng generate interface interface_name
ng generate component componet_name
ng generate service service_name
ng generate module views/dashboard --route dashboard --module app.module

ng add @angular/material
npm install -s @angular/cdk @angular/flex-layout
npm install @ngrx/store@16.2 @ngrx/effects@16.2
ng add @ngrx/store-devtools@16.2
npm install faker --save
ng generate @angular/material:navigation navigation
ng generate @angular/material:table table
```

## Install Old project
```bash
npm install
npm audit fix --force
```

## List all node_modules found in a Directory
> FOR /d /r . %d in (node_modules) DO @IF EXIST "%d" echo %d"

## Why to update:
- *Enhanced Performance:* Faster load times and better runtime performance.
- *New Features:* Introduction of new components and APIs.
- *Improved Developer Experience:* Enhanced debugging tools and error messages.
- *Security Updates:* Patches for known vulnerabilities and improved security practices.
- *Better Compatibility:* Improved support for modern web standards and third-party libraries.

## Uninstall Globally
```bash
npm uninstall -g angular-cli
npm uninstall -g @angular/cli
npm cache clean --force
npm cache verify --force
```

## Uninstall Locally
> npm uninstall @angular/cli@16.1.6 --save-dev

## Step-by-Step Upgrade Guide 
> npm install -g @angular/cli@latest

```bash
ng update @angular/cli@latest @angular/core@latest --force
ng update @angular/material@latest
npm install @angular-devkit/build-angular@latest --force
ng update @angular-eslint/schematics@latest --allow-dirty --force
```

## Handle Third-Party Dependencies:
```bash
npm outdated
npm update
```

## How to generate a component using cli command without creating its spec file
- `ng config schematics.@schematics/angular:component.skipTests true`
- `ng new breakpointobserver-example-angular --routing --prefix corp --style css --skip-tests`

This command will initialize a base project using some configuration options:

- `--routing.` It will create a routing module.
- `--prefix corp.` It defines a prefix to be applied to the selectors for created components(corp in this case). The default value is app.
- `--style scss.` The file extension for the styling files.
- `--skip-tests.` it avoids the generations of the .spec.ts files, which are used for testing

## Update TypeScript Version
```bash
npm install typescript@<required_version>
npm install typescript@">=3.4.0 and <3.5.0" --save-dev
```

## Getting ESLint Set Up in Your Angular Project
```bash
ng lint
ng add @angular-eslint/schematics@17.5.2
ng update @angular-eslint/schematics@latest
```

## Getting Spinner
```bash
npm install ngx-spinner --save
ng add ngx-spinner
ng update ngx-spinner@latest --allow-dirty --force
```

## Getting Bootstrap
```bash
npm install bootstrap@5.3
npm install bootstrap bootstrap-icons
ng add @ng-bootstrap/ng-bootstrap
ng update @ng-bootstrap/ng-bootstrap@latest --allow-dirty --force
```

## Getting Localisation
```bash
npm install @angular/localize --save
ng add @angular/localize
ng extract-i18n --output-path src/locale --i18n-format xlf
ng build --localize
```

## Getting Translation
> npm install @ngx-translate/core @ngx-translate/http-loader

## Run build with http server
> npx http-server dist/project_name

## Some common errors you may encounter
```
mat.define-palette to mat.m2-define-palette
mat.define-typography-config to mat.m2-define-typography-config
mat.define-light-theme to mat.m2-define-light-theme
mat.define-dark-theme to mat.m2-define-dark-theme
mat.red-palette to mat.$m2-red-palette
```

- Updates two-way bindings that have an invalid expression to use the longform expression instead.
- Replace deprecated HTTP related modules with provider functions.
- Updates calls to `afterRender` with an explicit phase to the new API.

## Step by Step Upgrade for older project
```bash
nvm use 18
ng update @angular/core@14 @angular/cli@14 --force
ng update @angular/core@15 @angular/cli@15 --force
ng update @angular/core@16 @angular/cli@16 --force
ng update @angular/core@17 @angular/cli@17 --force
nvm use 20
ng update @angular/core@latest @angular/cli@latest --force
```

## Dockerize angular
```bash
ng new angular-docker --ssr false --routing true --style scss
npm start
docker build -t my_angular_app:latest .
docker run -d -p 4201:4200 my_angular_app:latest
docker run -d -it -p 4200:4200/tcp --name my_angular_app my_angular_app:latest
```

## Tailwind CSS
```bash
npm install tailwindcss
npx tailwindcss init
npx tailwindcss -i ./src/style.css -o ./src/stylesheet.css --watch
```

## Prettier
```bash
npm install --save-dev --save-exact prettier
node --eval "fs.writeFileSync('.prettierrc','{}\n')"
npx prettier . --write
```

## Surge for deploying angular
Surge is an npm package used for deploying static websites for free. 
```bash
$ npm install --global surge
$ surge
$ npm run build
$ surge token
```
Set two Environment Variables in GitLab in: Repository > Settings > CI/CD > Variables
```
SURGE_LOGIN = <Email used on surge>
SURGE_TOKEN = <Surge token saved in the previous step>
```

```bash
git add .
git commit -m "added gitlab.yml"
git push origin main

# Running as balakarthikeyan07@gmail.com (Student)
# project: project_name
# domain: insidious-stream.surge.sh 
# token: 7a76ce0d677762cb2bd449c05fdb6a65
```

## Generate AppRoutingModule:
`ng generate module app-routing --flat --module=app`
- `--flat` puts the file in src/app instead of its own folder.
- `--module=app` tells the CLI to register it in the imports array of the AppModule.