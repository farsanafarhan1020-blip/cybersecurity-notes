# 🧪 Hands-on Tasks — Identity & Access Management (IAM)

> **Practical IAM security** involves analyzing authentication logs, reviewing user permissions, investigating suspicious login activities, identifying identity-related risks, and documenting security findings.

These tasks simulate the responsibilities of an identity security analyst or SOC analyst working in a real organization.

---

## 1. 🔐 Analyze Authentication Logs

Authentication logs record attempts to access systems. They help security analysts understand who logged in, when they logged in, where the login originated, and whether authentication succeeded.

### Objective

Analyze authentication records to identify normal and suspicious login activities.

### Step 1: Understand Authentication Log Fields

| Field | Description |
|---|---|
| Timestamp | Date and time of the login |
| Username | Account attempting authentication |
| Source IP | IP address from which the request originated |
| Event Type | Login success or failure |
| Status | Authentication result |
| Location | Approximate geographical location |
| Authentication Method | Password, MFA, Kerberos, etc. |

### Step 2: Analyze Sample Logs

Use this fictional authentication dataset:

| Time | User | Source IP | Activity | Status |
|---|---|---|---|---|
| 09:00 | alice | 192.168.1.10 | Login | Success |
| 09:05 | bob | 192.168.1.15 | Login | Success |
| 09:10 | admin | 203.0.113.25 | Login | Failed |
| 09:11 | admin | 203.0.113.25 | Login | Failed |
| 09:12 | admin | 203.0.113.25 | Login | Failed |
| 09:15 | alice | 198.51.100.20 | Login | Success |
| 09:20 | bob | 192.168.1.15 | Login | Success |

*Note: The IP addresses in this dataset are reserved for documentation and examples. They are not real attacker addresses.*

### Step 3: Identify Suspicious Patterns

Look for:

- Multiple failed login attempts.
- Repeated attempts against privileged accounts.
- Successful logins after several failures.
- Unexpected login locations or devices.
- Login activity outside normal working hours.
- Unusual authentication methods.

### Step 4: Investigate Your Findings

Answer these questions:

1. Which account experienced repeated authentication failures?
2. Which source IP generated those attempts?
3. Could this activity indicate a brute-force attack?
4. Which events require further investigation?
5. What additional evidence would you collect?

### Useful Tools

- Windows Event Viewer.
- PowerShell.
- Linux authentication logs.
- Splunk.
- Wazuh.
- Microsoft Sentinel.

**Windows Event IDs to remember:**

| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Account logged off |
| 4648 | Logon using explicit credentials |
| 4672 | Special privileges assigned to a new logon |

---

## 2. 👥 Review Access Control Scenarios

Access control determines what resources users can access and what actions they can perform.

An IAM analyst must ensure that users have only the permissions required for their responsibilities.

### Objective

Identify excessive permissions, inappropriate access, and violations of the Principle of Least Privilege.

### Step 1: Review User Permissions

Consider the following fictional organization:

| User | Department | Assigned Permissions |
|---|---|---|
| Alice | HR | HR Database |
| Bob | Finance | Finance Database, Domain Admin |
| Charlie | IT | Domain Admin |
| David | Marketing | Marketing Resources, Finance Database |
| Emma | HR | HR Database, IT Administration |

### Step 2: Identify Access Control Problems

Review the table and identify:

- Users with unnecessary administrative privileges.
- Users accessing resources outside their department.
- Permissions that may violate least privilege.
- Accounts requiring further investigation.

### Step 3: Recommend Improvements

Example recommendations:

| User | Potential Issue | Recommendation |
|---|---|---|
| Bob | Domain Admin access may be excessive | Verify business need and review privilege |
| David | Finance Database access may be unnecessary | Confirm authorization with Finance |
| Emma | IT Administration access requires justification | Review role and remove unnecessary access |

These are potential issues, not confirmed violations. Job responsibilities and approved exceptions must be checked before changing permissions.

### Step 4: Understand Access Control Models

| Model | Main Concept |
|---|---|
| RBAC | Permissions are assigned based on roles |
| ABAC | Access decisions use attributes |
| DAC | Resource owners control access |
| MAC | Access is controlled through centrally enforced security labels and policies |

### Practical Questions

1. Why should ordinary users not have Domain Admin privileges?
2. How does RBAC simplify permission management?
3. What is the Principle of Least Privilege?
4. Why should access reviews happen regularly?
5. What risks are created by excessive permissions?

---

## 3. 🚨 Investigate Suspicious Login Activities

Suspicious login investigations help determine whether an account is being attacked or has already been compromised.

### Objective

Investigate authentication patterns and build an incident timeline using available evidence.

### Step 1: Analyze the Incident

