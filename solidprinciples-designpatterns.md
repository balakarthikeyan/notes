# SOLID Principles & Design Patterns: Complete Developer Study Guide

This document serves as a comprehensive study guide, reference manual, and architectural guide for **SOLID Principles** and **Design Patterns**. It covers fundamental definitions, comparative legacy vs. modern syntaxes, and production-ready examples across **PHP (PHP 7.x vs. PHP 8.x)**, **Angular**, **NestJS**, and **React / Next.js**.

---

## SOLID Principles

SOLID is an acronym representing five foundational design principles for writing maintainable, extensible, and scalable software. Introduced by Robert C. Martin ("Uncle Bob"), these principles guide developers toward building loosely coupled and highly cohesive systems.

---

### 1. Single Responsibility Principle (SRP)

> **Definition**: A class or module should have one, and only one, reason to change. It should focus exclusively on a single responsibility or business concern.

#### PHP Implementation
##### Legacy Version (Violating SRP - PHP 7.x)
```php
<?php

class User
{
    private string $email;

    public function __construct(string $email)
    {
        $this->email =$email;
    }

    public function saveToDatabase(): void
    {
        // Direct DB connection and query inside user model
        $pdo = new PDO('mysql:host=localhost;dbname=test', 'root', '');
        $stmt =$pdo->prepare('INSERT INTO users (email) VALUES (?)');
        $stmt->execute([$this->email]);
    }

    public function sendWelcomeEmail(): void
    {
        // Direct email handling inside user model
        mail($this->email, "Welcome!", "Thanks for registering.");
    }
}

```

##### Modern Version (Adhering to SRP - PHP 8.x)

```php
<?php

readonly class User
{
    public function __construct(
        public string $email
    ) {}
}

class UserRepository
{
    public function save(User $user): void
    {
        // Responsibility: Data Persistence
        $pdo = new PDO('mysql:host=localhost;dbname=test', 'root', '');
        $stmt =$pdo->prepare('INSERT INTO users (email) VALUES (?)');
        $stmt->execute([$user->email]);
    }
}

class MailerService
{
    public function sendWelcomeEmail(User $user): void
    {
        // Responsibility: Email Communication
        mail($user->email, "Welcome!", "Thanks for registering.");
    }
}

```

#### Angular Example

```typescript
import { Component, Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

// Responsibility 1: Data Access Layer
@Injectable({ providedIn: 'root' })
export class UserService {
  constructor(private http: HttpClient) {}

  getUsers(): Observable<any[]> {
    return this.http.get<any[]>('/api/users');
  }
}

// Responsibility 2: UI Presentation Layer
@Component({
  selector: 'app-user-list',
  template: `
    <ul>
      <li *ngFor="let user of users">{{ user.name }}</li>
    </ul>
  `
})
export class UserListComponent {
  users: any[] = [];

  constructor(private userService: UserService) {}

  ngOnInit(): void {
    this.userService.getUsers().subscribe(data => this.users = data);
  }
}

```

#### NestJS Example

```typescript
import { Injectable, Controller, Post, Body } from '@nestjs/common';

// Domain entity interface
export interface CreateUserDto {
  email: string;
}

// Service handles business logic & data operation
@Injectable()
export class UserService {
  private readonly users: CreateUserDto[] = [];

  createUser(user: CreateUserDto): void {
    this.users.push(user);
  }
}

// Controller handles HTTP requests & routing only
@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Post()
  create(@Body() dto: CreateUserDto) {
    this.userService.createUser(dto);
    return { success: true };
  }
}

```

#### React / Next.js Example

```tsx
import React, { useState, useEffect } from 'react';

// Custom Hook: Responsibility = Data Fetching & Business Logic
function useUsers() {
  const [users, setUsers] = useState<string[]>([]);

  useEffect(() => {
    fetch('/api/users')
      .then(res => res.json())
      .then(data => setUsers(data));
  }, []);

  return { users };
}

// Presentation Component: Responsibility = UI Rendering
export default function UserList() {
  const { users } = useUsers();

  return (
    <ul>
      {users.map((user, idx) => (
        <li key={idx}>{user}</li>
      ))}
    </ul>
  );
}

```

---

### 2. Open/Closed Principle (OCP)

> **Definition**: Software entities (classes, modules, functions) should be open for extension, but closed for modification.

#### PHP Implementation

##### Legacy Version (Violating OCP with switch/if statements)

```php
<?php

class PaymentProcessor
{
    public function process(string $type, float$amount): void
    {
        if ($type === 'paypal') {             // Process PayPal         } elseif ($type === 'stripe') {
            // Process Stripe
        }
        // Adding a new gateway requires modifying this class directly!
    }
}

```

##### Modern Version (Adhering to OCP using Interfaces - PHP 8.x)

```php
<?php

interface PaymentGatewayInterface
{
    public function process(float $amount): void;
}

class PaypalGateway implements PaymentGatewayInterface
{
    public function process(float $amount): void
    {
        // Process PayPal Payment
    }
}

class StripeGateway implements PaymentGatewayInterface
{
    public function process(float $amount): void
    {
        // Process Stripe Payment
    }
}

class PaymentProcessor
{
    public function process(PaymentGatewayInterface $gateway, float$amount): void
    {
        $gateway->process($amount);
    }
}

```

#### Angular Example

```typescript
import { Component, Input } from '@angular/core';

export interface CardFormatter {
  format(value: string): string;
}

export class CreditCardFormatter implements CardFormatter {
  format(value: string): string {
    return '**** **** **** ' + value.slice(-4);
  }
}

@Component({
  selector: 'app-card-display',
  template: `<div>Formatted: {{ formatter.format(cardNumber) }}</div>`
})
export class CardDisplayComponent {
  @Input() cardNumber: string = '';
  @Input() formatter: CardFormatter = new CreditCardFormatter();
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';

export interface NotificationProvider {
  send(message: string): void;
}

@Injectable()
export class EmailProvider implements NotificationProvider {
  send(message: string): void {
    // Send email logic
  }
}

@Injectable()
export class SmsProvider implements NotificationProvider {
  send(message: string): void {
    // Send SMS logic
  }
}

@Injectable()
export class NotificationService {
  constructor(private readonly provider: NotificationProvider) {}

  notify(message: string): void {
    this.provider.send(message);
  }
}

```

