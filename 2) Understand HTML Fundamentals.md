# 🌐 2. Understand HTML Fundamentals

> **“HTML defines what a web page contains and how its components are structured. For cybersecurity, understanding HTML helps you understand how browsers receive, display, and submit user-controlled data.”**

HTML is the foundation of almost every webpage.

It describes the **structure and content** of a webpage.

HTML is not a programming language. It is a **markup language**.

A simplified web stack is:

```text
HTML
 ↓
Structure

CSS
 ↓
Appearance

JavaScript
 ↓
Behavior
```

For cybersecurity, HTML is especially important because it defines:

* Forms
* Input fields
* Links
* Buttons
* Images
* Scripts
* Embedded content
* Metadata
* User-controlled input locations

---

# 1. 🧱 HTML Structure

HTML documents are made from **elements**.

A basic HTML element looks like:

```html
<p>Hello World</p>
```

It contains:

```text
Opening Tag
     ↓
   <p>
     ↓
Content
     ↓
Hello World
     ↓
Closing Tag
     ↓
  </p>
```

Together:

```html
<p>Hello World</p>
```

---

# 🔹 HTML Tags

Tags tell the browser what something represents.

Examples:

```html
<h1>Heading</h1>

<p>Paragraph</p>

<a href="https://example.com">Link</a>

<button>Click Me</button>

<img src="image.jpg" alt="Example">
```

Common tags include:

| Tag           | Purpose                  |
| ------------- | ------------------------ |
| `<html>`      | Root HTML element        |
| `<head>`      | Metadata and resources   |
| `<body>`      | Visible page content     |
| `<h1>`–`<h6>` | Headings                 |
| `<p>`         | Paragraph                |
| `<a>`         | Link                     |
| `<img>`       | Image                    |
| `<div>`       | Generic block container  |
| `<span>`      | Generic inline container |
| `<form>`      | Form                     |
| `<input>`     | User input               |
| `<button>`    | Button                   |
| `<label>`     | Input label              |
| `<textarea>`  | Multi-line input         |
| `<select>`    | Selection menu           |
| `<script>`    | JavaScript               |
| `<link>`      | External resource        |

---

# 🏷️ Attributes

HTML elements can contain **attributes**.

Example:

```html
<a href="https://example.com">
    Visit Example
</a>
```

Here:

```text
<a>
 ↓
Element

href
 ↓
Attribute

"https://example.com"
 ↓
Attribute value
```

Another example:

```html
<input type="text" name="username">
```

Attributes include:

```text
type
name
```

---

# 🧠 Why Attributes Matter in Cybersecurity

Attributes can affect how the browser behaves.

Examples:

```html
href=""
src=""
action=""
method=""
id=""
class=""
name=""
type=""
value=""
```

Some attributes are particularly important when analyzing web applications.

For example:

```html
<form action="/login" method="POST">
```

tells us where the form submits its data and which HTTP method it uses.

---

# 🌳 2. Document Structure

A normal HTML document has a hierarchical structure.

Example:

```html
<!DOCTYPE html>

<html>
<head>
    <title>My Website</title>
</head>

<body>

    <h1>Welcome</h1>

    <p>Hello World</p>

</body>
</html>
```

The structure is:

```text
HTML
│
├── HEAD
│   └── TITLE
│
└── BODY
    ├── H1
    └── P
```

This hierarchical structure becomes the **DOM (Document Object Model)** when the browser parses the document.

---

# 🌳 DOM — Document Object Model

The browser converts HTML into a tree-like structure.

For:

```html
<body>
    <h1>Hello</h1>
    <p>Welcome</p>
</body>
```

the DOM can be represented as:

```text
Document
   │
   └── html
       │
       └── body
           ├── h1
           │   └── "Hello"
           │
           └── p
               └── "Welcome"
```

JavaScript can interact with this structure.

For example:

```javascript
document.querySelector("h1");
```

