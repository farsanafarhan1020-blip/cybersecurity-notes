# 🛠️ 10. Apply Through Hands-on Tasks

Learning Python becomes valuable when you can use it to solve real problems.

In cybersecurity, Python is commonly used to:

* 🔧 Build small security utilities
* 📊 Analyze security logs
* 📦 Process JSON datasets
* ♻️ Create reusable scripts
* ⚙️ Automate repetitive security workflows

The goal of this section is to move from:

```text
📚 Learning Python
       ↓
💻 Writing Code
       ↓
🔐 Solving Security Problems
       ↓
⚙️ Automating Tasks
```

---

# 🧩 1. Build Python Utilities

## What is a Python Utility?

A utility is a small program designed to perform a specific task.

Instead of building one huge security application, we can create many small tools.

```text
                 Python Security Toolkit
                         │
       ┌─────────────────┼─────────────────┐
       ▼                 ▼                 ▼
  IP Validator       Hash Tool        Port Checker
       │                 │                 │
       ▼                 ▼                 ▼
   Validate IP       Hash Files       Check Ports
```

Small tools are easier to:

* Understand
* Test
* Debug
* Reuse
* Maintain

---

# 🔎 Utility Example: IP Validator

We can create a simple utility that checks whether an input is a valid IP address.

Python provides the `ipaddress` module for this.

```python
import ipaddress

def validate_ip(ip):
    try:
        address = ipaddress.ip_address(ip)
        return f"{address} is valid"
    except ValueError:
        return f"{ip} is invalid"


ip = input("Enter an IP address: ")

print(validate_ip(ip))
```

Example:

```text
Enter an IP address: 192.168.1.10

192.168.1.10 is valid
```

---

# 🔐 Utility Example: File Hash Generator

Hashing is useful for checking file integrity.

```python
import hashlib

def calculate_hash(filename):
    sha256 = hashlib.sha256()

    with open(filename, "rb") as file:
        while chunk := file.read(4096):
            sha256.update(chunk)

    return sha256.hexdigest()


filename = input("Enter filename: ")

print("SHA-256:", calculate_hash(filename))
```

Flow:

```text
File
 ↓
Read bytes
 ↓
SHA-256
 ↓
Hash
 ↓
Compare later
```

If the file changes, its hash normally changes too.

---

# 🔌 Utility Example: TCP Port Checker

Using the `socket` module:

```python
import socket

def check_port(host, port):
    sock = socket.socket(
        socket.AF_INET,
        socket.SOCK_STREAM
    )

    sock.settimeout(1)

    result = sock.connect_ex((host, port))

    sock.close()

    return result == 0


host = "127.0.0.1"

for port in [22, 80, 443, 8080]:
    if check_port(host, port):
        print(f"{port}: OPEN")
    else:
        print(f"{port}: CLOSED")
```

This demonstrates the basic concept behind a network utility.

> ⚠️ Only scan systems you own or have explicit permission to test.

---

# 🧠 Principles of Good Security Utilities

A useful utility should generally:

```text
Input
  ↓
Validate
  ↓
Process
  ↓
Handle Errors
  ↓
Output
```

For example:

```text
User Input
    ↓
IP Validation
    ↓
Network Operation
    ↓
Exception Handling
    ↓
Result
```

---

# 📊 2. Process Security Logs

Security systems generate enormous amounts of logs.

Examples:

```text
Authentication logs
Web server logs
Firewall logs
SSH logs
Application logs
System logs
```

Manually reading thousands of lines is inefficient.

Python can automate the process.

```text
Security Log
     ↓
Python
     ↓
Parse
     ↓
Filter
     ↓
Analyze
     ↓
Generate Alert/Report
```

---

# 📄 Example Security Log

Suppose we have:

```text
Failed login from 192.168.1.20
Successful login from 192.168.1.10
Failed login from 192.168.1.20
Failed login from 192.168.1.20
Failed login from 10.0.0.5
```

We can count failed login attempts.

---

# 🔎 Basic Log Analyzer

```python
from collections import Counter

failed_ips = []

with open("auth.log", "r") as file:

    for line in file:

        if "Failed login" in line:

            ip = line.split()[-1]

            failed_ips.append(ip)


counts = Counter(failed_ips)

for ip, count in counts.items():
    print(ip, "→", count, "failed attempts")
```

Possible output:

```text
192.168.1.20 → 3 failed attempts
10.0.0.5 → 1 failed attempts
```

---

# 🚨 Detecting Suspicious Activity

We can create a simple threshold.

```python
THRESHOLD = 5
```

Then:

```python
for ip, count in counts.items():

    if count >= THRESHOLD:
        print(
            f"ALERT: {ip} has {count} failed logins"
        )
```

Flow:

```text
Logs
 ↓
Extract IPs
 ↓
Count attempts
 ↓
Compare threshold
 ↓
🚨 Alert
```

This is a simplified example of a pattern that can be used in larger security monitoring systems.

---

# ⚠️ Real-World Considerations

A simple threshold does **not** automatically mean an attack occurred.

For example:

```text
5 failed logins
```

could be:

* A user forgetting their password
* A misconfigured application
* A legitimate administrative task
* An automated service
* An actual attack

Therefore:

> **Detection ≠ Confirmation**

Security tools identify activity that deserves investigation.

---

# 📦 3. Parse JSON Datasets

JSON is extremely common in cybersecurity.

Security data can come from:

```text
Threat intelligence APIs
Security tools
SIEM systems
Vulnerability databases
Cloud services
Log systems
```

Example dataset:

```json
[
    {
        "ip": "192.168.1.10",
        "severity": "low"
    },
    {
        "ip": "192.168.1.20",
        "severity": "high"
    },
    {
        "ip": "10.0.0.5",
        "severity": "medium"
    }
]
```

---

# 📥 Reading a JSON Dataset

```python
import json

with open("alerts.json", "r") as file:
    alerts = json.load(file)

for alert in alerts:
    print(
        alert["ip"],
        "→",
        alert["severity"]
    )
```

Output:

```text
192.168.1.10 → low
192.168.1.20 → high
10.0.0.5 → medium
```

---

# 🚨 Filter High-Severity Alerts

```python
for alert in alerts:

    if alert["severity"] == "high":
        print(
            "HIGH ALERT:",
            alert["ip"]
        )
```

---

# 📊 Count Severity Levels

```python
from collections import Counter

severity_counts = Counter(
    alert["severity"]
    for alert in alerts
)

print(severity_counts)
```

Possible result:

```text
Counter({
    'high': 1,
    'medium': 1,
    'low': 1
})
```

---

# 🔄 JSON Processing Workflow

```text
JSON Dataset
     ↓
Load
     ↓
Parse
     ↓
Filter
     ↓
Analyze
     ↓
Transform
     ↓
Report
```

---

# ♻️ 4. Create Reusable Scripts

One of the most important programming concepts is **reusability**.

Instead of writing:

```python
# Code for IP validation

# Same code again

# Same code again
```

create a function:

```python
def validate_ip(ip):
    ...
```

Then reuse it:

```python
validate_ip("192.168.1.1")
validate_ip("10.0.0.5")
validate_ip("8.8.8.8")
```

---

# 🧩 Reusable Code Structure

A good security script can be divided into components:

```text
security_tool/
│
├── main.py
├── network.py
├── file_utils.py
├── log_parser.py
└── config.py
```

Each module has a specific responsibility.

---

# 🔧 Example

### `network.py`

```python
import socket

def check_port(host, port):

    sock = socket.socket(
        socket.AF_INET,
        socket.SOCK_STREAM
    )

    sock.settimeout(1)

    result = sock.connect_ex(
        (host, port)
    )

    sock.close()

    return result == 0
```

### `main.py`

```python
from network import check_port

host = "127.0.0.1"

for port in [22, 80, 443]:

    status = check_port(host, port)

    print(
        f"{host}:{port} → "
        f"{'OPEN' if status else 'CLOSED'}"
    )
```

Now the network functionality can be reused by other programs.

---

# 🧠 Why Reusability Matters

Reusable scripts provide:

* ♻️ Less duplicated code
* 🐛 Easier debugging
* 🔧 Easier maintenance
* 🧪 Easier testing
* 📦 Easier integration
* 📈 Easier expansion

---

# ⚙️ 5. Automate Simple Security Workflows

Automation means allowing a program to perform multiple steps automatically.

Instead of:

```text
Run command
 ↓
Copy result
 ↓
Open file
 ↓
Analyze
 ↓
Create report
```

Python can automate the workflow:

```text
Python
  ↓
Collect
  ↓
Analyze
  ↓
Process
  ↓
Generate Report
```

