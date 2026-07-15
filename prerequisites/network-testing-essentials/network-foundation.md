# Network Foundation

## Networking Fundamentals&#x20;

### What is a Network ?&#x20;

A **computer network** is a collection of interconnected devices (hosts) that communicate and exchange data using standardized communication protocols over wired or wireless transmission media.

A network enables:

* Data sharing
* Resource sharing (printers, storage, internet)
* Communication between users and applications
* Centralized management and security (if installed)

<figure><img src="../../.gitbook/assets/Gemini_Generated_Image_dgmh3ddgmh3ddgmh.png" alt=""><figcaption></figcaption></figure>

### Types of Network&#x20;

<figure><img src="../../.gitbook/assets/Gemini_Generated_Image_75b0vl75b0vl75b0.png" alt=""><figcaption></figcaption></figure>

| Feature                    | PAN (Personal Area Network)                                                               | LAN (Local Area Network)                                                               | MAN (Metropolitan Area Network)                                                            | WAN (Wide Area Network)                                                                              |
| -------------------------- | ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| Geographic Scope           | Very small; centered around a single person, typically within \~10 meters (e.g., a room). | Small; limited to a single building, a home, an office, or a small group of buildings. | Medium; covers a specific geographic area like a city, town, or a large university campus. | Very large; covers countries, continents, or the entire globe (e.g., the Internet).                  |
| Ownership                  | Usually a single individual or household.                                                 | Typically owned and managed by a single organization, office, or individual.           | Often owned by a consortium of users or a single network provider (like an ISP).           | Owned and managed by multiple telecommunications providers and organizations.                        |
| Data Transfer Speed        | Can be moderate to high, but often optimized for low-power (e.g., for wearables).         | Very high (e.g., gigabit or multi-gigabit speeds), with low latency.                   | High, typically using powerful backbones like fiber optics across a city.                  | Varied, often lower than LANs due to vast distances and differing technologies, with higher latency. |
| Typical Transmission Media | Often wireless (Bluetooth, NFC), but can be wired (USB).                                  | Wired (Ethernet cables like Cat 6/6a) and wireless (Wi-Fi).                            | Mostly wired (fiber optic cables) with some fixed wireless options.                        | Diverse, including fiber optics, satellite links, and leased telecommunications lines.               |
| Typical Cost               | Very low cost.                                                                            | Low to moderate cost.                                                                  | Moderate to high cost.                                                                     | Highest cost.                                                                                        |
| Examples                   | Bluetooth headphones connected to a phone; a laptop syncing to a tablet.                  | A home network; an office LAN; a university computer lab.                              | A city's cable television network that provides Internet; a university campus network.     | The Internet (the largest WAN); a large corporation's global network.                                |

### Network Topologies&#x20;

* Network topology is the arrangement of devices (nodes) and connections (links) in a computer network.&#x20;
* It shows how computers, servers, and other devices are connected and how data flows between them.

<figure><img src="../../.gitbook/assets/image (1103).png" alt=""><figcaption></figcaption></figure>

## OSI Model&#x20;

The **OSI (Open Systems Interconnection) model** is a universal conceptual framework created by the International Organization for Standardization (ISO) that divides network communications into seven abstract layers. It standardizes how different hardware and software systems interact and share data across a network.

### Layer 1: The Physical Layer (Bits)

<figure><img src="../../.gitbook/assets/image (1104).png" alt=""><figcaption></figcaption></figure>

This is the very first and lowest layer. Its job is to handle the physical transmission of data bits over a specific communication medium.&#x20;

{% hint style="info" %}
_It just deals with raw ones and zeros._
{% endhint %}

* **Core Responsibility:** It manages the hardware-level transmission of individual bits. When receiving data, it translates incoming physical signals back into digital 0s and 1s to be processed by the Data Link layer.
* **Essential Hardware:** Common devices operating at this level include Hubs, Repeaters, Modems, and Cables.
* **Bit Synchronization:** It ensures the sender and receiver are "in sync" by providing a clock signal that controls the timing of bit transmission.
* **Bit Rate Control:** It dictates the transmission speed, defined as the number of bits sent per second.
* **Physical Topologies:** It defines the structural layout of the network, such as Bus, Star, or Mesh configurations.
* **Transmission Mode:** It determines the direction of data flow, utilizing modes like Simplex (one-way), Half-Duplex (two-way, one at a time), or Full-Duplex (simultaneous two-way).

### Layer - 2: The Data Link Layer (Frames)

