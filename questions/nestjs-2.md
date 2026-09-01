### Architectural Cheat-Sheet for Senior Interviews

When asked high-level architectural questions about NestJS, draw on these key concepts:

1. **Dependency Injection (IoC Container)**: NestJS manages object lifecycles. Rather than manually calling `new Service()`, classes declare dependencies in constructors, and NestJS injects resolved singletons.
2. **Provider Scopes**:
  * **DEFAULT (Singleton)**: One single instance shared across the entire application runtime. Highly performant and memory-efficient.
  * **REQUEST**: A new instance is created for *every single incoming HTTP request* and garbage collected afterward. (Use sparingly due to performance cost).
  * **TRANSIENT**: A unique instance is created for every provider that injects it.
3. **Request Lifecycle Flow**:

$$\text{Incoming Request} \longrightarrow \text{Middleware} \longrightarrow \text{Guards} \longrightarrow \text{Interceptors (Pre)} \longrightarrow \text{Pipes} \longrightarrow \text{Controller Action} \longrightarrow \text{Service} \longrightarrow \text{Interceptors (Post)} \longrightarrow \text{Exception Filters} \longrightarrow \text{Response}$$

## 1. What is Tree-Shaking in NestJS?

### Definition & Explanation

Tree-shaking is a dead-code elimination technique executed by modern JavaScript bundlers (Webpack, SWC, Rollup, esbuild) to strip out unused modules from the final production bundle.

In NestJS, traditional static tree-shaking faces challenges because NestJS relies heavily on dynamic module registrations (`@Module({ controllers, providers })`), string/symbol-based dependency injection tokens, and runtime metadata reflection (`reflect-metadata`). Because bundlers cannot statically analyze whether a service decorated with `@Injectable()` is dynamically resolved at runtime, unused providers and modules remain in the bundle unless direct static imports and tree-shakeable modular patterns are strictly implemented.

### Workflow & Architecture

```
[ NestJS Source Modules ]
       │
       ├── Dynamic Registration (@Module) ───► Static Analyzer Bypass ───► Retained in Bundle (No Shaking)
       │
       └── Direct Pure Exports / SWC ────────► Static Graph Analysis ────► Dead Code Removed (Tree-Shaken)

```

### Example

**Legacy / Standard Pattern (Not Tree-Shakeable due to Barrel Export & Dynamic DI):**

```typescript
// user.barrel.ts (Barrel files cause bundlers to retain all exports)
export * from './user.service';
export * from './unused-analytics.service'; // Retained in bundle even if never called

// app.module.ts
import { Module } from '@nestjs/common';
import { UserService, UnusedAnalyticsService } from './user.barrel';

@Module({
  providers: [UserService, UnusedAnalyticsService], // Dynamic registration prevents dead-code stripping
})
export class AppModule {}

```

**Modern Pattern (Tree-Shakeable Bundling via Direct Imports & SWC/Webpack configuration):**

```typescript
// user.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class UserService {
  getUser(): string {
    return 'User Active';
  }
}

// main.ts (Direct static import allows tree-shakers to isolate and eliminate unreferenced dependencies)
import { UserService } from './user.service';

const userService = new UserService();
console.log(userService.getUser());

```

### Output

```text
[SWC/Webpack Builder] Dead-code elimination completed.
Removed unused exports: UnusedAnalyticsService (Saved 38 KB)
Final Bundle Size: 182 KB
User Active

```

---

## 2. What Does TypeScript Transpile To?

### Definition & Explanation

TypeScript does not compile directly into low-level machine byte-code. It **transpiles** (source-to-source compiles) `.ts` files into standard JavaScript (`.js`).

During this compilation step, the TypeScript compiler (`tsc`) performs two main operations:

1. **Type Erasure:** Completely strips out type annotations, interfaces, generics, and type aliases.
2. **Syntax Downleveling:** Converts modern or TS-specific features (enums, namespaces, decorators, async/await, optional chaining) into the ECMAScript target version defined in `tsconfig.json` (such as ES5, ES6, or ES2022).

### Workflow & Architecture

```
[ .ts Source Code ] ──► [ AST Parser ] ──► [ Type Checker ] ──► [ Emitter ] ──► [ .js Output File ]
                                                                       │ (Type Erasure & Downleveling)

```

### Example

**TypeScript Source Code:**

```typescript
enum UserRole {
  ADMIN = 'ADMIN',
  USER = 'USER',
}

class UserAccount {
  constructor(private username: string, public role: UserRole = UserRole.USER) {}

  public getProfile(): string {
    return `${this.username} (${this.role})`;
  }
}

const account = new UserAccount('alex', UserRole.ADMIN);
console.log(account.getProfile());

```

**Transpiled Legacy JavaScript Output (Target: ES5):**

```javascript
var UserRole;
(function (UserRole) {
    UserRole["ADMIN"] = "ADMIN";
    UserRole["USER"] = "USER";
})(UserRole || (UserRole = {}));
var UserAccount = /** @class */ (function () {
    function UserAccount(username, role) {
        if (role === void 0) { role = UserRole.USER; }
        this.username = username;
        this.role = role;
    }
    UserAccount.prototype.getProfile = function () {
        return this.username + " (" + this.role + ")";
    };
    return UserAccount;
}());
var account = new UserAccount('alex', UserRole.ADMIN);
console.log(account.getProfile());

```

**Transpiled Modern JavaScript Output (Target: ES2022):**

```javascript
var UserRole;
(function (UserRole) {
    UserRole["ADMIN"] = "ADMIN";
    UserRole["USER"] = "USER";
})(UserRole || (UserRole = {}));
class UserAccount {
    #username;
    constructor(username, role = UserRole.USER) {
        this.#username = username;
        this.role = role;
    }
    getProfile() {
        return `${this.#username} (${this.role})`;
    }
}
const account = new UserAccount('alex', UserRole.ADMIN);
console.log(account.getProfile());

```

### Output

```text
alex (ADMIN)

```

---

## 3. What is TypeORM's Metadata Reader?

### Definition & Explanation

TypeORM’s Metadata Reader (`EntityMetadataBuilder` working alongside `MetadataArgsStorage`) is an internal reflection subsystem responsible for inspecting and collecting entity configurations.

When your application starts, the Metadata Reader processes entity classes decorated with `@Entity()`, `@Column()`, `@ManyToOne()`, or explicit `EntitySchema` definitions. It converts decorator arguments and `reflect-metadata` type hints into structured `EntityMetadata` objects. These metadata objects dictate how TypeORM generates database migrations, maps SQL query result sets to JS objects, and builds `QueryBuilder` operations.

### Workflow & Architecture

```
[ @Entity / Decorators ] ──► [ reflect-metadata ] ──► [ MetadataArgsStorage ]
                                                              │
                                                              ▼
                                                   [ EntityMetadataBuilder ]
                                                              │
                                                              ▼
                                               [ SQL Query & Schema Engine ]

