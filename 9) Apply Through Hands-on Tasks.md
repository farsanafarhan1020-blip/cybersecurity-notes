# 🛠️ Hands-on Secure Development and DevSecOps Tasks

Understanding Secure SDLC, DevSecOps, CI/CD security, and software supply chain security is not enough by itself. A cybersecurity professional must be able to examine real applications, identify weaknesses, evaluate risks, recommend controls, and document findings.

This practical section combines the concepts from the Secure Software Development Lifecycle and SecOps module into realistic security assessment activities.

You will perform five major tasks:

1. Review application architectures.
2. Analyze security weaknesses.
3. Assess software supply chain risks.
4. Design secure development workflows.
5. Create DevSecOps assessment reports.

---

# 1. Review Application Architectures

## What Is an Application Architecture Review?

An **Application Architecture Review** is the process of examining how an application is designed, how its components communicate, how information flows, and how security controls protect the system.

The objective is to identify architectural weaknesses before they become vulnerabilities in a deployed application.

A security professional should understand:

- What components make up the application?
- Where is sensitive information stored?
- How do users authenticate?
- How is access controlled?
- How do application components communicate?
- Which components are exposed to the internet?
- Where are trust boundaries located?
- What happens if one component is compromised?

## Practical Scenario: E-Commerce Application

Imagine an organization is developing an e-commerce application.

The application contains:

- Frontend web application.
- Backend API.
- Authentication service.
- Customer database.
- Payment gateway.
- Administrator dashboard.
- Third-party services.

### Sample Architecture

```text
                   INTERNET
                       |
                       v
                +-------------+
                |   Frontend  |
                | Web App     |
                +-------------+
                       |
                       v
                +-------------+
                |  Backend API|
                +-------------+
                  /    |     \
                 /     |      \
                v      v       v
           +------+ +------+ +----------+
           | Auth | |  DB  | | Payment  |
           |Service| |     | | Gateway  |
           +------+ +------+ +----------+
                       |
                       v
                Customer Data
```

This is a simplified conceptual architecture. A real application may use additional services, network segments, queues, caches, and infrastructure components.

## What Should You Examine?

### A. Authentication

Check how the application verifies user identities.

Questions:

- Are passwords securely hashed?
- Is MFA available for privileged accounts?
- Are login attempts rate-limited?
- Are authentication tokens protected?
- Are sessions invalidated after logout?
- Is account recovery secure?

### B. Authorization

Authentication confirms identity, while authorization determines what that identity can access.

Check:

- Does the backend verify permissions?
- Are administrator functions restricted?
- Can users access other users' information?
- Are permissions based on roles?
- Is least privilege followed?

**Example:** A normal customer should not be able to access the administrator dashboard simply by changing a URL.

### C. Network Security

Examine communication between application components.

Check:

- Is HTTPS used for external communication?
- Are internal services unnecessarily exposed?
- Is database access restricted?
- Are administrative interfaces protected?
- Are network access rules appropriately configured?

### D. Data Protection

Identify sensitive information and how it is protected.

Examples:

- Password hashes.
- Payment-related information.
- Customer personal information.
- Authentication tokens.
- API credentials.

Check encryption, access restrictions, storage practices, and data retention.

### E. Logging and Monitoring

Determine whether important security events are recorded.

Examples:

- Failed login attempts.
- Privilege changes.
- Administrator activity.
- Sensitive data access.
- Configuration changes.
- Suspicious API requests.

Logs should be protected against unauthorized modification and should not expose passwords, tokens, or unnecessary personal information.

## Architecture Review Checklist

| Security Area | Review Question | Status |
|---|---|---|
| Authentication | Is identity verification secure? | Pending |
| Authorization | Are permissions enforced server-side? | Pending |
| Network security | Are services appropriately isolated? | Pending |
| Data protection | Is sensitive information protected? | Pending |
| API security | Are API requests authenticated and authorized? | Pending |
| Secrets management | Are credentials stored securely? | Pending |
| Logging | Are important security events recorded? | Pending |
| Dependencies | Are third-party components managed securely? | Pending |
| Deployment | Are production configurations secure? | Pending |

**Task:** Draw an architecture diagram for an application and complete this checklist. Mark each item as Pass, Fail, or Requires Investigation, with evidence.

