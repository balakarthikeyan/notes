## Core Features

- `Runtime` → Platform for running JS outside browsers.
- `V8 Engine` → Google's fast JS engine.
- `Libuv` → Provides async I/O and event loop.
- `Thread Pool` → Hidden worker threads.
- `Blocking` → Waits until completion.
- `Non-Blocking` → Delegates and continues.
- `Event Loop` → Core async mechanism.
- `Scalability` → Handling more users without crashing.

## Sync vs Async: What's the Difference?

`Synchronous (fs.writeFileSync)`
- Blocks the event loop until done
- Simple, but not scalable
- Example: writing to a file
- fs.writeFileSync("notes.txt", "My first note!"); console.log("Done!");

`Asynchronous (fs.writeFile)`
- Non-blocking → lets Node handle other tasks in the meantime
- Better for servers
- fs.writeFile("notes.txt", "My first async note!", (err) => { if (err) console.log(err); console.log("Done!"); });

### 🚦 Blocking vs Non-Blocking

#### 🔴 Blocking (Synchronous)
```js
const fs = require("fs");

const data = fs.readFileSync("file.txt", "utf8");
console.log(data);
console.log("This runs AFTER file reading finishes"); // Problem: If reading takes 5s, the entire thread is blocked.
```
#### 🟢 Non-Blocking (Asynchronous)
```js
const fs = require("fs");

fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});
console.log("This runs immediately, without waiting");
```

### ⚙️ The Event Loop

- Call Stack (executes code)
- Callback Queue (async results)
- Microtask Queue (promises, process.nextTick)
- Timers Queue (setTimeout, setInterval)