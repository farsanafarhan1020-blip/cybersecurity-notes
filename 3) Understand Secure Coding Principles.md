# 💻 Understanding Secure Coding Principles

> **Secure coding** is the practice of writing software in a way that prevents security vulnerabilities, protects sensitive information, and reduces opportunities for attackers to exploit an application.

Software can function correctly while still containing serious security vulnerabilities. For example, a login system may successfully authenticate users but still allow attackers to bypass authentication, steal sessions, or access other users' data.

Secure coding ensures that security is considered while writing and reviewing code, not just during security testing.

It is one of the most important parts of the **Secure Software Development Life Cycle (Secure SDLC)**.

---

## 1. 🛡️ Input Validation

Input validation is the process of checking data received from users, APIs, files, or external systems before an application processes it.

Applications should never automatically trust external input.

Attackers frequently manipulate input fields to inject malicious data, bypass application logic, or exploit vulnerabilities.

### Why Is Input Validation Important?

Consider a registration form that accepts:

- Username.
- Email address.
- Password.
- Age.

A normal user may enter valid information. However, an attacker might submit:

- Extremely long input.
- Unexpected characters.
- Malformed data.
- SQL commands.
- HTML or JavaScript.
- Negative values where only positive values are expected.

If the application processes this input without proper validation, it may become vulnerable.

### Types of Input Validation

#### 1. Type Validation

Ensures that the input has the expected data type.

Example:

A field expecting an integer should not accept arbitrary text.

```python
age = input("Enter your age: ")

try:
    age = int(age)

    if age < 0:
        print("Age cannot be negative")
    else:
        print("Valid age:", age)

except ValueError:
    print("Invalid input. Enter a number.")
```

#### 2. Length Validation

Ensures that input does not exceed an allowed length.

Example:

A username might be restricted to 3–30 characters.

```python
username = input("Enter username: ")

if 3 <= len(username) <= 30:
    print("Username length is valid")
else:
    print("Invalid username length")
```

#### 3. Format Validation

Checks whether input follows an expected format.

Examples:

- Email addresses.
- Phone numbers.
- Dates.
- Postal codes.

```python
import re

email = input("Enter email: ")

pattern = r"^[^@\s]+@[^@\s]+\.[^@\s]+$"

if re.fullmatch(pattern, email):
    print("Email format accepted")
else:
    print("Invalid email format")
```

This is a basic format check, not proof that an email address exists or belongs to the user. Email verification requires an additional verification process.

#### 4. Range Validation

Ensures that numeric input falls within an acceptable range.

Example:

```python
quantity = int(input("Enter quantity: "))

if 1 <= quantity <= 100:
    print("Valid quantity")
else:
    print("Quantity must be between 1 and 100")
```

#### 5. Allowlist Validation

Allowlist validation accepts only explicitly permitted values.

For example, suppose an application allows users to select a payment method:

```python
allowed_methods = ["card", "upi", "netbanking"]

method = input("Enter payment method: ").lower()

if method in allowed_methods:
    print("Payment method accepted")
else:
    print("Invalid payment method")
```

Allowlisting is generally safer than trying to block every possible malicious input.

### Input Validation vs Output Encoding

These two security practices solve different problems.

| Input Validation | Output Encoding |
|---|---|
| Checks whether incoming data is acceptable | Converts data into a safe representation for its output context |
| Rejects invalid or unexpected input | Prevents data from being interpreted as executable content |
| Helps enforce business rules | Helps prevent injection into HTML, JavaScript, and other contexts |

**Example:**

If a user submits:

```html
<script>alert("XSS")</script>
```

Input validation may reject it if HTML is not allowed.

However, if an application intentionally allows users to submit text containing special characters, it should encode that text correctly before displaying it in an HTML page.

### Common Vulnerabilities Related to Improper Input Handling

