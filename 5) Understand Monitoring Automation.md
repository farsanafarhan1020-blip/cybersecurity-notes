# 📡 5. Understand Monitoring Automation

> **Monitoring automation means continuously observing systems, services, files, and other security-relevant resources, automatically detecting important changes, and generating alerts when defined conditions occur.**

In manual monitoring, a person repeatedly checks:

```text
Is the server running?
Is the service running?
Did a file change?
Did something unexpected happen?
```

Monitoring automation turns this into:

```text
        System
           ↓
       Monitor
           ↓
      Detect Change
           ↓
     Evaluate Rule
           ↓
        Alert
           ↓
    Human Investigation
```

---

# 🧠 1. What Is Monitoring?

**Monitoring** is the continuous or periodic observation of a system to understand its current state and detect changes or problems.

In cybersecurity, monitoring can involve:

* CPU and memory
* Running processes
* Network connections
* Services
* Files
* Authentication activity
* Configuration changes
* System availability
* Security events

---

# 🔄 Monitoring vs One-Time Checking

This distinction is important.

### One-time check

```text
Check system
    ↓
Get result
    ↓
Finish
```

### Monitoring

```text
Check
 ↓
Wait
 ↓
Check again
 ↓
Compare
 ↓
Detect change
 ↓
Repeat
```

Example:

```text
10:00 → SSH running
10:01 → SSH running
10:02 → SSH stopped ⚠️
10:03 → SSH stopped ⚠️
```

Monitoring allows us to notice the change.

---

# 🏗️ Basic Monitoring Architecture

```text
┌─────────────────────────────┐
│       System Resources      │
│                             │
│ CPU / RAM / Processes       │
│ Services / Files / Network  │
└──────────────┬──────────────┘
               ↓
        ┌──────────────┐
        │   Monitor    │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │ Analyze Data │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │ Detection    │
        │    Rules     │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │    Alert     │
        └──────┬───────┘
               ↓
        Security Analyst
```

---

# 🖥️ 2. System Monitoring

## What Is System Monitoring?

System monitoring means observing the health and activity of an operating system.

Things we might monitor include:

```text
CPU usage
Memory usage
Disk usage
Running processes
Network connections
Logged-in users
System uptime
System load
```

Python can automate many basic checks.

---

# ⚙️ CPU Monitoring

A simple Python approach uses the `psutil` library.

First install it if necessary:

```bash
pip install psutil
```

Then:

```python
import psutil

cpu = psutil.cpu_percent(interval=1)

print(f"CPU Usage: {cpu}%")
```

Example:

```text
CPU Usage: 34.2%
```

---

# 🧠 Memory Monitoring

```python
import psutil

memory = psutil.virtual_memory()

print(f"Memory Usage: {memory.percent}%")
```

Possible output:

```text
Memory Usage: 61.5%
```

---

# 💾 Disk Monitoring

```python
import psutil

disk = psutil.disk_usage("/")

print(f"Disk Usage: {disk.percent}%")
```

Example:

```text
Disk Usage: 72.4%
```

---

# 🚨 Threshold Monitoring

Monitoring becomes useful when we define a threshold.

For example:

```python
import psutil

cpu = psutil.cpu_percent(interval=1)

if cpu > 80:
    print("ALERT: High CPU usage")
else:
    print("CPU usage normal")
```

Flow:

```text
CPU Usage
    ↓
Compare with threshold
    ↓
 ┌──────────────┐
 │              │
Normal        High
 │              │
 ↓              ↓
Continue       Alert
```

---

# ⚠️ Thresholds Are Context-Dependent

A CPU usage of 90% isn't automatically a security incident.

It could be:

* A legitimate workload
* Software compilation
* Video processing
* A backup
* A scheduled task
* An unexpected process

Therefore:

> **Monitoring detects conditions; investigation determines meaning.**

---

# 🔍 Process Monitoring

We can inspect running processes.

```python
import psutil

for process in psutil.process_iter(["pid", "name"]):
    print(process.info)
```

Example:

```text
{'pid': 1, 'name': 'systemd'}
{'pid': 724, 'name': 'sshd'}
{'pid': 1050, 'name': 'python'}
```

This can help build simple monitoring utilities.

---

# 🛡️ Security Use

A monitoring script could maintain a list of expected processes:

```python
expected = ["sshd", "python"]
```

Then compare running processes against the expected state.

However, real environments are more complex because legitimate processes can change depending on the system and workload.

---

# 🌐 Network Monitoring

Basic network information can also be collected.

```python
import psutil

connections = psutil.net_connections()

for connection in connections:
    print(connection)
```

This can provide information about network connections on your own machine.

Possible information includes:

```text
Local address
Remote address
Status
Protocol-related information
Process association
```

---

# 📡 3. Service Monitoring

## What Is a Service?

A **service** is a background program that provides functionality to the operating system or other applications.

Examples on Linux:

```text
SSH
Web server
DNS service
Database
Network services
```

A service may be:

```text
Running
Stopped
Failed
Restarting
```

---

# 🔎 Why Monitor Services?

Suppose a server requires:

```text
SSH
Web Server
Database
```

If the web service stops:

```text
Web Server
     ↓
    STOP
     ↓
Monitoring detects change
     ↓
🚨 Alert
```

This can be useful for both availability and security monitoring.

---

# 🐧 Checking Services on Linux

You can inspect services using:

```bash
systemctl status ssh
```

or:

```bash
systemctl status ssh.service
```

The exact service name depends on the Linux distribution and installed software.

---

# 🐍 Automating a Service Check

Python can execute a local command using `subprocess`.

```python
import subprocess

result = subprocess.run(
    ["systemctl", "is-active", "ssh"],
    capture_output=True,
    text=True
)

status = result.stdout.strip()

print("SSH status:", status)
```

Possible output:

```text
SSH status: active
```

---

# 🚨 Service Alert

```python
if status != "active":
    print("ALERT: SSH service is not active")
else:
    print("SSH service is running")
```

This is a basic automated service monitor.

---

# 🔐 Important Subprocess Practice

Prefer:

```python
subprocess.run(
    ["systemctl", "is-active", "ssh"],
    ...
)
```

instead of:

```python
subprocess.run(
    "systemctl is-active ssh",
    shell=True,
    ...
)
```

Using an argument list avoids unnecessary shell interpretation and is generally safer when command input could ever be influenced by external data.

---

# 📁 4. File Monitoring

## Why Monitor Files?

Files can contain important security information.

Examples:

```text
Configuration files
Authentication files
System binaries
Web application files
Security logs
Scripts
```

Unexpected changes may require investigation.

---

# 📝 File Attributes

A file has properties such as:

```text
Name
Path
Size
Modified time
Permissions
Hash
```

Example:

```text
/etc/example.conf
```

might have:

```text
Size: 2048 bytes
Modified: 10:30
```

If these values change unexpectedly, monitoring can detect it.

---

# 🕐 Monitoring Modification Time

Python:

```python
import os

file_path = "important.txt"

modified = os.path.getmtime(file_path)

print(modified)
```

A more readable version:

```python
import os
from datetime import datetime

file_path = "important.txt"

modified = os.path.getmtime(file_path)

print(datetime.fromtimestamp(modified))
```

---

# 📏 Monitoring File Size

```python
import os

file_path = "important.txt"

size = os.path.getsize(file_path)

print("File size:", size, "bytes")
```

If the size changes:

```text
Before → 2048 bytes
After  → 3072 bytes
```

something changed.

But file size alone doesn't tell us exactly what changed.

---

# 🔐 5. Hash-Based Change Detection

One of the most useful methods is comparing a file's cryptographic hash.

A hash produces a fixed-length representation of file contents.

Example:

```text
File
 ↓
SHA-256
 ↓
Hash
```

If the file content changes, its SHA-256 hash will normally change.

---

# 🐍 Generating a SHA-256 Hash

```python
import hashlib

def calculate_hash(file_path):

    sha256 = hashlib.sha256()

    with open(file_path, "rb") as file:

        while chunk := file.read(4096):
            sha256.update(chunk)

    return sha256.hexdigest()
```

Use it:

```python
hash_value = calculate_hash("important.txt")

print(hash_value)
```

Example:

```text
8c7d...example...91a
```

The exact hash depends on the file contents.

---

# 🔄 6. Change Detection

The basic idea:

```text
              File
                ↓
           Calculate Hash
                ↓
          Save Baseline
                ↓
              Wait
                ↓
          Calculate Hash
                ↓
           Compare Hashes
                ↓
       ┌────────┴────────┐
       ↓                 ↓
      Same             Different
       ↓                 ↓
   No change         ⚠️ Alert
```

