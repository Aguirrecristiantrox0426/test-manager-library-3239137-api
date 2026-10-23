# API Endpoints

## 1. Overview

This document describes the HTTP endpoints defined for the Manager Library API.

The API provides communication between the React frontend and the backend, allowing clients to manage authentication, users, categories, books, members, loans, returns, and reports.

The API uses JSON for data exchange and follows REST architectural principles.

## 2. General Configuration

| Property | Description |
|---|---|
| API Base Path | `/api/v1` |
| Communication Protocol | HTTP/HTTPS |
| Data Format | JSON |
| Authentication | JWT Bearer Token |
| Database | PostgreSQL 16 |

### 2.1 Authentication Header

Protected endpoints require a valid JWT token in the request header:

`Authorization: Bearer <token>`

### 2.2 Response Format

Responses use JSON and standard HTTP status codes.

Common status codes include:

| Status Code | Description |
|---|---|
| 200 | Request completed successfully. |
| 201 | Resource created successfully. |
| 204 | Request completed without a response body. |
| 400 | Invalid request. |
| 401 | Authentication required or invalid token. |
| 403 | Insufficient permissions. |
| 404 | Resource not found. |
| 409 | Resource conflict or business rule violation. |
| 422 | Request validation failed. |
| 500 | Internal server error. |

## 3. Authentication Endpoints

### 3.1 User Login

| Property | Value |
|---|---|
| Method | POST |
| Endpoint | `/api/v1/auth/login` |
| Authentication | Public |
| Description | Authenticates a registered user and returns an access token. |

The request must include the user's login credentials.

A successful response provides a JWT token with an eight-hour validity period.

## 4. Health Check

### 4.1 System Health

| Property | Value |
|---|---|
| Method | GET |
| Endpoint | `/api/v1/health` |
| Authentication | Public |
| Description | Checks the availability of the API. |

This endpoint provides information about the API's operational status.

## 5. User Management Endpoints

### 5.1 Create User

| Property | Value |
|---|---|
| Method | POST |
| Endpoint | `/api/v1/users` |
| Authentication | Required |
| Authorization | Admin |
| Description | Creates a librarian account. |

User creation must follow the role and account management requirements defined in the project specifications.

## 6. Category Management Endpoints

### 6.1 List Categories

| Property | Value |
|---|---|
| Method | GET |
| Endpoint | `/api/v1/categories` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Retrieves the predefined book categories. |

Categories are predefined and used to classify library books.

## 7. Book Management Endpoints

### 7.1 List Books

| Property | Value |
|---|---|
| Method | GET |
| Endpoint | `/api/v1/books` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Retrieves registered books. |

### 7.2 Create Book

| Property | Value |
|---|---|
| Method | POST |
| Endpoint | `/api/v1/books` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Registers a new book. |

Book registration must validate the ISBN, category, available quantity, and other required attributes.

### 7.3 Update Book

| Property | Value |
|---|---|
| Method | PUT |
| Endpoint | `/api/v1/books/{id}` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Updates an existing book. |

### 7.4 Delete Book

| Property | Value |
|---|---|
| Method | DELETE |
| Endpoint | `/api/v1/books/{id}` |
| Authentication | Required |
| Authorization | Admin |
| Description | Applies logical deletion to a book. |

Books must not be physically deleted when historical loan records reference them.

## 8. Member Management Endpoints

### 8.1 List Members

| Property | Value |
|---|---|
| Method | GET |
| Endpoint | `/api/v1/members` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Retrieves registered library members. |

### 8.2 Create Member

| Property | Value |
|---|---|
| Method | POST |
| Endpoint | `/api/v1/members` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Registers a new library member. |

Members are registered by authorized library personnel and do not authenticate directly into the system.

## 9. Loan Management Endpoints

### 9.1 Create Loan

| Property | Value |
|---|---|
| Method | POST |
| Endpoint | `/api/v1/loans` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Registers a new book loan. |

The operation must validate:

- The member has no overdue loans.
- The member has fewer than three outstanding books before the new loan.
- The loan contains between one and three items.
- No book is repeated within the same loan.
- Each requested book has available copies.

The loan registration and book availability updates must be executed within the same database transaction.

## 10. Overdue Loans Endpoint

### 10.1 List Overdue Loans

| Property | Value |
|---|---|
| Method | GET |
| Endpoint | `/api/v1/loans/overdue` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Retrieves loans that have exceeded their due date. |

Loan status is determined according to the loan due date and return information.

## 11. Reports Endpoints

### 11.1 Most Borrowed Books

| Property | Value |
|---|---|
| Method | GET |
| Endpoint | `/api/v1/reports/most-borrowed` |
| Authentication | Required |
| Authorization | Admin, Librarian |
| Description | Retrieves a report of the most borrowed books. |

The report provides information about book borrowing frequency.

## 12. Endpoint Summary

| Resource | Methods | Endpoint |
|---|---|---|
| Authentication | POST | `/auth/login` |
| Health | GET | `/health` |
| Users | POST | `/users` |
| Categories | GET | `/categories` |
| Books | GET, POST | `/books` |
| Book details | PUT, DELETE | `/books/{id}` |
| Members | GET, POST | `/members` |
| Loans | POST | `/loans` |
| Overdue loans | GET | `/loans/overdue` |
| Reports | GET | `/reports/most-borrowed` |

## 13. Contract Compliance

The API implementation must follow the endpoint definitions, request requirements, response behavior, and authorization rules established in the central Manager Library specifications.

If an implementation detail is not explicitly defined by the API contract, it must be treated as a technical decision rather than an established requirement.