# FTP (21)

**FTP (File Transfer Protocol)** is an application-layer protocol used to transfer files between a client and a server.

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

<details>

<summary><strong><code>/etc/vsftpd.conf</code></strong></summary>

{% code overflow="wrap" %}
```bash
# Example config file /etc/vsftpd.conf
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
#
# Run standalone?  vsftpd can run either from an inetd or as a standalone
# daemon started from an initscript.
listen=NO
#
# This directive enables listening on IPv6 sockets. By default, listening
# on the IPv6 "any" address (::) will accept connections from both IPv6
# and IPv4 clients. It is not necessary to listen on *both* IPv4 and IPv6
# sockets. If you want that (perhaps because you want to listen on specific
# addresses) then you must run two copies of vsftpd with two configuration
# files.
listen_ipv6=YES
#
# Allow anonymous FTP? (Disabled by default).
anonymous_enable=NO
#
# Uncomment this to allow local users to log in.
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
#write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
#local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# If enabled, vsftpd will display directory listings with the time
# in  your  local  time  zone.  The default is to display GMT. The
# times returned by the MDTM FTP command are also affected by this
# option.
use_localtime=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/vsftpd.log
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
#xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
#ascii_upload_enable=YES
#ascii_download_enable=YES
#
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd.banned_emails
#
# You may restrict local users to their home directories.  See the FAQ for
# the possible risks in this before using chroot_local_user or
# chroot_list_enable below.
#chroot_local_user=YES
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
#chroot_local_user=YES
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd.chroot_list
#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# Customization
#
# Some of vsftpd's settings don't fit the filesystem layout by
# default.
#
# This option should be the name of a directory which is empty.  Also, the
# directory should not be writable by the ftp user. This directory is used
# as a secure chroot() jail at times vsftpd does not require filesystem
# access.
secure_chroot_dir=/var/run/vsftpd/empty
#
# This string is the name of the PAM service vsftpd will use.
pam_service_name=vsftpd
#
# This option specifies the location of the RSA certificate to use for SSL
# encrypted connections.
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO

#
# Uncomment this to indicate that vsftpd use a utf8 filesystem.
#utf8_filesystem=YES
```
{% endcode %}

</details>

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

### Default vsftpd behavior

{% hint style="info" %}
_When you see:_

```
listen=NO
listen_ipv6=YES
```

_think that vsftpd is running in standalone mode on an IPv6 socket that usually accepts both IPv6 and IPv4 connections._
{% endhint %}

