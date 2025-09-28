# PHP (Hypertext Preprocessor)

It runs on the server side and generates dynamic content that is displayed on a web application. PHP is easy to embed in HTML, and it allows developers to create interactive web pages and handle tasks like database management, form handling, and user authentication.

## Introduction&#x20;

```html
<!DOCTYPE html>
<html>
<body>

<?php
echo "My first PHP script!";
?> 

</body>
</html>
```

{% hint style="info" %}
_PHP code is executed on the server, and the result is returned to the browser as plain HTML._&#x20;
{% endhint %}

### What Can PHP Do ?

* PHP can generate dynamic page content
* PHP can create, open, read, write, delete, and close files on the server
* PHP can collect form data
* PHP can send and receive cookies
* PHP can add, delete, modify data in your database
* PHP can be used to control user-access
* PHP can encrypt data
* With PHP you are not limited to output HTML. You can output images or PDF files. You can also output any text, such as XHTML and XML.

### Why PHP ?&#x20;

* **PHP teaches mechanics**: input → backend → DB → output → session → file.
* **Modern stacks change&#x20;**_**where**_**&#x20;the bugs hide**:
  * Containerization → SSRF, misconfigured cloud roles.
  * Microservices → broken object level auth (BOLA/IDOR).
  * Frameworks → misconfigured middleware, dangerous defaults.
  * Tokens → JWT, OAuth, SSO mishandling.

So yeah: PHP is a good “X-ray” into how backends breathe. Once you see that, translating to Node/Go/Rust/.NET is easier — you just need to adapt to their process models and auth patterns.

### Basics of PHP&#x20;

In PHP, keywords (e.g. `if`, `else`, `while`, `echo`, etc.), classes, functions, and user-defined functions are not case-sensitive except `variable names`.&#x20;

```php
//This is a single line comment. 

// This is also a single-line comment. 

/*This is a 
multi-line comment */
<?php
$y = "Abhishek Pawar";
echo "My name is ".$y."!";                       //Output: My name is Abhishek Pawar!
echo "My name is $y!";                        //Output: same as above
?>
```

### PHP Variable Scope&#x20;

In PHP, variables can be declared anywhere in the script. The scope of a variable is the part of the script where the variable can be referenced/used.

PHP has three different variable scopes:

* local
* global
* static

```php
$x = 5; // global scope

function myTest() {
  $x = 10;
  echo "<p>Variable x inside function is: $x</p>"; // 10
}
myTest();

function globalcheck() {
  global $x;
  echo ",<p>Variable x inside globalcheck function is: $x</p>"; // 5
}
globalcheck();

echo "<p>Variable x outside function is: $x</p>"; // 5
```

PHP also stores all global variables in an array called `$GLOBALS[index]`. The `index` hold the name of the variable. This array is also accessible from within functions and can be used to update global variables directly.&#x20;

```php
$x = 5;
$y = 10;

function myTest() {
  $GLOBALS['y'] = $GLOBALS['x'] + $GLOBALS['y'];
}

myTest();
echo $y; // outputs 15
```

### Data Types in PHP&#x20;

PHP supports the following data types:

* String
* Integer
* Float (floating point numbers - also called double)
* Boolean
* Array
* Object
* NULL
* Resource

### Conditionals in PHP&#x20;

```php
$t = date("H");

if ($t < "10") {
  echo "Have a good morning!";
} elseif ($t < "20") {
  echo "Have a good day!";
} else {
  echo "Have a good night!";
}
```

```php
$favcolor = "red";

switch ($favcolor) {
  case "red":
    echo "Your favorite color is red!";
    break;
  case "blue":
    echo "Your favorite color is blue!";
    break;
  case "green":
    echo "Your favorite color is green!";
    break;
  default:
    echo "Your favorite color is neither red, blue, nor green!";
}
```

### Loops&#x20;

#### While Loop&#x20;

```php
$i = 1;
while ($i < 6) {
  echo $i;
  $i++;
}
```

