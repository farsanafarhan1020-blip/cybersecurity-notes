# 🏢 Understanding Active Directory Fundamentals

> **Active Directory (AD) is Microsoft's directory service used to centrally manage users, computers, groups, and other resources in a Windows domain network.**

In an enterprise environment, hundreds or thousands of employees may need access to computers, shared folders, printers, applications, and internal services.

Managing every computer and user account individually would be difficult.

Active Directory solves this problem by providing a centralized system for identity management, authentication, authorization, and administrative control.

It is an important technology for cybersecurity professionals because attackers frequently target Active Directory environments to gain access, escalate privileges, and move across enterprise networks.

---

## 1. 🏢 What Is Active Directory?

### What Is Active Directory?

Active Directory (AD) is a directory service developed by Microsoft for Windows domain networks.

It stores information about network objects and provides centralized management of identities and resources.

These objects include:

- Users
- Computers
- Groups
- Servers
- Printers
- Organizational Units (OUs)
- Security policies

Think of Active Directory as a centralized directory for an organization's IT environment.

### Real-World Example

Imagine a company with 500 employees.

Each employee needs:

- A Windows login account.
- Access to company applications.
- Shared folders.
- Email and internal services.
- Permissions based on their job role.

Without Active Directory, administrators might need to configure accounts and permissions separately on many computers.

With Active Directory, administrators can manage these identities and access policies centrally.

```text
             COMPANY NETWORK
                    |
                    v
           ACTIVE DIRECTORY
                    |
        +-----------+-----------+
        |           |           |
        v           v           v
      Users      Computers    Groups
        |           |           |
        +-----------+-----------+
                    |
                    v
          Centralized Management
                    |
        +-----------+-----------+
        |           |           |
        v           v           v
    Authentication Authorization Policies
```

### Main Functions of Active Directory

| Function | Description |
|---|---|
| Identity Management | Creates and manages user and computer accounts |
| Authentication | Verifies identities during login |
| Authorization Support | Works with permissions and security groups |
| Resource Management | Organizes network resources |
| Group Management | Assigns users to security groups |
| Policy Management | Applies centralized configurations through Group Policy |
| Directory Services | Stores and provides information about network objects |

### Important Active Directory Technologies

Active Directory is not just one protocol. It uses several technologies to provide directory and authentication services.

| Technology | Purpose |
|---|---|
| LDAP | Accesses and manages directory information |
| Kerberos | Provides ticket-based network authentication |
| DNS | Helps locate domain services and domain controllers |
| Group Policy | Centrally manages computer and user configurations |
| SMB | Provides file and printer sharing |
| NTLM | Legacy Windows authentication protocol, still present in some environments |

**Important:** Active Directory Domain Services (AD DS) is the specific Windows Server role that provides traditional domain-based directory services.

Microsoft also offers other directory-related services, such as Microsoft Entra ID, formerly Azure Active Directory. Entra ID is a cloud identity service and is not simply another name for on-premises AD DS.

### Active Directory vs Workgroup

Windows computers can operate in a workgroup or join a domain.

| Workgroup | Active Directory Domain |
|---|---|
| Decentralized management | Centralized management |
| Accounts are generally managed locally | Domain identities are centrally managed |
| Suitable for small environments | Suitable for enterprise environments |
| Limited centralized policy management | Supports centralized Group Policy |
| No domain controller required | Uses domain controllers |

**Example:**

A home network with three computers may use a workgroup.

A company with hundreds of computers may use Active Directory to manage accounts and access centrally.

---

## 2. 🌐 Domains

### What Is an Active Directory Domain?

An Active Directory domain is a logical administrative and security boundary containing users, computers, groups, and other directory objects.

A domain provides a centralized environment for managing identities, authentication, and access.

Every domain has a DNS-style name.

Examples:

```text
company.local
corp.example.com
ad.company.com
```

These are illustrative names. In production, organizations commonly use a subdomain of a domain they own, such as `ad.company.com`.

### How a Domain Works

Imagine an organization called TechSecure.

It has:

- 200 employees.
- 150 Windows computers.
- 10 servers.
- Multiple departments.

The company creates an Active Directory domain:

```text
techsecure.local
```

Users and computers can join this domain.

