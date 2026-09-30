# 🌐 HTTP Fundamentals — Web Security Roadmap

> **HTTP (HyperText Transfer Protocol)** is the application-layer protocol used for communication between clients and web servers.

When you open a website, submit a login form, load an image, call an API, or send data to a server, HTTP is often involved.

The basic communication model is:

```text
┌──────────────┐
│    Client    │
│   Browser    │
└──────┬───────┘
       │
       │ HTTP Request
       ▼
┌──────────────┐
│ Web Server   │
│ Application  │
└──────┬───────┘
       │
       │ HTTP Response
       ▼
┌──────────────┐
│    Client    │
│   Browser    │
└──────────────┘
```

---

# 1. 📤 HTTP Requests

An **HTTP request** is a message sent by a client to a server asking it to perform an action or return a resource.

For example:

```text
Browser → GET /index.html → Server
```

A request can contain:

```text
Request Line
Headers
Blank Line
Body (optional)
```

Example:

```http
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

---

# 🔹 Request Line

The first line contains:

```text
METHOD + PATH + HTTP VERSION
```

Example:

```http
GET /login HTTP/1.1
```

Breakdown:

| Part       | Meaning                 |
| ---------- | ----------------------- |
| `GET`      | HTTP method             |
| `/login`   | Requested resource/path |
| `HTTP/1.1` | HTTP version            |

---

# 🔹 Host Header

```http
Host: example.com
```

It identifies the host being requested.

In HTTP/1.1, the `Host` header is required for requests.

Example:

```http
GET /products HTTP/1.1
Host: shop.example.com
```

The same server infrastructure can host multiple websites.

---

# 🔹 Request Headers

Headers provide additional information about the request.

Example:

```http
User-Agent: Mozilla/5.0
Accept: text/html
Accept-Language: en-US
Cookie: session=abc123
```

Headers can communicate:

* Client information
* Accepted content types
* Authentication information
* Cookies
* Caching instructions
* Security-related information

---

# 🔹 Request Body

Some HTTP requests contain a body.

For example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=farhan&password=example
```

The body contains the data being sent to the server.

Commonly used with:

```text
POST
PUT
PATCH
```

Although HTTP semantics allow bodies with other methods too, their use varies by implementation.

---

# 🔥 Complete HTTP Request

Example:

```http
POST /login HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 31
Cookie: session=abc123

username=farhan&password=test123
```

Think of it as:

```text
┌───────────────────────────────────┐
│ Request Line                      │
│ POST /login HTTP/1.1              │
├───────────────────────────────────┤
│ Headers                           │
│ Host: example.com                 │
│ Content-Type: ...                 │
│ Cookie: ...                       │
├───────────────────────────────────┤
│ Body                              │
│ username=farhan&password=test123  │
└───────────────────────────────────┘
```

---

# 2. 📥 HTTP Responses

After receiving a request, the server sends an **HTTP response**.

```text
Client
  │
  │ Request
  ▼
Server
  │
  │ Response
  ▼
Client
```

A response contains:

```text
Status Line
Headers
Blank Line
Body (optional)
```

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1250

<html>
    <body>
        <h1>Hello</h1>
    </body>
</html>
```

---

# 🔹 Response Status Line

The first line contains:

```text
HTTP VERSION + STATUS CODE + REASON PHRASE
```

Example:

```http
HTTP/1.1 200 OK
```

Meaning:

```text
HTTP/1.1 → Protocol version
200      → Status code
OK       → Reason phrase
```

---

# 🔹 Response Headers

Example:

```http
Content-Type: text/html
Content-Length: 1250
Set-Cookie: session=abc123
Cache-Control: no-cache
```

These tell the browser how to handle the response.

---

# 🔹 Response Body

The body contains the actual response data.

It might be:

```text
HTML
JSON
CSS
JavaScript
Image
PDF
Plain text
```

Example API response:

```json
{
    "username": "farhan",
    "role": "user"
}
```

---

# 🔄 Request → Response Flow

Consider opening:

```text
https://example.com/login
```

The simplified process is:

```text
Browser
   │
   │ GET /login
   ▼
