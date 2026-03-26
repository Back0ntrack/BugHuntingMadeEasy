# Concepts

## Storage Account&#x20;

* A _storage account_ is a container with unique name globally that groups a set of Azure Storage services together.&#x20;

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

* It is a part of a resource group.&#x20;

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Other Azure data services, such as Azure SQL and Azure Cosmos DB, are managed as independent Azure resources and can't be included in a storage account.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Deployment Model&#x20;

Azure provides two deployment models, _Resource Manager_ and _Classic_.

{% hint style="info" %}
_The key feature difference between the two models is their support for grouping. The Resource Manager model adds the concept of a resource group, which isn't available in the classic model. A resource group lets you deploy and manage a collection of resources as a single unit._
{% endhint %}

### Account Kind&#x20;

* **Standard - StorageV2 (general purpose v2)**: the current offering that supports all storage types and all of the latest features.
* **Premium - Page blobs**: Premium storage account type for page blobs only.
* **Premium - Block blobs**: Premium storage account type for block blobs and append blobs.
* **Premium - File shares**: Premium storage account type for file shares only.

### Access Keys&#x20;

Access keys provide access to the entire storage account. You're provided two access keys so you can maintain connections by using one key while regenerating the other.

## Azure Blob Storage

All blobs must be in a container.&#x20;

{% hint style="info" %}
_One Storage account -> Unlimited no. of containers_ \
_One Container -> Unlimited no. of blobs_
{% endhint %}

### Blob Types&#x20;

* **Block blobs**. A block blob consists of blocks of data that are assembled to make a blob. Most Blob Storage scenarios use block blobs. Block blobs are ideal for storing text and binary data in the cloud, like files, images, and videos. The block blob type is the default type for a new blob. When you're creating a new blob, if you don't choose a specific type, the new blob is created as a block blob.
* **Append blobs**. An append blob is similar to a block blob because the append blob also consists of blocks of data. The blocks of data in an append blob are optimized for _append_ operations. Append blobs are useful for logging scenarios, where the amount of data can increase as the logging operation continues.
* **Page blobs**. A page blob can be up to 8 TB in size. Page blobs are more efficient for frequent read/write operations. Azure Virtual Machines uses page blobs for operating system disks and data disks.

{% hint style="danger" %}
_After you create a blob, you can't change its type._
{% endhint %}

### Configure a container&#x20;

* **Public access level**: The access level specifies whether the container and its blobs can be accessed publicly. By default, container data is private and visible only to the account owner. There are three access level choices:
  * **Private**: (Default) Prohibit anonymous access to the container and blobs.
  * **Blob**: Allow anonymous public read access for the blobs only.
  * **Container**: Allow anonymous public read and list access to the entire container, including the blobs.

### Blob Lifecycle management rules&#x20;

* Lifecycle management is a set of rule-based policy for GPv2 and Blob storage accounts.&#x20;
* You can use lifecycle policy rules to transition your data to the appropriate access tiers, and set expiration times for the end of a data set's lifecycle.&#x20;
* Rules can be applied to an entire storage account, specific containers or specific blobs.

```
Hot -> Cool -> Cold -> Archive -> Delete (End of Life)
```

### Soft Delete for Blobs&#x20;

* Blob soft delete protects an individual blob, snapshot, or version from accidental deletes or overwrites by maintaining the deleted data in the system for a specified period of time.
* During the retention period, you can restore a soft-deleted object to its state at the time it was deleted.&#x20;
* After the retention period has expired, the object is permanently deleted.
* Soft delete retention period: 1 to 365 days (can be changed)

### Blob Versioning&#x20;

* Blob versioning is an Azure Storage data protection feature that automatically maintains historical versions of blobs (files) when they are modified, overwritten, or deleted.
* &#x20;It enables users to restore earlier versions to recover from accidental deletions, application errors, or malicious tampering.&#x20;
* Each version is identified by a unique ID and timestamp.

## Azure Files&#x20;

### Azure Files V/s Blob Storage

