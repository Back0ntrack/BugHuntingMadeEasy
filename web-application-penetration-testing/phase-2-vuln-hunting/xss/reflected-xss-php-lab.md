# Reflected XSS (PHP Lab)

## Case - 1 URL Query Parameters

#### Target

<figure><img src="../../../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

#### Find parameter of the URL

{% tabs %}
{% tab title="Using arjun" %}
<figure><img src="../../../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Using brute force" %}
<figure><img src="../../../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (156).png" alt=""><figcaption></figcaption></figure>

#### Check HTML Injection

<figure><img src="../../../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

#### Check for XSS

<figure><img src="../../../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

**What actually happened in backend:**&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

_since we're escaping two times we're getting two popup._&#x20;

<details>

<summary><strong>Understanding the beyond logic</strong></summary>

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Because `$user` is inserted into the HTML template without escaping, an attacker-controlled value like `?user=<script>alert('XSS')</script>` will be reflected verbatim into the page — e.g. the server would return HTML that looks like `<h2>Welcome, <script>alert('XSS')</script>!</h2>` and `<p><strong>Username:</strong> <script>alert('XSS')</script></p>`, which the browser parses as actual markup and executes, demonstrating a classic reflected XSS.

</details>

### Implementing Security

{% tabs %}
{% tab title="Level2" %}
_This will replace `<script>` tags with `''` (NULL) values._&#x20;

```php
<?php
$user = isset($_GET['user']) ? $_GET['user'] : 'Guest';
// Remove <script> tags (very naive)
$user = str_ireplace(['<script>', '</script>'], '', $user);
?>
<!DOCTYPE html>
<html>
<head><title>Level 2 - Basic Filter</title></head>
<body>
<h2>Welcome, <?php echo $user; ?>!</h2>
<p>Username: <?php echo $user; ?></p>
</body>
</html>
```

_We can bypass it using other tags such as: `<img src=x onerror=alert(1)>`._&#x20;
{% endtab %}

{% tab title="Level3" %}
_Encodes special HTML characters (`<`, `>`, `"`), but does not cover all contexts (like JS or attribute injection)._

{% code overflow="wrap" %}
```php
<?php
$user = isset($_GET['user']) ? $_GET['user'] : 'Guest';
// Encode for HTML body context
$user = htmlspecialchars($user, ENT_QUOTES | ENT_HTML5, 'UTF-8');
/*
This calls htmlspecialchars() to convert special HTML characters in $user into safe HTML entities (so < becomes &lt;, " becomes &quot;, etc.), using ENT_QUOTES to also escape single quotes, ENT_HTML5 for HTML5 entity behavior, and treating the string as UTF‑8
*/
?>
<!DOCTYPE html>
<html>
<head><title>Level 3 - HTML Encoded</title></head>
<body>
<h2>Welcome, <?php echo $user; ?>!</h2>
<p>Username: <?php echo $user; ?></p>
</body>
</html>

```
{% endcode %}

_<mark style="color:$danger;">While the existing HTML encoding prevents Cross-Site Scripting (XSS) in the HTML body context, rendering the input in a JavaScript block would still create an XSS vulnerability.</mark>_
{% endtab %}
{% endtabs %}

## Case - 2 URL Path Segments

#### Target

{% hint style="info" %}
_By default path segments are percent-encoded according to RFC 3986. So there are minimal chances of getting XSS here. XSS is possible if the entered string is decoded and reflected in the output._&#x20;
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Check if segment is reflected back in the response

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

> _We can see that there is no response in the earlier case1 in which the output is not reflected back from segment._&#x20;

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Check HTML injection

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Check for XSS

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**What actually happened in the backend**

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Case - 3 HTTP Request Body (POST Data)

#### Target

<figure><img src="../../../.gitbook/assets/image (158).png" alt=""><figcaption></figcaption></figure>

#### Check reflections

<figure><img src="../../../.gitbook/assets/image (159).png" alt=""><figcaption></figcaption></figure>

#### Check for HTML Injection

<figure><img src="../../../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (161).png" alt=""><figcaption></figcaption></figure>

#### Check for XSS

<figure><img src="../../../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Understanding the beyond logic</summary>

Since our input is reflected directly into the body we're able to execute XSS here.&#x20;

<div data-full-width="true" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (164).png" alt=""><figcaption></figcaption></figure></div>

</details>

## Case - 4 View Headers&#x20;

#### Target

<figure><img src="../../../.gitbook/assets/image (165).png" alt=""><figcaption></figcaption></figure>

#### Check for reflections

<figure><img src="../../../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>

#### Check for XSS

<figure><img src="../../../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Understanding the beyond logic</summary>

We can see that the headers is directly reflected back into the response. So we can manipulate the header by intercepting the request and inject our malicious script.&#x20;

<div data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (171).png" alt=""><figcaption></figcaption></figure></div>

</details>

## Case - 5 Cookies&#x20;

#### Target

<figure><img src="../../../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

#### Checking for reflection&#x20;

<figure><img src="../../../.gitbook/assets/image (172).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_In this case, we’re given the chance to edit our cookies, but in real life, sometimes the cookie is reflected back in the response—often in a hidden input field—so it can be sent as data in the request. This allows us to inject malicious payloads by intercepting and modifying the request._
{% endhint %}

#### Checking for XSS

**Modify cookie directly in the storage section**

<figure><img src="../../../.gitbook/assets/image (174).png" alt=""><figcaption></figcaption></figure>

### Other Possible Cases

6. **Error pages (404/500):** Requested URI or error details reflected in error messages.
7. **Redirect/next URLs:** Parameters like `?next=` or `?redirect=` used in links or meta refreshes.
8. **Filenames/uploads:** Simulated filename parameters (e.g., `?file=`) echoed as uploaded text.
9. **Text between tags:** Input echoed as plain text inside `<p>` or `<div>`, allowing tag injection.
10. **HTML attributes (unquoted):** Input placed in unquoted attributes, e.g. `<input value=userinput>`.
11. **HTML attributes (single/double quoted):** Input placed inside quoted attributes, e.g. `<input value="userinput">`.
12. **Event handlers:** Input inserted into `on*` attributes such as `onclick`, `onerror`, `onload`, etc.
13. **URLs in attributes:** Input used in `href`/`src`/`action`/`background` etc., including `javascript:` URLs.
14. **Inside script strings:** Input included in JavaScript strings, e.g. `var x = "userinput";`, allowing breakout.
15. **Script tag termination:** Input that breaks out of a `<script>` block to inject HTML or new scripts.
16. **Inline HTML (no tag breaking):** User cannot break out of the tag but can still trigger XSS via event handlers (e.g. adding `onmouseover="alert(1)"`).
17. **Escaped JS injection:** Input is placed in a JS string and lightly escaped; XSS may still be possible by bypassing or undoing the escaping (for example via crafted sequences like `\-alert()\-`).
