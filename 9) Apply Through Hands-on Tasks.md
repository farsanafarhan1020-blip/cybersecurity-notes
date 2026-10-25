# ☁️ Hands-on Cloud Security Tasks

> **Cloud security becomes meaningful when you can inspect cloud environments, analyze security events, identify misconfigurations, and communicate your findings through professional security reports.**

Hands-on cloud security practice helps you apply concepts such as **cloud architecture, identity and access management (IAM), logging and monitoring, cloud threats, and incident response** to realistic situations.

The goal is to develop the skills needed to assess cloud environments, recognize security risks, investigate suspicious activity, and recommend appropriate remediation.

---

## 1. 🌐 Explore Cloud Environments

### What Is a Cloud Environment?

A cloud environment is a collection of cloud-based resources used to run applications, store data, manage identities, and provide network services.

Examples include:

* **Compute:** Virtual machines and cloud instances.
* **Storage:** Object storage, file storage, and block storage.
* **Networking:** Virtual networks, subnets, firewalls, and routing.
* **Identity:** Users, groups, roles, and permissions.
* **Databases:** Managed relational and NoSQL databases.
* **Monitoring:** Audit logs, metrics, alerts, and security findings.

### What Should You Explore?

| Component  | What to Investigate                                 | Security Importance                                |
| ---------- | --------------------------------------------------- | -------------------------------------------------- |
| Compute    | Virtual machines and their configurations           | Detect exposed or unnecessarily accessible systems |
| Storage    | Storage permissions and access policies             | Prevent unauthorized data access                   |
| Networking | Public IPs, firewall rules, and network boundaries  | Identify unnecessary exposure                      |
| IAM        | Users, roles, policies, and credentials             | Detect excessive privileges                        |
| Logging    | Audit logs and monitoring configurations            | Ensure activities can be investigated              |
| Encryption | Encryption settings for data at rest and in transit | Protect sensitive information                      |

### How to Explore a Cloud Environment

Use a dedicated learning account, an approved training environment, or a local cloud simulator.

1. Sign in to the cloud management console.
2. Explore the available compute, storage, networking, and identity services.
3. Identify which resources are publicly accessible.
4. Review user roles and permissions.
5. Check whether audit logging and monitoring are enabled.
6. Document your observations without changing production resources.

### Example: Investigating a Storage Resource

Imagine a cloud storage bucket containing application logs.

During your review, you discover that the bucket allows public access.

**Potential risks:**

* Unauthorized users may access sensitive files.
* Application logs may expose internal system information.
* Credentials or personal information may be disclosed if the logs contain them.

**Recommended investigation:**

* Determine whether public access is intentional.
* Review the effective permissions and applicable access policies.
* Check access logs for unexpected requests.
* Restrict public access if it is not required and you are authorized to make the change.

**Important:** A public resource is not automatically vulnerable. The security impact depends on the data, permissions, intended purpose, and surrounding controls.

---

## 2. 🚨 Analyze Cloud Security Incidents

### What Is a Cloud Security Incident?

A cloud security incident is an event that compromises, or threatens to compromise, the confidentiality, integrity, or availability of cloud resources.

Examples include:

* Unauthorized access to a cloud account.
* Suspicious use of privileged credentials.
* Exposure of sensitive storage.
* Unexpected changes to firewall rules.
* Unauthorized creation of virtual machines.
* Attempts to disable security logging.
* Unusual data downloads or transfers.

### Cloud Incident Investigation Process

```text
        Security Alert
              |
              v
     Validate the Alert
              |
              v
     Collect Relevant Evidence
              |
              v
     Build the Event Timeline
              |
              v
     Determine Scope and Impact
              |
              v
     Contain the Threat
              |
              v
     Eradicate the Cause
              |
              v
     Recover and Validate
              |
              v
     Document Lessons Learned
```

### Step 1: Validate the Alert

Determine whether the alert represents a genuine security concern.

Check:

* What activity triggered the alert?
* Which resource or account was involved?
* Was the activity expected?
* Does the event match an approved maintenance task or deployment?

An unusual event is not automatically malicious. Context is essential.

### Step 2: Collect Evidence

Depending on the cloud platform, useful evidence may include:

* Identity and access audit logs.
* Authentication events.
* Network flow logs.
* Storage access logs.
* Resource configuration changes.
* Security alerts.
* Application logs.

Preserve relevant evidence according to your organization's incident-handling procedures.

### Step 3: Build a Timeline

Arrange related events chronologically.

For example:

| Time  | Event                            | Possible Significance                                   |
| ----- | -------------------------------- | ------------------------------------------------------- |
| 09:00 | User signs in                    | Establishes the beginning of the activity               |
| 09:05 | Privileged role is assigned      | May indicate privilege escalation or an approved change |
| 09:08 | Storage permissions are modified | Could expose sensitive information                      |
| 09:12 | Large data download occurs       | May indicate data exfiltration                          |
| 09:15 | Security alert is generated      | Requires investigation                                  |

These events are hypothetical. Their significance must be established through evidence.

### Step 4: Determine the Impact

Investigate:

* Which accounts and resources were affected?
* Was sensitive data accessed or modified?
* Were permissions changed?
* Did the activity spread to other resources?
* Is the incident ongoing?

### Step 5: Contain and Recover

When an incident is confirmed, authorized responders may:

* Revoke compromised sessions or credentials.
* Disable or restrict affected accounts.
* Remove unauthorized permissions.
* Isolate affected workloads.
* Block malicious network access.
* Restore trusted configurations.
* Validate that the original vulnerability is resolved.

Containment should be proportionate to the risk and coordinated with the responsible team.

### Practical Scenario: Suspicious Cloud Login

**Scenario:** A cloud account receives an alert for a login from an unfamiliar location, followed by the creation of a privileged identity.

Your investigation should answer:

1. Was the login successful?
2. Was multifactor authentication used?
3. Which identity performed the actions?
4. Was the privileged identity created by an authorized administrator?
5. What resources did the identity access?
6. Were any sensitive configurations or permissions changed?
7. What containment actions are necessary?

**Security lesson:** Investigate the complete sequence of events rather than treating each alert as an isolated problem.

---

## 3. 📋 Review Cloud Logs

### What Are Cloud Logs?

Cloud logs are records of activities and events occurring within cloud infrastructure, services, applications, and identities.

They help security professionals understand **who performed an action, what happened, when it happened, and which resource was involved**.

### Important Types of Cloud Logs

| Log Type            | Records                                        | Security Use                                           |
| ------------------- | ---------------------------------------------- | ------------------------------------------------------ |
| Audit logs          | API calls and administrative changes           | Investigate account activity and configuration changes |
| Authentication logs | Sign-ins, failures, and authentication methods | Detect suspicious access                               |
| Network flow logs   | Network connection metadata                    | Investigate unusual communication patterns             |
| Storage access logs | Requests to stored objects or files            | Investigate potential data exposure                    |
| Application logs    | Application events and errors                  | Identify suspicious application behavior               |
| Security findings   | Potential vulnerabilities and threats          | Prioritize investigation and remediation               |

The availability and names of these logs depend on the cloud provider and the services being used.

### Important Fields in a Log Entry

A typical cloud audit event may contain:

| Field      | Meaning                                       |
| ---------- | --------------------------------------------- |
| Timestamp  | When the event occurred                       |
| Identity   | User, role, or service responsible            |
| Action     | Operation performed                           |
| Resource   | Target of the operation                       |
| Source IP  | Network address associated with the request   |
| Result     | Whether the operation succeeded or failed     |
| User agent | Client or software information, when recorded |

Not every log contains all these fields.

### Example: Understanding a Cloud Audit Event

Consider this fictional JSON event:

```json
{
  "timestamp": "2026-10-25T09:15:00Z",
  "identity": "analyst-user",
  "action": "UpdateStoragePolicy",
  "resource": "training-bucket",
  "source_ip": "203.0.113.25",
  "result": "Success"
}
```

**Interpretation:**

* The event occurred at the recorded timestamp.
* The identity `analyst-user` performed an operation to update a storage policy.
* The operation affected `training-bucket`.
* The event records a successful operation.

The IP address belongs to a range reserved for documentation and examples, so it should not be treated as a real attacker address.

**Important:** This event does not prove that an attack occurred. You must determine whether the action was authorized and examine the resulting policy.

