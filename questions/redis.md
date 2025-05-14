## Topics
- What is Redis
- Installation and Setup
- Basic Data Types in Redis
- Strings
- Hashes
- Lists
- Sets
- Sorted Sets
- Pub/Sub Messaging
- Redis Persistence
- Redis Streams

### What is Redis?
Redis (Remote Dictionary Server) is an open-source, in-memory data structure store, widely used as a database, cache, message broker, and queue. Unlike traditional databases that store data on disk, Redis operates primarily in memory, which gives it the ability to provide sub-millisecond response times. This makes Redis a perfect fit for use cases where speed is paramount, such as real-time analytics, leaderboards, session management, and more.

Redis is a NoSQL database and its main difference from other NoSQL databases is that it works with key-value logic. We can think of it as a simple array. Each key points to a value, which can often be data structures, arrays, lists, sets or more complex data. This key-value structure of Redis makes data storage and access extremely fast and efficient.

### Why should we use Redis?
Performance and Speed: Redis is extremely fast because it keeps data on RAM.
Various Data Structures: Redis supports various data structures such as simple key-value, list, set, hash, sorted set.
Persistence: Although Redis is considered a memory-only database, it offers the option to write to disk so that data can be stored permanently.

### Where can we use redis ?
- Caching operations
- Session Management
- Real Time Analysis
- Real Time Messaging
- Queuing and Task Management
- Working with Geographic Data
- Temporary Data Storage
- Session management

### Installation and Setup
```bash
sudo apt-get install redis-server

systemctl start redis.service 
systemctl enable redis.service

# to prevent common error added to /etc/rc.local just above exit 0 below lines 
sudo sh -c 'echo "vm.overcommit_memory=1" >> /etc/sysctl.conf'
sudo sh -c 'echo "net.core.somaxconn=65535" >> /etc/sysctl.conf'
sudo sh -c 'echo "never" > /sys/kernel/mm/transparent_hugepage/enabled'

sudo apt-get install php-redis

# Check running Redis 
sudo redis-server

# Flush all
redis-cli flushall

# Configure Redis as a Cache
sudo nano /etc/redis/redis.conf
maxmemory 256mb
maxmemory-policy allkeys-lru

wget https://assets.digitalocean.com/articles/wordpress_redis/object-cache.php
sudo mv object-cache.php /var/www/html/wp-content/

```

```php
define('NONCE_SALT',        'put your unique phrase here');
define('WP_CACHE_KEY_SALT', 'example.com');
define('WP_CACHE',          true);
```

```bash
sudo service redis-server restart
sudo service apache2 restart
sudo service php5-fpm restart 

sudo touch /etc/php5/mods-available/phpiredis.ini
sudo sh -c 'echo "extension=phpiredis.so" > /etc/php5/mods-available/phpiredis.ini'
sudo php5enmod phpiredis
sudo service php5-fpm restart

composer require predis/predis
```

```php
require "predis/autoload.php";
PredisAutoloader::register();

try {
	$redis = new PredisClient();

	// This connection is for a remote server
	/*
		$redis = new PredisClient(array(
		    "scheme" => "tcp",
		    "host" => "153.202.124.2",
		    "port" => 6379
		));
	*/
}
catch (Exception $e) {
	die($e->getMessage());
}

$redis = new Client();
// or
$redis = new Client('tcp://127.0.0.1', [
    'connections' => [
        'tcp'  => 'Predis\Connection\PhpiredisStreamConnection',
        'unix' => 'Predis\Connection\PhpiredisSocketConnection',
    ],
]);
$hash = md5($_SERVER['QUERY_STRING']);
if (!$redis->get($hash . '-results')) {
    $redis->set($hash . '-results', serialize($results));
    $redis->expire($hash . '-results', 86400);
}
$results = unserialize($redis->get($hash . '-results'));
```

### SET, GET, and EXISTS

```php
$redis->set("hello_world", "Hi from php!");
$value = $redis->get("hello_world");
echo ($redis->exists("Santa Claus")) ? "true" : "false";
```

### INCR (INCRBY) and DECR (DECRBY)

```php
// increment the number of views by 1
$redis->incr("article_views_234");
// increment views for article 237 by 5
$redis->incrby("article_views_237", 5);
// decrement views for article 237
$redis->decr("article_views_237");
// decrement views for article 237 by 3
$redis->decrby("article_views_237", 3);
```

## Redis Data Types

