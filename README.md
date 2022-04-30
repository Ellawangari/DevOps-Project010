# DevOps-Project010

****Project Implementation****

The project tasks were to configure a  Nginx Load balancer and creating a domain and securing the domain using certificate . 
  
****Key Takeaways****
-  Creating a domain using freenom and connecting it to an EC2 instance elastic ip addresses. 
-  Learned how what Elastic IP addresses are and Route 53 on AWS and how to add AAA records.
-  Using Nginx Load Balancer instead of Apache.
-  How websites are made secured through SSL using digital certificates from HTTP to HTTPS.

****STEPS****

**Step 1: Configure Nginx as a Load Balancer**

- Launched an EC2 Ubuntu web server called Nginx-LB and open  TCP port 80 and 443(used for secure HTTPS secure connections)
![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/1.PNG)
![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/2.PNG)

- Updated and installed Nginx 
```
sudo apt update
sudo apt install nginx

```

- Edited the `/etc/host` file by adding the two webserver addresses 
![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/6.PNG)

- Also edited the default nginx configuration file `sudo vi /etc/nginx/nginx.conf` by adding the following.
 ```
 upstream myproject {
    server Web1 weight=5;
    server Web2 weight=5;
  }

server {
    listen 80;
    server_name www.domain.com;
    location / {
      proxy_pass http://myproject;
    }
  }

#comment out this line
#       include /etc/nginx/sites-enabled/*;
```
 - Restarted Nginx and made sure the service is up and running.
 ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/7.PNG)
 
 **Step 2:  Register a new domain name and Configure secure connection using SSL/TLS certificates**
 
 - Created a new domain using the freenom domain site.
 - I assigned an Elastic IP address( this is a static IP address that does not change on start and stop of an instance) to my Nginx Load balancer.
 - Added DNS records using Route 53 on AWS.
   ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/4.PNG)
   ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/5.PNG)
    
  - Associated my Domain name with the Elastic ip address.
  - Confirmed that my Nginx Lb could be accessed on the browser using my domain name.
   ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/10.PNG)
   ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/11.PNG)
   
   - Made sure snapd service is up and running using `sudo systemctl status snapd` command
   - Installed certbot `sudo snap install --classic certbot`
   - Ran `sudo certbot --nginx` to ensure the certificate was issued.
       ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/13.PNG)
   - Reloaded my browser again and the site was now secured and assigned with a certicate as below.
       ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/15.PNG)
    
   - Since the certificate lasts only fo 90 days ran a renewal test command `sudo certbot renew --dry-run`
     ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/16.PNG)
    
   - Configure a cronjob to run a command two renew the certifate twice a day by editing the file ``crontab -e`` and adding the following line to the file
     ``**/12 root /usr/bin/certbot renew > /dev/null 2>&1``
   
   
   - ![alt text](https://github.com/Ellawangari/DevOps-Project010/blob/main/Images/17.PNG)
    
 
