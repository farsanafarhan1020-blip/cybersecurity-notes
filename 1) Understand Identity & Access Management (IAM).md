# 🔐 Identity & Access Management (IAM)

> **Identity and Access Management (IAM)** is a security framework that ensures the right individuals and systems have the right access to the right resources at the right time.

IAM is one of the most important components of cybersecurity, especially in cloud environments. It controls who can access systems, how their identity is verified, what actions they can perform, and when their access should be removed.

Imagine an organization with hundreds of employees, applications, servers, and cloud resources. Without IAM, managing access securely would become extremely difficult.

IAM helps organizations prevent unauthorized access, reduce insider threats, enforce least privilege, and maintain accountability.

---

## 1. 🛡️ What Is Identity & Access Management?

### Understanding Identity

An **identity** represents a person, application, device, or service that needs to access a system.

Examples:

- An employee accessing a company network.
- An administrator managing cloud infrastructure.
- An application accessing a database.
- A virtual machine communicating with another service.
- A developer accessing a GitHub repository.

Each identity must be distinguishable from other identities.

### Understanding Access Management

Access management determines what an identity is allowed to do after its identity has been established.

For example:

Imagine a company has three employees:

| Employee | Identity | Allowed Access |
|---|---|---|
| Alice | alice@company.com | View customer records |
| Bob | bob@company.com | View and update customer records |
| Charlie | charlie@company.com | Manage user accounts |

All three employees may successfully sign in, but they have different permissions.

This is access management in action.

### Main Components of IAM

| Component | Purpose |
|---|---|
| Identity Management | Creates and manages digital identities |
| Authentication | Verifies the identity of a user or system |
| Authorization | Determines what an authenticated identity can access |
| Access Control | Enforces permissions and restrictions |
| Identity Lifecycle | Manages identities from creation to removal |
| Access Governance | Ensures access is appropriate, approved, and reviewed |
| Auditing | Records and monitors identity-related activities |

### Why Is IAM Important?

Without proper IAM:

- Unauthorized users may access sensitive information.
- Employees may retain access after leaving an organization.
- Compromised accounts may become entry points for attackers.
- Users may receive unnecessary administrative privileges.
- Organizations may struggle to identify who performed an action.

**Real-world example:**

Suppose an employee leaves a company, but their cloud account remains active.

An attacker who obtains that employee's old credentials may still be able to access company resources.

A properly managed IAM system helps prevent this by disabling or removing access during employee offboarding.

---

## 2. 🔑 Authentication Concepts

### What Is Authentication?

**Authentication is the process of verifying the identity of a user, device, application, or service.**

In simple terms, authentication answers:

> Who are you?

When you enter your username and password to log in to an account, the system checks whether the credentials match the expected identity.

If verification succeeds, the system establishes that the requester has authenticated as that identity.

### Authentication Factors

Authentication factors are categories of evidence used to verify identity.

There are three traditional authentication factors.

| Factor | Meaning | Examples |
|---|---|---|
| Something you know | Information you know | Password, PIN |
| Something you have | An object you possess | Security key, authenticator device |
| Something you are | A physical or behavioral characteristic | Fingerprint, facial recognition |

### 1. Something You Know

This factor uses secret information known to the user.

Examples:
- Passwords.
- PINs.
- Passphrases.

**Security concern:** Attackers may obtain passwords through phishing, brute-force attacks, credential stuffing, or data breaches.

### 2. Something You Have

This factor requires possession of a physical device or cryptographic credential.

Examples:
- Hardware security keys.
- Authenticator applications.
- Smart cards.
- Device-bound authentication credentials.

Even if an attacker knows your password, they may not be able to authenticate without the required second factor.

### 3. Something You Are

This factor uses biometric characteristics.

Examples:
- Fingerprints.
- Facial recognition.
- Iris recognition.

Biometric authentication can be convenient, but biometric data requires careful protection because it cannot be changed as easily as a password.

### Single-Factor Authentication (SFA)

Single-factor authentication uses one authentication factor.

Example:

```text
User
  |
  v
Enter Password
  |
  v
System Verifies Password
  |
  v
Access Granted / Denied
```

If the password is stolen, an attacker may be able to access the account.

### Multi-Factor Authentication (MFA)

Multi-Factor Authentication requires two or more different authentication factors.

Example:

```text
User
  |
  v
Enter Password
  |
  v
Password Verified
  |
  v
Security Key Verification
  |
  v
Access Granted
```

Even if an attacker steals the password, they still need to satisfy the additional authentication requirement.

**Important distinction:** Using a password and a PIN is generally not MFA because both are knowledge factors. MFA requires different factor categories.

### Common Authentication Methods

| Method | Description |
|---|---|
| Password Authentication | Verifies a password |
| OTP | Uses a one-time passcode |
| Push Authentication | Sends an approval request to a registered device |
| Biometric Authentication | Uses biometric characteristics |
| Hardware Security Key | Uses a physical cryptographic security device |
| Certificate-Based Authentication | Uses digital certificates to establish identity |
| Passwordless Authentication | Uses methods such as passkeys or security keys instead of traditional passwords |

### Understanding Passwordless Authentication

Passwordless authentication allows users to authenticate without entering a traditional password.

One example is a **passkey**.

Passkeys use public-key cryptography:

- The private key remains protected by the user's device or credential provider.
- The public key is registered with the service.
- During authentication, the service verifies a cryptographic response.
- The private key is not sent to the service.

Passkeys can provide strong resistance to phishing when implemented correctly.

### Authentication Protocols and Technologies

These technologies are commonly used in modern authentication systems.

| Technology | Purpose |
|---|---|
| LDAP | Accesses and manages directory information |
| Kerberos | Provides ticket-based network authentication |
| SAML | Exchanges authentication and authorization-related information between systems |
| OAuth 2.0 | Provides delegated authorization |
| OpenID Connect (OIDC) | Adds an authentication layer on top of OAuth 2.0 |
| FIDO2 / WebAuthn | Supports public-key-based passwordless and strong authentication |

**Important:** OAuth 2.0 is primarily an authorization framework, not an authentication protocol. OpenID Connect provides the identity layer.

---

## 3. 🚪 Authorization Concepts

### What Is Authorization?

**Authorization is the process of determining what an authenticated identity is permitted to access or perform.**

Authorization answers:

> What are you allowed to do?

Authentication verifies identity. Authorization determines permissions.

### Authentication vs Authorization

| Authentication | Authorization |
|---|---|
| Verifies identity | Determines access permissions |
| Answers: Who are you? | Answers: What can you do? |
| Usually happens before authorization | Usually follows successful authentication |
| Uses passwords, keys, biometrics, etc. | Uses roles, policies, permissions, and access rules |
| Example: Verifying Alice's login | Example: Allowing Alice to read a customer record |

### Real-World Example

Imagine logging into an online banking application.

**Authentication:**
- You enter your credentials.
- The system verifies your identity.
- MFA may be required.

**Authorization:**
- You can view your own account balance.
- You can transfer money within permitted limits.
- You cannot access another customer's account.
- You cannot modify the bank's internal system settings.

You are authenticated, but your permissions are restricted.

### Access Control Models

Access control models define how permissions are assigned and enforced.

#### 1. Role-Based Access Control (RBAC)

RBAC assigns permissions to roles rather than assigning every permission individually to each user.

Users receive roles, and roles contain permissions.

Example:

| Role | Permissions |
|---|---|
| Administrator | Manage users, systems, and configurations |
| Developer | Access development environments and code repositories |
| Analyst | Read logs and security reports |
| Guest | Access limited public resources |

```text
             Users
               |
       +-------+-------+
       |       |       |
     Alice    Bob    Charlie
       |       |       |
       v       v       v
      Admin Developer Analyst
       |       |       |
       v       v       v
   Permissions assigned to roles
```

**Advantages:**
- Easier permission management.
- Consistent access across users with similar responsibilities.
- Useful in large organizations.

**Limitations:**
- Poorly designed roles can accumulate unnecessary permissions.
- Role explosion can occur when too many specialized roles are created.

#### 2. Attribute-Based Access Control (ABAC)

ABAC makes access decisions using attributes associated with the user, resource, action, and environment.

Example:

A company allows access to a sensitive document only when:

- The user belongs to the Finance department.
- The user has an approved security clearance.
- The request comes from a trusted device.
- The access occurs during an approved period.

Example policy:

```text
ALLOW ACCESS IF:

Department = Finance
AND
Device = Trusted
AND
Clearance = Approved
```

**Advantages:**
- Flexible and context-aware.
- Supports detailed access policies.
- Useful in complex environments.

