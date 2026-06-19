# SSH (22)

## Introduction&#x20;

<figure><img src="../../../.gitbook/assets/image (405).png" alt=""><figcaption></figcaption></figure>

**SSH (Secure Shell)** is an **Application Layer protocol** that provides secure remote access, command execution, and file transfer capabilities over an encrypted network connection.

## SSH Setup&#x20;

### Install SSH&#x20;

{% code overflow="wrap" %}
```bash
sudo apt update
sudo apt install openssh-server -y
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (705).png" alt=""><figcaption></figcaption></figure>

### **Start the SSH Service**

{% code overflow="wrap" %}
```bash
sudo systemctl start ssh
sudo systemctl status ssh
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (706).png" alt=""><figcaption></figcaption></figure>

### Connecting to SSH&#x20;

{% code overflow="wrap" %}
```
ssh <user>@<IP> 
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (707).png" alt=""><figcaption></figcaption></figure>

## Default Files&#x20;

**All files related to the SSH service are stored in `/etc/ssh`.**

<figure><img src="../../../.gitbook/assets/image (708).png" alt=""><figcaption></figcaption></figure>

### sshd\_config file

This is the **default Ubuntu OpenSSH server configuration file**.

<figure><img src="../../../.gitbook/assets/image (710).png" alt=""><figcaption></figcaption></figure>

To view the final configuration used by the SSH daemon:&#x20;

{% code overflow="wrap" %}
```
sudo sshd -T
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (709).png" alt=""><figcaption></figcaption></figure>

### SSH Host Keys&#x20;

The first time you connect, you see:

```
The authenticity of host '192.168.16.142' can't be established.
ED25519 key fingerprint is ...
Are you sure you want to continue connecting (yes/no)?
```

That fingerprint is derived from the server's host key.

#### Working of SSH Host Keys

* SSH host keys identify the SSH server and are used to prevent man-in-the-middle attacks.&#x20;
* During the initial connection, the client's SSH software stores the server's public host key in `~/.ssh/known_hosts`.&#x20;
* On subsequent connections, the stored key is compared against the server's presented key to verify the server's identity.&#x20;
* Host keys are stored in `/etc/ssh/` and are distinct from user authentication keys.

<figure><img src="../../../.gitbook/assets/image (724).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_The most modern key is preferably used._&#x20;
{% endhint %}

### Recommended File Permissions&#x20;

| Item                                                  | Permission       |
| ----------------------------------------------------- | ---------------- |
| `~/.ssh/` directory                                   | `700`            |
| All private keys (`id_rsa`, `id_ed25519`, `id_ecdsa`) | `600`            |
| All public keys (`*.pub`)                             | `644`            |
| `authorized_keys`                                     | `600`            |
| `known_hosts`                                         | `644` (or `600`) |
| `~/.ssh/config`                                       | `600`            |

## Authentication Mechanisms in SSH&#x20;

All the authentication mechanisms for SSH is stored inside&#x20;

{% code overflow="wrap" %}
```
/etc/ssh/sshd_config
```
{% endcode %}

### Default Authentication Methods&#x20;

{% code overflow="wrap" %}
```
sshd -T | grep -E 'authentication|permitrootlogin'
```
{% endcode %}

{% hint style="info" %}
_Even though the authentication directives are commented out in `sshd_config`, `sshd -T` shows that OpenSSH enables both public key and password authentication by default. Root login is allowed, but password-based root login is disabled; the root user can authenticate only using public keys (`PermitRootLogin prohibit-password`)._
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (711).png" alt=""><figcaption></figcaption></figure>

### Password Authentication&#x20;

<figure><img src="../../../.gitbook/assets/image (712).png" alt=""><figcaption></figcaption></figure>

| State in config               | Actual behavior     |
| ----------------------------- | ------------------- |
| `PasswordAuthentication yes`  | Enabled             |
| `#PasswordAuthentication yes` | ✅ Enabled (default) |
| `PasswordAuthentication no`   | ❌ Disabled          |

Note that even if this field is commented `PasswordAuthentication` is enabled by default.&#x20;