#### React / Next.js Example

```tsx
import React, { ReactNode } from 'react';

interface ModalProps {
  isOpen: boolean;
  children: ReactNode;
}

// Modal is closed for modification but open for extension via children composition
export function Modal({ isOpen, children }: ModalProps) {
  if (!isOpen) return null;
  return (
    <div className="modal-overlay">
      <div className="modal-content">{children}</div>
    </div>
  );
}

// Extends functionality without altering Modal component
export function CustomModal() {
  return (
    <Modal isOpen="{true}">
      <h2>Extended Title</h2>
      <button onClick={() => alert('Closed')}>Close</button>
    </Modal>
  );
}

```

---

### 3. Liskov Substitution Principle (LSP)

> **Definition**: Derived classes must be completely substitutable for their base classes without altering the correctness or functionality of the program.

#### PHP Implementation

##### Legacy Version (Violating LSP)

```php
<?php

class Bird
{
    public function fly(): string
    {
        return "Flying high!";
    }
}

class Ostrich extends Bird
{
    public function fly(): string
    {
        // Exception breaks the expected behavior of the parent class
        throw new Exception("Ostriches cannot fly!");
    }
}

```

##### Modern Version (Adhering to LSP - PHP 8.x)

```php
<?php

interface BirdInterface
{
    public function move(): string;
}

interface FlyingBirdInterface extends BirdInterface
{
    public function fly(): string;
}

class Eagle implements FlyingBirdInterface
{
    public function move(): string { return $this->fly(); }
    public function fly(): string { return "Flying high!"; }
}

class Ostrich implements BirdInterface
{
    public function move(): string { return "Running fast!"; }
}

```

#### Angular Example

```typescript
export abstract class BaseDataService {
  abstract fetchData(): string[];
}

export class ApiDataService extends BaseDataService {
  override fetchData(): string[] {
    return ['Data 1', 'Data 2'];
  }
}

export class MockDataService extends BaseDataService {
  override fetchData(): string[] {
    return ['Mock 1', 'Mock 2'];
  }
}

```

#### NestJS Example

```typescript
export abstract class BaseStorageService {
  abstract save(fileName: string, content: Buffer): Promise<void>;
}

export class LocalStorageService extends BaseStorageService {
  async save(fileName: string, content: Buffer): Promise<void> {
    // Save to local filesystem
  }
}

export class S3StorageService extends BaseStorageService {
  async save(fileName: string, content: Buffer): Promise<void> {
    // Upload to AWS S3 bucket
  }
}

```

#### React / Next.js Example

```tsx
import React, { ButtonHTMLAttributes } from 'react';

interface CustomButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary';
}

// Replaces standard <button> without breaking native standard properties
export function CustomButton({ variant = 'primary', children, ...rest }: CustomButtonProps) {
  return (
    <button className={`btn btn-${variant}`} {...rest}>
      {children}
    </button>
  );
}

```

---

### 4. Interface Segregation Principle (ISP)

> **Definition**: Clients should not be forced to depend on interfaces they do not use. Prefer small, focused interfaces over large monolithic ones.

#### PHP Implementation

##### Legacy Version (Violating ISP)

```php
<?php

interface WorkerInterface
{
    public function work(): void;
    public function eat(): void;
}

class RobotWorker implements WorkerInterface
{
    public function work(): void { /* working */ }
    public function eat(): void {
        // Forced implementation of irrelevant method
        throw new UnsupportedOperationException("Robots don't eat");
    }
}

```

##### Modern Version (Adhering to ISP - PHP 8.x)

```php
<?php

interface WorkableInterface
{
    public function work(): void;
}

interface FeedableInterface
{
    public function eat(): void;
}

class HumanWorker implements WorkableInterface, FeedableInterface
{
    public function work(): void { /* working */ }
    public function eat(): void { /* eating */ }
}

class RobotWorker implements WorkableInterface
{
    public function work(): void { /* working */ }
}

```

#### Angular Example

```typescript
export interface Printable {
  print(): void;
}

export interface Shareable {
  share(url: string): void;
}

// Angular Component implements only relevant interfaces
export class DocumentComponent implements Printable {
  print(): void {
    window.print();
  }
}

```

#### NestJS Example

```typescript
export interface LogReader {
  readLogs(): string[];
}

export interface LogWriter {
  writeLog(entry: string): void;
}

export class ConsoleLogger implements LogWriter {
  writeLog(entry: string): void {
    console.log(entry);
  }
}

```

#### React / Next.js Example

```tsx
interface UserProps {
  name: string;
  avatarUrl: string;
}

// Instead of accepting full User object with unused fields, pass exact props required
export function UserAvatar({ name, avatarUrl }: UserProps) {
  return <img src={avatarUrl} alt={name} className="avatar" />;
}

```

---

### 5. Dependency Inversion Principle (DIP)

> **Definition**: High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.

#### PHP Implementation

##### Legacy Version (Violating DIP)

```php
<?php

class MySQLConnection
{
    public function connect(): void { /* DB Connect */ }
}

class UserReport
{
    private MySQLConnection $db;

    public function __construct()
    {
        // Tightly coupled to MySQLConnection
        $this->db = new MySQLConnection();
    }
}

```

##### Modern Version (Adhering to DIP - PHP 8.x)

```php
<?php

interface DatabaseConnectionInterface
{
    public function connect(): void;
}

class MySQLConnection implements DatabaseConnectionInterface
{
    public function connect(): void { /* MySQL Connect */ }
}

class UserReport
{
    public function __construct(
        private DatabaseConnectionInterface $db
    ) {}
}

```

#### Angular Example

```typescript
import { InjectionToken, Injectable, Inject } from '@angular/core';

export interface Logger {
  log(msg: string): void;
}

export const LOGGER_TOKEN = new InjectionToken<Logger>('LOGGER_TOKEN');

@Injectable()
export class ConsoleLoggerService implements Logger {
  log(msg: string): void {
    console.log(msg);
  }
}

@Injectable()
export class AnalyticsService {
  constructor(@Inject(LOGGER_TOKEN) private logger: Logger) {}

  trackEvent(event: string): void {
    this.logger.log(`Event: ${event}`);
  }
}

```

