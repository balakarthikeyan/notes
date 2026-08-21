# Node.js Master Technical Study Guide

## 1. What is Node.js?

Node.js can be defined as a dynamic, cross-platform, and open-source JavaScript runtime environment that is built on the Google Chrome JavaScript V8 engine. Node.js, developed by Ryan Dahl in 2009, was initially implemented as a server-side runtime environment to execute JavaScript outside browser environments.

It provides an event-driven, non-blocking (asynchronous) I/O model for building highly scalable server-side and networking applications using JavaScript.

Node.js uses a modular architecture to simplify the creation of complex applications. It is widely used for building backend API servers, microservices, real-time networking tools, and streaming platforms.

---

## Core Features & Advantages

* **Runtime:** → Platform executing JavaScript code outside web browsers.
* **V8 Engine:** → Google's high-performance open-source JavaScript engine written in C++, converting JS directly to native machine code.
* **Libuv:** → Cross-platform support library with a focus on asynchronous I/O operations, managing the event loop and thread pool.
* **Thread Pool:** → Hidden worker threads provided by `libuv` to handle heavy tasks (like file system operations, cryptography, and DNS lookups) that cannot be handled asynchronously by the OS.
* **Loose Typing:** → Dynamic JavaScript typing system.
* **Faster Code Execution:** → Powered by Google Chrome's V8 JIT compilation engine.
* **Highly Scalable:** → Asynchronous non-blocking architecture handles thousands of concurrent connections.
* **Blocking:** → Node.js process waits until the execution of a non-JavaScript operation completes (Synchronous).
* **Non-Blocking:** → Operations that delegate tasks to the system or thread pool and immediately continue executing the remaining script (Asynchronous).
* **Non-blocking APIs:** → I/O operations execute asynchronously without halting thread execution.
* **No Buffering:** → Applications output data in chunks (streams) rather than buffering whole datasets into memory.
* **REPL (`Read-Eval-Print-Loop`):** → Interactive shell environment for quick code execution and debugging.
* **Event Loop:** → The core orchestrator mechanism that allows Node.js to perform non-blocking I/O operations despite being single-threaded which managing callback queues and task execution.
* **Scalability:** → The capability of handling thousands of concurrent connections efficiently without crashing or exhausting system resources.

---

### CLI Execution & Commands

#### Basic CLI Commands
```bash
# Execute a local JavaScript file
node hello.js

# Interactive REPL Helper Commands
.help
.exit
```

#### Running Inline Scripts (Legacy Note vs. Correct Syntax)

```bash
# Modern CLI Command (-e / --eval):
node -e "console.log('Node is running');"
```

---

### JavaScript Data Types & Module Architecture

#### Primitive Types

* `String`
* `Number`
* `Boolean`
* `Undefined`
* `Null`
* `RegExp` *(Note: `RegExp` is an object type in JavaScript spec; primitive additions in ES6+ include `Symbol` and `BigInt`)*

#### Module Types

1. **Core Modules:** Built-in packages (`fs`, `http`, `path`, `events`, `crypto`, `stream`).
2. **Local Modules:** Custom files created within the application (`./greet.js`).
3. **Third-Party Modules:** External packages installed via NPM (`express`, `mongoose`, `lodash`).

---

### Key Application Domains & Ecosystem Architecture

* I/O-bound applications
* Data streaming applications
* Data-intensive real-time applications (DIRT)
* JSON API-based applications
* Single-page applications (SPA backends)
  * **ExpressJS Framework:** HTTP requests handling, routing, and middleware pipelines.
  * **Asynchronous Programming:** Deep knowledge of callbacks, promise chains, `async/await`, the Call Stack, and the Event Loop.
  * **Microservices Architecture:** Resilient microservices using event buses.
  * **AMQP / Messaging Systems:** Asynchronous message queues like RabbitMQ or Kafka.
  * **Debugging & Error Handling:** Stack trace inspection, structured logging, custom error classes.
  * **OData v4 Specifications:** Standardized sorting, pagination, and filtering paradigms across APIs.
  * **Static Code Analysis:** ESLint, Prettier, and static type checking via TypeScript.

---

## 2. Popular NPM Packages & Tooling