```

### Example

**Legacy Decorator-Based Metadata Reader Setup:**

```typescript
import 'reflect-metadata';
import { Entity, PrimaryGeneratedColumn, Column, getMetadataArgsStorage } from 'typeorm';

@Entity('products')
export class ProductEntity {
  @PrimaryGeneratedColumn()
  id!: number;

  @Column({ type: 'varchar', length: 150 })
  title!: string;
}

// Inspecting TypeORM's internal metadata storage directly
const tableMetadata = getMetadataArgsStorage().tables.filter(t => t.target === ProductEntity);
const columnMetadata = getMetadataArgsStorage().columns.filter(c => c.target === ProductEntity);

console.log({
  tableName: tableMetadata[0].name,
  columns: columnMetadata.map(col => ({ property: col.propertyName, type: col.options.type })),
});

```

**Modern Schema-Based Metadata Definition (Without Decorators/Reflect):**

```typescript
import { EntitySchema } from 'typeorm';

export interface Product {
  id: number;
  title: string;
}

export const ProductSchema = new EntitySchema<Product>({
  name: 'Product',
  tableName: 'products',
  columns: {
    id: { type: Number, primary: true, generated: true },
    title: { type: String, length: 150 },
  },
});

```

### Output

```text
{
  tableName: 'products',
  columns: [
    { property: 'id', type: undefined },
    { property: 'title', type: 'varchar' }
  ]
}

```

---

## 4. What is a Class Decorator?

### Definition & Explanation

A Class Decorator is a top-level decorator applied directly to a class declaration. It allows inspecting, mutating, wrapping, or replacing a class constructor definition at design time or runtime.

In NestJS and TypeORM, class decorators like `@Injectable()`, `@Controller()`, and `@Entity()` intercept class declarations to register metadata with the framework's internal dependency injection or ORM registries.

### Example

**Legacy Class Decorator (`experimentalDecorators: true`):**

```typescript
// Legacy Class Decorator Signature: (target: Function) => void | any
function FreezeClass(target: Function) {
  Object.freeze(target);
  Object.freeze(target.prototype);
}

@FreezeClass
class AuthServiceLegacy {
  public status = 'Active';
}

console.log('Legacy Class Frozen:', Object.isFrozen(AuthServiceLegacy));

```

**Modern Stage 3 Standard Class Decorator (TypeScript 5.0+ / ES Standard):**

```typescript
// Modern Stage 3 Signature: (value: Function, context: ClassDecoratorContext) => Function | void
function SealedClass<T extends abstract new (...args: any[]) => any>(
  value: T,
  context: ClassDecoratorContext
) {
  console.log(`Registering class: ${String(context.name)}`);
  return value;
}

@SealedClass
class AuthServiceModern {
  public status = 'Active';
}

const auth = new AuthServiceModern();
console.log('Modern Instance Status:', auth.status);

```

### Output

```text
Legacy Class Frozen: true
Registering class: AuthServiceModern
Modern Instance Status: Active

```

---

## 5. What is a Property Decorator?

### Definition & Explanation

A Property Decorator is applied to an instance or static property of a class.

In legacy TypeScript (`experimentalDecorators`), a property decorator receives two arguments: the class prototype (or constructor for static members) and the property key name. Property decorators cannot directly modify initial values at declaration, but they are used to record metadata for libraries like `class-validator` (`@IsString()`), NestJS (`@Inject()`), and TypeORM (`@Column()`).

### Example

**Legacy Property Decorator (`experimentalDecorators: true`):**

```typescript
import 'reflect-metadata';

function LogProperty(target: any, propertyKey: string) {
  const metaKey = `custom:meta:${propertyKey}`;
  Reflect.defineMetadata(metaKey, `Property ${propertyKey} registered`, target);
}

class UserDTO {
  @LogProperty
  email!: string;
}

const dto = new UserDTO();
console.log(Reflect.getMetadata('custom:meta:email', dto));

```

**Modern Stage 3 Property Decorator (TypeScript 5.0+):**

```typescript
// Modern Stage 3 Field Decorator Signature: (value: undefined, context: ClassFieldDecoratorContext)
function TransformUpper(value: undefined, context: ClassFieldDecoratorContext) {
  return function (this: any, initialValue: any) {
    return typeof initialValue === 'string' ? initialValue.toUpperCase() : initialValue;
  };
}

class UserProfile {
  @TransformUpper
  role: string = 'administrator';
}

const profile = new UserProfile();
console.log('Transformed Role:', profile.role);

```

### Output

```text
Property email registered
Transformed Role: ADMINISTRATOR

```

---

## 6. What is `Reflect.metadata()`?

### Definition & Explanation

`Reflect.metadata()` is a decorator factory function provided by the `reflect-metadata` polyfill library (part of an ECMAScript metadata proposal).

When `experimentalDecorators` and `emitDecoratorMetadata` are enabled in `tsconfig.json`, the TypeScript compiler automatically emits design-time type metadata (`design:type`, `design:paramtypes`, and `design:returntype`) using `Reflect.metadata()`. This allows frameworks like NestJS to automatically determine dependency types for constructor injection without manual string tokens.

### Example

```typescript
import 'reflect-metadata';

// Custom Metadata Decorator
function Roles(...roles: string[]) {
  return Reflect.metadata('roles', roles);
}

class OrderController {
  @Roles('admin', 'manager')
  checkout() {
    return 'Order Processed';
  }
}

// Explicit metadata tagging and retrieval
const controller = new OrderController();
const allowedRoles = Reflect.getMetadata('roles', controller, 'checkout');

console.log('Allowed Roles:', allowedRoles);

```

### Output

```text
Allowed Roles: [ 'admin', 'manager' ]

```

---

## 7. What is the Non-Null Assertion Operator?

### Definition & Explanation

The Non-Null Assertion Operator is a postfix exclamation mark (`!`) appended to an expression in TypeScript.

It explicitly signals to the TypeScript compiler that an operand is neither `null` nor `undefined`, suppressing strict null checking errors (`strictNullChecks`). It is purely a **compile-time type directive** and is completely removed during transpilation into JavaScript, offering no runtime safety checks or fallbacks.

### Example

**Legacy / Standard Non-Null Assertion:**

```typescript
function getLength(value: string | null): number {
  // Tells TS compiler to bypass null check
  return value!.length; 
}

console.log('Valid string length:', getLength('NestJS'));

```

**Modern Safe Alternatives (Optional Chaining & Nullish Coalescing):**

```typescript
function getLengthSafe(value: string | null): number {
  // Modern safe evaluation without forcing non-null assertion
  return value?.length ?? 0;
}

console.log('Safe null handling:', getLengthSafe(null));

```

### Output

```text
Valid string length: 6
Safe null handling: 0

