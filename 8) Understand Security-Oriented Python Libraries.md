# 🔐 8. Understand Security-Oriented Python Libraries

Python becomes especially powerful in cybersecurity when we combine it with libraries that allow us to:

* 🌐 Communicate with websites and APIs
* 📦 Process structured data
* 💻 Interact with the operating system
* 🔌 Work with network connections
* ⚙️ Execute system commands
* 🤖 Automate repetitive security tasks

The five important libraries in this topic are:

| Library      | Main Purpose              | Cybersecurity Use                                |
| ------------ | ------------------------- | ------------------------------------------------ |
| `requests`   | HTTP requests             | API interaction, web security testing            |
| `json`       | JSON data processing      | APIs, configurations, security data              |
| `os`         | OS interaction            | Files, environment variables, system information |
| `socket`     | Network communication     | Port checks, network tools                       |
| `subprocess` | Execute external programs | Security tool automation                         |

---

# 🌐 1. Requests

## What is `requests`?

`requests` is a Python library used to send **HTTP/HTTPS requests** to web servers.

It allows Python programs to communicate with websites and web APIs.

Think of it as:

```text
Python Program
      │
      │ HTTP Request
      ▼
   Web Server
      │
      │ HTTP Response
      ▼
Python Program
```

For example, when you open:

```text
https://example.com
```

your browser sends an HTTP request to the server.

Python can do something similar using `requests`.

---

## Installing Requests

`requests` is not part of Python's standard library.

Install it using:

```bash
pip install requests
```

---

## Importing Requests

```python
import requests
```

---

# 📤 Sending a GET Request

```python
import requests

response = requests.get("https://example.com")

print(response.status_code)
print(response.text)
```

### What happens?

```text
requests.get()
      ↓
HTTP GET request
      ↓
Web server
      ↓
HTTP response
      ↓
response object
```

---

## HTTP Status Codes

The response contains a status code.

|  Code | Meaning               |
| ----: | --------------------- |
| `200` | OK                    |
| `201` | Created               |
| `301` | Permanent redirect    |
| `302` | Temporary redirect    |
| `400` | Bad request           |
| `401` | Unauthorized          |
| `403` | Forbidden             |
| `404` | Not found             |
| `500` | Internal server error |
| `503` | Service unavailable   |

Example:

```python
if response.status_code == 200:
    print("Website is reachable")
else:
    print("Request failed")
```

---

# 📥 Reading Response Content

### Text response

```python
print(response.text)
```

### Raw bytes

```python
print(response.content)
```

### Headers

```python
print(response.headers)
```

### Specific header

```python
print(response.headers.get("Server"))
```

---

# 📦 Working with JSON APIs

Many APIs return JSON.

Example:

```python
import requests

response = requests.get("https://api.example.com/data")

data = response.json()

print(data)
```

This converts JSON returned by the server into Python objects.

---

# 📤 Sending Parameters

```python
import requests

params = {
    "page": 1,
    "limit": 10
}

response = requests.get(
    "https://example.com/search",
    params=params
)

print(response.url)
```

The parameters may become:

```text
/search?page=1&limit=10
```

---

# 📤 Sending POST Data

```python
import requests

data = {
    "username": "admin",
    "status": "active"
}

response = requests.post(
    "https://example.com/api",
    data=data
)

print(response.status_code)
```

---

# 🔐 Headers

Headers provide additional information to the server.

```python
headers = {
    "User-Agent": "SecurityScanner/1.0"
}

response = requests.get(
    "https://example.com",
    headers=headers
)
```

---

# ⏱️ Timeouts

Never allow a security script to wait forever.

```python
response = requests.get(
    "https://example.com",
    timeout=5
)
```

This means the request should not wait indefinitely.

---

# 🛡️ Cybersecurity Uses of Requests

`requests` can be used for:

* API security testing
* Security automation
* Checking HTTP responses
* Monitoring websites
* Collecting threat intelligence
* Interacting with security APIs
* Checking security headers
* Automating authorized web testing

Example:

```python
import requests

url = "https://example.com"

response = requests.get(url, timeout=5)

print("Status:", response.status_code)
print("Server:", response.headers.get("Server"))
```

---

# ⚠️ Security Considerations

When using `requests`:

### 1. Use timeouts

```python
requests.get(url, timeout=5)
```

### 2. Validate URLs

Don't blindly request URLs supplied by untrusted users.

### 3. Protect credentials

Never hardcode:

```python
password = "MyPassword123"
```

Use environment variables or secure secret storage instead.

### 4. Be careful with redirects

Automatically following redirects can sometimes lead to unexpected destinations.

### 5. Only test systems you are authorized to test.

---

# 📦 2. JSON

## What is JSON?

JSON stands for:

> **JavaScript Object Notation**

It is a lightweight format used to store and exchange structured data.

Example:

```json
{
    "username": "farhan",
    "role": "analyst",
    "active": true
}
```

JSON is extremely common in:

* APIs
* Configuration files
* Security tools
* Log systems
* Threat intelligence
* Web applications

---

# 🐍 Python vs JSON

JSON:

```json
{
    "name": "Alice",
    "age": 25
}
```

Python:

```python
{
    "name": "Alice",
    "age": 25
}
```

Python can convert between JSON and Python objects.

---

# Import JSON

```python
import json
```

---

# 🔄 JSON ↔ Python

The important functions are:

| Function       | Purpose              |
| -------------- | -------------------- |
| `json.loads()` | JSON string → Python |
| `json.dumps()` | Python → JSON string |
| `json.load()`  | JSON file → Python   |
| `json.dump()`  | Python → JSON file   |

Memory trick:

```text
LOAD  = JSON → Python
DUMP  = Python → JSON
```

---

# JSON String → Python

```python
import json

data = '{"username": "admin", "role": "analyst"}'

result = json.loads(data)

print(result["username"])
```

Output:

```text
admin
```

---

# Python → JSON

```python
import json

data = {
    "username": "admin",
    "role": "analyst"
}

result = json.dumps(data)

print(result)
```

---

# Pretty JSON

```python
print(json.dumps(data, indent=4))
```

Output:

```json
{
    "username": "admin",
    "role": "analyst"
}
```

---

# 📄 Reading a JSON File

Suppose `config.json` contains:

```json
{
    "server": "192.168.1.10",
    "port": 443,
    "secure": true
}
```

Python:

```python
import json

with open("config.json", "r") as file:
    config = json.load(file)

print(config["server"])
print(config["port"])
```

---

# ✍️ Writing JSON

```python
import json

data = {
    "tool": "SecurityScanner",
    "version": "1.0",
    "enabled": True
}

with open("config.json", "w") as file:
    json.dump(data, file, indent=4)
```

---

# 🛡️ Cybersecurity Uses of JSON

JSON is commonly used for:

```text
Security Tool
     ↓
Security Data
     ↓
JSON
     ↓
API / File / Database
```

Examples:

* Threat intelligence feeds
* SIEM data
* API responses
* Vulnerability information
* Security configurations
* Scan results
* Incident reports

Example:

```python
alert = {
    "ip": "192.168.1.50",
    "event": "Multiple failed logins",
    "severity": "high"
}

print(json.dumps(alert, indent=4))
```

---

# 💻 3. OS

## What is `os`?

The `os` module allows Python to interact with the **operating system**.

It is part of Python's standard library.

No installation is required.

```python
import os
```

---

# 🖥️ What Can `os` Do?

The `os` module can help Python:

* Work with files
* Work with directories
* Read environment variables
* Get system information
* Create directories
* Rename files
* Remove files/directories
* Work with paths

---

# 📁 Current Working Directory

```python
import os

print(os.getcwd())
```

Example:

```text
/home/farhan/security
```

---

# 📂 List Files

```python
import os

print(os.listdir())
```

You can also specify a directory:

```python
print(os.listdir("/tmp"))
```

---

# 📁 Create a Directory

```python
import os

os.mkdir("reports")
```

---

# 📁 Check Whether a File Exists

```python
import os

if os.path.exists("config.json"):
    print("File exists")
else:
    print("File not found")
```

---

# 📄 Check File or Directory

```python
os.path.isfile("test.txt")
```

```python
os.path.isdir("reports")
```

---

# 🔗 Working with Paths

Use `os.path.join()` instead of manually creating paths.

```python
import os

path = os.path.join("reports", "scan.txt")

print(path)
```