| Package | Purpose & Description |
| --- | --- |
| `webpack` | Builds static assets like browser JavaScript, CSS, and images. Bundles node modules for browser usage. |
| `babel` | Transpiles modern JavaScript (ES6+) into backward-compatible JavaScript versions for older environments. |
| `axios` | Promise-based HTTP client for browser and Node.js. |
| `express` | Fast, minimalist web framework for Node.js. |
| `mongoose` | MongoDB object modeling tool (ODM) designed to work in an asynchronous environment. |
| `socket.io` | Real-time bidirectional event-based communication library using WebSockets with fallbacks. |
| `cheerio` | Fast, flexible, and lean implementation of core jQuery designed specifically for the server. |
| `node-oauth` | Low-level, tested library to implement OAuth 1.0/2.0 authentication flows. |
| `passport` | Simple, unobtrusive authentication middleware for Node.js supporting multiple strategies. |
| `mocha` | Feature-rich JavaScript test framework running on Node.js and in the browser. |
| `async` | Utility module providing straight-forward, powerful functions for working with asynchronous JavaScript. |
| `concurrently` | Run multiple CLI commands concurrently in a single terminal session. |
| `sequelize` | Promise-based Node.js Object-Relational Mapper (ORM) for Postgres, MySQL, SQLite, and SQL Server. |
| `node-dev` | Utility that runs a Node application and restarts the process when files change. |
| `node-static` | Simple, RFC-compliant HTTP static-file server module for Node.js. |
| `node-inspector` | *(Legacy)* In-browser CLI debugger interface for Node.js using Chrome Developer Tools. |
| `docker` | Containerization technology to isolate environments, speed up deployments, and eliminate environment drift. |
| `curl` | Command-line tool for transferring data with URLs. |
| `nvm` | Node Version Manager - Allows switching between installed Node.js versions seamlessly. |
| `wintersmith` | Flexible, multi-format static site generator built on Node.js using Markdown and templates. |
| `pm2` | Production Process Manager for Node.js applications with built-in load balancer and zero-downtime reloads. |

---

### Local Module Linking

To link a local development module globally for local testing:

```bash
npm link package-name
```

---

### Debugging Configurations

#### Legacy vs. Modern Debugging Methods

##### Method 1: Chrome DevTools / Native Inspector (Recommended)

```bash
# Starts Node inspector and pauses execution at the first line
node --inspect-brk script.js

# Connect CLI inspector
node inspect localhost:9229
```

##### Method 2: Legacy Node Inspector

```bash
# Global installation (Obsolete in modern Node versions)
npm install -g node-inspector
node-inspector --web-port=5500
```

##### Method 3: Legacy Debug Flag (Deprecated in Node v8+)

```bash
# Legacy debug breakpoint flag
node --debug-brk script.js
```

---

### Node Version Management (`nvm`)

#### Installation & Version Switching

```bash
# Install specific version
nvm install 12.0.0

# Install latest available version
nvm install latest

# Use specific version
nvm use 10.14.0

# List installed versions
nvm list
nvm ls

# Set default Node version globally
nvm alias default v7.3.0

# Set default Node version to modern LTS
nvm alias default lts/*
```

---

## 3. Synchronization Models

### Sync vs. Async: What's the Difference?

#### Synchronous (`fs.writeFileSync`)

* Blocks the main single thread and event loop until operation completion.
* Simple, sequential, but degrades server throughput under load.

**Example:** Writing synchronously to a file.
```javascript
const fs = require("fs");

fs.writeFileSync("notes.txt", "My first note!");
console.log("Done!");

```

#### Asynchronous (`fs.writeFile`)

* Non-blocking operation delegating execution to the OS/Libuv thread pool.
* Non-blocking execution path that yields control back to the runtime to perform other tasks while the operation runs in the background.
* Allows the event loop to continue handling incoming HTTP requests.

**Example:** Writing asynchronously to a file.
```javascript
const fs = require("fs");

fs.writeFile("notes.txt", "My first async note!", (err) => {
  if (err) console.log(err);
  console.log("Done!");
});
console.log("This runs immediately, without waiting.");

```

---

### 🚦 Blocking vs. Non-Blocking

#### 🔴 Blocking (Synchronous Paradigm)

```javascript
const fs = require("fs");

// The thread blocks here until the file read is complete
const data = fs.readFileSync("file.txt", "utf8");
console.log(data);
// Architectural Bottleneck: If reading takes 5 seconds, the entire single thread is blocked.
console.log("This runs AFTER file reading finishes"); // Problem: Blocks execution if file is large

```

#### 🟢 Non-Blocking (Asynchronous Paradigm)

```javascript
const fs = require("fs");

// The thread initiates file read and registers callback
fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});
console.log("This runs immediately, without waiting");

```

---

### ⚙️ The Event Loop Queues

The Event Loop processes tasks across specialized queues in a specific order:

```
   ┌───────────────────────────┐
┌─>│           TIMERS          │  setTimeout(), setInterval()
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     PENDING CALLBACKS     │  I/O callbacks deferred to next loop iteration
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │      IDLE, PREPARE        │  Internal Node processing
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │           POLL            │  Retrieve new I/O events; execute I/O callbacks
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │           CHECK           │  setImmediate() callbacks execute here
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │      CLOSE CALLBACKS      │  socket.on('close', ...)
└──────────────┬───────────────┘

```

