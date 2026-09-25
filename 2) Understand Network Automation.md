# 🌐 2. Understand Network Automation

> **Network automation means using scripts and software to automatically communicate with network systems, interact with services, perform checks, and collect useful information.**

Instead of manually checking every server, port, or service, a security professional can write a program that performs these repetitive tasks automatically.

---

# 🧠 1. What Is Network Automation?

Imagine you have **100 servers** and you want to check:

* Are they reachable?
* Which ports are open?
* Is SSH running?
* Is a web service responding?
* Are expected services available?

Doing this manually would take a lot of time.

With automation:

```text
        Network
           │
   ┌───────┴────────┐
   │                │
Server 1          Server 2
   │                │
   └───────┬────────┘
           │
      Python Script
           │
   ┌───────┼────────┐
   │       │        │
 Reachability  Ports  Services
    Check      Check    Check
```

The script performs the repetitive checks and produces a result.

### 🔐 In cybersecurity

Network automation can help with:

* Port checking
* Service availability checks
* Server monitoring
* Connectivity testing
* Security validation
* Network inventory
* Log collection
* Alert generation
* Basic reconnaissance in authorized environments

---

# 🔌 2. Socket Programming Fundamentals

## What is a Socket?

A **socket** is a programming interface that allows two systems or programs to communicate over a network.

Think of it like a communication endpoint.

```text
Program A
   │
 Socket
   │
 Network
   │
 Socket
   │
Program B
```

Python provides socket programming through the built-in:

```python
import socket
```

---

# 🌐 Client and Server

Most network communication involves a **client** and a **server**.

### Client

The client initiates communication.

Examples:

* Web browser
* SSH client
* Python script
* Email client

### Server

The server listens for incoming connections.

Examples:

* Web server
* SSH server
* DNS server
* Database server

Basic model:

```text
Client                         Server
  │                              │
  │────── Connection Request ───>│
  │                              │
  │<────── Response ─────────────│
  │                              │
  │────── More Data ────────────>│
  │                              │
```

---

# 📦 IP Address + Port

To communicate with a network service, you generally need:

```text
IP Address + Port
```

For example:

```text
192.168.1.10:22
```

means:

```text
IP Address → 192.168.1.10
Port       → 22
```

Port `22` commonly corresponds to SSH.

Another example:

```text
192.168.1.10:80
```

Port `80` commonly corresponds to HTTP.

---

# 🚪 What Is a Port?

A port identifies a communication endpoint associated with a network service.

Think of an IP address as a **building address** and a port as a **specific door**.

```text
                 Server
            192.168.1.10
                  │
      ┌───────────┼───────────┐
      │           │           │
    :22         :80         :443
     │            │           │
    SSH          HTTP        HTTPS
```

This is why cybersecurity professionals are interested in ports.

An open port can indicate that a service is available.

---

# 🐍 Basic Socket Example

```python
import socket

s = socket.socket()

s.connect(("example.com", 80))

print("Connected!")

s.close()
```

### What happens?

```text
socket.socket()
      ↓
Create socket
      ↓
connect()
      ↓
Connect to server
      ↓
Communication
      ↓
close()
      ↓
End connection
```

---

# ⏱️ Using a Timeout

Network programs should normally use timeouts.

```python
import socket

s = socket.socket()

s.settimeout(3)

try:
    s.connect(("example.com", 80))
    print("Port is reachable")
except socket.timeout:
    print("Connection timed out")
finally:
    s.close()
```

### Why timeout matters?

Without a timeout, a network operation might wait longer than expected.

A timeout makes your automation more predictable.

---

# 🔍 3. Network Communication

Network communication involves exchanging data between systems.

A simplified process:

```text
Application
    ↓
Transport
    ↓
Network
    ↓
Network Interface
    ↓
Internet / LAN
    ↓
Destination
```

At the programming level, sockets allow applications to communicate through protocols such as:

* TCP
* UDP

---

# 🛡️ TCP

**TCP = Transmission Control Protocol**

