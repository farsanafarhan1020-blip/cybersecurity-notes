# 🔐 10. Apply Through Hands-on Tasks — Cryptography

> **“Cryptography becomes useful when you can observe, test, verify, and document it.”**

This section combines the concepts learned throughout the Cryptography module into practical cybersecurity tasks.

You will work with:

* HTTPS traffic
* TLS certificates
* Hashes
* Encryption
* Secure communication
* Security analysis
* Technical reporting

The goal is not simply to run commands.

The goal is to understand:

```text
What happened?
     ↓
Why did it happen?
     ↓
What cryptographic mechanism was involved?
     ↓
What security property did it provide?
     ↓
What evidence can I show?
     ↓
How do I document it?
```

---

# 🌐 1. Analyze HTTPS Traffic

HTTPS is one of the best places to observe cryptography in action.

Remember:

```text
HTTP
 +
TLS
 ↓
HTTPS
```

TLS provides a secure communication channel using several cryptographic mechanisms.

---

## 🔎 Task 1 — Observe HTTPS With Your Browser

Open a website that uses HTTPS.

For example:

```text
https://example.com
```

Look at the browser's security information.

Depending on the browser, you can inspect:

* Connection security
* Certificate
* Issuer
* Validity
* Hostname
* Encryption details

Your goal is to identify:

```text
Website
   ↓
HTTPS
   ↓
TLS
   ↓
Certificate
   ↓
Key Agreement
   ↓
Encrypted Traffic
```

---

## 🧠 Questions to Answer

Write down:

1. Is the connection using HTTPS?
2. What TLS version is being used, if shown?
3. Who issued the certificate?
4. What hostname does the certificate cover?
5. When does the certificate expire?
6. Who is the certificate issued to?
7. Is the connection encrypted?
8. What cryptographic algorithms are visible?

Do not assume that simply seeing a padlock means the website is trustworthy.

HTTPS protects the connection, but it does not guarantee that the website itself is legitimate.

---

# 🦈 Task 2 — Analyze HTTPS With Wireshark

If you already have Wireshark installed, you can observe TLS traffic.

Start Wireshark and capture traffic while visiting an HTTPS website.

Use a display filter such as:

```text
tls
```

You may see packets such as:

```text
ClientHello
ServerHello
Certificate
CertificateVerify
Finished
Application Data
```

The exact visible details depend on the TLS version and traffic.

---

## 🔬 What to Look For

### ClientHello

The client begins TLS negotiation.

Look for information such as:

* Supported TLS versions
* Cipher suites
* Extensions
* Supported groups
* Server Name Indication (SNI), when present

---

### ServerHello

The server selects parameters for the connection.

---

### Certificate

The server provides its certificate chain.

This allows the client to authenticate the server through the configured trust system.

---

### CertificateVerify

The server proves possession of the private key corresponding to its certificate.

---

### Encrypted Application Data

After the handshake establishes the necessary keys, application traffic is protected.

You should generally **not** expect to see ordinary HTTP contents in plaintext.

---

# ⚠️ Important Wireshark Concept

You may see:

```text
TLS
Encrypted Application Data
```

but you normally cannot simply read the webpage contents from those packets.

That is the purpose of encryption.

Conceptually:

```text
Plaintext
    ↓
TLS Record Protection
    ↓
Encrypted Data
    ↓
Network
    ↓
Encrypted Data
    ↓
TLS Decryption
    ↓
Plaintext
```

---

# 📜 2. Investigate Certificates

Certificates are an important part of modern secure communication.

A TLS certificate helps bind:

```text
Identity / Domain
       ↓
Public Key
       ↓
Certificate
       ↓
Certificate Authority Trust
```

---

# 🔎 Task 3 — Inspect a Website Certificate

Open a website using HTTPS.

Inspect its certificate.

Record:

| Field                     | Your Observation |
| ------------------------- | ---------------- |
| Subject                   |                  |
| Issuer                    |                  |
| Valid From                |                  |
| Valid Until               |                  |
| Public Key Algorithm      |                  |
| Signature Algorithm       |                  |
| Subject Alternative Names |                  |
| Certificate Chain         |                  |

---

# 🧠 Understand the Certificate Chain

A typical chain looks like:

```text
Root CA
   │
   ↓
Intermediate CA
   │
   ↓
Server Certificate
   │
   ↓
example.com
```

The browser uses its configured trust store to determine whether the certificate chain leads to a trusted root.

---

# 🔐 Task 4 — Verify the Certificate With OpenSSL

From Linux/Kali:

```bash
openssl s_client -connect example.com:443 -servername example.com
```

This establishes a TLS connection and displays certificate and handshake information.

You can inspect details such as:

* Certificate chain
* Subject
* Issuer
* TLS version
* Cipher
* Verification result

