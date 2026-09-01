## 1. What is NestJS and why use it?
NestJS is a progressive Node.js framework built with TypeScript for constructing scalable, enterprise-grade server-side applications. Inspired by Angular's architecture, NestJS provides a structured development experience through modularity, dependency injection, and decorators.

Key reasons to use NestJS:

* **Well-Defined Modular Structure:** Enforces strict domain separation through self-contained modules, improving maintainability across teams.
* **Built-in Inversion of Control (IoC):** Native Dependency Injection (DI) decouples business logic from class instantiations.
* **MVC Pattern & Separation of Concerns:** Segregates Controllers (request handling), Services (business logic), and Repositories (data layer).
* **TypeScript First:** Enforces compile-time type safety, catching structural errors early in development.

### Example

#### Modern NestJS Application Architecture

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

// app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}

// app.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello NestJS!';
  }
}

```

### Output

```text
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [NestFactory] Starting Nest application...
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [InstanceLoader] AppModule dependencies initialized +2ms
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [RoutesResolver] AppController {/}: +2ms
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [NestApplication] Nest application successfully started +3ms
HTTP GET / -> "Hello NestJS!"

```

---

## 2. What are modules in NestJS and how are imports, controllers, providers, and exports defined?
A Module is a class decorated with `@Module()` that provides metadata NestJS uses to organize application structure into feature domains. Every NestJS application requires at least one root module (`AppModule`).

The `@Module()` decorator metadata properties:

* `imports`: List of imported modules that export providers required by this module.
* `controllers`: Controllers defined within this module that handle incoming requests.
* `providers`: Services, repositories, or helpers instantiated by the NestJS IoC container.
* `exports`: Subset of providers that this module provides and should be available in other modules importing this module.

### Workflow / Architecture

```text
┌────────────────────────────────────────────────────────┐
│                      UsersModule                       │
│  ┌────────────────────┐      ┌──────────────────────┐  │
│  │    Controllers     │      │      Providers       │  │
│  │  [UsersController] │ ───► │   [UsersService]     │  │
│  └────────────────────┘      └──────────┬───────────┘  │
└─────────────────────────────────────────┼──────────────┘
                                          │ (Exported)
                                          ▼
┌────────────────────────────────────────────────────────┐
│                      OrdersModule                      │
│  ┌────────────────────┐      ┌──────────────────────┐  │
│  │      Imports       │      │     Controllers      │  │
│  │   [UsersModule]    │ ───► │  [OrdersController]  │  │
│  └────────────────────┘      └──────────────────────┘  │
└────────────────────────────────────────────────────────┘

```

### Example

```typescript
// users.module.ts
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
  imports: [],
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService], // Shared with other modules importing UsersModule
})
export class UsersModule {}

```

### Output

```text
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [InstanceLoader] UsersModule dependencies initialized +1ms
UsersModule registered: 1 Controller (UsersController), 1 Provider (UsersService), 1 Export (UsersService).

```

---

## 3. What are controllers in NestJS?
Controllers handle incoming HTTP requests and return responses to the client. They are annotated with `@Controller('prefix')`, which sets an optional route path prefix for all endpoints within the class. Route HTTP verbs are mapped via method decorators like `@Get()`, `@Post()`, `@Put()`, `@Delete()`, and `@Patch()`.

### Example

```typescript
// users.controller.ts
import { Controller, Get, Post, Body, Param } from '@nestjs/common';
import { UsersService } from './users.service';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() {
    return this.usersService.findAll();
  }

  @Post()
  createUser(@Body() user: { name: string; email: string }) {
    return this.usersService.createUser(user);
  }
}

```

### Output

```text
HTTP Request: GET /users
Response Status: 200 OK
Response Body: [ { "id": 1, "name": "Alice", "email": "alice@example.com" } ]

```

---

## 4. What are providers and services in NestJS?
Providers are fundamental NestJS concepts; services, repositories, factories, and helpers can all be treated as providers. A Service contains core business logic and is decorated with `@Injectable()`. This tells NestJS that the service can be managed by the IoC container and injected into controllers or other services.

### Example

```typescript
// users.service.ts
import { Injectable } from '@nestjs/common';

export interface User {
  id: number;
  name: string;
  email: string;
}

@Injectable()
export class UsersService {
  private users: User[] = [];

  findAll(): User[] {
    return this.users;
  }

  createUser(user: Omit<User, 'id'>): User {
    const newUser = { id: Date.now(), ...user };
    this.users.push(newUser);
    return newUser;
  }
}

```

### Output

```text
Method Execution: service.createUser({ name: 'Bob', email: 'bob@example.com' })
Result: { id: 1724543160000, name: 'Bob', email: 'bob@example.com' }

```

---

## 5. What is Dependency Injection (DI) in NestJS and how do you create custom providers?
Dependency Injection is an Inversion of Control (IoC) design pattern where dependencies are supplied to a class rather than created inside it manually.

NestJS resolves dependencies automatically using constructor injection. For custom scenarios (e.g., dynamic creation, runtime parameters, configuration injection), NestJS supports custom providers using `useValue`, `useFactory`, or `useClass` strategies.

### Workflow / Architecture

```text
 [ Application Bootstrap ]
            │
            ▼
 ┌──────────────────────┐
 │  Nest IoC Container  │
 └──────────┬───────────┘
            │ Resolves Dependencies dynamically
            ▼
 ┌──────────────────────────────────────────────┐
 │  Execute Factory Provider (useFactory)       │
 └──────────┬───────────────────────────────────┘
            │ Injects instantiated payload
            ▼
 ┌──────────────────────────────────────────────┐
 │  Inject into Service Constructor (@Inject)   │
 └──────────────────────────────────────────────┘

```

### Example

```typescript
import { Module, Injectable, Inject } from '@nestjs/common';

// 1. Define Provider Strategy
const CustomConfigProvider = {
  provide: 'CUSTOM_CONFIG',
  useFactory: async () => {
    // Simulating loading config from an external service/vault
    return { apiKey: 'secret-key-99', region: 'us-east-1' };
  },
};

// 2. Consume Injected Custom Provider
@Injectable()
export class AppConfigService {
  constructor(@Inject('CUSTOM_CONFIG') private readonly config: Record<string, any>) {}

  getRegion(): string {
    return this.config.region;
  }
}

// 3. Register Provider in Module
@Module({
  providers: [CustomConfigProvider, AppConfigService],
  exports: [AppConfigService],
})
export class ConfigModule {}

```

### Output

```text
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [InstanceLoader] ConfigModule dependencies initialized +3ms
Injected Custom Config Region: "us-east-1"

```

---

## 6. What are decorators in NestJS?
Decorators are functions annotated with `@` that add metadata to classes, methods, parameters, or properties. NestJS uses decorators extensively to configure routes, inject dependencies, assign guard authorizations, and apply pipes. Custom parameter decorators can be created using `createParamDecorator()`.

### Example

```typescript
import { createParamDecorator, ExecutionContext, Controller, Get } from '@nestjs/common';

// Custom Parameter Decorator to extract header token
export const AuthToken = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    return request.headers['authorization'];
  },
);

@Controller('session')
export class SessionController {
  @Get()
  getSession(@AuthToken() token: string) {
    return { tokenReceived: token || 'No token supplied' };
  }
}

```

### Output

```text
HTTP Request: GET /session Headers: { "authorization": "Bearer jwt.token.val" }
Response Body: { "tokenReceived": "Bearer jwt.token.val" }

```

---

## 7. What are pipes in NestJS and how do you implement custom validation pipes?
Pipes operate on arguments before route handlers are invoked. They are used for:

1. **Transformation:** Transforming input payloads to the desired schema/type (e.g., string to integer).
2. **Validation:** Validating incoming payloads and throwing exceptions when input validation fails.

Pipes implement `PipeTransform` and must implement the `transform()` method. While NestJS includes built-in pipes like `ValidationPipe` (using `class-validator`), custom pipes can be built for specific transformations.

### Example

```typescript
import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException, Controller, Get, Param } from '@nestjs/common';

// Custom Transformation Pipe
@Injectable()
export class CustomParseIntPipe implements PipeTransform<string, number> {
  transform(value: string, metadata: ArgumentMetadata): number {
    const val = parseInt(value, 10);
    if (isNaN(val)) {
      throw new BadRequestException(`Validation failed: "${value}" is not a valid number`);
    }
    return val;
  }
}

