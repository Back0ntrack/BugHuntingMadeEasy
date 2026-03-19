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

# Azure Storage Services

## Storage Account

A storage account provides a unique namespace for your Azure Storage data that's accessible from anywhere in the world over HTTP or HTTPS. Data in this account is secure, highly available, durable, and massively scalable.

**Rules:**&#x20;

* Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.
* Your storage account name must be unique within Azure. No two storage accounts can have the same name. This supports the ability to have a unique, accessible namespace in Azure.

### Determines

The type of account determines the&#x20;

* Storage Service&#x20;
* Redundancy Options
* Performance Tier&#x20;
* Access Tier

Although we can configure different options such as storage services, redundancy, performance tier, and access tier, Azure provides storage account types that act as capability templates and determine which configuration options are available for a storage account.

### Azure Storage Account Types&#x20;

| Type                        | Supported Services                                                                        | Redundancy Options                   | Usage                                                                                                                                                                                                                                        |
| --------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Standard general-purpose v2 | Blob Storage (including Data Lake Storage), Queue Storage, Table Storage, and Azure Files | LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS | Standard storage account type for blobs, file shares, queues, and tables. Recommended for most scenarios using Azure Storage. If you want support for network file system (NFS) in Azure Files, use the premium file shares account type.    |
| Premium block blobs         | Blob Storage (including Data Lake Storage)                                                | LRS, ZRS                             | Premium storage account type for block blobs and append blobs. Recommended for scenarios with high transaction rates or that use smaller objects or require consistently low storage latency.                                                |
| Premium file shares         | Azure Files                                                                               | LRS, ZRS                             | Premium storage account type for file shares only. Recommended for enterprise or high-performance scale applications. Use this account type if you want a storage account that supports both Server Message Block (SMB) and NFS file shares. |
| Premium page blobs          | Page blobs only                                                                           | LRS                                  | Premium storage account type for page blobs only.                                                                                                                                                                                            |

## Azure Storage Redundancy&#x20;

Azure Storage always stores multiple copies of your data so that it's protected from planned and unplanned events such as transient hardware failures, network or power outages, and natural disasters.

### Redundancy in the primary region

{% hint style="info" %}
_Data in an Azure Storage account is always replicated three times in the primary region._&#x20;
{% endhint %}

Azure Storage offers two options for how your data is replicated in the primary region, locally redundant storage (LRS) and zone-redundant storage (ZRS).

#### Locally Redundant Storage (LRS)

* Data replicated 3 times
* Within a single datacenter in primary region.&#x20;
* Lowest-cost redundancy option
* Protects data against server rack and drive failures&#x20;
* But it doesn't protect against disaster such as fire or flooding occurs
* Provides durability of 11 nines (99.999999999%) of objects over a given year

<figure><img src="../../.gitbook/assets/output.png" alt=""><figcaption></figcaption></figure>

#### Zone-redundant Storage

* Synchronously replicates across 3 availability zones in the primary region
* Provides durability of 12 nines (99.9999999999%) of objects over a given year
* With ZRS, your data is still accessible for both read and write operations even if a zone becomes unavailable.
* ZRS is also recommended for restricting replication of data within a country or region to meet data governance requirements.

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Redundancy in a secondary region&#x20;

* For applications requiring high durability, you can choose to additionally copy the data in your storage account to a secondary region that is hundreds of miles away from the primary region.&#x20;
* When you create a storage account, you select the primary region for the account. The paired secondary region is based on Azure Region Pairs, and can't be changed.
* By default, data in the secondary region isn't available for read or write access unless there's a failover to the secondary region.
* Azure Storage offers two options for copying your data to a secondary region: geo-redundant storage (GRS) and geo-zone-redundant storage (GZRS).&#x20;
  * GRS is similar to running LRS in two regions, and GZRS is similar to running ZRS in the primary region and LRS in the secondary region.
* If we enable read access to the secondary region, the data is always available, even when the primary region is running optimally. For read access enable (RA-GRS) or (RA-GZRS).
* If the primary region becomes unavailable, you can choose to fail over to the secondary region. After the failover has completed, the secondary region becomes the primary region, and you can again read and write data.

{% hint style="danger" %}
_Because data is replicated to the secondary region asynchronously, a failure that affects the primary region may result in data loss if the primary region can't be recovered. The interval between the most recent writes to the primary region and the last write to the secondary region is known as the recovery point objective (RPO). The RPO indicates the point in time to which data can be recovered. Azure Storage typically has an RPO of less than 15 minutes, although there's currently no SLA on how long it takes to replicate data to the secondary region._
{% endhint %}

