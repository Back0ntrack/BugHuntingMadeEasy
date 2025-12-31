# File Upload Attacks

File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like their name, type, contents or size.&#x20;

## Impact of File Upload Vulnerability

1. **Remote Code Execution**&#x20;
2. **Web Shell Deployment**
3. **Stored Cross-Site Scripting**
4. **Malware Hosting and Distribution**
5. **Critical File Overwrite (Uploading file with same name)**

## Prerequisite

### What is MIME Type ?&#x20;

A MIME type is metadata that describes the media type of the data being sent to the server and is conveyed via the `Content-Type` HTTP header; it only indicates how the data should be interpreted and does not guarantee the actual content or its safety.

General format

```
type/subtype
```

Examples:&#x20;

```
text/plain
text/html
image/jpeg: Used to send image
application/pdf: Used to send pdfs
appliciation/x-www-form-urlencoded: Used to send HTML forms
multipart/form-data : Used to safely send binary data/large files
```

It doesn't proof the type of content or the content itself but it is metadata which tells the server treat this data as "specific" content.&#x20;

## Methodology

### 1. Identify All Upload Entry Points

Look beyond obvious “Upload” buttons.

**Common locations**

* Profile photo / avatar
* KYC / document upload
* Support ticket attachments
* Blog / CMS media upload
* Import features (CSV, XML, ZIP)
* API endpoints (`/upload`, `/media`, `/files`)
* Drag-and-drop components

### 2. Understand Client-Side Restrictions

Inspect what the browser enforces (never trust it).

**Check**

* `accept` attribute in `<input type="file">`
* JavaScript validation (extensions, size)
* File preview behavior

🧪 **Test**

* Disable JS
* Use Burp Repeater / curl / Postman
* Send raw multipart requests

### 3. Test File Extension Validation

Try bypassing extension filters.&#x20;

Test for:&#x20;

* Double extension
* Case variations

### 4. Test MIME-Type Validation

Application often trust headers.&#x20;

Manipulate

```
Content-Type: image/jpeg
Content-Type: application/pdf
```

while uploading executable or HTML content.&#x20;

### 5. Test magic Bytes / File Signature Validation

If validation exists, test its depth.&#x20;

Add fake image headers to PHP:&#x20;

```
FF D8 FF E0 (JPEG magic bytes)
<?php system($_GET['cmd']); ?>
```

* Polyglot files (valid image + script)

### 6. Determine File Storage and Access

This step defines impact severity.&#x20;

**Questions**

* Where is the file stored?
  * `/uploads/`
  * `/static/`
  * Cloud bucket?
* Is it **publicly accessible**?
* Is directory listing enabled?
* Can you predict filenames?

### 7. Check execution Context

This is the **RCE decision point**.

**Test**

* Upload executable files:
  * `.php`, `.jsp`, `.aspx`, `.cgi`
* Access via browser
* Observe:
  * Code execution?
  * Download forced?
  * Rendered as text?

➡️ If executed → **Critical RCE**

### 8. Test for Stored XSS via uploads

Even non-executable uploads can be dangerous.

**Try uploading**

* `.html`
* `.svg` (with `<script>`)
* `.pdf` (JavaScript actions)

🧪 Open file as victim/admin user.
