# SNMP (161)

## Introduction to SNMP

**SNMP (Simple Network Management Protocol)** is an **Application Layer protocol** used to **monitor, manage, and collect information from network-connected devices** over an IP network.

It allows an administrator to remotely retrieve information (and, if permitted, modify certain configuration values) from devices such as servers, routers, switches, printers, firewalls, and IoT devices.

### Why is SNMP used?

Imagine a company with **500 switches, 300 routers, and 100 Linux servers**.

Instead of logging into each device individually to check:

* CPU usage
* Memory usage
* Disk usage
* Network interfaces
* Uptime
* Running processes
* Temperature

the administrator simply asks each device through SNMP.

```
            Administrator

                  │
      Collect Information
                  │
    ┌─────────────┴─────────────┐
    │             │             │
 Router         Switch       Linux Server
    │             │             │
 CPU Usage    Port Status    Disk Usage
 Memory       Traffic        Running Processes
 Uptime       Errors         Network Interfaces
```

### Information SNMP can provide

Depending on the device configuration, SNMP can provide information such as:

* System name
* Hostname
* Operating System
* Device uptime
* CPU utilization
* Memory usage
* Disk usage
* Running processes
* Network interfaces
* Interface statistics
* Routing table
* ARP table
* Installed software
* System description

### SNMP Communication Model&#x20;

```
         SNMP Manager
    (Monitoring System)
           │
 Requests Information
           │
    UDP Port 161
           │
           ▼
      SNMP Agent
  (Running on Device)
           │
  Reads Local System Data
           │
           ▼
     Managed Device
```

## SNMP Architecture&#x20;

SNMP follows a **manager-agent architecture**.

Instead of logging into every device, a central system called the **SNMP Manager** communicates with software called the **SNMP Agent** running on each device.

{% code overflow="wrap" %}
```
                     SNMP Manager
               (Monitoring Software)
                        │
         Request / Response (UDP 161)
                        │
        ┌───────────────┴────────────────┐
        │               │                │
   Linux Server      Router          Switch
        │               │                │
    SNMP Agent      SNMP Agent      SNMP Agent
        │               │                │
     System Data     Interface Data    Port Data
```
{% endcode %}

The architecture consists of **five main components**:

* SNMP Manager
* SNMP Agent
* Managed Device
* MIB (Management Information Base)
* OID (Object Identifier)

### 1. SNMP Manager

The **SNMP Manager** is the application that communicates with one or more SNMP agents to monitor or manage devices.

Think of it as the **administrator** of the SNMP environment.

#### Responsibilities

* Sends SNMP requests
* Receives responses
* Stores collected information
* Displays monitoring statistics
* Receives traps and informs

### 2. SNMP Agent

An **SNMP Agent** is software running on the managed device.

Its job is to:

* Collect local system information
* Maintain management data
* Respond to SNMP requests
* Send notifications (Traps/Informs)

Think of the agent as a **translator** between the operating system and the SNMP manager.

### 3. Managed Device

A **Managed Device** is any device running an SNMP agent.

Examples include:

* Linux Server
* Windows Server
* Router
* Switch
* Firewall
* Printer
* UPS
* NAS
* IP Camera

### 4. Management Information Base (MIB)

A **MIB is a database (or dictionary) that defines what information can be accessed through SNMP and how that information is organized.**

It **does not store the actual values**. Instead, it defines:

* What objects exist
* Their names
* Their data types
* Their Object Identifiers (OIDs)

Think of it like a **schema or blueprint**.

{% code overflow="wrap" %}
```
MIB

System Name
↓

OID = 1.3.6.1.2.1.1.5

Type = STRING

Description = Device Hostname
```
{% endcode %}

### 5. Object Identifier (OID)

Every piece of information in SNMP has a unique **Object Identifier (OID)**.

An OID is like the **address** of a specific management object.

Example:

```
System Name

↓

OID

1.3.6.1.2.1.1.5.0
```

When the manager wants the hostname, it asks:

> "Give me the value stored at OID `1.3.6.1.2.1.1.5.0`."

The agent responds:

```
Ubuntu-Server
```

Another example:

```
System Uptime

↓

OID

1.3.6.1.2.1.1.3.0
```

### Complete Architecture&#x20;

