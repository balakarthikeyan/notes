# NestJS Master Technical Study Guide

## 🛠 Step-by-Step Guide to Running NestJS

### 1. **Install Prerequisites**
- **Node.js & npm**: NestJS 10+ requires Node.js v18 or v20+ LTS as the modern standard platform, while legacy versions (v8/v9) installations required Node.js v16.
- Verify local installation:
```bash
  node -v
  npm -v
```

---

### 2. **Install Nest CLI**

#### Legacy & Modern Installation Command

* Install the NestJS Command Line Interface globally via npm:
```bash
npm install -g @nestjs/cli
```

* Confirm successful installation:
```bash
nest --version
```

---

### 3. **Create a New Project**

#### Legacy Approach (Standard NPM / Yarn CLI prompts)

* Use the CLI to scaffold a new project workspace:
```bash
nest new my-project
```

* Choose your preferred package manager (**npm** or **yarn**) when prompted.
* Navigate into the root of your newly created project:
```bash
cd my-project
```

#### Modern Approach (Explicit Package Manager & Monorepo/Pnpm/Bun Support)

* Scaffold a project explicitly designating modern package managers such as `pnpm` or `bun`:
```bash
# Using PNPM
nest new my-project --package-manager pnpm

# Using Bun
nest new my-project --package-manager bun
```

---

### 4. **Run the Application & Tests**

#### Application Modes

##### Standard / Legacy NPM Scripts Execution

Start the server using configured scripts in `package.json`:

```bash
# Development mode
npm run start

# Run the application with active Watch mode (automatic hot reload on file changes) hot reload enabled
npm run start:dev

# Production mode (runs compiled JS from the dist folder)
npm run start:prod

# Compile the TypeScript code into production-ready JavaScript inside the `/dist` directory
npm run build
```

##### Modern High-Performance SWC Watch Mode

```bash
# High-speed compilation using SWC builder in watch mode
npm run start:dev -- --b swc
```

#### Test Execution

Execute the pre-configured Jest testing suites:

```bash
# Run unit tests
npm run test

# Run end-to-end (e2e) tests
npm run test:e2e

# Run test coverage analysis
npm run test:cov
```

* Once started, your application will run on **http://localhost:3000** by default.

---

### 5. **Project Structure Overview**

* **`src/main.ts`** → The entry point of the application. Uses `NestFactory` to bootstrap the application instance.
* **`app.module.ts`** → The root module that ties together controllers, providers, and imports.
* **`app.controller.ts`** → The routing layer responsible for intercepting incoming HTTP requests and returning responses.
* **`app.service.ts`** → The business logic layer containing methods invoked by controllers.

---

## ⚡ Common Commands

| Command | Purpose |
| --- | --- |
| `nest new <name>` | Create and initialize a new NestJS project framework |
| `nest generate <schematic>` | Generate code elements (e.g., `co` for controller, `s` for service, `mo` for module) |
| `npm run start:dev` | Run the application with active watch-mode hot reload enabled |
| `npm run build` | Compile the TypeScript code into production-ready JavaScript inside the `/dist` directory |

---

### 1. High-Performance Compilation via SWC Builder

NestJS relied purely on `ts-loader` via Webpack, which can become slow during massive enterprise builds. Modern NestJS features built-in support for **SWC (Speedy Web Compiler)**, offering significantly faster startup and compilation benchmarks.

#### Legacy Compiler Setup (Webpack / TSC Default)

Default `nest-cli.json` configuration relying on standard `tsc`:

```json
{
  "$schema": "https://json.nestland.com/cli/schema.json",
  "collection": "@nestjs/schematics",
  "sourceRoot": "src",
  "compilerOptions": {
    "deleteOutDir": true
  }
}
```

#### Modern SWC Compiler Setup

* **Installation**:
```bash
npm i --save-dev @swc/cli @swc/core

```

* **CLI Execution**: Pass the `--b swc` flag or configure it within your `nest-cli.json`:
```bash
npm run start:dev -- --b swc
```

* **Updated `nest-cli.json**`:
```json
{
  "$schema": "https://json.nestland.com/cli/schema.json",
  "collection": "@nestjs/schematics",
  "sourceRoot": "src",
  "compilerOptions": {
    "builder": "swc",
    "typeCheck": true,
    "deleteOutDir": true
  }
}
```

---

### 2. Strict Type-Safety & Runtime Request Validation

Production environments must sanitize and guarantee the shape of incoming network payloads. This is accomplished using Data Transfer Objects (DTOs) alongside `class-validator` and `class-transformer`.

