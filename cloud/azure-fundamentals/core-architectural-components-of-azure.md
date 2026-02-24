# Core architectural components of Azure

## What is Microsoft Azure

Azure is a continually expanding set of cloud services that help you meet current and future business challenges. Azure gives you the freedom to build, manage, and deploy applications on a massive global network using your favorite tools and frameworks.

## Azure Governance Hierarchy

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Assigning Azure RBAC at the management group level means that all sub-management groups, subscriptions, resource groups, and resources underneath that management group would also inherit those permissions.

#### Important Facts

* 10,000 management groups can be supported in a single directory.
* A management group tree can support up to six levels of depth. This limit doesn't include the root level or the subscription level.
* Each management group and subscription can support only one parent.

### Subscription (Billing + Security Boundary)

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

It is not ideal for large-scale automation and CI/CD Pipelines.

### Using Command Line Interface

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

You can quickly change between PowerShell and BASH in the CLI by selecting the **Switch to ...** button or entering `BASH` or `PWSH`.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Using Azure CLI

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

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

## Azure Compute and Networking services

### Azure Virtual machines (IaaS)

#### Virtual Machine Scale Sets

Virtual Machine Scale Sets (VMSS) allow you to **create and manage a group of identical, load-balanced virtual machines** in Azure.

If created manually, you must:

* Configure each VM identically
* Set up load balancing manually
* Manage network routing
* Monitor usage
* Scale up/down manually

**Azure automatically:**

* Manages multiple identical VMs centrally
* Configures and updates all VMs together
* Automatically deploys a **load balancer**
* Scales VM instances up/down based on:
  * Demand (auto-scale)
  * Defined schedule (scheduled scaling)

#### Availability set

Availability sets accomplish these objectives by grouping VMs in two ways: update domain and fault domain.

* **Update Domain:** An **Update Domain** is a logical group of VMs that are **rebooted and updated at the same time**.
  * Only **one update domain** is taken offline at a time.
  * All VMs inside that domain update together.
  * Each update domain gets **30 minutes to recover** before the next domain is updated.
  * Ensures **continuous availability during planned maintenance downtime**.
* **Fault domain**: A **Fault Domain** is a group of VMs that share the **same physical hardware resources** (power source and network switch).
  * Availability sets distribute VMs across **up to 3 fault domains (by default)**.
  * Each fault domain has **separate power and networking resources**.
  * If one fault domain fails, others remain unaffected.
  * Ensures **continuous availability during unplanned hardware failures.**

### Azure Virtual Desktop (AVD)

Azure Virtual Desktop is a **desktop and application virtualization service** that runs on Azure.

It allows users to access:

* A full **Windows desktop**
* Individual applications

#### Azure Virtual Desktop - Security Features

* Uses **Microsoft Entra ID** for centralized identity and access management.
* Supports **Multi-Factor Authentication (MFA)** for secure sign-in.
* Provides **Role-Based Access Control (RBAC)** for granular permissions.
* It ensures that sensitive data is not left on a user's personal device.

### Azure Containers (PaaS)

Virtual machines run a full operating system per instance, while containers share the host OS and allow multiple lightweight application instances to run efficiently on a single machine.

Containers are lightweight and designed to be created, scaled out, and stopped dynamically.

{% hint style="info" %}
_Containers allow fast restart._
{% endhint %}

#### Azure Container Apps

Azure Container Apps let you run and scale containers easily without managing infrastructure.

* PaaS service for running containers.
* No container infrastructure management required.
* Supports built-in **scaling and load balancing**.
* Designed for **elastic and scalable applications**.

#### Azure Kubernetes Service (AKS)

AKS automates deployment and lifecycle management of large-scale containerized applications.

* A **container orchestration service**.
* Manages container lifecycle (deploy, scale, update, monitor).
* Ideal for managing large fleets of containers.

#### Azure Container apps (microservices)

Containers enable microservices by allowing applications to be split into independent, scalable components.

* Used in **microservice architecture**.
* Application is divided into smaller, independent services.
* Each service runs in its own container.
* Services can be:
  * Scaled independently
  * Updated independently
  * Maintained independently

**Example:**

* Frontend container
* Backend container
* Database container

for a single website we're maintaining three different containers increasing security and ease of maintenance.

### Azure Functions (SaaS)

**Azure Functions** is an **event-driven, serverless compute service**.

Azure Functions is a serverless, event-driven compute service that runs code only when triggered, without requiring continuously running VMs or containers.

Example: When a new order is placed in an online store, an Azure Function automatically triggers to send a confirmation email and update the inventory, then stops after completing the task.

