# Features and Tools in Azure for Governance and Compliance

### Module Overview

Governance and compliance in cloud environments ensure that resources are **secure, properly configured, and aligned with organizational and regulatory requirements**.

Azure provides several tools that help organizations:

* Enforce policies
* Protect resources from accidental changes
* Manage and classify data
* Maintain compliance with regulatory standards

Key governance tools include:

* **Microsoft Purview**
* **Azure Policy**
* **Resource Locks**
* **Service Trust Portal**

These tools help organizations maintain **security, compliance, and operational control** over their cloud environment.

***

## 1. Introduction to Governance and Compliance in Azure

When organizations operate in the cloud, they must ensure that:

* Resources follow company policies
* Data is protected and classified properly
* Regulations and compliance standards are met
* Resources cannot be accidentally deleted or modified

Azure governance tools provide mechanisms to **monitor, enforce, and audit these requirements**.

***

### Governance

Governance refers to the **process of managing cloud resources according to organizational standards and policies**.

Examples include:

* Ensuring only approved regions are used
* Enforcing naming conventions
* Controlling resource deployment
* Tracking resource ownership

Governance ensures **consistent resource management across the organization**.

***

### Compliance

Compliance ensures that systems follow **legal, regulatory, and industry standards**.

Examples include:

* GDPR
* ISO 27001
* HIPAA
* SOC compliance

Azure provides tools and documentation to help organizations **prove compliance with these standards**.

***

## 2. Microsoft Purview

### What is Microsoft Purview?

**Microsoft Purview** is a **data governance and compliance solution** that helps organizations manage and protect their data across multiple environments.

It provides tools for:

* Data discovery
* Data classification
* Data governance
* Compliance management

Purview works across:

* On-premises environments
* Azure
* Microsoft 365
* Other cloud providers

***

### Key Capabilities

#### Data Discovery

Purview scans data sources and automatically identifies:

* Databases
* Storage accounts
* Data lakes
* Files

This helps organizations **understand where their data exists**.

***

#### Data Classification

Purview identifies **sensitive data** using built-in classifiers.

Examples:

* Credit card numbers
* Personal identification numbers
* Financial records
* Health information

This helps organizations protect **sensitive or regulated information**.

***

#### Data Catalog

Purview creates a **centralized catalog of data assets**.

Benefits include:

* Improved data visibility
* Easier data management
* Better collaboration between teams

***

#### Data Governance

Organizations can define rules for:

* Data access
* Data usage
* Data protection

This ensures data is **used responsibly and securely**.

***

## 3. Azure Policy

### What is Azure Policy?

**Azure Policy** is a service that helps enforce **organizational standards and compliance rules** across Azure resources.

It evaluates resources to determine whether they comply with defined policies.

***

### Purpose of Azure Policy

Azure Policy allows organizations to:

* Control how resources are created
* Enforce configuration standards
* Prevent non-compliant deployments
* Maintain consistent environments

***

### Example Policy Scenarios

Examples of policies include:

| Policy Example                           | Purpose                       |
| ---------------------------------------- | ----------------------------- |
| Allow resources only in specific regions | Control geographic deployment |
| Require resource tags                    | Improve cost tracking         |
| Restrict VM sizes                        | Control cost and performance  |
| Require encryption                       | Improve security              |

***

### Policy Effects

Policies can take different actions when rules are violated.

Common effects include:

| Effect               | Description                                 |
| -------------------- | ------------------------------------------- |
| Deny                 | Prevent resource creation                   |
| Audit                | Log non-compliance without blocking         |
| Modify               | Automatically adjust resource configuration |
| Deploy if not exists | Deploy required resources automatically     |

***

### Policy Scope

Policies can be applied at different levels:

* Management group
* Subscription
* Resource group
* Individual resources

This provides **centralized governance across environments**.

***

## 4. Resource Locks

### What Are Resource Locks?

Resource locks prevent **accidental deletion or modification of Azure resources**.

They add an extra layer of protection beyond standard permissions.

***

### Types of Resource Locks

Azure provides two types of locks.

#### Delete Lock (CanNotDelete)

Prevents resources from being deleted.

Users can still:

* Read
* Modify

But they **cannot delete the resource**.

***

#### Read-Only Lock (ReadOnly)

Prevents any modification to the resource.

Users can only:

* View resource configuration

They **cannot change or delete it**.

***

### Example Use Case

Consider a production database.

To prevent accidental deletion:

* Apply a **Delete lock**

This ensures critical resources remain protected.

***

### Lock Scope

Locks can be applied to:

* Subscription
* Resource group
* Individual resource

Locks are inherited by resources under that scope.

Example:

If a lock is applied to a **resource group**, all resources within that group are protected.

***

## 5. Service Trust Portal

### What is the Service Trust Portal?

The **Service Trust Portal** is a Microsoft website that provides information about:

* Security
* Privacy
* Compliance
* Regulatory certifications

It helps organizations understand **how Microsoft services meet compliance requirements**.

***

### Purpose

The portal provides:

* Compliance documentation
* Audit reports
* Certifications
* Regulatory information

Organizations use this information to **verify Microsoft cloud services meet industry standards**.

***

### Information Available

Examples of information in the Service Trust Portal include:

| Information Type         | Description                   |
| ------------------------ | ----------------------------- |
| Compliance reports       | Detailed audit reports        |
| Certifications           | ISO, SOC, FedRAMP             |
| Security practices       | Microsoft's security controls |
| Regulatory documentation | Compliance guidance           |

***

### Example Certifications

Microsoft services comply with many global standards, including:

* ISO 27001
* SOC 1 / SOC 2
* GDPR
* HIPAA

The Service Trust Portal allows organizations to **download reports and documentation** related to these certifications.
