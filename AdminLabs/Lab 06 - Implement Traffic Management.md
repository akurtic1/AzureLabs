---
lab:
    title: 'Lab 06: Implement Traffic Management'
    module: 'Administer Network Traffic Management'
---


# Lab 06 - Implement Traffic Management

## Lab introduction

In this lab, I used and configured Load Balancer and Application Gateway.

## Lab scenario

Your organization has a public website. You need to load balance incoming public requests across different virtual machines. You also need to provide images and videos from different virtual machines. You plan on implementing an Azure Load Balancer and an Azure Application Gateway. All resources are in the same region.

## Tasks:

+ Task 1: Use a template to provision an infrastructure.
+ Task 2: Configure an Azure Load Balancer.
+ Task 3: Configure an Azure Application Gateway.

   
### Use a template to provision an infrastructure.

In this task, I used template to deploy it via Azure Portal. Template was provided by Microsoft.
Source: https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/558c74a029de10fd8b15c9a218b71dfb7d3e737b/Allfiles/Labs/06

### Configure an Azure Load Balancer.

In this task, I used Load Balancer in front of the two Azure VM's in the Virtual Network. I setup the front-end IP Adress, backend pool and rules
that define how connections should traverse the load balancer.
![Load Balancer Architecture.](D:/AzureProjects/AdmirLabs/az104-lab01-architecture.png)


**Frontend IP configuration:**

    | Setting | Value |
    | --- | --- |
    | Name | `az104-lbpip` |
    | SKU | Standard |
    | Tier | Regional |
    | Assignment | Static |
    | Routing Preference | **Microsoft network** |

**Adding backend pool:**
    | Setting | Value |
    | --- | --- |
    | Name | `az104-be` |
    | Virtual network | **az104-06-vnet1** |
    | Backend Pool Configuration | **NIC** |

**Adding Load balancing rule:**
    | Setting | Value |
    | --- | --- |
    | Name | `az104-lbrule` |
    | IP Version | **IPv4** |
    | Frontend IP Address | **az104-fe** |
    | Backend pool | **az104-be** |
    | Protocol | **TCP** |
    | Port | `80` |
    | Backend port | `80` |
    | Health probe | **new** |
    | Name | `az104-hp` |
    | Protocol | **TCP** |
    | Port | `80` |
    | Interval | `5` |
    | Session persistence | **None** |
    | Idle timeout (minutes) | `4` |
    | TCP reset | **Disabled** |
    | Floating IP | **Disabled** |
    | Outbound source network address translation (SNAT) | **Recommended** |

### Configuring an Azure Application Gateway

In this task, I implemented an Azure Application Gateaway in front of two VM's. The Application Gateway routes images to one virtual machine and videos to the other virtual machine.

**Configuring the Basic Tab**
    | Setting | Value |
    | --- | --- |
    | Subscription | my subscription |
    | Resource group | `az104-rg6` |
    | Application gateway name | `az104-appgw` |
    | Region | The **west europe**|
    | Tier | **Standard V2** |
    | Enable autoscaling | **No** |
    | Minimum instance count | `2` |
    | Availability zone | **Zone 1** |
    | HTTP2 | **Disabled** |
    | Virtual network | **az104-06-vnet1** |
    | Subnet | **subnet-appgw (10.60.3.224/27)** |

**Adding a backend pool - Images:**
    | Setting | Value |
    | --- | --- |
    | Name | `az104-imagebe` |
    | Add backend pool without targets | **No** |
    | Virtual machine | **az104-rg6-nic1 (10.60.1.4)** |

**Adding a backend pool - Video:**
    | Setting | Value |
    | --- | --- |
    | Name | `az104-videobe` |
    | Add backend pool without targets | **No** |
    | Virtual machine | **az104-rg6-nic2 (10.60.2.4)** |

After configuring the Application Gateaway, I used browser to test if the Application Gateway is working.
URL that I tested: `http://<frontend ip address>/images/
URL that I tested: `http://<frontend ip address>/video/

With the first URL, I was redirected to image server VM1 and with the another one, I was redirected to video server VM2.

## Key takeaways

+ Azure Load Balancer is an excellent choice for distributing network traffic across multiple virtual machines at the transport layer (OSI layer 4 - TCP and UDP).
+ Public Load Balancers are used to load balance internet traffic to your VMs. An internal (or private) load balancer is used where private IPs are needed at the frontend only.
+ The Basic load balancer is for small-scale applications that don't need high availability or redundancy. The Standard load balancer is for high performance and ultra-low latency.
+ Azure Application Gateway is a web traffic (OSI layer 7) load balancer that enables you to manage traffic to your web applications.
+ The Application Gateway Standard tier offers all the L7 functionality, including load balancing, The WAF tier adds a firewall to check for malicious traffic.
+ An Application Gateway can make routing decisions based on additional attributes of an HTTP request, for example URI path or host headers.