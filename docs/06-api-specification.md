# CivicFlow — API Specification

## 1. Overview

CivicFlow exposes a REST API used by the frontend application and external API clients.

The API uses JSON for request and response bodies.

All API endpoints are versioned under:

```text
/api/v1
```

The API uses standard HTTP methods and status codes.

---

## 2. Base URL

The local development API will be available at:

```text
http://localhost:8080/api/v1
```

The base URL may be different depending on the deployment environment.

---

## 3. Authentication

Protected endpoints require an authenticated user.

Authentication information shall be provided with each protected request.

The exact authentication mechanism will be implemented in the backend security layer.

---

# 4. Authentication Endpoints

## 4.1 Register User

### Endpoint

```http
POST /auth/register
```

### Description

Creates a new user account.

### Authentication

Not required.

### Request Body

```json
{
  "firstName": "John",
  "lastName": "Smith",
  "email": "john.smith@example.com",
  "password": "Password123!"
}
```

### Validation

* first name is required
* last name is required
* email is required
* email must have a valid format
* email must be unique
* password is required
* password must meet the configured security requirements

### Success Response

```http
201 Created
```

```json
{
  "id": 1,
  "firstName": "John",
  "lastName": "Smith",
  "email": "john.smith@example.com",
  "role": "CITIZEN"
}
```

### Possible Errors

```text
400 Bad Request
409 Conflict
```

---

## 4.2 Login

### Endpoint

```http
POST /auth/login
```

### Description

Authenticates a user.

### Authentication

Not required.

### Request Body

```json
{
  "email": "john.smith@example.com",
  "password": "Password123!"
}
```

### Success Response

```http
200 OK
```

The response shall contain the authentication information required for subsequent protected requests.

### Possible Errors

```text
400 Bad Request
401 Unauthorized
```

---

## 4.3 Logout

### Endpoint

```http
POST /auth/logout
```

### Description

Logs the authenticated user out of the application.

### Authentication

Required.

### Success Response

```http
204 No Content
```

### Possible Errors

```text
401 Unauthorized
```

---

# 5. User Endpoints

## 5.1 Get Current User

### Endpoint

```http
GET /users/me
```

### Description

Returns information about the currently authenticated user.

### Authentication

Required.

### Success Response

```http
200 OK
```

```json
{
  "id": 1,
  "firstName": "John",
  "lastName": "Smith",
  "email": "john.smith@example.com",
  "role": "CITIZEN",
  "active": true
}
```

### Possible Errors

```text
401 Unauthorized
404 Not Found
```

---

# 6. Category Endpoints

## 6.1 Get Categories

### Endpoint

```http
GET /categories
```

### Description

Returns active request categories.

### Authentication

Required.

### Success Response

```http
200 OK
```

```json
[
  {
    "id": 1,
    "name": "Roads and Infrastructure"
  },
  {
    "id": 2,
    "name": "Street Lighting"
  }
]
```

---

# 7. Ticket Endpoints

## 7.1 Get Tickets

### Endpoint

```http
GET /tickets
```

### Description

Returns requests available to the authenticated user.

Citizens shall only receive their own requests.

Employees shall receive requests available according to their permissions.

Administrators may access all requests.

### Authentication

Required.

### Query Parameters

Optional parameters:

```text
status
priority
categoryId
assignedTo
page
size
```

Example:

```http
GET /tickets?status=NEW&priority=HIGH
```

### Success Response

```http
200 OK
```

Example:

```json
{
  "content": [
    {
      "id": 101,
      "title": "Broken street light",
      "category": "Street Lighting",
      "priority": "HIGH",
      "status": "NEW",
      "createdAt": "2026-01-01T12:00:00Z"
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 1,
  "totalPages": 1
}
```

### Possible Errors

```text
401 Unauthorized
403 Forbidden
```

---

## 7.2 Create Ticket

### Endpoint

```http
POST /tickets
```

### Description

Creates a new citizen request.

### Authentication

Required.

### Authorization

CITIZEN.

### Request Body

```json
{
  "title": "Broken street light",
  "description": "The street light near the entrance is not working.",
  "categoryId": 2
}
```

### System-Generated Values

The system shall automatically assign:

* ticket identifier
* creator
* creation date
* initial status
* default priority

### Success Response

```http
201 Created
```

Example:

```json
{
  "id": 101,
  "title": "Broken street light",
  "description": "The street light near the entrance is not working.",
  "categoryId": 2,
  "status": "NEW",
  "priority": "MEDIUM",
  "createdBy": 1,
  "createdAt": "2026-01-01T12:00:00Z"
}
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

---

## 7.3 Get Ticket

### Endpoint

```http
GET /tickets/{id}
```

### Description

Returns details of a specific request.

### Authentication

Required.

### Authorization

The authenticated user must have permission to access the request.

### Success Response

```http
200 OK
```

Example:

```json
{
  "id": 101,
  "title": "Broken street light",
  "description": "The street light near the entrance is not working.",
  "category": {
    "id": 2,
    "name": "Street Lighting"
  },
  "status": "NEW",
  "priority": "MEDIUM",
  "createdBy": {
    "id": 1,
    "name": "John Smith"
  },
  "assignedTo": null,
  "createdAt": "2026-01-01T12:00:00Z",
  "updatedAt": "2026-01-01T12:00:00Z"
}
```

### Possible Errors

```text
401 Unauthorized
403 Forbidden
404 Not Found
```

---

## 7.4 Update Ticket

### Endpoint

```http
PUT /tickets/{id}
```

### Description

Updates request information.

### Authentication

Required.

### Authorization

The operation shall only be available to users with the required permissions.

### Request Body

```json
{
  "title": "Broken street light near entrance",
  "description": "The street light near the main entrance is not working.",
  "categoryId": 2
}
```

### Success Response

```http
200 OK
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

