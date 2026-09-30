# 🎨 CSS Fundamentals — Web Security Roadmap

> **CSS (Cascading Style Sheets)** is the language used to control the appearance, layout, and presentation of HTML documents.

HTML gives a webpage its **structure**.
CSS controls how that structure **looks and behaves visually**.

```text
HTML
  │
  │ Structure
  ▼
┌─────────────────────┐
│   Web Page Content  │
└─────────────────────┘
          │
          │ CSS
          ▼
┌─────────────────────┐
│ Colors              │
│ Fonts               │
│ Spacing             │
│ Layout              │
│ Responsive Design   │
│ UI Components       │
└─────────────────────┘
```

---

# 1. 🎨 Styling Concepts

CSS allows developers to style HTML elements.

A basic CSS rule looks like this:

```css
selector {
    property: value;
}
```

Example:

```css
p {
    color: blue;
    font-size: 18px;
}
```

Here:

| Part        | Meaning  |
| ----------- | -------- |
| `p`         | Selector |
| `color`     | Property |
| `blue`      | Value    |
| `font-size` | Property |
| `18px`      | Value    |

---

## 🔹 Three Ways to Add CSS

### 1. Inline CSS

CSS is written directly inside an HTML element.

```html
<p style="color: red;">Hello</p>
```

Useful for small experiments, but generally not preferred for larger applications.

---

### 2. Internal CSS

CSS is placed inside `<style>`.

```html
<head>
    <style>
        p {
            color: blue;
        }
    </style>
</head>
```

---

### 3. External CSS

CSS is stored in a separate file.

**HTML:**

```html
<link rel="stylesheet" href="style.css">
```

**style.css:**

```css
body {
    background-color: black;
    color: white;
}
```

External CSS is commonly preferred because it separates:

```text
HTML → Structure
CSS  → Presentation
JavaScript → Behavior
```

---

# 🎯 Common CSS Properties

## Colors

```css
color: white;
background-color: black;
```

Colors can be represented using:

```css
red
#00ff00
rgb(0, 255, 0)
rgba(0, 255, 0, 0.5)
hsl(120, 100%, 50%)
```

---

## Fonts

```css
font-family: Arial;
font-size: 20px;
font-weight: bold;
font-style: italic;
```

---

## Text

```css
text-align: center;
text-decoration: none;
line-height: 1.5;
letter-spacing: 1px;
```

---

# 📦 The CSS Box Model

One of the most important CSS concepts is the **box model**.

Every normal HTML element can be understood as a box:

```text
┌──────────────────────────────┐
│           Margin             │
│   ┌──────────────────────┐   │
│   │       Border         │   │
│   │  ┌────────────────┐  │   │
│   │  │    Padding     │  │   │
│   │  │ ┌────────────┐ │  │   │
│   │  │ │  Content   │ │  │   │
│   │  │ └────────────┘ │  │   │
│   │  └────────────────┘  │   │
│   └──────────────────────┘   │
└──────────────────────────────┘
```

### Content

The actual text/image/etc.

```css
width: 300px;
height: 200px;
```

### Padding

Space **inside** the element.

```css
padding: 20px;
```

### Border

The boundary around the element.

```css
border: 1px solid black;
```

### Margin

Space **outside** the element.

```css
margin: 20px;
```

---

## 🧠 Box Model Example

```css
.card {
    width: 300px;
    padding: 20px;
    border: 2px solid black;
    margin: 30px;
}
```

Think:

```text
Margin
  ↓
Border
  ↓
Padding
  ↓
Content
```

---

# 🎯 CSS Selectors

Selectors tell CSS **which HTML elements should be styled**.

### Element selector

```css
p {
    color: blue;
}
```

Targets all `<p>` elements.

---

### Class selector

HTML:

```html
<p class="warning">Warning!</p>
```

CSS:

```css
.warning {
    color: red;
}
```

---

### ID selector

HTML:

```html
<div id="login-box"></div>
```

CSS:

```css
#login-box {
    width: 300px;
}
```

---

### Attribute selector

```css
input[type="password"] {
    border: 1px solid red;
}
```

This targets password inputs.

---

# 🔥 CSS Cascade

The **Cascading** part of CSS determines which styles are applied when multiple rules affect the same element.

Example:

```css
p {
    color: blue;
}

p {
    color: red;
}
```

The later rule can override the earlier one when specificity and other cascade rules allow it.

CSS decisions can involve:

```text
Origin
   ↓
Importance
   ↓
Specificity
   ↓
Source Order
```

---

# 🎯 Specificity

Consider:

```css
p {
    color: blue;
}

.warning {
    color: orange;
}

#important {
    color: red;
}
```

HTML:

```html
<p id="important" class="warning">
    Warning
</p>
```

The ID selector has greater specificity than the class and element selector.

Conceptually:

```text
Element      → Low
Class        → Higher
ID           → Higher
!important   → Special priority
```

Avoid using `!important` unnecessarily because it can make CSS difficult to maintain.

---

# 2. 📐 Layout Systems

CSS layout determines **where elements appear on the page**.

The major layout concepts are:

```text
Normal Flow
   │
   ├── Block / Inline
   │
   ├── Flexbox
   │
   ├── CSS Grid
   │
   └── Positioning
```

---

# 🔹 Block vs Inline

### Block elements

Normally take available width and begin on a new line.

Examples:

```html
<div>
<p>
<section>
<h1>
```

Conceptually:

```text
┌─────────────────────┐
│ Block Element       │
└─────────────────────┘
┌─────────────────────┐
│ Another Block       │
└─────────────────────┘
```

---

### Inline elements

Normally occupy only the space they need.

Examples:

```html
<span>
<a>
<strong>
```

Conceptually:

```text
Text [link] more text
```

---

# 🔥 Flexbox

Flexbox is designed primarily for arranging elements along **one dimension**.

```css
.container {
    display: flex;
}
```

Example:

```html
<div class="container">
    <div>One</div>
    <div>Two</div>
    <div>Three</div>
</div>
```

```css
.container {
    display: flex;
    gap: 20px;
}
```

Result:

```text
┌───────┐  ┌───────┐  ┌───────┐
│ One   │  │ Two   │  │ Three │
└───────┘  └───────┘  └───────┘
```

---

## Important Flexbox Properties

### Direction

```css
flex-direction: row;
```

or:

```css
flex-direction: column;
```

### Horizontal alignment

```css
justify-content: center;
```

### Cross-axis alignment

```css
align-items: center;
```

### Spacing

```css
gap: 20px;
```

---

# 🔥 CSS Grid

Grid is designed for **two-dimensional layouts**.

```css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
}
```

Conceptually:

```text
┌──────┬──────┬──────┐
│  1   │  2   │  3   │
├──────┼──────┼──────┤
│  4   │  5   │  6   │
└──────┴──────┴──────┘
```

### Flexbox vs Grid

| Feature              | Flexbox   | Grid      |
| -------------------- | --------- | --------- |
| Main purpose         | 1D layout | 2D layout |
| Rows                 | Limited   | Strong    |
| Columns              | Limited   | Strong    |
| Navigation           | Excellent | Possible  |
| Cards                | Good      | Excellent |
| Complex page layouts | Good      | Excellent |

### Memory Trick

```text
Flexbox → One direction
Grid    → Rows + Columns
```

---

# 📍 CSS Positioning

CSS provides several positioning methods.

```css
position: static;
position: relative;
position: absolute;
position: fixed;
position: sticky;
```

### Relative

```css
.box {
    position: relative;
}
```

The element remains in normal flow but can become a positioning reference for descendants.

---

### Absolute

```css
.box {
    position: absolute;
    top: 20px;
    right: 20px;
}
```

The element is positioned relative to an appropriate positioned ancestor.

---

### Fixed

```css
.menu {
    position: fixed;
    top: 0;
}
```

The element remains fixed relative to the viewport.

Useful for:

* Fixed navigation
* Floating buttons
* Persistent UI elements

---

### Sticky

```css
.header {
    position: sticky;
    top: 0;
}
```

The element behaves normally until a scrolling threshold is reached.

---

# 3. 📱 Responsive Design

A website should work across different screen sizes.

```text
Desktop
   ↓
Laptop
   ↓
Tablet
   ↓
Mobile
```

Responsive design allows the layout to adapt.

---

# 📐 Viewport

The viewport is the visible area of a webpage.

A common HTML setting is:

```html
<meta name="viewport"
      content="width=device-width, initial-scale=1.0">
```

This helps the page use the device's viewport appropriately.

---

# 🔥 Media Queries

Media queries allow CSS rules to change based on conditions such as viewport width.

```css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
}

@media (max-width: 768px) {
    .container {
        grid-template-columns: 1fr;
    }
}
```

Desktop:

```text
┌─────┬─────┬─────┐
│  A  │  B  │  C  │
└─────┴─────┴─────┘
```

Mobile:

```text
┌─────────┐
│    A    │
├─────────┤
│    B    │
├─────────┤
│    C    │
└─────────┘
```

---

# 📏 Responsive Units

Avoid designing everything using fixed pixels.

Common units:

| Unit  | Meaning                      |
| ----- | ---------------------------- |
| `px`  | Pixels                       |
| `%`   | Percentage                   |
| `em`  | Relative to parent font size |
| `rem` | Relative to root font size   |
| `vw`  | Viewport width               |
| `vh`  | Viewport height              |
| `fr`  | Fractional grid space        |

Example:

```css
.container {
    width: 90%;
    max-width: 1200px;
}
```

This is generally more adaptable than:

```css
.container {
    width: 1200px;
}
```

---

# 🖼️ Responsive Images

A common technique:

```css
img {
    max-width: 100%;
    height: auto;
}
```

This helps prevent an image from overflowing its container.

---

# 📱 Mobile-First Design

A mobile-first approach starts with the smaller screen and progressively enhances the layout.

```text
Mobile
  ↓
Tablet
  ↓
Desktop
```

Example:

```css
.card {
    width: 100%;
}

@media (min-width: 768px) {
    .card {
        width: 50%;
    }
}
```

---

# 🔐 Responsive Design & Security

Responsive design itself is not a security mechanism.

However, poor responsive design can affect security indirectly.

For example:

* Important security warnings may become difficult to see.
* Login controls may become confusing.
* MFA prompts may be poorly presented.
* Buttons may overlap.
* Users may accidentally click the wrong control.
* Security-related information may be hidden on smaller screens.

Good responsive design improves usability, but **server-side security must still enforce authentication and authorization**.

---

# 4. 🧩 UI Components

Modern websites are built from reusable UI components.

Examples:

```text
Navbar
Button
Card
Form
Modal
Dropdown
Alert
Search box
Login form
Navigation menu
```

---

# 🔘 Buttons

HTML:

```html
<button>Login</button>
```

CSS:

```css
button {
    padding: 10px 20px;
    border-radius: 6px;
    cursor: pointer;
}
```

Different states can be styled:

```css
button:hover {
    opacity: 0.8;
}

button:focus {
    outline: 2px solid blue;
}

button:disabled {
    opacity: 0.5;
}
```

---

# 📝 Forms

A login form might contain:

```html
<form>
    <label for="username">Username</label>

    <input
        id="username"
        name="username"
        type="text"
    >

    <label for="password">Password</label>

    <input
        id="password"
        name="password"
        type="password"
    >

    <button type="submit">Login</button>
</form>
```

CSS controls the visual presentation:

```css
input {
    padding: 10px;
    width: 100%;
}

button {
    padding: 10px;
}
```

Remember:

```text
HTML → Form structure
CSS  → Form appearance
JavaScript → Client-side behavior
Backend → Actual security enforcement
```

---

# 🪟 Modal

A modal is a dialog displayed above the main page.

Conceptually:

```text
┌─────────────────────────────┐
│          Web Page           │
│                             │
│       ┌──────────────┐      │
│       │    Login     │      │
│       │              │      │
│       │ Username     │      │
│       │ Password     │      │
│       │              │      │
│       │    Login     │      │
│       └──────────────┘      │
│                             │
└─────────────────────────────┘
```

CSS can control:

```css
position
width
height
background
border
box-shadow
z-index
```

---

# ⚠️ UI Security Considerations

UI components can create security problems when implemented incorrectly.

For example:

### Fake security UI

A malicious page could visually imitate:

```text
🔒 Secure Login
```

while actually sending credentials somewhere else.

Therefore:

> **Visual appearance does not prove security.**

Users and security testers should inspect:

* URL
* HTTPS
* Certificate
* Network requests
* Form destination
* JavaScript behavior
* Browser security indicators

---

# 🔢 `z-index`

`z-index` controls stacking order for positioned elements.

```css
.modal {
    position: fixed;
    z-index: 1000;
}
```

Conceptually:

```text
Higher z-index
      ↑
┌───────────────┐
│     Modal     │
└───────────────┘
┌───────────────┐
│     Page      │
└───────────────┘
```

But remember:

> `z-index` controls visual stacking. It does **not** provide security.

---

# 5. 🧠 User Experience Basics

**UX — User Experience** is how users experience and interact with a system.

Good UX tries to make a system:

* Clear
* Predictable
* Consistent
* Accessible
* Efficient
* Easy to understand

---

# 🔄 Consistency

Similar actions should look and behave similarly.

For example:

```text
Primary action → consistent button style
Cancel         → consistent secondary style
Error          → consistent error presentation
```

---

# ⚠️ Error Messages

Poor:

```text
ERROR 1234
```

Better:

```text
Incorrect username or password.
Please check your credentials and try again.
```

However, security-sensitive applications should avoid unnecessarily revealing information.

