# 🔐 Understanding Authorization Models

> **Authorization is the process of determining what an authenticated user, application, or system is allowed to access and which actions it can perform.**

In cybersecurity, verifying someone's identity is only the first step. After authentication, systems must determine what that identity is permitted to do.

Imagine an employee successfully logs in to a company's cloud environment. Should that employee be allowed to view customer records, delete databases, create administrator accounts, or modify security policies?

Not necessarily.

Authorization ensures that every identity receives only the permissions required for its responsibilities.

It is one of the most important security controls in operating systems, enterprise networks, web applications, databases, and cloud infrastructure.

---

## 1. 👥 Role-Based Access Control (RBAC)

### What Is RBAC?

**Role-Based Access Control (RBAC)** is an authorization model in which permissions are assigned to roles, and users receive access through those roles.

Instead of assigning permissions individually to every user, an organization creates roles based on job responsibilities.

For example, a company might have:

- Administrator
- Developer
- Security Analyst
- Finance Employee
- Guest User

Each role receives a predefined set of permissions.

### How RBAC Works

RBAC involves three main components:

| Component | Description |
|---|---|
| Users | Individuals or identities requesting access |
| Roles | Job functions or responsibility groups |
| Permissions | Actions allowed on specific resources |

The relationship looks like this:

```text
             USERS
               |
       +-------+-------+
       |       |       |
     Alice    Bob    Charlie
       |       |       |
       v       v       v
      Admin Developer Analyst
       |       |       |
       +-------+-------+
               |
               v
          PERMISSIONS
               |
       +-------+-------+
       |       |       |
      Read   Write   Manage
```

### Real-World Example

Imagine a cybersecurity company with three employees.

| Employee | Role | Permissions |
|---|---|---|
| Alice | Administrator | Manage users, systems, and security policies |
| Bob | Developer | Access development systems and application code |
| Charlie | Security Analyst | View logs, investigate alerts, and generate reports |

When a new developer joins the organization, the administrator assigns the Developer role.

The employee automatically receives the permissions associated with that role.

This makes access management easier and more consistent.

### Types of RBAC

RBAC can be implemented with different levels of complexity.

**1. Flat RBAC**

Users are assigned roles, and roles contain permissions.

Example:

```text
User → Developer Role → Read and Write Code
```

**2. Hierarchical RBAC**

Roles inherit permissions from other roles.

Example:

```text
             Administrator
                   |
             Senior Analyst
                   |
                Analyst
```

In a properly designed hierarchy, higher-level roles can inherit permissions from lower-level roles.

The exact inheritance structure depends on the organization's policy.

**3. Constrained RBAC**

Additional restrictions are applied to prevent conflicts and dangerous combinations of permissions.

Example:

An employee may be allowed to request a financial payment but cannot approve their own payment request.

This supports Separation of Duties.

### Advantages of RBAC

- Simplifies permission management.
- Reduces repetitive permission assignments.
- Makes onboarding and offboarding easier.
- Supports consistent access policies.
- Helps enforce least privilege when roles are designed properly.
- Works well in large organizations.

### Limitations of RBAC

- Poorly designed roles can provide excessive permissions.
- Too many specialized roles can create role explosion.
- It may not handle dynamic context easily.
- Role membership must be reviewed regularly.

### RBAC vs ABAC

RBAC is not the only authorization model.

Another important model is **Attribute-Based Access Control (ABAC)**.

| RBAC | ABAC |
|---|---|
| Access is based on roles | Access is based on attributes |
| Uses predefined roles | Uses policy conditions |
| Easier to understand and manage in many environments | More flexible and context-aware |
| Example: Developer can access development resources | Example: Developer can access a resource only from a trusted device |
| Suitable for job-based permissions | Suitable for dynamic access decisions |

**Example:**

RBAC policy:

```text
IF User Role = Developer
THEN Allow Development Access
```

ABAC policy:

```text
IF Department = Development
AND Device = Trusted
AND Environment = Development
THEN Allow Access
```

