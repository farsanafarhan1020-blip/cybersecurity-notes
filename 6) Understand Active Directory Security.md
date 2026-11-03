# 🛡️ Understanding Active Directory Security

> **Active Directory Security is the practice of protecting an organization's identity infrastructure, user accounts, credentials, domain controllers, and directory resources against unauthorized access and cyberattacks.**

Active Directory (AD) is one of the most important components of many enterprise networks. It manages authentication, authorization, users, computers, groups, and access to organizational resources.

If an attacker compromises an ordinary user account, they may gain limited access. However, if they compromise a highly privileged account or a Domain Controller, they could potentially gain control over a large part of the organization's network.

For cybersecurity professionals, understanding Active Directory security is essential for identifying vulnerabilities, preventing privilege escalation, detecting suspicious activities, and protecting enterprise environments.

---

## 1. 🔑 Privileged Accounts

### What Are Privileged Accounts?

Privileged accounts are accounts that have elevated permissions to perform administrative or sensitive operations within an IT environment.

Unlike standard user accounts, privileged accounts can perform actions such as:

- Creating and deleting user accounts.
- Modifying group memberships.
- Changing security policies.
- Managing domain controllers.
- Accessing sensitive information.
- Modifying system configurations.
- Managing servers and network resources.

These accounts are powerful because they can control important parts of an organization's infrastructure.

### Types of Privileged Accounts

| Account Type | Description |
|---|---|
| Domain Administrator | Has extensive administrative privileges within a domain |
| Enterprise Administrator | Has forest-wide administrative privileges |
| Schema Administrator | Can modify the Active Directory schema |
| Local Administrator | Has administrative privileges on a particular computer |
| Server Administrator | Manages assigned servers |
| Service Account | Provides an identity for applications and services |
| Backup Operator | Performs privileged backup and restore operations |

**Important:** A service account is not automatically a privileged account. Its privileges depend on the permissions assigned to it.

### Important Privileged Groups

Active Directory contains several built-in groups with powerful permissions.

| Group | Purpose |
|---|---|
| Domain Admins | Domain-level administration |
| Enterprise Admins | Forest-wide administration |
| Schema Admins | Modifies directory schema |
| Administrators | Provides administrative capabilities |
| Backup Operators | Performs backup-related operations |
| Account Operators | Provides delegated account-management capabilities |

Membership in these groups must be strictly controlled.

### Why Are Privileged Accounts Dangerous?

Imagine an attacker compromises a normal employee account.

```text
       Normal User Account
               |
               v
        Limited Permissions
               |
               v
      Access to Some Resources
```

Now imagine the attacker compromises a Domain Admin account.

```text
       Domain Admin Account
               |
               v
       Extensive Domain Control
               |
       +-------+--------+
       |       |        |
       v       v        v
     Users   Servers   Policies
       |       |        |
       +-------+--------+
               |
               v
       Potential Domain Compromise
```

A compromised privileged account can allow attackers to:

- Create unauthorized accounts.
- Modify security policies.
- Access sensitive organizational data.
- Disable security controls.
- Deploy malicious software.
- Move laterally across systems.
- Establish persistent access.

### Privilege Escalation

Privilege escalation occurs when an attacker obtains permissions higher than those initially available to them.

There are two major types:

**1. Vertical Privilege Escalation**

An attacker moves from a lower privilege level to a higher privilege level.

Example:

```text
Standard User
      |
      v
Local Administrator
      |
      v
Domain Administrator
```

**2. Horizontal Privilege Escalation**

An attacker gains access to another user's resources or account at a similar privilege level.

Example:

```text
Alice's Account
      |
      v
Unauthorized Access
      |
      v
Bob's Account Data
```

### Privileged Access Management (PAM)

Privileged Access Management is a set of security practices and technologies used to control, monitor, and protect privileged accounts.

Common PAM capabilities include:

- Secure credential storage.
- Password rotation.
- Privileged session monitoring.
- Just-in-Time (JIT) access.
- Just Enough Administration (JEA).
- Approval-based administrative access.
- Privileged account discovery.

**JIT (Just-in-Time Access):** Gives administrative privileges only when required and for a limited period.

**JEA (Just Enough Administration):** Limits administrators to the specific operations they need to perform.

### Protecting Privileged Accounts

