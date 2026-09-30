# 🌐 Common Web Security Risks

> **Web security risks are weaknesses in web applications that attackers can exploit to access data, bypass security controls, execute unintended actions, or disrupt services.**

A web application can have:

```text
Frontend
   ↓
HTTP / HTTPS
   ↓
Backend / API
   ↓
Authentication
   ↓
Authorization
   ↓
Database
   ↓
External Services
```

Every layer can introduce security risks.

The major areas covered here are:

1. 💉 Injection Attacks
2. 🔑 Broken Authentication
3. 🔐 Sensitive Data Exposure
4. ⚙️ Security Misconfigurations
5. 🛡️ OWASP Awareness

---

# 1️⃣ Injection Attacks

## 💉 What Is an Injection Attack?

An **injection attack** happens when untrusted input is interpreted as part of a command, query, or executable language instead of being treated purely as data.

The basic problem is:

```text
Untrusted Input
      ↓
Application
      ↓
Interpreter / Query / Command
      ↓
Unintended Behavior
```

### Simple idea

Suppose an application expects:

```text
Username:
Farhan
```

But an attacker supplies specially crafted input that changes how the application interprets the request.

The application accidentally treats:

```text
DATA + INSTRUCTIONS
```

instead of:

```text
DATA
```

This can lead to serious vulnerabilities.

---

# 🎯 Common Types of Injection

| Type               | Target                          |
| ------------------ | ------------------------------- |
| SQL Injection      | SQL database                    |
| Command Injection  | Operating system commands       |
| XSS                | Browser/HTML/JavaScript context |
| LDAP Injection     | LDAP queries                    |
| NoSQL Injection    | NoSQL database queries          |
| Template Injection | Server-side templates           |

The exact attack mechanics differ, but the underlying problem is similar:

> **Untrusted data reaches an interpreter without safe handling.**

---

# 🗄️ SQL Injection

## What Is SQL Injection?

SQL injection occurs when attacker-controlled input changes the meaning of a SQL query.

### ❌ Unsafe pattern

```python
username = input("Username: ")

query = "SELECT * FROM users WHERE username = '" + username + "'"
```

The application is constructing SQL using string concatenation.

Conceptually:

```text
User Input
    ↓
String Concatenation
    ↓
SQL Query
    ↓
Database
```

This is dangerous because the input can potentially affect the SQL syntax.

---

# 🛡️ Parameterized Queries

A safer approach is:

```python
cursor.execute(
    "SELECT * FROM users WHERE username = ?",
    (username,)
)
```

The database driver treats the supplied value as **data**, rather than SQL instructions.

```text
User Input
     ↓
Parameterized Query
     ↓
Database
```

### Security principle

> **Separate code from data.**

---

# 💻 Command Injection

Command injection happens when untrusted input reaches an operating-system command.

### ❌ Risky example

```python
import subprocess

command = input("Command: ")
subprocess.run(command, shell=True)
```

If arbitrary user input reaches a shell, the user may be able to influence what commands are executed.

### Safer approach

Use a fixed executable and argument list:

```python
subprocess.run(
    ["ping", "-c", "1", ip_address],
    shell=False
)
```

Even here, the input should still be validated.

---

# 🌐 XSS as an Injection Problem

Cross-Site Scripting occurs when attacker-controlled data is inserted into a web page and interpreted as active content.

Example of dangerous user input:

```html
<script>
    alert("XSS");
</script>
```

If an application places this into HTML without appropriate handling, the browser may interpret it as code.

### Defenses

* Context-appropriate output encoding
* Safe templating
* Input validation where appropriate
* Content Security Policy
* Avoid unsafe DOM APIs

Remember:

> **Input validation and output encoding solve different parts of the problem.**

---

# 🧠 Injection Memory Trick

```text
INPUT
  ↓
Interpreter
  ↓
UNEXPECTED COMMAND
```

Think:

> **"Can my input become code?"**

---

# 2️⃣ Broken Authentication

## 🔑 What Is Authentication?

Authentication answers:

> **Who are you?**

Examples:

```text
Username + Password
MFA
Passkeys
Certificates
Biometrics
```

