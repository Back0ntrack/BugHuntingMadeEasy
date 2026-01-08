# Labs

## Most simple LFI

### Target

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Functionality

Whenever we select a language, we can see that the php file page changes in the URL.

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Now on analyzing the provided source code we can understand that it is retrieving and executing files directly provided to the _language_ parameter.&#x20;

```php
include($_GET['language']);
```

### Exploitation

Thus, to exploit this type of vulnerability we can simply add /etc/passwd because there is no any prefix or suffix in the include function and thus it will directly render the required file as seen in the below image.

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

## File Inclusion (using Path Traversal)

### Target&#x20;

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Functionality

We have the same functionality as in the previous lab, where we were able to fetch specific language PHP files accordingly. However, in this case, the source code has been modified and forcibly restricted to include files only from the ./languages directory.

```php
include("./lanugages/".$_GET(['language']);
```

### Exploitation

&#x20;Thus, adding any file will be opened only from \`_./languages_\`.  E.g., (_/etc/passwd => ./languages/etc/passwd_) which is invalid as in the below image.

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

We can easily bypass this restriction by traversing directories using relative paths. To do so, we can add ../ before our file name, which refers to the parent directory.

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## File Inclusion (Filename prefix bypass)

We’ve the same type of target and functionality as in the previous labs the only difference is in the source code behind the logic of the inclusion. This time the source code is changed to something like this.

```php
include("lang_".$_GET['language']);
```

Thus, if we add anything to the language parameter it will get a prefix of \`_lang\__\` automatically.

### Exploitation

Thus, even using directory traversal method we’re unable to fetch our intended file.

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Thus, what we can do is we can prefix a `/` before our payload, and this should consider the prefix as a directory, and then should bypass the filename and be able to traverse directories.

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## File Inclusion (Denied Path traversal using Filters)

### Target&#x20;

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Functionality

Now there are filters to prevent directory traversal by amending the source code.

```php
$language=str_replace('../', "", $_GET['language']);
include($_GET['language']);
```

### Exploitation

Yes, it is removing \`../\` but it is not recursively removing the \`../\` substring and thus we can modify the input string to get our intended result by using \`…//\`

Thus it will remove \`../\` in the substring which instead concatenate remaining string and again it becomes \`../\`.

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

**Method - 2 Encoding**

We can URL encode the \`../\` character and send it in the browser and we can see the same result

```
%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64 -> ../../../../etc/passwd
```

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

## File Inclusion (Regex Filters)

We’ve the same kind of target and functionality as provided in the above labs.

Some web applications may also use Regular Expressions to ensure that the file being included is under a specific path. For example, the web application we have been dealing with may only accept paths that are under the ./languages directory, as follows:

```php
if (preg_match('/^\.\/languages\/.+$/',$_GET['language'])) {
    include($_GET['langauge]);
}
else {
    echo "Illegal path specified!";
}
```

### Exploitation

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

## Simplest RFI&#x20;

### Target&#x20;

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

The backend PHP code is:

<p align="center"><code>include($_GET[‘language’]);</code></p>

One more thing modified in the php.ini file is this line `allow_url_include=On`. This line allows us to include remote URL in the function.

### Exploitation

We’ve added `<?php phpinfo();?>` as the code in info.php.

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

Our remote php code is successfully executed. Now let's try to enter a shell.&#x20;

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>
