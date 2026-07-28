# 🧪 Apply Through Hands-on Security Tasks

> **"The best way to learn cybersecurity is by doing it. Hands-on practice transforms theoretical knowledge into practical skills."**

---

# 📖 What are Hands-on Security Tasks?

Hands-on security tasks are practical activities that allow you to apply cybersecurity concepts in a safe and controlled environment.

Instead of only reading about attacks and vulnerabilities, you learn by:

- 🔍 Investigating malware
- 🚨 Researching real-world vulnerabilities
- 🕵️ Performing OSINT investigations
- 📝 Documenting attack techniques
- 📊 Creating professional security reports

These tasks are commonly performed by SOC analysts, penetration testers, malware analysts, and security researchers.

---

# 🎯 Why are Hands-on Tasks Important?

Hands-on practice helps you:

- Understand concepts more deeply
- Gain real-world experience
- Improve problem-solving skills
- Learn industry workflows
- Build a cybersecurity portfolio
- Prepare for security jobs and certifications

> **Remember:** Reading teaches you **what** something is. Practice teaches you **how** it works.

---

# 🔄 Security Investigation Workflow

```text
       📚 Learn Concept
              │
              ▼
      🧪 Perform Lab
              │
              ▼
      🔍 Analyse Results
              │
              ▼
     📝 Document Findings
              │
              ▼
     📊 Improve Skills
```

---

# 🦠 Analyse Malware Samples Safely

## 📖 What is Malware Analysis?

Malware analysis is the process of studying malicious software to understand:

- How it behaves
- What damage it causes
- How it spreads
- How to detect and prevent it

> ⚠️ **Never execute malware on your personal computer.** Always use an isolated virtual lab.

---

## 🛡️ Safe Malware Analysis Environment

```text
        Malware Sample
               │
               ▼
        🖥️ Virtual Machine
               │
               ▼
      🌐 No Internet Access
               │
               ▼
       Observe Behaviour
               │
               ▼
      Document Findings
```

---

## Safe Practices

✅ Use Virtual Machines (VMware/VirtualBox)

✅ Take VM snapshots

✅ Disable shared folders

✅ Isolate the virtual network

✅ Never run unknown malware on your host machine

---

## Example Observation

While analysing a ransomware sample, you may observe:

- New files being created
- Registry changes
- Suspicious processes
- Network connections
- File encryption attempts

---

# 🆔 Research Recent CVEs

## 📖 What is CVE Research?

A CVE (Common Vulnerabilities and Exposures) is a publicly disclosed security vulnerability.

Researching CVEs helps security professionals understand:

- How vulnerabilities work
- Which products are affected
- Severity levels
- Available patches
- Exploitation methods

---

## CVE Research Workflow

```text
      CVE Number
          │
          ▼
 Search Official Database
          │
          ▼
Read Description
          │
          ▼
Understand Impact
          │
          ▼
Find Mitigation
```

---

## Trusted Sources

- 🌐 National Vulnerability Database (NVD)
- 🛡️ MITRE CVE Database
- 🚨 CISA Advisories
- 📰 Vendor Security Bulletins

---

## Example

Research:

```text
CVE-2021-44228
(Log4Shell)
```

Find:

- Description
- Affected software
- CVSS score
- Exploitation method
- Available patch

---

# 🕵️ Perform OSINT Investigations

## 📖 What is an OSINT Investigation?

An OSINT investigation involves collecting intelligence from publicly available sources without hacking or gaining unauthorized access.

---

## Example Workflow

```text
Target Company
        │
        ▼
🌐 Google Search
        │
        ▼
👥 LinkedIn Employees
        │
        ▼
📧 Email Format
        │
        ▼
🌍 WHOIS Records
        │
        ▼
📊 Investigation Report
```

---

## Information You Can Collect

- Company domains
- Public IP addresses
- Email formats
- Social media profiles
- Job postings
- DNS information
- Technology stack

---

## Common OSINT Tools

| Tool | Purpose |
|------|----------|
| Google | General research |
| Shodan | Internet-connected devices |
| WHOIS | Domain information |
| theHarvester | Email gathering |
| SpiderFoot | Automated OSINT |
| Maltego | Relationship mapping |

---

> ⚠️ Only collect publicly available information and always follow legal and ethical guidelines.

---

# 📖 Document Attack Techniques