Authentication becomes a security risk when attackers can bypass, abuse, or compromise the authentication mechanism.

---

# 💥 Common Authentication Problems

### 1. Weak Password Handling

Bad practice:

```text
password = plaintext
```

Better:

```text
password
    ↓
Password hashing
    ↓
Argon2 / bcrypt / scrypt
    ↓
Stored hash
```

Never store plaintext passwords.

---

### 2. Weak Password Policies

Examples:

```text
123456
password
qwerty
```

Applications should support strong authentication mechanisms and avoid unnecessarily weak password requirements.

---

### 3. Missing Rate Limiting

Imagine:

```text
Login Attempt 1
Login Attempt 2
Login Attempt 3
...
Login Attempt 1,000,000
```

Without appropriate controls, automated password-guessing attacks become easier.

Possible defenses include:

* Rate limiting
* Progressive delays
* MFA
* Account protection mechanisms
* Monitoring and alerting

---

# 🔐 4. Poor Session Management

After successful login:

```text
Login
 ↓
Session Created
 ↓
Session Cookie
 ↓
Authenticated Requests
```

If sessions are poorly managed, attackers may exploit:

* Session theft
* Session fixation
* Session leakage
* Excessively long sessions
* Failure to invalidate sessions

---

# 🛡️ Secure Session Cookies

Common security attributes include:

```text
Secure
HttpOnly
SameSite
```

### Secure

Cookie should only be sent over HTTPS.

### HttpOnly

Helps prevent JavaScript from directly reading the cookie.

### SameSite

Controls when cookies are sent in cross-site contexts and can help reduce certain cross-site request risks.

---

# 🚨 Authentication vs Authorization

Do not confuse these.

| Authentication | Authorization    |
| -------------- | ---------------- |
| Who are you?   | What can you do? |
| Identity       | Permissions      |
| Login          | Access control   |

Example:

```text
User logs in
     ↓
Authentication
     ↓
User identified
     ↓
Authorization
     ↓
Can user access /admin?
```

A secure login does not automatically mean secure authorization.

---

# 3️⃣ Sensitive Data Exposure

## 🔐 What Is Sensitive Data?

Sensitive data can include:

* Passwords
* Authentication tokens
* Session identifiers
* API keys
* Private keys
* Financial information
* Personal information
* Health information
* Internal system information
* Confidential business data

The exact classification depends on the application and its requirements.

---

# 💥 How Sensitive Data Gets Exposed

## 1. Plaintext Transmission

Sending sensitive information without appropriate transport protection can expose it to network attackers.

### ❌

```text
HTTP
 ↓
Sensitive credentials
```

### ✅

```text
HTTPS
 ↓
TLS
 ↓
Encrypted transport
```

Remember:

> **HTTPS protects data in transit; it does not automatically protect data everywhere else.**

---

# 2. Plaintext Password Storage

### ❌

```text
Database

username | password
---------|-------------
farhan   | MyPassword
```

### ✅

```text
username | password_hash
---------|----------------
farhan   | $argon2id$...
```

Modern password hashing algorithms such as Argon2, bcrypt, or scrypt are designed for password storage.

---

# 3. Secrets in Source Code

### ❌

```python
API_KEY = "super-secret-key"
```

Problems:

* Git history may preserve it
* Developers may accidentally publish it
* Logs/build systems may expose it
* Other users of the repository may access it

Prefer:

```text
Environment variables
Secret managers
Secure configuration systems
```

---

# 4. Sensitive Information in URLs

Avoid putting sensitive secrets in URLs.

Example:

```text
https://example.com/reset?token=SECRET
```

URLs can appear in:

* Browser history
* Proxy logs
* Server logs
* Analytics systems
* Referrer information
* Screenshots

Sensitive tokens should be handled carefully.

---

# 5. Sensitive Data in Logs

A developer might accidentally log:

```text
Username: farhan
Password: MyPassword123
Token: eyJ...
```

This creates another copy of the sensitive information.

### Better

```text
Login attempt failed
User ID: 12345
Timestamp: ...
Reason: invalid credentials
```

Avoid unnecessary secrets.

