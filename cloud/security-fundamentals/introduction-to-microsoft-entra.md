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
