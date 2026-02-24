# Command Injection Attacks

## Prerequisite&#x20;

### Command Executing Functions

<figure><img src="../../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### Command Injection Operators&#x20;

<figure><img src="../../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Note:** _Command injection is possible when untrusted user input is passed—directly or_\
_indirectly—to any command-executing function of the respective backend language_\
_without proper validation, sanitization, or safe parameterization._
{% endhint %}

### PHP Example

<figure><img src="../../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

### Node JS Example

<figure><img src="../../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

## What is Command Injection ?&#x20;

Command injection is a security flaw where an application executes unintended system commands because it passes unsanitized user input to OS-level command execution functions, allowing an attacker to run arbitrary commands on the server.