#### do while

```php
$i = 1;

do {
  echo $i;
  $i++;
} while ($i < 6);
```

#### for loop&#x20;

```php
for ($x = 0; $x <= 10; $x++) {
  echo "The number is: $x <br>";
}
```

#### for each loop&#x20;

```php
$colors = array("red", "green", "blue", "yellow");

foreach ($colors as $x) {
  echo "$x <br>";
}
```

## PHP Superglobals

PHP superglobals are predefined variables that are globally available in all scopes. They are used to handle different types of data, such as input data, server data, session data, and more.

#### 1. $GLOBALS

```php
$x = 75;

function myfunction()
{
    echo $GLOBALS['x'];     //75
}
```

#### 2. $\_SERVER

`$_SERVER` is a PHP super global variable which holds information about headers, paths, and script locations.

The following table lists the most important elements that can go inside `$_SERVER`:

| Element/Code                        | Description                                                                                                                                                                                                             |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $\_SERVER\['PHP\_SELF']             | Returns the filename of the currently executing script                                                                                                                                                                  |
| $\_SERVER\['GATEWAY\_INTERFACE']    | Returns the version of the Common Gateway Interface (CGI) the server is using                                                                                                                                           |
| $\_SERVER\['SERVER\_ADDR']          | Returns the IP address of the host server                                                                                                                                                                               |
| $\_SERVER\['SERVER\_NAME']          | Returns the name of the host server (such as www.w3schools.com)                                                                                                                                                         |
| $\_SERVER\['SERVER\_SOFTWARE']      | Returns the server identification string (such as Apache/2.2.24)                                                                                                                                                        |
| $\_SERVER\['SERVER\_PROTOCOL']      | Returns the name and revision of the information protocol (such as HTTP/1.1)                                                                                                                                            |
| $\_SERVER\['REQUEST\_METHOD']       | Returns the request method used to access the page (such as POST)                                                                                                                                                       |
| $\_SERVER\['REQUEST\_TIME']         | Returns the timestamp of the start of the request (such as 1377687496)                                                                                                                                                  |
| $\_SERVER\['QUERY\_STRING']         | Returns the query string if the page is accessed via a query string                                                                                                                                                     |
| $\_SERVER\['HTTP\_ACCEPT']          | Returns the Accept header from the current request                                                                                                                                                                      |
| $\_SERVER\['HTTP\_ACCEPT\_CHARSET'] | Returns the Accept\_Charset header from the current request (such as utf-8,ISO-8859-1)                                                                                                                                  |
| $\_SERVER\['HTTP\_HOST']            | Returns the Host header from the current request                                                                                                                                                                        |
| $\_SERVER\['HTTP\_REFERER']         | Returns the complete URL of the current page (not reliable because not all user-agents support it)                                                                                                                      |
| $\_SERVER\['HTTPS']                 | Is the script queried through a secure HTTP protocol                                                                                                                                                                    |
| $\_SERVER\['REMOTE\_ADDR']          | Returns the IP address from where the user is viewing the current page                                                                                                                                                  |
| $\_SERVER\['REMOTE\_HOST']          | Returns the Host name from where the user is viewing the current page                                                                                                                                                   |
| $\_SERVER\['REMOTE\_PORT']          | Returns the port being used on the user's machine to communicate with the web server                                                                                                                                    |
| $\_SERVER\['SCRIPT\_FILENAME']      | Returns the absolute pathname of the currently executing script                                                                                                                                                         |
| $\_SERVER\['SERVER\_ADMIN']         | Returns the value given to the SERVER\_ADMIN directive in the web server configuration file (if your script runs on a virtual host, it will be the value defined for that virtual host) (such as someone@w3schools.com) |
| $\_SERVER\['SERVER\_PORT']          | Returns the port on the server machine being used by the web server for communication (such as 80)                                                                                                                      |
| $\_SERVER\['SERVER\_SIGNATURE']     | Returns the server version and virtual host name which are added to server-generated pages                                                                                                                              |
| $\_SERVER\['PATH\_TRANSLATED']      | Returns the file system based path to the current script                                                                                                                                                                |
| $\_SERVER\['SCRIPT\_NAME']          | Returns the path of the current script                                                                                                                                                                                  |
| $\_SERVER\['SCRIPT\_URI']           | Returns the URI of the current page                                                                                                                                                                                     |

