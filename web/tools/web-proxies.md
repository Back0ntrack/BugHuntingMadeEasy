---
icon: spider-web
cover: ../../.gitbook/assets/all_in_one_cover (3).PNG
coverY: 0
---

# Web Proxies

Web proxies are specialized tools that can be set up between a browser/mobile application and a back-end server to capture and view all the web requests being sent between both ends, essentially acting as man-in-the-middle (MITM) tools. While other `Network Sniffing` applications, like Wireshark, operate by analyzing all local traffic to see what is passing through a network, Web Proxies mainly work with web ports such as, but not limited to, `HTTP/80` and `HTTPS/443`.

## Proxy Setup

### Using Pre-Configured Browser

In Burp's (`Proxy>Intercept`), we can click on `Open Browser`, which will open Burp's pre-configured browser, and automatically route all web traffic through Burp:

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

In ZAP, we can click on the Firefox browser icon at the end of the top bar, and it will open the pre-configured browser:

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

### Proxy Setup

By default both Burp and ZAP use port `8080`by default, but we can use any available port.&#x20;

{% hint style="info" %}
Note: In case we wanted to serve the web proxy on a different port, we can do that in Burp under (`Proxy>Options`), or in ZAP under (`Tools>Options>Local Proxies`). In both cases, we must ensure that the proxy configured in Firefox uses the same port.
{% endhint %}

**Using Foxy Proxy Extension within browser**

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

By clicking on the Options we can configure the browser to use a web proxy over provided port. But we need to install CA certificate in the browser where we've added the foxy proxy extension.&#x20;

### Installing CA Certificate

By browsing to `http://burp`and download the certificate from there by clicking on `CA Certificate`.

<figure><img src="../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

To get ZAP's certificate, we can go to (`Tools > Options > Network > Server Certificate`), then click on `Save`:

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

Now we've to add these certificate within browser in order to use proxies without disturbance.&#x20;

## BurpSuite

### Setting Up

The first page of Burp Suite Professional provides options to work on a temporary project, create a new project, or open an existing project from the disk that was previously created using the 'New Project on Disk' option.

<figure><img src="../../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

The second page prompts us to choose a configuration file, or we can load the Burp defaults, which include all the necessary settings. Configuration files can often be found in resources like HackerOne's bug bounty programs.

<figure><img src="../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

Once Burp has successfully started, the dashboard appears, allowing us to perform automated vulnerability assessments or simply crawl a website, depending on our needs.

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

The dashboard displays a task list where we can view all the tasks assigned to Burp.

<figure><img src="../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

### Intercepting Web Requests

In Burp, we can navigate to the `Proxy`tab, where we can turn interception on or off.&#x20;

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

### Intercepting Web Responses

We can do this by either `Select Request > Right Click > Do Intercept > Response to this Request` or By Intercepting response for all requests in the `Options setting`. &#x20;

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption><p>Intercepting response for a specific request</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption><p>Response interception for all requests</p></figcaption></figure>

### Automatic Modification

Note that we can configure Burp Suite to modify request or response automatically for particular things using `Tools > Proxy > Match and Replace`

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

### Repeating Requests

We can modify the request as well as response before it goes towards or from the browser. Also if we want to perform manual penetration testing or want to send a single request again and again with different parameters then we can send it to the Repeater using Ctrl + R.

<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (105).png" alt=""><figcaption><p>Sending same request again and again by clicking on the send button. </p></figcaption></figure>

**Configuring filter for HTTP history**

<figure><img src="../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

### Encoding/Decoding

To access the full encoder in Burp, we can go to the `Decoder` tab.&#x20;

<figure><img src="../../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

In recent versions of Burp, we can also use the `Burp Inspector tool` to perform encoding and decoding (among other things), which can be found in various places like `Burp Proxy or` `Burp Repeater`.&#x20;

### Fuzzing with Burp

If we want to perform brute force for various things then we can use the Intruder.

<figure><img src="../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (112).png" alt=""><figcaption><p>To select position of bruteforce we can simply select text and click on Add on the right hand side. </p></figcaption></figure>

#### Types of Attack

<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

* **Sniper**: Tests one payload position at a time using a single set of payloads.
* **Battering Ram**: Sends the same payload to all defined positions simultaneously.
* **Pitchfork**: Uses multiple payload sets in parallel, assigning one set to each position.
* **Cluster Bomb**: Generates combinations of payloads by iterating through multiple sets across positions.

#### Sniper

<figure><img src="../../.gitbook/assets/image (114).png" alt=""><figcaption><p>If we're targeting a single position with huge set of payload</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

#### Payload Processing

We can perform processing on payload and add rules as we want.

<figure><img src="../../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

Also if the website we are targeting is going down on a short brute force then we can increase the delay between the request by creating a new resource pool in the Resource pool tab.

<figure><img src="../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

#### Pitchfork

<figure><img src="../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

#### Clusterbomb

<figure><img src="../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

## OWASP ZAP

### Setting Up

Once ZAP starts up, unlike the free version of Burp, we will be prompted to either create a new project or a temporary project. Let's use a temporary project by choosing `no`, as we will not be working on a big project that we will need to persist for several days:

<figure><img src="../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

### Intercepting Web Requests

In ZAP, interception is off by default. We can turn it on by clicking on the highlighted green button or clicking `Ctrl + B`.

<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
In ZAP we can intercept the response in ZAP HUD. Displayed in ZAP HUD Section.&#x20;
{% endhint %}

### Automatic Modification

In ZAP the `Replacer` can be accessed by pressing `Ctrl + R` or clicking on `Replacer` in ZAP's options menu.&#x20;

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

### Repeating Requests

In ZAP, once we locate our request, we can right-click on it and select `Open/Resend with Request Editor`, which would open the request editor window, and allow us to resend the request with the `Send` button to send our request:

{% hint style="info" %}
We can also see the `Method` drop-down menu, allowing us to quickly switch the request method to any other HTTP method.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

### Encoding/Decoding

In ZAP, we can use the `Encoder/Decoder/Hash` tool or press `Ctrl + E`, which will automatically decode strings using various decoders in the `Decode` tab:

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

## Proxying Tools

{% hint style="info" %}
Proxying tools usually slows them down, therefore, only proxy tools when you need to investigate their requests, and not for normal usage.
{% endhint %}

### cURL with ProxyChains

To use `proxychains`, we first have to edit `/etc/proxychains.conf`commented out the final line and add the following line at the end of it:&#x20;

<figure><img src="../../.gitbook/assets/image (110).png" alt=""><figcaption><p>Comment out the socks4 line and add same as it but change port. </p></figcaption></figure>

Then make request using `proxychains <tool> or cURL` which will get intercepted in the proxying tool.&#x20;

### Metasploit

We can set proxy in Metasploit using `set PROXIES HTTP:127.0.0.1:8080`

### Fuzzing with ZAP

Right click on Web request and select (`Attack>Fuzz`)

<figure><img src="../../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

Same as burp we can select text (position for bruteforce) and click on `Add...`

Types of payload supported in Zap are:&#x20;

<figure><img src="../../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

We can add processing on the payloads too.&#x20;

<figure><img src="../../.gitbook/assets/image (124).png" alt=""><figcaption><p>By clicking on processor we can add processor.</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption><p>Multiple processor available in ZAP. </p></figcaption></figure>

Change speed and delay if you want.&#x20;

<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>
