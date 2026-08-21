## Browser Architecture

```text
  [ User Types URL ]
          │
          ▼
  1. URL Parsing ──► 2. DNS Resolution ──► 3. TCP / TLS Handshake
                                                    │
                                                    ▼
  5. CSSOM Tree ◄────────── 4. DOM Tree ◄────── HTTP Response

```

---

### 1. URL Parsing & HTTP Request Generation

#### Definition

When a user types a string into the address bar (Omnibox), the browser's **Browser Process** determines whether the input is a search query or a valid Uniform Resource Locator (URL). If valid, it parses the URL into standard components and constructs an HTTP/1.1 or HTTP/2 request payload to send to the networking stack.

URL Structural Formula:


$$\text{URL} = \text{scheme} + \text{://} + \text{host} + \text{:} + \text{port} + \text{path} + \text{?} + \text{query} + \text{\#} + \text{fragment}$$

#### Code Example

```javascript
// URL Parsing via the standard URL API
const rawUrl = "https://example.com:443/api/v1/users?role=admin#profile";
const parsed = new URL(rawUrl);

console.log({
  protocol: parsed.protocol, // "https:"
  hostname: parsed.hostname, // "example.com"
  port:     parsed.port,     // "" (443 is default for HTTPS)
  pathname: parsed.pathname, // "/api/v1/users"
  search:   parsed.search,   // "?role=admin"
  hash:     parsed.hash      // "#profile"
});

```

```http
GET /api/v1/users?role=admin HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) Chrome/124.0.0.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: en-US,en;q=0.9
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Connection: keep-alive

```

#### Explanation

1. **Sanitization & Percent-Encoding:** Characters outside the ASCII set or reserved syntax characters (spaces, `#`, `?`) are percent-encoded (`%20` for spaces).
2. **HSTS Check (HTTP Strict Transport Security):** Before making an HTTP request, the browser checks its internal HSTS preload list. If the domain is registered, it automatically upgrades `http://` to `https://` before sending any network packet.
3. **Header Construction:** The browser injects identity (`User-Agent`), formatting capabilities (`Accept-Encoding`), state (`Cookie`), and security controls (`Sec-Fetch-*`).

#### Output

```text
Parsed Protocol : HTTPS (Port 443)
Destination Host: example.com
Target Resource : /api/v1/users?role=admin
HTTP Raw Frame  : Successfully generated (342 bytes ready for network socket)

```

---

### 2. DNS Resolution (Domain Name System)

#### Definition

Computers communicate over IP addresses ($IPv4$ or $IPv6$), whereas humans use domain names (`example.com`). **DNS Resolution** is the recursive lookup process that resolves a domain name into a network-routable IP address.

#### The 6-Level DNS Lookup Hierarchy

1. **Browser DNS Cache:** Chrome/Firefox maintain internal DNS caches (viewable in Chrome via `chrome://net-internals/#dns`).
2. **OS Cache / Hosts File:** Checks local OS resolver cache and `/etc/hosts` (Unix) or `C:\Windows\System32\drivers\etc\hosts` (Windows).
3. **Router Cache:** Local gateway router cache.
4. **ISP Recursive Resolver:** The configured DNS server (e.g., `8.8.8.8` or ISP default).
5. **Root Name Servers (`.`):** Directs request to the TLD server.
6. **TLD Name Servers (`.com`):** Directs request to the authoritative server.
7. **Authoritative Name Server (`example.com`):** Returns the final IP address mapping record ($A$ or $AAAA$).

#### Code Example

```javascript
// Node.js DNS resolution trace
const dns = require('dns/promises');

async function resolveDomain(domain) {
  // Query IPv4 A Record
  const ipv4Addresses = await dns.resolve4(domain, { ttl: true });
  
  // Query IPv6 AAAA Record
  const ipv6Addresses = await dns.resolve6(domain, { ttl: true });

  return { ipv4Addresses, ipv6Addresses };
}

resolveDomain('example.com').then(console.log);

```

#### Explanation

1. If the IP address is found in the Browser or OS cache, the lookup completes in $\approx 0\text{ ms}$.
2. If uncached, the Recursive Resolver queries Root $\rightarrow$ TLD $\rightarrow$ Authoritative servers.
3. **DNS-over-HTTPS (DoH) / DNS-over-TLS (DoT):** Modern browsers encrypt DNS queries using HTTPS to prevent DNS spoofing, eavesdropping, and man-in-the-middle manipulation.

#### Output

```text
DNS Resolution Results for example.com:
- IPv4 (A Record)    : 93.184.216.34  [TTL: 300s]
- IPv6 (AAAA Record) : 2606:2800:220:1:248:1893:25c8:1946 [TTL: 300s]
Total Lookup Latency : 14ms (Cache Hit) / 85ms (Full Recursive Query)

```

---

### 3. TCP Connection & TLS 1.3 Handshake

#### Definition

Once an IP address is known, the browser opens a network connection via a **TCP 3-Way Handshake** to guarantee reliable data delivery, followed immediately by a **TLS (Transport Layer Security) Handshake** to negotiate encryption keys for HTTPS.

#### Connection Establishment Timeline

Total Latency Formula:


$$T_{\text{connection}} = \text{RTT}_{\text{TCP}} + \text{RTT}_{\text{TLS}}$$

```text
Client (Browser)                              Server (93.184.216.34)
   │                                                    │
   ├───────────── TCP SYN (Seq=0) ─────────────────────►│ ┐
   │                                                    │ │ TCP Handshake
   │◄──────────── TCP SYN-ACK (Seq=0, Ack=1) ───────────┤ │ (1 RTT)
   │                                                    │ │
   ├───────────── TCP ACK (Seq=1, Ack=1) ──────────────►│ ┘
   │                                                    │
   ├───────────── TLS 1.3 ClientHello ─────────────────►│ ┐
   │              + Key Share (Diffie-Hellman)          │ │ TLS 1.3 Handshake
   │                                                    │ │ (1 RTT)
   │◄──────────── TLS 1.3 ServerHello ──────────────────┤ │
   │              + Key Share + Certificate + Finished  │ ┘
   │                                                    │
   ├───────────── Encrypted HTTP GET Data ─────────────►│ ── Application Data

```

#### Code Example

```text
=== TCP/TLS Socket Handshake Metadata ===
Transport Protocol : TCP (Transmission Control Protocol)
Security Protocol  : TLS 1.3 (Transport Layer Security)
Cipher Suite       : TLS_AES_256_GCM_SHA384
Key Exchange       : ECDHE_X25519 (Elliptic-Curve Diffie-Hellman)
ALPN Negotiation   : h2 (HTTP/2 negotiated during TLS handshake)

```

#### Explanation

1. **TCP 3-Way Handshake ($1\text{ RTT}$):**
* `SYN`: Client sends initial sequence number.
* `SYN-ACK`: Server acknowledges and sends its sequence number.
* `ACK`: Connection established.


2. **TLS 1.3 Handshake ($1\text{ RTT}$ vs TLS 1.2's $2\text{ RTT}$):**
* The client sends supported cipher suites and key shares directly inside `ClientHello`.
* Server responds with `ServerHello`, selects the cipher, completes the key exchange, verifies its certificate, and switches immediately to symmetric encryption.


3. **0-RTT Resumption:** If the browser previously connected to this server, TLS 1.3 allows sending encrypted early data in the very first packet.

#### Output

```text
Socket State : CONNECTED
Encrypted Tunnel: ACTIVE (AES-256-GCM)
ALPN Protocol   : h2 (Multiplexed stream ready)
Handshake Delay : 1 RTT (TCP) + 1 RTT (TLS 1.3) = ~48ms total

```

---

### The Critical Rendering Path (CRP)

#### Definition

The Critical Rendering Path is the sequence of steps the browser engine performs to convert HTML, CSS, and JavaScript into pixels on the screen.

#### Architectural Workflow

1. **HTML Parsing $\rightarrow$ DOM Tree:** The engine parses raw HTML bytes, converts them into tokens, creates element nodes, and constructs the **Document Object Model (DOM)** tree.
2. **CSS Parsing $\rightarrow$ CSSOM Tree:** CSS files and inline styles are parsed into the **CSS Object Model (CSSOM)** tree, mapping selectors to rules.
3. **Render Tree Construction:** The browser combines the DOM and CSSOM into a **Render Tree**. Unrendered elements (`<head>`, `display: none`) are omitted.
4. **Layout (Reflow):** The engine calculates the exact geometric coordinates and surface area dimensions for every node in the render tree.
5. **Paint:** The rasterizer converts elements into visual pixels across multiple layout layers (text, colors, borders, shadows).
6. **Compositing:** The GPU arranges these visual layers in correct z-order to draw the final composite frame onto the display.

### 4. DOM (Document Object Model) Construction

#### Definition

The **DOM** is an in-memory tree object representation of an HTML document. The browser engine's HTML Parser converts raw HTML response bytes received over the network into the DOM tree.

#### The Critical Parsing Pipeline

$$\text{Bytes} \longrightarrow \text{Characters} \longrightarrow \text{Tokens} \longrightarrow \text{Nodes} \longrightarrow \text{DOM Tree}$$

```text
[Bytes] "3C 68 74 6D 6C..." ──► [Characters] "<html><body>..."
                                      │
                                      ▼
[DOM Tree] ◄── [Nodes] ◄── [Tokenizer] <start-tag: html>, <start-tag: body>

```

#### Code Example

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Architecture Test</title>
  </head>
  <body>
    <h1>Browser Engine</h1>
    <p>Parsing HTML into DOM nodes.</p>
  </body>
</html>

```

```javascript
// Conceptual DOM Tree Representation generated by browser engine
Document
  └── html (HTMLHtmlElement)
       ├── head (HTMLHeadElement)
       │    └── title (HTMLTitleElement)
       │         └── TextNode: "Architecture Test"
       └── body (HTMLBodyElement)
            ├── h1 (HTMLHeadingElement)
            │    └── TextNode: "Browser Engine"
            └── p (HTMLParagraphElement)
                 └── TextNode: "Parsing HTML into DOM nodes."

```

#### Explanation

1. **Speculative Parsing (Pre-loader):** While the main thread is parsing HTML tokens, a lightweight speculative parser scans ahead in the stream to request external resources (`<script>`, `<link rel="stylesheet">`, `<img>`) early on the network thread.
2. **Parser-Blocking Scripts:** When the HTML parser encounters a `<script>` tag without `async` or `defer`, HTML parsing **stops completely** until the script is downloaded, parsed, and executed.
3. **Incremental Rendering:** HTML parsing is incremental—the browser does not wait for the entire document to download before creating DOM nodes and sending partial frames to be rendered.

#### Output

```text
DOM Construction State: COMPLETE
Node Count            : 7 Nodes created
Parser Status         : Interactive (DOMContentLoaded Fired)

```

---

### 5. CSSOM (CSS Object Model) Construction

#### Definition

While building the DOM, the browser encounters CSS stylesheets (`<link rel="stylesheet">` or `<style>`). The CSS parser reads raw CSS bytes and builds the **CSSOM**—a tree structure containing computed styling rules mapped to selectors.

#### Why CSS is Render-Blocking

Unlike HTML, CSS parsing **cannot be incremental**. Because of the CSS Cascade and inheritance rules, subsequent lines in a stylesheet can override earlier rules. The browser **blocks page rendering** until the CSSOM tree is fully constructed to avoid Flash of Unstyled Content (FOUC).

#### Code Example

```css
body {
  font-size: 16px;
  color: black;
}

header {
  display: flex;
}

header h1 {
  font-size: 24px;
  color: blue;
}

```

```javascript
// Structural Representation of the CSSOM Tree
CSSOM Root
  ├── body Rule: { font-size: 16px; color: black; }
  │    └── header Rule: { display: flex; }
  │         └── h1 Rule: { font-size: 24px; color: blue; } (Inherits color override from parent)

```

#### Selector Matching Mechanics

Browsers evaluate CSS selectors **from right to left** (key selector first).

* For `header h1`: The engine targets all `<h1>` tags first, then traverses up its ancestor tree to check if it resides inside a `<header>`. This avoids traversing the entire DOM tree for non-matching parents.

#### Output

```text
CSSOM Construction State : COMPLETE
Render-Blocking Status   : UNBLOCKED (CSSOM ready for render tree synthesis)
Selector Engine Strategy : Right-to-Left evaluation

```

---

## Browser Architecture

```text
  DOM Tree ──┐
             ├─► [ 6. Render Tree ] ─► [ 7. Layout / Reflow ] ─► [ 8. Paint ] ─► [ 9. Compositing ] ──► Screen
  CSSOM Tree ┘                                                                            ▲
                                                                                          │
  Main Thread Event Loop (Topic 10) ──────────────────────────────────────────────────────┘

```

---

### 6. Render Tree Construction

#### Definition

The **Render Tree** is the visual structural representation of a webpage generated by combining the **DOM** and **CSSOM** trees. It contains only the nodes required to render the page, along with their computed CSS styles.

#### Inclusion & Exclusion Rules

* **Excluded Nodes:**
* Non-visual DOM elements in `<head>` (`<script>`, `<meta>`, `<link>`, `<title>`).
* Elements with `display: none` in CSSOM (and all their descendents).


* **Included Nodes:**
* Visible structural elements (`<div>`, `<p>`, `<span>`).
* Elements with `visibility: hidden` or `opacity: 0` (**Included** because they still occupy spatial dimensions during layout).
* Pseudo-elements generated via CSS (`::before`, `::after`).



#### Code Example

```html
<!-- DOM Tree Source -->
<html>
  <head><title>Test</title></head>
  <body>
    <h1 style="display: none;">Hidden Heading</h1>
    <div class="card">
      <p style="visibility: hidden;">Invisible Paragraph</p>
    </div>
  </body>
</html>

```

```css
/* CSSOM Computed Rules */
.card::before { content: "Card Header"; }

```

```javascript
/* Render Tree Generated by Engine (Structural Representation) */
RenderView (Root)
 └── RenderBlock (body)
      └── RenderBlock (div.card)
           ├── RenderInline (::before)
           │    └── RenderText "Card Header"
           └── RenderBlock (p) [visibility: hidden] -> Included for layout space!

```

#### Explanation

1. The engine starts at the root of the DOM tree (`<html>`) and traverses down every visible node.
2. For each node, it looks up matching rules in the CSSOM to construct a computed style object.
3. `<h1 style="display: none;">` is omitted completely from the Render Tree.
4. `<p style="visibility: hidden;">` is retained in the Render Tree because the engine needs its geometry to reserve space during the subsequent Layout phase.

#### Output

```text
Render Tree Construction State: READY
Active Render Objects         : 5 Render Nodes (1 DOM node & 1 head tag pruned)
Layout Impact                 : 1 invisible node flagged for spatial reservation

```

---

### 7. Layout (Reflow)

#### Definition

The **Layout** phase (historically termed **Reflow** in Gecko/Firefox engines) calculates the exact geometric coordinates $(x, y)$, dimensions $(\text{width}, \text{height})$, and bounding boxes for every node in the Render Tree relative to the viewport.

#### Layout Triggers & Performance Cost

Layout is a CPU-intensive operation. Triggering Reflow on a single element forces the engine to recalculate geometry for surrounding siblings and parent wrappers in the DOM tree.

Common Reflow Triggers:

* Resizing the browser viewport.
* Modifying geometric CSS properties (`width`, `height`, `margin`, `padding`, `border`, `fontSize`, `display`, `position`).
* Adding/removing DOM nodes.
* Reading geometric properties in JS (**Forced Synchronous Layout**).

#### Code Example: Layout Thrashing (Anti-Pattern vs. Fix)

```javascript
// BAD PRACTICE: Layout Thrashing (Forced Synchronous Layout in a loop)
// Reads and writes geometry iteratively, forcing browser to compute layout on EVERY frame.
function badResizeBoxes(boxes) {
  for (let i = 0; i < boxes.length; i++) {
    // READ (Forces immediate layout calculation)
    const currentWidth = boxes[i].offsetWidth; 
    // WRITE (Invalidates layout)
    boxes[i].style.width = (currentWidth + 10) + 'px'; 
  }
}

// GOOD PRACTICE: Batching Reads and Writes
function goodResizeBoxes(boxes) {
  // Step 1: Batch READS
  const widths = boxes.map(box => box.offsetWidth); 
  
  // Step 2: Batch WRITES
  boxes.forEach((box, i) => {
    box.style.width = (widths[i] + 10) + 'px';
  });
}

```

#### Explanation

1. In `badResizeBoxes`, calling `offsetWidth` immediately after setting `style.width` forces the browser to interrupt execution and perform an synchronous layout recalculation on every iteration.
2. In `goodResizeBoxes`, reads are completed first. The browser queues style writes lazily and performs a single batched layout pass on the next animation frame.

#### Output

```text
Layout Thrashing Execution (100 Elements):
- Bad Pattern  : 100 Synchronous Reflows -> Total Frame Time: ~85ms (Frame Drop / Jank)
- Good Pattern : 1 Batched Reflow        -> Total Frame Time: ~2.1ms (Smooth 60 FPS)

```

---

### 8. Paint & Repaint

#### Definition

The **Paint** phase converts the physical geometric bounds computed during Layout into actual visual pixels on screen. The browser constructs a sequence of visual draw commands called a **Paint Record** (a display list of instructions like `drawRect()`, `drawText()`, `drawImage()`).

#### Repaint vs. Reflow

* **Reflow + Repaint:** Occurs when geometry changes (`width`, `height`, `top`). The engine recalculates positions (Reflow) and redraws pixels (Repaint).
* **Repaint Only:** Occurs when visual appearance changes without altering geometry (`color`, `background-color`, `visibility`, `box-shadow`). The browser skips Layout entirely and re-executes Paint commands.

#### Stacking Order in Paint Phase

Elements are painted onto visual surfaces in a specific back-to-front layer order:


$$\text{Background} \longrightarrow \text{Borders} \longrightarrow \text{Block Children} \longrightarrow \text{Positioned Elements} \longrightarrow \text{Text/Foreground}$$

#### Code Example

```css
/* Triggers REFLOW + REPAINT (Heavy) */
.box-reflow {
  width: 100px;
  /* Alters layout geometry; forces Layout -> Paint -> Composite */
}

/* Triggers REPAINT ONLY (Medium) */
.box-repaint {
  background-color: red;
  /* Geometry unchanged; skips Layout -> forces Paint -> Composite */
}

/* Triggers COMPOSITE ONLY (Ultra Light) */
.box-composite {
  transform: translateX(100px);
  opacity: 0.8;
  /* Skips Layout & Paint -> Handled on GPU during Compositing! */
}

```

#### Explanation

1. Modifying `.box-repaint` invalidates the visual pixel rectangle. The browser marks the region as a "dirty rect" and re-issues paint rasterization commands for that region without recomputing element geometries.
2. Paint operations are CPU-bound. Complex visual effects like heavy `box-shadow` or `backdrop-filter` slow down rasterization throughput.

#### Output

```text
Pipeline Stages Triggered:
- .box-reflow    : [ Layout (0.8ms) ] -> [ Paint (1.2ms) ] -> [ Composite (0.1ms) ]
- .box-repaint   : [ SKIPPED LAYOUT ] -> [ Paint (1.1ms) ] -> [ Composite (0.1ms) ]
- .box-composite : [ SKIPPED LAYOUT ] -> [ SKIPPED PAINT ] -> [ Composite (0.1ms) ]

```

---

### 9. Compositing & Layer Promotion

#### Definition

**Compositing** is the final stage of the rendering pipeline. The browser splits the webpage into separate visual layers (**RenderLayers** / **GraphicsLayers**), paints each layer independently on the CPU, uploads them as textures to the **GPU**, and blends (composites) them together onto the screen buffer.

#### Layer Promotion Triggers

An element is promoted to its own hardware-accelerated GPU layer when it meets criteria such as:

* 3D transforms (`transform: translate3d()` or `translateZ(0)`).
* Hardware-accelerated `<video>`, `<canvas>`, or WebGL elements.
* Elements using `will-change: transform` or `will-change: opacity`.
* Elements with `position: fixed` or `position: sticky`.
* Elements overlapping above a promoted GPU layer (**Implicit Compositing**).

#### Code Example

```css
.hero-card {
  /* Explicitly promote element to its own GPU Compositing Layer */
  will-change: transform, opacity;
  
  /* Isolate stacking context to prevent implicit layer leakage */
  isolation: isolate; 
}

.hero-card:hover {
  /* Animated strictly on GPU without triggering CPU Reflow or Repaint */
  transform: scale(1.05) translate3d(0, -10px, 0);
  opacity: 0.9;
}

```

#### Explanation

1. By setting `will-change: transform`, the compositor thread pre-allocates a dedicated GPU texture for `.hero-card`.
2. During the hover animation, the main thread is completely bypassed. The GPU transforms and scales the pre-rasterized layer texture directly, maintaining **60 FPS / 120 FPS** animations even during high JS thread loads.
3. **VRAM Trade-off:** Promoting too many elements to GPU layers consumes excessive memory and can lead to performance degradation.

#### Output

```text
Compositor Layer Tree:
├── Root Layer (Main Document) [CPU Rendered]
└── GraphicsLayer (.hero-card) [GPU VRAM Texture Allocation: 1.2 MB]
    └── Operations: GPU Scaling & Translation ONLY (0ms Main Thread CPU execution)

```

---

### 10. The Event Loop, Task Queues & Rendering Cycle

#### Definition

The **Event Loop** is the single-threaded concurrency engine running on the browser's Main Thread. It coordinates JavaScript code execution, event processing, asynchronous callbacks, microtasks, and orchestrates the periodic **Rendering Engine Frame Pipeline** ($60\text{ Hz} \approx 16.6\text{ ms}$ budget per frame). JavaScript is a single-threaded, non-blocking, asynchronous runtime language powered by engines like V8.

#### Core Components

* **Call Stack:** Single-threaded LIFO (Last In, First Out) stack that executes synchronous code frames.
* **Memory Heap:** Unstructured memory pool for object allocation and reference variables.
* **Web APIs:** Multi-threaded browser APIs handling async operations (`fetch`, DOM events, `setTimeout`).
* **Microtask Queue:** High-priority queue reserved for Promise callbacks, `queueMicrotask`, and `MutationObserver`.
* **Macrotask (Task) Queue:** Queue for I/O operations, timers (`setTimeout`), and event handlers.

#### Queue Hierarchy & Execution Order

In every tick of the Event Loop, execution strictly follows this pipeline:

1. **Call Stack:** Executes current synchronous JavaScript frame until empty.
2. **Microtask Queue:** Emptied **completely** after *every* single task before moving forward (`Promise.then`, `queueMicrotask`, `MutationObserver`).
3. **Macrotask Queue (Task Queue):** Executes **ONE** macrotask per loop tick (`setTimeout`, `setInterval`, I/O, user click events).
4. **Rendering Phase (Every 16.6ms):**
* Process `window.resize` / `scroll` events.
* Run `requestAnimationFrame` (rAF) callbacks (**Right before layout/paint!**).
* Execute Style $\rightarrow$ Layout $\rightarrow$ Paint $\rightarrow$ Composite.


5. **Idle Period:** Runs `requestIdleCallback` if main thread has free time left in the 16.6ms frame budget.

```text
[ Call Stack Executes ] ──► [ Exhaust ALL Microtasks ] ──► [ Is Render Frame Due? ]
                                                                 │
                                                       YES ┌─────┴─────┐ NO
                                                           │           │
   [ Screen Output ] ◄── [ Style/Layout/Paint ] ◄── [ Run rAF ]        └──► [ Run 1 Macrotask ]

```

#### Code Example

```javascript
console.log('1. Synchronous Start');

setTimeout(() => {
  console.log('2. Macrotask (setTimeout)');
}, 0);

Promise.resolve().then(() => {
  console.log('3. Microtask 1 (Promise)');
});

queueMicrotask(() => {
  console.log('4. Microtask 2 (queueMicrotask)');
});

requestAnimationFrame(() => {
  console.log('5. Rendering Phase (rAF)');
});

console.log('6. Synchronous End');

```

#### Explanation

1. `1. Synchronous Start` prints immediately. `setTimeout` registers a Macrotask. The two Promises register Microtasks. `requestAnimationFrame` registers an animation frame callback. `6. Synchronous End` prints.
2. Main Call Stack empties. The Event Loop inspects the **Microtask Queue** and executes ALL queued microtasks: printing `3. Microtask 1` followed by `4. Microtask 2`.
3. If a screen frame refresh is due ($\sim 16.6\text{ ms}$ tick), the rendering loop fires, executing `5. Rendering Phase (rAF)` *before* layout and paint.
4. Finally, the Event Loop pulls **ONE** item from the Macrotask Queue, printing `2. Macrotask (setTimeout)`.

#### Output

```text
Execution Order Console Log:
1. Synchronous Start
6. Synchronous End
3. Microtask 1 (Promise)
4. Microtask 2 (queueMicrotask)
5. Rendering Phase (rAF)      <-- Executed before browser paint!
2. Macrotask (setTimeout)

```

---

## Modern Browser Threading, Storage, and Real-Time Networking APIs

---

### Browser Threading & Asynchronous Offloading

```text
  Main Thread (DOM / UI / Event Loop)
      │
      ├── Worker PostMessage ──► [ Web Worker ] (Heavy Computation / CPU offload)
      │
      ├── Network Request ─────► [ Service Worker ] (Network Proxy / Cache / Offline)
      │
      ├── Async Storage ───────► [ IndexedDB ] (Structured NoSQL Transactional DB)
      │
      └── Persistent Connection ─► [ WebSocket ] (Full-Duplex Real-Time Socket)

```

---

### 11. Web Workers (Dedicated Multithreading)

#### Definition

JavaScript on the browser main thread is single-threaded. **Web Workers** run scripts in background threads, completely detached from the main thread. This allows intensive CPU operations (image processing, data manipulation, encryption, complex algorithms) to execute concurrently without causing UI freezing or dropping frame rates.

#### Key Constraints

* **No DOM Access:** Web Workers run in a `WorkerGlobalScope` and cannot access `document`, `window`, or direct DOM nodes.
* **Structured Clone Algorithm:** Communication between the main thread and workers occurs via `postMessage()`. Data is serialized/deserialized using the structured clone algorithm, or transferred via zero-copy `ArrayBuffer` objects (**Transferables**).

#### Code Example

```javascript
// === main.js ===
// 1. Instantiate background worker thread
const calculationWorker = new Worker('worker.js');

// 2. Listen for messages returned from worker
calculationWorker.onmessage = function (event) {
  console.log('Main Thread Received Result:', event.data.result);
};

// 3. Send payload to background thread
calculationWorker.postMessage({ number: 40 });

// === worker.js ===
// Dedicated Worker Global Context
self.onmessage = function (event) {
  const { number } = event.data;
  
  // Perform heavy CPU computation (e.g., Fibonacci)
  const result = fibonacci(number);
  
  // Post message back to Main Thread
  self.postMessage({ result });
};

function fibonacci(n) {
  return n <= 1 ? n : fibonacci(n - 1) + fibonacci(n - 2);
}

```

#### Explanation

1. The main thread spawns `worker.js` off the main loop.
2. When `postMessage` is called, the heavy `fibonacci` recalculation executes on an isolated OS background thread.
3. The main thread's Event Loop remains entirely unimpeded, maintaining smooth 60–120 FPS scrolling and UI responsiveness.

#### Output

```text
Main Thread Status : 100% Responsive during execution
Worker Thread      : Computed Fibonacci(40) = 102334155 on background thread
Total UI Freezes   : 0ms

```

---

### 12. Service Workers (Network Proxy & Offline Engine)

#### Definition

A **Service Worker** is an event-driven background worker that acts as a programmable network proxy between the browser, web app, and network. It intercepts HTTP requests, manages local cache storage, and enables features like offline functionality, background data synchronization, and push notifications.

#### Service Worker Lifecycle

1. **Registration:** Initiated by the main thread.
2. **Installation (`install` event):** Pre-caches static application assets.
3. **Activation (`activate` event):** Cleans up legacy cache versions.
4. **Idle / Intercept (`fetch`, `push`, `sync` events):** Handles background network events.

#### Code Example

```javascript
// === main.js ===
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js')
    .then(reg => console.log('SW Registered Scope:', reg.scope))
    .catch(err => console.error('SW Registration Failed:', err));
}

// === sw.js ===
const CACHE_NAME = 'app-v1';
const ASSETS_TO_CACHE = ['/', '/index.html', '/styles.css', '/app.js'];

// Install Phase: Pre-cache core shell assets
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => cache.addAll(ASSETS_TO_CACHE))
  );
});