can find the `<h1>` element.

---

# 🧠 Why the DOM Matters for Security

The DOM becomes particularly important when studying:

* JavaScript security
* DOM-based XSS
* Client-side injection
* Dynamic content
* Browser security mechanisms

You will encounter these concepts later.

---

# 🧩 3. The `<head>` Section

The `<head>` contains information and resources used by the browser.

Example:

```html
<head>

    <title>My Website</title>

    <meta charset="UTF-8">

    <meta name="viewport"
          content="width=device-width, initial-scale=1.0">

    <link rel="stylesheet" href="style.css">

</head>
```

Common elements include:

```text
<title>
<meta>
<link>
<style>
<script>
```

Not everything inside `<head>` is directly visible on the page.

---

# 🖥️ 4. The `<body>` Section

The `<body>` contains the page's main content.

Example:

```html
<body>

    <h1>Cybersecurity</h1>

    <p>Learn web security.</p>

    <button>Start</button>

</body>
```

The browser renders these elements for the user.

---

# 🧱 5. Common Web Page Components

A webpage can contain many different components.

```text
Web Page
│
├── Header
├── Navigation
├── Main Content
│   ├── Sections
│   ├── Articles
│   ├── Forms
│   └── Images
├── Sidebar
└── Footer
```

Semantic HTML can represent these areas.

Example:

```html
<header>
    <h1>Cybersecurity Blog</h1>
</header>

<nav>
    <a href="/">Home</a>
    <a href="/articles">Articles</a>
</nav>

<main>
    <article>
        <h2>Web Security</h2>
        <p>Understanding HTML.</p>
    </article>
</main>

<footer>
    Copyright 2026
</footer>
```

---

# 🎯 Semantic HTML

Semantic elements describe their purpose.

Examples:

```html
<header>
<nav>
<main>
<section>
<article>
<aside>
<footer>
```

Compare:

```html
<div>
    Navigation
</div>
```

with:

```html
<nav>
    Navigation
</nav>
```

The second gives the browser and assistive technologies more meaningful information.

---

# 📝 6. HTML Forms

Forms are extremely important for web security.

Forms allow users to submit information to a web application.

Example:

```html
<form>

    <label>Username</label>

    <input type="text">

    <label>Password</label>

    <input type="password">

    <button type="submit">
        Login
    </button>

</form>
```

The structure is:

```text
Form
│
├── Label
├── Input
├── Label
├── Input
└── Submit Button
```

---

# 🔗 Form `action`

A form can specify where the data should be sent.

```html
<form action="/login">
```

This means the form is associated with:

```text
/login
```

A more complete example:

```html
<form action="/login" method="POST">
```

Conceptually:

```text
User
 ↓
Fill Form
 ↓
Submit
 ↓
/login
 ↓
Backend
```

---

# 📡 Form `method`

The `method` specifies how the form data is submitted.

Common methods:

```text
GET
POST
```

---

## GET

Example:

```html
<form action="/search" method="GET">
```

A search might result in a URL such as:

```text
/search?q=cybersecurity
```

The data is part of the request URL.

---

## POST

Example:

```html
<form action="/login" method="POST">
```

The submitted form data is generally carried in the request body.

Conceptually:

```text
POST /login

username=farhan&password=...
```

Do not interpret POST as automatically secure.

Security depends on the complete application and transport.

---

# 🔐 GET vs POST

| Feature                           | GET           | POST                |
| --------------------------------- | ------------- | ------------------- |
| Common purpose                    | Retrieve data | Submit/process data |
| Data location                     | URL/query     | Request body        |
| URL may contain input             | Yes           | Usually no          |
| Suitable for passwords            | ❌             | More appropriate    |
| Automatically encrypted?          | ❌             | ❌                   |
| HTTPS required for sensitive data | ✅             | ✅                   |

Important:

> **POST does not encrypt data. HTTPS/TLS provides transport encryption.**

