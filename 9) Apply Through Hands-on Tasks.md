# 🐧 Apply Through Hands-on Tasks

> **"The goal of Linux security is not just to manage a system, but to understand what is happening on the system, detect abnormal activity, and respond safely."**

---

# 📖 What is Linux Security Operations?

Linux security operations involve administering, monitoring, analyzing, and securing Linux systems.

A cybersecurity professional should be able to:

* 👤 Manage users and permissions
* ⚙️ Manage services and processes
* 📦 Maintain software and updates
* 🌐 Monitor network configuration
* 📝 Analyze security logs
* 🐚 Write shell scripts
* ⚡ Automate repetitive tasks
* 🔍 Monitor suspicious activity
* 🚨 Investigate security events

The overall process can be represented as:

```text
              Linux System
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
   Administration Logs     Processes
        │          │          │
        └──────────┼──────────┘
                   ▼
              Analysis
                   │
                   ▼
             Automation
                   │
                   ▼
          Security Monitoring
                   │
                   ▼
             Detection
```

---

# 👨‍💻 A. Administer Linux Systems

## 📖 What is Linux System Administration?

Linux system administration is the process of configuring, maintaining, monitoring, and securing Linux systems.

In cybersecurity, Linux administration is important because many:

* Web servers
* Database servers
* Cloud servers
* Network appliances
* Security tools
* Containers
* SOC systems

use Linux.

A security professional therefore needs to understand how a normal Linux system operates before trying to identify abnormal behavior.

---

# 👤 1️⃣ User Management

Linux is a multi-user operating system.

Each user can have:

* Username
* User ID (UID)
* Primary group
* Secondary groups
* Home directory
* Login shell
* Permissions

Check the current user:

```bash
whoami
```

Get detailed information:

```bash
id
```

Example:

```text
uid=1000(farhan) gid=1000(farhan) groups=1000(farhan),27(sudo)
```

This tells you:

```text
Username
   ↓
UID
   ↓
Primary Group
   ↓
Additional Groups
```

---

## 🔎 List Users

Linux stores local user account information in:

```bash
/etc/passwd
```

View it:

```bash
cat /etc/passwd
```

A safer way to inspect usernames:

```bash
cut -d: -f1 /etc/passwd
```

Example:

```text
root
daemon
bin
sys
farhan
```

---

## ➕ Create a User

```bash
sudo adduser analyst
```

Check the new account:

```bash
id analyst
```

---

## 👥 Groups

View groups:

```bash
groups
```

View all groups:

```bash
cat /etc/group
```

Add a user to a group:

```bash
sudo usermod -aG groupname username
```

For example, adding a user to the sudo group:

```bash
sudo usermod -aG sudo username
```

### ⚠️ Security Consideration

Membership in privileged groups can give users significant control over the system.

Therefore:

> **Do not give administrative privileges unless they are actually required.**

This follows the **Principle of Least Privilege**.

---

# 🔐 2️⃣ Linux File Permissions

Linux controls access to files using permissions.

Check permissions:

```bash
ls -l
```

Example:

```text
-rwxr-xr-- 1 farhan farhan 1200 script.sh
```

Breakdown:

```text
- rwx r-x r--
  │   │   │
  │   │   └── Others
  │   └────── Group
  └────────── Owner
```

Permissions:

| Permission | Symbol | Meaning         |
| ---------- | ------ | --------------- |
| Read       | `r`    | Read contents   |
| Write      | `w`    | Modify contents |
| Execute    | `x`    | Execute file    |

---

## 🔧 chmod

Change permissions:

```bash
chmod 755 script.sh
```

This means:

```text
Owner  → rwx
Group  → r-x
Others → r-x
```

Restrict a sensitive file:

```bash
chmod 600 secret.txt
```

Meaning:

```text
Owner  → rw-
Group  → ---
Others → ---
```

---

## 👑 File Ownership

Check ownership:

```bash
ls -l file.txt
```

