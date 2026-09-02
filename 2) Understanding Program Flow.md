# 🐍 Understanding Program Flow

> **"Program flow is the order in which instructions are executed by a program. Python uses conditions, loops, nested logic, and control statements to determine what code runs, when it runs, and how many times it runs."**

---

# 📖 What is Program Flow?

**Program Flow** refers to the sequence in which a program executes its instructions.

Normally, Python executes code from **top to bottom**.

```text
Start
  │
  ▼
Statement 1
  │
  ▼
Statement 2
  │
  ▼
Statement 3
  │
  ▼
End
```

However, real programs need to make decisions, repeat tasks, and skip or stop certain operations.

Python provides:

* 🔀 Conditional statements
* 🔁 Loops
* 🧩 Nested conditions
* 🛑 Control statements
* 🧠 Problem-solving workflows

These features allow programs to make decisions and automate repetitive tasks.

---

# 🔀 Conditional Statements

## 📖 What is a Conditional Statement?

A **conditional statement** allows a program to make a decision based on whether a condition is `True` or `False`.

The most common conditional statements in Python are:

* `if`
* `elif`
* `else`

---

## ⚙️ How `if` Works

```python
age = 20

if age >= 18:
    print("Adult")
```

The condition:

```python
age >= 18
```

is evaluated.

```text
Condition
    │
    ▼
 Is it True?
   /    \
 Yes     No
 │        │
 ▼        ▼
Execute  Skip
```

Since `20 >= 18` is `True`, Python executes the `print()` statement.

---

# 🧩 The `if` Statement

The `if` statement executes code only when its condition is true.

```python
port = 443

if port == 443:
    print("HTTPS port detected")
```

Output:

```text
HTTPS port detected
```

---

# 🔀 The `else` Statement

`else` executes when the `if` condition is false.

```python
port = 80

if port == 443:
    print("HTTPS")
else:
    print("Not HTTPS")
```

Output:

```text
Not HTTPS
```

---

# 🔄 The `elif` Statement

`elif` means **"else if"**.

It allows a program to check multiple conditions.

```python
status_code = 404

if status_code == 200:
    print("Success")
elif status_code == 404:
    print("Not Found")
else:
    print("Other status")
```

Output:

```text
Not Found
```

---

## 🌐 Real-World Cybersecurity Example

A security program could classify HTTP status codes:

```python
status_code = 403

if status_code == 200:
    print("Request successful")
elif status_code == 403:
    print("Access forbidden")
elif status_code == 404:
    print("Resource not found")
else:
    print("Other response")
```

This type of decision-making is commonly used when processing network or web data.

---

# 🔁 Loops

## 📖 What is a Loop?

A **loop** repeatedly executes a block of code.

Loops are useful when the same operation needs to be performed multiple times.

```text
Start
  │
  ▼
Check Condition
  │
  ▼
Execute Code
  │
  ▼
Repeat
  │
  └───────►
```

Python provides two primary loops:

* `for`
* `while`

---

# 🔢 `for` Loop

A `for` loop is commonly used when iterating through a collection or a known sequence of values.

```python
ports = [22, 80, 443]

for port in ports:
    print(port)
```

Output:

```text
22
80
443
```

---

## ⚙️ How a `for` Loop Works

```text
ports
  │
  ▼
22 ──► Execute
  │
  ▼
80 ──► Execute
  │
  ▼
443 ─► Execute
  │
  ▼
End
```

Python takes each item from the collection and assigns it to the loop variable.

---

# 🔢 Using `range()`

`range()` generates a sequence of numbers.

```python
for i in range(5):
    print(i)
```

Output:

```text
0
1
2
3
4
```

The ending value `5` is not included.

---

## 🔐 Cybersecurity Example

Suppose you want to process several ports:

```python
ports = [22, 80, 443, 8080]

for port in ports:
    print(f"Checking port {port}")
```

Output:

```text
Checking port 22
Checking port 80
Checking port 443
Checking port 8080
```

This demonstrates how loops can automate repetitive security tasks.

---

# 🔄 `while` Loop

A `while` loop continues executing as long as its condition is `True`.

```python
count = 1

while count <= 5:
    print(count)
    count += 1
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

## ⚙️ How `while` Works

```text
        ┌──────────────┐
        │   Condition  │
        └──────┬───────┘
               │
          True │ False
               │
               ▼
          Execute Code
               │
               ▼
        Update Variable
               │
               └──────► Condition
                             
False ─────────────────────► End
```

---

# ⚠️ Infinite Loops

A loop that never becomes false can continue indefinitely.

Example:

```python
count = 1

while count <= 5:
    print(count)
