# 🌐 Understanding Network Protocols

> **"Network protocols are the rules that allow devices to communicate over a network. Just as people use languages to communicate, computers use protocols to exchange information accurately and securely."**

---

# 📖 What is a Network Protocol?

A **Network Protocol** is a set of rules and standards that define how data is transmitted, received, and interpreted between devices on a network.

Protocols ensure that different devices, operating systems, and applications can communicate with each other reliably.

---

## 🎯 Why are Network Protocols Important?

- Enable communication between devices
- Standardize data exchange
- Ensure reliable and secure transmission
- Allow different vendors' devices to work together
- Support Internet services like websites, email, and file sharing

---

## 🌍 Real-World Example

Imagine sending a letter.

- 📄 Letter → Data
- ✉️ Envelope → Packet
- 📮 Postal Rules → Protocol
- 🏠 Address → IP Address
- 📬 Post Office → Router

Without postal rules, the letter would never reach its destination. Similarly, without protocols, devices cannot communicate.

---

# 🌐 HTTP (HyperText Transfer Protocol)

## 📖 What is HTTP?

HTTP is the protocol used to transfer **web pages** between a web browser and a web server.

It is the foundation of the World Wide Web.

---

## ⚙️ How HTTP Works

```text
Browser
   │
HTTP Request
   │
   ▼
Web Server
   │
HTTP Response
   │
   ▼
Website Displayed
```

---

## Default Port

**Port 80**

---

## Features

- Fast
- Stateless
- Easy to implement

---

## Limitations

❌ No encryption

❌ Data can be intercepted

❌ Vulnerable to Man-in-the-Middle attacks

---

## Example

```text
http://example.com
```

---

# 🔒 HTTPS (HyperText Transfer Protocol Secure)

## 📖 What is HTTPS?

HTTPS is the **secure version of HTTP**.

It encrypts communication using **SSL/TLS**, protecting data from attackers.

---

## ⚙️ How HTTPS Works

```text
Browser
   │
Encrypted HTTPS Request
   │
   ▼
Web Server
   │
Encrypted Response
   │
   ▼
Secure Website
```

---

## Default Port

**Port 443**

---

## Benefits

✅ Encryption

✅ Authentication

✅ Data Integrity

---

## Example

```text
https://bank.com
```

The padlock 🔒 in your browser indicates that HTTPS is being used.

---

# ⚖️ HTTP vs HTTPS

| Feature | HTTP | HTTPS |
|----------|-------|--------|
| Encryption | ❌ No | ✅ Yes |
| Security | Low | High |
| Port | 80 | 443 |
| SSL/TLS | ❌ | ✅ |
| Browser Padlock | ❌ | ✅ |

---

# 🌍 DNS (Domain Name System)

## 📖 What is DNS?

DNS is often called the **"Phonebook of the Internet."**

It converts **domain names** into **IP addresses** that computers can understand.

---

## Why is DNS Needed?

Humans remember:

```text
www.google.com
```

Computers understand:

```text
142.250.xxx.xxx
```

DNS translates between the two.

---

## ⚙️ How DNS Works

```text
User
 │
 ▼
www.google.com
 │
 ▼
DNS Server
 │
 ▼
Returns IP Address
 │
 ▼
Browser Connects to Server
```

---

## Default Port

**Port 53**

---

## Examples

- google.com
- youtube.com
- github.com

---

# 📡 DHCP (Dynamic Host Configuration Protocol)

## 📖 What is DHCP?

DHCP automatically assigns network settings to devices when they join a network.

Instead of configuring IP addresses manually, DHCP performs the task automatically.

---

## Information Assigned

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

---

## ⚙️ How DHCP Works

```text
Computer Joins Network
        │
        ▼
DHCP Discover
        │
        ▼
DHCP Offer
        │
        ▼
DHCP Request
        │
        ▼
DHCP Acknowledgement
```

This process is known as **DORA**:

- Discover
- Offer
- Request
- Acknowledge

---

## Default Ports

- **67 (Server)**
- **68 (Client)**

---

# 📂 FTP (File Transfer Protocol)

## 📖 What is FTP?

FTP is used to transfer files between computers over a network.

It allows users to upload, download, rename, and delete files on a remote server.

---

## ⚙️ How FTP Works

```text
FTP Client
     │
Login
     │
     ▼
FTP Server
     │
Upload / Download Files
```

---

