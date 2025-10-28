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

Useful Encoding Mechanisms: `'URL Encoding'`,&#x20;

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
