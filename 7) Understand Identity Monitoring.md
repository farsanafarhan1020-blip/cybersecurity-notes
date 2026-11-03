# 🛡️ Understanding Identity Monitoring

> **Identity Monitoring is the continuous collection, analysis, and investigation of identity-related activities to detect unauthorized access, compromised accounts, suspicious authentication, and potential security threats.**

In modern organizations, employees access applications, servers, databases, cloud platforms, and network resources using digital identities.

Every authentication attempt, login, permission change, and resource access can provide valuable security information.

Identity monitoring helps security teams understand **who accessed a system, when they accessed it, where the activity originated, and whether the activity was legitimate.**

For cybersecurity professionals, especially SOC analysts, identity monitoring is an important skill because compromised credentials are frequently used in cyberattacks.

---

## 1. 🔑 Login Events

### What Are Login Events?

Login events are records generated when users or systems attempt to access a computer, application, server, or network resource.

A login event can record whether authentication was successful or unsuccessful.

Security teams analyze these events to identify:

- Successful logins.
- Failed login attempts.
- Repeated authentication failures.
- Unusual login times.
- Logins from unexpected locations.
- Privileged account logins.
- Multiple login attempts from suspicious sources.

### Types of Login Events

**1. Successful Login**

A user provides valid authentication credentials and gains access.

Example:

```text
Username: alice
Source IP: 192.168.1.20
Time: 09:15 AM
Result: Successful
```

**2. Failed Login**

Authentication fails because the provided credentials or authentication process are invalid.

Example:

```text
Username: admin
Source IP: 192.168.1.50
Time: 09:20 AM
Result: Failed
Reason: Invalid password
```

**3. Privileged Login**

An account with administrative permissions successfully logs in.

These events are important because privileged accounts can perform sensitive operations.

**4. Remote Login**

A user accesses a system remotely, such as through Remote Desktop Protocol (RDP) or another remote access service.

Unexpected remote logins can indicate compromised credentials or unauthorized access.

### Windows Login Event IDs

Windows records login-related activities in the Security event log.

| Event ID | Description |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Account logged off |
| 4647 | User initiated logoff |
| 4648 | Logon attempted using explicit credentials |
| 4672 | Special privileges assigned to a new logon |
| 4778 | Remote Desktop session reconnected |
| 4779 | Remote Desktop session disconnected |
| 4800 | Workstation locked |
| 4801 | Workstation unlocked |

**Important:** These events describe different stages of authentication and session activity. A successful logon does not always mean a person entered a password interactively.

### Understanding Logon Types

Windows Event ID 4624 includes a Logon Type field.

This field helps analysts understand how a logon occurred.

| Logon Type | Meaning |
|---|---|
| 2 | Interactive logon |
| 3 | Network logon |
| 4 | Batch logon |
| 5 | Service logon |
| 7 | Workstation unlock |
| 8 | NetworkCleartext logon |
| 9 | NewCredentials |
| 10 | RemoteInteractive logon, commonly RDP |
| 11 | CachedInteractive logon |

Examples:

- Type 2: A user signs in directly at a computer.
- Type 3: A user accesses a network resource.
- Type 5: A Windows service starts under an account.
- Type 10: A user connects through Remote Desktop.

A logon type is not inherently malicious. Its meaning depends on the account, source, destination, and surrounding activity.

### Identifying Suspicious Login Patterns

Consider this example:

```text
09:00 AM → alice → Successful → Office Network
09:05 AM → alice → Failed     → Unknown IP
09:06 AM → alice → Failed     → Unknown IP
09:07 AM → alice → Failed     → Unknown IP
09:08 AM → alice → Successful → Unknown IP
```

This pattern deserves investigation because the account experienced multiple failed logins followed by a successful login from an unexpected source.

Possible explanations include:

- A legitimate user entering the wrong password.
- A password-guessing attack.
- A compromised account.
- A legitimate remote connection.

An analyst must investigate before deciding that the activity is malicious.

### Viewing Login Events Using Event Viewer

Open Event Viewer:

```powershell
eventvwr.msc
```

Navigate to:

```text
Windows Logs
    |
    +-- Security
           |
           +-- Event ID 4624
           |
           +-- Event ID 4625
           |
           +-- Event ID 4672
```

You can filter events by Event ID to make investigation easier.

### Viewing Login Events Using PowerShell

Find recent successful logons:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4624
} -MaxEvents 20
```

Find recent failed logons:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4625
} -MaxEvents 20
```

