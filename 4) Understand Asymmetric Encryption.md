# 🔐 4. Understand Asymmetric Encryption

> **Asymmetric cryptography uses a pair of mathematically related keys — a public key and a private key — to enable secure communication, authentication, digital signatures, and key establishment.**

---

# 🔑 1. Public Keys

## 📌 What is a Public Key?

A **public key** is one part of an asymmetric key pair that can generally be shared with others.

An asymmetric key pair contains:

```text id="0v4w5p"
🔓 Public Key
🔐 Private Key
```

The public key is designed to be distributed.

The private key must be protected.

```text id="3j6z1f"
        KEY PAIR
           │
      ┌────┴────┐
      ↓         ↓
 Public Key  Private Key
  Shareable    Secret
```

---

# 🌐 Example

Imagine Bob creates an asymmetric key pair:

```text id="2u4r0y"
Bob's Public Key
Bob's Private Key
```

Bob can give his public key to Alice:

```text id="x0j8va"
Bob
 │
 │ Public Key
 ▼
Alice
```

Anyone can potentially obtain Bob's public key.

But Bob's private key should remain under Bob's control.

---

# 🔐 Why Can the Public Key Be Shared?

The public key is specifically designed for operations that do not require revealing the private key.

For example, depending on the cryptographic system:

```text id="hj4jtu"
Public Key
    ↓
Encryption / Signature Verification / Key Agreement
```

while the corresponding private key may be used for:

```text id="2v8c4k"
Private Key
    ↓
Decryption / Signature Creation / Key Agreement
```

The exact operation depends on the algorithm and protocol.

---

# 🔒 2. Private Keys

## 📌 What is a Private Key?

A **private key** is the secret component of an asymmetric key pair.

It must be protected from unauthorized access.

```text id="qf5qgr"
Public Key
   ↓
Can generally be shared

Private Key
   ↓
Must be protected
```

If an attacker obtains a private key, they may be able to impersonate its owner or perform cryptographic operations that were supposed to be restricted to the legitimate owner.

The consequences depend on how the key is being used.

---

# 🧠 Public vs Private Key

| Public Key                                         | Private Key                                 |
| -------------------------------------------------- | ------------------------------------------- |
| Can generally be shared                            | Must remain secret                          |
| Used in public-key operations                      | Used in private-key operations              |
| Often distributed through certificates/directories | Stored securely by its owner                |
| Can be used to verify signatures                   | Used to create signatures                   |
| Can participate in encryption/key agreement        | Can participate in decryption/key agreement |

### Memory Trick

> 🔓 **Public = Share**

> 🔐 **Private = Protect**

---

# 🔄 3. Key Exchange

One of the biggest problems with symmetric encryption is:

> **How can two parties securely establish a shared secret when they have never communicated before?**

Asymmetric cryptography helps solve this problem through **key establishment** mechanisms.

---

# 🔑 The Key Distribution Problem

With symmetric encryption:

```text id="lpn0k6"
Alice 🔑
     \
      \  How do I securely give
       \ the key to Bob?
        \
         🔑 Bob
```

If Alice sends the secret key over an insecure network:

```text id="z4ks73"
Alice ───── Secret Key ─────→ Bob
              ↑
           Attacker
```

the attacker might steal it.

Asymmetric cryptography provides mechanisms that can establish shared secret material without directly sending that final secret in plaintext.

---

# 🔄 Diffie-Hellman Key Exchange

A classic example is **Diffie-Hellman (DH)**.

Modern systems commonly use variants such as **Elliptic-Curve Diffie-Hellman (ECDH)**.

The basic idea is fascinating:

> Alice and Bob can establish a shared secret even though an observer can see the public communication.

---

# 🧩 Simplified Concept

Imagine:

```text id="r5p6q8"
Alice                    Bob
  │                        │
  │──── Public Information→│
  │←─── Public Information─│
  │                        │
  │   Private Information  │
  │                        │
  └──── Shared Secret ─────┘
```

The attacker can observe the public communication but should not be able to feasibly calculate the resulting shared secret.

---

# 🔐 Important Clarification

