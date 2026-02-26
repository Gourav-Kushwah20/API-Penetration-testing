# 🌐 Types of APIs

APIs can be classified in multiple ways depending on:

- 🏗 **Access method** (architecture / communication style)  
- 🔓 **Exposure level** (who can access them)  
- 🌍 **Usage context** (where they are used)  
- 🔄 **Communication pattern** (synchronous vs asynchronous)  

---

## 🏗 1️⃣ Types of APIs Based on Access Method (Architecture Style)

## 🌍 1.1 REST API (Representational State Transfer)

REST is an architectural style that uses HTTP methods to operate on resources.

### 🔑 Core Principles

- 📭 Stateless communication  
- 📦 Resource-based URLs  
- 🔁 Standard HTTP methods  
- 🧩 Uniform interface  
- 💾 Cacheable responses  


#### 🧪 Example

```http
GET    /api/users/101
POST   /api/users
PUT    /api/users/101
DELETE /api/users/101
```

### 📦 Data Format

- 🧾 Usually **JSON**  
- 🔄 Can also use **XML, HTML, or other formats**  

### ⭐ Advantages

- 🪶 Lightweight  
- ⚡ Easy to implement  
- 📈 Highly scalable  
- 🌐 Works naturally with web infrastructure  


### 🌍 Common Use Cases

- 🌐 Web applications  
- 📱 Mobile backends  
- 🏗 Microservices  
- 🔓 Public APIs  

---
## 🧱 1.2 SOAP API (Simple Object Access Protocol)

SOAP is a protocol that defines strict rules for message structure and communication.


### 🔑 Core Characteristics

- 📄 XML-based messaging  
- 📜 Uses WSDL (Web Services Description Language)  
- 🌐 Supports multiple transport protocols (HTTP, SMTP, TCP)  
- 🔐 Built-in security extensions (WS-Security)  
- 🏦 Supports ACID transactions  


#### 🧪 Example SOAP Request

```xml
<soap:Envelope>
  <soap:Body>
    <GetUser>
      <UserId>101</UserId>
    </GetUser>
  </soap:Body>
</soap:Envelope>
```



### ⭐ Advantages

* 🏗 Strong standardization
* 🔐 Enterprise-level security
* 📦 Reliable message delivery


### 🌍 Common Use Cases

* 🏦 Banking systems
* 🏢 Enterprise systems
* 🏛 Government services

---

## 🔷 1.3 GraphQL API

GraphQL is a query language for APIs that allows clients to request exactly the data they need.

### 🔑 Core Characteristics

- 🔗 Single endpoint (e.g., `/graphql`)  
- 🎯 Client defines response structure  
- 🧩 Strongly typed schema  
- 🔄 Supports queries, mutations, and subscriptions  



### 🧪 Example Query

```graphql
query {
  user(id: 101) {
    name
    email
  }
}
```

### ⭐ Advantages

* 🚫 Prevents over-fetching
* 📉 Prevents under-fetching
* ⚡ Efficient data retrieval
* 🎨 Flexible for frontend development

💡 **Security Insight (Pentesting Perspective):**

When testing GraphQL APIs, focus on:

* 🔍 Introspection exposure
* 📊 Query depth abuse
* 🔁 Query batching attacks
* 🔐 Authorization on nested objects

### 🌍 Common Use Cases (GraphQL)

- 🧩 Complex frontend applications  
- 📱 Mobile apps  
- 🔗 Applications with multiple related data models  

---

## 🚀 1.4 gRPC API
gRPC is a high-performance RPC framework developed by Google.

### 🔑 Core Characteristics

- 🌐 Uses HTTP/2  
- 📦 Binary serialization via Protocol Buffers  
- 🧩 Strong typing  
- 🔄 Supports streaming (client, server, bidirectional)  


### ⭐ Advantages

- ⚡ High performance  
- ⏱ Low latency  
- 🏗 Efficient for microservices  



### 🌍 Common Use Cases

