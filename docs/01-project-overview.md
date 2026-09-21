# CivicFlow — Project Overview

## 1. Overview

CivicFlow is a web-based system for managing citizen requests and service cases.

The system provides a central place where citizens can submit requests concerning local services and track their progress. Employees can review, process and update requests throughout their lifecycle.

The system also provides administrative features for managing users, categories and other system settings.

## 2. Problem

Citizens may need to report issues or request services related to areas such as:

* roads and infrastructure
* street lighting
* waste collection
* public transport
* green spaces
* other local services

Without a centralized system, requests can be difficult to track, assign and process consistently.

CivicFlow addresses this problem by providing a structured workflow for submitting, processing and monitoring service requests.

## 3. Goals

The main goals of CivicFlow are:

* provide a simple way to submit service requests
* allow citizens to track request progress
* provide employees with tools for processing requests
* maintain a complete history of request activity
* enforce role-based access to system functionality
* provide reliable and consistent request management

## 4. Scope

The initial version of CivicFlow will include:

* user registration and authentication
* role-based access control
* request creation and management
* request categorization
* request prioritization
* request assignment
* status management
* comments
* request history
* user management
* category management
* REST API

## 5. User Roles

The system will support three primary user roles.

### Citizen

A citizen can create and manage their own requests and track their progress.

### Employee

An employee can review and process citizen requests assigned to them or available within their department.

### Administrator

An administrator can manage users, categories and system configuration.

## 6. Request Lifecycle

A request will progress through a defined lifecycle:

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

The exact transition rules will be defined in the functional requirements.

## 7. Future Considerations

Potential future functionality may include:

* email notifications
* file attachments
* advanced reporting
* request prioritization rules
* department management
* audit logging
* mobile application support
