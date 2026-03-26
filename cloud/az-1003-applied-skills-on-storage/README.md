# AZ - 1003 (Applied Skills on Storage)

## Create Storage Account & Blob Storage

### Create Storage Account &#x20;

{% hint style="info" %}
_To create a blob storage we must have a storage container._&#x20;
{% endhint %}

1. Click on create button or Create storage account button.&#x20;

<figure><img src="../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

2. Select the appropriate options from the settings.&#x20;

{% tabs %}
{% tab title="Basics" %}
<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Advanced" %}
<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Networking" %}
<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Data Protection" %}
<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Encryption" %}
<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
_Note that the identities are just used to access the key vault. So whenever encryption and decryption occurs the identity are used automatically to encrypt/decrypt data while performing read/write operation._&#x20;
{% endhint %}

### Create Blob Storage&#x20;

1. Navigate to newly created storage account and from the side bar select container and create new container.&#x20;

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Once the container is created from the storage browser you can upload images to the blob containers. More permission on access can later implemented from container section.&#x20;

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

## Configuring Encryption in Storage account&#x20;

{% hint style="info" %}
_Encryption configured at the storage account level is automatically applied to all data within the account. However, encryption scopes allow you to define different encryption settings for specific blobs or containers within the same storage account._
{% endhint %}

1. Inside storage account settings select `Security + Networking` -> `Encryption`.&#x20;

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

2. To enable encryption for an entire storage account, configure the encryption type in the storage account settings. To apply different encryption settings to specific blobs or containers, create and use an encryption scope.

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_With encryption scopes, a single storage account can use multiple encryption keys for different data within the account._
{% endhint %}

## Lifecycle management&#x20;

1. Inside storage account settings select `Data management` -> `Lifecycle Management`.&#x20;

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Lifecycle management rules can be applied to all blobs as well as selected blobs as well.&#x20;

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. We can add conditions accordingly for moving the blobs from `Hot` -> `Cool`-> `Cold` -> `Archive` -> `Delete`.&#x20;

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Immutable Storage

It allows users to store critical data in (Write Once, Read Many) state.&#x20;

* Time-based retention policy: With a time-based retention policy, users can set policies to store data for a specific interval where objects can be created and read, but not modified or deleted. After retention period has expired, objects can be deleted but not overwritten.&#x20;
* Legal hold policies: A legal hold stores immutable data until the legal hold is explicitly cleared. Same as time-based but data is hold until someone removes the lock.&#x20;

1. Select `access policy` from settings of the container.&#x20;

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

2. Click on add policy under `Immutable blob Storage` and select necessary options.&#x20;

<figure><img src="../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Network Security&#x20;

To configure storage account to be accessed from specific Vnet and subnet.&#x20;

1. Select `Security + Networking` -> `Networking` from settings of storage account.&#x20;

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_When you go to "Public network access" and choose "Enabled from selected virtual networks and IP addresses," you are typically using Service Endpoints which will still have public IP address but only specified subnet traffic will be accepted._&#x20;

_When you create a Private Endpoint, you are effectively putting your Storage account inside your VNet._
{% endhint %}

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

choose options accordingly.&#x20;

## Manage RBAC in Azure&#x20;

1. Select `Access Control(IAM)` in the required resource you want and click on `Add`. and then `Add Role Assignment`.&#x20;

<figure><img src="../../.gitbook/assets/image (398).png" alt=""><figcaption></figcaption></figure>

2. In the add role assignment, select the role you want to give to a specific member or identity and let's go.&#x20;

<figure><img src="../../.gitbook/assets/image (399).png" alt=""><figcaption></figcaption></figure>