Diffie-Hellman is primarily a **key agreement** mechanism.

It does not automatically tell Alice that she is really communicating with Bob.

This creates an important problem:

```text id="8s6t9e"
Key Agreement
      ≠
Authentication
```

Without authentication, an attacker may perform a **man-in-the-middle (MITM)** attack.

---

# ⚠️ Man-in-the-Middle Problem

Imagine:

```text id="v0u2x8"
Alice ←──── Attacker ────→ Bob
```

Alice thinks:

```text
"I'm communicating with Bob."
```

Bob thinks:

```text
"I'm communicating with Alice."
```

But the attacker is actually sitting between them.

This is why secure protocols combine:

```text id="5e4z5h"
Key Agreement
      +
Authentication
      +
Integrity Protection
      +
Encryption
```

---

# 🌐 TLS as a Real-World Example

When your browser connects to a secure website:

```text id="7f5w0h"
Browser
   │
   │
   ▼
TLS Handshake
   │
   ├── Server Authentication
   │
   ├── Key Establishment
   │
   └── Cryptographic Parameters
   │
   ▼
Session Keys
   │
   ▼
Symmetric Encryption
   │
   ▼
Application Data
```

The details depend on the TLS version and cipher suite, but the high-level concept is:

> **Asymmetric/public-key cryptography helps establish trust and shared secrets; symmetric cryptography then efficiently protects the actual data.**

---

# 🔏 4. Digital Trust

Public keys are useful only if you can determine:

> **"Does this public key actually belong to the entity I think it belongs to?"**

This is where **digital trust** becomes important.

---

# 🪪 Digital Certificates

A **digital certificate** binds an identity to a public key.

Conceptually:

```text id="b48c1k"
Identity
   +
Public Key
   +
Certificate Information
   ↓
Digital Certificate
```

For example, when you visit a website:

```text id="f9p2e4"
example.com
     ↓
Certificate
     ↓
Public Key
```

Your browser can inspect the certificate and determine whether it chains to a trusted certification authority and whether important checks succeed.

---

# 🏛️ Certificate Authorities

A **Certificate Authority (CA)** is an entity trusted to issue certificates according to defined validation processes and policies.

Examples include organizations that issue TLS certificates.

The browser/operating system has a set of trusted CA certificates.

Simplified:

```text id="f3m5qp"
Trusted Root CA
       │
       ↓
Intermediate CA
       │
       ↓
Website Certificate
       │
       ↓
example.com
```

This creates a **chain of trust**.

---

# 🔗 Chain of Trust

```text id="d9m3zw"
Root CA
  │
  │ trusts
  ▼
Intermediate CA
  │
  │ signs
  ▼
Website Certificate
  │
  │ identifies
  ▼
example.com
```

The browser can verify the certificate chain back toward a trusted root.

---

# ⚠️ What Does a Certificate Actually Prove?

A valid TLS certificate can provide evidence that:

* The certificate is valid according to its rules.
* The domain identity matches the certificate.
* The certificate chain leads to a trusted CA.
* The certificate has not failed relevant validity checks.

But:

> **A valid certificate does not mean the website itself is honest, safe, or free from vulnerabilities.**

HTTPS can protect your connection to a malicious or compromised website too.

---

# 🔏 Digital Signatures

Asymmetric cryptography also enables **digital signatures**.

A digital signature can help provide:

* Authentication of the signer/key holder
* Integrity of the signed data
* Evidence that the signature corresponds to a particular private key

Simplified process:

```text id="h6z7qm"
Message
   │
   ▼
Hash
   │
   ▼
Sign with Private Key
   │
   ▼
Digital Signature
```

The recipient can then verify:

```text id="a8x4sp"
Message
   +
Digital Signature
   +
Public Key
   ↓
Verification
```

---

# 🧠 Signature Example

Alice wants to sign:

```text id="k2m5u0"
"I approve this document."
```

Alice uses her private key to create a signature.

Bob receives:

```text id="2n0qce"
Message
+
Signature
+
Alice's Public Key
```

Bob verifies the signature.

