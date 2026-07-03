# 🐧 Linux

Linux is a **free and open-source operating system kernel** created by **Linus Torvalds** in 1991.

Linux powers a huge percentage of today's infrastructure, including:

* Servers
* Cloud platforms
* Embedded devices
* Android smartphones (Linux kernel)
* IoT devices
* Supercomputers
* Security distributions (Kali, Parrot)

## Introduction to Linux

### Linux Architecture&#x20;

The Linux operating system architecture defines how different components of the system interact with each other to manage hardware resources, run applications, and provide a stable and secure computing environment. Linux follows a layered architecture, where each layer has a specific role and responsibility.

<figure><img src="../../../.gitbook/assets/image (803).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Identify current architecture
arch
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (814).png" alt=""><figcaption></figcaption></figure>

#### Kernel&#x20;

The **kernel** is the core of Linux.

It manages communication between software and hardware.

Responsibilities include:

* Process scheduling
* Memory management
* File system management
* Device drivers
* Network stack
* Security mechanisms
* Hardware communication

Without the kernel, applications cannot access hardware directly.

### Shell&#x20;

The shell is a software interface to the kernel. It takes commands from the user and interprets them. The shell transmits these commands to the kernel, which then performs the requested operations.&#x20;

<table data-search="false"><thead><tr><th width="149.99993896484375">Shell</th><th width="562.7999877929688">Description</th></tr></thead><tbody><tr><td><strong><code>bash</code> (Bourne Again Shell)</strong></td><td>Default shell on most Linux distributions. Widely used for scripting, system administration, and penetration testing.</td></tr><tr><td><strong><code>sh</code> (Bourne Shell)</strong></td><td>The original Unix shell. Provides basic shell functionality and is commonly used for portable shell scripts.</td></tr><tr><td><strong><code>zsh</code> (Z Shell)</strong></td><td>An enhanced shell with features like auto-completion, syntax highlighting (via plugins), themes, and improved scripting. Popular among developers and pentesters.</td></tr><tr><td><strong><code>ksh</code> (Korn Shell)</strong></td><td>Combines features of the Bourne Shell and C Shell. Commonly used in enterprise Unix environments for scripting.</td></tr><tr><td><strong><code>csh</code> (C Shell)</strong></td><td>Shell with C-like syntax, command history, and aliases. Primarily used on BSD systems; less common today.</td></tr><tr><td><strong><code>tcsh</code></strong></td><td>Enhanced version of C Shell (<code>csh</code>) with command-line editing, programmable completion, and history improvements.</td></tr><tr><td><strong>BusyBox Shell (<code>ash</code>)</strong></td><td>Minimal shell included with BusyBox, commonly found in embedded Linux devices, routers, IoT devices, and recovery environments.</td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
# Identify valid login shells
cat /etc/shells
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (815).png" alt=""><figcaption></figcaption></figure>

### Linux Distribution&#x20;

A **Linux distribution (Distro)** is a complete operating system built around the Linux kernel.

<figure><img src="../../../.gitbook/assets/image (804).png" alt=""><figcaption></figcaption></figure>

### Components of Linux&#x20;

<table data-search="false"><thead><tr><th width="123.60003662109375">Component</th><th>Description</th></tr></thead><tbody><tr><td><strong>Bootloader</strong></td><td>A small program that executes immediately after the system powers on. It locates and loads the operating system kernel into memory. Most Linux distributions use <strong>GRUB (Grand Unified Bootloader)</strong> as the default bootloader.</td></tr><tr><td><strong>OS Kernel</strong></td><td>The core component of the operating system that manages hardware resources, memory, processes, device drivers, file systems, and networking. It acts as the bridge between hardware and user applications.</td></tr><tr><td><strong>Daemons</strong></td><td>Background processes that start during boot or on demand to provide essential system services such as networking, scheduling, logging, printing, SSH, and web services. Daemon process names typically end with the letter <strong><code>d</code></strong> (e.g., <code>sshd</code>, <code>httpd</code>, <code>systemd</code>).</td></tr><tr><td><strong>OS Shell</strong></td><td>A command-line interpreter that provides an interface between the user and the operating system. It accepts user commands, executes programs, and runs shell scripts. Common Linux shells include <strong>Bash</strong>, <strong>Zsh</strong>, <strong>Fish</strong>, <strong>Ksh</strong>, <strong>Tcsh/Csh</strong>, and <strong>Dash</strong>.</td></tr><tr><td><strong>Graphics Server</strong></td><td>A software layer responsible for displaying graphical applications and handling input from the keyboard and mouse. Traditionally Linux uses the <strong>X Window System (X11/X Server)</strong>, while many modern distributions are adopting <strong>Wayland</strong> as its successor.</td></tr><tr><td><strong>Window Manager / Desktop Environment</strong></td><td>Provides the graphical user interface (GUI), including windows, panels, menus, icons, and desktop applications. Popular desktop environments include <strong>GNOME</strong>, <strong>KDE Plasma</strong>, <strong>Xfce</strong>, <strong>MATE</strong>, <strong>Cinnamon</strong>, and <strong>LXQt</strong>.</td></tr><tr><td><strong>Utilities / Applications</strong></td><td>User-space programs that perform specific tasks, such as file management, text editing, networking, package management, system monitoring, and penetration testing. Examples include <code>ls</code>, <code>grep</code>, <code>vim</code>, <code>curl</code>, <code>nmap</code>, and <code>Burp Suite</code>.</td></tr></tbody></table>

