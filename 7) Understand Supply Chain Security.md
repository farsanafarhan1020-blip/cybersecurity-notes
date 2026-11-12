# 🔗 Understanding Software Supply Chain Security

Software Supply Chain Security focuses on protecting every component, tool, process, and organization involved in creating, distributing, and maintaining software.

Modern applications are rarely developed entirely from scratch. Developers depend on external libraries, frameworks, open-source packages, build tools, container images, cloud services, and software vendors.

These external components make development faster, but they also introduce security risks.

An attacker does not always need to attack an application's source code directly. Instead, they may compromise one of its dependencies, a software vendor, a package repository, or the build process.

**The central principle: An application is only as trustworthy as the software components and processes used to create and deliver it.**

---

# 1. ⚠️ Third-Party Software Risks

## What Is Third-Party Software?

Third-party software refers to software developed, maintained, or distributed by an external organization or individual rather than by the organization using it.

Examples include:

- Open-source libraries.
- External frameworks.
- Commercial software.
- APIs and SDKs.
- Database systems.
- Container images.
- Development tools.
- Cloud services.
- Software plugins.

### Real-World Example

Imagine a company developing an e-commerce application using Python.

Instead of implementing everything from scratch, developers use:

```python
import requests
import flask
import sqlalchemy
```

These libraries provide functionality for HTTP communication, web application development, and database interaction.

However, the company does not directly control how these libraries are developed or maintained.

If one of these libraries contains a vulnerability, the application may inherit that security risk.

This is one of the fundamental challenges of software supply chain security.

## Common Third-Party Software Risks

### 1. Vulnerable Dependencies

A third-party library may contain a security vulnerability that attackers can exploit.

**Example:**

An application uses an outdated version of a web framework containing a known authentication bypass vulnerability.

If the application uses the affected functionality, attackers may be able to bypass authentication.

**Potential impact:**

- Unauthorized access.
- Sensitive information exposure.
- Remote code execution.
- Service disruption.

**Prevention:**

- Regularly scan dependencies.
- Monitor security advisories.
- Upgrade vulnerable packages.
- Remove unnecessary dependencies.
- Validate updates before production deployment.

### 2. Malicious Packages

Attackers sometimes publish malicious packages that appear legitimate.

These packages may imitate popular libraries or use names similar to trusted packages.

This technique is commonly associated with **typosquatting**.

**Example:**

Imagine a developer intends to install:

```bash
pip install requests
```

But accidentally installs a similarly named malicious package:

```bash
pip install requestss
```

The malicious package may contain code designed to:

- Steal credentials.
- Read sensitive files.
- Establish unauthorized network connections.
- Download additional malware.
- Execute unwanted commands.

This is why package names and sources must be verified before installation.

### 3. Dependency Confusion

Dependency confusion occurs when an attacker exploits package-management behavior to make a build system download an unintended package instead of the legitimate internal package.

Imagine an organization maintains a private package:

```text
company-authentication
```

An attacker publishes a public package with the same name.

If the organization's package configuration is incorrectly designed, the build system may download the attacker's package instead of the internal one.

**Potential impact:**

- Malicious code execution during installation.
- Credential theft.
- Build environment compromise.
- Unauthorized access to internal systems.

**Prevention:**

- Configure trusted package sources.
- Use private package registries correctly.
- Control package namespaces.
- Pin and verify dependencies.
- Restrict package installation permissions.

### 4. Compromised Maintainers or Accounts

Many open-source projects are maintained by individuals or small teams.

If an attacker compromises a maintainer's account, they may publish malicious updates to a legitimate package.

The package name may be completely genuine, making this attack harder to recognize.

**Prevention:**

- Monitor package releases.
- Verify package provenance where available.
- Use trusted maintainers and registries.
- Review unexpected dependency changes.
- Use automated security monitoring.

### 5. Compromised Software Vendors

Organizations often purchase software from external vendors.

If a vendor's development or distribution infrastructure is compromised, customers may receive malicious software updates.

This is a form of a **software supply chain attack**.

**Example:**

1. A company purchases monitoring software from a vendor.
2. Attackers compromise the vendor's software distribution process.
3. A malicious update is released.
4. Customers install the update because they trust the vendor.
5. The malicious code executes inside customer environments.

The attack reaches multiple organizations through a trusted software provider.

### 6. Unmaintained Dependencies

