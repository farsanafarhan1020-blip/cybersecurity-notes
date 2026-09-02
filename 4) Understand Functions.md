# 🐍 Understand Functions

> **"A function is a reusable block of code designed to perform a specific task."**

---

# 📖 What is a Function?

A **function** is a named block of code that performs a particular task.

Instead of writing the same code repeatedly, we can write it once inside a function and **call it whenever we need it**.

### Without a Function

```python
name = "Farhan"
print("Hello", name)

name = "Ali"
print("Hello", name)

name = "John"
print("Hello", name)
```

The same logic is repeated.

### With a Function

```python
def greet(name):
    print("Hello", name)

greet("Farhan")
greet("Ali")
greet("John")
```

Much cleaner and reusable.

---

# ⚙️ How Functions Work

A function generally follows this structure:

```python
def function_name(parameters):
    # code to execute
    return value
```

Example:

```python
def add(a, b):
    result = a + b
    return result
```

Calling the function:

```python
answer = add(10, 20)

print(answer)
```

Output:

```text
30
```

### Function Flow

```text
       Function Definition
              │
              ▼
       def function_name()
              │
              ▼
       Function Body
              │
              ▼
       Function is stored
              │
              ▼
        Function Call
              │
              ▼
      Parameters → Processing
              │
              ▼
        Return Value
```

---

# 🔨 1. Function Creation

Functions are created using the `def` keyword.

### Basic Syntax

```python
def function_name():
    # code
```

Example:

```python
def say_hello():
    print("Hello!")
```

The function does **not** execute when it is defined.

We need to call it:

```python
say_hello()
```

Output:

```text
Hello!
```

---

## 📌 Function Naming

Good:

```python
def calculate_total():
    pass

def check_password():
    pass

def scan_port():
    pass
```

Bad:

```python
def x():
    pass

def abc123():
    pass
```

Use descriptive names that explain what the function does.

### Python Naming Convention

Python commonly uses **snake_case**:

```python
def calculate_password_strength():
    pass
```

---

# 🔢 2. Parameters

**Parameters** are variables that allow us to send data into a function.

Example:

```python
def greet(name):
    print("Hello", name)
```

Here:

```text
name
  ↑
Parameter
```

When we call:

```python
greet("Farhan")
```

`"Farhan"` is the **argument** passed to the function.

```text
Parameter → name
Argument  → "Farhan"
```

---

## 🔹 Multiple Parameters

A function can have multiple parameters.

```python
def add(a, b):
    print(a + b)

add(10, 20)
```

Output:

```text
30
```

Another example:

```python
def introduce(name, age):
    print("Name:", name)
    print("Age:", age)

introduce("Farhan", 20)
```

---

# 🎯 Parameters vs Arguments

These two terms are often confused.

| Term      | Meaning                             |
| --------- | ----------------------------------- |
| Parameter | Variable defined in the function    |
| Argument  | Actual value passed to the function |

Example:

```python
def greet(name):      # name = parameter
    print(name)

greet("Farhan")       # "Farhan" = argument
```

---

# 🛠️ Default Parameters

A parameter can have a default value.

```python
def greet(name="User"):
    print("Hello", name)
```

Now:

```python
greet()
```

Output:

```text
Hello User
```

But we can override it:

```python
greet("Farhan")
```

Output:

```text
Hello Farhan
```

---

# 🔐 Parameters in Cybersecurity

Functions are extremely useful for security tools.

For example:

```python
def check_port(port):
    print("Checking port:", port)

check_port(22)
check_port(80)
check_port(443)
```

Instead of writing separate code for every port, we can reuse the function.

---

# ↩️ 3. Return Values

A function can send a result back to the code that called it.

This is done using the `return` statement.

Example:

```python
def add(a, b):
    return a + b
```

Then:

```python
result = add(10, 20)

print(result)
```

Output:

```text
30
```

### Function Flow

```text
10 ──┐
     ├──► add() ──► 30
20 ──┘              │
                    ▼
                 result
```

---

# 🆚 `print()` vs `return`

This is an important concept.

### Using `print()`

```python
def add(a, b):
    print(a + b)
```

The function displays the result.

### Using `return`

```python
def add(a, b):
    return a + b
```

The function sends the result back.

We can then use it:

```python
result = add(10, 20)

print(result)
```

Or:

```python
result = add(10, 20) * 2
print(result)
```

Output:

```text
60
```

### Comparison

| `print()`                                | `return`                     |
| ---------------------------------------- | ---------------------------- |
| Displays information                     | Sends a value back           |
| Mainly for output/debugging              | Mainly for producing results |
| Cannot directly store the printed result | Returned value can be stored |
| Function can continue after some prints  | `return` exits the function  |

---

# ⛔ Return Stops Function Execution

Once Python reaches `return`, the function ends.

```python
def test():
    print("Start")
    return
    print("End")

test()
```

Output:

```text
Start
```

`"End"` is never executed.

---