**Limitations:**
- Policies can become difficult to design and troubleshoot.
- Incorrect attributes can produce incorrect access decisions.

#### 3. Discretionary Access Control (DAC)

DAC allows resource owners to decide who can access their resources.

Example:

A file owner grants another employee permission to read or modify a document.

**Advantages:**
- Flexible.
- Resource owners can manage access directly.

**Limitations:**
- Permissions can spread without centralized oversight.
- Users may grant access too broadly.

#### 4. Mandatory Access Control (MAC)

MAC enforces access using centrally defined security policies and classification levels.

Example classifications:

- Public
- Confidential
- Secret
- Top Secret

A user can access a resource only if the system's security policy permits it.

**Advantages:**
- Strong centralized control.
- Useful in high-security environments.

**Limitations:**
- Less flexible.
- Requires careful classification and administration.

### Access Control Model Comparison

| Model | Access Decision Based On | Common Use |
|---|---|---|
| RBAC | User roles | Enterprise applications |
| ABAC | Attributes and context | Cloud and complex access policies |
| DAC | Resource owner decisions | File permissions |
| MAC | Central security classification | High-security environments |

### Important IAM Principle: Least Privilege

The **Principle of Least Privilege (PoLP)** states that every user, application, and system should receive only the minimum permissions required to perform its assigned tasks.

Example:

A developer who only needs to read application logs should not receive full cloud administrator privileges.

Why?

If that developer's account is compromised, the attacker's potential actions are limited by the permissions available to the account.

### Separation of Duties (SoD)

Separation of Duties ensures that sensitive operations are divided among multiple people or roles.

Example:

- One employee requests a financial transaction.
- Another employee approves it.
- A separate system records and audits the transaction.

This reduces the risk of fraud, misuse, and unauthorized changes.

---

## 4. 🔄 Identity Lifecycle

### What Is Identity Lifecycle Management?

Identity Lifecycle Management is the process of managing digital identities and their access permissions throughout their existence.

An identity should not simply be created and forgotten. It must be managed from onboarding to offboarding.

A typical identity lifecycle contains five stages:

```text
       Identity Request
              |
              v
          Creation
              |
              v
          Provisioning
              |
              v
       Active Management
              |
              v
        Modification
              |
              v
        Deprovisioning
              |
              v
          Removal
```

### 1. Identity Creation

An identity is created when a new employee, application, or service needs access.

Example:

A new employee joins the IT department.

The organization creates:
- A company email account.
- A unique username.
- An employee identity record.
- The required authentication credentials.

### 2. Provisioning

Provisioning means granting the identity the required accounts, permissions, and resources.

Example:

A new security analyst receives:
- Access to the security monitoring platform.
- Permission to view security logs.
- Access to approved investigation tools.

Access should be based on the person's job responsibilities.

### 3. Active Management

While the identity is active, the organization manages its permissions and authentication requirements.

Activities include:
- Password and credential management.
- MFA enforcement.
- Monitoring authentication activity.
- Reviewing account permissions.
- Detecting suspicious account behavior.

### 4. Modification

An identity's permissions may change when its responsibilities change.

Example:

An employee moves from the development team to the security operations team.

Their access should be updated:
- Remove unnecessary development permissions.
- Grant approved security monitoring permissions.
- Review access to existing resources.

This is important because employees can accumulate unnecessary privileges when old permissions are never removed.

### 5. Deprovisioning

Deprovisioning is the process of removing access when an identity no longer needs it.

Example:

An employee leaves the organization.

The organization should:
1. Disable or suspend the account according to policy.
2. Revoke active sessions and tokens where appropriate.
3. Remove access to applications and cloud resources.
4. Revoke or rotate relevant credentials.
5. Recover company-owned devices and security keys.
6. Preserve required audit records.

### Why Is Identity Lifecycle Management Important?

Poor lifecycle management can create:

- Orphaned accounts.
- Stale credentials.
- Unnecessary privileges.
- Unauthorized access.
- Increased attack surface.

**Orphaned account:** An account that remains active even though its owner or responsible entity is no longer associated with the organization.

Attackers may target such accounts because they are sometimes overlooked during routine security reviews.

---

## 5. ⚖️ Access Governance

### What Is Access Governance?

