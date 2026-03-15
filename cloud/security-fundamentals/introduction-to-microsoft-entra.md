# Introduction to Microsoft Entra

<figure><img src="../../.gitbook/assets/image (380).png" alt=""><figcaption></figcaption></figure>

Microsoft Entra ID, formerly Azure Active Directory, is Microsoft’s cloud-based identity and access management service. Organizations use Microsoft Entra ID to enable their employees, guests, and others to sign in and access the resources they need, including:

* Internal resources, such as apps on your corporate network and intranet, and cloud apps developed by your own organization.
* External services, such as Microsoft Office 365, the Azure portal, and any SaaS applications used by your organization.

{% hint style="info" %}
_Microsoft Entra ID can be synchronized with your existing on-premises Active Directory, synchronized with other directory services, or used as a standalone service._
{% endhint %}

### Identity Secure Score&#x20;

It is a percentage that functions as an indicator for how aligned you are with Microsoft's best practice recommendations for security.&#x20;

<figure><img src="../../.gitbook/assets/image (381).png" alt=""><figcaption></figcaption></figure>

## Basic Terminology of Entra&#x20;

### **Tenant**

* An instance of **Microsoft Entra ID** representing a single organization.
* Stores organizational objects such as users, groups, devices, and application registrations.
* Contains access control and compliance policies for organizational resources.
* Has a unique **Tenant ID** and domain name (e.g., contoso.onmicrosoft.com).
* Acts as the **security and administrative boundary** for managing resources, applications, devices, and services.

### **Directory**

* Logical container within a **Microsoft Entra tenant** that organizes identity-related objects.
* Stores users, groups, devices, applications, and other directory objects.
* Works like a **database or catalog of identities and resources**.
* The terms **tenant** and **directory** are often used interchangeably.
* Each **tenant contains only one directory**.

### **Multi-tenant**

* An organization that uses **multiple Microsoft Entra ID tenants**.
* Common scenarios include:
  * Organizations with **multiple subsidiaries or independent business units**.
  * **Mergers and acquisitions** where separate identity environments exist.
  * **Geographical regions** requiring different data residency or regulatory compliance.
* Allows organizations to **manage identities separately across different environments**.

## Identities in Microsoft&#x20;

* An identity represents **something that can be authenticated**.
* It can be a **person, application, service, or device** that needs access to resources.
* Identities allow systems to **verify who or what is requesting access**.
* Once authenticated, permissions determine **what the identity can do**.
* Identities are used to access:
  * Applications
  * Devices
  * Networks
  * Data
  * Cloud services
* Identity is the **security boundary in modern cloud systems**, meaning access control is based on identity rather than just network location.

## Types of Identities&#x20;

### User Identities (Human Identities)

<figure><img src="../../.gitbook/assets/image (382).png" alt=""><figcaption></figcaption></figure>

* Represent **people** who need access to organizational resources.
* Examples: employees, administrators, customers, partners, vendors, or consultants.
* Usually stored as **user objects** in the directory.
* Can be:
  * **Internal users** – employees inside the organization.
  * **External users** – partners or collaborators invited through external identity systems.
* These identities authenticate using credentials such as username/password, MFA, certificates, or other authentication methods.

### Device Identities&#x20;

* Represent **physical devices** connected to an organization’s environment.
* Examples: laptops, desktops, mobile phones, servers, or IoT devices.
* Device identities help verify that the device accessing resources is trusted and compliant.
* Stored as **device objects** in the directory with information such as operating system, associated user, and compliance state.

#### Ways to set up device identities&#x20;

* **Microsoft Entra Registered devices**
  * Main goal is to provide users with support for bring your own device (BYOD) or mobile device scenarios.&#x20;
  * A user can access your organization's resource using a personal device.&#x20;
* **Microsoft Entra Joined**&#x20;
  * Device joined to Microsoft Entra ID through an organizational account, which is then used to sign in the device.&#x20;
  * These devices are generally owned by the organization.&#x20;
* **Microsoft Entra Hybrid Joined Devices**&#x20;
  * Organizations with existing on-premises Active Directory implementations can benefit from the functionality provided by Microsoft Entra ID by implementing Microsoft Entra hybrid joined devices.&#x20;
  * These devices are joined to your on-premises Active Directory and Microsoft Entra ID requiring organizational account to sign in to the device.

