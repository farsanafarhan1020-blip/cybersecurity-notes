# 🐧 Understand Linux Fundamentals

> **"Linux is one of the most important operating systems in cybersecurity. Understanding how Linux manages files, users, processes, and services gives you the foundation needed for security testing and system administration."**

---

# 🐧 Linux Ecosystem

## 📖 What is Linux?

**Linux** is an open-source operating system kernel. In everyday use, "Linux" usually refers to a complete operating system built around the Linux kernel, called a **Linux distribution (distro)**.

The Linux ecosystem contains:

- 🧠 Linux Kernel
- 🖥️ Desktop environments
- 🐚 Shells
- 📦 Package managers
- ⚙️ System utilities
- 🔧 Applications and security tools

### Popular Linux Distributions

| Distribution | Common Use |
|---|---|
| Ubuntu | General use, servers, development |
| Debian | Stable servers and general use |
| Fedora | Development and modern Linux |
| Arch Linux | Customization and learning |
| Kali Linux | Cybersecurity and penetration testing |
| Red Hat Enterprise Linux | Enterprise servers |

---

## ⚙️ How Linux Works

A simplified Linux architecture looks like this:

```text
┌──────────────────────────┐
│       Applications       │
├──────────────────────────┤
│      Shell / GUI         │
├──────────────────────────┤
│   System Libraries       │
├──────────────────────────┤
│      Linux Kernel        │
├──────────────────────────┤
│       Hardware           │
└──────────────────────────┘
```

The **kernel** acts as the bridge between software and hardware.

It manages:

- CPU
- Memory
- Storage
- Processes
- Devices
- Networking
- Security permissions

---

# 📂 File System Hierarchy

## 📖 What is a Linux File System?

Linux organizes files and directories in a **hierarchical tree structure**.

Unlike Windows, Linux does not normally use drive letters such as `C:` or `D:`.

Everything starts from:

```text
/
```

This is called the **root directory**.

---

## 🌳 Important Linux Directories

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── tmp
├── usr
└── var
```

### `/home`

Contains normal users' personal directories.

```text
/home/farhan
```

---

### `/root`

Home directory of the **root user**.

> `/root` is different from `/`, which is the filesystem's root directory.

---

### `/etc`

Contains system and application configuration files.

Examples:

```text
/etc/passwd
/etc/hosts
/etc/ssh/
```

---

### `/var`

Contains frequently changing data.

Examples:

- Logs
- Caches
- Application data

Security analysts commonly investigate:

```text
/var/log/
```

---

### `/tmp`

Used for temporary files.

Because temporary directories can be abused by attackers, they are important during security investigations.

---

### `/usr`

Contains many user-space programs, libraries, and other shared resources.

---

### `/bin`

Contains essential user commands on systems where this directory is used separately.

Examples:

```text
ls
cp
mv
cat
```

---

### `/sbin`

Contains many system administration commands.

---

### `/dev`

Contains special files representing devices.

Examples:

```text
/dev/sda
/dev/null
```

---

### `/proc`

A virtual filesystem that exposes information about running processes and the kernel.

Example:

```text
/proc/1/
```

---

## 🛡️ Why the File System Matters in Cybersecurity

Security professionals need to understand where Linux stores:

- Configuration
- User information
- Logs
- Running-process information
- SSH settings
- Temporary files

For example:

```text
/var/log/
```

can contain evidence useful during an incident investigation.

---

# 👥 Users and Groups

## 📖 What are Users?

A **user account** represents an identity that can interact with a Linux system.

Each user has:

- Username
- User ID (UID)
- Home directory
- Permissions
- Group memberships

Example:

```text
farhan
```

---

## 👑 Root User

The **root user** is the Linux superuser.

Root has extremely high privileges and can perform administrative operations that normal users cannot.

```text
Normal User
     │
 Limited Permissions
     │
     ▼
System

Root
     │
 Extensive Privileges
     │
     ▼
System
```

Because root has powerful permissions, using it unnecessarily can increase the impact of mistakes or compromised accounts.

---

# 👥 Groups

A **group** is a collection of users.

Groups make permission management easier.

Example:

```text
Developers
├── Alice
├── Bob
└── Charlie
```

Instead of assigning permissions individually, an administrator can assign permissions to the group.

---

## ⚙️ How Users and Groups Work

Linux associates files with:

- Owner
- Group
- Permissions

Example:

```text
-rw-r-----  user  developers  project.txt
```

This means the file has:

- Owner → `user`
- Group → `developers`
- Permissions → `rw-r-----`

---

## 🛡️ Why Users and Groups Matter in Security

They provide **access control**.

A properly configured system should follow the principle of **least privilege**:

> Users should receive only the permissions required for their work.

---

# ⚙️ Processes

## 📖 What is a Process?

A **process** is a running instance of a program.

For example, when you start a web browser, Linux creates processes to run it.

```text
Program
   │
   ▼
Executed
   │
   ▼
