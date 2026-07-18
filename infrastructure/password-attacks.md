# Password Attacks

## Identify Hashes

### 1. Using hashid

<figure><img src="../.gitbook/assets/image (1630).png" alt=""><figcaption></figcaption></figure>

### 2. Using hash-identifier&#x20;

<figure><img src="../.gitbook/assets/image (1631).png" alt=""><figcaption></figcaption></figure>

### 3. Using Name-That-Hash&#x20;

<figure><img src="../.gitbook/assets/image (1632).png" alt=""><figcaption></figcaption></figure>

## Cracking Protected Files&#x20;

### Hash Convertors&#x20;

{% code overflow="wrap" %}
```bash
locate *2john*
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1634).png" alt=""><figcaption></figcaption></figure>

### Crack Encrypted SSH Keys

{% code overflow="wrap" %}
```bash
ssh2john ssh_key > ssh_hash
john ssh_hash --wordlist=<wordlist>
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1633).png" alt=""><figcaption></figcaption></figure>

### Crack Encrypted Excel File&#x20;

{% code overflow="wrap" %}
```
office2john Confidential.docx > file.hash
john file.hash --wordlist=<wordlist>
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1635).png" alt=""><figcaption></figcaption></figure>

### Crack Encrypted Drives&#x20;

#### Identify hash&#x20;

<figure><img src="../.gitbook/assets/image (1636).png" alt=""><figcaption></figcaption></figure>

#### Cracking the hash&#x20;

<figure><img src="../.gitbook/assets/image (1637).png" alt=""><figcaption></figcaption></figure>

#### Mounting Encrypted bitlocker drive in Linux&#x20;

{% code overflow="wrap" %}
```
sudo mkdir -p /media/bitlockermount
sudo mkdir -p /media/bitlocker
sudo losetup -f -P Private.vhd
losetup -a
lsblk
sudo dislocker /dev/loop1p1 -ufrancisco -- /media/bitlocker
sudo mount -t ntfs-3g -o loop /media/bitlocker/dislocker-file /media/bitlockermount
ls -l /media/bitlockermount
cat /media/bitlockermount/flag.txt
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1638).png" alt=""><figcaption></figcaption></figure>

## Windows Credential Extraction&#x20;

### Cracking SAM

#### Copy Files&#x20;

{% code overflow="wrap" %}
```cmd
reg.exe save hklm\sam C:\sam.save
reg.exe save hklm\system C:\system.save
reg.exe save hklm\security C:\security.save
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1640).png" alt=""><figcaption></figcaption></figure>

#### Dump Hashes&#x20;

1. **Using impacket-secretsdump**

{% code overflow="wrap" %}
```bash
impacket-secretsdump -sam ./sam -security ./security -system ./system LOCAL
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1641).png" alt=""><figcaption></figcaption></figure>

2. **Using samdump2**

{% code overflow="wrap" %}
```bash
samdump2 system sam
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1643).png" alt=""><figcaption></figcaption></figure>

3. **Using pwdump2**

#### Cracking hashes&#x20;

{% code overflow="wrap" %}
```bash
john --format=NT sam_hashes.txt /usr/share/wordlists/rockyou.txt
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1642).png" alt=""><figcaption></figcaption></figure>

### LSA

#### &#x20;Remotely dump LSA&#x20;

<figure><img src="../.gitbook/assets/image (1645).png" alt=""><figcaption></figcaption></figure>
