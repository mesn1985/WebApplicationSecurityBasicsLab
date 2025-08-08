# Basic Authentication Attacks Against Juice Shop

## 🧠 Learning Objectives
- Understand brute-force authentication attacks.
- Use password wordlists to simulate real-world password guessing.
- Recognize common weak password patterns.

---

## ⚙️ Prerequisites
- Completion of [crAPI Basic Authentication Attacks](../crAPI/5_basic_Authentication_Attacks.md).
- Access to the `best1050.txt` wordlist from SecLists.

---

## 🔐 Brute-Forcing Juice Shop Administrator Password

The email address for the Juice Shop administrator is **admin@juice-sh.op**.  
Your task is to discover the administrator’s password using the password wordlist `best1050.txt` from SecLists.

### 📌 Steps:
1. Intercept a login request in Juice Shop for the administrator.
2. Use a brute-force tool such as Burp Suite Intruder.
3. Set the email to `admin@juice-sh.op`.
4. Use the `best1050.txt` wordlist as the payload for the password field.
5. Launch the attack and observe which password returns a successful authentication (HTTP 200 OK).

---

## 🧠 Reflection
- How many attempts did it take to guess the correct password?
- What password did you find? Would this password meet your organization's security standards?
- What mitigation techniques could prevent this type of attack in a real-world application?