Some packages are abandoned by their developers.

They may no longer receive:

- Security updates.
- Bug fixes.
- Compatibility updates.
- Maintenance releases.

An application using an abandoned dependency may remain vulnerable for a long time.

**Prevention:**

- Check maintenance activity.
- Review release history.
- Monitor security advisories.
- Replace unsupported packages.
- Maintain an approved dependency inventory.

## Third-Party Risk Summary

| Risk | Description | Potential Impact |
|---|---|---|
| Vulnerable Dependency | Package contains a known vulnerability | Exploitation |
| Malicious Package | Package intentionally contains harmful code | Malware infection |
| Typosquatting | Attacker publishes a similarly named package | Accidental malicious installation |
| Dependency Confusion | Package manager retrieves an unintended package | Supply chain compromise |
| Compromised Maintainer | Legitimate package publisher is compromised | Malicious updates |
| Compromised Vendor | External software provider is attacked | Large-scale compromise |
| Unmaintained Software | Dependency no longer receives security updates | Long-term exposure |

---

# 2. 📦 Dependency Management

## What Is Dependency Management?

Dependency management is the process of identifying, selecting, installing, updating, tracking, and maintaining the external software components used by an application.

The objective is to ensure that applications use trusted, secure, and properly maintained dependencies.

A professional development team should know exactly which third-party components are included in its software.

## Understanding the Dependency Tree

Applications often contain more dependencies than developers realize.

Consider this example:

```text
                Your Application
                       |
             +---------+---------+
             |                   |
             v                   v
           Flask              Requests
             |                   |
        +----+----+              |
        |         |              v
        v         v           urllib3
     Werkzeug   Jinja2
        |
        v
   Other Packages
```

The application directly depends on Flask and Requests.

However, Flask and Requests also depend on other packages.

These additional packages are called **transitive dependencies**.

If a transitive dependency contains a vulnerability, your application may still be affected.

This is why dependency management must consider the complete dependency tree.

## Direct vs. Transitive Dependencies

| Direct Dependency | Transitive Dependency |
|---|---|
| Explicitly required by the application | Required by another dependency |
| Usually declared by the developer | Often installed automatically |
| Example: Flask | Example: Werkzeug |
| Can be managed directly | Must be tracked through the dependency tree |

## Dependency Versioning

Software packages are released in different versions.

For example:

```text
flask==2.0.0
flask==2.1.0
flask==2.2.0
flask==3.0.0
```

Different versions may contain:

- New features.
- Bug fixes.
- Performance improvements.
- Security patches.
- Breaking changes.

Using an outdated version may expose the application to known vulnerabilities.

However, automatically upgrading every dependency without testing may break application functionality.

A secure dependency management process must balance security and compatibility.

## What Is a Lock File?

A lock file records the exact versions of dependencies selected for a project, often including additional information such as hashes.

It helps developers and build systems install consistent dependency versions.

For example:

A Python project may use `requirements.txt` with pinned versions or a tool-specific lock file.

An npm project commonly uses:

```text
package.json
package-lock.json
```

The purpose is to make dependency installation more reproducible.

**Why is this important?**

Imagine two developers install the same application on different days.

Without controlled dependency versions, they might receive different package releases.

This can cause:

- Different application behavior.
- Unexpected compatibility problems.
- Inconsistent security results.
- Difficult-to-reproduce bugs.

Lock files help reduce these differences.

## Software Composition Analysis (SCA)

**SCA — Software Composition Analysis** is the process of identifying and examining third-party software components used by an application.

SCA tools help security teams understand the software supply chain.

They can:

1. Identify installed dependencies.
2. Detect known vulnerable versions.
3. Examine direct and transitive dependencies.
4. Identify outdated packages.
5. Provide remediation recommendations.
6. Generate software component inventories.

### Common SCA Tools

| Tool | Purpose |
|---|---|
| Dependabot | Detects outdated and vulnerable dependencies and proposes updates |
| Snyk | Identifies vulnerabilities in dependencies and other software components |
| OWASP Dependency-Check | Detects publicly known vulnerabilities in dependencies |
| pip-audit | Audits Python packages for known vulnerabilities |
| npm audit | Checks npm dependency trees for known vulnerabilities |
| Trivy | Scans dependencies, container images, filesystems, and configurations |

## Practical Example: Dependency Scanning with pip-audit

