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

## Service and Version Detection

Interrogates open ports to determine **what service/version** is running.

| Flag                        | Description                                        |
| --------------------------- | -------------------------------------------------- |
| `-sV`                       | Probe open ports for service/version info          |
| `--version-intensity <0-9>` | Set probe intensity (higher = more probes, slower) |
| `--version-light`           | Intensity 2 — fast but less accurate               |
| `--version-all`             | Intensity 9 — try every single probe               |
| `--version-trace`           | Show detailed version scan debug output            |

```bash
# Basic version detection
sudo nmap -sV 192.168.16.131

# Aggressive version detection
sudo nmap -sV --version-all 192.168.16.131

# Light and fast version scan
sudo nmap -sV --version-light 192.168.16.131

# Version scan with trace (debugging)
sudo nmap -sV --version-trace 192.168.16.131

# Version detection on specific ports
sudo nmap -sV -p 80,443,22,21 192.168.16.131
```

`-sV` with default intensity (7) is usually sufficient. Use `--version-all` when you get "unknown" services.

***

## OS Detection

Uses TCP/IP stack fingerprinting to guess the target's operating system.

| Flag                         | Description                                                        |
| ---------------------------- | ------------------------------------------------------------------ |
| `-O`                         | Enable OS detection                                                |
| `--osscan-limit`             | Only attempt OS detection if at least 1 open + 1 closed port found |
| `--osscan-guess` / `--fuzzy` | Guess OS more aggressively                                         |
| `--max-os-tries <n>`         | Maximum number of OS detection attempts (default 5)                |

```bash
# Basic OS detection
sudo nmap -O 192.168.16.131

# Aggressive OS guessing
sudo nmap -O --osscan-guess 192.168.16.131

# Limit OS detection attempts
sudo nmap -O --max-os-tries 2 192.168.16.140

# OS + version combined
sudo nmap -O -sV 192.168.1.65
```

OS detection needs at least **1 open and 1 closed** TCP port for best accuracy. Use `--osscan-limit` to skip hosts that don't meet this.

***

## Nmap Scripting Engine (NSE)

NSE extends Nmap with Lua scripts for vulnerability detection, brute-forcing, discovery, and more. Scripts are stored in `/usr/share/nmap/scripts/`.

### NSE Flags

| Flag                            | Description                                               |
| ------------------------------- | --------------------------------------------------------- |
| `-sC`                           | Run default category scripts (same as `--script=default`) |
| `--script <name/category/path>` | Run specific script(s) or categories                      |
| `--script-args <args>`          | Pass arguments to scripts                                 |
| `--script-args-file <file>`     | Load script arguments from a file                         |
| `--script-trace`                | Show all data sent/received by scripts                    |
| `--script-updatedb`             | Update the script database                                |
| `--script-help <script>`        | Show help for a specific script                           |

### Script Categories

| Category    | Purpose                                          |
| ----------- | ------------------------------------------------ |
| `auth`      | Authentication & credential checks               |
| `broadcast` | Discover hosts via broadcast                     |
| `brute`     | Brute-force credentials                          |
| `default`   | Safe, useful scripts run with `-sC`              |
| `discovery` | Discover more info (SNMP, DNS, SMB shares, etc.) |
| `dos`       | Denial of service (use with caution)             |
| `exploit`   | Actively exploit vulnerabilities                 |
| `external`  | Send data to third-party services (e.g., whois)  |
| `fuzzer`    | Fuzz protocols for unexpected behavior           |
| `intrusive` | May crash target or consume resources            |
| `malware`   | Check for malware/backdoors                      |
| `safe`      | Won't crash targets or use heavy resources       |
| `version`   | Advanced version detection                       |
| `vuln`      | Check for known vulnerabilities                  |

### Script Selection Syntax

