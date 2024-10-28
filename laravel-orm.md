## Eloquent
Laravel has an object-relational mapper (ORM) named “Eloquent” which allows you to work with your database. Eloquent is a new technique to work with the database queries using the model in Laravel. Eloquent provides simple and beautiful syntax to gain complex queries in a few seconds without writing long queries.

## COMMON ORM

1. `get():` Fetch All Matching Records
Imagine you have a database table of products, and you want to retrieve all the products that are currently available. You can use the get() method:

> $availableProducts = Product::where('status', 'available')->get();

2. `first():` Retrieve the First Matching Record
If you only need the first record that matches your criteria, use the first() method:

> $newestProduct = Product::orderBy('created_at', 'desc')->first();

3. `find($id):` Get a Record by Its Primary Key
If you know the primary key (usually ‘id’) of a specific record, you can retrieve it using the find() method:

> $product = Product::find(1); // Retrieve the product with ID 1

4. `pluck($column):` Retrieve a Specific Column
Sometimes, you may only need one value from a record. The pluck() method is handy for this:

> $productName = Product::where('id', 1)->pluck('name');

5. `count():` Count the Number of Matching Records
To determine how many records match your query, use the count() method:

> $productCount = Product::where('category', 'electronics')->count();

6. `where():` and `orWhere():` Filter Records
When you want to filter records based on specific conditions, you can use the where() and orWhere() methods:

> $expensiveProducts = Product::where('price', '>', 100)->orWhere('discounted', true)->get();

7. `orderBy($column, $direction):` Sort Records
To order the results, you can use the orderBy() method:

> $sortedProducts = Product::orderBy('price', 'asc')->get();

8. `groupBy($column):` Group Records
Suppose you have a table of orders, and you want to group them by the shipping country. Use the groupBy() method:

> $orderCountsByCountry = Order::groupBy('shipping_country')->get();

9. `select($columns):` Choose Columns to Retrieve
If you only need specific columns from your records, you can use the select() method:

> $productNamesAndPrices = Product::select('name', 'price')->get();

10. `distinct():` Retrieve Distinct Records
When you want unique records, use the distinct() method. It’s especially useful with select():

> $uniqueCategories = Product::select('category')->distinct()->get();

## Relationship
A relationship, in the context of the database, is a relation or link between two tables via primary key and reference key. One table has a foreign key that references the primary key of another table. Relationships allow relational databases to split and store data in different tables.

> Types of Eloquent Relationships in Laravel
- One To One
- One To Many
- Many To Many
- HasMany Through
- Polymorphic Relation
- Many To Many Polymorphic

> One to One Relationship with Example
```php
class Post extends Model
{
    public function post_content()
    {
        return $this->hasOne('App\Content');
    }
}
```
> Inverse of One to One Relationship Example
```php
class Content extends Model
{
    public function post()
    {
        return $this->belongsTo('App\Post');
    }
}
```
> Usage
```php
$content = Post::find(10)->post_content;
```
> One to Many Relationship Example
```php
class Author extends Model
{
    public function post()
    {
        return $this->hasMany('App\Post');
    }
}
```
> Usage
```php
$posts = App\Author::find(10)->post()->get();
```
> Inverse One to Many Relationship Example
```php
class Post extends Model
{
    public function author()
    {
        return $this->belongsTo('App\Author');
    }
}
```
> Usage
```php
$post->author->author_name;
```
> Many to Many Relationship Example
```php
class Post extends Model
{
    public function tags()
    {
        return $this->belongsToMany('App\Tag');
    }
}
```
> Usage
```php
$post = App\Post::find(8);
```
> Inverse of Many to Many Relationship Example
```php
class Tag extends Model
{
    public function posts()
    {
        return $this->belongsToMany('App\Post');
    }
}
```
> Usage
```php
$tag = App\Tag::find(8);
```
## Common Functions on Eloquent ORM
> Select
```php
  ->select('col1','col2')
  ->select(array('col1','col2'))
  ->select(DB::raw('products.*, COUNT(reviews.id) as no_of_ratings, IFNULL(sum(reviews.score),0) as rating'))  
  ->addSelect('col3','col4')
  ->distinct() // distinct select
```
> From
```php
  ->from('table')
  ->from(DB::raw('table, (select @n :=0) dummy'))
  ->from(DB::raw("({$subQuery->toSql()}) T ")->mergeBindings($subQuery->getQuery())
 ```
