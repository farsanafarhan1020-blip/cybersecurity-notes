# 🐧 Understand Linux Command Line

> **"The Linux command line gives you direct control over the operating system. For cybersecurity professionals, it is one of the most important tools for navigating systems, analyzing files, searching for evidence, and performing administration."**

---

# ⌨️ What is the Linux Command Line?

The **command line** is a text-based interface where users interact with Linux by entering commands.

Instead of clicking buttons in a graphical interface, you type commands that the **shell** interprets and executes.

```text
User
  │
  ▼
Shell
  │
  ▼
Linux Kernel
  │
  ▼
Hardware / Files / Processes
```

### Common Linux Shells

- Bash
- Zsh
- Fish
- Sh

**Bash** is one of the most commonly encountered shells on Linux systems.

---

# 📂 A. Navigation Commands

Navigation commands allow you to move around the Linux filesystem.

---

## `pwd` — Print Working Directory

Shows your current location.

```bash
pwd
```

Example:

```text
/home/farhan
```

---

## `ls` — List Files

Displays files and directories.

```bash
ls
```

Useful options:

```bash
ls -l
```

Shows detailed information.

```bash
ls -a
```

Shows hidden files.

```bash
ls -la
```

Shows hidden files with detailed information.

---

## `cd` — Change Directory

Moves to another directory.

```bash
cd /home
```

Go to your home directory:

```bash
cd ~
```

Move to the parent directory:

```bash
cd ..
```

---

## Navigation Example

```bash
pwd
ls
cd Documents
ls
cd ..
pwd
```

---

# 📄 B. File Management

Linux provides commands for creating, copying, moving, and deleting files and directories.

---

## `touch` — Create a File

```bash
touch notes.txt
```

Creates an empty file if it doesn't already exist.

---

## `mkdir` — Create a Directory

```bash
mkdir cybersecurity
```

Create nested directories:

```bash
mkdir -p labs/networking/week1
```

---

## `cp` — Copy

Copy a file:

```bash
cp notes.txt backup.txt
```

Copy a directory recursively:

```bash
cp -r lab1 lab1-backup
```

---

## `mv` — Move or Rename

Move a file:

```bash
mv notes.txt Documents/
```

Rename a file:

```bash
mv old.txt new.txt
```

---

## `rm` — Remove

Delete a file:

```bash
rm notes.txt
```

Delete an empty directory:

```bash
rmdir oldfolder
```

For recursive directory removal:

```bash
rm -r oldfolder
```

⚠️ **Be careful with `rm`.** Deleted files may not be recoverable through normal means.

---

# 👀 Viewing File Contents

## `cat`

Displays the contents of a file.

```bash
cat notes.txt
```

---

## `less`

Useful for reading large files one screen at a time.

```bash
less logfile.txt
```

Use:

```text
Space → Next page
b     → Previous page
q     → Quit
```

---

## `head`

Shows the beginning of a file.

```bash
head logfile.txt
```

Show the first 20 lines:

```bash
head -n 20 logfile.txt
```

---

## `tail`

Shows the end of a file.

```bash
tail logfile.txt
```

Useful for monitoring logs:

```bash
tail -f /var/log/syslog
```

---

# 🔎 C. Text Processing

Linux provides powerful tools for processing and analyzing text.

These tools are extremely useful when working with:

- Logs
- Configuration files
- Security reports
- Network data
- System information

---

## `grep` — Search Text

Search for a word:

```bash
grep "error" logfile.txt
```

Case-insensitive search:

```bash
grep -i "error" logfile.txt
```

Search recursively:

```bash
grep -r "password" /etc/
```

⚠️ Only search locations you are authorized to inspect.

---

## `cut` — Extract Fields

Example:

```bash
cut -d: -f1 /etc/passwd
```

This uses `:` as the delimiter and extracts the first field.

---

## `sort` — Sort Text

```bash
sort names.txt
```

---

## `uniq` — Remove Consecutive Duplicates