@Controller('items')
export class ItemsController {
  @Get(':id')
  getItem(@Param('id', new CustomParseIntPipe()) id: number) {
    return { itemId: id, type: typeof id };
  }
}

```

### Output

```text
HTTP GET /items/42  -> { "itemId": 42, "type": "number" }
HTTP GET /items/abc -> 400 Bad Request: { "statusCode": 400, "message": "Validation failed: \"abc\" is not a valid number" }

```

---

## 8. What are interceptors in NestJS and how do you implement a custom logging interceptor?
Interceptors implement `NestInterceptor` and make it possible to:

* Bind extra logic before and after method execution.
* Transform the result returned from a route handler.
* Extend basic function execution to measure timing, manipulate cache, or log executions.

They rely on RxJS `Observable` streams to manipulate request/response lifecycles.

### Workflow / Architecture

```text
[ Incoming Request ] ──► [ Interceptor (Before Logic) ]
                                    │
                                    ▼
                          [ Route Handler Logic ]
                                    │
                                    ▼
[ Outgoing Response ] ◄── [ Interceptor (After Logic via RxJS) ]

```

### Example

```typescript
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest();
    const method = request.method;
    const url = request.url;
    const now = Date.now();

    console.log(`Incoming Request: ${method} ${url}`);

    return next.handle().pipe(
      tap((data) => {
        console.log(
          `Response: ${JSON.stringify(data)} - Duration: ${Date.now() - now}ms`,
        );
      }),
    );
  }
}

```

### Output

```text
Incoming Request: GET /users
Response: [{"id":1,"name":"Alice"}] - Duration: 12ms

```

---

## 9. What are guards in NestJS and how do you implement Role-Based Access Control (RBAC)?
Guards implement `CanActivate` and determine whether an incoming request will be handled by the route handler based on runtime permissions, security roles, or authentication status.

Guards run **after** middleware but **before** pipes or interceptors. Role-based access control (RBAC) combines guards with custom metadata set using `Reflector`.

### Example

```typescript
import { Injectable, CanActivate, ExecutionContext, SetMetadata, UseGuards, Controller, Get } from '@nestjs/common';
import { Reflector } from '@nestjs/core';

// Custom Metadata Decorator
export const Roles = (...roles: string[]) => SetMetadata('roles', roles);

// RBAC Guard
@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private readonly reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.get<string[]>('roles', context.getHandler());
    if (!requiredRoles) return true; // No permissions restricted

    const request = context.switchToHttp().getRequest();
    const user = request.user || { roles: ['guest'] }; // Simulated attached user payload

    return requiredRoles.some((role) => user.roles.includes(role));
  }
}

// Protected Controller Path
@Controller('admin')
@UseGuards(RolesGuard)
export class AdminController {
  @Get('dashboard')
  @Roles('admin')
  getDashboard() {
    return { status: 'Access Granted to Admin Dashboard' };
  }
}

```

### Output

```text
Request User: { roles: ['guest'] } -> HTTP GET /admin/dashboard
Response Status: 403 Forbidden

Request User: { roles: ['admin'] } -> HTTP GET /admin/dashboard
Response Status: 200 OK -> { "status": "Access Granted to Admin Dashboard" }

```

---

## 10. What are exception filters in NestJS?
Exception filters process unhandled exceptions thrown during application execution. The framework provides a built-in global exception filter (`HttpException`), but custom filters allow developers to standardise output format, intercept errors, and handle logging.

### Example

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException, HttpStatus } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch()
export class GlobalExceptionFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    const status =
      exception instanceof HttpException
        ? exception.getStatus()
        : HttpStatus.INTERNAL_SERVER_ERROR;

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      message: (exception as any).message || 'Internal server error',
    });
  }
}

```

### Output

```text
HTTP Trigger: Throw Internal Server Exception
Response Output:
{
  "statusCode": 500,
  "timestamp": "2026-08-24T23:46:00.000Z",
  "path": "/error-test",
  "message": "Internal server error"
}

```

---

## 11. What is Middleware in NestJS?
Middleware is a function called **before** the route handler and before guards. Middleware functions access the HTTP `request` and `response` objects, as well as the `next()` middleware function in the application’s request-response cycle. NestJS middleware can be functional or class-based using the `NestMiddleware` interface.

### Example

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class HTTPLoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`[HTTP Middleware Log]: ${req.method} request to ${req.originalUrl}`);
    next();
  }
}

```

### Output

```text
[HTTP Middleware Log]: GET request to /api/v1/health
Route Execution proceeds to Guards -> Pipes -> Controller.

```

---

## 12. How does NestJS handle database integration?
NestJS is database-agnostic and supports integration with relational (SQL) or NoSQL databases via Object-Relational Mapping (ORM) tools such as TypeORM, Prisma, Sequelize, or Mongoose.

Integrations typically register connection configurations using static methods like `.forRoot()` in module imports, while domain models and entities are registered locally via `.forFeature()`.

### Example

```typescript
// app.module.ts Integration with TypeORM
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: 'localhost',
      port: 5432,
      username: 'postgres',
      password: 'password',
      database: 'app_db',
      autoLoadEntities: true,
      synchronize: false, // Set false for production
    }),
  ],
})
export class AppModule {}

```

### Output

```text
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [TypeOrmModule] Database connection initialized successfully +45ms

```

---

## 13. What are DTOs (Data Transfer Objects) in NestJS?
A Data Transfer Object (DTO) defines the shape of data sent over the network for a request or response. DTOs combined with `class-validator` and `class-transformer` enable runtime validation and object transformation when parsed by the global `ValidationPipe`.

### Example

```typescript
import { IsString, IsEmail, MinLength } from 'class-validator';

export class CreateUserDto {
  @IsString()
  @MinLength(2)
  name: string;

  @IsEmail()
  email: string;
}

```

### Output

```text
HTTP Request: POST /users Payload: { "name": "A", "email": "invalid-email" }
Response Status: 400 Bad Request
Response Payload:
{
  "statusCode": 400,
  "message": [
    "name must be longer than or equal to 2 characters",
    "email must be an email"
  ],
  "error": "Bad Request"
}

```

---

## 14. What is the difference between `forRoot()` and `forFeature()` in modules?
* `forRoot()`: Called in the root module (`AppModule`) to configure global singletons, options, or dynamic provider connections (e.g., global database credentials, main configuration options).
* `forFeature()`: Called in domain feature modules to register specific entities, repositories, or scoped configuration subsets belonging exclusively to that module context.

### Example

```typescript
// 1. Root Module Registration
@Module({
  imports: [
    TypeOrmModule.forRoot({ /* connection options */ }),
  ],
})
export class AppModule {}

// 2. Feature Module Registration
@Module({
  imports: [
    TypeOrmModule.forFeature([UserEntity]), // Registers entity scoped to UserModule
  ],
})
export class UserModule {}

```

### Output

```text
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [TypeOrmModule] Root connection established +30ms
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [TypeOrmModule] Registered UserEntity repository for UserModule +2ms

```

---

## 15. What are Dynamic Modules in NestJS and how do you implement them?
Dynamic Modules allow modules to be created dynamically based on dynamic parameters passed via static configuration methods (such as `register()`, `forRoot()`, or `forFeature()`). They are useful for abstracting shared infrastructure modules (such as configuration managers, database wrappers, or dynamic API clients).

### Example

```typescript
import { DynamicModule, Module } from '@nestjs/common';

export interface DynamicConfigOptions {
  apiKey: string;
  region: string;
}

@Module({})
export class DynamicConfigModule {
  static register(options: DynamicConfigOptions): DynamicModule {
    return {
      module: DynamicConfigModule,
      providers: [
        {
          provide: 'CONFIG_OPTIONS',
          useValue: options,
        },
      ],
      exports: ['CONFIG_OPTIONS'],
    };
  }
}

// Module Registration Usage
@Module({
  imports: [
    DynamicConfigModule.register({ apiKey: 'key_123', region: 'eu-central-1' }),
  ],
})
export class FeatureModule {}

```

### Output

```text
DynamicConfigModule configured dynamically with payload: { apiKey: 'key_123', region: 'eu-central-1' }