1. **Call Stack:** Executes synchronous JavaScript code.
2. **Microtask Queue:** Processes Highest priority async queue (`process.nextTick`, followed by resolved `Promise`, `queueMicrotask`).
3. **Timers Queue:** Handles callbacks scheduled by expired timers (`setTimeout`, `setInterval`).
4. **I/O Callbacks Queue / Poll Phase**: Retrieves new I/O events and executes almost all I/O related callbacks.
5. **Check Queue:** Holds `setImmediate` callbacks.
6. **Close Callbacks Queue**: Manages cleanup tasks like `socket.on('close', ...)`.

---

## 4. Node.js core topics

### 1. `async` (Asynchronous / `async`/`await`)

* **Definition:** Asynchronous programming is a non-blocking paradigm that allows operations (like disk reading or network requests) to run in the background without halting the main thread execution. In modern JavaScript, `async` is a keyword placed before a function declaration to indicate that it implicitly returns a Promise.

* **Explanation:** In traditional synchronous programming, every line must finish before the next line starts. Node.js relies on asynchronous execution so that expensive I/O operations delegate work to the OS or thread pool, keeping the main thread free to process other incoming requests. The `await` keyword pauses the execution of an `async` function until the Promise settles.

**Example:**
```javascript
const fs = require('fs').promises;

// Modern async/await approach
async function readConfigFile() {
  try {
    const data = await fs.readFile('config.json', 'utf8');
    console.log('File Content:', data);
  } catch (err) {
    console.error('Error reading file:', err);
  }
}

readConfigFile();
```

---

### 2. `REPL` (Read-Eval-Print Loop)

* **Definition:** REPL stands for **Read-Eval-Print Loop**. It is an interactive, line-by-line command-line environment built into Node.js used for rapid prototyping, debugging, and testing JavaScript code.

* **Explanation:**
1. **Read:** Reads user JavaScript input.
2. **Eval:** Evaluates the input code structure.
3. **Print:** Prints the result to the console.
4. **Loop:** Loops back and waits for the next user input.

**Example:**
Open your terminal and type `node` to launch the shell:
```text
$ node
> const a = 10;
undefined
> const b = 20;
undefined
> a + b
30
> .exit
```

---

### 3. `var` vs `let`

* **Definition:** `var` and `let` are JavaScript keywords used to declare variables, differing in scoping rules, re-declaration permissions, and hoisting behaviors.

* **Explanation:**
* **`var` (Legacy):** Function-scoped (or globally scoped), can be re-declared within the same scope, and is hoisted with an initial value of `undefined`.
* **`let` (Modern - ES6):** Block-scoped (bounded by `{}`), cannot be re-declared within the same block scope, and lives in a "Temporal Dead Zone" until declared.

**Example:**
```javascript
function scopeDemo() {
  if (true) {
    var functionScoped = "I am accessible outside the block";
    let blockScoped = "I am trapped inside this block";
  }
  console.log(functionScoped); // Outputs: I am accessible outside the block
  // console.log(blockScoped);  // Uncaught ReferenceError: blockScoped is not defined
}

scopeDemo();
```

---

### 4. `_unders` (REPL `_` Variable & Naming Conventions)

* **Definition:** `_unders` refers to both the special underscore `_` variable within the Node.js REPL and variable naming conventions in JavaScript codebases (e.g., private members, unused arguments, or utility libraries like Lodash/Underscore.js).

* **Explanation:**
1. **REPL Context:** In the Node.js REPL shell, `_` automatically stores the evaluation result of the *most recent* expression.
2. **Code Convention:** A single prefix underscore (e.g., `_id` or `_req`) signals an intentionally unused parameter or an internal variable.

**Example:**
```javascript
// 1. In REPL:
// > 10 + 5
// 15
// > _ * 2
// 30

// 2. In Function Parameters (Unused First Argument):
const express = require('express');
const app = express();

// _req indicates request object is intentionally unreferenced
app.get('/health', (_req, res) => {
  res.send('OK');
});
```

---

### 5. `npm` (Node Package Manager)

* **Definition:** NPM stands for **Node Package Manager**. It is the default package manager for the JavaScript runtime environment.

* **Explanation:** NPM consists of a Command Line Interface (CLI) client and an online public repository (registry) hosting millions of open-source reusable JavaScript packages. It handles dependency resolution, package installation, version locking (`package-lock.json`), and script automation.

**Example:**
```bash
# Initialize a new Node project
npm init -y

# Install a package
npm install express

# Run custom project scripts defined in package.json
npm run dev
```

---

### 6. `local depend` (Local Dependencies)

* **Definition:** Local dependencies are third-party packages installed specifically inside a single project's `node_modules` directory and recorded in its `package.json` file.

* **Explanation:** These modules are isolated to the specific project, ensuring that different projects running on the same host machine can use different versions of the same library without version conflicts.

