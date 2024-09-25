# Active Reconnaissance With crAPI

Active reconnaissance is about finding the attack surfaces of a system.
In these exercises you will do reconnaissance by find scanning all the ports of
a host, and afterwards scanning for active services on the host.
Then you will enumerate crAPI to determine available url paths.

## prerequisites

The [setup of the lab](../README.md) should be completed.

The wordlists from [Seclist](https://www.kali.org/tools/seclists/) should be install
along with [Gobuster](https://www.kali.org/tools/gobuster/) on a Kali Linux instance(I use [Kali](https://www.kali.org/docs/wsl/wsl-preparations/) on WSL for convenience).
If you use kali with WSL, you can simply can the loopback address(unless of course default settings have been changed). If you use a virtual machine, you should ensure
that a NAT is configured between the host and VM.

If you use WSL Kali Linux, the wordlist used in these exercise can be found on the following paths:
- `/usr/share/wordlists/seclists/Discovery/Web-Content/common.txt`
- `/usr/share/wordlists/seclists/Discovery/Web-Content/quickhits.txt`

If you wish to familiarize yourself with the basics of Gobuster, you can watch this [introduction tutorial](https://www.youtube.com/watch?v=HjXNK-mYwDQ)
  
In the Nmap exercises, all output are sent to XML files. You can review these files with any text editor, but i advise to use something
that provides you with a good overview, such as [vscode](https://code.visualstudio.com/). If you use Kali through WSL, and have vs code installed
on your windows host, you can open a file in vs code simply by typing `code <filename>` in the Kali console. If that for some reason does not work,
the setup is documented [here](https://code.visualstudio.com/docs/remote/wsl).
  
## 1 Nmap all ports
Use [NMap](https://nmap.org/) to [scan all ports](https://nmap.org/book/man-port-specification.html), and send the output to an [xml file](https://nmap.org/book/man-output.html).
This will give a complete overview of all available ports on the host, and the name of the service available on the port.

If you are using the setup provided by this repository, your console output will look somewhat like this:  
![NMap Full port scan](./Images/NmapFullPortScan.jpg)

This of course provides an overview of which ports have an active service, but there is no insight 
to what the service actual is. Next exercise you will have to perform a more detailed scan.

## 2 Nmap service scan
Use [NMap](https://nmap.org/) to perform a scan with the [default scripts and version detection enabled](https://explainshell.com/explain?cmd=nmap+-sC+-sV+-v+)
and output to an [xml file](https://nmap.org/book/man-output.html).
  
The output of the scan will be to big to make sense of in the console. You should use a text editor to review the
output file. For instance i use [vscode](https://code.visualstudio.com/), the setup is documented [here](https://code.visualstudio.com/docs/remote/wsl).
_If you are using Kali on WSL, and you already have vscode installed on the windows host, you can simply type code <Name of output file>_

Once you have opened the output file. Try to identify which port belongs to Juice shop, and which port belongs to crAPI.
_You know this from the docker setup. But the purpose here is to learn how to use port scans obtain information about services_

  
## 3 Enumerating crApi with gobuster and the wordlist common.txt
In this exercise, you will use the enumeration tool [Gobuster](https://www.kali.org/tools/gobuster/) along with the wordlist `common.txt` from [SecLists](https://www.kali.org/tools/seclists/),
to discover available URL paths on crAPI.

Most likely you will encounter two error. The first one is crAPI complaining about a self-signed certificate,
you need to tell Gobuster to [Skip TLS certificate verification](https://3os.org/penetration-testing/cheatsheets/gobuster-cheatsheet/#dir-mode-options).
The second error you will most likely encounter is Gobuster telling you, that crAPI returns [HTTP status code 200](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200)
with non-existing path. Essentially meaning that code 200 OK will always be returned, so every path from the wordlist will seem valid in Gobusters scan. To circumvent this 
error you need [Exclude the length](https://hackertarget.com/gobuster-tutorial/) of the default site. This tells Gobuster to ignore response with a specific content length,
and Gobuster can therefor separate default 200 responses from valid responses. 
  
Gobuster is a very agressive scanner and therefor also very noise (Easily detected by Intrusion dection system). To make Gobuster
less noisy you could use [delay](https://hackertarget.com/gobuster-tutorial/) to set the time between each send request.

Perform the following task:
- Use [Gobuster](https://www.kali.org/tools/gobuster/) to enumerate crAPI with the wordlist common.txt. **Remember that crAPI use the HTTPS protocol**
- Create a file called `crAPI_wordlist.txt` and add the path of all the positive responses to it(status code 200's and 300's).

You are now building your own specific crAPI wordlist using the paths you know have discovered exists.

_At least one of the resulting url paths are interesting, but for now we will just focus on discovery_
  
## 4 Enumerating crAPI with gobuster and the wordlist quickhits.txt
In this exercise you should repeat exercise 3, but use the wordlist `quickhits.txt` instead of `common.txt`.

Perform the following tasks:
- Use [Gobuster](https://www.kali.org/tools/gobuster/) to enumerate crAPI with the wordlist quickhits.txt.
- add the path of all the positive responses to it(status code 200's and 300's) to the `crAPI_wordlist.txt`.
- One of the positive responses lead to an interesting vulnerability. See if you can identify the vulnerability. (You dont have to abuse, just identify it).

Now you have your own specific wordlist for crAPI which you can execute any time. The paths on the wordlist does not lead to vulnerabilities(At least not all of them),
but it provides you with an overview of the available paths. And you even discovered a single vulnerability along the way. 
  
There should be fewer results from using the `quickhits.txt` file. This is because the file contains fewer words and different words than `common.txt`, but the  result
should show that they have at least 1 word in common.
  
## 5 Enumerating crAPI with ZAP.
[OWasp ZAP](https://www.zaproxy.org/) is a open source web analysis tool. Unlike [Burp suite](https://portswigger.net/burp), you don't have to pay for the full functionality of the application.
One of ZAPs uses is automated scanning a web applications, creating a site map and detecting potential vulnerabilities. In this exercise ZAP will be used for discovery against crAPI.

You can familiarize yourself with ZAP, following [this guide](../VariousGuides/SetteingUpZedAttackProxy.md) 
  
For now there are 3 interesting outputs from the automatic scan that we will pay attention to: The Site map (in the left pane), Alerts (shown in the bottom panes), spider (Or Ajax spider, in the bottom panes).

Perform the following tasks:
- Initiate an automated scan (use Ajax spiders) and wait for the scan to complete.
- Review the site tree in the left site. What information does it provide?
- Review the alerts. Can you correlate any of the alerts to the vulnerability you discovered in exercise 4?
- What information can you find in the pane `AJAX Spider`?