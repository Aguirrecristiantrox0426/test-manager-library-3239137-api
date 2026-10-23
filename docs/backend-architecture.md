# Backend Architecture

## 1. Overview

The Manager Library API is the backend component responsible for managing the system's business logic, processing HTTP requests, enforcing business rules, and accessing persistent data.

The backend is designed to use PHP with the Laravel framework and PostgreSQL 16 as its relational database management system.

Its architecture follows a layered approach to separate responsibilities, improve maintainability, and facilitate future changes.

This document describes the expected architectural organization of the backend. It does not claim that the components have already been implemented.

## 2. Architectural Objectives

The backend architecture aims to:

- Separate business logic from infrastructure concerns.
- Centralize the validation and enforcement of business rules.
- Provide a consistent HTTP API for the React frontend.
- Protect application resources through authentication and authorization.
- Maintain data consistency during loan and return operations.
- Facilitate maintenance, testing, and future extensions.

## 3. Architectural Layers

The proposed backend architecture is organized into four main layers.

### 3.1 Presentation Layer

This layer handles communication between external clients and the backend.

Responsibilities:

- Receive HTTP requests.
- Define and expose API routes.
- Validate incoming request data.
- Invoke the corresponding application services.
- Return structured JSON responses.
- Apply authentication and authorization middleware.

Laravel controllers, routes, request validation, and middleware belong to this layer.

### 3.2 Application Layer

This layer coordinates the execution of application use cases.

Responsibilities:

- Coordinate operations requested by controllers.
- Manage workflows for books, members, loans, and returns.
- Coordinate access to repositories.
- Control transactional operations.
- Return application results to the presentation layer.

Application services are responsible for coordinating operations without duplicating domain rules.

### 3.3 Domain Layer

This layer contains the core business concepts and rules of Manager Library.

Main domain concepts:

- User
- Category
- Book
- Member
- Loan
- Loan Item

Responsibilities:

- Define business entities and their relationships.
- Enforce lending restrictions.
- Validate loan conditions.
- Control individual book returns.
- Determine loan status according to the business rules.

The domain layer must remain independent of HTTP communication and infrastructure details.

### 3.4 Infrastructure Layer

This layer provides the technical mechanisms required by the application.

Responsibilities:

- Establish database connectivity.
- Persist and retrieve application information.
- Implement data access operations.
- Manage database transactions.
- Support secure password hashing and token-related mechanisms.
- Handle file storage for book covers.

PostgreSQL 16 is the database engine, and Laravel provides the framework mechanisms for database access and infrastructure integration.

## 4. Main Backend Components

| Component | Responsibility |
|---|---|
| Routes | Define the available HTTP endpoints. |
| Controllers | Receive requests and coordinate responses. |
| Middleware | Apply authentication and access control. |
| Request Validation | Validate incoming data. |
| Application Services | Coordinate business use cases. |
| Domain Models | Represent the main business entities. |
| Repositories / Data Access | Coordinate persistence operations. |
| Database | Store application information. |

The final internal organization may use Laravel conventions, while preserving the separation of responsibilities described in this document.

## 5. Main Functional Modules

### 5.1 Authentication and User Management

Responsible for user login, token generation, user account management, and role-based access control.

Supported roles:

- `admin`
- `librarian`

### 5.2 Category Management

Provides access to the predefined categories used to classify library books.

### 5.3 Book Management

Responsible for:

- Registering books.
- Consulting book information.
- Updating book records.
- Managing book availability.
- Applying logical deletion.

### 5.4 Member Management

Responsible for registering and consulting library members.

Members do not authenticate directly into the application.

### 5.5 Loan Management

Coordinates loan creation and validates the conditions required to lend books.

A loan must contain between one and three items and cannot include the same book more than once.

### 5.6 Return Management

Processes the return of individual loan items, prevents duplicate returns, and updates book availability.

### 5.7 Reports

Provides information about overdue loans and the most borrowed books.

## 6. Data Persistence

The backend uses PostgreSQL 16 and the `library` database schema.

The main entities are:

- `user`
- `category`
- `book`
- `member`
- `loan`
- `loan_item`

Database operations must preserve referential integrity and consistency.

Loan registration must update book availability and create the corresponding loan records within the same database transaction.

## 7. External Communication

The backend exposes an HTTP API that communicates with the React frontend.

The API uses:

- HTTP methods to identify operations.
- JSON for request and response payloads.
- JWT bearer tokens for protected resources.
- Standard HTTP status codes to communicate operation results.

The API contract is documented separately in `api-endpoints.md`.

## 8. Security Considerations

The architecture must incorporate the following security requirements:

- Authentication through JWT tokens.
- Token validity of eight hours.
- Role-based access control.
- Secure password hashing.
- Validation of incoming request data.
- Protection of restricted endpoints.
- Validation of uploaded book cover files.

Detailed security requirements are described in `security.md`.

## 9. Architecture Principles

The backend architecture follows these principles:

- **Separation of Concerns:** Each layer has clearly defined responsibilities.
- **Single Responsibility:** Components should focus on a specific responsibility.
- **Low Coupling:** Changes in one layer should have minimal impact on others.
- **High Cohesion:** Related functionality should remain grouped together.
- **Data Integrity:** Database operations must preserve consistent information.
- **Security by Design:** Security requirements must be considered throughout the architecture.

## 10. Architecture Summary

The proposed architecture establishes a clear separation between HTTP communication, application workflows, business rules, and infrastructure services.

This organization provides a maintainable foundation for implementing the Manager Library backend with Laravel and PostgreSQL 16.