```

---

## Senior Interview Cheatsheet

* **NestJS Tree-Shaking:** Tree-shaking requires static ESM imports. NestJS dynamic module registration (`@Module({ providers })`) and `reflect-metadata` force bundlers to preserve modules unless modular barrel-free imports and modern compiler options (SWC/esbuild) are enforced.
* **TypeScript Transpilation:** `tsc` only performs type erasure and syntax downleveling to JavaScript targets (e.g., ES5 vs. ES2022). It generates no runtime type checks or machine code.
* **TypeORM Metadata Reader:** Driven by `EntityMetadataBuilder` and `MetadataArgsStorage`. It aggregates decorator configurations and `reflect-metadata` types into runtime schema maps for query generation and migrations.
* **Class Decorators:** In legacy TS, they take `(target: Function)` and mutate constructor prototypes. In modern Stage 3 JS, they take `(value, context)` and return a new class constructor safely.
* **Property Decorators:** Legacy property decorators take `(target, propertyKey)` to record metadata. Modern Stage 3 property decorators accept `(value, context)` and can return an initializer function to transform properties at runtime.
* **Reflect.metadata():** Requires `emitDecoratorMetadata: true`. Automatically records `design:type`, `design:paramtypes`, and `design:returntype` at compile-time for runtime Dependency Injection in NestJS and Angular.
* **Non-Null Assertion (`!`):** Pure compile-time override to bypass `strictNullChecks`. Erased completely during JS transpilation. Prefer modern `?.` (optional chaining) and `??` (nullish coalescing) for runtime safety.

## 1. What is a Column Decorator?

### Definition & Explanation

A Column Decorator in TypeORM (`@Column()`) maps an entity class property to a table column in the relational database. It registers column specifications such as data type, length, nullability, unique constraints, and default values into TypeORM's internal metadata engine (`MetadataArgsStorage`).

### Workflow & Architecture

```
[ Class Property ] ──► [ @Column(options) ] ──► [ TypeORM Metadata Registry ] ──► [ SQL DDL / Table Schema ]

```

### Example

**Legacy Pattern (Implicit / Default Data Types):**

```typescript
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity('users_legacy')
export class UserLegacy {
  @PrimaryGeneratedColumn()
  id!: number;

  // Type inferred from JS metadata; defaults to default varchar/int without constraints
  @Column()
  name!: string;
}

```

**Modern Pattern (Explicit Database Specifications & Enums):**

```typescript
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

export enum UserRole {
  ADMIN = 'admin',
  USER = 'user',
}

@Entity('users')
export class UserEntity {
  @PrimaryGeneratedColumn('uuid')
  id!: string;

  @Column({ type: 'varchar', length: 100, nullable: false, unique: true })
  email!: string;

  @Column({ type: 'enum', enum: UserRole, default: UserRole.USER })
  role!: UserRole;

  @Column({ type: 'timestamp', default: () => 'CURRENT_TIMESTAMP' })
  createdAt!: Date;
}

```

### Output

```text
[TypeORM Database Schema Engine] SQL Execution:
CREATE TABLE "users" (
  "id" uuid NOT NULL DEFAULT uuid_generate_v4(), 
  "email" varchar(100) NOT NULL UNIQUE, 
  "role" "users_role_enum" NOT NULL DEFAULT 'user', 
  "createdAt" TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP, 
  CONSTRAINT "PK_users_id" PRIMARY KEY ("id")
);

```

---

## 2. Why Do We Use Classes Instead of TypeScript Interfaces for DTOs in NestJS?

### Definition & Explanation

TypeScript interfaces exist purely at compile-time and undergo **Type Erasure** when transpiled into JavaScript, leaving zero footprint in the runtime bundle.

NestJS relies on JavaScript runtime reflection to validate incoming network payloads, generate OpenAPI/Swagger specifications, and perform automatic type transformations. Because ECMAScript ES6 Classes preserve constructor definitions and structural metadata at runtime, NestJS features like `ValidationPipe` (`class-validator` / `class-transformer`) require **Classes** for DTO (Data Transfer Object) definitions.

### Workflow & Architecture

```
[ TypeScript DTO Definition ]
       │
       ├── Interface ──► TypeScript Transpilation ──► Stripped Completely (0 Runtime Data)
       │
       └── Class     ──► TypeScript Transpilation ──► Retained JS Constructor & Metadata (Validation Works)

```

### Example

**Interface DTO (Fails at Runtime Validation):**

```typescript
// Transpiles to empty object at runtime. Reflection cannot access metadata.
export interface CreateUserInterfaceDto {
  name: string;
  email: string;
}

```

**Modern Class DTO (Fully Functional at Runtime):**

```typescript
import { IsString, IsEmail, IsNotEmpty } from 'class-validator';

export class CreateUserClassDto {
  @IsString()
  @IsNotEmpty()
  name!: string;

  @IsEmail()
  email!: string;
}

```

### Output

```text
[Interface DTO Runtime Inspection]: Object.keys() => [] (Metadata lost, ValidationPipe bypassed)
[Class DTO Runtime Inspection]: Instance of CreateUserClassDto, Metadata keys: ['name', 'email'] (Validation operational)

```

---

## 3. What Are `class-validator` and `class-transformer`?

### Definition & Explanation

`class-validator` and `class-transformer` are companion libraries used by NestJS to handle payload validation and object conversion:

* **`class-transformer`**: Converts plain JavaScript JSON payloads received from HTTP requests into instances of typed class DTOs using `plainToInstance()`.
* **`class-validator`**: Inspects decorator annotations on class instance properties and executes validation rules, throwing structured error arrays if rules are violated.

### Workflow & Architecture

```
[ Incoming HTTP JSON Body ]
           │
           ▼
 [ class-transformer ]  ──► (Transforms plain JSON to DTO Class Instance)
           │
           ▼
  [ class-validator ]   ──► (Evaluates Decorator Rules on DTO Instance)
           │
           ├── Validated ────────► Passes to Route Handler
           └── Violations Found  ──► Throws BadRequestException (400)

```

### Example

```typescript
import { IsString, IsInt, Min } from 'class-validator';
import { plainToInstance } from 'class-transformer';
import { validate } from 'class-validator';

class CreateProductDto {
  @IsString()
  title!: string;

  @IsInt()
  @Min(1)
  price!: number;
}

async function validatePayload(rawJson: Record<string, any>) {
  // Step 1: Plain payload to Class Instance
  const dtoInstance = plainToInstance(CreateProductDto, rawJson);

  // Step 2: Run validation on the instance
  const errors = await validate(dtoInstance);

  if (errors.length > 0) {
    return { status: 'failed', errors: errors.map(e => Object.values(e.constraints!)) };
  }
  return { status: 'success', data: dtoInstance };
}

// Execution test with invalid payload
validatePayload({ title: 'Laptop', price: -50 }).then(console.log);

```

### Output

```text
{
  status: 'failed',
  errors: [ [ 'price must not be less than 1' ] ]
}

