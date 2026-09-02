# 🐍 Understand File Handling

> **"File handling allows Python programs to create, read, modify, and process data stored in files."**

---

# 📖 What is File Handling?

**File handling** is the process of using a program to interact with files stored on a computer.

Python can work with many types of files:

```text
Text Files       → .txt
CSV Files        → .csv
JSON Files       → .json
Log Files        → .log
Configuration    → .conf / .ini
Source Code      → .py
```

In cybersecurity, file handling is extremely important because security tools frequently need to:

* Read log files
* Analyze suspicious files
* Store scan results
* Process security reports
* Read configuration files
* Process CSV datasets
* Parse JSON API responses
* Generate reports

---

# ⚙️ How File Handling Works

The basic file-handling workflow is:

```text
        File on Disk
             │
             ▼
        open() File
             │
       ┌─────┴─────┐
       ▼           ▼
     Read        Write
       │           │
       └─────┬─────┘
             ▼
          Process
             │
             ▼
          close()
```

Python provides the `open()` function for working with files.

---

# 📂 1. Reading Files

Reading means retrieving data from a file into your Python program.

Suppose we have:

```text
notes.txt
```

Containing:

```text
Python is useful.
Python can automate tasks.
Python is widely used in cybersecurity.
```

We can read it using:

```python
file = open("notes.txt", "r")

content = file.read()

print(content)

file.close()
```

Output:

```text
Python is useful.
Python can automate tasks.
Python is widely used in cybersecurity.
```

---

# 🔑 File Opening Modes

The second argument of `open()` specifies what we want to do with the file.

| Mode | Meaning         |
| ---- | --------------- |
| `r`  | Read            |
| `w`  | Write           |
| `a`  | Append          |
| `x`  | Create new file |
| `rb` | Read binary     |
| `wb` | Write binary    |

Example:

```python
open("data.txt", "r")
```

means:

> Open `data.txt` for reading.

---

# 📖 `read()`

`read()` reads the entire file.

```python
with open("notes.txt", "r") as file:
    content = file.read()

print(content)
```

Using `with` is preferred because Python automatically handles closing the file.

---

# 📄 `readline()`

`readline()` reads one line.

```python
with open("notes.txt", "r") as file:
    line = file.readline()

print(line)
```

To read multiple lines:

```python
with open("notes.txt", "r") as file:
    print(file.readline())
    print(file.readline())
```

---

# 📚 `readlines()`

`readlines()` reads all lines and stores them as a list.

```python
with open("notes.txt", "r") as file:
    lines = file.readlines()

print(lines)
```

Example result:

```python
[
    "Python is useful.\n",
    "Python can automate tasks.\n",
    "Python is widely used in cybersecurity.\n"
]
```

---

# 🔄 Reading Line by Line

For large files, processing one line at a time is often better.

```python
with open("server.log", "r") as file:
    for line in file:
        print(line)
```

This is especially useful for **large log files**.

---

# 🛡️ Cybersecurity Example — Searching a File

Suppose:

```text
server.log
```

contains:

```text
User login successful
User login successful
FAILED LOGIN from 192.168.1.10
User login successful
FAILED LOGIN from 10.0.0.5
```

We can search for failed logins:

```python
with open("server.log", "r") as file:
    for line in file:
        if "FAILED LOGIN" in line:
            print(line.strip())
```

Output:

```text
FAILED LOGIN from 192.168.1.10
FAILED LOGIN from 10.0.0.5
```

This is a simple example of **log analysis**.

---

# ✍️ 2. Writing Files

Writing means storing data from Python into a file.

Example:

```python
with open("report.txt", "w") as file:
    file.write("Security scan completed.")
```

Python creates:

```text
report.txt
```

Containing:

```text
Security scan completed.
```

---

# ⚠️ The `w` Mode

Be careful with:

```python
open("file.txt", "w")
```

`w` means **write**.

If the file already exists, its previous contents can be overwritten.

Example:

Original:

```text
Hello
World
```

Then:

```python
with open("file.txt", "w") as file:
    file.write("New content")
```

The file becomes:

```text
New content
```

The old content is gone.

---

# ➕ Append Mode

Use `a` when you want to add content to the end of a file.

```python
with open("log.txt", "a") as file:
    file.write("New security event\n")
```

Existing content remains.

```text
Old event
Another event
New security event
```

---

# 🆚 Write vs Append

| Mode | Existing Content | New Content  |
| ---- | ---------------- | ------------ |
| `w`  | Replaced         | Written      |
| `a`  | Preserved        | Added at end |

### Memory Trick

```text
W → Wipe and Write
A → Add
```

---

# 📝 Writing Multiple Lines