* **Installation**:
```bash
npm i --save class-validator class-transformer
```

* **Global Pipe Activation** (`src/main.ts`):
```typescript
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,            // Strips out properties not explicitly declared in the DTO
      forbidNonWhitelisted: true, // Rejects requests containing properties not defined in the DTO
      transform: true,            // Automatically type-casts payloads to their declared DTO instance types
    }),
  );

  await app.listen(3000);
}
bootstrap();
```

* **DTO Implementation Example** (`src/users/dto/create-user.dto.ts`):
```typescript
import { IsString, IsEmail, MinLength } from 'class-validator';

export class CreateUserDto {
  @IsString()
  readonly name: string;

  @IsEmail()
  readonly email: string;

  @IsString()
  @MinLength(8, { message: 'Password must be at least 8 characters long.' })
  readonly password: string;
}
```

---

### 3. The NestJS Request/Response Lifecycle Flow

To build predictable interceptors, guards, and custom filters, you must understand the exact chronological execution order of an incoming request:

```
Incoming Request ──> Middleware ──> Guards ──> Interceptors (Pre-handler) ──> Pipes ──> Controller Handler ──> Service (Business Logic) ──> Interceptors (Post-handler) ──> Exception Filters (if error) ──> Outgoing Response
```

---

### 4. Enterprise Configuration Architecture (`@nestjs/config`)

Never hardcode environment strings or database secrets into your application source. Use the standard config package built over `dotenv` with programmatic typing support.

* **Installation**:
```bash
npm i --save @nestjs/config

```


* **Global Implementation** (`src/app.module.ts`):
```typescript
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true, // Makes configuration accessible across all feature modules without re-importing
      envFilePath: '.env',
    }),
  ],
})
export class AppModule {}
```

---

### 5. Production Database Layers (Prisma ORM Integration)

While TypeORM remains widely documented, Prisma has become a premier ecosystem choice for modern NestJS applications due to its strict, automated database-to-TypeScript type safety.

* **Setup Workflow**:
```bash
npm i prisma --save-dev
npx prisma init
npm i @prisma/client
```

* **Abstracting the Database Provider Service** (`src/prisma/prisma.service.ts`):

#### Legacy Lifecycle Pattern (Prisma v4 & NestJS Lifecycle)

```typescript
import { Injectable, OnModuleInit, OnModuleDestroy, INestApplication } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit, OnModuleDestroy {
  async onModuleInit() {
    await this.$connect();
  }

  async onModuleDestroy() {
    await this.$disconnect();
  }

  // Legacy shutdown hook for Prisma v4 engine management
  async enableShutdownHooks(app: INestApplication) {
    process.on('beforeExit', async () => {
      await app.close();
    });
  }
}
```

1. CQRS (Command and Query Responsibility Segregation) Pattern

An architectural pattern that separates the responsibilities of:

`Commands:` actions that change state (create, update, delete).
`Queries:` actions that read data (fetch data).

**CQRS Benefits:**

* `Separation of concerns.` The model separates the read and write operations into separate models.
* `Scalability`. The read and write operations can be scaled independently.
* `Flexibility.` The model allows for the use of different data stores for read and write operations.
* `Performance.` The model allows for the use of different data stores optimized for read and write operations.

**Use-case:**

- Your app has complex business logic.
- Read and write operations have different scaling needs.
- You want to use event sourcing or asynchronous workflows.
- You’re working in a microservices architecture.

2. NestJS CQRS

Installation 

```bash
npm install --save @nestjs/cqrs
```
Import the CqrsModule.forRoot()

### UUID Generation Comparison (Legacy vs. Modern)

#### Legacy Method (Deprecated)

```javascript
// Legacy syntax using node-uuid
var uuid = require('node-uuid');

// Time-based UUID v1
var idV1 = uuid.v1();

// Random UUID v4
var idV4 = uuid.v4();
```

#### Modern Method (ESM/CommonJS)

```javascript
// CommonJS Syntax
const { v4: uuidv4 } = require('uuid');
const data = { id: uuidv4() };

// ES Module Syntax
import { v4 as uuidv4 } from 'uuid';
const data = { id: uuidv4() };

// Native Web Crypto API (Node.js 16.7.0+)
const crypto = require('crypto');
const nativeId = crypto.randomUUID();
```
---

#### Modern Standard Pattern (Prisma v5 / v6 & NestJS 10+)

