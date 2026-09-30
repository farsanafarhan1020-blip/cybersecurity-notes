# 🔐 Secure Development Principles

> **Secure development** means designing and writing software in a way that reduces vulnerabilities, protects data, and prevents attackers from abusing application functionality.

A secure application does not simply “work.” It should also behave safely when users provide unexpected input, access restricted resources, or attempt to misuse the application.

A simple security mindset is:

```text
Input → Validate → Authenticate → Authorize → Process Safely → Output Safely
```

The five important principles in this topic are:

1. 🛡️ Input Validation
2. 📤 Output Handling
3. 🔑 Authentication Awareness
4. 🚪 Authorization Awareness
5. 🧠 Secure Coding Mindset

---

# 1️⃣ Input Validation

## 🧩 What Is Input Validation?

**Input validation** is the process of checking data received from users, clients, APIs, files, or other systems before the application processes it.

> **Never assume user input is trustworthy.**

Input can come from:

* Login forms
* Search boxes
* URL parameters
* HTTP headers
* Cookies
* JSON APIs
* File uploads
* Command-line arguments
* Uploaded files
* Third-party APIs

### Example

Suppose an application expects:

```text
Age: 21
```

An attacker might send:

```text
<script>alert(1)</script>
```

or:

```text
../../../../etc/passwd
```

or:

```text
' OR '1'='1
```

The application should not blindly process these values.

---

# 🎯 Why Input Validation Matters

Poor input handling can contribute to vulnerabilities such as:

| Vulnerability       | Example                                            |
| ------------------- | -------------------------------------------------- |
| SQL Injection       | Malicious SQL inserted into database queries       |
| XSS                 | Malicious HTML/JavaScript inserted into output     |
| Command Injection   | User input reaches OS commands                     |
| Path Traversal      | `../../secret.txt`                                 |
| File Upload Attacks | Dangerous files uploaded                           |
| SSRF                | Server manipulated into making unintended requests |
| Integer/Type Issues | Unexpected numeric or type values                  |

---

# 🧱 Types of Input Validation

## 1. Type Validation

Make sure the value has the expected type.

```python
age = int(input("Age: "))
```

A better application should also handle invalid input:

```python
try:
    age = int(input("Age: "))
except ValueError:
    print("Invalid age")
```

---

## 2. Length Validation

Limit how much data can be submitted.

```python
username = input("Username: ")

if len(username) > 30:
    print("Username too long")
```

This helps prevent unexpected input and resource abuse.

---

## 3. Range Validation

Check whether a value falls within an acceptable range.

```python
age = 25

if not 0 <= age <= 120:
    print("Invalid age")
```

---

## 4. Format Validation

Check whether data follows the expected format.

Examples:

```text
Email → user@example.com
IPv4 → 192.168.1.10
Date → 2026-09-30
Username → allowed character pattern
```

Regular expressions can sometimes help with structured formats.

---

## 5. Allowlist Validation

An **allowlist** defines what is permitted.

Example:

```python
allowed_roles = ["user", "editor"]

if role not in allowed_roles:
    print("Invalid role")
```

This is generally safer than trying to create a huge blacklist of everything that is considered dangerous.

### ❌ Weak approach

```text
Block "admin"
Block "root"
Block "administrator"
...
```

Attackers may find another representation.

### ✅ Better approach

```text
Allow only:
user
editor
```

---

# 🛡️ Client-Side vs Server-Side Validation

This is extremely important in web security.

### Client-side

```text
Browser
   ↓
JavaScript validation
   ↓
Server
```

Client-side validation improves user experience.

But the attacker controls the client.

They can:

* Disable JavaScript
* Modify HTML
* Modify JavaScript
* Send requests manually
* Use Burp Suite
* Use curl
* Create their own HTTP client

Therefore:

> **Client-side validation is not a security boundary.**

### Server-side

```text
Client
   ↓
Server
   ↓
Validate
   ↓
Process
```

Security-critical validation must happen on the server.

### 🔥 Memory Rule

> **Validate on the client for usability. Validate on the server for security.**

---

# 2️⃣ Output Handling

## 📤 What Is Output Handling?

Output handling means safely processing data before sending it to another context such as:

* HTML
* JavaScript
* SQL
* Shell commands
* HTTP headers
* JSON
* Logs
* File paths

