# ⚙️ Backend Fundamentals — Web Security Roadmap

> **The backend is the server-side part of a web application that processes requests, applies business logic, manages data, handles authentication, and communicates with databases and other services.**

So far:

```text
HTML → Structure
CSS  → Presentation
HTTP → Communication
```

Now:

```text
Backend → Processing + Logic + Data + Security
```

A simplified web application looks like:

```text
                    INTERNET
                       │
                       ▼
              ┌─────────────────┐
              │     Browser     │
              │ HTML/CSS/JS     │
              └────────┬────────┘
                       │
                    HTTPS
                       │
                       ▼
              ┌─────────────────┐
              │   Web Server    │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │    Backend      │
              │ Application     │
              │     Logic       │
              └───────┬─────────┘
                      │
             ┌────────┼─────────┐
             ▼        ▼         ▼
         Database    Cache    External API
```

---

# 1. 🔌 APIs

## What Is an API?

**API** stands for **Application Programming Interface**.

An API provides a defined way for one piece of software to communicate with another.

In web applications, APIs commonly use HTTP.

For example:

```text
Browser
   │
   │ GET /api/users/42
   ▼
Backend
   │
   │ Query database
   ▼
Database
   │
   │ User data
   ▼
Backend
   │
   │ JSON response
   ▼
Browser
```

---

# 🌐 Web API Example

A request might look like:

```http
GET /api/users/42 HTTP/1.1
Host: example.com
Accept: application/json
```

The server could respond:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": 42,
    "username": "farhan"
}
```

The API provides a structured interface between the client and backend.

---

# 🔹 API Endpoint

An **endpoint** is a specific URL through which an API can be accessed.

Example:

```text
/api/users
/api/users/42
/api/products
/api/orders
/api/login
```

Think:

```text
API
 │
 ├── /users
 ├── /products
 ├── /orders
 └── /login
```

Each endpoint can perform a different function.

---

# 🔹 REST APIs

One common style is **REST**.

Example:

| Method | Endpoint    | Typical purpose  |
| ------ | ----------- | ---------------- |
| GET    | `/users`    | Retrieve users   |
| GET    | `/users/42` | Retrieve user 42 |
| POST   | `/users`    | Create user      |
| PUT    | `/users/42` | Replace user 42  |
| PATCH  | `/users/42` | Modify user 42   |
| DELETE | `/users/42` | Delete user 42   |

These are conventions rather than automatic security rules.

---

# 📦 JSON APIs

Modern web APIs frequently exchange JSON.

Example request:

```json
{
    "username": "farhan",
    "email": "farhan@example.com"
}
```

Example response:

```json
{
    "id": 42,
    "username": "farhan",
    "status": "active"
}
```

JSON is popular because it is:

* Human-readable
* Lightweight
* Easy for programs to process
* Supported by many programming languages

---

# 🔐 API Security

APIs are an important cybersecurity attack surface.

A backend should not blindly trust API requests.

For example:

```http
GET /api/users/42
```

The server should ask:

```text
Who is making the request?
        ↓
Are they authenticated?
        ↓
Are they authorized to access user 42?
        ↓
Is the input valid?
        ↓
Perform operation
```

Not:

```text
Request received
      ↓
Immediately return data
```

---

# 🚨 Common API Security Problems

### 1. Broken Authentication

An API incorrectly handles authentication.

### 2. Broken Authorization

A user can access resources they are not permitted to access.

Example:

```text
User A
  ↓
GET /api/users/42
  ↓
Server returns User B's data
```

This is a serious authorization problem.

### 3. Excessive Data Exposure

The API returns more information than the client actually needs.

Example:

```json
{
    "username": "farhan",
    "email": "example@example.com",
    "password_hash": "...",
    "internal_id": 42
}
```

A properly designed API should avoid exposing sensitive internal data unnecessarily.

### 4. Missing Input Validation

The backend accepts unexpected or malicious input.

### 5. Missing Rate Limiting

An API may be abused through excessive requests.

---

# 🧠 API Security Mental Model

Every API request should pass through:

```text
Request
   ↓
Authentication
   ↓
Authorization
   ↓
Input Validation
   ↓
Business Logic
   ↓
Database / Service
   ↓
Safe Response
```

---

# 2. 🗄️ Databases

A database stores and manages application data.

Examples:

```text
Users
Products
Orders
Messages
Payments
Logs
Sessions
```

A simplified application:

```text
Browser
   │
   ▼
