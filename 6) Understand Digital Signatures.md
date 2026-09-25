# ✍️ 6. Understand Digital Signatures

> **Digital signatures use cryptography to provide evidence that data was signed by a particular private key and that the signed data has not been altered.**
>
> They are a major part of **HTTPS, software signing, secure documents, certificates, APIs, email security, and digital transactions**.

---

# 🧠 1. What is a Digital Signature?

A **digital signature** is a cryptographic mechanism used to provide:

* 🔐 **Authenticity** — evidence of who signed the data
* 🛡️ **Integrity** — evidence that the signed data was not modified
* ✍️ **Non-repudiation** — cryptographic evidence that can support accountability for a signature

Digital signatures are based primarily on **asymmetric cryptography**.

A signer has:

```text
🔑 Private Key
🔓 Public Key
```

The basic idea is:

```text
Message
   │
   ▼
Hash Function
   │
   ▼
Message Digest
   │
   ▼
Private Key + Signature Algorithm
   │
   ▼
Digital Signature
```

The recipient can then use the corresponding **public key** to verify the signature.

---

# 🔑 2. Public Key and Private Key

Digital signatures use a key pair.

### 🔒 Private Key

The private key:

* Must remain secret
* Is controlled by the signer
* Is used to create signatures

```text
PRIVATE KEY
    │
    └──→ Create Signature
```

### 🔓 Public Key

The public key:

* Can be shared
* Is used to verify signatures
* Corresponds to the private key

```text
PUBLIC KEY
    │
    └──→ Verify Signature
```

### Memory Trick

```text
🔒 Private → Sign
🔓 Public  → Verify
```

> **Never share your private key.**

If someone obtains it, they may be able to create signatures that appear to come from the key owner.

---

# 🧩 3. Digital Signature vs Electronic Signature

These terms are related but not identical.

### Electronic Signature

An electronic signature can be a broad concept involving an electronic indication of agreement or approval.

Examples:

```text
Typed name
Clicking "I Agree"
Drawing a signature
Electronic approval
```

### Digital Signature

A digital signature specifically uses **cryptographic mechanisms** involving keys and signature verification.

```text
Electronic Signature
        │
        └── Broad concept

Digital Signature
        │
        └── Cryptographic implementation
```

So:

> **Every digital signature is an electronic form of signing, but not every electronic signature is a cryptographic digital signature.**

---

# 🪪 4. Identity Verification

One major purpose of digital signatures is to provide evidence that a message or document was signed using a particular private key.

Consider:

```text
Alice
 │
 │ Private Key
 ▼
Digital Signature
 │
 ▼
Message
```

Bob receives:

```text
Message
+
Signature
+
Alice's Public Key
```

Bob verifies the signature.

If verification succeeds, Bob knows:

```text
✅ The signature corresponds to the public/private key pair
```

However, there is an important distinction:

> **A valid signature proves control of the corresponding private key; it does not automatically prove the real-world identity of the person behind that key.**

For identity binding, systems such as **digital certificates and trusted certificate authorities** may be used.

---

# 🔐 5. Authentication Through Digital Signatures

Digital signatures can support authentication.

Example:

```text
Client
   │
   │ Request + Signature
   ▼
Server
   │
   │ Verify Signature
   ▼
Authentication Decision
```

The server verifies that the signature was created using the private key corresponding to a trusted public key.

This can be used in:

* 🔑 SSH authentication
* 🔐 TLS
* 🌐 Secure APIs
* 📧 Secure email
* 💻 Software signing
* 🪪 Digital certificates

---

# 🛡️ 6. Integrity

Digital signatures also help detect modifications.

Suppose Alice signs:

```text
Transfer ₹10,000 to Bob
```

The signature is associated with the original data.

If someone changes it to:

```text
Transfer ₹90,000 to Bob
```

the signature verification should fail.

Conceptually:

```text
Original Message
      │
      ▼
     Hash
      │
      ▼
   Signature
```

After modification:

```text
Modified Message
      │
      ▼
Different Hash
      │
      ▼
❌ Signature verification fails
```

Therefore, digital signatures provide **integrity protection**.

---

# ✍️ 7. Signature Workflow

A simplified digital-signature workflow looks like this:

```text
                 SIGNING

      Original Message
             │
             ▼
       Hash Function
             │
             ▼
        Message Hash
             │
             ▼
      Sign with Private Key
             │
             ▼
       Digital Signature
```

The sender normally sends:

```text
┌──────────────────────────┐
│ Original Message         │
│ Digital Signature        │
│ Public Key / Certificate │
└──────────────────────────┘
```

The recipient then verifies the signature.

---

# 🔄 8. Complete Signing Example

Imagine Alice wants to sign:

```text
"Transfer 1000 to Bob"
```

