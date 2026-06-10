# Nmap

Nmap (Network Mapper) — open-source utility for network discovery and security auditing. It uses raw IP packets to determine available hosts, services, OS, firewalls, and hundreds of other characteristics.

## Syntax

```
nmap [Scan Type(s)] [Options] {target specification}
```

{% hint style="info" %}
_Nmap requires **root/sudo** for most scan types (SYN, OS detection, etc.) because they use raw sockets. Unprivileged users default to TCP connect scan (`-sT`)._
{% endhint %}

***

## Target Specification

Defines **what** to scan. Nmap accepts hosts in multiple formats.

<table><thead><tr><th width="145.800048828125">Method</th><th width="273.800048828125">Example</th><th>Description</th></tr></thead><tbody><tr><td>Single IP</td><td><code>192.168.16.131</code></td><td>One host</td></tr><tr><td>Multiple IPs</td><td><code>192.168.16.131 192.168.16.140</code></td><td>Space-separated</td></tr><tr><td>Range</td><td><code>192.168.16.130-140</code></td><td>Last octet range</td></tr><tr><td>CIDR</td><td><code>192.168.1.0/24</code></td><td>Entire subnet</td></tr><tr><td>From file</td><td><code>-iL targets.txt</code></td><td>One target per line</td></tr><tr><td>Exclude</td><td><code>--exclude 192.168.16.1</code></td><td>Skip specific hosts. generally used for skipping default gateways and network appliances. </td></tr><tr><td>Exclude file</td><td><code>--excludefile exclude.txt</code></td><td>Skip hosts listed in file</td></tr></tbody></table>

_⇒ We will see various types of target specifications in the scans performed later._

## Port Specification

Control **which ports** Nmap scans.

| Flag                   | Description                              |
| ---------------------- | ---------------------------------------- |
| `-p <range>`           | Specific ports or ranges                 |
| `-p-`                  | All 65535 ports                          |
| `-p U:53,T:80`         | UDP and TCP specific ports               |
| `--top-ports <n>`      | Scan top N most common ports             |
| `-F`                   | Fast scan (top 100 ports)                |
| `-r`                   | Scan ports sequentially (not randomized) |
| `--port-ratio <ratio>` | Scan ports with open frequency > ratio   |

***

## Nmap Default Scan&#x20;

#### Scan

{% code overflow="wrap" %}
```bash
nmap <IP/hostname>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (451).png" alt=""><figcaption></figcaption></figure>

### Analysis

Nmap performs roughly the following steps:

#### **Target Resolution**

* Determines whether the target is an IP address or hostname.
* If it's a hostname, performs DNS resolution

#### **Host Discovery**

* Checks if the target is alive.
* On a local network, it typically uses ARP requests.
* On remote networks, it may use ICMP, TCP, or other probes.

**Local Network**&#x20;

<figure><img src="../../../.gitbook/assets/image (452).png" alt=""><figcaption></figcaption></figure>

**Different network (8.8.8.8)**

<figure><img src="../../../.gitbook/assets/image (454).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (455).png" alt=""><figcaption></figcaption></figure>

> _Since `8.8.8.8` is a remote host, Nmap cannot use ARP. Instead, it sends multiple host discovery probes, including **ICMP Echo Requests (ping), TCP SYN ping (-PS), TCP ACK ping (-PA) and ICMP Timestamp request (-PP)**. The **ICMP Echo Reply** and **TCP SYN/ACK** responses confirm that the host is alive._

#### **Port Scanning**

* Scans the default **1,000 most common TCP ports using stealth scan (`-sS`)**
* Sends packets and analyzes responses to determine port states.

<figure><img src="../../../.gitbook/assets/image (453).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
&#x20;_This behavior is part of Nmap's default **SYN (stealth) scan**, which is discussed later._
{% endhint %}

## Port States

<table><thead><tr><th width="175">State</th><th>Meaning</th></tr></thead><tbody><tr><td><code>open</code></td><td>An application is actively listening on the port and accepting connections or packets.</td></tr><tr><td><code>closed</code></td><td>A closed port is accessible (it receives and responds to Nmap probe packet), but there is no application running on it or listening on it.</td></tr><tr><td><code>filtered</code></td><td>Nmap cannot determine whether port is open because packet filtering prevent its probes from reaching the port. The filter doesn’t respond and just drop the packet.</td></tr><tr><td><code>unfiltered</code></td><td>The port is reachable, but Nmap cannot determine whether it is open or closed (commonly seen with ACK scans).</td></tr><tr><td><code>open\|filtered</code></td><td>A port is placed in this state if or when it is unable to determine whether a port is open or filtered. It occurs when an open port give no response.</td></tr><tr><td><code>closed\|filtered</code></td><td>Nmap cannot determine whether the port is closed or filtered (rare; typically seen in Idle scans).</td></tr></tbody></table>

## Timing and Performance

Control scan speed. Higher timing = faster but noisier; lower = stealthier but slower.

### Timing Templates

<table><thead><tr><th width="95">Flag</th><th width="118">Name</th><th>Description</th></tr></thead><tbody><tr><td><code>-T0</code></td><td>Paranoid</td><td>Sends packets one at a time with very large delays (5 min between probes). Useless in modern network pentests. </td></tr><tr><td><code>-T1</code></td><td>Sneaky</td><td>Large delays (15 sec between probes). Attempt to avoid detection but scans can take hours or days. </td></tr><tr><td><code>-T2</code></td><td>Polite</td><td>Reduces scan rate to minimize impact on target/network. useful on fragile network. </td></tr><tr><td><code>-T3</code></td><td>Normal</td><td>Default, balanced</td></tr><tr><td><code>-T4</code></td><td>Aggressive</td><td>Reduces wait times and increases parallelism. Usually the best choice on stable LANs and modern networks. </td></tr><tr><td><code>-T5</code></td><td>Insane</td><td>Very aggresive timeouts and retransmission settings. May miss ports on slower or unstable network. </td></tr></tbody></table>

{% hint style="info" %}
_**`A`****&#x20;****(Aggressive Scan)** enables:_

* _**OS Detection** (`-O`)_
* _**Version Detection** (`-sV`)_
* _**Default NSE Scripts** (`-sC`)_
* _**Traceroute** (`--traceroute`)_

_It combines multiple reconnaissance techniques into a single scan to gather maximum information about the target._
{% endhint %}

```bash
# Stealthy scan
sudo nmap -T1 -sS 192.168.16.131

