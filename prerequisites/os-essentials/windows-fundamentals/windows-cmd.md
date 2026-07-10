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

