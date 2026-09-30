# 🌐 1. Understand Web Fundamentals

> **“Before learning how to secure or attack a web application, you must understand how the web application actually works.”**

Web applications are everywhere:

* Websites
* Online banking
* E-commerce
* Social media
* Cloud platforms
* APIs
* Online games
* Government portals
* SaaS applications

As a cybersecurity learner, you need to understand what happens when you enter a URL and press **Enter**.

The simplified process is:

```text
You
 ↓
Browser
 ↓
DNS
 ↓
Server
 ↓
Web Application
 ↓
Database / Services
 ↓
Response
 ↓
Browser
 ↓
Web Page
```

---

# 1. 🌍 How Websites Work

Let's start from the moment you type:

```text
https://example.com
```

into your browser.

Several things happen before you see the webpage.

---

## 🔹 Step 1 — The Browser Reads the URL

A URL has several parts.

Example:

```text
https://www.example.com/products?id=10
```

Break it down:

```text
https://
   ↓
Protocol / Scheme

www.example.com
   ↓
Hostname

/products
   ↓
Path

?id=10
   ↓
Query Parameter
```

### URL Structure

```text
https://www.example.com/products?id=10
  │       │              │       │
  │       │              │       └── Query
  │       │              └────────── Path
  │       └───────────────────────── Hostname
  └───────────────────────────────── Scheme
```

---

# 🔹 Step 2 — DNS Resolution

Your browser needs the server's IP address.

You know:

```text
example.com
```

but computers communicate using IP addresses.

DNS translates:

```text
Domain Name
     ↓
DNS
     ↓
IP Address
```

For example:

```text
example.com
     ↓
93.184.216.x
```

The exact address can vary by service and time.

---

## 🧠 Why DNS Exists

Imagine having to remember:

```text
142.250.x.x
104.x.x.x
172.x.x.x
```

for every website.

Instead, humans use:

```text
google.com
github.com
example.com
```

DNS provides the name-to-address resolution.

---

# 🔹 Step 3 — Establishing a Connection

Once the browser knows the destination IP, it needs to communicate with the server.

For HTTPS:

```text
Browser
   │
   │ TCP
   ↓
Server
   │
   │ TLS
   ↓
Secure Connection
```

The exact networking details depend on the protocol and transport being used.

Traditional HTTPS commonly uses:

```text
HTTP
 ↓
TLS
 ↓
TCP
 ↓
IP
```

Modern HTTP/3 uses:

```text
HTTP/3
 ↓
QUIC
 ↓
UDP
 ↓
IP
```

This distinction becomes important later when you study web traffic analysis.

---

# 🔐 Step 4 — HTTPS / TLS

If the website uses HTTPS, TLS helps establish a secure communication channel.

Conceptually:

```text
Browser
   ↓
TLS Handshake
   ↓
Authentication + Key Establishment
   ↓
Encrypted Communication
   ↓
Web Server
```

This protects data while it travels across the network.

For example:

```text
Username
Password
Session Cookie
API Request
```

should not normally travel across an HTTPS connection as readable plaintext.

---

# 📤 Step 5 — Browser Sends an HTTP Request

Once the connection is ready, the browser sends an HTTP request.

Example:

```http
GET /products HTTP/1.1
Host: example.com
```

The request essentially says:

> “I want the `/products` resource from this server.”

A modern HTTP request can contain:

* Method
* URL/path
* Headers
* Cookies
* Query parameters
* Request body

---

# 📥 Step 6 — Server Processes the Request

The web server receives the request.

For example:

```text
Browser
   ↓
HTTP Request
   ↓
Web Server
   ↓
Application
```

The application may:

* Authenticate the user
* Validate input
* Query a database
* Perform calculations
* Call another API
* Check permissions
* Generate a response

---

# 🔹 Step 7 — Server Sends a Response

The server sends an HTTP response.

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

followed by the response body.

The response could contain:

```text
HTML
CSS
JavaScript
JSON
Images
Videos
Files
```