# Default scan
sudo nmap -T3 -sS 192.168.16.131

# Aggressive (CTF / labs)
sudo nmap -T4 -sS 192.168.16.131

# Insane (local network, need results fast)
sudo nmap -T5 -sS 192.168.16.131
```

### Granular Timing Controls

| Flag                                                   | Description                         |
| ------------------------------------------------------ | ----------------------------------- |
| `--min-hostgroup <size>`                               | Minimum hosts to scan in parallel   |
| `--max-hostgroup <size>`                               | Maximum hosts to scan in parallel   |
| `--min-parallelism <probes>`                           | Minimum outstanding probes          |
| `--max-parallelism <probes>`                           | Maximum outstanding probes          |
| `--min-rtt-timeout <ms>`                               | Minimum probe round-trip timeout    |
| `--max-rtt-timeout <ms>`                               | Maximum probe round-trip timeout    |
| `--initial-rtt-timeout <ms>`                           | Initial probe timeout               |
| `--max-retries <n>`                                    | Max number of probe retransmissions |
| `--host-timeout <ms>`                                  | Give up on a host after this time   |
| `--scan-delay <ms>`                                    | Minimum delay between probes        |
| `--max-scan-delay <ms>`                                | Maximum delay between probes        |
| <mark style="color:$danger;">`--min-rate 10000`</mark> | Send at least N packets per second  |
| `--max-rate <n>`                                       | Send at most N packets per second   |
| `--defeat-rst-ratelimit`                               | Ignore RST rate limiting by target  |

```bash
# Force minimum packet rate
sudo nmap --min-rate 10000 -sS 192.168.16.131

# Limit max rate to stay under radar
sudo nmap --max-rate 10000 -sS 192.168.16.131

# Scan delay for IDS evasion
sudo nmap --scan-delay 5s -sS 192.168.16.131
```

***

## Firewall / IDS Evasion and Spoofing

Techniques to bypass firewalls, IDS/IPS, and avoid detection.

| Flag                                 | Description                                       |
| ------------------------------------ | ------------------------------------------------- |
| `-f`                                 | Fragment packets (8 byte fragments)               |
| `-f -f`                              | 16 byte fragments                                 |
| `--mtu <n>`                          | Set custom MTU (must be multiple of 8)            |
| `-D <decoy1,decoy2,...>`             | Cloak scan with decoys                            |
| `-D RND:10`                          | Use 10 random decoys                              |
| `-S <IP>`                            | Spoof source address                              |
| `-e <iface>`                         | Use specified network interface                   |
| `-g <port>` / `--source-port <port>` | Use given source port                             |
| `--proxies <url1,url2,...>`          | Relay through HTTP/SOCKS4 proxies                 |
| `--data <hex>`                       | Append custom binary data to packets              |
| `--data-string <string>`             | Append custom ASCII string to packets             |
| `--data-length <n>`                  | Append random data of given length                |
| `--ip-options <options>`             | Send packets with specified IP options            |
| `--ttl <val>`                        | Set IP time-to-live field                         |
| `--spoof-mac <mac/vendor/0>`         | Spoof MAC address (0 = random)                    |
| `--badsum`                           | Send packets with bad checksums (detect firewall) |
| `--adler32`                          | Use old SCTP Adler32 checksums                    |

```bash
# Fragment packets to bypass basic firewalls
sudo nmap -f -sS 192.168.16.131

