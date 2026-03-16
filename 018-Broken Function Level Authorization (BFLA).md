# 🔓 Broken Function Level Authorization (BFLA)

## 🔓 Broken Function Level Authorization (BFLA)

**Broken Function Level Authorization (BFLA)** occurs when an API fails to properly enforce **authorization checks for specific functions or endpoints**.  

This allows users with **lower privileges** to access functionality that should be restricted to **higher-privileged roles** (such as administrators).

If authorization checks are missing or improperly implemented, attackers may **invoke privileged operations directly** by interacting with backend API endpoints.

This vulnerability is listed in the **OWASP API Security Top 10** as a **critical API security risk**.

---

## ⚠️ How BFLA Occurs

## 1️⃣ Lack of Proper Role Verification

The API does not validate whether the **authenticated user has the required role or permission** before executing sensitive functions.

---

## 2️⃣ Overly Permissive Endpoints

Endpoints intended for **administrators or privileged users** are accessible to normal authenticated users.

---

## 3️⃣ Client-Side Authorization Enforcement

The application relies on the **frontend to restrict functionality** instead of enforcing authorization on the **server**.

---

## 4️⃣ Exposed Sensitive Endpoints

Administrative endpoints are exposed and accessible if an attacker **manually sends requests to them**.

---

## 🎯 Vulnerable Endpoint

```
DELETE /identity/api/v2/admin/videos/52
```

## 🌐 URL

```
http://192.168.1.28:8888/identity/api/v2/admin/videos/52
```

This endpoint appears to be an **administrative API function** used to **delete videos from the system**.

---

## 🧪 Proof of Concept (PoC)

The following request was sent using a **regular user JWT token** (`role = user`):

```
DELETE /identity/api/v2/admin/videos/52 HTTP/1.1
Host: 192.168.1.28:8888
Authorization: Bearer <USER_JWT_TOKEN>
Content-Type: application/json
```

---

## 🔍 Decoded JWT Payload

```json
{
  "sub": "ankit+1@gmail.com",
  "role": "user",
  "iat": 1732182096,
  "exp": 1732786896
}
```

Despite the user having the role:

```
role: user
```

the API allowed access to an endpoint intended for **administrative operations**.

---

## 🧾 Vulnerability Description

The endpoint:

```
/identity/api/v2/admin/videos/{id}
```

is part of the **admin API namespace**, indicating that it should only be accessible to users with **administrative privileges**.

However, the server **does not enforce role-based authorization checks**, allowing a normal authenticated user to perform privileged operations.

This represents a **Broken Function Level Authorization (BFLA)** vulnerability.

---

## 🚨 Impact

An attacker could exploit this vulnerability to:

- 🗑️ **Delete videos from the platform**
- ⚙️ **Perform administrative actions**
- 🛠️ **Modify or remove sensitive data**
- 🔌 **Disrupt platform functionality**
- 🔓 **Abuse privileged API operations**

If multiple admin endpoints exist, attackers could potentially gain **full administrative capabilities**.

---

## 🔥 Severity

**High**

### Reasons

- ⬆️ **Privilege escalation**
- 🔓 Unauthorized access to **admin-level functionality**
- ⚠️ Potential **data manipulation or service disruption**
- Direct API access without restrictions.

---

## 🚩 Indicators of BFLA

Common indicators include:

### 📡 API endpoints containing paths like:

```
/admin/
/management/
/internal/
/superuser/
```

### Other Warning Signs

- ⚠️ **Administrative functions accessible using normal user tokens**
- ❌ **Missing server-side authorization checks**

These signs often indicate that **privileged operations may be accessible to unauthorized users**.

---

## 🛠️ Recommended Remediation

## 1️⃣ Implement Role-Based Access Control (RBAC)

The API must **verify user roles before executing privileged actions**.

### Example Logic

```python
if user.role != "admin":
    return 403 Forbidden
```

This ensures that **only authorized users can access administrative endpoints**.

---

## 2️⃣ Enforce Authorization on the Server

Never rely on **frontend restrictions**.

All authorization checks must occur on the **backend API layer**, ensuring that users cannot bypass security controls by directly interacting with API endpoints.

---

## 3️⃣ Restrict Access to Admin Endpoints

Endpoints under paths like:

```
/admin/*
```

should only be accessible to **admin roles**.

This ensures that **privileged functionality cannot be accessed by normal users**.

---

## 4️⃣ Implement Centralized Authorization Middleware

Use **authorization middleware** to validate permissions for every request before it reaches the business logic.

This approach ensures:

- Consistent **authorization enforcement**
- Reduced risk of **missing authorization checks**
- Centralized **security control**

---

## 5️⃣ Log and Monitor Privileged Actions

Administrative actions such as **deleting resources or modifying sensitive data** should be **logged and monitored** to detect misuse or abuse.

Proper monitoring helps identify:

- Unauthorized privilege escalation
- Suspicious administrative activity
- Potential security incidents

---

## 🔐 Secure Response Example

If a normal user attempts this request, the API should respond:

```
HTTP/1.1 403 Forbidden
{
"error": "Unauthorized access"
}
```

---

## 📌 Conclusion

The API fails to enforce proper **function-level authorization checks**, allowing users with insufficient privileges to access **administrative functionality**.

This vulnerability can lead to:

- 🔓 Unauthorized administrative actions
- ⚠️ Privilege escalation
- 🛠️ Data manipulation or deletion
- 💥 Potential system compromise