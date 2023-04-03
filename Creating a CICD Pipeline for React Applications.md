# Continuous Integration and Continuous Delivery
CI/CD is a method to frequently deliver apps to customers by introducing automation into the stages of app development. The main concepts attributed to CI/CD are continuous integration, continuous delivery, and continuous deployment. CI/CD is a solution to the problems integrating new code can cause for development and operations teams.

Specifically, CI/CD introduces ongoing automation and continuous monitoring throughout the lifecycle of apps, from integration and testing phases to delivery and deployment. Taken together, these connected practices are often referred to as a "CI/CD pipeline" and are supported by development and operations teams working together in an agile way with either a DevOps or site reliability engineering (SRE) approach.

## What's the difference between CI and CD
The acronym CI/CD has a few different meanings. The "CI" in CI/CD always refers to continuous integration, which is an automation process for developers. Successful CI means new code changes to an app are regularly built, tested, and merged to a shared repository. It’s a solution to the problem of having too many branches of an app in development at once that might conflict with each other.

The "CD" in CI/CD refers to continuous delivery and/or continuous deployment, which are related concepts that sometimes get used interchangeably. Both are about automating further stages of the pipeline, but they’re sometimes used separately to illustrate just how much automation is happening.

Continuous delivery usually means a developer’s changes to an application are automatically bug tested and uploaded to a repository (like GitHub or a container registry), where they can then be deployed to a live production environment by the operations team. It’s an answer to the problem of poor visibility and communication between dev and business teams. To that end, the purpose of continuous delivery is to ensure that it takes minimal effort to deploy new code.

Continuous deployment (the other possible "CD") can refer to automatically releasing a developer’s changes from the repository to production, where it is usable by customers. It addresses the problem of overloading operations teams with manual processes that slow down app delivery. It builds on the benefits of continuous delivery by automating the next stage in the pipeline.

## How to go about it
In my case, I used buddyworks software to create a CI/CD pipeline for the React application. There are other tools that you can use for this purpose, including Travis CI and SonarCloud, just to mention a few. Whichever one you choose is okay as I've found that most entail the same steps to creating a CI/CD pipeline.

## Using buddyworks to create a CI/CD pipeline
The first thing you'll have to do is create an account and log in. For this project, I opted for a free trial because it didn't have a long lifecycle, but you can pick any other plan depending on your situation. Once logged in, the buddyworks UI should look like this:

![Buddy1](https://user-images.githubusercontent.com/122386130/229431623-6c0e42f8-cfaf-40d1-b94e-d5e89350a0f4.PNG)

Click on ````New project````. This will take you to a page that allows you to choose whichever integration you want, be it with a repository in Github, Gitlab, Bitbucket,...etc:

![Buddy2](https://user-images.githubusercontent.com/122386130/229431813-1aeae401-4c25-487d-9a1d-c67a5eea9eb4.PNG)

In my case, I chose Gitlab because that's where my project repository, ims-a, was.

Once this is done, click on the project's repository to get to a page that prompts you to create a ````New pipeline````:

![Buddy3](https://user-images.githubusercontent.com/122386130/229431970-d14a31df-8290-4a00-8002-72ed803af57b.PNG)

Click on ````New pipeline````, enter your choice name for the pipeline and a trigger event for the pipeline. For this example, I named my pipeline React Application and my trigger event was a git push to the main branch of the project's repository:

![Buddy9](https://user-images.githubusercontent.com/122386130/229432222-e98bc8d0-86c1-45e9-a70c-88a5259dabf7.PNG)

Once this is done, click on ````Add pipeline````. You'll be redirected to a page that prompts you to add a new action to your pipeline. Choose SSH since we want to be using SSH to access the server where the deployment is taking place:

![Buddy10](https://user-images.githubusercontent.com/122386130/229432561-4ce62e79-1dfe-46ea-934d-1edd70cae4ba.PNG)

This will take you to a page that prompts you to add the SSH commands you want run on your server once buddyworks connects to it. These will be:
```
git checkout .
git pull
npm install
npm run build
```

![Buddy11](https://user-images.githubusercontent.com/122386130/229433434-096939fe-f162-4bed-84b9-0a87a8a4a170.PNG)

When you click ````Add Action```` Buddy will require you to enter the IP address of your server, the authentication mode to your server, whether a password, key, or both, and your username. You'll also have to enter your project's directory path under the ````Working Directory```` prompt.

At the bottom, you'll also find a prompt that requires you to test the connection to your server. Do try it out to find out if your login details are correct and that buddy is able to establish and maintain a connection with your target server. Once this is done click ````Add this action````:

![Buddy7](https://user-images.githubusercontent.com/122386130/229433906-5138274a-68a0-474a-8fcf-bd3b07ff9e54.PNG)

This will take you to a page where you can run the pipeline to make sure its working. To do this, simply click on ````RUN````. If the process was successful, the result should look like this:

![Buddy12](https://user-images.githubusercontent.com/122386130/229434506-34160940-c556-45c6-a4fb-580a5d854d23.PNG)

If not, you'll see error logs on the logs screen. Buddy will also send you an email to confirm the same, so no matter where you are, you'll always be able to keep track of your pipeline.

## Reasons Buddyworks pipeline might fail:
### 1. Using Root Login
One of the reasons your pipeline might fail is because you are not logged in as the root user which is necessary when running npm install. Check if your server allows root login, and if not, make the necessary changes to ensure it does. Then, when Buddy asks for authentication details, enter those of the root user and set the username to root.
To make a server accessible using root login, open the ````/etc/ssh/sshd_config```` file with administrative privileges and change the following line:
```
From:
#PermitRootLogin prohibit-password
TO:
PermitRootLogin yes
```
Under users, also add ````root````. Then save the file and restart the SSH service:
```
sudo systemctl restart ssh
```

If you already don't have a root password, you can create one by following this process:
```
$ sudo passwd
[sudo] password for linuxconfig: 
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
```
From here, buddy will be able to connect to your server using root and execute npm install.

### 2. Buddy Server

Another problem that you could face with the CICD pipeline is when the Buddy server treats warning messages as errors. Warning messages always come up when running npm run build and when treated as errors, they'll end up interrupting your CI/CD execution process. If have this trouble with your pipeline, add a ````export CI=false```` command at the top of your SSH actions. The actions will then look like this:
```
export CI=false
git checkout .
git pull
npm install
npm run build
```
Once this is done, your pipeline should work successfully:)
