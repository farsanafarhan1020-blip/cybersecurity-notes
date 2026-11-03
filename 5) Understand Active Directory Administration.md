# 🛡️ Understanding Active Directory Administration

> **Active Directory Administration is the process of managing users, groups, permissions, security settings, and domain resources within an Active Directory environment.**

In an enterprise network, administrators must ensure that employees can access the resources required for their jobs while preventing unauthorized access.

Active Directory provides centralized tools to manage these responsibilities.

For cybersecurity professionals, understanding Active Directory administration is important for both defensive security and penetration testing. Many enterprise security incidents involve compromised accounts, excessive permissions, weak configurations, or poor administrative practices.

---

## 1. 👤 User Management

### What Is User Management?

User management is the process of creating, configuring, maintaining, and removing user accounts in an Active Directory domain.

Every employee who needs access to domain resources generally has a domain user account.

Administrators use these accounts to control identity, authentication, and access.

### What Is an Active Directory User Account?

A user account represents an individual or service identity in Active Directory.

It contains attributes such as:

| Attribute | Description |
|---|---|
| Username | Name used to identify the account |
| Password | Credential used for authentication |
| Display Name | User's visible name |
| Email | User's email address |
| Department | Department to which the user belongs |
| Description | Additional account information |
| Group Membership | Groups to which the user belongs |
| Account Status | Whether the account is enabled or disabled |

Example:

```text
Username: alice
Display Name: Alice Johnson
Department: Security
Email: alice@company.com
Account Status: Enabled
```

### Creating a User Account

Administrators can create users using:

- Active Directory Users and Computers (ADUC).
- Active Directory Administrative Center.
- PowerShell.
- Automation scripts.

**Using the graphical interface:**

1. Open Active Directory Users and Computers.
2. Navigate to the required Organizational Unit.
3. Right-click the OU.
4. Select New → User.
5. Enter the user's details.
6. Set an initial password.
7. Configure account options.
8. Finish creating the account.

**Using PowerShell:**

```powershell
New-ADUser `
  -Name "Alice Johnson" `
  -SamAccountName "alice" `
  -UserPrincipalName "alice@company.com" `
  -Path "OU=Security,DC=company,DC=com" `
  -Enabled $true `
  -AccountPassword (Read-Host "Enter Password" -AsSecureString)
```

This creates an enabled user account in the specified OU. In a real environment, ensure that the password meets the organization's password policy.

### Viewing User Accounts

```powershell
Get-ADUser -Filter *
```

Displays user accounts visible to the executing administrator.

To retrieve a specific user:

```powershell
Get-ADUser -Identity alice
```

To retrieve additional attributes:

```powershell
Get-ADUser -Identity alice -Properties *
```

### Modifying User Accounts

Administrators regularly update user information.

For example, changing a user's department:

```powershell
Set-ADUser -Identity alice -Department "IT"
```

Changing the user's display name:

```powershell
Set-ADUser -Identity alice -DisplayName "Alice J. Johnson"
```

### Resetting a Password

When an employee forgets a password, an administrator may reset it.

```powershell
Set-ADAccountPassword -Identity alice -Reset `
  -NewPassword (Read-Host "New Password" -AsSecureString)
```

An administrator may also require the user to change the password at the next login:

```powershell
Set-ADUser -Identity alice -ChangePasswordAtLogon $true
```

A password reset is different from retrieving the user's existing password. Secure directory systems do not provide administrators with users' original plaintext passwords.

### Disabling and Enabling Accounts

When an employee leaves an organization or an account is suspected of compromise, administrators may disable it.

Disable:

```powershell
Disable-ADAccount -Identity alice
```

Enable:

```powershell
Enable-ADAccount -Identity alice
```

Disabling an account prevents normal account authentication, but it does not automatically terminate every existing session or revoke every previously issued ticket and token.

For an incident response, administrators should also investigate active sessions, revoke relevant credentials or tickets where possible, and review the account's access.

### Deleting User Accounts

```powershell
Remove-ADUser -Identity alice
```

Deleting an account should follow an approved process.

In many organizations, disabling and retaining an account temporarily is safer than immediately deleting it because the organization may need its information for auditing, investigation, or recovery.

### User Account Lifecycle

A typical account lifecycle looks like this:

```text
       Employee Joins
              |
              v
        Create Account
              |
              v
       Assign Group Roles
              |
              v
       Configure Permissions
              |
              v
        Normal Usage
              |
              v
      Role / Department Change
              |
              v
       Review Permissions
              |
              v
        Employee Leaves
              |
              v
       Disable Account
              |
              v
      Retention / Investigation
              |
              v
       Approved Deletion
