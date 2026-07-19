---
icon: windows
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Windows Fundamentals

## Operating System Structure&#x20;

In Windows operating systems, the root directory is \<drive\_letter>:\ (commonly C drive). The root directory (also known as the boot partition) is where the operating system is installed.

<table data-search="false"><thead><tr><th width="129.20001220703125">Directory</th><th width="612.4000854492188">Function</th></tr></thead><tbody><tr><td>Perflogs</td><td>Can hold Windows performance logs but is empty by default.</td></tr><tr><td>Program Files</td><td>On 32-bit systems, all 16-bit and 32-bit programs are installed here. On 64-bit systems, only 64-bit programs are installed here.</td></tr><tr><td>Program Files (x86)</td><td>32-bit and 16-bit programs are installed here on 64-bit editions of Windows.</td></tr><tr><td>ProgramData</td><td>This is a hidden folder that contains data that is essential for certain installed programs to run. This data is accessible by the program no matter what user is running it.</td></tr><tr><td>Users</td><td>This folder contains user profiles for each user that logs onto the system and contains the two folders Public and Default.</td></tr><tr><td>Default</td><td>This is the default user profile template for all created users. Whenever a new user is added to the system, their profile is based on the Default profile.</td></tr><tr><td>Public</td><td>This folder is intended for computer users to share files and is accessible to all users by default. This folder is shared over the network by default but requires a valid network account to access.</td></tr><tr><td>AppData</td><td><p>Per user application data and settings are stored in a hidden user subfolder (i.e., <code>john_doe\AppData</code>). Each of these folders contains three subfolders. </p><ul><li>The <code>Roaming</code> folder contains machine-independent data that should follow the user's profile, such as custom dictionaries. </li><li>The <code>Local</code> folder is specific to the computer itself and is never synchronized across the network. </li><li><code>LocalLow</code> is similar to the Local folder, but it has a lower data integrity level. Therefore it can be used, for example, by a web browser set to protected or safe mode.</li></ul></td></tr><tr><td>Windows</td><td>The majority of the files required for the Windows operating system are contained here.</td></tr><tr><td>System, System32, SysWOW64</td><td>Contains all DLLs required for the core features of Windows and the Windows API. The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path.</td></tr><tr><td>WinSxS</td><td>The Windows Component Store contains a copy of all Windows components, updates, and service packs.</td></tr></tbody></table>

Integrity Control Access Control List (icacls)


**`icacls`** is a Windows command-line tool used to **view and manage NTFS file and folder permissions**, just like the **Security** tab in File Explorer.

<figure><img src="../../../.gitbook/assets/image (1428).png" alt=""><figcaption></figcaption></figure>

### Common Permission Meanings

<table data-search="false"><thead><tr><th width="103.5999755859375">Permission</th><th width="562">Meaning</th></tr></thead><tbody><tr><td><strong>F</strong></td><td><strong>Full Control</strong> – Read, write, modify, execute, change permissions, and take ownership.</td></tr><tr><td><strong>M</strong></td><td><strong>Modify</strong> – Read, write, execute, and delete files/folders.</td></tr><tr><td><strong>RX</strong></td><td><strong>Read &#x26; Execute</strong> – View contents and run programs.</td></tr><tr><td><strong>R</strong></td><td><strong>Read</strong> – View/open files and folders only.</td></tr><tr><td><strong>W</strong></td><td><strong>Write</strong> – Create or modify files and folders.</td></tr><tr><td><strong>D</strong></td><td><strong>Delete</strong> – Delete the file or folder.</td></tr><tr><td><strong>N</strong></td><td><strong>No Access</strong> – Denies all access.</td></tr></tbody></table>

### Inheritance Settings&#x20;

