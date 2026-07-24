# Hashcat (HC)

## Identifying hashes&#x20;

### Using hashid&#x20;

{% code overflow="wrap" %}
```bash
hashid <hash> -m -j
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1711).png" alt=""><figcaption></figcaption></figure>

### Using Name-That-Hash

{% code overflow="wrap" %}
```bash
name-that-hash -t <hash> --no-banner
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1709).png" alt=""><figcaption></figcaption></figure>

### Using haiti&#x20;

{% code overflow="wrap" %}
```bash
haiti <hash>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1710).png" alt=""><figcaption></figcaption></figure>

## Creating Custom Wordlists&#x20;

### Using Crunch&#x20;

Crunch can create wordlists based on parameters such as words of a specific length, a limited character set, or a certain pattern. It can generate both permutations and combinations.

**Syntax:** `crunch <minimum length> <maximum length> <charset> -t <pattern> -o <output file>`

#### Creating wordlist with default charset

{% code overflow="wrap" %}
```bash
crunch 2 3 -o default_crunch_wordlist
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1720).png" alt=""><figcaption></figcaption></figure>

#### Create wordlist using pattern&#x20;

* `%` ⇒ Replace with a digit (0-9)
* `@` ⇒ Replace with a lowercase letter (a-z).&#x20;
* `,` ⇒ Replace with a uppercase letter (A-Z).&#x20;
* `^` ⇒ Replace with a Special character.&#x20;

{% code overflow="wrap" %}
```bash
crunch 17 17 -t ILFREIGHT201%@@@@ -o wordlist
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1721).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>No repetition</strong></summary>

```bash
crunch 12 12 -t 10031998@@@@ -d 1 -o wordlist
```

#### Examples

With `-d 1`, passwords containing **consecutive identical letters** are skipped.

```
10031998abcd
10031998abca
10031998azby
10031998wxyz
```

Without `-d 1`, Crunch would generate:

```
10031998aaaa
10031998aaab
10031998aabb
10031998abcc
...
```

</details>

### Using CUPP&#x20;

`CUPP` stands for `Common User Password Profiler`, and is used to create highly targeted and customized wordlists based on information gained from social engineering and OSINT.&#x20;

{% code overflow="wrap" %}
```bash
cupp -i
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1722).png" alt=""><figcaption></figcaption></figure>

### KWProcessor (Keyword Processor)

**KWProcessor (Keyword Processor)** is a password wordlist generation tool that creates **highly customized password lists** from keywords.

{% code overflow="wrap" %}
```bash
kwp -s 1 basechars/full.base keymaps/en-us.keymap routes/2-to-10-max-3-direction-changes.route
```
{% endcode %}

<table><thead><tr><th width="338.00006103515625">Part</th><th>Meaning</th></tr></thead><tbody><tr><td><code>kwp</code></td><td>Starts KWProcessor.</td></tr><tr><td><code>-s 1</code></td><td>Start generating words with a minimum length of <strong>1</strong> character.</td></tr><tr><td><code>basechars/full.base</code></td><td>Defines the set of starting characters that can be used.</td></tr><tr><td><code>keymaps/en-us.keymap</code></td><td>Specifies the keyboard layout (US QWERTY).</td></tr><tr><td><code>routes/2-to-10-max-3-direction-changes.route</code></td><td>Defines how the keyboard is traversed to generate passwords.</td></tr></tbody></table>

{% hint style="info" %}
_It is useful for cracking those user's password who generally press random keys on keyboard to set password._&#x20;
{% endhint %}

### Using Princeprocessor&#x20;

`PRINCE` or `PRobability INfinite Chained Elements` is an efficient password guessing algorithm to improve password cracking rates.&#x20;

#### Find the number of combinations&#x20;

{% code overflow="wrap" %}
```bash
princeprocessor --keyspace < words 
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1723).png" alt=""><figcaption></figcaption></figure>

#### Creating Wordlist&#x20;

{% code overflow="wrap" %}
```bash
princeprocessor -o wordlist.txt < words
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1724).png" alt=""><figcaption></figcaption></figure>

#### Princeprocessor - Password length limits&#x20;

{% code overflow="wrap" %}
```bash
princeprocessor --pw-min=10 --pw-max=25 -o wordlist.txt < words
```
{% endcode %}

### Using CeWL

It spiders and scrapes a website and creates a list of the words that are present.

**Syntax:** `cewl -d <depth to spider> -m <minimum word length> -w <output wordlist> <url of website>`

## Previously Cracked Hashes&#x20;