<table><thead><tr><th width="232.20001220703125">Setting</th><th width="158.5999755859375">Default Value</th><th>Default Behavior</th></tr></thead><tbody><tr><td><code>listen</code></td><td><code>NO</code></td><td>Does not listen on IPv4 standalone socket</td></tr><tr><td><code>listen_ipv6</code></td><td><code>YES</code></td><td>Listens on IPv6 socket (also accepts IPv4 on most systems)</td></tr><tr><td><code>anonymous_enable</code></td><td><code>NO</code></td><td>Anonymous login disabled</td></tr><tr><td><code>local_enable</code></td><td><code>YES</code></td><td>Local Linux users can authenticate</td></tr><tr><td><code>write_enable</code></td><td><code>NO</code></td><td>Upload, delete, rename, mkdir disabled</td></tr><tr><td><code>local_umask</code></td><td><code>077</code></td><td>New files created as owner-only accessible</td></tr><tr><td><code>anon_upload_enable</code></td><td><code>NO</code></td><td>Anonymous uploads disabled</td></tr><tr><td><code>anon_mkdir_write_enable</code></td><td><code>NO</code></td><td>Anonymous directory creation disabled</td></tr><tr><td><code>dirmessage_enable</code></td><td><code>YES</code></td><td>Shows <code>.message</code> files when entering directories</td></tr><tr><td><code>use_localtime</code></td><td><code>YES</code></td><td>Directory listings show local system time</td></tr><tr><td><code>xferlog_enable</code></td><td><code>YES</code></td><td>File transfers logged</td></tr><tr><td><code>connect_from_port_20</code></td><td><code>YES</code></td><td>Active mode data connections originate from TCP port 20</td></tr><tr><td><code>xferlog_file</code></td><td><code>/var/log/xferlog</code></td><td>Default transfer log location</td></tr><tr><td><code>xferlog_std_format</code></td><td><code>NO</code></td><td>Uses vsftpd log format, not wu-ftpd format</td></tr><tr><td><code>idle_session_timeout</code></td><td><code>300</code> sec</td><td>Idle FTP session timeout</td></tr><tr><td><code>data_connection_timeout</code></td><td><code>300</code> sec</td><td>Data channel timeout</td></tr><tr><td><code>ascii_upload_enable</code></td><td><code>NO</code></td><td>ASCII upload conversion disabled</td></tr><tr><td><code>ascii_download_enable</code></td><td><code>NO</code></td><td>ASCII download conversion disabled</td></tr><tr><td><code>ftpd_banner</code></td><td>Not set</td><td>Standard vsftpd banner displayed</td></tr><tr><td><code>deny_email_enable</code></td><td><code>NO</code></td><td>Anonymous email blacklist disabled</td></tr><tr><td><code>chroot_local_user</code></td><td><code>NO</code></td><td>Local users not jailed to home directory</td></tr><tr><td><code>chroot_list_enable</code></td><td><code>NO</code></td><td>Chroot list not used</td></tr><tr><td><code>ls_recurse_enable</code></td><td><code>NO</code></td><td><code>LIST -R</code> recursive listing disabled</td></tr><tr><td><code>secure_chroot_dir</code></td><td><code>/var/run/vsftpd/empty</code></td><td>Empty directory used for secure chroot operations</td></tr><tr><td><code>pam_service_name</code></td><td><code>vsftpd</code></td><td>Uses <code>/etc/pam.d/vsftpd</code> for authentication</td></tr><tr><td><code>ssl_enable</code></td><td><code>NO</code></td><td>FTPS/TLS disabled</td></tr><tr><td><code>rsa_cert_file</code></td><td>Snakeoil cert</td><td>Default certificate path configured but unused</td></tr><tr><td><code>utf8_filesystem</code></td><td><code>NO</code></td><td>UTF-8 filename support not explicitly enabled</td></tr></tbody></table>

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

#### Default directory

By default `anonymous` login get access to `/srv/ftp` directory of the system.

<figure><img src="../../../.gitbook/assets/image (687).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (688).png" alt=""><figcaption></figcaption></figure>

#### Default user

Anonymous FTP runs as:

```
ftp:x:112:115:ftp daemon:/srv/ftp:/usr/sbin/nologin
```

So the anonymous user is effectively the Linux user `ftp`.

### Filesystem jail

**`chroot_local_user` restricts FTP users to their home directory by creating a chroot (filesystem jail) and restricting them from accessing original `/` directory or any other directory.**&#x20;

<figure><img src="../../../.gitbook/assets/image (672).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (673).png" alt=""><figcaption></figcaption></figure>

Inside a chroot jail, FTP `/` corresponds to the user's home directory (e.g., `/home/ftpshell`) on the Linux system.

{% hint style="warning" %}
_According to the vsftpd security design, a user confined to a chroot jail should not have write permissions on the jail's top-level directory (`/`, which maps to `/home/<user>` on the actual system). If the jail root is writable by the user, vsftpd refuses login with `500 OOPS: vsftpd: refusing to run with writable root inside chroot()` as a security precaution._
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (674).png" alt=""><figcaption></figcaption></figure>

> _Since chroot jailing is disabled by default, the `r0b` user can log in and is not restricted to their home directory. Therefore, they can navigate the filesystem and access the original `/` directory and other locations, subject to Linux file permissions._

This error can be bypassed by adding this line&#x20;

{% code overflow="wrap" %}
```bash
allow_writeable_chroot=YES
```
{% endcode %}

### Setup Guest users

1. Disable anonymous users&#x20;

<figure><img src="../../../.gitbook/assets/image (668).png" alt=""><figcaption></figcaption></figure>

2. Create a user and its home directory&#x20;

<figure><img src="../../../.gitbook/assets/image (671).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that since we've created directory using root permission the owner is root. This is done to avoid the `chroot_local_user` error.&#x20;
{% endhint %}

1. Install PAM user database support&#x20;

{% code overflow="wrap" %}
```bash
sudo apt install db5.3-util
```
{% endcode %}

4. Create virtual ftp users.&#x20;

{% hint style="info" %}
_**Note:** All users defined in the virtual users database are virtual accounts. After successful authentication, they are mapped to the Linux user specified by `guest_username` in main configuration file(e.g., `ftpuser`), and filesystem access is performed using that user's permissions._
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (675).png" alt=""><figcaption></figcaption></figure>

