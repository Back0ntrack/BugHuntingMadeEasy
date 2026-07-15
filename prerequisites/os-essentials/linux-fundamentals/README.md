# 🐧 Linux Fundamentals

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

<figure><img src="../../../.gitbook/assets/image (1283).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Identify current architecture
arch
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1294).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1295).png" alt=""><figcaption></figcaption></figure>

### Linux Distribution&#x20;

A **Linux distribution (Distro)** is a complete operating system built around the Linux kernel.

<figure><img src="../../../.gitbook/assets/image (1284).png" alt=""><figcaption></figcaption></figure>

### Components of Linux&#x20;

<table data-search="false"><thead><tr><th width="123.60003662109375">Component</th><th>Description</th></tr></thead><tbody><tr><td><strong>Bootloader</strong></td><td>A small program that executes immediately after the system powers on. It locates and loads the operating system kernel into memory. Most Linux distributions use <strong>GRUB (Grand Unified Bootloader)</strong> as the default bootloader.</td></tr><tr><td><strong>OS Kernel</strong></td><td>The core component of the operating system that manages hardware resources, memory, processes, device drivers, file systems, and networking. It acts as the bridge between hardware and user applications.</td></tr><tr><td><strong>Daemons</strong></td><td>Background processes that start during boot or on demand to provide essential system services such as networking, scheduling, logging, printing, SSH, and web services. Daemon process names typically end with the letter <strong><code>d</code></strong> (e.g., <code>sshd</code>, <code>httpd</code>, <code>systemd</code>).</td></tr><tr><td><strong>OS Shell</strong></td><td>A command-line interpreter that provides an interface between the user and the operating system. It accepts user commands, executes programs, and runs shell scripts. Common Linux shells include <strong>Bash</strong>, <strong>Zsh</strong>, <strong>Fish</strong>, <strong>Ksh</strong>, <strong>Tcsh/Csh</strong>, and <strong>Dash</strong>.</td></tr><tr><td><strong>Graphics Server</strong></td><td>A software layer responsible for displaying graphical applications and handling input from the keyboard and mouse. Traditionally Linux uses the <strong>X Window System (X11/X Server)</strong>, while many modern distributions are adopting <strong>Wayland</strong> as its successor.</td></tr><tr><td><strong>Window Manager / Desktop Environment</strong></td><td>Provides the graphical user interface (GUI), including windows, panels, menus, icons, and desktop applications. Popular desktop environments include <strong>GNOME</strong>, <strong>KDE Plasma</strong>, <strong>Xfce</strong>, <strong>MATE</strong>, <strong>Cinnamon</strong>, and <strong>LXQt</strong>.</td></tr><tr><td><strong>Utilities / Applications</strong></td><td>User-space programs that perform specific tasks, such as file management, text editing, networking, package management, system monitoring, and penetration testing. Examples include <code>ls</code>, <code>grep</code>, <code>vim</code>, <code>curl</code>, <code>nmap</code>, and <code>Burp Suite</code>.</td></tr></tbody></table>

### Linux File System Hierarchy&#x20;

<figure><img src="../../../.gitbook/assets/image (1286).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1287).png" alt=""><figcaption></figcaption></figure>

### Bash Configuration Files&#x20;

A **Bash configuration file** is a text file that contains commands, variables, aliases, and settings that Bash automatically executes when it starts.

Instead of typing the same commands every time you open a terminal, you place them in a configuration file. Bash reads the file during startup and applies those settings automatically.

<figure><img src="../../../.gitbook/assets/image (1296).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1297).png" alt=""><figcaption></figcaption></figure>

***

### 2. Set Environment Variables

Example:

```bash
export USERNAME="abhishek"
```

Now every terminal automatically has these variables available.

<figure><img src="../../../.gitbook/assets/image (1298).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1300).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1301).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1302).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1288).png" alt=""><figcaption></figcaption></figure>

#### Using man&#x20;

{% code overflow="wrap" %}
```bash
man <command> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1289).png" alt=""><figcaption></figcaption></figure>

#### Using ExplainShell&#x20;

{% embed url="https://www.explainshell.com" %}

<figure><img src="../../../.gitbook/assets/image (1290).png" alt=""><figcaption></figcaption></figure>

#### Using \`whatis\`

{% code overflow="wrap" %}
```
whatis <tool>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1293).png" alt=""><figcaption></figcaption></figure>

### Basic Navigation&#x20;

<table data-search="false"><thead><tr><th width="255.60003662109375">Command</th><th width="296.4000244140625">Description</th></tr></thead><tbody><tr><td><code>pwd</code></td><td>Print current working directory</td></tr><tr><td><code>ls</code></td><td>List files and directories</td></tr><tr><td><code>ls -l</code></td><td>Long listing format</td></tr><tr><td><code>ls -la</code></td><td>Show hidden files</td></tr><tr><td><code>cd directory</code></td><td>Change directory</td></tr><tr><td><code>cd ..</code></td><td>Move one directory up</td></tr><tr><td><code>cd ~</code></td><td>Go to home directory</td></tr><tr><td><code>cd -</code></td><td>Return to previous directory</td></tr><tr><td><code>tree</code></td><td>Display directory tree</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1291).png" alt=""><figcaption></figcaption></figure>

### Viewing Files&#x20;

<table data-search="false"><thead><tr><th width="210.79998779296875">Command</th><th width="495.59991455078125">Description</th></tr></thead><tbody><tr><td><code>cat</code></td><td>Display entire file</td></tr><tr><td><code>less</code></td><td><strong><code>less</code></strong> lets you scroll both up and down.</td></tr><tr><td><code>more</code></td><td><strong><code>more</code></strong> only lets you scroll down.</td></tr><tr><td><code>head</code></td><td>Show first 10 lines by default</td></tr><tr><td><code>tail</code></td><td>Show last 10 lines by default</td></tr><tr><td><code>nl</code></td><td>Show line numbers</td></tr><tr><td><code>file</code></td><td>Identify file type</td></tr><tr><td><code>stat</code></td><td>Show file metadata</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1292).png" alt=""><figcaption></figcaption></figure>

## Working with Files and Directories&#x20;

### Devices and Partitions

In Linux, storage devices such as hard disks, SSDs, USB drives, and optical media are represented as **device files** inside the `/dev` directory.

#### Types of Device Files&#x20;

1. **Character devices:** Character devices transfer data **one byte at a time**. Data arrives continuously and sequentially (keyboard, mouse, terminal, serial ports). These are **character devices**.

<figure><img src="../../../.gitbook/assets/image (1303).png" alt=""><figcaption></figcaption></figure>

