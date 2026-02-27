# 🧪 API Penetration Testing – Labs Setup

Below are intentionally vulnerable API applications designed for hands-on security testing practice.

These labs cover:

* 🌐 REST
* 🧩 GraphQL
* 🔐 JWT
* 🛡 OAuth
* 🔎 IDOR
* 🚨 SSRF
* 🔓 BOLA
* And more…

---

# 🏁 OWASP – crAPI

🔗 [https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)

**crAPI (Completely Ridiculous API)** is one of the most comprehensive modern API security labs.

## 🎯 Focus Areas

* OWASP API Security Top 10
* BOLA (Broken Object Level Authorization)
* Broken Authentication
* Mass Assignment
* JWT issues
* SSRF
* Rate limiting flaws

---

## 🛠 Setup

```bash
git clone https://github.com/OWASP/crAPI.git
```
```bash
cd crAPI
```
```bash
docker-compose up
```

🐳 Runs via Docker.

Includes both **Web UI + API backend**.

---

# 2️⃣ vAPI

🔗 [https://github.com/rootusk/vapi](https://github.com/rootusk/vapi)

A lightweight vulnerable **REST API** for beginners.

---

## 🎯 Focus Areas

* SQL Injection
* Authentication bypass
* IDOR
* Improper input validation

---

## 🛠 Setup

```bash
git clone https://github.com/rootusk/vapi.git
```
```bash
cd vapi
```
```bash
docker-compose up
```

🐳 Runs using Docker.

---

# 3️⃣ VAmPI (Vulnerable API)

🔗 [https://github.com/erev0s/VAmPI](https://github.com/erev0s/VAmPI)

A REST API built in **Flask** with deliberate vulnerabilities.

---

## 🎯 Focus Areas

* JWT weaknesses
* BOLA (Broken Object Level Authorization)
* Mass assignment
* Improper rate limiting
* Broken access control

---

## 🛠 Setup

```bash
git clone https://github.com/erev0s/VAmPI.git
```
```bash
cd VAmPI
```
```bash
docker-compose up
```

🐳 Runs using Docker.

---

# 4️⃣ Damn Vulnerable Bank

🔗 [https://github.com/rewanthtammana/Damn-Vulnerable-Bank](https://github.com/rewanthtammana/Damn-Vulnerable-Bank)

🔗 Backend: [https://github.com/rewanthtammana/Damn-Vulnerable-Bank/tree/master/BackendServer](https://github.com/rewanthtammana/Damn-Vulnerable-Bank/tree/master/BackendServer)

A full **banking simulation** with both frontend and backend components.


## 🎯 Focus Areas

* Business logic flaws
* Transaction manipulation
* IDOR
* Authentication bypass
* Privilege escalation

---

## 🛠 Setup

### 🔧 Backend

```bash
cd BackendServer
```

```bash
npm install
```

```bash
npm start
```

### 🎨 Frontend

```bash
cd Frontend
```
```bash
npm install
```
```bash
npm start
```

💡 This lab is great for practicing:

* Advanced API logic attacks
* Banking workflow exploitation
* Authorization bypass testing
* Business logic abuse scenarios

---

# 5️⃣ Damn Vulnerable GraphQL Application

🔗 [https://github.com/dolevf/Damn-Vulnerable-GraphQL-Application](https://github.com/dolevf/Damn-Vulnerable-GraphQL-Application)

A **GraphQL-based intentionally vulnerable lab**.

---

## 🎯 Focus Areas

* GraphQL introspection abuse
* Authorization bypass
* Query batching attacks
* IDOR in GraphQL
* Injection attacks

---

## 🛠 Setup

```bash id="gql7xp"
docker run -p 5013:5013 dolevf/dvga
```

🐳 Runs via Docker.

---

## 🧪 Recommended Lab Environment

Since you're working in a penetration testing setup (Kali, Burp, recon workflow), I recommend:

### 💻 Environment

* Kali Linux (Attacker machine)
* Docker installed
* Burp Suite
* Postman / Insomnia
* ffuf / wfuzz
* sqlmap
* jwt_tool
* mitmproxy (Optional)

## 🌐 Network Setup

* 🖧 Use **Host-only** or **NAT** networking in VirtualBox / VMware
* 🚧 Keep labs isolated from production networks

---

## 🎯 Suggested Learning Path (Progressive Difficulty)

1️⃣ **VAmPI** → Basic REST vulnerabilities

2️⃣ **vAPI** → Injection practice

3️⃣ **crAPI** → Advanced OWASP API Top 10

4️⃣ **Damn Vulnerable Bank** → Business Logic attacks

5️⃣ **DVGA (Damn Vulnerable GraphQL App)** → GraphQL-specific attacks

---

## 🚀 Next Steps

If you'd like, you can:

* 🔟 Map these labs to **OWASP API Security Top 10 (2023)**
* 📅 Create a structured **30-day API pentesting roadmap**
* 🕵️ Build a **Burp Suite testing checklist for APIs**
* 🐳 Create a Docker Compose automation script for all labs
* 📝 Build a professional reporting template for API findings

---