#### NestJS Example

```typescript
import { Injectable, Inject } from '@nestjs/common';

export interface DataRepository {
  getData(): string;
}

@Injectable()
export class SqlRepository implements DataRepository {
  getData(): string {
    return 'SQL Data';
  }
}

@Injectable()
export class DataService {
  constructor(
    @Inject('DATA_REPOSITORY') private readonly repository: DataRepository
  ) {}

  fetch(): string {
    return this.repository.getData();
  }
}

```

#### React / Next.js Example

```tsx
import React, { createContext, useContext, ReactNode } from 'react';

interface AuthService {
  isAuthenticated(): boolean;
}

const AuthContext = createContext<AuthService null |>(null);

export function AuthProvider({ service, children }: { service: AuthService; children: ReactNode }) {
  return <AuthContext.Provider value="{service}">{children}</AuthContext.Provider>;
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) throw new Error('useAuth must be used within AuthProvider');
  return context;
}

```

---

## What are Design Patterns in PHP & Modern Frameworks?

A **Design Pattern** is a general, reusable solution to a commonly occurring problem within software design. It is not a finished code snippet, but rather a template or blueprint for solving architectural problems.

### Four Essential Elements of a Design Pattern

* **Pattern Name**: A concise handle used to describe the design problem, its solutions, and consequences.
* **Problem**: The context and scenario where the pattern should be applied.
* **Solution**: The elements, relationships, responsibilities, and interactions forming the design.
* **Consequences**: The trade-offs and results of applying the pattern.

---

## Benefits of Implementing Design Patterns

* Provide proven solutions to repetitive software design problems.
* Standardize code architecture, enabling smoother developer communication.
* Improve codebase maintainability, scalability, and testability.
* Accelerate development velocity by reusing architectural blueprints.

---

## Pattern Categories & Overview

### Creational Patterns

Focus on object creation mechanisms to decouple creation logic from business usage.

| Name | Pattern Summary |
| --- | --- |
| **Abstract Factory** | Creates instances of several families of classes without specifying concrete classes. |
| **Builder** | Separates complex object construction from its representation. |
| **Factory Method** | Defines an interface for creating objects, letting subclasses decide which class to instantiate. |
| **Object Pool** | Recycles expensive objects that are no longer in active use to optimize memory/performance. |
| **Prototype** | Creates new objects by cloning an existing initialized prototype instance. |
| **Singleton** | Ensures a class has only one instance while providing a global point of access. |

### Structural Patterns

Focus on class and object composition to build larger structures without sacrificing flexibility.

| Name | Pattern Summary |
| --- | --- |
| **Adapter** | Matches and bridges interfaces between incompatible classes. |
| **Bridge** | Decouples an abstraction from its implementation so both can vary independently. |
| **Composite** | Composes objects into tree structures to represent part-whole hierarchies. |
| **Decorator** | Dynamically attaches additional responsibilities to an object at runtime. |
| **Facade** | Provides a simplified interface to a complex framework or subsystem. |
| **Flyweight** | Shares fine-grained instances to minimize memory usage across similar objects. |
| **Private Class Data** | Restricts accessor/mutator access to encapsulate structural state. |
| **Proxy** | Provides a surrogate or placeholder object to control access to another object. |

### Behavioral Patterns

Focus on algorithms, communication, and assignment of responsibilities between objects.

| Name | Pattern Summary |
| --- | --- |
| **Chain of Responsibility** | Passes requests along a chain of potential handlers until one processes it. |
| **Command** | Encapsulates a request as an object, allowing parameterization and undo operations. |
| **Interpreter** | Evaluates or interprets sentences in a specified grammar/language representation. |
| **Iterator** | Accesses elements of a collection sequentially without exposing underlying representation. |
| **Mediator** | Defines simplified, centralized communication between dynamic objects. |
| **Memento** | Captures and restores an object's internal state without violating encapsulation. |
| **Null Object** | Serves as a neutral default value or no-op behavior object to avoid null checks. |
| **Observer** | Notifies dependent subscriber objects automatically of state changes. |
| **State** | Alters an object's internal behavior dynamically when its state changes. |
| **Strategy** | Encapsulates interchangeable algorithms inside separate classes. |
| **Template Method** | Outlines skeletal algorithm steps in a base class, deferring details to subclasses. |
| **Visitor** | Defines a new operation to an object structure without modifying the classes. |

---

## Detailed Design Patterns & Framework Implementations

---

### 1. Factory Pattern

Encapsulates object creation logic to decouple object initialization from the client code.

#### PHP Implementation

##### Legacy Version (PHP 7.x)

```php
<?php

class LegacyButtonFactory
{
    public static function createButton($type)
    {
        if ($type === 'primary') {
            return new PrimaryButton();
        }
        return new DefaultButton();
    }
}

```

##### Modern Version (PHP 8.x)

```php
<?php

interface ButtonInterface
{
    public function render(): string;
}

class PrimaryButton implements ButtonInterface
{
    public function render(): string { return '<button class="primary">Submit</button>'; }
}

class DefaultButton implements ButtonInterface
{
    public function render(): string { return '<button>Default</button>'; }
}

class ButtonFactory
{
    public static function create(string $type): ButtonInterface
    {
        return match ($type) {
            'primary' => new PrimaryButton(),
            default   => new DefaultButton(),
        };
    }
}

```

#### Angular Example

```typescript
import { Provider } from '@angular/core';

export abstract class LoggerService {
  abstract log(message: string): void;
}

export class ProductionLogger extends LoggerService {
  log(message: string): void { /* Send to remote server */ }
}

export class DevelopmentLogger extends LoggerService {
  log(message: string): void { console.log(message); }
}

export const LoggerFactory: Provider = {
  provide: LoggerService,
  useFactory: () => {
    return isProductionEnv() ? new ProductionLogger() : new DevelopmentLogger();
  }
};

function isProductionEnv(): boolean { return false; }

```

#### NestJS Example