```text
             techsecure.local
                     |
          +----------+----------+
          |          |          |
          v          v          v
        Users     Computers    Groups
          |          |          |
          +----------+----------+
                     |
                     v
              Domain Policies
```

The organization can manage these objects centrally instead of configuring every computer independently.

### Domain Objects

A domain contains different types of directory objects.

| Object | Description |
|---|---|
| User | Represents an individual account |
| Computer | Represents a domain-joined computer |
| Group | Contains users or other supported directory objects |
| Organizational Unit | Organizes objects into administrative containers |
| Shared Resource | Represents or provides access to network resources through associated directory objects and permissions |

### Domain Naming

Active Directory domains use hierarchical DNS naming.

Example:

```text
corp.example.com
```

This can be broken down into:

- `com` — Top-level DNS domain.
- `example` — Registered domain.
- `corp` — Subdomain used by the organization.

A domain name is also used to identify the directory domain and locate its services through DNS.

### Domain Tree

A domain tree is a hierarchy of domains that share a contiguous DNS namespace.

Example:

```text
             example.com
                  |
          +-------+-------+
          |               |
          v               v
      india.example.com  uk.example.com
```

These domains can share a hierarchical namespace and establish trust relationships.

### Forest

A forest is the highest-level logical structure in traditional Active Directory.

It can contain one or more domain trees.

Domains within the same forest share a common schema, configuration, and global catalog infrastructure.

Example:

```text
             ACTIVE DIRECTORY FOREST
                       |
             +---------+---------+
             |                   |
             v                   v
        example.com          example.org
             |                   |
        +----+----+         +----+----+
        |         |         |         |
        v         v         v         v
      India      UK       Finance    Research
```

This is a conceptual illustration of multiple domain trees within one forest.

### Domain vs Tree vs Forest

| Structure | Meaning |
|---|---|
| Domain | Logical directory and administrative unit containing objects |
| Tree | One or more hierarchically connected domains sharing a contiguous namespace |
| Forest | One or more domain trees sharing a common AD structure |

### Trust Relationships

A trust relationship allows authentication information from one domain to be accepted by another domain under defined conditions.

Example:

An employee from the India domain needs access to a resource in the UK domain.

If the necessary trust and permissions exist, the employee may access that resource using their existing domain identity.

**Important:** Trust does not automatically grant access to every resource. Authorization permissions are still required.

### Why Domains Matter in Cybersecurity

Domains provide centralized identity and security management, but they also create attractive targets.

If an attacker compromises a highly privileged domain account, they may gain extensive control over domain resources.

Security professionals must understand:

- Domain authentication.
- Trust relationships.
- Domain account permissions.
- Privileged groups.
- Domain security policies.
- Domain controller security.

---

## 3. 🖥️ Domain Controllers (DC)

### What Is a Domain Controller?

A Domain Controller (DC) is a Windows Server system running Active Directory Domain Services (AD DS) that manages directory-based authentication and directory information for a domain.

It is one of the most important components of an Active Directory environment.

A domain controller helps users authenticate and allows administrators to manage domain resources centrally.

### What Does a Domain Controller Do?

| Function | Description |
|---|---|
| Authentication | Verifies domain user and computer identities |
| Directory Storage | Stores Active Directory objects and information |
| Authorization Support | Provides group and identity information used in access decisions |
| Policy Support | Helps distribute and apply Group Policy |
| Replication | Synchronizes directory changes with other domain controllers |
| Service Discovery | Registers and provides information needed to locate domain services |

### How Domain Authentication Works

Imagine an employee named Alice logs in to a domain-joined Windows computer.

A simplified process:

1. Alice enters her domain username and password.
2. The computer locates a suitable domain controller using DNS.
3. The computer and domain controller perform the authentication process.
4. The domain controller verifies the user's credentials using the configured authentication protocol.
5. If authentication succeeds, the computer receives the relevant authentication information.
6. Windows creates the user's login session.

```text
              ALICE
                |
                v
       Domain-Joined Computer
                |
                v
         Enter Credentials
                |
                v
        Locate Domain Controller
                |
                v
        Authentication Request
                |
                v
        Domain Controller
                |
                v
       Verify User Credentials
                |
           +----+----+
           |         |
        Success    Failure
           |         |
           v         v
        Login     Access Denied
```