# Custom MTU
sudo nmap --mtu 24 -sS 192.168.16.131

# Decoy scan — mix your IP among decoys
sudo nmap -D 192.168.16.100,192.168.16.101,ME 192.168.16.131

# Random decoys
sudo nmap -D RND:5 192.168.16.131

# Spoof source port (53 = DNS, often allowed through firewalls)
sudo nmap -g 53 -sS 192.168.16.131

# Spoof MAC address
sudo nmap --spoof-mac 0 -sS 192.168.16.131

# Spoof MAC with vendor prefix
sudo nmap --spoof-mac Dell -sS 192.168.16.131

# Append random data to packets
sudo nmap --data-length 25 -sS 192.168.16.131

# Set TTL
sudo nmap --ttl 64 -sS 192.168.16.131

# Bad checksum (test if firewall checks checksums)
sudo nmap --badsum -sS 192.168.16.131

# Combine: fragment + decoys + source port + data padding
sudo nmap -f -D RND:3 -g 53 --data-length 50 -sS 192.168.16.131

# Idle scan (ultimate stealth — no packets from your IP)
sudo nmap -sI 192.168.16.140 192.168.16.131

# Use a specific interface
sudo nmap -e eth0 -sS 192.168.16.131
```

Spoofing (`-S`) requires `-e` and `-Pn` and you **won't see responses** (they go to the spoofed IP). Use idle scan (`-sI`) for practical spoofed scanning.

***

## Output Formats

Control how results are saved and displayed.

| Flag             | Description                                       |
| ---------------- | ------------------------------------------------- |
| `-oN <file>`     | Normal output (human-readable)                    |
| `-oX <file>`     | XML output                                        |
| `-oG <file>`     | Grepable output                                   |
| `-oA <basename>` | All 3 formats at once (`.nmap`, `.xml`, `.gnmap`) |
| `--open`         | Show only open ports                              |
| `--packet-trace` | Show all packets sent/received                    |

```bash
# Normal output
sudo nmap -sS 192.168.16.131 -oN scan_result.nmap

#

```

### Normal Output&#x20;

<figure><img src="../../../.gitbook/assets/image (544).png" alt=""><figcaption></figcaption></figure>

### Grepable Output

{% code overflow="wrap" %}
```bash
sudo nmap <scan config> -oG scan.gnmap
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (545).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# View alive hosts
grep "Status: Up" scan.gnmap | awk '{print $2}'

# Not much useful
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (546).png" alt=""><figcaption></figcaption></figure>

### XML Output

{% code overflow="wrap" %}
```bash
sudo nmap <scan config> -oX <filename> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (543).png" alt=""><figcaption></figcaption></figure>

Always use `-oA` to save in all formats. XML is especially useful — it can be imported into Metasploit (`db_import`), parsed with `xmllint`/`xsltproc`, or processed with tools like `searchsploit --nmap`.

| What You Want             | Command                             |
| ------------------------- | ----------------------------------- |
| Scan a single host        | `nmap 192.168.16.131`               |
| Scan and detect services  | `nmap -sV 192.168.16.131`           |
| Scan with default scripts | `nmap -sC 192.168.16.131`           |
| Aggressive full scan      | `nmap -A 192.168.16.131`            |
| Scan all ports            | `nmap -p- 192.168.16.131`           |
| Scan top 100 fast         | `nmap -F 192.168.16.131`            |
| OS detection              | `nmap -O 192.168.16.131`            |
| UDP scan                  | `nmap -sU 192.168.16.131`           |
| Stealth SYN scan          | `nmap -sS 192.168.16.131`           |
| Ping sweep                | `nmap -sn 192.168.16.0/24`          |
| Skip host discovery       | `nmap -Pn 192.168.16.131`           |
| Vulnerability scan        | `nmap --script vuln 192.168.16.131` |
| Save all output formats   | `nmap -oA output 192.168.16.131`    |
| Show only open ports      | `nmap --open 192.168.16.131`        |
| Fragment packets          | `nmap -f 192.168.16.131`            |
| Use decoys                | `nmap -D RND:5 192.168.16.131`      |
