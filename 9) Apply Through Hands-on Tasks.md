# 🛠️ Apply Through Hands-on Tasks

> **"Networking is best learned by doing. Reading concepts builds knowledge, but hands-on practice builds real skills. These activities help you think like a Network Engineer and a Cybersecurity Analyst."**

---

# 📖 Why Hands-on Practice?

Theory explains **how networking works**, but practical exercises teach you **how to analyze, troubleshoot, and secure networks**.

Hands-on tasks help you:

- Apply networking concepts
- Improve troubleshooting skills
- Understand real network traffic
- Develop cybersecurity thinking
- Prepare for certifications and job roles

---

# 🎯 Skills You Will Develop

- Packet analysis
- Network troubleshooting
- Protocol identification
- IP addressing and subnetting
- Network documentation
- Incident investigation
- Critical thinking

---

# 📦 A. Analyze Packet Captures

## 📖 Objective

Learn how data travels across a network by examining **packet captures (PCAP files)**.

Packet analysis helps you understand network communication and detect suspicious activities.

---

## Tools

- Wireshark
- tcpdump
- TShark

---

## What to Analyze

- Source IP
- Destination IP
- MAC Address
- Protocol
- Port Numbers
- TCP Flags
- Packet Length
- Timestamps

---

## Example Workflow

```text
Capture Traffic
        │
        ▼
Open PCAP in Wireshark
        │
        ▼
Filter Protocols
        │
        ▼
Inspect Packets
        │
        ▼
Identify Suspicious Activity
```

---

## Practice Tasks

- Find the source and destination IPs.
- Identify HTTP and HTTPS traffic.
- Follow a TCP stream.
- Count DNS requests.
- Detect failed login attempts.
- Identify large file transfers.

---

## Why It Matters

Packet analysis is used in:

- Incident Response
- Malware Analysis
- Threat Hunting
- Digital Forensics
- Network Troubleshooting

---

# 🌐 B. Perform Subnet Calculations

## 📖 Objective

Learn how to divide networks into smaller subnets and determine:

- Network Address
- Broadcast Address
- Valid Host Range
- Number of Hosts

---

## Example

Given:

```text
IP Address

192.168.10.45/24
```

Find:

- Network Address → 192.168.10.0
- Broadcast Address → 192.168.10.255
- Usable Hosts → 192.168.10.1 – 192.168.10.254
- Total Hosts → 254

---

## Practice Tasks

Calculate subnet information for:

```text
192.168.1.0/24

10.0.0.0/16

172.16.5.0/24
```

---

## Useful Tools

- Subnet Calculator
- CIDR Calculator
- ipcalc (Linux)

---

## Why It Matters

Subnetting is essential for:

- Network Design
- Routing
- Firewall Rules
- VLAN Configuration
- IP Address Planning

---

# 🌍 C. Identify Network Protocols

## 📖 Objective

Recognize common network protocols by observing network traffic.

---

## Common Protocols

| Protocol | Default Port | Purpose |
|-----------|-------------|---------|
| HTTP | 80 | Web Browsing |
| HTTPS | 443 | Secure Web Browsing |
| DNS | 53 | Domain Name Resolution |
| DHCP | 67/68 | IP Address Assignment |
| FTP | 20/21 | File Transfer |
| SMTP | 25/465/587 | Email Sending |
| SSH | 22 | Secure Remote Access |

---

## Practice Tasks

Using Wireshark:

- Identify HTTP packets.
- Find DNS queries.
- Locate HTTPS traffic.
- Detect SSH sessions.
- Observe DHCP DORA process.
- Differentiate TCP and UDP traffic.

---

## Why It Matters

Understanding protocols helps analysts:

- Troubleshoot connectivity
- Detect attacks
- Investigate incidents
- Secure network services

---

# 🗺️ D. Build Network Diagrams

## 📖 Objective

Create simple diagrams that show how devices communicate within a network.

Network diagrams improve understanding and documentation.

---

## Basic Home Network

```text
            🌍 Internet
                 │
             ┌────────┐
             │ Router │
             └────────┘
            /     |     \
           /      |      \
      Laptop   Phone   Desktop
```

---

## Small Office Network

```text
             Internet
                 │
              Firewall
                 │
               Router
                 │
              Switch
       ┌───────┼────────┐
       │       │        │
      PC     Server   Printer
```

---

## Tools