2. **Block devices:** Block devices transfer data in **fixed-size blocks** and are primarily used for storage. Data is stored in addressable locations and benefits from random access and caching (HDDs, SSDs, USB drives). These are **block devices**.

<figure><img src="../../../.gitbook/assets/image (1304).png" alt=""><figcaption></figcaption></figure>

#### Disk Naming Conventions

Linux assigns names to storage devices based on their type.

* **sda** → Physical disk
* **sda1** → First partition
* **sda2** → Second partition

If another hard disk were attached, Linux would create:

```
/dev/sdb
```

<figure><img src="../../../.gitbook/assets/image (1305).png" alt=""><figcaption></figcaption></figure>

#### Disk Usage&#x20;

{% code overflow="wrap" %}
```
df -Th
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1306).png" alt=""><figcaption></figcaption></figure>

### Absolute and relative path&#x20;

A **path** specifies the location of a file or directory within the Linux filesystem. It tells the operating system where a resource is located.

<figure><img src="../../../.gitbook/assets/image (1307).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1308).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1309).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1310).png" alt=""><figcaption></figcaption></figure>

#### Brace Expansions&#x20;

{% code overflow="wrap" %}
```bash
touch file{1,2,3}.txt
touch file{4..8}.txt
touch file{1..2}{b..d}.txt
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1311).png" alt=""><figcaption></figcaption></figure>

### Wildcards&#x20;

**Wildcards** are special characters used by the shell to match one or more filenames or directory names based on a pattern. Before executing a command, the shell expands the wildcard pattern into all matching files and directories. This process is known as **pathname expansion (globbing)**.

<table><thead><tr><th width="115.39996337890625">Wildcard</th><th width="413.5999755859375">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>*</code></td><td>Matches zero or more characters</td><td><code>ls *.txt</code></td></tr><tr><td><code>?</code></td><td>Matches exactly one character</td><td><code>ls file?.txt</code></td></tr><tr><td><code>[abc]</code></td><td>Matches any one character inside the brackets</td><td><code>ls file[123].txt</code></td></tr><tr><td><code>[a-z]</code></td><td>Matches any one character within a range</td><td><code>ls file[a-z].txt</code></td></tr><tr><td><code>[!abc]</code></td><td>Matches any one character except those inside the brackets</td><td><code>ls file[!0-9].txt</code></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1312).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1313).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1314).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1317).png" alt=""><figcaption></figcaption></figure>

#### Soft Link

* A **symbolic link** is a special file that stores the **path** to another file or directory.
* Unlike a hard link, it has its own inode and simply redirects to the target whenever it is accessed.
* Becomes a **broken (dangling) link** if the target is deleted or moved.

<figure><img src="../../../.gitbook/assets/image (1316).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1318).png" alt=""><figcaption></figcaption></figure>

#### Extracting an archive&#x20;

{% code overflow="wrap" %}
```bash
tar -xvf archive.tar 
# -x => Extract an archive
# -v => verbose output
# -f => archive filename
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1319).png" alt=""><figcaption></figcaption></figure>

#### Creating Compressed archive&#x20;

{% code overflow="wrap" %}
```bash
tar -czvf temp_compr_archive.gz ./temp/
# -z => Compress with gzip
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1320).png" alt=""><figcaption></figcaption></figure>

#### Extracting Compressed archive

{% code overflow="wrap" %}
```bash
tar -xzvf temp_compr_archive.gz
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1322).png" alt=""><figcaption></figcaption></figure>

#### Compressing Files&#x20;

<table><thead><tr><th width="190.00006103515625">Option</th><th width="310.79998779296875">Description</th></tr></thead><tbody><tr><td><code>-r</code></td><td>Compress directories recursively</td></tr><tr><td><code>-e</code></td><td>Encrypt archive with a password</td></tr><tr><td><code>-9</code></td><td>Maximum compression</td></tr><tr><td><code>-0</code></td><td>No compression (archive only)</td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
zip backup.zip <files> 
zip backup.zip -r <folder> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1323).png" alt=""><figcaption></figcaption></figure>

#### Extracting Compressed files&#x20;

<table><thead><tr><th width="190.00006103515625">Option</th><th width="310.79998779296875">Description</th></tr></thead><tbody><tr><td><code>-d</code></td><td>Extracting files in specific directory</td></tr><tr><td><code>-l</code></td><td>Listing archive contents</td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
unzip <zip file> -d <folder> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1324).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1325).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1326).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1327).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1328).png" alt=""><figcaption></figcaption></figure>

#### Locate

The **`locate`** command searches for files using a **pre-built database** instead of scanning the filesystem. As a result, it is much faster than `find`.

* update the database: `sudo updatedb`
* search for files: `locate <filename>`

<figure><img src="../../../.gitbook/assets/image (1329).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1330).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1331).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1333).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1334).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (1335).png" alt=""><figcaption></figcaption></figure>

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

## Users and Groups&#x20;

### Types of Users&#x20;

Linux distinguishes users by their **UID (User ID)**, which determines their privilege level and purpose on the system.

<table data-search="false"><thead><tr><th width="149.99993896484375">UID Range</th><th width="562.7999877929688">Type</th></tr></thead><tbody><tr><td><strong>0</strong></td><td><strong>root</strong> — superuser, full access, exempt from permission checks</td></tr><tr><td><strong>1–999</strong></td><td><strong>System / service accounts</strong> — created for daemons and services (e.g., <code>www-data</code>, <code>sshd</code>), not meant for interactive login</td></tr><tr><td><strong>1000+</strong></td><td><strong>Normal users</strong> — regular human accounts, created via <code>useradd</code>/<code>adduser</code></td></tr></tbody></table>

### User Information

{% code overflow="wrap" %}
```bash
whoami
id
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

### Passwd file&#x20;

`/etc/passwd` stores basic account information for every user on the system. It's world-readable — it holds no password hashes.

<table data-search="false"><thead><tr><th width="123.60003662109375">Field</th><th width="381.99993896484375">Description</th></tr></thead><tbody><tr><td><strong>Username</strong></td><td>Login name</td></tr><tr><td><strong>Password</strong></td><td>Always <code>x</code> — actual hash lives in <code>/etc/shadow</code></td></tr><tr><td><strong>UID</strong></td><td>User ID</td></tr><tr><td><strong>GID</strong></td><td>Primary Group ID</td></tr><tr><td><strong>Comment</strong></td><td>Full name / description (GECOS field)</td></tr><tr><td><strong>Home Directory</strong></td><td>Path to the user's home</td></tr><tr><td><strong>Shell</strong></td><td>Default shell on login</td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
cat /etc/passwd
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Service accounts usually have `/usr/sbin/nologin` or `/bin/false` as their shell in `/etc/passwd` — this blocks interactive login even if someone obtains the password._
{% endhint %}