- Use separate standard and administrative accounts.
- Enable MFA wherever supported.
- Minimize Domain Admin membership.
- Use dedicated privileged workstations.
- Avoid using privileged accounts for everyday browsing or email.
- Monitor privileged logons.
- Review privileged group membership regularly.
- Use controlled administrative access and approval processes.

<text weight="semibold" color="success">Key takeaway:</text> Privileged accounts are high-value targets. Protecting them is one of the most important responsibilities in Active Directory security.

---

## 2. 🔐 Credential Security

### What Is Credential Security?

Credential security is the process of protecting authentication information used to access systems and resources.

Credentials can include:

- Passwords.
- Password hashes.
- Kerberos tickets.
- Authentication tokens.
- Private keys.
- Smart card credentials.
- Service-account secrets.

If attackers obtain valid credentials, they may access systems without exploiting traditional software vulnerabilities.

This is why credential theft is one of the most important threats to enterprise identity infrastructure.

### How Windows Handles Credentials

When a user logs in to a Windows system, Windows uses authentication mechanisms to verify their identity.

In an Active Directory domain, Kerberos is the primary authentication protocol.

A simplified authentication flow:

```text
       User Enters Credentials
                 |
                 v
          Windows Logon
                 |
                 v
        Authentication Request
                 |
                 v
       Domain Controller (KDC)
                 |
                 v
       Identity Verification
                 |
                 v
       Ticket Granting Ticket
                 |
                 v
         Access to Resources
```

The Kerberos Key Distribution Center (KDC) operates on the Domain Controller.

It consists of two logical services:

- Authentication Service (AS).
- Ticket Granting Service (TGS).

The KDC helps issue Kerberos tickets that allow users to access domain resources.

### Credential Storage in Windows

Windows stores and manages authentication-related information using different components.

Important examples:

| Component | Purpose |
|---|---|
| LSASS | Handles important local security and authentication operations |
| SAM Database | Stores local account information |
| NTDS.dit | Active Directory database containing directory information and credential-related data |
| Windows Credential Manager | Stores supported credentials for applications and services |
| Kerberos Cache | Holds Kerberos tickets associated with logon sessions |

### Understanding LSASS

LSASS stands for Local Security Authority Subsystem Service.

It is a critical Windows process responsible for important security operations, including:

- Enforcing local security policies.
- Handling authentication-related operations.
- Managing security tokens.
- Supporting credential validation and security mechanisms.

LSASS is a high-value target because sensitive authentication material may be accessible to appropriately privileged processes.

### Credential Theft Attacks

#### 1. Credential Dumping

Credential dumping is the process of extracting authentication-related information from a system.

Attackers may target:

- LSASS memory.
- Local account databases.
- Active Directory databases.
- Cached credentials.
- Application secrets.

Stolen information may be used for unauthorized access or further attacks.

#### 2. Pass-the-Hash (PtH)

Pass-the-Hash is an attack technique in which an attacker uses a captured password hash to authenticate to supported services without knowing the original plaintext password.

```text
       Password Hash Stolen
                |
                v
        Attacker Obtains Hash
                |
                v
       Uses Hash for Authentication
                |
                v
       Unauthorized System Access
```

The attack is especially relevant to environments that support NTLM authentication.

#### 3. Pass-the-Ticket (PtT)

Pass-the-Ticket involves stealing and reusing valid Kerberos tickets.

Instead of obtaining a user's plaintext password, an attacker attempts to use a stolen authentication ticket to access resources.

#### 4. Kerberoasting

Kerberoasting targets service accounts associated with Service Principal Names (SPNs).

A simplified process:

1. An authenticated domain user requests a service ticket for an SPN.
2. The service ticket contains data encrypted using the service account's secret.
3. An attacker obtains the ticket.
4. The attacker attempts offline password guessing against the encrypted material.
5. If the service-account password is weak, it may be recovered.

**Defensive measures:**

- Use long, randomly generated service-account secrets.
- Prefer Group Managed Service Accounts (gMSAs) where appropriate.
- Minimize service-account privileges.
- Monitor unusual service-ticket requests.

#### 5. AS-REP Roasting

AS-REP Roasting targets accounts configured without Kerberos pre-authentication.

An attacker may request authentication data that can be subjected to offline password guessing.

**Defensive measures:**