---

# 👤 7. User Inputs

HTML provides several input types.

Example:

```html
<input type="text">
<input type="password">
<input type="email">
<input type="number">
<input type="date">
<input type="file">
<input type="checkbox">
<input type="radio">
```

Each provides a different interface.

---

# 📝 Text Input

```html
<input type="text" name="username">
```

The user can enter text.

Example:

```text
username = farhan
```

---

# 🔑 Password Input

```html
<input type="password" name="password">
```

The browser visually hides the entered characters.

But remember:

> `<input type="password">` does **not encrypt the password**.

It mainly controls how the browser displays the input.

Transport protection comes from mechanisms such as HTTPS/TLS.

---

# 📧 Email Input

```html
<input type="email" name="email">
```

Browsers can perform basic client-side validation.

For example, they may expect something resembling:

```text
user@example.com
```

But this is not a security boundary.

The backend must validate the data too.

---

# 🔢 Number Input

```html
<input type="number" name="age">
```

A browser may restrict the UI to numeric input.

But a malicious client can send a manually crafted HTTP request.

Therefore:

```text
Browser Validation
       ↓
Helpful UX
```

while:

```text
Server Validation
       ↓
Security Boundary
```

---

# 📁 File Input

```html
<input type="file" name="document">
```

This allows a user to select a file.

File uploads are extremely important in web security.

Potential security concerns include:

* File type validation
* File size
* Filename handling
* Storage location
* Executable files
* Malicious content
* Path traversal
* Server-side processing

You will study file-upload vulnerabilities later.

---

# ☑️ Checkbox

```html
<input type="checkbox" name="terms">
```

Useful for:

* Terms acceptance
* Preferences
* Options

But never assume:

```text
checkbox checked
=
user trusted
```

The backend must enforce important conditions.

---

# 🔘 Radio Buttons

```html
<input type="radio"
       name="role"
       value="user">

<input type="radio"
       name="role"
       value="admin">
```

This is a very important cybersecurity example.

Suppose the frontend provides:

```text
○ User
○ Admin
```

An attacker might manually send:

```text
role=admin
```

The backend must **not** trust the client's selected role.

Authorization must be enforced server-side.

---

# 🛡️ 8. Client-Side Validation

HTML provides attributes such as:

```html
<input
    type="text"
    required
    minlength="3"
>
```

The browser may prevent submission when the requirements are not met.

This is useful for user experience.

But:

> **Client-side validation is not sufficient security validation.**

Why?

Because the user controls the client.

They can:

* Disable JavaScript
* Modify HTML
* Modify requests
* Use browser developer tools
* Use an intercepting proxy
* Send requests directly

Therefore:

```text
Client Validation
       +
Server Validation
       ↓
Better Security
```

---

# 🔥 9. Hidden Inputs

HTML supports hidden inputs:

```html
<input type="hidden"
       name="user_id"
       value="123">
```

The field isn't normally visible in the page.

But:

> **Hidden does not mean secret.**

A user can inspect the HTML and modify it.

For example:

```html
value="123"
```

could potentially be changed to:

```html
value="999"
```

before submitting the request.

Therefore sensitive authorization decisions must never depend solely on hidden fields.

---

# 🔐 Important Cybersecurity Rule

## Never Trust HTML

Everything delivered to the user's browser should be considered potentially controllable by the user.

This includes:

```text
HTML
CSS
JavaScript
Hidden fields
Form values
Client-side validation
Cookies
Request parameters
Headers
```

The backend must independently enforce security controls.

---

# 🧩 10. Labels

Labels improve usability and accessibility.

Example:

```html
<label for="username">
    Username
</label>

<input
    id="username"
    name="username"
    type="text"
>
```

The relationship is:

```text
label for="username"
        ↓
id="username"
```

---

# 🔘 11. Buttons

Example:

```html
<button type="submit">
    Login
</button>
```

