### Q1. What is Laravel?
Laravel is an open-source PHP web framework used for building robust web applications. It follows the MVC (Model-View-Controller) architectural pattern and provides expressive syntax for developers. It is created by Taylor Otwell.

### Q2. What are the features of Laravel?
Laravel features, including routing, middleware, authentication, Eloquent ORM, Blade templating engine, database migrations, and more. It also provides built-in support for caching, session management, and task scheduling.

### Q3. What is Composer, and how does Laravel utilize it?
Composer is a dependency manager for PHP that allows developers to manage project dependencies. Laravel utilizes Composer to handle its own dependencies and streamline package management within Laravel projects.

### Q4. Explain the concept of routing in Laravel.
Routing in Laravel refers to the process of defining the routes for incoming HTTP requests and mapping them to appropriate controller actions. Laravel's routing system provides a clean and expressive way to define application endpoints.

### Q5. What is middleware in Laravel?
Middleware acts as a bridge between the incoming HTTP requests and the application's response. It enables developers to filter and modify requests before they reach the application's core logic. Laravel includes middleware for tasks such as authentication, CSRF protection, and logging.

### Q6. What is Eloquent ORM?
Eloquent is Laravel's built-in ORM (Object-Relational Mapping) system, which simplifies database interactions by mapping database tables to PHP objects. It offers an intuitive syntax for performing CRUD (Create, Read, Update, Delete) operations on database records.

### Q7. How do you define relationships in Eloquent ORM?
In Eloquent ORM, relationships between database tables are defined using methods such as hasOne, hasMany, belongsTo, belongsToMany, and morphTo. These methods establish associations between different models based on their database relationships.

### Q8. What is a Blade templating engine?
Blade is Laravel's powerful templating engine that enables developers to write clean and efficient PHP code within their views. It provides features like template inheritance, control structures, and easy integration with data from controllers.

### Q9. Explain database migrations in Laravel.
Database migrations in Laravel allow developers to manage database schema changes in a version-controlled manner. Migrations are written as PHP classes and provide a convenient way to create, modify, or rollback database tables and columns.

### Q10. What is Laravel Artisan?
Artisan is a command-line interface included with Laravel that provides a variety of useful commands for tasks such as generating code, running migrations, clearing caches, and managing application assets. It simplifies common development tasks and boosts developer productivity.

### Q11: How do you create a new Laravel project?
To create a new Laravel project, you can use Composer to install Laravel globally via the command: 
`composer create-project --prefer-dist laravel/laravel projectName.`

### Q12. What is route caching in Laravel?
Route caching is a performance optimization technique in Laravel that compiles the application's route definitions into a single file for faster route resolution. It significantly reduces the time required to process incoming HTTP requests.

### Q13. What is the difference between Laravel and Lumen?
Laravel and Lumen are both PHP frameworks created by Taylor Otwell. While Laravel is a full-featured framework suitable for building large-scale applications, Lumen is a lightweight micro-framework designed for developing fast and efficient APIs and microservices.

### Q14, What is Laravel Mix?
Laravel Mix is a fluent API wrapper for Webpack, a popular JavaScript module bundler. It simplifies the process of compiling assets like CSS, JavaScript, and images by providing a clean and expressive API for common front-end build tasks.

### Q15. How do you handle authentication in Laravel?
Laravel provides a built-in authentication system that makes it easy to authenticate users with features like user registration, login, password reset, and session management. Developers can use Laravel's authentication scaffolding or customize it to fit their application's requirements.

### Q16. What is Laravel Horizon?
Laravel Horizon is a dashboard and monitoring tool for Laravel's queue system. It provides insights into queued jobs, failed jobs, throughput, and performance metrics, allowing developers to optimize and manage their application's background processing tasks.

### Q17. How do you schedule tasks in Laravel?
Laravel's task scheduling feature allows developers to define recurring tasks that run automatically at specified intervals. Tasks are defined using the Artisan command scheduler and can execute commands, closures, or queued jobs at regular intervals.

### Q18. What is a Laravel Passport?
Laravel Passport is an OAuth2 server implementation built on top of Laravel. It provides a simple and secure way to implement authentication and authorization for APIs by issuing access tokens and managing token-based authentication.

