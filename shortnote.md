# 🛡️ MODULE 10 — IDENTITY & ACCESS MANAGEMENT (IAM) AND ACTIVE DIRECTORY

# 1. Identity & Access Management (IAM)

## What is IAM?

**Identity and Access Management (IAM)** is a framework of policies, processes, and technologies used by organizations to manage digital identities and control access to resources.

Its main purpose is to ensure that:

- The right person gets access.
- They get access to the right resources.
- They have only the required permissions.
- Access is available at the appropriate time.

### Why is IAM important?

Imagine a company with 500 employees.

Without IAM, administrators would have to manage accounts and permissions individually.

With IAM, the company can create predefined roles such as:

- HR Department
- IT Department
- Finance Department
- Management

When a new employee joins, the administrator creates an account and assigns the appropriate role.

The employee receives the permissions associated with that role.

**Benefits of IAM:**
- Centralized identity management.
- Easier employee onboarding and offboarding.
- Reduced unauthorized access.
- Better security and accountability.
- Easier access reviews and auditing.

## Four important IAM concepts

| Concept | Meaning | Example |
|---|---|---|
| Identification | Claiming an identity | Entering username |
| Authentication | Verifying identity | Entering password |
| Authorization | Determining access permissions | Allowing access to HR files |
| Accountability | Tracking user activities | Recording login history |

### Remember the difference

- Identification = Who are you?
- Authentication = Prove who you are.
- Authorization = What can you access?
- Accountability = What did you do?

## IAM Identity Lifecycle

The identity lifecycle has three important stages.

```text
       PROVISIONING
       Create account
       Assign permissions
             |
             v
        MODIFICATION
       Update permissions
       Change roles
             |
             v
       DEPROVISIONING
       Disable account
       Revoke access
       Delete when appropriate
```

### 1. Provisioning

The process of creating a new user account and providing the required access.

Example:
- A new employee joins the IT department.
- Administrator creates an account.
- Adds the employee to the IT security group.
- Assigns the necessary permissions.

### 2. Modification

Updating user accounts and permissions when an employee's responsibilities change.

Example:

An employee moves from Finance to HR.

- Remove old Finance permissions.
- Assign HR permissions.
- Update group memberships.

### 3. Deprovisioning

Removing access when an employee leaves the organization.

Important steps:

1. Disable the account.
2. Revoke access and active sessions.
3. Preserve necessary logs and audit evidence.
4. Transfer ownership of important resources.
5. Delete the account according to retention policies.

**Important:** Inactive employee accounts can become security risks if they remain active.

## Access Governance

Access governance ensures that users have appropriate permissions according to organizational policies.

It includes:
- Regular access reviews.
- Permission auditing.
- Role management.
- Least privilege enforcement.
- Removing unnecessary access.

---

# 2. Authentication Mechanisms

## What is Authentication?

Authentication is the process of verifying the identity of a user, device, or system before allowing access.

Example: When you log in to your company's account, the system checks your credentials to verify your identity.

## 2.1 Password-Based Authentication

Password authentication uses a secret password to verify a user's identity.

### Why should passwords never be stored in plaintext?

Imagine a company stores employee passwords directly in its database.

If an attacker compromises that database, they can immediately obtain all stored passwords.

This can lead to:
- Account compromise.
- Privileged account compromise.
- Unauthorized access.
- Further attacks against other systems.

### Solution: Password Hashing

Password hashing converts a password into a fixed-length hash value using a one-way cryptographic function.

Example:

```text
Original Password
       |
       v
Password Hashing Algorithm
       |
       v
Stored Password Hash
```

When a user logs in:

```text
User enters password
        |
        v
Server receives it over HTTPS
        |
        v
Password hashing and verification
        |
        v
Compare with stored hash
        |
        v
Authentication successful / failed
```

The server uses the stored salt and password-hashing algorithm to verify the submitted password.

**Important concepts:**

- Hashing is designed to be one-way.
- Salting adds a unique random value to each password before hashing.
- Salt prevents attackers from efficiently using precomputed hash tables.
- Strong password-hashing algorithms make guessing attacks more expensive.

Recommended password-hashing algorithms:

| Algorithm | Purpose |
|---|---|
| Argon2id | Modern password hashing |
| bcrypt | Password hashing |
| scrypt | Password hashing |

**Important:** Password hashing protects stored passwords. HTTPS/TLS protects passwords during transmission.

## 2.2 Common Password Attacks

| Attack | Explanation | Example |
|---|---|---|
| Brute Force | Tries many possible passwords | Trying thousands of combinations |
| Dictionary Attack | Tries commonly used passwords | Trying `password123` |
| Credential Stuffing | Uses leaked username-password combinations | Trying leaked credentials on another website |
| Password Spraying | Tries a few common passwords against many accounts | Trying `Welcome123` against 100 employees |
| Phishing | Tricks users into revealing credentials | Fake company login page |

### Brute Force vs Dictionary Attack vs Password Spraying

- **Brute Force:** Many password combinations against a target account.
- **Dictionary Attack:** Common words and known password lists.
- **Password Spraying:** A few common passwords across many accounts.

## 2.3 Multi-Factor Authentication (MFA)

MFA uses two or more different authentication factor categories to verify identity.

### Three authentication factors

| Factor | Meaning | Examples |
|---|---|---|
| Something you know | Knowledge | Password, PIN |
| Something you have | Possession | Smartphone, security key |
| Something you are | Biometrics | Fingerprint, face recognition |

Example:

A user logs in using:
1. Password.
2. OTP received on their authentication device.

This is two-factor authentication because it uses two different factor categories.

**Important exam points:**
- 2FA means exactly two authentication factors.
- MFA means two or more authentication factors.
- Two passwords do not count as MFA because both are something you know.

### Common MFA attacks

- OTP theft.
- SIM swapping.
- MFA fatigue attacks.
- Phishing.
- Session hijacking.

**Phishing-resistant authentication:** FIDO2, WebAuthn, and passkeys provide stronger protection against credential phishing than SMS OTP.

## 2.4 Single Sign-On (SSO)

SSO allows a user to authenticate once and access multiple authorized applications without repeatedly entering credentials.

Example:

An employee logs into their company account and can access:

- Company email.
- HR portal.
- Internal dashboard.
- Project management software.

```text
             USER
               |
               v
       Identity Provider
               |
        Authentication
               |
          Issues Token
               |
       ----------------
       |       |      |
       v       v      v
      HR      Email  Dashboard
      App      App     App
```

**Benefits:**
- Convenient login experience.
- Reduced password fatigue.
- Centralized authentication.
- Easier access management.

**Security risk:** If an attacker compromises the central identity account, multiple connected applications may be affected.

## 2.5 Federation

Federation establishes trust between separate identity systems or organizations.

Example:

An employee from Company A uses their Company A identity to access an approved application hosted by Company B.

The external application trusts the identity provider of Company A.

### SSO vs Federation

| SSO | Federation |
|---|---|
| Single login experience across applications | Trust between separate identity systems |
| Often used within an organization | Often used across organizations |
| Focuses on reducing repeated logins | Focuses on identity trust and sharing |

Federation can enable SSO across organizations.

## 2.6 Important Authentication Protocols

| Protocol | Purpose |
|---|---|
| SAML | XML-based authentication assertions, commonly used for enterprise SSO and federation |
| OAuth 2.0 | Delegated authorization |
| OpenID Connect (OIDC) | Authentication and identity layer built on OAuth 2.0 |

**Remember:**

- OAuth 2.0 = Authorization.
- OpenID Connect = Authentication.
- SAML = Enterprise identity federation and SSO.

## 2.7 Modern Authentication

Modern authentication reduces dependence on traditional passwords.

**Passwordless Authentication**

Uses:
- Passkeys.
- Biometrics.
- Hardware security keys.
- FIDO2/WebAuthn.

**Risk-Based Authentication**

Evaluates signals such as:
- Login location.
- Device information.
- IP reputation.
- Login behavior.

**Adaptive Authentication**

Changes authentication requirements according to risk.

Example:

A login from a recognized device may require normal authentication, while an unusual login from a suspicious location may trigger additional verification.

---

# 3. Authorization Models

## What is Authorization?

Authorization determines what an authenticated user is allowed to access or perform.

Example:

An HR employee may access employee records but should not automatically have access to the Finance department's confidential files.

