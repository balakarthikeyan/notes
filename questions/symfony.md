### What is Symfony?
> Symfony is a high-performance PHP framework that follows the Model-View-Controller (MVC) architectural pattern. It provides reusable components and libraries for accelerating the development process of web applications.

### What are the key components of the Symfony framework?

> Symfony is built on a set of decoupled and reusable components such as Routing, Templating (Twig), Forms, Dependency Injection, and ORM (Doctrine).

### Comparison with other PHP frameworks like Laravel?

Laravel is an open-source framework. It observes a model-view-controller design pattern and uses components of different frameworks to create a web application. It is famous for developing simple applications and reducing the development time framework. 

Symfony is an open-source PHP project like Propel, Doctrine, PHPUnit, Twig, and Swift Mailer. Symfony has components like Symfony YAML, Symfony Event Dispatcher, Symfony Dependency Injector, and Symfony Templating. Symfony has risen as a more reliable and mature framework. It is used for complex enterprise projects cause it is very scalable, maintainable, and well structured. 

### What is the feature of Symfony?

Symfony is a PHP framework that aims to accelerate the design and maintenance of web applications and replace recurrent coding tasks.

Rails-like programming method, clean design, and code readability. Symfony offers Ajax helpers, plugins, and an admin generator interface, which renders the programming of complete applications truly easy. The developers hence can concentrate on applicative logic without wasting time writing infinite XML configuration files. 

Symfony can be utilized to create powerful applications in an enterprise context as it permits developers to test, debug, and document projects, giving them complete control over configuration and customization - from the directory structure to the foreign libraries. Symfony utilizes the Model-View-Controller design pattern, splitting the business logic from the presentation layer. 

### What are the benefits of Symfony?

> Symfony PHP framework is effortless to use gratitude to its Ruby-On-As a development framework. Symfony simplifies a developer's work, as it doesn't require writing extensive codes to create web applications.

### Advantages of Symfony Framework: 

- `Fast-Paced Development:` Time requires fast and high-performance applications. The developer community needs the flexibility to create great applications. Symfony 2 has the ability for performance optimization of applications. It ingests less memory and authorizes developers to develop applications at a considerable speed. 
- `Flexibility:` Symfony is fully adjustable to a developer's unique requirements, with independent and configurable components. Developers can easily create complicated applications with multiple functional features with their event dispatcher and dependency injector. The required functionalities can be built brick by brick and carry out the development task at their own pace. 
- `Expandable Framework:` Everything is present as a bundle in Symfony. Each bundle adds unique functionality and adds to its expansion. A developer reuses a bundle in another project and shares it with the community. Bundles can be reused to change everything within Symfony without the necessity to reconfigure the complete framework. It allows changing the core itself to help expand the framework. 
- `Stable Framework:` The company's developers offer support for three years, although they can provide you lifetime support on security-related issues. The company plans to furnish more stability by assuring compatibility among its minor versions. This guarantees the stability and sustainability of the application developed in any version of the framework. Moreover, it can also deliver compatibility with the public APIs for more functional advantages. 
- `Comfort & Convenience:` The Framework delivers a highly functional development environment and conveys to developers the desired level of pleasure and ease. Instead of concentrating on minor functionalities, the framework facilitates developers to focus more on the core functional features of an application. This improves a developer's productivity and makes their task smooth and error-free. Symfony devices, such as the Web Debug Toolbar, bring more convenience for a developer to address errors and security-related issues. 
- `User Friendly:` Symfony is an easy-to-use framework for any developer and is highly convenient for beginners and advanced users, with a host of documentation and support from the company and the community. With an acceptable level of professional support, a newbie can quickly learn the best practices of Symfony Development to help create agile and performance-driven solutions. 

### How can you enhance the performance of a Symfony application?

> Performance can be improved by optimizing the code, using caching mechanisms like Varnish or Symfony's built-in HTTPCache, and optimizing database queries.

