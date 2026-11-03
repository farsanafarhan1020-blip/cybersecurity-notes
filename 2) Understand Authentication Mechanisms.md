# 🔐 Understanding Authentication Mechanisms

> **Authentication mechanisms are the technologies and processes used to verify the identity of users, devices, applications, and services before allowing them to access protected resources.**

Authentication is a fundamental part of cybersecurity and Identity & Access Management (IAM).

Whenever you log in to an application, access a company network, connect to a cloud platform, or use an online banking service, authentication mechanisms help verify that you are who you claim to be.

Modern authentication goes beyond passwords. It includes Multi-Factor Authentication (MFA), Single Sign-On (SSO), identity federation, passwordless authentication, and cryptographic verification.

---

## 1. 🔑 Password-Based Authentication

### What Is Password-Based Authentication?

Password-based authentication is a method of verifying a user's identity using a secret password associated with their account.

It is one of the most widely used authentication methods.

**Example:**

When you log in to GitHub:

1. Enter your username or email.
2. Enter your password.
3. The authentication system verifies your credentials.
4. If verification succeeds, access is granted.

```text
        User
          |
          v
    Enter Username
          |
          v
     Enter Password
          |
          v
   Authentication Server
          |
          v
   Verify Credentials
          |
       +--+--+
       |     |
    Valid   Invalid
       |     |
       v     v
    Access  Access
    Granted Denied
```

### How Does Password Authentication Work Internally?

A common misconception is that websites store your actual password.

Secure authentication systems should not store passwords in plaintext.

Instead, they store a password hash produced using a password-hashing algorithm.

**Password hashing process:**

```text
       User Creates Password
                |
                v
       Password + Unique Salt
                |
                v
        Password Hashing
                |
                v
       Stored Password Hash
```

When the user logs in:

1. The user submits their password.
2. The authentication system retrieves the stored password hash and salt.
3. It uses the submitted password with the appropriate hashing process.
4. It compares the resulting hash with the stored hash.
5. If they match, authentication succeeds.

### What Is Password Hashing?

Hashing converts input data into a fixed-length output using a mathematical algorithm.

Password hashing is designed to make recovering the original password computationally difficult.

Example:

```python
import hashlib

password = "CyberSecure123"

hashed_password = hashlib.sha256(
    password.encode()
).hexdigest()

print(hashed_password)
```

This demonstrates basic hashing, but **SHA-256 alone is not recommended for storing passwords**.

For real password storage, use dedicated password-hashing algorithms such as:

- Argon2id.
- bcrypt.
- scrypt.
- PBKDF2.

These algorithms are designed to make password guessing more expensive.

### What Is a Salt?

A salt is a unique random value added to a password before hashing.

Example:

```text
Password: password123
Salt:     x7k9p2

Password + Salt
       |
       v
    Hashing
       |
       v
   Stored Hash
```

Why use salts?

- Prevents identical passwords from producing identical stored hashes when unique salts are used.
- Makes precomputed rainbow-table attacks much less effective.
- Forces attackers to perform separate cracking computations for each salted password.

A salt does not make a weak password strong. It is one component of secure password storage.

### Common Password-Based Attacks

| Attack | Description |
|---|---|
| Brute-Force Attack | Tries many possible password combinations |
| Dictionary Attack | Tries passwords from a prepared wordlist |
| Credential Stuffing | Uses stolen username-password combinations from previous breaches |
| Password Spraying | Tries a small number of common passwords against many accounts |
| Phishing | Tricks users into revealing their credentials |
| Keylogging | Captures keystrokes to obtain passwords |
| Shoulder Surfing | Observes someone entering their password |

### Password Security Best Practices

- Use long, unique passwords.
- Avoid reusing passwords across services.
- Use a reputable password manager.
- Enable MFA wherever possible.
- Never share passwords.
- Avoid entering credentials on suspicious websites.
- Use secure password-hashing algorithms when developing applications.
- Implement account protections such as rate limiting and login monitoring.

**Security lesson:** Passwords are still important, but password-only authentication is not sufficient protection for many sensitive systems.

---

## 2. 🛡️ Multi-Factor Authentication (MFA)

