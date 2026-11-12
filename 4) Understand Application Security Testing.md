# 🛡️ Understanding Application Security Testing

Application Security Testing (AST) is the process of examining an application to identify security weaknesses, verify security controls, and determine whether the application can withstand potential attacks.

It helps developers and security professionals discover vulnerabilities **before attackers exploit them**.

Security testing should not happen only after development. It should be integrated throughout the Software Development Life Cycle (SDLC).

---

## 1. 🔍 Security Reviews

### What Is a Security Review?

A security review is a structured examination of an application's design, source code, configuration, architecture, and security controls to identify potential security weaknesses.

Unlike ordinary software testing, which mainly checks whether an application works correctly, security reviews examine whether it works securely.

**Example:**

Imagine a banking application that allows users to view their account balance.

A normal functional test checks:

- Can the user log in?
- Does the account balance display correctly?
- Can the user transfer money?

A security review asks:

- Can one user access another user's account?
- Are passwords stored securely?
- Can an unauthenticated user access sensitive endpoints?
- Are API requests properly authorized?
- Is sensitive information exposed in application logs?

### Types of Security Reviews

| Review Type | Description | Main Focus |
|---|---|---|
| Architecture Review | Examines the application's overall structure | Design-level security weaknesses |
| Design Review | Examines security decisions made during development | Authentication, authorization, data protection |
| Code Review | Examines source code for insecure implementations | Coding vulnerabilities |
| Configuration Review | Checks application and server configurations | Misconfigurations |
| Dependency Review | Examines third-party libraries and packages | Known vulnerabilities and supply-chain risks |

### Manual Security Code Review

Manual code review involves a security professional examining source code to identify vulnerabilities that automated tools may miss.

Consider this Python example:

```python
import sqlite3

username = input("Enter username: ")

connection = sqlite3.connect("users.db")

query = "SELECT * FROM users WHERE username = '" + username + "'"

result = connection.execute(query)
```

**Security problem:**

The application directly combines user input with an SQL query.

An attacker may manipulate the input to change the query's intended behavior. This is known as **SQL Injection (SQLi)**.

A safer implementation uses parameterized queries:

```python
import sqlite3

username = input("Enter username: ")

connection = sqlite3.connect("users.db")

query = "SELECT * FROM users WHERE username = ?"

result = connection.execute(query, (username,))
```

The database treats the supplied username as data rather than executable SQL syntax.

### What Should Security Reviewers Look For?

- Hardcoded passwords, API keys, and secrets.
- Weak authentication mechanisms.
- Missing authorization checks.
- SQL injection vulnerabilities.
- Cross-Site Scripting (XSS).
- Insecure file uploads.
- Sensitive information exposure.
- Insecure session management.
- Unsafe cryptographic implementations.
- Vulnerable third-party dependencies.
- Insecure configurations.

### Security Review vs. Functional Review

| Functional Review | Security Review |
|---|---|
| Checks whether features work | Checks whether features work securely |
| Focuses on expected behavior | Focuses on expected and unexpected behavior |
| Tests normal user interactions | Examines potentially malicious interactions |
| Finds functional defects | Finds security weaknesses |

**Important:** A feature can work perfectly while still containing serious security vulnerabilities.

---

## 2. 📄 Static Analysis Awareness

### What Is Static Analysis?

Static Application Security Testing (SAST) is a security testing technique that examines an application's source code, bytecode, or intermediate representation **without executing the application**.

It identifies potentially insecure coding patterns before the application reaches production.

In simple terms:

> Static analysis examines the code to identify security problems without running the program.

### How Does SAST Work?

A typical SAST process:

1. A developer writes application code.
2. A SAST tool analyzes the source code.
3. The tool searches for insecure patterns and dangerous functions.
4. It identifies potential vulnerabilities.
5. It reports findings with file names, code locations, severity, and sometimes remediation suggestions.
6. Developers investigate and fix confirmed issues.

