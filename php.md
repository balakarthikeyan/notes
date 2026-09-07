### Core PHP Concepts, Architecture & Security

#### Sessions & Protocols

HTTP is a stateless protocol, so PHP sessions maintain state on the server and share data across multiple pages by storing data for individual clients against a unique session ID.

* **Session ID & Cookies**: Session IDs are sent to the browser via cookies. If a session ID is not present on the server, PHP creates a new session and generates a new session ID.
* **Defaults**: The default session name is `"PHPSESSID"`. The default session lifetime is 1440 seconds (24 minutes), and default session files are stored in the temporary `/tmp` directory on the server (which is inaccessible from the outside world).

```php
// Example: Setting custom session lifetime
// Server should keep session data for AT LEAST 1 hour
ini_set('session.gc_maxlifetime', 3600);

// Each client should remember their session id for EXACTLY 1 hour
session_set_cookie_params(3600);
```

---

#### Data Types & Language Constructs

PHP supports 9 primitive types:

* **4 Scalar Types**: `integer`, `boolean`, `float`, `string`
* **3 Compound Types**: `array`, `object`, `callable`
* **2 Special Types**: `resource`, `NULL`

**`echo` vs. `print`**

* **`echo`**: Has a `void` return type, can take multiple parameters separated by commas, and is slightly faster.
* **`print`**: Has a return value of `1` (allowing it to be used in expressions), cannot take multiple parameters, and is slightly slower.

---

#### OOP Concepts & Dynamic Features

* **Namespaces**: Provide a way of grouping related classes, interfaces, functions, and constants.
* **Overriding vs. Overloading**:
    * **Overriding**: A child/derived class defines a method with the exact same name and parameters as a parent class (inherent to inheritance).
    * **Overloading**: Creating multiple methods with the same name but different parameter signatures within the same class (handled via dynamic magic methods in PHP).
* **Magical Methods**: Functions residing inside a PHP class with definitions supplied by the programmer:
`__construct()`, `__destruct()`, `__call()`, `__callStatic()`, `__get()`, `__set()`, `__isset()`, `__unset()`, `__sleep()`, `__wakeup()`, `__toString()`, `__invoke()`, `__set_state()`, `__clone()`, and `__debugInfo()`.

---

#### Package Management & Engine

* **Composer**: Application-level package manager used to declare and manage (install/update) project dependencies.
* **PEAR**: (PHP Extension and Application Repository) A structured library of reusable PHP code components that maintains distribution standards.
* **PECL**: (PHP Extension Community Library) An online directory/repository providing hosting facilities for downloading and developing C extensions for PHP.
* **Zend Engine**: The open-source scripting engine that drives PHP (e.g., Zend Engine 3.0).

---

#### Security, Hashing & Utilities

* **XSS (Cross-Site Scripting)**: Security vulnerability allowing attackers to inject client-side scripts into web pages viewed by other users to bypass access controls like the same-origin policy.
* **Exploitable/Sensitive Functions**: Functions requiring careful sanitization include `exec`, `passthru`, `system`, `proc_open`, `eval()`, `assert()`, `phpinfo`, `posix_mkfifo`, `posix_getlogin`, and `posix_ttyname`.
* **Error Suppression**: The `@` operator suppresses error messages for a single statement; runtime errors occurring on that line are handled internally by PHP.
* **Argument Handling**: `func_get_args()` retrieves an array of arguments passed to a function.
* **Exit Function**: `exit(message)` terminates script execution. Passing an integer status sets an exit code (valid range: 0–254; 255 is reserved) rather than printing a string. Can be called without parentheses if no status is passed.
* **MD5 vs. SHA256**: Cryptographic hash functions generating fixed-size outputs (MD5 = 128-bit, SHA256 = 256-bit).
* **Mbstring Extension**: Multibyte string extension used to manage non-ASCII character encodings (like UTF-8 or UCS-2) where characters exceed 256 byte-wise slots. Provides specific multibyte string functions like `mb_strlen()` and `mb_split()`.
* **GD Library**: An open-source library requiring an ANSI C compiler used by PHP to dynamically create and manipulate images (PNG, JPEG, GIF, charts, and graphics).
* **MIME**: (Multipurpose Internet Mail Extensions) Extension of the email protocol supporting exchange of diverse media types (audio, video, application programs, ASCII text) via SMTP.

---

### Error Handling & Exceptions

#### PHP Error Types (13 Core Types)

