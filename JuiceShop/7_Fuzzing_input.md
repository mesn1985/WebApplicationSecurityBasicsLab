# Juice shop Fuzzing input 
The purpose of this exercise is to repeat the techniques learned with crAPI input fuzzing exercise

## prerequisites
You should have completed the [crAPI Fuzzing input exercise](../crAPI/7_Fuzzing_input.md) prior to starting
this exercise.

## Discovering possible Juice shop SQL injection vulnerability with fuzzing

1. Capture a juice shop login request and sent it to intruder
2. Create a small sql inject wordlist with the follwoing:
```
'
;
OR 1==1
```
3. use the created wordlist to fuzz the password, what responses are there? does the body in any of the responses contain anything intersting?

if there was no interesting responses fuzzing the password, lets try it with the user  name instead.

1. set the password to a static value again
2. fuzz the user name with the created wordlist.
3. which of the response with error code 500 contain something interesting?

We will abuse this potential vulnerability in a later exercise.