### Linux File System Hierarchy&#x20;

<figure><img src="../../../.gitbook/assets/image (806).png" alt=""><figcaption></figcaption></figure>

<table data-search="false"><thead><tr><th width="110">Path</th><th>Description</th></tr></thead><tbody><tr><td><strong><code>/</code> (Root)</strong></td><td>The top-level directory of the Linux filesystem. Every file and directory originates from the root (<code>/</code>). All other filesystems are mounted under this directory during boot.</td></tr><tr><td><strong><code>/bin</code></strong></td><td>Contains essential user command binaries required for system operation, such as <code>ls</code>, <code>cp</code>, <code>mv</code>, <code>cat</code>, and <code>bash</code>.</td></tr><tr><td><strong><code>/boot</code></strong></td><td>Stores files required during the boot process, including the Linux kernel, initial RAM filesystem (<code>initramfs</code>), and bootloader files (e.g., GRUB).</td></tr><tr><td><strong><code>/dev</code></strong></td><td>Contains device files that represent hardware and virtual devices, such as disks (<code>/dev/sda</code>), terminals (<code>/dev/tty</code>), and special devices like <code>/dev/null</code> and <code>/dev/random</code>.</td></tr><tr><td><strong><code>/etc</code></strong></td><td>Holds system-wide configuration files for the operating system and installed applications. Important files include <code>/etc/passwd</code>, <code>/etc/shadow</code>, <code>/etc/hosts</code>, and <code>/etc/ssh/</code>.</td></tr><tr><td><strong><code>/home</code></strong></td><td>Contains personal home directories for regular users, where documents, downloads, SSH keys, and user-specific configuration files are stored.</td></tr><tr><td><strong><code>/lib</code></strong></td><td>Stores essential shared libraries and kernel modules required by system binaries and during the boot process. On 64-bit systems, libraries may also be found in <code>/lib64</code>.</td></tr><tr><td><strong><code>/media</code></strong></td><td>Default mount point for removable storage devices such as USB flash drives, CDs, DVDs, and external hard disks.</td></tr><tr><td><strong><code>/mnt</code></strong></td><td>Used as a temporary mount point for manually mounting filesystems, network shares, or disk images.</td></tr><tr><td><strong><code>/opt</code></strong></td><td>Contains optional or third-party software packages that are not part of the default operating system installation.</td></tr><tr><td><strong><code>/root</code></strong></td><td>The home directory of the <strong>root</strong> (superuser) account. This is different from the root directory (<code>/</code>).</td></tr><tr><td><strong><code>/sbin</code></strong></td><td>Contains essential system administration binaries used for tasks such as system initialization, networking, and filesystem management (e.g., <code>fsck</code>, <code>shutdown</code>, <code>iptables</code>).</td></tr><tr><td><strong><code>/tmp</code></strong></td><td>Stores temporary files created by the operating system and applications. This directory is world-writable and is typically cleared during system reboot.</td></tr><tr><td><strong><code>/usr</code></strong></td><td>Contains the majority of user-space applications, libraries, documentation, and shared resources. Common subdirectories include <code>/usr/bin</code>, <code>/usr/lib</code>, and <code>/usr/share</code>.</td></tr><tr><td><strong><code>/var</code></strong></td><td>Stores variable data that changes during normal system operation, including log files (<code>/var/log</code>), web server content (<code>/var/www</code>), mail spools, caches, databases, and cron jobs.</td></tr></tbody></table>

