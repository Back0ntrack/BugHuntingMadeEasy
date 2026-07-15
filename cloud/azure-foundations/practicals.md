# Practicals

{% hint style="info" %}
_In Azure, we can use either the search bar or the sidebar to find and create the required resources._
{% endhint %}

## Azure Fundamentals&#x20;

### Create VM&#x20;

<details>

<summary><strong>Using Azure Portal</strong></summary>

1. Click on the create option and select virtual machine.&#x20;

<figure><img src="../../.gitbook/assets/image (805).png" alt=""><figcaption></figcaption></figure>

2. Select things as per requirement.&#x20;

<figure><img src="../../.gitbook/assets/image (807).png" alt=""><figcaption></figcaption></figure>

There are not much other things to be taken care of while creating a virtual machine. Thus click on `Review + create` and you're done.&#x20;

</details>

<details>

<summary><strong>Using Azure PowerShell</strong></summary>

1. Click on the Azure Cloud Shell and select powershell from the options.&#x20;

<figure><img src="../../.gitbook/assets/image (808).png" alt=""><figcaption></figcaption></figure>

2. Select the mount storage account section and select subscription on which you want to perform the operation.&#x20;

{% hint style="warning" %}
_Note that if you select mount storage account then it will create a storage account and create a file share and all your sessions of the powershell will be stored in it._&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (809).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Selecting the mount storage option will next provide you with three option_&#x20;

1. _Either to select already built storage account._&#x20;
2. _Azure will create a storage account for you._&#x20;
3. _You'll create a storage account on your own._&#x20;
{% endhint %}

3. After that PowerShell will start.&#x20;

<figure><img src="../../.gitbook/assets/image (810).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (812).png" alt=""><figcaption></figcaption></figure>

2. Select the mount storage account section and select subscription on which you want to perform the operation.&#x20;

{% hint style="warning" %}
_Note that if you select mount storage account then it will create a storage account and create a file share and all your sessions of the powershell will be stored in it._&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (809).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Selecting the mount storage option will next provide you with three option_&#x20;

1. _Either to select already built storage account._&#x20;
2. _Azure will create a storage account for you._&#x20;
3. _You'll create a storage account on your own._&#x20;
{% endhint %}

3. After that bash cli will start.

<figure><img src="../../.gitbook/assets/image (815).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (816).png" alt=""><figcaption></figcaption></figure>

2. Select the appropriate options from the settings.&#x20;

{% tabs %}
{% tab title="Basics" %}
<figure><img src="../../.gitbook/assets/image (817).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Advanced" %}
<figure><img src="../../.gitbook/assets/image (818).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Networking" %}
<figure><img src="../../.gitbook/assets/image (819).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Data Protection" %}
<figure><img src="../../.gitbook/assets/image (820).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Encryption" %}
<figure><img src="../../.gitbook/assets/image (821).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
_Note that the identities are just used to access the key vault. So whenever encryption and decryption occurs the identity are used automatically to encrypt/decrypt data while performing read/write operation._&#x20;
{% endhint %}

#### Create Blob Storage&#x20;

1. Navigate to newly created storage account and from the side bar select container and create new container.&#x20;

<figure><img src="../../.gitbook/assets/image (824).png" alt=""><figcaption></figcaption></figure>

Once the container is created from the storage browser you can upload images to the blob containers. More permission on access can later implemented from container section.&#x20;

<figure><img src="../../.gitbook/assets/image (825).png" alt=""><figcaption></figcaption></figure>

### Configuring Encryption in Storage account&#x20;

{% hint style="info" %}
_Encryption configured at the storage account level is automatically applied to all data within the account. However, encryption scopes allow you to define different encryption settings for specific blobs or containers within the same storage account._
{% endhint %}

1. Inside storage account settings select `Security + Networking` -> `Encryption`.&#x20;

<figure><img src="../../.gitbook/assets/image (794).png" alt=""><figcaption></figcaption></figure>

2. To enable encryption for an entire storage account, configure the encryption type in the storage account settings. To apply different encryption settings to specific blobs or containers, create and use an encryption scope.