{% code overflow="wrap" %}
```bash
/home/kali/.local/share/hashcat/hashcat.potfile
/root/.local/share/hashcat/hashcat.potfile
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1725).png" alt=""><figcaption></figcaption></figure>





## Hands-On

{% hint style="danger" %}
**Warning**

_Avoid using the `--force` option with Hashcat unless absolutely necessary. It disables important safety checks and suppresses warnings, which can lead to unreliable results such as false positives, false negatives, or unexpected behavior. If Hashcat only works with `--force`, troubleshoot the underlying issue (such as GPU driver or OpenCL/CUDA configuration) instead of bypassing it. Use `--force` only if you fully understand the associated risks._
{% endhint %}

### Attack Modes&#x20;

<table data-search="false"><thead><tr><th width="55.60003662109375">#</th><th width="221.199951171875">Mode</th><th>Description</th></tr></thead><tbody><tr><td>0</td><td>Straight (default)</td><td>Uses a wordlist directly. </td></tr><tr><td>1</td><td>Combination</td><td>Combines two wordlists. </td></tr><tr><td>3</td><td>Brute-force (Mask)</td><td>Generate passwords from a mask/pattern.</td></tr><tr><td>6</td><td>Hybrid Wordlist + Mask</td><td>Appends a mask to each wordlist entry.</td></tr><tr><td>7</td><td>Hybrid Mask + Wordlist</td><td>Prepends a mask to each wordlist entry.</td></tr></tbody></table>

### Dictionary Attack&#x20;

**Syntax:** `hashcat -m <hash type> <hash file> <wordlist>`

{% code overflow="wrap" %}
```bash
hashcat -m 1000 sam.hashes /usr/share/wordlists/rockyou.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1712).png" alt=""><figcaption></figcaption></figure>

### Combination Attack&#x20;

<details>

<summary><strong>What if Combination attack doesn't exists ?</strong> </summary>

We need to create wordlist like this&#x20;

1. **using `awk` tool.**&#x20;

{% code overflow="wrap" %}
```bash
awk '(NR==FNR) { a[NR]=$0 } (NR != FNR) { for (i in a) { print $0 a[i] } }' wordlist1 wordlist2
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1713).png" alt=""><figcaption></figcaption></figure>

2. **Using `hashcat`**&#x20;

{% code overflow="wrap" %}
```bash
hashcat -a 1 --stdout file1 file2
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1714).png" alt=""><figcaption></figcaption></figure>

</details>

**Syntax:** `hashcat -a 1 -m <hash type> <hash file> <wordlist1> <wordlist2>`

{% code overflow="wrap" %}
```bash
hashcat -a 1 -m 0 2034f6e32958647fdff75d265b455ebf wordlist1 wordlist2
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1715).png" alt=""><figcaption></figcaption></figure>

### Mask Attack (bruteforce)

Mask attacks are used to generate words matching a specific pattern. This type of attack is particularly useful when the password length or format is known.

**Syntax:** `hashcat -a 3 -m <hash type> <hash file> <placeholder>`

#### Important Placeholders&#x20;

<table data-search="false"><thead><tr><th width="204.39996337890625">Placeholder</th><th width="456.39996337890625">Meaning</th></tr></thead><tbody><tr><td><code>?l</code></td><td>lower-case ASCII letters (a-z)</td></tr><tr><td><code>?u</code></td><td>upper-case ASCII letters (A-Z)</td></tr><tr><td><code>?d</code></td><td>digits (0-9)</td></tr><tr><td><code>?h</code></td><td>0123456789abcdef</td></tr><tr><td><code>?H</code></td><td>0123456789ABCDEF</td></tr><tr><td><code>?s</code></td><td>special characters («space»!"#$%&#x26;'()*+,-./:;&#x3C;=>?@[]^_`{</td></tr><tr><td><code>?a</code></td><td><code>?l</code> + <code>?u</code> + <code>?d</code> + <code>?s</code></td></tr><tr><td><code>?b</code></td><td>0x00 - 0xff</td></tr></tbody></table>

#### Attacking it&#x20;

{% code overflow="wrap" %}
```bash
hashcat -a 3 -m 0 <hash> '<placeholders>`
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1716).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Another examples</strong> </summary>

{% code overflow="wrap" %}
```
hashcat -a 3 hashes.txt Admin@?d?d?d
hashcat -a 3 hashes.txt Summer2024?d?d
hashcat -a 3 -m 0 md5_mask_example_hash -1 01 'ILFREIGHT?l?l?l?l?l20?1?d'
```
{% endcode %}

</details>

#### Custom Charset&#x20;

You can define your own character sets with `-1`, `-2`, `-3`, and `-4`. if you know that password can be limited within specific charset.&#x20;

{% hint style="info" %}
_Since the current year is **2026**, we can define a custom character set using `-1 026`. Then, using `?1` in the mask, Hashcat will generate all possible combinations using the characters **0**, **2**, and **6**. This is useful when you know the password is likely to contain the current year but are unsure of the exact order._
{% endhint %}

