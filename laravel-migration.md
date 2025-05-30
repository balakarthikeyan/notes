# The New Features of Laravel

### Laravel 6:

- The support of Semantic Versioning
- Compatibility with Vapor, a serverless deployment platform for Laravel
- Improved Authorization Responses
- **Job Middleware:** A new feature that allows you wrap custom logic around the execution of queued jobs
- **Lazy Collections:** A new feature that leverages PHP's generators to enable you to efficently work with very large datasets
- **Eloquent Subquery** Enhancements
- **Laravel UI:** UI scaffolding logic such as Bootstrap or Vue, is extracted in its own laravel/ui package.
- **Ignition:** A new and smart error page.

Other features

- Inbuilt CRSF (`Cross-Site Request Forgery`) Protection.
- Inbuilt paginations
- Reverse Routing
- Query builder
- Route caching
- Database Migration
- IOC (`Inverse of Control`) Container Or service container.
- Job middleware
- Lazy collections

### Laravel 7:

- **Laravel Airlock:** An official package for API authentication
- **Custom Eloquent Casts:** They allow you add your won custom casts
- **CORS** support by default i.e without third-party plugins
- **Blade Component Tags & Improvements:** Allows you to create class-less components
- **HTTP Client:** An API for making HTTP requests
- **Route Caching** Speed Improvements

### Laravel 8:
- `Jetstream`, is introduced in Version 8.

#### Migration of Laravel 8 to 9
- **Flysystem 2.0** Laravel 9.x has migrated from Flysystem 1.x to 2.x.
- **Symfony Mailer**  SwiftMailer has stopped the support.
- **Custom Casts & null** In Laravel 9.x, the set strategy of the cast course will be invoked with null as the given $value argument
- **Default HTTP Client Timeout** The HTTP client now includes a default timeout of 30 seconds.
- **The lang Directory** the resources/lang directory as of now is located within the root project directory.
- **The Password Rule** which validates that the given input esteem matches the confirmed user's current password.

#### Migration of Laravel 9 to 10
- Remove various deprecations
- Remove deprecated `dates` property
- Remove `handleDeprecation` method
- Remove deprecated `assertTimesSent` method
- Remove deprecated `ScheduleListCommand`'s $defaultName property
- Remove deprecated `Route::home` method
- Remove deprecated `dispatchNow` functionality

### Laravel 9:

Laravel 9 installation requires the most up-to-date form of `PHP 8, PHPUnit 9, has Symfony 6`

- Anonymous Stub Migration
```php
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
```php
use Illuminate\Contracts\Database\QueryBuilder; // interface
use Illuminate\Database\Eloquent\Concerns\DecoratesQueryBuilder; // trait

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
```php
use Illuminate\Support\Str // class

// functions include the use of str_contains(), str_starts_with(), and str_ends_with() 
```

### Laravel 10:
Laravel 10 installation requires the most up-to-date form of PHP 8.1
- Artisan Command Becomes More Interactive
- Invokable Validation Rules by Default
- Uses Native Types Instead of Docblocks
- Dropped Support for Predis v1

### Laravel 11:
Laravel 11 introduce streamlined application structure, per-second rate limiting, health routing etc.

### Laravel 12:
- New database structures.
- MD5 hashing algorithm with xxHash
    * Caching
    * Generating unique identifiers
    * Data hashing in high-throughput applications
- Optimized Query Execution
- UUID v7 for Models
    * UUID v7 is its time-based component, making it more suitable for ordered queries and indexing, leading to faster lookups and sorting.
- Native Support for MariaDB
- Enhanced Collections
- Real-Time Features: Improved WebSocket Integration
    * Laravel 11's introduction of Reverb, Laravel 12 refines WebSocket broadcasting and real-time event handling.
- Testing & Debugging Enhancements
    * Chunked Queries Now Respect Limits
- Pest PHP Integration
- New Artisan Commands

