---
lab:
    title: 'Lab 05: Implement Intersite Connectivity'
    module: 'Administer Intersite Connectivity'
---


# Lab 05 - Implement Intersite Connectivity

## Lab introduction

In this lab, I learned how to segment areas and learned how to make communication with apps and services.

## Lab scenario

Your organization segments core IT apps and services (such as DNS and security services) from other parts of the business, including your manufacturing department. However, in some scenarios, apps and services in the core area need to communicate with apps and services in the manufacturing area. In this lab, you configure connectivity between the segmented areas. This is a common scenario for separating production from development or separating one subsidiary from another.

## Architecture diagram
![Diagram of the lab 02 architecture.](../AdminLabs/Media/az104-lab05-architecture.png)

## Tasks:

+ Task 1: Create a virtual machine in a virtual network.
+ Task 2: Create a virtual machine in a different virtual network.
+ Task 3: Use Network Watcher to test the connection between virtual machines. 
+ Task 4: Configure virtual network peerings between different virtual networks.
+ Task 5: Use Azure PowerShell to test the connection between virtual machines.
+ Task 6: Create a custom route. 
   
### Creating a core services virtual machine and virtual network

In this task, I created a VM and setup the basic information below:
    | Setting | Value | 
    | --- | --- |
    | Subscription |  *my subscription* |
    | Resource group |  `az104-rg5` 
    | Virtual machine name |   `CoreServicesVM` |
    | Region | **(EU) West EU** |
    | Availability options | No infrastructure redundancy required |
    | Security type | **Standard** |
    | Image | **Windows Server 2019 Datacenter: x64 Gen2** |
    | Size | **Standard_DS2_v3** |
    | Username | `localadmin` | 
    | Password | **password** |
    | Public inbound ports | **None** |

On the **Networking** tab, for Virtual network, I used following properties.
    | Setting | Value | 
    | --- | --- |
    | Name | `CoreServicesVNet` (Create new) |
    | Address range | `10.0.0.0/16`  |
    | Subnet Name | `Core` | 
    | Subnet address range | `10.0.0.0/24` |

### Creating a virtual machine in a different virtual network

In this task, I created a different Virtual Machine with a different Networking properties.
Basic properties are the same but the Networking ones are different:
    | Setting | Value | 
    | --- | --- |
    | Name | `ManufacturingVNet` |
    | Address range | `172.16.0.0/16`  |
    | Subnet Name | `Manufacturing` |
    | Subnet address range | `172.16.0.0/24` |

### Using Network Watcher to test the connection between virtual machines 

In this task, I used the Network Watcher Resource to check if the created Virtual Machines can communicate with each other.
In Network Watched I used diagnostic tool, setup the troubleshoot connection and used the following properties:
    | Field | Value | 
    | --- | --- |
    | Source type           | **Virtual machine**   |
    | Virtual machine       | **CoreServicesVM**    | 
    | Destination type      | **Virtual machine**   |
    | Virtual machine       | **ManufacturingVM**   | 
    | Preferred IP Version  | **Both**              | 
    | Protocol              | **TCP**               |
    | Destination port      | `3389`                |  
    | Source port           | *Blank*         |
    | Diagnostic tests      | *Defaults*      |

## Configuring virtual network peerings between virtual networks
In this task, I used Virtual Network Peering to setup a communication between vritual networks.
Properties below:
| **Parameter**                                    | **Value**                             |
| --------------------------------------------- | ------------------------------------- |
| **This virtual network**                                       |                                       |
| Peering link name                             | `CoreServicesVnet-to-ManufacturingVnet` |
| Allow CoreServicesVNet to access the peered virtual network            | selected (default)                       |
| Allow CoreServicesVNet to receive forwarded traffic from the peered virtual network | selected                       |
| Allow gateway in CoreServicesVNet to forward traffic to the peered virtual network | Not selected (default) |
| Enable CoreServicesVNet to use the peered virtual networks' remote gateway       | Not selected (default)                        |
| **Remote virtual network**                                   |                                       |
| Peering link name                             | `ManufacturingVnet-to-CoreServicesVnet` |
| Virtual network deployment model              | **Resource manager**                      |
| I know my resource ID                         | Not selected                          |
| Subscription                                  | *my subscription*    |
| Virtual network                               | **ManufacturingVnet**                     |
| Allow ManufacturingVNet to access CoreServicesVNet  | selected (default)                       |
| Allow ManufacturingVNet to receive forwarded traffic from CoreServicesVNet | selected                        |
| Allow gateway in CoreServicesVNet to forward traffic to the peered virtual network | Not selected (default) |
| Enable ManufacturingVNet to use CoreServicesVNet's remote gateway       | Not selected (default)          

### Using Azure PowerShell to test the connection between virtual machines

In this task, I used Azure PowerShell to test the connection.
I used the following script: Test-NetConnection <CoreServicesVM private IP address> -port 3389


1. **Assign policy** and specify the **Scope**:

    | Setting | Value |
    | --- | --- |
    | Subscription | your Azure subscription |
    | Resource Group | `az104-rg2` |

1. To specify the **Policy definition**,I selected `Inherit a tag from the resource group if missing`.

1. Configuring the **Basics** properties of the assignment.

    | Setting | Value |
    | --- | --- |
    | Assignment name | `Inherit the Cost Center tag and its value 000 from the resource group if missing` |
    | Description | `Inherit the Cost Center tag and its value 000 from the resource group if missing` |
    | Policy enforcement | Enabled |

1. Configuring the **Parameters** to the following values:

    | Setting | Value |
    | --- | --- |
    | Tag Name | `Cost Center` |

1. **Remediation** tab, configuring the following settings:

    | Setting | Value |
    | --- | --- |
    | Create a remediation task | enabled |
    | Policy to remediate | **Inherit a tag from the resource group if missing** |

### Creating a custom route 

In this task, I used the new resource "Custom Route", where I wanted to control the network traffic between the perimeter subnet and the internal core services subnet.
In the text below, you will see the properties that I used for this task.
    | Setting | Value | 
    | --- | --- |
    | Subscription | your subscription |
    | Resource group | `az104-rg5`  |
    | Region | **East US** |
    | Name | `rt-CoreServices` |
    | Propagate gateway routes | **No** |

Creating a route to the CoreServices Virtual Network.
    | Setting | Value | 
    | --- | --- |
    | Route name | `PerimetertoCore` |
    | Destination type | **IP Addresses** |
    | Destination IP addresses | `10.0.0.0/16` (core services virtual network) |
    | Next hop type | **Virtual appliance** (notice your other choices) |
    | Next hop address | `10.0.1.7` (future NVA) |

## Key takeaways

+ By default, resources in different virtual networks cannot communicate.
+ Virtual network peering enables you to seamlessly connect two or more virtual networks in Azure.
+ Peered virtual networks appear as one for connectivity purposes.
+ The traffic between virtual machines in peered virtual networks uses the Microsoft backbone infrastructure.
+ System defined routes are automatically created for each subnet in a virtual network. User-defined routes override or add to the default system routes.
+ Azure Network Watcher provides a suite of tools to monitor, diagnose, and view metrics and logs for Azure IaaS resources. 
