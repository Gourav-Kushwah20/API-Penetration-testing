# Broken Object Level Authorization (BOLA)

## Vulnerability Description

**Broken Object-Level Authorization (BOLA)** occurs when an application fails to properly enforce authorization checks on objects referenced by user-supplied identifiers.  

This allows attackers to manipulate object identifiers (such as **vehicle_id**, **order_id**, or **report_id**) to access or modify resources belonging to other users.

In the affected application, several API endpoints expose object identifiers in URLs or parameters but **do not validate whether the authenticated user owns the requested resource**.

As a result, an authenticated user can modify identifiers and access **sensitive data belonging to other users**.

---

## Affected Endpoints

### 1. Vehicle Location API

```
GET /identity/api/v2/vehicle/{vehicle_id}/location
```

### Example Request

```
GET /identity/api/v2/vehicle/4bae9968-ec7f-4de3-a3a0-ba1b2ab5e5e5/location HTTP/1.1
Host: 192.168.1.28:8888
Authorization: Bearer <user_token>
```

---

### Steps to Reproduce

1. Login as **User A**
2. Navigate to the dashboard

```
[http://192.168.1.28:8888/dashboard]
```
3. Capture the request for the vehicle location

```
/identity/api/v2/vehicle/<vehicle_id>/location
```

4. Replace the `vehicle_id` with another valid UUID belonging to **User B**

```
/identity/api/v2/vehicle/fa8cca43-d125-4a0c-8872-8501bcb30145/location
```
5. Send the request
---

### Result

The server returns the **location data of another user's vehicle**.

### Expected Result

The application should return:

```
403 Forbidden
```

or

```
Access Denied
```

if the vehicle does **not belong to the authenticated user**.

---

### 2. Orders API

### Vulnerable Endpoint

```
GET /workshop/api/shop/orders/{order_id}
```

### Example

```
GET /workshop/api/shop/orders/1
GET /workshop/api/shop/orders/9
```

---

### Steps to Reproduce

1. Login as **User A**.

2. Capture the request:

```
GET /workshop/api/shop/orders/1
```

3. Modify the order ID:

```
GET /workshop/api/shop/orders/9
```

4. Send the request.

### Result

The application returns **order details belonging to another user**.

---

## 3. Mechanic Reports

### Vulnerable Endpoint

```
GET /workshop/api/mechanic/mechanic_report?report_id=2
```

---

### Steps to Reproduce

1. Login as a **normal user**.

2. Capture the request for the mechanic report.

3. Modify the parameter:

```
report_id=1
report_id=2
report_id=3
```

4. Send the request.

### Result

The application returns **mechanic reports belonging to other users**.

---

## Security Impact

Successful exploitation allows an attacker to access sensitive information including:

- Vehicle location data  
- Other users' orders  
- Mechanic diagnostic reports  
- Potential personally identifiable information (PII)  

This vulnerability could enable:

- Privacy violations  
- Vehicle tracking  
- Data leakage  
- Business logic abuse  

---

## Risk Severity

**High**

---

## CVSS v3.1 Example
```
CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:L/A:N
Score: 7.5 (High)
```

---

## Recommended Remediation

### 1. Enforce Object-Level Authorization

Verify ownership of every object before returning data.

**Example:**
```python
if order.user_id != current_user.id:
    return 403
```

---

### 2. Implement Access Control Middleware

Centralize authorization checks for all sensitive resources.

---

### 3. Use Indirect Object References

Avoid exposing predictable IDs. Use secure references or enforce strict authorization checks.

---

### 4. Apply Authorization at the Database Layer

**Example:**

```sql
SELECT * FROM orders
WHERE order_id = ?
AND user_id = current_user_id;
```

---

### 5. Log and Monitor Access

Log abnormal access patterns such as:

* Multiple ID enumeration attempts
* Requests for objects belonging to other users

---