- `String` - the basic data type used in Redis in which you can store from few characters to the content of an entire file.
- `List` - a simple list of strings order by the insertion of its elements. You can add and remove elements from both the list’s head and tail, so you can use this data type to implement queues.
- `Hash` - a map of string keys and string values. In this way you can represent objects (think of it as a one-level deep JSON object).
- `Set` - an unordered collection of strings where you can add, remove, and test for existence of members. The one constraint is that you are not allowed to have repeated members.
- `Sorted set` - a particular case of the set data type. The difference is that every member has and associated score that is used order the set from the smallest to the greatest score.

### HSET, HGET and HGETALL, HINCRBY, and HDEL

- HSET - sets the value for a key on the the hash object.
- HGET - gets the value for a key on the hash object.
- HINCRBY - increment the value for a key of the hash object with a specified value.
- HDEL - remove a key from the object.
- HGETALL - get all keys and data for a object.

```php
$redis->hset("taxi_car", "brand", "Toyota");
$redis->hset("taxi_car", "model", "Yaris");
$redis->hset("taxi_car", "license number", "RO-01-PHP");
$redis->hset("taxi_car", "year of fabrication", 2010);
$redis->hset("taxi_car", "nr_starts", 0);
/*
$redis->hmset("taxi_car", array(
    "brand" => "Toyota",
    "model" => "Yaris",
    "license number" => "RO-01-PHP",
    "year of fabrication" => 2010,
    "nr_stats" => 0)
);
*/
echo "License number: " .  $redis->hget("taxi_car", "license number") . "<br>";

// remove license number
$redis->hdel("taxi_car", "license number");
$redis->del("taxi_car", "license number");

// increment number of starts
$redis->hincrby("taxi_car", "nr_starts", 1);

$taxi_car = $redis->hgetall("taxi_car");
echo "All info about taxi car";
echo "<pre>";var_dump($taxi_car);echo "</pre>";

// Example
$key = "countries";
$redis->sadd($key, 'china');
$redis->sadd($key, ['england', 'france', 'germany']);
$redis->sadd($key, 'china'); // this entry is ignored
$redis->srem($key, ['england', 'china']);
$redis->sismember($key, 'england'); // false
$redis->smembers($key); // ['france', 'germany']
```

### LPUSH, RPUSH, LPOP, RPOP, LLEN, LRANGE

- `LPUSH` - prepends element(s) to a list.
- `RPUSH` - appends element(s) to a list.
- `LPOP` - removes and retrieves the first element of a list.
- `RPOP` - removes and retrieves the last element of a list.
- `LLEN` - gets the length of a list.
- `LRANGE` - gets elements from a list.

```php
$list = "PHP Frameworks List";
$redis->rpush($list, "Symfony 2");
$redis->rpush($list, "Symfony 1.4");
$redis->lpush($list, "Zend Framework");

echo "Number of frameworks in list: " . $redis->llen($list) . "<br>";

$arList = $redis->lrange($list, 0, -1);
echo "<pre>"; print_r($arList); echo "</pre>";

// the last entry in the list
echo $redis->rpop($list) . "<br>";

// the first entry in the list
echo $redis->lpop($list) . "<br>";
```

### EXPIRE , EXPIREAT , TTL, and PERSIST

- `EXPIRE` - sets an expiration timeout (in seconds) for a key after which it and its value will be deleted.
- `EXPIREAT` - sets and expiration time using a unix timestamp that represents when the key and value will be deleted.
- `TTL` - gets the remaining time left to live for a key with an expiration.
- `PERSIST` - removes the expiration on the given key.

```php
// set the expiration for next week
$redis->set("expire in 1 week", "I have data for a week");
$redis->expireat("expire in 1 week", strtotime("+1 week"));
$ttl = $redis->ttl("expire in 1 week"); // will be 604800 seconds

// set the expiration for one hour
$redis->set("expire in 1 hour", "I have data for an hour");
$redis->expire("expire in 1 hour", 3600);
$ttl = $redis->ttl("expire in 1 hour"); // will be 3600 seconds

// never expires
$redis->set("never expire", "I want to leave forever!");
$redis->persist($key);

// Example
$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$redis->auth('REDIS_PASSWORD');
if (!$redis->get($key)) {
    $source = 'MySQL Server';
    $database_name     = 'test_store';
    $database_user     = 'test_user';
    $database_password = 'PASSWORD';
    $mysql_host        = 'localhost';

    $pdo = new PDO('mysql:host=' . $mysql_host . '; dbname=' . $database_name, $database_user, $database_password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

    $sql  = "SELECT * FROM products";
    $stmt = $pdo->prepare($sql);
    $stmt->execute();

    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
       $products[] = $row;
    }

    $redis->set($key, serialize($products));
    $redis->expire($key, 10);

} else {
     $source = 'Redis Server';
     $products = unserialize($redis->get($key));

}

echo $source . ': <br>';
print_r($products);
```