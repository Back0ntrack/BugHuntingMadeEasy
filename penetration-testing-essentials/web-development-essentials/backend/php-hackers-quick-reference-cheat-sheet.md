# PHP Hacker's Quick-Reference Cheat-Sheet

When testing a web application with a PHP backend, a hacker's mindset is to identify and exploit weaknesses in how PHP handles sessions, input processing, file operations, and server-side logic.

## Purpose & Hacker Mindset

* **PHP = Server-Side Logic, Often Sloppy**: PHP processes user inputs, manages sessions, and interacts with databases/files. Developers often make assumptions (e.g., trusting client-side validation) or leave defaults insecure.
* **HTML as a Map**: HTML (like your example) reveals input points (forms, uploads), endpoints, and client-side logic, but the real vulnerabilities lie in how PHP handles these on the server.
* **Core Strategy**: Identify all entry points, test for improper handling, and exploit misconfigurations. Tools like Burp Suite, cURL, or browser DevTools are used to manipulate requests and bypass restrictions.

***

## Core Focus Areas for PHP-Based Apps

Below are the key areas to investigate when testing a PHP web application, with a hacker's lens on finding exploitable flaws.

#### 1. Session Management Vulnerabilities

PHP's session handling and custom logic are prime targets for session hijacking or fixation.

* **PHPSESSID (Default PHP Sessions)**:
  * **What It Is**: PHP's built-in session system (session\_start()) uses a random, unique PHPSESSID cookie (e.g., PHPSESSID=abc123def456) to track user sessions. Data is stored server-side, linked to this ID.
  * **Hacker's Check**:
    * **Cannot Decrypt**: As you noted, the PHPSESSID is a random string (typically 26-32 chars, generated securely). It’s not encrypted data but a random identifier, so "decrypting" isn’t relevant—its strength lies in unpredictability.
    * **Hijacking Risk**: If stolen (e.g., via network sniffing on HTTP, XSS stealing cookies, or social engineering like users sharing IDs), attackers can impersonate the user. For example, if User A shares PHPSESSID=abc123def456 with User B, B can set this cookie in DevTools and access A’s session (profile, actions, etc.) without credentials.
    * **Fixation**: Check if the site regenerates session IDs on login (session\_regenerate\_id()). If not, an attacker can set a known PHPSESSID before login, forcing the victim to use it.
    * **Weak Configs**: Test for missing Secure (requires HTTPS) or HttpOnly (blocks JS access) flags on the cookie. Use Burp to inspect cookie attributes. Check session timeout (default \~24 minutes) for long-lived sessions.
    * **Attack**: Sniff cookies (Wireshark on non-HTTPS), steal via XSS (document.cookie), or guess IDs if the server uses weak generation (rare in modern PHP).
* **Custom Session Logic (Like Your Script)**:
  * **What It Is**: Developers sometimes bypass session\_start() and use custom cookies (e.g., session\_cookie=alicew1234 in your script, derived from username + last 4 password chars). Data might be verified against a file (like users.json) or database.
  * **Hacker's Check**:
    * **Predictable Cookies**: As you pointed out, custom logic often uses predictable values (e.g., username-based). If the cookie is guessable (e.g., bob1234 for Bob’s common password), attackers can forge it without stealing.
    * **Logic Flaws**: Test the verification logic. In your script, it checks only the last 4 password chars—weak! Try common endings (e.g., 1234, 0000). If logic skips full credential checks, it’s exploitable.
    * **Data Exposure**: Custom cookies might include sensitive data (e.g., partial password). If HTTP is used, sniff it. If stored client-side (like in JSON or DB with weak permissions), exploit file access vulnerabilities (below).
    * **Attack**: Forge cookies with guessed values, tamper in DevTools, or steal via network/XSS. Check if logout clears cookies (your script doesn’t)—persistent cookies allow prolonged access.

#### 2. Input Handling and Injection Attacks

PHP often processes user inputs (from HTML forms like yours) insecurely, leading to injection vulnerabilities.