---

# 🔹 Step 8 — Browser Renders the Page

The browser receives the response.

For example:

```text
HTML
 ↓
DOM

CSS
 ↓
Styling

JavaScript
 ↓
Behavior
```

The browser combines these resources to produce the webpage you see.

---

# 🌐 Complete Website Workflow

The simplified workflow is:

```text
                 User
                   │
                   ↓
                Browser
                   │
                   ↓
                DNS Lookup
                   │
                   ↓
                IP Address
                   │
                   ↓
            Network Connection
                   │
                   ↓
               TLS / HTTPS
                   │
                   ↓
             HTTP Request
                   │
                   ↓
              Web Server
                   │
                   ↓
             Web Application
                   │
          ┌────────┴────────┐
          ↓                 ↓
      Database          Other APIs
          │                 │
          └────────┬────────┘
                   ↓
             HTTP Response
                   │
                   ↓
                Browser
                   │
                   ↓
              Web Page
```

---

# 🖥️ 2. Client-Server Architecture

The **client-server model** is one of the fundamental concepts of web applications.

## Client

The client is the system requesting a service.

Examples:

* Web browser
* Mobile application
* Desktop application
* API client

In a normal website:

```text
Chrome / Firefox
       ↓
     Client
```

---

## Server

The server provides the requested service or resource.

It may:

* Receive requests
* Process data
* Authenticate users
* Access databases
* Execute application logic
* Return responses

```text
Client
  ↓
Server
```

---

# 🔄 Basic Client-Server Model

```text
┌──────────────┐
│    Client    │
│   Browser    │
└──────┬───────┘
       │
       │ Request
       ↓
┌──────────────┐
│    Server    │
│ Application  │
└──────┬───────┘
       │
       │ Response
       ↓
┌──────────────┐
│    Client    │
│   Browser    │
└──────────────┘
```

The basic pattern is:

```text
REQUEST → PROCESS → RESPONSE
```

---

# 🧠 Example

You visit:

```text
https://example.com/profile
```

The browser sends something like:

```text
GET /profile
```

The server:

```text
Receives request
      ↓
Checks authentication
      ↓
Retrieves user information
      ↓
Generates response
      ↓
Sends response
```

The browser displays the result.

---

# 🏗️ Servers Are Not Always One Computer

A modern application may have multiple servers.

For example:

```text
                 Internet
                    │
                    ↓
              Load Balancer
                    │
          ┌─────────┼─────────┐
          ↓         ↓         ↓
       Server 1  Server 2  Server 3
          │         │         │
          └─────────┼─────────┘
                    ↓
               Database
```

This provides scalability and availability.

---

# ⚔️ Cybersecurity Perspective

The client-server architecture creates multiple attack surfaces.

For example:

```text
Client
  │
  ├── Browser vulnerabilities
  ├── Malicious JavaScript
  └── Credential theft
       │
       ↓
Network
  │
  ├── Traffic interception
  ├── DNS attacks
  └── TLS misconfiguration
       │
       ↓
Server
  │
  ├── Authentication flaws
  ├── Authorization flaws
  ├── Injection
  ├── Misconfiguration
  └── Vulnerable software
       │
       ↓
Database
  │
  ├── Data exposure
  ├── Weak access control
  └── Injection
```

Understanding architecture helps you understand where vulnerabilities can exist.

---

# 🎨 3. Frontend and Backend Concepts

A web application is commonly divided into two major areas:

```text
Frontend
   +
Backend
```

---

# 🎨 Frontend

The **frontend** is the part of the application that runs in the user's browser or client.

Common technologies:

```text
HTML
CSS
JavaScript
```

Modern frameworks include:

```text
React
Angular
Vue
Svelte
```

The frontend handles things such as:

* User interface
* Buttons
* Forms
* Navigation
* Displaying data
* Client-side interaction
* Sending requests to APIs

---

# 🧱 HTML

HTML defines the structure.

Example:

```html
<h1>Welcome</h1>

<p>Hello, cybersecurity student.</p>

<button>Login</button>
```

Think:

```text
HTML = Structure
```

---

# 🎨 CSS

CSS controls appearance.

Example:

```css
button {
    font-size: 20px;
}
```

Think:

```text
CSS = Presentation
```

---

# ⚙️ JavaScript

JavaScript provides behavior.

Example:

```javascript
console.log("Hello");
```

Think:

```text
JavaScript = Behavior
```

---

# 🧠 Frontend Security

Frontend code should never be treated as a trusted security boundary.

For example:

```javascript
if (userIsAdmin) {
    showAdminPanel();
}
```

Hiding an admin button does **not** provide real authorization.

A malicious user may directly send requests to the backend.

Therefore:

> **Security decisions must be enforced on the server side.**

---

# ⚙️ Backend

The backend runs on the server.

It contains application logic.

Common backend technologies include:

```text
Python
Java
JavaScript / Node.js
PHP
Go
C#
Ruby
```

Backend responsibilities can include:

* Authentication
* Authorization
* Business logic
* Database operations
* API handling
* Session management
* Input validation
* File processing
* Security controls

---

# 🗄️ Database

The backend commonly communicates with a database.

Examples:

```text
PostgreSQL
MySQL
MariaDB
MongoDB
Redis
```

Typical flow:

```text
Browser
   ↓
Frontend
   ↓
Backend API
   ↓
Database
```

The browser should generally not have direct unrestricted access to the database.

---

# 🔄 Frontend + Backend Example

Imagine a login page.

```text
                Login Page
                    │
                    ↓
               Frontend
                    │
             POST /login
                    │
                    ↓
                Backend
                    │
              Verify user
                    │
                    ↓
                Database
                    │
              User record
                    │
                    ↓
                Backend
                    │
             Authentication
                    │
                    ↓
                Browser
```

---

# 🔐 Where Security Happens

Security is not only a frontend responsibility.

A simplified model:

```text
Frontend
 ↓
Basic validation / UX
 ↓
Backend
 ↓
Real security validation
 ↓
Database / Services
```

For example, if the frontend says:

```text
age = 20
```

the backend should still validate the received data.

Never assume:

> “The browser already checked it.”

The client is controlled by the user.

---

# 🌐 4. Web Application Workflows

A web application is not simply:

```text
Request → Page
```

Modern applications contain workflows.

For example:

```text
Register
   ↓
Login
   ↓
Session
   ↓
Dashboard
   ↓
Profile
   ↓
Logout
```

Let's examine this.

---

# 👤 Registration Workflow

A simplified registration process:

```text
User
 ↓
Registration Form
 ↓
Frontend
 ↓
POST /register
 ↓
Backend
 ↓
Validate Input
 ↓
Hash Password
 ↓
Store User
 ↓
Response
```

Important security concepts include:

* Input validation
* Password hashing
* Duplicate-account handling
* Email verification
* Rate limiting
* Secure session handling

---

# 🔑 Login Workflow

```text
User
 ↓
Login Form
 ↓
POST /login
 ↓
Backend
 ↓
Find User
 ↓
Verify Password
 ↓
Create Session / Token
 ↓
Response
 ↓
Authenticated User
```

---

# 🍪 Session Workflow

HTTP is fundamentally request/response based and does not inherently remember previous requests.

Web applications therefore use mechanisms such as:

* Cookies
* Sessions
* Tokens

Example:

```text
Login
 ↓
Server creates session
 ↓
Session identifier
 ↓
Browser stores cookie
 ↓
Browser sends cookie
 ↓
Server identifies session
```

Conceptually:

```text
Browser
   │
   │ Cookie: session_id=...
   ↓
Server
   │
   ↓
Session Store
   │
   ↓
Authenticated User
```

Session security becomes extremely important later in web security.

---

# 🛒 E-Commerce Workflow

Imagine buying a product.