Imagine you are developing a Python application.

First, install the tool:

```bash
pip install pip-audit
```

Run a scan:

```bash
pip-audit
```

The tool examines installed Python packages and checks them against known vulnerability information.

An illustrative finding might look like:

```text
Package: example-library
Installed Version: 1.0.0

Vulnerability: CVE-2024-XXXX

Fixed Version: 1.2.0

Status: Vulnerable
```

This is an example of the information a scanner may report, not an actual scan result.

### What Should a Security Engineer Do?

When a vulnerability is identified:

1. Identify the affected package.
2. Determine whether the application uses the affected functionality.
3. Check the vulnerability's severity and technical details.
4. Identify a patched version.
5. Review compatibility requirements.
6. Upgrade the dependency.
7. Run application tests.
8. Repeat the security scan.
9. Document the result.

**Important:** A scanner finding does not automatically mean the application is exploitable. The security engineer must understand the affected code, configuration, and application context.

## What Is an SBOM?

**SBOM — Software Bill of Materials**

An SBOM is a structured inventory of the software components included in an application.

Think of an SBOM as an ingredient list for software.

A food manufacturer lists the ingredients used in a product. Similarly, an SBOM lists the software components used to create an application.

An SBOM may contain:

- Component names.
- Package versions.
- Suppliers.
- Dependency relationships.
- License information.
- Unique component identifiers.

### Why Is an SBOM Important?

Imagine a security vulnerability is discovered in a popular open-source library.

An organization with an SBOM can search its software inventory and identify which applications contain that library.

Without an SBOM, identifying affected applications may require manually examining many repositories and systems.

### Common SBOM Formats

| Format | Description |
|---|---|
| SPDX | Standard format for communicating software component and licensing information |
| CycloneDX | Standard format for describing software component inventories and related supply-chain information |

## Dependency Management Best Practices

- Use trusted package repositories.
- Avoid unnecessary dependencies.
- Maintain lock files.
- Monitor security advisories.
- Scan direct and transitive dependencies.
- Update vulnerable packages.
- Review dependency maintenance activity.
- Generate and maintain SBOMs.
- Verify package integrity and provenance.
- Test dependency updates before production deployment.

---

# 3. 🔐 Software Integrity

## What Is Software Integrity?

Software integrity means ensuring that software, its components, and its distribution artifacts have not been modified in unauthorized or unexpected ways.

In cybersecurity, integrity is one of the three principles of the CIA Triad:

- Confidentiality.
- Integrity.
- Availability.

Software integrity focuses particularly on protecting software from unauthorized modification.

For example, imagine a company downloads a legitimate software package from an official repository.

An attacker modifies the package before it reaches the organization.

If the organization installs the modified package, it may unknowingly execute malicious code.

Software integrity controls help detect or prevent this situation.

## Why Is Software Integrity Important?

Software integrity helps ensure:

- Source code has not been modified without authorization.
- Dependencies are obtained from trusted sources.
- Build artifacts have not been tampered with.
- Software updates are authentic.
- Deployment packages match approved releases.
- Software distribution processes remain trustworthy.

## 1. Cryptographic Hashing

A cryptographic hash function converts data into a fixed-length value.

For example, SHA-256 produces a 256-bit hash.

Even a small modification to a file will normally produce a different hash.

### Example

Suppose a company downloads:

```text
application.zip
```

It calculates the file's SHA-256 hash:

```bash
sha256sum application.zip
```

Example output:

```text
a1b2c3d4... application.zip
```

The company compares this result with the expected hash obtained through a trusted channel.

If the values match, it provides evidence that the file has not changed since the trusted hash was generated.

If the values differ, the file may have been modified or corrupted.

### Important Limitation

A hash alone does not prove who created a file.

If an attacker can modify both the software and the expected hash, they may replace both values.

Therefore, the expected hash must come from a trusted source.

## 2. Digital Signatures

Digital signatures use asymmetric cryptography to help verify the authenticity and integrity of software.

They involve two keys:

- Private key.
- Public key.

### How Digital Signing Works

A simplified process:

```text
          Software Artifact
                 |
                 v
          Generate Signature
           Using Private Key
                 |
                 v
          Signed Software
                 |
                 v
        Distribute Software
                 |
                 v
       Verify Using Public Key
                 |
          +------+------+
          |             |
          v             v
       Valid          Invalid
          |             |
          v             v
     Accept After    Reject or
     Other Checks    Investigate
```

