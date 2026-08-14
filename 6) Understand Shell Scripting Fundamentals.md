# 🐚 Understand Shell Scripting Fundamentals

> **"Shell scripting turns individual Linux commands into automated workflows. In cybersecurity, scripts can save time, automate repetitive tasks, collect information, and help analyze systems."**

---

# 📖 What is Shell Scripting?

**Shell scripting** is the process of writing a sequence of shell commands into a file so they can be executed automatically.

Instead of typing:

```bash
pwd
ls
whoami
date
```

every time, you can put them into a script:

```bash
#!/bin/bash

pwd
ls
whoami
date
```

and execute the script.

```text
Commands
   ↓
Shell Script
   ↓
Shell Interpreter
   ↓
Linux
   ↓
System
```

---

# 🐚 What is a Shell?

A **shell** is a program that interprets commands and communicates with the operating system.

Common shells include:

- Bash
- Zsh
- Fish
- Sh

**Bash** is one of the most widely used shells and is commonly used for Linux scripting.

---

# 🎯 Why Use Shell Scripts?

Shell scripts are useful for:

- ⚙️ Automating repetitive tasks
- 🔍 System information gathering
- 📁 File management
- 📊 Log processing
- 🛠️ System administration
- 🔐 Security monitoring
- 🚨 Incident investigation
- 🔄 Backup and maintenance tasks

### Example

Instead of manually checking several systems:

```text
CPU
Memory
Disk
Users
Network
Services
```

a script can collect all of this information automatically.

---

# 🧱 Basic Shell Script Structure

A simple Bash script:

```bash
#!/bin/bash

echo "Hello World"
```

The first line:

```bash
#!/bin/bash
```

is called a **shebang**.

It tells the system which interpreter should be used to execute the script.

---

# 📦 A. Variables

## 📖 What is a Variable?

A **variable** stores information that can be used later in a script.

Example:

```bash
name="Farhan"
```

Use the variable:

```bash
echo "$name"
```

Output:

```text
Farhan
```

---

## ⚙️ How Variables Work

```text
name="Farhan"
      │
      ▼
┌─────────────┐
│   Variable  │
│ name        │
│ value=Farhan│
└─────────────┘
      │
      ▼
echo "$name"
      │
      ▼
Farhan
```

---

## 🔢 Different Types of Data

Shell variables can store different kinds of values.

### String

```bash
name="Farhan"
```

### Number

```bash
age=20
```

### Path

```bash
logfile="/var/log/auth.log"
```

---

## ⚠️ Spaces Matter

Correct:

```bash
name="Farhan"
```

Incorrect:

```bash
name = "Farhan"
```

Shell syntax does not treat the second form as a normal variable assignment.

---

# 🧑‍💻 Command Substitution

A variable can store the output of a command.

```bash
username=$(whoami)
```

Now:

```bash
echo "$username"
```

might output:

```text
farhan
```

Another example:

```bash
current_date=$(date)
```

---

# 🔐 Why Variables Matter in Cybersecurity

Variables allow scripts to work with changing information.

Example:

```bash
username=$(whoami)
ip=$(hostname -I)
```

The script does not need to know the user's name or IP address beforehand.

This makes scripts reusable across different systems.

---

# 🔀 B. Conditional Statements

## 📖 What are Conditional Statements?

Conditional statements allow a script to make decisions.

The basic idea is:

```text
IF condition is true
       ↓
Do something

ELSE
       ↓
Do something else
```

---

# `if`

Example:

```bash
if [ "$USER" = "root" ]; then
    echo "You are root"
fi
```

If the condition is true, the message is displayed.

---

# `if ... else`

```bash
if [ "$USER" = "root" ]; then
    echo "Administrative user"
else
    echo "Normal user"
fi
```

---

# `if ... elif ... else`

```bash
if [ "$USER" = "root" ]; then
    echo "Root user"
elif [ "$USER" = "admin" ]; then
    echo "Administrator"
else
    echo "Standard user"
fi
```

---

# 🔍 File Conditions

Shell scripts can check whether files exist.

```bash
if [ -f "notes.txt" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

Common tests:

| Test | Meaning |
|---|---|
| `-f` | Regular file exists |
| `-d` | Directory exists |
| `-e` | Path exists |
| `-r` | Readable |
| `-w` | Writable |
| `-x` | Executable |

---

# 🛡️ Security Example

A script can check whether an important configuration file exists:

```bash
if [ -f "/etc/ssh/sshd_config" ]; then
    echo "SSH configuration found"
else
    echo "SSH configuration not found"
fi
```

This can be useful during system auditing.

---

# 🔁 C. Loops

## 📖 What is a Loop?

A loop repeats a block of commands.

Instead of writing:

```bash
echo "1"
echo "2"
echo "3"
echo "4"
echo "5"
```

you can use a loop.

```text
Start
  ↓
