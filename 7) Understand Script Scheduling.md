# ⏰ 7. Understand Script Scheduling

> **A useful security script becomes much more powerful when it can run automatically at the right time, without requiring a person to start it manually.**

Security teams often need tasks to run repeatedly:

* Check system resources every 5 minutes
* Monitor a service every minute
* Analyze logs every hour
* Generate a daily security report
* Check files for unexpected changes
* Run backups
* Perform authorized network checks
* Clean temporary security data

This is where **script scheduling** becomes important.

---

# 🧠 1. What Is Script Scheduling?

**Script scheduling** means configuring a script or program to execute automatically at a specific time, interval, or event.

### Manual execution

```text
User
 ↓
Open Terminal
 ↓
Run Script
 ↓
Wait for Result
```

### Scheduled execution

```text
Scheduler
    ↓
Specified Time / Interval
    ↓
Run Script
    ↓
Collect Result
    ↓
Log Result
    ↓
Alert if Necessary
```

The scheduler becomes responsible for starting the script.

---

# ⏰ 2. Scheduled Execution

There are several ways to schedule scripts depending on the operating system.

## Linux

Common scheduling mechanisms include:

* `cron`
* `crontab`
* `systemd timers`
* `at`

## Windows

Common mechanisms include:

* Task Scheduler
* PowerShell scheduled tasks

Since you're learning cybersecurity with Kali/Linux, **cron and systemd timers** are especially important.

---

# 🐧 3. Linux `cron`

`cron` is a Linux service used to execute commands or scripts automatically according to a schedule.

The schedule is defined using a **crontab**.

View your current crontab:

```bash
crontab -l
```

Edit your crontab:

```bash
crontab -e
```

---

# 🧩 4. Cron Syntax

A cron entry normally contains:

```text
┌──────── minute
│ ┌────── hour
│ │ ┌──── day of month
│ │ │ ┌── month
│ │ │ │ ┌ day of week
│ │ │ │ │
* * * * * command
```

There are **five time fields**:

| Field        | Range |
| ------------ | ----- |
| Minute       | 0–59  |
| Hour         | 0–23  |
| Day of month | 1–31  |
| Month        | 1–12  |
| Day of week  | 0–7   |

`0` and `7` commonly represent Sunday.

---

# 🔥 5. Cron Examples

### Run every minute

```bash
* * * * * /usr/bin/python3 /home/user/script.py
```

### Run every 5 minutes

```bash
*/5 * * * * /usr/bin/python3 /home/user/script.py
```

### Run every hour

```bash
0 * * * * /usr/bin/python3 /home/user/script.py
```

### Run every day at 9:00 AM

```bash
0 9 * * * /usr/bin/python3 /home/user/script.py
```

### Run every Sunday at 10:00 AM

```bash
0 10 * * 0 /usr/bin/python3 /home/user/script.py
```

---

# 🧠 Easy Cron Memory Trick

Think:

```text
MINUTE
  ↓
HOUR
  ↓
DAY
  ↓
MONTH
  ↓
WEEKDAY
```

So:

```text
* * * * *
│ │ │ │ │
│ │ │ │ └── Weekday
│ │ │ └──── Month
│ │ └────── Day
│ └──────── Hour
└────────── Minute
```

---

# 🛠️ 6. Scheduling a Python Security Script

Suppose you have:

```text
/home/user/security_monitor.py
```

First test it manually:

```bash
python3 /home/user/security_monitor.py
```

Make sure it works before scheduling it.

Then:

```bash
crontab -e
```

Add:

```bash
*/5 * * * * /usr/bin/python3 /home/user/security_monitor.py
```

Now the script will be started approximately every **5 minutes**.

---

# ⚠️ 7. Why Use Absolute Paths?

This is safer and more reliable:

```bash
/usr/bin/python3 /home/user/security_monitor.py
```

Instead of:

```bash
python3 security_monitor.py
```

Scheduled jobs may run with a different environment and working directory than your interactive terminal.

Using explicit paths reduces unexpected behavior.

You can find Python's location with:

```bash
which python3
```

---

# 🤖 8. Automated Tasks

Script scheduling becomes useful when combined with automation.

For example:

```text
                 Scheduler
                    ↓
             Run every 5 min
                    ↓
          Security Monitoring Script
                    ↓
          ┌─────────┼─────────┐
          ↓         ↓         ↓
       CPU Check  Disk Check  Service Check
          ↓         ↓         ↓
          └─────────┼─────────┘
                    ↓
                Analyze
                    ↓
              Alert if needed
```

---

# 🔐 9. Security Automation Examples

### Example 1 — Log Monitoring

```text
Every 10 minutes
      ↓
Read authentication logs
      ↓
Count failed logins
      ↓
Detect suspicious patterns
      ↓
Generate alert
```

### Example 2 — File Integrity

```text
Every hour
    ↓
Calculate file hashes
    ↓
Compare with baseline
    ↓
Detect changes
    ↓
Generate alert
```

### Example 3 — Daily Report

```text
Every day at 18:00
        ↓
Collect security events
        ↓
Process data
        ↓
Generate report
        ↓
Save report
```

### Example 4 — System Monitoring

```text
Every 5 minutes
       ↓
Check CPU
       ↓
Check RAM
       ↓
Check disk
       ↓
Check services
       ↓
Record results
```

---

# 📋 10. Job Management

Once you create scheduled jobs, you need to manage them.

Job management includes:

* Creating jobs
* Viewing jobs
* Modifying jobs
* Removing jobs
* Checking execution
* Handling failures
* Reviewing logs

---

## View Cron Jobs

```bash
crontab -l
```

Example:

```text
*/5 * * * * /usr/bin/python3 /home/user/security_monitor.py
0 18 * * * /usr/bin/python3 /home/user/daily_report.py
```

This means:

```text
Every 5 minutes → security_monitor.py

Every day at 18:00 → daily_report.py
```

---

# 🗑️ Removing a Cron Job

Edit:

```bash
crontab -e
```

Then remove the unwanted entry.

Avoid using:

```bash
crontab -r
```

unless you intentionally want to remove the **entire user's crontab**.

---

# 🧩 11. `systemd` Timers

Modern Linux systems can also use **systemd timers**.

A systemd timer can trigger a systemd service according to a schedule.

Architecture:

```text
systemd Timer
      ↓
Triggers
      ↓
systemd Service
      ↓
Runs Script
```

This separates:

```text
WHEN should it run?
        ↓
Timer

WHAT should run?
        ↓
Service
```

This can provide more control than a simple cron entry.

---

# 📊 Cron vs Systemd Timer

| Feature                           | Cron           | systemd Timer              |
| --------------------------------- | -------------- | -------------------------- |
| Scheduling                        | ✅              | ✅                          |
| Simple setup                      | ✅              | ⚠️ More configuration      |
| Service integration               | Limited        | Strong                     |
| Logging integration               | Basic/external | Strong systemd integration |
| Failure handling                  | Basic          | More control               |
| Dependencies                      | Limited        | Strong                     |
| Good for simple scripts           | ✅              | ✅                          |
| Good for service-oriented systems | ⚠️             | ✅                          |

For beginner automation, **cron is usually easier to understand first**.

---

# 🧪 12. Testing Scheduled Scripts

Never assume a scheduled job works just because it appears in `crontab`.

Test the script manually first:

```bash
python3 security_monitor.py
```

Then test scheduling with a simple script.

Example:

```python
from datetime import datetime

with open("/tmp/scheduler_test.txt", "a") as file:
    file.write(f"Executed: {datetime.now()}\n")
```

Schedule it:

```bash
* * * * * /usr/bin/python3 /home/user/scheduler_test.py
```

Wait for the next minute and inspect:

```bash
cat /tmp/scheduler_test.txt
```

You should see entries similar to:

```text
Executed: 2026-09-25 12:01:00
Executed: 2026-09-25 12:02:00
Executed: 2026-09-25 12:03:00
```

This is a simple way to verify that the scheduler is actually executing your script.

---

# 📡 13. Monitoring Execution

Scheduling a script is only half the job.

You also need to know:

> **Did the script actually execute successfully?**

A reliable automation system should monitor execution.

---

## What Should Be Monitored?

```text
Execution
   ↓
 ┌───────────────┐
 │               │
 ↓               ↓
Success         Failure
 ↓               ↓
Log Result      Log Error
 ↓               ↓
Continue        Alert
```

Important information includes:

* Start time
* End time
* Exit status
* Execution duration
* Output
* Error messages
* Number of processed items
* Number of alerts generated

---

# 🔢 14. Exit Codes

Programs commonly use an **exit code** to indicate whether they completed successfully.

Typically:

```text
0 → Success

Non-zero → Error / abnormal result
```

Example:

```python
import sys

print("Security check completed.")

sys.exit(0)
```

Failure example:

```python
import sys

print("Security check failed.")

sys.exit(1)
```

You can inspect a command's exit status in Bash:

```bash
echo $?
```

Example:

```bash
python3 script.py
echo $?
```

Output:

```text
0
```

means the previous command reported success.

---

# 📝 15. Logging Scheduled Execution

Instead of only printing:

```python
print("Check completed")
```

you can use Python's logging system.

```python
import logging

logging.basicConfig(
    filename="security_automation.log",
    level=logging.INFO
)

logging.info("Security check started")

# Perform task

logging.info("Security check completed")
```

Errors can be recorded:

```python
logging.error("Security check failed")
```

This gives you a history of what happened.

---

# 📂 16. Redirecting Cron Output

Cron jobs can also redirect output to a file.

Example:

```bash
*/5 * * * * /usr/bin/python3 /home/user/security_monitor.py >> /home/user/security_monitor.log 2>&1
```

Meaning:

```text
>> file
   ↓
Append normal output

2>&1
   ↓
Send errors to the same location
```

So both normal output and errors can be recorded.

---

# 🚨 17. Failure Detection

Imagine your scheduled security script normally runs every 5 minutes.

You could have:

```text
12:00 → SUCCESS
12:05 → SUCCESS
12:10 → SUCCESS
12:15 → FAILURE
```

A monitoring system could detect:

```text
Expected execution
       ↓
No successful result
       ↓
Execution failure
       ↓
Generate alert
```

This is called **monitoring the monitor**.

The automation itself must also be monitored.

---

# 🛡️ 18. Reliability Considerations

Scheduled security scripts need to be reliable.

Important considerations include:

### 1. Error Handling

Use:

```python
try:
    # task
    pass

except Exception as error:
    # log error
    print(error)
```

Do not allow one unexpected error to silently destroy the workflow.

---

### 2. Logging

Record:

```text
When?
What happened?
What was processed?
Did it succeed?
What failed?
```

---

### 3. Timeouts

Network operations should not wait forever.

For example:

```python
import requests

response = requests.get(
    "https://example.com",
    timeout=10
)
```

A timeout prevents the script from becoming stuck indefinitely.

---

### 4. Resource Usage

A scheduled script should not consume excessive:

* CPU
* RAM
* Disk
* Network bandwidth

For example, running a very expensive scan every minute could unnecessarily overload a system.

---

### 5. Duplicate Execution

Be careful if a previous execution is still running when the next scheduled execution begins.

```text
12:00 → Script starts
12:05 → Script still running
12:05 → Scheduler starts another copy
```

Now:

```text
Script A ────────────────→
Script B       ────────────────→
```

This can cause:

* Duplicate processing
* Resource consumption
* Conflicting file writes
* Duplicate alerts

---

# 🔒 19. Preventing Overlapping Jobs

One approach is to use a lock.

Conceptually:

```text
Start
 ↓
Check Lock
 ↓
Already running?
 ├── YES → Exit
 └── NO
      ↓
   Create Lock
      ↓
   Run Task
      ↓
 Remove Lock
```

Python example using a simple lock file:

```python
from pathlib import Path
import sys

lock_file = Path("/tmp/security_monitor.lock")

if lock_file.exists():
    print("Script is already running.")
    sys.exit(1)

try:
    lock_file.touch()

    print("Running security task...")

    # Your task here

finally:
    if lock_file.exists():
        lock_file.unlink()
```

For production systems, use a robust locking mechanism rather than relying on a simplistic lock-file implementation.

---

# 🔁 20. Retry Logic

Some failures are temporary.

For example:

```text
Network request
      ↓
Failed
      ↓
Wait
      ↓
Retry
      ↓
Success
```

Example:

```python
import time
import requests

for attempt in range(3):
    try:
        response = requests.get(
            "https://example.com",
            timeout=5
        )

        print("Request successful")
        break

    except requests.RequestException:
        print("Request failed")

        if attempt < 2:
            time.sleep(5)
```