This is a simplified illustration. Actual Windows authentication involves protocols, tickets, policies, and additional security checks.

### Kerberos Authentication

Kerberos is the primary authentication protocol used by modern Active Directory domain environments.

It uses tickets to authenticate users and services without repeatedly sending the user's password across the network.

Three important Kerberos components are:

| Component | Purpose |
|---|---|
| Client | User or computer requesting authentication |
| Key Distribution Center (KDC) | Issues and manages Kerberos tickets |
| Service | Resource the client wants to access |

The KDC runs on a domain controller and contains two logical services:

- Authentication Service (AS).
- Ticket Granting Service (TGS).

### Simplified Kerberos Authentication Flow

```text
          User / Client
                |
                v
       Request Authentication
                |
                v
          Domain Controller
                |
                v
      Authentication Service
                |
                v
       Ticket Granting Ticket
              (TGT)
                |
                v
       Request Service Ticket
                |
                v
       Ticket Granting Service
                |
                v
          Service Ticket
                |
                v
        Access Network Service
```

**Important:** A successful Kerberos authentication does not automatically authorize access to every service. The target service still checks the user's permissions.

### Other Important Domain Controller Components

**1. Active Directory Database**

The directory database is stored in:

```text
C:\Windows\NTDS\NTDS.dit
```

It contains directory information, including user, computer, and group objects.

This file is highly sensitive because it contains security-related directory data.

**2. SYSVOL**

SYSVOL is a shared directory replicated between domain controllers in a domain.

It stores important domain-wide files, including:

- Group Policy templates.
- Group Policy scripts.
- Other domain-related files.

**3. DNS Integration**

Active Directory relies heavily on DNS.

Domain-joined computers use DNS to locate domain controllers and other domain services.

### Multiple Domain Controllers

Enterprise environments commonly use multiple domain controllers.

Why?

- Redundancy.
- Availability.
- Load distribution.
- Geographic distribution.
- Directory replication.

Example:

```text
              techsecure.local
                      |
             +--------+--------+
             |                 |
             v                 v
          DC-01              DC-02
             |                 |
             +--------+--------+
                      |
                 Replication
                      |
             +--------+--------+
             |                 |
             v                 v
         Office A           Office B
```

When a directory change occurs, domain controllers replicate the relevant information according to Active Directory replication rules.

### Why Domain Controllers Are High-Value Targets

A compromised domain controller can expose or affect a large portion of an organization's identity infrastructure.

Attackers may attempt to:

- Steal domain credentials.
- Obtain privileged access.
- Modify user and group permissions.
- Create unauthorized accounts.
- Modify Group Policy.
- Access sensitive directory information.

### Domain Controller Security Best Practices

- Keep domain controllers patched.
- Restrict interactive logins.
- Use dedicated administrative accounts.
- Enforce MFA where supported by the environment.
- Monitor privileged group changes.
- Protect the NTDS database and SYSVOL.
- Restrict network access to domain controllers.
- Maintain secure and tested backups.
- Monitor suspicious Kerberos and NTLM activity.
- Use a tiered administrative model.

**Security lesson:** Domain controllers should be treated as highly sensitive infrastructure, not ordinary application servers.

---

## 4. 🗂️ Organizational Units (OU)

### What Is an Organizational Unit?

An Organizational Unit (OU) is a container inside an Active Directory domain used to organize directory objects and delegate administrative control.

It allows administrators to group users, computers, and other supported objects based on departments, locations, or responsibilities.

OUs are also important because Group Policy Objects can be linked to them.

### Real-World Example

Imagine TechSecure has four departments:

- IT.
- Human Resources.
- Finance.
- Development.

Instead of keeping all users and computers in one large container, administrators can organize them using OUs.

```text
              techsecure.local
                      |
              +-------+-------+
              |       |       |
              v       v       v
             IT       HR    Finance
              |       |       |
              v       v       v
           Users   Users   Users
           Computers Computers Computers
```

A more detailed structure:

```text
               techsecure.local
                       |
                 Company OU
                       |
          +------------+------------+
          |            |            |
          v            v            v
       IT OU        HR OU      Development OU
          |            |            |
       +--+--+      +--+--+      +--+--+
       |     |      |     |      |     |
      Users Computers Users Computers Users Computers
```

