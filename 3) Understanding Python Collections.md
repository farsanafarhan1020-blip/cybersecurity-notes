# 🐍 Understanding Python Collections

> **"Python collections are data structures used to store and organize multiple values. They allow programs to efficiently manage, search, modify, and process groups of related data."**

---

# 📖 What are Python Collections?

A **collection** is a data structure that allows multiple values to be stored together.

Instead of creating separate variables:

```python
ip1 = "192.168.1.10"
ip2 = "192.168.1.11"
ip3 = "192.168.1.12"
```

We can store them in one collection:

```python
ips = ["192.168.1.10", "192.168.1.11", "192.168.1.12"]
```

This makes data easier to organize and process.

Python provides four fundamental collection types:

* 📋 **List**
* 📦 **Tuple**
* 🧩 **Set**
* 🗂️ **Dictionary**

---

## 🎯 Why are Collections Important?

Collections are useful when working with:

* 🌐 IP addresses
* 🔌 Network ports
* 👤 User accounts
* 📄 Log entries
* 🔐 Security events
* 🧪 Malware indicators
* 📊 Large datasets
* 🔎 OSINT information

---

## 🌍 Real-World Example

A security analyst may collect suspicious IP addresses:

```python
suspicious_ips = [
    "192.168.1.10",
    "10.0.0.25",
    "172.16.0.50"
]
```

Instead of storing every IP in a separate variable, one collection can contain all of them.

```text
Suspicious IPs
      │
      ▼
┌─────────────────────────┐
│ 192.168.1.10            │
│ 10.0.0.25               │
│ 172.16.0.50             │
└─────────────────────────┘
```

---

# 📋 Lists

## 📖 What is a List?

A **list** is an ordered collection that can store multiple values.

Lists are:

* Ordered
* Mutable
* Indexed
* Able to contain duplicate values
* Able to contain different data types

Lists are created using square brackets:

```python
ports = [22, 80, 443]
```

---

## ⚙️ How Lists Work

Each element has an **index**.

```text
Index:     0      1       2
           │      │       │
           ▼      ▼       ▼
ports = [ 22,    80,     443 ]
```

Python uses **zero-based indexing**, meaning the first element has index `0`.

---

## 🔎 Accessing List Elements

```python
ports = [22, 80, 443]

print(ports[0])
```

Output:

```text
22
```

Access the second element:

```python
print(ports[1])
```

Output:

```text
80
```

---

## 🔙 Negative Indexing

Python also supports negative indexes.

```text
Index:    -3     -2     -1
           │      │      │
           ▼      ▼      ▼
ports = [ 22,    80,    443 ]
```

Example:

```python
print(ports[-1])
```

Output:

```text
443
```

---

# ✏️ Modifying Lists

Because lists are **mutable**, their contents can be changed.

```python
ports = [22, 80, 443]

ports[1] = 8080

print(ports)
```

Output:

```text
[22, 8080, 443]
```

---

# ➕ Adding Elements

## `append()`

Adds an element to the end.

```python
ports = [22, 80]

ports.append(443)

print(ports)
```

Output:

```text
[22, 80, 443]
```

---

## `insert()`

Adds an element at a specific position.

```python
ports = [22, 443]

ports.insert(1, 80)

print(ports)
```

Output:

```text
[22, 80, 443]
```

---

## `extend()`

Adds multiple elements.

```python
ports = [22, 80]

ports.extend([443, 8080])

print(ports)
```

Output:

```text
[22, 80, 443, 8080]
```

---

# ❌ Removing Elements

## `remove()`

Removes a specific value.

```python
ports = [22, 80, 443]

ports.remove(80)

print(ports)
```

Output:

```text
[22, 443]
```

---

## `pop()`

Removes an element using its index.

```python
ports = [22, 80, 443]

ports.pop(1)

print(ports)
```

Output:

```text
[22, 443]
```

If no index is provided, `pop()` removes the last element.

```python
ports.pop()
```

---

## `clear()`

Removes all elements.

