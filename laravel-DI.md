# What is Laravel?

Laravel is one of the most popular, highly used, open-source modern web application framework. It provides unique features like Eloquent ORM, Query builder ,Homestead which are the modern features, only present in Laravel.
I like Laravel because of its unique architectural design.Behind the scene Laravel uses different design pattern such as Singleton, Factory, Builder, Facade, Strategy, Provider, Proxy etc. 

# Dependency Inversion

Dependency Inversion Principle it will help us to understand why we need IoC Container
High-level modules should not depend on low-level modules. Both should depend on abstractions.
Abstractions should not depend on details. Details should depend on abstractions.

Dependency Injection is a design pattern, that does what the name states. It injects objects into the constructor or methods of other objects, so that one object depends on one or more other objects.
```
class DatabaseWriter {
    protected $db;

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

You can see that we require the classes constructor a DatabaseAdapter instance to be passed. Since we are doing that in the constructor, an object of that class cannot be instantiated without it: We are injecting a dependency. Now, that we know that the DatabaseAdapter is always existent within the class we can easily rely on it.

The write() method just calls methods on the adapter, since we definitely know it exists because we made use of DI.

The enormous advantage of using DI instead of misusing static classes, god objects, and other stuff like that is, that you can easily track down where the dependencies come from.

The other huge advantage is, that you can easily swap out dependencies. If you want to use another implementation of your dependency just pass it to the constructor. You don't need to hunt down hardcoded instances anymore in order to be able to swap them.

Leaving aside the fact that by using Dependency Injection you will easily be able to Unit-Test your classes, since you can just mock the dependencies which is hardly possible with hardcoded dependencies.

## The three types of Dependency Injection

### Constructor Injection
The type of Dependency Injection explained above is called Constructor Injection. That simply means that dependencies are passed as arguments to the classes constructor. The dependencies are then stored as properties and thus made available in all methods of the class. The big advantage here is, that a object of the class cannot exist without passing the dependencies.

### Setter Injection
This type makes use of dedicated methods to inject dependencies. Instead of using the constructor. The advantage of using Setter Injection is, that you can add dependencies to an object after it has been created. It's commonly used for optional dependencies. Setter Injection is also great to declutter your constructor and have your dependencies only in methods where you need them.
```
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

$service = new RegisterUserService;
$service->registerUser(); // Nothing is Logged

$service->setLogger(new ConcreteLogger);
$service->registerUser(); // Now we log
```
The object can be instantiated without any dependencies. There is a method to inject the dependency (setLogger()) which can be called optionally. Now it's up to the methods implementation to either make use of the dependency or not (if it's not set).

It's worth pointing out to be cautious with Setter Injection. Calling methods or accessing attributes on a dependency that has not yet been injected will result in a nasty Fatal error: Call to a member function XXX() on a non-object . Therefore everytime the dependency is accessed it has to be null-checked first. A much cleaner way to go about this would be to use the Null Object Pattern and to move the dependency into the constructor (as optional argument, if nothing is passed the null object is created inside the class)

### Interface Injection
The idea of interface injection is basically, that the method(s) to inject a dependency is defined in an interface. The class that is going to need the dependency must implement the interface. Hereby it is ensured that the needed dependency can be properly injected into the dependent object. It's a stricter form of the previously explained Setter Injection.
```
interface Database {
    public function query();
}

interface InjectDatabaseAccess {
    // The user of this interface MUST provide a concrete of Database through this method
    public function injectDatabase( Database $db );
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

The meaningfulness of Interface Injection is open to dispute. Personally I have never used it. I prefer Constructor Injection over it. That allows me to accomplish the exact same task without the need of additional interfaces on the injectors side.

## Dependency Inversion
Instead of depending on a concrete instance we can also depend on abstractions.
```
class DatabaseWriter {
    protected $db;

    public function __construct(DatabaseAdapterInterface $db)
    {
        $this->db = $db;
    }

    public function write()
    {
        $this->db->query('...');
    }
}
```
Now we inverted the control. Instead of relying on a concrete instance, we can now inject any instance that consumes the type hinted interface. The interface takes care that the later concrete instance implements all the methods that we are going to use, so that we can still rely on them in the dependent classes.

Make sure you get that one, because it is a core concept of the IoC-Container.

## The IoC Container
In the shortest terms I can think of I would describe the IoC container like that:

The IoC-Container is a component that knows how instances are created and knows of all their underlying dependencies and how to resolve them.

If we take our example above, imagine that the DatabaseAdapter itself has it's own dependencies.
```
class ConcreteDatabaseAdapter implements DatabaseAdapterInterface{
    protected $driver;

    public function __construct(DatabaseDriverInterface $driver)
    {
        $this->driver = $driver;
    }
}
```
So in order to be able to use the DatabaseAdapter you would need to pass in a instance of the DatabaseDriverInterface abstraction as a dependency.

But your DatabaseWriter class does not know about it, nor should it. The DatabaseWriter should not care about how the DatabaseAdapter works, it should only care about that a DatabaseAdapter is passed and not how it needs to be created. That's where the IoC-Container comes in handy.

```
App::bind('DatabaseWriter', function(){
    return new DatabaseWriter(
        new ConcreteDatabaseAdapter(new ConcreteDatabaseDriver)
    );
});
```
As I already said, the DatabaseWriter itself does not know anything about the dependencies of it's dependencies. But the IoC-Container knows all about them and also knows where to find them. So eventually when the DatabaseWriter class is going to be instantiated, the IoC-Container first is asked how it needs to be instantiated.