### Why Use OUs?

**1. Organization**

OUs help organize objects logically.

For example, users and computers can be separated by department or office location.

**2. Group Policy Management**

Administrators can link Group Policy Objects to OUs to configure users and computers.

Example:

The IT OU may receive a policy that configures specific administrative tools.

The HR OU may receive a policy that restricts certain applications.

**3. Delegation of Administration**

An administrator can delegate limited administrative tasks over a specific OU.

Example:

The HR administrator may be allowed to reset passwords for users in the HR OU without receiving full domain administrator privileges.

### Nested OUs

OUs can contain other OUs.

Example:

```text
             techsecure.local
                     |
                   India
                     |
               Development
                     |
             +-------+-------+
             |               |
             v               v
          Developers      Interns
```

This makes complex organizations easier to manage.

### OU vs Group

OUs and groups are different Active Directory objects.

| Organizational Unit | Group |
|---|---|
| Organizes directory objects | Organizes users and other supported members for management or access |
| Supports Group Policy linking | Commonly used to assign permissions |
| Supports administrative delegation | Supports access control and distribution |
| Provides directory structure | Provides membership-based organization |

**Example:**

A company may create an OU named `Security Department` to organize employee accounts.

It may also create a security group named `Security-Analysts` to grant those employees permission to access security logs.

An OU does not automatically grant resource permissions simply because an object is placed inside it.

### Important: OU vs Container

An OU is not the same as a default Active Directory container.

Some default containers, such as `Users` and `Computers`, are not OUs and cannot have Group Policy Objects linked directly to them.

Administrators often create OUs to achieve more flexible organization, policy application, and delegation.

### OU Security Best Practices

- Organize OUs around administrative and policy requirements.
- Avoid unnecessarily complicated OU structures.
- Delegate only the required administrative permissions.
- Review OU permissions regularly.
- Test Group Policy changes before wide deployment.
- Avoid giving departmental administrators unnecessary domain-level access.

---

## 5. 📋 Group Policies (GPO)

### What Is Group Policy?

Group Policy is a Windows feature used to centrally manage and configure user and computer settings in an Active Directory environment.

Administrators use Group Policy to enforce security configurations, restrictions, and standard operating settings across domain-joined computers.

Instead of manually configuring hundreds of computers, administrators can create policies and apply them centrally.

A collection of these settings is stored in a **Group Policy Object (GPO)**.

### Real-World Example

Imagine a company has 300 Windows computers.

The security team wants to:

- Enforce password policies.
- Configure account lockout settings.
- Disable unauthorized removable storage access.
- Configure Windows Defender settings.
- Apply firewall rules.
- Configure Windows security settings.

Without Group Policy, administrators might need to configure each computer separately.

With Group Policy, the organization can centrally manage many of these settings.

### How Group Policy Works

Group Policy Objects can be linked to:

- Sites.
- Domains.
- Organizational Units.

When a computer or user processes Group Policy, applicable settings are evaluated and applied according to Group Policy processing rules.

```text
             Active Directory
                    |
                    v
              Domain GPO
                    |
                    v
              Department OU
                    |
                    v
             Computer / User
                    |
                    v
          Group Policy Processing
                    |
                    v
          Apply Configured Settings
```

### Two Main Types of Group Policy Settings

| Type | Description |
|---|---|
| Computer Configuration | Applies settings to computers |
| User Configuration | Applies settings to users |

#### 1. Computer Configuration

Controls settings related to the computer itself.

Examples:

- Windows Firewall configuration.
- Security settings.
- Software installation policies.
- Windows Defender configuration.
- Device restrictions.

#### 2. User Configuration

Controls settings related to the user account.

Examples:

- Desktop restrictions.
- Control Panel access.
- User-specific application settings.
- Logon scripts.
- Start menu configuration.

### Group Policy Processing Order: LSDOU

A common Group Policy processing order is:

1. Local.
2. Site.
3. Domain.
4. Organizational Unit.

This is remembered using **LSDOU**.

```text
       Local Policy
            |
            v
        Site GPO
            |
            v
       Domain GPO
            |
            v
          OU GPO
            |
            v
      Nested OU GPOs
            |
            v
    Resultant Policy Settings
```

