# 🧪 vapi

## 🚀 vapi – Lab Setup & Testing Guide

🔗 **Repository:**
[https://github.com/roottusk/vapi](https://github.com/roottusk/vapi)


## 1️⃣ Installation (Your Steps – Verified)

### 📂 Step 1: Navigate to Working Directory

```bash id="vapi1a"
cd /opt
```

### 📥 Step 2: Clone Repository

```bash id="vapi2b"
git clone https://github.com/roottusk/vapi.git
```

### 📁 Step 3: Enter Project Directory

```bash id="vapi3c"
cd /opt/vapi
```

### 🐳 Step 4: Start Lab (Docker Compose)

```bash id="vapi4d"
docker-compose up -d
```

### ✅ Verify Services

```bash id="vapi5e"
docker ps
netstat -nltup
```

### 2️⃣ Access URLs

If your IP is:

```text
192.168.1.28
```

---

### 🔗 Available Endpoints

| Endpoint                        | Purpose                |
| ------------------------------- | ---------------------- |
| `http://192.168.1.28/`          | Default landing        |
| `http://192.168.1.28/vapi`      | API root               |
| `http://192.168.1.28/vapi/api1` | Versioned API endpoint |

---

### What vapi Is

vapi is a **minimal intentionally vulnerable REST API**.  
It is lightweight compared to crAPI and ideal for:

- API enumeration practice  
- Injection testing  
- Authentication bypass testing  
- IDOR discovery  
- Basic fuzzing  

It’s good as a **starter lab before crAPI** in your API pentesting workflow.

---

## 🔎 4️⃣ Initial Enumeration

### Step 1 – Directory Enumeration

```bash
ffuf -u http://192.168.1.28/FUZZ -w /usr/share/wordlists/dirb/common.txt
````

Also:

```bash
ffuf -u http://192.168.1.28/vapi/FUZZ -w wordlist.txt
```

---

### Step 2 – HTTP Method Testing

```bash
curl -X OPTIONS http://192.168.1.28/vapi/api1 -i
```

Test:

- GET
- POST
- PUT
- DELETE

Sometimes insecure methods are enabled.

---

### Step 3 – Parameter Discovery

```bash
ffuf -u "http://192.168.1.28/vapi/api1?FUZZ=test" -w param.txt
```

Look for:

* Hidden parameters
* Debug flags
* Admin flags

---

## 5️⃣ Common Vulnerabilities in vapi

### 1. SQL Injection

Test with:

```text
' OR 1=1--
" OR "1"="1
```

```bash
sqlmap -u "http://192.168.1.28/vapi/api1?id=1" --batch --dbs
```

---

## 2. Authentication Bypass

**Try:**

```text
admin' --
admin" OR "1"="1
```

**Check:**

* Hardcoded credentials
* Missing password validation
* JWT manipulation (if present)

---

## 3. IDOR (Broken Object Level Authorization)

If endpoint exists:

```text
/vapi/api1/user/1
```

**Try:**

```text
/vapi/api1/user/2
/vapi/api1/user/3
```

**Check:**

* Horizontal privilege escalation
* Access to other users' data

---

## 4. Input Validation Issues

**Test:**

- Long strings  
- Special characters  
- JSON tampering  
- Negative values  
- Large numeric values  

---

## 5. Mass Assignment

If JSON body:

```json
{
  "username": "rahul",
  "role": "admin"
}
```

**Try adding:**

* isAdmin
* role
* balance
* user_id


---

## 6. Burp Testing Flow (Recommended)

1. Proxy request through Burp  
2. Send to Repeater  
3. Modify:
   - ID values  
   - HTTP method  
   - Headers  
   - JSON body  
4. Observe response differences  

---

## 7. Compare With crAPI

| Feature        | vapi   | crAPI    |
|---------------|--------|----------|
| Complexity    | Low    | High     |
| Microservices | No     | Yes      |
| JWT           | Limited| Heavy    |
| Business Logic| Basic  | Advanced |
| GraphQL       | No     | No       |

vapi is good for:

- Basic API fuzzing drills  
- Automation testing  
- sqlmap practice  

---

## 8. Suggested Attack Checklist

- Enumerate endpoints  
- Identify authentication mechanism  
- Test IDOR  
- Test SQLi  
- Test auth bypass  
- Test rate limiting  
- Try method override  
- Check CORS policy  
- Test error message leakage  

---