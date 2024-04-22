---
lab:
    title: 'Lab 09a: Implement Web Apps'
    module: 'Administer PaaS Compute Options'
---

# Lab 09a - Implement Web Apps

## Lab introduction

In this task, I used Azure Web apps to host my companys a website.

## Lab scenario

Your organization is interested in Azure Web apps for hosting your company websites. The websites are currently hosted in an on-premises data center. The websites are running on Windows servers using the PHP runtime stack. The hardware is nearing end-of-life and will soon need to be replaced. Your organization wants to avoid new hardware costs by using Azure to host the websites. 

## Tasks:

+ Task 1: Create and configure an Azure web app.
+ Task 2: Create and configure a deployment slot.
+ Task 3: Configure web app deployment settings.
+ Task 4: Swap deployment slots.
+ Task 5: Configure and test autoscaling of the Azure web app.

### Diagram of the Task
![Diagram of the tasks.](../media/az104-lab09a-architecture.png)

### Creating and configuring an Azure web app.

In this task, I used Azure Web apps to configure and create an Azure App.
![Screenshot of this task](../media/az104-lab09a-architecture.png)


### Creating and configuring a deployment slot.

After the resource is created, I added a new slot called "staging". In this task, we need to make sure to 
test our app first then we can switch to production which means that our app is done.
    | Setting | Value |
    | --- | ---|
    | Name | `staging` |
    | Clone settings from | **Do not clone settings**|


### Configuring web app deployment settings

In this task, I configured Web App deployment settings. I used the source from External Git and 
in repository field, I used the source provided by Microsoft: https://github.com/Azure-Samples/php-docs-hello-world

### Swap deployment slots

In this task, I swaped the Production slot with staging slot that I configured in the previous tasks.
![Screenshot of this task](../media/az104-lab09a-architecture.png)

###  Configuring and testing autoscaling of the Azure web app
In this task, I configured autoscaling of Azure App, which enables us to mantain optimal performance when web app traffic increases.
On the settings tab, I used scale out option and configured with Automatic scaling with maximum burst of 2.

After that, I created a new load test to test the URL from our Default Domain.
![Screenshot of this task](../media/az104-lab09a-architecture.png)

Engine Health test:
![Screenshot of this task](../media/az104-lab09a-architecture.png)

## Key takeaways

+ Azure App Services lets you quickly build, deploy, and scale web apps.
+ App Service includes support for many developer environments including ASP.NET, Java, PHP, and Python.
+ Deployment slots allow you to create separate environments for deploying and testing your web app.
+ You can manually or automatically scale a web app to handle additional demand.
+ A wide variety of diagnostics and testing tools are available. 