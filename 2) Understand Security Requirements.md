# 🔐 Understanding Security Requirements

> **Security requirements** are the security-related conditions, controls, and capabilities that a software application or system must satisfy to protect its data, users, infrastructure, and operations from security threats.

They define what an application must protect, which security controls it needs, and how securely it should operate.

Security requirements should be identified before development begins and maintained throughout the Secure Software Development Life Cycle (Secure SDLC).

For example, when developing an online banking application, security requirements may include:

- Users must authenticate before accessing their accounts.
- Sensitive information must be encrypted.
- Users must only access their own financial records.
- Administrative activities must be logged.
- The application must protect against common web vulnerabilities.

Without clearly defined security requirements, developers may build an application that works correctly but exposes sensitive information or allows unauthorized access.

---

## 1. 📋 Security Planning

Security planning is the process of identifying security needs, defining security objectives, selecting appropriate controls, and assigning responsibilities before software development begins.

It ensures that security is treated as a fundamental part of the project rather than an activity added at the end.

### Why Is Security Planning Important?

Without proper security planning:

- Important security requirements may be overlooked.
- Developers may implement weak authentication.
- Sensitive data may not be adequately protected.
- Security testing may happen too late.
- Vulnerabilities may become expensive to fix.
- Development teams may not understand their security responsibilities.

### Key Activities in Security Planning

**1. Identify Assets**

Determine what needs protection.

Examples:
- User credentials.
- Personal information.
- Payment information.
- Application source code.
- Databases.
- Authentication tokens.
- Business-critical services.

**2. Identify Security Objectives**

Define what the application must achieve from a security perspective.

The three fundamental objectives are the CIA Triad:

| Objective | Meaning | Example |
|---|---|---|
| Confidentiality | Prevent unauthorized information disclosure | Encrypt customer records |
| Integrity | Prevent unauthorized data modification | Protect financial transactions |
| Availability | Ensure systems remain accessible | Protect against DoS attacks |

**3. Define Security Requirements**

Translate security objectives into specific requirements.

Example:

| Requirement | Security Purpose |
|---|---|
| Passwords must be securely hashed | Protect stored credentials |
| MFA must be supported | Strengthen authentication |
| HTTPS must be used | Protect data in transit |
| Access must follow RBAC | Prevent unauthorized access |
| Security events must be logged | Support detection and investigation |
| Backups must be maintained | Support recovery |

**4. Assign Security Responsibilities**

Identify who is responsible for implementing, testing, approving, and maintaining security controls.

**5. Plan Security Testing**

Decide how security requirements will be verified.

Examples:
- Code reviews.
- Vulnerability scanning.
- Authentication testing.
- Access control testing.
- Penetration testing.

### Example: Security Planning for a Login System

Suppose developers are building a login system.

A security plan might specify:

1. Users must authenticate before accessing protected pages.
2. Passwords must be stored using a dedicated password-hashing algorithm such as Argon2id or bcrypt.
3. Login attempts must be monitored.
4. MFA must be supported.
5. Sessions must use secure cookies.
6. Authentication failures must not reveal whether a username exists.
7. Password reset tokens must be unpredictable and expire.

These requirements give developers and security testers clear security goals.

---

## 2. ⚠️ Threat Awareness

Threat awareness is the understanding of potential threats, attackers, attack techniques, and vulnerabilities that could affect an application or organization.

Before designing security controls, development teams must understand what they are defending against.

### Threat vs Vulnerability vs Risk

These three terms are closely related but have different meanings.

| Term | Definition | Example |
|---|---|---|
| Threat | Something capable of causing harm | A cybercriminal attempting to steal customer data |
| Vulnerability | A weakness that can be exploited | An application does not validate user input |
| Risk | The possibility and impact of a threat exploiting a vulnerability | An attacker exploits SQL injection to access customer records |

**Example:**

Imagine an application stores customer information in a database.

- **Threat:** An attacker wants to steal customer information.
- **Vulnerability:** The application contains SQL injection.
- **Attack:** The attacker manipulates a database query.
- **Impact:** Unauthorized access to customer records.
- **Risk:** The organization may experience data exposure, financial loss, and reputational damage.

### Common Software Security Threats

