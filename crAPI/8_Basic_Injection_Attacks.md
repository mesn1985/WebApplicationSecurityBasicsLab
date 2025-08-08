# 🧪 Basic Injection Attack

## 🧠 Learning Objectives
- Understand the concept of injection attacks and how they manifest in applications.
- Perform basic NoSQL and SQL injection attacks to retrieve unauthorized information.
- Identify risks in server-side request handling through SSRF attacks.
- Reflect on defense mechanisms such as input validation and allowlists.

---

Injection attacks occur when malicious actors inject harmful data into a program to alter its execution in unintended ways. These attacks can allow unauthorized actions or data access.

There are multiple categories of injection attacks, including:
- **SQL injection**
- **Code injection**
- **Command injection**
- **NoSQL injection**
- **Server-side request forgery (SSRF)**

This set of exercises provides a basic introduction to injection attacks, focusing on practical examples using crAPI.

> ⚠️ **Ethical Reminder**: These exercises should only be performed in authorized, isolated environments such as crAPI. Never test real systems without explicit permission.

---

## ⚙️ Prerequisites
Before starting, you should have completed the [Fuzzing Input](./7_Fuzzing_input.md) exercises.

---

## 🔍 Basic NoSQL Injection Attack

In the previous fuzzing exercise, the input `{ "$ne": null }` triggered an unusual response. You will now attempt to exploit this as a NoSQL injection vulnerability.

### 📌 Steps
1. In the crAPI web app, validate a coupon and capture the request in Burp Suite.
2. Modify the coupon code in the request body to: `{ "$ne": 1 }`
3. Observe and interpret the response. If successful, note the returned coupon—you will need it for the next task.

> 🧠 NoSQL databases like MongoDB do **not** use SQL. Instead, they rely on structured documents (typically JSON). Learn more about [MongoDB here](https://www.mongodb.com/nosql-explained).

---

## 🛠️ Basic SQL Injection – Retrieve Database Version

You’ll now attempt a SQL injection attack to identify the relational database in use.

### 📌 Steps
1. Use the coupon retrieved in the previous exercise and capture the apply coupon request.
2. Change the coupon code in the request to: `0'; select version() --+`
3. Observe the reply and search online to determine the database type based on the returned version.

> 🧠 Many web applications use both relational and NoSQL databases. This mix can lead to diverse and hard-to-detect vulnerabilities.

> ✅ Secure coding practices such as input validation, parameterized queries, and ORM use can prevent these attacks. Combined, they form **defense in depth**.

---

## 🌐 Basic Server-Side Request Forgery (SSRF)

In this challenge, you’ll attempt to make the crAPI backend send a request to an external site—Google.

### 🎯 Goal
Make the API issue a **GET** request to `https://www.google.dk` by modifying a POST request to the **contact mechanic** endpoint.

### 📌 Steps
1. Capture the **POST** request used to contact a mechanic.
2. Modify the request so that the API attempts a server-side call to https://www.google.dk.

> ✅ To prevent SSRF, applications should maintain a **request allowlist** that restricts outgoing requests to known and safe destinations.

---

## 🧠 Reflection Questions

- What differentiates NoSQL injection from SQL injection?
- How could the application have prevented these injection attacks?
- What are the broader implications of a successful SSRF attack?
- How do secure practices like parameterized queries and request allowlists help prevent injection attacks?