```bash
# Run a single script
sudo nmap --script http-title 192.168.16.131

# Run a category
sudo nmap --script vuln 192.168.16.131

# Run multiple categories
sudo nmap --script "vuln,safe" 192.168.16.131

# Wildcard
sudo nmap --script "http-*" 192.168.16.131

# Boolean expressions
sudo nmap --script "default and safe" 192.168.16.131
sudo nmap --script "default or vuln" 192.168.16.131
sudo nmap --script "not intrusive" 192.168.16.131
sudo nmap --script "(default or vuln) and not dos" 192.168.16.131

# Run scripts from a custom path
sudo nmap --script /path/to/custom-script.nse 192.168.16.131
```

***

### Most Used NSE Scripts

#### Vulnerability Scanning

```bash
# Run all vuln scripts
sudo nmap --script vuln 192.168.16.131

# Check for specific CVEs
sudo nmap --script vuln --script-args vulns.showall 192.168.16.131

# SMB vulnerabilities (MS17-010 EternalBlue, MS08-067, etc.)
sudo nmap --script smb-vuln-* -p 445 192.168.16.140

# Specific: EternalBlue check
sudo nmap --script smb-vuln-ms17-010 -p 445 192.168.16.140

# Shellshock
sudo nmap --script http-shellshock --script-args uri=/cgi-bin/test.cgi -p 80 192.168.16.131

# Heartbleed (OpenSSL)
sudo nmap --script ssl-heartbleed -p 443 192.168.16.131

# POODLE
sudo nmap --script ssl-poodle -p 443 192.168.16.131

# SSL/TLS enumeration
sudo nmap --script ssl-enum-ciphers -p 443 192.168.16.131
```

#### Brute Force

```bash
# SSH brute force
sudo nmap --script ssh-brute -p 22 192.168.1.65 \
  --script-args userdb=users.txt,passdb=passwords.txt

# FTP brute force
sudo nmap --script ftp-brute -p 21 192.168.16.131

# HTTP form brute force
sudo nmap --script http-form-brute -p 80 192.168.16.131 \
  --script-args http-form-brute.path=/login

# MySQL brute force
sudo nmap --script mysql-brute -p 3306 192.168.16.131

# SNMP brute force
sudo nmap --script snmp-brute -p 161 192.168.16.131

# Telnet brute force
sudo nmap --script telnet-brute -p 23 192.168.16.140
```

#### Discovery & Enumeration

```bash
# HTTP enumeration
sudo nmap --script http-enum -p 80 192.168.16.131

# HTTP title grab
sudo nmap --script http-title -p 80 192.168.16.131

# HTTP methods allowed
sudo nmap --script http-methods -p 80 192.168.16.131 \
  --script-args http-methods.url-path=/

# HTTP headers
sudo nmap --script http-headers -p 80 192.168.16.131

# HTTP robots.txt
sudo nmap --script http-robots.txt -p 80 192.168.16.131

# DNS zone transfer
sudo nmap --script dns-zone-transfer --script-args dns-zone-transfer.domain=example.com -p 53

# DNS brute force subdomains
sudo nmap --script dns-brute --script-args dns-brute.domain=example.com

# SMB shares enumeration
sudo nmap --script smb-enum-shares -p 445 192.168.16.140

# SMB users enumeration
sudo nmap --script smb-enum-users -p 445 192.168.16.140

# SMB OS discovery
sudo nmap --script smb-os-discovery -p 445 192.168.16.140

# SNMP enumeration
sudo nmap --script snmp-info -p 161 192.168.16.131

# NFS shares
sudo nmap --script nfs-ls,nfs-showmount,nfs-statfs -p 111,2049 192.168.16.140

# MySQL info
sudo nmap --script mysql-info -p 3306 192.168.16.131

# MySQL databases
sudo nmap --script mysql-databases -p 3306 192.168.16.131 \
  --script-args mysqluser=root,mysqlpass=

# Banner grabbing (version detection is better, but this exists)
sudo nmap --script banner -p 21,22,80 192.168.16.131

# WHOIS lookup
sudo nmap --script whois-ip 192.168.16.131
```