<figure><img src="../../.gitbook/assets/image (795).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_With encryption scopes, a single storage account can use multiple encryption keys for different data within the account._
{% endhint %}

### Lifecycle management&#x20;

1. Inside storage account settings select `Data management` -> `Lifecycle Management`.&#x20;

<figure><img src="../../.gitbook/assets/image (796).png" alt=""><figcaption></figcaption></figure>

2. Lifecycle management rules can be applied to all blobs as well as selected blobs as well.&#x20;

<figure><img src="../../.gitbook/assets/image (797).png" alt=""><figcaption></figcaption></figure>

3. We can add conditions accordingly for moving the blobs from `Hot` -> `Cool`-> `Cold` -> `Archive` -> `Delete`.&#x20;

<figure><img src="../../.gitbook/assets/image (798).png" alt=""><figcaption></figcaption></figure>

### Immutable Storage

It allows users to store critical data in (Write Once, Read Many) state.&#x20;

* Time-based retention policy: With a time-based retention policy, users can set policies to store data for a specific interval where objects can be created and read, but not modified or deleted. After retention period has expired, objects can be deleted but not overwritten.&#x20;
* Legal hold policies: A legal hold stores immutable data until the legal hold is explicitly cleared. Same as time-based but data is hold until someone removes the lock.&#x20;

1. Select `access policy` from settings of the container.&#x20;

<figure><img src="../../.gitbook/assets/image (800).png" alt=""><figcaption></figcaption></figure>

2. Click on add policy under `Immutable blob Storage` and select necessary options.&#x20;

<figure><img src="../../.gitbook/assets/image (799).png" alt=""><figcaption></figcaption></figure>

### Network Security&#x20;

To configure storage account to be accessed from specific Vnet and subnet.&#x20;

1. Select `Security + Networking` -> `Networking` from settings of storage account.&#x20;

<figure><img src="../../.gitbook/assets/image (801).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_When you go to "Public network access" and choose "Enabled from selected virtual networks and IP addresses," you are typically using Service Endpoints which will still have public IP address but only specified subnet traffic will be accepted._&#x20;

_When you create a Private Endpoint, you are effectively putting your Storage account inside your VNet._
{% endhint %}

<figure><img src="../../.gitbook/assets/image (802).png" alt=""><figcaption></figcaption></figure>

choose options accordingly.&#x20;

### Implement Azure Key Vault&#x20;

1. Open the key vault and click on create.&#x20;

<figure><img src="../../.gitbook/assets/image (875).png" alt=""><figcaption></figcaption></figure>

2. Enter the required details and click on `Review + create`.&#x20;

<figure><img src="../../.gitbook/assets/image (876).png" alt=""><figcaption></figcaption></figure>

3. Once the key vault has been created then you can find the objects to be stored in it in the side bar.&#x20;

<figure><img src="../../.gitbook/assets/image (877).png" alt=""><figcaption></figcaption></figure>

### Manage RBAC in Azure&#x20;

1. Select `Access Control(IAM)` in the required resource you want and click on `Add`. and then `Add Role Assignment`.&#x20;

<figure><img src="../../.gitbook/assets/image (878).png" alt=""><figcaption></figcaption></figure>

2. In the add role assignment, select the role you want to give to a specific member or identity and let's go.&#x20;

<figure><img src="../../.gitbook/assets/image (879).png" alt=""><figcaption></figcaption></figure>

### Creating an Azure Policy&#x20;

1. Click on `Authoring` -> `Assignment` -> `Assign Policy` section under the policy.&#x20;

<figure><img src="../../.gitbook/assets/image (880).png" alt=""><figcaption></figcaption></figure>

2. Select appropriate policy to implement from the policy definition selection section.&#x20;

<figure><img src="../../.gitbook/assets/image (881).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (882).png" alt=""><figcaption></figcaption></figure>

3. Assign parameters according to your need and click on `Review + create`.&#x20;

<figure><img src="../../.gitbook/assets/image (883).png" alt=""><figcaption></figcaption></figure>

### Deploy a static website using Azure Blob Storage

