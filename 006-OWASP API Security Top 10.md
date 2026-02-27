# 🛡 OWASP API Security Top 10

![alt text](./images/OWASP%20API%20security.png)

The `OWASP API Security Top 10` is published by **OWASP (Open Web Application Security Project)**.
It highlights the most critical security risks affecting APIs.

The latest stable release is:

*  [**OWASP API Security Top 10 (2023)**](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

This list focuses specifically on **API-related risks** (not traditional web application vulnerabilities).

---

## 🚨 OWASP API Security Top 10 (2023)

### 🔓 API1: Broken Object Level Authorization (BOLA)

#### 📌 What It Is

APIs expose objects (users, orders, accounts).
If the API does **not properly verify ownership**, attackers can access other users' data by manipulating object identifiers.

#### 💻 Example

```http
GET /api/users/124
```

If user `123` can access user `124`’s data by changing the ID → **BOLA vulnerability**.


#### 💥 Impact

* Data leakage
* Account takeover
* Financial fraud

#### 🛡 Prevention for BOLA (API1)

* Always validate ownership **server-side**
* Do not rely on client-side checks
* Use proper authorization middleware

---

## 🔐 API2: Broken Authentication

### 📌 What It Is

Authentication mechanisms are implemented incorrectly, allowing attackers to compromise tokens or credentials.

### ⚠️ Common Issues

* Weak JWT secrets
* Missing token expiration
* Brute force without rate limiting
* Accepting unsigned JWT (`alg: none`)


### 💥 Impact

* Account takeover
* Privilege escalation

### 🛡 Prevention

* Use strong token secrets
* Enforce expiration
* Implement MFA (Multi-Factor Authentication)
* Rate limit login endpoints

---

## 🔓 API3: Broken Object Property Level Authorization

### 📌 What It Is

Also called **Mass Assignment vulnerability**.

APIs allow modification of properties that should **not** be user-controlled.

### 💻 Example

```json
{
  "username": "john",
  "role": "admin"
}
```

If the `role` field can be changed by the user → ⬆️ **Privilege Escalation**


### 💥 Impact

* Privilege escalation
* Data corruption

### 🛡 Prevention

* Whitelist allowed fields
* Use DTOs (Data Transfer Objects)
* Do not auto-bind entire objects

---

## 🚨 API4: Unrestricted Resource Consumption

### 📌 What It Is

APIs do not limit resource usage, allowing abuse.


### 🔎 Examples

* No rate limiting
* Large payload uploads
* Expensive queries
* Infinite pagination

### 💥 Impact

* Denial of Service (DoS)
* Increased infrastructure cost

### 🛡 Prevention

* Implement rate limiting
* Apply throttling
* Enforce payload size limits
* Apply query depth limiting (GraphQL)

---

## 🔐 API5: Broken Function Level Authorization

### 📌 What It Is

Improper authorization checks on **sensitive functions**.

### 💻 Example

```http id="gj2l3m"
POST /api/admin/deleteUser
```

If regular users can access **admin endpoints** → 🚨 Broken Function-Level Authorization


### 💥 Impact

* Full system compromise
* Data destruction

### 🛡 Prevention

* Implement Role-Based Access Control (RBAC)
* Verify roles **server-side**
* Separate admin routes properly

---
