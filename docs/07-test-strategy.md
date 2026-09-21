# CivicFlow — Test Strategy

## 1. Purpose

The purpose of this test strategy is to define the overall approach to testing CivicFlow.

Testing will be performed throughout the development lifecycle to verify that the system meets its functional and non-functional requirements and that changes do not introduce regressions.

The strategy covers manual testing, API testing, database validation, automated testing, regression testing and exploratory testing.

---

## 2. Testing Objectives

The main testing objectives are:

* verify that implemented functionality meets defined requirements
* identify defects as early as possible
* verify correct behavior for valid and invalid input
* verify role-based access control
* verify API behavior and HTTP responses
* verify data integrity in the database
* verify application behavior across supported browsers and screen sizes
* prevent regressions after changes
* provide evidence that completed features meet their acceptance criteria

---

## 3. Testing Scope

Testing will cover the following areas:

* authentication
* user registration
* user authorization
* user roles and permissions
* user profile
* request creation
* request editing
* request deletion
* request filtering
* request categorization
* request prioritization
* request assignment
* request status transitions
* comments
* request history
* user administration
* category administration
* REST API
* validation and error handling
* database integrity
* responsive behavior
* browser compatibility

---

## 4. Testing Levels

CivicFlow will use multiple levels of testing.

### 4.1 Unit Testing

Unit tests will verify individual classes and methods in isolation.

The main focus will be:

* service layer logic
* validation logic
* business rules
* status transition rules
* authorization-related logic

Backend unit tests will use:

* JUnit
* Mockito

---

### 4.2 Integration Testing

Integration tests will verify interactions between application components.

Examples include:

* service and repository interaction
* REST controller and service interaction
* database operations
* authentication and authorization
* transaction behavior

Integration tests may use:

* Spring Boot Test
* Testcontainers
* PostgreSQL

---

### 4.3 API Testing

REST API endpoints will be tested independently from the frontend.

API testing will verify:

* HTTP methods
* request validation
* response status codes
* response bodies
* authentication
* authorization
* error handling
* boundary conditions
* data persistence

Thunder Client will be used for manual API testing.

Automated API tests may be added as the project evolves.

---

### 4.4 UI Testing

The Angular application will be tested through the user interface.

UI testing will verify:

* navigation
* forms
* validation messages
* request creation
* request management
* role-specific functionality
* error messages
* responsive behavior

Playwright will be used for automated end-to-end testing.

---

### 4.5 Database Testing

Database validation will be performed using SQL queries.

Testing will verify:

* records are created correctly
* records are updated correctly
* relationships between entities are maintained
* required fields are not null
* unique constraints are enforced
* unauthorized operations do not modify protected data
* status and history changes are persisted correctly

PostgreSQL will be used as the test database.

---

## 5. Testing Types

### 5.1 Functional Testing

Functional testing will verify that the system behaves according to the functional requirements.

Each major feature should have corresponding test cases.

---

### 5.2 Negative Testing

Negative tests will verify how the system behaves when invalid or unexpected input is provided.

Examples:

* missing required fields
* invalid email address
* invalid password
* duplicate email
* invalid category
* invalid ticket identifier
* invalid status transition
* unauthorized access
* forbidden operation
* malformed API request

---

### 5.3 Boundary Value Analysis

Boundary values will be tested for fields with defined limits.

Examples include:

* minimum password length
* maximum title length
* maximum description length
* category name length
* comment length

Tests should include values:

* below the minimum
* at the minimum
* just above the minimum
* below the maximum
* at the maximum
* above the maximum

---

### 5.4 Equivalence Partitioning

Input values will be divided into valid and invalid groups.

For example, email validation may use:

```text
Valid:
user@example.com

Invalid:
user
user@
@example.com
user@example
```

One or more representative values from each partition should be tested.

---

### 5.5 Exploratory Testing

Exploratory testing will be used to discover unexpected behavior that may not be covered by predefined test cases.

Exploratory sessions may focus on:

* authentication
* request creation
* role permissions
* status transitions
* navigation
* API behavior
* responsive UI

Exploratory testing should be documented with:

* test session objective
* test area
* observations
* defects found
* follow-up actions

---

### 5.6 Regression Testing

Regression testing will verify that changes do not break existing functionality.

Regression testing should be performed after:

* bug fixes
* changes to authentication
* changes to authorization
* database changes
* API changes
* major UI changes
* changes affecting shared components

---

### 5.7 Smoke Testing

Smoke testing will be performed after a new build is deployed to the test environment.

