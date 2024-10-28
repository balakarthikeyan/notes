### What is Symfony?
> Symfony is a high-performance PHP framework that follows the Model-View-Controller (MVC) architectural pattern. It provides reusable components and libraries for accelerating the development process of web applications.

### What are the key components of the Symfony framework?

> Symfony is built on a set of decoupled and reusable components such as Routing, Templating (Twig), Forms, Dependency Injection, and ORM (Doctrine).

### How does Symfony compare to other PHP frameworks like Laravel or CodeIgniter?

> Symfony is known for its flexibility and modular architecture, while Laravel is admired for its elegant syntax and rapid development features. CodeIgniter is more lightweight and has a smaller footprint.

### What is Twig in the Symfony ecosystem?

> Twig is a flexible and fast templating engine integrated with Symfony for creating dynamic views.

### What is a Symfony bundle?

> A bundle in Symfony is a structured set of files (PHP files, stylesheets, JavaScripts, images) that implement a specific feature. Bundles are reusable packages or modules in Symfony that encapsulate functionality such as controllers, models, views, configuration files, and routes. They help in organizing code and promoting code reusability across different projects.

### How does Symfony handle database interactions?

> Symfony often uses Doctrine, an ORM (Object-Relational Mapping) tool, to interact with databases.

### How can you enhance the performance of a Symfony application?

> Performance can be improved by optimizing the code, using caching mechanisms like Varnish or Symfony's built-in HTTPCache, and optimizing database queries.

### How does Symfony support API development?

> Symfony has built-in tools and bundles like FOSRestBundle and API Platform that facilitate the creation of RESTful and GraphQL APIs.

### What's the role of the Symfony profiler?

> The Symfony profiler is a development tool that provides detailed insights into an application's performance, database queries, and other vital metrics.

### How do you handle forms in Symfony?

> Symfony provides a Form component that allows developers to create, render, and handle forms in an object-oriented manner.

### How can you secure a Symfony application?

> Symfony offers a Security component that manages authentication, authorization, CSRF protection, and more.

### What are Symfony Flex and Symfony Recipes?

> Symfony Flex is a Composer plugin that automates installing and configuring Symfony bundles. Recipes are instructions defined by bundles to automate configuration tasks.

### How does Symfony handle sessions and cookies?

> Symfony provides a Session component to manage user sessions and a Cookie class for handling cookies.

### How do you handle translations and internationalization in Symfony?

> Symfony offers a Translation component that provides tools for internationalization and translations.

### How can you handle errors and exceptions in Symfony?

> Symfony provides an Error Handling component that manages errors and exceptions, allowing developers to create custom error pages and log issues.

### How do you integrate third-party libraries in Symfony?

> Third-party libraries can be integrated into Symfony projects using Composer and then utilized within the application's code.

### How do you manage configurations in a Symfony project?

> Symfony uses YAML, XML, or PHP files to manage configurations, and the configuration values can be accessed using the service container.

### How can you optimize database queries in Symfony?

> Using Doctrine, developers can utilize the DQL (Doctrine Query Language), ensure proper indexing, and use caching mechanisms to optimize queries.

### What's the role of a Controller in a Symfony application?

> In Symfony, a Controller is responsible for handling HTTP requests, processing logic, and returning a response, typically a rendered view.

### What is the role of the Service container in Symfony?

> The service container in Symfony is a dependency injection container that manages object instances and their dependencies.

### How do you handle events and listeners in Symfony?

> Symfony provides an EventDispatcher component, which allows developers to create, dispatch, and listen to events throughout the application.

### How do you manage assets in a Symfony project?

> Symfony offers an Asset component and Webpack Encore to manage and compile assets like CSS, JavaScript, and images.

### How do you set up authentication using Symfony Guard?

> Symfony Guard is a powerful and customizable authentication system. By creating a Guard authenticator class and configuring the security settings, developers can define custom authentication processes.

### What is Symfony?