### $\_SERVER\['PHP\_SELF'] to XSS

The `$_SERVER['PHP_SELF']` is a global variable that can led to XSS. Let's understand how.&#x20;

Assume we have the following form in a page named "test\_form.php":

```php
<form method="post" action="<?php echo $_SERVER["PHP_SELF"];?>">
```

Now, if a user enters the normal URL in the address bar like "http://www.example.com/test\_form.php", the above code will be translated to:

```html
<form method="post" action="test_form.php">
```

So far, so good.

However, consider that a user enters the following URL in the address bar:

```
http://www.example.com/test_form.php/%22%3E%3Cscript%3Ealert('hacked')%3C/script%3E
```

In this case, the above code will be translated to:

```html
<form method="post" action="test_form.php/"><script>alert('hacked')</script>
```

#### 3. $\_REQUEST

`$_REQUEST` is a PHP super global variable which contains submitted form data, and all cookie data. In other words, `$_REQUEST` is an array containing data from `$_GET`, `$_POST`, and `$_COOKIE`.

{% hint style="info" %}
_`$_REQUEST` is avoided because it merges `$_GET`, `$_POST` and `$_COOKIE` (in the order from `variables_order`), creating ambiguity and allowing an attacker to override expected input; testers therefore send payloads in both GET and POST (and cookies) to see which source the app trusts—if the app reads the value as GET but accepts a malicious POST (or cookie) override, that can enable XSS/SQLi or logic bypasses, so always use explicit `$_GET`/`$_POST` and strict validation._
{% endhint %}

#### 4. $\_GET

```
$name = $_GET['fname'];
echo $name;
```

#### 5. $\_POST

```
$name = $_POST['fname'];
echo $name;
```

#### 6. $\_SESSION

The `$_SESSION` superglobal used to store session variables, which persist across multiple pages for the duration of the user's session. It is used for managing user login states, preferences or temporary data that should persist across multiple page requests. \
\






## PHP Form Handling

The PHP superglobals `$_GET` and `$_POST` are used to collect form-data.

**HTML**

```html
<html>
<body>

<form action="welcome.php" method="POST">
Name: <input type="text" name="name"><br>
E-mail: <input type="text" name="email"><br>
<input type="submit">
</form>

</body>
</html>
```

#### welcome.php for POST

```php
<html>
<body>

Welcome <?php echo $_POST["name"]; ?><br>
Your email address is: <?php echo $_POST["email"]; ?>

</body>
</html>
```

#### welcome.php for GET

```php
<html>
<body>

Welcome <?php echo $_GET["name"]; ?><br>
Your email address is: <?php echo $_GET["email"]; ?>

</body>
</html>
```