This is better because different operating systems use different path separators.

---

# 🌱 Environment Variables

Environment variables are useful for storing configuration and secrets.

Example:

```bash
export API_KEY="secret_value"
```

Python:

```python
import os

api_key = os.getenv("API_KEY")

print(api_key)
```

Instead of putting secrets directly into source code:

```python
API_KEY = "secret_value"
```

you can keep them outside the program.

---

# 🛡️ Cybersecurity Uses of `os`

The `os` module is useful for:

* Security automation
* File monitoring
* Log processing
* Environment configuration
* Collecting system information
* Security script setup
* Searching directories
* Checking suspicious files

Example:

```python
import os

for file in os.listdir("."):
    print(file)
```

This can be extended into a basic security file inventory tool.

---

# 🔌 4. Socket

## What is a Socket?

A socket is an endpoint used for **network communication**.

Think of it as a communication channel between two systems.

```text
Computer A
    │
    │ Network
    │
    ▼
Computer B
```

A socket allows programs to communicate across that network.

---

# 🌐 IP Address + Port

Network communication commonly involves:

```text
IP Address + Port
```

Example:

```text
192.168.1.10:443
```

Where:

```text
192.168.1.10 → IP address
443          → Port
```

---

# Import Socket

```python
import socket
```

No installation is required.

---

# 🔎 Get Hostname

```python
import socket

print(socket.gethostname())
```

---

# 🌐 Get Local IP

```python
import socket

hostname = socket.gethostname()

ip = socket.gethostbyname(hostname)

print(ip)
```

---

# 🔍 DNS Resolution

```python
import socket

ip = socket.gethostbyname("example.com")

print(ip)
```

Flow:

```text
example.com
     ↓
DNS Resolution
     ↓
IP Address
```

---

# 🔌 Connecting to a Port

A socket can attempt to connect to a specific TCP port.

```python
import socket

sock = socket.socket(
    socket.AF_INET,
    socket.SOCK_STREAM
)

sock.settimeout(2)

result = sock.connect_ex(("127.0.0.1", 80))

if result == 0:
    print("Port is open")
else:
    print("Port is closed or unreachable")

sock.close()
```

### Important:

```text
AF_INET
```

means IPv4.

```text
SOCK_STREAM
```

means TCP.

---

# 🔎 Simple Port Checker

```python
import socket

target = "127.0.0.1"

for port in [22, 80, 443, 8080]:

    sock = socket.socket(
        socket.AF_INET,
        socket.SOCK_STREAM
    )

    sock.settimeout(1)

    result = sock.connect_ex((target, port))

    if result == 0:
        print(f"Port {port}: OPEN")
    else:
        print(f"Port {port}: CLOSED")

    sock.close()
```

This demonstrates the basic principle behind a TCP port checker.

Only scan systems you own or have explicit permission to test.

---

# 🛡️ Cybersecurity Uses of Socket

Sockets are useful for:

* Network programming
* Port checking
* Network monitoring
* Security scanners
* Client/server applications
* Connectivity testing
* Security tool development

---

# ⚙️ 5. Subprocess

## What is `subprocess`?

The `subprocess` module allows Python to run **external programs and system commands**.

It is part of Python's standard library.

```python
import subprocess
```

---

# Why is `subprocess` Important in Cybersecurity?

Many security tools are command-line programs.

Examples:

```text
Python
  │
  ├── Nmap
  ├── Ping
  ├── Netstat/ss
  ├── whois
  ├── Hashing tools
  └── Other security utilities
```

Python can automate these tools using `subprocess`.

---

# ▶️ Running a Command

```python
import subprocess

result = subprocess.run(
    ["python", "--version"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

---

# 📤 Capturing Output

```python
import subprocess

