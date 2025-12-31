# Reflected XSS (NodeJS Lab)

## Case - 1 URL Query Parameter

#### Target

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Find parameters of the URL&#x20;

{% tabs %}
{% tab title="Using arjun" %}
<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Using Bruteforce" %}
<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

#### Check Reflections

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Check for HTML Injection

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Check for XSS

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**What actually happened in the backend:**&#x20;

<figure><img src="../../../.gitbook/assets/image (11) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<details>

<summary><strong>Understanding the beyond logic</strong></summary>

<figure><img src="../../../.gitbook/assets/image (12) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

In this scenario, `profile.html` served as a template processed by `index.js`. When a request was made to `/cases/case1/profile`, the system fetched the `profile.html` template and replaced the `{{ USER }}` placeholder with the value from the GET request, defaulting to "Guest" if no user was provided with no sanitization or validation resulting in XSS.&#x20;

</details>

### Implementing Security

{% code overflow="wrap" %}
```javascript
const express = require('express');
const fs = require('fs');
const path = require('path');

const app = express();
const PORT = process.env.PORT || 3000;
const base = __dirname;

// helper: naive strip of <script> tags (level2)
function stripScriptTags(s) {
  return s.replace(/<\s*\/?\s*script\b[^>]*>/gi, '');
}

// helper: HTML-escape (level3+)
function escapeHtml(s) {
  if (s === undefined || s === null) return '';
  return String(s)
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#39;');
}

// helper: whitelist username (level5)
function whitelistUser(s) {
  if (!s) return '';
  // allow letters, numbers, dash and underscore, 1..30 chars
  if (/^[\w\-]{1,30}$/.test(s)) return s;
  return '';
}

// serve static assets from this folder
app.use('/cases/case1', express.static(base));

// LEVEL 1: fully vulnerable (direct reflection)
app.get('/cases/case1/level1', (req, res) => {
  const user = req.query.user ? req.query.user : 'Guest';
  const tpl = fs.readFileSync(path.join(base, 'profile_level1.html'), 'utf8');
  res.send(tpl.replace(/{{USER}}/g, user));
});

// LEVEL 2: naive filtering (strip simple <script> tags)
app.get('/cases/case1/level2', (req, res) => {
  let user = req.query.user ? req.query.user : 'Guest';
  user = stripScriptTags(user);
  const tpl = fs.readFileSync(path.join(base, 'profile_level2.html'), 'utf8');
  res.send(tpl.replace(/{{USER}}/g, user));
});

// LEVEL 3: HTML entity encoding for output context
/*
However, if the same value is later inserted into a different context (an attribute, a URL, or a JS string) without the correct context encoding, an attacker can still find a bypass relevant to that context.
*/
app.get('/cases/case1/level3', (req, res) => {
  let user = req.query.user ? req.query.user : 'Guest';
  user = escapeHtml(user);
  const tpl = fs.readFileSync(path.join(base, 'profile_level3.html'), 'utf8');
  res.send(tpl.replace(/{{USER}}/g, user));
});

// LEVEL 4: Context-aware encoding (HTML body + safe JS embedding)
app.get('/cases/case1/level4', (req, res) => {
  const raw = req.query.user ? req.query.user : 'Guest';
  // body-safe
  const user_html = escapeHtml(raw);
  // JS-safe via JSON.stringify (produces a quoted, escaped JS string)
  const user_js = JSON.stringify(String(raw));
  let tpl = fs.readFileSync(path.join(base, 'profile_level4.html'), 'utf8');
  tpl = tpl.replace(/{{USER_HTML}}/g, user_html);
  tpl = tpl.replace(/{{USER_JS}}/g, user_js); // inserts into <script> var user = {{USER_JS}};
  res.send(tpl);
});

// LEVEL 5: Whitelist validation + encoding
app.get('/cases/case1/level5', (req, res) => {
  let user = req.query.user ? req.query.user : 'Guest';
  const wl = whitelistUser(user);
  user = wl ? wl : 'Guest';
  user = escapeHtml(user);
  const tpl = fs.readFileSync(path.join(base, 'profile_level5.html'), 'utf8');
  res.send(tpl.replace(/{{USER}}/g, user));
});

app.listen(PORT, () => {
  console.log(`Case1 server running on http://localhost:${PORT}`);
  console.log(`Levels: /cases/case1/level1 .. /cases/case1/level5`);
});

```
{% endcode %}

> _It doesn't matter which backend language the developer used (Node.js, PHP, or anything else): ultimately the server returns HTML (from templates, PHP files, etc.). So we've to find tricks to break out._&#x20;