```text
Browse Product
      ↓
View Product
      ↓
Add to Cart
      ↓
Checkout
      ↓
Payment
      ↓
Order Creation
      ↓
Confirmation
```

Each step may involve:

```text
HTTP Requests
       ↓
Authentication
       ↓
Authorization
       ↓
Database Operations
       ↓
External Services
```

A vulnerability in one workflow step can affect the entire application.

---

# 💳 Example: Payment Workflow

A simplified architecture:

```text
User
 ↓
Frontend
 ↓
Backend
 ↓
Payment Service
 ↓
Payment Provider
 ↓
Backend
 ↓
Order Database
 ↓
User
```

The backend should verify important information rather than trusting values sent by the browser.

For example:

```text
❌ Trust price from browser

price = ₹10
```

Instead:

```text
Browser → Product ID
             ↓
         Backend
             ↓
       Database
             ↓
      Official Price
```

This is an important cybersecurity principle:

> **Never trust client-controlled input for security-sensitive decisions.**

---

# 🔄 5. Modern Web Architecture

Modern web applications are much more complex than a single server.

A typical architecture may look like:

```text
                       Internet
                           │
                           ↓
                    DNS / CDN / WAF
                           │
                           ↓
                    Load Balancer
                           │
                ┌──────────┴──────────┐
                ↓                     ↓
           Web Frontend          API Gateway
                                      │
                                      ↓
                              Application Services
                           ┌──────────┼──────────┐
                           ↓          ↓          ↓
                       Auth       Payments    Users
                           │          │          │
                           └──────────┼──────────┘
                                      ↓
                                  Databases
                                      │
                           ┌──────────┼──────────┐
                           ↓          ↓          ↓
                        Cache      Storage    Logs
```

This is a simplified architecture, but it demonstrates the idea.

---

# 🌍 CDN

A **Content Delivery Network (CDN)** distributes content through geographically distributed infrastructure.

It can help with:

* Faster content delivery
* Caching
* Availability
* Traffic distribution
* Some security protections

Example:

```text
User
 ↓
Nearest CDN Edge
 ↓
Cached Content
```

---

# 🛡️ WAF

A **Web Application Firewall (WAF)** sits in front of a web application and analyzes HTTP traffic according to configured rules.

Conceptually:

```text
Internet
   ↓
WAF
   ↓
Application
```

It can help detect or block certain malicious requests.

However:

> A WAF is not a replacement for secure application development.

---

# ⚖️ Load Balancer

A load balancer distributes traffic between application servers.

```text
             Load Balancer
             /     |     \
            ↓      ↓      ↓
         Server  Server  Server
            1      2      3
```

Benefits can include:

* Scalability
* Availability
* Traffic distribution

---

# 🚪 API Gateway

An API gateway can act as an entry point for APIs.

It may handle:

* Routing
* Authentication integration
* Rate limiting
* Request policies
* Logging
* Traffic management

Example:

```text
Mobile App
    │
    ↓
API Gateway
    │
 ┌──┼──────────┐
 ↓  ↓          ↓
Auth Users   Orders
API  API      API
```

---

# 🧩 Microservices

Instead of one large application:

```text
Monolithic Application
```

a system may use multiple services:

```text
Authentication Service
        │
        ├── User Service
        │
        ├── Payment Service
        │
        ├── Order Service
        │
        └── Notification Service
```

Each service may have its own:

* API
* Logic
* Database
* Authentication requirements
* Security boundaries

---

# 🗄️ Modern Data Architecture

Modern applications can use several types of storage:

```text
Application
    │
    ├── Relational Database
    │
    ├── NoSQL Database
    │
    ├── Cache
    │
    ├── Object Storage
    │
    └── Search Engine
```

Examples:

```text
PostgreSQL
MongoDB
Redis
Object Storage
Elasticsearch
```

---

# ☁️ Cloud Web Architecture

Modern applications are frequently deployed using cloud infrastructure.

A simplified architecture:

```text
                    Internet
                       │
                       ↓
                    CDN/WAF
                       │
                       ↓
                 Load Balancer
                       │
              ┌────────┴────────┐
              ↓                 ↓
         App Server         App Server
              │                 │
              └────────┬────────┘
                       ↓
                    Database
                       │
                 ┌─────┴─────┐
                 ↓           ↓
              Storage       Cache
```

Cloud environments can introduce additional security considerations:

* IAM
* Security groups
* Network segmentation
* Secrets management
* Logging
* Storage permissions
* API security
* Container security

---

# 🐳 Containers

Applications may also run inside containers.

For example:

```text
Host
 │
 ├── Container
 │     └── Web App
 │
 ├── Container
 │     └── API
 │
 └── Container
       └── Worker
```

Popular container technologies include:

```text
Docker
Kubernetes
```

Containerized architecture introduces additional security areas such as:

* Image vulnerabilities
* Container privileges
* Secrets
* Network policies
* Orchestration security

---

# 🔐 Cybersecurity View of Modern Web Architecture

As architecture becomes more complex, the attack surface can increase.

```text
                    Internet
                       │
                       ↓
                      CDN
                       │
                       ↓
                      WAF
                       │
                       ↓
                Load Balancer
                       │
                       ↓
                  API Gateway
                       │
            ┌──────────┼──────────┐
            ↓          ↓          ↓
           API       Auth       Services
            │          │          │
            └──────────┼──────────┘
                       ↓
                    Database
                       │
              ┌────────┼────────┐
              ↓        ↓        ↓
           Storage    Cache     Logs
```

Every component creates potential:

```text
Configuration
Authentication
Authorization
Network
Software
Data
```

security considerations.

---

# 🎯 Attack Surface

An **attack surface** is the collection of points where an attacker could potentially interact with or affect a system.

For a web application:

```text
Attack Surface
│
├── Web pages
├── APIs
├── Login systems
├── File uploads
├── Parameters
├── Cookies
├── Headers
├── Third-party integrations
├── Admin interfaces
└── Infrastructure
```

This concept will become extremely important when you start web penetration testing.

---

# 🧠 Trust Boundaries

A **trust boundary** is a point where data or control moves between different trust levels.

Example:

```text
Untrusted Internet
       │
       ↓
     WAF
       │
       ↓
Application
       │
       ↓
Database
```

Another example:

```text
Browser
   │
   │ User-controlled input
   ↓
Backend
```

The backend should treat incoming data as **untrusted** until it has been validated.

---

# 🔥 The Most Important Web Security Principle

Remember:

> ## **Never Trust the Client**

The browser belongs to the user.

A user can modify:

* HTML
* JavaScript
* Requests
* Headers
* Cookies
* Parameters
* API calls

For example, suppose a browser sends:

```http
POST /transfer
```

with:

```text
amount=100
```

The backend cannot simply assume:

> “The frontend only allows valid amounts.”

The backend must independently validate:

```text
Authentication
      ↓
Authorization
      ↓
Input Validation
      ↓
Business Logic
      ↓
Transaction
```

This principle will appear repeatedly throughout web security.

---

# 🔍 Web Application Security Map

You will eventually study vulnerabilities around these components:

```text
                 Web Application
                       │
       ┌───────────────┼───────────────┐
       ↓               ↓               ↓
    Frontend         Backend          APIs
       │               │               │
       ↓               ↓               ↓
   Browser         Business Logic   Endpoints
       │               │               │
       └───────────────┼───────────────┘
                       ↓
                  Authentication
                       ↓
                  Authorization
                       ↓
                    Database
                       ↓
                     Data
```

Later, this will connect to topics such as:

* XSS
* SQL Injection
* CSRF
* IDOR / Broken Access Control
* Authentication vulnerabilities
* Session attacks
* File upload vulnerabilities
* SSRF
* API security
* Security misconfiguration

---

# 🧪 Hands-on Task

Before moving to the next topic, do this simple exercise.

## Task 1 — Browser → Server Observation

