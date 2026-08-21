# Software Architecture and Design Principles

## 1. Core Software Design Principles

* **DRY (Don't Repeat Yourself):** Focuses on eliminating redundant patterns by replacing repeated logic with abstractions, functions, or data normalization, making codebase updates single-point and low-maintenance.
* **DIE (Duplication Is Evil):** A reinforcing philosophy to DRY emphasizing that duplicated logic introduces bugs when one copy is updated while another is missed.
* **SOLID Principles:**
    * **S - Single Responsibility Principle (SRP):** A class/module should have one, and only one, reason to change.
    * **O - Open/Closed Principle (OCP):** Software entities should be open for extension (e.g., via inheritance or strategy patterns) but closed for modification.
    * **L - Liskov Substitution Principle (LSP):** Subtypes must be substitutable for their base types without altering the correctness of the program.
    * **I - Interface Segregation Principle (ISP):** Clients should not be forced to depend upon interfaces they do not use; split large interfaces into smaller, specific ones.
    * **D - Dependency Inversion Principle (DIP):** High-level modules should depend on abstractions (interfaces), not concrete low-level implementation details.

---

## 2. Module Design Dynamics

### Cohesion

Cohesion measures how tightly focused and functionally related the elements within a single module are. Higher functional cohesion is preferred.

* **High Cohesion (Desirable):** *Functional* (all elements perform a single essential task).
* **Intermediate Cohesion:** *Communicational* (operate on same data), *Sequential* (output of one feeds the next), *Procedural* (executed in a specific order).
* **Low Cohesion (Avoid):** *Temporal* (grouped because they happen at the same time), *Logical* (grouped because they do similar broad tasks), *Coincidental* (arbitrary grouping with no meaningful relationship).

### Coupling

Coupling measures the degree of interdependence between different modules or components. Low/loose coupling is preferred.

* **Loose Coupling / Uncoupled (Desirable):** *No coupling*, *Data coupling* (passing only minimal scalar data parameters), *Stamp coupling* (passing composite data structures where only a portion is needed).
* **Tight Coupling (Avoid):** *Control coupling* (one module controls the internal execution flow of another via flags), *External coupling* (modules share an external protocol or hardware format), *Common coupling* (modules share global data), *Content coupling* (one module directly modifies or relies on the internal data/implementation of another).

---

## 3. Database & Storage Concepts

* **ACID Guarantees:**
* **Atomicity:** All operations in a transaction succeed, or the entire transaction is rolled back (all-or-nothing).
* **Consistency:** Data moves from one valid state to another, preserving all database constraints and rules.
* **Isolation:** Concurrent transactions execute independently without interfering with each other's intermediate state.
* **Durability:** Once committed, transaction results persist permanently, even in the event of a system crash.
* **Sharding:** A horizontal scaling technique where a single logical dataset is partitioned across multiple distinct database servers/nodes to handle larger read/write volumes and storage capacity.

---

## 4. Performance Testing Best Practices

1. **Define Scope & Plan:** Establish clear non-functional requirements (e.g., target throughput, latency SLAs, peak load limits).
2. **Component vs. End-to-End Testing:** Test individual services/microservices in isolation to isolate bottlenecks before conducting integrated system testing.
3. **Agile Integration:** Incorporate performance validation continuous integration pipelines rather than waiting until the end of a release cycle.
4. **Test Early & Frequently:** Identify performance regressions early when architectural changes are less costly to make.
1
## 1. High Cohesion vs. Low Cohesion

Cohesion describes how closely related the functions inside a single class or module are.

### Low Cohesion (Avoid)

A single `UserManager` class handles database access, email delivery, input validation, and log formatting. Elements are grouped arbitrarily rather than functionally.

```typescript
class UserManager {
  // Responsibility 1: Database Operations
  saveUserToDatabase(user: any) {
    console.log(`INSERT INTO users VALUES (${user.name})`);
  }

  // Responsibility 2: Email Processing
  sendWelcomeEmail(email: string) {
    console.log(`Sending email to ${email}`);
  }

  // Responsibility 3: Formatting & Analytics
  formatUserLogs(user: any): string {
    return `[LOG] User ${user.name} logged in at ${new Date().toISOString()}`;
  }
}

```

### High Cohesion (Prefer)

The responsibilities are split so that each class focuses on one specific, well-defined operational boundary.

```typescript
// Focused purely on database operations
class UserRepository {
  save(user: User): void {
    console.log(`INSERT INTO users VALUES (${user.name})`);
  }
}

// Focused purely on notification delivery
class EmailService {
  sendWelcome(email: string): void {
    console.log(`Sending email to ${email}`);
  }
}

// Focused purely on user activity logging
class UserLogger {
  formatLoginLog(user: User): string {
    return `[LOG] User ${user.name} logged in at ${new Date().toISOString()}`;
  }
}

```

---

## 2. Tight Coupling vs. Loose Coupling

Coupling describes how interdependent two separate modules are.

### Tight Coupling (Avoid)

The `OrderProcessor` directly instantiates a concrete `StripePaymentProcessor`. Switching to PayPal or mocking the payment provider for unit testing requires changing `OrderProcessor` source code.

```typescript
class StripePaymentProcessor {
  chargeStripe(amount: number) {
    console.log(`Charged $${amount} via Stripe API`);
  }
}

class OrderProcessor {
  private paymentProcessor: StripePaymentProcessor;

  constructor() {
    // Hard dependency on concrete implementation
    this.paymentProcessor = new StripePaymentProcessor();
  }

  processOrder(amount: number) {
    this.paymentProcessor.chargeStripe(amount);
  }
}

```

### Loose Coupling (Prefer)

`OrderProcessor` depends on a `PaymentProcessor` interface. Concrete implementations (`StripeService`, `PayPalService`, `MockPaymentService`) are injected via constructor, allowing easy swapping and unit testing without modifying `OrderProcessor`.

```typescript
// Shared Abstraction
interface PaymentProcessor {
  charge(amount: number): void;
}

// Concrete Implementation 1
class StripeService implements PaymentProcessor {
  charge(amount: number): void {
    console.log(`Charged $${amount} via Stripe API`);
  }
}

// Concrete Implementation 2
class PayPalService implements PaymentProcessor {
  charge(amount: number): void {
    console.log(`Charged $${amount} via PayPal API`);
  }
}

// High-level module depends on the abstraction, not the concrete class
class OrderProcessor {
  constructor(private paymentProcessor: PaymentProcessor) {}

  processOrder(amount: number) {
    this.paymentProcessor.charge(amount);
  }
}

// Usage with Dependency Injection
const stripeProcessor = new OrderProcessor(new StripeService());
stripeProcessor.processOrder(100);

const paypalProcessor = new OrderProcessor(new PayPalService());
paypalProcessor.processOrder(100);

```

Here are concise TypeScript examples demonstrating each of the five SOLID principles.

---

## 1. Single Responsibility Principle (SRP)

> *A class should have one, and only one, reason to change.*

Instead of having a single `Invoice` class handle calculations, database persistence, and printing, separate each responsibility into distinct classes.

```typescript
class Invoice {
  constructor(public amount: number) {}
}

// Responsibility 1: Data persistence
class InvoiceRepository {
  save(invoice: Invoice): void {
    console.log(`Saving invoice of amount ${invoice.amount} to database.`);
  }
}

// Responsibility 2: Presentation/Formatting
class InvoicePrinter {
  printHtml(invoice: Invoice): string {
    return `<div>Invoice Amount: $${invoice.amount}</div>`;
  }
}

```

---

## 2. Open/Closed Principle (OCP)

> *Software entities should be open for extension, but closed for modification.*

Extend functionality by adding new classes that implement an interface rather than modifying existing `switch` or `if/else` logic inside a core calculation function.

```typescript
interface DiscountStrategy {
  apply(price: number): number;
}

class StandardDiscount implements DiscountStrategy {
  apply(price: number): number { return price * 0.95; }
}

class VIPDiscount implements DiscountStrategy {
  apply(price: number): number { return price * 0.80; }
}

// Closed for modification: supports new discount types without changing calculate()
class PriceCalculator {
  calculate(price: number, discount: DiscountStrategy): number {
    return discount.apply(price);
  }
}

```

---

## 3. Liskov Substitution Principle (LSP)

> *Subtypes must be substitutable for their base types without breaking application behavior.*

Avoid forcing subclasses to implement methods they cannot support (e.g., making an `Ostrich` class throw an error inside a `fly()` method inherited from a base `Bird` class).

```typescript
interface Bird {
  eat(): void;
}

interface FlyingBird extends Bird {
  fly(): void;
}

class Eagle implements FlyingBird {
  eat() { console.log("Eating..."); }
  fly() { console.log("Flying high..."); }
}

// Ostrich implements only Bird, maintaining contract integrity without throwing errors
class Ostrich implements Bird {
  eat() { console.log("Eating..."); }
}

function makeBirdEat(bird: Bird) {
  bird.eat(); // Safe for any Bird subtype
}

```

---

## 4. Interface Segregation Principle (ISP)

> *Clients should not be forced to depend upon interfaces they do not use.*

Split monolithic interfaces into smaller, dedicated ones so simpler classes don't have to provide empty/dummy implementations.

```typescript
interface Printer {
  print(document: string): void;
}

interface Scanner {
  scan(): string;
}

// Basic printer only implements Printer
class BasicPrinter implements Printer {
  print(doc: string) { console.log(`Printing: ${doc}`); }
}

// All-in-one printer implements both
class MultiFunctionMachine implements Printer, Scanner {
  print(doc: string) { console.log(`Printing: ${doc}`); }
  scan(): string { return "Scanned document content"; }
}

```

---

## 5. Dependency Inversion Principle (DIP)

> *High-level modules should not depend on low-level modules. Both should depend on abstractions.*

The `UserService` depends on a `Logger` interface rather than directly instantiating a concrete `ConsoleLogger` or `FileLogger`.

```typescript
// Abstraction
interface Logger {
  log(message: string): void;
}

// Low-level module
class ConsoleLogger implements Logger {
  log(message: string): void {
    console.log(`[CONSOLE]: ${message}`);
  }
}

// High-level module depends on abstraction
class UserService {
  constructor(private logger: Logger) {}

  registerUser(username: string): void {
    // Business logic...
    this.logger.log(`User ${username} registered successfully.`);
  }
}

// Dependency injection in practice
const userService = new UserService(new ConsoleLogger());
userService.registerUser("alex");

```

Design patterns are reusable solutions to recurring design problems. They naturally enforce SOLID principles because they were created to address the exact coupling and rigidity issues SOLID aims to prevent.

---

## 1. Strategy Pattern

The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable at runtime.

```typescript
// Abstraction
interface ExportStrategy {
  export(data: object[]): string;
}

// Concrete Strategies (SRP: Each strategy handles only its export format)
class CsvExportStrategy implements ExportStrategy {
  export(data: object[]): string { return "CSV data stream"; }
}

class JsonExportStrategy implements ExportStrategy {
  export(data: object[]): string { return JSON.stringify(data); }
}

// Context Class
class ReportExporter {
  // DIP: High-level class depends on the ExportStrategy abstraction
  constructor(private strategy: ExportStrategy) {}

  setStrategy(strategy: ExportStrategy) {
    this.strategy = strategy;
  }

  generateReport(data: object[]): string {
    return this.strategy.export(data);
  }
}

```

### How Strategy Enforces SOLID:

* **Open/Closed Principle (OCP):** You can add new formats (`PdfExportStrategy`, `XmlExportStrategy`) by creating new classes without modifying `ReportExporter`.
* **Single Responsibility Principle (SRP):** Formatting logic lives in dedicated strategy classes rather than polluting the core business logic.
* **Dependency Inversion Principle (DIP):** `ReportExporter` depends on the `ExportStrategy` interface rather than concrete exporters.

---

## 2. Factory Method Pattern

The Factory Method pattern defines an interface for creating objects, but lets subclasses decide which class to instantiate.

```typescript
// Product Interface
interface Logger {
  log(message: string): void;
}

class FileLogger implements Logger {
  log(message: string) { console.log(`[FILE] ${message}`); }
}

class CloudLogger implements Logger {
  log(message: string) { console.log(`[CLOUD] ${message}`); }
}

// Creator Abstract Class (SRP: Isolates creation from usage)
abstract class LoggerFactory {
  abstract createLogger(): Logger;

  // Client workflow works with the abstraction
  logMessage(message: string): void {
    const logger = this.createLogger();
    logger.log(message);
  }
}

// Concrete Factories
class FileLoggerFactory extends LoggerFactory {
  createLogger(): Logger { return new FileLogger(); }
}

class CloudLoggerFactory extends LoggerFactory {
  createLogger(): Logger { return new CloudLogger(); }
}

```

### How Factory Method Enforces SOLID:

* **Single Responsibility Principle (SRP):** Object creation logic is separated from business operations using the product.
* **Open/Closed Principle (OCP):** Supporting a new logger type (`DatabaseLogger`) only requires creating a `DatabaseLogger` class and a `DatabaseLoggerFactory` without modifying existing factory code.
* **Dependency Inversion Principle (DIP):** Application workflows depend on the `Logger` interface, deferring exact class instantiation to subclasses.

---

## 3. Adapter Pattern

The Adapter pattern allows incompatible interfaces to work together by wrapping an existing class with a new interface.

```typescript
// Target Interface (used across existing app)
interface PaymentGateway {
  processPayment(amountInCents: number): void;
}

// Incompatible 3rd Party Service (Adaptee)
class LegacyPaypalApi {
  makeTransaction(dollars: number): void {
    console.log(`Processed $${dollars} transaction on Paypal.`);
  }
}

// Adapter Class
class PaypalAdapter implements PaymentGateway {
  constructor(private legacyPaypal: LegacyPaypalApi) {}

  processPayment(amountInCents: number): void {
    const dollars = amountInCents / 100;
    // Translates the target call into the adaptee call
    this.legacyPaypal.makeTransaction(dollars);
  }
}

```

### How Adapter Enforces SOLID:

* **Single Responsibility Principle (SRP):** Translation and impedance-matching logic between two incompatible systems is isolated inside the adapter.
* **Open/Closed Principle (OCP):** New 3rd-party vendors or legacy APIs can be integrated without modifying the client code or the 3rd-party SDK code.
* **Liskov Substitution Principle (LSP):** The `PaypalAdapter` seamlessly substitutes for any native `PaymentGateway` without altering client behavior expectations.

---

## SOLID-to-Pattern Mapping Matrix

| Pattern | Primary SOLID Principles Enforced | Key Architectural Benefit |
| --- | --- | --- |
| **Strategy** | OCP, DIP, SRP | Swap algorithms at runtime without changing caller code. |
| **Factory Method** | SRP, OCP, DIP | Decouple object creation from business execution. |
| **Adapter** | OCP, SRP, LSP | Integrate legacy/third-party APIs without modifying core code. |
| **Observer** | OCP, DIP | Decouple state publishers from subscribers. |
| **Decorator** | OCP, SRP | Extend object behavior dynamically without using deep inheritance. |

## 1. Decorator Pattern

The Decorator pattern allows behavior to be added to individual objects dynamically, without affecting the behavior of other objects from the same class, by placing these objects inside wrapper objects that contain the behaviors.

```typescript
// Component Interface
interface Notifier {
  send(message: string): void;
}

// Concrete Component
class BasicNotifier implements Notifier {
  send(message: string): void {
    console.log(`Sending Email: ${message}`);
  }
}

// Base Decorator (implements the same interface and holds a reference to a Notifier)
abstract class NotifierDecorator implements Notifier {
  constructor(protected wrapped: Notifier) {}

  abstract send(message: string): void;
}

// Concrete Decorator 1: Adds Slack notification capability
class SlackDecorator extends NotifierDecorator {
  send(message: string): void {
    this.wrapped.send(message); // Execute original behavior
    console.log(`Sending Slack Notification: ${message}`); // Add new behavior
  }
}

// Concrete Decorator 2: Adds SMS notification capability
class SmsDecorator extends NotifierDecorator {
  send(message: string): void {
    this.wrapped.send(message);
    console.log(`Sending SMS Notification: ${message}`);
  }
}

// Usage: Dynamically combine behaviors at runtime
const notifier: Notifier = new SmsDecorator(
  new SlackDecorator(
    new BasicNotifier()
  )
);

notifier.send("Server CPU usage is at 95%!");

```

### How the Decorator Pattern Enforces SOLID:

* **Open/Closed Principle (OCP):** You can extend the functionality of a `Notifier` (adding push notifications, logs, or encryption) by writing new decorator classes without modifying the core `BasicNotifier` or existing decorators.
* **Single Responsibility Principle (SRP):** Instead of stuffing every notification channel into a monolithic `NotificationManager` class, each decorator is responsible for one specific supplementary concern (e.g., Slack integration or SMS delivery).

---

## 2. Observer Pattern

The Observer pattern defines a subscription mechanism to notify multiple objects about any events that happen to the object they are observing (the Subject).

```typescript
// Observer Interface
interface OrderObserver {
  update(orderId: string): void;
}

// Subject Class
class OrderPublisher {
  private observers: OrderObserver[] = [];

  subscribe(observer: OrderObserver): void {
    this.observers.push(observer);
  }

  unsubscribe(observer: OrderObserver): void {
    this.observers = this.observers.filter(obs => obs !== observer);
  }

  notify(orderId: string): void {
    for (const observer of this.observers) {
      observer.update(orderId);
    }
  }
}

// Concrete Subject (High-level business workflow)
class OrderService extends OrderPublisher {
  createOrder(orderId: string): void {
    console.log(`[Database]: Order ${orderId} successfully saved.`);
    this.notify(orderId); // Trigger event listeners
  }
}

// Concrete Observer 1
class InventoryService implements OrderObserver {
  update(orderId: string): void {
    console.log(`[Inventory]: Reserving items for order ${orderId}`);
  }
}

// Concrete Observer 2
class BillingService implements OrderObserver {
  update(orderId: string): void {
    console.log(`[Billing]: Generating invoice for order ${orderId}`);
  }
}

// Usage
const orderService = new OrderService();
orderService.subscribe(new InventoryService());
orderService.subscribe(new BillingService());

orderService.createOrder("ORD-98765");

```

### How the Observer Pattern Enforces SOLID:

* **Open/Closed Principle (OCP):** You can introduce entirely new listeners (e.g., `AnalyticsService`, `EmailMarketingService`) by implementing the `OrderObserver` interface and subscribing them, without changing a single line of code inside `OrderService`.
* **Dependency Inversion Principle (DIP):** The `OrderService` depends solely on the `OrderObserver` abstraction interface rather than being tightly coupled to concrete lower-level services like `InventoryService` or `BillingService`.
* **Single Responsibility Principle (SRP):** The core order-creation workflow remains strictly isolated from downstream business side-effects, leaving each observer class to manage its own discrete domain task.


## 1. Authentication & Token Deep-Dive

### Key Authentication Concepts

| Concept | Primary Purpose | Key Advantage |
| --- | --- | --- |
| **JWT (JSON Web Token)** | Self-contained, digitally signed claim container. | **Stateless validation:** Microservices verify signatures locally using a public key without database lookups. |
| **Access Token** | Short-lived credential (e.g., 15 mins) authorizing API requests. | **Limits exposure window:** If stolen, damage is capped by the short Time-To-Live (TTL). |
| **Refresh Token** | Long-lived credential (e.g., 7–30 days) used solely to obtain new access tokens. | **Maintains active sessions:** Keeps users logged in securely without extending access token lifetime. |
| **Refresh Token Rotation** | One-time-use refresh token exchange mechanism. | **Theft detection:** If a rotated refresh token is reused, the entire token family is revoked immediately. |
| **HttpOnly Cookies** | Storage mechanism for sensitive tokens on client side. | **XSS protection:** JavaScript running in the browser cannot read or extract `HttpOnly` cookies. |
| **Redis Auth Layer** | RAM key-value cache for session state and revocations. | **Sub-millisecond lookups:** Handles token blacklists, family tracking, and rate limiting effortlessly. |

---

### Key Comparisons & Edge Cases

#### Why JWT vs. Why Not Traditional Stateful Sessions?

* **Stateful Sessions:** Server stores session ID in memory/DB (e.g., Redis) and validates it against a session store on *every request*.
* *Pros:* Instant revocation at any time.
* *Cons:* Requires central DB lookups, creating a bottleneck that complicates multi-region horizontal scaling.


* **JWT (Stateless):** Client holds signed claims. Server checks signature using a secret/public key.
* *Pros:* Scales horizontally across services with zero database reads required for authentication.
* *Cons:* Cannot be natively revoked before expiration without maintaining a token revocation list/blacklist.



#### What if Redis Crashes?

* **Stateless JWT Verification:** Unaffected. Core API request authentication continues working because JWT signature validation relies on memory/public keys, not Redis.
* **Stateful Operations (Logout/Refresh):** Refresh token exchanges, rate-limiting, and explicit revocations will temporarily fail unless mitigated.
* **Resilience Patterns:**
* **Redis Sentinel / Redis Cluster:** Auto-failover to replica nodes in milliseconds.
* **Fallback Store:** Fall back to primary database (PostgreSQL) for refresh token lookups during cache downtime.



#### How Google & Microsoft Handle Login (SSO / OAuth 2.0 / OIDC)

Both tech giants rely on **OpenID Connect (OIDC)** over **OAuth 2.0** with **PKCE (Proof Key for Code Exchange)**:

1. **Federated Identity Provider (IdP):** Central identity servers manage single sign-on (SSO) globally via domain-scoped session cookies.
2. **Authorization Code Flow with PKCE:**
* Client app redirects user to Google/Microsoft accounts page.
* User completes Auth + MFA/Passkey challenge.
* Google/Microsoft redirects back with an ephemeral **Authorization Code**.
* Client backend exchanges this authorization code server-to-server for an **ID Token (JWT)**, **Access Token**, and **Refresh Token**.



---

## 2. System Architecture

```
                                  +---------------------------------------+
                                  |            Monorepo (Turborepo)       |
                                  +---------------------------------------+
                                                     |
                          +--------------------------+--------------------------+
                          |                                                     |
              +-----------------------+                             +-----------------------+
              |   Frontend (Next.js)  |                             |   Backend (NestJS)    |
              +-----------------------+                             +-----------------------+
              |  Server Components    |                             |  Guards & DTOs        |
              |  Client Components    | -- HTTP / REST API (JSON) --> |  Service Pattern      |
              |  TanStack Query       |                             |  Repository Pattern   |
              |  Zod & Hook Form      |                             |  Prisma ORM           |
              +-----------------------+                             +-----------------------+
                                                                                |
                                                                    +-----------+-----------+
                                                                    |                       |
                                                           +-----------------+     +-----------------+
                                                           |   PostgreSQL    |     |   Redis Cache   |
                                                           | (Primary DB)    |     | (Session/Revoke)|
                                                           +-----------------+     +-----------------+

```

### Stack Selection Rationale

* **NestJS instead of Express:** Express is unopinionated and prone to inconsistent application structures across teams. NestJS provides an enterprise-ready architecture (TypeScript-first, Dependency Injection, Modules, Controllers, Guards) modeled on proven design patterns out of the box.
* **Next.js instead of React SPA:** SPAs push routing, rendering, and data fetching entirely to the browser, leading to large client JS bundles and weak SEO. Next.js delivers Hybrid Rendering (RSC, SSR, SSG, ISR) to load pages faster with reduced client bundle size.
* **Prisma:** Full type-safety generated automatically from the database schema. Prevents runtime SQL syntax errors and maps query results directly to auto-generated TypeScript types.
* **PostgreSQL:** ACID-compliant, battle-tested relational database with native support for JSONB (semi-structured data), advanced indexing, and connection pooling support.
* **Docker:** Guarantees environment parity ("works on my machine" = "works in production") by packaging runtimes, OS dependencies, and code into reproducible containers.
* **Monorepo (Turborepo / Nx):** Allows sharing TypeScript types, validation schemas, and UI components between frontend and backend in a single repository with caching for builds.

### Design Patterns

* **Repository Pattern:** Abstracts raw database queries behind explicit data-access interfaces (`UserRepository.findById()`). Decouples domain logic from ORM implementation, simplifying unit testing with mock repositories.
* **Dependency Injection (DI):** Inverts control (IoC). Dependencies are injected into constructors rather than instantiated manually inside classes (`new Service()`), producing loosely coupled and testable components.

---

## 3. Frontend Architecture

### Server vs. Client Components

* **React Server Components (RSC):**
* *Why:* Execute exclusively on the server. They fetch data close to the database, emit zero JavaScript to the client bundle, and protect sensitive API keys.
* *Use cases:* Data fetching, layout rendering, static content.


* **Client Components (`'use client'`):**
* *Why:* Hydrate in the browser to enable user interactivity and client-side browser APIs.
* *Use cases:* Event listeners (`onClick`), React hooks (`useState`, `useEffect`), form controls, browser state.



### State & Validation Management

* **TanStack Query (React Query):** Manages asynchronous server state. Handles background refetching, request deduplication, caching, stale-while-revalidate policies, and optimistic updates out of the box.
* **React Hook Form:** Uncontrolled form state manager driven by native refs. Prevents re-rendering the entire component tree on every keystroke, significantly outperforming controlled state forms.
* **Zod:** TypeScript-first schema validation engine. Infers runtime type declarations directly from schemas (`type User = z.infer<typeof Schema>`). Shared seamlessly across frontend forms and backend API pipelines.

---

## 4. Backend Lifecycle & NestJS Concepts

```
      Incoming Request
             │
             ▼
        [ Guards ]             <-- Authentication & RBAC permissions (CanActivate)
             │
             ▼
      [ Interceptors ]         <-- Pre-controller transformation & logging
             │
             ▼
     [ ValidationPipe ]        <-- Enforces schema validation using DTOs
             │
             ▼
       [ Controller ]          <-- Endpoints mapping & HTTP status codes
             │
             ▼
        [ Service ]            <-- Domain business logic orchestration
             │
             ▼
       [ Repository ]         <-- Database abstractions (Prisma / SQL)
             │
             ▼
      [ Interceptors ]         <-- Post-controller response format mapping
             │
             ▼
      Outgoing Response

```

* **Guards:** Determine whether a given request should be handled by the route handler based on runtime conditions (e.g., JWT validation, roles check).
* **Interceptors:** Intercept request/response execution streams to extend capabilities (e.g., transform response structures, log metrics, cache responses).
* **Decorators:** Annotations (`@Injectable()`, `@UseGuards()`, `@CurrentUser()`) adding declarative metadata to classes, methods, or parameters.
* **Validation & DTOs:** Data Transfer Objects define payload runtime contracts. NestJS `ValidationPipe` maps incoming request payloads against DTO rules (`class-validator`), discarding unknown fields and rejecting invalid inputs before they reach business logic.
* **Service Pattern:** Encapsulates business logic, orchestrations, and rules separate from HTTP request transport layers.

---

## 5. Performance Optimization

* **Pagination:**
* *Offset-Based (`LIMIT / OFFSET`):* Simple to implement; degrades on deep page offsets ($O(N)$ scan).
* *Cursor-Based (`WHERE id > last_seen_id`):* Scale-invariant ($O(1)$ lookup speed); prevents missing or duplicate records when database rows shift dynamically.


* **Lazy Loading:** Defers downloading non-critical code or resources until needed via dynamic code splitting (`import()`) or image lazy-loading.
* **Compression:** Shrinks HTTP payload byte size over the wire using modern algorithms (Brotli / Gzip) at the gateway or proxy tier.
* **Redis Cache:** Keeps frequently requested, read-heavy data (e.g., public profiles, configuration tables) in memory to reduce database CPU pressure and lower response times to under 5ms.
* **Query Optimization:** Eliminates N+1 query patterns using explicit join fetches, selective projections (`SELECT id, name` instead of `SELECT *`), and indexing foreign keys and filter columns.
* **Connection Pooling:** Uses proxies like PgBouncer or built-in ORM pools to reuse a limited number of active PostgreSQL connections, avoiding connection overhead under high traffic.

---

## 6. Security Architecture

```
+------------------+-------------------------------------------------------------------------+
| Threat           | Mitigation Strategy                                                     |
+------------------+-------------------------------------------------------------------------+
| OWASP Top 10     | Reference guidelines for addressing major web application risks.         |
| Helmet           | Sets security headers (CSP, HSTS, X-Frame-Options) to shield browsers. |
| CORS             | Restricts cross-origin HTTP calls to explicit, trusted domain origins.  |
| CSRF             | SameSite=Strict/Lax HttpOnly cookies, combined with anti-CSRF headers.  |
| XSS              | Content Security Policy (CSP) + auto-escaping UI frameworks (React).    |
| SQL Injection    | Prepared statements and parameterized inputs (handled by Prisma ORM).   |
| Password Hashing | Slow adaptive algorithms (Argon2id or bcrypt) with unique random salts. |
+------------------+-------------------------------------------------------------------------+

```