```

### Important User Account Types

| Account Type | Purpose |
|---|---|
| Standard User | Normal employee activities |
| Administrator | Performs approved administrative operations |
| Service Account | Used by applications or services |
| Guest Account | Provides limited access where required |
| Managed Service Account | Provides managed credentials for supported services |

**Important:** Service accounts should not be treated like ordinary employee accounts. They require controlled permissions, credential management, and monitoring. Group Managed Service Accounts (gMSAs) can help automate password management for supported services.

### User Management Security Best Practices

- Follow least privilege.
- Use unique accounts for each person.
- Disable accounts when no longer required.
- Review inactive accounts.
- Use strong authentication and MFA where supported.
- Avoid shared administrator accounts.
- Protect service-account credentials.
- Monitor password resets and account changes.
- Review account permissions when employees change departments.

---

## 2. 👥 Group Management

### What Is Group Management?

Group management is the process of creating, organizing, maintaining, and controlling groups of users or other supported directory objects.

Instead of assigning permissions to each employee individually, administrators can assign users to groups and grant permissions to those groups.

This simplifies access management.

### Real-World Example

Imagine a company has 100 employees.

The security department has 15 employees who need access to security monitoring tools.

Instead of granting access individually to all 15 employees, an administrator creates a group:

```text
Security-Analysts
```

The employees are added to this group, and the group receives the required permissions.

```text
             Active Directory
                     |
                     v
             Security-Analysts
                     |
          +----------+----------+
          |          |          |
          v          v          v
        Alice       Bob       Charlie
          |          |          |
          +----------+----------+
                     |
                     v
            Security Resources
```

If a new employee joins the security team, the administrator simply adds that user to the group.

### Types of Active Directory Groups

Active Directory supports two main group types:

#### 1. Security Groups

Security groups are used to assign permissions to resources.

Examples:

- File server access.
- Application access.
- Administrative permissions.
- Network resource permissions.

Example:

```text
Group: Finance-Users
Members:
- Alice
- Bob
- Charlie

Permission:
Read access to Finance shared folder
```

#### 2. Distribution Groups

Distribution groups are primarily used for email distribution.

They are not used to assign security permissions.

Example:

```text
Group: All-Employees

Members:
- HR
- IT
- Finance
- Development

Purpose:
Send company announcements
```

### Security Groups vs Distribution Groups

| Security Group | Distribution Group |
|---|---|
| Used for access control | Used for email distribution |
| Can be assigned permissions | Cannot be used as a security principal for permissions |
| Supports security-related authorization | Supports communication and mailing lists |

### Group Scopes

Active Directory security and distribution groups can have different scopes.

| Scope | Description |
|---|---|
| Domain Local | Used to assign permissions to resources within its domain |
| Global | Commonly groups users and other groups from its own domain |
| Universal | Can contain members from multiple domains within the same forest and be used across the forest |

A simplified comparison:

| Group Scope | Typical Membership | Permission Use |
|---|---|---|
| Global | Accounts and groups from its own domain | Can be added to domain-local groups in relevant domains |
| Domain Local | Accounts and groups from trusted domains, depending on scope rules | Assigns permissions to resources in its own domain |
| Universal | Accounts and groups from domains in the same forest | Can be used across the forest |

### AGDLP Model

AGDLP is a common Microsoft group nesting strategy for managing access.

It stands for:

- **A** — Accounts
- **G** — Global Groups
- **DL** — Domain Local Groups
- **P** — Permissions

Example:

```text
          User Accounts
                |
                v
         Global Group
       (Finance-Users)
                |
                v
       Domain Local Group
       (Finance-Share-Read)
                |
                v
          Permissions
                |
                v
       Finance Shared Folder
```

This approach separates user membership from resource permissions.

It makes administration easier when employees change roles or when access requirements change.

### Creating a Security Group

Using PowerShell:

```powershell
New-ADGroup `
  -Name "Security-Analysts" `
  -SamAccountName "Security-Analysts" `
  -GroupScope Global `
  -GroupCategory Security `
  -Path "OU=Security,DC=company,DC=com"
```

### Adding a User to a Group

```powershell
Add-ADGroupMember `
  -Identity "Security-Analysts" `
  -Members alice
```