**Example:**
```bash
# Installs 'axios' locally for the current project only
npm install axios

```

```javascript
// Imported directly in code from local node_modules
const axios = require('axios');
```

---

### 7. `global depend` (Global Dependencies)

* **Definition:** Global dependencies are packages installed system-wide in a shared global directory on your operating system rather than a local `node_modules` folder.

* **Explanation:** Global packages are primarily CLI binaries, developer tools, or process managers (like `pm2`, `nodemon`, or `nvm`) that you run directly from any terminal prompt regardless of your current directory path.

**Example:**
```bash
# Install PM2 globally across your machine
npm install -g pm2

# Execute command globally from any directory
pm2 status
```

---

### 8. `package.json` (`<package class="json"></package>`)

* **Definition:** `package.json` is a JSON format metadata file located at the root directory of a Node.js project.

* **Explanation:** It acts as the manifest for the application, storing details such as project name, version, entry file (`main`), executable CLI scripts (`scripts`), and lists of required local production (`dependencies`) and development (`devDependencies`) packages.

**Example:**
```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

---

### 9. `callback`

* **Definition:** A callback is a function passed as an argument to another function, which is executed after the parent function finishes its current task.

* **Explanation:** In Node.js, asynchronous callbacks typically follow the **Error-First Callback Pattern**, where the first argument is reserved for an `Error` object (or `null`/`undefined` if successful), and subsequent arguments contain returned data.

**Example:**
```javascript
const fs = require('fs');

// Asynchronous error-first callback pattern
fs.readFile('hello.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Failed to read file:', err.message);
    return;
  }
  console.log('File contents:', data);
});
```

---

### 10. `blocking code, prevent` (Blocking Code & Prevention)

* **Definition:** Blocking code refers to synchronous code execution that halts the single main thread of Node.js until the operation completes.

* **Explanation:** Because Node.js executes JavaScript on a single thread, calling synchronous I/O operations (e.g., `fs.readFileSync`) or running intense computational loops blocks the Event Loop, causing all other incoming HTTP requests to freeze.

* **Prevention Strategies:**
  1. Use asynchronous, non-blocking APIs (`fs.readFile` instead of `fs.readFileSync`).
  2. Offload CPU-heavy operations to `worker_threads` or background job queues (RabbitMQ/BullMQ).
  3. Use Streams for large files instead of loading entire datasets into memory.

**Example:**
```javascript
const fs = require('fs');

// ❌ BLOCKING (Avoid in web servers):
// const data = fs.readFileSync('large_file.txt', 'utf8');

// 🟢 NON-BLOCKING (Prevents blocking the thread):
fs.readFile('large_file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log('Read completed asynchronously without blocking!');
});
```

---

### 11. `event loop`

* **Definition:** The Event Loop is an infinite orchestration process managed by `libuv` that enables Node.js to perform non-blocking asynchronous I/O operations on a single thread.

* **Explanation:** It constantly checks if the Call Stack is empty. When the stack clears, it pulls queued callbacks from distinct priority queues (Timers Queue, Microtask Queue, Poll Queue, Check Queue) and pushes them to the Call Stack for execution.

**Example:**
```javascript
console.log('1: Synchronous code start');

setTimeout(() => {
  console.log('2: Macrotask (Timer Queue)');
}, 0);

Promise.resolve().then(() => {
  console.log('3: Microtask (Promise Queue)');
});

console.log('4: Synchronous code end');

// Output order:
// 1: Synchronous code start
// 4: Synchronous code end
// 3: Microtask (Promise Queue)
// 2: Macrotask (Timer Queue)
```

---

### 12. `event emit` (EventEmitter)

* **Definition:** Event Emitting is a core publisher-subscriber architecture pattern provided by the Node.js `events` module via the `EventEmitter` class.

* **Explanation:** An `EventEmitter` instance emits named signals using `.emit('eventName', payload)`. Listeners registered with `.on('eventName', handler)` intercept these events and execute their associated callback logic.

**Example:**
```javascript
const EventEmitter = require('events');
const userLogger = new EventEmitter();

// Register Event Listener
userLogger.on('login', (username) => {
  console.log(`[LOG]: User ${username} logged in at ${new Date().toLocaleTimeString()}`);
});

// Emit/Trigger Event
userLogger.emit('login', 'JohnDoe');
```

---

### 13. `buffer class`

* **Definition:** The `Buffer` class is a globally available core class in Node.js designed to handle raw binary data directly in memory.

* **Explanation:** Since standard JavaScript historically only handled string types, Node introduced `Buffer` to allocate fixed-sized raw memory blocks outside the V8 V8 heap. Buffers represent sequences of bytes (integers from 0 to 255) and are essential for file system operations, TCP socket handling, and network streams.

**Example:**
```javascript
// Create a buffer from a string
const buf = Buffer.from('Node.js', 'utf8');

