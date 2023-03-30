# Queue

A queue data structure is based on FIFO (First In First Out). That is the element that enters first in the queue, leaves first while popping the element from the queue. This data structure is mostly used for scheduling processes. The queue is also used to store data which will be transmitted while sending and receiving the data.

# Laravel Queue

A Laravel queue is an effective way of deferring the processing of a time-consuming task in your application. For example, we can use Laravel queue for sending out verification emails whenever a new user registers or shares a post. Let's say we want to send an E-mail notification called "Registration Successful" to the user. There are two ways to send the notification. First one is to send an email straight away when a user registers. And the second one is to put an E-mail notification in a queue called Notifications.

In the first case, a user has to wait 2-3 seconds because the email notification is being sent directly after clicking on register button. But in the second case, you can queue an email notification in few milliseconds and tell the user that an email has been sent.

Sending notifications via queue will also take 2-3 seconds to deliver based on the traffic on queues, but it feels faster because the processes are executed in the background. Also queues are not limited to any specific number of tasks you can perform. You can run multiple workers using supervisor to process tasks even faster.

Note: Workers are the processes that run continuously on the server, constantly waiting for a job to perform.

For example, if you want to run Notifications queue, you have to keep the following processes running on the server.

```
php artisan queue: work --queue=Notifications
```
## Configure Supervisor for Laravel Queues

### Install Supervisor

Supervisor is very easy to install, configure and manage processes. There are some good features that you can use to make queues even more efficient. Like auto restart. I will create a **Notifications** queue and run it using supervisor with multiple workers. Here, multiple workers basically means that we can execute same tasks in parallel. Which means, multiple workers will pick task from the queue at the same time and process all of them together.

```
sudo apt-get install supervisor -y
```
It will only take a few seconds to install and configure. The next thing we have to do is to create a configuration file for running **Notifications** queue workers.

Check server is running after installation:
```
service supervisor status
```
### Create Supervisor Configuration File for Queue

The configuration files for supervisor are typically stored in the ````/etc/supervisor/conf.d```` directory. In this directory, you can create any number of configuration files that instruct supervisor how your processes should be monitored. To create a configuration file, execute the following command.
```
sudo nano /etc/supervisor/conf.d/Notifications.conf
```
Edit this file and enter the configurations as shown below.
```
[program: Notifications]
process_name=%(program_name)s_&(process_num)02d
command=php /var/www/directoryname/artisan queue:work --queue=Notifications --tries=3 --sleep=3 --daemon
user=root
directory=/var/www/msaas
autostart=true
autorestart=true
numprocs=1
redirect_stderr=true
stdout_logfile=/var/www/directoryname/storage/logs/supervisor.log
```
**Note**:The numprocs line in the configuration. Here, you can decide how many workers you want to run for this queue. If you keep it to 3, it will send 3 email notifications concurrently.

### Start the Process in Supervisor

We have our configuration ready. But supervisor does not know about it yet. We have to reload the configuration and start the processes using Supervisor command line interface. To start supervisor command line interface, execute the following command.
```
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start Notifications:*
```
### Check the status of Supervisor
Now, execute the status command in supervisor command line interface and check if your processes are running or not. Run the following command
```
sudo supervisorctl status
```
