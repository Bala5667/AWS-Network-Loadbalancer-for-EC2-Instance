# AWS-Network-Loadbalancer-for-EC2-Instance

Primary Objective:
Launch EC2 Instances: Deploy Linux and Windows instances with appropriate security groups.
Configure Web Servers:

Linux: Install Apache, serve content, and change the listening port (e.g., 80 → 81).

Windows: Configure IIS, serve content, and change the port (e.g., 80 → 8082).
Update Firewall Rules: Allow the respective ports in both Linux (firewall-cmd) and Windows (Windows Defender Firewall).
Create Target Groups: Register instances with health checks for Linux and Windows.
Set Up Network Load Balancer:

Create listeners for both Linux and Windows instances.

Ensure requests are routed correctly based on configured ports.
Testing: Access the NLB DNS and verify traffic distribution across both instances.

Steps to achieve the above Objective:

1. EC2 Instance Setup (Linux)
I created an Amazon Linux EC2 instance with a security group allowing all TCP traffic and used the following user data script to install and configure Apache:

#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo mkdir -p /var/www/html
echo "Welcome to my Facebook on EC2" | sudo tee /var/www/html/index.html > /dev/null
sudo chmod -R 755 /var/www/html
sudo systemctl restart httpd

2. Changing Apache Port (Linux)
To change the Apache listening port from 80 to 81, I modified the httpd.conf file:

Open the configuration file:

sudo nano /etc/httpd/conf/httpd.conf
Found and changed:

Edit
Listen 80  
to

Listen 81  
Saved and exited the file.

Updated firewall rules:

sudo firewall-cmd --permanent --remove-port=80/tcp  
sudo firewall-cmd --permanent --add-port=81/tcp  
sudo firewall-cmd --reload  

Restarted Apache to apply changes:

sudo systemctl restart httpd  
Verified if Apache is listening on port 81:

sudo netstat -tulnp | grep httpd  
Then, accessed the server using the EC2 Public IP:81 in the browser.

3. EC2 Instance Setup (Windows)
I created a Windows EC2 instance and placed website files inside the default IIS root directory:

C:\inetpub\wwwroot
Files inside this path will be displayed on the web server.

4. Changing IIS Port (Windows)
Opened IIS Manager → Selected Default Website → Bindings → Edit Port

Changed the port number and saved the settings.

Restarted IIS to apply changes.

5. Configuring Windows Firewall to Allow New Port
Opened Windows Firewall & Advanced Settings

Added an Inbound Rule to allow traffic on the newly assigned port.

6. Target Group Creation
Created Target Group for Linux EC2 instance with its configured port (81).

Created Target Group for Windows EC2 instance with its configured port.

7. Load Balancer Configuration
Created an Application Load Balancer

Added Listeners for both Linux and Windows target groups

After creating the load balancer, I accessed the Load Balancer DNS from my PC:

http://<Load-Balancer-DNS>
Verified that traffic was correctly distributed between the Linux (Apache - Facebook) and Windows (IIS - Instagram) instances.
Refreshed the page multiple times to ensure requests alternated between both instances.
Confirmed both target groups were Healthy in AWS Console.
Successfully tested load balancing functionality.








