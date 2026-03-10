# Finding Endpoints & API Fuzzing Methodology

Endpoint discovery is one of the most important phases in API penetration testing.  
If you miss endpoints, you miss vulnerabilities.

This guide uses practical examples from **OWASP Juice Shop**.

---

## 1️⃣ Finding Endpoints (Manual Discovery)

### 1.1 Browse the Application

- Interact with every feature  
- Register / login  
- Update profile  
- Add items to cart  
- Checkout  
- Admin panel (if accessible)  

### Use proxy tools like:

- Burp Suite  
- OWASP ZAP  

### Capture:

- Hidden API calls  
- JSON responses  
- Authentication tokens  
- Role-based endpoints  

---

## 1.2 Analyze JavaScript Files

Look for:

- /assets/*.js  
- /main.js  
- /runtime.js  

Search for:

```
/api/
/rest/
fetch(
axios(
http.get(
```

JS files often reveal:

- Hidden endpoints  
- Admin routes  
- Deprecated versions  
- Hardcoded API paths  

---

## 2️⃣ Fuzzing Web Application APIs

Repository:  
https://github.com/chrislockard/api_wordlist  

Clone:

```bash
git clone https://github.com/chrislockard/api_wordlist.git
```

Wordlists include:

* Objects
* Actions
* Paths

---

## 2.1 Fuzzing with ffuf

Tool:

* ffuf

Example:

```bash
ffuf -u http://target.com/FUZZ -w api_wordlist/common.txt
```

Fuzz:

* /api/FUZZ
* /rest/FUZZ
* /v1/FUZZ
* /admin/FUZZ
---


## 2.2 What to Fuzz

### 📦 Objects

```
users
admin
orders
products
payments
wallet
```

### 📌 Actions

```
login
register
delete
update
export
download
reset
```

### 📂 Paths

```
internal
debug
test
beta
backup
old
```

---

## 3️⃣ Request Methods Testing

Test every HTTP method on discovered endpoints.

| Method  | Purpose            | Test Case                |
|----------|-------------------|--------------------------|
| GET      | Retrieve data      | IDOR                     |
| POST     | Create             | Mass assignment          |
| PUT      | Update             | Privilege escalation     |
| DELETE   | Remove             | Unauthorized deletion    |
| PATCH    | Partial update     | Logic abuse              |
| HEAD     | Header testing     | Info leakage             |
| OPTIONS  | Allowed methods    | Misconfiguration         |
| TRACE    | XST                | Rare but test            |
| CONNECT  | Proxy abuse        | Rare                     |

---

### Example

```bash
curl -X PUT http://target.com/api/users/1
```

### Check:

* 200 vs 403
* Method override headers
* Improper restrictions

---

## 4️⃣ Using Postman for API Testing

**Tool:**

- Postman  


## 4.1 Testing Token Authentication

### Example token:

```
eyJhbGciOiJSUzI1NiJ9 ...
```

### Decode JWT:

- Check algorithm (**RS256**)  
- Inspect claims:
  - sub  
  - role  
  - exp  

### Test:

- Modify role → admin  
- Remove signature  
- Change expiration  
- Replay expired token  

---

## 5️⃣ OWASP Juice Shop – Endpoint Mapping

Below are major API endpoints from **OWASP Juice Shop**.

---

## 5.1 Authentication & User-Related Endpoints

```
/rest/user/authentication-details
/rest/user/login
/rest/user/change-password
/rest/user/reset-password
/rest/user/whoami
/rest/saveLoginIp
/rest/deluxe-membership
```

### Test For:

- Brute force login  
- Weak password reset  
- IDOR in change-password  
- JWT manipulation  
- Account enumeration  

---

## 5.2 Admin & Application Info

```
/rest/admin/application-version
```

### Test For:

- Unauthorized access  
- Information disclosure  

---

## 5.3 Languages Endpoint

```
/rest/languages
```

### Test:

- Injection  
- Header manipulation  
- Path traversal  

---

## 5.4 Address Management

```
/api/Address
```

### Methods:

- get()  
- getById(id)  
- save(data)  
- put(id, data)  
- del(id)  

**test :**

- IDOR
- Unauthorized deletion
- Mass assignment

---

## 5.5 Product & Delivery

```
/api/Products
/api/Deliverys
```

### Test:

- Price manipulation  
- Stock tampering  
- Hidden fields  
- Business logic abuse  

---

## 5.6 Wallet & Financial Data

```
/rest/wallet/balance
/rest/web3/nftUnlocked
/rest/web3/nftMintListen
/rest/web3/submitKey
/rest/web3/walletNFTVerify
/rest/web3/walletExploitAddress
```

### Test:

- Financial data exposure  
- Replay attacks  
- Logic bypass  
- Blockchain-related input tampering  

---

## 5.7 Security & Challenges

```
/api/Challenges
/rest/repeat-notification
/rest/continue-code
/rest/continue-code-findIt
/rest/continue-code-fixIt
```

### Test:

- Access control flaws  
- Business logic bypass  

---

## 5.8 Miscellaneous Endpoints

```
/rest/country-mapping
/snippets/fixes
/snippets/verdict
/api/Recycles
/api/Cards
```

### Test:

- Data leakage  
- IDOR  
- Unauthorized modification  

---

## 6️⃣ Advanced Endpoint Discovery Techniques

## 6.1 Version Testing

Try:

```
/v1/
/v2/
/v3/
/beta/
/internal/
```

Older versions often:

- Lack auth checks  
- Have debug features enabled  

---

## 6.2 Parameter Discovery

Fuzz parameters:

```bash
ffuf -u http://target.com/api/users?FUZZ=test -w params.txt
```

Look for:

* Hidden parameters
* Debug flags
* admin=true
* role=admin

---


## 6.3 GraphQL Endpoint Discovery

### Check:

```
/graphql
/graphiql
```

Test introspection.

---

## 7️⃣ Endpoint Testing Checklist

- Endpoint discovered  
- Auth required?  
- Role required?  
- IDOR tested  
- Mass assignment tested  
- Rate limit tested  
- Business logic tested  
- HTTP methods tested  
- Parameter fuzzed  
- Version tested  

---

## 8️⃣ Professional API Endpoint Mapping Template

## 🎯 Target

```
Base URL:
Version:
Authentication:
```

---

## 📋 Endpoint Table

| Endpoint     | Method | Auth | Role | IDOR  | Notes |
|-------------|--------|------|------|-------|-------|
| /api/users  | GET    | Yes  | User | Tested|       |

---