---

# 2. Analyze Security Weaknesses

## What Is Security Weakness Analysis?

Security weakness analysis is the process of examining an application, its source code, configurations, dependencies, and architecture to identify weaknesses that could be exploited by attackers.

A weakness does not automatically mean that a successful attack is possible. You must investigate the conditions required for exploitation and the potential impact.

### Common Application Security Weaknesses

| Weakness | Description | Potential Impact |
|---|---|---|
| SQL Injection | Untrusted input changes SQL query behavior | Database compromise |
| Cross-Site Scripting (XSS) | Untrusted content executes as script in a browser | Account compromise or data theft |
| Broken Access Control | Users access resources without proper permission | Unauthorized data access |
| Weak Authentication | Inadequate identity verification | Account takeover |
| Security Misconfiguration | Unsafe application or server settings | Unauthorized access or exposure |
| Insecure Deserialization | Unsafe processing of untrusted serialized data | Data manipulation or code execution in vulnerable implementations |
| Path Traversal | Manipulating file paths to access unintended files | Sensitive file disclosure |
| Sensitive Data Exposure | Inadequate protection of confidential information | Privacy and data breaches |
| Vulnerable Dependencies | Using components with known security vulnerabilities | Application compromise |

## Practical Task: Review a Sample Application

Consider the following Python code:

```python
import sqlite3

def login(username, password):

    connection = sqlite3.connect("users.db")
    cursor = connection.cursor()

    query = f"""
    SELECT * FROM users
    WHERE username = '{username}'
    AND password = '{password}'
    """

    cursor.execute(query)

    user = cursor.fetchone()

    if user:
        return "Login successful"

    return "Login failed"
```

### Identify the Weaknesses

**Weakness 1: SQL Injection**

User-controlled values are directly inserted into the SQL query.

An attacker may manipulate the input to alter the query's behavior.

**Recommended control:** Use parameterized SQL queries.

**Weakness 2: Insecure Password Storage Assumption**

The code compares the supplied password directly against a database value.

If passwords are stored in plaintext, a database compromise exposes users' passwords.

**Recommended control:** Store passwords using a dedicated password-hashing algorithm such as Argon2id, bcrypt, or scrypt. Verify submitted passwords using the corresponding password-verification function.

**Weakness 3: Missing Account Protection**

The example does not implement protections against repeated login attempts.

**Recommended controls:**

- Rate limiting.
- Monitoring failed authentication attempts.
- Appropriate account protection mechanisms.
- MFA where appropriate.

**Weakness 4: Incomplete Session Management**

The example only returns a message. It does not demonstrate secure session creation or management.

A production application needs secure session handling, including appropriate cookie settings, expiration, and session invalidation.

## Security Analysis Workflow

```text
       Application or Source Code
                  |
                  v
          Identify Components
                  |
                  v
          Examine Weaknesses
                  |
                  v
         Validate Each Finding
                  |
                  v
          Determine Severity
                  |
                  v
        Recommend Security Fixes
                  |
                  v
          Retest the Application
                  |
                  v
          Document the Results
```

## Security Testing Tools

| Tool | Purpose |
|---|---|
| Semgrep | Static analysis of source code |
| Bandit | Identifies common Python security issues |
| SonarQube | Code quality and security analysis |
| OWASP ZAP | Dynamic web application security testing |
| Burp Suite | Web application testing and traffic analysis |
| Nmap | Network and service discovery |
| Trivy | Vulnerability and configuration scanning |
| Gitleaks | Detects exposed secrets in repositories |

### Static vs Dynamic Analysis

**Static Application Security Testing (SAST):**

Examines source code or other application artifacts without executing the application.

Examples: Semgrep and Bandit.

**Dynamic Application Security Testing (DAST):**

Tests a running application by interacting with it.

Examples: OWASP ZAP and Burp Suite.

These approaches complement each other. Neither guarantees that an application is free of vulnerabilities.

---

# 3. Assess Software Supply Chain Risks

## What Is Software Supply Chain Risk Assessment?

Modern applications depend on more than the code written by their developers.

They also depend on:

- Open-source libraries.
- Third-party packages.
- Frameworks.
- Build tools.
- Container images.
- External services.
- Package repositories.
- CI/CD platforms.