// Fetch Interception: Cache-First Strategy with Network Fallback
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((cachedResponse) => {
      // Return cached asset if hit; otherwise fetch from network
      return cachedResponse || fetch(event.request);
    })
  );
});

```

#### Explanation

1. During `install`, `sw.js` caches static application assets (`Cache Storage API`).
2. Every subsequent HTTP request emitted by the page passes through the worker's `fetch` handler.
3. If network connection is lost, `caches.match()` supplies cached offline assets directly, turning the web app into a reliable Progressive Web App (PWA).

#### Output

```text
Network State : OFFLINE
Interceptor   : Service Worker intercepted GET /index.html
Response      : 200 OK (Served directly from SW Cache Storage - 0ms latency)

```

---

## Client-Side Storage & Real-Time Communication

### 13. IndexedDB (Transactional NoSQL Database)

#### Definition

**IndexedDB** is a low-level, asynchronous, transactional NoSQL object store built natively into the browser. Unlike `localStorage` (which is synchronous, blocking, text-only, and limited to $\sim 5\text{ MB}$), IndexedDB can store massive volumes of structured data (JS objects, `Blob`s, `ArrayBuffer`s, files) with indexed querying capabilities.

#### Structural Concepts

* **Database & Object Stores:** Equivalent to tables in relational databases.
* **Transactions:** All reads and writes occur inside atomic transaction boundaries (`readonly` or `readwrite`).
* **Indexes:** Secondary lookups for querying data by properties other than primary keys.

#### Code Example

```javascript
// Open IndexedDB connection
const request = indexedDB.open('UserDB', 1);