```

---

## 4. What is NestJS's Inversion of Control (IoC)?

### Definition & Explanation

Inversion of Control (IoC) is an architectural principle where control over object creation, lifecycle management, and dependency binding is inverted from the consuming application code to a dedicated framework container.

In NestJS, providers decorated with `@Injectable()` are registered with the central NestJS IoC container. When a class requests a dependency via constructor parameters, the IoC container automatically resolves, instantiates, caches (Singleton scope by default), and injects the dependency.

### Workflow & Architecture

```
[ App Bootstrap ] ──► [ IoC Container Scans Providers ] ──► [ Resolves Dependency Tree ] ──► [ Injects Dependencies into Controller ]

```

### Example

**Legacy Approach (Manual Tightly-Coupled Instantiation):**

```typescript
class DatabaseService {
  query() { return 'Data retrieved'; }
}

class LegacyController {
  private dbService: DatabaseService;
  constructor() {
    // Controller manually controls dependency creation
    this.dbService = new DatabaseService(); 
  }
}

```

**Modern NestJS IoC Approach:**

```typescript
import { Injectable, Controller, Get } from '@nestjs/common';

@Injectable()
export class DatabaseService {
  query(): string {
    return 'Data retrieved via IoC';
  }
}

@Controller('users')
export class UserController {
  // IoC container resolves and injects instance automatically
  constructor(private readonly dbService: DatabaseService) {}

  @Get()
  getUsers(): string {
    return this.dbService.query();
  }
}

```

### Output

```text
[Nest] Logs: Nest push IoC container resolved [DatabaseService] -> Injected into [UserController]
HTTP GET /users -> 200 OK: "Data retrieved via IoC"

```

---

## 5. What is a Custom Token Decorator in NestJS?

### Definition & Explanation

A Custom Token (or Parameter) Decorator in NestJS is built using the `createParamDecorator()` factory. It allows developers to extract specific property paths, security claims, or transformed data directly out of the `ExecutionContext` (HTTP request object) and inject them cleanly into route handler parameters.

### Workflow & Architecture

```
[ Incoming Request ] ──► [ ExecutionContext ] ──► [ createParamDecorator Evaluates ] ──► [ Extracted Value Passed to Route Parameter ]

```

### Example

```typescript
import { createParamDecorator, ExecutionContext, Controller, Get } from '@nestjs/common';

// Custom Parameter Decorator Definition
export const User = createParamDecorator(
  (data: string | undefined, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user || { id: 101, username: 'alex', role: 'admin' };

    return data ? user?.[data] : user;
  },
);

// Route Handler Implementation
@Controller('profile')
export class ProfileController {
  @Get()
  getProfile(@User('username') username: string, @User() fullUser: any) {
    return { username, fullUser };
  }
}

```

### Output

```text
{
  "username": "alex",
  "fullUser": { "id": 101, "username": "alex", "role": "admin" }
}

```

---

## 6. What is the Repository Pattern in NestJS / TypeORM?

### Definition & Explanation

The Repository Pattern decouples business logic from data access logic by inserting an abstraction layer between domain services and database operations.

In TypeORM with NestJS, `Repository<Entity>` provides generic data access methods (`find`, `findOne`, `save`, `delete`). Repositories are injected into NestJS services using the `@InjectRepository(Entity)` decorator, enabling isolation for mocking during unit testing.

### Workflow & Architecture

```
[ Controller Layer ] ──► [ Service Layer (Business Logic) ] ──► [ Repository Layer (ORM) ] ──► [ SQL Database ]

```

### Example

```typescript
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { UserEntity } from './user.entity';

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(UserEntity)
    private readonly userRepository: Repository<UserEntity>,
  ) {}

  async findByEmail(email: string): Promise<UserEntity | null> {
    return await this.userRepository.findOne({ where: { email } });
  }

  async createUser(email: string): Promise<UserEntity> {
    const user = this.userRepository.create({ email });
    return await this.userRepository.save(user);
  }
}

```

### Output

```text
[TypeORM Query]: SELECT * FROM "users" WHERE "email" = 'alex@example.com' LIMIT 1
Result: UserEntity { id: 'a4b1-...', email: 'alex@example.com', role: 'user' }

```

---

## 7. What Major Design Patterns Must a NestJS Developer Know?

### Definition & Explanation

NestJS is built on proven enterprise architectural and design patterns:

1. **Dependency Injection (DI) & IoC**: Container-managed dependency resolution.
2. **Singleton Pattern**: Providers are singletons by default across the container scope.
3. **Repository Pattern**: Data layer abstraction separating storage access from business logic.
4. **Factory Pattern**: Dynamic provider generation via `useFactory`.
5. **Decorator Pattern**: Metadata annotation of classes, properties, and parameters.
6. **Chain of Responsibility / Middleware Pipeline**: Request processing via Guards, Interceptors, Pipes, and Exception Filters.
7. **Module Pattern**: Domain encapsulation via cohesive module declarations (`@Module()`).

### Workflow & Architecture

```
[ Incoming Request ]
        │
        ▼
   [ Middleware ]
        │
        ▼
     [ Guard ]  ──► (Auth Authorization Check)
        │
        ▼
  [ Interceptor ] (Pre-controller logic)
        │
        ▼
     [ Pipe ]   ──► (Validation & Payload Conversion)
        │
        ▼
  [ Controller ] ──► [ Service (Singleton) ] ──► [ Repository Layer ]

```

### Example (Factory Provider Pattern):

```typescript
import { Module } from '@nestjs/common';

export class ConnectionConfig {
  constructor(public connectionUrl: string) {}
}

@Module({
  providers: [
    {
      provide: 'DATABASE_CONFIG',
      useFactory: (): ConnectionConfig => {
        const env = process.env.NODE_ENV || 'development';
        return new ConnectionConfig(env === 'production' ? 'postgres://prod' : 'postgres://dev');
      },
    },
  ],
})
export class AppModule {}

```

### Output

```text
[NestJS Provider Engine] Factory 'DATABASE_CONFIG' resolved instance: ConnectionConfig { connectionUrl: 'postgres://dev' }

```

---

## 8. What are NestJS HTTP Exceptions?

### Definition & Explanation

NestJS provides a built-in Exception Layer to handle application runtime errors gracefully. The framework includes a standard `HttpException` base class and several specialized exception wrappers (e.g., `BadRequestException`, `NotFoundException`, `UnauthorizedException`). When thrown from anywhere within the application lifecycle, the global exception filter catches the error and serializes it into a standardized JSON response format.

### Example

**Custom & Standard Exception Implementation:**

```typescript
import { Controller, Get, Param, HttpException, HttpStatus, NotFoundException } from '@nestjs/common';

@Controller('items')
export class ItemController {
  @Get(':id')
  getItem(@Param('id') id: string) {
    if (id === '0') {
      // Specialized Exception Wrapper
      throw new NotFoundException(`Item with ID ${id} does not exist`);
    }

    if (id === 'invalid') {
      // Base HttpException Usage
      throw new HttpException(
        { status: 'error', reason: 'Invalid ID Format' },
        HttpStatus.BAD_REQUEST,
      );
    }

    return { id, title: 'Item Found' };
  }
}