## What is Attack Documentation?

Attack documentation records how a cyber attack works, making it easier to understand, detect, and defend against similar attacks.

Security teams create documentation after:

- Threat research
- Penetration testing
- Incident response
- Malware analysis

---

## Typical Report Structure

```text
Attack Name
      │
      ▼
Attack Description
      │
      ▼
Attack Method
      │
      ▼
Affected Systems
      │
      ▼
Indicators of Compromise
      │
      ▼
Mitigation
```

---

## Example

**Attack:** Phishing

Include:

- Attack objective
- Delivery method
- Target
- Impact
- Prevention techniques

---

## Benefits

- Knowledge sharing
- Faster incident response
- Security awareness
- Better future defence

---

# 📊 Create Vulnerability Reports

## 📖 What is a Vulnerability Report?

A vulnerability report is a document that explains:

- What vulnerability was found
- Where it exists
- Risk level
- Business impact
- Recommended remediation

These reports help IT teams understand what needs to be fixed.

---

## Typical Report Format

```text
Vulnerability
      │
      ▼
Affected System
      │
      ▼
Severity (CVSS)
      │
      ▼
Business Impact
      │
      ▼
Recommendation
      │
      ▼
Current Status
```

---

## Example Report

| Field | Example |
|--------|----------|
| Vulnerability | Outdated Apache Version |
| Severity | High (CVSS 8.8) |
| Affected Asset | Web Server |
| Risk | Remote Code Execution |
| Recommendation | Update Apache to latest version |
| Status | Pending |

---

# 🌍 Real-World Scenario

A company receives an alert about a newly discovered vulnerability.

```text
🚨 Security Advisory
         │
         ▼
Research CVE
         │
         ▼
Perform OSINT
         │
         ▼
Analyse Possible Attack
         │
         ▼
Create Vulnerability Report
         │
         ▼
Apply Patch
         │
         ▼
Verify Fix
```

This workflow is commonly followed by SOC analysts and security teams.

---

# ⚖️ Advantages & Limitations

## ✅ Advantages

- Improves practical cybersecurity skills
- Builds industry experience
- Develops analytical thinking
- Enhances report-writing skills
- Strengthens understanding of real-world threats
- Creates portfolio projects for GitHub

---

## ❌ Limitations

- Requires a safe lab environment
- Some tools have a learning curve
- Malware analysis can be dangerous if performed incorrectly
- Research may involve large amounts of information

---

# 📊 Comparison

| Task | Purpose | Example |
|------|---------|----------|
| 🦠 Malware Analysis | Understand malware behaviour | Analyse ransomware safely |
| 🆔 CVE Research | Learn about vulnerabilities | Research Log4Shell |
| 🕵️ OSINT Investigation | Gather public intelligence | WHOIS & LinkedIn research |
| 📖 Attack Documentation | Record attack details | Document a phishing attack |
| 📊 Vulnerability Report | Recommend fixes | Report outdated software |

---

# 🧠 Easy Memory Trick

Imagine you're a **cybersecurity detective**.

🦠 Examine the evidence (**Malware Analysis**)

🆔 Check the criminal database (**CVE Research**)

🕵️ Gather public clues (**OSINT**)

📖 Write what happened (**Attack Documentation**)

📊 Submit the investigation report (**Vulnerability Report**)

---

# 📝 Quick Revision

### 🦠 Analyse Malware Samples
- Use isolated virtual machines
- Observe behaviour safely
- Never analyse malware on your host system

### 🆔 Research CVEs
- Learn about vulnerabilities
- Check severity and available patches
- Use trusted databases

### 🕵️ Perform OSINT
- Gather only public information
- Use ethical investigation techniques

### 📖 Document Attack Techniques
- Record attack methods
- Include indicators and mitigation

### 📊 Create Vulnerability Reports
- Describe the issue
- Explain the impact
- Recommend remediation

---

# 💡 Interview Tip

### ❓What is the purpose of documenting vulnerabilities and attack techniques?

Documentation helps security teams:

- Share knowledge
- Improve incident response
- Track vulnerabilities
- Prioritise remediation
- Meet compliance requirements
- Build a historical record for future investigations

> **Remember:**  
> A skilled cybersecurity professional doesn't just **find problems**—they also **document, explain, and communicate them clearly** so others can understand and fix them.
````