- Require Kerberos pre-authentication for accounts unless a documented exception exists.
- Identify accounts with pre-authentication disabled.
- Use strong passwords for service and user accounts.
- Monitor unusual authentication requests.

### Credential Protection Best Practices

| Practice | Security Benefit |
|---|---|
| MFA | Reduces the impact of stolen passwords |
| Windows Defender Credential Guard | Helps protect supported credential material using virtualization-based security |
| LAPS / Windows LAPS | Manages unique local administrator passwords |
| gMSA | Automates managed service-account password handling |
| Strong Service Account Secrets | Reduces password-guessing risk |
| Restrict Local Admin Rights | Limits opportunities for credential theft |
| Secure Domain Controllers | Protects sensitive directory credentials |
| Disable Unnecessary NTLM | Reduces reliance on legacy authentication where feasible |

### Windows LAPS

Windows Local Administrator Password Solution (Windows LAPS) automatically manages and backs up local administrator passwords for supported Windows devices.

Without proper management, organizations may accidentally use the same local administrator password across multiple computers.

Example:

```text
Computer-01 → Unique Password A
Computer-02 → Unique Password B
Computer-03 → Unique Password C
```

If one computer is compromised, the same password cannot automatically be reused on every other computer.

<text weight="semibold" color="success">Key takeaway:</text> Credential security protects the authentication material that attackers often target to gain access to enterprise systems.

---

## 3. 👁️ Account Monitoring

### What Is Account Monitoring?

Account monitoring is the continuous observation and analysis of user and computer account activities to identify suspicious behavior, unauthorized access, and potential security incidents.

In Active Directory environments, monitoring helps security teams detect:

- Brute-force attacks.
- Password spraying.
- Account compromise.
- Privilege escalation.
- Unauthorized account creation.
- Suspicious group membership changes.
- Unusual administrative activity.

Monitoring is an essential part of enterprise security operations.

### Why Is Account Monitoring Important?

Imagine an attacker obtains an employee's password.

Without monitoring, the attacker may access the company's resources without being noticed.

With proper monitoring, security teams may detect unusual login times, unexpected locations, repeated failures, or abnormal access patterns.

```text
        User Account Activity
                 |
                 v
          Security Logging
                 |
                 v
          Event Collection
                 |
                 v
          Activity Analysis
                 |
          +------+------+
          |             |
          v             v
       Normal        Suspicious
          |             |
          v             v
       Routine       Investigation
       Activity          |
                         v
                   Incident Response
```

### Important Windows Security Events

Windows records security-related activities in the Security event log.

Some important event IDs include:

| Event ID | Description |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Account logged off |
| 4648 | Logon attempted using explicit credentials |
| 4672 | Special privileges assigned to a new logon |
| 4720 | User account created |
| 4722 | User account enabled |
| 4724 | Password reset attempted |
| 4725 | User account disabled |
| 4726 | User account deleted |
| 4728 | Member added to a global security group |
| 4732 | Member added to a local security group |
| 4756 | Member added to a universal security group |
| 4740 | User account locked out |

These events provide useful evidence during security investigations. Their availability depends on the audit configuration and the activity being performed.

### Understanding Important Events

**Event ID 4624 — Successful Logon**

Records a successful logon.

Security analysts may investigate:

- Account name.
- Logon type.
- Source network address, when available.
- Workstation information.
- Authentication details.

**Event ID 4625 — Failed Logon**

Records a failed logon attempt.

Repeated failures may indicate:

- Incorrect passwords.
- Brute-force attempts.
- Password spraying.
- Misconfigured applications or services.

**Event ID 4672 — Special Privileges Assigned**

Indicates that special privileges were assigned to a new logon session.

It is useful for identifying privileged sessions, but it does not automatically mean malicious activity occurred.

**Event ID 4720 — User Account Created**

Can help detect unexpected account creation.

**Event ID 4728 — Member Added to a Global Security Group**

Can help detect unauthorized group membership changes.

### Common Account-Based Attacks to Monitor

#### 1. Brute-Force Attack

An attacker attempts many password combinations against an account.

Example:

```text
admin → password123
admin → welcome123
admin → qwerty123
admin → ...
```

Potential indicators:

- Many failed logons.
- Repeated attempts against one account.
- Authentication attempts from unusual sources.

#### 2. Password Spraying

An attacker tries a small number of common passwords against many accounts.

Example:

```text
Password: Winter2026!

alice → Failed
bob → Failed
charlie → Successful
david → Failed
```

Unlike traditional brute force, password spraying attempts to avoid triggering account lockout thresholds on individual accounts.

#### 3. Account Takeover

An attacker gains control of a legitimate account using stolen credentials, phishing, or another technique.

Potential indicators:

- Unusual login locations or devices.
- Unexpected MFA activity.
- New authentication patterns.
- Access to resources the user rarely accesses.
- Unexpected password or recovery changes.

#### 4. Privilege Escalation

An attacker obtains additional permissions.

Potential indicators:

- Unexpected privileged group membership.
- Changes to user rights.
- Unusual administrative activity.
- Unauthorized changes to security policies.

### Monitoring Tools

| Tool | Purpose |
|---|---|
| Windows Event Viewer | Views Windows event logs |
| PowerShell | Queries and analyzes Windows information |
| Windows Event Forwarding | Collects events from multiple Windows systems |
| Microsoft Sentinel | Cloud-based SIEM and security analytics |
| Microsoft Defender for Identity | Detects identity-related threats in supported environments |
| Splunk | Collects and analyzes security logs |
| Sysmon | Provides detailed system activity telemetry |

A SIEM (Security Information and Event Management) platform collects and analyzes security events from multiple systems to help detect suspicious activity.

### Practical PowerShell Commands

View recent failed logon events:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4625
} -MaxEvents 20
```

View successful logon events:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4624
} -MaxEvents 20
```

View account creation events:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4720
} -MaxEvents 20
```

View privileged group membership changes:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4728
} -MaxEvents 20
```

These commands require access to the relevant Security log. Administrative privileges and suitable audit settings may be necessary.

### Account Monitoring Best Practices

- Monitor failed and successful logons.
- Review privileged group membership changes.
- Alert on suspicious account creation.
- Monitor inactive and dormant accounts.
- Investigate unusual login patterns.
- Centralize logs where possible.
- Establish alert thresholds based on normal activity.
- Retain logs according to organizational requirements.

<text weight="semibold" color="success">Key takeaway:</text> Account monitoring helps detect suspicious identity activity before it develops into a larger security incident.

---

## 4. 📋 Security Baselines

### What Is a Security Baseline?

A security baseline is a predefined set of recommended security configurations used to establish a minimum acceptable security level for systems and infrastructure.

In Active Directory environments, security baselines help organizations maintain consistent security settings across:

- Domain controllers.
- Windows servers.
- Domain-joined computers.
- User accounts.
- Group Policies.
- Authentication mechanisms.

Instead of configuring every computer independently, organizations can use security baselines to apply consistent policies.

### Why Are Security Baselines Important?

Imagine an organization has 500 Windows computers.

If every computer is configured differently, maintaining security becomes difficult.

Some computers may have:

- Weak password policies.
- Disabled firewalls.
- Outdated security settings.
- Unnecessary services.
- Excessive user privileges.

A security baseline establishes a consistent configuration standard.

```text
             Security Baseline
                    |
          +---------+---------+
          |         |         |
          v         v         v
       Server    Computer   Domain
          |         |       Controller
          v         v         v
       Secure    Secure     Secure
       Config    Config     Config
```

### Important Security Baseline Areas

| Area | Recommended Configuration |
|---|---|
| Password Security | Strong authentication requirements |
| Account Lockout | Appropriate protection against guessing |
| Windows Firewall | Restrict unnecessary network traffic |
| Audit Policy | Enable relevant security logging |
| User Rights | Restrict sensitive privileges |
| Remote Access | Limit and secure administrative access |
| Defender | Enable appropriate endpoint protections |
| SMB Security | Disable insecure legacy configurations where possible |
| PowerShell | Apply appropriate logging and security controls |
| Domain Controllers | Apply hardened configurations |

### Microsoft Security Baselines

Microsoft provides security baseline recommendations for supported Windows products and other Microsoft technologies.

These baselines help administrators configure systems according to established security guidance.

Tools and resources include:

- Microsoft Security Compliance Toolkit.
- Group Policy Management Console (GPMC).
- Group Policy Objects (GPOs).
- Microsoft security documentation.
- Microsoft Defender security recommendations.

### Security Baselines and Group Policy

Group Policy is commonly used to enforce baseline configurations across Active Directory environments.

