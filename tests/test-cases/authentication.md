# CivicFlow — Authentication Test Cases

## 1. Registration

### TC-AUTH-001 — Register with valid data

**Requirement:** FR-001
**Priority:** High
**Test Type:** Functional / Positive

**Preconditions:**

* Registration page is accessible.
* The email address is not registered in the system.

**Test Data:**

```text
First name: John
Last name: Smith
Email: john.smith@example.com
Password: Password123!
```

**Steps:**

1. Open the registration page.
2. Enter a valid first name.
3. Enter a valid last name.
4. Enter a valid email address.
5. Enter a valid password.
6. Submit the registration form.

**Expected Result:**

* The account is created successfully.
* The user receives the CITIZEN role.
* The user is redirected to the appropriate application page or login page.
* The account exists in the database.

**Status:** Passed

---

### TC-AUTH-002 — Register with empty first name

**Requirement:** FR-001
**Priority:** Medium
**Test Type:** Validation / Negative

**Preconditions:**

* Registration page is accessible.

**Steps:**

1. Open the registration page.
2. Leave the first name field empty.
3. Enter a valid last name.
4. Enter a valid email address.
5. Enter a valid password.
6. Submit the registration form.

**Expected Result:**

* Registration is rejected.
* A validation message indicates that first name is required.
* No user account is created.

**Status:** Passed

---

### TC-AUTH-003 — Register with empty last name

**Requirement:** FR-001
**Priority:** Medium
**Test Type:** Validation / Negative

**Steps:**

1. Open the registration page.
2. Enter a valid first name.
3. Leave the last name field empty.
4. Enter a valid email address.
5. Enter a valid password.
6. Submit the registration form.

**Expected Result:**

* Registration is rejected.
* A validation message indicates that last name is required.
* No user account is created.

**Status:** Passed

---

### TC-AUTH-004 — Register with empty email

**Requirement:** FR-001
**Priority:** High
**Test Type:** Validation / Negative

**Steps:**

1. Open the registration page.
2. Enter a valid first name.
3. Enter a valid last name.
4. Leave the email field empty.
5. Enter a valid password.
6. Submit the registration form.

**Expected Result:**

* Registration is rejected.
* A validation message indicates that email is required.
* No user account is created.

**Status:** Passed

---

### TC-AUTH-005 — Register with invalid email format

**Requirement:** FR-001
**Priority:** High
**Test Type:** Validation / Negative

**Test Data:**

```text
Email: john.smith
```

**Steps:**

1. Open the registration page.
2. Enter valid first name and last name.
3. Enter an invalid email address.
4. Enter a valid password.
5. Submit the registration form.

**Expected Result:**

* Registration is rejected.
* A validation message indicates that the email format is invalid.
* No user account is created.

**Status:** Passed

---

### TC-AUTH-006 — Register with duplicate email

**Requirement:** FR-001 / FR-038
**Priority:** High
**Test Type:** Functional / Negative

**Preconditions:**

* An account already exists with `john.smith@example.com`.

**Steps:**

1. Open the registration page.
2. Enter valid registration data.
3. Use the email address `john.smith@example.com`.
4. Submit the registration form.

**Expected Result:**

* Registration is rejected.
* The system informs the user that the email address is already registered.
* No duplicate account is created.

**Expected API Response:**

```text
409 Conflict
```

**Status:** Passed

---

### TC-AUTH-007 — Register with empty password

**Requirement:** FR-001
**Priority:** High
**Test Type:** Validation / Negative

**Steps:**

1. Open the registration page.
2. Enter a valid first name.
3. Enter a valid last name.
4. Enter a valid email address.
5. Leave the password field empty.
6. Submit the registration form.

**Expected Result:**

* Registration is rejected.
* A validation message indicates that password is required.
* No user account is created.

**Status:** Passed

---

### TC-AUTH-008 — Register with password below minimum length

**Requirement:** FR-001 / NFR-005
**Priority:** High
**Test Type:** Boundary Value / Negative

**Test Data:**

```text
Password: short
```

**Steps:**

1. Open the registration page.
2. Enter valid registration data.
3. Enter a password shorter than the configured minimum length.
4. Submit the registration form.

**Expected Result:**

* Registration is rejected.
* A validation message indicates that the password does not meet the required rules.
* No user account is created.

**Status:** Passed

---

### TC-AUTH-009 — Register with password at minimum length

**Requirement:** FR-001 / NFR-005
**Priority:** Medium
**Test Type:** Boundary Value

**Steps:**

1. Open the registration page.
2. Enter valid registration data.
3. Enter a password exactly equal to the configured minimum length.
4. Submit the registration form.

**Expected Result:**

* The password is accepted if it satisfies all configured password rules.
* Registration succeeds.

**Status:** Passed

---

### TC-AUTH-010 — Register with leading and trailing spaces in email

**Requirement:** FR-001 / FR-030
**Priority:** Medium
**Test Type:** Boundary / Negative

**Test Data:**

```text
Email: " john.smith@example.com "
```

**Steps:**

1. Open the registration page.
2. Enter valid registration data.
3. Enter an email address containing leading and trailing spaces.
4. Submit the registration form.

**Expected Result:**

The system shall handle whitespace consistently according to the defined validation rules.

The behavior shall be documented and verified at both the UI and API levels.

**Status:** Passed

---

## 2. Login

### TC-AUTH-011 — Login with valid credentials

**Requirement:** FR-002
**Priority:** High
**Test Type:** Functional / Positive

**Preconditions:**

* A registered active user exists.

**Test Data:**

```text
Email: john.smith@example.com
Password: Password123!
```

**Steps:**

1. Open the login page.
2. Enter the registered email address.
3. Enter the correct password.
4. Submit the login form.

