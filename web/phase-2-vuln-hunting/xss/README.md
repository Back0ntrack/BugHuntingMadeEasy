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

<figure><img src="../../../.gitbook/assets/image (41) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 1. URL Reflection

When URL is reflected somehow in source code, we can add our own XSS vector/payload to it. For PHP pages it’s possible to do add anything in the URL after page name (without changing it) with the use of a slash character (/).

<figure><img src="../../../.gitbook/assets/image (43) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (44) (1).png" alt=""><figcaption></figcaption></figure>

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

## Reflected XSS Possible Cases&#x20;

1. **URL Query Parameters (GET)**: Input from `?param=value` echoed unsanitized.\
   &#xNAN;_&#x45;xample_: `search.php?q=<script>alert(1)</script>`
2. **HTTP Request Body (POST Data)**: Form data reflected without escaping.\
   &#xNAN;_&#x45;xample_: POST `message=<script>alert(1)</script>` to form handler.
3. **HTTP Headers**: Headers like User-Agent echoed directly.\
   &#xNAN;_&#x45;xample_: Set `User-Agent: <script>alert(1)</script>` via proxy.
4. **Cookies**: Cookie values displayed or used in output.\
   &#xNAN;_&#x45;xample_: `?setcookie=<script>alert(1)</script>` then view page.
5. **Error Pages (404/500)**: URI or error details reflected in messages.\
   &#xNAN;_&#x45;xample_: `nonexistent.php?q=<script>alert(1)</script>`
6. **Redirect/Next URLs**: Params in `Location` or meta refresh.\
   &#xNAN;_&#x45;xample_: `redirect.php?next=javascript:alert(1)`
7. **Filenames/Uploads (Simulated)**: Filename params echoed as text.\
   &#xNAN;_&#x45;xample_: `upload.php?file=<script>alert(1)</script>`
8. **Text Between Tags**: Input inside `<p>` or `<div>` allowing tag injection.\
   &#xNAN;_&#x45;xample_: `<img src=x onerror=alert(1)>` in text field.
9. **HTML Attributes (Unquoted)**: In unquoted attrs like `value=userinput`.\
   &#xNAN;_&#x45;xample_: `foo" onfocus=alert(1)`
10. **HTML Attributes (Single/Double Quoted)**: In quoted attrs like `value="userinput"`.\
    &#xNAN;_&#x45;xample_: `" onfocus=alert(1)" x="` (for double quotes).
11. **Event Handlers**: In `on*` attrs like `onclick=userinput`.\
    &#xNAN;_&#x45;xample_: `alert(1)` in onclick field, then trigger event.
12. **URLs in Attributes**: In `href`/`src` allowing `javascript:`.\
    &#xNAN;_&#x45;xample_: `javascript:alert(1)` in link href.
13. **Inside Script Strings**: In JS like `var x = "userinput";`.\
    &#xNAN;_&#x45;xample_: `"; alert(1); //`
14. **Script Tag Termination**: Breaking out of `<script>` to inject HTML.\
    &#xNAN;_&#x45;xample_: `'; </script><img src=x onerror=alert(1)>`
15. **Inline HTML (No Tag Breaking, Events Only)**: Escaped text but events injectable.\
    &#xNAN;_&#x45;xample_: `' onmouseover=alert(1)//` in input attr.
16. **Escaped JS Injection**: Backslash-escaped strings (e.g., addslashes).\
    &#xNAN;_&#x45;xample_: `\\"; alert(1); //` to escape the escape.
17. **HTTP Method Reflection**: Echoing `$_SERVER['REQUEST_METHOD']`.\
    &#xNAN;_&#x45;xample_: Tamper to POST, but payload in related param like `<script>alert(1)</script>`.
18. **Path/URI Segments**: Reflecting `$_SERVER['PATH_INFO']` or routes.\
    &#xNAN;_&#x45;xample_: `/page/<script>alert(1)</script>` if routed.
19. **JSON Responses**: Unsanitized input in JSON strings.\
    &#xNAN;_&#x45;xample_: `{"data": "<script>alert(1)</script>"}` → `"}; alert(1); //`
20. **DOM-Based (Client-Side Reflection)**: JS using `location.search` unsafely.\
    &#xNAN;_&#x45;xample_: `document.write(location.search)` with `?q=<script>alert(1)</script>`.

## Ultimate Hint

The main hint for finding XSS is **reflections** — check where your input is echoed back. It doesn't matter which backend language the developer used (Node.js, PHP, or anything else): ultimately the server returns HTML (from templates, PHP files, etc.). Focus on how the input is rendered and whether it can be escaped. Developers may have put defenses in place (sanitization, validation, encoding), so you need to test how those are implemented and bypassed.

Steps to check for XSS:

1. Identify where your input is reflected — cookies, headers, URL path/parameters, POST data, or any of the reflection cases in the lab.
2. Probe by injecting characters used in JavaScript/HTML (`<`, `>`, `"`, `'`, `/`, `)`, `;`, etc.).
3. Try to break out of the surrounding context: if you’re inside a tag, attempt attribute-based attacks; if you’re inside a script/string, try to break the string or script context.
4. Use context-appropriate payloads — there is no single “master” payload that works everywhere. Learn to read the context and craft payloads accordingly.
