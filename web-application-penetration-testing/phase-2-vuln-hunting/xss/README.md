# XSS

Cross-Site Scripting is a client side vulnerability that allows attackers to inject malicious scripts into web pages. When a user loads a web page containing an XSS payload, the script executes it within their browser, potentially stealing cookies, session tokens, or other sensitive data, or performing actions on behalf of the user without their consent.

### Impact of XSS

* Cookie stealing/session hijacking: Stealing cookies from users with authenticated sessions, allowing you to login as other users by leveraging the authentication information contained in the cookie.
* Browser exploitation: Exploitation of browser vulnerabilities.
* Keylogging: Logging keyboard entries made by other users on a web application.
* Phishing: Injecting fake login forms into a webpage to capture credentials.
* Navigating user to the malicious website created by attacker.
* Downloading some malicious tool in the user’s machine without his consent and knowledge.

## Types of XSS

### Reflected XSS

Reflected Cross-site scripting arises when an application receives data in an HTTP request and includes that data within the immediate response in an unsafe way.

### Stored XSS

Stored cross site scripting is a vulnerability where an attacker is able to inject malicious JavaScript code into a web application's database or source code via an input that is not sanitized. For example, if an attacker is able to inject a malicious XSS payload into a webpage on a website without proper sanitization, the XSS payload injected in to the webpage will be executed by the browser of anyone that visits that webpage. Best example: Blogs, CMS.

### DOM Based XSS

DOM-Based XSS occurs when JavaScript on the client side processes user input and dynamically updates the Document Object Model (DOM) without proper validation or sanitization. This vulnerability allows an attacker to inject malicious scripts that are executed directly in the browser, entirely within the context of JavaScript execution. DOM XSS doesn't include communication with the server.&#x20;

{% hint style="info" %}
_**If the****&#x20;****`Content-Type`****&#x20;****is****&#x20;****`text/html`****, that’s when you should actively check for XSS vulnerabilities.** This is because the browser will interpret the response as HTML, allowing any injected scripts (like `<script>` tags or event handlers) to potentially execute._

_However, **if the****&#x20;****`Content-Type`****&#x20;****is****&#x20;****`application/json`****,****&#x20;****`text/plain`****, or any other non-HTML type**, the browser treats it as raw data — so injected scripts won’t execute directly, and **XSS is generally not exploitable** in that response unless the data is later rendered unsafely in an HTML context (like in a frontend app)._
{% endhint %}

## 6 Main XSS Cases😀

<figure><img src="../../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

### 1. URL Reflection

When URL is reflected somehow in source code, we can add our own XSS vector/payload to it. For PHP pages it’s possible to do add anything in the URL after page name (without changing it) with the use of a slash character (/).

<figure><img src="../../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

### 2. Inline HTMLi: Tag Breaking

As the username is reflected back in the same input field we can break out of it using `“>` and add our script tag.

<figure><img src="../../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

### 3. Inline HTMLi: No Tag Breaking

When input lands in an HTML attribute and there’s filtering of greater than character (>), it’s not possible to break out of current tag like in the previous case.

<figure><img src="../../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

### 4. HTML in JS Block

Input sometimes land into a javascript block (script tags), usually in the value of some variable of the code. But because the HTML tags has priority in the browser’s parsing, we can simple terminate the block and insert a new tag.

<figure><img src="../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

### 5. HTML in JS Block (Tag Filtering)

<figure><img src="../../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

### 6. Escaped JS Injection

<figure><img src="../../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

_**Escape the escape**_

<figure><img src="../../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

