# Getting to Know the Application

### 🎯 Objective
Familiarize yourself with the crAPI application from a legitimate user’s perspective.  
This understanding will serve as the baseline for identifying abnormal or insecure behavior in later exercises.

## Prerequisite
The hacker lab setup must be completed, as specified in the [Readme file](../README.md)

## 🌐 Accessing crAPI

By default, crAPI runs on port `8888` using HTTP.  
You can access it at [http://127.0.0.1:8888/login](http://127.0.0.1:8888/login).  

If you exposed the HTTPS interface (port `8443`), you can also use:  
[https://127.0.0.1:8443/login](https://127.0.0.1:8443/login) *(may trigger browser security warnings due to self-signed cert)*.

> 💡 **Troubleshooting:** If the site doesn’t load, make sure the Docker lab environment is running using `docker compose up`.

---

### 📝 Signing Up

Start by signing up with a new user account.

![Login screen where you enter your credentials](../images/crAPI/LoginScreen.jpg)  
*Login screen where you enter your credentials*

![Signup page to register a new user](../images/crAPI/Signup.jpg)  
*Signup page to register a new user*

Once signed in, you will be directed to the application’s main page, which features three top navigation tabs.  
Explore the application freely to get a sense of its features and flow.

---

### 📬 Checking MailHog

After signup, visit the MailHog email simulation service:  
[http://127.0.0.1:8025/](http://127.0.0.1:8025/)

![MailHog dashboard showing inbox](../images/crAPI/MailHoqService.jpg)  
*MailHog dashboard showing inbox*

![Welcome mail with VIN and PIN code](../images/crAPI/MailHoqWelcomeMail.jpg)  
*Welcome mail with VIN and PIN code*

> 📝 **Note:** Write down the VIN number and the PIN code from the welcome email — you’ll need them to register your vehicle.

---

### 🚗 Adding a Vehicle

Go to the dashboard in the crAPI interface.

![Form where you input VIN and PIN](../images/crAPI/DashboardAddVehicle.jpg)  
*Form where you input VIN and PIN*

Enter the credentials from the welcome email:

![Successful vehicle registration](../images/crAPI/AddVehicle.jpg)  
*Successful vehicle registration*

You should now see your vehicle's image, details, and current location:

![Vehicle details and location view](../images/crAPI/VehicleInfo.jpg)  
*Vehicle details and location view*

---

### 🧭 General Use Case Overview

![Use case diagram of crAPI functionality](../images/crAPI_Use_Case_Diagram.drawio.svg)  
*Use case diagram of crAPI functionality*

---

### 🔍 Reflect & Explore

Spend some time interacting with the app and consider the following:

- What features are available to you as an authenticated user?
- Can you add or remove vehicles?
- What kind of information is visible about your vehicle?

This exploration will help you build a mental model of how the application is intended to work — useful knowledge for identifying unintended behaviors later on.

---

### ✅ By the end of this exercise, you should:
- Understand the signup and login flow in crAPI.
- Know how to interact with MailHog and extract credentials.
- Be familiar with VIN/PIN vehicle registration workflow.