1. Create desired Resource Group and storage account.&#x20;
2. In the resource group navigate to `Static Website` in the Data management section and enable it. Two containers named `$web` and `$logs` will be automatically created.&#x20;

<figure><img src="../../.gitbook/assets/image (898).png" alt=""><figcaption></figcaption></figure>

3. Add `index.html` and `404.html` in the $web container.&#x20;

<figure><img src="../../.gitbook/assets/image (899).png" alt=""><figcaption></figcaption></figure>

4. Copy the `primary endpoint` from the `static website` in `Data Management` from the storage account settings and open it in another browser tab.&#x20;

### Share Files Securely

1. Create resource group and a storage account.&#x20;
2. Create a container with name `partner-drop` and add a random file in it.&#x20;
3. Try to open the file using the URL. we can see that it is accessible.&#x20;
4. Now in the `access-policy` section of the `partner-drop` container click on `Add Policy`.&#x20;

<figure><img src="../../.gitbook/assets/image (900).png" alt=""><figcaption></figcaption></figure>

5. Add policy with name `partner-read-policy` and add today's date as start time and 1 hour later as end time and save the policy.&#x20;
6. Select the text file and generate `SAS` token from the settings and select `partner-read-policy` from the dropdown and generate SAS token and URL.&#x20;

<figure><img src="../../.gitbook/assets/image (901).png" alt=""><figcaption></figcaption></figure>

7. Now try to access the text file using the URL. Now it shows like this.&#x20;

<figure><img src="../../.gitbook/assets/image (902).png" alt=""><figcaption></figcaption></figure>

8. Now paste the URL generated while generating SAS Token.&#x20;

<figure><img src="../../.gitbook/assets/image (903).png" alt=""><figcaption></figcaption></figure>

9. Now delete the stored access policy. it will revoke all SAS tokens that were generated from it.&#x20;

{% hint style="info" %}
* &#x20;_If we don't link an access policy to the SAS token then we need to change the entire Azure Storage Account key for revoking the link access._&#x20;
* _This breaks every other application or service using that key._&#x20;
* _With access policy, just changing or removing the policy instantly revokes the access._&#x20;
{% endhint %}

### Set up new employee access Entra ID and RBAC&#x20;

1. Create new resource group and storage account.&#x20;
2. From the tenant create group inside the `manage` section. You can get the tenant by searching `Microsoft Entra ID`.&#x20;

<figure><img src="../../.gitbook/assets/image (904).png" alt=""><figcaption></figcaption></figure>

3. Click on `New Group` from the overview section.&#x20;

<figure><img src="../../.gitbook/assets/image (905).png" alt=""><figcaption></figcaption></figure>

4. Fill the required details and click on `Create`.&#x20;

<figure><img src="../../.gitbook/assets/image (906).png" alt=""><figcaption></figcaption></figure>

5. Select `users` from the tenant page in the manage section.&#x20;

<figure><img src="../../.gitbook/assets/image (907).png" alt=""><figcaption></figcaption></figure>

6. Click on `New User` and add details appropriately.&#x20;

<figure><img src="../../.gitbook/assets/image (908).png" alt=""><figcaption></figcaption></figure>

7. select the newly created users from the above section again and add it to the group from the `Edit` section.&#x20;
8. From the resource group assign reader role to the group. This thing can be done from groups section as well.&#x20;
9. Select `Access-Control` from the resource group and click on `Add` to add a role assignment.&#x20;

<figure><img src="../../.gitbook/assets/image (915).png" alt=""><figcaption></figcaption></figure>

10. Select reader role from the roles and select the whole group from the groups.&#x20;

<figure><img src="../../.gitbook/assets/image (916).png" alt=""><figcaption></figcaption></figure>

11. In the `check access` section of the IAM verify that it has readers role.&#x20;

<figure><img src="../../.gitbook/assets/image (917).png" alt=""><figcaption></figcaption></figure>

12. Sign in using `Alex` account and try to create a storage account in the resource group. you'll see the following error.&#x20;

