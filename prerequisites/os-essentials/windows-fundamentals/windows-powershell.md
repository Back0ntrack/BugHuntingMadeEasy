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

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="179.5999755859375">Policy</th><th width="501.2000732421875">Description</th></tr></thead><tbody><tr><td><strong>Restricted</strong></td><td>No scripts can run. Interactive commands only. Default on Windows clients</td></tr><tr><td><strong>AllSigned</strong></td><td>Only scripts signed by a trusted publisher can run</td></tr><tr><td><strong>RemoteSigned</strong></td><td>Downloaded scripts need a signature. Local scripts run freely. Default on servers</td></tr><tr><td><strong>Unrestricted</strong></td><td>All scripts run. Prompts for downloaded scripts</td></tr><tr><td><strong>Bypass</strong></td><td>Nothing is blocked, no warnings or prompts</td></tr><tr><td><strong>Undefined</strong></td><td>No policy set at this scope. Inherits from parent</td></tr></tbody></table>

#### Setting Execution Policy&#x20;

{% code overflow="wrap" %}
```powershell
Set-ExecutionPolicy RemoteSigned                     # Machine-wide (needs admin)
Set-ExecutionPolicy Bypass -Scope CurrentUser        # User-level only
Set-ExecutionPolicy Bypass -Scope Process            # This session only
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

#### Common Aliases&#x20;

<table data-search="false"><thead><tr><th>Alias</th><th>Cmdlet</th><th>Linux Equivalent</th></tr></thead><tbody><tr><td><code>ls</code>, <code>dir</code>, <code>gci</code></td><td><code>Get-ChildItem</code></td><td><code>ls</code></td></tr><tr><td><code>cd</code>, <code>chdir</code>, <code>sl</code></td><td><code>Set-Location</code></td><td><code>cd</code></td></tr><tr><td><code>pwd</code>, <code>gl</code></td><td><code>Get-Location</code></td><td><code>pwd</code></td></tr><tr><td><code>cat</code>, <code>type</code>, <code>gc</code></td><td><code>Get-Content</code></td><td><code>cat</code></td></tr><tr><td><code>cp</code>, <code>copy</code>, <code>cpi</code></td><td><code>Copy-Item</code></td><td><code>cp</code></td></tr><tr><td><code>mv</code>, <code>move</code>, <code>mi</code></td><td><code>Move-Item</code></td><td><code>mv</code></td></tr><tr><td><code>rm</code>, <code>del</code>, <code>ri</code></td><td><code>Remove-Item</code></td><td><code>rm</code></td></tr><tr><td><code>mkdir</code>, <code>md</code></td><td><code>New-Item -ItemType Directory</code></td><td><code>mkdir</code></td></tr><tr><td><code>echo</code>, <code>write</code></td><td><code>Write-Output</code></td><td><code>echo</code></td></tr><tr><td><code>cls</code>, <code>clear</code></td><td><code>Clear-Host</code></td><td><code>clear</code></td></tr><tr><td><code>ps</code>, <code>gps</code></td><td><code>Get-Process</code></td><td><code>ps</code></td></tr><tr><td><code>kill</code>, <code>spps</code></td><td><code>Stop-Process</code></td><td><code>kill</code></td></tr><tr><td><code>curl</code>, <code>wget</code>, <code>iwr</code></td><td><code>Invoke-WebRequest</code></td><td><code>curl</code>/<code>wget</code></td></tr><tr><td><code>select</code></td><td><code>Select-Object</code></td><td>—</td></tr><tr><td><code>where</code>, <code>?</code></td><td><code>Where-Object</code></td><td>—</td></tr><tr><td><code>%</code>, <code>foreach</code></td><td><code>ForEach-Object</code></td><td>—</td></tr><tr><td><code>sort</code></td><td><code>Sort-Object</code></td><td><code>sort</code></td></tr><tr><td><code>measure</code></td><td><code>Measure-Object</code></td><td><code>wc</code></td></tr><tr><td><code>ft</code></td><td><code>Format-Table</code></td><td>—</td></tr><tr><td><code>fl</code></td><td><code>Format-List</code></td><td>—</td></tr><tr><td><code>sls</code></td><td><code>Select-String</code></td><td><code>grep</code></td></tr><tr><td><code>sc</code></td><td><code>Set-Content</code></td><td>—</td></tr><tr><td><code>oh</code>, <code>out-host</code></td><td><code>Out-Host</code></td><td>—</td></tr></tbody></table>

### Environment variable&#x20;

#### List all environment variable&#x20;

{% code overflow="wrap" %}
```
gci env:
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1465).png" alt=""><figcaption></figcaption></figure>