**Example:**

Consider this code:

```python
import os

filename = input("Enter filename: ")

os.system("cat " + filename)
```

The application passes user-controlled input into a system command.

This creates a potential **OS Command Injection** vulnerability.

A static analysis tool may identify this dangerous use of `os.system()` and trace whether untrusted input reaches it.

A safer approach is to avoid invoking a shell when possible:

```python
import subprocess

filename = input("Enter filename: ")

subprocess.run(["cat", "--", filename], check=True)
```

This avoids shell interpretation. However, applications must still validate filenames and restrict access to authorized files.

### What Can SAST Detect?

| Vulnerability | Description |
|---|---|
| SQL Injection | Unsafe construction of database queries |
| Hardcoded Secrets | Passwords or API keys stored in source code |
| Command Injection | Untrusted input reaching command execution |
| Path Traversal | Unsafe file path handling |
| Weak Cryptography | Use of insecure cryptographic algorithms |
| Insecure Deserialization | Unsafe handling of serialized data |
| Sensitive Data Exposure | Potentially unsafe handling of confidential information |

SAST tools can also identify insecure functions, missing validation patterns, and certain authorization weaknesses.

### Common SAST Tools

| Tool | Description |
|---|---|
| Semgrep | Rule-based static analysis for multiple programming languages |
| SonarQube | Code quality and security analysis |
| Bandit | Identifies common security issues in Python code |
| CodeQL | Analyzes code using query-based security analysis |
| Checkmarx | Commercial application security testing platform |

### Example: Using Bandit

Bandit is a Python security analysis tool.

Install it:

```bash
pip install bandit
```

Analyze a Python file:

```bash
bandit app.py
```

Analyze an entire project:

```bash
bandit -r .
```

Generate a JSON report:

```bash
bandit -r . -f json -o bandit-report.json
```

Bandit examines Python code for potentially dangerous patterns and produces findings for further investigation.

### Advantages of SAST

- Can detect vulnerabilities early in development.
- Does not require a running application.
- Can analyze code automatically.
- Can be integrated into CI/CD pipelines.
- Helps developers understand insecure coding patterns.

### Limitations of SAST

- May report false positives.
- Cannot reliably identify every vulnerability.
- May not understand the application's complete runtime behavior.
- Can miss vulnerabilities involving complex business logic.
- Results depend on tool capabilities, rules, and code coverage.

**Important Concept: False Positive vs. False Negative**

| Term | Meaning |
|---|---|
| False Positive | The tool reports a vulnerability, but investigation shows that the reported issue is not actually exploitable. |
| False Negative | A real vulnerability exists, but the tool fails to detect it. |

Security professionals must validate automated findings rather than blindly trusting tool output.

---

## 3. ⚡ Dynamic Analysis Awareness

### What Is Dynamic Analysis?

Dynamic Application Security Testing (DAST) is a security testing technique that examines an application **while it is running**.

Instead of analyzing source code, DAST interacts with the application through its exposed interfaces, such as:

- Web pages.
- Login forms.
- Search fields.
- API endpoints.
- HTTP requests and responses.
- Session management mechanisms.

In simple terms:

> Dynamic analysis tests the behavior of a running application by sending requests and examining its responses.

### How Does DAST Work?

A typical DAST process:

1. Deploy the application in a test environment.
2. Configure the DAST tool with the application URL.
3. The tool discovers accessible pages and endpoints.
4. It sends normal and specially crafted requests.
5. It examines application responses.
6. It identifies suspicious behavior and potential vulnerabilities.
7. Security professionals validate the findings.

### Example: Testing for SQL Injection

Imagine a login endpoint:

```http
POST /login HTTP/1.1
Host: example.test
Content-Type: application/x-www-form-urlencoded

username=admin&password=test123
```

A DAST tool may test how the application handles unusual input.

For example, it may submit specially crafted input to determine whether the application improperly constructs SQL queries.

