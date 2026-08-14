# 👥 Understand User & Permission Management

> **"Linux security starts with controlling who can access the system, what they can access, and what they are allowed to do."**

---

# 👤 What is User & Permission Management?

**User & Permission Management** is the process of controlling user identities, groups, file access, ownership, and privileges in a Linux system.

It answers three important questions:

```text
WHO are you?
     ↓
WHAT can you access?
     ↓
WHAT are you allowed to do?
```

This is one of the most important parts of Linux security because even if an attacker gains access to a system, properly configured permissions can limit what they can do.

---

# 👤 A. User Accounts

## 📖 What is a User Account?

A **user account** represents an identity on a Linux system.

A user account can be used to:

- Log in to the system
- Own files
- Run programs
- Access resources
- Perform administrative tasks

Each user normally has information such as:

- Username
- User ID (UID)
- Primary group
- Home directory
- Login shell
- Password/authentication information

---

## 🔢 User ID (UID)

Linux internally identifies users using a **UID**.

Example:

```text
Username: farhan
UID:      1000
```

The username is easier for humans to remember, while Linux uses the UID internally.

---

## 🏠 Home Directory

Normal users usually have a personal directory under:

```text
/home/
```

Example:

```text
/home/farhan
```

This is where the user's personal files and configuration are commonly stored.

---

## 🔍 Useful Commands

### Current user

```bash
whoami
```

### User information

```bash
id
```

### List logged-in users

```bash
who
```

or:

```bash
w
```

---

## 👤 Creating a User

On many Linux distributions:

```bash
sudo useradd -m farhan
```

Set a password:

```bash
sudo passwd farhan
```

A more interactive command on Debian-based systems is:

```bash
sudo adduser farhan
```

---

## 🗑️ Removing a User

```bash
sudo userdel farhan
```

To remove the user's home directory as well:

```bash
sudo userdel -r farhan
```

⚠️ Be careful when deleting accounts because their files and data may also be removed.

---

# 🛡️ Why User Accounts Matter in Security

Each person should ideally use their **own account** rather than sharing credentials.

This provides:

- Accountability
- Access control
- Better auditing
- Easier incident investigation

### Example

Instead of:

```text
Everyone → admin
```

Use:

```text
Alice → employee account
Bob   → employee account
Admin → administrator account
```

If suspicious activity occurs, logs can help identify which account was involved.

---

# 👥 B. Groups

## 📖 What is a Group?

A **group** is a collection of users.

Groups make permission management easier because permissions can be assigned to an entire group instead of individual users.

Example:

```text
Developers
├── Alice
├── Bob
└── Charlie
```

All developers can be given access to the same project files.

---

## 🔍 View Groups

```bash
groups
```

For a specific user:

```bash
groups farhan
```

More detailed information:

```bash
id farhan
```

---

## ➕ Create a Group

```bash
sudo groupadd developers
```

---

## ➕ Add a User to a Group

```bash
sudo usermod -aG developers farhan
```

The `-aG` combination is important because it adds the user to the group without removing their existing supplementary group memberships.

---

## ➖ Remove a User from a Group

On many Linux distributions:

```bash
sudo gpasswd -d farhan developers
```

---

# 🌍 Real-World Example

Imagine a company has:

```text
Developers
    │
    ├── Alice
    ├── Bob
    └── Charlie
```

Project files belong to:

```text
developers
```

Instead of giving each person individual permissions, the administrator gives the **developers group** access.

This makes management much easier.

---

# 🔐 C. File Permissions

## 📖 What are File Permissions?

Linux permissions determine **who can read, modify, or execute a file**.

Linux commonly uses three permission categories:

```text
Owner
Group
Others
```

And three basic permissions:

```text
r = Read
w = Write
x = Execute
```

---

# 📊 Permission Structure

Consider:

```text
-rwxr-xr--
```

Break it down:

```text
- | rwx | r-x | r--
    │     │     │
  Owner  Group Others
```

### Owner

```text
rwx
```

Can:

- Read
- Write
- Execute

### Group

```text
r-x
```

Can:

- Read
- Execute

Cannot:

- Write

### Others

```text
r--
```

Can:

- Read

Cannot:

- Write
- Execute

---

