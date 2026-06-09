# Host Discovery

Determines which hosts are **alive** before port scanning.&#x20;

## Default Things

&#x20; ⇒  On local networks, Nmap uses ARP requests for host discovery by default.

&#x20; ⇒  For remote hosts, or when ARP cannot be used, Nmap falls back to discovery method listed  below in random sequence.&#x20;

{% code overflow="wrap" %}
```
Host Discovery
├── ARP Discovery (-PR)
├── ICMP Echo Discovery (-PE)
├── ICMP Timestamp Discovery (-PP)
├── TCP SYN Discovery (-PS)
└── TCP ACK Discovery (-PA)
```
{% endcode %}

{% hint style="info" %}
_`-sn` is used in all host discovery scans to avoid port discovery. Also if we use only `-sn` then Nmap will try any of the above listed method for host discovery._&#x20;
{% endhint %}

## -PR (ARP Ping)

ARP is used to resolve an IP address to its corresponding MAC address on the local network.

### Scan

<figure><img src="../../../.gitbook/assets/image (463).png" alt=""><figcaption></figcaption></figure>

### Analysis

<figure><img src="../../../.gitbook/assets/image (464).png" alt=""><figcaption></figcaption></figure>

## -PE (Ping Scan)

**ICMP Echo (Ping)** is used to verify device reachability and connectivity.

### Scan

<figure><img src="../../../.gitbook/assets/image (456).png" alt=""><figcaption></figcaption></figure>

### Analysis

<figure><img src="../../../.gitbook/assets/image (457).png" alt=""><figcaption></figcaption></figure>

## -PP (Timestamp Ping)

**ICMP Timestamp** is used to query remote time for clock synchronization and transit-time analysis.

### Scan

<figure><img src="../../../.gitbook/assets/image (458).png" alt=""><figcaption></figcaption></figure>

### Analysis

<figure><img src="../../../.gitbook/assets/image (459).png" alt=""><figcaption></figcaption></figure>

## -PM (ICMP Address mask Ping)&#x20;

ICMP Address Mask Requests were historically used by hosts to discover their subnet mask from a router or gateway.

### Scan

<figure><img src="../../../.gitbook/assets/image (460).png" alt=""><figcaption></figcaption></figure>

### Analysis

<figure><img src="../../../.gitbook/assets/image (461).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
_ICMP Address Mask Requests (`-PM`) are largely obsolete and are ignored by most modern operating systems. As a result, this discovery method rarely receives a response, even on local networks without a firewall._
{% endhint %}

## -PS (TCP SYN Ping)

### Scan

<figure><img src="../../../.gitbook/assets/image (484).png" alt=""><figcaption></figcaption></figure>

### Analysis

<figure><img src="../../../.gitbook/assets/image (485).png" alt=""><figcaption></figcaption></figure>

## -PA (TCP ACK Ping)

### Scan

<figure><img src="../../../.gitbook/assets/image (486).png" alt=""><figcaption></figcaption></figure>

### Analysis

<figure><img src="../../../.gitbook/assets/image (487).png" alt=""><figcaption></figcaption></figure>

## -PU (UDP Ping)

### Scan

<figure><img src="../../../.gitbook/assets/image (488).png" alt=""><figcaption></figcaption></figure>

### Analysis

<figure><img src="../../../.gitbook/assets/image (489).png" alt=""><figcaption></figcaption></figure>

## -PO (Protocol Ping)

IP protocols are used to identify what type of network communication an IP packet contains so the receiving host knows how to process it.