The tool examines responses for evidence of SQL injection, such as unexpected database errors or differences in application behavior.

A response alone does not always prove a vulnerability. The finding must be investigated and validated.

### What Can DAST Detect?

| Vulnerability | Description |
|---|---|
| SQL Injection | Unsafe handling of input in database queries |
| Cross-Site Scripting (XSS) | Untrusted input being executed as browser-side script |
| Security Misconfiguration | Insecure application or server settings |
| Authentication Weaknesses | Problems with login and authentication behavior |
| Session Management Issues | Weak or improperly handled sessions |
| Information Disclosure | Exposure of sensitive information |
| Certain Access Control Issues | Unauthorized access through exposed application interfaces |

### Common DAST Tools

| Tool | Description |
|---|---|
| OWASP ZAP | Open-source web application security testing tool |
| Burp Suite | Web security testing platform |
| Invicti | Commercial web application security scanner |
| Acunetix | Automated web vulnerability scanner |
| Wapiti | Open-source web application vulnerability scanner |

### Example: Using OWASP ZAP

OWASP ZAP is a popular tool for testing web application security.

Its common capabilities include:

- Spidering: Discovering application pages and links.
- Passive Scanning: Examining requests and responses without actively attacking the application.
- Active Scanning: Sending test payloads to identify potential vulnerabilities.
- Request Inspection: Examining HTTP requests and responses.
- Reporting: Generating vulnerability reports.

**Basic testing workflow:**

1. Start OWASP ZAP.
2. Configure a deliberately vulnerable test application.
3. Access the application through ZAP's browser.
4. Allow ZAP to discover application pages.
5. Review passive scanning alerts.
6. Run active scanning only against an authorized target.
7. Examine and validate the results.

Use deliberately vulnerable applications such as OWASP Juice Shop or DVWA for practice.

### Advantages of DAST

- Tests real application behavior.
- Does not require source code.
- Can identify certain runtime vulnerabilities.
- Helps discover issues caused by server configurations.
- Can test applications built using different programming languages.

### Limitations of DAST

- Requires a running application.
- May not discover every page or API endpoint.
- Can miss vulnerabilities behind authentication.
- May struggle with complex business logic.
- Can generate false positives.
- Active scanning may modify data or disrupt services if performed carelessly.

---

## 4. 🔎 Vulnerability Discovery Concepts

### What Is Vulnerability Discovery?

Vulnerability discovery is the process of identifying security weaknesses in an application, its source code, dependencies, configuration, architecture, or runtime behavior.

A vulnerability is a weakness that could be exploited to compromise the confidentiality, integrity, or availability of a system.

The objective is not simply to find suspicious code. It is to understand:

- What is the weakness?
- Where does it exist?
- Why does it exist?
- How could an attacker exploit it?
- What impact could it have?
- How can it be fixed?

### Common Application Vulnerabilities

#### 1. SQL Injection (SQLi)

Occurs when untrusted input is incorporated into SQL queries in an unsafe way.

**Potential impact:**
- Unauthorized database access.
- Exposure of sensitive information.
- Modification or deletion of database records.

**Prevention:**
- Use parameterized queries.
- Avoid dynamically constructing SQL with untrusted input.
- Apply least-privilege database permissions.

#### 2. Cross-Site Scripting (XSS)

Occurs when untrusted data is included in a web page in a way that allows malicious JavaScript to execute in another user's browser.

**Example:**

An application displays a user's comment without safely encoding it.

An attacker may submit malicious HTML or JavaScript that executes when another user views the comment.

**Potential impact:**
- Unauthorized actions through a victim's browser.
- Theft of accessible session information.
- Modification of displayed content.
- Phishing through manipulated pages.

**Prevention:**
- Apply context-appropriate output encoding.
- Use safe DOM APIs.
- Sanitize HTML when rich HTML input is required.
- Implement Content Security Policy (CSP) as an additional defense.

#### 3. Broken Access Control