Display selected event details:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4625
} -MaxEvents 10 |
Select-Object TimeCreated, Id, MachineName, Message
```

### What Should a SOC Analyst Check?

When investigating a login event, examine:

1. Username.
2. Timestamp.
3. Source IP address.
4. Destination computer.
5. Logon type.
6. Authentication package.
7. Failure reason or status code.
8. Whether the activity matches the user's normal behavior.

<text weight="semibold" color="success">Key takeaway:</text> Login events provide evidence about authentication activity and help security analysts identify unusual access patterns.

---

## 2. 📋 Authentication Logs

### What Are Authentication Logs?

Authentication logs are records generated during the process of verifying the identity of a user, device, or service.

They provide information about authentication attempts, authentication methods, results, and related security events.

Authentication logs may be generated by:

- Windows operating systems.
- Active Directory Domain Controllers.
- Linux systems.
- Web applications.
- VPN gateways.
- Cloud identity providers.
- Multi-Factor Authentication systems.

### Login Events vs Authentication Logs

These terms are related, but they are not exactly the same.

| Login Events | Authentication Logs |
|---|---|
| Record login and session-related activities | Record identity verification activities |
| Often include successful and failed logons | May include additional authentication details |
| Focus on access and session activity | Focus on how identity was verified |
| Example: Windows Event ID 4624 | Example: Kerberos authentication events |

A login event can be part of a broader authentication process.

### Important Authentication Mechanisms to Monitor

**1. Password Authentication**

The user provides a username and password.

Security analysts monitor:

- Repeated password failures.
- Password-spraying patterns.
- Suspicious password changes.
- Unexpected password resets.

**2. Kerberos Authentication**

Kerberos is the primary authentication protocol used in modern Active Directory domains.

It uses tickets to support authentication and access to domain resources.

Important Windows Security event IDs include:

| Event ID | Description |
|---|---|
| 4768 | Kerberos Ticket Granting Ticket (TGT) requested |
| 4769 | Kerberos service ticket requested |
| 4770 | Kerberos service ticket renewed |
| 4771 | Kerberos pre-authentication failed |
| 4776 | NTLM credential validation |

These events are especially useful when investigating Active Directory authentication activity.

**3. NTLM Authentication**

NTLM is an older Windows authentication protocol that may still exist in enterprise environments.

Security teams monitor NTLM activity to identify:

- Unexpected NTLM usage.
- Authentication from unusual systems.
- Potential credential-relay risks.
- Legacy authentication dependencies.

**4. Multi-Factor Authentication (MFA)**

MFA requires additional verification beyond a password.

For example:

```text
Username + Password
        |
        v
   Authentication
        |
        v
   MFA Challenge
        |
        v
  Second Verification
        |
        v
     Access Granted
```

MFA logs can help identify:

- Successful MFA challenges.
- Failed MFA challenges.
- Repeated push notifications.
- Unexpected MFA registrations.
- Changes to authentication methods.

### Understanding Kerberos Authentication Logs

Kerberos uses tickets rather than sending a user's password to every service.

A simplified process:

```text
          User Login
              |
              v
       Authentication
        Service (AS)
              |
              v
       Ticket Granting
        Ticket (TGT)
              |
              v
       Ticket Granting
        Service (TGS)
              |
              v
        Service Ticket
              |
              v
       Access to Resource
```

Security analysts monitor Kerberos events to identify unusual ticket requests and potential attacks.

For example:

- Unusual volumes of service-ticket requests may deserve investigation.
- Repeated Kerberos pre-authentication failures may indicate password guessing.
- Unexpected authentication patterns involving service accounts may indicate misuse.

However, a high number of ticket requests or a failed authentication event is not automatically malicious.

### Important Authentication Event IDs

| Event ID | Activity | Why It Matters |
|---|---|---|
| 4768 | TGT requested | Helps monitor Kerberos authentication |
| 4769 | Service ticket requested | Helps investigate service access and Kerberoasting indicators |
| 4770 | Ticket renewed | Helps understand ticket activity |
| 4771 | Kerberos pre-authentication failed | Can reveal password-guessing activity |
| 4776 | NTLM credential validation | Helps monitor NTLM authentication |

These events are generally recorded on the relevant Domain Controller when the appropriate auditing is enabled.

### Viewing Kerberos Authentication Events

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4768,4769,4771
} -MaxEvents 30
```