```typescript
import { Module } from '@nestjs/common';

export class DevDatabase { connect() { return 'Dev DB'; } }
export class ProdDatabase { connect() { return 'Prod DB'; } }

const dbProvider = {
  provide: 'DATABASE_CONNECTION',
  useFactory: () => {
    return process.env.NODE_ENV === 'production' ? new ProdDatabase() : new DevDatabase();
  },
};

@Module({ providers: [dbProvider], exports: ['DATABASE_CONNECTION'] })
export class DatabaseModule {}

```

#### React / Next.js Example

```tsx
import React from 'react';

function PrimaryCard() { return <div className="card-primary">Primary Card Content</div>; }
function SecondaryCard() { return <div className="card-secondary">Secondary Card Content</div>; }

interface CardProps {
  variant: 'primary' | 'secondary';
}

export default function CardFactory({ variant }: CardProps) {
  const cards = {
    primary: <PrimaryCard/>,
    secondary: <SecondaryCard/>,
  };

  return cards[variant] || <SecondaryCard/>;
}

```

---

### 2. Adapter Pattern

Converts the interface of a class into another interface expected by clients.

#### PHP Implementation

##### Legacy Version (PHP 7.x)

```php
<?php

class LegacyMailer
{
    public function sendOldEmail($to,$subject) { /* legacy send */ }
}

class MailerAdapter
{
    private $legacyMailer;
    public function __construct(LegacyMailer $legacyMailer) {
        $this->legacyMailer =$legacyMailer;
    }
    public function send($to, $body) {$this->legacyMailer->sendOldEmail($to,$body);
    }
}

```

##### Modern Version (PHP 8.x)

```php
<?php

interface MailerInterface
{
    public function send(string $recipient, string$content): void;
}

class ThirdPartyMailer
{
    public function dispatchEmail(string $emailTo, string$text): void
    {
        // Native 3rd-party library call
    }
}

class ThirdPartyMailerAdapter implements MailerInterface
{
    public function __construct(
        private ThirdPartyMailer $thirdPartyMailer
    ) {}

    public function send(string $recipient, string$content): void
    {
        $this->thirdPartyMailer->dispatchEmail($recipient,$content);
    }
}

```

#### Angular Example

```typescript
import { Injectable } from '@angular/core';

// Server DTO
export interface ApiUserDto {
  first_name: string;
  last_name: string;
}

// Client UI Model
export interface UserModel {
  fullName: string;
}

@Injectable({ providedIn: 'root' })
export class UserAdapterService {
  adapt(dto: ApiUserDto): UserModel {
    return {
      fullName: `${dto.first_name} ${dto.last_name}`,
    };
  }
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';

export interface ModernPaymentResponse {
  transactionId: string;
  status: string;
}

@Injectable()
export class StripeAdapter {
  adaptResponse(stripeResponse: any): ModernPaymentResponse {
    return {
      transactionId: stripeResponse.id,
      status: stripeResponse.status === 'succeeded' ? 'SUCCESS' : 'FAILED',
    };
  }
}

```

#### React / Next.js Example

```tsx
import React from 'react';

// Adapter Hook to translate API data layout into Hook format
export function useAdaptedUserData(rawApiData: { user_id: number; user_name: string }) {
  return {
    id: rawApiData.user_id,
    displayName: rawApiData.user_name,
  };
}

export function UserProfileCard({ rawData }: { rawData: { user_id: number; user_name: string } }) {
  const user = useAdaptedUserData(rawData);
  return <div>{user.displayName} (ID: {user.id})</div>;
}

```

---

### 3. Singleton Pattern

Ensures a class has only one instance and provides a global point of access to it.

#### PHP Implementation

##### Legacy Version (PHP 7.x)

```php
<?php

class DatabaseSingleton
{
    private static $instance = null;

    private function __construct() {}

    public static function getInstance()
    {
        if (self::$instance === null) {
            self::$instance = new DatabaseSingleton();
        }
        return self::$instance;
    }
}

```

##### Modern Version (PHP 8.x)

```php
<?php

class DatabaseSingleton
{
    private static ?DatabaseSingleton $instance = null;

    private function __construct() {}

    // Prevent cloning and unserialization
    private function __clone() {}
    public function __wakeup() { throw new \Exception("Cannot unserialize singleton"); }

    public static function getInstance(): DatabaseSingleton
    {
        return self::$instance ??= new self();
    }
}

```

#### Angular Example

```typescript
import { Injectable } from '@angular/core';

// 'providedIn: root' creates a single shared instance throughout application lifetime
@Injectable({
  providedIn: 'root',
})
export class AppConfigService {
  private config = { theme: 'dark' };

  getConfig() {
    return this.config;
  }
}

```

#### NestJS Example

```typescript
import { Injectable, Scope } from '@nestjs/common';

// Scope.DEFAULT makes NestJS injectables singletons by default
@Injectable({ scope: Scope.DEFAULT })
export class CacheStore {
  private cache = new Map<string, any>();

  get(key: string) { return this.cache.get(key); }
  set(key: string, val: any) { this.cache.set(key, val); }
}

```

#### React / Next.js Example

```typescript
// Singleton instance pattern inside module scope (Next.js server-side / client-side module cache)
class GlobalStore {
  private static instance: GlobalStore;
  public data: Record<string, any> = {};

  private constructor() {}

  public static getInstance(): GlobalStore {
    if (!GlobalStore.instance) {
      GlobalStore.instance = new GlobalStore();
    }
    return GlobalStore.instance;
  }
}

export const storeInstance = GlobalStore.getInstance();

```

---

### 4. Observer Pattern

Defines a subscription mechanism to notify multiple objects about events happening to the object they're observing.

#### PHP Implementation

##### Legacy Version (PHP 7.x using SplObserver)

```php
<?php

class LegacySubject implements SplSubject {
    private $observers = [];
    public function attach(SplObserver $o) { $this->observers[] =$o; }
    public function detach(SplObserver $o) {}
    public function notify() {
        foreach ($this->observers as$o) { $o->update($this); }
    }
}

```

##### Modern Version (PHP 8.x)