Generally, later-applied policies can override conflicting earlier settings, subject to inheritance, enforcement, filtering, and other Group Policy rules.

### Group Policy Inheritance

Policies linked at higher levels can affect objects in lower-level OUs.

Example:

```text
            Domain GPO
                 |
                 v
              IT OU
                 |
                 v
           Security OU
                 |
                 v
          Security Computers
```

The Security OU can receive applicable settings linked to its parent containers.

Administrators can use features such as:

- Block Inheritance.
- Enforced GPO links.
- Security Filtering.
- WMI Filtering.

These features help control how policies are applied.

### Important Group Policy Security Settings

| Security Setting | Purpose |
|---|---|
| Password Policy | Controls password requirements |
| Account Lockout Policy | Restricts repeated failed login attempts |
| Audit Policy | Configures security event auditing |
| Windows Firewall Policy | Controls firewall configuration |
| User Rights Assignment | Defines privileged system rights |
| Security Options | Configures Windows security behavior |
| Software Restriction Policies | Restricts execution of certain software |
| AppLocker | Controls which applications and scripts can run |
| Windows Defender Policies | Configures endpoint protection settings |

Some settings have specific operating-system and policy prerequisites.

### Useful Group Policy Commands

**1. View Applied Group Policies**

```powershell
gpresult /r
```

Displays a summary of Resultant Set of Policy information for the user and computer.

**2. Generate a Detailed Group Policy Report**

```powershell
gpresult /h report.html
```

Generates an HTML report showing applied Group Policy information.

**3. Refresh Group Policy**

```powershell
gpupdate /force
```

Forces a refresh of applicable Group Policy settings.

**4. Open Local Group Policy Editor**

```powershell
gpedit.msc
```

Opens the Local Group Policy Editor on supported Windows editions.

**5. Open Group Policy Management Console**

```powershell
gpmc.msc
```

Opens the Group Policy Management Console when the required administration tools are installed.

### Group Policy vs Local Policy

| Local Group Policy | Domain Group Policy |
|---|---|
| Configured on an individual computer | Managed centrally through Active Directory |
| Applies to the local computer and its users | Can apply to domain users and computers |
| Suitable for standalone systems | Suitable for enterprise environments |
| Managed locally | Managed through Group Policy Management |

### Group Policy Security Risks

Incorrect Group Policy configurations can introduce security weaknesses.

Examples:

- Weak password requirements.
- Disabled firewall protection.
- Excessive user privileges.
- Unrestricted script execution.
- Insecure application configurations.
- Incorrect security filtering.
- Unauthorized modification of GPOs.

### Group Policy Security Best Practices

- Enforce strong password and account policies.
- Configure Windows Firewall.
- Enable appropriate security auditing.
- Restrict unnecessary administrative privileges.
- Control application execution.
- Protect GPO modification permissions.
- Test policy changes before deployment.
- Monitor changes to sensitive Group Policy Objects.
- Review policy inheritance and filtering.

**Security lesson:** Group Policy can strengthen an entire Windows domain, but a poorly configured GPO can weaken security across many computers at once.

---

## 6. 🔗 How Active Directory Components Work Together

Understanding each component individually is important. However, cybersecurity professionals must also understand how they work together in a real enterprise environment.

Consider a company called TechSecure.

It has:
- One Active Directory forest.
- One domain named `techsecure.local`.
- Two domain controllers.
- Multiple departmental OUs.
- Security groups for different job roles.
- Group Policies for security configuration.

### Complete Environment Structure

```text
                 TECHSECURE FOREST
                         |
                         v
                 techsecure.local
                         |
              +----------+----------+
              |                     |
              v                     v
            DC-01                 DC-02
              |                     |
              +------ Replication --+
                         |
                         v
                    DOMAIN
                         |
             +-----------+-----------+
             |           |           |
             v           v           v
           IT OU       HR OU    Development OU
             |           |           |
             v           v           v
           Users       Users       Users
             |           |           |
             +-----------+-----------+
                         |
                         v
                  Security Groups
                         |
                         v
                    Permissions
                         |
                         v
                  Resource Access

        Group Policies apply to relevant
        users and computers through their
        AD site, domain, and OU structure.
```

### Example: Employee Login Process

Imagine Bob is a developer.