| Vulnerability | Description |
|---|---|
| SQL Injection | Malicious input changes the meaning of a database query |
| Cross-Site Scripting (XSS) | Untrusted input becomes executable browser-side script |
| Command Injection | Input modifies an operating system command |
| Path Traversal | Input manipulates file paths to access unauthorized files |
| Integer Overflow | Numeric operations exceed the supported range |

### Best Practices

- Validate all untrusted input on the server.
- Use allowlists where practical.
- Enforce appropriate length and range restrictions.
- Use parameterized SQL queries.
- Encode output according to its context.
- Never rely only on client-side validation.
- Treat data from APIs and external services as untrusted.

**Important:** Input validation is not a replacement for parameterized queries, output encoding, or proper authorization.

---

## 2. ⚠️ Secure Error Handling

Error handling is the process of detecting, managing, and responding to errors that occur during application execution.

Secure error handling ensures that errors do not expose sensitive information or create opportunities for attackers.

### Why Is Secure Error Handling Important?

Applications encounter errors for many reasons:

- Invalid user input.
- Database connection failures.
- Authentication failures.
- Missing files.
- Network problems.
- Unexpected application exceptions.

If errors are handled incorrectly, attackers may obtain information about the application's internal structure.

### Example: Insecure Error Message

Imagine an application displays this error:

```text
Database connection failed.

Host: 192.168.1.10
Database: customer_production
Username: db_admin
Password: ********
File: /var/www/app/database.py
Line: 45
```

Even though the password is masked, the error exposes:

- Internal IP address.
- Database name.
- Database username.
- Application file location.
- Internal implementation details.

An attacker could use this information to plan further attacks.

### Secure Error Handling

Instead of displaying internal technical details, show a generic message to the user.

```python
try:
    result = 10 / 0

except ZeroDivisionError:
    print("An unexpected error occurred. Please try again later.")
```

The application can record the technical details in a protected server-side log.

### Error Handling: User vs Developer

| Information | User-Facing Message | Internal Security Log |
|---|---|---|
| General error | Generic message | Detailed exception |
| Database failure | Service temporarily unavailable | Database connection error |
| Authentication failure | Invalid credentials | Relevant authentication event |
| File access failure | Unable to process request | File path and exception details, where appropriate |
| System failure | Something went wrong | Diagnostic information |

Internal logs must also be protected because they can contain sensitive information.

### Important Secure Error Handling Practices

**1. Do Not Expose Sensitive Information**

Avoid displaying:

- Passwords.
- API keys.
- Database credentials.
- Access tokens.
- Secret configuration values.
- Internal stack traces.

**2. Use Proper Exception Handling**

Handle expected errors explicitly rather than allowing uncontrolled application crashes.

**3. Log Security Events**

Record relevant events such as:

- Failed authentication attempts.
- Unauthorized access attempts.
- Unexpected administrative actions.
- Important application failures.

**4. Protect Log Files**

Restrict access to logs and protect them against unauthorized modification or deletion.

**5. Avoid Excessive Logging**

Do not record sensitive information unnecessarily.

### Python Example: Secure Error Handling

```python
import logging

logging.basicConfig(
    filename="application.log",
    level=logging.ERROR
)

try:
    number = int(input("Enter a number: "))
    result = 100 / number
    print("Result:", result)

except ValueError:
    print("Invalid input. Enter a valid number.")

except ZeroDivisionError:
    print("Division by zero is not allowed.")

except Exception:
    logging.exception("Unexpected application error")
    print("An unexpected error occurred.")
```

Here:

- Users receive understandable messages.
- Unexpected errors are recorded internally.
- Detailed exception information is not displayed to users.

For production applications, logging configuration, log access, retention, and sensitive-data handling must also be properly managed.

---

## 3. 🔑 Authentication Security

Authentication is the process of verifying the identity of a user, device, or application.

Secure authentication ensures that only legitimate identities can access protected systems.

Authentication is one of the most important security components in modern applications.

### Authentication vs Authorization

These concepts are closely related but serve different purposes.