TCP provides connection-oriented communication.

Simplified:

```text
Client                     Server

  │──── SYN ───────────────>│
  │<─── SYN + ACK ─────────│
  │──── ACK ───────────────>│
  │                         │
  │<──── Data ─────────────>│
```

TCP is commonly used by:

* HTTP
* HTTPS
* SSH
* FTP
* SMTP

---

# ⚡ UDP

**UDP = User Datagram Protocol**

UDP is connectionless.

Instead of establishing a connection like TCP, data can be sent directly.

```text
Client
  │
  │──── Datagram ──────────> Server
  │
  │──── Datagram ──────────> Server
```

UDP is commonly used by:

* DNS
* DHCP
* Streaming
* Some online games
* Voice/video applications

---

# 🐍 TCP Socket

Python TCP socket:

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect(("example.com", 80))

print("Connected")

s.close()
```

### Important arguments

```python
socket.AF_INET
```

means IPv4.

```python
socket.SOCK_STREAM
```

means TCP.

Therefore:

```python
socket.socket(
    socket.AF_INET,
    socket.SOCK_STREAM
)
```

means:

> Create an IPv4 TCP socket.

---

# 📡 Sending Data

Sockets can also send data.

Example:

```python
import socket

s = socket.socket()
s.settimeout(5)

s.connect(("example.com", 80))

request = b"GET / HTTP/1.1\r\nHost: example.com\r\n\r\n"

s.send(request)

response = s.recv(4096)

print(response.decode(errors="ignore"))

s.close()
```

Here:

```python
send()
```

sends data.

And:

```python
recv()
```

receives data.

---

# 🔄 Basic Communication Model

```text
socket()
   ↓
connect()
   ↓
send()
   ↓
recv()
   ↓
close()
```

Remember:

> **Create → Connect → Send → Receive → Close**

---

# 🧩 4. Service Interaction

Network automation isn't only about connecting to a port.

A port usually exists because some **service** is listening there.

For example:

| Port | Common Service |
| ---: | -------------- |
|   21 | FTP            |
|   22 | SSH            |
|   25 | SMTP           |
|   53 | DNS            |
|   80 | HTTP           |
|  443 | HTTPS          |
| 3306 | MySQL          |
| 5432 | PostgreSQL     |

These are **common associations**, not guarantees. A service can be configured to use a different port.

---

# 🔎 Port vs Service

This distinction is important.

### Port

A communication endpoint.

### Service

The application actually providing functionality.

Example:

```text
192.168.1.10:22
       │
       ↓
     SSH
       │
       ↓
SSH Server
```

A port being open doesn't automatically prove which application is behind it.

---

# 🌐 HTTP Service Interaction

Suppose a web server is listening on port `80`.

A Python program can communicate with it.

```python
import socket

s = socket.socket()
s.settimeout(5)

s.connect(("example.com", 80))

request = b"GET / HTTP/1.1\r\nHost: example.com\r\nConnection: close\r\n\r\n"

s.sendall(request)

while True:
    data = s.recv(4096)

    if not data:
        break

    print(data.decode(errors="ignore"))

s.close()
```

The important concept is:

```text
Connect to service
       ↓
Speak the expected protocol
       ↓
Receive response
       ↓
Analyze response
```

---

# 🧠 Why Protocol Knowledge Matters

Different services expect different communication formats.

For example:

```text
HTTP
 ↓
HTTP requests/responses

DNS
 ↓
DNS messages

SSH
 ↓
SSH protocol

SMTP
 ↓
Email protocol
```

You cannot simply send random data and expect every service to respond meaningfully.

This is why networking knowledge is important for security automation.

---

# 🤖 5. Automated Checks

One of the most useful applications of network automation is performing **automated checks**.

Instead of manually checking:

```text
Is server reachable?
Is port 22 open?
Is port 80 open?
Is HTTPS working?
```

you can automate it.

---

# 🟢 Simple Port Checker

```python
import socket

target = "127.0.0.1"
port = 22

