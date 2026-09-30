# 🌐 10. Apply Through Hands-on Tasks — Web Security

> 🛡️ **Theory becomes cybersecurity skill when you apply it.**
>
> In this hands-on task, we combine web development, authentication, logging, vulnerability identification, and secure development reporting into a small local web-security project.

---

## 📌 What We'll Build

We will create a simple web application and use it as a controlled security laboratory.

```text
                    🌐 Browser
                        │
                        │ HTTP/HTTPS
                        ▼
                ┌───────────────┐
                │   Web App     │
                │   Backend     │
                └───────┬───────┘
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
     🔐 Authentication  🗄️ Database   📋 Logs
          │                           │
          └──────────────┬────────────┘
                         ▼
                  🔎 Security Analysis
                         │
                         ▼
                  📄 Security Report
```

The project covers:

1. 🌐 Build a simple web application
2. 🔐 Implement authentication workflows
3. 📋 Analyze application logs
4. 🛡️ Identify security weaknesses
5. 📄 Create secure development reports

---

# 1. 🌐 Build a Simple Web Application

## What Is a Web Application?

A **web application** is software that users interact with through a web browser.

Examples:

* Login systems
* Online banking
* E-commerce websites
* Social media platforms
* Learning platforms
* Admin dashboards
* Security monitoring dashboards

A basic web application contains:

```text
Frontend
   ↓
HTTP Request
   ↓
Backend
   ↓
Database
   ↓
Backend
   ↓
HTTP Response
   ↓
Frontend
```

---

## 🧱 Basic Project Structure

A simple Flask security lab can look like:

```text
web-security-lab/
│
├── app.py
├── users.json
├── requirements.txt
│
├── templates/
│   ├── login.html
│   ├── register.html
│   └── dashboard.html
│
├── logs/
│   └── app.log
│
└── reports/
    └── security-report.md
```

### Main Components

| Component          | Purpose                |
| ------------------ | ---------------------- |
| `app.py`           | Backend application    |
| `templates/`       | HTML pages             |
| `users.json`       | Lab user data          |
| `logs/`            | Application logs       |
| `reports/`         | Security documentation |
| `requirements.txt` | Python dependencies    |

---

## 🐍 Simple Flask Application

Example:

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route("/")
def home():
    return """
    <h1>Web Security Lab</h1>
    <p>Welcome to the security laboratory.</p>
    <a href="/login">Login</a>
    """


@app.route("/login")
def login():
    return render_template("login.html")


if __name__ == "__main__":
    app.run(debug=True)
```

Run:

```bash
python app.py
```

Then access:

```text
http://127.0.0.1:5000
```

### Security Lesson

A web application consists of multiple trust boundaries:

```text
Browser → Web Server → Application → Database
```

The browser is controlled by the user.

Therefore:

> 🔐 **Never trust the client.**

Security decisions must be enforced by the backend.

---

# 2. 🔐 Implement Authentication Workflows

Authentication answers:

> **"Who are you?"**

Authorization answers:

> **"What are you allowed to do?"**

These are different security concepts.

---

## 🔄 Registration Workflow

A secure registration process can look like:

```text
User
 │
 ▼
Registration Form
 │
 ▼
Server-side Validation
 │
 ▼
Password Hashing
 │
 ▼
Database
 │
 ▼
Account Created
```

The application should **never store plaintext passwords**.

Instead:

```text
Password
   ↓
Password Hashing
   ↓
Password Hash
   ↓
Database
```

Modern password-hashing algorithms include:

* Argon2
* bcrypt
* scrypt

---

## 🔑 Login Workflow

```text
User
 │
 ▼
Username + Password
 │
 ▼
Server
 │
 ▼
Find Account
 │
 ▼
Verify Password Hash
 │
 ├── ❌ Invalid → Reject
 │
 └── ✅ Valid
        │
        ▼
   Create Session
        │
        ▼
   Session Cookie
        │
        ▼
      Dashboard
```

---

## 🍪 Session Management

HTTP is stateless.

That means each HTTP request is normally independent.

Sessions allow the server to remember that a user has authenticated.

Example:

```text
POST /login
       │
       ▼
Authentication successful
       │
       ▼
Create session
       │
       ▼
Set-Cookie: session=...
       │
       ▼
Browser stores cookie
       │
       ▼
GET /dashboard
Cookie: session=...
```

Important cookie security attributes:

| Attribute  | Purpose                               |
| ---------- | ------------------------------------- |
| `Secure`   | Send cookie over HTTPS                |
| `HttpOnly` | Prevent JavaScript access             |
| `SameSite` | Helps reduce cross-site request risks |

---

## 🔐 Authentication vs Authorization

Example:

```text
Farhan logs in
      ↓