For NTLM credential validation events:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4776
} -MaxEvents 20
```

### Linux Authentication Logs

Linux systems also generate authentication-related logs.

Common locations include:

| File | Purpose |
|---|---|
| `/var/log/auth.log` | Authentication and authorization-related events on Debian-based systems |
| `/var/log/secure` | Authentication-related events on many RHEL-based systems |
| `/var/log/syslog` | General system messages on some distributions |
| `journalctl` | Queries logs collected by systemd-journald |

Example:

```bash
sudo tail -f /var/log/auth.log
```

This displays new authentication-related log entries as they are written on systems that use this file.

Search for SSH authentication failures:

```bash
sudo grep "Failed password" /var/log/auth.log
```

Search for successful SSH logins:

```bash
sudo grep "Accepted" /var/log/auth.log
```

On systems that use journald:

```bash
sudo journalctl -u ssh
```

The service may be named `sshd` instead of `ssh`, depending on the distribution.

### What Should an Analyst Look For?

- Repeated authentication failures.
- Successful login after many failures.
- Unusual Kerberos ticket requests.
- Unexpected NTLM authentication.
- Authentication from unfamiliar devices.
- MFA fatigue or repeated MFA challenges.
- Authentication involving disabled accounts.
- Suspicious service-account activity.

<text weight="semibold" color="success">Key takeaway:</text> Authentication logs help analysts understand how identities are verified and detect suspicious authentication behavior across different systems.

---

## 3. 🔍 Access Monitoring

### What Is Access Monitoring?

Access monitoring is the process of observing and analyzing what users and systems do after authentication.

Authentication tells us who a user is. Access monitoring helps us understand what that identity accesses and what actions it performs.

Examples include:

- Accessing files and folders.
- Reading sensitive documents.
- Modifying system configurations.
- Accessing databases.
- Using administrative tools.
- Changing permissions.
- Accessing restricted applications.

### Authentication vs Access Monitoring

| Authentication Monitoring | Access Monitoring |
|---|---|
| Focuses on identity verification | Focuses on resource access and actions |
| Tracks login attempts | Tracks what users do after logging in |
| Example: Password authentication | Example: Accessing a sensitive folder |
| Helps detect compromised credentials | Helps detect unauthorized or unusual resource usage |

### Example: Monitoring File Access

Imagine an organization stores sensitive financial documents in a shared folder.

```text
D:\Company\Finance
```

A user from the Finance Department accesses the folder regularly.

One day, an account from an unrelated department attempts to access it.

This activity may deserve investigation.

Example:

```text
User: bob
Resource: Finance/Salary.xlsx
Action: Access Attempt
Result: Access Denied
Time: 11:30 PM
```

An access-denied event does not prove an attack occurred, but repeated attempts to access restricted resources may indicate suspicious behavior.

### Important Windows File Access Events

| Event ID | Description |
|---|---|
| 4663 | An attempt was made to access an object |
| 4656 | A handle to an object was requested |
| 4660 | An object was deleted |
| 4670 | Permissions on an object were changed |

These events can help investigate file and object access.

**Important:** Object Access auditing and appropriate SACL entries must be configured for relevant events to be generated. Enabling auditing without carefully selecting objects can create large amounts of log data.

### Monitoring Privileged Access

Administrators can perform sensitive actions such as:

- Creating users.
- Changing group membership.
- Modifying Group Policies.
- Accessing sensitive files.
- Changing security settings.

Security teams should monitor these activities carefully.

Example:

```text
Account: admin01
Action: Added user to Domain Admins
Time: 02:15 AM
Source: Workstation-05
```

An unexpected change to Domain Admin membership should be investigated.

### Monitoring Access to Shared Resources

Organizations can monitor access to:

- File servers.
- Network shares.
- Databases.
- Internal applications.
- Administrative systems.

For SMB file shares, Windows provides events such as:

| Event ID | Description |
|---|---|
| 5140 | A network share was accessed |
| 5145 | A network share object was checked to see whether the client could be granted access |

These events can help identify access to sensitive network shares.

### Monitoring User Behavior

Access monitoring can also identify unusual behavior.

Example:

A normal employee generally accesses:

- Email.
- HR applications.
- Department documents.

Suddenly, the account starts accessing:

- Sensitive financial records.
- Administrative shares.
- Multiple servers.
- Large numbers of files.

This change in behavior may indicate account compromise or misuse.

### User and Entity Behavior Analytics (UEBA)

UEBA stands for User and Entity Behavior Analytics.

It uses behavioral patterns and analytics to identify unusual activity involving users, devices, and other entities.

Example:

```text
Normal Behavior:
Alice logs in from Office PC
Accesses HR Application
Works during office hours