| Authentication | Authorization |
|---|---|
| Verifies identity | Determines permissions |
| Answers: Who are you? | Answers: What can you access? |
| Happens before protected access is granted | Determines which actions an authenticated identity can perform |
| Example: Login using username and password | Example: Accessing an administrator dashboard |

**Example:**

Alice logs into an application using her credentials.

- Authentication confirms that Alice is the user.
- Authorization determines whether Alice can access the administrator dashboard.

A successfully authenticated user is not automatically authorized to access every resource.

### Common Authentication Methods

#### 1. Password-Based Authentication

Users authenticate using a username and password.

Passwords must never be stored as plaintext.

Instead, applications should use dedicated password-hashing algorithms.

Common choices include:

- Argon2id.
- bcrypt.
- scrypt.
- PBKDF2.

**Password hashing example using bcrypt:**

```python
import bcrypt

password = b"SecurePassword123"

hashed_password = bcrypt.hashpw(
    password,
    bcrypt.gensalt()
)

print("Password stored securely:", hashed_password)

if bcrypt.checkpw(password, hashed_password):
    print("Authentication successful")
else:
    print("Authentication failed")
```

The application stores the password hash rather than the original password.

A secure password-hashing algorithm uses a salt and is designed to make large-scale password guessing more expensive.

#### 2. Multi-Factor Authentication (MFA)

MFA requires users to provide multiple authentication factors.

The three common factor categories are:

| Factor | Meaning | Example |
|---|---|---|
| Something You Know | Information known by the user | Password or PIN |
| Something You Have | An item possessed by the user | Hardware security key |
| Something You Are | A biometric characteristic | Fingerprint or facial recognition |

Example:

A user logs in using:

1. Password.
2. Authenticator application code.

Even if the password is stolen, the attacker may still need the second authentication factor.

**Important:** Two passwords are not MFA because both belong to the same factor category.

#### 3. Single Sign-On (SSO)

SSO allows users to authenticate once through an identity provider and access multiple authorized applications without repeatedly entering credentials.

Example:

An employee signs in through an organization's identity provider and then accesses email, internal applications, and collaboration tools.

#### 4. Passwordless Authentication

Passwordless authentication allows users to authenticate without entering a traditional password.

Examples:

- Passkeys.
- FIDO2 security keys.
- Platform authenticators.

Passkeys use public-key cryptography. The private key remains protected by the user's device or authenticator, while the service stores the corresponding public key.

### Common Authentication Attacks

| Attack | Description | Defense |
|---|---|---|
| Brute Force | Repeatedly guessing passwords | Rate limiting and MFA |
| Password Spraying | Trying common passwords against multiple accounts | Detection, MFA, and password protection |
| Credential Stuffing | Using previously leaked username-password combinations | MFA and breached-password detection |
| Phishing | Tricking users into revealing credentials | User awareness and phishing-resistant MFA |
| Session Hijacking | Stealing or misusing an authenticated session | Secure cookies and session protection |
| Credential Theft | Obtaining passwords or authentication secrets | Secure storage and secret protection |

### Secure Authentication Best Practices

- Use secure password-hashing algorithms.
- Enforce appropriate password security policies.
- Support MFA, especially for privileged accounts.
- Apply rate limiting to authentication endpoints.
- Protect password-reset mechanisms.
- Use secure session management.
- Avoid revealing whether a username exists during login failures.
- Monitor suspicious authentication patterns.
- Prefer phishing-resistant authentication where practical.

---

## 4. 🚪 Authorization Security

Authorization security ensures that authenticated users can only access resources and perform actions they are permitted to use.

A system with weak authorization may allow users to access another person's data or perform administrative actions without permission.

### Why Is Authorization Security Important?

Consider an online shopping application.

Alice logs in successfully and views her account.

However, if she can change a URL from:

```text
/account/1001
```

to:

```text
/account/1002
```

and access another customer's account, the application has an authorization vulnerability.

Authentication worked correctly, but authorization failed.

This is an example of **Broken Access Control** and may involve an **Insecure Direct Object Reference (IDOR)**.

