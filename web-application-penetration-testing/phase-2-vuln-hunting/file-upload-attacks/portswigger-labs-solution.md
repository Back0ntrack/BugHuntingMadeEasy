# Portswigger Labs Solution

## Remote code execution via web shell upload (No validation)

In description it is given that there is no validation at all.&#x20;

### Target

<figure><img src="../../../.gitbook/assets/image (14) (1) (1).png" alt=""><figcaption></figcaption></figure>

Thus we can see that we've the ability to upload profile picture.

### Functionality

Let's check on which tech stack the website is running.&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Thus we can assume that PHP is being used in background as the web server is Apache (XAMPP stack or LAMP Stack). Thus we need to upload a PHP webshell.&#x20;

### Exploitation

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now we need to find the location of the avatar where it is being uploaded. We can find it by right clicking and opening it in new tab.&#x20;

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Thus we got our shell.&#x20;

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can also upload file accordingly to perform read and write operations on the server.&#x20;

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Flawed File Type Validation -> Web shell upload via Content-Type restriction bypass

Sometimes the server assumes&#x20;

```
if (Content-Type == image/jpeg)
allow upload
```

But the server forgets that an attacker can intercept and modify the MIME type according to his requirement and if the server blindly trusts MIME type received then it can lead to a successful file upload attack.&#x20;

### Target

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

here again we're provided with the option to upload avatar. Thus we've identified our entry point.&#x20;

### Functionality

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

This time again we've the same Apache server but we're not allowed to upload files other than impage/jpeg or image/png.&#x20;

### Exploitation

We can see that a jpg file is uploaded successfully.&#x20;

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Let's try to intercept this request in burp and try to exploit it .&#x20;

{% tabs %}
{% tab title="Original Request" %}
<figure><img src="../../../.gitbook/assets/image (13) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Modified Request" %}
<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can see that Content-Type: \*/\* is not allowed only `image/jpeg` or `image/png` is allowed.&#x20;
{% endtab %}

{% tab title="Exploitation" %}
What we've done now is don't modify anything else. Just remove the data part of the image and add your malicious php code, change Content-Type to required one and we're good to go.&#x20;

<figure><img src="../../../.gitbook/assets/image (14) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

<figure><img src="../../../.gitbook/assets/image (15) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Web Shell upload via path traversal

Sometimes as a second line of defense what the developer generally do is they protect the whole directory where the file is being uploaded from execution even though if it is successfully uploaded in the required file type.&#x20;

### Target

<figure><img src="../../../.gitbook/assets/image (16) (1) (1).png" alt=""><figcaption></figcaption></figure>

Again we're provided with the ability to upload our profile picture. Let's try to upload shell.php file using the same way we uploaded previously.&#x20;

### Functionality

<figure><img src="../../../.gitbook/assets/image (17) (1) (1).png" alt=""><figcaption></figcaption></figure>

A simple jpg file is uploaded successfully. now let's try to upload shell.php.&#x20;

<figure><img src="../../../.gitbook/assets/image (18) (1) (1).png" alt=""><figcaption></figcaption></figure>

Our shell.php uploaded successfully. Let's try to execute some commands.

<figure><img src="../../../.gitbook/assets/image (20) (1).png" alt=""><figcaption></figcaption></figure>

We can see that even though it is uploaded as shell.php we're not getting php file executed. It might be the case that the developer has taken some restrictions on the avatars folder to stop file execution.&#x20;

### Exploitation

When we uploaded `../exploit.php` then it automatically get uploaded to `avatars/exploit.php`. Thus we used `..%2fexploit.php` which get uploaded as `avatars/../exploit.php`.&#x20;

{% tabs %}
{% tab title="../exploit.php" %}
<figure><img src="../../../.gitbook/assets/image (22) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="..%2fexploit.php" %}
<figure><img src="../../../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Shell" %}
<figure><img src="../../../.gitbook/assets/image (24) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

Thus what we've done is we successfully uploaded a php file in a directory before the general upload directory and thus it successfully executed our php file.&#x20;

## Web Shell upload via extension blacklist bypass

### Target

<figure><img src="../../../.gitbook/assets/image (25) (1).png" alt=""><figcaption></figcaption></figure>

Again we're provided with the ability to upload our profile picture.&#x20;

### Functionality

<figure><img src="../../../.gitbook/assets/image (26) (1).png" alt=""><figcaption></figcaption></figure>

It is allowing `jpeg` and `png` to upload.&#x20;

Let's check the request in the repeater. We can see that there is some blacklisting which is not allowing us to upload a `.php` file.

<figure><img src="../../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>

### Exploitation

## Web Shell upload via obfuscated file extension

### Target

<figure><img src="../../../.gitbook/assets/image (28) (1).png" alt=""><figcaption></figcaption></figure>

Again we're provided with the ability to upload our avatar.&#x20;

### Functionality

Thus we simply took this list [PHP\_Extension\_Payload\_all\_the\_things.lst](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst) and fuzzed the file extensions and found one working.&#x20;

<figure><img src="../../../.gitbook/assets/image (32) (1).png" alt=""><figcaption></figcaption></figure>

### Exploitation

<figure><img src="../../../.gitbook/assets/image (31) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (29) (1).png" alt=""><figcaption></figcaption></figure>

## Remote code execution via polyglot web shell upload

### Target

<figure><img src="../../../.gitbook/assets/image (33) (1).png" alt=""><figcaption></figcaption></figure>

Thus we're provided with the upload image functionality.&#x20;

### Functionality

<figure><img src="../../../.gitbook/assets/image (34) (1).png" alt=""><figcaption></figcaption></figure>

It is accepting jpg image correctly.&#x20;

<figure><img src="../../../.gitbook/assets/image (35) (1).png" alt=""><figcaption></figcaption></figure>

This time it is not accepting even a jpg extension when the only modifcation i've done is removed the content of the image. Thus it is drastic. We need to modify the magic bytes of the file. JPEG files always begin with the bytes `FF D8 FF`. This is a much more robust way of validating the file type.&#x20;

### Exploitation

Thus what we've done is we modified the hex of the image and added the malicious php code which successfully got uploaded.&#x20;

<figure><img src="../../../.gitbook/assets/image (40) (1).png" alt=""><figcaption></figcaption></figure>

But only problem arise here is the output will also come in the dispersed manner.&#x20;

<figure><img src="../../../.gitbook/assets/image (41) (1).png" alt=""><figcaption></figcaption></figure>