// Handle Schema Upgrades & Table Creation
request.onupgradeneeded = (event) => {
  const db = event.target.result;
  // Create an object store using 'id' as auto-increment key
  const store = db.createObjectStore('users', { keyPath: 'id', autoIncrement: true });
  // Create secondary index for searching by email
  store.createIndex('email', 'email', { unique: true });
};

request.onsuccess = (event) => {
  const db = event.target.result;
  
  // Create a readwrite transaction
  const transaction = db.transaction(['users'], 'readwrite');
  const store = transaction.objectStore('users');
  
  // Add a structured record
  const addRequest = store.add({
    name: 'Alice',
    email: 'alice@example.com',
    preferences: { theme: 'dark', notifications: true }
  });

  addRequest.onsuccess = () => {
    console.log('Record inserted successfully!');
  };
};

```

#### Explanation

1. `indexedDB.open` triggers `onupgradeneeded` on initial run or version updates to define the database schema.
2. `db.transaction()` wraps operations inside acid-compliant transactions. If an error occurs, modifications revert automatically.
3. Writes are non-blocking and execute asynchronously on the storage engine.

#### Output

```text
Database          : UserDB (v1)
ObjectStore       : "users" created
Inserted Object   : { id: 1, name: "Alice", email: "alice@example.com", ... }
Storage Capacity  : Hundreds of Megabytes / Gigabytes (Subject to disk space quota)