#### Geo-Redundant Storage&#x20;

* Data is replicated **3 times synchronously** in the **primary region** using **LRS**
* Replication inside primary region occurs within a **single physical location (datacenter)**
* Data is then copied **asynchronously** to the **secondary region**
* Secondary region is the **Azure Region Pair**
* Secondary region stores **3 additional copies using LRS**
* Total copies = **6 copies (3 primary + 3 secondary)**
* Offers durability of at least **99.99999999999999% (16 nines) per year**

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Geo-zone Redundant Storage&#x20;

* Combines **zone-level redundancy + geo-replication**
* Data is copied across **3 Availability Zones** in the **primary region** (like ZRS)
* Data is also replicated to a **secondary geographic region using LRS**
* Total copies:
  * 3 copies across zones (primary)
  * 3 copies in secondary region
  * **Total = 6 copies**
* Provides maximum availability, excellent performance and resilience for disaster recovery.&#x20;
* Offers durability of at least **99.99999999999999% (16 nines) per year**

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

## Azure Storage Services&#x20;

The Azure Storage platform includes the following data services:

* **Azure Blobs**: A massively scalable object store for text and binary data. Also includes support for big data analytics through Data Lake Storage Gen2.
* **Azure Files**: Managed file shares for cloud or on-premises deployments.
* **Azure Queues**: A messaging store for reliable messaging between application components.
* **Azure Disks**: Block-level storage volumes for Azure VMs.
* **Azure Tables:** NoSQL table option for structured, non-relational data.

### Azure Blobs (Binary Large Object)

* Cloud-based **object storage solution**
* In **Azure Blobs** everything is stored inside a blob container.&#x20;
* Designed to store **massive amounts of data**
* It is unstructured and there is no restriction on the type of data it holds.&#x20;
* It can store anything irrespective of the data type and size.&#x20;
* Supports thousands of simultaneous uploads.
* It can store binary data streamed from a scientific instrument or custom format for an app.&#x20;
* Data is uploaded as blobs, and Azure takes care of the physical storage needs.&#x20;

{% hint style="info" %}
_One Storage account -> Unlimited no. of containers_ \
_One Container -> Unlimited no. of blobs_
{% endhint %}

### Azure Files&#x20;

* Fully managed **file shares in the cloud**
* Accessible using industry-standard protocols:
  * **SMB (Server Message Block)** \[Both Windows and Linux]
  * **NFS (Network File System)** \[Linux Only]
* Azure File Sync
  * SMB shares can be cached on **Windows Servers**
  * Uses **Azure File Sync**
  * Provides:
    * Faster local access
    * Hybrid cloud integration
    * Frequently accessed data stored locally

### Azure Queues

* Store large number of messages.&#x20;
* Can store up to millions of message with each message can be upto 64kb in size.&#x20;
* Commonly used to create a backlog of work.
* Can be combined with Azure functions to take an action when a message is received.&#x20;

For example, you want to perform an action after a customer uploads a form to your website. You could have the submit button on the website trigger a message to the Queue storage. Then, you could use Azure Functions to trigger an action once the message was received.

### Azure Disks&#x20;

Azure Disk storage, or Azure managed disks, are block-level storage volumes managed by Azure for use with Azure VMs. Conceptually, they’re the same as a physical disk, but they’re virtualized – offering greater resiliency and availability than a physical disk. With managed disks, all you have to do is provision the disk, and Azure will take care of the rest.

### Azure tables&#x20;

Azure Table storage stores large amounts of structured data. Azure tables are a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud. This enables you to use Azure tables to build your hybrid or multicloud solution and have your data always available. Azure tables are ideal for storing structured, non-relational data.

{% hint style="info" %}
_Archive storage stores data offline and offers the lowest storage costs, but also the highest costs to rehydrate and access data._
{% endhint %}

## Azure Access Tiers&#x20;

Access tiers are applicable only to Blob Storage in Microsoft Azure and are used to optimize storage costs based on how frequently the data is accessed.

* **Hot**
  * For **frequently accessed data**
  * **Highest storage cost**
  * **Lowest access cost**
  * Example: website images, active application files
* **Cool**
  * For **infrequently accessed data**
  * **Lower storage cost than Hot**
  * **Higher access cost than Hot**
  * Requires **minimum storage duration (\~30 days)**
  * Example: short-term backups, monthly reports