### Practical Task: Analyze a JSON Log Using Python

Create a file named `cloud_logs.json`:

```json
[
  {
    "timestamp": "2026-10-25T09:00:00Z",
    "identity": "admin-user",
    "action": "CreateVirtualMachine",
    "resource": "vm-101",
    "result": "Success"
  },
  {
    "timestamp": "2026-10-25T09:05:00Z",
    "identity": "unknown-user",
    "action": "UpdateStoragePolicy",
    "resource": "training-bucket",
    "result": "Success"
  },
  {
    "timestamp": "2026-10-25T09:10:00Z",
    "identity": "analyst-user",
    "action": "Login",
    "resource": "cloud-console",
    "result": "Failure"
  }
]
```

Create another file named `analyze_logs.py`:

```python
import json

with open("cloud_logs.json", "r", encoding="utf-8") as file:
    logs = json.load(file)

for event in logs:
    print(
        f"[{event['timestamp']}] "
        f"Identity: {event['identity']} | "
        f"Action: {event['action']} | "
        f"Result: {event['result']}"
    )

print("\nPotential events requiring investigation:")

for event in logs:
    suspicious = (
        event["identity"] == "unknown-user"
        or event["result"] == "Failure"
    )

    if suspicious:
        print(
            f"Identity: {event['identity']} | "
            f"Action: {event['action']} | "
            f"Resource: {event['resource']}"
        )
```

Run the script:

```bash
python analyze_logs.py
```

### What This Script Teaches You

* Reading structured JSON data.
* Extracting fields from log records.
* Iterating through security events.
* Filtering events using conditions.
* Identifying records that need further investigation.

The script uses simple demonstration rules. A real detection system needs richer context, identity validation, event correlation, and carefully tuned detection logic.

**Security lesson:** Log analysis is not just about finding unusual events. It is about separating normal activity from activity that requires investigation.

---

## 4. ⚙️ Identify Cloud Misconfigurations

### What Is a Cloud Misconfiguration?

A cloud misconfiguration occurs when a resource, identity, service, or security control is configured in a way that violates security requirements or creates unnecessary risk.

Misconfigurations can expose resources even when the underlying cloud platform is operating correctly.

### Common Cloud Misconfigurations

| Misconfiguration                         | Potential Risk                        | Recommended Control                               |
| ---------------------------------------- | ------------------------------------- | ------------------------------------------------- |
| Public storage containing sensitive data | Unauthorized data exposure            | Restrict access and review storage policies       |
| Overly permissive IAM roles              | Privilege abuse                       | Apply least privilege                             |
| Unrestricted administrative ports        | Increased attack surface              | Restrict access to trusted networks               |
| Disabled audit logging                   | Reduced investigation capability      | Enable and protect relevant audit logs            |
| Missing encryption requirements          | Data confidentiality risks            | Enforce appropriate encryption                    |
| Unused credentials                       | Credential misuse                     | Disable or remove credentials according to policy |
| Unrestricted outbound traffic            | Unnecessary external communication    | Apply appropriate egress controls                 |
| Unpatched virtual machines               | Exploitation of known vulnerabilities | Establish a patch-management process              |

### How to Assess a Cloud Configuration

Follow a structured process.

**1. Establish the baseline**

Identify the security requirements the environment must meet.

Examples:

* Only authorized identities can access sensitive resources.
* Administrative interfaces are not exposed unnecessarily.
* Audit logs are enabled.
* Sensitive data is encrypted as required.

**2. Inspect the configuration**

Review:

* Resource access policies.
* Identity permissions.
* Network rules.
* Encryption settings.
* Logging configurations.
* Patch and vulnerability status.

**3. Compare against the baseline**

Identify differences between the current configuration and the required security settings.

**4. Validate the finding**

Determine whether the configuration is genuinely unsafe or justified by an approved business requirement.

**5. Assess risk**

Consider:

* **Likelihood:** How likely is the weakness to be exploited?
* **Impact:** What would happen if it were exploited?
* **Exposure:** Can an attacker reach the resource?
* **Data sensitivity:** Does the resource contain sensitive information?
* **Existing controls:** Are other protections reducing the risk?

