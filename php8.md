# Topic 1: PHP Core Language Features (PHP 8.x Versioning)

### PHP 8.0 Features

* **Just-In-Time (JIT) Compilation:** Dynamic compilation engine integrated into OPcache to optimize execution speeds.
* **Named Arguments:** Pass arguments into a function by specifying the parameter name.
* **Union Types:** Declare that a variable, parameter, or return type can accept multiple types (e.g., `int|float`).
* **Constructor Property Promotion:** Define and assign class properties directly inside the constructor declaration.
* **Null-safe Operator (`?->`):** Short-circuit null checks instead of making chained explicit checks.
* **Trailing Comma in Parameters:** Allows trailing commas in function definition parameter lists and call arguments.
* **Match Expression:** A strict, expression-based alternative to the traditional `switch` statement.
* **Attributes:** Structured, native metadata declarations for classes, methods, and properties (replacing PHPDoc annotations).
* **WeakMaps:** Collections of references to objects that do not prevent those objects from being garbage collected.
* **`mixed` Type:** A built-in pseudo-type representing any value (`array|bool|callable|int|float|object|resource|string|null`).
* **Throw Exception From New Places:** `throw` converted into an expression, enabling throwing exceptions in arrow functions, ternary operators, etc.
* **Call `::class` on Objects:** Retrieve the class string directly from an instantiated variable (e.g., `$obj::class`).
* **Non-Capturing Catch:** Catch exceptions without assigning them to a variable if the variable isn't needed.
* **New String Functions:** Built-in functions like `str_contains()`, `str_starts_with()`, and `str_ends_with()`.

### PHP 8.1 Features

* **Enums:** First-class enumerations (both pure and backed enums).
* **Array Unpacking with String Keys:** Support for `...` unpacking on associative arrays with string keys.
* **Readonly Properties:** Immutable class properties that can only be written to once within their scope.
* **Intersection Types:** Require a value to satisfy multiple type constraints simultaneously (e.g., `Countable&Iterator`).
* **First-Class Callable Syntax:** Create closures using `Callable(...)` syntax.
* **`array_is_list()` Function:** Validates if an array contains continuous 0-indexed integer keys.
* **Final Class Constants:** Prevent child classes from overriding constant declarations.
* **`new` in Initializers:** Instantiate objects directly inside default parameter values, attributes, and initializers.
* **Named Arguments After Unpacking:** Pass named arguments after unpacking positional arrays.
* **Octal Literal Notation:** Explicit syntax for octal numbers using the `0o` prefix (e.g., `0o16`).

### PHP 8.2+ Features

* **Readonly Classes:** Declare an entire class as `readonly` (making all instance properties implicitly readonly).
* **Disjunctive Normal Form (DNF) Types:** Combining Union and Intersection types together (e.g., `(A&B)|C`).
* **Better Performance Optimizations:** Further JIT improvements and internal memory optimizations.

---

## Code Examples

