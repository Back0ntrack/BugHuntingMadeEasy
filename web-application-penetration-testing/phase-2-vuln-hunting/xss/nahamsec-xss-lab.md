---
hidden: true
---

# Nahamsec XSS Lab

## 1. Inline HTMLi

### 1.1 Tag Breaking (Input Tag)

#### Target

<figure><img src="../../../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

#### Try to break the tag

<figure><img src="../../../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (183).png" alt=""><figcaption></figcaption></figure>

### 1.2 Tag Breaking (TextArea)

#### Target

<figure><img src="../../../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

#### Try to break the tag

<figure><img src="../../../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>

### 1.3 No Tag Breaking

#### Target

<figure><img src="../../../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

#### Executing XSS without breaking

<figure><img src="../../../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (182).png" alt=""><figcaption></figcaption></figure>

## 2. Context JS Variable

#### Target

<figure><img src="../../../.gitbook/assets/image (187).png" alt=""><figcaption></figcaption></figure>

#### Check for reflections

<figure><img src="../../../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

#### Try to break the <kbd>\<span></kbd> tag

<figure><img src="../../../.gitbook/assets/image (189).png" alt=""><figcaption></figcaption></figure>

We can see that there is sanitization in place due to which we're unable to break `<span>` tag.&#x20;

#### Try to escape the JS Context

<figure><img src="../../../.gitbook/assets/image (190).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (191).png" alt=""><figcaption></figcaption></figure>

#### Method 2

<figure><img src="../../../.gitbook/assets/image (192).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

## 3. XSS in JSON Response&#x20;

#### Target

<figure><img src="../../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Interacting with the website

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Analyzing the request&#x20;

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

This seems to be an API but the content type is `text/html`. It should be `application/json` in the case if it is using an API. So we picked the first mistake here. Note that we can also try to modify the `Accept:` header in the request to allow it to use `text/html`.&#x20;

#### Check for reflections

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

yay! we get into HTML.&#x20;

#### Check for HTMLi

<figure><img src="../../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Check for XSS

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 4. Filter Evasion

### 4.1 Script Tag Filtered

#### Target

<figure><img src="../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

#### Check for reflections&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Check for HTMLi

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

#### Script tag is filtered

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

The script tag is replaced with NULL (`''`) values.&#x20;

#### XSS Using event handlers

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

### 4.2 Attributes are filtered

#### Target

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

#### Check for reflections

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

#### Check for HTMLi

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

#### Executing XSS using event handlers&#x20;

<figure><img src="../../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

Using `<img>` tag.&#x20;

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

So we can see attributes are set to Null (`''`) values but initial values of tag are as it is. So we can use `iframe` or `href` here.&#x20;

<figure><img src="../../../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

## 5. CSP Bypass

You can use the following online tools to check Content Security Policy:

* [https://csp-evaluator.withgoogle.com/](https://csp-evaluator.withgoogle.com/)
* [https://cspvalidator.org/](https://cspvalidator.org/)

{% hint style="info" %}
_Only the `unsafe-inline` and `unsafe-eval` allow us to execute our XSS payloads if there is no validation in place._&#x20;
{% endhint %}

Other Bypass Techniques:&#x20;

* [Cobalt.io](https://www.cobalt.io/blog/csp-and-bypasses)
* [Payatu](https://payatu.com/blog/content-security-policy/)

### 5.1 CSP URI Scheme Bypass

#### Target

<figure><img src="../../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

We can see that CSP is in place which is forcing to use scripts from the same origin and `https://app.hackinghub.io` specific domain and `data:` is allowing inline scripts embedded as `data:` in the HTML.&#x20;

#### Checking if object data is allowed to execute XSS

Payload: **`<object data="data:text/html,<script>alert91)</script>"</object>`**

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

We can see that it is getting blocked due to the CSP policy. It would work if there is `object-src: 'self' data:` in the CSP.&#x20;

#### Executing XSS Using Script src

Payload: **`<script/src="data:text/javascript,alert('Hi')"></script>`**

<figure><img src="../../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

### 5.2 CSP JSONP Bypass

#### Target

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

We can see that now it is allowing three domains to execute scripts: `app.hackinghub.io`, `www.google.com` and `www.youtube.com`. &#x20;

#### Abusing 3rd party JSON results to bypass the CSP

Data from other websites can be fetched using JSONP. We've to find the JSONP endpoint of the whitelisted URLs and set a callback function.&#x20;

Payload: **`<script/src=https://www.youtube.com/oembed?url=https://www.youtube.com/watch?v=Z_Kk1zf16l4&callback=alert(1)></script>`**

<figure><img src="../../../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

This is not working in firefox.&#x20;

Some good payloads: \`[https://github.com/zigoo0/JSONBee/blob/master/jsonp.txt](https://github.com/zigoo0/JSONBee/blob/master/jsonp.txt)\`&#x20;

### CSP Upload Bypass

#### Target

<figure><img src="../../../.gitbook/assets/image (216).png" alt=""><figcaption></figcaption></figure>

We can login using the provided credentials.&#x20;

Let's intercept and check the request.&#x20;

<figure><img src="../../../.gitbook/assets/image (217).png" alt=""><figcaption></figcaption></figure>

Any javascript will not be executed except it is sent from the source. Let's check these two payloads in the input.&#x20;

```
<script>alert(1)</script>
```

```
<script src='https://nahamsec.com/1.js'>
```

<figure><img src="../../../.gitbook/assets/image (218).png" alt=""><figcaption></figcaption></figure>

Thus we can see that our javascript payloads are getting blocked. Thus to bypass this we can upload the file and try to execute XSS exploiting the `self` directive.&#x20;

Thus we intercepted the `Upload Profile Picture` request and let's inject XSS file.&#x20;

<figure><img src="../../../.gitbook/assets/image (222).png" alt=""><figcaption></figcaption></figure>

We can see the image location by right clicking on the image and `Open in New Tab`. And we can see our payload there.&#x20;

<figure><img src="../../../.gitbook/assets/image (223).png" alt=""><figcaption></figcaption></figure>

Final Payload: `<script src='https://437awe1c.eu1.ctfio.com/csp-upload/uploads/57e581995754d0d64c5c655728a0d831.js'></script>`

<figure><img src="../../../.gitbook/assets/image (221).png" alt=""><figcaption></figcaption></figure>