```

### Output

```json
// HTTP GET /items/0 -> Status 404 Not Found
{
  "statusCode": 404,
  "message": "Item with ID 0 does not exist",
  "error": "Not Found"
}

```

---

## 9. What is a Route Handler Decorator?

### Definition & Explanation

A Route Handler Decorator annotates class methods inside a NestJS controller to bind them to specific HTTP request methods and path routes. Standard route handler decorators include `@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`, `@Options()`, `@Head()`, and `@All()`.

### Example

```typescript
import { Controller, Get, Post, Put, Delete, Body, Param } from '@nestjs/common';

@Controller('orders')
export class OrderController {
  @Get()
  findAll() {
    return 'Fetch all orders';
  }

  @Post()
  create(@Body() payload: any) {
    return 'Order created';
  }

  @Put(':id')
  update(@Param('id') id: string) {
    return `Order ${id} updated`;
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return `Order ${id} deleted`;
  }
}

```

### Output

```text
[NestFactory] RoutesResolver mapped {/orders, GET} route
[NestFactory] RoutesResolver mapped {/orders, POST} route
[NestFactory] RoutesResolver mapped {/orders/:id, PUT} route
[NestFactory] RoutesResolver mapped {/orders/:id, DELETE} route

```

---

## 10. What is a Controller Parameter Decorator?

### Definition & Explanation

Controller Parameter Decorators extract contextual data from incoming HTTP requests and map them directly into arguments of route handler methods. They abstract manual access to raw platform request objects (`req`, `res`). Standard parameter decorators provided by NestJS include:

* `@Body(key?)`: Extracts `req.body` or a specific body field.
* `@Param(key?)`: Extracts `req.params` path parameters.
* `@Query(key?)`: Extracts `req.query` URL search parameters.
* `@Headers(key?)`: Extracts `req.headers` HTTP headers.
* `@Req()` / `@Request()`: Grants direct access to the underlying Request object.
* `@Res()` / `@Response()`: Grants direct access to the underlying Response object.

### Example

```typescript
import { Controller, Get, Query, Param, Headers } from '@nestjs/common';

@Controller('search')
export class SearchController {
  @Get(':category')
  search(
    @Param('category') category: string,
    @Query('q') query: string,
    @Headers('user-agent') userAgent: string,
  ) {
    return { category, query, userAgent };
  }
}

```

### Output

```text
// HTTP GET /search/books?q=nest&headers[user-agent]=Mozilla
{
  "category": "books",
  "query": "nest",
  "userAgent": "Mozilla"
}

```

---

## 11. What is Pipe Transformation, and How Many Built-in Pipe Types Exist in NestJS?

### Definition & Explanation

A Pipe in NestJS is a class annotated with `@Injectable()` implementing the `PipeTransform` interface. Pipes process input arguments right before a route handler executes, performing two functions:

1. **Transformation:** Converts input data from string/JSON types to targeted JavaScript types (e.g., converting route parameter string `'123'` to integer `123`).
2. **Validation:** Assesses input payload integrity and throws an `HttpException` if evaluation rules fail.

NestJS includes **9 Built-in Pipe Types**:

1. `ValidationPipe` (Enforces DTO validation rules)
2. `ParseIntPipe` (Parses string parameter to integer)
3. `ParseFloatPipe` (Parses string parameter to float)
4. `ParseBoolPipe` (Parses string parameter to boolean)
5. `ParseArrayPipe` (Parses delimited strings to typed arrays)
6. `ParseUUIDPipe` (Validates string matches UUID format)
7. `ParseEnumPipe` (Validates input against TypeScript Enum)
8. `DefaultValuePipe` (Provides default value if parameter is missing)
9. `ParseFilePipe` (Validates uploaded file size and mime types)

### Workflow & Architecture

```
[ Incoming Request Parameter ] ──► [ Pipe (transform) ] ──► [ Converted/Validated Parameter ] ──► [ Controller Handler ]

```

### Example

**Built-in & Custom Pipe Usage:**

```typescript
import { Controller, Get, Param, Query, ParseIntPipe, DefaultValuePipe, PipeTransform, Injectable, BadRequestException } from '@nestjs/common';

// Custom Pipe Definition
@Injectable()
export class UpperCasePipe implements PipeTransform<string, string> {
  transform(value: string): string {
    if (typeof value !== 'string') {
      throw new BadRequestException('Validation failed: Parameter must be a string');
    }
    return value.toUpperCase();
  }
}

@Controller('products')
export class ProductController {
  @Get(':id')
  getProduct(
    // Built-in Transformation Pipe
    @Param('id', ParseIntPipe) id: number,
    // Combination of Default Value Pipe & Custom Pipe
    @Query('code', new DefaultValuePipe('default'), new UpperCasePipe()) code: string,
  ) {
    return { id, idType: typeof id, code };
  }
}

```

### Output

```text
// HTTP GET /products/42?code=promo
{
  "id": 42,
  "idType": "number",
  "code": "PROMO"
}

// HTTP GET /products/invalid
HTTP 400 Bad Request: { "statusCode": 400, "message": "Validation failed (numeric string is expected)" }

```

---

## Senior Interview Cheatsheet

* **TypeORM `@Column()` Decorator:** Maps class attributes to database columns with specific types, constraints, and default values directly in the metadata registry.
* **Classes vs Interfaces for DTOs:** TypeScript interfaces suffer from type erasure at transpilation time. Classes persist as ES6 constructors at runtime, allowing `ValidationPipe` and OpenAPI tools to access runtime metadata.
* **`class-validator` & `class-transformer`:** `class-transformer` turns plain payload objects into class instances via `plainToInstance()`. `class-validator` runs validation metadata checks against these instances.
* **Inversion of Control (IoC):** NestJS IoC container controls object lifecycles, dependency instantiation, and injection based on constructor types and custom provider tokens.
* **Custom Parameter Decorators:** Created via `createParamDecorator()` to extract custom fields (e.g., user profiles, authorization tokens) directly from the `ExecutionContext`.
* **Repository Pattern:** Abstract data layer using TypeORM's `Repository<Entity>`. Injected via `@InjectRepository()` to separate business logic from SQL/DB queries and enable unit test mocking.
* **Core NestJS Design Patterns:** Includes Dependency Injection, Singleton providers, Repository pattern, Factory providers, Decorator composition, Chain of Responsibility (Pipes/Guards/Interceptors), and Domain Modules.
* **NestJS HTTP Exceptions:** NestJS catches unhandled exceptions through a global filter. Base `HttpException` or built-in wrappers (`NotFoundException`, `BadRequestException`) emit standardized JSON responses.
* **Route & Parameter Decorators:** Route handler decorators (`@Get()`, `@Post()`) define route routes and verbs; Parameter decorators (`@Body()`, `@Param()`, `@Query()`) extract typed request parameters.
* **Pipes:** 9 built-in pipes handle parameter transformation and validation. Custom pipes implement `PipeTransform` to validate or convert values before reaching route handler parameters.

## 1. What is an Example of the `ValidationPipe` in NestJS?

### Definition & Explanation

The `ValidationPipe` uses the `class-validator` and `class-transformer` packages to automatically enforce validation rules defined on DTO classes. It inspects incoming HTTP request bodies, path parameters, or query parameters, transforms plain JavaScript objects into typed class instances, and validates property constraints before passing data to the route handler.

### Workflow & Architecture

```
[ Incoming Request Body ] ──► [ ValidationPipe ] ──► [ class-transformer ] ──► [ class-validator ] ──► [ Route Handler ]
                                                                                   │ (If invalid)
                                                                                   ▼
                                                                        [ 400 Bad Request Exception ]

