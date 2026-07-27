# ⚠️ Understanding Cyber Threats

> "A system is not compromised because a threat exists—it is compromised when a threat exploits a vulnerability."

---

# 🧩 What are Cyber Threats?

A **Cyber Threat** is any potential event or action that can harm a computer system, network, application, or data.

Threats can come from:
- 👨‍💻 Hackers
- 🦠 Malware
- 👤 Insider threats
- 🌍 Nation-state attackers
- 🤖 Automated bots
- ⚠️ Human mistakes

### 🎯 Goal of a Threat

- Steal data
- Disrupt services
- Damage systems
- Gain unauthorized access
- Demand ransom

---

# 🎯 Threats

## 📖 Definition

A **Threat** is anything that has the potential to exploit a vulnerability and cause damage.

### 💡 Examples

- Malware
- Phishing emails
- Ransomware
- Insider attacks
- DDoS attacks

### 🌍 Real-World Example

A hacker wants to steal customer information from an online shopping website.

➡️ The **hacker** is the **Threat**.

---

# 🕳️ Vulnerabilities

## 📖 Definition

A **Vulnerability** is a weakness or flaw in a system that can be exploited by a threat.

### 💡 Examples

- Weak passwords
- Outdated software
- Misconfigured firewall
- Unpatched operating system
- SQL Injection vulnerability

### 🌍 Real-World Example

A company is running an old version of Windows with known security flaws.

➡️ The outdated Windows version is the **Vulnerability**.

---

# ⚠️ Risks

## 📖 Definition

A **Risk** is the possibility of a threat successfully exploiting a vulnerability and causing damage.

### 📌 Formula

```text
Risk = Threat + Vulnerability + Impact
```

### 💡 Examples

- Data theft
- Financial loss
- Reputation damage
- Business downtime

### 🌍 Real-World Example

An employee uses the password **123456**.

A hacker tries common passwords.

If successful, company data is exposed.

➡️ This possibility is the **Risk**.

---

# 💥 Exploits

## 📖 Definition

An **Exploit** is a method, tool, or piece of code used to take advantage of a vulnerability.

### 💡 Examples

- SQL Injection payload
- Buffer Overflow exploit
- EternalBlue exploit
- Cross-Site Scripting (XSS)

### 🌍 Real-World Example

A website has an SQL Injection vulnerability.

A hacker enters:

```sql
' OR '1'='1
```

The vulnerability is abused to bypass authentication.

➡️ The SQL Injection payload is the **Exploit**.

---

# 🚪 Attack Vectors

## 📖 Definition

An **Attack Vector** is the path or method an attacker uses to gain unauthorized access to a system.

Think of it as the **entry point** used by attackers.

### 💡 Common Attack Vectors

- 📧 Phishing Emails
- 🌐 Malicious Websites
- 🔌 Infected USB Drives
- 📶 Public Wi-Fi
- 📱 Mobile Apps
- 🌍 Open Network Ports
- ☁️ Cloud Misconfigurations
- 🔑 Weak Passwords

### 🌍 Real-World Example

A user clicks a fake bank email and enters their password.

➡️ The phishing email is the **Attack Vector**.

---

# 🔄 Relationship Between Them

```text
Threat
   │
   ▼
Finds a Vulnerability
   │
   ▼
Uses an Exploit
   │
   ▼
Through an Attack Vector
   │
   ▼
Creates a Risk
```

---

# 🌍 Real-World Scenario

Imagine a company website.

👨‍💻 **Threat**
> A hacker wants to steal customer data.

🕳️ **Vulnerability**
> The website has an outdated login page with an SQL Injection flaw.

💥 **Exploit**
> The hacker uses an SQL Injection payload.

🚪 **Attack Vector**
> The login page exposed on the internet.

⚠️ **Risk**
> Customer data is stolen, causing financial loss and reputation damage.

---

# 📊 Quick Comparison

| Concept | Meaning | Example |
|---------|---------|---------|
| 🎯 Threat | Potential danger | Hacker, Malware |
| 🕳️ Vulnerability | Weakness in a system | Weak Password |
| ⚠️ Risk | Chance of damage | Data Theft |
| 💥 Exploit | Tool or technique to abuse a vulnerability | SQL Injection Payload |
| 🚪 Attack Vector | Path used to attack | Phishing Email |

---

# 💭 Easy Way to Remember

Imagine a **house**:

🏠 **House** → Computer System

👨‍💻 **Thief** → Threat

🚪 **Unlocked Door** → Vulnerability

🛠️ **Lock Pick** → Exploit

🚪 **Front Door** → Attack Vector

💰 **Stolen Valuables** → Risk

---

# 📝 Quick Revision

### 🎯 Threat
Potential source of danger.

### 🕳️ Vulnerability
Weakness that can be exploited.

### ⚠️ Risk
Possibility of damage when a threat exploits a vulnerability.

### 💥 Exploit
Tool or technique used to exploit a vulnerability.

### 🚪 Attack Vector
The path used to carry out an attack.

---
> **💡 Interview Tip:**  
> Remember this sequence:
>
> **Threat → Vulnerability → Exploit → Attack Vector → Risk**
>
> Interviewers often ask you to explain these terms with a real-world example. Using the **house analogy** or a **website attack scenario** makes your answer much stronger.
