# What is React
React (also known as React.js or ReactJS) is a free and open-source front-end JavaScript library[3] for building user interfaces based on components. In this file, I'll take you through a step by step guide on deploying React applications on Ubuntu servers.
## Prerequisites
* Apache web server
* nvm (Node Version Manager)
* npm

## Step 1: Install Apache on Ubuntu

Apache HTTP server is the most popular web server in the world. It is a free, open-source, and cross-platform HTTP server providing powerful features which can be extended by a wide variety of modules. This set of commands will enable you to install the Apache Web server on Ubuntu 20.04.:
```
sudo apt-get update
sudo apt-get install apache2
sudo a2enmod rewrite
```
## Step 2: Install NVM
The first thing to do when deploying React Applications is to install npm. A shell script is available for the installation of nvm on the Ubuntu 20.04 Linux system. Open a terminal on your system or connect a remote system using SSH. Use the following commands to install curl on your system, then run the nvm installer script:
```
sudo apt install curl 
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
```
The nvm installer script creates an environment entry to the login script of the current user. You can either log out and log in again to load the environment or execute the below command to do the same:
```
source ~/.profile
```
The nvm installation is successfully completed on your Ubuntu system.
## Step 3: Installing Node using NVM
To install a specific version of node:
```
nvm install 12.18.3
```
You can choose any other version to install using the above command.
## Step 4: Serve on Apache

In this step, you'll have to create a file in the sites-available folder. Use:
```
sudo nano /etc/apache2/sites-available/filename.conf
```
Then, add these configurations:
```
VirtualHost *:9005>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/projectdirectory/build
        DirectoryIndex index.html
        <Directory /var/www/projectdirectory/build>
                RewriteEngine On
                RewriteBase/var/www/projectdirectory/build
                RewriteRule ^index.html$ - [L]
                RewriteCond %{REQUEST_FILENAME} !-d
                RewriteCond %{REQUEST_FILENAME} !-f
                RewriteRule . /index.html [L]
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
## Step 5: Enable the Apache Configuration File:

Use the following commands to enable the confiuration files that you just created:
```
sudo a2ensite filename.conf
sudo service apache2 restart
```
## Step 6: Enable Ports
Enable the port that you want to set up your listener on. You can do this by navigating to the ports.conf file using:
```
sudo nano /etc/apache2/ports.conf
```
and adding ````Listen 10000```` ( I used port 10000 in this case)
## Step 7: Build your Application using NPM
Run the following command to build:
```
npm run build
```
## FINISH
Once you're done with all these steps, copy your server's ip address and search it up using a browser to load your Laravel Application. The page should look like this:
