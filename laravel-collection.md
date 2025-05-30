## Laravel's Collection
Laravel's Collection `Illuminate\Support\Collection` class, provides some static functions which we can use to create new collection instances.

### make()
make() method create a new instance of a collection from an array
> Method
```php
/**
* Create a new collection instance if the value isn't one already.
*
* @param  mixed  $items
* @return static
*/
public static function make($items = [])
{
   return new static($items);
}
```
> Example
```php
$languages = [
    'en'    =>  'English',
    'fr'    =>  'French',
    'de'    =>  'German',
    'sp'    =>  'Spanish'
];
use Illuminate\Support\Collection;
$collection = Collection::make($languages);
$isCollection = $collection instanceof Collection; // true
```

### wrap()
wrap() method will create a collection from any value supplied to it
> Method
```php
/**
 * Wrap the given value in a collection if applicable.
 *
 * @param  mixed  $value
 * @return static
 */
public static function wrap($value)
{
    return $value instanceof self
        ? new static($value)
        : new static(Arr::wrap($value));
}
```
> Example
```php
use App\Product;
use Illuminate\Support\Collection;
$product = new Product();
$products = Collection::wrap($product);
```

### unwrap()
> Method
```php
/**
 * Get the underlying items from the given collection if applicable.
 *
 * @param  array|static  $value
 * @return array
 */
public static function unwrap($value)
{
    return $value instanceof self ? $value->all() : $value;
}
```
> Example
```php
use Illuminate\Support\Collection;
// Create a new collection
$users = Collection(['London', 'Paris', 'Berlin', 'Moscow', 'Madrid']);
// Get the collection items
$items = Collection::unwrap($users);
```

### map()
The map method applies a given callback function to each element of a collection. The callback function will modify the item and create a new Laravel collection for them.
> Method
```php
/**
 * Run a map over each of the items.
 *
 * @param  callable  $callback
 * @return static
 */
public function map(callable $callback)
{
    $keys = array_keys($this->items);
    $items = array_map($callback, $this->items, $keys);
    return new static(array_combine($keys, $items));
}
```
> Example
```php
use Illuminate\Support\Collection;
// Create a new collection
$collection = new Collection(['London', 'Paris', 'Berlin', 'Moscow', 'Madrid']);
// Change all items to uppercase and create a new collection of them
$names = $collection->map(function($item, $key) {
   return strtoupper($item);
});
```

### times()
The times() method of Laravel Collection is similar to PHP's range() function with some extra functionality for Collections.
> Method
```php
/**
 * Create a new collection by invoking the callback a given amount of times.
 *
 * @param  int  $number
 * @param  callable  $callback
 * @return static
 */
public static function times($number, callable $callback = null)
{
    if ($number < 1) {
        return new static;
    }
    if (is_null($callback)) {
        return new static(range(1, $number));
    }
    return (new static(range(1, $number)))->map($callback);
}
```
> Example
```php
use Illuminate\Support\Collection;
$nextWeek = Collection::times(7, function ($index) {
    return \Carbon\Carbon::now()->addDay($index);
});
```

### collapse()
The collapse() method can be used to convert a multi-dimensional collection into a single dimension collection.
> Method
```php
/**
 * Collapse the collection of items into a single array.
 *
 * @return static
 */
public function collapse()
{
    return new static(Arr::collapse($this->items));
}
```
> Example
```php
use Illuminate\Support\Collection;
$cities1 = ['London', 'Paris', 'Berlin', 'Moscow', 'Madrid'];
$cities2 = ['New York', 'Toranto', 'San Jose', 'Tokyo', 'Sydney'];
$collection = new Collection([$cities1, $cities2]);
$collapsed = $collection->collapse();
```

### contains()
The contains() method determines if a given key exist in the collection. You can also provide the value to check against the key as a second argument, which will check if the given key/value pair exists in the collection.
> Example
```php
use App\Product;
use Illuminate\Support\Collection;
// Create a new collection with given values
$collection = new Collection(['London', 'Paris', 'Berlin', 'Moscow', 'Madrid']);
$collection->contains('Paris'); // true
$collection->contains('Manchester'); // false
$languages = [
    'en'    =>  'English',
    'fr'    =>  'French',
    'de'    =>  'German',
    'sp'    =>  'Spanish'
];
$collection = new Collection($languages);
$collection->contains('en', 'English'); // true
$collection->contains('ar', 'Arabic'); // false
// Create a new collection instance.
$collection = new Collection([
    new Product('MacBook Air', 1499),
    new Product('Lenovo ThinkPad', 799)
]);
$collection->contains('name', 'Lenovo ThinkPad'); // true
$collection->contains(function($key, $value) {
    return $value->price > 1000;
}); // true
```

