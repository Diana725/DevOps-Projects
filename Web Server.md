# Web Server
DevOps involves deployment and monitoring of web applications, web services, microservices and other types of applications hosted on web servers. DevOps team has a focus of smoothening operations of mission-critical web applications. Specific skills required are:

* Understanding of layer 7 routing & HTTP protocols
* Ability to execute web application deployments
* Experience with common web servers like Apache and Nginx
* Basic knowledge of common web application stacks and how to configure them

Here I will preview how to set up a simple Web Server to run a simple web application called [UnifiedTransform](https://github.com/changeweb/Unifiedtransform).
## Setting up a Web Server
On your Ubuntu Server, what should be added is an apache webserver to allow it to host a simple website from it. This is often referred to as a LAMP stack.

* Linux Operating System
* Apache Web Server
* mySQL database
* PHP

## Linux
Before performing any software installation on Ubuntu, the first action you need to perform is to update the system repository to ensure that the OS has all of the latest packages available for installation. Run the following command to update the APT package index to the latest version:

```
sudo apt update
```
Do not forget the ````sudo```` command to elevate permissions for a non-priviledged account.

Also, check the firewall settings to make sure that the Apache software installed will be accessible on a public IP address. The UFW(Uncomplicated Firewall) program on Ubuntu lets users manage firewalls on Linux. UFW comes pre-installed on Ubuntu 22.04 but in case you do not have it, run the following command to install it.

```
sudo apt install ufw
```
To view status of ufw, type:
```
sudo ufw status
```
The default policy firewall works great for servers. It is always a good policy to close all ports on the server and open only required ports one by one. To block all incoming connection and only allow outgoing connections use the following commands
```
sudo ufw default allow outgoing
sudo ufw default deny incoming
```
The next logical step is to allow incoming SSH ports. Use:
```
sudo ufw allow ssh
```
Finally turn on the firewall. Use the command:
```
sudo ufw enable
```
If you're using an aws server, you can set firewall rules as security groups, and set outbound rules to any destination while restricting in bound rules to SSH and HTTP.
## Apache2
Apache2 is an open-source HTTP server. It is installed with the following command.
```
sudo apt-get install apache2
```
To confirm that apache2 is installed correctly run
```
sudo service apache2 restart
```
Then using a web browser go to the address of that server. i.e ````http://34.232.76.191/```` It should display the following on your web page:

![Apache2](https://user-images.githubusercontent.com/122386130/229458397-26fee345-3ee4-4e84-9b9c-226ef3d7426c.PNG)

## MySQL
MySQL is a database where data for the website is stored. To get MySQL installed use the command:
```
sudo apt-get install mysql-server
```
## PHP
PHP is a server-side scripting language, we will use this to interact with a MySQL database. The final installation is to get PHP and dependencies installed using
```
sudo apt-get install php libapache2-mod-php php-mysql
```
From here, follow instructions on the [Deploying Laravel Application](https://github.com/Diana725/DevOps-Projects/blob/main/Deploying%20Laravel%20Applications.md) to finish setting up the web application.

When the web application is up and running, it should display:
![Laravel2](https://user-images.githubusercontent.com/122386130/229482313-eb0e0797-1237-4569-8fef-e64f20ab2168.PNG)
![Laravel3](https://user-images.githubusercontent.com/122386130/229482586-d84fb82b-e16c-497e-bde5-481e365410b4.PNG)

