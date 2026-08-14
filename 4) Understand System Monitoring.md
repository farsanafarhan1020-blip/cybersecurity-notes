# 📊 Understand System Monitoring

> **"System monitoring is the process of observing what is happening inside a computer. In cybersecurity, monitoring helps identify performance problems, suspicious processes, resource abuse, and signs of compromise."**

---

# 📖 What is System Monitoring?

**System Monitoring** is the continuous observation of a computer's **processes, memory, storage, services, and overall performance**.

A Linux administrator uses monitoring to keep systems healthy, while a security analyst uses it to detect unusual or potentially malicious activity.

```text
              Linux System
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
   Processes     Memory       Disk
       │           │           │
       └───────────┼───────────┘
                   ▼
              Services
                   │
                   ▼
          System Performance
```

---

# 🎯 Why is System Monitoring Important?

System monitoring helps you:

- 🔍 Detect suspicious activity
- ⚙️ Find performance problems
- 🧠 Identify memory exhaustion
- 💾 Monitor storage usage
- 🔧 Check system services
- 🚨 Investigate possible attacks
- 🛠️ Troubleshoot system problems

---

# ⚙️ A. Process Monitoring

## 📖 What is a Process?

A **process** is a running instance of a program.

For example:

```text
Program
   ↓
Executed
   ↓
Process
```

Every process normally has a **Process ID (PID)**.

Example:

```text
PID     Program
101     sshd
250     firefox
421     python
```

---

## 🔍 Why Monitor Processes?

Monitoring processes helps identify:

- Programs consuming excessive CPU
- Unexpected applications
- Suspicious processes
- Processes running under unusual users
- Malware
- Applications that have stopped responding

---

## 🛠️ Useful Commands

### `ps`

Displays running processes.

```bash
ps
```

Detailed view:

```bash
ps aux
```

---

### `top`

Provides real-time process information.

```bash
top
```

It can show:

- CPU usage
- Memory usage
- PID
- User
- Process state

---

### `htop`

A more interactive process monitor:

```bash
htop
```

> `htop` may need to be installed separately.

---

### `pgrep`

Search for processes by name.

```bash
pgrep ssh
```

---

## 🛑 Managing a Process

If a process is no longer needed, an authorized administrator can send it a signal.

```bash
kill PID
```

Example:

```bash
kill 421
```

The default signal requests graceful termination.

---

# 🚨 Security Example

Suppose a server normally runs:

```text
nginx
sshd
systemd
```

Suddenly you discover:

```text
/tmp/.hidden/python
```

using a large amount of CPU.

That does not automatically mean it is malware, but it is **suspicious and worth investigating**.

You could inspect:

```bash
ps aux
```

and investigate the process, its executable, parent process, user, network connections, and related logs.

---

# 🧠 B. Memory Usage

## 📖 What is Memory Usage?

**Memory usage** refers to how much **RAM** is being used by the operating system and applications.

RAM stores data and instructions that actively running programs need.

---

## 🔍 Why Monitor Memory?

Low available memory can cause:

- Slow applications
- System instability
- Processes being terminated
- Increased swap usage

From a security perspective, unusual memory consumption may also indicate:

- Memory leaks
- Resource exhaustion
- Abnormal applications
- Certain types of malicious activity

---

# 🛠️ `free`

The `free` command provides a quick overview of memory.

```bash
free -h
```

Example:

```text
              total   used   free
Mem:           8Gi    4Gi    2Gi
Swap:          2Gi    500M   1.5Gi
```

The `-h` option displays values in a human-readable format.

---

# 🔍 Understanding RAM

```text
Physical RAM
     │
     ├── Programs
     ├── Kernel
     ├── Cache
     └── Other system data
```

Linux uses otherwise-unused memory for useful caching, so **"used memory" alone does not necessarily mean the system is running out of RAM**.

Available memory is generally more useful when assessing memory pressure.

---

# 🔄 Swap

**Swap** is disk space used as an extension of memory when RAM pressure is high.

```text
RAM
 │
 │ Memory pressure
 ▼
Swap
 │
 ▼
Disk
```