1. Bob's account exists in the `techsecure.local` domain.
2. His computer is joined to the domain.
3. He enters his domain credentials.
4. The computer locates a domain controller through DNS.
5. The domain controller authenticates Bob.
6. Bob receives the relevant authentication information.
7. Applicable Group Policy settings are processed.
8. His group memberships and resource permissions determine what he can access.
9. Bob accesses the development server if the required authorization rules allow it.

This demonstrates how Active Directory connects identity management, authentication, policy enforcement, and authorization.

---

## 7. ⚔️ Active Directory from a Cybersecurity Perspective

Active Directory is a common target in enterprise attacks because it manages important identities and resources.

An attacker who gains access to a low-privilege account may attempt to discover weaknesses and move toward more privileged accounts.

### Common Active Directory Attack Techniques

| Attack Technique | Description |
|---|---|
| Password Spraying | Attempts common passwords against multiple accounts |
| Kerberoasting | Requests service tickets and attempts offline cracking of service-account credentials |
| AS-REP Roasting | Targets accounts that do not require Kerberos preauthentication |
| Pass-the-Hash | Reuses captured password hashes in supported Windows authentication scenarios |
| Pass-the-Ticket | Reuses stolen Kerberos tickets |
| DCSync | Abuses directory replication privileges to request sensitive credential data |
| Golden Ticket Attack | Forges Kerberos Ticket Granting Tickets using the domain's KRBTGT account secret |
| Group Policy Abuse | Exploits excessive permissions or insecure GPO configurations |
| Privilege Escalation | Obtains higher privileges through misconfigurations or compromised accounts |
| Lateral Movement | Moves between systems using compromised credentials or access |

These techniques are useful to understand during authorized Active Directory security assessments.

### Why Attackers Target Domain Controllers

Domain controllers contain or manage highly sensitive identity information.

Compromising a domain controller or obtaining equivalent domain privileges may allow an attacker to:

- Control user accounts.
- Modify security groups.
- Change domain policies.
- Access sensitive resources.
- Create persistence mechanisms.
- Move across domain-joined systems.

### Active Directory Defensive Security Practices

1. Protect Domain Admin and other privileged accounts.
2. Use separate administrative accounts.
3. Apply least privilege.
4. Monitor privileged group membership changes.
5. Audit Kerberos and NTLM authentication activity.
6. Disable unnecessary legacy authentication where possible.
7. Secure service accounts and use managed service accounts where appropriate.
8. Protect domain controllers and their backups.
9. Review Group Policy permissions and configurations.
10. Monitor unusual directory replication activity.
11. Maintain a tiered administration model.
12. Regularly assess the Active Directory environment.

---

## 8. 🧪 Hands-on Lab: Explore Active Directory

You can learn Active Directory through a Windows Server lab and a Windows client virtual machine.

### Recommended Lab Environment

| Machine | Purpose |
|---|---|
| Windows Server Evaluation VM | Domain Controller and AD DS |
| Windows 10/11 VM | Domain-joined client |
| Kali Linux VM | Security assessment and authorized testing |

Keep the environment isolated in a host-only or otherwise controlled virtual network. Never expose a deliberately vulnerable lab domain to the public internet.

### Task 1: Install Active Directory Domain Services

On Windows Server:

1. Open Server Manager.
2. Select **Add Roles and Features**.
3. Select **Active Directory Domain Services**.
4. Install the required features.
5. Promote the server to a domain controller.
6. Create a new forest.
7. Choose a lab domain name, such as:

```text
lab.example.com
```

8. Complete the configuration and restart if required.

### Task 2: Explore Active Directory Users and Computers

Open:

```powershell
dsa.msc
```

Explore:

- Domain structure.
- Users.
- Computers.
- Groups.
- Organizational Units.

Create a test user:

```text
Username: alice
Department: Security
```

Create a security group:

```text
Security-Analysts
```

Add Alice to the group.

### Task 3: Create Organizational Units

Create the following structure:

```text
lab.example.com
       |
       +-- IT
       |
       +-- Security
       |
       +-- Development
```

Create test user accounts inside the appropriate OUs.

Observe how objects are organized.

### Task 4: Explore Group Policy

Open:

```powershell
gpmc.msc
```

Create a test GPO and link it to a lab OU.