<table><thead><tr><th width="103.60003662109375">Setting</th><th width="565.2000122070312">Meaning</th></tr></thead><tbody><tr><td><strong>(I)</strong></td><td>Inherited permission (from parent folder).</td></tr><tr><td><strong>(OI)</strong></td><td>Object Inherit – Applies to <strong>files</strong> inside the folder. (not applied on subfolders)</td></tr><tr><td><strong>(CI)</strong></td><td>Container Inherit – Applies to <strong>subfolders</strong>. </td></tr><tr><td><strong>(IO)</strong></td><td>Inherit Only – Doesn't apply to the current object; only to child objects.</td></tr><tr><td><strong>(NP)</strong></td><td>No Propagate – Child objects inherit the permission, but it isn't passed further.</td></tr></tbody></table>

#### Grant Permission to a file&#x20;

{% code overflow="wrap" %}
```cmd
C:\htb> icacls c:\users /grant joe:f
processed file: c:\users
Successfully processed 1 files; Failed processing 0 files
```
{% endcode %}

## NTFS File System&#x20;

NTFS (**New Technology File System**) is the default file system used by modern Windows operating systems.

### Share Permissions&#x20;

<table><thead><tr><th width="154.79998779296875">Permission</th><th>Description</th></tr></thead><tbody><tr><td><code>Full Control</code></td><td>Users are permitted to perform all actions given by Change and Read permissions as well as change permissions for NTFS files and subfolders</td></tr><tr><td><code>Change</code></td><td>Users are permitted to read, edit, delete and add files and subfolders</td></tr><tr><td><code>Read</code></td><td>Users are allowed to view file &#x26; subfolder contents</td></tr></tbody></table>

### NTFS Basic Permissions&#x20;

<table data-search="false"><thead><tr><th width="201.20001220703125">Permission</th><th>Description</th></tr></thead><tbody><tr><td><code>Full Control</code></td><td>Users are permitted to add, edit, move, delete files &#x26; folders as well as change NTFS permissions that apply to all allowed folders</td></tr><tr><td><code>Modify</code></td><td>Users are permitted or denied permissions to view and modify files and folders. This includes adding or deleting files</td></tr><tr><td><code>Read &#x26; Execute</code></td><td>Users are permitted or denied permissions to read the contents of files and execute programs</td></tr><tr><td><code>List folder contents</code></td><td>Users are permitted or denied permissions to view a listing of files and subfolders</td></tr><tr><td><code>Read</code></td><td>Users are permitted or denied permissions to read the contents of files</td></tr><tr><td><code>Write</code></td><td>Users are permitted or denied permissions to write changes to a file and add new files to a folder</td></tr><tr><td><code>Special Permissions</code></td><td>A variety of advanced permissions options</td></tr></tbody></table>

### NTFS Special Permissions&#x20;

<table data-search="false"><thead><tr><th width="279.60003662109375">Permission</th><th width="437.99993896484375">Description</th></tr></thead><tbody><tr><td><code>Full control</code></td><td>Users are permitted or denied permissions to add, edit, move, delete files &#x26; folders as well as change NTFS permissions that apply to all permitted folders</td></tr><tr><td><code>Traverse folder / execute file</code></td><td>Users are permitted or denied permissions to access a subfolder within a directory structure even if the user is denied access to contents at the parent folder level. Users may also be permitted or denied permissions to execute programs</td></tr><tr><td><code>List folder/read data</code></td><td>Users are permitted or denied permissions to view files and folders contained in the parent folder. Users can also be permitted to open and view files</td></tr><tr><td><code>Read attributes</code></td><td>Users are permitted or denied permissions to view basic attributes of a file or folder. Examples of basic attributes: system, archive, read-only, and hidden</td></tr><tr><td><code>Read extended attributes</code></td><td>Users are permitted or denied permissions to view extended attributes of a file or folder. Attributes differ depending on the program</td></tr><tr><td><code>Create files/write data</code></td><td>Users are permitted or denied permissions to create files within a folder and make changes to a file</td></tr><tr><td><code>Create folders/append data</code></td><td>Users are permitted or denied permissions to create subfolders within a folder. Data can be added to files but pre-existing content cannot be overwritten</td></tr><tr><td><code>Write attributes</code></td><td>Users are permitted or denied to change file attributes. This permission does not grant access to creating files or folders</td></tr><tr><td><code>Write extended attributes</code></td><td>Users are permitted or denied permissions to change extended attributes on a file or folder. Attributes differ depending on the program</td></tr><tr><td><code>Delete subfolders and files</code></td><td>Users are permitted or denied permissions to delete subfolders and files. Parent folders will not be deleted</td></tr><tr><td><code>Delete</code></td><td>Users are permitted or denied permissions to delete parent folders, subfolders and files.</td></tr><tr><td><code>Read permissions</code></td><td>Users are permitted or denied permissions to read permissions of a folder</td></tr><tr><td><code>Change permissions</code></td><td>Users are permitted or denied permissions to change permissions of a file or folder</td></tr><tr><td><code>Take ownership</code></td><td>Users are permitted or denied permission to take ownership of a file or folder. The owner of a file has full permissions to change any permissions</td></tr></tbody></table>

