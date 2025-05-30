## CDN for Bootstrap & Jquery
```html
<link href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.4.1/css/bootstrap.min.css" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.0/jquery.validate.js"></script>  
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.0/additional-methods.min.js"></script> 
<!-- Bootstrap CSS library -->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
<!-- jQuery library -->
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" crossorigin="anonymous"></script>
<!-- JS library -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" crossorigin="anonymous"></script>
<!-- Latest compiled JavaScript library -->
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" crossorigin="anonymous"></script>
```
## Mixins
#### Run all Mix
`npm run dev`

#### Run all Mix and minify
`npm run production`

#### Watching Assets For Changes
`npm run watch`

#### Loading Js & Css
```js
mix.sass('resources/sass/app.sass', 'public/css')
mix.styles([
    'public/css/reset.css',
    'public/css/common.css'
], 'public/css/app.css');

mix.js('resources/js/app.js', 'public/js');
mix.js('resources/js/app.js', 'public/js').version();  
mix.scripts([
    'public/js/utils.js',
    'public/js/theme.js'
], 'public/js/all.js');

mix.copy('resources/font-awesome/fonts/*', 'public/fonts/');
mix.copyDirectory('resources/img', 'public/images');
```
> More Methods
```js
mix.js(src, output);
mix.react(src, output); // Identical to mix.js(), but registers React Babel compilation.
mix.extract(vendorLibs);
mix.sass(src, output);
mix.standaloneSass('src', output); // Faster, but isolated from Webpack.
mix.fastSass('src', output);  // Alias for mix.standaloneSass().
mix.less(src, output);
mix.stylus(src, output);
mix.postCss(src, output, [require('postcss-some-plugin')()]);
mix.browserSync('my-site.dev');
mix.combine(files, destination);
mix.babel(files, destination); // Identical to mix.combine(), but also includes Babel compilation.
mix.copy(from, to);
mix.copyDirectory(fromDir, toDir);
mix.minify(file);
mix.sourceMaps(); // Enable sourcemaps
mix.version(); // Enable versioning.
mix.disableNotifications();
mix.setPublicPath('path/to/public');
mix.setResourceRoot('prefix/for/resource/locators');
mix.autoload({});  // Will be passed to Webpack's ProvidePlugin.
mix.webpackConfig({});  // Override webpack.config.js, without editing the file directly.
mix.then(function () {}) // Will be triggered each time Webpack finishes building.
mix.options({
  extractVueStyles: false, // Extract .vue component styling to file, rather than inline.
  processCssUrls: true, // Process/optimize relative stylesheet url()'s. Set to false, if you don't want them touched.
  purifyCss: false, // Remove unused CSS selectors.
  uglify: {}, // Uglify-specific options. https://webpack.github.io/docs/list-of-plugins.html#uglifyjsplugin
  postCss: [] // Post-CSS options: https://github.com/postcss/postcss/blob/master/docs/plugins.md
});
```

## Blade directives:
- `@include` pulls in information from the files in the includes directory.
- `@yield` pulls in information from the content section of the home page.
- `@extends` tells Laravel that the contents of this file extend another view. It also defines the view - that it's extending as our default template: resources/views/layouts/default.blade.php.
- `@section` defines the beginning of a section, which we have named content. Everything contained in this section will appear in the @yield which we defined in the template.
- `@endsection` closes that section. 
- `@section and @endsection` are terms which must be used in pairs, much like other HTML tags (for example, `<div> and </div>`).
> Examples

1. @include
```php
        @include('partials.sidebar',['menu' => $menu])
```
2. @push & @stack
3. @php
4. @hasSection
```php
        @hasSection('navigation') 
        // your logic here
        @endif
```
5. @each
```php
        @each('users.index', $users, 'user', 'users.notfound')
```
6. @includeWhen
```php
        @includeWhen($isUserAdmin, 'users.admin_card', ['user' => $user])
```
7. @json
```php
        var data = @json($product);
```
8. @forelse
```php
        @forelse($users as $user)
        {{ $user->name }}
        @empty
        No Users Found
        @endforelse
```
9. @verbatim
```php
        @{{ name }} 
        @verbatim
        {{ name }}
        @endverbatim
``` 
10. @isset & @empty
```php
        @isset($users)
        // your logic here
        @endisset

        @empty($users)
        // your logic here
        @endempty
```
11. @inject
```php
        @inject('menu', 'App\Services\MenuService')
        // then in your view
        {!! $menu->render() !!}
```
12. @csrf & @method