Common button types:

```text
submit
button
reset
```

For example:

```html
<button type="button">
    Click Me
</button>
```

A button doesn't automatically perform a security-sensitive action safely.

The server must still validate the request.

---

# 📦 12. `<div>` and `<span>`

These are generic containers.

### `<div>`

Usually used as a block-level container.

```html
<div>
    <h2>Security</h2>
    <p>Learn web security.</p>
</div>
```

### `<span>`

Usually used for smaller inline content.

```html
<p>
    This is <span>important</span>.
</p>
```

They are heavily used in modern frontend frameworks.

---

# 🔗 13. Links

Links use the `<a>` element.

```html
<a href="/login">
    Login
</a>
```

External:

```html
<a href="https://example.com">
    Example
</a>
```

The `href` attribute determines the destination.

---

# ⚠️ Links and Security

When analyzing web applications, links can reveal:

* Endpoints
* Paths
* Parameters
* Application structure
* External services

For example:

```html
<a href="/user/profile?id=123">
```

reveals:

```text
Path:
 /user/profile

Parameter:
 id=123
```

This kind of information can be useful during authorized web security testing.

---

# 🖼️ 14. Images

Example:

```html
<img
    src="logo.png"
    alt="Company Logo"
>
```

Important attributes:

```text
src
alt
width
height
```

The `src` tells the browser where to retrieve the image.

---

# 📜 15. Scripts

JavaScript can be embedded or loaded externally.

### Inline

```html
<script>
    console.log("Hello");
</script>
```

### External

```html
<script src="app.js"></script>
```

JavaScript can:

* Modify the DOM
* Send HTTP requests
* Handle user interaction
* Update content
* Communicate with APIs

This makes JavaScript a major part of modern web applications.

It also introduces security considerations such as:

* XSS
* DOM manipulation
* Unsafe dynamic content
* Third-party scripts

---

# 🔍 16. HTML Comments

HTML supports comments:

```html
<!-- This is a comment -->
```

Comments aren't normally displayed on the webpage.

But they are still delivered to the client.

Therefore developers should not place secrets inside comments.

❌ Bad:

```html
<!-- Database password: MySecretPassword -->
```

Even though users don't see it normally, they can inspect the page source.

---

# 🕵️ 17. View Source vs Developer Tools

You can inspect HTML in two common ways.

### View Page Source

Shows the HTML source delivered by the server.

### Developer Tools

Allows you to inspect the current DOM, requests, responses, storage, and other browser information.

Open Developer Tools:

```text
F12
```

Useful tabs include:

```text
Elements
Network
Console
Application
Security
```

You'll use these heavily in web security.

---

# 🧪 Hands-on Practice

Now let's create your own HTML page.

Create:

```text
index.html
```

Use:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Cybersecurity Lab</title>
</head>

<body>

    <header>
        <h1>Cybersecurity Lab</h1>
    </header>

    <main>

        <h2>Login</h2>

        <form action="/login" method="POST">

            <label for="username">
                Username:
            </label>

            <input
                type="text"
                id="username"
                name="username"
                required
            >

            <br><br>

            <label for="password">
                Password:
            </label>

            <input
                type="password"
                id="password"
                name="password"
                required
            >

            <br><br>

            <button type="submit">
                Login
            </button>

        </form>

    </main>

    <footer>
        <p>Cybersecurity Practice Lab</p>
    </footer>

</body>

</html>
```

Open it in your browser.

---

# 🔎 Analyze Your Own Page

Open:

```text
F12
```

Go to:

```text
Elements
```

Find:

```html
<form>
```

Then identify:

```text
action
method
input names
input types
button type
```

Now inspect the page source.

Ask yourself:

> Which information is visible to the client?

You should realize that essentially the entire HTML structure is available to the browser.

---

# 🧪 Security Experiment — Hidden Input

Add:

```html
<input
    type="hidden"
    name="role"
    value="user"
