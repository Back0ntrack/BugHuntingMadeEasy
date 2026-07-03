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