To inspect a certificate more directly, you can save or extract certificate data and use:

```bash
openssl x509 -text -noout
```

---

# 🧪 Useful Certificate Questions

When investigating a certificate, ask:

```text
Who issued it?
Who is it issued to?
Is the hostname covered?
Is it currently valid?
Is the chain trusted?
What public-key algorithm is used?
What signature algorithm is used?
```

These questions turn certificate inspection into a security investigation.

---

# #️⃣ 3. Explore Hashing Workflows

Hashing is another practical cryptography concept you can test very easily.

A hash function converts input into a fixed-size digest.

```text
Input
  ↓
Hash Function
  ↓
Digest
```

For SHA-256:

```text
Input
  ↓
SHA-256
  ↓
256-bit digest
```

---

# 🧪 Task 5 — Hash a Text File

Create a file:

```bash
echo "Hello Cybersecurity" > message.txt
```

Calculate its SHA-256 hash:

```bash
sha256sum message.txt
```

You should receive a hash similar to:

```text
<64 hexadecimal characters>  message.txt
```

The exact value will depend on the file contents.

---

# 🔄 Task 6 — Demonstrate the Avalanche Effect

Change one character:

```bash
echo "Hello Cybersecurity!" > message.txt
```

Run:

```bash
sha256sum message.txt
```

Compare the two hashes.

Even though the input changed only slightly:

```text
Original
↓
SHA-256
↓
Hash A
```

versus:

```text
Modified
↓
SHA-256
↓
Hash B
```

the resulting digest should be dramatically different.

This demonstrates the **avalanche effect**.

---

# 🐍 Task 7 — Hash Data Using Python

You can also perform hashing using Python:

```python
import hashlib

message = b"Hello Cybersecurity"

digest = hashlib.sha256(message).hexdigest()

print("SHA-256:", digest)
```

Output:

```text
SHA-256: <64 hexadecimal characters>
```

---

# 📁 Task 8 — Hash an Actual File

Create:

```python
import hashlib

filename = "message.txt"

with open(filename, "rb") as file:
    data = file.read()

digest = hashlib.sha256(data).hexdigest()

print("SHA-256:", digest)
```

For larger files, reading the entire file into memory is unnecessary.

A better approach is to process the file in chunks:

```python
import hashlib

filename = "message.txt"

sha256 = hashlib.sha256()

with open(filename, "rb") as file:
    while chunk := file.read(4096):
        sha256.update(chunk)

print("SHA-256:", sha256.hexdigest())
```

This is a useful pattern for security tools that calculate hashes of large files.

---

# 🛡️ 4. Demonstrate Encryption Concepts

Now demonstrate the difference between:

```text
Plaintext
     ↓
Encryption
     ↓
Ciphertext
     ↓
Decryption
     ↓
Plaintext
```

---

# 🧪 Task 9 — Symmetric Encryption With Python

Using the Python `cryptography` library:

```bash
pip install cryptography
```

Then:

```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()

cipher = Fernet(key)

message = b"Cybersecurity training"

encrypted = cipher.encrypt(message)

print("Encrypted:", encrypted)

decrypted = cipher.decrypt(encrypted)

print("Decrypted:", decrypted.decode())
```

Conceptually:

```text
"Cybersecurity training"
          ↓
       Encrypt
          ↓
      Ciphertext
          ↓
       Decrypt
          ↓
"Cybersecurity training"
```

---

# 🔑 Understand What Happened

The program generated a secret key:

```python
key = Fernet.generate_key()
```

Then the key was used by the Fernet cipher to protect the message.

The same secret key is required to decrypt the protected data.

This demonstrates the basic concept of **symmetric encryption**.

---

# ⚠️ Important Real-World Lesson

Do not treat the example as a complete production key-management system.

In a real application, you must consider:

* Key storage
* Key access control
* Key rotation
* Key backup
* Key revocation
* Key destruction
* Secret management

For example:

```text
Hardcoded key
      ❌

Environment/secret management
      ↓
Controlled access
      ↓
Application
```

Never commit real secret keys to GitHub.

---

# 🔐 Task 10 — Demonstrate Asymmetric Cryptography

You can also demonstrate public/private keys.

Generate an RSA key pair:

```python
from cryptography.hazmat.primitives.asymmetric import rsa

private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)

public_key = private_key.public_key()

print("Private key:", private_key)
print("Public key:", public_key)
```

Conceptually:

```text
             RSA Key Pair
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
   Private Key          Public Key
   🔒 Secret             📢 Shareable
```

The private key must be protected.

---

# ✍️ Task 11 — Demonstrate Digital Signatures

You can extend the RSA example to demonstrate signing and verification.

