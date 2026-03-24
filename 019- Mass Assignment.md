# ⚠️ Mass Assignment

**Mass Assignment** is a vulnerability that occurs when an API automatically binds client-supplied input to internal object properties **without proper validation or field-level restrictions**.  

This allows attackers to modify **sensitive or restricted attributes** that should not be controlled by the user.

This vulnerability commonly arises when developers rely on frameworks that automatically map request parameters to backend model objects, without implementing **allowlists (whitelists)** or proper **authorization checks** for each field.

---

## 🎯 What Attackers Can Manipulate

When Mass Assignment is possible, an attacker can add unexpected parameters to the request payload and manipulate attributes such as:

- 👤 **User roles or permissions**
- 🚩 **Account status flags**
- 🆔 **User identifiers**
- 📦 **Object ownership fields**
- ✔️ **Verification flags**
- ⚙️ **Internal configuration attributes**

⚠️ If these fields are processed without validation, attackers may:

- 🔓 Gain **privileged access**
- 🛠️ Modify **other users’ data**
- 🚫 Bypass **security controls**

---

## 🧪 Steps to Identify Mass Assignment Vulnerabilities in API Pentesting

### 1️⃣ Understand the API Behavior

Analyze the API endpoints and identify how request parameters are mapped to backend objects.

- Review API documentation  
- Analyze request/response structures  
- Identify expected vs unexpected fields  

---

### 2️⃣ Enumerate Sensitive Fields

Attempt to identify hidden or sensitive attributes such as:

- `role`
- `is_admin`
- `isverified`
- `user_id`
- `account_status`
- `OwnerID`

These fields are often **not intended to be controlled by users**, but may still be processed by the API.

---

### 3️⃣ Test Unauthorized Fields

Add additional parameters to requests and observe whether the API **accepts and processes them**.

#### 🔍 Example Approach

- Include fields that are **not present in the original request or UI forms**
- Modify request payloads by injecting **hidden or sensitive attributes**

This helps determine whether the API is vulnerable to **mass assignment**.

---

### 4️⃣ Observe Responses

Analyze the API response and application behavior to determine if the injected fields were **accepted and applied**.

####     📊 Indicators include:

- ✏️ **Modified response values**
- ⬆️ **Privilege escalation**
- 🔄 **Changes reflected in subsequent requests**

These signs confirm that **unauthorized fields are being processed by the backend**.


---

## 🧪 Example Scenarios Demonstrating Mass Assignment

### 🔺 Example 1: Privilege Escalation

### 📥 Request

```
POST /api/updateProfile HTTP/1.1
Host: example.com
Content-Type: application/json

{
"username": "test_user",
"email": "user@example.com",
"role": "admin"
}
```

---

### ⚠️ Vulnerable Response

```json
{
  "status": "success",
  "message": "Profile updated successfully",
  "user": {
    "username": "test_user",
    "email": "user@example.com",
    "role": "admin"
  }
}
```

---

### 🚨 Explanation

The API **incorrectly accepts and processes the `role` field**, even though it should not be controlled by the user.

This results in:

* 🔓 **Privilege escalation**
* 👤 User gaining **admin-level access**
* ⚠️ Lack of **field-level validation**

This confirms a **Mass Assignment vulnerability**, where sensitive fields are **not properly restricted**.

---

## 🔺 Example 2: Unauthorized Account Modification / Account Takeover

### 📥 Request

```
PUT /api/updateAccount HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "userID": "12345",
  "username": "attacker",
  "password": "newpassword",
  "email": "attacker@example.com"
}
```

---

### ⚠️ Vulnerable Response

```json
{
  "status": "success",
  "message": "Account updated successfully",
  "user": {
    "userID": "12345",
    "username": "attacker",
    "email": "attacker@example.com"
  }
}
```

---

### 🚨 Explanation

The API **accepts the `userID` parameter from the client-side request**, even though this field should be **server-controlled** and associated with the authenticated user.

This results in:

* 🔓 **Account takeover** - Attackers can modify any user's account by changing the `userID` parameter
* 🔑 **Password reset** - Attackers can change other users' passwords
* 📧 **Email hijacking** - Attackers can modify email addresses of any user
* ⚠️ **Missing authorization checks** - The API fails to verify ownership of the account being modified

