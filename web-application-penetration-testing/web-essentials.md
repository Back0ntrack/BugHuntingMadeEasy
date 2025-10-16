---
icon: stairs
---

# Web Essentials

## DNS

**DNS (Domain Name System)** is a system that translates human-readable domain names (e.g., `google.com`) into IP addresses (e.g., `142.250.190.78`), allowing computers to locate and communicate with each other over the internet.

### Domain Name Structure

<figure><img src="../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

#### TLD (Top Level Domain)

A **Top-Level Domain (TLD)** is the rightmost part of a domain name (e.g., `.com` in `tryhackme.com`). TLDs are categorized into **gTLDs (Generic TLDs)**, originally indicating purpose (`.com` for commercial, `.org` for organizations, `.edu` for education) and **ccTLDs (Country Code TLDs)**, denoting geographic location (`.ca` for Canada, `.co.uk` for the UK). Due to demand, many new gTLDs like `.online`, `.club`, and `.biz` have emerged.

#### SLD (Second-Level Domain)

A **Second-Level Domain (SLD)** is the part of a domain name directly to the left of the TLD (e.g., `tryhackme` in `tryhackme.com`). It uniquely identifies a website within a TLD and is chosen by the domain owner.

#### Subdomain

A **Subdomain** is a prefix added to a domain name to organize or navigate different sections of a website (e.g., `blog.example.com`, where `blog` is the subdomain of `example.com`). It allows the creation of separate web addresses under the same domain.

### How DNS Works

<figure><img src="../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

**1. Your Computer Asks for Directions (DNS Query):** When you type a domain name, your computer first checks its local cache for the matching IP address. If it's not found, it contacts a DNS resolver—typically provided by your Internet Service Provider.

**2. The DNS Resolver Checks its Map (Recursive Lookup):** The resolver first checks its own cache. If the IP address isn't there, it contacts a root name server to start the search.

**3. Root Name Server Points the Way:** While the root server doesn't store the IP address, it guides the resolver to the right TLD name server (e.g., `.com`, `.org`).

**4. TLD Name Server Narrows It Down:** The TLD server points the resolver to the authoritative name server responsible for the specific domain.

**5. Authoritative Name Server Delivers the Address:** This server provides the exact IP address for the requested domain.

**6. The DNS Resolver Returns the Information:** The resolver sends the IP address to your computer and saves it temporarily in its cache for quicker future access.

**7. Your Computer Connects:** Using this IP address, your computer connects directly to the website's server, enabling you to access the site.

### The Hosts file

The `hosts` file is a simple text file used to map hostnames to IP addresses, providing a manual method of domain name resolution that bypasses the DNS process.

* **Windows Location:** `C:\\Windows\\System32\\drivers\\etc\\hosts`
* **Linux Location:** `/etc/hosts`

### DNS Server Records

Each domain has a **zone file** stored on its authoritative name server, which contains all the DNS records (A, MX, NS, etc.) for that domain.

|                 |                           |                                                                                                                           |                                                       |
| --------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| **Record Type** | **Full Name**             | **Purpose & Explanation**                                                                                                 | **Example**                                           |
| **A**           | Address Record            | Maps a domain name to an IPv4 address, allowing browsers to locate websites.                                              | `example.com → 192.168.1.1`                           |
| **AAAA**        | IPv6 Address Record       | Similar to an A record, but maps a domain to an IPv6 address instead of IPv4.                                             | `example.com → 2001:db8::1`                           |
| **CNAME**       | Canonical Name Record     | Creates an alias for another domain, useful for pointing multiple domains to the same destination.                        | `blog.example.com → example.com`                      |
| **MX**          | Mail Exchange Record      | Specifies mail servers responsible for handling email for a domain, with priority values.                                 | `example.com → mail.example.com (Priority: 10)`       |
| **NS**          | Name Server Record        | Defines which name servers are authoritative for a domain, managing DNS queries.                                          | `example.com → ns1.example.com, ns2.example.com`      |
| **TXT**         | Text Record               | Stores text-based information, often used for domain verification, SPF records, and security purposes.                    | `example.com → "v=spf1 include:_spf.google.com ~all"` |
| **SOA**         | Start of Authority Record | Contains key administrative details about a DNS zone, such as the primary name server, admin email, and update intervals. | `example.com → Primary NS: ns1.example.com`           |
| **SRV**         | Service Record            | Specifies the location (hostname and port) of specific services (e.g., VoIP, LDAP, SIP) within a domain.                  | `_sip._tcp.example.com → sip.example.com:5060`        |
| **PTR**         | Pointer Record            | Used for **reverse DNS lookups**, mapping an IP address back to a domain name, often for email validation and security.   | `192.168.1.1 → example.com`                           |

