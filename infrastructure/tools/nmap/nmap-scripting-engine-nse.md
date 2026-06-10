# Nmap Scripting Engine (NSE)

NSE extends Nmap with Lua scripts for vulnerability detection, brute-forcing, discovery, and more. Scripts are stored in `/usr/share/nmap/scripts/`.

## NSE Flags

{% hint style="info" %}
_Update the script database using `--script-updatedb`_&#x20;
{% endhint %}

| Flag                            | Description                                               |
| ------------------------------- | --------------------------------------------------------- |
| `-sC`                           | Run default category scripts (same as `--script=default`) |
| `--script <name/category/path>` | Run specific script(s) or categories                      |
| `--script-args <args>`          | Pass arguments to scripts                                 |
| `--script-args-file <file>`     | Load script arguments from a file                         |
| `--script-trace`                | Show all data sent/received by scripts                    |
| `--script-updatedb`             | Update the script database                                |
| `--script-help <script>`        | Show help for a specific script                           |

## Run default scripts

{% code overflow="wrap" %}
```bash
sudo nmap -sC -Pn -n -F <ip> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (538).png" alt=""><figcaption></figcaption></figure>

## Run Specific scripts&#x20;

{% code overflow="wrap" %}
```bash
sudo nmap --script <script_name> -Pn -n <host>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (539).png" alt=""><figcaption></figcaption></figure>

## Useful NSE Commands

### Search for script by protocol name

{% code overflow="wrap" %}
```bash
ls /usr/share/nmap/scripts/ | grep smb
ls /usr/share/nmap/scripts/ | grep http
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (540).png" alt=""><figcaption></figcaption></figure>

### Get help for specific script

```bash
nmap --script-help http-enum
nmap --script-help smb-vuln-ms17-010
```

<figure><img src="../../../.gitbook/assets/image (541).png" alt=""><figcaption></figcaption></figure>

## Script Categories

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

## Most Used NSE Scripts

### Vulnerability Scanning

```bash
sudo nmap --script vuln 192.168.16.131
```

### Brute Force

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