# 📖 Meaning of Permissions

| Permission | Meaning for File |
|---|---|
| `r` | Read contents |
| `w` | Modify contents |
| `x` | Execute the file |

For directories, permissions have slightly different meanings:

| Permission | Meaning for Directory |
|---|---|
| `r` | List directory contents |
| `w` | Create/delete entries |
| `x` | Access/traverse the directory |

---

# 🔍 Viewing Permissions

Use:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 farhan developers 1250 notes.txt
```

This tells us:

```text
Permissions → -rw-r--r--
Owner       → farhan
Group       → developers
File        → notes.txt
```

---

# 🔢 Numeric Permissions

Linux can also represent permissions using numbers.

```text
r = 4
w = 2
x = 1
```

Add them together:

```text
rwx = 4 + 2 + 1 = 7

rw- = 4 + 2 = 6

r-x = 4 + 1 = 5

r-- = 4
```

Therefore:

```text
755
```

means:

```text
Owner  → 7 → rwx
Group  → 5 → r-x
Others → 5 → r-x
```

---

# 🔧 Changing Permissions

The command used is:

```bash
chmod
```

Example:

```bash
chmod 755 script.sh
```

This gives:

```text
Owner  → rwx
Group  → r-x
Others → r-x
```

Another example:

```bash
chmod 600 private.txt
```

Means:

```text
Owner  → rw-
Group  → ---
Others → ---
```

This is useful for sensitive files.

---

# 🛡️ Why Permissions Matter

Incorrect permissions can expose sensitive information.

For example:

```text
passwords.txt
```

with:

```text
-rw-r--r-- 
```

allows other users to read it.

For sensitive information, a more restrictive permission may be appropriate, depending on the application's requirements.

---

# 👑 D. Ownership

## 📖 What is Ownership?

Every Linux file and directory normally has:

- An owner
- An associated group

Example:

```text
-rw-r-----  farhan  developers  project.txt
```

Here:

```text
Owner → farhan
Group → developers
```

Permissions are then evaluated based on whether the accessing user is:

```text
Owner
   ↓
Group member
   ↓
Other user
```

---

# 🔍 Viewing Ownership

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 farhan developers 500 report.txt
```

---

# 🔄 Changing Ownership

The command is:

```bash
chown
```

Example:

```bash
sudo chown alice report.txt
```

Change both owner and group:

```bash
sudo chown alice:developers report.txt
```

---

## 📁 Changing Group Ownership

The command:

```bash
chgrp
```

Example:

```bash
sudo chgrp developers project.txt
```

---

# 🌍 Real-World Example

A company has a shared project directory:

```text
/opt/project
```

The administrator sets:

```text
Owner → projectadmin
Group → developers
```

Developers can work on project files, while unrelated users cannot modify them.

---

# 👑 E. Privilege Management

## 📖 What are Privileges?

**Privileges** determine what actions a user is allowed to perform on the system.

A normal user usually has limited permissions.

An administrator can perform powerful operations such as:

- Installing software
- Changing system configuration
- Managing users
- Starting/stopping services
- Accessing protected files

---

# 👑 Root Privileges

The **root user** has extensive control over the Linux system.

For example, root can potentially:

```text
Modify system files
Create/delete users
Change permissions
Install software
Stop services
Access protected data
```

Because of this power, root access must be carefully controlled.

---

# 🔑 `sudo`

`sudo` allows authorized users to execute specific commands with elevated privileges.

Example:

```bash
sudo systemctl restart ssh
```

The user remains logged in as themselves, but the command runs with elevated privileges if permitted by the system's sudo policy.

---

# ⚖️ Root vs Sudo

| Root | sudo |
|---|---|
| Superuser account | Controlled privilege elevation |
| Very broad privileges | Can be restricted by policy |
| High risk if misused | Better accountability |
| Usually avoided for routine work | Common for administration |

---

# 🛡️ Principle of Least Privilege

The **Principle of Least Privilege (PoLP)** means:

> A user or process should receive only the permissions required to perform its job.

### Bad Configuration

```text
Normal Employee
      ↓
Full Root Access
```

### Better Configuration

```text
Normal Employee
      ↓
Required Permissions Only
```

This limits damage if the account is compromised.

