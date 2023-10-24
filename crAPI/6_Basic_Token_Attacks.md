# Basic Token  attacks against crAPI

## prerequisites
Before starting these exercises, you should have completed [basic authentication attacks exercise](5_basic_Authentication_Attacks.md).



#### Forging tokens

Manual load analysis Burp
    - Create Script to obtain 100 tokens
Live Capture  Token Burp
Decode a captured JWT
CLI tool _jwt_tool_
None and switch attack
JWT Cracking

### Manipulating tokens
Capture /rest/user/whoami with JWT in header and body.

##### Using Invalid signature
https://portswigger.net/burp/documentation/desktop/testing-workflow/session-management/jwts
JWT Editor  https://portswigger.net/burp/documentation/desktop/testing-workflow/session-management/jwts
https://medium.com/@riteshs4hu/owasp-crapi-walkthrough-baec54214f85
Install web token extention
Intercept a user dashboard request
Send it to repeater.
change the alghorithm to none. (signature removed)
Resend the request with another users email

