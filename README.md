# AWS-Network-Loadbalancer-for-EC2-Instance


1.	Create EC2 Instance with linux, security group – all tcp
User data : 
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo mkdir -p /var/w ww/html
echo "Welcome to my Facebook on EC2" | sudo tee /var/www/html/index.html > /dev/null
sudo chmod -R 755 /var/www/html
sudo systemctl restart httpd

2.	Change port number 81 in linux that listening to 80
command to open the configuration file in a text editor (like vi or nano):
sudo nano /etc/httpd/conf/httpd.conf
or
sudo vi /etc/httpd/conf/httpd.conf
Look for the following line: Search using ctrl + W
Listen 80
Edit to Listen 81
Then save and exit
Update firewall rule
Since you're changing the port to 81, you need to allow it in the firewall:
Command to remove old port (80)
sudo firewall-cmd --permanent --remove-port=81/tcp 
Command to allow new port (81)
sudo firewall-cmd --permanent --add-port=82/tcp 

Apply Changes    

sudo firewall-cmd --reload                          
Restart the Apache service to apply the changes:
sudo systemctl restart httpd
Verify Apache is Listening on Port 81
sudo netstat -tulnp | grep httpd
Expected output:
tcp6  0  0 :::81   :::*   LISTEN  1234/httpd
If this comes’s port listening to 81. Then check with browser and hit ip.




Fow windows machine 
Create one Instance for Windows machine
This is the path for windows pc
C:\inetpub\wwwroot\
Place the files inside this path, Files inside this will display on Server

Steps to change port number in windows machine
Search>Admin>Windows Tools>Select IIS>Default web page>Bindings>Edit Port>Enter port number >Don’t enter port name >Change&save port>Restart IIS 
Then port number will be changed.
Steps to allow firewall port that changed
Goto firewall and advanced setting>Inbound rules>Bindings>Addport>Give access>Save

Then port number will be allowed by firewall.
Open the browser and hit ec2 instance Ip it will shows the result.
Target group creation :

•	Create target group for Linux machine with port number of Linux that entered
•	Create target group for Windows machine with port number of Windows that entered 
Load Balancer Creation:
•	Create Load balancer 
•	And add Listener for Linux machine created
•	And add Listener for Windows machine created