### Viewing Group Members

```powershell
Get-ADGroupMember -Identity "Security-Analysts"
```

### Viewing a User's Group Membership

```powershell
Get-ADPrincipalGroupMembership -Identity alice
```

### Removing a User from a Group

```powershell
Remove-ADGroupMember `
  -Identity "Security-Analysts" `
  -Members alice `
  -Confirm:$false
```

Use confirmation or an approved change process in production environments.

### Important Privileged Groups

Some Active Directory groups have highly privileged permissions.

Examples include:

| Group | Security Significance |
|---|---|
| Domain Admins | Highly privileged administration within a domain |
| Enterprise Admins | Highly privileged forest-wide administration |
| Schema Admins | Can modify the Active Directory schema |
| Administrators | Built-in administrative group with powerful permissions |
| Account Operators | Delegated account-management capabilities, with important restrictions |
| Backup Operators | Backup-related privileges that can expose sensitive data if misused |

**Important:** Group membership and effective permissions must be reviewed carefully. A user can gain significant privileges through nested group membership and delegated permissions, even if they are not directly listed in a highly privileged group.

### Group Management Security Best Practices

- Use groups instead of assigning permissions individually.
- Follow AGDLP or an appropriate group nesting strategy.
- Avoid unnecessary privileged group membership.
- Review nested group membership.
- Remove users from groups when their roles change.
- Monitor privileged group modifications.
- Document the purpose and owner of each important group.

---

## 3. 🔐 Access Control

### What Is Access Control?

Access control is the process of determining who can access a resource and what actions they are allowed to perform.

In Active Directory environments, access control is commonly implemented using security groups, security identifiers (SIDs), access control lists, and permissions.

For example:

An employee may be allowed to read a shared folder but not modify or delete its files.

### Three Important Access Control Concepts

| Concept | Meaning |
|---|---|
| Authentication | Verifies who the user is |
| Authorization | Determines what the authenticated user can do |
| Auditing | Records and monitors access-related activity |

Remember:

- Authentication answers: Who are you?
- Authorization answers: What are you allowed to do?
- Auditing answers: What happened?

### Access Control Lists (ACLs)

An Access Control List is a collection of permission entries associated with a securable object.

In Windows, there are two important types:

**1. Discretionary Access Control List (DACL)**

A DACL specifies which users or groups are allowed or denied access to an object.

**2. System Access Control List (SACL)**

A SACL defines which access attempts should be audited.

### Understanding DACL and SACL

| DACL | SACL |
|---|---|
| Controls access permissions | Controls auditing of access attempts |
| Contains access control entries | Contains audit control entries |
| Determines allowed or denied access | Determines which activities generate audit events |

### Access Control Entry (ACE)

An ACE is an individual entry within an ACL.

It can specify:

- A user or group SID.
- Allowed or denied permissions.
- Audit settings, where applicable.
- Inheritance information.

### Example: File Server Permissions

Imagine a company has a shared folder:

```text
D:\Company\Finance
```

The administrator configures permissions:

| Group | Permission |
|---|---|
| Finance-Users | Read |
| Finance-Managers | Modify |
| IT-Administrators | Full Control |
| Other Employees | No access |

The permissions determine what members of each group can do.

### Common Windows Permissions

| Permission | Description |
|---|---|
| Read | View files and folders |
| Write | Create files and write data |
| Modify | Read, write, and delete files and folders |
| Read & Execute | Read files and execute supported programs |
| Full Control | Includes changing permissions and taking ownership, along with other rights |

Actual effective access depends on the complete permission configuration, inheritance, group memberships, and other security controls.

### NTFS Permissions vs Share Permissions

Windows file servers can apply both NTFS permissions and SMB share permissions.

| NTFS Permissions | Share Permissions |
|---|---|
| Apply to files and folders on NTFS volumes | Apply when accessing resources through an SMB share |
| Work with local and network access | Apply to network access through the share |
| Offer detailed permission control | Provide permissions at the share level |

For network access, both sets of permissions apply. The effective access is generally the most restrictive combination of the applicable share and NTFS permissions.

Example:

```text
Share Permission: Read
NTFS Permission: Modify

Effective Network Access: Read
```

### Permission Inheritance

Windows permissions can be inherited from parent folders.

Example:

```text
Company
   |
   +-- Finance
   |      |
   |      +-- Reports
   |
   +-- HR
          |
          +-- Employee Records
