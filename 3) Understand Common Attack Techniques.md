# 🎯 Understanding Common Attack Techniques

> "Most cyber attacks don't begin with sophisticated hacking—they begin by exploiting human trust, weak passwords, or publicly available information."

---

# 📖 What are Attack Techniques?

**Attack Techniques** are the methods or strategies used by attackers to gain unauthorized access to systems, steal sensitive information, or disrupt services.

Rather than attacking systems randomly, cybercriminals often follow a structured approach. They first gather information, identify weaknesses, and then exploit them using different attack techniques.

---

# 🎯 Why are Attack Techniques Important?

Understanding attack techniques helps security professionals:

- Detect attacks early.
- Build stronger security measures.
- Train users to recognise threats.
- Reduce the risk of successful cyber attacks.
- Improve incident response.

By learning how attackers think and operate, defenders can better protect systems and data.

---

# ⚙️ Typical Attack Flow

Many cyber attacks follow a similar process:

```text
Reconnaissance
      │
      ▼
Target Selection
      │
      ▼
Social Engineering / Phishing
      │
      ▼
Credential Theft
      │
      ▼
Malware Installation
      │
      ▼
Data Theft or System Compromise
```

Attackers may not always follow every step, but reconnaissance and credential theft are common in many attacks.

---

# 🎣 Phishing

## 📖 What is Phishing?

**Phishing** is a cyber attack where attackers pretend to be a trusted person or organisation to trick victims into revealing sensitive information or installing malware.

Phishing is one of the most common and successful cyber attacks because it targets **people rather than technology**.

---

## ⚙️ How Phishing Works

1. Attacker creates a fake email, message, or website.
2. The message appears to come from a trusted source.
3. The victim clicks a malicious link or opens an attachment.
4. The victim enters credentials or downloads malware.
5. The attacker gains access to accounts or systems.

---

## Common Types of Phishing

- 📧 Email Phishing
- 🎯 Spear Phishing (Targeted attack)
- 👔 Whaling (Targets executives)
- 📱 Smishing (SMS phishing)
- 📞 Vishing (Voice phishing)

---

## 🌍 Real-World Example

A user receives an email claiming to be from Microsoft asking them to verify their account.

The email contains a fake login page.

The user enters their username and password.

The attacker steals the credentials.

---

## Prevention

- Verify sender email addresses.
- Don't click suspicious links.
- Enable Multi-Factor Authentication (MFA).
- Check website URLs carefully.
- Report suspicious emails.

---

# 🧠 Social Engineering

## 📖 What is Social Engineering?

**Social Engineering** is the psychological manipulation of people to make them reveal confidential information or perform actions that benefit the attacker.

Instead of exploiting software vulnerabilities, attackers exploit **human behaviour**.

---

## ⚙️ How it Works

1. Attacker gathers information about the victim.
2. Builds trust or creates urgency.
3. Manipulates the victim.
4. Victim unknowingly reveals information or grants access.

---

## Common Techniques

- Pretexting
- Baiting
- Tailgating
- Impersonation
- Quid Pro Quo

---

## 🌍 Real-World Example

An attacker calls an employee pretending to be an IT technician and asks for their login credentials to "fix a problem."

The employee trusts the caller and shares the password.

---

## Prevention

- Verify identities before sharing information.
- Never reveal passwords.
- Follow company verification procedures.
- Participate in security awareness training.

---

# 🔑 Credential Attacks

## 📖 What are Credential Attacks?

Credential attacks aim to steal, guess, or misuse usernames and passwords to gain unauthorized access.

Since passwords protect most digital accounts, they are a primary target for attackers.

---

## Common Credential Attacks

### Brute Force Attack

The attacker tries every possible password combination until the correct one is found.

### Dictionary Attack

Uses a list of commonly used passwords instead of trying every combination.

### Credential Stuffing

Uses usernames and passwords leaked from one website to access accounts on other websites.

### Password Spraying

Attempts a few common passwords against many user accounts to avoid account lockouts.

---

## 🌍 Real-World Example

A user uses the same password for social media and online banking.

After a social media data breach, attackers use the leaked credentials to log into the banking account.

---

## Prevention

- Use unique passwords.
- Enable MFA.
- Use a password manager.
- Monitor for compromised credentials.

---

# 🦠 Malware Delivery

## 📖 What is Malware Delivery?

