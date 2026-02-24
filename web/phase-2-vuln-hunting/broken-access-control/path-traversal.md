# Path Traversal

## Path Traversal&#x20;

Webpages are stored on the web server in a location called the web root.&#x20;

**The Directory or Path Traversal vulnerability allows an attacker to break out of the intended directory structure (web root) and access files or resources outside of it, such as system files, configuration files, or other sensitive data.**

<figure><img src="../../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

### How to Identify and Exploit Path Traversal Vulnerabilities

* Look for parameters like `filename`, `file`, `path`, `dir`, `download`, `image`, etc.\
  ↪️ Example: `GET /image?filename=4.jpg`
* Guess the backend folder structure (commonly: `/var/www/html/images/` for images)
* Try replacing the parameter value with traversal payloads like:\
  `../../../../etc/passwd`\
  (Use multiple `../` to break out of web root) \[_**Note:** Even if you use 8 `../` you cannot break out of `/` so use as many `../` as you can to break out of web root_]

<table><thead><tr><th width="89.79998779296875">Character</th><th width="89.199951171875">Encoded</th><th width="133.60003662109375">Double Encoding</th></tr></thead><tbody><tr><td><code>.</code></td><td><code>%2e</code></td><td><code>%252e</code></td></tr><tr><td><code>/</code></td><td><code>%2f</code></td><td><code>%252f</code></td></tr><tr><td><code>\</code></td><td><code>%5c</code></td><td><code>%255c</code></td></tr></tbody></table>

* Observe error messages or file content changes to confirm traversal
* If filtering is in place, try:
  * Double encoding (`..%2f..%2fetc/passwd`)
  * Null byte tricks (`../../etc/passwd%00.jpg`)
  * Directory shortcuts (`....//`, `..\/..\/`, `..;/`)
  * Direct Bypasss (`/etc/passwd`)
* **If an application enforces a file extension like `.png`, a null byte (`%00`) can be used to bypass it**, allowing access to files like `filename=../../../etc/passwd%00.png`.
* Always start with known file paths to test (like `/etc/passwd` on Linux or `C:\Windows\win.ini` on Windows)

{% hint style="info" %}
**Path Traversal vulnerabilities are often found where the website tries to access system resources like images, files, or text files.** For example, a request like `/product?productId=1` likely queries a database, but something like `/image?filename=4.jpg` may access a file on the server — so it’s important to carefully analyze where the request is being directed.
{% endhint %}

### Automation using Burp

The Intruder of BurpSuite Professional has predefined list of payloads for path traversal which can be selected from `Add from list...` in the payloads section. We've to just select the path and attack.&#x20;

### Some useful payloads

* h[ttps://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/Intruder/deep\_traversal.txt](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/Intruder/deep_traversal.txt)
* [https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Directory-Traversal-Payloads.txt](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Directory-Traversal-Payloads.txt)