- 🔄 Internal microservice communication  
- 📊 High-throughput systems  

---

## 🔓 2️⃣ Types of APIs Based on Exposure Level

## 🌍 2.1 Public APIs (Open APIs)

- 🌐 Available to external developers  
- 🔑 Often require API keys or OAuth  

#### 🧪 Examples:
- 💳 Payment gateways  
- 📱 Social media APIs  

## 🏢 2.2 Private APIs (Internal APIs)

- 🏭 Used within an organization  
- 🚫 Not exposed to external users  

#### 📍 Common In:
- 🏗 Microservices architecture  
- 📊 Internal dashboards  

## 🤝 2.3 Partner APIs

- 🔗 Shared with specific business partners  
- 📜 Controlled access via contracts and agreements  

#### 🧪 Examples:
- 🚚 Logistics integration  
- 🏦 Banking integrations  

💡 **Security Insight:**

- Public APIs → Higher external attack surface  
- Private APIs → Risk of internal trust abuse  
- Partner APIs → Misconfigured access can expose sensitive data  

---

## 🧩 3️⃣ Types of APIs Based on Usage Context

APIs can also be classified based on **where and how they are used**.


## 🌐 3.1 Web APIs (HTTP APIs)

- 🌍 Communicate over HTTP/HTTPS  
- 📱 Used in web and mobile applications  
- 🚀 Most common modern API type  

#### 📌 Examples:
- REST APIs  
- GraphQL APIs  
- Public & private backend APIs  

💡 These power:
- SPAs (React, Vue, Angular)
- Mobile backends
- SaaS platforms

---

## 🖥 3.2 Operating System APIs

Allow applications to interact with **OS-level services**.

#### 🔧 What They Control:
- 📁 File system access  
- 🧠 Memory management  
- ⚙️ Process control  
- 🖨 Device interaction  

#### 🧪 Examples Include:
- 🪟 Windows API  
- 🐧 POSIX system calls  

💡 These are critical for:
- System programming  
- Desktop applications  
- Low-level utilities  

---

## 📚 3.3 Library / Framework APIs

Used inside programming environments to provide **reusable functionality**.

#### 🛠 Examples:
- 🗄 Database connectors  
- 🔐 Authentication libraries  
- 📝 Logging frameworks  
- 🌐 HTTP client libraries  

#### Example in Python:

```python
import os
os.listdir()
```

💡 These help developers:
- Avoid rewriting common functionality  
- Improve modularity  
- Speed up development  

---

## 🔄 4️⃣ Communication Patterns in APIs

APIs can also be classified based on **how communication happens between client and server**.


## ⏳ 4.1 Synchronous APIs

> The client sends a request and **waits** for the server response.

### 🔹 Characteristics:
- ⌛ Client blocks until response is received  
- 🌐 Most REST APIs follow this model  
- 🔁 Immediate request → response cycle  

#### 🧪 Example:
```http
GET /api/orders/1001
```

📌 Flow:

1. Client sends request
2. Server processes immediately
3. Server returns response
4. Client continues execution

💡 Best for:
* Data retrieval
* Login requests
* CRUD operations

---

## 🚀 4.2 Asynchronous APIs

> The client sends a request, and the server processes it **later**.

#### 🔹 Characteristics:

* 📨 Often used with message queues
* ⚙️ Suitable for background processing
* 🔄 Client does not wait for full processing

#### 🧪 Examples:

* 💳 Payment processing
* 📧 Email sending
* 📊 Report generation
* 🧾 Invoice creation

📌 Flow:

1. Client submits request
2. Server acknowledges (e.g., 202 Accepted)
3. Processing happens in background
4. Result delivered later (callback / polling / webhook)

---
## 🚀 Benefits of Using APIs

APIs are the backbone of modern software systems. They enable flexibility, scalability, and secure communication between services.

### 🧩 1️⃣ Modularity