Swap is much slower than RAM.

High swap activity can therefore indicate memory pressure and affect performance.

---

# 🛠️ C. Disk Utilization

## 📖 What is Disk Utilization?

Disk monitoring involves checking:

- Available storage space
- Used storage
- Filesystem capacity
- Disk activity

---

## 💾 Check Filesystem Space

Use:

```bash
df -h
```

Example:

```text
Filesystem     Size   Used   Avail   Use%
/dev/sda2      100G   65G    30G     68%
```

---

## 📁 Check Directory Size

Use:

```bash
du -sh /var/log
```

This shows how much disk space a directory uses.

---

## ⚠️ Why Disk Monitoring Matters

A completely full filesystem can cause:

- Applications to fail
- Logs to stop being written
- Updates to fail
- Databases to malfunction
- System instability

---

# 🚨 Security Example

Imagine:

```text
/var/log/
```

suddenly grows from:

```text
500 MB
```

to:

```text
40 GB
```

This could have a legitimate explanation, such as excessive application logging.

But it could also indicate:

- A misconfigured application
- Repeated attack activity
- Log flooding
- Unexpected files
- An attacker attempting to consume storage

The increase should therefore be investigated rather than immediately assumed to be malicious.

---

# ⚙️ D. Service Management

## 📖 What is a Service?

A **service** is a background process that provides functionality to the system or other applications.

Examples:

- SSH
- Web servers
- Database servers
- DNS services
- Logging services

---

## ⚙️ systemd

Many modern Linux distributions use **systemd** to manage services.

The primary command is:

```bash
systemctl
```

---

## 🔍 Check Service Status

```bash
systemctl status ssh
```

This can show:

- Whether the service is running
- Recent status information
- Main process/PID
- Recent log messages

---

## ▶️ Start a Service

```bash
sudo systemctl start ssh
```

---

## ⏹️ Stop a Service

```bash
sudo systemctl stop ssh
```

---

## 🔄 Restart a Service

```bash
sudo systemctl restart ssh
```

---

## 🚀 Enable at Boot

```bash
sudo systemctl enable ssh
```

This configures the service to start automatically during boot.

---

## 🚫 Disable at Boot

```bash
sudo systemctl disable ssh
```

---

# 🛡️ Services and Security

Every network-facing service can potentially increase a system's **attack surface**.

For example:

```text
Linux Server
│
├── SSH      → Required
├── Web      → Required
├── FTP      → Not Required
└── Old DB   → Not Required
```

If a service is unnecessary, disabling it can reduce the number of potential attack paths.

> **Important:** Never stop or disable a production service without understanding its purpose and authorization.

---

# 📈 E. Performance Observation

## 📖 What is Performance Monitoring?

**Performance observation** means monitoring how efficiently the system is operating.

Important areas include:

```text
CPU
RAM
Disk
Network
Processes
Services
```

---

# 🧠 CPU Monitoring

`top` provides CPU information:

```bash
top
```

You can also inspect CPU information with:

```bash
lscpu
```

---

## 🌐 Network Monitoring

The `ip` command provides network information:

```bash
ip addr
```

Routing information:

```bash
ip route
```

For active network connections, tools such as:

```bash
ss
```

can be useful.

Example:

```bash
ss -tuln
```

This can display listening TCP/UDP sockets.

---

# 📊 Performance Indicators

| Resource | What to Observe |
|---|---|
| CPU | High or unusual utilization |
| RAM | Available memory and swap |
| Disk | Capacity and disk activity |
| Network | Connections and traffic |
| Processes | CPU/memory consumption |
| Services | Running/stopped status |

---

# 🔎 Normal vs Suspicious Activity

System monitoring is not simply about finding **high usage**.

High resource usage can be completely legitimate.

For example:

```text
CPU 95%
```

could mean:

- Video rendering
- Software compilation
- A backup
- A legitimate security scan
- Malware

Therefore, analysts need to examine **context**.

---

# 🌍 Real-World Cybersecurity Investigation

Imagine a Linux server suddenly becomes very slow.

