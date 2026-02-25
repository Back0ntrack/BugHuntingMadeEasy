---
layout:
  width: wide
  title:
    visible: false
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
  metadata:
    visible: false
  tags:
    visible: true
---

# Core architectural components of Azure

## What is Microsoft Azure

Azure is a continually expanding set of cloud services that help you meet current and future business challenges. Azure gives you the freedom to build, manage, and deploy applications on a massive global network using your favorite tools and frameworks.

## Azure Governance Hierarchy

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### Management Groups (Top Level Governance Layer)

Management Groups are used to organize multiple Azure subscriptions. Large enterprise don't have 1 subscription. They may have multiple subscriptions. Managing each individually would be chaos. So Azure provides Management Groups to apply:

* Policies
* RBAC Roles
* Compliance Controls

at a higher level.

> _**It's a governance boundary, not a billing boundary.**_

{% hint style="info" %}
_If you apply a **policy at Management Group level**, it automatically applies to all child subscriptions. Management Groups can be nested._
{% endhint %}

#### What Management Groups Control

* Azure Policy
* RBAC role assignments
* Compliance standards
* Security baselines

<figure><img src="../../.gitbook/assets/management-groups-subscriptions-dfd5a108-60f31f5a.png" alt=""><figcaption></figcaption></figure>

Assigning Azure RBAC at the management group level means that all sub-management groups, subscriptions, resource groups, and resources underneath that management group would also inherit those permissions.

#### Important Facts

* 10,000 management groups can be supported in a single directory.
* A management group tree can support up to six levels of depth. This limit doesn't include the root level or the subscription level.
* Each management group and subscription can support only one parent.

### Subscription (Billing + Security Boundary)

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

A subscription provides you with authenticated and authorized access to Azure products and services.

A subscription is a container for resources and also a billing boundary. You cannot create resources without a subscription.

Organization use multiple subscription to:

* Isolate production from development
* Separate workloads
* Budget separation
* Compliance separation

#### What Subscription Control

* Billing account
* Access control (RBAC)
* Resource limits (quotas)
* Cost tracking
* Security boundary

### Resource Groups (Logical Container Layer)

A Resource Group is a logical container that holds related Azure resources. If you're deploying a web app, you may create:

* Web App
* SQL Database
* Storage Account
* Virtual Network

All inside ONE Resource Group.

{% hint style="warning" %}
_A resource group has a location but it only stores the metadata of the resources. Also there is no restriction that resources from only a single region can be stored in it._
{% endhint %}

#### Key Points

{% stepper %}
{% step %}
A resource belongs to only ONE resource group.
{% endstep %}

{% step %}
Resource groups cannot contain other resource groups.
{% endstep %}

{% step %}
They are used for lifecycle management.

* If you delete a resource group everything inside it gets deleted.
{% endstep %}
{% endstepper %}

#### What Resource Group Controls

* RBAC roles
* Policies
* Tags

### Resources

These are the actual Azure services. It may includes resources such as Azure virtual machines, Azure SQL Database, Storage accounts, App service, Key vault, Kubernetes services. They only inherit

* Policies
* RBAC
* Tags

from Management Groups -> Subscription -> Resources Group

{% hint style="info" %}
_A single resource can be in one resource group only._
{% endhint %}

### Inheritance Model

Azure follows a **parent-child inheritance model**.

If you apply:

* RBAC role at Management Group → All subscriptions inherit
* Policy at Subscription → All resource groups inherit
* Policy at Resource Group → All resources inherit

Unless explicitly overridden (where allowed).

## Interacting with Azure

### Using Azure Portal

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

It is not ideal for large-scale automation and CI/CD Pipelines.

### Using Command Line Interface

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

You can quickly change between PowerShell and BASH in the CLI by selecting the **Switch to ...** button or entering `BASH` or `PWSH`.

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

### Using Azure CLI

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

### Using Windows Utilities

We can connect to Azure using Windows Utilities such as PowerShell or Command Prompt .

## Azure Physical Infrastructure

### Regions

A region is a geographical area on the planet that contains at least one, but potentially multiple datacenters that are nearby and networked together with a low-latency network. When you deploy a resource in Azure, you'll often need to choose the region where you want your resource deployed.

{% hint style="info" %}
_Some services or virtual machine (VM) features are only available in certain regions, such as specific VM sizes or storage types. There are also some global Azure services that don't require you to select a particular region, such as Microsoft Entra ID, Azure Traffic Manager, and Azure DNS._
{% endhint %}

#### Why regions matter

When you deploy a resource, you must select a region.

Reasons:

* Latency
* Compliance
* Data residency laws
* Disaster recovery planning

### Availability Zones

An **Availability Zone (AZ)** is:

> One or more physically separate datacenters within the same region.

Each zone has:

* Independent power
* Independent cooling
* Independent networking

#### Why It Exists

To protect against:

* Power failure
* Cooling failure
* Hardware failure
* Local disaster

#### Example

In a region like East US:

* Zone 1
* Zone 2
* Zone 3

If Zone 1 fails → Zone 2 and 3 continue working.

{% hint style="info" %}
_To ensure resiliency, a minimum of three separate availability zones are present in all availability zone-enabled regions. However, not all Azure Regions currently support availability zones._
{% endhint %}

### Region Pairs (Disaster Recovery Design)

<figure><img src="../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

Azure pairs regions within the same geography at least 300 miles away.

Examples:

* East US ↔ West US
* North Europe ↔ West Europe
* Central India ↔ South India

#### Why Region Pairing Exists

* Disaster recovery
* Planned maintenance sequencing
* Data replication

If one region goes down due to large-scale disaster:\
Paired region can act as DR.

Microsoft ensures:

* Updates are rolled out one region at a time in a pair.
* Not both go down for maintenance simultaneously.