* **`E_ERROR`**: Fatal run-time error that halts script execution.
* **`E_WARNING`**: Non-fatal run-time warning; script execution continues.
* **`E_PARSE`**: Compile-time parse/syntax error.
* **`E_NOTICE`**: Run-time notice indicating potential script defects.
* **`E_CORE_ERROR`**: Fatal error occurring during PHP's initial startup.
* **`E_CORE_WARNING`**: Non-fatal warning occurring during PHP's initial startup.
* **`E_COMPILE_ERROR`**: Fatal compile-time error generated by the script.
* **`E_USER_ERROR`**: User-generated error message triggered via `trigger_error()`.
* **`E_USER_WARNING`**: User-generated warning message.
* **`E_USER_NOTICE`**: User-generated notice message.
* **`E_STRICT`**: Run-time notices for code interoperability/compatibility.
* **`E_RECOVERABLE_ERROR`**: Catchable fatal error indicating a dangerous state.
* **`E_ALL`**: Bitmask capturing all supported errors and warnings.

#### Standard Exception Methods

* `getMessage()` – Exception message
* `getCode()` – Exception code
* `getFile()` – Source filename where exception was thrown
* `getLine()` – Source line number
* `getTrace()` – Backtrace array
* `getTraceAsString()` – Formatted string of backtrace
* `Exception::__toString` – String representation of exception object

---

### Code Implementation Snippets & Patterns

#### 1. 301 Permanent Redirect

```php
header("HTTP/1.1 301 Moved Permanently"); 
header("Location: /option-a"); 
exit();

```

#### 2. Accessing Standard Error Stream (STDERR)

```php
$stderr = fwrite(STDERR, "Error output");
$stderr = fopen("php://stderr", "w");
$stderr = STDERR;

```

#### 3. Check if cURL Extension is Enabled

```php
if (function_exists('curl_version')) {
    echo "Curl is enabled";
}

```

#### 4. Print Floyd's Triangle

```php
echo "print Floyd's triangle";
echo "<pre>";
$key = 1; 
for ($i = 1; $i <= 4; $i++) { 
    for ($j = 1; $j <= $i; $j++) { 
        echo $key; 
        $key++; 
        if ($j == $i) { 
            echo "<br/>"; 
        } 
    } 
} 
echo "</pre>";
```

#### 5. Check if an Array is Associative

```php
function has_string_keys(array $array) {
    return count(array_filter(array_keys($array), 'is_string')) > 0;
}

```

#### 6. Singleton Design Pattern

```php
/**
 * Singleton class
 */
final class UserFactory {
    /**
     * Call this method to get singleton
     *
     * @return UserFactory
     */
    public static function Instance() {
        static $inst = null;
        if ($inst === null) {
            $inst = new UserFactory();
        }
        return $inst;
    }

    /**
     * Private construct so nobody else can instantiate it
     */
    private function __construct() {
    }
}

$fact = UserFactory::Instance();

```

#### 7. CamelCase String Generator Function

```php
public static function camelCase($str, array $noStrip = []) {
    // Non-alpha and non-numeric characters become spaces
    $str = preg_replace('/[^a-z0-9' . implode("", $noStrip) . ']+/i', ' ', $str);
    $str = trim($str);
    // Uppercase the first character of each word
    $str = ucwords($str);
    $str = str_replace(" ", "", $str);
    $str = lcfirst($str);

    return $str;
}

```

#### 8. PHPMailer Integration Setup

```php
require_once('/library/PHPMailer/PHPMailerAutoload.php');
$mail = new \PHPMailer();
$mail->IsSMTP();
$mail->SMTPDebug = 0;
$mail->SMTPAuth = 'login';
$mail->SMTPSecure = 'ssl';
$mail->Host = 'smtp.gmail.com';
$mail->Port = 465;
$mail->Username = 'example@gmail.com';
$mail->Password = 'somepassword';
$mail->SetFrom('example@gmail.com', 'Example');
$mail->Subject = 'The subject';
$mail->Body = 'The content';
$mail->IsHTML(true);
$mail->AddAddress('receiver@gmail.com');
$mail->Send();

```


---

### PHP Version Evolution (7.0 through 8.4)

#### PHP 7.0 Features

* **50% Speed Improvement**: PHP 7 reduces memory and resource consumption significantly.
* **Engine Exceptions**: Errors can be replaced with catchable exceptions (uncaught exceptions fall back to errors).
* **64-bit Architecture**: Native 64-bit Windows support system.
* **Anonymous Classes**: Anonymous classes supported via `new class` syntax.
* **Spaceship Operator (`<=>`)**: Performs 3-way type comparisons returning `<0`, `0`, or `>0` to assist sorting.
* **Scalar Type Declarations**: Function signatures accept scalar types like `int`, `string`, `bool`.
* **Return Type Declarations**: Preceded by a colon `:` before the opening brace to specify return types.
* **`Closure::call()` Method**: Temporarily binds object scope to an anonymous function/closure and invokes it.

