# 🚦 Rate Limiting

## 🚦 Rate Limiting

**Rate limiting** is a technique used to control the number of requests a client can send to an API within a specified time window. It helps protect the system from abuse, excessive traffic, and automated attacks such as **brute-force attempts** or **denial-of-service (DoS) attacks**.

By restricting request frequency, rate limiting ensures **API availability, stability, and fair usage among clients**.

---

## ❓ Why Rate Limiting is Important

### 1️⃣ Prevents DoS/DDoS Attacks
Without proper rate limiting, attackers may flood an API with **thousands of requests per second**, exhausting server resources and causing downtime.

### 2️⃣ Protects Backend Resources
APIs often interact with **databases, file systems, or external services**. Excessive requests can overload these resources.

### 3️⃣ Ensures Fair Usage
Rate limiting prevents **one client from consuming all available API resources**, ensuring fair access for all users.

### 4️⃣ Improves System Stability
Controlled request flow ensures the API remains **responsive and available to legitimate users**.

### 5️⃣ Mitigates Brute Force Attacks
Login endpoints and sensitive operations become safer when **repeated attempts are restricted**.

---

## ⚙️ How Rate Limiting Works

Rate limiting is typically implemented based on:

## 1️⃣ Request Threshold

Maximum number of requests allowed.

### Example

- **100 requests per minute**
- **10 requests per second**

---

## 2️⃣ Time Window

The duration over which requests are counted.

### Examples

- Per second  
- Per minute  
- Per hour  
- Per day  

---

## 3️⃣ Identification Mechanism

Rate limiting may be applied based on:

- 🌐 **IP Address**
- 🔑 **API Key**
- 👤 **User Account**
- 🎫 **Session Token**
- 🪪 **JWT Token**

---

## 🌐 Example API Endpoint

```
POST /workshop/api/merchant/contact_mechanic HTTP/1.1
Host: 192.168.1.28:8888
```
---

## 📦 Request Body

```json
{
  "mechanic_code": "TRAC_JHN",
  "problem_details": "test",
  "vin": "4CVZR19NKXF615223",
  "mechanic_api": "http://192.168.1.28:8888/workshop/api/mechanic/receive_report",
  "repeat_request_if_failed": true,
  "number_of_repeats": 100
}
```

---

## ⚠️ Security Risk Identified

The parameter:

```
number_of_repeats: 100
```

may allow the client to **trigger multiple backend requests automatically**, potentially bypassing **server-side rate limiting**.

If the backend processes this parameter directly, an attacker could:

* 💥 Generate **massive internal requests**
* ⚙️ Trigger **resource exhaustion**
* 🌊 Cause **internal service flooding**

---

## 🧪 Rate Limiting Testing Methodology

During API security testing, the following steps are commonly performed:

### 1️⃣ Automated Request Flooding

Send multiple requests in a **short time period** to determine whether the API enforces proper rate limiting controls.

## 🛠️ Example Using Tools

Common tools used for **rate limiting testing** include:

- 🐝 **Burp Suite Intruder**
- ⚡ **Turbo Intruder**
- 🎯 **ffuf**
- 🐍 **Python scripts**

### 🎯 Goal

Check whether the **API blocks excessive requests** or allows unlimited requests without restrictions.

---

## 👀 2️⃣ Observe Server Response

Indicators of rate limiting include responses such as:

```
HTTP 429 Too Many Requests
```

### 📡 Or Response Headers Like

```
X-RateLimit-Limit
X-RateLimit-Remaining
Retry-After
```
---

## 🔄 3️⃣ Token Rotation Testing

Test whether rate limits are applied per:

- 🌐 **IP address**
- 🪪 **JWT token**
- 👤 **User account**

Weak implementations may allow bypass using:

- 🔑 **Multiple tokens**
- 🔁 **Multiple sessions**

---

## ⚙️ 4️⃣ Parameter Abuse Testing

Test parameters such as:

```
number_of_repeats
repeat_request_if_failed
```

to check if they trigger **backend request loops** or automated internal calls.

---

## 🚨 Impact

If rate limiting is **missing or weak**, attackers may:

- ⚠️ Perform **API abuse**
- 🔓 Conduct **brute force attacks**
- 💥 Trigger **DoS conditions**
- ⚙️ Exhaust **backend services**
- 📡 Generate **excessive internal API calls**

---

## 📉 Potential Consequences

This can lead to:

- ⚠️ **Service degradation**
- 🔌 **API downtime**
- 💰 **Increased infrastructure costs**

---

# 🛡️ Recommended Mitigations

## 1️⃣ Implement Server-Side Rate Limiting

### Example limits

- **10 requests per second**
- **100 requests per minute per user**

Implement rate limiting **on the server side** rather than relying on client behavior.

---

## 2️⃣ Enforce Global and Per-User Limits

Rate limits should apply to:

- 🌐 **IP address**
- 👤 **User account**
- 🔑 **API key**

This ensures attackers cannot bypass limits using **multiple accounts or tokens**.

---

## 3️⃣ Ignore Client-Controlled Loop Parameters

Do not allow client parameters such as:

```
number_of_repeats
repeat_request_if_failed
```

to control **backend request behavior**, as they can be abused to generate excessive internal requests.

---

## 4️⃣ Implement Request Throttling

When a client exceeds the allowed request limit, the server should return a response such as:

```
HTTP/1.1 429 Too Many Requests
```

### Example Response Header

```
Retry-After: 60
```

This informs the client to **wait 60 seconds before sending another request**.

---

## 5️⃣ Use API Gateways or WAF

Examples include:

- 🌐 **API gateways**
- 🔁 **Reverse proxies**
- 🛡️ **Web Application Firewalls (WAF)**

These solutions can **enforce rate limits before requests reach the backend**, providing an additional layer of protection.

---

## 📌 Conclusion

Rate limiting is a **critical security control** for protecting APIs from abuse, brute-force attacks, and denial-of-service conditions.

Proper implementation ensures:

- ⚖️ **Fair resource usage**
- ⚙️ **System stability**
- 🔐 **Protection against automated attacks**
- 🌐 **Reliable API availability**
```
