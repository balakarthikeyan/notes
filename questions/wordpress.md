## 1. Describe the process of creating a custom REST API endpoint in WordPress ? 

It is a powerful way to extend the capabilities of your website or application. You can customize the API to expose new data or to integrate with external software. Here's a step-by-step guide on how to add a custom endpoint:

- `Hook Into the REST API Initialization:` Hook into the REST API initialization using the `rest_api_init` action. This is where you will register your custom routes.
- `Register a New Route:` Use the `register_rest_route()` function within your hook callback to define a new endpoint. This function takes three parameters: a namespace, a route pattern, and an array of options that define request methods and callback functions.
- `Define a Callback Function:` For the route, you need to specify a callback function that will run when the endpoint is requested. This function should handle retrieving or manipulating the data you want to serve.
- `Return a Response:` Construct and return a `WP_REST_Response` object within your callback function. This standardized response object will ensure your data is properly formatted for JSON transmission.

## 2. How can you create a custom database table in WordPress ?

You can use the $wpdb object, which is an instance of the WordPress database class. Use the $wpdb->query() method to execute SQL queries for creating the table.

## 3. Describe the process of creating a Gutenberg block with dynamic content ? 

It involves both JavaScript and PHP.

- `Register the Block in JavaScript:` Use the `@wordpress/blocks` package to define the structure and behavior of the block within your JavaScript file. This includes setting up attributes that your block will use to manage content.
- `Enqueue Block Assets:` Enqueue your JavaScript and any additional CSS as needed, to ensure your block editor has the necessary resources to function.
- `Define Server-Side Rendering with PHP:` Create a PHP function that will handle rendering the block on the server. This function will use attributes passed from the block to generate the final output. Unlike static blocks, dynamic blocks do not define their markup in the save function in JavaScript. Instead, they return null or use the `<InnerBlocks.Content />` component if they include nested blocks.
- `Register Block Type with PHP:` Use the `register_block_type()` function in PHP to register your block. As part of the registration, you will provide it with the name of the rendering function created in the previous step. WordPress knows to call your PHP function to get the content when it's time to display the block on the front end.
- `Implement AJAX or API Calls (if necessary):` If your dynamic content needs to be updated or fetched in real time, `wp.apiFetch` or AJAX calls can be used to retrieve up-to-date information from the server. This would typically happen within the block's edit function.

4. Explain the role of the WP_Object_Cache class in WordPress ?

The `WP_Object_Cache` class in WordPress provides a caching mechanism to store and retrieve often-used data, reducing the need for repetitive database queries. It helps improve site performance by storing data in memory and preventing unnecessary database requests.

Plugins and themes can use this class to cache data and improve the overall speed of a WordPress site.

5. What is a widget callback function ?

A widget callback function in WordPress is the core logic that determines what content or functionality a widget displays in sidebar areas Widgets are registered using register_widget.

- `Callback Function:` This function, defined during registration, generates the widget's content.
- `Usage:` When the widget is added to a sidebar, the callback function is called to display its content.

6. What are WordPress taxonomies ?

WordPress taxonomies are a way to categorize and classify content. The two main built-in taxonomies are "Categories" and "Tags." 
You can also create custom taxonomies for more specific content organizations.

7. Explain the concept of headless WordPress and its benefits ?

Headless WordPress is an architecture where the backend (content management) and frontend (user interface) are decoupled. Content is managed via WordPress, while the front is built using modern JavaScript frameworks or static site generators. This allows for more flexibility, better performance, and easier integration with different platforms and devices.


8. How can you optimize the performance of a WordPress site ? 

- `Caching:` Utilize plugins like W3 Total Cache or WP Super Cache to serve cached content.
- `Image Optimization:` Compress and resize images to reduce page load times.
- `Minification:` Minify HTML, CSS, and JavaScript files to reduce file sizes.
- `Content Delivery Network (CDN):` Offload static assets to a CDN for faster global delivery.
- `GZIP Compression:` Enable GZIP compression to reduce file sizes during transfer.
- `Database Optimization:` Clean up unnecessary data and optimize database tables.
- `Lazy Loading:` Load images and other assets only when they are visible in the viewport.


9. What is the importance of the .htaccess file about WordPress ? 

The `.htaccess` file is important for configuring server-level settings and overrides for your WordPress site. It can control URL rewriting, redirection, security settings, and more. It's often used to create search engine-friendly URLs and improve site security by blocking unauthorized access.

10. How can you mitigate XML-RPC attacks in WordPress, and why is it essential for security hardening ?

XML-RPC attacks can be mitigated in WordPress by disabling XML-RPC functionality unless necessary. This can be achieved by adding the following code to your theme's functions.php file or using a security plugin:

`add_filter('xmlrpc_enabled', '__return_false');`

XML-RPC attacks are critical to address because they can lead to brute force attacks and DDoS attacks. By disabling XML-RPC when not needed, you reduce the attack surface. If you need XML-RPC for specific purposes, consider implementing a plugin like "Disable XML-RPC Pingback" to limit potential threats. Additionally, keeping WordPress and plugins up to date is vital, as many vulnerabilities are patched in newer versions.