Backend
   │
   ▼
Database
```

---

# 🔹 Relational Databases

Relational databases organize information into **tables**.

Examples include:

* PostgreSQL
* MySQL
* MariaDB
* SQLite
* Microsoft SQL Server

Example:

```text
USERS

┌────┬──────────┬─────────────────────┐
│ ID │ Username │ Email               │
├────┼──────────┼─────────────────────┤
│ 1  │ farhan   │ farhan@example.com  │
│ 2  │ ali      │ ali@example.com     │
└────┴──────────┴─────────────────────┘
```

---

# 🔹 Rows and Columns

A table consists of:

```text
Table
 │
 ├── Columns → properties
 │
 └── Rows    → records
```

Example:

```text
Username
Email
Password Hash
Created At
```

are columns.

One user's complete information is a row.

---

# 🔑 Primary Key

A **primary key** uniquely identifies a record.

Example:

```text
ID
--
1
2
3
```

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT
);
```

---

# 🔗 Relationships

Databases can connect tables.

Example:

```text
Users
  │
  │ user_id
  ▼
Orders
```

One user can have multiple orders.

```text
USERS
┌────┬──────────┐
│ ID │ Username │
├────┼──────────┤
│ 1  │ Farhan   │
└────┴──────────┘

ORDERS
┌────┬─────────┬─────────┐
│ ID │ user_id │ product │
├────┼─────────┼─────────┤
│ 10 │ 1       │ Laptop  │
│ 11 │ 1       │ Mouse   │
└────┴─────────┴─────────┘
```

---

# 🧾 SQL

SQL is commonly used to interact with relational databases.

Example:

```sql
SELECT username
FROM users
WHERE id = 1;
```

Insert:

```sql
INSERT INTO users (username)
VALUES ('farhan');
```

Update:

```sql
UPDATE users
SET username = 'newname'
WHERE id = 1;
```

Delete:

```sql
DELETE FROM users
WHERE id = 1;
```

---

# 🚨 SQL Injection

One of the most important web security concepts involving databases is **SQL Injection**.

It occurs when untrusted input is incorporated into SQL in an unsafe way.

Unsafe conceptual example:

```python
query = "SELECT * FROM users WHERE username = '" + username + "'"
```

If attacker-controlled input changes the intended SQL structure, the resulting query can behave differently from what the developer intended.

The secure approach is generally:

```text
User Input
    ↓
Parameterized Query
    ↓
Database
```

For example, with a database library:

```python
cursor.execute(
    "SELECT * FROM users WHERE username = ?",
    (username,)
)
```

The exact placeholder syntax varies by database/library.

---

# 🔐 Passwords in Databases

A secure application should **not store plaintext passwords**.

Bad:

```text
username: farhan
password: mypassword123
```

Better:

```text
username: farhan
password_hash: <password hash>
```

Modern password storage should use dedicated password-hashing algorithms such as:

```text
Argon2
bcrypt
scrypt
```

with appropriate parameters and salts.

This connects directly to your **Cryptography → Hashing** module.

---

# 🟣 NoSQL Databases

Not every database uses traditional tables.

Examples include:

* MongoDB
* Redis
* Cassandra

A document-oriented database might store:

```json
{
    "username": "farhan",
    "email": "farhan@example.com",
    "role": "user"
}
```

The database model changes, but security principles remain:

```text
Validate Input
      ↓
Control Access
      ↓
Protect Data
      ↓
Use Least Privilege
```

---

# 🔐 Database Security

Important practices include:

* Strong authentication
* Least-privilege database accounts
* Parameterized queries
* Encryption where appropriate
* Secure backups
* Access control
* Monitoring and logging
* Patching
* Secret management
* Avoiding unnecessary exposure to the internet

---

# 3. 🔐 Authentication Concepts

## What Is Authentication?

**Authentication** answers:

> **"Who are you?"**

Examples:

```text
Username + Password
MFA
Passkeys
Security Keys
Certificates
Biometrics
```

---

# 🔑 Authentication vs Authorization

This distinction is extremely important.

### Authentication

```text
Who are you?
```

### Authorization

```text
What are you allowed to do?
```

Example:

```text
Farhan logs in
       ↓
Authentication
       ↓
Identity confirmed
       ↓
Authorization
       ↓
Can Farhan access /admin?
```

A user can be:

```text
Authenticated = YES
Authorized for admin = NO
```

