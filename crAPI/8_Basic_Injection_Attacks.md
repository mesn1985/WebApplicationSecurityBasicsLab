# Basic Injection attack

Injection attacks are attacks where malicious actors injects malicious data into software, to make
the software behave in a unintended way, allowing for malious operations on the software.

There are multiple categories of injection attacks, such as SQL injection, code injection,
Command injection etc. There are many categories. 

The purpose of these exercise, is to give an basic introduction to Injection attacks.

The 2 first exercise are NoSQl and SQL Injection attacks, Exploiting the vulnerabilities
discovered in the previous [Fuzzing input exercise](./7_Fuzzing_input.md).

## prerequisites
Prior to starting these exercise, you should have completed the exercises in [Fuzzing input](7_Fuzzing_input.md)


## Basic NoSql Injection attack
In the previous fuzzing exercise, it was discoverd that the statement `{ "$ne": null}` triggered a
particularly interesting response, we will try to exploit this with at NoSql injection attack.

Perform the following actions: 
1. In the crAPI website, attempt to validate a coupon and capture the request.
2. Alter the request, so the coupon code is `{ "$ne": 1 }`
3. Read and verify the response. Note the coupon as you need it for the next exercise.

NoSql is not a syntax we cover in this course, but if you are interested, you can read
about [Mongodb here](https://www.mongodb.com/nosql-explained), which is a NoSql database.
They are very different from relational database, but one of the major difference is that
they do not use SQL as query syntax.


## Basic SQL injection attack - Get database version
In this exercise we will attempt to perform a SQL injection attack, to obtain the version
of the used relational database.
1. in the crAPI, redeem the coupon obtained in previous exercise, and capture the apply coupon request.
2. Alter the request so the coupon code is `0'; select version() --+`
3. Review the reply, and google which type of database the specified database is.

It is not uncommon for web applications to use multiple types of database, e.g. relational and NoSql database.
Obisvisouly this vulnerability are easy to exploit, when you know they exist, but finding these rather simple
vulnerabilities without any prior knowledge would be rather hard. Then it is better to review the code of the 
application, to determine if proper input validation is used, Objects upholds invariance, or either parameterized statements
or ORM are used. All of the before mentioned could mitigate this attack, and in combination with each other, create a strong
safety net (Defense in depth).


## Basic Server side request forgery SSRF
In this exercise you will attempt to get api to make a GET request to google,
by altering the request input. The name of this attack is server side request forgery,
but it still qualify as an injection attack, because it depends on what you send to 
the application. 

No so many clues in  this exercise, so it requires a bit of hacking, but once you find
the solution, you will discover that it is quiet simple.

1. Capture the post request for contact mechanic 
2. find a way to use the post request making the server calls `https://www.google.dk`

This type of exploit can be avoid by keeping a `request allow list` in the application,
ensure that the application can only make request to a selected set of URL's.