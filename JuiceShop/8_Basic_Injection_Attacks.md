# Basic injection attacks.

## prerequisites
These exercises are a continuation of the injection attack exercises in [Juice shop basic injection attacks](../crAPI/8_Basic_Injection_Attacks.md).


## Basic SQL injection attack 
In the [Fuzzing input exercise](7_Fuzzing_input.md) you discovered that the user name value is vulnerable to SQL injection attacks.
In this exercise you will exploit that.

Perform the following actions:
1. In the juiceshop web application, authenticate with `'` as username, and the password `123` and capture the response.
2. Inspect the response (Should be error code 500). What type of error is it?
3. Locate the sql string in the response, and copy it to notepad.
4. In the sql string you copied to notepad, replace the value of email with `' OR TRUE --`, what does this sql string do?

Because the application made a printout of the SQL string, it is now very easy to experiment with manipulation of the string.
Therefor such information should not be exposed. Stack traces and error messages are attack vectors.

1. in the juice shop web site, authenticate your self with the username `' OR TRUE --` and the password `123`.
2. Go to account information, which user are you authenticated as?

SQL injection attacks can be mitigated in numerous ways. Either use [Parameterized queries](https://learn.microsoft.com/en-us/aspnet/web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs),
or  [ORM (.Net har Entity framework)](https://learn.microsoft.com/en-us/ef/core/). Furthermore, input validation and upholding objects invariance help mitigate SQL (Or noSql) attacks.

## Perform the sql injection attack with fuzzing
The purpose of this exercise is to perform a repetive exercise of the fuzzing technique.

1. create a small wordlist with a known SQL injection string, including the `' OR TRUE --`.
2. Obtain a response 200 code with a fuzzing attack (Using Burp suite or WFuzz).


## Basic DOM Cross site scripting
DOM XSS is an injection attack on the browser (Client side) where you try to execute malicious
scripts in the browser by injecting javascript code. So initially the backend server(s) or database
is not affected by this attack. The main goal of this types of attacks is to steal data from the 
user of the browser. E.g. Credentials. 

1. in the search bar, input the string `hey` and view the result.
2. in the search bar, input the string ```<h6>hey</h6>``` and view the result

The result shows that the font size in the search result have changed, which means that  
the input is not [HTML encoded](https://www.w3schools.com/html/html_charset.asp) and the
browser therefor have accepted the input string as valid HTML. To prove this do the following:

1. In the search bar, input the string ```&#60;h6&#62;hey&#60;/h6&#62```
2. Review the result, which should be ```<h6>hey</h6>```.

Knowing that the application is vulnerable to XSS, lets try to inject a simple script into
the search field.

1. In the search bar, input the string `<script>alert('You got hacked')</script>`

The result shows up blank. This indicates that inline scripts are disables for this
website(Or the browser disallows it). So we will have to try to inject a script, without using an inline script
tag. Some tags such as `iframe` allows its attributes to be executable scripts, which 
can circumvent disallowed inline scripts. Let try to do that.

1. In the search bar, input the string `<iframe src="javascript:alert('you got hacked')">`.

The [IFrame tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) is a way to embed other
html pages into the current one. The embedded HTML page can be generated or requested through Javascript as a 
convenience, but unfortunately it can also be abused. The `IFrame` tag is not the only tag which allows attributes
to execute Javascript. You could try with `<img src=x onerror=alert('You got hacked')>`.
The mitigation for this, is of course to ensure that the client side(browser) encodes all received input.

This attack is not the most useful. But if you can perform what is known as a stored XSS attack, which
means you get the server to persist your Javascript injection as data, and deliver it to other users,
this can be very useful.