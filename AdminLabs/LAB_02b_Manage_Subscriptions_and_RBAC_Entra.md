---
lab:
    title: 'Lab 02b: Manage Governance via Azure Policy'
    module: 'Administer Governance and Compliance'
---


# Lab 02b - Manage Governance via Azure Policy

## Lab introduction

In this lab, I learned how to implement organization's governance plans. I used Azure Policies and tags to improve reporting.

## Lab scenario

Your organization’s cloud footprint has grown considerably in the last year. During a recent audit, you discovered a substantial number of resources that do not have a defined owner, project, or cost center. In order to improve management of Azure resources in your organization, you decide to implement the following functionality:

apply resource tags to attach important metadata to Azure resources

enforce the use of resource tags for new resources by using Azure policy

update existing resources with resource tags

use resource locks to protect configured resources

## Architecture diagram
![Diagram of the lab 02 architecture.](D:/AzureProjects/AdmirLabs/az104-lab01-architecture.png)

## Tasks:

+ Task 1: Create and assign tags via the Azure portal.
+ Task 2: Enforce tagging via an Azure Policy.
+ Task 3: Apply tagging via an Azure Policy.
+ Task 4: Configure and test resource locks.
   
### Create and assign tags via the Azure portal

In this task, I created an Azure resource group and assigned a tag via Azure portal.

**Resource Group**
    | Setting | Value |
    | --- | --- |
    | Subscription name | your subscription |
    | Resource group name | `az104-rg2` |
    | Location | **East US** |

**Next: Tags**
| Setting | Value |
    | --- | --- |
    | Name | `Cost Center` |
    | Value | `000` |

### Enforce tagging via an Azure Policy

In this task, I assigned the policy "*Reguire a Tag*" to the resource group created in previous task.

1. In the section below, I specified the scope by selecting the resource group and subscription.
    | Setting | Value |
    | --- | --- |
    | Subscription | *your subscription* |
    | Resource Group | **az104-rg2** |


1. Configuring the **Basics** properties of the assignment by specifying the following settings.

    | Setting | Value |
    | --- | --- |
    | Assignment name | `Require Cost Center tag with Default value`|
    | Description | `Require Cost Center tag with default value for all resources in the resource group`|
    | Policy enforcement | Enabled |

1. **Parameters**

    | Setting | Value |
    | --- | --- |
    | Tag Name | `Cost Center` |
    | Tag Value | `000` |

### Checking if the policy is working

In this task, I tried to create the storage account and I didn't fill the properties information. Since the policy that I assigned is not allowing me to create the
storage account without using "Tag Name" and "Tag Value", with this we can confirm that the policy is working.

## Policy Error
![Diagram of the lab 02 architecture.](D:/AzureProjects/AdmirLabs/az104-lab01-architecture.png)
    

### Apply tagging via Azure Policy

In this task, I used the new policy definition to remediate any non-compliant resources. In this secnarion, I created that any child resources
of a resource group will inherit the **Cost Center Tag** that was defined on the resource group.

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

### Testing the resource lock

In this task, I tested if the resource lock is working. I received the notice warning: Deleting this resource group and its dependent resources is a permanent action and cannot be undone. Select Delete.
**Deleting the resource group az104-rg2 failed.**

## Key takeaways

+ Azure tags are metadata that consists of a key-value pair. Tags describe a particular resource in your environment. In particular, tagging in Azure enables you to label your resources in a logical manne.
+ Azure Policy establishes conventions for resources. Policy definitions describe resource compliance conditions and the effect to take if a condition is met. A condition compares a resource property field or a   value to a required value. There are many built-in policy definitions and you can customize the policies.
+ The Azure Policy remediation task feature is used to bring resources into compliance based on a definition and assignment. Resources that are non-compliant to a modify or deployIfNotExist definition assignment, can be brought into compliance using a remediation task.
+ You can configure a resource lock on a subscription, resource group, or resource. The lock can protect a resource from accidental user deletions and modifications. The lock overrides any user permissions.
Azure Policy is pre-deployment security practice. RBAC and resource locks are post-deployment security practice.