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

Administrative practices are the procedures and responsibilities administrators