---

# 🔄 Example Security Workflow

Imagine a simple workflow:

```text
1. Check website
       ↓
2. Get HTTP status
       ↓
3. Resolve IP
       ↓
4. Collect information
       ↓
5. Store results
```

Python can combine:

```text
requests + socket + json + os
```

---

# 🧪 Simple Automation Example

```python
import socket
import requests
import json

target = "example.com"

# DNS resolution
ip = socket.gethostbyname(target)

# HTTP request
response = requests.get(
    f"https://{target}",
    timeout=5
)

# Build result
result = {
    "target": target,
    "ip": ip,
    "status_code": response.status_code,
    "server": response.headers.get("Server")
}

# Display JSON report
print(
    json.dumps(
        result,
        indent=4
    )
)
```

This combines several skills from previous topics.

---

# 📁 Saving the Report

We can save the result:

```python
import json

with open("report.json", "w") as file:
    json.dump(
        result,
        file,
        indent=4
    )
```

Now the workflow becomes:

```text
Target
  ↓
DNS Resolution
  ↓
HTTP Request
  ↓
Collect Information
  ↓
Create Dictionary
  ↓
Convert to JSON
  ↓
Save Report
```

---

# 🤖 Using Subprocess for Automation

Python can also automate existing command-line tools.

For example:

```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

The general concept is:

```text
Python
  ↓
subprocess
  ↓
External Tool
  ↓
Output
  ↓
Python
  ↓
Analysis
```

This allows Python to act as an automation layer around other tools.

---

# 🔐 Important Security Rule

Be extremely careful when executing commands.

Avoid:

```python
subprocess.run(
    user_input,
    shell=True
)
```

when `user_input` is untrusted.

This can introduce **command injection**.

Prefer structured arguments where possible:

```python
subprocess.run(
    ["python", "--version"]
)
```

---

# 🧩 Putting Everything Together

At this point, you've learned:

```text
Python Fundamentals
        ↓
Program Flow
        ↓
Collections
        ↓
Functions
        ↓
File Handling
        ↓
Exception Handling
        ↓
Modules
        ↓
Security Libraries
        ↓
AI-Assisted Development
        ↓
Hands-on Security Automation
```

This is where the individual concepts start becoming one skill.

---

# 🏗️ Mini Project 1 — Security Log Analyzer

Build a program that:

### Input

```text
auth.log
```

### Program should:

1. Read the log.
2. Find failed login attempts.
3. Extract IP addresses.
4. Count attempts per IP.
5. Identify IPs above a threshold.
6. Generate a report.

Architecture:

```text
auth.log
   ↓
Read File
   ↓
Parse Lines
   ↓
Extract IP
   ↓
Count
   ↓
Threshold Check
   ↓
🚨 Alert
   ↓
JSON Report
```

---

# 🏗️ Mini Project 2 — JSON Security Analyzer

Create:

```text
alerts.json
```

Then build a program that:

1. Loads the JSON.
2. Counts severity levels.
3. Displays high-severity alerts.
4. Groups alerts by IP.
5. Generates a summary.

Example output:

```text
===== SECURITY REPORT =====

Total Alerts: 25

High:   5
Medium: 10
Low:    10

High Severity IPs:
192.168.1.20
10.0.0.15
```

---

# 🏗️ Mini Project 3 — Network Utility

Create a simple utility that:

1. Accepts a hostname/IP.
2. Resolves the hostname.
3. Checks selected TCP ports.
4. Displays open/closed status.
5. Handles errors.
6. Saves the results as JSON.

Architecture:

```text
Target
  ↓
Validate
  ↓
Resolve
  ↓
Port Checks
  ↓
Collect Results
  ↓
JSON
  ↓
Report
```

Only perform network checks against systems you own or are explicitly authorized to test.

---

# 🏗️ Mini Project 4 — Security Workflow Automation

Create a script that:

```text
Input Target
     ↓
Resolve IP
     ↓
Check Connectivity
     ↓
Collect HTTP Information
     ↓
Process Results
     ↓
Generate JSON
     ↓
Save Report
```

Suggested modules:

```text
main.py
network.py
web.py
report.py
```

This will give you experience with **reusable security tooling**.

---

# 🤖 Using AI During These Projects

AI can assist you, but follow the workflow from Topic 9:

```text
Problem
  ↓
