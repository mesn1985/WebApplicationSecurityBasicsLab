# 🔐 Basic Attacks Against Authentication Tokens in Juice Shop

## 🧠 Learning Objectives
- Understand the structure and purpose of JWT tokens.
- Explore risks of missing or disabled JWT signature verification.
- Practice manipulating JWT tokens to perform privilege escalation.
- Recognize the impact of weak token validation on authentication security.

---

## ⚙️ Prerequisites
- Completion of the [crAPI Basic Token Attacks](../crAPI/6_Basic_Token_Attacks.md) exercise.  
- Burp Suite installed with the [JWT Editor extension](https://portswigger.net/burp/documentation/desktop/testing-workflow/session-management/jwts).  
- Familiarity with Burp Repeater and basic JWT concepts.
- Burp suite installed, along with JWT editor

---

## 📝 Introduction

JSON Web Tokens (JWTs) contain a header, payload, and signature. The signature ensures the token has not been tampered with by verifying the header and payload using a secret key. If the signature validation is missing or can be bypassed, attackers may forge tokens and escalate privileges.

In this exercise, you will test whether the Juice Shop backend properly validates JWT signatures and exploit this to escalate privileges to the admin user.

---

## 🧪 Exercise Steps

### Token Validation

1. **Create or reuse** a Juice Shop user account.  
2. **Authenticate** with that user and **capture** a GET request to `/rest/user/whoami` (with a 200 OK response, and at JWT token).  
3. Send the request to **Burp Repeater**.  
4. Inspect the request: note the `Authorization` token present in both the **Header** and **Cookie**.  

We notice that the token are included in both a header, and a cookie, so lets investigate which of the token are
actaul used for validation in this path

5. **Modify the token in the Header** by appending a character; resend and observe the response.  
6. **Modify the token in the Cookie** similarly; resend and observe the response.  
7. Try to **Remove the Authorization header completely** and resend; note if the response is still 200 OK, and what is the response?
8. which token the does server use for validation on this request path?, and which does it use for autentication?
9. Change the request back to its original state before moving on.
  
### JWT Payload Manipulation

9. Using the **JWT Editor tab** in Burp Repeater, **change the payload**:  
   - Set `email` to `admin@juice-sh.op`  
   - Set `id` to `1`  (We know from earlier, that user id's are given sequencelly)
10. Resend the request with the modified token and observe the status code (likely 304).

### Using Unsigned Tokens

11. In Burp Repeater’s JWT Editor, click **Attack → none signing Algorithm** to:  
    - Set `alg` to `none` in the JWT header  
    - Remove the signature entirely (token format: `<header>.<payload>.`)  
12. Send the unsigned token request and confirm you receive a **200 OK** response.  
13. **Save this unsigned token admin** for subsequent steps.

### Privilege Escalation: Changing Admin Password

14. Capture a **password change request** and send it to Repeater.  
15. Replace the token in the `Authorization` header and `Cookie` with the **unsigned admin token**.  
16. Send the request and confirm a **200 OK** response.  
17. Extract the **MD5 hashed password** from the response, crack it (e.g., [crackstation.net](https://crackstation.net/)), and verify it matches your chosen password.  
18. Log in as the **Juice Shop administrator** using the new password.

---

## 🔑 Key Concepts

- Changing the `alg` field to `none` disables signature verification, allowing token forgery.  
- Unsigned tokens are a critical security flaw enabling privilege escalation.   
- Proper JWT validation always requires verifying the signature server-side.

---

## 🧠 Reflection Questions

- Why does the server accept unsigned JWTs with `alg:none`?  
- What risks arise from trusting JWT payloads without verifying the signature?  
- How could the server mitigate this vulnerability?  
- What differences arise if the server uses the token from the cookie versus the header?

---