# 📦 Returning Multiple Values

Python can return multiple values.

```python
def calculate(a, b):
    return a + b, a - b
```

Then:

```python
sum_result, difference = calculate(10, 5)

print(sum_result)
print(difference)
```

Output:

```text
15
5
```

Python actually returns them together as a tuple.

---

# 🌍 Real-World Example: Password Checking

Instead of writing password checking logic repeatedly:

```python
def check_password(password):
    if len(password) >= 8:
        return True
    else:
        return False
```

Use it:

```python
password = input("Enter password: ")

if check_password(password):
    print("Password length is acceptable")
else:
    print("Password is too short")
```

The function handles one specific responsibility.

---

# 🌐 4. Scope

**Scope** determines where a variable can be accessed in a program.

The two most important scopes for beginners are:

* Local scope
* Global scope

---

# 🔹 Local Scope

A variable created inside a function is normally **local to that function**.

```python
def test():
    message = "Hello"
    print(message)

test()
```

This works.

But:

```python
def test():
    message = "Hello"

test()

print(message)
```

This produces an error because `message` only exists inside `test()`.

```text
Outside Function
      │
      ✖
      │
   message

Inside Function
      │
      ▼
   message ✓
```

---

# 🌎 Global Scope

A variable created outside functions has global scope.

```python
name = "Farhan"

def greet():
    print(name)

greet()
```

The function can access the global variable.

---

# ⚠️ Local vs Global

```python
name = "Farhan"       # Global

def test():
    age = 20          # Local

    print(name)
    print(age)

test()
```

Inside the function:

```text
name → accessible ✓
age  → accessible ✓
```

Outside:

```python
print(name)   # ✓
print(age)    # ✗
```

---

# 🔄 Modifying Global Variables

Python allows modifying a global variable using `global`, but excessive use of global variables is generally discouraged.

```python
count = 0

def increase():
    global count
    count += 1

increase()

print(count)
```

Output:

```text
1
```

### Why avoid unnecessary global variables?

They can make programs:

* harder to understand
* harder to debug
* harder to test
* more prone to unexpected changes

Prefer passing values through parameters and returning results.

---

# 🧠 LEGB Rule

Python searches for variables using the **LEGB** rule:

```text
L → Local
E → Enclosing
G → Global
B → Built-in
```

Example:

```text
          Python Variable Search
                  │
                  ▼
             Local Scope
                  │
             not found?
                  ▼
           Enclosing Scope
                  │
             not found?
                  ▼
            Global Scope
                  │
             not found?
                  ▼
            Built-in Scope
```

Built-in examples include:

```python
print()
len()
type()
input()
```

---

# ♻️ 5. Reusable Code

One of the biggest advantages of functions is **code reuse**.

Imagine you need to check whether a username is valid in several places.

Without a function:

```python
username = "Farhan"

if len(username) >= 3:
    print("Valid")
```

Later, you might repeat the same logic:

```python
username = "Admin"

if len(username) >= 3:
    print("Valid")
```

With a function:

```python
def validate_username(username):
    return len(username) >= 3
```

Now:

```python
print(validate_username("Farhan"))
print(validate_username("Admin"))
print(validate_username("A"))
```

Output:

```text
True
True
False
```

The logic exists in **one place**.

---

# 🧩 Function Design Principle

A good function should generally have **one clear responsibility**.

Good:

```python
def validate_username(username):
    ...

def validate_password(password):
    ...

def calculate_score(score):
    ...
```

Instead of:

```python
def do_everything():
    # validate username
    # check password
    # scan network
    # save file
    # send email
    # print report
```

This principle makes programs easier to maintain.

---

# 🛡️ Functions in Cybersecurity

Functions are heavily used when creating cybersecurity tools and scripts.

For example:

```python
def analyze_ip(ip):
    # analyze IP address
    return result
```

```python
def check_port(port):
    # check a port
    return result
```

```python
def calculate_hash(data):
    # calculate hash
    return hash_value
```

```python
def parse_log(line):
    # process a log entry
    return event
```

A security program can then combine these functions:

```text
                 Security Tool
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
   parse_log()   analyze_ip()   check_port()
        │             │             │
        └─────────────┼─────────────┘
                      ▼
                  Final Report
```

This is much easier to manage than putting the entire program into one huge block.

---

# 🧱 Function Composition

Functions can also work together.

```python
def add(a, b):
    return a + b

def square(number):
    return number * number

result = square(add(2, 3))

print(result)
```

Execution:

```text
add(2, 3)
    ↓
    5
    ↓
square(5)
    ↓
   25
```

Output:

```text
25
```

This allows complex programs to be built from smaller pieces.

---

# 🔍 Function Example: Log Analysis

Imagine a security log:

```python
log = "FAILED LOGIN 192.168.1.10"
```

We can create a function:

```python
def detect_failed_login(log):
    if "FAILED LOGIN" in log:
        return True
    return False
```

Then:

```python
if detect_failed_login(log):
    print("Suspicious login detected")
```

