## What is OOPS?

OOPS stands for Object Oriented Programming system. It is a programming technique which is used to design your application. In this programs are considered as a collection of objects and each object is an instance of a class.

## What are the main features of OOPs?

- Inheritance
- Polymorphism
- Encapsulation
- Data Abstraction

## What are the advantages of OOPS in PHP?

- Code Resusability
- Flexibility
- Maintainability
- Security
- Testability

## What is Class and Objects in OOPS?

Class
In Object-Oriented Programming, a class is a blueprint that defines the functions and variables which are common to objects of a certain kind.

Object
In OOPS an object is a specimen of the class. It is nothing but a component that consists of methods and properties which make the data useful and help users to determine the behavior of the class.

```php
class Person{
    public $name;

    function __construct($name){
        $this->name = $name;
    }

    function getUserDetails(){
        return "My name is ".$this->name;
    }
}
```

## What is an object?

An object is an instance of a class. It has its own behavior and identity.

```php
$object = new MyCustomClass();
// OR
$object = new MyCustomClass;
```

## What is Encapsulation in oops with an example?

The process of binding up data and functions together that manipulates them is known as encapsulation in OOPS.

It is an attribute of an object, and it contains all data which is hidden. That hidden data is restricted to the members of that class.

```php
class Account {

    private int account_number;
    private int account_balance;

    public show_summary() : void {
        //..
    }

    public deposit(int a) : void {
        if (a < 0) {
            //show error
        } else {
            account_balance = account_balance + a;
        }
    }
}
```

## What is Polymorphism and its types in OOPS?

Polymorphism is one of the core concepts which is used in object-oriented programming. Polymorphism generally shows the relationships between parent class objects and child class objects.

Types of polymorphism in OOPs:

`Compile Time Polymorphism` - It is also known as Static Binding and allows the programmer to implement various methods. Names can be the same but their parameters should be different.
`Runtime Polymorphism` - It is also known as Dynamic Binding and allows the programmer to call a single overridden method during the runtime of the program.

## What is Inheritance?

It is a technique in which one class acquires the property of another class. With the help of inheritance, we can reuse the fields and methods of the existing class.

Inheritance has three type, are given below.

- Single inheritance
- Multiple inheritance
- Multi level inheritance

But PHP supports only single inheritance, where only one class can be derived from single parent class. We can also use multiple inheritance by using interfaces.

## What is constructor and destructor?

Constructor and Destructor both are special functions which are automatically called when an object is created and destroyed.

```php
class Animal
{
    public function __construct() { 
        echo "I'm alive!";
    } 

    public function __destruct() { 
        echo "I'm dead now :("; 
    } 
}

$animal = new Animal(); 
```

## What is different types of Visibility?

PHP have three access modifiers such as public, private and protected.

- `public` scope of this variable or function is available from anywhere, other classes and instances of the object.
- `private` scope of this variable or function is available in its own class only.
- `protected` scope of this variable or function is available in all classes that extend current class including the parent class.

## What are the Final class and Final methods?

`Final Class` - A class that can’t be extended and inherited further is known as Final Class. This class is declared with the keyword final and should be declared.

`Final Method` - Methods in the final class are implicitly final and if a user uses the final keyword that means methods can’t be overridden by subclasses.

## What is static keyword ?

When a variable or function declared as static then it cannot be accessed with an instantiated class object. It is treats as public if no visibility is defined. It can also be used to define static variables and for late static bindings.

## What are the difference between overloading and overriding in oops?

- `Overloading` : It occurs when two or more methods in one class have the same method name but different parameters.

- `Overriding` : It means having two methods with the same method name and parameters. One of the methods is in the parent class and the other is in the child class.

## What is "this"?

It refers to the current object of a class.

## What are the difference between abstract class and interface in OOPS?

Thera are many differences between abstract class and interface in php.

1. Abstract methods can declare with public, private and protected. 
* Interface methods declared with public.
2. Abstract classes can have constants, members, method stubs and defined methods.
* Interfaces can only have constants and methods stubs.
3. Abstract classes doest not support multiple inheritance but interface support this.
4. Abstract classes can contain constructors but interface doest not support constructors.

## What is namespace in PHP?

It allows us to use the same function or class name in different parts of the same program without causing a name collision.

## What is traits?

Traits is a group of methods that reuse in single inheritance. A Trait is intended to reduce some limitations of single inheritance by enabling a developer to reuse sets of methods.

## What are magic methods?

Magic methods are special names, starts with two underscores, which denote methods which will be triggered in response to particular PHP events.

- __construct()
- __destruct()
- __call()
- __get()
- __set()
- __isset()
- __unset()
- __sleep()
- __clone()
- __autoload()