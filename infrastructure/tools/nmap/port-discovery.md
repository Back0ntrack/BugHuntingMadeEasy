# Port Discovery

**Determines** the attack surface.&#x20;

<figure><img src="../../../.gitbook/assets/Gemini_Generated_Image_5nbf9j5nbf9j5nbf.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_**We use****&#x20;****`-Pn`****&#x20;****because the host has already been confirmed as alive, and****&#x20;****`-n`****&#x20;****to disable DNS resolution.**_
{% endhint %}

## TCP Connect Scan (-sT)

Uses the OS `connect()` system call — completes the full TCP handshake. Default for unprivileged users. Slower and more detectable than SYN.

```bash
nmap -sT <IP> -Pn -n --reason
```

#### Scan

<figure><img src="../../../.gitbook/assets/image (496).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (497).png" alt=""><figcaption></figcaption></figure>

## TCP SYN Scan (Half-Open / Stealth Scan)&#x20;

**Default scan when running as root.** Sends SYN → receives SYN/ACK (open) or RST (closed). Never completes the 3-way handshake → stealthier than connect scan.

```bash
sudo nmap -sS <IP> -Pn -n --reason
```

#### Scan

<figure><img src="../../../.gitbook/assets/image (498).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (499).png" alt=""><figcaption></figcaption></figure>

## UDP Scan

Scans UDP ports. Sends empty UDP packets (or protocol-specific payloads). Slow because open/filtered ports rarely respond.

```bash
sudo nmap -sU -Pn -n <IP> --reason 
```

#### Scan

<figure><img src="../../../.gitbook/assets/image (500).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (501).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_UDP scans are extremely slow. Limit with `-p` and use `--top-ports` or combine with `--version-intensity` to speed up._
{% endhint %}

## TCP ACK Scan

{% hint style="warning" %}
_Does **not** determine open/closed — used to map **firewall rulesets**. Determines whether ports are filtered or unfiltered (detect presence of firewall)._
{% endhint %}

{% code overflow="wrap" %}
```bash
sudo nmap -sA -Pn -n <ip> -p 445 --reason 
```
{% endcode %}

### Unfiltered (firewall off)

#### Scan

<figure><img src="../../../.gitbook/assets/image (502).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (503).png" alt=""><figcaption></figcaption></figure>

### Filtered (Firewall on)

#### Scan

<figure><img src="../../../.gitbook/assets/image (504).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (505).png" alt=""><figcaption></figcaption></figure>

## TCP Flag Scans

{% hint style="warning" %}
_FIN, NULL, and Xmas scans work best against Unix systems and are generally unreliable against Microsoft Windows because Windows sends RST for all, regardless of port state._&#x20;
{% endhint %}

### TCP FIN Scan

Sends a FIN packet. Closed ports respond with RST; open/filtered ports drop silently. Can bypass some non-stateful firewalls.

```bash
sudo nmap -sF -Pn -n --reason <IP> 
```

#### Scan

<figure><img src="../../../.gitbook/assets/image (506).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (507).png" alt=""><figcaption></figcaption></figure>

### TCP NULL Scan

Sends a packet with **no flags set**. Same response logic as FIN/Xmas.

```bash
sudo nmap -sN -Pn -n --reason <IP> 
```

#### Scan

<figure><img src="../../../.gitbook/assets/image (508).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (509).png" alt=""><figcaption></figcaption></figure>

### TCP Xmas Scan

Sets FIN, PSH, and URG flags (lit up like a Christmas tree). Same logic as FIN scan.

```bash
sudo nmap -sX -Pn -n --reason <IP> 
```

#### Scan

<figure><img src="../../../.gitbook/assets/image (510).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (511).png" alt=""><figcaption></figcaption></figure>

### Custom TCP Scan

Set arbitrary TCP flags using `--scanflags`. Values: `URG`, `ACK`, `PSH`, `RST`, `SYN`, `FIN` or numeric.

```bash
# SYN+FIN scan
sudo nmap --scanflags SYNFIN 192.168.16.131

# Custom flags with base scan type for response interpretation
sudo nmap --scanflags SYNFIN -sF 192.168.16.131
```

