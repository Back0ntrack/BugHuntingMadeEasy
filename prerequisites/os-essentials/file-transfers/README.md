# File Transfers

## Windows File transfer Methods&#x20;

## Linux File Transfer Methods

### Using Web&#x20;

####

{% code overflow="wrap" %}
```bash
python3 -m http.server <port if you want to specify> 
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (610).png" alt=""><figcaption></figcaption></figure>



## Hybrid Methods&#x20;

### Using Base64&#x20;

{% hint style="warning" %}
_Note that this method of file transfer using Base64 is sufficient for small files only, up to a size of a few megabytes._&#x20;
{% endhint %}

#### Sender Side

1. **Windows**

{% code overflow="wrap" %}
```powershell
[Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes((Get-Content .\secret.txt -Raw)))
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (606).png" alt=""><figcaption></figcaption></figure>

2. **Linux**

{% code overflow="wrap" %}
```bash
cat secret.txt | base64
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (607).png" alt=""><figcaption></figcaption></figure>

#### Receiver Side&#x20;

1. **Windows**

{% code overflow="wrap" %}
```powershell
[Text.Encoding]::UTF8.GetString([Convert]::FromBase64String(<encoded string>))
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (608).png" alt=""><figcaption></figcaption></figure>

2. **Linux**

{% code overflow="wrap" %}
```bash
echo "<encoded string>" | base64 -d
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (609).png" alt=""><figcaption></figcaption></figure>

