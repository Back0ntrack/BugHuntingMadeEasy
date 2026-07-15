# Windows PowerShell

## Introduction to PowerShell&#x20;

PowerShell is a task automation framework built on .NET. Unlike CMD which passes text between commands, PowerShell passes **objects** — structured data with properties and methods.&#x20;

| Feature           | CMD                | PowerShell                                 |
| ----------------- | ------------------ | ------------------------------------------ |
| Data type         | Plain text         | .NET objects                               |
| Scripting         | Batch files (.bat) | Scripts (.ps1), modules (.psm1)            |
| Remote execution  | Limited (PsExec)   | Built-in (WinRM, PSRemoting)               |
| Output formatting | Manual parsing     | Select, Where, Format cmdlets              |
| Extensibility     | External tools     | Modules, .NET classes, COM objects         |
| Logging           | Minimal            | ScriptBlock, Module, Transcription logging |

### PowerShell Version&#x20;

{% code overflow="wrap" %}
```powershell
$PSVersionTable
$PSVersionTable.PSVersion
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Most targets run **5.1** (Windows PowerShell). Version 2.0 can be invoked for **downgrade attacks** to bypass logging: `powershell -Version 2` (if .NET 2.0 is installed)._
{% endhint %}

### Execution Policy&#x20;

#### Check current execution policy

Execution policy controls which scripts can run. It is **NOT a security boundary** — it's a safety net to prevent accidental script execution.

{% code overflow="wrap" %}
```powershell
# Check current policy
Get-ExecutionPolicy
Get-ExecutionPolicy -List    # Shows all scopes
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="179.5999755859375">Policy</th><th width="501.2000732421875">Description</th></tr></thead><tbody><tr><td><strong>Restricted</strong></td><td>No scripts can run. Interactive commands only. Default on Windows clients</td></tr><tr><td><strong>AllSigned</strong></td><td>Only scripts signed by a trusted publisher can run</td></tr><tr><td><strong>RemoteSigned</strong></td><td>Downloaded scripts need a signature. Local scripts run freely. Default on servers</td></tr><tr><td><strong>Unrestricted</strong></td><td>All scripts run. Prompts for downloaded scripts</td></tr><tr><td><strong>Bypass</strong></td><td>Nothing is blocked, no warnings or prompts</td></tr><tr><td><strong>Undefined</strong></td><td>No policy set at this scope. Inherits from parent</td></tr></tbody></table>

#### Setting Execution Policy&#x20;

{% code overflow="wrap" %}
```powershell
Set-ExecutionPolicy RemoteSigned                     # Machine-wide (needs admin)
Set-ExecutionPolicy Bypass -Scope CurrentUser        # User-level only
Set-ExecutionPolicy Bypass -Scope Process            # This session only
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### PowerShell Profile&#x20;

A **PowerShell profile** is a script that PowerShell automatically executes every time a new PowerShell session starts.

It is commonly used to:

* Create aliases
* Define custom functions
* Set environment variables
* Customize the PowerShell prompt
* Import frequently used modules

Every time PowerShell starts, these commands run automatically.

You can check the profile location using:

```
$PROFILE
```

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

#### \`PowerShell -NoProfile\`&#x20;

This starts PowerShell **without loading any profile scripts**.

So:

* No aliases
* No custom functions
* No prompt customization
* No automatically imported user modules

It gives a "clean" PowerShell environment.

This is useful because:

* Scripts behave consistently on different systems.
* Startup is slightly faster.
* You avoid problems caused by a user's customized profile.

### Cmdlet Syntax

PowerShell uses a **Verb-Noun** naming convention. This makes cmdlets predictable.

{% code overflow="wrap" %}
```ps
Verb-Noun -ParameterName ParameterValue
```
{% endcode %}

#### Common Verbs

<table data-search="false"><thead><tr><th>Verb</th><th>Purpose</th><th>Example</th></tr></thead><tbody><tr><td><code>Get</code></td><td>Retrieve data</td><td><code>Get-Process</code>, <code>Get-Service</code></td></tr><tr><td><code>Set</code></td><td>Modify existing</td><td><code>Set-Location</code>, <code>Set-Content</code></td></tr><tr><td><code>New</code></td><td>Create something</td><td><code>New-Item</code>, <code>New-Object</code></td></tr><tr><td><code>Remove</code></td><td>Delete something</td><td><code>Remove-Item</code>, <code>Remove-Variable</code></td></tr><tr><td><code>Start</code></td><td>Begin an operation</td><td><code>Start-Process</code>, <code>Start-Service</code></td></tr><tr><td><code>Stop</code></td><td>End an operation</td><td><code>Stop-Process</code>, <code>Stop-Service</code></td></tr><tr><td><code>Restart</code></td><td>Stop then start</td><td><code>Restart-Service</code>, <code>Restart-Computer</code></td></tr><tr><td><code>Test</code></td><td>Check/validate</td><td><code>Test-Path</code>, <code>Test-NetConnection</code></td></tr><tr><td><code>Invoke</code></td><td>Execute an action</td><td><code>Invoke-WebRequest</code>, <code>Invoke-Command</code></td></tr><tr><td><code>Export</code></td><td>Write to external format</td><td><code>Export-Csv</code>, <code>Export-Clixml</code></td></tr><tr><td><code>Import</code></td><td>Read from external format</td><td><code>Import-Csv</code>, <code>Import-Module</code></td></tr><tr><td><code>ConvertTo</code></td><td>Transform output format</td><td><code>ConvertTo-Json</code>, <code>ConvertTo-Html</code></td></tr><tr><td><code>ConvertFrom</code></td><td>Parse input format</td><td><code>ConvertFrom-Json</code>, <code>ConvertFrom-Csv</code></td></tr><tr><td><code>Out</code></td><td>Send output somewhere</td><td><code>Out-File</code>, <code>Out-GridView</code></td></tr><tr><td><code>Write</code></td><td>Write to streams</td><td><code>Write-Output</code>, <code>Write-Host</code>, <code>Write-Error</code></td></tr><tr><td><code>Select</code></td><td>Choose/filter properties</td><td><code>Select-Object</code>, <code>Select-String</code></td></tr><tr><td><code>Where</code></td><td>Filter objects</td><td><code>Where-Object</code></td></tr><tr><td><code>ForEach</code></td><td>Iterate over objects</td><td><code>ForEach-Object</code></td></tr><tr><td><code>Measure</code></td><td>Calculate statistics</td><td><code>Measure-Object</code></td></tr><tr><td><code>Sort</code></td><td>Order objects</td><td><code>Sort-Object</code></td></tr><tr><td><code>Group</code></td><td>Group objects</td><td><code>Group-Object</code></td></tr><tr><td><code>Compare</code></td><td>Compare objects</td><td><code>Compare-Object</code></td></tr><tr><td><code>Format</code></td><td>Format output display</td><td><code>Format-Table</code>, <code>Format-List</code></td></tr></tbody></table>

{% code overflow="wrap" %}
```
# List all approved verbs
Get-Verb