```

---

## 16. How does NestJS support Microservices architecture, what are the trade-offs, and how is distributed tracing handled?
NestJS includes built-in microservices support via the `@nestjs/microservices` package. It handles network protocols such as TCP, Redis, NATS, RabbitMQ, Kafka, and gRPC using message and event patterns.

**Trade-offs of Microservices:**

* **Scalability & Fault Isolation:** Services scale independently; failures are isolated to localized boundaries.
* **Challenges:** Network overhead/latency, distributed transaction handling, and cross-service debugging complexity.

**Distributed Tracing Solution:** Distributed tracing can be implemented using standard OpenTelemetry packages to attach tracing context (like trace parent IDs) across service requests.

### Workflow / Architecture

```text
[ HTTP Gateway ] ──► (Message Pattern via TCP/Kafka) ──► [ Microservice Instance ]
       │                                                         │
       └────────────── OpenTelemetry Trace ID propagation ──────┘

```

### Example

```typescript
// Microservice Controller Implementation
import { Controller } from '@nestjs/common';
import { MessagePattern, EventPattern, Payload } from '@nestjs/microservices';

@Controller()
export class MicroserviceItemController {
  // Request-Response pattern
  @MessagePattern({ cmd: 'get_item_details' })
  getItemDetails(@Payload() id: number) {
    return { id, name: 'Microservice Transferred Item' };
  }

  // Event-Driven pattern (Fire and forget)
  @EventPattern('item_created')
  handleItemCreated(@Payload() data: Record<string, any>) {
    console.log('Async event handled for created item:', data);
  }
}

```

### Output

```text
[Nest Microservice] Listening on Transport.TCP (Port 8877)
Message received on pattern {"cmd":"get_item_details"}: Output -> { id: 10, name: "Microservice Transferred Item" }

```

---

## 17. How does NestJS handle Authentication and Authorization?
Authentication in NestJS is commonly implemented using Passport strategies via `@nestjs/passport` and JWT (JSON Web Tokens) using `@nestjs/jwt`. The standard strategy encapsulates verification in a `Strategy` implementation and enforces routes using a `JwtAuthGuard`.

### Example

```typescript
// jwt.strategy.ts
import { Injectable } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: 'super-secret-key',
    });
  }

  async validate(payload: { sub: string; username: string }) {
    return { userId: payload.sub, username: payload.username };
  }
}

// Guard application
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {}

```

### Output

```text
Incoming Bearer token verified by JwtStrategy.
Request Object decorated with validated payload: req.user = { userId: "usr_10", username: "dev" }

```

---

## 18. What is the lifecycle of a request in NestJS?
When a request enters a NestJS application, it flows through specific framework layers in a strict order.

Execution Order:

1. **Incoming Request**
2. **Global / Module Middleware**

3. **Guards** (Global -> Controller -> Route)
4. **Interceptors (Before handler execution)**

5. **Pipes** (Global -> Controller -> Route -> Parameters)
6. **Controller (Route Handler Execution)**

7. **Service (Business logic execution)**

8. **Interceptors (After handler execution via RxJS response stream)**

9. **Exception Filters** (If unhandled exceptions occur)
10. **Outgoing Response**

### Workflow / Architecture

```text
 Request ──► [ Middleware ] ──► [ Guards ] ──► [ Interceptor (Before) ] ──► [ Pipes ]
                                                                                │
 Response ◄── [ Filters ] ◄── [ Interceptor (After) ] ◄── [ Controller Handler ]

```

### Output

```text
[1] HTTP Middleware Execution
[2] Guard Permission Evaluated: Passed
[3] Interceptor (Pre-Processing)
[4] Pipe Validation Executed
[5] Controller Method Executed -> Service Layer Executed
[6] Interceptor (Post-Processing Response Formatted)
[7] HTTP Response Sent to Client

```

---

## 19. What is the difference between Monorepo and Microservice architecture in NestJS?
NestJS natively supports both structural paradigms:

* **Monorepo:** Applications, domain projects, and shared libraries reside inside a single code repository managed via Nest CLI workspaces (`nest-cli.json`). Shares direct internal code dependencies efficiently.
* **Microservices Architecture:** Codebases are physically separated into independent repositories or autonomous projects. Services communicate across network transports (such as Kafka, gRPC, or RabbitMQ).

### Example

#### NestJS Monorepo CLI Structure (`nest-cli.json`)

```json
{
  "$schema": "https://json.schemastore.org/nest-cli",
  "collection": "@nestjs/schematics",
  "sourceRoot": "apps/nest-app/src",
  "compilerOptions": {
    "webpack": true
  },
  "projects": {
    "api-gateway": {
      "type": "application",
      "root": "apps/api-gateway"
    },
    "shared-lib": {
      "type": "library",
      "root": "libs/shared-lib"
    }
  }
}

```

### Output

```text
Nest CLI Monorepo build:
Building project "api-gateway"... Done.
Building library "shared-lib"... Done.

```

---

## 20. How do you implement caching in NestJS?
NestJS provides caching abstraction through `@nestjs/cache-manager`. Caching can be applied using:

1. **Auto-Caching Interceptor (`CacheInterceptor`):** Automatically caches HTTP responses.
2. **Manual Cache Control:** Injecting `CACHE_MANAGER` to explicitly read, write, or delete keys programmatically.

### Example

```typescript
import { Controller, Get, UseInterceptors, Inject } from '@nestjs/common';
import { CacheInterceptor, CacheKey, CacheTTL, CACHE_MANAGER } from '@nestjs/cache-manager';
import { Cache } from 'cache-manager';

@Controller('products')
export class ProductsController {
  constructor(@Inject(CACHE_MANAGER) private cacheManager: Cache) {}

  // 1. Automatic Route Caching
  @Get('auto')
  @UseInterceptors(CacheInterceptor)
  @CacheTTL(10000) // TTL in milliseconds
  getAutoCachedData() {
    return { data: 'Expensive product payload result' };
  }

  // 2. Programmatic Manual Cache API
  @Get('manual')
  async getManualCachedData() {
    const cached = await this.cacheManager.get('custom_key');
    if (cached) return { source: 'cache', data: cached };

    const freshData = 'Computed Value';
    await this.cacheManager.set('custom_key', freshData, 10000);
    return { source: 'db', data: freshData };
  }
}

```

### Output

```text
Request 1 (Manual): GET /products/manual -> Output: { "source": "db", "data": "Computed Value" }
Request 2 (Manual): GET /products/manual -> Output: { "source": "cache", "data": "Computed Value" }

```

---

## 21. How do you integrate and scale WebSockets in a distributed NestJS application?
WebSockets in NestJS are managed via the `@nestjs/websockets` package using gateways annotated with `@WebSocketGateway()`.

When scaling WebSocket applications horizontally across multiple server instances, state must be synchronized so connected sockets on separate nodes can communicate. This is typically solved using a Redis Pub/Sub adapter (e.g., `@socket.io/redis-adapter`) to broadcast socket messages across node clusters.

### Example

```typescript
import {
  WebSocketGateway,
  WebSocketServer,
  SubscribeMessage,
  MessageBody,
} from '@nestjs/websockets';
import { Server } from 'socket.io';

@WebSocketGateway({ cors: { origin: '*' } })
export class AppEventsGateway {
  @WebSocketServer()
  server: Server;

  @SubscribeMessage('events')
  handleEvent(@MessageBody() data: string): string {
    // Broadcast event to connected client sockets
    this.server.emit('broadcast', { event: data, time: new Date() });
    return data;
  }
}

```

### Output

```text
[WebSocket Client] Connected to socket gateway server.
[Gateway Emit Event] Channel: "broadcast" Payload: { "event": "Hello WS", "time": "2026-08-24T23:46:00.000Z" }

```

---

## 22. How do you handle circular dependencies in NestJS?
Circular dependencies happen when two classes or modules directly or indirectly depend on each other (`ClassA` injects `ClassB`, and `ClassB` injects `ClassA`).

NestJS resolves circular dependencies using the `forwardRef()` function, which defers reference resolution until both dependencies can be initialized.

### Example

```typescript
// service-a.ts
import { Injectable, Inject, forwardRef } from '@nestjs/common';
import { ServiceB } from './service-b';

@Injectable()
export class ServiceA {
  constructor(
    @Inject(forwardRef(() => ServiceB))
    private readonly serviceB: ServiceB,
  ) {}

