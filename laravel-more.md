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
## Blade directives:

```
@include pulls in information from the files in the includes directory.
@yield pulls in information from the content section of the home page.
@extends tells Laravel that the contents of this file extend another view. It also defines the view that it's extending as our default template: resources/views/layouts/default.blade.php.
@section defines the beginning of a section, which we have named content. Everything contained in this section will appear in the @yield which we defined in the template.
@endsection closes that section. 
@section and @endsection are terms which must be used in pairs, much like other HTML tags (for example, <div> and </div>).
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