For example, an organization may create a GPO called:

```text
Company-Security-Baseline
```

It can contain settings for:

- Password and account policies where applicable.
- Windows Firewall.
- Security auditing.
- User rights.
- Defender configuration.
- Remote access restrictions.

The GPO can be linked to appropriate Organizational Units or other supported directory scopes.

### Understanding Configuration Drift

Configuration drift occurs when a system's actual settings gradually differ from its approved baseline.

Example:

```text
Initial Configuration:
Firewall Enabled
Defender Enabled
Audit Policy Configured
        |
        v
     Over Time
        |
        v
Firewall Disabled
Audit Settings Changed
Unapproved Software Installed
        |
        v
Configuration Drift
```

Configuration drift can introduce security weaknesses.

Organizations should regularly compare actual system configurations against approved baselines.

### Compliance and Security Baselines

Security baselines can support compliance with security frameworks and standards.

Examples include:

| Framework / Guidance | Purpose |
|---|---|
| CIS Benchmarks | Security configuration recommendations |
| Microsoft Security Baselines | Microsoft product hardening recommendations |
| NIST Cybersecurity Framework | Cybersecurity risk management guidance |
| ISO/IEC 27001 | Information security management system requirements |

A baseline does not automatically make an organization compliant. Organizations must evaluate requirements, implementation, evidence, and operational risks.

### Security Baseline Best Practices

1. Use trusted baseline recommendations.
2. Test configurations before production deployment.
3. Apply baselines consistently.
4. Document exceptions.
5. Review configurations regularly.
6. Monitor configuration drift.
7. Update baselines when security guidance changes.
8. Avoid applying settings without understanding their operational impact.

<text weight="semibold" color="success">Key takeaway:</text> Security baselines provide a consistent starting point for hardening Active Directory infrastructure and reducing configuration-related vulnerabilities.

---

## 5. 🎯 Attack Surface Awareness

### What Is an Attack Surface?

An attack surface is the collection of all possible points through which an attacker could attempt to interact with, compromise, or gain unauthorized access to a system.

In Active Directory, the attack surface includes accounts, credentials, domain controllers, network services, permissions, configurations, and connected systems.

Understanding the attack surface helps security professionals identify and reduce potential entry points.

### Active Directory Attack Surface

Important areas include:

| Attack Surface | Potential Risk |
|---|---|
| User Accounts | Password attacks and account takeover |
| Privileged Accounts | High-impact compromise |
| Service Accounts | Credential theft and privilege abuse |
| Domain Controllers | Centralized identity infrastructure compromise |
| Group Membership | Unauthorized privilege escalation |
| Group Policies | Policy manipulation and malicious configuration |
| Network Services | Exploitation of exposed or vulnerable services |
| Authentication Protocols | Credential and ticket-based attacks |
| Trust Relationships | Potential access across domain boundaries |
| Administrative Workstations | Theft of privileged credentials |

### 5.1 Weak User Accounts

Weak passwords and poorly protected accounts provide opportunities for attackers.

Examples:

- Short passwords.
- Reused passwords.
- Passwords exposed through phishing.
- Inactive accounts left enabled.
- Accounts without appropriate MFA protections.

**Defensive approach:**

- Use strong, unique credentials.
- Enable MFA where supported.
- Disable unnecessary accounts.
- Monitor authentication activity.
- Review account exposure.

### 5.2 Excessive Privileges

Users may have more permissions than required.

For example:

An ordinary employee may accidentally become a member of a privileged group.

This creates an opportunity for privilege escalation.

**Defensive approach:**

- Review privileged group memberships.
- Use least privilege.
- Audit delegated permissions.
- Monitor changes to sensitive groups.

### 5.3 Exposed Services

Domain environments use different network services to support authentication, directory queries, file sharing, and administration.

Common examples include:

| Port | Protocol / Service | Purpose |
|---|---|---|
| 53 | DNS | Name resolution |
| 88 | Kerberos | Authentication |
| 135 | RPC Endpoint Mapper | Windows remote procedure services |
| 389 | LDAP | Directory access |
| 445 | SMB | File sharing and Windows networking |
| 464 | Kerberos Password Change | Kerberos password operations |
| 636 | LDAPS | LDAP over TLS |
| 3268 | Global Catalog LDAP | Forest-wide directory searches |
| 3269 | Global Catalog LDAPS | Secure Global Catalog access |
| 3389 | RDP | Remote Desktop access |