The administrator begins monitoring:

```text
        System Slow
             │
             ▼
        Check CPU
             │
             ▼
       Find Process
             │
             ▼
      Check Memory
             │
             ▼
     Check Disk Usage
             │
             ▼
     Check Network
             │
             ▼
     Review Services
             │
             ▼
       Check Logs
```

They discover an unknown process consuming most of the CPU and making outbound network connections.

The process becomes a potential security indicator and is investigated further.

---

# 🛠️ Useful Monitoring Commands

| Command | Purpose |
|---|---|
| `ps aux` | List processes |
| `top` | Real-time process/resource monitoring |
| `htop` | Interactive process monitoring |
| `free -h` | Memory information |
| `df -h` | Filesystem usage |
| `du -sh` | Directory size |
| `systemctl` | Manage services |
| `ss` | Network sockets/connections |
| `ip addr` | Network interfaces |
| `ip route` | Routing table |
| `uptime` | System uptime/load information |
| `journalctl` | View systemd journal logs |

---

# 🔗 Combining Monitoring Commands

A security analyst rarely uses only one command.

For example:

```bash
ps aux
```

finds a suspicious process.

Then:

```bash
ss -tupn
```

can help examine network connections.

Then:

```bash
systemctl status <service>
```

can help determine whether the process belongs to a managed service.

Finally:

```bash
journalctl
```

can provide additional system/service events.

This creates a bigger picture.

---

# ⚖️ System Monitoring vs Performance Monitoring

| System Monitoring | Performance Monitoring |
|---|---|
| Broad system observation | Focuses mainly on efficiency |
| Processes, services, logs, resources | CPU, RAM, disk, network |
| Often security-focused | Often operations-focused |
| Helps detect suspicious activity | Helps detect bottlenecks |

They overlap heavily and are often performed together.

---

# 🧠 Memory Trick

Think of Linux as a **car**:

- 🧠 **CPU** = Engine
- 💾 **RAM** = Workspace
- 💽 **Disk** = Storage/boot
- 🌐 **Network** = Roads
- ⚙️ **Services** = Systems running inside the car
- 📊 **Monitoring** = Dashboard

If the engine suddenly runs at maximum power while the car is parked, you investigate.

The same principle applies to a computer.

---

# 📝 Quick Revision

### ⚙️ Process Monitoring

- Processes are running programs.
- Use `ps`, `top`, and `htop`.
- Look for unusual processes and resource usage.

### 🧠 Memory Usage

- RAM stores data used by active programs.
- Use `free -h`.
- Monitor available memory and swap activity.

### 💾 Disk Utilization

- Shows storage capacity and usage.
- Use `df -h` and `du`.
- Full disks can cause serious system problems.

### 🔧 Service Management

- Services run in the background.
- Use `systemctl`.
- Unnecessary services can increase attack surface.

### 📈 Performance Observation

Monitor:

```text
CPU
RAM
Disk
Network
Processes
Services
```

---

# 💡 Interview Tips

### ❓ Why is process monitoring important in cybersecurity?

It allows analysts to identify unusual or malicious processes, investigate resource abuse, and understand what software is running on a system.

### ❓ What is the difference between `df` and `du`?

- `df` → shows **filesystem/disk space usage**
- `du` → shows **space used by files and directories**

### ❓ What does `systemctl` do?

It is commonly used on systemd-based Linux systems to inspect and manage system services.

### ❓ Does high CPU usage always mean malware?

**No.** High CPU usage can be caused by legitimate applications, updates, compilation, backups, or other normal workloads. Security analysts must investigate the process and context before deciding whether it is malicious.

> **Remember:**
>
> ⚙️ **Monitor processes to know what is running.**
>
> 🧠 **Monitor memory to detect resource pressure.**
>
> 💾 **Monitor disk usage to prevent storage problems.**
>
> 🔧 **Monitor services to control the attack surface.**
>
> 📈 **Observe overall performance to detect both technical and security problems.**
>
> 🛡️ **In cybersecurity, unusual system behavior is a clue—not automatically proof of an attack.**