#### List specific environment variable&#x20;

{% code overflow="wrap" %}
```
Write-Output $env:USERNAME
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1464).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

## Pipeline&#x20;

{% code overflow="wrap" %}
```
Get-Process | Select-Object Name, CPU
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

## Navigation&#x20;

### Get-Location&#x20;

Displays the current working directory. Equivalent to `pwd` in Linux or `cd` with no arguments in CMD.

<figure><img src="../../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

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

Creates new files or directories.

| Parameter   | Description                                                 |
| ----------- | ----------------------------------------------------------- |
| `-Path`     | Where to create the item                                    |
| `-Name`     | Name of the new item                                        |
| `-ItemType` | `File`, `Directory`, `SymbolicLink`, `HardLink`, `Junction` |
| `-Value`    | Initial content (for files)                                 |
| `-Force`    | Overwrite if exists / create parent directories             |

{% code overflow="wrap" %}
```powershell
# Create a directory
New-Item -Path C:\Temp\TestDir -ItemType Directory

# Create a file with content
New-Item -Path C:\Temp\test.txt -ItemType File -Value "Hello World"

# Create nested directories
New-Item -Path C:\Temp\a\b\c -ItemType Directory -Force
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Copy-Item&#x20;

Copies files and directories.

<table data-search="false"><thead><tr><th>Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>-Path</code></td><td>Source path</td></tr><tr><td><code>-Destination</code></td><td>Destination path</td></tr><tr><td><code>-Recurse</code></td><td>Copy directories recursively</td></tr><tr><td><code>-Force</code></td><td>Overwrite read-only files</td></tr><tr><td><code>-Filter</code></td><td>Filter by name pattern</td></tr><tr><td><code>-Exclude</code></td><td>Exclude pattern</td></tr><tr><td><code>-PassThru</code></td><td>Return copied item objects</td></tr></tbody></table>

{% code overflow="wrap" %}
```powershell
Copy-Item -Path C:\Temp\file.txt -Destination C:\Backup\
Copy-Item -Path C:\Temp\Dir -Destination C:\Backup\Dir -Recurse
Copy-Item -Path C:\Temp\*.log -Destination C:\Backup\
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### Move-Item&#x20;

Moves files and directories (also used for renaming).

| Parameter      | Description              |
| -------------- | ------------------------ |
| `-Path`        | Source path              |
| `-Destination` | Destination path         |
| `-Force`       | Overwrite existing items |

{% code overflow="wrap" %}
```powershell
Move-Item -Path C:\Temp\file.txt -Destination C:\Backup\
Move-Item -Path C:\Temp\old_name.txt -Destination C:\Temp\new_name.txt  # Rename
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Remove-Item

Deletes files and directories.

| Parameter  | Description                     |
| ---------- | ------------------------------- |
| `-Path`    | Item to remove                  |
| `-Recurse` | Remove directories and contents |
| `-Force`   | Remove hidden/read-only items   |
| `-WhatIf`  | Preview what would be deleted   |
| `-Confirm` | Prompt before deleting          |

{% code overflow="wrap" %}
```
Remove-Item -Path C:\Temp\file.txt
Remove-Item -Path C:\Temp\Dir -Recurse -Force
Remove-Item -Path C:\Temp\*.log -WhatIf       # Preview deletion
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### Rename-Item&#x20;

Renames a file or directory.

```
Rename-Item -Path C:\Temp\old.txt -NewName new.txt
Get-ChildItem *.txt | Rename-Item -NewName { $_.Name -replace 'old','new' }  # Bulk rename
```

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

## File Operations&#x20;

### Get-Content&#x20;

Reads the content of a file. Equivalent to `cat` (Linux) or `type` (CMD).

<table data-search="false"><thead><tr><th>Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>-Path</code></td><td>File to read</td></tr><tr><td><code>-TotalCount</code> / <code>-Head</code></td><td>Read first N lines</td></tr><tr><td><code>-Tail</code></td><td>Read last N lines (like <code>tail</code>)</td></tr><tr><td><code>-Raw</code></td><td>Read entire file as single string (not line-by-line)</td></tr><tr><td><code>-Encoding</code></td><td>File encoding (UTF8, ASCII, Unicode, etc.)</td></tr><tr><td><code>-Wait</code></td><td>Keep reading as file grows (like <code>tail -f</code>)</td></tr><tr><td><code>-ReadCount</code></td><td>Number of lines to send through pipeline at once</td></tr></tbody></table>

