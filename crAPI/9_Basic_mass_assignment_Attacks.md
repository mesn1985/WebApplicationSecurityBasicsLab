# 🛠️ Mass Assignment and Access Control in crAPI

## 🧠 Learning Objectives
- Understand the concept of mass assignment vulnerabilities.
- Identify insecure handling of object initialization and DTO usage.
- Practice exploiting mass assignment to alter restricted application state.
- Recognize the overlap with access control flaws (BFLA).

---

## ⚙️ Prerequisites

Before starting, you should have completed the [Basic Injection Attacks](8_Basic_Injection_Attacks.md) exercises.

> ⚠️ **Ethical Reminder**: These actions should only be tested in controlled lab environments like crAPI. Do not test production systems without explicit permission.

---

## 📘 Introduction to Mass Assignment

**Mass assignment** occurs when an application automatically binds user input to an object without explicitly validating which fields are allowed to be set. This can allow an attacker to overwrite unintended or protected fields—such as status flags or user roles.

This often happens when applications bind data directly to **DTOs (Data Transfer Objects)** and then pass these objects directly into the application's logic or persistence layers. Proper separation and validation must be applied to avoid this.

---

## 🚨 Exploiting Mass Assignment to Change Order Status

In this exercise, you’ll exploit a mass assignment vulnerability in crAPI that allows unauthorized changes to an order's status. This also represents a **Broken Function Level Authorization (BFLA)** vulnerability, since users can perform actions outside their privileges.

### 📌 Steps

1. In the crAPI web application, create a new order (e.g., purchase a wheel).
2. Navigate to **Past Orders** in the interface.
3. Click **Order Details** for any order and **capture the request** using Burp Suite.
4. Inspect the **Allow** header in the response—what HTTP methods are listed?
5. In the response body, identify the current `status` of the order.
6. Change the request method to **PUT**.
7. Modify the request body as follows:
   ```json
   {
     "status": "returned"
   }
   ```
8. Send the modified request.

### 🔎 Observation

If the request succeeds and the status changes to `returned`, you have successfully exploited a mass assignment vulnerability. This also shows that the application does **not** check whether the user is authorized to change the order status.

---

## 🧠 Reflection Questions

- What role does the DTO play in enabling this vulnerability?
- How does the server fail to enforce authorization?
- What mitigation strategies would prevent this mass assignment?
- Why is it dangerous to expose unrestricted update methods to clients?
- How could role-based checks or input whitelisting help mitigate this risk?

---

## ✅ Summary

- Mass assignment allows attackers to manipulate unintended data by injecting fields directly into requests.
- Bypassing server-side business logic or authorization checks can lead to **Broken Access Control**.
- Developers should validate input explicitly, avoid binding directly to DTOs, and enforce **invariance** within business logic.
- Input filtering, role-based access checks, and separating data representation from internal logic are effective countermeasures.
