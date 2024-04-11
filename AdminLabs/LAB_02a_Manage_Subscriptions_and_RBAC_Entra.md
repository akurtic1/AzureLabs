---
lab:
    title: 'Lab 02: Manage Subscriptions and RBAC'
    module: 'Administer Governance and Compliance
'
---

# Lab 02 - Manage Subscriptions and RBAC

## Lab introduction

In this lab, I learned about RBAC and how to use permissions and scope to control what actions identities can and cannot perform.
I also make subscription management easier using management groups.

## Lab scenario

To simplify management of Azure resources in your organization, you have been tasked with implementing the following functionality:

Creating a management group that includes all your Azure subscriptions.

Granting permissions to submit support requests for all subscriptions in the management group. The permissions should be limited only to:

Create and manage virtual machines
Create support request tickets (do not include adding Azure providers)

## Architecture diagram
![Diagram of the lab 02 architecture.](../AdminLabs/Media/az104-lab02a-architecture.png)

## Tasks:

+ Task 1: Implement management groups.
+ Task 2: Review and assign a built-in Azure role.
+ Task 3: Create a custom RBAC role.
+ Task 4: Monitor role assignments with the Activity Log.
   
### Implement Management Groups

    | Setting | Value |
    | --- | --- |
    | Management group ID | `az104-mg1` (must be unique in the directory) |
    | Management group display name | `az104-mg1` |

    Resources: Microsoft Entra ID

### Review and assign a built-in Azure role

In this task, I used Management group with (IAM) Blade and managed roles. I reviewed information
about Permissions, JSON and Assignments. (Owner, Contributor and Reader).
After this short management, I selected Virtual Machine Contributor which helps users to manage virtual machines,
but not access their operating system or vritual network they are connected to.

### Create a custom RBAC role

In this task I was still using Management Groups and Access Control Bade to create a new role and
remove permissions that are not be necessary.

    | Setting | Value |
    | --- | --- |
    | Custom role name | `Custom Support Request` |
    | Description | ``A custom contributor role for support requests.` |

### Monitor role assignments with the Activity Log

I used activity log to determine if anyone has created a new role.

## Key takeaways

+ Management groups are used to logically organize subscriptions.
+ The built-in root management group includes all the management groups and subscriptions.
+ Azure has many built-in roles. You can assign these roles to control access to resources.
+ You can create new roles or customize existing roles.
+ Roles are defined in a JSON formatted file and include *Actions*, *NotActions*, and *AssignableScopes*.
+ You can use the Activity Log to monitor role assignments.
