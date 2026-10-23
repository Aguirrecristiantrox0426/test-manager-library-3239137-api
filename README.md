# Manager Library – API

## 1. Project Overview

Manager Library API is the backend component of the Manager Library system, a web-based solution designed to manage the administrative and operational processes of a small library.

The API is responsible for processing application requests, enforcing business rules, managing authentication and authorization, and communicating with the database.

This repository documents the backend component and its technical responsibilities within the overall system.

## 2. Project Objectives

- Provide HTTP endpoints for the application.
- Manage authentication and user authorization.
- Manage books and predefined categories.
- Register and consult library members.
- Process book loans and individual returns.
- Identify overdue loans.
- Provide reports about the most borrowed books.
- Preserve data integrity and enforce business rules.

## 3. Technology Stack

| Component | Technology | Responsibility |
|---|---|---|
| Backend | PHP | Backend programming language. |
| Framework | Laravel | Application structure and HTTP API management. |
| Database | PostgreSQL 16 | Relational data storage. |
| Authentication | JWT | Token-based authentication. |
| API Format | JSON | Data exchange between frontend and backend. |

## 4. Main Responsibilities

The API is organized around the following functional areas:

- **Authentication:** User login and JWT token validation.
- **User Management:** Management of administrator and librarian accounts.
- **Category Management:** Consultation of predefined book categories.
- **Book Management:** Book registration, consultation, updates, and logical deletion.
- **Member Management:** Library member registration and consultation.
- **Loan Management:** Loan registration and validation of lending conditions.
- **Return Management:** Individual return processing and book availability updates.
- **Reports:** Overdue loan consultation and most borrowed book reports.

## 5. Main Business Rules

The backend must enforce the following rules:

- Authentication tokens have a validity period of 8 hours.
- Each loan has a maximum duration of 14 days.
- A member cannot have more than 3 books without returning them.
- Members with overdue loans cannot request additional loans.
- A loan must contain between 1 and 3 items.
- The same book cannot appear more than once in a loan.
- Book availability must be updated consistently during loan and return operations.
- Loan and return records must be preserved.
- Loan status is calculated as `open`, `overdue`, or `closed`.

## 6. API Contract

The backend must follow the HTTP contract defined in the central project specifications.

The API uses the `/api/v1` base path, JSON payloads, and standard HTTP status codes.

Authentication is required for protected endpoints through the following header:

`Authorization: Bearer <token>`

The login and health check endpoints are public.

## 7. Database

The API uses PostgreSQL 16 with the `library` schema.

The main database entities are:

- `user`
- `category`
- `book`
- `member`
- `loan`
- `loan_item`

The data model defines the relationships, constraints, and integrity rules for these entities.

## 8. Repository Documentation

Detailed backend documentation is organized in the following files:

| Document | Description |
|---|---|
| `docs/backend-architecture.md` | Backend architecture and component responsibilities. |
| `docs/api-endpoints.md` | HTTP endpoints and API contract details. |
| `docs/business-rules.md` | Business rules enforced by the backend. |
| `docs/data-model.md` | Database entities, relationships, and constraints. |
| `docs/security.md` | Authentication, authorization, and security requirements. |

## 9. Related Repositories

Manager Library is organized into six repositories:

| Repository | Responsibility |
|---|---|
| Docs | Central specifications and project documentation. |
| API | Backend application and HTTP API. |
| App | React frontend application. |
| Page | Public static presentation website. |
| Infra | Container and database infrastructure. |
| Tool | Utilities and demonstration data seeding. |

## 10. Reference Specifications

The original project specifications are maintained in the central documentation repository:

- [Python Specification](https://github.com/code-sena/test-manager-library-3239137-docs/tree/main/spec-python)
- [.NET Specification](https://github.com/code-sena/test-manager-library-3239137-docs/tree/main/spec-.net)

Both specifications describe the same business requirements and serve as references for adapting the solution to the target stack: React, PHP with Laravel, and PostgreSQL 16.

## 11. Scope Note

This repository documents the backend component of Manager Library. The documentation describes its expected architecture, responsibilities, API contract, business rules, data model, and security requirements.

Technical implementation details must not be considered completed or verified unless they are supported by the corresponding implementation evidence.