```python
ports = [22, 80, 443]

ports.clear()

print(ports)
```

Output:

```text
[]
```

---

# 🔍 Searching Lists

Use `in` to check whether an item exists.

```python
ports = [22, 80, 443]

if 443 in ports:
    print("HTTPS port found")
```

Output:

```text
HTTPS port found
```

---

# 📏 List Length

Use `len()` to determine the number of elements.

```python
ports = [22, 80, 443]

print(len(ports))
```

Output:

```text
3
```

---

# 🔄 Looping Through Lists

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

# ✂️ List Slicing

Slicing allows you to extract part of a list.

```python
ports = [22, 80, 443, 8080, 8443]

print(ports[1:4])
```

Output:

```text
[80, 443, 8080]
```

The ending index is not included.

---

# 🧪 List Comprehension

A **list comprehension** provides a compact way to create lists.

```python
numbers = [1, 2, 3, 4, 5]

squares = [number ** 2 for number in numbers]

print(squares)
```

Output:

```text
[1, 4, 9, 16, 25]
```

A cybersecurity example:

```python
ports = [22, 80, 443, 8080]

web_ports = [port for port in ports if port in [80, 443, 8080]]

print(web_ports)
```

Output:

```text
[80, 443, 8080]
```

---

# 📦 Tuples

## 📖 What is a Tuple?

A **tuple** is an ordered collection that is **immutable**.

Tuples are created using parentheses:

```python
coordinates = (10, 20)
```

A tuple can contain multiple values:

```python
ports = (22, 80, 443)
```

---

## ⚙️ Tuple Characteristics

Tuples are:

* Ordered
* Indexed
* Immutable
* Able to contain duplicates
* Able to contain different data types

---

## 🔎 Accessing Tuple Elements

```python
ports = (22, 80, 443)

print(ports[0])
```

Output:

```text
22
```

---

# 🔒 Tuple Immutability

You cannot normally change an existing tuple element.

```python
ports = (22, 80, 443)

ports[0] = 21
```

This produces:

```text
TypeError
```

because tuples are immutable.

---

# 🎯 Why Use Tuples?

Tuples are useful for data that should remain fixed.

Example:

```python
server = ("192.168.1.10", 443)
```

The server's IP and port can be represented as a fixed pair.

---

# 📏 Tuple Methods

Tuples provide fewer methods because they cannot be modified.

### `count()`

Counts how many times a value appears.

```python
ports = (80, 443, 80)

print(ports.count(80))
```

Output:

```text
2
```

### `index()`

Returns the position of a value.

```python
ports = (22, 80, 443)

print(ports.index(443))
```

Output:

```text
2
```

---

# 🧩 Sets

## 📖 What is a Set?

A **set** is an unordered collection of **unique values**.

Sets are created using curly braces:

```python
ports = {22, 80, 443}
```

---

## ⚙️ Set Characteristics

Sets are:

* Unordered
* Mutable
* Unique
* Not index-based

This means you cannot access a set using:

```python
ports[0]
```

because sets do not maintain indexes like lists.

---

# 🧹 Removing Duplicates

One of the most useful features of sets is automatically removing duplicates.

```python
ips = {
    "192.168.1.10",
    "192.168.1.11",
    "192.168.1.10"
}

print(ips)
```

The duplicate value is removed.

Conceptually:

```text
Input
│
├── 192.168.1.10
├── 192.168.1.11
└── 192.168.1.10
          │
          ▼
       Set
          │
          ▼
├── 192.168.1.10
└── 192.168.1.11
```

---

# ➕ Adding to a Set

Use `add()`.

```python
ports = {22, 80}

ports.add(443)

print(ports)
```

---

# ❌ Removing from a Set

### `remove()`

```python
ports = {22, 80, 443}

ports.remove(80)
```

If the value does not exist, `remove()` raises an error.

### `discard()`

```python
ports.discard(80)
```

`discard()` does not raise an error if the value is missing.

---

# 🔀 Set Operations

Sets are especially useful for comparing groups of data.

---

## Union

