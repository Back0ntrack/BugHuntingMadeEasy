# HTML Hacker’s Quick-Reference Cheat-Sheet

## Purpose & Hacker Mindset

* **HTML = Presentation + Input Surface, not Fortress**: HTML only hints at behavior—nothing is secured. Attackers can read, modify, intercept, or recreate any behavior via DevTools or tools like Burp/cURL.
* **Primary focus**: Data entry points (forms) and file uploads — these are the richest vectors for real vulnerability exploitation.

{% hint style="info" %}
_From a hacker’s perspective, HTML is important for identifying input points, forms, file uploads, meta directives, links, and frames, but the real security testing always targets how the server handles this data._
{% endhint %}

***

## Core Focus Areas

### What a tester should check in the HTML <a href="#what-a-tester-should-check-in-the-html" id="what-a-tester-should-check-in-the-html"></a>

#### 1. **Form Structure & Input Fields** <a href="#id-1.-form-structure-and-input-fields" id="id-1.-form-structure-and-input-fields"></a>

* **Input types** (`text`, `password`, `hidden`, `email`, `number`, `file`, `date`, etc.)
  * Each input gives a hint about the backend expectation → e.g., `email` → try injection with `test@example.com'--`.
* **Hidden fields**
  * Look for sensitive values (tokens, roles, prices, product IDs, API keys).
  * Tampering = privilege escalation, price manipulation, etc.
* **Disabled fields**
  * Not sent by default → but attacker can enable or re-add manually.
* **Readonly fields**
  * Sent by default → but attacker can change in DevTools/proxy.
* **Autocomplete**
  * `autocomplete="off"` may leak creds if missing.
* **Input validation hints** (`maxlength`, `pattern`, `required`)
  * Client-side only → attacker bypasses easily.
  * Backend might wrongly assume validation is enforced.

#### 2. **File Uploads** <a href="#id-2.-file-uploads" id="id-2.-file-uploads"></a>

* `<input type="file">`
  * Check if multiple allowed (`multiple` attribute).
  * Any extension restrictions in `accept=".jpg,.png"` (client-side only, bypassable).
  * If no restrictions → risk of RCE via malicious upload.

#### 3. **Authentication/Authorization Clues** <a href="#id-3.-authentication-authorization-clues" id="id-3.-authentication-authorization-clues"></a>

* Login forms → look for:
  * `username` / `password` fields.
  * Hidden roles (`role=admin`).
  * Tokens (`auth=xyz123`).
* Signup forms → may leak role assignment fields.
* Password fields → look for missing `autocomplete="off"`.

#### 4. **Client-Side Scripts (JS in HTML)** <a href="#id-4.-client-side-scripts-js-in-html" id="id-4.-client-side-scripts-js-in-html"></a>

* Inline `<script>` or linked `.js` files.
  * Look for hardcoded secrets (API keys, JWTs, credentials).
  * Look for API endpoints or URLs.
  * Look for client-side validation logic (regex, length checks) → backend may not enforce.

#### 5. **URLs & Endpoints** <a href="#id-5.-urls-and-endpoints" id="id-5.-urls-and-endpoints"></a>

* `<form action="...">`
  * Reveals backend endpoints (`/login`, `/upload`, `/admin/deleteUser`).
  * Method (`GET` vs `POST`) — GET exposing sensitive data is a bug.
* `<a href>` links to hidden functionality (`/backup`, `/admin`, `/test.php`).
* API endpoints hidden in JS variables.

#### 6. **Comments in Code** <a href="#id-6.-comments-in-code" id="id-6.-comments-in-code"></a>

* Devs leave sensitive info in HTML comments.
  * E.g., `<!-- TODO: remove debug.php -->`
  * API keys, passwords, SQL queries sometimes left.

***

## Final Hacker's Strategy

1. Enumerate **all input fields** (visible/hidden/readonly/disabled).
2. Note **all form actions & methods** (endpoints, GET/POST).
3. Check **client-side restrictions** → mark them bypassable.
4. Inspect **comments, scripts, meta tags** → gather secrets.
5. Mark potential **attack vectors**: SQLi, XSS, LFI, CSRF, file upload, IDOR, etc.