These are common ports, not a complete list of Active Directory-related services.

Not every service should be exposed to every network.

**Defensive approach:**

- Restrict network access to necessary services.
- Use firewalls and network segmentation.
- Secure LDAP connections.
- Limit remote administration.
- Monitor unexpected service exposure.

### 5.4 Misconfigured Group Policies

Group Policy Objects can control important security settings.

If an attacker gains permission to modify a sensitive GPO, they may be able to influence the configuration of systems within its scope.

**Defensive approach:**

- Restrict GPO modification rights.
- Review GPO permissions.
- Monitor changes.
- Audit GPO links and inheritance.
- Back up important GPOs.

### 5.5 Unsecured Service Accounts

Service accounts may have access to sensitive systems and resources.

Weak passwords, excessive privileges, and poor credential management can make them attractive targets.

**Defensive approach:**

- Use gMSAs where supported.
- Restrict service-account permissions.
- Avoid interactive logon where unnecessary.
- Monitor service-account authentication.
- Rotate or automatically manage credentials.

### 5.6 Legacy Authentication Protocols

Older authentication protocols can introduce additional security risks.

NTLM, for example, may expose organizations to credential-relay and related authentication attacks when improperly configured.

**Defensive approach:**

- Inventory NTLM usage.
- Prefer modern authentication mechanisms.
- Reduce NTLM usage where operationally possible.
- Apply signing and channel protections where supported.
- Monitor suspicious authentication behavior.

### 5.7 Domain Trust Relationships

Trust relationships allow authentication and resource access between domains or forests under configured conditions.

If trust relationships are poorly configured, they can increase the attack surface.

**Defensive approach:**

- Review all trust relationships.
- Remove unnecessary trusts.
- Understand trust direction and scope.
- Apply appropriate filtering and restrictions.
- Monitor authentication across trust boundaries.

### Attack Surface Reduction Process

A security team can use the following process:

```text
        Identify Assets
              |
              v
      Discover Entry Points
              |
              v
       Identify Weaknesses
              |
              v
        Assess Risk
              |
              v
      Prioritize Remediation
              |
              v
      Apply Security Controls
              |
              v
       Monitor Continuously
              |
              v
       Review and Improve
```

### Attack Surface vs Attack Vector

These terms are related but different.

| Attack Surface | Attack Vector |
|---|---|
| All possible points of exposure | A specific method used to attack |
| Describes the overall exposure | Describes the attack path or technique |
| Example: Exposed SMB service | Example: Exploiting a vulnerable SMB implementation |

An attack surface describes where an attack may happen. An attack vector describes how an attacker may attempt it.

<text weight="semibold" color="success">Key takeaway:</text> Attack surface awareness helps security teams identify exposed components, understand potential attack paths, and reduce unnecessary exposure.

---

## 6. 🔗 How the Five Security Areas Work Together

Consider a company with an Active Directory domain.

A security team wants to protect its identity infrastructure against credential theft and privilege escalation.

Each of the five areas contributes to the defense.

| Security Area | Defensive Responsibility |
|---|---|
| Privileged Accounts | Protect powerful accounts and administrative access |
| Credential Security | Protect passwords, hashes, tickets, and secrets |
| Account Monitoring | Detect unusual identity activity |
| Security Baselines | Maintain secure and consistent configurations |
| Attack Surface Awareness | Identify and reduce possible entry points |

### Example: Detecting a Compromised Administrator Account

Imagine an attacker steals the credentials of an administrator.

```text
       Attacker Steals Credentials
                  |
                  v
         Attempts Authentication
                  |
                  v
        Account Monitoring Detects
          Unusual Login Activity
                  |
                  v
         Security Team Investigates
                  |
                  v
        Privileged Account Protection
                  |
                  v
        Contain the Compromised Account
                  |
                  v
       Reset / Revoke Affected Credentials
                  |
                  v
         Review Systems and Activity
                  |
                  v
            Recover Securely
```

This demonstrates why Active Directory security requires multiple defensive controls rather than relying on a single security feature.

---

## 7. 🧪 Hands-on Lab: Active Directory Security Assessment

You can practice these concepts using an isolated Windows Server lab with a domain controller and a Windows client.

Use only systems and accounts you own or are explicitly authorized to assess.