### Workload Identities (Non-Human / Application Identities)

* Represent **software workloads** instead of people.
* Used by applications, services, scripts, containers, or automation tools to access resources securely.
* Allow applications to authenticate and perform tasks without human interaction.
* **Types of Workload Identities:**
  * **Service principals** – instance of an application within a tenant that defines permissions. It can be described as an identity created for an application within a tenant.&#x20;

```
Application Object
        ↓
Service Principal
```

* **Managed identities** – special type of service principals where credentials are automatically managed by Azure.

### Managed Identities&#x20;

<figure><img src="../../.gitbook/assets/image (383).png" alt=""><figcaption></figcaption></figure>

**Managed Identities (in Microsoft Entra ID / Azure)** are a special type of **workload identity** used by Azure services to **authenticate to other Azure services securely without storing credentials like passwords, keys, or secrets**.

* A managed identity is an **identity automatically created and managed by Azure for a resource**.
* It allows an Azure resource (like a VM or App Service) to **authenticate to other Azure services using Microsoft Entra ID**.
* Instead of storing credentials in code or configuration files, the Azure platform **handles authentication automatically**.
* Main purpose
  * Secure **service-to-service authentication**
  * Eliminate **hard-coded credentials**
  * Reduce **risk of credential leakage**
* Common scenario
  * An application running on Azure needs to access another Azure service such as:
    * Azure Key Vault
    * Azure Storage
    * Azure SQL Database
    * Azure Cosmos DB
  * Instead of storing passwords or connection strings, the application **uses its managed identity to authenticate**.

#### Types of Managed Identity&#x20;

1. **System-Assigned Managed Identity**

* A **system-assigned identity** is tied directly to a single Azure resource.
* Key characteristics
  * Created **automatically when enabled** on a resource
  * **One-to-one relationship** between resource and identity
  * Lifecycle tied to the resource
  * If the resource is deleted → the identity is **also deleted automatically**
* Example
  * A **Virtual Machine uses its system-assigned managed identity to securely read secrets from Azure Key Vault without storing credentials.**
* Use cases
  * When only **one resource needs the identity**
  * When identity lifecycle should **match the resource lifecycle**

***

2. **User-Assigned Managed Identity**&#x20;

* A **user-assigned identity** is created as a **separate Azure resource**.
* Key characteristics
  * Created independently from any resource
  * Can be **assigned to multiple Azure resources**
  * Lifecycle **not tied to a specific resource**
  * Remains even if assigned resources are deleted
* Example
  * Multiple **Azure Functions and VMs share a user-assigned managed identity to access the same Azure Storage account.**
* Use cases
  * When **multiple resources need the same identity**
  * When you want **central identity management**

***

### Groups&#x20;

In Microsoft Entra ID, if you have several identities with the same access needs, you can create a group. You use groups to give access permissions to all members of the group, instead of having to assign access rights individually.

#### Security Groups

* Used to **manage access to resources**.
* Most common group type in Entra ID.
* Purpose
  * Grant permissions to resources such as:
    * Applications
    * Azure resources
    * Shared services
* Characteristics
  * Used for **access control**

#### Microsoft 365 Groups&#x20;

* Used to support **collaboration across Microsoft 365 services**.
* When a Microsoft 365 group is created, it automatically creates shared resources like:
  * Shared mailbox
  * Shared calendar
  * SharePoint site
  * Planner
  * Microsoft Teams workspace
* These groups are designed for **team collaboration**, not just access control.
* Example
  * Microsoft 365 group: **Marketing Team**
  * Automatically provides
    * Outlook shared mailbox
    * SharePoint document library
    * Teams collaboration space.

### Hybrid Identity&#x20;

**Hybrid Identity** in Microsoft Entra is a model where an organization **integrates its on-premises identity system with cloud identity services**, allowing users to **use the same identity to access both on-premises resources and cloud services**.

* Inter-directory provisioning is provisioning an identity between two different directory services systems. For a hybrid environment, the most common scenario for inter-directory provisioning is when a user already in Active Directory is provisioned into Microsoft Entra ID.
* Synchronization is responsible for making sure identity information for your on-premises users and groups is matching the cloud.