## Getting Started

### Bash (Bourne Again Shell)

**Bash (Bourne Again Shell)** is the default shell on most Linux distributions.

#### Check current shell and bash version&#x20;

{% code overflow="wrap" %}
```bash
echo $SHELL
bash --version
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (807).png" alt=""><figcaption></figcaption></figure>

### Bash Configuration Files&#x20;

A **Bash configuration file** is a text file that contains commands, variables, aliases, and settings that Bash automatically executes when it starts.

Instead of typing the same commands every time you open a terminal, you place them in a configuration file. Bash reads the file during startup and applies those settings automatically.

<figure><img src="../../../.gitbook/assets/image (816).png" alt=""><figcaption></figcaption></figure>

#### \~/.bashrc

* **User-specific** Bash configuration file.
* Stored in the user's home directory.
* Applied **only to the current user**.
* Used to configure aliases, environment variables, prompt (`PS1`), functions, etc.

#### /etc/bash.bashrc

* **System-wide** Bash configuration file.
* Applied **to all users** on the system.
* Usually maintained by the system administrator.
* Used for global aliases, prompts, and environment settings.

<details>

<summary><strong>Modifying Bash Configuration File</strong></summary>

## What Can You Do With `~/.bashrc`?

Almost anything related to your shell environment.

***

### 1. Create Aliases

Instead of typing long commands repeatedly:

```bash
alias ll='ls -lah'
alias update='sudo apt update && sudo apt upgrade'
alias ports='ss -tulnp'
```

Now simply type:

```bash
ll
```

instead of

```bash
ls -lah
```

<figure><img src="../../../.gitbook/assets/image (817).png" alt=""><figcaption></figcaption></figure>

***

### 2. Set Environment Variables

Example:

```bash
export USERNAME="abhishek"
```

Now every terminal automatically has these variables available.

<figure><img src="../../../.gitbook/assets/image (818).png" alt=""><figcaption></figcaption></figure>

***

### 3. Modify the PATH Variable

Suppose you install custom tools in:

```
~/tools
```

Instead of typing

```bash
~/tools/script.sh
```

every time, add:

```bash
export PATH=$PATH:$HOME/tools
```

<figure><img src="../../../.gitbook/assets/image (820).png" alt=""><figcaption></figcaption></figure>

***

### 4. Define Shell Functions

Functions behave like your own custom commands.

Example:

```bash
userinfo() {
    echo "===== User Information ====="
    echo "Username : $USER"
    echo "Hostname : $(hostname)"
    echo "Home Dir : $HOME"
    echo "Current Dir : $PWD"
    echo "Shell : $SHELL"
    echo "Date : $(date)"
}
```

<figure><img src="../../../.gitbook/assets/image (821).png" alt=""><figcaption></figcaption></figure>

***

### 5. Automatically Run Commands

Every new terminal can execute commands automatically.

Example:

{% code overflow="wrap" %}
```bash
echo "Welcome back!"
echo "I can add here a custom executable location that can execute on starting the shell"
whoami
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (822).png" alt=""><figcaption></figcaption></figure>

***

### 6. Source Other Scripts

You can execute another file whenever Bash starts.

Example:

```bash
source ~/.aliases
```

Now all aliases stored in `.aliases` become available.

## Apply Changes Without Restarting

Normally, changes take effect only in new Bash sessions.

To apply them immediately:

```bash
source ~/.bashrc
```

or

```bash
. ~/.bashrc
```

***

Since `.bashrc` is just a shell startup script executed as the user, it can be customized extensively. Understanding it helps you both personalize your own Linux environment and better analyze user environments during security assessments.

</details>

### Getting Help&#x20;

#### Using \`--help\`&#x20;

{% code overflow="wrap" %}
```bash
<command> --help
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (808).png" alt=""><figcaption></figcaption></figure>

#### Using man&#x20;

{% code overflow="wrap" %}
```bash
man <command> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (809).png" alt=""><figcaption></figcaption></figure>

#### Using ExplainShell&#x20;

{% embed url="https://www.explainshell.com" %}

