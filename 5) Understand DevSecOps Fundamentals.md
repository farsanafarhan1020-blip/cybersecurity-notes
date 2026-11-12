# 🛡️ Understanding DevSecOps Fundamentals

## 1. 🚀 What Is DevSecOps?

### Definition

**DevSecOps (Development, Security, and Operations)** is a software development approach that integrates security practices into every stage of the Software Development Life Cycle (SDLC).

It brings three major teams and responsibilities together:

- **Development (Dev):** Builds and maintains application features.
- **Security (Sec):** Identifies risks, prevents vulnerabilities, and protects systems.
- **Operations (Ops):** Deploys, manages, monitors, and maintains applications and infrastructure.

The main idea behind DevSecOps is:

> Security should not be treated as a separate activity performed only before deployment. It should be continuously integrated into the entire development and operations process.

### Why Do We Need DevSecOps?

Consider a traditional software development environment.

A development team builds an application and sends it to the security team only after development is complete.

The security team performs a penetration test and discovers:

- SQL Injection vulnerabilities.
- Weak authentication.
- Hardcoded API keys.
- Insecure dependencies.
- Incorrect server configurations.

Now developers must go back, understand the findings, modify the code, and repeat testing.

This creates several problems:

1. Security issues are discovered late.
2. Fixing vulnerabilities becomes more expensive.
3. Releases may be delayed.
4. Developers and security teams may blame each other.
5. Security testing becomes a bottleneck.

DevSecOps addresses these problems by integrating security throughout development.

### Traditional DevOps vs. DevSecOps

First, understand that DevSecOps extends DevOps by making security an integrated responsibility.

| Traditional DevOps | DevSecOps |
|---|---|
| Focuses on development and operations collaboration | Includes security throughout the process |
| Security may be handled separately | Security is a shared responsibility |
| Security testing may happen late | Security testing starts early |
| Some security activities remain manual | Repetitive security activities are automated |
| Vulnerabilities may be discovered near deployment | Vulnerabilities can be detected throughout development |

### The DevSecOps Lifecycle

A typical DevSecOps lifecycle includes the following stages:

```text
       PLAN
         |
         v
       CODE
         |
         v
       BUILD
         |
         v
       TEST
         |
         v
      RELEASE
         |
         v
      DEPLOY
         |
         v
      OPERATE
         |
         v
      MONITOR
         |
         +----------> Continuous Feedback
```

Security is integrated into every stage.

| Stage | Security Activities |
|---|---|
| Plan | Security requirements, risk assessment, threat modeling |
| Code | Secure coding, code reviews, secret detection |
| Build | SAST, dependency scanning, container image scanning |
| Test | DAST, security testing, penetration testing |
| Release | Security approval, artifact verification, policy checks |
| Deploy | Secure configuration, access control, infrastructure validation |
| Operate | Monitoring, patch management, incident response |
| Monitor | Log analysis, threat detection, vulnerability monitoring |

**Important:** DevSecOps is not a single tool or software product. It is a culture, set of practices, and technical approach to integrating security into software delivery.

---

## 2. ⬅️ Shift-Left Security

### What Is Shift-Left Security?

Shift-left security is the practice of moving security activities earlier in the Software Development Life Cycle.

In traditional development, security testing may happen near the end of the development process.

In shift-left security, developers and security professionals identify and address security problems during requirements gathering, design, and coding.

Imagine the SDLC as a timeline:

```text
Traditional Approach:

PLAN ---- CODE ---- BUILD ---- TEST ---- DEPLOY
                                      |
                               Security Testing


Shift-Left Approach:

PLAN ---- CODE ---- BUILD ---- TEST ---- DEPLOY
  |        |         |         |
  S        S         S         S
  E        E         E         E
  C        C         C         C
  U        U         U         U
  R        R         R         R
  I        I         I         I
  T        T         T         T
  Y        Y         Y         Y
```

Security begins early and continues throughout development.

### Why Is Shift-Left Important?

Consider a developer building a user authentication system.

**Scenario A: Security testing happens late**

1. Developer writes authentication code.
2. The application is completed.
3. Security testing discovers that passwords are stored using plain text.
4. Developers must redesign password storage.
5. Database migration and additional testing are required.

**Scenario B: Shift-left security**

1. Security requirements specify secure password storage.
2. The developer selects an appropriate password-hashing algorithm.
3. Code review checks the implementation.
4. SAST examines the code.
5. Testing validates the authentication system.