#### Authentication Checks

```bash
# Anonymous FTP login
sudo nmap --script ftp-anon -p 21 192.168.16.131

# MySQL empty password
sudo nmap --script mysql-empty-password -p 3306 192.168.16.131

# MS-SQL info
sudo nmap --script ms-sql-info -p 1433 192.168.16.140

# SSH host key fingerprint
sudo nmap --script ssh-hostkey -p 22 192.168.1.65

# SSH auth methods
sudo nmap --script ssh-auth-methods -p 22 192.168.1.65
```

#### Malware & Backdoor Detection

```bash
# Check for known malware
sudo nmap --script http-malware-host -p 80 192.168.16.131

# Check for IRC backdoors
sudo nmap --script irc-unrealircd-backdoor -p 6667 192.168.16.131

# Check for FTP backdoor (vsftpd 2.3.4)
sudo nmap --script ftp-vsftpd-backdoor -p 21 192.168.16.131
```

***

### Script Arguments

Pass data to scripts via `--script-args`:

```bash
# Key=value pairs (comma-separated)
sudo nmap --script http-brute -p 80 192.168.16.131 \
  --script-args http-brute.path=/admin,http-brute.method=POST

# Credentials
sudo nmap --script mysql-databases -p 3306 192.168.16.131 \
  --script-args mysqluser=root,mysqlpass=toor

# Arguments from file
sudo nmap --script http-brute --script-args-file args.txt 192.168.16.131
```

***

### Useful NSE Commands

```bash
# List all available scripts
ls /usr/share/nmap/scripts/

# Search for scripts by name
ls /usr/share/nmap/scripts/ | grep smb
ls /usr/share/nmap/scripts/ | grep http

# Get help on a script
nmap --script-help http-enum
nmap --script-help smb-vuln-ms17-010

# Update the script database after adding custom scripts
sudo nmap --script-updatedb

# View a script's source to understand what it does
cat /usr/share/nmap/scripts/http-enum.nse
```

***

## Timing and Performance

Control scan speed. Higher timing = faster but noisier; lower = stealthier but slower.

### Timing Templates

| Flag  | Name       | Description                               |
| ----- | ---------- | ----------------------------------------- |
| `-T0` | Paranoid   | IDS evasion, 5 min between probes         |
| `-T1` | Sneaky     | IDS evasion, 15 sec between probes        |
| `-T2` | Polite     | Reduces bandwidth, 0.4 sec between probes |
| `-T3` | Normal     | Default, balanced                         |
| `-T4` | Aggressive | Faster, assumes reliable network          |
| `-T5` | Insane     | Sacrifices accuracy for speed             |

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

| Flag                         | Description                         |
| ---------------------------- | ----------------------------------- |
| `--min-hostgroup <size>`     | Minimum hosts to scan in parallel   |
| `--max-hostgroup <size>`     | Maximum hosts to scan in parallel   |
| `--min-parallelism <probes>` | Minimum outstanding probes          |
| `--max-parallelism <probes>` | Maximum outstanding probes          |
| `--min-rtt-timeout <ms>`     | Minimum probe round-trip timeout    |
| `--max-rtt-timeout <ms>`     | Maximum probe round-trip timeout    |
| `--initial-rtt-timeout <ms>` | Initial probe timeout               |
| `--max-retries <n>`          | Max number of probe retransmissions |
| `--host-timeout <ms>`        | Give up on a host after this time   |
| `--scan-delay <ms>`          | Minimum delay between probes        |
| `--max-scan-delay <ms>`      | Maximum delay between probes        |
| `--min-rate <n>`             | Send at least N packets per second  |
| `--max-rate <n>`             | Send at most N packets per second   |
| `--defeat-rst-ratelimit`     | Ignore RST rate limiting by target  |

