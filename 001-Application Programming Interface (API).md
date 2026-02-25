# 🌐 Application Programming Interface (API)

![API Work](./images/image.png)

### 📘 What is an API?

An **API (Application Programming Interface)** is a structured interface that allows different software systems to communicate with each other through predefined rules and protocols.

APIs abstract internal logic and expose only necessary functionality to external systems.

> 💡 **Think of an API like a restaurant menu** — it lists what you can request without exposing how the kitchen prepares it.


### 🍽️ Real-World Analogy

- 👤 **Client** → You placing an order  
- 📋 **API** → The menu (interface)  
- 🧑‍🍳 **Server** → The kitchen (processes request and returns result)  

---

## 🧩 Core Components of an API

### 1️⃣ Endpoint

An endpoint is a specific URL representing a resource or action.

### 🧪 Example:

```http
GET https://api.example.com/users/123
```

#### 🔎 Breakdown:

* `/users` → Resource
* `/123` → Specific object

---

## 🌍 2. HTTP Methods

| 🛠 Method | 🎯 Purpose              | 📌 Example       |
|-----------|------------------------|------------------|
| GET       | Retrieve data          | `/products`      |
| POST      | Create new resource    | `/users`         |
| PUT       | Update resource        | `/users/123`     |
| PATCH     | Partial update         | `/users/123`     |
| DELETE    | Remove resource        | `/users/123`     |

---

## 📤 3. Request Structure

A client request may contain:

- 🧾 **Headers** (Authorization, Content-Type)  
- 🔎 **Query Parameters**  
- 📦 **Request Body**  
- 🍪 **Cookies**  


### 🧪 Example:

```bash
curl -X POST https://api.example.com/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"1234"}'
```

---

## 📥 4. Response Structure

A server response includes:

* 🔢 **Status Code**
* 🧾 **Headers**
* 📦 **Body (JSON/XML/etc.)**

### 📦 Example Response Body