```python
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import padding

message = b"Cybersecurity report"

signature = private_key.sign(
    message,
    padding.PSS(
        mgf=padding.MGF1(hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)

public_key.verify(
    signature,
    message,
    padding.PSS(
        mgf=padding.MGF1(hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)

print("Signature verified.")
```

Conceptually:

```text
Message
   ↓
Hash
   ↓
Private Key
   ↓
Signature
```

Verification:

```text
Message + Signature + Public Key
              ↓
           Verify
              ↓
       Valid / Invalid
```

---

# 🧪 Task 12 — Modify the Message

After creating the signature, change the message:

```python
message = b"Modified cybersecurity report"
```

Try verification again.

The signature should no longer verify against the modified message.

This demonstrates how digital signatures help detect modification.

---

# 📊 5. Create a Secure Communication Report

This is the most important final task.

You are going to combine your observations into a professional security report.

Your report should explain:

```text
HTTPS
 ↓
TLS
 ↓
Certificate
 ↓
Authentication
 ↓
Key Establishment
 ↓
Encrypted Communication
 ↓
Security Analysis
```

---

# 📄 Secure Communication Report Structure

## 1. Title

```text
Secure Communication Analysis — HTTPS/TLS
```

---

## 2. Date

Record when you performed the investigation.

---

## 3. Target

Use a website or lab system that you are authorized to inspect.

Example:

```text
Target:
example.com
```

For your own lab, you can use:

```text
localhost
```

or another system you control.

---

# 4. Objective

Explain what you investigated.

Example:

```text
The objective of this investigation was to analyze
HTTPS communication, inspect the TLS certificate,
observe TLS traffic, and understand the cryptographic
mechanisms used to protect communication.
```

---

# 5. Tools Used

Example:

```text
- Web Browser
- Wireshark
- OpenSSL
- Python
- cryptography library
- Linux/Kali terminal
```

---

# 6. HTTPS Analysis

Document:

```text
Protocol:
HTTPS

TLS Version:
[Your observation]

Cipher:
[Your observation]

Connection:
Encrypted
```

Explain what you observed.

---

# 7. Certificate Investigation

Record:

```text
Subject:
Issuer:
Valid From:
Valid Until:
Public Key Algorithm:
Signature Algorithm:
SAN:
Certificate Chain:
```

Then explain the significance of the certificate.

---

# 8. TLS Traffic Analysis

Document what you observed in Wireshark.

Example:

```text
Observed:
ClientHello
ServerHello
Certificate
CertificateVerify
Finished
Encrypted Application Data
```

Explain the purpose of each relevant message.

---

# 9. Hashing Experiment

Document:

```text
Original File:
message.txt

Algorithm:
SHA-256

Original Hash:
[Your hash]

Modified File:
message.txt

Modified Hash:
[Your hash]
```

Then explain how changing the input affected the digest.

---

# 10. Encryption Experiment

Document:

```text
Plaintext:
Cybersecurity training

Encryption:
Symmetric encryption

Ciphertext:
[Output]

Decrypted text:
Cybersecurity training
```

Explain:

```text
Plaintext → Encryption → Ciphertext
Ciphertext → Decryption → Plaintext
```

---

# 11. Security Properties

Create a table:

| Mechanism                 | Security Purpose                    |
| ------------------------- | ----------------------------------- |
| TLS                       | Secure communication                |
| Certificate               | Server identity binding             |
| Key agreement             | Establish session secrets           |
| Symmetric encryption      | Confidentiality                     |
| AEAD/integrity protection | Detect tampering                    |
| Hashing                   | Integrity/fingerprinting            |
| Digital signature         | Authentication evidence + integrity |

---

# 12. Security Findings

Record what you actually observed.

Example:

```text
Finding 1:
The connection used HTTPS.

Finding 2:
A valid certificate was presented for the target hostname.

Finding 3:
TLS encrypted application traffic was observed.

Finding 4:
Changing a file changed its SHA-256 digest.

Finding 5:
A modified message failed digital signature verification.
```

Do not claim something was verified if you did not actually test it.

---

# 13. Limitations

A professional report should mention limitations.

For example:

```text
- Only a limited amount of traffic was analyzed.
- Browser behavior may hide some TLS details.
- Certificate inspection does not prove that an entire website is trustworthy.
- The experiment was performed in a controlled environment.
- Results represent the observed connection at the time of testing.
```

---

# 14. Conclusion

Summarize what you learned.

Example:

```text
The practical investigation demonstrated how cryptography
is used in secure communications. HTTPS uses TLS to establish
a protected communication channel. Certificates support
server authentication, key establishment enables session
secrets, and symmetric cryptography protects application data.

Hashing experiments demonstrated how small input changes
produce different cryptographic digests. Digital signature
testing demonstrated how signatures can detect message
modification and provide evidence of possession of the
corresponding private key.
```