  getValueA(): string {
    return 'Value A';
  }
}

// service-b.ts
import { Injectable, Inject, forwardRef } from '@nestjs/common';
import { ServiceA } from './service-a';

@Injectable()
export class ServiceB {
  constructor(
    @Inject(forwardRef(() => ServiceA))
    private readonly serviceA: ServiceA,
  ) {}

  executeCombined(): string {
    return `ServiceB calling: ${this.serviceA.getValueA()}`;
  }
}

```

### Output

```text
[Nest] 15234 - 08/24/2026, 11:46:00 PM   LOG [InstanceLoader] ServiceA and ServiceB resolved using forwardRef +2ms
Method Output: "ServiceB calling: Value A"

```

---

## 23. How do you implement CQRS (Command Query Responsibility Segregation) in NestJS?
CQRS separates write operations (Commands) from read operations (Queries) to optimize performance, complexity, and maintainability. NestJS provides an official `@nestjs/cqrs` module exposing dedicated Command Busses, Query Busses, Command Handlers, Query Handlers, and Event Publishers.

### Workflow / Architecture

```text
  [ Client Endpoint ]
           │
     ┌─────┴─────┐
     ▼           ▼
[ Command ]  [ Query ]
     │           │
     ▼           ▼
[ Handler ]  [ Handler ]
     │           │
(Write Side) (Read Side)

```

### Example

```typescript
import { Module } from '@nestjs/common';
import { CqrsModule, CommandHandler, ICommandHandler, QueryHandler, IQueryHandler } from '@nestjs/cqrs';

// 1. Command & Handler
export class CreateUserCommand {
  constructor(public readonly username: string) {}
}

@CommandHandler(CreateUserCommand)
export class CreateUserHandler implements ICommandHandler<CreateUserCommand> {
  async execute(command: CreateUserCommand): Promise<string> {
    return `User ${command.username} successfully created via CommandHandler`;
  }
}

// 2. Query & Handler
export class GetUserQuery {
  constructor(public readonly userId: string) {}
}

@QueryHandler(GetUserQuery)
export class GetUserHandler implements IQueryHandler<GetUserQuery> {
  async execute(query: GetUserQuery): Promise<any> {
    return { id: query.userId, username: 'read_side_user' };
  }
}

@Module({
  imports: [CqrsModule],
  providers: [CreateUserHandler, GetUserHandler],
})
export class CqrsUserModule {}

```

### Output

```text
CommandBus -> Executed CreateUserCommand -> "User dev_john successfully created via CommandHandler"
QueryBus   -> Executed GetUserQuery      -> { "id": "usr_100", "username": "read_side_user" }

```

---

## 24. How do you ensure transactional consistency in a distributed NestJS application?
Ensuring transaction consistency across microservices requires patterns like **Saga** or **Transactional Outbox** instead of traditional ACID transactions.

A Saga manages transactions through sequential steps. If a step fails, compensation events are dispatched to undo previous actions. NestJS implements Sagas using RxJS event streams via `@nestjs/cqrs`.

### Workflow / Architecture

```text
[ OrderCreated Event ] ──► [ Saga Manager ] ──► Dispatch Trigger Component Commands
                                 │
                                 ▼ (If Step Fails)
                        [ Trigger Compensating Actions ]

```

### Example

```typescript
import { Injectable } from '@nestjs/common';
import { Saga, ICommand } from '@nestjs/cqrs';
import { Observable } from 'rxjs';
import { map, filter } from 'rxjs/operators';

export class OrderCreatedEvent {
  constructor(public readonly orderId: string) {}
}

export class CreateInvoiceCommand {
  constructor(public readonly orderId: string) {}
}

@Injectable()
export class OrderSaga {
  @Saga()
  orderCreated = (events$: Observable<any>): Observable<ICommand> => {
    return events$.pipe(
      filter((event) => event instanceof OrderCreatedEvent),
      map((event: OrderCreatedEvent) => {
        console.log(`[Saga] Reacting to OrderCreatedEvent for order: ${event.orderId}`);
        return new CreateInvoiceCommand(event.orderId);
      }),
    );
  };
}

```

### Output

```text
[Event Published]: OrderCreatedEvent { orderId: 'ord_9901' }
[Saga Reacted]: Dispatched CreateInvoiceCommand for order: ord_9901

```

---

## Senior NestJS Interview Cheatsheet & Key Points

### 1. Framework Fundamentals & Architectural Hierarchy

* Core lifecycle execution sequence: `Middleware` → `Guards` → `Interceptors (Before)` → `Pipes` → `Controller Handler` → `Interceptors (After)` → `Exception Filters`.
* Modularity principles: Wrap functionalities inside domain modules. Export shared services in `exports` array to reuse across importing modules.
* Provider Lifetime Scopes:
* `DEFAULT`: Singleton scope initialized once across application startup (Optimal performance).
* `REQUEST`: New instance generated for every incoming request (Higher memory cost).
* `TRANSIENT`: Dedicated instance created for every injecting consumer.

### 2. Enterprise Design Patterns in NestJS

* Dependency Injection: Leverage constructor injection; use dynamic custom providers (`useValue`, `useFactory`, `useClass`) for dynamic setups.
* Dynamic Modules: Structure reusable libraries by exposing static methods like `forRoot()` or `register()` returning custom `DynamicModule` definitions.
* CQRS: Separate mutations (Commands) from reads (Queries) using `@nestjs/cqrs` to enable horizontal read scaling.
* Distributed Consistency: Use the Saga pattern via RxJS streams to handle compensating actions across distributed services.

### 3. Performance & Infrastructure Best Practices

* Underlying Engine: Swap standard Express adapter for `@nestjs/platform-fastify` to increase HTTP throughput.
* Memory Management: Avoid using `REQUEST` scope unless strictly required for tenant/context isolation.
* Distributed Caching: Use `@nestjs/cache-manager` backed by Redis stores for shared read caches.
* Distributed Microservices: Leverage gRPC for low-latency sync internal communication, and RabbitMQ/NATS for event driven async tasks. Scale WebSockets across multiple nodes using Redis adapters.

# NestJS Developer Interview Guide (Top 20 Questions & Answers)

---

## SECTION 1: Essential Technical Questions & Core Framework Knowledge

### Question 1. What is dependency injection in NestJS and how does it differ from traditional Node.js module patterns?

#

Dependency Injection (DI) is an Inversion of Control (IoC) design pattern where dependencies are supplied to a class rather than created inside it. NestJS features a built-in IoC container managed via TypeScript metadata decorators (`@Injectable()`).

Unlike traditional Node.js patterns (e.g., standard `require` or `import` statements), which tightly couple modules through direct instantiation or static singletons, NestJS's DI container automatically manages provider lifecycles, resolves complex dependency trees at runtime, handles circular reference injection via forward references, and enables dynamic mocking during automated testing.

#### Workflow / Architecture

```text
[ Incoming Request / Service Trigger ]
                   │
                   ▼
       [ NestJS IoC Container ]
       (Scans @Injectable() Metadata)
                   │
         ┌─────────┴─────────┐
         ▼                   ▼
[ Resolve Dependencies ] ──► [ Instantiate Provider ] ──► [ Inject into Constructor ]

```

#### Example

##### Legacy Node.js / Express Pattern (Manual Instantiation & Tight Coupling)

```typescript
// Legacy: Direct instantiation inside class constructor
import { DatabaseClient } from './database';

export class UserService {
  private db: DatabaseClient;

  constructor() {
    // Tightly coupled dependency; difficult to mock or swap
    this.db = new DatabaseClient({ host: 'localhost', port: 5432 });
  }

  async getUser(id: string) {
    return this.db.findUser(id);
  }
}

```

##### Modern NestJS v10+ Pattern (Dependency Injection)

```typescript
// Modern NestJS: Injected dependency via IoC container
import { Injectable } from '@nestjs/common';
import { DatabaseService } from '../database/database.service';

@Injectable()
export class UserService {
  // Dependency automatically injected by NestJS IoC container
  constructor(private readonly dbService: DatabaseService) {}

  async getUser(id: string) {
    return this.dbService.findUser(id);
  }
}

```

#### Output

```text
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [InstanceLoader] DatabaseService dependencies initialized +2ms
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [InstanceLoader] UserService dependencies initialized +1ms
[UserService] User retrieved successfully: { id: 'usr_101', name: 'Alex' }

