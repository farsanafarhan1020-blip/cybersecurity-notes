# 🛡️ 9. Apply Through Hands-on Tasks

> **Turn security automation concepts into practical Python tools.**

This stage combines everything learned so far:

* Python
* File handling
* JSON/CSV
* Regular expressions
* `os`
* `socket`
* `subprocess`
* `psutil`
* Logging
* Monitoring
* Scheduling
* Workflow automation
* Security reporting
* Incident support

The goal is to build **small, useful security utilities** that can later be combined into larger automation systems.

---

# 1️⃣ Build Log Analysis Scripts

## 📌 What is Log Analysis?

Log analysis means reading security/system logs and extracting useful information.

Example:

```text
2026-09-25 10:01:22 Failed login from 192.168.1.10
2026-09-25 10:02:15 Failed login from 192.168.1.10
2026-09-25 10:03:01 Failed login from 192.168.1.10
2026-09-25 10:04:22 Successful login from 192.168.1.10
```

A script can automatically detect:

```text
192.168.1.10
Failed attempts: 3
Successful login: 1

Potential suspicious authentication activity
```

## 🔄 Basic Workflow

```text
Log File
   ↓
Read
   ↓
Parse
   ↓
Extract Events
   ↓
Count / Correlate
   ↓
Apply Detection Rule
   ↓
Generate Alert
```

## 🧪 Task 1 — Failed Login Analyzer

Create:

```text
log_analyzer.py
```

Sample log:

```text
2026-09-25 10:00:01 Failed login from 192.168.1.10
2026-09-25 10:01:13 Failed login from 192.168.1.10
2026-09-25 10:02:44 Failed login from 192.168.1.20
2026-09-25 10:03:12 Failed login from 192.168.1.10
2026-09-25 10:04:22 Successful login from 192.168.1.10
```

Your program should:

1. Read the file.
2. Find failed-login events.
3. Extract IP addresses.
4. Count failures per IP.
5. Display the results.
6. Generate an alert when an IP reaches a chosen threshold.

Example output:

```text
===== LOGIN ANALYSIS =====

192.168.1.10 → 3 failed attempts
192.168.1.20 → 1 failed attempt

[ALERT]
192.168.1.10 exceeded the threshold.
```

### 🔐 Security Lesson

A detection is **not automatically proof of an attack**.

For example:

```text
Multiple failed logins
        ↓
Detection
        ↓
Investigation
        ↓
Possible brute-force activity
```

---

# 2️⃣ Create Monitoring Tools

Monitoring tools continuously check the state of a system.

You already learned about:

```text
CPU
Memory
Disk
Processes
Services
Files
Network
```

Now combine them into a small monitoring utility.

## 🧪 Task 2 — System Monitor

Create:

```text
system_monitor.py
```

Use:

```python
import psutil
```

Collect:

* CPU usage
* Memory usage
* Disk usage
* Running process count

Example:

```text
===== SYSTEM MONITOR =====

CPU Usage     : 32%
Memory Usage  : 61%
Disk Usage    : 48%
Processes     : 143
```

Add thresholds:

```text
CPU > 80%
Memory > 85%
Disk > 90%
```

If a threshold is exceeded:

```text
[WARNING] High CPU usage detected.
```

### 🔐 Security Connection

High resource usage can sometimes be associated with:

* malware
* cryptomining
* runaway processes
* denial-of-service conditions
* misconfigured applications

But:

> **High resource usage alone does not prove malicious activity.**

---

# 3️⃣ Develop Automation Workflows

Now combine individual security actions into a workflow.

Instead of:

```text
Detect
```

build:

```text
Detect
 ↓
Analyze
 ↓
Create Record
 ↓
Notify
 ↓
Generate Report
 ↓
Log Result
```

## 🧪 Task 3 — Failed Login Automation Workflow

Build:

```text
security_workflow.py
```

Workflow:

```text
Authentication Log
       ↓
Parse Events
       ↓
Count Failed Logins
       ↓
Threshold Check
       ↓
Suspicious Activity?
      / \
    NO   YES
    ↓     ↓
  Finish  Create Incident
              ↓
        Generate JSON
              ↓
        Generate Report
              ↓
        Log Result
```

Example incident JSON:

```json
{
    "event": "Repeated failed login",
    "source_ip": "192.168.1.10",
    "failed_attempts": 5,
    "severity": "HIGH",
    "status": "OPEN"
}
```

## 🔄 Automation vs Orchestration

| Automation                    | Orchestration                   |
| ----------------------------- | ------------------------------- |
| Performs a task automatically | Coordinates multiple tasks      |
| Parse a log                   | Parse → alert → report          |
| Hash a file                   | Detect → investigate → document |
| Check a port                  | Check → record → notify         |

Think:

> **Automation = DO**

> **Orchestration = COORDINATE**

---

# 4️⃣ Generate Security Reports

Security automation should not only detect events.

It should also **document what happened**.

## 🧪 Task 4 — Security Report Generator

Create:

```text
security_report.py
```

Input:

```text
Incident information
System information
Detected events
Statistics
```

Generate:

```text
security_report.txt
```

Example:

```text
====================================
        SECURITY INCIDENT REPORT
====================================

Date:
2026-09-25

Incident:
Repeated Failed Login Attempts

Source IP:
192.168.1.10

Failed Attempts:
5

Severity:
HIGH

Status:
OPEN

Detection:
Authentication log analysis

Recommended Action:
Investigate the source IP and review
authentication activity.

====================================
```

You can later generate:

```text
TXT
CSV
JSON
```