| Threat | Description | Example |
|---|---|---|
| SQL Injection | Manipulating database queries through untrusted input | Extracting database records |
| Cross-Site Scripting (XSS) | Injecting malicious scripts into web content | Stealing session information |
| Broken Access Control | Accessing resources without proper authorization | Viewing another user's account |
| Authentication Attacks | Exploiting weak authentication mechanisms | Password spraying |
| CSRF | Tricking a browser into submitting an unwanted authenticated request | Unauthorized account changes |
| DoS/DDoS | Disrupting service availability | Overloading a web server |
| Credential Theft | Obtaining usernames, passwords, or tokens | Phishing |
| Insecure Dependencies | Exploiting vulnerable third-party components | Attacking an outdated library |
| Security Misconfiguration | Exploiting insecure system settings | Exposed administrative interface |

### Understanding the OWASP Top 10

The **OWASP Top 10** is a widely recognized awareness document describing major categories of web application security risks.

It helps developers understand common application security problems.

Some important categories in the 2025 edition include:

- Broken Access Control.
- Security Misconfiguration.
- Software Supply Chain Failures.
- Cryptographic Failures.
- Injection.
- Insecure Design.
- Authentication Failures.
- Software or Data Integrity Failures.
- Security Logging and Alerting Failures.
- Mishandling of Exceptional Conditions.

The OWASP Top 10 is an awareness resource, not a complete security testing standard.

