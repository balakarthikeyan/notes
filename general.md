## How do you configure and handle security in transmitting data in application?
- Data transmission security is ensured through configuration and handling of encryption, authentication, and secure protocols.
- Implement encryption algorithms like SSL/TLS to secure data transmission.
- Use secure protocols like HTTPS, SFTP, or SSH for transmitting sensitive data.
- Implement authentication mechanisms like OAuth, JWT, or certificates to verify the identity of the sender and receiver.
- Apply data integrity checks like digital signatures or checksums to detect tampering.
- Implement firewall rules and network segmentation to protect against unauthorized access.
- Regularly update and patch software and libraries to address security vulnerabilities.

## What is Caching?
- Caching is a technique used to store frequently accessed data in memory for faster access.
- Caching can improve application performance by reducing the number of requests to the database or other external systems.
- Caching can be implemented at different levels such as application level, database level, and network level.
- Caching strategies include time-based expiration, least recently used (LRU) eviction, and invalidation. 
- Examples of caching technologies include Redis, Memcached.

## Explain RabbitMQ and Service Worker?
- RabbitMQ is a message broker that enables communication between distributed systems. 
- RabbitMQ is used for asynchronous messaging between applications and services. 
- It supports multiple messaging protocols such as AMQP (Advanced Message Queuing Protocol), MQTT (Message Queuing Telemetry Transport), and STOMP (Simple (or Streaming) Text Orientated Messaging Protocol).
- Service Worker is a browser API that allows background processing.
- Service Worker allows web applications to run scripts in the background, even when the browser is closed. It can be used for tasks such as caching, push notifications, and background synchronization.

## What is Monolithic Architecture?
A monolithic application is a single, unified unit where all software components are interdependent. This makes it easier to build upon and debug, and it's a good choice for smaller teams or organizations with centralized decision-making. 
However, it can be restrictive and time-consuming to modify, as changes to the code base can impact large areas. 

## What is Microservices Architecture?
A microservices architecture is made up of smaller, independent services that communicate with each other through a defined interface. This makes it easier to update, modify, deploy, or scale each service individually. Microservices are a good choice for larger, distributed teams that want decentralized decision-making.

## What is Event-Driven Architecture?
Event-driven architecture is defined as a software design pattern in which decoupled applications can asynchronously publish and subscribe to events via an event broker (modern messaging-oriented-middleware).

Event-driven architecture is a way of building enterprise IT systems that lets information flow between applications, microservices, and connected devices in a real-time manner as events occur throughout the business.

## What is Varnish Cache?
Varnish is a caching HTTP reverse proxy. It receives requests from clients and tries to answer them from the cache. If Varnish cannot answer the request from the cache it will forward the request to the backend, fetch the response, store it in the cache and deliver it to the client.

## The fundamentals of web proxy caching with Varnish?
Varnish decides whether it can store the content or not, based on the response it gets back from the backend. The backend can instruct Varnish to cache the content with the HTTP response header Cache-Control. There are a few conditions where Varnish will not cache, the most common one being the use of cookies. Since cookies indicates a client-specific web object, Varnish will by default not cache it.

This behaviour as most of Varnish functionality can be changed using policies written in the Varnish Configuration Language (VCL). 

## What is FastCGI Cache?
FastCGI Cache is a module of Nginx, a popular web server and reverse proxy. This module allows Nginx to cache the responses from FastCGI processes, such as those produced by a PHP application.

The cache is used to save the results of frequent or complex requests, so that the responses to future identical requests can be served more quickly, without the need to fully reprocess them.

## What is the DRY principle?
DRY means `"don't repeat yourself."` This principle is used in software development to keep software pattern repetition to a minimum. Instead of repetition, data architects replace redundancy with abstractions or data normalization, and the DRY principle makes it easier to maintain code.

## What is the DIE principle?
DIE in software development is an acronym that means `"duplication is evil."` The DIE principle is used in the same situations as the DRY principle and aims to ensure that software architects and developers avoid duplicating concepts. It also contributes to efficient code maintainability.

## Explain what SOLID means?
The SOLID acronym features five principles for software architect and development roles. These principles are:
- `Single responsibility:` This principle indicates that each class should be responsible for a specific part of an application.
- `Open/closed:` The open/closed principle indicates that although a module or class should be open for extension, it should be closed for modification.
- `Liskov substitution:` The Liskov substitution principle indicates that if developers use inheritance when designing an application, it should function with an object created using the parent class or a subclass.
- `Interface segregation:` The interface segregation principle indicates that software developers and architects should keep interfaces small.
- `Dependency inversion:` The dependency inversion principle suggests that a high-level class shouldn't rely on a low-level class, though both can depend on high-level abstractions.

## Explain the ACID acronym?
The ACID acronym means `"atomicity, consistency, isolation, and durability."` These database interaction properties help software developers and architects guarantee the validity of data, even when errors occur.

## Describe four best practices for performance testing?
- Defining the scope and making a plan.
- Testing components together and separately.
- Sticking to agile approaches.
- Testing early and frequently.

## Explain what sharding?
Sharding is a method software architects use to split and store one logical dataset within several databases. Such distribution in several machines facilitates the ability to store a bigger dataset.

## What cohesion means in software architecture?
When software architects divide a system into modules, cohesion measures the extent to which all elements that belong to the module are functionally related. Some of the main types of cohesion include:
- Communicational cohesion
- Functional cohesion
- Sequential cohesion
- Procedural cohesion
- Temporal cohesion
- Logical cohesion
- Coincidental cohesion