Explore settings under:

- Computer Configuration.
- User Configuration.
- Windows Settings.
- Administrative Templates.

Use a harmless test configuration and observe how it applies to the domain-joined client.

Check applied policies:

```powershell
gpresult /r
```

### Task 5: Explore Domain Controller Services

On your Windows Server VM, explore:

```powershell
Get-Service NTDS
```

Check DNS:

```powershell
Get-Service DNS
```

Explore domain information:

```powershell
Get-ADDomain
```

List domain controllers:

```powershell
Get-ADDomainController -Filter *
```

These Active Directory PowerShell commands require the appropriate Active Directory module and permissions.

### Task 6: Explore Active Directory Using Linux

On Kali Linux, you can explore authorized directory information using tools such as:

- `ldapsearch`
- `smbclient`
- `enum4linux-ng`
- Impacket tools

For example, check SMB shares on your lab domain controller:

```bash
smbclient -L //DC-IP -U 'LAB\alice'
```

Replace `DC-IP` with your lab domain controller's IP address.

This command attempts to list available SMB shares using the specified account. What you can see depends on the account's permissions and server configuration.

**Your goal:** Understand how Windows domain services can be inspected from a Linux security workstation.

---

## 9. 🧠 Common Active Directory Misconceptions

| Misconception | Correct Understanding |
|---|---|
| Active Directory is just a database | It is a directory service with authentication, management, and policy capabilities |
| A domain and a forest are the same | A forest can contain multiple domains and domain trees |
| A domain controller is just a normal server | It runs AD DS and provides critical domain services |
| DNS is optional for Active Directory | DNS is essential for locating domain services |
| OUs and groups are identical | OUs organize and delegate; groups commonly manage permissions |
| Being authenticated means access to everything | Authorization still determines permitted access |
| Group Policy only controls passwords | GPOs configure many user and computer settings |
| Trust automatically grants access | Trust enables identity acceptance; resource permissions are still required |
| Multiple domain controllers are unnecessary | They provide redundancy, availability, and replication |
| Enabling Group Policy guarantees security | Incorrect policies can introduce vulnerabilities |

---

## 10. ⚡ Quick Revision

### Active Directory Components at a Glance

| Component | Main Purpose |
|---|---|
| Active Directory | Centralized directory and identity management |
| Domain | Logical directory and administrative unit |
| Domain Controller | Provides AD DS services and domain authentication |
| Forest | Highest-level logical AD structure |
| Domain Tree | Hierarchical collection of related domains |
| OU | Organizes objects and supports policy linking and delegation |
| Group | Organizes members for management and permissions |
| GPO | Centrally configures users and computers |
| DNS | Helps locate domain services |
| Kerberos | Ticket-based authentication protocol |
| LDAP | Directory access and management protocol |
| SYSVOL | Stores replicated domain files, including Group Policy templates |

### Important Commands

| Command | Purpose |
|---|---|
| `dsa.msc` | Opens Active Directory Users and Computers |
| `gpmc.msc` | Opens Group Policy Management |
| `gpedit.msc` | Opens Local Group Policy Editor |
| `gpupdate /force` | Forces Group Policy refresh |
| `gpresult /r` | Displays applied Group Policy information |
| `gpresult /h report.html` | Generates an HTML Group Policy report |
| `Get-ADDomain` | Displays domain information |
| `Get-ADUser` | Retrieves user account information |
| `Get-ADGroup` | Retrieves group information |
| `Get-ADDomainController -Filter *` | Lists domain controllers |

### Key Takeaways

1. Active Directory provides centralized identity and resource management for Windows domain networks.
2. A domain organizes users, computers, groups, and other directory objects.
3. A domain controller provides AD DS services and manages domain authentication.
4. A forest can contain multiple domains and domain trees.
5. OUs organize directory objects and support delegation and Group Policy linking.
6. Group Policy centrally configures Windows users and computers.
7. Kerberos is the primary authentication protocol in modern AD domain environments.
8. DNS is essential for locating domain controllers and other domain services.
9. Domain controllers are high-value targets because they manage critical identity infrastructure.
10. Poorly configured permissions and Group Policies can expose an entire enterprise environment.

---

## 11. 🎯 Active Recall & Exam Questions

Try answering these questions without looking at the notes.