The security weakness is prevented or detected much earlier.

### Benefits of Shift-Left Security

| Benefit | Explanation |
|---|---|
| Early Detection | Vulnerabilities are discovered before they reach production |
| Lower Remediation Cost | Earlier fixes generally require less rework |
| Faster Development | Reduces security-related delays near release |
| Better Code Quality | Encourages secure coding practices |
| Reduced Risk | Decreases the chance of vulnerable software reaching users |
| Developer Awareness | Developers learn to recognize security problems |

### Shift-Left Does Not Mean Shift-Only

This is an important distinction.

Shift-left security does not mean performing security testing only during development.

Security must continue after deployment.

For example:

- Before deployment: SAST, code review, dependency scanning.
- During deployment: Configuration and infrastructure checks.
- After deployment: Monitoring, vulnerability management, and incident response.

The goal is **early security plus continuous security**.

---

## 3. 🤖 Automation in Security

### What Is Security Automation?

Security automation is the use of software, scripts, tools, and predefined workflows to perform security-related tasks with minimal manual intervention.

Instead of requiring security professionals to manually check every application, automated systems can perform repetitive security checks whenever code changes or software is deployed.

### Why Is Automation Important?

Modern applications may contain:

- Thousands of source-code files.
- Hundreds of third-party dependencies.
- Multiple APIs.
- Container images.
- Cloud infrastructure.
- Frequent code changes.

Manually reviewing everything after every update is difficult and time-consuming.

Automation helps security teams perform repeatable checks quickly and consistently.

### Common Security Automation Activities

| Activity | What It Does |
|---|---|
| SAST | Automatically analyzes source code |
| SCA | Scans third-party dependencies |
| Secret Scanning | Detects accidentally committed credentials |
| DAST | Tests running applications |
| Container Scanning | Identifies vulnerable container packages and configurations |
| Infrastructure Scanning | Detects insecure infrastructure configurations |
| Compliance Checks | Checks whether defined security policies are followed |
| Log Monitoring | Detects suspicious events and patterns |

### Example 1: Automated Source Code Scanning

Imagine a developer pushes new code to a GitHub repository.

A security automation workflow can automatically run Semgrep against the updated code.

```text
Developer writes code
        |
        v
Pushes code to GitHub
        |
        v
GitHub Actions starts
        |
        v
Semgrep scans source code
        |
        v
Security findings generated
        |
        v
Developer reviews and fixes issues
```

The developer does not need to remember to run the scanner manually every time.

### Example 2: Automated Dependency Scanning

Applications use external libraries to avoid developing everything from scratch.

For example, a Python application may depend on:

```text
requests
django
flask
numpy
```

An outdated package may contain a known vulnerability.

An automated dependency scanner can identify vulnerable package versions and report them.

Tools include:

- Dependabot.
- Snyk.
- OWASP Dependency-Check.
- pip-audit.

Example using `pip-audit`:

Install:

```bash
pip install pip-audit
```

Scan the current Python environment:

```bash
pip-audit
```

The tool checks installed Python packages against known vulnerability information.

### Example 3: Automated Secret Detection

Developers sometimes accidentally commit sensitive information into Git repositories.

Example:

```python
API_KEY = "123456789abcdef"
DATABASE_PASSWORD = "admin123"
```

These are examples of insecure hardcoded secrets.

Secret-scanning tools can detect patterns that resemble:

- API keys.
- Access tokens.
- Private keys.
- Database credentials.
- Cloud credentials.

Common tools include:

- Gitleaks.
- TruffleHog.
- GitHub Secret Scanning.

**Important:** If a secret has been committed, simply deleting it from the latest version of the code is not sufficient. It may remain in Git history or other copies. The exposed credential should be revoked or rotated, and the exposure should be investigated.

### Benefits of Security Automation

1. Reduces repetitive manual work.
2. Speeds up vulnerability detection.
3. Improves consistency.
4. Supports frequent software releases.
5. Helps enforce security policies.
6. Provides repeatable security evidence.

### Limitations of Security Automation

Automation does not replace human security professionals.

Automated tools may:

- Produce false positives.
- Miss complex business logic vulnerabilities.
- Fail to understand the application's actual environment.
- Require correct configuration.
- Generate large numbers of findings.

Security professionals must investigate important findings, prioritize risks, and decide appropriate remediation.