```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

---

## 🔢 5. HTTP Status Codes

### 📊 HTTP Status Code Categories

[HTTP status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status) are grouped into five main categories:

### ℹ️ 1️⃣ Informational Responses (100 – 199)

These indicate that the request was received and the process is continuing.

- **100** – Continue  
- **101** – Switching Protocols  
- **102** – Processing  

---

### ✅ 2️⃣ Successful Responses (200 – 299)

These indicate that the request was successfully received, understood, and processed.

- **200** – OK  
- **201** – Created  
- **202** – Accepted  
- **204** – No Content  

---

### 🔁 3️⃣ Redirection Messages (300 – 399)

These indicate that further action needs to be taken to complete the request.

- **301** – Moved Permanently  
- **302** – Found  
- **304** – Not Modified  
- **307** – Temporary Redirect  

---

### ❌ 4️⃣ Client Error Responses (400 – 499)

These indicate that there was an issue with the request sent by the client.

- **400** – Bad Request  
- **401** – Unauthorized  
- **403** – Forbidden  
- **404** – Not Found  
- **429** – Too Many Requests  

---

### 💥 5️⃣ Server Error Responses (500 – 599)

These indicate that the server failed to fulfill a valid request.

- **500** – Internal Server Error  
- **502** – Bad Gateway  
- **503** – Service Unavailable  
- **504** – Gateway Timeout  

---

| 🔢 Code | 📖 Meaning            |
| ------- | --------------------- |
| 200     | Success               |
| 201     | Created               |
| 204     | No Content            |
| 400     | Bad Request           |
| 401     | Unauthorized          |
| 403     | Forbidden             |
| 404     | Not Found             |
| 429     | Too Many Requests     |
| 500     | Internal Server Error |


<hr style="border: 1px solid red;">

# 🌍 Common API Use Cases

## 🌐 1️⃣ Web Applications

- 🔗 Fetch external services (Maps, Payments, Authentication)  
- ⚛️ SPA frameworks using REST APIs  
- 🔄 AJAX-based dynamic content  

---

## 📱 2️⃣ Mobile Applications

- 🔐 User login  
- 🔔 Push notifications  
- 👤 Profile retrieval  

---

## 🏗 3️⃣ Microservices Architecture

- 🔄 Internal service-to-service communication  
- 📡 Event-driven workflows  

---

## 🌐 4️⃣ IoT Systems

- 🏠 Smart home device communication  
- 📊 Remote monitoring systems  

---

### ⭐ Why APIs are Important

| 🎯 Benefit          | 📝 Description                                   |
|--------------------|--------------------------------------------------|
| 🧱 Modularity       | Break large systems into independent services    |
| ♻️ Code Reuse       | Avoid rebuilding core functionality              |
| 📈 Scalability      | Independent scaling of backend services          |
| 🔗 Integration      | Connect CRM, ERP, payment gateways               |
| 🔐 Security         | Controlled access via tokens and scopes          |
| 🌍 Ecosystem Growth | Enable third-party developers                    |

---

### 🔐 API Authentication Mechanisms

| 🔑 Method   | 📖 Description                     | 🛡 Security Level |
|-------------|-----------------------------------|------------------|
| API Key     | Static key in header or URL       | Low–Medium       |
| Basic Auth  | Base64 `username:password`        | Low              |
| OAuth2      | Token-based with scopes           | High             |
| JWT         | Signed JSON token                 | High             |
| mTLS        | Mutual TLS authentication         | Very High        |

---

### 🧪 Example Authorization Header

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

---

## ⚔️ API-Based Applications vs Traditional Web Applications

| 🧩 Aspect      | 🌐 API-Based                          | 🖥 Traditional                     |
|---------------|----------------------------------------|------------------------------------|
| 🎨 Rendering   | Client-side (React, Vue, Angular)     | Server-side (PHP, JSP)            |
| 📦 Data Format | JSON                                  | HTML                               |
| 🏗 Architecture| Decoupled                             | Monolithic                         |
| 📈 Scalability | High                                  | Moderate                           |
| 🔐 State       | Token-based                           | Session-based                      |

---

## 🔌 API Protocol Types

### 🌍 REST

- 🧭 Stateless  
- 📦 Resource-based  
- 🔁 Uses HTTP verbs  
- 🧾 Typically JSON  


### 🧱 SOAP

- 📄 XML-based  
- 📜 Strict schema (WSDL)  
- 🏢 Common in enterprise systems  


### 🔷 GraphQL

- 🎯 Client defines requested fields  
- 🔗 Single endpoint  
- 🚫 Prevents over-fetching  
- ⚠️ Can introduce authorization complexity  

<hr style="border: 1px solid red;">

## 🔐 API Security Considerations

Since you are working in penetration testing, here are key API-specific risks:

### 1️⃣ Broken Object Level Authorization (BOLA)

- 🔓 Accessing other users’ resources by modifying IDs  

### 🧪 Example:

```http
GET /api/v1/users/43
```

> ⚠️ Changing the ID may allow unauthorized access (IDOR/BOLA).

---

### 2️⃣ Broken Authentication

* ❌ Weak JWT validation
* ⚠️ Accepting `alg: none`
* ⏳ Long-lived tokens

---

### 3️⃣ Mass Assignment

* 📦 Overwriting internal fields via JSON body

> 💡 Example: Modifying `role`, `isAdmin`, or internal flags in request body.

---

### 4️⃣ Rate Limiting Issues

* 🔁 Brute-force attacks
* 🔍 Enumeration attacks

---

### 5️⃣ Excessive Data Exposure

* 📤 Returning sensitive internal fields in JSON

> ⚠️ Example: `passwordHash`, internal notes, system flags.

---

### 6️⃣ Improper CORS Configuration

* 🌍 Allowing arbitrary origins

> ⚠️ Misconfigured `Access-Control-Allow-Origin` can expose APIs to cross-origin attacks.

---
### 7️⃣ Injection in JSON Parameters

- 💉 SQL Injection  
- 🗄 NoSQL Injection  
- 💻 Command Injection  

> ⚠️ Always test JSON body parameters, nested objects, and data types for injection vulnerabilities.

---

### 🛠 Tools for API Testing (Security Perspective)

- 🐝 **Burp Suite**  
- 📬 **Postman**  
- 🖥 **curl**  
- 🚀 **ffuf**  
- 🔎 **mitmproxy**  
- 🔐 **JWT Editor**  
- 📘 **Swagger UI** (for API discovery)  

---

💡 **Pro Tip for Pentesters:**

- Use **Burp** for interception and manipulation  
- Use **ffuf** for endpoint and parameter fuzzing  
- Use **JWT Editor** to test token tampering  
- Use **Swagger/OpenAPI** for attack surface mapping  