### What Is Multi-Factor Authentication?

Multi-Factor Authentication is an authentication method that requires users to provide two or more independent authentication factors.

Instead of relying only on a password, MFA adds additional verification.

**Example:**

When accessing an online account:

1. Enter your password.
2. Verify using an authenticator application.
3. Access is granted if both checks succeed.

Even if an attacker steals your password, they may not be able to access your account without the additional factor.

### Three Main Authentication Factors

| Factor | Meaning | Examples |
|---|---|---|
| Something You Know | Information known to the user | Password, PIN |
| Something You Have | Something possessed by the user | Security key, authenticator device |
| Something You Are | A biometric characteristic | Fingerprint, face recognition |

### How MFA Works

```text
           User
             |
             v
       Enter Password
             |
             v
     Password Verified
             |
             v
    Additional Verification
             |
       +-----+-----+
       |           |
    Success      Failure
       |           |
       v           v
   Access       Access
   Granted      Denied
```

### Common MFA Methods

#### 1. SMS-Based OTP

A one-time password is sent to the user's registered mobile number.

Example:

```text
Enter Password
      |
      v
SMS OTP Sent
      |
      v
Enter 6-Digit Code
      |
      v
Verify Code
      |
      v
Access Granted
```

**Advantages:**
- Easy to understand.
- Does not require a separate authenticator application.

**Limitations:**
- Vulnerable to SIM-swapping attacks.
- SMS messages can be intercepted through certain attack scenarios.
- Phishing can trick users into sharing the OTP.

SMS-based MFA is generally weaker than phishing-resistant methods.

#### 2. Authenticator Applications

Authenticator applications generate time-based or event-based one-time passwords.

Examples:
- Google Authenticator.
- Microsoft Authenticator.
- Authy.

A common standard is **TOTP (Time-Based One-Time Password)**.

How TOTP works:

1. The service and authenticator application share a secret during enrollment.
2. Both calculate a one-time code using the secret and current time interval.
3. The user enters the generated code.
4. The server verifies the code.

TOTP codes commonly change every 30 seconds.

**Important:** TOTP is better than SMS in several respects, but it can still be vulnerable to real-time phishing.

#### 3. Push-Based Authentication

The authentication system sends an approval request to a registered device.

Example:

You attempt to log in to a cloud account. Your authenticator application receives a notification asking you to approve the login.

Some systems use number matching to make accidental approval and certain MFA fatigue attacks more difficult.

#### 4. Hardware Security Keys

Hardware security keys are physical devices used to authenticate through cryptographic protocols.

Examples include FIDO2 security keys.

They provide strong protection against phishing when correctly implemented because authentication is bound to the legitimate website's origin.

#### 5. Biometric Authentication

Uses physical or behavioral characteristics to verify identity.

Examples:
- Fingerprint recognition.
- Facial recognition.
- Iris recognition.

Biometrics are often combined with device-based cryptographic authentication.

### MFA vs 2FA

| MFA | 2FA |
|---|---|
| Requires two or more authentication factors | Requires exactly two authentication factors |
| Broader concept | A specific form of MFA |
| Example: Password + security key + biometric factor | Example: Password + security key |

Every properly implemented 2FA system is MFA, but not every MFA system is limited to two factors.

### MFA Fatigue Attack

An MFA fatigue attack occurs when an attacker repeatedly sends authentication approval requests to a victim, hoping the victim eventually approves one.

**Example:**

An attacker obtains a user's password and repeatedly triggers push notifications.

The user becomes frustrated and accidentally approves the request.

**Defenses:**
- Number matching.
- Rate limiting.
- User awareness.
- Phishing-resistant MFA.
- Monitoring repeated authentication requests.

### MFA Best Practices

- Prefer phishing-resistant authentication methods.
- Enforce MFA for privileged accounts.
- Protect MFA recovery processes.
- Monitor unusual authentication attempts.
- Avoid relying only on SMS for high-risk accounts.
- Require stronger verification for sensitive actions.
- Provide secure recovery methods if a device is lost.

---

## 3. 🔄 Single Sign-On (SSO) Concepts