```

The value of `count` never changes, so the condition remains true.

This creates an **infinite loop**.

Correct version:

```python
count = 1

while count <= 5:
    print(count)
    count += 1
```

---

# ⚖️ `for` vs `while`

| `for` Loop                                  | `while` Loop                                          |
| ------------------------------------------- | ----------------------------------------------------- |
| Used to iterate through sequences           | Runs while a condition is true                        |
| Useful when iteration is known              | Useful when repetition depends on a condition         |
| Common with lists and ranges                | Common with conditions                                |
| Less likely to accidentally become infinite | Can easily become infinite if condition never changes |

---

# 🧩 Nested Conditions

## 📖 What is a Nested Condition?

A **nested condition** is a conditional statement placed inside another conditional statement.

Example:

```python
username = "admin"
password_correct = True

if username == "admin":
    if password_correct:
        print("Login successful")
```

Here, the second `if` exists inside the first `if`.

---

## ⚙️ How Nested Conditions Work

```text
Check Username
      │
      ▼
   Is Admin?
   /       \
 No         Yes
 │           │
End     Check Password
              │
              ▼
       Password Correct?
          /       \
        No         Yes
        │           │
       Deny       Allow
```

---

# 🔐 Cybersecurity Example

A system might check whether a user is an administrator before checking whether they have a valid authentication token.

```python
is_admin = True
token_valid = True

if is_admin:
    if token_valid:
        print("Administrative access granted")
    else:
        print("Invalid token")
else:
    print("Administrative privileges required")
```

This demonstrates multiple decision levels.

---

# 🧠 Nested Conditions vs Logical Operators

Sometimes nested conditions can be simplified using logical operators.

### Nested:

```python
if is_admin:
    if token_valid:
        print("Access granted")
```

### Using `and`:

```python
if is_admin and token_valid:
    print("Access granted")
```

Both can express similar logic, but the best choice depends on readability and complexity.

---

# 🛑 Control Statements

## 📖 What are Control Statements?

**Control statements** change the normal flow of a loop or program.

Important Python control statements include:

* `break`
* `continue`
* `pass`

---

# 🛑 `break`

The `break` statement immediately terminates a loop.

```python
for port in [22, 80, 443, 8080]:

    if port == 443:
        break

    print(port)
```

Output:

```text
22
80
```

When Python reaches `443`, the loop stops.

---

## ⚙️ How `break` Works

```text
Start Loop
    │
    ▼
Process Item
    │
    ▼
Condition Met?
   /      \
 No       Yes
 │         │
 ▼         ▼
Continue  break
            │
            ▼
           End
```

---

# ⏭️ `continue`

The `continue` statement skips the current iteration and moves to the next one.

```python
for port in [22, 80, 443]:

    if port == 80:
        continue

    print(port)
```

Output:

```text
22
443
```

Port `80` is skipped.

---

## ⚖️ `break` vs `continue`

| `break`                                     | `continue`                           |
| ------------------------------------------- | ------------------------------------ |
| Stops the entire loop                       | Skips current iteration              |
| Loop ends                                   | Loop continues                       |
| Used when further processing is unnecessary | Used when one item should be skipped |

---

# 💤 `pass`

`pass` does nothing.

It acts as a placeholder when Python requires a statement but you do not want to execute anything yet.

```python
for port in [80, 443]:

    if port == 80:
        pass

    print(port)
```

Output:

```text
80
443
```

`pass` is useful when developing code incrementally.

---

# ⚖️ `break` vs `continue` vs `pass`

| Statement  | Purpose                |
| ---------- | ---------------------- |
| `break`    | Exit the loop          |
| `continue` | Skip current iteration |
| `pass`     | Do nothing             |

---

# 🧩 Nested Loops

A loop can exist inside another loop.

```python
users = ["admin", "guest"]
ports = [80, 443]

for user in users:
    for port in ports:
        print(user, port)
```

Output:

```text
admin 80
admin 443
guest 80
guest 443
```

---

## ⚠️ Be Careful with Nested Loops

Nested loops can increase the amount of processing significantly.

For example:

```text
10 users
   ×
100 ports
   │
   ▼
1000 operations
```

With large datasets, inefficient nested loops can make programs slow.

---

# 🧠 Problem-Solving Workflows

## 📖 What is a Problem-Solving Workflow?

A **problem-solving workflow** is a structured process used to turn a problem into a working solution.

Instead of immediately writing code, first understand the problem and design the solution.

---

## 🔄 General Workflow

```text
Problem
   │
   ▼
Understand Requirements
   │
   ▼
Break Into Smaller Problems
   │
   ▼
Design Algorithm
   │
   ▼