```

### Example

**Legacy Pattern (Manual Validation inside Route Handler):**

```typescript
import { Controller, Post, Body, BadRequestException } from '@nestjs/common';

@Controller('users')
export class UserLegacyController {
  @Post()
  create(@Body() body: any) {
    // Manual validation boilerplate
    if (!body.email || !body.email.includes('@')) {
      throw new BadRequestException('Invalid email address');
    }
    if (!body.age || typeof body.age !== 'number' || body.age < 18) {
      throw new BadRequestException('Age must be a number greater than or equal to 18');
    }
    return { status: 'User Created', data: body };
  }
}

```

**Modern Pattern (Declarative DTO with ValidationPipe):**

```typescript
import { Controller, Post, Body, UsePipes, ValidationPipe } from '@nestjs/common';
import { IsEmail, IsInt, Min } from 'class-validator';

export class CreateUserDto {
  @IsEmail()
  email!: string;

  @IsInt()
  @Min(18)
  age!: number;
}

@Controller('users')
export class UserController {
  @Post()
  @UsePipes(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true }))
  create(@Body() dto: CreateUserDto) {
    return { status: 'User Created', data: dto };
  }
}

```

### Output

```json
// Invalid HTTP POST payload: { "email": "invalid-email", "age": 15 }
// Status Code: 400 Bad Request
{
  "statusCode": 400,
  "message": [
    "email must be an email",
    "age must not be less than 18"
  ],
  "error": "Bad Request"
}

```

---

## 2. What is an Example of the `ParseIntPipe` in NestJS?

### Definition & Explanation

`ParseIntPipe` parses incoming request parameters (such as string route parameters or query parameters) into primitive JavaScript numbers (`number`). If the parameter cannot be converted into a valid integer, it immediately halts request processing and throws a `400 Bad Request` exception.

### Example

```typescript
import { Controller, Get, Param, ParseIntPipe } from '@nestjs/common';

@Controller('orders')
export class OrderController {
  @Get(':id')
  getOrder(@Param('id', ParseIntPipe) id: number) {
    return { orderId: id, type: typeof id };
  }
}

```

### Output

```json
// HTTP GET /orders/1024
{
  "orderId": 1024,
  "type": "number"
}

// HTTP GET /orders/abc
// Status Code: 400 Bad Request
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}

```

---

## 3. What is an Example of the `ParseFloatPipe` in NestJS?

### Definition & Explanation

`ParseFloatPipe` converts a string parameter into a floating-point number (`number`). It is typically used for query parameters representing financial amounts, percentages, geographical coordinates, or measurement metrics.

### Example

```typescript
import { Controller, Get, Query, ParseFloatPipe } from '@nestjs/common';

@Controller('metrics')
export class MetricsController {
  @Get('threshold')
  checkThreshold(@Query('val', ParseFloatPipe) val: number) {
    return { original: val, scaled: val * 100, isFloat: !Number.isInteger(val) };
  }
}

```

### Output

```json
// HTTP GET /metrics/threshold?val=45.67
{
  "original": 45.67,
  "scaled": 4567,
  "isFloat": true
}

```

---

## 4. What is an Example of the `ParseBoolPipe` in NestJS?

### Definition & Explanation

`ParseBoolPipe` parses string representations of boolean values (such as `'true'`, `'false'`, `'1'`, `'0'`) into primitive JavaScript boolean types (`true` or `false`). It prevents logical errors caused by truthy non-empty string evaluations in standard JavaScript.

### Example

```typescript
import { Controller, Get, Query, ParseBoolPipe } from '@nestjs/common';

@Controller('products')
export class ProductController {
  @Get()
  getProducts(@Query('activeOnly', ParseBoolPipe) activeOnly: boolean) {
    return { activeOnly, type: typeof activeOnly };
  }
}

```

### Output

```json
// HTTP GET /products?activeOnly=true
{
  "activeOnly": true,
  "type": "boolean"
}

```

---

## 5. What is an Example of the `ParseArrayPipe` in NestJS?

### Definition & Explanation

`ParseArrayPipe` parses comma-separated string parameters or raw array structures into typed JavaScript arrays. It can additionally run nested validation and type conversion on every individual element within the array.

### Example

```typescript
import { Controller, Get, Query, ParseArrayPipe } from '@nestjs/common';

@Controller('tags')
export class TagController {
  @Get('filter')
  filterByIds(
    @Query('ids', new ParseArrayPipe({ items: Number, separator: ',' }))
    ids: number[],
  ) {
    return { parsedIds: ids, count: ids.length, isArray: Array.isArray(ids) };
  }
}

```

### Output

```json
// HTTP GET /tags/filter?ids=10,20,30,40
{
  "parsedIds": [10, 20, 30, 40],
  "count": 4,
  "isArray": true
}

```

---

## 6. What is an Example of the `ParseUUIDPipe` in NestJS?

### Definition & Explanation

`ParseUUIDPipe` validates whether an incoming string parameter conforms strictly to the RFC 4122 UUID standard format (e.g., UUID v4). If the string is not a valid UUID format, a `400 Bad Request` exception is returned.

### Example

```typescript
import { Controller, Get, Param, ParseUUIDPipe } from '@nestjs/common';

@Controller('accounts')
export class AccountController {
  @Get(':uuid')
  getAccount(@Param('uuid', new ParseUUIDPipe({ version: '4' })) uuid: string) {
    return { accountId: uuid, status: 'Valid UUID v4' };
  }
}

```

### Output

```json
// HTTP GET /accounts/e3b0c442-98fc-42c4-be01-29174d817201
{
  "accountId": "e3b0c442-98fc-42c4-be01-29174d817201",
  "status": "Valid UUID v4"
}

// HTTP GET /accounts/invalid-uuid-123
// Status Code: 400 Bad Request
{
  "statusCode": 400,
  "message": "Validation failed (uuid v4 is expected)",
  "error": "Bad Request"
}

```

---

## 7. What is an Example of the `ParseEnumPipe` in NestJS?

### Definition & Explanation

`ParseEnumPipe` validates that an incoming string parameter matches one of the values defined in a specified TypeScript Enum. It restricts route parameter values to valid domain enumeration states.

### Example

```typescript
import { Controller, Get, Query, ParseEnumPipe } from '@nestjs/common';

