# CivicFlow — Architecture

## 1. Architecture Overview

CivicFlow will use a modular monolith architecture.

The application will consist of a frontend application, a backend application and a relational database.

The main components are:

```text
┌───────────────────────┐
│       Web Browser     │
│   Angular Frontend    │
└───────────┬───────────┘
            │
            │ HTTPS / REST API
            ▼
┌───────────────────────┐
│    Spring Boot API    │
│       Backend         │
│                       │
│  ┌─────────────────┐  │
│  │ Authentication  │  │
│  │ Users           │  │
│  │ Tickets         │  │
│  │ Categories      │  │
│  │ Comments        │  │
│  │ Notifications   │  │
│  └─────────────────┘  │
└───────────┬───────────┘
            │
            │ JPA / Hibernate
            ▼
┌───────────────────────┐
│      PostgreSQL       │
│       Database        │
└───────────────────────┘
```

The architecture is intentionally designed as a modular monolith. Business modules are separated within a single backend application, allowing the system to remain relatively simple while maintaining clear boundaries between domains.

---

## 2. Technology Stack

### Backend

* Java 21
* Spring Boot
* Spring Web
* Spring Data JPA
* Hibernate
* Maven

### Frontend

* Angular
* TypeScript
* HTML
* CSS

### Database

* PostgreSQL

### API

* REST
* JSON
* HTTP/HTTPS

### Testing

* JUnit
* Mockito
* Playwright
* Thunder Client

### Development and Deployment

* Git
* GitHub
* Docker
* Docker Compose
* GitHub Actions

---

## 3. Backend Architecture

The backend will be organized into business modules.

The initial package structure will be:

```text
com.civicflow
│
├── auth
│   ├── controller
│   ├── service
│   ├── repository
│   └── dto
│
├── user
│   ├── controller
│   ├── service
│   ├── repository
│   └── dto
│
├── ticket
│   ├── controller
│   ├── service
│   ├── repository
│   └── dto
│
├── category
│   ├── controller
│   ├── service
│   ├── repository
│   └── dto
│
├── comment
│   ├── controller
│   ├── service
│   ├── repository
│   └── dto
│
├── notification
│   ├── controller
│   ├── service
│   ├── repository
│   └── dto
│
└── common
    ├── configuration
    ├── exception
    └── security
```

Business logic shall be implemented in service classes.

Controllers shall be responsible for handling HTTP requests and responses.

Repositories shall provide access to persistent data.

DTOs shall be used to define API request and response models.

---

## 4. Frontend Architecture

The Angular application will be organized into feature-oriented modules and shared functionality.

The initial structure will be:

```text
src/app
│
├── auth
│   ├── login
│   ├── register
│   └── services
│
├── dashboard
│
├── tickets
│   ├── ticket-list
│   ├── ticket-details
│   ├── ticket-create
│   └── services
│
├── comments
│
├── profile
│
├── admin
│   ├── users
│   └── categories
│
├── shared
│   ├── components
│   ├── models
│   └── utilities
│
└── core
    ├── guards
    ├── interceptors
    └── services
```

The frontend shall communicate with the backend exclusively through the REST API.

---

## 5. Database Architecture

PostgreSQL will be used as the primary relational database.

The initial database model will contain the following main entities:

```text
users
roles
categories
tickets
comments
ticket_history
notifications
```

### Main Relationships

```text
roles
  │
  └── users
        │
        ├── tickets
        │     ├── category
        │     ├── comments
        │     ├── ticket_history
        │     └── notifications
        │
        └── comments
```

A user may create multiple tickets.

A ticket belongs to one category.

A ticket may contain multiple comments and history entries.

A ticket may be assigned to an employee.

---

## 6. Main Data Entities

### User

The User entity will contain information such as:

* id
* email
* password
* first name
* last name
* role
* active status
* created date
* updated date

### Role

The Role entity will define the permissions associated with a user.

Initial roles:

* CITIZEN
* EMPLOYEE
* ADMIN

### Category

The Category entity will represent the type of service request.

Example categories:

* Roads and Infrastructure
* Street Lighting
* Waste Collection
* Public Transport
* Green Spaces
* Other

### Ticket

The Ticket entity will contain information such as:

* id
* title
* description
* status
* priority
* category
* creator
* assigned employee
* created date
* updated date
* resolved date

### Comment

The Comment entity will contain:

* id
* ticket
* author
* content
* created date

### Ticket History

The Ticket History entity will record important changes to a ticket.

It will contain information such as:

* id
* ticket
* user
* event type
* previous value
* new value
* created date

### Notification

The Notification entity will represent system notifications related to important ticket events.

---

## 7. API Architecture

The backend will expose versioned REST endpoints.