console.log(buf); 
// Output raw bytes in hexadecimal: <Buffer 4e 6f 64 65 2e 6a 73>

console.log(buf.toString('utf8')); 
// Converts binary back to string: "Node.js"
```

---

### 14. `piping` (Stream Piping)

* **Definition:** Piping is a mechanism that connects the output stream of a **Readable Stream** directly into the input of a **Writable Stream** using the `.pipe()` method.

* **Explanation:** Piping automates chunk-by-chunk data transfer and handles **backpressure** (preventing a fast reader from overwhelming a slow writer) without requiring the entire dataset to reside in RAM.

**Example:**
```javascript
const fs = require('fs');

// Create readable stream and writable stream
const readerStream = fs.createReadStream('source.txt');
const writerStream = fs.createWriteStream('destination.txt');

// Pipe reader to writer
readerStream.pipe(writerStream);

writerStream.on('finish', () => {
  console.log('File streaming and writing completed successfully!');
});
```

---

### 15. `filebase` (File-Based Data Operations / Storage)

* **Definition:** `filebase` refers to persisting, querying, and updating structured or unstructured data directly on disk using flat files (JSON, CSV, TXT) via the Node.js File System (`fs`) core module or lightweight flat-file embedded databases (e.g., Lowdb, NeDB).

* **Explanation:** Instead of relying on a dedicated database server (like PostgreSQL or MongoDB), file-based data operations read file contents from disk into memory, transform the structure, and write the result back to disk.

**Example:**
```javascript
const fs = require('fs').promises;

// Simple file-based data store helper
async function addRecord(newRecord) {
  const filePath = 'db.json';

  // Read existing file data
  const rawData = await fs.readFile(filePath, 'utf8');
  const db = JSON.parse(rawData);

  // Mutate and save back
  db.users.push(newRecord);
  await fs.writeFile(filePath, JSON.stringify(db, null, 2));
  console.log('Record written to file-based store!');
}

// addRecord({ id: 1, name: 'Alice' });
```

---

### 16. `promises`

* **Definition:** A Promise is an object representing the eventual completion (fulfillment) or failure (rejection) of an asynchronous operation and its resulting value.

* **Explanation:** Promises solve "Callback Hell" (nested pyramids of callbacks) by flattening asynchronous control flows into chainable `.then()`, `.catch()`, and `.finally()` handlers, or integrating cleanly with `async/await` syntax.

* **States:**
  * `Pending`: Initial state, neither fulfilled nor rejected.
  * `Fulfilled`: Operation completed successfully.
  * `Rejected`: Operation failed with an error.

**Example:**
```javascript
// Wrap asynchronous operation in a Promise
function checkInventory(item) {
  return new Promise((resolve, reject) => {
    const available = true;
    if (available) {
      resolve(`Item "${item}" is in stock.`);
    } else {
      reject(new Error(`Item "${item}" is out of stock.`));
    }
  });
}

// Consume Promise
checkInventory('Laptop')
  .then((message) => console.log('Success:', message))
  .catch((err) => console.error('Failure:', err.message));
```

---

## Technical Glossary

* **async**: Syntax keyword modifier used to write asynchronous code using synchronous-looking structures (`await`).
* **REPL**: Read-Eval-Print Loop. The interactive shell environment for executing raw Node.js commands directly from the terminal.
* **var vs let**: `var` is function-scoped and hoisted, whereas `let` introduces block-scoping and resides within the Temporal Dead Zone (TDZ) until declared.
* **_unders (Underscore Variable)**: In the Node.js REPL, the special variable `_` stores the evaluation result of the last executed expression.
* **npm**: Node Package Manager. The default software registry and command-line execution client for distributing and configuring Node.js modular packages.
* **local depend**: Local Dependencies. Packages installed within a specific project directory (stored inside `node_modules`) and tracked inside `package.json`.
* **global depend**: Global Dependencies. System-wide package installations executed via `npm install -g <package>`, primarily used for system-level CLI tools.
* **package.json**: The fundamental manifest file configuration that holds configuration details, script mappings, author metadata, and project dependency lists.
* **callback**: A function passed as an argument to another function, invoked once an asynchronous or background operation finishes execution.
* **blocking code, prevent**: Architectural strategy of utilizing asynchronous APIs, stream piping, and worker pools to ensure the main thread never stalls.
* **event loop**: The continuous processing loop inside Node.js that checks, dispatches, and handles multi-phased asynchronous callback queues.
* **event emit**: The design pattern where objects subclassing `EventEmitter` synchronously fire named signals to trigger associated listener routines.
* **buffer class**: A core Node.js global class optimized for handling raw data streams, binary chunks, and out-of-browser octet manipulation.
* **piping**: The mechanism of connecting the raw readable data stream output directly into a writeable target stream destination (e.g., `readable.pipe(writable)`).
* **filebase**: File system operations handled via the built-in `fs` or `fs/promises` internal modules.
* **promises**: A proxy object representing the ultimate completion or failure of an asynchronous operation, bypassing deep callback structures.

---

## 1. Node.js Core Architecture & Scalability

### Event Loop

Node.js uses a single-threaded, event-driven architecture powered by **libuv**. The Event Loop offloads I/O tasks to the system kernel or background thread pool.

```
   ┌───────────────────────────┐
