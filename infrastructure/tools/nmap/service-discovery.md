# Service Discovery

Interrogates open ports to determine **what service/version** is running.

| Flag                        | Description                                        |
| --------------------------- | -------------------------------------------------- |
| `-sV`                       | Probe open ports for service/version info          |
| `--version-intensity <0-9>` | Set probe intensity (higher = more probes, slower) |
| `--version-light`           | Intensity 2 — fast but less accurate               |
| `--version-all`             | Intensity 9 — try every single probe               |
| `--version-trace`           | Show detailed version scan debug output            |

### Basic Version detection&#x20;

{% code overflow="wrap" %}
```bash
sudo nmap -sV -Pn -n -p <port> <IP> 
```
{% endcode %}

#### Scan

<figure><img src="../../../.gitbook/assets/image (535).png" alt=""><figcaption></figcaption></figure>

#### Analysis

{% hint style="info" %}
_Thus, `-sV` performs_&#x20;

* _port discovery (typically using a SYN scan) to determine whether port 22 is open_
* _then establishes a full TCP three-way handshake with the target service and,_
* _exchanges application-layer data to identify the service, version, and other banner information before gracefully closing the connection._
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (536).png" alt=""><figcaption></figcaption></figure>

### Other options

```bash
# Basic version detection
sudo nmap -sV 192.168.16.131

# Aggressive version detection
sudo nmap -sV --version-all 192.168.16.131

# Light and fast version scan
sudo nmap -sV --version-light 192.168.16.131
```

{% hint style="info" %}
_`-sV` with default intensity (7) is usually sufficient. Use `--version-all` when you get "unknown" services._
{% endhint %}