The software publisher uses its private signing key to sign the artifact.

The recipient uses the corresponding trusted public key to verify the signature.

A valid signature provides evidence that:

1. The artifact matches the content that was signed.
2. The signature was created using the corresponding private key.

However, a valid signature does not automatically prove that the software is safe. A legitimate publisher's signing key or build process could itself be compromised.

## 3. Package Integrity Verification

Package managers and software repositories may use hashes and signatures to verify downloaded packages.

This helps protect against:

- Modified packages.
- Corrupted downloads.
- Unauthorized package replacement.
- Certain repository compromise scenarios.

For example, Linux package managers commonly use repository signing mechanisms to verify package metadata and authenticity.

## 4. Build Provenance

Build provenance describes how a software artifact was created.

It may contain information about:

- Source repository.
- Source commit.
- Build workflow.
- Build environment.
- Build tools.
- Dependencies.
- Generated artifact.

Provenance helps organizations determine whether an artifact was produced through an expected and trusted build process.

### Example

A company expects its production application to be built from:

```text
Repository: company/web-app
Branch: main
Commit: 8f32abc
Workflow: production-build
```

If a deployment artifact claims to come from a different repository or unexpected commit, the security team should investigate.

## 5. Reproducible Builds

A reproducible build is a build process in which independent builds using the same specified inputs and environment can produce identical or verifiably equivalent outputs.

This helps improve confidence in the build process.

For example:

1. Developer A builds an application.
2. Developer B independently builds the same source code using the same controlled inputs.
3. The resulting artifacts are compared.
4. Unexpected differences are investigated.

Reproducible builds can help identify unexpected modifications introduced during compilation or packaging.

Not every software project can achieve perfectly reproducible builds without additional engineering work.

## Software Integrity vs. Software Authenticity

These concepts are related but different.

| Software Integrity | Software Authenticity |
|---|---|
| Focuses on whether software has been modified unexpectedly | Focuses on whether software comes from the claimed source |
| Hashes can help detect changes | Digital signatures can help verify the publisher |
| Detects unexpected modifications | Helps establish origin and trust |
| Does not identify the creator by itself | Depends on trustworthy identity and key management |

**Professional point:** Strong software supply-chain security combines integrity verification, trusted identities, signatures, provenance, and controlled distribution.

---

# 4. 📡 Security Monitoring

## What Is Supply Chain Security Monitoring?

Security monitoring in the software supply chain is the continuous observation of software components, development activities, repositories, build systems, package sources, and external vendors to detect suspicious changes and emerging threats.

Supply chain security does not end after software is installed.

A dependency that was safe yesterday may become vulnerable tomorrow.

A trusted package maintainer's account may be compromised.

A new malicious release may appear in a package repository.

Continuous monitoring helps organizations discover these changes.

## What Should Organizations Monitor?

### 1. Dependency Changes

Monitor changes in:

- Package versions.
- Dependency sources.
- New dependencies.
- Removed dependencies.
- Unexpected package updates.

**Example:**

A developer unexpectedly adds a package from an unfamiliar repository.

A security monitoring system may flag the change for review.

### 2. Repository Activity

Monitor activities such as:

- Unauthorized code modifications.
- Changes to dependency files.
- Changes to CI/CD workflow configurations.
- Unexpected branch modifications.
- Suspicious commits.
- Unauthorized repository access.

### 3. Build System Activity

Monitor:

- Unexpected build execution.
- Changes to build scripts.
- Unusual network connections.
- Unexpected credential access.
- Changes to build artifacts.
- Unauthorized runner activity.

### 4. Package Repository Activity

Monitor:

- New package releases.
- Unexpected package ownership changes.
- Suspicious package versions.
- Vulnerability advisories.
- Package removal or replacement.
- Changes in package integrity information.

### 5. Third-Party Vendor Activity

Organizations should monitor external vendors and software providers.

Important areas include:

- Security advisories.
- Reported breaches.
- Product vulnerabilities.
- Software update notifications.
- End-of-life announcements.
- Changes in vendor security practices.

## Software Vulnerability Monitoring

A common monitoring activity is checking whether installed dependencies become affected by newly disclosed vulnerabilities.

For example:

```text
Current Application
        |
        v
Dependency Inventory
        |
        v
Monitor Security Advisories
        |
        v
New Vulnerability Discovered
        |
        v
Identify Affected Packages
        |
        v
Assess Application Exposure
        |
        v
Update or Mitigate
```

## Common Monitoring Tools

| Tool | Purpose |
|---|---|
| Dependabot | Monitors dependencies and proposes updates |
| Snyk | Identifies software vulnerabilities |
| GitHub Security Advisories | Provides vulnerability information |
| Trivy | Scans software components and container images |
| Wazuh | Security monitoring and threat detection |
| Splunk | Security log analysis and monitoring |
| Elastic Security | Security analytics and threat detection |
| OSV | Provides open-source vulnerability information |

## What Is OSV?

**OSV — Open Source Vulnerabilities**

OSV is a vulnerability database and ecosystem for identifying vulnerabilities in open-source software.

It provides information about affected package versions and vulnerable code ranges.

Security tools can use vulnerability databases such as OSV to identify affected dependencies.

## What Is Vulnerability Intelligence?

Vulnerability intelligence involves collecting and analyzing information about known vulnerabilities, affected software, exploitation activity, and available remediation.

Sources include:

- CVE records.
- National Vulnerability Database (NVD).
- OSV.
- Vendor security advisories.
- CERT advisories.
- Security research publications.

This information helps organizations prioritize security updates.

## Monitoring Example: Suspicious Dependency Update

Imagine a developer submits a pull request that changes:

```text
requests==2.x
```

to:

```text
requests==new-version
```

The security team should examine:

1. Is the new version legitimate?
2. Did it come from a trusted package source?
3. Are there unexpected new dependencies?
4. Does the release have known vulnerabilities?
5. Does the update require additional permissions?
6. Does the package have appropriate provenance information?

This is a simple example of how dependency monitoring can support supply chain security.

---

# 5. 🛡️ Risk Mitigation

## What Is Supply Chain Risk Mitigation?

Risk mitigation is the process of reducing the likelihood or impact of security threats affecting the software supply chain.

Organizations cannot completely eliminate every supply chain risk.

However, they can introduce controls that reduce exposure and improve their ability to detect and respond to attacks.

The objective is to make the software supply chain more trustworthy, resilient, and manageable.

## Understanding Supply Chain Risk

Consider an application that depends on 100 external packages.

Each package introduces some level of dependency risk.

Potential issues include:

- Vulnerable versions.
- Malicious packages.
- Unmaintained software.
- Compromised maintainers.
- Untrusted distribution sources.
- License and compliance concerns.

The organization must identify which risks matter most and prioritize appropriate security controls.

## Major Risk Mitigation Strategies

### 1. Use Trusted Sources

Install software only from trusted package repositories and verified publishers.

Examples:

- Official Python Package Index (PyPI).
- Official npm registry.
- Trusted Linux distribution repositories.
- Approved private package registries.
- Verified vendor download channels.

Avoid downloading random packages from unfamiliar websites.

### 2. Apply Least Privilege

Developers, build systems, and deployment services should receive only the permissions necessary for their tasks.

For example:

A build pipeline that only needs to read source code should not receive administrator access to production infrastructure.

This limits the damage if the pipeline is compromised.

### 3. Maintain an SBOM

Maintain an accurate Software Bill of Materials for applications.

An SBOM helps organizations:

- Identify software components.
- Track dependency versions.
- Find affected applications.
- Respond to newly disclosed vulnerabilities.
- Understand software supply-chain exposure.

### 4. Verify Software Integrity

Use:

- Cryptographic hashes.
- Digital signatures.
- Trusted package metadata.
- Build provenance.
- Artifact verification.

These controls help identify unauthorized changes and verify software origin.

### 5. Regular Vulnerability Scanning

Scan dependencies, container images, and application components regularly.

Do not rely only on a scan performed during initial development.

New vulnerabilities may be discovered after deployment.

### 6. Secure the Build Environment

Protect the systems that compile, package, and distribute software.

Use:

- Isolated runners.
- Restricted permissions.
- Trusted build tools.
- Protected build configurations.
- Artifact signing.
- Build logging.

### 7. Vendor Risk Management

Before using third-party software or services, organizations should assess the security risks associated with the provider.

Questions to consider:

- Does the vendor provide security updates?
- Does the vendor have a vulnerability disclosure process?
- Is the software actively maintained?
- Does the vendor publish security advisories?
- How are customer data and credentials protected?
- What happens if the vendor experiences a security breach?