<figure><img src="../../../.gitbook/assets/image (810).png" alt=""><figcaption></figcaption></figure>

#### Using \`whatis\`

{% code overflow="wrap" %}
```
whatis <tool>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (813).png" alt=""><figcaption></figcaption></figure>

### Basic Navigation&#x20;

<table data-search="false"><thead><tr><th width="255.60003662109375">Command</th><th width="296.4000244140625">Description</th></tr></thead><tbody><tr><td><code>pwd</code></td><td>Print current working directory</td></tr><tr><td><code>ls</code></td><td>List files and directories</td></tr><tr><td><code>ls -l</code></td><td>Long listing format</td></tr><tr><td><code>ls -la</code></td><td>Show hidden files</td></tr><tr><td><code>cd directory</code></td><td>Change directory</td></tr><tr><td><code>cd ..</code></td><td>Move one directory up</td></tr><tr><td><code>cd ~</code></td><td>Go to home directory</td></tr><tr><td><code>cd -</code></td><td>Return to previous directory</td></tr><tr><td><code>tree</code></td><td>Display directory tree</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (811).png" alt=""><figcaption></figcaption></figure>

### Viewing Files&#x20;

<table data-search="false"><thead><tr><th width="210.79998779296875">Command</th><th width="495.59991455078125">Description</th></tr></thead><tbody><tr><td><code>cat</code></td><td>Display entire file</td></tr><tr><td><code>less</code></td><td><strong><code>less</code></strong> lets you scroll both up and down.</td></tr><tr><td><code>more</code></td><td><strong><code>more</code></strong> only lets you scroll down.</td></tr><tr><td><code>head</code></td><td>Show first 10 lines by default</td></tr><tr><td><code>tail</code></td><td>Show last 10 lines by default</td></tr><tr><td><code>nl</code></td><td>Show line numbers</td></tr><tr><td><code>file</code></td><td>Identify file type</td></tr><tr><td><code>stat</code></td><td>Show file metadata</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (812).png" alt=""><figcaption></figcaption></figure>

## Working with Files and Directories&#x20;

### Devices and Partitions

In Linux, storage devices such as hard disks, SSDs, USB drives, and optical media are represented as **device files** inside the `/dev` directory.

#### Types of Device Files&#x20;

1. **Character devices:** Character devices transfer data **one byte at a time**. Data arrives continuously and sequentially (keyboard, mouse, terminal, serial ports). These are **character devices**.

<figure><img src="../../../.gitbook/assets/image (823).png" alt=""><figcaption></figcaption></figure>

2. **Block devices:** Block devices transfer data in **fixed-size blocks** and are primarily used for storage. Data is stored in addressable locations and benefits from random access and caching (HDDs, SSDs, USB drives). These are **block devices**.

<figure><img src="../../../.gitbook/assets/image (824).png" alt=""><figcaption></figcaption></figure>

#### Disk Naming Conventions

Linux assigns names to storage devices based on their type.

* **sda** → Physical disk
* **sda1** → First partition
* **sda2** → Second partition

If another hard disk were attached, Linux would create:

```
/dev/sdb
```

<figure><img src="../../../.gitbook/assets/image (825).png" alt=""><figcaption></figcaption></figure>

#### Disk Usage&#x20;

{% code overflow="wrap" %}
```
df -Th
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (826).png" alt=""><figcaption></figcaption></figure>

### Absolute and relative path&#x20;

A **path** specifies the location of a file or directory within the Linux filesystem. It tells the operating system where a resource is located.

<figure><img src="../../../.gitbook/assets/image (827).png" alt=""><figcaption></figcaption></figure>

#### Absolute File

An **absolute path** specifies the complete location of a file or directory starting from the **root directory (`/`)**.

{% hint style="info" %}
_Since it always begins with `/`, an absolute path refers to the same location regardless of the current working directory._
{% endhint %}

E.g.,&#x20;

{% code overflow="wrap" %}
```
cat /etc/passwd

cat /home/r0b/tools/myscript.sh
```
{% endcode %}

#### Relative File

A **relative path** specifies the location of a file or directory relative to the **current working directory (PWD)**.

{% hint style="info" %}
_The same relative path may refer to different locations depending on where the user is currently located._
{% endhint %}

E.g.,&#x20;

{% code overflow="wrap" %}
```
cat ./myscript.sh
```
{% endcode %}