┌─>│           Timers          │  (setTimeout, setInterval)
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     Pending Callbacks     │  (I/O callbacks deferred from previous loop)
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │      Idle, Prepare        │  (Internal Node.js usage)
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │           Poll            │  (Retrieve new I/O events; execute I/O callbacks)
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │           Check           │  (setImmediate callbacks)
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │      Close Callbacks      │  (e.g., socket.on('close', ...))
└────────────────┴─────────────┘

```

### Event Loop Execution & Microtasks

* **`process.nextTick()`**: Executes callbacks **immediately** after the current synchronous operation finishes, before the Event Loop proceeds to the next phase or process microtasks (`Promise.then`). Overusing `process.nextTick` can starve the Event Loop of I/O.
* **`setImmediate()`**: Queues callbacks to run in the **Check phase** of the Event Loop (after I/O polling).

### Event Queueing & Handling Heavy Processing

Node.js processes I/O asynchronously, but synchronous or CPU-heavy tasks block the single main thread.

#### Processing Large Operations (e.g., Sending Emails to Millions)

1. **Message Queues**: Offload job payloads to persistent queue systems like **RabbitMQ** or **BullMQ (Redis)**.
2. **Worker Processes**: Use worker pools or background workers to fetch and process batches from the queue outside the main HTTP server process.
3. **Chunking & Rate Limiting**: Stream or paginate large database queries; execute emails in rate-limited batches to prevent memory exhaustion and API throttling.

### Scaling Node.js Applications

* **Vertical Scaling**:
* **Cluster Module**: Fork multiple instances of the Node process across CPU cores sharing a single server port.
* **Worker Threads (`worker_threads`)**: Run CPU-heavy tasks (e.g., encryption, image processing) on dedicated threads sharing memory within the same process.


* **Horizontal Scaling**:
* Deploy multiple application nodes behind a Load Balancer (e.g., NGINX, AWS ALB).
* Maintain stateless API layers and delegate session state to Redis.


* **Microservices Architecture**: Split monolithic applications into independent services connected via REST, gRPC, or message brokers.

### Child Process Module

Spawns subprocesses to execute system commands or external scripts without blocking the main event thread:

* **`spawn()`**: Streams data chunks (`stdout`/`stderr`). Best for long-running tasks or large data streams.
* **`exec()`**: Buffers full output in memory. Best for short tasks with small output.
* **`execFile()`**: Directly executes an executable file.
* **`fork()`**: Special instance of `spawn()` that opens an IPC (Inter-Process Communication) channel between master and child Node.js processes.

### Cron Jobs

Scheduled background tasks triggered at defined time intervals. In Node.js, libraries like `node-cron` or system-level crontabs schedule periodic routines (e.g., nightly database cleanups or report generators).

### *Missed Core Concepts to Include*

* **Streams (`stream`)**: Read/write large files in chunks without filling RAM memory (`Readable`, `Writable`, `Transform`, `Duplex`).
* **Buffers (`Buffer`)**: Allocate raw memory outside the V8 heap to handle binary data.
* **EventEmitter (`events`)**: Native pattern enabling pub/sub event-driven logic inside modules.

---

## 2. JavaScript Core & Asynchronous Mechanics

### Scopes & Declarations

| Feature | `var` | `let` / `const` |
| --- | --- | --- |
| **Scope** | Function scope | Block scope (`{ ... }`) |
| **Hoisting** | Hoisted with `undefined` | Hoisted into Temporal Dead Zone (TDZ) |
| **Re-declaration** | Allowed | Syntax Error |

### JavaScript Inheritance

#### Prototypal Inheritance (ES5)

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return `Hello, I am ${this.name}`;
};

function Employee(name, title) {
  Person.call(this, name);
  this.title = title;
}
Employee.prototype = Object.create(Person.prototype);
Employee.prototype.constructor = Employee;

```

#### Class Syntax (ES6+)

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    return `Hello, I am ${this.name}`;
  }
}

class Employee extends Person {
  constructor(name, title) {
    super(name);
    this.title = title;
  }
}

```

### Object Merging & Cloning

#### Object Merging

```javascript
const obj1 = { a: 1 };
const obj2 = { b: 2 };

