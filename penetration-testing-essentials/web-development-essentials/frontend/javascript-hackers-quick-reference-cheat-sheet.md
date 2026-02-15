# JavaScript Hacker's Quick-Reference Cheat-Sheet

## Purpose & Hacker Mindset

* **JS = Dynamic behavior + Data Flow, Not Secure by Default:** JavaScript (JS) powers web app interactivity, manipulating the DOM, handling events, and communicating with servers. It’s executed client-side, making all code, variables, and logic accessible via browser DevTools or source inspection.
* **Primary Focus:** Identify data flows (inputs, APIs, storage), uncover hidden functionality (endpoints, secrets),dangerous sinks (eval/innerHTML/etc.).

{% hint style="info" %}
_From a hacker’s perspective, JS is a map to the server’s attack surface—every function, event, or API call is a potential entry point. The real security testing lies in how the server processes JS-driven inputs and responses._
{% endhint %}

***

## Core Focus Areas

### What a tester should check in the JavaScript&#x20;

#### 1. User → JS

* XSS occurs whenever untrusted input (either from URL, cookies, HTML forms etc.) is injected into JavaScript source (e.g., inside string literals, variables, or code) so it can break out of the intended context and execute — treat any user-supplied value embedded in JS as dangerous.

#### **2. Hardcoded Sensitive Data**

* API keys, JWTs, session tokens, or credentials in \<script> tags or external .js files.
* Hidden variables (e.g., const adminToken = 'xyz123').
* Configuration objects (e.g., { apiKey: 'abc123', endpoint: '/api/v1/data' }).

#### **3. API Endpoints & Network Calls**

* AJAX/fetch/XHR calls (e.g., fetch('/api/users'), $.post('/submit')).
* Sensitive URLs or endpoints in JS variables or functions (e.g., const baseUrl = '/api/v2').
* WebSocket connections or dynamic API routes.

#### **4. Event Listeners & User Interactions**

* Event handlers (e.g., onclick, oninput, onload) tied to forms, buttons, or inputs.
* Functions that trigger sensitive actions (e.g., deleteUser(), grantAccess()).
* Dynamic DOM manipulation (e.g., document.write(), innerHTML).

#### **5. Comments in Code**

* Devs leave sensitive info in HTML comments.
  * {% code overflow="wrap" %}
    ```
    // TODO: This endpoint is insecure and needs to be rewritten. For now, it's disabled.
    // fetch('/api/v2/old/insecure-data-dump')
    ```
    {% endcode %}
  * API keys, passwords, SQL queries sometimes left.

***

## Final Hacker’s Strategy

* **Enumerate all JS files** (inline, external, third-party) and unminify for analysis.
* **Extract sensitive data** (API keys, tokens, endpoints) from variables, comments, or logs.
* **Map all network calls** (AJAX, fetch, WebSocket) to identify backend endpoints.
* **Bypass client-side validation** to test server-side enforcement (SQLi, XSS, injection).
* **Invoke functions manually** via DevTools to trigger sensitive actions or uncover hidden features.
* **Mark potential attack vectors**: XSS (DOM-based), CSRF, IDOR, session hijacking, privilege escalation, or supply chain attacks.
* **Cross-reference with HTML**: Combine JS findings with form actions, input fields, and file uploads to build a complete attack surface.

{% hint style="info" %}
_**Key mindset:** JS is fully exposed, so attackers treat it as a blueprint to map the app and target the server’s trust in client-side data._
{% endhint %}
