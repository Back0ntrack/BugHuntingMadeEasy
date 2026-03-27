# SSH (22)

## Introduction&#x20;

<figure><img src="../../.gitbook/assets/image (405).png" alt=""><figcaption></figcaption></figure>

SSH or Secure Shell is a protocol that is used to connect to remote networks securely and execute commands.&#x20;

## **Start the SSH Service**

<figure><img src="../../.gitbook/assets/image (406).png" alt=""><figcaption></figcaption></figure>

### Configuration files&#x20;

<figure><img src="../../.gitbook/assets/image (407).png" alt=""><figcaption></figcaption></figure>

## Authentication Mechanisms in SSH&#x20;

All the authentication mechanisms for SSH is stored inside&#x20;

{% code overflow="wrap" %}
```
/etc/ssh/sshd_config
```
{% endcode %}

### Password Authentication&#x20;

<figure><img src="../../.gitbook/assets/image (409).png" alt=""><figcaption></figcaption></figure>

| State in config               | Actual behavior     |
| ----------------------------- | ------------------- |
| `PasswordAuthentication yes`  | Enabled             |
| `#PasswordAuthentication yes` | ✅ Enabled (default) |
| `PasswordAuthentication no`   | ❌ Disabled          |

Note that even if this field is commented `PasswordAuthentication` is enabled by default.&#x20;

<figure><img src="../../.gitbook/assets/image (408).png" alt=""><figcaption></figcaption></figure>

#### Getting Blocked

<figure><img src="../../.gitbook/assets/image (412).png" alt=""><figcaption></figcaption></figure>

### Public Key Authentication&#x20;

It is the most secure way to login to an SSH server as it is brute-force resistance and no need to remember password.

<figure><img src="../../.gitbook/assets/image (417).png" alt=""><figcaption></figcaption></figure>

| State in config file        | Actual behavior     |
| --------------------------- | ------------------- |
| `PubkeyAuthentication yes`  | Enabled             |
| `#PubkeyAuthentication yes` | ✅ Enabled (default) |
| `PubkeyAuthentication no`   | ❌ Disabled          |

**What public key auth really is:**

* **Private key (of client)** → proves identity
* **Public key (of client stored in server)** → verifies identity
* **authorized\_keys (server)** → access control list

<figure><img src="../../.gitbook/assets/image (410).png" alt=""><figcaption></figcaption></figure>

#### Generate Key Pair&#x20;

{% hint style="info" %}
Server → kali\
Client → r0b
{% endhint %}

{% code overflow="wrap" %}
```bash
#we need to generate keys on client as SSH only allow access to a list of allowed keys
ssh-keygen -t ed25519

#other options available are: rsa, ecdsa, dsa (deprecated)
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (413).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
**Danger**

_The password you entered is to protect the private key file. So even if private key gets leaked the attacker wouldn't be able to log in._
{% endhint %}

#### Store keys

* The SSH server on `kali` needs to know that which public keys are allowed to login.
* So we need to store the public key of `rob` into `kali`.

**Way - 1:**\
Thus we can either copy paste the keys manually if we don't have password:

<figure><img src="../../.gitbook/assets/image (414).png" alt=""><figcaption></figcaption></figure>

**Way - 2:**

{% code overflow="wrap" %}
```bash
#If you know the password then use this command from client (password authentication must not be denied)
ssh-copy-id username@kali_ip

#It will store public key of r0b directly into ~/.ssh/authorized_keys of kali linux
# That's why writable ~/.ssh/authorized_keys create an issue
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (415).png" alt=""><figcaption></figcaption></figure>

#### Login directly

<figure><img src="../../.gitbook/assets/image (416).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Recommended .ssh permission**

_chmod 700 \~/.ssh_\
_chmod 600 \~/.ssh/id\_ed25519_\
_chmod 600 \~/.ssh/id\_ed25519.pub # optional, can be 644_\
_chmod 600 \~/.ssh/authorized\_keys_
{% endhint %}