### What Is Single Sign-On?

Single Sign-On is an authentication approach that allows a user to authenticate once and access multiple connected applications without repeatedly entering credentials.

Instead of maintaining separate login sessions for every application, SSO uses a centralized identity provider.

**Example:**

Imagine you use:
- Gmail.
- Google Drive.
- Google Docs.
- Google Calendar.

After signing in to your Google account, you can access multiple Google services without entering your password every time.

This is an example of an SSO experience.

### How SSO Works

SSO typically involves three important components:

| Component | Purpose |
|---|---|
| User | The person requesting access |
| Identity Provider (IdP) | Authenticates the user |
| Service Provider (SP) | Application or service the user wants to access |

### SSO Authentication Flow

```text
          User
            |
            v
     Open Application
            |
            v
   Application Checks Session
            |
      +-----+-----+
      |           |
   Session      No Session
   Exists          |
      |            v
      |      Redirect to IdP
      |            |
      |            v
      |      User Authenticates
      |            |
      |            v
      |       IdP Verifies
      |         Identity
      |            |
      |            v
      |      Authentication
      |         Response
      |            |
      |            v
      |      Application
      |       Validates
      |            |
      +------+-----+
             |
             v
       Access Granted
```

### Important SSO Components

#### Identity Provider (IdP)

An Identity Provider is responsible for authenticating users and providing trusted identity information to connected applications.

Examples:
- Microsoft Entra ID.
- Okta.
- Google Identity.

#### Service Provider (SP)

A Service Provider is an application that relies on an Identity Provider for authentication.

Examples:
- A company HR application.
- A cloud management portal.
- A business collaboration platform.

### Benefits of SSO

- Users need fewer separate passwords.
- Simplifies login experiences.
- Centralizes authentication management.
- Makes employee onboarding and offboarding easier.
- Supports centralized MFA enforcement.
- Reduces password reuse across connected applications.

### SSO Security Risks

SSO also introduces security considerations.

**1. Single Point of Failure**

If an attacker compromises the central identity provider account, multiple connected applications may become accessible.

**2. Session Hijacking**

An attacker who steals a valid session token may be able to access an application without knowing the user's password.

**3. Misconfigured Trust**

Incorrect SSO configurations may allow unauthorized identities or applications to gain access.

**4. Excessive Application Access**

A user may receive access to applications they do not actually need.

### SSO Security Best Practices

- Enforce MFA at the identity provider.
- Protect administrator accounts.
- Monitor authentication and session activity.
- Configure application trust correctly.
- Use secure session management.
- Revoke sessions when accounts are disabled or compromised.
- Regularly review connected applications and assigned access.

**Important:** SSO does not automatically mean MFA. SSO simplifies authentication across applications; MFA strengthens identity verification.

---

## 4. 🌐 Identity Federation Awareness

### What Is Identity Federation?

Identity Federation allows different organizations, identity systems, or security domains to trust authentication information provided by one another.

It enables users to access external applications or services using an identity managed by another trusted organization.

In simple terms:

> Identity federation allows one organization to trust another organization's identity provider to authenticate users.

### Real-World Example

Imagine a university provides students with institutional accounts.

A student wants to access an external online learning platform.

Instead of creating a separate account, the learning platform trusts the university's identity provider.

The student signs in using their university credentials.

```text
       Student
          |
          v
   Learning Platform
          |
          v
   Redirect to University
      Identity Provider
          |
          v
   Student Authenticates
          |
          v
  University Issues Trusted
   Identity Assertion/Token
          |
          v
   Learning Platform Validates
          |
          v
      Access Granted
```

The learning platform does not need to manage the student's original password.

### Main Components of Identity Federation

| Component | Description |
|---|---|
| Identity Provider (IdP) | Authenticates the user |
| Service Provider / Relying Party | Relies on the trusted identity system |
| Trust Relationship | Defines which identity provider is trusted |
| Identity Assertion / Token | Carries verified identity information |
| Federation Protocol | Defines how identity information is exchanged |

### Common Federation Technologies

#### 1. SAML 2.0

Security Assertion Markup Language (SAML) is an XML-based standard used to exchange authentication and authorization-related information between identity providers and service providers.

