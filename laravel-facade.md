## What are the Facade?
In Laravel, a Facade is a design pattern that provides a static interface to the classes that are available in the Laravel service container. Facades provides direct access to object from a container. Facades provides us the static interface to classes.
```php
// if want to use it then we wll use like:
$car = new car();
$car->name();

// with the help of facade we can use it like:
car::name();
```
All of Laravel's facades are defined in the `Illuminate\Support\Facades` namespace.

## Explain Facade in Laravel?
A Facade is a proxy to a dependency registered in the container.

Unlike true static methods, they provide a static interface without reducing testability. They also facilitate dependency swapping at run time.

Facades have a bad reputation because people think you perform a real static call. But they're not. Here's a representation of what happens that I stole in one of my tweets:

`You → Facade::foo() → Laravel → __callStatic() → Container → $dependency→foo()`

You call Facade::foo();
Laravel intercepts the call to the foo() method using the magic method __callStatic()
It pulls from the container the corresponding dependency (which is defined in a Facade's getFacadeAccessor() method).
Then, the framework basically translates Facade::foo() to app('some-dependency')->foo().

## How does Facade works?

- `Static Proxy:` Facades act as static proxies to underlying classes in Laravel's service container. This means you can access methods on these classes in a static-like manner without needing to instantiate them directly.
- `Clean Syntax:` Facades provide a clean and expressive syntax for accessing Laravel services. For example we can use a facade to access its methods directly.
- `Global Accessibility:` You know Facades are globally accessible, that means you can use them anywhere in your application.
- `Service Container Integration:` Under the hood, facades interact with Laravel's service container to resolve the actual instances of the classes they represent.


In Laravel, there are several built-in facades that provide a simplified interface to various components of the framework. Here are some commonly used facades and their corresponding underlying classes:

1. `Cache:` Illuminate\Support\Facades\Cache – Access the caching system

Underlying Class: Illuminate\Cache\CacheManager

2. `Config:` Illuminate\Support\Facades\Config – Access the configuration values

Underlying Class: Illuminate\Config\Repository

3. `DB:` Illuminate\Support\Facades\DB – Interact with the database using Laravel's query builder

Underlying Class: Illuminate\Database\DatabaseManager

4. `Event:` Illuminate\Support\Facades\Event – Manage events and event listeners

Underlying Class: Illuminate\Events\Dispatcher

5. `File:` Illuminate\Support\Facades\File – Perform file system operations

Underlying Class: Illuminate\Filesystem\Filesystem

6. `Log:` Illuminate\Support\Facades\Log – Write log messages

Underlying Class: Illuminate\Log\Logger

7. `Mail:` Illuminate\Support\Facades\Mail – Send emails

Underlying Class: Illuminate\Mail\Mailer

8. `Queue:` Illuminate\Support\Facades\Queue – Work with queues and jobs.

Underlying Class: Illuminate\Queue\QueueManager

9. `Route:` Illuminate\Support\Facades\Route – Define routes and handle HTTP requests

Underlying Class: Illuminate\Routing\Router

10. `Session:` Illuminate\Support\Facades\Session – Manage session data

Underlying Class: Illuminate\Session\SessionManager

11. `Storage:` Illuminate\Support\Facades\Storage – Interact with storage, such as local or cloud-based filesystems

Underlying Class:  Illuminate\Filesystem\FilesystemManager

12. `URL:` Illuminate\Support\Facades\URL – Generate URLs to your application's routes

Underlying Class: Illuminate\Routing\UrlGenerator
