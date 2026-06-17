# FTP (21)

**FTP (File Transfer Protocol)** is an application-layer protocol used to transfer files between a client and a server.

{% hint style="info" %}
Note that in FTP&#x20;
{% endhint %}

* TCP/21 → Control Channel (commands, authentication)
* TCP/20 → Data Channel (Active Mode)

## FTP Setup&#x20;

### Install ftp

{% code overflow="wrap" %}
```bash
sudo apt update
sudo apt install vsftpd
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (645).png" alt=""><figcaption></figcaption></figure>

### Running Server&#x20;

{% code overflow="wrap" %}
```bash
sudo systemctl start vsftpd

# Use this in case of errors while starting FTP server. this is the actual binary. 
sudo /usr/sbin/vsftpd /etc/vsftpd.conf 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (647).png" alt=""><figcaption></figcaption></figure>

## Default files

### Main Configuration File&#x20;

{% code overflow="wrap" %}
```bash
/etc/vsftpd.conf
```
{% endcode %}

* Main VSFTPD configuration
* Authentication
* Anonymous access
* Uploads
* TLS
* Chroot
* Logging

{% hint style="info" %}
_We'll see some common configuration settings of this file later._&#x20;
{% endhint %}

### PAM Authentication&#x20;

{% code overflow="wrap" %}
```bash
/etc/pam.d/vsftpd
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (646).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
1. PAM checks /etc/ftpusers using pam_listfile.so
   └─ If user exists in /etc/ftpusers → DENY login.

2. PAM executes common-auth
   └─ Verifies password against Linux authentication backend
      (typically /etc/shadow).

4. PAM executes common-account
   └─ Checks whether account is valid.
   └─ Checks account expiry, lock status, etc.

5. PAM executes common-session
   └─ Creates and manages the user session.

6. PAM executes pam_shells.so
   └─ Checks whether user's login shell exists in /etc/shells.
   └─ If shell is not listed → DENY login.

7. PAM returns SUCCESS or FAILURE to vsftpd.

8. vsftpd allows or denies the FTP session
```
{% endcode %}

## FTP Basics&#x20;

### FTP Channels&#x20;

FTP uses **two separate TCP connections**:

```
Control Channel (21)      → Used for sending FTP Commands
Data Channel (any port)   → Used for File transfer / Directory listing
```

### Active V/s Passive

<figure><img src="../../../.gitbook/assets/image (655).png" alt=""><figcaption></figcaption></figure>

### Passive Mode (Default)

{% hint style="info" %}
_Note that passive mode is now default mode for FTP in modern FTP servers._&#x20;
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (651).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_• FTP commands such as USER, PASS, PWD, TYPE, and SIZE use only the control channel (TCP/21)._

_• When a data-transfer operation such as LIST (ls) or RETR (get) is requested, the FTP client automatically negotiates Extended Passive Mode (EPSV), establishes a separate TCP data connection to the server's random passive port, and transfers the data over that connection._
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (652).png" alt=""><figcaption></figcaption></figure>

### Active Mode&#x20;

<figure><img src="../../../.gitbook/assets/image (656).png" alt=""><figcaption></figcaption></figure>

In the Wireshark capture, we can see that the client sends a request specifying the port that will be used for data communication. The server then initiates a TCP three-way handshake to that port, and the data is transferred from the server's port 20 to the client-specified port.

<figure><img src="../../../.gitbook/assets/image (654).png" alt=""><figcaption></figcaption></figure>

## Connecting to FTP&#x20;

{% hint style="info" %}
_Note that when connecting to an FTP server, users are typically placed in the home directory of the authenticated account._
{% endhint %}

### FTP Commands&#x20;