Combines values from both sets.

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)
```

Result:

```text
{1, 2, 3, 4, 5}
```

---

## Intersection

Returns values that exist in both sets.

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a & b)
```

Result:

```text
{3}
```

---

## Difference

Returns values present in the first set but not the second.

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a - b)
```

Result:

```text
{1, 2}
```

---

## 🔐 Cybersecurity Example

Suppose you have two lists of IP addresses:

```python
known_bad = {"10.0.0.1", "10.0.0.2", "10.0.0.3"}

observed = {"10.0.0.2", "10.0.0.4"}
```

Find IPs that appear in both:

```python
suspicious = known_bad & observed

print(suspicious)
```

Result:

```text
{"10.0.0.2"}
```

This basic concept can be useful when comparing threat-intelligence data with observed network activity.

---

# 🗂️ Dictionaries

## 📖 What is a Dictionary?

A **dictionary** stores data as **key-value pairs**.

Example:

```python
user = {
    "username": "admin",
    "role": "administrator",
    "active": True
}
```

Think of it like:

```text
Key              Value
────────────────────────────
username   →     admin
role       →     administrator
active     →     True
```

---

## ⚙️ How Dictionaries Work

A dictionary allows you to retrieve a value using its key.

```python
user = {
    "username": "admin",
    "role": "administrator"
}

print(user["username"])
```

Output:

```text
admin
```

---

# ➕ Adding Dictionary Data

```python
user = {
    "username": "admin"
}

user["role"] = "administrator"

print(user)
```

---

# ✏️ Updating Dictionary Data

```python
user = {
    "username": "admin",
    "role": "user"
}

user["role"] = "administrator"

print(user)
```

---

# ❌ Removing Dictionary Data

### `pop()`

```python
user = {
    "username": "admin",
    "role": "user"
}

user.pop("role")
```

---

### `del`

```python
del user["username"]
```

---

### `clear()`

```python
user.clear()
```

This removes all key-value pairs.

---

# 🔎 Checking Dictionary Keys

```python
user = {
    "username": "admin",
    "role": "administrator"
}

if "username" in user:
    print("Username exists")
```

---

# 🔑 Dictionary Methods

### `keys()`

Returns dictionary keys.

```python
print(user.keys())
```

### `values()`

Returns values.

```python
print(user.values())
```

### `items()`

Returns key-value pairs.

```python
print(user.items())
```

---

# 🔄 Looping Through Dictionaries

```python
user = {
    "username": "admin",
    "role": "administrator",
    "active": True
}

for key, value in user.items():
    print(key, value)
```

Output:

```text
username admin
role administrator
active True
```

---

# 🔐 Cybersecurity Example

Dictionaries are extremely useful for representing security events.

```python
event = {
    "source_ip": "192.168.1.50",
    "destination_port": 443,
    "username": "admin",
    "status": "failed"
}
```

Access information:

```python
print(event["source_ip"])
print(event["status"])
```

Output:

```text
192.168.1.50
failed
```

A collection of events could then be stored in a list:

```python
events = [
    {
        "source_ip": "192.168.1.50",
        "status": "failed"
    },
    {
        "source_ip": "192.168.1.60",
        "status": "success"
    }
]
```

This structure is common when processing structured data.

---

# 🧩 Collection Manipulation

## 📖 What is Collection Manipulation?

**Collection manipulation** means adding, removing, searching, sorting, filtering, and transforming data stored inside collections.

```text
Collection
    │
    ├── Add
    ├── Remove
    ├── Search
    ├── Sort
    ├── Filter
    └── Transform
```

---

# 🔍 Searching Collections

### List

```python
ports = [22, 80, 443]

if 443 in ports:
    print("Port found")
```

### Set

```python
ports = {22, 80, 443}

if 443 in ports:
    print("Port found")
```

### Dictionary

```python
user = {
    "username": "admin"
}

if "username" in user:
    print("Key found")
```

---

# 🔢 Sorting Lists

The `sort()` method modifies the existing list.

```python
ports = [443, 22, 80]

