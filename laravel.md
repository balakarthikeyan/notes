##What is Laravel?
Laravel is web development framework for building web applications with PHP. It's based on modern and powerful design patterns like MVC, dependency injection and ORMs.

##Laravel Requirements

Composer,
MySQL,
PHP >= 7.2.0 installed on your system with the following extensions:
- PHP >= 7.2.0
- BCMath PHP Extension
- Ctype PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension

##The New Features of Laravel 6 & 7

####Laravel 6:

    The support of Semantic Versioning,
    Compatibility with Vapor, a serverless deployment platform for Laravel,
    Improved Authorization Responses,
    Job Middleware: A new feature that allows you wrap custom logic around the execution of queued jobs,
    Lazy Collections: A new feature that leverages PHP's generators to enable you to efficently work with very large datasets,
    Eloquent Subquery Enhancements,
    Laravel UI: UI scaffolding logic such as Bootstrap or Vue, is extracted in its own laravel/ui package.
    Ignition: A new and smart error page.
    Check out the docs for more details.

####Laravel 7:

    Laravel Airlock: An official package for API authentication,
    Custom Eloquent Casts: They allow you add your won custom casts,
    CORS support by default i.e without third-party plugins,
    Blade Component Tags & Improvements: Allows you to create class-less components,
    HTTP Client: An API for making HTTP requests,
    Route Caching Speed Improvements, etc.

##Basic Concepts

    Routing
    Models, controllers and views
    Blade templating
    Requests and responses
    Database migrations
    Middlewares
    Eloquent ORM

composer global require "laravel/lumen-installer"
lumen new mylumen

composer global require "laravel/installer"
laravel new mylaravel
(or)
composer create-project --prefer-dist laravel/laravel mylaravel
composer require laravel/ui --dev
php artisan ui bootstrap --auth
npm install && npm run dev 

php artisan serve
php artisan serve --port=8000 --host=localhost

php artisan make:migration create_tablename_table --create=tablename
php artisan make:controller NameController
php artisan make:controller NameController --resource --model=tablename
php artisan migrate 
php artisan migrate --force
php artisan migrate:install
php artisan migrate:rollback
php artisan db:seed  
php artisan db:seed --class=NameSeeder
php artisan make:component NameComponent
php artisan make:model NameModel -m
php artisan make:seeder NameSeeder
php artisan make:middleware TestMiddleware
php artisan down 
php artisan up 

php artisan tinker
factory(App\User::class,50)->create()
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
