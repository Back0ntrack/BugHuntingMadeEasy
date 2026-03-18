# Microsoft Security Solutions

## Privileged Identity Management&#x20;

Privileged Identity Management (PIM) is a service of Microsoft Entra ID that enables you to manage, control, and monitor access to important resources in your organization. These include resources in Microsoft Entra, Azure, and other Microsoft online services such as Microsoft 365 or Microsoft Intune.

PIM is:

* Just in time, providing privileged access only when needed, and not before.
* Time-bound, by assigning start and end dates that indicate when a user can access resources.
* Approval-based, requiring specific approval to activate privileges.
* Visible, sending notifications when privileged roles are activated.
* Auditable, allowing a full access history to be downloaded.

### General Workflow&#x20;

* assign&#x20;
* activate&#x20;
* approve/deny
* extend and renew&#x20;

## Microsoft Security Copilot&#x20;

**Microsoft Security Copilot** is a **Generative AI-powered cybersecurity assistant** designed to help security teams **detect, investigate, and respond to threats faster**.

* Built using **LLMs (like GPT via Azure OpenAI) + Microsoft threat intelligence**
* Works across **security, identity, endpoint, cloud, and data environments**
* Enhances **SOC (Security Operations Center)** productivity

👉 Official definition:\
It is a _“generative AI-powered security solution that improves defender efficiency at machine speed and scale”_

### How Security Copilot Works&#x20;

* LLM (Large Language Model)
  * Core AI brain
  * Understands prompts and generates responses
* Microsoft Security data
  * Uses real security data from:
    * Microsoft Defender
    * Entra
    * Sentinel
    * Other sources
* Microsoft Security-Specific System
  * Combines:&#x20;
    * AI Model&#x20;
    * Security Data&#x20;
    * Microsoft Threat Intelligence

### Ways to Use Security Copilot&#x20;

#### 🔹 1. Standalone Experience

* Dedicated interface (like ChatGPT)
* Used for:
  * Investigations
  * Queries
  * Analysis

#### 🔹 2. Embedded Experience

* Integrated into Microsoft security tools:
  * Defender
  * Sentinel
  * etc.

### Terminology of Security Copilot

#### Core Interaction Concepts&#x20;

* Session
  * A **session** represents a **single conversation** with Security Copilot.
  * It maintains **context** across multiple interactions.
  * All prompts and responses within a session are **connected and remembered**.
* Prompt
  * A **prompt** is a **question, command, or instruction** given to Copilot.
  * It is entered in the **prompt bar**.
  * Copilot processes prompts to generate responses.
* Capability
  * A **capability** is a **function or feature** that Copilot uses to solve a task.
  * It represents **what Copilot can do**.
* Plugins
  * **Plugins** extend Copilot capabilities.
  * They connect Copilot to:
    * Microsoft security tools
    * Third-party tools
* Workspace
  * A **workspace** = a **separate Copilot work environment within a tenant**
* **Agents** = AI-powered tools that:
  * Autonomously manage **security + IT tasks**

#### Prompt Interaction Model&#x20;

* **Prompt Bar**&#x20;
  * Central interface for security copilot to enter prompts and request insights
* **Promptbooks**&#x20;
  * It includes
    * Predefined Prompts
    * Suggested Prompts

<figure><img src="../../.gitbook/assets/image (389).png" alt=""><figcaption></figcaption></figure>

### Process Flow&#x20;

<figure><img src="../../.gitbook/assets/image (390).png" alt=""><figcaption></figcaption></figure>

## Core Infrastructure Security services&#x20;

### Azure DDoS Protection&#x20;

It provides protection at layer 3 and 4 only which is (Network and Transport Layer).&#x20;

#### Types  of DDoS Attack&#x20;

* **Volumetric Attacks**
  * Flood **network layer** with large traffic
  * Traffic appears **legitimate**
  * Effect:
    * Consumes bandwidth
      * Blocks real users
* **Protocol Attacks**
  * Exploit **Layer 3 (Network)** and **Layer 4 (Transport)**
  * Use **fake protocol requests**
  * Effect:
    * Exhaust server resources
      * Make target inaccessible
* **Resource (Application Layer) Attacks**
  * Target **web application packets**
  * Disrupt:
    * Communication between hosts

#### Key Features&#x20;

* Always-On traffic Monitoring
* Adaptive Real-Time Tuning
* Telemetry, Monitoring and Alerting
  * Provides rich telemetry via Azure Monitor

#### DDoS Protection Tiers&#x20;

1. DDoS Network Protection
2. DDoS IP Protection
   1. It lacks value-added services of Network protection.&#x20;

### Azure Firewall&#x20;

* A **firewall** is a:
  * Hardware / Software / Combination
* Function:
  * Monitors **incoming & outgoing traffic**
  * Applies **predefined security rules**

👉 Purpose:

* Acts as a **barrier between**:
  * Trusted internal network
  * Untrusted external networks (internet)

#### Core features&#x20;