5. Convert to PAM database. (you can delete `.txt` file after database creation for security)

{% code overflow="wrap" %}
```bash
sudo db_load -T -t hash \
-f /etc/vsftpd/virtual_users.txt \
/etc/vsftpd/virtual_users.db
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (676).png" alt=""><figcaption></figcaption></figure>

6. Change permissions of the PAM database (so that local user can't modify it) and create PAM configuration file at `/etc/pam.d/virtual_users_pam_config` and add the following content

{% code overflow="wrap" %}
```
auth required pam_userdb.so db=/etc/vsftpd/virtual_users
account required pam_userdb.so db=/etc/vsftpd/virtual_users
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (677).png" alt=""><figcaption></figcaption></figure>

7. Enable guest access at the end of the main configuration file.&#x20;

<figure><img src="../../../.gitbook/assets/image (683).png" alt=""><figcaption></figcaption></figure>

8. Restart the `vsftpd` server.&#x20;

{% code overflow="wrap" %}
```bash
sudo systemctl restart vsftpd
```
{% endcode %}

9. Login with that userid and password.&#x20;

<figure><img src="../../../.gitbook/assets/image (680).png" alt=""><figcaption></figcaption></figure>

### Write Permissions

`write_enable` controls whether FTP users are allowed to perform **write operations** on the server.

By default:

```
write_enable=NO
```

which means FTP users can generally only **read/download/list files**.

When enabled:

```
write_enable=YES
```

FTP users can perform write-related commands **if Linux filesystem permissions also allow them**.

#### Operations Enabled by `write_enable`

| Operation          | FTP Command      | FTP Equivalent Command |
| ------------------ | ---------------- | ---------------------- |
| Upload files       | `STOR`           | **`put`**              |
| Delete files       | `DELE`           | **`delete`**           |
| Rename files       | `RNFR`, `RNTO`   | **`rename`**           |
| Create directories | `MKD`            | **`mkdir`**            |
| Remove directories | `RMD`            | **`rmdir`**            |
| Modify files       | Upload/overwrite |                        |

<figure><img src="../../../.gitbook/assets/image (681).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_Since login is allowed only when the chroot directory is not writable, this demonstrates that `harshil_rana` maps to `ftpuser`, but `ftpuser` is not allowed to write to its home directory because it is owned by `root`, which created the directory earlier. Therefore, we need to create a separate writable directory on Ubuntu where files can be uploaded._
{% endhint %}

#### Create writeable directory&#x20;

<figure><img src="../../../.gitbook/assets/image (682).png" alt=""><figcaption></figcaption></figure>

#### Write into directory&#x20;

<figure><img src="../../../.gitbook/assets/image (684).png" alt=""><figcaption></figcaption></figure>

#### Delete a file&#x20;

<figure><img src="../../../.gitbook/assets/image (685).png" alt=""><figcaption></figcaption></figure>

### The Unmask permission

`local_umask=022` prevents group and other users from modifying uploaded files and directories; only the owner can write to them. Uploaded files are created without execute permissions by default.

#### After uncommenting&#x20;

<figure><img src="../../../.gitbook/assets/image (686).png" alt=""><figcaption></figcaption></figure>

`local_umask=022` creates uploaded files with permissions 644 and directories with permissions 755 by default as we can see in the above screenshot our script is uploaded with `rw` permission for our user only. These permissions can later be modified using chmod.

### Anon write

The write permission for anonymous user is held by:&#x20;

{% code overflow="wrap" %}
```
anon_upload_enable=YES
anon_mkdir_write_enable=YES
```
{% endcode %}

#### Create writable upload directory&#x20;

Note that anonymous user always run with permission of `ftp` user. by default it doesn't have write permission on the `/srv/ftp/` folder.&#x20;

<figure><img src="../../../.gitbook/assets/image (689).png" alt=""><figcaption></figcaption></figure>

#### Writing to the directory&#x20;

<figure><img src="../../../.gitbook/assets/image (690).png" alt=""><figcaption></figcaption></figure>

### Anon root&#x20;

Turning anon root to `/` allows anonymous user to list folders within the system.&#x20;

{% code overflow="wrap" %}
```
anon_root=/
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (701).png" alt=""><figcaption></figcaption></figure>