```powershell
Get-Content C:\Temp\file.txt
Get-Content C:\Temp\file.txt -TotalCount 10    # First 10 lines
Get-Content C:\Temp\file.txt -Tail 20          # Last 20 lines
Get-Content C:\Temp\log.txt -Wait              # Live tail (Ctrl+C to stop)
Get-Content C:\Temp\file.txt -Raw              # Entire file as one string
cat C:\Temp\file.txt                           # Alias

# Count lines
(Get-Content C:\Temp\file.txt).Count

# Read specific line
(Get-Content C:\Temp\file.txt)[4]              # 5th line (zero-indexed)
```

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

### Set-Content&#x20;

Writes content to a file, **overwriting** existing content.

| Parameter    | Description                  |
| ------------ | ---------------------------- |
| `-Path`      | File to write                |
| `-Value`     | Content to write             |
| `-Encoding`  | Output encoding              |
| `-Force`     | Create file if doesn't exist |
| `-NoNewline` | Don't add newline at end     |

```powershell
Set-Content -Path C:\Temp\output.txt -Value "Hello World"
"Line 1", "Line 2", "Line 3" | Set-Content C:\Temp\output.txt
Get-Process | Out-String | Set-Content C:\Temp\processes.txt
```

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

### Add-Content&#x20;

Appends content to a file (like `>>` redirection).

```powershell
Add-Content -Path C:\Temp\log.txt -Value "New log entry: $(Get-Date)"
"Another line" | Add-Content C:\Temp\log.txt
```

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

### Out-File

Sends output to a file. Similar to `>` redirection but with more control.

| Parameter    | Description                                               |
| ------------ | --------------------------------------------------------- |
| `-FilePath`  | Output file path                                          |
| `-Append`    | Append instead of overwrite                               |
| `-Encoding`  | Output encoding                                           |
| `-Width`     | Line width (default 80, use larger to prevent truncation) |
| `-NoClobber` | Don't overwrite existing file                             |
| `-Force`     | Overwrite read-only files                                 |

```powershell
Get-Process | Out-File -FilePath C:\Temp\processes.txt
Get-Process | Out-File C:\Temp\processes.txt -Width 200   # Prevent truncation
Get-Service | Out-File C:\Temp\services.txt -Append
```

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Set-Content vs Out-File**

_`Out-File` preserves the PowerShell formatting (what you see on screen). `Set-Content` writes raw data. For most pentesting output, `Out-File` is better._
{% endhint %}

### Select-String (Search in file)

Searches for text patterns in files. Equivalent to `grep` in Linux.

| Parameter        | Description                                 |
| ---------------- | ------------------------------------------- |
| `-Pattern`       | Regex pattern to search for                 |
| `-Path`          | File(s) to search                           |
| `-SimpleMatch`   | Treat pattern as literal string (not regex) |
| `-CaseSensitive` | Case-sensitive matching                     |
| `-NotMatch`      | Return lines that DON'T match               |
| `-Context`       | Show N lines before/after match             |
| `-AllMatches`    | Return all matches per line                 |
| `-List`          | Return only first match per file            |
| `-Quiet`         | Return false only                           |

```powershell
# Search a single file
Select-String -Path C:\Temp\log.txt -Pattern "error"

# Search recursively (like grep -r)
Get-ChildItem -Recurse -Filter "*.txt" | Select-String -Pattern "password"

# Case-sensitive search
Select-String -Path C:\Temp\config.txt -Pattern "admin" -CaseSensitive

# Show context (lines around match)
Select-String -Path C:\Temp\log.txt -Pattern "failed" -Context 2,2

# Literal string search (no regex)
Select-String -Path C:\Temp\file.txt -Pattern "192.168.1.1" -SimpleMatch

# Search for multiple patterns
Select-String -Path C:\Temp\*.txt -Pattern "password|secret|credential"

# Inverse match (lines NOT matching)
Select-String -Path C:\Temp\log.txt -Pattern "INFO" -NotMatch

# Alias
sls "error" C:\Temp\log.txt
```

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

**Find `password` inside a file by searching recursively**&#x20;

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### Get-FileHash

alculates the hash of a file. Essential for integrity verification.

| Parameter    | Description    |
| ------------ | -------------- |
| `-Path`      | File to hash   |
| `-Algorithm` | Hash algorithm |