```
Symfony is a PHP framework that aims to accelerate the design and maintenance of web applications and replace recurrent coding tasks. A few prerequisites are mandate for installation: Linux, FreeBSD, Mac OS or Microsoft Windows, and a web server with PHP. 

Rails-like programming method, clean design, and code readability. Symfony offers Ajax helpers, plugins, and an admin generator interface, which renders the programming of complete applications truly easy. The developers hence can concentrate on applicative logic without wasting time writing infinite XML configuration files. 

Symfony can be utilized to create powerful applications in an enterprise context as it permits developers to test, debug, and document projects, giving them complete control over configuration and customization - from the directory structure to the foreign libraries. Symfony utilizes the Model-View-Controller design pattern, splitting the business logic from the presentation layer. 
```
### What are the benefits of Symfony?

> Symfony PHP framework is effortless to use gratitude to its Ruby-On-As a development framework. Symfony simplifies a developer's work, as it doesn't require writing extensive codes to create web applications.

### Advantages of Symfony Framework: 

- `Fast-Paced Development:` Time requires fast and high-performance applications. The developer community needs the flexibility to create great applications. Symfony 2 has the ability for performance optimization of applications. It ingests less memory and authorizes developers to develop applications at a considerable speed. 
- `Flexibility:` Symfony is fully adjustable to a developer's unique requirements, with independent and configurable components. Developers can easily create complicated applications with multiple functional features with their event dispatcher and dependency injector. The required functionalities can be built brick by brick and carry out the development task at their own pace. 
- `Expandable Framework:` Everything is present as a bundle in Symfony. Each bundle adds unique functionality and adds to its expansion. A developer reuses a bundle in another project and shares it with the community. Bundles can be reused to change everything within Symfony without the necessity to reconfigure the complete framework. It allows changing the core itself to help expand the framework. 
- `Stable Framework:` The company's developers offer support for three years, although they can provide you lifetime support on security-related issues. The company plans to furnish more stability by assuring compatibility among its minor versions. This guarantees the stability and sustainability of the application developed in any version of the framework. Moreover, it can also deliver compatibility with the public APIs for more functional advantages. 
- `Comfort & Convenience:` The Framework delivers a highly functional development environment and conveys to developers the desired level of pleasure and ease. Instead of concentrating on minor functionalities, the framework facilitates developers to focus more on the core functional features of an application. This improves a developer's productivity and makes their task smooth and error-free. Symfony devices, such as the Web Debug Toolbar, bring more convenience for a developer to address errors and security-related issues. 
- `User Friendly:` Symfony is an easy-to-use framework for any developer and is highly convenient for beginners and advanced users, with a host of documentation and support from the company and the community. With an acceptable level of professional support, a newbie can quickly learn the best practices of Symfony Development to help create agile and performance-driven solutions. 

###  What are the components in Symfony?

Symfony Components are a collection of decoupled and reusable PHP libraries. It is open-source software that desires to quicken or speed up the creation and maintenance of web applications, substitute repetitive coding tasks, create powerful and robust applications in an enterprise context, and give developers complete control over the configuration. 

Symfony Components are decoupled libraries for PHP applications. Symfony provides components ranging from the simple feature, say file system, to advanced features, events, container, and dependency injection. 

```bash
## Run the command in cmd to install Symfony using Composer 

composer create-project Symfony/website-skeleton myFirstProject

## The website is optimized for conventional web applications. If you are creating microservices, console applications, or APIs, consider using the much simpler skeleton project: 

composer create-project symfony/skeleton myFirstProject 

cd myFirstProject 

composer require symfony/web-server-bundle --dev 

## Running your Symfony Application 

>> Move to new project and start the server: 

cd myFirstProject 

php bin/console server:run 

## Open your browser and navigate to localhost.
```

### Differentiate between Symfony 4 and Symfony 5.
Symfony 4 introduced Flex, a new way of managing Symfony applications and bundles, making it more lightweight and customizable. Symfony 5 further improved performance and introduced new features like Mailer and HttpClient.

### What is Symfony Flex?
Symfony Flex is a Composer plugin used for managing Symfony applications and bundles. It simplifies the installation, configuration, and removal of Symfony components by automatically handling dependencies and configuration updates.

### Explain Dependency Injection in Symfony.
Dependency Injection (DI) is a design pattern used in Symfony for managing class dependencies. It allows the inversion of control by injecting dependencies into a class rather than creating them internally, promoting loose coupling and easier testing.

### What is Symfony Console Component?
The Symfony Console Component provides a simple and consistent interface for building command-line applications in Symfony. It allows developers to create commands with options and arguments, making it easy to automate tasks and interact with the application via the terminal.

