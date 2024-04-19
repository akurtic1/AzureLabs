---
lab:
    title: 'Lab 08: Manage Virtual Machines'
    module: 'Administer Virtual Machines'
---

# Lab 08 - Manage Virtual Machines

## Lab introduction

In this lab, I configured Virtual Machines and Virtual Machine scale sets.

## Lab scenario

Your organization wants to explore deploying and configuring Azure virtual machines. First, you implement an Azure virtual machine with manual scaling. Next, you implement a Virtual Machine Scale Set and explore autoscaling.

## Tasks:

+ Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal.
+ Task 2: Manage compute and storage scaling for virtual machines.
+ Task 3: Create and configure Azure Virtual Machine Scale Sets.
+ Task 4: Scale Azure Virtual Machine Scale Sets.
+ Task 5: Create a virtual machine using Azure PowerShell.

### Deploying a Virtual Machine using Azure Portal

![Diagram of the vm architecture tasks.](../media/az104-lab08-vm-architecture.png)

In this task, I deployed a Virtual Machine using Azure Portal. In the text below, you will see the properties that I configured for a VM:
    | Setting | Value |
    | --- | --- |
    | Subscription | subscription |
    | Resource group |  **az104-rg8**  |
    | Virtual machine names | `az104-vm1` and `az104-vm2` (because I selected 2 Availability Zones) |
    | Region | **West EU** |
    | Availability options | **Availability zone** |
    | Availability zone | **Zone 1, 2**  |
    | Security type | **Standard** |
    | Image | **Windows Server 2019 Datacenter - x64 Gen2** |
    | Azure Spot instance | **unchecked** |
    | Size | **Standard D2s v3** |
    | Username | `localadmin` |
    | Password | **strong password** |
    | Public inbound ports | **None** |
    | Would you like to use an existing Windows Server license? | **Unchecked** |

I also went trough and configured small details about monitoring, networking, management and disks.

### Manage compute and storage scaling for virtual machines.

After the resource is creating, I went to "Size" blade and went trough all properties that are available to edit.
I attached a simple disk:
    | Setting | Value |
    | --- | --- |
    | Disk name | `vm1-disk1` |
    | Storage type | **Standard HDD** |
    | Size (GiB) | `32` |

### Creating and configure Azure Virtual Machine Scale Sets

In this task, I deployed Virtual Machine Scale sets using Azure portal, across availability zones.
I configured the basic properties:
    | Setting | Value |
    | --- | --- |
    | Subscription | subscription  |
    | Resource group | **az104-rg8**  |
    | Virtual machine scale set name | `vmss1` |
    | Region | **West EU** |
    | Availability zone | **Zones 1, 2, 3** |
    | Orchestration mode | **Uniform** |
    | Security type | **Standard** |
    | Image | **Windows Server 2019 Datacenter - x64 Gen2** |
    | Run with Azure Spot discount | **Unchecked** |
    | Size | **Standard D2s_v3** |
    | Username | `localadmin` |
    | Password | **password**  |
    | Already have a Windows Server license? | **Unchecked** |

After configuring the basic properties, I also configured Networking. I created a Virtual Network, Load Balancer and added an inboud rule.

### Scale Azure Virtual Machine Scale Sets

In this task, I used the resource I created in previous task (Virtual Machine Scale Sets) and decided to go with scaling.
So, I added a custom scale and configured the following properties:
    | Setting | Value |
    | --- | --- |
    | Metric source | **Current resource (vmss1)** |
    | Metric namespace | **Virtual Machine Host** |
    | Metric name | **Percentage CPU** |
    | Operator | **Greater than** |
    | Metric threshold to trigger scale action | **70** |
    | Duration (minutes) | **10** |
    | Time grain statistic | **Average** |
    | Operation | **Increase percent by**  |
    | Cool down (minutes) | **5** |
    | Percentage | **20** |

This rule scales out when the average CPU load is greater than 70% over a 10-minute period. When the rule triggers, the number of VM instances is increased by 20%.

###  Creating a Virtual Machine using Powershell
In this task, I used Cloud Shell (Powershell) to deploy a new Virtual Machine
I used the following script:

    ```powershell
    New-AzVm `
    -ResourceGroupName 'az104-rg8' `
    -Name 'myPSVM' `
    -Location 'East US' `
    -Image 'Win2019Datacenter' `
    -Zone '1' `
    -Size 'Standard_D2s_v3' ` 
    -Credential (Get-Credential)
    ```
When a Virtual Machine was deployed, I used a few more scripts for checking if everything was deployed correctly.

    ```powershell
    Get-AzVM `
    -ResourceGroupName 'az104-rg8' `
    -Status
    ```

## Key takeaways

+ Azure virtual machines are on-demand, scalable computing resources.
+ Azure virtual machines provide both vertical and horizontal scaling options.
+ Configuring Azure virtual machines includes choosing an operating system, size, storage and networking settings.
+ Azure Virtual Machine Scale Sets let you create and manage a group of load balanced VMs.
+ The virtual machines in a Virtual Machine Scale Set are created from the same image and configuration.
+ In a Virtual Machine Scale Set the number of VM instances can automatically increase or decrease in response to demand or a defined schedule.