| Algorithm | Description                             |
| --------- | --------------------------------------- |
| `SHA256`  | Default. 256-bit hash                   |
| `SHA1`    | 160-bit. Considered weak                |
| `SHA384`  | 384-bit hash                            |
| `SHA512`  | 512-bit hash                            |
| `MD5`     | 128-bit. Considered broken for security |

```powershell
Get-FileHash C:\Windows\System32\cmd.exe
Get-FileHash C:\Temp\file.exe -Algorithm MD5
Get-FileHash C:\Temp\file.exe -Algorithm SHA1

# Hash all files in a directory
Get-ChildItem -File | ForEach-Object { Get-FileHash $_.FullName -Algorithm SHA256 }

# Compare hashes
(Get-FileHash file1.exe).Hash -eq (Get-FileHash file2.exe).Hash
```

<figure><img src="../../../.gitbook/assets/image (1462).png" alt=""><figcaption></figcaption></figure>

## Filtering and Pipeline&#x20;

### $\_

`$_` is one of the **most important variables in PowerShell**. It represents the **current object** being processed in a pipeline.

It represents each object at a time.&#x20;

E.g., while using `gci` each file properties is an object for that file.&#x20;

### Where-Object&#x20;

Filters objects in the pipeline based on conditions. Alias: `?` or `where`.

**Comparison Operators:**

<table data-search="false"><thead><tr><th>Operator</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><code>-eq</code></td><td>Equal</td><td><code>$_.Name -eq "svchost"</code></td></tr><tr><td><code>-ne</code></td><td>Not equal</td><td><code>$_.Status -ne "Running"</code></td></tr><tr><td><code>-gt</code></td><td>Greater than</td><td><code>$_.CPU -gt 100</code></td></tr><tr><td><code>-lt</code></td><td>Less than</td><td><code>$_.Length -lt 1MB</code></td></tr><tr><td><code>-ge</code></td><td>Greater or equal</td><td><code>$_.Id -ge 1000</code></td></tr><tr><td><code>-le</code></td><td>Less or equal</td><td><code>$_.Count -le 5</code></td></tr><tr><td><code>-like</code></td><td>Wildcard match</td><td><code>$_.Name -like "*svc*"</code></td></tr><tr><td><code>-notlike</code></td><td>Wildcard non-match</td><td><code>$_.Name -notlike "system*"</code></td></tr><tr><td><code>-match</code></td><td>Regex match</td><td><code>$_.Name -match "^svc"</code></td></tr><tr><td><code>-notmatch</code></td><td>Regex non-match</td><td><code>$_.Path -notmatch "windows"</code></td></tr><tr><td><code>-contains</code></td><td>Collection contains</td><td><code>$arr -contains "item"</code></td></tr><tr><td><code>-notcontains</code></td><td>Collection doesn't contain</td><td><code>$arr -notcontains "item"</code></td></tr><tr><td><code>-in</code></td><td>Value in collection</td><td><code>$_.Status -in @("Running","Paused")</code></td></tr><tr><td><code>-notin</code></td><td>Value not in collection</td><td><code>$_.Name -notin $exclude</code></td></tr><tr><td><code>-not</code> / <code>!</code></td><td>Logical NOT</td><td><code>-not (Test-Path C:\file)</code></td></tr><tr><td><code>-and</code></td><td>Logical AND</td><td><code>$_.CPU -gt 10 -and $_.Name -ne "idle"</code></td></tr><tr><td><code>-or</code></td><td>Logical OR</td><td><code>$_.Status -eq "Stopped" -or $_.StartType -eq "Disabled"</code></td></tr></tbody></table>