s = socket.socket()
s.settimeout(2)

try:
    s.connect((target, port))
    print(f"Port {port} is open")
except (socket.timeout, ConnectionRefusedError):
    print(f"Port {port} is closed or unreachable")
finally:
    s.close()
```

### Flow

```text
Target
  ↓
Create socket
  ↓
Set timeout
  ↓
Attempt connection
  ↓
 ┌───────────────┐
 │               │
Success        Failure
 │               │
 ↓               ↓
Open        Closed/Unreachable
```

---

# 🔢 Checking Multiple Ports

You can automate multiple checks.

```python
import socket

target = "127.0.0.1"

ports = [22, 80, 443, 3306]

for port in ports:

    s = socket.socket()
    s.settimeout(1)

    try:
        s.connect((target, port))
        print(f"{port}: OPEN")
    except (socket.timeout, ConnectionRefusedError):
        print(f"{port}: CLOSED/FILTERED")
    finally:
        s.close()
```

This is a basic form of **network scanning**.

---

# ⚠️ Important Result Interpretation

A failed connection does **not always mean**:

> "The port definitely does not exist."

Possible reasons include:

```text
Connection failed
       │
       ├── Port closed
       ├── Firewall filtering
       ├── Host unreachable
       ├── Service unavailable
       └── Timeout
```

Professional security tools distinguish between different states more carefully.

This simple Python script is primarily a **learning tool**, not a replacement for mature scanners.

---

# 🔍 6. Basic Scanning Concepts

## What Is Network Scanning?

Network scanning means systematically checking network targets to discover information such as:

* Hosts
* Open ports
* Available services
* Network exposure
* Reachability

A simplified process:

```text
Target
  ↓
Identify host
  ↓
Check ports
  ↓
Identify services
  ↓
Collect information
  ↓
Analyze results
```

---

# 🎯 Host Discovery

First you may want to determine whether a system is reachable.

Example concept:

```text
Network
   │
   ├── 192.168.1.1   → Online
   ├── 192.168.1.2   → Online
   ├── 192.168.1.3   → Offline
   └── 192.168.1.4   → Online
```

This is called **host discovery**.

---

# 🚪 Port Scanning

After identifying a host:

```text
192.168.1.10

22    → Open
80    → Open
443   → Open
3306  → Closed
```

This is port scanning.

---

# 🧩 Service Discovery

The next step can be identifying what service is associated with an open port.

Example:

```text
Port 22
   ↓
SSH

Port 80
   ↓
HTTP

Port 443
   ↓
HTTPS
```

More advanced tools can attempt **service/version detection**.

---

# 🛠️ Python Port Scanner

A simple educational scanner:

```python
import socket

target = "127.0.0.1"

ports = [22, 80, 443, 8080]

for port in ports:

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(0.5)

    result = s.connect_ex((target, port))

    if result == 0:
        print(f"[+] Port {port} is OPEN")
    else:
        print(f"[-] Port {port} is CLOSED/FILTERED")

    s.close()
```

### Why use `connect_ex()`?

`connect()` raises an exception when it fails.

`connect_ex()` returns an error code.

```python
result = s.connect_ex(...)
```

Typically:

```text
0
↓
Connection successful
```

Non-zero:

```text
Connection failed
```

---

# ⚡ Improving the Scanner

A basic scanner can be improved with:

* Timeouts
* Exception handling
* User input
* Port ranges
* Result storage
* Logging
* Threading
* Service detection
* Reporting

For example:

```text
Target
  ↓
Port range
  ↓
Connection attempts
  ↓
Results
  ↓
Save to file
  ↓