When we query DNS records using the `dig` command, the results are provided by the **authoritative name server**, which stores and manages all DNS information for the specified domain.

## The HTTP Protocol

HTTP is the core communication protocol used to access the World Wide Web.

{% hint style="info" %}
Our browsers usually first look up records in the local '`/etc/hosts`' file, and if the requested domain does not exist within it, then they would contact other DNS servers. We can use the '`/etc/hosts`' to manually add records to for DNS resolution, by adding the IP followed by the domain name.
{% endhint %}

### HTTP Requests

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

The first line of every HTTP request consist of three items separated by spaces:

1. A verb indicating HTTP methods.
2. The requested URL.
3. The HTTP Version being used.

**Other point of interests:**

1. The `Referrer` header used to indicate URL from which request is originated.
2. The `User-Agent` header is used to provide information about the browser.
3. The `Host` header specifies the hostname that appeared in full URL being accessed. This is necessary when multiple websites are hosted on the same server.
4. The `Cookie` header.

#### HTTP Methods

1. **`GET`:** It is designed to retrieve resources. It can be used to send parameters to requested resources in URL query string.
2. **`POST`:** The POST method is designed to perform actions. With this method the request parameter can be sent in the message body.

> Because the POST method is designed for performing actions, if a user click’s the back button, the browser warns the user that to display the page it must resend information.

1. **`HEAD`:** Returns headers without message body.
2. **`TRACE`:** The server should return in the response body the exact content of the requested message it received.
3. **`OPTIONS`:** Returns HTTP methods accepted by the Web Server.
4. **`PUT`:** It attempts to upload specific resources to server.

### HTTP Responses

<figure><img src="../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

The first line of every HTTP response consist of three items, separated by spaces:

1. The HTTP Version being used.
2. The numeric status code indicating result of the request.
3. A textual “reason phrase” further describing the status of the response.

**Other point of interests:**

1. The `Server` header contains a banner indicating web server software being used.
2. The `Set-Cookie` header issues the browser a further cookie.
3. The `Pragma` header instructs the browser not to store response in its cache.
4. The `Content-type` Header indicates the type of content.

### HTTP Headers

#### ⇒ **General Headers**

1. `Connection` tells the other end that whether it should close TCP connection or keep it open after HTTP transmission has completed.
2. `Content-Encoding` specifies what kind of encoding being used for content in message body such as gzip which enables faster transmission.
3. `Content-Length` specifies the length of the message body.
4. `Content-Type`: Specifies the type of content contained in the message body.
5. `Transfer-Encoding` specifies any encoding that was performed on message body.

#### **⇒ Request Headers**

1. `Accept` tells the server what kind of content the client is willing to accept such as image types, office document formats.
2. `Accept-Encoding` tells the server what kind of content-encoding the client is willing to accept.
3. `Authorization` submit credentials to server for one of built-in HTTP authentication types.
4. `Cookie`
5. `Host` specifies the hostname that appeared in the full URL being requested.
6. `If-Modified-Since` specifies when the browser last received the requested resource. If the resources has not been changed since that time the server instruct client to use its cached resources.
7. `If-None-Match` specifies an entity tag, which is an identifier denoting content of the message body.
8. `Origin` is used in cross-domain ajax requests to indicate domain from which request originated.
9. `Referrer` specifies the URL from which current request originated.
10. `User-Agent` provides information about the browser.

#### **⇒ Response Headers**

1. `Access-Control-Allow-Origin`: indicates whether the resource can be retrieved via cross-domain ajax requests.
2. `Cache-Control` passes caching directives to browser. (e.g., no-cache)
3. `Expires` tells the browser for how long the content of message body are valid.
4. `Location` is used in redirection responses to specify the target of redirect.
5. `Pragma`
6. `Server` provides information about the server.
7. `Set-Cookie` used to set the cookie value.
8. `WWW-Authenticate` provide detail on type of authentication the server supports.
9. `X-Frame-Options` indicate whether and how the current response may be loaded within a browser frame.

