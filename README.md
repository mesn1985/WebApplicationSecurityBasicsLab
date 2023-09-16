# HackerLab

This lab is a collection of intentionally vulnerable application.
The lab consist of major intentionally vulnerable application, in a ready to use docker-compose.

The projects used are:
- Damn vulnerable web application : [https://github.com/digininja/DVWA](https://github.com/digininja/DVWA)
- Juiceshop: [https://github.com/digininja/DVWA](https://github.com/digininja/DVWA)
- Completely ridiculous API: [https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)

# Deploy the hackerlab

Inside the attacklab folder, execute the command `docker-compose -f docker-compose.yml --compatibility up -d`

# Remeber to shutdown the service when not in use.
  
Remeber to shutdown the services when the application are not in use.  
Either use `docker-compose down` from within the attacklab folder, or shutdown it down
in the docker gui.  
  
![Shutdown the lab](./images/Shutdownthelab.jpg)
  
  
# Service ports used.

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