**6. Recommend remediation**

Provide a clear, practical corrective action and verify the change after it is implemented.

### Example: An Overly Permissive IAM Policy

Suppose an application identity has permission to:

* Read storage objects.
* Delete storage objects.
* Modify access policies.
* Create new administrative identities.

However, the application only needs to read a specific storage location.

**Problem:** The identity has more privileges than required.

**Potential consequences:**

* Sensitive data could be deleted.
* Access policies could be changed.
* Additional identities could be created.
* A compromised application could become a path to broader access.

**Recommended remediation:**

1. Identify the exact operations the application requires.
2. Restrict access to the necessary resources.
3. Remove unnecessary permissions.
4. Test that the application continues to function.
5. Monitor for unexpected permission changes.

This is an application of the **principle of least privilege**.

### Use Security Frameworks as Assessment Guides

Security frameworks help turn general security advice into consistent assessment criteria.

Examples include:

* **CIS Benchmarks:** Security configuration recommendations for supported technologies.
* **NIST Cybersecurity Framework:** A structure for managing cybersecurity risk.
* **Cloud provider security guidance:** Provider-specific recommendations for identities, networking, storage, logging, and workloads.

Use the version and guidance applicable to the environment being assessed. A failed benchmark check is a finding to evaluate, not automatic proof of an exploitable vulnerability.

---

## 5. 📝 Create Cloud Security Assessment Reports

### What Is a Cloud Security Assessment Report?

A cloud security assessment report documents the security posture of an environment, the weaknesses discovered, their potential impact, and the recommended remediation.

It allows technical teams and decision-makers to understand security risks and prioritize corrective actions.

A useful report must be **accurate, evidence-based, understandable, and actionable**.

### Recommended Report Structure

#### 1. Executive Summary

Briefly explain:

* What environment was assessed.
* What the assessment covered.
* The most important findings.
* The overall security concerns.
* The recommended priorities.

Write this section so a nontechnical manager can understand it.

#### 2. Scope and Methodology

Document:

* The environment and resources reviewed.
* The assessment period.
* The approved tools and methods used.
* The security standards or baseline applied.
* Any limitations.

Clearly distinguish between resources actually assessed and resources that were outside the scope.

#### 3. Findings Summary

| ID   | Finding                                          | Severity                                     | Status |
| ---- | ------------------------------------------------ | -------------------------------------------- | ------ |
| C-01 | Sensitive storage is publicly accessible         | High, if confirmed                           | Open   |
| C-02 | Application identity has excessive permissions   | Medium, depending on context                 | Open   |
| C-03 | Audit logging is disabled for a critical service | High, depending on exposure and requirements | Open   |

These are illustrative findings. Actual severity must be based on evidence and the organization's risk criteria.

#### 4. Detailed Finding

Each finding should contain:

| Field             | Description                                                   |
| ----------------- | ------------------------------------------------------------- |
| Finding ID        | Unique reference, such as C-01                                |
| Title             | Short description of the issue                                |
| Severity          | Critical, High, Medium, Low, or Informational                 |
| Affected resource | Resource or configuration involved                            |
| Evidence          | Relevant logs, configuration details, or approved screenshots |
| Description       | What is wrong and how it was identified                       |
| Impact            | Potential consequences                                        |
| Recommendation    | Specific corrective action                                    |
| Validation        | How the fix can be verified                                   |
| Status            | Open, In Progress, Resolved, or Accepted Risk                 |

#### 5. Remediation Plan

Prioritize findings according to risk.

| Priority      | Typical Response                                                       |
| ------------- | ---------------------------------------------------------------------- |
| Critical      | Immediate coordinated action when a severe, credible risk is confirmed |
| High          | Prompt remediation with appropriate escalation                         |
| Medium        | Schedule remediation according to the risk and business context        |
| Low           | Address through planned maintenance or configuration improvement       |
| Informational | Record useful observations or improvement opportunities                |

Severity labels and response deadlines should follow the organization's established risk policy.

#### 6. Conclusion

Summarize:

* The most important security weaknesses.
* The remediation actions required.
* Any remaining risks.
* The need for follow-up validation.

---

## 6. 📄 Example Cloud Security Assessment Finding