## 3.1 Role-Based Access Control (RBAC)

RBAC assigns permissions to roles, and users receive permissions through their assigned roles.

Example:

| Role | Permissions |
|---|---|
| HR | Access employee records |
| Finance | Access financial records |
| IT Admin | Manage IT systems |
| Employee | Access basic company resources |

```text
         USERS
           |
           v
          ROLES
           |
           v
       PERMISSIONS
           |
           v
        RESOURCES
```

**Benefits:**
- Centralized access management.
- Easier administration.
- Consistent permissions.
- Reduced manual work.

## 3.2 Principle of Least Privilege (PoLP)

The Principle of Least Privilege means giving users, applications, and systems only the minimum permissions required to perform their tasks.

Example:

A normal employee needs to read a document but does not need permission to delete it.

Give read access, not full control.

### Just Enough Access (JEA)

Provides only the permissions needed for a particular task.

Example: An administrator can restart a service but cannot modify unrelated server settings.

### Just-in-Time Access (JIT)

Provides elevated permissions temporarily when required.

Example:

An administrator receives Domain Admin privileges for a specific maintenance task, and the privileges are removed afterward.

| JEA | JIT |
|---|---|
| Gives only necessary permissions | Gives access only when needed |
| Focuses on permission scope | Focuses on access duration |

## 3.3 Access Management

Access management includes:

1. Granting permissions.
2. Modifying permissions.
3. Revoking permissions.
4. Monitoring access.
5. Reviewing permissions regularly.

### Privilege Creep

Privilege creep occurs when employees accumulate unnecessary permissions over time.

Example:

An employee moves from Finance to HR but still retains old Finance permissions.

**Solution:** Conduct regular access reviews and remove unnecessary permissions.

## 3.4 Permission Types

| Permission | Meaning |
|---|---|
| Read | View files |
| Write | Modify files |
| Execute | Run programs |
| Delete | Remove files |
| Full Control | Complete control over the resource |

## 3.5 Privilege Escalation

Privilege escalation occurs when an attacker gains permissions beyond their authorized level.

**Vertical Privilege Escalation**

Moving from a lower privilege level to a higher one.

Example: Normal user → Administrator.

**Horizontal Privilege Escalation**

Accessing another user's resources at the same privilege level.

Example: User A accesses User B's private account information without permission.

---

# 4. Active Directory Fundamentals

## What is Active Directory?

Active Directory (AD) is a Microsoft directory service used to centrally manage users, computers, groups, resources, and access within an organization.

It is commonly used in Windows-based enterprise networks.

**Main purposes:**
- Centralized user management.
- Authentication.
- Access control.
- Group management.
- Policy enforcement.

## 4.1 Domain

A domain is a logical administrative boundary containing users, computers, groups, and other resources managed through Active Directory.

Example:

`company.local`

All domain-joined computers and users can be centrally managed.

## 4.2 Domain Controller (DC)

A Domain Controller is a server running Active Directory Domain Services (AD DS).

It provides centralized authentication and directory services.

**Responsibilities:**
- Verifies user credentials.
- Maintains directory information.
- Processes authentication requests.
- Supports policy and access management.

Example:

When an employee logs into a domain-joined computer, the computer can contact a Domain Controller to authenticate the user.

## 4.3 Organizational Unit (OU)

An Organizational Unit is a container used to organize Active Directory objects.

Example:

```text
             COMPANY DOMAIN
                   |
          ---------------------
          |         |         |
         HR        IT      FINANCE
          |         |         |
        Users     Users     Users
        PCs       PCs       PCs
```

**Uses:**
- Organize users and computers.
- Apply Group Policies.
- Delegate administrative responsibilities.

**Important:** An OU is used for organization and policy management. It is not itself a security group.

## 4.4 Group Policy Object (GPO)

A GPO is a collection of configuration and security settings that can be applied to users and computers in Active Directory.

Examples:
- Password policies.
- Account lockout policies.
- Screen lock settings.
- USB restrictions.
- Windows security configurations.

GPOs can be linked to:
- Sites.
- Domains.
- Organizational Units.

## 4.5 Security Groups

Security groups are collections of user or computer accounts used to assign permissions to resources.

