---
hidden: true
---

# Nahamsec Labs

## Filename Vulns

When the filname of our file is reflected back as it is without any modifications then there are chances that we can execute XSS or command injection attacks using that. We've to try for multiple combinations and permutations with that filename and inject our script in between that.&#x20;

### Target&#x20;

<figure><img src="../../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

### Functionality

Whenever we upload a file, the name of the file is reflected back as it is in the response.&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Thus we can see that our underline is visible there.&#x20;

### Exploitation

Let's try to exploit this behavior of the web application.&#x20;

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Content-Type Bypass

### Target

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Functionality&#x20;

This time they are modifying the name of the file that the user is uploading so that he wouldn't be able to execute any XSS. But still remember if the file is displayed in the backend as well to the administrator or any other user then still there are chances of blind XSS being fired. Thus always use blind XSS paylods.

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Along with that all the uploaded files are not even listed in the user's response.&#x20;

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Exploitation

Let's try to modify requests without Content-Type and malforming the Content-Type.&#x20;

{% tabs %}
{% tab title="Without Content-Type" %}

{% endtab %}

{% tab title="Blocked " %}
<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Malforming the Content-Type" %}
<figure><img src="../../../.gitbook/assets/image (11) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We've fuzzed the Content-Type to know all the other Content-Type we can send in the request as here we've modified the Content-Type and thus the image is not displayed instead of which we get the raw format of the image unlike previous one.&#x20;

<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

Again modifying the Content-Type to `text%2fhtml` and adding payload `<script>alert()</script>` gave us XSS.&#x20;

<figure><img src="../../../.gitbook/assets/image (13) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

List of Content-Type Headers for XSS: [https://github.com/BlackFan/content-type-research/blob/master/XSS.md](https://github.com/BlackFan/content-type-research/blob/master/XSS.md)
