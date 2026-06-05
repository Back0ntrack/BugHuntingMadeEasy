# Identities and Governance

Microsoft Entra ID is part of the platform as a service (PaaS) offering and operates as a Microsoft-managed directory service in the cloud.

Microsoft Entra ID is the central Identity and Access Management (IAM) system for Azure and Microsoft 365.

Everything revolves around identities:

```
User
Group
Device
Application
Service Principal
Managed Identity
```

Before a user can access a resource:

```
Identity
    ↓
Authentication
    ↓
Authorization
    ↓
Resource Access
```

***

## 1. Users

### What is a User?

A user is a digital identity representing a person inside Microsoft Entra ID.

Examples:

```
john@company.com
alice@company.com
```

Each user has:

* User Principal Name (UPN)
* Object ID
* Display Name
* Department
* Job Title
* Manager
* Group Memberships
* Licenses
* Assigned Roles

### User Types

#### Member User

Internal employee.

Example:

```
john@company.com
```

Can receive:

* Licenses
* Roles
* Group memberships

#### Guest User

External user invited into tenant.

Example:

```
partner@external.com
```

Used for:

* Contractors
* Vendors
* Partners

B2B collaboration is built around guest users.

### User Lifecycle

```
Create User
      ↓
Assign Password
      ↓
Assign License
      ↓
Assign Group
      ↓
Assign Role
      ↓
Disable/Delete User
```

### Deleted Users

Deleted users enter a soft-deleted state.

Default retention:

```
30 Days
```

During this period:

* Restore user
* Recover group memberships
* Recover licenses

After retention expires:

```
Permanent Deletion
```

## 2. Groups

### What is a Group?

A group is a container for users, devices, and other identities.

Instead of assigning permissions individually:

```
Assign Permission
        ↓
To Group
        ↓
Members inherit permission
```

### Group Types

#### Security Group

Used for:

* RBAC
* Conditional Access
* Application access

Most important group type for security.

#### Microsoft 365 Group

Used for collaboration services:

* Teams
* SharePoint
* Outlook
* Planner

### Membership Types

#### Assigned Membership

Manual membership.

Example:

```
Admin manually adds user
```

***

#### Dynamic Membership

Automatic membership using rules.

Example:

```
Department = IT
```

Any user matching the rule joins automatically.

Requires appropriate licensing. ([Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/create-configure-manage-identities/?utm_source=chatgpt.com))

### Group-Based Access

Example:

```
IT Admin Group
        ↓
Contributor Role
```

All members receive Contributor permissions.

## 3. Device Registration

Microsoft Entra can manage trusted devices.

Three device states exist.

***

### Entra Registered

BYOD model.

```
Personal Device
       +
Company Account
```

Characteristics:

* User-owned
* Mobile devices
* Personal laptops
* Intune optional

Purpose:

```
Access Company Resources
```

### Entra Joined

Cloud-native device.

```
Company Device
        ↓
Entra ID
```

Characteristics:

* Organization-owned
* Cloud-first
* No on-prem AD required
* Intune managed

Purpose:

```
Cloud Device Management
```

### Hybrid Entra Joined

Combination of:

```
Active Directory
      +
Entra ID
```

Characteristics:

* Existing AD environments
* Supports legacy applications
* Supports Group Policy

Purpose:

```
Modernize Existing AD Environment.
```

## 4. Licensing

### What is a License?

A license unlocks Microsoft services and security features.

Without a license:

```
User Exists
      ≠
Service Access
```

### Common Licenses

#### Microsoft 365 E3

Provides:

* Office Apps
* Exchange
* SharePoint
* Teams

#### Microsoft 365 E5

Adds advanced security:

* Defender
* Advanced Auditing
* PIM
* Identity Protection

#### Entra ID P1

Adds:

* Conditional Access
* Dynamic Groups

#### Entra ID P2

Adds:

* PIM
* Identity Protection
* Access Reviews

### License Assignment Methods

#### Direct Assignment

```
License
    ↓
User
```

***

#### Group-Based Licensing

```
License
      ↓
Group
      ↓
All Members
```

Recommended for large environments

## 5. Custom Security Attributes

### What are Custom Security Attributes?

Custom key-value pairs attached to Entra objects.

Think:

```
Custom Tags
```

Examples:

```
Clearance = Secret

Project = CARTP

EmployeeType = Contractor
```

Custom security attributes can be assigned to users and applications and are used for access decisions and governance.

### Structure

```
Attribute Set
      ↓
Attribute
      ↓
Value
```

Example:

```
Security

 ├─ Clearance
 │      └─ Secret

 └─ Project
        └─ RedTeam
```

***

### Uses

#### Access Control

```
Clearance = Secret
```

Allow access to sensitive resources.

***

#### Resource Segmentation

```
Project = Finance
```

Allow access to Finance systems.

***

#### Automation

Used in:

* Policies
* Workflows
* Governance

## 6. Automatic User Provisioning

### What is Provisioning?

Automatic creation and management of identities.

Instead of manually creating users:

```
HR System
      ↓
Entra ID
      ↓
Create User
      ↓
Assign Groups
      ↓
Assign License
```

Provisioning synchronizes identity data between systems

### Provisioning Flow

```
Source System
        ↓
Provisioning Service
        ↓
Entra ID
        ↓
Applications
```

### Deprovisioning

When employee leaves:

```
Disable User
      ↓
Remove Groups
      ↓
Remove License
      ↓
Remove Access
```