This confirms a **Mass Assignment vulnerability** combined with **Broken Object Level Authorization (BOLA)**, allowing attackers to modify **arbitrary user accounts**.

---

## 🔺 Example 3: Password Enumeration

### 📥 Request

```
POST /api/authenticate HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "test_user",
  "passwords": [
    "password123",
    "letmein",
    "admin@123",
    "password"
  ]
}
```

---

### ⚠️ Vulnerable Response

```json
{
  "status": "success",
  "message": "Authenticated successfully with password: letmein"
}
```

---

### 🚨 Explanation

The API **accepts multiple passwords** in a single request and **reveals which password is correct** in the response message.

This results in:

* 🔐 **Password enumeration** - Attackers can test multiple password candidates and identify valid ones
* 🛠️ **Brute force acceleration** - The API feedback enables rapid password guessing
* 💾 **Weakened authentication** - The unexpected `passwords` array parameter should not be accepted
* ⚠️ **Information disclosure** - Revealing the correct password in the response is a security flaw

This confirms a **Mass Assignment vulnerability** combined with **Excessive Data Exposure**, allowing attackers to enumerate and compromise user passwords through **password enumeration attacks**.

---

## 🔺 Example 4: Modifying Hidden Attributes

### 📥 Request

```
PATCH /api/users/12345 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "test_user",
  "email": "user@example.com",
  "isVerified": true
}
```

---

### ⚠️ Vulnerable Response

```json
{
  "status": "success",
  "message": "User updated successfully",
  "user": {
    "userID": "12345",
    "username": "test_user",
    "email": "user@example.com",
    "isVerified": true
  }
}
```

---

### 🚨 Explanation

The API **allows modification of the `isVerified` attribute**, even though this is a **hidden/internal field** that should only be controlled by the server.

This results in:

* ⚠️ **Verification bypass** - Attackers can mark unverified accounts as verified without completing the verification process
* 🚫 **Security control bypass** - Account verification procedures can be circumvented
* 📧 **Email verification bypass** - Attackers can activate accounts without verifying email addresses
* 🔓 **Unauthorized access** - Unverified malicious accounts gain the same privileges as legitimate verified users

This confirms a **Mass Assignment vulnerability** where **hidden internal attributes** are exposed to client-side manipulation, enabling attackers to **bypass security controls and verification processes**.

---

## 🔺 Example 5: Changing Ownership of Objects

### 📥 Request

```
PUT /api/updateOrder HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "orderID": "67890",
  "userID": "12345",
  "status": "completed"
}
```

---

### ⚠️ Vulnerable Response

```json
{
  "status": "success",
  "message": "Order updated successfully",
  "order": {
    "orderID": "67890",
    "userID": "12345",
    "status": "completed"
  }
}
```

---

### 🚨 Explanation

The API **allows modification of the `userID` field**, even though this field should be **immutable** and server-controlled to maintain object ownership.

This results in:

* 🔓 **Ownership takeover** - Attackers can claim ownership of orders belonging to other users
* 📦 **Object privilege escalation** - Attackers can modify critical business objects they don't own
* 💰 **Financial fraud** - Attackers can claim orders and modify associated transactions
* ⚠️ **Broken Object Level Authorization** - The API fails to verify that the user owns the object being modified

This confirms a **Mass Assignment vulnerability** combined with **Broken Object Level Authorization (BOLA)**, allowing attackers to **change object ownership** and compromise the **integrity of business-critical data**.

---

## 💥 Impact of Mass Assignment

Mass Assignment vulnerabilities can lead to:

* 🔓 **Privilege escalation** - Attackers gain elevated permissions or admin access
* ✏️ **Unauthorized data modification** - Attackers modify sensitive data they shouldn't control
* 👤 **Account takeover** - Attackers compromise entire user accounts
* 🛠️ **Business logic bypass** - Attackers circumvent critical business rules and workflows
* 🚫 **Unauthorized access to other users' resources** - Attackers access and modify data belonging to other users

---

## 🛡️ Recommended Mitigations

### 1️⃣ Use Allowlisting (Whitelist Fields)

Only accept **explicitly permitted fields** in API requests.

- Define a whitelist of allowed parameters for each endpoint
- Reject any parameters not in the whitelist
- Document which fields users can modify