## Obsolete Scans

### TCP Maimon&#x20;

Sends FIN/ACK. Some BSD-derived systems drop the packet for open ports. Rarely useful today.

```bash
sudo nmap -sM 192.168.16.131
```

***

### TCP Window&#x20;

Like ACK scan but examines the TCP window size field in RST responses. Can differentiate open/closed on some systems.

```bash
sudo nmap -sW 192.168.16.131
```

### SCTP INIT Scan

Like TCP SYN scan but for SCTP protocol (used in telecom/VoIP). Sends INIT chunk.

```bash
sudo nmap -sY 192.168.16.131
```

***

### SCTP COOKIE-ECHO Scan

Sends COOKIE-ECHO chunk. Open ports silently drop it; closed ports respond with ABORT.

```bash
sudo nmap -sZ 192.168.16.131
```

***

### IP Protocol Scan

Not a port scan — determines which **IP protocols** (TCP, UDP, ICMP, IGMP, etc.) are supported by the target.

```bash
sudo nmap -sO 192.168.16.131 -Pn -n --reason
```

{% hint style="info" %}
_This scan can scan only ports from 0-255_
{% endhint %}

#### Scan

<figure><img src="../../../.gitbook/assets/image (512).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (513).png" alt=""><figcaption></figcaption></figure>

## Idle / Zombie Scan

The **stealthiest scan** — no packets sent from your real IP. Uses a "zombie" host's IP ID sequence to infer port state.

<figure><img src="../../../.gitbook/assets/Gemini_Generated_Image_ojgdg0ojgdg0ojgd.png" alt=""><figcaption></figcaption></figure>

### Identify Zombies&#x20;

{% hint style="warning" %}
_**The zombie host must have incremental (or predictable) IP ID values and must be idle (low traffic). Not all hosts qualify.**_
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (518).png" alt=""><figcaption></figcaption></figure>

### Scan&#x20;

<details>

<summary><strong>Zombie Scan</strong></summary>