This layer sits above the Physical Layer.

* Responsible for the node-to-node delivery of data within the same local network.
* Major role is to ensure error-free transmission of information.
* Also responsible for encoding, decoding, and organizing the outgoing and incoming data.
* Considered as the most complex layer of the OSI model as it hides all the underlying complexities of the hardware from the other above layers.&#x20;

#### Functions of Data Link Layer

**1. Data Packaging (Framing):** At this layer, data packets received from the Network layer are divided into manageable units called Frames. This is done by adding specific "start" and "stop" bits so the receiver can recognize where one unit of data ends and the next begins.

**2. Addressing and Hardware:** The DLL uses **MAC addresses** (Physical Addressing) to identify hosts. It encapsulates the sender’s and receiver’s MAC addresses into the frame header. Common devices operating here are **Switches and Bridges**.

**3. Sublayers:**

* **Logical Link Control (LLC):** Manages flow and error control while acting as an interface for upper-layer protocols.
* **Media Access Control (MAC):** Determines how devices physically access the transmission medium and handles hardware addressing.

**4. Error Control:** It detects and retransmits damaged or lost frames to maintain data integrity.

**5. Flow Control:** It synchronizes the data rate between a fast sender and a slower receiver to prevent data "bottlenecks" or loss.

**6. Access Control:** When multiple devices share the same communication channel, the MAC sublayer dictates which device has the right to transmit at any given time to avoid collisions.

<figure><img src="../../.gitbook/assets/image (1105).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Framing" %}
<figure><img src="../../.gitbook/assets/image (1106).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Error Detection" %}
<figure><img src="../../.gitbook/assets/image (1107).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Error Correction" %}
<figure><img src="../../.gitbook/assets/image (1108).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Flow Control" %}
<figure><img src="../../.gitbook/assets/image (1109).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Addressing" %}
<figure><img src="../../.gitbook/assets/image (1110).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Layer - 3: The Network Layer (Packets)

* While Layer 2 handles communication within a local network, the Network Layer is what allows networks to connect to _other networks_.&#x20;
* Its main job is routing data from the source host to the destination host, even if they are located on completely different network segments.

#### Functions of Network Layer&#x20;

* **Data Unit:** Data segments are encapsulated into Packets.
* **Logical Addressing:** Assigns unique **IP addresses** (sender and receiver) to the packet header to identify devices globally.
* **Routing:** Determines the most efficient physical path from the source to the destination across interconnected networks.
* **Hardware:** Primarily implemented via **Routers and Switches**.
* **Inter-networking:** Facilitates communication between disparate networks by directing traffic to the correct destination.
* **Fragmentation and Reassembly**: Splits large packets into smaller fragments to match the maximum transmission unit (MTU) of a network, and reassembles them at the destination.
* **Subnetting**: Divides larger networks into smaller subnetworks for efficient addressing and traffic management.
* **Network Address Translation (NAT)**: Maps private IPs to public IPs for internet communication, conserving address space and adding security.
* **Protocols operating at Network Layer:** IP, ICMP, ARP, RARP, NAT, IPSec.&#x20;
* **Routing Protocol:** RIP, OSPF, BGP

<figure><img src="../../.gitbook/assets/image (1111).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="IP Addressing" %}
<figure><img src="../../.gitbook/assets/image (1112).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Packetizing" %}
<figure><img src="../../.gitbook/assets/image (1113).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Host-to-Host Delivery" %}
<figure><img src="../../.gitbook/assets/image (1114).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Subnetting" %}
<figure><img src="../../.gitbook/assets/image (1115).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Network Address Translation" %}
<figure><img src="../../.gitbook/assets/image (1116).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Routing" %}
<figure><img src="../../.gitbook/assets/image (1117).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### Layer - 4: The Transport Layer (Segments)

<figure><img src="../../.gitbook/assets/image (1118).png" alt=""><figcaption></figcaption></figure>

The Transport Layer manages the complete, end-to-end delivery of data. Its primary role is to process data from the upper layers and prepare it for transmission over the network, ensuring it gets to the correct process on the destination host in the right format.

#### Functions of Network Layer

