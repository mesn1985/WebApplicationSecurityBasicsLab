# Basic Injection Attacks

## 🧠 Learning Objectives
- Understand the basics of SQL injection and how error messages can reveal vulnerabilities.
- Practice exploiting SQL injection to bypass authentication.
- Explore DOM-based Cross-Site Scripting (XSS) and understand how client-side injection works.
- Learn common XSS payload types and how they differ.
- Understand mitigation techniques for SQL injection and XSS vulnerabilities.
- Recognize ethical considerations when testing for vulnerabilities.

---

## ⚙️ Prerequisites
- Completion of [Juice Shop Fuzzing Input](../JuiceShop/XX_Fuzzing_Input.md) and [Basic Injection Attacks with crAPI](../crAPI/8_Basic_Injection_Attacks.md).
- Familiarity with Burp Suite or similar interception and fuzzing tools.

---

## Basic SQL Injection Attack

SQL Injection is an attack where an attacker manipulates SQL queries by injecting malicious input, often exploiting unescaped user inputs.

### Steps

1. In Juice Shop, attempt to login with username: `'` and password: `123`. Capture the response.
2. Inspect the response; it should be an error (usually HTTP 500). Note the error type.
3. Look for the SQL query printed in the error message; copy it for analysis.
4. Replace the email value in the SQL query with: `' OR TRUE --` and consider what effect this has on the query.
5. Login again using the username: `' OR TRUE --` and password: `123`.
6. Check which user you are authenticated as.

> ⚠️ Exposing SQL queries or stack traces is a serious security flaw as it aids attackers.

### Fuzzing with SQL Injection Payloads

1. Create a small wordlist including `' OR TRUE --`.
2. Using Burp Suite Intruder or WFuzz, fuzz the login username or password field with this list.
3. Observe responses; look for HTTP 200 or other anomalous responses indicating injection success.

---

## Basic DOM Cross Site Scripting (XSS)

DOM XSS is a client-side vulnerability where malicious scripts execute in the browser by injecting JavaScript code through input fields, potentially stealing user data.

### Steps

1. Search for `hey` and observe the result.
2. Search for `<h6>hey</h6>` and observe the change in font size, indicating HTML is not encoded.
3. Search for `&#60;h6&#62;hey&#60;/h6&#62;` to see the raw HTML output.
4. Try injecting a script tag: `<script>alert('You got hacked')</script>`. Observe that it does not execute.
5. Inject an iframe with JS payload: `<iframe src="javascript:alert('You got hacked')">` and see if it executes.
6. Try an image tag with an error event handler: `<img src=x onerror=alert('You got hacked')>`.

### XSS Mitigations

- Properly encode all user inputs before rendering.
- Use Content Security Policy (CSP) headers to restrict executable sources.
- Sanitize input to remove dangerous tags or attributes on client side, and reject invalid data on server side.

---

## Ethical Reminder

Only test vulnerabilities on authorized, isolated environments like Juice Shop. Unauthorized testing is illegal and unethical.

---

## 🧠 Reflection Questions

- Why is exposing SQL queries in error messages dangerous?
- How does the `' OR TRUE --` payload bypass authentication?
- What differentiates DOM XSS from traditional server-side XSS?
- How do CSP and input sanitization mitigate XSS?
- What ethical considerations should guide security testing?

