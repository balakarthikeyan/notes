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
The service container is one of the core components of Laravel. Service containers manage class dependencies and dependency injection.

- **Service Providers**
Classes and dependencies are injected into the service containers.

- **Facades & Packages**
A facade is a static interface for classes bound in the service container.

- **Command-Line Interfaces**
Laravel includes a set of command-line interfaces (CLIs). The Artisan Console includes commands that help developers quickly build skeleton code, simplify and automate repetitive tasks, and more easily complete an application build

- **Eloquent**
Eloquent is an object-relational mapper (ORM) that allows easy interaction with databases. 

- **Composer**
Composer is a third-party application-level PHP dependency management tool

- **Homestead**
Laravel Homestead is a development environment allowing development on a virtual machine by providing a pre-packaged Vagrant box. 

**Authentication Starter Kits**
- `Laravel Breeze` is an authentication starter kit. It includes common authentication and user account features such as user registration, login, email verification, and password confirmation and reset.
- `Jetstream`, first introduced in Version 8. Jetstream also offers additional advanced features such as two-factor authentication, session management, API support via `Laravel Sanctum`, and optional team management.
- `Fortify` is the engine for the `Jetstream` authentication starter kit and includes all of the Laravel authentication features.
- Laravel also offers social media-based authentication (OAuth) through `Laravel Socialite`.
- `Forge` allows for deployment through a variety of infrastructure providers with minimal configuration efforts. 
- `Vapor` is a serverless deployment platform based on AWS.

**Packages**
- Authentication i.e. `Laravel Breeze/Fortify`
- Billing/Subscription i.e. `Laravel Cashier`
- Browser Automation and Testing i.e. `Laravel Dusk`
- Remote Task Manager i.e. `Laravel Envoy`
- Local Development i.e. `Laravel Sail/Homestead`

## The New Features of Laravel

### Laravel 6:

- The support of Semantic Versioning,
- Compatibility with Vapor, a serverless deployment platform for Laravel,
- Improved Authorization Responses,
- **Job Middleware:** A new feature that allows you wrap custom logic around the execution of queued jobs,
- **Lazy Collections:** A new feature that leverages PHP's generators to enable you to efficently work with very large datasets,
- Eloquent Subquery Enhancements,
- **Laravel UI:** UI scaffolding logic such as Bootstrap or Vue, is extracted in its own laravel/ui package.
- **Ignition:** A new and smart error page.

### Laravel 7:

- **Laravel Airlock:** An official package for API authentication,
- **Custom Eloquent Casts:** They allow you add your won custom casts,
- CORS support by default i.e without third-party plugins,
- **Blade Component Tags & Improvements:** Allows you to create class-less components,
- **HTTP Client:** An API for making HTTP requests,
- Route Caching Speed Improvements, etc.

### Laravel 9:

Laravel 9 installation requires the most up-to-date form of PHP 8, PHPUnit 9, has Symfony 6

- Anonymous Stub Migration
```
php artisan make:migration

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
return new class extends Migration {
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('people', function (Blueprint $table)
        {
            $table->string('first_name')->nullable();
        });
    }
};
```
- New Query Builder Interface
```
Illuminate\Contracts\Database\QueryBuilder interface
Illuminate\Database\Eloquent\Concerns\DecoratesQueryBuilder trait

return Model::query()
	->whereNotExists(function($query) {
		// $query is a Query\Builder
	})
	->whereHas('relation', function($query) {
		// $query is an Eloquent\Builder
	})
	->with('relation', function($query) {
		// $query is an Eloquent\Relation
	});
```
- PHP 8 String Functions
```
\Illuminate\Support\Str class

functions include the use of str_contains(), str_starts_with(), and str_ends_with() 
```

#### Migration of Laravel 8 to 9
- **Flysystem 2.0** Laravel 9.x has migrated from Flysystem 1.x to 2.x.
- **Symfony Mailer**  SwiftMailer has stopped the support.
- **Custom Casts & null** In Laravel 9.x, the set strategy of the cast course will be invoked with null as the given $value argument
- **Default HTTP Client Timeout** The HTTP client now includes a default timeout of 30 seconds.
- **The lang Directory** the resources/lang directory as of now is located within the root project directory.
- **The Password Rule** which validates that the given input esteem matches the confirmed user’s current password

### Laravel 10:

Laravel 10 installation requires the most up-to-date form of PHP 8.1

- Artisan Command Becomes More Interactive
- Invokable Validation Rules by Default
- Uses Native Types Instead of Docblocks
- Dropped Support for Predis v1

#### Migration of Laravel 9 to 10
- Remove various deprecations
- Remove deprecated dates property
- Remove handleDeprecation method
- Remove deprecated assertTimesSent method
- Remove deprecated ScheduleListCommand’s $defaultName property
- Remove deprecated Route::home method
- Remove deprecated dispatchNow functionality