---

# 🧠 Memory Trick

> **Authentication = AuthN = Identity**

> **Authorization = AuthZ = Permission**

---

# 🔐 Password Authentication

A basic login flow:

```text
User
 │
 │ Username + Password
 ▼
Backend
 │
 │ Find user
 ▼
Database
 │
 │ Password hash
 ▼
Backend
 │
 │ Verify password
 ▼
Authentication result
```

The backend should compare the supplied password against the stored password hash using the password-hashing system's verification function.

---

# 🧂 Password Salts

A salt is additional random data used when hashing a password.

Conceptually:

```text
Password
   +
Random Salt
   ↓
Password Hash
```

Different users can have different hashes even if they choose the same password.

Dedicated password-hashing algorithms such as Argon2, bcrypt, and scrypt handle salt-related details as part of their normal use.

---

# 🔢 Multi-Factor Authentication

MFA uses multiple authentication factors.

Common categories:

### Something you know

```text
Password
PIN
```

### Something you have

```text
Phone
Security key
Authenticator device
```

### Something you are

```text
Fingerprint
Face
```

A stronger authentication flow could be:

```text
Password
   ↓
MFA verification
   ↓
Authenticated session
```

---

# 🔑 Session After Login

A successful login normally needs some way for the server to recognize the authenticated user on subsequent requests.

This leads to:

```text
Authentication
       ↓
Session
       ↓
Future requests
```

---

# 4. 🍪 Session Management

HTTP is fundamentally **stateless**.

That means each request is independent unless the application adds a mechanism to maintain state.

Without sessions:

```text
Request 1 → Who are you?
Request 2 → Who are you?
Request 3 → Who are you?
```

A session allows the application to remember the authenticated state.

---

# 🔄 Basic Session Flow

```text
              LOGIN
                │
                ▼
       ┌─────────────────┐
       │    Backend      │
       │ Verify password │
       └────────┬────────┘
                │
                ▼
         Create session
                │
                ▼
       Session identifier
                │
                ▼
            Browser
```

The browser then sends the session identifier with subsequent requests.

Example:

```http
Cookie: session_id=abc123
```

The server can use that identifier to locate the corresponding session.

---

# 🍪 Cookies

Cookies are small pieces of data that browsers store and send according to their rules.

Example:

```http
Set-Cookie: session_id=abc123; Secure; HttpOnly; SameSite=Lax
```

The browser later sends:

```http
Cookie: session_id=abc123
```

---

# 🔐 Important Session Cookie Attributes

## Secure

```text
Secure
```

The cookie should only be sent over secure connections.

---

## HttpOnly

```text
HttpOnly
```

Helps prevent normal JavaScript from directly reading the cookie.

This can reduce certain forms of session-cookie exposure, but it does not make an application immune to XSS.

---

## SameSite

```text
SameSite=Strict
SameSite=Lax
SameSite=None
```

Controls cookie sending behavior in cross-site contexts.

---

# 🔥 Session Lifecycle

A secure session should have a lifecycle:

```text
Login
  ↓
Create Session
  ↓
Use Session
  ↓
Rotate/Refresh as appropriate
  ↓
Logout / Expire
  ↓
Invalidate Session
```

---

# 🚨 Session Security Problems

### 1. Session Fixation

An attacker attempts to cause a victim to use a session identifier known to the attacker.

A common defense is to issue a new session identifier after successful authentication.

---

### 2. Session Hijacking

An attacker obtains a valid session identifier and uses it to impersonate the user.

Possible defenses include:

```text
HTTPS
Secure cookies
HttpOnly
Appropriate SameSite settings
Session expiration
Session invalidation
Protecting tokens
```

---

### 3. Session Leakage

Session identifiers may accidentally appear in:

```text
URLs
Logs
Screenshots
Browser history
Referrer data
Client-side storage
```

Sensitive session identifiers should be handled carefully.

---

### 4. Long-Lived Sessions

A session that never expires increases the potential impact if the session identifier is compromised.

Applications should choose reasonable session lifetimes based on risk and usability.

---

# 🔄 Session vs Token

Two common approaches are:

```text
Session-based authentication
        ↓
Server stores session state
        ↓
Browser sends session identifier
```

and:

```text
Token-based authentication
        ↓
Client sends token
        ↓
Server validates token
```

Neither is automatically secure simply because it is called a "session" or "token."

Security depends on implementation.

---

# 🪪 JWT

