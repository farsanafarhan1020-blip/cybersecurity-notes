# 🌐 Understanding Denial of Service (DoS) Attacks

> **"A Denial of Service attack aims to make a system, website, or network unavailable to legitimate users by overwhelming it with traffic or resource requests."**

---

# 💥 What is a Denial of Service (DoS) Attack?

A **Denial of Service (DoS)** attack is a cyberattack where a **single attacker or system** floods a target with excessive traffic or malicious requests, preventing legitimate users from accessing the service.

Instead of stealing data, the attack focuses on **disrupting availability**, one of the three pillars of the **CIA Triad**.

---

## ⚙️ How a DoS Attack Works

```text
          Attacker
              │
              ▼
     Sends Massive Requests
              │
              ▼
        Target Server
              │
              ▼
 Resources Become Exhausted
              │
              ▼
 Legitimate Users Can't Access
```

---

## 🎯 Common Targets

- 🌐 Websites
- 🎮 Gaming servers
- ☁️ Cloud services
- 💳 Banking applications
- 📧 Email servers
- 📡 DNS servers

---

## 🛠 Common Types of DoS Attacks

### 🌊 Flood Attack

Floods the server with huge amounts of traffic.

Examples:

- UDP Flood
- ICMP (Ping) Flood

---

### 🤝 SYN Flood

Exploits the TCP three-way handshake by sending many SYN requests without completing the connection.

```text
Attacker ── SYN ──► Server

Server ── SYN-ACK ──► Attacker

(No ACK Returned)

Server Waits...
Resources Get Consumed
```

---

### 📦 Application Layer Attack

Targets specific applications instead of the network.

Example:

- HTTP GET Flood

Even a small number of requests can overload the web application.

---

## 🌍 Real-World Example

A student launches a DoS attack against a school website from a single computer during online exams.

The website becomes unavailable until the attack stops.

---

# 🌐 Distributed Denial of Service (DDoS)

## 📖 What is a DDoS Attack?

A **Distributed Denial of Service (DDoS)** attack is similar to a DoS attack, but instead of using **one computer**, it uses **thousands or even millions of compromised devices**.

This makes DDoS attacks much larger and harder to stop.

---

## ⚙️ How a DDoS Attack Works

```text
      Compromised Devices
     (Botnet Computers)
      /    |     |     \
     ▼     ▼     ▼      ▼
 Massive Traffic Sent Together
              │
              ▼
        Target Server
              │
              ▼
 Server Overloaded & Crashes
```

---

## Why is DDoS More Dangerous?

✅ Traffic comes from many locations.

✅ Hard to identify real users.

✅ Can generate enormous amounts of traffic.

✅ Difficult to block without affecting legitimate users.

---

## 🌍 Real-World Example

The **Mirai Botnet** launched one of the largest DDoS attacks in 2016 by infecting thousands of insecure IoT devices like CCTV cameras and routers, causing major websites to become inaccessible.

---

# 🤖 Botnets

## 📖 What is a Botnet?

A **Botnet** is a network of infected computers or IoT devices controlled remotely by an attacker (called a **Botmaster**).

Each infected device is known as a **Bot** or **Zombie**.

---

## ⚙️ How Botnets Work

```text
        Botmaster
            │
            ▼
   Command & Control (C2)
            │
     ┌──────┼──────┐
     ▼      ▼      ▼
   Bot 1  Bot 2  Bot 3
            │
            ▼
      Launch DDoS Attack
```

---

## Common Devices in Botnets

- 💻 Computers
- 📱 Smartphones
- 📷 CCTV Cameras
- 🌐 Routers
- 🏠 Smart Home Devices

---

## Uses of Botnets

- DDoS attacks
- Spam campaigns
- Malware distribution
- Credential attacks
- Cryptocurrency mining

---

# 💥 Attack Impact

A successful DoS or DDoS attack can cause serious damage.

---

## Business Impact

💸 Financial losses

🛑 Service downtime

👥 Loss of customers

📉 Reputation damage

⚖️ Compliance issues

---

## Technical Impact

- High CPU usage
- Memory exhaustion
- Network congestion
- Server crashes
- Slow response times

---

## 🌍 Example

During an online shopping sale, attackers launch a DDoS attack.

Customers cannot access the website.

Orders are lost, and the company suffers significant financial damage.

---

# 🛡️ Mitigation Approaches

## 📖 What is Mitigation?

Mitigation means reducing or preventing the impact of an attack.

No single solution can stop every DDoS attack, so organisations use multiple layers of defence.

---

## Common Mitigation Techniques

### 🔥 Firewall

Filters unwanted traffic before it reaches the server.

---

### 🚦 Rate Limiting

Limits the number of requests a user can send within a certain time.

---

### 🌍 Content Delivery Network (CDN)

Distributes traffic across multiple servers, reducing the load on the main server.

Examples:

- Cloudflare
- Akamai

---

### 🤖 DDoS Protection Services

Specialised services detect and filter malicious traffic before it reaches the target.

---

### 📊 Traffic Monitoring

Continuously monitors network traffic to detect unusual spikes.

---

### 🔄 Load Balancing

Distributes incoming traffic across multiple servers.

```text
Users
   │
   ▼
Load Balancer
   │
 ┌─┴───────────┐
 ▼             ▼
Server 1    Server 2
```

---

# ⚖️ DoS vs DDoS

| Feature | DoS | DDoS |
|----------|-----|-------|
| Number of Attackers | One | Thousands |
| Traffic Volume | Lower | Very High |
| Difficulty to Stop | Easier | Harder |
| Uses Botnet | ❌ | ✅ |
| Attack Source | Single Device | Multiple Devices |

---

# 🧠 Memory Trick

Think of a restaurant.

🍽️ **DoS Attack**

One person repeatedly enters the restaurant and occupies every table.

Customers cannot get a seat.

---

🌍 **DDoS Attack**

Thousands of fake customers arrive at once.

The restaurant becomes completely overwhelmed.

---

# 📝 Quick Revision

### 💥 DoS Attack

- Single attacking system
- Overloads server resources
- Makes services unavailable

### 🌐 DDoS Attack

- Multiple attacking systems
- Usually uses botnets
- Larger and harder to stop

### 🤖 Botnet

- Network of infected devices
- Controlled by a Botmaster
- Used for DDoS and other attacks

### 💥 Attack Impact

- Downtime
- Financial loss
- Reputation damage
- Poor user experience

### 🛡️ Mitigation

- Firewalls
- Rate limiting
- CDN
- DDoS protection
- Traffic monitoring
- Load balancing

---

# 💡 Interview Tip

### **What is the difference between a DoS and a DDoS attack?**

| DoS | DDoS |
|------|-------|
| One attacker | Multiple attackers |
| Lower traffic | Massive traffic |
| Easier to detect | Harder to detect |
| No botnet required | Usually uses botnets |

> **Remember:**
>
> - 💥 **DoS = One attacker blocks the service.**
> - 🌐 **DDoS = Thousands of compromised devices overwhelm the service together.**
> - 🤖 **Botnets are the backbone of most modern DDoS attacks.**
````