{% code overflow="wrap" %}
```
                  SNMP Manager
                     (Kali)
                        │
               Request an OID
                        │
                        ▼
             OID: 1.3.6.1.2.1.1.5.0
                        │
                        ▼
                 SNMP Agent (snmpd)
                        │
            Looks up the MIB definition
                        │
            Retrieves current hostname
                        │
                        ▼
              Returns "Ubuntu-Server"
```
{% endcode %}

* **MIB** = Defines what objects exist and their OIDs.
* **OID** = The unique identifier used to request a specific object.
* **SNMP Agent** = Retrieves the current value for that OID from the system.

## How SNMP Works&#x20;

SNMP works using a simple **request-response model**.

The **SNMP Manager** requests information from an **SNMP Agent**, and the agent retrieves the requested data from the operating system before sending it back.

{% code overflow="wrap" %}
```
SNMP Manager
      │
      │ Request OID
      ▼
SNMP Agent
      │
      │ Lookup OID in MIB
      ▼
MIB
      │
      │ Object = Hostname
      ▼
Operating System
      │
      │ Read Current Value
      ▼
SNMP Agent
      │
      │ Response
      ▼
SNMP Manager
```
{% endcode %}

## SNMP Versions&#x20;

Over time, SNMP evolved to improve **performance** and **security**.

There are **three major versions**:

* SNMPv1
* SNMPv2c
* SNMPv3

### 1. SNMPv1

SNMPv1 is the **original version** of SNMP.

It provides basic monitoring and management capabilities but offers **very limited security**.

Authentication is performed using a **Community String**, which is sent in **plain text** over the network.

```
Manager
    │
Community String: public
    │
    ▼
SNMP Agent
```

Anyone who captures the network traffic can read the community string.

#### Features

* Basic GET and SET operations
* Uses Community Strings
* No encryption
* No integrity protection
* UDP transport

#### Advantages

* Simple
* Lightweight
* Supported by almost every device

#### Limitations

* Plain-text authentication
* No encryption
* No message integrity
* Weak security

### 2. SNMPv2c

SNMPv2 introduced several improvements.

The most commonly deployed variant is **SNMPv2c**, where **"c" stands for Community-based**.

It **still uses Community Strings**, so security is almost the same as SNMPv1.

The major improvements are in **performance and protocol capabilities**, not authentication.

#### New Features

* GETBULK operation
* Better error handling
* More efficient data retrieval
* Improved protocol operations

Authentication remains:

```
Community String

↓

Plain Text
```

#### Advantages

* Faster than SNMPv1
* Supports bulk retrieval
* Better performance on large networks
* Widely deployed

#### Limitations

* Community Strings remain unencrypted
* No confidentiality
* No integrity protection

### 3. SNMPv3

SNMPv3 focuses on **security**.

Instead of relying only on Community Strings, it introduces:

* User-based authentication
* Message integrity
* Encryption
* Access control

Authentication can use algorithms such as:

* MD5
* SHA

Encryption can use:

* DES
* AES

Communication becomes:

```
Manager
      │
Authentication
Encryption
Integrity Check
      │
      ▼
Agent
```

Now even if someone captures the packets, the management data and credentials are protected.

#### Features

* Authentication
* Encryption
* Message integrity
* User accounts
* Access control

#### Advantages

* Secure authentication
* Confidential communication
* Data integrity verification
* Recommended for modern deployments

#### Limitations

* More complex configuration
* Slightly higher overhead

## SNMP Lab&#x20;

We'll build a small SNMP lab with **Ubuntu (Agent)** and **Kali (Manager)**.

```
We'll build a small SNMP lab with Ubuntu (Agent) and Kali (Manager).

           Kali Linux
        (SNMP Manager)
               │
         UDP 161 / 162
               │
               ▼
          Ubuntu Server
           (SNMP Agent)
```

### Install SNMP Packages&#x20;

#### On Ubuntu

Install the SNMP agent (`snmpd`) and SNMP utilities (`snmp`).

```
sudo apt update

sudo apt install snmp snmpd
```

#### Verify Installation&#x20;

