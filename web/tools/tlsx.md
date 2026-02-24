# tlsx

TLSx is a quick and configurable tool that acts as a Swiss army knife for finding TLS misconfigurations and performing reconnaissance.

### Installation

* `go install github.com/projectdiscovery/tlsx/cmd/tlsx@latest`
* `sudo cp ~/go/bin/tlsx /bin`
* `tlsx -version` or `~/go/bin/tlsx -version`

### Input options

* `-u`, `-host`: Target host to scan.
* `-l`, `-list`: target list to scan.&#x20;
* `-p`, `-port`: target port to connect (default 443).&#x20;

### SAN and CNs Detection (Subject alternative name/Common Name)

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

### Enumerate TLS Versions

> The creator forgot the `.` between the version. In below image version is tls 1.2

<figure><img src="../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

**Note:** These results are shown for **all** IPs associated with the apex domain.\
The `tlsx` tool queries **all resolved IPs** for a domain, whereas `nmap` (with `--script ssl-enum-ciphers`) typically tests **only one IP address** from the resolution.

### Check Misconfigurations

* `-ex`, `-expired`: display host with host expired certificates
* `-ss`, `-self-signed`: display host with self-signed certificate
* `-mm`, `-mismatched`: display host with mismatched certificate