## Laravel Installer Update

Feature: The updated Laravel installer now ensures compatibility with the new starter kits in Laravel 12.

> Laravel 11

```bash 
laravel new my-app  # Uses Laravel 11 skeleton
```

> Laravel 12

```bash 
composer global update laravel/installer
laravel new my-app  # Installs Laravel 12 with new starter kits
```

- `Step 1:` Verify PHP Version - Ensure your server is running PHP 8.2 or higher, as Laravel 12 requires it. 

- `Step 2:` Update Laravel with Composer - `composer require laravel/framework:12.0`

- `Step 3:` Run Database Migrations - `php artisan migrate`, To preview changes - `php artisan migrate --pretend`

## Migration of Laravel 11 to 12 

1. `Enhanced Performance and Query Optimizations`

Performance improvements are a key focus in Laravel 12, especially for large applications handling complex queries.

> Laravel 11 (Previous Approach)

```php
$users = User::all(); // Loads all users into memory, inefficient for large datasets
```

> Laravel 12 (Optimized Query Handling)

```php
$users = User::query()->select(['id', 'name'])->cursor(); // More efficient memory usage 
```

With cursor(), Laravel 12 optimizes large dataset handling by fetching records in smaller chunks, reducing memory consumption.

2. `Improved Eloquent Lazy and Eager Loading`

Eloquent in Laravel 12 now handles relationships more efficiently, reducing N+1 query problems and improving execution speed.

> Laravel 11 (Potential N+1 Query Issue)

```php
$users = User::all();
foreach ($users as $user) {
    echo $user->posts->count(); // Triggers multiple queries
}
```

> Laravel 12 (Optimized Eager Loading)

```php
$users = User::withCount('posts')->get(); // Fetches posts count in a single query
```
By using withCount(), Laravel 12 minimizes database queries, enhancing performance.

3. `Asynchronous Caching for Real-Time Performance`

No more waiting on slow cache operations. Laravel 12 introduces an asynchronous caching method that runs in the background, providing a more responsive experience.

> Laravel 11

```php
use Illuminate\Support\Facades\Cache;
$user = Cache::remember("user_{$id}", 600, function() use ($id) {
    return User::find($id);
});
```

> Laravel 12

```php
use Illuminate\Support\Facades\Cache;
$user = Cache::rememberAsync("user_{$id}", 600, function() use ($id) {
    return User::find($id);
});
```

4. `Streamlined Dependency Injection with PHP 8 Features`

Laravel 12 fully embraces constructor property promotion, reducing boilerplate code and making controllers cleaner.

> Laravel 11

```php
class OrderController
{
    protected $orderService;
    public function __construct(OrderService $orderService)
    {
        $this->orderService = $orderService;
    }
}
```

> Laravel 12

```php
class OrderController
{
    public function __construct(private OrderService $orderService) {}
}
```

5. `Enhanced API Development and Routing Optimizations`

Building robust APIs is easier than ever with native GraphQL support and a cleaner API versioning system. Additionally, Laravel 12 brings performance optimizations behind the scenes — improving routing internals and reducing execution time without changing your existing route definitions.

> Laravel 11

```php
Route::get('/api/v1/products', [ProductController::class, 'index']);
```

> Laravel 12

```php
Route::apiVersion(1)->group(function () {
    Route::get('/products', [ProductController::class, 'index']);
});
```

6. `Next-Generation Eloquent ORM Features`

The Eloquent ORM now supports conditional eager loading and more flexible relationship constraints, enabling you to build complex queries with less code.

> Laravel 11

```php
$orders = Order::with(['items' => function($query) {
    $query->where('status', 'shipped');
}])->get();
```

> Laravel 12

```php
$orders = Order::withFiltered('items', ['status' => 'shipped'])->get();
```

7. `Optimized Queue & Job Handling`

With dynamic job prioritization and advanced retry strategies, Laravel 12 ensures that your critical tasks are processed first.