export enum UserStatus {
  ACTIVE = 'active',
  PENDING = 'pending',
  SUSPENDED = 'suspended',
}

@Controller('users')
export class UserStatusController {
  @Get('by-status')
  getUsersByStatus(
    @Query('status', new ParseEnumPipe(UserStatus)) status: UserStatus,
  ) {
    return { queriedStatus: status };
  }
}

```

### Output

```json
// HTTP GET /users/by-status?status=active
{
  "queriedStatus": "active"
}

// HTTP GET /users/by-status?status=archived
// Status Code: 400 Bad Request
{
  "statusCode": 400,
  "message": "Validation failed (enum value is expected)",
  "error": "Bad Request"
}

```

---

## 8. What is an Example of the `DefaultValuePipe` in NestJS?

### Definition & Explanation

`DefaultValuePipe` acts as a fallback layer for optional route parameter values. If an incoming parameter is `null` or `undefined`, `DefaultValuePipe` injects a predefined fallback value before handing execution over to downstream transformation pipes like `ParseIntPipe`.

### Example

```typescript
import { Controller, Get, Query, DefaultValuePipe, ParseIntPipe } from '@nestjs/common';

@Controller('posts')
export class PostController {
  @Get()
  getPosts(
    @Query('page', new DefaultValuePipe(1), ParseIntPipe) page: number,
    @Query('limit', new DefaultValuePipe(10), ParseIntPipe) limit: number,
  ) {
    return { page, limit, offset: (page - 1) * limit };
  }
}

```

### Output

```json
// HTTP GET /posts (No query parameters supplied)
{
  "page": 1,
  "limit": 10,
  "offset": 0
}

// HTTP GET /posts?page=3&limit=20
{
  "page": 3,
  "limit": 20,
  "offset": 40
}

```

---

## 9. What is an Example of the `ParseFilePipe` in NestJS?

### Definition & Explanation

`ParseFilePipe` inspects and validates uploaded file objects (typically extracted via `@UploadedFile()`). It allows defining custom assertion rules such as maximum file size (`MaxFileSizeValidator`) and permitted MIME types (`FileTypeValidator`).

### Example

```typescript
import { Controller, Post, UseInterceptors, UploadedFile, ParseFilePipe, MaxFileSizeValidator, FileTypeValidator } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';

@Controller('documents')
export class DocumentController {
  @Post('upload')
  @UseInterceptors(FileInterceptor('file'))
  uploadDocument(
    @UploadedFile(
      new ParseFilePipe({
        validators: [
          new MaxFileSizeValidator({ maxSize: 1024 * 1024 * 2 }), // 2 MB Limit
          new FileTypeValidator({ fileType: 'application/pdf' }),
        ],
      }),
    )
    file: Express.Multer.File,
  ) {
    return { fileName: file.originalname, size: file.size, mimeType: file.mimetype };
  }
}

```

### Output

```json
// Valid File Upload (PDF <= 2MB)
{
  "fileName": "report.pdf",
  "size": 1048576,
  "mimeType": "application/pdf"
}

// Invalid File Upload (Image file sent instead of PDF)
// Status Code: 400 Bad Request
{
  "statusCode": 400,
  "message": "Validation failed (expected type is application/pdf)",
  "error": "Bad Request"
}

```

---

## 10. What is an Example of the Dependency Injection (DI) & IoC Pattern in NestJS?

### Definition & Explanation

The Dependency Injection (DI) pattern separates the creation of a dependency from its usage. In NestJS, the Inversion of Control (IoC) container instantiates providers marked with `@Injectable()` and automatically supplies them to controllers or services via constructor parameters.

### Workflow & Architecture

```
[ NestJS Bootstrapper ] ──► [ IoC Container Instantiates LoggerService ]
                                            │
                                            ▼
                             [ Injects into PaymentService Constructor ]
                                            │
                                            ▼
                             [ Injects into PaymentController ]

```

### Example

```typescript
import { Injectable, Controller, Post, Body } from '@nestjs/common';

@Injectable()
export class LoggerService {
  log(message: string) {
    console.log(`[LOG]: ${message}`);
  }
}

@Injectable()
export class PaymentService {
  // Dependency injected via constructor token
  constructor(private readonly logger: LoggerService) {}

  processPayment(amount: number) {
    this.logger.log(`Processing payment of $${amount}`);
    return { success: true, transactionId: 'TX-9901' };
  }
}

@Controller('payments')
export class PaymentController {
  constructor(private readonly paymentService: PaymentService) {}

  @Post()
  pay(@Body('amount') amount: number) {
    return this.paymentService.processPayment(amount);
  }
}

```

### Output

```text
[LOG]: Processing payment of $250
JSON Response: { "success": true, "transactionId": "TX-9901" }

```

---

## 11. What is an Example of the Singleton Pattern in NestJS?

### Definition & Explanation

By default, every provider registered in a NestJS module is configured as a **Singleton**. The NestJS IoC container instantiates the provider exactly once during application startup and shares that single instance across all modules that import and inject it.

### Example

```typescript
import { Injectable, Scope } from '@nestjs/common';

// DEFAULT: Scope.DEFAULT (Singleton Scope across the entire application)
@Injectable({ scope: Scope.DEFAULT })
export class AppConfigService {
  private readonly instanceId = Math.floor(Math.random() * 100000);

  getInstanceId(): number {
    return this.instanceId;
  }
}

@Injectable()
export class ServiceA {
  constructor(public config: AppConfigService) {}
}

@Injectable()
export class ServiceB {
  constructor(public config: AppConfigService) {}
}

```

### Output

```text
// Inspecting injected instance IDs from ServiceA and ServiceB:
ServiceA Config ID: 48291
ServiceB Config ID: 48291  (Exact same instance reference)

```

---

## 12. What is an Example of the Repository Pattern in NestJS?

### Definition & Explanation

The Repository Pattern abstracts data persistence logic behind a uniform collection-like API (`Repository<T>`). In NestJS with TypeORM, the service queries domain model entities through repository abstractions rather than writing raw SQL or coupling directly to database connection drivers.

### Example

```typescript
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository, Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity('products')
export class Product {
  @PrimaryGeneratedColumn()
  id!: number;

  @Column()
  title!: string;
}

@Injectable()
export class ProductService {
  constructor(
    @InjectRepository(Product)
    private readonly productRepo: Repository<Product>,
  ) {}

  async findById(id: number): Promise<Product | null> {
    // Abstracted database operation via repository interface
    return await this.productRepo.findOne({ where: { id } });
  }
}

```

### Output

```text
[SQL Driver Output]: SELECT "Product"."id" AS "Product_id", "Product"."title" AS "Product_title" FROM "products" "Product" WHERE "Product"."id" = 1 LIMIT 1
Result: Product { id: 1, title: 'Mechanical Keyboard' }