{% code overflow="wrap" %}
```powershell
# Script block syntax (PS 2.0+)
Get-Process | Where-Object { $_.CPU -gt 10 }
Get-Service | Where-Object { $_.Status -eq 'Running' }

# Simplified syntax (PS 3.0+)
Get-Process | Where-Object CPU -gt 10
Get-Service | Where-Object Status -eq Running

# Multiple conditions
Get-Process | Where-Object { $_.CPU -gt 5 -and $_.WorkingSet64 -gt 100MB }

# Regex matching
Get-Process | Where-Object { $_.Name -match '^svc' }

# Alias
Get-Process | ? { $_.CPU -gt 10 }
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1466).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1467).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1468).png" alt=""><figcaption></figcaption></figure>

### Select-Object

Selects specific properties from objects or a subset of objects. Alias: `select`.

<table data-search="false"><thead><tr><th>Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>-Property</code></td><td>Properties to select</td></tr><tr><td><code>-First</code></td><td>First N objects</td></tr><tr><td><code>-Last</code></td><td>Last N objects</td></tr><tr><td><code>-Skip</code></td><td>Skip first N objects</td></tr><tr><td><code>-Unique</code></td><td>Remove duplicates</td></tr><tr><td><code>-ExpandProperty</code></td><td>Expand a single property (unwrap collection)</td></tr><tr><td><code>-ExcludeProperty</code></td><td>Properties to exclude</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1469).png" alt=""><figcaption></figcaption></figure>

## User and Group Enumeration&#x20;

### whoami&#x20;

Displays current user context. Works in both CMD and PowerShell.

```powershell
# check in cmd for the same
```

### Get-LocalUser

<figure><img src="../../../.gitbook/assets/image (1470).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1471).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1472).png" alt=""><figcaption></figcaption></figure>

### Get-LocalGroup&#x20;

Enumerates local groups.

```powershell
# List all local groups
Get-LocalGroup

# Get specific group
Get-LocalGroup -Name "Administrators"
```

<figure><img src="../../../.gitbook/assets/image (1473).png" alt=""><figcaption></figcaption></figure>

### Get-LocalGroupMember&#x20;

<figure><img src="../../../.gitbook/assets/image (1474).png" alt=""><figcaption></figcaption></figure>

## System Enumeration&#x20;

### Get-ComputerInfo&#x20;

{% code overflow="wrap" %}
```
# Everything (very long output)
Get-ComputerInfo

# Specific properties
Get-ComputerInfo | Select-Object CsName, OsName, OsVersion, OsArchitecture, OsBuildNumber, WindowsVersion, OsHotFixes

# OS info
Get-ComputerInfo | Select-Object Os*

# Hardware info
Get-ComputerInfo | Select-Object Cs*

# Check if domain joined
Get-ComputerInfo | Select-Object CsDomainRole, CsDomain, CsPartOfDomain
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1475).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1476).png" alt=""><figcaption></figcaption></figure>

### Get-CimInstance

The modern way to query Windows Management Instrumentation. Replaces the deprecated `Get-WmiObject`.

**Common Win32 Classes:**

<table data-search="false"><thead><tr><th>Class</th><th>What It Returns</th><th>Key Properties</th></tr></thead><tbody><tr><td><code>Win32_OperatingSystem</code></td><td>OS version, build, architecture</td><td>Caption, Version, BuildNumber, OSArchitecture, LastBootUpTime</td></tr><tr><td><code>Win32_ComputerSystem</code></td><td>Hostname, domain, manufacturer</td><td>Name, Domain, Manufacturer, Model, TotalPhysicalMemory</td></tr><tr><td><code>Win32_BIOS</code></td><td>BIOS serial and manufacturer</td><td>SerialNumber, Manufacturer, Version</td></tr><tr><td><code>Win32_LogicalDisk</code></td><td>Drives and free space</td><td>DeviceID, Size, FreeSpace, FileSystem, VolumeName</td></tr><tr><td><code>Win32_Process</code></td><td>Running processes</td><td>ProcessId, Name, CommandLine, ParentProcessId, ExecutablePath</td></tr><tr><td><code>Win32_NetworkAdapterConfiguration</code></td><td>Network config</td><td>IPAddress, MACAddress, DNSServerSearchOrder, DHCPEnabled</td></tr><tr><td><code>Win32_QuickFixEngineering</code></td><td>Installed patches/hotfixes</td><td>HotFixID, InstalledOn, Description</td></tr><tr><td><code>Win32_Share</code></td><td>Network shares</td><td>Name, Path, Description, Type</td></tr><tr><td><code>Win32_StartupCommand</code></td><td>Startup programs</td><td>Name, Command, Location, User</td></tr><tr><td><code>Win32_Product</code></td><td>Installed software</td><td>Name, Version, Vendor</td></tr></tbody></table>