11. Explain the concept of Content Security Policy (CSP) in WordPress ?

`Content Security Policy (CSP)` is a security feature that helps prevent cross-site scripting (XSS) and other code injection attacks. In WordPress, you can implement CSP by adding HTTP headers specifying which sources of content are allowed to be loaded and executed on your site. This is done in your site's server configuration or through security plugins.

CSP is crucial as it reduces the risk of XSS attacks by controlling what can be executed in the user's browser. It can block malicious scripts from running and enhance the overall security of your WordPress site by limiting potential attack vectors.

## What is REST API?
REST API (Representational State Transfer Application Programming Interface) is a newer and lightweight mode using which the developers enjoy the convenience of connecting WordPress with other applications.

## WordPress major vulnerability issues ?

- Authenticated File Delete.
- Authenticated Post Type Bypass.
- PHP Object Injection via Metadata.
- Authenticated Cross-Site Scripting (XSS).
- Cross-Site Scripting (XSS) that could affect plugins.
- User Activation Screen Search Engine Indexing.
- File Upload to XSS on Apache Web Servers.

## When a typical AJAX request to admin-ajax.php is made, it loads a few other core WordPress files to make sure the core functions are loaded.

- `/wp-load.php`
- `/wp-config.php`
- `/wp-settings.php` (loads most core files, all active plugins and themes, and the REST API)
- `/wp-admin/includes/admin.php`
- `/wp-admin/includes/ajax-actions.php`
After loading these files, WordPress calls the `admin_init` hook

## New core functions are registered on this hook in WordPress 6.3:

- handle_legacy_widget_preview_iframe
- wp_admin_headers
- default_password_nag_handler
- WP_Privacy_Policy_Content::text_change_check
- WP_Privacy_Policy_Content::add_suggested_content
- register_setting
- add_privacy_policy_content
- send_frame_options_header
- register_admin_color_schemes
- _wp_check_for_scheduled_split_terms
- _wp_check_for_scheduled_update_comment_type
- _wp_admin_bar_init
- wp_schedule_update_network_counts
- _maybe_update_core
- _maybe_update_plugins
- _maybe_update_themes

## How REST endpoints are handled ?

REST endpoints are handled by the WordPress Rewrite API, the request is passed to /index.php, which then loads the rest of WordPress normally.

- /index.php
- /wp-blog-header.php
- /wp-load.php
- /wp-config.php
- /wp-settings.php (loads most core files, all active plugins and themes, and the REST API)

## What are Hooks?
Hooks, are basically placeholders developers, to insert their own custom code for processing as a page is loading.

`Filter hooks`, let you intercept and modify data as it's being processed. Basically, filters let you manipulate data coming out of the database before going to the browser, or coming from the browser before going into the database

`Action hooks`, lets you add extra functionality at specific points in the processing and loading of a page.

## What is Noodp & Noydir ?

* `Noodp` : No Open Directory Project
* `Noydir` : No Yahoo Directory

The Open Directory Project and Yahoo Directory used to be massive online directories that contained links to websites along with their titles and descriptions.

Noodp and Noydir are meta tags that are added to the html code of pages.

Meta robots tags are code that is hidden from your site's visitors but provides some instructions to search engine bots.

Noodp looks like this: `<meta name="robots" content="noodp" />`
Noydir looks like this: `<meta name="robots" content="noydir" />`

## Composer With WordPress?

`Improved Organization:` By declaring all your dependencies (plugins, themes, libraries) in a central location (composer.json file), you keep your project clean and well-documented. This makes it easier for you and your team to understand what the project relies on.
`Simplified Updates:` Updating plugins and libraries can be a risky business – incompatible versions can break your site. Composer allows you to easily update all dependencies while ensuring they remain compatible with each other and your WordPress core version.
`Version Control and Consistency:` Composer lets you specify the exact version of each dependency needed for your project. This ensures everyone working on the project uses the same codebase, preventing unexpected bugs or functionality issues that might arise from version discrepancies.
`Reduced Repository Size:` Composer eliminates the entire codebase of plugins and themes within their project repository, by simply referencing the required libraries, keeping your repository lean and efficient.

## Introduction To Composer And Packagist
To import and manage own and third-party components into our PHP projects, we can rely on the PHP-dependency manager Composer which by default retrieves packages from the PHP package repository Packagist 

## Introduction To WPackagist 
WPackagist is a PHP package repository. It contains all the themes and plugins hosted on the WordPress plugin and theme directories, making them available to be managed through Composer.

## What is a child theme?
The extension of a parent theme is a child theme. In case you make changes to the parent theme, then any update will undo the changes. When working on a child theme, the customizations are preserved on an update.

## What is usermeta function in Wordpress?
The usermeta function is used to retrieve the metadata of users. It can return a single value or an array of metadata.

Syntax: `get_user_meta( int $user_id, string $key = '', bool $single = false )`

- `$user_id` is the required user id parameter
- `$key` is the optional parameter which is the meta key to retrieve. By default, it returns data for all key values.
- `$single` is an optional parameter that tells whether the single value will return. By default, it is false.