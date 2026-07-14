# Windows CMD

## Introduction to CMD

Command Prompt (commonly known as **CMD**) is the traditional **command-line interpreter** included with Windows. It provides a text-based interface that allows users and administrators to interact with the operating system by typing commands instead of using the graphical user interface (GUI).

### CMD Syntax

```
command /switch parameter
```

* Switches use `/` (not `-` like PowerShell/Linux)
* Commands are **case-insensitive** (`DIR` = `dir` = `Dir`)
* Quoting: use double quotes `"path with spaces"` (single quotes don't work as string delimiters in CMD)
* Comments: `REM this is a comment` or `:: this is also a comment` (in batch files)

### Environment Variables&#x20;

Environment variables store system and user configuration. Access them with `%VARIABLE%` syntax.

<table data-search="false"><thead><tr><th width="193.79998779296875">Variable</th><th width="221">Description</th><th width="290.79998779296875">Example Value</th></tr></thead><tbody><tr><td><code>%USERNAME%</code></td><td>Current username</td><td><code>john</code></td></tr><tr><td><code>%COMPUTERNAME%</code></td><td>Computer name</td><td><code>WORKSTATION01</code></td></tr><tr><td><code>%SYSTEMROOT%</code></td><td>Windows directory</td><td><code>C:\Windows</code></td></tr><tr><td><code>%USERPROFILE%</code></td><td>User profile path</td><td><code>C:\Users\john</code></td></tr><tr><td><code>%PATH%</code></td><td>Executable search paths</td><td><code>C:\Windows\System32;...</code></td></tr><tr><td><code>%TEMP%</code> / <code>%TMP%</code></td><td>Temporary directory</td><td><code>C:\Users\john\AppData\Local\Temp</code></td></tr><tr><td><code>%APPDATA%</code></td><td>Roaming app data</td><td><code>C:\Users\john\AppData\Roaming</code></td></tr><tr><td><code>%LOCALAPPDATA%</code></td><td>Local app data</td><td><code>C:\Users\john\AppData\Local</code></td></tr><tr><td><code>%PROGRAMFILES%</code></td><td>Program Files (x64)</td><td><code>C:\Program Files</code></td></tr><tr><td><code>%PROGRAMFILES(X86)%</code></td><td>Program Files (x86)</td><td><code>C:\Program Files (x86)</code></td></tr><tr><td><code>%HOMEDRIVE%</code></td><td>Drive of home directory</td><td><code>C:</code></td></tr><tr><td><code>%HOMEPATH%</code></td><td>Path portion of home</td><td><code>\Users\john</code></td></tr><tr><td><code>%LOGONSERVER%</code></td><td>Authenticating DC</td><td><code>\\DC01</code> or <code>\\WORKSTATION01</code></td></tr><tr><td><code>%USERDOMAIN%</code></td><td>Domain or computer name</td><td><code>WORKGROUP</code> or <code>CORP</code></td></tr><tr><td><code>%OS%</code></td><td>Operating system</td><td><code>Windows_NT</code></td></tr><tr><td><code>%PROCESSOR_ARCHITECTURE%</code></td><td>CPU architecture</td><td><code>AMD64</code> or <code>x86</code></td></tr><tr><td><code>%NUMBER_OF_PROCESSORS%</code></td><td>CPU count</td><td><code>4</code></td></tr><tr><td><code>%COMSPEC%</code></td><td>Path to cmd.exe</td><td><code>C:\Windows\system32\cmd.exe</code></td></tr><tr><td><code>%PATHEXT%</code></td><td>Executable extensions</td><td><code>.COM;.EXE;.BAT;.CMD;...</code></td></tr><tr><td><code>%WINDIR%</code></td><td>Windows directory (same as SYSTEMROOT)</td><td><code>C:\Windows</code></td></tr></tbody></table>

#### View all Environment variable&#x20;

<figure><img src="../../../.gitbook/assets/image (953).png" alt=""><figcaption></figcaption></figure>

#### View single environment variable&#x20;

<figure><img src="../../../.gitbook/assets/image (954).png" alt=""><figcaption></figcaption></figure>

### Command Chaining&#x20;

<table><thead><tr><th width="124.20001220703125">Operator</th><th width="289.4000244140625">Behavior</th><th width="307.20001220703125">Example</th></tr></thead><tbody><tr><td><code>&#x26;</code></td><td>Run sequentially (regardless of success/failure)</td><td><code>dir &#x26; whoami</code></td></tr><tr><td><code>&#x26;&#x26;</code></td><td>Run next only if previous <strong>succeeded</strong> (exit code 0)</td><td><code>ping 192.168.1.1 &#x26;&#x26; echo reachable</code></td></tr><tr><td><code>\|\|</code></td><td>Run next only if previous <strong>failed</strong> (non-zero exit)</td><td><code>ping 10.0.0.1 \|\| echo unreachable</code></td></tr><tr><td><code>( )</code></td><td>Group commands</td><td><code>(echo line1 &#x26; echo line2) > file.txt</code></td></tr></tbody></table>

### Redirection&#x20;

<table data-search="false"><thead><tr><th>Operator</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><code>></code></td><td>Redirect stdout, <strong>overwrite</strong></td><td><code>dir > listing.txt</code></td></tr><tr><td><code>>></code></td><td>Redirect stdout, <strong>append</strong></td><td><code>echo new line >> log.txt</code></td></tr><tr><td><code>&#x3C;</code></td><td>Redirect stdin (input from file)</td><td><code>sort &#x3C; unsorted.txt</code></td></tr><tr><td><code>2></code></td><td>Redirect stderr</td><td><code>dir Z:\ 2> errors.txt</code></td></tr><tr><td><code>2>&#x26;1</code></td><td>Redirect stderr to stdout</td><td><code>dir Z:\ > all.txt 2>&#x26;1</code></td></tr><tr><td><code>>nul</code></td><td>Discard stdout</td><td><code>ping 192.168.1.1 >nul</code></td></tr><tr><td><code>2>nul</code></td><td>Discard stderr</td><td><code>dir Z:\ 2>nul</code></td></tr><tr><td><code>>nul 2>&#x26;1</code></td><td>Discard everything</td><td><code>ping 10.0.0.1 >nul 2>&#x26;1</code></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (955).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (956).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (957).png" alt=""><figcaption></figcaption></figure>

### Pipes&#x20;

The `|` operator passes the **text output** of one command as **text input** to the next.

<figure><img src="../../../.gitbook/assets/image (958).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (959).png" alt=""><figcaption></figcaption></figure>

### Wildcards&#x20;

<table><thead><tr><th width="116.20001220703125">Wildcard</th><th width="241.4000244140625">Matches</th><th width="275.99993896484375">Example</th></tr></thead><tbody><tr><td><code>*</code></td><td>Any number of characters</td><td><code>*.txt</code> matches all .txt files</td></tr><tr><td><code>?</code></td><td>Exactly one character</td><td><code>file?.txt</code> matches <code>file1.txt</code>, <code>fileA.txt</code></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (960).png" alt=""><figcaption></figcaption></figure>

### Command History and Alias&#x20;

* `F7` ⇒ Show command history popup&#x20;
* `doskey /history` ⇒ List command history

<figure><img src="../../../.gitbook/assets/image (961).png" alt=""><figcaption></figcaption></figure>

#### Create macros&#x20;

{% code overflow="wrap" %}
```
doskey ls=dir $*
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (962).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
doskey ls=dir $*
doskey cls=cls
doskey ll=dir /a $*
```
{% endcode %}

## Navigation and File Management&#x20;

### Get Current Directory&#x20;

{% code overflow="wrap" %}
```
:: Display current directory
cd
echo %CD%
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### List files and directories&#x20;

The most-used CMD command is `dir`

<table data-search="false"><thead><tr><th width="122.800048828125">Switch</th><th width="577.9998779296875">Description</th></tr></thead><tbody><tr><td><code>/a</code></td><td>Show all attributes. <code>/a:h</code> hidden, <code>/a:d</code> directories, <code>/a:s</code> system, <code>/a:r</code> read-only, <code>/a:-d</code> files only</td></tr><tr><td><code>/s</code></td><td>Recursive (subdirectories)</td></tr><tr><td><code>/q</code></td><td>Show file owner</td></tr><tr><td><code>/r</code></td><td>Show Alternate Data Streams (ADS)</td></tr><tr><td><code>/b</code></td><td>Bare format (names only, no headers/footers)</td></tr><tr><td><code>/t</code></td><td>Time field. <code>/t:c</code> creation, <code>/t:a</code> access, <code>/t:w</code> write</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

### Copy and Move&#x20;

{% code overflow="wrap" %}
```
copy <source> <destination>
move <source> <destination>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### Rename a file&#x20;

{% code overflow="wrap" %}
```
move <file> <rename file> 
ren <file> <rename file>  
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

### Delete a file

<table><thead><tr><th width="178.79998779296875">Switch</th><th width="405.20001220703125">Description</th></tr></thead><tbody><tr><td><code>/f</code></td><td>Force delete read-only files</td></tr><tr><td><code>/s</code></td><td>Delete from subdirectories too</td></tr><tr><td><code>/q</code></td><td>Quiet mode</td></tr><tr><td><code>/a</code></td><td>Delete by attribute. <code>/a:h</code> hidden, <code>/a:r</code> read-only</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

### Searching&#x20;

#### where&#x20;

Locates executables on the system PATH or in specified directories.

| Switch | Description                       |
| ------ | --------------------------------- |
| `/r`   | Recursive search from a directory |
| `/q`   | Quiet mode (just set ERRORLEVEL)  |
| `/f`   | Surround output with quotes       |
| `/t`   | Show file size and timestamp      |

{% code overflow="wrap" %}
```
:: Find where an executable is
where cmd.exe
where notepad.exe
where python.exe

:: Recursive search for a file
where /r C:\ password.txt
where /r C:\Users *.kdbx

:: Show size and date
where /r C:\ /t *.exe

:: Check if a program exists (scripting)
where /q python.exe && echo Python found || echo Python not found
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

#### find&#x20;

Searches for text strings in files. Simple but limited.

| Switch | Description                                 |
| ------ | ------------------------------------------- |
| `/i`   | Case-insensitive                            |
| `/c`   | Display count of matching lines             |
| `/n`   | Display line numbers                        |
| `/v`   | Display lines that DON'T contain the string |

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

#### findstr&#x20;

Advanced string searching with regex support. CMD's `grep`.

<table data-search="false"><thead><tr><th>Switch</th><th>Description</th></tr></thead><tbody><tr><td><code>/i</code></td><td>Case-insensitive</td></tr><tr><td><code>/s</code></td><td>Search subdirectories</td></tr><tr><td><code>/r</code></td><td>Regex mode (default)</td></tr><tr><td><code>/c:"string"</code></td><td>Literal search string (spaces included)</td></tr><tr><td><code>/b</code></td><td>Match at beginning of line</td></tr><tr><td><code>/e</code></td><td>Match at end of line</td></tr><tr><td><code>/m</code></td><td>Print only filenames with matches</td></tr><tr><td><code>/n</code></td><td>Print line numbers</td></tr><tr><td><code>/v</code></td><td>Print lines that DON'T match</td></tr><tr><td><code>/l</code></td><td>Literal match (no regex)</td></tr><tr><td><code>/x</code></td><td>Print lines that match exactly</td></tr></tbody></table>

{% code overflow="wrap" %}
```
:: Basic search in file
findstr "password" C:\Temp\config.txt
findstr /i "error warning critical" C:\Temp\log.txt

:: Recursive search for passwords in files
findstr /si "password" C:\Users\*.txt C:\Users\*.xml C:\Users\*.ini C:\Users\*.cfg C:\Users\*.config

:: Literal string with spaces
findstr /c:"access denied" C:\Temp\log.txt

:: Regex: find IP addresses
findstr /r "[0-9][0-9]*\.[0-9][0-9]*\.[0-9][0-9]*\.[0-9][0-9]*" C:\Temp\log.txt

:: Show only filenames with matches
findstr /m /si "password" C:\Users\*.txt

:: Line numbers
findstr /n "error" C:\Temp\log.txt

:: Search through command output
netstat -ano | findstr "LISTENING"
tasklist | findstr /i "chrome firefox"
```
{% endcode %}

**Find specific string out of unknown file**&#x20;

{% code overflow="wrap" %}
```
findstr /si <text> <possible location> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

**using dir and findstr**&#x20;

{% code overflow="wrap" %}
```
dir /s /b | findstr /i <text file> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

## Users and Group&#x20;

### whoami&#x20;

Displays the current user context. Works identically in CMD and PowerShell.

<table data-search="false"><thead><tr><th>Switch</th><th>Description</th></tr></thead><tbody><tr><td>(none)</td><td>Just username (DOMAIN\user)</td></tr><tr><td><code>/all</code></td><td>Everything — user, SID, groups, privileges</td></tr><tr><td><code>/user</code></td><td>Username and SID</td></tr><tr><td><code>/groups</code></td><td>All group memberships</td></tr><tr><td><code>/priv</code></td><td>All privileges and their status</td></tr><tr><td><code>/logonid</code></td><td>Logon ID</td></tr><tr><td><code>/fqdn</code></td><td>Fully qualified domain name</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

### Net User&#x20;

Manages local user accounts.

{% code overflow="wrap" %}
```
:: List all local users
net user

:: Details for a specific user
net user Administrator

:: Details for current user
net user %USERNAME%

:: Add a user (requires admin)
net user hacker P@ssw0rd123 /add

:: Add user to Administrators group
net localgroup Administrators hacker /add

:: Delete a user
net user hacker /delete

:: Set password for existing user
net user Administrator NewP@ssw0rd

:: Domain users (if domain-joined)
net user /domain
net user Administrator /domain
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

### net localgroup&#x20;

Manages local groups.

```
:: List all local groups
net localgroup

:: List members of a group
net localgroup Administrators
net localgroup "Remote Desktop Users"
net localgroup "Remote Management Users"
net localgroup "Backup Operators"

:: Add user to group (requires admin)
net localgroup Administrators hacker /add

:: Remove user from group
net localgroup Administrators hacker /delete
```

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

### Net Accounts (Password and lockout policy)&#x20;

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

### quser (query user)&#x20;

Shows logged-in users and their session information.

```
quser
query user

:: Remote machine (if accessible)
quser /server:COMPUTER01
```

<figure><img src="../../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

## System Enumeration&#x20;

### hostname&#x20;

{% code overflow="wrap" %}
```
hostname
echo %COMPUTERNAME%
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

### systeminfo&#x20;

Comprehensive system information. **One of the most important enumeration commands.**

{% code overflow="wrap" %}
```
systeminfo
systeminfo | more              :: Paginated

:: Quick key info extraction
systeminfo | findstr /i "os name version architecture domain hotfix"
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

### identify windows version&#x20;

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

## Process Management&#x20;

### tasklist&#x20;

<table data-search="false"><thead><tr><th width="149.20001220703125">Switch</th><th width="403.60003662109375">Description</th></tr></thead><tbody><tr><td><code>/v</code></td><td>Verbose (includes status, username, CPU time)</td></tr><tr><td><code>/fo</code></td><td>Format: <code>TABLE</code>, <code>LIST</code>, <code>CSV</code></td></tr><tr><td><code>/fi</code></td><td>Filter by criteria</td></tr><tr><td><code>/svc</code></td><td>Show services hosted by each process</td></tr><tr><td><code>/m</code></td><td>Show loaded modules (DLLs)</td></tr><tr><td><code>/nh</code></td><td>No header</td></tr><tr><td><code>/s</code></td><td>Remote computer</td></tr><tr><td><code>/u</code></td><td>Username for remote</td></tr><tr><td><code>/p</code></td><td>Password for remote</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (963).png" alt=""><figcaption></figcaption></figure>

### Taskkill

Terminates processes.

| Switch | Description                           |
| ------ | ------------------------------------- |
| `/pid` | Kill by PID                           |
| `/im`  | Kill by image name                    |
| `/f`   | Force kill                            |
| `/t`   | Kill process tree (parent + children) |
| `/fi`  | Kill by filter criteria               |

{% code overflow="wrap" %}
```
taskkill /im notepad.exe
taskkill /pid 1234 /f
taskkill /im chrome.exe /f /t     :: Kill Chrome and all child processes
taskkill /fi "username eq SYSTEM" /f    :: Kill all SYSTEM processes (dangerous!)
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (964).png" alt=""><figcaption></figcaption></figure>

## Service Enumeration&#x20;

### sc query

Queries service status.

```
:: All services
sc query

:: All services (including stopped)
sc query state= all

:: Specific service
sc query WinRM

:: Active services only
sc query state= active

:: Inactive services only
sc query state= inactive

:: Type filter
sc query type= service         :: Win32 services
sc query type= driver          :: Drivers
```

<figure><img src="../../../.gitbook/assets/image (965).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (966).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (967).png" alt=""><figcaption></figcaption></figure>

### sc qc&#x20;

Queries service configuration — shows the **binary path**, start type, and run-as account.

{% code overflow="wrap" %}
```
sc qc WinRM
sc qc Spooler
sc qc wuauserv
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (968).png" alt=""><figcaption></figcaption></figure>

### Start and stop a service (sc)

{% code overflow="wrap" %}
```
sc start WinRM
sc stop Spooler
sc pause SomeService
sc continue SomeService
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (969).png" alt=""><figcaption></figcaption></figure>

### Create new service and modify

<figure><img src="../../../.gitbook/assets/image (970).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_:: Note: You'll get an error "service did not respond" when you start the service.— that's normal_&#x20;

_:: because cmd.exe isn't a proper service binary, but the command DOES execute_
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (971).png" alt=""><figcaption></figcaption></figure>

### Start and stop a service (net)

{% code overflow="wrap" %}
```
:: List all running services
net start

:: Start/stop a service by display name
net start "Windows Remote Management (WS-Management)"
net stop Spooler
net start Spooler
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (972).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (973).png" alt=""><figcaption></figcaption></figure>

## Scheduled Tasks&#x20;

### schtasks /query&#x20;

<figure><img src="../../../.gitbook/assets/image (974).png" alt=""><figcaption></figcaption></figure>

### schtasks /create&#x20;

{% code overflow="wrap" %}
```
:: Run a command at startup as SYSTEM
schtasks /create /tn "MyTask" /tr "C:\Temp\payload.exe" /sc onstart /ru SYSTEM

:: Run a command once
schtasks /create /tn "OnceTask" /tr "C:\Temp\script.bat" /sc once /st 14:00

:: Run daily
schtasks /create /tn "DailyTask" /tr "C:\Temp\script.bat" /sc daily /st 03:00

:: Run every 5 minutes
schtasks /create /tn "FreqTask" /tr "cmd /c whoami > C:\Temp\who.txt" /sc minute /mo 5
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (975).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Note that we need a readymade executable so that we can use it._&#x20;
{% endhint %}

### schtasks modify&#x20;

{% code overflow="wrap" %}
```
:: Change task command
schtasks /change /tn "MyTask" /tr "C:\Temp\new_payload.exe"

:: Run task immediately
schtasks /run /tn "MyTask"

:: Stop running task
schtasks /end /tn "MyTask"

:: Delete task
schtasks /delete /tn "MyTask" /f
```
{% endcode %}

## Firewall&#x20;

`netsh` is a command-line tool for network configuration.

### Firewall Commands&#x20;

| Command                                                                                                 | Description                |
| ------------------------------------------------------------------------------------------------------- | -------------------------- |
| `netsh advfirewall show allprofiles`                                                                    | Show all firewall profiles |
| `netsh advfirewall set allprofiles state off`                                                           | Disable firewall (noisy!)  |
| `netsh advfirewall firewall show rule name=all`                                                         | Show all firewall rules    |
| `netsh advfirewall firewall add rule name="Allow 4444" dir=in action=allow protocol=tcp localport=4444` | Add inbound rule           |
| `netsh advfirewall firewall delete rule name="Allow 4444"`                                              | Delete a rule              |

{% code overflow="wrap" %}
```
:: Show firewall status
netsh advfirewall show allprofiles

:: Show all rules
netsh advfirewall firewall show rule name=all | more

:: Add inbound rule for reverse shell
netsh advfirewall firewall add rule name="Windows Update" dir=in action=allow protocol=tcp localport=4444

:: Disable firewall (very noisy — last resort)
netsh advfirewall set allprofiles state off

:: WLAN profiles (WiFi passwords!)
netsh wlan show profiles
netsh wlan show profile name="WiFiNetworkName" key=clear    :: Shows WiFi password!

:: Export all WiFi passwords
for /f "tokens=2 delims=:" %a in ('netsh wlan show profiles ^| find "Profile"') do @netsh wlan show profile name=%a key=clear 2>nul | find "Key Content"

:: Interface configuration
netsh interface show interface
netsh interface ip show config

:: Port proxy (port forwarding)
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=192.168.1.100
netsh interface portproxy show all
netsh interface portproxy delete v4tov4 listenport=8080 listenaddress=0.0.0.0
```
{% endcode %}

## Registry

### reg query

Queries registry keys and values.

<table data-search="false"><thead><tr><th>Switch</th><th>Description</th></tr></thead><tbody><tr><td><code>/v</code></td><td>Query specific value name</td></tr><tr><td><code>/ve</code></td><td>Query default (unnamed) value</td></tr><tr><td><code>/s</code></td><td>Recurse into subkeys</td></tr><tr><td><code>/f</code></td><td>Find a string in keys/values/data</td></tr><tr><td><code>/t</code></td><td>Filter by type (REG_SZ, REG_DWORD, etc.)</td></tr><tr><td><code>/d</code></td><td>Search in data only</td></tr><tr><td><code>/k</code></td><td>Search in key names only</td></tr><tr><td><code>/e</code></td><td>Return exact matches only</td></tr></tbody></table>

```cmd
:: Query a key (show all values)
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

:: Query specific value
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v SecurityHealth

:: Recursive query
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /s

:: Search for "password" in registry data
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s

:: Search for specific key names
reg query HKLM /f "Run" /k /s
```

***

### reg add

Adds or modifies registry keys and values.

**Value Types:**

| Type            | Description                  | Example                |
| --------------- | ---------------------------- | ---------------------- |
| `REG_SZ`        | String                       | `Hello World`          |
| `REG_DWORD`     | 32-bit number                | `1`                    |
| `REG_QWORD`     | 64-bit number                | `1`                    |
| `REG_EXPAND_SZ` | Expandable string (env vars) | `%SystemRoot%\cmd.exe` |
| `REG_MULTI_SZ`  | Multi-line string            | `Line1\0Line2`         |
| `REG_BINARY`    | Binary data                  | `01020304`             |

```cmd
:: Add a string value
reg add HKCU\SOFTWARE\TestKey /v TestValue /t REG_SZ /d "Hello World" /f

:: Add a DWORD value
reg add HKCU\SOFTWARE\TestKey /v TestDWORD /t REG_DWORD /d 1 /f

:: Add to Run key (persistence!)
reg add HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v Backdoor /t REG_SZ /d "C:\Temp\payload.exe" /f

:: Create a key (no value)
reg add HKCU\SOFTWARE\NewKey
```

***

### reg delete

Deletes registry keys or values.

| Switch | Description              |
| ------ | ------------------------ |
| `/v`   | Delete specific value    |
| `/ve`  | Delete default value     |
| `/va`  | Delete all values in key |
| `/f`   | Force (no prompt)        |

```cmd
reg delete HKCU\SOFTWARE\TestKey /v TestValue /f     :: Delete value
reg delete HKCU\SOFTWARE\TestKey /f                  :: Delete key and all values
reg delete HKCU\SOFTWARE\TestKey /va /f              :: Delete all values, keep key
```

***

### reg export / reg import

```cmd
:: Export to .reg file
reg export HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run C:\Temp\run_backup.reg

:: Import from .reg file
reg import C:\Temp\run_backup.reg
```

***

### reg save / reg load / reg unload

For saving and mounting registry hives. Requires privileges.

```cmd
:: Save a hive to file (requires admin/SYSTEM)
reg save HKLM\SAM C:\Temp\sam.bak
reg save HKLM\SYSTEM C:\Temp\system.bak
reg save HKLM\SECURITY C:\Temp\security.bak

:: Load a hive file
reg load HKLM\TempHive C:\Temp\sam.bak

:: Unload
reg unload HKLM\TempHive
```

Pentester Note

`reg save HKLM\SAM` + `reg save HKLM\SYSTEM` — These files contain local user hashes. Extract them and crack offline with tools like `secretsdump.py`, `samdump2`, or `mimikatz`.

***

### reg compare

Compares two registry keys.

```cmd
reg compare HKLM\SOFTWARE\Key1 HKLM\SOFTWARE\Key2
reg compare HKLM\SOFTWARE\Key1 HKLM\SOFTWARE\Key2 /v ValueName
```

***

### Important Registry Locations for Pentesters

| Registry Path                                                | Purpose                             |
| ------------------------------------------------------------ | ----------------------------------- |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`         | Machine startup programs            |
| `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`         | User startup programs               |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`     | Run once at startup                 |
| `HKLM\SYSTEM\CurrentControlSet\Services`                     | All service configurations          |
| `HKLM\SAM\SAM`                                               | Local user hashes (requires SYSTEM) |
| `HKLM\SECURITY`                                              | Security policies (requires SYSTEM) |
| `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon` | Auto-logon credentials              |
| `HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer`         | AlwaysInstallElevated               |
| `HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer`         | AlwaysInstallElevated (user)        |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`   | Installed programs                  |
| `HKLM\SYSTEM\CurrentControlSet\Control\Lsa`                  | LSA protection config               |
| `HKCU\SOFTWARE\SimonTatham\PuTTY\Sessions`                   | Saved PuTTY sessions                |
| `HKCU\SOFTWARE\ORL\WinVNC3\Password`                         | VNC passwords                       |

```cmd
:: Check auto-logon credentials
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUserName 2>nul
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultPassword 2>nul

:: Check AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated 2>nul
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated 2>nul

:: Check Run keys
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
reg query HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

:: Check for saved PuTTY sessions
reg query HKCU\SOFTWARE\SimonTatham\PuTTY\Sessions /s 2>nul

:: Search for passwords in registry
reg query HKLM /f password /t REG_SZ /s 2>nul
reg query HKCU /f password /t REG_SZ /s 2>nul
```