### Managing Files and Directories&#x20;

<table data-search="false"><thead><tr><th width="255.60003662109375">Command</th><th width="432.4000244140625">Description</th></tr></thead><tbody><tr><td><code>touch file</code></td><td>Create an empty file / update timestamp if it exists</td></tr><tr><td><code>mkdir dir</code></td><td>Create a directory</td></tr><tr><td><code>mkdir -p a/b/c</code></td><td>Create nested directories in one go</td></tr><tr><td><code>cp src dest</code></td><td>Copy a file</td></tr><tr><td><code>cp -r src dest</code></td><td>Copy a directory recursively</td></tr><tr><td><code>mv src dest</code></td><td>Move or rename a file/directory</td></tr><tr><td><code>rm file</code></td><td>Remove a file</td></tr><tr><td><code>rm -r dir</code></td><td>Remove a directory recursively</td></tr><tr><td><code>rm -rf dir</code></td><td>Force remove, no confirmation prompts</td></tr><tr><td><code>rmdir dir</code></td><td>Remove an empty directory only</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (828).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (829).png" alt=""><figcaption></figcaption></figure>

### Working with File Paths (Spaces and Expansion)

The shell splits a command line into arguments using **whitespace**. A file or directory name containing a space will be treated as two separate arguments unless it's quoted or escaped.

#### Handling Spaces&#x20;

{% code overflow="wrap" %}
```bash
touch "my file.txt"      # Double quotes
touch 'my file.txt'      # Single quotes
touch my\ file.txt       # Escape the space
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (830).png" alt=""><figcaption></figcaption></figure>

#### Brace Expansions&#x20;

{% code overflow="wrap" %}
```bash
touch file{1,2,3}.txt
touch file{4..8}.txt
touch file{1..2}{b..d}.txt
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (831).png" alt=""><figcaption></figcaption></figure>

### Wildcards&#x20;

**Wildcards** are special characters used by the shell to match one or more filenames or directory names based on a pattern. Before executing a command, the shell expands the wildcard pattern into all matching files and directories. This process is known as **pathname expansion (globbing)**.

<table><thead><tr><th width="115.39996337890625">Wildcard</th><th width="413.5999755859375">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>*</code></td><td>Matches zero or more characters</td><td><code>ls *.txt</code></td></tr><tr><td><code>?</code></td><td>Matches exactly one character</td><td><code>ls file?.txt</code></td></tr><tr><td><code>[abc]</code></td><td>Matches any one character inside the brackets</td><td><code>ls file[123].txt</code></td></tr><tr><td><code>[a-z]</code></td><td>Matches any one character within a range</td><td><code>ls file[a-z].txt</code></td></tr><tr><td><code>[!abc]</code></td><td>Matches any one character except those inside the brackets</td><td><code>ls file[!0-9].txt</code></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (832).png" alt=""><figcaption></figcaption></figure>

### Hard and Soft Links

{% hint style="info" %}
_An inode (index node) is a foundational data structure in Linux filesystems that stores metadata about a file or directory (such as permissions, size, owner, and disk location) but excludes its actual data and filename._
{% endhint %}

A **link** is a reference to a file. Linux supports two kinds&#x20;

#### Hard Link

* A **hard link** is another directory entry that points directly to the **same inode** as the original file.
* Both filenames refer to the same physical data on disk.
* Modifying either file changes the same data.&#x20;

{% hint style="danger" %}
- _Deleting one filename doesn't delete the actual file until all hard links are removed._&#x20;
- _Hard link cannot link directories._&#x20;
- _Hard link cannot cross file systems (another device/drive)_
{% endhint %}