<figure><img src="../../../.gitbook/assets/image (714).png" alt=""><figcaption></figcaption></figure>

#### Getting Blocked

<figure><img src="../../../.gitbook/assets/image (715).png" alt=""><figcaption></figcaption></figure>

### Public Key Authentication&#x20;

It is the most secure way to login to an SSH server as it is brute-force resistance and no need to remember password.

<figure><img src="../../../.gitbook/assets/image (716).png" alt=""><figcaption></figcaption></figure>

| State in config file        | Actual behavior     |
| --------------------------- | ------------------- |
| `PubkeyAuthentication yes`  | Enabled             |
| `#PubkeyAuthentication yes` | ✅ Enabled (default) |
| `PubkeyAuthentication no`   | ❌ Disabled          |

**What public key auth really is:**

* **Private key (of client)** → proves identity
* **Public key (of client stored in server)** → verifies identity
* **authorized\_keys (server)** → access control list

<figure><img src="../../../.gitbook/assets/image (410).png" alt=""><figcaption></figcaption></figure>

### Host-based authentication

1. Enable host based authentication on the SSH server (ubuntu).&#x20;

<figure><img src="../../../.gitbook/assets/image (725).png" alt=""><figcaption></figcaption></figure>

2. Enable host based authentication on the SSH client (kali).&#x20;

<figure><img src="../../../.gitbook/assets/image (726).png" alt=""><figcaption></figcaption></figure>

3. Verify client host keys on the SSH client (kali)

<figure><img src="../../../.gitbook/assets/image (727).png" alt=""><figcaption></figcaption></figure>

4. Add hostname of client and server in each other's host files.&#x20;

<figure><img src="../../../.gitbook/assets/image (728).png" alt=""><figcaption></figcaption></figure>

5. Copy the value of public key of SSH client to SSH server.&#x20;

<figure><img src="../../../.gitbook/assets/image (729).png" alt=""><figcaption></figcaption></figure>

6. Configure trusted users on the SSH server. Since we've copied the public key of `root@kali` we've to add root user of `kali` host.&#x20;

<figure><img src="../../../.gitbook/assets/image (730).png" alt=""><figcaption></figcaption></figure>

7. Login with `HostbasedAuthentication=yes` method.&#x20;

<figure><img src="../../../.gitbook/assets/image (731).png" alt=""><figcaption></figcaption></figure>

## Public Key Authentication Setup&#x20;

### Generate Key Pair&#x20;

{% hint style="info" %}
_Server → Ubuntu (r0b)_\
_Client → kali (nexploit)_
{% endhint %}

#### Client Side

{% code overflow="wrap" %}
```bash
#we need to generate keys on client as SSH only allow access to a list of allowed keys
ssh-keygen -t ed25519

#other options available are: rsa, ecdsa, dsa (deprecated)
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (718).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
**Danger**

_The password you entered is to protect the private key file. So even if private key gets leaked the attacker wouldn't be able to log in._
{% endhint %}

### Store keys

* The SSH server on `ubuntu` needs to know that which public keys are allowed to login.
* So we need to store the public key of `kali` in `ubuntu`.

**Way - 1:**\
Thus we can either copy paste the keys manually if we don't have password:

<figure><img src="../../../.gitbook/assets/image (719).png" alt=""><figcaption></figcaption></figure>

**Way - 2:**

{% code overflow="wrap" %}
```bash
#If you know the password then use this command from client (password authentication must not be denied)
ssh-copy-id username@kali_ip

#It will store public key of r0b directly into ~/.ssh/authorized_keys of kali linux
# That's why writable ~/.ssh/authorized_keys create an issue
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (721).png" alt=""><figcaption></figcaption></figure>

### Login

<figure><img src="../../../.gitbook/assets/image (722).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Recommended .ssh permission**

_chmod 700 \~/.ssh_\
_chmod 600 \~/.ssh/id\_ed25519_\
_chmod 600 \~/.ssh/id\_ed25519.pub # optional, can be 644_\
_chmod 600 \~/.ssh/authorized\_keys_
{% endhint %}

## Agent Forwarding&#x20;