```php
<?php

interface ObserverInterface
{
    public function update(string $eventData): void;
}

class UserRegistrationNotifier
{
    /** @var ObserverInterface[] */
    private array $observers = [];

    public function subscribe(ObserverInterface $observer): void
    {
        $this->observers[] =$observer;
    }

    public function registerUser(string $email): void
    {
        // Logic for user registration
        $this->notify($email);
    }

    private function notify(string $email): void
    {
        foreach ($this->observers as$observer) {
            $observer->update($email);
        }
    }
}

```

#### Angular Example

```typescript
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class ThemeService {
  private themeSubject = new BehaviorSubject<string>('light');
  public theme$: Observable<string> = this.themeSubject.asObservable();

  toggleTheme(newTheme: string): void {
    this.themeSubject.next(newTheme);
  }
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';
import { EventEmitter2, OnEvent } from '@nestjs/event-emitter';

@Injectable()
export class OrderService {
  constructor(private eventEmitter: EventEmitter2) {}

  createOrder() {
    this.eventEmitter.emit('order.created', { orderId: 101 });
  }
}

@Injectable()
export class EmailListener {
  @OnEvent('order.created')
  handleOrderCreated(payload: { orderId: number }) {
    // Send email notification
  }
}

```

#### React / Next.js Example

```tsx
import React, { useState, useEffect } from 'react';

// Shared Event Bus
class EventBus {
  private listeners: Record<string, Function[]> = {};

  on(event: string, fn: Function) {
    (this.listeners[event] = this.listeners[event] || []).push(fn);
  }

  emit(event: string, data: any) {
    (this.listeners[event] || []).forEach(fn => fn(data));
  }
}

export const globalBus = new EventBus();

export function NotificationBadge() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    globalBus.on('notify', () => setCount(prev => prev + 1));
  }, []);

  return <span>Notifications: {count}</span>;
}

```

---

### 5. Decorator Pattern

Dynamically attaches additional responsibilities or behavior to an object.

#### PHP Implementation

##### Legacy Version (Class Wrapping - PHP 7.x)

```php
<?php

class BasicService {
    public function execute() { return "Data"; }
}

class ServiceDecorator {
    protected $service;
    public function __construct($service) { $this->service =$service; }
    public function execute() { return "Logged: " . $this->service->execute(); }
}

```

##### Modern Version (PHP 8.x Attributes & Class Wrapping)

```php
<?php

interface ServiceInterface
{
    public function execute(): string;
}

class CoreService implements ServiceInterface
{
    public function execute(): string
    {
        return "Core Execution Result";
    }
}

class LoggingDecorator implements ServiceInterface
{
    public function __construct(
        private ServiceInterface $wrapped
    ) {}

    public function execute(): string
    {
        return "[LOG]: " . $this->wrapped->execute();
    }
}

```

#### Angular Example

```typescript
import { Component } from '@angular/core';

// Angular utilizes Native TypeScript Decorators heavily
@Component({
  selector: 'app-decorated',
  template: `<p>Decorated Angular Component</p>`
})
export class DecoratedComponent {}

```

#### NestJS Example

```typescript
import { Controller, Get, UseGuards } from '@nestjs/common';

// Method/Class Decorators enrich functionality dynamically
@Controller('protected')
export class ProtectedController {
  @Get()
  @UseGuards(/* AuthGuard */)
  getProtectedData() {
    return 'Secure Data';
  }
}

```

#### React / Next.js Example

```tsx
import React, { ComponentType } from 'react';

// Higher-Order Component (HOC) acting as a Decorator
function withLogging<P extends object>(WrappedComponent: ComponentType<P>) {
  return function EnhancedComponent(props: P) {
    console.log('Rendering component with props:', props);
    return <WrappedComponent {...props}/>;
  };
}

const SimpleText = ({ text }: { text: string }) => <p>{text}</p>;
export const DecoratedText = withLogging(SimpleText);

```

---

### 6. Flyweight Pattern

Minimizes memory usage by sharing common state data across multiple objects.

#### PHP Implementation

##### Legacy Version (PHP 7.x)

```php
<?php

class LegacyFlyweightFactory {
    private $pool = [];
    public function getFlyweight($key) {
        if (!isset($this->pool[$key])) {$this->pool[$key] = new SharedData($key);
        }
        return $this->pool[$key];
    }
}

```

##### Modern Version (PHP 8.x)

```php
<?php

class SharedTreeType
{
    public function __construct(
        public readonly string $name,
        public readonly string $color,
        public readonly string $texture
    ) {}
}

class TreeFactory
{
    private static array $types = [];

    public static function getTreeType(string $name, string $color, string$texture): SharedTreeType
    {
        $key = md5($name . '_' . $color . '_' .$texture);
        if (!isset(self::$types[$key])) {
            self::$types[$key] = new SharedTreeType($name, $color,$texture);
        }
        return self::$types[$key];
    }
}

```

#### Angular Example

```typescript
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class IconFlyweightService {
  private iconCache = new Map<string, string>();

  getIconSvg(iconName: string): string {
    if (!this.iconCache.has(iconName)) {
      this.iconCache.set(iconName, `<svg id="${iconName}"></svg>`);
    }
    return this.iconCache.get(iconName)!;
  }
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class FlyweightCacheService {
  private sharedObjects = new Map<string, any>();

  getSharedObject(type: string): any {
    if (!this.sharedObjects.has(type)) {
      this.sharedObjects.set(type, { type, immutableMetadata: 'Heavy Data' });
    }
    return this.sharedObjects.get(type);
  }
}

```

#### React / Next.js Example

```tsx
import React, { memo } from 'react';

interface SharedIconProps {
  name: string;
}

// React.memo caches rendered output for shared props (Flyweight concept)
export const SharedIcon = memo(({ name }: SharedIconProps) => {
  return <i className={`icon icon-${name}`} />;
});

```

---

### 7. Simple Factory Pattern

Generates an object without exposing the creation logic to the client.

#### PHP Implementation

##### Modern Version (PHP 8.x)

```php
<?php

class SimpleUser
{
    public function __construct(public string $role) {}
}

class SimpleUserFactory
{
    public static function create(string $role): SimpleUser
    {
        return new SimpleUser($role);
    }
}

```

#### Angular Example