Access Governance is the process of ensuring that access permissions are appropriate, authorized, regularly reviewed, and compliant with organizational security policies.

It answers questions such as:

- Who has access to this resource?
- Why do they need that access?
- Who approved it?
- Is the access still necessary?
- Does it violate any security policy?
- When was it last reviewed?

IAM controls access, while access governance provides oversight and accountability over how that access is managed.

### Main Components of Access Governance

#### 1. Access Requests and Approvals

Users should request access through an approved process.

Example:

A developer requests access to a production database.

The request may require:
- A business justification.
- Manager approval.
- Data owner approval.
- Security approval for sensitive access.

Access should not be granted simply because someone asks for it.

#### 2. Access Reviews (Access Certification)

Organizations periodically review existing permissions to determine whether they are still necessary.

Example:

During a quarterly review, a manager discovers that an employee still has administrator access even though their role changed months ago.

The unnecessary permissions should be removed.

#### 3. Privileged Access Management (PAM)

Privileged accounts have elevated permissions that can affect critical systems.

Examples:
- Domain administrators.
- Cloud administrators.
- Database administrators.
- Root accounts.

PAM focuses on securing these high-risk identities.

Common controls include:
- Restricting privileged access.
- Enforcing MFA.
- Using temporary privileged access.
- Recording administrative sessions.
- Monitoring privileged activities.
- Managing privileged credentials securely.

#### 4. Just-In-Time (JIT) Access

JIT access provides elevated permissions only when they are needed and for a limited period.

Example:

A security engineer needs administrator access to investigate a server issue.

Instead of keeping permanent administrator privileges, the engineer requests temporary access for 30 minutes.

After the approved period, the elevated access is automatically removed.

**Benefit:** Reduces the amount of time powerful permissions remain available.

#### 5. Audit and Compliance

Organizations maintain records of access-related activities.

Examples:
- Who created an account?
- Who approved a permission change?
- Who accessed a sensitive resource?
- Who modified an IAM policy?
- When was an access review completed?

These records support security investigations, accountability, and compliance requirements.

### Important Access Governance Concepts

| Concept | Meaning |
|---|---|
| Least Privilege | Grant only the permissions required |
| Need-to-Know | Allow access only to information required for a task |
| Separation of Duties | Divide sensitive responsibilities |
| Access Certification | Periodically review and approve existing access |
| Privileged Access Management | Secure high-privilege accounts |
| Just-In-Time Access | Grant temporary access when needed |
| Audit Trail | Maintain records of access-related activities |

### IAM vs Access Governance

| IAM | Access Governance |
|---|---|
| Manages identities and access | Oversees whether access is appropriate |
| Creates and manages accounts | Reviews account access and approvals |
| Enforces authentication and permissions | Checks whether access follows policies |
| Handles provisioning and deprovisioning | Ensures access is reviewed and justified |
| Focuses on access management | Focuses on accountability and control |

---

## 6. 🧩 How IAM Works in a Real Organization

Consider a company with developers, security analysts, and administrators.

An employee wants to access a cloud application.

The following process takes place:

```text
          Employee
              |
              v
      Request to Access
              |
              v
       Authentication
       (Verify Identity)
              |
              v
       Authorization
       (Check Permissions)
              |
              v
       Access Governance
       (Approved Policy)
              |
              v
        Access Granted
              |
              v
       Activity Logging
              |
              v
      Continuous Monitoring
```

Let's understand the process:

1. **Identity:** The employee has a unique digital identity.
2. **Authentication:** The system verifies the employee using credentials and MFA.
3. **Authorization:** The system checks the employee's roles and permissions.
4. **Policy enforcement:** Access is allowed only if the applicable security policies permit it.
5. **Logging:** Relevant activities are recorded.
6. **Monitoring:** Suspicious access patterns are investigated.
7. **Lifecycle management:** Permissions are updated or removed when responsibilities change.

This process helps maintain controlled access throughout the employee's time in the organization.

---

## 7. ☁️ IAM in Cloud Computing

IAM is especially important in cloud environments because cloud resources are accessible through management consoles, APIs, applications, and automated services.

Cloud IAM commonly manages:

- Users and groups.
- Roles and service identities.
- Permissions and policies.
- Authentication methods.
- Temporary credentials.
- Federated identities.
- Administrative access.

### Example: AWS IAM

In Amazon Web Services (AWS), IAM manages access to AWS services and resources.