Example:

Instead of giving 50 employees individual access to a shared folder, create an HR security group and assign permissions to that group.

### Important Differences

| Component | Purpose |
|---|---|
| OU | Organizes AD objects |
| Security Group | Assigns permissions to members |
| GPO | Applies configuration settings |
| Domain Controller | Provides AD authentication and directory services |
| Domain | Logical administrative boundary |

**Exam tip:** Do not confuse OU, Security Groups, and GPOs.

---

# 5. Active Directory Administration

Active Directory administration involves managing user accounts, groups, permissions, and security configurations.

## 5.1 User Management

Administrators perform the following tasks:

- Create user accounts.
- Modify user information.
- Reset passwords.
- Unlock accounts.
- Disable accounts.
- Delete accounts when appropriate.

### Employee Offboarding

When an employee leaves:

1. Disable the account.
2. Revoke sessions and access.
3. Remove unnecessary group memberships.
4. Preserve necessary audit evidence.
5. Transfer ownership of important resources.
6. Delete the account according to retention policies.

**Orphaned Account:** An account that remains active even though its owner has left the organization or no longer needs it.

Orphaned accounts create security risks.

## 5.2 Group Management

Active Directory supports different group types.

### Security Groups

Used to manage permissions and access to resources.

Example: HR employees receive access to HR documents through an HR security group.

### Distribution Groups

Used primarily for distributing email messages.

Example: Sending an announcement to all Finance employees.

| Security Group | Distribution Group |
|---|---|
| Used for access control | Used for email distribution |
| Can be assigned permissions | Not used to assign security permissions |

## 5.3 Access Control

Access control determines which users can access specific resources and what actions they can perform.

Example:

A company file server may provide:
- HR: Read and write access.
- Finance: Access only to Finance folders.
- Normal employees: Read-only access to shared documents.

Misconfigured permissions can expose confidential information.

## 5.4 Security Configuration

Important security configurations include:

- Strong password policies.
- Account lockout policies.
- Multi-factor authentication.
- Restricting administrator access.
- Disabling unnecessary guest accounts.
- Enabling security auditing.
- Regularly updating Domain Controllers.

### Account Lockout Policy

Account lockout temporarily locks an account after a specified number of failed login attempts.

**Purpose:** Reduce the effectiveness of brute-force and password-guessing attacks.

However, account lockout alone does not eliminate these attacks. It should be combined with MFA, rate limiting, and monitoring.

## 5.5 Administrative Best Practices

1. Follow least privilege.
2. Regularly review user permissions.
3. Disable inactive accounts.
4. Avoid using Domain Admin accounts for routine activities.
5. Use MFA for privileged accounts.
6. Monitor administrator activities.
7. Maintain backups.
8. Apply security updates.

---

# 6. Active Directory Security

Active Directory contains sensitive organizational identities and permissions. Attackers often target it to gain control over an organization's network.

## 6.1 Privileged Accounts

Privileged accounts have elevated permissions to manage systems and resources.

Examples:

| Account | Purpose |
|---|---|
| Domain Admin | Broad administrative control over a domain |
| Enterprise Admin | Forest-wide administrative privileges |
| Local Admin | Administrative control over an individual computer |
| Server Admin | Manages servers |
| Service Account | Used by applications and services |

**Why are privileged accounts important?**

If an attacker compromises a Domain Admin account, they may gain extensive control over the domain.

### Protection Methods

- Least privilege.
- MFA.
- Separate standard and administrative accounts.
- Privileged access monitoring.
- Just-in-Time access.
- Strong credential protection.

## 6.2 Credential Security

Credentials are information used to authenticate users and systems.

Examples:
- Passwords.
- Password hashes.
- Kerberos tickets.
- Authentication tokens.
- Certificates and keys.

### Credential Reuse

Credential reuse occurs when users use the same password across multiple accounts or services.

Example:

An employee uses the same password for a personal website and company account.

If the personal website is compromised, attackers may try the leaked credentials against the company account.

**Protection:**
- Use unique passwords.
- Use password managers.
- Enable MFA.
- Detect compromised credentials.
- Monitor suspicious login activity.

## 6.3 Kerberos Authentication

Kerberos is a ticket-based authentication protocol commonly used in Active Directory environments.

