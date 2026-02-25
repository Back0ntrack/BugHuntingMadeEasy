# Portswigger Labs Solution

## CSRF Vulnerability with no defenses

### Target

<figure><img src="../../../.gitbook/assets/image (42) (1).png" alt=""><figcaption></figcaption></figure>

We're provided with an account `wiener:peter` and some blog posts are provided. There is no option for registration.&#x20;

### Functionality

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Thus we've an email change functionality in which we can change the email of the user.&#x20;

Let's intercept and see the actual request going through.&#x20;

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can see that we've successfully got 302 request which redirects to the same page again with changed mail id.&#x20;

### Exploitation

Generate CSRF PoC using CSRF PoC Generator in Burp Suite using Engagement Tools for the same above request and deliver it to victim.&#x20;

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

When the victim will open the website in which this code is hosted his email will automatically change to `change@mail.com`.&#x20;

## CSRF where token validation depends on request method&#x20;

### Target

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We're provided with an account `wiener:peter` and some blog posts are provided. There is no option for registration.&#x20;

### Functionality

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Thus we've an email change functionality in which we can change the email of the user.&#x20;

Let's intercept and see the actual request going through.&#x20;

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Exploitation

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can see that it is also accepting get requests. Let's try to change email without CSRF token through GET request in the repeater and we can see that we've successfully changed the email without csrf token.&#x20;

<figure><img src="../../../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now we can generate csrf PoC using burp suite and send it to victim to execute our CSRF attack.&#x20;

## CSRF where token validation depends on token being present

### Target

<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

We're provided with an account `wiener:peter` and some blog posts are provided. There is no option for registration.&#x20;

### Functionality

<figure><img src="../../../.gitbook/assets/image (13) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Thus we've an email change functionality in which we can change the email of the user.&#x20;

Let's intercept and see the actual request going through.&#x20;

<figure><img src="../../../.gitbook/assets/image (14) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (15) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Exploitation

We can see that even without CSRF token present therein the email id is changing.&#x20;

<figure><img src="../../../.gitbook/assets/image (16) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Thus generate CSRF PoC using Burp CSRF PoC Generator and send it to the victim.

## CSRF where token is not tied to user session

### Target

<figure><img src="../../../.gitbook/assets/image (17) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

This time we've been provided with two user accounts in which we're able to change the email id as previous labs.&#x20;

### Functionality

<figure><img src="../../../.gitbook/assets/image (18) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Let's try to intercept and check the request for updating the email address.&#x20;

<figure><img src="../../../.gitbook/assets/image (19) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (20) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Exploitation

This time CSRF parameter is mandatory and we can't change email without supplying email parameter and the CSRF token is changing on each request.&#x20;

<figure><img src="../../../.gitbook/assets/image (22) (1) (1).png" alt=""><figcaption></figcaption></figure>

We can see that if we interchange the csrf token while intercepting both the request we can change the email id because the csrf token is not tied to the user session. Thus we can intercept a fresh request and craft a PoC using Burp CSRF PoC Generator to exploit the vulnerability.&#x20;

We're provided with two user accounts in this lab.&#x20;

