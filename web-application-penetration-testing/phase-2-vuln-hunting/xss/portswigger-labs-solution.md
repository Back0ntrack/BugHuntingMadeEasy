# Portswigger Labs Solution

## Reflected XSS into HTML context with nothing encoded

<div data-full-width="true"><figure><img src="../../../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><em>payload is reflected in the <mark style="color:red;">&#x3C;h1></mark> tag</em></p></figcaption></figure></div>

<figure><img src="../../../.gitbook/assets/image (13) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><em>context successfully escaped using <code>'></code></em></p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (14) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>payload: <code>&#x3C;img/src=x onerror=alert();//</code></p></figcaption></figure>

## Reflected XSS into attribute with angle brackets HTML-encoded

<figure><img src="../../../.gitbook/assets/image (15) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><em>note that the payload is reflected at two places as seen in next pic.</em> </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (17) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
_So this time, we can craft the payload to trigger XSS in both the `<h1>` tag and the input field, since the payload is reflected in both locations._
{% endhint %}

### Trying to trigger XSS by escaping \`\<h1>\` tag

<figure><img src="../../../.gitbook/assets/image (18) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (19) (1) (1) (1) (1).png" alt=""><figcaption><p><em>Angle brackets are HTML-encoded, so we can't escape the context.</em></p></figcaption></figure>

### Trying to trigger XSS by escaping the \`\<input>\` tag context

<figure><img src="../../../.gitbook/assets/image (20) (1) (1) (1).png" alt=""><figcaption><p><em>payload: <code>" onmouseover=alert(1);//</code></em></p></figcaption></figure>

Thus, we successfully triggered XSS inside the input tag using an event handler.

## Reflected XSS into a JavaScript string with angle brackets HTML encoded

<figure><img src="../../../.gitbook/assets/image (23) (1) (1) (1).png" alt=""><figcaption><p><em>The payload is reflected in three different locations, as shown in the image below.</em></p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (22) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/image (24) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (25) (1) (1) (1).png" alt=""><figcaption><p><em>Since angle brackets are HTML encoded we can't  trigger XSS in H1.</em> </p></figcaption></figure>

### Trying to trigger XSS by escaping \`searchTerms\`  variable

<figure><img src="../../../.gitbook/assets/image (26) (1) (1) (1).png" alt=""><figcaption><p>payload used: <code>var_value'; alert();//</code></p></figcaption></figure>

### Trying to trigger XSS in \`\<img>\` tag

> _Since both angle brackets and quotes are HTML-encoded, we can't escape the `<img>` tag context._

<figure><img src="../../../.gitbook/assets/image (27) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Reflected XSS into HTML context with most tags and attributes blocked

<mark style="color:red;">**Level: Intermediate (WAF in action)**</mark>

<figure><img src="../../../.gitbook/assets/image (28) (1) (1) (1).png" alt=""><figcaption><p><em>only 1 reflection in the <code>&#x3C;h1></code> tab.</em> </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (29) (1) (1) (1).png" alt=""><figcaption><p><em>tags are blocked. oh no.</em> </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (30) (1) (1) (1).png" alt=""><figcaption><p><em>custom tags seems to be worked as in the below pic.</em> </p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (31) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (32) (1) (1) (1).png" alt=""><figcaption><p><em>Custom tag went through, but the attribute ruined the game. payload used: </em><kbd><em>&#x3C;hola onmouseover=alert()>here&#x3C;/hola></em></kbd></p></figcaption></figure>

{% hint style="info" %}
_Whenever we get errors like 'Attribute is not allowed' or 'Tag is not allowed', the response status code is 400._
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (33) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (34) (1) (1).png" alt=""><figcaption></figcaption></figure>

_We've found some attributes that might work, but our custom tag isn't getting through—so we need to brute-force again to find which tag is allowed._

<figure><img src="../../../.gitbook/assets/image (35) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (36) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now that we know which tag works, let’s use it.

<figure><img src="../../../.gitbook/assets/image (37) (1) (1).png" alt=""><figcaption><p><kbd><em>&#x3C;body onresize=alert()>hola</em></kbd><br><em>other working payload: </em><kbd><em>`'&#x3C;body/onresize=alert();//`</em></kbd></p></figcaption></figure>

## Reflected XSS into HTML context with all tags blocked except custom ones

<mark style="color:red;">**Level: Intermediate (WAF in action)**</mark>

<figure><img src="../../../.gitbook/assets/image (38) (1) (1).png" alt=""><figcaption><p><em>reflection only in `&#x3C;h1>` tag.</em> </p></figcaption></figure>

Tags are blocked.&#x20;

<figure><img src="../../../.gitbook/assets/image (39) (1) (1).png" alt=""><figcaption><p><em>payload: <code>&#x3C;hola onmouseover=alert()>here&#x3C;/hola></code></em></p></figcaption></figure>

If filtering is based on opening and closing tags, use a payload like `` ` <hola onmouseover=alert(1);// `` to bypass it. Make sure to know where to mouse over, as it's possible you might be hovering over something else and the XSS won't trigger.&#x20;

{% hint style="info" %}
Other good event handler: `onfocus` But you need to call it as:&#x20;

`<xss id=x onfocus=alert(document.cookie) tabindex=1>#x';`
{% endhint %}

## Reflected XSS in canonical link tag

As given in the title we're not provided any place to fed our input (i.e., search form, or any other input tag). Thus as per the title let's understand the `canonical tag`.&#x20;

<figure><img src="../../../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

#### Canonical tag

A **canonical tag** is an HTML element that tells search engines which version of a **duplicate or similar webpage** is the **preferred (original) version** to index.

```
<link rel="canonical" href="https://example.com/preferred-page/" />
```