### How does Routing work in Symfony?
Routing in Symfony maps URL patterns to controller actions. Routes are defined in configuration files or annotations, and Symfony matches incoming requests to the corresponding controller based on the defined routes.

### Explain Twig in Symfony.
Twig is a flexible and secure template engine used in Symfony for rendering views. It provides a syntax similar to HTML and allows developers to write reusable and maintainable templates by separating presentation logic from business logic.

### What are Symfony Events and Event Listeners?
Symfony Events allow developers to hook into the application’s lifecycle and execute custom code at specific moments. Event Listeners are callback functions registered to listen for specific events and perform actions accordingly.

### Explain Symfony Forms.
Symfony Forms provides a convenient way to create and handle forms in Symfony applications. They offer features like form rendering, data transformation, validation, and CSRF protection, making it easy to build complex forms with minimal boilerplate code.

### What is Doctrine in Symfony?
Doctrine is an Object-Relational Mapping (ORM) library used in Symfony for database abstraction and manipulation. It provides a set of powerful tools for working with databases, including entity mapping, query building, and schema migration.

### How to handle Authentication and Authorization in Symfony?
Symfony provides security components for handling authentication and authorization. Authentication is achieved through various methods like form-based authentication, HTTP Basic/Digest authentication, and OAuth. Authorization is controlled using access control rules defined in security configuration.

### Explain Symfony Cache Component.
The Symfony Cache Component provides a flexible and efficient way to cache data in Symfony applications. It supports multiple cache adapters like Filesystem, Memcached, Redis, and APCu, allowing developers to improve performance by caching frequently accessed data.

### What are Symfony Services?
Symfony Services are reusable objects managed by the Symfony Dependency Injection Container. They encapsulate business logic or functionality and are configured as services in the Symfony application, promoting modularity and maintainability.

### How to handle Error Handling and Logging in Symfony?
Symfony provides robust error handling and logging mechanisms for identifying and debugging issues in applications. Errors and exceptions are logged to files or other logging systems like Monolog, allowing developers to monitor and troubleshoot application behavior effectively.

### Which method of Kernel class is used to enable bundles in Symfony?
In Symfony, the registerBundles() method of the Kernel class is used to enable bundles.

### In Symfony### which is the default routing configuration file?
Following is the default configuration file: app/config/routing.yml

### What are the advantages of using Symfony?
The main advantages of using the Symfony framework are given below - 

- It is time-efficient
- The MVC pattern is followed
- Provides extreme flexibility to users
- It is user-friendly as well as stable a framework
- It is easy to use and customize
- It has a large community.

### What are the components in Symfony?
Symfony Components are reusable libraries that are the foundation of almost all main PHP projects. The objectives of this open-source software are maintaining web apps, replacing repetitive coding, speeding up the creation, building vigorous enterprise apps, and offering developers complete configuration control.

### What is meant by bundles in Symfony?
A Symfony bundle collects the folders and files that are arranged in a specific manner. You can use these bundles in several applications. Usually, the "AppBundle" contains the package of the main application. There are app-specific bundles available also, like BlogBundle. You can't share these bundles with other apps. But you can use the general bundle as a particular part of your app. The common files and structure of bundles are as follows:

- Controller - All the controllers are collected here.
- DepedencyInjection - It contains the code of dependency injection and extension classes also.
- Tests - It contains all the unit test files connected to the bundle.
- Resources/config - The configurations related to the bundle are contained here, such as routing.yaml.
- Resources/public - Static resources like JavaScripts, stylesheets, images, etc., are contained in this.
- Resources/view - The view templates are collected in this.

### What do you mean by Serializer in Symfony?
Serializer is used to convert a PHP object into other formats like JSON, Binary, XML, etc. You can do this by creating a Serializer object and adding normalizers, encoders, etc.

### Describe Twig.
Tasks like sandboxing, whitespace control, and automatic HTML escaping can be done using a powerful templating language of Symfony called Twig.

### Which Template Engine is supported by Symfony?
The default template engine supported by Symfony is Twig. Plain PHP code can also be used if required.

### What is a Controller in Symfony?
In Symfony, a controller is a class method used to obtain information from the HTTP request and return an HTTP response in the form of an HTML page, an XML document, a ###error, a redirect, a JSON array, etc.

