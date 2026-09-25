# 🔐 1. Understand Cryptography Fundamentals

> **Cryptography is the foundation of secure communication, protecting information from unauthorized access, modification, and impersonation.**

---

# 🔑 1. What is Cryptography?

## 📌 Definition

**Cryptography** is the practice of protecting information by transforming it into a form that unauthorized people cannot understand or use.

The word comes from:

```text
Crypto = Hidden / Secret
Graphy = Writing
```

In cybersecurity, cryptography is used to protect:

* Passwords
* Messages
* Files
* Network communication
* Financial transactions
* Authentication credentials
* Digital communications
* Sensitive databases

### Simple Example

Suppose you send:

```text
Hello Farhan
```

Without protection, someone intercepting the communication may be able to read it.

Cryptography can transform the data into something like:

```text
X7@kP9#mL2...
```

The protected data is called **ciphertext**.

Only someone with the appropriate cryptographic mechanism/key should be able to recover the original information.

```text
Plaintext
   │
   │ Encryption
   ▼
Ciphertext
   │
   │ Decryption
   ▼
Plaintext
```

### Important Terms

| Term       | Meaning                                        |
| ---------- | ---------------------------------------------- |
| Plaintext  | Original readable data                         |
| Ciphertext | Transformed/protected data                     |
| Encryption | Converting plaintext into ciphertext           |
| Decryption | Recovering plaintext from ciphertext           |
| Key        | Secret/value used by a cryptographic algorithm |
| Cipher     | Algorithm used for encryption/decryption       |

---

# 🛡️ 2. Security Goals of Cryptography

Cryptography is not simply about **hiding information**.

It supports several important security goals.

The major goals are:

```text
        CRYPTOGRAPHY
             │
     ┌───────┼────────┐
     ↓       ↓        ↓
Confidentiality Integrity Authentication
```

These goals address different security problems.

| Goal            | Protects Against                   |
| --------------- | ---------------------------------- |
| Confidentiality | Unauthorized reading               |
| Integrity       | Unauthorized modification          |
| Authentication  | Impersonation / verifying identity |

These concepts are closely related to the **CIA Triad** you learned earlier.

```text
CIA TRIAD
│
├── Confidentiality
├── Integrity
└── Availability
```

Cryptography directly provides strong mechanisms for **Confidentiality** and **Integrity**, and supports **Authentication**.

---

# 🔒 3. Confidentiality

## 📌 What is Confidentiality?

**Confidentiality means preventing unauthorized people from accessing or reading information.**

Example:

You send your password to a website.

You don't want someone monitoring the network to see:

```text
username: farhan
password: mypassword123
```

Encryption helps protect the communication.

```text
Your Device
     │
     │ Plaintext
     ▼
Encryption
     │
     │ Ciphertext
     ▼
Network
     │
     ▼
Decryption
     │
     ▼
Website
```

An attacker intercepting the encrypted communication should not be able to understand the protected data without the required cryptographic key/material.

---

## 🌐 Real-World Example: HTTPS

When you visit:

```text
https://example.com
```

HTTPS uses cryptographic protocols to protect communication between your browser and the server.

Instead of sending sensitive information as ordinary readable network traffic, the connection uses encryption.

This helps protect things such as:

* Login credentials
* Personal information
* Payment information
* Session data

### HTTP vs HTTPS

| HTTP                                                    | HTTPS                                   |
| ------------------------------------------------------- | --------------------------------------- |
| Communication is not protected by TLS encryption        | Uses TLS                                |
| Data can potentially be observed or modified in transit | Provides protection for data in transit |
| No cryptographic server authentication through TLS      | TLS can authenticate the server         |
| Less suitable for sensitive communication               | Standard for modern websites            |

> **HTTPS ≠ "the website is safe."**

HTTPS protects the communication channel, but it does not guarantee that the website itself is trustworthy.

---

# 🧬 4. Integrity

## 📌 What is Integrity?

**Integrity means ensuring that information has not been modified in an unauthorized or unexpected way.**

Imagine downloading:

```text
security_tool.zip
```

You expect the original file.

An attacker modifies the file and inserts malicious code.

Now you have:

```text
Original File
     ↓
Modified File
```