Process
```

A process has information such as:

- Process ID (PID)
- Owner
- Memory usage
- CPU usage
- State

---

## 🆔 Process ID

Every running process receives a **PID**.

Example:

```text
PID
│
├── 1     system initialization
├── 450   ssh service
└── 821   browser
```

PID `1` is typically associated with the system's initial service manager on modern Linux systems.

---

## 🔍 Useful Commands

### View processes

```bash
ps
```

### Detailed process information

```bash
ps aux
```

### Interactive process monitoring

```bash
top
```

or:

```bash
htop
```

---

## 🛑 Managing Processes

A process can sometimes be terminated using:

```bash
kill PID
```

For example:

```bash
kill 821
```

This sends a termination signal to the process.

---

## 🛡️ Processes in Cybersecurity

Security analysts examine processes to identify:

- Unknown programs
- Suspicious processes
- Unexpected CPU usage
- Malware
- Unauthorized activity

For example, a suspicious process running from an unusual temporary directory may deserve investigation.

---

# ⚙️ System Services

## 📖 What is a Service?

A **service** is a background program that provides a specific function to the operating system or other applications.

Examples:

- SSH server
- Web server
- DNS server
- Database server
- Logging service

---

## 🌐 Example

An SSH server allows remote users to securely connect to a Linux machine.

```text
Remote Computer
       │
       │ SSH
       ▼
Linux Server
       │
       ▼
SSH Service
```

---

# ⚙️ systemd

Many modern Linux distributions use **systemd** to manage system services.

The main command is:

```bash
systemctl
```

### Check a service

```bash
systemctl status ssh
```

### Start a service

```bash
sudo systemctl start ssh
```

### Stop a service

```bash
sudo systemctl stop ssh
```

### Enable a service at startup

```bash
sudo systemctl enable ssh
```

---

## 🛡️ Services and Cybersecurity

Unnecessary services increase the **attack surface**.

For example:

```text
Server
│
├── SSH       → Required
├── Web       → Required
├── FTP       → Not Required
└── Old DB    → Not Required
```

If unnecessary services are running, attackers may have additional opportunities to exploit vulnerabilities.

### Security Principle

> **Run only the services that are actually required.**

---

# 🌍 Real-World Security Example

Imagine a Linux web server:

```text
                 🌐 Internet
                      │
                      ▼
                ┌──────────┐
                │ Firewall │
                └──────────┘
                      │
                      ▼
              ┌─────────────┐
              │ Linux Server│
              └─────────────┘
                │    │    │
               SSH  Web  Logs
                │    │    │
                ▼    ▼    ▼
              Users Processes
```

A security analyst may investigate:

### 👥 Users

Are there unauthorized accounts?

### ⚙️ Processes

Is suspicious software running?

### 🌐 Services

Are unnecessary network services enabled?

### 📂 Files

Were important configuration files modified?

### 📝 Logs

What happened before and after the suspicious activity?

---

# ⚖️ Linux vs Windows

| Feature | Linux | Windows |
|---|---|---|
| Source | Mostly open-source ecosystem | Primarily proprietary |
| Filesystem | `/` hierarchy | Drive letters such as `C:` |
| Main CLI | Shell | PowerShell / Command Prompt |
| Permissions | User/group/other model | ACL-based permissions |
| Services | Often systemd on modern distros | Windows Service Manager |
| Security Tools | Many open-source options | Strong enterprise ecosystem |

Both operating systems can be secure. **Configuration, updates, permissions, and security practices matter more than simply choosing one OS.**

---

# 🧠 Memory Trick

Think of Linux as a **city**:

- 🏙️ `/` = Entire city
- 🏠 `/home` = Houses
- 🏛️ `/etc` = Government/configuration offices
- 📜 `/var/log` = City records
- 👥 Users = Citizens
- 👨‍👩‍👧 Groups = Organizations
- 🚗 Processes = Vehicles currently moving
- 🏢 Services = Public services running in the background
- 👑 Root = System administrator with the highest authority

---

# 📝 Quick Revision

### 🐧 Linux Ecosystem

- Linux kernel + software + utilities
- Distributed through Linux distributions
- Widely used in servers, cloud, development, and cybersecurity

### 📂 File System

- `/` = Root of filesystem
- `/home` = User directories
- `/etc` = Configuration
- `/var/log` = Logs
- `/tmp` = Temporary files
- `/proc` = Process/kernel information

### 👥 Users & Groups

- Users represent identities
- Groups organize users
- Permissions control access
- Root has extensive privileges

### ⚙️ Processes

- Running instances of programs
- Identified by PIDs
- Can be monitored and managed

### 🔧 Services

- Background programs providing functionality
- Often managed with `systemctl`
- Unnecessary services increase attack surface

---

# 💡 Interview Tips

### ❓ What is Linux?

Linux is an open-source operating system kernel. Linux distributions combine the kernel with system utilities, libraries, applications, and other components to provide a complete operating system.

### ❓ What is the difference between a program and a process?

A **program** is a set of instructions stored on disk, while a **process** is an instance of that program currently running.

### ❓ What is root in Linux?

**Root** is the superuser account with extensive privileges over the system.

### ❓ Why are unnecessary services a security risk?

Every running service can potentially contain vulnerabilities or provide an entry point for attackers. Disabling unnecessary services reduces the system's **attack surface**.

> **Remember:**
>
> 🐧 **Linux manages the system through the kernel.**
>
> 📂 **The filesystem organizes data and system resources.**
>
> 👥 **Users and groups control identity and access.**
>
> ⚙️ **Processes are running programs.**
>
> 🔧 **Services provide background functionality.**
>
> 🛡️ **Understanding all five is essential for Linux cybersecurity.**
````
