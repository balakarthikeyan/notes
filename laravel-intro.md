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

## Basic Concepts

- Routing
- Models, controllers and views
- Blade templating
- Requests and Responses
- Database migrations
- Middlewares
- Eloquent ORM

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

### Blade templating
Laravel makes use a templating language called Blade which is powerful and easy to use. They support modern features like inheritance which enables the reuse of templates.You can create templates in the resources/views folder of your project using the blade.php extension.

    Blade is the simple, yet powerful templating engine provided with Laravel. Unlike other popular PHP templating engines, Blade does not restrict you from using plain PHP code in your views. 
    In fact, all Blade views are compiled into plain PHP code and cached until they are modified, meaning Blade adds essentially zero overhead to your application. Blade view files use the .blade.php file extension and are typically stored in the resources/views directory.

### Request and Responses
Traditionnal frameworks like Laravel works based on HTTP request and response cycles. Requests are sent from clients using HTTP verbs like GET, POST, DELETE and UPDATE. When they are processed by Laravel, appropriate responses are created and sent back to clients. A response is created and sent from the controller mapped to the route which received the request.

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
composer global require "laravel/installer"
laravel new mylaravel
(or)
composer create-project --prefer-dist laravel/laravel mylaravel

php artisan serve
php artisan serve --port=8000 --host=localhost

php artisan make:controller NameController // Parameters: --resource, --api, --invokable, --model=NameModel
php artisan make:controller NameController --resource --model=NameModel

// Creates: Model, Factory, Table and Controller
php artisan make:model NameModel --factory --migration --resource /-fmr 
php artisan make:model NameModel --migration --controller --resource / -mcr

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

php artisan view:clear
php artisan cache:clear

php artisan make:request NameControllerRequest
php artisan make:observer NameObserver --model=NameModel

php artisan make:exception CustomException --render --report

php artisan make:test NameControllerTest --unit

php artisan make:resource NameModelResource --collection=NameModel

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

mkdir resources/views/layouts
mkdir resources/views/pages
mkdir resources/views/includes
code resources/views/layouts/default.blade.php

composer require laravel/ui --dev
php artisan ui bootstrap --auth
npm install && npm run dev 

composer require captcha-com/laravel-captcha:"4.*"
php artisan vendor:publish

composer require laravel/socialite
php artisan vendor:publish

php artisan vendor:publish --tag=laravel-pagination

composer require laravel/passport
php artisan migrate
php artisan passport:install
composer require doctrine/dbal

composer require laravel/telescope --dev
php artisan telescope:install
php artisan migrate
```