Web Server
   │
   │ Process request
   ▼
Application
   │
   │ Generate response
   ▼
Web Server
   │
   │ 200 OK + HTML
   ▼
Browser
```

The browser then processes the response and renders the page.

---

# 3. 🛠️ HTTP Methods

HTTP methods describe the intended operation of a request.

Common methods include:

```text
GET
POST
PUT
PATCH
DELETE
HEAD
OPTIONS
```

---

# 🔵 GET

Used to retrieve a resource.

Example:

```http
GET /products HTTP/1.1
Host: example.com
```

Conceptually:

```text
Client → "Give me this resource."
```

Example:

```text
GET /images/logo.png
GET /users/42
GET /products
```

GET requests are generally intended to be **safe** and should not cause a state-changing action on the server.

---

# 🟢 POST

Used to submit data or request an operation that may create/change server-side state.

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "username": "farhan",
    "password": "example"
}
```

Common uses:

* Login
* Registration
* Creating resources
* Submitting forms
* Uploading data

---

# 🟡 PUT

Usually used to create or completely replace a resource at a specified target URI.

Example:

```http
PUT /users/42 HTTP/1.1
Content-Type: application/json

{
    "username": "farhan",
    "role": "user"
}
```

Conceptually:

```text
PUT → "Set this resource to this representation."
```

---

# 🟠 PATCH

Used for a **partial modification**.

Example:

```http
PATCH /users/42 HTTP/1.1
Content-Type: application/json

{
    "email": "new@example.com"
}
```

Conceptually:

```text
PUT
→ Replace/update the resource representation

PATCH
→ Modify selected parts
```

---

# 🔴 DELETE

Used to request deletion of a resource.

Example:

```http
DELETE /users/42 HTTP/1.1
```

Conceptually:

```text
Client → "Delete this resource."
```

Whether the operation succeeds depends on server-side authorization and application logic.

---

# ⚪ HEAD

Similar to GET, but asks for the response headers without the response body.

Example:

```http
HEAD /index.html HTTP/1.1
Host: example.com
```

Useful for checking things such as:

```text
Content-Type
Content-Length
Last-Modified
Caching information
```

---

# 🟣 OPTIONS

Used to ask what communication options are available for a target resource.

Example:

```http
OPTIONS /api/users HTTP/1.1
Host: example.com
```

A server might respond with:

```http
Allow: GET, POST, OPTIONS
```

OPTIONS is also involved in **CORS preflight requests** in browsers.

---

# 🧠 HTTP Methods Summary

| Method  | Common purpose                           |
| ------- | ---------------------------------------- |
| GET     | Retrieve                                 |
| POST    | Submit/create/process                    |
| PUT     | Replace/create at target URI             |
| PATCH   | Partially modify                         |
| DELETE  | Delete                                   |
| HEAD    | Headers without body                     |
| OPTIONS | Discover supported communication options |

---

# 🔥 Important Security Concept: GET vs POST

Beginners often think:

> "POST is secure and GET is insecure."

That is incorrect.

HTTP method ≠ encryption.

For example:

```text
HTTP GET
```

and

```text
HTTP POST
```

can both expose data to anyone able to observe unencrypted HTTP traffic.

Security comes from mechanisms such as:

```text
HTTPS
  ↓
TLS
  ↓
Encryption + Authentication + Integrity
```

Also, **POST data is not automatically protected just because it is in the body**.

---

# 4. 🚦 HTTP Status Codes

HTTP status codes tell the client what happened to the request.

They are three-digit numbers.

```text
1xx → Informational
2xx → Success
3xx → Redirection
4xx → Client error
5xx → Server error
```

---

# 🔵 1xx — Informational

These indicate that the request has been received and processing is continuing.

Examples:

```text
100 Continue
101 Switching Protocols
```

They are less commonly encountered during normal beginner web testing.

---

# 🟢 2xx — Success

The request was successfully received and processed.

### 200 OK

```http
HTTP/1.1 200 OK
```

Common for successful:

```text
GET
API request
Page loading
```

---

### 201 Created

```http
HTTP/1.1 201 Created
```

Usually means a new resource was successfully created.

Example:

```text
POST /users
```

→

```text
201 Created
```

---

### 204 No Content

The request succeeded, but there is no response body.

Common example:

```text
DELETE /users/42
```

→

```text
204 No Content
```

---

# 🟡 3xx — Redirection

These indicate that the client should take additional action, often involving another URL.

### 301 Moved Permanently

Indicates a resource has permanently moved.

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.com/new-page
```

---

### 302 Found

Indicates a temporary redirection in common usage.

```http
HTTP/1.1 302 Found
Location: /login
```

---

### 304 Not Modified

Used with caching.

Conceptually:

```text
Browser:
"Has this resource changed?"

Server:
"No. Use your cached copy."
```

---

# 🔴 4xx — Client Errors

These indicate a problem with the request from the client's perspective.

---

### 400 Bad Request

The server cannot process the request because it is malformed or invalid.

Example:

```http
HTTP/1.1 400 Bad Request
```

Possible causes:

* Invalid syntax
* Invalid parameters
* Malformed JSON

---

### 401 Unauthorized

This generally means authentication is required or the provided authentication is not acceptable.

Example:

```http
HTTP/1.1 401 Unauthorized
```

Think:

```text
Authentication problem
```

---

### 403 Forbidden

The server understood the request but refuses to authorize it.

Think:

```text
Authentication may exist
        ↓
Authorization denied
```

Example:

```text
User → /admin
       ↓
       403 Forbidden
```

---

### 404 Not Found

The requested resource could not be found.

```http
HTTP/1.1 404 Not Found
```

Example:

```text
GET /does-not-exist
```

---

### 405 Method Not Allowed

The method is not supported for the requested resource.

Example:

```text
POST /read-only-resource
```

might produce:

```http
405 Method Not Allowed
```

---

### 429 Too Many Requests

The client has sent too many requests in a given period.

Often associated with:

```text
Rate limiting
Brute-force protection
API abuse protection
```

Example:

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 60
```

---

# 🔥 5xx — Server Errors

These indicate that the server failed to successfully process a valid request.

---

### 500 Internal Server Error

Generic server-side failure.

```http
HTTP/1.1 500 Internal Server Error
```

---

### 502 Bad Gateway

A server acting as a gateway/proxy received an invalid response from an upstream server.

---

### 503 Service Unavailable

The server is currently unable to handle the request.

Possible reasons include:

```text
Overload
Maintenance
Unavailable dependency
```

---

### 504 Gateway Timeout

A gateway/proxy did not receive a timely response from an upstream server.

---

# 🧠 Status Code Memory

```text
1xx → Information
2xx → Success
3xx → Redirect
4xx → Client-side problem
5xx → Server-side problem
```

Important security-related codes:

```text
200 → Success
301/302 → Redirect
400 → Bad request
401 → Authentication required/failed
403 → Authorization denied
404 → Not found
405 → Method not allowed
429 → Rate limited
500 → Server error
```

---

# 5. 📋 HTTP Headers

Headers are key-value metadata attached to HTTP requests and responses.

Format:

```text
Header-Name: value
```

Example:

```http
Content-Type: application/json
```

Headers are extremely important in cybersecurity.

They can contain information about:

* Content
* Authentication
* Cookies
* Caching
* Security policies
* Client behavior
* Server behavior

---

# 📤 Common Request Headers

## Host

```http
Host: example.com
```

Specifies the target host.

---

## User-Agent

```http
User-Agent: Mozilla/5.0
```

Identifies information about the client software.

Example:

```text
Browser
Operating system
Browser engine
```

Do not treat the User-Agent as trustworthy authentication information because clients can generally modify it.

---

## Accept

```http
Accept: text/html
```

Tells the server which response content types the client can handle.

Example:

```http
Accept: application/json
```

---

## Accept-Language

```http
Accept-Language: en-US,en;q=0.9
```

Indicates preferred languages.

---

## Authorization

