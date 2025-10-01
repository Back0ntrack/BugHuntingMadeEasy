# Reflected XSS

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

**What actually hapenned in backend:** \