Retries should be used carefully.

Repeatedly retrying a failed operation can create unnecessary traffic or load.

---

# 📈 21. Reliability Model

A reliable scheduled security script should follow:

```text
SCHEDULE
   ↓
EXECUTE
   ↓
VALIDATE
   ↓
LOG
   ↓
HANDLE ERRORS
   ↓
RETRY IF APPROPRIATE
   ↓
REPORT RESULT
   ↓
MONITOR EXECUTION
```

This turns a basic script into a more dependable automation system.

---

# 🔐 22. Security Considerations for Scheduled Jobs

Scheduled scripts themselves can become a security risk if configured poorly.

### Protect the script

Avoid:

```text
World-writable scripts
```

If another user can modify a scheduled security script, they may be able to execute malicious commands with the permissions of the scheduler.

---

### Protect credentials

Avoid hardcoding:

```python
password = "MyPassword123"
```

Prefer secure configuration mechanisms such as:

* Environment variables
* Protected configuration files
* Secret-management systems

---

### Use least privilege

A monitoring script may only need:

```text
Read logs
Read system information
Write its own report
```

It may not need:

```text
Full administrator privileges
```

---

### Secure output files

Reports and logs may contain sensitive information.

Protect them with appropriate:

* File permissions
* Ownership
* Storage location
* Retention policies

---

# 🧠 23. Real-World Security Automation Example

Imagine a daily security report system.

### Schedule

```text
Every day at 18:00
```

### Workflow

```text
             Scheduler
                 ↓
          Start Python Script
                 ↓
          Collect Security Logs
                 ↓
             Parse Data
                 ↓
          Detect Events
                 ↓
          Generate Statistics
                 ↓
          Generate Report
                 ↓
          Save Report
                 ↓
       Log Execution Result
                 ↓
       Notify if Script Failed
```

This combines many topics you've already learned:

```text
Python
 ↓
File Handling
 ↓
JSON / CSV
 ↓
Log Processing
 ↓
Security Data Processing
 ↓
Monitoring
 ↓
Workflow Automation
 ↓
Script Scheduling
```

---

# 🧩 24. Cron vs Python `time.sleep()`

You might wonder:

> Why not just create an infinite Python loop?

Example:

```python
import time

while True:
    security_check()

    time.sleep(300)
```

This works for some simple cases, but it has limitations.

| Python Loop                         | Scheduler                                        |
| ----------------------------------- | ------------------------------------------------ |
| Script must remain running          | Script can start when needed                     |
| Uses a persistent process           | Scheduler manages execution                      |
| Script must handle its own timing   | Scheduler handles timing                         |
| Restart may require manual handling | Scheduler can provide better service integration |
| Useful for continuous monitoring    | Useful for scheduled jobs                        |

A continuously running monitor and a scheduled task are different approaches.

---

# 🔄 25. Scheduling vs Continuous Monitoring

### Scheduled monitoring

```text
Run
 ↓
Check
 ↓
Exit
 ↓
Wait
 ↓
Run again
```

### Continuous monitoring

```text
Start
 ↓
Monitor
 ↓
Monitor
 ↓
Monitor
 ↓
Monitor
 ↓
...
```

Choose based on the task.

For example:

```text
Daily report → Scheduled
Hourly file check → Scheduled
Real-time event monitoring → Continuous/event-driven
```

---

# 🧪 26. Hands-On Practice

## 🟢 Task 1 — Create a Scheduled Test Script

Create:

```text
scheduler_test.py
```

It should append the current time to:

```text
scheduler_test.log
```

Example:

```text
Execution: 2026-09-25 12:00:00
Execution: 2026-09-25 12:01:00
Execution: 2026-09-25 12:02:00
```

---

## 🟡 Task 2 — Schedule It With Cron

Open:

```bash
crontab -e
```

Schedule the script every minute.

Then verify:

```bash
cat scheduler_test.log
```

---

## 🟠 Task 3 — Security Monitoring Script

Create a Python script that checks:

```text
CPU usage
Memory usage
Disk usage
```

Use the `psutil` library you learned earlier.

Example output:

```text
SYSTEM SECURITY CHECK

CPU: 34%
Memory: 61%
Disk: 72%

Status: NORMAL
```