Vendor risk management is especially important when external software has privileged access to internal systems.

### 8. Incident Response Planning

Organizations should prepare for situations in which a dependency, vendor, or build system is compromised.

A simplified response process:

```text
Supply Chain Threat Detected
           |
           v
      Investigate
           |
           v
   Identify Affected Systems
           |
           v
       Containment
           |
           v
 Remove or Disable Affected Component
           |
           v
     Apply Remediation
           |
           v
      Verify Security
           |
           v
       Recovery
           |
           v
      Lessons Learned
```

For example, if a malicious dependency is discovered:

1. Identify applications using it.
2. Stop new builds using the affected version.
3. Determine whether it executed in existing environments.
4. Investigate potential credential exposure.
5. Remove or replace the dependency.
6. Rotate potentially exposed credentials.
7. Rebuild and redeploy trusted artifacts.
8. Document the incident.

## Supply Chain Risk Treatment

Organizations generally use four broad approaches to risk treatment.

| Strategy | Meaning | Example |
|---|---|---|
| Avoid | Stop the activity that creates unacceptable risk | Remove an unnecessary, untrusted dependency |
| Mitigate | Reduce the likelihood or impact | Scan dependencies and use trusted package sources |
| Transfer | Shift some financial or contractual consequences | Cyber insurance or contractual vendor obligations |
| Accept | Formally acknowledge and retain the risk | Accept a low-risk issue with documented justification |

**Important:** Risk transfer does not eliminate the technical security risk. For example, insurance may cover certain financial losses, but it does not prevent a supply chain attack.

---

# 🏢 Real-World Scenario: A Compromised Open-Source Dependency

Imagine a company developing a banking application.

The application uses a popular open-source package for processing user authentication requests.

One day, attackers compromise the package maintainer's account.

They publish a malicious package update containing code that attempts to steal environment variables.

The organization automatically updates its dependencies and includes the malicious package in its next build.

### What Happens?

```text
Trusted Open-Source Package
             |
             v
     Maintainer Account
        Compromised
             |
             v
      Malicious Update
             |
             v
      Package Repository
             |
             v
       Company Build
             |
             v
      Malicious Code
         Included
             |
             v
       Application
        Deployed
             |
             v
    Potential Credential Theft
```

### How Could the Organization Prevent or Reduce the Impact?

| Security Control | How It Helps |
|---|---|
| Dependency Monitoring | Detects unexpected package changes |
| Version Locking | Prevents uncontrolled dependency updates |
| Package Verification | Helps establish package integrity and authenticity |
| SCA Scanning | Identifies known vulnerabilities |
| SBOM | Identifies applications containing the package |
| Build Isolation | Limits access available to malicious installation code |
| Secret Management | Reduces unnecessary credential exposure |
| Runtime Monitoring | Helps detect suspicious behavior |
| Incident Response | Supports containment and recovery |

**Important:** No single control guarantees protection. Effective supply chain security requires multiple defensive layers.

---

# 🔄 Understanding the Complete Software Supply Chain

To understand supply chain security professionally, you must understand how software moves from development to the end user.

```text
       Developer
           |
           v
     Source Code
           |
           v
   Third-Party Dependencies
           |
           v
      Build System
           |
           v
    Build Artifacts
           |
           v
    Package Registry
           |
           v
       Deployment
           |
           v
    Production System
           |
           v
        End User
```

Security must be maintained at every stage.

| Stage | Potential Risk | Security Control |
|---|---|---|
| Development | Unauthorized code changes | Code review and repository protection |
| Dependencies | Vulnerable or malicious packages | SCA and trusted repositories |
| Build | Compromised build environment | Isolated runners and access restrictions |
| Artifacts | Unauthorized modifications | Hash verification and digital signatures |
| Distribution | Malicious package replacement | Trusted registries and provenance |
| Deployment | Insecure configurations | Deployment security checks |
| Production | Newly discovered vulnerabilities | Continuous monitoring and patching |

---

# 🧪 Practical Lab: Introduction to Supply Chain Security

These exercises will help you understand how security professionals identify and manage software supply chain risks.

## Lab 1: Dependency Discovery

**Objective:** Identify which third-party packages your Python project uses.

Inside a Python project, run:

```bash
pip list
```