```typescript
import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit, OnModuleDestroy {
  constructor() {
    super({
      log: process.env.NODE_ENV === 'development' ? ['query', 'info', 'warn', 'error'] : ['error'],
    });
  }

  async onModuleInit() {
    await this.$connect();
  }

  async onModuleDestroy() {
    await this.$disconnect();
  }
}
```

---

## Q1: How does NestJS Dependency Injection (DI) compare to Laravel's Service Container or Symfony's DependencyInjection Container, and how are custom providers instantiated?

**Answer:**
Both ecosystems implement Inversion of Control (IoC), but their mechanisms differ:

* **Laravel / Symfony**: Use runtime reflection (`ReflectionClass`) in PHP to dynamically resolve dependencies from type hints at execution time.
* **NestJS**: Uses TypeScript design-time type annotations emitted into JavaScript runtime metadata via `reflect-metadata`. Nest builds a topographically sorted Dependency Graph during application startup (`bootstrap`).

### Provider Types in NestJS:

1. **Class Providers (`useClass`)**: Swaps implementations (e.g., swapping `MockDatabaseService` for `RealDatabaseService`).
2. **Value Providers (`useValue`)**: Injects static objects, configuration blocks, or third-party libraries.
3. **Factory Providers (`useFactory`)**: Dynamically creates providers based on runtime logic and injected dependencies.
4. **Existing Providers (`useExisting`)**: Creates aliases for existing providers.

### Implementation Example:

```typescript
import { Module } from '@nestjs/common';
import { PaymentService } from './payment.service';
import { StripePaymentService } from './stripe-payment.service';
import { PaypalPaymentService } from './paypal-payment.service';

@Module({
  providers: [
    {
      provide: PaymentService,
      useFactory: (configService: ConfigService) => {
        const provider = configService.get<string>('PAYMENT_PROVIDER');
        return provider === 'stripe' ? new StripePaymentService() : new PaypalPaymentService();
      },
      inject: [ConfigService],
    },
  ],
  exports: [PaymentService],
})
export class PaymentModule {}
```

---

## Q2: What is the recommended architectural pattern for incrementally migrating a monolithic legacy PHP application (Laravel/Symfony) to NestJS microservices without application downtime?

**Answer:**
The **Strangler Fig Pattern** is used alongside a Reverse Proxy API Gateway (such as Nginx, Kong, or Traefik):

1. **Reverse Proxy Layer**: Route all incoming traffic through an API Gateway configured in front of the legacy PHP application.
2. **Domain Decoupling**: Identify bounded contexts (e.g., Auth, Notifications, Order Processing).
3. **Incremental Redirection**: Reimplement a single endpoint inside a new NestJS microservice. Update the gateway route rule to divert traffic for `/api/v2/orders` to NestJS while retaining `/api/v1/*` on PHP.
4. **Shared Session / Auth Strategy**: Decode shared JWT tokens or read session states stored in Redis across both PHP and Node.js instances.
5. **Event-Driven Synchronization**: Use message brokers (RabbitMQ or Apache Kafka) to emit state changes between systems asynchronously until PHP components are decommissioned.

---

## Q3: How do you architect NestJS as a Backend-For-Frontend (BFF) layer over a Headless WordPress installation using GraphQL or REST APIs?

**Answer:**
Using NestJS as a BFF middleware abstracts WordPress internals, enhances performance via caching, and enforces strict TypeScript typing for client applications (Next.js/React Native).

```
[ Frontend Client ] ──> [ NestJS BFF Layer (Auth, Redis Cache, Data Transformation) ] ──> [ WP GraphQL / Headless WP ]
```

### NestJS WordPress Integration Service Example:

```typescript
import { Injectable, BadGatewayException } from '@nestjs/common';
import { HttpService } from '@nestjs/axios';
import { firstValueFrom } from 'rxjs';

@Injectable()
export class WordPressService {
  constructor(private readonly httpService: HttpService) {}

  async FetchPosts(page: number = 1): Promise<any[]> {
    try {
      const wpUrl = process.env.WORDPRESS_GRAPHQL_ENDPOINT;
      const query = `
        query GetPosts($first: Int) {
          posts(first: $first) {
            nodes {
              id
              title
              slug
              content
            }
          }
        }
      `;

      const response = await firstValueFrom(
        this.httpService.post(wpUrl, {
          query,
          variables: { first: page * 10 },
        })
      );

      return response.data.data.posts.nodes;
    } catch (error) {
      throw new BadGatewayException('Unable to communicate with Headless WordPress backend.');
    }
  }
}

```