The important idea is:

> **Data should not accidentally become executable code.**

---

# 💥 Example: XSS

Suppose a website displays:

```text
Welcome, <username>
```

The user enters:

```html
<script>alert("XSS")</script>
```

If the application directly inserts that into HTML, the browser may interpret it as JavaScript.

This can lead to **Cross-Site Scripting (XSS)**.

---

# 🛡️ Output Encoding

Instead of treating user-controlled data as HTML, encode it appropriately.

For example:

```text
< → &lt;
> → &gt;
```

So:

```html
<script>
```

can become something like:

```html
&lt;script&gt;
```

The browser displays it as text rather than interpreting it as HTML.

---

# 🎯 Context Matters

Different output contexts require different protections.

| Context      | Main concern               |
| ------------ | -------------------------- |
| HTML         | XSS                        |
| JavaScript   | Script injection           |
| URL          | URL manipulation/injection |
| SQL          | SQL injection              |
| Shell        | Command injection          |
| HTTP headers | Header injection           |
| Logs         | Log injection              |

You should not assume that one escaping method works everywhere.

### Important principle

> **Encode data for the context where it will be used.**

---

# 🧾 Safe Output Example

In Python web development, frameworks often provide HTML escaping.

Conceptually:

```python
username = "<script>alert(1)</script>"
```

Unsafe output:

```html
Welcome <script>alert(1)</script>
```

Safe encoded output:

```html
Welcome &lt;script&gt;alert(1)&lt;/script&gt;
```

---

# 📋 Output Handling and Logging

Logs are also output.

Suppose an application logs:

```text
Username: <user input>
```

An attacker might provide specially crafted input that makes log analysis confusing.

Therefore:

* Validate log data where appropriate
* Structure logs
* Avoid blindly trusting user-controlled values
* Protect log files
* Avoid logging passwords or secrets

---

# 3️⃣ Authentication Awareness

## 🔑 What Is Authentication?

**Authentication (AuthN)** answers:

> **"Who are you?"**

Examples:

```text
Username + Password
MFA
Passkeys
Certificates
Biometrics
```

---

# 🔐 Secure Authentication Principles

## 1. Never Store Plaintext Passwords

❌ Bad:

```text
username: farhan
password: MyPassword123
```

If the database is compromised, the passwords are immediately exposed.

### Better

Store a password hash using a password hashing algorithm such as:

* Argon2
* bcrypt
* scrypt

Conceptually:

```text
Password
   ↓
Password Hashing
   ↓
Stored Password Hash
```

---

# 2. Use Strong Password Handling

Applications should consider:

* Secure password hashing
* Unique salts
* Rate limiting
* Account protection
* MFA
* Secure password reset
* Session protection

---

# 3. Understand MFA

MFA means using multiple authentication factors.

Examples:

```text
Password
+
Authenticator App
```

or:

```text
Passkey
+
Device verification
```

Common factor categories:

| Factor             | Example      |
| ------------------ | ------------ |
| Something you know | Password     |
| Something you have | Security key |
| Something you are  | Fingerprint  |

---

# 4. Protect Authentication Requests

Authentication should normally happen over:

```text
HTTPS
```

Without transport protection, credentials could potentially be exposed to network attackers.

---

# 5. Protect Sessions

After login, the application needs to maintain authenticated state.

For example:

```text
Login
  ↓
Authentication
  ↓
Session created
  ↓
Session cookie
  ↓
Authenticated requests
```

Session cookies should use appropriate security attributes such as:

```text
Secure
HttpOnly
SameSite
```

---

# 4️⃣ Authorization Awareness

## 🚪 What Is Authorization?

**Authorization (AuthZ)** answers:

> **"What are you allowed to do?"**

Authentication and authorization are different.

### Example

You log into an application:

```text
Authentication:
"Farhan is logged in."
```

Then the application checks:

```text
Authorization:
"Is Farhan allowed to access this resource?"
```

---

# 🔑 AuthN vs AuthZ

| Authentication               | Authorization               |
| ---------------------------- | --------------------------- |
| Who are you?                 | What can you access?        |
| Identity                     | Permissions                 |
| Login                        | Access control              |
| Password/MFA                 | Roles/permissions           |
| Happens before authorization | Uses authenticated identity |