Occurs when an application fails to enforce restrictions on what authenticated users are allowed to access.

**Example:**

A user accesses:

```http
GET /api/users/1001/profile
```

The application returns the profile belonging to user `1001`.

The user then changes the identifier:

```http
GET /api/users/1002/profile
```

If the application returns another user's private profile without checking ownership or permissions, it contains an access control vulnerability.

This type of issue may be an example of **Insecure Direct Object Reference (IDOR)**.

**Prevention:**
- Enforce authorization on the server.
- Verify resource ownership.
- Check permissions on every sensitive request.
- Deny access by default.

#### 4. Security Misconfiguration

Occurs when application or infrastructure settings introduce security weaknesses.

Examples:

- Debug mode enabled in production.
- Default administrator credentials.
- Unnecessary services exposed.
- Directory listing enabled.
- Sensitive error messages exposed.
- Insecure HTTP security headers.

**Prevention:**
- Use secure configuration baselines.
- Disable unnecessary services.
- Remove default credentials.
- Separate development and production configurations.
- Regularly review server and application settings.

#### 5. Vulnerable and Outdated Components

Applications often depend on external libraries, frameworks, and packages.

If a dependency contains a known vulnerability, the application may inherit that risk.

**Example:**

A web application uses an outdated version of a JavaScript library containing a publicly documented security vulnerability.

An attacker may exploit that weakness depending on how the library is used and whether the vulnerable code path is reachable.

**Prevention:**
- Maintain a software inventory.
- Scan dependencies regularly.
- Monitor security advisories.
- Update vulnerable packages.
- Remove unnecessary dependencies.

### Vulnerability Discovery Techniques

| Technique | Purpose |
|---|---|
| Manual Code Review | Identifies insecure coding and logic |
| SAST | Examines source code without execution |
| DAST | Tests a running application |
| Software Composition Analysis (SCA) | Identifies vulnerable third-party dependencies |
| Fuzz Testing | Sends unexpected or malformed inputs to find failures |
| Configuration Review | Identifies insecure settings |
| Penetration Testing | Attempts to validate and exploit weaknesses within an authorized scope |
| Threat Modeling | Identifies possible attack paths during design |

### Understanding CVE and CVSS

When a vulnerability is discovered, it may be documented in a public vulnerability database.

**CVE — Common Vulnerabilities and Exposures**

CVE provides standardized identifiers for publicly disclosed cybersecurity vulnerabilities.

Example format:

```text
CVE-2024-XXXX
```

A CVE identifier helps security professionals refer to a specific vulnerability consistently.

**CVSS — Common Vulnerability Scoring System**

CVSS provides a standardized way to communicate the technical severity of vulnerabilities.

CVSS scores range from 0.0 to 10.0.

| CVSS Score | Severity |
|---|---|
| 0.0 | None |
| 0.1–3.9 | Low |
| 4.0–6.9 | Medium |
| 7.0–8.9 | High |
| 9.0–10.0 | Critical |

CVSS severity is useful, but it does not automatically determine business risk.

For example, a critical vulnerability in an isolated test system may have less immediate business impact than a medium-severity vulnerability in an internet-facing banking application.

Risk prioritization should also consider:

- Whether the application is exposed to the internet.
- Whether exploitation is possible in the actual environment.
- Whether sensitive data is affected.
- Whether exploitation is occurring in the wild.
- Business importance.
- Available mitigations.

---

## 5. ✅ Security Validation

### What Is Security Validation?

Security validation is the process of confirming that identified vulnerabilities have been correctly fixed and that the application's security controls work as intended.

Finding a vulnerability is only part of the job.

A security professional must verify that the remediation actually solves the problem.

**Example:**

A developer discovers SQL injection in a login endpoint and replaces unsafe SQL construction with parameterized queries.

The security team must test the endpoint again to verify that:

1. Normal login functionality still works.
2. Malicious SQL input is treated as data.
3. The database query cannot be manipulated through user input.
4. No related injection vulnerability remains in other endpoints.

