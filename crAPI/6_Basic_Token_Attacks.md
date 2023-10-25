# Basic attacks against authentication token in crAPI
The purpose of these exercise are to demonstrate some basic attacks against authentication tokens.
The token type used in these exercises are jwt tokens, these are widely used.

You can get a basic introduction to JWT tokens [here](https://auth0.com/docs/secure/tokens/json-web-tokens).

## prerequisites
Before starting these exercises, you should have completed [basic authentication attacks exercise](5_basic_Authentication_Attacks.md).

You should install the Burp suite extension [JWT editor](https://portswigger.net/burp/documentation/desktop/testing-workflow/session-management/jwts)
_The BApp store used to install extentions, can be viewed under the extensions tab. If the extension tab is not visible, access it by view->Extensions_


## Getting to know the JWT token
In this first exercise, you will learn about the structure of the JWT token.
enter the site: [https://jwt.io/](https://jwt.io/).

The basic format of the JWT token is _header_._payload_._signature_

The header contains meta data about the JWT token. Usually the header contains information about the algorithm used to create
the signature by hashing base64 URL encoded conjugation of the header and the payload, This ensures the neither the header nor
the payload of the token can be forged or altered.
  
The payload of the JWT contains various information about the token, which can be used to determine authentication and authorization.

Both header and payload of the jwt are base64 encoded.

The signature as explained earlier is a hash of base64 URL encoded conjugation of the header and the payload. 
If the header or payload is altered in anyway, the signature will also be altered and therefor no longer be valid.
In the case of HS256 signature the hash is initially created using secret key as input to the hash function, on the server side.
When the server side needs to validate the signature on an incoming token, it again uses the secret key upon the encoded header and payload.
If the recreated hash value matches the value of the signature, the token is assumed to be valid.

perform the following actions in the site [https://jwt.io/](https://jwt.io/):
1. Try altering the value of sub(abbreviation for subject). How much does the signature change?
2. Try altering the value of iat(abbreviation for Issued  At). How much does the signature change?
3. Copy the encoded jwt header, and decode it in a base64 decoder, What values do you see?
4. Copy the encoded jwt payload, and decode it in a base64 decoder, What values do you see?

As you see, the header and the payload simply plain text that is base64 encoded. In some cases, the 
role (which grants authorization) is defined without the jwt. Meaning that the security of the application
using the token, relies fully on the signature of the JWT token.


##### Manipulate the payload
In this exercise you will abuse a vulnerability which allows an attacker to bypass signature verification,
by disabling the JWT signature check. 

For this exercise you should have to user accounts, user A and User Y. (how you name the users is up to you)

1. As an authenticated user A, intercept a GET request for user dashboard and send it to repeater. 
_Notice the installed JWT editor extension highlight all requests with the color green_
2. Go to the repeater.
3. In the repeater, select the _JSON Web Token_ tab,
4. In the payload text field, change the value of `email` to user B's email adresse.
5. Click the send button in the upper left corner.
6. notice that the information in the response code is now User B's information(Information disclosure). This indicates improper token validation (or complete lack thereof)
7. Note the encoded token down for later use.

It would seem that the application does not validate the header and payload hash against
the signature in this part of the application.
Next, we will see if we can create a new password using the manipulated token.

1. Capture a _new password_ request with burp suite and send it to the repeater.
2. Paste in the previous token to the request, and resend it.

The return message is a code 401 stating that the token is invalid, so this approach did not work,
indicating that the token validation works in this part of the application.

