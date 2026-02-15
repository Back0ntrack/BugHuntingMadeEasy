# HTML (Hyper Text Markup Language)

## References Used

[_Geeks For Geeks_](https://www.geeksforgeeks.org/html/html-tutorial/)

[_Tutorials Point_](https://www.tutorialspoint.com/html/index.htm)

[_<mark style="color:$primary;">Attributes</mark>_](https://www.w3schools.com/tags/ref_attributes.asp)

[_<mark style="color:$primary;">HTML Tags A-Z</mark>_](https://www.w3schools.com/tags/default.asp)

***

The first and most dominant component of the front end of web applications is HTML (Hyper Text Markup Language). It contains each page's basic elements, including titles, forms, images, and many other elements.

## HTML Page Structure (Basic)

{% hint style="info" %}
**Note: HTML tags are not case sensitive: `<P>` means the same as `<p>`.**
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

All HTML documents must start with a document type declaration: `<!DOCTYPE html>` which tells that it is an HTML 5 document.&#x20;

To view the HTML source code click `Ctrl + U` or right-click on the page and select `"View Page Source"`.&#x20;

### Empty Tags

HTML elements without any content i.e., that do not print anything are called Empty elements. Empty HTML elements do not have an ending tag. E.g., `<br>`, `<hr>`, `<link>`, `<input>`.&#x20;

{% hint style="info" %}
Note that in XHTML even the empty tags are closed using `<br/>`.&#x20;
{% endhint %}

## Block-Level V/s Inline elements

### Block-Level Elements

1. **Block-Level Elements -** Block-level elements typically start on a new line and take up the full width available to them, regardless of their actual content width.
   1. `<div>`: A general purpose container for other elements.&#x20;
   2. `<p>`: Defines a paragraph.
   3. `<h1>, <h2>,...,<h6>`: Heading elements of different levels.&#x20;
   4. `<ol>, <ul>`: Ordered and unordered lists.&#x20;
   5. `<table>`: Defines a table.&#x20;
   6. `<form>`: used for HTML forms to collect user inputs.&#x20;
   7. `<section>, <article>, <nav>, <aside>, <header>, <footer>`: Semantic elements that define areas of a webpage.&#x20;

### The \<div> tag

The `<div>` element is used as a container for other HTML elements.&#x20;

```html
<div id="menu"><!-- all code required for menu bar --></div>
<div id="background"><!-- all code required for background change --></div>
```

`div` tag is very useful with CSS to apply formatting on a specific group/category.&#x20;

### Inline-level elements

2. **Inline Elements -** Inline elements do not start on a new line; they appear on the same line as adjacent content, as long as there is space. Inline elements are typically used within block-level elements to add content or style.&#x20;
   1.  `<span>`: A general-purpose inline container for phrasing content. <br>

       ```html
       <p>This is a <span style="color:blue;">blue</span> text within a paragraph. </p>
       ```
   2. `<a>`: Creates hyperlink.&#x20;
   3. `<img>`: Embeds an image.&#x20;
   4. `<strong> , <b>`: used for strong emphasis and bold text, respectively.
   5. `<em>, <i>`: used for emphasis and italic text, respectively.&#x20;
   6. `<br>`: Inserts a line break within text.&#x20;
   7. `<input>`: Creates interactive controls for form.&#x20;

### Absolute URLs V/s Relative URLs

```html
<h2>Absolute URLs</h2>
<p><a href="https://www.w3.org/">W3C</a></p>
<p><a href="https://www.google.com/">Google</a></p>

<h2>Relative URLs</h2>
<p><a href="html_images.asp">HTML Images</a></p>
<p><a href="/css/default.asp">CSS Tutorial</a></p>
```

> **While using relative URLs the browser resolves the link relative to the path of the current HTML file. All below case assumes that our current HTML page is index.html.**

**Example 1: Same folder**

```html
<a href="resume.html">My resume</a>
```

```
/project
 ├── index.html
 └── resume.html
```

**Example 2: Resume inside a subfolder**

```html
<a href="pages/resume.html">My resume</a>
```

```
/project
 ├── index.html
 └── pages/
      └── resume.html
```

**Example 3: Resume one folder rup**

```html
<a href="../resume.html">My resume</a>
```

```
/project
 ├── pages/
 │    └── index.html
 └── resume.html
```

## Attributes

* All HTML elements can have attributes
* Attributes provide additional information about elements
* Attributes are always specified in the start tag
* Attributes usually come in name/value pairs like: name="value"

### the href attribute

```html
<a href="https://www.w3schools.com">Visit W3Schools</a>
```

In the `href` attribute we can include absolute and relative URL.&#x20;

### Image as a link

```html
<a href="default.html">
<img src="smiley.gif" alt="HTML tutorial" style="width:42px;height:42px;">
</a> 
<!-- On clicking smiley would navigate back to default.html -->
```

### the src attribute

```html
<img src="img_girl.jpg" width="500" height="600" alt="Girl with a jacker">
```

In the `src` attribute too we can include absolute and relative URL.&#x20;

### the style attribute

Using the `style` attribute in HTML is the same as using CSS, but it is applied inline to that specific element instead of in a separate CSS file.

{% code overflow="wrap" %}
```html
<p style="color:red; font-family:'Courier New';" title='red paragraph'>This is a red paragraph.</p>
```
{% endcode %}

Note all the elements(tags) possesses attributes.&#x20;

## HTML Formatting Elements

* `<b>` - Bold text
* `<strong>` - Important text. (Same as `<b>` but treated important from browser)
* `<i>` - Italic text
* `<em>` - Emphasized text (Same as `<i>` but treated important from browser)
* `<mark>` - Marked text (highlighted text).
* `<small>` - Smaller text
* `<del>` - Deleted text (Strikethrough text).
* `<ins>` - Inserted text (strikethrough deleted and insert underlined).
  * ```html
    <p>My favorite color is <del>blue</del> <ins>red</ins>.</p>
    ```
* `<sub>` - Subscript text
* `<sup>` - Superscript text

## HTML Tags (Imp only)

### HTML Images

```html
<img src = "smiley.gif" alt="Smiley face" style="float:right;width:42px;height:42px;">
```

**Common Image Formats**

| Abbreviation | File Format                           | File Extension                             |
| ------------ | ------------------------------------- | ------------------------------------------ |
| APNG         | Animated Portable Network Graphics    | `.apng`                                    |
| GIF          | Graphics Interchange Format           | `.gif`                                     |
| ICO          | Microsoft Icon                        | `.ico`, `.cur`                             |
| JPEG         | Joint Photographic Expert Group Image | `.jpg`, `.jpeg`, `.jfif`, `.pjpej`, `.pjp` |
| PNG          | Portable Network Graphics             | `.png`                                     |
| SVG          | Scalable Vector Graphics              | `.svg`                                     |

The HTML `<map>` tag defines an image map which is an image with clickable areas. We can set that when someone clicks at this area of the image then do this.&#x20;

```html
<img src="workplace.jpg" alt="Workplace" usemap="#workmap">

<map name="workmap">
  <area shape="rect" coords="34,44,270,350" alt="Computer" href="computer.htm">
  <area shape="rect" coords="290,172,333,250" alt="Phone" href="phone.htm">
  <area shape="circle" coords="337,300,44" alt="Coffee" href="coffee.htm">
</map>
```

### HTML Tables

**A simple table**

```html
<table> <!-- used to create a table -->
  <tr>  <!-- defines row of the table -->
    <th>Company</th> <!-- defines table heading -->
    <th>Contact</th>
    <th>Country</th>
  </tr>
  <tr> <!-- another row -->
    <td>Alfreds Futterkiste</td> <!-- data -->
    <td>Maria Anders</td>
    <td>Germany</td>
  </tr>
  <tr>
    <td>Centro comercial Moctezuma</td>
    <td>Francisco Chang</td>
    <td>Mexico</td>
  </tr>
</table>
```

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### HTML Lists

```html
<!DOCTYPE html>
<html>
<body>
<h2>An Unordered HTML List</h2>
<ul>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ul>  
<h2>An Ordered HTML List</h2>
<ol>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ol> 
</body>
</html>
```

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```html
<!DOCTYPE html>
<html>
<body>

<h2>A Description List</h2>

<dl>
  <dt>Coffee</dt>
  <dd>- black hot drink</dd>
  <dt>Milk</dt>
  <dd>- white cold drink</dd>
</dl>

</body>
</html>
```

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### HTML Iframes

An HTML iframe is used to display a web page within a web page.

```html
<iframe src="url" height="200" width="300" title="description"></iframe>
```

### HTML Head Element

The HTML `<head>` element is a container for the following elements: `<title>`, `<style>`, `<meta>`, `<link>`, `<script>`, and `<base>`. HTML metadata is data about the HTML document. Metadata is not displayed on the page.

\
&#xNAN;**`<link>` element:** used to link to external style sheets.&#x20;

```html
<link rel="stylesheet" href="mystyle.css">
```

**`<meta>`** tag: The `<meta>` element is typically used to specify the character set, page description, keywords, author of the document, and viewport settings.

### HTML Script Element

The `<script>` element is used to define client-side JavaScript's.&#x20;

> Note that for both CSS and JS we can embed it into HTML documents using `<style>` and `<script>` tag respectively else we can use separate file `.css` and `.js` and link it to the HTML file using `<link>` tag in head in HTML.&#x20;

### HTML Layout Elements

HTML has several semantic elements that define the different parts of a web page:

* `<header>` - Defines a header for a document or a section
* `<nav>` - Defines a set of navigation links
* `<section>` - Defines a section in a document
* `<article>` - Defines independent, self-contained content
* `<aside>` - Defines content aside from the content (like a sidebar)
* `<footer>` - Defines a footer for a document or a section
* `<details>` - Defines additional details that the user can open and close on demand
* `<summary>` - Defines a heading for the `<details>` element

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Difference between class and id

{% code overflow="wrap" %}
```html
<!DOCTYPE html>
<html>
<head>
<style>
/* Style the element with the id "myHeader" */
#myHeader {
  background-color: lightblue;
  color: black;
  padding: 40px;
  text-align: center;
}

/* Style all elements with the class name "city" */
.city {
  background-color: tomato;
  color: white;
  padding: 10px;
} 
</style>
</head>
<body>

<h2>Difference Between Class and ID</h2>
<p>A class name can be used by multiple HTML elements, while an id name must only be used by one HTML element within the page:</p>

<!-- An element with a unique id -->
<h1 id="myHeader">My Cities</h1>

<!-- Multiple elements with same class -->
<h2 class="city">London</h2>
<p>London is the capital of England.</p>

<!-- <h3 class="city">Tokyo</h3> --> 
<!-- Multiple elements with same class is possible but id must have only 1 element -->
</body>
</html>
```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## HTML Forms

### HTML Form Elements

The HTML `<form>` element can contain one or more of the following form elements:

* `<input>`
* `<label>`
* `<select>`
* `<textarea>`
* `<button>`
* `<fieldset>`
* `<legend>`
* `<datalist>`
* `<output>`
* `<option>`
* `<optgroup>`

{% code overflow="wrap" %}
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>HTML Form Elements Demo</title>
</head>
<body>
  <h2>Form Elements Demo</h2>

  <form action="#" method="post">

    <!-- Label + Input -->
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" placeholder="Enter your username">
    <br><br>

    <!-- Password Input -->
    <label for="password">Password:</label>
    <input type="password" id="password" name="password">
    <br><br>

    <!-- Select with Option + Optgroup -->
    <label for="country">Choose Country:</label>
    <select id="country" name="country">
      <optgroup label="Asia">
        <option value="india">India</option>
        <option value="japan">Japan</option>
      </optgroup>
      <optgroup label="Europe">
        <option value="germany">Germany</option>
        <option value="france">France</option>
      </optgroup>
    </select>
    <br><br>

    <!-- Textarea -->
    <label for="message">Message:</label><br>
    <textarea id="message" name="message" rows="4" cols="40">Type something...</textarea>
    <br><br>

    <!-- Fieldset + Legend -->
    <fieldset>
      <legend>Choose your skills</legend>
      <input type="checkbox" id="html" name="skills" value="HTML">
      <label for="html">HTML</label>
      <input type="checkbox" id="css" name="skills" value="CSS">
      <label for="css">CSS</label>
      <input type="checkbox" id="js" name="skills" value="JS">
      <label for="js">JavaScript</label>
    </fieldset>
    <br>

    <!-- Datalist with Input -->
    <label for="browser">Favorite Browser:</label>
    <input list="browsers" id="browser" name="browser">
    <datalist id="browsers">
      <option value="Chrome">
      <option value="Firefox">
      <option value="Edge">
      <option value="Safari">
    </datalist>
    <br><br>

    <!-- Output element -->
    <label for="num1">Add Numbers:</label>
    <input type="number" id="num1" name="num1" oninput="result.value=parseInt(num1.value)||0 + parseInt(num2.value)||0">
    +
    <input type="number" id="num2" name="num2" oninput="result.value=parseInt(num1.value)||0 + parseInt(num2.value)||0">
    =
    <output name="result" for="num1 num2">0</output>
    <br><br>

    <!-- Buttons -->
    <button type="submit">Submit</button>
    <button type="reset">Reset</button>
    <button type="button" onclick="alert('Hello!')">Click Me</button>

  </form>
</body>
</html>

```
{% endcode %}

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### HTML Form Attributes

1. `action`: It defines the action to be performed when the form is submitted. Usually, the form data is sent to a file on the server when the user clicks on the submit button.
2. `method`: The `method` attribute specifies the HTTP method to be used when submitting the form data.&#x20;
3. `enctype`: Specifies how the form-data should be encoded when submitting it to the server (only for method="post").

**Attribute values for `enctype`**

| Attribute Values                    | Description                                                                                                                                     |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `application/x-www-form-urlencoded` | Default. All characters are encoded before sent (spaces are converted to "+" symbols, and special characters are converted to ASCII HEX values) |
| `multipart/form-data`               | This value is necessary if the user will upload a file through the form.                                                                        |
| `text/plain`                        | Sends data without any encoding at all. Not recommended.                                                                                        |

4. `name`: specifies the name of the form.&#x20;

{% code overflow="wrap" %}
```html
<!DOCTYPE html>
<html>
<head>
<!-- Name is used to submit a specific input details -->
<script>
function formSubmit() {
    document.forms["myForm"].submit();
}
</script>
</head>
<body>

<h1>The form name attribute</h1>

<form name="myForm" action="/action_page.php" method="get">
  <label for="fname">First name:</label>
  <input type="text" id="fname" name="fname"><br><br>
  <label for="lname">Last name:</label>
  <input type="text" id="lname" name="lname"><br><br>
  <input type="button" onclick="formSubmit()" value="Send form data!">
</form>

<p>Notice that the JavaScript in the head section uses the name of the form to specify which form to submit.</p>

</body>
</html>

```
{% endcode %}

> When a form is submitted, all form fields (visible, hidden, or readonly) with a `name` attribute are sent to the page in the `action` attribute, except `disabled` fields which are not submitted.

### File upload in html

```html
<form action="upload.php" method="post" enctype="multipart/form-data">
  <input type="file" name="myfile">
  <button type="submit">Upload</button>
</form>

<!-- upload multiple file -->
<form action="upload.php" method="post" enctype="multipart/form-data">
  <input type="file" name="multifile" multiple>
  <button type="submit">Upload</button>
</form>
```

### File Upload Input Restrictions

```html
<form action="/action_page.php">
  <label for="img">Select image:</label>
  <input type="file" id="img" name="img" accept="image/*">
  <input type="submit">
</form>
```

<table><thead><tr><th width="138.79998779296875">Value</th><th>Description</th></tr></thead><tbody><tr><td><em>file_extension</em></td><td>Specify the file extension(s) (e.g: .gif, .jpg, .png, .doc) the user can pick from</td></tr><tr><td>audio/*</td><td>The user can pick all sound files</td></tr><tr><td>video/*</td><td>The user can pick all video files</td></tr><tr><td>image/*</td><td>The user can pick all image files</td></tr><tr><td><em>media_type</em></td><td>A valid media type, with no parameters. Look at <a href="http://www.iana.org/assignments/media-types/">IANA Media Types</a> for a complete list of standard media types</td></tr></tbody></table>

But again this is client side restrictions that during the upload we're subject to check the file type. the user can change it by intercepting the request.&#x20;

### Input Restrictions

| Attribute | Description                                                                                                    |
| --------- | -------------------------------------------------------------------------------------------------------------- |
| checked   | Specifies that an input field should be pre-selected when the page loads (for type="checkbox" or type="radio") |
| disabled  | Specifies that an input field should be disabled                                                               |
| max       | Specifies the maximum value for an input field                                                                 |
| maxlength | Specifies the maximum number of character for an input field                                                   |
| min       | Specifies the minimum value for an input field                                                                 |
| pattern   | Specifies a regular expression to check the input value against                                                |
| readonly  | Specifies that an input field is read only (cannot be changed)                                                 |
| required  | Specifies that an input field is required (must be filled out)                                                 |
| size      | Specifies the width (in characters) of an input field                                                          |
| step      | Specifies the legal number intervals for an input field                                                        |
| value     | Specifies the default value for an input field                                                                 |

Input restrictions like `required`, `maxlength`, `min`, `max`, `pattern`, `readonly`, and `disabled` are enforced only by the browser; attackers can bypass them using developer tools or intercepting requests.&#x20;

## HTML URLs

URL Encoding is the process of converting the URL into a valid format that is accepted by web browsers.

### URL Syntax

A web address follows these syntax rules:

```
scheme://prefix.domain:port/path/filename

//Example
https://www.geeksforgeeks.org/
```

* **Scheme:** It specifies the protocol used for communication, such as "https://" for secure communication or "http://" for unsecured communication.
* **Prefix:** It is an optional subdomain or www indicating the location of the resource within the domain.
* **Domain:** Identifies the website's primary address, like "example.com", indicating its unique location on the Internet.
* **Port:** Optional and signifies a specific endpoint for communication. Common values are 80 for HTTP and 443 for HTTPS.
* **Path:** It **s**pecifies the location or directory on the server where the resource is located.
* **Filename**: It refers to the specific file or resource within the specified path.

**URL Encoding Reference:** [**https://www.w3schools.com/tags/ref\_urlencode.asp**](https://www.w3schools.com/tags/ref_urlencode.asp)

## HTML Web Storage

With web storage, applications can store data locally within the user's browser. Web storage is per origin (per domain and protocol). All pages, from one origin, can store and access the same data. Unlike cookies, the storage limit is far larger (at least 5MB) and information is never transferred to the server.

### Web Storage API Objects

Web storage provides two objects for storing data in the browser:

* `window.localStorage` - stores data with no expiration date (data is not lost when the browser tab is closed)
* `window.sessionStorage` - stores data for one session (data is lost when the browser tab is closed)

***

## HTML Summary

### 1. Nature of HTML

HTML (HyperText Markup Language) is purely a **frontend presentation language**. It defines how content is structured and displayed in the browser — not how it is processed or secured. Everything written in HTML is **visible, editable, and controllable by the client (the user/attacker)**. Since HTML is static by nature, it **cannot provide any strong security guarantees**. Any restrictions implemented in HTML are only hints for the browser’s behavior and can be bypassed easily using developer tools, request interception (Burp Suite/ZAP), or custom scripts (curl, Python, etc.).

### 2. Form Submission & Input Handling

Forms are the most interactive part of HTML, allowing data submission to the server. A form sends all fields with a `name` attribute (including hidden ones and readonly inputs, but not disabled inputs) to the page defined in the `action` attribute, using the method defined in `method` (`GET` or `POST`) and encoded as specified in `enctype` (`application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`).

🔐 From a hacker’s view, **all client-side input restrictions** such as `required`, `maxlength`, `min`, `max`, `pattern`, `readonly`, and `disabled` are **browser-enforced only**. They can be removed or modified before submission. Attackers can change hidden fields, modify readonly fields, and even inject parameters that were never present in the form by intercepting and editing the request.

### 3. Input Restrictions & Validation

HTML provides basic restrictions like input type (`email`, `number`, `password`), patterns, or the `accept` attribute in file uploads. These are only **cosmetic filters** for the user’s browser experience. They are **not security controls** because an attacker can bypass them by sending raw requests. This is why server-side validation is always mandatory.

### 4. File Uploads

HTML supports file uploads via `<input type="file">` inside a form with `enctype="multipart/form-data"`. While this enables users to upload files, it is a **high-risk area** for security. Attackers exploit it by:

* Uploading malicious files (like `.php`, `.jsp`, `.aspx`) to gain Remote Code Execution.
* Bypassing client-side filters (`accept` or file extension checks).
* Using double extensions (`shell.php.jpg`).
* Uploading large files to cause Denial of Service (DoS).

Thus, HTML only provides the upload interface — security must be enforced on the server.

### 5. The Bottom Line for Hackers

HTML is **not a security layer**. It is only a **presentation and data submission layer**. Any validation, restriction, or filtering provided by HTML is **cosmetic** and can be bypassed with basic tools. For attackers, this means HTML is a rich source of **injection points** but never a barrier. The real security lies in the **server-side logic**, not the frontend.

{% hint style="info" %}
_From a hacker’s perspective, HTML is important for identifying input points, forms, file uploads, meta directives, links, and frames, but the real security testing always targets how the server handles this data._
{% endhint %}

***

## What a tester should check in the HTML

### 1. **Form Structure & Input Fields**

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

### 2. **File Uploads**

* `<input type="file">`
  * Check if multiple allowed (`multiple` attribute).
  * Any extension restrictions in `accept=".jpg,.png"` (client-side only, bypassable).
  * If no restrictions → risk of RCE via malicious upload.

### 3. **Authentication/Authorization Clues**

* Login forms → look for:
  * `username` / `password` fields.
  * Hidden roles (`role=admin`).
  * Tokens (`auth=xyz123`).
* Signup forms → may leak role assignment fields.
* Password fields → look for missing `autocomplete="off"`.

### 4. **Client-Side Scripts (JS in HTML)**

* Inline `<script>` or linked `.js` files.
  * Look for hardcoded secrets (API keys, JWTs, credentials).
  * Look for API endpoints or URLs.
  * Look for client-side validation logic (regex, length checks) → backend may not enforce.

### 5. **URLs & Endpoints**

* `<form action="...">`
  * Reveals backend endpoints (`/login`, `/upload`, `/admin/deleteUser`).
  * Method (`GET` vs `POST`) — GET exposing sensitive data is a bug.
* `<a href>` links to hidden functionality (`/backup`, `/admin`, `/test.php`).
* API endpoints hidden in JS variables.

### 6. **Comments in Code**

* Devs leave sensitive info in HTML comments.
  * E.g., `<!-- TODO: remove debug.php -->`
  * API keys, passwords, SQL queries sometimes left.

***

Hacker’s Strategy with given `index.html`:

1. Enumerate **all input fields** (visible/hidden/readonly/disabled).
2. Note **all form actions & methods** (endpoints, GET/POST).
3. Check **client-side restrictions** → mark them bypassable.
4. Inspect **comments, scripts, meta tags** → gather secrets.
5. Mark potential **attack vectors**: SQLi, XSS, LFI, CSRF, file upload, IDOR, etc.
