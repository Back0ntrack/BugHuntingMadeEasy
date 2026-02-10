# File Inclusion

## Prerequisite

### Tech-stack function

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### PHP Wrappers

PHP wrappers are built-in code libraries that tell PHP’s stream functions (like fopen(), file\_get\_contents(), etc.) how to handle different protocols, data sources and encodings. It provides a unified way to work with various data streams.

<table data-full-width="true"><thead><tr><th width="158.20001220703125">Wrapper</th><th>Description</th></tr></thead><tbody><tr><td>file://</td><td>Accesses the local file system (this is the default if no wrapper is specified).</td></tr><tr><td>http:// &#x26; https://</td><td>Makes HTTP(s) requests to remote web server.</td></tr><tr><td>ftp://</td><td>Accesses files on an FTP server.</td></tr><tr><td>php://</td><td>Provides access to various internal I/O streams and filters. (e.g., php://input, php://filter) and allows execution.</td></tr><tr><td>data://</td><td>Allows embedding data directly within the URI itself, often using Base64 encoding and executes directly into the memory.</td></tr><tr><td>zip://</td><td>Accesses files within a ZIP archive.</td></tr><tr><td>phar://</td><td>PHP archive files (can be abused if not secured).</td></tr><tr><td>expect://</td><td>Execute system commands.</td></tr></tbody></table>

## What is File Inclusion

File Inclusion is a web application vulnerability that occurs when an application\
dynamically loads or executes files based on user-controlled input without proper\
validation. This vulnerability only allows to read or execute existing files.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Types of File Inclusion

1. Local File Inclusion (High chances)
   * Local File Inclusion (LFI) is a web application vulnerability that occurs when an application includes or reads files from the local server filesystem based on user-controlled input, without proper validation or sanitization.
2. Remote File Inclusion (Less chances)
   * Remote file inclusion (RFI) is a web application vulnerability where an application includes an executes a file hosted on a remote server, fully controlled by the attacker.
   * It may result in immediate remote code execution and requires a specific configuration (e.g., PHP allow\_url\_include).

## Attack Methodology

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