Unusual Behavior:
Alice logs in from an unfamiliar device
Accesses Finance Server
Downloads unusual amounts of data
```

UEBA tools may flag this behavior for investigation.

However, behavioral analytics can generate false positives. Alerts must be evaluated using context and supporting evidence.

### Access Monitoring Best Practices

- Monitor access to sensitive resources.
- Audit administrative operations.
- Review denied access attempts.
- Track permission changes.
- Monitor large or unusual data access.
- Use appropriate SACL auditing.
- Correlate access activity with login information.
- Investigate unusual behavior using multiple sources.

<text weight="semibold" color="success">Key takeaway:</text> Access monitoring provides visibility into what authenticated users actually do, helping detect unauthorized activity and misuse of legitimate accounts.

---

## 4. 🚨 Suspicious Activities

### What Are Suspicious Activities?

Suspicious activities are behaviors that may indicate an attack, compromised account, policy violation, or unauthorized access.

In identity monitoring, suspicious activity often involves unusual login patterns, credential misuse, unexpected permission changes, or abnormal resource access.

Security analysts use logs, alerts, and contextual information to determine whether an activity represents a real threat.

### Common Suspicious Identity Activities

#### 1. Multiple Failed Login Attempts

An account experiences many failed authentication attempts within a short period.

Possible explanation:

- Brute-force attack.
- Password spraying.
- User entering incorrect credentials.
- Misconfigured application.

Relevant event:

```text
4625 → Failed logon
```

#### 2. Successful Login After Repeated Failures

An account experiences repeated authentication failures followed by a successful login.

Example:

```text
09:00 → Failed
09:01 → Failed
09:02 → Failed
09:03 → Successful
```

This may indicate that an attacker eventually guessed or obtained the correct password.

#### 3. Unusual Login Location

An account logs in from an unfamiliar network, device, or geographic location.

Example:

```text
Normal:
India → Office Network

Unusual:
Unknown Country → Unknown IP
```

This may indicate compromised credentials, but VPNs, travel, and network routing can also explain location differences.

#### 4. Impossible Travel

A user appears to authenticate from two geographically distant locations within a time period that makes normal travel impossible.

Example:

```text
10:00 AM → Location A
10:15 AM → Location B
```

This may indicate credential sharing, token theft, or compromised credentials.

However, VPN gateways, proxies, and inaccurate IP geolocation can generate false positives.

#### 5. Password Spraying

An attacker tries a small number of common passwords against many accounts.

Example:

```text
Password: Welcome123