```

If permissions are inherited, a child folder may receive applicable permissions from its parent.

Administrators can configure inheritance when specific folders require different access rules.

### Access Control Security Risks

**1. Excessive Permissions**

Users receive more access than their job requires.

**2. Broken Access Control**

An application or system fails to enforce the intended access restrictions.

**3. Privilege Escalation**

An attacker obtains permissions beyond their authorized level.

**4. Insecure Group Membership**

An ordinary user is accidentally added to a privileged group.

**5. Permission Inheritance Misconfiguration**

Sensitive folders inherit permissions that expose their contents to unauthorized users.

### Access Control Best Practices

- Follow least privilege.
- Use security groups to assign permissions.
- Review effective permissions regularly.
- Avoid unnecessary Full Control permissions.
- Separate administrative and standard accounts.
- Monitor changes to ACLs and privileged group memberships.
- Use appropriate auditing for sensitive resources.
- Review inherited permissions on critical folders.

---

## 4. 🛡️ Security Configuration

### What Is Active Directory Security Configuration?

Security configuration is the process of establishing and maintaining secure settings across Active Directory users, computers, domain controllers, and other domain resources.

The objective is to reduce the attack surface and protect the organization's identity infrastructure.

Active Directory security depends on multiple layers, including:

- Account security.
- Authentication security.
- Group Policy.
- Privilege management.
- Domain controller protection.
- Logging and monitoring.
- Network security.

### 4.1 Password Policy

Password policies define requirements for passwords and account security.

Common settings include:

| Setting | Purpose |
|---|---|
| Minimum Password Length | Defines the minimum number of characters |
| Password Complexity | Controls password composition requirements |
| Password History | Prevents reuse of recent passwords |
| Maximum Password Age | Controls when password changes are required |
| Minimum Password Age | Controls how frequently a password can be changed |

Modern password security should emphasize long, unique passwords, secure password storage, and MFA rather than relying only on frequent password changes or arbitrary complexity rules.

### 4.2 Account Lockout Policy

Account lockout policies help reduce password-guessing attacks.

Important settings include:

- Account lockout threshold.
- Account lockout duration.
- Reset account lockout counter after.

Example:

```text
Account Lockout Threshold: 5 attempts
Account Lockout Duration: 15 minutes
Reset Counter After: 15 minutes
```

These are illustrative values, not universal recommendations. Lockout policies should be designed carefully because aggressive settings can allow attackers to cause denial of service by deliberately locking users out.

### 4.3 Kerberos Security

Kerberos is the primary authentication protocol in modern Active Directory domains.

Security configuration should include:

- Protecting the KRBTGT account.
- Using strong cryptographic settings.
- Monitoring unusual ticket activity.
- Protecting service-account credentials.
- Avoiding unnecessary legacy encryption.
- Maintaining secure time synchronization.

The KRBTGT account is especially sensitive because its secret is used by the domain's Kerberos Key Distribution Center.

### 4.4 User Rights Assignment

User Rights Assignment determines which users and groups can perform certain system-level actions.

Examples:

- Log on locally.
- Log on through Remote Desktop Services.
- Access this computer from the network.
- Shut down the system.
- Back up files and directories.

Administrators should restrict powerful rights to accounts that genuinely require them.

### 4.5 Windows Firewall Configuration

Windows Defender Firewall helps control network traffic entering and leaving Windows systems.

Group Policy can centrally configure firewall settings.

Examples:

- Block unnecessary inbound connections.
- Allow only required management traffic.
- Restrict access to sensitive services.
- Enable firewall logging where appropriate.

### 4.6 Audit Policy

Audit policies determine which security activities Windows records.

Important events include:

- Successful and failed logons.
- Account creation and deletion.
- Password resets.
- Group membership changes.
- Privilege use.
- Policy changes.
- Directory service modifications.

Audit configuration helps security teams investigate suspicious activity.

### 4.7 Restricting Administrative Privileges

Administrative accounts have powerful permissions.

A compromised administrator account can allow an attacker to modify important system settings or access sensitive information.

Security measures include:

- Separate standard and administrative accounts.
- Restrict Domain Admin membership.
- Use dedicated privileged workstations where appropriate.
- Apply Just-in-Time (JIT) administrative access where supported.
- Use MFA for privileged access.
- Monitor administrative logons.

### 4.8 Group Policy Security Configuration

Group Policy can enforce security configurations across domain-joined computers.

Examples:

| Policy Area | Security Purpose |
|---|---|
| Password Policy | Enforces password requirements |
| Account Lockout | Helps limit password guessing |
| Windows Firewall | Controls network traffic |
| Audit Policy | Records security activity |
| User Rights Assignment | Restricts powerful system privileges |
| Windows Defender | Configures endpoint protection |
| Application Control | Restricts unauthorized software execution |

### Security Configuration Checklist

- [ ] Enforce appropriate password and authentication policies.
- [ ] Enable MFA for privileged access where supported.
- [ ] Restrict Domain Admin membership.
- [ ] Configure Windows Firewall.
- [ ] Enable relevant security auditing.
- [ ] Secure domain controllers.
- [ ] Review service accounts.
- [ ] Disable unnecessary accounts and services.
- [ ] Monitor privileged group changes.
- [ ] Regularly review Group Policy configurations.

---

## 5. 🧑‍💻 Administrative Practices

### What Are Administrative Practices?

Administrative practices are the procedures and responsibilities administrators follow to operate Active Directory securely, reliably, and consistently.

Good administration is not just about creating accounts and assigning permissions. It also includes change management, documentation, monitoring, auditing, backup, and incident response.

### 5.1 Principle of Least Privilege

The Principle of Least Privilege (PoLP) means giving users and administrators only the permissions necessary to perform their responsibilities.

Example:

A developer needs access to development resources.

They do not automatically need:

- Domain Admin privileges.
- Access to HR records.
- Access to finance systems.
- Permission to modify domain security policies.

Granting unnecessary permissions increases security risk.

### 5.2 Role-Based Administration

Administrators should receive permissions based on their responsibilities.

Example:

| Administrative Role | Responsibility |
|---|---|
| Help Desk Administrator | Password resets and basic account support |
| User Administrator | Approved user account management |
| Group Administrator | Manages specified groups |
| Server Administrator | Manages approved servers |
| Domain Administrator | Performs high-level domain administration |

The exact permissions should be delegated rather than assuming that every person with an administrative job title requires broad privileges.

### 5.3 Separate Administrative Accounts

Administrators should maintain separate accounts for normal and privileged activities.

Example:

```text
Normal Account:
alice