The software supply chain includes the components and processes involved in developing, building, distributing, and deploying software.

A **Software Supply Chain Risk Assessment** examines whether these components and processes introduce security risks.

## Example: Python Application

Imagine a Python application uses the following dependencies:

```text
Flask
Requests
SQLAlchemy
Pillow
```

These are third-party packages.

Even if your own source code is secure, a vulnerable or compromised dependency can introduce security weaknesses.

### Common Supply Chain Risks

| Risk | Explanation |
|---|---|
| Vulnerable dependency | A package contains a known security vulnerability |
| Malicious package | A package intentionally contains malicious functionality |
| Typosquatting | An attacker publishes a package with a name similar to a legitimate package |
| Dependency confusion | A package manager selects an unintended package because of repository or naming ambiguity |
| Compromised maintainer | An attacker compromises a package maintainer's account |
| Unmaintained dependency | A package no longer receives security updates |
| Compromised build system | Attackers manipulate the software build process |
| Exposed secrets | Credentials leak through source code or CI/CD logs |
| Unsigned or unverified artifacts | Software integrity or origin cannot be adequately established |

## Practical Task: Dependency Assessment

### Step 1: List Installed Dependencies

For a Python environment:

```bash
pip list
```

To examine dependencies and their relationships:

```bash
pip install pipdeptree
pipdeptree
```

This helps identify direct and transitive dependencies.

- **Direct dependency:** A package explicitly required by your application.
- **Transitive dependency:** A package required by another dependency.

### Step 2: Audit Python Dependencies

Install and run:

```bash
pip install pip-audit
pip-audit
```

`pip-audit` checks Python dependencies against known vulnerability information.

It can identify packages and versions associated with known security advisories.

A reported vulnerability still requires investigation to understand whether the affected functionality is present and exploitable in your application.

### Step 3: Generate a Software Bill of Materials

A **Software Bill of Materials (SBOM)** is an inventory of software components and dependencies included in a software product.

Common SBOM formats include:

- SPDX.
- CycloneDX.

An SBOM helps security teams understand what components are present and investigate vulnerabilities affecting those components.

### Step 4: Scan with Trivy

Trivy can scan filesystems, repositories, container images, and other supported targets.

Example:

```bash
trivy fs .
```

This scans the current directory for supported security findings, including vulnerabilities and misconfigurations.

### Step 5: Check for Exposed Secrets

Use Gitleaks:

```bash
gitleaks detect --source .
```

It can identify patterns that may indicate exposed credentials or secrets.

A detected secret should be investigated. If a real credential has been exposed, removing it from the latest code is not enough: revoke or rotate it and investigate possible use.

## Supply Chain Assessment Checklist

| Area | What to Examine |
|---|---|
| Dependencies | Are packages trusted and maintained? |
| Vulnerabilities | Are known vulnerabilities present? |
| Version management | Are dependency versions controlled? |
| SBOM | Is a component inventory available? |
| Package integrity | Are packages obtained from trusted sources? |
| Build security | Are build processes protected? |
| Secrets | Are credentials excluded from repositories? |
| Monitoring | Are new vulnerabilities monitored? |
| Updates | Are security patches applied promptly? |

### Important Security Principle

**A secure application can still be compromised through an insecure dependency or build process.**

Supply chain security must cover the complete software lifecycle, not only application source code.

---

# 4. Design Secure Development Workflows

## What Is a Secure Development Workflow?

A secure development workflow integrates security activities into the software development process.

Instead of waiting until the application is completed, security checks are performed throughout development.

This is closely connected to **DevSecOps** and the principle of **Shift Left Security**.

### Traditional Development Workflow

```text
Planning
   |
   v
Development
   |
   v
Testing
   |
   v
Deployment
   |
   v
Security Testing
```

In this approach, security testing may happen too late, when fixing architectural or coding problems can be more expensive.

### Secure DevSecOps Workflow

```text
             Planning
                |
                v
          Threat Modeling
                |
                v
           Development
                |
                v
           Code Review
                |
                v
          SAST + SCA
                |
                v
           Build Process
                |
                v
          Artifact Scanning
                |
                v
          Security Testing
                |
                v
       Deployment Approval
                |
                v
          Production
                |
                v
       Security Monitoring
                |
                v
          Feedback Loop
```