### Q19. Explain Laravel Cashier.
Laravel Cashier is a package for handling subscription billing and recurring payments in Laravel applications. It provides a fluent interface for managing subscriptions, handling subscription billing cycles, and integrating with popular payment gateways like Stripe and Braintree.

### Q20. What is Laravel Socialite?
Laravel Socialite is an OAuth authentication package that simplifies the process of integrating social authentication providers like Facebook, Twitter, Google, and GitHub into Laravel applications. It provides a unified API for authenticating users via social media platforms.

### Q21. How do you handle file uploads in Laravel?
In Laravel, file uploads are handled using the built-in Request object, which provides methods for retrieving uploaded files and storing them in the filesystem or cloud storage services like Amazon S3. Laravel's validation and security features ensure safe and secure file uploads.

### Q22. What are Laravel Collections?
Laravel Collections are a set of powerful array manipulation methods provided by Laravel's `Illuminate\Support\Collection` class. They offer a fluent API for filtering, mapping, sorting, and aggregating array data, making it easy to work with collections of objects or database query results.

### Q23. How do you handle sessions in Laravel?
Laravel's session management system allows developers to store and retrieve user session data across multiple requests. Session data can be stored in various drivers such as file, database, or Redis, providing flexibility and scalability for session management.

### Q24. What is Laravel Valet?
Laravel Valet is a development environment tool for macOS that provides a simple way to serve Laravel applications locally. It configures your system to run PHP applications using Nginx and Dnsmasq, making it easy to develop and test Laravel projects on your local machine.

### Q25. How do you deploy Laravel applications?
Laravel applications can be deployed to production servers using various methods such as FTP, SSH, Git, or cloud deployment platforms like Laravel Forge, Envoyer, and Vapor. It's essential to follow best practices for deploying Laravel applications to ensure reliability and security.

### Q26. Difference between Guards, Gates & Policies ?
- `Guards:` Guards handle user authentication and define where the application should look for user credentials (e.g., sessions, tokens).
- `Gates:` Gates are used to define and check authorization policies for specific actions or operations in your application.
- `Policies:` Policies are used to define authorization policies for specific models, making it easier to manage rules for CRUD operations related to those models.

### Q27. Why use Laravel Eloquent instead of the DB facade:
Laravel Eloquent ORM (Object-Relational Mapping) is a powerful and elegant ActiveRecord implementation for working with databases. It acts as a bridge between your applications and the underlying relational database. Eloquent makes it easy to interact with your database using objects, making interactions more intuitive and efficient.

Eloquent is a good solution for processing a single entity in CRUD manner — that is, create a new entity with filled properties and then save it to a database, load a record from a database, or delete.

- You can write code that is object-oriented.
- It is easier to use than writing raw SQL or using the DB facade.
- There is no binding to the table schema, just change the table name in the Eloquent model.
- Relationships between tables can be maintained in an elegant way. Just mention the type of relationship (JOIN, LEFT JOIN, RIGHT JOIN etc.) to get data from related tables.
- Eloquent queries are more readable than raw SQL or the DB facade.
- You can use methods, scopes, accessors, modifiers etc. inside of a model, which is a maintainable pattern.

### Q28. What is Query Builder ?
Laravel's Query Builder is a powerful and flexible tool for interacting with your database, which provides a convenient, fluent interface for creating and running database queries.

The Laravel query builder uses PDO parameter binding to protect your application against SQL injection attacks. There is no need to clean or sanitize strings passed to the query builder as query bindings.

### Q29. What is primary key in Laravel ?
A primary key is a fundamental concept in database design. It is a unique identifier that is assigned to each record in a database table. The primary key serves as a means to uniquely identify and distinguish individual rows or records within a table.

### Q30. What is Laravel Breeze ?
Laravel Breeze is a minimal, simple implementation of all of Laravel's authentication features, including login, registration, password reset, email verification, and password confirmation and profile. 

### Q31. What is the principal diferences between Doctrine and Eloquent?
`Data Source Architectural Patterns :` Active Record, Data Mapper

Eloquent makes use of the Active Record, where the Model will be the representation of the database and also through it the operations in the database will be made.

Doctrine makes use of it depends on an Entity Manager — to be able to perform the actions in the database and the entities are representations of the database tables.

- `Active Record`
An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data.

- `Data Mapper`
A layer of mappers that moves data between objects and a database while keeping them independent of each other and the mapper itself.

