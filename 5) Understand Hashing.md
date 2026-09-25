# 🔐 5. Understand Hashing

> **Hashing is a one-way cryptographic process that converts data of any size into a fixed-size value called a hash.**
>
> In cybersecurity, hashes are commonly used for **password protection, file integrity, malware identification, and data verification**.

---

## 🧠 1. What is Hashing?

**Hashing** is the process of taking input data and passing it through a **hash function** to produce a fixed-length output.

```text
Input Data
    │
    ▼
┌─────────────────┐
│  Hash Function  │
└─────────────────┘
    │
    ▼
Hash Value
```

### Example

Suppose we hash:

```text
Hello
```

Using SHA-256, we get a fixed-length hexadecimal value:

```text
185f8db32271fe25f561a6fc938b2e264306ec304eda518007d1764826381969
```

Even though `"Hello"` is only 5 characters, the SHA-256 result is always **256 bits** (64 hexadecimal characters).

If we hash a huge file:

```text
movie.iso
```

the SHA-256 output is still:

```text
64 hexadecimal characters
```

### Important idea

> **Small input or huge input → fixed-size hash**

---

# ⚙️ 2. Hash Functions

A **hash function** is an algorithm that converts input data into a hash value.

Common cryptographic hash functions include:

| Algorithm | Output Size | Security Status                       |
| --------- | ----------: | ------------------------------------- |
| MD5       |    128 bits | ❌ Broken for security use             |
| SHA-1     |    160 bits | ❌ Deprecated for collision resistance |
| SHA-256   |    256 bits | ✅ Widely used                         |
| SHA-512   |    512 bits | ✅ Widely used                         |
| SHA-3     |    Variable | ✅ Modern cryptographic hash family    |

For modern cybersecurity work, you'll commonly encounter:

```text
SHA-256
SHA-512
SHA-3
```

---

## 🔑 Properties of a Cryptographic Hash

A secure cryptographic hash function should have several important properties.

### 1. Deterministic

The same input should always produce the same hash.

```text
"hello"
   ↓
SHA-256
   ↓
same hash
```

So:

```text
SHA256("hello") == SHA256("hello")
```

---

### 2. Fixed-Length Output

The output size depends on the algorithm, not the input size.

For SHA-256:

```text
"Hi"
     ↓
256-bit hash

"A very large file..."
     ↓
256-bit hash
```

---

### 3. One-Way / Preimage Resistant

Given:

```text
Hash → ?
```

it should be computationally infeasible to recover the original input.

```text
Original Data
     ↓
   SHA-256
     ↓
    Hash
```

You should not be able to simply reverse:

```text
Hash → Original Data
```

This is why hashing is fundamentally different from encryption.

---

### 4. Avalanche Effect

A tiny change in the input should produce a drastically different hash.

Example:

```text
Hello
```

vs

```text
hello
```

Changing only:

```text
H → h
```

causes a completely different hash.

```text
Hello → 185f8db3...
hello → 2cf24dba...
```

This makes hashes useful for detecting even small modifications.

---

### 5. Collision Resistance

A **collision** occurs when two different inputs produce the same hash.

```text
Input A ──┐
          ├── Hash Function ──→ Same Hash
Input B ──┘

Input A ≠ Input B
```

A secure cryptographic hash should make finding such collisions computationally difficult.

---

# 🔐 3. Hashing vs Encryption

This is one of the most important distinctions.

| Feature                    | Hashing      | Encryption              |
| -------------------------- | ------------ | ----------------------- |
| Purpose                    | Verification | Confidentiality         |
| Usually reversible?        | ❌ No         | ✅ Yes, with key         |
| Uses key?                  | Usually ❌    | ✅                       |
| Same input → same output?  | Yes          | Depends on scheme/nonce |
| Can recover original data? | No           | Yes                     |
| Example                    | SHA-256      | AES                     |
| Password storage           | ✅            | ❌ Not normally          |
| File integrity             | ✅            | ❌ Not directly          |