---

## 4. 🔗 Security Integration

### What Is Security Integration?

Security integration means connecting security tools, controls, and processes directly into the software development and deployment workflow.

Instead of treating security testing as an independent activity, security checks become part of the tools and processes developers already use.

For example, security can be integrated into:

- Git repositories.
- Code editors.
- Build systems.
- CI/CD pipelines.
- Container registries.
- Cloud environments.
- Monitoring platforms.

### What Is CI/CD?

To understand security integration, you need to understand CI/CD.

**CI — Continuous Integration**

Continuous Integration is the practice of frequently integrating code changes into a shared repository and automatically building and testing those changes.

Typical CI activities:

1. Developer pushes code.
2. Automated build starts.
3. Unit tests run.
4. Code quality checks execute.
5. Security scans run.
6. Results are reported.

**CD — Continuous Delivery / Continuous Deployment**

Continuous Delivery ensures that software is kept in a deployable state, with production releases generally requiring an approval or manual trigger.

Continuous Deployment automatically releases changes that pass the required checks into production.

The exact meaning of CD depends on the workflow being used.

### How Is Security Integrated Into CI/CD?

Consider a development team building a web application using GitHub.

A simplified secure CI/CD pipeline might look like this:

```text
Developer
    |
    v
GitHub Repository
    |
    v
Continuous Integration
    |
    +-----------------------+
    |                       |
    v                       v
   Build                 Unit Tests
    |                       |
    +-----------+-----------+
                |
                v
        Security Scanning
                |
       +--------+--------+
       |        |        |
       v        v        v
      SAST     SCA    Secret Scan
       |        |        |
       +--------+--------+
                |
                v
        Security Evaluation
                |
         +------+------+
         |             |
         v             v
       Pass           Fail
         |             |
         v             v
    Continue       Block or
    Pipeline       Investigate
         |
         v
    Deployment
         |
         v
  Runtime Monitoring
```

This is an illustrative pipeline. Real implementations may include additional checks, approval stages, and exception handling.

### Common Security Integration Points

| Integration Point | Security Activity |
|---|---|
| IDE | Detect insecure code while developers write it |
| Git Repository | Secret scanning and commit checks |
| Pull Requests | Automated code and dependency analysis |
| CI Pipeline | SAST, SCA, and security policy checks |
| Container Registry | Image vulnerability scanning |
| Deployment Pipeline | Infrastructure and configuration validation |
| Production | Runtime monitoring and threat detection |

### What Is a Security Gate?

A security gate is a rule or checkpoint in a development pipeline that determines whether a workflow can continue based on security requirements.

Example:

A company defines this policy:

- Critical vulnerabilities must block production deployment.
- High-severity vulnerabilities require investigation.
- Low-severity findings may be tracked for later remediation.

If an automated scan identifies a critical vulnerability, the pipeline may stop the deployment.

```text
Security Scan
     |
     v
Evaluate Findings
     |
     v
Critical Vulnerability?
     |
   Yes
     |
     v
Block Deployment
     |
     v
Developer Fixes Issue
     |
     v
Run Security Scan Again
```

A good security gate should use sensible policies.

Blocking every minor finding can create unnecessary delays, while ignoring serious findings can expose the organization to unacceptable risk.

### Common Tools Used for Security Integration

| Category | Tools |
|---|---|
| Source Code Management | GitHub, GitLab, Bitbucket |
| CI/CD | GitHub Actions, Jenkins, GitLab CI/CD |
| SAST | Semgrep, SonarQube, CodeQL |
| SCA | Snyk, Dependabot, OWASP Dependency-Check |
| Secret Scanning | Gitleaks, TruffleHog |
| Container Security | Trivy |
| DAST | OWASP ZAP, Burp Suite |
| Infrastructure Security | Checkov, tfsec |
| Monitoring | Splunk, Wazuh, Elastic Security |

**Remember:** These tools serve different purposes. A secure CI/CD pipeline combines appropriate tools instead of relying on one scanner to detect every type of vulnerability.

---

## 5. 🔄 Continuous Security

### What Is Continuous Security?

Continuous security is the practice of continuously monitoring, assessing, testing, and improving an application's security throughout its lifecycle.

Security is not a one-time activity.

An application that is secure today may become vulnerable tomorrow because of:

- Newly discovered vulnerabilities.
- Outdated dependencies.
- Configuration changes.
- Newly introduced features.
- Changes in user permissions.
- New attack techniques.
- Compromised credentials.

Continuous security helps organizations identify and respond to these changes.

### Why Is Continuous Security Necessary?

Imagine a company deploys a secure web application.

At deployment:

- No known critical vulnerabilities exist.
- Authentication works correctly.
- Dependencies are updated.
- Security configurations are reviewed.

Three months later:

1. A vulnerability is discovered in a third-party library.
2. The application still uses the vulnerable version.
3. Attackers begin exploiting the vulnerability.
4. The company has not updated its dependency inventory.

The application was tested before deployment, but its security posture changed over time.

Continuous security addresses this problem through ongoing assessment and monitoring.

### Main Components of Continuous Security

#### 1. Continuous Vulnerability Scanning

Regularly scan applications, systems, containers, and dependencies for known vulnerabilities.

Example tools:

- Trivy.
- Nessus.
- OpenVAS.
- OWASP Dependency-Check.

#### 2. Continuous Security Monitoring

Monitor application and infrastructure activity to identify suspicious behavior.

Examples:

- Repeated failed login attempts.
- Unusual administrator activity.
- Unexpected network connections.
- Suspicious API requests.
- Unauthorized access attempts.

Monitoring platforms include:

- Wazuh.
- Splunk.
- Elastic Security.
- Microsoft Sentinel.

#### 3. Continuous Compliance Monitoring

Check whether systems continue to follow required security policies and standards.

Examples:

- Password policies.
- Access control requirements.
- Encryption requirements.
- Logging requirements.
- Cloud configuration policies.

#### 4. Continuous Patch Management

Regularly identify, prioritize, test, and install security updates.

For example:

```text
Vulnerability Discovered
        |
        v
Identify Affected Systems
        |
        v
Assess Risk and Priority
        |
        v
Obtain Security Update
        |
        v
Test the Update
        |
        v
Deploy the Patch
        |
        v
Verify the Fix
```

#### 5. Continuous Incident Response

Security teams must be prepared to detect, investigate, contain, and recover from security incidents.

A simplified incident response process:

1. Preparation.
2. Detection and analysis.
3. Containment.
4. Eradication.
5. Recovery.
6. Lessons learned.

### Continuous Security vs. Traditional Security

| Traditional Approach | Continuous Security |
|---|---|
| Security testing at specific stages | Security assessment throughout the lifecycle |
| Periodic manual checks | Frequent automated checks and ongoing monitoring |
| Vulnerabilities may remain unnoticed between tests | New vulnerabilities can be detected more quickly |
| Security may be handled as a separate activity | Security is integrated into development and operations |
| Limited visibility between assessments | Ongoing security visibility |

### Continuous Feedback Loop

Continuous security creates a feedback loop between development, security, and operations.

```text
      Develop
         |
         v
       Build
         |
         v
        Test
         |
         v
      Deploy
         |
         v
      Monitor
         |
         v
   Detect Security Issue
         |
         v
   Investigate and Fix
         |
         v
    Test the Fix
         |
         +-----------> Develop
```

This loop continues throughout the application's lifetime.

---

# 🏢 Real-World Example: DevSecOps in an E-Commerce Company

Imagine an organization developing an online shopping platform.

The application includes:

- User registration.
- Login and authentication.
- Product search.
- Shopping cart.
- Payment processing.
- Administrator dashboard.
- Cloud infrastructure.

The organization wants to release new features frequently without compromising security.

### Step 1: Planning

The team identifies security requirements:

- Passwords must be securely hashed.
- Sensitive information must be protected.
- APIs must enforce authorization.
- Payment information must be handled securely.
- Administrative access must be restricted.

Security participates in architecture and threat modeling.

### Step 2: Development

Developers write code using secure coding practices.

Security measures include:

- Input validation.
- Parameterized database queries.
- Output encoding.
- Secure session management.
- Server-side authorization.

### Step 3: Automated Code Checks

When developers push code to GitHub:

- Semgrep checks for insecure code patterns.
- Gitleaks searches for exposed secrets.
- Dependency scanners identify vulnerable packages.

### Step 4: Security Testing

The application is deployed in a testing environment.

The security team performs:

- DAST using OWASP ZAP.
- Manual security reviews.
- Authentication testing.
- Authorization testing.
- Penetration testing.

### Step 5: Secure Deployment

