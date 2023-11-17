# HackerLab Setup

This lab is a collection of intentionally vulnerable applications.
The lab consist of major intentionally vulnerable applications, with a ready to use docker-compose file.

The projects used are:
- Damn vulnerable web application : [https://github.com/digininja/DVWA](https://github.com/digininja/DVWA)
- Juiceshop: [https://github.com/digininja/DVWA](https://github.com/digininja/DVWA)
- Completely ridiculous API: [https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)

## Deploy the hackerlab

Inside the attacklab folder, execute the command `docker-compose -f docker-compose.yml --compatibility up -d`

## Remeber to shutdown the service when not in use.
  
Remeber to shutdown the services when the application are not in use.  
Either use `docker-compose down` from within the attacklab folder, or shutdown it down
in the docker gui.  
  
![Shutdown the lab](./images/Shutdownthelab.jpg)


## General Introduction to the applications

after setup, the general introduction for each of the applications can
be found here:

| Application  | Link  | 
|---|---|
|crAPI|[Getting to know the application](crAPI/1_Getting_To_Know_the_Application.md)|
|Juiceshop|[Getting to know the application](JuiceShop/1_Getting_To_Know_the_Application.md)|
|DVWA|Yet to come|

I recommend starting with the crAPI exercises and once those are completed, you can move on 
to the juiceshop Exercises. both applications contains exercises in the same category and with
the same tool, so the juiceshop exercises as repetitive exercise after completing the crAPI exercises.


  
## Service ports used.

| Service  | Port  | 
|---|---|
| Damn vunerable web application  | 4280  |
| Juiceshop  | 3000  |
|  api.mypremiumdealership.com | 8443  |
|  crapi-identity| Not exposed  (Uncomment in  the docker-compose file)  |
|  crapi-community | Not exposed  (Uncomment in  the docker-compose file)  |
| crapi-workshop  | Not exposed  (Uncomment in  the docker-compose file)  |
|  crapi-web | HTTP: 8888 HTTPS: 8443  |
|  mailhog | 8025  |