For example, an authentication system may avoid messages such as:

```text
Username exists, but password is incorrect.
```

because they could help attackers enumerate valid accounts.

A safer generic message can be:

```text
Invalid username or password.
```

---

# 🔐 UX + Security

Security and usability need to work together.

Consider MFA.

Bad UX:

```text
Enter code
[            ]
```

Better UX can provide:

```text
Enter the 6-digit verification code
We sent a code to your registered device.

[ _ _ _ _ _ _ ]

Didn't receive it?
[Resend code]
```

The security mechanism remains important, but the interface makes the workflow understandable.

---

# ♿ Accessibility

Accessible design helps people with different abilities use the application.

Important practices include:

* Proper `<label>` elements
* Keyboard navigation
* Sufficient text contrast
* Meaningful button names
* Alternative text for meaningful images
* Visible focus indicators
* Semantic HTML
* Avoiding color as the only indicator

Example:

❌

```text
🔴 = Error
🟢 = Success
```

Better:

```text
❌ Error: Password is incorrect.

✓ Success: Password updated.
```

This provides information beyond color alone.

---

# 🧠 CSS + Cybersecurity

CSS is primarily a presentation technology, but cybersecurity professionals still need to understand it.

Why?

Because security testers frequently inspect webpages.

A security tester may need to understand:

```text
HTML
  ↓
CSS
  ↓
JavaScript
  ↓
HTTP Requests
  ↓
Backend
  ↓
Database
```

CSS helps you understand the **visual layer**, while HTML and JavaScript expose more of the application's structure and behavior.

---

# 🔍 CSS in Security Testing

When investigating a webpage, inspect:

### 1. HTML

Look for:

```text
Forms
Inputs
Hidden fields
Links
Endpoints
Comments
```

### 2. CSS

Look for:

```text
Hidden UI
Responsive behavior
Overlays
Modals
Visual-only restrictions
```

### 3. JavaScript

Look for:

```text
Client-side validation
API calls
Tokens
DOM manipulation
Authentication logic
```

### 4. Network

Look for:

```text
Requests
Responses
Endpoints
Parameters
Cookies
Headers
Status codes
```

---

# 🚨 Important Security Concept: CSS Is Not Access Control

Suppose a website hides an admin button:

```css
.admin-button {
    display: none;
}
```

This does **not** make the admin functionality secure.

An attacker might still directly access the endpoint if the server does not enforce authorization.

Correct security model:

```text
Browser
   │
   │ Request
   ▼
Server
   │
   ├── Authenticate
   ├── Authorize
   ├── Validate
   └── Process
```

The server must decide:

```text
"Is this user actually allowed to perform this action?"
```

Not CSS.

---

# 🔥 Important Example

Imagine:

```html
<button class="admin-button">
    Delete User
</button>
```

CSS:

```css
.admin-button {
    display: none;
}
```

The button disappears.

But this:

```text
Hidden button
      ≠
Protected functionality
```

If the backend has:

```text
DELETE /users/123
```

the backend must verify that the requester has permission.

---

# 🧪 Hands-On Practice

## Task 1 — Create a Styled Page

Create:

```text
css-lab/
├── index.html
└── style.css
```

### `index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSS Security Lab</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>

    <div class="card">
        <h1>Security Lab</h1>

        <p>
            Learning CSS fundamentals.
        </p>

        <button>Login</button>
    </div>