Security report
```

You will eventually be able to turn this into a small cybersecurity utility.

---

# 🆚 Python Socket Scanner vs Nmap

You have already encountered Nmap in your cybersecurity learning.

The difference is important.

| Feature                  | Python Socket Script | Nmap      |
| ------------------------ | -------------------- | --------- |
| Basic port checking      | ✅                    | ✅         |
| Host discovery           | Basic/manual         | ✅         |
| Service detection        | Limited              | ✅         |
| OS detection             | ❌                    | ✅         |
| Advanced scan techniques | ❌                    | ✅         |
| Scripting capabilities   | You build them       | Extensive |
| Learning value           | ⭐⭐⭐⭐⭐                | ⭐⭐⭐⭐⭐     |
| Professional scanning    | Limited              | ✅         |

Python helps you understand **how automation works**.

Nmap is a mature security tool designed specifically for network discovery and auditing.

---

# 🔐 7. Security Automation Example

Suppose your organization has a server that should expose only:

```text
22 → SSH
443 → HTTPS
```

You could automate a security check.

```text
             Server
                │
        ┌───────┴────────┐
        │                │
       22               443
       ✓                 ✓
      SSH              HTTPS

Expected ports:
22, 443

Unexpected port:
3306
      ↓
⚠️ Alert
```

Python could periodically check the expected ports and report unexpected exposure.

---

# 📋 Example Security Check

```python
import socket

target = "127.0.0.1"

expected_ports = {
    22: "SSH",
    443: "HTTPS"
}

for port, service in expected_ports.items():

    s = socket.socket()
    s.settimeout(1)

    result = s.connect_ex((target, port))

    if result == 0:
        print(f"[OK] {service} ({port}) is reachable")
    else:
        print(f"[WARNING] {service} ({port}) is not reachable")

    s.close()
```

This is an example of **automated security validation**.

---

# 🧠 8. Network Automation Workflow

A practical security automation workflow might look like this:

```text
             Target
                │
                ▼
        ┌───────────────┐
        │ Reachability  │
        │     Check     │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │ Port Checking │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │    Service    │
        │   Interaction │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │ Analyze Result│
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │ Log / Alert   │
        └───────────────┘
```

This connects directly with the **Security Automation Fundamentals** topic from the previous section.

---

# 🛡️ 9. Network Automation in SOC

Network automation can help a SOC perform repetitive tasks.

### Example

A SOC receives an alert:

```text
Suspicious connection detected
        │
        ▼
Extract IP
        │
        ▼
Check connectivity
        │
        ▼
Check relevant service
        │
        ▼
Collect information
        │
        ▼
Create investigation data
        │
        ▼
Analyst reviews
```

Automation performs the repetitive technical work.

The analyst can then focus on interpretation and response.

---

# ⚙️ 10. Important Network Automation Concepts

| Concept           | Meaning                                       |
| ----------------- | --------------------------------------------- |
| Socket            | Communication endpoint                        |
| IP address        | Identifies a network interface/host location  |
| Port              | Identifies a transport-layer endpoint         |
| TCP               | Connection-oriented transport protocol        |
| UDP               | Connectionless transport protocol             |
| Client            | Initiates communication                       |
| Server            | Provides a service                            |
| Service           | Application/network functionality             |
| Port check        | Tests whether a connection can be established |
| Port scan         | Checks multiple ports                         |
| Host discovery    | Finds reachable hosts                         |
| Service discovery | Identifies services                           |
| Automation        | Performs repetitive tasks programmatically    |

---

# ⚠️ 11. Security Considerations

Network automation can become dangerous if used carelessly.

### Only scan systems you:

```text
OWN
  OR
HAVE EXPLICIT AUTHORIZATION TO TEST
```

For learning, use:

```text
127.0.0.1
```

your own machine, or your authorized cybersecurity lab.

### Why?

Large or aggressive scans can:

* Generate significant traffic
* Trigger IDS/IPS alerts
* Affect fragile services
* Violate organizational policies
* Be considered unauthorized reconnaissance

Start with small, controlled checks.

---

# 🧪 12. Your Practice Tasks

## 🟢 Task 1 — Socket Connection

Create a Python script that:

1. Creates a TCP socket.
2. Sets a timeout.
3. Connects to:

```text
127.0.0.1
```

on port:

```text
80
```

4. Prints whether the connection succeeded.
5. Closes the socket.

---

## 🟢 Task 2 — Multiple Port Checker

Create a program that checks:

```text
22
80
443
8080
```

on:

```text
127.0.0.1
```

Expected style:

```text
22   → OPEN
80   → CLOSED
443  → OPEN
8080 → CLOSED
```

Your actual results will depend on which services are running on your machine.

---

## 🟡 Task 3 — Port Range

Modify your script to check:

```text
20 → 100
```

instead of manually entering individual ports.

Concept:

```python
for port in range(20, 101):
    # check port