Commonly used in enterprise SSO.

Example:

A company employee signs in once through the company identity provider and accesses a third-party HR application.

#### 2. OpenID Connect (OIDC)

OpenID Connect is an identity layer built on top of OAuth 2.0.

It allows applications to verify a user's identity using an ID token.

Commonly used for:
- Modern web applications.
- Mobile applications.
- Social login.
- Cloud identity integration.

#### 3. OAuth 2.0

OAuth 2.0 is an authorization framework that allows applications to obtain limited access to resources without receiving the user's password.

Example:

You allow a third-party application to access selected information from your Google account.

The application receives an access token with defined permissions rather than your Google password.

**Important distinction:**

| Technology | Primary Purpose |
|---|---|
| SAML | Federated identity and enterprise SSO |
| OAuth 2.0 | Delegated authorization |
| OpenID Connect | Authentication and identity information on top of OAuth 2.0 |

OAuth 2.0 alone should not be treated as proof of user authentication.

### SSO vs Identity Federation

These concepts are related, but they are not identical.

| SSO | Identity Federation |
|---|---|
| Allows access to multiple applications through a shared authentication experience | Establishes trust between separate identity domains |
| Focuses on user login experience | Focuses on cross-domain identity trust |
| Can operate within one organization | Often connects different organizations |
| May use federation protocols | Commonly uses protocols such as SAML and OIDC |

**Relationship:** Federation is commonly used to implement SSO across organizational boundaries, but SSO can also operate within a single organization.

### Federation Security Risks

- Trusting an incorrectly configured identity provider.
- Accepting expired or invalid tokens.
- Incorrect token audience or issuer validation.
- Misconfigured redirect URIs.
- Excessive trust in external identity attributes.
- Stolen session tokens.
- Weak identity provider security.

### Federation Security Best Practices

- Establish trust only with verified identity providers.
- Validate token signatures, issuer, audience, and expiration.
- Use secure redirect URI configurations.
- Protect signing keys and client secrets.
- Apply MFA at the appropriate identity provider.
- Limit permissions granted to federated identities.
- Monitor federation authentication events.

---

## 5. 🚀 Modern Authentication Practices

Traditional password-based authentication is no longer sufficient for protecting many modern systems.

Modern authentication combines stronger verification, cryptography, contextual security decisions, and continuous monitoring.

### 1. Passwordless Authentication

Passwordless authentication allows users to authenticate without entering a traditional password.

Common methods:
- Passkeys.
- FIDO2 security keys.
- Device-bound cryptographic credentials.
- Biometric verification through secure device authentication.

**Benefits:**
- Reduces password theft risks.
- Can provide phishing-resistant authentication.
- Improves user experience.
- Reduces password reset requirements.

### 2. Passkeys and FIDO2

Passkeys are passwordless credentials based on public-key cryptography.

They are commonly implemented using FIDO2 standards, including WebAuthn.

How passkey authentication works:

```text
       User Registers Passkey
                |
                v
       Device Creates Key Pair
                |
          +-----+-----+
          |           |
          v           v
      Public Key   Private Key
          |           |
          v           v
      Server Stores  Device Protects
      Public Key     Private Key
                          |
                          v
                   User Authenticates
                          |
                          v
                  Cryptographic Proof
                          |
                          v
                  Server Verifies
                          |
                          v
                    Access Granted
```

**Important:**
- The public key is registered with the service.
- The private key remains protected by the user's device or credential provider.
- The server verifies a cryptographic signature.
- The private key is not transmitted to the server.

This makes properly implemented passkeys resistant to many traditional phishing attacks.

### 3. Risk-Based Authentication

Risk-Based Authentication adjusts authentication requirements based on the risk associated with a login attempt.

It evaluates contextual information such as:

- Login location.
- Device identity and security status.
- Network characteristics.
- Unusual login behavior.
- Previous authentication patterns.
- Sensitivity of the requested resource.

**Example:**

A user normally signs in from a recognized device.

Suddenly, a login occurs from an unfamiliar device and unusual location.