{% hint style="info" %}
_One of the available methods for accomplishing inter-directory provisioning and synchronization is through Microsoft Entra Cloud Sync._
{% endhint %}

The Microsoft Entra Cloud Sync provisioning agent uses the System for Cross-domain Identity Management (SCIM) specification with Microsoft Entra ID to provision and deprovision users and groups.

### External Identities&#x20;

**External Identities** in Microsoft Entra allow **users outside an organization to securely access applications and resources**. These users are not part of the organization’s internal directory but can still authenticate and collaborate using their own identities.

<figure><img src="../../.gitbook/assets/image (384).png" alt=""><figcaption></figcaption></figure>

### Business-to-Business (B2B) Collaboration

* **B2B collaboration**
  * Allows organizations to **invite external users to access internal resources**.
  * External users authenticate using their **own identity provider**.
* External users can sign in with accounts from:
  * **Microsoft Entra ID**
  * **Microsoft Account**
  * **Google Account**
  * Other federated identity providers.
* After invitation:
  * A **guest user account** is created in the tenant.
  * The guest user gets **limited access to assigned resources**.
* Example
  * A company invites a **consultant** from another organization.
  * The consultant logs in using their existing identity.
  * They can access:
    * Shared documents
    * Teams workspace
    * Specific applications.

***

### Business-to-Customer (B2C)

* **B2C identity**
  * Designed for **applications that allow customers to sign in**.
  * Commonly used for:
    * Public websites
    * Mobile applications
    * Customer portals.
* Customers can authenticate using:
  * Email accounts
  * Social accounts
  * Federated identity providers.
* Example
  * A shopping website allows customers to sign in with:
    * Google
    * Facebook
    * Email account.
* B2C identity management handles:
  * Registration
  * Authentication
  * Password reset
  * Profile management.

| Identity Category     | Identity Type / Subtype             | Description                                                                                                                 | Example Use Case                                                    |
| --------------------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Human Identity**    | Member User                         | Internal users who belong to the organization’s tenant. Usually employees or administrators.                                | Employee logging into Microsoft 365 or internal applications.       |
| **Human Identity**    | Guest User                          | External users invited into the organization’s tenant for collaboration. They have limited access.                          | Vendor or consultant accessing shared documents or Teams workspace. |
| **Workload Identity** | Application                         | A registered application in Entra ID that needs to access APIs or other resources.                                          | Web application accessing Microsoft Graph API.                      |
| **Workload Identity** | Service Principal                   | The identity instance of an application within a specific tenant that defines what resources the application can access.    | Automation script accessing Azure resources.                        |
| **Workload Identity** | Managed Identity                    | Special identity automatically managed by Azure for services so they can authenticate securely without storing credentials. | Azure VM accessing Azure Key Vault without storing secrets.         |
| **Workload Identity** | System-Assigned Managed Identity    | Identity tied directly to one Azure resource. Created and deleted with that resource.                                       | Azure App Service accessing Azure Storage.                          |
| **Workload Identity** | User-Assigned Managed Identity      | Identity created separately and can be assigned to multiple Azure resources.                                                | Multiple VMs sharing one identity to access a database.             |
| **External Identity** | B2B Collaboration                   | Allows external users from other organizations to access internal resources using their existing identity provider.         | Partner company employee accessing your SharePoint site.            |
| **External Identity** | B2C Identity                        | Used for customer-facing applications where users sign up and sign in with social or email accounts.                        | Customers logging into an e-commerce website.                       |
| **External Identity** | Guest Account                       | Representation of an invited external user inside the tenant directory.                                                     | External consultant appearing as a guest user in the tenant.        |
| **Hybrid Identity**   | Password Hash Synchronization (PHS) | Password hashes from on-premises directory are synchronized to Entra ID and authentication occurs in the cloud.             | Employee logs into cloud services using synchronized password.      |
| **Hybrid Identity**   | Pass-Through Authentication (PTA)   | User authentication happens directly against on-premises Active Directory without storing passwords in the cloud.           | Cloud login request validated by on-premises authentication agent.  |
| **Hybrid Identity**   | Federation                          | Authentication handled by a federated identity provider such as a federation service.                                       | Organization using federation servers for login authentication.     |

## Conditional Access&#x20;

Conditional Access is a **security policy engine** and a foundational tool in Microsoft Entra ID (formerly Azure AD) that automates access decisions based on specific conditions.&#x20;