Administrative Account:
alice-admin
```

The normal account is used for everyday work.

The administrative account is used only for approved administrative tasks.

This reduces the chance that everyday browsing, email, or application activity exposes privileged credentials.

### 5.4 Account Lifecycle Management

Organizations should establish clear procedures for:

- New employee onboarding.
- Role and department changes.
- Temporary access.
- Employee termination.
- Contractor account expiration.
- Inactive account review.

This ensures that access is granted and removed at the correct time.

### 5.5 Change Management

Changes to Active Directory can affect many users and systems.

Examples:

- Modifying a Group Policy.
- Changing domain security settings.
- Adding a user to a privileged group.
- Changing permissions on shared folders.
- Modifying domain controller configurations.

A secure change-management process should include:

1. Document the requested change.
2. Identify security and operational risks.
3. Obtain approval.
4. Test in a controlled environment.
5. Implement the change.
6. Verify the result.
7. Document the final configuration.

### 5.6 Logging and Monitoring

Administrators and security teams should monitor important Active Directory events.

Examples:

| Event ID | Activity |
|---|---|
| 4720 | User account created |
| 4722 | User account enabled |
| 4723 | Attempt to change an account password |
| 4724 | Attempt to reset an account password |
| 4725 | User account disabled |
| 4726 | User account deleted |
| 4728 | Member added to a security-enabled global group |
| 4732 | Member added to a security-enabled local group |
| 4756 | Member added to a security-enabled universal group |
| 4740 | User account locked out |
| 4739 | Domain policy changed |

These are Windows Security log event IDs. Whether they are recorded depends on the relevant audit policy and system configuration.

Administrators should investigate suspicious events rather than treating every event as malicious.

### 5.7 Backup and Recovery

Active Directory is critical infrastructure. Organizations need reliable backup and recovery procedures.

Important practices include:

- Regular System State backups of domain controllers.
- Secure backup storage.
- Offline or immutable backup copies where possible.
- Restricted backup access.
- Regular recovery testing.
- Documented disaster recovery procedures.

Backups should be protected because attackers may attempt to compromise or delete them during an intrusion.

### 5.8 Documentation

Administrators should document:

- Domain structure.
- Domain controllers.
- Organizational Units.
- Important security groups.
- Group Policy Objects.
- Privileged accounts.
- Delegated permissions.
- Administrative procedures.
- Backup and recovery processes.

Documentation makes troubleshooting, auditing, and incident response more effective.

### 5.9 Periodic Access Reviews

Access should not be granted permanently without review.

Administrators should regularly check:

- Inactive accounts.
- Privileged users.
- Group memberships.
- Service accounts.
- Shared folder permissions.
- Delegated administrative permissions.
- Accounts with unnecessary access.

### Administrative Best Practices Summary

| Practice | Security Benefit |
|---|---|
| Least Privilege | Reduces unnecessary access |
| Separate Admin Accounts | Protects privileged credentials |
| Regular Access Reviews | Removes excessive permissions |
| Change Management | Reduces configuration mistakes |
| Logging and Monitoring | Improves threat detection |
| Backup and Recovery | Supports business continuity |
| Documentation | Improves consistency and investigation |
| MFA | Reduces the impact of stolen passwords |
| Account Lifecycle Management | Prevents abandoned accounts from remaining active |

---

## 6. 🔗 How Active Directory Administration Works Together

Let's understand how these five areas work together in a real company.

Imagine a company called TechSecure.

A new employee, Alice, joins the Security Department.

### Step 1: User Management

The administrator creates Alice's domain account.

```text
Username: alice
Department: Security
Status: Enabled
```

### Step 2: Group Management

Alice is added to the appropriate security group.

```text
Security-Analysts
```

### Step 3: Access Control

The security group receives access to the required resources.

```text
Security-Analysts
        |
        v