// ES6 Spread
const merged = { ...obj1, ...obj2 };

// Object.assign
const mergedAssign = Object.assign({}, obj1, obj2);

```

#### Object Cloning

* **Shallow Copy**: `const clone = { ...original };` or `Object.assign({}, original);`
* **Deep Copy**: `const clone = structuredClone(original);` or `JSON.parse(JSON.stringify(original));`

### Callbacks, Callback Hell, Promises, & Async/Await

* **Callback**: Function passed into another function to execute when an async operation finishes.
* **Callback Hell**: Nested callbacks leading to pyramid-shaped, unmaintainable code.

```javascript
// Callback Hell Avoidance using Promises & Async/Await
async function executeTask() {
  try {
    const res1 = await asyncFunc1();
    const res2 = await asyncFunc2(res1);
    return res2;
  } catch (error) {
    console.error(error);
  }
}

```

#### Executing Promises in Parallel

`Promise.all` executes an array of promises concurrently and rejects if any single promise fails.

```javascript
const fetchData = async () => {
  const [user, posts] = await Promise.all([
    fetch('/api/user').then(r => r.json()),
    fetch('/api/posts').then(r => r.json())
  ]);
  return { user, posts };
};

```

---

## 3. Express, Hapi, & Web Frameworks

### Express Middleware & Execution Chain

Middlewares are functions that access the request (`req`), response (`res`), and the `next` function in the application's request-response cycle.

```javascript
// Implementation of a Custom Middleware Chain
const app = express();

const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next(); // Pass control to the next middleware
};

app.use(logger);

```

### Hapi Server Endpoint

```javascript
const Hapi = require('@hapi/hapi');

const init = async () => {
  const server = Hapi.server({ port: 3000, host: 'localhost' });

  server.route({
    method: 'GET',
    path: '/time',
    handler: (request, h) => {
      return { currentTime: new Date().toISOString() };
    }
  });

  await server.start();
  console.log('Server running on %s', server.info.uri);
};

init();

```

---

## 4. Frontend & Web Infrastructure (Angular Fundamentals)

### TypeScript Scope Modifiers

* **`public`**: Accessible anywhere.
* **`private`**: Accessible only within the defining class.
* **`protected`**: Accessible within the defining class and its subclasses.

### Observables vs. Promises

* **Promise**: Single async response, non-cancellable, eager execution.
* **Observable**: Stream of multiple values over time, cancellable via `Unsubscription`, lazy execution (runs only when subscribed).
* **Cancelling an Observable**: Use `takeUntil` or unsubscribe when a user navigates away or type-ahead search updates before the prior request completes.

### Services, Dependency Injection, & Providers

* **Services**: Modular singletons that handle business logic, state, or HTTP calls.
* **Dependency Injection (DI)**: Design pattern where components request dependencies rather than instantiating them manually.
* **Providers**: Tell Angular's DI framework how to resolve a dependency token (e.g., `providedIn: 'root'`).

### Web Infrastructure Concepts

* **`event.stopPropagation()`**: Stops the event from bubbling up the DOM tree.
* **`event.preventDefault()`**: Prevents default browser actions for an event (e.g., submitting a form, navigating a link).
* **CORS (Cross-Origin Resource Sharing)**: HTTP-header-based mechanism that allows a server to explicitly list origins allowed to load resources in browsers.
* **Virtual DOM vs Angular Change Detection**:
* **Virtual DOM (React)**: In-memory representation of real DOM; reconciles structural changes via diffing algorithm.
* **Angular Zone.js / Change Detection**: Tracks async operations and traverses component property trees to update DOM nodes directly.



---

## 5. Output Prediction Corrections

### Question 1: Execution Order with `setTimeout`

```javascript
console.log('Good Morning');
setTimeout(function() {
  console.log("Hello, World!");
}, 0);
console.log('Good Afternoon');

```

**Output**:

```
Good Morning
Good Afternoon
Hello, World!

```

*Explanation*: `setTimeout` transfers the callback to the Timers API/Queue. Main synchronous code executes first.

### Question 2: Hoisting Error Correction

```javascript
a();
b();

function a() {
  console.log('This is function a');
}

var b = function() {
  console.log('This is function b');
};

```

**Actual Output**:

```
This is function a
Uncaught TypeError: b is not a function

```

*Correction*: Function declaration `a` is fully hoisted. Variable declaration `var b` is hoisted as `undefined`. Calling `b()` prior to initialization throws a runtime `TypeError`.

### Question 3: Matrix Right Diagonal Output

```javascript
function rightDiagonal(a, row, column) {
  for (let i = 0; i < row; i++) {
    for (let j = 0; j < column; j++) {
      if (i + j === column - 1) {
        console.log(a[i][j]);
      }
    }
  }
}