Condition
  ↓
Run commands
  ↓
Repeat
  ↓
Condition false
  ↓
End
```

---

# 🔢 `for` Loop

Example:

```bash
for i in 1 2 3 4 5
do
    echo "$i"
done
```

Output:

```text
1
2
3
4
5
```

---

## 📁 Loop Through Files

```bash
for file in *.txt
do
    echo "$file"
done
```

This processes `.txt` files in the current directory.

---

# 🔄 `while` Loop

A `while` loop continues while a condition remains true.

```bash
count=1

while [ "$count" -le 5 ]
do
    echo "$count"
    count=$((count + 1))
done
```

Output:

```text
1
2
3
4
5
```

---

# 🛡️ Cybersecurity Example

A script could process multiple log files:

```bash
for file in /var/log/*.log
do
    echo "Analyzing $file"
done
```

This allows the same operation to be applied repeatedly.

---

# 🛑 `break`

Stops a loop early.

```bash
for i in 1 2 3 4 5
do
    if [ "$i" -eq 3 ]; then
        break
    fi

    echo "$i"
done
```

Output:

```text
1
2
```

---

# ⏭️ `continue`

Skips the current iteration.

```bash
for i in 1 2 3 4 5
do
    if [ "$i" -eq 3 ]; then
        continue
    fi

    echo "$i"
done
```

Output:

```text
1
2
4
5
```

---

# 🧩 D. Functions

## 📖 What is a Function?

A **function** is a reusable block of commands.

Instead of writing the same commands repeatedly, you define them once and call them whenever needed.

---

# 🔧 Creating a Function

```bash
greeting() {
    echo "Welcome to Linux"
}
```

Call it:

```bash
greeting
```

Output:

```text
Welcome to Linux
```

---

# 🎯 Why Use Functions?

Functions make scripts:

- Easier to read
- Easier to maintain
- More organized
- Reusable
- Easier to troubleshoot

---

# 📦 Function Arguments

Functions can receive arguments.

```bash
greet() {
    echo "Hello $1"
}
```

Call:

```bash
greet Farhan
```

Output:

```text
Hello Farhan
```

Here:

```text
$1 = Farhan
```

---

# 🔢 Multiple Arguments

```bash
show_user() {
    echo "User: $1"
    echo "ID: $2"
}

show_user Farhan 1000
```

Output:

```text
User: Farhan
ID: 1000
```

---

# 🛡️ Security Example

You could create a reusable function for checking a service:

```bash
check_service() {
    systemctl is-active "$1"
}
```

Then:

```bash
check_service ssh
check_service nginx
```

The same function can be reused for different services.

---

# ▶️ E. Script Execution

## 📄 Creating a Script

Create a file:

```bash
nano script.sh
```

Add:

```bash
#!/bin/bash

echo "Cybersecurity Lab"
```

Save the file.

---

# 🔐 Making a Script Executable

Linux uses file permissions to determine whether a script can be executed directly.

```bash
chmod +x script.sh
```

Check permissions:

```bash
ls -l script.sh
```

You may see:

```text
-rwxr-xr-x
```

The `x` indicates execute permission.

---

# ▶️ Running a Script

If the script is executable:

```bash
./script.sh
```

You can also explicitly run it using Bash:

```bash
bash script.sh
```

These approaches are slightly different.

### `./script.sh`

The operating system uses the script's shebang to determine the interpreter.

### `bash script.sh`

You explicitly tell Bash to interpret the script.

---

# 📍 Script Arguments

Scripts can receive arguments from the command line.

Example:

```bash
#!/bin/bash

echo "Hello $1"
```

Run:

```bash
./script.sh Farhan
```

Output:

```text
Hello Farhan
```

Here:

```text
$1 = First argument
```

Other useful special variables:

| Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$#` | Number of arguments |
| `$?` | Exit status of previous command |
| `$@` | All arguments |

---

# 🚦 Exit Status

Linux commands return an **exit status**.

Usually:

```text
0 → Success
Non-zero → Error/failure
```

Example:

```bash
ls /home
echo $?
```

If the command succeeds, `$?` will normally be:

```text
0
```

This is extremely useful in automation.

---

# 🛡️ Example

```bash
if ls /important-folder > /dev/null 2>&1; then
    echo "Folder accessible"
else
    echo "Unable to access folder"
fi
```

The script uses the command's exit status to make a decision.

---

# 🔐 Shell Scripting in Cybersecurity

Shell scripting is widely useful for defensive and administrative tasks such as:

### 🔍 System Enumeration

```text
Users
Processes
Services
Network interfaces
Disk usage
```

### 📝 Log Analysis

```text
Search logs
Filter events
Count occurrences
Identify patterns
```

### 🛡️ Security Auditing

```text
Check permissions
Check services
Check configurations
Check system settings
```

### ⚙️ Automation

```text
Collect information
Run repetitive checks
Generate reports
Perform maintenance
```

---

# 🌍 Real-World Example

Imagine a security analyst wants to quickly collect basic information from a Linux machine.

A script could contain:

```bash
#!/bin/bash

echo "===== SYSTEM ====="
hostname

echo "===== USER ====="
whoami

echo "===== UPTIME ====="
uptime

echo "===== MEMORY ====="
free -h

echo "===== DISK ====="
df -h

echo "===== NETWORK ====="
ip addr
```

Instead of manually running six commands, the analyst executes:

```bash
./system_check.sh
```

The script performs the entire collection automatically.

---

# 🔗 Combining Everything

A useful script can combine:

```text
Variables
    ↓
Conditions
    ↓
Loops
    ↓
Functions
    ↓
Commands
    ↓
Output
```

Example:

```bash
#!/bin/bash

check_file() {
    if [ -f "$1" ]; then
        echo "$1 exists"
    else
        echo "$1 does not exist"
    fi
}

for file in /etc/passwd /etc/hosts /etc/ssh/sshd_config
do
    check_file "$file"
done
```

This script:

1. Defines a function.
2. Receives a filename.
3. Checks whether it exists.
4. Loops through multiple files.
5. Prints the result.

---

# ⚖️ Manual Commands vs Shell Scripts

| Manual Commands | Shell Scripts |
|---|---|
| Run commands individually | Automate multiple commands |
| Good for one-time tasks | Good for repeated tasks |
| Easy for simple operations | Better for complex workflows |
| Can become repetitive | Reduces repetition |
| Less reusable | Highly reusable |
| Human-driven | Can be automated |

---

# ⚠️ Common Shell Scripting Mistakes

### Forgetting the shebang

```bash
#!/bin/bash
```

### Incorrect variable assignment

```bash
name = "Farhan"
```

Instead:

```bash
name="Farhan"
```

### Forgetting quotes

Prefer:

```bash
echo "$name"
```

rather than relying on unquoted variables, especially when values may contain spaces or special characters.

### Forgetting execute permission

```bash
chmod +x script.sh
```

### Running commands with unnecessary privileges

Avoid:

```bash
sudo ./script.sh
```

unless elevated privileges are actually required.

---

# 🚨 Security Considerations

Shell scripts are powerful, which means poorly written scripts can also cause serious problems.

Be careful with:

- `sudo`
- `rm`
- File permissions
- User input
- Untrusted files
- Commands constructed from variables
- Scripts downloaded from unknown sources

For example, blindly executing:

```bash
curl ... | bash
```

from an untrusted source is risky because you are executing code without first inspecting it.

---

# 🧠 Memory Trick

Think of a shell script as a **robot assistant**:

```text
📦 Variables
    ↓
Remember information

🔀 Conditions
    ↓
Make decisions

🔁 Loops
    ↓
Repeat tasks

🧩 Functions
    ↓
Reuse tasks

▶️ Execution
    ↓
Run everything
```

---

# 📝 Quick Revision

### 📦 Variables

Store information:

```bash
name="Farhan"
```

### 🔀 Conditions

Make decisions:

```bash
if [ condition ]; then
    command
fi
```

### 🔁 Loops

Repeat operations:

```bash
for item in list
do
    command
done
```

### 🧩 Functions

Create reusable commands:

```bash
function_name() {
    command
}
```

### ▶️ Script Execution

```bash
chmod +x script.sh
./script.sh
```

or:

```bash
bash script.sh
```

---

# 💡 Interview Tips

### ❓ What is shell scripting?

Shell scripting is writing a sequence of shell commands into a script so that tasks can be automated and executed as a workflow.

### ❓ What is a variable?

A variable stores a value that can be referenced and changed during script execution.

### ❓ Why are loops useful?

Loops allow a script to repeat the same operation for multiple files, users, systems, or other data without manually writing the command repeatedly.

### ❓ What is a function?

A function is a reusable block of commands that performs a specific task.

### ❓ What does `chmod +x` do?

It adds execute permission to a file, allowing an authorized user to execute the file directly.

### ❓ What does `$1` mean?

`$1` represents the **first positional argument** passed to a shell script or function.

> **Remember:**
>
> 📦 **Variables store information.**
>
> 🔀 **Conditions make decisions.**
>
> 🔁 **Loops repeat operations.**
>
> 🧩 **Functions make code reusable.**
>
> ▶️ **Script execution turns commands into automation.**
>
> 🛡️ **In cybersecurity, shell scripting helps automate system administration, investigation, monitoring, and defensive security tasks.**