### Laravel 11:
Laravel 11 introduce streamlined application structure, per-second rate limiting, health routing etc.
Laravel middleware is a mechanism that filters HTTP requests entering your application. It sits between the request and the application's core, allowing you to intercept, modify, or reject requests based on certain conditions. Middleware can be used for tasks like authentication, logging, and request manipulation. It provides a flexible way to handle cross-cutting concerns in your application's HTTP lifecycle, ensuring clean and modular code organization.

### Laravel 6/7 Routing
Routing is an important feature in any web application and allows your app to route HTTP requests to the appropriate handler which sends an appropriate response be it an HTML view or a JSON response.
You can create routes in the routes folder of your project in one of the avaiabe files:
- **api.php:** API Routes
- **channels.php:** Broadcast Channels
- **console.php:** Console Routes
- **web.php:** Web routes

### Laravel 6/7 Controllers
A Laravel controller reresents the C part in the MVC architecture which is reponsible for orchistering the oerations between the View and Model and passing information from the model to the view for rendering it.

### Laravel 6/7 Views
A Laravel view represents the V part of the MVC architecture. They are simply HTML pages (with CSS and JavaScript) composing the UI of the application that are sent to the client once they are processed and rendered. Views are merely presentationel i.e they don't contain business logic .

### Laravel 6/7 Models and Migrations
In Laravel, the Model from the MVC pattern represents the part that contains the business/domain logic. It corresponds to a table in the database and allow you to interact with the database with high level APIs.

Migrations allow you to create the intial database tables and their fields and then track any changes made to the database schema during the development or even in production and apply them without the need of dropping and recreating the database each time.

### Laravel Middlewares
Middlewares are pieces of code that are executed before some specific HTTP requests before running the correspong controllers. Laravel includes several builtin middlewares for authentication and CSRF protection but you can also create your custom middlewares when needed.

### Laravel Eloquent ORM
Laravel makes use of an ORM which stands for Object Relationnal Mapper and implements the PHP Active Record Pattern. This allows you to work with databases without actually dealing with SQL and the differences between database systems. The Laravel ORM is called Eloquent ORM.

## Artisan Commands
```
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
```
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

// Creates: Model, Factory, Table and Controller
php artisan make:model NameModel --factory --migration --controller --resource / -fmcr

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
```
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
```
composer require laravel/ui --dev
php artisan ui bootstrap --auth
npm install && npm run dev 
```

## Enable Capctcha
```
composer require captcha-com/laravel-captcha:"4.*"
php artisan vendor:publish
```

## Enable Social
```
composer require laravel/socialite
php artisan vendor:publish
```

## Enable pagination
```
php artisan vendor:publish --tag=laravel-pagination
```

## Enable Oauth
```
composer config --global --auth github-oauth.github.com <token>
composer update kylekatarnls/update-helper
composer update --prefer-source
composer dump-autoload
```

## Re-Run or Migrate
```
php artisan key:generate
php artisan config:cache
php artisan route:cache
php artisan migrate or php artisan migrate --force
php artisan db:seed or php artisan migrate:refresh --seed
php artisan optimize:clear
php artisan serve
```

## Run Laravel 11
```
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
``
npm install -D tailwindcss postcss autoprefixer
npm install -D @tailwindcss/forms
``

Run the following command to generate the tailwind.config.js and postcss.config.js files: `npx tailwindcss init -p`

### Enable Query Log
```
use Illuminate\Support\Facades\DB;
use Illuminate\Support\Facades\Log;
DB::enableQueryLog(); Log::debug(DB::getQueryLog());
```

### Enable Fortify
```
composer clear-cache
composer require laravel/fortify
php artisan fortify:install
php artisan vendor:publish --provider="Laravel\Fortify\FortifyServiceProvider"
php artisan migrate
```

### Enable Passport
```
composer require doctrine/dbal
composer require laravel/passport
php artisan migrate
php artisan passport:install
```

### To create passport password
```
php artisan passport:keys
php artisan passport:client --password
```

### Enable Jetstream
Jetstream provides the implementation for your application's login, registration, email verification, two-factor authentication, session management, API via Laravel Sanctum, and optional team management features.

```
composer require laravel/jetstream
php artisan jetstream:install livewire
php artisan migrate
php artisan make:livewire posts
php artisan livewire:publish --config
```

### Enable Telescope
```
composer require laravel/telescope --dev
php artisan telescope:install
php artisan migrate
localhost:8000/telescope/requests
```

### Enable Maintaince
```
php artisan down --render="errors::503"
```

### Enable JWT
```
composer require php-open-source-saver/jwt-auth
php artisan vendor:publish --provider="PHPOpenSourceSaver\JWTAuth\Providers\LaravelServiceProvider"
php artisan jwt:secret

```

### Enable Swagger
```
composer require "darkaonline/l5-swagger"
php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"
npm install swagger-ui-dist
php artisan l5-swagger:generate
http://localhost:8000/api/documentation

```