### Memory Trick

> 🔑 **AuthN = Name**
>
> 🚪 **AuthZ = Zone/permissions**

---

# 💥 Example: Insecure Authorization

Imagine:

```text
GET /api/users/100
```

A user can access their profile.

Then they change:

```text
/api/users/100
```

to:

```text
/api/users/101
```

If the server returns another user's information without checking authorization, the application has an access-control problem.

The critical mistake is:

```text
User is authenticated
        ↓
Server assumes they are authorized
```

That assumption is wrong.

---

# 🛡️ Correct Authorization Flow

```text
Request
   ↓
Authenticate user
   ↓
Identify requested resource
   ↓
Check permission
   ↓
Allow / Deny
```

Example:

```python
if user.id != requested_user_id and not user.is_admin:
    deny_access()
```

The exact authorization logic depends on the application's access-control model.

---

# 🎭 Role-Based Access Control

A common approach is:

```text
User
 ↓
Role
 ↓
Permissions
```

Example:

| Role   | View | Edit | Delete |
| ------ | ---: | ---: | -----: |
| User   |    ✅ |    ❌ |      ❌ |
| Editor |    ✅ |    ✅ |      ❌ |
| Admin  |    ✅ |    ✅ |      ✅ |

But simply hiding buttons is not enough.

### ❌ Insecure

```text
Admin button hidden from normal user
```

An attacker could still directly send:

```text
DELETE /api/users/42
```

### ✅ Secure

The backend checks:

```text
Is the current user authorized?
```

before performing the operation.

> **UI restrictions are not access control.**

---

# 5️⃣ Secure Coding Mindset

Secure coding is more than memorizing vulnerabilities.

It means thinking about security during the entire development process.

---

# 🧠 The Secure Developer Mindset

Instead of asking only:

> "Does this code work?"

also ask:

> "How could this code be misused?"

---

# 🔍 Think About Trust Boundaries

Identify where untrusted data enters your application.

```text
Internet
   ↓
User
   ↓
Browser
   ↓
API
   ↓
Backend
   ↓
Database
```

Potentially untrusted inputs include:

```text
User input
HTTP requests
Cookies
Headers
Uploaded files
API responses
External data
```

---

# 🛡️ Principle 1 — Never Trust the Client

The browser is controlled by the user.

Therefore, never rely on:

```text
HTML
JavaScript
CSS
Hidden fields
Disabled buttons
Client-side validation
```

for security decisions.

Security decisions belong on the server.

---

# 🛡️ Principle 2 — Least Privilege

Give users, applications, and services only the permissions they actually need.

Example:

```text
Web application
      ↓
Database account
      ↓
Only required permissions
```

Avoid:

```text
Web application
      ↓
Database administrator account
```

If the application is compromised, excessive privileges increase the potential impact.

---

# 🛡️ Principle 3 — Fail Securely

When something goes wrong, the application should fail in a secure way.

Example:

```python
try:
    perform_sensitive_operation()
except Exception:
    deny_access()
```

Conceptually:

```text
Error
 ↓
Safe failure
 ↓
No unintended privilege
```

Avoid errors that accidentally grant access.

---

# 🛡️ Principle 4 — Don't Expose Secrets

Never hardcode sensitive credentials directly into source code.

❌ Avoid:

```python
API_KEY = "secret-key-here"
PASSWORD = "mypassword"
```

Prefer:

```text
Environment variables
Secret managers
Secure configuration systems
```

Also avoid putting secrets into:

* Git repositories
* Public logs
* Screenshots
* URLs
* Error messages

---

# 🛡️ Principle 5 — Use Secure Libraries

Do not reinvent cryptography or security mechanisms unless you have a very strong reason and appropriate expertise.

Prefer established libraries/frameworks for:

```text
Password hashing
Cryptography
Authentication
Session management
Database access
Input/output encoding
```

---

# 🛡️ Principle 6 — Parameterized Queries

Avoid constructing SQL queries using raw user input.

### ❌ Dangerous pattern

```python
query = "SELECT * FROM users WHERE name = '" + username + "'"
```

### ✅ Safer pattern

Use parameterized queries:

```python
cursor.execute(
    "SELECT * FROM users WHERE name = ?",
    (username,)
)
```

The exact syntax depends on the database library.

The important principle is:

> **Keep data separate from SQL instructions.**

---

# 🛡️ Principle 7 — Safe Command Execution

Avoid directly inserting user-controlled input into shell commands.

### ❌ Risky

```python
subprocess.run(user_input, shell=True)
```

This can create command-injection risks.

Prefer:

```python
subprocess.run(
    ["ping", "-c", "1", ip_address],
    shell=False
)
```

But even then, validate the input and restrict what the application is allowed to execute.

---

# 🛡️ Principle 8 — Handle Errors Safely

Avoid exposing internal information to users.

### ❌ Bad error

```text
Database connection failed:
postgres://admin:password@internal-db:5432/app
```

This can reveal:

* Database details
* Internal hostnames
* Credentials
* File paths
* Stack traces
* Framework information

### Better

```text
Something went wrong. Please try again later.
```

Detailed information can go into protected server-side logs.

---

# 🛡️ Principle 9 — Log Security Events

Useful events can include:

```text
Successful login
Failed login
Password reset
Privilege changes
Access-control failures
Administrative actions
Security configuration changes
```

Logs should be:

* Protected
* Timestamped
* Structured
* Monitored
* Free from unnecessary secrets

---

# 🛡️ Principle 10 — Keep Dependencies Updated

Applications often depend on:

```text
Libraries
Frameworks
Packages
Operating systems
Containers
Third-party services
```

Outdated dependencies can contain known vulnerabilities.

A secure development process therefore includes:

```text
Identify dependencies
       ↓
Track versions
       ↓
Monitor vulnerabilities
       ↓
Patch/update
       ↓
Test
       ↓
Deploy
```

---

# 🔄 Secure Development Workflow

A simple secure development lifecycle can look like:

```text
Requirements
     ↓
Threat Modeling
     ↓
Secure Design
     ↓
Secure Coding
     ↓
Code Review
     ↓
Security Testing
     ↓
Deployment
     ↓
Monitoring
     ↓
Patch & Improve
```

Security should not be added only after the application is finished.

---

# 🧩 Putting Everything Together

Consider a login API:

```text
POST /api/login
```

The client sends:

```json
{
  "username": "farhan",
  "password": "example"
}
```

A secure backend should think through:

### 1. Input Validation

```text
Is username valid?
Is password input acceptable?
Are request fields correctly typed?
Are size limits enforced?
```

### 2. Authentication

```text
Does the account exist?
Does the password hash match?
Is MFA required?
```

### 3. Authorization

After login:

```text
What permissions does this user have?
```

### 4. Session Management

```text
Create secure session
Set appropriate cookie attributes
```

### 5. Output Handling

Do not expose:

```text
Password hashes
Internal database information
Sensitive account details
```

### 6. Logging

Record useful security events without logging the password.

---

# 🧠 Secure Development Mental Model

Use this flow whenever you write web application code:

```text
             ┌───────────────┐
             │  User Request │
             └───────┬───────┘
                     ↓
             ┌───────────────┐
             │ Input Validate│
             └───────┬───────┘
                     ↓
             ┌───────────────┐
             │ Authenticate  │
             └───────┬───────┘
                     ↓
             ┌───────────────┐
             │  Authorize    │
             └───────┬───────┘
                     ↓
             ┌───────────────┐
             │ Process Safely│
             └───────┬───────┘
                     ↓
             ┌───────────────┐
             │ Output Safely │
             └───────┬───────┘
                     ↓
             ┌───────────────┐
             │ Log / Monitor │
             └───────────────┘
```

---

# ⚔️ Common Secure Development Mistakes

| Mistake                                 | Why it is dangerous                         |
| --------------------------------------- | ------------------------------------------- |
| Trusting client-side validation         | Client can be modified                      |
| Storing plaintext passwords             | Database compromise exposes passwords       |
| Using weak authorization                | Users may access other resources            |
| Trusting hidden fields                  | Users can modify them                       |
| Building SQL with strings               | SQL injection risk                          |
| Using `shell=True` with untrusted input | Command injection risk                      |
| Exposing stack traces                   | Reveals internal information                |
| Hardcoding secrets                      | Secrets can leak through source control     |
| Logging passwords                       | Creates unnecessary sensitive-data exposure |
| Using outdated dependencies             | Known vulnerabilities may remain            |
| Writing custom crypto                   | Easy to implement incorrectly               |