* **SQL Injection (SQLi)**:
  * **What to Look For**: Forms (like your username, password fields) or URL parameters (GET in your case, e.g., login.php?username=alicew\&password=secret1234) that feed into database queries.
  * **Hacker's Check**:
    * Test inputs with payloads like ' OR 1=1--, admin'-- to bypass authentication or dump data. Your script uses users.json, so SQLi isn’t relevant, but real apps use MySQL/PostgreSQL.
    * Look for errors revealing DB structure (e.g., MySQL syntax errors). Use tools like SQLMap to automate.
    * Check if inputs are sanitized (PHP’s mysqli\_real\_escape\_string or PDO with prepared statements). If not, inject malicious SQL.
  * **Attack**: Manipulate queries to log in without credentials, extract user data, or escalate to admin.
* **Cross-Site Scripting (XSS)**:
  * **What to Look For**: Inputs reflected in output (e.g., your error message in login.php or greeting). GET parameters in URLs are prime targets.
  * **Hacker's Check**:
    * Test inputs with \<script>alert('xss')\</script>, \<img src=x onerror=alert('xss')>. If reflected without sanitization, it’s exploitable.
    * In your script, test username=\<script>alert('xss')\</script>. If the greeting or error echoes it raw, XSS works.
    * Steal cookies (e.g., document.cookie to get PHPSESSID or custom cookies) or redirect users.
  * **Attack**: Inject scripts to steal sessions, log keys, or deface the site. Persistent XSS (if inputs stored) is worse.
* **Command Injection**:
  * **What to Look For**: Inputs passed to PHP functions like system(), exec(), or shell\_exec() (rare but catastrophic).
  * **Hacker's Check**: Test inputs with ; ls, && whoami. If PHP runs shell commands with user input, you can execute OS commands.
  * **Attack**: Gain remote code execution (RCE) to run arbitrary commands (e.g., rm -rf, upload backdoors).

#### 3. File Handling Vulnerabilities

PHP’s file operations (like your users.json usage) are often insecure.

* **Local File Inclusion (LFI)**:
  * **What to Look For**: Parameters controlling file paths (e.g., include($\_GET\['page'])). Not in your script, but common in PHP.
  * **Hacker's Check**:
    * Test URLs like page=../../etc/passwd or page=php://filter/convert.base64-encode/resource=index.php to read sensitive files or source code.
    * Look for directory traversal (../) vulnerabilities.
  * **Attack**: Read configs, passwords, or PHP source. Escalate to RCE if paired with file uploads.
* **File Upload Vulnerabilities**:
  * **What to Look For**: File upload forms (not in your HTML but noted in your reference).
  * **Hacker's Check**:
    * Test uploading PHP files (e.g., shell.php with \<?php system($\_GET\['cmd']); ?>). Check if extensions are filtered server-side (client-side accept=".jpg" is bypassable).
    * Try double extensions (e.g., shell.php.jpg) or null bytes (shell.php%00.jpg).
  * **Attack**: Upload a web shell for RCE or backdoor access.