The smoke suite should verify that the application is generally usable.

Initial smoke tests will include:

* application starts successfully
* user can log in
* user can log out
* authenticated user can access the dashboard
* citizen can create a request
* employee can access assigned requests
* administrator can access administration functionality
* REST API is available

---

### 5.8 Sanity Testing

Sanity testing will be performed after focused changes or bug fixes.

The purpose is to verify that the specific changed functionality works as expected before broader regression testing.

---

## 6. Test Design Techniques

The following test design techniques will be used:

* Equivalence Partitioning
* Boundary Value Analysis
* Decision Table Testing
* State Transition Testing
* Error Guessing
* Exploratory Testing

### State Transition Testing

State transition testing will be particularly important for request status management.

Example:

```text
NEW → IN_PROGRESS
IN_PROGRESS → WAITING
WAITING → IN_PROGRESS
IN_PROGRESS → RESOLVED
RESOLVED → CLOSED
```

Invalid transitions shall also be tested.

---

## 7. Test Case Structure

Each test case should contain at least:

* Test Case ID
* Requirement ID
* Test Case Title
* Preconditions
* Test Data
* Steps
* Expected Result
* Priority
* Test Type
* Test Status

Example:

```text
Test Case ID: TC-AUTH-001
Requirement: FR-002
Title: Login with valid credentials
Priority: High
Test Type: Functional

Preconditions:
A registered active user exists.

Steps:
1. Open the login page.
2. Enter a valid email address.
3. Enter a valid password.
4. Submit the login form.

Expected Result:
The user is authenticated and redirected to the appropriate application page.
```

---

## 8. Test Data

Test data should be created specifically for testing purposes.

Test data may include:

* citizen accounts
* employee accounts
* administrator accounts
* active and inactive users
* request categories
* requests in different statuses
* requests with different priorities
* comments
* invalid input values

Sensitive real-world personal data shall not be used.

---

## 9. Test Environment

The primary test environment will contain:

```text
Frontend
    ↓
Backend API
    ↓
PostgreSQL
```

The test environment should use isolated test data.

The application should be configurable for different environments without changing source code.

---

## 10. Browser and Device Testing

The application should be tested on supported browsers.

Initial browser coverage:

* Google Chrome
* Mozilla Firefox
* Microsoft Edge

Responsive testing should include representative:

* desktop viewport
* tablet viewport
* mobile viewport

The exact browser and viewport matrix may be expanded as the project evolves.

---

## 11. API Testing Strategy

API tests will verify both positive and negative scenarios.

Each important endpoint should have tests covering:

### Positive Scenarios

* valid request
* valid authentication
* valid authorization
* expected response body
* expected status code

### Negative Scenarios

* missing authentication
* insufficient permissions
* invalid input
* missing required fields
* invalid identifiers
* non-existing resources
* invalid state transitions
* duplicate data

Example:

```text
POST /api/v1/tickets

Test:
Create ticket without authentication

Expected:
401 Unauthorized
```

---

## 12. Authorization Testing

Authorization testing is a high-priority area.

Each protected endpoint should be tested with different roles.

Example matrix:

| Endpoint                      | CITIZEN |                 EMPLOYEE | ADMIN |
| ----------------------------- | ------: | -----------------------: | ----: |
| Create ticket                 |     Yes |                       No |    No |
| View own ticket               |     Yes |                      Yes |   Yes |
| View another citizen's ticket |      No | According to permissions |   Yes |
| Change ticket status          |      No |                      Yes |   Yes |
| Assign ticket                 |      No |                      Yes |   Yes |
| Manage users                  |      No |                       No |   Yes |
| Manage categories             |      No |                       No |   Yes |

Authorization tests shall verify both frontend behavior and backend enforcement.

---

## 13. Database Validation Strategy

Database validation will be performed after operations that modify persistent data.

Examples:

### User Registration

Verify that:

* a user record exists
* the email is unique
* the password is not stored in plain text
* the correct role is assigned

### Ticket Creation

Verify that:

* the ticket exists
* the creator is correct
* the category is correct
* the initial status is correct
* the default priority is correct

### Status Change

Verify that:

* the ticket status is updated
* a history record is created
* the responsible user is recorded
* the timestamp is stored

---

## 14. Defect Management

Defects will be documented with sufficient information to reproduce the problem.

Each defect should contain:

* Bug ID
* Title
* Description
* Environment
* Preconditions
* Steps to Reproduce
* Expected Result
* Actual Result
* Severity
* Priority
* Attachments or evidence
* Related Requirement
* Related Test Case
* Status