Authentication
      ↓
"Farhan is a valid user."
      ↓
Authorization
      ↓
"Farhan can access his profile."
      ↓
"Farhan cannot access the admin panel."
```

A hidden admin button is **not** authorization.

Bad:

```html
<button style="display:none">
    Admin Panel
</button>
```

An attacker can still attempt:

```text
/admin
```

The backend must verify permissions.

---

# 3. 📋 Analyze Application Logs

Logs provide visibility into application activity.

Without logs:

```text
Attack
  ↓
???
```

With useful logs:

```text
Attack
  ↓
Event recorded
  ↓
Detection
  ↓
Investigation
  ↓
Response
```

---

## 📝 Example Application Log

```text
2026-09-30 10:15:23 INFO User login successful username=farhan
2026-09-30 10:16:01 WARNING Login failed username=admin
2026-09-30 10:16:04 WARNING Login failed username=admin
2026-09-30 10:16:07 WARNING Login failed username=admin
2026-09-30 10:16:12 INFO User login successful username=farhan
```

We can analyze:

* Timestamp
* Event type
* Username
* Success/failure
* Frequency
* Source IP
* Request information

---

## 🚨 Detecting Suspicious Activity

For example:

```text
5 failed logins
       ↓
Within a short time period
       ↓
Same account/IP
       ↓
Generate alert
       ↓
Investigate
```

Important:

> ⚠️ A detection signal is not automatically proof of an attack.

Repeated failed logins could have legitimate causes.

---

## 🐍 Python Logging

Example:

```python
import logging

logging.basicConfig(
    filename="logs/app.log",
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s"
)

logging.info("Application started")
logging.warning("Login failed")
logging.error("Database connection failed")
```

Common log levels:

```text
DEBUG
INFO
WARNING
ERROR
CRITICAL
```

---

## 🚫 Never Log Secrets

Avoid logging:

```text
❌ Passwords
❌ API keys
❌ Private keys
❌ Session tokens
❌ Access tokens
❌ Database credentials
```

Instead of:

```text
password=MyPassword123
```

use:

```text
login attempt failed username=farhan
```

---

# 4. 🛡️ Identify Security Weaknesses

Now we use our application as a security-testing laboratory.

The goal is not simply:

> "Does the application work?"

We also ask:

> **"How could this application be abused?"**

---

## 🔎 Security Review Areas

### 1. Input Validation

Ask:

```text
Can users submit unexpected input?
```

Check:

* Length
* Type
* Format
* Range
* Allowed characters

---

### 2. Authentication

Check:

```text
Are passwords securely stored?
Is login protected?
Are sessions secure?
Are repeated login attempts controlled?
```

---

### 3. Authorization

Check:

```text
Can User A access User B's data?
Can normal users access admin functionality?
Are permissions checked server-side?
```

---

### 4. Session Security

Check:

```text
Are session cookies protected?
Are sessions invalidated during logout?
Are sessions rotated when appropriate?
Can session identifiers leak?
```

---

### 5. Injection

Look for unsafe handling of user-controlled data.

Examples:

```text
SQL Injection
Command Injection
XSS
NoSQL Injection
Template Injection
```

Core security principle:

> 🔐 **Keep data separate from code.**

For SQL, use parameterized queries instead of string concatenation.

---

### 6. Sensitive Data Exposure

Look for:

```text
Passwords
API keys
Tokens
Private information
Database credentials
Internal configuration
```

Potential locations:

```text
Source code
Logs
URLs
HTML
JavaScript
Error messages
Configuration files
```

---

### 7. Security Misconfiguration

Check for:

```text
Debug mode enabled
Default credentials
Unnecessary services
Exposed admin interfaces
Directory listing
Weak configuration
Overly permissive CORS
Missing security headers
```

---

## 🧠 Security Review Mindset

When looking at an application, ask:

```text
What can I control?
        ↓
Where does my input go?
        ↓
Does the server validate it?
        ↓
Who am I?
        ↓
What am I allowed to do?
        ↓
Where is my data stored?
        ↓
What gets logged?
        ↓
What happens when something fails?
```

---

# 5. 📄 Create Secure Development Reports

A security report documents what you discovered.

A professional report should allow another developer or security engineer to understand:

```text
What is wrong?
        ↓
Where is it?
        ↓
Why does it matter?
        ↓
How can it be reproduced?
        ↓
How can it be fixed?
```

---

# 📑 Security Report Structure

```markdown
# Web Application Security Report

## 1. Executive Summary

## 2. Scope

## 3. Application Overview

## 4. Testing Methodology

## 5. Findings

### Finding 1
- Title:
- Severity:
- Location:
- Description:
- Evidence:
- Impact:
- Reproduction:
- Recommendation:

## 6. Logging & Monitoring Review

## 7. Authentication Review

## 8. Authorization Review

## 9. Secure Development Review

## 10. Conclusion
```

---

## 🔍 Example Finding

```markdown
### Finding 1 — Missing Server-Side Authorization

**Severity:** High

**Location:** `/admin`

**Description:**

The application relies on client-side UI restrictions
instead of enforcing authorization on the server.

**Evidence:**

The admin interface is hidden from normal users,
but the endpoint remains accessible directly.

**Impact:**

An unauthorized user may attempt to access
administrative functionality directly.

**Recommendation:**

Implement server-side authorization checks for
every protected administrative endpoint.

Authorization should be based on the authenticated
user's permissions rather than UI visibility.
```

---

# 🧪 Recommended Hands-on Workflow

Complete the project in this order:

```text
1. Build Application
        ↓
2. Add Registration
        ↓
3. Add Login
        ↓
4. Add Password Hashing
        ↓
5. Add Sessions
        ↓
6. Add Dashboard
        ↓
7. Add Logging
        ↓
8. Analyze Logs
        ↓
9. Perform Security Review
        ↓
10. Document Findings
        ↓
11. Fix Weaknesses
        ↓
12. Create Final Report
```

---

# 🛠️ Tools You Can Use

| Tool                 | Purpose                         |
| -------------------- | ------------------------------- |
| 🐍 Python            | Backend development             |
| 🌐 Flask             | Web application framework       |
| 🔎 Browser DevTools  | Inspect requests/responses      |
| 🧪 Burp Suite        | HTTP security testing           |
| 📋 Wireshark         | Network traffic analysis        |
| 📝 Python `logging`  | Application logging             |
| 🗄️ SQLite           | Local database                  |
| 🔐 Werkzeug / Argon2 | Password hashing                |
| 🤖 AI                | Code/security review assistance |

Use security tools only against applications you own or are explicitly authorized to test.

---

# 🔗 How This Connects to Previous Topics

```text
HTML
 ↓
Forms + User Input
 ↓
HTTP
 ↓
Backend
 ↓
API
 ↓
Authentication
 ↓
Sessions
 ↓
Database
 ↓
Logging
 ↓
Security Analysis
 ↓
Secure Development
 ↓
Security Report
```

This single task brings together most of the Web Security module.

---

# ⚠️ Security Checklist

Before calling the application secure, review:

### 🌐 Application

* [ ] Server-side validation
* [ ] Safe error handling
* [ ] Secure configuration
* [ ] Dependencies updated

### 🔐 Authentication

* [ ] Passwords hashed securely
* [ ] HTTPS used in real deployments
* [ ] Login protections
* [ ] Secure sessions
* [ ] Logout invalidates sessions

### 🛡️ Authorization

* [ ] Server-side permission checks
* [ ] Admin endpoints protected
* [ ] Users cannot access unauthorized resources

### 🗄️ Data

* [ ] Parameterized SQL
* [ ] Secrets protected
* [ ] Sensitive data minimized
* [ ] Secure backups

### 📋 Logging

* [ ] Important security events logged
* [ ] Timestamps included
* [ ] Logs protected
* [ ] Secrets excluded
* [ ] Failed authentication monitored

### 🔎 Testing

* [ ] Input validation tested
* [ ] Authentication tested
* [ ] Authorization tested
* [ ] Session behavior tested
* [ ] Error handling tested
* [ ] Security findings documented

---

# 🧠 Important Security Principles

### 1. Never Trust the Client

```text
Browser = UNTRUSTED
```

### 2. Authentication ≠ Authorization

```text
Authentication → Who are you?

Authorization → What can you do?
```

### 3. Hidden ≠ Secure

```text
Hidden button
     ≠
Protected endpoint
```

### 4. Logs ≠ Proof

```text
Suspicious event
     ↓
Investigation
     ↓
Conclusion
```

### 5. Security Is a Lifecycle

```text
Build
 ↓
Test
 ↓
Find
 ↓
Fix
 ↓
Verify
 ↓
Monitor
 ↓
Improve
```

---

# 🧠 Memory Trick

Remember:

> **B → A → L → I → R**

```text
B → Build
A → Authenticate
L → Logs
I → Identify weaknesses
R → Report
```

### 🔥 Final Takeaway

> **A cybersecurity professional should not only know how to build a web application — they should understand how the application processes input, authenticates users, enforces authorization, records activity, handles failures, and protects data.**

This hands-on task turns the concepts from the Web Security module into a practical security workflow:

```text
🌐 BUILD
   ↓
🔐 AUTHENTICATE
   ↓
📋 LOG
   ↓
🛡️ ANALYZE
   ↓
🔧 FIX
   ↓
📄 REPORT
```