```bash
sort names.txt | uniq
```

Count occurrences:

```bash
sort names.txt | uniq -c
```

---

## `wc` — Count

```bash
wc logfile.txt
```

It can count:

- Lines
- Words
- Characters

Example:

```bash
wc -l logfile.txt
```

Counts lines.

---

# 🔗 Pipes

One of the most powerful features of the Linux command line is the **pipe (`|`)**.

A pipe sends the output of one command into another command.

```text
Command 1
   │
   ▼
Command 2
   │
   ▼
Command 3
```

Example:

```bash
cat logfile.txt | grep "failed"
```

This means:

```text
Read logfile
     ↓
Search for "failed"
```

Another example:

```bash
ps aux | grep ssh
```

This can help identify processes related to SSH.

---

# 🔍 D. Search Operations

Searching is extremely important in cybersecurity because analysts often need to find:

- Files
- Logs
- Configuration
- Suspicious processes
- Evidence

---

## `find`

Search for files by name:

```bash
find /home -name "notes.txt"
```

Find all `.log` files:

```bash
find /var/log -name "*.log"
```

Find files modified recently:

```bash
find /home -type f -mtime -1
```

---

## `which`

Shows where a command is located.

```bash
which python
```

Example:

```text
/usr/bin/python
```

---

## `locate`

Searches a pre-built database of filenames.

```bash
locate passwd
```

The database may need to be updated before new files appear.

---

# 🔐 Searching for Security Evidence

Suppose you are investigating failed SSH logins.

You could inspect authentication logs and search for relevant entries.

For example:

```bash
grep "Failed password" /var/log/auth.log
```

On some Linux distributions, authentication information may instead be available through the system journal:

```bash
journalctl
```

The exact log location depends on the distribution and logging configuration.

---

# 👑 E. Administrative Commands

Administrative commands allow authorized users to manage the Linux system.

These commands can affect the entire operating system, so they should be used carefully.

---

# `sudo`

`sudo` allows an authorized user to execute a command with elevated privileges.

Example:

```bash
sudo apt update
```

It does **not** automatically make every user a root user; it grants elevated privileges according to the system's sudo configuration.

---

# `whoami`

Shows the current username.

```bash
whoami
```

Example:

```text
farhan
```

---

# `id`

Displays user and group information.

```bash
id
```

Example output may contain:

```text
uid=1000(farhan)
gid=1000(farhan)
groups=1000(farhan),27(sudo)
```

---

# `uname`

Displays system information.

```bash
uname -a
```

Useful for identifying:

- Kernel
- Architecture
- Operating system information

---

# `hostname`

Displays the system's hostname.

```bash
hostname
```

---

# `ip`

Used to inspect and manage network configuration.

Show network interfaces:

```bash
ip addr
```

Show routing information:

```bash
ip route
```

---

# `systemctl`

Used on systems using **systemd** to manage services.

Check a service:

```bash
systemctl status ssh
```

Start a service:

```bash
sudo systemctl start ssh
```

Stop a service:

```bash
sudo systemctl stop ssh
```

---

# 📦 Package Management

Linux distributions use package managers to install and update software.

### Debian / Ubuntu / Kali

```bash
sudo apt update
```

```bash
sudo apt install nmap
```

Remove a package:

```bash
sudo apt remove nmap
```

Other distributions use package managers such as:

```text
dnf
yum
pacman
zypper
```

---

# 🔐 File Permissions

Linux uses permissions to control access to files and directories.

Example:

```text
-rwxr-xr--
```

These permissions are divided into:

```text
Owner   Group   Others
rwx     r-x     r--
```

### Permission Types

| Permission | Meaning |
|---|---|
| `r` | Read |
| `w` | Write |
| `x` | Execute |

---

## `chmod`

Changes permissions.

Example:

```bash
chmod +x script.sh
```

This adds execute permission.

---

## `chown`

Changes file ownership.

```bash
sudo chown user:user file.txt
```

Incorrect ownership or permissions can create serious security problems.