### Common Authorization Models

#### 1. Role-Based Access Control (RBAC)

Permissions are assigned to roles, and users receive permissions through their assigned roles.

Example:

| Role | Permissions |
|---|---|
| Customer | View own account and place orders |
| Support Agent | View authorized customer support records |
| Administrator | Manage application users and settings |

RBAC simplifies access management when an organization has many users with similar responsibilities.

#### 2. Attribute-Based Access Control (ABAC)

Access decisions are made using attributes associated with the user, resource, action, and environment.

Example:

A user may access a financial report only if:

- Their department is Finance.
- Their role permits report access.
- The report belongs to their authorized business unit.

ABAC supports more context-sensitive access decisions than simple role assignments.

#### 3. Discretionary Access Control (DAC)

The owner of a resource can decide who receives access to that resource.

Example:

A file owner grants another user permission to read a document.

#### 4. Mandatory Access Control (MAC)

Access is controlled by centrally enforced security policies, often using classifications and security clearances.

Example:

A user with an appropriate clearance can access documents classified at an authorized level.

### Common Authorization Vulnerabilities

| Vulnerability | Explanation |
|---|---|
| Broken Access Control | Access restrictions are missing or incorrectly enforced |
| IDOR | Users access resources by manipulating object identifiers |
| Privilege Escalation | Users gain permissions beyond their authorization |
| Missing Function-Level Authorization | Users access restricted functions |
| Horizontal Privilege Escalation | Users access another user's resources at the same privilege level |
| Vertical Privilege Escalation | Users gain access to higher-privilege functions |

### Secure Authorization Practices

**1. Enforce Server-Side Authorization**

Never rely only on frontend checks.

Hiding an administrator button in a webpage does not prevent a user from directly sending an HTTP request to the administrator endpoint.

The server must independently verify permissions.

**2. Apply Least Privilege**

Give users only the permissions they need.

**3. Check Resource Ownership**

Before allowing access to a resource, verify that the user is authorized to access that specific resource.

**4. Deny by Default**

If the system cannot establish that an action is authorized, it should deny the request.

**5. Validate Every Sensitive Request**

Authorization should be checked on every relevant request, not just during login.

### Example: Secure Authorization in Python

```python
def view_account(current_user, account):
    if current_user["id"] != account["owner_id"]:
        print("Access denied")
        return

    print("Account details:", account)
```

This simplified example checks whether the current user owns the account.

In a real application, authorization should be enforced in trusted server-side code using verified identities, properly scoped database queries, and appropriate permission checks.

---

## 5. 🧠 Secure Development Mindset

A secure development mindset means considering security continuously while designing, writing, reviewing, testing, and maintaining software.

Developers should not think only about how an application works. They must also consider how an attacker might misuse it.

A normal developer may ask:

- Does this feature work?
- Does the application produce the expected output?
- Is the interface user-friendly?

A security-conscious developer also asks:

- Can an attacker manipulate this input?
- Can a user access another user's information?
- What happens if authentication fails?
- Can sensitive data leak through error messages?
- What if an API token is stolen?
- Can this feature be abused?

### Important Secure Development Habits

#### 1. Never Trust User Input

Every external input should be treated as untrusted until properly validated and handled.

This includes:

- Form submissions.
- HTTP headers.
- API requests.
- Uploaded files.
- Query parameters.
- Data received from external services.

#### 2. Think Like an Attacker

Consider how a feature could be abused.

Example:

A normal developer builds a file-upload feature.

A security-conscious developer additionally considers:

- Can someone upload executable files?
- Can an uploaded file overwrite an existing file?
- Can the application expose uploaded files publicly?
- Can a malicious file exploit a processing library?
- Can the upload consume excessive storage?

#### 3. Protect Sensitive Data

Avoid exposing sensitive information through:

- Source code.
- Error messages.
- Application logs.
- Public repositories.
- API responses.
- Insecure storage.

