# 🐍 Understand Exception Handling

> **"Exception handling allows a Python program to detect and handle unexpected problems without crashing unnecessarily."**

---

# 📖 What is Exception Handling?

When a Python program encounters an unexpected problem while running, it can produce an **exception**.

Example:

```python
number = int(input("Enter a number: "))
```

If the user enters:

```text
hello
```

Python cannot convert `"hello"` into an integer.

It produces an exception:

```text
ValueError
```

Without proper handling, the program stops.

**Exception handling** allows us to anticipate and handle such problems gracefully.

---

# ⚙️ How Exception Handling Works

The basic flow is:

```text
        Program Starts
             │
             ▼
       Execute Code
             │
        ┌────┴────┐
        │         │
      Works      Error
        │         │
        ▼         ▼
     Continue   Exception
                  │
                  ▼
             Handle Error
                  │
                  ▼
              Continue
```

Instead of:

```text
Error → Program Crash
```

we can have:

```text
Error → Handle → Inform User → Continue/Exit Safely
```

---

# ⚠️ 1. Error Handling

Errors are problems that prevent a program from working as intended.

Python errors can generally be understood as:

### 1. Syntax Errors

Problems with Python syntax.

```python
if True
    print("Hello")
```

Python reports a syntax error because the `:` is missing.

---

### 2. Runtime Errors / Exceptions

The program starts running but encounters a problem.

Example:

```python
number = 10 / 0
```

Result:

```text
ZeroDivisionError
```

---

### 3. Logical Errors

The program runs but produces the wrong result.

```python
a = 10
b = 20

result = a - b

print(result)
```

The code works, but perhaps we intended addition.

---

# 🆚 Types of Problems

| Problem       | When it occurs        | Example           |
| ------------- | --------------------- | ----------------- |
| Syntax Error  | Code cannot be parsed | Missing `:`       |
| Exception     | Runtime problem       | Divide by zero    |
| Logical Error | Wrong program logic   | Wrong calculation |

Exception handling mainly deals with **runtime exceptions**.

---

# 🧩 Common Python Exceptions

Python provides many built-in exception types.

| Exception             | Example Cause                 |
| --------------------- | ----------------------------- |
| `ValueError`          | Invalid value conversion      |
| `TypeError`           | Wrong data type operation     |
| `ZeroDivisionError`   | Division by zero              |
| `FileNotFoundError`   | File doesn't exist            |
| `IndexError`          | Invalid list index            |
| `KeyError`            | Missing dictionary key        |
| `NameError`           | Variable doesn't exist        |
| `PermissionError`     | Insufficient file permissions |
| `ModuleNotFoundError` | Module cannot be found        |

---

# 🛡️ 2. `try-except` Blocks

The basic structure is:

```python
try:
    # risky code
except:
    # handle error
```

Example:

```python
try:
    number = int(input("Enter a number: "))
    print(number)
except:
    print("Invalid input")
```

If the user enters:

```text
25
```

Output:

```text
25
```

If the user enters:

```text
hello
```

Output:

```text
Invalid input
```

The program doesn't unexpectedly crash at that point.

---

# 🎯 Handling Specific Exceptions

It is better to catch specific exceptions instead of using a completely generic `except`.

```python
try:
    number = int(input("Enter a number: "))
except ValueError:
    print("Please enter a valid number.")
```

This tells Python exactly which problem we expect.

---

# 🔢 Multiple Exceptions

We can handle different exceptions separately.

```python
try:
    a = int(input("Enter first number: "))
    b = int(input("Enter second number: "))

    result = a / b

    print(result)

except ValueError:
    print("Please enter numbers only.")

except ZeroDivisionError:
    print("Cannot divide by zero.")
```

Example:

```text
Enter first number: 10
Enter second number: 0
```

Output:

```text
Cannot divide by zero.
```

---

# 🔗 Multiple Exceptions in One Block

If different exceptions require the same response:

```python
try:
    number = int(input("Enter number: "))
except (ValueError, TypeError):
    print("Invalid input.")
```

---

# 🧹 `finally`

The `finally` block runs whether an exception occurs or not.

```python
try:
    file = open("data.txt")
    data = file.read()

except FileNotFoundError:
    print("File not found.")

finally:
    print("Operation completed.")
```

The `finally` block is useful for cleanup operations.

```text
try
 │
 ├── Success ──────┐
 │                 │
 └── Exception ────┤
                   ▼
                finally
```

---

# 🔄 `else`

Python also provides an `else` block.

It runs **only when no exception occurs**.

