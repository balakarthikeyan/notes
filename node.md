## What is Node.js?
Node.js can be defined as a dynamic, cross-platform and open-source JavaScript framework or runtime environment that is built on the Google Chrome JavaScript V8 engine. Node.js, developed by Ryan Dahl in 2009, was initially implemented as a client-side scripting language.

Node.js is an open-source server side runtime environment built on Chrome's V8 JavaScript engine. It provides an event driven, non-blocking (asynchronous) I/O and cross-platform runtime environment for building highly scalable server-side applications using JavaScript.

Node.js uses a module architecture to simplify the creation of complex applications. Node.js is widely used for creating server-side and networking applications too.

```
node D:\hello.js
or
node console.log('Node is running');
node .help
node .exit
```

Some of the features and advantages of Node.js are:
- Loose Typing
- Faster code execution
- Highly scalable
- Non-blocking APIs
- No buffering
- REPL - `Read Eval Print Loop` (Debugging)

### Primitive Types:
- String
- Number
- Boolean
- Undefined
- Null
- RegExp

### Module Types:
- Core Modules
- Local Modules
- Third Party Modules

The following are the key areas where Node.js is widely used:

    I/O-bound applications
    Data streaming applications
    Data-intensive real-time applications (DIRT)
    JSON API-based applications
    Single-page applications
    • ExpressJS Framework (HTTP Requests, Middleware)
    • Good understanding of Asynchronous Programming with NodeJS (Callback, Call Stack and Event Loop)
    • Knowledge of Micro-services Architecture / Resilient Micro-Services
    • RabbitMQ / AMQP Messaging
    • Debugging NodeJS Programs and Error Handling
    • OData v4 (Sorting, Pagination, Filtering)
    • Static code analysis tools like ESLint/etc.

### Popular NPM
```
webpack: Builds static assets like browser JavaScript, CSS and even images. It allows to use node modules in the browser.
babel: Allows to code in the latest versions of JavaScript/ECMAScript without having to worry about your runtime by converting the new code to the code compatible with older versions of ECMAScript
axios: Makes HTTP requests
express: the most popular Node web framework
mongoose: MongoDB object-document mapper library
socket.io: Real-time library with support of Web Sockets and others.
cheerio: jQuery syntax for working with HTML-like data on the server
node-oauth: Low-level but very mature and tested library to roll out any OAuth integration
passport: OAuth library to quickly integrate with major services
mocha: Testing framework
async: Controls flow by running function concurrently, sequentially or any way you want
concurrently: Allows to execute CLI tools (local) as multiple processes all at the same time, e.g., webpack and node-static.
sequelize: PostgreSQL object-relational mapper library
node-dev: Monitor and restart your Node app automatically on any file change within the current folder
node-static: Serve files over HTTP web server
node-inspector: Debug Node code in a familiar interface of DevTools
docker: Build and run Docker containers to isolate app environment, speed up deployment and eliminate conflicts between dev and prod (or any other) environments
curl: Make HTTP(S) requests to test your web apps (default for POSIX but can get for Windows too)
nvm: Change Node versions without having to install and re-install them each time
wintersmith: Build static website using Node templates and Markdown
pm2: Process manager to vertically scale Node processes and ensure fail-tolerance and 0-time reload.
```

### To link Local Node module
```
npm link package-name
```

### Debugging
> Method 1
```
node --inspect-brk script.js
node inspect localhost:9229
```
> Method 2
```
npm install -g node-inspector
node-inspector --web-port=5500
```
> Method 3
node --debug-brk script.js

### to work with mutliple version of node use nvm
##### Install
`nvm install 12.0.0` or`to install latest `nvm install latest 
##### Use node version
`nvm use 10.14.0`
##### Other general commands
```
nvm list
nvm ls
```
### Examples
```
npm i uuid
import { v4 as uuidv4 } from "uuid";
var data = { id: uuidv4() };
```