const matrix = [
  [1, 2, 4],
  [5, 7, 8],
  [10, 11, 13]
];
rightDiagonal(matrix, 3, 3);

```

**Output**:

```
4
7
10

```

---

## 6. Algorithmic Solutions

### 1. Find Unique Number in Pair Array

```javascript
// O(n) Time | O(1) Space using Bitwise XOR
function findUnique(arr) {
  return arr.reduce((acc, curr) => acc ^ curr, 0);
}
console.log(findUnique([2, 2, 1, 3, 3, 4, 4, 5, 5])); // 1

```

### 2. Recursive Cost and Discount Calculator

```javascript
const obj = {
  id1: { name: "adf", cost: 10, disc: 20 },
  id2: {
    name: "lkfd", cost: 20, disc: 10,
    items: {
      id3: { name: "sfd", cost: 10, disc: 20 }
    }
  }
};

function calculateTotals(data) {
  let totals = { cost: 0, disc: 0 };
  for (const key in data) {
    const node = data[key];
    if (node.cost) totals.cost += node.cost;
    if (node.disc) totals.disc += node.disc;
    if (node.items) {
      const nested = calculateTotals(node.items);
      totals.cost += nested.cost;
      totals.disc += nested.disc;
    }
  }
  return totals;
}
console.log(calculateTotals(obj)); // { cost: 40, disc: 50 }

```

### 3. Alternate Element In-Place Reversal

```javascript
function reverseAlternates(arr) {
  let indices = [];
  let values = [];

  for (let i = 0; i < arr.length; i += 2) {
    indices.push(i);
    values.push(arr[i]);
  }

  values.reverse();

  for (let i = 0; i < indices.length; i++) {
    arr[indices[i]] = values[i];
  }
  return arr;
}
console.log(reverseAlternates([3, 1, 5, 2, 7])); // [7, 1, 5, 2, 3]

```

### 4. Sort 0s, 1s, 2s (Dutch National Flag Algorithm in Single Loop)

```javascript
function sort012(arr) {
  let low = 0, mid = 0, high = arr.length - 1;
  while (mid <= high) {
    if (arr[mid] === 0) {
      [arr[low], arr[mid]] = [arr[mid], arr[low]];
      low++;
      mid++;
    } else if (arr[mid] === 1) {
      mid++;
    } else {
      [arr[mid], arr[high]] = [arr[high], arr[mid]];
      high--;
    }
  }
  return arr;
}
console.log(sort012([2, 1, 1, 0, 2, 0, 1, 0])); // [0, 0, 0, 1, 1, 1, 2, 2]

```

### 5. Format Seconds into `HH:mm:ss`

```javascript
function formatTime(seconds) {
  const hrs = Math.floor(seconds / 3600).toString().padStart(2, '0');
  const mins = Math.floor((seconds % 3600) / 60).toString().padStart(2, '0');
  const secs = (seconds % 60).toString().padStart(2, '0');
  return `${hrs}:${mins}:${secs}`;
}
console.log(formatTime(302)); // "00:05:02"

```

### 6. Replace Every Element with Maximum Element to its Right

```javascript
function replaceWithMax(arr) {
  const n = arr.length;
  if (n === 0) return arr;
  
  let maxFromRight = arr[n - 1];
  for (let i = n - 2; i >= 0; i--) {
    let current = arr[i];
    arr[i] = maxFromRight;
    if (current > maxFromRight) {
      maxFromRight = current;
    }
  }
  return arr;
}
console.log(replaceWithMax([12, 15, 8, 7, 9])); // [15, 15, 9, 9, 9]

```

### 7. Move Unique Elements to Front (Sorted Array)

```javascript
function removeDuplicates(arr) {
  if (arr.length === 0) return 0;
  let writeIndex = 1;

  for (let i = 1; i < arr.length; i++) {
    if (arr[i] !== arr[i - 1]) {
      arr[writeIndex] = arr[i];
      writeIndex++;
    }
  }
  return arr.slice(0, writeIndex);
}
console.log(removeDuplicates([1, 2, 2, 3, 4, 4, 4, 4, 5, 6, 6])); // [1, 2, 3, 4, 5, 6]

```

### 8. Anagram Checker

```javascript
function isAnagram(str1, str2) {
  const normalize = str => str.toLowerCase().replace(/[^a-z0-9]/g, '').split('').sort().join('');
  return normalize(str1) === normalize(str2);
}
console.log(isAnagram("listen", "silent")); // true

```

### 9. Two Sum Target Check

```javascript
function hasTwoSum(arr, target) {
  const seen = new Set();
  for (const num of arr) {
    const diff = target - num;
    if (seen.has(diff)) return true;
    seen.add(num);
  }
  return false;
}
console.log(hasTwoSum([4, 5, 6, 3], 7)); // true (4+3)

```