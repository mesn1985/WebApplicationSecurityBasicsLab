# Basic mass assignment attack

## 🧠 Learning Objectives
- Understand the concept of mass assignment vulnerabilities.
- Perform escalation of privilege by manipulating user role during sign-up.
- Recognize the risks of insufficient input validation in APIs.

## ⚙️ Prerequisites
- Completion of previous Juice Shop or crAPI exercises on injection and access control.
- Basic familiarity with intercepting and modifying HTTP requests using Burp Suite or similar tools.

## Obtain admin role 
In this exercise you will use mass assignment to perform an
Escalation of privilege with a user account.

First you must:
- Capture a sign up request
- Review the response in this request, and determine what attribute shows the users role
- Create a new request sign up request, with an extra parameter that sets the role as admin. 

Once you have done that, sign in to the juice page, and confirm that you can
enter the admin page at the url: http://localhost:3000/#/administration

## 🧠 Reflection
- How did the server fail to protect against unauthorized role assignment?
- What input validation or backend controls could prevent this attack?
- Why is it important to separate data transfer objects from internal business logic?
