# Understanding the XSS Input Context

Here are some contexts where we as a  Bug Hunter must include XSS payload.

### URLs & routing

* Path segments (e.g., /user/\<input>/profile)
* Query parameters (e.g., ?q=, ?id=, ?redirect=)
* Fragment/hash (#section, client-side routing values)

### HTTP request headers

* User-Agent
* Referrer
* Origin
* X-Forwarded-For, X-Real-IP, X-Requested-With
* Host
* Custom headers (e.g., app-specific headers)
* Cookie header (when echoed into pages or logs)

### Cookies & storage that later get reflected

* Cookie names and values
* localStorage, sessionStorage, IndexedDB, Service Worker cache keys (when used to generate DOM)
* Stored tokens or flags that are later rendered into pages

### Form inputs & form metadata

* Text inputs, textareas
* Hidden inputs and default values
* Select/option values, checkbox labels
* Placeholder and label text (if user-controlled)
* Form action attribute and form-encoded payloads

### File uploads & file metadata

* Uploaded file content (HTML/SVG/JS inside uploaded files)
* Filenames and path components (including zip entries)
* Image metadata (EXIF, IPTC) that is displayed anywhere
* SVG files (inline scripts, event attributes, \<script> in SVG)
* Uploaded HTML or markdown files rendered later

### JSON / XML / API payloads

* JSON fields that get injected into pages or into \<script> blocks (e.g., var data = {...})
* JSONP callback names and dynamic script endpoints

### DOM sources (client-side)

* document.location, location.hash, location.search
* document.referrer
* window.name
* postMessage messages (origin and data)
* DOM-retrieved values used directly with innerHTML, outerHTML, insertAdjacentHTML, document.write, or eval
* Values read from file inputs (file names) or drag-and-drop

### HTML attribute contexts

* src, srcset, href (including javascript: or data: URIs)
* style attribute (inline CSS url() or expression() in legacy contexts)

### JavaScript/JS string contexts

* Inside single/double-quoted JS strings
* Inside template literals/backticks
* Inside regular expressions or as part of new Function() / eval inputs
* JSON serialized into \<script type="application/json"> or inline config objects

### CSS & style contexts

* Inline \<style> blocks populated from user data
* style attributes with url() or expression() (legacy)
* Class names or IDs used in CSS selectors (when built dynamically)

These all are the attack vectors from where we can insert our malicious/crafted payload to execute and demonstrate javascript execution.
