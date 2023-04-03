# What Is Laravel?
Laravel is a free and open-source PHP framework that comes with tools and resources that help you build robust and modern web applications. It provides all of the features you need as a back-end developer such as routing, validation, caching, queues, file storage and more.

## Deploying Laravel Applications

In this section, you'll find a step by step guide on how to deploy Laravel applications on an Apache Server running on a Linux Server with an Ubuntu operating system.

## Prerequisites

* An operating system running on Ubuntu Linux
* A root user with Sudo privileges
* Command line Interface

## Step 1: [Install Apache on Ubuntu](https://techvblogs.com/blog/install-apache-on-ubuntu-20-04)

Apache HTTP server is the most popular web server in the world. It is a free, open-source, and cross-platform HTTP server providing powerful features which can be extended by a wide variety of modules. This set of commands will enable you to install the Apache Web server on Ubuntu 20.04.:
```
sudo apt-get update
sudo apt-get install apache2
```

In my experience with installing Apache2 on an Ubuntu server, I found that the latest PHP version, PHP 8.1, includes an Apache2 module. In this case, its best to simply install PHP without having to go through the Apache2 installation process above.
Here are the commands to go about this process:
```
sudo apt update
sudo apt -y install software-properties-common 
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt-get install php8.1
```
Then, you can use the command below to install the necessary PHP 8.1 dependencies:
```
sudo apt-get install php8.1-{BCMath,Ctype,curl,DOM,Fileinfo,Mbstring,PDO,Tokenizer,XML,zip,mysql,fpm,gd}
```
No matter which of the above options you use to install Apache2, you'll need to enable the Apache mod_rewrite module using this command:
```
sudo a2enmod rewrite
```
## Step 2: Install MySQL

Next, we'll install MySQL on the server. This is done because Laravel uses MySQL as a dataase backend. That means you'll need to create a database and user for Laravel. 

Install and connect to MySQL using the commands below:
```
sudo apt-get install mysql-server
sudo mysql -u root -p
```
Once logged into MySQL, type in the following commands to set the new password for the root user (replace the $PASSWORD field with your desired password);
```
use myql;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '$PASSWORD';
```
## Step 3: Install PHP
If you had already installed PHP when installing the Apache2 module, skip this step.
Otherwise, you can install PHP and its dependancies by running:
```
sudo apt-get install php7.4
sudo apt-get install php7.4-{BCMath,Ctype,curl,DOM,Fileinfo,Mbstring,PDO,Tokenizer,XML,zip,mysql,fpm}
```
Its important to ensure you're working with the correct PHP version, and you can confirm this by running:
```
php -v
```
If you already had another version of PHP running, say PHP 8.1, and you want to change it to your desired version, you can do this using the following commands:
```
sudo a2dismod php8.1
sudo a2enmod php7.4
sudo update-alternatives --set php /usr/bin/php7.4
```
## Step 4: Install PhpMyAdmin

phpMyAdmin was created so that users can interact with MySQL through a web interface. The following command will help you install and secure phpMyAdmin so that you can safely use it to manage your databases on your Ubuntu system.

```
sudo apt-get install phpmyadmin
```
Once this is done, you'll need to Open the apache2.conf file and include the phpmyadmin apache configuration path at the bottom. You can open the apache2.conf file using
```
sudo nano /etc/apache2/apache2.conf
```
Then add ````Include /etc/phpmyadmin/apache.conf```` at the bottom of this page and save the file. You'll then have to restart Apache for the changes in the config file to take effect:
```
sudo service apache2 restart
```
## Step 5: Clone your Github or Gitlab repository

To get your application on the Ubuntu server, you'll need to clone it from github or gitlab. You can do this by using ````git clone```` followed by the ssh link to the repository. You can also specify the branch that you want to clone from using ````git clone -b (branchname)```` followed by the repository's ssh link.

## Step 6: Install Composer

Composer is a dependency management tool for PHP and was created mainly to facilitate installation and updates for project dependencies. It is a tool that determines the required packages and installs it on your system using the right version based on the project's need. Also, depending on the version of composer that we need, we will have to install php version that is compatible with it. NB - Composer is installed into the folder of the project. In my case the folder of my project was cloned into /var/www/investments-diana

```
cd /var/www/investments-diana
sudo apt-get install composer
composer install
```
## Step 7: Link Storage to Public Disk
The ````public```` disk included in your application's filesystems configuration file is intended for files that are going to be publicly accessible. By default, the ````public```` disk uses the local driver and stores it in ````storage/app/public```` To make these files accessible from the web, you should create a symbolic link from ````public/storage```` to ````storage/app/public````.
Use the command below:
```
php artisan storage:link
```
## Step 8: Change Folder Ownerships and File Permissions
Use the following commands to change folder ownership:
```
cd ..
sudo chown -R www-data:www-data investments-diana
```
Next, use the folowing command to change file permissions:

```
cd investments-diana
sudo chown -R 755 bootstrap/ public/ storage/
```
## Step 9: Create Database
To create a database for you application, use these commands:

```
mysql -u root -p
create database investmentsdiana;
exit
```
## Step 10: Update the .env file
Now, you have to update the .env file by adding database name, app name, gmail credentials, and app url
Start by using 
```
sudo cp .env.example .env
```
Then use a file editor to make the necessary changes using:
```
sudo nano .env
```
## Step 11: Populate Your Database

Use the following commands to populate your database:
```
sudo php artisan migrate
sudo php artisan key:generate
```
## Step 12: Serve on Apache

In this step, you'll have to create a file in the sites-available folder. Use:
```
sudo nano /etc/apache2/sites-available/investments-diana.conf
```
Then, add these configurations (Note that I have set the port number as my own port number, you can change this to suit your preference):
```
<VirtualHost *:9006>
       # The ServerName directive sets the request scheme, hostname and port that
       # the server uses to identify itself. This is used when creating
       # redirection URLs. In the context of virtual hosts, the ServerName
       # specifies what hostname must appear in the request's Host: header to
       # match this virtual host. For the default virtual host (this file) this
       # value is not decisive as it is used as a last resort host regardless.
       # However, you must set it for any further virtual host explicitly.
       #ServerName www.example.com

       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/investments-diana/public

       DirectoryIndex index.php

       <Directory /var/www/investments-diana/public>
               Options Indexes FollowSymlinks Multiviews
               Require all granted
               AllowOverride All
               RewriteEngine On
               RewriteBase /var/www/investments-diana/public

       </Directory>
       # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
       # error, crit, alert, emerg.
       # It is also possible to configure the loglevel for particular
       # modules, e.g.
       #LogLevel info ssl:warn

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined

       # For most configuration files from conf-available/, which are
       # enabled or disabled at a global level, it is possible to
       # include a line for only one particular virtual host. For example the
       # following line enables the CGI configuration for this host only
       # after it has been globally disabled with "a2disconf".
       #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```
## Step 13: Enable the Apache Configuration File:

Use the following commands to enable the confiuration files that you just created:
```
sudo a2ensite investments-diana.conf
sudo service apache2 restart
```
## Step 14: Enable Ports
Enable the port that you want to set up your listener on. You can do this by navigating to the ports.conf file using:
```
sudo nano /etc/apache2/ports.conf
```
and adding ````Listen 9006```` ( I used port 9006 in this case)
## FINISH
Once you're done with all these steps, copy your server's ip address and search it up using a browser to load your Laravel Application. The page should look like this:
![Laravel](https://user-images.githubusercontent.com/122386130/229452379-5ffcd67e-cb2d-427e-8cd4-b7c8770d691d.PNG)
