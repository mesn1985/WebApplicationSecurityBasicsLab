# Juice shop Fuzzing input 

## 🧠 Learning Objectives
- Apply fuzzing techniques to identify injection vulnerabilities.
- Practice using Burp Suite Intruder for input fuzzing.
- Understand how SQL injection might manifest in user inputs.

## ⚙️ Prerequisites
- Completion of the [crAPI Fuzzing Input Exercise](../crAPI/7_Fuzzing_input.md).
- Basic familiarity with Burp Suite Intruder tool.

## Discovering possible Juice shop SQL injection vulnerability with fuzzing

1. Capture a Juice Shop authentication request and send it to Intruder.
2. Create a small SQL injection wordlist with the following payloads:
   
```
'
;
OR 1==1
```
  
3. Use this wordlist to fuzz the password parameter.
4. Observe the responses. Does any response body contain interesting information?  
> 💡 **Hint:** Before starting fuzzing, capture and note a baseline response for the endpoint. This will help you identify unusual or error responses during your fuzzing attempts.

5. Reset the password to a static value.
6. Fuzz the username field with the same wordlist.
7. Identify responses with HTTP 500 status code that contain notable content.

## 🧠 Reflection Questions
- What does an HTTP 500 error indicate in the context of fuzzing inputs?
- How might these responses indicate a vulnerability?
- Why might fuzzing usernames reveal different information than fuzzing passwords?

> 🔍 Note: This exercise identifies potential vulnerabilities. You will attempt to exploit these in a later exercise.