# Find all cmdlets with a specific verb
Get-Command -Verb Get
Get-Command -Noun Process
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Aliases

PowerShell supports aliases — short names that map to full cmdlet names. Many mimic Linux/CMD commands.

{% code overflow="wrap" %}
```powershell
# List all aliases
Get-Alias

# Find alias for a cmdlet
Get-Alias -Definition Get-ChildItem

# Find what an alias maps to
Get-Alias ls

# Create alias (session only)
Set-Alias -Name ll -Value Get-ChildItem # Create or modify an alias
New-Alias -Name np -Value notepad.exe   # Create a brand new alias. can't modify
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

#### Common Aliases&#x20;

<table data-search="false"><thead><tr><th>Alias</th><th>Cmdlet</th><th>Linux Equivalent</th></tr></thead><tbody><tr><td><code>ls</code>, <code>dir</code>, <code>gci</code></td><td><code>Get-ChildItem</code></td><td><code>ls</code></td></tr><tr><td><code>cd</code>, <code>chdir</code>, <code>sl</code></td><td><code>Set-Location</code></td><td><code>cd</code></td></tr><tr><td><code>pwd</code>, <code>gl</code></td><td><code>Get-Location</code></td><td><code>pwd</code></td></tr><tr><td><code>cat</code>, <code>type</code>, <code>gc</code></td><td><code>Get-Content</code></td><td><code>cat</code></td></tr><tr><td><code>cp</code>, <code>copy</code>, <code>cpi</code></td><td><code>Copy-Item</code></td><td><code>cp</code></td></tr><tr><td><code>mv</code>, <code>move</code>, <code>mi</code></td><td><code>Move-Item</code></td><td><code>mv</code></td></tr><tr><td><code>rm</code>, <code>del</code>, <code>ri</code></td><td><code>Remove-Item</code></td><td><code>rm</code></td></tr><tr><td><code>mkdir</code>, <code>md</code></td><td><code>New-Item -ItemType Directory</code></td><td><code>mkdir</code></td></tr><tr><td><code>echo</code>, <code>write</code></td><td><code>Write-Output</code></td><td><code>echo</code></td></tr><tr><td><code>cls</code>, <code>clear</code></td><td><code>Clear-Host</code></td><td><code>clear</code></td></tr><tr><td><code>ps</code>, <code>gps</code></td><td><code>Get-Process</code></td><td><code>ps</code></td></tr><tr><td><code>kill</code>, <code>spps</code></td><td><code>Stop-Process</code></td><td><code>kill</code></td></tr><tr><td><code>curl</code>, <code>wget</code>, <code>iwr</code></td><td><code>Invoke-WebRequest</code></td><td><code>curl</code>/<code>wget</code></td></tr><tr><td><code>select</code></td><td><code>Select-Object</code></td><td>—</td></tr><tr><td><code>where</code>, <code>?</code></td><td><code>Where-Object</code></td><td>—</td></tr><tr><td><code>%</code>, <code>foreach</code></td><td><code>ForEach-Object</code></td><td>—</td></tr><tr><td><code>sort</code></td><td><code>Sort-Object</code></td><td><code>sort</code></td></tr><tr><td><code>measure</code></td><td><code>Measure-Object</code></td><td><code>wc</code></td></tr><tr><td><code>ft</code></td><td><code>Format-Table</code></td><td>—</td></tr><tr><td><code>fl</code></td><td><code>Format-List</code></td><td>—</td></tr><tr><td><code>sls</code></td><td><code>Select-String</code></td><td><code>grep</code></td></tr><tr><td><code>sc</code></td><td><code>Set-Content</code></td><td>—</td></tr><tr><td><code>oh</code>, <code>out-host</code></td><td><code>Out-Host</code></td><td>—</td></tr></tbody></table>

### Help System&#x20;

#### First Update the Help system&#x20;

{% code overflow="wrap" %}
```powershell
Update-Help -Force -ErrorAction SilentlyContinue
```
{% endcode %}

#### take Help

{% code overflow="wrap" %}
```powershell
# Get help for a cmdlet
Get-Help Get-Process
Get-Help Get-Process -Full          # Complete help with all parameters
Get-Help Get-Process -Examples      # Just the examples
Get-Help Get-Process -Online        # Open in browser
Get-Help Get-Process -Parameter Name   # Help for specific parameter
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## Pipeline&#x20;

