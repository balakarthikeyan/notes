# PHP Features 8.0 
- Just-In-Time (JIT)
- Names Arguments
- Union Types
- Constructor Property Promotion
- Null-Safe Operator
- Trailing Comma in Parameters
- Match Expression
- Attributes
- WeakMaps
- Mixed Type
- Throw Exception From New Places
- Call ::class on objects
- Non-Capturing Catch
- New String Functions

# PHP Features 8.1
- Enums
- Array unpacking with string values
- Readonly properties
- Intersection types
- First class callable syntax
- array_is_list function
- Final class constants
- new in initializers
- Names arguments after unpacking
- Octal literal notation

## Examples:

```
$numbers = [1, 2, 3, 4, 5, 6];
$squares = array_map(array:  $numbers, callback: function($n) {
    return $n * $n;
});
echo '<pre>'; var_dump($squares); echo '</pre>';

$position = strpos( needle: "World", haystack: "Hello World");
echo '<pre>'; var_dump($position); echo '</pre>';

class SampleClass {
    /**
     * @var int|float
     */
    public int|float $number;
	
    public function __construct(
        public string $firstname,
        public string $lastname,
        public int $age
    ){}
	
    public function setNumber(int|float $number)
    {
        $this->number = $number;
    }

    public function getNumber(): int|float
    {
        return $this->number;
    }
}

// Which check the value associcated with variable object is there or not
$country = $session?->user?->getAddress()?->country;

// Alternative for switch is Match Expression
$message = match ($statusCode) {
    200,
    300 => null,
    400 => 'not found',
    500 => 'server error',
    default => 'unknown status code',
};
echo '<pre>'; var_dump($message); echo '</pre>';

//New String functions
str_contains('string with lots of words', 'words'); // true

str_starts_with('haystack', 'hay'); // true

str_ends_with('haystack', 'stack'); // true

echo '<pre>'; var_dump(array_is_list($numbers)); echo '</pre>'; // true 

//Enums
interface MyInterface{
    public static function hello();
}

trait MyTrait{}

enum UserStatus: int implements MyInterface
{
    use MyTrait;
    case Active = 1;
    case Pending = 2;
    case Deleted = 3;

    public const ARCHIVED = self::Deleted;

    public function text()
    {
        return match($this) {
            self::Active => 'User is active',
            self::Pending => 'User is pending',
            self::Deleted => 'User is deleted',
        };
    }

    public static function staticMethod()
    {
        return "Hello from static";
    }

    public static function hello()
    {
        return "hello from interface";
    }
}

echo '<pre>'; var_dump(UserStatus::Pending->hello()); echo '</pre>';

echo '<pre>'; var_dump(UserStatus::hello()); echo '</pre>';

echo '<pre>'; var_dump(UserStatus::cases()); echo '</pre>';

$numbers = [1, 2, 3, 4];
$newNumbers = [...$numbers, 5, 6, 7];

echo '<pre>'; var_dump($newNumbers); echo '</pre>';

class Cart{}

class SampleClass {
	
	final public const PI = 3.14;
	
	public readonly string $username;
	 
	// Constructor Property Promotion
    public function __construct(
        public string $firstname,
        public string $lastname,
        public int $age,
		string $username,
		public ?Cart $cart = new Cart() // in class initializers
    ){
		$this->username = $username;
    }
	
}

$obj = new SampleClass('Bala', 'karthikeyan', 33, 'balakarthikeya');

```