A Conditional Access policy analyses signals including user, location, device, application, and risk to automate decisions for authorizing access to resources (apps and data).

<figure><img src="../../.gitbook/assets/image (385).png" alt=""><figcaption></figcaption></figure>

### Conditional access policy components&#x20;

<figure><img src="../../.gitbook/assets/image (386).png" alt=""><figcaption></figcaption></figure>

#### Assignments&#x20;

* Define **who or what the policy applies to**.
* All assignments must be satisfied to trigger a policy.
* Includes:
  * **Users or Groups** – Specific users, groups, or roles targeted by the policy.
  * **Cloud Apps or Actions** – Applications or services the policy protects.
  * **Conditions** – Additional contextual signals such as:
    * Sign-in risk
    * Device platform
    * Location
    * Client apps.

#### **Access Controls**

* Define **what must happen for access to be granted or denied**.
* Includes:
  * **Grant Controls** – Requirements users must satisfy to gain access (e.g., MFA, compliant device).
  * **Block Access** – Completely deny access if conditions are met.
  * **Session Controls** – Control how the session behaves after login (e.g., limited session duration, monitoring).

## Global Secure Access&#x20;

<figure><img src="../../.gitbook/assets/image (387).png" alt=""><figcaption></figcaption></figure>

**Global Secure Access (GSA)** is a **unified cloud security solution within Microsoft Entra** that secures how users connect to both **internet resources and private corporate applications**.

It combines two major services:

* **Microsoft Entra Internet Access**
* **Microsoft Entra Private Access**

Together they form Microsoft’s **Security Service Edge (SSE)** architecture.

The solution is built on **Zero Trust security principles and identity based policies**:

* Verify explicitly
* Use least privilege
* Assume breach

### Components of Global Secure Access&#x20;

#### Microsoft Entra Internet Access&#x20;

**Microsoft Entra Internet Access** protects user access to:

* Internet websites
* SaaS applications
* cloud services

It functions as a **cloud-based Secure Web Gateway (SWG)**.

**Capabilities**

* Identity-based internet security
* Conditional Access enforcement
* Web filtering
* Protection from phishing and malware
* Visibility into internet traffic

This allows organizations to **control how employees access internet resources**.

#### Microsoft Entra Private Access&#x20;

**Microsoft Entra Private Access** enables secure access to:

* internal corporate applications
* on-premises resources
* private cloud resources

It implements **Zero Trust Network Access (ZTNA)** and acts as a **modern replacement for VPNs**.

**Capabilities**

* Secure access to internal applications
* No traditional VPN required
* Per-application access control
* Integration with Conditional Access
* Identity-centric security

Users can securely connect to internal resources **from any device and any network**

### Security Architecture Model&#x20;

Global Secure Access is part of Microsoft's **Security Service Edge (SSE)** architecture.

```
User / Device
      │
Global Secure Access Client
      │
Microsoft Global Security Edge
      │
Policy Enforcement
(Identity + Device + Risk)
      │
 ├── Internet resources
 └── Private corporate apps
```

Security decisions are made **before network access is granted**.

### Global Secure Access Client&#x20;

The **Global Secure Access client** runs on user devices.

Its purpose is to **route selected traffic through Microsoft’s security service**.

#### Functions

* Routes protected traffic to Microsoft security edge
* Enforces policies from Entra
* Applies traffic forwarding rules
* Maintains secure connectivity

Only traffic defined in **forwarding profiles** is routed through the service.

### Benefits of Global Secure Access&#x20;

* Unified access control: One platform secures both internet and private resources.
* Identity-centric security: Policies evaluate identity before network access.
* VPN replacement: Private Access allows secure connectivity without traditional VPN.
* Zero Trust implementation: Security decisions rely on identity, device posture, and risk signals.
* Simplified management: Administrators manage access, policies, and traffic in one place.

## Azure RBAC&#x20;

**Azure Role-Based Access Control (RBAC)** is an **authorization system in Azure** used to manage **who can access Azure resources, what actions they can perform, and the scope of that access**.

It is implemented through **Azure Resource Manager (ARM)** and provides **fine-grained access control to cloud resources**.

RBAC is mainly used to enforce the **principle of least privilege**, ensuring users receive only the permissions required to perform their tasks.