Important concepts:

| AWS IAM Component | Purpose |
|---|---|
| IAM User | Identity representing a user or workload in applicable use cases |
| IAM Group | Collection of IAM users |
| IAM Role | Identity with permissions that can be assumed |
| IAM Policy | Document defining allowed or denied actions |
| MFA | Additional authentication protection |
| Access Keys | Credentials used for programmatic access |

**Important:** Modern AWS workloads commonly use IAM roles and temporary credentials rather than long-lived access keys.

### Example: Azure Entra ID

Microsoft Entra ID provides identity and access capabilities for Microsoft cloud services and connected applications.

Important concepts include:
- Users and groups.
- Enterprise applications.
- Single Sign-On (SSO).
- Multifactor Authentication.
- Conditional Access.
- Identity Protection.

### Example: Google Cloud IAM

Google Cloud IAM manages who can access Google Cloud resources and what actions they can perform.

Important concepts include:
- Principals.
- Roles.
- Permissions.
- IAM policies.
- Service accounts.
- Policy inheritance.

### Cloud IAM Security Best Practices

1. Enforce MFA for human identities, especially privileged users.
2. Apply least privilege to users and workloads.
3. Prefer temporary credentials over long-lived secrets.
4. Avoid sharing user accounts.
5. Protect privileged identities.
6. Review permissions regularly.
7. Disable unused identities and credentials.
8. Monitor authentication and permission changes.
9. Use federation and centralized identity management where appropriate.
10. Audit IAM policies and access activity.

---

## 8. 🧪 Hands-on Lab: Understanding IAM Through Practical Scenarios

You can practice IAM concepts without accessing a real organization's accounts.

### Task 1: Design an IAM Structure

Imagine a company has these employees:

| Employee | Department | Responsibility |
|---|---|---|
| Alice | Development | Develop applications |
| Bob | Security | Monitor security events |
| Charlie | IT Administration | Manage cloud infrastructure |
| David | Finance | Access financial records |

Create suitable roles for each employee.

Example:

| Role | Required Permissions |
|---|---|
| Developer | Access development resources and application repositories |
| Security Analyst | Read security logs and investigate alerts |
| Cloud Administrator | Manage approved cloud infrastructure |
| Finance User | Access approved financial resources |

**Your task:** Explain why each employee should not receive full administrator access.

### Task 2: Identify Authentication and Authorization

Consider these situations:

1. A user enters a password and successfully signs in.
2. A user is asked to verify their identity using a security key.
3. A user can view a file but cannot delete it.
4. An administrator can modify IAM policies.
5. A user is denied access to another employee's account.

Identify whether each situation relates primarily to authentication or authorization.

### Task 3: Detect Excessive Permissions

Consider this fictional IAM policy:

```json
{
  "identity": "developer-user",
  "permissions": [
    "read_application_logs",
    "write_application_logs",
    "delete_storage",
    "create_users",
    "modify_access_policies",
    "manage_virtual_machines"
  ]
}
```

The developer only needs to read application logs.

**Your task:**

1. Identify the unnecessary permissions.
2. Explain the risks of excessive privileges.
3. Create a reduced permission list.
4. Explain how least privilege improves security.

### Task 4: Identity Lifecycle Investigation

Scenario:

An employee moves from the development department to the security department.

Their old development permissions remain active, and they receive additional security permissions.

**Your task:**

1. Identify the lifecycle stage involved.
2. Explain why retaining old permissions is risky.
3. Identify which permissions should be removed.
4. Explain how access reviews could detect the issue.

### Task 5: Design an Access Governance Policy

Write a simple policy covering:

- Who can request access?
- Who approves access?
- How are privileged accounts protected?
- How frequently should access be reviewed?
- What happens when an employee leaves?
- How are access changes recorded?

**Expected output:** A short IAM and access governance policy for a fictional organization.

---

## 9. ⚠️ Common IAM Security Mistakes

| Mistake | Why It Is Dangerous | Better Practice |
|---|---|---|
| Sharing user accounts | Reduces accountability | Use unique identities |
| Weak passwords | Increases account compromise risk | Use strong authentication |
| Not enforcing MFA | Password theft may lead directly to account compromise | Enforce MFA |
| Excessive permissions | Increases potential attack impact | Apply least privilege |
| Permanent admin access | Creates unnecessary privileged exposure | Use JIT access where appropriate |
| Unused active accounts | Creates opportunities for unauthorized access | Regularly review and disable unused accounts |
| Ignoring access reviews | Permissions accumulate over time | Perform periodic access certification |
| Exposed access keys | Attackers may use stolen credentials | Protect, rotate, and revoke credentials as appropriate |
| No audit logging | Makes investigations difficult | Enable and protect relevant audit logs |