```

---

### Question 2. Explain the different types of providers in NestJS and when to use each.

#

In NestJS, a provider is any class or token registered with the IoC container. NestJS supports four primary provider configuration options inside module metadata:

* **Standard Class Providers (`useClass` or simple class name):** Standard singletons auto-instantiated by the container. Use for default services, repositories, and utilities.
* **Value Providers (`useValue`):** Injects a static value, constant, or pre-configured object. Use for configuration objects, environment constants, or replacing services with mock implementations during testing.
* **Factory Providers (`useFactory`):** Dynamically computes and returns a provider instance. Use when creation logic requires async operations (e.g., database connection setup) or conditional instantiation depending on runtime configuration.
* **Existing Providers (`useExisting`):** Creates an alias for an already registered provider token. Use to expose a single provider under multiple interface tokens or retain backward compatibility.

#### Example

```typescript
import { Module } from '@nestjs/common';
import { UserService } from './user.service';

const CONFIG_VALUE = { apiVersion: 'v2', environment: 'production' };

@Module({
  providers: [
    // 1. Standard Class Provider
    UserService,

    // 2. Value Provider
    {
      provide: 'APP_CONFIG',
      useValue: CONFIG_VALUE,
    },

    // 3. Factory Provider (Async initialization)
    {
      provide: 'ASYNC_CONNECTION',
      useFactory: async () => {
        const connection = await Promise.resolve({ connected: true, dbName: 'prod_db' });
        return connection;
      },
    },

    // 4. Existing Provider (Alias)
    {
      provide: 'IUserService',
      useExisting: UserService,
    },
  ],
})
export class UserModule {}

```

#### Output

```text
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [InstanceLoader] UserModule dependencies initialized +4ms
App Config: { apiVersion: 'v2', environment: 'production' }
Async Connection Status: { connected: true, dbName: 'prod_db' }

```

---

### Question 3. How do guards, interceptors, and pipes differ in NestJS, and what is their execution order?

#

NestJS provides specialized middleware building blocks that run in a strict request lifecycle:

* **Guards (`CanActivate`):** Determine if a request should be processed based on permissions, authorization tokens, or roles.
* **Interceptors (`NestInterceptor`):** Bind extra logic before and after method execution, transform returning payloads, cache results, or modify exceptions using RxJS streams.
* **Pipes (`PipeTransform`):** Validate and transform incoming request payloads (query, params, body) before reaching the controller handler.

**Execution Lifecycle Order:**

`Incoming Request` → `Global / Route Middleware` → `Guards` → `Interceptors (Before)` → `Pipes` → `Route Handler` → `Interceptors (After)` → `Exception Filters` → `Outgoing Response`.

#### Workflow / Architecture

```text
[ Client Request ]
       │
       ▼
 [ Middleware ] ──► [ Guards ] ──► [ Interceptors (Before) ] ──► [ Pipes ]
                                                                     │
 [ Client Response ] ◄── [ Exception Filters ] ◄── [ Interceptors (After) ] ◄── [ Route Handler ]

```

#### Example

```typescript
import { 
  Injectable, CanActivate, ExecutionContext, 
  NestInterceptor, CallHandler, PipeTransform, BadRequestException 
} from '@nestjs/common';
import { Observable, tap } from 'rxjs';

// 1. Guard
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    console.log('[1. Guard] Authenticating request...');
    return true; // Request allowed
  }
}

// 2. Interceptor
@Injectable()
export class BenchmarkInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('[2. Interceptor] BEFORE route handler execution');
    const start = Date.now();
    return next.handle().pipe(
      tap(() => console.log(`[5. Interceptor] AFTER handler execution: ${Date.now() - start}ms`))
    );
  }
}

// 3. Pipe
@Injectable()
export class ParseIntCustomPipe implements PipeTransform<string, number> {
  transform(value: string): number {
    console.log('[3. Pipe] Transforming payload parameter...');
    const val = parseInt(value, 10);
    if (isNaN(val)) throw new BadRequestException('Validation failed');
    return val;
  }
}

```

#### Output

```text
[1. Guard] Authenticating request...
[2. Interceptor] BEFORE route handler execution
[3. Pipe] Transforming payload parameter...
[4. Route Handler] Executing GET /users/42 logic...
[5. Interceptor] AFTER handler execution: 3ms

```

---

## SECTION 2: Advanced NestJS Concepts

### Question 4. What are dynamic modules in NestJS and how do they enable configuration?

#

Dynamic Modules allow NestJS modules to be dynamically configured at runtime by returning a `DynamicModule` object from static methods (conventionally named `forRoot()`, `forFeature()`, or `forRootAsync()`).

This pattern permits reusable modules (e.g., database clients, mailers, authentication services) to receive configuration parameters dynamically during importing in feature modules or root applications.

#### Example

```typescript
import { DynamicModule, Module, Provider } from '@nestjs/common';

export interface DatabaseOptions {
  host: string;
  port: number;
}

@Module({})
export class DynamicDatabaseModule {
  static forRoot(options: DatabaseOptions): DynamicModule {
    const dbProvider: Provider = {
      provide: 'DB_CONNECTION',
      useValue: `Connected to ${options.host}:${options.port}`,
    };

    return {
      module: DynamicDatabaseModule,
      providers: [dbProvider],
      exports: [dbProvider],
    };
  }
}

```

#### Output

```text
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [InstanceLoader] DynamicDatabaseModule initialized +3ms
Database Injection Token Resolved: "Connected to localhost:5432"

```

---

### Question 5. Explain custom decorators in NestJS and provide a use case.

#

Custom decorators extract parameter extraction logic, custom route annotations, or custom metadata bindings into reusable functions. Parameter decorators use `createParamDecorator()` to access the `ExecutionContext` and retrieve parameters directly from the request object (e.g., extracting the authenticated user).

#### Example

```typescript
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

// Custom Parameter Decorator
export const CurrentUser = createParamDecorator(
  (data: string | undefined, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user || { id: 'usr_99', email: 'dev@nestjs.com', role: 'admin' };
    
    return data ? user?.[data] : user;
  },
);

// Usage in Controller
import { Controller, Get } from '@nestjs/common';

@Controller('profile')
export class ProfileController {
  @Get()
  getProfile(@CurrentUser('email') userEmail: string) {
    return { email: userEmail };
  }
}

```

#### Output

```text
HTTP GET /profile
Response Body:
{
  "email": "dev@nestjs.com"
}

```

---

### Question 6. How does NestJS implement circular dependencies and what are best practices to avoid them?

#

A circular dependency occurs when two services or modules directly depend on each other (`ServiceA → ServiceB → ServiceA`). NestJS resolves circular references using the `forwardRef()` utility function, which defers reference resolution until both tokens are registered.

However, circular dependencies indicate structural architectural coupling. Best practices to avoid circular dependencies include:

* Using event-driven architecture (`@nestjs/event-emitter`) for decoupled notification.
* Creating a shared module containing the shared model or service dependency.
* Applying Dependency Inversion by depending on abstract interfaces.

#### Example

```typescript
// service-a.ts
import { Injectable, Inject, forwardRef } from '@nestjs/common';
import { ServiceB } from './service-b';

@Injectable()
export class ServiceA {
  constructor(
    @Inject(forwardRef(() => ServiceB))
    private readonly serviceB: ServiceB,
  ) {}

  executeA() {
    return 'Service A output';
  }
}

// service-b.ts
import { Injectable, Inject, forwardRef } from '@nestjs/common';
import { ServiceA } from './service-a';

@Injectable()
export class ServiceB {
  constructor(
    @Inject(forwardRef(() => ServiceA))
    private readonly serviceA: ServiceA,
  ) {}

  executeB() {
    return 'Service B calls: ' + this.serviceA.executeA();
  }
}

```

#### Output

```text
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [InstanceLoader] ServiceA & ServiceB resolved with forwardRef +2ms
Execution Result: "Service B calls: Service A output"