> Where
```php
  ->where('column','value')
  ->where('column','LIKE','%'.$value.'%')
  ->where(function ($query) { $query->where('a', '=', 1)->orWhere('b', '=', 1); })
  ->orWhere('column','!=', 'value')
 
  ->whereRaw('age > ? and votes = 100', array(25))
  ->whereRaw(DB::raw("id in (select city_id from cities GROUP BY cities.city_id)"))

  ->whereExists(function($query){ 
    $query->select(DB::raw(1))
    ->from('languages')
    ->whereRaw('languages.language_id = content.language_id')
    ->groupBy('languages.language_id')
    ->havingRaw("COUNT(*) > 0");
  })
  ->orWhereExists()
  ->whereNotExists()
  ->orWhereNotExists()

  ->whereIn('column',[1,2,3])
  ->orWhereIn()
  ->whereNotIn('id', function($query){
    $query->select('city_id')->from('cities')->groupBy('cities.city_id');
  })
  ->whereNotIn()
  ->orWhereNotIn()

  ->whereNull('column') //where `column` is null
  ->orWhereNull('column') //or where `column` is null
  ->whereNotNull('column')  //where `column` is not null 
  ->orWhereNotNull('column')  //or where `column` is not null 

  ->whereDay()
  ->whereMonth('column', '=', 1) 
  ->whereYear('column', '>', 2000) //uses sql YEAR() function on 'column'
  ->whereDate('column', '>', '2000-01-01')
```
> Joins
```php
  ->join('business_category','business_category.business_id','=','businesses.id')
  ->leftJoin('reviews','reviews.business_id', '=', 'businesses.id')
  ->join('business_category',function($join) use($cats) {
    $join->on('business_category.business_id', '=', 'businesses.id')->on('business_category.id', '=', $cats, 'and', true);
  })
  ->join(DB::raw('(SELECT *, ROUND(AVG(rating),2) avg FROM reviews WHERE rating!=0 GROUP BY item_id ) T' ), function($join){
    $join->on('genre_relation.movie_id', '=', 'T.id')
  })
```
> Eager Loading
```php
  ->with('table1','table2')
  ->with(array('table1','table2','table1.nestedtable3'))
  ->with(array('posts' => function($query) use($name){
    $query->where('title', 'like', '%'.$name.'%')->orderBy('created_at', 'desc');
  }))
```  
> Grouping
```php
  ->groupBy('state_id','locality')
  ->havingRaw('count > 1 ')
  ->having('items.name','LIKE',"%$keyword%")
  ->orHavingRaw('brand LIKE ?',array("%$keyword%"))
```
> Cache
```php
  ->remember($minutes)
  ->rememberForever()
```  
> Offset & Limit
```php
  ->take(10)
  ->limit(10)
  ->skip(10)
  ->offset(10)
  ->forPage($pageNo, $perPage)
```  
> Order
```php
  ->orderBy('id','DESC')
  ->orderBy(DB::raw('RAND()'))
  ->orderByRaw('type = ? , type = ? ', array('published','draft'))
  ->latest() // on 'created_at' column
  ->latest('column')
  ->oldest() // on 'created_at' column
  ->oldest('column')
```  
> Create
```php
  ->insert(array('email' => 'john@example.com', 'votes' => 0))
  ->insert(array(   
    array('email' => 'taylor@example.com', 'votes' => 0),
    array('email' => 'dayle@example.com', 'votes' => 0)
  )) //batch insert
  ->insertGetId(array('email' => 'john@example.com', 'votes' => 0)) //insert and return id
```  
> Update
```php
  ->update(array('email' => 'john@example.com'))
  ->update(array('column' => DB::raw('NULL')))
  ->increment('column')
  ->decrement('column')
  ->touch() //update timestamp
```  
> Delete
```php
  ->delete()
  ->forceDelete() // when softdeletes enabled
  ->destroy($ids) // delete by array of primary keys
  ->roles()->detach() //delete from pivot table: associated by 'belongsToMany'
```  
> Getters
```php
  ->find($id)
  ->find($id, array('col1','col2'))
  ->findOrFail($id)
  ->findMany($ids, $columns)
  ->first(array('col1','col2'))
  ->firstOrFail()
  ->all()
  ->get()
  ->get(array('col1','col2')) 
  ->getFresh() // no caching
  ->getCached() // get cached result
  ->chunk(1000, function($rows){
      $rows->each(function($row){
        ...
      });
  })
  ->lists('column') // numeric index
  ->lists('column','id') // 'id' column as index
  ->lists('column')->implode('column', ',') // comma separated values of a column
  ->pluck('column')  //Pluck a single column's value from the first result of a query.
  ->value('column')  //Get a single column's value from the first result of a query.
```
> Paginated results
```php
  ->paginate(10)
  ->paginate(10, array('col1','col2'))
  ->simplePaginate(10)
  ->getPaginationCount() //get total no of records
```  
> Aggregate
```php
  ->count()
  ->count('column')
  ->count(DB::raw('distinct column'))
  ->max('rating')
  ->min('rating')
  ->sum('rating')
  ->avg('rating')
  ->aggregate('sum', array('rating')) // use of aggregate functions
```  
> Others
```php
  ->toSql() // output sql query
  ->exists() // check if any row exists
  ->fresh() // Return a fresh data for current model from database
```  
> Object methods
```php
  ->toArray()
  ->toJson()
  ->relationsToArray() //Get the model's relationships in array form.
  ->implode('column', ',') // comma separated values of a column
  ->isDirty()
  ->getDirty() //Get the attributes that have been changed but not saved to DB
```  
> Debugging
```php
DB::enableQueryLog();
DB::getQueryLog();
Model::where()->toSql() // output sql query
```
## Examples
```php
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
        ->join('profiles', 'users.id', '=', 'profiles.user_id')
        ->select('users.*', 'profiles.username', 'profiles.status')
        ->get();

$Query = DB::table('users')
        ->join('profiles', function ($join) {
            $join->on('users.id', '=', 'profiles.user_id')->orOn('profiles.status', '=', 'vip');
        })
        ->get();

$SubQuery = DB::table('profiles')->whereNull('first_name');
        
$Query = DB::table('profiles')
        ->whereNull('last_name')
        ->union($SubQuery)
        ->get();

$id = DB::table('profiles')->insertGetId([
        'username' => 'bala',
        'dob' => '1989-01-07',
]);

$Query = Users::whereBetween('created_at', [$firstDate, $secondDate])->get();

$Query = DB::table('profiles')->insert([
            [
                'username' => 'balakarthikeya',
                'status' => 'free'
            ],
            [
                'username' => 'ramesh',
                'status' => 'vip'
            ],
        ]);

$Query = DB::table('profiles')
        ->where('points', '>', 100)
        ->update(
            [
                'status' => 'vip'
            ]
        );

$Query = DB::table('profiles')->increment('points', 5);
$Query = DB::table('profiles')->decrement('points', 5);

$Query = DB::table('users')
        ->where('last_login', '<', Carbon::now()->subYear())
        ->delete();

$Query = DB::table('profiles')->truncate();

Log::debug(DB::getQueryLog());

DB::listen(function($sql, $bindings, $time) {
    var_dump($sql);
    var_dump($bindings);
    var_dump($time);
}); 
```
## FOREIGN KEY CONSTRAINTS
> Query Builder
```php
$table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
```
> Query
```sql
ALTER TABLE `posts` ADD CONSTRAINT `user_id` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON UPDATE CASCADE ON DELETE CASCADE;
```