This is the foundation of **file integrity monitoring**.

---

# 🛡️ File Integrity Monitoring

**File Integrity Monitoring (FIM)** means monitoring files for unexpected changes.

It can be used to monitor:

* Configuration files
* System files
* Important scripts
* Web application files
* Security-sensitive files

Example:

```text
Baseline:
config.conf → HASH_A

Later:
config.conf → HASH_B

HASH_A != HASH_B
       ↓
⚠️ File changed
```

---

# ⚠️ Change Does Not Automatically Mean Attack

This is very important.

A changed file could be:

```text
Legitimate update
Configuration change
Software installation
Administrator modification
Automated deployment
Malicious modification
```

Therefore:

> **Change detection identifies something that changed; it does not determine why it changed.**

---

# 🧩 7. Directory Monitoring

You can also monitor multiple files.

```python
import os

directory = "monitored"

for filename in os.listdir(directory):

    path = os.path.join(directory, filename)

    if os.path.isfile(path):
        print(path)
```

This gives you the files that can be included in a monitoring baseline.

---

# 🗂️ Creating a File Baseline

A baseline is a known-good state that we can compare against later.

Example:

```text
Baseline
────────────────────
config.txt → HASH_A
script.py  → HASH_B
index.html → HASH_C
```

Later:

```text
Current
────────────────────
config.txt → HASH_A
script.py  → HASH_X
index.html → HASH_C
```

Comparison:

```text
config.txt → Same
script.py  → CHANGED ⚠️
index.html → Same
```

---

# 💾 Storing a Baseline in JSON

Python:

```python
import json

baseline = {
    "config.txt": "hash_a",
    "script.py": "hash_b",
    "index.html": "hash_c"
}

with open("baseline.json", "w") as file:
    json.dump(baseline, file, indent=4)
```

Now the baseline can be reused later.

This connects directly with your previous topic:

> **Security Data Processing → JSON analysis**

---

# 🔔 8. Alerting Workflows

Monitoring becomes much more useful when it can automatically generate alerts.

A basic workflow:

```text
Monitor
   ↓
Detect
   ↓
Evaluate
   ↓
Generate Alert
   ↓
Record
   ↓
Notify
   ↓
Human Investigation
```

---

# 🚨 Example Alert

Suppose:

```text
SSH service stopped
```

The monitoring system could generate:

```text
================================
 SECURITY ALERT
================================

Type: Service Failure
Service: SSH
Status: inactive
Host: localhost

Action:
Investigate service status.

================================
```

---

# 📊 Alert Structure

A useful alert should contain context.

For example:

```text
Alert ID
Timestamp
Alert Type
Severity
Source
Affected Resource
Observed Value
Expected Value
Description
```

Example:

```text
Alert ID: 0012
Time: 10:45:21
Type: File Change
Severity: Medium
File: important.conf
Expected Hash: HASH_A
Current Hash: HASH_B
```

This makes investigation easier.

---

# 🎚️ 9. Alert Severity

Automation can assign different levels.

```text
LOW
MEDIUM
HIGH
CRITICAL
```

Example:

| Event                              | Example Severity |
| ---------------------------------- | ---------------- |
| Non-critical file changed          | Low              |
| Unexpected service stopped         | Medium           |
| Security configuration changed     | High             |
| Critical security control disabled | Critical         |

These are examples. Actual severity depends on the environment and organization's detection rules.

---

# 🔗 10. Complete Alerting Workflow

```text
              MONITOR
                 │
        ┌────────┼────────┐
        ↓        ↓        ↓
      System   Service   File
        │        │        │
        └────────┼────────┘
                 ↓
             Detection
                 ↓
          Compare With Rule
                 ↓
        ┌────────┴────────┐
        ↓                 ↓
      Normal            Change
        ↓                 ↓
    Continue            Alert
                          ↓
                    Log Alert
                          ↓
                    Notify SOC
                          ↓
                    Investigation
```

---

# 🤖 11. Monitoring Loop

A basic monitor often runs repeatedly.

```python
import time

while True:

    print("Checking system...")

    # monitoring logic

    time.sleep(10)
```

Flow:

```text
Check
 ↓
Wait
 ↓
Check
 ↓
Wait
 ↓
Check
 ↓
...
```

This is called **polling**.

---

# ⏱️ Polling vs Event-Based Monitoring

### Polling

The program repeatedly asks:

> "Did anything change?"

```text
Check → Wait → Check → Wait
```

### Event-based monitoring

The system notifies the program when something happens.

```text
Event occurs
     ↓
Notification
     ↓
Program reacts
```

Event-based approaches can be more efficient for certain workloads, while polling is easier to understand and implement.

---

# 🐍 12. Simple Monitoring Program

Here's a small example combining system monitoring with alerting:

```python
import psutil

cpu = psutil.cpu_percent(interval=1)
memory = psutil.virtual_memory().percent

print(f"CPU: {cpu}%")
print(f"Memory: {memory}%")

if cpu > 80:
    print("⚠️ ALERT: High CPU usage")

if memory > 80:
    print("⚠️ ALERT: High memory usage")
```

This follows:

```text
Collect
   ↓
Evaluate
   ↓
Alert
```

---

# 🧩 13. Combining Multiple Monitors

A security monitoring script could check:

```text
             Monitor
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
      CPU     Services   Files
       │        │        │
       └────────┼────────┘
                ↓
          Analyze Results
                ↓
             Alerts
```

For example:

```text
CPU → Normal
SSH → Running
Config → Changed ⚠️
```

Final output:

```text
1 security event detected.
```

---

# 🛡️ 14. Monitoring in a SOC

In a SOC environment, monitoring data can come from:

```text
Servers
Endpoints
Network Devices
Firewalls
Applications
Cloud Services
```

These events can flow into:

```text
Collection
    ↓
Processing
    ↓
Detection
    ↓
Correlation
    ↓
Alerting
    ↓
SOC Analyst
```

This connects everything you've learned so far:

```text
Network Automation
       ↓
Log Processing
       ↓
Security Data Processing
       ↓
Monitoring Automation
       ↓
Security Operations
```

---

# 🧠 15. Monitoring vs Detection vs Alerting

These three concepts are related but different.

| Concept    | Meaning                                      |
| ---------- | -------------------------------------------- |
| Monitoring | Observing system activity/state              |
| Detection  | Identifying a condition of interest          |
| Alerting   | Notifying someone/system about the condition |

Example:

```text
Monitoring:
CPU = 95%

Detection:
CPU > 80% threshold

Alerting:
"High CPU usage detected"
```

---

# ⚠️ 16. Common Monitoring Problems

## 1. Too Many Alerts

```text
Normal activity
 ↓
Thousands of alerts
 ↓
Alert fatigue
```

---

## 2. Poor Thresholds

If the threshold is too low:

```text
Too many alerts
```

If too high:

```text
Important events may be missed
```

---

## 3. No Baseline

Without knowing normal behavior, it is harder to identify unusual changes.

---

## 4. No Logging

If your monitoring system itself doesn't record what happened, troubleshooting becomes difficult.

---

## 5. Excessive Permissions

Monitoring scripts should use only the permissions they actually need.

> 🔐 **Least privilege applies to automation too.**

---

## 6. No Error Handling

A monitoring script that crashes when a file disappears is not reliable.

Use:

```python
try:
    # monitoring operation
except Exception as error:
    print("Monitoring error:", error)
```

For production systems, handle specific expected exceptions where possible rather than catching everything indiscriminately.

---

# 🧪 17. Hands-On Practice

## 🟢 Task 1 — System Monitor

Install:

```bash
pip install psutil
```

Create a script that displays:

```text
CPU Usage:
Memory Usage:
Disk Usage:
```

Run it and observe how the values change.

---

## 🟢 Task 2 — Threshold Alerts

Add rules:

```text
CPU > 80%
Memory > 80%
Disk > 90%
```

Example:

```text
CPU: 35%
Memory: 61%
Disk: 72%

System status: NORMAL
```

or:

```text
CPU: 91%

⚠️ ALERT: High CPU usage
```

---

## 🟡 Task 3 — Service Monitor

On your Linux/Kali machine, choose an installed service.

Check it using:

```bash
systemctl status <service>
```

Then write Python code using `subprocess` to determine whether the service is active.

Your program should produce:

```text
[OK] Service is running
```

or:

```text
[ALERT] Service is not running
```