```python
lines = [
    "Scan started\n",
    "Port 22 detected\n",
    "Scan completed\n"
]

with open("scan.log", "w") as file:
    file.writelines(lines)
```

---

# 🔒 File Closing

A file should be closed after use.

Manual approach:

```python
file = open("data.txt", "r")

content = file.read()

file.close()
```

Preferred approach:

```python
with open("data.txt", "r") as file:
    content = file.read()
```

The `with` statement automatically handles closing the file.

### Recommended Pattern

```python
with open("filename", "mode") as file:
    # work with file
```

---

# 📊 3. CSV Processing

**CSV** stands for:

> **Comma-Separated Values**

CSV files are commonly used to store tabular data.

Example:

```text
username,ip,status
farhan,192.168.1.10,success
admin,192.168.1.20,failed
guest,192.168.1.30,success
```

CSV files are widely used for:

* Reports
* User lists
* Network data
* Security datasets
* Vulnerability reports
* Exported logs

---

# 🐍 Python CSV Module

Python provides the built-in `csv` module.

```python
import csv
```

---

# 📖 Reading CSV Files

Example:

```python
import csv

with open("users.csv", "r") as file:
    reader = csv.reader(file)

    for row in reader:
        print(row)
```

Output:

```text
['username', 'ip', 'status']
['farhan', '192.168.1.10', 'success']
['admin', '192.168.1.20', 'failed']
['guest', '192.168.1.30', 'success']
```

Each row becomes a list.

---

# 🧾 CSV as Dictionary

For structured data, `DictReader` can be easier.

```python
import csv

with open("users.csv", "r") as file:
    reader = csv.DictReader(file)

    for row in reader:
        print(row["username"])
```

Output:

```text
farhan
admin
guest
```

---

# 🔐 Cybersecurity Example — Find Failed Accounts

```python
import csv

with open("users.csv", "r") as file:
    reader = csv.DictReader(file)

    for row in reader:
        if row["status"] == "failed":
            print("Failed login:", row["username"])
```

Output:

```text
Failed login: admin
```

---

# ✍️ Writing CSV Files

```python
import csv

with open("results.csv", "w", newline="") as file:
    writer = csv.writer(file)

    writer.writerow(["IP", "Port", "Status"])
    writer.writerow(["192.168.1.10", 22, "Open"])
    writer.writerow(["192.168.1.10", 80, "Open"])
```

Result:

```text
IP,Port,Status
192.168.1.10,22,Open
192.168.1.10,80,Open
```

---

# 🗂️ 4. JSON Processing

**JSON** stands for:

> **JavaScript Object Notation**

JSON is a popular format for exchanging structured data.

Example:

```json
{
    "username": "Farhan",
    "role": "student",
    "skills": [
        "Python",
        "Linux",
        "Cybersecurity"
    ]
}
```

JSON is extremely common in:

* APIs
* Web applications
* Configuration files
* Security tools
* Cloud services
* Threat intelligence platforms

---

# 🐍 Python JSON Module

Python provides the built-in `json` module.

```python
import json
```

---

# 📖 Reading JSON

Suppose `user.json` contains:

```json
{
    "username": "Farhan",
    "role": "student",
    "active": true
}
```

Python:

```python
import json

with open("user.json", "r") as file:
    data = json.load(file)

print(data)
```

Output:

```python
{'username': 'Farhan', 'role': 'student', 'active': True}
```

The JSON object becomes a Python dictionary.

---

# 🔍 Accessing JSON Data

```python
print(data["username"])
print(data["role"])
print(data["active"])
```

Output:

```text
Farhan
student
True
```

---

# 🧩 JSON and Python Data Types

| JSON   | Python     |
| ------ | ---------- |
| object | dictionary |
| array  | list       |
| string | string     |
| number | int/float  |
| true   | `True`     |
| false  | `False`    |
| null   | `None`     |

---

# ✍️ Writing JSON

```python
import json

data = {
    "username": "Farhan",
    "role": "student",
    "active": True
}

with open("user.json", "w") as file:
    json.dump(data, file, indent=4)
```

The `indent=4` makes the JSON easier to read.

Result:

```json
{
    "username": "Farhan",
    "role": "student",
    "active": true
}
```

---

# 🔄 `load()` vs `loads()`

These are commonly confused.

### `load()`

Reads JSON from a **file**.

```python
json.load(file)
```

### `loads()`

Reads JSON from a **string**.

```python
data = '{"name": "Farhan"}'

result = json.loads(data)
```

---

# 💾 `dump()` vs `dumps()`

### `dump()`

Writes JSON to a file.

```python
json.dump(data, file)
```

### `dumps()`

Converts Python data into a JSON string.