{% code overflow="wrap" %}
```powershell
# Operating System
Get-CimInstance Win32_OperatingSystem | Select-Object Caption, Version, BuildNumber, OSArchitecture, LastBootUpTime

# Computer System
Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, Manufacturer, Model, @{N='RAM_GB';E={[math]::Round($_.TotalPhysicalMemory/1GB,2)}}

# BIOS
Get-CimInstance Win32_BIOS | Select-Object SerialNumber, Manufacturer, Version

# Logical Disks
Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID, FileSystem, @{N='SizeGB';E={[math]::Round($_.Size/1GB,2)}}, @{N='FreeGB';E={[math]::Round($_.FreeSpace/1GB,2)}}

# Running Processes with command lines
Get-CimInstance Win32_Process | Select-Object ProcessId, Name, CommandLine | Format-Table -AutoSize -Wrap

# Installed hotfixes
Get-CimInstance Win32_QuickFixEngineering | Select-Object HotFixID, InstalledOn, Description | Sort-Object InstalledOn -Descending

# Installed software
Get-CimInstance Win32_Product | Select-Object Name, Version, Vendor | Sort-Object Name

# Startup programs
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location, User

# Network shares
Get-CimInstance Win32_Share | Select-Object Name, Path, Description
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1477).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1478).png" alt=""><figcaption></figcaption></figure>

## Process Management

### Get-process

Lists running processes with detailed information.

| Parameter          | Description                                |
| ------------------ | ------------------------------------------ |
| `-Name`            | Filter by process name (no .exe extension) |
| `-Id`              | Filter by PID                              |
| `-IncludeUserName` | Show owning user (requires admin)          |
| `-Module`          | Show loaded modules                        |
| `-FileVersionInfo` | Show file version details                  |

{% code overflow="wrap" %}
```powershell
# List all processes
Get-Process

# Specific process
Get-Process -Name "svchost"
Get-Process -Id 1234

# With username (needs admin)
Get-Process -IncludeUserName | Select-Object Id, UserName, ProcessName, Path | Format-Table -AutoSize

# Top CPU consumers
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, Id, CPU, @{N='MemMB';E={[math]::Round($_.WorkingSet64/1MB,2)}}

# Find processes with specific string in name
Get-Process | Where-Object { $_.Name -like "*chrome*" }

# Process with path (find sus binaries)
Get-Process | Where-Object { $_.Path -and $_.Path -notlike "C:\Windows\*" -and $_.Path -notlike "C:\Program Files*" } | Select-Object Name, Id, Path

# Loaded modules for a process
Get-Process -Name "explorer" -Module

# Process command lines (CIM is better for this)
Get-CimInstance Win32_Process | Where-Object { $_.CommandLine } | Select-Object ProcessId, Name, CommandLine | Format-Table -Wrap
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1479).png" alt=""><figcaption></figcaption></figure>

### Stop-Process&#x20;

Terminates a running process.&#x20;

| Parameter   | Description                             |
| ----------- | --------------------------------------- |
| `-Name`     | Kill by name                            |
| `-Id`       | Kill by PID                             |
| `-Force`    | Force termination                       |
| `-WhatIf`   | Preview without killing                 |
| `-PassThru` | <p></p><p>Return the process object</p> |

{% code overflow="wrap" %}
```powershell
Stop-Process -Name "notepad"
Stop-Process -Id 1234 -Force
Get-Process -Name "notepad" | Stop-Process
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1480).png" alt=""><figcaption></figcaption></figure>

### Start-Process&#x20;

Starts a new process.

<table data-search="false"><thead><tr><th>Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>-FilePath</code></td><td>Program to run</td></tr><tr><td><code>-ArgumentList</code></td><td>Command-line arguments</td></tr><tr><td><code>-Verb RunAs</code></td><td>Run as administrator (UAC prompt)</td></tr><tr><td><code>-NoNewWindow</code></td><td>Run in current console window</td></tr><tr><td><code>-Wait</code></td><td>Wait for process to exit</td></tr><tr><td><code>-RedirectStandardOutput</code></td><td>Redirect stdout to file</td></tr><tr><td><code>-RedirectStandardError</code></td><td>Redirect stderr to file</td></tr><tr><td><code>-PassThru</code></td><td>Return the process object</td></tr><tr><td><code>-WindowStyle</code></td><td>Hidden, Minimized, Maximized, Normal</td></tr><tr><td><code>-WorkingDirectory</code></td><td>Starting directory</td></tr></tbody></table>

{% code overflow="wrap" %}
```powershell
Start-Process notepad.exe
Start-Process cmd.exe -ArgumentList "/c dir C:\" -NoNewWindow -Wait
Start-Process powershell.exe -Verb RunAs                              # Elevate
Start-Process cmd.exe -WindowStyle Hidden -ArgumentList "/c whoami > C:\Temp\out.txt"  # Hidden
Start-Process -FilePath "ping" -ArgumentList "127.0.0.1" -RedirectStandardOutput C:\Temp\ping.txt -NoNewWindow -Wait
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1481).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1482).png" alt=""><figcaption></figcaption></figure>