---

# 🌐 Real-World Web Security Example

Imagine an e-commerce website:

```text
Customer
   ↓
Browser
   ↓
HTTPS
   ↓
Web Application
   ↓
Authentication
   ↓
Authorization
   ↓
Order API
   ↓
Database
```

A secure application checks:

### Input

```text
Is the product ID valid?
Is the quantity valid?
```

### Authentication

```text
Is the customer logged in?
```

### Authorization

```text
Can this customer access this order?
```

### Business logic

```text
Is the quantity available?
Is the price valid?
Can the user modify this order?
```

### Output

```text
Don't expose internal database information.
Don't reflect unsafe user input into HTML.
```

### Logging

```text
Record important security events.
Don't log passwords or payment secrets.
```

---

# 🧪 Hands-On Practice

## 🔬 Task 1 — Input Validation

Create a Python program:

```python
username = input("Username: ")
age = input("Age: ")
```

Add validation for:

```text
Username length
Allowed characters
Age type
Age range
```

Test with:

```text
Farhan
123
-5
999
hello
```

---

## 🔬 Task 2 — Client vs Server Validation

Create a local HTML form containing:

```html
<input type="number" min="1" max="100">
```

Then understand:

```text
Browser restriction
       ≠
Server security
```

Try submitting values outside the expected range and observe how a real backend would need to validate them independently.

---

## 🔬 Task 3 — Authentication vs Authorization

Create a simple local application with:

```text
User
Editor
Admin
```

Define:

```text
User   → view
Editor → view + edit
Admin  → view + edit + delete
```

Then write down:

```text
Authentication:
Who is the user?

Authorization:
What can the user do?
```

---

## 🔬 Task 4 — Safe SQL

Create a small SQLite database and compare:

```text
String-built SQL
```

with:

```text
Parameterized SQL
```

Your goal is to understand why data should remain separate from SQL instructions.

---

## 🔬 Task 5 — Secure Code Review

Take a small Python web application and search for:

```text
☐ Hardcoded secrets
☐ Missing input validation
☐ Unsafe SQL
☐ Unsafe shell commands
☐ Missing authorization
☐ Sensitive information in errors
☐ Passwords in logs
☐ Weak session handling
```

This is the beginning of a real **secure code review mindset**.

---

# 🧠 Memory Tricks

### Input Validation

> **Don't Trust Input.**

### Output Handling

> **Data should stay data.**

### Authentication

> 🔑 **Who are you?**

### Authorization

> 🚪 **What can you do?**

### Secure Coding

> 🧠 **Assume misuse, minimize trust, fail safely.**

---

# 🔥 The 5-Part Security Model

Remember:

```text
I → O → A → A → S
```

### I — Input Validation

Control what enters.

### O — Output Handling

Control how data leaves/is interpreted.

### A — Authentication

Verify identity.

### A — Authorization

Verify permissions.

### S — Secure Coding Mindset

Design assuming things can go wrong.

---

# 📌 Quick Revision

| Principle        | Main Question                            |
| ---------------- | ---------------------------------------- |
| Input Validation | Can I trust this input?                  |
| Output Handling  | Could this data be interpreted unsafely? |
| Authentication   | Who is this user?                        |
| Authorization    | What is this user allowed to do?         |
| Secure Coding    | How could this feature be misused?       |

### Most Important Rules

```text
Client ≠ Trusted
Hidden ≠ Secure
Validation ≠ Authorization
Authentication ≠ Authorization
Encryption ≠ Input Validation
UI Restrictions ≠ Access Control
```

---

# 🎯 Final Takeaway

Secure development is about building applications that remain secure even when users provide unexpected input or deliberately try to misuse the system.

The core flow is:

```text
Validate Input
      ↓
Authenticate Identity
      ↓
Authorize Action
      ↓
Process Securely
      ↓
Handle Output Safely
      ↓
Log & Monitor
```

> 🛡️ **A secure developer doesn't only ask "Does it work?" — they also ask "What happens if someone tries to abuse it?"**

This mindset is fundamental to web application security, secure coding, vulnerability assessment, penetration testing, and secure software development.