Change ownership:

```bash
sudo chown user:user file.txt
```

### 🛡️ Security Importance

Incorrect permissions can expose:

* Passwords
* SSH keys
* Configuration files
* Application secrets
* Logs
* Backups

A security administrator should regularly check for unnecessarily broad permissions.

---

# ⚙️ 3️⃣ Process Management

A **process** is a running instance of a program.

List processes:

```bash
ps aux
```

Interactive monitoring:

```bash
top
```

If installed:

```bash
htop
```

Find a specific process:

```bash
ps aux | grep ssh
```

Find the PID:

```bash
pgrep ssh
```

Terminate a process:

```bash
kill PID
```

### 🚨 Security Relevance

Unexpected processes can indicate:

```text
Unknown Process
      ↓
Investigate
      ↓
Who started it?
      ↓
What file launched it?
      ↓
What network connections does it have?
      ↓
Is it legitimate?
```

Never assume that an unknown process is malware simply because its name looks suspicious.

---

# ⚙️ 4️⃣ Service Management

Linux services provide background functionality.

Check a service:

```bash
systemctl status ssh
```

Start:

```bash
sudo systemctl start ssh
```

Stop:

```bash
sudo systemctl stop ssh
```

Restart:

```bash
sudo systemctl restart ssh
```

Enable at boot:

```bash
sudo systemctl enable ssh
```

Disable at boot:

```bash
sudo systemctl disable ssh
```

---

## 🔍 Why Services Matter in Security

Every running service can potentially expose functionality to users or networks.

```text
Running Service
      ↓
Listening Port
      ↓
Potential Attack Surface
```

Therefore:

> **Only run services that are necessary.**

---

# 🌐 5️⃣ Network Administration

View network interfaces:

```bash
ip addr
```

View routing information:

```bash
ip route
```

Test connectivity:

```bash
ping 8.8.8.8
```

Test DNS:

```bash
dig example.com
```

View listening ports:

```bash
ss -tuln
```

Example:

```text
LISTEN
0.0.0.0:22
0.0.0.0:80
```

This tells you that services are listening for connections on those ports.

### 🛡️ Security Question

Whenever you find an open port, ask:

```text
Which service?
      ↓
Why is it running?
      ↓
Who can access it?
      ↓
Is it required?
      ↓
Is it patched?
```

---

# 📦 6️⃣ Package Management

Update package information:

```bash
sudo apt update
```

Upgrade installed packages:

```bash
sudo apt upgrade
```

Install software:

```bash
sudo apt install package-name
```

Remove software:

```bash
sudo apt remove package-name
```

Search:

```bash
apt search package-name
```

### 🔐 Security Importance

Software updates frequently contain security fixes.

An outdated application may contain known vulnerabilities such as:

* Remote Code Execution
* Privilege Escalation
* Authentication Bypass
* Information Disclosure

Therefore:

```text
Vulnerability
      ↓
Security Update
      ↓
Patch
      ↓
Reduced Risk
```

---

# 📝 B. Analyze Security Logs

## 📖 What Are Logs?

Logs are records generated by operating systems, applications, and services.

They provide evidence about what happened on a system.

Logs can contain:

* Login attempts
* Authentication events
* Service activity
* Errors
* Network events
* System changes
* Application activity

For cybersecurity professionals:

> **Logs are one of the most important sources of evidence during an investigation.**

---

# 📂 Linux Log Locations

Many Linux systems store logs under:

```bash
/var/log/
```

View the directory:

```bash
ls -lah /var/log/
```

On Debian-based systems, you may encounter:

```text
auth.log
syslog
kern.log
dpkg.log
```

The exact logs available depend on the Linux distribution and logging configuration.

---

# 🔐 Authentication Logs

On Debian/Kali systems, authentication information may be available in:

```bash
/var/log/auth.log
```

View it:

```bash
sudo less /var/log/auth.log
```

Find failed password attempts:

```bash
sudo grep "Failed password" /var/log/auth.log
```

Find successful SSH authentication:

```bash
sudo grep "Accepted" /var/log/auth.log
```

---

# 🔎 Example Investigation

Suppose you discover:

```text
Failed password for invalid user admin from 192.168.1.50
Failed password for invalid user admin from 192.168.1.50
Failed password for invalid user admin from 192.168.1.50
Failed password for invalid user test from 192.168.1.50
```

A possible pattern is:

```text
Multiple failed attempts
          ↓
Same source IP
          ↓
Multiple usernames
          ↓
Potential credential guessing
```

But this is **not automatically proof of an attack**.

Investigate further:

```text
Source IP
    ↓
Number of attempts
    ↓
Time period
    ↓
Target accounts
    ↓
Successful login?
    ↓
Activity after login?
```

Evidence determines whether the activity is actually malicious.

---

# 🕐 journalctl

Systems using `systemd` commonly provide the `journalctl` command.

View logs:

```bash
sudo journalctl
```

Show the latest entries:

```bash
sudo journalctl -n 50
```

Follow logs in real time:

```bash
sudo journalctl -f
```

View logs for SSH:

```bash
sudo journalctl -u ssh
```

View today's logs:

```bash
sudo journalctl --since today
```

---

# 🔧 Useful Log Analysis Commands

### grep

Search for specific text:

```bash
grep "Failed" logfile
```

### tail

View the end of a log:

```bash
tail logfile
```

### tail -f

Monitor a log continuously:

```bash
tail -f logfile
```

### sort

Sort results:

```bash
sort results.txt
```

### uniq

Remove repeated adjacent entries:

```bash
sort results.txt | uniq
```

Count occurrences:

```bash
sort results.txt | uniq -c
```

---

# 🧠 Log Investigation Workflow

```text
Collect Logs
     ↓
Filter Relevant Events
     ↓
Identify Users
     ↓
Identify IP Addresses
     ↓
Build Timeline
     ↓
Look for Patterns
     ↓
Investigate Context
     ↓
Determine Possible Cause
     ↓
Document Findings
```

---

# 🐚 C. Build Shell Scripts

## 📖 What is Shell Scripting?

Shell scripting means combining Linux commands into a script that can perform tasks automatically.

Instead of repeatedly typing commands:

```text
Command
   ↓
Command
   ↓
Command
   ↓
Command
```

we can create:

```text
security_check.sh
```

and run the entire workflow automatically.

---

# 📝 Basic Bash Script

Create a file:

```bash
nano hello.sh
```

Add:

```bash
#!/bin/bash

echo "Linux Security Lab"
echo "Current user:"
whoami
```

Make it executable:

```bash
chmod +x hello.sh
```

Run:

```bash
./hello.sh
```

---

# 📦 Variables

```bash
#!/bin/bash

name="Farhan"

echo "Hello $name"
```

Variables allow scripts to store and reuse information.

---

# ⌨️ User Input

```bash
#!/bin/bash

read -p "Enter your username: " username

echo "Hello $username"
```

---

# 🔀 Conditional Statements

```bash
#!/bin/bash

if [ -f "/etc/passwd" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

Conditions allow a script to make decisions.

---

# 🔁 Loops

Example:

```bash
#!/bin/bash

