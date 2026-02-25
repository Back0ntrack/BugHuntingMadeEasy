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

# Describe Cloud concepts

## Cloud Computing

Cloud computing is the delivery of computing services over the internet. These services include core IT infrastructure such as virtual machines, storage, databases, and networking, along with advanced capabilities like IoT, machine learning (ML), and artificial intelligence (AI).

### Shared Responsibility Model

<figure><img src="../../.gitbook/assets/shared responsibility b3829bfe.png" alt=""><figcaption></figcaption></figure>

The shared responsibility model explains how security and management duties are divided between the cloud provider and the customer. In a traditional on-premises datacenter, the organization is responsible for everything—physical security, power, hardware maintenance, infrastructure, software updates, and system patching.

In cloud computing, these responsibilities are shared which is described in this shared responsibility model.

***

### Cloud Models

#### Private Cloud

* Used exclusively by a single organization
* Can be hosted on-premises or in a dedicated offsite datacenter (even by a third party but it'll provide services to your organization only).
* Offers full control over resources and security
* Higher cost (hardware purchase + maintenance required)
* Organization responsible for infrastructure management

E.g., _A bank hosts its core banking systems in its own dedicated datacenter to maintain full control and security._

> Real Life Non-Technical Example: _The car is 100% yours. Only you (and those you invite) can use it. You decide how it looks, how it’s driven, and where it’s parked._

#### Public Cloud

* Built and managed by a third-party cloud provider
* Resources available to any customer
* No capital expenditure (pay-as-you-go model)
* Rapid provisioning and scalability
* Less direct control over infrastructure

E.g., _A startup deploys its web application on Microsoft Azure and pays only for the resources it uses._

> Real Life Non-Technical Example: _You don’t own the bus or drive it. You just pay for your seat (subscription) and get a ride. Other people are on the bus too, but you all stay in your own seats._

#### Hybrid Cloud

* Combination of private and public clouds
* Enables workload flexibility (choose where to run applications)
* Supports demand surges using public cloud
* Helps meet security, compliance, and legal requirements
* Provides high flexibility and control balance

E.g., _An e-commerce company keeps customer payment data in a private cloud but uses the public cloud to handle traffic spikes during sales._

#### Multi-Cloud

* Use of multiple public cloud providers
* May be used for feature optimization or migration strategy
* Requires managing resources and security across providers

#### Azure Arc

* Centralized management across public, private, hybrid, and multi-cloud environments
* Extends Azure management capabilities to non-Azure resources

#### Azure VMware Solution

* Runs VMware workloads directly in Azure
* Enables migration from private VMware environments
* Provides seamless integration and scalability

***

### Pricing Model

#### CapEx vs OpEx

* **Capital Expenditure (CapEx):** One-time upfront investment to acquire physical assets (e.g., buildings, datacenters, hardware, vehicles).
* **Operational Expenditure (OpEx):** Ongoing spending on services or products over time (e.g., leasing, subscriptions, cloud services).

#### Cloud Computing and OpEx

* Cloud computing follows a **consumption-based (pay-as-you-go) model**.
* No need to purchase or maintain physical infrastructure.
* Pay only for the resources you use.
* If no resources are used, no cost is incurred.

***

## Benefits of Cloud Services

### High Availability (HA)

* Ensures IT resources remain accessible despite failures or disruptions.
* Critical when deploying applications or services that must stay online.
* Azure provides uptime guarantees through **Service Level Agreements (SLAs)** in percentage.
* Availability level depends on the specific Azure service used.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### Scalability

* Ability to adjust resources based on demand.
* Handles traffic spikes by increasing resources.
* Reduces costs by scaling down when demand decreases.
* Supports the cloud’s consumption-based (pay-for-what-you-use) model.

#### Types of Scaling

**Vertical Scaling (Scale Up/Down)**

* Increase or decrease resource capacity (e.g., CPU, RAM).
* Example: Upgrading a VM to more CPU/RAM for higher performance.

**Horizontal Scaling (Scale Out/In)**

* Add or remove resource instances.
* Example: Adding more VMs or containers during high traffic; removing them when demand drops.

### Reliability

* Ability of a system to recover from failures and continue operating.
* A core pillar of the **Azure Well-Architected Framework**.
* Azure’s decentralized, global infrastructure increases resilience.
* Resources can be deployed across multiple regions.
* If one region fails, others remain operational.
* Applications can be designed for automatic failover to maintain continuity.

### Predictability

* Ensures confidence in both performance and cost outcomes.
* Strongly guided by the Azure Well-Architected Framework.
* Well-architected solutions provide stable performance and controlled costs.

#### Performance Predictability

* Focuses on delivering consistent user experience.
* Supported by autoscaling, load balancing, and high availability.
* Autoscaling adjusts resources automatically based on demand.
* Load balancing distributes traffic to prevent overload.

#### Cost Predictability

* Focuses on forecasting and controlling cloud spending.
* Real-time monitoring of resource usage.
* Use analytics to identify usage patterns and optimize costs.
* Tools like TCO Calculator and Pricing Calculator help estimate expenses.

### Management of Cloud

Management in the cloud speaks to how you’re able to manage your cloud environment and resources. You can manage these:

* Through a web portal.
* Using a command line interface.
* Using APIs.
* Using PowerShell.

## Cloud Service Types

### Infrastructure-as-a-Service (IaaS)

* Most flexible cloud service model with maximum customer control.
* Cloud provider manages physical infrastructure (hardware, networking, physical security).
* Customer manages operating system, configurations, updates, networking setup, databases, and storage.
* Suitable when full control over the environment is required.

E.g., _A company deploys virtual machines in Azure to host its custom web application, managing the operating system, configurations, and updates itself._

### Platform as a Service (PaaS)

* Positioned between IaaS and SaaS.
* Cloud provider manages physical infrastructure, internet connectivity, operating systems, databases, middleware, and development tools.
* Customer focuses only on application code and data.
* No need to manage OS updates, patching, database maintenance, or licensing.
* Comparable to using a domain-joined machine where IT handles system updates and maintenance.
* Ideal for developing and deploying applications without managing underlying infrastructure.

E.g., _A development team builds and deploys a web app using Azure App Service, focusing only on the application code while Azure manages the OS, runtime, and infrastructure_.

### Software as a Service (SaaS)

* Most complete cloud service model; delivers fully developed applications over the internet.
* Examples include email, messaging platforms, financial software, and productivity tools.
* Least flexible model but easiest to deploy and use.
* Requires minimal technical expertise from the customer.
* Cloud provider holds the majority of responsibility.
* Provider manages physical infrastructure, security, power, networking, application maintenance, and patching.
* Customer is responsible for their data, user access management, and connected devices.

E.g., _An organization uses Microsoft 365 for email and collaboration, simply managing users and data while Microsoft handles the entire application and infrastructure._
