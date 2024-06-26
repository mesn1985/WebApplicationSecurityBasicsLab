﻿# Web application security basic introduction lab Setup

The purpose of this lab is to establish an environment suitable for basic introductory exercises in web application security. 
The primary focus of these exercises is on API vulnerabilities, aiming to raise awareness of the potential security gaps that may occur. 
The objective is to create a general understanding of these vulnerabilities and to offer a basic introduction to approaches and tools used for testing. 
It's important to note that these exercises are not intended as part of pentesting training.
  
The lab comprises a collection of intentionally vulnerable applications. 
It consists of several intentionally vulnerable applications, each accompanied by a ready-to-use Docker Compose file.

The projects used in this lab include:
- Juice Shop: [https://github.com/bkimminich/juice-shop](https://github.com/bkimminich/juice-shop)
- Completely Ridiculous API: [https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)"
- Damn Vulnerable Web Application (DVWA): [https://github.com/digininja/DVWA](https://github.com/digininja/DVWA)

# Getting stared


## Setting up the lab
For convenience, the lab is deployed using [Docker Compose](https://docs.docker.com/compose/), requiring both Docker and Docker Compose for usage. For Windows users, you can easily set up the lab by following [the installation instructions for Docker Desktop](https://docs.docker.com/desktop/install/windows-install/), which include the installation of both Docker and Docker Compose.
   
Linux users can follow the instructions for [installing Docker on Linux](https://docs.docker.com/desktop/install/linux-install/) and, subsequently, the guidelines for [installing Docker Compose on Linux](https://docs.docker.com/compose/install/linux/).
  
A Docker desktop setup video guide can be found [here](https://www.youtube.com/watch?v=7y50rZItKCQ)

### Clone the project
You can clone this entire repository to your local PC using [Git](https://git-scm.com/), 
or you can simply copy-paste the contents of the [docker-compose.yml file](./docker-compose.yml) to a file with a .yml extension on your local PC.
  
A guide for cloning a Github repository using git, can be found [here](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)

### Deploy the lab to docker
Inside the folder which contains the docker-compose.yml file (The cloned project from github), execute the command ```docker-compose -f docker-compose.yml --compatibility up -d``` using powershell(Or  Bash for Linux users).
  
You can find the guide [Starting windows powershell here](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/starting-windows-powershell?view=powershell-7.4), and a video for
navigating powershell [here](https://www.youtube.com/watch?v=gd1GT5gfIPk).
  
Now, you should be able to access the JuiceShop website at the URL [http://127.0.0.1:3000](http://127.0.0.1:3000) and the CrAPI website at the URL [https://127.0.0.1:8443](https://127.0.0.1:8443).
  
You can get a full description of the `Docker compose up` command [here](https://docs.docker.com/engine/reference/commandline/compose_up/)
  
### Remember to shutdown the service when not in use.
  
Remember to shutdown the services when the applications are not in use. Either use the command docker-compose down from within the folder that contains the docker-compose.yml file 
or shut them down using the Docker GUI..  
  
![Shutdown the lab](./images/Shutdownthelab.jpg)


## General Introduction to the applications

The general introduction for each of the applications can be found here:

| Application  | Link  | 
|---|---|
|crAPI|[Getting to know the application](crAPI/1_Getting_To_Know_the_Application.md)|
|Juiceshop|[Getting to know the application](JuiceShop/1_Getting_To_Know_the_Application.md)|
|DVWA|Yet to come|

The intention is to start with the CrAPI exercises, and once those are completed, you can move on to the JuiceShop exercises. 
Both applications contain exercises in the same category and with the same tools, so the JuiceShop exercises act as repetitive exercises after completing the CrAPI exercises.

## Service ports used.

| Service  | Port  | 
|---|---|
| Damn vunerable web application  | 4280  |
| Juiceshop  | 3000  |
| api.mypremiumdealership.com | 8443  |
| crapi-identity| Not exposed  (Uncomment in  the docker-compose file)  |
| crapi-community | Not exposed  (Uncomment in  the docker-compose file)  |
| crapi-workshop  | Not exposed  (Uncomment in  the docker-compose file)  |
| crapi-web | HTTP: 8888 HTTPS: 8443  |
| mailhog | 8025  |