```typescript
export class FormFieldFactory {
  static createField(type: string) {
    switch (type) {
      case 'text': return { component: 'TextInput' };
      case 'select': return { component: 'SelectInput' };
      default: return { component: 'TextInput' };
    }
  }
}

```

#### NestJS Example

```typescript
export class NotificationFactory {
  static createProvider(type: 'email' | 'sms') {
    if (type === 'email') return new EmailProvider();
    return new SmsProvider();
  }
}

```

#### React / Next.js Example

```tsx
import React from 'react';

export function InputFactory({ type }: { type: 'text' | 'checkbox' }) {
  if (type === 'checkbox') return <input type="checkbox" />;
  return <input type="text" />;
}

```

---

### 8. Abstract Factory Pattern

Constructs families of related or dependent objects without specifying their concrete classes.

#### PHP Implementation

##### Modern Version (PHP 8.x)

```php
<?php

interface Button { public function render(): string; }
interface Checkbox { public function render(): string; }

interface GUIFactory {
    public function createButton(): Button;
    public function createCheckbox(): Checkbox;
}

class DarkButton implements Button { public function render(): string { return "Dark Button"; } }
class DarkCheckbox implements Checkbox { public function render(): string { return "Dark Checkbox"; } }

class DarkThemeFactory implements GUIFactory {
    public function createButton(): Button { return new DarkButton(); }
    public function createCheckbox(): Checkbox { return new DarkCheckbox(); }
}

```

#### Angular Example

```typescript
export interface UIWidgetFactory {
  createHeader(): any;
  createFooter(): any;
}

export class MaterialUIFactory implements UIWidgetFactory {
  createHeader() { return 'Material Header'; }
  createFooter() { return 'Material Footer'; }
}

```

#### NestJS Example

```typescript
export interface DatabaseDriverFactory {
  createQueryRunner(): any;
  createConnection(): any;
}

export class PostgresDriverFactory implements DatabaseDriverFactory {
  createQueryRunner() { return 'Postgres Query Runner'; }
  createConnection() { return 'Postgres Connection'; }
}

```

#### React / Next.js Example

```tsx
import React from 'react';

interface ThemeUIFactory {
  Button: React.FC;
}

const DarkTheme: ThemeUIFactory = {
  Button: () => <button style={{ background: '#000', color: '#fff' }}>Dark</button>
};

export function ThemeWidget({ factory }: { factory: ThemeUIFactory }) {
  const { Button } = factory;
  return <Button/>;
}

```

---

### 9. Proxy Pattern

Provides a surrogate or placeholder for another object to control access, perform lazy loading, or execute caching.

#### PHP Implementation

##### Modern Version (PHP 8.x)

```php
<?php

interface ImageInterface { public function display(): void; }

class RealImage implements ImageInterface {
    public function __construct(private string $filename) {
        $this->loadFromDisk();
    }
    private function loadFromDisk(): void { /* Heavy IO */ }
    public function display(): void { echo "Displaying " . $this->filename; }
}

class ProxyImage implements ImageInterface {
    private ?RealImage $realImage = null;

    public function __construct(private string $filename) {}

    public function display(): void {
        // Lazy initialization
        if ($this->realImage === null) {
            $this->realImage = new RealImage($this->filename);
        }
        $this->realImage->display();
    }
}

```

#### Angular Example

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, of } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable({ providedIn: 'root' })
export class CachingUserProxyService {
  private cache = new Map<string, any>();

  constructor(private http: HttpClient) {}

  getUser(id: string): Observable<any> {
    if (this.cache.has(id)) {
      return of(this.cache.get(id));
    }
    return this.http.get(`/api/users/${id}`).pipe(
      tap(data => this.cache.set(id, data))
    );
  }
}

```

#### NestJS Example

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';

@Injectable()
export class LoggingProxyMiddleware implements NestMiddleware {
  use(req: any, res: any, next: () => void) {
    console.log(`[Proxy Guard Log] ${req.method} ${req.url}`);
    next();
  }
}

```

#### React / Next.js Example

```typescript
// JavaScript Proxy API usage in Next.js/React state wrappers
export function createProtectedState<T extends object>(target: T) {
  return new Proxy(target, {
    set(obj, prop, value) {
      console.log(`Setting property ${String(prop)} to ${value}`);
      Reflect.set(obj, prop, value);
        return true;
    }
  });
}

```

---

### 10. Interpreter Pattern

Evaluates or interprets sentences in a specified grammar or syntax representation.

#### PHP Implementation

##### Modern Version (PHP 8.x)

```php
<?php

interface ExpressionInterface
{
    public function interpret(): int;
}

class NumberExpression implements ExpressionInterface
{
    public function __construct(private int $number) {}
    public function interpret(): int { return $this->number; }
}

class AddExpression implements ExpressionInterface
{
    public function __construct(
        private ExpressionInterface $left,
        private ExpressionInterface $right
    ) {}

    public function interpret(): int
    {
        return $this->left->interpret() + $this->right->interpret();
    }
}

```

#### Angular Example

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'evaluate' })
export class ExpressionInterpreterPipe implements PipeTransform {
  transform(expression: string): number {
    // Simple expression interpreter evaluation
    const tokens = expression.split(' + ');
    return tokens.reduce((acc, curr) => acc + Number(curr), 0);
  }
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class RuleInterpreterService {
  evaluateCondition(rule: string, context: Record<string, any>): boolean {
    if (rule.includes('AGE_GREATER_THAN_18')) {
      return context['age'] > 18;
    }
    return false;
  }
}

```

#### React / Next.js Example

```tsx
import React from 'react';

// Interprets dynamic template string expressions into HTML components
export function VariableInterpreter({ template, vars }: { template: string; vars: Record<string, string> }) {
  const parsed = template.replace(/\{\{(\w+)\}\}/g, (_, key) => vars[key] || '');
  return <span>{parsed}</span>;
}

```

---

### 11. State Pattern

Enables an object to alter its behavior when its internal state changes.

#### PHP Implementation

##### Modern Version (PHP 8.x)

```php
<?php

interface OrderStateInterface { public function proceed(OrderContext $context): void; }

class PendingState implements OrderStateInterface {
    public function proceed(OrderContext $context): void {
        $context->setState(new ShippedState());
    }
}

class ShippedState implements OrderStateInterface {
    public function proceed(OrderContext $context): void {
        // Final state reached
    }
}

class OrderContext {
    public function __construct(private OrderStateInterface $state) {}
    public function setState(OrderStateInterface $state): void { $this->state = $state; }
    public function proceed(): void { $this->state->proceed($this); }
}

```

#### Angular Example

```typescript
export type StepState = 'draft' | 'submitted' | 'approved';

export class WorkflowState {
  private currentState: StepState = 'draft';

