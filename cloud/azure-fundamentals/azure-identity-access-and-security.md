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

# Azure Identity Access and Security

## Azure Directory Services&#x20;

Microsoft Entra ID is a directory service that enables you to sign in and access both Microsoft cloud applications and cloud applications that you develop. Microsoft Entra ID can also help you maintain your on-premises Active Directory deployment.

Microsoft Entra ID provides services such as:&#x20;

* **Authentication**: This includes verifying identity to access applications and resources. It also includes providing functionality such as self-service password reset, multifactor authentication, a custom list of banned passwords, and smart lockout services.
* **Single sign-on**: Single sign-on (SSO) enables you to remember only one username and one password to access multiple applications. A single identity is tied to a user, which simplifies the security model. As users change roles or leave an organization, access modifications are tied to that identity, which greatly reduces the effort needed to change or disable accounts.
* **Application management**: You can manage your cloud and on-premises apps by using Microsoft Entra ID. Features like Application Proxy, SaaS apps, the My Apps portal, and single sign-on provide a better user experience.
* **Device management**: Along with accounts for individual people, Microsoft Entra ID supports the registration of devices. Registration enables devices to be managed through tools like Microsoft Intune.

### Microsoft Entra Domain Services&#x20;

* A **managed domain service** in Azure
* Provides traditional Active Directory features such as:
  * Domain Join
  * Group Policy
  * LDAP (Lightweight Directory Access Protocol)
  * Kerberos authentication
  * NTLM authentication
* No need to:
  * Deploy Domain Controllers (DCs) (as two DCs are automatically deployed in selected Azure region known as replica set)
  * Manage or patch DCs
  * Maintain AD infrastructure in the cloud
* Integrates with existing Microsoft Entra tenant
* Users can:
  * Sign in using existing credentials
  * Use existing user accounts and groups
* Enables seamless migration to Azure

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## Azure Authentication Methods&#x20;

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### Single Sign-On

Single sign-on (SSO) enables a user to sign in one time and use that credential to access multiple resources and applications from different providers.

With SSO, you need to remember only one ID and one password. Access across applications is granted to a single identity that's tied to the user, which simplifies the security model.

### Multifactor authentication

Multifactor authentication is the process of prompting a user for an extra form (or factor) of identification during the sign-in process.&#x20;

Multifactor authentication provides additional security for your identities by requiring two or more elements to fully authenticate. These elements fall into three categories:

* Something the user knows – this might be a challenge question.
* Something the user has – this might be a code that's sent to the user's mobile phone.
* Something the user is – this is typically some sort of biometric property, such as a fingerprint or face scan.

### Passwordless authentication

Passwordless authentication needs to be set up on a device before it can work. For example, your computer is something you have. Once it’s been registered or enrolled, Azure now knows that it’s associated with you. Now that the computer is known, once you provide something you know or are (such as a PIN or fingerprint), you can be authenticated without using a password.

Microsoft global Azure and Azure Government offer the following three Passwordless authentication options that integrate with Microsoft Entra ID:

* Windows Hello for Business
* Microsoft Authenticator app
* FIDO2 security keys

## Azure External Identities&#x20;

Microsoft Entra External ID refers to all the ways you can securely interact with users outside of your organization.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

The following capabilities make up External Identities:

* **Business to business (B2B) collaboration** - Collaborate with external users by letting them use their preferred identity to sign-in to your Microsoft applications or other enterprise applications (SaaS apps, custom-developed apps, etc.). B2B collaboration users are represented in your directory, typically as guest users.
* **B2B direct connect** - Establish a mutual, two-way trust with another Microsoft Entra organization for seamless collaboration. B2B direct connect currently supports Teams shared channels, enabling external users to access your resources from within their home instances of Teams. B2B direct connect users aren't represented in your directory, but they're visible from within the Teams shared channel and can be monitored in Teams admin center reports.
* **Microsoft Azure Active Directory business to customer (B2C)** - Publish modern SaaS apps or custom-developed apps (excluding Microsoft apps) to consumers and customers, while using Azure AD B2C for identity and access management.

## Azure Conditional Access&#x20;

Conditional Access is a tool that Microsoft Entra ID uses to allow (or deny) access to resources based on identity signals. These signals include who the user is, where the user is, and what device the user is requesting access from.

Conditional Access also provides a more granular multifactor authentication experience for users. For example, a user might not be challenged for second authentication factor if they're at a known location. However, they might be challenged for a second authentication factor if their sign-in signals are unusual or they're at an unexpected location.

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Conditional Access is useful when you need to:

* Require multifactor authentication (MFA) to access an application depending on the requester’s role, location, or network. For example, you could require MFA for administrators but not regular users or for people connecting from outside your corporate network.
* Require access to services only through approved client applications. For example, you could limit which email applications are able to connect to your email service.
* Require users to access your application only from managed devices. A managed device is a device that meets your standards for security and compliance.
* Block access from untrusted sources, such as access from unknown or unexpected locations.

## Azure RBAC&#x20;

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Scopes include:

* A management group (a collection of multiple subscriptions).
* A single subscription.
* A resource group.
* A single resource.

Azure RBAC is hierarchical, in that when you grant access at a parent scope, those permissions are inherited by all child scopes. For example:

* When you assign the Owner role to a user at the management group scope, that user can manage everything in all subscriptions within the management group.
* When you assign the Reader role to a group at the subscription scope, the members of that group can view every resource group and resource within the subscription.

Azure RBAC is enforced on any action that's initiated against an Azure resource that passes through Azure Resource Manager. Resource Manager is a management service that provides a way to organize and secure your cloud resources.

## Zero Trust Model&#x20;

Zero Trust is a security model that assumes the worst case scenario and protects resources with that expectation. Zero Trust assumes breach at the outset, and then verifies each request as though it originated from an uncontrolled network.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure></div>

To address this new world of computing, Microsoft highly recommends the Zero Trust security model, which is based on these guiding principles:

* **Verify explicitly** - Always authenticate and authorize based on all available data points.
* **Use least privilege access** - Limit user access with Just-In-Time and Just-Enough-Access (JIT/JEA), risk-based adaptive policies, and data protection.
* **Assume breach** - Minimize blast radius and segment access. Verify end-to-end encryption. Use analytics to get visibility, drive threat detection, and improve defenses.

## Defense In Depth&#x20;

The objective of defense-in-depth is to protect information and prevent it from being stolen by those who aren't authorized to access it.

### Layers&#x20;

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Each layer provides protection so that if one layer is breached, a subsequent layer is already in place to prevent further exposure. This approach removes reliance on any single layer of protection. It slows down an attack and provides alert information that security teams can act upon, either automatically or manually.

### Physical Security&#x20;

Physically securing access to buildings and controlling access to computing hardware within the datacenter are the first line of defense.

With physical security, the intent is to provide physical safeguards against access to assets. These safeguards ensure that other layers can't be bypassed, and loss or theft is handled appropriately.

### Identity and Access&#x20;

The identity and access layer is all about ensuring that identities are secure, that access is granted only to what's needed, and that sign-in events and changes are logged.

### Perimeter&#x20;

The network perimeter protects from network-based attacks against your resources. Identifying these attacks, eliminating their impact, and alerting you when they happen are important ways to keep your network secure.

At this layer, it's important to:

* Use DDoS protection to filter large-scale attacks before they can affect the availability of a system for users.
* Use perimeter firewalls to identify and alert on malicious attacks against your network.

### Network&#x20;

At this layer, the focus is on limiting the network connectivity across all your resources to allow only what's required. By limiting this communication, you reduce the risk of an attack spreading to other systems in your network.

At this layer, it's important to:

* Limit communication between resources.
* Deny by default.
* Restrict inbound internet access and limit outbound access where appropriate.
* Implement secure connectivity to on-premises networks.

### Compute&#x20;

Malware, unpatched systems, and improperly secured systems open your environment to attacks. The focus in this layer is on making sure that your compute resources are secure and that you have the proper controls in place to minimize security issues.

At this layer, it's important to:

* Secure access to virtual machines.
* Implement endpoint protection on devices and keep systems patched and current.

### Application&#x20;

Integrating security into the application development lifecycle helps reduce the number of vulnerabilities introduced in code. Every development team should ensure that its applications are secure by default.

At this layer, it's important to:

* Ensure that applications are secure and free of vulnerabilities.
* Store sensitive application secrets in a secure storage medium.
* Make security a design requirement for all application development.

### Data&#x20;

Those who store and control access to data are responsible for ensuring that it's properly secured. Often, regulatory requirements dictate the controls and processes that must be in place to ensure the confidentiality, integrity, and availability of the data.

In almost all cases, attackers are after data:

* Stored in a database.
* Stored on disk inside virtual machines.
* Stored in software as a service (SaaS) applications, such as Office 365.
* Managed through cloud storage.
