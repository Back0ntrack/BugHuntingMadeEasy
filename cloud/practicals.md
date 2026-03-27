# Practicals

{% hint style="info" %}
_In Azure, we can use either the search bar or the sidebar to find and create the required resources._
{% endhint %}

## Azure Fundamentals&#x20;

### Create VM&#x20;

<details>

<summary><strong>Using Azure Portal</strong></summary>

1. Click on the create option and select virtual machine.&#x20;

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Select things as per requirement.&#x20;

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

There are not much other things to be taken care of while creating a virtual machine. Thus click on `Review + create` and you're done.&#x20;

</details>

<details>

<summary><strong>Using Azure PowerShell</strong></summary>

1. Click on the Azure Cloud Shell and select powershell from the options.&#x20;

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Select the mount storage account section and select subscription on which you want to perform the operation.&#x20;

{% hint style="warning" %}
_Note that if you select mount storage account then it will create a storage account and create a file share and all your sessions of the powershell will be stored in it._&#x20;
{% endhint %}

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Selecting the mount storage option will next provide you with three option_&#x20;

1. _Either to select already built storage account._&#x20;
2. _Azure will create a storage account for you._&#x20;
3. _You'll create a storage account on your own._&#x20;
{% endhint %}

3. After that PowerShell will start.&#x20;

<figure><img src="../.gitbook/assets/image (6) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

4. Verify resource group and create a virtual machine.&#x20;

```powershell
Get-AzResourceGroup | Format-Table

New-AzVm `
-ResourceGroupName "myRGPS" `
-Name "myVMPS" `
-Location "East US" `
-VirtualNetworkName "myVnetPS" `
-SubnetName "mySubnetPS" `
-SecurityGroupName "myNSGPS" `
-PublicIpAddressName "myPublicIpPS"

Get-AzVM -name myVMPS -status | Format-Table -autosize

Stop-AzVM -ResourceGroupName myRGPS -Name myVMPS
```

</details>

<details>

<summary><strong>Using CLI</strong> </summary>

{% hint style="info" %}
_Note that if you've already started PowerShell then you can switch to Bash when needed._
{% endhint %}

1. Click on the Azure Cloud Shell and select `cli` from the options.&#x20;

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Select the mount storage account section and select subscription on which you want to perform the operation.&#x20;

{% hint style="warning" %}
_Note that if you select mount storage account then it will create a storage account and create a file share and all your sessions of the powershell will be stored in it._&#x20;
{% endhint %}

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Selecting the mount storage option will next provide you with three option_&#x20;

1. _Either to select already built storage account._&#x20;
2. _Azure will create a storage account for you._&#x20;
3. _You'll create a storage account on your own._&#x20;
{% endhint %}

3. After that bash cli will start.

<figure><img src="../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption></figcaption></figure>

4. Verify resource group and create a new virtual machine.&#x20;

{% code overflow="wrap" %}
```bash
az group list --output table

az vm create \
--name myVMCLI \
--resource-group myRGCLI-lod60176453 \
--image Ubuntu2204 \
--location EastUS2 \
--admin-username azureuser \
--admin-password Pa$$w0rd1234

az vm show --resource-group myRGCLI-lod60176453 --name myVMCLI --show-details --output table

 az vm stop --resource-group myRGCLI-lod60176453 --name myVMCLI 