reports.

---

# 5️⃣ Build Incident-Support Utilities

Incident-support utilities help analysts investigate and document suspicious activity.

They don't have to automatically perform dangerous response actions.

Useful utilities include:

```text
IP information collector
File hash generator
Log extractor
Process information collector
System information collector
Incident JSON creator
Evidence organizer
Report generator
```

## 🧪 Task 5 — Incident Information Collector

Create:

```text
incident_collector.py
```

Collect basic information such as:

```text
Hostname
Operating system
Current user
Current working directory
CPU usage
Memory usage
Disk usage
Timestamp
```

Example:

```text
===== INCIDENT COLLECTION =====

Hostname       : security-lab
OS             : Linux
User           : analyst
CPU Usage      : 34%
Memory Usage   : 58%
Disk Usage     : 46%
Timestamp      : 2026-09-25 12:30:10
```

Save the result as:

```text
incident_data.json
```

---

# 🔥 Combine Everything

The final goal is to combine the five tasks.

```text
                  SECURITY AUTOMATION
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
     Log Analysis    Monitoring    Incident Data
          │              │              │
          └──────────────┼──────────────┘
                         ↓
                  Detection Logic
                         ↓
                   Alert / Event
                         ↓
                  Workflow Engine
                         ↓
                ┌────────┴────────┐
                ↓                 ↓
             Report          Incident Record
                │                 │
                └────────┬────────┘
                         ↓
                  Analyst Review
```

---

# 🧩 Mini Project — Security Automation Toolkit

Create this project:

```text
security-automation-toolkit/
│
├── log_analyzer.py
├── system_monitor.py
├── security_workflow.py
├── security_report.py
├── incident_collector.py
│
├── logs/
│   └── sample.log
│
├── reports/
│
├── incidents/
│
└── README.md
```

## 🎯 Project Workflow

```text
sample.log
    ↓
log_analyzer.py
    ↓
Suspicious Event
    ↓
security_workflow.py
    ↓
incident_collector.py
    ↓
incident JSON
    ↓
security_report.py
    ↓
Security Report
```

---

# 🔐 Security Requirements

When building these tools:

### 1. Least Privilege

Don't run everything as root unnecessarily.

### 2. Validate Input

Don't blindly trust:

```text
log files
IP addresses
filenames
URLs
user input
```

### 3. Handle Errors

Use:

```python
try:
    ...
except Exception as e:
    ...
```

where appropriate.

### 4. Log Your Automation

Record:

```text
start time
action
result
errors
completion time
```

### 5. Protect Sensitive Data

Don't place these in reports unnecessarily:

```text
passwords
API keys
private keys
session tokens
authentication cookies
sensitive personal information
```

### 6. Don't Automate Dangerous Actions Without Controls

For example, automatically blocking an IP can have unintended consequences.

A safer beginner workflow is:

```text
Detection
   ↓
Alert
   ↓
Human Review
   ↓
Authorized Response
```

---

# 🧪 Recommended Practice Order

Don't build the whole project at once.

Follow this order:

```text
Task 1
Log Analyzer
   ↓
Task 2
System Monitor
   ↓
Task 3
Automation Workflow
   ↓
Task 4
Report Generator
   ↓
Task 5
Incident Collector
   ↓
Mini Project
```

Each task should build on the previous one.

---

# 🧠 Skills You Are Combining

| Skill                   | Used For             |
| ----------------------- | -------------------- |
| File Handling           | Reading logs         |
| Regex/String Processing | Extracting events    |
| Dictionaries            | Counting events      |
| JSON                    | Incident records     |
| CSV                     | Structured reports   |
| `os`                    | System information   |
| `psutil`                | Monitoring           |
| `socket`                | Network information  |
| `subprocess`            | System commands      |
| Logging                 | Execution tracking   |
| Scheduling              | Periodic execution   |
| Exception Handling      | Reliability          |
| Functions               | Reusable code        |
| Modules                 | Project organization |

---

# 🧠 Memory Trick

Remember:

> **A → M → W → R → I**

```text
A = Analyze logs
M = Monitor systems
W = Workflow automation
R = Reports
I = Incident support
```

Or simply:

```text
ANALYZE → MONITOR → AUTOMATE → REPORT → INVESTIGATE
```

---

# 🎯 Final Goal

By completing this section, you should be able to build a small security automation system that can:

```text
✔ Read security logs
✔ Extract security events
✔ Detect predefined patterns
✔ Monitor system resources
✔ Create incident records
✔ Generate reports
✔ Track automation results
✔ Organize investigation data
✔ Automate repetitive security tasks
```

The important mindset is:

> **Don't automate just because you can. Automate tasks that are repetitive, predictable, measurable, and safe to automate.**

---

# ⚡ Quick Revision

```text
Log Analysis
     ↓
Find useful events

Monitoring
     ↓
Watch system state

Automation
     ↓
Perform repetitive actions

Workflow
     ↓
Connect multiple actions

Reporting
     ↓
Document results

Incident Utilities
     ↓
Support investigation
```

### 🛡️ Final Takeaway

Security automation is not about replacing the security analyst.

It is about allowing the analyst to spend less time performing repetitive tasks and more time **understanding events, investigating suspicious activity, and making informed decisions**.

> **Automate the repetitive. Monitor the important. Document the results. Keep humans in control of high-impact decisions.**

For the hands-on part, I recommend we **actually build Task 1 together first** rather than jumping directly to the mini-project. Send me your environment (Kali/Linux or Windows + your Python version), and we’ll build the **Log Analyzer step by step**.