Write Pseudocode
   │
   ▼
Write Python Code
   │
   ▼
Test
   │
   ▼
Debug
   │
   ▼
Improve
   │
   ▼
Final Solution
```

---

# 1️⃣ Understand the Problem

Before writing code, identify:

* What is the input?
* What is the expected output?
* What conditions exist?
* What limitations exist?
* What could go wrong?

---

# 2️⃣ Break the Problem Down

Large problems should be divided into smaller tasks.

Example:

**Problem:**

Detect whether an IP has too many failed login attempts.

Break it into:

```text
Read Log
   ↓
Extract IP addresses
   ↓
Count failed attempts
   ↓
Compare against threshold
   ↓
Identify suspicious IPs
```

---

# 3️⃣ Create an Algorithm

An **algorithm** is a step-by-step procedure for solving a problem.

Example:

```text
1. Read failed login records
2. Extract IP address
3. Count attempts
4. Check if count > 5
5. Mark IP as suspicious
6. Display result
```

---

# 4️⃣ Write Pseudocode

Pseudocode describes program logic without worrying about exact Python syntax.

```text
START

Read failed login attempts

FOR each IP:
    Count failed attempts

    IF attempts > 5:
        Mark IP as suspicious

Display suspicious IPs

END
```

Pseudocode helps you understand the logic before coding.

---

# 5️⃣ Implement in Python

After designing the logic, write actual Python code.

```python
failed_attempts = 7

if failed_attempts > 5:
    print("⚠️ Suspicious login activity")
else:
    print("Activity appears normal")
```

---

# 6️⃣ Test the Program

Testing checks whether the program behaves correctly.

Test normal cases:

```text
Attempts = 3
Expected → Normal
```

Test suspicious cases:

```text
Attempts = 8
Expected → Suspicious
```

Test boundary cases:

```text
Attempts = 5
Expected → Depends on defined threshold
```

---

# 7️⃣ Debug

**Debugging** means finding and fixing errors in a program.

Common types of errors include:

### Syntax Errors

Incorrect Python syntax.

```python
if age > 18
    print("Adult")
```

Missing `:` causes an error.

---

### Runtime Errors

Errors that occur while the program is running.

```python
number = int("hello")
```

This produces a `ValueError`.

---

### Logical Errors

The program runs but produces the wrong result.

Example:

```python
attempts = 5

if attempts >= 10:
    print("Attack detected")
```

If the intended threshold is 5, the logic is incorrect.

---

# 🔐 Cybersecurity Problem-Solving Example

## Problem

Identify suspicious login activity.

### Input

```text
admin → 2 failed attempts
root → 8 failed attempts
guest → 1 failed attempt
```

### Logic

```text
FOR each user
      │
      ▼
Check failed attempts
      │
      ▼
Attempts > 5?
   /       \
 No         Yes
 │           │
Normal    Suspicious
```

### Python

```python
login_attempts = {
    "admin": 2,
    "root": 8,
    "guest": 1
}

for user, attempts in login_attempts.items():

    if attempts > 5:
        print(f"⚠️ Suspicious activity: {user}")
    else:
        print(f"Normal activity: {user}")
```

Output:

```text
Normal activity: admin
⚠️ Suspicious activity: root
Normal activity: guest
```

This combines:

* Dictionary
* `for` loop
* Conditional statement
* Comparison operator
* f-string
* Security logic

---

# 🧠 Flowchart Thinking

Before coding, it can help to visualize the logic.

Example:

```text
             Start
               │
               ▼
       Get Login Attempts
               │
               ▼
        Attempts > 5 ?
          /         \
        Yes          No
         │            │
         ▼            ▼
   Suspicious       Normal
         │            │
         └──────┬─────┘
                ▼
               End
```

This makes complicated logic easier to understand.

---

# 🌍 Real-World Example

Consider a network monitoring system.

```text
Network Traffic
      │
      ▼
Read Packets
      │
      ▼
Analyze Source IP
      │
      ▼
Check Traffic Volume
      │
      ▼
Is Traffic Abnormal?
    /          \
  Yes           No
   │             │
   ▼             ▼
Generate       Continue
Alert          Monitoring
```

Python can automate this type of decision-making when combined with networking libraries and monitoring systems.

---

# ⚠️ Common Programming Mistakes

### ❌ Forgetting the colon

```python
if age > 18
```

Correct:

```python
if age > 18:
```

---

### ❌ Incorrect indentation

```python
if age > 18:
print("Adult")
```

Correct:

```python
if age > 18:
    print("Adult")
```

---

### ❌ Infinite `while` loops

```python
count = 0

while count < 5:
    print(count)