- 🏗 Systems can be divided into independent services  
- 🔄 Components can be updated without affecting the entire system  
- 📦 Encourages microservices architecture  

💡 Result: Cleaner architecture & easier maintenance  


### 🔗 2️⃣ Interoperability

- 🌍 Different technologies and platforms can communicate  
- 🖥 Frontend ↔ Backend ↔ Mobile ↔ Third-party services  
- 🧱 Language-agnostic integration (Java, Python, Node, etc.)

💡 Result: Seamless cross-platform connectivity  


### 📈 3️⃣ Scalability

- ⚙️ Backend services can scale independently  
- 📊 Load can be distributed across services  
- ☁️ Cloud-native scaling becomes easier  

💡 Result: Better performance under high traffic  


### 🔐 4️⃣ Security Control

APIs support modern security mechanisms:

- 🔑 Authentication tokens (JWT, OAuth)  
- 👥 Role-Based Access Control (RBAC)  
- ⏱ Rate limiting  
- 📜 Logging and monitoring  
- 🔒 mTLS & encryption  

💡 Result: Controlled and auditable access to resources  

### ⚡ 5️⃣ Faster Development

- 🔌 Integrate third-party services instead of building from scratch  
- 💳 Payment gateways  
- 📧 Email services  
- 🗺 Map integrations  
- 🔔 Notification systems  

💡 Result: Reduced development time & cost  

---

# <span style="border: 2px solid rgba(147, 246, 17, 0.92); padding: 2px; border-radius: 4px;">⚔️ Differences Between SOAP and REST</span>

Understanding the differences between SOAP and REST helps in choosing the right API architecture for your system.

## 🔎 SOAP vs REST Comparison

| Feature | 🧼 SOAP | 🌐 REST |
|----------|----------|----------|
| **Type** | Protocol | Architectural style |
| **Data Format** | XML only | JSON, XML, etc. |
| **Transport** | HTTP, SMTP, TCP | HTTP |
| **State** | Can be stateful | Stateless |
| **Caching** | Not built-in | Supported via HTTP |
| **Performance** | Heavier | Lightweight |
| **Complexity** | High | Moderate |
| **Standardization** | Strict WS-* standards | No strict official standard |
| **Description Language** | WSDL | OpenAPI (optional) |
| **Resource Exposure** | Service-based | Resource-based URLs |

---

## 🚀 REST vs SOAP vs GraphQL vs gRPC

A broader comparison of modern API technologies:

| Feature | 🌐 REST | 🧼 SOAP | 🧩 GraphQL | ⚡ gRPC |
|-----------|----------|----------|------------|---------|
| **Style/Type** | Architectural style | Protocol | Query language | RPC framework |
| **Format** | JSON (mostly) | XML | JSON | Protocol Buffers (binary) |
| **Performance** | High | Moderate | High | Very High |
| **Endpoint Structure** | Multiple | Multiple | Single | Multiple |
| **Flexibility** | Medium | Low | High | Medium |
| **Learning Curve** | Easy | Complex | Moderate | Moderate |

---

## 📌 Summary: Choosing the Right API Style

Each API architecture has strengths depending on system requirements, scalability needs, and security demands.

## 🌐 REST

- 🌍 Most widely used  
- ⚡ Lightweight and scalable  
- 📱 Best for web and mobile applications  
- 🧩 Ideal for microservices and public APIs  


## 🧼 SOAP

- 📏 Strict standards (WS-*)  
- 🔐 Enterprise-grade security  
- 🏦 Common in banking & government systems  
- 🏛 Often used in legacy enterprise environments  


## 🧩 GraphQL

- 🎯 Flexible data querying  
- 🎛 Client-controlled responses  
- 🚫 Prevents over-fetching & under-fetching  
- 💻 Efficient for frontend-heavy applications  


## ⚡ gRPC

- 🚀 High performance  
- 📦 Binary protocol (Protocol Buffers)  
- 🔄 Supports streaming  
- 🏗 Ideal for microservices communication  

---