### Shadow file

`/etc/shadow` stores the actual password hashes and aging policy. It's readable only by **root**.

<table data-search="false"><thead><tr><th width="123.60003662109375">Field</th><th width="445.19989013671875">Description</th></tr></thead><tbody><tr><td><strong>Username</strong></td><td>Login name</td></tr><tr><td><strong>Password Hash</strong></td><td>Hashed password (or <code>!</code>/<code>*</code> if locked/disabled)</td></tr><tr><td><strong>Last Changed</strong></td><td>Days since epoch password was last changed</td></tr><tr><td><strong>Min Days</strong></td><td>Minimum days before password can be changed again</td></tr><tr><td><strong>Max Days</strong></td><td>Maximum days before password must be changed</td></tr><tr><td><strong>Warn Days</strong></td><td>Days before expiry the user is warned</td></tr><tr><td><strong>Inactive</strong></td><td>Days after expiry before account is disabled</td></tr><tr><td><strong>Expire Date</strong></td><td>Absolute account expiry date</td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
sudo cat /etc/shadow
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_The hash prefix identifies the algorithm — `$1$` MD5, `$5$` SHA-256, `$6$` SHA-512. Ubuntu defaults to SHA-512 (`$6$`)._
{% endhint %}

#### Check group membership&#x20;

{% code overflow="wrap" %}
```bash
groups
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

### Creating and Managing Users&#x20;

#### Creating a user&#x20;

1. `useradd`

<table data-search="false"><thead><tr><th width="171.4000244140625">Option</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><code>-m</code></td><td>Create the user's home directory</td><td><code>sudo useradd -m alice</code></td></tr><tr><td><code>-d &#x3C;dir></code></td><td>Specify a custom home directory</td><td><code>sudo useradd -d /home/dev alice</code></td></tr><tr><td><code>-s &#x3C;shell></code></td><td>Set the login shell</td><td><code>sudo useradd -s /bin/bash alice</code></td></tr><tr><td><code>-u &#x3C;UID></code></td><td>Assign a specific UID</td><td><code>sudo useradd -u 1500 alice</code></td></tr><tr><td><code>-g &#x3C;group></code></td><td>Set the primary group</td><td><code>sudo useradd -g developers alice</code></td></tr><tr><td><code>-G &#x3C;groups></code></td><td>Add supplementary groups</td><td><code>sudo useradd -G sudo,docker alice</code></td></tr><tr><td><code>-c "&#x3C;comment>"</code></td><td>Add GECOS/comment (full name, etc.)</td><td><code>sudo useradd -c "Alice Smith" alice</code></td></tr><tr><td><code>-e &#x3C;YYYY-MM-DD></code></td><td>Set account expiration date</td><td><code>sudo useradd -e 2026-12-31 alice</code></td></tr><tr><td><code>-f &#x3C;days></code></td><td>Days after password expiry before disabling account</td><td><code>sudo useradd -f 30 alice</code></td></tr><tr><td><code>-p &#x3C;encrypted-password></code></td><td>Set an <strong>already encrypted</strong> password (avoid plain text)</td><td><code>sudo useradd -p '$6$...' alice</code></td></tr><tr><td><code>-r</code></td><td>Create a system account</td><td><code>sudo useradd -r nginx</code></td></tr><tr><td><code>-M</code></td><td>Do <strong>not</strong> create a home directory</td><td><code>sudo useradd -M serviceuser</code></td></tr><tr><td><code>-N</code></td><td>Do not create a user-private group</td><td><code>sudo useradd -N alice</code></td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
sudo useradd [OPTIONAL-OPTIONS] <username>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

2. adduser

{% code overflow="wrap" %}
```bash
adduser
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

#### Setting or changing password&#x20;

{% code overflow="wrap" %}
```bash
sudo passwd <username>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

#### Locking and Unlocking a user&#x20;

**Way - 1**

{% code overflow="wrap" %}
```bash
sudo passwd -l john
sudo passwd -u john
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

**Way - 2**&#x20;

{% code overflow="wrap" %}
```bash
sudo usermod -L <username>
sudo usermod -U <username>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

#### Modifying User Information&#x20;

<table data-search="false"><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>usermod -l newuser olduser</code></td><td>Rename a user</td></tr><tr><td><code>usermod -u 2001 user</code></td><td>Change the user's UID</td></tr><tr><td><code>usermod -d /home/newdir user</code></td><td>Change the home directory</td></tr><tr><td><code>usermod -d /home/newdir -m user</code></td><td>Move the home directory to a new location</td></tr><tr><td><code>usermod -s /bin/zsh user</code></td><td>Change the login shell</td></tr><tr><td><code>usermod -g group user</code></td><td>Change the primary group</td></tr><tr><td><code>usermod -aG group1,group2 user</code></td><td>Add supplementary groups</td></tr><tr><td><code>usermod -e YYYY-MM-DD user</code></td><td>Set the account expiration date</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

#### Deleting User&#x20;

{% code overflow="wrap" %}
```
sudo userdel -r <username>
# -r => remove home directory and mail spoof
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

### Types of Groups&#x20;

Every user belongs to exactly one **primary group** and can additionally belong to any number of **secondary (supplementary) groups**.

<table data-search="false"><thead><tr><th width="149.99993896484375">Type</th><th width="562.7999877929688">Description</th></tr></thead><tbody><tr><td><strong>Primary Group</strong></td><td>Assigned in the GID field of <code>/etc/passwd</code>. Applied by default to any file the user creates.</td></tr><tr><td><strong>Secondary Group</strong></td><td>Listed in <code>/etc/group</code>. Grants additional access — e.g., membership in <code>sudo</code>, <code>docker</code>, or a custom team group — without changing the primary group.</td></tr></tbody></table>

### Group File&#x20;

{% code overflow="wrap" %}
```bash
cat /etc/group
# Show all groups within the system

groups
# Shows groups the logged-in user belongs to 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

### Creating and Managing Groups&#x20;

#### Create a group&#x20;

| Command                       | Description                      |
| ----------------------------- | -------------------------------- |
| `groupadd developers`         | Create a new group               |
| `groupadd -g 2001 developers` | Create a group with a custom GID |

{% code overflow="wrap" %}
```bash
sudo groupadd developers
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

#### Modifying Groups&#x20;

| Command                       | Description            |
| ----------------------------- | ---------------------- |
| `groupmod -n dev developers`  | Rename a group         |
| `groupmod -g 3001 developers` | Change the group's GID |

{% code overflow="wrap" %}
```bash
sudo groupmod -n dev developers
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

#### Delete a group&#x20;

{% code overflow="wrap" %}
```bash
sudo groupdel <groupname>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