### Q32. Explain Events in laravel ?
An event is an action or occurrence recognized by a program that may be handled by the program or code. Laravel events provides a simple observer implementation, that allowing you to subscribe and listen for various events/actions that occur in your application.

All Event classes are generally stored in the `app/Events` directory, while their listeners are stored in `app/Listeners` of your application.

### Q33. Explain validations in laravel?
Laravel provides several different ways to validate your application incoming data. By default Laravel's base controller class uses a `ValidatesRequests trait` which provides a convenient method to validate all incoming HTTP requests coming from client.You can also validate data in laravel by creating Form Request.
```php
$validatedData = $request->validate([
    'name' => 'required|max:255',
    'username' => 'required|alpha_num',
    'age' => 'required|numeric',
]);
```

### Q34. How to install laravel ?
We can install Laravel via composer. There are two options to install Laravel via composer. 
`Note:` that you should have PHP and Composer installed.

Below code shows the first way to create the app using Composer.

`composer create-project laravel/laravel your-project-name version`

Below Code shows the second way to create the Laravel app using Composer
```bash
composer global require laravel/installer
 
laravel new your-project-name

cd your-project-name
 
php artisan serve
```

### Q35. What is PHP artisan. List out some artisan commands ?
PHP artisan is the command line interface/tool included with Laravel. It provides a number of helpful commands that can help you while you build your application easily.

```bash
php artisan list
php artisan help
php artisan tinker
php artisan make
php artisan --versian
php artisan make:model model_name
php artisan make:controller controller_name
```

### Q36. What are named routes in Laravel?
Named routes allow referring to routes when generating redirects or Urls more comfortably. You can specify named routes by chaining the name method onto the route definition:
```php
Route::get('user/profile', function () {
    //
})->name('profile');
```
You can specify route names for controller actions:

> Route::get('user/profile', 'UserController@showProfile')->name('profile');

Once you have assigned a name to your routes, you may use the route's name when generating URLs or redirects via the global route function:
```php
// Generating URLs...
$url = route('profile');
// Generating Redirects...
return redirect()->route('profile');
```

### Q37. What is database migration. How to create migration via artisan ?
Migrations are like version control for your database, that's allow your team to easily modify and share the application's database schema. Migrations are typically paired with Laravel's schema builder to easily build your application's database schema.

Use below commands to create migration data via artisan.
```bash
php artisan make:migration create_users_table
```

### Q39. What are Laravel Contract's ?
Laravel's Contracts are nothing but a set of interfaces that define the core services provided by the Laravel framework.

### Q40. Explain Facades in Laravel ?
Laravel Facades provides a static like an interface to classes that are available in the application's service '. Laravel self-ships with many facades which provide access to almost all features of Laravel's. Laravel facades serve as "static proxies" to underlying classes in the service ' and provide benefits of a terse, expressive syntax while maintaining more testability and flexibility than traditional static methods of classes. All of Laravel's facades are defined in the `Illuminate\Support\Facades` namespace. You can easily access a facade like so:
```php
use Illuminate\Support\Facades\Cache;

Route::get('/cache', function () {
    return Cache::get('key');
});
```

### Q41. What are Laravel eloquent ORM?
Laravel's Eloquent ORM is simple Active Record implementation for working with your database. Laravel provide many different ways to interact with your database, Eloquent is most notable of them. Each database table has a corresponding "Model" which is used to interact with that table. Models allow you to query for data in your tables, as well as insert new records into the table.
```php
// Querying or finding records from products table where tag is 'new'
$products= Product::where('tag','new');
// Inserting new record 
$product =new Product;
$product->title="Iphone 7";
$product->price="$700";
$product->tag='iphone';
$product->save();
```

### Q42. How to enable query log in Laravel ?
Use the enableQueryLog method to enable query log in Laravel: `DB::connection()->enableQueryLog(); `
You can get array of the executed queries by using getQueryLog method: `DB::getQueryLog();`

### Q43. What is reverse routing in Laravel?
Laravel reverse routing is generating URL's based on route declarations. Reverse routing makes your application so much more flexible. It defines a relationship between links and Laravel routes. When a link is created by using names of existing routes, appropriate Uri's are created automatically by Laravel. Here is an example of reverse routing.
`Route::get('login', 'users@login');`

Using reverse routing we can create a link to it and pass in any parameters that we have defined. Optional parameters, if not supplied, are removed from the generated link.