Official reference: [OWASP Top 10](https://owasp.org/www-project-top-ten/?utm_source=chatgpt.com)

### Threat Awareness in Development

Developers should consider questions such as:

- What happens if an attacker submits unexpected input?
- Can one user access another user's information?
- What happens if an authentication token is stolen?
- Can an attacker modify important application data?
- Are third-party libraries secure?
- Can sensitive information leak through error messages?
- What happens if an important service becomes unavailable?

Understanding these threats helps teams design appropriate security requirements.

---

## 3. ⚖️ Compliance Awareness

Compliance awareness means understanding the laws, regulations, industry standards, and organizational policies that apply to software development and information security.

Organizations may be legally or contractually required to protect certain types of information.

Developers and security teams must understand these requirements when designing and building applications.

### Why Is Compliance Important?

Compliance helps organizations:

- Protect personal and sensitive information.
- Establish security responsibilities.
- Maintain customer trust.
- Meet legal and contractual obligations.
- Reduce regulatory and financial risks.
- Improve security governance.

### Important Security Regulations and Standards

| Regulation / Standard | Main Focus |
|---|---|
| GDPR | Protection and processing of personal data in the European Union |
| India's DPDP Act, 2023 | Protection of digital personal data in India |
| HIPAA | Protection of certain health information in the United States |
| PCI DSS | Security of payment card account data |
| ISO/IEC 27001 | Information Security Management Systems (ISMS) |
| SOC 2 | Independent examination of controls relevant to trust services |

**Important distinction:**

- Regulations and laws establish legal obligations.
- Standards provide requirements or frameworks for managing security.
- Certifications and independent assessments can provide evidence that an organization meets particular requirements.

Not every regulation applies to every organization. Applicability depends on the organization's location, activities, data, and contractual obligations.

### Example: Compliance in an E-Commerce Application

Imagine an online shopping platform that collects:

- Customer names.
- Email addresses.
- Phone numbers.
- Delivery addresses.
- Payment-related information.

The development team must consider:

**Data Protection**
- Collect only necessary personal information.
- Protect personal data from unauthorized access.
- Establish appropriate data retention practices.

**Access Control**
- Restrict customer information to authorized users.
- Limit employee access based on job responsibilities.

**Data Protection in Transit**
- Use HTTPS to protect communication between users and servers.

**Logging and Monitoring**
- Record relevant security events.
- Protect logs from unauthorized modification.

**Payment Security**
- Follow applicable PCI DSS requirements when handling payment card data.

### Compliance vs Security

Compliance and security are related, but they are not identical.

| Compliance | Security |
|---|---|
| Focuses on meeting applicable requirements | Focuses on protecting systems and information |
| Often involves laws, standards, and policies | Involves technical and organizational controls |
| Can be evaluated through audits and assessments | Requires continuous protection and improvement |
| Meeting requirements does not guarantee complete security | Security must adapt to changing threats |

**Key idea:** An organization can be compliant with a particular standard and still experience a security incident.

---

## 4. 🏗️ Secure Design Principles

Secure design principles are fundamental rules used when designing systems and applications to reduce security weaknesses.

They help developers create architectures that remain secure even when individual components fail or are attacked.

### 1. Principle of Least Privilege

Give users, applications, and services only the permissions necessary to perform their tasks.

**Example:**

A website that only needs to read product information should not use a database account with full administrative privileges.

**Security benefit:** Limits the damage caused by compromised accounts or applications.

### 2. Defense in Depth

Use multiple independent security controls to protect important resources.

**Example:**

A web application may use:

1. Firewall.
2. Web Application Firewall.
3. Authentication.
4. Authorization.
5. Input validation.
6. Database access controls.
7. Security monitoring.

If one security layer fails, other layers may still prevent or detect an attack.

### 3. Fail Securely

When a system encounters an error or unexpected condition, it should fail in a secure state rather than accidentally granting access.

**Example:**

If an authorization service becomes unavailable, a protected application should deny access rather than allow the user to proceed without permission checks.

This is also known as **fail closed** in access control.

### 4. Secure by Default

The initial configuration of a system should be secure.

**Example:**

- Newly created accounts should not receive administrator permissions.
- Unnecessary services should be disabled.
- Default credentials should not remain active.
- Sensitive resources should not be publicly accessible by default.

### 5. Complete Mediation

Every access to a protected resource should be checked for authorization.

Do not assume that a user who was authorized earlier is automatically authorized for every later request.

**Example:**

A banking application must verify that the currently authenticated customer is authorized to access a particular account every time a sensitive account operation is requested.

### 6. Separation of Duties

Divide sensitive responsibilities among different people or roles to reduce the possibility of abuse or mistakes.

**Example:**

One employee requests a financial transaction, while another authorized employee approves it.

### 7. Minimize Attack Surface

Reduce the number of components, services, interfaces, and permissions that attackers could target.

**Example:**

- Remove unused application features.
- Disable unnecessary network services.
- Restrict administrative interfaces.
- Remove unused dependencies.
- Expose only required API endpoints.

### 8. Keep Security Mechanisms Simple

Complex security designs are harder to implement, understand, test, and maintain.

Prefer clear, well-tested security mechanisms over unnecessary custom implementations.

**Example:** Use established authentication libraries rather than developing a custom password-hashing algorithm.

### Summary of Secure Design Principles

| Principle | Main Purpose |
|---|---|
| Least Privilege | Limit unnecessary permissions |
| Defense in Depth | Provide multiple security layers |
| Fail Securely | Maintain security during failures |
| Secure by Default | Start with secure configurations |
| Complete Mediation | Check authorization for every protected access |
| Separation of Duties | Prevent excessive control by one person |
| Minimize Attack Surface | Reduce possible entry points |
| Simplicity | Reduce design and implementation mistakes |

---

## 5. 🔍 Risk Identification

Risk identification is the process of discovering potential security risks that could affect an application, its users, its data, or its infrastructure.

It is an important part of security planning and threat modeling.

The purpose is to identify problems before they become security incidents.

### What Should Be Considered During Risk Identification?

Security teams should examine:

- Application architecture.
- Sensitive data and storage locations.
- Authentication mechanisms.
- Authorization and access controls.
- Network communication.
- Third-party libraries.
- Server configurations.
- User input and data processing.
- Logging and monitoring.
- External integrations and APIs.

### Step 1: Identify Assets

An asset is anything valuable that needs protection.

Example:

| Asset | Importance |
|---|---|
| User database | Contains customer information |
| Password hashes | Protect user authentication credentials |
| Payment system | Processes financial transactions |
| Application server | Hosts application services |
| API credentials | Allow communication with external services |
| Source code | Contains application logic |

### Step 2: Identify Threats

Determine what could cause harm to those assets.

Examples:

- Unauthorized access.
- Credential theft.
- Data manipulation.
- Malware infection.
- Service disruption.
- Data leakage.

### Step 3: Identify Vulnerabilities

Find weaknesses that could allow threats to succeed.

Examples:

- Weak passwords.
- Missing MFA.
- SQL injection.
- Excessive permissions.
- Unpatched software.
- Publicly exposed storage.
- Insecure API authorization.

### Step 4: Analyze Likelihood and Impact

Two important factors in risk assessment are:

**Likelihood:** How likely is the threat to exploit the vulnerability?

**Impact:** How serious would the consequences be if exploitation occurred?

A simple risk estimation model is:

\[
\text{Risk Score} = \text{Likelihood} \times \text{Impact}
\]

For example, using a scale of 1–5:

| Finding | Likelihood | Impact | Risk Score |
|---|---:|---:|---:|
| SQL Injection in a public login endpoint | 5 | 5 | 25 |
| Missing MFA for privileged accounts | 4 | 5 | 20 |
| Unnecessary open service | 3 | 3 | 9 |
| Missing security banner | 1 | 1 | 1 |

These numbers are illustrative. Organizations should define their own scoring criteria and risk thresholds.

### Step 5: Prioritize Risks

Example risk classification:

| Score | Risk Level | Suggested Action |
|---|---|---|
| 20–25 | Critical | Immediate attention |
| 15–19 | High | Urgent remediation |
| 6–14 | Medium | Planned remediation |
| 1–5 | Low | Monitor or address as appropriate |

A risk score is a prioritization aid, not a guarantee of actual exploitability.

### Step 6: Define Mitigation Strategies

Once risks are identified, choose appropriate responses.

There are four common risk treatment strategies:

| Strategy | Meaning | Example |
|---|---|---|
| Avoid | Eliminate the activity creating the risk | Remove an unnecessary public-facing feature |
| Mitigate | Reduce likelihood or impact | Add MFA and access controls |
| Transfer | Shift certain financial consequences to another party | Cyber insurance |
| Accept | Formally acknowledge and retain the risk | Accept a low-impact risk with documented approval |

**Important:** Transferring financial consequences does not eliminate the underlying technical risk.

### Example: Risk Identification for a Login System

Imagine a developer creates a login system with the following weaknesses:

| Vulnerability | Threat | Potential Impact | Mitigation |
|---|---|---|---|
| Weak password policy | Credential attacks | Account compromise | Strengthen password security |
| No MFA | Stolen credentials | Unauthorized access | Implement MFA |
| SQL injection | Malicious input | Database compromise | Use parameterized queries |
| Weak session management | Session theft | Account takeover | Secure session handling |
| Missing login monitoring | Undetected attacks | Delayed response | Implement authentication logging |

This process helps developers understand what needs to be secured before the application is released.

---

## 6. 🌐 Real-World Example: Planning Security for an Online Banking Application

Imagine a company is developing an online banking application.

The application handles:

- Customer identities.
- Account balances.
- Transaction records.
- Login credentials.
- Financial transactions.

Before development begins, the security team performs the following activities.

| Area | Security Requirement |
|---|---|
| Authentication | Strong authentication and MFA |
| Authorization | Customers can only access authorized accounts |
| Data Protection | Encrypt sensitive information |
| Session Security | Secure session creation and expiration |
| Input Validation | Validate and safely process user input |
| Logging | Record important authentication and financial events |
| Availability | Implement resilience and recovery mechanisms |
| Compliance | Identify applicable financial and data protection obligations |

### Example Threat Analysis

**Threat:** An attacker attempts to transfer money from another customer's account.

**Possible vulnerability:** The application does not properly verify account ownership when processing a transaction.

**Potential impact:** Unauthorized financial transactions.

**Security requirements:**

1. Verify the authenticated user's identity.
2. Verify authorization for the selected account.
3. Validate transaction details on the server.
4. Require appropriate additional verification for high-risk operations.
5. Record transaction events.
6. Monitor suspicious transaction patterns.

This demonstrates how security requirements are derived from real threats and business risks.

---

## 7. 🧪 Practical Application

### Task 1: Identify Security Requirements

Imagine you are developing a web application with:

- User registration.
- Login and logout.
- User profiles.
- File uploads.
- Administrator dashboard.

Write at least five security requirements for this application.

Consider authentication, authorization, data protection, and monitoring.

### Task 2: Threat Identification

Identify at least five possible threats to the application.

Use the following format:

| Asset | Threat | Possible Vulnerability |
|---|---|---|
| User accounts | Account takeover | Weak authentication |
| User profiles | Unauthorized access | Broken access control |
| Uploaded files | Malicious file upload | Insufficient file validation |
| Database | Data theft | SQL injection |
| Admin dashboard | Privilege escalation | Improper authorization |

### Task 3: Apply STRIDE

Choose one application feature, such as the login system.

Identify possible threats under the six STRIDE categories.

### Task 4: Perform Risk Assessment

Choose three vulnerabilities and assign:

- Likelihood (1–5).
- Impact (1–5).
- Risk score.
- Recommended mitigation.

### Task 5: Apply Secure Design Principles

Explain how you would use:

- Least Privilege.
- Defense in Depth.
- Secure by Default.
- Fail Securely.
- Complete Mediation.

### Task 6: Write a Security Requirements Document

Prepare a short document containing:

1. Application overview.
2. Important assets.
3. Security objectives.
4. Security requirements.
5. Identified threats.
6. Risk assessment.
7. Recommended security controls.

---

## 8. 🧠 Quick Revision

| Concept | Key Point |
|---|---|
| Security Planning | Defines security goals, controls, and responsibilities |
| Threat Awareness | Understanding possible attackers and attack techniques |
| Threat | Something capable of causing harm |
| Vulnerability | A weakness that can be exploited |
| Risk | Potential likelihood and impact of exploitation |
| Compliance Awareness | Understanding applicable legal, regulatory, and industry requirements |
| Security by Design | Integrating security into system architecture |
| Least Privilege | Giving only necessary permissions |
| Defense in Depth | Using multiple layers of protection |
| Fail Securely | Maintaining secure behavior when failures occur |
| Complete Mediation | Checking authorization for every protected access |
| Risk Identification | Discovering and documenting potential security risks |
| Risk Mitigation | Reducing the likelihood or impact of risks |

### 🔑 Key Takeaways

- Security requirements define what an application must protect and how.
- Security planning should happen before development begins.
- Threat awareness helps developers understand what they are defending against.
- Compliance requirements depend on the organization's activities and applicable laws.
- Secure design principles help prevent architectural security weaknesses.
- Risk identification involves examining assets, threats, vulnerabilities, likelihood, and impact.
- Security requirements should be specific enough to test and verify.
- Security is a continuous responsibility throughout the SDLC.

---

## 9. 🎯 Active Recall — Test Your Understanding

Try answering these questions without looking at the explanations.

<details>
<summary><strong>1. What are security requirements?</strong></summary>

Security requirements are conditions and controls that software must satisfy to protect its data, users, infrastructure, and operations from security threats.
</details>

<details>
<summary><strong>2. Why is security planning important?</strong></summary>

It identifies security needs early, defines security controls, assigns responsibilities, and ensures security is considered throughout development.
</details>

<details>
<summary><strong>3. What is the difference between a threat, vulnerability, and risk?</strong></summary>

- Threat: Something capable of causing harm.
- Vulnerability: A weakness that can be exploited.
- Risk: The potential likelihood and impact of that exploitation.
</details>

<details>
<summary><strong>4. What is threat awareness?</strong></summary>

It is understanding potential attackers, attack techniques, and security weaknesses that could affect an application.
</details>

<details>
<summary><strong>5. What is compliance awareness?</strong></summary>

It is understanding the laws, regulations, standards, and policies that an organization must consider when protecting information and developing software.
</details>

<details>
<summary><strong>6. What is the difference between security and compliance?</strong></summary>

Security focuses on protecting systems and information. Compliance focuses on meeting applicable legal, regulatory, and organizational requirements.
</details>

<details>
<summary><strong>7. Explain Defense in Depth.</strong></summary>

Defense in Depth uses multiple layers of security controls so that if one control fails, other controls may still prevent or detect an attack.
</details>

<details>
<summary><strong>8. What is the Principle of Least Privilege?</strong></summary>

It means giving users, applications, and services only the permissions necessary to perform their required tasks.
</details>

<details>
<summary><strong>9. What does Fail Securely mean?</strong></summary>

When a system encounters an error, it should maintain a secure state rather than accidentally granting unauthorized access.
</details>

<details>
<summary><strong>10. How is risk calculated in a basic risk assessment?</strong></summary>

A simple model is:

Risk Score = Likelihood × Impact.

Organizations define the scoring scale and thresholds used to prioritize remediation.
</details>

<details>
<summary><strong>11. What are the four common risk treatment strategies?</strong></summary>

- Avoid.
- Mitigate.
- Transfer.
- Accept.
</details>

<details>
<summary><strong>12. Why should security requirements be identified before development?</strong></summary>

Early identification helps prevent architectural weaknesses, guides secure implementation, and reduces the cost and difficulty of fixing vulnerabilities later.
</details>

---

> 🛡️ **Security Professional Mindset:** Before building an application, understand what you are protecting, who might attack it, how it could be exploited, and what controls are required. Good security begins with good requirements.