---

## 7.5 Delete Ticket

### Endpoint

```http
DELETE /tickets/{id}
```

### Description

Deletes a request when the authenticated user has permission to perform the operation.

### Authentication

Required.

### Success Response

```http
204 No Content
```

### Possible Errors

```text
401 Unauthorized
403 Forbidden
404 Not Found
```

---

## 7.6 Change Ticket Status

### Endpoint

```http
PATCH /tickets/{id}/status
```

### Description

Changes the status of a request.

### Authentication

Required.

### Authorization

EMPLOYEE or ADMIN.

### Request Body

```json
{
  "status": "IN_PROGRESS"
}
```

### Success Response

```http
200 OK
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
```

---

## 7.7 Assign Ticket

### Endpoint

```http
PATCH /tickets/{id}/assign
```

### Description

Assigns a request to an employee.

### Authentication

Required.

### Authorization

EMPLOYEE or ADMIN.

### Request Body

```json
{
  "employeeId": 15
}
```

### Success Response

```http
200 OK
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

---

# 8. Comment Endpoints

## 8.1 Get Ticket Comments

### Endpoint

```http
GET /tickets/{id}/comments
```

### Description

Returns comments associated with a request.

### Authentication

Required.

### Success Response

```http
200 OK
```

Example:

```json
[
  {
    "id": 1,
    "author": "John Smith",
    "content": "The issue has been present since yesterday.",
    "createdAt": "2026-01-01T13:00:00Z"
  }
]
```

### Possible Errors

```text
401 Unauthorized
403 Forbidden
404 Not Found
```

---

## 8.2 Add Comment

### Endpoint

```http
POST /tickets/{id}/comments
```

### Description

Adds a comment to a request.

### Authentication

Required.

### Request Body

```json
{
  "content": "The issue has been present since yesterday."
}
```

### Success Response

```http
201 Created
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

---

# 9. Ticket History

## 9.1 Get Ticket History

### Endpoint

```http
GET /tickets/{id}/history
```

### Description

Returns the history of important changes made to a request.

### Authentication

Required.

### Success Response

```http
200 OK
```

Example:

```json
[
  {
    "id": 1,
    "eventType": "STATUS_CHANGED",
    "previousValue": "NEW",
    "newValue": "IN_PROGRESS",
    "performedBy": "Employee One",
    "createdAt": "2026-01-01T14:00:00Z"
  }
]
```

### Possible Errors

```text
401 Unauthorized
403 Forbidden
404 Not Found
```

---

# 10. Administration Endpoints

## 10.1 Get Users

### Endpoint

```http
GET /admin/users
```

### Description

Returns registered users.

### Authentication

Required.

### Authorization

ADMIN.

### Success Response

```http
200 OK
```

---

## 10.2 Change User Status

### Endpoint

```http
PATCH /admin/users/{id}/status
```

### Description

Activates or deactivates a user account.

### Authentication

Required.

### Authorization

ADMIN.

### Request Body

```json
{
  "active": false
}
```

### Success Response

```http
200 OK
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

---

## 10.3 Change User Role

### Endpoint

```http
PATCH /admin/users/{id}/role
```

### Description

Changes the role assigned to a user.

### Authentication

Required.

### Authorization

ADMIN.

### Request Body

```json
{
  "role": "EMPLOYEE"
}
```

### Success Response

```http
200 OK
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

---

# 11. Category Administration

## 11.1 Create Category

### Endpoint

```http
POST /admin/categories
```

### Description

Creates a new request category.

### Authentication

Required.

### Authorization

ADMIN.

### Request Body

```json
{
  "name": "Public Transport"
}
```

### Success Response

```http
201 Created
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
409 Conflict
```

---

## 11.2 Update Category

### Endpoint

```http
PUT /admin/categories/{id}
```

### Description

Updates an existing request category.

### Authentication

Required.

### Authorization

ADMIN.

### Request Body

```json
{
  "name": "Public Transportation"
}
```

### Success Response

```http
200 OK
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
```

---

## 11.3 Change Category Status

### Endpoint

```http
PATCH /admin/categories/{id}/status
```

### Description

Activates or deactivates a request category.

### Authentication

Required.

### Authorization

ADMIN.

### Request Body

```json
{
  "active": false
}
```

### Success Response

```http
200 OK
```

### Possible Errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

---

# 12. Standard Error Response

The API shall use a consistent error response format.

Example:

```json
{
  "timestamp": "2026-01-01T12:00:00Z",
  "status": 400,
  "error": "Bad Request",
  "message": "Title is required",
  "path": "/api/v1/tickets"
}
```

The exact error response structure may evolve during implementation.

---

# 13. HTTP Status Code Guidelines

The API shall use HTTP status codes according to the result of an operation.

| Status Code | Meaning                                       |
| ----------- | --------------------------------------------- |
| 200         | Successful request                            |
| 201         | Resource successfully created                 |
| 204         | Successful request with no response body      |
| 400         | Invalid request data                          |
| 401         | Authentication required or invalid            |
| 403         | Authenticated user is not authorized          |
| 404         | Resource not found                            |
| 409         | Operation conflicts with current system state |
| 500         | Unexpected server error                       |

---

# 14. API Design Principles

The API shall follow these principles:

* use standard HTTP methods
* use meaningful resource-oriented URLs
* use JSON for data exchange
* use appropriate HTTP status codes
* validate incoming data
* enforce authorization on protected resources
* provide consistent error responses
* version the API
* avoid exposing sensitive information
* maintain consistent naming conventions

The API specification may be updated as implementation details are refined.