```
{% endcode %}

</details>

### Create Storage Account & Blob Storage

#### Create Storage Account &#x20;

{% hint style="info" %}
_To create a blob storage we must have a storage container._&#x20;
{% endhint %}

1. Click on create button or Create storage account button.&#x20;

<figure><img src="../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Select the appropriate options from the settings.&#x20;

{% tabs %}
{% tab title="Basics" %}
<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Advanced" %}
<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Networking" %}
<figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Data Protection" %}
<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Encryption" %}
<figure><img src="../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
_Note that the identities are just used to access the key vault. So whenever encryption and decryption occurs the identity are used automatically to encrypt/decrypt data while performing read/write operation._&#x20;
{% endhint %}

#### Create Blob Storage&#x20;

1. Navigate to newly created storage account and from the side bar select container and create new container.&#x20;

<figure><img src="../.gitbook/assets/image (20) (1).png" alt=""><figcaption></figcaption></figure>

Once the container is created from the storage browser you can upload images to the blob containers. More permission on access can later implemented from container section.&#x20;

<figure><img src="../.gitbook/assets/image (21) (1).png" alt=""><figcaption></figcaption></figure>

### Configuring Encryption in Storage account&#x20;

{% hint style="info" %}
_Encryption configured at the storage account level is automatically applied to all data within the account. However, encryption scopes allow you to define different encryption settings for specific blobs or containers within the same storage account._
{% endhint %}

1. Inside storage account settings select `Security + Networking` -> `Encryption`.&#x20;

<figure><img src="../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. To enable encryption for an entire storage account, configure the encryption type in the storage account settings. To apply different encryption settings to specific blobs or containers, create and use an encryption scope.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_With encryption scopes, a single storage account can use multiple encryption keys for different data within the account._
{% endhint %}

### Lifecycle management&#x20;

1. Inside storage account settings select `Data management` -> `Lifecycle Management`.&#x20;

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Lifecycle management rules can be applied to all blobs as well as selected blobs as well.&#x20;

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. We can add conditions accordingly for moving the blobs from `Hot` -> `Cool`-> `Cold` -> `Archive` -> `Delete`.&#x20;

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Immutable Storage

It allows users to store critical data in (Write Once, Read Many) state.&#x20;

* Time-based retention policy: With a time-based retention policy, users can set policies to store data for a specific interval where objects can be created and read, but not modified or deleted. After retention period has expired, objects can be deleted but not overwritten.&#x20;
* Legal hold policies: A legal hold stores immutable data until the legal hold is explicitly cleared. Same as time-based but data is hold until someone removes the lock.&#x20;

1. Select `access policy` from settings of the container.&#x20;

<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Click on add policy under `Immutable blob Storage` and select necessary options.&#x20;

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Network Security&#x20;

To configure storage account to be accessed from specific Vnet and subnet.&#x20;

1. Select `Security + Networking` -> `Networking` from settings of storage account.&#x20;

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_When you go to "Public network access" and choose "Enabled from selected virtual networks and IP addresses," you are typically using Service Endpoints which will still have public IP address but only specified subnet traffic will be accepted._&#x20;

_When you create a Private Endpoint, you are effectively putting your Storage account inside your VNet._
{% endhint %}

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

choose options accordingly.&#x20;

### Implement Azure Key Vault&#x20;

1. Open the key vault and click on create.&#x20;

<figure><img src="../.gitbook/assets/image (395).png" alt=""><figcaption></figcaption></figure>

2. Enter the required details and click on `Review + create`.&#x20;

<figure><img src="../.gitbook/assets/image (396).png" alt=""><figcaption></figcaption></figure>

3. Once the key vault has been created then you can find the objects to be stored in it in the side bar.&#x20;

<figure><img src="../.gitbook/assets/image (397).png" alt=""><figcaption></figcaption></figure>

### Manage RBAC in Azure&#x20;

1. Select `Access Control(IAM)` in the required resource you want and click on `Add`. and then `Add Role Assignment`.&#x20;

<figure><img src="../.gitbook/assets/image (398).png" alt=""><figcaption></figcaption></figure>

2. In the add role assignment, select the role you want to give to a specific member or identity and let's go.&#x20;

<figure><img src="../.gitbook/assets/image (399).png" alt=""><figcaption></figcaption></figure>

### Creating an Azure Policy&#x20;

1. Click on `Authoring` -> `Assignment` -> `Assign Policy` section under the policy.&#x20;

<figure><img src="../.gitbook/assets/image (400).png" alt=""><figcaption></figcaption></figure>

2. Select appropriate policy to implement from the policy definition selection section.&#x20;

<figure><img src="../.gitbook/assets/image (401).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (402).png" alt=""><figcaption></figcaption></figure>

3. Assign parameters according to your need and click on `Review + create`.&#x20;

<figure><img src="../.gitbook/assets/image (403).png" alt=""><figcaption></figcaption></figure>