Security is integrated into every relevant stage.

## Security Activities at Each Stage

| SDLC Stage | Security Activity |
|---|---|
| Planning | Define security requirements |
| Design | Perform threat modeling |
| Development | Follow secure coding practices |
| Code review | Examine code for security weaknesses |
| CI/CD | Automate security checks |
| Build | Protect build environments and artifacts |
| Testing | Perform SAST, DAST, and security validation |
| Deployment | Apply secure configurations and access controls |
| Maintenance | Monitor vulnerabilities and apply patches |
| Operations | Investigate security events and incidents |

## Practical Task: Design a Secure GitHub Workflow

Imagine you are developing a Python web application using GitHub.

Your workflow should include the following controls.

### 1. Source Code Management

- Use Git for version control.
- Protect important branches.
- Require code reviews for sensitive changes.
- Restrict repository permissions.
- Avoid committing credentials.

### 2. Static Code Analysis

Use Bandit to examine Python code.

```bash
pip install bandit
bandit -r .
```

This checks source code for several common security issues.

### 3. Dependency Scanning

Use:

```bash
pip-audit
```

This helps identify known vulnerabilities in installed Python dependencies.

### 4. Secret Detection

Use:

```bash
gitleaks detect --source .
```

This helps identify potentially exposed credentials in the repository.

### 5. Dynamic Security Testing

Run your application in a controlled test environment and use OWASP ZAP to examine its security.

### 6. Deployment Security

Before deployment:

- Review security findings.
- Protect deployment credentials.
- Restrict production access.
- Verify application configuration.
- Ensure secrets are supplied through an approved secrets-management mechanism.
- Require appropriate approval for production releases.

### 7. Continuous Monitoring

After deployment:

- Monitor application logs.
- Track new dependency vulnerabilities.
- Investigate suspicious activity.
- Apply security patches.
- Review access permissions.

## Example: Basic GitHub Actions Security Workflow

GitHub Actions can automate security checks whenever code is pushed or a pull request is created.

Create this file in your repository:

`.github/workflows/security.yml`

```yaml
name: Security Checks

on:
  push:
  pull_request:

jobs:
  security:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Set Up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install Security Tools
        run: |
          python -m pip install --upgrade pip
          pip install bandit pip-audit

      - name: Run Bandit
        run: bandit -r .

      - name: Run Dependency Audit
        run: pip-audit
```

### Understanding the Workflow

| Component | Purpose |
|---|---|
| `name` | Defines the workflow name |
| `on` | Specifies when the workflow runs |
| `jobs` | Defines automated jobs |
| `runs-on` | Selects the runner environment |
| `actions/checkout` | Retrieves repository code |
| `setup-python` | Configures Python |
| `bandit` | Performs Python static security analysis |
| `pip-audit` | Checks Python dependencies for known vulnerabilities |

**Important implementation considerations:**

- Pin third-party GitHub Actions to reviewed versions or immutable commit SHAs in production workflows.
- Add dependency installation from a controlled, reviewed dependency manifest.
- A failed security check should be investigated rather than automatically bypassed.
- Production deployment should have separate permissions and approval controls.
- This is a learning example, not a complete production-ready DevSecOps pipeline.

---

# 5. Create DevSecOps Assessment Reports

## What Is a DevSecOps Assessment Report?

A **DevSecOps Assessment Report** is a document that describes the security condition of a software development and deployment process.

It records what was examined, which weaknesses were identified, how findings were validated, what risks they create, and what improvements are recommended.

Security teams use these reports to communicate findings to developers, management, and other stakeholders.

## What Should a Report Contain?

A professional report normally includes the following sections.

### 1. Executive Summary

A short overview of the assessment.

Include:

- What application or workflow was examined.
- The overall security condition.
- Major findings.
- Important recommendations.

### 2. Scope of Assessment

Describe what was included and excluded.

Example:

```text
Application: E-Commerce Web Application

Assessment Scope:
- Application architecture
- Python source code
- Authentication and authorization
- Third-party dependencies
- GitHub Actions CI/CD workflow
- Deployment configuration

Out of Scope:
- External production infrastructure
- Third-party payment provider internals
```

### 3. Methodology

Explain how the assessment was conducted.

Possible activities:

- Architecture review.
- Secure code review.
- Static analysis.
- Dependency auditing.
- Secret detection.
- CI/CD configuration review.
- Controlled dynamic testing.
- Risk assessment.

### 4. Security Findings

Document each identified issue.

A useful finding format:

| Field | Description |
|---|---|
| Finding ID | Unique reference number |
| Title | Name of the security issue |
| Severity | Critical, High, Medium, or Low |
| Affected Component | Application component involved |
| Description | Explanation of the weakness |
| Evidence | Supporting technical information |
| Impact | Possible consequences |
| Recommendation | Proposed security improvement |
| Status | Open, In Progress, or Resolved |

### 5. Risk Assessment

Prioritize findings based on likelihood, impact, and business context.

Example:

| Finding | Likelihood | Impact | Priority |
|---|---|---|---|
| SQL Injection | High | High | Critical |
| Exposed API Key | Medium | Critical | High |
| Outdated Dependency | Medium | Medium | Medium |
| Missing Security Headers | Low | Medium | Low |

These are illustrative ratings. Real priorities must be based on evidence, exposure, exploitability, affected assets, and business impact.

### 6. Recommendations

Provide practical remediation steps.

Examples:

- Replace unsafe SQL queries with parameterized queries.
- Rotate exposed credentials.
- Update vulnerable dependencies.
- Implement server-side authorization.
- Add automated security testing.
- Protect CI/CD credentials.
- Restrict production deployment permissions.
- Improve security monitoring.

### 7. Conclusion

Summarize the overall assessment and identify the most important next steps.

---

# 6. Complete Practical Assessment Scenario

Now combine all five tasks into one realistic exercise.

## Scenario: Student Management Application

An organization develops a student management application.

The application contains:

- Student registration.
- Login system.
- Student profiles.
- Admin dashboard.
- Database.
- REST API.
- Third-party Python libraries.
- GitHub repository.
- Automated deployment workflow.

You are assigned as an application security analyst.

Your job is to assess the application and prepare a DevSecOps report.

### Task A: Architecture Review

Draw the application architecture.

Identify:

- External entry points.
- Backend services.
- Database connections.
- Authentication mechanisms.
- Trust boundaries.
- Sensitive information.

### Task B: Security Weakness Analysis

Examine the application for:

- SQL injection.
- Broken access control.
- Weak authentication.
- Insecure error handling.
- Missing input validation.
- Exposed secrets.
- Security misconfigurations.

Validate every reported issue.

### Task C: Supply Chain Assessment

Examine:

- Python dependencies.
- Known dependency vulnerabilities.
- Unmaintained packages.
- Exposed credentials.
- Dependency version control.
- Build process security.

Generate an SBOM if the available tooling and project format support it.

### Task D: Secure Workflow Design

Design a CI/CD pipeline containing:

- Code review.
- SAST.
- Dependency scanning.
- Secret detection.
- Security testing.
- Deployment approval.
- Production monitoring.

Explain where each security control belongs.

### Task E: Prepare the Assessment Report

Document:

1. Application overview.
2. Assessment scope.
3. Architecture review.
4. Security weaknesses.
5. Supply chain findings.
6. CI/CD assessment.
7. Risk ratings.
8. Recommendations.
9. Conclusion.

---

# 7. DevSecOps Assessment Report Template

Use the following template when documenting your practical assessment.

```markdown
# DevSecOps Security Assessment Report

## 1. Executive Summary

Application Name:
Assessment Date:
Assessed By:

Overall Security Status:

Summary of Findings:

## 2. Assessment Scope

- Application Architecture:
- Source Code:
- Authentication and Authorization:
- Dependencies:
- CI/CD Pipeline:
- Deployment Configuration:

## 3. Methodology

- Architecture Review:
- Manual Code Review:
- Static Analysis:
- Dependency Scanning:
- Secret Detection:
- Dynamic Security Testing:

## 4. Architecture Review

Application Components:

Identified Trust Boundaries:

Security Controls:

Architecture Weaknesses:

## 5. Security Findings

### Finding 1

Title:
Severity:
Affected Component:

Description:

Evidence:

Potential Impact:

Recommended Fix:

Validation Method:

Status:

## 6. Software Supply Chain Assessment

Dependency Inventory:

Known Vulnerabilities:

Exposed Secrets:

SBOM Availability:

Build Security:

Recommendations:

## 7. CI/CD Security Assessment

Code Review:

SAST:

SCA:

Secret Scanning:

Artifact Security:

Deployment Security:

Monitoring:

## 8. Risk Assessment

| Finding | Likelihood | Impact | Priority |
|---------|------------|--------|----------|
|         |            |        |          |

## 9. Recommendations

1.
2.
3.
4.
5.

## 10. Conclusion

Overall Assessment:

Major Risks:

Recommended Next Steps:
```

