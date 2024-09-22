### What is Nginx?
- **Ans:** Nginx is a type of an open source web server which is especially used for reverse proxy, load balancer, mail proxy and for the HTTP cache. NGINX can run on Linux, Mac OS X, Solaris, AIX, HP-UX, and the BSD variants.
It was created by Igor Sysoev in 2002 and released as an open-source project in 2004. Nginx is known for its high performance, scalability, stability, and low resource consumption. It can handle millions of concurrent connections and serve static and dynamic content efficiently. 

### In what language was the Nginx software being written?
- **Ans:** The language in which the Nginx software is written is 'C' Language.

### What are the main tasks of the Nginx web server?
- **Ans:** The main task of the Nginx web server is to deploy dynamic HTTP content on a network using SCGI, Fast CGI handlers for scripts, WSGI application servers or Phusion passenger module. Nginx is also used to serve as a load balancer.

### What is the official website of Nginx web server?
- **Ans:** The official website of the Nginx web server is Nginx.org

### What are the difference between Nginx and Apache?
- Nginx uses an event-driven and asynchronous model, while Apache uses a process-based and synchronous model. This means that Nginx can handle more concurrent connections and requests with less CPU and memory usage than Apache. 
- Nginx is faster and more efficient at serving static files than Apache, while Apache has more features and modules for serving dynamic content than Nginx. 
- Nginx is the best when it comes to memory consumption and connection whereas Apache is not best in this category.
- In Nginx, a single thread is handling all of the requests whereas in Apache single thread handles a single request.
- Nginx is best when you want the load balancing. But Apache will refuse the new connection when traffic reaches the limit of the process.
- Apache provides lots of functionality as compared to Nginx.
- Nginx has a simpler and more modular configuration file than Apache, which makes it easier to maintain and customise. 

### What are the types of versions of Nginx web server?
- `Mainline` - This version of Nginx contains latest features and bug fixes that are up to date. It is also reliable but it may contain some experimental modules.
- `Stable`- This version of Nginx doesn't have the latest features but has critical bug fixes. A stable version is always recommended for the production servers.

### What is the master processor in Nginx?
- **Ans:** The master processor in Nginx is responsible for reading and validating the configuration file, creating and managing the worker processes, handling the signals and events, and binding to ports. The master processor runs as a root user and does not process any requests. It only communicates with the worker processes through shared memory and sockets.

### What is work processor in Nginx?
- **Ans:** The worker processor in Nginx is responsible for processing clients' requests. The worker processes run as a non-root user and do not communicate with each other. They only communicate with the master process and the upstream servers. The number of worker processes can be configured by the worker_processes directive in the configuration file. The optimal number of worker processes depends on the number of CPU cores and the expected load. 

### What is the use of sub_status directives in Nginx?
- **Ans:** The sub status directives is mainly used to know all the current status of Nginx. You can know the current active connections, total connections that are being accepted and all the handled current number of read, write and wait connections.

### What is the purpose of the –s with Nginx Server?
- **Ans:** The purpose of the –s with Nginx Server is to send a signal to the master process to perform a certain action. Generally to run the executable files.
- stop: to stop the Nginx server immediately 
- quit: to stop the Nginx server gracefully 
- reload: to reload the configuration file 
- reopen: to reopen the log files 

### What are the controls used in the Nginx web server? 
- **Ans:** The controls used in the Nginx web server are the directives and the blocks. 
The directives are the commands that specify the behaviour and parameters of the server. The directives are written in the form of name values. 
The blocks are the containers that group the directives according to their context and scope. The blocks are written in the form of name { ... }. 
The main types of blocks are:  
- main: the global configuration of the server 
- events: the configuration of the connection processing 
- http: the configuration of the HTTP server 
- server: the configuration of the virtual server 
- location: the configuration of the request processing based on the URI 
- upstream: the configuration of the upstream server group 

### What is the CSK10 problem for which the Nginx web server was discovered?
- **Ans:** CSK10 problem is being referred to the network socket that failed to handle a large number of clients at the same time.

### How can you add module in the Nginx server?
- **Ans:** Firstly, you have to select the Nginx module during the compile. Runtime selection of module is not being supported by the Nginx server.
Nginx does not support dynamic loading of modules, so you need to recompile the server with the module that you want to add. To do this, you need to download the Nginx source code and the module source code, and then use the --add-module option with the configure script. 
For example, to add the ngx_http_geoip_module, you can use the following command:  
```
./configure --add-module=/path/to/ngx_http_geoip_module  
make
make install
```

### What is a directive in Nginx?
- **Ans:** A directive in Nginx is a command that specifies the behaviour and parameters of the server. A directive consists of a name and a value, separated by a space, and ends with a semicolon

### What is the advantage of using a “reverse proxy server”?
- **Ans:** The reverse proxy server can hide the presence and characteristics of the origin server. It acts as an intermediate between internet cloud and web server. It is good for security reason especially when you are using web hosting services.