```python
try:
    number = int(input("Enter number: "))

except ValueError:
    print("Invalid number.")

else:
    print("You entered:", number)
```

The complete structure is:

```python
try:
    # risky operation

except SomeError:
    # handle error

else:
    # runs if no error

finally:
    # always runs
```

---

# 🆚 `try`, `except`, `else`, `finally`

| Block     | Purpose                            |
| --------- | ---------------------------------- |
| `try`     | Code that may cause an exception   |
| `except`  | Handles the exception              |
| `else`    | Runs when no exception occurs      |
| `finally` | Runs regardless of success/failure |

---

# 🚨 Raising Exceptions

Sometimes our own program needs to deliberately generate an exception.

We can use:

```python
raise
```

Example:

```python
age = -5

if age < 0:
    raise ValueError("Age cannot be negative")
```

This is useful when validating data.

---

# 🧱 Custom Exceptions

For larger applications, we can create our own exception types.

```python
class InvalidIPError(Exception):
    pass
```

Then:

```python
raise InvalidIPError("Invalid IP address")
```

Custom exceptions can make large applications easier to understand.

---

# 🐞 3. Debugging

**Debugging** is the process of finding and fixing problems in a program.

```text
Bug
 │
 ▼
Detect
 │
 ▼
Investigate
 │
 ▼
Find Cause
 │
 ▼
Fix
 │
 ▼
Test
```

Debugging is an essential programming skill.

---

# 🔍 Common Debugging Techniques

### 1. Read the Error Message

Example:

```text
ValueError: invalid literal for int()
```

This tells us that Python couldn't convert something into an integer.

Don't immediately ignore error messages.

They often tell you:

* What went wrong
* Which exception occurred
* Where it happened

---

### 2. Check the Traceback

Example:

```text
Traceback (most recent call last):
  File "scanner.py", line 15
    port = int(value)
ValueError: invalid literal for int()
```

Important information:

```text
File       → scanner.py
Line       → 15
Exception  → ValueError
```

---

### 3. Use `print()` for Simple Debugging

```python
username = "Farhan"

print("DEBUG:", username)
```

This helps us understand what values the program is processing.

---

### 4. Use Assertions

An assertion checks whether an assumption is true.

```python
port = 443

assert port > 0
```

If the condition is false, Python raises:

```text
AssertionError
```

Example:

```python
port = -1

assert port > 0, "Port must be positive"
```

---

# 🐍 Using a Debugger

Python development environments can provide debuggers that allow us to:

* Set breakpoints
* Execute code step-by-step
* Inspect variables
* Follow program flow
* Examine the call stack

Conceptually:

```text
Program
   │
   ▼
Breakpoint
   │
   ▼
Pause
   │
   ├── Inspect variables
   ├── Step over
   ├── Step into
   └── Continue
```

Debuggers become especially useful as programs become larger.

---

# 📝 4. Logging Errors

`print()` is useful for simple debugging, but production programs often use **logging**.

Python provides a built-in `logging` module.

```python
import logging
```

Basic example:

```python
import logging

logging.basicConfig(level=logging.INFO)

logging.info("Program started")
```

Output:

```text
INFO:root:Program started
```

---

# 📊 Logging Levels

Python provides several standard logging levels:

```text
DEBUG
INFO
WARNING
ERROR
CRITICAL
```

Their general meaning:

| Level      | Meaning                                        |
| ---------- | ---------------------------------------------- |
| `DEBUG`    | Detailed debugging information                 |
| `INFO`     | Normal program events                          |
| `WARNING`  | Something unexpected but not necessarily fatal |
| `ERROR`    | An operation failed                            |
| `CRITICAL` | Serious failure                                |

---

# 🔍 Logging Example

```python
import logging

logging.basicConfig(level=logging.INFO)

logging.debug("Debug information")
logging.info("Program started")
logging.warning("Configuration is missing")
logging.error("Unable to read file")
logging.critical("System failure")
```

If the logging level is `INFO`, `DEBUG` messages may not be displayed.

---

# 🚨 Logging an Exception

A useful feature is:

```python
logging.exception()
```

Example:

```python
import logging

logging.basicConfig(level=logging.ERROR)

try:
    result = 10 / 0

except ZeroDivisionError:
    logging.exception("Calculation failed")
```

This records the exception and traceback, making debugging easier.

---

# 📁 Logging to a File

Instead of displaying logs only on the terminal:

```python
import logging

logging.basicConfig(
    filename="application.log",
    level=logging.INFO
)

logging.info("Application started")
logging.warning("Suspicious event detected")
logging.error("File processing failed")
```

The events are stored in:

```text
application.log
```

This is useful for long-running applications and security tools.

---

# 🛡️ Logging in Cybersecurity

Logging is extremely important for security applications.

For example:

```python
import logging

logging.basicConfig(
    filename="security.log",
    level=logging.INFO
)

try:
    username = input("Username: ")

    if username == "":
        raise ValueError("Empty username")

except ValueError:
    logging.exception("Invalid username input")
```

The application can maintain a record of errors without relying entirely on terminal output.

---

# 🔐 Security Consideration: Don't Log Secrets

Never casually log sensitive information such as:

```text
Passwords
API keys
Authentication tokens
Private keys
Session cookies
```

Bad:

```python
logging.info("Password: " + password)
```

Better:

```python
logging.info("Password validation attempted")
```

Log useful security information without exposing secrets.

---

# 🏗️ 5. Building Reliable Scripts

A reliable script should be able to handle expected problems without unexpectedly crashing or producing dangerous results.

A basic reliable-script design:

```text
             START
               │
               ▼
        Validate Input
               │
               ▼
          Try Operation
               │
        ┌──────┴──────┐
        ▼             ▼
     Success         Error
        │             │
        ▼             ▼
    Process        Handle Error
        │             │
        └──────┬──────┘
               ▼
          Log Result
               │
               ▼
             END
```

---

# 🧩 Input Validation

Don't blindly trust user input.

Example:

```python
try:
    port = int(input("Enter port: "))

    if not 1 <= port <= 65535:
        raise ValueError("Invalid port number")

except ValueError as error:
    print("Error:", error)
```

This checks both:

1. Whether the input is an integer.
2. Whether it is within the valid port range.

---

# 🔄 Retry Logic

Sometimes an operation can temporarily fail.

For example:

```python
attempts = 3

for attempt in range(attempts):
    try:
        number = int(input("Enter a number: "))
        print("Valid input:", number)
        break

    except ValueError:
        print("Invalid input")
```

This allows the program to recover from temporary user mistakes.

---

# 🛑 Fail Safely

A reliable security script should avoid continuing with dangerous or invalid data.

For example:

```python
try:
    with open("config.json") as file:
        config = json.load(file)

except FileNotFoundError:
    print("Configuration file missing.")
```

Instead of assuming the configuration exists and continuing with invalid values.

---

# 🧱 Functions + Exception Handling

Exception handling becomes even more useful when combined with functions.

```python
def read_file(filename):
    try:
        with open(filename, "r") as file:
            return file.read()

    except FileNotFoundError:
        return None
```

Then:

```python
data = read_file("server.log")

if data is None:
    print("Could not read log file.")
else:
    print(data)
```

This keeps error-handling logic organized.

---

# 🛡️ Cybersecurity Example — Safe Log Analyzer

A basic log analyzer:

```python
import logging

logging.basicConfig(
    filename="analyzer.log",
    level=logging.INFO
)

def analyze_log(filename):

    try:
        with open(filename, "r") as file:

            for line in file:
                if "FAILED LOGIN" in line:
                    print("Alert:", line.strip())

        logging.info("Log analysis completed successfully")

    except FileNotFoundError:
        logging.error("Log file not found: %s", filename)

    except PermissionError:
        logging.error("Permission denied: %s", filename)

    except OSError as error:
        logging.error("File operation failed: %s", error)


analyze_log("auth.log")
```

This script:

```text
Input File
    │
    ▼
Try Opening
    │
    ├── File Missing ──► Log Error
    │
    ├── Permission ────► Log Error
    │
    └── Success
          │
          ▼
      Analyze Logs
          │
          ▼
      Log Result
```

This is much more reliable than simply doing:

```python
file = open("auth.log")
```

and allowing unexpected errors to terminate the program.

---

# 🆚 `print()` vs Logging

| `print()`                  | Logging                    |
| -------------------------- | -------------------------- |
| Simple output              | Structured event recording |
| Mainly for users/debugging | Useful for applications    |
| Usually temporary          | Can be stored permanently  |
| Limited severity levels    | Multiple log levels        |
| Less configurable          | Highly configurable        |

Use `print()` when you want to communicate simple information to the user.

Use **logging** when you need a record of application events.

---

# ⚠️ Common Mistakes

### 1. Catching Every Exception

Avoid:

```python
try:
    risky_operation()
except:
    pass
```

This can hide important problems.

Prefer:

```python
try:
    risky_operation()
except ValueError:
    print("Invalid value")
```

---

### 2. Ignoring Errors

Bad:

```python
try:
    data = process()
except:
    pass
```

Now you don't know whether the operation succeeded.

---

### 3. Showing Technical Errors to Users

Instead of exposing a complicated traceback to a normal user:

```text
Traceback...
PermissionError...
/internal/path/...
```

provide a useful message:

```text
Unable to access the file. Check your permissions.
```

The detailed error can still be logged internally.

---

### 4. Logging Sensitive Information

Never accidentally record:

```text
password=123456
api_key=...
token=...
```

in logs.

---

# 🧠 Memory Trick

Remember:

```text
TRY
 ↓
Something might fail
 ↓
EXCEPT
 ↓
Handle the problem
 ↓
ELSE
 ↓
Success-only code
 ↓
FINALLY
 ↓
Cleanup
```

For debugging:

```text
DETECT → INVESTIGATE → FIX → TEST
```

For reliable scripts:

```text
VALIDATE → TRY → HANDLE → LOG → RECOVER
```

---

# 📝 Quick Revision

### Basic Exception Handling

```python
try:
    risky_code()

except ValueError:
    print("Invalid value")
```

### Multiple Exceptions

```python
try:
    operation()

except ValueError:
    print("Invalid value")

except FileNotFoundError:
    print("File missing")
```

### `finally`

```python
try:
    operation()

except Exception:
    print("Error")

finally:
    print("Finished")
```

### Raise an Exception

```python
if value < 0:
    raise ValueError("Value cannot be negative")
```

### Logging

```python
import logging

logging.basicConfig(level=logging.INFO)

logging.info("Program started")
logging.warning("Warning")
logging.error("Error occurred")
```

### Log an Exception

```python
try:
    operation()

except Exception:
    logging.exception("Operation failed")
```

---

# 💡 Interview Tip

### Q: What is exception handling?

> Exception handling is a mechanism for detecting and handling runtime errors so that programs can respond safely instead of failing unexpectedly.

### Q: What is `try-except`?

> `try` contains code that may raise an exception, while `except` handles the specified exception.

### Q: What is `finally`?

> `finally` contains code that executes regardless of whether an exception occurred.

### Q: What is debugging?

> Debugging is the process of identifying, analyzing, and fixing errors or unexpected behavior in a program.

### Q: Why use logging instead of `print()`?

> Logging provides structured, configurable, and persistent records of program events and errors.

### Q: Why should we avoid `except:`?

> A bare `except` can catch unexpected exceptions and hide important problems. Catching specific exceptions makes programs easier to debug and maintain.

---

# 🧪 Mini Practice

### Task 1 — Safe Integer Input

Create a program that asks the user for a number.

If the user enters something invalid:

```text
Invalid number
```

should be displayed instead of crashing.

---

### Task 2 — Safe Division

Create:

```python
def divide(a, b):
    ...
```

Handle:

```text
ZeroDivisionError
```

Example:

```python
divide(10, 0)
```

Expected:

```text
Cannot divide by zero
```

---

### Task 3 — Safe File Reader

Create:

```python
def read_file(filename):
    ...
```

Handle:

```text
FileNotFoundError
PermissionError
```

---

### Task 4 — Error Logger

Create a program that attempts to open:

```text
security.log
```

If the file doesn't exist, record the error using Python's `logging` module.

---

### Task 5 — Log Analyzer

Create a program that reads:

```text
auth.log
```

and:

1. Counts failed login attempts.
2. Prints each failed login.
3. Handles a missing file.
4. Logs the error if the file cannot be opened.

---

# 📚 Summary

Exception handling allows Python programs to deal with unexpected runtime problems in a controlled way.

The important concepts are:

```text
                 EXCEPTION HANDLING
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
    Error Handling    Debugging        Logging
        │                │                │
        ▼                ▼                ▼
    try-except       Find & Fix       Record Events
        │
        ├── try
        ├── except
        ├── else
        └── finally
                         │
                         ▼
                  Reliable Scripts
```

> **Remember:**

> `try` contains code that may fail.

> `except` handles specific exceptions.

> `else` runs when no exception occurs.

> `finally` runs regardless of success or failure.

> Debugging means finding and fixing problems.

> Logging creates a useful record of application events and errors.

> Don't hide exceptions with unnecessary `except: pass`.

> Never expose passwords, tokens, API keys, or other secrets in logs.

> Reliable scripts validate input, handle expected failures, log important events, and fail safely.
