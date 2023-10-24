# Basic authentication attacks against crAPI
The purpose of these exercises, is to introduce the basic attacks that can be performed
against user credential authentication. 

## prerequisites
Before starting these exercise you should have completed the 4 previous exercises, to ensure 
basic skills with Burp suite and Kali.

In these exercises you will be using burp suite intruder. You can read about the fundamentals of 
intruder [here](https://portswigger.net/burp/documentation/desktop/tools/intruder/getting-started)


## Dictionary attack with Burp suite
In this exercise you will use use a wordlist of passwords that you have created yourself,
and use that wordlist to execute a dictionary attack against a crAPI's user (The dictionary being your password wordlist).

The approach of this exercise is also described in the section _Running a dictionary attack_ in [this guide](https://portswigger.net/burp/documentation/desktop/testing-workflow/authentication-mechanisms/brute-forcing-passwords)

Perform the following actions:
1. Create a wordlist with 20 passwords of your own choosing that fits the password policy of crAPI
2. Create an user with a password from your password wordlist file.
3. Using burp suite, capture a crAPI login request.
4. Send the login request to Intruder. (Similar to the way you send a request to repeater)
5. Go to intruder
6. select _Sniper_ as _Attack type_ (See Step 4 in the [basic introduction](https://portswigger.net/burp/documentation/desktop/tools/intruder/getting-started)
7. In the content body of the request, set the email value to match the email of the user you created in step 2.
8. In the content body of the request Highlight mark the value of password, and click the add button on the right site (See Step 3 in the [basic introduction](https://portswigger.net/burp/documentation/desktop/tools/intruder/getting-started)
9. switch to _Payloads_
10. Ensure that _Payload set_ are set to 1, and _Payload type_ is set to _Simple list_
11. In the payload Settings, load the password wordlist you create in step 1.
12. In the payload Settings, in the bottom, uncheck _URL-encode these characters_ **Always remember this**
12. In the upper right corner, click _Start attack_
13. Observe which requests return status 200.

To save time, a small list was used. But in an actual attack you would have to use a larger list (such as rockyou.txt).
A simple preventive measure against bruteforce attacks such as Dictionary attack, is max attempts on authentication.

**Extra information**
Burp suite intruder can also be used for enumeration of API's.
Simply set the variable in the URL instead.


## Dictionary attack with WFuzz
WFuzz is a great free tool, which is a lot faster than Burp suite Community edition (Not the payed version).
The purpose of this exercise is for you to have tried the WFuzz tool. And to have tried the dictionary attack
with a password wordlist that is of an realistic size. 

**Once you have started the execution of the attack, you should let the execution run in the background, and proceed with the other exercises, else you will be waiting for along time**

You will need the rockyou.txt file, to obtain it in Kali, execute the following command `wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt`.

The basic usage of Wfuzz is explained [here](https://wfuzz.readthedocs.io/en/latest/user/basicusage.html) and there are also a getting started guide [here](https://wfuzz.readthedocs.io/en/latest/user/getting.html#specifying-a-payload).

Perform the following actions:
1. create a crAPI user with a password from the `rockyou.txt` file.
2. Execute the dictionary attack against crAPI with wfuzz, following requirement should be meet:
    - The content body of the request should be '{"email":  "<email>", "password": "<password>"}'
    - return code 401 should be hidden from the results (hide content)
    - The `Content-Type` header should be set for `application/json`.
    - The wordlist used should 'rockyou.txt'

**Extra information**
WFuzz can also be used for enumeration of API's.
simply set the variable in the URL instead.

## Exhaustive Brute force attack with BurpSuite
In this exercise, you will perform a brute force crAPI authentication, by using random character combinations
as password. This is the least effective attack against password authentication. But given infinite tries (no max attempts) and
infinite time(could be 1000 Years, so not so realistic), this attack will always succeed. Under the previous (Not so realistic) conditions
no password is safe against this attack.

Perform the following actions:

1. Go the website (https://www.grc.com/haystack.htm)[https://www.grc.com/haystack.htm] and see how long the brute force of 
  the password passwd might take (worst case).
2. Add a single alphabetic character. And note how much the time increases.
3. Add a single special character. And note how much the time increases. 

It should now be clear, that this attack i not very effective. But if all other possibilities are exhausted,
this is the only attack approach.

The approach used in the following steps is also described in the section _Running an exhaustive brute-force attack_ in [this guide](https://portswigger.net/burp/documentation/desktop/testing-workflow/authentication-mechanisms/brute-forcing-passwords)

Perform the following actions:

1. Create a crAPI user with the password `passwd` (We know that the front end validation will prevent this, so use the postman request from [exercise 4](4_Endpoint_Analysis_with_postman.md)).
2. With Burp suite, capture an authentication request, and send this to _Intruder_
3. In the content body of the request, set the email value to match the email of the user you created in step 1.
4. In the content body of the request Highlight mark the value of password, and click the add button on the right site.
5. In the _payloads_ tabs, under the _Payload sets_ set the payload type to _Brute forcer_
6. Under _Payload settings_, set the character set to be used as `passwd`.
7. Under _Payload settings_, set the Min and Mx length to 6.
8. Observe the _payload count_ and _request count_ under _Payload settings_, what is the meaning of those?
9. In the upper right corner, click _Start attack_
10. Observe some of the combinations used. Then allow the attack to run in the background, as  this takes a while. (Maybe WFuzz is done with rockyou.txt)


## Password Spraying
The purpose of this exercise is to introduce a technique called password spraying.
When max attempts on authentication is enforced, using the dictionary attack has shortcomings.
But if you create a wordlist of all the known user names, and combine it with a password list
of the most likely passwords. The number of passwords in the wordlist should match the number of max 
tries, e.g. If max attempts are 3, then 3 passwords in the list will be appropriate.

Combining the list of all known usernames with a list of most likely passwords (within the limit of max attempts),
will increase the likely hood of discovering a password.

Perform the following actions:
1. Create a crAPI user (Or reuse one of the previous created users)
2. Create a wordlist of 5 likely passwords (one being the password of the user from step 1)
3. Create a wordlist of all known crAPI users email adresses (Use the Excessive data exposure vulnerability you found in [2 Exploiting BOLA and Excessive data exposure](2_Exploiting_BOLA_And_Excessive_Data_Exposure.md))
4. add the email address of the user from step 1 to the wordlist
5. In Burp suite, capture a login request, and send it to intruder
6. In the content body of the request highlight mark the value of email, and click the add button on the right site.
7. In the content body of the request Highlight mark the value of password, and click the add button on the right site.
8. Set the _Attack type_ to _Cluster bomb_
9. In the _payloads_ tabs, under the _Payload sets_ set the payload set to 1.
10. Set the _Payload type_ to _Simple list_
11. load the user wordlist as payload for _set 1_
12. change to payload _set 2_
13. load the wordlist with most likely password
14. In the upper right corner, click _Start attack_
15. Observer the attempted combination of usernames and password.
16. Find the one(s) that returned status code 200.
