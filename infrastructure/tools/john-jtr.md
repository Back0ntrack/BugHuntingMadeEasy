# John (JtR)

## Cracking Protected Files&#x20;

### Hash Convertors&#x20;

{% code overflow="wrap" %}
```bash
locate *2john*
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1634).png" alt=""><figcaption></figcaption></figure>

### Crack Encrypted SSH Keys

{% code overflow="wrap" %}
```bash
ssh2john ssh_key > ssh_hash
john ssh_hash --wordlist=<wordlist>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1633).png" alt=""><figcaption></figcaption></figure>

### Crack Encrypted Excel File&#x20;

{% code overflow="wrap" %}
```
office2john Confidential.docx > file.hash
john file.hash --wordlist=<wordlist>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1635).png" alt=""><figcaption></figcaption></figure>

### Crack Encrypted Drives&#x20;

#### Identify hash&#x20;

<figure><img src="../../.gitbook/assets/image (1636).png" alt=""><figcaption></figcaption></figure>

#### Cracking the hash&#x20;

<figure><img src="../../.gitbook/assets/image (1637).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (1638).png" alt=""><figcaption></figcaption></figure>