### Security Validation Process

**Step 1: Identify the Vulnerability**

Use security reviews, SAST, DAST, or manual testing to discover a potential weakness.

**Step 2: Analyze the Finding**

Determine whether it is a genuine vulnerability or a false positive.

Understand its root cause, affected component, and potential impact.

**Step 3: Apply Remediation**

The development team fixes the underlying problem.

Examples:

- SQL Injection → Parameterized queries.
- XSS → Context-aware output encoding.
- Broken Access Control → Server-side authorization checks.
- Hardcoded Secrets → Secure secret management.
- Vulnerable Dependencies → Upgrade or replace affected packages.

**Step 4: Retest the Application**

Repeat the original security test to confirm the vulnerability is no longer present.

**Step 5: Perform Regression Testing**

Check that the security fix has not broken existing functionality or introduced new weaknesses.

**Step 6: Document the Result**

Update the vulnerability report with the remediation status and validation evidence.

### Retesting vs. Regression Testing

These two concepts are related but different.

| Retesting | Regression Testing |
|---|---|
| Confirms that a specific vulnerability has been fixed | Checks whether the fix has affected other functionality |
| Focuses on the original security issue | Focuses on unintended side effects |
| Repeats the original vulnerability test | Runs relevant existing tests |
| Example: Verify SQL injection is no longer possible | Verify login and other database operations still work |

### Security Validation Checklist

Before declaring a vulnerability resolved, verify:

- [ ] The original vulnerability is no longer reproducible.
- [ ] The underlying root cause has been addressed.
- [ ] Related endpoints and components have been checked.
- [ ] Existing functionality still works.
- [ ] No new security issues were introduced.
- [ ] The fix is documented.
- [ ] The vulnerability status is updated.

**Important:** A vulnerability should not be marked as fixed merely because a developer says the code was changed. It should be validated using appropriate evidence.

---

# 🔄 Understanding SAST vs. DAST

SAST and DAST are two major application security testing approaches. They complement each other rather than replace each other.

| Feature | SAST | DAST |
|---|---|---|
| Full Form | Static Application Security Testing | Dynamic Application Security Testing |
| Application State | Not running | Running |
| Main Target | Source code or compiled representation | Running application |
| Source Code Required | Usually yes | No |
| Testing Approach | Examines code and data flows | Sends requests and analyzes responses |
| Finds | Insecure coding patterns | Runtime vulnerabilities and behavior |
| Testing Stage | Often during development | Often during testing or staging |
| Example Tools | Semgrep, Bandit, CodeQL | OWASP ZAP, Burp Suite |

### Simple Real-World Analogy

Imagine a software company building a banking application.

**SAST:** A security engineer examines the application's source code to identify unsafe SQL queries, hardcoded passwords, and insecure functions.

**DAST:** Another security engineer runs the application and tests its login page, APIs, sessions, and access controls using specially crafted HTTP requests.

**Manual Security Review:** A reviewer examines the application's architecture, source code, and business logic to identify issues automated tools may miss.

**Security Validation:** After developers fix the reported vulnerabilities, the security team retests the application to verify the fixes.

Using these methods together provides broader security coverage.

---

# 🏗️ Application Security Testing in the SDLC

Application security testing should be integrated into different stages of software development.

| SDLC Phase | Security Testing Activity |
|---|---|
| Requirements | Identify security requirements and compliance needs |
| Design | Threat modeling and architecture reviews |
| Development | Secure code reviews and SAST |
| Build | Dependency scanning and secret detection |
| Testing | DAST, penetration testing, and manual validation |
| Deployment | Configuration and security checks |
| Maintenance | Continuous monitoring, vulnerability scanning, and patch validation |

### Example: Secure E-Commerce Application

Imagine a company developing an e-commerce platform.

It includes:

- User registration and login.
- Product search.
- Shopping cart.
- Online payments.
- User profiles.
- Administrator dashboard.