```php
// -------------------------------------------------------------
// 1. NAMED ARGUMENTS (PHP 8.0)
// -------------------------------------------------------------
$numbers = [1, 2, 3, 4, 5, 6];

// Note: Passing named arguments to array_map
$squares = array_map(array: $numbers, callback: function($n) {
    return $n * $n;
});
echo '<pre>'; var_dump($squares); echo '</pre>';

$position = strpos(needle: "World", haystack: "Hello World");
echo '<pre>'; var_dump($position); echo '</pre>';

// -------------------------------------------------------------
// 2. UNION TYPES & CONSTRUCTOR PROPERTY PROMOTION (PHP 8.0)
// -------------------------------------------------------------
class SampleClass {
    /**
     * @var int|float
     */
    public int|float $number;
	
    // Constructor Property Promotion
    public function __construct(
        public string $firstname,
        public string $lastname,
        public int $age
    ){}
	
    public function setNumber(int|float $number): void
    {
        $this->number = $number;
    }

    public function getNumber(): int|float
    {
        return $this->number;
    }
}

// -------------------------------------------------------------
// 3. NULL-SAFE OPERATOR (PHP 8.0)
// -------------------------------------------------------------
// Safely navigates relationships; returns null if any object in the chain is null
$country = $session?->user?->getAddress()?->country;

// -------------------------------------------------------------
// 4. MATCH EXPRESSION (PHP 8.0)
// -------------------------------------------------------------
$statusCode = 400;

$message = match ($statusCode) {
    200,
    300 => null,
    400 => 'not found',
    500 => 'server error',
    default => 'unknown status code',
};

echo '<pre>'; var_dump($message); echo '</pre>';

// -------------------------------------------------------------
// 5. NEW STRING FUNCTIONS (PHP 8.0)
// -------------------------------------------------------------
str_contains('string with lots of words', 'words'); // true
str_starts_with('haystack', 'hay');                 // true
str_ends_with('haystack', 'stack');                 // true

// -------------------------------------------------------------
// 6. ARRAY_IS_LIST FUNCTION (PHP 8.1)
// -------------------------------------------------------------
echo '<pre>'; var_dump(array_is_list($numbers)); echo '</pre>'; // true

// -------------------------------------------------------------
// 7. ENUMS (PHP 8.1)
// -------------------------------------------------------------
interface MyInterface {
    public static function hello();
}

trait MyTrait {}

// Backed Enum with Int scalar values implementing interfaces and traits
enum UserStatus: int implements MyInterface
{
    use MyTrait;

    case Active = 1;
    case Pending = 2;
    case Deleted = 3;

    public const ARCHIVED = self::Deleted;

    public function text(): string
    {
        return match($this) {
            self::Active => 'User is active',
            self::Pending => 'User is pending',
            self::Deleted => 'User is deleted',
        };
    }

    public static function staticMethod(): string
    {
        return "Hello from static";
    }

    public static function hello(): string
    {
        return "hello from interface";
    }
}

// Fixed Invocation: Instantiating case to call interface method vs calling via enum directly
echo '<pre>'; var_dump(UserStatus::Pending->hello()); echo '</pre>';
echo '<pre>'; var_dump(UserStatus::hello()); echo '</pre>';
echo '<pre>'; var_dump(UserStatus::cases()); echo '</pre>';

// -------------------------------------------------------------
// 8. ARRAY UNPACKING (PHP 7.4+ Indexed / PHP 8.1+ Associative)
// -------------------------------------------------------------
$numbers = [1, 2, 3, 4];
$newNumbers = [...$numbers, 5, 6, 7];
echo '<pre>'; var_dump($newNumbers); echo '</pre>';

// -------------------------------------------------------------
// 9. READONLY PROPERTIES, FINAL CONSTANTS, NEW IN INITIALIZERS (PHP 8.1)
// -------------------------------------------------------------
class Cart {}

class AdvancedSampleClass {
	
    // Final Class Constants (PHP 8.1)
    final public const PI = 3.14;
	
    // Readonly property (PHP 8.1)
    public readonly string $username;
	
    // Constructor Property Promotion + New in Initializers (PHP 8.1)
    public function __construct(
        public string $firstname,
        public string $lastname,
        public int $age,
        string $username,
        public ?Cart $cart = new Cart() // "new" in initializers
    ){
        $this->username = $username;
    }
}

$obj = new AdvancedSampleClass('Bala', 'karthikeyan', 33, 'balakarthikeya');
```

---

# Topic 2: PHP Caching Systems (OPcache vs. APCu)

### OPcache

* **Purpose:** Caches compiled PHP opcode (bytecode) in shared memory.
* **Mechanism:** Eliminates the need for PHP to compile source scripts on every incoming HTTP request.
* **Scope:** System-level operation, built directly into modern PHP installations.
* **Recommendation:** Essential for standard production environments.

### APCu (Alternative PHP Cache User Cache)

* **Purpose:** An in-memory data cache specifically for user variables, data objects, arrays, and query outputs.
* **Mechanism:** Saves values in RAM so application logic can bypass heavy database queries or disk I/O operations.

#### Operational Flow Architecture:

```text
Without APCu
Request -> PHP -> Database/File System -> Return Result

With APCu
Request -> PHP -> [Cache Hit]  -> Return immediately
               -> [Cache Miss] -> Database/File System -> Store in APCu -> Return Result

```

#### Application Data Caching Example:

```php
// Store data into APCu
apcu_store('countries', $countries);

// Retrieve data from APCu
$countries = apcu_fetch('countries');
```

