---
lab:
    title: 'Lab 10: Implement Data Protection'
    module: 'Administer Data Protection'
---

# Lab 10 - Implement Data Protection

## Lab introduction

In this task, I moved applications to the Azure Cloud. I used Azure Container Apps
to finish this simple and short task.

## Lab scenario

Your organization is evaluating how to backup and restore Azure virtual machines from accidental or malicious data loss. 
Additionally, the organization wants to explore using Azure Site Recovery for disaster recovery scenarios. 

## Tasks:

+ Task 1: Use a template to provision an infrastructure.
+ Task 2: Create and configure a Recovery Services vault.
+ Task 3: Configure Azure virtual machine-level backup.
+ Task 4: Monitor Azure Backup.
+ Task 5: Enable virtual machine replication. 

### Diagram of the Lab 10
![Diagram of the lab.](../media/az104-lab09a-architecture.png)

### Using a template to provision an infrastructure.
In this task, I deployed a template provided by Microsoft using "Deploy a custom template" resource
via Azure Portal.
![Screenshot of this task](../media/az104-lab09a-architecture.png)

### Creating and configuring a Recovery Services vault.
In this task, I created a Recovery Services vault. I also configured blades such as Security Settings and Backup Configuration.
![Screenshot of this task](../media/az104-lab09a-architecture.png)

### Configuring Azure Virtual machine-level backup.
In this task, I configured a backup for Azure Virtual Machine.
I also created a new policy, where I configured backup schedule that was more fit for my timezone.
![Screenshot of this task](../media/az104-lab09a-architecture.png)

### Monitoring Azure Backup.
In this task, I deployed an Azure Storage Account. After creating a Storage Account, I configured the vault that sends logs and metrics to
azure storage account.
![Screenshot of this task](../media/az104-lab09a-architecture.png)

When I wanted to save configuration, I received an error that you will see on the screenshot.
![Screenshot of the error](../media/az104-lab09a-architecture.png)
After seeing this error, I had to research to see what can fix this. I used Azure Portal
to create, find resource with insights. I created Application Insights and that fixed the problem that I received
in this task.

### Enabling virtual machiune replication.
In this task, I created again the Recovery Services vault, but this time I configured
it with the different region from my first Virtual Machine. I decided to go with West US.

I used the VM that was created with the template and configured Backup + Disaster Recovery.
![Screenshot of the task](../media/az104-lab09a-architecture.png)
Deployment and recovery took time but there is also a screenshot of the storage account.

## Key takeaways

+ Azure Container Apps (ACA) is a serverless platform that allows you to maintain less infrastructure and save costs while running containerized applications.
+ Container Apps provides server configuration, container orchestration, and deployment details. 
+ Workloads on ACA are usually long-running processes like a Web App.