```

---

### Question 7. Describe the difference between `@Module()` scope types: DEFAULT, REQUEST, and TRANSIENT.

#

Providers in NestJS have specific lifetime scopes:

* **`DEFAULT` (Singleton):** A single instance is instantiated upon startup and shared across the entire application lifetime. It offers optimal memory usage and performance.
* **`REQUEST` (Per-Request):** A new provider instance is created exclusively for each incoming request and garbage collected when the request finishes. Use when holding request-specific context (e.g., tenant ID, auth header), but note the memory overhead per request.
* **`TRANSIENT` (Per-Consumer):** A dedicated provider instance is created for every class that injects it. It ensures isolated instance state across consuming services.

#### Scope Properties Comparison

* **DEFAULT Scope:**
* Lifecycle: Application lifetime
* Primary Use Case: Stateless logic, utility services, singleton repositories
* Performance Impact: Lowest (single instance created once)
* **REQUEST Scope:**
* Lifecycle: Per HTTP/Microservice request lifecycle
* Primary Use Case: Multi-tenant request isolation, request header context
* Performance Impact: Moderate (instantiated on every incoming request)
* **TRANSIENT Scope:**
* Lifecycle: Per injection point
* Primary Use Case: Stateful isolated helper objects
* Performance Impact: Highest (multiple unique instances instantiated)

#### Example

```typescript
import { Injectable, Scope } from '@nestjs/common';

// 1. DEFAULT Scope (Singleton)
@Injectable({ scope: Scope.DEFAULT })
export class SingletonService {
  public id = Math.random();
}

// 2. REQUEST Scope (New instance per incoming HTTP request)
@Injectable({ scope: Scope.REQUEST })
export class RequestScopedService {
  public id = Math.random();
}

// 3. TRANSIENT Scope (New instance per injection point)
@Injectable({ scope: Scope.TRANSIENT })
export class TransientService {
  public id = Math.random();
}

```

#### Output

```text
[HTTP Request 1]
Singleton Instance ID: 0.142857
RequestScoped Instance ID: 0.884123
Transient Instance ID: 0.449102

[HTTP Request 2]
Singleton Instance ID: 0.142857 (Unchanged)
RequestScoped Instance ID: 0.339120 (New Instance)
Transient Instance ID: 0.912847 (New Instance)

```

---

## SECTION 3: Performance & Optimization

### Question 8. What strategies would you use to optimize a NestJS application's performance?

#

Optimizing NestJS applications involves reducing runtime execution overhead and streamlining resource utilization:

1. **Switch Underlying HTTP Adapter:** Replace the default Express engine with `@nestjs/platform-fastify` for significantly higher throughput.
2. **Caching:** Implement cache stores via `@nestjs/cache-manager` and Redis for read-heavy operations.
3. **Database Query Optimization:** Utilize pagination, lean query execution, explicit indexing, and prevent N+1 queries.
4. **Compression & Rate Limiting:** Compress payloads using `compression` and safeguard endpoints using `@nestjs/throttler`.
5. **Asynchronous Processing:** Offload heavy computations to background queues (e.g., BullMQ with Redis).

#### Example

```typescript
// main.ts - Fastify Adapter & Security Optimization Setup
import { NestFactory } from '@nestjs/core';
import { FastifyAdapter, NestFastifyApplication } from '@nestjs/platform-fastify';
import { AppModule } from './app.module';
import compression from '@fastify/compress';

async function bootstrap() {
  // Using Fastify for high performance
  const app = await NestFactory.create<NestFastifyApplication>(
    AppModule,
    new FastifyAdapter({ logger: true })
  );

  // Enable fastify response payload compression
  await app.register(compression, { encodings: ['gzip', 'deflate'] });

  await app.listen(3000, '0.0.0.0');
}
bootstrap();

```

#### Output

```text
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [NestApplication] Nest application successfully started using Fastify HTTP Server
Throughput Benchmark: ~32,000 req/sec (Fastify) vs ~12,000 req/sec (Express standard)

```

---

### Question 9. How do you implement caching in NestJS and what are the different caching strategies?

#

NestJS provides caching abstraction via `@nestjs/cache-manager`. Primary caching strategies include:

* **Time-To-Live (TTL):** Automatic expiration of cached keys after a set duration.
* **Cache-Aside (Lazy Loading):** Service queries the cache first; if missing, fetches from database and writes result to cache.
* **Write-Through / Write-Around:** Updating or invalidating cache keys immediately when mutating records.

#### Example

```typescript
// app.module.ts (NestJS 10 Cache Setup)
import { Module } from '@nestjs/common';
import { CacheModule, CacheInterceptor } from '@nestjs/cache-manager';
import { APP_INTERCEPTOR } from '@nestjs/core';
import { AppController } from './app.controller';

@Module({
  imports: [
    CacheModule.register({
      ttl: 10000, // 10 seconds TTL
      max: 100, // Maximum items in cache
      isGlobal: true,
    }),
  ],
  controllers: [AppController],
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: CacheInterceptor, // Automatic response caching
    },
  ],
})
export class AppModule {}

```

#### Output

```text
Request 1 (Cache Miss): GET /products -> DB execution time: 145ms
Request 2 (Cache Hit):  GET /products -> Response time: 2ms (Served from Cache)

```

---

## SECTION 4: Architecture & State Management

### Question 10. How would you structure a large-scale NestJS application with multiple domains?

#

Large-scale applications follow a **Modular Monolith** or **Domain-Driven Design (DDD)** folder structure:

* **Feature Modules:** Self-contained domains (`UserModule`, `OrderModule`, `PaymentModule`).
* **Core Module:** Application-wide singletons initialized once (`DatabaseModule`, `LoggerModule`).
* **Shared Module:** Reusable utilities, custom pipes, and global decorators.
* **Layered Boundaries:** Controller → Service → Repository pattern per domain.

#### Workflow / Architecture

```text
                  ┌────────────────────────┐
                  │       AppModule        │
                  └───────────┬────────────┘
         ┌────────────────────┼────────────────────┐
         ▼                    ▼                    ▼
  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
  │ CoreModule  │      │ SharedModule│      │ FeatureMod  │
  │ (Auth, DB)  │      │(Pipes,Utils)│      │(Orders/User)│
  └─────────────┘      └─────────────┘      ──────┬──────┘
                                                  │
                                    ┌─────────────┴─────────────┐
                                    ▼                           ▼
                              [ Controller ]              [ Controller ]
                                    │                           │
                              [ Service ]                 [ Service ]
                                    │                           │
                              [ Repository ]              [ Repository ]

```

#### Example

```text
src/
├── core/
│   ├── database/
│   └── logger/
├── shared/
│   ├── interceptors/
│   └── pipes/
├── modules/
│   ├── users/
│   │   ├── dto/
│   │   ├── users.controller.ts
│   │   ├── users.service.ts
│   │   └── users.module.ts
│   └── orders/
│       ├── orders.controller.ts
│       ├── orders.service.ts
│       └── orders.module.ts
└── app.module.ts

```

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { CoreModule } from './core/core.module';
import { SharedModule } from './shared/shared.module';
import { UsersModule } from './modules/users/users.module';
import { OrdersModule } from './modules/orders/orders.module';

@Module({
  imports: [CoreModule, SharedModule, UsersModule, OrdersModule],
})
export class AppModule {}

```

#### Output

```text
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [RoutesResolver] UsersController {/users}: +2ms
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [RoutesResolver] OrdersController {/orders}: +1ms

```

---

### Question 11. Explain the repository pattern in NestJS and its benefits with TypeORM or Prisma.

#

The Repository Pattern abstracts database persistence operations behind an interface. In NestJS, business services consume repositories rather than running raw database queries directly.

Benefits include decoupling business logic from ORM dependencies, simplifying unit testing via mock repositories, and centralizing data retrieval logic.

#### Example

```typescript
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { UserEntity } from './user.entity';

@Injectable()
export class UserRepositoryService {
  constructor(
    @InjectRepository(UserEntity)
    private readonly repo: Repository<UserEntity>,
  ) {}

  async findActiveUsers(): Promise<UserEntity[]> {
    return this.repo.find({ where: { isActive: true } });
  }

  async createUser(userData: Partial<UserEntity>): Promise<UserEntity> {
    const user = this.repo.create(userData);
    return this.repo.save(user);
  }
}

```

#### Output

```text
Repository query executed: SELECT * FROM "user_entity" WHERE "isActive" = true
Active users count returned: 5

```

---

### Question 12. How do you implement CQRS (Command Query Responsibility Segregation) in NestJS?

#

