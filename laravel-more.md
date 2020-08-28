## CDN for Bootstrap & Jquery
```
<link href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.4.1/css/bootstrap.min.css" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.0/jquery.validate.js"></script>  
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.0/additional-methods.min.js"></script>  
```
## Mixins
#### Run all Mix
`npm run dev`

#### Run all Mix and minify
`npm run production`

#### Watching Assets For Changes
`npm run watch`

#### Loading Js & Css
```
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
```
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

```
@include pulls in information from the files in the includes directory.
@yield pulls in information from the content section of the home page.
@extends tells Laravel that the contents of this file extend another view. It also defines the view that it's extending as our default template: resources/views/layouts/default.blade.php.
@section defines the beginning of a section, which we have named content. Everything contained in this section will appear in the @yield which we defined in the template.
@endsection closes that section. 
@section and @endsection are terms which must be used in pairs, much like other HTML tags (for example, <div> and </div>).
```
> Examples
```
1. @include
    @include('partials.sidebar',['menu' => $menu])
2. @push & @stack
3. @php
4. @hasSection
    @hasSection('navigation') 
    ...
    @endif
5. @each
    @each('users.index', $users, 'user', 'users.notfound')
6. @includeWhen
    @includeWhen($isUserAdmin, 'users.admin_card', ['user' => $user])
7. @json
    var data = @json($product);
8. @forelse
    @forelse($users as $user)
        {{ $user->name }}
    @empty
        No Users Found
    @endforelse
9. @verbatim
    @{{ name }} //Call
    @verbatim
        {{ name }}
    @endverbatim    
10. @isset & @empty
    @isset($users)
    // your logic here
    @endisset
    @empty($users)
    // your logic here
    @endempty
11. @inject
    @inject('menu', 'App\Services\MenuService')
    // then in your view
    {!! $menu->render() !!}
12. @csrf & @method
```
## Common Query Builder
```
$Query = DB::table('users')
        ->where('active', true)
        ->get();

$Query = DB::table('users')
    ->where('created_at', '>', Carbon::now()->subDay())
    ->get();

$Query = DB::table('users')
        ->where('active', true)
        ->orWhere('last_login', '>', Carbon::now()->subDay())
        ->where('isSubscribed', true)
        ->get();

$Query = DB::table('users')
    ->where('active', true)
    ->orWhere(function ($query) {
        $query->where('last_login', '>', Carbon::now()->subDay())
            ->where('isSubscribed', true);
    })
    ->get();

$Query = DB::table('users')
    ->whereExists(function ($query) {
        $query->select('id')
            ->from('reviews')
            ->whereRaw('reviews.user_id = users.id');
    })
    ->get();

$Query = DB::table('users')->skip(30)->take(10)->get(); //Page 4

$Count = DB::table('users')
        ->where('active', true)
        ->count();

$highestCost = DB::table('orders')->max('amount');

$averageCost = DB::table('orders')
                ->where('status', 'completed')
                ->avg('amount');

$Query = DB::table('users')
        ->join('contacts', 'users.id', '=', 'contacts.user_id')
        ->select('users.*', 'contacts.name', 'contacts.status')
        ->get();

$Query = DB::table('users')
        ->join('contacts', function ($join) {
            $join
                ->on('users.id', '=', 'contacts.user_id')
                ->orOn('users.id', '=', 'contacts.proxy_user_id');
        })
        ->get();

$SubQuery = DB::table('contacts')->whereNull('first_name');
        
$Query = DB::table('contacts')
        ->whereNull('last_name')
        ->union($SubQuery)
        ->get();

$id = DB::table('contacts')->insertGetId([
        'name' => 'bala',
        'email' => 'balakarthikeya@gmail.com',
    ]);

$Query = DB::table('contacts')->insert([
            [
                'name' => 'Tamika Johnson',
                'email' => 'tamikaj@gmail.com'
            ],
            [
                'name' => 'Jim Patterson',
                'email' => 'james.patterson@hotmail.com'
            ],
        ]);

$Query = DB::table('contacts')
        ->where('points', '>', 100)
        ->update(
            [
                'status' => 'vip'
            ]
        );

$Query = DB::table('contacts')->increment('tokens', 5);
$Query = DB::table('contacts')->decrement('tokens', 5);

$Query = DB::table('users')
        ->where('last_login', '<', Carbon::now()->subYear())
        ->delete();

$Query = DB::table('contacts')->truncate();

Log::debug(DB::getQueryLog());

DB::listen(function($sql, $bindings, $time) {
    var_dump($sql);
    var_dump($bindings);
    var_dump($time);
}); 
```
## Macroable

Allow Laravel's macros to be created by using the Illuminate\Support\Traits\Macroable trait. 
Here are some of the most commonly used classes to create macros :

- Request: Illuminate\Http\Request
- Response: Illuminate\Http\Response
- Collection: Illuminate\Support\Collection
- Str: Illuminate\Support\Str
- Router: Illuminate\Routing\Router
- UrlGenerator: Illuminate\Routing\UrlGenerator
- Cache: Illuminate\Cache\Repository
- Filesystem: Illuminate\Filesystem\Filesystem
- Arr: Illuminate\Support\Arr
- Rule: Illuminate\Validation\Rule

## Gate & Policy
#### Gate
`Route::get('/posts/delete', 'PostController@delete')->middleware('can:delete')->name('post.delete');`

#### Route
```
Route::put('/post/{post}', function (Post $post) {
    // The current user may update the post...
})->middleware('can:update,post');
```

#### Policy
```
if ($user->can('update', $post)) {
    //user is authorized now
}
```
#### Blade directive
```
@can('update', $post)
    
@elsecan('create', App\Post::class)
    
@endcan
```
#### Policy Methods
```
Controller      	   Policy
index	               viewAny
show	               view
create	               create
store	               create
edit	               update
update	               update
destroy	               delete
```
## Roles & Premissions
```
php artisan make:migration create_permissions_table --create=permissions
php artisan make:migration create_roles_table --create=roles
php artisan make:migration create_user_permissions_table --create=user_permissions
php artisan make:migration create_user_roles_table --create=user_roles
php artisan make:migration create_role_permissions_table --create=role_permissions
```
#### FOREIGN KEY CONSTRAINTS
###### Query Builder
    $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
###### Query
    ALTER TABLE `posts` ADD CONSTRAINT `user_id` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON UPDATE CASCADE ON DELETE CASCADE;
## What are Notifications?
Laravel provides mailables which can be used to send emails, these emails are long with custom markup styling but we can not be sent through different channels. Laravel Notifications solves our this problem, by providing an elegant and fun way to send notifications to users.

### Laravel Notifications Channels
The whole beauty of Laravel Notifications is that it allows you to choose from different notifications channels through which your notifications will be delivered. Let’s go through the different notifications channels currently supported by Laravel Framework.

- **Mail :** The notifications will be send in the form of email to users.
- **SMS :** Users will revieve notifications on their mobile phones.
- **Database : This option allows you to save notifications into the database, which you can show to users in your own way.
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
## Routes
> Return with dynamic data for route like `Route::get('post/{id}', 'PostController@edit')-name('post.edit');`
```
return redirect()->route('post.edit', ['id' => $post->id]);
return redirect()->route('post.edit', $post->id);
```
> With return message
` return redirect()->back()->with('success', 'Post saved successfully.');`

> With more Response
```
$response = [
    'success'   =>  'Post saved successfully.',
    'post_id'   =>  $post->id,
];
return redirect()->back()->with($response);
```
> Controller action
```
return redirect()->action('PostController@create);
return response()->action('PostController@edit', ['id' => $post->id]);
```