{% code overflow="wrap" %}
```
Get-Process | Select-Object Name, CPU
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

## Navigation&#x20;

### Get-Location&#x20;

Displays the current working directory. Equivalent to `pwd` in Linux or `cd` with no arguments in CMD.

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

### Set-Location

Changes the current working directory. Equivalent to `cd`.

| Parameter      | Description                                                                                                                |
| -------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `-Path`        | Path to navigate to                                                                                                        |
| `-LiteralPath` | Use this path exactly as I typed it. Don't treat any characters as wildcards or special pattern characters. (E.g., Test\*) |
| `-PassThru`    | Return the new location object                                                                                             |

<figure><img src="../../../.gitbook/assets/image (1456).png" alt=""><figcaption></figcaption></figure>

### Get-ChildItem&#x20;

Lists files and directories. Equivalent to `ls` (Linux) or `dir` (CMD).

<table data-search="false"><thead><tr><th>Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>-Path</code></td><td>Directory to list</td></tr><tr><td><code>-Recurse</code></td><td>Include subdirectories</td></tr><tr><td><code>-Filter</code></td><td>Filter by name pattern (faster than -Include)</td></tr><tr><td><code>-Include</code></td><td>Include pattern (requires -Recurse or trailing <code>\*</code>)</td></tr><tr><td><code>-Exclude</code></td><td>Exclude pattern</td></tr><tr><td><code>-Force</code></td><td>Show hidden and system files</td></tr><tr><td><code>-Hidden</code></td><td>Show only hidden items</td></tr><tr><td><code>-File</code></td><td>Show only files</td></tr><tr><td><code>-Directory</code></td><td>Show only directories</td></tr><tr><td><code>-Depth</code></td><td>Recursion depth limit</td></tr><tr><td><code>-Name</code></td><td>Output names only (strings, not objects)</td></tr></tbody></table>

{% code overflow="wrap" %}
```powershell
# Basic listing
Get-ChildItem
Get-ChildItem C:\Users
ls                                 # Alias

# Recursive search for files
Get-ChildItem -Path C:\ -Recurse -Filter "*.txt" -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\ -Recurse -Include "*.log","*.txt" -ErrorAction SilentlyContinue

# Show hidden files
Get-ChildItem -Force
Get-ChildItem -Hidden

# Only files or only directories
Get-ChildItem -File
Get-ChildItem -Directory

# Limit recursion depth
Get-ChildItem -Recurse -Depth 2

# Find large files
Get-ChildItem -Recurse -File | Where-Object {$_.Length -gt 100MB} | Select-Object FullName, @{N='SizeMB';E={[math]::Round($_.Length/1MB,2)}}

# Find recently modified files
Get-ChildItem -Recurse -File | Where-Object {$_.LastWriteTime -gt (Get-Date).AddDays(-7)} | Select-Object FullName, LastWriteTime
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1459).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1458).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1460).png" alt=""><figcaption></figcaption></figure>

### New-Item