* **Cold**
  * For **rarely accessed data**
  * **Lower storage cost than Cool**
  * **Higher access cost**
  * Requires **longer minimum storage duration (\~90 days)**
  * Example: older backups, compliance logs
* **Archive**
  * For **very rarely accessed long-term data**
  * **Lowest storage cost**
  * **Highest retrieval cost**
  * Data must be **rehydrated before access**
  * Requires **minimum storage duration (\~180 days)**
  * Example: legal records, long-term archival backups

## Azure Performance Tier&#x20;

It is defined at storage account level. The performance tiers are:&#x20;

* **Standard**
  * Uses **HDD (Hard Disk Drives)**
  * **Lower cost**
  * Suitable for **general-purpose workloads**
  * Used for most storage accounts like **GPv2**
  * Example: backups, logs, documents, images
* **Premium**
  * Uses **SSD (Solid State Drives)**
  * **Higher cost**
  * Provides **low latency and high IOPS (I/O Operations per second)**
  * Designed for **high-performance workloads**
  * Example: databases, high-transaction applications, VM disks

## Azure Data Migration Options&#x20;

Azure supports both real-time migration of infrastructure, applications, and data using Azure Migrate as well as asynchronous migration of data using Azure Data Box.

### Service Based Migration Tools&#x20;

#### Infrastructure / Workload Migration

* **Azure Migrate**
  * Discovers and assesses on-prem resources
  * Migrates **servers, applications, and workloads**

#### Database Migration

* **Azure Database Migration Service**
  * Migrates databases like SQL Server, MySQL, PostgreSQL
  * Supports **near-zero downtime migration**

#### Data Integration / ETL Migration

* **Azure Data Factory**
  * Orchestrates data movement pipelines
  * Used for **continuous data replication or transformation**

### Azure Data Box&#x20;

* A **physical data migration service**
* Used to transfer **large amounts of data**
* Fast, cost-effective, and reliable
* Ideal for:
  * Initial cloud migration
  * Large dataset transfer
  * Limited bandwidth scenarios
* Microsoft ships a **proprietary Data Box device**
* Maximum usable capacity: **80 TB**
* Rugged and tamper-resistant case
* Secure data transfer
* Transported via **regional carrier**

#### Workflow

{% stepper %}
{% step %}
### Order Device

* Request Data Box through **Azure Portal**
* Choose:
  * Import to Azure
  * Export from Azure
{% endstep %}

{% step %}
### Data Transfer

* Receive device at your datacenter
* Configure via **local web UI**
* Connect to your network
* Copy data into or out of the device
{% endstep %}

{% step %}
### Return Device

* Ship device back to Microsoft
* If importing:
  * Data automatically uploaded to Azure
* Entire process tracked in Azure Portal
{% endstep %}
{% endstepper %}

## Azure File Movement Options&#x20;

### AzCopy

* A **command-line utility**
* Used to copy data to and from **Azure Storage**
* Used to upload files, download files, copy files and even synchronize files
* Supports:
  * Blob Storage
  * Azure Files
* Can transfer data:
  * Between Azure storage accounts
  * Between on-premises and Azure
  * Between Azure and other cloud providers

{% hint style="info" %}
_Synchronizing blobs or files with AzCopy is one-direction synchronization. When you synchronize, you designate the source and destination, and AzCopy will copy files or blobs in that direction. It doesn't synchronize bi-directionally based on timestamps or other metadata._
{% endhint %}

### Azure Storage Explorer&#x20;

Azure Storage Explorer is a standalone app that provides a graphical interface to manage files and blobs in your Azure Storage Account. It works on Windows, macOS, and Linux operating systems and uses AzCopy on the backend to perform all of the file and blob management tasks. With Storage Explorer, you can upload to Azure, download from Azure, or move between storage accounts.

### Azure File Sync&#x20;

Azure File Sync is a tool that lets you centralize your file shares in Azure Files and keep the flexibility, performance, and compatibility of a Windows file server. It’s almost like turning your Windows file server into a miniature content delivery network. Once you install Azure File Sync on your local Windows server, it will automatically stay bi-directionally synced with your files in Azure.

With Azure File Sync, you can:

* Use any protocol that's available on Windows Server to access your data locally, including SMB, NFS, and FTPS.
* Have as many caches as you need across the world.
* Replace a failed local server by installing Azure File Sync on a new server in the same datacenter.
* Configure cloud tiering so the most frequently accessed files are replicated locally, while infrequently accessed files are kept in the cloud until requested.