### Lab 1: Identify Privileged Group Members

List Domain Admins:

```powershell
Get-ADGroupMember -Identity "Domain Admins"
```

List Enterprise Admins:

```powershell
Get-ADGroupMember -Identity "Enterprise Admins"
```

**Task:** Identify the members and determine whether each account needs its privileges.

### Lab 2: Find Enabled User Accounts

```powershell
Get-ADUser -Filter 'Enabled -eq $true' `
  -Properties LastLogonDate |
  Select-Object Name, SamAccountName, LastLogonDate
```

**Task:** Identify accounts that appear inactive and investigate them before recommending changes.

Note that `LastLogonDate` is based on replicated logon information and may not represent the exact last logon time.

### Lab 3: Identify Accounts Without Kerberos Pre-authentication

```powershell
Get-ADUser -Filter * `
  -Properties DoesNotRequirePreAuth |
  Where-Object {$_.DoesNotRequirePreAuth -eq $true} |
  Select-Object Name, SamAccountName
```

**Task:** Identify accounts where Kerberos pre-authentication is disabled and determine whether each exception is justified.

### Lab 4: Review Service Accounts

Search for accounts with service principal names:

```powershell
Get-ADUser -Filter {ServicePrincipalName -like "*"} `
  -Properties ServicePrincipalName |
  Select-Object Name, SamAccountName, ServicePrincipalName
```

**Task:** Identify service accounts and review their privileges, ownership, and credential-management approach.

### Lab 5: Review Failed Logons

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4625
} -MaxEvents 20
```

**Task:** Examine the event details and identify repeated failed authentication attempts.