---

## Q4: How should a high-throughput WooCommerce webhook ingestion engine be built in NestJS to prevent server starvation during sudden spike events?

**Answer:**
Directly handling heavy business operations inside webhook HTTP handlers risks timeout failures and server overload. The solution is an **Asynchronous Queue Pipeline** using BullMQ backed by Redis (`@nestjs/bullmq`).

```typescript
// webhook.controller.ts
import { Controller, Post, Body, HttpCode, HttpStatus } from '@nestjs/common';
import { InjectQueue } from '@nestjs/bullmq';
import { Queue } from 'bullmq';

@Controller('webhooks/woocommerce')
export class WebhookController {
  constructor(@InjectQueue('orders') private readonly orderQueue: Queue) {}

  @Post('order-created')
  @HttpCode(HttpStatus.OK)
  async handleOrderCreated(@Body() payload: any) {
    // Instantly acknowledge receipt back to WooCommerce
    await this.orderQueue.add('process-order', payload, {
      attempts: 5,
      backoff: { type: 'exponential', delay: 2000 },
    });
    return { status: 'acknowledged' };
  }
}

// order.processor.ts
import { Processor, WorkerHost } from '@nestjs/bullmq';
import { Job } from 'bullmq';

@Processor('orders')
export class OrderProcessor extends WorkerHost {
  async process(job: Job<any, any, string>): Promise<any> {
    const orderData = job.data;
    // Perform decoupled tasks: Sync inventory, process fulfillment, send emails
    console.log(`Processing async WooCommerce Order ID: ${orderData.id}`);
  }
}
```

---

## Q5: How do you design NestJS Microservices supporting both HTTP/REST for external clients and high-performance internal gRPC / RabbitMQ transport protocols (Hybrid Applications)?

**Answer:**
A **Hybrid Application** in NestJS exposes standard REST/GraphQL HTTP endpoints to end-users while concurrently listening to internal microservice transports (gRPC, TCP, RabbitMQ, Kafka).

### Bootstrap Hybrid Application (`src/main.ts`):

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { MicroserviceOptions, Transport } from '@nestjs/microservices';
import { join } from 'path';

async function bootstrap() {
  // Primary HTTP App
  const app = await NestFactory.create(AppModule);

  // Connect gRPC Internal Microservice
  app.connectMicroservice<MicroserviceOptions>({
    transport: Transport.GRPC,
    options: {
      package: 'hero',
      protoPath: join(__dirname, 'hero/hero.proto'),
      url: '0.0.0.0:50051',
    },
  });

  // Connect RabbitMQ Event Bus
  app.connectMicroservice<MicroserviceOptions>({
    transport: Transport.RMQ,
    options: {
      urls: [process.env.RABBITMQ_URL || 'amqp://localhost:5672'],
      queue: 'orders_queue',
      queueOptions: { durable: true },
    },
  });

  await app.startAllMicroservices();
  await app.listen(3000);
}
bootstrap();
```

---

## Q6: What strategy should be used to implement dynamic, multi-tenant database routing (Schema-per-tenant vs Database-per-tenant) in NestJS?

**Answer:**
Multi-tenant dynamic database routing is cleanly achieved using **Request Context Resolution** and **Dynamic Connection Pools** using Node.js `AsyncLocalStorage`.

```typescript
// tenant.middleware.ts
import { Injectable, NestMiddleware, BadRequestException } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';
import { AsyncLocalStorage } from 'async_hooks';

export const tenantStorage = new AsyncLocalStorage<string>();

@Injectable()
export class TenantMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    const tenantId = req.headers['x-tenant-id'] as string;
    if (!tenantId) {
      throw new BadRequestException('Missing X-Tenant-ID header');
    }
    tenantStorage.run(tenantId, () => next());
  }
}
```

---

## Q7: What are the differences between Injection Scopes (`DEFAULT`, `REQUEST`, `TRANSIENT`), how do they impact performance, and how does `AsyncLocalStorage` solve request-tracking overhead?

**Answer:**

* **`Scope.DEFAULT` (Singleton)**: One instance is cached across the entire application lifecycle. **Optimal memory and speed performance**.
* **`Scope.REQUEST`**: A new provider instance is created for **every incoming HTTP request**. *Performance Risk*: Request scope cascades up the dependency chain. If a Service is Request-Scoped, any Controller or Service injecting it becomes Request-Scoped, leading to high garbage collection churn and latency spikes under concurrent load.
* **`Scope.TRANSIENT`**: A dedicated instance is created for every provider that injects it.

### Comparative Paradigm: `Scope.REQUEST` vs Modern `AsyncLocalStorage`

#### Legacy / Memory-Intensive Request Tracking (`Scope.REQUEST`):

```typescript
import { Injectable, Scope, Inject } from '@nestjs/common';
import { REQUEST } from '@nestjs/core';
import { Request } from 'express';