### Step 1 — Create message

```text
Message
   ↓
"Transfer 1000 to Bob"
```

### Step 2 — Calculate hash

```text
Message
   ↓
SHA-256
   ↓
Message Digest
```

### Step 3 — Create signature

The signing algorithm uses Alice's private key:

```text
Message Digest
      +
Alice's Private Key
      ↓
Digital Signature
```

### Step 4 — Send

Alice sends:

```text
Message
+
Digital Signature
```

along with whatever public-key/certificate information the verification system requires.

---

# 🔍 9. Verification Process

Bob receives:

```text
Message
+
Digital Signature
```

The verification process conceptually involves checking the signature against the message using Alice's public key.

```text
              VERIFICATION

Received Message
       │
       ▼
   Hash Message
       │
       ▼
    Hash A
       
Signature
       │
       ▼
Verify with Public Key
       │
       ▼
    Hash B
       
Hash A == Hash B ?
       │
   ┌───┴───┐
  YES      NO
   │        │
   ▼        ▼
Valid     Invalid
```

If verification succeeds:

```text
✅ Signature Valid
```

If verification fails:

```text
❌ Signature Invalid
```

---

# ⚠️ 10. What Can Cause Verification Failure?

A signature may fail verification if:

### 1. Message was modified

```text
Original → Modified
```

The hash changes.

---

### 2. Wrong public key

The verifier may be using a public key that doesn't correspond to the signing private key.

---

### 3. Invalid signature

The signature itself may be corrupted or forged.

---

### 4. Wrong algorithm or parameters

The verifier must use compatible signature algorithms and parameters.

---

### 5. Certificate/trust problem

The cryptographic signature may mathematically verify, but the public key may not be trusted or may not be valid for the claimed identity.

---

# ⚖️ 11. Non-Repudiation

**Non-repudiation** refers to mechanisms that provide evidence supporting accountability for an action or signature.

In digital-signature systems:

```text
Private Key
     │
     ▼
Digital Signature
     │
     ▼
Signed Data
```

If the private key is properly controlled, the signature can provide evidence that the corresponding key was used to sign the data.

However, non-repudiation is **not simply a mathematical property of a signature**.

It depends on the wider system, including:

* Private-key protection
* Identity verification
* Certificate management
* Audit logs
* Signature policies
* Legal/regulatory requirements
* Evidence of who controlled the key

### Important distinction

```text
Digital Signature
        ↓
Cryptographic evidence
        ↓
Can support non-repudiation
```

It does **not automatically mean**:

```text
Valid Signature = Absolute proof of a person's identity
```

---

# 🔐 12. Protecting the Private Key

Non-repudiation and authentication depend heavily on private-key security.

If Alice's private key is stolen:

```text
Alice's Private Key
       │
       ▼
Attacker obtains it
       │
       ▼
Attacker can potentially create
valid signatures
```

This creates a serious security problem.

Private keys should therefore be protected using appropriate controls such as:

* 🔒 Strong access controls
* 💾 Encrypted storage
* 🔐 Hardware security modules
* 🪪 Smart cards/security tokens
* 🔑 Hardware-backed key stores
* 🔄 Key rotation/replacement where appropriate
* 🚫 Never committing private keys to Git

---

# 🏛️ 13. Trust Models

A digital signature tells us that a particular key was used.

But how do we know **whose key it is**?

This is the problem of **digital trust**.

Suppose you receive:

```text
Public Key:
ABC123...
```

How do you know:

```text
ABC123... = Alice
```

rather than:

```text
ABC123... = Attacker
```

Trust models solve this problem.

---

# 🌐 14. Certificate Authority Model

One major trust model used on the Internet is the **Public Key Infrastructure (PKI)** model.

A simplified structure is:

```text
              Root CA
                 │
                 ▼
        Intermediate CA
                 │
                 ▼
        Digital Certificate
                 │
                 ▼
         Public Key + Identity
```

### Certificate Authority

A **Certificate Authority (CA)** is a trusted entity that issues and signs digital certificates.

A certificate can bind information such as:

```text
Identity / Domain
       +
Public Key
       +
Certificate Information
       +
CA Signature
```

---

# 🔗 15. Certificate Chain of Trust

Trust can be hierarchical.

Example:

```text
Root CA
  │
  └── Intermediate CA
          │
          └── Server Certificate
                  │
                  └── example.com
```

A browser can validate the certificate chain back to a trusted root.

Conceptually:

```text
Server Certificate
       ↓
Intermediate CA
       ↓
Trusted Root CA
       ↓
Browser Trust Store
```

If the chain and other certificate checks are valid, the public key can be associated with the certified identity.

---

# 🌐 16. Digital Signatures in HTTPS

HTTPS is one of the most important real-world examples.