**Malware Delivery** refers to the methods attackers use to distribute malicious software to victims.

The malware itself is harmful, but it first needs a way to reach the victim.

---

## Common Delivery Methods

- Email attachments
- Malicious websites
- Fake software downloads
- USB devices
- Drive-by downloads
- Compromised advertisements (Malvertising)

---

## ⚙️ How it Works

```text
Malicious Email
       │
       ▼
Victim Opens Attachment
       │
       ▼
Malware Executes
       │
       ▼
System Becomes Infected
```

---

## 🌍 Real-World Example

A user downloads a pirated application from an untrusted website.

The installer secretly installs ransomware alongside the application.

---

## Prevention

- Download software only from trusted sources.
- Scan email attachments.
- Keep antivirus software updated.
- Disable unnecessary macros in documents.

---

# 🔍 Reconnaissance Activities

## 📖 What is Reconnaissance?

**Reconnaissance** is the process of gathering information about a target before launching an attack.

It is often the **first phase of a cyber attack**.

Attackers collect information to identify weaknesses and plan their attack.

---

## Types of Reconnaissance

### Passive Reconnaissance

Information is gathered without directly interacting with the target.

Examples:

- Company website
- Social media
- Public records
- Job postings
- Search engines

### Active Reconnaissance

The attacker directly interacts with the target system.

Examples:

- Port scanning
- Network scanning
- Service enumeration
- Vulnerability scanning

---

## 🌍 Real-World Example

Before attacking a company, an attacker:

- Finds employee names on LinkedIn.
- Collects email addresses from the company website.
- Scans public servers for open ports.
- Uses the information to launch a targeted phishing campaign.

---

## Prevention

- Limit publicly available information.
- Disable unnecessary services.
- Monitor network scans.
- Use firewalls and intrusion detection systems.
- Conduct regular security assessments.

---

# 📊 Comparison

| Technique | Primary Goal | Main Target |
|-----------|--------------|-------------|
| 🎣 Phishing | Steal information | Users |
| 🧠 Social Engineering | Manipulate people | Human behaviour |
| 🔑 Credential Attacks | Obtain valid login credentials | User accounts |
| 🦠 Malware Delivery | Infect systems | Devices & Networks |
| 🔍 Reconnaissance | Gather information | Target infrastructure |

---

# 🌍 Real-World Attack Scenario

An attacker wants to compromise a company's network.

1. 🔍 Performs **Reconnaissance** by collecting employee emails.
2. 🎣 Sends a **Phishing** email pretending to be the HR department.
3. 🧠 Uses **Social Engineering** to convince employees to act quickly.
4. 🔑 Steals employee credentials through a fake login page.
5. 🦠 Uses the compromised account to deliver malware and gain deeper access to the network.

This demonstrates how multiple attack techniques are often combined in a single cyber attack.

---

# 💭 Easy Way to Remember

Imagine a burglar planning to rob a house.

- 🔍 **Reconnaissance** → Watches the house and learns the owner's routine.
- 🧠 **Social Engineering** → Pretends to be a delivery person.
- 🎣 **Phishing** → Sends a fake message asking the owner to unlock the door.
- 🔑 **Credential Attack** → Tries to obtain or guess the house key.
- 🦠 **Malware Delivery** → Places a hidden tracking device inside the house.

---

# 📝 Quick Revision

### 🎣 Phishing
- Fake emails or websites
- Steals sensitive information

### 🧠 Social Engineering
- Manipulates people instead of systems
- Exploits trust and human behaviour

### 🔑 Credential Attacks
- Target usernames and passwords
- Includes brute force, credential stuffing, and password spraying

### 🦠 Malware Delivery
- Distributes malicious software
- Common methods include email attachments and fake downloads

### 🔍 Reconnaissance
- First stage of many cyber attacks
- Gathers information about the target

---

> **💡 Interview Tip:**  
> A common interview question is:
>
> **"What is the difference between Phishing and Social Engineering?"**
>
> - 🎣 **Phishing** is a **specific type of social engineering attack** that uses fake emails, messages, or websites to steal information.
> - 🧠 **Social Engineering** is the **broader concept** of manipulating people into revealing information or performing actions that benefit the attacker.
>
> **Remember:** **Every phishing attack is a form of social engineering, but not every social engineering attack is phishing.**
````
