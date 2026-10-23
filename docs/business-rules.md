# Business Rules

## 1. Overview

This document describes the business rules that govern the Manager Library system.

These rules define the conditions and restrictions that the backend must enforce when managing users, books, members, loans, and returns.

The purpose is to ensure consistent operations, protect data integrity, and maintain proper control over library resources.

## 2. User Management Rules

- The system must support two authenticated user roles: `admin` and `librarian`.
- Only authorized administrators can create librarian accounts.
- User authentication must be performed using valid credentials.
- Authenticated users must access only the resources permitted by their assigned roles.
- Authentication tokens must have a validity period of eight hours.

## 3. Category Management Rules

- Book categories are predefined.
- Categories are used to classify registered books.
- Books must reference a valid category.
- The system must preserve category consistency when managing books.

## 4. Book Management Rules

- Every book must have a valid ISBN consisting of 13 digits.
- The ISBN must be unique among active books.
- Every book must belong to a valid category.
- Book cover images must use JPG or PNG format.
- The maximum permitted cover image size is 2 MB.
- Book deletion must be logical rather than physical.
- Books referenced by historical loan records must remain available for historical consultation.

## 5. Member Management Rules

- Library members must be registered before requesting a loan.
- Member information must be stored and associated with loan records.
- Members do not authenticate directly into the application.
- A member's borrowing history must remain associated with their account.

## 6. Loan Management Rules

### 6.1 Loan Duration

- Each loan has a maximum duration of 14 days.
- The due date must be calculated from the loan registration date.
- Loan status must be determined using the due date and return information.

### 6.2 Maximum Number of Books

- A member cannot have more than three books without returning them.
- The system must validate the member's outstanding loan items before creating a new loan.
- Members with overdue loans cannot request additional loans.

### 6.3 Loan Items

- Each loan must contain at least one item.
- Each loan can contain a maximum of three items.
- The same book cannot appear more than once within a single loan.
- Each requested book must have available copies.

### 6.4 Transaction Integrity

- Loan creation and book availability updates must occur within the same database transaction.
- If any operation fails, the transaction must be rolled back.
- The system must prevent inconsistent inventory information.

## 7. Return Management Rules

- Returns must be processed individually for each loan item.
- A loan item cannot be returned more than once.
- Returning a book must update its availability.
- Return operations must preserve the historical information associated with the loan.
- Loan records and return records must not be deleted.

## 8. Loan Status Rules

The system must determine loan status according to the following conditions:

| Status | Condition |
|---|---|
| `open` | The loan has outstanding items and has not exceeded its due date. |
| `overdue` | The loan has outstanding items and its due date has passed. |
| `closed` | All items associated with the loan have been returned. |

Loan status is calculated from loan dates and return information rather than being treated as an independent manual decision.

## 9. Reporting Rules

- The system must provide a list of overdue loans.
- The system must provide a report of the most borrowed books.
- Reports must be based on registered loan information.
- Historical loan information must remain available for reporting purposes.

## 10. Data Integrity Rules

- All relationships between books, categories, members, users, loans, and loan items must preserve referential integrity.
- Required information must be validated before database operations.
- Loan creation and return operations must preserve transactional consistency.
- Historical loan information must not be physically deleted.
- Logical deletion must be applied to books when required.

## 11. Rule Enforcement

The backend is responsible for validating and enforcing these rules through its application and domain logic.

All operations that modify library information must comply with the requirements defined in the central Manager Library specifications.

When a technical detail is not explicitly defined by the specifications, it must be treated as an implementation decision rather than an established business rule.