# Basic attacks against authentication token in crAPI
The purpose of these exercise are to demonstrate some basic attacks against authentication tokens.
The token type used in these exercises are jwt tokens, these are widely used.

You can get a basic introduction to JWT tokens [here](https://auth0.com/docs/secure/tokens/json-web-tokens).

## prerequisites
You should have completed the exercise in with [crAPI authentication attacks](../crAPI/6_Basic_Token_Attacks.md),
before starting these exercises.

### Manipulating and abusing unsigned tokens
The purpose of this exercise is to show how lack of server side validation of the jwt's
signature can be abuse. In this exercise you will change the password of the juice shop admin.
First you will determine whether it is possible to use an unsigned token, and thereafter,
you will manipulate the payload of that, to contain the information of the admin account.


1. Create (or re user) a juice shop user.
2. Authenticate yourself with that user, and capture the GET request to `/rest/user/whoami` (The one with response code 200)
3. Send the captured request to the repeater.
4. In the repeater, inspect the content of the request, notice that the authorization token is in both the Header and the Cookie.
  
In the next steps, you'll figure out which of the tokens are used by the server for validation, the one in the header,
or the one in the cookie. You'll do this by "breaking" the token, and resending the request, to see which response you receive.

1. Break the token in the header by adding a character to the token, and send request, does this yield any changes to response?
2. Break the token in the cookie by adding a character to the token, and send request, does this yield any changes to response?
  
What this experiment should show you, is that it is the token in the cookie that the server side uses for validation, when 
GET request are made for the path `/rest/user/whoami`. To prove this. Try to remove Authorization header completely, and resend
the request. You should still at a code 200.
  
In the next step the JWT payload needs to be manipulate, to contain another email  address and id.
You can do this by paste the token into the token field of JWT.io, or using the `Json web token` editor,
in the repeater. The advantage of the ladder, is that you don't need to copy the new token into the request
token manually afterwards.

1. Change the email address in the JWT payload to `admin@juice-sh.op`. (That is the mail address of juice shop administrator)
2. Change the id number in the JWT payload to `1`.
  
The two pieces of information above, shows how seemly unimportant information can be used in an attack. The email address of the juice shop
admin is not confidential, and in combination with the fact that the email address is used along with a password in the credentials when
authenticating towards Juice shop, we know that authentication requires an email and a password.
The user id of the admin is a qualified guess. The User id in the application i assigned inclemently. This could be discovered by creating User A, whom
received the id 50, and thereafter creating User B whom received the id 51. Guessing that admin is the first account created, the id must be 1 (or 0).

1. Resend the request, and confirm that you do not receive response code 200. (304 instead)

Next we will remove the signature from the JWT, and change the used algorithm (defined in the header) to `none`. 
You could do this manually by changing the value of `alg` in the header to `none` and completely remove the signature
from the jwt, so the format would be `<header>.<payload>.`. But there is an easier approach with Burp suite repeater
and the _JWT editor_ extension, which you should use.

1. in the Burp suite repeater request section. Click the `JSON Web Token` 
2. In the lower left corner, click `Attack`
3. Select `none signing Algorithm` from the context menu that appears.
4. Leave the `Algorithm value` as `none` and click ok.
5. Send the request again, and confirm that you receive status code 200 in the response.
6. Note down the token, as you will need to paste it into another request in the following steps.

Now that you have a working unsigned JWT token that contains the administrators id and email address. We will attempt to
perform an _Escalation of privilege_ attack on the admin account, by reusing the unsigned token in a password change request.

1. Capture a change password request, and send it to the repeater.
2. Replace the token in the `Authorization header` and in the `Cookie`, with the unsigned token you created.
3. send the request and confirm that you receive a status code 200 in the request.
4. Copy the password md5 hash from the response code, and crack at. (E.g. Use crackstation.com)
5. Confirm that the Password is the one you created earlier.
6. Log in as juice shop administrator.

With at bit of knowledge about seemingly harmless information, combined with a bit of trial and error manipulation of the JWT token,
this vulnerability was discovered. Showing that multiple mistakes and errors, can lead to big vulnerability (Escalation of privileges is probably the worst case).