| Azure Files (file shares)                                                                                                                                                                                                                                                                                                                                                       | Azure Blob Storage (blobs)                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Azure Files provides the SMB and NFS protocols, client libraries, and a REST interface that allows access from anywhere to stored files.                                                                                                                                                                                                                                        | Azure Blob Storage provides client libraries and a REST interface that allows unstructured data to be stored and accessed at a massive scale in block blobs.                                                                                                     |
| <p>- Files in an Azure Files share are true directory objects.<br>- Data in Azure Files is accessed through file shares across multiple virtual machines.</p>                                                                                                                                                                                                                   | <p>- Blobs in Azure Blob Storage are a flat namespace.<br>- Blob data in Azure Blob Storage is accessed through a container.</p>                                                                                                                                 |
| <p><em><strong>Azure Files</strong> is ideal to lift and shift an application to the cloud that already uses the native file system APIs. Share data between the app and other applications running in Azure.</em><br><br><em>Azure Files is a good option when you want to store development and debugging tools that need to be accessed from many virtual machines.</em></p> | <p><em><strong>Azure Blob Storage</strong> is ideal for applications that need to support streaming and random-access scenarios.</em><br><br><em>Azure Blob Storage is a good option when you want to be able to access application data from anywhere.</em></p> |

### Types of Azure File Shares&#x20;

<table><thead><tr><th width="115.5999755859375">Storage tier</th><th>Description</th></tr></thead><tbody><tr><td>Premium</td><td>Premium file shares store data on solid-state drives (SSDs), and are available only in the FileStorage storage account kind. They provide consistent high performance and low latency, and are available in LRS redundancy, with ZRS available in some regions. Not available in all Azure regions.</td></tr><tr><td>Standard</td><td>Standard file shares store data on hard disk drives (HDDs) and deploy in the general-purpose version 2 (GPv2) storage account type. Provide performance for workloads such as general-purpose file shares and dev/test environments. Standard file shares are available for LRS, ZRS, GRS, and GZRS, in all Azure regions.</td></tr></tbody></table>

### File Share Snapshot&#x20;

* The Azure Files share snapshot capability is provided at the file share level.
* If you delete a file share that has share snapshots, all of its snapshots are deleted along with the share.

## Azure Storage Security&#x20;

### Access

&#x20;We've two options to connect to our service.&#x20;

1. Service Endpoints (available in `Public access` option)
2. Private Endpoints (available in `Private Endpoint` option)

{% hint style="info" %}
_Both Service Endpoints and Private Endpoints limit access to Azure resources to specific virtual networks. In Service Endpoints, the resource remains publicly accessible and uses a public IP, but access is restricted at the network level. In contrast, Private Endpoints assign a private IP to the resource within the VNet, eliminating the need for public exposure._
{% endhint %}

### Encryption&#x20;

* When you create a storage account, Azure generates two 512-bit storage account access keys for that account.
* &#x20;These keys can be used to authorize access to data in your storage account via Shared Key authorization, or via SAS tokens that are signed with the shared key.
* Microsoft recommends that you use Azure Key Vault to manage your access keys, and that you regularly rotate and regenerate your keys.

#### Types of Keys&#x20;

* **Infrastructure encryption**
  * Infrastructure encryption can be enabled for the entire storage account, or for an encryption scope within an account.&#x20;
  * When infrastructure encryption is enabled for a storage account or an encryption scope, data is encrypted twice—once at the service level and once at the infrastructure level—with two different encryption algorithms and two different keys.
* **Platform-managed keys**
  * Platform-managed keys (PMKs) are encryption keys generated, stored, and managed entirely by Azure.&#x20;
  * Customers don't interact with PMKs.&#x20;
  * The keys used for Azure Data Encryption-at-Rest, for instance, are PMKs by default.
* **Customer-managed keys**
  * Customer managed keys (CMK), on the other hand, are keys read, created, deleted, updated, and/or administered by one or more customers.&#x20;
  * Keys stored in a customer-owned key vault or hardware security module (HSM) are CMKs. Bring Your Own Key (BYOK) is a CMK scenario in which a customer imports (brings) keys from an outside storage location.&#x20;