<figure><img src="../../../.gitbook/assets/image (741).png" alt=""><figcaption></figcaption></figure>

### Start ssh-agent on Kali&#x20;

{% code overflow="wrap" %}
```bash
eval "$(ssh-agent -s)"
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (732).png" alt=""><figcaption></figcaption></figure>

### Load SSH key

{% code overflow="wrap" %}
```bash
ssh-add ~/.ssh/<your ssh private key>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (737).png" alt=""><figcaption></figcaption></figure>

#### Verify if the key is loaded successfully

{% code overflow="wrap" %}
```bash
ssh-add -l
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (738).png" alt=""><figcaption></figcaption></figure>

### Connect to Ubuntu with agent forwarding&#x20;

{% code overflow="wrap" %}
```bash
ssh -A r0b@192.168.16.142
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (735).png" alt=""><figcaption></figcaption></figure>

### Verify Forwarding on Ubuntu

{% code overflow="wrap" %}
```bash
echo $SSH_AUTH_SOCK
ssh-add -l
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (739).png" alt=""><figcaption></figcaption></figure>

### Login to OWASPBWA without password&#x20;

<figure><img src="../../../.gitbook/assets/image (740).png" alt=""><figcaption></figcaption></figure>

## SSH Port Forwarding&#x20;

* **Local Port Forwarding (-L)** → Access remote/internal service through SSH.
* **Remote Port Forwarding (-R)** → Expose your local service on the SSH server.

### Local Port Forwarding

SSH Local Port Forwarding creates a port on the attacker's machine and tunnels traffic through an SSH server to reach a remote service that may not be directly accessible.

<figure><img src="../../../.gitbook/assets/image (742).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
ssh -L <local host port>:<remote host>:<remote host port> r0b@<ip>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (743).png" alt=""><figcaption></figcaption></figure>

### Remote Port Forwarding&#x20;

<figure><img src="../../../.gitbook/assets/image (744).png" alt=""><figcaption></figcaption></figure>

#### Start listening on local port 9999&#x20;

{% code overflow="wrap" %}
```
nc -lvnp 9999
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (745).png" alt=""><figcaption></figcaption></figure>

#### Start and keep the session open with remote forwarding

{% code overflow="wrap" %}
```bash
ssh -R 7777:127.0.0.1:9999 r0b@192.168.16.142
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (746).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_This will forward any traffic sent to Ubuntu's port 7777 to kali's port 9999._&#x20;
{% endhint %}

#### Make sure ubuntu has settings on

<figure><img src="../../../.gitbook/assets/image (747).png" alt=""><figcaption></figcaption></figure>

#### Connect from OWASP BWA&#x20;

<figure><img src="../../../.gitbook/assets/image (748).png" alt=""><figcaption></figcaption></figure>

**We see that we've received the connection.**&#x20;

<figure><img src="../../../.gitbook/assets/image (749).png" alt=""><figcaption></figcaption></figure>

#### Send message to kali&#x20;

<figure><img src="../../../.gitbook/assets/image (750).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (751).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_See the remote port forwarding is generally used for reverse shell connection. We can start a reverse shell listener on kali and use remote port forwarding._
{% endhint %}

### Dynamic Forwarding and Proxychains&#x20;

SSH Dynamic Port Forwarding creates a local SOCKS proxy that tunnels arbitrary TCP connections through an SSH server, allowing access to any host and port reachable from that server.

<figure><img src="../../../.gitbook/assets/image (752).png" alt=""><figcaption></figcaption></figure>

#### Start Dynamic Port Forwarding&#x20;

{% code overflow="wrap" %}
```bash
ssh -D <port> r0b@192.168.16.142
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (753).png" alt=""><figcaption></figcaption></figure>

#### Configure Proxychains&#x20;

<figure><img src="../../../.gitbook/assets/image (754).png" alt=""><figcaption></figcaption></figure>

#### Connect to any services using proxychains&#x20;

<figure><img src="../../../.gitbook/assets/image (755).png" alt=""><figcaption></figcaption></figure>

#### Proof&#x20;

<figure><img src="../../../.gitbook/assets/image (756).png" alt=""><figcaption></figcaption></figure>
