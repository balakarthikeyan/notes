# Dependency Inversion
Laravel uses different design pattern such as Singleton, Factory, Builder, Facade, Strategy, Provider, Proxy etc. 

Dependency Injection is a design pattern. Dependency Injection is High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

It injects objects into the constructor or methods of other objects, so that one object depends on one or more other objects. It is a broader form of inversion of control (IOC).

A `dependency` is an object that can be used (a service). An `injection` is the passing of a dependency to a dependent object (a client) that would use it.

```php
class DatabaseWriter {
    protected $db;

    // Instead of depending on a concrete instance, we can now inject any instance that consumes the type hinted interface. The interface takes care that the later concrete instance implements all the methods that we are going to use, so that we can still rely on them in the dependent classes.
    public function __construct(DatabaseAdapterInterface $db)
    {
        $this->db = $db;
    }

    // You can see that we require the classes constructor a DatabaseAdapter instance to be passed. Since we are doing that in the constructor, an object of that class cannot be instantiated without it: We are injecting a dependency. Now, that we know that the DatabaseAdapter is always existent within the class we can easily rely on it.
    public function __construct(DatabaseAdapter $db)
    {
        $this->db = $db;
    }

    public function write()
    {
        $this->db->query('...');
    }
}
```
The write() method just calls methods on the adapter, since we definitely know it exists because we made use of DI.

## The three types of Dependency Injection

### Constructor Injection
The type of Dependency Injection explained above is called Constructor Injection. That simply means that dependencies are passed as arguments to the classes constructor. The dependencies are then stored as properties and thus made available in all methods of the class. The big advantage here is, that a object of the class cannot exist without passing the dependencies.

### Setter Injection
This type makes use of dedicated methods to inject dependencies. Instead of using the constructor. The advantage of using Setter Injection is, that you can add dependencies to an object after it has been created. It's commonly used for optional dependencies. Setter Injection is also great to declutter your constructor and have your dependencies only in methods where you need them.
```php
class RegisterUserService {

    protected $logger;

    public function setLogger( Logger $logger )
    {
        $this->logger = $logger;
    }

    public function registerUser()
    {
        // Do stuff to register the user
        if($this->logger)
            $this->logger->log("User has been registered");
    }
}
// Create an instance of the User Service
$service = new RegisterUserService();
$service->registerUser(); // Nothing is Logged
// Inject the Logger dependency into the User object
$service->setLogger(new ConcreteLogger);
// Call the register method, which will log a message
$service->registerUser();
```

### Interface Injection
The idea of interface injection is basically, that the method(s) to inject a dependency is defined in an interface. The class that is going to need the dependency must implement the interface. Hereby it is ensured that the needed dependency can be properly injected into the dependent object. It's a stricter form of the previously explained Setter Injection.
```php
interface Database {
    public function query();
}

interface InjectDatabaseAccess {
    // The user of this interface MUST provide a concrete of Database through this method
    public function injectDatabase( Database $db );
}

class MariaDB implements Database {
    public function query($args)
    {
        // Execute Query
    }
}

class MySQL implements Database {
    public function query($args)
    {
        // Execute Query
    }
}

class DbDoer implements InjectDatabaseAccess {
    protected $db;

    public function injectDatabase( Database $db )
    {
        $this->db = $db;
    }

    public function doSomethingInDb($args)
    {
        $this->db->query();
    }
}

$user = new DbDoer();
$user->injectDatabase( new MySQL );
$user->doSomethingInDb($stuff);
```

## The IoC Container
Inversion of control is a design pattern that is used for decoupling components and layers of a system. Inversion of control(IOC) is implemented through injecting dependencies into a component when it is constructed.

IoC Container is a powerful tool for managing class dependencies. It has the power to automatically resolve classes without configuration. The IoC Container is a component that knows how instances are created and knows of all their underlying dependencies and how to resolve them.

If we take our example above, imagine that the DatabaseAdapter itself has it's own dependencies.
```php
class ConcreteDatabaseAdapter implements DatabaseAdapterInterface{
    protected $driver;

    public function __construct(DatabaseDriverInterface $driver)
    {
        $this->driver = $driver;
    }
}
```
So in order to be able to use the DatabaseAdapter you would need to pass in a instance of the DatabaseDriverInterface abstraction as a dependency.

The DatabaseWriter should not care about how the DatabaseAdapter works, it should only care about that a DatabaseAdapter is passed and not how it needs to be created.

```php
App::bind('DatabaseWriter', function(){
    return new DatabaseWriter(
        new ConcreteDatabaseAdapter(new ConcreteDatabaseDriver)
    );
});
```
the DatabaseWriter itself does not know anything about the dependencies of it's dependencies. But the IoC Container knows all about them and also knows where to find them. So eventually when the DatabaseWriter class is going to be instantiated, the IoC Container first is asked how it needs to be instantiated.