#### Managing Group membership&#x20;

| Command                       | Description                         |
| ----------------------------- | ----------------------------------- |
| `usermod -aG developers john` | Add a user to a supplementary group |
| `gpasswd -d john developers`  | Remove a user from a group          |

<figure><img src="../../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

## Installing Software

### Updating package information&#x20;

{% code overflow="wrap" %}
```bash
sudo apt udpate
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

### Upgrading packages&#x20;

{% code overflow="wrap" %}
```
sudo apt upgrade
sudo apt full-upgrade
```
{% endcode %}

* **`sudo apt upgrade`** – Upgrades installed packages **without removing or installing additional packages**.
* **`sudo apt full-upgrade`** – Upgrades packages **and allows installing or removing packages** to resolve dependencies and complete the upgrade.

<figure><img src="../../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

### Installing packages&#x20;

{% code overflow="wrap" %}
```bash
sudo apt install <package name/tool name> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

### Removing Package&#x20;

{% code overflow="wrap" %}
```
sudo apt remove <package name/tool name>
```
{% endcode %}

* **`sudo apt remove`** – Uninstalls the package **but keeps its system-wide configuration files**.
* **`sudo apt purge`** – Uninstalls the package **and removes its system-wide configuration files**.

<figure><img src="../../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

### Check Installed Packages

{% code overflow="wrap" %}
```bash
dpkg -l 
apt list --installed
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

### Installing Local Packages&#x20;

{% code overflow="wrap" %}
```
sudo dpkg -i package.deb
sudo apt install -f
```
{% endcode %}

### Cleaning Package cache&#x20;

{% code overflow="wrap" %}
```
sudo apt clean
sudo apt autoclean
sudo apt autoremove
```
{% endcode %}

* **`sudo apt clean`** – Removes **all downloaded package files** from the local APT cache (`/var/cache/apt/archives/`).
* **`sudo apt autoclean`** – Removes **only outdated or no longer downloadable package files** from the APT cache.
* **`sudo apt autoremove`** – Removes **unused packages** that were installed automatically as dependencies and are no longer needed.

## Working with the shell&#x20;

### Environment Variables&#x20;

**Environment variables** are named values held in memory by the shell, used by both the shell itself and the programs it launches to determine behavior — paths to search, default editors, locale, and more.

<table data-search="false"><thead><tr><th width="149.99993896484375">Variable</th><th width="562.7999877929688">Description</th></tr></thead><tbody><tr><td><code>PATH</code></td><td>Directories searched for executables</td></tr><tr><td><code>HOME</code></td><td>Current user's home directory</td></tr><tr><td><code>USER</code></td><td>Current logged-in username</td></tr><tr><td><code>SHELL</code></td><td>Path to the user's default shell</td></tr><tr><td><code>PWD</code></td><td>Current working directory</td></tr><tr><td><code>LANG</code></td><td>System language/locale</td></tr><tr><td><code>TERM</code></td><td>Terminal type</td></tr><tr><td><code>EDITOR</code></td><td>Default text editor for CLI tools</td></tr></tbody></table>

#### Listing all environment variables&#x20;

{% code overflow="wrap" %}
```bash
env
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

#### Printing specific variable

{% code overflow="wrap" %}
```bash
echo $<variable>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

#### Add an environment variable&#x20;

Add the below line in the `~/.bashrc` file and apply changes using `source ~/.bashrc`

{% code overflow="wrap" %}
```bash
export COMMENT="comment added by user" 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

### Startup Files&#x20;

Bash reads different configuration files depending on whether the shell is **login** or **non-login**, and **interactive** or **non-interactive**. This determines which settings actually apply when a terminal opens.

