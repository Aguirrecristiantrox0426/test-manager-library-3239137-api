# Security Documentation

## 1. Purpose

This document describes the security principles and requirements that must guide the development and maintenance of the Manager Library backend API.

The objective is to protect user accounts, library information, and business operations while ensuring that each system role has access only to the resources and actions it is authorized to use.

## 2. Authentication

The API must use JSON Web Tokens (JWT) to authenticate users.

Authentication requirements:

- Users must provide valid credentials to access protected resources.
- The API must issue a JWT after successful authentication.
- The token must have a validity period of 8 hours.
- Protected endpoints must validate the token before processing requests.
- Invalid, expired, or missing tokens must not grant access.
- Passwords must never be stored or transmitted as plain text.

## 3. Authorization and Roles

The system defines two authenticated roles:

| Role | Description |
|---|---|
| `admin` | Manages users and has administrative permissions over the system. |
| `librarian` | Performs library management operations, including books, members, and loans. |

Authorization requirements:

- Each protected operation must verify the authenticated user's role.
- Users must not access administrative operations without the required permissions.
- Authorization rules must be consistent across the API.
- A member is not an authenticated system role. Members are registered library users who participate in borrowing operations.

## 4. Password Protection

The backend must protect user credentials.

Requirements:

- Passwords must be stored using a secure password-hashing algorithm.
- Password hashes must never be returned in API responses.
- Passwords must not appear in application logs.
- Authentication errors must not expose sensitive account information.

## 5. Data Protection

The API must protect personal and operational information.

Requirements:

- Only authorized users may access protected information.
- Responses must include only the information required by the operation.
- Sensitive information must not be exposed through error messages.
- Database access must follow the relationships and constraints defined in the data model.
- Database credentials and secrets must not be included in source code or public documentation.

## 6. Input Validation

All incoming requests must be validated before processing.

Validation must include:

- Required fields and data types.
- Valid identifiers and resource references.
- ISBN format and uniqueness requirements.
- Valid loan item quantities.
- Valid image formats and file size limits.
- Valid dates and business constraints.

Invalid requests must be rejected with an appropriate HTTP response without modifying stored information.

## 7. File Upload Security

Book cover uploads must follow these restrictions:

- Accepted formats: JPG and PNG.
- Maximum file size: 2 MB.
- Uploaded files must be validated before being stored.
- File names and storage paths must be handled safely.
- Files must not be treated as trusted solely because of their extensions.

## 8. Database Integrity and Transactions

The API must preserve data consistency during library operations.

Security and integrity requirements:

- Database constraints must protect primary keys, foreign keys, and unique values.
- Loan creation must register loan records and decrease available book copies within the same database transaction.
- If any operation fails, the transaction must be rolled back.
- A loan item must not be returned more than once.
- Loan and return records must not be physically deleted through normal business operations.

## 9. Error Handling and Logging

The API must handle errors without exposing internal implementation details.

Requirements:

- Return appropriate HTTP status codes.
- Avoid exposing passwords, tokens, database credentials, or stack traces in API responses.
- Record relevant security and operational events.
- Protect log files from unauthorized access.
- Avoid storing sensitive authentication data in logs.

## 10. Security Responsibilities

| Area | Responsibility |
|---|---|
| Authentication | Verify user identity through JWT. |
| Authorization | Enforce permissions according to user roles. |
| Password management | Protect credentials through secure hashing. |
| Input validation | Reject invalid or unexpected data. |
| Data protection | Prevent unauthorized information disclosure. |
| Database integrity | Preserve relationships and transactional consistency. |
| File uploads | Validate uploaded images and their size. |
| Error handling | Avoid exposing sensitive internal information. |

## 11. Scope and Implementation Status

This document defines security requirements and recommendations for the Manager Library backend API.

It does not certify that these controls have already been implemented. Their implementation must be verified against the final application code and the project's technical specifications.

Any additional security mechanisms must be documented and aligned with the approved project requirements.

## 12. Related Documentation

- [Backend Architecture](backend-architecture.md)
- [API Endpoints](api-endpoints.md)
- [Business Rules](business-rules.md)
- [Data Model](data-model.md)