# Streamy Application Design and Refactoring
## UML Class Diagrams, Dependencies, and Interfaces

### 1. Initial Tightly Coupled Architecture

The initial registration design passes email and username separately through several method signatures. The controller also depends directly on the concrete registration service.

```mermaid
---
title: Initial Tightly Coupled Classes
---
classDiagram
    class UserController {
        -CreateUserService _createUserService
        +UserController(CreateUserService createUserService)
        +Create(string email, string username) User
    }
    class CreateUserService {
        -UserModel _userModel
        -SendWelcomeEmailService _sendWelcomeEmailService
        -Kafka _kafka
        +CreateUserService(UserModel um, SendWelcomeEmailService es, Kafka k)
        +Call(string email, string username) User
        -CheckActiveUsers(List~User~ users) bool
    }
    class UserModel {
        +FindUsersByEmail(string email) List~User~
        +CreateUser(string email, string username) User
    }
    class User {
        +int UserId
        +string Username
        +string Email
    }
    class SendWelcomeEmailService {
        +Call(string email, string username) bool
    }
    class Kafka {
        +PublishUserCreatedEvent(string email, string username) bool
    }

    UserController ..> CreateUserService : depends on
    CreateUserService ..> UserModel : depends on
    UserModel ..> User : depends on
    CreateUserService ..> SendWelcomeEmailService : depends on
    CreateUserService ..> Kafka : depends on
```

### 2. Code Smell Analysis

**Code Smell 1 — Data Clumps**

The `email` and `username` parameters repeatedly appear together across multiple method signatures. This creates duplicated parameter groups and makes future changes harder.

**Refactoring:** Introduce `CreateUserRequest` to encapsulate registration data and provide a `Validate()` method. The email and username are now passed together as one request object. Downstream notification services consume the `User` entity instead of separate primitive parameters.

**Code Smell 2 — Tight Coupling**

`UserController` initially depends directly on the concrete `CreateUserService`. This makes the controller dependent on a specific implementation.

**Refactoring:** Introduce the `ICreateUserService` interface. The controller depends on the interface, while `CreateUserService` implements it. This applies the Dependency Inversion Principle and reduces direct coupling.

### 3. Final Decoupled Architecture

```mermaid
---
title: Final Decoupled Architecture
---
classDiagram
    class UserController {
        <<Class>>
        -ICreateUserService _createUserService
        +UserController(ICreateUserService createUserService)
        +Create(string email, string username) User
    }
    class CreateUserRequest {
        <<Class>>
        +string Email
        +string Username
        +Validate() bool
    }
    class ICreateUserService {
        <<Interface>>
        +Call(CreateUserRequest createUserRequest) User
    }
    class CreateUserService {
        <<Class>>
        -UserModel _userModel
        -SendWelcomeEmailService _sendWelcomeEmailService
        -Kafka _kafka
        +CreateUserService(UserModel um, SendWelcomeEmailService es, Kafka k)
        +Call(CreateUserRequest createUserRequest) User
        -CheckActiveUsers(List~User~ users) bool
    }
    class UserModel {
        <<Class>>
        +FindUsersByEmail(string email) List~User~
        +CreateUser(string email, string username) User
    }
    class User {
        <<Class>>
        +int UserId
        +string Username
        +string Email
    }
    class SendWelcomeEmailService {
        <<Class>>
        +Call(User user) bool
    }
    class Kafka {
        <<Class>>
        +PublishUserCreatedEvent(User user) bool
    }

    UserController ..> CreateUserRequest : depends on
    UserController ..> ICreateUserService : depends on
    CreateUserService ..|> ICreateUserService : implements
    CreateUserService ..> UserModel : depends on
    UserModel ..> User : depends on
    CreateUserService ..> SendWelcomeEmailService : depends on
    CreateUserService ..> Kafka : depends on
```

### 4. Implementation Note

These diagrams describe a proposed Streamy registration architecture. The current repository contains architectural documentation rather than an implemented backend, so the classes and method signatures shown here are illustrative and have not been verified against production source code.