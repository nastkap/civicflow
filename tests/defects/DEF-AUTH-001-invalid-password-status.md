# DEF-AUTH-001 — Invalid password returns incorrect HTTP status

## Summary

Login with valid email and invalid password returns HTTP 409 Conflict instead of an authentication failure response.

## Environment

* Application: CivicFlow
* Endpoint: `POST /api/v1/auth/login`
* Environment: Local development
* Backend port: 8081

## Preconditions

* A registered and active user exists:

  * Email: `anna.kowalska@example.com`
  * Password: `Password123`

## Steps to Reproduce

1. Send a POST request to `/api/v1/auth/login`.
2. Use a valid registered email.
3. Use an incorrect password.
4. Submit the request.

## Request

```json
{
  "email": "anna.kowalska@example.com",
  "password": "WrongPassword123"
}
```

## Expected Result

The API should return HTTP 401 Unauthorized for invalid credentials.

## Actual Result

The API returned:

**HTTP 409 Conflict**

## Evidence

The backend log confirmed that the user was retrieved from the database before the authentication failure occurred.

## Severity

**Medium**

## Priority

**High**

## Status

**CLOSED**

## Resolution

The login exception handling was updated to return HTTP 401 Unauthorized for invalid credentials instead of HTTP 409 Conflict.

## Retest

TC-AUTH-012 was retested after the fix.

**Result:** Passed

**Observed response:** HTTP 401 Unauthorized