The system may:
- Request additional verification.
- Require a phishing-resistant authentication method.
- Restrict sensitive actions.
- Block the login if the risk is sufficiently high.

Risk-based authentication should be carefully configured to avoid false positives and unnecessary account lockouts.

### 4. Adaptive Authentication

Adaptive Authentication dynamically adjusts authentication requirements based on changing context and risk.

For example:

- Low-risk activity may require normal authentication.
- Access to sensitive data may require stronger verification.
- High-risk administrative operations may require step-up authentication.

Risk-based and adaptive authentication are closely related and are often used together.

### 5. Zero Trust Authentication

Zero Trust is a security approach based on the principle:

> Never trust implicitly; always verify explicitly.

Traditional network security sometimes assumes that users inside a corporate network are trustworthy.

Zero Trust does not automatically trust users or devices just because they are inside the network.

It considers:
- User identity.
- Device security.
- Resource sensitivity.
- Request context.
- Authentication strength.
- Authorization policy.

Access decisions are continuously evaluated according to the organization's architecture and available signals.

**Example:**

An employee successfully signs in to a company network.

However, when attempting to access a sensitive production database, the system checks additional conditions such as device compliance, role permissions, and access policy.

Successful authentication does not automatically grant unrestricted access.

### 6. Continuous Authentication and Session Security

Authentication should not be considered the only security check needed throughout a session.

Modern systems can monitor ongoing sessions for suspicious behavior.

Examples:
- Unusual changes in device or network context.
- Suspicious session activity.
- Risky access patterns.
- Unexpected privileged operations.

Possible responses include:
- Requiring reauthentication.
- Revoking sessions.
- Blocking sensitive operations.
- Triggering security alerts.

Session security is important because attackers may steal session cookies or tokens and bypass the normal login process.

### 7. Secure Authentication Practices

| Practice | Security Benefit |
|---|---|
| Phishing-resistant MFA | Reduces credential phishing risk |
| Passkeys | Reduces dependence on passwords |
| Short-lived tokens | Limits the useful lifetime of stolen credentials |
| Secure session management | Reduces session hijacking risks |
| Rate limiting | Helps prevent automated login attacks |
| Account lockout or throttling | Limits repeated password guessing |
| Risk-based authentication | Detects unusual login situations |
| Centralized identity management | Simplifies policy enforcement |
| Secure account recovery | Prevents attackers from bypassing normal authentication |
| Continuous monitoring | Helps detect suspicious sessions and activity |

---

## 6. ⚔️ Authentication Attacks and Defensive Controls

Understanding authentication mechanisms also means understanding how attackers attempt to bypass them.

| Attack | How It Works | Defensive Control |
|---|---|---|
| Brute Force | Repeatedly guesses passwords | Rate limiting and strong authentication |
| Credential Stuffing | Uses credentials stolen from other services | Unique passwords and MFA |
| Password Spraying | Tries common passwords against many accounts | MFA, monitoring, and smart throttling |
| Phishing | Tricks users into revealing credentials | Phishing-resistant MFA and awareness |
| MFA Fatigue | Repeatedly sends approval requests | Number matching and request throttling |
| SIM Swapping | Takes control of a victim's mobile number | Prefer stronger MFA methods |
| Session Hijacking | Steals a valid session token | Secure cookies, session monitoring, and revocation |
| Token Theft | Steals authentication or authorization tokens | Secure token storage and short lifetimes |
| Pass-the-Hash | Reuses captured password hashes in certain authentication environments | Credential protection and protocol hardening |
| Kerberoasting | Requests service tickets and attempts offline cracking of service-account credentials in Active Directory environments | Strong service-account secrets, managed service accounts, and monitoring |

### Important: Authentication vs Session Security

A user may authenticate successfully, but an attacker can still gain access by stealing an active session.

For example:

1. A user signs in successfully.
2. The application creates a session.
3. An attacker steals the session cookie.
4. The attacker reuses the valid session.

The attacker may not need the original password or MFA code.

This is why secure authentication must be combined with secure session management.

---

## 7. 🧪 Hands-on Lab: Explore Authentication Mechanisms

The following exercises help you understand authentication through practical examples.