Before production deployment:

- Required security checks must pass.
- Container images are scanned.
- Deployment configurations are reviewed.
- Access permissions are verified.

### Step 6: Continuous Monitoring

After deployment:

- Application logs are monitored.
- Suspicious authentication activity is investigated.
- Vulnerable dependencies are tracked.
- Security patches are applied.
- Incidents are handled by the security team.

**Result:** Security becomes part of the company's everyday software development and operations process rather than a final checkpoint.

---

# 🧪 Practical Lab: Introduction to DevSecOps

The best way to understand DevSecOps is to build a small security workflow.

## Lab 1: Create a Basic Security Scanning Workflow

**Objective:** Understand how security checks can be automated whenever code is pushed to GitHub.

You need:

- A GitHub account.
- A GitHub repository.
- A small Python project.

### Step 1: Create a Python Project

Create a file named:

```text
app.py
```

Add:

```python
def greet(username):
    print(f"Welcome, {username}!")

name = input("Enter your name: ")

greet(name)
```

### Step 2: Create a Requirements File

Create:

```text
requirements.txt
```

For this simple application, it can remain empty because no external packages are required.

### Step 3: Create a GitHub Actions Workflow

Inside your repository, create:

```text
.github/workflows/security.yml
```

Add:

```yaml
name: Python Security Checks

on:
  push:
  pull_request:

jobs:
  security:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
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
        run: pip-audit -r requirements.txt
```

### Step 4: Push Your Code

Commit and push your project to GitHub.

GitHub Actions should automatically start the workflow.

### Step 5: Examine the Results

Open your GitHub repository and navigate to:

**Actions → Python Security Checks**

Observe:

- Did the workflow start automatically?
- Did Bandit complete?
- Did the dependency audit complete?
- Did any security issues appear?
- What happens when a security check fails?

**Important implementation notes:**

- GitHub Actions workflows execute on repository events such as pushes and pull requests.
- The example uses versioned GitHub Actions for illustration; production workflows should use appropriately maintained versions and preferably pin third-party actions to verified commit SHAs.
- `pip-audit` checks the dependencies listed in the supplied requirements file. An empty file means there are no third-party dependencies to audit.
- A real project should also use dependency lock files and scan the actual dependency set.
- Bandit and `pip-audit` provide limited coverage; passing these checks does not prove that an application is secure.

## Lab 2: Dependency Vulnerability Scanning

**Objective:** Understand how vulnerable third-party packages can be detected.

Install:

```bash
pip install pip-audit
```

Run:

```bash
pip-audit
```

Observe the results.

Understand:

1. Which packages are installed?
2. Are any known vulnerabilities identified?
3. Which package versions are affected?
4. Is an updated version available?
5. What remediation is recommended?

Do not intentionally install an outdated vulnerable package into an important working environment. Use an isolated virtual environment if experimenting with vulnerable dependencies.

## Lab 3: Secret Scanning with Gitleaks

**Objective:** Understand how organizations detect accidentally committed secrets.

Install Gitleaks using an appropriate installation method for your operating system.

Scan a Git repository:

```bash
gitleaks git .
```

Observe:

- What types of secrets can be detected?
- How does the tool identify suspicious patterns?
- Why is committing API keys dangerous?
- Why should exposed credentials be rotated?

Use synthetic test credentials only.

## Lab 4: Understand a DevSecOps Pipeline

Draw a pipeline containing:

1. GitHub repository.
2. GitHub Actions.
3. SAST scanning.
4. Dependency scanning.
5. Secret scanning.
6. Security evaluation.
7. Deployment.
8. Runtime monitoring.

For each stage, identify which security activity happens and why it is necessary.

---

# 📊 DevSecOps Tools — Quick Reference

| Security Area | Example Tools | Purpose |
|---|---|---|
| Source Code Management | GitHub, GitLab | Manage source code |
| CI/CD Automation | GitHub Actions, Jenkins | Automate builds and deployment |
| SAST | Semgrep, Bandit, CodeQL | Analyze source code |
| SCA | Snyk, Dependabot, pip-audit | Identify vulnerable dependencies |
| Secret Scanning | Gitleaks, TruffleHog | Detect exposed credentials |
| DAST | OWASP ZAP, Burp Suite | Test running applications |
| Container Security | Trivy | Scan container images |
| Infrastructure Security | Checkov | Identify insecure infrastructure configurations |
| Monitoring | Wazuh, Splunk | Monitor security events |
| Vulnerability Management | Nessus, OpenVAS | Discover system vulnerabilities |