When connecting to a secure website:

```text
Browser
   │
   ▼
Web Server
   │
   ▼
Server Certificate
   │
   ▼
Certificate Validation
   │
   ▼
Key Establishment
   │
   ▼
Secure Session
```

Certificates and digital signatures help establish trust during TLS.

After the secure session is established, symmetric cryptography is generally used to protect the actual application data efficiently.

### This connects your previous topics:

```text
Asymmetric Cryptography
        │
        ├── Certificates
        │
        ├── Digital Signatures
        │
        └── Key Establishment
                 │
                 ▼
          Symmetric Encryption
                 │
                 ▼
          Application Data
```

---

# 🧩 17. Trust Models Beyond PKI

Different systems can use different trust models.

### 1. Hierarchical Trust

Used heavily by Internet PKI.

```text
Root
 ↓
Intermediate
 ↓
Certificate
 ↓
Identity
```

---

### 2. Web of Trust

Associated with systems such as **OpenPGP**.

Instead of depending entirely on a central CA hierarchy, users can establish trust relationships by signing each other's keys.

Conceptually:

```text
Alice ───── trusts/signs ───── Bob
  │                            │
  └──────────────┬─────────────┘
                 ▼
              Charlie
```

Trust can therefore be distributed across users.

---

### 3. Direct Trust

A system can explicitly trust a particular public key.

Example:

```text
Known Public Key
      ↓
Trusted Directly
```

SSH known-host mechanisms are an example of a system where specific keys can be trusted directly.

---

# 🆚 18. Trust Model Comparison

| Model            | Main Idea                           | Example         |
| ---------------- | ----------------------------------- | --------------- |
| Hierarchical PKI | CA chain establishes trust          | HTTPS/TLS       |
| Web of Trust     | Users establish trust relationships | OpenPGP         |
| Direct Trust     | Specific key is trusted directly    | SSH known hosts |

---

# 🛠️ 19. Practical Applications

Digital signatures are used in many security systems.

### 🌐 HTTPS / TLS

Certificates and signatures help establish server identity and secure communication.

---

### 💻 Software Signing

Software developers can sign applications or packages.

```text
Software
   ↓
Hash
   ↓
Developer's Private Key
   ↓
Digital Signature
```

The operating system or user can verify:

```text
Is the software signature valid?
```

This helps detect tampering and supports publisher verification.

---

### 📧 Secure Email

Systems such as S/MIME and OpenPGP can use digital signatures to provide:

```text
Authenticity
+
Integrity
```

for email messages.

---

### 🔐 SSH

SSH can use public-key cryptography for authentication.

```text
Client
  │
  │ Proves possession of private key
  ▼
SSH Server
  │
  ▼
Authentication
```

---

### 📄 Digital Documents

Digital signatures can be applied to electronic documents to provide evidence of signing and detect modifications.

---

### 🌐 APIs

Applications can sign requests to demonstrate possession of a private key.

```text
API Request
    ↓
Hash / Canonicalization
    ↓
Private Key
    ↓
Signature
    ↓
Server Verification
```

---

# 🧠 20. Digital Signature vs Encryption

These concepts are often confused.

| Feature                | Digital Signature        | Encryption                                            |
| ---------------------- | ------------------------ | ----------------------------------------------------- |
| Main goal              | Authenticity + integrity | Confidentiality                                       |
| Private key            | Used for signing         | May be used in some asymmetric schemes for decryption |
| Public key             | Used for verification    | Commonly used for encryption/key establishment        |
| Hides message?         | ❌ No                     | ✅ Yes                                                 |
| Detects modification?  | ✅                        | Depends on encryption mode/protocol                   |
| Proves key possession? | ✅                        | Not necessarily                                       |
| Example                | Software signature       | AES/TLS session encryption                            |

### Memory Trick

```text
✍️ Signature → "Did this come from the expected key, and was it changed?"
🔐 Encryption → "Can unauthorized people read it?"
```

---

# 🔄 21. Digital Signature + Hashing + Asymmetric Cryptography

These three topics work together.

```text
                 DIGITAL SIGNATURE
                        │
          ┌─────────────┴─────────────┐
          ▼                           ▼
      Hashing                  Asymmetric Crypto
          │                           │
          ▼                           ▼
    Message Digest             Private/Public Key
          │                           │
          └─────────────┬─────────────┘
                        ▼
                 Digital Signature
```

This is why understanding the previous topics was important.

---

# 🐍 22. Digital Signature with Python

Python's `cryptography` library can generate keys and create signatures.

Example using RSA:

```python
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes

private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)

public_key = private_key.public_key()

message = b"Hello Cybersecurity"

signature = private_key.sign(
    message,
    padding.PSS(
        mgf=padding.MGF1(hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)

print("Signature created")
```