Used to send authentication credentials or tokens according to the authentication scheme.

Example:

```http
Authorization: Bearer <token>
```

Treat authentication tokens as sensitive secrets.

---

## Cookie

```http
Cookie: session=abc123
```

Sends cookies stored for the site.

Cookies can contain:

```text
Session identifiers
Preferences
Tracking identifiers
Other application data
```

---

## Content-Type

Describes the format of the request body.

Example:

```http
Content-Type: application/json
```

Another example:

```http
Content-Type: application/x-www-form-urlencoded
```

---

# 📥 Common Response Headers

## Content-Type

```http
Content-Type: text/html
```

Tells the client what type of content is being returned.

Examples:

```text
text/html
application/json
text/css
application/javascript
image/png
```

---

## Content-Length

```http
Content-Length: 1250
```

Indicates the length of the response body in bytes for applicable HTTP messages.

---

## Location

Used with redirects.

```http
Location: /login
```

---

## Set-Cookie

Used by a server to ask the browser to store a cookie.

```http
Set-Cookie: session=abc123; Secure; HttpOnly
```

---

# 🔐 Important Cookie Security Attributes

### Secure

```http
Secure
```

The browser should only send the cookie over a secure connection.

---

### HttpOnly

```http
HttpOnly
```

Prevents normal JavaScript access to the cookie through APIs such as `document.cookie`.

This can reduce exposure of session cookies to certain client-side attacks such as cookie theft through XSS, but it does not prevent XSS itself.

---

### SameSite

Example:

```http
SameSite=Lax
```

Controls when browsers send cookies in cross-site contexts.

Common values:

```text
Strict
Lax
None
```

`SameSite` is an important defense related to cross-site request behavior and CSRF.

---

# 🛡️ Security Headers

Some HTTP response headers provide security-related browser instructions.

---

## Content-Security-Policy

```http
Content-Security-Policy: default-src 'self'
```

CSP can restrict where different types of resources may be loaded from and can help mitigate certain classes of XSS.

---

## Strict-Transport-Security

```http
Strict-Transport-Security: max-age=31536000
```

Also called **HSTS**.

It tells compatible browsers to use HTTPS for future connections to the host for the specified period.

---

## X-Content-Type-Options

```http
X-Content-Type-Options: nosniff
```

Helps prevent MIME-type sniffing in supported browsers.

---

## Referrer-Policy

```http
Referrer-Policy: strict-origin-when-cross-origin
```

Controls what referrer information browsers send with requests.

---

# 🔥 HTTP Request vs Response

| Feature          | Request         | Response        |
| ---------------- | --------------- | --------------- |
| Direction        | Client → Server | Server → Client |
| Method           | Usually present | Not present     |
| Status code      | No              | Yes             |
| Request headers  | Yes             | No              |
| Response headers | No              | Yes             |
| Body             | Optional        | Optional        |
| Cookies          | `Cookie`        | `Set-Cookie`    |

---

# 🧩 Complete Example

Suppose you visit:

```text
https://example.com/profile
```

The browser might send:

```http
GET /profile HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: session=abc123
```

The server might respond:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
Content-Security-Policy: default-src 'self'

<html>
    <body>
        <h1>Profile</h1>
    </body>
</html>
```

Now you can understand the complete communication:

```text
REQUEST
│
├── GET
├── /profile
├── Host
├── User-Agent
├── Accept
└── Cookie
       │
       ▼
    SERVER
       │
       ▼
RESPONSE
│
├── 200 OK
├── Content-Type
├── Set-Cookie
├── CSP
└── HTML body
```

---

# 🔐 HTTP & HTTPS

HTTP itself does not encrypt application data.

With HTTPS:

```text
HTTP
 +
TLS
 =
HTTPS
```

Conceptually:

```text
Browser
   │
   │ HTTPS
   ▼
┌─────────────┐
│ TLS Security│
├─────────────┤
│ HTTP        │
└─────────────┘
   │
   ▼
 Server