Open a website you are authorized to inspect.

Use browser Developer Tools:

```text
F12
```

Open:

```text
Network
```

Reload the page.

You should see requests.

For one request, identify:

```text
Request URL
HTTP Method
Status Code
Request Headers
Response Headers
Content Type
```

Try to identify:

```text
HTML
CSS
JavaScript
Images
API requests
```

---

# 🧪 Task 2 — Identify the Architecture

Choose a website/application and create a simplified architecture diagram.

Example:

```text
             Browser
                │
                ↓
             HTTPS
                │
                ↓
          Web Server
                │
                ↓
          Backend API
                │
          ┌─────┴─────┐
          ↓           ↓
      Database      External API
```

Then answer:

1. What is the client?
2. What is the server?
3. Where is the frontend?
4. Where is the backend?
5. Is there an API?
6. Where might authentication happen?
7. Where might sensitive data be stored?
8. What are the possible attack surfaces?

---

# 🧪 Task 3 — Follow a Login Workflow

Use a **practice/lab application or your own test application**.

Observe:

```text
Login Page
   ↓
Login Request
   ↓
Server Response
   ↓
Cookie / Session / Token
   ↓
Authenticated Request
```

Don't test credentials or accounts that don't belong to you.

Your goal is only to understand the workflow.

---

# 🧠 Interview Questions

### 1. What is a web application?

A software application accessed through a web interface or web protocols, typically involving a client, server-side application logic, and data/services.

### 2. What is client-server architecture?

A model where a client requests services or resources and a server processes those requests and returns responses.

### 3. What is frontend?

The client-side portion of a web application responsible mainly for the user interface and browser-side behavior.

### 4. What is backend?

The server-side portion responsible for application logic, data processing, authentication, authorization, APIs, and other server-side operations.

### 5. What is DNS?

A system that resolves domain names into network addresses and provides other DNS information.

### 6. What is HTTPS?

HTTP carried over a TLS-protected connection.

### 7. Why shouldn't the backend trust the frontend?

Because the client is controlled by the user and its requests can be modified.

### 8. What is an API?

An interface that allows software components to communicate through defined operations and data formats.

### 9. What is an attack surface?

The collection of points through which a system can potentially be interacted with or attacked.

### 10. What is a trust boundary?

A boundary where data or control crosses between different trust levels or security domains.

---

# ⚡ Quick Revision

```text
URL
 ↓
DNS
 ↓
IP Address
 ↓
Connection
 ↓
TLS / HTTPS
 ↓
HTTP Request
 ↓
Web Server
 ↓
Backend
 ↓
Database / Services
 ↓
HTTP Response
 ↓
Browser
```

### Remember:

```text
Frontend = What the user interacts with

Backend = Where server-side logic happens

Database = Where application data may be stored

API = Communication interface between components

DNS = Domain → Network information

HTTPS = HTTP + TLS

Client = Requests

Server = Processes and responds

Attack Surface = Possible interaction/attack points

Trust Boundary = Change in trust/security domain
```

---

# 🧠 Memory Map

## **B → C → F → W → M**

```text
B = Browser / How websites work
C = Client-Server
F = Frontend + Backend
W = Web Application Workflow
M = Modern Web Architecture
```

And the most important cybersecurity principle:

> 🔥 **Never Trust the Client.**

---

# 🏁 Final Takeaway

A modern web application is not just a webpage.

It is a collection of interacting components:

```text
Browser
   ↓
Frontend
   ↓
HTTP / HTTPS
   ↓
Web Server
   ↓
Backend / APIs
   ↓
Authentication
   ↓
Business Logic
   ↓
Database
   ↓
External Services
```

As a cybersecurity professional, you need to understand **how data moves through these components, where trust changes, and where security decisions are enforced**.

Once you understand this architecture, web security vulnerabilities become much easier to understand because you can ask:

> **“Which component processes this data, what does it trust, and what happens next?”**

That question will become one of your most useful habits in web security.