### When Is RBAC Useful?

RBAC is commonly used in:

- Enterprise applications.
- Operating systems.
- Database management.
- Cloud platforms.
- Security monitoring systems.
- Content management systems.

**Security lesson:** Assign permissions to carefully designed roles rather than giving every user broad access.

---

## 2. 🎯 Principle of Least Privilege

### What Is the Principle of Least Privilege?

The **Principle of Least Privilege (PoLP)** states that every user, application, process, and system should receive only the minimum permissions necessary to perform its assigned tasks.

In simple terms:

> Give an identity only the access it needs, and nothing more.

This is one of the most important principles in access control and cybersecurity.

### Real-World Example

Imagine a company has a security analyst who needs to investigate security logs.

The analyst requires:

- Permission to read security logs.
- Permission to view security alerts.
- Permission to generate investigation reports.

The analyst does not need:

- Permission to delete production databases.
- Permission to create administrator accounts.
- Permission to modify cloud security policies.
- Full control over the company's infrastructure.

Giving the analyst unnecessary administrator privileges violates least privilege.

### How Least Privilege Reduces Risk

Consider two situations.

<box border radius="lg" gap={3} padding={3}>
  **Scenario A — Excessive Permissions**

  <badge color="danger">High potential impact</badge>
  <row align="center" gap={2}>
    <icon name="user" size="xl"/>
    <icon name="arrow-right"/>
    **Compromised developer account**
  </row>
  <row align="center" gap={2}>
    <icon name="shield-alert" color="danger" size="xl"/>
    **Administrator-level access**
  </row>
  <text color="secondary" size="sm">An attacker may be able to modify security policies, access sensitive data, or delete resources.</text>
  <divider color="subtle"/>
  **Scenario B — Least Privilege**

  <badge color="success">Reduced potential impact</badge>
  <row align="center" gap={2}>
    <icon name="user" size="xl"/>
    <icon name="arrow-right"/>
    **Compromised developer account**
  </row>
  <row align="center" gap={2}>
    <icon name="shield-check" color="success" size="xl"/>
    **Limited development permissions**
  </row>
  <text color="secondary" size="sm">The attacker is restricted to the permissions available to that identity, limiting potential damage.</text>
</box>

Least privilege does not guarantee that an attack will be prevented. It reduces the possible impact of a compromised identity.

### Important Least Privilege Practices

**1. Minimum Permissions**

Grant only the permissions required for a specific job.

**2. Temporary Access**

Provide elevated permissions only when necessary.

**3. Regular Permission Reviews**

Remove unnecessary permissions when responsibilities change.

**4. Separate Administrative Accounts**

Use dedicated privileged identities for administrative tasks rather than using administrator accounts for everyday activities.

**5. Restrict Service Accounts**

Applications and automated processes should have only the permissions required for their functions.

### Just-In-Time (JIT) Access

JIT access is a technique for providing elevated permissions temporarily.

Example:

A security engineer needs administrator access to investigate a server issue.

Instead of keeping permanent administrator access:

1. The engineer requests temporary access.
2. The request is approved.
3. Elevated permissions are granted for a limited period.
4. Permissions expire automatically.

```text
       Access Request
             |
             v
          Approval
             |
             v
     Temporary Privileges
             |
             v
      Required Task Done
             |
             v
     Privileges Automatically
           Removed
```

This reduces the time during which powerful permissions are available.

### Just Enough Administration (JEA)

Just Enough Administration limits administrative users to a specific set of permitted management operations.

For example, an administrator may be allowed to restart a particular service without receiving full control over the entire server.

### Why Least Privilege Matters

- Reduces unauthorized access.
- Limits the impact of compromised accounts.
- Reduces insider-threat risks.
- Prevents accidental destructive actions.
- Helps protect sensitive information.
- Supports compliance and accountability.

**Security lesson:** An account should never have administrator access simply because it is convenient.

---

