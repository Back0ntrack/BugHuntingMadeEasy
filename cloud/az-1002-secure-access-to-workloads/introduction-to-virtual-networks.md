# Introduction to Virtual Networks

## **Azure Virtual Networks**

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

* A **Virtual Network (VNet)** is a **logical isolation of Azure resources**
* Used to create **private networks in the cloud**

Azure Virtual Networks (VNets) enable secure communication between:

* Azure resources
* The internet
* On-premises infrastructure

### **Core Features**

* Supports **VPN setup** in Azure
* Uses **CIDR IP address ranges**
* Can connect to:
  * Other VNets
  * On-premises networks _(non-overlapping CIDR required)_
* Allows:
  * Custom **DNS configuration**
  * Network segmentation using **subnets**

### Azure Subnets&#x20;

Azure subnets provide a way to implement logical divisions within your virtual network.&#x20;

#### 🌐 **Subnet Fundamentals**

* Each subnet:
  * Uses a **range of IPs from the VNet address space**
  * Must have a **unique IP range across the virtual network**
  * **Cannot overlap** with other subnets
* Defined using **CIDR notation**
* A VNet can contain **multiple subnets**

### **Reserved IP address**

In every subnet, **5 IP addresses are reserved by Azure**:

<table data-full-width="true"><thead><tr><th>IP Address</th><th>Purpose</th></tr></thead><tbody><tr><td><code>.0</code></td><td>Network address</td></tr><tr><td><code>.1</code></td><td>Default gateway</td></tr><tr><td><code>.2, .3</code></td><td>Azure DNS</td></tr><tr><td>Last IP</td><td>Broadcast address</td></tr></tbody></table>

### Public IP attachment

| Resource                                                        | Configuration Type       |
| --------------------------------------------------------------- | ------------------------ |
| Virtual Machine                                                 | Network Interface        |
| VPN Gateway / ExpressRoute / NAT Gateway                        | Gateway IP configuration |
| Load Balancer / Application Gateway / Firewall / API Management | Frontend configuration   |
| Bastion Host                                                    | Public IP configuration  |

### VNet Peering

<figure><img src="../../.gitbook/assets/global-vnet-peering-2368962c.png" alt=""><figcaption></figcaption></figure>

Virtual network peering enables you to seamlessly connect two Azure virtual networks. Once peered, the virtual networks appear as one, for connectivity purposes. There are two types of VNet peering.

* **Regional VNet peering** connects Azure virtual networks in the same region.
* **Global VNet peering** connects Azure virtual networks in different regions. The peered virtual networks can exist in any Azure public cloud region or China cloud regions, but not in Government cloud regions. You can only peer virtual networks in the same region in Azure Government cloud regions.

### Extended peering with User defined routes

&#x20;Suppose you have three virtual networks: A, B, and C. You establish virtual network peering between networks A and B, and also between networks B and C. You don't set up peering between networks A and C. The virtual network peering capabilities that you set up between networks B and C don't automatically enable peering communication capabilities between networks A and C.

This is done with help of User defined routes.&#x20;

## Traffic Control&#x20;

### Routing&#x20;

Defines **how packets travel**

#### **Types:**&#x20;

**✅ System Routes**

* Default routes by Azure
* Enable automatic communication

**✅ User Defined Routes (UDR)**

* Override system routes
* Force traffic via:
  * Firewall
  * NVA

**✅ BGP**

* Dynamic route exchange (Azure ↔ On-prem)

### Network Security Groups (NSG)

Layer 4 firewall (IP + Port based)

#### What it does:

* Allow / Deny traffic
* Works at:
  * Subnet level
  * NIC level

#### Key feature:

* Stateful (remembers connections)

### Application Security Groups (ASG)

#### What it is:

Logical grouping of VMs

#### What it does:

* Replace IP-based rules with:
  * Application-based grouping

#### Why important:

➡️ Makes large environments manageable

### **Private Endpoint**

#### What it is:

Private connection to Azure services

#### What it does:

* Access services via **private IP**
* No internet involved

### **Service Endpoint**

#### What it is:

Extends VNet identity to Azure services

#### What it does:

* Secure connection to Azure services
* Still uses public endpoint (but restricted)