## SQL Joins with Laravel Eloquent

### INNER JOIN
An INNER JOIN returns only the rows that have matching values in both tables.
```php
// Models
class User extends Model {
    public function posts() {
        return $this->hasMany(Post::class);
    }
}

class Post extends Model {
    public function user() {
        return $this->belongsTo(User::class);
    }
}

// Controller or any place you want to use it
$users = User::has('posts')->with('posts')->get();

$usersWithOrders = DB::table('users')
    ->join('orders', 'users.id', '=', 'orders.user_id')
    ->select('users.name', 'orders.order_date')
    ->get();
```

### LEFT JOIN
A LEFT JOIN retrieves all records from the left table and the matched records from the right table. If no match is found, NULLs are returned for columns from the right table.
```php
$users = User::with('posts')->get();

$usersWithOrders = DB::table('users')
    ->leftJoin('orders', 'users.id', '=', 'orders.user_id')
    ->select('users.name', 'orders.order_date')
    ->get();
```

### RIGHT JOIN
A RIGHT JOIN retrieves all records from the right table and the matched records from the left table. While Laravel Eloquent doesn’t natively support RIGHT JOIN, you can achieve the same effect using raw queries or the query builder.
```php
$posts = Post::rightJoin('users', 'posts.user_id', '=', 'users.id')
             ->select('posts.*', 'users.name as user_name')
             ->get();

$ordersWithUsers = DB::table('orders')
    ->rightJoin('users', 'users.id', '=', 'orders.user_id')
    ->select('users.name', 'orders.order_date')
    ->get();
```

### FULL JOIN
A FULL JOIN retrieves all records when there is a match in either the left or right table. Eloquent doesn’t directly support FULL JOIN, but you can simulate it using a union of LEFT JOIN and RIGHT JOIN.
```php
$leftJoin = User::leftJoin('posts', 'users.id', '=', 'posts.user_id')
                ->select('users.*', 'posts.title as post_title');

// "select `users`.*, `posts`.`title` as `post_title` from `users` left join `posts` on `users`.`id` = `posts`.`user_id`"

$rightJoin = Post::rightJoin('users', 'posts.user_id', '=', 'users.id')
                 ->select('users.*', 'posts.title as post_title');

// "select `users`.*, `posts`.`title` as `post_title` from `posts` right join `users` on `posts`.`user_id` = `users`.`id`"

$fullJoin = $leftJoin->union($rightJoin)->get();

$fullOuterJoin = DB::table('users')
    ->leftJoin('orders', 'users.id', '=', 'orders.user_id')
    ->select('users.name', 'orders.order_date')
    ->union(
        DB::table('users')
            ->rightJoin('orders', 'users.id', '=', 'orders.user_id')
            ->select('users.name', 'orders.order_date')
    )
    ->get();
```

## Procedure with Laravel ?
```sql
DROP PROCEDURE IF EXISTS `get_subcategory_by_catid`;
delimiter \\
CREATE PROCEDURE `get_subcategory_by_catid` (IN idx int)
BEGIN
SELECT id, parent_id, title, slug, created_at FROM category WHERE parent_id = idx AND status = 1 ORDER BY title;
END
\\
delimiter ;
```
Call the procedure 
```php
$getSubCategories = DB::select(
   'CALL get_subcategory_by_catid('.$item->category_id.')'
);
```