The following is a fictional example of how to document a finding professionally.

### Finding C-01: Public Access to a Storage Resource

**Severity:** High — illustrative; final severity depends on evidence and impact.

**Affected resource:** `training-bucket`

**Status:** Open

#### Description

The assessment identified a storage resource whose access configuration permits public access. The resource's intended purpose and the sensitivity of its contents must be verified before determining the full impact.

#### Evidence

* The storage access policy allows public access.
* The assessment identified the relevant policy configuration.
* The contents and access history have not yet been fully investigated.

No real cloud resource was accessed or modified for this example.

#### Potential Impact

If the resource contains sensitive data, unauthorized users may be able to retrieve information without valid authentication. Depending on the permissions granted, the configuration could also create risks involving data modification or deletion.

#### Risk Analysis

The risk depends on:

* Whether the resource is intended to be public.
* Whether sensitive information is stored.
* Whether the effective policy permits reading, writing, or deletion.
* Whether other security controls limit access.
* Whether logs show unexpected access.

#### Recommendation

1. Confirm the resource's intended access requirements.
2. Review the effective storage policy and inherited permissions.
3. Remove unnecessary public access.
4. Apply least-privilege permissions.
5. Review available access logs for suspicious activity.
6. Verify the corrected configuration.
7. Document the remediation and any remaining risk.

#### Validation

After the authorized change, verify that:

* Unauthenticated access is blocked where public access is not required.
* Authorized users can still perform their intended tasks.
* The effective policy matches the approved security baseline.
* Logging and monitoring are functioning as expected.

---

## 7. 🧪 Hands-on Lab: Perform a Mini Cloud Security Assessment

### Objective

Practice the complete assessment workflow without risking a real cloud environment.

You can use:

* A cloud provider's approved training environment.
* A dedicated free-tier account with strict spending controls.
* A local cloud simulator or mock configuration files.

Do not create deliberately vulnerable public resources in a real environment simply to practice identifying misconfigurations.

### Task 1: Explore the Environment

Record the following:

| Component  | Questions to Answer                               |
| ---------- | ------------------------------------------------- |
| Identity   | Which users, roles, and service identities exist? |
| Storage    | Which resources are public or restricted?         |
| Networking | Which services are exposed to external networks?  |
| Logging    | Are important administrative activities recorded? |
| Encryption | Are applicable encryption requirements satisfied? |

### Task 2: Review Logs

Collect a small sample of authorized training logs.

Identify:

* Failed authentication attempts.
* Unexpected administrative actions.
* Changes to storage policies.
* Unusual resource creation.
* Attempts to disable logging.

For every event, record the evidence and explain whether it is suspicious, expected, or inconclusive.

### Task 3: Identify Misconfigurations

Compare the environment against an appropriate security baseline.

Document at least three findings, if present. Do not invent vulnerabilities just to reach a target number.

For each finding, record:

1. The configuration observed.
2. The expected secure configuration.
3. The potential impact.
4. The recommended remediation.
5. How you would validate the fix.

### Task 4: Investigate a Simulated Incident

Use the following fictional event sequence:

| Time  | Event                                             |
| ----- | ------------------------------------------------- |
| 10:00 | An unfamiliar identity successfully authenticates |
| 10:03 | The identity receives additional permissions      |
| 10:06 | A storage policy is modified                      |
| 10:09 | A large download is recorded                      |
| 10:12 | A security alert is generated                     |

Answer these questions:

1. Which event should be investigated first, and why?
2. What evidence would you collect?
3. How would you determine whether the actions were authorized?
4. How would you establish whether data was exposed?
5. What immediate containment actions might be appropriate?
6. Which logs would help establish the scope of the incident?
7. What remediation would prevent similar activity?

Remember that a timeline alone does not prove causation. Correlate events using identity, resource, timestamp, and other available evidence.

### Task 5: Write the Assessment Report

Prepare a report containing:

* Executive summary.
* Scope and methodology.
* Findings summary.
* Detailed findings with evidence.
* Risk assessment.
* Recommended remediation.
* Validation steps.
* Conclusion.

Save it as:

`cloud-security-assessment-report.md`