Output:

```text
Suspicious login detected
```

The same function can now process thousands of log entries.

---

# 📊 Types of Functions

Python functions can be broadly divided into:

### 1. Built-in Functions

Already provided by Python.

```python
print()
len()
type()
input()
max()
min()
sum()
```

### 2. User-defined Functions

Created by the programmer.

```python
def greet():
    print("Hello")
```

### 3. Functions from Modules

Provided by Python libraries/modules.

```python
import math

print(math.sqrt(25))
```

---

# 🆚 Function vs Code Repetition

| Without Functions                   | With Functions      |
| ----------------------------------- | ------------------- |
| Code repeated                       | Code reused         |
| Harder to maintain                  | Easier to maintain  |
| More code                           | Less code           |
| Changes must be made multiple times | Change one function |
| Harder to test                      | Easier to test      |
| Less organized                      | More organized      |

---

# ⚠️ Common Mistakes

### 1. Forgetting to Call the Function

```python
def greet():
    print("Hello")
```

Nothing happens until:

```python
greet()
```

---

### 2. Forgetting Parameters

```python
def greet(name):
    print("Hello", name)

greet()
```

This causes an error because `name` is required.

Correct:

```python
greet("Farhan")
```

---

### 3. Confusing `print()` and `return`

Incorrect assumption:

```python
def add(a, b):
    print(a + b)

result = add(10, 20)

print(result)
```

Output:

```text
30
None
```

The function printed `30`, but it did not return it.

Correct:

```python
def add(a, b):
    return a + b

result = add(10, 20)

print(result)
```

---

### 4. Using Too Many Global Variables

Avoid building your entire program around global state.

Prefer:

```python
def calculate(a, b):
    return a + b
```

over unnecessary global variables.

---

### 5. Making Functions Too Large

Avoid:

```python
def huge_function():
    # hundreds of lines
```

Break the problem into smaller functions:

```python
def read_data():
    ...

def process_data():
    ...

def generate_report():
    ...
```

---

# 🧠 Memory Trick

Remember:

```text
CREATE → PASS → PROCESS → RETURN → REUSE
```

Or:

```text
DEF → PARAMETERS → CODE → RETURN → CALL
```

### Example

```python
def add(a, b):
    return a + b

result = add(10, 20)
```

```text
def       → CREATE
a, b      → PARAMETERS
a + b     → PROCESS
return    → SEND RESULT
add()     → CALL / REUSE
```

---

# 📝 Quick Revision

### Function

```python
def greet():
    print("Hello")
```

### Parameter

```python
def greet(name):
    print(name)
```

### Return

```python
def add(a, b):
    return a + b
```

### Call

```python
result = add(10, 20)
```

### Local Variable

```python
def test():
    x = 10
```

`x` normally exists only inside `test()`.

### Global Variable

```python
x = 10

def test():
    print(x)
```

### Default Parameter

```python
def greet(name="User"):
    print(name)
```

---

# 💡 Interview Tip

### Q: What is a function?

> A function is a reusable block of code designed to perform a specific task.

### Q: Why are functions useful?

> Functions improve code reusability, readability, maintainability, testing, and organization.

### Q: What is the difference between a parameter and an argument?

> A parameter is a variable defined in a function, while an argument is the actual value passed to that parameter.

### Q: What does `return` do?

> `return` sends a value back to the caller and terminates the function's execution.

### Q: What is variable scope?

> Scope defines where a variable can be accessed within a program.

---

# 🧪 Mini Practice

### Task 1 — Greeting Function

Create:

```python
def greet(name):
    ...
```

Expected:

```text
Hello Farhan
```

---

### Task 2 — Calculator

Create:

```python
def multiply(a, b):
    ...
```

Expected:

```python
multiply(5, 4)
```

Output:

```text
20
```

---

### Task 3 — Even Number

Create a function that returns `True` if a number is even.

Example:

```python
is_even(10)
```

Expected:

```text
True
```

---

### Task 4 — Password Length

Create:

```python
def check_password(password):
    ...
```

Return `True` when the password contains at least 8 characters.

---

### Task 5 — Security Log

Create:

```python
def detect_failed_login(log):
    ...
```

Return `True` when the log contains:

```text
FAILED LOGIN
```

Otherwise return `False`.

---

# 📚 Summary

Functions allow us to divide a program into **small, reusable pieces of code**.

The key concepts are:

```text
Function Creation
       ↓
Parameters
       ↓
Processing
       ↓
Return Value
       ↓
Scope
       ↓
Reusable Code
```

### Remember:

> **`def` creates a function.**

> **Parameters receive data.**

> **`return` sends data back.**

> **Local variables belong to their function.**

> **Functions reduce repetition and improve organization.**

> **Good functions usually perform one clear task.**

> **Functions are fundamental building blocks of Python programs and cybersecurity automation.**

If you want, we can continue directly with **5. File Handling** in the exact same Git-note format.
