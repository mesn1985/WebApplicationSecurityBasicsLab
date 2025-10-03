# Basic Authentication Attacks Against crAPI

## 🏁 Learning Objectives
- Understand and execute common authentication attacks such as dictionary, brute-force, and password spraying.
- Explore password recovery vulnerabilities via weak OTP implementations.
- Practice automating attacks using tools like Burp Suite, WFuzz, and scripting.

---

The purpose of these exercises is to introduce basic attack techniques against user credential authentication. You will use various tools and methods to evaluate the robustness of crAPI’s authentication mechanisms.

---

## 🔧 Prerequisites

Before starting these exercises, you should have completed the 4 previous ones to ensure basic skills with Burp Suite and Kali Linux.

You will be using **Burp Suite Intruder**. A guide to its fundamentals is [here](https://portswigger.net/burp/documentation/desktop/tools/intruder/getting-started).

---

## 🔐 Dictionary Attack with Burp Suite

In this exercise, you will use a **wordlist of passwords** that you create yourself and run a dictionary attack against a crAPI user. The idea is to try multiple likely passwords until the correct one is found.

The approach is explained in [Running a dictionary attack](https://portswigger.net/burp/documentation/desktop/testing-workflow/authentication-mechanisms/brute-forcing-passwords).

### 📌 Steps:
1. Create a wordlist with 20 passwords of your own choosing that comply with the crAPI password policy.
2. Create a user whose password is included in your wordlist.
3. Capture a crAPI login request using Burp Suite.
4. Send the request to **Intruder** (like sending it to Repeater).
5. Go to the Intruder tab.
6. Set **Attack Type** to _Sniper_.
7. In the request body, set the email to match the user from step 2.
8. Highlight the value of the `password` field and click **Add**.
9. Switch to the **Payloads** tab.
10. Set _Payload Type_ to `Simple list`.
11. Load your wordlist from step 1.
12. ⚠️ Uncheck the **"URL-encode these characters"** box at the bottom. _This prevents Burp from altering the password format._
13. Click **Start attack**.
14. Observe which request(s) return HTTP status **200 OK** and confirm successful login.

> **Expected Outcome**: One of the requests returns a 200 OK status, indicating the correct password was guessed.

---

## 🚀 Dictionary Attack with WFuzz

**WFuzz** is a faster, free tool than Burp Suite Community edition. This exercise involves launching a realistic-sized dictionary attack using the `rockyou.txt` wordlist.

📌 Download rockyou.txt on kali:
```bash
wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
```
  
More info: [WFuzz basic usage](https://wfuzz.readthedocs.io/en/latest/user/basicusage.html)

> **Tip**: If you are having troubles with getting WFUZZ to work probably, you can start out with a list smaller then rockyou.txt.

### 📌 Steps:
1. Create a crAPI user with a password found in `rockyou.txt`.
2. Execute the dictionary attack using the following parameters:
   - Request body format: `{"email": "<email>", "password": "<password>"}`
   - Hide HTTP 401 responses.
   - Set `Content-Type: application/json` header.
   - Use `rockyou.txt` as the wordlist.

> **Tip**: Let the attack run in the background while doing other tasks—it may take a while.

---

## 💥 Exhaustive Brute-Force Attack with Burp Suite

This exercise demonstrates brute-forcing by attempting every possible character combination.

### 📌 Before You Begin:
1. Visit [https://www.grc.com/haystack.htm](https://www.grc.com/haystack.htm).
2. Enter the password `passwd`.
3. Add a single alphabetic character and observe how time increases.
4. Add a special character and see the difference.

> **Insight**: A weak password increases brute-force feasibility. Longer and more complex passwords drastically reduce success chances.

### 📌 Steps:
1. Use Postman to create a user with the password `passwd` (bypassing frontend validation, as shown in [Exercise 4 - Endpoint Analysis](4_Endpoint_Analysis_with_postman.md)).
2. Capture the login request in Burp Suite.
3. Send it to Intruder.
4. Set the email to match the user created.
5. Highlight the value of the `password` field and click **Add**.
6. In _Payload Type_, select **Brute forcer**.
7. Set character set to `passwd`.

> Note: This does not mean you're brute-forcing the password `passwd`. Instead, you're using the individual characters in "passwd" (p, a, s, w, d) to generate combinations. This is just a shortcut for illustration purposes.

8. Set min and max length to 6.

9. Observe the payload count and request count — this is the total number of combinations Intruder will try.
10. Click **Start attack** and let it run.

> **Expected Outcome**: The correct password is found through exhaustive enumeration (eventually).

---

## 🧪 Password Spraying

This exercise covers **Password Spraying**, which is useful when max login attempts are enforced.
  
The approach divides each password attempt across multiple users. This means it tries a single password with user A, then user B, then user C, and so on. By distributing attempts in this way, it avoids triggering account lockouts and increases the likelihood of success against systems that enforce a maximum number of login attempts per user.

### 📌 Steps:
1. Create or reuse a crAPI user.
2. Create a short wordlist of 5 likely passwords (include the one for the user created in step 1).
3. Get user data from the EDE vulnerability found in [Exercise 2 in Exploting BOLA and Excessive data exposure](2_Exploiting_BOLA_And_Excessive_Data_Exposure.md) to gather 4 users email adresses.
4. Add the email adress you created in step 1 along with the email adresses found in step 3 to a wordlist.
5. Capture a login request and send to Intruder.
6. Set **Attack type** to _Cluster bomb_
7. Highlight the value of the `email` field and click **Add**. (This will be set 1 when you set the payload)
8. Highlight the the value of the `password` field and click **Add**. (This will be set 2 when you set the payload)
9. Set Payload Set 1 to the email wordlist. (There is a dropdown menu to select this in the payload tab)
10. Set Payload Set 2 to the password list.
11. Uncheck "URL-encode these characters" in both payloads.
12. Click **Start attack**.
13. Look for requests returning **HTTP 200 OK**.

> **Expected Outcome**: At least one username-password combination results in successful login.

---

## 🔓 Attacking Password Recovery & OTP
> Burp suite commmunity edition, is very slow, so you could choose to use Wfuzz or ZAP for step 3
This exercise explores exploiting weak OTP and legacy API behavior.

### Step 1: Understand crAPI's OTP Password Recovery
1. Create a user.
2. Use the **Forgot Password?** feature.
3. Input email and leave OTP page open.
4. Open **Mailhoq**, get the OTP.
5. Enter OTP and new password.
6. Log in to confirm password reset.

> **Expected Result**: You’ve changed the user’s password using OTP.

---

### Step 2: Test OTP Strength
1. Request 3 new OTPs.
2. Check if all are 4-digit numeric codes.

> **Expected Outcome**: All OTPs are weak, 4-digit numbers.

---

### Step 3: Test OTP Brute Force Attempt
1. In the browser, Request An OTP again.
2. Enter wrong OTP in the browser, and capture request to `/identity/api/auth/v3/check-otp`.
3. Resend 10x using Burp suite Repeater, and observe status change to 503.
4. Check the error message.

> **Note**: While a `503` error is suppose to indicate a server issue, here it signals that the API has enforced a max-try lockout mechanism.

---

### Step 4: Bypass with Legacy API Version
1. Modify path from `/v3/check-otp` to `/v2/check-otp`.
2. Send 10+ bad requests.
3. Confirm no 503 response, meaning that v2 does **not** enforce limits.
  
When no limits are enforced, we can try to brute for the OTP. OTP's are just passwords, and we know that 4 digit make for a weak password.
---

### Step 5: Create OTP Wordlist
We will create a wordlist with all posible OTP combination, from 0000 to 9999.
This can be done with BASH with:

```bash
for i in $(seq -f "%04g" 0 9999); do echo $i; done | iconv -f UTF-8 -t UTF-8 > wordlist.txt
```
  
Or powershell with:
  
```powershell
0..9999 | ForEach-Object { $_.ToString('D4') } | Out-File -FilePath wordlist.txt -Encoding utf8
```

---

### Step 6: Brute Force OTP
> Burp suite community is "throttled", meaning it is slow. Alternative free tools like **ZAP** can be used. 

1. Send the OTP check request with legacy path to `/v2/check-otp`, to Burp suite intruder.
2. Use  OTP wordlist to bruteforce the OTP.
3. Look for a **200 OK** response.

> **Expected Outcome**: OTP is brute-forced successfully and password is changed.


---

## ​​ Ethical Reminder

These authentication attack techniques can have real-world implications and should **only** be performed in authorized, isolated environments like your crAPI lab setup.  
Never attempt these methods against production systems, third-party services, or client infrastructure without **explicit permission**.

---

## ​ Reflection Questions

- Which attack method was the most efficient?
- What would happen if the API enforced rate limits and lockouts?
- How can developers defend against these attacks?
- Should all parts of a legacy API implmentations be allowed to execute parralel with non legacy implementations?