```

---

## 🟡 Task 4 — Save Results

Modify the scanner so that results are saved to:

```text
scan_results.txt
```

Example:

```text
Target: 127.0.0.1

Port 22: OPEN
Port 80: CLOSED
Port 443: OPEN
```

---

## 🔴 Task 5 — Security Validation

Create a script where you define expected services:

```python
expected_ports = {
    22: "SSH",
    443: "HTTPS"
}
```

The program should report:

```text
[OK] SSH is reachable
[OK] HTTPS is reachable
```

or:

```text
[WARNING] SSH is not reachable
```

This starts turning a simple scanner into a **security automation tool**.

---

# 🧠 13. Memory Tricks

### Socket communication

> **C → S → R → C**

**Connect → Send → Receive → Close**

---

### Network automation

> **H → P → S → A → R**

**Host → Port → Service → Analyze → Report**

---

### Basic scanning

```text
Host Discovery
      ↓
Port Discovery
      ↓
Service Discovery
      ↓
Analysis
```

---

# 🎯 14. Interview Questions

### What is socket programming?

Socket programming is a way for programs to communicate over a network using communication endpoints called sockets.

### What is a port?

A port is a transport-layer communication endpoint used to identify a particular network service or application endpoint.

### TCP vs UDP?

**TCP** is connection-oriented and provides reliable, ordered delivery.

**UDP** is connectionless and does not provide TCP's built-in reliability and ordering guarantees.

### What is port scanning?

Port scanning is the process of checking network ports on a target to determine which ports are reachable or exposed.

### What is service discovery?

Service discovery attempts to determine what network service or application is associated with an accessible port.

### Why automate network checks?

Automation reduces repetitive manual work, improves consistency, and allows checks to be performed regularly and at scale.

### Is an open port automatically a vulnerability?

**No.**

An open port means that a network endpoint is reachable. Whether it represents a security issue depends on the service, configuration, exposure, authentication, vulnerabilities, and organizational requirements.

---

# ⚡ Quick Revision

```text
NETWORK AUTOMATION
│
├── 🔌 Socket Programming
│   ├── Socket
│   ├── Client
│   ├── Server
│   ├── TCP
│   └── UDP
│
├── 🌐 Network Communication
│   ├── IP
│   ├── Port
│   ├── Send
│   └── Receive
│
├── ⚙️ Service Interaction
│   ├── Connect
│   ├── Protocol
│   └── Response
│
├── 🤖 Automated Checks
│   ├── Reachability
│   ├── Port checks
│   ├── Service checks
│   └── Security validation
│
└── 🔍 Basic Scanning
    ├── Host discovery
    ├── Port scanning
    ├── Service discovery
    └── Result analysis
```

---

# 🧩 Final Takeaway

Network automation combines **programming + networking + security**.

The fundamental idea is:

```text
Network Knowledge
       +
Python
       +
Sockets
       ↓
Automated Network Checks
       ↓
Security Automation
```

A simple socket program can evolve from:

```text
Check one port
```

into:

```text
Check many ports
      ↓
Identify services
      ↓
Record results
      ↓
Compare against expected state
      ↓
Generate alerts
      ↓
Support security operations
```

> 🛡️ **The goal of network automation is not simply to scan faster. It is to turn repetitive network and security checks into reliable, controlled, repeatable processes.**

This gives you the foundation you’ll need before moving into more advanced **network monitoring, automated reconnaissance, and security workflows**.