Command Query Responsibility Segregation (CQRS) separates read operations (Queries) from write operations (Commands). NestJS provides the official `@nestjs/cqrs` package, exposing Command Busses, Query Busses, Command Handlers, Query Handlers, and Event Handlers.

#### Workflow / Architecture

```text
HTTP Request ──► [ Controller ]
                     │
         ┌───────────┴───────────┐
         ▼                       ▼
  [ Command Bus ]         [ Query Bus ]
         │                       │
         ▼                       ▼
 [ Command Handler ]     [ Query Handler ]
         │                       │
   (Write DB / Event)      (Read DB / Cache)

```

#### Example

```typescript
import { Module } from '@nestjs/common';
import { CqrsModule, CommandHandler, ICommandHandler, QueryHandler, IQueryHandler } from '@nestjs/cqrs';

// 1. Command Definition & Handler
export class CreateOrderCommand {
  constructor(public readonly item: string, public readonly price: number) {}
}

@CommandHandler(CreateOrderCommand)
export class CreateOrderHandler implements ICommandHandler<CreateOrderCommand> {
  async execute(command: CreateOrderCommand): Promise<string> {
    // Write side logic
    return `Order created for item: ${command.item}`;
  }
}

// 2. Query Definition & Handler
export class GetOrderQuery {
  constructor(public readonly orderId: string) {}
}

@QueryHandler(GetOrderQuery)
export class GetOrderHandler implements IQueryHandler<GetOrderQuery> {
  async execute(query: GetOrderQuery): Promise<any> {
    // Read side logic
    return { orderId: query.orderId, status: 'DELIVERED' };
  }
}

@Module({
  imports: [CqrsModule],
  providers: [CreateOrderHandler, GetOrderHandler],
})
export class OrderCqrsModule {}

```

#### Output

```text
[CommandBus] Executed CreateOrderCommand -> "Order created for item: Laptop"
[QueryBus] Executed GetOrderQuery -> { orderId: 'ord_123', status: 'DELIVERED' }

```

---

## SECTION 5: Testing & Quality Assurance

### Question 13. What testing strategies do you use for NestJS applications and how do you mock dependencies?

#

Testing in NestJS uses `@nestjs/testing` (built on top of Jest):

* **Unit Testing:** Tests services/controllers in isolation. Dependencies are mocked using `Test.createTestingModule()` with `useValue` or `useFactory`.
* **Integration & E2E Testing:** Tests full HTTP lifecycle execution using `supertest` against an instantiated `INestApplication`.

#### Example

##### 1. Unit Test with Mock Service

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { UserService } from './user.service';

describe('UserService', () => {
  let service: UserService;

  const mockRepository = {
    find: jest.fn().mockResolvedValue([{ id: '1', name: 'John Doe' }]),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UserService,
        { provide: 'USER_REPOSITORY', useValue: mockRepository },
      ],
    }).compile();

    service = module.get<UserService>(UserService);
  });

  it('should return users list', async () => {
    const users = await service.getUsers();
    expect(users).toEqual([{ id: '1', name: 'John Doe' }]);
    expect(mockRepository.find).toHaveBeenCalled();
  });
});

```

##### 2. End-to-End (E2E) Test using Supertest

```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import request from 'supertest';
import { AppModule } from '../src/app.module';

describe('AppController (E2E)', () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  it('/health (GET)', () => {
    return request(app.getHttpServer())
      .get('/health')
      .expect(200)
      .expect({ status: 'ok' });
  });

  afterAll(async () => {
    await app.close();
  });
});

```

#### Output

```text
 PASS  src/users/users.service.spec.ts
  UserService
    ✓ should return users list (12 ms)

 PASS  test/app.e2e-spec.ts
  AppController (E2E)
    ✓ /health (GET) (45 ms)

Test Suites: 2 passed, 2 total

```

---

## SECTION 6: Diagnostics & Security

### Question 14. A NestJS API endpoint is experiencing high latency under load. How would you diagnose and resolve this?

#

Systematic approach for diagnosing and resolving API latency issues:

1. **APM & Profiling:** Identify bottleneck endpoints using DataDog/New Relic or Node.js inspect CPU profiling (`clinic.js bubbleprof`).
2. **Database Query Analysis:** Log slow queries, eliminate N+1 queries, verify indexes, and utilize lean data projection.
3. **Caching Layer:** Add `@nestjs/cache-manager` with Redis for expensive read operations.
4. **Asynchronous Execution:** Offload long-running computations to background queue processes (`BullMQ`).

#### Example

```typescript
// Optimizing high latency controller with caching and pagination
import { Controller, Get, Query, UseInterceptors } from '@nestjs/common';
import { CacheInterceptor, CacheTTL } from '@nestjs/cache-manager';
import { UserService } from './user.service';

@Controller('users')
export class UserOptimizedController {
  constructor(private readonly userService: UserService) {}

  @Get('fast-list')
  @UseInterceptors(CacheInterceptor)
  @CacheTTL(30000) // Cache response for 30 seconds
  async getUsers(
    @Query('page') page = 1,
    @Query('limit') limit = 20,
  ) {
    // Paginated DB call avoids loading huge data arrays in memory
    return this.userService.findPaginated(page, limit);
  }
}

```

#### Output

```text
[Diagnostic Metric] Latency Before Optimization: 1850ms (Full table scan without index)
[Diagnostic Metric] Latency After Indexing & Pagination: 85ms
[Diagnostic Metric] Latency After Redis Cache Hit: 3ms

```

---

### Question 15. What security best practices should be implemented in a production NestJS application?

#

Essential security measures for production deployment:

* **HTTP Headers:** Secure HTTP headers using `helmet`.
* **CORS:** Restrict origin domains via CORS setup.
* **Input Validation:** Use global `ValidationPipe` with `class-validator` (`whitelist: true`, `forbidNonWhitelisted: true`) to strip unexpected parameters.
* **Rate Limiting:** Protect endpoints against brute-force attacks using `@nestjs/throttler`.
* **Secrets Validation:** Use `@nestjs/config` with schema validation (e.g., Joi/Zod).

#### Example

```typescript
// main.ts Security Configuration
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import helmet from 'helmet';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // 1. Helmet headers
  app.use(helmet());

  // 2. Strict CORS
  app.enableCors({
    origin: ['https://myapp.com'],
    methods: 'GET,HEAD,PUT,PATCH,POST,DELETE',
    credentials: true,
  });

  // 3. Validation Pipe (Sanitize incoming data)
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true, // Strips non-whitelisted properties
      forbidNonWhitelisted: true, // Rejects requests with unexpected fields
      transform: true,
    }),
  );

  await app.listen(3000);
}
bootstrap();

```

#### Output

```text
Request Payload: { "name": "John", "maliciousInject": "<script></script>" }
Response Status: 400 Bad Request
Response Body:
{
  "statusCode": 400,
  "message": ["property maliciousInject should not exist"],
  "error": "Bad Request"
}

```

---

### Question 16. Describe a situation where you had to refactor a legacy Node.js application to NestJS. What challenges did you face?

#

Refactoring legacy Express applications to NestJS is achieved using the **Strangler Fig Pattern**, gradually wrapping legacy routes inside NestJS until migration is complete.

Key challenges include:

* Transitioning unstructured Express middleware to NestJS Guards, Interceptors, and Pipes.
* Migrating dynamic objects attached to `req` (e.g., `req.user`) into strongly typed NestJS decorators.
* Maintaining functionality and API contract backward compatibility through E2E regression suites.

#### Example

```typescript
// NestJS main.ts wrapping legacy Express Router
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import express from 'express';

// Legacy Express router
const legacyRouter = express.Router();
legacyRouter.get('/legacy-api', (req, res) => {
  res.json({ legacy: true, message: 'Served from Express legacy route' });
});

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Mount legacy Express router under NestJS HTTP server
  app.use('/v1', legacyRouter);

  await app.listen(3000);
}
bootstrap();

```

#### Output

```text
HTTP GET /v1/legacy-api -> Response: { legacy: true, message: "Served from Express legacy route" }
HTTP GET /v2/users      -> Response: { legacy: false, data: [...] } (Served by NestJS UsersController)