How can you determine whether the file changed?

One important technique is using a **cryptographic hash**.

---

# 🔢 Hashing

A cryptographic hash function converts data into a fixed-length value called a **hash** or **digest**.

Example:

```text
File
 │
 ▼
SHA-256
 │
 ▼
256-bit hash
```

A simplified example:

```text
hello
 ↓
SHA-256
 ↓
2cf24dba5fb0a30e...
```

If the input changes:

```text
hello
```

becomes:

```text
Hello
```

the resulting hash changes dramatically.

```text
hello
 ↓
Hash A

Hello
 ↓
Hash B

Hash A ≠ Hash B
```

This property makes hashes useful for detecting changes.

---

## 🔐 Important Hash Properties

A cryptographic hash function should generally provide:

### 1. Deterministic Output

Same input:

```text
hello → Hash A
hello → Hash A
```

### 2. Fixed-Length Output

For SHA-256:

```text
Input: any reasonable size
Output: 256 bits
```

### 3. Avalanche Effect

A tiny input change should produce a substantially different digest.

```text
Input A → Hash A
Input B → Completely different Hash
```

### 4. One-Way Property

A secure cryptographic hash should make it computationally infeasible to recover the original input from the digest alone.

---

# ⚠️ Hashing ≠ Encryption

This is extremely important.

| Hashing                                              | Encryption                |
| ---------------------------------------------------- | ------------------------- |
| One-way transformation                               | Reversible transformation |
| Usually no decryption process                        | Can be decrypted          |
| Used for integrity and password-related applications | Used for confidentiality  |
| Produces a digest                                    | Produces ciphertext       |
| Example: SHA-256                                     | Examples: AES, RSA        |

Don't say:

> "SHA-256 encrypts the file."

More accurately:

> "SHA-256 hashes the data."

---

# 🔑 5. Authentication

## 📌 What is Authentication?

**Authentication is the process of verifying that an entity is who or what it claims to be.**

For example, when you log into an account:

```text
Username
   +
Password
   ↓
Authentication
   ↓
Access Granted / Denied
```

The system asks:

> **"Who are you, and can you prove it?"**

---

# 🪪 Authentication Factors

Authentication can use different types of evidence.

### Something You Know

Examples:

* Password
* PIN
* Security question

### Something You Have

Examples:

* Security token
* Smartphone
* Hardware security key
* Smart card

### Something You Are

Examples:

* Fingerprint
* Face recognition
* Iris characteristics

---

# 🔐 Multi-Factor Authentication

Using multiple independent authentication factors provides stronger protection.

For example:

```text
Password
   +
Authenticator App Code
   ↓
Authentication
```

This is commonly called **MFA — Multi-Factor Authentication**.

The important concept is:

> **Two passwords are not necessarily two different authentication factors.**

For example:

```text
Password + PIN
```

may both be knowledge factors.

Whereas:

```text
Password + Security Key
```

uses two different factor categories.

---

# 🔏 How Cryptography Supports Authentication

Cryptography plays an important role in authentication.

Examples include:

### Password Protection

Systems should not normally store user passwords as plaintext.

Instead, passwords should be processed using a password hashing scheme designed for this purpose, typically with:

* Salt
* Password hashing function
* Appropriate work factor

Common password hashing algorithms include:

* Argon2
* bcrypt
* scrypt

> **SHA-256 alone is generally not the appropriate choice for storing passwords.**

---

## 🔐 Digital Signatures

Cryptography can also provide authentication through **digital signatures**.

Simplified process:

```text
Message
   │
   ▼
Hash
   │
   ▼
Digital Signature
   │
   ▼
Receiver
   │
   ▼
Verify Signature
```

Digital signatures can help establish:

* Who signed the data
* Whether the data was modified
* Whether the signature corresponds to the claimed public key

This connects **authentication + integrity**.

---

# 🔄 Putting Everything Together

Consider an online banking transaction.

You enter:

```text
Transfer ₹5,000
```

Different security goals are involved.

### Confidentiality

Protect the communication from unauthorized reading.

```text
Encryption
   ↓
Protect transaction data
```

### Integrity

Detect unauthorized modification.