ports.sort()

print(ports)
```

Output:

```text
[22, 80, 443]
```

Reverse order:

```python
ports.sort(reverse=True)
```

---

## `sorted()`

`sorted()` creates a new sorted result without modifying the original collection.

```python
ports = [443, 22, 80]

sorted_ports = sorted(ports)

print(sorted_ports)
```

---

# 🔄 Reversing a List

```python
ports = [22, 80, 443]

ports.reverse()

print(ports)
```

Output:

```text
[443, 80, 22]
```

---

# 🧹 Removing Duplicates

A common technique is converting a list into a set.

```python
ips = [
    "10.0.0.1",
    "10.0.0.2",
    "10.0.0.1"
]

unique_ips = list(set(ips))

print(unique_ips)
```

The duplicate IP is removed.

> **Note:** Converting a list to a set does not preserve the original list's ordering.

---

# 🔎 Filtering Data

Suppose we have ports:

```python
ports = [21, 22, 25, 53, 80, 443]
```

We can select web ports:

```python
web_ports = [port for port in ports if port in [80, 443]]

print(web_ports)
```

Output:

```text
[80, 443]
```

---

# 🔄 Transforming Data

Suppose we have usernames:

```python
users = ["admin", "guest", "root"]
```

Convert them to uppercase:

```python
upper_users = [user.upper() for user in users]

print(upper_users)
```

Output:

```text
["ADMIN", "GUEST", "ROOT"]
```

---

# 🔗 Combining Collections

The `zip()` function combines corresponding elements.

```python
users = ["admin", "guest"]
roles = ["administrator", "user"]

for user, role in zip(users, roles):
    print(user, role)
```

Output:

```text
admin administrator
guest user
```

---

# 🧠 Choosing the Right Collection

```text
Need ordered data?
       │
       ▼
     List
       │
       ├── Need to modify? → List
       │
       └── Need fixed data? → Tuple

Need unique values?
       │
       ▼
      Set

Need key-value relationships?
       │
       ▼
   Dictionary
```

---

# ⚖️ List vs Tuple vs Set vs Dictionary

| Feature      | List               | Tuple            | Set           | Dictionary     |
| ------------ | ------------------ | ---------------- | ------------- | -------------- |
| Ordered      | ✅                  | ✅                | ❌             | ✅*             |
| Mutable      | ✅                  | ❌                | ✅             | ✅              |
| Duplicates   | ✅                  | ✅                | ❌             | Keys ❌         |
| Indexed      | ✅                  | ✅                | ❌             | By key         |
| Syntax       | `[]`               | `()`             | `{}`          | `{key:value}`  |
| Main purpose | General collection | Fixed collection | Unique values | Key-value data |

> `*` Python dictionaries preserve insertion order in modern Python versions.

---

# ⚖️ When Should You Use Each?

### 📋 Use a List When:

You need an ordered collection that can change.

```python
ports = [22, 80, 443]
```

---

### 📦 Use a Tuple When:

You have data that should remain fixed.

```python
server = ("192.168.1.10", 443)
```

---

### 🧩 Use a Set When:

You need unique values or set operations.

```python
unique_ips = {"10.0.0.1", "10.0.0.2"}
```

---

### 🗂️ Use a Dictionary When:

You need relationships between keys and values.

```python
user = {
    "username": "admin",
    "role": "administrator"
}
```

---

# 🌍 Real-World Cybersecurity Example

A security monitoring system might organize information like this:

```python
users = ["admin", "guest", "root"]

ports = {22, 80, 443}

server = ("192.168.1.10", 443)

event = {
    "source_ip": "10.0.0.5",
    "username": "admin",
    "status": "failed"
}
```

Each collection has a different purpose.

```text
Security System
      │
      ├── Users
      │     └── List
      │
      ├── Unique Ports
      │     └── Set
      │
      ├── Fixed Server Information
      │     └── Tuple
      │
      └── Security Event
            └── Dictionary
