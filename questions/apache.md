### What is Apache ?
- **Ans:** Apache is a free and open-source web server that is used to serve web pages and other content over the internet. Its primary function is to listen for incoming requests from clients and respond with the appropriate web page or other content.

### What are the key features of Apache ?
- **Ans:** Some of the key features of Apache include its ability to handle multiple requests simultaneously, its support for a wide range of programming languages, its flexibility and extensibility through modules, and its reliability and stability.

### What is a difference between Apache and Nginx web server ?
Nginx is event-based web server where Apache is process based
Nginx is known for better performance than Apache
Apache supports a wide range of OS where Nginx doesn’t support OpenVMS and IBMi
Apache has a large number of modules integration with backend application server where Nginx is still catching up
Nginx is lightweight and capturing the market share rapidly. If you are new to Nginx, then you may be interested in checking out my articles on Nginx.

### What is the configuration file for Apache and where is it located ?
- **Ans:** The main configuration file for Apache on Linux is typically named httpd.conf and is usually located in the `/etc/httpd/conf/` directory.

### How can you start and stop the Apache web server ?
- **Ans:** To start the Apache service, you would run the command `sudo systemctl start httpd`, and to stop it, you would run `sudo systemctl stop httpd`.

### What is the DocumentRoot directive in Apache ?
- **Ans:** The DocumentRoot directive in Apache specifies the directory on the server where web pages and other content are stored. It’s also called as WebRoot.
`Default DocumentRoot location is **/var/www/html**`

### What is the purpose of the .htaccess file in Apache ?
- **Ans:** The .htaccess file in Apache is a configuration file that is used to customize the behavior of the web server for a specific directory or set of directories. It can be used to control access to resources, set custom error pages, enable server-side scripting, and much more.

### How can you enable directory listing in Apache ?
- **Ans:** To enable directory listing in Apache, you can use the Options directive in the main configuration file or in an .htaccess file.
`Options +Indexes`. This will cause Apache to display a list of all files and subdirectories in that directory.

### What is a server signature in Apache and ### How can it be disabled ?
- **Ans:** A server signature in Apache is a line of text that is included in the HTTP header of a server response, indicating the name and version number of the web server software.To disable the server signature in Apache, you can add the following line to the main configuration file or an .htaccess file: `ServerSignature Off`.

### What is an error log in Apache ?
- **Ans:** An error log in Apache is a file that contains information about errors and other events that occur while the server is running. path: `/var/log/httpd/`

### How can you password protect a directory in Apache ?
- **Ans:** To password protect a directory in Apache, you can use the `AuthType` and `AuthUserFile` directives in an .htaccess file or in the main configuration file.
```
AuthType Basic
AuthName "Restricted Area"
AuthUserFile /path/to/password/file
Require valid-user
```

### What is a MIME type in Apache and ### How is it used ?
- **Ans:** A MIME type in Apache is a string of text that identifies the format and content of a file being served by the web server. 
`AddType` directive in the main configuration file or an .htaccess file.

### How can you redirect URLs in Apache ?
- **Ans:** The `Redirect` directive is used for simple URL redirects, while the `RewriteRule` directive is used for more complex URL rewriting. 

### What is a CGI script in Apache ?
- **Ans:** A CGI script in Apache is a program or script that can be executed by the web server to generate dynamic content or perform other tasks.

### How Apache handles concurrent requests ?
- **Ans:** Apache uses a multi-process model to handle concurrent requests, with one or more child processes created to handle incoming connections. Each child process is a separate instance of the Apache web server that can process requests independently. Requests are distributed among the child processes using a technique called load balancing, which can be based on a variety of factors such as the number of requests per process, the amount of available memory, or the number of active connections.

### What are virtual hosts in Apache ?
- **Ans:** Virtual hosts in Apache are a way of serving multiple websites or domains from a single physical server. Each virtual host has its own configuration settings, including the DocumentRoot directory, ServerName, and other options. You can either create IP based or Name based on virtual hosting.

### What is mod_rewrite ?
- **Ans:** Mod_rewrite is a module in Apache that allows for powerful URL rewriting and redirection. It can be used to modify or remove parts of a URL, add query parameters or other data, or even redirect a request to a different URL entirely. Mod_rewrite uses regular expressions to match patterns in the requested URL and determine ### How to handle it.

### How to enable HTTPS in Apache?
- **Ans:** A valid SSL/TLS certificate from a trusted certificate authority, configure the server to use the certificate, and make any necessary changes to the server configuration to enable HTTPS support.

### What are the common causes of Apache performance issues ? 
- **Ans:** Apache performance issues can include high traffic volume, poorly optimized or resource-intensive web applications, insufficient server resources (such as CPU or memory), or configuration issues. 

### How can performance issues be resolved ?
- **Ans:** To resolve performance issues, you may need to adjust server configuration options such as the number of child processes or threads, optimize your web application code, use caching or load balancing solutions, or upgrade your server hardware. 

### What are Apache modules?
- **Ans:** Apache modules are software components that can be used to extend the functionality of the Apache web server. Modules can be used to add support for specific programming languages, protocols, or other features.