Security Monitoring Folder
        |
        v
Read / Modify Permissions
```

### Step 4: Security Configuration

Alice's computer receives appropriate security policies.

Examples:

- Windows Firewall configuration.
- Endpoint protection settings.
- Audit settings.
- Device restrictions.

### Step 5: Administrative Practices

The administrator documents the account creation, verifies permissions, and ensures that access follows company policy.

```text
            New Employee
                  |
                  v
            Create Account
                  |
                  v
            Assign Groups
                  |
                  v
          Configure Permissions
                  |
                  v
           Apply Security Policies
                  |
                  v
            Verify and Audit
                  |
                  v
          Monitor Account Activity
```

This is the basic administrative workflow of an Active Directory environment.

---

## 7. ⚔️ Active Directory Administration from a Cybersecurity Perspective

Poor Active Directory administration can create opportunities for attackers.

Understanding how administrative mistakes become security vulnerabilities is essential for security professionals.

### Common Administrative Security Weaknesses

| Weakness | Potential Impact |
|---|---|
| Excessive privileges | Unauthorized administrative access |
| Weak service-account passwords | Credential compromise |
| Unmonitored privileged groups | Undetected privilege escalation |
| Inactive accounts | Potential unauthorized access |
| Poor Group Policy configuration | Weakened domain security |
| Unrestricted delegation | Excessive control over directory objects |
| Shared administrator accounts | Reduced accountability |
| Inadequate logging | Difficult incident investigation |
| Unprotected backups | Directory compromise or data exposure |

### Example: Privilege Escalation Through Group Management

Imagine an ordinary employee account is accidentally added to a highly privileged group.

```text
        Ordinary User
              |
              v
        Group Membership
              |
              v
          Domain Admins
              |
              v
      Extensive Domain Privileges
```

The user may now possess powerful administrative capabilities.

A security team should detect and investigate unexpected changes to privileged group membership.

### Defensive Monitoring Priorities

Pay particular attention to:

1. Unexpected additions to privileged groups.
2. Creation of suspicious accounts.
3. Unexpected password resets.
4. Changes to sensitive Group Policy Objects.
5. Unusual administrative logons.
6. Disabled security controls.
7. Suspicious directory replication activity.
8. Unusual service-account authentication.

### Blue Team vs Red Team Perspective

| Blue Team | Red Team |
|---|---|
| Reviews user and group permissions | Identifies excessive permissions |
| Monitors privileged group changes | Tests privilege escalation paths |
| Audits account activity | Evaluates account security |
| Secures service accounts | Assesses service-account weaknesses |
| Reviews Group Policies | Identifies insecure policy configurations |
| Protects domain controllers | Tests domain controller security under authorization |

Both perspectives help improve enterprise security.

---

## 8. 🧪 Hands-on Lab: Active Directory Administration

You can practice these administration tasks in your own isolated Windows Server lab.

### Recommended Lab Setup

| Machine | Purpose |
|---|---|
| Windows Server VM | Active Directory Domain Controller |
| Windows 10/11 VM | Domain-joined client |
| Kali Linux VM | Security assessment workstation |

Use a private virtual network and test accounts. Do not perform these activities on an organization’s domain without explicit authorization.

### Lab 1: Create a User

Create a user named `student01`.

```powershell
New-ADUser `
  -Name "Student One" `
  -SamAccountName "student01" `
  -UserPrincipalName "student01@lab.example.com" `
  -Path "OU=Security,DC=lab,DC=example,DC=com" `
  -Enabled $true `
  -AccountPassword (Read-Host "Password" -AsSecureString)