{% code overflow="wrap" %}
```bash
sudo nmap -sI 192.168.16.135 192.168.16.131 -p 80 -n -Pn --reason --packet-trace
Starting Nmap 7.95 ( https://nmap.org ) at 2026-06-09 20:13 IST
SENT (0.0420s) ARP who-has 192.168.16.131 tell 192.168.16.138
RCVD (0.0429s) ARP reply 192.168.16.131 is-at 00:0C:29:88:BB:40
SENT (0.1504s) TCP 192.168.16.138:36745 > 192.168.16.135:80 SA ttl=45 id=3015 iplen=44  seq=3132043177 win=1024 <mss 1460>
RCVD (0.1510s) TCP 192.168.16.135:80 > 192.168.16.138:36745 R ttl=128 id=812 iplen=40  seq=1838472793 win=0 
SENT (0.1819s) TCP 192.168.16.138:36746 > 192.168.16.135:80 SA ttl=39 id=28978 iplen=44  seq=3132043178 win=1024 <mss 1460>
RCVD (0.1826s) TCP 192.168.16.135:80 > 192.168.16.138:36746 R ttl=128 id=813 iplen=40  seq=1838472793 win=0 
SENT (0.2133s) TCP 192.168.16.138:36747 > 192.168.16.135:80 SA ttl=45 id=45715 iplen=44  seq=3132043179 win=1024 <mss 1460>
SENT (0.2459s) TCP 192.168.16.138:36748 > 192.168.16.135:80 SA ttl=59 id=36867 iplen=44  seq=3132043180 win=1024 <mss 1460>
RCVD (0.2248s) TCP 192.168.16.135:80 > 192.168.16.138:36747 R ttl=128 id=814 iplen=40  seq=1838472793 win=0 
SENT (0.2778s) TCP 192.168.16.138:36749 > 192.168.16.135:80 SA ttl=38 id=10983 iplen=44  seq=3132043181 win=1024 <mss 1460>
RCVD (0.2474s) TCP 192.168.16.135:80 > 192.168.16.138:36748 R ttl=128 id=815 iplen=40  seq=1838472793 win=0 
RCVD (0.2784s) TCP 192.168.16.135:80 > 192.168.16.138:36749 R ttl=128 id=816 iplen=40  seq=1838472793 win=0 
SENT (0.3098s) TCP 192.168.16.138:36750 > 192.168.16.135:80 SA ttl=58 id=19270 iplen=44  seq=3132043182 win=1024 <mss 1460>
RCVD (0.3104s) TCP 192.168.16.135:80 > 192.168.16.138:36750 R ttl=128 id=817 iplen=40  seq=1838472793 win=0 
Idle scan using zombie 192.168.16.135 (192.168.16.135:80); Class: Incremental
SENT (0.3106s) TCP 192.168.16.131:36744 > 192.168.16.135:80 SA ttl=57 id=65195 iplen=44  seq=3132043177 win=1024 <mss 1460>
SENT (0.3616s) TCP 192.168.16.131:36744 > 192.168.16.135:80 SA ttl=43 id=55159 iplen=44  seq=3132043178 win=1024 <mss 1460>
SENT (0.4124s) TCP 192.168.16.131:36744 > 192.168.16.135:80 SA ttl=45 id=52159 iplen=44  seq=3132043179 win=1024 <mss 1460>
SENT (0.4634s) TCP 192.168.16.131:36744 > 192.168.16.135:80 SA ttl=42 id=51564 iplen=44  seq=3132043180 win=1024 <mss 1460>
SENT (0.7640s) TCP 192.168.16.138:36891 > 192.168.16.135:80 SA ttl=49 id=25347 iplen=44  seq=2203684827 win=1024 <mss 1460>
RCVD (0.7645s) TCP 192.168.16.135:80 > 192.168.16.138:36891 R ttl=128 id=822 iplen=40  seq=3774054854 win=0 
SENT (0.7646s) TCP 192.168.16.135:80 > 192.168.16.131:80 S ttl=59 id=26650 iplen=44  seq=1822051562 win=1024 <mss 1460>
SENT (0.8152s) TCP 192.168.16.138:36875 > 192.168.16.135:80 SA ttl=43 id=2790 iplen=44  seq=2203685327 win=1024 <mss 1460>
RCVD (0.8157s) TCP 192.168.16.135:80 > 192.168.16.138:36875 R ttl=128 id=824 iplen=40  seq=3774054854 win=0 
SENT (0.8670s) TCP 192.168.16.138:36826 > 192.168.16.135:80 SA ttl=43 id=13235 iplen=44  seq=2203685827 win=1024 <mss 1460>
RCVD (0.8673s) TCP 192.168.16.135:80 > 192.168.16.138:36826 R ttl=128 id=825 iplen=40  seq=3774054854 win=0 
SENT (0.8674s) TCP 192.168.16.135:80 > 192.168.16.131:80 S ttl=54 id=56591 iplen=44  seq=1822051562 win=1024 <mss 1460>
SENT (0.9177s) TCP 192.168.16.138:36977 > 192.168.16.135:80 SA ttl=47 id=14608 iplen=44  seq=2203686327 win=1024 <mss 1460>
RCVD (0.9183s) TCP 192.168.16.135:80 > 192.168.16.138:36977 R ttl=128 id=827 iplen=40  seq=3774054854 win=0 
SENT (0.9692s) TCP 192.168.16.138:36885 > 192.168.16.135:80 SA ttl=39 id=58460 iplen=44  seq=2203686827 win=1024 <mss 1460>
RCVD (0.9698s) TCP 192.168.16.135:80 > 192.168.16.138:36885 R ttl=128 id=828 iplen=40  seq=3774054854 win=0 
Nmap scan report for 192.168.16.131
Host is up, received arp-response (0.0076s latency).

PORT   STATE SERVICE REASON
80/tcp open  http    ipid-change
MAC Address: 00:0C:29:88:BB:40 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.97 seconds
                                                                    
```
{% endcode %}

</details>

<figure><img src="../../../.gitbook/assets/image (519).png" alt=""><figcaption></figcaption></figure>

### Analysis&#x20;

<figure><img src="../../../.gitbook/assets/Gemini_Generated_Image_imvknzimvknzimvk.png" alt=""><figcaption></figcaption></figure>