## 3. 🚪 Access Management

### What Is Access Management?

Access Management is the process of controlling how users, applications, and systems access protected resources.

It includes identifying access requirements, enforcing authorization policies, managing sessions, and monitoring access activities.

IAM manages the broader identity and access lifecycle, while access management focuses on controlling access to resources.

### Main Components of Access Management

| Component | Purpose |
|---|---|
| Access Control | Enforces who can access a resource |
| Permission Management | Defines allowed actions |
| Policy Enforcement | Applies access rules |
| Session Management | Controls active access sessions |
| Privileged Access Management | Secures high-privilege identities |
| Access Reviews | Identifies unnecessary permissions |
| Audit Logging | Records access-related activities |

### Types of Access

Access permissions generally describe what actions an identity can perform.

Common examples:

| Permission | Meaning |
|---|---|
| Read | View information |
| Write | Create or modify information |
| Execute | Run a program or operation |
| Delete | Remove information |
| Create | Create resources or objects |
| Update | Modify existing resources |
| Administer | Manage settings, users, and permissions |

The exact meaning of these permissions depends on the application or system.

### Access Control in Different Environments

**1. Operating Systems**

Linux uses file permissions and ownership to control access to files and directories.

Example:

```bash
ls -l
```

Possible output:

```text
-rwxr-xr-- 1 farhan developers 1024 app.sh
```

Permission breakdown:

```text
-     rwx     r-x     r--
|      |       |       |
|    Owner   Group   Others
|
File type
```

- Owner: Read, write, execute.
- Group: Read and execute.
- Others: Read only.

**2. Web Applications**

Applications use authorization to control access to features and data.

Example:

- Regular users can view their own profiles.
- Managers can view team information.
- Administrators can manage accounts.

**3. Cloud Environments**

Cloud IAM policies control access to resources such as virtual machines, storage, databases, and networking services.

Example:

A cloud application may receive permission to read objects from one specific storage bucket but not access other buckets.

### Authentication vs Access Management

| Authentication | Access Management |
|---|---|
| Verifies identity | Controls resource access |
| Uses credentials and authentication factors | Uses policies, roles, and permissions |
| Answers who you are | Determines what you can access |
| Example: Password verification | Example: Permission to read a database |

**Important:** Successful authentication does not mean unrestricted access.

---

## 4. ⚙️ Permission Assignment

### What Is Permission Assignment?

Permission assignment is the process of granting specific access rights to users, roles, groups, or service identities.

It determines which actions an identity can perform on a particular resource.

Example:

A developer may receive:

- Read access to application logs.
- Write access to development files.
- No access to production databases.

### Basic Permission Structure

A permission assignment can be understood through four elements:

| Element | Meaning |
|---|---|
| Subject | Who is requesting access? |
| Resource | What is being accessed? |
| Action | What operation is requested? |
| Policy | What rule determines whether access is allowed? |

Example:

```text
Subject: developer-user

Resource: application-logs

Action: Read

Policy: Allow

Result: Access Granted
```

### How Permissions Are Assigned

A typical permission assignment process follows these steps:

```text
       User Requests Access
                |
                v
       Identify Required Resource
                |
                v
       Determine Required Actions
                |
                v
        Review Business Need
                |
                v
          Approval Process
                |
                v
       Assign Role or Permission
                |
                v
         Verify Access
                |
                v
        Monitor and Review
```

### Permission Assignment Methods

#### 1. Direct Permission Assignment

Permissions are assigned directly to an individual identity.

Example:

Alice receives permission to read a specific document.

**Advantage:** Simple for limited, special cases.

**Disadvantage:** Difficult to manage at scale and can create inconsistent permissions.

#### 2. Group-Based Assignment

Users are placed into groups, and permissions are assigned to those groups.

Example:

```text
Users
  |
  v
Security Analysts Group
  |
  v
Read Security Logs
View Alerts
Generate Reports
```

When a new analyst joins, adding them to the approved group grants the required access.

