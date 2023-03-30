# What is Rsync?
Rsync is a Linux-based tool that can be used to sync files between remote and local servers.

The purpose of creating a mirror of your Web Server with Rsync is if your main web server fails, your backup server can take over to reduce downtime of your website. This way of creating a web server backup is very good and effective for small and medium size web businesses.

## Using RSYNC

This is a step by step set of instruction on how to use Rsync to sync data between two web servers. The purpose of creating a backup web server is if your main web server fails, the backup web server can take over to reduce downtime of your website. In this case, we were transferring data from a localized web server to another due to preventive maintenance. This is way of creating a backup web server is very good and effective for small and medium size web businesses.

Main Server:
* 192.168.1.9
Backup Server:
* 192.168.1.5

### Step 1:Install Rsync

Install Rsync on both servers with the following command.
```
apt-get install rsync
```
NB: **Ensure you are a root user** and if not, use the following command to create a root user and password:
```
useradd <username>
passwd <password>
```
### Step 2: Setup SSH Passwordless Login

Using password-less login with SSH Keys will increase the trust between two linux servers for easy file synchronization or transfer.

We first generate a public and private key on the 192.168.1.5 server with the following command:
```
ssh-keygen -t rsa -b 2048
```

When prompted to enter a passphrase, don't provide one and hit ````ENTER```` to proceed without one.

Now once the public and private keys have been generated, we will share it with the 192.168.1.9 server so that the main server will recognize this backup machine and will allow it to login without asking any password while syncing data. Navigate to where the public key is saved, normally the /root/.ssh/ folder and use this command:

```
ssh-copy-id ./id_rsa.pub <username>@192.168.1.9
```
### Step 3: Synchronize/Transfer the files
The typical rsync format isn't very intimidating when broken down.
```
rsync [option] [origin] [destination]
```
Each rsync command should begin with rsync. It is then followed by an option. Below is a list of basic rsync options to familiarize yourself with.
| Option | Destription |
| --- | --- |
| ````-a```` | copy files recursively and preserve ownership of files when files are copied |
| ````-v```` | runs the verbose version of the command; all of the processes that are run will be written out for the user to read |
| ````-z```` | compress the data synced |
| ````-h```` | initiate rsync help |
| ````-e```` | tell rsync what shell to use |
| ````-exclude="*.fileType"```` | exclude all of a specific filetype. Replace filetype with the actual file type that should be excluded |
| ````-delete```` | delete any extraneous files from the destination directory |

If more than one option with a dash is used, they may be consolidated into one bundle of options that has only one dash i.e ````-avzhe````
Here is the command you will use on the 192.168.1.9 server to synchronize all files that end with ````*.sql```` in the ````/var/www/```` directory to save them in the 192.168.1.5 backup server in the ````/var/www/server9/databases```` directory:
```
rsync -avzhe "ssh -p 10000" /var/www/*.sql <username>@192.168.1.5:/var/www/server9/databases/
```