### How does Symfony support API development?

> Symfony has built-in tools and bundles like FOSRestBundle and API Platform that facilitate the creation of RESTful and GraphQL APIs.

### What's the role of the Symfony profiler?

> The Symfony profiler is a development tool that provides detailed insights into an application's performance, database queries, and other vital metrics.

###  What are the components in Symfony?

Symfony Components are a collection of decoupled and reusable PHP libraries. It is open-source software that desires to quicken or speed up the creation and maintenance of web applications, substitute repetitive coding tasks, create powerful and robust applications in an enterprise context, and give developers complete control over the configuration. 

Symfony Components are decoupled reusable libraries for PHP applications. Symfony provides components ranging from the simple feature, say file system, to advanced features, events, container, and dependency injection. 

```bash
## Run the command in cmd to install Symfony using Composer 

composer create-project symfony/website-skeleton myFirstProject

## The website is optimized for conventional web applications. If you are creating microservices, console applications, or APIs, consider using the much simpler skeleton project: 

composer create-project symfony/skeleton myFirstProject 

cd myFirstProject 

composer require symfony/web-server-bundle --dev 

## Running your Symfony Application 

php bin/console server:run 

## Open your browser and navigate to localhost.
```

### What is a Controller in Symfony?

> In Symfony, a Controller is responsible for handling HTTP requests, processing logic, and returning a response, typically a rendered view.

### What are Symfony Services?

> Symfony Services are reusable objects managed by the Symfony Dependency Injection Container. They encapsulate business logic or functionality and are configured as services in the Symfony application, promoting modularity and maintainability.

### What is the role of the Service container in Symfony?

> The service container in Symfony is a dependency injection container that manages object instances and their dependencies.

### Differentiate between Symfony 4 and Symfony 5.
> Symfony 4 introduced Flex, a new way of managing Symfony applications and bundles, making it more lightweight and customizable. 

> Symfony 5 further improved performance and introduced new features like Mailer and HttpClient.

### How do you manage assets in a Symfony project?

> Symfony offers an `Asset component` and Webpack Encore to manage and compile assets like CSS, JavaScript, and images.

### How does Symfony handle sessions and cookies?

> Symfony provides a `Session component` to manage user sessions and a Cookie class for handling cookies.

### How do you handle translations and internationalization in Symfony?

> Symfony offers a `Translation component` that provides tools for internationalization and translations.

### How can you handle errors and exceptions in Symfony?

> Symfony provides an `ErrorHandler component` that manages errors and exceptions, allowing developers to create custom error pages and log issues.

### How do you integrate third-party libraries in Symfony?

> Third-party libraries can be integrated into Symfony projects using Composer and then utilized within the application's code.

### How do you manage configurations in a Symfony project?

> Symfony uses YAML, XML, or PHP files to manage configurations, and the configuration values can be accessed using the service container.

### What is Symfony Flex?

> `Symfony Flex component` is a Composer plugin used for managing Symfony applications and bundles. It simplifies the installation, configuration, and removal of Symfony components by automatically handling dependencies and configuration updates.

### What are Symfony Recipes?

> `Symfony Recipe` are instructions defined by bundles to automate configuration tasks. It is a tool that simplifies and automates the setup of packages in a Symfony project. It uses a `manifest.json` file to define how a package integrates into your project via Symfony Flex.

### Explain Dependency Injection in Symfony?

> `Dependency Injection` (DI) is a design pattern used in Symfony for managing class dependencies. It allows the inversion of control by injecting dependencies into a class rather than creating them internally, promoting loose coupling and easier testing.

### What is Symfony Console?

> The `Symfony Console component` provides a simple and consistent interface for building command-line applications in Symfony. It allows developers to create commands with options and arguments, making it easy to automate tasks and interact with the application via the terminal.

### How does Routing work in Symfony?