alice   → Failed
bob     → Failed
charlie → Failed
david   → Failed
```

Unlike brute force, password spraying distributes attempts across many accounts to reduce the likelihood of triggering individual account lockouts.

#### 6. Privileged Group Modification

An unexpected account is added to a powerful group.

Example:

```text
Group: Domain Admins
Added Account: employee01
Time: 01:30 AM
```

Relevant event:

```text
4728 → Member added to a global security group
```

This is important because privileged group membership can provide extensive access.

#### 7. Suspicious Account Creation

An unexpected account is created, especially outside normal administrative processes.

Example:

```text
Username: support_backup
Created: 03:00 AM
Created By: Unknown Administrator
```

Relevant event:

```text
4720 → User account created
```

#### 8. Abnormal Service Account Activity

A service account starts logging in from unexpected computers or accessing resources outside its normal responsibilities.

Potential causes:

- Compromised service credentials.
- Misconfigured applications.
- Unauthorized use of the service identity.

#### 9. Unusual Access to Sensitive Resources

A user suddenly accesses large numbers of files or restricted systems.

This may indicate:

- Data theft.
- Insider misuse.
- Compromised credentials.
- Unauthorized reconnaissance.

#### 10. Suspicious Authentication Protocol Usage

Unexpected or excessive use of legacy authentication mechanisms may indicate security weaknesses or malicious activity.

Examples include unusual NTLM authentication and abnormal Kerberos ticket requests.

### Suspicious Activity Detection Table

| Activity | Possible Threat | Useful Evidence |
|---|---|---|
| Repeated failed logins | Brute force / Password spraying | 4625 |
| Login after many failures | Account compromise | 4625 + 4624 |
| Unusual privileged login | Stolen administrator credentials | 4624 + 4672 |
| Unexpected account creation | Persistence / Unauthorized access | 4720 |
| Privileged group modification | Privilege escalation | 4728 / 4732 / 4756 |
| Unusual Kerberos requests | Possible Kerberos abuse | 4768 / 4769 / 4771 |
| Unexpected file access | Data exposure / Insider misuse | 4663 / 5140 / 5145 |
| Account lockout | Password guessing / Misconfiguration | 4740 |

### Indicators of Compromise (IoCs)

An Indicator of Compromise is an observable piece of evidence that may suggest a system or account has been compromised.

Examples:

- Known malicious IP addresses.
- Suspicious domain names.
- Unexpected executable files.
- Unusual authentication sources.
- Unexpected privileged accounts.
- Known malicious file hashes.

IoCs are useful, but not every suspicious event is an IoC, and the absence of known IoCs does not prove a system is safe.

### False Positives

A false positive occurs when a legitimate activity is incorrectly identified as suspicious.

Example:

An employee travels to another country and logs in from a new IP address.

A monitoring system may flag the login as suspicious even though it is legitimate.

SOC analysts must validate alerts using:

- User information.
- Device information.
- IP reputation.
- Authentication history.
- Business context.
- Related security events.

### Suspicious Activity Investigation Principles

1. Do not assume every alert is an attack.
2. Identify the affected user and system.
3. Check the timeline.
4. Compare the activity with normal behavior.
5. Correlate related events.
6. Determine the potential impact.
7. Escalate when evidence supports a security incident.
8. Document findings.

<text weight="semibold" color="success">Key takeaway:</text> Suspicious activity detection is about recognizing unusual patterns and validating them with evidence, not simply reacting to individual log events.

---

## 5. 🖥️ SOC Investigation Workflows

### What Is a SOC?

SOC stands for Security Operations Center.

A SOC is a team responsible for monitoring, detecting, investigating, and responding to cybersecurity threats within an organization.

SOC analysts use security tools, logs, alerts, and threat intelligence to protect systems and data.

### Main SOC Responsibilities

- Monitor security events.
- Investigate suspicious activities.
- Analyze authentication and access logs.
- Detect compromised accounts.
- Identify potential attacks.
- Escalate security incidents.
- Support containment and recovery.
- Document investigations.

### SOC Analyst Levels

Organizations use different SOC structures, but a common model includes:

| Level | Responsibilities |
|---|---|
| L1 Analyst | Initial alert monitoring, validation, and triage |
| L2 Analyst | Detailed investigation and incident analysis |
| L3 Analyst | Advanced threat hunting, detection engineering, and complex investigations |

These levels are organizational conventions, not universal job definitions.

### What Is Alert Triage?

Alert triage is the initial process of reviewing a security alert to determine its importance and whether further investigation is required.

Example:

A SIEM generates an alert:

```text
Alert: Multiple Failed Logins
User: alice
Source IP: 203.0.113.50
Failed Attempts: 25
Time: 10 Minutes
```

An L1 analyst checks:

- Is the source IP known?
- Is the account active?
- Are the attempts coming from one or multiple systems?
- Are other users affected?
- Was there a successful login afterward?

Based on the evidence, the analyst decides whether to close, monitor, or escalate the alert.

### Standard SOC Investigation Workflow

```text
             Security Alert
                    |
                    v
               Alert Triage
                    |
                    v
            Validate the Alert
                    |
                    v
          Collect Relevant Evidence
                    |
                    v
          Correlate Security Events
                    |
                    v
          Determine Scope and Impact
                    |
                    v
          Classify the Incident
                    |
                    v
           Containment / Escalation
                    |
                    v
           Recovery and Monitoring
                    |
                    v
          Documentation and Lessons