<figure><img src="../../.gitbook/assets/image (918).png" alt=""><figcaption></figcaption></figure>

since the new user only has read access he can't create any storage account inside the resource group.&#x20;

### Build a simple website endpoint using Azure Functions

1. Create a Resource Group.&#x20;
2. Search and select `Functions app` and click on create.&#x20;

<figure><img src="../../.gitbook/assets/image (909).png" alt=""><figcaption></figcaption></figure>

3. Select `flex` consumption from the options.&#x20;

<figure><img src="../../.gitbook/assets/image (910).png" alt=""><figcaption></figcaption></figure>

4. Fill in the required details within the tabs that will open and select `Node JS` in the runtime stack in `basics` tab and click on `Create`.&#x20;
5. Verify the status if running of the functions app.&#x20;

<figure><img src="../../.gitbook/assets/image (911).png" alt=""><figcaption></figcaption></figure>

6. Create the function project using command as in below screenshot.&#x20;

<figure><img src="../../.gitbook/assets/image (912).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (913).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (914).png" alt=""><figcaption></figcaption></figure>

7. The Invoke URL will print `Hello World` on the browsere.&#x20;

### Manage Microsoft Entra Groups and Users&#x20;

#### Create Microsoft Entra Users&#x20;

1. Search `Microsoft Entra ID` and select `Users` in the manage section. and click on `New User`.

<figure><img src="../../.gitbook/assets/image (919).png" alt=""><figcaption></figcaption></figure>

2. Enter things as per the requirement and click on `Review + Create`.&#x20;

<figure><img src="../../.gitbook/assets/image (920).png" alt=""><figcaption></figcaption></figure>

#### Create Microsoft Entra Groups&#x20;

1. Search `Microsoft Entra ID` and select `Groups` in the manage section. and click on `New Group`.

<figure><img src="../../.gitbook/assets/image (921).png" alt=""><figcaption></figcaption></figure>

2. Add required things and click on `Create` .&#x20;

<figure><img src="../../.gitbook/assets/image (922).png" alt=""><figcaption></figcaption></figure>

### Manage Azure resource deployment using an Azure Resource Manager Template&#x20;

1. Navigate to the already created resource group and select `Export Templates` from the `Automation` tab.&#x20;

<figure><img src="../../.gitbook/assets/image (923).png" alt=""><figcaption></figcaption></figure>

2. you can make changes in this template as per your requirement.&#x20;
3. Search `Custom Deployment` in the portal and then paste your template in the `Build your own template in the editor`/&#x20;

<figure><img src="../../.gitbook/assets/image (924).png" alt=""><figcaption></figcaption></figure>

## AZ - 104

### Implement Azure backups for Virtual Machines&#x20;

1. navigate to the virtual machine for which you want to take backup and select `backup` in the `Backup + disaster recovery`.&#x20;

<figure><img src="../../.gitbook/assets/image (925).png" alt=""><figcaption></figcaption></figure>

2. Enter the required details and click on `Enable backup`.&#x20;

<figure><img src="../../.gitbook/assets/image (926).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_A recovery service vault will automatically gets created when you create backup of the vm._&#x20;
{% endhint %}

To initiate the first backup again navigate to the backup and click on `backup now`.&#x20;

<figure><img src="../../.gitbook/assets/image (927).png" alt=""><figcaption></figcaption></figure>

The same can be done using this commands:&#x20;

{% code overflow="wrap" %}
```powershell
az backup protection enable-for-vm --resource-group AZ300-RGlod62425250 --vault-name AZ300-RecoveryServicesVault-62425250 --vm VM3    --policy-name DefaultPolicy
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (929).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
Get-AzRecoveryServicesVault -Name "AZ300-RecoveryServicesVault-62425250" | Set-AzRecoveryServicesVaultContext

$policy = Get-AzRecoveryServicesBackupProtectionPolicy -Name "DefaultPolicy"

Enable-AzRecoveryServicesBackupProtection -ResourceGroupName "AZ300-RGlod62425250" -Name "VM2" -Policy $policy
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (928).png" alt=""><figcaption></figcaption></figure>