```

Verify:

```powershell
Get-ADUser -Identity student01
```

### Lab 2: Create a Security Group

```powershell
New-ADGroup `
  -Name "Lab-Security-Team" `
  -SamAccountName "Lab-Security-Team" `
  -GroupScope Global `
  -GroupCategory Security `
  -Path "OU=Security,DC=lab,DC=example,DC=com"
```

Verify:

```powershell
Get-ADGroup -Identity "Lab-Security-Team"
```

### Lab 3: Add a User to the Group

```powershell
Add-ADGroupMember `
  -Identity "Lab-Security-Team" `
  -Members student01
```

Verify:

```powershell
Get-ADGroupMember -Identity "Lab-Security-Team"
```

### Lab 4: Review User Group Membership

```powershell
Get-ADPrincipalGroupMembership -Identity student01
```

Identify the groups to which the account belongs.

### Lab 5: Disable and Re-enable a User

Disable:

```powershell
Disable-ADAccount -Identity student01
```

Check status:

```powershell
Get-ADUser student01 -Properties Enabled
```

Enable:

```powershell
Enable-ADAccount -Identity student01
```

### Lab 6: Explore File Permissions

Create a test folder:

```powershell
New-Item -Path "C:\Lab\Finance" -ItemType Directory
```

Inspect its access control list:

```powershell
Get-Acl "C:\Lab\Finance" | Format-List
```

Explore the permissions assigned to different users and groups.

### Lab 7: Review Security Events

Open Event Viewer:

```powershell
eventvwr.msc
```

Navigate to:

```text
Windows Logs → Security
```

Search for events such as:

- 4720 — User created.
- 4724 — Password reset attempted.
- 4725 — User disabled.
- 4728 — Member added to global security group.
- 4740 — Account locked out.

The events will appear only when the appropriate auditing is configured and the activity occurs.

### Lab 8: Review Privileged Group Membership

Run:

```powershell
Get-ADGroupMember -Identity "Domain Admins"
```

Review the members of the Domain Admins group.

**Your task:** Identify whether any ordinary test account has unnecessary administrative privileges. Do not modify production privileged groups during this exercise.

---

## 9. ⚡ Quick Revision

### Important Concepts

| Topic | Remember |
|---|---|
| User Management | Creates and maintains domain accounts |
| Group Management | Organizes users and manages access through groups |
| Security Group | Used to assign permissions |
| Distribution Group | Used primarily for email distribution |
| AGDLP | Accounts → Global → Domain Local → Permissions |
| Access Control | Determines who can access resources and perform actions |
| DACL | Controls allowed and denied access |
| SACL | Defines which access attempts are audited |
| NTFS Permissions | Control access to files and folders |
| Share Permissions | Control access through SMB shares |
| Security Configuration | Establishes secure domain settings |
| Least Privilege | Gives only necessary permissions |
| Change Management | Controls and documents administrative changes |
| Auditing | Records security-related activities |

### Important PowerShell Commands

| Command | Purpose |
|---|---|
| `New-ADUser` | Creates a user |
| `Get-ADUser` | Retrieves user information |
| `Set-ADUser` | Modifies user attributes |
| `Set-ADAccountPassword` | Changes or resets a password |
| `Disable-ADAccount` | Disables an account |
| `Enable-ADAccount` | Enables an account |
| `Remove-ADUser` | Deletes a user |
| `New-ADGroup` | Creates a group |
| `Get-ADGroup` | Retrieves group information |
| `Add-ADGroupMember` | Adds members to a group |
| `Remove-ADGroupMember` | Removes members from a group |
| `Get-ADGroupMember` | Lists group members |
| `Get-ADPrincipalGroupMembership` | Lists a user's group memberships |
| `Get-Acl` | Inspects an object's access control list |

