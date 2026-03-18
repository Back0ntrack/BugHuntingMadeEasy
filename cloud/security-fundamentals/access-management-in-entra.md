# Access Management in Entra

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

{% hint style="danger" %}
_Custom roles require a Microsoft Entra ID P1 or P2 license._
{% endhint %}