This displays installed packages.

To examine dependency relationships, install:

```bash
pip install pipdeptree
```

Run:

```bash
pipdeptree
```

Observe:

- Which packages are directly required?
- Which packages are transitive dependencies?
- Which packages depend on other packages?
- Are there unnecessary dependencies?

## Lab 2: Dependency Vulnerability Scanning

**Objective:** Identify known vulnerabilities in installed Python packages.

Install:

```bash
pip install pip-audit
```

Run:

```bash
pip-audit
```

Investigate any findings.

Record:

- Package name.
- Installed version.
- Vulnerability identifier.
- Affected version range.
- Fixed version.
- Recommended remediation.

Remember that a vulnerability report must be interpreted in the context of the application.

## Lab 3: Software Integrity Verification

**Objective:** Understand how cryptographic hashes help detect file modifications.

Create a file:

```bash
echo "Trusted software package" > application.txt
```

Generate its SHA-256 hash:

```bash
sha256sum application.txt
```

Now modify the file:

```bash
echo "Modified content" >> application.txt
```

Generate the hash again:

```bash
sha256sum application.txt
```

Compare the two hashes.

**Observation:** Even a small content change should produce a different SHA-256 hash, demonstrating how hashes can detect modifications.

## Lab 4: Generate an SBOM with Trivy

**Objective:** Understand how an SBOM identifies software components.

Install Trivy using its official installation instructions.

Scan a local project directory:

```bash
trivy fs --format cyclonedx --output sbom.json .
```

This attempts to generate an SBOM in CycloneDX format.

Examine:

- Component names.
- Package versions.
- Dependency information.
- Component identifiers.

The exact output depends on the files and package manifests present in your project.

## Lab 5: Secret and Repository Security

**Objective:** Identify accidental secret exposure and understand repository protection.

Use Gitleaks to scan your own Git repository:

```bash
gitleaks git .
```

Then review your repository security settings.

Check:

- Are important branches protected?
- Are code reviews required?
- Are GitHub Actions permissions restricted?
- Are secrets stored outside source code?
- Are dependency updates monitored?

Use synthetic credentials only. Never create real exposed credentials for practice.

---

# 📋 Supply Chain Security Assessment Report

Security professionals document supply chain risks and their mitigation.

Use this structure when preparing a basic assessment.

| Field | Description |
|---|---|
| Finding ID | Unique identifier |
| Component | Affected package, library, vendor, or build system |
| Risk | Identified supply chain weakness |
| Severity | Critical, High, Medium, or Low |
| Evidence | Technical information supporting the finding |
| Impact | Potential consequences |
| Recommendation | Proposed mitigation |
| Owner | Responsible team |
| Status | Open, In Progress, Fixed, or Verified |

### Example Finding

| Field | Example |
|---|---|
| Finding ID | SC-001 |
| Component | Python Dependency |
| Risk | Outdated third-party package |
| Severity | Medium (illustrative) |
| Evidence | Dependency scanner identifies a known vulnerability in the installed version. |
| Impact | The vulnerable component may expose application functionality to exploitation. |
| Recommendation | Upgrade to a supported fixed version and validate compatibility. |
| Status | Fixed |
| Validation | Dependency scan repeated successfully and application tests passed. |

---

# 🧠 Quick Revision

| Concept | Remember This |
|---|---|
| Software Supply Chain | All components and processes involved in creating and delivering software |
| Third-Party Software | Software developed or maintained by external providers |
| Supply Chain Attack | Attack that compromises software or a component in its delivery chain |
| Malicious Package | Software package intentionally containing harmful code |
| Typosquatting | Using similar package names to trick users into installing malicious packages |
| Dependency Confusion | Exploiting package resolution to make systems retrieve unintended packages |
| Dependency Management | Tracking, maintaining, and securing software dependencies |
| Direct Dependency | Package directly required by an application |
| Transitive Dependency | Package required by another dependency |
| SCA | Software Composition Analysis |
| SBOM | Software Bill of Materials |
| Software Integrity | Ensuring software has not been modified without authorization |
| Cryptographic Hash | Fixed-length value used to help verify data integrity |
| Digital Signature | Cryptographic mechanism used to verify authenticity and integrity |
| Build Provenance | Information describing how an artifact was produced |
| Security Monitoring | Continuously observing software components and activities |
| Risk Mitigation | Reducing the likelihood or impact of supply chain threats |
| Vendor Risk Management | Assessing and managing risks from external software providers |