### Memory Trick

```text
HASH = VERIFY
ENCRYPT = HIDE
```

---

# 🔑 4. Password Hashing

Passwords are one of the most important uses of hashing.

A system should **not normally store users' passwords as plaintext**.

Bad:

```text
username: farhan
password: MyPassword123
```

If the database is compromised, attackers immediately obtain the password.

Instead:

```text
Password
    ↓
Password Hashing Function
    ↓
Stored Password Hash
```

When the user logs in:

```text
User enters password
        ↓
Password hashing / verification
        ↓
Compare with stored password representation
        ↓
Match?
   ↙         ↘
 YES         NO
 ↓            ↓
Login       Reject
```

---

# 🧂 5. What is a Salt?

A **salt** is a unique random value added to a password before password hashing.

Conceptually:

```text
Password + Salt
       ↓
Password Hashing Function
       ↓
Stored Hash
```

For example:

```text
Password:
MyPassword123

Salt:
x7K@92Lm

        ↓

Password Hashing Function

        ↓

Stored Password Hash
```

The salt does **not** need to be secret.

It is normally stored alongside the password hash.

---

## 🚨 Why Do We Need Salts?

Without salts, two users with the same password could have the same hash.

```text
User A:
Password → Hash A

User B:
Same Password → Hash A
```

An attacker could identify that both users have the same password.

With unique salts:

```text
Password + Salt A → Hash A
Password + Salt B → Hash B
```

Now:

```text
Hash A ≠ Hash B
```

even though the passwords are identical.

---

# 🛡️ 6. Password Hashing Algorithms

General-purpose hashes such as:

```text
MD5
SHA-1
SHA-256
```

are **not the preferred tools for password storage**.

Password storage should use dedicated password-hashing / password-KDF algorithms designed to be expensive and resistant to password-cracking attacks.

Common choices include:

### Argon2

Modern password hashing/KDF algorithm.

```text
Password
   ↓
Argon2 + Salt
   ↓
Stored Password Hash
```

### bcrypt

Widely used password hashing algorithm.

```text
Password
   ↓
bcrypt + Salt
   ↓
Stored Hash
```

### scrypt

Designed to make password cracking more resource-intensive, particularly through memory requirements.

---

# ⚠️ Why Not Just SHA-256?

Consider:

```text
SHA-256("password123")
```

Modern computers can calculate huge numbers of SHA-256 hashes very quickly.

That's useful for many integrity applications, but **bad for password storage**.

Attackers can try:

```text
password
password1
password123
qwerty
123456
...
```

very quickly.

Password-hashing algorithms are deliberately designed to make each password guess more expensive.

### Important distinction

```text
SHA-256
↓
Fast cryptographic hash
```

versus:

```text
Argon2 / bcrypt / scrypt
↓
Password hashing / key derivation
↓
Intentionally expensive
```

---

# 🧬 7. Data Integrity

Hashing is heavily used to verify **data integrity**.

Integrity means:

> **The data has not been modified unexpectedly.**

Suppose you download:

```text
ubuntu.iso
```

The publisher provides:

```text
Expected SHA-256:
ABC123...
```

You calculate the hash of your downloaded file:

```text
Downloaded File
      ↓
   SHA-256
      ↓
Actual Hash
```

Then compare:

```text
Expected Hash
      =
Actual Hash
```

If they match:

```text
✅ Hashes match
```

This provides evidence that the file contents match the data represented by that expected hash.

If they don't:

```text
❌ Hash mismatch
```

The file may have been corrupted, altered, or you may have downloaded a different file.

> A hash match is evidence of matching content; it does not by itself prove who provided the file.

---

# 🦠 8. Hashes in Malware Analysis

Security professionals frequently calculate hashes for suspicious files.

For example:

```text
suspicious.exe
      ↓
SHA-256
      ↓
9f86d081884c7d...
```