### Lab 6: Review Privileged Group Changes

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4728
} -MaxEvents 20
```

You can also investigate:

- 4732 — Local security group membership changes.
- 4756 — Universal security group membership changes.

**Task:** Determine whether the changes were expected and properly authorized.

### Lab 7: Inspect Network Exposure

From an authorized assessment workstation, identify common Active Directory services exposed by your lab domain controller.

Example Nmap command:

```bash
nmap -sV -p 53,88,135,389,445,464,636,3268,3269,3389 <DC-IP>
```

**Task:**

1. Identify which services are running.
2. Understand the purpose of each service.
3. Determine whether each service needs to be reachable from your test network.
4. Document unnecessary exposure.

### Lab 8: Review Security Configuration

Open Group Policy Management:

```text
gpmc.msc
```

Inspect:

- Password policies.
- Account lockout policies.
- Windows Firewall settings.
- Audit policies.
- User Rights Assignment.
- Domain Controller security policies.

**Task:** Compare your configuration against an appropriate Microsoft security baseline or CIS Benchmark. Document any differences and test changes before applying them.

---

## 8. ⚡ Quick Revision

### Important Concepts

| Topic | Remember |
|---|---|
| Privileged Account | Account with elevated permissions |
| Domain Admin | Highly privileged domain-level account |
| Enterprise Admin | Highly privileged forest-level account |
| PAM | Privileged Access Management |
| JIT | Just-in-Time administrative access |
| JEA | Just Enough Administration |
| LSASS | Windows security and authentication process |
| NTDS.dit | Active Directory database |
| Pass-the-Hash | Uses a captured password hash for authentication |
| Pass-the-Ticket | Reuses a stolen Kerberos ticket |
| Kerberoasting | Targets service tickets associated with SPNs |
| AS-REP Roasting | Targets accounts without Kerberos pre-authentication |
| Account Monitoring | Observes and analyzes account activity |
| SIEM | Collects and analyzes security events |
| Security Baseline | Recommended minimum security configuration |
| Configuration Drift | Difference between approved and actual configuration |
| Attack Surface | All potential points of exposure |
| Attack Vector | A specific method or path used to attack |

### Important Event IDs

| Event ID | Activity |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4672 | Special privileges assigned |
| 4720 | User created |
| 4724 | Password reset attempted |
| 4725 | User disabled |
| 4728 | Member added to global security group |
| 4732 | Member added to local security group |
| 4740 | Account locked out |

### Key Takeaways

1. Privileged accounts are high-value targets.
2. Administrative access should follow least privilege.
3. Credentials include more than just passwords.
4. LSASS and NTDS.dit are important credential-security targets.
5. Kerberos-related attacks can exploit weak service-account security or misconfigurations.
6. Account monitoring helps identify suspicious authentication and privilege changes.
7. Windows security events provide valuable investigation evidence.
8. Security baselines establish consistent security configurations.
9. Configuration drift can introduce unexpected vulnerabilities.
10. Attack surface reduction limits unnecessary exposure.
11. Active Directory security requires continuous monitoring and improvement.

---

## 9. 🎯 Active Recall & Exam Questions

Try answering these without looking at the notes.

1. What is an Active Directory privileged account?
2. Differentiate between Domain Admins and Enterprise Admins.
3. What is Privileged Access Management (PAM)?
4. Explain the difference between JIT and JEA.
5. What is credential security?
6. What is the role of LSASS in Windows?
7. What is NTDS.dit?
8. Explain Pass-the-Hash and Pass-the-Ticket.
9. What is Kerberoasting?
10. What is AS-REP Roasting?
11. What is account monitoring?
12. Explain the purpose of Windows Security Event ID 4624 and 4625.
13. What does Event ID 4728 indicate?
14. What is a SIEM?
15. What is a security baseline?
16. What is configuration drift?
17. What is an attack surface?
18. Differentiate between an attack surface and an attack vector.
19. Why are service accounts important from a security perspective?
20. Explain five ways to secure an Active Directory environment.

<details>
<summary><strong>✅ Check Your Answers</strong></summary>

**1. Privileged Account**

An account with elevated permissions that can perform administrative or sensitive operations.

**2. Domain Admins vs Enterprise Admins**

Domain Admins have extensive administrative permissions within a domain. Enterprise Admins have forest-wide administrative privileges.

**3. PAM**

Privileged Access Management controls, protects, and monitors privileged accounts and sessions.

**4. JIT vs JEA**

JIT provides administrative privileges only when needed and for a limited period. JEA restricts administrators to only the operations they are authorized to perform.

**5. Credential Security**

Protecting authentication information such as passwords, hashes, tickets, tokens, and private keys.

**6. LSASS**

A Windows process responsible for important local security and authentication operations.

**7. NTDS.dit**

The database file used by Active Directory Domain Services to store directory information and credential-related data.

**8. Pass-the-Hash vs Pass-the-Ticket**

Pass-the-Hash reuses a captured password hash for supported authentication. Pass-the-Ticket reuses a stolen Kerberos ticket.

**9. Kerberoasting**

An attack that obtains service tickets for SPN-associated accounts and attempts to crack their encrypted material offline.

**10. AS-REP Roasting**

An attack targeting accounts that do not require Kerberos pre-authentication, allowing authentication data to be subjected to offline password guessing.

**11. Account Monitoring**

Continuously observing account activity to identify suspicious authentication, account changes, and unauthorized access.

**12. Event IDs 4624 and 4625**

4624 records a successful logon. 4625 records a failed logon.

**13. Event ID 4728**

A member was added to a security-enabled global group.

**14. SIEM**

Security Information and Event Management. It collects, correlates, and analyzes security events to help detect threats.

**15. Security Baseline**

A predefined set of recommended security configurations for systems and infrastructure.

**16. Configuration Drift**

When actual system settings gradually differ from the approved security configuration.

**17. Attack Surface**

All possible points through which an attacker may attempt to interact with or compromise a system.

**18. Attack Surface vs Attack Vector**

Attack surface describes the overall exposure. Attack vector describes a particular attack method or path.

**19. Service Accounts**

They provide identities for applications and services. If poorly secured, they can become targets for credential theft and privilege escalation.

**20. Five Active Directory Security Practices**

- Protect privileged accounts.
- Use strong credential management and MFA.
- Monitor account activity.
- Apply secure configuration baselines.
- Identify and reduce unnecessary attack surface.

</details>

---

## 🏁 Final Thought

Active Directory is not just a user-management system. It is a critical identity infrastructure that controls access across enterprise environments.

An attacker who compromises identities, credentials, or privileged permissions may gain access to an organization's most important systems.

A cybersecurity professional must understand how to:

- Protect privileged identities.
- Secure authentication credentials.
- Detect suspicious account activity.
- Maintain hardened configurations.
- Identify and reduce attack surfaces.

> **Strong Active Directory security means protecting identities, controlling privileges, monitoring activity, and continuously reducing opportunities for attackers.**
