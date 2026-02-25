---
noIndex: true
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

# Azure Compute and Network services

## Azure Virtual machines (IaaS)

### Virtual Machine Scale Sets

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

### Availability set

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

## Azure Virtual Desktop (AVD)

Azure Virtual Desktop is a **desktop and application virtualization service** that runs on Azure.

It allows users to access:

* A full **Windows desktop**
* Individual applications

### Azure Virtual Desktop - Security Features

* Uses **Microsoft Entra ID** for centralized identity and access management.
* Supports **Multi-Factor Authentication (MFA)** for secure sign-in.
* Provides **Role-Based Access Control (RBAC)** for granular permissions.
* It ensures that sensitive data is not left on a user's personal device.

## Azure Containers (PaaS)

Virtual machines run a full operating system per instance, while containers share the host OS and allow multiple lightweight application instances to run efficiently on a single machine.

Containers are lightweight and designed to be created, scaled out, and stopped dynamically.

{% hint style="info" %}
_Containers allow fast restart._
{% endhint %}

### Azure Container Apps

Azure Container Apps let you run and scale containers easily without managing infrastructure.

* PaaS service for running containers.
* No container infrastructure management required.
* Supports built-in **scaling and load balancing**.
* Designed for **elastic and scalable applications**.

### Azure Kubernetes Service (AKS)

AKS automates deployment and lifecycle management of large-scale containerized applications.

* A **container orchestration service**.
* Manages container lifecycle (deploy, scale, update, monitor).
* Ideal for managing large fleets of containers.

### Azure Container apps (microservices)

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

## Azure Functions (SaaS)

**Azure Functions** is an **event-driven, serverless compute service**.

Azure Functions is a serverless, event-driven compute service that runs code only when triggered, without requiring continuously running VMs or containers.

Example: When a new order is placed in an online store, an Azure Function automatically triggers to send a confirmation email and update the inventory, then stops after completing the task.

Functions are commonly used when you need to perform work in response to an event (often via a REST request), timer, or message from another Azure service, and when that work can be completed quickly, within seconds or less.

### Azure Functions – Key Points

* Used when you only care about **running code**, not managing infrastructure.
* Executes in response to **events, REST requests, timers, or messages**.
* Best for **short-running tasks (seconds or less)**.
* **Automatically scales** based on demand.
* Charges only for **execution time (CPU usage)**.
* Automatically deallocates resources after execution.
* Can be:
  * **Stateless (default)** → No memory of previous runs
  * **Stateful (Durable Functions)** → Maintains execution context

## Azure App Service

### Hosting Options in Azure

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

### Key features of Azure App services

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

### Types of App services

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

## Azure Virtual Networking

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

## Azure ExpressRoute

Azure ExpressRoute extends your on-premises network to Microsoft cloud services like Azure and Microsoft 365 through a dedicated private connection called an ExpressRoute circuit. It does not use the public internet, providing higher reliability, faster speeds, consistent latency, and improved security compared to standard internet-based connections.

ExpressRoute enables direct access to the following services in all regions:

* Microsoft Office 365
* Microsoft Dynamics 365
* Azure compute services, such as Azure Virtual Machines
* Azure cloud services, such as Azure Cosmos DB and Azure Storage

## Azure DNS

Azure DNS is a hosting service for DNS domains that provides name resolution by using Microsoft Azure infrastructure. By hosting your domains in Azure, you can manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services.

Azure DNS uses the scope and scale of Microsoft Azure to provide numerous benefits, including:

* Reliability and performance
* Security
* Ease of Use
* Customizable virtual networks
* Alias records