```bash
# Force minimum packet rate
sudo nmap --min-rate 1000 -sS 192.168.16.131

# Limit max rate to stay under radar
sudo nmap --max-rate 100 -sS 192.168.16.131

# Set max retries to 2 (faster, less accurate)
sudo nmap --max-retries 2 -sS 192.168.16.131

# Skip hosts that take too long (30 seconds)
sudo nmap --host-timeout 30s 192.168.16.0/24

# Scan delay for IDS evasion
sudo nmap --scan-delay 5s -sS 192.168.16.131

# Custom RTT timeout
sudo nmap --initial-rtt-timeout 100ms --max-rtt-timeout 500ms 192.168.16.131
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

| Flag                   | Description                                       |
| ---------------------- | ------------------------------------------------- |
| `-oN <file>`           | Normal output (human-readable)                    |
| `-oX <file>`           | XML output                                        |
| `-oG <file>`           | Grepable output                                   |
| `-oA <basename>`       | All 3 formats at once (`.nmap`, `.xml`, `.gnmap`) |
| `-oS <file>`           | Script kiddie output (leet speak, just for fun)   |
| `-v`                   | Increase verbosity (use `-vv` for more)           |
| `-d`                   | Increase debugging (use `-dd` for more)           |
| `--reason`             | Show reason for port state                        |
| `--open`               | Show only open ports                              |
| `--packet-trace`       | Show all packets sent/received                    |
| `--iflist`             | Show host interfaces and routes                   |
| `--append-output`      | Append to output file instead of overwriting      |
| `--resume <file>`      | Resume aborted scan                               |
| `--stats-every <time>` | Print status every N seconds                      |
| `--stylesheet <url>`   | XSL stylesheet for XML output                     |
| `--webxml`             | Use Nmap.org stylesheet for XML                   |
| `--no-stylesheet`      | No XSL stylesheet for XML                         |

```bash
# Normal output
sudo nmap -sS 192.168.16.131 -oN scan_result.nmap

# XML output (for tools like Metasploit, Searchsploit)
sudo nmap -sS 192.168.16.131 -oX scan_result.xml

# Grepable output
sudo nmap -sS 192.168.16.131 -oG scan_result.gnmap

# All formats at once
sudo nmap -sS 192.168.16.131 -oA full_scan

# Show only open ports
sudo nmap -sS --open 192.168.16.131

# Show reason for each port state
sudo nmap -sS --reason 192.168.16.131

# Verbose scan with stats every 10 seconds
sudo nmap -sS -vv --stats-every 10s 192.168.16.131

# Resume an interrupted scan
sudo nmap --resume scan_result.nmap

# Packet trace (debugging)
sudo nmap -sS --packet-trace -p 80 192.168.16.131

# Show host interfaces and routes
nmap --iflist
```

Always use `-oA` to save in all formats. XML is especially useful — it can be imported into Metasploit (`db_import`), parsed with `xmllint`/`xsltproc`, or processed with tools like `searchsploit --nmap`.

***

## Miscellaneous Options

| Flag             | Description                                 |
| ---------------- | ------------------------------------------- |
| `-6`             | Enable IPv6 scanning                        |
| `-A`             | Aggressive scan = `-O -sV -sC --traceroute` |
| `--send-eth`     | Send raw ethernet frames                    |
| `--send-ip`      | Send raw IP packets                         |
| `--privileged`   | Assume user has full raw socket privileges  |
| `--unprivileged` | Assume user lacks raw socket privileges     |
| `-V`             | Print Nmap version                          |
| `-h`             | Print help summary                          |

```bash
# Aggressive scan (OS + version + scripts + traceroute)
sudo nmap -A 192.168.16.131

# IPv6 scan
sudo nmap -6 ::1