@Injectable({ scope: Scope.REQUEST }) // Re-created on every single request!
export class LoggerService {
  constructor(@Inject(REQUEST) private readonly request: Request) {}

  log(message: string) {
    const correlationId = this.request.headers['x-correlation-id'];
    console.log(`[${correlationId}] ${message}`);
  }
}

```

#### Modern High-Performance Request Tracking (`AsyncLocalStorage` + Singleton Scope):

```typescript
import { Injectable, Scope } from '@nestjs/common';
import { AsyncLocalStorage } from 'async_hooks';

export const traceStorage = new AsyncLocalStorage<Map<string, string>>();

@Injectable({ scope: Scope.DEFAULT }) // Remains a high-speed Singleton!
export class LoggerService {
  log(message: string) {
    const store = traceStorage.getStore();
    const correlationId = store?.get('correlationId') || 'UNKNOWN';
    console.log(`[${correlationId}] ${message}`);
  }
}

```

---

## Q8: How do you construct an Enterprise-Grade Global Exception Filter and Structured Interceptor with Request Tracing (Correlation IDs)?

**Answer:**

### 1. Enterprise Exception Filter (`src/common/filters/http-exception.filter.ts`):

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException, HttpStatus, Logger } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch()
export class GlobalExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(GlobalExceptionFilter.name);

  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    const status = exception instanceof HttpException 
      ? exception.getStatus() 
      : HttpStatus.INTERNAL_SERVER_ERROR;

    const message = exception instanceof HttpException 
      ? exception.getResponse() 
      : 'Internal Server Error';

    const correlationId = request.headers['x-correlation-id'] || 'N/A';

    this.logger.error(`[Correlation ID: ${correlationId}] HTTP Status: ${status} Error: ${JSON.stringify(message)}`);

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      correlationId,
      error: typeof message === 'object' ? message : { message },
    });
  }
}

```

### 2. Logging Interceptor (`src/common/interceptors/logging.interceptor.ts`):

```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler, Logger } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class TransformInterceptor implements NestInterceptor {
  private readonly logger = new Logger('HTTP');

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const req = context.switchToHttp().getRequest();
    const { method, url } = req;
    const now = Date.now();

    return next.handle().pipe(
      tap(() => {
        const delay = Date.now() - now;
        this.logger.log(`${method} ${url} ${delay}ms`);
      }),
    );
  }
}

```

## NestJS app example

- Bootstrapping with middleware (compression, helmet, session, permissions-policy) 
- A module with TypeORM integration with SQLite
- A controller with validation/transform, guards, pipes and Swagger decorators 
- A service that injects the request object 
- A DTO with class-transformer & class-validator 
- A test file using @nestjs/testing 

## 📂 Project Structure 
```bash
nestjs-demo/
  package.json
  tsconfig.json
  src/
    main.ts
    app.module.ts
    user.entity.ts
    user.dto.ts
    user.service.ts
    user.controller.ts
  test/
    user.controller.spec.ts
```

## 🚀 Running the App
1. Install dependencies:
   ```bash
   npm install
   ```
2. Start the app:
   ```bash
   npm run start:dev
   ```
3. Open Swagger UI:
   ```
   http://localhost:3000/api
   ```
4. Production:
   Use `npm run start:prod` after building with `npm run build`

🚀 Bootstrapping & Modules 
```ts
import { NestFactory } from '@nestjs/core'; 
import { Module, NestModule, MiddlewareConsumer, VersioningType } from '@nestjs/common'; 
 
@Module({ 
  imports: [], 
  controllers: [], 
  providers: [], 
}) 
export class AppModule implements NestModule { 
  configure(consumer: MiddlewareConsumer) { 
    // Example: apply middleware globally 
    consumer.apply(compression(), bodyParser.json()).forRoutes('*'); 
  } 
} 
 
async function bootstrap() { 
  const app = await NestFactory.create(AppModule); 
  app.enableVersioning({ type: VersioningType.URI }); 
  await app.listen(3000); 
} 
bootstrap(); 
```