> Routing in Symfony maps URL patterns to controller actions. Routes are defined in configuration files or annotations, and Symfony matches incoming requests to the corresponding controller based on the defined routes.

### Explain Twig in Symfony?

> Twig is a flexible, fast and secure template engine used in Symfony for rendering views. It provides a syntax similar to HTML and allows developers to write reusable and maintainable templates by separating presentation logic from business logic.

### Describe Twig?
Tasks like sandboxing, whitespace control, and automatic HTML escaping can be done using a powerful templating language of Symfony called Twig.

### What are Symfony Events and Event Listeners?

> `Symfony Event component` allow developers to hook into the application's lifecycle and execute custom code at specific moments. Event Listeners are callback functions registered to listen for specific events and perform actions accordingly.

### How do you handle events and listeners in Symfony?

> Symfony provides an `EventDispatcher component`, which allows developers to create, dispatch, and listen to events throughout the application.

### Explain Symfony Forms?

> Symfony provides a `Form component` a convenient way to create, render and handle forms in Symfony applications. They offer features like form rendering, data transformation, validation, and CSRF protection, making it easy to build complex forms with minimal boilerplate code.

### What is Doctrine in Symfony?

> Doctrine is an `Object-Relational Mapping (ORM)` library used in Symfony for database abstraction and manipulation. It provides a set of powerful tools for working with databases, including entity mapping, query building, and schema migration.

### How can you optimize database queries in Symfony?

> Using Doctrine, developers can utilize the `DQL (Doctrine Query Language)`, ensure proper indexing, and use caching mechanisms to optimize queries.

### How to handle Authentication and Authorization in Symfony?

> Symfony provides `Security component` for handling authentication, authorization and CSRF protection. Authentication is achieved through various methods like form-based authentication, HTTP Basic/Digest authentication, and OAuth. Authorization is controlled using access control rules defined in security configuration.

### How do you set up authentication using Symfony Guard?

> `Symfony Guard` is a powerful and customizable authentication system. By creating a Guard authenticator class and configuring the security settings, developers can define custom authentication processes.

### Explain Symfony Cache?

> The `Symfony Cache component` provides a flexible and efficient way to cache data in Symfony applications. It supports multiple cache adapters like Filesystem, Memcached, Redis, and APCu, allowing developers to improve performance by caching frequently accessed data.

### How to handle Error Handling and Logging in Symfony?

> Symfony provides robust error handling and logging mechanisms for identifying and debugging issues in applications. Errors and exceptions are logged to files or other logging systems like Monolog, allowing developers to monitor and troubleshoot application behavior effectively.

### What is a Symfony bundle?
A bundle in Symfony is a structured set of files (PHP files, stylesheets, JavaScripts, images) that implement a specific feature. Bundles are reusable packages or modules in Symfony that encapsulate functionality such as controllers, models, views, configuration files, and routes. They help in organizing code and promoting code reusability across different projects.

Symfony has two types of bundles:

- Application specific bundles
- Reusable bundles

Pre-built features packaged in third-party bundles are flexible and can be utilized to create or distribute our own bundles. The common files and structure of bundles are as follows:

- `Controller` - All the controllers are collected here.
- `DepedencyInjection` - It contains the code of dependency injection and extension classes also.
- `Tests` - It contains all the unit test files connected to the bundle.
- `Resources/config` - The configurations related to the bundle are contained here, such as routing.yaml.
- `Resources/public` - Static resources like JavaScripts, stylesheets, images, etc., are contained in this.
- `Resources/view` - The view templates are collected in this.

Usually, the `AppBundle` contains the package of the main application. There are app-specific bundles available also, like `BlogBundle`. You can't share these bundles with other apps. 

### Which method of Kernel class is used to enable bundles in Symfony?

> the `registerBundles()` method of the Kernel class is used to enable bundles.

### Which is the default routing configuration file?

> the default configuration file: `app/config/routing.yml`. Use native attributes in PHP 8 to configure routes, and On PHP 7, use annotations