### Difference between Microsoft Entra RBAC and Azure RBAC&#x20;

* Microsoft Entra RBAC - Microsoft Entra roles control access to Microsoft Entra resources such as users, groups, and applications.
* Azure RBAC - Azure roles control access to Azure resources such as virtual machines or storage using Azure Resource Management.

### Purpose of Azure RBAC&#x20;

Azure RBAC exists to securely manage access in large cloud environments where multiple users interact with many resources.

#### Main objectives

* Control **who can access Azure resources**
* Control **what actions users can perform**
* Limit **the scope of those permissions**

This allows organizations to maintain **security, governance, and operational control** across their Azure environment.

### Core Components of RBAC&#x20;

RBAC works through **three primary elements**.

#### Security Principal&#x20;

A **security principal** represents the identity requesting access to a resource.

Types include:

* Users
* Groups
* Service principals (applications)
* Managed identities

These identities are authenticated by Microsoft Entra and then authorized through RBAC.

#### Role Definition&#x20;

A **role definition** is a set of permissions that defines what actions are allowed.

It specifies:

* Allowed operations
* Denied operations
* Management actions on resources

Azure provides **built-in roles**, but organizations can also create **custom roles**.

Examples of common roles:

| Role        | Permission                                       |
| ----------- | ------------------------------------------------ |
| Owner       | Full access including role assignments           |
| Contributor | Full resource management but cannot assign roles |
| Reader      | View resources but cannot modify                 |

Azure includes many built-in roles that administrators can assign to identities.

#### Scope&#x20;

The **scope** determines **where the role applies**.

RBAC permissions can be assigned at different levels.

**Scope hierarchy is**&#x20;

```
Management Group
      ↓
Subscription
      ↓
Resource Group
      ↓
Resource
```

## Role Assignment&#x20;

A **role assignment** connects the three RBAC elements together.

```
Security Principal
        +
Role Definition
        +
Scope
        =
Role Assignment
```

**Example:**&#x20;

```
User: DevOps Engineer
Role: Contributor
Scope: Resource Group "WebAppRG"
```

The user can manage resources only in that resource group.

To grant access, administrators assign roles to identities at a chosen scope.

## Types of Roles&#x20;

### Built-in Roles in Azure&#x20;

Microsoft Entra ID includes many built-in roles, which are roles with a fixed set of permissions. A few of the most common built-in roles are:

* _Global administrator_: users with this role have access to all administrative features in Microsoft Entra. The person who signs up for the Microsoft Entra tenant automatically becomes a global administrator.
* _User administrator_: users with this role can create and manage all aspects of users and groups. This role also includes the ability to manage support tickets and monitor service health.
* _Billing administrator_: users with this role make purchases, manage subscriptions and support tickets, and monitor service health.

All built-in roles are preconfigured bundles of permissions designed for specific tasks. The fixed set of permissions included in the built-in roles can't be modified.

### Service Specific Built-in roles

<figure><img src="../../.gitbook/assets/image (388).png" alt=""><figcaption></figcaption></figure>

There are three broad categories.

* **Microsoft Entra specific roles**: These roles grant permissions to manage resources within Microsoft Entra-only. For example, User Administrator, Application Administrator, Groups Administrator all grant permissions to manage resources that live in Microsoft Entra ID.
* **Service-specific roles**: For major Microsoft 365 services, Microsoft Entra ID includes built-in, service-specific roles that grant permissions to manage features within the service. For example, Microsoft Entra ID includes built-in roles for Exchange Administrator, Intune Administrator, SharePoint Administrator, and Teams Administrator roles that can manage features with their respective services.
* **Cross-service roles**: There are some roles within Microsoft Entra ID that span services. For example, Microsoft Entra ID has security-related roles, like Security Administrator, that grant access across multiple security services within Microsoft 365. Similarly, the Compliance Administrator role grants access to manage Compliance-related settings in Microsoft 365 Compliance Center, Exchange, and so on.

### Custom Roles&#x20;

A custom role definition is a collection of permissions that you choose from a preset list. The list of permissions to choose from are the same permissions used by the built-in roles. The difference is that you get to choose which permissions you want to include in a custom role.

{% hint style="warning" %}
_Custom roles require a Microsoft Entra ID P1 or P2 license._
{% endhint %}