🎯 Controllers, Pipes & Guards 
```ts
import { 
  Controller, Get, Param, Query, Headers, 
  ValidationPipe, UsePipes, UseGuards, Injectable, CanActivate, ExecutionContext 
} from '@nestjs/common'; 
 
@Injectable() 
class AuthGuard implements CanActivate { 
  canActivate(context: ExecutionContext): boolean { 
    const request = context.switchToHttp().getRequest(); 
    return request.headers['authorization'] === 'secret-token'; 
  } 
} 
 
@Controller('users') 
@UseGuards(AuthGuard) 
export class UserController { 
  @Get(':id') 
  @UsePipes(new ValidationPipe({ transform: true })) 
  getUser(@Param('id') id: string, @Query('details') details: string, @Headers() headers: any) { 
    return { id, details, headers }; 
  } 
} 
```

🗄️ Database (TypeORM) 
```ts
import { TypeOrmModule } from '@nestjs/typeorm'; 
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm'; 
 
@Entity() 
export class User { 
  @PrimaryGeneratedColumn() 
  id: number; 
 
  @Column() 
  name: string; 
} 
 
@Module({ 
  imports: [ 
    TypeOrmModule.forRoot({ 
      type: 'sqlite', 
      database: 'test.db', 
      entities: [User], 
      synchronize: true, 
    }), 
    TypeOrmModule.forFeature([User]), 
  ], 
}) 
export class AppModule {} 
```

🔒 Security Middleware 
```ts
import * as session from 'express-session'; 
import RedisStore from 'connect-redis'; 
import helmet from 'helmet'; 
import * as permissionsPolicy from 'permissions-policy'; 
 
app.use(helmet()); 
app.use(permissionsPolicy({ features: { camera: ['none'], geolocation: ['none'] } })); 
app.use(session({ 
  store: new RedisStore({ client: redisClient }), 
  secret: 'keyboard cat', 
  resave: false, 
  saveUninitialized: false, 
})); 
```

🧰 Utilities 
```ts
import { omitBy, isNil } from 'lodash'; 
 
const cleaned = omitBy({ name: 'Alice', age: null }, isNil); 
// Result: { name: 'Alice' } 
```

🧪 Testing 
```ts
import { Test, TestingModule } from '@nestjs/testing'; 
 
describe('UserController', () => { 
  let controller: UserController; 
 
  beforeEach(async () => { 
    const module: TestingModule = await Test.createTestingModule({ 
      controllers: [UserController], 
    }).compile(); 
 
    controller = module.get<UserController>(UserController); 
  }); 
 
  it('should return user', () => { 
    expect(controller.getUser('1', 'full', {})).toEqual({ id: '1', details: 'full', headers: {} }); 
  }); 
}); 
```

🏗️ Request Injection 
```ts
import { REQUEST } from '@nestjs/core'; 
import { Request } from 'express'; 
 
@Injectable() 
class MyService { 
  constructor(@Inject(REQUEST) private readonly request: Request) {} 
 
  getIp(): string { 
    return this.request.ip; 
  } 
} 
```

🔄 Class Transformer & Validator 
```ts
import { plainToInstance } from 'class-transformer'; 
import { validate, IsEnum, IsString } from 'class-validator'; 
 
enum Role { ADMIN = 'admin', USER = 'user' } 
 
class UserDto { 
  @IsString() 
  name: string; 
 
  @IsEnum(Role) 
  role: Role; 
} 
 
const input = { name: 'Bob', role: 'admin' }; 
const user = plainToInstance(UserDto, input); 
 
validate(user).then(errors => { 
  if (errors.length > 0) console.log('Validation failed', errors); 
  else console.log('Validation succeeded', user); 
}); 
```

📖 Swagger Decorators 
```ts
import { ApiTags, ApiOperation, ApiResponse, ApiBearerAuth, ApiHeaders, ApiProperty } from '@nestjs/swagger'; 
 
class UserResponse { 
  @ApiProperty({ example: 'Alice' }) 
  name: string; 
} 
 
@ApiTags('users') 
@Controller('users') 
export class UserController { 
  @Get() 
  @ApiOperation({ summary: 'Get all users' }) 
  @ApiResponse({ status: 200, type: [UserResponse] }) 
  @ApiBearerAuth() 
  @ApiHeaders([{ name: 'x-custom-header', description: 'Custom header' }]) 
  getUsers(): UserResponse[] { 
    return [{ name: 'Alice' }]; 
  } 
} 
```

---