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
sudo a2enmod rewrite
```

In my experience with installing Apache2 on an Ubuntu server, I found that the latest PHP version, PHP 8.1, includes an Apache2 module. In this case, its best to simply install PHP without having to go through the Apache2 installation process above.
Here are the commands to go about this process:
```
sudo apt update
sudo apt -y install software-properties-common 
sudo add-apt-repository ppa:ondrej/php
apt update
apt-get install php8.1
```
Then, you can use the command below to install the necessary PHP 8.1 dependencies:
```
apt-get install php8.1-{BCMath,Ctype,curl,DOM,Fileinfo,Mbstring,PDO,Tokenizer,XML,zip,mysql,fpm,gd}
```
No matter which of the above options you use to install Apache2, you'll need to enable the Apache mod_rewrite module using this command:
```
sudo a2enmod rewrite
```
## Step 2: Install MySQL

Next, we'll install MySQL on the server. This is done because Laravel uses MySQL as a dataase backend. That means you'll need to create a database and user for Laravel. 

Install and connect to MySQL using the commands below:
```
apt-get install mysql-server
sudo mysql -u root -p
```
Once logged into MySQL, type in the following commands to set the new password for the root user (replace the $PASSWORD field with your desired password);
```
use myql;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '$PASSWORD';
```
## Step 3: Install PHP
If you had already installed PHP when installing the Apache2 module, skip this step.
To install PHP and its dependancies, run:
```
sudo apt-get install php7.4
apt-get install php8.1-{BCMath,Ctype,curl,DOM,Fileinfo,Mbstring,PDO,Tokenizer,XML,zip,mysql,fpm}
```
Its important to ensure you're working with the correct PHP version, and you can confirm this by running:
```
php -v
```
If for you already had another version of PHP running, say PHP 8.2, and you want to change it to your desired version, you can do this using the following commands:
```
sudo a2dismod php8.2
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

To get your application on the Ubuntu server, you'll need to clone it from github or gitlab. you can do this by using ````git clone```` followed by the ssh link to the repository. You can also specify the brach that you want to clone from using ````git clone -b (branchname)```` followed by the repository's ssh link.

## Step 6: Install Composer

Composer is a depe