1. Stateful firewall
   1. Tracks state of active connections and,
   2. make decisions based on context of traffic (not just individual packets)
2. High availability&#x20;
3. Network and Application Level Filtering
4. NAT capabilities
5. Threat Intelligence
6. Logging and Monitoring
7. Integration with azure services and Microsoft Security Copilot

#### Used in&#x20;

* Centralized Virtual Network
  * ALL VNets
  * On-premises network

### Web Application Firewall&#x20;

Same as any other WAF but provides integration with Azure tools and Azure security copilot at application layer (Layer - 7).

### Network Segmentation&#x20;

Provided with the help of Azure Virtual Network (VNet) which becomes a fundamental building block of private network in azure and Subnet.&#x20;

Each VNet can contain multiple subnets.&#x20;

<figure><img src="../../.gitbook/assets/image (391).png" alt=""><figcaption></figcaption></figure>

### Network Security Groups&#x20;

* **Network Security Group (NSG)**:
  * Filters **network traffic**
  * Controls:
    * Inbound traffic
    * Outbound traffic

👉 Applies to:

* Azure resources (e.g., Virtual Machines)
* Inside an **Azure Virtual Network (VNet)**

{% hint style="info" %}
_NSG = **set of rules** that define:_

* _Allow / Deny traffic_
{% endhint %}

#### Where it can be applied&#x20;

Only one NSG can be applied per:&#x20;

* Subnet&#x20;
* Network Interface (NIC of VM)

_We can apply same NSG to multiple VMs or Subnet_

{% hint style="warning" %}
_By default azure automatically creates:_&#x20;

* _3 inbound rules_
* _3 outbound rules_

_You cannot delete these rules but can override them by create new rules with higher priorities._&#x20;
{% endhint %}

#### Default Inbound rules&#x20;

* AllowVNetInBound - The AllowVNetInBound rule is processed first as it has the lowest priority value. Recall that rules with the lowest priority value get processed first. This rule allows traffic from a source with the VirtualNetwork service tag to a destination with the VirtualNetwork service tag on any port, using any protocol. If a match is found for this rule, then no other rules are processed. If no match is found, then the next rule gets processed.
* AllowAzureLoadBalancerInBound - The AllowAzureLoadBalancerInBound rule is processed second, as its priority value is higher than the AllowVNetInBound rule. This rule allows traffic from a source with the AzureLoadBalancer service tag to a destination with the AzureLoadBalancer service tag on any port to any IP address on any port, using any protocol. If a match is found for this rule, then no other rules are processed. If no match is found, then the next rule gets processed.
* DenyAllInBound - The last rule in this NSG is the DenyAllInBound rule. This rule denies all traffic from any source IP address on any port to any other IP address on any port, using any protocol.

{% hint style="info" %}
_In summary, any virtual network subnet or network interface card to which this NSG is assigned will only allow inbound traffic from an Azure Virtual Network or an Azure load balancer (as defined by their respective service tags). All other inbound network traffic is denied._
{% endhint %}

### Azure Bastion&#x20;

* The Azure Bastion service is a fully platform-managed PaaS service that you provision inside your virtual network.&#x20;
* Azure Bastion provides secure and seamless RDP and SSH connectivity to your virtual machines directly from the Azure portal using Transport Layer Security (TLS).
* Deployed per virtual network and once you provision the Azure Bastion service in your virtual network, the RDP/SSH experience is available to all your VMs in the same VNet, and peered VNets without public IP.

#### Why it is needed&#x20;

What if a remote developer want to use some virtual machine. We need to open a port for him and provide access to the virtual machine via RDP port which increase the attack surface for attackers as well.&#x20;

Thus azure bastion provides a solution by securely connecting to virtual machine using a browser.&#x20;

<figure><img src="../../.gitbook/assets/image (392).png" alt=""><figcaption></figcaption></figure>

### Azure Key Vault&#x20;

Azure Key Vault is a cloud service for securely storing and accessing secrets. A secret is anything that you want to tightly control access to, such as API keys, passwords, certificates, or cryptographic keys.

It helps in managing:&#x20;

* Secret management
  * secure and controlled access to tokens, passwords, certificates, APIs
* Key management&#x20;
  * can be used as key management solution for creating and storing encryption keys
* Certificate managment
  * Allows to provision, manage and deploy your public and private SSL/TLS certificates for Azure and internally connnected resources.&#x20;

<figure><img src="../../.gitbook/assets/image (393).png" alt=""><figcaption></figcaption></figure>



## Azure Sentinel

<figure><img src="../../.gitbook/assets/image (394).png" alt=""><figcaption></figcaption></figure>

### SIEM (Security Information and Event Management)

A SIEM system is a tool that an organization uses to collect data from across the whole estate, including infrastructure, software, and resources. It does analysis, looks for correlations or anomalies, and generates alerts and incidents

### SOAR (Security Orchestration Automated Response)

A SOAR system takes alerts from many sources, such as a SIEM system. The SOAR system then triggers action-driven automated workflows and processes to run security tasks that mitigate the issue.