The initial API structure will be:

```text
/api/v1
```

### Authentication

```text
POST /api/v1/auth/register
POST /api/v1/auth/login
POST /api/v1/auth/logout
```

### Tickets

```text
GET    /api/v1/tickets
POST   /api/v1/tickets
GET    /api/v1/tickets/{id}
PUT    /api/v1/tickets/{id}
DELETE /api/v1/tickets/{id}
PATCH  /api/v1/tickets/{id}/status
PATCH  /api/v1/tickets/{id}/assign
```

### Comments

```text
GET  /api/v1/tickets/{id}/comments
POST /api/v1/tickets/{id}/comments
```

### History

```text
GET /api/v1/tickets/{id}/history
```

### Administration

```text
GET    /api/v1/admin/users
PATCH  /api/v1/admin/users/{id}/status
PATCH  /api/v1/admin/users/{id}/role
DELETE /api/v1/admin/users/{id}
```

### Categories

```text
GET    /api/v1/categories
POST   /api/v1/admin/categories
PUT    /api/v1/admin/categories/{id}
PATCH  /api/v1/admin/categories/{id}/status
```

The API contract may evolve during implementation as technical requirements become more precise.

---

## 8. Authentication and Authorization

The application will use authenticated sessions for protected functionality.

The backend will be responsible for validating authentication and authorization.

Role-based authorization will be enforced on backend endpoints.

The frontend may hide functionality that is unavailable to a user, but frontend restrictions shall not be considered a security mechanism.

The backend shall always verify whether the authenticated user has permission to perform the requested operation.

---

## 9. Request Processing Flow

A typical request from the frontend will follow this flow:

```text
User
 │
 ▼
Angular Frontend
 │
 │ HTTP Request
 ▼
REST Controller
 │
 ▼
Service Layer
 │
 ▼
Repository
 │
 ▼
PostgreSQL
 │
 ▼
Repository
 │
 ▼
Service Layer
 │
 ▼
REST Controller
 │
 │ HTTP Response
 ▼
Angular Frontend
 │
 ▼
User
```

Business rules shall primarily be implemented in the service layer.

---

## 10. Error Handling

The backend shall provide centralized exception handling.

The API shall return appropriate HTTP status codes.

Examples include:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
500 Internal Server Error
```

API errors should follow a consistent response structure.

Example:

```json
{
  "timestamp": "2026-01-01T12:00:00Z",
  "status": 400,
  "error": "Bad Request",
  "message": "Category is required",
  "path": "/api/v1/tickets"
}
```

Sensitive implementation details shall not be exposed to clients.

---

## 11. Docker Architecture

The application will be containerized using Docker.

The initial Docker Compose environment will contain:

```text
┌─────────────────────────┐
│       Docker Host       │
│                         │
│  ┌───────────────────┐  │
│  │ Angular Frontend  │  │
│  └─────────┬─────────┘  │
│            │             │
│            ▼             │
│  ┌───────────────────┐  │
│  │ Spring Boot API   │  │
│  └─────────┬─────────┘  │
│            │             │
│            ▼             │
│  ┌───────────────────┐  │
│  │    PostgreSQL     │  │
│  └───────────────────┘  │
│                         │
└─────────────────────────┘
```

The application should eventually be runnable using:

```bash
docker compose up
```

Environment-specific configuration shall be provided through environment variables or external configuration.

Sensitive values shall not be stored directly in the repository.

---

## 12. Environment Strategy

The project will conceptually use three environments:

### DEV

Used for local development.

### TEST

Used for automated and manual testing.

### PROD

Represents the production configuration.

The environments shall use separate configuration and database settings.

Test data shall not be mixed with production data.

---

## 13. Source Control Strategy

The project will use Git for source control.

The main branch will contain stable project changes.

Feature and testing work should be performed on dedicated branches.

Examples:

```text
feature/user-registration
feature/ticket-management
test/authentication
test/ticket-creation
bugfix/ticket-authorization
```

Changes should be merged through pull requests where appropriate.

---

## 14. Continuous Integration

GitHub Actions will be used for continuous integration.

The initial CI pipeline should perform:

```text
Push / Pull Request
        ↓
Checkout repository
        ↓
Build backend
        ↓
Run backend tests
        ↓
Build frontend
        ↓
Run frontend tests
        ↓
Build Docker images
```

The CI pipeline may be extended as the project evolves.

---

## 15. Architectural Principles

CivicFlow will follow the following principles:

* separation of responsibilities
* modular design
* clear API contracts
* backend-enforced authorization
* validation at application boundaries
* consistent error handling
* testability
* maintainability
* environment separation
* secure handling of sensitive data

The architecture may evolve as new requirements are introduced, but significant architectural changes should be documented.
