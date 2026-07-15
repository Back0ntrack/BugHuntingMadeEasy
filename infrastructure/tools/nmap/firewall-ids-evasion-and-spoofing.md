# Firewall/IDS Evasion & Spoofing

## Decoys

{% hint style="info" %}
_**Important:** Decoy scanning (`-D`) does **not replace your real IP**. Nmap sends packets from the **real source IP and all specified decoy IPs**. Therefore, the target receives requests from both the attacker and the decoys and may reply to all of them. The purpose of decoys is to **obfuscate the true scanner in logs and IDS/IPS alerts**, not to make the real source disappear._
{% endhint %}

{% code overflow="wrap" %}
```bash
sudo nmap <scan config> -D decoy1,decoy2 <target>

sudo nmap <scan config> -D RND:2 <target>
```
{% endcode %}

#### Scan - 1

<figure><img src="../../../.gitbook/assets/image (1027).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1028).png" alt=""><figcaption></figcaption></figure>

#### Scan - 2

<figure><img src="../../../.gitbook/assets/image (1029).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1030).png" alt=""><figcaption></figcaption></figure>

## Source Port Manipulation&#x20;

#### Scan

{% code overflow="wrap" %}
```bash
sudo nmap -sS -Pn -n -iL host_list.txt -p 445 --source-port 80
```
{% endcode %}

{% code overflow="wrap" %}
```bash
sudo nmap -ss -Pn -n -iL host_list.txt -p 445 -g 80
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1032).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (1031).png" alt=""><figcaption></figcaption></figure>

## Fragment packets&#x20;

{% code overflow="wrap" %}
```bash
sudo nmap -sS -Pn -n -iL host_list.txt -p 445 -f
```
{% endcode %}

#### Scan

<figure><img src="../../../.gitbook/assets/image (1033).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (1034).png" alt=""><figcaption></figcaption></figure>

## Custom data&#x20;

The below technique can resemble FTP traffic for bypassing firewall.&#x20;

{% code overflow="wrap" %}
```bash
nmap --data-string "USER anonymous" <target>

nmap --data "5553455220616e6f6e796d6f7573" <target>
```
{% endcode %}

### Hex String

#### Scan

{% code overflow="wrap" %}
```bash
sudo nmap --data <hex string> <scan_config> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1037).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (1036).png" alt=""><figcaption></figcaption></figure>

### Normal String&#x20;

#### Scan

{% code overflow="wrap" %}
```bash
sudo nmap --data-string <normal-string> <scan config> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1039).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (1038).png" alt=""><figcaption></figcaption></figure>

## Change Packet Size

{% hint style="info" %}
_Normal Nmap packet → predictable_\
_Randomized payload → harder signature matching_
{% endhint %}

#### Scan

<figure><img src="../../../.gitbook/assets/image (1040).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (1041).png" alt=""><figcaption></figcaption></figure>

## Spoof MAC&#x20;

### Specific MAC&#x20;

{% code overflow="wrap" %}
```bash
sudo nmap --spoof-mac 00:11:22:33:44:55 <target>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1042).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (1043).png" alt=""><figcaption></figcaption></figure>

### Device Specific

#### Scan&#x20;

{% code overflow="wrap" %}
```bash
sudo nmap 192.168.16.135 -sS -Pn -n -p 445 --reason --spoof-mac Dell
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1044).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (1045).png" alt=""><figcaption></figcaption></figure>

### Random MAC&#x20;

#### Scan

{% code overflow="wrap" %}
```bash
sudo nmap 192.168.16.135 -sS -Pn -n -p 445 --reason --spoof-mac 0
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1047).png" alt=""><figcaption></figcaption></figure>

#### Analysis

<figure><img src="../../../.gitbook/assets/image (1046).png" alt=""><figcaption></figcaption></figure>

## Other Options

### Send Bad Checksum

{% code overflow="wrap" %}
```bash
sudo nmap <scan_config> --badsum
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1035).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_`--badsum` sends intentionally corrupted packets. Legitimate hosts should ignore them. Any response likely comes from a firewall, IDS/IPS, or other network middlebox rather than the target host itself._
{% endhint %}

We got a filtered response because .135 is a legitimate host and it dropped the packet. but we'll get some respond from firewall, IDS/IPS or middleware if any.