Ask AI
  ↓
Understand suggestion
  ↓
Write/modify code
  ↓
Review
  ↓
Test
  ↓
Security review
```

For example:

```text
"Review my log analyzer.

Check:
- parsing errors
- incorrect assumptions
- exception handling
- performance
- security problems

Explain every finding instead of simply rewriting the code."
```

---

# 🔐 Security Checklist

Before considering a security utility complete:

### Input

* [ ] Is input validated?
* [ ] Are unexpected inputs handled?

### Errors

* [ ] Are exceptions handled?
* [ ] Does the program fail safely?

### Files

* [ ] Are files opened safely?
* [ ] Are paths handled correctly?

### Network

* [ ] Are timeouts used?
* [ ] Are network errors handled?

### Commands

* [ ] Is `subprocess` used safely?
* [ ] Is untrusted input prevented from becoming a command?

### Secrets

* [ ] Are passwords/API keys excluded from source code?
* [ ] Are sensitive values protected?

### Output

* [ ] Does the program produce useful results?
* [ ] Is sensitive information unnecessarily exposed?

---

# 🧠 The Security Automation Mindset

Don't think:

> "I need to write a big cybersecurity application."

Think:

> "What repetitive security task can I automate?"

For example:

```text
Repetitive Task
      ↓
Can Python do it?
      ↓
Build small utility
      ↓
Make it reusable
      ↓
Add error handling
      ↓
Add logging/reporting
      ↓
Automate workflow
```

This is how small scripts can eventually become useful security tools.

---

# 🧠 Memory Trick

Remember:

> **BUILD → ANALYZE → PARSE → REUSE → AUTOMATE**

```text
🔧 BUILD
   ↓
📊 ANALYZE
   ↓
📦 PARSE
   ↓
♻️ REUSE
   ↓
⚙️ AUTOMATE
```

---

# 🔄 Quick Revision

### 1. Build Python Utilities

Create focused tools for specific tasks.

```text
IP Validator
Hash Generator
Port Checker
```

### 2. Process Security Logs

```text
Read → Parse → Filter → Count → Alert
```

### 3. Parse JSON Datasets

```text
Load → Extract → Filter → Analyze → Report
```

### 4. Create Reusable Scripts

Use:

```text
Functions
+
Modules
+
Clear responsibilities
```

### 5. Automate Security Workflows

Combine Python capabilities:

```text
requests
json
os
socket
subprocess
```

with:

```text
functions
files
exceptions
modules
```

to automate repetitive tasks.

---

# 💼 Interview Tip

### Question:

**"How can Python be used in cybersecurity?"**

A strong answer:

> Python can be used to automate repetitive security tasks, analyze logs, process JSON and other security data, interact with networks and APIs, build security utilities, and integrate existing command-line tools. Python's functions, modules, exception handling, and libraries such as `requests`, `socket`, `os`, `json`, and `subprocess` make it useful for developing reusable security automation scripts.

---

# 🎯 Final Practical Goal

By the end of this topic, you should be able to take a problem like:

> "There are thousands of authentication logs. Find suspicious failed-login activity and generate a report."

and think:

```text
📄 Log File
    ↓
🐍 Python
    ↓
📖 Read
    ↓
🔍 Parse
    ↓
📊 Analyze
    ↓
🚨 Detect
    ↓
📦 JSON
    ↓
📝 Report
```

That is the transition from **learning Python** to **using Python as a cybersecurity tool**.

---

# 📌 Final Summary

| Task                   | Python Skills Used                       |
| ---------------------- | ---------------------------------------- |
| 🔧 Build utilities     | Functions, modules                       |
| 📊 Process logs        | Files, strings, collections              |
| 📦 Parse JSON          | `json`, dictionaries/lists               |
| ♻️ Reusable scripts    | Functions, modules                       |
| ⚙️ Automate workflows  | `requests`, `socket`, `subprocess`, `os` |
| 🔐 Make tools reliable | Exception handling                       |
| 🤖 Improve development | AI-assisted development                  |

### 🔥 Final Principle

> **Don't just write Python code. Build small, reliable, reusable tools that solve real cybersecurity problems.**

```text
Learn
  ↓
Practice
  ↓
Build
  ↓
Test
  ↓
Secure
  ↓
Automate
  ↓
Repeat
```

This is the foundation for moving from **Python programming → cybersecurity scripting → security automation**.
