# AZ - 1002 (Secure access to workloads)

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Note: The environment currently consists of VNet1 and VNet2 in West Europe, housing VM1 (Subnet1-1) and VM2/VM3 (Subnet2-1). The following tasks outline the new infrastructure, routing, and security configurations required.

* Task 1: Deploy Azure Firewall and a Route Table for Subnet1-1 to block all public internet traffic except for connections to RelecloudDataService ($$131.107.3.210$$) on TCP port 9000.
* Task 2: Deploy VNet4 in North Europe with Subnet having address space 10.3.1.0/24 for VM4, and establish a VNet Peering connection specifically between VNet4 and VNet2.
* Task 3: Configure a Private DNS zone contoso.com with auto-registration enabled for VNet4, and create an A record so vm2.contoso.com resolves to VM2's IP for hosts in the new subnet.
* Task 4: Create an Application Security Group (ASG) containing VM2 and VM3, then configure an NSG rule on Subnet1-1 to allow inbound traffic _only_ from that ASG over TCP port 1433.

{% hint style="danger" %}
_Note that regions may vary due to policy violation in certain regions. Steps will be same irrespective of the region._&#x20;
{% endhint %}

## Create a virtual network&#x20;

1. Select virtual network and click on create.&#x20;

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. Enter details accordingly.&#x20;

{% tabs %}
{% tab title="Basics" %}
<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Security" %}
<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="IP addresses" %}
<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
_Note that two address space (subnet) can't have same IP address range. Also if you want network peering that also requires different IP address range even within different virtual network._&#x20;
{% endhint %}

3. After selecting required options click on `Review + Create`. Note that address space selected in `IP addresses` section is for VNet range not Subnet range.&#x20;

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

## Create Subnet&#x20;

1. Click on subnet from the `Settings` of the VNet and then click on Add.&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

2. Enter the required details and click on Add.&#x20;

<figure><img src="../../.gitbook/assets/image (6).png" alt="" width="563"><figcaption></figcaption></figure>

## Establishing VNet Peering&#x20;

1. Click on `Peerings` from the Settings of the VNet4 and then click on Add.&#x20;

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Understanding the concept</strong> </summary>

You are creating a **VNet Peering between TWO VNets**:

```
VNet4  ←────────→  Other VNet (say VNet1)
```

Azure actually creates **2 connections internally**:

* VNet4 ➝ Remote VNet
* Remote VNet ➝ VNet4

That’s why you see:

* Remote side config
* Local side config

## 1. Remote Virtual Network Summary

👉 This defines **“WHO are you connecting TO”**

#### Fields:

* **Peering link name**\
  → Name of connection FROM VNet4 TO other VNet\
  Example: `VNet4-to-VNet2`
* **Peering type**\
  → Keep `Virtual network` (Subnet peering is advanced)
* **Subscription**\
  → Where the remote VNet exists
* **Virtual network**\
  → Select target VNet (e.g., VNet2)

## 2. Remote Virtual Network Peering Settings&#x20;

This defines **WHAT the remote VNet can do with VNet4**

#### ✅ Allow the peered virtual network to access 'VNet4'

👉 Means:

* Remote VNet (VNet1) **can reach VNet4 resources**

📌 Without this → NO communication

#### ✅ Allow the peered virtual network to receive forwarded traffic

👉 Advanced (used with Firewall / NVA)

✔️ Enable ONLY if:

* You have firewall routing traffic
* Using UDR (User Defined Routes

#### ⛔ Allow gateway or route server to forward traffic

👉 Used when:

* VPN Gateway / ExpressRoute involved

📌 For now → KEEP OFF

#### ⛔ Enable remote gateway usage

👉 Means:

* VNet4 will use **other VNet's VPN gateway**

📌 Only for hybrid networking → ignore now

## 3. Local Virtual Network Summary&#x20;

What VNet4 can do with the remote VNet

#### ✅ Allow 'VNet4' to access the peered virtual network

✔️ Enabled

👉 Means:

* VNet4 can talk to remote VNet

📌 This is REQUIRED for communication

#### ✅ Allow 'VNet4' to receive forwarded traffic

→ Same as before (firewall scenarios)

#### ⛔ Allow gateway or route server

→ VPN / ExpressRoute use

#### ⛔ Use remote gateway

→ Advanced hybrid networking

</details>

2. Select necessary option and click on Add.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

3. Verify the status `connected` which proves that two ways communication is established.&#x20;

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## Deploy Azure Firewall&#x20;

### Create Subnet&#x20;

{% hint style="info" %}
_We need to create a subnet for the firewall in the VNet where we want to deploy a firewall._&#x20;
{% endhint %}

1. Navigate to the specific VNet where you want to deploy the firewall and select Subnet from the sidebar and click on `Create`.

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

2. Select the necessary options and click on `Add`.&#x20;

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

### Deploy Firewall&#x20;

1. Click on `Create` in the Azure Firewalls section from the sidebar.&#x20;

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

2. Select the required option and click on `Review + Create` and `Create` button will deploy the firewall.&#x20;

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

3. Copy the private firewall IP for future use.&#x20;

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

### Create Rules&#x20;

1. Select `Rules` from the Settings in the sidebar of the Firewall section.&#x20;

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

2. Select `Network Rule Collection` and clik on Add.&#x20;

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

3. Select the required options and click on Add.&#x20;

{% hint style="info" %}
_Note that Azure Firewall allows only what is explicitly permitted, and everything else is denied due to an implicit deny rule._
{% endhint %}

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

4. Once the rule is implemented it will only allow traffic to specified destination and port. But we've not yet forced the subnet traffic to go through firewall. that is done with help of UDR.&#x20;

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## Create UDR (User Defined Routing)

> _Till now we've deployed a firewall and an allow rule in it which allows access to ReleCloud Service only via internet. But we need to route traffic from subnet to firewall in order to implement firewall policies._&#x20;

### Create Route Table

1. Select `Route tables` from the sidebar and click on `Create`.&#x20;

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

2. Select required options and click on `Review + Create` and `Create`.&#x20;

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### Create UDR

1. Navigate to the deployed resource.

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

2. click on `Routes` from the setting and click on `Add`. and enter required details and click on `Add`.&#x20;

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Note that this only created a route rule that on whoever this Route table is applied have to forcefully send traffic to firewall (10.0.1.4) when he wants to access internet. But we've not yet associated it with any subnet._&#x20;
{% endhint %}

### Associate with Subnet&#x20;

1. Once the Route Table is created select `Subnets` from the Settings of Route table.&#x20;

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

2. Click on `Associate`, enter required details and click on `Add`&#x20;

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

## Create Private DNS Zone&#x20;

### Create Zone

1. Select `Private DNS Zone` from the search bar and click on Create.&#x20;

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

2. Enter required details and click on `Review + Create`.&#x20;

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### Create Virtual Network Links&#x20;

1. Select `Virtual Network Links` from DNS management and click on `Add`.&#x20;

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

2. Enter required details for VNet4 and click on `Create`.&#x20;

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_When a VNet is linked to a Private DNS zone with auto-registration enabled, **VMs in that VNet automatically create and update their DNS A records (hostname → private IP) in the zone.**_\
\
A VM named `vm4` in VNet4 gets private IP `10.2.1.4`, and with auto-registration enabled, it is automatically registered as:

👉 `vm4.contoso.com → 10.2.1.4`
{% endhint %}

### Create Record Set&#x20;

{% hint style="info" %}
_Virtual Network Link is mandatory for a VNet to use (resolve) records from a Private DNS zone, regardless of whether it is created before or after record sets._
{% endhint %}

1. Select `Recordsets` from DNS management and click on Add.&#x20;

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

2. Add necessary details and click on Add. (_Note that we need to add record if a VM or resource is already there before auto registration or its entry is not added_)

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

## Create a NSG

1. Select network security group from the side bar and click on `create`.

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

2. Provide the required settings and you're good to go.&#x20;

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

3. NSG is deployed with some predefined inbound and outbound rules.&#x20;

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

4. Now select `Settings` -> `Network interfaces` -> `Associate` and select the required network interface on which you want to apply NSG.&#x20;

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

5. After association you can add inbound and outbound rule from `Inbound securtiy rules` and `Outbound security rules`.&#x20;

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

6. Setting Inbound rule to allow any source to connect to RDP.&#x20;

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
_This thing can also be added from the interface of the resource on which the NSG is associated._&#x20;
{% endhint %}