  next() {
    if (this.currentState === 'draft') this.currentState = 'submitted';
    else if (this.currentState === 'submitted') this.currentState = 'approved';
  }

  get state() { return this.currentState; }
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class ConnectionContextService {
  private state: 'CONNECTED' | 'DISCONNECTED' = 'DISCONNECTED';

  connect() { this.state = 'CONNECTED'; }
  disconnect() { this.state = 'DISCONNECTED'; }
  getStatus() { return this.state; }
}

```

#### React / Next.js Example

```tsx
import React, { useReducer } from 'react';

type State = { step: 'idle' | 'loading' | 'success' };
type Action = { type: 'FETCH' } | { type: 'SUCCESS' };

function stateReducer(state: State, action: Action): State {
  switch (action.type) {
    case 'FETCH': return { step: 'loading' };
    case 'SUCCESS': return { step: 'success' };
  }
}

export function StateMachineComponent() {
  const [state, dispatch] = useReducer(stateReducer, { step: 'idle' });
  return <button onClick={() => dispatch({ type: 'FETCH' })}>State: {state.step}</button>;
}

```

---

### 12. Strategy Pattern

Encapsulates interchangeable algorithms within individual strategy classes.

#### PHP Implementation

##### Modern Version (PHP 8.x)

```php
<?php

interface ExportStrategyInterface { public function export(array $data): string; }

class CsvExportStrategy implements ExportStrategyInterface {
    public function export(array $data): string { return "id,name\n1,Test"; }
}

class JsonExportStrategy implements ExportStrategyInterface {
    public function export(array $data): string { return json_encode($data); }
}

class ExportContext {
    public function __construct(private ExportStrategyInterface $strategy) {}
    public function executeExport(array $data): string {
        return $this->strategy->export($data);
    }
}

```

#### Angular Example

```typescript
import { Injectable } from '@angular/core';

export interface SortStrategy {
  sort(data: number[]): number[];
}

@Injectable({ providedIn: 'root' })
export class AscendingSort implements SortStrategy {
  sort(data: number[]): number[] { return data.sort((a, b) => a - b); }
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';

export interface AuthStrategy {
  validate(token: string): boolean;
}

@Injectable()
export class JwtStrategy implements AuthStrategy {
  validate(token: string): boolean { return token === 'valid_jwt'; }
}

```

#### React / Next.js Example

```tsx
import React from 'react';

const renderStrategies = {
  grid: (items: string[]) => <div className="grid">{items.map(i => <div key={i}>{i}</div>)}</div>,
  list: (items: string[]) => <ul>{items.map(i => <li key={i}>{i}</li>)}</ul>,
};

export function StrategyList({ mode, items }: { mode: 'grid' | 'list'; items: string[] }) {
  return renderStrategies[mode](items);
}

```

---

### 13. Service Layer Pattern

Establishes a dedicated layer of business logic abstraction between UI controllers and persistence layers.

#### PHP Implementation

##### Modern Version (PHP 8.x)

```php
<?php

class UserService
{
    public function __construct(
        private UserRepository $repository,
        private MailerService $mailer
    ) {}

