# 🔍 Active Reconnaissance with Juice Shop

## 🎯 Learning Objectives
- Reinforce active reconnaissance skills using industry-standard tools (Nmap, Gobuster, ZAP).
- Identify exposed services, paths, and technologies in a web application.
- Recognize the value of repeated methodology across different environments (crAPI vs. Juice Shop).
- Understand how enumeration can expose vulnerabilities such as FTP misconfigurations or exposed documentation.

---

## ⚙️ Prerequisites

Before starting these exercises, you should have completed all tasks in  
[crAPI - Active Reconnaissance](../crAPI/3_Active_reconnaissance.md).  
You will reuse many of the same techniques and tools in a new context.

---

## 1. 🔍 Nmap Service Scan

This exercise mirrors the approach used in [crAPI Exercise 2 - Nmap Scan](../crAPI/3_Active_reconnaissance.md).

- Run an Nmap scan against the Juice Shop instance.
- Can you determine which port Juice Shop uses?
- What services or protocols are exposed?

> 🧠 **Tip**: Focus on identifying service banners, open ports, and any unexpected services like FTP.

---

## 2. 🧭 Enumerating Juice Shop with Gobuster and `common.txt`

This follows directly from [crAPI Exercise 3 - Directory Enumeration](../crAPI/3_Active_reconnaissance.md).

- Use [Gobuster](https://www.kali.org/tools/gobuster/) to enumerate Juice Shop using the wordlist `common.txt`.
- Record all positive results (status codes 200 and 300) in a new file called `Juiceshop_wordlist.txt`.

> 🧠 Just like in crAPI, keep an eye out for unusual response codes or unexpected resource exposure.

- One of the discovered paths may expose a vulnerability related to the **File Transfer Protocol (FTP)**.
  - Can you identify it?

---

## 3. 🔍 Enumerating Juice Shop with `swagger.txt`

Parallel to [crAPI Exercise 4 - Swagger Enumeration](../crAPI/3_Active_reconnaissance.md).

- Run Gobuster again, this time using the `swagger.txt` wordlist.
- Append new discoveries to your `Juiceshop_wordlist.txt`.
- Visit any newly discovered Swagger/OpenAPI endpoints.

### 🔎 Discussion:
- What information does Swagger expose?
- Why might this be a **security risk**?

---

## 4. 🧾 Enumerating Juice Shop with `quickhits.txt`

This step deepens what you practiced in [crAPI Exercise 5](../crAPI/3_Active_reconnaissance.md).

- Use Gobuster with the `quickhits.txt` wordlist.
- Append any new paths to your wordlist file.
- Investigate the results.

> ❗ What kind of **data leakage** occurs in the discovered paths?

---

## 🧠 Reflection Questions

- How did the results differ from the crAPI enumeration tasks?
- Why is it important to repeat these methods across different applications?
- What do these findings suggest about common developer oversights?
- What defenses could help protect against the vulnerabilities you observed?

