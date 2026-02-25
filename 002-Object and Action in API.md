# 🧩 Object and Action in API

In APIs, **objects** and **actions** define how resources are structured and manipulated.

- 📦 **Object** → What the system manages (noun)  
- ⚡ **Action** → What you do to that object (verb)  

Understanding this distinction is critical when testing APIs for authorization and logic flaws.

---

## 1️⃣ Object in API

An **object** represents a **resource/entity** in the system.

Objects are typically **nouns** and appear in the URL path.

---

## 📚 Common API Objects

- `users`
- `accounts`
- `orders`
- `products`
- `invoices`
- `payments`
- `tickets`
- `comments`
- `files`
- `projects`

### 🧪 Example 1: User Object

```http
GET /api/users/007
```

* 📦 **Object** → `users`
* 🆔 **Object ID** → `007`


### 📥 Example Response

```json
{
  "id": 7,
  "username": "rahul",
  "email": "rahul@example.com",
  "role": "user"
}
```

> 💡 This JSON represents a **User object**.

---

### 🧾 Example 2: Order Object

```http
GET /api/orders/9001
```

---

### 📥 Response

```json
{
  "order_id": 9001,
  "user_id": 101,
  "amount": 1500,
  "status": "shipped"
}
```

> 📦 Here, the object is `order`.

### 🧩 Example 3: Nested Object

```http
GET /api/users/101/orders
```

* 👨‍👩‍👧 **Parent Object** → `users`
* 📦 **Child Object** → `orders`

> 🔎 This retrieves all orders belonging to user `101`.

---

### ⚡ 2️⃣ Action in API

An **action** defines what operation is performed on an object.

Actions are expressed using:

* 🌐 **HTTP methods (REST style)**
* 🎯 **Action-based endpoints (RPC style)**

---

## 🔄 RESTful Actions (Using HTTP Methods)

| 🛠 Method | ⚙️ Action      | 📌 Example   |
| --------- | -------------- | ------------ |
| GET       | Read           | `/users/101` |
| POST      | Create         | `/users`     |
| PUT       | Full update    | `/users/101` |
| PATCH     | Partial update | `/users/101` |
| DELETE    | Remove         | `/users/101` |


---

### 🧪 Example 1: Create User

```http
POST /api/users
```

#### 📦 Body

```json
{
  "username": "testuser",
  "email": "test@example.com"
}
```

* 📦 **Object** → `users`
* ⚡ **Action** → `POST` (create)

---

### 🔄 Example 2: Update Order

```http
PUT /api/orders/9001
```

* 📦 **Object** → `orders`
* ⚡ **Action** → `update`

---

### 🗑️ Example 3: Delete Comment

```http
DELETE /api/comments/555
```

* 📦 **Object** → `comments`
* ⚡ **Action** → `delete`

---
## ⚡ Action-Based Endpoints (Non-REST Style)

Some APIs define actions explicitly in the URL.

In this style:
- 📦 **Object** appears in the path  
- ⚙️ **Action** is written as part of the endpoint  

---

### 🔐 Example 1: Reset Password

```http
POST /api/users/007/reset-password
```

* 📦 **Object** → `users`
* ⚙️ **Action** → `reset-password`

---

### ❌ Example 2: Cancel Order

```http
POST /api/orders/9001/cancel
```

* 📦 **Object** → `orders`
* ⚙️ **Action** → `cancel`

---

### 💸 Example 3: Transfer Funds

```http
POST /api/accounts/2001/transfer
```

* 📦 **Object** → `accounts`
* ⚙️ **Action** → `transfer`

---

### ✅ Example 4: Approve Invoice

```http
POST /api/invoices/300/approve
```

* 📦 **Object** → `invoices`
* ⚙️ **Action** → `approve`

---

💡 **REST vs Non-REST Quick Insight**

* 🌐 **REST Style** → Action is implied by HTTP method

  * `DELETE /users/101`
* 🎯 **Non-REST Style** → Action is explicitly written in URL

  * `POST /users/101/delete`

---

## 🧠 Complex Object + Action Examples

### 🏦 Banking API Example

```http
GET  /accounts/5001
POST /accounts
POST /accounts/5001/withdraw
POST /accounts/5001/deposit
POST /accounts/5001/close
```

#### 📦 Objects:

* `accounts`

### ⚙️ Actions:

* `withdraw`
* `deposit`
* `close`

💡 Here:

* `/accounts/5001` → Specific account object
* `/withdraw`, `/deposit`, `/close` → Business-critical actions

These endpoints are highly sensitive and must be strictly protected with proper authorization and validation.

---

## ☁️ SaaS (Software as a service) Application Example

```http
GET    /projects/10
POST   /projects
POST   /projects/10/archive
POST   /projects/10/add-member
DELETE /projects/10
```

#### 📦 Objects:

* `projects`

#### ⚙️ Actions:

* `archive`
* `add-member`
* `delete`

💡 In SaaS systems:

* Object-level access control is critical
* Action-based endpoints like `/add-member` and `/archive` often introduce privilege escalation risks

---

## ⚖️ Object vs Action

| 🧩 Aspect     | 📦 Object        | ⚙️ Action              |
|--------------|------------------|------------------------|
| **Type**     | Noun             | Verb                   |
| **Represents** | Resource         | Operation              |
| **Example**  | `user`, `order`  | `create`, `delete`, `cancel` |

---

## 📌 Summary

- 📦 **Object** = What the API manages  
- ⚙️ **Action** = What operation is performed  
- 🧱 Objects are **nouns**  
- 🏃 Actions are **verbs**  

---

## 🔐 Security Perspective

Security testing requires validating **both**:

- 🔑 Access to the object  
- 🛡 Permission to perform the action  

> 💡 If either is improperly enforced, it can lead to:
> - Broken Access Control  
> - Privilege Escalation  
> - IDOR / BOLA vulnerabilities  

---

✅ **Golden Rule for API Testing:**  
Always ask:  
1. *Can I access this object?*  
2. *Can I perform this action on it?*  