```

The condition never changes.

Correct:

```python
count = 0

while count < 5:
    print(count)
    count += 1
```

---

### ❌ Confusing `=` and `==`

```python
if age = 20:
```

`=` is assignment.

Use:

```python
if age == 20:
```

`==` performs comparison.

---

# ⚖️ `if` vs `for` vs `while`

| Feature                 | `if`            | `for`              | `while`                    |
| ----------------------- | --------------- | ------------------ | -------------------------- |
| Main purpose            | Decision        | Repetition         | Repetition                 |
| Executes multiple times | Usually no      | Yes                | Yes                        |
| Based on condition      | Yes             | Not necessarily    | Yes                        |
| Common use              | Decision-making | Collections/ranges | Condition-based repetition |

---

# 🧠 Memory Trick

Think of program flow like a cybersecurity analyst investigating an incident.

* 🔀 **`if`** = Making a security decision
* 🔄 **`for`** = Checking every item in a dataset
* ♻️ **`while`** = Continuing monitoring while a condition exists
* 🧩 **Nested conditions** = Making decisions inside decisions
* 🛑 **`break`** = Stop the investigation
* ⏭️ **`continue`** = Skip one irrelevant event
* 💤 **`pass`** = Leave something for later
* 🧠 **Problem-solving workflow** = Plan → Code → Test → Debug

---

# 📝 Quick Revision

### 🔀 Conditional Statements

* `if`
* `elif`
* `else`
* Used for decision-making

### 🔁 Loops

* `for`
* `while`
* Used for repetition

### 🧩 Nested Conditions

* Conditions inside other conditions
* Useful for multi-level decisions

### 🛑 Control Statements

* `break` → Exit loop
* `continue` → Skip iteration
* `pass` → Do nothing

### 🧠 Problem-Solving Workflow

```text
Understand
   ↓
Break Down
   ↓
Algorithm
   ↓
Pseudocode
   ↓
Code
   ↓
Test
   ↓
Debug
   ↓
Improve
```

---

# 💡 Interview Tip

### ❓What is program flow?

**Answer:** Program flow is the order in which instructions are executed by a program. Conditions, loops, and control statements allow the programmer to change the normal sequential flow.

### ❓What is the difference between `for` and `while`?

**Answer:** A `for` loop is commonly used to iterate through a sequence or collection, while a `while` loop continues executing as long as a condition remains true.

### ❓What does `break` do?

**Answer:** `break` immediately terminates the current loop.

### ❓What does `continue` do?

**Answer:** `continue` skips the current iteration and moves to the next iteration of the loop.

### ❓What is a nested condition?

**Answer:** A nested condition is a conditional statement placed inside another conditional statement.

### ❓What is an algorithm?

**Answer:** An algorithm is a step-by-step procedure used to solve a particular problem.

### ❓Why is pseudocode useful?

**Answer:** Pseudocode allows programmers to design and understand program logic before implementing it in a specific programming language.

---

# 🧪 Mini Practice

### 1. Check whether a number is positive, negative, or zero.

### 2. Write a program that checks whether a port is:

```text
22  → SSH
80  → HTTP
443 → HTTPS
Other → Unknown
```

### 3. Use a `for` loop to print:

```text
10
20
30
40
50
```

### 4. Use a `while` loop to count from 1 to 10.

### 5. Write a program that skips port `80` using `continue`.

### 6. Write a program that stops searching when it finds port `443` using `break`.

### 7. Create a program that checks login attempts and displays a warning when attempts exceed 5.

### 8. Design the pseudocode for a simple IP monitoring system.

---

# 📚 Summary

Program flow is the foundation of making Python programs **think and behave intelligently**.

```text
Conditions
    ↓
Make Decisions
    ↓
Loops
    ↓
Repeat Tasks
    ↓
Control Statements
    ↓
Control Execution
    ↓
Problem-Solving Workflow
    ↓
Build Reliable Programs
```

Understanding program flow is especially important in cybersecurity because security tools constantly need to **make decisions, process large amounts of data, repeat operations, and respond to suspicious conditions**.

> **Remember:**
>
> 🔀 **Conditions make decisions.**
>
> 🔁 **Loops repeat operations.**
>
> 🧩 **Nested conditions handle multiple levels of decisions.**
>
> 🛑 **Control statements change loop execution.**
>
> 🧠 **Algorithms provide the solution strategy.**
>
> 📝 **Pseudocode helps design the logic.**
>
> 🧪 **Testing verifies the solution.**
>
> 🐞 **Debugging finds and fixes problems.**
>
> 🔐 **Good program flow is the foundation of effective cybersecurity automation.**