---

# 🚨 Why Privilege Management Matters in Cybersecurity

Imagine an attacker compromises a normal account.

### With restricted privileges:

```text
Attacker
   ↓
Normal User
   ↓
Limited Access
   ↓
Attack Contained
```

### With excessive privileges:

```text
Attacker
   ↓
Administrator
   ↓
System Access
   ↓
Major Damage
```

Proper privilege management can therefore reduce the impact of a compromised account.

---

# 🔥 Common Permission Problems

## 1. World-Writable Files

Example:

```text
-rwxrwxrwx
```

Everyone can potentially modify the file.

This can create security risks depending on where the file is located and how it is used.

---

## 2. Excessive Root Access

Giving unnecessary users administrative privileges increases the impact of account compromise.

---

## 3. Shared Accounts

If multiple people use the same account:

```text
admin
```

it becomes difficult to determine who performed an action.

---

## 4. Weak File Permissions

Sensitive files should not be readable or writable by unauthorized users.

---

## 5. Incorrect Ownership

If an important file belongs to the wrong user or group, unauthorized access may become possible.

---

# 🌍 Real-World Security Example

Suppose a Linux web server contains:

```text
/var/www/html/
```

A web application runs with a specific service account.

If the application's files are unnecessarily writable by that account, an attacker who compromises the application may be able to modify files and potentially execute malicious code.

Proper ownership and permissions can reduce this risk:

```text
Web Application
      ↓
Restricted Service Account
      ↓
Limited File Permissions
      ↓
Reduced Attack Impact
```

---

# 🔄 How Linux Access Control Works

When a user attempts to access a file, Linux evaluates their identity and the file's permissions.

Simplified:

```text
User requests file
        │
        ▼
Who is the user?
        │
        ▼
Are they the owner?
   │           │
  Yes          No
   │           │
   ▼           ▼
Owner       Are they in
permissions the group?
                 │
             ┌───┴───┐
            Yes      No
             │        │
             ▼        ▼
          Group    Others
        permissions permissions
```

The kernel then enforces the applicable access rules.

---

# 🧠 Memory Trick

Think of Linux as a **secure office building**.

- 👤 **Users** = Employees
- 👥 **Groups** = Departments
- 📁 **Files** = Documents
- 🔐 **Permissions** = Access rules
- 🏷️ **Ownership** = Person responsible for a document
- 👑 **Root** = Building administrator
- 🔑 **sudo** = Temporary authorized access
- 🛡️ **Least Privilege** = Give employees only the rooms they need

---

# 📝 Quick Revision

### 👤 User Accounts

- Represent individual identities
- Have UIDs
- Usually have home directories
- Provide accountability

### 👥 Groups

- Organize users
- Simplify permission management
- Allow shared access to resources

### 🔐 File Permissions

```text
r = Read
w = Write
x = Execute
```

Applied to:

```text
Owner | Group | Others
```

### 🏷️ Ownership

Every file normally has:

```text
Owner
Group
```

Managed using:

```bash
chown
chgrp
```

### 👑 Privilege Management

- Controls administrative access
- `sudo` provides controlled privilege elevation
- Root has extensive privileges
- Least privilege reduces security risk

---

# 💡 Interview Tips

### ❓ What are the three basic Linux permission categories?

**Owner, Group, and Others.**

### ❓ What does `chmod 755` mean?

```text
Owner  → rwx
Group  → r-x
Others → r-x
```

### ❓ What is the difference between `chmod` and `chown`?

- `chmod` → changes **permissions**
- `chown` → changes **ownership**

### ❓ Why is least privilege important?

It limits what users and processes can do. If an account or application is compromised, restricted privileges can significantly reduce the attacker's ability to damage the system.

### ❓ Why shouldn't everyone use root?

Root has extensive system privileges. A mistake or compromised root account can result in severe system damage or complete system compromise.

> **Remember:**
>
> 👤 **Users define identities.**
>
> 👥 **Groups organize identities.**
>
> 🔐 **Permissions control access.**
>
> 🏷️ **Ownership defines who owns a resource.**
>
> 👑 **Privileges determine how much control a user has.**
>
> 🛡️ **Least privilege reduces the impact of compromise.**