```

### Step 1: Alert Detection

The monitoring system detects suspicious behavior.

Example:

- Multiple failed logins.
- Unexpected privileged access.
- Suspicious authentication from an unknown source.

The alert may originate from a SIEM, endpoint security platform, identity provider, or other monitoring tool.

### Step 2: Alert Triage

The analyst reviews the alert and determines its initial priority.

Important questions:

- Which account is affected?
- Which system generated the event?
- When did it happen?
- Is the source familiar?
- Does the event match known attack patterns?

### Step 3: Evidence Collection

The analyst collects relevant information.

Examples:

- Windows Security logs.
- Domain Controller authentication events.
- VPN logs.
- Firewall logs.
- Endpoint detection alerts.
- Identity provider logs.
- User and group information.

The purpose is to build an accurate picture of the activity.

### Step 4: Event Correlation

Event correlation means connecting multiple related events to identify a larger activity pattern.

Consider this example:

```text
10:00 → Multiple Failed Logins
10:05 → Successful Login
10:07 → Privileged Group Access
10:10 → Sensitive File Access
10:15 → Unusual Remote Session
```

Individually, some events may look ordinary.

Together, they may indicate a compromised account.

Correlation helps analysts understand the complete sequence of activity.

### Step 5: Determine Scope and Impact

The analyst identifies what may have been affected.

Questions include:

- Was only one account compromised?
- Were privileged accounts involved?
- Were multiple computers accessed?
- Was sensitive information exposed?
- Were security settings changed?
- Is the attacker still active?

### Step 6: Incident Classification

The activity is classified according to its severity and impact.

Example:

| Severity | Description |
|---|---|
| Informational | Activity worth recording |
| Low | Minor suspicious activity with limited impact |
| Medium | Potential security incident requiring investigation |
| High | Strong evidence of compromise or significant risk |
| Critical | Severe compromise affecting critical systems or infrastructure |

Severity definitions should follow the organization's incident classification policy.

### Step 7: Containment

If the investigation confirms or strongly indicates account compromise, the security team takes action to limit the damage.

Possible actions include:

- Disable or restrict the compromised account.
- Revoke affected sessions and tokens.
- Reset credentials.
- Remove unauthorized group memberships.
- Block malicious network sources where appropriate.
- Isolate affected endpoints.

Containment must be coordinated carefully to avoid disrupting critical services or destroying investigation evidence.

### Step 8: Recovery

After containment, the organization restores secure operations.

Activities may include:

- Restoring legitimate account access.
- Resetting affected credentials.
- Removing unauthorized changes.
- Checking endpoint integrity.
- Reviewing affected systems.
- Monitoring for recurring suspicious activity.

### Step 9: Documentation

The analyst records:

- Alert details.
- Affected accounts.
- Relevant timestamps.
- Source and destination systems.
- Evidence collected.
- Investigation findings.
- Actions taken.
- Final incident classification.

Good documentation supports future investigations, auditing, and incident response improvements.

---

## 6. 🧰 Identity Monitoring Tools

SOC analysts use different tools to collect and investigate identity-related security information.

| Tool | Main Purpose |
|---|---|
| Windows Event Viewer | Examines Windows security logs |
| PowerShell | Queries and analyzes Windows events |
| Microsoft Sentinel | SIEM and security analytics |
| Splunk | Log collection, search, and correlation |
| Microsoft Defender for Identity | Detects identity-related threats in Active Directory environments |
| Wazuh | Open-source security monitoring and log analysis |
| Sysmon | Provides detailed Windows system activity telemetry |
| Elastic Security | Security analytics, detection, and investigation |

### SIEM vs EDR vs XDR

These tools are related but serve different purposes.

| Technology | Main Focus |
|---|---|
| SIEM | Collects and correlates security data from multiple sources |
| EDR | Monitors and responds to endpoint threats |
| XDR | Correlates security information across multiple security layers |
| UEBA | Identifies unusual user and entity behavior |
| SOAR | Automates security workflows and response actions |

Example:

```text
Windows Logs ──────┐
                   |
VPN Logs ──────────|
                   |
Firewall Logs ─────|----> SIEM
                   |        |
Identity Logs ─────┘        v
                        Correlation
                            |
                            v
                           Alert
                            |
                            v
                      SOC Investigation
```

---

## 7. 🧪 Hands-on Lab: Identity Monitoring and Investigation

You can practice identity monitoring using your Windows system, Active Directory lab, and Kali Linux VM.

Use only your own lab systems or environments where you have explicit authorization.

### Lab 1: Generate and Identify Login Events

First, open Event Viewer:

```powershell
eventvwr.msc
```

Navigate to:

```text
Windows Logs → Security
```

Perform these activities on your test Windows account:

1. Lock and unlock the workstation.
2. Log out and log in again.
3. Attempt one login with an incorrect password.
4. Examine the generated events.

Find:

- 4624 — Successful logon.
- 4625 — Failed logon.
- 4634 — Logoff.
- 4800 — Workstation locked.
- 4801 — Workstation unlocked.

**Task:** Identify the timestamp, account name, logon type, and event result.

### Lab 2: Analyze Failed Logins with PowerShell

Run:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4625
} -MaxEvents 20 |
Select-Object TimeCreated, Id, Message
```

**Task:** Examine the output and identify:

- Account name.
- Source information, if available.
- Failure reason.
- Timestamp.

### Lab 3: Investigate Kerberos Events

On your Active Directory Domain Controller, run:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4768,4769,4771
} -MaxEvents 30
```

**Task:** Identify TGT requests, service-ticket requests, and failed Kerberos pre-authentication events.

You may need to enable the appropriate advanced audit policy and generate authentication activity in your lab.

### Lab 4: Monitor User Account Changes

Create a test account in your lab and examine the event log.

Search for:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4720
} -MaxEvents 10
```