>
```

Inspect the page using Developer Tools.

You'll see that the "hidden" value is still accessible.

Change:

```text
user
```

to:

```text
admin
```

This demonstrates an important principle:

> **Hidden HTML fields are not a security mechanism.**

Do this only on your own local practice page.

---

# 🧪 Security Experiment — Client Validation

Add:

```html
<input
    type="text"
    name="username"
    minlength="5"
    required
>
```

Try submitting an empty or short value.

The browser may block the form.

Now remember:

```text
Browser validation
        ≠
Security validation
```

A real backend must perform its own validation.

---

# 🧠 HTML Security Mind Map

```text
                         HTML
                          │
        ┌─────────────────┼─────────────────┐
        ↓                 ↓                 ↓
     Structure           Forms           Components
        │                 │                 │
        ↓                 ↓                 ↓
      DOM             User Input        Links/Images
        │                 │                 │
        ↓                 ↓                 ↓
   JavaScript        Validation          Scripts
        │                 │                 │
        └─────────────────┼─────────────────┘
                          ↓
                   Web Security
                          │
              ┌───────────┼───────────┐
              ↓           ↓           ↓
             XSS      Input Attacks  File Upload
```

---

# 🔥 Cybersecurity Takeaways

### 1. HTML is client-side

The user can inspect and modify it.

### 2. Forms are important

They are a major way users send data to applications.

### 3. Input is untrusted

Never assume browser-generated input is safe.

### 4. Hidden doesn't mean secret

Hidden fields can be inspected and modified.

### 5. Client-side validation isn't enough

Security validation must happen on the server.

### 6. HTML can reveal application structure

Forms, links, endpoints, parameters, and resources can provide useful information during authorized security analysis.

### 7. Never put secrets in HTML

Anything delivered to the browser should be considered potentially visible to the user.

---

# ⚡ Quick Revision

| Concept           | Meaning                                             |
| ----------------- | --------------------------------------------------- |
| HTML              | Markup language for webpage structure               |
| Element           | HTML building block                                 |
| Tag               | Defines an element                                  |
| Attribute         | Provides additional information/configuration       |
| DOM               | Browser's structured representation of the document |
| Form              | Collects/submits user input                         |
| Input             | Allows users to provide data                        |
| `action`          | Form submission destination                         |
| `method`          | HTTP method used by the form                        |
| Frontend          | Client-side interface and behavior                  |
| Backend           | Server-side application logic                       |
| Hidden input      | Invisible UI field, **not a secret**                |
| Client validation | Browser-side validation                             |
| Server validation | Backend-side validation                             |

---

# 🧠 Memory Trick

Remember:

## **S → F → I → D → C**

```text
S = Structure
F = Forms
I = Inputs
D = Document / DOM
C = Components
```

And for cybersecurity:

## 🔥 **CLIENT = UNTRUSTED**

```text
HTML
JavaScript
Forms
Hidden Fields
Cookies
Requests
        ↓
Potentially controlled by the user
        ↓
Backend must validate and authorize
```

---

# 🏁 Final Takeaway

HTML is the foundation of the web interface.

You should now understand:

```text
HTML
 │
 ├── Document Structure
 │
 ├── Elements
 │
 ├── Attributes
 │
 ├── Forms
 │
 ├── User Inputs
 │
 ├── Links
 │
 ├── Images
 │
 ├── Scripts
 │
 └── DOM
```

From a cybersecurity perspective, the most important lesson is:

> **Anything sent to the browser can potentially be inspected or modified by the user.**

Therefore:

```text
Browser
   ↓
UNTRUSTED INPUT
   ↓
Backend
   ↓
Validate
   ↓
Authorize
   ↓
Process
```

Once you understand HTML forms and user input, you'll have the foundation needed to understand **HTTP requests, Burp Suite, authentication, sessions, XSS, injection, and other web security concepts**.
