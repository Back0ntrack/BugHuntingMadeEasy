# OS Detection

Uses TCP/IP stack fingerprinting to guess the target's operating system by sending varieties of packet to open and closed port.&#x20;

{% hint style="info" %}
_An `open` and `closed` port is required for detecting the type of operating system as OS is detected based on the response nmap gets from `open` and `closed` ports._&#x20;
{% endhint %}

| Flag                         | Description                                                                                                                     |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `-O`                         | Enable OS detection                                                                                                             |
| `--osscan-limit`             | Only attempt OS detection if at least 1 open + 1 closed port found saving time.                                                 |
| `--osscan-guess` / `--fuzzy` | Guess the target operating system more aggressively and display all matching fingerprints, even those with very low confidence. |
| `--max-os-tries <n>`         | Maximum number of OS detection attempts (default 5)                                                                             |

### Basic OS Detection

```bash
sudo nmap -O 192.168.16.131
```

<figure><img src="../../../.gitbook/assets/image (1017).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_OS detection needs at least **1 open and 1 closed** TCP port for best accuracy. Use `--osscan-limit` to skip hosts that don't meet this._
{% endhint %}