```

---

# 🧠 Memory Trick

Think of Python collections like different containers in a security lab.

* 📋 **List** = A numbered checklist that can be changed.
* 📦 **Tuple** = A sealed package containing fixed information.
* 🧩 **Set** = A box that automatically keeps only unique items.
* 🗂️ **Dictionary** = A labeled storage cabinet where every key points to a value.

Remember:

```text
List       → Ordered & Changeable
Tuple      → Ordered & Fixed
Set        → Unique Values
Dictionary → Key → Value
```

---

# 📝 Quick Revision

### 📋 Lists

* Ordered
* Mutable
* Indexed
* Allow duplicates
* Use `[]`

### 📦 Tuples

* Ordered
* Immutable
* Indexed
* Allow duplicates
* Use `()`

### 🧩 Sets

* Unique values
* No normal indexing
* Mutable
* Useful for set operations
* Use `{}`

### 🗂️ Dictionaries

* Key-value pairs
* Mutable
* Keys must be unique
* Access data using keys
* Use `{key: value}`

### 🔧 Collection Manipulation

Common operations:

* Add
* Remove
* Search
* Sort
* Filter
* Transform
* Iterate
* Combine

---

# 💡 Interview Tip

### ❓What is a list?

**Answer:** A list is an ordered and mutable collection that can store multiple values and allows duplicate elements.

### ❓What is the difference between a list and tuple?

**Answer:** Both are ordered and indexed collections, but lists are mutable while tuples are immutable.

### ❓Why are sets useful?

**Answer:** Sets store unique values and provide efficient operations such as union, intersection, and difference.

### ❓What is a dictionary?

**Answer:** A dictionary is a mutable collection that stores data as key-value pairs.

### ❓Can a set contain duplicate values?

**Answer:** No. Sets automatically eliminate duplicate values.

### ❓Can dictionary keys be duplicated?

**Answer:** No. Dictionary keys must be unique. If the same key is assigned again, its previous value is replaced.

### ❓What is list slicing?

**Answer:** List slicing is a way to extract a portion of a list using a range of indexes.

Example:

```python
ports[1:4]
```

### ❓What is list comprehension?

**Answer:** List comprehension is a concise way to create a new list by applying an expression to elements of an iterable, optionally using a condition.

---

# 🧪 Mini Practice

### 1. Create a list containing:

```text
22, 80, 443, 8080
```

Then:

* Add `8443`
* Remove `80`
* Print the length
* Sort the list
* Check whether `443` exists

---

### 2. Create a tuple containing:

```text
IP address
Port
Protocol
```

Print each value.

---

### 3. Create a set containing repeated IP addresses.

Remove the duplicates automatically.

---

### 4. Create a dictionary representing a user:

```text
username
role
active
login_attempts
```

Print each value.

---

### 5. Create two sets:

```text
Known malicious IPs
Observed IPs
```

Use **intersection** to find IPs appearing in both.

---

### 6. Create a list of login attempts:

```python
attempts = [1, 8, 3, 10, 2, 7]
```

Use a list comprehension to find attempts greater than `5`.

Expected result:

```text
[8, 10, 7]
```

---

### 7. Create a dictionary of users and login attempts.

Use a loop to identify users with more than five failed attempts.

---

# 📚 Summary

Python collections provide the foundation for storing and processing groups of data.

```text
                Python Collections
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
      List           Tuple           Set
        │              │              │
   Changeable        Fixed          Unique
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                  Dictionary
                       │
                  Key → Value
```

Collections are extremely important in cybersecurity because security tools constantly process **IP addresses, ports, users, logs, indicators, alerts, and other structured information**.

> **Remember:**
>
> 📋 **List stores ordered, changeable data.**
>
> 📦 **Tuple stores ordered, fixed data.**
>
> 🧩 **Set stores unique values.**
>
> 🗂️ **Dictionary stores key-value relationships.**
>
> 🔍 **Collection manipulation allows you to search, modify, sort, filter, and transform data.**
>
> 🔐 **Mastering collections is essential for processing cybersecurity data with Python.**