**Advantage:** Easier centralized management.

#### 3. Role-Based Assignment

Users receive roles, and roles contain predefined permissions.

Example:

```text
Bob
 |
 v
Developer Role
 |
 +-- Read Source Code
 +-- Write Source Code
 +-- Access Test Environment
```

**Advantage:** Suitable for organizations with clearly defined job responsibilities.

#### 4. Policy-Based Assignment

Access is controlled through policies that evaluate conditions.

Example:

```text
ALLOW ACCESS IF:

User Role = Security Analyst
AND
Resource = Security Logs
AND
Device = Trusted
```

This is useful when access depends on multiple conditions.

### Allow vs Deny Permissions

Access policies may explicitly allow or deny an action.

| Policy | Meaning |
|---|---|
| Allow | Permits the specified action |
| Deny | Blocks the specified action |

In many systems, explicit deny rules take precedence over allow rules. However, the exact evaluation behavior depends on the authorization system.

### Permission Inheritance

Some systems allow permissions to be inherited from parent resources or groups.

Example:

```text
Company Folder
      |
      v
Department Folder
      |
      v
Project Folder
      |
      v
Individual Files
```

A user may receive access to a child resource through permissions assigned to a parent resource.

**Security risk:** Inherited permissions can unintentionally expose sensitive resources if administrators do not understand the full permission structure.

### Permission Assignment Best Practices

- Assign permissions based on job responsibilities.
- Prefer groups and roles over excessive direct assignments.
- Apply least privilege.
- Require approval for sensitive access.
- Avoid unnecessary administrator permissions.
- Review inherited permissions.
- Remove access when it is no longer required.
- Record permission changes in audit logs.
- Test access after policy changes.

---

## 5. ⚠️ Security Implications of Authorization Models

Authorization mistakes can create serious security vulnerabilities even when authentication is implemented correctly.

An application may successfully verify a user's identity but still expose sensitive resources because its authorization logic is incorrect.

### Common Authorization Vulnerabilities

#### 1. Broken Access Control

Broken Access Control occurs when an application fails to correctly enforce restrictions on what authenticated users can access or modify.

Example:

A normal employee can access an administrator-only page because the application checks whether the user is logged in but does not verify their role.

**Potential impact:**
- Unauthorized data access.
- Privilege escalation.
- Unauthorized administrative operations.
- Data modification or deletion.

**Prevention:**
- Enforce authorization on the server side.
- Apply least privilege.
- Test access restrictions.
- Deny access by default.

#### 2. Insecure Direct Object Reference (IDOR)

IDOR is an access control vulnerability where an application exposes direct references to objects without properly checking whether the requester is authorized to access them.

Example:

A user accesses their invoice using:

```text
https://example.com/invoice?id=1001
```

They change the identifier:

```text
https://example.com/invoice?id=1002
```

If the application displays another user's invoice without checking ownership or permission, an IDOR vulnerability exists.

**Important:** Hiding or changing object identifiers is not a substitute for authorization checks.

**Prevention:**
- Verify resource ownership or access rights on the server.
- Check authorization for every sensitive object request.
- Apply deny-by-default policies.

#### 3. Privilege Escalation

Privilege escalation occurs when an attacker gains permissions beyond those originally assigned to their identity.

Two main types:

| Type | Description |
|---|---|
| Horizontal Privilege Escalation | Accessing another user's resources at the same privilege level |
| Vertical Privilege Escalation | Gaining higher-level permissions than originally allowed |

**Horizontal example:**

Alice accesses Bob's private account information.

**Vertical example:**

A regular employee gains administrator privileges.

#### 4. Excessive Permissions

Excessive permissions occur when users or systems receive more access than necessary.

Example:

A monitoring application receives full administrator access even though it only needs permission to read logs.

**Risk:**

If the application is compromised, the attacker may inherit those excessive permissions.

**Prevention:** Apply least privilege and periodically review permissions.