---

# 🔒 Data Protection Layers

Think about data in different states:

```text
              DATA
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
   In Transit  At Rest  In Use
       │        │        │
      TLS     Storage   Memory/
                         Processing
```

Security needs to consider each state.

---

# 🛡️ 4️⃣ Security Misconfigurations

## ⚙️ What Is Security Misconfiguration?

A security misconfiguration happens when a system is deployed or configured in an insecure way.

This can occur in:

* Web servers
* Applications
* Databases
* Cloud services
* Containers
* Operating systems
* Firewalls
* Authentication systems

---

# 💥 Common Examples

## 1. Debug Mode Enabled

A production application might accidentally expose detailed errors:

```text
DEBUG = True
```

An error may reveal:

```text
File paths
Stack traces
Database details
Framework information
Environment variables
```

### Better

```text
Production
   ↓
Debug disabled
   ↓
Generic user-facing errors
   ↓
Detailed protected server-side logs
```

---

# 2. Default Credentials

Example:

```text
admin / admin
```

or vendor-default passwords that were never changed.

Default credentials should be replaced with secure credentials and, where possible, unnecessary default accounts should be disabled.

---

# 3. Unnecessary Services

Imagine a server running:

```text
HTTP
SSH
FTP
Telnet
Database
Unused service
Unused admin panel
```

Every unnecessary service can increase the attack surface.

### Principle

> **Reduce the attack surface.**

Disable what isn't needed.

---

# 4. Directory Listing

A web server might expose:

```text
/uploads/
    backup.zip
    old_database.sql
    config.txt
```

If directory listing is enabled unnecessarily, sensitive files could become discoverable.

---

# 5. Exposed Administration Interfaces

Examples:

```text
/admin
/management
/server-status
/debug
```

Simply hiding these paths is not security.

They still need:

```text
Authentication
+
Authorization
+
Secure configuration
```

---

# 6. Missing Security Headers

Applications can use security-related HTTP headers such as:

```text
Content-Security-Policy
Strict-Transport-Security
X-Content-Type-Options
Referrer-Policy
```

These can help reduce certain classes of web attacks and unsafe browser behavior.

---

# 7. Overly Permissive CORS

CORS controls which browser origins can make certain cross-origin requests.

A careless configuration can expose data or functionality to unintended origins.

The correct configuration depends on the application's architecture.

---

# 🧱 Secure Configuration Checklist

Before deployment:

```text
☐ Debug disabled
☐ Default credentials removed
☐ Unnecessary services disabled
☐ Directory listing reviewed
☐ Admin interfaces protected
☐ Security headers configured
☐ CORS reviewed
☐ TLS configured
☐ Secrets protected
☐ Error messages reviewed
☐ Dependencies updated
☐ Permissions minimized
```

---

# 5️⃣ OWASP Awareness

## 🛡️ What Is OWASP?

**OWASP** stands for:

> **Open Worldwide Application Security Project**

OWASP is a community-driven organization that provides resources for improving application security.

One of its best-known resources is the:

> **OWASP Top 10**

The OWASP Top 10 is an awareness document covering major categories of web application security risks.

It is useful for:

* Learning
* Secure development
* Code review
* Security testing
* Threat modeling
* Security awareness
* Interview preparation

---

# 📋 OWASP Top 10

The **OWASP Top 10: 2025** is the current OWASP Top 10 release as of 2026.

Its categories are:

|   # | Category                               |
| --: | -------------------------------------- |
| A01 | Broken Access Control                  |
| A02 | Security Misconfiguration              |
| A03 | Software Supply Chain Failures         |
| A04 | Cryptographic Failures                 |
| A05 | Injection                              |
| A06 | Insecure Design                        |
| A07 | Authentication Failures                |
| A08 | Software or Data Integrity Failures    |
| A09 | Security Logging and Alerting Failures |
| A10 | Mishandling of Exceptional Conditions  |

> **Important:** The OWASP Top 10 is an awareness and education resource, not a complete list of every possible web vulnerability.

---

# 🔥 Understanding the Categories

## A01 — Broken Access Control

The application fails to correctly enforce what users are allowed to access.