**Purpose:**\
To prevent **duplicate content issues** by consolidating ranking signals (like backlinks and crawl budget) to one **canonical URL**.

**How It Works:**\
When multiple URLs show similar content (e.g., `?sort=`, `/amp/`, or both HTTP/HTTPS versions), the canonical tag guides search engines to treat the specified URL as the **main source**.

#### Example:&#x20;

Suppose the website **`https://www.example.com`** is an e-commerce site.\
Each product page can be accessed with different query parameters, such as:

```
https://www.example.com/products?id=1  
https://www.example.com/products?id=2  
https://www.example.com/products?id=3
```

Even though the URLs are different, the **base content (the main product listing page)** might be similar or derived from the same source.\
To avoid duplicate content and tell search engines which version to index as the **main (preferred) page**, you can use a **canonical tag** like this:

```
<link rel="canonical" href="https://www.example.com/products" />
```

This tells Google and other search engines:

> “These pages (`?id=1`, `?id=2`, etc.) are variations of the same content — please treat `https://www.example.com/products` as the original (canonical) page and consolidate ranking signals there.”

:sob: This lab has specific simulation that user will click some shortcut keys and has used this method to execute XSS **`?accesskey='x'&onclick='alert(1)`**

## Reflected XSS into a JavaScript String with single quote and backslash escaped&#x20;

### Trying to execute XSS in H1 tag&#x20;

Payload used: `</h1><script>alert(1)</script>`

<figure><img src="../../../.gitbook/assets/image (200).png" alt=""><figcaption></figcaption></figure>

Angle brackets are HTML encoded

### Trying to execute XSS in JavaScript Variable

Payload used: `'; alert(1)//`

We can see that the single quotes are escaped with `/` due to which we're unable to come out of the quotes.&#x20;

<figure><img src="../../../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

Even if we use double quotes we're unable to escape the single quotes and execute XSS. Thus I checked internet for some payloads and got this one.&#x20;

```
'"`><script>/* *\x2Fjavascript:alert(1)// */</script>
```

Even if this one didn't worked but I got some hint see the below image.&#x20;

<figure><img src="../../../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>

We've escaped out the code successfully but XSS isn't triggered. So I checked the source code.&#x20;

<figure><img src="../../../.gitbook/assets/image (203).png" alt=""><figcaption></figcaption></figure>

Thus we can see that we've jumped out to `document.write` because of the `</script>` snippet. So finally we can break out of the tag and execute XSS.&#x20;

#### Finally Found XSS&#x20;

Payload used: **`</script><script>alert(1)</script>`**

<figure><img src="../../../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

## Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped

<figure><img src="../../../.gitbook/assets/image (204).png" alt=""><figcaption></figcaption></figure>

#### Checking the reflections&#x20;

<figure><img src="../../../.gitbook/assets/image (205).png" alt=""><figcaption></figcaption></figure>

So we can see that the backend is injecting the input in a message variable and then using javascript it is dynamically inserted into the h1 tag.&#x20;

### Trying to break the h1 tag&#x20;

Payload used: `'></h1><img/src=x>'`

<figure><img src="../../../.gitbook/assets/image (206).png" alt=""><figcaption></figcaption></figure>

We can see that everything is encoded and escaped. There is no way of escaping and breaking out of these stuffs.&#x20;

Thus we can see those backticks (`` ` ``) being used there. It tells that [Javascript template](../../../penetration-testing-essentials/web-development-essentials/frontend/javascript-js.md#javascript-template-literal) literals is being used there. thus we can execute javascript inside `${here}` .&#x20;

<figure><img src="../../../.gitbook/assets/image (207).png" alt=""><figcaption></figcaption></figure>

### Executing XSS

<figure><img src="../../../.gitbook/assets/image (208).png" alt=""><figcaption></figcaption></figure>

## Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

<figure><img src="../../../.gitbook/assets/image (209).png" alt=""><figcaption></figcaption></figure>

#### Check reflections&#x20;

<figure><img src="../../../.gitbook/assets/image (210).png" alt=""><figcaption></figcaption></figure>

After checking for reflections within the H1 tag and a variable in the script tag I tried multiple payloads to escape out of the context. But the single quotes are escaped and the double quotes and encoded.&#x20;

<figure><img src="../../../.gitbook/assets/image (211).png" alt=""><figcaption></figcaption></figure>

### Escaping the escape

<figure><img src="../../../.gitbook/assets/image (212).png" alt=""><figcaption></figcaption></figure>

### Executing XSS

<figure><img src="../../../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>



Final payload: `abhi\'; alert(1)//`

<figure><img src="../../../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (215).png" alt=""><figcaption></figcaption></figure>

Other working payload: `\'-alert(1)//`

## DOM XSS in `document.write` sink using source `location.search`

#### Target

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Checking for reflections

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We get two reflections. In the `<h1>` tag and in the `<img src...>` tag.&#x20;

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can see a `<script>` tag where a function is written to dynamically add the input in the HTML.&#x20;

### Trying to break the h1 tag

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

But since angle brackets are encoded we can't break out of the h1 tag.&#x20;

### Trying to break the image tag

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## DOM XSS in innerHTML sink using source location.search

#### Target

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Checking for reflections



<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can see only one reflection and that is because of this script.&#x20;

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

This script is dynamically adding the input in the `<span>` tag.&#x20;

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## DOM XSS in jQuery anchor `href` attribute sink using `location.search` source

#### Target

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

On clicking `Submit Feedback` we got a form.&#x20;

Found an interesting script in the source code of the form. There is nothing that seems to be reflecting in the form.&#x20;

<figure><img src="../../../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can see that the `returnPath` that is in the URL is appending to anchor tag.&#x20;

<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Executing XSS

<figure><img src="../../../.gitbook/assets/image (13) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