**Task:** Identify the account creation event and the account that performed the action, where recorded.

### Lab 5: Investigate Privileged Group Changes

On your Domain Controller:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Security'
    Id = 4728,4732,4756
} -MaxEvents 20
```

**Task:** Determine whether any accounts were added to security groups and identify the relevant group and actor.

### Lab 6: Monitor Linux Authentication

On Kali Linux:

```bash
sudo tail -f /var/log/auth.log
```

Open another terminal and perform a controlled SSH login test against your own lab machine.

Search for failed authentication:

```bash
sudo grep "Failed password" /var/log/auth.log
```

Search for successful authentication:

```bash
sudo grep "Accepted" /var/log/auth.log
```

**Task:** Compare successful and failed SSH authentication records.

### Lab 7: Build a Mini SOC Investigation

Use the following fictional incident data:

```text
Incident ID: SOC-001

10:00 AM
User: alice
Activity: Failed Login
Source: 192.0.2.50

10:02 AM
User: alice
Activity: Failed Login
Source: 192.0.2.50

10:04 AM
User: alice
Activity: Failed Login
Source: 192.0.2.50

10:06 AM
User: alice
Activity: Successful Login
Source: 192.0.2.50

10:08 AM
User: alice
Activity: Sensitive File Access
Resource: Finance/Salary.xlsx

10:10 AM
User: alice
Activity: Unusual Remote Access
Destination: Server-01
```

Investigate this incident using the SOC workflow.

Answer these questions:

1. What is the first suspicious activity?
2. Which event indicates successful authentication?
3. Why is the sensitive file access important?
4. What additional logs would you collect?
5. Could the account be compromised?
6. What containment actions would you recommend if compromise is confirmed?
7. How would you document the incident?

**Expected outcome:** Produce a short investigation report with evidence, findings, impact, and recommended actions.

---

## 8. 📝 SOC Investigation Report Template

Use this structure when documenting identity-related incidents.

```markdown
# SOC Investigation Report

## 1. Incident Details
- Incident ID:
- Date and Time:
- Analyst:
- Severity:
- Status:

## 2. Alert Summary
- Alert Name:
- Affected User:
- Source IP:
- Destination System:
- Detection Source:

## 3. Evidence Collected
- Login Events:
- Authentication Logs:
- Access Events:
- Related Security Alerts:

## 4. Timeline
- Event 1:
- Event 2:
- Event 3:

## 5. Investigation Findings
- What happened?
- Was the activity expected?
- Is there evidence of compromise?

## 6. Impact Assessment
- Affected Accounts:
- Affected Systems:
- Data Exposure:
- Privilege Changes:

## 7. Response Actions
- Containment:
- Credential Reset:
- Session Revocation:
- Recovery:

## 8. Final Conclusion
- False Positive / Suspicious Activity / Confirmed Incident

## 9. Recommendations
- Preventive Controls:
- Monitoring Improvements:
- Follow-up Actions:
```

---

## 9. ⚡ Quick Revision

### Important Concepts

| Concept | Remember |
|---|---|
| Identity Monitoring | Observing and analyzing identity-related activity |
| Login Event | Records login or session activity |
| Authentication Log | Records identity verification activity |
| Access Monitoring | Tracks resource access and user actions |
| Suspicious Activity | Behavior that may indicate a threat |
| Event Correlation | Connecting related events to identify patterns |
| SIEM | Collects and analyzes security events |
| UEBA | Detects unusual user and entity behavior |
| SOC | Security Operations Center |
| Alert Triage | Initial review and prioritization of alerts |
| Incident Response | Process for handling security incidents |
| False Positive | Legitimate activity incorrectly flagged as suspicious |
| IoC | Observable evidence that may indicate compromise |

### Important Windows Event IDs

| Event ID | Activity |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Account logged off |
| 4648 | Explicit credentials used |
| 4672 | Special privileges assigned |
| 4720 | User account created |
| 4728 | Member added to global security group |
| 4732 | Member added to local security group |
| 4740 | Account locked out |
| 4768 | Kerberos TGT requested |
| 4769 | Kerberos service ticket requested |
| 4771 | Kerberos pre-authentication failed |
| 4776 | NTLM credential validation |
| 4663 | Object access attempt |
| 5140 | Network share accessed |
| 5145 | Network share object access check |

### SOC Investigation Workflow

```text
Detection
   ↓
Triage
   ↓
Validation
   ↓
Evidence Collection
   ↓
