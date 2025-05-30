## What is Laravel?
Laravel is web development framework for building web applications with PHP. It's based on modern and powerful design patterns like MVC, dependency injection and ORMs.

## Laravel Requirements

- Composer,
- MySQL,
- PHP >= 7.2.0 

### PHP should have the following extensions:
- BCMath PHP Extension
- Ctype PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension

### Laravel's significant features include:
- Route Handling
- Security
- Migration
- Templating
- Sessions
- Data Validation
- Cache Handling
- Error Handling
- Testing
- Storage and File Management
- Email
- Notifications

## Basic Concepts

- Routing
- Models, controllers and views
- Blade Template Engine
- Requests and Responses
- Database Migrations
- Middlewares
- Eloquent ORM
- Artisan CLI
- MVC architecture
- Securtiy
- Built in Multi-language support
- Background Jobs/Queues
- Unit Testing
- Dependecy Injection
- Real time events
- Scalability

## Important structures and functionalities in Laravel

- **Service Containers**
Service containers manage class dependencies and dependency injection.

- **Service Providers**
Classes and dependencies are injected into the service containers.

- **Facades & Packages**
A facade is a static interface for classes bound in the service container.

- **Command-Line Interfaces** 
The Artisan Console includes commands that help developers quickly build skeleton code, simplify and automate repetitive tasks, and more easily complete an application build

- **Eloquent**
Eloquent is an object-relational mapper (ORM) that allows easy interaction with databases. 

- **Composer**
Composer is a third-party application-level PHP dependency management tool

### Packages
- `Laravel Breeze:` A simple, minimal implementation of all common authentication and user account features such as login, registration, password reset, email verification and password confirmation.
- `Laravel Fortify:` A headless authentication backend that includes the above authentication features along with two-factor authentication. It is the engine for the `Laravel Jetstream` authentication starter kit.
- `Laravel Jetstream:` An application starter kit that provides a UI on top of `Laravel Fortify` authentication features. It offers additional advanced features such as two-factor authentication, session management, API support via `Laravel Sanctum`, and optional team management.
- `Laravel Passport:` An OAuth2 authentication provider.
- `Laravel Sanctum:` An API token authentication provider.
- `Laravel Telescope:` It is a powerful debugging assistant for Laravel applications. It provides insights into the requests coming into your application, exceptions, database queries, and much more.
- `Laravel Octane:` supercharges your application's performance by serving requests using high-performance application servers like Swoole and RoadRunner.
- `Laravel Sail:` Local Development
- `Laravel Homestead:` It is a development environment, allowing development on a virtual machine by providing a pre-packaged Vagrant box. 
- `Laravel Envoy:` Remote Task Manager
- `Laravel Dusk:` Browser Automation and Testing
- `Laravel Cashier:` Billing/Subscription
- `Laravel Scout:` 
- `Laravel Horizon:` 
- `Laravel Socialite` offers social media-based authentication (OAuth).
- `Laravel Forge` allows for deployment through a variety of infrastructure providers with minimal configuration efforts. 
- `Laravel Vapor` is a serverless deployment platform based on AWS.

## Artisan Commands
```bash
  make:channel         Create a new channel class
  make:command         Create a new Artisan command
  make:controller      Create a new controller class
  make:event           Create a new event class
  make:exception       Create a new custom exception class
  make:factory         Create a new model factory
  make:job             Create a new job class
  make:listener        Create a new event listener class
  make:mail            Create a new email class
  make:middleware      Create a new middleware class
  make:migration       Create a new migration file
  make:model           Create a new Eloquent model class
  make:notification    Create a new notification class
  make:observer        Create a new observer class
  make:policy          Create a new policy class
  make:provider        Create a new service provider class
  make:request         Create a new form request class
  make:resource        Create a new resource
  make:rule            Create a new validation rule
  make:seeder          Create a new seeder class
  make:test            Create a new test class
```

## My working Commands
```bash
composer global require "laravel/lumen-installer"
lumen new mylumen
composer global require "laravel/installer"
laravel new mylaravel
(or)
composer create-project laravel/laravel mylaravel --prefer-dist 
composer create-project laravel/laravel mylaravel "11.*" --prefer-dist

php artisan serve
php artisan serve --port=8000 --host=localhost

php artisan make:controller NameController // Parameters: --resource, --api, --invokable, --model=NameModel

## Creates: Model, Factory, Table and Controller
php artisan make:model NameModel --factory --migration --controller --resource 
# or
php artisan make:model NameModel -fmcr

php artisan make:migration create_tablename_table --create=tablename
php artisan make:migration add_to_tablename_table --table=tablename

php artisan make:factory NameFactory --model=NameModel

php artisan migrate 
php artisan migrate --force
php artisan migrate:install
php artisan migrate:rollback
php artisan migrate:refresh

php artisan db:seed  
php artisan db:seed --class=NameSeeder
php artisan make:seeder NameSeeder

php artisan make:component NameComponent
php artisan make:middleware NameMiddleware

php artisan down 
php artisan up 

php artisan make:mail NameMail
php artisan make:mail NameEmail --markdown=view_folder.view_filename

php artisan make:event NameEvent
php artisan make:listener NameListener --event="NameEvent" --queued

php artisan make:command NewCommandName --command=Command
php artisan command:name

php artisan notifications:table
php artisan make:notification NameNotification --markdown

php artisan schedule:run

php artisan queue:table
php artisan make:job JobName --sync
php artisan queue:work

php artisan make:provider NameServiceProvider

php artisan make:policy NewPolicy --model=ModelName

php artisan make:rule NewRule
php artisan storage:link

php artisan cache:clear
php artisan route:clear
php artisan config:clear 
php artisan view:clear

php artisan make:request NameControllerRequest
php artisan make:observer NameObserver --model=NameModel

php artisan make:exception CustomException --render --report

php artisan make:test NameControllerTest --unit

php artisan make:resource NameModelResource --collection=NameModel

```