Most `*-AD*` commands require the Active Directory PowerShell module and appropriate permissions.

### Key Takeaways

1. User management controls the lifecycle of domain accounts.
2. Group management simplifies access administration.
3. Security groups are used to assign permissions.
4. Access control determines what users can do.
5. DACLs control access, while SACLs define auditing.
6. NTFS and SMB share permissions work together for network file access.
7. Security configuration protects domain identities and systems.
8. Administrative accounts must follow least privilege.
9. Logging and monitoring help identify suspicious administrative activity.
10. Proper administration reduces the risk of Active Directory compromise.

---

## 10. 🎯 Active Recall & Exam Questions

Try answering these questions without looking at the notes.

1. What is Active Directory user management?
2. Explain the lifecycle of a domain user account.
3. What is the difference between a standard user and a service account?
4. What is group management, and why is it important?
5. Differentiate between security groups and distribution groups.
6. Explain Global, Domain Local, and Universal group scopes.
7. What is the AGDLP model?
8. What is access control?
9. Differentiate between authentication and authorization.
10. What is an Access Control List?
11. Explain the difference between DACL and SACL.
12. What is the difference between NTFS and SMB share permissions?
13. What is permission inheritance?
14. What is the Principle of Least Privilege?
15. Why should administrators use separate administrative accounts?
16. What is the purpose of Group Policy security configuration?
17. Why is monitoring privileged group membership important?
18. What is change management?
19. Why are Active Directory backups important?
20. Explain five security best practices for Active Directory administration.

<details>
<summary><strong>✅ Check Your Answers</strong></summary>

**1. User Management**

The process of creating, configuring, maintaining, and removing user accounts in Active Directory.

**2. User Account Lifecycle**

Account creation → Group assignment → Permission configuration → Normal usage → Access review → Account disabling → Retention or deletion.

**3. Standard User vs Service Account**

A standard user account represents an employee. A service account provides an identity for an application or service.

**4. Group Management**

The process of organizing users into groups to simplify permission and access management.

**5. Security vs Distribution Groups**

Security groups assign permissions. Distribution groups are primarily used for email distribution.

**6. Group Scopes**

- Global: Commonly groups accounts from its own domain.
- Domain Local: Used to assign permissions to resources in its own domain.
- Universal: Can contain members from multiple domains in the same forest.

**7. AGDLP**

Accounts → Global Groups → Domain Local Groups → Permissions.

**8. Access Control**

Determines who can access a resource and which actions they can perform.

**9. Authentication vs Authorization**

Authentication verifies identity. Authorization determines permitted actions.

**10. ACL**

An Access Control List is a collection of entries defining permissions or auditing behavior for a securable object.

**11. DACL vs SACL**

DACL controls access. SACL defines which access attempts are audited.

**12. NTFS vs Share Permissions**

NTFS permissions apply to files and folders. Share permissions apply to access through SMB shares.

**13. Permission Inheritance**

Child files and folders can receive applicable permissions from parent folders.

**14. Least Privilege**

Giving users and administrators only the permissions required for their responsibilities.

**15. Separate Administrative Accounts**

They reduce exposure of privileged credentials during everyday activities and improve accountability.

**16. Group Policy Security Configuration**

It centrally enforces security settings across domain users and computers.

**17. Privileged Group Monitoring**

Unexpected membership changes can indicate privilege escalation or account compromise.

**18. Change Management**

A controlled process for requesting, approving, testing, implementing, and documenting configuration changes.

**19. Active Directory Backups**

They help recover directory services after accidental deletion, corruption, ransomware, or infrastructure failure.

**20. Five Security Best Practices**

- Apply least privilege.
- Use separate administrative accounts.
- Review group membership.
- Monitor security events.
- Maintain secure backups.

</details>

---

## 🏁 Final Thought

Active Directory administration is not simply about creating users and assigning permissions. It is about maintaining control over an organization's identity infrastructure.

A good administrator ensures that:

- Every user has an appropriate account.
- Every group has a clear purpose.
- Every permission has a business justification.
- Every important configuration is secure.
- Every administrative action can be reviewed and audited.

> **In enterprise cybersecurity, controlling identities and permissions is one of the most important steps toward protecting the entire organization.**