<table><thead><tr><th width="104.20001220703125">Protocol Number</th><th width="114.4000244140625">Protocol</th><th width="142.39996337890625">What Nmap Sends</th><th width="176.199951171875">What it is Used For</th><th>How Nmap Detects Host is Alive</th></tr></thead><tbody><tr><td><code>1</code></td><td>ICMP</td><td>ICMP Echo Request</td><td>Test ICMP support and reachability</td><td>Receives ICMP Echo Reply or another ICMP response</td></tr><tr><td><code>2</code></td><td>IGMP</td><td>IGMP Membership Query</td><td>Test multicast-capable hosts/routers</td><td>Receives IGMP Membership Report (rare on normal hosts)</td></tr><tr><td><code>3</code></td><td>IPv4</td><td>Raw IP packet with Protocol=3</td><td>Trigger a response from hosts that don't support GGP</td><td>Receives ICMP Protocol Unreachable, proving the host processed the packet</td></tr><tr><td><code>6</code></td><td>TCP</td><td>Raw TCP protocol packet</td><td>Test TCP protocol handling</td><td>Receives TCP response or ICMP error</td></tr><tr><td><code>17</code></td><td>UDP</td><td>Raw UDP protocol packet</td><td>Test UDP protocol handling</td><td>Receives UDP response or ICMP Port Unreachable</td></tr><tr><td><code>132</code></td><td>SCTP</td><td>SCTP INIT packet</td><td>Test SCTP protocol support and discover SCTP-enabled services</td><td>Receives an SCTP INIT-ACK, ABORT, or another SCTP response proving the host processed the packet</td></tr></tbody></table>

### PO1 Scan (ICMP)

<figure><img src="../../../.gitbook/assets/image (471).png" alt=""><figcaption></figcaption></figure>

#### **Analysis**

<figure><img src="../../../.gitbook/assets/image (470).png" alt=""><figcaption></figcaption></figure>

### PO2 Scan (IGMPv1)

<figure><img src="../../../.gitbook/assets/image (472).png" alt=""><figcaption></figcaption></figure>

#### **Analysis**

<figure><img src="../../../.gitbook/assets/image (473).png" alt=""><figcaption></figcaption></figure>

### PO3 Scan (IPv4)

<figure><img src="../../../.gitbook/assets/image (476).png" alt=""><figcaption></figcaption></figure>

#### **Analysis**

<figure><img src="../../../.gitbook/assets/image (475).png" alt=""><figcaption></figcaption></figure>

### PO6 Scan (TCP)

<figure><img src="../../../.gitbook/assets/image (478).png" alt=""><figcaption></figcaption></figure>

#### **Analysis**

<figure><img src="../../../.gitbook/assets/image (479).png" alt=""><figcaption></figcaption></figure>

### PO17 Scan (UDP)

<figure><img src="../../../.gitbook/assets/image (480).png" alt=""><figcaption></figcaption></figure>

#### **Analysis**

<figure><img src="../../../.gitbook/assets/image (481).png" alt=""><figcaption></figcaption></figure>

### PO132 Scan (SCTP)

<figure><img src="../../../.gitbook/assets/image (482).png" alt=""><figcaption></figcaption></figure>

#### **Analysis**

<figure><img src="../../../.gitbook/assets/image (483).png" alt=""><figcaption></figcaption></figure>

### Default PO

When `-PO` is used without specifying a protocol number, Nmap sends probes using multiple common IP protocols and determines whether the target is alive based on any valid response received.

<figure><img src="../../../.gitbook/assets/image (467).png" alt=""><figcaption></figcaption></figure>

#### **Analysis**

<figure><img src="../../../.gitbook/assets/image (465).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_`-PO` without a protocol number uses multiple common IP protocols for host discovery, making it more comprehensive but generally slower than using a specific scan._&#x20;
{% endhint %}

## Combining Flags

### Scan

<figure><img src="../../../.gitbook/assets/image (490).png" alt=""><figcaption></figcaption></figure>

### Analysis

<figure><img src="../../../.gitbook/assets/image (491).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Nmap sent multiple host discovery probes simultaneously (ICMP Echo, ICMP Timestamp, TCP SYN, TCP ACK, and IGMP). The target responded to several of them, but Nmap marked the host as alive based on the first valid response received, which was an ICMP Echo Reply._
{% endhint %}