**JWT (JSON Web Token)** is one token format commonly used in web applications.

A JWT commonly contains:

```text
Header
Payload
Signature
```

Conceptually:

```text
xxxxx.yyyyy.zzzzz
```

Important:

> **Encoding a JWT does not make its contents secret.**

A JWT is commonly Base64URL-encoded, while its signature provides integrity/authenticity properties depending on how it is used.

Do not put unnecessary sensitive information into a token payload.

---

# 5. 🔄 Web Application Workflows

A web application is not just individual pages.

It is a sequence of connected operations.

Example:

```text
Register
   ↓
Login
   ↓
Create Session
   ↓
Access Dashboard
   ↓
Perform Action
   ↓
Database Update
   ↓
Response
```

Understanding these workflows is extremely important in cybersecurity.

---

# 👤 Registration Workflow

Example:

```text
User
 │
 │ POST /register
 ▼
Backend
 │
 ├── Validate input
 │
 ├── Check username/email
 │
 ├── Hash password
 │
 └── Store account
 │
 ▼
Database
```

Possible response:

```text
201 Created
```

---

# 🔐 Login Workflow

```text
User
 │
 │ POST /login
 │ username + password
 ▼
Backend
 │
 ▼
Database
 │
 │ Retrieve password hash
 ▼
Verify password
 │
 ├── FAIL → Authentication error
 │
 └── SUCCESS
        │
        ▼
   Create session
        │
        ▼
      Response
```

---

# 🏠 Dashboard Workflow

After authentication:

```text
Browser
   │
   │ GET /dashboard
   │ Cookie: session_id=...
   ▼
Backend
   │
   ├── Find session
   ├── Validate session
   ├── Identify user
   └── Check authorization
          │
          ▼
       Database
          │
          ▼
       Response
```

---

# 💳 E-Commerce Workflow

A simplified shopping workflow:

```text
Browse Products
      ↓
Add to Cart
      ↓
Checkout
      ↓
Create Order
      ↓
Payment Processing
      ↓
Payment Confirmation
      ↓
Update Order
      ↓
Send Confirmation
```

There may be several backend services:

```text
Browser
   │
   ▼
API
   │
   ├── Product Service
   ├── Cart Service
   ├── Order Service
   ├── Payment Service
   └── Notification Service
```

---

# 🛡️ Security Checks in a Workflow

Every important operation should be protected.

For example:

```text
POST /api/orders/42/cancel
```

The backend should not simply execute:

```text
Cancel order
```

It should conceptually perform:

```text
Request
   ↓
Authenticate user
   ↓
Validate input
   ↓
Check authorization
   ↓
Check business rules
   ↓
Perform operation
   ↓
Log important event
   ↓
Return response
```

---

# 🚨 Business Logic Security

Not every vulnerability is a technical bug such as SQL injection.

Applications can also have **business logic flaws**.

Example:

```text
User balance = ₹1000

Application allows:

Request 1 → Withdraw ₹1000
Request 2 → Withdraw ₹1000

If both requests are accepted incorrectly:

Balance → -₹1000
```

The underlying problem could involve concurrency, insufficient server-side validation, or incorrect business rules.

The lesson:

> **Security must protect the application's logic, not just its code.**

---

# 🔥 Complete Web Application Example

Imagine a social media application.

```text
                  Browser
                     │
                     │ HTTPS
                     ▼
              ┌──────────────┐
              │   Backend    │
              └──────┬───────┘
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
 Authentication   API Logic     Database
       │             │             │
       └─────────────┼─────────────┘
                     │
                  Response
                     │
                     ▼
                  Browser
```

### Login

```text
POST /login
     ↓
Authentication
     ↓
Session created
     ↓
Cookie returned
```

### View profile

```text
GET /profile
Cookie: session_id=...
     ↓
Session validation
     ↓
Authorization
     ↓
Database
     ↓
Profile returned
```

### Update profile

```text
PATCH /profile
     ↓
Authentication
     ↓
Authorization
     ↓
Input validation
     ↓
Database update
     ↓
Response
```

This is how all your previous topics start connecting.

---

# 🔐 Backend Security Model

A good mental model is:

```text
                REQUEST
                   │
                   ▼
          ┌─────────────────┐
          │ Input Validation │
          └────────┬────────┘
                   ▼
          ┌─────────────────┐
          │ Authentication  │
          └────────┬────────┘
                   ▼
          ┌─────────────────┐
          │ Authorization   │
          └────────┬────────┘
                   ▼
          ┌─────────────────┐
          │ Business Logic  │
          └────────┬────────┘
                   ▼
          ┌─────────────────┐
          │ Database / API  │
          └────────┬────────┘
                   ▼
             SAFE RESPONSE
```

Not every application implements these steps in exactly this order, but these are core security concerns.

---

# 🧩 Connecting Frontend + Backend

You have now studied:

```text
HTML
 ↓
Structure

CSS
 ↓
Presentation

HTTP
 ↓
Communication

Backend
 ↓
Processing

API
 ↓
Application Interface

Database
 ↓
Data Storage

Authentication
 ↓
Identity

Session
 ↓
State

Workflow
 ↓
Complete Application Process
```

Together:

```text
             WEB APPLICATION
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
     FRONTEND                BACKEND
        │                       │
   HTML/CSS/JS             Application
        │                    Logic
        │                       │
        └────── HTTP ──────────┘
                                │
                       ┌────────┴────────┐
                       ▼                 ▼
                    Database          APIs/Services
```

---

# 🕵️ Cybersecurity Perspective

When testing a web application, don't just look at the webpage.

Ask:

### API

```text
What endpoints exist?
What methods do they accept?
What data do they return?
```

### Authentication

```text
How does login work?
How are passwords verified?
Is MFA available?
```

### Authorization

```text
What can this user access?
Can one user access another user's resources?
```

### Sessions

```text
How is the session created?
How is it stored?
When does it expire?
How is logout handled?
```

### Database

```text
What data is stored?
How does the backend query it?
Is input safely handled?
```

### Workflow

```text
What happens from registration
→ login
→ action
→ database update
→ logout?
```

This workflow-oriented thinking is fundamental to web security testing.

---

# 🧪 Hands-On Practice

## Task 1 — Draw a Backend Architecture

Create this diagram yourself:

```text
Browser
   ↓
HTTP/HTTPS
   ↓
Web Server
   ↓
Backend
   ├── Authentication
   ├── Authorization
   ├── API
   └── Business Logic
          ↓
       Database
```

Then add:

```text
Session
Cache
External API
Logging
```

---

# 🧪 Task 2 — Observe a Real API

Open browser DevTools:

```text
DevTools
   ↓
Network
   ↓
Fetch/XHR
```

Use a website you are authorized to inspect.

Look for requests returning JSON.

For each request identify:

```text
Method
URL
Status Code
Request Headers
Request Body
Response Headers
Response Body
Cookies
```

Do **not** modify or attack third-party systems.

---

# 🧪 Task 3 — Build a Local API

Since you are learning Python, eventually build a small local API.

Conceptually:

```text
POST /register
POST /login
GET  /profile
PATCH /profile
POST /logout
```

Architecture:

```text
Python API
    │
    ├── Authentication
    ├── Sessions
    ├── Validation
    └── Database
```

Use only your local environment while learning.

---

# 🧪 Task 4 — Map a Login Workflow

Create a diagram:

```text
Login Form
    ↓
POST /login
    ↓
Backend
    ↓
Find User
    ↓
Verify Password Hash
    ↓
Create Session
    ↓
Set Cookie
    ↓
Dashboard
```

Then identify where each security control belongs.

---

# 🧪 Task 5 — Security Questions

For your local application, ask:

```text
1. What happens if the username does not exist?

2. What happens if the password is incorrect?

3. What happens if the session cookie is missing?

4. What happens if the session expires?

5. What happens if a normal user requests an admin endpoint?

6. What happens if an API receives unexpected input?

7. What happens if a user tries to access another user's data?
```

These questions train you to think like a web security tester.

---

# 🛡️ Backend Security Checklist

### APIs

* [ ] Authenticate API requests when required.
* [ ] Authorize every sensitive operation.
* [ ] Validate input server-side.
* [ ] Avoid unnecessary data exposure.
* [ ] Apply rate limiting where appropriate.
* [ ] Protect sensitive endpoints.

### Databases

* [ ] Use parameterized queries.
* [ ] Hash passwords with a password-hashing algorithm.
* [ ] Apply least privilege.
* [ ] Protect database credentials.
* [ ] Secure backups.
* [ ] Monitor important database activity.

### Authentication

* [ ] Never store plaintext passwords.
* [ ] Use secure password hashing.
* [ ] Consider MFA.
* [ ] Protect authentication endpoints.
* [ ] Avoid unnecessary account-enumeration information.