`{{ HTML::link_to_action('users@login') }}`
It will automatically generate an Url like http://xyz.com/login in view.

### Q44. How to turn off CRSF protection for specific route in Laravel?
To turn off CRSF protection in Laravel add following codes in `app/Http/Middleware/VerifyCsrfToken.php`
```php
//add an array of Routes to skip CSRF check
private $exceptUrls = ['controller/route1', 'controller/route2'];
//modify this function
public function handle($request, Closure $next) {
    //add this condition 
    foreach($this->exceptUrls as $route) {
        if ($request->is($route)) return $next($request);
    }
    return parent::handle($request, $next);
}
```

### Q45. What are traits in Laravel?
PHP Traits are simply a group of methods that you want include within another class. A Trait, like an abstract class cannot be instantiated by itself. Trait are created to reduce the limitations of single inheritance in PHP by enabling a developer to reuse sets of methods freely in several independent classes living in different class hierarchies.

Here is an example of trait.
```php
trait Sharable {
public function share($item)
{
return 'share this item';
}
}
```
You could then include this Trait within other classes like this:
```php
class Post {
use Sharable;
}

class Comment {
use Sharable;
}
```
Now if you were to create new objects out of these classes you would find that they both have the share() method available:
```php
$post = new Post;
echo $post->share(''); // 'share this item' 
 
$comment = new Comment;
echo $comment->share(''); // 'share this item'
```

### Q46. Does Laravel support caching?
Yes, Laravel supports popular caching backends like `Memcached` and `Redis`.
By default, Laravel is configured to use the file cache driver, which stores the serialized, cached objects in the file system.

### Q47. Explain Laravel's Middleware?
As the name suggests, Middleware acts as a middleman between request and response. It is a type of filtering mechanism. For example, Laravel includes a middleware that verifies whether the user of the application is authenticated or not. If the user is authenticated, he will be redirected to the home page otherwise, he will be redirected to the login page.

There are two types of Middleware in Laravel.
`Global Middleware:` will run on every HTTP request of the application.
`Route Middleware:` will be assigned to a specific route.

### Q48. What is Lumen?
Lumen is PHP micro-framework that built on Laravel's top components.It is created by Taylor Otwell. It is perfect option for building Laravel based micro-services and fast REST API's. It's one of the fastest micro-frameworks available.
You can install Lumen using composer by running below command

`composer create-project --prefer-dist laravel/lumen blog`

### Q49. Explain Bundles in Laravel?
In Laravel, bundles are also called packages. Packages are the primary way to extend the functionality of Laravel. Packages might be anything from a great way to work with dates like Carbon, or an entire BDD testing framework like Behat.In Laravel, you can create your custom packages too. You can read more about packages from here

### Q50. How to use custom table in Laravel Modal ?
You can use custom table in Laravel by overriding protected $table property of Eloquent.
```php
class User extends Eloquent{
    protected $table="my_user_table";
}
```

### Q51. List some Aggregates methods provided by query builder in Laravel ?
- count()
- max()
- min()
- avg()
- sum()

### Q52. How to check request is ajax or not ?
In Laravel, we can use $request->ajax() method to check request is ajax or not.
```php
public function saveData(Request $request)
{
    if($request->ajax()){
        return "Request is of Ajax Type";
    }
    return "Request is of Http type";
}
```

### Q53. What is Laravel Vapor?
It is a serverless deployment platform that is powered by AWS. Laravel Vapor provides on-demand auto-scaling with zero server maintenance.

### Q54. What is the Laravel Cursor?
The cursor method in the Laravel is used to iterate the database records using a cursor. It will only execute a single query through the records. This method is used to reduce the memory usage when processing through a large amount of data.
```php
foreach (Product::where('name', 'Bob')->cursor() as $fname) {
//do some stuff
}
```
### Q55. What is the use of dd() in Laravel?
In Laravel, dd() is a helper function used to dump a variable's contents to the browser and stop the further script execution. It stands for Dump and Die. It is used to dump the variable/object and then die the script execution. You can also isolate this function in a reusable functions file or a Class.

### Q56. What is yield in Laravel?
In Laravel, `@yield` is principally used to define a section in a layout and is constantly used to get content from a child page unto a master page. So, when the Laravel performs blade file, it first verifies if you have extended a master layout, if you have extended one, then it moves to the master layout and commences getting the @sections.

