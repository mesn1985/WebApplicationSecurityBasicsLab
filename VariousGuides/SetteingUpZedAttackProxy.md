There are two major tools for proxiyng and analyzing browser and API interaction.
One is the commercial product [Burp suite](https://portswigger.net/burp), which have a 
free community edition with limited functionality(No auto scans, request throttling and so on).
The other one is [Zed attack proxy(ZAP)](https://www.zaproxy.org/) which is an [open source project](https://github.com/zaproxy/zaproxy)
driven by [Open Web Application Security Project(OWASP)](https://owasp.org/), and is therefor free to use, in a fully featured version.  
  
**Software and its documentation change over time. If the guide provided here have become stale, you can always find the latest documentation for ZAP [Here](https://www.zaproxy.org/docs/)**
  
# Prerequisites  
You will perform an automated scan against juiceshop, so prior to following the steps, you should have the lab running, with Juiceshop on port 3000.
You should also have installed either Firefox or chrome browser on your pc. And have it updated to the latest version.

# Basic ZAP usage 
This guide introduces the basic use of ZAP.
The official guide for getting started with ZAP, can be found [here](https://www.zaproxy.org/getting-started/?ref=blog.gitguardian.com).  
   
In my opinion, the major advantage to using ZAP instead of Burp suite, is the price. ZAP comes fully featured for free.
Where as only Burp suite community edition is free, and it have limited features. But Burp suite is far more user friendly for
beginners(Which is the intended audience of this repo). But if you are up for the challenge, ZAP have a lot of great features once
you get the hang of it.  
  
This is merely a very basic introduction to ZAP. Please explorer it further in the (official documentation)[https://www.zaproxy.org/docs/]

## 1. Installing ZAP

- Download the installer for your operating system [Here](https://www.zaproxy.org/download/), and execute the installation wizards.
_Choose the standard install option_

- After the install, start ZAP to verify its correct operation.
_When asked if you want to persist the session, just select no_
  
**Throughout the years, i have experienced various browser driver issues (E.g. Selinium driver). which can cause some problem at times. if you experience any, try to restart your computer**

## 2. Running an automated scan
ZAP can perform automated scan for potential vulnerabilities against web site and the API's used by the web site.
  
**If you experience any driver issues, you should try to restart your pc, if this is the first time running the automated scan after installing ZAP**

 - Select the _Quick start menu_ by clicking _View -> Show tab -> Quick start tab_  
 ![Access quick start menu](./Images/AccessZAPQuickStartTab.jpg)
   
 -  Select the now visible _Quick start tab_, and configure the automated scan as shown in the picture below, and click _Attack_
 ![Configuring Automated scan](./Images/SettingAutomatedScanAgainstJuiceShop.jpg)
 _If you experience any issues related to the choosen browser, try restarting your PC (First time after installing)_
 Note that _Headless browser is used. This ensure that the browser is not acutal opened during the scan_  
 ** If you experience any issues with either Chrome of fire**

- If you click the scanning tab, you can now see the progress of the automated scan. Once completed,   
![Progress of automated scan](./Images/ProgressOfAutomatedScan.jpg)  
  
- If you unfold the _sites_ tree in the left side of the screen, and unfold _127.0.0.1:3000_ you will be able to see all paths successfully retrieved during the scan. This is called a site tree.
![Site tree](./Images/AutoScanSiteTree.jpg)
The automated scan only use request method it can derive from the links found, if there is no indicator, it uses GET request, so this mean that all the path shown in the site tree are mainly GET requests, and it is only the paths that the scan was able to find with 
the available links in the web site. The might be many more available paths. Also it should be noted that i only add paths that are within the 2xx status code range. If an authenticated 
user traversed the web site the same way, there might be more available paths.

- If you click the _spider_ tab, you can see all the request the spiders have made during the traversal of the site. Notice that some have a red marking in the _Processed_ column and is denoted _Out of scope_ i the _flags_ column.
![Spider results](./Images/AutomatedScanSpiderResult.jpg)
The reason why these URL's are marked out of scope, is because they are by default set out of scope, meaning that only links with the baseurl _127.0.0.1:3000_ are used in the website traversal. This is to avoid accidentally flooding
other website or API's with GET requests.

-If you click the _Alerts_ tab, you should be able to see all the security alerts generated during the scan.  
![Alert results](./Images/AutomatedScanAlerts.jpg)  
The security Alerts are potential vulnerabilities discoverd by the automated scan E.g. Missing security headers.
Each alerts can be what is called a false positive, meaning it might no nesceseraly be a vulnerability, so each requires manual
investigation, to determine whether or not it actual are a vulnerability. To avoid false positive, the settings for automated scans 
can be changed, but this comes with the risk om missing actual vulnerabilities. Using multiple tools for automatic scanning can also
prove useful, as each tool have its own strength and weaknesses.

## Intercepting requests with ZAP(Manual scan in ZAP terminology)
Zap can act as a proxy between the browser and all web request sent to API's.
There are two ways to use ZAP as a proxy. Either open a browser directly from ZAP, or 
configure your browser to use ZAP as a proxy (Using a tool such as [Foxy proxy](https://getfoxyproxy.org/))
  
This guide will show you how to use ZAP as a proxy by opening the browser directly from within ZAP.
If you wish to use Foxy proxy instead, an up to date guide should be pretty easy to google.

**If you experience browser driver issues, this is not uncommon. Try to google around for a solution**

 - Select the _Quick start menu_ by clicking _View -> Show tab -> Quick start tab_  
 ![Access quick start menu](./Images/AccessZAPQuickStartTab.jpg)
   
- Click the _Manual Explore_ button in the _Quick start menu_.
 
- In the _Manual Explore menu_, Set the settings as shown in the picture below, and click _Launch browser_
![Manual explore](./Images/ManualExplore.jpg) 

- The Browser of your choosing should now appear showing the juice shop web site.

- If you click on the _history tab_, you see all the HTTP request sent by the browser.
![History tab](./Images/HistoryTabManualScan.jpg)

- If you double click a request in the _history tab_ , you can see the request sent in the upper right corner.
![Sent request](./Images/SentRequestManualScan.jpg)

- Similar you can see the response, by clicking the _Response_ tab
![Request response](./Images/ResponseManualScan.jpg)

- If you wish to resend or edit the request. Right click the request in the _History_, and click _Open/Resend with request editor_
![Resend request](./Images/ResendRequest.jpg)