for file in /var/log/*; do
    echo "$file"
done
```

Loops are useful when performing the same operation against multiple files or values.

---

# 🔗 Pipes

Linux commands can be combined using pipes.

Example:

```bash
ps aux | grep ssh
```

The output from `ps aux` becomes input for `grep`.

Conceptually:

```text
ps aux
  ↓
All Processes
  ↓
grep ssh
  ↓
SSH-related Processes
```

This ability to combine small commands is one of the most powerful features of Linux.

---

# 🛡️ Shell Scripting in Cybersecurity

Scripts can automate:

```text
Log Analysis
System Auditing
User Auditing
Port Monitoring
Process Monitoring
File Integrity Checks
Security Reports
```

---

# ⚠️ Secure Shell Scripting

AI-generated or manually written scripts can cause serious damage if poorly designed.

Before running a script, ask:

```text
What does it do?
      ↓
What files does it modify?
      ↓
Does it delete anything?
      ↓
Does it require root?
      ↓
Does it connect to the network?
      ↓
Does it accept user input?
      ↓
Can the input be abused?
```

Never blindly execute an unknown script with:

```bash
sudo ./script.sh
```

---

# ⚙️ D. Automate Administrative Tasks

## 📖 What is Automation?

Automation means allowing a script or scheduled system process to perform repetitive administrative tasks automatically.

For example:

```text
Manual
  ↓
Check Disk
  ↓
Check Memory
  ↓
Check Processes
  ↓
Check Services
```

can become:

```text
system_audit.sh
       ↓
Everything Checked Automatically
```

---

# 🖥️ Example: System Health Script

```bash
#!/bin/bash

echo "===== SYSTEM HEALTH ====="

echo
echo "[Hostname]"
hostname

echo
echo "[Uptime]"
uptime

echo
echo "[Disk Usage]"
df -h

echo
echo "[Memory]"
free -h

echo
echo "[IP Address]"
ip addr

echo
echo "[Listening Ports]"
ss -tuln
```

This creates a basic system information report.

---

# ⏰ Scheduled Automation with Cron

Linux can execute tasks automatically using cron.

View current cron jobs:

```bash
crontab -l
```

Edit them:

```bash
crontab -e
```

Example:

```text
0 * * * * /home/user/system_audit.sh
```

This means:

```text
Every hour
     ↓
Run system_audit.sh
```

---

# 🔐 Automation Security

Automation can create security problems if configured incorrectly.

Important considerations:

* Script permissions
* File ownership
* Secure paths
* Input validation
* Error handling
* Logging
* Least privilege
* Protecting credentials

A script running with root privileges can make system-wide changes.

Therefore:

> **Use the minimum privileges required for the task.**

---

# 🔍 E. Create Security Monitoring Scripts

## 📖 What is Security Monitoring?

Security monitoring means continuously observing a system for abnormal or suspicious activity.

A basic Linux security monitoring script can check:

```text
Users
  ↓
Processes
  ↓
Services
  ↓
Listening Ports
  ↓
Authentication Logs
  ↓
File Changes
```

---

# 🔐 1️⃣ Failed Login Monitor

Create:

```bash
nano failed_logins.sh
```

Example:

```bash
#!/bin/bash

LOG="/var/log/auth.log"

echo "===== FAILED LOGIN ATTEMPTS ====="

grep "Failed password" "$LOG" 2>/dev/null | tail -20
```

Make it executable:

```bash
chmod +x failed_logins.sh
```

Run:

```bash
sudo ./failed_logins.sh
```

This gives you a basic view of recent failed authentication attempts.

---

# 🌐 2️⃣ Listening Port Monitor

```bash
#!/bin/bash

echo "===== LISTENING PORTS ====="

ss -tuln
```

This helps identify services that are listening for network connections.

---

# ⚙️ 3️⃣ Service Monitor

You can check whether a service is active:

```bash
systemctl is-active ssh
```

A script can use this information:

```bash
#!/bin/bash

SERVICE="ssh"

if systemctl is-active --quiet "$SERVICE"; then
    echo "$SERVICE is running"
else
    echo "WARNING: $SERVICE is not running"
fi
```

---

# 📊 4️⃣ Failed Login Counter

```bash
#!/bin/bash

LOG="/var/log/auth.log"

COUNT=$(grep -c "Failed password" "$LOG" 2>/dev/null)

echo "Failed login attempts: $COUNT"
```

This demonstrates how a script can transform raw logs into useful security information.

---

# 🧬 5️⃣ File Integrity Monitoring

File integrity monitoring detects unexpected changes to important files.

The basic concept is:

```text
Important File
      ↓
Calculate Hash
      ↓
Save Baseline
      ↓
Time Passes
      ↓
Calculate Hash Again
      ↓
Compare
      ↓
Different?
   /       \
 Yes       No
 ↓          ↓
Alert      Normal
```

Use SHA-256:

```bash
sha256sum important_file
```

Example:

```bash
sha256sum /etc/hosts
```

If the file changes, its hash may change.

### Important

A changed hash does **not automatically mean an attack occurred**.

Legitimate system updates can also modify files.

---

# 🛡️ Building a Linux Security Monitor

Now combine the individual checks.

```text
             Linux Security Monitor
                       │
       ┌───────────────┼───────────────┐
       ▼               ▼               ▼
 System Info      Authentication    Network
       │               │               │
       ▼               ▼               ▼
 Processes          Logins          Ports
 Services           Errors         Connections
 Disk               Users
       │               │               │
       └───────────────┼───────────────┘
                       ▼
                   Analysis
                       │
                       ▼
                    Alert
                       │
                       ▼
                   Report
```

---

# 🧪 Hands-on Project

Create a directory:

```bash
mkdir linux-security-monitor
cd linux-security-monitor
```

Create:

```text
main.sh
system_info.sh
login_monitor.sh
port_monitor.sh
service_monitor.sh
file_integrity.sh
```

Your final project could look like:

```text
linux-security-monitor/
│
├── main.sh
├── system_info.sh
├── login_monitor.sh
├── port_monitor.sh
├── service_monitor.sh
├── file_integrity.sh
│
├── logs/
│
└── README.md
```

---

# 🖥️ Create a Menu

Your `main.sh` could eventually provide:

```text
====================================
       LINUX SECURITY MONITOR
====================================

1. System Information
2. Login Monitoring
3. Port Monitoring
4. Service Monitoring
5. File Integrity Check
6. Full Security Audit
7. Exit

Choose an option:
```

This turns individual scripts into a small Linux security tool.

---

# 🔄 Complete Security Workflow

The overall learning process should look like:

```text
Administer Linux
       ↓
Understand Normal Behavior
       ↓
Collect Logs
       ↓
Analyze Logs
       ↓
Write Shell Scripts
       ↓
Automate Repetitive Tasks
       ↓
Monitor Security Events
       ↓
Detect Abnormal Activity
       ↓
Investigate
       ↓
Document
```

This is an important transition from **Linux administration to cybersecurity operations**.

---

# 🧠 Real-World Example

Imagine a Linux web server suddenly receives hundreds of failed SSH login attempts.

A security analyst might investigate:

```text
Authentication Logs
        ↓
Failed SSH Attempts
        ↓
Source IP Analysis
        ↓
Number of Attempts
        ↓
Time Pattern
        ↓
Successful Login?
        ↓
Activity After Login
        ↓
Determine Risk
        ↓
Take Appropriate Action
```

A monitoring script could detect the unusual number of failures and generate an alert.

However:

> **Detection is not the same as confirmation.**

A script can identify something unusual, but a human analyst must investigate the evidence and determine what actually happened.

---

# 🛡️ Common Mistakes

### ❌ Running everything as root

```bash
sudo ./script.sh
```

Not every task requires root privileges.

---

### ❌ Trusting scripts blindly

A script can:

* Delete files
* Modify permissions
* Stop services
* Change configurations
* Send information over a network

Always understand the script first.

---

### ❌ Treating every anomaly as an attack

```text
Unusual Activity
      ↓
Investigate
```

Not:

```text
Unusual Activity
      ↓
Definitely Attack
```

---

### ❌ Ignoring logs

Logs are essential evidence during security investigations.

---

### ❌ Hardcoding sensitive credentials

Never place passwords, API keys, or other secrets directly inside scripts.

---

### ❌ Creating insecure automation

A scheduled script with excessive privileges can itself become a security risk.

---

# 🧪 Practical Exercises

## Exercise 1 — Linux Administration

Perform the following on your Kali lab:

```bash
whoami
id
ip addr
ip route
ps aux
ss -tuln
systemctl --type=service
df -h
free -h
```

Understand what every command tells you.

---

## Exercise 2 — Log Analysis

Find:

```text
Failed authentication attempts
Successful authentication attempts
Source IP addresses
Usernames
Timestamps
```

Then answer:

```text
What happened?
When did it happen?
Which account was targeted?
Where did the connection originate?
Was authentication successful?
Is the behavior suspicious?
What additional evidence would you investigate?
```

---

## Exercise 3 — Shell Scripting

Create a script that displays:

```text
Hostname
Current User
Kernel Version
IP Address
Disk Usage
Memory Usage
Running Processes
Listening Ports
```

---

## Exercise 4 — Automation

Modify the script so that it automatically saves its output to:

```text
security_report.txt
```

Then schedule it with cron.

---

## Exercise 5 — Security Monitoring

Create a script that checks:

```text
Failed logins
Listening ports
Important services
Disk usage
Running processes
```

and displays warnings when something requires attention.

---

# 🎯 Final Project

Build:

## `Linux Security Monitor`

The project should eventually be capable of:

```text
┌───────────────────────────────────┐
│       LINUX SECURITY MONITOR      │
├───────────────────────────────────┤
│                                   │
│  System Information               │
│  User Audit                       │
│  Process Monitoring               │
│  Service Monitoring               │
│  Port Monitoring                  │
│  Login Monitoring                 │
│  File Integrity                   │
│  Security Report                  │
│                                   │
└───────────────────────────────────┘
```

The important part is **not** making a huge script.

The important part is understanding how each component works.

---

# 📝 Quick Revision

### 🐧 Linux Administration

```text
Users
Permissions
Processes
Services
Packages
Networking
Storage
```

### 📝 Security Logs

```text
Authentication
System Events
Service Events
Errors
Network Events
```

### 🐚 Shell Scripting

```text
Variables
Conditions
Loops
Functions
Pipes
Command Substitution
```

### ⚙️ Automation

```text
Identify Repetitive Task
        ↓
Create Script
        ↓
Test
        ↓
Schedule
        ↓
Monitor
        ↓
Verify
```

### 🛡️ Security Monitoring

```text
Collect
  ↓
Analyze
  ↓
Detect
  ↓
Investigate
  ↓
Alert
  ↓
Document
```

---

# 💡 Interview Tips

### ❓ Why is Linux administration important in cybersecurity?

Linux is widely used for servers, cloud infrastructure, security tools, and network services. Understanding Linux administration allows security professionals to understand normal system behavior and investigate abnormal activity.

### ❓ Why are logs important?

Logs provide records of system and user activity and are an important source of evidence during security monitoring and incident investigation.

### ❓ Why is shell scripting useful?

Shell scripting allows administrators and security professionals to automate repetitive tasks such as system auditing, log analysis, monitoring, and reporting.

### ❓ What is the benefit of automation?

Automation reduces repetitive manual work, improves consistency, saves time, and allows security teams to monitor systems more efficiently.

### ❓ Does an unusual log entry mean an attack occurred?

**No.** An unusual event is an indicator that requires investigation. Additional evidence is necessary to determine whether it is actually malicious.

### ❓ Why should security scripts use least privilege?

If a script is compromised or contains an error, excessive privileges can allow it to cause much greater damage.

### ❓ What is file integrity monitoring?

It is the process of detecting unexpected changes to files, commonly by comparing cryptographic hashes against a known-good baseline.

---

> **Remember:**
>
> 🐧 **First learn how to administer Linux.**
>
> 📝 **Then learn how to read what Linux is telling you.**
>
> 🐚 **Then learn to automate your work.**
>
> 🔍 **Then use automation to monitor security.**
>
> 🚨 **And always investigate evidence before declaring an incident.**

