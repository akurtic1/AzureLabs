---
lab:
    title: 'Lab 03: Manage Azure resources by using Azure Resource Manager Templates'
    module: 'Administer Azure Resources'
---

# Lab 03 - Manage Azure resources by using Azure Resource Manager Templates

## Lab introduction

In this lab, I learned how to automate resource deployments. I used Azure Resource Manager templates and Bicep templates.
Steps provided by Microsoft are written using **East-US** but I used **West-EU**. 
This Lab helped me understand other deployments using Cloud Shell (PowerShell), CLI and Bicep.

## Lab scenario

Your team wants to look at ways to automate and simplify resource deployments. Your organization is looking for ways to reduce administrative overhead, 
reduce human error and increase consistency. 

## Architecture diagram
![Diagram of the lab 03 architecture.](../AdminLabs/Media/az104-lab03-architecture.png)

## Tasks:

+ Task 1: Create an Azure Resource Manager template.
+ Task 2: Edit an Azure Resource Manager template and redeploy the template.
+ Task 3: Configure the Cloud Shell and deploy a template with Azure PowerShell.
+ Task 4: Deploy a template with the CLI.
+ Task 5: Deploy a resource by using Azure Bicep.
   
### Creating an ARM (Azure Resource Manager) template

In this task, I created a manged Disk in the Azure Portal. After disk is deployed I exported a template that helped me in other deployments.

**Properties for creating a Managed Disk:** 

    | Setting | Value |
    | --- | --- |
    | Subscription | *your subscription* | 
    | Resource Group | `az104-rg3` 
    | Disk name | `az104-disk1` | 
    | Region | **East US** |
    | Availability zone | **No infrastructure redundancy required** | 
    | Source type | **None** |
    | Performance | **Standard HDD** (change size) |
    | Size | **32 Gib** | 

After deploying a Managed disk using Portal, I went to Automation blade and exported template.

### Edit an Azure Resource Manager template and redeploy the template

In this task, I searched in Azure portal "deploy a custom template". After checking the properties, I edited
the template and changed disk names using JSON file that was exported in previous task.
I used two files, "template.json" and "parameter.json".

After deploying the exported template, I checked if the disk was created successfully.

### Configure the Cloud Shell and deploy a template with Azure PowerShell

In this task, I configured the Cloud Shell and deployed a template using Azure PowerShell.
I used following script to deploy a resource grup:

**New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json**
After that, I checked if the disk was created by typing **Get-AzDisk**.

    
### Deploy a template with the CLI

In this task, I continiue to use Cloud Shell but this time I used "Bash".
I verified if the files are available in Cloud Shell storage by typing "ls".

Deploying a Resource Group using Bash.
**az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json**.
Checking if the disk was created: **az disk list --output table**.

### Deploy a resource by using Azure Bicep

In this task, I'm still using **Bash** session.
**I used the official Bicep file provided by Microsoft.**

Source: https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/558c74a029de10fd8b15c9a218b71dfb7d3e737b/Allfiles/Labs/03

I used Bicep file and made the changes below:

+ Changed the managedDiskName value to Disk4.
+ Changed the sku name value to StandardSSD_LRS.
+ Changed the diskSizeinGiB value to 32.

Deploying a template with: **az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep**

Confirming if the disk was created: az disk list **--output table**

## Key takeaways

+ Azure Resource Manager templates let you deploy, manage, and monitor all the resources for your solution as a group, rather than handling these resources individually.
+ An Azure Resource Manager template is a JavaScript Object Notation (JSON) file that lets you manage your infrastructure declaratively rather than with scripts.
+ Rather than passing parameters as inline values in your template, you can use a separate JSON file that contains the parameter values.
+ Azure Resource Manager templates can be deployed in a variety of ways including the Azure portal, Azure PowerShell, and CLI.
+ Bicep is an alternative to Azure Resource Manager templates. Bicep uses a declarative syntax to deploy Azure resources.
+ Bicep provides concise syntax, reliable type safety, and support for code reuse. Bicep offers a first-class authoring experience 