### Q57. How to clear Cache in Laravel?
You can use `php artisan cache:clear` commnad to clear Cache in Laravel.

### Q58. What is Laravel nova?
Laravel Nova is an admin panel by laravel ecosystem. It easy to install and maintain. Laravel Nova comes with features that have ability to administer our database records using Eloquent.

### Q58. What Is CSRF Token In Laravel
CSRF stands for Cross-site request forgeries. CSRF is an malicious attack that can be done by unauthorized websites based on an authenticated user. CSRF token is stored in the user session.

Laravel generates automatic CSRF token for each session that identify the request via CSRF token that we pass in requests. In this way it helps and save us form malicious websites. CSRF token is automatically generated by Laravel by itself for each application user. Whenever you do put a request with your form except getting method it gets regenerated with the session.

### Q59. What Is Controller In Laravel
You can build your logics and can handle the data in controller Instead of doing it in route files. Controller can group the related classes for a particular model or as you wish you can do multiple actions also.

You can see controller files under `app/Http/Controllers` directory.

`php artisan make:controller ControllerName`

### Q60. How to Redirect a Request to Controller Action
`Route::get('url', [HomeController::class, 'method_name'])->name('routename');`

### Q61. How to Redirect a Request with Session Data
This very easy to redirect a request with session data. You also have to use `"Illuminate\Http\Request"` in the file.

`$request->session()->flash('status', 'Task was successful!');`

### Q62. How to Return A JSON Response In Laravel
if you do use this JSON method in Laravel it automatically set the header "content-type" and and given array in JSON form.
```php
return response()->json([
    'name' => 'fist',
    'age' => 35,
])
```
### Q63. How to Pass Data to View In Laravel
You have to use the view() method. `return view('superadmin.users', compact('Users'));`

### Q64. How to Create a Component In Laravel
Creating a Component in Laravel is very easy when we have artisan structure.

`php artisan make:component ComponentName`
You can see newly created component under `app/View/Components` directory.

### Q65. Why we use @once Directive In Laravel Blade
`@Once` as name defines, used for pushing any single piece of code at once when script rendered in first time.

### Q66. How to Delete, Retrieve and Store Data With Session In Laravel
```php
/* Store data in a session: */
`$request->session()->put('key', 'value');`

/* Delete data from the session: */

// delete a single key...
$request->session()->forget('name');
 
// delete multiple keys...
$request->session()->forget(['name', 'status']);

// Retrieve data from the session:
$request->session()->get('key');
```
### Q67. What are the available route methods in Laravel?

Routes help us to land on any HTTP verb and also help us to communicate with any kind of controller or model. In Laravel, we can find common routes in `routes/web.php` file

Sometimes we need to define routes that respond to multiple HTTP verbs then we can use match method.
```php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```

### Q68. How to print route lists using Command?
We can use `route:list` Artisan command to print the list of routes.

### Q69. What are the Regular Expression Constraints in Laravel?
You can use where() method on a route instance in which you can pass the name of the parameter and regular expression also. Which defined how parameters should be constrained. For example, you are passing a route with a user modal and you have a parameter with a user id so you can specify that the id should be a number.
```php
Route::get('/user/{id}', function ($id) {
    //
})->where('id', '[0-9]+');
```

### Q70. How to check whether a user is authenticated or not in Laravel?
You have two options here to check whether a user is authenticated or not.

First, if you are into any php file you can use like:
```php
if(Auth::check()){
//code
}
```
And if you are adding any condition in the blade file then you can use like this:

```php
@auth()
@endauth
```

### Q71. How to generate Model Class in Laravel?
You can find Models in `app/models` the directory. The model extends `Illuminate\Database\Eloquent\Model` class. We can use the make: model artisan command to generate a modal class. `php artisan make:model ModelName`

### Q72. Quick Questions About Laravel
- Laravel is written In	PHP Programming (PHP 7)
- Laravel is a PHP Framework for Developing websites and mobile API's.
- Laravel is developed By	Taylor Otwell
- Laravel is Based on	MVC architectural pattern
- Laravel Dependencies	Composer, OpenSSL, PDO, Mbstring, etc.
- Laravel Licence	MIT License
- Laravel Current Stable release 6.9.0

### Q73. Get current action name in Laravel?
We can use  `request()->route()->getActionMethod()`