> Laravel 11

```php
dispatch(new ProcessPayment($payment))->onQueue('high');
```

> Laravel 12

```php
dispatch(new ProcessPayment($payment))->setPriority('high');
```

## Additional Enhancements & Breaking Changes

8. `WorkOS AuthKit Integration`

Feature: Integrates WorkOS AuthKit for SSO, social login, and passkeys in starter kits, offering an alternative to traditional authentication.

> Laravel 11

```php
// routes/web.php
use Laravel\Socialite\Facades\Socialite;
Route::get('/auth/redirect', fn() => Socialite::driver('github')->redirect());
Route::get('/auth/callback', fn() => Auth::login(Socialite::driver('github')->user()));
```

> Laravel 12

```php
// routes/web.php (with WorkOS AuthKit enabled)
use WorkOS\Laravel\AuthKit;
Route::get('/auth/workos', [AuthKit::class, 'redirect']);
Route::get('/auth/workos/callback', [AuthKit::class, 'handleCallback']);
Configuration adjustments in config/auth.php may be required:

'workos' => [
    'client_id' => env('WORKOS_CLIENT_ID'),
    'client_secret' => env('WORKOS_CLIENT_SECRET'),
    'redirect' => '/auth/workos/callback',
],
```
9. `Updated Dependencies (Carbon 3.x)`

Feature: Laravel 12 now requires Carbon 3.x, dropping support for Carbon 2.x, with minor API adjustments.

> Laravel 11

```json 
// composer.json
"require": {
    "laravel/framework": "^11.0",
    "nesbot/carbon": "^2.0"
}
```

> Laravel 12

```json 
// composer.json
"require": {
    "laravel/framework": "^12.0",
    "nesbot/carbon": "^3.0"
}
```

```php
use Carbon\Carbon;
$date = Carbon::parse('2024-01-01')->plusDays(10); // Carbon 3.x preferred syntax
```

10. `Schema::getTableListing Enhancement`

Feature: `Schema::getTableListing()` now returns schema-qualified table names by default.

> Laravel 11

```php
use Illuminate\Support\Facades\Schema;
$tables = Schema::getTableListing(); // ['users', 'posts']
```

> Laravel 12

```php
use Illuminate\Support\Facades\Schema;
$tables = Schema::getTableListing(); // ['main.users', 'main.posts']
$tables = Schema::getTableListing(schemaQualified: false); // ['users', 'posts']
```

11. `Concurrency::run with Associative Arrays`

Feature: When using associative arrays, the keys are preserved in the results.

> Laravel 11

```php
use Illuminate\Support\Facades\Concurrency;
$result = Concurrency::run([
    'task1' => fn() => 1 + 1,
    'task2' => fn() => 2 + 2,
]); // [2, 4]
```

> Laravel 12

```php
use Illuminate\Support\Facades\Concurrency;
$result = Concurrency::run([
    'task1' => fn() => 1 + 1,
    'task2' => fn() => 2 + 2,
]); // ['task1' => 2, 'task2' => 4]
```

12. `mergeIfMissing with Dot Notation`

Feature: Dot notation now expands into nested arrays, enabling cleaner data merging.

> Laravel 11

```php
$request->mergeIfMissing(['user.name' => 'John']); // ['user.name' => 'John']
```

> Laravel 12

```php
$request->mergeIfMissing(['user.name' => 'John']); // ['user' => ['name' => 'John']]
```

13. `Image Validation Rule Change`

Feature: SVG images are no longer allowed by default in the image validation rule, enhancing security and consistency.

> Laravel 11

```php
$request->validate(['avatar' => 'image']); // Allowed PNG, JPG, SVG, etc.
```

> Laravel 12

```php
$request->validate(['avatar' => 'image|mimes:png,jpg']); // SVG blocked
$request->validate(['avatar' => 'image|mimes:svg']);   // Explicitly allow SVG
```