* **Data Unit:** Data is broken down into Segments.
* **Service Point Addressing:** Uses Port Numbers (e.g., Port 80 for web traffic) to ensure data is delivered to the specific process or application intended, not just the device.
* **Segmentation & Reassembly:** Splits large messages into smaller segments for transmission and reassembles them in the correct order at the destination.
* **Protocols:** Common protocols include **TCP** (reliable), **UDP** (fast).
* **Connection Control**
  * **Connection-Oriented (TCP):** Requires a "handshake" to establish a connection; ensures reliability via error checking and acknowledgements.
    * 3-way handshake explained further.&#x20;
  * **Connectionless (UDP):** Sends data immediately without a formal connection; faster but offers no guarantee of delivery.

### Layer - 5: The Session Layer (Data)

<figure><img src="../../.gitbook/assets/image (1119).png" alt=""><figcaption></figcaption></figure>

* This layer manages the overall session or dialogue between two communicating applications.&#x20;
* It establishes, maintains, and coordinates the communication stream (session), deciding things like which application can talk when and how to synchronize the exchange.

#### Functions of Session Layer

* **Session Lifecycle:** Manages the establishment, maintenance, and termination of connections between applications.
* **Authentication & Security:** Ensures that the communicating parties are verified and the connection is secure.
* **Synchronization:** Inserts checkpoints into the data stream. This allows a transfer to resume from the last saved point if a failure occurs, rather than restarting from the beginning.
* **Dialog Control:** Directs whether communication is half-duplex (alternating) or full-duplex (simultaneous).
* **Practical Example:** In a web-based messenger, the Session Layer maintains the active link between your browser and the server, ensuring your specific chat remains open and synchronized while handling background encryption and data conversion.

### Layer 6: The Presentation Layer (Data)

The Presentation Layer acts as a "translator." It ensures that the data being sent from the source application is received in a format that the destination application can understand. It handles aspects of data representation, such as syntax and semantics.

#### Functions of Presentation Layer

* **Translation:** It extracts data from the Application Layer and converts it into a standardized format for network transmission, bridging differences in data representation between different systems.
* **Standards & Formats:** Handles the encoding of media using standards such as JPEG, MPEG, and GIF.
* **Encryption/Decryption:** Provides security by converting "plain text" into "ciphertext" (encryption) and back again (decryption) using key values. This process is typically handled by protocols like **TLS/SSL**.
* **Compression:** Reduces the total number of bits required for transmission, which increases network efficiency and speed.

### Layer 7: The Application Layer (Data)&#x20;

This is the top layer of the OSI model and the one that is closest to you, the user. The Application Layer provides network services directly to applications like your web browser, email client, or file transfer utility.

#### Functions of Application Layer&#x20;