It allows users and services to authenticate through trusted tickets instead of repeatedly sending passwords to every service.

Simplified process:

```text
        USER
          |
          v
   Authentication
       Server
          |
          v
    Ticket Granting
       Ticket
          |
          v
   Ticket Granting
       Service
          |
          v
    Service Ticket
          |
          v
    Access Resource
```

**Important:** Kerberos is ticket-based authentication. It does not automatically eliminate weak passwords or credential reuse.

## 6.4 Account Monitoring

Administrators should monitor:

- Successful logins.
- Failed login attempts.
- Account creation.
- Account deletion.
- Password changes.
- Group membership changes.
- Privilege assignments.
- Suspicious administrator activity.

## 6.5 Important Windows Security Event IDs

These are particularly important for your exam.

| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Account logged off |
| 4648 | Logon attempted using explicit credentials |
| 4672 | Special privileges assigned to a new logon |
| 4720 | User account created |
| 4722 | User account enabled |
| 4726 | User account deleted |
| 4728 | Member added to security-enabled global group |
| 4732 | Member added to security-enabled local group |
| 4740 | Account locked out |
| 4663 | Object access attempt recorded when appropriate auditing is enabled |

### Important Example

If Event ID 4728 appears, it means someone added a member to a security-enabled global group.

If that group is Domain Admins, this could be a serious security event.

Investigate:
- Which account was added?
- Who performed the change?
- When did it happen?
- Was the change authorized?

## 6.6 Security Baselines

A security baseline is a predefined set of recommended minimum security configurations.

Examples:
- Strong password settings.
- Account lockout configuration.
- Restricted administrator access.
- Firewall configuration.
- Security auditing.

Group Policy can be used to enforce many baseline settings.

**Security Baseline vs Security Policy**

- Security policy: Broad organizational security requirements.
- Security baseline: Specific technical configurations used to implement security requirements.

## 6.7 Attack Surface

An attack surface is the collection of possible entry points an attacker could exploit to compromise a system or network.

Common Active Directory attack surface risks:

- Inactive accounts.
- Excessive permissions.
- Weak passwords.
- Unprotected privileged accounts.
- Misconfigured group policies.
- Outdated Domain Controllers.
- Exposed unnecessary services.

**Reducing the attack surface:**
- Disable inactive accounts.
- Enforce least privilege.
- Enable MFA.
- Patch systems.
- Remove unnecessary services.
- Monitor AD activities.
- Audit permissions regularly.

---

# 7. Identity Monitoring

## What is Identity Monitoring?

Identity monitoring is the continuous monitoring of user identities, login activities, authentication attempts, and access permissions to detect suspicious or unauthorized activities.

Its purpose is to identify identity-related threats before they cause serious damage.

## 7.1 Login Event Monitoring

Login events are records generated when users or systems attempt to authenticate.

Important information includes:

- Timestamp.
- Username.
- Source IP address.
- Computer or workstation.
- Event ID.
- Logon type.
- Authentication result.

### Important Login Event IDs

| Event ID | Meaning |
|---|---|
| 4624 | Successful login |
| 4625 | Failed login |
| 4634 | Logoff |
| 4648 | Explicit credentials used |
| 4740 | Account locked out |

## 7.2 Access Monitoring

Access monitoring tracks changes to permissions and access rights.

Examples:
- User added to an administrator group.
- User permissions modified.
- Sensitive files accessed.
- Privileged account used.
- New user account created.

Important Event IDs:

| Event ID | Meaning |
|---|---|
| 4720 | User account created |
| 4722 | User account enabled |
| 4726 | User account deleted |
| 4728 | Member added to global security group |
| 4732 | Member added to local security group |
| 4672 | Special privileges assigned |
| 4663 | Object access audited |

## 7.3 Suspicious Identity Activities

Security teams should watch for:

1. Repeated failed login attempts.
2. Successful login after many failed attempts.
3. Login from unusual locations.
4. Login at unusual times.
5. Password spraying.
6. Brute-force attempts.
7. Unexpected administrator access.
8. Unauthorized group membership changes.
9. Unusual resource access.
10. Suspicious privilege escalation.

### Example of a Suspicious Login