---

# 🔑 Key Takeaways

1. Modern applications depend on third-party software, making supply chain security essential.
2. A vulnerability in an external dependency can affect every application that uses it.
3. Malicious packages, typosquatting, dependency confusion, and compromised maintainers are important supply chain threats.
4. Dependency management requires tracking direct and transitive dependencies.
5. SCA tools help identify vulnerable software components.
6. SBOMs provide visibility into the components used by an application.
7. Cryptographic hashes help detect unexpected modifications.
8. Digital signatures help verify software authenticity and integrity.
9. Build provenance provides information about how software artifacts were created.
10. Continuous monitoring helps detect newly discovered vulnerabilities and suspicious dependency changes.
11. Least privilege, trusted repositories, secure builds, and vendor assessments reduce supply chain risk.
12. Effective supply chain security requires multiple layers of protection rather than relying on one tool.

---

# 🎯 Active Recall — Test Your Understanding

Try answering these questions without looking at the notes.

<details>
<summary><strong>1. What is Software Supply Chain Security?</strong></summary>

It is the practice of protecting all components, tools, processes, and external providers involved in developing, building, distributing, and maintaining software.
</details>

<details>
<summary><strong>2. Why are third-party dependencies a security risk?</strong></summary>

Third-party dependencies may contain vulnerabilities, malicious code, outdated components, or compromised updates. Applications inherit risks from the software they depend on.
</details>

<details>
<summary><strong>3. What is typosquatting?</strong></summary>

Typosquatting is an attack in which an attacker creates a package with a name similar to a legitimate package to trick developers into installing malicious software.
</details>

<details>
<summary><strong>4. What is dependency confusion?</strong></summary>

Dependency confusion is an attack that exploits package resolution behavior to make a build system retrieve an unintended package instead of the legitimate internal dependency.
</details>

<details>
<summary><strong>5. What is the difference between direct and transitive dependencies?</strong></summary>

Direct dependencies are explicitly required by an application. Transitive dependencies are required by other dependencies.
</details>

<details>
<summary><strong>6. What is SCA?</strong></summary>

Software Composition Analysis identifies and examines third-party software components to discover vulnerabilities, outdated packages, and other dependency risks.
</details>

<details>
<summary><strong>7. What is an SBOM, and why is it useful?</strong></summary>

A Software Bill of Materials is an inventory of software components. It helps organizations identify which applications contain a component affected by a newly discovered vulnerability.
</details>

<details>
<summary><strong>8. What is the difference between hashing and digital signatures?</strong></summary>

Hashing produces a fixed-length value that helps detect data changes. Digital signatures use cryptographic keys to help verify the authenticity and integrity of software.
</details>

<details>
<summary><strong>9. What is software provenance?</strong></summary>

Software provenance describes where an artifact came from and how it was produced, including its source, build process, and relevant inputs.
</details>

<details>
<summary><strong>10. Why is continuous supply chain monitoring important?</strong></summary>

Because new vulnerabilities, malicious package releases, compromised maintainers, and vendor security incidents can appear after software has already been developed and deployed.
</details>

<details>
<summary><strong>11. What should an organization do if a dependency is discovered to be malicious?</strong></summary>

Identify affected applications, stop using the malicious component, investigate possible execution and credential exposure, replace the dependency, rotate potentially exposed credentials, rebuild trusted artifacts, and validate the remediation.
</details>

<details>
<summary><strong>12. Does using a digitally signed package guarantee that it is safe?</strong></summary>

No. A valid signature helps establish that the package matches what was signed and was signed by the holder of the corresponding private key. However, the publisher, signing key, or build process could itself be compromised.
</details>

---

## 💡 Professional Mindset

A cybersecurity professional must look beyond the application's own source code.

When evaluating software supply chain security, ask:

- Where did this dependency come from?
- Who maintains it?
- Is it actively maintained?
- Does it contain known vulnerabilities?
- Could the package have been replaced or compromised?
- Can we verify its integrity and authenticity?
- Do we know every component included in our application?
- Can we identify all applications affected by a newly discovered vulnerability?
- What happens if a trusted vendor is compromised?

**Remember:** You may trust your own code, but your application also depends on code written by other people and organizations. Supply chain security is about making that entire chain trustworthy.