```

---

## SECTION 7: Code Reviews & Framework Choices

### Question 17. How do you approach code reviews for NestJS applications? What do you look for?

#

NestJS Code Review Verification Criteria:

* **Architecture:** Ensure strict separation of concerns (Controllers handle requests, Services manage business logic, Repositories handle data persistence).
* **Dependency Injection:** Verify providers are injected rather than manually instantiated via `new Service()`.
* **Type Safety:** Avoid explicit `any` types; verify custom DTOs and interfaces.
* **Validation & Security:** Confirm route parameters are validated using `class-validator` DTOs and authorized via Guards.
* **Error Handling:** Verify usage of built-in NestJS exceptions (`NotFoundException`, `ForbiddenException`) instead of generic errors.

#### Example

##### Anti-Pattern (Rejected in Code Review)

```typescript
@Controller('users')
export class BadUserController {
  @Post()
  async createUser(@Req() req: any) {
    // Bad: Manual class instantiation instead of DI
    const userService = new UserService(); 
    
    // Bad: Loose typing, unvalidated input
    if (!req.body.email) {
      throw new Error('Email missing'); // Bad: Generic Error
    }
    return userService.save(req.body);
  }
}

```

##### Approved NestJS Pattern (Passed Code Review)

```typescript
@Controller('users')
export class GoodUserController {
  // Good: Properly injected dependency
  constructor(private readonly userService: UserService) {}

  @Post()
  // Good: Strongly typed DTO validated by global ValidationPipe
  async createUser(@Body() createUserDto: CreateUserDto) {
    return this.userService.save(createUserDto);
  }
}

```

#### Output

```text
Linter Check: PASSED
Type Check: 0 Errors
Unit Tests Coverage: 100% (User Controller & Service)

```

---

### Question 18. When would you choose NestJS over Express, Fastify, or other Node.js frameworks?

#

Framework Selection Criteria:

* **NestJS:** Enterprise-scale applications, multi-developer teams, microservice networks, and systems requiring strict architectural consistency, modularity, built-in dependency injection, and automated testability out of the box.
* **Express:** Simple micro-APIs, quick hackathons, or applications where minimal abstractions and structural freedom are desired.
* **Fastify:** Ultra-high throughput APIs where performance and schema serialization speed are the primary constraints.

#### Framework Comparison Breakdown

* **NestJS:**
* Target Use Case: Enterprise applications, microservices, complex backends
* Learning Curve: Steep (Requires TypeScript, DI, Decorator comprehension)
* Performance: High (Configurable to run on Fastify engine)
* Architectural Style: Highly opinionated, modular design
* **Express:**
* Target Use Case: Lightweight APIs, quick prototypes
* Learning Curve: Low (Minimal core concepts)
* Performance: Standard
* Architectural Style: Unopinionated, custom structure
* **Fastify:**
* Target Use Case: High-throughput microservices, low-latency APIs
* Learning Curve: Medium (Schema-driven validation)
* Performance: Fast (High throughput)
* Architectural Style: Semi-opinionated plugin architecture
* **Koa:**
* Target Use Case: Modern lightweight middleware applications
* Learning Curve: Low (Async/await focused)
* Performance: Good
* Architectural Style: Minimalist middleware chain

#### Example

##### Standard Express Setup

```typescript
const express = require('express');
const app = express();
app.get('/ping', (req, res) => res.send('pong'));
app.listen(3000);

```

##### Structured NestJS Setup

```typescript
import { Controller, Get, Module } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';

@Controller()
class PingController {
  @Get('ping')
  ping() { return 'pong'; }
}

@Module({ controllers: [PingController] })
class AppModule {}

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();

```

#### Output

```text
NestJS initialized: Structured modules, dependency container ready, OpenAPI documentation ready.

```

---

## SECTION 8: Microservices & Real-Time Distributed Systems

### Question 19. How do you implement microservices communication in NestJS?

#

NestJS supports distributed microservices architecture via `@nestjs/microservices`. Supported transport layers include TCP, Redis, NATS, RabbitMQ, Kafka, and gRPC.

Communication patterns:

* **Request-Response (`@MessagePattern`):** Synchronous request execution returning payload responses.
* **Event-Driven (`@EventPattern`):** Asynchronous publish-subscribe notification without waiting for responses.

#### Workflow / Architecture

```text
[ Client Gateway ] ──► (ClientProxy.send) ──► [ Transport Layer (RabbitMQ / gRPC) ]
                                                            │
                                                            ▼
                                                 [ Microservice Worker ]
                                                 (@MessagePattern)

```

#### Example

```typescript
// 1. Microservice Server Application (main.ts)
import { NestFactory } from '@nestjs/core';
import { Transport, MicroserviceOptions } from '@nestjs/microservices';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(AppModule, {
    transport: Transport.TCP,
    options: { host: '127.0.0.1', port: 8877 },
  });
  await app.listen();
}
bootstrap();

// 2. Microservice Controller Handler
import { Controller } from '@nestjs/common';
import { MessagePattern, EventPattern } from '@nestjs/microservices';

@Controller()
export class MathMicroserviceController {
  @MessagePattern({ cmd: 'sum' })
  accumulate(data: number[]): number {
    return (data || []).reduce((a, b) => a + b, 0);
  }

  @EventPattern('user_created')
  handleUserCreated(data: Record<string, unknown>) {
    console.log('Async Event Triggered:', data);
  }
}

```

#### Output

```text
[Nest] 12404 - 08/24/2026, 11:41:14 PM   LOG [NestMicroservice] Nest microservice successfully started via TCP
[ClientProxy] Requesting { cmd: 'sum' } with payload [1, 2, 3, 4] -> Response: 10

```

---

### Question 20. Explain how to implement GraphQL subscriptions in NestJS and what challenges they present.

#

GraphQL subscriptions provide bi-directional real-time updates over WebSockets using `@nestjs/graphql` and `@Subscription()`.

Real-world deployment challenges:

* **State & Multi-Instance Scaling:** WebSockets maintain state. Scaling across multiple instances requires a centralized PubSub system (e.g., Redis PubSub).
* **Authentication Context:** Standard HTTP auth headers are missing during WebSocket connections; auth must be extracted during the initial connection handshake.
* **Connection Lifecycle:** Managing connection drops, heartbeats, and avoiding memory leaks.

#### Example

```typescript
import { Resolver, Subscription, Query } from '@nestjs/graphql';
import { PubSub } from 'graphql-subscriptions';

const pubSub = new PubSub();

@Resolver()
export class NotificationResolver {
  @Query(() => String)
  hello() { return 'Hello World'; }

  // Publish Event
  async triggerNotification(message: string) {
    await pubSub.publish('commentAdded', { commentAdded: message });
  }

  // GraphQL Subscription
  @Subscription(() => String, {
    resolve: (payload) => payload.commentAdded,
  })
  commentAdded() {
    return pubSub.asyncIterableIterator('commentAdded');
  }
}

```

#### Output

```text
[WebSocket Client Subscribed] wss://api.example.com/graphql
PubSub Event Fired: 'commentAdded' ("New comment posted!")
Client WebSocket Payload Received: { "data": { "commentAdded": "New comment posted!" } }

```

---

## Senior NestJS Interview Cheatsheet & Summary Points

### Core Framework Lifecycle Order

* Request Lifecycle Flow: `Middleware` → `Guards` → `Interceptors (Before)` → `Pipes` → `Route Handler` → `Interceptors (After)` → `Exception Filters`.
* Application Startup Flow: `OnModuleInit` → `OnApplicationBootstrap`.
* Application Shutdown Flow: `OnModuleDestroy` → `beforeApplicationShutdown` → `onApplicationShutdown`.

### Provider Lifetime Scopes

* `DEFAULT`: Application-wide singleton instance. Recommended for performance.
* `REQUEST`: New instance per request. Use for tenant/request headers isolation. Note performance cost.
* `TRANSIENT`: Unique instance per consumer injection point. Use for isolated state containers.

### High-Frequency Architectural Decision Points

* **Performance:** Swap default Express HTTP adapter with `@nestjs/platform-fastify`.
* **Microservices:** Use gRPC for low-latency internal RPC; use RabbitMQ or Kafka for event streaming and reliable queuing.
* **State & Scaling:** Use Redis PubSub for scaling GraphQL subscriptions and WebSockets across multiple cluster nodes.
* **CQRS:** Use `@nestjs/cqrs` when separating high-frequency read and complex write operations.
* **Security:** Use `helmet`, `@nestjs/throttler`, global `ValidationPipe` with `whitelist: true`, and strictly type DTO payloads.