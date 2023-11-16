# Basic mass assignment attack

## Obtain admin role 
In this exercise you will use mass assignment to perform an
Escalation of privilege with a user account.

First you must:
- Capture a sign up request
- Review the response in this request, and determine what attribute shows the users role
- Create a new request sign up request, with an extra parameter that sets the role as admin. 

Once you have done that. sign in to the juice page, and confirm that you can
enter the admin page at the url: http:localhost:3000/#/administration