### When does Symfony deny access to the user?
When a web application is accessed by an unauthorized user, it denies the user access by throwing a ###HTTP status and error page.

### Explain Bundles in Symfony?
Symfony bundles are the same as packages or plugins in other frameworks or CMS ( Content Management Systems). It is a group of directories organized in a specific structure. Pre-built features packaged in third-party bundles are flexible and can be utilized to create or distribute our own bundles.

### Discuss innovations in Symfony###
Following are the innovations provided by Symfony###

- Dependency Injection Pattern
- Easy debugging 
- Everything is bundled
- High Security

### What are Annotations in Symfony?
Annotations in the Symfony framework configure validation and map the doctrine details. It can be used for templates, cache, routing, securing, etc. Two additional bundles can also be used for this. They are SensioFrameworkExtraBundle and JMSSecurityExtraBundle.

### Describe Environment in Symfony.
An Environment is a group of configurations used to run applications. By default, it has two environments:

- `dev:` It is well suited for developing applications locally
- `prod:` It is used for the applications on production

### Describe the method used to handle an Ajax request on the server side.
"To handle an Ajax request in the server, we use the current request object’s isXmlHttpRequest() method; if it can handle an Ajax request, it will return true, or else it will return false. 

Following is the snippet to illustrate the same:
```php
if ($request->isXmlHttpRequest()) {    
    // Ajax request    
} else {    
    // Normal request    
}
```

### How to check the installed version of Symfony?
"If you have access to the command line, then the installed version can be found in the PHP bin/console of the command.

If you wish to install this version of Symfony, you can do it in this file: symfony/src/Symfony/Component/HttpKernel/Kernel.php

### How can you clear the cache in Symfony?
"We can use the cache:pool: clear command to clear the cache in Symfony. This will delete all the data stored in the project storage directory. We have three default cache clearers in Symfony:

- system_clearer
- app_clearer
- global_clearer

### What tasks does the Symfony controller perform?
"A Symfony controller can virtually perform anything within this framework. From redirects and transfers to accessing core services to rendering templates, Symfony's controllers can be used for a variety of basic tasks.

### Write the method used to get the Request Parameters?
Symfony provides a Request class which is used to request information. It is an object-oriented representation of the HTTP request message. Use the following method: 

`$request->query->get('parameter_name')`

### What do you mean by form Helper? List the form helper functions in Symfony?
"To write form inputs in templates, we use form helpers. They are used mostly for complex elements like drop-down lists, dates, and rich text. Some of the helper functions are:

- form_tag()
- textarea_tag()
- select_tag()
- options_for_select()
- checkbox_tag()
- input_tag()
- input_file_tag()
- input_password_tag()
- input_hidden_tag()
- submit_tag()

### What is the syntax for EmailType in Symfony?
"The EmailType field in Symfony is a text field. The HTML###input type="email"/> tag is used to render it.

Following is the syntax for EmailType in Symfony:
```php
use Symfony\Component\Form\Extension\Core\Type\EmailType; 
$builder->add('token',
    EmailType::class, array(    
   'data' => 'hello,')
);
```
You can use the type as an array if you want to provide extra attributes to an HTML field. The data field overrides the initial value for all the fields.

### Name the cache adapters available in Symfony.
- FileSystem Cache Adapter
- PHP files Cache Adapter
- APCu Cache Adapter
- Array Cache adapter
- Redis Cache Adapter

### Does Symfony support web servers? If yes, name them.
- WAMP (Windows)
- XAMP (Multi-platform)
- LAMP (Linux)
- MAMP (Macintosh)
- Nginx (Multi-platform)
- Microsoft IIS (Windows)
- PHP built-in development web server (Multi-platform)

### List the rules you need to follow while creating methods inside the controller.
"The rules that should be followed in Symfony for creating a method within the controller:

The "Action" suffix must be used in Action methods.
Long controller methods should not be used; refactoring it
The public method should be used in only actions method
Valid response objects should be used by the action method

### List some benefits of Symfony.
- Development is fast
- Works on MVC design patterns
- It is expandable
- Better user control 
- User flexibility is unlimited 
- It is a feasible and stable framework