```text
Cryptographic mechanisms
   ↓
Detect changes
```

### Authentication

Verify the identity of the user/server.

```text
Credentials / MFA / Certificates
   ↓
Identity verification
```

So:

```text
              SECURE COMMUNICATION
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
Confidentiality   Integrity   Authentication
       │              │              │
   Hide data      Detect change   Verify identity
       │              │              │
   Encryption       Hashes       Certificates/
                                 MFA/Signatures
```

---

# 🧠 Important Differences

| Concept         | Main Question    | Common Mechanism                         |
| --------------- | ---------------- | ---------------------------------------- |
| Confidentiality | Who can read it? | Encryption                               |
| Integrity       | Was it changed?  | Hashes / MACs / signatures               |
| Authentication  | Who are you?     | Passwords, MFA, certificates, signatures |

### Easy Memory Trick

```text
CONFIDENTIALITY → HIDE
INTEGRITY       → PROTECT FROM CHANGE
AUTHENTICATION  → VERIFY IDENTITY
```

Or:

> 🔒 **Confidentiality = Secret**
> 🧬 **Integrity = Unchanged**
> 🪪 **Authentication = Identity**

---

# ⚠️ Common Misunderstandings

### ❌ "Encryption provides everything."

Not necessarily.

Encryption primarily addresses confidentiality, while integrity and authentication may require additional cryptographic mechanisms.

### ❌ "Hashing is encryption."

No.

```text
Encryption → reversible with appropriate key
Hashing    → designed as one-way
```

### ❌ "HTTPS means the website is trustworthy."

No.

HTTPS helps secure communication with the server. It doesn't guarantee the site's content, business practices, or intentions are trustworthy.

### ❌ "Authentication means authorization."

They are different.

```text
Authentication
      ↓
Who are you?

Authorization
      ↓
What are you allowed to do?
```

Example:

```text
Login
 ↓
Authentication

Access admin panel
 ↓
Authorization
```

---

# 🌍 Real-World Cybersecurity Examples

| Situation                  | Cryptographic Goal                           |
| -------------------------- | -------------------------------------------- |
| HTTPS connection           | Confidentiality + Integrity + Authentication |
| File hash verification     | Integrity                                    |
| Password authentication    | Authentication                               |
| MFA security key           | Authentication                               |
| Digital signature          | Integrity + Authentication                   |
| Encrypted database backup  | Confidentiality                              |
| Software package signature | Integrity + Authentication                   |
| Secure messaging           | Confidentiality + Integrity + Authentication |

---

# 🧪 Mini Practice

Try answering these without looking above.

### Question 1

You want to prevent an attacker from reading network traffic.

**Which security goal?**

---

### Question 2

You download a file and calculate its SHA-256 hash to compare with a trusted value.

**Which security property are you checking?**

---

### Question 3

A website asks for your password and authenticator-app code.

**What security concept is being used?**

---

### Question 4

Is SHA-256 encryption or hashing?

---

### Question 5

What is the difference between:

```text
Authentication
Authorization
```

---

# ⚡ Quick Revision

```text
Cryptography
     │
     ├── Confidentiality
     │       └── Prevent unauthorized reading
     │
     ├── Integrity
     │       └── Detect unauthorized modification
     │
     └── Authentication
             └── Verify identity
```

### Core mechanisms

```text
Encryption
    ↓
Confidentiality

Hashing / MAC / Digital Signature
    ↓
Integrity

Passwords / MFA / Certificates / Digital Signatures
    ↓
Authentication
```

---

# 🧠 Final Takeaway

Cryptography is much more than **"making data secret."**

It provides mechanisms that help answer three fundamental security questions:

> 🔒 **Confidentiality — Can unauthorized people read the information?**

> 🧬 **Integrity — Has the information been changed?**

> 🪪 **Authentication — Can we verify the identity of the entity involved?**

These concepts form the foundation for understanding the next cryptography topics such as **encryption, symmetric cryptography, asymmetric cryptography, hashing, digital signatures, certificates, and TLS**.

This gives you the foundation. The next topic can build directly on this by explaining **symmetric vs asymmetric encryption**, including how AES, RSA, and public/private keys actually work.
