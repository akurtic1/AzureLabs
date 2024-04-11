---
lab:
    title: 'Lab 04: Implement Virtual Networking'
    module: 'Implement Virtual Networking'
---

# Lab 04 - Implement Virtual Networking

## Lab introduction

In this lab, i learned the basics of virtual networking and subnetting. I also used protection resources such as Network Security Groups and Application Security Groups.

## Lab scenario

Your global organization plans to implement virtual networks. The immediate goal is to accommodate all the existing resources. However, the organization is in a growth phase and wants to ensure there is additional capacity for the growth.

The CoreServicesVnet virtual networkhas the largest number of resources. A large amount of growth is anticipated, so a large address space is necessary for this virtual network.

The ManufacturingVnet virtual network contains systems for the operations of the manufacturing facilities. The organization is anticipating a large number of internal connected devices for their systems to retrieve data from.. 

## Architecture diagram
![Diagram of the lab 04 architecture.](../AdminLabs/Media/az104-lab04-architecture.png)

## Tasks:

+ Task 1: Create a virtual network with subnets using the portal.
+ Task 2: Create a virtual network and subnets using a template.
+ Task 3: Create and configure communication between an Application Security Group and a Network Security Group.
+ Task 4: Configure public and private Azure DNS zones.
   
### Create a virtual network with subnets using the portal.

In this task, I created a VNet and associated subnets to the existing resources. 

I used Azure Portal to do this task.

**Basic tab** for the CoreServiceVnet 

|  **Option**        | **Value**            |
| ------------------ | -------------------- |
	| Resource Group     | `az104-rg4`          |
	| Name               | `CoreServicesVnet`   |
	| Region             | (US) **East US**     | 

**IP Adresses** tab

        |  **Option**         | **Value**           |
	| ------------------ | -------------------- |

	| IPv4 address space | `10.20.0.0/16`       |

**Adding a subnet**

        | **Subnet**             | **Option**           | **Value**              |
	| ---------------------- | -------------------- | ---------------------- |
	| SharedServicesSubnet   | Subnet name          | `SharedServicesSubnet` |
	|                        | Starting address		| `10.20.10.0`   |
	|						| Size			| `/24`	|
	| DatabaseSubnet         | Subnet name          | `DatabaseSubnet`       |
	|                        | Starting address		| `10.20.20.0`   |
	|						| Size			| `/24`	|

After creating a VNet and adding a subnet, I went to the resource and checked if everything is deployed successfully.

### Create a virtual network and subnets using a template

In this task, I used a template to create the resources.
In the JSON template, I did the following changes:

**ManufacturingVnet virtual network**
Replaced all occurrences of CoreServicesVnet with ManufacturingVnet.

Replaced all occurrences of 10.20.0.0 with 10.30.0.0.

**changes for the ManufacturingVnet subnets**

Changed all occurrences of SharedServicesSubnet to SensorSubnet1.

Changed all occurrences of 10.20.10.0/24 to 10.30.20.0/24.

Changed all occurrences of DatabaseSubnet to SensorSubnet2.

Changed all occurrences of 10.20.20.0/24 to 10.30.21.0/24.

I also did the changes in the "perimeter.json" file and I checked if everything was ready for the deployment.

### Create and configure communication between an Application Security Group and a Network Security Group

In this task, I created Application Security Group and a Network Security GRoup. NSG will have an inbound security rule
that allows traffic from ASG. NSG also have an outbound rule that denis access to the Internet.

Basic Information:

    | Setting | Value |
    | -- | -- |
    | Subscription | *your subscription* |
    | Resource group | **az104-rg4** |
    | Name | `asg-web` |
    | Region | **East US**  |

Settings after deployment NSG

    | Setting | Value |
    | -- | -- |
    | Virtual network | **CoreServicesVnet (az104-rg4)** |
    | Subnet | **SharedServicesSubnet** 

**Configuring an inbound security rule to allow ASG traffic**

    | Setting | Value |
    | -- | -- |
    | Source | **Application security group** |
    | Source application security groups | **asg-web** |
    | Source port ranges |  * |
    | Destination | **Any** |
    | Service | **Custom** (notice your other choices) |
    | Destination port ranges | **80,443** |
    | Protocol | **TCP** |
    | Action | **Allow** |
    | Priority | **100** |
    | Name | `AllowASG` |

**Configuring an outbound NSG rule that denies Internet access**

    | Setting | Value |
    | -- | -- |
    | Source | **Any** |
    | Source port ranges |  * |
    | Destination | **Service tag** |
    | Destination service tag | **Internet** |
    | Service | **Custom** |
    | Destination port ranges | **8080** |
    | Protocol | **Any** |
    | Action | **Deny** |
    | Priority | **4096** |
    | Name | **DenyAnyCustom8080Outbound** |

### Configure a private DNS zone

**Creating a Private DNS Basics**

    | Property | Value    |
    |:---------|:---------|
    | Subscription | **Select your subscription** |
    | Resource group | **az04-rg4** |
    | Name | `private.contoso.com`|
    | Region |**East US** |

**Linking Private Network**

    | Property | Value    |
    |:---------|:---------|
    | Link name | `manufacturing-link` |
    | Virtual network | `ManufacturingVnet` |

**Adding record set**

    | Property | Value    |
    |:---------|:---------|
    | Name | **sensorvm** |
    | Type | **A** |
    | TTL | **1** |
    | IP address | **10.1.1.4** |


## Key takeaways

+ A virtual network is a representation of your own network in the cloud. 
+ When designing virtual networks it is a good practice to avoid overlapping IP address ranges. This will reduce issues and simplify troubleshooting.
+ A subnet is a range of IP addresses in the virtual network. You can divide a virtual network into multiple subnets for organization and security.
+ A network security group contains security rules that allow or deny network traffic. There are default incoming and outgoing rules which you can customize to your needs.
+ Application security groups are used to protect groups of servers with a common function, such as web servers or database servers.
+ Azure DNS is a hosting service for DNS domains that provides name resolution. You can configure Azure DNS to resolve host names in your public domain.  You can also use private DNS zones to assign DNS names to virtual machines (VMs) in your Azure virtual networks.