Imagine the following events:

```text
09:00 - Failed login - User: John
09:01 - Failed login - User: John
09:02 - Failed login - User: John
09:03 - Failed login - User: John
09:04 - Successful login - User: John
        Source IP: Unknown
```

This activity is suspicious.

Possible explanations:
- Brute-force attack.
- Password guessing.
- Compromised credentials.

The security team should investigate the source IP, login history, device information, and subsequent account activities.

## 7.4 SOC Investigation Workflow

A Security Operations Center (SOC) investigates suspicious activities and security incidents.

The typical investigation workflow:

```text
        1. DETECTION
             |
             v
          2. TRIAGE
             |
             v
        3. INVESTIGATION
             |
             v
         4. CONTAINMENT
             |
             v
          5. RECOVERY
             |
             v
       6. DOCUMENTATION
```

### 1. Detection

Identify suspicious activities through alerts and monitoring systems.

Example: Multiple failed login attempts.

### 2. Triage

Determine the severity and priority of the alert.

Example: Is this a normal login mistake or a possible attack?

### 3. Investigation

Collect and analyze evidence.

Check:
- Source IP.
- Timestamps.
- User activity.
- Login history.
- Group membership changes.
- Access logs.

### 4. Containment

Limit the impact of the incident.

Possible actions:
- Disable or restrict compromised accounts.
- Revoke active sessions and tokens.
- Reset credentials.
- Isolate affected devices when necessary.

Preserve relevant evidence before making changes whenever possible.

### 5. Recovery

Restore normal operations and ensure systems are secure.

### 6. Documentation

Prepare an incident report containing:

- Incident summary.
- Affected accounts.
- Evidence.
- Timeline.
- Actions taken.
- Recommendations.

---

# 8. AI-Assisted Identity Security

AI can assist cybersecurity professionals in analyzing identity-related security activities.

It can help process large amounts of information and identify suspicious patterns.

## Important Applications

### 1. Authentication Analysis

AI can analyze login activities and identify unusual patterns.

Example: Detecting a login from an unusual location.

### 2. Access Reviews

AI can help identify excessive permissions and unnecessary access.

Example: Finding employees who still have administrator privileges even though they no longer need them.

### 3. Identity Security Assessments

AI can assist in identifying:

- Weak configurations.
- Missing MFA.
- Excessive privileges.
- Inactive accounts.
- Identity-related risks.

### 4. Investigation Support

AI can help:
- Summarize authentication logs.
- Identify suspicious events.
- Build incident timelines.
- Explain possible attack patterns.

### 5. Validate AI Outputs

AI recommendations must always be verified by security professionals.

**Important limitations:**
- False positives.
- False negatives.
- Incorrect recommendations.
- Exposure of sensitive identity information.

**Exam point:** AI assists security professionals; it should not replace human verification and decision-making.

---

# 9. Hands-on Tasks

This topic focuses on applying IAM and Active Directory knowledge in practical situations.

## 9.1 Analyze Authentication Logs

Analyze login events to identify suspicious activities.

Example:

| Event ID | Activity | Interpretation |
|---|---|---|
| 4625 | Failed login | Possible authentication failure or attack |
| 4624 | Successful login | Authentication succeeded |
| 4740 | Account locked | Account reached lockout threshold |

Tasks:
- Identify repeated failed logins.
- Find successful logins.
- Check unusual source IP addresses.
- Investigate login times.

## 9.2 Review Access Control Scenarios

Examine user permissions and identify excessive access.

Example:

An HR employee has full administrator access to the company's servers.

This violates the Principle of Least Privilege.

Recommendation: Remove unnecessary administrator privileges and provide only the permissions required for HR responsibilities.

## 9.3 Investigate Suspicious Login Activities

Example:

An employee account has:
- 25 failed login attempts.
- One successful login.
- An unfamiliar source IP.
- Login activity outside normal working hours.

Possible security concern: Brute-force or password-guessing attack followed by possible account compromise.

Investigation:
1. Check the source IP.
2. Review login timestamps.
3. Check the affected account's activities.
4. Identify suspicious privilege changes.
5. Restrict the account if compromise is suspected.
6. Revoke sessions and reset credentials as appropriate.

## 9.4 Assess Identity Security Risks

Common identity risks:

| Risk | Security Problem |
|---|---|
| Weak passwords | Easier password attacks |
| Inactive accounts | Unauthorized access opportunities |
| Excessive permissions | Increased damage if compromised |
| Missing MFA | Weaker authentication security |
| Unprotected admin accounts | Privileged account compromise |
| Poor monitoring | Attacks may go undetected |

## 9.5 Create an IAM Security Report

A basic identity security report should contain:

1. **Incident Summary:** What happened?
2. **Affected Accounts:** Which accounts were involved?
3. **Evidence:** Event IDs, IP addresses, timestamps, logs.
4. **Risk Assessment:** What is the potential impact?
5. **Actions Taken:** What steps were performed?
6. **Recommendations:** How can similar incidents be prevented?

---

# 🔥 Final Rapid Revision — Important Exam Points

Read this section at least twice before your exam.

| Topic | Remember |
|---|---|
| IAM | Managing identities and access |
| Identification | Who are you? |
| Authentication | Prove who you are |
| Authorization | What can you access? |
| Accountability | Track user activities |
| Provisioning | Create accounts and assign access |
| Modification | Update permissions |
| Deprovisioning | Disable accounts and revoke access |
| Password Hashing | Protect stored passwords |
| Salting | Adds unique random data before hashing |
| HTTPS/TLS | Protects passwords during transmission |
| MFA | Two or more different authentication factors |
| SSO | One authentication for multiple applications |
| Federation | Trust between identity systems |
| OAuth 2.0 | Delegated authorization |
| OIDC | Authentication built on OAuth 2.0 |
| SAML | XML-based identity assertions |
| RBAC | Permissions assigned through roles |
| Least Privilege | Minimum necessary permissions |
| JEA | Just Enough Access |
| JIT | Just-in-Time Access |
| Privilege Creep | Accumulation of unnecessary permissions |
| Vertical Escalation | Lower privilege to higher privilege |
| Horizontal Escalation | Access another user's resources |
| Active Directory | Microsoft's directory service |
| Domain | Logical administrative boundary |
| Domain Controller | Provides AD authentication and directory services |
| OU | Organizes AD objects |
| GPO | Applies configuration policies |
| Security Group | Manages resource permissions |
| Distribution Group | Email distribution |
| Kerberos | Ticket-based authentication |
| Security Baseline | Recommended security configurations |
| Attack Surface | Possible entry points for attackers |
| Identity Monitoring | Monitors identity and access activities |
| SOC | Security Operations Center |

## 🚨 Windows Event IDs — Memorize These

| Event ID | Meaning |
|---|---|
| **4624** | Successful logon |
| **4625** | Failed logon |
| **4634** | Logoff |
| **4648** | Explicit credentials used |
| **4672** | Special privileges assigned |
| **4720** | User created |
| **4722** | User enabled |
| **4726** | User deleted |
| **4728** | Added to global security group |
| **4732** | Added to local security group |
| **4740** | Account locked out |
| **4663** | Object access audited |

### SOC Investigation — Memorize the Order

**Detection → Triage → Investigation → Containment → Recovery → Documentation**

---

# 🎯 Final Exam Preparation Checklist

Before entering your exam, make sure you can explain these without looking at the notes:

- [ ] Difference between identification, authentication, and authorization.
- [ ] Three stages of the IAM identity lifecycle.
- [ ] Password hashing, salting, and HTTPS.
- [ ] Brute force, dictionary attack, credential stuffing, and password spraying.
- [ ] MFA, SSO, and federation.
- [ ] Difference between OAuth 2.0, OIDC, and SAML.
- [ ] RBAC and Principle of Least Privilege.
- [ ] JEA and JIT.
- [ ] Active Directory, Domain, and Domain Controller.
- [ ] Difference between OU, Security Group, and GPO.
- [ ] User and group administration.
- [ ] Privileged accounts and Kerberos.
- [ ] Windows Event IDs.
- [ ] Identity monitoring and suspicious login detection.
- [ ] SOC investigation workflow.
- [ ] AI applications in identity security.
- [ ] IAM security reports.


You've already studied these concepts. This is a revision, not starting from zero. Good luck with your exam!