Use fictional names and sanitized evidence in a public GitHub repository. Never publish credentials, access tokens, sensitive logs, private IP details that reveal a protected environment, or confidential cloud configuration.

---

## 8. 🧠 Professional Security Practices

When performing cloud security assessments, follow these principles:

* **Authorization first:** Assess only accounts and resources you are permitted to examine.
* **Read before changing:** Understand the configuration before attempting remediation.
* **Preserve evidence:** Keep relevant logs and document investigation steps.
* **Verify findings:** Confirm that a suspected weakness exists and understand its impact.
* **Prioritize risk:** Address serious, credible threats before cosmetic issues.
* **Avoid unnecessary disruption:** Coordinate changes that could affect production services.
* **Protect sensitive data:** Redact credentials, personal information, and confidential details.
* **Validate remediation:** A reported fix is not complete until the corrected configuration is verified.
* **Document limitations:** Clearly identify anything you could not test or confirm.
* **Use automation carefully:** Automated findings require validation and contextual interpretation.

---

## 9. ⚡ Quick Revision

| Task                       | Main Purpose                                        | Expected Output                              |
| -------------------------- | --------------------------------------------------- | -------------------------------------------- |
| Explore cloud environments | Understand cloud resources and their configurations | Environment inventory                        |
| Analyze cloud incidents    | Reconstruct events and determine impact             | Incident timeline and investigation findings |
| Review cloud logs          | Detect and investigate suspicious activity          | Log analysis results                         |
| Identify misconfigurations | Compare actual settings with secure baselines       | Validated security findings                  |
| Create assessment reports  | Communicate risks and corrective actions            | Professional assessment report               |

### Key Takeaways

* Cloud security assessment combines configuration review, log analysis, risk evaluation, and reporting.
* Cloud logs provide evidence, but suspicious events require contextual investigation.
* Misconfigurations can create significant risks even when cloud services operate as designed.
* Security findings should include evidence, impact, remediation, and validation steps.
* Effective incident response requires investigation, containment, recovery, and lessons learned.
* A professional report communicates both technical details and business risk.
* Always verify the outcome of remediation rather than assuming that a change solved the problem.

---

## 10. 🎯 Active Recall

Try answering these questions without looking at the notes.

1. What is a cloud security assessment?
2. Name five cloud components that should be reviewed during a security assessment.
3. What is the difference between an audit log and a security finding?
4. Why does a successful API request not necessarily indicate malicious activity?
5. What steps would you follow when investigating a suspicious cloud login?
6. How can excessive IAM permissions increase the impact of a compromised identity?
7. Why should security findings be validated before assigning a final severity?
8. What information should a professional cloud security finding contain?
9. Why is remediation validation important?
10. How would you distinguish a confirmed vulnerability from a potential security concern?

<details>
<summary><strong>✅ Check Your Answers</strong></summary>

1. A structured review of cloud resources, configurations, identities, logs, and controls to identify and assess security risks.
2. Compute, storage, networking, IAM, logging, databases, and encryption configurations are examples.
3. An audit log records an event or action. A security finding identifies a potential or confirmed security issue that requires evaluation or remediation.
4. Legitimate administrative activity can also produce successful requests. Authorization, intent, context, and the resulting configuration must be examined.
5. Validate the alert, collect evidence, establish a timeline, verify the identity and activity, determine scope and impact, contain the threat if necessary, and document the response.
6. Excessive permissions can allow an attacker to access additional resources, change security settings, or perform destructive actions after compromising an identity.
7. Severity depends on exposure, exploitability, business impact, data sensitivity, and existing controls. Unverified assumptions can cause incorrect prioritization.
8. A unique ID, title, severity, affected resource, evidence, description, impact, remediation, validation method, and status.
9. Validation confirms that the weakness has been addressed without creating new problems or breaking required functionality.
10. A confirmed vulnerability is supported by sufficient evidence. A potential concern requires further investigation before its existence or impact can be established.

</details>

---

## 🏁 Final Thought

A cybersecurity professional does more than identify a suspicious configuration or alert. They **collect evidence, understand the risk, investigate the cause, recommend a practical solution, verify the result, and communicate the outcome clearly**.

These skills form the foundation of cloud security assessment, cloud incident response, and security operations.
