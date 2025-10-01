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

**What actually happened in backend:** \


<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

_since we're escaping two times we're getting two popup._&#x20;

<details>

<summary><strong>Understanding the beyond logic</strong></summary>

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

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