### Explain how you can start Nginx through a different port other than 80?
- **Ans:** To start Nginx through a different port, you have to go to /etc/Nginx/sites-enabled/ and if this is the default file, then you have to open file called “default.” Edit the file and put the port you want

```server { listen 81; }```

### In Nginx, explain how you can keep double slashes in URLs?
- **Ans:** To keep double slashes in URLs you have to use merge_slashes_off;
```
Syntax: merge_slashes [on/off]
Default: merge_slashes on
Context: http, server
```

### Explain what is ngx_http_upstream_module is used for?
- **Ans:** The ngx_http_upstream_module is used to define groups of servers that can reference by the fastcgi pass, proxy pass, uwsgi pass, memcached pass and scgi pass directives.

### Explain what is C10K problem?
- **Ans:** C10K problem is referred for the network socket unable to handle a large number of client (10,000### at the same time.

### What Are Features Of Nginx?
- Simultaneous Connections with low reminiscence
- Auto Indexing
- Load Balancing
- Reverse Proxy with Caching
- Fault Tolerance

### Define Some Special Features Of Nginx?
- Reverse proxy / L7 Load Balancer
- Embedded Perl interpreter
- On the fly binary improve
- Useful for re­writing URLs and excellent PCRE aid

### Explain How Nginx Can Handle Http Requests?
- **Ans:** Nginx makes use of the reactor sample. The important occasion loop waits for the OS to sign a readiness event­ such that the information is out there to read from a socket, at which example it's miles study into the buffer and processed. A Single thread can serve tens of lots of simultaneous connections.

### Explain Is It Possible To Replace Nginx Errors Like 502 Error With 503?
- **Ans:** 
502 = Bad gateway
503 = Server overloaded
Yes, it is possible but you to ensure that fastcgi_intercept_errors is set to ON, and use the error page directive.
```bash
Location /
{
  fastcgi_pass 127.0.01:9001;
  fastcgi_intercept_errors on;
  error_page 502 = 503/error_page.html;
  ...
}
```

### How to start and stop nginx server?
- **Ans:** Run the below command to start Nginx server:
```sudo systemctl start nginx``` 

### What are the main tasks of the Nginx web server? 
- **Ans:** The main tasks of the Nginx web server are:  
- To accept and process HTTP requests from clients 
- To serve static files such as images, CSS, JavaScript, etc. 
- To pass dynamic requests to upstream servers such as PHP, Python, Ruby, etc. 
- To balance the load among multiple upstream servers 
- To cache the responses from upstream servers 
- To compress and encrypt the responses 
- To log the requests and errors

### What is a reverse proxy?
- **Ans:** A reverse proxy is a server that sits in front of web servers and forwards client (e.g. web browser### requests to those web servers. Reverse proxies are typically implemented to help increase security, performance, and reliability.

### Difference between a forward and reverse proxy ?
- Forward proxy sits in front of a client and ensures that no origin server ever communicates directly with that specific client. 
- Reverse proxy sits in front of an origin server and ensures that no client ever communicates directly with that origin server.

### What is a CDN?
- **Ans:** A content delivery network (CDN) is a geographically distributed group of servers that caches content close to end users. A CDN allows for the quick transfer of assets needed for loading Internet content, including HTML pages, JavaScript files, stylesheets, images, and videos.

### What are the benefits of using a CDN?
- Improving website load times
- Reducing bandwidth costs
- Increasing content availability and redundancy
- Improving website security

### What is a Load Balancer?
- **Ans:** A load balancer can be deployed as software or hardware to a device that distributes connections from clients between a set of servers. A load balancer acts as a 'reverse-proxy' to represent the application servers to the client through a virtual IP address (VIP). This technology is known as server load balancing (SLB). SLB is designed for pools of application servers within a single site or local area network (LAN).

### What is Load balancing?
- **Ans:** Load balancing is designed to give the application availability, scalability, and security. As a reverse-proxy, the load balancer acts as a multi-functional valve to direct and control the traffic between the clients and servers.

### Benefits of a reverse proxy?
- Load balancing: it can distribute the requests among multiple upstream servers based on the availability and capacity of each server.  
- Caching: it can store the responses from the upstream servers and serve them to the clients without contacting the upstream servers again, which reduces the network latency and bandwidth consumption 
- Compression: it can compress the responses before sending them to the clients, which reduces the size of the data and improves the speed 
- Encryption: it can encrypt the communication between the client and the server, which enhances the security and privacy of the data 
- Authentication: it can verify the identity and credentials of the clients before allowing them to access the upstream servers, which prevents unauthorised access and attacks 
- Logging: it can record the requests and responses for debugging and analysis purposes, which helps to monitor and optimise the performance and functionality of the web application 
- Protection from attacks
- Global server load balancing (GSLB)