{% code overflow="wrap" %}
```
ln original.txt hardlink.txt
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (833).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (834).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (837).png" alt=""><figcaption></figcaption></figure>

#### Soft Link

* A **symbolic link** is a special file that stores the **path** to another file or directory.
* Unlike a hard link, it has its own inode and simply redirects to the target whenever it is accessed.
* Becomes a **broken (dangling) link** if the target is deleted or moved.

<figure><img src="../../../.gitbook/assets/image (836).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
_Notice the `../` because from inside `linked`, you must go **one directory up** to reach `original.txt`._

_**Always provide absolute path for correct symlinks for better results.**_&#x20;
{% endhint %}

### Compression, Decompression and Archiving

* **Archiving** bundles multiple files into a single file without reducing size (`tar`).&#x20;
* **Compression** reduces the size of that archive using an algorithm (`gzip`, `bzip2`, `xz`).&#x20;

<table data-search="false"><thead><tr><th width="156.39996337890625">Option</th><th width="413.20001220703125">Description</th></tr></thead><tbody><tr><td><code>-c</code></td><td>Create a new archive</td></tr><tr><td><code>-x</code></td><td>Extract an archive</td></tr><tr><td><code>-t</code></td><td>List archive contents</td></tr><tr><td><code>-f</code></td><td>Specify archive filename</td></tr><tr><td><code>-v</code></td><td>Verbose output (show processed files)</td></tr><tr><td><code>-z</code></td><td>Compress/Decompress using <strong>gzip</strong> (<code>.tar.gz</code>)</td></tr><tr><td><code>-j</code></td><td>Compress/Decompress using <strong>bzip2</strong> (<code>.tar.bz2</code>)</td></tr><tr><td><code>-J</code></td><td>Compress/Decompress using <strong>xz</strong> (<code>.tar.xz</code>)</td></tr><tr><td><code>-C</code></td><td>Extract or archive from a specific directory</td></tr></tbody></table>

#### Creating an archive

{% code overflow="wrap" %}
```bash
tar -cvf archive.tar testdir/
# -c => Create a new archive
# -v => Verbose output
# -f => archive filename
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (838).png" alt=""><figcaption></figcaption></figure>

#### Extracting an archive&#x20;

{% code overflow="wrap" %}
```bash
tar -xvf archive.tar 
# -x => Extract an archive
# -v => verbose output
# -f => archive filename
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (839).png" alt=""><figcaption></figcaption></figure>

#### Creating Compressed archive&#x20;

{% code overflow="wrap" %}
```bash
tar -czvf temp_compr_archive.gz ./temp/
# -z => Compress with gzip
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (840).png" alt=""><figcaption></figcaption></figure>

#### Extracting Compressed archive

