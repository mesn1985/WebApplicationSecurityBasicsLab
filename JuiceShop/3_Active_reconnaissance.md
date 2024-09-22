# Active reconnaissance with Juice shop
The intention of these exercises, is to use the learned skills in [active reconnaissance with crAPi](../crAPI/3_Active_reconnaissance.md),
with Juice shop
  
## prerequisites
Use should have completed all exercises in [active reconnaissance with crAPi](../crAPI/3_Active_reconnaissance.md) before
starting these exercises.
  
## 1 Nmap service scan
In this exercise you should use same approach as you used in [active reconnaissance with crAPi exercise 2](../crAPI/3_Active_reconnaissance.md).
  
See if you can determine which port juice shop uses, based on the information that NMap outputs.
  
## 2 Enumerating juice shop with gobuster and common.txt
The approach in this exercise is similar to the approach used in the [crAPI enumeration exercises](../crAPI/3_Active_reconnaissance.md).
  
Perform the following task:
- Use [Gobuster](https://www.kali.org/tools/gobuster/) to enumerate juice shop with the wordlist ´common.txt´.
- Create a file called `Juiceshop_wordlist.txt` and add the path of all the positive responses to it(status code 200's and 300's).
  
One of the urls found with the wordlist `common.txt` expose a vulnerability with the use of the `File Transfer Protocol`, can you
find it?
  
## 4 Enumerating juice shop with gobuster and Swagger.txt
  
Perform the following task:
- Use [Gobuster](https://www.kali.org/tools/gobuster/) to enumerate juice shop with the wordlist ´swagger.txt´ .
- Append the discovered URL to the wordlist you created in the previous exercise.
- Go to the discovered URL browser around, what information can you find?
- What is Swagger? and why can could it lead to a potential vulnerability?
  
## 5 Enumerating juice shop with gobuster and quickhits.txt
Perform the following task:
- Use [Gobuster](https://www.kali.org/tools/gobuster/) to enumerate juice shop with the wordlist `quickhits.txt´ .
- Append the discovered URL to the wordlist you created in the previous exercise.
- What data leaks in the discovered path?
  
## 6 Enumerating juice shop with Zap.
Use an approach similar to [crAPI enumeration exercise 5](../crAPI/3_Active_reconnaissance.md).
Are there any interesting alerts? Explorer the site map, are there any interesting paths?