- Draw.io (diagrams.net)
- Microsoft Visio
- Lucidchart
- Microsoft PowerPoint

---

## Practice Tasks

Create diagrams for:

- Home Wi-Fi Network
- Office LAN
- School Network
- Cloud-based Architecture

---

## Why It Matters

Network diagrams are used for:

- Documentation
- Troubleshooting
- Security Planning
- Incident Response
- Network Expansion

---

# 🚨 E. Investigate Network Incidents

## 📖 Objective

Learn how to investigate suspicious network activity using logs, packet captures, and network monitoring tools.

---

## Basic Investigation Process

```text
Alert Received
       │
       ▼
Collect Logs
       │
       ▼
Analyze Packets
       │
       ▼
Identify Indicators of Compromise
       │
       ▼
Determine Root Cause
       │
       ▼
Document Findings
```

---

## Common Incidents

- Brute-force login attempts
- Malware communication
- Port scanning
- DNS attacks
- Data exfiltration
- DDoS attacks

---

## Investigation Checklist

✔ Identify source IP

✔ Identify destination IP

✔ Determine protocol used

✔ Check timestamps

✔ Review firewall logs

✔ Examine packet captures

✔ Search for Indicators of Compromise (IoCs)

✔ Document findings

---

## Example

A firewall reports repeated failed SSH logins.

Investigation:

```text
Firewall Alert
        │
        ▼
Review SSH Logs
        │
        ▼
Analyze Source IP
        │
        ▼
Detect Brute-Force Attempt
        │
        ▼
Block Attacker
        │
        ▼
Generate Incident Report
```

---

## Why It Matters

Network investigations help organizations:

- Detect attacks early
- Reduce damage
- Improve defenses
- Understand attacker behavior
- Support digital forensics

---

# 🧰 Recommended Practice Tools

| Tool | Purpose |
|------|---------|
| Wireshark | Packet Analysis |
| tcpdump | Command-Line Packet Capture |
| Cisco Packet Tracer | Network Simulation |
| GNS3 | Advanced Network Lab |
| Nmap | Network Discovery & Scanning |
| Draw.io | Network Diagrams |
| Security Onion | Network Monitoring |
| Kali Linux | Security Testing |

---

# 🌍 Real-World Example

A company notices slow network performance.

```text
Users Report Slow Internet
          │
          ▼
Capture Network Traffic
          │
          ▼
Analyze in Wireshark
          │
          ▼
Identify Large DNS Requests
          │
          ▼
Detect Malware Communication
          │
          ▼
Block Malicious Device
          │
          ▼
Restore Normal Operations
```

By combining packet analysis, protocol identification, and incident investigation, the security team quickly finds and resolves the issue.

---

# 🧠 Memory Trick

Think like a **Cybersecurity Detective**:

- 📦 **Packets** = Clues
- 🌐 **Protocols** = Language spoken by the suspects
- 🗺️ **Network Diagram** = Crime scene map
- 📋 **Logs** = Witness statements
- 🚨 **Incident Investigation** = Solving the case

The more evidence you collect and verify, the better your conclusions.

---

# 📝 Quick Revision

### 📦 Analyze Packet Captures

- Use Wireshark or tcpdump
- Inspect packets and identify suspicious activity

### 🌐 Perform Subnet Calculations

- Find network address
- Calculate host range
- Understand CIDR notation

### 🔍 Identify Network Protocols

- Recognize HTTP, HTTPS, DNS, DHCP, FTP, SMTP, SSH
- Understand their ports and purposes

### 🗺️ Build Network Diagrams

- Visualize network layouts
- Document devices and connections

### 🚨 Investigate Network Incidents

- Collect evidence
- Analyze logs and packets
- Identify threats
- Document findings

---

# 💡 Interview Tip

### ❓Why are hands-on networking labs important?

**Answer:**

Hands-on labs bridge the gap between theory and real-world practice. They teach you how to analyze traffic, troubleshoot issues, investigate attacks, and build secure networks—skills that are essential for cybersecurity professionals.

> **Remember:**
>
> 📦 **Analyze packets to understand communication.**
>
> 🌐 **Practice subnetting to master IP addressing.**
>
> 🔍 **Identify protocols to understand network services.**
>
> 🗺️ **Build diagrams to visualize network architecture.**
>
> 🚨 **Investigate incidents to develop real-world cybersecurity skills.**