**Other best example:** [**https://www.geeksforgeeks.org/php/php-form-processing/**](https://www.geeksforgeeks.org/php/php-form-processing/)

## Demo Lab for understanding the form handling&#x20;

#### **style.css**

```css
body {
  font-family: system-ui, Arial, sans-serif;
  max-width: 700px;
  margin: 30px auto;
  padding: 0 16px;
}

h2 {
  margin-bottom: 10px;
}

form {
  margin-bottom: 20px;
}

label {
  display: block;
  margin: 10px 0 6px;
  font-weight: 600;
}

input {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 6px;
  width: 100%;
  max-width: 300px;
}

.buttons {
  margin-top: 12px;
  display: flex;
  gap: 10px;
}

button {
  padding: 8px 12px;
  border-radius: 6px;
  border: 1px solid #888;
  cursor: pointer;
  background: #f0f0f0;
}

button:hover {
  background: #e6e6e6;
}

.box {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 12px;
  margin: 14px 0;
  background: #fbfbfb;
}

h3 {
  margin-top: 0;
}

.muted {
  color: #666;
  font-size: 0.95rem;
}

```

#### **form.html**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Form Demo</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h2>Simple GET vs POST Form</h2>

  <form action="process.php" method="post" autocomplete="off">
    <label for="first">First name</label>
    <input id="first" name="first" type="text">

    <label for="last">Last name</label>
    <input id="last" name="last" type="text">

    <div class="buttons">
      <button type="submit" formmethod="get">Send (GET)</button>
      <button type="submit" formmethod="post">Send (POST)</button>
      <button type="reset">Reset</button>
    </div>
  </form>
</body>
</html>


```

#### process.php

```php
<?php
// process.php

function e($v) {
    return htmlspecialchars((string)$v, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8');
}
/*
the htmlspecialchars function is key function for escaping HTML special characters.
It converts `<` to `&lt;` to anything that is in (). 

ENT_QUOTES escape both single (') and double ('') quotes. 
ENT_SUBSTITUTE -> If input has invalid UTF-8 bytes, replace then with a replacement 
character instead of breaking the page. 
*/

$method = $_SERVER['REQUEST_METHOD'] ?? 'GET';

if ($method === 'POST') {
    $first = $_POST['first'] ?? '';
    $last  = $_POST['last'] ?? '';
} else {
    $first = $_GET['first'] ?? '';
    $last  = $_GET['last'] ?? '';
}
?>
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Form Result</title>
  <link rel="stylesheet" href="style.css">
  <script src="script.js" defer></script>
</head>
<body>
  <h2>Received values</h2>

  <div class="box" id="php-output">
    <h3>Output from PHP</h3>
    <?php if ($first || $last): ?>
      <p>Hello <?= e($first) ?> <?= e($last) ?>, this message is generated by PHP.</p>
    <?php else: ?>
      <p class="muted">(No input received)</p>
    <?php endif; ?>
  </div>

  <div class="box" id="js-output">
    <h3>Output from JavaScript</h3>
    <p class="muted">(waiting for JS...)</p>
  </div>

  <p><a href="form.html">Back to form</a></p>

  <script>
    // Expose PHP values to JavaScript
    window.formData = {
      first: <?= json_encode($first) ?>,
      last: <?= json_encode($last) ?>
    };
  </script>
</body>
</html>
```

#### script.js

{% code overflow="wrap" %}
```javascript
// script.js

document.addEventListener("DOMContentLoaded", () => {
  const jsBox = document.querySelector("#js-output p");

  if (!window.formData) {
    jsBox.textContent = "(No input data found)";
    return;
  }
/*
The browser will never parse HTML tags inside `textContent`. It sets the content as
plain text, not HTML. If we used jsBox.innerHTML then there are chances that we
could break it and alert a popup. 
*/

  const { first, last } = window.formData;

  if (first || last) {
    jsBox.textContent = `Hello ${first} ${last}, this message is generated by JavaScript.`;
  } else {
    jsBox.textContent = "(No input received)";
  }
});

```
{% endcode %}

### Explanation

#### 1. Form Loads

* Browser requests `form.html`.&#x20;
* HTML + CSS load, you see two input fields (first,last) and buttons.&#x20;

<figure><img src="../../../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

#### 2. If you click GET button

* Browser sends request → `process.php?first=Virat&last=Kohli`.
* PHP backend (`process.php`) reads from `$_GET`.
* It generates HTML response →
  * Box 1: PHP says “Hello Virat Kohli…”
  * Box 2: JS fills “Hello Virat Kohli…” using injected values.

<figure><img src="../../../.gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

#### **3. If you click POST button**

* Browser sends request with body: `first=Virat&last=Kohli`.
* PHP backend (`process.php`) reads from `$_POST`.
* Response is the same: PHP box filled, JS box filled.

<figure><img src="../../../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

So: **form.html (frontend)** → **request (GET or POST)** → **process.php (backend)** → **HTML back to browser** → **JS runs in browser** → shows second box.

### The Actual flow&#x20;

#### Step 1: Form ready in browser

* In `index.html`, the `<input name="first">` and `<input name="last">` fields exist.
* You type:
  * `first` box = `"Virat"`
  * `last` box = `"Kohli"`
* Right now values live only in **DOM properties**:
  * `input[name=first].value === "Virat"`
  * `input[name=last].value === "Kohli"`

***

#### 🔹 Step 2: Click “Submit with GET”

* Browser sees the form’s `action="process.php"` and `method="GET"`.
*   It builds the request:

    ```
    GET /process.php?first=Virat&last=Kohli HTTP/1.1
    Host: localhost
    ```
* **Data flow:** DOM values → packed into query string.

***

#### 🔹 Step 3: PHP receives request

* PHP auto-fills the **superglobal** `$_GET`:
  * `$_GET['first'] = "Virat"`
  * `$_GET['last'] = "Kohli"`
*   Your PHP code does:

    ```php
    $first = $_GET['first'] ?? $_POST['first'] ?? '';
    $last  = $_GET['last']  ?? $_POST['last']  ?? '';
    ```
* So now:
  * `$first = "Virat"`
  * `$last = "Kohli"`

***

#### 🔹 Step 4: PHP builds response

* PHP prints HTML back to the browser.
*   In the “PHP Output” box:

    ```
    Hello Virat Kohli (from PHP via GET)
    ```
*   In the bottom `<script>` tag, PHP _injects_ the same values into JS space:

    ```html
    <script>
      window.formData = { first: "Virat", last: "Kohli" };
    </script>
    ```

***

#### 🔹 Step 5: Browser renders page

* Browser parses HTML → sees the inline `<script>` → executes it.
* Now in JS memory:
  * `window.formData.first === "Virat"`
  * `window.formData.last === "Kohli"`

***

#### 🔹 Step 6: External JS runs

* `script.js` reads `window.formData`:
*   Note that the `script.js` is called above already but with defer keyword which tells browser to render full HTML first and then call the JS.&#x20;

    ```js
    const { first, last } = window.formData;
    document.querySelector('#js-output p').textContent =
      `Hello ${first} ${last} (from JavaScript)`;
    ```
*   DOM updates → “JS Output” box shows:

    ```
    Hello Virat Kohli (from JavaScript)
    ```

***

✅ **Final Path Recap**:\
Input box values → HTTP request → PHP `$_GET/$_POST` → `$first/$last` → injected into HTML → `window.formData` → read by `script.js` → DOM updated.

### Strict Validation for PHP&#x20;

[https://www.w3schools.com/php/php\_form\_url\_email.asp](https://www.w3schools.com/php/php_form_url_email.asp)

## Creating cookies with PHP&#x20;

A cookie is created with the [`setcookie()`](https://www.w3schools.com/php/func_network_setcookie.asp) function.

**Syntax**

```php
setcookie(name, value, expire, path, domain, secure,httponly);
```

{% hint style="info" %}
_Only the name parameter is required. All other parameters are optional._&#x20;
{% endhint %}

To delete a cookie, use the `setcookie()` function with an expiration date.&#x20;

```php
setcookie("user", "", time() - 3600);
// This cookie will delete within one hour
```

## PHP Sessions

* A **session** in PHP is a way to **store data about a user across multiple page requests**.
* HTTP is **stateless** — normally, when a user loads `page1.php` and then `page2.php`, the server has no memory of the first request.
* Sessions allow the server to “remember” information about the user, like login status, preferences, shopping cart, etc.

#### Start a PHP session&#x20;

A session is started with the `session_start()` function. Session variables are set with the PHP global variable: $\_SESSION.

{% hint style="info" %}
_**How does it work? How does it know it's me?**_\
\
_Most sessions set a user-key on the user's computer that looks something like this: 765487cf34ert8dede5a562e4f3a7e12. Then, when a session is opened on another page, it scans the computer for a user-key. If there is a match, it accesses that session, if not, it starts a new session._
{% endhint %}

To remove all global session variables and destroy the session, use `session_unset()` and `session_destroy()`:

An example `session_demo.php`

```php
<?php
// session_demo.php
session_start();

// Handle logout
if(isset($_GET['logout'])){
    session_destroy();
    header("Location: session_demo.php");
    exit;
}

// Handle login
if(isset($_POST['username']) && isset($_POST['password'])){
    // For demo, accept any username/password
    $_SESSION['username'] = $_POST['username'];
}

// If session exists, show welcome
if(isset($_SESSION['username'])){
    echo "Welcome, " . htmlspecialchars($_SESSION['username']) . "!<br>";
    echo '<a href="?logout=1">Logout</a>';
} else {
    // Show login form
    ?>
    <form method="post" action="">
        Username: <input type="text" name="username" required><br>
        Password: <input type="password" name="password" required><br>
        <input type="submit" value="Login">
    </form>
    <?php
}
?>

```

#### **Step 1 — Open the page**

* Browser requests `session_demo.php`.
* **No session exists yet** → PHP does `session_start()`.

**What happens internally:**

1. PHP checks if the browser sent a cookie called `PHPSESSID`.
2. Browser has none → PHP **generates a new session ID** (random string, e.g., `a1b2c3d4e5f6`).
3. PHP creates a session file on the server (something like `/tmp/sess_a1b2c3d4e5f6`) to store `$_SESSION` data.
4. PHP sends **Set-Cookie: PHPSESSID=a1b2c3d4e5f6** to the browser.
5. Browser now remembers that cookie.

Result: You see the login form.

***

#### **Step 2 — Enter username and password, click Login**

*   Browser submits POST request:

    ```
    POST /session_demo.php
    username=Virat
    password=123
    Cookie: PHPSESSID=a1b2c3d4e5f6
    ```
* PHP runs `session_start()`:
  1. Reads `PHPSESSID` from cookie → finds the session file `sess_a1b2c3d4e5f6`.
  2. Loads the saved `$_SESSION` array (empty right now).
*   Code executes:

    ```php
    $_SESSION['username'] = $_POST['username']; // "Virat"
    ```
*   PHP writes this to the session file on the server:

    ```
    sess_a1b2c3d4e5f6 content:
    $_SESSION = ['username' => 'Virat'];
    ```
* Response sent back → browser sees **Welcome, Virat!**

***

#### **Step 3 — Open new tab**

* Browser requests `session_demo.php`.
* Cookie `PHPSESSID=a1b2c3d4e5f6` is sent automatically.
* PHP reads session file `sess_a1b2c3d4e5f6` → loads `$_SESSION['username'] = 'Virat'`.
* Page shows the welcome message **automatically**.

✅ That’s why the session “persists” across tabs — browser is just sending the same session ID cookie.

***

#### **Step 4 — Using another username**

* Suppose you log in as `Rohit`.
* PHP creates a **new session file** with **new PHPSESSID** (e.g., `f6g7h8i9j0k1`).
* `$_SESSION['username'] = 'Rohit'` is stored in that file.
* Browser stores **new cookie** for that tab.

***

#### **Step 5 — Copying PHPSESSID from first user**

* If you manually set your cookie in the browser to the old session ID (`a1b2c3d4e5f6`), the browser now **pretends to be that session**.
* PHP reads the session file `sess_a1b2c3d4e5f6` → loads `$_SESSION['username'] = 'Virat'`.
* That’s why you see the first user’s session.

**Important:** The server doesn’t check “who should have this cookie” — it **trusts the session ID**.

## Set Cookie and Session (Together)

<p align="center"><strong>Best example to set cookies and use that cookies in the session handling and do session hijacking if you can guess the username and the password (last 4 characters).</strong> </p>

{% file src="../../../.gitbook/assets/archive.zip" %}