* **Insecure File Storage** (Like Your users.json):
  * **What to Look For**: Files like users.json storing sensitive data (plain-text passwords in your case).
  * **Hacker's Check**:
    * Test if file is publicly accessible (e.g., http://site/users.json). If not protected (e.g., via .htaccess), download it.
    * Check file permissions. If writable, attackers might overwrite with malicious data.
    * Your script stores passwords in plain text—huge risk if exposed.
  * **Attack**: Steal all user credentials or tamper with storage.

#### 4. HTML and Client-Side Clues (From Your Reference)

Your HTML example highlights input points that guide server-side testing.

* **Form Structure (Your** register.htm&#x6C;**,** login.ph&#x70;**)**:
  * **GET Method**: Exposes all data in URLs (e.g., login.php?username=alicew\&password=secret1234). Check server logs, browser history, or referrers for leaks. Sniffable on HTTP.
  * **Hidden Fields**: None in your HTML, but always check for type="hidden" with tokens, roles, or IDs. Tamper to escalate privileges (e.g., role=admin).
  * **No Validation**: Your reference notes client-side validation (e.g., maxlength, pattern) is bypassable. Test if PHP assumes client-side checks (your script doesn’t validate, so it’s vulnerable to malformed inputs).
* **Client-Side Scripts**:
  * Your script.js is minimal, but check for hardcoded secrets or endpoints in real apps. If JS accesses cookies, test for XSS to steal PHPSESSID or custom cookies.
  * Inline \<script> in login.php output could be a vector for reflected XSS.
* **Comments and Metadata**:
  * Your HTML has no comments, but always inspect for developer notes (e.g., \<!-- debug.php -->) revealing endpoints or secrets.
  * Check \<meta> tags for CSP misconfigurations allowing XSS.

#### 5. Authentication and Authorization

PHP apps often mishandle auth, leading to bypasses.

* **Weak Credentials Handling**:
  * **What to Look For**: Like your script, check if passwords are stored plain-text (huge risk). Test for weak hashing (e.g., MD5) or no hashing.
  * **Hacker’s Check**: If a DB is used, try SQLi to dump credentials. Your users.json is a goldmine if accessible.
  * **Attack**: Brute-force logins if rate-limiting is absent. Use stolen credentials from leaks.
* **Insecure Direct Object References (IDOR)**:
  * **What to Look For**: Parameters like user\_id=123 or profile=alicew. Not in your script, but common in PHP.
  * **Hacker’s Check**: Change IDs/values (e.g., user\_id=124) to access others’ data.
  * **Attack**: View/edit other users’ profiles or data.
* **No CSRF Protection**:
  * **What to Look For**: Forms without CSRF tokens (your script lacks them).
  * **Hacker’s Check**: Craft malicious forms to submit actions (e.g., change password) as the victim.
  * **Attack**: Trick users into clicking links or visiting malicious sites to perform actions.

#### 6. Configuration and Defaults

PHP’s default settings or misconfigurations are exploitable.

* **PHP Info Exposure**:
  * **What to Look For**: Pages like phpinfo.php exposing server details (version, paths, configs).
  * **Hacker’s Check**: Try http://site/phpinfo.php or scan for common paths. Use info to exploit known PHP vulnerabilities.
  * **Attack**: Target outdated PHP versions (e.g., <7.4) with CVEs.
* **Error Reporting**:
  * **What to Look For**: Verbose errors leaking paths, DB queries, or stack traces.
  * **Hacker’s Check**: Trigger errors (e.g., invalid inputs) and check responses. Your script’s error message is basic but could leak if expanded.
  * **Attack**: Use leaks to craft targeted attacks (SQLi, LFI).
* **Directory Listing**:
  * **What to Look For**: Accessible directories (e.g., http://site/uploads/).
  * **Hacker’s Check**: Try common paths (/config, /backup, /data). Your users.json might be exposed if not protected.
  * **Attack**: Download sensitive files or backups.

#### Final Hacker’s Strategy

* **Enumerate**: Use your HTML insights to map forms, endpoints, and inputs (GET URLs, hidden fields, uploads).
* **Session Focus**: Check for PHPSESSID vs. custom cookies. Test hijacking (steal/forgery), fixation, or weak logic (like your predictable username+last4pass).
* **Injection**: Probe for SQLi, XSS, and command injection in all inputs. Your GET-based forms are prime for XSS and data leaks.
* **File Attacks**: Test file access (users.json), uploads, or LFI. Your JSON storage is a critical risk if exposed.
* **Auth Bypasses**: Try IDOR, CSRF, or weak credential checks. Your plain-text storage is a jackpot.
* **Config Exploits**: Look for phpinfo, errors, or directory listings to escalate attacks.
* **Tools**: Use Burp Suite (intercept/modify requests), SQLMap (SQLi), Nikto (scan for misconfigs), or XSS Hunter (XSS payloads).
* **Mindset**: Assume all client-side controls (like your HTML validation) are bypassable. Test PHP’s server-side logic for trust in client data.