The public key can then be used to verify the signature.

```python
public_key.verify(
    signature,
    message,
    padding.PSS(
        mgf=padding.MGF1(hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)

print("Signature verified")
```

If the message is changed:

```python
message = b"Modified message"
```

verification should fail.

---

# ⚠️ 23. Common Mistakes

### ❌ Mistake 1 — Sharing the private key

```text
Private Key = Secret
```

Never treat it like a public certificate.

---

### ❌ Mistake 2 — Thinking a public key identifies someone automatically

A public key only becomes associated with an identity through a trust mechanism such as:

```text
Certificate
CA
Known key
Trusted fingerprint
Web of trust
```

---

### ❌ Mistake 3 — Thinking signatures provide confidentiality

A signature does not hide the message.

```text
Signature → Authenticity + Integrity
Encryption → Confidentiality
```

You may use both together.

---

### ❌ Mistake 4 — Ignoring private-key compromise

If a private signing key is compromised, attackers may be able to create valid-looking signatures.

Key compromise requires appropriate incident response, such as revocation/replacement depending on the system.

---

### ❌ Mistake 5 — Trusting every certificate automatically

A certificate being present does not mean you should ignore:

* Certificate validity
* Hostname/domain matching
* Certificate chain
* Expiration
* Revocation/status mechanisms
* Trust store
* TLS configuration

---

# 🧪 24. Mini Practical Exercise

You can experiment with a simple signing workflow.

### Step 1 — Install the library

```bash
pip install cryptography
```

### Step 2 — Generate a key pair

```text
Private Key
     +
Public Key
```

### Step 3 — Sign a message

```text
"Cybersecurity"
```

### Step 4 — Verify it

```text
Original Message
      +
Signature
      +
Public Key
      ↓
Verification
```

### Step 5 — Modify the message

Change:

```text
Cybersecurity
```

to:

```text
Cybersecurity!
```

Run verification again.

Expected result:

```text
Original message → ✅ Valid
Modified message → ❌ Invalid
```

This demonstrates how digital signatures provide integrity protection.

---

# 🧠 25. Quick Revision

### What is a digital signature?

A cryptographic mechanism that provides evidence of authenticity and integrity using asymmetric cryptography.

### Which key creates the signature?

```text
🔒 Private Key
```

### Which key verifies it?

```text
🔓 Public Key
```

### Does a signature encrypt the message?

```text
❌ No
```

### What happens if the message changes?

```text
Signature verification → ❌ Fails
```

### What is non-repudiation?

Evidence supporting accountability for a signed action, depending on key control, identity systems, records, and applicable policies/laws.

### What does a CA do?

A Certificate Authority can issue certificates that bind public keys to identities under a trust model.

### What is PKI?

**Public Key Infrastructure** is the broader system of certificates, keys, CAs, policies, validation, and trust relationships used to manage public-key trust.

---

# 🧠 Memory Map

```text
                 ✍️ DIGITAL SIGNATURES
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
   🔒 Private Key    🔓 Public Key      🔐 Hash
        │                │                │
        ▼                ▼                ▼
      Sign            Verify          Integrity
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                  Authentication
                         │
                         ▼
                   Digital Trust
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
            PKI      Web of Trust  Direct Trust
```

### Core memory trick

```text
🔒 PRIVATE → SIGN
🔓 PUBLIC  → VERIFY

HASH → Detect Changes
SIGNATURE → Prove Key Possession + Integrity
CERTIFICATE → Bind Key to Identity
CA → Establish Trust
```

---

# 🎯 Final Takeaway

Digital signatures are a bridge between **cryptography and digital trust**.

The complete concept can be remembered as:

```text
                 MESSAGE
                    │
                    ▼
                  HASH
                    │
                    ▼
            Sign with Private Key
                    │
                    ▼
            DIGITAL SIGNATURE
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
      Integrity          Key Possession
          │                   │
          └─────────┬─────────┘
                    ▼
             Authentication
                    │
                    ▼
               TRUST MODEL
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
         PKI            Web of Trust
```

### Remember these 5 points:

> 🔹 **Private key → creates the signature**
> 🔹 **Public key → verifies the signature**
> 🔹 **Hashing → helps detect modification**
> 🔹 **Certificates → connect public keys with identities**
> 🔹 **Trust models → determine why a public key should be trusted**

**The big picture:**

```text
🔐 Encryption      → Confidentiality
🔎 Hashing         → Integrity / Identification
✍️ Digital Signatures → Authenticity / Integrity
🏛️ PKI             → Digital Trust
```

That completes **Cryptography Topic 6: Digital Signatures**. Your next topic can build directly on this by going into **certificates, PKI, or whatever Topic 7 contains in your Brototype syllabus**.