### containsStrict()
The containsStrict() method test the value based on a stric comparison operator.
> Example
```php
$collection = new Collection(['99.99', '149.99', '199.99']);
$collection->containsStrict(149.99); // false
$collection->containsStrict('99.99'); // true
```

### each()
The each() method of Laravel Collection takes a callback function and perform it on each item of the collection.
> Example
```php
use Illuminate\Support\Collection;
$cities = new Collection(['London', 'Paris', 'New York', 'Toranto', 'Tokyo']);
$cities->each(function($item, $key) {
    // Print each city name
    echo $item;
});
```

### toJson()
The toJson() method will return the JSON encoded version of the data stored in a Laravel Collection
> Method
```php
/**
 * Get the collection of items as JSON.
 *
 * @param  int  $options
 * @return string
 */
public function toJson($options = 0)
{
    return json_encode($this->jsonSerialize(), $options);
}
```
> Example
```php
use Illuminate\Support\Collection;
// Create a new collection for our example
$collection = new Collection([
    ['name' =>  'iPhone 11 Pro', 'price'    =>  999],
    ['name' =>  'iPhone 11', 'price'    =>  799],
]);
// Convert the above collection to JSON format
$jsonValue = $collection->toJson();
$jsonValue = $collection->toJson(JSON_PRETTY_PRINT);
```

### where()
Laravel Collection provides the ability to filter a collection's items by using a key value pair by using where() method.
> Example
```php
use Illuminate\Support\Collection;
// Create a new collection for our example
$collection = new Collection([
    ['name' =>  'iPhone 11 Pro', 'price'    =>  999],
    ['name' =>  'iPhone 11', 'price'    =>  799],
    ['name' =>  'iPhone 5s', 'price'    =>  500],
]);
$filteredCollection = $collection->where('price', '>', 800);
$filteredCollection = $collection->where('name', 'iPhone 5s');
```

### splice()
Laravel Collection splice method is very useful when you want to remove a portion of a collection and return the removed section. This method takes three parameters: $offset, $length and $replacement. The $offset dictates the splice method where to begin when removing the items from a collection.
> Example
```php
$collection = new Collection([0,1,2,3,4,5,6,7,8,9]);
// Splice the $collection from 5th index, which is 4
$spliced = $collection->splice(5);
$spliced = $collection->splice(2,3);
```

### chunk()
If you want to split your collection into equal parts, then you can use chunk() method.
> Example
```php
$products = Product::all();
$chunk = $products->chunk(2);
dd($chunk->toArray());
```

### every()
The every() method will check every element of the collection for a given condition. 
> Example
```php
$every = $products->every(function ($item, $key){
    return $item['price'] > 100;
});
dd($every); // true
```

### flip()
The flip() method swap the collection keys with their values.
> Example
```php
$products = collect(['name' => 'iPhone', 'category' => 'phone']);
$flipped = $products->flip();
dd($flipped);
```

### filter()
The filter() method will filter the collection with a given callback function and keep items in the collection which pass the callback test.
> Example
```php
$products = Product::all();
$filtered = $products->filter(function ($item, $key) {
    return $item['price'] > 300;
});
dd($filtered);
```

### forget()
The forget() method will remove an item from the collection by a given key.
> Example
```php
$products = collect(['name' => 'iPhone', 'category' => 'phone']);
$products = $products->forget('category');
dd($products);
```

### keyBy()
The keyBy() method keys the colllection with the given key.
> Example
```php
$products = Product::all();
$keyed = $products->keyBy('price');
dd($keyed);
```

### pluck()
The pluck() method will return the values of a given key.
> Example
```php
$products = Product::all();
$pluckedProducts = $products->pluck('name');
dd($pluckedProducts->all());
```

### isEmpty()
The isEmpty() method will return true if the given collection is empty, otherwise it will return false.
> Example
```php
$products = Product::all();
$products->isEmpty(); // false
```

### isNotEmpty()
The isNotEmpty() method will return true if the given collection is not empty, otherwise it will return false.
> Example
```php
$products = Product::all();
$products->isNotEmpty(); // true
```
### sortBy()
The sortBy() method will sort a collection by a given key. This method keeps the original keys, so we can use values() method to reset the keys.
> Example
```php
$products = Product::all();
$sorted = $products->sortBy('featured');
$sorted->values()->all();
```

### reject()
The reject() methos also filter the collection using a given callback function. The callback function should return true if you want to remove a particular item from the collection.
> Example
```php
$products = Product::all();
$filtered = $products->reject(function ($value, $key) {
    return $value['price'] > 300;
});
dd($filtered);
```