#### 5. Missing Server-Side Authorization

Some applications hide buttons or pages based on user roles but fail to enforce the same restrictions on the server.

Example:

The user interface hides the Delete button from regular users, but the backend still accepts a direct delete request.

An attacker may bypass the interface and send the request manually.

**Prevention:**

Always validate permissions on the server for sensitive operations. Client-side restrictions are not security boundaries.

#### 6. Misconfigured Role Inheritance

Incorrect role hierarchies can unintentionally grant users powerful permissions.

Example:

A junior analyst inherits administrator permissions because of an incorrectly configured role hierarchy.

**Prevention:**
- Review role inheritance.
- Keep roles simple.
- Test effective permissions.
- Monitor changes to role assignments.

### Authorization Security Controls

| Security Control | Purpose |
|---|---|
| Least Privilege | Limits unnecessary permissions |
| Deny by Default | Blocks access unless explicitly allowed |
| RBAC | Organizes access through roles |
| ABAC | Makes context-aware access decisions |
| Separation of Duties | Prevents dangerous combinations of permissions |
| Access Reviews | Identifies unnecessary or outdated access |
| Server-Side Validation | Prevents client-side authorization bypass |
| Audit Logging | Records access decisions and permission changes |
| JIT Access | Limits the duration of elevated privileges |

---

## 6. ☁️ Authorization in Cloud Environments

Cloud platforms rely heavily on authorization policies to protect infrastructure and data.

Cloud authorization determines which identities can perform actions on cloud resources.

### Example: AWS IAM Authorization

AWS IAM uses policies to define permissions.