---

# 🧠 Quick Revision

| Concept | Remember This |
|---|---|
| DevSecOps | Integrates security into development and operations |
| DevOps | Combines development and operations practices |
| Shift-Left Security | Moves security activities earlier in the SDLC |
| Security Automation | Uses tools and scripts to automate security tasks |
| Security Integration | Connects security checks to development and deployment workflows |
| CI | Continuous Integration |
| CD | Continuous Delivery or Continuous Deployment |
| Security Gate | A security checkpoint that controls pipeline progression |
| Continuous Security | Ongoing security assessment, monitoring, and improvement |
| SAST | Analyzes source code without executing the application |
| SCA | Identifies risks in third-party dependencies |
| DAST | Tests a running application |
| Secret Scanning | Detects accidentally exposed credentials |

## 🔑 Key Takeaways

- DevSecOps makes security a shared responsibility between development, security, and operations.
- Security should be integrated into every stage of the SDLC.
- Shift-left security helps discover vulnerabilities earlier, reducing rework and remediation costs.
- Security automation makes repetitive security checks faster and more consistent.
- CI/CD pipelines can automatically execute security scans whenever code changes.
- Security gates prevent software from progressing when it violates defined security policies.
- Continuous security ensures that security does not stop after deployment.
- Automated tools support security professionals but cannot replace human investigation and judgment.
- DevSecOps is a culture and engineering practice, not simply a collection of tools.

---

# 🎯 Active Recall — Test Your Understanding

Try answering these questions without looking at the explanations.

<details>
<summary><strong>1. What is DevSecOps?</strong></summary>

DevSecOps is an approach that integrates security into the entire software development and operations lifecycle. It makes security a shared responsibility between developers, security professionals, and operations teams.
</details>

<details>
<summary><strong>2. What is the main difference between DevOps and DevSecOps?</strong></summary>

DevOps emphasizes collaboration between development and operations. DevSecOps extends this approach by integrating security throughout the software delivery lifecycle.
</details>

<details>
<summary><strong>3. What is shift-left security?</strong></summary>

Shift-left security means moving security activities earlier in the SDLC so vulnerabilities can be identified and fixed during requirements, design, and development rather than waiting until the end.
</details>

<details>
<summary><strong>4. Why is fixing vulnerabilities earlier generally cheaper?</strong></summary>

Early fixes usually require less rework because the vulnerable design or code has not yet spread into other components, environments, or production systems.
</details>

<details>
<summary><strong>5. What is security automation?</strong></summary>

Security automation uses software, scripts, and predefined workflows to perform security tasks such as code scanning, dependency analysis, secret detection, and vulnerability monitoring.
</details>

<details>
<summary><strong>6. What is CI/CD?</strong></summary>

Continuous Integration automatically builds and tests code changes frequently. Continuous Delivery keeps software ready for release, while Continuous Deployment automatically releases qualifying changes into production.
</details>

<details>
<summary><strong>7. What is a security gate in a CI/CD pipeline?</strong></summary>

A security gate is a checkpoint that evaluates security results against predefined policies. It can block a build or deployment when important security requirements are not met.
</details>

<details>
<summary><strong>8. What is continuous security?</strong></summary>

Continuous security is the ongoing process of assessing, testing, monitoring, and improving an application's security throughout its lifecycle, including after deployment.
</details>

<details>
<summary><strong>9. Does DevSecOps eliminate the need for penetration testing?</strong></summary>

No. Automated security checks cannot identify every vulnerability, especially complex business logic and chained attack paths. Manual security testing and penetration testing remain important.
</details>

<details>
<summary><strong>10. What happens when a security scan finds a critical vulnerability in a CI/CD pipeline?</strong></summary>

Depending on the organization's security policy, the pipeline may block the build or deployment. The issue should be investigated, fixed, and retested before the application progresses.
</details>

---

## 💡 Professional Mindset

A DevSecOps engineer does not simply run security tools.

A professional DevSecOps engineer understands how software is built, how code moves through CI/CD pipelines, how security controls can be automated, how vulnerabilities should be prioritized, and how security can be maintained after deployment.

**The goal is not to slow down development with security. The goal is to make secure development a natural part of delivering software.**