{% code overflow="wrap" %}
```bash
hashcat -a 3 -m 0 <hash> -2 0123 ?l?l?l?l?s?2?2?2?2
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1717).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Another example</strong> </summary>

String: `HASHCATqrstu2020`

{% code overflow="wrap" %}
```bash
hashcat -a 3 -m 0 50a742905949102c961929823a2e8ca0 -1 02 'HASHCAT?l?l?l?l?l20?1?d'
```
{% endcode %}

</details>

### Hybrid Mode&#x20;

Hybrid mode is a variation of the combinator attack, wherein multiple modes can be used together for a fine-tuned wordlist creation.

#### Hybrid Wordlist + Mask

**Syntax:** `hashcat -a 6 -m <hash type> <hash file> <wordlist> <placeholder to append at end>`

{% code overflow="wrap" %}
```bash
hashcat -a 6 -m 0 hybrid_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt '?d?s'
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1718).png" alt=""><figcaption></figcaption></figure>

#### Mask + Hybrid Wordlist&#x20;

**Syntax:** `hashcat -a 7 -m <hash type> <hash file> <placeholder to append at end> <wordlist>`

{% code overflow="wrap" %}
```bash
hashcat -a 7 -m 0 hybrid_hash_prefix -1 01 '20?1?d' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1719).png" alt=""><figcaption></figcaption></figure>

## Working with Rules

The rule-based attack is the most advanced and complex password cracking mode. Rules help perform various operations on the input wordlist, such as prefixing, suffixing, toggling case, cutting, reversing, and much more.

<table data-search="false"><thead><tr><th width="147">Function</th><th width="231.99993896484375">Description</th><th>Input</th><th>Output</th></tr></thead><tbody><tr><td>l</td><td>Convert all letters to lowercase</td><td>InlaneFreight2020</td><td>inlanefreight2020</td></tr><tr><td>u</td><td>Convert all letters to uppercase</td><td>InlaneFreight2020</td><td>INLANEFREIGHT2020</td></tr><tr><td>c / C</td><td>Capitalize first character, lowercase the rest / Lowercase first character, uppercase the rest</td><td>inlaneFreight2020 / Inlanefreight2020</td><td>Inlanefreight2020 / iNLANEFREIGHT2020</td></tr><tr><td>t / TN</td><td>Toggle case : whole word / at position N</td><td>InlaneFreight2020</td><td>iNLANEfREIGHT2020</td></tr><tr><td>d / q / zN / ZN</td><td>Duplicate word / all characters / first character / last character</td><td>InlaneFreight2020</td><td>InlaneFreight2020InlaneFreight2020 / IInnllaanneeFFrreeiigghhtt22002200 / IInlaneFreight2020 / InlaneFreight20200</td></tr><tr><td>{ / }</td><td>Rotate word left / right</td><td>InlaneFreight2020</td><td>nlaneFreight2020I / 0InlaneFreight202</td></tr><tr><td>^X / $X</td><td>Prepend / Append character X</td><td>InlaneFreight2020 (^! / $! )</td><td>!InlaneFreight2020 / InlaneFreight2020!</td></tr><tr><td>r</td><td>Reverse</td><td>InlaneFreight2020</td><td>0202thgierFenalnI</td></tr></tbody></table>

### Hashcat - Default Rules&#x20;

{% code overflow="wrap" %}
```bash
ls -l /usr/share/hashcat/rules
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1729).png" alt=""><figcaption></figcaption></figure>

### Example Rules&#x20;

{% code overflow="wrap" %}
```bash
c so0 si1 se3 ss5 sa@ $2 $0 $1 $9
```
{% endcode %}

The first letter word is capitalized with the `c` function. Then rule uses the substitute function `s` to replace `o` with `0`, `i` with `1`, `e` with `3` and a with `@`. At the end, the year `2019` is appended to it. Copy the rule to a file so that we can debug it.

### Create Rule&#x20;

{% code overflow="wrap" %}
```bash
echo 'c so0 si1 se3 ss5 sa@ $2 $0 $1 $9' > rule.txt
```
{% endcode %}

{% code overflow="wrap" %}
```bash
echo 'password_ilfreight' > test.txt
```
{% endcode %}

{% code overflow="wrap" %}
```bash
hashcat -r rule.txt test.txt --stdout
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1726).png" alt=""><figcaption></figcaption></figure>

### Cracking Password&#x20;

{% code overflow="wrap" %}
```bash
hashcat -a 0 -m 100 08004e35561328e357e34d07c53c7e4f41944e28  /usr/share/wordlists/rockyou.txt -r rule.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1727).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1728).png" alt=""><figcaption></figcaption></figure>