## Service Management

### Get-Service&#x20;

Lists Windows services and their status.

| Parameter            | Description                                 |
| -------------------- | ------------------------------------------- |
| `-Name`              | Filter by service name                      |
| `-DisplayName`       | Filter by display name (supports wildcards) |
| `-DependentServices` | Services that depend on this one            |
| `-RequiredServices`  | Services this one depends on                |

```powershell
# All services
Get-Service

# Running services only
Get-Service | Where-Object Status -eq Running

# Stopped services
Get-Service | Where-Object Status -eq Stopped

# Specific service
Get-Service -Name "WinRM"
Get-Service -DisplayName "*remote*"

# Service dependencies
Get-Service -Name "WinRM" -RequiredServices
Get-Service -Name "WinRM" -DependentServices
```

<figure><img src="../../../.gitbook/assets/image (1483).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1484).png" alt=""><figcaption></figcaption></figure>

### Set-Service&#x20;

Modifies service configuration.

| Parameter      | Description                                                |
| -------------- | ---------------------------------------------------------- |
| `-Name`        | Service name                                               |
| `-StartupType` | `Automatic`, `Manual`, `Disabled`, `AutomaticDelayedStart` |
| `-Status`      | `Running`, `Stopped`, `Paused`                             |
| `-Description` | Service description                                        |
| `-DisplayName` | Display name                                               |

```powershell
Set-Service -Name "Spooler" -StartupType Disabled
Set-Service -Name "WinRM" -StartupType Automatic -Status Running
```

<figure><img src="../../../.gitbook/assets/image (1485).png" alt=""><figcaption></figcaption></figure>

### Get-CimInstance Win32\_Service&#x20;

Provides much more detail than Get-Service — including the **binary path** (critical for privilege escalation).

{% code overflow="wrap" %}
```powershell
# All services with paths
Get-CimInstance Win32_Service | Select-Object Name, State, StartMode, PathName, StartName | Format-Table -AutoSize -Wrap

# Find services running as SYSTEM
Get-CimInstance Win32_Service | Where-Object { $_.StartName -like "*LocalSystem*" } | Select-Object Name, PathName, State | Format-Table -Wrap

# Find unquoted service paths (privesc vector!)
Get-CimInstance Win32_Service | Where-Object { $_.PathName -and $_.PathName -notlike '"*' -and $_.PathName -notlike 'C:\Windows\*' -and $_.PathName -match '.* .*' } | Select-Object Name, PathName, StartName | Format-Table -Wrap

# Find services NOT in standard directories (potential hijack)
Get-CimInstance Win32_Service | Where-Object { $_.PathName -and $_.PathName -notlike "C:\Windows\*" -and $_.PathName -notlike "C:\Program Files*" } | Select-Object Name, PathName, StartName | Format-Table -Wrap
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1486).png" alt=""><figcaption></figcaption></figure>

## Networkin

### Get-NetIPAddress

Shows IP address configuration.

{% code overflow="wrap" %}
```powershell
Get-NetIPAddress
Get-NetIPAddress -AddressFamily IPv4                    # IPv4 only
Get-NetIPAddress -InterfaceAlias "Ethernet*"            # Specific adapter
Get-NetIPAddress -AddressFamily IPv4 | Select-Object InterfaceAlias, IPAddress, PrefixLength | Format-Table
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1487).png" alt=""><figcaption></figcaption></figure>

### Get-NetIPConfiguration

Shows IP configuration (similar to `ipconfig`).

{% code overflow="wrap" %}
```powershell
Get-NetIPConfiguration
Get-NetIPConfiguration -Detailed                       # Full config with DNS, DHCP, etc.
Get-NetIPConfiguration | Select-Object InterfaceAlias, IPv4Address, IPv4DefaultGateway, DNSServer
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1488).png" alt=""><figcaption></figcaption></figure>

### Test-NetConnection

Tests network connectivity (ping + port check). Replacement for `ping` and `telnet`.

| Parameter           | Description                           |
| ------------------- | ------------------------------------- |
| `-ComputerName`     | Target host                           |
| `-Port`             | TCP port to test                      |
| `-TraceRoute`       | Perform traceroute                    |
| `-InformationLevel` | `Quiet` (returns false) or `Detailed` |

{% code overflow="wrap" %}
```powershell
# Ping
Test-NetConnection 192.168.1.1
Test-NetConnection google.com

