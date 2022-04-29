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
