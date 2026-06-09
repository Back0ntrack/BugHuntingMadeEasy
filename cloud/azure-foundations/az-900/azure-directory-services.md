# Azure Directory Services

## Microsoft Entra ID (Formerly Azure Active Directory)

### Overview

Microsoft Entra ID is Microsoft's cloud-based **Identity and Access Management (IAM)** service.\
It manages identities, authentication, authorization, and access policies for users, applications, and devices across cloud and hybrid environments.

It acts as the **central identity provider (IdP)** for:

* Microsoft 365
* Azure resources
* SaaS applications
* Custom enterprise applications
* Hybrid on‑premises environments

***

## 1. Tenant Architecture

### Tenant

A **tenant** is the top-level logical container for an organization inside Microsoft Entra. It is similar to a forest in AD.&#x20;

Example:

```
companyname.onmicrosoft.com
```

A tenant contains:

* Users
* Groups
* Applications
* Devices
* Policies
* Roles
* Subscriptions
* Security configurations

Hierarchy:

```
Tenant
 ├── Users
 ├── Groups
 ├── Applications
 ├── Devices
 ├── Roles
 ├── Policies
 └── Identity Governance
```

***

## 2. Identity Management

Identity is the **core layer** of Entra ID.

### Identity Types

#### Users

Human identities used for authentication.

Attributes include:

* Username
* Email
* Password hash
* MFA methods
* Assigned roles
* Group memberships

#### Guest Users (B2B)

External users invited into a tenant.

Example:

* Vendors
* Partners
* Contractors

#### Service Principals

Identity created for applications or automation.

Used by:

* APIs
* DevOps pipelines
* Automation scripts
* Cloud services

#### Managed Identities

Azure-managed identities for services.

Types:

**System Assigned**

* Bound to a single resource

**User Assigned**

* Reusable identity across resources

***

## 3. Device Management

Entra ID tracks devices accessing the environment.

Types of devices:

#### Azure AD Registered

Personal/BYOD devices.

#### Azure AD Joined

Cloud-managed devices.

#### Hybrid Azure AD Joined

Devices joined to on-prem AD and synced to Entra.

Device properties include:

* Device ID
* Compliance state
* Ownership
* Join type

Device compliance integrates with **Microsoft Intune**.

***

## 4. Access Management

Controls **who can access what resources**.

### Role Based Access Control (RBAC)

Roles assign permissions.

Common roles:

* Global Administrator
* Security Administrator
* User Administrator
* Application Administrator
* Billing Administrator
* Helpdesk Administrator

RBAC structure:

```
User
 ↓
Role Assignment
 ↓
Permission Scope
 ↓
Resource Access
```

***

## 5. Group Management

Groups simplify permission assignments.

### Group Types

#### Security Groups

Used for:

* Resource permissions
* Conditional Access policies

#### Microsoft 365 Groups

Used for collaboration tools:

* Teams
* SharePoint
* Outlook

Membership types:

* Assigned
* Dynamic

Dynamic groups use attribute-based rules.

Example:

```
department == "Engineering"
```

***

## 6. Application Management

Applications integrate with Entra ID for authentication.

### App Registration

Defines an application inside the tenant.

Contains:

* Application ID (Client ID)
* Redirect URIs
* API permissions
* Certificates / secrets

### Service Principal

Created when an application is used in a tenant.

Relationship:

```
App Registration (Global template)
        ↓
Service Principal (Tenant instance)
```

***

## 7. Authentication System

Handles user login verification.

Protocols supported:

* OAuth 2.0
* OpenID Connect
* SAML 2.0

Authentication factors:

#### Password

Traditional authentication.

#### Multi-Factor Authentication (MFA)

Examples:

* Microsoft Authenticator
* SMS
* Phone call
* Hardware tokens

#### Passwordless Authentication

Examples:

* Windows Hello
* FIDO2 security keys
* Authenticator push approval

***

## 8. Conditional Access

Policy engine controlling login conditions.

Example logic:

```
IF location = outside trusted network
AND device not compliant
THEN require MFA
```

Policy conditions:

* User or group
* Application
* Device state
* Location
* Risk level

Actions:

* Block access
* Require MFA
* Require compliant device
* Require password reset

***

## 9. Identity Protection

Risk detection system based on Microsoft threat intelligence.

Detected risks include:

* Anonymous IP logins
* Impossible travel
* Malware-linked IP addresses
* Leaked credentials

Risk levels:

* Low
* Medium
* High

Policies can enforce:

* MFA
* Password reset
* Account block

***

## 10. Privileged Identity Management (PIM)

Controls administrative privileges.

Features:

* Just-in-time admin activation
* Approval workflows
* Role expiration
* Activity auditing

Example:

```
User requests Global Admin role
 ↓
Approval required
 ↓
Temporary privilege granted
 ↓
Role removed after expiration
```

***

## 11. Identity Governance

Ensures access lifecycle management.

Components:

#### Access Reviews

Managers verify if users still need access.

#### Entitlement Management

Bundles access packages for users.

Example package:

```
Engineering Package
 ├ SharePoint access
 ├ GitHub access
 └ Azure resource group
```

#### Lifecycle Workflows

Automates onboarding/offboarding.

***

## 12. Hybrid Identity

Many organizations combine on-prem Active Directory with Entra ID.

Tool used:

**Azure AD Connect**

Functions:

* User synchronization
* Password hash sync
* Pass-through authentication
* Federation

Architecture:

```
On-Prem AD
     ↓
Azure AD Connect
     ↓
Microsoft Entra ID
```

***

## 13. Logging and Monitoring

Security visibility tools.

Logs available:

#### Sign-in Logs

Records authentication attempts.

#### Audit Logs

Administrative changes.

#### Provisioning Logs

Identity lifecycle operations.

Integration with:

* Microsoft Sentinel
* SIEM systems

***

## 14. External Identities

Supports external authentication scenarios.

### B2B Collaboration

Invite external users into tenant.

### B2C Identity

Customer identity platform for applications.

Supports:

* Social logins
* Email/password accounts
* Custom identity providers

***

## 15. Security Model

Entra ID security model follows **Zero Trust**.

Principles:

1. Verify explicitly
2. Use least privilege access
3. Assume breach

Core protections:

* MFA enforcement
* Conditional Access
* Privileged Identity Management
* Risk-based authentication

***

## 16. Management Hierarchy Summary

Complete management flow:

```
Tenant
 ├ Identity Management
 │   ├ Users
 │   ├ Guests
 │   ├ Service Principals
 │   └ Managed Identities
 │
 ├ Access Management
 │   ├ Roles (RBAC)
 │   ├ Groups
 │   └ Permissions
 │
 ├ Application Management
 │   ├ App Registrations
 │   └ Enterprise Applications
 │
 ├ Authentication
 │   ├ Password
 │   ├ MFA
 │   └ Passwordless
 │
 ├ Security
 │   ├ Conditional Access
 │   ├ Identity Protection
 │   └ PIM
 │
 ├ Governance
 │   ├ Access Reviews
 │   ├ Entitlement Management
 │   └ Lifecycle Workflows
 │
 └ Monitoring
     ├ Sign-in Logs
     ├ Audit Logs
     └ SIEM Integration
```

***

## 17. Why Entra ID is Critical in Cloud Security

Compromising Entra ID often means compromising the entire cloud environment.

Common attack surfaces:

* OAuth consent phishing
* Service principal secret leaks
* Token theft
* Conditional Access misconfigurations
* Privileged role abuse

Understanding Entra ID architecture is essential for:

* Cloud security engineering
* Identity architecture
* Red teaming
* Azure penetration testing