```

---

### 14. WebSockets (Full-Duplex Persistent Communication)

#### Definition

The **WebSocket API** provides a persistent, low-latency, bidirectional communication channel over a single TCP socket connection.

Unlike traditional HTTP (which uses request-response polling and carries header overhead on every frame), WebSockets start with an HTTP Upgrade request (`ws://` or `wss://`) and transition into a lightweight TCP framing protocol.

#### HTTP vs WebSocket Header Overhead

* **HTTP/1.1 Request:** $\sim 500 - 2000\text{ bytes}$ of header metadata per request.
* **WebSocket Data Frame:** As small as $2 - 10\text{ bytes}$ per packet after connection handshake.

#### Code Example

```javascript
// Establish persistent encrypted WebSocket connection
const socket = new WebSocket('wss://stream.example.com/live-data');

// Handshake Opened
socket.addEventListener('open', (event) => {
  console.log('WebSocket Connection Established!');
  // Send JSON payload to server
  socket.send(JSON.stringify({ action: 'subscribe', channel: 'stock-ticker' }));
});

// Receive Real-Time Messages pushed by Server
socket.addEventListener('message', (event) => {
  const data = JSON.parse(event.data);
  console.log('Server Push Received:', data);
});

// Handle Connection Closure
socket.addEventListener('close', (event) => {
  console.log('Connection closed cleanly:', event.wasClean);
});

```

#### Connection Lifecycle

```text
Client                                             Server
  │                                                  │
  ├────── HTTP GET /live-data ──────────────────────►│ ┐
  │       Upgrade: websocket                         │ │ HTTP Upgrade Handshake
  │       Connection: Upgrade                        │ │
  │◄───── HTTP 101 Switching Protocols ──────────────┤ ┘
  │                                                  │
  │◄═════ Bi-Directional TCP Socket Protocol ═══════►│ ── Low-latency Full Duplex Frames

```

#### Explanation

1. The client initiates a standard HTTP request with `Upgrade: websocket`.
2. The server responds with status code `101 Switching Protocols`.
3. The underlying TCP socket connection remains open, allowing both client and server to push frames freely with minimal overhead.

#### Output

```text
Protocol           : WSS (WebSocket Secure)
Handshake State    : 101 Switching Protocols
Latency            : Real-time (<5ms frame transmission overhead)
Duplex Operational : TRUE (Server can push updates asynchronously without client polling)

```

---