</body>
</html>
```

### `style.css`

```css
body {
    font-family: Arial, sans-serif;
    background: #111;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

.card {
    width: 300px;
    padding: 30px;
    border: 1px solid #555;
    border-radius: 10px;
    text-align: center;
}

button {
    padding: 10px 20px;
    cursor: pointer;
}
```

Open `index.html` in your browser.

---

# 🧪 Task 2 — Practice Flexbox

Create three cards:

```text
┌──────┐ ┌──────┐ ┌──────┐
│  Web │ │Network│ │Linux │
└──────┘ └──────┘ └──────┘
```

Use:

```css
display: flex;
gap: 20px;
```

Then experiment with:

```css
justify-content
align-items
flex-direction
```

---

# 🧪 Task 3 — Practice Grid

Create a security dashboard:

```text
┌────────┬────────┬────────┐
│  CPU   │ Memory │ Disk   │
├────────┼────────┼────────┤
│ Alerts │ Logs   │ Users  │
└────────┴────────┴────────┘
```

Use:

```css
display: grid;
grid-template-columns: repeat(3, 1fr);
```

---

# 🧪 Task 4 — Responsive Design

Make the dashboard become one column on mobile.

```css
@media (max-width: 768px) {
    .dashboard {
        grid-template-columns: 1fr;
    }
}
```

Test it using:

```text
Browser
   ↓
DevTools
   ↓
Toggle Device Toolbar
   ↓
Select Mobile Device
```

---

# 🧪 Task 5 — Security Experiment

Create:

```html
<button class="admin-button">
    Admin Panel
</button>
```

Then:

```css
.admin-button {
    display: none;
}
```

Open DevTools.

Find the button in the **Elements** panel.

Remove:

```css
display: none;
```

Observe what happens.

### Question

Did CSS actually protect the admin functionality?

**Answer: No.**

It only changed the visual presentation.

This demonstrates:

> **Client-side hiding is not server-side authorization.**

---

# 🔐 CSS Security Checklist

When reviewing a web application:

* [ ] Do not treat hidden elements as protected functionality.
* [ ] Do not put secrets inside CSS.
* [ ] Do not assume visual restrictions are security controls.
* [ ] Remember that users control their browser.
* [ ] Check server-side authorization.
* [ ] Inspect hidden UI elements during testing.
* [ ] Test responsive layouts for security-related UI.
* [ ] Check error messages for unnecessary information disclosure.
* [ ] Consider accessibility.
* [ ] Use HTTPS for sensitive communication.
* [ ] Keep authentication and authorization on the server.

---

# 🧠 CSS Mental Model

Remember:

```text
HTML
  ↓
Structure

CSS
  ↓
Presentation + Layout

JavaScript
  ↓
Client-side Behavior

HTTP
  ↓
Communication

Backend
  ↓
Business Logic + Security

Database
  ↓
Data
```

The most important cybersecurity rule is:

```text
┌─────────────────────────────────────┐
│       CLIENT = UNTRUSTED            │
│                                     │
│ HTML / CSS / JS can be inspected    │
│ and potentially modified by users.  │
└─────────────────────────────────────┘
                  ↓
           Server validates
                  ↓
        Authentication + Authorization
```

---

# ⚡ Quick Revision

| Topic             | Remember                               |
| ----------------- | -------------------------------------- |
| CSS               | Controls webpage presentation          |
| Selector          | Chooses elements to style              |
| Box Model         | Content → Padding → Border → Margin    |
| Flexbox           | One-dimensional layout                 |
| Grid              | Two-dimensional layout                 |
| Media Query       | Changes styles based on conditions     |
| Responsive Design | Adapts to different screens            |
| UI Component      | Reusable interface element             |
| UX                | How users experience the system        |
| Accessibility     | Makes interfaces usable by more people |
| `z-index`         | Controls visual stacking               |
| CSS hiding        | **Not security**                       |
| Client            | **Untrusted**                          |

---

# 🎯 Memory Tricks

### CSS

> **CSS = Make HTML Look and Layout Better**

### Layout

```text
Flex → 1D
Grid → 2D
```

### Responsive

```text
Desktop
   ↓
Tablet
   ↓
Mobile
```

### Security

> **Hidden ≠ Protected**

> **CSS ≠ Access Control**

> **Client ≠ Trusted**

---

# 💼 Interview Questions

### 1. What is CSS?

CSS is a stylesheet language used to control the presentation, styling, and layout of HTML documents.

### 2. What is the CSS box model?

It describes an element as:

```text
Content → Padding → Border → Margin
```

### 3. Flexbox vs Grid?

Flexbox is primarily designed for one-dimensional layouts, while Grid is designed for two-dimensional layouts.

### 4. What is responsive design?

Responsive design allows a webpage to adapt its layout and presentation to different screen sizes and devices.

### 5. What are media queries?

Media queries allow CSS rules to be applied based on conditions such as viewport width.

### 6. Can CSS hide an admin page securely?

No.

CSS can hide the interface, but the server must enforce authorization.

### 7. Why is this important in cybersecurity?

Because anything delivered to the client can potentially be inspected or modified.

---

# 🔥 Final Takeaway

CSS controls the **visual presentation and layout** of a web application.

You should understand:

```text
Styling
   ↓
Box Model
   ↓
Flexbox / Grid
   ↓
Responsive Design
   ↓
UI Components
   ↓
UX
   ↓
Security Implications
```

For cybersecurity, the most important lesson is:

> **Never confuse what the browser displays with what the server actually allows.**

A hidden button is not protected.
A disabled input is not trusted.
A CSS rule is not authorization.

**The browser is the client — and the client must be treated as untrusted.**

This completes **CSS Fundamentals**. The next topic can continue directly into **JavaScript / client-side scripting** if that is the next item in your roadmap.