1. What is Active Directory, and why do organizations use it?
2. What is the difference between Active Directory and AD DS?
3. Explain the difference between a workgroup and an Active Directory domain.
4. What is an Active Directory domain?
5. What is the difference between a domain, domain tree, and forest?
6. What is a Domain Controller, and what are its responsibilities?
7. What is Kerberos, and why is it important in Active Directory?
8. What are the functions of LDAP and DNS in an Active Directory environment?
9. What is the purpose of the `NTDS.dit` database?
10. What is SYSVOL?
11. What is an Organizational Unit?
12. What is the difference between an OU and a security group?
13. What is Group Policy, and why is it used?
14. Explain Computer Configuration and User Configuration.
15. What does LSDOU represent?
16. What is Group Policy inheritance?
17. Why are Domain Controllers high-value targets?
18. Explain Kerberoasting and Pass-the-Hash at a high level.
19. How can an attacker abuse excessive permissions in Active Directory?
20. List five security best practices for protecting an Active Directory environment.

<details>
<summary><strong>✅ Check Your Answers</strong></summary>

**1. Active Directory**

A Microsoft directory service that centrally manages identities, computers, groups, and resources in Windows domain networks.

**2. Active Directory vs AD DS**

Active Directory is commonly used as an umbrella term. AD DS is the specific Windows Server directory service that provides traditional domain-based functionality.

**3. Workgroup vs Domain**

A workgroup uses decentralized local account management. A domain uses centralized identity management through domain controllers.

**4. Domain**

A logical directory and administrative unit containing users, computers, groups, and other objects.

**5. Domain vs Tree vs Forest**

- Domain: Contains directory objects.
- Tree: Hierarchical collection of domains sharing a contiguous DNS namespace.
- Forest: Contains one or more domain trees sharing common AD infrastructure.

**6. Domain Controller**

A Windows Server running AD DS that stores directory information, authenticates domain identities, and supports centralized domain management.

**7. Kerberos**

A ticket-based authentication protocol used by Active Directory to authenticate users and services.

**8. LDAP and DNS**

- LDAP: Accesses and manages directory information.
- DNS: Helps clients locate domain controllers and domain services.

**9. NTDS.dit**

The main Active Directory database file containing directory objects and sensitive security-related information.

**10. SYSVOL**

A shared and replicated directory that stores domain files, including Group Policy templates and scripts.

**11. OU**

A directory container used to organize objects, link Group Policies, and delegate administrative responsibilities.

**12. OU vs Security Group**

An OU organizes directory objects and supports delegation and policy application. A security group organizes members for access control and permissions.

**13. Group Policy**

A Windows feature used to centrally manage user and computer settings.

**14. Computer vs User Configuration**

Computer Configuration applies settings to computers. User Configuration applies settings to users.

**15. LSDOU**

Local, Site, Domain, Organizational Unit.

**16. Group Policy Inheritance**

Applicable policies linked at higher levels can flow down to objects in lower-level containers.

**17. Domain Controller Security**

Domain controllers manage critical authentication and directory information. Compromise can provide extensive control over an organization's identity infrastructure.

**18. Kerberoasting and Pass-the-Hash**

- Kerberoasting: Attempts offline cracking of Kerberos service tickets to recover service-account credentials.
- Pass-the-Hash: Reuses a captured password hash in supported Windows authentication scenarios.

**19. Excessive Permissions**

An attacker may exploit unnecessary privileges to access sensitive resources, modify policies, or escalate privileges.

**20. Five Security Best Practices**

- Protect privileged accounts.
- Apply least privilege.
- Monitor domain controller activity.
- Secure service accounts.
- Audit Group Policy and group membership changes.

</details>

---

## 🏁 Final Thought

Active Directory is the foundation of identity and access management in many Windows enterprise environments.

As a cybersecurity professional, you should understand not only how administrators manage domains, users, and policies, but also how attackers target these components.

When investigating an Active Directory environment, always think about:

- Who controls the domain?
- Which accounts have privileged access?
- How are users authenticated?
- Which groups control sensitive resources?
- Which Group Policies are applied?
- Where could misconfigurations create security weaknesses?

> **If you understand Active Directory, you understand one of the most important identity attack surfaces in enterprise cybersecurity.**
