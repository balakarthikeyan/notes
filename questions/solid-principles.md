# Introduction
SOLID is an acronym that represents five design principles for writing maintainable and scalable software. These principles were introduced by Robert C. Martin and have become fundamental guidelines for object-oriented design. This repository serves as a practical guide to implementing SOLID principles in PHP with real-world examples.

## SOLID Principles

### Single Responsibility Principle (SRP)
The SRP states that a class should have only one reason to change. In other words, a class should have only one responsibility.

A class should have one and only one reason to change, meaning that a class should have only one job.

### Open/Closed Principle (OCP)
The OCP suggests that a class should be open for extension but closed for modification. This means that you can add new functionality without altering existing code.

Objects or entities should be open for extension but closed for modification.

### Liskov Substitution Principle (LSP)
The LSP states that objects of a superclass should be able to replace objects of a subclass without affecting the correctness of the program.

### Interface Segregation Principle (ISP)
The ISP recommends that a class should not be forced to implement interfaces it does not use. It promotes the creation of smaller, more specific interfaces.

### Dependency Inversion Principle (DIP)
The DIP emphasizes that high-level modules should not depend on low-level modules, but both should depend on abstractions. It also introduces the concept that abstractions should not depend on details, but details should depend on abstractions.