Never hardcode production passwords, API keys, or private tokens in source code.

Use an appropriate secrets-management mechanism.

#### 4. Follow Secure Coding Standards

Use established security guidelines instead of relying on assumptions.

Useful references:

- OWASP Secure Coding Practices.
- OWASP Application Security Verification Standard (ASVS).
- CERT Secure Coding Standards.
- CWE (Common Weakness Enumeration).

#### 5. Review Code Before Deployment

Code reviews should examine both functionality and security.

Reviewers should look for:

- Hardcoded credentials.
- Unsafe database queries.
- Missing authorization checks.
- Improper input handling.
- Insecure cryptographic practices.
- Sensitive information exposure.

#### 6. Keep Dependencies Updated

Applications depend on external packages and libraries.

Outdated dependencies may contain publicly known vulnerabilities.

Developers should:

- Use trusted packages.
- Monitor security advisories.
- Scan dependencies.
- Update vulnerable components.
- Remove unused packages.

#### 7. Design for Failure

Assume that errors, attacks, and unexpected conditions will happen.

Applications should fail safely, protect sensitive information, and recover appropriately.

### Security Mindset Comparison

| Normal Development Thinking | Secure Development Thinking |
|---|---|
| Does the login work? | Can login be bypassed or abused? |
| Can users upload files? | Can users upload malicious files? |
| Can users view records? | Can users view only authorized records? |
| Does the API respond? | Does the API enforce authentication and authorization? |
| Does the application handle errors? | Do errors expose sensitive information? |
| Does the application work in production? | Is the production environment securely configured? |

---

## 6. 🌐 Real-World Example: Secure User Registration and Login

Imagine you are developing a web application that allows users to register, log in, and access personal information.

Let's examine how secure coding principles apply.

### Step 1: Input Validation

Validate:

- Username length.
- Email format.
- Password requirements.
- Unexpected input.
- Request data types.

### Step 2: Secure Error Handling

Do not reveal internal system details.

For example:

Instead of:

```text
User does not exist in database.
```

Use a generic authentication message:

```text
Invalid username or password.
```

### Step 3: Authentication Security

- Store passwords using Argon2id or bcrypt.
- Support MFA.
- Protect authentication endpoints.
- Apply rate limiting.
- Secure session handling.

### Step 4: Authorization Security

- Users should access only their own profiles.
- Administrators should have separately controlled permissions.
- Every sensitive request should be authorized server-side.

### Step 5: Continuous Security Review

- Perform code reviews.
- Scan dependencies.
- Test authentication and authorization.
- Monitor suspicious activities.
- Fix discovered vulnerabilities.

### Result

The application does not simply provide working registration and login features. It also protects user identities, personal information, and application resources.

---

## 7. 🧪 Practical Application

To understand secure coding, build or examine a small application in a controlled local environment.

### Task 1: Input Validation

Write a Python program that accepts:

- Username.
- Age.
- Email address.

Validate the input using:

- Type validation.
- Length validation.
- Range validation.
- Format validation.

Test both valid and invalid inputs.

### Task 2: Secure Error Handling

Create a Python program that handles:

- Invalid number input.
- Division by zero.
- Missing files.
- Unexpected exceptions.

Make sure users receive safe error messages while useful diagnostic information is recorded in protected logs.

### Task 3: Authentication Security

Create a basic local authentication demonstration.

Implement:

- Password hashing with bcrypt.
- Password verification.
- Failed login tracking.
- A basic account lockout or rate-limiting demonstration.

Do not use real credentials or expose this demonstration as a production authentication system.

### Task 4: Authorization Security

Create three fictional user roles:

- Customer.
- Support Agent.
- Administrator.

Define which actions each role can perform.

Test whether:

- A customer can access another customer's records.
- A support agent can access administrative functions.
- An administrator can perform authorized management actions.

### Task 5: Security Review

Review your code and identify:

1. Untrusted inputs.
2. Sensitive information.
3. Authentication weaknesses.
4. Missing authorization checks.
5. Unsafe error handling.
6. Potential attack paths.