Event Correlation
   ↓
Scope and Impact
   ↓
Classification
   ↓
Containment
   ↓
Recovery
   ↓
Documentation
```

### Key Takeaways

1. Login events show when users and systems attempt to access resources.
2. Authentication logs reveal how identities are verified.
3. Access monitoring tracks what users do after authentication.
4. Suspicious activity must be validated using supporting evidence.
5. Windows Event IDs are valuable for identity investigations.
6. SIEM platforms correlate logs from multiple systems.
7. SOC analysts follow structured workflows to investigate alerts.
8. Incident documentation is essential for accountability and future prevention.

---

## 10. 🎯 Active Recall & Exam Questions

Try answering these questions without looking at the notes.

1. What is identity monitoring?
2. What information can a login event contain?
3. Differentiate between successful and failed login events.
4. What is the difference between a login event and an authentication log?
5. What are the different Windows logon types?
6. What is the purpose of Event ID 4624?
7. What does Event ID 4625 indicate?
8. Explain the purpose of Kerberos events 4768 and 4769.
9. What is NTLM credential validation?
10. What is access monitoring?
11. Differentiate between authentication monitoring and access monitoring.
12. What is the purpose of Windows Event ID 4663?
13. What is UEBA?
14. Explain five examples of suspicious identity activities.
15. What is an Indicator of Compromise?
16. What is a false positive?
17. What is a SOC?
18. Explain the responsibilities of L1, L2, and L3 SOC analysts.
19. What is alert triage?
20. Explain the complete SOC investigation workflow.
21. What is event correlation, and why is it important?
22. What information should a SOC analyst include in an incident report?

<details>
<summary><strong>✅ Check Your Answers</strong></summary>

**1. Identity Monitoring**

The continuous collection and analysis of identity-related activities to detect suspicious behavior and unauthorized access.

**2. Login Event Information**

Username, timestamp, source IP, destination system, logon type, and authentication result, depending on the event.

**3. Successful vs Failed Login**

A successful login means authentication succeeded. A failed login means the authentication attempt was unsuccessful.

**4. Login Event vs Authentication Log**

A login event records login or session activity. Authentication logs provide information about identity verification processes.

**5. Windows Logon Types**

Examples include interactive (2), network (3), batch (4), service (5), unlock (7), and RemoteInteractive (10).

**6. Event ID 4624**

Records a successful logon.

**7. Event ID 4625**

Records a failed logon.

**8. Kerberos Events 4768 and 4769**

4768 records a TGT request. 4769 records a service-ticket request.

**9. NTLM Credential Validation**

The process of validating credentials through the NTLM authentication mechanism. Event ID 4776 can record related validation activity.

**10. Access Monitoring**

Monitoring which resources users access and what actions they perform.

**11. Authentication vs Access Monitoring**

Authentication monitoring checks identity verification. Access monitoring examines resource access and user actions.

**12. Event ID 4663**

Records an attempt to access an object when the relevant auditing is configured.

**13. UEBA**

User and Entity Behavior Analytics. It identifies unusual behavior involving users, devices, and other entities.

**14. Suspicious Identity Activities**

- Repeated failed logins.
- Successful login after repeated failures.
- Unexpected privileged group changes.
- Unusual login locations.
- Unexpected sensitive resource access.

**15. IoC**

An observable indicator that may suggest a security compromise.

**16. False Positive**

A legitimate activity incorrectly identified as suspicious.

**17. SOC**

Security Operations Center. A team responsible for monitoring, detecting, investigating, and responding to security threats.

**18. SOC Levels**

- L1: Initial monitoring and alert triage.
- L2: Detailed investigation and analysis.
- L3: Advanced threat hunting and complex security investigations.

**19. Alert Triage**

The initial process of reviewing, validating, and prioritizing security alerts.

**20. SOC Investigation Workflow**

Detection → Triage → Validation → Evidence Collection → Correlation → Scope and Impact → Classification → Containment → Recovery → Documentation.

**21. Event Correlation**

Connecting related events from different sources to understand a larger security activity or attack pattern.

**22. Incident Report**

It should include incident details, alert summary, evidence, timeline, findings, impact, response actions, conclusion, and recommendations.

</details>

---

## 🏁 Final Thought

Identity monitoring gives security teams visibility into how users and systems authenticate, access resources, and behave across an organization's environment.

A SOC analyst must go beyond simply reading logs. They must connect events, recognize unusual patterns, investigate evidence, and determine whether an actual security incident has occurred.

> **A single log event provides information. Correlated identity events reveal the bigger security picture.**