Schedule it every 5 minutes.

---

## 🔴 Task 4 — Failure Logging

Modify the script so that it records:

```text
START
SUCCESS
FAILURE
ERROR MESSAGE
END
```

Example:

```text
2026-09-25 12:00:00 - START
2026-09-25 12:00:01 - SUCCESS
2026-09-25 12:00:01 - END
```

---

## 🔥 Task 5 — Build a Mini Security Scheduler

Combine your previous topics:

```text
             CRON
               ↓
        Python Security Script
               ↓
        ┌──────┼──────┐
        ↓      ↓      ↓
      Logs   System   Files
        ↓      ↓      ↓
        └──────┼──────┘
               ↓
            Analyze
               ↓
          Detect Event
               ↓
         Generate Alert
               ↓
          Save Report
               ↓
         Log Execution
```

This becomes your first small **scheduled security automation system**.

---

# 🧠 Memory Tricks

### Scheduling

> **S → R → M → L**

```text
S = Schedule
R = Run
M = Monitor
L = Log
```

### Reliable Automation

> **E → H → L → R → M**

```text
E = Execute
H = Handle errors
L = Log
R = Retry when appropriate
M = Monitor
```

### Cron

> **Minute → Hour → Day → Month → Weekday**

```text
* * * * *
```

---

# 🎯 Interview Questions

### 1. What is script scheduling?

Script scheduling is the automatic execution of a script at predefined times or intervals.

### 2. What is cron?

`cron` is a Linux scheduling mechanism used to execute commands or scripts according to a defined schedule.

### 3. What is crontab?

A crontab is a configuration containing scheduled cron jobs.

### 4. What are the five cron fields?

```text
Minute
Hour
Day of month
Month
Day of week
```

### 5. Why should scheduled scripts use logging?

Logging allows you to determine whether the script executed successfully and helps investigate failures.

### 6. What is an exit code?

An exit code is a value returned by a program indicating its execution result. Conventionally, `0` indicates success and non-zero values indicate an error or abnormal result.

### 7. Why are absolute paths useful in cron jobs?

Scheduled jobs may run with a different working directory or environment, so absolute paths reduce path-related failures.

### 8. Why can overlapping executions be a problem?

Multiple instances can consume resources, duplicate work, generate duplicate alerts, or conflict when accessing files.

### 9. What is a systemd timer?

A systemd timer is a systemd unit used to trigger another unit, commonly a service, according to a schedule or time condition.

### 10. Why is least privilege important for scheduled scripts?

If a scheduled script is compromised, its permissions determine what an attacker could potentially do through it.

---

# ⚡ Quick Revision

| Concept           | Meaning                                             |
| ----------------- | --------------------------------------------------- |
| Script Scheduling | Running scripts automatically at specified times    |
| Cron              | Linux scheduling mechanism                          |
| Crontab           | Configuration containing cron jobs                  |
| Job Management    | Creating, modifying, monitoring and removing jobs   |
| Exit Code         | Indicates program execution result                  |
| Logging           | Records execution history                           |
| Retry             | Attempts a temporary failed operation again         |
| Timeout           | Prevents operations from waiting indefinitely       |
| Overlap           | Multiple instances running simultaneously           |
| systemd Timer     | systemd-based scheduling mechanism                  |
| Reliability       | Ability to execute consistently and handle failures |

---

# 🏁 Final Takeaway

Script scheduling transforms a Python security script from something you **run manually** into something that can operate as part of an automated security system.

```text
             SCHEDULE
                 ↓
              EXECUTE
                 ↓
              MONITOR
                 ↓
               LOG
                 ↓
          HANDLE ERRORS
                 ↓
          RETRY IF NEEDED
                 ↓
          REPORT RESULT
                 ↓
        DETECT EXECUTION FAILURE
                 ↓
               ALERT
```

Remember:

> **Scheduling answers "WHEN should the script run?"**

> **Automation answers "WHAT should it do?"**

> **Monitoring answers "DID it work?"**

> **Reliability answers "WHAT happens when something goes wrong?"**

That combination is what turns a simple Python script into a dependable **security automation job**.

This topic gives you the scheduling layer. Your next Security Automation topic can build on it with **event-driven automation**, where scripts respond to events instead of only running at fixed times.