### What are Apache's prefork and worker MPMs (Multi-Processing Modules)?
- **Ans:** Apache's prefork and worker MPMs are two different approaches to handling concurrent requests in the web server. 
- The prefork MPM uses a multi-process model, where each request is handled by a separate process, while the worker MPM uses a multi-threaded model, where each process can handle multiple requests using multiple threads.
- The worker MPM can provide better performance and scalability in some cases, especially for high-traffic sites or those serving many small requests. 
- worker MPM  may be more complex to configure and can have higher memory usage than the prefork MPM.

### What is Apache logrotate utility?
- **Ans:** The logrotate utility in Apache is used to rotate and manage log files generated by the web server. 

### What is mod_security ?
- **Ans:** Mod_security is a module in Apache that provides web application firewall (WAF) functionality. It is designed to protect web applications from a wide range of attacks, including SQL injection, cross-site scripting (XSS), and file inclusion attacks.

### How Mod_security is been used in Apache ?
- **Ans:** Mod_security works by analyzing incoming HTTP requests and blocking or filtering requests that match known attack patterns. It can be configured to use a variety of rule sets and custom rules, and can be used in combination with other security measures such as SSL/TLS encryption.

### What is the purpose of the Apache mod_proxy module ?
- **Ans:** The mod_proxy module in Apache provides proxying and load balancing capabilities for the web server. It can be used to forward requests from one server to another, or to distribute requests among multiple servers in a load-balanced configuration.

### How mod_proxy is used ?
- **Ans:** Mod_proxy can be configured to use a variety of proxy protocols, including HTTP, HTTPS, and FTP, and can be used to provide reverse proxying functionality to protect back-end servers or to provide access to internal resources over the internet. Mod_proxy is often used in conjunction with other Apache modules such as mod_rewrite and mod_ssl.

### What is the Apache HTTP Server benchmarking tool ?
- **Ans:** The ab tool can be used to generate a high volume of requests to a web server and measure the response time, throughput, and other performance metrics. It can be useful for identifying performance bottlenecks, testing the impact of configuration changes, or comparing the performance of different web servers or configurations. 

### What are Apache security vulnerabilities ?
- **Ans:** Apache security vulnerabilities can include misconfiguration, outdated or vulnerable software components, insecure web applications, or insufficient server hardening or monitoring. To prevent security vulnerabilities, it is important to keep the Apache web server and all installed modules and software components up to date with the latest security patches and updates. 

### What is the role of caching in Apache ?
- **Ans:** Caching in Apache is a technique used to improve the performance and efficiency of web applications by storing frequently accessed data or content in memory or on disk. Caching can help to reduce the load on the server and speed up response times, especially for content that does not change frequently. Apache includes mod_cache and mod_mem_cache to implement caching for different types of content.

### What is the purpose of the Apache mod_ssl module ?
- **Ans:** The mod_ssl module in Apache provides SSL/TLS encryption capabilities for the web server. It can be used to secure data transmitted between the web server and clients, such as login credentials or payment information. 

### What is the role of the Apache reverse proxy?
- **Ans:** The reverse proxy works by intercepting incoming requests and forwarding them to a different server or resource, which may be located on a different network or behind a firewall. Reverse proxying can be implemented using the mod_proxy module in Apache.

### How to know if a web server is running?
- **Ans:** 
`ps -ef |grep httpd` Check if configured IP and port is listening on the server with `netstat -anlp |grep 80`

### How do I disable directory indexing?
`<Directory />
    Options -Indexes
</Directory>`

### Which module is required to have redirection possible?
`LoadModule rewrite_module modules/mod_rewrite.so`

### How to create a CSR?
- **Ans:** To create new CSR with a private key
`openssl req -out geekflare.csr -newkey rsa:2048 -nodes -keyout geekflare.key`

### How to put Log level in Debug mode?
- **Ans:** Often needed when you are troubleshooting the issue and wish to capture more details. Add in httpd.conf file.
`LogLevel debug`

### How to find httpd.conf file installation location?
`find / -name httpd.conf`

### How to hide server version details in the HTTP response header?
- **Ans:** Add the following in httpd.conf file and restart the webserver
```
ServerTokens Prod
ServerSignature Off
```

### How to disable trace HTTP requests?
- **Ans:** Add the following in httpd.conf file and restart the instance
`TraceEnable off`

### How to assign multiple names to virtual hosts?
- **Ans:** You can make use of the ServerAlias directive as shown below.
```
ServerName  example.com  
ServerAlias abc.com  xyz.com
```

### How to limit upload size?
```
<Directory "usr/local/apache2/uploads"> 
LimitRequestBody 9000 
</Directory>
```

### How to restrict access by IPs?
- **Ans:** You can make use of `mod_authz_core` or `mod_authz_host` modules to restrict access using the `Require` directive.
`Require ip_address_1 ip_address_2 ip_address_3`

### How will you enable the PHP scripts on the server?
- **Ans:** Install `mod_php` and run the command : `AddHandler application/x-httpd-PHP .phtml .php`

### How will you install the Apache server on Linux Machine?
- Centos: `yum install httpd`
- Debian: `apt-get install apache2.`

### What is the log level of Apache?
- **Ans:** The log level is: debug, info, warn, notice, crit, alarm, emerg, error.

### What the difference between `<Location>` and `<Directory>`?
```
<Location> is used to set element related to the URL / address bar of the web server.
<Directory> refers that the location of file system object on the server
```