Document your findings and recommended fixes.

---

## 8. 🧠 Quick Revision

| Concept | Key Point |
|---|---|
| Secure Coding | Writing software to prevent security vulnerabilities |
| Input Validation | Checking incoming data before processing |
| Allowlist Validation | Accepting only explicitly permitted values |
| Output Encoding | Preventing untrusted data from being interpreted as executable content |
| Error Handling | Managing application failures safely |
| Secure Logging | Recording useful events without exposing sensitive information |
| Authentication | Verifying identity |
| Password Hashing | Securely storing password verifiers |
| MFA | Using multiple authentication factor categories |
| Authorization | Determining what an authenticated identity can access |
| RBAC | Assigning permissions through roles |
| ABAC | Making access decisions using attributes |
| Broken Access Control | Allowing unauthorized access to resources or actions |
| Least Privilege | Providing only necessary permissions |
| Secure Development Mindset | Thinking about security throughout development |

### 🔑 Key Takeaways

- Never trust user input or external data automatically.
- Validate input on the server and encode output according to its context.
- Never expose sensitive information through error messages.
- Store passwords using dedicated password-hashing algorithms.
- Authentication verifies identity; authorization controls access.
- Enforce authorization on the server for every sensitive operation.
- Use least privilege and deny access by default.
- Review dependencies and source code regularly.
- Think about how attackers could misuse every application feature.

---

## 9. 🎯 Active Recall — Test Your Understanding

Try answering these questions without looking at the explanations.

<details>
<summary><strong>1. What is secure coding?</strong></summary>

Secure coding is the practice of writing software that prevents vulnerabilities, protects sensitive information, and reduces opportunities for attackers to exploit the application.
</details>

<details>
<summary><strong>2. Why is input validation important?</strong></summary>

It ensures that incoming data meets expected requirements before processing, helping prevent malicious or unexpected input from exploiting application weaknesses.
</details>

<details>
<summary><strong>3. What is the difference between input validation and output encoding?</strong></summary>

Input validation checks whether incoming data is acceptable. Output encoding ensures untrusted data is safely represented in its output context so it is not interpreted as executable content.
</details>

<details>
<summary><strong>4. Why should applications avoid displaying detailed error messages?</strong></summary>

Detailed errors may expose internal paths, database information, system configuration, or other sensitive implementation details that attackers can use.
</details>

<details>
<summary><strong>5. Why should passwords be hashed instead of encrypted or stored as plaintext?</strong></summary>

Dedicated password hashing algorithms are designed to make password guessing expensive. Unlike plaintext storage, a properly implemented password-hashing system does not directly reveal the original password if the database is exposed.
</details>

<details>
<summary><strong>6. What is MFA?</strong></summary>

Multi-Factor Authentication requires users to provide authentication factors from multiple categories, such as something they know and something they have.
</details>

<details>
<summary><strong>7. What is the difference between authentication and authorization?</strong></summary>

Authentication verifies who a user is. Authorization determines which resources and actions that authenticated user is permitted to access.
</details>

<details>
<summary><strong>8. What is Broken Access Control?</strong></summary>

Broken Access Control occurs when an application fails to properly enforce access restrictions, allowing users to perform actions or access resources they are not authorized to use.
</details>

<details>
<summary><strong>9. Why must authorization be checked on the server?</strong></summary>

Frontend checks can be bypassed or manipulated. Server-side authorization ensures that permissions are independently verified before protected operations are performed.
</details>

<details>
<summary><strong>10. What does a secure development mindset mean?</strong></summary>

It means considering potential attacks, vulnerabilities, misuse cases, and security requirements throughout the software development process rather than focusing only on functionality.
</details>

---

> 🛡️ **Security Professional Mindset:** Every input is potentially untrusted, every permission must be justified, every error must be handled safely, and every feature must be examined from an attacker's perspective. Secure software is not created by security testing alone; it is created through secure development practices.
