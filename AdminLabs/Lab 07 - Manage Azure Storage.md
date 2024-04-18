---
lab:
    title: 'Lab 07: Manage Azure storage'
    module: 'Administer Azure Storage'
---

# Lab 07 - Manage Azure Storage

## Lab introduction

In this lab, I used Azure Storage to create Azure Blobs and Azure Files. I also configured and secured blob containers.
## Lab scenario

Your organization is currently storing data in on-premises data stores. Most of these files are not accessed frequently. You would like to minimize the cost of storage by placing infrequently accessed files in lower-priced storage tiers. You also plan to explore different protection mechanisms that Azure Storage offers, including network access, authentication, authorization, and replication. Finally, you want to determine to what extent Azure Files is suitable for hosting your on-premises file shares.

## Tasks:

+ Task 1: Create and configure a storage account.
+ Task 2: Create and configure secure blob storage.
+ Task 3: Create and configure secure Azure file storage.

![Data Storage Architecture.](../AdminLabs/Media/az104-lab07-architecture.png)


### Creating and configure a storage account.

In this task, I created a Data Storage account and configured the following Basic properties:

    | Setting | Value |
    | --- | --- |
    | Subscription          | my subscription  |
    | Resource group        | **az104-rg7** |
    | Storage account name  | azstoragedemo |
    | Region                | **(US) East US**  |
    | Performance           | **Standard** ( |
    | Redundancy            | **Geo-redundant storage** )|

I also checked and went trough the material and other properties tabs such as encryption, networking and data protection.

### Creating a blob container and a time-based retention policy

In this task, I created a blob container and uploaded a random file.
After creating blob container I assigned a policy.

**Adding Policy**

    | Setting | Value |
    | --- | --- |
    | Policy type | **Time-based retention**  |
    | Set retention period for | `180` days |

I also review the other options in the Blob Storage, such as **Download**, **Delete**, **Change tier**, and **Acquire lease**.

### Configuring limited access to the blob storage

In this task, I used "Generate SAS" and configured the following properties:

    | Setting | Value |
    | --- | --- |
    | Signing key | **Key 1** |
    | Permissions | **Read**  |
    | Start date | yesterday's date |
    | Start time | current time |
    | Expiry date | tomorrow's date |
    | Expiry time | current time |
    | Allowed IP addresses | blank |

After this configuration and used the URL and tested if the file is available. (It was successfull)

### Creating and configuring an Azure File storage

In this task, I crated an Azure File Storage and uploaded a random file.
After this task, I deployed a Virtual Network.

After configuring the Virtual network, In the **Firewall** section, I made sure that my local machine IP is deleted.
I tested if I'll be able to navigate to blob storage or azure files. As expected, I received the following message "*not authorized to perform this operation*".

## Key takeaways

+ An Azure storage account contains all your Azure Storage data objects: blobs, files, queues, and tables. The storage account provides a unique namespace for your Azure Storage data that is accessible from anywhere in the world over HTTP or HTTPS.
+ Azure storage provides several redundancy models including Locally redundant storage (LRS), Zone-redundant storage (ZRS), and Geo-redundant storage (GRS). 
+ Azure blob storage allows you to store large amounts of unstructured data on Microsoft's data storage platform. Blob stands for Binary Large Object, which includes objects such as images and multimedia files.
+ Azure file Storage provides shared storage for structured data. The data can be organized in folders.
+ Immutable storage provides the capability to store data in a write once, read many (WORM) state. Immutable storage policies can be time-based or legal-hold.