---

# 8. Tools to Practice

These tools are useful for performing the assessment tasks.

| Tool | Category | Practical Use |
|---|---|---|
| Git | Version Control | Track source code changes |
| GitHub Actions | CI/CD | Automate development workflows |
| Semgrep | SAST | Detect insecure code patterns |
| Bandit | SAST | Analyze Python code |
| OWASP ZAP | DAST | Test running web applications |
| Burp Suite | Application Testing | Inspect and test HTTP traffic |
| Trivy | Supply Chain Security | Scan dependencies, images, and configurations |
| Gitleaks | Secret Detection | Identify exposed credentials |
| pip-audit | SCA | Identify known Python dependency vulnerabilities |

Only scan applications, systems, repositories, and infrastructure that you own or have explicit authorization to assess.

---

# 9. Final Revision

| Practical Task | What You Should Be Able to Do |
|---|---|
| Architecture Review | Understand application components, data flows, and trust boundaries |
| Security Weakness Analysis | Identify, validate, and prioritize application weaknesses |
| Supply Chain Assessment | Examine dependencies, packages, build systems, and software integrity |
| Secure Workflow Design | Integrate security checks into development and CI/CD |
| DevSecOps Reporting | Document evidence, impact, risks, and recommendations |

## Key Takeaways

- Application architecture reviews help identify design-level security weaknesses.
- Security analysis requires evidence and validation, not just automated tool output.
- Software supply chain security protects dependencies, build processes, and software artifacts.
- DevSecOps integrates security throughout the development lifecycle.
- CI/CD automation helps detect security problems earlier and more consistently.
- Assessment reports turn technical findings into actionable security improvements.

---

# 10. Practical Knowledge Check

<details>
<summary>1. What is the main purpose of an application architecture review?</summary>

To examine application components, communication paths, data flows, trust boundaries, and security controls to identify design-level weaknesses.

</details>

<details>
<summary>2. What is the difference between SAST and DAST?</summary>

SAST analyzes source code or application artifacts without executing the application. DAST tests a running application by interacting with it.

</details>

<details>
<summary>3. What is a Software Bill of Materials?</summary>

An SBOM is an inventory of software components and dependencies included in a software product.

</details>

<details>
<summary>4. Why should dependency vulnerabilities be monitored?</summary>

Because third-party components can introduce known vulnerabilities into otherwise secure applications.

</details>

<details>
<summary>5. What is Shift Left Security?</summary>

It means integrating security activities earlier in the software development lifecycle rather than waiting until the end.

</details>

<details>
<summary>6. What is the purpose of security gates in a CI/CD pipeline?</summary>

They enforce defined security requirements before software progresses to later stages, such as production deployment.

</details>

<details>
<summary>7. Why is evidence important in a DevSecOps assessment report?</summary>

Evidence helps demonstrate that a finding is real, supports risk evaluation, and allows developers to reproduce and fix the issue.

</details>

<details>
<summary>8. What is the difference between a vulnerability finding and a risk assessment?</summary>

A vulnerability finding describes a security weakness. A risk assessment evaluates its likelihood, impact, and priority in a particular environment.

</details>

<details>
<summary>9. Can automated security tools guarantee that an application is secure?</summary>

No. Tools have limitations, including false positives and false negatives. Manual review, testing, and security validation are also necessary.

</details>

<details>
<summary>10. What is the main goal of DevSecOps?</summary>

To integrate security into development, delivery, and operations so that software can be built, tested, deployed, and maintained securely and continuously.

</details>

**Professional Mindset:** A cybersecurity professional should not simply report that a vulnerability exists. They should explain the evidence, understand the impact, recommend a practical fix, and verify that the fix works.