A simplified example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::training-bucket/*"
    }
  ]
}
```

### Understanding the Policy

| Field | Meaning |
|---|---|
| Version | Policy language version |
| Effect | Allow or Deny |
| Action | Requested operation |
| Resource | Resource affected by the permission |

This example allows the specified identity to retrieve objects from the named training bucket, subject to other applicable authorization controls.

It does not grant permission to delete objects or modify the bucket policy.

### Other Cloud Authorization Concepts

**Microsoft Azure:**

Azure Role-Based Access Control (Azure RBAC) manages access to Azure resources using roles, assignments, scopes, and permissions.

**Google Cloud:**

Google Cloud IAM uses principals, roles, permissions, and policies to control access to resources.

### Cloud Authorization Best Practices

1. Avoid granting broad administrator roles unnecessarily.
2. Assign permissions at the narrowest practical scope.
3. Prefer workload identities and temporary credentials.
4. Review inherited permissions.
5. Monitor policy modifications.
6. Separate production and development access.
7. Regularly remove unused permissions.
8. Test policies before applying them to critical environments.

---

## 7. 🧪 Hands-on Lab: Practice Authorization Concepts

These exercises help you understand how authorization decisions are made.

Use a local environment or an authorized training application. Do not test access-control weaknesses against systems without permission.

### Task 1: Design an RBAC System

Imagine you are responsible for designing access control for a cybersecurity company.

The company has these employees:

| Employee | Department | Responsibility |
|---|---|---|
| Alice | Development | Develop applications |
| Bob | Security | Monitor logs and investigate alerts |
| Charlie | IT | Manage infrastructure |
| David | Finance | Manage financial records |

**Your task:**

1. Create one role for each department.
2. Assign appropriate permissions to each role.
3. Identify which roles require administrative privileges.
4. Explain why regular employees should not receive full administrator access.

Expected output:

| Role | Read | Write | Delete | Administer |
|---|---|---|---|---|
| Developer | ? | ? | ? | ? |
| Security Analyst | ? | ? | ? | ? |
| IT Administrator | ? | ? | ? | ? |
| Finance User | ? | ? | ? | ? |

Fill the table according to realistic job requirements.

### Task 2: Identify Least Privilege Violations

Consider the following permission assignment:

```text
User: security-analyst

Permissions:
- Read Security Logs
- View Security Alerts
- Generate Reports
- Delete Production Database
- Create Administrator Accounts
- Modify Cloud Security Policies
```

Answer:

1. Which permissions are necessary?
2. Which permissions are excessive?
3. What could happen if this account is compromised?
4. How would you correct the permission assignment?

### Task 3: Understand Linux Access Control

Create a test file:

```bash
touch security.txt
```

Check its permissions:

```bash
ls -l security.txt
```

Change its permissions:

```bash
chmod 640 security.txt
```

Check again:

```bash
ls -l security.txt
```

Understand the permission values:

| Number | Permission |
|---|---|
| 4 | Read |
| 2 | Write |
| 1 | Execute |
| 6 | Read + Write |
| 5 | Read + Execute |
| 7 | Read + Write + Execute |

For `640`:

- Owner: Read and write.
- Group: Read only.
- Others: No permissions.

**Your task:** Explain why these permissions may be appropriate for a sensitive file and identify situations where different permissions would be required.

### Task 4: Understand an Authorization Vulnerability

Consider this fictional application endpoint:

```text
GET /api/invoices/1001
```

A user is authenticated and requests an invoice.

The application verifies that the user is logged in but does not verify whether the invoice belongs to that user or is otherwise accessible to them.

Answer:

1. What authorization vulnerability could exist?
2. Why is authentication alone insufficient?
3. What server-side check should be implemented?
4. How would you test the fix using two authorized test accounts?

### Task 5: Review a Cloud IAM Policy

Review the example AWS IAM policy in Section 6.

Answer:

1. Which action is allowed?
2. Which resource does the policy apply to?
3. Does it allow object deletion?
4. Does it allow bucket-policy modification?
5. Why is restricting permissions to a specific resource important?

---

## 8. 🧠 Common Authorization Mistakes

| Mistake | Security Risk | Better Practice |
|---|---|---|
| Giving everyone administrator access | Large attack impact | Apply least privilege |
| Assigning permissions without approval | Unauthorized access | Use approval workflows |
| Relying only on frontend restrictions | Authorization bypass | Enforce server-side checks |
| Ignoring role inheritance | Unexpected permissions | Review effective permissions |
| Never reviewing permissions | Privilege accumulation | Perform regular access reviews |
| Using broad cloud policies | Excessive resource access | Restrict actions and resource scope |
| Trusting user-supplied resource IDs | IDOR vulnerabilities | Validate ownership and access |
| Sharing privileged accounts | Poor accountability | Use unique identities |
| Permanent administrative access | Increased exposure | Use JIT and PAM |

---

## 9. ⚡ Quick Revision

### Authorization Models at a Glance

| Concept | Main Purpose |
|---|---|
| RBAC | Assign permissions through roles |
| ABAC | Make access decisions using attributes |
| Least Privilege | Grant minimum necessary permissions |
| Access Management | Control access to protected resources |
| Permission Assignment | Grant specific actions to identities |
| JIT Access | Provide temporary elevated permissions |
| Separation of Duties | Divide sensitive responsibilities |
| Deny by Default | Block access unless explicitly permitted |

### Important Security Distinctions

- **Authentication vs Authorization:** Authentication verifies identity; authorization determines permitted actions.
- **RBAC vs ABAC:** RBAC uses roles; ABAC uses attributes and contextual conditions.
- **Horizontal vs Vertical Privilege Escalation:** Horizontal accesses another user's resources; vertical gains higher privileges.
- **Frontend vs Backend Authorization:** Frontend restrictions improve user experience; backend checks enforce security.
- **Direct vs Group-Based Assignment:** Direct assignment targets individual identities; group-based assignment manages permissions collectively.
- **PAM vs JIT:** PAM is the broader practice of securing privileged access; JIT provides temporary elevated permissions.

### Key Takeaways

1. Authorization determines what authenticated identities can access.
2. RBAC simplifies access management through predefined roles.
3. Least privilege reduces the potential impact of compromised accounts.
4. Access management controls how identities interact with resources.
5. Permission assignment must be based on legitimate business requirements.
6. Misconfigured authorization can expose sensitive data even when authentication is strong.
7. IDOR and privilege escalation are important access-control vulnerabilities.
8. Authorization must always be enforced on the server side.
9. Cloud IAM policies should restrict both actions and resource scope.
10. Regular access reviews help prevent unnecessary privileges from accumulating.

---

## 10. 🎯 Active Recall & Exam Questions

Try answering these questions without looking at the notes.

1. What is authorization, and how is it different from authentication?
2. Explain Role-Based Access Control with a practical example.
3. What are the main components of RBAC?
4. Explain flat, hierarchical, and constrained RBAC.
5. What is the difference between RBAC and ABAC?
6. Explain the Principle of Least Privilege.
7. What is Just-In-Time access?
8. What are the main components of access management?
9. What is permission assignment?
10. Explain direct, group-based, role-based, and policy-based permission assignment.
11. What is permission inheritance, and why can it be risky?
12. What is Broken Access Control?
13. Explain IDOR with an example.
14. What is horizontal privilege escalation?
15. What is vertical privilege escalation?
16. Why must authorization be enforced on the server side?
17. How does least privilege reduce the impact of a compromised cloud account?
18. Why should cloud IAM policies restrict both actions and resources?

<details>
<summary><strong>✅ Check Your Answers</strong></summary>

**1. Authorization vs Authentication**

Authentication verifies identity. Authorization determines what that identity is allowed to do.

**2. RBAC**

RBAC assigns permissions to predefined roles. Users receive permissions through their assigned roles.

**3. Main RBAC Components**

Users, roles, and permissions.

**4. Types of RBAC**

- Flat: Basic user-to-role assignment.
- Hierarchical: Roles inherit permissions from other roles.
- Constrained: Restrictions prevent conflicting or dangerous role combinations.

**5. RBAC vs ABAC**

RBAC uses predefined roles. ABAC evaluates attributes such as department, device status, resource sensitivity, and environment.

**6. Least Privilege**

Give every identity only the minimum permissions required to perform its responsibilities.

**7. JIT Access**

A technique that provides elevated permissions temporarily when needed and removes them afterward.

**8. Access Management Components**

Access control, permission management, policy enforcement, session management, privileged access management, reviews, and audit logging.

**9. Permission Assignment**

The process of granting specific permissions to users, groups, roles, or service identities.

**10. Permission Assignment Methods**

- Direct: Assign to an individual.
- Group-based: Assign through group membership.
- Role-based: Assign through a predefined role.
- Policy-based: Determine access using policy conditions.

**11. Permission Inheritance**

Permissions flow from parent resources, groups, or roles to child resources or members. Incorrect inheritance can expose resources unintentionally.

**12. Broken Access Control**

A vulnerability where an application fails to correctly enforce access restrictions.

**13. IDOR**

An attacker manipulates a resource identifier to access another user's resource because the application fails to verify authorization.

**14. Horizontal Privilege Escalation**

Accessing another user's resources at a similar privilege level.

**15. Vertical Privilege Escalation**

Obtaining higher privileges than the identity was originally authorized to have.

**16. Server-Side Authorization**

Client-side controls can be bypassed. The server must independently verify every sensitive access request.

**17. Least Privilege in Cloud Security**

It restricts compromised identities to a smaller set of actions and resources, reducing potential damage.

**18. Restricting Cloud Policies**

Limiting actions and resources prevents identities from accessing or modifying unrelated cloud resources.

</details>

---

## 🏁 Final Thought

A secure system must do more than verify who is logging in. It must carefully control what that identity can access and which operations it can perform.

Think like a security engineer:

- Who is requesting access?
- What resource are they trying to access?
- What action are they performing?
- Is that action authorized?
- Does the identity actually need that permission?
- What could happen if the identity is compromised?

> **Strong authentication proves who you are. Strong authorization limits what you can do.**