## What coupling means in software architecture.
Coupling refers to the extent to which each module, or each component, depends on another module.
If two modules are tightly coupled, they are highly dependent on each other. If they are loosely coupled, they don't rely on each other as much. If two modules are uncoupled, they are not interdependent.
There are many different examples of coupling in modules:
- No coupling
- Content coupling
- Common coupling
- Control coupling
- External coupling
- Stamp coupling
- Data coupling

## What Are Content Management Systems?

CMS is the abbreviation for `Content Management System`, is a software application that automates tasks before and after publishing digital content. Digital content in a CMS may include blog posts, press releases, guides or user manuals that a business may publish on websites, mobile applications, business portals and social media platforms. CMS provides a single interface from which multiple users can publish, edit and modify content in a structured manner. 

A CMS usually has two principal components:

- `Content management application:` A content management application (CMA) is a user interface that allows individuals to modify content for a web page.
- `Content delivery application:` A content delivery application (CDA) applies the users' changes to the web page.

Primary functions of a CMS include:

- Creating, editing and publishing content efficiently
- Formatting content suitable for multiple channels like websites and mobile applications
- Storing and retrieving assets like images and videos
- Creating access control for multiple users like writers, designers, editors and managers
- Creating reusable components of content for repetitive and common features
- Integrating with other systems like a CRM or an e-commerce system
- Setting up content workflows for necessary approvals before publishing
- Reporting on website traffic, SEO analytics and user behaviour

Types of CMS:

1. `Web content management system (WCMS)` is an application that creates, stores, manages and publishes content for a webpage. The content can include text, graphics, audio or video files. It stores content, metadata and assets in a database and uses languages like XML or JSON.
    - `Open source CMS:` A user can use or download the open source software without a fee.
    - `Commercial CMS:` Companies create CMS after obtaining licences to sell commercial CMS software, which may be faster and ready-made for use.
    - `Custom CMS:` Depending on specific requirements, a business may invest towards creating their own CMS, suited to their business practice and industry.

2. A `Component content management system (CCMS)` is a type of CMS that breaks down and manages content at a granular level. A component is a piece of structured content of any length and it can be a word, paragraph, a series of paragraphs, a picture or a video. The advantage of CCMS is that it allows content reuse, tracking and multi-channel publishing.

3. `Digital asset management (DAM)` software allows users to upload, sort, share and track digital assets belonging to a business. Digital assets include images, photos, videos, blueprints, drafts, spreadsheets, presentations and documents.


4. `Enterprise content management (ECM)` system manages content throughout its lifecycle from ideation to publishing. It helps store, archive and manage unstructured data like emails, reports and official documents.

Features Of Content Management System:

- `Search engine optimisation (SEO)` : Search engines operate on algorithms to ensure that web searches return the most relevant pages.
- `Analytics` : To know details about who visits your webpage, when they visit it and for what reasons. Provide information on visitors' demographic, the technology used, peak web traffic intervals and the most popular content. 
- `Versatility` : You can use a CMS to process content for a variety of publications.
- `Workflows` : A CMS with a well-structured workflow assigns tasks and responsibilities, and tracks the order of their - priority. 
- `Permissions` : Permission levels within a CMS can pertain to writers, editors, publishers and administrators. Each of these roles can have different permission levels to work with content in a CMS. 
- `Version control` :  A CMS typically allows you to restore a previous version of your content or site.
- `Security` : It is important to keep company information and customer data private and secure.

## What is Headless CMS ?
A headless CMS is a WordPress site that is used as a content management system (CMS) to store and manage content, but the front end of the site is built using a separate technology, such as React.js.

In Traditional CMS, the `front-end` (presentation) and the `backend` (the content management system) are tightly integrated. However, in a headless CMS architecture, the front-end and backend are decoupled, allowing a greater flexibility and scalability. This enables developers to build dynamic, fast and highly-customized user interfaces across various platforms, including websites, mobile apps and more.

12. How to develop WordPress as a Headless CMS ?
- Define your Custom Post Types:
- Use API Support:
- Configure CORS (Cross-Origin Resource Sharing):
- Develop the Front-End:
    * Custom Website: 
    * Static Site Generator (SSG):
    * Single-Page Application (SPA):
- Advantages of Using WordPress as a Headless CMS
    * Content Creation and Editing: 
    * Content Reusability:
    * Scalability:
    * Rapid Development:
    * E-commerce Integration:
    * Multilingual Capabilities:
    * Dynamic Content:
    * A/B Testing and Personalization:
    * SEO-Friendly:
    * Analytics Integration:
    * Community and Support:
- Challenges and Considerations
    * Custom Development:
    * Performance Optimization:
    * Security:
    * Costs:
    * Third-Party Integrations:
    
### Best Practice SEO-friendly URL structure
- Screaming Frog URLs 
- Redirect all old URL to new website (provided by Christoph)
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
- Beware of trailing slash

## Docker Essentials:
- `Platform:` the software that makes Docker containers possible
- `Engine:` client-server application (CE or Enterprise)
- `Client:` handles Docker CLI so you can communicate with the Daemon
- `Daemon:` Docker server that manages key things
- `Volumes:` persistent data storage
- `Registry:` remote image storage
- `Docker Hub:` default and largest Docker Registry
- `Repository:` collection of Docker images, e.g. Alpine

## Docker Scaling:
- `Networking:` connect containers together
- `Compose:` time saver for multi-container apps
- `Swarm:` orchestrates container deployment
- `Services:` containers in production