---

## 10. ⚡ Quick Revision

### Five Core IAM Concepts

| Topic | Key Question | Main Idea |
|---|---|---|
| IAM | Who should have access? | Manage identities and access |
| Authentication | Who are you? | Verify identity |
| Authorization | What can you do? | Determine permissions |
| Identity Lifecycle | How long should access exist? | Manage identities from creation to removal |
| Access Governance | Is access appropriate? | Review, approve, and audit access |

### Remember These Important Differences

- **Authentication ≠ Authorization:** Identity verification is different from permission checking.
- **RBAC ≠ ABAC:** RBAC uses roles; ABAC uses attributes and context.
- **MFA ≠ Two passwords:** MFA requires different authentication factor categories.
- **Provisioning ≠ Deprovisioning:** Provisioning grants access; deprovisioning removes it.
- **IAM ≠ Access Governance:** IAM manages access; governance oversees its appropriateness and accountability.
- **PAM ≠ JIT:** PAM is the broader discipline of securing privileged access; JIT is one technique used to limit elevated access.

### Key Takeaways

1. IAM controls digital identities and their access to resources.
2. Authentication verifies identity.
3. Authorization determines permissions.
4. Identity lifecycle management prevents stale and orphaned accounts.
5. Access governance ensures permissions remain appropriate.
6. Least privilege reduces the impact of compromised accounts.
7. MFA strengthens account security.
8. Privileged identities require additional protection.
9. Cloud IAM is essential for securing users, workloads, and cloud resources.

---

## 11. 🎯 Active Recall & Exam Questions

Try answering these questions without looking at the notes.

1. What is Identity and Access Management?
2. Explain the difference between authentication and authorization with an example.
3. What are the three traditional authentication factors?
4. Why is MFA more secure than single-factor authentication?
5. What is the difference between RBAC and ABAC?
6. Explain the principle of least privilege.
7. What is identity lifecycle management?
8. What are orphaned accounts, and why are they dangerous?
9. What is access governance?
10. Explain Privileged Access Management and Just-In-Time access.
11. What is Separation of Duties?
12. How does IAM help secure cloud environments?

<details>
<summary><strong>✅ Check Your Answers</strong></summary>

**1. What is IAM?**

A framework of policies, processes, and technologies used to manage digital identities and control access to resources.

**2. Authentication vs Authorization**

Authentication verifies who someone is. Authorization determines what that authenticated identity can do.

**3. Three authentication factors**

- Something you know.
- Something you have.
- Something you are.

**4. Why MFA?**

It requires multiple different authentication factors, making stolen passwords alone insufficient for access.

**5. RBAC vs ABAC**

RBAC assigns permissions through roles. ABAC evaluates attributes and context to make access decisions.

**6. Least Privilege**

Give users and systems only the minimum permissions required for their responsibilities.

**7. Identity Lifecycle Management**

Managing identities and their access from creation and provisioning through modification and eventual deprovisioning.

**8. Orphaned Accounts**

Accounts that remain active even though their owner or responsible entity is no longer associated with the organization. They can provide attackers with overlooked access paths.

**9. Access Governance**

The oversight process that ensures access is properly requested, approved, reviewed, monitored, and compliant with organizational policies.

**10. PAM and JIT**

PAM secures privileged accounts and activities. JIT provides elevated permissions temporarily when required.

**11. Separation of Duties**

Dividing sensitive responsibilities among different people or roles to reduce fraud and unauthorized activity.

**12. IAM in Cloud Security**

It controls access to cloud resources, protects identities, enforces permissions, manages credentials, and supports monitoring and auditing.

</details>

---

## 🏁 Final Thought

IAM is not simply about creating usernames and passwords. It is about controlling digital identities throughout their lifecycle and ensuring that every user, application, and service has only the access it genuinely needs.

**A strong IAM system follows one fundamental principle:**

> Verify every identity, authorize every action, limit every privilege, and continuously review access.