* **User Interface:** Acts as a "window" for network-based applications (like web browsers or email clients) to access network services.
* **Core Protocols:** Utilizes high-level protocols such as [**HTTP/S**](https://www.geeksforgeeks.org/computer-networks/difference-between-http-and-https-2/) (Web), [**SMTP**](https://www.geeksforgeeks.org/computer-networks/simple-mail-transfer-protocol-smtp/) (Email), [**FTP**](https://www.geeksforgeeks.org/computer-networks/file-transfer-protocol-ftp-in-application-layer/) (File Transfer), and [**DNS**](https://www.geeksforgeeks.org/computer-networks/domain-name-system-dns-in-application-layer/) (Domain Name Resolution).
* **Network Virtual Terminal (NVT):** Enables users to log into and interact with remote hosts as if they were physically present at the terminal.

## TCP/IP Model

The **TCP/IP** (Transmission Control Protocol/Internet Protocol) model is the practical, real-world framework that powers the internet. Related to the **OSI** (Open Systems Interconnection) reference model, TCP/IP essentially condenses OSI's 7 abstract layers into 4 functional layers, specifically mapping theoretical concepts to usable protocols.&#x20;

<figure><img src="../../.gitbook/assets/image (1120).png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="141">TCP/IP Model (4 Layers)</th><th width="195.79998779296875">OSI Model Equivalent (7 Layers)</th><th>What It Does</th></tr></thead><tbody><tr><td>1. Application</td><td>Application, Presentation, Session (Layers 7, 6, 5)</td><td>Handles user interfaces, data formatting (encryption/compression), and maintaining communication sessions (e.g., HTTP, DNS).</td></tr><tr><td>2. Transport</td><td>Transport (Layer 4)</td><td>Manages end-to-end communication, ensuring data arrives reliably and in order (e.g., TCP, UDP).</td></tr><tr><td>3. Internet</td><td>Network (Layer 3)</td><td>Handles logical addressing and routes packets across different networks to their destination (e.g., IP).</td></tr><tr><td>4. Network Access</td><td>Data Link, Physical (Layers 2, 1)</td><td>Deals with hardware addressing (MAC addresses), framing data, and the actual physical transmission of bits over cables or radio waves (e.g., Ethernet, Wi-Fi).</td></tr></tbody></table>

## TCP V/s UDP&#x20;

| TCP (Transmission Control Protocol)                                                           | UDP (User Datagram Protocol)                                                                      |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| TCP is a connection-oriented protocol that establishes a connection before data transmission. | UDP is a connectionless protocol that sends data without establishing a connection.               |
| TCP provides reliable data delivery by using acknowledgments and retransmissions.             | UDP does not guarantee reliable delivery and does not retransmit lost packets.                    |
| TCP ensures packets are received in the correct order.                                        | UDP does not guarantee packet ordering.                                                           |
| TCP performs error checking and recovery to maintain data integrity.                          | UDP performs basic error checking but does not provide error recovery.                            |
| TCP has higher overhead due to connection management and reliability mechanisms.              | UDP has lower overhead because it uses a simpler communication process.                           |
| TCP is generally slower because of acknowledgments, flow control, and retransmissions.        | UDP is generally faster because it sends data without waiting for acknowledgments.                |
| TCP uses flow control and congestion control mechanisms to manage network traffic.            | UDP does not provide flow control or congestion control.                                          |
| TCP is suitable for applications where accuracy and reliability are critical.                 | UDP is suitable for applications where speed and low latency are more important than reliability. |
| TCP is commonly used for web browsing (HTTP/HTTPS), email, and file transfers.                | UDP is commonly used for video streaming, VoIP, online gaming, and DNS queries.                   |
| TCP retransmits lost packets to ensure complete data delivery.                                | UDP does not retransmit lost packets, which may result in data loss.                              |
| TCP requires more processing resources due to its complex features.                           | UDP requires fewer processing resources because of its lightweight design.                        |
| TCP uses a three-way handshake to establish a connection.                                     | UDP does not use any handshake mechanism before transmitting data.                                |

## TCP Three-Way handshake&#x20;

The TCP Three-Way Handshake is the process used to establish a reliable connection between a client and a server before data transmission begins.

<figure><img src="../../.gitbook/assets/image (1121).png" alt=""><figcaption></figcaption></figure>

#### Step 1: SYN (Synchronize)

* The client initiates the connection by sending a **SYN** packet to the server.
* This packet contains the client's **Initial Sequence Number (ISN)**.
* The SYN flag is set to **1**.
* The client enters the **SYN-SENT** state.

#### Step 2: SYN-ACK (Synchronize-Acknowledge)

* The server receives the SYN packet.
* The server acknowledges the client's sequence number by sending a **SYN-ACK** packet.
* The ACK number is set to the client's sequence number plus one.
* The server also sends its own Initial Sequence Number.
* The server enters the **SYN-RECEIVED** state.

#### Step 3: ACK (Acknowledge)

* The client receives the SYN-ACK packet.
* The client sends an **ACK** packet to acknowledge the server's sequence number.
* The ACK number is set to the server's sequence number plus one.
* Both client and server transition to the **ESTABLISHED** state.
* Data transfer can now begin.

## TCP Connection Termination&#x20;

<figure><img src="../../.gitbook/assets/image (1122).png" alt=""><figcaption></figcaption></figure>

#### Step 1: FIN (Finish)

* The client decides to close the connection.
* It sends a **FIN** packet to the server.
* This indicates that the client wants to terminate the connection.&#x20;
* The client enters the **FIN-WAIT-1** state.

#### Step 2: ACK (Acknowledge)

* The server receives the FIN packet.
* It acknowledges the FIN by sending an **ACK** packet.
* The server may still have data to send.
* The server enters the **CLOSE-WAIT** state.
* The client enters the **FIN-WAIT-2** state.

#### Step 3: FIN (Finish)

* After completing its remaining data transmission, the server sends its own **FIN** packet.
* This indicates that the server has also finished sending data.
* The server enters the **LAST-ACK** state.

#### Step 4: ACK (Acknowledge)

* The client receives the server's FIN packet.
* It sends a final **ACK** packet.
* The client enters the **TIME-WAIT** state.
* The server closes the connection and enters the **CLOSED** state.

## DHCP

**DHCP (Dynamic Host Configuration Protocol)** is a protocol used to automatically assign network configuration settings to devices, such as:

* IP Address
* Subnet Mask
* Default Gateway
* DNS Server
* Lease Duration

Without DHCP, network administrators would need to configure IP addresses manually on every device.

### DORA Process

<figure><img src="../../.gitbook/assets/image (1123).png" alt=""><figcaption></figcaption></figure>

#### Step 1: DHCP Discover

* When a device joins a network, it does not have an IP address.
* The client initiates communication by sending a **DHCP Discover** message.
* This message is sent as a **broadcast** because the client does not know the DHCP server's IP address.
* The source IP address is **0.0.0.0** and the destination IP address is **255.255.255.255**.
* The Discover packet contains the client's MAC address so that DHCP servers can identify the requesting device.
* The purpose of this message is to locate one or more DHCP servers available on the network.

#### Step 2: DHCP Offer

* One or more DHCP servers receive the Discover message.
* Each DHCP server checks its available IP address pool (scope).
* The server selects an unused IP address and prepares a DHCP Offer message.
* The Offer includes:
  * Proposed IP address
  * Subnet mask
  * Default gateway
  * DNS server information
  * Lease duration
* The DHCP server sends the Offer back to the client.
* Multiple DHCP servers may send different offers if more than one server exists on the network.

#### Step 3: DHCP Request

* The client receives one or more DHCP Offers.
* The client chooses one of the offered IP addresses.
* The client sends a **DHCP Request** message to indicate which offer it has accepted.
* This Request is usually sent as a broadcast so all DHCP servers can see the decision.
* The selected DHCP server reserves the requested IP address for the client.
* Other DHCP servers withdraw their offers and return those IP addresses to their available pools.
* This step formally requests the lease of the offered IP address.

#### Step 4: DHCP Acknowledge (ACK)

* The selected DHCP server receives the DHCP Request.
* The server verifies that the requested IP address is still available.
* The server creates a lease entry associating the client's MAC address with the assigned IP address.
* The server sends a **DHCP ACK** message to confirm the lease.
* The ACK contains all final network configuration details:
  * IP address
  * Subnet mask
  * Default gateway
  * DNS server
  * Lease duration
* Upon receiving the ACK, the client configures its network interface with the assigned settings.
* The client can now communicate with other devices and access network resources.

## IPv4 Packet Structure&#x20;

IP stands for Internet Protocol, and IPv4 refers to Internet Protocol Version 4. IPv4 was the first widely deployed version of the Internet Protocol and was introduced for use in the ARPANET in 1983. An IPv4 address is a 32-bit numeric value, which is represented in dotted decimal notation (for example, 192.168.1.1). IPv4 is responsible for logical addressing and routing of packets across networks.

<figure><img src="../../.gitbook/assets/image (1124).png" alt=""><figcaption></figcaption></figure>

* **VERSION**_**:**_ Version of the IP protocol (4 bits), which is 4 for IPv4&#x20;
* **HLEN:** IP header length (4 bits), which is the number of 32 bit words in the header. The minimum value for this field is 5 and the maximum is 15.&#x20;
* **Type of service:** Low Delay, High Throughput, Reliability (8 bits)&#x20;
* **Total Length:** Length of header + Data (16 bits), which has a minimum value 20 bytes and the maximum is 65,535 bytes.&#x20;
* **Identification**_**:**_ Unique Packet Id for identifying the group of fragments of a single IP datagram (16 bits)&#x20;
* **Flags**_**:**_ 3 flags of 1 bit each : reserved bit (must be zero), do not fragment flag, more fragments flag (same order)&#x20;
* **Fragment Offset:** Represents the number of Data Bytes ahead of the particular fragment in the particular Datagram. Specified in terms of number of 8 bytes, which has the maximum value of 65,528 bytes.&#x20;
* **Time to live**_**:**_ Datagram’s lifetime (8 bits), It prevents the datagram to loop through the network by restricting the number of Hops taken by a Packet before delivering to the Destination.
* **Protocol:** Name of the protocol to which the data is to be passed (8 bits)&#x20;
* **Header Checksum**_**:**_ 16 bits header [checksum](https://www.geeksforgeeks.org/computer-networks/error-detection-code-checksum/) for checking errors in the datagram header&#x20;
* **Source IP address**_**:**_ 32 bits IP address of the sender&#x20;
* **Destination IP address**_**:**_ 32 bits[ IP address ](https://www.geeksforgeeks.org/computer-science-fundamentals/what-is-an-ip-address/)of the receiver&#x20;
* **Option:** Optional information such as source route, record route. Used by the Network administrator to check whether a path is working or not.