#### PHP 8.0 - 8.2 Baseline Features (Referenced in Notes)

* **Readonly Classes & Properties**: Create immutable objects.
* **Asynchronous Signal Handling**: Enables handling `SIGINT`/`SIGTERM` signals asynchronously.
* **Fibers**: Lightweight coroutines for native concurrency.
* **DNF (Disjunctive Normal Form) Types**: Combines intersection and union types.
* **Intersection Types**: Requires a parameter or return value to satisfy multiple interfaces/types simultaneously.
* **Array Unpacking**: Extended to support string keys.

---

#### PHP 8.3 Detailed Breakdown

##### 1. Deep Cloning of Readonly Properties

Allows `readonly` properties to be reinitialized during object cloning inside the `__clone()` method.

##### 2. New `#[\Override]` Attribute

Ensures a child method overrides a parent method, raising a compile-time error if the parent method is renamed or missing.

##### 3. Dynamic Fetching of Class Constants & Enum Members

```php
$constantName = 'THE_CONST';
$memberName = 'FirstMember';

echo MyClass::{$constantName};
echo MyEnum::{$memberName}->value;
```

##### 4. Typed Class Constants
Applies type declarations to constants across classes, interfaces, traits, and enums:
```php
interface ConstTest {
    const string VERSION = "PHP 8.3";
}
```

##### 5. `json_validate()` Function
Validates JSON syntax without allocating memory to parse an object or array:
```php
if (json_validate($maybeJSON)) {
    // Valid JSON string
}
```

##### 6. Random Extension Additions
* `Randomizer::getBytesFromString($string, $length)`: Selects random characters from a provided string.
* `Randomizer::getFloat($min, $max)` & `nextFloat()`: Generates random floating-point numbers.

##### 7. INI Environment Variable Fallback Syntax
```ini
session.name = ${SESSION_NAME:-Foo}
sendmail_from = "${MAIL_FROM_USER:-info}@${MAIL_FROM_DOMAIN:-example.com}"
```

##### 8. `class_alias()` with Built-in Classes
```php
class_alias(\DateTime::class, 'MyDateTime');
$customDateTime = new MyDateTime();
```

##### 9. CLI Multi-File Syntax Linting
```bash
php -l file1.php file2.php file3.php
```

##### 10. Granular Exception & Error Updates
* **SQLite3**: Throws `SQLite3Exception`.
* **Date/Time**: Introduces specific errors like `DateRangeError` and `DateException`.
* **`unserialize()`**: Elevates syntax and handler notices (`E_NOTICE`) to warnings (`E_WARNING`).

##### 11. PHP 8.3 Deprecations & Breaking Changes
* **Argument-less `get_class()` / `get_parent_class()`**: Deprecated.
* **`highlight_file()` / `highlight_string()` Output**: Wrapped in `<pre><code></code></pre>`, without converting spaces to HTML entities or line breaks to `<br/>`.
* **Non-Numeric Increment/Decrement**: Using `++` or `--` on non-numeric strings is deprecated.
* **Array Negative Keys**: Appending items (`$array[]`) to an array containing a negative key no longer defaults to index `0`.
* **`proc_get_status()`**: Requires the process resource argument explicitly (`proc_get_status($process)`).

---

#### PHP 8.4 Detailed Breakdown

##### 1. Property Hooks
Defines property-level `get` and `set` operations directly.

* **Legacy (Before PHP 8.4)**:
```php
class Locale {
    private string $languageCode;
    private string $countryCode;

    public function __construct(string $languageCode, string $countryCode) {
        $this->setLanguageCode($languageCode);
        $this->setCountryCode($countryCode);
    }
    public function getLanguageCode(): string { return $this->languageCode; }
    public function setLanguageCode(string $languageCode): void { $this->languageCode = $languageCode; }
    public function getCountryCode(): string { return $this->countryCode; }
    public function setCountryCode(string $countryCode): void { $this->countryCode = strtoupper($countryCode); }
    public function getCombinedCode(): string { return sprintf("%s_%s", $this->languageCode, $this->countryCode); }
}

```

* **Modern (PHP 8.4)**:

```php
class Locale {
    public string $languageCode;

    public string $countryCode {
        set (string $countryCode) {
            $this->countryCode = strtoupper($countryCode);
        }
    }

    public string $combinedCode {
        get => sprintf("%s_%s", $this->languageCode, $this->countryCode);
        set (string $value) {
            [$this->countryCode, $this->languageCode] = explode('_', $value, 2);
        }
    }

    public function __construct(string $languageCode, string $countryCode) {
        $this->languageCode = $languageCode;
        $this->countryCode = $countryCode;
    }
}

```

