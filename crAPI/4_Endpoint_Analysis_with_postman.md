# 📬 Endpoint Analysis with Postman

### 🎯 Objective
Understand how Postman can be used for structured API testing and endpoint analysis.  
Learn how to capture and replay requests, manipulate authentication headers, and pair Postman with Burp Suite for enhanced inspection.

This exercise serves both as a Postman introduction and a repetition of prior techniques using Burp Suite.

---

## ✅ Prerequisites

- Exercises [2 – Exploiting BOLA and Excessive Data Exposure](2_Exploiting_BOLA_And_Excessive_Data_Exposure.md) and [3 – Active Reconnaissance](3_Active_reconnaissance.md) should be completed.
- Install [Postman](https://www.postman.com/downloads/).
- Create a free Postman account — some features are limited for unauthenticated users.
- Familiarize yourself with Postman basics using [this guide](https://learning.postman.com/docs/getting-started/first-steps/sending-the-first-request/).
- Reference for collections: [Using Postman Collections](https://learning.postman.com/docs/collections/using-collections/)

> 💾 **Important:** Postman does **not auto-save**. Click **Save** after every modification to avoid losing work.

---

## 1 – Create a Collection in Postman

In this task, you'll create a reusable collection of HTTP requests for crAPI.

🧪 **Do this:**
1. In Postman, click the **Collections** tab (left panel).
2. Click the `+` next to **Collections** and select **Blank Collection**.
3. Rename it to **crAPI**.

---

## 2 – Capture the Signup Request

You'll use Burp Suite to intercept the signup request and replicate it in Postman.

🧪 **Do this:**
1. Use Burp Suite to capture the **POST /signup** request during user registration.
2. In Postman, create a new **POST** request inside the `crAPI` collection named `signup`.
3. Paste the intercepted **URL** into the Postman request.(Burp suite only have the path, remember to append protocol, ip and port number:)
4. Copy the **JSON body** from Burp and paste it into Postman’s request body:
   - Set body type to **raw**
   - Set format to **JSON**
5. 💾 **Save** the request.
6. Click **Send**.
   - If you get `403 Forbidden`, modify the values until you receive `200 OK`.

> 🔁 **Reflection:** Why is it helpful to replicate captured requests in a structured tool like Postman?

---

## 3 – Bypass Frontend Validation

Test if password policies are enforced only on the frontend (browser) or also on the backend API.

🧪 **Do this:**
1. In Postman, modify the `signup` request.
2. Set a password that breaks the visible frontend policy (e.g., only 4 letters).
3. Click **Send** and analyze the API response.

> 🔍 **Reflection:**  
> What does the API allow that the frontend UI blocks? What does that imply about backend validation?

---

## 4 – Add Authorization Token to Postman

Obtain a bearer token from Burp and use it in Postman to authenticate future requests.

🧪 **Do this:**
1. Use Burp to capture the **POST /login** request and its **response**.
2. Copy the **token** value (exclude quotes).
3. In Postman, open the **crAPI collection** → click the **three dots** → select **Edit**.
4. Go to the **Authorization** tab.
5. Select **Bearer Token** and paste the token.
6. 💾 **Save** the collection.

> 🔐 You’ve now configured all requests in the collection to use this token automatically.

---

## 5 – Post to the Community Board

Use Postman to add a post to crAPI’s community section, verifying that authorization works.

🧪 **Do this:**
1. Capture the **POST /community/post** request via Burp.
2. In Postman, create a **POST** request in the `crAPI` collection named `post_to_community`.
3. Paste the URL and JSON body from the captured request.
4. Click **Send** and confirm that you receive a `200 OK`.
5. 💾 **Save** the request.
6. Check the crAPI UI to confirm that the post appears.

> 🔁 **Reflection:**  
> How does this workflow compare to testing via browser forms?

---

## 6 – Create a Collection by Capturing Browser Traffic

Postman can automatically generate requests by intercepting browser traffic using the **Interceptor extension**.

🧪 **Do this:**
1. Install [Postman Interceptor](https://learning.postman.com/docs/sending-requests/capturing-request-data/interceptor/).
2. Create a new collection named **crAPI-Automated**.
3. Open the crAPI app in the browser.
4. Activate Interceptor to begin capturing.
5. Interact with crAPI:
   - Signup and login
   - Register a vehicle (using MailHog)
   - Use `Contact Mechanic`, `Refresh Location`, buy an item, view past orders
   - Create a community post
6. Stop Interceptor capturing.
7. In Postman, select all captured requests → click **+ Save Requests**
8. Save to `crAPI-Automated`, organized by **domain** and **endpoints**.

> ⚠️ **Note:** Captured requests include bearer tokens in headers.  
> You can replace the token by setting it at the collection level, as in step 4.

> 🔁 **Reflection:**  
> How could automated capture help you quickly prepare for a penetration test or bug bounty?

---

## 7 – Use Burp Suite as a Proxy for Postman

Pair Postman’s structured request generation with Burp Suite’s deep inspection features.

🧪 **Do this:**
1. In Postman: go to **File > Settings > Proxy**
2. Enable **Custom Proxy Configuration**
3. Set:
   - **Server:** `127.0.0.1`
   - **Port:** `8080` (default Burp proxy port)
4. In **General Settings**, disable **SSL certificate verification**
5. Open Burp Suite and ensure the proxy is listening.
6. In Burp’s **HTTP History**, verify that Postman requests are now visible.

> 🔁 **Reflection:**  
> Why might combining Postman and Burp be more powerful than using either alone?

---

### 🧠 Final Reflection

1. What advantages does Postman offer over browser-based testing?
2. What does the ability to bypass frontend validation suggest about API security?
3. How can token-based authorization be manipulated during testing?
4. What are the risks of capturing live traffic with Interceptor or a proxy?
5. How can you ensure your token use and captured requests do not accidentally leak?

---

### ⚖️ Ethical Reminder

These techniques should **only** be used in controlled environments with **explicit permission**.  
Interception and token manipulation must never be done on production systems, client environments, or third-party services without authorization.