### Sessions

* [ ] Use HTTPS.
* [ ] Protect session cookies.
* [ ] Use appropriate `Secure`, `HttpOnly`, and `SameSite` settings.
* [ ] Rotate session identifiers where appropriate.
* [ ] Expire and invalidate sessions appropriately.
* [ ] Protect session identifiers as sensitive secrets.

### Workflows

* [ ] Validate every important operation server-side.
* [ ] Enforce authorization.
* [ ] Validate business rules.
* [ ] Log important security events.
* [ ] Handle errors safely.
* [ ] Never trust client-side restrictions.

---

# 🧠 Quick Revision

| Topic          | Core Idea                                             |
| -------------- | ----------------------------------------------------- |
| Backend        | Server-side application logic                         |
| API            | Interface for software communication                  |
| Endpoint       | Specific API resource/address                         |
| Database       | Stores application data                               |
| SQL            | Language commonly used with relational databases      |
| Authentication | Who are you?                                          |
| Authorization  | What are you allowed to do?                           |
| Session        | Maintains state across requests                       |
| Cookie         | Browser-managed data sent with requests               |
| Password Hash  | Secure representation used for password verification  |
| Business Logic | Rules governing application behavior                  |
| Workflow       | Sequence of operations forming an application feature |

---

# ⚡ Memory Tricks

### Authentication vs Authorization

```text
AuthN → Who are you?
AuthZ → What can you do?
```

### Backend

```text
REQUEST
   ↓
VALIDATE
   ↓
AUTHENTICATE
   ↓
AUTHORIZE
   ↓
PROCESS
   ↓
DATABASE
   ↓
RESPONSE
```

### Session

```text
LOGIN
  ↓
SESSION
  ↓
COOKIE / TOKEN
  ↓
REQUESTS
  ↓
LOGOUT / EXPIRY
```

### API

> **Endpoint + Method + Data + Response**

---

# 💼 Interview Questions

### 1. What is a backend?

The backend is the server-side part of an application responsible for processing requests, applying business logic, managing data, authentication, and communicating with databases or other services.

### 2. What is an API?

An API provides a defined interface through which software components communicate.

### 3. What is authentication?

Authentication is the process of verifying the identity of a user or system.

### 4. What is authorization?

Authorization determines what an authenticated user or system is allowed to access or perform.

### 5. What is the difference between authentication and authorization?

```text
Authentication → Who are you?
Authorization   → What are you allowed to do?
```

### 6. Why are sessions needed?

HTTP requests are independent by default. Sessions allow an application to maintain authenticated state across multiple requests.

### 7. Why shouldn't passwords be stored directly?

If plaintext passwords are exposed, attackers immediately obtain the actual credentials. Passwords should instead be stored using an appropriate password-hashing algorithm.

### 8. What is SQL injection?

SQL injection is a vulnerability where attacker-controlled input can alter the intended structure or behavior of a database query. Parameterized queries are a key defense.

### 9. What is an API endpoint?

An endpoint is a specific location/interface through which an API exposes a resource or operation.

### 10. Why is authorization important?

Authentication only establishes identity. Authorization prevents authenticated users from performing actions or accessing resources they are not permitted to use.

---

# 🔥 Final Takeaway

A modern web application can be understood as:

```text
                    USER
                     │
                     ▼
                  BROWSER
                     │
                HTTPS / HTTP
                     │
                     ▼
                  BACKEND
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       AuthN       AuthZ       API
          │          │          │
          └──────────┼──────────┘
                     ▼
               Business Logic
                     │
                     ▼
                 Database
                     │
                     ▼
                  Response
```

The most important cybersecurity principle is:

> **Never trust the client.**

The browser can send:

```text
Modified parameters
Modified headers
Modified cookies
Unexpected data
Different HTTP methods
Direct API requests
```

Therefore, the backend must enforce:

```text
Authentication
       +
Authorization
       +
Input Validation
       +
Business Logic
       +
Secure Data Access
```

Once you understand these concepts, a web application stops looking like **"a webpage"** and starts looking like a collection of **requests, APIs, authentication mechanisms, sessions, business logic, and data flows**.

That is the mindset you need for web security testing.

You now have the **frontend → HTTP → backend** foundation. The next topics can build directly on this by going deeper into **cookies, sessions, authentication, APIs, and common web vulnerabilities**.