```

HTTPS helps provide:

```text
Confidentiality
Integrity
Server authentication
```

This connects directly to the cryptography module you already completed.

---

# 🕵️ HTTP From a Cybersecurity Perspective

Security testers pay close attention to HTTP because many web vulnerabilities involve manipulating requests or interpreting responses.

For example:

```text
Client
  │
  │ Request
  ▼
┌──────────────────┐
│ Security Tester  │
│                  │
│ Inspect/modify   │
│ request          │
└────────┬─────────┘
         │
         ▼
      Server
```

Tools such as **Burp Suite** can intercept and display HTTP traffic.

A request might look like:

```http
GET /profile?id=42 HTTP/1.1
Host: lab.example
Cookie: session=abc123
```

A tester can investigate things such as:

```text
Method
URL
Parameters
Headers
Cookies
Authentication
Authorization
Response
Status code
```

Only perform testing against systems you own or are explicitly authorized to test.

---

# 🔎 What to Look for During HTTP Analysis

When you capture a request, inspect it in this order:

### 1. Method

```text
GET?
POST?
PUT?
PATCH?
DELETE?
```

### 2. URL

```text
/path
?parameter=value
```

### 3. Headers

Look for:

```text
Host
Authorization
Cookie
Content-Type
Origin
Referer
```

### 4. Body

Check:

```text
Form data
JSON
XML
Uploaded data
```

### 5. Response

Check:

```text
Status code
Headers
Cookies
Body
Error messages
```

---

# 🧪 Hands-On Lab

## Task 1 — Inspect a Request

Open any website you are authorized to inspect.

Open:

```text
Browser
   ↓
Developer Tools
   ↓
Network
```

Reload the page.

Select one request.

Find:

```text
Request URL
Request Method
Status Code
Request Headers
Response Headers
Response Body
```

---

# 🧪 Task 2 — Create a Local HTTP Server

Since you're learning Python, create:

```python
from http.server import HTTPServer, SimpleHTTPRequestHandler

server = HTTPServer(("127.0.0.1", 8000), SimpleHTTPRequestHandler)

print("Server running on http://127.0.0.1:8000")

server.serve_forever()
```

Run:

```bash
python server.py
```

Then open:

```text
http://127.0.0.1:8000
```

Your browser sends a request to your own machine.

You can observe it using browser DevTools.

---

# 🧪 Task 3 — Use curl

Try:

```bash
curl -i http://127.0.0.1:8000
```

The `-i` option includes response headers.

You should see something similar to:

```http
HTTP/1.0 200 OK
Server: SimpleHTTP/...
Date: ...
Content-type: text/html
Content-Length: ...
```

Now you are seeing HTTP directly instead of only through a browser.

---

# 🧪 Task 4 — Inspect Headers

Try:

```bash
curl -I https://example.com
```

This requests headers without downloading the normal response body.

Look for:

```text
HTTP status
Content-Type
Server
Location
Cache-Control
Security headers
```

Use this only for normal public inspection; do not turn header inspection into unauthorized scanning.

---

# 🧪 Task 5 — Send a POST Request Locally

You can create a simple test endpoint later using Flask or another local framework.

Conceptually:

```http
POST /login HTTP/1.1
Host: 127.0.0.1:8000
Content-Type: application/json