```python
json_data = json.dumps(data)
```

### Memory Trick

```text
load   → File → Python
loads  → String → Python

dump   → Python → File
dumps  → Python → String
```

---

# 📝 5. Log File Handling

A **log file** records events that happen inside a system, application, server, or network device.

Examples:

```text
system.log
server.log
access.log
error.log
auth.log
firewall.log
```

Logs are extremely important in cybersecurity.

They can help identify:

* Failed logins
* Successful logins
* Suspicious activity
* Errors
* Network connections
* Authentication attempts
* Malware activity
* System changes

---

# 🔍 Example Log

```text
2026-09-02 10:15:21 INFO User farhan logged in
2026-09-02 10:16:05 INFO User admin logged in
2026-09-02 10:18:42 WARNING Failed login from 192.168.1.10
2026-09-02 10:19:03 WARNING Failed login from 192.168.1.10
2026-09-02 10:19:30 WARNING Failed login from 192.168.1.10
```

Python can process this automatically.

---

# 🛡️ Detect Failed Login Attempts

```python
with open("auth.log", "r") as file:
    for line in file:
        if "Failed login" in line:
            print("ALERT:", line.strip())
```

Output:

```text
ALERT: 2026-09-02 10:18:42 WARNING Failed login from 192.168.1.10
ALERT: 2026-09-02 10:19:03 WARNING Failed login from 192.168.1.10
ALERT: 2026-09-02 10:19:30 WARNING Failed login from 192.168.1.10
```

---

# 🧮 Counting Events

We can count how many failed login attempts occurred.

```python
failed_logins = 0

with open("auth.log", "r") as file:
    for line in file:
        if "Failed login" in line:
            failed_logins += 1

print("Failed logins:", failed_logins)
```

Output:

```text
Failed logins: 3
```

---

# 🚨 Simple Security Detection

We can create a basic threshold.

```python
failed_logins = 0

with open("auth.log", "r") as file:
    for line in file:
        if "Failed login" in line:
            failed_logins += 1

if failed_logins >= 3:
    print("WARNING: Multiple failed login attempts detected!")
```

This demonstrates the basic idea behind automated security monitoring.

> Real security systems use much more sophisticated detection, correlation, authentication context, time windows, and alerting.

---

# 🧹 `strip()` for Log Processing

When reading lines:

```python
line = "Failed login\n"
```

The `\n` represents a newline.

Using:

```python
line.strip()
```

removes unnecessary whitespace and the newline.

Example:

```python
with open("auth.log", "r") as file:
    for line in file:
        print(line.strip())
```

---

# 🔎 Searching Logs with Keywords

```python
keywords = ["ERROR", "WARNING", "FAILED"]

with open("server.log", "r") as file:
    for line in file:
        for keyword in keywords:
            if keyword in line:
                print(line.strip())
                break
```

This can identify potentially interesting events.

---

# 🧠 File Handling Workflow for Security Analysis

A simple security-analysis script may follow:

```text
          Log File
             │
             ▼
        Open File
             │
             ▼
       Read Line
             │
             ▼
       Parse Data
             │
             ▼
      Search Patterns
             │
       ┌─────┴─────┐
       ▼           ▼
   Normal       Suspicious
       │           │
       │           ▼
       │         Alert
       │           │
       └─────┬─────┘
             ▼
        Generate Report
```

---

# 🆚 TXT vs CSV vs JSON vs LOG

| Format  | Structure            | Common Use             |
| ------- | -------------------- | ---------------------- |
| `.txt`  | Plain text           | Notes/simple data      |
| `.csv`  | Rows & columns       | Tables/datasets        |
| `.json` | Structured key-value | APIs/configuration     |
| `.log`  | Event records        | System/security events |

---

# 🔐 File Handling & Security

When writing security-related Python programs, remember:

### 1. Validate File Paths

Don't blindly trust file paths supplied by users.

### 2. Handle Missing Files

```python
try:
    with open("data.txt", "r") as file:
        data = file.read()
except FileNotFoundError:
    print("File not found")
```

### 3. Handle Invalid JSON

```python
import json

try:
    with open("data.json", "r") as file:
        data = json.load(file)
except json.JSONDecodeError:
    print("Invalid JSON")
```

### 4. Don't Expose Sensitive Data

Avoid accidentally printing:

```text
Passwords
API keys
Tokens
Private keys
Personal information
```

### 5. Don't Trust File Contents

A file's extension does not guarantee its actual contents.

For example:

```text
malware.exe
```

should not be treated as safe simply because a script sees a filename.

---

# ⚠️ Common Mistakes

### Mistake 1 — Forgetting `with`

Less preferable:

```python
file = open("data.txt")
data = file.read()
```

Preferred:

```python
with open("data.txt") as file:
    data = file.read()
```

---

### Mistake 2 — Using `w` Accidentally

```python
open("important.log", "w")
```

can overwrite existing contents.

Use:

```python
open("important.log", "a")
```

when you intend to append.

---

### Mistake 3 — Wrong File Path

```python
open("data.txt")
```

Python searches relative to the program's current working directory.

You can check it with:

```python
import os

print(os.getcwd())
```

---

### Mistake 4 — Assuming Every File is Text

Binary files such as images, executables, and some documents should not be treated like ordinary text files.

Binary mode:

```python
with open("file.bin", "rb") as file:
    data = file.read()
```

---

# 🧠 Memory Trick

Remember:

```text
TXT  → Plain Text
CSV  → Tables
JSON → Structured Data
LOG  → Events
```

And for Python:

```text
open()
  │
  ├── r → Read
  ├── w → Write
  ├── a → Append
  └── x → Create
```

For JSON:

```text
load   → File → Python
loads  → String → Python
dump   → Python → File
dumps  → Python → String
```

---

# 📝 Quick Revision

### Read a file

```python
with open("data.txt", "r") as file:
    data = file.read()
```

### Write a file

```python
with open("data.txt", "w") as file:
    file.write("Hello")
```

### Append

```python
with open("data.txt", "a") as file:
    file.write("New line\n")
```

### Read CSV

```python
import csv

with open("data.csv") as file:
    reader = csv.reader(file)

    for row in reader:
        print(row)
```

### Read JSON

```python
import json

with open("data.json") as file:
    data = json.load(file)
```

### Write JSON

```python
with open("data.json", "w") as file:
    json.dump(data, file, indent=4)
```

### Analyze logs

```python
with open("server.log") as file:
    for line in file:
        if "ERROR" in line:
            print(line.strip())
```

---

# 💡 Interview Tip

### Q: What is file handling?

> File handling is the process of reading, writing, creating, and modifying files using a program.

### Q: What is the difference between `r`, `w`, and `a`?

> `r` reads a file, `w` writes and can overwrite existing content, while `a` appends data to the end of a file.

### Q: Why is `with open()` preferred?

> It automatically manages the file and ensures that the file is properly closed after the operation.

### Q: What is CSV?

> CSV stands for Comma-Separated Values and is commonly used to represent tabular data.

### Q: What is JSON?

> JSON is a lightweight structured data format commonly used for APIs, configuration files, and data exchange.

### Q: Why is log analysis important in cybersecurity?

> Logs provide records of system and application events that can be analyzed to detect errors, suspicious behavior, and potential security incidents.

---

# 🧪 Mini Practice

### Task 1 — Read a File

Create a `notes.txt` file and write a Python program that prints its contents.

---

### Task 2 — Count Lines

Write a program that counts the number of lines in:

```text
server.log
```

---

### Task 3 — Find Errors

Read:

```text
application.log
```

and print only lines containing:

```text
ERROR
```

---

### Task 4 — CSV Analysis

Given:

```text
username,status
farhan,success
admin,failed
guest,failed
```

Write a Python program that prints only users with:

```text
failed
```

---

### Task 5 — JSON

Create:

```text
security.json
```

containing:

```json
{
    "tool": "Nmap",
    "target": "localhost",
    "status": "completed"
}
```

Read it using Python and print:

```text
Tool: Nmap
Target: localhost
Status: completed
```

---

### Task 6 — Security Log Counter

Given:

```text
FAILED LOGIN from 192.168.1.10
SUCCESS LOGIN from 192.168.1.20
FAILED LOGIN from 192.168.1.10
FAILED LOGIN from 10.0.0.5
```

Write a program that counts the number of failed login attempts.

Expected:

```text
Failed login attempts: 3
```

---

# 📚 Summary

Python file handling allows programs to interact with data stored on disk.

The major concepts are:

```text
                 FILE HANDLING
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
      Reading       Writing      Appending
        │             │             │
        └─────────────┼─────────────┘
                      ▼
                  Processing
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
         TXT         CSV         JSON
                                  │
                                  ▼
                                Logs
                                  │
                                  ▼
                           Security Analysis
```

> **Remember:**

> `open()` is used to interact with files.

> `r` reads, `w` writes, and `a` appends.

> Prefer `with open(...)` for safe file management.

> CSV is useful for tabular data.

> JSON is useful for structured data and APIs.

> Log files contain records of system and application events.

> File handling is an essential skill for cybersecurity automation, log analysis, reporting, and security tooling.

That completes **Topic 5: File Handling**. The next syllabus topic can continue in exactly this format.