{% code overflow="wrap" %}
```bash
tar -xzvf temp_compr_archive.gz
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (842).png" alt=""><figcaption></figcaption></figure>

#### Compressing Files&#x20;

<table><thead><tr><th width="190.00006103515625">Option</th><th width="310.79998779296875">Description</th></tr></thead><tbody><tr><td><code>-r</code></td><td>Compress directories recursively</td></tr><tr><td><code>-e</code></td><td>Encrypt archive with a password</td></tr><tr><td><code>-9</code></td><td>Maximum compression</td></tr><tr><td><code>-0</code></td><td>No compression (archive only)</td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
zip backup.zip <files> 
zip backup.zip -r <folder> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (843).png" alt=""><figcaption></figcaption></figure>

#### Extracting Compressed files&#x20;

<table><thead><tr><th width="190.00006103515625">Option</th><th width="310.79998779296875">Description</th></tr></thead><tbody><tr><td><code>-d</code></td><td>Extracting files in specific directory</td></tr><tr><td><code>-l</code></td><td>Listing archive contents</td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
unzip <zip file> -d <folder> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (844).png" alt=""><figcaption></figcaption></figure>

#### GZIP&#x20;

{% hint style="danger" %}
_It compresses a single file only with `.gz` extension._&#x20;
{% endhint %}

The **`gzip`** command compresses a **single file** using the GNU Zip compression algorithm. It does **not** create an archive or combine multiple files. After compression, the original file is replaced with a **`.gz`** file.

| Command               | Description                         |
| --------------------- | ----------------------------------- |
| `gzip file.txt`       | Compress a file                     |
| `gzip -k file.txt`    | Compress and keep the original file |
| `gzip -d file.txt.gz` | Decompress a `.gz` file             |

#### Gunzip&#x20;

The **`gunzip`** command decompresses files compressed with **`gzip`**. It is equivalent to using `gzip -d`.

<figure><img src="../../../.gitbook/assets/image (845).png" alt=""><figcaption></figcaption></figure>

### Searching the Filesystem&#x20;

Linux offers several tools to locate files and directories — by name, type, size, modification time, or a pre-built index.

Tools available:&#x20;

* find
* locate

For builtin tools

* which
* whereis

#### Find

<table data-search="false"><thead><tr><th width="270">Command</th><th>Description</th></tr></thead><tbody><tr><td><code>find / -name "file.txt"</code></td><td>Search by exact filename in root directory. </td></tr><tr><td><code>find / -iname "file.txt"</code></td><td>Case-insensitive filename search</td></tr><tr><td><code>find . -name "*.txt"</code></td><td>Search using wildcards</td></tr><tr><td><code>find . -type f</code></td><td>Search for regular files</td></tr><tr><td><code>find . -type d</code></td><td>Search for directories</td></tr><tr><td><code>find . -size +10M</code></td><td>Search for files larger than 10 MB</td></tr><tr><td><code>find . -size -1M</code></td><td>Search for files smaller than 1 MB</td></tr><tr><td><code>find . -perm 644</code></td><td>Search by file permissions</td></tr><tr><td><code>find . -user r0b</code></td><td>Search by file owner</td></tr><tr><td><code>find . -empty</code></td><td>Search for empty files and directories</td></tr><tr><td><code>find . -name "*.log" -delete</code></td><td>Delete matching files</td></tr><tr><td><code>find . -name "*.log" -exec rm {} \;</code></td><td>Execute a command on matching files</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (846).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (847).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (848).png" alt=""><figcaption></figcaption></figure>

#### Locate

The **`locate`** command searches for files using a **pre-built database** instead of scanning the filesystem. As a result, it is much faster than `find`.

* update the database: `sudo updatedb`
* search for files: `locate <filename>`

<figure><img src="../../../.gitbook/assets/image (849).png" alt=""><figcaption></figcaption></figure>

### File and Directory Security&#x20;

Every file and directory on Linux carries a permission set that governs who can read, write, or execute it. This set is not fixed — it's **assigned by default**, can be **inherited**, **changed** deliberately, or **bypassed** under specific conditions.

#### Security values

<table data-search="false"><thead><tr><th width="123.60003662109375">Value</th><th width="165.9998779296875">Permission</th></tr></thead><tbody><tr><td><strong>4</strong></td><td>Read (r)</td></tr><tr><td><strong>2</strong></td><td>Write (w)</td></tr><tr><td><strong>1</strong></td><td>Execute (x)</td></tr></tbody></table>

#### Default Permissions - \`umask\`

{% hint style="danger" %}
_Linux files do not inherit permissions from their parent directory._&#x20;
{% endhint %}

<table><thead><tr><th width="148.39996337890625">Object</th><th width="198.79998779296875">Default Permission</th></tr></thead><tbody><tr><td>File</td><td><code>666</code> (<code>rw-rw-rw-</code>)</td></tr><tr><td>Directory</td><td><code>777</code> (<code>rwxrwxrwx</code>)</td></tr></tbody></table>

When a file or directory is created, Linux doesn't ask what permissions to give it — it starts from a maximum (666 for files, 777 for directories) and subtracts the **umask** value of the shell that created it.

<figure><img src="../../../.gitbook/assets/image (850).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
002 means 
Owner : 0
Group : 0
Others: 2
A value of 2 means remove the write (w) permission.
```
{% endcode %}

{% code overflow="wrap" %}
```
Default File Permission

666
-002
----
664 (-rw-rw-r--)
```
{% endcode %}

#### Changing Ownership&#x20;

{% hint style="warning" %}
_Only the root user can change the ownership of a file._&#x20;
{% endhint %}

Every file and directory in Linux has an **owner (user)** and an associated **group**. Linux provides commands to change the ownership of files and directories.

<figure><img src="../../../.gitbook/assets/image (851).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Change owner and group of file
chown <owner>:<group> <file>
# Change owner of the file
chown <owner> <file> 
# Change owner and group of files within a directory recursively
chown -R <owner>:<group> <file>
# Change group of the file 
chgrp <group> <file> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (853).png" alt=""><figcaption></figcaption></figure>

#### Changing Permissions&#x20;

{% code overflow="wrap" %}
```
-rwsr-xr-x 1 root root 93640 Feb  3 02:15 /usr/bin/passwd
 │││ │││ │││
 │││ │││ │└┴── Others (r-x)
 │││ │└┴────── Group (r-x)
 │└┴────────── Owner (rws)
 └──────────── File Type (- = Regular File, d for directory)
```
{% endcode %}

Linux controls access to files and directories using **permission bits** for the **Owner**, **Group**, and **Others**. The **`chmod` (Change Mode)** command is used to modify these permissions.