### 2️⃣ Implement Proper Authorization Checks

Ensure users **cannot modify attributes they are not authorized** to control.

- Verify that the user has permission to modify each field
- Implement role-based access control for sensitive attributes
- Validate ownership before allowing modifications

### 3️⃣ Separate DTOs from Database Models

Use Data Transfer Objects instead of **binding directly to internal models**.

- Create separate request/response DTOs for API communication
- Map DTOs to internal models with explicit field mapping
- Prevent direct binding of client input to domain objects

### 4️⃣ Ignore Sensitive Fields

**Prevent modification** of fields such as:

- `role` / `is_admin`
- `isAdmin` / `isVerified`
- `accountStatus`
- `userID`

Mark these fields as read-only or exclude them from binding entirely.

### 5️⃣ Server-Side Validation

Implement **comprehensive server-side validation** for all inputs:

- Validate field types and values
- Ensure immutable fields cannot be modified
- Perform authorization checks on sensitive operations

---

## 🧪 crAPI Lab - Mass Assignment Examples

The following examples were identified while testing the **crAPI** training application. These scenarios demonstrate how improper validation and unrestricted parameter binding can lead to **Mass Assignment and Business Logic vulnerabilities**.

### 🔺 1: Get an Item for Free

#### 📝 Description

The API allows manipulation of **financial parameters** such as `quantity` and `credit`. By submitting a **negative quantity**, the total price calculation can be bypassed, allowing the attacker to obtain items without paying.

#### 📥 Request

```
POST /workshop/api/shop/orders HTTP/1.1
Host: 192.168.1.28:8888
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{
  "product_id": 2,
  "quantity": -10,
  "credit": 25500
}
```

#### Issue

- The API **does not validate** that `quantity` must be a **positive integer**.
- **Negative values** manipulate the order calculation logic.
- This allows attackers to **purchase items without reducing their balance**.

#### 🎯 Impact

* 🎁 **Free purchases** - Attackers can obtain items without payment
* 🛠️ **Business logic bypass** - Price calculation can be circumvented
* 💰 **Potential financial loss** - The business loses revenue through unauthorized transactions

---

## 🔺 2: Increase Your Balance by $1,000 or More

#### 📝 Description

An attacker can manipulate the `status` field on an order to trigger refund logic in the application.

#### 📥 Request

```
PUT /workshop/api/shop/orders/21 HTTP/1.1
Host: 192.168.1.28:8888
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{
  "quantity": 1,
  "status": "returned"
}
```

#### Issue

- The API allows users to directly update the `status` field.
- Changing the status to `returned` triggers the refund process.
- This increases the attacker's account balance.

#### 🎯 Impact

* 💸 **Unauthorized refunds** - Attackers can generate fraudulent refunds
* 🪙 **Balance manipulation** - Account balance can be increased without legit activity
* 💰 **Financial fraud** - Monetary impact through incorrect refund processing

---

## 🔺 3: Update Internal Video Properties

#### 📝 Description

The API allows modification of internal processing parameters such as `conversion_params`.

#### 📥 Request

```
PUT /identity/api/v2/user/videos/102 HTTP/1.1
Host: 192.168.1.28:8888
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{
  "videoName": "Sample_Video_1M.mp4",
  "conversion_params": "-v codec test"
}
```

#### Issue

- The API exposes internal processing parameters.
- Attackers can modify `conversion_params`, which should normally be controlled only by the backend.
- If these parameters are passed to a system command (e.g., FFmpeg), it may lead to **command injection** or **arbitrary command execution**.

#### 🎯 Impact

* ⚙️ **Manipulation of backend processing logic** - Attackers control media conversion behavior
* 🐛 **Potential command injection** - Arbitrary command execution through untrusted params
* 🔒 **Unauthorized control over media pipeline** - Attackers can disrupt or hijack processing workflows

---

## 🔑 Key Observations

The vulnerabilities observed in the crAPI lab demonstrate several common API security issues:

- ⚠️ **Mass Assignment**
- 🚫 **Lack of input validation**
- 🧠 **Business logic flaws**
- 🔐 **Exposure of internal system parameters**

These issues are commonly highlighted in the **OWASP API Security Top 10** and can lead to serious security risks if not properly mitigated.