<table data-search="false"><thead><tr><th width="197.199951171875">File</th><th width="539.6000366210938">Loaded By</th></tr></thead><tbody><tr><td><code>/etc/profile</code></td><td>System-wide, login shells</td></tr><tr><td><code>/etc/profile.d/*.sh</code></td><td>System-wide, sourced from <code>/etc/profile</code></td></tr><tr><td><code>~/.bash_profile</code></td><td><p>Per-user, login shells (takes priority if present)</p><ul><li>A user-specific startup file that is executed <strong>once when a Bash login shell starts</strong> to configure the user's login environment (e.g., environment variables, <code>PATH</code>, and startup commands).</li></ul></td></tr><tr><td><code>~/.bash_login</code></td><td>Per-user, login shells (used if <code>.bash_profile</code> is absent)</td></tr><tr><td><code>~/.profile</code></td><td>Per-user, login shells (fallback, also used by <code>sh</code>)</td></tr><tr><td><code>~/.bashrc</code></td><td>Per-user, non-login interactive shells (e.g., opening a new terminal tab)</td></tr><tr><td><code>/etc/bash.bashrc</code></td><td>System-wide, non-login interactive shells</td></tr></tbody></table>

#### Login Flow&#x20;

{% code overflow="wrap" %}
```
User Login
     │
     ▼
~/.bash_profile (if exists)
     │
else ▼
~/.bash_login (if exists)
     │
else ▼
~/.profile
     │
     ▼
Sources ~/.bashrc (if using Bash)
     │
     ▼
Interactive Bash Shell Ready
```
{% endcode %}

### Redirecting Input and Output&#x20;

Every process starts with three open file descriptors — **stdin (0)**, **stdout (1)**, and **stderr (2)**. Redirection reroutes these to files instead of the terminal.

<table data-search="false"><thead><tr><th width="149.99993896484375">Operator</th><th width="562.7999877929688">Meaning</th></tr></thead><tbody><tr><td><code>></code></td><td>Redirect stdout, overwrite file</td></tr><tr><td><code>>></code></td><td>Redirect stdout, append to file</td></tr><tr><td><code>&#x3C;</code></td><td>Redirect stdin from a file</td></tr><tr><td><code>2></code></td><td>Redirect stderr, overwrite file</td></tr><tr><td><code>2>></code></td><td>Redirect stderr, append to file</td></tr><tr><td><code>&#x26;></code></td><td>Redirect both stdout and stderr</td></tr><tr><td><code>2>&#x26;1</code></td><td>Merge stderr into stdout's current destination</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1336).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1337).png" alt=""><figcaption></figcaption></figure>

### Pipes&#x20;

A **pipe (`|`)** connects the stdout of one command directly to the stdin of the next, without creating an intermediate file.

{% code overflow="wrap" %}
```
ps aux | grep ssh
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1338).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Redirection connects a command to a file. A pipe connects a command directly to another command's input — no file is ever written to disk in between._
{% endhint %}

### Command History

Bash keeps a record of executed commands, stored in `~/.bash_history` and loaded into memory each session.

<table data-search="false"><thead><tr><th width="255.60003662109375">Command</th><th width="296.4000244140625">Description</th></tr></thead><tbody><tr><td><code>history</code></td><td>List command history</td></tr><tr><td><code>history | grep ssh</code></td><td>Search history for a keyword</td></tr><tr><td><code>!!</code></td><td>Re-run the last command</td></tr><tr><td><code>!45</code></td><td>Re-run history entry number 45</td></tr><tr><td><code>!ssh</code></td><td>Re-run the last command starting with <code>ssh</code></td></tr><tr><td><code>Ctrl + R</code></td><td>Reverse-search history interactively</td></tr><tr><td><code>history -c</code></td><td><p></p><p>Clear history for the current session</p></td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1339).png" alt=""><figcaption></figcaption></figure>

## Text Editors&#x20;

### Nano

<figure><img src="../../../.gitbook/assets/image (1340).png" alt=""><figcaption></figcaption></figure>

### Vim&#x20;

<table data-search="false"><thead><tr><th>Command / Key</th><th>Description</th></tr></thead><tbody><tr><td><code>vim file.txt</code></td><td>Open or create a file</td></tr><tr><td><code>i</code></td><td>Enter Insert mode before the cursor</td></tr><tr><td><code>a</code></td><td>Enter Insert mode after the cursor</td></tr><tr><td><code>o</code></td><td>Insert a new line below</td></tr><tr><td><code>Esc</code></td><td>Return to Normal mode</td></tr><tr><td><code>h</code> <code>j</code> <code>k</code> <code>l</code></td><td>Move left, down, up, right</td></tr><tr><td><code>gg</code></td><td>Go to the first line</td></tr><tr><td><code>G</code></td><td>Go to the last line</td></tr><tr><td><code>:10</code></td><td>Go to line 10</td></tr><tr><td><code>dd</code></td><td>Delete the current line</td></tr><tr><td><code>p</code></td><td>Paste copied/deleted text</td></tr><tr><td><code>u</code></td><td>Undo last change</td></tr><tr><td><code>Ctrl + r</code></td><td>Redo last undone change</td></tr><tr><td><code>/text</code></td><td>Search for text</td></tr><tr><td><code>n</code></td><td>Next search result</td></tr><tr><td><code>N</code></td><td>Previous search result</td></tr><tr><td><code>:w</code></td><td>Save the file</td></tr><tr><td><code>:q</code></td><td>Quit Vim</td></tr><tr><td><code>:wq</code></td><td>Save and quit</td></tr><tr><td><code>:q!</code></td><td>Quit without saving</td></tr><tr><td><code>:set number</code></td><td>Show line numbers</td></tr><tr><td><code>:set nonumber</code></td><td>Hide line numbers</td></tr><tr><td><code>:!&#x3C;command></code></td><td>Execute a shell command without leaving Vim</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1341).png" alt=""><figcaption></figcaption></figure>

## Process Management

### Viewing Process&#x20;

<table data-search="false"><thead><tr><th>Command</th><th>Description</th><th>Expected Output</th></tr></thead><tbody><tr><td><code>ps</code></td><td>Display processes running in the current terminal</td><td>PID, TTY, TIME, CMD</td></tr><tr><td><code>ps -e</code></td><td>Display all running processes</td><td>List of all system processes</td></tr><tr><td><code>ps -ef</code></td><td>Display all processes in full format</td><td>UID, PID, PPID, STIME, CMD</td></tr><tr><td><code>ps aux</code></td><td>Display detailed process information (BSD format)</td><td>USER, PID, %CPU, %MEM, COMMAND</td></tr><tr><td><code>top</code></td><td>Display real-time running processes</td><td>Interactive process monitor</td></tr><tr><td><code>htop</code></td><td>Interactive process viewer (if installed)</td><td>Color-coded process list</td></tr><tr><td><code>pstree</code></td><td>Display processes in a tree hierarchy</td><td>Parent-child process tree</td></tr><tr><td><code>pstree -p</code></td><td>Display process tree with PIDs</td><td>Process hierarchy with PIDs</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (1342).png" alt=""><figcaption></figcaption></figure>

### Find a process by port&#x20;

#### Using lsof (List Open Files)

{% code overflow="wrap" %}
```
sudo lsof -i :<port>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1343).png" alt=""><figcaption></figcaption></figure>

#### Using ss (Socket investigation Utility)

{% code overflow="wrap" %}
```
sudo ss -tulpn | grep <port>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1344).png" alt=""><figcaption></figcaption></figure>

#### Using ps&#x20;

{% code overflow="wrap" %}
```
ps -aux | grep <port>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1345).png" alt=""><figcaption></figcaption></figure>

#### Using pgrep

{% code overflow="wrap" %}
```
pgrep -l ssh
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1346).png" alt=""><figcaption></figcaption></figure>

### Identify Process id with process name&#x20;

{% code overflow="wrap" %}
```
pidof <process name>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1347).png" alt=""><figcaption></figcaption></figure>

### Killing Process&#x20;

<table data-search="false"><thead><tr><th width="255.60003662109375">Command</th><th width="296.4000244140625">Description</th></tr></thead><tbody><tr><td><code>kill PID</code></td><td>SIGTERM — graceful stop</td></tr><tr><td><code>kill -9 PID</code></td><td>SIGKILL — force kill</td></tr><tr><td><code>kill -1 PID</code></td><td>SIGHUP — reload config (common for daemons)</td></tr><tr><td><code>pkill -f "process_name"</code></td><td>Kill by matching command line</td></tr><tr><td><code>killall process_name</code></td><td>Kill all instances by exact name</td></tr></tbody></table>

{% code overflow="wrap" %}
```bash
kill <pid>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1348).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1349).png" alt=""><figcaption></figcaption></figure>

## Text Processing&#x20;

{% hint style="info" %}
_Linux treats **everything as text**. Configuration files, logs, passwords, command outputs, enumeration results, scan reports, etc. Instead of manually opening files, Linux provides powerful text processing utilities that allow searching, filtering, extracting, modifying, and transforming data directly from the command line._
{% endhint %}

### Regular Expressions&#x20;

A **regex** is a pattern-matching language for text.

#### Core Metacharacters

* `^` — anchors to the start of a line
* `$` — anchors to the end of a line
* `.` — matches any single character
* `*` — zero or more of the preceding character
* `[abc]` — any one character in the set
* `[^abc]` — any one character NOT in the set
* `[0-9]`, `[a-zA-Z]` — ranges
* `\.` — escapes a literal dot (metacharacters must be escaped to be literal)
* `+` — one or more (needs `-E`)
* `?` — zero or one (needs `-E`)
* `{n,m}` — between n and m repetitions (needs `-E`)
* `(...)` — grouping (needs `-E`)
* `|` — alternation / OR (needs `-E`)
* `\d`, `\w`, `\s` — digit/word/whitespace shorthand — only with `grep -P` (PCRE), not standard BRE/ERE

<figure><img src="../../../.gitbook/assets/image (1350).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Explanation of commands</summary>

#### 1. Extract Email Addresses

```bash
grep -oE "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" regex_practice.txt
```

* `grep` – Searches for text matching a pattern.
* `-E` – Enables **Extended Regular Expressions (ERE)**.
* `-o` – Prints **only the matched text**, not the entire line.
* `[a-zA-Z0-9._%+-]+` – Matches the **username** part of an email (one or more letters, digits, `.`, `_`, `%`, `+`, `-`).
* `@` – Matches the `@` symbol.
* `[a-zA-Z0-9.-]+` – Matches the **domain name** (e.g., `gmail`, `example.co`).
* `\.` – Matches the literal dot (`.`) before the top-level domain.
* `[a-zA-Z]{2,}` – Matches the **top-level domain (TLD)** containing at least 2 letters (e.g., `com`, `org`, `in`).
* `regex_practice.txt` – File to search.

***

#### 2. Extract IPv4 Addresses

```bash
grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" regex_practice.txt
```

* `grep` – Searches for text matching a pattern.
* `-E` – Enables **Extended Regular Expressions (ERE)**.
* `-o` – Prints **only the matched text**, not the entire line.
* `([0-9]{1,3}\.){3}` – Matches **three groups** of 1–3 digits followed by a dot (`.`), e.g., `192.168.1.`
* `[0-9]{1,3}` – Matches the **last octet** (1–3 digits).
* `regex_practice.txt` – File to search.
* **Note:** This regex matches the IPv4 **format only**. It does **not** validate that each octet is within the valid range (`0–255`), so values like `999.999.999.999` will also match.

</details>

### cut

`cut` extracts specific columns from delimiter-based text — faster than `awk` when you just need to pull one or two fields and don't need any logic.

Key flags:

* `-d` — set the delimiter
* `-f` — select field(s) by number
* `-c` — select by character position/range
* `--complement` — invert the selected fields

<figure><img src="../../../.gitbook/assets/image (1356).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1357).png" alt=""><figcaption></figcaption></figure>

### awk - Field processing

Where `cut` only extracts, `awk` extracts, filters by condition, and computes — in one line. Structure: `awk 'pattern {action}' file`. `$1`, `$2`... are fields, `$0` is the whole line, `NR` is the current record number, `NF` is the number of fields, `-F` sets the delimiter.

<figure><img src="../../../.gitbook/assets/image (1358).png" alt=""><figcaption></figcaption></figure>

### sed - Stream Editing&#x20;

`sed` edits a text stream — substituting, printing, or deleting lines — without opening the file in an editor. Critical for bulk-editing configs or sanitizing logs.

Key syntax:

* `s/pattern/replacement/` — substitute the first match per line
* `s/pattern/replacement/g` — substitute all matches per line
* `-i` — edit the file in place (use `-i.bak` to keep a backup)
* `-n` with `p` — print only matching lines (suppresses default output)
* `d` — delete matching lines
* `^` — anchor a replacement/action to the start of a line

{% hint style="danger" %}
_Too complex. AI usage is must. also make sure you've backup of your file somewhere else as well before using sed. as it may removes stuffs. Use carefully._&#x20;
{% endhint %}

Other tools: `sort`, `uniq`, `wc`, `tr`

## Authentication&#x20;

### PAM (Pluggable Authentication Modules)

PAM is a modular framework that decouples applications (login, sshd, su, sudo) from the actual authentication mechanism. Instead of each program hardcoding "check /etc/passwd", it calls into PAM, which loads a stack of modules defined in config files. This is why you can plug in fingerprint auth, LDAP, Kerberos, or 2FA without recompiling every program.

**Each PAM rule has the format:**&#x20;

{% code overflow="wrap" %}
```
type    control    module-path    arguments
```
{% endcode %}

#### Types&#x20;

* `auth` — verifies identity (password, key, biometric)
* `account` — checks account validity (expired, locked, time restrictions)
* `password` — handles updating credentials
* `session` — sets up/tears down session (mounting home dir, logging, env vars)

**Control flags:**

* `required` — must pass, but continues evaluating rest of stack before failing
* `requisite` — must pass, fails immediately (short-circuits)
* `sufficient` — if it passes, stack succeeds immediately (unless a prior `required` failed)
* `optional` — doesn't usually affect outcome unless it's the only module

#### Authentication Flow&#x20;

{% code overflow="wrap" %}
```
User attempts login
        │
        ▼
  auth stack (are you who you say you are?)
        │  (PAM modules run top-to-bottom per control flags)
        ▼
  account stack (is this account allowed to log in right now?)
        │
        ▼
  password stack (only invoked when changing password)
        │
        ▼
  session stack (setup env, mount home, write logs)
```
{% endcode %}

#### /etc/pam.d

Directory holding one config file per service (`sshd`, `login`, `sudo`, `su`, `passwd`, etc.). Each file lists the rule stack for that specific program.

{% code overflow="wrap" %}
```bash
ls /etc/pam.d
cat /etc/pam.d/sshd
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1359).png" alt=""><figcaption></figcaption></figure>

#### common-auth&#x20;

Shared `auth` stack included by most service files. Defines how a user actually proves identity system-wide (usually `pam_unix.so` checking `/etc/shadow`).

<figure><img src="../../../.gitbook/assets/image (1360).png" alt=""><figcaption></figcaption></figure>

#### common-account&#x20;

Shared `account` stack — checks things like account expiry (`/etc/shadow` expiry fields), whether the account is locked, or time-based access restrictions (`pam_time.so`).

<figure><img src="../../../.gitbook/assets/image (1361).png" alt=""><figcaption></figcaption></figure>

#### common-password&#x20;

Shared `password` stack — enforces password complexity/history when a user runs `passwd`. This is where `pam_pwquality.so` / `pam_cracklib.so` rules live (min length, complexity classes, reuse history).

<figure><img src="../../../.gitbook/assets/image (1362).png" alt=""><figcaption></figcaption></figure>

## Sudo&#x20;

`sudo` ("superuser do") lets an authorized user run commands as another user (typically root) based on rules in `/etc/sudoers`, without sharing the root password. Every invocation is logged (usually to `/var/log/auth.log` or via `syslog`), unlike raw `su`.

### List Sudo Permissions (\`sudo -l\`)

Lists what the _current user_ is permitted to run via sudo — the single most important enumeration command on any box.

{% code overflow="wrap" %}
```
sudo -l
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1363).png" alt=""><figcaption></figcaption></figure>

### Sudoers file (/etc/sudoers)

Master config file defining who can run what, as whom, and under what conditions.

{% code overflow="wrap" %}
```
cat /etc/sudoers
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1364).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
%admin ALL=(ALL) ALL
```
{% endcode %}

<table><thead><tr><th width="114.5999755859375">Field</th><th width="213.59991455078125">Value</th><th width="279.00006103515625">Meaning</th></tr></thead><tbody><tr><td><code>%admin</code></td><td>User Specification</td><td>Members of the <strong>admin group</strong></td></tr><tr><td><code>ALL</code></td><td>Host Specification</td><td>On all hosts</td></tr><tr><td><code>(ALL)</code></td><td>Run As Specification</td><td>Can run commands as any user</td></tr><tr><td><code>ALL</code></td><td>Command Specification</td><td>Can execute any command</td></tr></tbody></table>

**`%admin ALL=(ALL) ALL`** — Allows members of the **`admin`** group to execute **any command on any host as any user**.

```
%sudo ALL=(ALL:ALL) ALL
```

* Members of the **sudo** group can execute **any command**.
* Can run commands as:
  * Any user
  * Any group

This is the default configuration on Ubuntu.

### Modifying sudoers file&#x20;

<figure><img src="../../../.gitbook/assets/image (1365).png" alt=""><figcaption></figcaption></figure>

#### With NOPASSWD

Allows the command to run without re-prompting for a password. High-value from an offensive standpoint — if a `NOPASSWD` binary can be abused (GTFOBins), it's an instant root path with no credential needed.

<figure><img src="../../../.gitbook/assets/image (1366).png" alt=""><figcaption></figcaption></figure>

#### visudo&#x20;

The **only safe way** to edit `/etc/sudoers` — it locks the file, validates syntax before saving, and prevents you from locking yourself out with a broken config.

### Secure\_path&#x20;

A sudoers `Defaults` setting that overrides `$PATH` when a command is run via sudo, preventing PATH-hijacking attacks (planting a malicious binary earlier in a user-controlled PATH).

<figure><img src="../../../.gitbook/assets/image (1367).png" alt=""><figcaption></figcaption></figure>

If `secure_path` is **not set**, and a sudoers entry allows running a command without an absolute path, PATH hijacking becomes possible.

<details>

<summary><strong>Detailed Explanation</strong></summary>

## Step 1: What is PATH?

Suppose you type

```bash
ls
```

How does Linux know where `ls` is?

It checks the `PATH` environment variable.

Example:

```bash
echo $PATH
```

Output:

```
/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin
```

Linux searches directories **from left to right.**

It literally does:

```
Is ls inside /usr/local/bin?
        No

Is ls inside /usr/bin?
        Yes

Execute /usr/bin/ls
```

It never checks further once it finds it.

***

## Step 2: What if PATH starts with my directory?

Suppose

```bash
PATH=/home/r0b/bin:/usr/bin:/bin
```

Now create

```bash
mkdir ~/bin

nano ~/bin/ls
```

Contents

```bash
#!/bin/bash

echo "Hacked!"
```

Make executable

```bash
chmod +x ~/bin/ls
```

Now

```bash
ls
```

Output

```
Hacked!
```

NOT

```
Desktop
Downloads
Pictures
```

Because Linux found your fake `ls` first.

This is called **PATH precedence**.

***

## Step 3: What happens with sudo?

Suppose

```bash
sudo ls
```

How does sudo find `ls`?

There are TWO possibilities.

***

### Case 1

sudo uses YOUR PATH.

Suppose

```
PATH=/home/r0b/bin:/usr/bin:/bin
```

sudo checks

```
/home/r0b/bin/ls
```

Finds it.

Runs it **AS ROOT.**

Your fake script now executes with root privileges.

Very dangerous.

***

### Case 2

sudo ignores your PATH.

Instead it uses

```
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Now

```
sudo ls
```

always becomes

```
/usr/bin/ls
```

Your fake program is ignored.

Safe.

***

## Step 4: This is exactly why secure\_path exists.

In `/etc/sudoers`

```
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
```

This means

> Whenever sudo executes a command, ignore the user's PATH and use this trusted PATH instead.

So even if the user changes

```bash
export PATH=/tmp:$PATH
```

sudo does

```
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

before running the command.

***

## Step 5: Why is this needed?

Imagine a system administrator wrote

```bash
sudo backup
```

instead of

```bash
sudo /usr/local/bin/backup
```

If sudo trusted the user's PATH...

Attacker creates

```
/tmp/backup
```

```bash
#!/bin/bash

cp /bin/bash /tmp/rootbash
chmod +s /tmp/rootbash
```

Changes PATH

```
export PATH=/tmp:$PATH
```

Admin runs

```
sudo backup
```

Oops.

Instead of

```
/usr/local/bin/backup
```

it executes

```
/tmp/backup
```

as root.

Game over.

This is called **PATH hijacking**.

***

## Step 6: But I still need sudo password!!

Exactly.

This is the point that confuses everyone.

Suppose

```
sudo backup
```

asks

```
[sudo] password for r0b:
```

You DON'T know the password.

Nothing happens.

So merely changing PATH does NOT give you root.

***

## Step 7: Then why do pentesters care?

Because of **sudo permissions**.

Suppose

```
sudo -l
```

shows

```
User r0b may run:

(root) NOPASSWD: /usr/bin/find
```

or

```
(root) NOPASSWD: ALL
```

Now you don't need a password.

</details>

## Access Control Lists (ACL)

Standard Linux permissions (owner/group/other, rwx) only allow **one** owning user and **one** owning group per file. ACLs extend this to grant fine-grained permissions to specific additional users or groups on a single file/directory, without changing standard ownership.

A file with an ACL shows a `+` after its permission bits:

<figure><img src="../../../.gitbook/assets/image (1368).png" alt=""><figcaption></figcaption></figure>

### getfacl&#x20;

{% code overflow="wrap" %}
```
getfacl <file>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1369).png" alt=""><figcaption></figcaption></figure>

### setfacl

Sets or modifies ACL entries.&#x20;

{% code overflow="wrap" %}
```
setfacl -m u:abhishek:rwx file.txt     # grant user rwx
setfacl -m g:developers:rx  file.txt   # grant group r-x
setfacl -x u:abhishek file.txt         # remove a specific entry
setfacl -b file.txt                    # remove ALL ACL entries
```
{% endcode %}

## Linux Capabilities&#x20;

Capabilities split the monolithic power of "root" into distinct, independently grantable units. Instead of giving a binary full root via SUID, you can give it just the one root-level privilege it actually needs (e.g., binding to a low port).

### Capability sets&#x20;

Each process/file has multiple capability sets:

* **Permitted** — capabilities the process is allowed to use
* **Effective** — capabilities currently active/in-use. automatically enabled when program starts.&#x20;
* **Inheritable** — capabilities passed down to child processes on exec
* **Bounding** — upper limit on what the process can ever acquire
* **Ambient** — capabilities preserved across `execve` for non-SUID/non-capability-aware programs

### Some Common Capabilities&#x20;

<table data-search="false"><thead><tr><th width="198.79998779296875">Capability</th><th>Allows</th></tr></thead><tbody><tr><td><code>CAP_NET_BIND_SERVICE</code></td><td>Bind to privileged ports (1–1023), such as port 80 or 443.</td></tr><tr><td><code>CAP_NET_ADMIN</code></td><td>Configure network interfaces, routing tables, firewall rules, and network settings.</td></tr><tr><td><code>CAP_NET_RAW</code></td><td>Create raw sockets for tools like <code>ping</code>, <code>tcpdump</code>, and packet crafting.</td></tr><tr><td><code>CAP_SYS_ADMIN</code></td><td>Perform various system administration tasks such as mounting filesystems and namespace management.</td></tr><tr><td><code>CAP_CHOWN</code></td><td>Change the ownership of files and directories.</td></tr><tr><td><code>CAP_DAC_OVERRIDE</code></td><td>Bypass file permission checks for reading, writing, and executing files.</td></tr><tr><td><code>CAP_SETUID</code></td><td>Change the process's User ID (UID).</td></tr><tr><td><code>CAP_SETGID</code></td><td>Change the process's Group ID (GID).</td></tr><tr><td><code>CAP_KILL</code></td><td>Send signals (e.g., <code>SIGTERM</code>, <code>SIGKILL</code>) to processes owned by other users.</td></tr><tr><td><code>CAP_SYS_CHROOT</code></td><td>Use <code>chroot()</code> to change the root directory of a process.</td></tr></tbody></table>

Capabilities can be attached to file or processes.&#x20;

### Viewing Capabilities&#x20;

{% code overflow="wrap" %}
```
getcap <file>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1370).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
ep means: 
e => The capability is automatically enabled when the program starts.
p => When /usr/bin/ping starts, it is allowed to use CAP_NET_RAW.
```
{% endcode %}

### Set Capabilities&#x20;

Assign a capability to a binary (requires root)

{% code overflow="wrap" %}
```
sudo setcap <capability> <file>
```
{% endcode %}

**cap\_setuid** — allows a process to change its UID, including to `0` (root). If set on an interpreter (python, perl), it's an instant privesc.

<figure><img src="../../../.gitbook/assets/image (1371).png" alt=""><figcaption></figcaption></figure>

## Linux Networking&#x20;

### hostname

The system's network identity/name.

<figure><img src="../../../.gitbook/assets/image (1372).png" alt=""><figcaption></figcaption></figure>

### hostnamectl&#x20;

Modern systemd tool to view/set hostname and related metadata (also shows OS, kernel, architecture — useful recon).

<figure><img src="../../../.gitbook/assets/image (1373).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1374).png" alt=""><figcaption></figcaption></figure>

### hosts file

Static, manual hostname → IP mappings, checked **before** DNS resolution.

<figure><img src="../../../.gitbook/assets/image (1375).png" alt=""><figcaption></figcaption></figure>

### resolve.conf&#x20;

Defines which DNS servers the system queries for name resolution.

<figure><img src="../../../.gitbook/assets/image (1376).png" alt=""><figcaption></figcaption></figure>

### Network Interfaces&#x20;

Physical/virtual adapters through which the machine sends/receives traffic (`eth0`, `ens33`, `lo`, `wlan0`, etc.).

{% code overflow="wrap" %}
```
ls /sys/class/net/
ip link show
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1377).png" alt=""><figcaption></figcaption></figure>

### ip command&#x20;

Modern, unified tool (replaces `ifconfig`, `route`, `arp` individually) for interfaces, addresses, and routing.

{% code overflow="wrap" %}
```
ip a                  # show all addresses
ip addr show eth0      # specific interface
ip link set eth0 up    # bring interface up
ip route               # show routing table
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1378).png" alt=""><figcaption></figcaption></figure>

### ss

Modern replacement for `netstat` — shows socket statistics (listening ports, established connections) much faster.

{% code overflow="wrap" %}
```
ss -tulnp     # tcp/udp, listening, numeric, with process
ss -antp      # all tcp connections with process info
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1379).png" alt=""><figcaption></figcaption></figure>

## Scheduled Tasks&#x20;

### cron

The classic time-based job scheduler. A background daemon (`crond`) reads crontab files and executes commands at specified times.

{% code overflow="wrap" %}
```
* * * * *  command
│ │ │ │ │
│ │ │ │ └── day of week (0-6, Sun=0)
│ │ │ └──── month (1-12)
│ │ └────── day of month (1-31)
│ └──────── hour (0-23)
└────────── minute (0-59)
```
{% endcode %}

### List Scheduled Tasks&#x20;

{% code overflow="wrap" %}
```
crontab -l
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1380).png" alt=""><figcaption></figcaption></figure>

### Cron file locations&#x20;

<table><thead><tr><th width="294">Location</th><th>Purpose</th></tr></thead><tbody><tr><td><code>/var/spool/cron/crontabs/&#x3C;user></code></td><td>Individual user crontabs (<code>crontab -e</code>)</td></tr><tr><td><code>/etc/crontab</code></td><td>System-wide crontab</td></tr><tr><td><code>/etc/cron.d/</code></td><td>Additional system-wide cron files installed by packages or administrators</td></tr></tbody></table>

### Create Scheduled tasks&#x20;

#### Using User Crontabs

{% code overflow="wrap" %}
```
mkdir ~/scripts
nano ~/scripts/test.sh
chmod +x ~/scripts/test.sh
crontab -e
* * * * * /home/r0b/scripts/test.sh
crontab -l
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (1381).png" alt=""><figcaption></figcaption></figure>

Wait for one minute and check `/tmp/cron_test.log`.&#x20;

<figure><img src="../../../.gitbook/assets/image (1382).png" alt=""><figcaption></figcaption></figure>

It is written over here&#x20;

<figure><img src="../../../.gitbook/assets/image (1383).png" alt=""><figcaption></figcaption></figure>

#### Using system-wide crontab&#x20;

it is system-wide job scheduler.&#x20;

Since it's shared by the whole system, it can schedule jobs for **any user**.

<figure><img src="../../../.gitbook/assets/image (1384).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1385).png" alt=""><figcaption></figcaption></figure>