    public function registerUser(string $email): void
    {
        $user = new User($email);
        $this->repository->save($user);
        $this->mailer->sendWelcomeEmail($user);
    }
}

```

#### Angular Example

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({ providedIn: 'root' })
export class ProductService {
  constructor(private http: HttpClient) {}

  getProducts() {
    return this.http.get('/api/products');
  }
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class OrderService {
  processOrder(orderId: number) {
    // Process business logic
    return { status: 'processed', orderId };
  }
}

```

#### React / Next.js Example

```typescript
// Server Action / API Service module for Next.js
export async function createUserService(formData: FormData) {
  'use server';
  const email = formData.get('email') as string;
  // Encapsulated Business Service Logic
  return { success: true, email };
}

```

---

### 14. Repository Pattern

Decouples business logic from data access mechanisms, providing a collection-like interface for accessing domain entities.

#### PHP Implementation

##### Legacy Version (PHP 7.x)

```php
<?php

class LegacyUserRepository {
    public function find($id) {
        $sql = "SELECT * FROM users WHERE id = " . intval($id);
        return mysql_query($sql);
    }
}

```

##### Modern Version (PHP 8.x)

```php
<?php

interface UserRepositoryInterface
{
    public function findById(int $id): ?User;
    public function save(User $user): void;
}

class SqlUserRepository implements UserRepositoryInterface
{
    public function __construct(private \PDO $pdo) {}

    public function findById(int $id): ?User
    {
        $stmt = $this->pdo->prepare('SELECT email FROM users WHERE id = :id');
        $stmt->execute(['id' => $id]);
        $data = $stmt->fetch();
        return $data ? new User($data['email']) : null;
    }

    public function save(User $user): void
    {
        $stmt = $this->pdo->prepare('INSERT INTO users (email) VALUES (:email)');
        $stmt->execute(['email' => $user->email]);
    }
}

```

#### Angular Example

```typescript
import { Injectable } from '@angular/core';
import { Observable, of } from 'rxjs';

export interface Repository<T> {
  getAll(): Observable<T[]>;
}

@Injectable({ providedIn: 'root' })
export class ClientRepository implements Repository<{ id: number; name: string }> {
  getAll(): Observable<{ id: number; name: string }[]> {
    return of([{ id: 1, name: 'Client A' }]);
  }
}

```

#### NestJS Example

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class UserRepository {
  private users = [{ id: 1, name: 'John Doe' }];

  async findOne(id: number) {
    return this.users.find(user => user.id === id);
  }
}

```

#### React / Next.js Example

```typescript
// Data Access Layer (DAL) in Next.js Server Components
import { cache } from 'react';

export const getUserRepository = cache(async (id: string) => {
  // Abstracted database retrieval logic
  return { id, name: 'Jane Doe' };
});

```

---

### Frequently Asked Questions

#### What is Singleton design pattern?

Singleton design pattern is a creational pattern that is used whenever only one instance of an object is needed to be created. In this pattern, you can't initialize the class freely. Any attempts to duplicate, or to instantiate additional instances of the class should fail.

---

## ➕ Missing Technical Questions

This section contains high-level interview questions and complete answers for **Senior Full Stack PHP Engineer**, **Lead WordPress Developer**, **Solution Architect**, and **Technical Lead** profiles.

---

### Senior Full Stack PHP Engineer

#### Q1: What are PHP 8.x Fibers, and how do they differ from multithreading or Promises in Node.js?

**Answer**:
PHP Fibers are full-stack, re-entrant coroutines that allow asynchronous, non-blocking execution flow in PHP. Unlike multithreading, Fibers run on a single thread and rely on cooperative multitasking—yielding control explicitly using `Fiber::suspend()` and resuming execution via `Fiber::resume()`. Unlike Node.js Promises (which rely on the event loop and dynamic callback queues), Fibers allow developers to write non-blocking async code with synchronous coding syntax without callback nesting or promise chaining.

#### Q2: How does PHP 8 JIT (Just-In-Time) compiler work, and when does it yield performance benefits?

**Answer**:
The JIT compiler in PHP 8 operates inside the OPCache extension. Instead of repeatedly interpreting Zend Opcodes into CPU instructions, JIT compiles OPCache instructions directly into native machine code at runtime. It offers significant performance speedups for CPU-intensive tasks (such as mathematical algorithms, data processing, and image manipulation). However, for traditional I/O-bound web requests (database queries, network API calls), JIT provides minimal gains because execution time is dominated by external wait states rather than CPU processing.

---

### Lead WordPress Developer

#### Q1: How do you architect a high-throughput Headless WordPress system using Next.js?

**Answer**:
To build a performant Headless WordPress architecture:

1. **API Layer**: Expose endpoints using GraphQL (via `WPGraphQL`) rather than REST API to avoid over-fetching and eliminate dynamic N+1 database queries.
2. **Caching & ISR**: Implement Incremental Static Regeneration (ISR) or On-Demand Revalidation in Next.js. Use WordPress webhooks (`post_save`) to trigger Next.js revalidation endpoints for updated content only.
3. **Database Optimization**: Disable unnecessary plugin queries, optimize `wp_postmeta` indexing, and store expensive operations inside WordPress Transients or Object Cache (Redis / Memcached).

#### Q2: How do you resolve `wp_postmeta` database performance degradation at scale?

**Answer**:
The `wp_postmeta` table suffers from performance degradation due to unindexed `meta_value` searches and unscalable EAV (Entity-Attribute-Value) database design.

* **Custom Database Tables**: Create dedicated custom relational database tables for high-volume custom post types instead of storing custom fields in `wp_postmeta`.
* **Indexing**: Add custom composite indexes on `meta_key` and `post_id` if legacy queries cannot be migrated.
* **Persistent Cache**: Implement Redis Object Cache using `wp_cache_set` and `wp_cache_get` to bypass direct database hits for repeated metadata requests.

---

### Solution Architect

#### Q1: How do you decide between a Modular Monolith and Microservices architecture for an enterprise system?

**Answer**:

* **Modular Monolith**: Preferred when team size is small/medium, domain boundaries are evolving, and operational simplicity is required. Modules maintain strict boundaries with clean internal interfaces, sharing a single deployment pipeline and database.
* **Microservices**: Preferred when distinct domains require independent scaling, distinct technology stacks, independent deployment lifecycles, and decentralized team ownership.
* **Architectural Decision Rule**: Start with a well-structured Modular Monolith. Decouple domain modules via interfaces and events first. Migrate individual modules into independent microservices only when domain boundaries stabilize and scaling demands justify the additional DevOps complexity.

#### Q2: How do you implement Zero-Downtime Deployments for high-traffic PHP/Node applications?

**Answer**:

1. **Blue/Green or Canary Deployments**: Route traffic via NGINX or Application Load Balancers between active (Blue) and new (Green) environment instances.
2. **Database Schema Migrations**: Follow the **Expand-Contract Pattern**:
* *Phase 1 (Expand)*: Add new database columns/tables without deleting existing ones.
* *Phase 2 (Deploy)*: Deploy new application code that writes to both old and new schema.
* *Phase 3 (Contract)*: Remove legacy columns after verifying system stability.


3. **Atomic Symlink Switching**: For PHP deployments (e.g., via Deployer/Capistrano), deploy code to releases directories and update the webroot symlink atomically.

---

### Technical Lead

#### Q1: How do you manage and reduce technical debt while delivering business features?

**Answer**:

1. **Technical Debt Backlog**: Maintain technical debt as transparent user stories in the product backlog alongside product features.
2. **20% Allocation Rule**: Reserve ~20% of every sprint capacity specifically for refactoring, dependency upgrades, and operational improvements.
3. **Boy Scout Rule**: Require engineers to leave any code snippet cleaner than they found it during routine task execution.
4. **Architectural Guardrails**: Enforce automated static analysis (PHPStan, ESLint), unit test coverage thresholds in CI/CD pipelines, and strict peer code review standards.

#### Q2: How do you protect a full-stack application against OWASP Top 10 vulnerabilities?

**Answer**:

* **Injection Attacks (SQLi, Command)**: Enforce prepared statements (PDO / ORM query builders) and ban raw query concatenation.
* **Cross-Site Scripting (XSS)**: Sanitize inputs, escape outputs in templates, enforce Content Security Policy (CSP) HTTP headers, and use React/Angular auto-escaping JSX/templates.
* **Cross-Site Request Forgery (CSRF)**: Enforce `SameSite=Strict/Lax` cookie flags and validate anti-CSRF tokens on state-changing state mutations.
* **Broken Access Control**: Enforce centralized RBAC/ABAC authorization guards at API middleware/controller levels.