# Port check (like telnet)
Test-NetConnection 192.168.1.1 -Port 445
Test-NetConnection 192.168.1.1 -Port 80 -InformationLevel Quiet   # Returns $true/$false

# Traceroute
Test-NetConnection google.com -TraceRoute

# Port scan (basic)
1..1024 | ForEach-Object { $result = Test-NetConnection -ComputerName 192.168.1.1 -Port $_ -WarningAction SilentlyContinue -InformationLevel Quiet; if ($result) { "Port $_ is open" } }
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1490).png" alt=""><figcaption></figcaption></figure>

### Get-NetTCPConnection

Shows active TCP connections. Equivalent to `netstat`.

{% code overflow="wrap" %}
```powershell
# All TCP connections
Get-NetTCPConnection

# Listening ports
Get-NetTCPConnection -State Listen | Select-Object LocalAddress, LocalPort, OwningProcess | Sort-Object LocalPort

# Established connections
Get-NetTCPConnection -State Established | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, OwningProcess

# With process names (the gold standard)
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State, @{N='Process';E={(Get-Process -Id $_.OwningProcess -ErrorAction SilentlyContinue).Name}} | Format-Table -AutoSize

# Specific remote port
Get-NetTCPConnection -RemotePort 443
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1491).png" alt=""><figcaption></figcaption></figure>

## Firewall&#x20;

{% code overflow="wrap" %}
```powershell
# List all firewall rules
Get-NetFirewallRule

# Enabled rules only
Get-NetFirewallRule -Enabled True | Select-Object Name, DisplayName, Direction, Action | Format-Table -AutoSize

# Firewall profiles
Get-NetFirewallProfile | Select-Object Name, Enabled

# Check if firewall is enabled
Get-NetFirewallProfile | Select-Object Name, Enabled, DefaultInboundAction, DefaultOutboundAction

# Create a new firewall rule (admin)
New-NetFirewallRule -DisplayName "Allow Port 4444" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 4444

# Remove a firewall rule
Remove-NetFirewallRule -DisplayName "Allow Port 4444"

# Disable firewall (admin — very noisy)
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1492).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1493).png" alt=""><figcaption></figcaption></figure>

## Registry

### Registry Provider Overview

PowerShell treats the registry as a file system using PSDrives.

| PSDrive | Registry Hive        | Description           |
| ------- | -------------------- | --------------------- |
| `HKLM:` | HKEY\_LOCAL\_MACHINE | Machine-wide settings |
| `HKCU:` | HKEY\_CURRENT\_USER  | Current user settings |

```powershell
# Navigate registry like filesystem
Set-Location HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion
Get-ChildItem                      # List subkeys
Get-ChildItem -Recurse             # Recursive listing

# Mount additional hives
New-PSDrive -Name HKU -PSProvider Registry -Root HKEY_USERS
New-PSDrive -Name HKCR -PSProvider Registry -Root HKEY_CLASSES_ROOT
```

<figure><img src="../../../.gitbook/assets/image (1494).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1495).png" alt=""><figcaption></figcaption></figure>

### Get-Item and Get-ItemProperty

{% code overflow="wrap" %}
```powershell
# Get a registry key (returns the key object)
Get-Item HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

# Get all values in a key
Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

# Get a specific value
Get-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run -Name "SecurityHealth"
(Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run).SecurityHealth
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1496).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1497).png" alt=""><figcaption></figcaption></figure>

### New-Item & Set-ItemProperty

{% code overflow="wrap" %}
```powershell
# Create a new registry key
New-Item -Path HKCU:\SOFTWARE\MyTestKey

# Create/set a registry value
Set-ItemProperty -Path HKCU:\SOFTWARE\MyTestKey -Name "TestValue" -Value "Hello"
New-ItemProperty -Path HKCU:\SOFTWARE\MyTestKey -Name "TestDWORD" -Value 1 -PropertyType DWORD
```
{% endcode %}

### Remove-Item & Remove-ItemProperty

```powershell
# Remove a registry value
Remove-ItemProperty -Path HKCU:\SOFTWARE\MyTestKey -Name "TestValue"

# Remove a registry key (and all subkeys)
Remove-Item -Path HKCU:\SOFTWARE\MyTestKey -Recurse
```

### Test-Path

```powershell
# Check if a registry key exists
Test-Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run    # $true
Test-Path HKLM:\SOFTWARE\NonExistent     
```