Example defect title:

```text
[BUG] Citizen can access another user's ticket
```

---

## 15. Severity Levels

Defect severity will be classified as:

### Critical

The defect prevents the system or a critical business function from operating.

### High

The defect significantly affects an important function and there is no reasonable workaround.

### Medium

The defect affects functionality but the main system remains usable.

### Low

The defect has a limited impact on functionality or usability.

Severity describes the impact of a defect on the system.

---

## 16. Test Priority

Test cases will use the following priority levels:

### High

Critical business functionality, security and core user workflows.

### Medium

Important functionality with moderate business impact.

### Low

Low-risk or secondary functionality.

Priority determines the order in which tests should be executed.

---

## 17. Defect Lifecycle

The initial defect lifecycle will be:

```text
OPEN
  ↓
IN_PROGRESS
  ↓
FIXED
  ↓
READY_FOR_RETEST
  ↓
RETEST
  ↓
CLOSED
```

If the defect still occurs during retest:

```text
RETEST
  ↓
REOPENED
  ↓
IN_PROGRESS
```

---

## 18. Retesting

After a defect is marked as fixed, the original failing test case shall be executed again.

The retest shall verify that:

* the original defect no longer occurs
* the expected behavior is restored
* no immediate related issues are introduced

The result shall be documented.

---

## 19. Regression Strategy

Regression testing will focus on areas potentially affected by a change.

Examples:

```text
Authentication change
        ↓
Authentication regression
        ↓
Authorization regression
        ↓
Protected endpoint regression
```

```text
Ticket status change
        ↓
Status transition regression
        ↓
History regression
        ↓
Authorization regression
        ↓
API regression
```

The regression suite will grow as new functionality is implemented.

---

## 20. Automation Strategy

Automation will be introduced gradually.

The initial automation targets will include:

* authentication
* critical API endpoints
* ticket creation
* ticket status changes
* authorization scenarios
* critical end-to-end user journeys

Playwright will be used for browser-based end-to-end automation.

JUnit and Mockito will be used for backend unit tests.

Integration testing may use Testcontainers with PostgreSQL.

Automation should focus on stable, repeatable and high-value scenarios.

---

## 21. CI Testing

Automated tests should be executed as part of the CI pipeline.

The initial pipeline should include:

```text
Pull Request
      ↓
Build
      ↓
Unit Tests
      ↓
Integration Tests
      ↓
Frontend Tests
      ↓
Application Build
```

A pull request should not be merged when required CI checks fail.

---

## 22. Requirements Traceability

Each important requirement should be traceable to one or more test cases.

The planned relationship is:

```text
Requirement
     ↓
Test Case
     ↓
Test Execution
     ↓
Defect
     ↓
Retest
     ↓
Regression
```

Example:

```text
FR-002
  ↓
TC-AUTH-001
  ↓
PASS
```

If a defect is found:

```text
FR-006
  ↓
TC-AUTHZ-005
  ↓
FAIL
  ↓
BUG-001
  ↓
FIX
  ↓
RETEST
  ↓
PASS
```

A Requirements Traceability Matrix will be maintained as testing progresses.

---

## 23. Entry Criteria

Testing of a feature may begin when:

* requirements are defined
* acceptance criteria are available
* the feature is implemented
* the application can be deployed to the test environment
* required test data is available
* known blocking environment issues have been resolved

---

## 24. Exit Criteria

Testing of a feature may be considered complete when:

* planned test cases have been executed
* critical defects are resolved
* high-severity defects are resolved or formally accepted
* failed tests have been investigated
* required retesting has been completed
* relevant regression tests have passed
* required automated tests pass
* test results have been documented

---

## 25. Definition of Done

A feature should not be considered complete only because the code has been implemented.

A feature is considered done when:

* requirements are documented
* acceptance criteria are defined
* implementation is completed
* positive scenarios are tested
* negative scenarios are tested
* relevant API endpoints are tested
* database changes are validated
* defects are documented
* fixed defects are retested
* relevant regression tests pass
* appropriate automated tests are implemented
* documentation is updated

---

## 26. Test Deliverables

The project may produce the following QA artifacts:

```text
Test Strategy
Test Cases
Test Data
API Test Collections
Test Execution Results
Bug Reports
Retest Results
Regression Results
Requirements Traceability Matrix
Automated Test Reports
```

These artifacts will be maintained as the project evolves.
