# CivicFlow — Functional Requirements

## 1. Authentication and User Management

### FR-001 — User Registration

The system shall allow a new user to create an account by providing:

* first name
* last name
* email address
* password

The system shall validate all required registration fields before creating the account.

The email address shall be unique within the system.

### FR-002 — User Login

The system shall allow registered users to authenticate using their email address and password.

The system shall reject authentication attempts with invalid credentials.

### FR-003 — User Logout

The system shall allow authenticated users to log out of the system.

After logout, the user shall no longer be able to access protected functionality.

### FR-004 — User Profile

The system shall allow authenticated users to view their profile information.

Users shall only be able to modify information that is permitted for their role.

---

## 2. Role-Based Access Control

### FR-005 — User Roles

The system shall support the following roles:

* CITIZEN
* EMPLOYEE
* ADMIN

Each role shall have a defined set of permissions.

### FR-006 — Citizen Permissions

A citizen shall be able to:

* create requests
* view their own requests
* view request details
* add comments to their own requests
* track request status
* view request history for their own requests

A citizen shall not be able to access or modify another citizen's requests.

### FR-007 — Employee Permissions

An employee shall be able to:

* view requests available to their department
* view request details
* update request status
* assign requests
* add comments
* view request history

An employee shall not be able to manage system users.

### FR-008 — Administrator Permissions

An administrator shall be able to:

* manage users
* manage request categories
* view requests
* manage system configuration

---

## 3. Request Management

### FR-009 — Create Request

An authenticated citizen shall be able to create a new request.

A request shall contain at least:

* title
* description
* category

The system shall automatically assign:

* the request creator
* creation date and time
* initial status
* default priority

### FR-010 — View Request

The system shall allow authorized users to view request details.

The request details shall include:

* title
* description
* category
* priority
* status
* creator
* assigned employee
* creation date
* last update date

### FR-011 — Update Request

The system shall allow authorized users to update request information according to their role.

The system shall prevent users from modifying fields that they are not authorized to change.

### FR-012 — Delete Request

The system shall allow deletion of a request only when the user has the required permission.

The system shall prevent unauthorized users from deleting requests.

### FR-013 — Request List

The system shall provide a list of requests available to the authenticated user.

The system shall display relevant information for each request, including:

* request identifier
* title
* category
* priority
* status
* creation date

### FR-014 — Request Filtering

The system shall allow authorized users to filter requests by relevant attributes, including:

* status
* priority
* category
* assigned employee

---

## 4. Request Categories

### FR-015 — Request Categories

The system shall support predefined request categories.

Initial categories shall include:

* Roads and Infrastructure
* Street Lighting
* Waste Collection
* Public Transport
* Green Spaces
* Other

### FR-016 — Category Management

Administrators shall be able to:

* create categories
* edit categories
* deactivate categories
* view existing categories

Deactivated categories shall not be available when creating new requests.

---

## 5. Request Priority

### FR-017 — Request Priority

Each request shall have a priority.

The initial priority levels shall be:

* LOW
* MEDIUM
* HIGH
* CRITICAL

A default priority shall be assigned when a request is created.

Authorized employees shall be able to change the priority.

---

## 6. Request Status

### FR-018 — Request Status

Each request shall have a status.

The initial statuses shall be:

* NEW
* IN_PROGRESS
* WAITING
* RESOLVED
* CLOSED

### FR-019 — Status Transitions

The system shall enforce valid status transitions.

The default workflow shall be:

```text
NEW
 ↓
IN_PROGRESS
 ↓
WAITING
 ↓
IN_PROGRESS
 ↓
RESOLVED
 ↓
CLOSED
```

The system shall reject invalid status transitions.

### FR-020 — Status Authorization

Only authorized users shall be able to change the status of a request.

Citizens shall not be able to arbitrarily change the status of their requests.

---

## 7. Request Assignment

### FR-021 — Assign Request

Authorized employees and administrators shall be able to assign a request to an employee.

The system shall store information about the employee assigned to the request.

### FR-022 — Reassign Request

Authorized users shall be able to change the employee assigned to a request.

The system shall record assignment changes in the request history.

---

## 8. Comments

### FR-023 — Add Comment

Authorized users shall be able to add comments to a request.

A comment shall contain:

* author
* request
* content
* creation date and time

### FR-024 — View Comments

Authorized users shall be able to view comments associated with a request.

Comments shall be displayed in chronological order.

---

## 9. Request History

### FR-025 — Request History

The system shall maintain a history of important changes made to each request.

The history shall include events such as:

* request creation
* status changes
* priority changes
* assignment changes
* category changes
* comments

### FR-026 — View Request History

Authorized users shall be able to view the history of a request.

Each history entry shall contain:

* event type
* user responsible for the action
* date and time
* relevant change information

---

## 10. User Administration

### FR-027 — View Users

Administrators shall be able to view registered users.

The user list shall provide relevant information such as:

* user identifier
* name
* email
* role
* account status
* registration date

### FR-028 — Manage User Status

Administrators shall be able to activate or deactivate user accounts.

Deactivated users shall not be able to authenticate.

### FR-029 — Manage User Roles

Administrators shall be able to change a user's role.

The system shall enforce the permissions associated with the new role.

---

## 11. Validation and Error Handling

### FR-030 — Input Validation

The system shall validate user input before processing requests.

Validation shall include:

* required fields
* field length limits
* valid email format
* password requirements
* valid identifiers
* valid enum values

### FR-031 — Error Responses

The REST API shall return an appropriate HTTP status code when an operation fails.

The response shall contain sufficient information to identify the type of error.

### FR-032 — Unauthorized Access

The system shall reject requests from unauthenticated users when accessing protected resources.

### FR-033 — Forbidden Operations

The system shall reject authenticated users who attempt to perform operations outside their permissions.

---

## 12. Notifications

### FR-034 — Request Notifications

The system shall support notifications related to important request events.

Notifications may be generated when:

* a request is assigned
* a request status changes
* a request receives a comment
* a request is resolved

The initial implementation may provide notifications within the application.

---

## 13. REST API

### FR-035 — REST API

The system shall expose a REST API for supported application operations.

The API shall use HTTP methods according to the operation being performed.

The API shall return JSON responses.

### FR-036 — API Authentication

Protected API endpoints shall require authentication.

The API shall enforce the same role-based access rules as the web application.

### FR-037 — API Error Handling

The API shall return consistent error responses for validation, authentication, authorization and resource errors.

---

## 14. Data Integrity

### FR-038 — Unique Email Address

The system shall prevent multiple user accounts from being created with the same email address.

### FR-039 — Referential Integrity

The system shall maintain relationships between users, requests, categories, comments and request history.

The system shall prevent invalid references between related entities.

### FR-040 — Auditability

Important request changes shall be recorded with information about the user who performed the action and the time of the action.
