# WinRM (5985)

## Introduction to WinRM

**Windows Remote Management (WinRM)** is **Microsoft's implementation of the WS-Management (WS-Man) protocol**, designed to allow administrators to **remotely manage Windows computers by executing commands, running PowerShell sessions, and performing administrative tasks over a network**.

### What WinRM enables

* Remote PowerShell sessions (`Enter-PSSession`, `New-PSSession`)
* Fan-out command execution across many hosts (`Invoke-Command`)
* Remote WMI/CIM queries (`Get-CimInstance -ComputerName`)
* Remote Server Manager / Event Viewer / Computer Management consoles
* Configuration management tooling (DSC, Ansible's `winrm` connection plugin, SCCM/MECM remote actions)
* Cross-platform management, since WS-Man is an open standard (Linux tools like `pywinrm`, Ansible can talk to Windows over WinRM )

## Introduction to PowerShell Remoting&#x20;

WinRM is a communication protocol/service that allows applications to remotely manage Windows. PowerShell Remoting is one application that uses WinRM to provide a remote PowerShell session.

### Common Cmdlets&#x20;

| Cmdlet             | Purpose                     |
| ------------------ | --------------------------- |
| `Enter-PSSession`  | Interactive remote session  |
| `Exit-PSSession`   | Leave remote session        |
| `Invoke-Command`   | Execute commands remotely   |
| `New-PSSession`    | Create a persistent session |
| `Get-PSSession`    | List active sessions        |
| `Remove-PSSession` | Close sessions              |

<figure><img src="../../../.gitbook/assets/image (1527).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1528).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1529).png" alt=""><figcaption></figcaption></figure>

## Enable WinRM&#x20;

### Using quickconfig

{% code overflow="wrap" %}
```
winrm quickconfig
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1515).png" alt=""><figcaption></figcaption></figure>

### Using PSRemoting&#x20;

{% code overflow="wrap" %}
```powershell
Enable-PSRemoting -Force 
# It will do all the things
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1525).png" alt=""><figcaption></figcaption></figure>

### Set Connection Profile&#x20;

{% code overflow="wrap" %}
```
Set-NetConnectionProfile -NetworkCategory Private
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1518).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```powershell
winrm quickconfig
# we need to run this in order to 
# 1. create firewall rule so remote machines can reach WinRM. 
# 2. LocalAccountTokenFilterPolicy=1 allows remote logons using local administrator account to receive a full administrator token instead of filtered one which is default behavior of windows. 
```
{% endcode %}

### WinRM Checks and Changes&#x20;

Think of it as an automated setup script. It performs these checks and changes:

<table><thead><tr><th width="115.5999755859375">Step</th><th width="591.5999755859375">Action</th></tr></thead><tbody><tr><td>1</td><td>Starts the <strong>WinRM</strong> service if it isn't already running.</td></tr><tr><td>2</td><td>Sets the WinRM service startup type to <strong>Automatic (Delayed Start)</strong> if needed.</td></tr><tr><td>3</td><td>Creates or verifies a <strong>WinRM listener</strong> (typically HTTP on TCP 5985).</td></tr><tr><td>4</td><td>Enables the required <strong>Windows Firewall</strong> rule so remote systems can reach WinRM.</td></tr><tr><td>5</td><td>Configures <strong>LocalAccountTokenFilterPolicy</strong> (when prompted) so local administrator accounts can perform remote administrative tasks in workgroup scenarios.</td></tr></tbody></table>

### Check if it is running&#x20;

{% code overflow="wrap" %}
```
winrm enumerate winrm/config/listener
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1516).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```powershell
Get-Service WinRM
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1517).png" alt=""><figcaption></figcaption></figure>

## Connecting to WinRM&#x20;

### In Windows

#### 1. Test WinRM Connectivity&#x20;

* `Test-NetConnection 192.168.16.132 -Port 5985`
* `Test-WSMan 192.168.16.132`

<figure><img src="../../../.gitbook/assets/image (1519).png" alt=""><figcaption></figcaption></figure>

#### 2. Authentication without Kerberos

PowerShell always tries to authenticate using **Kerberos**.

Kerberos requires:

* Client and server to be in the same Active Directory domain (or trusted domains).
* The server to have a valid Service Principal Name (SPN).

My setup is:

```
Windows 10 (WinRM Client)   <------->   Windows 10 (WinRM Server)

Workgroup                               Workgroup
```

There is **no domain**, so Kerberos cannot be used.

PowerShell therefore says:

> If you're not using Kerberos, either:
>
> * use HTTPS, or
> * add the server to TrustedHosts.

#### 3. Add Server to TrustedHosts&#x20;

When you connect by **IP address**:

```
192.168.16.132
```

Windows cannot verify the identity of the remote machine.

To prevent man-in-the-middle attacks, PowerShell blocks the connection unless you explicitly trust that host.

1. Setup the `winrm quickconfig` from the client machine as well. As windows require WinRM to be enabled for configuring the TrustedHosts. do the same thing as done in the first 2 steps done in server.&#x20;
2. Check the current `TrustedHosts`: `get-Item WSMan:\localhosts\Client\TrustedHosts`

<figure><img src="../../../.gitbook/assets/image (1521).png" alt=""><figcaption></figcaption></figure>

3. Set the `TrustedHosts`:&#x20;

* `Set-Item WSMan:\localhost\Client\TrustedHosts -Value "<IP>"`
* `winrm set winrm/config/client @{TrustedHosts="192.168.16.132"}`

<figure><img src="../../../.gitbook/assets/image (1522).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1523).png" alt=""><figcaption></figcaption></figure>

4. Connect to the machine:&#x20;

{% code overflow="wrap" %}
```powershell
$cred = Get-Credential # Put the credentials here 
Enter-PSSession -ComputerName <IP> -Credential $cred
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1524).png" alt=""><figcaption></figcaption></figure>

#### Connecting to WinRM using CMD&#x20;

{% code overflow="wrap" %}
```
winrs -r:192.168.16.132 -u:arjun -p:P@$$w0rd cmd
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1526).png" alt=""><figcaption></figcaption></figure>

### In Linux

{% code overflow="wrap" %}
```bash
evil-winrm -u arjun -p 'P@$$w0rd' -i 192.168.1.73
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1520).png" alt=""><figcaption></figcaption></figure>

