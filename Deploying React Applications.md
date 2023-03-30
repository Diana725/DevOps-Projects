# What is React
React (also known as React.js or ReactJS) is a free and open-source front-end JavaScript library[3] for building user interfaces based on components. In this file, I'll take you through a step by step guide on deploying React applications on Ubuntu servers.
## Prerequisites
* Apache web server
* nvm (Node Version Manager)
* npm

## Step 1: Install Apache2

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