If the message has been changed:

```text id="e2v7bs"
Original Message
      ↓
Signature
      ↓
Message Modified
```

verification should fail.

This provides an important integrity property.

---

# 🔐 5. Practical Applications

Asymmetric cryptography appears throughout cybersecurity.

---

## 🌐 1. HTTPS / TLS

TLS uses public-key cryptography and key agreement mechanisms as part of establishing secure connections.

```text id="r4y4pt"
Browser
   ↓
TLS Handshake
   ↓
Server Authentication
   ↓
Key Establishment
   ↓
Session Keys
   ↓
Symmetric Encryption
```

---

# 🔑 2. SSH

SSH uses public-key cryptography for secure remote administration.

For example:

```text id="brz8e4"
Your Computer
      │
      │ SSH
      ▼
Linux Server
```

SSH can use public/private key pairs for user authentication.

You may encounter files such as:

```text id="yupv8j"
id_ed25519
id_ed25519.pub
```

Conceptually:

```text id="0l1m3d"
id_ed25519
     ↓
Private Key 🔐

id_ed25519.pub
     ↓
Public Key 🔓
```

The private key should remain protected.

---

# ✍️ 3. Software Signing

Software developers can digitally sign software or packages.

Conceptually:

```text id="4z4y0b"
Software
   ↓
Hash
   ↓
Private Key
   ↓
Digital Signature
```

A user/system can use the corresponding public key to verify the signature.

This helps detect:

* Tampering
* Unauthorized modification
* Incorrect origin/signing key

---

# 📧 4. Email Security

Public-key cryptography can be used in email security technologies for:

* Encryption
* Digital signatures
* Sender authentication/integrity mechanisms

Examples include:

```text id="2om8s9"
PGP
S/MIME
```

---

# 💳 5. Digital Transactions

Digital signatures and public-key cryptography are used in various systems involving:

* Secure transactions
* Authentication
* Signed documents
* Software distribution
* Certificates
* Cryptographic protocols

---

# 🔐 6. Secure APIs

Public-key cryptography can be used for:

* Client authentication
* Digital signatures
* Certificate-based authentication
* Key establishment

For example:

```text id="zjv8be"
Client
   │
   │ Signed Request
   ↓
API Server
   │
   ↓
Signature Verification
```

---

# ⚖️ Symmetric vs Asymmetric

| Feature           | Symmetric               | Asymmetric                                     |
| ----------------- | ----------------------- | ---------------------------------------------- |
| Keys              | Shared secret           | Public + private pair                          |
| Speed             | Generally faster        | Generally slower                               |
| Main challenge    | Secure key distribution | Private-key protection                         |
| Bulk encryption   | Excellent               | Usually not preferred                          |
| Authentication    | Limited by itself       | Strong support through signatures/certificates |
| Key establishment | More difficult          | Designed to support key establishment          |
| Examples          | AES, ChaCha20           | RSA, ECC                                       |
| Common use        | Data protection         | Signatures, authentication, key establishment  |

---

# 🔥 Hybrid Cryptography

Modern systems frequently combine both approaches.

Why?

Because each has different strengths.

```text id="c5f84y"
       Asymmetric Cryptography
                │
                ↓
      Authentication / Key
          Establishment
                │
                ↓
        Session Key Created
                │
                ↓
       Symmetric Cryptography
                │
                ↓
       Fast Data Encryption
```

This is one of the most important concepts in practical cryptography.

### Example: HTTPS

```text id="z4y9a6"
TLS Handshake
     │
     ├── Certificate
     │
     ├── Authentication
     │
     └── Key Agreement
             ↓
       Session Secrets
             ↓
     Symmetric Encryption
             ↓
       Web Communication
```

---

# ⚠️ Security Considerations

## 1. Protect Private Keys

A public key can be distributed.

A private key must be protected.

```text id="ak4wq9"
Public Key  → 📢 Share
Private Key → 🔒 Protect
```

---

## 2. Never Put Private Keys in Git

Avoid:

```text id="9sp0m3"
github.com/project/
    private_key.pem
```

Private keys accidentally committed to repositories can become exposed.

---

## 3. Use Strong Algorithms

Do not casually choose old or deprecated cryptographic algorithms.

Use modern, well-reviewed algorithms and protocols appropriate for the use case.

---

## 4. Verify Certificates

Certificate validation matters.

A secure client should consider checks such as:

```text id="0c3k4z"
Certificate valid?
       ↓
Hostname matches?
       ↓
Not expired?
       ↓
Trusted chain?
       ↓
Not revoked / otherwise unacceptable?
```

The exact validation behavior depends on the protocol and environment.

---

## 5. Protect Against MITM

Key exchange without authentication can be vulnerable to interception.

Secure protocols combine:

```text id="a6w5p3"
Key Agreement
      +
Authentication
      +
Integrity
```

---

# 🧪 Practical Python Example

Python can generate an RSA key pair using the `cryptography` library.

```python id="j4v6ks"
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

```text id="31w6qf"
RSA Key Generation
       │
       ├──────────────┐
       ↓              ↓
Private Key 🔐     Public Key 🔓
```

For real applications, keys must also be serialized, stored securely, and managed appropriately.

---

# 🧪 Mini Practice

Try answering these yourself.

### 1.

What is the difference between a public key and a private key?

### 2.

Why can a public key generally be shared while a private key must be protected?

### 3.

What problem does key exchange/key agreement help solve?

### 4.

What is a Man-in-the-Middle attack?

### 5.

Why is authentication important during key establishment?

### 6.

What is a digital certificate?

### 7.

What is a Certificate Authority?

### 8.

What does a digital signature help provide?

### 9.

Why does HTTPS commonly use both asymmetric and symmetric cryptography?

### 10.

If someone obtains your private SSH key, why is that a serious security problem?

---

# 🧠 Memory Tricks

### Public vs Private

```text
PUBLIC  → SHARE
PRIVATE → PROTECT
```

### Key Agreement

```text
PUBLIC INFORMATION
        +
PRIVATE INFORMATION
        ↓
SHARED SECRET
```

### Digital Signature

```text
PRIVATE KEY
    ↓
SIGN

PUBLIC KEY
    ↓
VERIFY
```

### Hybrid Cryptography

```text
ASYMMETRIC
    ↓
AUTHENTICATE / ESTABLISH KEY
    ↓
SYMMETRIC
    ↓
ENCRYPT DATA
```

---

# ⚡ Quick Revision

```text id="3j7g1r"
        ASYMMETRIC CRYPTOGRAPHY
                  │
          ┌───────┴────────┐
          ↓                ↓
     Public Key 🔓    Private Key 🔐
          │                │
      Shareable         Secret
          │                │
          └───────┬────────┘
                  ↓
        Cryptographic Operations
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
   Key Agreement  Signatures  Authentication
```

### Core concepts

| Concept           | Remember                                                                 |
| ----------------- | ------------------------------------------------------------------------ |
| Public key        | Can generally be shared                                                  |
| Private key       | Must be protected                                                        |
| Key agreement     | Establish shared secret material                                         |
| Certificate       | Binds identity information to a public key                               |
| CA                | Issues/validates certificates within a trust system                      |
| Digital signature | Sign with private key, verify with public key                            |
| TLS               | Combines authentication/key establishment with symmetric data protection |
| SSH               | Uses cryptography for secure remote access and authentication            |

---

# 🛡️ Final Takeaway

Asymmetric cryptography solves problems that are difficult to handle with only shared-key encryption.

Instead of one secret key:

```text
🔑 One Shared Secret
```

we have:

```text
🔓 Public Key
🔐 Private Key
```

This enables important security capabilities such as:

```text
Key Agreement
Authentication
Digital Signatures
Certificates
Secure Remote Access
Software Signing
TLS
```

Modern systems generally don't choose **symmetric OR asymmetric** cryptography.

They often combine them:

> **Asymmetric cryptography helps establish trust and shared secrets; symmetric cryptography efficiently protects the actual data.**

That hybrid model is fundamental to technologies such as **TLS/HTTPS and SSH**.