| FTP Client Equivalent    | FTP Protocol Command | Usage                        |
| ------------------------ | -------------------- | ---------------------------- |
| `user`                   | `USER`               | Specify username             |
| _(automatic after user)_ | `PASS`               | Specify password             |
| `quote SYST`             | `SYST`               | OS Fingerprinting            |
| `quote FEAT`             | `FEAT`               | Feature Enumeration          |
| `pwd`                    | `PWD`                | Show current directory       |
| `cd <dir>`               | `CWD`                | Change directory             |
| `ls` / `dir`             | `LIST`               | Detailed directory listing   |
| `nlist`                  | `NLST`               | Filename-only listing        |
| `get <file>`             | `RETR`               | Download file                |
| `put <file>`             | `STOR`               | Upload file                  |
| `delete <file>`          | `DELE`               | Delete file                  |
| `mkdir <dir>`            | `MKD`                | Create directory             |
| `rmdir <dir>`            | `RMD`                | Remove directory             |
| `rename <old> <new>`     | `RNFR` + `RNTO`      | Rename file                  |
| `quote SIZE <file>`      | `SIZE`               | Get file size                |
| `quote MDTM <file>`      | `MDTM`               | Get file timestamp           |
| `passive off`            | `PORT`               | Active Mode Data Connection  |
| `passive`                | `PASV`               | Passive Mode Data Connection |
| `ascii`                  | `TYPE A`             | ASCII Transfer Mode          |
| `binary`                 | `TYPE I`             | Binary Transfer Mode         |
| `quote NOOP`             | `NOOP`               | Keep connection alive        |
| `bye` / `quit`           | `QUIT`               | Terminate session            |

### Using FTP tool

{% code overflow="wrap" %}
```bash
ftp <ip>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (648).png" alt=""><figcaption></figcaption></figure>

#### basic commands sent&#x20;

<figure><img src="../../../.gitbook/assets/image (649).png" alt=""><figcaption></figcaption></figure>

### Using Netcat&#x20;

{% hint style="info" %}
_Note that we can only use the standard FTP commands over a Netcat connection to interact with an FTP server._
{% endhint %}

{% code overflow="wrap" %}
```bash
nc <ip> <port> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (650).png" alt=""><figcaption></figcaption></figure>

### Using Kali Explorer

Enter the following command in the address bar

{% code overflow="wrap" %}
```bash
ftp://192.168.16.142
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (657).png" alt=""><figcaption></figcaption></figure>

### Using FileZilla Client

<figure><img src="../../../.gitbook/assets/image (658).png" alt=""><figcaption></figcaption></figure>

## FTP Configuration&#x20;

{% hint style="info" %}
_Make sure to restart the service after you make changes to the configuration file._&#x20;
{% endhint %}

### Anonymous Login&#x20;

<figure><img src="../../../.gitbook/assets/image (659).png" alt=""><figcaption></figcaption></figure>

Turning this on will allow anonymous login.&#x20;

<figure><img src="../../../.gitbook/assets/image (660).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
_The `LIST` command failed because no data connection was established to the passive port (`42670`) returned by the server, so vsftpd had nowhere to send the directory listing.  But the agenda is to show that any password can be used against `anonymous` user._&#x20;
{% endhint %}

### Setup Guest users

1. Disable anonymous users&#x20;

<figure><img src="../../../.gitbook/assets/image (668).png" alt=""><figcaption></figcaption></figure>

1. Create a user with no login shell and a home directory&#x20;

<figure><img src="../../../.gitbook/assets/image (662).png" alt=""><figcaption></figcaption></figure>

3. Install PAM user database support&#x20;

{% code overflow="wrap" %}
```bash
sudo apt install db5.3-util
```
{% endcode %}

4. Create virtual ftp users.&#x20;

<figure><img src="../../../.gitbook/assets/image (663).png" alt=""><figcaption></figcaption></figure>

5. Convert to PAM database.&#x20;

{% code overflow="wrap" %}
```bash
sudo db_load -T -t hash \
-f /etc/vsftpd/virtual_users.txt \
/etc/vsftpd/virtual_users.db
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (665).png" alt=""><figcaption></figcaption></figure>

6. Change permissions of the PAM database and create PAM configuration file at `/etc/pam.d/vsftpd_virtual` and add the following content

{% code overflow="wrap" %}
```
auth required pam_userdb.so db=/etc/vsftpd/virtual_users
account required pam_userdb.so db=/etc/vsftpd/virtual_users
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (666).png" alt=""><figcaption></figcaption></figure>

7. Enable guest access at the end of the main configuration file.&#x20;

<figure><img src="../../../.gitbook/assets/image (670).png" alt=""><figcaption></figcaption></figure>

8. Restart the `vsftpd` server.&#x20;

{% code overflow="wrap" %}
```bash
sudo systemctl restart vsftpd
```
{% endcode %}

9. Login with that userid and password.&#x20;

<figure><img src="../../../.gitbook/assets/image (669).png" alt=""><figcaption></figcaption></figure>