**Expected Result:**

* Authentication succeeds.
* The user is redirected to the appropriate authenticated area.
* Protected functionality becomes available.

**Status:** Passed

---

### TC-AUTH-012 — Login with invalid password

**Requirement:** FR-002
**Priority:** High
**Test Type:** Negative

**Steps:**

1. Open the login page.
2. Enter a registered email address.
3. Enter an incorrect password.
4. Submit the login form.

**Expected Result:**

* Authentication fails.
* The user remains unauthenticated.
* An appropriate error message is displayed.

**Expected API Response:**

```text
401 Unauthorized
```

**Status:** Passed

**Defect:** DEF-AUTH-001


---

### TC-AUTH-013 — Login with non-existing email

**Requirement:** FR-002
**Priority:** High
**Test Type:** Negative

**Steps:**

1. Open the login page.
2. Enter an email address that does not exist.
3. Enter a password.
4. Submit the login form.

**Expected Result:**

* Authentication fails.
* The user remains unauthenticated.
* An appropriate error response is returned.

**Expected API Response:**

```text
401 Unauthorized
```

**Status:** Not Run

---

### TC-AUTH-014 — Login with empty email

**Requirement:** FR-002 / FR-030
**Priority:** High
**Test Type:** Validation / Negative

**Steps:**

1. Open the login page.
2. Leave the email field empty.
3. Enter a password.
4. Submit the login form.

**Expected Result:**

* Login is rejected.
* A validation message indicates that email is required.
* No authentication request should be submitted if client-side validation prevents submission.

**Status:** Not Run

---

### TC-AUTH-015 — Login with empty password

**Requirement:** FR-002 / FR-030
**Priority:** High
**Test Type:** Validation / Negative

**Steps:**

1. Open the login page.
2. Enter a valid email address.
3. Leave the password field empty.
4. Submit the login form.

**Expected Result:**

* Login is rejected.
* A validation message indicates that password is required.

**Status:** Not Run

---

### TC-AUTH-016 — Login with inactive account

**Requirement:** FR-002 / FR-028
**Priority:** High
**Test Type:** Authorization / Negative

**Preconditions:**

* A registered user exists.
* The user account has been deactivated by an administrator.

**Steps:**

1. Open the login page.
2. Enter the inactive user's email address.
3. Enter the correct password.
4. Submit the login form.

**Expected Result:**

* Authentication is rejected.
* The inactive user cannot access protected functionality.

**Status:** Not Run

---

## 3. Logout

### TC-AUTH-017 — Logout successfully

**Requirement:** FR-003
**Priority:** High
**Test Type:** Functional / Positive

**Preconditions:**

* The user is authenticated.

**Steps:**

1. Log in with valid credentials.
2. Open the authenticated application area.
3. Select the logout option.

**Expected Result:**

* The user is logged out.
* Protected functionality is no longer accessible.
* The user is redirected to an unauthenticated page.

**Status:** Not Run

---

### TC-AUTH-018 — Access protected resource after logout

**Requirement:** FR-003 / FR-032
**Priority:** Critical
**Test Type:** Security / Negative

**Preconditions:**

* The user has successfully logged in.
* The user has successfully logged out.

**Steps:**

1. Attempt to access a protected page after logout.
2. Attempt to call a protected API endpoint after logout.

**Expected Result:**

* The protected page cannot be accessed.
* The protected API request is rejected.

**Expected API Response:**

```text
401 Unauthorized
```

**Status:** Not Run

---

## 4. Authorization

### TC-AUTHZ-001 — Citizen cannot access another user's ticket

**Requirement:** FR-006 / FR-033
**Priority:** Critical
**Test Type:** Security / Negative

**Preconditions:**

* Citizen A owns ticket 101.
* Citizen B is authenticated.

**Steps:**

1. Authenticate as Citizen B.
2. Attempt to access ticket 101.
3. Send a direct API request to the ticket endpoint.

**Expected Result:**

* Citizen B cannot access Citizen A's ticket.
* The backend rejects the unauthorized request.

**Expected API Response:**

```text
403 Forbidden
```

**Status:** Passed

---

### TC-AUTHZ-002 — Citizen cannot change ticket status

**Requirement:** FR-020 / FR-033
**Priority:** Critical
**Test Type:** Security / Negative

**Preconditions:**

* A citizen owns an existing ticket.

**Steps:**

1. Authenticate as a citizen.
2. Send a request to change the ticket status.
3. Attempt to change the status to `IN_PROGRESS`.

**Expected Result:**

* The operation is rejected.
* The ticket status remains unchanged.

**Expected API Response:**

```text
403 Forbidden
```

**Status:** Not Run

---

### TC-AUTHZ-003 — Employee cannot manage users

**Requirement:** FR-007 / FR-033
**Priority:** High
**Test Type:** Security / Negative

**Preconditions:**

* An employee account exists.

**Steps:**

1. Authenticate as an employee.
2. Attempt to access the user administration endpoint.
3. Attempt to modify another user's account.

**Expected Result:**

* The operation is rejected.
* No user data is modified.

**Expected API Response:**

```text
403 Forbidden
```

**Status:** Not Run

---

### TC-AUTHZ-004 — Administrator can manage users

**Requirement:** FR-008 / FR-027 / FR-028
**Priority:** High
**Test Type:** Authorization / Positive

**Preconditions:**

* An administrator account exists.

**Steps:**

1. Authenticate as an administrator.
2. Open the user administration area.
3. Retrieve the user list.
4. Change the status of a user account.

**Expected Result:**

* The administrator can access user administration.
* The user list is returned.
* The selected user status is updated successfully.

**Status:** Not Run