{% hint style="info" %}
_The permissions at the NTFS level provide administrators much more granular control over what users can do within a folder or file._
{% endhint %}

## Introduction to Registry&#x20;

The Windows Registry is a hierarchical database that stores almost every configuration used by Windows.

It contains:

* User preferences
* System configurations

{% hint style="info" %}
**Offensive relevance**\
_The registry is where Windows keeps things it "shouldn't" — cached credentials, autologon passwords, service binary paths, autorun entries, and security policy toggles like UAC behavior. A huge fraction of local privesc on Windows comes down to registry misconfiguration or abuse._
{% endhint %}

### Registry Hives&#x20;

These are the primary registry roots visible in Registry Editor.

<table><thead><tr><th width="109">Hive</th><th width="217.4000244140625">Full Name</th><th>Purpose</th></tr></thead><tbody><tr><td><code>HKLM</code></td><td>HKEY_LOCAL_MACHINE</td><td>Machine-wide config — hardware, services, installed software, security policy</td></tr><tr><td><code>HKCU</code></td><td>HKEY_CURRENT_USER</td><td>Per-user settings for the currently logged-on user (mapped from <code>HKU\&#x3C;SID></code>)</td></tr><tr><td><code>HKU</code></td><td>HKEY_USERS</td><td>All loaded user profile hives, keyed by SID</td></tr><tr><td><code>HKCR</code></td><td>HKEY_CLASSES_ROOT</td><td>File associations, COM class registrations (merged view of <code>HKLM\SOFTWARE\Classes</code> + <code>HKCU\Software\Classes</code>)</td></tr><tr><td><code>HKCC</code></td><td>HKEY_CURRENT_CONFIG</td><td>Active hardware profile (pointer into <code>HKLM\SYSTEM</code>)</td></tr></tbody></table>

### Registry Value Types&#x20;

<table><thead><tr><th width="150.79998779296875">Type</th><th width="506.79998779296875">Meaning</th></tr></thead><tbody><tr><td><code>REG_SZ</code></td><td>Fixed-length string</td></tr><tr><td><code>REG_EXPAND_SZ</code></td><td>String with environment variable references (<code>%SystemRoot%</code>)</td></tr><tr><td><code>REG_MULTI_SZ</code></td><td>Array of strings</td></tr><tr><td><code>REG_DWORD</code></td><td>32-bit number</td></tr><tr><td><code>REG_QWORD</code></td><td>64-bit number</td></tr><tr><td><code>REG_BINARY</code></td><td>Raw binary blob — this is where hashes, secrets, SIDs often live</td></tr></tbody></table>

### Physical Hive files on the disk&#x20;

<table data-search="false"><thead><tr><th width="161.199951171875">Hive</th><th width="487.60009765625">File Location</th></tr></thead><tbody><tr><td><code>HKLM\SAM</code></td><td><code>C:\Windows\System32\config\SAM</code></td></tr><tr><td><code>HKLM\SECURITY</code></td><td><code>C:\Windows\System32\config\SECURITY</code></td></tr><tr><td><code>HKLM\SYSTEM</code></td><td><code>C:\Windows\System32\config\SYSTEM</code></td></tr><tr><td><code>HKLM\SOFTWARE</code></td><td><code>C:\Windows\System32\config\SOFTWARE</code></td></tr><tr><td><code>HKLM\DEFAULT</code></td><td><code>C:\Windows\System32\config\DEFAULT</code> (default user profile)</td></tr><tr><td><code>HKCU</code> (per user)</td><td><code>C:\Users\&#x3C;user>\NTUSER.DAT</code></td></tr><tr><td><code>HKU\&#x3C;SID>_Classes</code></td><td><code>C:\Users\&#x3C;user>\AppData\Local\Microsoft\Windows\UsrClass.dat</code></td></tr></tbody></table>

{% hint style="info" %}
**Offensive relevance**

&#x20;_These files are **locked while Windows is running** (can't just `copy` them). The standard workaround is `reg save` (needs SeBackupPrivilege or local admin) or Volume Shadow Copy. This is exactly how `secretsdump.py` pulls SAM/SECURITY/SYSTEM remotely._
{% endhint %}

### Dumping the hives&#x20;

{% code overflow="wrap" %}
```cmd
# Offline dump of the three hives needed to extract local hashes + LSA secrets
reg save HKLM\SAM C:\temp\sam.hive
reg save HKLM\SYSTEM C:\temp\system.hive
reg save HKLM\SECURITY C:\temp\security.hive
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1432).png" alt=""><figcaption></figcaption></figure>

### Registry Tools&#x20;

<table><thead><tr><th width="270.79998779296875">Tool</th><th>Notes</th></tr></thead><tbody><tr><td><code>regedit.exe</code></td><td>GUI editor, interactive access only</td></tr><tr><td><code>reg.exe</code></td><td>CLI — query, add, save, load, export, delete. Scriptable, works remotely (<code>reg query \\host\...</code>)</td></tr><tr><td>PowerShell <code>Get-ItemProperty</code> / <code>Set-ItemProperty</code></td><td>Registry exposed as a PSDrive (<code>HKLM:</code>, <code>HKCU:</code>)</td></tr><tr><td>Remote Registry service (<a href="../../network-testing-essentials/network-ports-and-services/rpc-111-135-593/"><code>RemoteRegistry</code></a>)</td><td>Enables <code>reg.exe \\target</code> and <code>winreg</code> RPC named pipe access — disabled by default on workstations since Win10, often enabled on servers</td></tr></tbody></table>

### Important Hives&#x20;

#### Credential  Material

<table data-search="false"><thead><tr><th width="249">Registry Key</th><th>What It Holds</th></tr></thead><tbody><tr><td><code>HKLM\SAM</code></td><td>Local user account password hashes (SAM database). Requires <strong>SYSTEM</strong> privileges to access; local Administrator alone cannot read it directly.</td></tr><tr><td><code>HKLM\SECURITY</code></td><td>LSA Secrets such as service account passwords, cached credentials, scheduled task secrets, and AutoLogon credentials (stored in encrypted form).</td></tr><tr><td><code>HKLM\SYSTEM\CurrentControlSet\Control\Lsa</code></td><td>LSA configuration. Contains references to the <strong>Boot Key (SysKey)</strong> components (<code>JD</code>, <code>Skew1</code>, <code>GBG</code>, <code>Data</code>) used to decrypt the SAM and SECURITY hives.</td></tr><tr><td><code>HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon</code></td><td>Stores AutoLogon settings such as <code>DefaultUserName</code>, <code>DefaultPassword</code>, <code>DefaultDomainName</code>, and <code>AutoAdminLogon</code>.</td></tr><tr><td><code>HKCU\Software\SimonTatham\PuTTY\Sessions\*</code></td><td>Saved PuTTY session configuration, including hostnames, usernames, SSH settings, and other connection details (not passwords directly).</td></tr><tr><td><code>HKCU\Software\ORL\WinVNC3\PasswordHKLM\SOFTWARE\RealVNC\WinVNC4\Password</code></td><td>Stores the VNC server password in a weakly encoded format.</td></tr></tbody></table>

#### Security Policy&#x20;

<table data-search="false"><thead><tr><th>Registry Value</th><th>Purpose / Effect</th><th>Pentester Relevance</th></tr></thead><tbody><tr><td><code>HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\LocalAccountTokenFilterPolicy</code> (DWORD)</td><td><code>0</code> (default): Local accounts connecting remotely receive a <strong>filtered token</strong> (no administrative privileges), even if they belong to the <strong>Administrators</strong> group.<code>1</code>: Disables token filtering so remote local Administrator logons receive a <strong>full administrator token</strong>.</td><td>Important for remote administration and Pass-the-Hash attacks. A value of <code>1</code> allows techniques such as WMI, PsExec, and PowerShell Remoting using local administrator credentials to obtain full admin rights.</td></tr><tr><td><code>HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\EnableLUA</code></td><td><code>1</code> (default): User Account Control (UAC) is enabled.<br><code>0</code>: Completely disables UAC.</td><td>Disabling UAC removes an important security boundary and may simplify privilege escalation or remote administrative actions.</td></tr><tr><td><code>HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\ConsentPromptBehaviorAdmin</code></td><td>Controls how Windows prompts administrators for elevation (Always Prompt, Prompt for Consent, Elevate Without Prompt, etc.).</td><td>Determines how difficult it is to elevate privileges through UAC. Useful during privilege escalation assessment.</td></tr><tr><td><code>HKLM\SYSTEM\CurrentControlSet\Control\Lsa\LimitBlankPasswordUse</code></td><td><code>1</code> (default): Blocks network authentication for accounts with blank passwords.<br><code>0</code>: Allows blank-password accounts to authenticate over the network.</td><td>A value of <code>0</code> introduces a significant security weakness and may allow remote authentication using blank passwords.</td></tr><tr><td><code>HKLM\SYSTEM\CurrentControlSet\Control\Lsa\RunAsPPL</code></td><td>Enables <strong>LSASS Protected Process Light (PPL)</strong> when set. Prevents untrusted processes from reading LSASS memory.</td><td>Makes credential dumping significantly more difficult by blocking many traditional LSASS memory dumping techniques.</td></tr><tr><td><code>HKLM\SYSTEM\CurrentControlSet\Control\Lsa\NoLMHash</code></td><td><code>1</code>: Prevents Windows from storing or computing <strong>LM hashes</strong> for newly created passwords.<br><code>0</code>: LM hashes may be stored (legacy behavior).</td><td>LM hashes are weak and easily cracked. A value of <code>1</code> improves password security.</td></tr><tr><td><code>HKLM\SYSTEM\CurrentControlSet\Control\Lsa\pipe\lanmanserver</code></td><td>Contains settings related to SMB named pipes, session handling, and null-session accessibility.</td><td>Relevant when assessing anonymous (null session) access to SMB/MSRPC named pipes and legacy enumeration techniques.</td></tr><tr><td><code>HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest\UseLogonCredential</code></td><td><code>1</code>: Forces <strong>WDigest</strong> to keep plaintext-equivalent credentials in LSASS memory.<code>0</code> (modern default): Plaintext credentials are not retained.</td><td>A value of <code>1</code> greatly increases the impact of credential dumping because plaintext credentials can often be recovered from LSASS memory.</td></tr></tbody></table>

## Authentication Basics&#x20;

### The core players

<table><thead><tr><th width="178.60003662109375">Component</th><th>Role</th><th>Pentester Relevance</th></tr></thead><tbody><tr><td><strong>LSASS (<code>lsass.exe</code>)</strong></td><td>Local Security Authority Subsystem Service. The core authentication process responsible for validating logons, creating access tokens, enforcing local security policy, and storing credential material in memory.</td><td><p>Primary target for credential dumping (e.g., extracting NTLM hashes, Kerberos tickets, or plaintext credentials depending on configuration). </p><ul><li>Stores credentials of remote machine in memory. See SMB Session persistence. </li></ul></td></tr><tr><td><strong>SAM (Security Account Manager)</strong></td><td>Database containing local user accounts, group memberships, and password hashes. Stored on disk in the <strong>SAM</strong> registry hive.</td><td>Source of local NTLM password hashes used for offline cracking or Pass-the-Hash attacks.</td></tr><tr><td><strong>LSA (Local Security Authority)</strong></td><td>Windows security policy engine that works through LSASS. Manages local security policy, audit policy, user rights, and <strong>LSA Secrets</strong>.</td><td>LSA Secrets may contain service account passwords, scheduled task credentials, cached secrets, and other sensitive information.<br><br><strong>Saved chrome passwords.</strong> </td></tr><tr><td><strong>Winlogon (<code>winlogon.exe</code>)</strong></td><td>Handles the interactive logon process (e.g., Ctrl+Alt+Del, credential prompt, lock screen) and passes user credentials to LSASS for authentication.</td><td>Understanding the logon flow helps explain how Windows authenticates users and where credentials are processed.</td></tr><tr><td><strong>Authentication Packages</strong></td><td>DLLs loaded by LSASS to perform authentication using different protocols. Examples include <code>msv1_0.dll</code> (NTLM), <code>wdigest.dll</code>, <code>tspkg.dll</code>, <code>livessp.dll</code>, <code>negotiate.dll</code> (selects NTLM or Kerberos), and <code>kerberos.dll</code> (Active Directory environments).</td><td>Determines which authentication mechanisms are available and what credential material may be present in LSASS memory. <code>WDigest</code> is especially relevant when plaintext credentials are retained.</td></tr></tbody></table>

{% hint style="info" %}
**Offensive relevance**

&#x20;_LSASS memory is the single highest-value target on a Windows box for credential material — it's where plaintext-recoverable creds (if WDigest/tspkg enabled), NTLM hashes, and Kerberos tickets (domain context) all transiently live. Tools like Mimikatz's `sekurlsa::logonpasswords` or a raw `procdump -ma lsass.exe` dump exist specifically to read this process's memory._
{% endhint %}

### The SAM database

Local accounts (non-domain) are stored in `HKLM\SAM`, backed by `C:\Windows\System32\config\SAM` on disk (see \[\[Windows Registry]] for hive-to-file mapping).

Each local account entry stores:

* **RID** (Relative Identifier) — numeric suffix of the SID identifying the account on this machine (well-known RIDs: `500` = built-in Administrator, `501` = built-in Guest, `1000+` = regular created accounts)
* **NT hash** — MD4 of the UTF-16LE password, this is "the" hash used in Pass-the-Hash
* **LM hash** (legacy, disabled by default since Vista via `NoLMHash`) — weak, case-insensitive, split into two 7-char DES-encrypted halves; trivially reversible with rainbow tables when present

SAM itself is encrypted using a key derived from the **boot key**, which is reconstructed from fragments stored across `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\{JD,Skew1,GBG,Data}`. This is why dumping SAM alone is useless — you need SYSTEM too.

### NTLM Authentication Flow (Challenge-Response)

NTLM is a challenge-response protocol — the password (or its hash) never crosses the wire.

1. Client sends `NEGOTIATE_MESSAGE` to server.
2. Server responds with `CHALLENGE_MESSAGE` containing a random 8-byte nonce.
3. Client encrypts the challenge using a key derived from its NT hash, sends back `AUTHENTICATE_MESSAGE`.
4. Server (or a Domain Controller, in domain context) verifies by performing the same computation with its copy of the hash.

**NTLMv1** vs **NTLMv2**: NTLMv2 adds a client nonce, timestamp, and target info into the response computation (HMAC-MD5 based) — much harder to crack/relay than NTLMv1's weaker DES-based scheme. NTLMv1 should be considered broken; if you see it negotiated, it's a red flag worth reporting.

### Credential storage location

<table data-search="false"><thead><tr><th>Location</th><th>What's Stored</th><th>Access Required</th><th>Pentester Relevance</th></tr></thead><tbody><tr><td><code>HKLM\SAM</code></td><td>Local user <strong>NTLM/LM password hashes</strong> (SAM database).</td><td><strong>SYSTEM</strong> privileges are required to read the hive directly.</td><td>Used for offline password cracking and Pass-the-Hash attacks.</td></tr><tr><td><code>HKLM\SECURITY</code> (LSA Secrets)</td><td>Service account passwords, cached credentials, DPAPI-protected AutoLogon secrets, and other LSA secrets.</td><td><strong>SYSTEM</strong> privileges are required.</td><td>Valuable source of credentials during post-exploitation.</td></tr><tr><td><strong>LSASS (<code>lsass.exe</code>) Process Memory</strong></td><td>Live authentication material such as NTLM hashes, plaintext passwords (if WDigest/TSPKG is enabled), Kerberos tickets (on domain-joined systems), and security tokens.</td><td>Administrator with <strong>SeDebugPrivilege</strong>, or SYSTEM. LSASS must not be protected by <strong>PPL (Protected Process Light)</strong>.</td><td>Primary target for credential dumping tools like Mimikatz.</td></tr><tr><td><code>%APPDATA%\Microsoft\Credentials%LOCALAPPDATA%\Microsoft\Credentials</code>Other <strong>DPAPI-protected</strong> credential stores</td><td>User-encrypted secrets including Windows credentials, browser passwords, application secrets, certificates, and other DPAPI-protected blobs.</td><td>User context (owner of the data) or <strong>SYSTEM</strong> (via DPAPI master key extraction).</td><td>Enables recovery of stored credentials after obtaining the user's DPAPI keys.</td></tr><tr><td><strong>Credential Manager (Windows Vault)</strong></td><td>Saved credentials for network shares, RDP connections, web credentials, and other Windows-managed secrets protected by DPAPI.</td><td>User context (or SYSTEM with DPAPI access).</td><td>Frequently contains reusable credentials for lateral movement.</td></tr><tr><td><code>HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon</code></td><td>AutoLogon configuration including <code>DefaultUserName</code>, <code>DefaultPassword</code>, <code>DefaultDomainName</code>, and <code>AutoAdminLogon</code>.</td><td>Standard read permissions for most values.</td><td>May expose <strong>plaintext AutoLogon credentials</strong> if automatic logon is configured.</td></tr></tbody></table>

### LSASS

<figure><img src="../../../.gitbook/assets/image (1639).png" alt=""><figcaption></figcaption></figure>

LSASS ⇒ (Local Security Authority Subsystem Service)

`lsass.exe` located at `%SYSTEMROOT%\System32\Lsass.exe` enforces the local security policy — handles logon, password changes, and **holds credential material in memory** for the current session (plaintext in some configs, NTLM hashes, Kerberos tickets).

Instead, while users are logged in, LSASS may keep in memory:

* NTLM hashes
* Kerberos tickets (TGTs/TGSs)
* Cached credentials
* Sometimes plaintext passwords (depending on authentication providers like WDigest and system configuration)

{% hint style="info" %}
_`lsass.exe` enforces the local security policy — handles logon, password changes, and **holds credential material in memory** for the current session (plaintext in some configs, NTLM hashes, Kerberos tickets)._

_This is the single highest-value process on any Windows box you land on. Dumping LSASS memory (`procdump`, `Task Manager -> Create Dump File`, `comsvcs.dll MiniDump`) and parsing it offline (Mimikatz `sekurlsa::logonpasswords`) is the standard technique to recover cached credentials of every user who has logged on._
{% endhint %}

### Access Tokens

Every process/thread runs under a **token** representing the security context it operates with — this is what actually gets checked against ACLs, not "the user" abstractly.

| Token type              | Meaning                                                                                                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Primary token**       | Assigned to a process at creation, defines the process's default security context                                                                                      |
| **Impersonation token** | Assigned to a thread, lets it act as another (often more privileged) security context temporarily — this is what service/RPC calls use when servicing a client request |

Impersonation levels (weakest to strongest): `Anonymous` → `Identification` → `Impersonation` → `Delegation`. Only `Impersonation` and above let a thread actually _act as_ the client locally.

<table data-search="false"><thead><tr><th width="218.00006103515625">Privilege</th><th>Why it matters offensively</th></tr></thead><tbody><tr><td><code>SeImpersonatePrivilege</code></td><td>Enables the entire Potato exploit family — near-universal service-account-to-SYSTEM path</td></tr><tr><td><code>SeAssignPrimaryTokenPrivilege</code></td><td>Similar impersonation-adjacent abuse potential</td></tr><tr><td><code>SeDebugPrivilege</code></td><td>Lets you open a handle to <em>any</em> process, including LSASS — direct credential-dumping enabler</td></tr><tr><td><code>SeBackupPrivilege</code></td><td>Bypasses read ACLs entirely for backup purposes — lets you read SAM/SYSTEM/NTDS.dit-equivalent files you otherwise couldn't</td></tr><tr><td><code>SeRestorePrivilege</code></td><td>Bypasses write ACLs — lets you overwrite protected files/registry, including planting a service binary or DLL in a protected path</td></tr><tr><td><code>SeTakeOwnershipPrivilege</code></td><td>Lets you take ownership of any object then rewrite its ACL — a path to touching anything on the box</td></tr><tr><td><code>SeLoadDriverPrivilege</code></td><td>Lets you load a kernel driver — full kernel-mode code exec, including loading known-vulnerable signed drivers ("BYOVD")</td></tr></tbody></table>

{% hint style="info" %}
**Enumeration tip**&#x20;

_`whoami /priv` should be one of the very first commands run on any shell. Any of the above privileges present (even if disabled by default state) is worth immediately checking against known local privesc chains — winPEAS flags all of these automatically._
{% endhint %}

### NTDS.dit&#x20;

`NTDS.dit` is a database file that stores Active Directory data, including but not limited to:

* User accounts (username & password hash)
* Group accounts
* Computer accounts
* Group policy objects

## Windows Vault and Credential Manager&#x20;

Credential Manager is a feature built into Windows since `Server 2008 R2` and `Windows 7`.

Credentials are stored in special encrypted folders on the computer under the user and system profiles

* `%UserProfile%\AppData\Local\Microsoft\Vault\`
* `%UserProfile%\AppData\Local\Microsoft\Credentials\`
* `%UserProfile%\AppData\Roaming\Microsoft\Vault\`
* `%ProgramData%\Microsoft\Vault\`
* `%SystemRoot%\System32\config\systemprofile\AppData\Roaming\Microsoft\Vault\`

Each vault folder contains a `Policy.vpol` file with AES keys (AES-128 or AES-256) that is protected by DPAPI. These AES keys are used to encrypt the credentials. Newer versions of Windows make use of `Credential Guard` to further protect the DPAPI master keys by storing them in secured memory enclaves.&#x20;

<table><thead><tr><th width="185.20001220703125">Name</th><th>Description</th></tr></thead><tbody><tr><td>Web Credentials</td><td>Credentials associated with websites and online accounts. This locker is used by Internet Explorer and legacy versions of Microsoft Edge.</td></tr><tr><td>Windows Credentials</td><td>Used to store login tokens for various services such as OneDrive, and credentials related to domain users, local network resources, services, and shared directories.</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1669).png" alt=""><figcaption></figcaption></figure>

### Export Windows Vault&#x20;

{% code overflow="wrap" %}
```
C:\Users\arjun> rundll32 keymgr.dll,KRShowKeyMgr
```
{% endcode %}

It will provide some pop up and then provide things to it and it will save a `.crd` file.&#x20;

<figure><img src="../../../.gitbook/assets/image (1670).png" alt=""><figcaption></figcaption></figure>

Backups created this way are encrypted with a password supplied by the user, and can be imported on other Windows systems.

### Storing interactive credentials of a user&#x20;

{% code overflow="wrap" %}
```cmd
cmdkey /add:"Domain:interactive=bakasura\backup" /user:bakasura\backup /pass:B@$$w0rd
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1675).png" alt=""><figcaption></figcaption></figure>