Use your own test accounts, local scripts, or authorized training environments.

### Task 1: Understand Password Hashing

Create a Python file named `password_hash.py`.

```python
import hashlib

password = input("Enter a password: ")

hashed = hashlib.sha256(
    password.encode()
).hexdigest()

print("Password Hash:", hashed)
```

Run:

```bash
python password_hash.py
```

Try entering the same password twice.

**Observe:**
- The same input produces the same SHA-256 hash.
- The hash is not the original password.
- SHA-256 is a general-purpose hash, not a suitable standalone password-storage algorithm.

### Task 2: Compare Passwords Using bcrypt

Install bcrypt:

```bash
pip install bcrypt
```

Create `bcrypt_demo.py`:

```python
import bcrypt

password = input("Create a password: ").encode()

hashed = bcrypt.hashpw(
    password,
    bcrypt.gensalt()
)

print("Stored Hash:", hashed.decode())

login = input("Enter password to login: ").encode()

if bcrypt.checkpw(login, hashed):
    print("Authentication Successful")
else:
    print("Authentication Failed")
```

Run:

```bash
python bcrypt_demo.py
```

**Observe:**
- bcrypt generates a salted password hash.
- Hashing the same password again can produce a different stored hash because a new salt is generated.
- `checkpw()` verifies a password against the stored hash.

This is a local demonstration, not a complete production authentication system.

### Task 3: Understand MFA

Create a test account on a service that supports MFA.

1. Enable MFA.
2. Compare password-only login with MFA-protected login.
3. Observe how the additional verification step works.
4. Review available recovery methods.
5. Check whether the service supports security keys or passkeys.

Do not share your OTP, recovery codes, or security keys with anyone.

### Task 4: Explore Single Sign-On

Use a service that supports SSO, or explore a local identity-provider training environment.

Investigate:

1. What is the identity provider?
2. What is the service provider?
3. Does the application redirect you to the identity provider?
4. What happens after successful authentication?
5. How does the application establish its own session?

### Task 5: Understand Identity Federation

Research a sample SAML or OIDC login flow.

Identify:
- The identity provider.
- The application or relying party.
- The trust relationship.
- The identity assertion or token.
- How the application validates the response.

Never copy real access tokens, ID tokens, or session cookies into public notes.

---

## 8. 🧠 Common Authentication Misconceptions

| Misconception | Correct Understanding |
|---|---|
| Authentication and authorization are the same | Authentication verifies identity; authorization controls permissions |
| Two passwords mean MFA | MFA requires different authentication factor categories |
| OAuth 2.0 is an authentication protocol | OAuth 2.0 is primarily an authorization framework |
| SSO automatically provides MFA | SSO and MFA solve different problems |
| Password hashing encrypts passwords | Hashing is a one-way transformation, unlike encryption |
| All OTP methods are equally secure | SMS, TOTP, and phishing-resistant methods have different security properties |
| Successful login means the session is secure | Session tokens can still be stolen or misused |
| Biometrics are always secure | Biometric systems have privacy, spoofing, and implementation risks |
| Passkeys send passwords to websites | Passkeys use public-key cryptography rather than transmitting a traditional password |

---

## 9. ⚡ Quick Revision

### Authentication Mechanisms at a Glance

| Mechanism | Main Purpose | Example |
|---|---|---|
| Password-Based Authentication | Verify identity using a secret | Username and password |
| MFA | Require multiple authentication factors | Password + security key |
| SSO | Access multiple applications through a shared login experience | Company login portal |
| Identity Federation | Establish trust between separate identity systems | University login for an external service |
| Passwordless Authentication | Authenticate without a traditional password | Passkeys |
| Risk-Based Authentication | Adjust verification based on risk | Additional verification from an unfamiliar device |

### Important Technologies

| Technology | Remember |
|---|---|
| Argon2id | Password hashing |
| bcrypt | Password hashing |
| TOTP | Time-based one-time passwords |
| FIDO2 | Strong public-key-based authentication |
| WebAuthn | Web authentication API supporting public-key credentials |
| SAML | Federated identity and enterprise SSO |
| OAuth 2.0 | Delegated authorization |
| OpenID Connect | Authentication and identity layer on OAuth 2.0 |
| Kerberos | Ticket-based network authentication |
| LDAP | Directory access and management |