The security team can apply different testing methods:

| Application Feature | Security Testing |
|---|---|
| Login | Authentication and session testing |
| Registration | Input validation and password storage review |
| Product Search | SQL injection and XSS testing |
| Shopping Cart | Authorization and business logic testing |
| Payment System | Sensitive data protection and access control testing |
| Admin Dashboard | Privilege and authorization testing |
| Third-Party Packages | Dependency vulnerability scanning |

The purpose is to identify security weaknesses before attackers can exploit them.

---

# 🧪 Practical Lab: Introduction to Application Security Testing

Theory becomes much more useful when you apply it to real applications.

For practice, use intentionally vulnerable applications and your own test code.

### Lab 1: Static Analysis with Bandit

**Objective:** Identify potentially insecure patterns in Python code.

Create a file named:

```text
security_test.py
```

Add:

```python
import os

password = "admin123"

user_input = input("Enter command: ")

os.system(user_input)
```

Install Bandit:

```bash
pip install bandit
```

Run:

```bash
bandit security_test.py
```

Observe:

- What security issues does Bandit report?
- Why is `os.system()` dangerous with untrusted input?
- Why is hardcoding passwords a security weakness?
- Which findings require manual investigation?

Do not assume every scanner finding represents an exploitable vulnerability.

### Lab 2: Dynamic Analysis with OWASP ZAP

**Objective:** Understand how a DAST tool examines a running application.

Use an authorized training environment such as OWASP Juice Shop.

Suggested workflow:

1. Start the training application.
2. Open OWASP ZAP.
3. Configure the browser to use ZAP.
4. Browse the application.
5. Examine discovered URLs and HTTP requests.
6. Review passive scanning alerts.
7. If appropriate, run an active scan against the local training application.
8. Examine the findings and understand their security impact.

Observe:

- Which pages were discovered?
- What information is present in HTTP requests?
- What security headers are missing?
- Which findings require manual verification?

### Lab 3: Security Review

Review a small application or code sample.

Look for:

- Hardcoded credentials.
- Unsafe SQL queries.
- Missing input validation.
- Missing authorization checks.
- Insecure error handling.
- Sensitive information exposure.

Write down each issue and explain why it creates a security risk.

### Lab 4: Security Validation

Take one confirmed vulnerability from your training application.

1. Document the original behavior.
2. Identify the root cause.
3. Apply an appropriate fix.
4. Repeat the original test.
5. Check that normal application functionality still works.
6. Record the validation result.

**Safety note:** Perform active security testing only against systems you own or have explicit permission to test. Avoid scanning public websites or production systems without authorization.

---

# 📋 Application Security Testing Report

Professional security teams document their findings so developers can understand and resolve them.

A vulnerability report commonly includes:

| Field | Description |
|---|---|
| Finding ID | Unique identifier for the finding |
| Vulnerability Name | Name of the security issue |
| Affected Component | File, endpoint, service, or application |
| Severity | Critical, High, Medium, or Low |
| Description | Explanation of the vulnerability |
| Evidence | Supporting technical evidence |
| Impact | What could happen if exploited |
| Remediation | Recommended solution |
| Status | Open, In Progress, Fixed, or Verified |
| Retest Result | Whether the vulnerability was successfully resolved |

### Example Finding

| Field | Example |
|---|---|
| Finding ID | APP-001 |
| Vulnerability | SQL Injection |
| Affected Component | Login endpoint |
| Severity | High (illustrative) |
| Description | User-controlled input is directly incorporated into an SQL query. |
| Impact | An attacker may manipulate database queries and access unauthorized information. |
| Remediation | Use parameterized SQL queries and least-privilege database permissions. |
| Status | Fixed |
| Retest Result | Verified — original injection test no longer succeeds. |

A real severity rating must be based on the actual technical conditions and business impact, not simply assigned from an example.

---

# 🧠 Quick Revision