---

# 🧪 Final Mini Project

Now combine everything.

## 🔐 Cryptography Practical Investigation

Build a small project:

```text
cryptography-hands-on/
│
├── README.md
│
├── https-analysis/
│   └── report.md
│
├── certificate-analysis/
│   └── certificate-report.md
│
├── hashing/
│   ├── hash_demo.py
│   └── report.md
│
├── encryption/
│   ├── encryption_demo.py
│   └── report.md
│
└── secure-communication/
    └── final-report.md
```

---

# 📁 Project 1 — HTTPS Analysis

Perform:

```text
Browser
   ↓
HTTPS website
   ↓
Inspect connection
   ↓
Inspect TLS traffic
   ↓
Document findings
```

---

# 📁 Project 2 — Certificate Investigation

Perform:

```text
Website
   ↓
Certificate
   ↓
OpenSSL / Browser
   ↓
Inspect fields
   ↓
Analyze certificate chain
   ↓
Document findings
```

---

# 📁 Project 3 — Hashing

Build:

```text
hash_demo.py
```

It should:

1. Read a file
2. Calculate SHA-256
3. Display the hash
4. Modify the file
5. Calculate the hash again
6. Compare the results

---

# 📁 Project 4 — Encryption

Build:

```text
encryption_demo.py
```

It should demonstrate:

```text
Generate Key
     ↓
Encrypt Message
     ↓
Display Ciphertext
     ↓
Decrypt Message
     ↓
Display Plaintext
```

---

# 📁 Project 5 — Secure Communication Report

Combine your findings:

```text
HTTPS
+
Certificate
+
TLS
+
Hashing
+
Encryption
+
Digital Signatures
        ↓
Secure Communication Report
```

---

# 🛡️ Security Rules for Your Practical Work

Always follow these rules:

### 1. Use authorized targets

Only inspect systems you own or have explicit permission to test.

### 2. Don't collect sensitive data unnecessarily

Avoid capturing:

* Passwords
* Session cookies
* Authentication tokens
* Private keys
* Personal information

### 3. Use your own lab whenever possible

A controlled environment is ideal for learning.

### 4. Don't modify systems you don't control

Your goal is:

```text
Observe → Analyze → Document
```

not:

```text
Attack → Damage → Disrupt
```

### 5. Keep evidence

Save:

* Screenshots
* Command output
* Hash values
* Certificate information
* Wireshark observations
* Python output

These make your report reproducible.

---

# 🧠 Cryptography Practical Workflow

Remember:

```text
OBSERVE
   ↓
ANALYZE
   ↓
VERIFY
   ↓
EXPERIMENT
   ↓
DOCUMENT
```

### 🌐 HTTPS

Observe secure traffic.

### 📜 Certificates

Investigate identity and trust.

### #️⃣ Hashing

Experiment with integrity and fingerprints.

### 🔐 Encryption

Demonstrate confidentiality.

### 📄 Reporting

Document evidence and conclusions.

---

# ⚡ Quick Revision

| Task                         | Main Concept                        |
| ---------------------------- | ----------------------------------- |
| Analyze HTTPS traffic        | TLS                                 |
| Investigate certificates     | PKI / Trust                         |
| Explore hashing workflows    | Integrity / Fingerprinting          |
| Demonstrate encryption       | Confidentiality                     |
| Digital signature experiment | Authentication evidence + Integrity |
| Create security report       | Security analysis + Documentation   |

---

# 🧠 Complete Cryptography Module

You have now connected the entire module:

```text
1. Cryptography Fundamentals
             ↓
2. Encryption
             ↓
3. Symmetric Encryption
             ↓
4. Asymmetric Encryption
             ↓
5. Hashing
             ↓
6. Digital Signatures
             ↓
7. Secure Communications
             ↓
8. Modern Communication Security
             ↓
9. AI-Assisted Security Analysis
             ↓
10. Hands-on Tasks
             ↓
       🔐 CRYPTOGRAPHY
```

---

# 🏁 Final Takeaway

Cryptography is not just about knowing algorithms.

As a cybersecurity professional, you should be able to:

> 🔐 **Understand** how cryptographic mechanisms work
> 🔎 **Observe** them in real systems
> 🧪 **Experiment** with them safely
> ✅ **Verify** their behavior
> 📊 **Analyze** their security properties
> 📝 **Document** your findings

### The ultimate workflow:

**Understand → Observe → Analyze → Verify → Test → Report**

That is how cryptography knowledge becomes practical cybersecurity skill.

This completes **Cryptography Module 10 — Apply Through Hands-on Tasks**, and therefore the **Cryptography module as a whole**. The natural next step is to move to the next module in your Brototype roadmap and continue the same one-topic-at-a-time approach.
