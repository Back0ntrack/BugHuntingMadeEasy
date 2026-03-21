# Practicals

{% hint style="info" %}
_In Azure, we can use either the search bar or the sidebar to find and create the required resources._
{% endhint %}

## Azure Fundamentals&#x20;

### Create VM&#x20;

<details>

<summary><strong>Using Azure Portal</strong></summary>

1. Click on the create option and select virtual machine.&#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

2. Select things as per requirement.&#x20;

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

There are not much other things to be taken care of while creating a virtual machine. Thus click on `Review + create` and you're done.&#x20;

</details>

<details>

<summary><strong>Using Azure PowerShell</strong></summary>

1. Click on the Azure Cloud Shell and select powershell from the options.&#x20;

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

2. Select the mount storage account section and select subscription on which you want to perform the operation.&#x20;

{% hint style="warning" %}
_Note that if you select mount storage account then it will create a storage account and create a file share and all your sessions of the powershell will be stored in it._&#x20;
{% endhint %}

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Selecting the mount storage option will next provide you with three option_&#x20;

1. _Either to select already built storage account._&#x20;
2. _Azure will create a storage account for you._&#x20;
3. _You'll create a storage account on your own._&#x20;
{% endhint %}

3. After that PowerShell will start.&#x20;

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

2. Select the mount storage account section and select subscription on which you want to perform the operation.&#x20;

{% hint style="warning" %}
_Note that if you select mount storage account then it will create a storage account and create a file share and all your sessions of the powershell will be stored in it._&#x20;
{% endhint %}

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Selecting the mount storage option will next provide you with three option_&#x20;

1. _Either to select already built storage account._&#x20;
2. _Azure will create a storage account for you._&#x20;
3. _You'll create a storage account on your own._&#x20;
{% endhint %}

3. After that bash cli will start.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

2. Select the appropriate options from the settings.&#x20;

{% tabs %}
{% tab title="Basics" %}
<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Advanced" %}
<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Networking" %}
<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Data Protection" %}
<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Encryption" %}
<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
_Note that the identities are just used to access the key vault. So whenever encryption and decryption occurs the identity are used automatically to encrypt/decrypt data while performing read/write operation._&#x20;
{% endhint %}

#### Create Blob Storage&#x20;

1. Navigate to newly created storage account and from the side bar select container and create new container.&#x20;

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Once the container is created from the storage browser you can upload images to the blob containers. More permission on access can later implemented from container section.&#x20;

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

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