{% code overflow="wrap" %}
```
snmpd -v
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (931).png" alt=""><figcaption></figcaption></figure>

### Important Packages&#x20;

| Package | Purpose               |
| ------- | --------------------- |
| `snmpd` | SNMP Agent (Daemon)   |
| `snmp`  | SNMP Client Utilities |

### Important Configuration Files&#x20;

| File                    | Purpose                       |
| ----------------------- | ----------------------------- |
| `/etc/snmp/snmpd.conf`  | Main SNMP Agent configuration |
| `/etc/snmp/snmp.conf`   | Client configuration          |
| `/usr/share/snmp/mibs/` | MIB definition files          |

### Backup Configuration&#x20;

Before modifying&#x20;

{% code overflow="wrap" %}
```
sudo cp /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.bak
```
{% endcode %}

### Configuring SNMPv2c (Read-only)

By default, Ubuntu's `snmpd` is configured to listen only on localhost and expose very little information.

We'll modify it so another machine in our lab (Kali) can communicate with it.

#### Edit Configuration file&#x20;

By default you'll find something similar to:

```
agentAddress 127.0.0.1,[::1]
```

This means:

> Accept SNMP requests **only from localhost**.

Change it to:

```
agentAddress udp:161
```

or

```
agentAddress udp:0.0.0.0:161
```

Now the SNMP agent listens on all network interfaces.

<figure><img src="../../../.gitbook/assets/image (932).png" alt=""><figcaption></figcaption></figure>

**Add** `rocommunity public` text if not present in the file.&#x20;

This creates a read-only SNMPv2c community named `public`.

<figure><img src="../../../.gitbook/assets/image (933).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_`default` in the above line means by allow connections from any source IP._&#x20;

* _`rocommunity ...` tells where clients are allowed to access the SNMP._&#x20;
  * _`public` is a community string that clients must present. administrator can change it to anything else as per requirement. But default is public._&#x20;
* _`agentaddress ...` make SNMP listens on which network interface._&#x20;
{% endhint %}

#### Restart the service&#x20;

{% code overflow="wrap" %}
```bash
sudo systemctl start snmpd
```
{% endcode %}

### Important MIB Unset (Optional)

{% hint style="info" %}
_If you don't do this then you've to query the SNMP using the numeric OID's as done in this notes._&#x20;
{% endhint %}

Check your configuration:

```
grep '^mibs' /etc/snmp/snmp.conf
```

If it contains:

```
mibs :
```

then MIB loading has been intentionally disabled (this is the default on many Debian-based systems, including Kali).

You can either:

* keep using numeric OIDs (common in pentesting), or
* enable/install MIBs if you prefer readable names while learning by commenting the mibs.&#x20;

<figure><img src="../../../.gitbook/assets/image (941).png" alt=""><figcaption></figcaption></figure>

#### Install the MIBs

{% code overflow="wrap" %}
```bash
sudo apt update
sudo apt install snmp snmp-mibs-downloader
```
{% endcode %}

### Configuring SNMPv3

Unlike SNMPv1 and SNMPv2c, **SNMPv3 no longer relies on community strings for authentication**.

Instead, it uses:

* **Username**
* **Authentication**
* **Encryption (optional but recommended)**

#### Three Security level of SNMPv3

| Security Level   | Authentication | Encryption (Privacy) | Description                                                                                 | Pentester Note                                                                                  |
| ---------------- | -------------- | -------------------- | ------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **noAuthNoPriv** | ❌ No           | ❌ No                 | No authentication and no encryption. Anyone who knows the SNMPv3 username can communicate.  | Weakest SNMPv3 mode; data is sent in plaintext.                                                 |
| **authNoPriv**   | ✅ Yes          | ❌ No                 | Authenticates the user using a password (e.g., MD5, SHA), but traffic is **not encrypted**. | Credentials are verified, but SNMP data can still be sniffed.                                   |
| **authPriv**     | ✅ Yes          | ✅ Yes                | Authenticates the user and encrypts SNMP traffic (e.g., AES, DES).                          | Most secure and recommended SNMPv3 mode; protects both authentication and data confidentiality. |

#### Stop the service&#x20;

{% code overflow="wrap" %}
```
sudo systemctl stop snmpd
```
{% endcode %}

#### Create an SNMPv3 User&#x20;

{% code overflow="wrap" %}
```
sudo net-snmp-create-v3-user
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (945).png" alt=""><figcaption></figcaption></figure>