## Default Ports

- **21 (Control)**
- **20 (Data)**

---

## Advantages

- Fast file transfer
- Easy file management

---

## Limitations

❌ No encryption

❌ Credentials sent in plain text

---

## Secure Alternatives

- SFTP
- FTPS

---

# 📧 SMTP (Simple Mail Transfer Protocol)

## 📖 What is SMTP?

SMTP is the protocol responsible for **sending emails**.

It transfers outgoing email from a client to a mail server and between mail servers.

---

## ⚙️ How SMTP Works

```text
Sender
   │
Email Client
   │
SMTP Server
   │
Internet
   │
Recipient Mail Server
```

---

## Default Ports

- **25**
- **465 (Secure)**
- **587 (Submission)**

---

## Note

SMTP is used only for **sending** emails.

Receiving emails typically uses:

- POP3
- IMAP

---

# 🔑 SSH (Secure Shell)

## 📖 What is SSH?

SSH is a secure protocol used for remote login and secure command execution on another computer.

It encrypts all communication between the client and the remote system.

---

## ⚙️ How SSH Works

```text
Administrator
      │
Encrypted SSH Connection
      │
      ▼
Remote Linux Server
```

---

## Default Port

**22**

---

## Uses

- Remote server administration
- Secure file transfer (SCP/SFTP)
- Network device management
- Secure automation

---

## Advantages

✅ Strong encryption

✅ Secure authentication

✅ Remote management

---

# 📊 Common Network Protocols

| Protocol | Full Form | Port | Purpose |
|----------|-----------|------|---------|
| HTTP | HyperText Transfer Protocol | 80 | Web Browsing |
| HTTPS | HyperText Transfer Protocol Secure | 443 | Secure Web Browsing |
| DNS | Domain Name System | 53 | Name Resolution |
| DHCP | Dynamic Host Configuration Protocol | 67/68 | IP Address Assignment |
| FTP | File Transfer Protocol | 20/21 | File Transfer |
| SMTP | Simple Mail Transfer Protocol | 25/465/587 | Sending Email |
| SSH | Secure Shell | 22 | Secure Remote Access |

---

# 🌍 Real-World Example

When you visit **https://github.com**:

```text
You Type github.com
        │
        ▼
DNS Resolves Domain
        │
        ▼
IP Address Found
        │
        ▼
HTTPS Connection Established
        │
        ▼
Web Server Sends Secure Webpage
        │
        ▼
Browser Displays Website
```

If you're managing a Linux server, you might use:

```text
SSH ──► Remote Server
```

To upload files:

```text
FTP/SFTP ──► Server
```

To send an email:

```text
SMTP ──► Mail Server
```

---

# 🧠 Memory Trick

Think of building a website:

```text
DNS     → Finds the website
HTTP    → Opens the website
HTTPS   → Opens it securely
DHCP    → Gives your computer an IP address
FTP     → Uploads website files
SMTP    → Sends emails
SSH     → Lets you securely manage the server
```

---

# 📝 Quick Revision

### 🌐 HTTP
- Web browsing
- Port 80
- Not encrypted

### 🔒 HTTPS
- Secure web browsing
- Port 443
- Uses SSL/TLS encryption

### 🌍 DNS
- Converts domain names to IP addresses
- Port 53

### 📡 DHCP
- Automatically assigns IP addresses
- Ports 67 & 68
- Uses DORA process

### 📂 FTP
- Transfers files
- Ports 20 & 21
- Not secure by default

### 📧 SMTP
- Sends emails
- Ports 25, 465, 587

### 🔑 SSH
- Secure remote login
- Port 22
- Encrypted communication

---

# 💡 Interview Tip

### ❓Which protocol would you use for the following tasks?

| Task | Protocol |
|------|----------|
| Open a website | HTTP / HTTPS |
| Secure website access | HTTPS |
| Find an IP from a domain name | DNS |
| Get an IP address automatically | DHCP |
| Upload files to a server | FTP / SFTP |
| Send an email | SMTP |
| Securely manage a Linux server | SSH |

> **Remember:**
>
> 🌍 **DNS finds the server.**
>
> 🌐 **HTTP/HTTPS communicate with the server.**
>
> 📡 **DHCP gives your device an IP address.**
>
> 📂 **FTP transfers files.**
>
> 📧 **SMTP sends emails.**
>
> 🔑 **SSH securely controls remote systems.**