{
    "username": "test",
    "password": "test123"
}
```

Observe:

```text
Method
URL
Headers
Content-Type
Body
Response
```

This will prepare you for **Burp Suite request interception**.

---

# 🛡️ HTTP Security Checklist

When analyzing an HTTP request/response:

* [ ] Identify the HTTP method.
* [ ] Identify the target URL/path.
* [ ] Inspect query parameters.
* [ ] Inspect request headers.
* [ ] Check cookies.
* [ ] Check authentication headers.
* [ ] Inspect the request body.
* [ ] Identify the status code.
* [ ] Inspect response headers.
* [ ] Check `Set-Cookie` attributes.
* [ ] Look for security headers.
* [ ] Inspect response content.
* [ ] Check whether sensitive data is transmitted over HTTPS.
* [ ] Never assume client-controlled headers are trustworthy.
* [ ] Never assume a hidden UI element means authorization exists.

---

# 🧠 Quick Revision

| Concept      | Meaning                                    |
| ------------ | ------------------------------------------ |
| HTTP         | Web communication protocol                 |
| Request      | Client → Server                            |
| Response     | Server → Client                            |
| Method       | Intended operation                         |
| GET          | Retrieve                                   |
| POST         | Submit/create/process                      |
| PUT          | Replace/create                             |
| PATCH        | Partial modification                       |
| DELETE       | Delete                                     |
| Status Code  | Result of request                          |
| 2xx          | Success                                    |
| 3xx          | Redirection                                |
| 4xx          | Client-side error                          |
| 5xx          | Server-side error                          |
| Header       | Metadata about request/response            |
| Cookie       | Client-side state sent with requests       |
| `Set-Cookie` | Server instructs browser to store a cookie |
| HTTPS        | HTTP protected by TLS                      |

---

# ⚡ Memory Tricks

### HTTP Communication

> **Request → Server → Response**

### Status Codes

```text
1 = Information
2 = Success
3 = Redirect
4 = Client Error
5 = Server Error
```

### Important Security Codes

```text
401 → Authentication issue
403 → Authorization denied
404 → Resource not found
429 → Too many requests
500 → Server-side failure
```

### HTTP Structure

```text
REQUEST
├── Request Line
├── Headers
├── Blank Line
└── Body

RESPONSE
├── Status Line
├── Headers
├── Blank Line
└── Body
```

---

# 💼 Interview Questions

### 1. What is HTTP?

HTTP is an application-layer protocol used for communication between clients and web servers.

### 2. What is the difference between a request and a response?

A request is sent by the client to the server, while a response is sent by the server back to the client.

### 3. What is an HTTP method?

An HTTP method indicates the intended operation for a request, such as retrieving, creating, modifying, or deleting a resource.

### 4. What is the difference between 401 and 403?

**401** generally indicates that authentication is required or not accepted.

**403** indicates that the server understood the request but refuses to authorize it.

### 5. What is a HTTP header?

A header is metadata associated with an HTTP request or response.

### 6. What is the difference between HTTP and HTTPS?

HTTPS is HTTP transmitted through TLS, providing protections such as confidentiality, integrity, and server authentication.

### 7. Is POST more secure than GET?

No. The HTTP method itself does not provide encryption. HTTPS/TLS provides transport protection.

### 8. What is `Content-Type`?

It describes the media type of the message body.

Example:

```http
Content-Type: application/json
```

### 9. What is `Set-Cookie`?

It is a response header used by a server to instruct a browser to store a cookie.

### 10. Why are HTTP headers important in cybersecurity?

Headers can contain authentication information, cookies, content metadata, caching directives, and security policies, making them important during web security analysis.

---

# 🔥 Final Takeaway

HTTP is the foundation of web communication.

Understand this flow:

```text
                  HTTP
                   │
        ┌──────────┴──────────┐
        │                     │
     REQUEST               RESPONSE
        │                     │
   ┌────┴────┐           ┌────┴────┐
   │ Method  │           │ Status  │
   │ URL     │           │ Headers │
   │ Headers │           │ Body    │
   │ Body    │           └─────────┘
   └─────────┘
```

For cybersecurity, don't just memorize:

```text
GET
POST
200
404
Cookie
```

Learn to **read the entire HTTP conversation**.

Once you can look at:

```http
POST /login HTTP/1.1
Host: lab.local
Content-Type: application/json
Cookie: session=abc123

{"username":"test","password":"test123"}
```

and immediately understand what each part means, you have the foundation needed for:

```text
HTTP
  ↓
Web Applications
  ↓
Burp Suite
  ↓
Authentication Testing
  ↓
Session Security
  ↓
Input Validation
  ↓
Web Vulnerability Testing
```

> **HTTP is the conversation. Web security testing is learning how to understand, inspect, and safely test that conversation.**

This gives you the HTTP foundation you'll need before moving deeper into **web requests, cookies, sessions, authentication, APIs, and eventually Burp Suite**.