Example:

```text
Normal User
    ↓
/admin
    ↓
Access granted ❌
```

Correct behavior:

```text
Normal User
    ↓
/admin
    ↓
Authorization check
    ↓
Access denied
```

---

# A02 — Security Misconfiguration

Examples:

```text
Debug enabled
Default passwords
Unnecessary services
Exposed files
Weak security settings
```

---

# A03 — Software Supply Chain Failures

Modern applications depend on:

```text
Application
   ↓
Libraries
   ↓
Packages
   ↓
Build systems
   ↓
Third-party components
```

A vulnerability or compromise in a dependency or software delivery process can affect the application.

This is why dependency management and software supply-chain security matter.

---

# A04 — Cryptographic Failures

Problems involving inadequate protection of sensitive data through cryptography.

Examples include:

```text
Weak cryptographic algorithms
Poor key management
Missing encryption where needed
Improper certificate validation
Weak password storage
```

This connects directly to the **Cryptography module** you already completed.

---

# A05 — Injection

Examples:

```text
SQL Injection
Command Injection
XSS
NoSQL Injection
LDAP Injection
```

Core issue:

```text
Untrusted Data
      ↓
Interpreter
      ↓
Unexpected Behavior
```

---

# A06 — Insecure Design

This is different from simply having a coding bug.

The problem exists in the **design or architecture itself**.

Example:

```text
Password reset
      ↓
No identity verification
      ↓
Anyone can reset another user's password
```

Even perfect syntax cannot fix a fundamentally insecure design.

Security must be considered during design.

---

# A07 — Authentication Failures

Examples:

```text
Weak authentication
Poor session management
Missing MFA where appropriate
Credential stuffing exposure
Weak password recovery
```

---

# A08 — Software or Data Integrity Failures

Applications often trust:

```text
Updates
Libraries
Serialized data
CI/CD components
External packages
Build artifacts
```

If integrity is not properly protected, attackers may influence what software or data the application accepts.

---

# A09 — Security Logging and Alerting Failures

If important security events aren't properly logged or monitored:

```text
Attack
  ↓
No useful log
  ↓
No alert
  ↓
Delayed detection
```

Good logging supports:

```text
Detection
Investigation
Incident response
Forensics
Accountability
```

---

# A10 — Mishandling of Exceptional Conditions

Applications encounter unexpected situations:

```text
Invalid input
Database failure
Network failure
Timeout
Resource exhaustion
Unexpected state
```

If these conditions are handled incorrectly, the application may enter an unsafe state.

Example:

```text
Payment processing
       ↓
Unexpected error
       ↓
Application incorrectly marks payment as successful
```

This is why secure error handling and safe failure matter.

---

# 🔗 Connecting OWASP to What You've Learned

Your previous topics now connect together:

```text
Web Fundamentals
       ↓
HTML
       ↓
CSS
       ↓
HTTP
       ↓
Backend
       ↓
Secure Development
       ↓
Common Web Security Risks
       ↓
OWASP
```

For example:

```text
Input Validation
      ↓
Injection Prevention

Authentication
      ↓
Authentication Security

Authorization
      ↓
Access Control

Output Handling
      ↓
XSS Prevention

Secure Configuration
      ↓
Security Misconfiguration
```

This is why learning the fundamentals first is important.

---

# 🧠 Attacker's Perspective

When reviewing a web application, you can think in terms of:

```text
1. What can I control?
2. Where does my input go?
3. What does the server trust?
4. Am I authenticated?
5. What am I authorized to access?
6. Can I access another user's data?
7. Are sensitive secrets exposed?
8. Is anything misconfigured?
9. Are security events logged?
10. What happens when something unexpected occurs?
```

This is a much more useful mindset than simply memorizing vulnerability names.

---

# 🧪 Safe Hands-On Practice

Perform these exercises only against applications you own or are explicitly authorized to test.

## 🔬 Task 1 — SQL Injection Concept

Create a local SQLite application.

First implement:

```python
query = "SELECT * FROM users WHERE username = '" + username + "'"
```

Then replace it with a parameterized query.

Your goal is to understand:

```text
String concatenation
       ↓
SQL interprets input
```

versus:

```text
Parameterized query
       ↓
Input treated as data
```

---

# 🔬 Task 2 — Authentication Review

Create a local login application.

Check:

```text
☐ Password hashing
☐ Failed login handling
☐ Rate limiting concept
☐ Session creation
☐ Session expiration
☐ Logout
☐ Secure cookie attributes
```

---

# 🔬 Task 3 — Authorization Testing

Create:

```text
User A
User B
Admin
```

Give each user different resources.

Then verify that:

```text
User A → User A data ✅
User A → User B data ❌
User A → Admin functions ❌
Admin → Authorized admin functions ✅
```

This teaches you the difference between authentication and authorization.

---

# 🔬 Task 4 — Security Misconfiguration Lab

Create a local application and deliberately configure:

```text
Debug mode
Directory listing
Weak/default credentials
Missing security headers
```

Then document:

```text
Configuration
   ↓
Security impact
   ↓
Recommended fix
```

Restore the secure configuration afterward.

---

# 🔬 Task 5 — OWASP Mapping

Take the local application you've built and create a table:

| Finding               | OWASP Category  | Risk                   | Fix                       |
| --------------------- | --------------- | ---------------------- | ------------------------- |
| Missing authorization | A01             | Unauthorized access    | Server-side authorization |
| Debug enabled         | A02             | Information disclosure | Disable debug             |
| Unsafe SQL            | A05             | SQL injection          | Parameterized queries     |
| Weak password storage | A04/A07 context | Credential exposure    | Password hashing          |

The goal is not to memorize the list.

The goal is to learn how to **recognize and classify security problems**.

---

# 🧠 Common Mistakes to Remember

```text
❌ Client-side validation = security
❌ Hidden button = authorization
❌ HTTPS = entire application is secure
❌ Login = authorization
❌ Password masking = password encryption
❌ Secret in source code = safe if repository is private
❌ OWASP Top 10 = complete vulnerability list
❌ Error messages are harmless
❌ Default configuration is always secure
```

Instead:

```text
✅ Validate server-side
✅ Enforce authorization server-side
✅ Protect data at appropriate layers
✅ Separate authentication from authorization
✅ Protect secrets
✅ Review configuration
✅ Use secure development practices
✅ Monitor important security events
```

---

# 🧠 Memory Map

Remember:

```text
I → A → D → M → O
```

### 💉 I — Injection

Can input become instructions?

### 🔑 A — Authentication

Can attackers bypass or abuse identity verification?

### 🔐 D — Data Exposure

Can sensitive information leak?

### ⚙️ M — Misconfiguration

Is something deployed insecurely?

### 🛡️ O — OWASP

Can I recognize and classify common application-security risks?

---

# ⚡ Quick Revision

| Risk                    | Core Problem                     | Main Defense                                     |
| ----------------------- | -------------------------------- | ------------------------------------------------ |
| Injection               | Data becomes instructions        | Parameterized queries, safe APIs, encoding       |
| Authentication failures | Identity controls are weak       | Strong authentication, MFA, secure sessions      |
| Sensitive data exposure | Sensitive information leaks      | Encryption, secure storage, secret management    |
| Misconfiguration        | System is configured insecurely  | Hardening, secure defaults, configuration review |
| OWASP awareness         | Security risks aren't recognized | Learn categories and apply secure practices      |

---

# 🎯 Final Takeaway

Web security is not just about finding "hacks."

It is about understanding **where trust exists, where data flows, and where security controls can fail.**

A simple mental model is:

```text
             WEB APPLICATION
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
    INPUT        IDENTITY       DATA
       │            │            │
   Injection     AuthN/AuthZ   Exposure
       │            │            │
       └────────────┼────────────┘
                    ↓
             CONFIGURATION
                    ↓
               OWASP AWARENESS
```

> 🔥 **The most important skill is not memorizing OWASP categories. It is learning to look at an application and ask: "What does the application trust, and what happens if that trust is abused?"**

That mindset will become especially useful when you start **Burp Suite, web vulnerability testing, and penetration-testing labs**.