```

---

## 13. What is an Example of the Factory Pattern (`useFactory`) in NestJS?

### Definition & Explanation

The Factory Pattern dynamically generates and configures a provider at runtime. In NestJS, `useFactory` creates providers whose instantiation requires conditional logic, runtime configuration reads, or dependencies resolved from other providers.

### Example

```typescript
import { Module } from '@nestjs/common';

export class ApiClient {
  constructor(private readonly apiKey: string, private readonly timeout: number) {}
  getSettings() { return { apiKey: this.apiKey, timeout: this.timeout }; }
}

@Module({
  providers: [
    {
      provide: 'API_CLIENT',
      useFactory: (): ApiClient => {
        const isProd = process.env.NODE_ENV === 'production';
        const apiKey = isProd ? 'PROD-SECRET-KEY' : 'DEV-SANDBOX-KEY';
        const timeout = isProd ? 5000 : 30000;
        return new ApiClient(apiKey, timeout);
      },
    },
  ],
})
export class ApiModule {}

```

### Output

```text
[NestJS Provider Registry]: Resolved 'API_CLIENT' via useFactory() -> ApiClient { apiKey: 'DEV-SANDBOX-KEY', timeout: 30000 }

```

---

## 14. What is an Example of the Decorator Pattern in NestJS?

### Definition & Explanation

The Decorator Pattern dynamically adds metadata and runtime behavior to classes, methods, or properties without modifying their structural source code. NestJS uses decorators (`@Controller()`, `@Get()`, `@UseGuards()`) to wrap target targets with framework execution behavior.

### Example

```typescript
import { SetMetadata, CustomDecorator, applyDecorators, Controller, Get, UseGuards } from '@nestjs/common';

// Custom metadata decorator using Decorator Pattern
export const Roles = (...roles: string[]) => SetMetadata('roles', roles);

// Composed Decorator combining multiple decorators into a single declaration
export function AdminAuthRoute(path: string): CustomDecorator {
  return applyDecorators(
    Get(path),
    Roles('admin', 'superadmin'),
  );
}

@Controller('dashboard')
export class DashboardController {
  @AdminAuthRoute('stats')
  getStats() {
    return { systemHealth: '100%' };
  }
}

```

### Output

```text
[NestFactory] RoutesResolver mapped {/dashboard/stats, GET} route with metadata: roles = ['admin', 'superadmin']

```

---

## 15. What is an Example of the Chain of Responsibility Pattern (Execution Pipeline) in NestJS?

### Definition & Explanation

The Chain of Responsibility pattern passes an incoming request through a sequential chain of handlers. In NestJS, the request pipeline passes through Middleware, Guards, Interceptors, Pipes, Controller Handlers, and Exception Filters in a defined, deterministic sequence.

### Workflow & Architecture

```
[ Request ] ──► [ Middleware ] ──► [ Guard ] ──► [ Interceptor (Pre) ] ──► [ Pipe ] ──► [ Controller Handler ]
                                                                                                  │
                                                                                                  ▼
[ Response ] ◄── [ Exception Filter ] ◄── [ Interceptor (Post) ] ◄────────────────────────────────┘

```

### Example

```typescript
import { Injectable, NestMiddleware, CanActivate, ExecutionContext, NestInterceptor, CallHandler, PipeTransform } from '@nestjs/common';
import { Observable, tap } from 'rxjs';

// Step 1: Middleware
@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: any, res: any, next: () => void) {
    console.log('1. Middleware: Request logging');
    next();
  }
}

// Step 2: Guard
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    console.log('2. Guard: Authenticating request');
    return true;
  }
}

// Step 3: Interceptor
@Injectable()
export class TimingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('3. Interceptor: Before handler execution');
    return next.handle().pipe(tap(() => console.log('6. Interceptor: After handler execution')));
  }
}

// Step 4: Pipe
@Injectable()
export class SanitizePipe implements PipeTransform {
  transform(value: any) {
    console.log('4. Pipe: Transforming payload');
    return value;
  }
}

```

### Output

```text
1. Middleware: Request logging
2. Guard: Authenticating request
3. Interceptor: Before handler execution
4. Pipe: Transforming payload
5. Controller Handler: Executing business logic
6. Interceptor: After handler execution

```

---

## 16. What is an Example of the Module Pattern in NestJS?

### Definition & Explanation

The Module Pattern encapsulates related features, controllers, and providers into cohesive, isolated functional units using the `@Module()` decorator. Modules control visibility by explicitly declaring which internal providers are private and which are exported for consumption by other modules.

### Example

```typescript
import { Module, Injectable } from '@nestjs/common';

@Injectable()
export class InternalTaskService {
  execute() { return 'Task Completed'; }
}

@Injectable()
export class SharedNotificationService {
  notify() { return 'Notification Sent'; }
}

@Module({
  providers: [InternalTaskService, SharedNotificationService],
  // SharedNotificationService is exposed to importing modules; InternalTaskService remains private
  exports: [SharedNotificationService], 
})
export class NotificationModule {}

```

### Output

```text
[NestModule] NotificationModule initialized.
Exported providers available externally: [SharedNotificationService]
Encapsulated private providers: [InternalTaskService]

```

---

## Senior Interview Cheatsheet

* **Built-in NestJS Pipes:** 9 built-in pipes (`ValidationPipe`, `ParseIntPipe`, `ParseFloatPipe`, `ParseBoolPipe`, `ParseArrayPipe`, `ParseUUIDPipe`, `ParseEnumPipe`, `DefaultValuePipe`, `ParseFilePipe`) handle input parsing, type casting, domain validation, and parameter defaulting.
* **`ValidationPipe` Engine:** Integrates `class-transformer` and `class-validator` to convert plain JSON objects to DTO class instances and enforce constraint decorators (`@IsEmail`, `@Min`).
* **`ParseUUIDPipe` & `ParseEnumPipe`:** Critical for parameter safety; `ParseUUIDPipe` enforces RFC 4122 compliance, while `ParseEnumPipe` validates string path/query values against TypeScript enums.
* **Dependency Injection & IoC:** The IoC container controls provider instantiation and resolves dependency graphs automatically via constructor types.
* **Singleton Scope Default:** Providers in NestJS are singletons by default across the application lifecycle unless explicitly configured as `Scope.REQUEST` or `Scope.TRANSIENT`.
* **Repository Pattern:** Separates database access code from business logic through generic interface repositories (`Repository<Entity>`), enabling unit testing via mocked repositories.
* **Factory Providers (`useFactory`):** Resolves dynamic dependencies at runtime using environment variables, conditional checks, or dependent service values.
* **Chain of Responsibility Execution:** NestJS processes requests in a precise sequence: Middleware → Guards → Interceptors (Pre) → Pipes → Controller Handler → Interceptors (Post) → Exception Filters.
* **Module Encapsulation:** NestJS `@Module()` declarations encapsulate domain scopes. Only providers listed in the `exports` array are accessible to external importing modules.