→ **Options for Set-Cookie**

1. `expires` sets a date until which cookie is valid.
2. `domain` specifies domain for which cookie is valid.
3. `path` specifies the URL path for which cookie is valid.
4. `secure` The cookie will be submitted only in HTTPS.
5. `HTTPOnly` cookie cannot be accessed directly by JS.

### Status Codes

**⇒ Five Key groups**

1. `1xx`: Informational
2. `2xx`: The request was successful.
3. `3xx`: The client is redirected to a different source.
4. `4xx`: The request contain an error of some kind.
5. `5xx`: The server encountered an error fulfilling the request.

#### ⇒ Commonly Encountered Status Codes

* `100 Continue`
* `200 OK`
* `201 Created` (Successful PUT request)
* `301 Moved Permanently` (Redirects to `Location`)
* `302 Found` (Temporary redirects to `Location`)
* `304 Not Modified` (instructs the browser to use its cached copy of requested resource. The server uses `If-Modified-Since` & `If-None-Match` request header to determine whether the client has latest version.
* `400 Bad Request`
* `401 Unauthorized` (The `WWW-Authenticate` header contains the details on the type of authentication supported.
* `403 Forbidden` (Access post authorization)
* `404 Not Found`
* `405 Method Not Allowed`
* `500 Internal Server Error`
* `503 Service Unavailable`

### Making Web Requests with cURL

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

## Web Servers

A **web server** is software that listens for HTTP(S) requests from users (browsers, APIs, or tools) and responds with content like webpages, JSON, or files.

Think of it as a **messenger** between:

* **Clients (Users/Browsers/Attackers)**
* **Backend Code (PHP, Python, Node.js, Java, etc.)**
* **Database (MySQL, PostgreSQL, MongoDB, etc.)**
* **Static Files (HTML, CSS, JS, Images, PDFs, etc.)**

### Common Web Servers

| Web Server | OS             | Common Use                                  | Pros                          | Cons                                   |
| ---------- | -------------- | ------------------------------------------- | ----------------------------- | -------------------------------------- |
| Apache     | Linux, Windows | Popular, PHP-friendly                       | Flexible, .htaccess support   | Slower than Nginx for high traffic     |
| Nginx      | Linux          | Reverse proxy, high-performance sites       | Fast, scalable                | Harder to configure                    |
| IIS        | Windows        | Enterprise, [ASP.NET](http://asp.net/) apps | Deep integration with Windows | Proprietary, limited community support |
| LiteSpeed  | Linux, Windows | WordPress & PHP hosting                     | Faster than Apache            | Paid versions for full features        |

### Web Server Configuration and Default Directories

Web servers have configuration files that control **how requests are handled, security settings, and directory permissions**.

#### **Apache Configuration**

* **Main Config File:**
  * `/etc/apache2/apache2.conf` (Ubuntu)
  * `/etc/httpd/conf/httpd.conf` (CentOS)
  * `C:\\Program Files\\Apache Group\\Apache2\\conf\\` (Windows)
* **Virtual Host Config Management:** `/etc/apache2/sites-enabled`
* **Modules Configs:** `/etc/apache2/mods-enabled/`
* **Ports configuration:** `/etc/apache2/ports.conf`

**Key Directives:**

* `DocumentRoot "/var/www/html"` → Sets the default website directory.
* `AllowOverride`: Controls if `.htaccess` can override server settings.
* `DirectoryIndex index.html index.php` → Defines default pages.
* `Options Indexes` → Enables **directory listing** (potential vulnerability!).
* `ServerTokens Full` → Shows **detailed server version** in HTTP headers (bad for security).
* `ErrorDocument 404 /error.html` → Defines **custom error pages**.

The **.htaccess** (Hypertext Access) file is a configuration file used on **Apache web servers** to control server behavior at the directory level. It allows settings like URL redirection, authentication, access control, MIME types, and mod\_rewrite rules without modifying the main server configuration.

Attackers often target **.htaccess** for **malicious redirects, privilege escalation, or backdoors** in compromised websites.

Common locations include:

* **Website Root:** `/var/www/html/.htaccess` (Linux) or `C:\\xampp\\htdocs\\.htaccess` (Windows with XAMPP).
* **CMS Directories:** `public_html/.htaccess` (for cPanel-hosted websites), `wp-content/uploads/.htaccess` (in WordPress), or inside Joomla and Drupal installations.

#### **Nginx Configuration**

* **Main Config File:** `/etc/nginx/nginx.conf`
* **Virtual Hosts:** `/etc/nginx/sites-available/`
* **Error Logs:** `/var/log/nginx/error.log`

**Key Directives:**

* `root /var/www/html;` → Sets the root directory.
* `index index.html index.php;` → Defines default files.
* `server_name example.com;` → Defines which domain this block applies to.
* `location /admin { deny all; }` → Restricts access to `/admin`.
* `autoindex on;`: Enables directory listing.
* `location / {}`: Controls URL path behavior.

#### **IIS (Windows Web Server)**

Configuration file: `C:\\Windows\\System32\\inetsrv\\config\\applicationHost.config`

**Key Misconfigurations:**

* `web.config` may leak internal paths.
* Directory listing can be enabled via IIS Manager.
* Anonymous authentication may expose internal applications.

When no **index.html** or **index.php** exists, and **directory listing** is enabled, the server shows all files in the folder.

### Server-Side Languages

Server-side languages are programming languages used to develop the backend (server-side) of web applications. These languages handle business logic, database interactions, authentication, and dynamic content generation before sending the final response to the client.

**Languages: PHP, Python (Django, Flask), JavaScript (Node.js), Java (Spring, JSP, Servlets), Ruby (Ruby on Rails), C# (.NET Framework,** [**ASP.NET**](http://asp.net)**), Perl etc**

Common attack vectors in server-side languages include **SQL Injection (SQLi)** to manipulate databases, **Command Injection** to execute OS commands, **Server-Side Template Injection (SSTI)** to run arbitrary code via template engines, and **Remote Code Execution (RCE)** due to insecure deserialization or poor input validation. **Server-Side Request Forgery (SSRF)** can force the server to access internal resources, while **Path Traversal** allows access to restricted files. **Weak session management** leads to **session hijacking**, and **file upload vulnerabilities** enable attackers to deploy web shells. **Improper error handling** can leak sensitive information, aiding further exploitation.

### Interaction with Databases

#### **Web Application & Database Interaction**

Web applications interact with databases using **backend server-side languages** (e.g., PHP, Python, Node.js) and **Database Management Systems (DBMS)** (e.g., MySQL, PostgreSQL, MSSQL, MongoDB). The interaction typically follows:

1. **User Input** → Data is entered via forms or API requests.
2. **Query Execution** → The backend processes input and sends SQL/NoSQL queries to the database.
3. **Response Handling** → Data is fetched, processed, and displayed to the user.

## Security Headers

HTTP security headers help improve the overall security of the web application by providing mitigations against attack like XSS, clickjacking, and others.

{% hint style="info" %}
_All headers given below are response headers._&#x20;
{% endhint %}

### Strict-Transport Security

The **HTTP Strict Transport Security Header (HSTS)** is set on the server and enforces the use of encrypted HTTPS connections instead of plain-text HTTP communication.

```html
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

This informs visiting web browsers that the site along with all its subdomains only communicates over SSL/TLS and the browser should only access it over HTTPS for the next two years (the `max-age` value in seconds). The `preload` directive indicates that the site is present on a global list of HTTPS-only sites. The purpose of preloading is to speed up page loads.

### Content-Security Policy&#x20;

#### 1️⃣ CSP Overview

**CSP (Content-Security-Policy)** is an **HTTP response header** (or a `<meta>` tag) that tells the browser **what resources are allowed to load and execute** on a web page.

It mainly protects against:

* **Cross-Site Scripting (XSS)**
* **Data Injection attacks**
* **Clickjacking (via frame-ancestors)**
* **Mixed content loading**

#### 2️⃣ Basic Concept — How It Works

When the browser loads a page with this header:

```http
Content-Security-Policy: default-src 'self'
```

It means:

> “Only load resources (scripts, images, styles, etc.) from the same origin as this page.”

If the page tries to load:

* A script from `https://evil.com/script.js` → ❌ blocked
* A style from `https://cdn.something.com/style.css` → ❌ blocked
* A script from same domain → ✅ allowed

#### 3️⃣ Syntax Structure

A **CSP header** is made of **directives** and **sources**:

```http
Content-Security-Policy: directive source source ...
```

Example:

```http
Content-Security-Policy: script-src 'self' https://cdn.jsdelivr.net
```

→ Only allow JS from your own site and from `cdn.jsdelivr.net`.

#### 4️⃣ Default Directive: `default-src`

`default-src` is the **fallback rule** for all types of resources if a specific one isn’t defined.

Example:

```http
Content-Security-Policy: default-src 'self'
```

→ All resources (scripts, images, CSS, fonts, etc.) must come from the same domain.

You can override it with specific rules:

{% code overflow="wrap" %}
```http
Content-Security-Policy: default-src 'self'; img-src *; style-src 'self' 'unsafe-inline';
```
{% endcode %}

Here:

* Scripts follow `default-src` (`'self'`)
* Images can come from anywhere (`*`)
* Styles can come from `'self'` and inline styles are allowed

#### 5️⃣ The Main Anti-XSS Directives

Let’s now focus on the **four directives** that matter most for **XSS prevention**:

#### **a. `script-src`**

Controls **where scripts can be loaded or executed from**.

Examples:

```http
Content-Security-Policy: script-src 'self'
```

✅ Only allow scripts from your own domain.\
❌ Block inline `<script>alert(1)</script>`\
❌ Block external scripts from any other domain.

**Common Values**

| Value             | Meaning                                    | Example                    |
| ----------------- | ------------------------------------------ | -------------------------- |
| `'self'`          | Same origin only                           | `'self'`                   |
| `'unsafe-inline'` | Allows inline `<script>` or `onclick=`     | `'unsafe-inline'`          |
| `'unsafe-eval'`   | Allows use of `eval()` or `new Function()` | `'unsafe-eval'`            |
| `'none'`          | No scripts at all                          | `'none'`                   |
| `https://cdn.com` | Allows that external domain                | `https://cdn.jsdelivr.net` |
| `data:`           | Allows inline **base64/data URI** scripts  | `data:`                    |

**❗Why “unsafe-inline” is dangerous**

```html
<script>alert(1)</script>
<button onclick="alert(2)">Click</button>
```

If your CSP contains `'unsafe-inline'`, both above will run → making **XSS possible**.

If `'unsafe-inline'` is **not** allowed, browser blocks them → XSS prevented.

#### The \`data:\` value&#x20;

`data:` allows **resources to be loaded from inline data URIs**.\
It looks like this:

```html
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUA...">
```

This is a legitimate way to embed small images **directly into HTML** without fetching from a server.

✅ Example (Safe Use):

```http
Content-Security-Policy: img-src 'self' data:
```

→ Allows images like above.

***

❌ But for **scripts or styles**, `data:` can be dangerous.

Because:

```html
<script src="data:text/javascript,alert(1)"></script>
```

If your CSP says:

```http
Content-Security-Policy: script-src 'self' data:
```

Then the browser would **execute the base64 or text/javascript data directly** — which can allow **inline XSS bypass**.

So **never allow `data:` for script-src or style-src**.

### X-Content-Type-Options

The **X-Content-Type-Options** header is a security feature that stops web browsers from guessing the type of a file (MIME-type sniffing). This helps prevent attacks where a file (like an image or text file) is wrongly interpreted as executable code, leading to **Cross-Site Scripting (XSS) or malware execution**. **Example:**

```html
X-Content-Type-Options: nosniff
```

This tells the browser to **only use the file type declared by the server** (e.g., a `.jpg` file will always be treated as an image, not as a script).

Without this header, an attacker could upload a malicious script disguised as a harmless file (e.g., a `.jpg` or `.txt` file) and trick the browser running it as JavaScript, leading to code execution on the victim’s browser.

#### Referrer Policy

The Referrer-Policy header controls how much referrer information (the URL of the previous page) is sent when navigating to a new site. This helps protect **user privacy** and **sensitive data** from being leaked in URLs.

```html
Referrer-Policy: no-referrer
```

This means the browser **won’t send any referrer information** when navigating to another page.

**Common Policies & Their Effects:**

* **`no-referrer`** → Sends no referrer data at all.
* **`no-referrer-when-downgrade`** (default in some browsers) → Sends referrer only to HTTPS sites, not HTTP.
* **`same-origin`** → Sends referrer only within the same website.
* **`strict-origin`** → Sends only the origin (domain) but only to HTTPS sites.

A strict **Referrer-Policy** helps **prevent information disclosure**

## Web Application Architecture

### Frontend & Backend Separation

Web applications are divided into two primary components:

* **Frontend (Client-side)** → Runs in the user's browser (HTML, CSS, JavaScript).
* **Backend (Server-side)** → Processes logic, authentication, and interacts with databases.

#### **🔹 Pentesting Focus**

1️⃣ **Client-Side Validation Bypass:**

* Many apps perform input validation on the frontend using JavaScript. Attackers can **bypass this validation** using browser **DevTools or Burp Suite**.
* Example: A form requiring a minimum password length of 8 characters, but the request can be modified to use **"123"** instead.

2️⃣ **Insecure Direct API Calls:**

* If APIs are exposed directly to the frontend, an attacker can **manipulate API requests** using tools like **Postman or Burp Suite**.
* Example: A price modification API that should only be accessible to admins, but a normal user can access it by **changing the API request in Burp**.

3️⃣ **Cross-Origin Resource Sharing (CORS) Exploitation:**

* **Weak CORS policies** allow unauthorized domains to request sensitive data.
* Example: If an API allows `Access-Control-Allow-Origin: *`, any external site can steal user data by making **cross-origin requests**.

### **Client-Side vs. Server-Side Logic**

Client-side logic **runs on the user’s machine**, while **server-side logic** executes on the backend, enforcing business rules and security.

#### **🔹 Pentesting Focus**

1️⃣ **Hidden Business Logic Attacks:**

* If critical security decisions (e.g., access control) are handled on the **client-side**, attackers can modify requests to **escalate privileges**.
* Example: An attacker modifies a POST request from `role=user` to `role=admin`, gaining admin access.

2️⃣ **Session Management Attacks:**

* Poor session management can allow **Session Hijacking** (stealing cookies) or **Session Fixation** (forcing a victim to use a known session).
* Example: If the session cookie is **not marked as `HttpOnly`**, JavaScript-based attacks like **XSS** can steal it.

3️⃣ **Local Storage/JavaScript Manipulation:**

* Sensitive data stored in `localStorage` or `sessionStorage` can be accessed by JavaScript, making it vulnerable to **XSS attacks**.
* Example: If an API key or authentication token is stored in `localStorage`, an attacker injecting a malicious script can steal it.

### **REST APIs & Web Application Interaction**

Web apps communicate with backend servers using **REST APIs**, which return **JSON or XML** responses.

#### **🔹 Pentesting Focus**

1️⃣ **Broken Authentication & Authorization:**

* If authentication **relies only on API tokens** without validating user roles, an attacker can **reuse stolen tokens**.
* Example: Changing the `user_id` in API requests to another user’s ID to access their private data (**IDOR attack**).

2️⃣ **Rate-Limiting & Brute-Force Attacks:**

* APIs without rate limiting allow attackers to brute-force credentials.
* Example: Trying thousands of passwords against a login API without getting blocked.

3️⃣ **Sensitive Data Exposure in API Responses:**

* Some APIs return unnecessary sensitive data.
* Example: A response containing **password hashes, credit card details, or internal server errors (stack traces)**.

4️⃣ **Testing for Injection Attacks in APIs:**

* APIs may be vulnerable to **SQL Injection, XML External Entity (XXE), Server-Side Request Forgery (SSRF)**, or **Remote Code Execution (RCE)** if input is not sanitized.
* Example: An API endpoint using `user_id=1` can be tested for SQLi using `user_id=1' OR '1'='1`.

5️⃣ **Testing API Tokens & Secrets Exposure:**

* API keys, JWT tokens, and OAuth tokens should not be exposed in **frontend JavaScript files**.
* Example: Searching `apiKey` in JavaScript files using `grep` or `Burp Suite`.

### Authentication in Web Applications

#### JSON Web Token (JWT)

* A **self-contained token** used for authentication and authorization in web apps & APIs.
* Encodes user data in a **compact, URL-safe format** and is signed using HMAC or RSA.
* JWTs are **stateless**, meaning they don’t require server-side storage.
* Used in **OAuth2.0, API authentication, and single sign-on (SSO)**.

**Example:**

```html
header:
{
  "alg" : "HS256",
  "typ" : "JWT"
}

Payload:
{
  "id" : 123456789,
  "name" : "Joseph"
}

Secret: GeeksForGeeks
```

**Output**

```html
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MTIzNDU2Nzg5LCJuYW1lIjoiSm9zZXBoIn0.OpOSSw7e485LOP5PrzScxHb7SR6sAOMRckfFwi4rp7o
```

#### OAuth (Authorization Framework)

* OAuth **delegates authentication** to a third-party provider (Google, Facebook, etc.).
* Used for **SSO (Single Sign-On)** to avoid password sharing between services.

**OAuth Flow:**

1. User logs in → Requests access from OAuth provider.
2. Provider redirects user to **grant access**.
3. If authorized, provider sends an **access token**.
4. Application uses the token to authenticate API requests.

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

**Common Attack Vectors:**

* **Token Theft** (stealing OAuth tokens via XSS, phishing).
* **Improper Redirects** (attacker manipulates the redirect URL to steal tokens).
* **OAuth Misconfiguration** (weak token validation, missing expiry).

#### **Session IDs (Cookie-Based Authentication)**

* A **server stores a session ID** when a user logs in and assigns it to the client via cookies.
* Session persistence is managed **server-side**.

🔹 **Common Attack Vectors:**

* **Session Hijacking** (stealing session IDs via MITM, XSS).
* **Session Fixation** (forcing a victim to use a pre-set session ID).

### Secure Cookies

* **`Secure` Flag** → Ensures cookies are sent only over HTTPS.
* **`HttpOnly` Flag** → Prevents JavaScript from accessing cookies (mitigates XSS-based session theft).
* **`SameSite` Flag** → Protects against CSRF attacks by restricting cross-site cookie usage.

### How Login System Works

#### Basic Authentication (HTTP Auth)

* Uses **base64-encoded credentials** sent with every request.

**Example:**

```html

Authorization: Basic YWRtaW46cGFzc3dvcmQ=
```

**Weakness** → Credentials are **easily intercepted** if not used over HTTPS.

#### **Token-Based Authentication**

* Uses a **unique token (JWT, OAuth token)** instead of passwords for login.
* No need for session storage—tokens are **stateless** and included in requests.

#### **OAuth & SAML (Single Sign-On)**

* OAuth → Delegates authentication (Google, Facebook login).
* SAML → Uses **XML-based authentication tokens** for enterprise applications.

### File Upload Mechanisms

#### **Common Server Handling Methods**

* **Direct Storage** → File is stored **on the server** in a dedicated directory (`uploads/`).
* **Database Storage** → File is **encoded (Base64)** and stored in a database.
* **Cloud Storage** → Uploaded files are stored in **Amazon S3, Google Cloud, etc.**

#### MIME Types and Content Validation

MIME types identify the **type of file being uploaded** (e.g., image, document, script).

#### **🔹 MIME Type Examples**

* `image/png` → PNG image
* `application/pdf` → PDF document
* `text/html` → HTML file
* `application/x-php` → PHP script

#### **MIME Typed & Content Validation Bypasses**

* **MIME Spoofing:** Changing file headers to **bypass validation**.
  * Example: Uploading a `.php` file with a MIME type of `image/png`.
* **Double Extension Trick:** Renaming `shell.php` to `shell.php.jpg` to evade blacklists.
* **Null Byte Injection:** Uploading a file as `shell.php%00.jpg`—some older PHP versions **ignore everything after `%00`**, treating it as `shell.php`

### **What is Authorization?**

🔸 Authorization **determines what actions a user can perform** after authentication.

🔸 If authentication is **"Who are you?"**, authorization is **"What are you allowed to do?"**.

#### **🔹 Types of Authorization Models**

1️⃣ **Role-Based Access Control (RBAC)** → Users are assigned **roles** (Admin, Editor, User) with specific permissions.

2️⃣ **Discretionary Access Control (DAC)** → The **owner** of a resource decides who can access it.

3️⃣ **Mandatory Access Control (MAC)** → Access is controlled **strictly by policies** (e.g., military security levels).

#### **🔹 Common Authorization Flaws**

✅ **IDOR (Insecure Direct Object Reference)**

* Directly accessing restricted resources by modifying URLs.
*   **Example:**

    ```
    <http://example.com/user/profile?id=123>
    <http://example.com/user/profile?id=124>  (View another user's profile)
    ```

✅ **Horizontal Privilege Escalation**

* Normal users access **other users'** data (e.g., viewing other people's orders).
* **Test Case:** Change `UserID=10` to `UserID=11`.

✅ **Vertical Privilege Escalation**

* Normal users **gain admin privileges**.
* **Example:** Changing `user_role=member` to `user_role=admin` in a request.

### **What is Session Management?**

🔸 A session **tracks a user’s state** across requests after login.

🔸 Usually done using **Session IDs or Tokens (JWT, OAuth, etc.)**.

#### **🔹 Common Session Management Vulnerabilities**

✅ **Session Hijacking**

* Attacker steals a **valid session token** and takes over the session.
* **Test Case:**
  * Log in, copy `session_id`, log out, **reuse the session ID**.

✅ **Session Fixation**

* Attacker sets a predefined **session ID** for a victim.
* **Test Case:**
  *   Try logging in with a **fixed session ID**:

      ```
      CopyEdit
      Set-Cookie: sessionid=ABC123;
      ```

✅ **No Session Expiry**

* Sessions remain **active indefinitely**, leading to **long-term risks**.
* **Test Case:**
  * Log in, **wait 1 hour**, and check if the session is still active.

✅ **Secure & HttpOnly Flags Missing**

* `Secure` flag → **Prevents cookies from being sent over HTTP (only HTTPS)**.
* `HttpOnly` flag → **Prevents JavaScript from accessing cookies (mitigates XSS attacks)**.
* **Test Case:**
  * Check cookies in `Developer Tools → Storage → Cookies`.
  *   **Fix:**

      ```
      Set-Cookie: session=xyz; HttpOnly; Secure; SameSite=Strict
      ```



## Web application on cloud

Nowadays, most web applications are hosted on the cloud, which makes reverse DNS/IP lookups and brute-forcing more difficult.&#x20;

* **Amazon EC2** – Runs the actual web app (usually Nginx/Apache). It is acutally a virtual machine in the cloud.&#x20;
* **Amazon Route 53** – DNS resolver; maps domain names to EC2 IPs or Load Balancers
* **Elastic Load Balancer (ELB)** – Distributes incoming traffic to multiple EC2 instances
* **Amazon S3** – Stores static files (images, scripts, backups, etc.)
* **Amazon RDS** – Managed database service (MySQL, PostgreSQL, etc.)
* **Amazon VPC** – Private network where EC2 and other resources live
* **Security Groups** – Acts as a firewall for EC2 and other services
* **Elastic IP** – Static public IP address for EC2
* **Auto Scaling Groups** – Automatically adds/removes EC2 instances based on load

## What is a Proxy

A **proxy** is something that sits **between you and the internet**, and **forwards your requests** to the destination (like a website or server).

Think of it like a **middleman**:

* You send a request to the proxy
* The proxy sends it to the website
* The website replies to the proxy
* The proxy sends the reply back to you

The website **only sees the proxy’s IP address**, not yours.

### How this connects to `X-Forwarded-For`

When traffic goes through **any proxy**, the **original IP is lost** unless it is **added to headers** like `X-Forwarded-For`.

So:

* If you're behind a **proxy or VPN**, your real IP is hidden
* `X-Forwarded-For` helps the server know who the **original client** was

**E.g., Cloudflare, Squid**

## What is a CDN (Content Delivery Network)

A **CDN** is a network of servers **spread across the world** that deliver website content **faster and more securely**.

CDNs act like **proxy servers**, but with a focus on **performance, availability, and security**.

When you visit a website:

* Instead of connecting directly to the **original server**, you connect to a **CDN server** near you.
* The CDN **caches static content** (like images, CSS, JavaScript).
* It may also **filter or block** malicious traffic (basic WAF protection).

**Flow:**\
You → CDN (proxy) → Real Website

So again — the **real server doesn't see your IP**, it sees the **CDN's IP**, **unless** `X-Forwarded-For` is used.

**E.g., Cloudflare, Akamai, Amazon CloudFront, Fastly**