#### Caching Strategies in WordPress

* **Commonly Used:** Redis, Memcached, Object Cache, Transients, OPcache.
* **Optional Extension:** APCu.

---

# Topic 3: PHP-FPM (FastCGI Process Manager)

PHP-FPM manages process execution for high-traffic environments with features like user isolation (`uid`/`gid`/`chroot`), separate configuration overrides per pool, `stdout`/`stderr` logging, emergency dynamic worker restarts, optimized file uploads, slow-log profiling, and basic status metrics.

### Process Management Modes

#### 1. Static

* Maintains a constant, fixed number of child processes running continuously.
* **Advantage:** Fastest execution speed under steady traffic; avoids worker instantiation overhead.
* **Configuration Target:** `/etc/php/7.2/fpm/pool.d/www.conf`

```ini
pm = static 
pm.max_children = 10 
```

#### 2. Dynamic

* Spawns worker processes dynamically based on load rules while keeping a designated pool idle.
* **Configurable Directives:**
* `pm.max_children`: Maximum allowed child processes.
* `pm.start_servers`: Initial child processes started on FPM boot.
* `pm.min_spare_servers`: Minimum idle process pool size.
* `pm.max_spare_servers`: Maximum idle process pool size.
* `pm.process_idle_timeout`: Time (in seconds) before an idle worker process is killed.

#### 3. Ondemand

* Creates zero child processes on startup and instantiates worker processes strictly as incoming requests arrive.

---

### PHP-FPM Sizing Formula & Server Memory Profiling

#### Sizing Rule of Thumb Guidelines:

| Directive | Mathematical Definition |
| --- | --- |
| **`max_children`** | `(Total RAM – Memory used for OS, DB, background services) / Average PHP Process Size` |
| **`start_servers`** | `Number of CPU Cores × 4` |
| **`min_spare_servers`** | `Number of CPU Cores × 2` |
| **`max_spare_servers`** | Identical to `start_servers` |
| **`process_idle_timeout`** | Time threshold in seconds after which idle processes are destroyed |

#### Linux Memory Inspection Commands & Output Logs:

```bash
# Query kernel for total available memory
vagrant@vagrant:~$ grep MemTotal /proc/meminfo
MemTotal:        2095136 kB

# Review total system memory utilization
vagrant@vagrant:~$ free -hl
              total        used        free      shared  buff/cache   available
Mem:           2.0G         96M        1.5G         34M        408M        1.7G
Low:           2.0G        504M        1.5G
High:            0B          0B          0B
Swap:          979M          0B        979M

banandakumar@wordpress:~$ grep MemTotal /proc/meminfo
MemTotal:        2001920 kB
```

#### Final Recommended Configuration Template (`www.conf`):

```ini
pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
pm.max_requests = 500
```

---

# Topic 4: PHPUnit Testing Framework

### Installation & Alias Setup

```bash
# Install PHPUnit via Composer as a development dependency
composer require --dev phpunit/phpunit ^9.2.5

# Windows Command Prompt Wrapper Alias Setup
echo @php "%~dp0phpunit.phar" %* > phpunit.cmd

# Output installed framework version
phpunit --version
```

### Execution Output Indicators

* `.` : Test completed successfully.
* `F` : Assertion failure encountered during execution.
* `E` : Unhandled error or PHP exception thrown during execution.
* `R` : Test flagged as **Risky** (e.g., missing assertions, unintended output).
* `S` : Test execution **Skipped**.
* `I` : Test marked as **Incomplete** or pending implementation.
* `W` : Execution emitted internal system warnings.

### CLI Execution Commands

#### Local Binary Execution

```bash
# Execute tests directly through explicit Phar invocation
php phpunit-9.2.5.phar tests/filename.php

# Execute tests with Composer autoloader & export text coverage report
php phpunit-9.2.5.phar phpunit --bootstrap ./vendor/autoload.php tests/filename.php --coverage-text="coverage.txt"
```

#### Global Executable Usage

```bash
# Execute entire suite inside the tests directory
phpunit tests/

# Execute a single targeted test file
phpunit tests/filename.php

# Filter execution by class name or method pattern
phpunit tests/ --filter filename
```