That hash can act as a **file identifier**.

Security teams can then search their systems for the same hash.

```text
File
 ↓
SHA-256
 ↓
File Hash
 ↓
Search security systems
 ↓
Found elsewhere?
```

This is useful for:

* Malware identification
* Incident response
* Threat intelligence
* File integrity monitoring
* Digital forensics
* Security investigations

### Example

Suppose analysts identify:

```text
malware.exe
SHA-256:
abc123...
```

They can search logs, endpoint telemetry, or other security data for:

```text
abc123...
```

and investigate where that exact file hash appears.

---

# 🔍 9. Hash Verification

**Hash verification** means calculating a hash and comparing it against a trusted expected hash.

### Workflow

```text
                 ┌──────────────────┐
                 │ Original / Known │
                 │ Expected Hash    │
                 └────────┬─────────┘
                          │
                          ▼
                     Compare
                          ▲
                          │
                 ┌────────┴─────────┐
                 │ Hash Your File   │
                 └──────────────────┘
```

### Result

```text
Expected Hash
      │
      ▼
ABC123

Actual Hash
      │
      ▼
ABC123

      ↓

✅ Match
```

or:

```text
Expected:
ABC123

Actual:
XYZ789

      ↓

❌ Mismatch
```

---

# 💻 10. Hashing Files in Linux

Linux provides tools for calculating hashes.

### SHA-256

```bash
sha256sum file.txt
```

Example:

```text
$ sha256sum file.txt

185f8db32271fe25f561a6fc938b2e264306ec304eda518007d1764826381969  file.txt
```

### SHA-512

```bash
sha512sum file.txt
```

### MD5

```bash
md5sum file.txt
```

> MD5 can still appear in legacy systems and as a non-security identifier, but it should not be chosen for modern security-sensitive collision resistance.

---

# 🐍 11. Hashing with Python

Python provides the `hashlib` module.

```python
import hashlib

data = b"Hello"

hash_value = hashlib.sha256(data).hexdigest()

print(hash_value)
```

Output:

```text
185f8db32271fe25f561a6fc938b2e264306ec304eda518007d1764826381969
```

---

## 📁 Hash a File

```python
import hashlib

with open("example.txt", "rb") as file:
    data = file.read()

hash_value = hashlib.sha256(data).hexdigest()

print("SHA-256:", hash_value)
```

For large files, reading the entire file into memory isn't ideal.

A better approach is to process the file in chunks:

```python
import hashlib

sha256 = hashlib.sha256()

with open("example.txt", "rb") as file:
    while chunk := file.read(4096):
        sha256.update(chunk)

print(sha256.hexdigest())
```

This is useful when working with large files.

---

# 🔄 12. Hash Verification in Python

You can compare an expected hash with the calculated hash.

```python
import hashlib

expected_hash = "YOUR_EXPECTED_HASH"

with open("example.txt", "rb") as file:
    sha256 = hashlib.sha256()

    while chunk := file.read(4096):
        sha256.update(chunk)

actual_hash = sha256.hexdigest()

if actual_hash == expected_hash:
    print("Hash verified")
else:
    print("Hash mismatch")
```

This basic idea is used in many integrity-checking workflows.

---

# 💥 13. Collision Awareness

A **collision** occurs when:

```text
Input A ≠ Input B
```

but:

```text
Hash(Input A) = Hash(Input B)
```

Example conceptually:

```text
File A ──→ Hash Function ──→ ABC123
                              ▲
File B ──→ Hash Function ──→ ABC123

File A ≠ File B
```

This is a collision.

---

## 🎯 Why Are Collisions Possible?

A hash function can accept an enormous number of possible inputs but produces a fixed-size output.

For example, SHA-256 has:

```text
2²⁵⁶
```

possible outputs.

There are far more possible inputs than outputs.

Therefore, mathematically, collisions must exist.

The security goal is not:

> "Collisions are impossible."

Instead:

> **Finding a useful collision should be computationally infeasible for a secure cryptographic hash.**

---

# ⚠️ 14. MD5 and SHA-1

### MD5

MD5 produces:

```text
128-bit hash
```

It has known practical collision weaknesses.

Therefore:

```text
❌ Don't use MD5 for modern security-sensitive integrity/collision-resistance purposes.
```

### SHA-1

SHA-1 produces:

```text
160-bit hash
```

It also has demonstrated collision attacks and is deprecated for security applications requiring collision resistance.

### Modern choices

Prefer modern algorithms such as:

```text
SHA-256
SHA-512
SHA-3
```

depending on the application.

---

# 🎲 15. Birthday Paradox and Hash Collisions

Collision probability becomes relevant sooner than simply waiting for every possible hash value to be used.

This is related to the **birthday paradox**.

For an ideal `n`-bit hash, collision resistance is roughly on the order of:

```text
2^(n/2)
```

work because of birthday attacks.

For SHA-256:

```text
256-bit output
      ↓
rough collision-security scale
      ↓
2^128
```

This is one reason larger hash outputs provide stronger collision resistance.

---

# 🔐 16. Hashing and Digital Signatures

Hashing is also an important part of **digital signatures**.

Instead of signing a huge file directly:

```text
Large File
    ↓
SHA-256
    ↓
Hash
    ↓
Digital Signature
```

The signature system can sign the digest.

Verification:

```text
Received File
      ↓
Calculate Hash
      ↓
Hash A

Digital Signature
      ↓
Verify
      ↓
Hash B

Hash A == Hash B
      ↓
Integrity + signature verification
```

This combines hashing with asymmetric cryptography.

---

# 🧩 17. Hashing in Cybersecurity

Hashing appears in many areas:

| Security Area                | Hashing Use                     |
| ---------------------------- | ------------------------------- |
| 🔑 Password Security         | Password hashing                |
| 📁 File Integrity            | Detect file changes             |
| 🦠 Malware Analysis          | Identify files                  |
| 🔎 Digital Forensics         | Evidence/file identification    |
| 🌐 Digital Signatures        | Message/file digest             |
| 📦 Software Distribution     | Verify downloads                |
| 🛡️ SIEM                     | Search and correlate indicators |
| 🔐 Cryptographic Protocols   | Integrity-related constructions |
| 💾 File Integrity Monitoring | Detect modifications            |

---

# 🆚 18. Hashing vs Password Hashing vs Encryption

These are related but different concepts.

| Feature              | General Hashing          | Password Hashing       | Encryption      |
| -------------------- | ------------------------ | ---------------------- | --------------- |
| Main purpose         | Integrity/identification | Password protection    | Confidentiality |
| Reversible           | ❌                        | ❌                      | ✅               |
| Secret key required  | Usually ❌                | ❌                      | Usually ✅       |
| Typical algorithms   | SHA-256, SHA-3           | Argon2, bcrypt, scrypt | AES, ChaCha20   |
| Designed to be fast? | Usually yes              | Intentionally slower   | Depends         |
| Example              | File hash                | Login password storage | HTTPS data      |

### Easy memory trick

```text
🔐 Encryption → Hide it
🔎 Hashing → Identify/verify it
🔑 Password hashing → Protect passwords
```

---

# ⚠️ 19. Common Hashing Mistakes

### ❌ Mistake 1 — Treating hashing as encryption

```text
Hash ≠ Encryption
```

A hash is not something you normally decrypt.

---

### ❌ Mistake 2 — Storing plaintext passwords

Never design a password system around:

```text
password = "MyPassword123"
```

stored directly in a database.

---

### ❌ Mistake 3 — Using plain SHA-256 for passwords

For password storage, use an appropriate password hashing/KDF algorithm such as:

```text
Argon2
bcrypt
scrypt
```

with proper salt handling.

---

### ❌ Mistake 4 — Using MD5 for security-sensitive collision resistance