result = subprocess.run(
    ["ping", "-c", "1", "127.0.0.1"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

On Windows, the ping syntax differs:

```python
result = subprocess.run(
    ["ping", "-n", "1", "127.0.0.1"],
    capture_output=True,
    text=True
)
```

---

# ❌ Handling Errors

```python
import subprocess

result = subprocess.run(
    ["some_command"],
    capture_output=True,
    text=True
)

print("Return code:", result.returncode)

if result.returncode != 0:
    print("Command failed")
    print(result.stderr)
```

---

# 🔐 Security Risk: Command Injection

This is extremely important.

Never blindly place user-controlled input into shell commands.

Dangerous pattern:

```python
import subprocess

user_input = input("Enter command: ")

subprocess.run(
    user_input,
    shell=True
)
```

If the input is malicious, the attacker may be able to execute unintended commands.

---

# ✅ Safer Approach

Prefer passing commands as a list:

```python
import subprocess

subprocess.run(
    ["ping", "-c", "1", "127.0.0.1"]
)
```

Instead of constructing shell commands from untrusted input.

---

# 🛡️ Cybersecurity Uses of Subprocess

`subprocess` can be used for:

* Automating security tools
* Running system diagnostics
* Collecting command output
* Automating network utilities
* Building security scripts
* Integrating existing command-line tools

Example architecture:

```text
Python Security Script
        │
        ▼
   subprocess
        │
        ▼
Command-line Tool
        │
        ▼
     Output
        │
        ▼
Python Analysis
        │
        ▼
 Security Report
```

---

# 🔥 Combining All Five Libraries

The real power comes from combining them.

For example, imagine a security automation tool:

```text
                 Security Automation Tool
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
      requests            socket          subprocess
          │                │                │
       APIs/Web        Network checks     CLI tools
          │                │                │
          └────────────────┼────────────────┘
                           ▼
                         JSON
                           │
                           ▼
                         os
                           │
                           ▼
                    Files / Environment
```

---

# 🧪 Mini Security Automation Example

The following example combines several concepts.

```python
import requests
import json
import socket

target = "example.com"

# DNS resolution
ip = socket.gethostbyname(target)

# HTTP request
response = requests.get(
    f"https://{target}",
    timeout=5
)

# Create security information
result = {
    "domain": target,
    "ip": ip,
    "status_code": response.status_code,
    "server": response.headers.get("Server")
}

# Convert to JSON
print(json.dumps(result, indent=4))
```

Possible output:

```json
{
    "domain": "example.com",
    "ip": "93.184.216.34",
    "status_code": 200,
    "server": "..."
}
```

This is the basic idea behind many security automation scripts:

```text
Collect
   ↓
Process
   ↓
Analyze
   ↓
Format
   ↓
Report
```

---

# 🧠 Library Comparison

| Library      | Main Job              | Standard Library? |
| ------------ | --------------------- | ----------------- |
| `requests`   | HTTP communication    | ❌ No              |
| `json`       | JSON processing       | ✅ Yes             |
| `os`         | OS interaction        | ✅ Yes             |
| `socket`     | Network communication | ✅ Yes             |
| `subprocess` | Run external programs | ✅ Yes             |

---

# ⚔️ Requests vs Socket

These two are especially important.

| Feature           | `requests` | `socket`              |
| ----------------- | ---------- | --------------------- |
| Main purpose      | HTTP/HTTPS | Network communication |
| Abstraction       | High-level | Low-level             |
| HTTP built-in     | ✅          | ❌                     |
| TCP communication | Indirectly | ✅                     |
| API interaction   | Excellent  | Manual                |
| Port checking     | Not ideal  | Excellent             |
| Ease of use       | Easy       | More complex          |

Think:

```text
requests → "Talk to websites"

socket → "Talk over networks"
```

---

# ⚔️ OS vs Subprocess

| Feature               | `os`    | `subprocess` |
| --------------------- | ------- | ------------ |
| OS interaction        | ✅       | Indirectly   |
| File operations       | ✅       | Possible     |
| Environment variables | ✅       | Possible     |
| Run external commands | Limited | ✅            |
| Security automation   | ✅       | ✅            |

Think:

```text
os          → Manage/interact with the OS

subprocess  → Run other programs
```

---

# ⚠️ Common Mistakes

## 1. Forgetting to install `requests`

```text
ModuleNotFoundError: No module named 'requests'
```

Fix:

```bash
pip install requests
```

---

## 2. No timeout

Bad:

```python
requests.get(url)
```

Better:

```python
requests.get(url, timeout=5)
```

---

## 3. Hardcoding secrets

Avoid:

```python
API_KEY = "secret123"
```

Prefer environment variables:

```python
import os

API_KEY = os.getenv("API_KEY")
```

---

## 4. Using `shell=True` carelessly

Avoid combining:

```python
shell=True
```

with untrusted input.

This can create **command injection** vulnerabilities.

---

## 5. Forgetting to close sockets

Instead of manually managing everything, use proper cleanup or context management where appropriate.

---

## 6. Ignoring exceptions

Network operations can fail.

```python
import requests

try:
    response = requests.get(
        "https://example.com",
        timeout=5
    )
except requests.RequestException as error:
    print("Request failed:", error)
```

---

# 🧠 Memory Trick

Remember:

> **R-J-O-S-S**

```text
R → Requests → Web/API
J → JSON → Data
O → OS → Operating System
S → Socket → Network
S → Subprocess → System Commands
```

Or:

> 🌐 **Requests the web, JSON handles data, OS handles the system, Socket handles networks, Subprocess runs programs.**

---

# 🔄 Quick Revision

### `requests`

```python
import requests

response = requests.get(
    "https://example.com",
    timeout=5
)
```

Used for:

> HTTP/HTTPS communication.

---

### `json`

```python
import json

data = json.loads(text)
```

Used for:

> JSON data processing.

---

### `os`

```python
import os

print(os.getcwd())
```

Used for:

> Operating-system interaction.

---

### `socket`

```python
import socket

ip = socket.gethostbyname("example.com")
```

Used for:

> Network communication.

---

### `subprocess`

```python
import subprocess

subprocess.run(["python", "--version"])
```

Used for:

> Running external programs and commands.

---

# 💼 Interview Tip

If an interviewer asks:

**"How can Python be used in cybersecurity?"**

A strong answer is:

> Python can automate cybersecurity tasks by communicating with web APIs using `requests`, processing security data using `json`, interacting with the operating system using `os`, performing network communication and port checks using `socket`, and automating command-line security tools using `subprocess`.

---

# 🧪 Mini Practice

## Task 1 — Website Checker

Create a program that:

1. Takes a URL from the user.
2. Sends an HTTP request.
3. Displays the status code.
4. Uses a timeout.
5. Handles request errors.

---

## Task 2 — JSON Security Report

Create:

```python
security_report = {
    "target": "example.com",
    "status": "reachable",
    "severity": "low"
}
```

Convert it into formatted JSON.

---

## Task 3 — File Inventory

Using `os`:

* List all files in a directory.
* Identify files.
* Display their names.

---

## Task 4 — DNS Resolver

Using `socket`:

```text
Enter domain:
example.com

IP:
...
```

---

## Task 5 — Port Checker

Create a program that checks these ports on **your own machine/lab**:

```text
22
80
443
8080
```

Display:

```text
22 → OPEN
80 → CLOSED
...
```

---

## Task 6 — Command Automation

Using `subprocess`, execute a safe command such as:

```bash
python --version
```

and display its output.

---

# 🚀 What You Should Remember

The goal isn't simply to memorize five libraries.

You should understand how they fit into **security automation**:

```text
                Python
                   │
       ┌───────────┼───────────┐
       │           │           │
       ▼           ▼           ▼
    Web/API     Network       System
       │           │           │
   requests      socket    os/subprocess
       │           │           │
       └───────────┼───────────┘
                   ▼
                  JSON
                   │
                   ▼
            Security Data
                   │
                   ▼
             Automation
```

These libraries form a foundation for building tools such as:

* 🌐 Website/API monitors
* 🔎 Network scanners
* 📊 Log analyzers
* 🛡️ Security automation scripts
* 📡 Network utilities
* 🤖 Threat-intelligence collectors
* ⚙️ Command-line tool automation

> **Learn the library → understand what it communicates with → automate a security task with it.**

---

# 📌 Summary

| Library      | Remember It As | Security Application    |
| ------------ | -------------- | ----------------------- |
| `requests`   | 🌐 Web         | APIs, HTTP analysis     |
| `json`       | 📦 Data        | Security data & APIs    |
| `os`         | 💻 System      | Files, environment      |
| `socket`     | 🔌 Network     | DNS, ports, connections |
| `subprocess` | ⚙️ Commands    | Tool automation         |

### Final Memory Line:

> **Requests talks to the Web → JSON handles the Data → OS talks to the System → Socket talks to the Network → Subprocess runs the Tools.**
