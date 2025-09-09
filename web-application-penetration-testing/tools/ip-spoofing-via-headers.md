# IP Spoofing via headers

| Header Name                 | Purpose / Behavior                                                | Notes                                                              |
| --------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------ |
| `X-Forwarded-For`           | Indicates original client IP if it is behind a proxy.             | Most common; often trusted by proxies, use `127.0.0.1`, or real IP |
| `X-Real-IP`                 | Original IP of client                                             | Used by some nginx setups                                          |
| `Client-IP`                 | Custom header; sometimes used to pass real IP                     | Unofficial, but apps may log/trust it                              |
| `Forwarded`                 | Standardized version of `X-Forwarded-For` (e.g., `for=127.0.0.1`) | RFC 7239 compliant                                                 |
| `True-Client-IP`            | Used by some CDN/WAFs like Akamai to pass the real client IP      | May be honored by edge servers                                     |
| `X-Client-IP`               | Another unofficial variant                                        | May be seen in misconfigured setups                                |
| `X-Remote-IP`               | May be used internally in some frameworks                         | Less common                                                        |
| `X-Remote-Addr`             | Variant; treated as IP in some custom environments                | Rare, but still seen                                               |
| `X-Host`                    | Override the `Host` header (used in SSRF host poisoning)          | Can trigger SSRF or virtual host misrouting                        |
| `X-Originating-IP`          | Used by some mail or proxy systems to indicate origin             | May be logged or trusted                                           |
| `X-Forwarded-Host`          | Spoof host in some SSRF or proxy misconfigs                       | Sometimes leads to open redirect or SSRF                           |
| `X-Original-URL`            | IIS/ASP.NET may use this to route internally                      | Can be used to bypass URL restrictions                             |
| `X-Rewrite-URL`             | Like above; used by proxies/web apps to rewrite URL paths         | Exploitable in certain middleware setups                           |
| `X-Custom-IP-Authorization` | **Custom header** seen in some legacy apps to trust specific IPs  | If value is `127.0.0.1`, may grant local-only access               |
| `X-Forwarded-User`          | Spoof authenticated username in some internal systems             | If the backend trusts this, you may impersonate users              |
| `X-Authenticated-User`      | Similar to above; can spoof a logged-in user                      | Dangerous if unchecked                                             |
| `X-Authenticated-Email`     | Spoof an email used for login                                     | Seen in SSO bypasses                                               |
| `Authorization`             | Send Basic, Bearer, or custom tokens                              | Used for direct token manipulation (e.g., `Bearer null`, etc.)     |

## 1. X-Forwarded-For (XFF)

### **Who adds it ?**&#x20;

* Trusted proxies or Load balancers _(like Nginx, Cloudflare)_

This HTTP header tells the server the original IP address of the user - even if they are behind one or more proxies.&#x20;

### **Why it's used:**

When a user connects through a proxy or load balancer, the server only sees the proxy’s IP.\
But sometimes we **need to know who the real user is**, especially for:

* Logging
* Geo-blocking
* Rate-limiting

### Exploitation

```
X-Forwarded-For: 127.0.0.1
```

If the app **trusts this header without checking**, it may think you're coming from **localhost** and **give you access** meant only for internal users.

## 2. X-Custom-IP-Authorization

### Who adds it ?&#x20;

* Custom or legacy applications or proxies

### **What it does:**

A **custom header** used by some older or legacy apps to allow access for **local/internal IPs**.

### **Why it exists:**

Developers sometimes let users from **trusted local IPs** access restricted parts of the app by checking this header.

### Exploitation

If the app sees:

```
X-Custom-IP-Authorization: 127.0.0.1
```

It may **think you are a trusted local user** and let you access things without normal authentication.

## 3. X-Original-URL

### What it does ?

This header holds the **original URL path** requested by the client before any proxy or load balancer rewrites or redirects it. rewrite is done for `routing`, `security`, or `caching purposes`.&#x20;

### Why it exists ?&#x20;

When a proxy or load balancer modifies the URL (for example, for routing or rewriting), the backend app may lose the original path info.\
`X-Original-URL` lets the backend **know what URL the client actually requested**.

### Who adds it ?&#x20;

Trusted proxies or load balancers (like IIS ARR, Nginx) add this header when they rewrite URLs.&#x20;

### Exploitation

```
X-Original-URL: /admin
```

If the app trusts this header blindly, you might access restricted URLs by tricking the app into using your supplied URL instead of the rewritten one.

**E.g.,**&#x20;