Consider this fictional scenario:

A user reports unusual account activity. The security team discovers the following events:

| Time | Activity | Observation |
|---|---|---|
| 08:30 | Failed login | Incorrect password |
| 08:31 | Failed login | Incorrect password |
| 08:32 | Failed login | Incorrect password |
| 08:35 | Successful login | Authentication succeeded |
| 08:40 | Login from unusual location | Requires verification |
| 08:45 | Sensitive file accessed | Requires investigation |
| 08:50 | Privileged group modification | High-priority event |

### Step 2: Identify Possible Attack Techniques

The activity may indicate:

- **Brute-force attack:** Repeated password guesses against an account.
- **Credential compromise:** An attacker may have obtained valid credentials.
- **Account takeover:** Unauthorized control of a legitimate account.
- **Privilege escalation:** An account may have gained additional permissions.
- **Data exposure:** Sensitive resources may have been accessed without authorization.

Do not assume that all these attacks occurred. They are hypotheses that must be investigated.

### Step 3: Build an Incident Timeline

```text
08:30 — Failed Authentication
          |
08:31 — Failed Authentication
          |
08:32 — Failed Authentication
          |
08:35 — Successful Authentication
          |
08:40 — Unusual Location Detected
          |
08:45 — Sensitive File Accessed
          |
08:50 — Privileged Group Modified
          |
          v
    Security Investigation
```

### Step 4: Collect Additional Evidence

An analyst should investigate:

1. Source IP addresses and device information.
2. Authentication methods and MFA results.
3. Previous login history.
4. Account and group membership changes.
5. Resource access records.
6. Endpoint security alerts.
7. Whether the user confirms the activity.

### Step 5: Recommend Defensive Actions

Depending on the evidence and organizational procedures:

- Reset compromised credentials.
- Revoke suspicious active sessions and tokens.
- Temporarily disable an account when justified.
- Remove unauthorized privileges.
- Enforce MFA.
- Investigate affected systems.
- Preserve logs and document the incident.

---

## 4. 🛡️ Assess Identity Risks

Identity risk assessment identifies weaknesses that could allow unauthorized users to access systems or misuse legitimate accounts.

### Objective

Evaluate identity-related security weaknesses and prioritize remediation.

### Step 1: Review Identity Security Areas

| Area | What to Check | Possible Risk |
|---|---|---|
| Password Security | Weak password policies | Credential compromise |
| MFA | Missing MFA protection | Unauthorized access |
| Privileged Accounts | Excessive admin permissions | Privilege abuse |
| Inactive Accounts | Old enabled accounts | Account takeover |
| Group Membership | Unnecessary group assignments | Excessive access |
| Monitoring | Missing authentication logs | Delayed detection |
| Service Accounts | Unmanaged credentials | Service account compromise |

### Step 2: Assign Risk Levels

Use a simple risk classification:

| Risk Level | Meaning |
|---|---|
| Critical | Immediate and severe security concern |
| High | Significant risk requiring prompt attention |
| Medium | Risk requiring planned remediation |
| Low | Limited risk that should still be monitored |

Risk ratings should consider both the likelihood of exploitation and the potential impact.

### Step 3: Create a Risk Assessment Table

Example:

| Finding | Likelihood | Impact | Risk |
|---|---|---|---|
| Admin account without MFA | High | Critical | Critical |
| Inactive enabled account | Medium | High | High |
| Excessive user permissions | Medium | High | High |
| Missing login monitoring | Medium | Medium | Medium |

*These are illustrative ratings. Actual ratings depend on the organization's environment, exposure, and security controls.*

### Step 4: Prioritize Remediation

A typical priority order:

1. Secure exposed privileged accounts.
2. Investigate potentially compromised identities.
3. Remove unnecessary administrative access.
4. Disable or review stale accounts.
5. Improve MFA coverage.
6. Strengthen identity monitoring.
7. Establish regular access reviews.

---

## 5. 📄 Create IAM Security Reports

Security reports communicate identity-related findings, risks, evidence, and recommendations to technical teams and management.

A good IAM report should be clear, evidence-based, and actionable.

### Objective

Create a professional IAM security assessment report based on the findings from the previous tasks.

### IAM Security Report Template

Use the following structure for your report:

# IAM Security Assessment Report

**Organization:** Example Organization  
**Assessment Type:** Identity & Access Management Review  
**Assessment Date:** YYYY-MM-DD  
**Prepared By:** Security Analyst  
**Status:** Draft

### 1. Executive Summary

Briefly describe the purpose of the assessment, the main findings, and the overall security condition.

### 2. Assessment Scope

List the areas reviewed:

- Authentication mechanisms.
- User and group management.
- Access control and permissions.
- Privileged accounts.
- Login monitoring.
- Identity security configurations.

### 3. Findings

| ID | Finding | Severity | Status |
|---|---|---|---|
| IAM-001 | Privileged account requires MFA review | Critical | Open |
| IAM-002 | Inactive accounts remain enabled | High | Open |
| IAM-003 | Excessive permissions identified | High | Under Review |
| IAM-004 | Login monitoring needs improvement | Medium | Open |

### 4. Evidence

Include relevant evidence such as:

- Authentication log entries.
- Failed login records.
- Group membership information.
- Access review results.
- Relevant configuration details.

Do not include passwords, access tokens, private keys, or other authentication secrets.

### 5. Risk Analysis

Explain:

- What could happen if each finding is exploited?
- Which accounts or systems could be affected?
- What is the potential business impact?
- Which findings require immediate attention?

### 6. Recommendations

Provide clear and actionable improvements.

Example:

| Finding | Recommendation | Priority |
|---|---|---|
| Missing MFA protection | Enforce MFA for privileged accounts | Critical |
| Inactive accounts | Review and disable unnecessary accounts | High |
| Excessive permissions | Apply least privilege | High |
| Insufficient monitoring | Configure identity event monitoring | Medium |

### 7. Conclusion

Summarize the assessment results and identify the next steps for improving identity security.

### 8. Evidence and Limitations

Document the data sources, assessment limitations, and any areas that could not be verified.

---

## 6. 🤖 AI-Assisted Practical Analysis

AI can help analyze the datasets from these tasks, but all findings must be verified.

### Prompt 1: Authentication Log Analysis

```text
Act as a SOC analyst specializing in Identity and Access Management.

Analyze the authentication logs I provide.

Identify:
1. Repeated failed login attempts.
2. Unusual login patterns.
3. Suspicious source IP addresses.
4. Successful logins following repeated failures.
5. Accounts requiring further investigation.

Separate observed facts from assumptions.
Do not declare an account compromised without sufficient evidence.

Provide your findings in a structured table.

[Insert sanitized logs]
```

### Prompt 2: Access Control Review

```text
Act as an IAM security analyst.

Review the following user accounts, roles, group memberships,
and assigned permissions.

Identify:
1. Excessive permissions.
2. Potential least-privilege violations.
3. Unnecessary administrative access.
4. Suspicious group memberships.
5. Access that requires business justification.

Recommend improvements, but do not assume that permissions
are unnecessary without understanding job responsibilities.

[Insert sanitized access data]
```

### Prompt 3: IAM Risk Assessment

```text
Act as an identity security assessor.

Review the following IAM configuration information.

Identify potential weaknesses involving:
- Authentication and MFA
- Privileged accounts
- Password policies
- Inactive accounts
- Access control
- Group memberships
- Identity monitoring

Classify findings by risk level.
Explain the potential impact and recommend remediation.

Clearly distinguish verified findings from assumptions.

[Insert sanitized assessment data]
```

### Prompt 4: IAM Security Report

```text
Act as a professional cybersecurity analyst.

Using the verified findings I provide, create an IAM
Security Assessment Report.

Include:
1. Executive Summary
2. Assessment Scope
3. Security Findings
4. Supporting Evidence
5. Risk Analysis
6. Remediation Recommendations
7. Conclusion

Use professional cybersecurity terminology.
Do not invent evidence or claim that unverified findings
are confirmed vulnerabilities.

[Insert verified assessment findings]
```

---

## 7. 📋 Practical Task Completion Checklist

Track your progress as you complete the exercises.

- [ ] Analyze authentication logs.
- [ ] Identify repeated failed login attempts.
- [ ] Review user permissions and group memberships.
- [ ] Identify potential least-privilege violations.
- [ ] Investigate a suspicious login scenario.
- [ ] Build an incident timeline.
- [ ] Assess identity-related security risks.
- [ ] Prioritize findings by severity.
- [ ] Create an IAM security assessment report.
- [ ] Validate AI-generated findings and recommendations.

---

## 8. 🧠 Final Review

After completing the tasks, make sure you can explain:

1. How authentication logs help detect identity-related attacks.
2. How excessive permissions increase organizational risk.
3. How to investigate suspicious login activities.
4. How to distinguish suspicious behavior from confirmed compromise.
5. How to perform a basic identity risk assessment.
6. How to prioritize IAM security findings.
7. How to prepare a professional IAM security report.
8. How AI can assist with identity security analysis and why human verification is necessary.

> 🛡️ **Real-World Security Mindset:** An IAM analyst does more than manage user accounts. They continuously verify identities, review permissions, investigate suspicious access, reduce identity risks, and ensure that every security finding is supported by evidence.