Functions are commonly used when you need to perform work in response to an event (often via a REST request), timer, or message from another Azure service, and when that work can be completed quickly, within seconds or less.

#### Azure Functions – Key Points

* Used when you only care about **running code**, not managing infrastructure.
* Executes in response to **events, REST requests, timers, or messages**.
* Best for **short-running tasks (seconds or less)**.
* **Automatically scales** based on demand.
* Charges only for **execution time (CPU usage)**.
* Automatically deallocates resources after execution.
* Can be:
  * **Stateless (default)** → No memory of previous runs
  * **Stateful (Durable Functions)** → Maintains execution context

### Azure App Service

#### Hosting Options in Azure

* Virtual Machines
* Containers
* Kubernetes
* Azure App Service

Azure App Service lets you focus on building and maintaining your app, and Azure focuses on keeping the environment up and running.

Azure App Service is a **fully managed PaaS (Platform as a Service)** for hosting:

* Web apps
* REST APIs
* Mobile back ends
* Background jobs

#### Key features of Azure App services

* No infrastructure management
* Automatic scaling
* High availability
* Supports Windows and Linux
* Continuous deployment (GitHub, Azure DevOps, Git repos)
* Supports multiple languages:
  * Java
  * PHP
  * Python
  * JavaScript (Node.js)
  * .NET / .NET Core

#### Types of App services

{% stepper %}
{% step %}
Web Apps

App Service includes full support for hosting web apps by using ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP, or Python. You can choose either Windows or Linux as the host operating system.
{% endstep %}

{% step %}
API apps

You can build REST-based web APIs by using your choice of language and framework. You get full Swagger support and the ability to package and publish your API in Azure Marketplace. The produced apps can be consumed from any HTTP- or HTTPS-based client.
{% endstep %}

{% step %}
WebJobs

You can use the WebJobs feature to run a program (.exe, Java, PHP, Python, or Node.js) or script (.cmd, .bat, PowerShell, or Bash) in the same context as a web app, API app, or mobile app. They can be scheduled or run by a trigger. WebJobs are often used to run background tasks as part of your application logic.
{% endstep %}

{% step %}
Mobile Apps

Use the Mobile Apps feature of App Service to quickly build a back end for iOS and Android apps. With just a few actions in the Azure portal, you can:

* Store mobile app data in a cloud-based SQL database.
* Authenticate customers against common social providers, such as MSA, Google, X, and Facebook.
* Send push notifications.
* Execute custom back-end logic in C# or Node.js.
{% endstep %}
{% endstepper %}

### Azure Virtual Networking

Azure virtual networks and virtual subnets enable Azure resources, such as VMs, web apps, and databases, to communicate with each other, with users on the internet, and with your on-premises client computers. You can think of an Azure network as an extension of your on-premises network with resources that link other Azure resources.

Azure virtual networks provide the following key networking capabilities:

* Isolation and segmentation
* Internet communications
* Communicate between Azure resources
* Communicate with on-premises resources
  * Point-to-Site
    * Individual device (machine outside organization) can securely connects to Azure VNet using encrypted VPN connection.
  * Site-to-Site
    * Connects on-premises network to Azure VNet using VPN gateway.
  * Azure ExpressRoute
    * Dedicated private connection to Azure without using the public internet.
* Route network traffic
* Filter network traffic
* Connect virtual networks

Azure virtual networking supports both public and private endpoints to enable communication between external or internal resources with other internal resources.

* Public endpoints have a public IP address and can be accessed from anywhere in the world.
* Private endpoints exist within a virtual network and have a private IP address from within the address space of that virtual network.

### Azure ExpressRoute

Azure ExpressRoute extends your on-premises network to Microsoft cloud services like Azure and Microsoft 365 through a dedicated private connection called an ExpressRoute circuit. It does not use the public internet, providing higher reliability, faster speeds, consistent latency, and improved security compared to standard internet-based connections.

ExpressRoute enables direct access to the following services in all regions:

* Microsoft Office 365
* Microsoft Dynamics 365
* Azure compute services, such as Azure Virtual Machines
* Azure cloud services, such as Azure Cosmos DB and Azure Storage

### Azure DNS

Azure DNS is a hosting service for DNS domains that provides name resolution by using Microsoft Azure infrastructure. By hosting your domains in Azure, you can manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services.

Azure DNS uses the scope and scale of Microsoft Azure to provide numerous benefits, including:

* Reliability and performance
* Security
* Ease of Use
* Customizable virtual networks
* Alias records