##### 2. Asymmetric Property Visibility

Enables separate visibility scope declarations for read vs write operations.

```php
class PhpVersion {
    // Read is public, Write is restricted to private
    public private(set) string $version = '8.4';
}

$phpVersion = new PhpVersion();
var_dump($phpVersion->version); // "8.4"
// $phpVersion->version = '8.3'; // Fatal Error: Visibility violation

```

##### 3. `#[\Deprecated]` Attribute

Native attribute to mark functions, methods, and constants as deprecated.

```php
class PhpVersion {
    #[\Deprecated(
        message: "use PhpVersion::getVersion() instead",
        since: "8.4"
    )]
    public function getPhpVersion(): string {
        return $this->getVersion();
    }

    public function getVersion(): string {
        return '8.4';
    }
}

```

##### 4. Spec-Compliant HTML5 `ext-dom` Namespace

Introduces `Dom\HTMLDocument` and `Dom\XMLDocument` with native CSS selector support (`querySelector`).

```php
$dom = Dom\HTMLDocument::createFromString(
    <<<HTML
    <main>
        <article>PHP 8.4</article>
        <article class="featured">Spec Compliant DOM</article>
    </main>
    HTML,
    LIBXML_NOERROR
);

$node = $dom->querySelector('main > article:last-child');
var_dump($node->classList->contains("featured")); // bool(true)

```

##### 5. New Array Callback Helpers (`array_*`)

* **`array_find()`**: Returns the value of the first matching element.
```php
$animal = array_find(['dog', 'cat', 'cow'], fn($v) => str_starts_with($v, 'c')); // "cat"

```
* **`array_find_key()`**: Returns the key of the first matching element.
```php
$key = array_find_key(['a' => 10, 'b' => 50], fn($v) => $v > 20); // "b"

```
* **`array_any()`**: Returns `true` if at least one element passes the truth test.
```php
$hasStock = array_any($items, fn($item) => $item['stock'] > 0);

```
* **`array_all()`**: Returns `true` if every element passes the truth test.
```php
$allValid = array_all($prices, fn($price) => $price > 0);

```

##### 6. Driver-Specific PDO Subclasses

Instantiates driver-specific connections via `PDO::connect()` to expose driver-only features safely.

```php
$connection = PDO::connect('sqlite:foo.db', $user, $pass); // Object of class Pdo\Sqlite
$connection->createFunction('prepend_php', fn($str) => "PHP $str");

```

##### 7. Method Chaining Without Parentheses

Instantiate an object and invoke methods or access properties directly without outer wrapping parentheses.

```php
// Legacy
var_dump((new PhpVersion())->getVersion());

// Modern PHP 8.4
var_dump(new PhpVersion()->getVersion());

```

##### 8. Additional PHP 8.4 Functionality & Extensions

* **BCMath**: Added `bcceil()`, `bcdivmod()`, `bcfloor()`, and `bcround()`.
* **Rounding Options**: `RoundingMode` Enum introduced with modes: `TowardsZero`, `AwayFromZero`, `NegativeInfinity`, `PositiveInfinity`.
* **Date Microsecond Support**: `createFromTimestamp()`, `getMicrosecond()`, `setMicrosecond()` added.
* **Multibyte Additions**: `mb_trim()`, `mb_ltrim()`, `mb_rtrim()`, `mb_ucfirst()`, `mb_lcfirst()`.
* **PCNTL & Reflection**: Extended process and reflection capabilities (e.g., `ReflectionProperty::isDynamic()`).
* **New JIT Implementation**: Built upon the IR framework.
* **`request_parse_body()`**: Utility function to force HTTP request payload parsing.

##### 9. Deprecations & Breaking Changes in PHP 8.4

* **Removed Extensions**: `IMAP`, `OCI8`, `PDO_OCI`, and `pspell` are detached from core and migrated to PECL.
* **Undefined Property Suppression**: `unset()` on an undefined property emits a warning.
* **Implicit Nullable Types**: Implicitly nullable parameters (e.g. `function foo(string $param = null)`) are deprecated; explicit `?string` must be declared.
* **Class Name `_**`: Using `_` as a class name is deprecated.
* **Final Classes**: `GMP` class is now marked `final`.
* **Removed Constants**: Various `MYSQLI_*` flags (e.g. `MYSQLI_SET_CHARSET_DIR`) are removed.