| Concept | Remember This |
|---|---|
| Application Security Testing | Testing an application to identify and validate security weaknesses |
| Security Review | Examining design, code, architecture, and configurations |
| SAST | Analyzes code without executing the application |
| DAST | Tests a running application |
| Vulnerability Discovery | Identifying weaknesses that could be exploited |
| False Positive | Reported issue is not a genuine vulnerability |
| False Negative | A genuine vulnerability is missed |
| CVE | Standard identifier for a publicly disclosed vulnerability |
| CVSS | Standardized vulnerability severity scoring system |
| Security Validation | Confirms that a security fix works |
| Retesting | Checks whether the original vulnerability is fixed |
| Regression Testing | Checks whether the fix broke other functionality |

## 🔑 Key Takeaways

- Application security testing is an essential part of Secure SDLC.
- Security reviews help identify weaknesses in application design, code, and configuration.
- SAST analyzes code without running the application.
- DAST examines a running application through its interfaces.
- Automated security tools can produce false positives and miss real vulnerabilities.
- Vulnerability discovery must include understanding the root cause and potential impact.
- CVE identifies publicly disclosed vulnerabilities, while CVSS communicates technical severity.
- Security validation confirms that vulnerabilities have actually been fixed.
- Retesting and regression testing are both important after remediation.
- Combining manual reviews, SAST, DAST, dependency analysis, and penetration testing provides stronger application security coverage.

---

# 🎯 Active Recall — Test Your Understanding

Try answering these questions without looking at the notes.

<details>
<summary><strong>1. What is Application Security Testing, and why is it important?</strong></summary>

Application Security Testing is the process of identifying, analyzing, and validating security weaknesses in an application. It helps prevent attackers from exploiting vulnerabilities and protects application data and functionality.
</details>

<details>
<summary><strong>2. What is the difference between SAST and DAST?</strong></summary>

SAST analyzes source code or compiled representations without running the application. DAST tests an application while it is running by sending requests and examining responses.
</details>

<details>
<summary><strong>3. Can SAST identify every application vulnerability?</strong></summary>

No. SAST may miss runtime issues, complex business logic vulnerabilities, and problems that require environmental context. Manual review and other testing methods are also necessary.
</details>

<details>
<summary><strong>4. Why does DAST require a running application?</strong></summary>

Because DAST examines actual application behavior by interacting with its interfaces, sending requests, and analyzing responses.
</details>

<details>
<summary><strong>5. What is a false positive?</strong></summary>

A false positive occurs when a security tool reports a potential vulnerability that investigation shows is not actually a vulnerability.
</details>

<details>
<summary><strong>6. What is the difference between CVE and CVSS?</strong></summary>

CVE provides a standardized identifier for a publicly disclosed vulnerability. CVSS provides a standardized method for communicating its technical severity.
</details>

<details>
<summary><strong>7. What is the purpose of security validation?</strong></summary>

Security validation confirms that a vulnerability has been correctly fixed and that the application's security controls work as intended.
</details>

<details>
<summary><strong>8. What is the difference between retesting and regression testing?</strong></summary>

Retesting verifies that the original vulnerability has been fixed. Regression testing checks whether the fix has negatively affected existing functionality.
</details>

<details>
<summary><strong>9. Why should automated security findings be manually verified?</strong></summary>

Automated tools can produce false positives, miss vulnerabilities, or misunderstand application context. Human investigation helps confirm exploitability, impact, and the correct remediation.
</details>

<details>
<summary><strong>10. Why should SAST and DAST be used together?</strong></summary>

They examine different aspects of application security. SAST identifies potentially insecure code patterns, while DAST tests actual runtime behavior. Combining them provides broader coverage.
</details>

---

**Professional Mindset:** A security tester does not simply run a scanner and copy its report. A good security professional understands the application, investigates findings, validates vulnerabilities, explains their impact, recommends appropriate fixes, and verifies the remediation.