### What do you mean by Serializer in Symfony?

> Serializer is used to convert a PHP object into other formats like JSON, Binary, XML, etc. You can do this by creating a Serializer object and adding normalizers, encoders, etc.

### What are Annotations in Symfony?

> Annotations in the Symfony framework configure validation and map the doctrine details. It can be used for templates, cache, routing, securing, etc. Two additional bundles can also be used for this. They are `SensioFrameworkExtraBundle` and `JMSSecurityExtraBundle`.

### Describe Environment in Symfony.

> An Environment is a group of configurations used to run applications. By default, it has two environments:

- `dev:` It is used for developing applications locally
- `prod:` It is used for the applications on production

### How can you clear the cache in Symfony?
We can use the `cache:clear` command to clear the cache in Symfony. This will delete all the data stored in the project storage directory. We have three default cache clearers in Symfony:

- system_clearer
- app_clearer
- global_clearer

### Name the cache adapters available in Symfony?
Cache adapters are the actual caching means to store the data in the filesystem, in a database, etc.

Symfony has five cache adapters are as follows: 
- FileSystem Cache Adapter
- PHP files Cache Adapter
- APCu Cache Adapter
- Array Cache adapter
- Redis Cache Adapter

### Does Symfony support web servers?

- WAMP (Windows)
- XAMP (Multi-platform)
- LAMP (Linux)
- MAMP (Macintosh)
- Nginx (Multi-platform)
- Microsoft IIS (Windows)
- PHP built-in development web server (Multi-platform)

### What is Session management in Symfony?

Session management is handled by Symfony `HttpFoundation` component, that has a very powerful and flexible session subsystem which is designed to provide session management through a simple object-oriented interface using a variety of session storage drivers.

Typically, by default, it will save to file in directory `/var/cache/sessions`

#### Session Attributes 
Session attributes are stored in a bag, and acts like an array.

- `set()` : Sets an attribute by key.
- `get()` : Gets an attribute by key.
- `all()` : Gets all attributes as an array of key => value.
- `has()` : Returns true if the attribute exists.
- `replace()` : Sets multiple attributes at once: takes a keyed array and sets each key => value pair.
- `remove()` : Deletes an attribute by key.
- `clear() `: Clear all attributes.

#### Bag Management
- `registerBag()` : Registers a SessionBagInterface.
- `getBag()` : Gets a SessionBagInterface by bag name.
- `getFlashBag()` : Gets the FlashBagInterface. This is just a shortcut for convenience.

#### Basic Example
```php 
use Symfony\Component\HttpFoundation\Session\Session;
// Creating Session in Symfony 
$session = new Session(); 
$session->start(); 
// Setting Session in Symfony
$session->set('username', 'balakarthikeyan'); 
// Getting session in Symfony 
$user_name = $session->get('username');
// Removing / Destroying session in Symfony 
$session->remove('username');
```

### When Symfony denies user access?

In Symfony, an `AccessDeniedException` can be thrown when access is denied to the user. Symfony will handle this exception. 

Symphony denies access to the user for the below reasons:
- The user is not authenticated and is redirected to the login page, or there is 401 Unauthorized response 
- The user is authenticated but doesn't have required permissions; a 403 Forbidden response is induced. 

### What do you know about descriptors in Symfony?

> Descriptors are objects in Symfony that can be used to generate documentation and information on the console Apps.

### What is Serializer in Symfony?

The Serializer component turns objects into a specific format (XML, JSON, YAML). An array is an intermediate between objects and serialized contents. Encoders will deal with turning specific formats into arrays and vice versa.

We can change the data type object, array to another data type like json or xml using the class 

```php
use Symfony\Component\Serializer\Serializer; 

$users = $userRepository->findAll(); 
$jsonContent = $serializer->serialize($users, 'json'); 

// And to return it to the previous data type
$jsonContent = $serializer->deserialize($users, 'json'); 
```