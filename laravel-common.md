## Roles & Premissions
```bash
php artisan make:migration create_permissions_table --create=permissions
php artisan make:migration create_roles_table --create=roles
php artisan make:migration create_user_permissions_table --create=user_permissions
php artisan make:migration create_user_roles_table --create=user_roles
php artisan make:migration create_role_permissions_table --create=role_permissions
```

## Gate & Policy
> Usage of Gate in Routes
```php
Route::get('/posts/delete', 'PostController@delete')->middleware('can:delete')->name('post.delete');
```

> Usage of Gate in Routes
```php
Route::put('/post/{post}', function (Post $post) {
    // The current user may update the post...
})->middleware('can:update,post');
```

> Usage of Policy in Model
```php
if ($user->can('update', $post)) {
    //user is authorized now
}
```

> Usage of Policy in Blade directive
```php
@can('update', $post)
    //
@elsecan('create', App\Post::class)
    //
@endcan
```

## Policy Methods
```
Controller              Policy
index                   viewAny
show                    view
create                  create
store                   create
edit                    update
update                  update
destroy                 delete
```

## Routes
> Return with dynamic data for route like `Route::get('post/{id}', 'PostController@edit')->name('post.edit');`
```php
return redirect()->route('post.edit', ['id' => $post->id]);
return redirect()->route('post.edit', $post->id);
```

> With return message
```php
return redirect()->back()->with('success', 'Post saved successfully.');
```

> With more Response
```php
$response = [
    'success'   =>  'Post saved successfully.',
    'post_id'   =>  $post->id,
];
return redirect()->back()->with($response);
```

> Controller action
```php
return redirect()->action('PostController@create');
return response()->action('PostController@edit', ['id' => $post->id]);
```

## Sessions
> Adding Value to Session Array
```php
$request->session()->put('my_session', $request->all());
```

> Regenerate session IDs
```php
echo "<pre>Session ID : ".$request->session()->regenerate()."</pre>";  
```

> Retrieve Session Values 
```php
echo "<pre>Added to Session : <br/>"; print_r($request->session()->get('my_session')); echo "</pre>"; 
echo "<pre>From Global Session : <br/>"; print_r(session('my_session')); echo "</pre>";
```

> Specifying a default value
```php
$request->session()->get('my_session.test-consent', false);

if (!$request->session()->has('my_session.test-consent')) {
    // Pushing To Array Session Values
    $request->session()->push('my_session.test-consent', true);
}
```

> Retrieving & Deleting An Item
```php
$request->session()->pull('my_session.test-consent', true);
```

> Remove an item from the session
```php
$request->session()->forget('my_session.test-consent');
```

> Store items in the session only for the next request.
```php
$request->session()->flash('status', 'Added successfully!');
```

> Remove all values from session
```php
$request->session()->flush();
```

> Retrieving All Session Data
```php
$request->session()->all();
```

## Views
```php
return View::make('pages.index')->with(compact('content', 'session', 'message'));
```    