Only monitor services on systems you own or are authorized to administer.

---

## 🟡 Task 4 — File Hash Monitor

Create:

```text
important.txt
```

Calculate its SHA-256 hash.

Then:

1. Save the hash.
2. Modify the file.
3. Calculate the hash again.
4. Compare the two hashes.

Expected:

```text
Original Hash:
ABC123...

Current Hash:
XYZ789...

⚠️ FILE CHANGE DETECTED
```

---

## 🟠 Task 5 — JSON Baseline

Create:

```text
baseline.json
```

Store:

```text
filename
hash
```

for several files.

Then write a program that compares the current hashes against the baseline.

---

## 🔴 Task 6 — Complete Monitoring Tool

Build a small monitoring program:

```text
        SECURITY MONITOR
              │
      ┌───────┼────────┐
      ↓       ↓        ↓
    System  Service   Files
      │       │        │
      └───────┼────────┘
              ↓
         Check Rules
              ↓
         ┌────┴────┐
         ↓         ↓
       Normal    Changed
         ↓         ↓
       Continue  Generate Alert
                    ↓
                 Log Alert
```

Your output could look like:

```text
================================
     SECURITY MONITOR
================================

[OK] CPU usage: 32%
[OK] Memory usage: 58%
[OK] SSH service: active
[OK] config.txt: unchanged

================================
No security events detected.
================================
```

Or:

```text
================================
     SECURITY MONITOR
================================

[OK] CPU usage: 32%
[OK] Memory usage: 58%
[OK] SSH service: active

[ALERT] config.txt was modified!

================================
1 security event detected.
================================
```

---

# 🧠 18. Memory Tricks

### Monitoring components

> **S → S → F → C → A**

```text
S = System
S = Service
F = File
C = Change
A = Alert
```

### Monitoring workflow

> **WATCH → CHECK → COMPARE → ALERT**

```text
Watch resource
      ↓
Check current state
      ↓
Compare with baseline/rule
      ↓
Alert if necessary
```

### File integrity

> **HASH → SAVE → RECHECK → COMPARE**

---

# 🎯 Interview Questions

### What is system monitoring?

System monitoring is the process of observing system resources and activity such as CPU, memory, processes, disk usage, and network connections.

### What is service monitoring?

Service monitoring checks whether required services are running and operating as expected.

### What is file integrity monitoring?

File integrity monitoring detects changes to important files, often by comparing their current state or cryptographic hashes against a known baseline.

### Why are hashes useful for file monitoring?

A cryptographic hash provides a representation of file contents. If the contents change, the resulting hash will normally change as well.

### What is a baseline?

A baseline is a known reference state used to compare against the current state of a system or resource.

### What is alerting?

Alerting is the process of notifying an analyst, administrator, or another system when a defined condition is detected.

### Monitoring vs alerting?

Monitoring observes the system. Alerting communicates when a monitored condition meets a defined rule.

### Does a file modification mean malware?

No. A file modification can be legitimate or malicious. The change is an event requiring appropriate context and investigation.

---

# ⚡ Quick Revision

```text
📡 MONITORING AUTOMATION
│
├── 🖥️ System Monitoring
│   ├── CPU
│   ├── Memory
│   ├── Disk
│   └── Processes
│
├── ⚙️ Service Monitoring
│   ├── Running
│   ├── Stopped
│   └── Failed
│
├── 📁 File Monitoring
│   ├── Size
│   ├── Modification time
│   └── Hash
│
├── 🔄 Change Detection
│   ├── Baseline
│   ├── Current state
│   └── Comparison
│
└── 🚨 Alerting
    ├── Detection rule
    ├── Alert
    ├── Logging
    └── Notification
```

---

# 🛡️ Final Takeaway

Monitoring automation turns a passive system into something that can continuously watch for changes.

```text
             SYSTEM
                ↓
             MONITOR
                ↓
          COLLECT STATE
                ↓
          COMPARE / CHECK
                ↓
          DETECT CHANGE
                ↓
             ALERT
                ↓
        HUMAN INVESTIGATION
```

The most important principle is:

> **Monitoring tells you what changed; detection determines whether the change matches a condition of interest; alerting brings that condition to someone's attention.**

And remember:

> 🔥 **WATCH → CHECK → COMPARE → DETECT → ALERT**