## Understanding Macros
A macro is the injection of custom code into the core of a framework, without the need for complex inheritance or modification.

To implement macros we use the Macroable trait, which has two main methods `macro()` and `mixin()`.

Laravel macros allow you to extend core functionality and create custom methods for classes like Eloquent models and collections.

### Mixin Macro
The concept of macros allows multiple user-defined functions to be registered at once via a mixin class. Each method of which must return a closure. The name of the method acts as the name of the macro.

## Macroable

Allow Laravel's macros to be created by using the `Illuminate\Support\Traits\Macroable` trait. 
Here are some of the most commonly used classes to create macros :

- **Request:** Illuminate\Http\Request
- **Response:** Illuminate\Http\Response
- **Collection:** Illuminate\Support\Collection
- **Str:** Illuminate\Support\Str
- **Router:** Illuminate\Routing\Router
- **UrlGenerator:** Illuminate\Routing\UrlGenerator
- **Cache:** Illuminate\Cache\Repository
- **Filesystem:** Illuminate\Filesystem\Filesystem
- **Arr:** Illuminate\Support\Arr
- **Rule:** Illuminate\Validation\Rule

## What are Notifications?
Laravel provides mailables which can be used to send emails, these emails are long with custom markup styling but we can not be sent through different channels. Laravel Notifications solves our this problem, by providing an elegant and fun way to send notifications to users.

### Laravel Notifications Channels
The whole beauty of Laravel Notifications is that it allows you to choose from different notifications channels through which your notifications will be delivered. Let's go through the different notifications channels currently supported by Laravel Framework.

- **Mail :** The notifications will be send in the form of email to users.
- **SMS :** Users will revieve notifications on their mobile phones.
- **Database :** This option allows you to save notifications into the database, which you can show to users in your own way.
- **Slack :** This option allows us to send notification messages to Slack channel.

## Storage
```
Storage::disk('local')->put('filename.txt', 'File content goes here..');
Storage::get('filename.txt');
Storage::append('filename.txt', 'Append this text');
Storage::prepend('filename.txt', 'Append this text');
Storage::download('filename.txt');
Storage::url('filename.txt');
Storage::size('filename.txt');
Storage::delete('filename.txt');
Storage::files($directory);
Storage::allFiles($directory);
Storage::directories($directory);
Storage::allDirectories();
Storage::makeDirectory($name);
Storage::deleteDirectory($name);
```
## API resources

API resources present a way to easily transform our models into JSON responses. 
It acts as a transformation layer that sits between our Eloquent models and the JSON responses that are actually returned by our API. 
API resources is made of two entities: a resource class and a resource collection. 
A resource class represents a single model that needs to be transformed into a JSON structure, while a resource collection is used for transforming collections of models into a JSON structure.

### Status Code	Meaning
- `200:` OK. The standard success code and default option.
- `201:` Object created. Useful for the store actions.
- `204:` No content. When an action was executed successfully, but there is no content to return.
- `206:` Partial content. Useful when you have to return a paginated list of resources.
- `400:` Bad request. The standard option for requests that fail to pass validation. (Wrong with URL or parameters)
- `401:` Unauthorized. The user needs to be authenticated.
- `403:` Forbidden. The user is authenticated, but does not have the permissions to perform an action.
- `404:` Not found. This will be returned automatically by Laravel when the resource is not found.
- `422:` Un-processable Entity (validation failed)
- `500:` Internal server error. Ideally you're not going to be explicitly returning this, but if something unexpected breaks, this is what your user is going to receive.
- `503:` Service unavailable. Pretty self explanatory, but also another code that is not going to be returned explicitly by the application.

## Laravel Pagination Instance Methods
```php
$results->count()
$results->currentPage()
$results->firstItem()
$results->hasMorePages()
$results->lastItem()
$results->lastPage() (Not available when using simplePaginate)
$results->nextPageUrl()
$results->onFirstPage()
$results->perPage()
$results->previousPageUrl()
$results->total() (Not available when using simplePaginate)
$results->url($page)
```
## Laravel Model Events
Eloquent provides a handful of events to monitor the model state which are:

- `retrieved` : after a record has been retrieved.
- `creating` : before a record has been created.
- `created` : after a record has been created.
- `updating` : before a record is updated.
- `updated` : after a record has been updated.
- `saving` : before a record is saved (either created or updated).
- `saved` : after a record has been saved (either created or updated).
- `deleting` : before a record is deleted or soft-deleted.
- `deleted` : after a record has been deleted or soft-deleted.
- `restoring` : before a soft-deleted record is going to be restored.
- `restored` : after a soft-deleted record has been restored.

## Laravel Scheduler Methods
Laravel scheduler provides many methods which map to a cron job timing, some of them are below which I have used a lot.
```php
->everyMinute(); // Run the task every minute
->everyFiveMinutes(); // Run the task every five minutes
->hourly(); // Run the task every hour
->hourlyAt(15); // Run the task every hour at 15 mins past the hour
->daily(); // Run the task every day at midnight
->dailyAt('15:00'); // Run the task every day at 15:00
->weekly(); // Run the task every week
->monthly(); // Run the task every month
```
## Understanding Cron Jobs and Laravel's Task Scheduler
Cron jobs are time-based tasks that run automatically at specified intervals on a Unix-based system. Laravel's Task Scheduler allows you to define and manage these cron jobs using expressive syntax and commands.

> Cron setup
`* * * * * php /path-to-your-laravel-project/artisan schedule:run >> /dev/null 2>&1`

## Why Laravel might be vulnerable to SQL injections?
- Raw queries
- Insufficient input validation
- Insecure query building
- Failure to use parameterized statements
- Outdated libraries or framework versions

## What is LaravelData?
`Spatie` developed this package to simplify data transfer and transformation within Laravel applications. It allows you to easily manage data coming into and going out of your application, ensuring that it is properly formatted, validated, and transformed as needed.

### Benefits Of Using LaravelData
- `Cleaner Code:` DTO helps us to keep code organized by using Data Transfer Objects (DTOs) to manage data, making it easier to read and maintain.

- `Easy Validation:` You can simplify data validation by defining validation rules directly in your DTOs, ensuring that data is correct before processing it.

- `Automatic Data Transformation:` DTO automatically transforms data between different formats, saving the time and reducing errors when working with APIs.

- `Type Safety:` By defining data types in DTOs, it helps catch errors early, ensuring that your data is always in the expected format.

- `Simplified Serialization:` It makes serializing and deserializing data easy, so you can quickly convert data to and from formats like JSON, without writing extra code.

## Permission

```bash
composer require spatie/laravel-permission
```

Publish the configuration file for the Spatie package by running the following command. This command will create a permissions.php configuration file in your config directory.

`php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="config"`

## Sitemap

To generate a dynamic sitemap XML file in Laravel, we'll use a package called laravel-sitemap. You can install it via Composer by running the following command.

```bash
composer require spatie/laravel-sitemap
php artisan make:controller SitemapController
```

## What is ReactPHP ?

PHP has been known for its synchronous, blocking execution model, but ReactPHP changes that by enabling asynchronous programming within PHP.

ReactPHP is a low-level library for event-driven programming in PHP. It allows you to build scalable applications with non-blocking I/O operations, making it perfect for real-time applications, long-running processes, and microservice

ReactPHP is an open-source, event-driven library for asynchronous programming in PHP.

It brings non-blocking I/O capabilities to PHP, enabling you to execute tasks like reading from a file, querying a database, or sending an HTTP request without waiting for the operation to complete.

This non-blocking behavior allows PHP to handle multiple tasks concurrently, improving performance in scenarios that require real-time.

## To show HTML tag directly in view
`{{ $username }}` is simply used to display text contents but `{!! $username !!}` is used to display content with HTML tags if exists.

## To add Constant 
Add in config
```php
return [
  'ADMINEMAIL' => 'info@example.com',
];
```
Now we can display constant:
`Cofig::get('constants.ADMINEMAIL');`

## To add CSRF ?
In between head, tag put `<meta name="csrf-token" content="{{ csrf_token() }}">` and in Ajax, we have to add

```js
$.ajaxSetup({
   headers: {
     'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
   }
});
```