### How to create a controller in Symfony?
A controller in Symfony is used to take information from the HTTP request and returns an HTTP response in the form of a Response object.

In Symfony, a controller can be created by extending AbstractActionController class. An example is shown below:
```php
use Zend\Mvc\Controller\AbstractActionController;  
use Zend\View\Model\ViewModel;    
class IndexController extends AbstractActionController {  
   public function indexAction() {  
      return new ViewModel();  
   }  
}
```

### Which method is used to set and get the session in Symfony?
"To store user information between requests, Symfony offers a session object and a number of utilities. We can use the method - "SessionInterface" to set and get sessions. $session->set() is used to set an attribute, and $session->get() is used to get an attribute that some controller already sets.

```php
public function sessionAction(SessionInterface $session)  
{  
    // to set an attribute
    $session->set('user_id,' ###;  
    // to get the attribute that a controller already sets
    $user_id = $session->get('user_id');    
}
```

### What is the syntax to install Symfony 
"Use the following syntax to install Symfony###

For Windows:

`php -r "readfile('https://symfony.com/installer');" > symfony`

For Mac and Linux:

```bash
sudo mkdir -p /usr/local/bin    
sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony    
sudo chmod a+x /usr/local/bin/symfony
```

### What is the method to create action in the Symfony###controller?
"In order to serve HTML, you will need to render a template, which can be done using the render() method. It is used to put the content to be rendered in a Response object.

To create an action in the Symfony###controller, we need to implement the following method:
```php
public function indexAction()  
{
    return $this->render('user/index.html.twig', [ ]);  
}
```

### Best Practice SEO-friendly URL structure
- Screaming Frog URLs  Redirect all old URL to new website (provided by Christoph)
- Website and content structure? (clarifies with Hong)

### Best Practices
- No dates, numbers, stop words
- Speaking url
- Short as possible
- Reduction of hierachy levels
- Fewer folders
- No several URL for same content
- URL written in lowercase
- Hyphens instead of underscores
- Beware of trailing slash!!!

### How can the request parameters be obtained in Symfony
In Symfony, we can get the request parameters using the following method:
```php
$request = $this->container->get('request');  
$name=$request->query->get('name')
```

### What is the syntax for checking a valid email address?
The following syntax is used for checking valid email addresses:

```php
use Symfony\Component\Validator\Constraints as Assert; 
class Student {     
    /**   
    * @Assert\Email(   
    * message = "The email '{{ value }}' is not a valid email.",   
    * checkMX = true   
    * )
    */     
    protected $email;
} 
```

### What is the default port of Symfony?
80 is the default port in Symfony.

### How can an action be created in the controller of Symfony 
Action in Symfony ###can be created in the Symfony controller with the help of given commands:
```php
public function indexAction()    
{    
    return $this->render('user/index.html.twig', [ ]);    
}
```

### In Symfony, is the directory structure of bundles fixed?
No, the directory structure of bundles is not fixed in Symfony.

### Which file of routing configuration is set as default in Symfony 
app/config/routing.yml is the file of routing configuration that is set as default in Symfony 

### What is  Session management in Symfony?

Session management is handled by Symfony `HttpFoundation` component, that has a very powerful and flexible session subsystem which is designed to provide session management through a simple object-oriented interface using a variety of session storage drivers.

Typically, by default, it will save to file in directory `/var/cache/sessions`

### #Session Attributes 
Session attributes are stored in a bag, and acts like an array.

- `set()` : Sets an attribute by key.
- `get()` : Gets an attribute by key.
- `all()` : Gets all attributes as an array of key => value.
- `has()` : Returns true if the attribute exists.
- `replace()` : Sets multiple attributes at once: takes a keyed array and sets each key => value pair.
- `remove()` : Deletes an attribute by key.
- `clear() `: Clear all attributes.

### #Bag Management
- `registerBag()` : Registers a SessionBagInterface.
- `getBag()` : Gets a SessionBagInterface by bag name.
- `getFlashBag()` : Gets the FlashBagInterface. This is just a shortcut for convenience.

### #Basic Example
use `Symfony\Component\HttpFoundation\Session\Session;`
```php 
$session = new Session(); 
$session->start(); 
// setting and getting sessions 
$session->set('username', 'digvijaytiwari'); 
$session->get('name');
```