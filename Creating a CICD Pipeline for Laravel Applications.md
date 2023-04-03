# Continuous Integration and Continuous Delivery

CI/CD is a method to frequently deliver apps to customers by introducing automation into the stages of app development. The main concepts attributed to CI/CD are continuous integration, continuous delivery, and continuous deployment. CI/CD is a solution to the problems integrating new code can cause for development and operations teams.

Specifically, CI/CD introduces ongoing automation and continuous monitoring throughout the lifecycle of apps, from integration and testing phases to delivery and deployment. Taken together, these connected practices are often referred to as a "CI/CD pipeline" and are supported by development and operations teams working together in an agile way with either a DevOps or site reliability engineering (SRE) approach.

## What's the difference between CI and CD 

The acronym CI/CD has a few different meanings. The "CI" in CI/CD always refers to continuous integration, which is an automation process for developers. Successful CI means new code changes to an app are regularly built, tested, and merged to a shared repository. It’s a solution to the problem of having too many branches of an app in development at once that might conflict with each other.

The "CD" in CI/CD refers to continuous delivery and/or continuous deployment, which are related concepts that sometimes get used interchangeably. Both are about automating further stages of the pipeline, but they’re sometimes used separately to illustrate just how much automation is happening.

Continuous delivery usually means a developer’s changes to an application are automatically bug tested and uploaded to a repository (like GitHub or a container registry), where they can then be deployed to a live production environment by the operations team. It’s an answer to the problem of poor visibility and communication between dev and business teams. To that end, the purpose of continuous delivery is to ensure that it takes minimal effort to deploy new code.

Continuous deployment (the other possible "CD") can refer to automatically releasing a developer’s changes from the repository to production, where it is usable by customers. It addresses the problem of overloading operations teams with manual processes that slow down app delivery. It builds on the benefits of continuous delivery by automating the next stage in the pipeline.

## How to go about it

In my case, I used buddyworks software to create a CI/CD pipeline for the Laravel application. There are other tools that you can use for this purpose, including Travis CI and SonarCloud, just to mention a few. Whichever one you choose is okay as I've found that most entail the same steps to creating a CI/CD pipeline.

## Using buddyworks to create a CI/CD pipeline

The first thing you'll have to do is create an account and log in. For this project, I opted for a free trial because it didn't have a long lifecycle, but you can pick any other plan depending on your situation. Once logged in, the buddyworks UI should look like this:

![Buddy1](https://user-images.githubusercontent.com/122386130/229411249-b7d2f8af-ab77-48d4-8a93-e804f44a2801.PNG)

Click on ````New project````. This will take you to a page that allows you to choose whichever integration you want, be it with a repository in Github, Gitlab, Bitbucket,...etc:

![Buddy2](https://user-images.githubusercontent.com/122386130/229411647-d3598fa1-ac9e-46db-9354-29b40088135b.PNG)

In my case, I chose Gitlab because that's where my project repository, ims-a, was.

Once this is done, click on the project's repository to get to a page that prompts you to create a ````New pipeline````:

![Buddy3](https://user-images.githubusercontent.com/122386130/229412198-a6330134-8232-401a-8326-d5636c5d5d90.PNG)

Click on ````New pipeline````, enter your choice name for the pipeline and a trigger event for the pipeline. For this example, I named my pipeline Laravel Applications and my trigger event was a git push to the main branch of the project's repository:

![Buddy4](https://user-images.githubusercontent.com/122386130/229412736-9394228f-1dd4-4e2d-b0aa-cb60ecdc04ba.PNG)

Once this is done, click on ````Add pipeline````. You'll be redirected to a page that prompts you to add a new action to your pipeline. Choose SSH since we want to be using SSH to access the server where the deployment is taking place:

![Buddy5](https://user-images.githubusercontent.com/122386130/229413247-5b3be4dd-c75e-4c4e-a3b6-2a5fe5027762.PNG)

This will take you to a page that prompts you to add the SSH commands you want run on your server once buddyworks connects to it. These will be
```
git checkout .
git pull
composer install
php artisan migrate
```
![Buddy6](https://user-images.githubusercontent.com/122386130/229413991-f124df6e-788c-4985-8697-afa048d3d7cf.PNG)

When you click ````Add Action```` Buddy will require you to enter the IP address of your server, the authentication mode to your server, whether a password, key, or both, and your user name. You'll also have to enter your project's directory path under the ````Working Directory```` prompt.

At the bottom, you'll also find a prompt that requires you to test the connection to your server. Do try it out to find out if your login details are correct and that buddy is able to establish and maintain a connection with your target server. Once this is done click ````Add this action````:

![Buddy7](https://user-images.githubusercontent.com/122386130/229414870-c32f24cf-466f-4ffc-b09b-2605d773635c.PNG)

This will take you to a page where you can run the pipeline to make sure its working. To do this, simply click on ````RUN````. If the process was successful, the result should look like this:

![Buddy8](https://user-images.githubusercontent.com/122386130/229415519-3312ab0d-6652-4bce-856f-60e002691c3c.PNG)

If not, you'll see error logs on the logs screen. Buddy will also send you an email to confirm the same, so no matter where you are, you'll always be able to keep track of your pipeline.

## Reasons Buddyworks pipeline might fail:

Firstly, I had trouble creating this pipeline because of the file permissions in the folder I had pulled from Gitlab to my server. To fix this, I went one folder behind the one with my laravel files and entered this command to grant me full read/write/execute permissions:
```
chmod -R 777/
```
If this still doesn't work, use this command to give write permissions to the folder:
```
sudo chmod -R ug+w .;
```

Also, you have to ensure that you have complete ownership to your folder, which you can do using this command (In my case, the owner was www-data, so you'll need to change this part to suit your specific situation):

```
chown -R www-data:www-data ims-A
```
Once all this is done, your pipeline should be working just fine:)