```text
MD5 ❌
```

Use a modern cryptographic hash where appropriate.

---

### ❌ Mistake 5 — Assuming a hash proves authenticity

A hash tells you whether two inputs produce the same digest.

It does **not automatically prove who created or published the file**.

For authenticity, systems may use:

```text
Digital Signatures
Certificates
Trusted distribution channels
MACs
```

depending on the situation.

---

### ❌ Mistake 6 — Ignoring collision resistance

A hash function should be selected according to the security requirement.

Don't choose an algorithm simply because:

```text
"It produces a hash."
```

---

# 🧪 20. Mini Practical Exercise

Create a file:

```bash
nano message.txt
```

Add:

```text
Cybersecurity is interesting.
```

Calculate:

```bash
sha256sum message.txt
```

Now modify the file:

```text
Cybersecurity is very interesting.
```

Run:

```bash
sha256sum message.txt
```

Compare the results.

You should observe:

```text
Before modification
        ↓
Hash A

After modification
        ↓
Hash B

Hash A ≠ Hash B
```

This demonstrates the **avalanche effect** and how hashes can help detect changes.

---

# 🧠 21. Hashing Memory Map

Remember:

```text
                 🔐 HASHING
                     │
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
  Hash Function  Passwords     Integrity
       │             │             │
       ▼             ▼             ▼
 SHA-256        Argon2       File Hash
 SHA-512        bcrypt       Verification
 SHA-3          scrypt       Change Detection
       │
       ▼
  Collision Awareness
```

### Memory Trick

> **HASH = HIDE? ❌**
>
> **HASH = CHECK 🔎**

---

# 🧠 Quick Revision

### What is hashing?

A one-way transformation that produces a fixed-size digest.

### What is a hash function?

An algorithm that converts input data into a hash.

### Why hash passwords?

To avoid storing passwords directly and make database compromise less damaging.

### What is a salt?

A unique random value used with password hashing to prevent identical passwords from producing identical stored hashes.

### What is data integrity?

Confidence that data has not been unexpectedly modified.

### What is hash verification?

Calculating a hash and comparing it with a trusted expected hash.

### What is a collision?

Two different inputs producing the same hash.

### Why are MD5 and SHA-1 problematic?

They have known collision weaknesses and should not be selected for modern security-sensitive collision resistance.

### What should you remember?

```text
SHA-256 → General cryptographic hashing
Argon2  → Password hashing
Hash    → Verify / identify
Encrypt → Hide
Salt    → Protect password hashes
Collision → Different inputs, same hash
```

---

# 🎯 Final Takeaway

Hashing is one of the fundamental building blocks of cybersecurity.

You should understand the difference between:

```text
              HASHING
                 │
       ┌─────────┴─────────┐
       ▼                   ▼
   Integrity          Identification
       │                   │
       ▼                   ▼
 File Verification    Malware Hash
       │
       ▼
 Password Protection
       │
       ▼
 Argon2 / bcrypt / scrypt
```

The most important concepts to remember are:

> 🔹 **Hashing is generally one-way.**
> 🔹 **A hash has a fixed output size.**
> 🔹 **Small input changes produce very different hashes.**
> 🔹 **Hashes can verify data integrity.**
> 🔹 **Passwords require dedicated password-hashing algorithms.**
> 🔹 **Salts protect against identical password hashes and precomputed attacks.**
> 🔹 **Collision resistance matters.**
> 🔹 **MD5 and SHA-1 should not be used for modern security-sensitive collision resistance.**
> 🔹 **Hashing and encryption solve different problems.**

**Core memory:**

```text
🔐 Encryption → Confidentiality
🔎 Hashing    → Integrity / Identification
🔑 Argon2     → Password Protection
💥 Collision  → Different Input → Same Hash
```

This completes **Topic 5: Understand Hashing**. The next natural topic in the cryptography module is **Topic 6**, whenever you send its syllabus points.