# Print version
nmap -V
```

`-A` is convenient but **noisy and slow**. In real engagements, run individual flags for more control.

***

## Real-World Command Combinations

These are battle-tested combinations used in pentesting, CTFs, and real assessments.

### Quick Initial Recon

```bash
# Fast scan — top 100 ports with service detection
sudo nmap -F -sV 192.168.16.131

# Quick TCP SYN scan with version on top 1000
sudo nmap -sS -sV -T4 192.168.16.131

# Quick scan with OS and default scripts
sudo nmap -sS -sV -sC -O -T4 192.168.16.131
```

### Full Comprehensive Scan

```bash
# All TCP ports with version + OS + scripts
sudo nmap -sS -sV -sC -O -p- -T4 192.168.16.131 -oA full_tcp

# All TCP + top 1000 UDP with version + scripts
sudo nmap -sS -sU -sV -sC -O --top-ports 1000 -T4 192.168.16.131 -oA full_scan

# All ports, aggressive, save all formats
sudo nmap -A -p- -T4 192.168.16.131 -oA aggressive_full
```

### Stealth Scanning

```bash
# Stealth SYN scan with fragmentation and decoys
sudo nmap -sS -f -D RND:5 -T2 -p- 192.168.16.131

# Stealth with source port 53 (DNS) and data padding
sudo nmap -sS -g 53 --data-length 40 -T1 192.168.16.131

# FIN scan with timing control
sudo nmap -sF --scan-delay 2s -T1 192.168.16.131
```

### Vulnerability Assessment

```bash
# Full vuln scan
sudo nmap -sV --script vuln -p- 192.168.16.131 -oA vuln_scan

# Web vulnerability scan
sudo nmap -sV --script "http-vuln-*" -p 80,443,8080,8443 192.168.16.131

# SMB vulnerability scan
sudo nmap --script "smb-vuln-*" -p 139,445 192.168.16.140

# SSL/TLS audit
sudo nmap --script ssl-enum-ciphers,ssl-heartbleed,ssl-poodle,ssl-cert -p 443 192.168.16.131
```

### Network Sweep

```bash
# Fast ping sweep with no port scan
sudo nmap -sn -n -T4 192.168.16.0/24

# Discover live hosts and get open ports fast
sudo nmap -sS -T4 --open -n 192.168.16.0/24

# Full subnet scan with version detection
sudo nmap -sS -sV -T4 --open -n 192.168.16.0/24 -oA subnet_scan
```

### Targeted Service Enumeration

```bash
# HTTP full enumeration
sudo nmap -sV --script "http-enum,http-title,http-methods,http-headers,http-robots.txt,http-server-header" -p 80,443,8080 192.168.16.131

# SMB full enumeration
sudo nmap --script "smb-enum-shares,smb-enum-users,smb-os-discovery,smb-protocols,smb-security-mode" -p 445 192.168.16.140

# FTP enumeration
sudo nmap -sV --script "ftp-anon,ftp-bounce,ftp-syst,ftp-vsftpd-backdoor" -p 21 192.168.16.131

# SSH enumeration
sudo nmap -sV --script "ssh-hostkey,ssh-auth-methods,ssh2-enum-algos" -p 22 192.168.1.65

# MySQL enumeration
sudo nmap -sV --script "mysql-info,mysql-enum,mysql-empty-password,mysql-databases" -p 3306 192.168.16.131

# DNS enumeration
sudo nmap --script "dns-zone-transfer,dns-brute,dns-cache-snoop" -p 53 192.168.16.131
```

### High Performance Scanning

```bash
# Maximum speed on local network
sudo nmap -sS --min-rate 5000 --max-retries 1 -T5 -p- -n 192.168.16.131

# Fast scan with parallelism
sudo nmap -sS --min-parallelism 100 --min-hostgroup 256 -T4 -n 192.168.16.0/24

# Balanced speed and accuracy
sudo nmap -sS --min-rate 1000 --max-retries 2 -T4 -p- 192.168.16.131
```

***

## Nmap Cheat Table

Quick reference for the most common flags.

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