```
GET /admin HTTP/1.1
Host: target.com
X-Original-URL: /public
```

This may serve public content despite `/admin` in the path.

## 4. X-Rewrite URL \[IIS Specific]

### **What it does:**

This header shows the **URL path after the proxy has rewritten it**, especially in systems like **IIS (Internet Information Services)**.

### **Why it exists:**

* Used mainly by **Microsoft IIS reverse proxy setups**.
* Helps the backend app understand **which URL** the proxy actually processed **after rewriting**.

### Who adds it ?&#x20;

**IIS Reverse Proxy** or **Application Request Routing (ARR)** adds this automatically when URL rewriting rules are in place.

### Exploitation \[IIS specific]

```
GET /public HTTP/1.1
Host: target.com
X-Rewrite-URL: /admin
```

If the app routes based on `X-Rewrite-URL`, it may let you access `/admin` even though you requested `/public`.

## 5. X-Forwarded-Host

### **What it does:**

Tells the backend server what the **original Host header** was **before** being changed by a proxy or load balancer.

### Why it exists:

When a **reverse proxy** (like nginx, AWS ELB, Cloudflare, etc.) sits in front of an app, it may change the `Host` header to something internal for security and load balancing purpose. \
This header allows the original host to still be passed to the backend.

### Example

Client sends:

```
GET / HTTP/1.1
Host: app.target.com
```

But a proxy rewrites it to:

```
Host: internal-server
X-Forwarded-Host: app.target.com
```

So the backend knows what host the **client originally requested**.

### Who adds it ?&#x20;

Reverse proxies like nginx, Apache, Cloudflare or load balancers (if configured to).&#x20;

### Exploitation

Try spoofing it like this:

```
X-Forwarded-Host: admin.target.com
```

If the app uses this to build links, redirect, or validate access based on the host, it could:

* Leak info
* Cause open redirect
* Trigger host-based bypasses

## 6. X-Forwarded-Scheme

### What it does ?&#x20;

Tells the backend whether the **original request** from the client was sent over `http` or `https`.

### Why it's used ?&#x20;

When a **reverse proxy** (like nginx, Cloudflare, or AWS Load Balancer) sits in front of a server, the **client connects to the proxy**, and the proxy connects to the backend.

The proxy might connect to the backend using plain `http`, even if the client used `https`.

So the backend won’t know whether the original request was secure (`https`) or not.

To fix that, the proxy adds:

```
X-Forwarded-Scheme: https
```

### Who adds it ?&#x20;

* _Reverse proxies (if configured) to save time and CPU resources_

### Example&#x20;

Client connects over HTTPS to:

```
https://secure.example.com
```

But proxy forwards request to backend over HTTP:

```
GET / HTTP/1.1
Host: backend.local
X-Forwarded-Scheme: https
```

Now the backend knows:\
🔒 "Okay, even though this came over HTTP, the user originally used **HTTPS**."

### Exploitation

In rare cases, setting:

```
X-Forwarded-Scheme: http
```

may trick misconfigured apps into:

* Generating insecure URLs
* Allowing redirects to HTTP
* Downgrading security headers

## 7. X-HTTP Method Override

### What it does ?

Allows a client to **override the HTTP method** (like `GET`, `POST`, etc.) using a header — even if the actual request method is something else (usually `POST`).

### Why it exists ?&#x20;

Some **proxies, firewalls, or browsers**:

* **Block or don’t support** HTTP methods like `PUT`, `DELETE`, or `PATCH`.

So, developers created a workaround:

* **Send a `POST` request**, but add this header:

```
X-HTTP-Method-Override: DELETE
```

The backend will then **treat the request as a `DELETE`**, not `POST`.

### Who adds this header ?&#x20;

The **X-HTTP-Method-Override** header is typically used in scenarios where certain HTTP methods—like `DELETE` or `PUT`—are restricted or unsupported by the client, server, or intermediary (e.g., firewalls or proxies). In such cases, developers implement logic (often via JavaScript or backend code) to send a `POST` request while including the `X-HTTP-Method-Override` header to indicate the intended HTTP method (e.g., `DELETE`). The server then interprets the request accordingly and performs the appropriate action.

### Exploitation

```
POST /resource HTTP/1.1
X-HTTP-Method-Override: PUT
```

and see if the app acts like a `PUT` request.

## 8. X-Client-IP/X-Real-IP

Same as the `X-Forwarded-For` header but it only specifies a single client IP as compared to `X-Forwarded-For` which allows multiple client as well as proxy IPs.&#x20;