## Tinker
```bash
php artisan tinker
factory(App\User::class,5)->create()
exit

php artisan tinker
$user = new App\User
$user->name = 'Balakarthikeyan'
$user->email = 'balakarthikeyan@mail.com'
$user->password = bcrypt('password')
$user->save()
exit
```

## Enable UI
```bash
composer require laravel/ui --dev
php artisan ui bootstrap --auth
npm install && npm run dev 
```

## Enable Capctcha
```bash
composer require captcha-com/laravel-captcha:"4.*"
php artisan vendor:publish
```

## Enable Social
```bash
composer require laravel/socialite
php artisan vendor:publish
```

## Enable pagination
```bash
php artisan vendor:publish --tag=laravel-pagination
```

## Enable Oauth
```bash
composer config --global --auth github-oauth.github.com <token>
composer update kylekatarnls/update-helper
composer update --prefer-source
composer dump-autoload
```

## Re-Run or Migrate
```bash
php artisan key:generate
php artisan config:cache
php artisan route:cache
php artisan migrate or php artisan migrate --force
php artisan db:seed or php artisan migrate:refresh --seed
php artisan optimize:clear
php artisan serve
```

## Run Laravel 11
```bash
php artisan key:generate
php artisan migrate
php artisan make:migration create_notes_table --create=notes
php artisan make:migration add_to_notes_table --table=notes
php artisan make:request NoteStoreRequest
php artisan make:request NoteUpdateRequest
php artisan make:controller NoteController --resource --api --invokable --model=Note
php artisan make:model Note -fmcr

php artisan lang:publish
php artisan lang:publish --existing
php artisan make:class Helpers/Helper
php artisan make:interface Interfaces/TestInterface
php artisan make:enum Enums/NoteStatus
php artisan make:trait Traits/Sluggable
php artisan make:command TestJob
php artisan make:exception InvalidProductException
php artisan make:factory UserFactory
php artisan make:seeder UserSeeder
php artisan db:seed UserSeeder
php artisan make:middleware IsAdmin
php artisan config:publish cors
php artisan make:class OnceTest
php artisan make:command ShowMethod --command=show:method
php artisan make:resource MyCollection
php artisan install:api
php artisan make:view layouts.app
php artisan make:view auth.register
php artisan make:view auth.login
php artisan make:view auth.home
php artisan make:scope ActiveScope
php artisan make:rule Uppercase
php artisan make:observer ProductObserver --model=Product

php artisan tinker
App\Models\Note::factory()->count(5)->create();
User::factory(30)->create();
```

### Tailwind CSS and its peer dependencies:
Tailwind CSS is an open-source CSS framework. The main feature of this library is that, unlike other CSS frameworks like Bootstrap, it does not provide a series of predefined classes for elements such as buttons or tables.
```bash
npm install -D tailwindcss postcss autoprefixer
npm install -D @tailwindcss/forms
```

Run the following command to generate the tailwind.config.js and postcss.config.js files: `npx tailwindcss init -p`

### Enable Query Log
```php
use Illuminate\Support\Facades\DB;
use Illuminate\Support\Facades\Log;
DB::enableQueryLog(); 
Log::debug(DB::getQueryLog());
```

### Enable Fortify
```bash
composer clear-cache
composer require laravel/fortify
php artisan fortify:install
php artisan vendor:publish --provider="Laravel\Fortify\FortifyServiceProvider"
php artisan migrate
```

### Enable Passport
```bash
composer require doctrine/dbal
composer require laravel/passport
php artisan migrate
php artisan passport:install
```

### To create passport password
```bash
php artisan passport:keys
php artisan passport:client --password
```

### Enable Jetstream
```bash
composer require laravel/jetstream
php artisan jetstream:install livewire
php artisan migrate
php artisan make:livewire posts
php artisan livewire:publish --config
```

## Installation Laravel Breeze
```bash
composer create-project laravel/laravel laravel-breeze-example
composer require laravel/breeze — dev
php artisan breeze:install blade
php artisan migrate
npm install
npm run dev
php artisan serve
```

### Enable Telescope
```bash
composer require laravel/telescope --dev
php artisan telescope:install
php artisan migrate
localhost:8000/telescope/requests
```

### Enable Maintaince
```bash
php artisan down --render="errors::503"
```

### Enable JWT
```bash
composer require php-open-source-saver/jwt-auth
php artisan vendor:publish --provider="PHPOpenSourceSaver\JWTAuth\Providers\LaravelServiceProvider"
php artisan jwt:secret
```

### Enable Swagger
```bash
composer require "darkaonline/l5-swagger"
php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"
npm install swagger-ui-dist
php artisan l5-swagger:generate
http://localhost:8000/api/documentation
```