### Key Takeaways

1. Password-based authentication verifies identity using secret credentials.
2. Passwords should be stored using dedicated password-hashing algorithms.
3. MFA reduces the risk of password-only account compromise.
4. SSO allows users to access multiple connected applications through a shared authentication experience.
5. Identity federation establishes trust between separate identity systems.
6. OAuth 2.0 handles delegated authorization, while OIDC provides authentication.
7. Passkeys use public-key cryptography and can resist phishing.
8. Risk-based authentication evaluates contextual risk before allowing access.
9. Authentication security must include session and token protection.
10. Modern authentication combines strong verification, secure identity management, and continuous monitoring.

---

## 10. 🎯 Active Recall & Exam Questions

Try answering these questions without looking at the notes.

1. What is password-based authentication?
2. Why should passwords be hashed instead of stored in plaintext?
3. What is the purpose of a salt in password hashing?
4. Why are Argon2id and bcrypt preferred over plain SHA-256 for password storage?
5. Explain the three authentication factors with examples.
6. What is the difference between MFA and 2FA?
7. What is TOTP, and how does it work?
8. What is an MFA fatigue attack?
9. Explain Single Sign-On and identify its main components.
10. What is the difference between an Identity Provider and a Service Provider?
11. What is identity federation?
12. Explain the differences between SAML, OAuth 2.0, and OpenID Connect.
13. How do passkeys use public-key cryptography?
14. What is risk-based authentication?
15. Why is session security important even after successful authentication?

<details>
<summary><strong>✅ Check Your Answers</strong></summary>

**1. Password-Based Authentication**

A method of verifying identity using a secret password associated with an account.

**2. Why hash passwords?**

Hashing avoids storing the original password. If the password database is exposed, attackers cannot immediately read plaintext passwords.

**3. Purpose of a salt**

A unique random value added before password hashing to make identical passwords produce different stored hashes and reduce the effectiveness of precomputed attacks.

**4. Argon2id and bcrypt**

They are purpose-built password-hashing algorithms designed to make password guessing more expensive than using a fast general-purpose hash such as SHA-256.

**5. Three authentication factors**

- Something you know: Password.
- Something you have: Security key.
- Something you are: Fingerprint.

**6. MFA vs 2FA**

MFA uses two or more different authentication factors. 2FA specifically uses two.

**7. TOTP**

Time-Based One-Time Password generates temporary codes using a shared secret and a time-based calculation.

**8. MFA Fatigue Attack**

An attacker repeatedly sends authentication approval requests, hoping the victim eventually approves one.

**9. SSO**

SSO allows users to authenticate through a shared identity system and access multiple connected applications. Its main components include the user, Identity Provider, and Service Provider.

**10. IdP vs SP**

The IdP authenticates the user. The SP provides the application or service that relies on the IdP.

**11. Identity Federation**

A trust relationship that allows one identity system to provide trusted identity information to another organization or application.

**12. SAML vs OAuth 2.0 vs OIDC**

- SAML: Federated identity and enterprise SSO.
- OAuth 2.0: Delegated authorization.
- OIDC: Authentication and identity information built on OAuth 2.0.

**13. Passkeys**

They use a public-private key pair. The private key remains protected by the user's device or credential provider, while the service verifies cryptographic signatures using the public key.

**14. Risk-Based Authentication**

A method that evaluates contextual risk, such as device, location, and behavior, to adjust authentication requirements.

**15. Session Security**

A stolen session token may allow an attacker to access an account without repeating the original authentication process. Secure session management helps reduce this risk.

</details>

---

## 🏁 Final Thought

Authentication is the first major security checkpoint between an identity and a protected resource.

But modern cybersecurity requires more than checking passwords.

A secure authentication system must **verify identities strongly, resist phishing, protect tokens and sessions, establish trust carefully, and continuously evaluate suspicious access behavior.**

> **Never rely on a single authentication mechanism to protect an entire digital environment. Build security through multiple layers of verification and control.**