Permissions are modified using symbolic operators.

{% code overflow="wrap" %}
```bash
sudo chmod <class><symbol><permission> <file>
sudo chmod <numeric value> <file>
```
{% endcode %}

| Symbol | Description          |
| ------ | -------------------- |
| `+`    | Add permission       |
| `-`    | Remove permission    |
| `=`    | Set exact permission |

| Class          | Description  |
| -------------- | ------------ |
| `u`            | User (Owner) |
| `g`            | Group        |
| `o`            | Others       |
| `a` / Nothing  | All users    |

<table data-search="false"><thead><tr><th width="143.60003662109375" align="right">Value</th><th width="141.19989013671875">Permission</th></tr></thead><tbody><tr><td align="right"><code>0</code></td><td><code>---</code></td></tr><tr><td align="right"><code>1</code></td><td><code>--x</code></td></tr><tr><td align="right"><code>2</code></td><td><code>-w-</code></td></tr><tr><td align="right"><code>3</code></td><td><code>-wx</code></td></tr><tr><td align="right"><code>4</code></td><td><code>r--</code></td></tr><tr><td align="right"><code>5</code></td><td><code>r-x</code></td></tr><tr><td align="right"><code>6</code></td><td><code>rw-</code></td></tr><tr><td align="right"><code>7</code></td><td><code>rwx</code></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (854).png" alt=""><figcaption></figcaption></figure>

### Special Permission bits (SGID and SUID bit)

The three special permissions are:

* **SUID (Set User ID)**
* **SGID (Set Group ID)**
* **Sticky Bit**

#### SUID (Set User ID)

The **SUID** permission allows a program to execute with the **permissions of the file owner**, rather than the permissions of the user running it.

This is commonly used by system programs that require elevated privileges to perform specific tasks.

{% code overflow="wrap" %}
```bash
sudo chmod u+s file
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (855).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_The `/usr/bin/passwd` program runs with the effective UID of `root` (due to the SUID bit), allowing it to modify `/etc/shadow`. However, the `passwd` program itself enforces security checks, permitting regular users to change only their own password, while only the `root` user can change the passwords of other users._
{% endhint %}

{% hint style="danger" %}
_If you have execute permission and the SUID bit is set, you can execute the program with the **effective UID of the file owner**. We should have at least `x` permission to run the program with the file owner's privileges._&#x20;
{% endhint %}

{% hint style="info" %}
_**Linux ignores the SUID bit on interpreted scripts** (e.g., **Shell**, **Python**, and **Perl**) for security reasons. Therefore, executing a SUID script **does not** grant the effective UID of the file owner. SUID is honored only for **compiled binaries (ELF executables)**._
{% endhint %}

#### SGID (Set Group ID)

The **SGID** permission has different behavior for **files** and **directories**.

* Files
  * When SGID is applied to an executable file, the program executes with the **permissions of the file's group** instead of the user's primary group.
* Directories
  * New files inherit the **directory's group** instead of the creator's primary group.
  * New subdirectories also inherit the SGID bit.

{% code overflow="wrap" %}
```bash
sudo chmod g+s project
```
{% endcode %}

<table data-search="false"><thead><tr><th>Feature</th><th>SUID (Set User ID)</th><th>SGID (Set Group ID)</th></tr></thead><tbody><tr><td>Permission Position</td><td>Owner execute field (<code>rws</code>)</td><td>Group execute field (<code>r-s</code>)</td></tr><tr><td>Affects</td><td><strong>Effective User ID (EUID)</strong></td><td><strong>Effective Group ID (EGID)</strong></td></tr><tr><td>Executes As</td><td>File owner</td><td>File's group</td></tr><tr><td>Requires</td><td>Execute (<code>x</code>) permission</td><td>Execute (<code>x</code>) permission</td></tr><tr><td>Need to belong to file's group?</td><td><strong>No</strong></td><td><strong>No</strong></td></tr><tr><td>Typical Use</td><td><code>/usr/bin/passwd</code> runs as <code>root</code></td><td>Programs that need temporary group privileges</td></tr><tr><td>Gives root shell?</td><td><strong>Yes</strong>, if owner is <code>root</code> and the program is exploitable (e.g., <code>bash -p</code>)</td><td><strong>No</strong>, it only changes the effective group</td></tr></tbody></table>