---

# 🔄 Command Chaining

Commands can be combined to create powerful workflows.

### `&&`

Runs the second command only if the first succeeds.

```bash
mkdir lab && cd lab
```

### `;`

Runs commands sequentially regardless of whether the previous command succeeds.

```bash
pwd; ls
```

---

# 📊 Redirection

Linux can redirect command output into files.

### `>`

Overwrite a file:

```bash
ls > files.txt
```

### `>>`

Append to a file:

```bash
echo "New entry" >> notes.txt
```

### `<`

Use a file as input:

```bash
command < input.txt
```

---

# 🛡️ Why the Command Line Matters in Cybersecurity

Cybersecurity professionals frequently work with Linux servers where graphical interfaces may not be installed.

The command line allows analysts to:

- Investigate logs
- Inspect processes
- Search files
- Analyze network settings
- Manage services
- Check permissions
- Automate repetitive tasks
- Perform authorized security testing

---

# 🌍 Real-World Cybersecurity Example

Imagine a security analyst investigating a suspicious Linux server.

```text
Suspicious Activity
        │
        ▼
whoami
        │
        ▼
Identify Current User
        │
        ▼
ps aux
        │
        ▼
Inspect Processes
        │
        ▼
ss / ip
        │
        ▼
Inspect Network Activity
        │
        ▼
grep / journalctl
        │
        ▼
Analyze Logs
        │
        ▼
find
        │
        ▼
Search for Suspicious Files
```

The analyst combines multiple commands instead of relying on a single tool.

---

# ⚖️ GUI vs Command Line

| GUI | Command Line |
|---|---|
| Visual | Text-based |
| Easy for beginners | Requires command knowledge |
| Good for general tasks | Excellent for administration |
| Often slower for repetitive tasks | Easy to automate |
| Less flexible | Highly flexible |
| Uses more system resources | Usually lightweight |

---

# 🧠 Memory Trick

Think of the Linux command line as a **toolbox**:

```text
📍 pwd / cd / ls
      ↓
Navigation

📁 touch / mkdir / cp / mv / rm
      ↓
File Management

🔎 grep / cut / sort / wc
      ↓
Text Processing

🔍 find / locate / which
      ↓
Searching

👑 sudo / systemctl / ip
      ↓
Administration
```

---

# 📝 Quick Revision

### 📍 Navigation

```bash
pwd
ls
cd
```

Used to move around and inspect directories.

### 📁 File Management

```bash
touch
mkdir
cp
mv
rm
```

Used to create, copy, move, rename, and delete files.

### 📝 Text Processing

```bash
cat
less
head
tail
grep
cut
sort
uniq
wc
```

Used to read and analyze text.

### 🔍 Search

```bash
find
locate
which
```

Used to locate files and commands.

### 👑 Administration

```bash
sudo
systemctl
ip
uname
id
```

Used for authorized system administration and investigation.

---

# 💡 Interview Tips

### ❓ What is a shell?

A **shell** is a command interpreter that accepts commands from the user and executes them through the operating system.

### ❓ What is the difference between `grep` and `find`?

- `grep` → searches **inside text**
- `find` → searches for **files/directories**

Example:

```bash
grep "error" logfile.txt
```

```bash
find /var/log -name "*.log"
```

### ❓ Why is the Linux command line important in cybersecurity?

Because many security tools, servers, forensic workflows, and administrative tasks are performed from the command line. It also makes automation and large-scale analysis possible.

> **Remember:**
>
> 📍 **Navigate with `pwd`, `ls`, and `cd`.**
>
> 📁 **Manage files with `cp`, `mv`, `rm`, and `mkdir`.**
>
> 🔎 **Process text with `grep`, `cut`, `sort`, and `wc`.**
>
> 🔍 **Search with `find` and `locate`.**
>
> 👑 **Administer systems with tools such as `sudo` and `systemctl`.**
>
> 🛡️ **In cybersecurity, the command line turns raw system information into useful evidence.**
````
